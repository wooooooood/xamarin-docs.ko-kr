---
title: SkiaSharp 비트맵 파일을 저장 하는 중
description: 사용자의 사진 라이브러리에 비트맵을 저장 하기 위한 SkiaSharp에서 지 원하는 다양 한 파일 형식을 살펴봅니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 2D696CB6-B31B-42BC-8D3B-11D63B1E7D9C
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 7f34bd5bbab4accaa30c22266dacd30692bf9ccc
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107575"
---
# <a name="saving-skiasharp-bitmaps-to-files"></a>SkiaSharp 비트맵 파일을 저장 하는 중

SkiaSharp 응용 프로그램을 만들거나 수정 비트맵에, 후 응용 프로그램 사용자의 사진 라이브러리에 비트맵 저장 할 수 있습니다.

![비트맵 저장](saving-images/SavingSample.png "비트맵 저장")

이 작업은 두 단계:

- SkiaSharp 비트맵 JPEG 또는 PNG와 같은 특정 파일 형식으로 데이터를에서 변환 합니다.
- 플랫폼별 코드를 사용 하 여 사진 라이브러리에 결과 저장 합니다.

## <a name="file-formats-and-codecs"></a>파일 형식 및 코덱

대부분 오늘날 인기 있는 비트맵 파일의 저장소 공간을 줄이기 위해 사용 하 여 압축 형식을 지정 합니다. 두 가지 범주의 압축 기술 이라고 _손실_ 하 고 _무손실_합니다. 이러한 용어는 압축 알고리즘을 데이터 손실에서 발생 여부를 나타냅니다.

가장 인기 있는 손실 Joint Photographic Experts Group에서 개발한 형식과 JPEG 라고 합니다. JPEG 압축 알고리즘 이라고 하는 수학 도구를 사용 하 여 이미지를 분석 하는 _불연속 코사인 변환_, 이미지의 시각적 품질을 유지 하는 데 중요 하지 않은 데이터를 제거 하려고 합니다. 일반적으로 라고 하는 설정을 사용 하 여 압축 수준을 제어할 수 있습니다 _품질_합니다. 더 높은 품질 설정으로 인해 더 큰 파일입니다.

반면, 손실 없는 압축 알고리즘을 반복 하 고 픽셀 데이터를 줄이지만의 모든 정보가 손실 되지 않습니다 하는 방식으로 인코딩할 수 있는 패턴에 대 한 이미지를 분석 합니다. 원래 비트맵 데이터를 압축된 파일에서 복원할 수 있습니다. 주 손실 없는 압축 된 파일 형식을 사용 하 여 PNG 이동식 네트워크 그래픽 () 임.

일반적으로 알고리즘 방식으로 또는 수동으로 생성 된 이미지에 대 한 PNG 사용 하는 동안 JPEG 사진에 사용 됩니다. 일부 파일의 크기를 줄이는 손실 없는 압축 알고리즘 반드시 다른 크기를 늘려야 합니다. 다행 스럽게도이 증가 크기에만 발생 임의 (또는 임의의) 정보가 많이 포함 된 데이터에 대 한 합니다.

압축 알고리즘은 두 용어는 보증 하는 복잡 한 압축 및 압축 풀기 프로세스에 설명 합니다.

- _디코딩_ &mdash; 비트맵 파일 형식을 읽고 압축을 풀어
- _인코딩할_ &mdash; 비트맵을 압축 하 고 비트맵 파일 형식으로 작성 합니다.

합니다 [ `SKBitmap` ](xref:SkiaSharp.SKBitmap) 메서드가 여러 개를 포함 하는 클래스 `Decode` 생성 하는 `SKBitmap` 압축 된 원본에서 합니다. 필요한 모든 파일 이름, 스트림 또는 바이트 배열을 제공 하는 것입니다. 디코더는 파일 형식을 결정 하 고 적절 한 내부 디코딩 함수에 전달할 수 있습니다.

