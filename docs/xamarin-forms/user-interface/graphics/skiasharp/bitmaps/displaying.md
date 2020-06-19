---
title: SkiaSharp 비트맵 표시
description: 가로 세로 비율을 유지 하면서 SkiaSharp 비트맵을 픽셀 크기로 표시 하 고 사각형을 채우도록 확장 하는 방법을 알아봅니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8E074F8D-4715-4146-8CC0-FD7A8290EDE9
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b0fecac02c44e3ba721dd4625e7bee264c6869d0
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84134760"
---
# <a name="displaying-skiasharp-bitmaps"></a>SkiaSharp 비트맵 표시

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

SkiaSharp 비트맵의 제목은 **[SkiaSharp의 비트맵 기본 사항](../basics/bitmaps.md)** 문서에서 도입 되었습니다. 이 문서에서는 비트맵을 로드 하는 세 가지 방법 및 비트맵을 표시 하는 세 가지 방법을 보여 주었습니다. 이 문서에서는 비트맵을 로드 하는 방법을 검토 하 고의 메서드 사용에 `DrawBitmap` 대해 자세히 설명 `SKCanvas` 합니다.

![샘플 표시](displaying-images/DisplayingSample.png "샘플 표시")

`DrawBitmapLattice`및 메서드에 대 한 자세한 내용은 `DrawBitmapNinePatch` **[SkiaSharp 비트맵의 분할](segmented.md)** 된 항목 표시를 참조 하세요.

이 페이지에 대 한 샘플은 **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** 응용 프로그램에서 가져온 것입니다. 해당 응용 프로그램의 홈 페이지에서 **SkiaSharp 비트맵**을 선택 하 고 **비트맵 표시** 섹션으로 이동 합니다.

## <a name="loading-a-bitmap"></a>비트맵 로드

SkiaSharp 응용 프로그램에서 사용 하는 비트맵은 일반적으로 다음과 같은 세 가지 원본 중 하나에서 제공 됩니다.

- 인터넷을 통해
- 실행 파일에 포함 된 리소스에서
- 사용자의 사진 라이브러리에서

SkiaSharp 응용 프로그램에서 새 비트맵을 만든 다음이를 그리거나 비트맵 비트 알고리즘 방식으로를 설정할 수도 있습니다. 이러한 기술은 **[SkiaSharp 비트맵에 대 한 만들기 및 그리기](drawing.md)** 및 **[SkiaSharp 비트맵 픽셀 액세스](pixel-bits.md)** 문서에서 설명 합니다.

비트맵을 로드 하는 다음 세 가지 코드 예제에서 클래스는 형식의 필드를 포함 하는 것으로 간주 됩니다 `SKBitmap` .

```csharp
SKBitmap bitmap;
```

**[SkiaSharp의 비트맵 기본 사항에](../basics/bitmaps.md)** 설명 된 것 처럼 인터넷을 통해 비트맵을 로드 하는 가장 좋은 방법은 클래스를 사용 하는 것입니다 [`HttpClient`](xref:System.Net.Http.HttpClient) . 클래스의 단일 인스턴스를 필드로 정의할 수 있습니다.

```csharp
HttpClient httpClient = new HttpClient();
```

`HttpClient`IOS 및 Android 응용 프로그램에서을 사용 하는 경우 **[TLS (전송 계층 보안) 1.2](~/cross-platform/app-fundamentals/transport-layer-security.md)** 문서에 설명 된 대로 프로젝트 속성을 설정 하는 것이 좋습니다.

를 사용 하는 코드는 `HttpClient` 대개 `await` 연산자를 포함 하므로 메서드에 있어야 합니다 `async` .

```csharp
try
{
    using (Stream stream = await httpClient.GetStreamAsync("https:// ··· "))
    using (MemoryStream memStream = new MemoryStream())
    {
        await stream.CopyToAsync(memStream);
        memStream.Seek(0, SeekOrigin.Begin);

        bitmap = SKBitmap.Decode(stream);
        ···
    };
}
catch
{
    ···
}
```

