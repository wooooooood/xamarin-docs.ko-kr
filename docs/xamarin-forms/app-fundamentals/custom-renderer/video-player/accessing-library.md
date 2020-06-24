---
title: 디바이스의 비디오 라이브러리에 액세스
description: 이 문서에서는 Xamarin.Forms를 사용하여 비디오 플레이어 애플리케이션에서 디바이스의 비디오 라이브러리에 액세스하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 364C1D43-EAAE-45B9-BE24-0DA5AE74C4D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 00f1434a55fb815710bff26ac090a90bce5f41ee
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84197530"
---
# <a name="accessing-the-devices-video-library"></a>디바이스의 비디오 라이브러리에 액세스

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

대부분의 최신 모바일 디바이스 및 데스크톱 컴퓨터에는 디바이스의 카메라를 사용하여 비디오를 녹화할 수 있는 기능이 있습니다. 사용자가 생성한 비디오는 디바이스에 파일로 저장됩니다. 이러한 파일을 이미지 라이브러리에서 가져와서 다른 비디오처럼 `VideoPlayer` 클래스로 재생할 수 있습니다.

## <a name="the-photo-picker-dependency-service"></a>사진 선택기 종속성 서비스

각 플랫폼에는 사용자가 디바이스의 이미지 라이브러리에서 사진이나 비디오를 선택할 수 있는 기능이 있습니다. 디바이스의 이미지 라이브러리에서 비디오를 재생하는 첫 번째 단계는 각 플랫폼에서 이미지 선택기를 호출하는 종속성 서비스를 작성하는 것입니다. 아래에 설명된 종속성 서비스는 [**사진 라이브러리에서 사진 선택**](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md) 문서에 정의된 내용과 매우 유사합니다. 단, 비디오 선택기가 `Stream` 개체를 반환하지 않고 파일 이름을 반환한다는 점이 다릅니다.

.NET Standard 라이브러리 프로젝트는 종속성 서비스에 대해 `IVideoPicker`라는 인터페이스를 정의합니다.

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPicker
    {
        Task<string> GetVideoFileAsync();
    }
}
```

각 플랫폼에는 이 인터페이스를 구현하는 `VideoPicker`라는 클래스가 있습니다.

### <a name="the-ios-video-picker"></a>iOS 비디오 선택기

iOS `VideoPicker`는 iOS [`UIImagePickerController`](xref:UIKit.UIImagePickerController)를 사용하여 이미지 라이브러리에 액세스하고 iOS `MediaType` 속성에서 비디오("영화"라고 함)로 제한되도록 지정합니다. `VideoPicker`는 `IVideoPicker` 인터페이스를 명시적으로 구현합니다. `Dependency` 특성은 이 클래스를 종속성 서비스로 식별합니다. 이 항목이 Xamarin.Forms가 플랫폼 프로젝트에서 종속성 서비스를 찾을 수 있도록 하는 두 가지 요구 사항입니다.

```csharp
using System;
using System.Threading.Tasks;
using UIKit;
using Xamarin.Forms;

[assembly: Dependency(typeof(FormsVideoLibrary.iOS.VideoPicker))]

namespace FormsVideoLibrary.iOS
{
    public class VideoPicker : IVideoPicker
    {
        TaskCompletionSource<string> taskCompletionSource;
        UIImagePickerController imagePicker;

        public Task<string> GetVideoFileAsync()
        {
            // Create and define UIImagePickerController
            imagePicker = new UIImagePickerController
            {
                SourceType = UIImagePickerControllerSourceType.SavedPhotosAlbum,
                MediaTypes = new string[] { "public.movie" }
            };

            // Set event handlers
            imagePicker.FinishedPickingMedia += OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled += OnImagePickerCancelled;

            // Present UIImagePickerController;
            UIWindow window = UIApplication.SharedApplication.KeyWindow;
            var viewController = window.RootViewController;
            viewController.PresentViewController(imagePicker, true, null);

            // Return Task object
            taskCompletionSource = new TaskCompletionSource<string>();
            return taskCompletionSource.Task;
        }

        void OnImagePickerFinishedPickingMedia(object sender, UIImagePickerMediaPickedEventArgs args)
        {
            if (args.MediaType == "public.movie")
            {
                taskCompletionSource.SetResult(args.MediaUrl.AbsoluteString);
            }
            else
            {
                taskCompletionSource.SetResult(null);
            }
            imagePicker.DismissModalViewController(true);
        }