또한 합니다 [ `SKCodec` ](xref:SkiaSharp.SKCodec) 클래스 라는 두 가지 방법에 `Create` 을 만들 수는 `SKCodec` 압축 된 원본에서 개체 및 응용 프로그램이 디코딩 프로세스에서 더 참여 하도록 허용 합니다. (합니다 `SKCodec` 클래스는 문서에 표시 됩니다 [ **SkiaSharp 비트맵 애니메이션** ](animating.md#gif-animation) 애니메이션된 GIF 파일 디코딩와 관련 하 여.)

비트맵 인코딩, 더 많은 정보를 필요한: 인코더에는 응용 프로그램 (JPEG 또는 PNG 또는 다른)를 사용 하려는 특정 파일 형식을 알고 있어야 합니다. 손실 형식으로 필요한 경우 인코딩 원하는 수준의 품질도 알아야 합니다. 

합니다 `SKBitmap` 클래스를 정의 [ `Encode` ](xref:SkiaSharp.SKBitmap.Encode(SkiaSharp.SKWStream,SkiaSharp.SKEncodedImageFormat,System.Int32)) 다음 구문 사용 하 여 메서드:

```csharp
public Boolean Encode (SKWStream dst, SKEncodedImageFormat format, Int32 quality)
```

이 메서드는 곧 자세히 설명 되어 있습니다. 인코드된 비트맵 쓰기 가능한 스트림을에 기록 됩니다. ('W'에서 `SKWStream` "쓰기"는 의미입니다.) 두 번째와 세 번째 인수는 파일 형식을 지정 하 고 (손실 형식)에 대 한 0에서 100 사이의 원하는 품질입니다.

또한 합니다 [ `SKImage` ](xref:SkiaSharp.SKImage) 및 [ `SKPixmap` ](xref:SkiaSharp.SKPixmap) 클래스를 정의할 수도 `Encode` 방법과 약간 더 나쁠 수 있는 것을 선호할 수 있는 합니다. 쉽게 만들 수 있습니다는 `SKImage` 에서 개체를 `SKBitmap` 사용 하는 정적 개체 [ `SKImage.FromBitmap` ](xref:SkiaSharp.SKImage.FromBitmap(SkiaSharp.SKBitmap)) 메서드. 가져올 수 있습니다는 `SKPixmap` 에서 개체를 `SKBitmp` 사용 하 여 개체를 [ `PeekPixels` ](xref:SkiaSharp.SKBitmap.PeekPixels) 메서드.

중 하나는 [ `Encode` ](xref:SkiaSharp.SKImage.Encode) 정의한 메서드 `SKImage` 매개 변수가 없는 및 PNG 형식으로 자동으로 저장 합니다. 해당 매개 변수가 없는 메서드는 사용 하기가 매우 간편 합니다.

## <a name="platform-specific-code-for-saving-bitmap-files"></a>비트맵 파일을 저장 하기 위한 플랫폼 특정 코드

인코딩할 때는 `SKBitmap` 개체로 특정 파일 형식에 수 일반적으로 일종의 스트림 개체와 데이터의 배열 이어야 합니다. 일부를 `Encode` 메서드 (정의한 매개 변수가 없는 것을 포함 하 여 `SKImage`) 반환을 [ `SKData` ](xref:SkiaSharp.SKData) 를 사용 하 여 바이트 배열로 변환 될 수 있는 개체를 [ `ToArray` ](xref:SkiaSharp.SKData.ToArray) 메서드. 이 데이터 파일에 저장 해야 합니다. 

응용 프로그램 로컬 저장소에서 파일에 저장 하는 표준 사용할 수 있으므로 쉽게 `System.IO` 클래스 및이 태스크에 대 한 메서드. 이 기술 문서에 설명 되어 [ **SkiaSharp 비트맵 애니메이션** ](animating.md#bitmap-animation) 관련 하 여 일련의 비트맵 Mandelbrot 집합의 애니메이션.

다른 응용 프로그램에서 공유 하도록 파일을 원하는 경우 사용자의 사진 라이브러리에 저장 해야 합니다. 이 작업을 수행 하려면 Xamarin.Forms 사용 하 여 플랫폼 특정 코드 [ `DependencyService` ](xref:Xamarin.Forms.DependencyService)합니다.

**SkiaSharpFormsDemo** 프로젝트를 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 응용 프로그램 정의 `IPhotoLibrary` 사용 하는 인터페이스를 `DependencyService` 클래스. 이 구문을 정의 `SavePhotoAsync` 메서드:

```csharp
public interface IPhotoLibrary
{
    Task<Stream> PickPhotoAsync();

    Task<bool> SavePhotoAsync(byte[] data, string folder, string filename);
}
```

또한이 인터페이스를 정의 합니다 `PickPhotoAsync` 메서드를 사용 하는 장치의 사진 라이브러리에 대 한 플랫폼별 파일 선택기를 엽니다.

에 대 한 `SavePhotoAsync`, 첫 번째 인수는 비트맵, JPEG 또는 PNG와 같은 특정 파일 형식으로 인코딩된 이미 포함 하는 바이트 배열입니다. 응용 프로그램에 파일 이름 뒤에 다음 매개 변수에 지정 된 특정 폴더를 만들면 모든 비트맵을 격리 하기를 원하는 가능성이 있습니다. 메서드 또는 성공 여부를 나타내는 부울 값을 반환 합니다.

다음은 어떻게 `SavePhotoAsync` 세 가지 플랫폼에서 구현 됩니다:

### <a name="the-ios-implementation"></a>IOS 구현

IOS 구현의 `SavePhotoAsync` 사용 하 여 [ `SaveToPhotosAlbum` ](https://developer.xamarin.com/api/member/UIKit.UIImage.SaveToPhotosAlbum/) 메서드의 `UIImage`:

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        NSData nsData = NSData.FromArray(data);
        UIImage image = new UIImage(nsData);
        TaskCompletionSource<bool> taskCompletionSource = new TaskCompletionSource<bool>();

        image.SaveToPhotosAlbum((UIImage img, NSError error) =>
        {
            taskCompletionSource.SetResult(error == null);
        });

        return taskCompletionSource.Task;
    }
}
```

아쉽게도 없기 파일 이름 또는 이미지에 대 한 폴더를 지정 합니다. 

합니다 **Info.plist** iOS 프로젝트의 파일에는 사진 라이브러리에 이미지를 추가한 것입니다 나타내는 키가 필요 합니다.

```xml
<key>NSPhotoLibraryAddUsageDescription</key>
<string>SkiaSharp Forms Demos adds images to your photo library</string>
```

조심하세요! 단순히 사진 라이브러리에 액세스 하기 위한 권한을 키는 매우 유사 하지만 동일 하지는 않습니다.

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>SkiaSharp Forms Demos accesses your photo library</string>
```

### <a name="the-android-implementation"></a>Android 구현

Android 구현의 `SavePhotoAsync` 있는지 먼저 확인 합니다 `folder` 인수가 `null` 또는 빈 문자열입니다. 그렇다면 비트맵 사진 라이브러리의 루트 디렉터리에 저장 됩니다. 이 고, 그렇지 폴더를 가져오고 존재 하지 않는 경우 만들어집니다.

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public async Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        try
        {
            File picturesDirectory = Environment.GetExternalStoragePublicDirectory(Environment.DirectoryPictures);
            File folderDirectory = picturesDirectory;

            if (!string.IsNullOrEmpty(folder))
            {
                folderDirectory = new File(picturesDirectory, folder);
                folderDirectory.Mkdirs();
            }

            using (File bitmapFile = new File(folderDirectory, filename))
            {
                bitmapFile.CreateNewFile();

                using (FileOutputStream outputStream = new FileOutputStream(bitmapFile))
                {
                    await outputStream.WriteAsync(data);
                }

                // Make sure it shows up in the Photos gallery promptly.
                MediaScannerConnection.ScanFile(MainActivity.Instance,
                                                new string[] { bitmapFile.Path },
                                                new string[] { "image/png", "image/jpeg" }, null);
            }
        }
        catch
        {
            return false;
        }

        return true;
    }
}
```

에 대 한 호출 `MediaScannerConnection.ScanFile` 엄격 하 게 필요 하지 않습니다 하지만 라이브러리 갤러리 보기를 업데이트 하 여 많은 통해 즉시 사진 라이브러리를 확인 하 여 프로그램을 테스트 하는 경우.

합니다 **AndroidManifest.xml** 파일에 다음 사용 권한 태그가 필요 합니다.

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

### <a name="the-uwp-implementation"></a>UWP 구현

UWP 구현의 `SavePhotoAsync` Android 구현 구조의 매우 유사 합니다.

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public async Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        StorageFolder picturesDirectory = KnownFolders.PicturesLibrary;
        StorageFolder folderDirectory = picturesDirectory;

        // Get the folder or create it if necessary
        if (!string.IsNullOrEmpty(folder))
        {
            try
            {
                folderDirectory = await picturesDirectory.GetFolderAsync(folder);
            }
            catch
            { }

            if (folderDirectory == null)
            {
                try
                {
                    folderDirectory = await picturesDirectory.CreateFolderAsync(folder);
                }
                catch
                {
                    return false;
                }
            }
        }

        try
        {
            // Create the file.
            StorageFile storageFile = await folderDirectory.CreateFileAsync(filename,
                                                CreationCollisionOption.GenerateUniqueName);

            // Convert byte[] to Windows buffer and write it out.
            IBuffer buffer = WindowsRuntimeBuffer.Create(data, 0, data.Length, data.Length);
            await FileIO.WriteBufferAsync(storageFile, buffer);
        }
        catch
        {
            return false;
        }

        return true;
    }
}
```

