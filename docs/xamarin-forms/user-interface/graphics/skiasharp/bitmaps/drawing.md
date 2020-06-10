---
제목: "SkiaSharp 비트맵에서 만들기 및 그리기" 설명: "SkiaSharp 비트맵을 만들고이 비트맵을 기반으로 캔버스를 만들어이 비트맵을 그리는 방법에 대해 알아봅니다."
ms. prod: xamarin. 기술: xamarin-skiasharp assetid: 79BD3266-D457-4E50-BDDF-33450035FA0F author: davidbritch: dabritch: 07/17/2018:-loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="creating-and-drawing-on-skiasharp-bitmaps"></a>SkiaSharp 비트맵 만들기 및 그리기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

응용 프로그램에서 웹, 응용 프로그램 리소스 및 사용자의 사진 라이브러리에서 비트맵을 로드 하는 방법을 살펴보았습니다. 또한 응용 프로그램 내에서 새 비트맵을 만들 수 있습니다. 가장 간단한 방법은의 생성자 중 하나입니다 [`SKBitmap`](xref:SkiaSharp.SKBitmap.%23ctor(System.Int32,System.Int32,System.Boolean)) .

```csharp
SKBitmap bitmap = new SKBitmap(width, height);
```

`width`및 `height` 매개 변수는 정수 이며 비트맵의 픽셀 크기를 지정 합니다. 이 생성자는 픽셀당 4 바이트 (빨강, 녹색, 파랑 및 알파 (불투명도) 구성 요소 당 1 바이트)를 사용 하 여 전체 색 비트맵을 만듭니다.

새 비트맵을 만든 후에는 비트맵의 표면에 항목을 가져와야 합니다. 일반적으로 다음 두 가지 방법 중 하나로이 작업을 수행 합니다.

- 표준 그리기 메서드를 사용 하 여 비트맵을 그립니다 `Canvas` .
- 픽셀 비트에 직접 액세스 합니다.

이 문서에서는 첫 번째 방법을 보여 줍니다.

![그리기 샘플](drawing-images/DrawingSample.png "그리기 샘플")

두 번째 방법은 [**SkiaSharp 비트맵 픽셀 액세스**](pixel-bits.md)문서에서 설명 합니다.

## <a name="drawing-on-the-bitmap"></a>비트맵에서 그리기

비트맵 표면의 그리기는 비디오 디스플레이에서 그리기와 동일 합니다. 비디오 디스플레이를 그리려면 `SKCanvas` 이벤트 인수에서 개체를 가져옵니다 `PaintSurface` . 비트맵을 그리려면 `SKCanvas` 생성자를 사용 하 여 개체를 만듭니다 [`SKCanvas`](xref:SkiaSharp.SKCanvas.%23ctor(SkiaSharp.SKBitmap)) .

```csharp
SKCanvas canvas = new SKCanvas(bitmap);
```

비트맵에서 그리기를 완료 하 고 나면 개체를 삭제할 수 있습니다 `SKCanvas` . 이러한 이유로 `SKCanvas` 생성자는 일반적으로 문에서 호출 됩니다 `using` .

```csharp
using (SKCanvas canvas = new SKCanvas(bitmap))
{
    ··· // call drawing function
}
```

그런 다음 비트맵을 표시할 수 있습니다. 나중에 프로그램은 `SKCanvas` 동일한 비트맵을 기반으로 새 개체를 만들고이 개체에 좀 더 그릴 수 있습니다.