        void OnImagePickerCancelled(object sender, EventArgs args)
        {
            taskCompletionSource.SetResult(null);
            imagePicker.DismissModalViewController(true);
        }
    }
}
```

### <a name="the-android-video-picker"></a>Android 비디오 선택기

`IVideoPicker`를 Android에 구현하려면 애플리케이션 활동의 일부인 콜백 메서드가 필요합니다. 따라서 `MainActivity` 클래스는 필드와 콜백 메서드라는 두 가지 속성을 정의합니다.

```csharp
namespace VideoPlayerDemos.Droid
{
    ···
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            Current = this;
            ···
        }

        // Field, properties, and method for Video Picker
        public static MainActivity Current { private set; get; }

        public static readonly int PickImageId = 1000;

        public TaskCompletionSource<string> PickImageTaskCompletionSource { set; get; }

        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);

            if (requestCode == PickImageId)
            {
                if ((resultCode == Result.Ok) && (data != null))
                {
                    // Set the filename as the completion of the Task
                    PickImageTaskCompletionSource.SetResult(data.DataString);
                }
                else
                {
                    PickImageTaskCompletionSource.SetResult(null);
                }
            }
        }
    }
}
```

`MainActivity`의 `OnCreate` 메서드는 정적 `Current` 속성에 자체 인스턴스를 저장합니다. 이를 통해 `IVideoPicker`를 구현하면 **Select Video**(비디오 선택) 선택기를 시작할 수 있는 `MainActivity` 인스턴스를 얻을 수 있습니다.

```csharp
using System;
using System.Threading.Tasks;
using Android.Content;
using Xamarin.Forms;

// Need application's MainActivity
using VideoPlayerDemos.Droid;

[assembly: Dependency(typeof(FormsVideoLibrary.Droid.VideoPicker))]

namespace FormsVideoLibrary.Droid
{
    public class VideoPicker : IVideoPicker
    {
        public Task<string> GetVideoFileAsync()
        {
            // Define the Intent for getting images
            Intent intent = new Intent();
            intent.SetType("video/*");
            intent.SetAction(Intent.ActionGetContent);

            // Get the MainActivity instance
            MainActivity activity = MainActivity.Current;

            // Start the picture-picker activity (resumes in MainActivity.cs)
            activity.StartActivityForResult(
                Intent.CreateChooser(intent, "Select Video"),
                MainActivity.PickImageId);

            // Save the TaskCompletionSource object as a MainActivity property
            activity.PickImageTaskCompletionSource = new TaskCompletionSource<string>();

            // Return Task object
            return activity.PickImageTaskCompletionSource.Task;
        }
    }
}
```

`MainActivity` 개체에 대한 추가 사항은 [**VideoPlayerDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos) 솔루션의 유일한 코드이며 `FormsVideoLibrary` 클래스를 지원하도록 일반 애플리케이션 코드를 변경해야 합니다.

### <a name="the-uwp-video-picker"></a>UWP 비디오 선택기

`IVideoPicker` 인터페이스를 UWP에 구현하면 UWP [`FileOpenPicker`](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/)가 사용됩니다. 사진 라이브러리에서 파일 검색을 시작하고 파일 형식을 MP4 및 WMV(Windows Media Video)로 제한합니다.

```csharp
using System;
using System.Threading.Tasks;
using Windows.Storage;
using Windows.Storage.Pickers;
using Xamarin.Forms;

[assembly: Dependency(typeof(FormsVideoLibrary.UWP.VideoPicker))]

namespace FormsVideoLibrary.UWP
{
    public class VideoPicker : IVideoPicker
    {
        public async Task<string> GetVideoFileAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary
            };

            openPicker.FileTypeFilter.Add(".wmv");
            openPicker.FileTypeFilter.Add(".mp4");