합니다 **기능** 섹션을 **Package.appxmanifest** 파일에 필요한 **사진 라이브러리**합니다.

## <a name="exploring-the-image-formats"></a>이미지 형식 탐색

다음은 [ `Encode` ](xref:SkiaSharp.SKBitmap.Encode(SkiaSharp.SKWStream,SkiaSharp.SKEncodedImageFormat,System.Int32)) 메서드의 `SKImage` 다시 합니다.

```csharp
public Boolean Encode (SKWStream dst, SKEncodedImageFormat format, Int32 quality)
```

[`SKEncodedImageFormat`](xref:SkiaSharp.SKEncodedImageFormat) 일부는 다소 모호한 11 비트맵 파일 형식을 참조 하는 멤버로 구성 된 열거형이 같습니다.

- `Astc` &mdash; 적응 확장성 있는 질감 압축
- `Bmp` &mdash; Windows 비트맵
- `Dng` &mdash; Adobe 디지털 네거티브
- `Gif` &mdash; Graphics Interchange Format
- `Ico` &mdash; Windows 아이콘 이미지
- `Jpeg` &mdash; Joint Photographic Experts Group
- `Ktx` &mdash; OpenGL Khronos 질감 형식
- `Pkm` &mdash; Pokémon 파일 저장
- `Png` &mdash; 이동식 네트워크 그래픽
- `Wbmp` &mdash; 무선 응용 프로그램 프로토콜 비트맵 형식 (픽셀당 1 비트)
- `Webp` &mdash; Google WebP 형식

