---
title: 사진 라이브러리에서 사진 선택
description: 이 문서에서는 Xamarin.Forms DependencyService 클래스를 사용하여 휴대폰의 사진 라이브러리에서 사진을 선택하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: 6669dbaff3cfb5b929261352b8db046b35ec5b4f
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "76020694"
---
# <a name="picking-a-photo-from-the-picture-library"></a>사진 라이브러리에서 사진 선택

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice/)

이 문서에서는 사용자가 휴대폰의 사진 라이브러리에서 사진을 선택할 수 있는 애플리케이션 만들기를 설명합니다. Xamarin.Forms에는 이 기능이 포함되지 않으므로 [`DependencyService`](xref:Xamarin.Forms.DependencyService)를 사용하여 각 플랫폼에서 기본 API에 액세스해야 합니다.

## <a name="creating-the-interface"></a>인터페이스 만들기

먼저, 원하는 기능이 표시되는 공유 코드에서 인터페이스를 만듭니다. 사진 선택 애플리케이션의 경우 하나의 메서드만 필요합니다. 이 메서드는 샘플 코드의.NET Standard 라이브러리의 [`IPhotoPickerService`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceDemos/Services/IPhotoPickerService.cs) 인터페이스에서 정의됩니다.

```csharp
namespace DependencyServiceDemos
{
    public interface IPhotoPickerService
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

`GetImageStreamAsync` 메서드는 해당 메서드가 신속하게 반환해야 하므로 비동기로 정의되지만 사용자가 사진 라이브러리를 검색하여 사진 하나를 선택하기까지 선택된 사진에 대한 `Stream` 개체를 반환할 수 없습니다.

이 인터페이스는 플랫폼별 코드를 사용하여 모든 플랫폼에서 구현됩니다.

## <a name="ios-implementation"></a>iOS 구현

`IPhotoPickerService` 인터페이스의 iOS 구현은 [**갤러리에서 사진 선택**](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery) 레시피 및 [샘플 코드](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery)에 설명된 대로 [`UIImagePickerController`](xref:UIKit.UIImagePickerController)를 사용합니다.

iOS 구현은 샘플 코드의 iOS 프로젝트에서 [`PhotoPickerService`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceDemos.iOS/Services/PhotoPickerService.cs) 클래스에 포함됩니다. 이 클래스를 `DependencyService` 관리자에 표시하려면 해당 클래스를 `Dependency` 유형의 [`assembly`] 특성을 사용하여 식별해야 합니다. 해당 클래스는 공개되어야 하며 명시적으로 `IPhotoPickerService` 인터페이스를 구현해야 합니다.

```csharp
[assembly: Dependency (typeof (PhotoPickerService))]
namespace DependencyServiceDemos.iOS
{
    public class PhotoPickerService : IPhotoPickerService
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

`FinishedPickingMedia` 이벤트 처리기는 사용자가 사진을 선택한 경우 호출됩니다. 하지만 해당 처리기는 `UIImage` 개체를 제공하고 `Task`는 .NET `Stream` 개체를 반환해야 합니다. 이 반환은 두 단계로 수행됩니다. 먼저 `UIImage` 개체가 `NSData` 개체에 저장된 메모리 내 PNG 또는 JPEG 파일로 변환된 다음, `NSData` 개체가 .NET `Stream` 개체로 변환됩니다. `TaskCompletionSource` 개체의 `SetResult` 메서드에 대한 호출은 `Stream` 개체를 제공하여 태스크를 완료합니다.

```csharp
namespace DependencyServiceDemos.iOS
{
    public class PhotoPickerService : IPhotoPickerService
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
                NSData data;
                if (args.ReferenceUrl.PathExtension.Equals("PNG") || args.ReferenceUrl.PathExtension.Equals("png"))
                {
                    data = image.AsPNG();
                }
                else
                {
                    data = image.AsJPEG(1);
                }
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

## <a name="android-implementation"></a>Android 구현

Android 구현은 [**이미지 선택**](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image) 레시피 및 [샘플 코드](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image)에 설명된 기술을 사용합니다. 그러나 사용자가 사진 라이브러리에서 이미지를 선택한 경우 호출되는 메서드는 `Activity`에서 파생된 클래스의 `OnActivityResult` 재정의입니다. 따라서 Android 프로젝트의 기본 [`MainActivity`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceDemos.Android/MainActivity.cs) 클래스는 `OnActivityResult` 메서드의 필드, 속성 및 재정의를 통해 보완되었습니다.

```csharp
public class MainActivity : FormsAppCompatActivity
{
    internal static MainActivity Instance { get; private set; }  