            // Get a file and return the path
            StorageFile storageFile = await openPicker.PickSingleFileAsync();
            return storageFile?.Path;
        }
    }
}
```

## <a name="invoking-the-dependency-service"></a>종속성 서비스 호출

[**VideoPlayerDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos) 프로그램의 **Play Library Video**(라이브러리 비디오 재생) 페이지는 비디오 선택기 종속성 서비스를 사용하는 방법을 보여줍니다. XAML 파일에는 `VideoPlayer` 인스턴스와 **Show Video Library**(비디오 라이브러리 표시) 레이블이 지정된 `Button`이 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayLibraryVideoPage"
             Title="Play Library Video">
    <StackLayout>
        <video:VideoPlayer x:Name="videoPlayer"
                           VerticalOptions="FillAndExpand" />

        <Button Text="Show Video Library"
                Margin="10"
                HorizontalOptions="Center"
                Clicked="OnShowVideoLibraryClicked" />
    </StackLayout>
</ContentPage>
```

코드 숨김 파일에는 `Button`의 `Clicked` 처리기가 포함됩니다. 종속성 서비스를 호출하려면 `DependencyService.Get`을 호출하여 플랫폼 프로젝트에서 `IVideoPicker` 인터페이스 구현을 확보해야 합니다. 그러면 해당 인스턴스에서 `GetVideoFileAsync` 메서드가 호출됩니다.

```csharp
namespace VideoPlayerDemos
{
    public partial class PlayLibraryVideoPage : ContentPage
    {
        public PlayLibraryVideoPage()
        {
            InitializeComponent();
        }

       async void OnShowVideoLibraryClicked(object sender, EventArgs args)
        {
            Button btn = (Button)sender;
            btn.IsEnabled = false;

            string filename = await DependencyService.Get<IVideoPicker>().GetVideoFileAsync();

            if (!String.IsNullOrWhiteSpace(filename))
            {
                videoPlayer.Source = new FileVideoSource
                {
                    File = filename
                };
            }

            btn.IsEnabled = true;
        }
    }
}
```

그런 다음, `Clicked` 처리기가 해당 파일 이름을 사용하여 `FileVideoSource` 개체를 만들어서 `VideoPlayer`의 `Source` 속성으로 설정합니다.

`VideoPlayerRenderer` 클래스마다 `FileVideoSource` 유형의 개체에 대한 `SetSource` 메서드의 코드가 포함됩니다. 이 내용은 아래에 표시됩니다.

### <a name="handling-ios-files"></a>iOS 파일 처리

`VideoPlayerRenderer`의 iOS 버전은 파일 이름과 함께 정적 `Asset.FromUrl` 메서드를 사용하여 `FileVideoSource` 개체를 처리합니다. 그러면 디바이스의 이미지 라이브러리에 있는 파일을 나타내는 `AVAsset` 개체가 생성됩니다.

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void SetSource()
        {
            AVAsset asset = null;
            ···
            else if (Element.Source is FileVideoSource)
            {
                string uri = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    asset = AVAsset.FromUrl(new NSUrl(uri));
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="handling-android-files"></a>Android 파일 처리

`FileVideoSource` 유형의 개체를 처리할 때, Android에서 `VideoPlayerRenderer`를 구현하면 `VideoView`의 `SetVideoPath` 메서드를 사용하여 디바이스의 이미지 라이브러리에 있는 파일이 지정됩니다.

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void SetSource()
        {
            isPrepared = false;
            bool hasSetSource = false;
            ···
            else if (Element.Source is FileVideoSource)
            {
                string filename = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(filename))
                {
                    videoView.SetVideoPath(filename);
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="handling-uwp-files"></a>UWP 파일 처리

`FileVideoSource` 유형의 개체를 처리하는 경우 UWP에서 `SetSource` 메서드를 구현하려면 `StorageFile` 개체를 만들고, 해당 파일을 열어서 읽고, 스트림 개체를 `MediaElement`의 `SetSource` 메서드로 전달해야 합니다.

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
        async void SetSource()
        {
            bool hasSetSource = false;
            ···
            else if (Element.Source is FileVideoSource)
            {
                // Code requires Pictures Library in Package.appxmanifest Capabilities to be enabled
                string filename = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(filename))
                {
                    StorageFile storageFile = await StorageFile.GetFileFromPathAsync(filename);
                    IRandomAccessStreamWithContentType stream = await storageFile.OpenReadAsync();
                    Control.SetSource(stream, storageFile.ContentType);
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

각 플랫폼마다 파일이 디바이스에 있어서 다운로드할 필요가 없기 때문에 비디오 소스가 설정된 직후에 비디오 재생이 시작됩니다.

## <a name="related-links"></a>관련 링크

- [비디오 플레이어 데모(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)
- [사진 라이브러리에서 사진 선택](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