곧 확인 하겠지만 이러한 3 개만 파일 형식 (`Jpeg`, `Png`, 및 `Webp`) SkiaSharp 실제로 지 합니다.

저장 하는 `SKBitmap` 라는 개체 `bitmap` 사용자의 사진 라이브러리도 하려면의 구성원을 `SKEncodedImageFormat` 라는 열거형 `imageFormat` 한 (손실 형식) 정수 `quality` 변수입니다. 다음 코드를 사용 하 여 해당 비트맵 이름 사용 하 여 파일 저장 `filename` 에 `folder` 폴더:

```csharp
using (MemoryStream memStream = new MemoryStream())
using (SKManagedWStream wstream = new SKManagedWStream(memStream))
{
    bitmap.Encode(wstream, imageFormat, quality);
    byte[] data = memStream.ToArray();

    // Check the data array for content!

    bool success = await DependencyService.Get<IPhotoLibrary>().SavePhotoAsync(data, folder, filename);

    // Check return value for success!
}
```

합니다 `SKManagedWStream` 클래스에서 파생 되며 `SKWStream` (나타내는 "쓰기 가능 스트림"). `Encode` 메서드는 스트림으로 인코딩된 비트맵 파일을 씁니다. 일부 오류 검사를 수행 해야를 해당 코드의 주석을 참조 하십시오. 

