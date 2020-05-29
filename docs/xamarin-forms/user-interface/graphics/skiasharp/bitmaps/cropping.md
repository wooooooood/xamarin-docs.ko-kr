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
ms.openlocfilehash: 6c5e340818b702d79a1157f29c1ecec19bf1db76
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139947"
---
# <a name="cropping-skiasharp-bitmaps"></a>SkiaSharp 비트맵 자르기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

[**SkiaSharp 비트맵 만들기 및 그리기**](drawing.md) 문서에서는 `SKBitmap` 개체를 생성자에 전달 하는 방법을 설명 `SKCanvas` 했습니다. 해당 캔버스에 대해 호출 된 모든 그리기 메서드로 인해 그래픽이 비트맵에서 렌더링 됩니다. 이러한 그리기 메서드는 `DrawBitmap` 를 포함 합니다. 즉,이 기술을 사용 하면 변환 적용 시 한 비트맵의 일부 또는 전체를 다른 비트맵으로 전송할 수 있습니다.

[`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint))소스 및 대상 사각형을 사용 하 여 메서드를 호출 하 여 비트맵을 자르는 데이 방법을 사용할 수 있습니다.

```csharp
canvas.DrawBitmap(bitmap, sourceRect, destRect);
```

그러나 잘라내기를 구현 하는 응용 프로그램은 종종 사용자가 자르기 사각형을 대화형으로 선택 하기 위한 인터페이스를 제공 합니다.

![자르기 샘플](cropping-images/CroppingSample.png "자르기 샘플")

이 문서에서는 해당 인터페이스에 대해 집중적으로 설명 합니다.

## <a name="encapsulating-the-cropping-rectangle"></a>자르기 사각형 캡슐화

이라는 클래스에서 자르기 논리를 분리 하는 것이 유용 `CroppingRectangle` 합니다. 생성자 매개 변수에는 최대 사각형 (일반적으로 잘라내는 비트맵의 크기 및 가로 세로 비율)이 포함 됩니다. 생성자는 먼저 형식의 속성에서 공용으로 설정 되는 초기 자르기 사각형을 정의 합니다 `Rect` `SKRect` . 이 초기 자르기 사각형은 비트맵 사각형의 너비와 높이의 80% 이지만 가로 세로 비율을 지정 하면 조정 됩니다.

```csharp
class CroppingRectangle
{
    ···
    SKRect maxRect;             // generally the size of the bitmap
    float? aspectRatio;

    public CroppingRectangle(SKRect maxRect, float? aspectRatio = null)
    {
        this.maxRect = maxRect;
        this.aspectRatio = aspectRatio;

        // Set initial cropping rectangle
        Rect = new SKRect(0.9f * maxRect.Left + 0.1f * maxRect.Right,
                          0.9f * maxRect.Top + 0.1f * maxRect.Bottom,
                          0.1f * maxRect.Left + 0.9f * maxRect.Right,
                          0.1f * maxRect.Top + 0.9f * maxRect.Bottom);

        // Adjust for aspect ratio
        if (aspectRatio.HasValue)
        {
            SKRect rect = Rect;
            float aspect = aspectRatio.Value;

            if (rect.Width > aspect * rect.Height)
            {
                float width = aspect * rect.Height;
                rect.Left = (maxRect.Width - width) / 2;
                rect.Right = rect.Left + width;
            }
            else
            {
                float height = rect.Width / aspect;
                rect.Top = (maxRect.Height - height) / 2;
                rect.Bottom = rect.Top + height;
            }

            Rect = rect;
        }
    }
    
    public SKRect Rect { set; get; }
    ···
}
```

사용할 수 있는 유용한 정보 중 하나는 `CroppingRectangle` `SKPoint` 왼쪽 위, 오른쪽 위, 오른쪽 아래 및 왼쪽 아래 순서 대로 자르기 사각형의 네 모퉁이에 해당 하는 값의 배열입니다.

```csharp
class CroppingRectangle
{
    ···
    public SKPoint[] Corners
    {
        get
        {
            return new SKPoint[]
            {
                new SKPoint(Rect.Left, Rect.Top),
                new SKPoint(Rect.Right, Rect.Top),
                new SKPoint(Rect.Right, Rect.Bottom),
                new SKPoint(Rect.Left, Rect.Bottom)
            };
        }
    }
    ···
}
```

이 배열은를 호출 하는 다음 메서드에서 사용 됩니다 `HitTest` . `SKPoint`매개 변수는 손가락 터치 또는 마우스 클릭에 해당 하는 지점입니다. 메서드는 매개 변수로 지정 된 거리 내에서 손가락 또는 마우스 포인터가 접촉 한 모퉁이에 해당 하는 인덱스 (0, 1, 2 또는 3)를 반환 합니다 `radius` . 

```csharp
class CroppingRectangle
{
    ···
    public int HitTest(SKPoint point, float radius)
    {
        SKPoint[] corners = Corners;

        for (int index = 0; index < corners.Length; index++)
        {
            SKPoint diff = point - corners[index];
                
            if ((float)Math.Sqrt(diff.X * diff.X + diff.Y * diff.Y) < radius)
            {
                return index;
            }
        }

        return -1;
    }
    ···
}
```

터치 또는 마우스 지점이 모퉁이의 단위 내에 있지 않은 경우이 `radius` 메서드는 1을 반환 &ndash; 합니다.

의 마지막 메서드는 `CroppingRectangle` `MoveCorner` 터치 또는 마우스 이동에 대 한 응답으로 호출 되는입니다. 두 매개 변수는 이동 중인 모퉁이의 인덱스와 해당 모퉁이의 새 위치를 표시 합니다. 메서드의 첫 번째 절반은 모퉁이의 새 위치를 기준으로 자르기 사각형을 조정 하지만 항상 비트맵 크기인의 범위 내에 `maxRect` 있습니다. 또한이 논리 `MINIMUM` 는 자르기 사각형을 아무 것도 축소 하지 않도록 필드의 계정을 사용 합니다.

```csharp
class CroppingRectangle
{
    const float MINIMUM = 10;   // pixels width or height
    ···
    public void MoveCorner(int index, SKPoint point)
    {
        SKRect rect = Rect;

        switch (index)
        {
            case 0: // upper-left
                rect.Left = Math.Min(Math.Max(point.X, maxRect.Left), rect.Right - MINIMUM);
                rect.Top = Math.Min(Math.Max(point.Y, maxRect.Top), rect.Bottom - MINIMUM);
                break;

            case 1: // upper-right
                rect.Right = Math.Max(Math.Min(point.X, maxRect.Right), rect.Left + MINIMUM);
                rect.Top = Math.Min(Math.Max(point.Y, maxRect.Top), rect.Bottom - MINIMUM);
                break;

            case 2: // lower-right
                rect.Right = Math.Max(Math.Min(point.X, maxRect.Right), rect.Left + MINIMUM);
                rect.Bottom = Math.Max(Math.Min(point.Y, maxRect.Bottom), rect.Top + MINIMUM);
                break;

            case 3: // lower-left
                rect.Left = Math.Min(Math.Max(point.X, maxRect.Left), rect.Right - MINIMUM);
                rect.Bottom = Math.Max(Math.Min(point.Y, maxRect.Bottom), rect.Top + MINIMUM);
                break;
        }

        // Adjust for aspect ratio
        if (aspectRatio.HasValue)
        {
            float aspect = aspectRatio.Value;

            if (rect.Width > aspect * rect.Height)
            {
                float width = aspect * rect.Height;

                switch (index)
                {
                    case 0:
                    case 3: rect.Left = rect.Right - width; break;
                    case 1:
                    case 2: rect.Right = rect.Left + width; break;
                }
            }
            else
            {
                float height = rect.Width / aspect;

                switch (index)
                {
                    case 0:
                    case 1: rect.Top = rect.Bottom - height; break;
                    case 2:
                    case 3: rect.Bottom = rect.Top + height; break;
                }
            }
        }

        Rect = rect;
    }
}
```

메서드의 두 번째 절반은 선택적 가로 세로 비율에 맞게 조정 됩니다.

이 클래스의 모든 항목은 픽셀 단위를 염두에 두어야 합니다.

## <a name="a-canvas-view-just-for-cropping"></a>자르기만을 위한 캔버스 뷰

`CroppingRectangle`방금 살펴본 클래스는 `PhotoCropperCanvasView` 에서 파생 되는 클래스에서 사용 됩니다 `SKCanvasView` . 이 클래스는 자르기 사각형의 변경에 대 한 터치 또는 마우스 이벤트를 처리 하는 것 뿐만 아니라 비트맵과 자르기 사각형을 표시 합니다.

생성자에는 `PhotoCropperCanvasView` 비트맵이 필요 합니다. 가로 세로 비율은 선택 사항입니다. 생성자는 `CroppingRectangle` 이 비트맵 및 가로 세로 비율을 기반으로 형식의 개체를 인스턴스화하고 필드로 저장 합니다.

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    ···
    public PhotoCropperCanvasView(SKBitmap bitmap, float? aspectRatio = null)
    {
        this.bitmap = bitmap;

        SKRect bitmapRect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
        croppingRect = new CroppingRectangle(bitmapRect, aspectRatio);
        ···
    }
    ···
}
```

이 클래스는에서 파생 되므로 `SKCanvasView` 이벤트에 대 한 처리기를 설치할 필요가 없습니다 `PaintSurface` . 대신 메서드를 재정의할 수 있습니다 `OnPaintSurface` . 메서드는 비트맵을 표시 하 고 필드로 저장 된 몇 가지 개체를 사용 하 여 `SKPaint` 현재 자르기 사각형을 그립니다.

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    const int CORNER = 50;      // pixel length of cropper corner
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    SKMatrix inverseBitmapMatrix;
    ···
    // Drawing objects
    SKPaint cornerStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.White,
        StrokeWidth = 10
    };

    SKPaint edgeStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.White,
        StrokeWidth = 2
    };
    ···
    protected override void OnPaintSurface(SKPaintSurfaceEventArgs args)
    {
        base.OnPaintSurface(args);

        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Gray);

        // Calculate rectangle for displaying bitmap 
        float scale = Math.Min((float)info.Width / bitmap.Width, (float)info.Height / bitmap.Height);
        float x = (info.Width - scale * bitmap.Width) / 2;
        float y = (info.Height - scale * bitmap.Height) / 2;
        SKRect bitmapRect = new SKRect(x, y, x + scale * bitmap.Width, y + scale * bitmap.Height);
        canvas.DrawBitmap(bitmap, bitmapRect);

        // Calculate a matrix transform for displaying the cropping rectangle
        SKMatrix bitmapScaleMatrix = SKMatrix.MakeIdentity();
        bitmapScaleMatrix.SetScaleTranslate(scale, scale, x, y);

        // Display rectangle
        SKRect scaledCropRect = bitmapScaleMatrix.MapRect(croppingRect.Rect);
        canvas.DrawRect(scaledCropRect, edgeStroke);

        // Display heavier corners
        using (SKPath path = new SKPath())
        {
            path.MoveTo(scaledCropRect.Left, scaledCropRect.Top + CORNER);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Left + CORNER, scaledCropRect.Top);

            path.MoveTo(scaledCropRect.Right - CORNER, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Top + CORNER);

            path.MoveTo(scaledCropRect.Right, scaledCropRect.Bottom - CORNER);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Right - CORNER, scaledCropRect.Bottom);

            path.MoveTo(scaledCropRect.Left + CORNER, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Bottom - CORNER);

            canvas.DrawPath(path, cornerStroke);
        }

        // Invert the transform for touch tracking
        bitmapScaleMatrix.TryInvert(out inverseBitmapMatrix);
    }
    ···
}
```

클래스의 코드는 `CroppingRectangle` 자르기 사각형을 비트맵의 픽셀 크기에 기반으로 합니다. 그러나 클래스에의 한 비트맵 표시는 `PhotoCropperCanvasView` 표시 영역의 크기를 기준으로 크기가 조정 됩니다. `bitmapScaleMatrix`재정의에서 계산 된는 `OnPaintSurface` 비트맵 픽셀에서 비트맵의 크기 및 위치 (표시 되는 경우)로 매핑됩니다. 그런 다음이 매트릭스를 사용 하 여 비트맵을 기준으로 표시 될 수 있도록 자르기 사각형을 변환 합니다.

재정의의 마지막 줄은의 `OnPaintSurface` 역함수를 사용 하 여 `bitmapScaleMatrix` 필드로 저장 합니다 `inverseBitmapMatrix` . 터치 처리에 사용 됩니다.

`TouchEffect`개체는 필드로 인스턴스화되고 생성자는 이벤트에 처리기를 연결 `TouchAction` 하지만를 `TouchEffect` `Effects` 파생의 _부모_ 에 대 한 컬렉션에 추가 해야 `SKCanvasView` 하므로 재정의에서 수행 됩니다 `OnParentSet` .

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    const int RADIUS = 100;     // pixel radius of touch hit-test
    ···
    CroppingRectangle croppingRect;
    SKMatrix inverseBitmapMatrix;

    // Touch tracking 
    TouchEffect touchEffect = new TouchEffect();
    struct TouchPoint
    {
        public int CornerIndex { set; get; }
        public SKPoint Offset { set; get; }
    }

    Dictionary<long, TouchPoint> touchPoints = new Dictionary<long, TouchPoint>();
    ···
    public PhotoCropperCanvasView(SKBitmap bitmap, float? aspectRatio = null)
    {
        ···
        touchEffect.TouchAction += OnTouchEffectTouchAction;
    }
    ···
    protected override void OnParentSet()
    {
        base.OnParentSet();

        // Attach TouchEffect to parent view
        Parent.Effects.Add(touchEffect);
    }
    ···
    void OnTouchEffectTouchAction(object sender, TouchActionEventArgs args)
    {
        SKPoint pixelLocation = ConvertToPixel(args.Location);
        SKPoint bitmapLocation = inverseBitmapMatrix.MapPoint(pixelLocation);

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Convert radius to bitmap/cropping scale
                float radius = inverseBitmapMatrix.ScaleX * RADIUS;

                // Find corner that the finger is touching
                int cornerIndex = croppingRect.HitTest(bitmapLocation, radius);

                if (cornerIndex != -1 && !touchPoints.ContainsKey(args.Id))
                {
                    TouchPoint touchPoint = new TouchPoint
                    {
                        CornerIndex = cornerIndex,
                        Offset = bitmapLocation - croppingRect.Corners[cornerIndex]
                    };

                    touchPoints.Add(args.Id, touchPoint);
                }
                break;

            case TouchActionType.Moved:
                if (touchPoints.ContainsKey(args.Id))
                {
                    TouchPoint touchPoint = touchPoints[args.Id];
                    croppingRect.MoveCorner(touchPoint.CornerIndex, 
                                            bitmapLocation - touchPoint.Offset);
                    InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchPoints.ContainsKey(args.Id))
                {
                    touchPoints.Remove(args.Id);
                }
                break;
        }
    }

    SKPoint ConvertToPixel(Xamarin.Forms.Point pt)
    {
        return new SKPoint((float)(CanvasSize.Width * pt.X / Width),
                           (float)(CanvasSize.Height * pt.Y / Height));
    }
}
```

