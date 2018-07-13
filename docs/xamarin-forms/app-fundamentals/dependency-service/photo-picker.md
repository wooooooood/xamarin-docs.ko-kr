---
title: 사진 그림 라이브러리에서 선택
description: 이 문서에서는 Xamarin.Forms DependencyService 클래스를 사용 하 여 휴대폰의 사진 라이브러리에서 사진을 선택 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: b593815df9ce942a98496806116bacfa63e2a2d9
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38999087"
---
# <a name="picking-a-photo-from-the-picture-library"></a>사진 그림 라이브러리에서 선택

이 문서에서는 사진을 휴대폰의 사진 라이브러리에서 선택할 수 있는 응용 프로그램 만들기를 안내 합니다. 사용 해야 하는 Xamarin.Forms에는이 기능이 포함 되어 있지 않으므로, [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) 각 플랫폼에서 네이티브 Api에 액세스 합니다.  이 문서에서는 사용 하는 다음 단계를 설명 하는 `DependencyService` 이 태스크에 대 한 합니다.

- **[인터페이스를 만드는](#Creating_the_Interface)**  &ndash; 공유 코드에서 인터페이스를 만드는 방법을 이해 합니다.
- **[iOS 구현](#iOS_Implementation)**  &ndash; iOS 용 네이티브 코드에서 인터페이스를 구현 하는 방법을 알아봅니다.
- **[Android 구현을](#Android_Implementation)**  &ndash; Android 용 네이티브 코드에서 인터페이스를 구현 하는 방법을 알아봅니다.
- **[유니버설 Windows 플랫폼 구현](#UWP_Implementation)**  &ndash; 유니버설 Windows 플랫폼 (UWP)에 대 한 네이티브 코드에서 인터페이스를 구현 하는 방법을 알아봅니다.
- **[공유 코드에서 구현](#Implementing_in_Shared_Code)**  &ndash; 사용 하는 방법을 알아봅니다 `DependencyService` 공유 코드에서 기본 구현을 호출 합니다.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>인터페이스 만들기

먼저, 원하는 기능을 표현 하는 공유 코드에서 인터페이스를 만듭니다. 응용 프로그램을 사진 선택의 경우 하나의 메서드는 필요 합니다. 이것은 [ `IPicturePicker` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/IPicturePicker.cs) 샘플 코드의.NET Standard 라이브러리에서 인터페이스:

```csharp
namespace DependencyServiceSample
{
    public interface IPicturePicker
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

합니다 `GetImageStreamAsync` 메서드는이 메서드는 신속 하 게 반환 해야 하지만 반환할 수 없습니다 때문에 비동기로 정의 됩니다는 `Stream` 사용자가 그림 라이브러리를 검색 하 고 하나를 선택 될 때까지 선택한 사진에 대 한 개체입니다.

이 인터페이스는 플랫폼별 코드를 사용 하 여 모든 플랫폼에서 구현 됩니다.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS 구현

IOS 구현의 합니다 `IPicturePicker` 사용 하 여 인터페이스를 [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) 에 설명 된 대로 [ **갤러리에서 사진을 선택** ](https://developer.xamarin.com/recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery/) 작성법 및 [샘플 코드](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery)합니다.

IOS 구현에 포함 된 [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/iOS/PicturePickerImplementation.cs) 샘플 코드의 iOS 프로젝트에서 클래스. 이 클래스를 볼 수 있도록 합니다 `DependencyService` 관리자를 사용 하 여 클래스를 식별 해야는 [`assembly`] 유형의 특성입니다 `Dependency`, 클래스는 public 이어야 하며 명시적으로 구현 하 고는 `IPicturePicker` 인터페이스:

```csharp
[assembly: Dependency (typeof (PicturePickerImplementation))]

namespace DependencyServiceSample.iOS
{
    public class PicturePickerImplementation : IPicturePicker
    {
        TaskCompletionSource<Stream> taskCompletionSource;
        UIImagePickerController imagePicker;

        public Task<Stream> GetImageStreamAsync()
        {
            // Create and define UIImagePickerController
            imagePicker = new UIImagePickerController
            {
                SourceType = UIImagePickerControllerSourceType.PhotoLibrary,
                MediaTypes = UIImagePickerController.AvailableMediaTypes(UIImagePickerControllerSourceType.PhotoLibrary)
            };

            // Set event handlers
            imagePicker.FinishedPickingMedia += OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled += OnImagePickerCancelled;

            // Present UIImagePickerController;
            UIWindow window = UIApplication.SharedApplication.KeyWindow;
            var viewController = window.RootViewController;
            viewController.PresentModalViewController(imagePicker, true);

            // Return Task object
            taskCompletionSource = new TaskCompletionSource<Stream>();
            return taskCompletionSource.Task;
        }
        ...
    }
}

```

합니다 `GetImageStreamAsync` 메서드를 만듭니다를 `UIImagePickerController` 사진 라이브러리에서 이미지를 선택 하려면이 초기화 합니다. 두 명의 이벤트 처리기는 필요한: 사용자가 사진 및 다른 사용자가 사진 라이브러리의 표시를 취소 하는 경우를 선택 하는 경우에 대 한 합니다. `PresentModalViewController` 다음 사용자에 게 사진 라이브러리를 표시 합니다.

이 시점에서 합니다 `GetImageStreamAsync` 메서드 반환 해야 합니다는 `Task<Stream>` 개체 코드를 호출 하는 것입니다. 사용자가 사진 라이브러리와 상호 작용 하 고 이벤트 처리기 중 하나를 호출 하는 경우에이 태스크가 완료 되었습니다. 경우에는 [ `TaskCompletionSource` ](https://msdn.microsoft.com/library/dd449174(v=vs.110).aspx) 클래스는 필수입니다. 이 클래스는 제공를 `Task` 에서 반환할 적절 한 제네릭 형식의 개체는 `GetImageStreamAsync` 메서드 및 클래스는 나중에 신호를 보낼 수는 작업이 완료 되 면 합니다.

`FinishedPickingMedia` 사용자가 그림을 선택한 경우 이벤트 처리기가 호출 됩니다. 하지만 처리기를 제공, 한 `UIImage` 개체 및 `Task` .NET 반환 해야 합니다 `Stream` 개체입니다. 두 단계로 이루어집니다:는 `UIImage` 개체 먼저 메모리에 저장 된 JPEG 파일로로 변환 하는 `NSData` 개체를 차례로 `NSData` 개체를.NET으로 변환 됩니다 `Stream` 개체. 에 대 한 호출을 `SetResult` 메서드를 `TaskCompletionSource` 개체를 제공 하 여 작업을 완료를 `Stream` 개체:

```csharp
namespace DependencyServiceSample.iOS
{
    public class PicturePickerImplementation : IPicturePicker
    {
        TaskCompletionSource<Stream> taskCompletionSource;
        UIImagePickerController imagePicker;
        ...
        void OnImagePickerFinishedPickingMedia(object sender, UIImagePickerMediaPickedEventArgs args)
        {
            UIImage image = args.EditedImage ?? args.OriginalImage;

            if (image != null)
            {
                // Convert UIImage to .NET Stream object
                NSData data = image.AsJPEG(1);
                Stream stream = data.AsStream();

                // Set the Stream as the completion of the Task
                taskCompletionSource.SetResult(stream);
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

IOS 응용 프로그램에 휴대폰의 사진 라이브러리에 액세스 하려면 사용자의 권한이 필요 합니다. 다음을 추가 하 여 `dict` Info.plist 파일의 섹션:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Picture Picker uses photo library</string>
```


<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android 구현

Android 구현에 설명 된 기술을 사용 합니다 [ **이미지를 선택** ](https://developer.xamarin.com/recipes/android/other_ux/pick_image/) 작성법 및 [샘플 코드](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image)합니다. 그러나는 사용자가 그림 라이브러리에서 이미지를 선택 하는 경우 호출 되는 메서드를 `OnActivityResult` 에서 파생 된 클래스에서 재정의 `Activity`합니다. 따라서 수직 [ `MainActivity` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/MainActivity.cs) Android 프로젝트에 클래스 필드, 속성 및 재정의 사용 하 여 보완 되었습니다에 `OnActivityResult` 메서드:

```csharp
public class MainActivity : FormsAppCompatActivity
{
    ...
    // Field, property, and method for Picture Picker
    public static readonly int PickImageId = 1000;

    public TaskCompletionSource<Stream> PickImageTaskCompletionSource { set; get; }

    protected override void OnActivityResult(int requestCode, Result resultCode, Intent intent)
    {
        base.OnActivityResult(requestCode, resultCode, intent);

        if (requestCode == PickImageId)
        {
            if ((resultCode == Result.Ok) && (intent != null))
            {
                Android.Net.Uri uri = intent.Data;
                Stream stream = ContentResolver.OpenInputStream(uri);

                // Set the Stream as the completion of the Task
                PickImageTaskCompletionSource.SetResult(stream);
            }
            else
            {
                PickImageTaskCompletionSource.SetResult(null);
            }
        }
    }
}

```

`OnActivityResult`재정의 Android 사용 하 여 선택한 그림 파일을 나타냅니다 `Uri` 개체가 아니라.NET으로 변환할 수이 있습니다 `Stream` 호출 하 여 개체를 `OpenInputStream` 메서드를 `ContentResolver` 에서 가져온 개체는 활동의 `ContentResolver` 속성입니다.

Android 구현을 사용 하 여 iOS 구현 같은 `TaskCompletionSource` 작업이 완료 되 면 알립니다. 이렇게 `TaskCompletionSource` 개체에서 public 속성으로 정의 됩니다는 `MainActivity` 클래스입니다. 이렇게 하면 속성을 참조할 수는 [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/PicturePickerImplementation.cs) Android 프로젝트의 클래스입니다. 이 사용 하 여 클래스를 `GetImageStreamAsync` 메서드:

```csharp
[assembly: Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.Droid
{
    public class PicturePickerImplementation : IPicturePicker
    {
        public Task<Stream> GetImageStreamAsync()
        {
            // Define the Intent for getting images
            Intent intent = new Intent();
            intent.SetType("image/*");
            intent.SetAction(Intent.ActionGetContent);

            // Start the picture-picker activity (resumes in MainActivity.cs)
            MainActivity.Instance.StartActivityForResult(
                Intent.CreateChooser(intent, "Select Picture"),
                MainActivity.PickImageId);

            // Save the TaskCompletionSource object as a MainActivity property
            MainActivity.Instance.PickImageTaskCompletionSource = new TaskCompletionSource<Stream>();

            // Return Task object
            return MainActivity.Instance.PickImageTaskCompletionSource.Task;
        }
    }
}
```

액세스 하는이 메서드는 `MainActivity` 여러 목적에 대 한 클래스:에 대 한는 `Instance` 속성에 대 한를 `PickImageId` 필드에 대 한를 `TaskCompletionSource` 속성인을 호출 하 `StartActivityForResult`. 이 메서드는 정의한 합니다 `FormsAppCompatActivity` 클래스는 기본 클래스의 `MainActivity`합니다.

<a name="UWP_Implementation" />

## <a name="uwp-implementation"></a>UWP 구현

IOS 및 Android 구현와 달리 유니버설 Windows 플랫폼에 대 한 사진 선택기를 구현 하지 않아도 `TaskCompletionSource` 클래스입니다. [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/UWP/PicturePickerImplementation.cs) 클래스가 사용 하 여 [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/) 사진 라이브러리에 액세스 하는 클래스입니다. 때문에 합니다 `PickSingleFileAsync` 메서드의 `FileOpenPicker` 는 비동기 이며 자체는 `GetImageStreamAsync` 메서드를 사용 하기만 하면 `await` 메서드 (및 다른 비동기 메서드)를 사용 하 여 반환 하 고는 `Stream` 개체:


```csharp
[assembly: Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.UWP
{
    public class PicturePickerImplementation : IPicturePicker
    {
        public async Task<Stream> GetImageStreamAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary,
            };

            openPicker.FileTypeFilter.Add(".jpg");
            openPicker.FileTypeFilter.Add(".jpeg");
            openPicker.FileTypeFilter.Add(".png");

            // Get a file and return a Stream
            StorageFile storageFile = await openPicker.PickSingleFileAsync();

            if (storageFile == null)
            {
                return null;
            }

            IRandomAccessStreamWithContentType raStream = await storageFile.OpenReadAsync();
            return raStream.AsStreamForRead();
        }
    }
}
```

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>공유 코드에서 구현

각 플랫폼에 대 한 인터페이스를 구현한 다음에 이제.NET Standard 라이브러리에서 응용 프로그램 활용을 걸릴 수 있습니다.

합니다 [ `App` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/DependencyServiceSample.cs) 클래스를 만들고를 `Button` 사진을 선택:

```csharp
Button pickPictureButton = new Button
{
    Text = "Pick Photo",
    VerticalOptions = LayoutOptions.CenterAndExpand,
    HorizontalOptions = LayoutOptions.CenterAndExpand
};
stack.Children.Add(pickPictureButton);
```

`Clicked` 처리기에서 사용 합니다 `DependencyService` 클래스에서 호출할 `GetImageStreamAsync`합니다. 이 플랫폼 프로젝트에서 호출 됩니다. 메서드를 반환 하는 경우는 `Stream` 개체를 처리기를 만듭니다는 `Image` 를 통해 해당 상황에 대 한 요소를 `TabGestureRecognizer`, 하 고 대체를 `StackLayout` 된 페이지의 `Image`:

```csharp
pickPictureButton.Clicked += async (sender, e) =>
{
    pickPictureButton.IsEnabled = false;
    Stream stream = await DependencyService.Get<IPicturePicker>().GetImageStreamAsync();

    if (stream != null)
    {
        Image image = new Image
        {
            Source = ImageSource.FromStream(() => stream),
            BackgroundColor = Color.Gray
        };

        TapGestureRecognizer recognizer = new TapGestureRecognizer();
        recognizer.Tapped += (sender2, args) =>
        {
            (MainPage as ContentPage).Content = stack;
            pickPictureButton.IsEnabled = true;
        };
        image.GestureRecognizers.Add(recognizer);

        (MainPage as ContentPage).Content = image;
    }
    else
    {
        pickPictureButton.IsEnabled = true;
    }
};
```

탭의 `Image` 요소 정상으로 페이지를 반환 합니다.


## <a name="related-links"></a>관련 링크

- [(IOS) 갤러리에서 사진을 선택합니다](https://developer.xamarin.com/recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery/)
- [(Android) 이미지를 선택 합니다.](https://developer.xamarin.com/recipes/android/other_ux/pick_image/)
- [DependencyService (샘플)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample)
