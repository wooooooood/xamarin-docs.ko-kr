---
title: ''
description: ''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 01f4fcf1953658af44d2a8996913860a3b605abf
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138660"
---
# <a name="saving-skiasharp-bitmaps-to-files"></a>SkiaSharp 비트맵을 파일에 저장

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

SkiaSharp 응용 프로그램에서 비트맵을 만들거나 수정한 후에는 응용 프로그램에서 사용자의 사진 라이브러리에 비트맵을 저장 하려고 할 수 있습니다.

![비트맵 저장](saving-images/SavingSample.png "비트맵 저장")

이 작업에는 두 단계가 포함 됩니다.

- SkiaSharp 비트맵을 JPEG 또는 PNG와 같은 특정 파일 형식의 데이터로 변환 합니다.
- 플랫폼별 코드를 사용 하 여 사진 라이브러리에 결과를 저장 합니다.

## <a name="file-formats-and-codecs"></a>파일 형식 및 코덱

최신의 인기 있는 비트맵 파일 형식은 압축을 사용 하 여 저장소 공간을 줄입니다. 압축 기술 중 두 가지 광범위 한 범주를 _손실_ 및 _무손실_이라고 합니다. 이러한 용어는 압축 알고리즘으로 인해 데이터가 손실 될 수 있는지 여부를 나타냅니다.

가장 인기 있는 손실 형식은 공동 사진 전문가 그룹에 의해 개발 되었으며이를 JPEG 라고 합니다. JPEG 압축 알고리즘은 _불연속 코사인 변환_이라는 수학 도구를 사용 하 여 이미지를 분석 하 고 이미지의 시각적 충실도를 유지 하기 위해 중요 하지 않은 데이터를 제거 하려고 합니다. 압축 수준은 일반적으로 _품질_이라고 하는 설정을 사용 하 여 제어할 수 있습니다. 품질 설정이 높으면 파일이 커집니다.

이와 대조적으로 무손실 압축 알고리즘은 데이터를 줄이는 방식으로 인코딩할 수 있지만 정보의 손실을 초래 하지 않는 반복 및 픽셀 패턴에 대 한 이미지를 분석 합니다. 원래 비트맵 데이터는 압축 된 파일에서 완전히 복원할 수 있습니다. 현재 사용 중인 기본 무손실 압축 파일 형식은 PNG (이동식 네트워크 그래픽)입니다.

일반적으로 JPEG는 사진에 사용 되는 반면 PNG는 수동으로 또는 알고리즘 방식으로 생성 된 이미지에 사용 됩니다. 일부 파일의 크기를 줄이는 무손실 압축 알고리즘은 반드시 다른 파일의 크기를 늘려야 합니다. 다행히 이러한 크기 증가는 일반적으로 무작위 또는 무작위 정보를 많이 포함 하는 데이터에 대해서만 발생 합니다.

압축 알고리즘은 압축 및 압축 풀기 프로세스를 설명 하는 두 가지 용어를 보증 하기에 충분히 복잡 합니다.

- _디코드_ &mdash; 비트맵 파일 형식 읽기 및 압축 풀기
- _인코드_ &mdash; 비트맵 압축 및 비트맵 파일 형식에 쓰기

클래스에는 [`SKBitmap`](xref:SkiaSharp.SKBitmap) `Decode` 압축 된 소스에서를 만드는 라는 여러 메서드가 포함 되어 있습니다 `SKBitmap` . 파일 이름, 스트림 또는 바이트 배열을 제공 해야 합니다. 디코더는 파일 형식을 확인 하 고 적절 한 내부 디코딩 함수로 전달할 수 있습니다.