처리기에서 처리 하는 터치 이벤트는 `TouchAction` 장치 독립적 단위에 있습니다. 이러한 첫 번째는 클래스 아래쪽의 메서드를 사용 하 여 픽셀로 변환 된 `ConvertToPixel` 다음를 `CroppingRectangle` 사용 하 여 단위로 변환 해야 `inverseBitmapMatrix` 합니다.

이벤트의 경우 `Pressed` `TouchAction` 처리기는의 메서드를 호출 합니다 `HitTest` `CroppingRectangle` . 이가 1이 아닌 인덱스를 반환 하면 &ndash; 자르기 사각형의 모퉁이 중 하나가 조작 됩니다. 이 인덱스와 모퉁이에서 실제 터치 지점의 오프셋은 개체에 저장 되 `TouchPoint` 고 사전에 추가 됩니다 `touchPoints` .

이벤트의 경우 `Moved` `MoveCorner` 의 메서드를 `CroppingRectangle` 호출 하 여 가로 세로 비율을 조정할 수 있는 모퉁이를 이동 합니다.

언제 든 지를 사용 하는 프로그램은 `PhotoCropperCanvasView` 속성에 액세스할 수 있습니다 `CroppedBitmap` . 이 속성은 `Rect` 의 속성을 사용 `CroppingRectangle` 하 여 잘린 크기의 새 비트맵을 만듭니다. `DrawBitmap`대상 및 원본 사각형이 있는의 버전은 원래 비트맵의 하위 집합을 추출 합니다.

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    ···
    public SKBitmap CroppedBitmap
    {
        get
        {
            SKRect cropRect = croppingRect.Rect;
            SKBitmap croppedBitmap = new SKBitmap((int)cropRect.Width, 
                                                  (int)cropRect.Height);
            SKRect dest = new SKRect(0, 0, cropRect.Width, cropRect.Height);
            SKRect source = new SKRect(cropRect.Left, cropRect.Top, 
                                       cropRect.Right, cropRect.Bottom);

            using (SKCanvas canvas = new SKCanvas(croppedBitmap))
            {
                canvas.DrawBitmap(bitmap, source, dest);
            }

            return croppedBitmap;
        }
    }
    ···
}
```

## <a name="hosting-the-photo-cropper-canvas-view"></a>Photo cropper canvas 뷰 호스팅

자르기 논리를 처리 하는 이러한 두 클래스를 사용 하 여 **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** 응용 프로그램의 **사진 자르기** 페이지에서 수행할 작업은 거의 없습니다. XAML 파일은를 인스턴스화하고 `Grid` `PhotoCropperCanvasView` **Done** 단추를 호스팅합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoCroppingPage"
             Title="Photo Cropping">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid x:Name="canvasViewHost"
              Grid.Row="0"
              BackgroundColor="Gray"
              Padding="5" />

        <Button Text="Done"
                Grid.Row="1"
                HorizontalOptions="Center"
                Margin="5"
                Clicked="OnDoneButtonClicked" />
    </Grid>
</ContentPage>
```