`Stream`에서 가져온 개체가로 복사 되었는지 확인 `GetStreamAsync` `MemoryStream` 합니다. Android에서는 비동기 메서드를 제외 하 고는 `Stream` `HttpClient` 주 스레드에서을 처리할 수 없습니다. 

는 [`SKBitmap.Decode`](xref:SkiaSharp.SKBitmap.Decode(System.IO.Stream)) 많은 작업을 수행 합니다. `Stream` 여기에 전달 된 개체는 일반적인 비트맵 파일 형식 (일반적으로 JPEG, PNG 또는 GIF) 중 하나에서 전체 비트맵을 포함 하는 메모리 블록을 참조 합니다. `Decode`메서드는 형식을 확인 한 다음 비트맵 파일을 SkiaSharp의 고유한 내부 비트맵 형식으로 디코딩합니다.

코드가를 호출 하면 `SKBitmap.Decode` `CanvasView` `PaintSurface` 처리기가 새로 로드 된 비트맵을 표시할 수 있도록이 무효화 될 수 있습니다.

비트맵을 로드 하는 두 번째 방법은 개별 플랫폼 프로젝트에서 참조 하는 .NET Standard 라이브러리에 비트맵을 포함 리소스로 포함 하는 것입니다. 리소스 ID는 메서드에 전달 됩니다 [`GetManifestResourceStream`](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) . 이 리소스 ID는 마침표로 구분 된 리소스의 어셈블리 이름, 폴더 이름 및 파일 이름으로 구성 됩니다.

```csharp
string resourceID = "assemblyName.folderName.fileName";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
{
    bitmap = SKBitmap.Decode(stream);
    ···
}
```

비트맵 파일은 iOS, Android 및 유니버설 Windows 플랫폼 (UWP) 용 개별 플랫폼 프로젝트에 리소스로 저장할 수도 있습니다. 그러나 이러한 비트맵을 로드 하려면 플랫폼 프로젝트에 있는 코드가 필요 합니다.

비트맵을 가져오는 세 번째 방법은 사용자의 그림 라이브러리를 사용 하는 것입니다. 다음 코드에서는 **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** 응용 프로그램에 포함 된 종속성 서비스를 사용 합니다. **SkiaSharpFormsDemo** .NET Standard 라이브러리는 `IPhotoLibrary` 인터페이스를 포함 하는 반면, 각 플랫폼 프로젝트는 `PhotoLibrary` 해당 인터페이스를 구현 하는 클래스를 포함 합니다.

```csharp
IPhotoicturePicker picturePicker = DependencyService.Get<IPhotoLibrary>();

using (Stream stream = await picturePicker.GetImageStreamAsync())
{
    if (stream != null)
    {
        bitmap = SKBitmap.Decode(stream);
        ···
    }
}
```

일반적으로 이러한 코드는 처리기가 `CanvasView` `PaintSurface` 새 비트맵을 표시할 수 있도록를 무효화 합니다.

`SKBitmap`클래스는 및를 비롯 하 여 [`Width`](xref:SkiaSharp.SKBitmap.Width) 비트맵의 픽셀 크기를 표시 하는 여러 가지 유용한 속성과 비트맵을 [`Height`](xref:SkiaSharp.SKBitmap.Height) 만들고 복사 하 고 픽셀 비트를 표시 하는 메서드를 비롯 한 여러 메서드를 정의 합니다. 

## <a name="displaying-in-pixel-dimensions"></a>픽셀 치수로 표시

SkiaSharp [`Canvas`](xref:SkiaSharp.SKCanvas) 클래스는 네 가지 메서드를 정의 합니다 `DrawBitmap` . 이러한 메서드를 사용 하면 기본적으로 서로 다른 두 가지 방법으로 비트맵을 표시할 수 있습니다. 

- `SKPoint`값 (또는 별도의 `x` 및 값)을 지정 하면 `y` 해당 픽셀 크기의 비트맵이 표시 됩니다. 비트맵의 픽셀은 비디오 디스플레이의 픽셀에 직접 매핑됩니다.
- 사각형을 지정 하면 비트맵이 사각형의 크기와 모양으로 늘어납니다. 

