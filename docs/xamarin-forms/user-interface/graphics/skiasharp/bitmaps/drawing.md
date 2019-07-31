---
title: 만들고 SkiaSharp 비트맵에 그리기
description: SkiaSharp 비트맵을 만들고 그에 따라 캔버스를 만들어 이러한 비트맵에 드로잉을 하는 방법을 알아봅니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 79BD3266-D457-4E50-BDDF-33450035FA0F
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: 030655ba94130294729871348b3408fe6c3695e6
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656947"
---
# <a name="creating-and-drawing-on-skiasharp-bitmaps"></a>만들고 SkiaSharp 비트맵에 그리기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

살펴보았습니다 방법을 통해 응용 프로그램 웹, 응용 프로그램 리소스 및 사용자의 사진 라이브러리에서 비트맵을 로드할 수 있습니다. 응용 프로그램 내에서 새 비트맵을 만들 수 이기도 합니다. 가장 간단한 방법은의 생성자 중 하나를 포함 [ `SKBitmap` ](xref:SkiaSharp.SKBitmap.%23ctor(System.Int32,System.Int32,System.Boolean)):

```csharp
SKBitmap bitmap = new SKBitmap(width, height);
```

합니다 `width` 고 `height` 매개 변수는 정수 및 비트맵의 픽셀 크기를 지정 합니다. 이 생성자는 픽셀당 4 바이트를 사용 하 여 전체 색 비트맵을 만듭니다: 빨강, 녹색, 파랑 및 알파 (불투명) 구성 요소에 대 한 1 바이트입니다. 

새 비트맵을 만든 후 비트맵의 화면에 결과가 표시 해야 합니다. 일반적으로 두 가지 방법 중 하나에서 이렇게합니다.

- Standard를 사용 하 여 비트맵에 드로잉 `Canvas` 그리기 메서드.
- 픽셀 비트에 직접 액세스 합니다.

이 문서에서는 첫 번째 방법을 보여 줍니다.

![샘플을 그리기](drawing-images/DrawingSample.png "샘플 그리기")

두 번째 방법은 문서에 설명 되어 [ **SkiaSharp 비트맵 픽셀에 액세스**](pixel-bits.md)합니다.

## <a name="drawing-on-the-bitmap"></a>비트맵에 그리기

비트맵의 화면에서 그리기 비디오 디스플레이에 그리기와 같습니다. 가져올 비디오 디스플레이에 그릴를 `SKCanvas` 에서 개체를 `PaintSurface` 이벤트 인수입니다. 만든 비트맵을 그리려면를 `SKCanvas` 를 사용 하 여 개체를 [ `SKCanvas` ](xref:SkiaSharp.SKCanvas.%23ctor(SkiaSharp.SKBitmap)) 생성자:

```csharp
SKCanvas canvas = new SKCanvas(bitmap);
```

비트맵을 토대로 작업을 완료 하는 경우의 삭제할 수 있습니다는 `SKCanvas` 개체입니다. 이러한 이유로 합니다 `SKCanvas` 생성자에서 일반적으로 호출 되는 `using` 문:

```csharp
using (SKCanvas canvas = new SKCanvas(bitmap))
{
    ··· // call drawing function
}
```

비트맵 표시 될 수 있습니다. 나중에 프로그램을 새로 만들 수 있습니다 `SKCanvas` 동일한, 비트맵 및 조금 더 그릴 개체 기반으로 합니다.

**Hello 비트맵** 페이지에 **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** 응용 프로그램 작성 텍스트 "Hello, 비트맵!" 비트맵에 여러 번 비트맵을 표시 됩니다.  

생성자는 `HelloBitmapPage` 만들어 시작을 `SKPaint` 텍스트 표시에 대 한 개체입니다. 텍스트 문자열의 크기를 결정 하 고 해당 차원을 사용 하 여 비트맵을 만듭니다. 그런 다음 만듭니다는 `SKCanvas` 해당 비트맵, 호출을 기반으로 하는 개체 `Clear`, 다음 호출 `DrawText`. 호출 하는 것이 좋습니다는 항상 `Clear` 새 비트맵을 사용 하 여 새로 만든된 비트맵 임의 데이터를 포함 될 수 있습니다.

만들어 마지막 생성자는 `SKCanvasView` 비트맵을 표시 하는 개체:

```csharp
public partial class HelloBitmapPage : ContentPage
{
    const string TEXT = "Hello, Bitmap!";
    SKBitmap helloBitmap;

    public HelloBitmapPage()
    {
        Title = TEXT;

        // Create bitmap and draw on it
        using (SKPaint textPaint = new SKPaint { TextSize = 48 })
        {
            SKRect bounds = new SKRect();
            textPaint.MeasureText(TEXT, ref bounds);

            helloBitmap = new SKBitmap((int)bounds.Right,
                                       (int)bounds.Height);

            using (SKCanvas bitmapCanvas = new SKCanvas(helloBitmap))
            {
                bitmapCanvas.Clear();
                bitmapCanvas.DrawText(TEXT, 0, -bounds.Top, textPaint);
            }
        }

        // Create SKCanvasView to view result 
        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Aqua);

        for (float y = 0; y < info.Height; y += helloBitmap.Height)
            for (float x = 0; x < info.Width; x += helloBitmap.Width)
            {
                canvas.DrawBitmap(helloBitmap, x, y);
            }
    }
}
```

`PaintSurface` 처리기에 여러 번 표시의 행과 열 비트맵을 렌더링 합니다. 있음을 `Clear` 에서 메서드를 `PaintSurface` 처리기의 인수에 `SKColors.Aqua`, 화면 배경의 색입니다.