    protected override void OnCreate(Bundle savedInstanceState)
    {
        // ... 
        Instance = this;
    }
    // ...
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

iOS 구현과 같이 Android 구현은 태스크가 완료된 경우 `TaskCompletionSource`를 사용하여 신호를 보냅니다. 이 `TaskCompletionSource` 개체는 `MainActivity` 클래스의 공용 속성으로 정의됩니다. 이렇게 하면 Android 프로젝트의 [`PhotoPickerService`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceDemos.Android/Services/PhotoPickerService.cs) 클래스에서 속성을 참조할 수 있습니다. 이 클래스는 `GetImageStreamAsync` 메서드를 사용합니다.

```csharp
[assembly: Dependency(typeof(PhotoPickerService))]
namespace DependencyServiceDemos.Droid
{
    public class PhotoPickerService : IPhotoPickerService
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

## <a name="uwp-implementation"></a>UWP 구현

iOS 및 Android 구현과 달리 유니버설 Windows 플랫폼에 대한 사진 선택기 구현에는 `TaskCompletionSource` 클래스가 필요하지 않습니다. [`PhotoPickerService`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceDemos.UWP/Services/PhotoPickerService.cs) 클래스는 [`FileOpenPicker`](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/) 클래스를 사용하여 사진 라이브러리에 액세스합니다. `FileOpenPicker`의 `PickSingleFileAsync` 메서드 자체는 비동기이므로 `GetImageStreamAsync` 메서드는 해당 메서드(및 다른 비동기 메서드)를 통해 `await`를 사용하고 `Stream` 개체를 반환할 수 있습니다.

```csharp
[assembly: Dependency(typeof(PhotoPickerService))]
namespace DependencyServiceDemos.UWP
{
    public class PhotoPickerService : IPhotoPickerService
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

## <a name="implementing-in-shared-code"></a>공유 코드에서 구현

각 플랫폼에 대해 인터페이스를 구현했으므로 이제 .NET Standard 라이브러리의 공유 코드는 해당 인터페이스를 활용할 수 있습니다.

UI에는 클릭하여 사진을 선택할 수 있는 [`Button`](xref:Xamarin.Forms.Button)이 포함됩니다.

```xaml
<Button Text="Pick Photo"
        Clicked="OnPickPhotoButtonClicked" />
```

`Clicked` 이벤트 처리기는 `DependencyService` 클래스를 사용하여 `GetImageStreamAsync`를 호출합니다. 이렇게 하면 플랫폼 프로젝트에서 호출이 발생합니다. 메서드가 `Stream` 개체를 반환하는 경우 처리기는 `image` 개체의 `Source` 속성을 `Stream` 데이터로 설정합니다.

```csharp
async void OnPickPhotoButtonClicked(object sender, EventArgs e)
{
    (sender as Button).IsEnabled = false;

    Stream stream = await DependencyService.Get<IPhotoPickerService>().GetImageStreamAsync();
    if (stream != null)
    {
        image.Source = ImageSource.FromStream(() => stream);
    }

    (sender as Button).IsEnabled = true;
}
```

## <a name="related-links"></a>관련 링크

- [DependencyService(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice/)
- [갤러리에서 사진 선택(iOS)](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery)
- [이미지 선택(Android)](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image)