또한 [`SKCodec`](xref:SkiaSharp.SKCodec) 클래스에 `Create` 는 압축 된 `SKCodec` 소스에서 개체를 만들고 응용 프로그램에서 디코딩 프로세스에 더 많은 관련 작업을 수행할 수 있도록 하는 라는 두 개의 메서드가 있습니다. `SKCodec`이 클래스는 애니메이션 GIF 파일 디코딩에 [**SkiaSharp 비트맵**](animating.md#gif-animation) 의 연결에 애니메이션 적용 문서에 나와 있습니다.

비트맵을 인코딩할 때 추가 정보가 필요 합니다. 인코더는 응용 프로그램에서 사용 하려는 특정 파일 형식 (JPEG 또는 PNG 또는 기타)을 알고 있어야 합니다. 손실 형식이 필요한 경우 인코딩에도 원하는 품질 수준을 알아야 합니다.

`SKBitmap`클래스는 [`Encode`](xref:SkiaSharp.SKBitmap.Encode(SkiaSharp.SKWStream,SkiaSharp.SKEncodedImageFormat,System.Int32)) 다음 구문을 사용 하 여 하나의 메서드를 정의 합니다.

```csharp
public Boolean Encode (SKWStream dst, SKEncodedImageFormat format, Int32 quality)
```

이 방법은 잠시 자세히 설명 되어 있습니다. 인코딩된 비트맵이 쓰기 가능한 스트림에 기록 됩니다. 의 ' W '는 `SKWStream` "쓰기 가능"을 의미 합니다. 두 번째 및 세 번째 인수는 0에서 100 사이의 원하는 품질의 파일 형식 및 (손실 형식에 대 한)를 지정 합니다.

또한 [`SKImage`](xref:SkiaSharp.SKImage) 및 [`SKPixmap`](xref:SkiaSharp.SKPixmap) 클래스는 `Encode` 보다 다양 하 고 원하는 메서드를 정의 합니다. `SKImage` `SKBitmap` 정적 메서드를 사용 하 여 개체에서 개체를 쉽게 만들 수 있습니다 [`SKImage.FromBitmap`](xref:SkiaSharp.SKImage.FromBitmap(SkiaSharp.SKBitmap)) . `SKPixmap` `SKBitmap` 메서드를 사용 하 여 개체에서 개체를 가져올 수 있습니다 [`PeekPixels`](xref:SkiaSharp.SKBitmap.PeekPixels) .

[`Encode`](xref:SkiaSharp.SKImage.Encode)로 정의 된 메서드 중 하나 `SKImage` 에는 매개 변수가 없으며 PNG 형식에 자동으로 저장 됩니다. 이 매개 변수가 없는 메서드는 매우 쉽게 사용할 수 있습니다.

## <a name="platform-specific-code-for-saving-bitmap-files"></a>비트맵 파일을 저장 하기 위한 플랫폼 특정 코드

`SKBitmap`개체를 특정 파일 형식으로 인코딩하면 일반적으로 일부 정렬 또는 데이터 배열에 대 한 스트림 개체를 사용할 수 있습니다. 일부 `Encode` 메서드 (에 의해 정의 된 매개 변수가 없는 메서드 포함 `SKImage` )는 [`SKData`](xref:SkiaSharp.SKData) 메서드를 사용 하 여 바이트 배열로 변환 될 수 있는 개체를 반환 [`ToArray`](xref:SkiaSharp.SKData.ToArray) 합니다. 그런 다음이 데이터를 파일에 저장 해야 합니다.

이 작업에 표준 클래스와 메서드를 사용할 수 있기 때문에 응용 프로그램 로컬 저장소의 파일에 저장 하는 작업은 매우 간단 `System.IO` 합니다. 이 기법은 만델브로트 집합의 일련의 비트맵에 애니메이션 효과를 주는 연결 [**SkiaSharp 비트맵에 애니메이션을 적용**](animating.md#bitmap-animation) 하는 문서에 나와 있습니다.

다른 응용 프로그램에서 파일을 공유 하려면 해당 파일을 사용자의 사진 라이브러리에 저장 해야 합니다. 이 작업을 수행 하려면 플랫폼별 코드와를 사용 해야 합니다 Xamarin.Forms [`DependencyService`](xref:Xamarin.Forms.DependencyService) .

[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 응용 프로그램의 **SkiaSharpFormsDemo** 프로젝트는 `IPhotoLibrary` 클래스와 함께 사용 되는 인터페이스를 정의 합니다 `DependencyService` . 다음은 메서드 구문을 정의 합니다 `SavePhotoAsync` .

```csharp
public interface IPhotoLibrary
{
    Task<Stream> PickPhotoAsync();

    Task<bool> SavePhotoAsync(byte[] data, string folder, string filename);
}
```

또한이 인터페이스는 `PickPhotoAsync` 장치 사진 라이브러리의 플랫폼별 파일 선택기를 여는 데 사용 되는 메서드를 정의 합니다.

의 경우 `SavePhotoAsync` 첫 번째 인수는 JPEG 또는 PNG와 같이 특정 파일 형식으로 이미 인코드된 비트맵을 포함 하는 바이트 배열입니다. 응용 프로그램은 자신이 만든 모든 비트맵을 다음 매개 변수에 지정 된 다음 파일 이름에 지정 된 특정 폴더에 격리할 수 있습니다. 메서드는 성공 여부를 나타내는 부울을 반환 합니다.

다음 섹션에서는 `SavePhotoAsync` 각 플랫폼에서가 구현 되는 방식에 대해 설명 합니다.

### <a name="the-ios-implementation"></a>IOS 구현

의 iOS 구현에서는 `SavePhotoAsync` 의 메서드를 사용 합니다 [`SaveToPhotosAlbum`](xref:UIKit.UIImage.SaveToPhotosAlbum*) `UIImage` .

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

그러나 이미지에 대 한 파일 이름 또는 폴더를 지정할 수 있는 방법은 없습니다.

IOS 프로젝트의 **info.plist** 파일에는 사진 라이브러리에 이미지를 추가 함을 나타내는 키가 필요 합니다.

```xml
<key>NSPhotoLibraryAddUsageDescription</key>
<string>SkiaSharp Forms Demos adds images to your photo library</string>
```

조심하세요! 단순히 사진 라이브러리에 액세스 하기 위한 권한 키가 매우 유사 하지만 동일 하지는 않습니다.

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>SkiaSharp Forms Demos accesses your photo library</string>
```

### <a name="the-android-implementation"></a>Android 구현

의 Android 구현은 `SavePhotoAsync` 먼저 `folder` 인수가 인지 `null` 아니면 빈 문자열 인지를 확인 합니다. 이 경우 비트맵이 사진 라이브러리의 루트 디렉터리에 저장 됩니다. 그렇지 않으면 폴더가 생성 되 고, 존재 하지 않는 경우 생성 됩니다.

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

에 대 한 호출은 반드시 `MediaScannerConnection.ScanFile` 필요한 것은 아니지만, 사진 라이브러리를 즉시 확인 하 여 프로그램을 테스트 하는 경우 라이브러리 갤러리 보기를 업데이트 하면 많은 도움이 됩니다.

**Androidmanifest .xml** 파일에는 다음 사용 권한 태그가 필요 합니다.

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

### <a name="the-uwp-implementation"></a>UWP 구현

의 UWP 구현은 `SavePhotoAsync` Android 구현과 구조가 매우 유사 합니다.

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

**Appxmanifest.xml** 파일의 **기능** 섹션에는 **그림 라이브러리가**필요 합니다.

## <a name="exploring-the-image-formats"></a>이미지 형식 탐색

[`Encode`](xref:SkiaSharp.SKBitmap.Encode(SkiaSharp.SKWStream,SkiaSharp.SKEncodedImageFormat,System.Int32))의 메서드는 `SKImage` 다음과 같습니다.

```csharp
public Boolean Encode (SKWStream dst, SKEncodedImageFormat format, Int32 quality)
```

[`SKEncodedImageFormat`](xref:SkiaSharp.SKEncodedImageFormat)는 11 가지 비트맵 파일 형식을 참조 하는 멤버를 포함 하는 열거형 이며, 그 중 일부는 모호 하지 않습니다.

- `Astc`&mdash;적응 확장 가능한 질감 압축
- `Bmp`&mdash;Windows 비트맵
- `Dng`&mdash;Adobe Digital 부정
- `Gif`&mdash;그래픽 교환 형식
- `Ico`&mdash;Windows 아이콘 이미지
- `Jpeg`&mdash;공동 사진 전문가 그룹
- `Ktx`&mdash;OpenGL의 Khronos 질감 형식
- `Pkm`&mdash;Pokémon 파일 저장
- `Png`&mdash;휴대용 네트워크 그래픽
- `Wbmp`&mdash;무선 응용 프로그램 프로토콜 비트맵 형식 (픽셀당 1 비트)
- `Webp`&mdash;Google WebP 형식

곧 표시 되는 것 처럼 이러한 파일 형식 ( `Jpeg` , 및) 중 세 개만 `Png` `Webp` 실제로 SkiaSharp에서 지원 됩니다.

`SKBitmap`이라는 개체를 `bitmap` 사용자의 사진 라이브러리에 저장 하려면 열거형의 멤버 `SKEncodedImageFormat` `imageFormat` (손실 형식)와 정수 변수를 함께 사용 해야 `quality` 합니다. 다음 코드를 사용 하 여 폴더에 있는 이름의 파일에 비트맵을 저장할 수 있습니다 `filename` `folder` .

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

`SKManagedWStream`클래스가에서 파생 `SKWStream` 되는 경우 ("쓰기 가능 스트림"을 나타냄) `Encode`메서드는 인코딩된 비트맵 파일을 해당 스트림에 씁니다. 해당 코드의 주석은 수행 해야 할 수 있는 몇 가지 오류 검사를 나타냅니다.

[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 응용 프로그램의 **파일 형식 저장** 페이지는 비슷한 코드를 사용 하 여 다양 한 형식으로 비트맵을 저장 하는 방법을 시험해 볼 수 있습니다.

XAML 파일에는 비트맵을 표시 하는가 포함 되어 있고 `SKCanvasView` , 페이지의 나머지 부분에는 응용 프로그램에서의 메서드를 호출 하는 데 필요한 모든 것이 포함 되어 있습니다 `Encode` `SKBitmap` . 이 클래스 `Picker` 에는 열거형의 멤버 `SKEncodedImageFormat` , `Slider` 손실 비트맵 형식의 품질 인수에 대 한, `Entry` 파일 이름 및 폴더 이름에 대 한 두 개의 뷰, 파일을 `Button` 저장 하기 위한가 있습니다.

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

코드 숨김이 파일은 비트맵 리소스를 로드 하 고를 사용 하 여 `SKCanvasView` 표시 합니다. 이 비트맵은 변경 되지 않습니다. `SelectedIndexChanged`에 대 한 처리기는 `Picker` 열거형 멤버와 동일한 확장명을 사용 하 여 파일 이름을 수정 합니다.

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

`Clicked`에 대 한 처리기는 `Button` 모든 실제 작업을 수행 합니다. 및에서에 대 한 두 개의 인수를 가져온 `Encode` `Picker` `Slider` 다음 앞에 표시 된 코드를 사용 하 여 `SKManagedWStream` 메서드에 대 한를 만듭니다 `Encode` . 두 `Entry` 뷰는 메서드에 대 한 폴더 및 파일 이름을 제공할 것 `SavePhotoAsync` 입니다.

이 방법의 대부분은 문제 또는 오류 처리를 위한 것입니다. 가 `Encode` 빈 배열을 만드는 경우 특정 파일 형식이 지원 되지 않음을 의미 합니다. `SavePhotoAsync` `false` 가를 반환 하면 파일이 성공적으로 저장 되지 않은 것입니다.

실행 되는 프로그램은 다음과 같습니다.

[![파일 형식 저장](saving-images/SaveFileFormats.png "파일 형식 저장")](saving-images/SaveFileFormats-Large.png#lightbox)

이 스크린샷에서는 이러한 플랫폼에서 지원 되는 다음 세 가지 형식만 보여 줍니다.

- JPEG
- PNG
- WebP

다른 모든 형식의 경우 `Encode` 메서드는 아무것도 스트림에 쓰지 않고 결과 바이트 배열이 비어 있습니다.

**파일 형식 저장** 페이지에서 저장 하는 비트맵은 600 픽셀 사각형입니다. 픽셀당 4 바이트를 사용 하 여 메모리에서 총 144만 바이트입니다. 다음 표에서는 다양 한 파일 형식 및 품질 조합의 파일 크기를 보여 줍니다.

|서식|품질|Size|
|---
제목: 설명: ms. prod: ms. 기술: assetid: author: ms author: ms. date: no loc:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---|---제목: 설명: ms. prod: assetid: author: ms author: ms. date: no loc:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---:|---:| | PNG | 해당 없음 | 492K | | JPEG | 0 | 2.95 k | |      | 50 | 22.1 k | |      | 100 | 206K | | WebP | 0 | 2.71 k | |      | 50 | 11.9 k | |      | 100 | 101K |

다양 한 품질 설정으로 실험 하 고 결과를 검토할 수 있습니다.

## <a name="saving-finger-paint-art"></a>손가락 저장-그리기 아트

비트맵의 일반적인 용도 중 하나는 프로그램 그리기에서 _그림자 비트맵_이라고 하는 것입니다. 모든 그리기는 비트맵에 유지 되며이는 프로그램에 의해 표시 됩니다. 비트맵은 그리기를 저장 하는 데에도 유용 합니다.

[**SkiaSharp에서 손가락 그리기**](../paths/finger-paint.md) 문서는 터치 추적을 사용 하 여 기본 손가락 그리기 프로그램을 구현 하는 방법을 보여 주었습니다. 프로그램은 한 가지 색과 하나의 스트로크 너비만 지원 하지만 개체의 컬렉션에 전체 그리기를 유지 합니다 `SKPath` .

[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플의 **저장을 사용 하 여 손가락으로** 그리기는 개체의 컬렉션에도 전체 그리기를 유지 `SKPath` 하지만 비트맵에서 그리기를 렌더링 하 여 사진 라이브러리에 저장할 수 있습니다.

이 프로그램의 대부분은 원래 **손가락 그리기** 프로그램과 유사 합니다. 한 가지 향상 된 기능은 이제 XAML 파일이 **Clear** 및 **Save**라는 레이블이 지정 된 단추를 인스턴스화하는 것입니다.

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

코드 숨김이 파일은 라는 형식의 필드를 유지 관리 `SKBitmap` 합니다 `saveBitmap` . 이 비트맵은 `PaintSurface` 디스플레이 표면의 크기가 변경 될 때마다 처리기에서 만들어지거나 다시 만들어집니다. 비트맵을 다시 만들어야 하는 경우 표시 표면의 크기가 변경 되는 방식에 관계 없이 모든 항목이 유지 되도록 기존 비트맵의 내용이 새 비트맵으로 복사 됩니다.

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

처리기에서 수행 하는 그리기는 `PaintSurface` 매우 끝에서 발생 하며 비트맵 렌더링 으로만 구성 됩니다.

터치 처리는 이전 프로그램과 유사 합니다. 프로그램은를 `inProgressPaths` 마지막으로 표시 한 `completedPaths` 후 사용자가 그린 모든 항목을 포함 하는 두 개의 컬렉션 및를 유지 관리 합니다. 각 터치 이벤트에 대해 `OnTouchEffectAction` 처리기는 다음을 호출 합니다 `UpdateBitmap` .

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

`UpdateBitmap` `saveBitmap` 새을 만들고 `SKCanvas` 지운 다음 비트맵의 모든 경로를 렌더링 하 여 메서드를 다시 그립니다. `canvasView`표시에 비트맵을 그릴 수 있도록 무효화로 결론을 마칩니다.

다음은 두 단추에 대 한 처리기입니다. **지우기** 단추를 클릭 하면 경로 컬렉션, 업데이트 `saveBitmap` (비트맵이 지워집니다)가 모두 지워지고이 무효화 됩니다 `SKCanvasView` .

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

**저장** 단추 처리기는의 간소화 된 메서드를 사용 합니다 [`Encode`](xref:SkiaSharp.SKImage.Encode) `SKImage` . 이 메서드는 PNG 형식을 사용 하 여 인코딩합니다. `SKImage`개체는을 기반으로 만들어지고 `saveBitmap` `SKData` 개체는 인코딩된 PNG 파일을 포함 합니다.

`ToArray`의 메서드는 `SKData` 바이트 배열을 가져옵니다. 이는 `SavePhotoAsync` 고정 폴더 이름과 현재 날짜 및 시간으로 생성 된 고유 파일 이름과 함께 메서드에 전달 됩니다.

실행 중인 프로그램은 다음과 같습니다.

[![손가락 그리기 저장](saving-images/FingerPaintSave.png "손가락 그리기 저장")](saving-images/FingerPaintSave-Large.png#lightbox)

[**SpinPaint**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-spinpaint) 샘플에서 매우 유사한 기법을 사용 합니다. 또한 손가락 그리기 프로그램은 사용자가 회전 하는 디스크를 페인트 한 후 다른 4 개의 사분면에서 디자인을 재현 한다는 점을 제외 하 고는 손가락 그리기 프로그램입니다. 디스크가 회전 하는 동안 손가락 페인트의 색이 변경 됩니다.

[![페인트 회전](saving-images/SpinPaint.png "페인트 회전")](saving-images/SpinPaint-Large.png#lightbox)

클래스의 **저장** 단추는 `SpinPaint` 이미지를 고정 폴더 이름 (**SpainPaint**)에 저장 하 고 날짜 및 시간으로 생성 된 파일 이름을 저장 한다는 점에서 **손가락 그리기** 와 비슷합니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [SpinPaint (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-spinpaint)