[![Hello, 비트맵! ](drawing-images/HelloBitmap.png "Hello, 비트맵!")](drawing-images/HelloBitmap-Large.png#lightbox)

바다색 백그라운드 모양의 텍스트를 제외 하 고 투명 한 비트맵 임을 알 수 있습니다.

## <a name="clearing-and-transparency"></a>지우기 및 투명도

표시 된 **Hello 비트맵** 페이지 비트맵 만든 프로그램은 검은색 텍스트로 제외 하 고 투명 하 게 하는 방법을 보여 줍니다. 이유는 바다색 색 화면을 통해 보여 줍니다.

`Clear` 다음`SKCanvas` 방법의 설명서에서는 문을 사용 하 여 설명 합니다. "캔버스의 현재 클립에서 모든 픽셀을 바꿉니다." "Replace" 라는 단어를 사용 하면 이러한 메서드의 중요 한 특징이 표시 됩니다. 기존 표시 화면에 항목 `SKCanvas` 을 추가 하는 모든 그리기 메서드입니다. 합니다 `Clear` 메서드 _대체_ 가 이미 있습니다.

`Clear` 두 가지 버전에 있습니다. 

- 합니다 [ `Clear` ](xref:SkiaSharp.SKCanvas.Clear(SkiaSharp.SKColor)) 메서드를 `SKColor` 매개 변수는 픽셀 색을 디스플레이 화면의 픽셀을 대체 합니다.

- 합니다 [ `Clear` ](xref:SkiaSharp.SKCanvas.Clear) 메서드 매개 변수 없이 사용 하 여 픽셀을 대체 합니다 [ `SKColors.Empty` ](xref:SkiaSharp.SKColors.Empty) 색 (빨강, 녹색, 파랑 및 알파) 구성 요소는 0으로 설정 하는 모든 색입니다. 이 색은 라고도 함 투명 검정입니다.

호출 `Clear` 새 비트맵에 인수를 사용 하 여 완전히 투명 하 게 전체 비트맵을 초기화 합니다. 비트맵에 이후에 그려지는 아무것도 보통은 불투명 하 게 또는 부분적으로 불투명 합니다.

시도해 볼 수 있는 항목은 다음과 같습니다. **Hello 비트맵** 페이지에서에 적용 된 `Clear` `bitmapCanvas` 메서드를이 항목으로 바꿉니다.

```csharp
bitmapCanvas.Clear(new SKColor(255, 0, 0, 128));
```

순서는 `SKColor` 생성자 매개 변수는 빨간색, 녹색, 파랑 및 알파, 여기서 각 값의 범위는 0에서 255입니다. 알파 값이 0이 투명 알파 값이 255는 불투명 해지고 염두에 두어야 합니다.

(255, 0, 0, 128) 값을 50% 불투명도 사용 하 여 빨간색 픽셀로 비트맵 픽셀을 지웁니다. 이 비트맵 배경을 반투명 임을 의미 합니다. 비트맵의 반투명 하 게 빨간색 배경이 회색 배경이 만들려는 화면 배경의 바다색와 결합 합니다.

다음 할당에 배치 하 여 텍스트의 색을 투명 검정으로 설정 된 `SKPaint` 이니셜라이저:

```csharp
Color = new SKColor(0, 0, 0, 0)
```

이 투명 한 텍스트는는 바다색 배경의 화면 표시는 비트맵의 완전히 투명 한 영역을 만든 생각할 수 있습니다. 하지만 다릅니다. 항목의 비트맵에 이미 있습니다. 위에 텍스트를 그립니다. 투명 한 텍스트 표시 되지 않습니다 전혀 합니다.

이상 `Draw` 메서드 적이 비트맵 보다 투명 하 게 만듭니다. 만 `Clear` 수행할 수 있습니다.

## <a name="bitmap-color-types"></a>비트맵 색 형식

가장 간단한 `SKBitmap` 생성자를 사용 하면 정수 픽셀 너비 및 비트맵의 높이 지정할 수 있습니다. 다른 `SKBitmap` 생성자는 더 복잡 합니다. 이 생성자에는 두 열거형 형식의 인수가 필요 합니다. [ `SKColorType` ](xref:SkiaSharp.SKColorType) 하 고 [ `SKAlphaType` ](xref:SkiaSharp.SKAlphaType)합니다. 다른 생성자를 사용 합니다 [ `SKImageInfo` ](xref:SkiaSharp.SKImageInfo) 구조는이 정보를 통합 합니다.

`SKColorType` 열거형 멤버가 9입니다. 이러한 각각의이 멤버에는 비트맵 픽셀을 저장 하는 특정 방식으로 설명 합니다.

- `Unknown`
- `Alpha8` &mdash; 각 픽셀은 완전 불투명에서 완전 투명 알파 값을 나타내는 8 비트
- `Rgb565` &mdash; 각 픽셀은 16 비트를 5 비트가 빨강 및 파랑 및 녹색 6
- `Argb4444` &mdash; 각 픽셀은 16 비트를 4 각 알파, 빨강, 녹색 및 파랑
- `Rgba8888` &mdash; 각 픽셀은 각 8 빨간색, 녹색, 파랑 및 알파에 대 한 32 비트
- `Bgra8888` &mdash; 각 픽셀은 각 8 파랑, 녹색, 빨강 및 알파에 대 한 32 비트
- `Index8` &mdash; 각 픽셀은 8 비트 나타내고 인덱스는 [`SKColorTable`](xref:SkiaSharp.SKColorTable)
- `Gray8` &mdash; 각 픽셀은 흰색으로 검정에서 회색 음영을 나타내는 8 비트
- `RgbaF16` &mdash; 각 픽셀은 빨간색, 녹색, 파랑 및 알파는 16 비트 부동 소수점 형식에서 사용 하 여 64 비트

각 픽셀은 32 픽셀 (4 바이트)는 두 가지 형식 이라고 _컬러_ 형식입니다. 여러 비디오 자체 표시 되는 경우 한 번에서 다른 형식 날짜의 전체 색 수 있었습니다. 제한 된 색 비트맵이 디스플레이 대 한 적절 한 되었으며 비트맵 메모리에 더 적은 공간을 차지 하도록 허용 합니다. 

이러한 일 프로그래머에 게 거의 항상 전체 색 비트맵을 사용 하 여 및 다른 형식과 이름을 바꾸지 않습니다. 예외는 `RgbaF16` 컬러 형식에도 보다 큰 컬러 해상도 허용 하는 형식입니다. 그러나이 형식은 의료 이미지 등의 특수 한 용도로 사용 되 고 타당성을 많은 표준 컬러 디스플레이 사용 하는 경우.

이 문서 시리즈에서는 자체를 제한 합니다는 `SKBitmap` 하지 않으면 기본적으로 사용 되는 형식 색 `SKColorType` 멤버를 지정 합니다. 이 기본 형식 기본 플랫폼을 기반으로 합니다. Xamarin.Forms에서 지 원하는 플랫폼에 대 한 기본 색 유형은입니다.

- `Rgba8888` iOS 및 Android 용
- `Bgra8888` UWP에 대 한

유일한 차이점은 메모리에 4 바이트의 순서와이만 문제가 됩니다 픽셀 비트에 직접 액세스 하는 경우. 이 않습니다 더욱 중요 문서를 가져와야만 [ **SkiaSharp 비트맵 픽셀에 액세스**](pixel-bits.md)합니다.

`SKAlphaType` 열거형에는 네 가지 멤버:

- `Unknown`
- `Opaque` &mdash; 비트맵에 투명도 없이
- `Premul` &mdash; 색의 구성 요소는 알파 구성 요소를 곱한 미리
- `Unpremul` &mdash; 색의 구성 요소는 하지 미리 곱한 알파 구성 요소

순서 빨간색, 녹색, 파란색, 알파에 표시 된 바이트를 사용 하 여 50% 투명도 사용 하 여 4 바이트 빨간색 비트맵 픽셀은 다음과 같습니다.

0xFF 0x00 0x00 0x80

반투명 하 게 픽셀을 포함 하는 비트맵을 화면에 렌더링 되 면 각 비트맵 픽셀의 색 구성 요소를 해당 픽셀의 알파 값을 곱할 수 있어야 합니다. 및 디스플레이 화면의 해당 픽셀의 색 구성 요소를 곱할 수 있어야 합니다. 알파 값을 뺀 255입니다. 다음 두 픽셀을 결합할 수 있습니다. 비트맵을 렌더링할 수 더 빠르게 비트맵 픽셀에서 색 구성 요소를 이미 사전 mulitplied 알파 값을 기준으로 합니다. 해당 동일한 빨간색 픽셀 같이 미리 곱한 형식으로 저장 됩니다.

0x80 0x00 0x00 0x80

이러한 성능 향상은 이유 `SkiaSharp` 비트맵 기본적으로 사용 하 여 만들어진를 `Premul` 형식입니다. 하지만이 액세스 하 고 픽셀 비트를 조작 하는 경우에 알고이 다시입니다.

## <a name="drawing-on-existing-bitmaps"></a>에 기존 비트맵 그리기

에 그릴 새 비트맵을 만들 필요는 없습니다. 기존 비트맵 이미지에 그릴 수 있습니다. 

**Monkey 콧수염** 페이지의 생성자를 사용 하 여 로드 하는 **MonkeyFace.png** 이미지입니다. 그런 다음 만듭니다는 `SKCanvas` 개체는 비트맵을 기반으로 하며 사용 하 여 `SKPaint` 및 `SKPath` 는 콧수염에 그릴 개체:

```csharp
public partial class MonkeyMoustachePage : ContentPage
{
    SKBitmap monkeyBitmap;

    public MonkeyMoustachePage()
    {
        Title = "Monkey Moustache";

        monkeyBitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MonkeyFace.png");

        // Create canvas based on bitmap
        using (SKCanvas canvas = new SKCanvas(monkeyBitmap))
        {
            using (SKPaint paint = new SKPaint())
            {
                paint.Style = SKPaintStyle.Stroke;
                paint.Color = SKColors.Black;
                paint.StrokeWidth = 24;
                paint.StrokeCap = SKStrokeCap.Round;

                using (SKPath path = new SKPath())
                {
                    path.MoveTo(380, 390);
                    path.CubicTo(560, 390, 560, 280, 500, 280);

                    path.MoveTo(320, 390);
                    path.CubicTo(140, 390, 140, 280, 200, 280);

                    canvas.DrawPath(path, paint);
                }
            }
        }

        // Create SKCanvasView to view result 
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
        canvas.DrawBitmap(monkeyBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

만들어 마지막 생성자는 `SKCanvasView` 해당 `PaintSurface` 결과 단순히 표시 하는 처리기:

[![조정 콧수염](drawing-images/MonkeyMoustache.png "콧수염 조정")](drawing-images/MonkeyMoustache-Large.png#lightbox)

## <a name="copying-and-modifying-bitmaps"></a>복사 하 여 비트맵을 수정

메서드 `SKCanvas` 비트맵 포함 이미지를 그릴 사용할 수 있는 `DrawBitmap`합니다. 즉, 그릴 수 있는 하나의 비트맵, 일반적으로 어떤 방식으로든에서 수정 합니다.

실제 픽셀 비트에 액세스 하는 비트맵을 수정 하는 가장 다양 한 방법, 문서에 설명 제목을  **[SkiaSharp 액세스 비트맵 픽셀](pixel-bits.md)** 합니다. 하지만 여러 가지 기술을 픽셀 비트에 액세스 되지 않아도 되는 비트맵을 수정할 수 있습니다.

포함 된 다음 비트맵을 **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** 응용 프로그램은 360 픽셀 너비 및 높이 480 픽셀:

![Mountain Climbers](drawing-images/MountainClimbers.jpg "Mountain Climbers")

이 사진을 게시 하려면 왼쪽 monkey에서 권한을 받지 못했으면 가정. 한 가지 해결책은 기법을 사용 하는 monkey의 얼굴을 모호 _pixelization_합니다. 기능을 수행할 수 없습니다 있도록 글꼴의 픽셀 색의 요소를 사용 하 여 대체 됩니다. 색의 요소는 일반적으로 이러한 블록을 해당 픽셀의 색을 구하여 원래 이미지에서 파생 됩니다. 하지만이 평균 직접 수행할 필요가 없습니다. 이것이 자동으로 비트맵을 작은 픽셀 치수를 복사할 때. 

왼쪽된 monkey의 얼굴 약는 72 픽셀 사각형 영역 (112, 238) 지점에서 왼쪽 위 모퉁이 차지합니다. 보겠습니다 8-8 하 여 픽셀 사각형은 각 색이 지정 된 블록의 9에서 9에 대 한 배열을 사용 하 여 해당 72 픽셀 사각형 영역을 대체 합니다.

합니다 **이미지 흠집 제거 이미지** 페이지가 해당 비트맵에서 로드 되 고 먼저 라는 작은 9 픽셀 사각형 비트맵을 만들어 `faceBitmap`합니다. 이것이 바로 monkey의 얼굴을 복사 하는 것에 대 한 대상입니다. 대상 사각형은 단지 9 픽셀 사각형 이지만 소스 사각형 72 픽셀 사각형입니다. 모든 8-8 하 여 블록의 원본 픽셀 색 평균을 구하여 하나의 픽셀까지 통합 합니다.

원래 비트맵을 호출 하는 동일한 크기의 새 비트맵을 복사 하려면 다음 단계는 `pixelizedBitmap`합니다. 작은 `faceBitmap` 72 픽셀 사각형 대상 사각형을 사용 하 여 그 위에 후 복사 됩니다 있도록의 각 픽셀 `faceBitmap` 8 배 크기로 확장 됩니다.

```csharp
public class PixelizedImagePage : ContentPage
{
    SKBitmap pixelizedBitmap;

    public PixelizedImagePage ()
    {
        Title = "Pixelize Image";

        SKBitmap originalBitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

        // Create tiny bitmap for pixelized face
        SKBitmap faceBitmap = new SKBitmap(9, 9);

        // Copy subset of original bitmap to that
        using (SKCanvas canvas = new SKCanvas(faceBitmap))
        {
            canvas.Clear();
            canvas.DrawBitmap(originalBitmap,
                              new SKRect(112, 238, 184, 310),   // source
                              new SKRect(0, 0, 9, 9));          // destination

        }

        // Create full-sized bitmap for copy
        pixelizedBitmap = new SKBitmap(originalBitmap.Width, originalBitmap.Height);

        using (SKCanvas canvas = new SKCanvas(pixelizedBitmap))
        {
            canvas.Clear();

            // Draw original in full size
            canvas.DrawBitmap(originalBitmap, new SKPoint());

            // Draw tiny bitmap to cover face
            canvas.DrawBitmap(faceBitmap, 
                              new SKRect(112, 238, 184, 310));  // destination
        }

        // Create SKCanvasView to view result
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
        canvas.DrawBitmap(pixelizedBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

만들어 마지막 생성자는 `SKCanvasView` 결과 표시:

[![이미지를 이미지 흠집 제거](drawing-images/PixelizeImage.png "이미지를 이미지 흠집 제거")](drawing-images/PixelizeImage-Large.png#lightbox)

<a name="rotating-bitmaps" />

## <a name="rotating-bitmaps"></a>비트맵 회전

다른 일반적인 작업에는 비트맵을 회전 됩니다. IPhone 또는 iPad 사진 라이브러리에서 비트맵을 검색할 때 특히 유용 합니다. 사진의 찍을 때 때 장치 특정 방향에 보유 된 경우가 아니면 그림이 거꾸로 또는 옆쪽 일 수입니다.

첫 번째 크기가 같은 다른 비트맵을 만들기로 설정한 다음 첫 번째 두 번째 복사 하는 동안으로 180도 회전 변환이 필요 거꾸로 비트맵을 설정 합니다. 이 섹션의 예의 모든 `bitmap` 되는 `SKBitmap` 회전 해야 하는 개체:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.RotateDegrees(180, bitmap.Width / 2, bitmap.Height / 2);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

90도 회전 하면 높이 너비를 교환 하 여 원본 보다 다양 한 크기로 사용 되는 비트맵 만들기 해야 합니다. 예를 들어, 원래 비트맵 1200 픽셀 너비 및 800 픽셀, 회전된 비트맵 경우 1200 픽셀 너비 및 800 픽셀입니다. 비트맵 왼쪽 위 구석에 해당 중심으로 회전 하 고 다음 뷰로 이동 되도록 변환 및 회전을 설정 합니다. (에 유의 합니다 `Translate` 고 `RotateDegrees` 메서드가 적용 되는 방식의 반대 순서로 호출 됩니다.) 시계 방향으로 90도 회전 하는 것에 대 한 코드는 다음과 같습니다.

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.Translate(bitmap.Height, 0);
    canvas.RotateDegrees(90);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```                        

그리고은 시계 반대 방향으로 90도 회전 하는 것에 대 한 유사한 기능.

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.Translate(0, bitmap.Width);
    canvas.RotateDegrees(-90);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

이러한 두 메서드가 사용 됩니다 합니다 **사진 퍼즐** 문서에서 설명 하는 페이지 [ **자르기 SkiaSharp 비트맵**](cropping.md#tile-division)합니다.

90도 증가분에서 비트맵을 회전 시킬 수 있도록 프로그램으로 90도 회전 하는 것에 대 한 하나의 함수를 구현만 필요 합니다. 사용자 한이 함수의 반복된 실행 하 여 90도의 모든 증가에 회전할 수 있습니다.

또한 프로그램 모든 양만큼 비트맵을 회전할 수 있습니다. 일반화 된를 180을 바꿔으로 180도 회전 하는 함수를 수정 하는 한 가지 간단한 방법은 것 `angle` 변수:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.RotateDegrees(angle, bitmap.Width / 2, bitmap.Height / 2);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

그러나 일반적인 경우이 논리를 자르고 회전된 비트맵의 모퉁이. 삼각 함수를 사용 하 여 해당 모퉁이 포함 하도록 회전된 비트맵의 크기를 계산 하는 것이 좋습니다. 

이 trigonometry 같으며 합니다 **비트맵 Rotator** 페이지입니다. XAML 파일은는 `SKCanvasView` 와 `Slider` 는 까지일 수 있습니다 0에서 360도 사용 하 여는 `Label` 현재 값을 보여 주는:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.BitmapRotatorPage"
             Title="Bitmap Rotator">
    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="slider"
                Maximum="360"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='Rotate by {0:F0}&#x00B0;'}"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

코드 숨김 파일 비트맵 리소스를 로드 하 고 라는 정적 읽기 전용 필드 이름으로 저장 `originalBitmap`합니다. 비트맵에 표시 합니다 `PaintSurface` 처리기는 `rotatedBitmap`, 처음 설정 된 `originalBitmap`:

```csharp
public partial class BitmapRotatorPage : ContentPage
{
    static readonly SKBitmap originalBitmap = 
        BitmapExtensions.LoadBitmapResource(typeof(BitmapRotatorPage),
            "SkiaSharpFormsDemos.Media.Banana.jpg");

    SKBitmap rotatedBitmap = originalBitmap;

    public BitmapRotatorPage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(rotatedBitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        double angle = args.NewValue;
        double radians = Math.PI * angle / 180;
        float sine = (float)Math.Abs(Math.Sin(radians));
        float cosine = (float)Math.Abs(Math.Cos(radians));
        int originalWidth = originalBitmap.Width;
        int originalHeight = originalBitmap.Height;
        int rotatedWidth = (int)(cosine * originalWidth + sine * originalHeight);
        int rotatedHeight = (int)(cosine * originalHeight + sine * originalWidth);

        rotatedBitmap = new SKBitmap(rotatedWidth, rotatedHeight);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear(SKColors.LightPink);
            canvas.Translate(rotatedWidth / 2, rotatedHeight / 2);
            canvas.RotateDegrees((float)angle);
            canvas.Translate(-originalWidth / 2, -originalHeight / 2);
            canvas.DrawBitmap(originalBitmap, new SKPoint());
        }

        canvasView.InvalidateSurface();
    }
}
```

합니다 `ValueChanged` 처리기는 `Slider` 새 작업을 수행 `rotatedBitmap` 회전 각도 기반 합니다. 새 너비와 높이 기반한 absolute values의 사인 및 코사인 원래 너비 및 높이입니다. 회전된 된 비트맵에 원래 비트맵을 그릴 때 사용 되는 변환 각도 지정된 된 수 만큼 회전 고 회전된 된 비트맵의 가운데에는 center 번역할 원래 비트맵 center 원점으로 이동 합니다. (합니다 `Translate` 고 `RotateDegrees` 메서드 적용 되는 방식을 보다 반대 순서로 호출 됩니다.)

사용 하 여는 `Clear` 메서드를 배경의 `rotatedBitmap` 연한 분홍. 크기를 보여 주기 위해 전적으로 이것이 `rotatedBitmap` 디스플레이에서:

[![Rotator 비트맵](drawing-images/BitmapRotator.png "비트맵 Rotator")](drawing-images/BitmapRotator-Large.png#lightbox)

회전된 된 비트맵 이하의 하지만 전체 원래 비트맵을 포함할 만큼 큰 경우

## <a name="flipping-bitmaps"></a>비트맵을 대칭 이동

일반적으로 비트맵에서 수행 하는 다른 작업 이라고 _대칭 이동_합니다. 개념적으로 비트맵 세로 축 또는 center 비트맵의 가로 축 주위 3 차원에서 회전 합니다. 수직 대칭 이동 미러 이미지를 만듭니다.

**비트맵 플리퍼** 페이지에 **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** 응용 프로그램에서는 이러한 프로세스를 보여 줍니다. XAML 파일에는 `SKCanvasView` 세로 및 가로로 대칭 이동에 대 한 두 개의 단추:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.BitmapFlipperPage"
             Title="Bitmap Flipper">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        
        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Button Text="Flip Vertical"
                Grid.Row="1" Grid.Column="0"
                Margin="0, 10"
                Clicked="OnFlipVerticalClicked" />

        <Button Text="Flip Horizontal"
                Grid.Row="1" Grid.Column="1"
                Margin="0, 10"
                Clicked="OnFlipHorizontalClicked" />
    </Grid>
</ContentPage>
```

이러한 두 작업을 구현 하는 코드 숨김 파일은 `Clicked` 단추에 대 한 처리기: 

```csharp
public partial class BitmapFlipperPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(BitmapRotatorPage),
            "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    public BitmapFlipperPage()
    {
        InitializeComponent();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnFlipVerticalClicked(object sender, ValueChangedEventArgs args)
    {
        SKBitmap flippedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

        using (SKCanvas canvas = new SKCanvas(flippedBitmap))
        {
            canvas.Clear();
            canvas.Scale(-1, 1, bitmap.Width / 2, 0);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = flippedBitmap;
        canvasView.InvalidateSurface();
    }

    void OnFlipHorizontalClicked(object sender, ValueChangedEventArgs args)
    {
        SKBitmap flippedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

        using (SKCanvas canvas = new SKCanvas(flippedBitmap))
        {
            canvas.Clear();
            canvas.Scale(1, -1, 0, bitmap.Height / 2);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = flippedBitmap;
        canvasView.InvalidateSurface();
    }
}
```

수직 대칭 이동의 가로 배율 인수를 사용 하 여 크기 조정 변환을 통해 수행 됩니다 &ndash;1입니다. 크기 조정 center 경우 비트맵의 세로 가운데 수평 대칭 이동 후에는의 세로 배율 인수를 사용 하 여 크기 조정 변환 &ndash;1입니다. 

Monkey의 shirt 시 역방향된 처리에서 보듯이 대칭 이동 회전 같지는 않습니다. 이지만 오른쪽 UWP 스크린샷에서 보여 주듯이 둘 다가 가로 및 세로로 대칭 이동 180도 회전와 동일 합니다.

[![플리퍼 비트맵](drawing-images/BitmapFlipper.png "플리퍼 비트맵")](drawing-images/BitmapFlipper-Large.png#lightbox)

비슷한 기술을 사용 하 여 처리할 수 있는 다른 일반적인 작업 비트맵 사각형 하위 집합을 자르기 됩니다. 이 다음 문서에서 설명 [ **자르기 SkiaSharp 비트맵**](cropping.md)합니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