는 `PhotoCropperCanvasView` 형식의 매개 변수가 필요 하므로 XAML 파일에서 인스턴스화할 수 없습니다 `SKBitmap` .

대신 `PhotoCropperCanvasView` 리소스 비트맵 중 하나를 사용 하 여 코드를 사용 하는 파일의 생성자에서가 인스턴스화됩니다.

```csharp
public partial class PhotoCroppingPage : ContentPage
{
    PhotoCropperCanvasView photoCropper;
    SKBitmap croppedBitmap;

    public PhotoCroppingPage ()
    {
        InitializeComponent ();

        SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

        photoCropper = new PhotoCropperCanvasView(bitmap);
        canvasViewHost.Children.Add(photoCropper);
    }

    void OnDoneButtonClicked(object sender, EventArgs args)
    {
        croppedBitmap = photoCropper.CroppedBitmap;

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
        canvas.DrawBitmap(croppedBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

그러면 사용자가 자르기 사각형을 조작할 수 있습니다.

[![사진 Cropper 1](cropping-images/PhotoCropping1.png "사진 Cropper 1")](cropping-images/PhotoCropping1-Large.png#lightbox)

적절 한 자르기 사각형이 정의 되 면 **완료** 단추를 클릭 합니다. `Clicked`처리기는의 속성에서 잘린 비트맵을 가져오고 `CroppedBitmap` `PhotoCropperCanvasView` 페이지의 모든 내용을 `SKCanvasView` 이 잘린 비트맵을 표시 하는 새 개체로 바꿉니다.

[![사진 Cropper 2](cropping-images/PhotoCropping2.png "사진 Cropper 2")](cropping-images/PhotoCropping2-Large.png#lightbox)

의 두 번째 인수 `PhotoCropperCanvasView` 를 1.78이 f (예:)로 설정 해 봅니다.

```csharp
photoCropper = new PhotoCropperCanvasView(bitmap, 1.78f);
```

높은 정의 텔레비전의 16-9 가로 세로 비율 특성으로 제한 되는 자르기 사각형을 볼 수 있습니다.

<a name="tile-division" />

## <a name="dividing-a-bitmap-into-tiles"></a>비트맵을 타일로 분할

Xamarin.FormsXamagonXuzzle를 [_사용 하 여 Mobile Apps를 만드는_](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) 책의 22 장에서 발췌 한 유명한 14-15 퍼즐의 버전은 [**XamagonXuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle)로 다운로드할 수 있습니다. 그러나 퍼즐은 사용자의 사진 라이브러리의 이미지를 기반으로 하는 경우에 더 재미 있고 자주 사용 하기가 더 어려워집니다.

이 버전의 14-15 퍼즐은 **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** 응용 프로그램의 일부 이며 **사진 퍼즐**이라는 일련의 페이지로 구성 됩니다.

**PhotoPuzzlePage1** 파일은 다음으로 구성 됩니다 `Button` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoPuzzlePage1"
             Title="Photo Puzzle">
    
    <Button Text="Pick a photo from your library"
            VerticalOptions="CenterAndExpand" 
            HorizontalOptions="CenterAndExpand"
            Clicked="OnPickButtonClicked"/>
    
</ContentPage>
```