**저장 파일 형식** 페이지에 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 응용 프로그램에서 유사한 코드를 사용 하 여 다양 한 형식에서 비트맵을 저장 하는 실험을 수행할 수 있도록 합니다.

XAML 파일에 포함 되어는 `SKCanvasView` 비트맵을 표시 하는, 응용 프로그램을 호출 해야 하지만 페이지의 나머지 부분에 있는 모든 항목을 `Encode` 메서드의 `SKBitmap`합니다. 있기를 `Picker` 의 멤버에 대 한는 `SKEncodedImageFormat` 열거형을 `Slider` 손실 비트맵 형식에 대 한 품질 인수에 대 한 두 `Entry` 파일 이름 및 폴더 이름에 대 한 뷰 및 `Button` 파일을 저장 하는 것에 대 한 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.SaveFileFormatsPage"
             Title="Save Bitmap Formats">

    <StackLayout Margin="10">
        <skiaforms:SKCanvasView PaintSurface="OnCanvasViewPaintSurface"
                                VerticalOptions="FillAndExpand" />

        <Picker x:Name="formatPicker"
                Title="image format"
                SelectedIndexChanged="OnFormatPickerChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKEncodedImageFormat}">
                    <x:Static Member="skia:SKEncodedImageFormat.Astc" />
                    <x:Static Member="skia:SKEncodedImageFormat.Bmp" />
                    <x:Static Member="skia:SKEncodedImageFormat.Dng" />
                    <x:Static Member="skia:SKEncodedImageFormat.Gif" />
                    <x:Static Member="skia:SKEncodedImageFormat.Ico" />
                    <x:Static Member="skia:SKEncodedImageFormat.Jpeg" />
                    <x:Static Member="skia:SKEncodedImageFormat.Ktx" />
                    <x:Static Member="skia:SKEncodedImageFormat.Pkm" />
                    <x:Static Member="skia:SKEncodedImageFormat.Png" />
                    <x:Static Member="skia:SKEncodedImageFormat.Wbmp" />
                    <x:Static Member="skia:SKEncodedImageFormat.Webp" />
                </x:Array>
            </Picker.ItemsSource>
        </Picker>

        <Slider x:Name="qualitySlider"
                Maximum="100"
                Value="50" />

        <Label Text="{Binding Source={x:Reference qualitySlider},
                              Path=Value,
                              StringFormat='Quality = {0:F0}'}"
               HorizontalTextAlignment="Center" />

        <StackLayout Orientation="Horizontal">
            <Label Text="Folder Name: "
                   VerticalOptions="Center" />

            <Entry x:Name="folderNameEntry"
                   Text="SaveFileFormats"
                   HorizontalOptions="FillAndExpand" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="File Name: "
                   VerticalOptions="Center" />

            <Entry x:Name="fileNameEntry"
                   Text="Sample.xxx"
                   HorizontalOptions="FillAndExpand" />
        </StackLayout>

        <Button Text="Save" 
                Clicked="OnButtonClicked">
            <Button.Triggers>
                <DataTrigger TargetType="Button"
                             Binding="{Binding Source={x:Reference formatPicker},
                                               Path=SelectedIndex}"
                             Value="-1">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>

                <DataTrigger TargetType="Button"
                             Binding="{Binding Source={x:Reference fileNameEntry},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Button.Triggers>
        </Button>

        <Label x:Name="statusLabel"
               Text="OK"
               Margin="10, 0" />
    </StackLayout>