[`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKPoint,SkiaSharp.SKPaint)) `SKPoint` 매개 변수를 사용 하거나 [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,System.Single,System.Single,SkiaSharp.SKPaint)) 별도의 `x` 및 `y` 매개 변수를 사용 하 여의 픽셀 크기에 비트맵을 표시 합니다.

```csharp
DrawBitmap(SKBitmap bitmap, SKPoint pt, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, float x, float y, SKPaint paint = null)
```

이러한 두 메서드는 기능적으로 동일 합니다. 지정 된 위치는 캔버스를 기준으로 비트맵의 왼쪽 위 모퉁이 위치를 나타냅니다. 모바일 장치의 픽셀 해상도는 매우 높아서 이러한 장치에서는 일반적으로 작은 비트맵이 매우 작게 표시 됩니다.

선택적 `SKPaint` 매개 변수를 사용 하면 투명도를 사용 하 여 비트맵을 표시할 수 있습니다. 이렇게 하려면 개체를 만들고 `SKPaint` `Color` `SKColor` 알파 채널이 1 보다 작은 값으로 속성을 설정 합니다. 다음은 그 예입니다.

```csharp
paint.Color = new SKColor(0, 0, 0, 0x80);
```

마지막 인수로 전달 된 0x80은 50% 투명도를 나타냅니다. 미리 정의 된 색 중 하나에 알파 채널을 설정할 수도 있습니다.

```csharp
paint.Color = SKColors.Red.WithAlpha(0x80);
```

그러나 색 자체는 관련이 없습니다. 호출에서 개체를 사용 하는 경우 알파 채널만 검사 됩니다 `SKPaint` `DrawBitmap` .

`SKPaint`개체는 또한 혼합 모드 또는 필터 효과를 사용 하 여 비트맵을 표시할 때 역할을 수행 합니다. 이러한 항목은 [SkiaSharp 합성 및 blend 모드](../effects/blend-modes/index.md) 및 [SkiaSharp 이미지 필터](../effects/image-filters.md)문서에 나와 있습니다.

**[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** 샘플 프로그램의 **픽셀 차원** 페이지에는 240 픽셀의 320 픽셀 너비의 비트맵 리소스가 표시 됩니다.

```csharp
public class PixelDimensionsPage : ContentPage
{
    SKBitmap bitmap;

    public PixelDimensionsPage()
    {
        Title = "Pixel Dimensions";

        // Load the bitmap from a resource
        string resourceID = "SkiaSharpFormsDemos.Media.Banana.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }

        // Create the SKCanvasView and set the PaintSurface handler
        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        float x = (info.Width - bitmap.Width) / 2;
        float y = (info.Height - bitmap.Height) / 2;

        canvas.DrawBitmap(bitmap, x, y);
    }
}
```

`PaintSurface`처리기는 `x` `y` 표시 표면의 픽셀 크기와 비트맵의 픽셀 크기를 기준으로 및 값을 계산 하 여 비트맵을 가운데에 맞춥니다.

[![픽셀 크기](displaying-images/PixelDimensions.png "픽셀 크기")](displaying-images/PixelDimensions-Large.png#lightbox)

응용 프로그램에서 왼쪽 위 모퉁이에 비트맵을 표시 하려는 경우에는 (0, 0)의 좌표를 전달 하기만 하면 됩니다. 

## <a name="a-method-for-loading-resource-bitmaps"></a>리소스 비트맵을 로드 하는 방법

들어오는 샘플 대부분은 비트맵 리소스를 로드 해야 합니다. `BitmapExtensions` **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** 솔루션의 정적 클래스에는 다음을 수행 하는 데 도움이 되는 메서드가 포함 되어 있습니다.

```csharp
static class BitmapExtensions
{
    public static SKBitmap LoadBitmapResource(Type type, string resourceID)
    {
        Assembly assembly = type.GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            return SKBitmap.Decode(stream);
        }
    }
    ···
}
```

`Type`매개 변수를 확인 합니다. 이 `Type` 개체는 비트맵 리소스를 저장 하는 어셈블리의 모든 형식과 연결 된 개체 일 수 있습니다.

이 `LoadBitmapResource` 메서드는 비트맵 리소스가 필요한 모든 후속 샘플에서 사용 됩니다.

## <a name="stretching-to-fill-a-rectangle"></a>사각형을 채우도록 늘이기

`SKCanvas`또한 클래스는 비트맵을 [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKPaint)) 사각형으로 렌더링 하는 메서드와 [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint)) 비트맵의 사각형 하위 집합을 사각형으로 렌더링 하는 다른 메서드를 정의 합니다.

```
DrawBitmap(SKBitmap bitmap, SKRect dest, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, SKRect source, SKRect dest, SKPaint paint = null)
```

두 경우 모두 비트맵이 이라는 사각형을 채우도록 확장 됩니다 `dest` . 두 번째 방법에서는 사각형을 `source` 사용 하 여 비트맵의 하위 집합을 선택할 수 있습니다. `dest`사각형은 출력 장치를 기준으로 `source` 합니다. 사각형은 비트맵을 기준으로 합니다.

**사각형 채우기** 페이지는 이전 예제에서 캔버스와 동일한 크기의 사각형에 사용 되는 것과 동일한 비트맵을 표시 하 여 이러한 두 가지 방법 중 첫 번째를 보여 줍니다. 

```csharp
public class FillRectanglePage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public FillRectanglePage ()
    {
        Title = "Fill Rectangle";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.DrawBitmap(bitmap, info.Rect);
    }
}
```

새 메서드를 사용 하 여 `BitmapExtensions.LoadBitmapResource` 필드를 설정 하는 것을 확인 합니다 `SKBitmap` . 대상 사각형은 [`Rect`](xref:SkiaSharp.SKImageInfo.Rect) 표시 표면의 크기를 설명 합니다 하는의 속성에서 가져옵니다 `SKImageInfo` .

[![사각형 채우기](displaying-images/FillRectangle.png "사각형 채우기")](displaying-images/FillRectangle-Large.png#lightbox)

이는 일반적으로 원하는 항목이 _아닙니다_ . 이미지는 가로 및 세로 방향으로 다르게 늘이기 때문에 왜곡 됩니다. 비트맵을 픽셀 크기 이외의 항목으로 표시 하는 경우 일반적으로 비트맵의 원래 가로 세로 비율을 유지 하려고 합니다.

## <a name="stretching-while-preserving-the-aspect-ratio"></a>가로 세로 비율을 유지 하면서 늘이기

가로 세로 비율을 유지 하면서 비트맵을 확장 하는 것은 _균일 한 크기 조정_이 라고도 하는 프로세스입니다. 해당 용어는 알고리즘 방법을 제안 합니다. 한 가지 가능한 솔루션은 균일 한 **크기 조정** 페이지에 표시 됩니다.

```csharp
public class UniformScalingPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(UniformScalingPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public UniformScalingPage()
    {
        Title = "Uniform Scaling";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        float scale = Math.Min((float)info.Width / bitmap.Width, 
                               (float)info.Height / bitmap.Height);
        float x = (info.Width - scale * bitmap.Width) / 2;
        float y = (info.Height - scale * bitmap.Height) / 2;
        SKRect destRect = new SKRect(x, y, x + scale * bitmap.Width, 
                                           y + scale * bitmap.Height);

        canvas.DrawBitmap(bitmap, destRect);
    }
}
```

`PaintSurface`처리기는 `scale` 표시 너비와 높이의 최소 비율에 해당 하는 요소를 비트맵 너비와 높이에 대해 계산 합니다. `x` `y` 그러면 크기 조정 된 비트맵을 표시 너비와 높이 내에 맞추기 위해 및 값을 계산할 수 있습니다. 대상 사각형에는 및의 왼쪽 위 모퉁이가 있고 `x` `y` 해당 값의 오른쪽 아래 모퉁이와 비트맵의 배율 및 높이를 더한 값이 있습니다.

[![균일 한 크기 조정](displaying-images/UniformScaling.png "균일 한 크기 조정")](displaying-images/UniformScaling-Large.png#lightbox)

휴대폰을 옆으로 돌려 해당 영역으로 확장 된 비트맵이 있는지 확인 합니다.

[![균일 한 크기 조정](displaying-images/UniformScaling-Landscape.png "균일 한 크기 조정")](displaying-images/UniformScaling-Landscape-Large.png#lightbox)

이 요소를 사용 하는 이점은 `scale` 약간 다른 알고리즘을 구현 하려는 경우에 분명 하 게 드러납니다. 비트맵의 가로 세로 비율을 유지 하 고 대상 사각형을 채우도록 하려고 한다고 가정 합니다. 이렇게 하려면 이미지의 일부를 자르는 것이 가능 하지만 `Math.Min` 위의 코드에서을로 변경 하 여이 알고리즘을 간단히 구현할 수 있습니다 `Math.Max` . 결과는 다음과 같습니다. 

[![균일 한 확장 대안](displaying-images/UniformScaling-Alternative.png "균일 한 확장 대안")](displaying-images/UniformScaling-Alternative-Large.png#lightbox)

비트맵의 가로 세로 비율이 유지 되지만 비트맵의 왼쪽과 오른쪽에 있는 영역은 잘립니다.

## <a name="a-versatile-bitmap-display-function"></a>다용도 비트맵 표시 함수

XAML 기반 프로그래밍 환경 (예: UWP 및)에는 Xamarin.Forms 가로 세로 비율을 유지 하면서 비트맵 크기를 확장 하거나 축소할 수 있는 기능이 있습니다. SkiaSharp에는이 기능이 포함 되어 있지 않지만 직접 구현할 수 있습니다. `BitmapExtensions` [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 응용 프로그램에 포함 된 클래스는 방법을 보여 줍니다. 클래스는 `DrawBitmap` 가로 세로 비율 계산을 수행 하는 두 개의 새 메서드를 정의 합니다. 이러한 새 메서드는의 확장 메서드입니다 `SKCanvas` .

새 `DrawBitmap` 메서드에는 `BitmapStretch` **BitmapExtensions.cs** 파일에 정의 된 열거형 인 형식의 매개 변수가 포함 됩니다.

```csharp
public enum BitmapStretch
{
    None,
    Fill,
    Uniform,
    UniformToFill,
    AspectFit = Uniform,
    AspectFill = UniformToFill
}
```

`None`,, `Fill` `Uniform` 및 멤버는 `UniformToFill` UWP 열거형의 멤버와 동일 합니다 [`Stretch`](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.stretch.aspx) . 유사한 열거형은, Xamarin.Forms [`Aspect`](xref:Xamarin.Forms.Aspect) 및 멤버를 정의 합니다 `Fill` `AspectFit` `AspectFill` .

위에 표시 된 **균일 한 크기 조정** 페이지는 사각형 내에서 비트맵을 가운데에 배치 하지만 사각형의 왼쪽 이나 오른쪽에 비트맵을 배치 하는 등의 다른 옵션을 원할 수도 있습니다. 이는 열거형의 목적입니다 `BitmapAlignment` .

```csharp
public enum BitmapAlignment
{
    Start,
    Center,
    End
}
```

에서 사용 하는 경우 맞춤 설정은 적용 되지 않습니다 `BitmapStretch.Fill` .

첫 번째 `DrawBitmap` 확장 함수는 대상 사각형을 포함 하지만 소스 사각형은 포함 하지 않습니다. 기본값이 정의 되므로 비트맵을 가운데에 배치 하려면 구성원만 지정 하면 됩니다 `BitmapStretch` .

```csharp
static class BitmapExtensions
{
    ···
    public static void DrawBitmap(this SKCanvas canvas, SKBitmap bitmap, SKRect dest, 
                                  BitmapStretch stretch, 
                                  BitmapAlignment horizontal = BitmapAlignment.Center, 
                                  BitmapAlignment vertical = BitmapAlignment.Center, 
                                  SKPaint paint = null)
    {
        if (stretch == BitmapStretch.Fill)
        {
            canvas.DrawBitmap(bitmap, dest, paint);
        }
        else
        {
            float scale = 1;

            switch (stretch)
            {
                case BitmapStretch.None:
                    break;

                case BitmapStretch.Uniform:
                    scale = Math.Min(dest.Width / bitmap.Width, dest.Height / bitmap.Height);
                    break;

                case BitmapStretch.UniformToFill:
                    scale = Math.Max(dest.Width / bitmap.Width, dest.Height / bitmap.Height);
                    break;
            }

            SKRect display = CalculateDisplayRect(dest, scale * bitmap.Width, scale * bitmap.Height, 
                                                  horizontal, vertical);

            canvas.DrawBitmap(bitmap, display, paint);
        }
    }
    ···
}
```

이 메서드의 주요 목적은 `scale` 메서드를 호출할 때 비트맵 너비와 높이에 적용 되는 이라는 배율 인수를 계산 하는 것입니다 `CalculateDisplayRect` . 이는 가로 및 세로 맞춤을 기준으로 비트맵을 표시 하는 사각형을 계산 하는 메서드입니다.

```csharp
static class BitmapExtensions
{
    ···
    static SKRect CalculateDisplayRect(SKRect dest, float bmpWidth, float bmpHeight, 
                                       BitmapAlignment horizontal, BitmapAlignment vertical)
    {
        float x = 0;
        float y = 0;

        switch (horizontal)
        {
            case BitmapAlignment.Center:
                x = (dest.Width - bmpWidth) / 2;
                break;

            case BitmapAlignment.Start:
                break;

            case BitmapAlignment.End:
                x = dest.Width - bmpWidth;
                break;
        }

        switch (vertical)
        {
            case BitmapAlignment.Center:
                y = (dest.Height - bmpHeight) / 2;
                break;

            case BitmapAlignment.Start:
                break;

            case BitmapAlignment.End:
                y = dest.Height - bmpHeight;
                break;
        }

        x += dest.Left;
        y += dest.Top;

        return new SKRect(x, y, x + bmpWidth, y + bmpHeight);
    }
}
```

클래스에는 `BitmapExtensions` `DrawBitmap` 비트맵의 하위 집합을 지정 하기 위한 소스 사각형을 포함 하는 추가 메서드가 포함 되어 있습니다. 배율 인수가 사각형을 기준으로 계산 된 `source` 다음 `source` 에 대 한 호출의 사각형에 적용 된다는 점을 제외 하 고이 메서드는 첫 번째 방법과 비슷합니다 `CalculateDisplayRect` .

```csharp
static class BitmapExtensions
{
    ···
    public static void DrawBitmap(this SKCanvas canvas, SKBitmap bitmap, SKRect source, SKRect dest,
                                  BitmapStretch stretch,
                                  BitmapAlignment horizontal = BitmapAlignment.Center,
                                  BitmapAlignment vertical = BitmapAlignment.Center,
                                  SKPaint paint = null)
    {
        if (stretch == BitmapStretch.Fill)
        {
            canvas.DrawBitmap(bitmap, source, dest, paint);
        }
        else
        {
            float scale = 1;

            switch (stretch)
            {
                case BitmapStretch.None:
                    break;

                case BitmapStretch.Uniform:
                    scale = Math.Min(dest.Width / source.Width, dest.Height / source.Height);
                    break;

                case BitmapStretch.UniformToFill:
                    scale = Math.Max(dest.Width / source.Width, dest.Height / source.Height);
                    break;
            }

            SKRect display = CalculateDisplayRect(dest, scale * source.Width, scale * source.Height, 
                                                  horizontal, vertical);

            canvas.DrawBitmap(bitmap, source, display, paint);
        }
    }
    ···
}
```

이러한 두 가지 새 방법 중 첫 번째 `DrawBitmap` 방법은 **크기 조정 모드** 페이지에서 보여 줍니다. XAML 파일에는 `Picker` 및 열거형의 멤버를 선택할 수 있는 세 개의 요소가 포함 되어 있습니다 `BitmapStretch` `BitmapAlignment` .

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SkiaSharpFormsDemos"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.ScalingModesPage"
             Title="Scaling Modes">

    <Grid Padding="10">
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <skia:SKCanvasView x:Name="canvasView" 
                           Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Label Text="Stretch:"
               Grid.Row="1" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="stretchPicker"
                Grid.Row="1" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapStretch}">
                    <x:Static Member="local:BitmapStretch.None" />
                    <x:Static Member="local:BitmapStretch.Fill" />
                    <x:Static Member="local:BitmapStretch.Uniform" />
                    <x:Static Member="local:BitmapStretch.UniformToFill" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Label Text="Horizontal Alignment:"
               Grid.Row="2" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="horizontalPicker" 
                Grid.Row="2" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapAlignment}">
                    <x:Static Member="local:BitmapAlignment.Start" />
                    <x:Static Member="local:BitmapAlignment.Center" />
                    <x:Static Member="local:BitmapAlignment.End" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Label Text="Vertical Alignment:"
               Grid.Row="3" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="verticalPicker" 
                Grid.Row="3" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapAlignment}">
                    <x:Static Member="local:BitmapAlignment.Start" />
                    <x:Static Member="local:BitmapAlignment.Center" />
                    <x:Static Member="local:BitmapAlignment.End" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>
    </Grid>
</ContentPage>
```