코드 숨김이 파일은 `Clicked` 종속성 서비스를 사용 하 여 `IPhotoLibrary` 사용자가 사진 라이브러리에서 사진을 선택할 수 있도록 하는 처리기를 구현 합니다.

```csharp
public partial class PhotoPuzzlePage1 : ContentPage
{
    public PhotoPuzzlePage1 ()
    {
        InitializeComponent ();
    }

    async void OnPickButtonClicked(object sender, EventArgs args)
    {
        IPhotoLibrary photoLibrary = DependencyService.Get<IPhotoLibrary>();
        using (Stream stream = await photoLibrary.PickPhotoAsync())
        {
            if (stream != null)
            {
                SKBitmap bitmap = SKBitmap.Decode(stream);

                await Navigation.PushAsync(new PhotoPuzzlePage2(bitmap));
            }
        }
    }
}
```

그런 다음이 메서드는를 탐색 하 여 `PhotoPuzzlePage2` 선택한 비트맵을 생성자에 전달 합니다.

라이브러리에서 선택 된 사진이 사진 라이브러리에 표시 되는 것과 다른 방향으로 진행 되는 것이 아니라 회전 또는 대칭 이동 될 수 있습니다. 특히 iOS 장치에 문제가 있는 것입니다. 따라서에서는 이미지를 `PhotoPuzzlePage2` 원하는 방향으로 회전할 수 있습니다. XAML 파일에는 **90&#x00B0; 오른쪽** (시계 방향), **90&#x00B0; 왼쪽** (시계 반대) 및 **완료**레이블이 지정 된 세 개의 단추가 있습니다.

