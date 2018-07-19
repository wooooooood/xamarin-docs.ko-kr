---
title: SkiaSharp 비트맵을 표시합니다.
description: SkiaSharp 비트맵 픽셀의 크기 및 가로 세로 비율을 유지 하면서 사각형에 맞게 확장을 표시 하는 방법에 알아봅니다.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8E074F8D-4715-4146-8CC0-FD7A8290EDE9
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: 31a5841c78b68b916e3df3d1d6598b86b0ec8bd4
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39131518"
---
# <a name="displaying-skiasharp-bitmaps"></a>SkiaSharp 비트맵을 표시합니다.

SkiaSharp 비트맵의 제목을 문서의 도입  **[SkiaSharp의 비트맵 기본 사항](../basics/bitmaps.md)** 합니다. 이 문서에는 세 가지 방법 부하 비트맵 및 비트맵을 표시 하는 세 가지 방법 보여 주었습니다. 이 문서에서는 비트맵을 로드 하는 기술을 검토 하 고 심층적인 사용 되는 `DrawBitmap` 메서드의 `SKCanvas`합니다.

![샘플을 표시](displaying-images/DisplayingSample.png "샘플 표시")

`DrawBitmapLattice` 하 고 `DrawBitmapNinePatch` 메서드는 문서에 설명 되어  **[SkiaSharp 비트맵의 표시를 분할](segmented.md)**.

이 페이지에는 샘플은는 **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** 응용 프로그램입니다. 해당 응용 프로그램의 홈 페이지에서 선택 **SkiaSharp 비트맵**로 이동 합니다 **표시 비트맵** 섹션.

## <a name="loading-a-bitmap"></a>비트맵을 로드합니다.

SkiaSharp 응용 프로그램에 일반적으로 사용 하는 비트맵의 세 가지 소스 중 하나에서 나옵니다.

- 인터넷을 통한
- 실행 파일에 포함 된 리소스에서
- 사용자의 사진 라이브러리에서

SkiaSharp 또는 응용 프로그램을 새 비트맵을 만들고 및 다음에 그릴 비트맵 비트 알고리즘 방식으로 설정할 수 이기도 합니다. 이러한 기술은 문서에 설명 **[만들고 SkiaSharp 비트맵에 드로잉](drawing.md)** 하 고 **[SkiaSharp 비트맵 픽셀 액세스](pixel-bits.md)**.

비트맵을 로드 하는 다음 세 가지 코드 예제에서는 클래스 형식의 필드를 포함 하도록 가정 `SKBitmap`:

```csharp
SKBitmap bitmap;
```

아티클로 **[SkiaSharp의 비트맵 기본 사항](../basics/bitmaps.md)** 사용 하 여 인터넷을 통해 비트맵을 로드 하는 가장 좋은 방법은 언급 합니다 [ `HttpClient` ](xref:System.Net.Http.HttpClient) 클래스입니다. 필드로 클래스의 단일 인스턴스를 정의할 수 있습니다.

```csharp
HttpClient httpClient = new HttpClient();
```

사용 하는 경우 `HttpClient` iOS 및 Android 응용 프로그램에서 문서에 설명 된 대로 프로젝트 속성을 설정 하려는  **[전송 계층 보안 (TLS) 1.2](~/cross-platform/app-fundamentals/transport-layer-security.md)** 합니다.

사용 하는 코드 `HttpClient` 종종 합니다 `await` 연산자를 사용 하므로에 상주해 야를 `async` 메서드:

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

있음을 합니다 `Stream` 개체에서 가져온 `GetStreamAsync` 에 복사 됩니다는 `MemoryStream`합니다. Android 허용 하지 않습니다 합니다 `Stream` 에서 `HttpClient` 비동기 메서드에서 제외 하 고 주 스레드에서 처리 되도록 합니다. 