코드를 `CanvasView` 변경 하면 항목이 변경 될 때 코드 숨김이 간단히 무효화 됩니다 `Picker` . `PaintSurface`처리기는 `Picker` 확장 메서드를 호출 하기 위해 세 가지 뷰에 액세스 합니다 `DrawBitmap` .

```csharp
public partial class ScalingModesPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(ScalingModesPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public ScalingModesPage()
    {
        InitializeComponent();
    }

    private void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect dest = new SKRect(0, 0, info.Width, info.Height);

        BitmapStretch stretch = (BitmapStretch)stretchPicker.SelectedItem;
        BitmapAlignment horizontal = (BitmapAlignment)horizontalPicker.SelectedItem;
        BitmapAlignment vertical = (BitmapAlignment)verticalPicker.SelectedItem;

        canvas.DrawBitmap(bitmap, dest, stretch, horizontal, vertical);
    }
}
```

다음은 몇 가지 옵션 조합입니다.

[![크기 조정 모드](displaying-images/ScalingModes.png "크기 조정 모드")](displaying-images/ScalingModes-Large.png#lightbox)

**사각형 하위 집합** 페이지에는 **크기 조정 모드**와 거의 동일한 XAML 파일이 있지만 코드 숨김이 파일은 필드에서 제공 하는 비트맵의 사각형 하위 집합을 정의 합니다 `SOURCE` . 

```csharp
public partial class ScalingModesPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(ScalingModesPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");

    static readonly SKRect SOURCE = new SKRect(94, 12, 212, 118);

    public RectangleSubsetPage()
    {
        InitializeComponent();
    }

    private void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect dest = new SKRect(0, 0, info.Width, info.Height);

        BitmapStretch stretch = (BitmapStretch)stretchPicker.SelectedItem;
        BitmapAlignment horizontal = (BitmapAlignment)horizontalPicker.SelectedItem;
        BitmapAlignment vertical = (BitmapAlignment)verticalPicker.SelectedItem;

        canvas.DrawBitmap(bitmap, SOURCE, dest, stretch, horizontal, vertical);
    }
}
```

다음 스크린샷에 표시 된 것 처럼이 사각형 소스는 원숭이의 머리를 격리 합니다.

[![사각형 하위 집합](displaying-images/RectangleSubset.png "사각형 하위 집합")](displaying-images/RectangleSubset-Large.png#lightbox)

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
