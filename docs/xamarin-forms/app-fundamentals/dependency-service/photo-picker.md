---
title: "사진 그림 라이브러리에서 선택"
description: "DependencyService를 사용 하 여 휴대폰의 그림 라이브러리에서 사진을 선택합니다"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: 6ac228f85f3f717ddf95e0dc2e434b13bfec5d06
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="picking-a-photo-from-the-picture-library"></a>사진 그림 라이브러리에서 선택

이 문서는 사용자에 게 휴대폰의 그림 라이브러리에서 사진을 선택 허용 하는 응용 프로그램 만들기를 안내 합니다. 사용 해야 하는 Xamarin.Forms에는이 기능이 포함 되어 있지 않으므로, [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) 에 각 플랫폼에서 네이티브 Api에 액세스 합니다.  이 문서에서는 사용 하기 위한 다음 단계를 설명 하는 `DependencyService` 이 작업에 대 한 합니다.

- **[인터페이스를 만드는 방법](#Creating_the_Interface)**  &ndash; 공유 코드에서 인터페이스 만들어지는 방법을 이해 합니다.
- **[iOS 구현](#iOS_Implementation)**  &ndash; iOS에 대 한 네이티브 코드에서 인터페이스를 구현 하는 방법을 알아봅니다.
- **[Android 구현](#Android_Implementation)**  &ndash; Android 용 네이티브 코드에서 인터페이스를 구현 하는 방법을 알아봅니다.
- **[유니버설 Windows 플랫폼 구현](#UWP_Implementation)**  &ndash; 유니버설 Windows 플랫폼 (UWP)에 대 한 네이티브 코드에서 인터페이스를 구현 하는 방법을 알아봅니다.
- **[Windows Phone 구현](#Windows_Phone_Implementation)**  &ndash; 네이티브 코드에 Windows Phone 8.1에 대 한 인터페이스를 구현 하는 방법을 알아봅니다.
- **[공유 코드에서 구현](#Implementing_in_Shared_Code)**  &ndash; 사용 방법을 배울 `DependencyService` 공유 코드에서 네이티브 구현으로 호출할 수 있습니다.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>인터페이스 만들기

원하는 기능을 표현 하는 공유 코드에서 인터페이스를 먼저 만듭니다. 사진 선정 응용 프로그램의 경우 하나의 방법이 필요 합니다. 이것은에 정의 된 [ `IPicturePicker` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/IPicturePicker.cs) 인터페이스는 샘플 코드의 이식 가능한 클래스 라이브러리에:

```csharp
namespace DependencyServiceSample
{
    public interface IPicturePicker
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

`GetImageStreamAsync` 메서드 신속 하 게 반환 해야 하지만 반환할 수 없습니다 비동기으로 정의 됩니다는 `Stream` 사용자가 그림 라이브러리를 검색 하 고 하나를 선택한 될 때까지 선택 된 사진에 대 한 개체입니다.

이 인터페이스는 플랫폼별 코드를 사용 하는 모든 플랫폼에서 구현 됩니다.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS 구현

IOS 구현의 `IPicturePicker` 사용 하 여 인터페이스는 [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) 에 설명 된 대로 [ **사진 갤러리에서 선택** ](https://developer.xamarin.com/recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery/) 레시피 및 [샘플 코드](https://github.com/xamarin/recipes/tree/master/ios/media/video_and_photos/choose_a_photo_from_the_gallery)합니다.

IOS 구현에 포함 되어는 [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/iOS/PicturePickerImplementation.cs) 샘플 코드의 iOS 프로젝트에 클래스입니다. 이 클래스를 볼 수 있도록는 `DependencyService` 관리자 클래스도 식별 되어야 합니다는 [`assembly`] 유형의 특성입니다 `Dependency`, 클래스는 공용 이어야 하며 명시적으로 구현 하 고는 `IPicturePicker` 인터페이스:

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

`GetImageStreamAsync` 메서드 만듭니다는 `UIImagePickerController` 사진 라이브러리에서 이미지를 선택 하 여 초기화 합니다. 두 명의 이벤트 처리기는 필요한: 사용자가 사진을 용이고 다른 사용자가 사진 라이브러리의 표시를 취소 하는 경우를 선택 하는 경우에 대 한 합니다. `PresentModalViewController` 사진 라이브러리 사용자에 게 표시 합니다.

이 시점에서 `GetImageStreamAsync` 메서드 반환 해야 합니다는 `Task<Stream>` 개체 코드를 호출 하는 것입니다. 이 작업은 사용자가 사진 라이브러리와의 상호 작용 하 고 이벤트 처리기 중 하나가 호출 될 경우에 완료 됩니다. 이러한 상황에 대 한는 [ `TaskCompletionSource` ](https://msdn.microsoft.com/library/dd449174(v=vs.110).aspx) 클래스는 필수입니다. 이 클래스를 제공는 `Task` 에서 반환 하 여 적절 한 제네릭 형식의 개체는 `GetImageStreamAsync` 작업이 완료 되 면 메서드와 클래스 알릴 나중에 있습니다.

`FinishedPickingMedia` 사용자가 그림을 선택 하는 경우 이벤트 처리기가 호출 됩니다. 그러나 제공 된 처리기는 `UIImage` 개체 및 `Task` .NET 반환 해야 합니다 `Stream` 개체입니다. 이 두 단계로 수행:는 `UIImage` JPEG 파일에 저장 하는 메모리에 개체는 먼저 변환는 `NSData` 개체를 차례로 `NSData` 개체.NET으로 변환 됩니다 `Stream` 개체입니다. 에 대 한 호출에서 `SetResult` 의 메서드는 `TaskComkpletionSource` 개체가 제공 하 여 작업을 완료할는 `Stream` 개체:

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

IOS 응용 프로그램 사용자의 휴대폰의 사진 라이브러리 액세스 권한이 필요 합니다. 다음을 추가 `dict` Info.plist 파일의 섹션:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Picture Picker uses photo library</string>
```


<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android 구현

설명 하는 기법을 사용 하 여 Android 구현은 [ **이미지를 선택** ](https://developer.xamarin.com/recipes/android/other_ux/pick_image/) 레시피 및 [샘플 코드](https://github.com/xamarin/recipes/tree/master/android/other_ux/pick_image)합니다. 그러나 사용자가 그림 라이브러리에서 이미지를 선택 될 때 호출 되는 메서드는 한 `OnActivityResult` 에서 파생 된 클래스에서 재정의 `Activity`합니다. 이러한 이유로 표준 [ `MainActivity` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/MainActivity.cs) Android 프로젝트의 클래스에 필드, 속성을 재정의 하는 보완 되어가 `OnActivityResult` 메서드:

```csharp
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
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

`OnActivityResult`재정의 Android와 선택한 그림 파일을 나타내는 `Uri` 개체가 아니라.NET으로 변환할 수이 있습니다 `Stream` 호출 하 여 개체는 `OpenInputStream` 의 메서드는 `ContentResolver` 에서 가져온 개체는 활동의 `ContentResolver` 속성입니다.

Android 구현 마찬가지로 iOS 구현을 사용 하 여 한 `TaskCompletionSource` 작업이 완료 되 면 신호를 보내 합니다. 이 `TaskCompletionSource` 개체가에서 공용 속성으로 정의 된는 `MainActivity` 클래스입니다. 이렇게 하면 속성에서 참조 될 수는 [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/PicturePickerImplementation.cs) Android 프로젝트의 클래스. 이 클래스는 클래스와는 `GetImageStreamAsync` 메서드:

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

            // Get the MainActivity instance
            MainActivity activity = Forms.Context as MainActivity;

            // Start the picture-picker activity (resumes in MainActivity.cs)
            activity.StartActivityForResult(
                Intent.CreateChooser(intent, "Select Picture"),
                MainActivity.PickImageId);

            // Save the TaskCompletionSource object as a MainActivity property
            activity.PickImageTaskCompletionSource = new TaskCompletionSource<Stream>();

            // Return Task object
            return activity.PickImageTaskCompletionSource.Task;
        }
    }
}
```

액세스 하는 메서드는 `MainActivity` 여러 용도로 클래스:에 대 한는 `PickImageId` 필드에 대 한는 `TaskCompletionSource` 속성을 호출 하 고 `StartActivityForResult`합니다. 이 메서드는 여 정의 된는 `FormsApplicationActivity` 클래스의 기본 클래스 즉 `MainActivity`합니다.

<a name="UWP_Implementation" />

## <a name="uwp-implementation"></a>UWP 구현

IOS 및 Android 구현 달리 유니버설 Windows 플랫폼에 대 한 사진 선택기의 구현이 필요 하지 않습니다는 `TaskCompletionSource` 클래스입니다. [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/UWP/PicturePickerImplementation.cs) 클래스에서 사용 하 여 [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/) 사진 라이브러리 액세스 하는 클래스입니다. 때문에 `PickSingleFileAsync` 방식의 `FileOpenPicker` 는 그 자체가 비동기적 이며는 `GetImageStreamAsync` 메서드를 사용 하기만 하면 `await` 해당 메서드 (및 다른 비동기 메서드)로 반환 하 고는 `Stream` 개체:


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

<a name="Windows_Phone_Implementation" />

## <a name="windows-phone-81-implementation"></a>Windows Phone 8.1 구현

Windows Phone 8.1 구현은 큰 영향을 가진 하나의 큰 차이 제외 하 고 UWP 구현 비슷합니다:는 `PickSingleFileAsync` 방식의 `FileOpenPicker` 를 사용할 수 없습니다. 대신,는 `GetImageStreamAsync` 메서드를 호출 해야 `PickSingleFileAndContinue`합니다. 사진 라이브러리는 사용자에 게 표시 되지만 사용자가 선택한 사진의 호출 하 여 신호를 받는 경우이 메서드는 반환 된 `OnActivated` 의 메서드는 `App` 클래스입니다.

이러한 이유로 [ `App` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/WinPhone/App.xaml.cs) Xamarin.Forms 프로젝트 템플릿에 Windows Phone 8.1 프로젝트에 의해 만들어진 클래스 형식의 속성으로 보완 되었습니다에 `TaskCompletionSource` 및의 재정의 `OnActivated` 메서드:

```csharp
namespace DependencyServiceSample.WinPhone
{
    public sealed partial class App : Application
    {
        ...
        public TaskCompletionSource<Stream> TaskCompletionSource { set; get; }

        protected async override void OnActivated(IActivatedEventArgs args)
        {
            base.OnActivated(args);

            IContinuationActivatedEventArgs continuationArgs = args as IContinuationActivatedEventArgs;

            if (continuationArgs != null &&
                continuationArgs.Kind == ActivationKind.PickFileContinuation)
            {
                FileOpenPickerContinuationEventArgs pickerArgs = args as FileOpenPickerContinuationEventArgs;

                if (pickerArgs.Files.Count > 0)
                {
                    // Get the file and a Stream
                    StorageFile storageFile = pickerArgs.Files[0];
                    IRandomAccessStreamWithContentType raStream = await storageFile.OpenReadAsync();
                    Stream stream = raStream.AsStreamForRead();

                    // Set the completion of the Task
                    TaskCompletionSource.SetResult(stream);
                }
                else
                {
                    TaskCompletionSource.SetResult(null);
                }
            }
        }
    }
}
```

`OnActivated` 메서드는 응용 프로그램 시작을 비롯 한 여러 가지 이유로 호출 될 수 있습니다. 코드 사용 하도록 제한만 이러한 호출에는 파일 열기 선택기가 완료 하 고를 획득 한 `Stream` 에서 개체는 `StorageFile`합니다.

[ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/WinPhone/PicturePickerImplementation.cs) 클래스에 포함 되어는 `GetImageStreamAsync` 만드는 메서드를는 `FileOpenPicker` 및 호출 `PickSingleFileAndContainue`:

```csharp
[assembly: Xamarin.Forms.Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.WinPhone
{
    public class PicturePickerImplementation : IPicturePicker
    {
        public Task<Stream> GetImageStreamAsync()
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

            // Display the picker for a single file (resumes in OnActivated in App.xaml.cs)
            openPicker.PickSingleFileAndContinue();

            // Create a TaskCompletionSource stored in App.xaml.cs
            TaskCompletionSource<Stream> taskCompletionSource = new TaskCompletionSource<Stream>();
            (Application.Current as App).TaskCompletionSource = taskCompletionSource;

            // Return the Task object
            return taskCompletionSource.Task;
        }
    }
}

```

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>공유 코드에서 구현

각 플랫폼에 대 한 인터페이스를 구현한 했으므로 공통 이식 가능한 클래스 라이브러리에 응용 프로그램의 사용할 수 있습니다.

[ `App` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/DependencyServiceSample.cs) 클래스를 만듭니다는 `Button` 사진을 선택:

```csharp
Button pickPictureButton = new Button
{
    Text = "Pick Photo",
    VerticalOptions = LayoutOptions.CenterAndExpand,
    HorizontalOptions = LayoutOptions.CenterAndExpand
};
stack.Children.Add(pickPictureButton);
```

`Clicked` 처리기 사용 하 여는 `DependencyService` 클래스에서 호출할 `GetImageStreamAsync`합니다. 이 플랫폼 프로젝트에서 호출 됩니다. 메서드가 반환 하는 경우는 `Stream` 개체를 다음 처리기를 만듭니다는 `Image` 요소와 해당 사진에 대 한는 `TabGestureRecognizer`, 대체 하 고는 `StackLayout` 를 사용 하 여 페이지에 `Image`:

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

탭의 `Image` 요소 normal로 페이지를 반환 합니다.


## <a name="related-links"></a>관련 링크

- [사진 갤러리 (iOS)에서 선택](https://developer.xamarin.com/recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery/)
- [(Android) 이미지를 선택 합니다.](https://developer.xamarin.com/recipes/android/other_ux/pick_image/)
- [DependencyService (샘플)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample)