</ContentPage>
```

비트맵 리소스를 로드 하 고 사용 하는 코드 숨김 파일은 `SKCanvasView` 표시 합니다. 비트맵 변경 되지 않습니다. `SelectedIndexChanged` 에 대 한 처리기를 `Picker` 열거형 멤버와 같은 확장명을 가진 파일을 수정 합니다.

```csharp
public partial class SaveFileFormatsPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(typeof(SaveFileFormatsPage),
        "SkiaSharpFormsDemos.Media.MonkeyFace.png");

    public SaveFileFormatsPage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        args.Surface.Canvas.DrawBitmap(bitmap, args.Info.Rect, BitmapStretch.Uniform);
    }

    void OnFormatPickerChanged(object sender, EventArgs args)
    {
        if (formatPicker.SelectedIndex != -1)
        {
            SKEncodedImageFormat imageFormat = (SKEncodedImageFormat)formatPicker.SelectedItem;
            fileNameEntry.Text = Path.ChangeExtension(fileNameEntry.Text, imageFormat.ToString());
            statusLabel.Text = "OK";
        }
    }

    async void OnButtonClicked(object sender, EventArgs args)
    {
        SKEncodedImageFormat imageFormat = (SKEncodedImageFormat)formatPicker.SelectedItem;
        int quality = (int)qualitySlider.Value;

        using (MemoryStream memStream = new MemoryStream())
        using (SKManagedWStream wstream = new SKManagedWStream(memStream))
        {
            bitmap.Encode(wstream, imageFormat, quality);
            byte[] data = memStream.ToArray();

            if (data == null)
            {
                statusLabel.Text = "Encode returned null";
            }
            else if (data.Length == 0)
            {
                statusLabel.Text = "Encode returned empty array";
            }
            else
            {
                bool success = await DependencyService.Get<IPhotoLibrary>().
                    SavePhotoAsync(data, folderNameEntry.Text, fileNameEntry.Text);

                if (!success)
                {
                    statusLabel.Text = "SavePhotoAsync return false";
                }
                else
                {
                    statusLabel.Text = "Success!";
                }
            }
        }
    }
}
```

합니다 `Clicked` 에 대 한 처리기를 `Button` 모든 실제 작동 합니다. 에 대 한 두 개의 인수를 가져와서 `Encode` 에서 합니다 `Picker` 및 `Slider`, 다음 만들려면 앞서 살펴본 코드를 사용 하 여는 `SKManagedWStream` 에 대 한는 `Encode` 메서드. 두 개의 `Entry` 보기에 대 한 폴더와 파일 이름을 제공 합니다 `SavePhotoAsync` 메서드.

이 메서드는 대부분 할애 되는데 문제 또는 오류를 처리 합니다. 경우 `Encode` 빈 배열을 만듭니다. 즉, 특정 파일 형식은 지원 되지 않습니다. 하는 경우 `SavePhotoAsync` 반환 `false`, 다음 파일을 성공적으로 저장 되지 않았습니다. 

세 가지 플랫폼에서 실행 중인 프로그램이 다음과 같습니다.

[![파일 형식 저장](saving-images/SaveFileFormats.png "파일 형식 저장")](saving-images/SaveFileFormats-Large.png#lightbox)

스크린샷에 이러한 플랫폼에서 지원 되는 세 개의 형식을 보여 줍니다.

- JPEG
- PNG
- WebP

다른 모든 형식에 대해는 `Encode` 메서드 nothing 스트림에 쓰고 결과 바이트 배열이 비어 있는 것입니다.

비트맵은를 **파일 형식은 저장** 페이지 저장은 600 픽셀 사각형이 됩니다. 픽셀당 4 바이트를 사용 하 여 총 메모리에 1,440,000 바이트입니다. 다음 표에서 파일 형식 및 품질의 다양 한 조합에 대 한 파일 크기를 보여 줍니다.

|형식|품질|크기|
|------|------:|---:|
| PNG | N/A | 492 K |
| JPEG | 0 | 2.95 K |
|      | 50 | 22.1 K |
|      | 100 | 206 K |
| WebP | 0 | 2.71 K |
|      | 50 | 11.9 K |
|      | 100 | 101 K |

다양 한 품질 설정으로 실험 하는 결과 검토할 수 있습니다.

## <a name="saving-finger-paint-art"></a>저장 finger-paint 아트

비트맵의 일반적 용도 중 하나는 그리기 프로그램 라는 대로 작동 하는지는 _섀도 비트맵_합니다. 모든 그리기 프로그램에 의해 표시 되는 비트맵에 유지 됩니다. 비트맵도 유용 그리기를 저장 합니다.

합니다 [ **SkiaSharp에서 손가락 페인팅** ](../paths/finger-paint.md) 문서 추적 기본 손가락 프로그램 구현에서 터치를 사용 하는 방법을 설명 합니다. 하나의 색 및 하나의 스트로크 너비 프로그램 지원 되지만 컬렉션의 전체 그리기 유지 `SKPath` 개체입니다.

합니다 **손가락으로 그리기와 저장** 페이지에 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 샘플은 또한 컬렉션의 전체 그리기 유지 `SKPath` 개체 하지만 또한 사진 라이브러리에 저장할 수 있는 비트맵에 드로잉을 렌더링 합니다.

이 프로그램의 대부분은 원래 비슷합니다 **손가락으로 그리기** 프로그램입니다. 개선 사항 중 하나는 XAML 파일에 이제 라는 레이블이 있는 단추가 인스턴스화합니다 **명확한** 하 고 **저장**:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Bitmaps.FingerPaintSavePage"
             Title="Finger Paint Save">

    <StackLayout>
        <Grid BackgroundColor="White"
              VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="canvasView"
                               PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>

        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
        </Grid>

        <Button Text="Clear"
                Grid.Row="0"
                Margin="50, 5"
                Clicked="OnClearButtonClicked" />

        <Button Text="Save"
                Grid.Row="1"
                Margin="50, 5"
                Clicked="OnSaveButtonClicked" />

    </StackLayout>
</ContentPage>
```