코드 숨김이 **[SkiaSharp 비트맵에서 만들기 및 그리기](drawing.md#rotating-bitmaps)** 문서에 표시 된 비트맵 회전 논리를 구현 합니다. 사용자는 이미지를 90도 시계 방향으로, 시계 반대 방향으로 회전할 수 있습니다. 

```csharp
public partial class PhotoPuzzlePage2 : ContentPage
{
    SKBitmap bitmap;

    public PhotoPuzzlePage2 (SKBitmap bitmap)
    {
        this.bitmap = bitmap;

        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnRotateRightButtonClicked(object sender, EventArgs args)
    {
        SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear();
            canvas.Translate(bitmap.Height, 0);
            canvas.RotateDegrees(90);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = rotatedBitmap;
        canvasView.InvalidateSurface();
    }

    void OnRotateLeftButtonClicked(object sender, EventArgs args)
    {
        SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear();
            canvas.Translate(0, bitmap.Width);
            canvas.RotateDegrees(-90);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = rotatedBitmap;
        canvasView.InvalidateSurface();
    }

    async void OnDoneButtonClicked(object sender, EventArgs args)
    {
        await Navigation.PushAsync(new PhotoPuzzlePage3(bitmap));
    }
}
```

사용자가 **완료** 단추를 클릭 하면 처리기가 `Clicked` 로 이동 하 여 `PhotoPuzzlePage3` 페이지의 생성자에서 최종 회전 된 비트맵을 전달 합니다.

`PhotoPuzzlePage3`사진을 자를 수 있습니다. 이 프로그램을 사용 하려면 타일의 4 x 4 모눈으로 나누는 사각형 비트맵이 필요 합니다.

**PhotoPuzzlePage3** 파일에는를 `Label` `Grid` 호스트 하는 `PhotoCropperCanvasView` 및 다른 **완료** 단추를 포함 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoPuzzlePage3"
             Title="Photo Puzzle">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Label Text="Crop the photo to a square"
               Grid.Row="0"
               FontSize="Large"
               HorizontalTextAlignment="Center"
               Margin="5" />

        <Grid x:Name="canvasViewHost"
              Grid.Row="1"
              BackgroundColor="Gray"
              Padding="5" />

        <Button Text="Done"
                Grid.Row="2"
                HorizontalOptions="Center"
                Margin="5"
                Clicked="OnDoneButtonClicked" />
    </Grid>
</ContentPage>
```

코드를 사용 하는 파일은 해당 생성자에 전달 된 비트맵을 사용 하 여를 인스턴스화합니다 `PhotoCropperCanvasView` . 1은에 대 한 두 번째 인수로 전달 됩니다 `PhotoCropperCanvasView` . 이 가로 세로 비율 1은 자르기 사각형을 사각형으로 강제 합니다.

```csharp
public partial class PhotoPuzzlePage3 : ContentPage
{
    PhotoCropperCanvasView photoCropper;

    public PhotoPuzzlePage3(SKBitmap bitmap)
    {
        InitializeComponent ();

        photoCropper = new PhotoCropperCanvasView(bitmap, 1f);
        canvasViewHost.Children.Add(photoCropper);
    }

    async void OnDoneButtonClicked(object sender, EventArgs args)
    {
        SKBitmap croppedBitmap = photoCropper.CroppedBitmap;
        int width = croppedBitmap.Width / 4;
        int height = croppedBitmap.Height / 4;

        ImageSource[] imgSources = new ImageSource[15];

        for (int row = 0; row < 4; row++)
        {
            for (int col = 0; col < 4; col++)
            {
                // Skip the last one!
                if (row == 3 && col == 3)
                    break;

                // Create a bitmap 1/4 the width and height of the original
                SKBitmap bitmap = new SKBitmap(width, height);
                SKRect dest = new SKRect(0, 0, width, height);
                SKRect source = new SKRect(col * width, row * height, (col + 1) * width, (row + 1) * height);

                // Copy 1/16 of the original into that bitmap
                using (SKCanvas canvas = new SKCanvas(bitmap))
                {
                    canvas.DrawBitmap(croppedBitmap, source, dest);
                }

                imgSources[4 * row + col] = (SKBitmapImageSource)bitmap;
            }
        }

        await Navigation.PushAsync(new PhotoPuzzlePage4(imgSources));
    }
}
```

**완료** 단추 처리기는 잘린 비트맵의 너비와 높이를 가져온 다음 (이 두 값은 동일 해야 함) 15 개의 개별 비트맵으로 나눕니다. 각 비트맵은 원래 1/4의 너비와 높이를 각각 합니다. 가능 하면 16 개의 비트맵 중 마지막 비트맵이 생성 되지 않습니다. `DrawBitmap`원본 및 대상 사각형이 있는 메서드를 사용 하면 큰 비트맵의 하위 집합을 기반으로 비트맵을 만들 수 있습니다.

## <a name="converting-to-xamarinforms-bitmaps"></a>비트맵으로 변환 Xamarin.Forms

`OnDoneButtonClicked`메서드에서 15 개의 비트맵에 대해 만들어진 배열은 [`ImageSource`](xref:Xamarin.Forms.ImageSource) 다음과 같습니다.

```csharp
ImageSource[] imgSources = new ImageSource[15];
```

`ImageSource`는 Xamarin.Forms 비트맵을 캡슐화 하는 기본 형식입니다. 다행히 SkiaSharp는 SkiaSharp 비트맵에서 비트맵으로의 변환을 허용 Xamarin.Forms 합니다. **SkiaSharp** 어셈블리는 [`SKBitmapImageSource`](xref:SkiaSharp.Views.Forms.SKBitmapImageSource) 에서 파생 되는 클래스를 정의 `ImageSource` 하지만 SkiaSharp 개체를 기반으로 만들 수 있습니다 `SKBitmap` . `SKBitmapImageSource`는 및 간의 변환만 `SKBitmapImageSource` 정의 `SKBitmap` 하며,이는 `SKBitmap` 개체가 배열에 비트맵으로 저장 되는 방식입니다 Xamarin.Forms .

```csharp
imgSources[4 * row + col] = (SKBitmapImageSource)bitmap;
```

이 비트맵 배열은에 생성자로 전달 됩니다 `PhotoPuzzlePage4` . 해당 페이지는 완전히 Xamarin.Forms SkiaSharp 사용 하지 않습니다. [**XamagonXuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle)와 매우 유사 하기 때문에 여기서는 설명 하지 않지만 선택한 사진을 15 개의 사각형 타일로 표시 합니다.

[![사진 퍼즐 1](cropping-images/PhotoPuzzle1.png "사진 퍼즐 1")](cropping-images/PhotoPuzzle1-Large.png#lightbox)

[ **임의** ] 단추를 누르면 모든 타일이 혼합 됩니다.

[![사진 퍼즐 2](cropping-images/PhotoPuzzle2.png "사진 퍼즐 2")](cropping-images/PhotoPuzzle2-Large.png#lightbox)

이제 올바른 순서 대로 다시 배치할 수 있습니다. 빈 사각형으로 동일한 행 또는 열에 있는 모든 타일을 탭 하 여 빈 사각형으로 이동할 수 있습니다. 

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
