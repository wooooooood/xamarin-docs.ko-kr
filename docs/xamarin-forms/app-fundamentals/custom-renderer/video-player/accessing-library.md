---
title: "장치의 비디오 라이브러리 액세스"
ms.topic: article
ms.prod: xamarin
ms.assetid: 364C1D43-EAAE-45B9-BE24-0DA5AE74C4D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 95f992c2f3ca976c1e02ff3043ceef0a4a81e4f6
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="accessing-the-devices-video-library"></a>장치의 비디오 라이브러리 액세스

대부분의 모바일 장치 및 데스크톱 컴퓨터 장치의 카메라를 사용 하 여 레코드 비디오를 수가 있습니다. 사용자가 만드는 동영상은 장치에 파일로 저장 됩니다. 이러한 파일을 이미지 라이브러리에서 검색 하 여 재생 하 고는 `VideoPlayer` 다른 비디오와 마찬가지로 클래스입니다.

## <a name="the-photo-picker-dependency-service"></a>사진 선택 종속성 서비스

세 플랫폼 각각 사용자가 장치의 이미지 라이브러리에서 사진이 나 비디오를 선택할 수 있게 하는 기능을 포함 합니다. 장치 이미지 라이브러리에서 비디오를 재생 하는 첫 번째 단계는 각 플랫폼에서 이미지 선택기를 호출 하는 종속성 서비스를 만드는 중입니다. 아래에 설명 된 종속 서비스가 매우 비슷합니다에 정의 된 하나는 [ **사진 그림 라이브러리에서 선택** ](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md) 문서, 비디오 선택기는 아닌파일이름을반환한다는`Stream`개체입니다.

이라는 인터페이스를 정의 하는 PCL 프로젝트 `IVideoPicker` 종속성 서비스에 대 한:

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPicker
    {
        Task<string> GetVideoFileAsync();
    }
}
```

클래스를 포함 하는 세 플랫폼 각각 `VideoPicker` 이 인터페이스를 구현 하는 합니다.

### <a name="the-ios-video-picker"></a>IOS 비디오 선택

IOS `VideoPicker` iOS를 사용 하 여 [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) iOS에서 비디오를 ("영화" 라고도 함)을 제한 해야 것을 지정 하는 이미지 라이브러리에 액세스 하도록 `MediaType` 속성입니다. 다음에 유의 `VideoPicker` 명시적으로 구현 하는 `IVideoPicker` 인터페이스입니다. 또한는 `Dependency` 종속성 서비스로 서이 클래스를 식별 하는 특성입니다. 다음은 Xamarin.Forms는 플랫폼 프로젝트에서 해당 종속성 서비스를 허용 하는 두 가지 요구 사항:

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
            viewController.PresentModalViewController(imagePicker, true);

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

### <a name="the-android-video-picker"></a>Android 비디오 선택

Android 구현의 `IVideoPicker` 응용 프로그램의 활동의 일부인 콜백 메서드가 필요 합니다. 이런 이유로 `MainActivity` 두 개의 속성, 필드 및 콜백 메서드를 클래스 정의:

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

`OnCreate` 메서드에서 `MainActivity` 자체 인스턴스는 정적 저장 `Current` 속성입니다. 이 구현할 수 `IVideoPicker` 얻으려고는 `MainActivity` 시작에 대 한 인스턴스는 **선택 비디오** 선택:

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

추가 기능에는 `MainActivity` 개체의 유일한 코드는는 [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) 일반 응용 프로그램 코드를 지원 하도록 업데이트 해야 하는 솔루션의 `FormsVideoLibrary` 클래스입니다.

### <a name="the-uwp-video-picker"></a>UWP 비디오 선택

UWP 구현의 `IVideoPicker` 인터페이스 UWP에 사용 하 여 [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/)합니다. 사진 라이브러리를 사용 하 여 파일 검색을 시작 하 고 MP4 및 WMV (Windows Media Video) 파일 형식을 제한:

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

## <a name="invoking-the-dependency-service"></a>종속성 서비스를 호출합니다.

**라이브러리 비디오 재생** 의 페이지는 [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) 프로그램 비디오 선택 종속성 서비스를 사용 하는 방법을 보여 줍니다. XAML 파일에 포함 되어는 `VideoPlayer` 인스턴스 및 `Button` 레이블이 **비디오 라이브러리 표시**:

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

코드 숨김 파일에 포함 되어는 `Clicked` 에 대 한 처리기는 `Button`합니다. 에 대 한 호출에 필요한 종속성 서비스를 호출 `DependencyService.Get` 구현의 가져올 수는 `IVideoPicker` 플랫폼 프로젝트에 대 한 인터페이스입니다. `GetVideoFileAsync` 해당 인스턴스에서 메서드가 호출 됩니다.

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

`Clicked` 처리기를 사용 하 여 해당 파일 이름을 만들기는 `FileVideoSource` 개체를 설정 하는 `Source` 의 속성은 `VideoPlayer`합니다.

각각의 `VideoPlayerRenderer` 클래스의 코드가 포함 되어 해당 `SetSource` 형식의 개체에 대 한 메서드 `FileVideoSource`합니다. 이 다음과 같습니다.

### <a name="handling-ios-files"></a>IOS 파일 처리

IOS 버전 `VideoPlayerRenderer` 프로세스 `FileVideoSource` 정적을 사용 하 여 개체 `Asset.FromUrl` 파일 이름 사용 하 여 메서드. 이 `AVAsset` 장치의 이미지 라이브러리에 파일을 나타내는 개체입니다.

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

형식의 개체를 처리할 때 `FileVideoSource`, Android 구현의 `VideoPlayerRenderer` 사용 하 여는 `SetVideoPath` 방식의 `VideoView` 장치의 이미지 라이브러리에 파일을 지정 하려면:

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

형식의 개체를 처리할 때 `FileVideoSource`, UWP 구현의 `SetSource` 메서드를 만드는 데 필요한는 `StorageFile` 개체, 읽기를 위해 해당 열 및 스트림 개체를 전달는 `SetSource` 의 메서드는 `MediaElement`:

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

세 플랫폼 모두에 비디오 재생이 시작 되는 비디오 거의 바로 뒤 원본이 설정 때문에 파일이 장치에 다운로드할 필요가 없습니다.



## <a name="related-links"></a>관련 링크

- [비디오 플레이어 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
- [사진 그림 라이브러리에서 선택](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