코드 숨김 파일 형식의 필드를 유지 `SKBitmap` 라는 `saveBitmap`합니다. 이 비트맵을 생성 또는 다시는 `PaintSurface` 디스플레이의 크기를 화면 변경 될 때마다 처리기입니다. 비트맵을 만들어야 하는 경우 모든 화면 크기에서 변경 하는 방법에 관계 없이 유지 됩니다 있도록 기존 비트맵 이미지의 내용은 새 비트맵에 복사 됩니다.

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    ···
    SKBitmap saveBitmap;

    public FingerPaintSavePage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        // Create bitmap the size of the display surface
        if (saveBitmap == null)
        {
            saveBitmap = new SKBitmap(info.Width, info.Height);
        }
        // Or create new bitmap for a new size of display surface
        else if (saveBitmap.Width < info.Width || saveBitmap.Height < info.Height)
        {
            SKBitmap newBitmap = new SKBitmap(Math.Max(saveBitmap.Width, info.Width),
                                              Math.Max(saveBitmap.Height, info.Height));

            using (SKCanvas newCanvas = new SKCanvas(newBitmap))
            {
                newCanvas.Clear();
                newCanvas.DrawBitmap(saveBitmap, 0, 0);
            }

            saveBitmap = newBitmap;
        }

        // Render the bitmap
        canvas.Clear();
        canvas.DrawBitmap(saveBitmap, 0, 0);
    }
    ···
}
```

수행한 그리기를 `PaintSurface` 처리기 맨 끝에서 발생 하 고 으로만 이루어져 비트맵을 렌더링 합니다.

터치를 처리 하는 이전 프로그램와 비슷합니다. 프로그램에는 두 개의 컬렉션 유지 관리 `inProgressPaths` 및 `completedPaths`를 포함 하는 모든 표시의 선택이 취소 된 마지막 시간 이후 사용자가 그린 합니다. 각 터치 이벤트에 대 한 합니다 `OnTouchEffectAction` 처리기 호출 `UpdateBitmap`:

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    Dictionary<long, SKPath> inProgressPaths = new Dictionary<long, SKPath>();
    List<SKPath> completedPaths = new List<SKPath>();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };
    ···
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = new SKPath();
                    path.MoveTo(ConvertToPixel(args.Location));
                    inProgressPaths.Add(args.Id, path);
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Moved:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = inProgressPaths[args.Id];
                    path.LineTo(ConvertToPixel(args.Location));
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Released:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    completedPaths.Add(inProgressPaths[args.Id]);
                    inProgressPaths.Remove(args.Id);
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Cancelled:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    inProgressPaths.Remove(args.Id);
                    UpdateBitmap();
                }
                break;
        }
    }

    SKPoint ConvertToPixel(Point pt)
    {
        return new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                            (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));
    }

    void UpdateBitmap()
    {
        using (SKCanvas saveBitmapCanvas = new SKCanvas(saveBitmap))
        {
            saveBitmapCanvas.Clear();

            foreach (SKPath path in completedPaths)
            {
                saveBitmapCanvas.DrawPath(path, paint);
            }

            foreach (SKPath path in inProgressPaths.Values)
            {
                saveBitmapCanvas.DrawPath(path, paint);
            }
        }

        canvasView.InvalidateSurface();
    }
    ···
}
```