[ `SKBitmap.Decode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Decode/p/System.IO.Stream/) 은 많은 작업 수행:는 `Stream` 전달 된 개체를 일반적인 비트맵 파일 형식, 일반적으로 JPEG, PNG 또는 GIF 중 하나는 전체 비트맵을 포함 하는 메모리 블록을 참조 합니다. `Decode` 메서드는 형식을 결정 하 고 SkiaSharp의 내부 비트맵 형식으로 비트맵 파일을 디코딩 한 다음 해야 합니다.

코드 호출 후 `SKBitmap.Decode`, 아마도으로 무효화 됩니다 합니다 `CanvasView` 있도록는 `PaintSurface` 처리기 새로 로드 된 비트맵을 표시할 수 있습니다.

.NET Standard 라이브러리에 포함 된 리소스로 비트맵을 포함 하 여 비트맵을 로드 하는 두 번째 방법은 개별 플랫폼 프로젝트에서 참조 합니다. 리소스 ID가 전달 된 [ `GetManifestResourceStream` ](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) 메서드. 이 리소스 ID는 어셈블리 이름, 폴더 이름과 마침표로 구분 하 여 리소스의 파일 이름으로 구성 됩니다.

```csharp
string resourceID = "assemblyName.folderName.fileName";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
{
    bitmap = SKBitmap.Decode(stream);
    ···
}
```

또한 iOS, Android 및 유니버설 Windows 플랫폼 (UWP)에 대 한 개별 플랫폼 프로젝트에 리소스로 비트맵 파일을 저장할 수 있습니다. 그러나 이러한 비트맵 로드 플랫폼 프로젝트에 있는 코드가 필요 합니다.

비트맵을 가져오는 세 번째 방법은 사용자의 사진 라이브러리 에서입니다. 다음 코드에 포함 되어 있는 종속성 서비스를 사용 합니다 **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** 응용 프로그램입니다. **SkiaSharpFormsDemo** .NET Standard 라이브러리는 포함를 `IPhotoLibrary` 인터페이스를 각 플랫폼 프로젝트를 포함 하는 동안는 `PhotoLibrary` 인터페이스를 구현 하는 클래스입니다.

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

일반적으로 이러한 코드 무효화 합니다 `CanvasView` 있도록는 `PaintSurface` 처리기 새로운 비트맵을 표시할 수 있습니다.

합니다 `SKBitmap` 클래스를 비롯 한 몇 가지 유용한 속성을 정의 [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Width/) 하 고 [ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Height/), 등 여러 방법 뿐만 아니라 비트맵의 픽셀 크기를 표시 하는 비트맵, 픽셀 비트를 노출 하 고 복사를 만드는 메서드를 제공 합니다. 

## <a name="displaying-in-pixel-dimensions"></a>픽셀 크기를 표시합니다.

SkiaSharp [ `Canvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/) 4 클래스 정의 `DrawBitmap` 메서드. 이러한 메서드는 근본적으로 서로 다른 두 가지 방법으로 표시할 비트맵 허용: 

- 지정 하는 `SKPoint` 값 (또는 별도 `x` 및 `y` 값) 해당 픽셀 크기의 비트맵을 표시 합니다. 비트맵의 픽셀은 비디오 디스플레이의 픽셀에 직접 매핑됩니다.
- 사각형을 지정 하면 비트맵을 사각형 모양의 크기를 늘일 수 있습니다. 

사용 하 여 해당 픽셀 크기의 비트맵을 표시 [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKPoint/SkiaSharp.SKPaint/) 와 `SKPoint` 매개 변수 또는 [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/System.Single/System.Single/SkiaSharp.SKPaint/) 별도의 `x` 고 `y` 매개 변수:

```csharp
DrawBitmap(SKBitmap bitmap, SKPoint pt, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, float x, float y, SKPaint paint = null)
```

이러한 메서드와 기능적으로 동일 합니다. 지정된 된 지점 캔버스를 기준으로 비트맵의 왼쪽 위 모퉁이 위치를 나타냅니다. 모바일 장치의 픽셀 해상도 높아 이기 때문에 작은 비트맵은 일반적으로 이러한 장치에서 매우 작은 표시 합니다.

선택적 `SKPaint` 매개 변수를 사용 하면 혼합 모드를 사용 하 여 비트맵을 표시 하거나 결과 필터링 합니다. 이러한 이후 기사에서 소개 합니다.

합니다 **픽셀 크기** 페이지에 **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** 320 x 240 픽셀 높은 있는 비트맵 리소스를 표시 하는 샘플 프로그램:

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

합니다 `PaintSurface` 처리기를 계산 하 여 비트맵을 센터 `x` 및 `y` 값을 디스플레이 화면의 픽셀 크기 및 비트맵의 픽셀 크기에 따라:

[![픽셀 치수](displaying-images/PixelDimensions.png "픽셀 크기")](displaying-images/PixelDimensions-Large.png#lightbox)

응용 프로그램의 왼쪽 위 모퉁이에서 비트맵을 표시할 경우, 해당 간단히 전달 좌표 (0, 0). 

## <a name="a-method-for-loading-resource-bitmaps"></a>리소스 비트맵을 로드 하는 방법

에 나오는 샘플은 대부분 비트맵 리소스를 로드 해야 합니다. 정적 `BitmapExtensions` 클래스를 **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** 도와줄 메서드를 포함 하는 솔루션:

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

통지를 `Type` 매개 변수입니다. 이 수는 `Type` 비트맵 리소스를 저장 하는 어셈블리의 모든 형식과 관련 된 개체입니다.

이 `LoadBitmapResource` 메서드 비트맵 리소스를 필요로 하는 모든 후속 샘플에서 사용 됩니다.

## <a name="stretching-to-fill-a-rectangle"></a>사각형에 맞게 늘이기

합니다 `SKCanvas` 클래스도 정의 [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKPaint/) 사각형 및 다른 비트맵을 렌더링 하는 메서드 [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/) 비트맵의 사각형 하위 집합을 렌더링 하는 메서드를 사각형:

```
DrawBitmap(SKBitmap bitmap, SKRect dest, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, SKRect source, SKRect dest, SKPaint paint = null)
```

두 경우 모두 비트맵은 명명 된 사각형을 채우도록 확장 `dest`합니다. 두 번째 메서드에서 `source` 사각형을 사용 하면 비트맵의 하위 집합을 선택할 수 있습니다. 합니다 `dest` 사각형은 출력 장치를 기준으로 `source` 사각형 비트맵에 상대적입니다.

합니다 **사각형 채우기** 페이지 캔버스와 크기가 같은 사각형에서 앞의 예제에서 사용는 같은 비트맵을 표시 하 여 이러한 두 가지 방법 중 첫 번째 하는 방법을 보여 줍니다. 

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

사용 하 여 새 `BitmapExtensions.LoadBitmapResource` 설정 하는 메서드는 `SKBitmap` 필드입니다. 대상 사각형에서 가져온 합니다 [ `Rect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Rect/) 속성의 `SKImageInfo`는 설치 화면 크기:

[![사각형을 채우는](displaying-images/FillRectangle.png "사각형 채우기")](displaying-images/FillRectangle-Large.png#lightbox)

일반적으로 이것이 _되지_ 원하는 합니다. 가로 및 세로 방향으로 다르게 스트레치 되 여 이미지가 왜곡 됩니다. 비트맵 픽셀 크기로 외의 다른 표시 하는 경우 비트맵의 원래 가로 세로 비율을 유지 하려는 일반적으로 합니다.

## <a name="stretching-while-preserving-the-aspect-ratio"></a>가로 세로 비율을 유지 하면서 늘이기

이 프로세스를 라고도 가로 세로 비율을 유지 하면서 비트맵을 확대 _균일 한 크기 조정_합니다. 용어 알고리즘 방법을 제시 합니다. 한 가지 해결책 같습니다 합니다 **균일 한 크기 조정** 페이지:

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

합니다 `PaintSurface` 처리기에서 계산을 `scale` 표시 너비 및 높이 비트맵 너비와 높이의 비율의 최소 비율입니다. 합니다 `x` 고 `y` 값의 가운데에 크기 조정 된 비트맵 내 표시 너비와 높이 계산할 수 있습니다. 대상 사각형에는 왼쪽 위 상단 `x` 및 `y` 해당 값과 배율 조정 된 너비의 오른쪽 아래 모서리와 비트맵의 높이 및:

[![균일 한 크기 조정](displaying-images/UniformScaling.png "균일 한 크기 조정")](displaying-images/UniformScaling-Large.png#lightbox)

해당 영역을 채우도록 확장 비트맵을 확인 하려면 휴대폰을 옆으로 설정 합니다.

[![크기 조정 가로 uniform](displaying-images/UniformScaling-Landscape.png "가로 균일 한 크기 조정")](displaying-images/UniformScaling-Landscape-Large.png#lightbox)

이 사용 하는 이점은 `scale` 약간 다른 알고리즘을 구현 하려는 경우 단계 알 수 있습니다. 대상 사각형을 채울 수도 있지만 비트맵의 가로 세로 비율 유지 한다고 가정 합니다. 이것이 가능 하는 유일한 방법은 이미지 부분을 자르기가 있지만 변경 하면 해당 알고리즘을 구현할 수 있습니다 `Math.Min` 에 `Math.Max` 위 코드에서. 결과 다음과 같습니다. 

[![균일 한 크기 조정 대안](displaying-images/UniformScaling-Alternative.png "균일 한 크기 조정 대안")](displaying-images/UniformScaling-Alternative-Large.png#lightbox)

비트맵의 가로 세로 비율이 유지 됩니다 있지만 영역 비트맵 오른쪽 및 왼쪽에서 잘립니다.

## <a name="a-versatile-bitmap-display-function"></a>다양 한 비트맵 표시 함수

XAML 기반 프로그래밍 환경 (예: UWP 및 Xamarin.Forms)에 확장 또는 해당 가로 세로 비율을 유지 하면서 비트맵의 크기를 축소 하는 기능을 있습니다. SkiaSharp이이 기능을 포함 하지 않습니다, 하지만 직접 구현할 수 있습니다 것입니다. `BitmapExtensions` 에 포함 된 클래스를 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 응용 프로그램 표시 하는 방법. 클래스 정의 두 개의 새 `DrawBitmap` 가로 세로 비율 계산을 수행 하는 방법입니다. 이러한 새 메서드는 확장 메서드의 `SKCanvas`합니다.

새 `DrawBitmap` 형식의 매개 변수를 포함 하는 방법 `BitmapStretch`에 정의 된 열거형 합니다 **BitmapExtensions.cs** 파일:

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

합니다 `None`, `Fill`를 `Uniform`, 및 `UniformToFill` 멤버 UWP에서와 동일 [ `Stretch` ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.stretch.aspx) 열거형입니다. 유사한 Xamarin.Forms [ `Aspect` ](xref:Xamarin.Forms.Aspect) 멤버를 정의 하는 열거형 `Fill`를 `AspectFit`, 및 `AspectFill`합니다.

합니다 **균일 한 크기 조정** 페이지 위에 표시 된 사각형 내의 비트맵 센터 있지만 비트맵 왼쪽 또는 오른쪽 사각형 위쪽 또는 아래쪽에 있는 위치 등의 다른 옵션을 수 있습니다. 용도는 `BitmapAlignment` 열거형:

```csharp
public enum BitmapAlignment
{
    Start,
    Center,
    End
}
```

맞춤 설정이 함께 사용할 경우 효과가 `BitmapStretch.Fill`합니다.

첫 번째 `DrawBitmap` 확장 함수 없는 소스 사각형 대상 사각형을 포함 합니다. 가운데에 비트맵을 하려는 경우 지정 하면 있도록 기본값이 정의 되어는 `BitmapStretch` 멤버:

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

명명 된 배율 인수를 계산 하는이 메서드는 주로 `scale` 다음에 적용 되는 비트맵의 너비와 높이 호출 하는 경우는 `CalculateDisplayRect` 메서드. 이 값은 가로 및 세로 맞춤에 따라 비트맵을 표시 하는 것에 대 한 사각형을 계산 하는 메서드입니다.

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

합니다 `BitmapExtensions` 클래스 추가 포함 `DrawBitmap` 비트맵의 하위 집합을 지정 하는 데 사용 하 여 원본 사각형이 메서드. 이 메서드는 첫 번째 점을 제외 하 고 배율 인수를 기준으로 계산 됩니다는 `source` 사각형 후 적용 된 `source` 사각형에 대 한 호출에서 `CalculateDisplayRect`:

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

이러한 두 가지 새로운 중 첫 번째 `DrawBitmap` 에 설명 된 메서드를 **크기 조정 모드** 페이지입니다. XAML 파일이 세 `Picker` 수 있도록 하는 요소에 구성원을 선택 합니다 `BitmapStretch` 및 `BitmapAlignment` 열거형:

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

코드 숨김 파일을 간단히 무효화 합니다 `CanvasView` 있으면 `Picker` 항목이 변경 되었습니다. 합니다 `PaintSurface` 세 가지 액세스 하는 처리기 `Picker` 호출에 대 한 보기를 `DrawBitmap` 확장 메서드:

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

옵션의 일부 조합은 다음과 같습니다.

[![크기 조정 모드](displaying-images/ScalingModes.png "크기 조정 모드")](displaying-images/ScalingModes-Large.png#lightbox)

**사각형 하위 집합** 페이지에 거의 동일한 XAML 파일로 **크기 조정 모드**, 코드 숨김 파일을 제공한 비트맵의 사각형 하위 집합을 정의 하지만 `SOURCE` 필드: 

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

이 스크린 샷에 표시 된 것 처럼 monkey의 머리를 격리 하는이 사각형 원본:

[![사각형 하위 집합](displaying-images/RectangleSubset.png "사각형 하위 집합")](displaying-images/RectangleSubset-Large.png#lightbox)

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

