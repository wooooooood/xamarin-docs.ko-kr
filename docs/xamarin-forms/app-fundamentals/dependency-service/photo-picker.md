---
title: 사진 라이브러리에서 사진 선택
description: 이 문서에서는 Xamarin.Forms DependencyService 클래스를 사용하여 휴대폰의 사진 라이브러리에서 사진을 선택하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: 00308a6c7883d4ac6ce41592d4a0e18f9fb28d52
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113315"
---
# <a name="picking-a-photo-from-the-picture-library"></a>사진 라이브러리에서 사진 선택

이 문서에서는 사용자가 휴대폰의 사진 라이브러리에서 사진을 선택할 수 있는 애플리케이션 만들기를 설명합니다. Xamarin.Forms에는 이 기능이 포함되지 않으므로 [`DependencyService`](xref:Xamarin.Forms.DependencyService)를 사용하여 각 플랫폼에서 기본 API에 액세스해야 합니다.  이 문서에서는 이 태스크에 대한 `DependencyService`를 사용하기 위한 다음 단계를 설명합니다.

- **[인터페이스 만들기](#Creating_the_Interface)** &ndash; 공유 코드에서 인터페이스를 만드는 방법을 이해합니다.
- **[iOS 구현](#iOS_Implementation)**  &ndash; iOS용 네이티브 코드에서 인터페이스를 구현하는 방법을 알아봅니다.
- **[Android 구현](#Android_Implementation)**  &ndash; Android용 네이티브 코드에서 인터페이스를 구현하는 방법을 알아봅니다.
- **[유니버설 Windows 플랫폼 구현](#UWP_Implementation)** &ndash; UWP(유니버설 Windows 플랫폼)용 네이티브 코드에서 인터페이스를 구현하는 방법을 알아봅니다.
- **[공유 코드에서 구현](#Implementing_in_Shared_Code)** &ndash; 공유 코드에서 기본 구현을 호출하기 위해 `DependencyService`를 사용하는 방법을 알아봅니다.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>인터페이스 만들기

먼저, 원하는 기능이 표시되는 공유 코드에서 인터페이스를 만듭니다. 사진 선택 애플리케이션의 경우 하나의 메서드만 필요합니다. 이 메서드는 샘플 코드의.NET Standard 라이브러리의 [`IPicturePicker`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/IPicturePicker.cs) 인터페이스에서 정의됩니다.

```csharp
namespace DependencyServiceSample
{
    public interface IPicturePicker
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

`GetImageStreamAsync` 메서드는 해당 메서드가 신속하게 반환해야 하므로 비동기로 정의되지만 사용자가 사진 라이브러리를 검색하여 사진 하나를 선택하기까지 선택된 사진에 대한 `Stream` 개체를 반환할 수 없습니다.

이 인터페이스는 플랫폼별 코드를 사용하여 모든 플랫폼에서 구현됩니다.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS 구현

`IPicturePicker` 인터페이스의 iOS 구현은 [**갤러리에서 사진 선택**](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery) 레시피 및 [샘플 코드](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery)에 설명된 대로 [`UIImagePickerController`](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/)를 사용합니다.

iOS 구현은 샘플 코드의 iOS 프로젝트에서 [`PicturePickerImplementation`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/iOS/PicturePickerImplementation.cs) 클래스에 포함됩니다. 이 클래스를 `DependencyService` 관리자에 표시하려면 해당 클래스를 `Dependency` 유형의 [`assembly`] 특성을 사용하여 식별해야 합니다. 해당 클래스는 공개되어야 하며 명시적으로 `IPicturePicker` 인터페이스를 구현해야 합니다.

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

`GetImageStreamAsync` 메서드는 사진 라이브러리에서 이미지를 선택하려면 `UIImagePickerController`를 만들고 초기화합니다. 두 개의 이벤트 처리기가 필요합니다. 하나는 사용자가 사진을 선택할 경우 필요하며 다른 하나는 사용자가 사진 라이브러리의 표시를 취소하는 경우 필요합니다. 그런 다음, `PresentModalViewController`는 사용자에게 사진 라이브러리를 표시합니다.

이 시점에서 `GetImageStreamAsync` 메서드는 `Task<Stream>` 개체를 호출하는 코드에 해당 개체를 반환해야 합니다. 이 태스크는 사용자가 사진 라이브러리와 상호 작용을 완료하고 이벤트 처리기 중 하나를 호출하는 경우에만 완료됩니다. 이와 같은 경우에 [`TaskCompletionSource`](https://msdn.microsoft.com/library/dd449174(v=vs.110).aspx) 클래스는 필수입니다. 클래스는 `GetImageStreamAsync` 메서드에서 반환할 적절한 제네릭 형식의 `Task` 개체를 제공하며, 해당 클래스는 태스크를 완료한 경우 나중에 신호를 받을 수 있습니다.

`FinishedPickingMedia` 이벤트 처리기는 사용자가 사진을 선택한 경우 호출됩니다. 하지만 해당 처리기는 `UIImage` 개체를 제공하고 `Task`는 .NET `Stream` 개체를 반환해야 합니다. 이 반환은 두 단계로 수행됩니다. 먼저 `UIImage` 개체가 `NSData` 개체에 저장된 메모리에서 JPEG 파일로 변환된 다음, `NSData` 개체가 .NET `Stream` 개체로 변환됩니다. `TaskCompletionSource` 개체의 `SetResult` 메서드에 대한 호출은 `Stream` 개체를 제공하여 태스크를 완료합니다.

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

                UnregisterEventHandlers();

                // Set the Stream as the completion of the Task
                taskCompletionSource.SetResult(stream);
            }
            else
            {
                UnregisterEventHandlers();
                taskCompletionSource.SetResult(null);
            }
            imagePicker.DismissModalViewController(true);
        }

        void OnImagePickerCancelled(object sender, EventArgs args)
        {
            UnregisterEventHandlers();
            taskCompletionSource.SetResult(null);
            imagePicker.DismissModalViewController(true);
        }

        void UnregisterEventHandlers()
        {
            imagePicker.FinishedPickingMedia -= OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled -= OnImagePickerCancelled;
        }
    }
}

```

iOS 애플리케이션에는 휴대폰의 사진 라이브러리에 액세스하려면 사용자의 권한이 필요합니다. 다음을 Info.plist 파일의 `dict` 섹션에 추가합니다.

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Picture Picker uses photo library</string>
```


<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android 구현

Android 구현은 [**이미지 선택**](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image) 레시피 및 [샘플 코드](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image)에 설명된 기술을 사용합니다. 그러나 사용자가 사진 라이브러리에서 이미지를 선택한 경우 호출되는 메서드는 `Activity`에서 파생된 클래스의 `OnActivityResult` 재정의입니다. 따라서 Android 프로젝트의 기본 [`MainActivity`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/MainActivity.cs) 클래스는 `OnActivityResult` 메서드의 필드, 속성 및 재정의를 통해 보완되었습니다.

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

`OnActivityResult` 재정의는 Android `Uri` 개체를 사용하여 선택한 사진 파일을 나타내지만 활동의 `ContentResolver` 속성에서 가져온 `ContentResolver` 개체의 `OpenInputStream` 메서드를 호출하여 이 파일을 .NET `Stream` 개체로 변환할 수 있습니다.

iOS 구현과 같이 Android 구현은 태스크가 완료된 경우 `TaskCompletionSource`를 사용하여 신호를 보냅니다. 이 `TaskCompletionSource` 개체는 `MainActivity` 클래스의 공용 속성으로 정의됩니다. 이렇게 하면 Android 프로젝트의 [`PicturePickerImplementation`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/PicturePickerImplementation.cs) 클래스에서 속성을 참조할 수 있습니다. 이 클래스는 `GetImageStreamAsync` 메서드를 사용합니다.

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

이 메서드는 `Instance` 속성, `PickImageId` 필드, `TaskCompletionSource` 속성을 위해, `StartActivityForResult`를 호출하기 위해 등 여러 목적으로 `MainActivity` 클래스에 액세스합니다. 이 메서드는 `MainActivity`의 기본 클래스인 `FormsAppCompatActivity` 클래스에 따라 정의됩니다.

<a name="UWP_Implementation" />

## <a name="uwp-implementation"></a>UWP 구현

iOS 및 Android 구현과 달리 유니버설 Windows 플랫폼에 대한 사진 선택기 구현에는 `TaskCompletionSource` 클래스가 필요하지 않습니다. [`PicturePickerImplementation`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/UWP/PicturePickerImplementation.cs) 클래스는 [`FileOpenPicker`](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/) 클래스를 사용하여 사진 라이브러리에 액세스합니다. `FileOpenPicker`의 `PickSingleFileAsync` 메서드 자체는 비동기이므로 `GetImageStreamAsync` 메서드는 해당 메서드(및 다른 비동기 메서드)를 통해 `await`를 사용하고 `Stream` 개체를 반환할 수 있습니다.


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

## <a name="implementing-in-shared-code"></a>공유 코드에서의 구현

각 플랫폼에 대해 인터페이스를 구현했으므로 이제 .NET Standard 라이브러리의 애플리케이션은 해당 인터페이스를 활용할 수 있습니다.

[`App`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/DependencyServiceSample.cs) 클래스는 `Button`을 만들어 사진을 선택합니다.

```csharp
Button pickPictureButton = new Button
{
    Text = "Pick Photo",
    VerticalOptions = LayoutOptions.CenterAndExpand,
    HorizontalOptions = LayoutOptions.CenterAndExpand
};
stack.Children.Add(pickPictureButton);
```

`Clicked` 처리기는 `DependencyService` 클래스를 사용하여 `GetImageStreamAsync`를 호출합니다. 이렇게 하면 플랫폼 프로젝트에서 호출이 발생합니다. 메서드가 `Stream` 개체를 반환하는 경우 처리기는 `TapGestureRecognizer`를 통해 `Image` 요소를 만들고 `Image`로 해당 페이지의 `StackLayout`을 바꿉니다.

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

`Image` 요소를 탭하면 해당 페이지가 기본으로 반환됩니다.


## <a name="related-links"></a>관련 링크

- [갤러리에서 사진 선택(iOS)](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery)
- [이미지 선택(Android)](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image)
- [DependencyService(샘플)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample)