합니다 `UpdateBitmap` 메서드 다시 그리기 횟수 `saveBitmap` 새 `SKCanvas`를 지운 다음 비트맵의 모든 경로 렌더링 합니다. 무효화 하 여 마지막으로 `canvasView` 디스플레이에서 비트맵을 그릴 수 있도록 합니다.

두 개의 단추에 대 한 처리기는 다음과 같습니다. 합니다 **지우기** 단추 두 경로 컬렉션을 업데이트를 지운 `saveBitmap` (결과 비트맵을 선택 취소)을 무효화 하 고는 `SKCanvasView`:

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    ···
    void OnClearButtonClicked(object sender, EventArgs args)
    {
        completedPaths.Clear();
        inProgressPaths.Clear();
        UpdateBitmap();
        canvasView.InvalidateSurface();
    }

    async void OnSaveButtonClicked(object sender, EventArgs args)
    {
        using (SKImage image = SKImage.FromBitmap(saveBitmap))
        {
            SKData data = image.Encode();
            DateTime dt = DateTime.Now;
            string filename = String.Format("FingerPaint-{0:D4}{1:D2}{2:D2}-{3:D2}{4:D2}{5:D2}{6:D3}.png",
                                            dt.Year, dt.Month, dt.Day, dt.Hour, dt.Minute, dt.Second, dt.Millisecond);

            IPhotoLibrary photoLibrary = DependencyService.Get<IPhotoLibrary>();
            bool result = await photoLibrary.SavePhotoAsync(data.ToArray(), "FingerPaint", filename);

            if (!result)
            {
                await DisplayAlert("FingerPaint", "Artwork could not be saved. Sorry!", "OK");
            }
        }
    }
}
```

합니다 **저장** 단추 처리기를 사용 하 여 단순한 [ `Encode` ](xref:SkiaSharp.SKImage.Encode) 메서드에서 `SKImage`합니다. 이 메서드는 PNG 형식으로 인코딩합니다. 합니다 `SKImage` 개체를 기반으로 생성 됩니다 `saveBitmap`, 및 `SKData` 인코딩된 PNG 파일을 포함 하는 개체입니다. 

합니다 `ToArray` 메서드의 `SKData` 바이트의 배열을 가져옵니다. 에 전달 되는이 `SavePhotoAsync` 고정된 폴더 이름 및 현재 날짜 및 시간에서 생성 된 고유한 파일 이름을 함께 메서드.

작업에서 프로그램이 다음과 같습니다.

[![저장 그리기 손가락](saving-images/FingerPaintSave.png "저장 그리기 손가락으로")](saving-images/FingerPaintSave-Large.png#lightbox)

매우 유사한 기술을 합니다 [ **SpinPaint** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SpinPaint/) 샘플입니다. 그런 다음 해당 다른 4 개의 사분면에 디자인을 재현 하는 회전 디스크에 그리는 사용자 한다는 손가락 프로그램 이기도 합니다. 디스크로 손가락 그리기 변경의 색이 회전 합니다.

[![그리기를 스핀업](saving-images/SpinPaint.png "그리기를 실행 합니다.")](saving-images/SpinPaint-Large.png#lightbox)

합니다 **저장** 단추의 `SpinPaint` 와 비슷하지만 **손가락으로 그리기** 고정된 폴더 이름에 해당 이미지를 저장 하는 (**SpainPaint**)에서 생성 된 파일 이름 및 날짜 및 시간입니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [SpinPaint (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SpinPaint/)