**[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** 응용 프로그램의 **hello 비트맵** 페이지에서 "hello, Bitmap!" 텍스트를 씁니다. 비트맵을 찾은 다음이 비트맵을 여러 번 표시 합니다.

의 생성자는 `HelloBitmapPage` `SKPaint` 텍스트를 표시 하기 위한 개체를 만들기 시작 합니다. 텍스트 문자열의 크기를 결정 하 고 이러한 차원을 사용 하 여 비트맵을 만듭니다. 그런 다음이 `SKCanvas` 비트맵을 기반으로 개체를 만들고를 호출한 `Clear` 다음을 호출 `DrawText` 합니다. `Clear`새로 만든 비트맵에 임의의 데이터가 포함 될 수 있으므로 항상 새 비트맵을 사용 하 여를 호출 하는 것이 좋습니다.

생성자는 개체를 만들어 `SKCanvasView` 비트맵을 표시 합니다.

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

`PaintSurface`처리기는 표시의 행과 열에서 비트맵을 여러 번 렌더링 합니다. `Clear`처리기의 메서드에는 `PaintSurface` `SKColors.Aqua` 표시 표면의 배경을 색으로 하는의 인수가 있습니다.

[![Hello, Bitmap!](drawing-images/HelloBitmap.png "Hello, Bitmap!")](drawing-images/HelloBitmap-Large.png#lightbox)

바다색 배경의 모양은 텍스트를 제외 하 고 비트맵이 투명 함을 나타냅니다.

## <a name="clearing-and-transparency"></a>지우기 및 투명성

**Hello 비트맵** 페이지가 표시 되 면 프로그램이 만든 비트맵이 검정색 텍스트를 제외 하 고 투명 하 게 표시 됩니다. 표시 표면의 바다색 색은를 통해 표시 됩니다.

의 메서드에 대 한 설명서에서는 `Clear` `SKCanvas` "현재 클립 캔버스의 모든 픽셀을 바꿉니다." 문을 사용 하 여 설명 합니다. "Replace" 단어를 사용 하면 이러한 메서드의 중요 한 특성이 표시 됩니다. 모든 그리기 메서드는 `SKCanvas` 기존 표시 화면에 항목을 추가 합니다. `Clear`메서드는 이미 있는 항목을 _대체_ 합니다.

`Clear`는 두 가지 다른 버전으로 존재 합니다.

- [`Clear`](xref:SkiaSharp.SKCanvas.Clear(SkiaSharp.SKColor))매개 변수가 있는 메서드는 `SKColor` 표시 표면의 픽셀을 해당 색의 픽셀로 바꿉니다.

- [`Clear`](xref:SkiaSharp.SKCanvas.Clear)매개 변수가 없는 메서드는 픽셀을 색으로 바꿉니다 [`SKColors.Empty`](xref:SkiaSharp.SKColors.Empty) .이 색은 모든 구성 요소 (빨강, 녹색, 파랑 및 알파)가 0으로 설정 되는 색입니다. 이 색을 "투명 검은색"이 라고도 합니다.

`Clear`새 비트맵에서 인수를 사용 하지 않고를 호출 하면 전체 비트맵이 완전히 투명 하 게 초기화 됩니다. 이후에 비트맵에 그려진 모든 항목은 일반적으로 불투명 하거나 부분적으로 불투명 합니다.

사용해 볼 수 있는 작업은 다음과 같습니다. **Hello 비트맵** 페이지에서에 `Clear` 적용 되는 메서드를 `bitmapCanvas` 이 항목으로 바꿉니다.

```csharp
bitmapCanvas.Clear(new SKColor(255, 0, 0, 128));
```

`SKColor`생성자 매개 변수의 순서는 빨강, 녹색, 파랑 및 알파 이며 각 값의 범위는 0 ~ 255입니다. 알파 값 0은 투명 하지만 알파 값 255은 불투명 합니다.

값 (255, 0, 0, 128)은 비트맵 픽셀을 50% 불투명도를 사용 하는 빨강 픽셀로 지웁니다. 즉, 비트맵 배경은 반 투명 합니다. 비트맵의 반투명 빨강 배경은 표시 표면의 바다색 배경과 결합 되어 회색 배경을 만듭니다.

이니셜라이저에 다음 할당을 추가 하 여 텍스트 색을 투명 검정으로 설정 해 봅니다 `SKPaint` .

```csharp
Color = new SKColor(0, 0, 0, 0)
```

이 투명 텍스트는 표시 화면의 바다색 배경을 표시 하는 데 사용할 수 있는 완전히 투명 한 비트맵 영역을 만드는 것으로 생각할 수 있습니다. 이는 그렇지 않습니다. 텍스트는 비트맵에 이미 있는 내용 위에 그려집니다. 투명 텍스트는 전혀 표시 되지 않습니다.

어떤 `Draw` 방법으로도 비트맵을 더 투명 하 게 만들 수 있습니다. `Clear`이 작업을 수행할 수 있습니다.

## <a name="bitmap-color-types"></a>비트맵 색 형식

가장 간단한 `SKBitmap` 생성자를 사용 하면 비트맵의 정수 픽셀 너비와 높이를 지정할 수 있습니다. 다른 `SKBitmap` 생성자는 더 복잡 합니다. 이러한 생성자에는 및 라는 두 열거형 형식의 인수가 필요 [`SKColorType`](xref:SkiaSharp.SKColorType) [`SKAlphaType`](xref:SkiaSharp.SKAlphaType) 합니다. 다른 생성자는 [`SKImageInfo`](xref:SkiaSharp.SKImageInfo) 이 정보를 통합 하는 구조를 사용 합니다.

열거형에는 `SKColorType` 9 개의 멤버가 있습니다. 이러한 각 멤버는 비트맵 픽셀을 저장 하는 특정 방법을 설명 합니다.

- `Unknown`
- `Alpha8`&mdash;각 픽셀은 완전히 투명 하 고 완전히 불투명 한 알파 값을 나타내는 8 비트입니다.
- `Rgb565`&mdash;각 픽셀은 16 비트, 빨강 및 파랑의 경우 5 비트, 녹색의 경우 6 비트입니다.
- `Argb4444`&mdash;각 픽셀은 16 비트, 알파, 빨강, 녹색, 파랑 각각 4입니다.
- `Rgba8888`&mdash;각 픽셀은 32 비트, 빨강, 녹색, 파랑 및 알파의 8 비트입니다.
- `Bgra8888`&mdash;각 픽셀은 32 비트, 각각 파랑, 녹색, 빨강 및 알파에 대 한 8 비트입니다.
- `Index8`&mdash;각 픽셀은 8 비트 이며에 대 한 인덱스를 나타냅니다.[`SKColorTable`](xref:SkiaSharp.SKColorTable)
- `Gray8`&mdash;각 픽셀은 검정에서 흰색으로 회색 음영을 나타내는 8 비트입니다.
- `RgbaF16`&mdash;각 픽셀은 16 비트 부동 소수점 형식의 빨강, 녹색, 파랑 및 알파를 사용 하는 64 비트입니다.

각 픽셀이 32 픽셀 (4 바이트) 인 두 가지 형식은 일반적 _으로 전체 색_ 서식 이라고 합니다. 비디오 디스플레이 자체가 전체 색이 아닌 경우의 다른 많은 형식이 날짜입니다. 제한 된 색의 비트맵은 이러한 표시 및 허용 된 비트맵에서 메모리의 공간을 절약 하기에 적합 했습니다.

프로그래머는 거의 항상 전체 색 비트맵을 사용 하 고 다른 형식을 사용 하지 마세요. 예외는 `RgbaF16` 전체 색 서식 보다 색을 더 크게 확인할 수 있는 형식입니다. 그러나이 형식은 의료 이미지와 같은 특수 한 용도로 사용 되며 표준 전체 색 표시와 함께 사용 하는 경우에는 적합 하지 않습니다.

이러한 일련의 문서는 `SKBitmap` 멤버가 지정 되지 않은 경우 기본적으로 사용 되는 색 형식으로 제한 됩니다 `SKColorType` . 기본 형식은 기본 플랫폼을 기반으로 합니다. 에서 지 원하는 플랫폼의 경우 Xamarin.Forms 기본 색 형식은 다음과 같습니다.

- `Rgba8888`iOS 및 Android의 경우
- `Bgra8888`UWP의 경우

유일한 차이점은 메모리에서 4 바이트의 순서 이며이는 픽셀 비트에 직접 액세스 하는 경우에만 문제가 됩니다. [**SkiaSharp 비트맵 픽셀에 액세스**](pixel-bits.md)하는 문서에 도달할 때까지이는 중요 하지 않습니다.

열거형에는 `SKAlphaType` 네 개의 멤버가 있습니다.

- `Unknown`
- `Opaque`&mdash;비트맵에 투명도가 없습니다.
- `Premul`&mdash;색 구성 요소에 알파 구성 요소가 미리 곱해집니다.
- `Unpremul`&mdash;색 구성 요소가 알파 구성 요소에 의해 미리 곱해집니다.

다음은 빨강, 녹색, 파랑, 알파 순서로 표시 되는 바이트를 50% 투명 한 4 바이트 빨강 비트맵 픽셀입니다.

0xFF 0x00 0x00 0x80

반투명 픽셀을 포함 하는 비트맵이 디스플레이 표면에서 렌더링 되는 경우 각 비트맵 픽셀의 색 구성 요소를 해당 픽셀의 알파 값으로 곱하고, 표시 표면에 해당 하는 픽셀의 색 구성 요소에 255을 곱하여 알파 값을 뺀 값 이어야 합니다. 그런 다음 두 픽셀을 결합할 수 있습니다. 비트맵 픽셀의 색 구성 요소가 알파 값으로 미리 mulitplied 이미 있는 경우 비트맵을 더 빠르게 렌더링할 수 있습니다. 동일한 빨강 픽셀은 미리 곱하기 형식으로 다음과 같이 저장 됩니다.

0x80 0x00 0x00 0x80

이러한 성능 향상으로 `SkiaSharp` 인해 기본적으로 비트맵이 형식으로 생성 됩니다 `Premul` . 그러나 픽셀 비트에 액세스 하 고 조작 하는 경우에만이를 확인 해야 합니다.

## <a name="drawing-on-existing-bitmaps"></a>기존 비트맵에서 그리기

그릴 새 비트맵을 만들 필요는 없습니다. 기존 비트맵을 그릴 수도 있습니다.

**원숭이 콧수염** 페이지는 해당 생성자를 사용 하 여 **monkeyface .png** 이미지를 로드 합니다. 그런 다음이 `SKCanvas` 비트맵을 기반으로 개체를 만들고 및 개체를 사용 하 여 `SKPaint` `SKPath` 콧수염을 그립니다.

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

생성자는 `SKCanvasView` `PaintSurface` 처리기가 단순히 결과를 표시 하는를 만들어 종료 합니다.

[![원숭이 콧수염](drawing-images/MonkeyMoustache.png "원숭이 콧수염")](drawing-images/MonkeyMoustache-Large.png#lightbox)

## <a name="copying-and-modifying-bitmaps"></a>비트맵 복사 및 수정

`SKCanvas`비트맵에 그리는 데 사용할 수 있는 메서드는 다음과 같습니다 `DrawBitmap` . 즉, 한 비트맵을 다른 비트맵으로 그릴 수 있으며, 일반적으로 일부 비트맵을 수정 합니다.

비트맵을 수정 하는 가장 다양 한 방법은 실제 픽셀 비트 ( **[SkiaSharp 비트맵 픽셀 액세스](pixel-bits.md)** 문서에 설명 된 주제)에 액세스 하는 것입니다. 그러나 픽셀 비트에 액세스 하지 않아도 되는 비트맵을 수정 하는 다른 여러 가지 방법이 있습니다.

**[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** 응용 프로그램에 포함 된 다음 비트맵은 360 픽셀 너비 및 480 픽셀 높이로 되어 있습니다.

![산지 Climbers](drawing-images/MountainClimbers.jpg "산지 Climbers")

왼쪽의 원숭이에서이 사진을 게시할 수 있는 권한을 받지 않았다고 가정 합니다. 한 가지 해결책은 _pixelization_이라는 기법을 사용 하 여 원숭이의 얼굴을 숨기는 것입니다. 글꼴의 픽셀은 색의 블록으로 대체 되므로 기능을 만들 수 없습니다. 색의 블록은 일반적으로 이러한 블록에 해당 하는 픽셀 색의 평균을 계산 하 여 원래 이미지에서 파생 됩니다. 그러나이 평균을 직접 수행할 필요는 없습니다. 비트맵을 더 작은 픽셀 차원에 복사 하면 자동으로 발생 합니다.

왼쪽 원숭이 얼굴은 (112, 238) 점에서 왼쪽 위 모퉁이가 있는 72 픽셀 정사각형 영역을 차지 합니다. 72 픽셀 사각형 영역을 색이 지정 된 블록의 9 x 9 배열로 바꿔 보겠습니다. 각 색은 8 x 픽셀 사각형입니다.

**Pixelize 이미지** 페이지는 해당 비트맵에서 로드 되며 먼저 라는 작은 9 픽셀 사각형 비트맵을 만듭니다 `faceBitmap` . 이는 원숭이의 얼굴을 복사 하기 위한 대상입니다. 대상 사각형은 단순히 9 픽셀 사각형 이지만 소스 사각형은 72 픽셀 사각형입니다. 원본 픽셀의 8 x 8 블록 마다 색의 평균을 계산 하 여 한 픽셀씩 통합 됩니다.

다음 단계는 라는 동일한 크기의 새 비트맵으로 원래 비트맵을 복사 하는 것입니다 `pixelizedBitmap` . `faceBitmap`그런 다음 작은가 72 픽셀 사각형 대상 사각형을 사용 하 여 그 위에 복사 되므로의 각 픽셀 `faceBitmap` 크기가 8 배까지 확장 됩니다.

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

생성자는를 만들어 결과를 `SKCanvasView` 표시 합니다.

[![Pixelize 이미지](drawing-images/PixelizeImage.png "Pixelize 이미지")](drawing-images/PixelizeImage-Large.png#lightbox)

## <a name="rotating-bitmaps"></a>비트맵 회전

또 다른 일반적인 작업은 비트맵을 회전 하는 것입니다. 이 기능은 iPhone 또는 iPad 사진 라이브러리에서 비트맵을 검색할 때 특히 유용 합니다. 사진이 촬영 될 때 특정 방향으로 장치를 보유 하지 않은 경우 사진은 거꾸로 또는 옆쪽으로 진행 될 수 있습니다.

비트맵을 뒤집어 만들려면 첫 번째 비트맵 크기와 동일한 크기의 다른 비트맵을 만든 다음 첫 번째를 두 번째에 복사 하는 동안 180도 회전 하도록 변환을 설정 해야 합니다. 이 단원의 모든 예제에서 `bitmap` 는 `SKBitmap` 회전 해야 하는 개체입니다.

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.RotateDegrees(180, bitmap.Width / 2, bitmap.Height / 2);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

90 도씩 회전 하는 경우 높이와 너비를 바꿔 원래 크기와 다른 비트맵을 만들어야 합니다. 예를 들어 원래 비트맵이 1200 픽셀 너비와 800 픽셀 높으면 회전 된 비트맵은 800 픽셀 너비와 1200 픽셀 너비입니다. 비트맵이 왼쪽 위 모퉁이를 중심으로 회전 한 다음 뷰로 이동 하도록 변환 및 회전을 설정 합니다. 및 메서드는 적용 되는 `Translate` `RotateDegrees` 순서와 반대 방향으로 호출 됩니다. 90도 시계 방향으로 회전 하는 코드는 다음과 같습니다.

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

여기에는 90도 시계 반대 방향으로 회전 하는 비슷한 기능이 있습니다.

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

이러한 두 가지 방법은 [**SkiaSharp 비트맵 자르기**](cropping.md#cropping-skiasharp-bitmaps)문서에 설명 된 **사진 퍼즐** 페이지에서 사용 됩니다.

사용자가 90 도씩 비트맵을 회전 하는 데 사용할 수 있는 프로그램은 90 도씩 회전 하기 위한 함수 하나를 구현 해야 합니다. 그런 다음이 함수를 반복적으로 실행 하 여 90 각도의 증분을 회전할 수 있습니다.

프로그램은 임의의 양만큼 비트맵을 회전할 수도 있습니다. 한 가지 간단한 방법은 180을 일반화 된 변수로 바꿔 180 각도로 회전 하는 함수를 수정 하는 것입니다 `angle` .

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.RotateDegrees(angle, bitmap.Width / 2, bitmap.Height / 2);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

그러나 일반적으로이 논리는 회전 된 비트맵의 모퉁이를 자릅니다. 더 나은 방법은 삼각를 사용 하 여 회전 된 비트맵의 크기를 계산 하 여 해당 모퉁이를 포함 하는 것입니다.

이 삼각법는 **비트맵 회전자** 페이지에 표시 됩니다. XAML 파일은 `SKCanvasView` `Slider` 현재 값을 표시 하는 0에서 360 까지의 범위에 해당 하는 및를 인스턴스화합니다 `Label` .

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

코드 숨김이 파일은 비트맵 리소스를 로드 하 고 라는 정적 읽기 전용 필드로 저장 합니다 `originalBitmap` . 처리기에 표시 되는 비트맵은 이며 `PaintSurface` `rotatedBitmap` , 처음에는로 설정 됩니다 `originalBitmap` .

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

`ValueChanged`의 처리기는 `Slider` `rotatedBitmap` 회전 각도를 기준으로 새를 만드는 작업을 수행 합니다. 새 너비와 높이는 원래 너비와 높이의 사인 및 코사인의 절대값을 기준으로 합니다. 회전 된 비트맵에서 원래 비트맵을 그리는 데 사용 되는 변환은 원본 비트맵 중심을 원본으로 이동한 다음 지정 된 각도 만큼 회전 하 고 해당 중심을 회전 된 비트맵의 중심으로 변환 합니다. `Translate`및 메서드는 `RotateDegrees` 적용 되는 방식과 반대 순서로 호출 됩니다.

메서드를 사용 하 여 `Clear` 연한 분홍의 배경을 만듭니다 `rotatedBitmap` . 이는 표시의 크기를 보여 주기 위한 것입니다 `rotatedBitmap` .

[![비트맵 회전자](drawing-images/BitmapRotator.png "비트맵 회전자")](drawing-images/BitmapRotator-Large.png#lightbox)

회전 된 비트맵은 전체 원본 비트맵을 포함할 만큼 충분히 크지만 더 크지 않습니다.

## <a name="flipping-bitmaps"></a>비트맵 대칭 이동

비트맵에서 일반적으로 수행 되는 다른 작업을 _대칭 이동_이라고 합니다. 개념적으로 비트맵은 세로 축 둘레의 3 차원 또는 비트맵의 중심을 통한 가로 축에서 회전 됩니다. 수직 대칭 이동은 미러 이미지를 만듭니다.

**[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** 응용 프로그램의 **비트맵 플리퍼** 페이지는 이러한 프로세스를 보여 줍니다. XAML 파일에는 `SKCanvasView` 가로 및 세로로 대칭 이동 하기 위한 및 두 개의 단추가 포함 되어 있습니다.

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

코드에 대 한 처리기에서 이러한 두 작업을 구현 하는 코드 숨김이 `Clicked` 있습니다.

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

수직 대칭 이동은 가로 크기 조정 인수가 1 인 크기 조정 변환에 의해 수행 됩니다 &ndash; . 크기 조정 중심은 비트맵의 세로 가운데입니다. 수평 대칭 이동은 세로 배율 인수가 1 인 크기 조정 변환입니다 &ndash; .

원숭이의 셔츠에서 역순으로 표시 되는 것 처럼 대칭 이동은 회전과 동일 하지 않습니다. 하지만 오른쪽의 UWP 스크린샷에서는 가로 및 세로 방향으로 대칭 이동 하는 것이 180도를 회전 하는 것과 같습니다.

[![비트맵 플리퍼](drawing-images/BitmapFlipper.png "비트맵 플리퍼")](drawing-images/BitmapFlipper-Large.png#lightbox)

유사한 기법을 사용 하 여 처리할 수 있는 또 다른 일반적인 작업은 비트맵을 사각형 하위 집합으로 자르는 것입니다. 이 내용은 다음 문서 [**SkiaSharp 비트맵 자르기**](cropping.md)에 설명 되어 있습니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
