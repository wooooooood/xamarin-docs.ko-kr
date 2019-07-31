---
title: SkiaSharp 비트맵 자르기
description: SkiaSharp 사용 하 여 대화형으로 설명 하는 자르기 사각형의 사용자 인터페이스를 디자인 하는 방법에 알아봅니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 0A79AB27-C69F-4376-8FFE-FF46E4783F30
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: 50174844100eb852ac7daf5ce3f33b02b490ceb2
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68646654"
---
# <a name="cropping-skiasharp-bitmaps"></a>SkiaSharp 비트맵 자르기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

합니다 [ **만들고 그리기 SkiaSharp 비트맵** ](drawing.md) 하는 방법을 설명 하는 문서를 `SKBitmap` 개체를 전달할 수는 `SKCanvas` 생성자입니다. 비트맵에 렌더링 되는 캔버스 원인 그래픽 라는 모든 그리기 메서드. 이러한 그리기 메서드를 포함 `DrawBitmap`, 즉,이 기술은 한 비트맵의 전체 또는 일부 다른 비트맵 아마도 사용 하 여 전송 적용 되는 변환을 허용함.

비트맵을 호출 하 여 자르기에 대 한 해당 기법을 사용할 수는 [ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint)) 소스 및 대상 사각형을 사용 하 여 메서드:

```csharp
canvas.DrawBitmap(bitmap, sourceRect, destRect);
```

그러나 종종 자르기 구현 하는 응용 프로그램을 대화형으로 자르기 사각형을 선택할 사용자에 대 한 인터페이스를 제공 합니다.

![샘플 자르기](cropping-images/CroppingSample.png "샘플 자르기")

이 문서는 해당 인터페이스에 중점을 둡니다.

## <a name="encapsulating-the-cropping-rectangle"></a>자르기 사각형을 캡슐화합니다.

라는 클래스에는 자르기 논리 중 일부를 격리 하는 것이 유용 `CroppingRectangle`합니다. 생성자 매개 변수는 잘리지 비트맵의 크기는 일반적으로 최대 직사각형을 및 선택적 가로 세로 비율이 포함 됩니다. 생성자는 먼저 공개에 있도록 초기 자르기 사각형을 정의 합니다 `Rect` 형식의 속성 `SKRect`합니다. 이 초기 자르기 사각형 비트맵 사각형의 높이 및 너비의 80% 이지만 가로 세로 비율을 지정 하는 경우 다음 조정 됩니다.

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

한 가지 유용한 정보는 `CroppingRectangle` 도 사용할 수 있도록 설정의 배열이 `SKPoint` 왼쪽 위, 오른쪽 위, 아래 오른쪽 및 왼쪽 아래 순서로 자르기 사각형의 네 모퉁이에 해당 하는 값:

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

이 배열 이라고 하는 다음 메서드는 `HitTest`합니다. `SKPoint` 매개 변수는 지점에 해당 하는 손가락 터치 또는 마우스를 클릭 합니다. 인덱스 (0, 1, 2 또는 3)를 반환 하 여 지정 된 거리 내 손가락이 나 마우스 포인터를 작업 하는 모퉁이에 해당 합니다 `radius` 매개 변수: 

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

터치 또는 마우스 지점이 없는 경우 내 `radius` 메서드가 반환 하는 모든 모퉁이의 단위 &ndash;1입니다.

마지막 메서드로 `CroppingRectangle` 라고 `MoveCorner`, 터치 또는 마우스를 이동에 대 한 응답에서 이라고 합니다. 두 매개 변수 인덱스 이동 하는 모퉁이 및 해당 모퉁이의 새 위치를 나타냅니다. 자르기 사각형의 모퉁이 있지만 범위 내에서 항상 새 위치를 기반으로 조정 하는 메서드의 첫 번째 절반 `maxRect`, 비트맵의 크기는 합니다. 이 논리도 고려 합니다 `MINIMUM` nothing 자르기 사각형을 축소 하지 않으려면 필드:

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

메서드의 두 번째 절반에서는 선택적 가로 세로 비율을 조정합니다.

이 클래스의 모든 픽셀 단위로 점을 염두에 두십시오.

## <a name="a-canvas-view-just-for-cropping"></a>자르기에 대 한 캔버스 뷰

`CroppingRectangle` 방금 살펴봤습니다 클래스를 사용 합니다 `PhotoCropperCanvasView` 클래스에서 파생 되는 `SKCanvasView`합니다. 이 클래스는 자르기 사각형 변경에 대 한 터치 또는 마우스 이벤트를 처리할 수 있을 뿐만 아니라 비트맵 및 자르기 사각형을 표시 하는 일을 담당 합니다.

`PhotoCropperCanvasView` 생성자 비트맵에 필요 합니다. 가로 세로 비율은 선택 사항입니다. 생성자는 형식의 개체를 인스턴스화합니다 `CroppingRectangle` 이 비트맵 및 가로 세로 비율을 기반으로 하며 필드로 저장 합니다.

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

이 클래스에서 파생 되므로 `SKCanvasView`에 대 한 처리기를 설치 하지 않아도 `PaintSurface` 이벤트입니다. 대신을 재정의할 수 있습니다 해당 `OnPaintSurface` 메서드. 메서드는 비트맵을 표시 하 고 몇 가지를 사용 하 여 `SKPaint` 현재 자르기 사각형을 그릴 필드로 저장 된 개체:

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

코드는 `CroppingRectangle` 클래스 자르기 사각형 비트맵의 픽셀 크기를 기반 합니다. 그러나 하 여 비트맵의 표시를 `PhotoCropperCanvasView` 클래스 표시 영역의 크기에 따라 확장 됩니다. `bitmapScaleMatrix` 에서 계산 된 `OnPaintSurface` 표시 되는 비트맵의 위치 및 크기가 비트맵 픽셀의 지도 재정의 합니다. 이 매트릭스 비트맵을 기준으로 표시 될 수 있도록 자르기 사각형을 변환에 사용 됩니다.

마지막 줄을 `OnPaintSurface` 재정의의 역함수 값을 사용 합니다 `bitmapScaleMatrix` 로 저장 하 고는 `inverseBitmapMatrix` 필드. 터치 처리를 위해 사용 됩니다.

`TouchEffect` 필드로 개체가 인스턴스화되고 생성자에 처리기를 연결 합니다 `TouchAction` 이벤트 하지만 `TouchEffect` 에 추가 해야는 `Effects` 의 컬렉션을 _부모_ 합니다 의`SKCanvasView`완료 되도록 파생 된 `OnParentSet` 재정의:

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

터치 이벤트 처리는 `TouchAction` 처리기는 장치 독립적 단위입니다. 먼저 사용 하 여 픽셀 변환할 필요가 합니다 `ConvertToPixel` 클래스의 맨 아래에 있는 메서드를 변환한 후 `CroppingRectangle` 사용 하 여 단위 `inverseBitmapMatrix`.

에 대 한 `Pressed` 이벤트를 `TouchAction` 처리기 호출을 `HitTest` 메서드의 `CroppingRectangle`합니다. 이외의 인덱스를 반환 하는 경우 &ndash;1 다음 자르기 사각형의 모서리 중 하나를 조작 중인 합니다. 인덱스 및 모서리에서 실제 터치 지점의 오프셋에 저장 되는 `TouchPoint` 개체를 추가할는 `touchPoints` 사전입니다.

에 대 한 합니다 `Moved` 이벤트를 `MoveCorner` 메서드의 `CroppingRectangle` 가로 세로 비율에 대 한 가능한 조정 모퉁이 이동 하기 위해 호출 됩니다.

언제 든 지 사용 하 여 프로그램이 `PhotoCropperCanvasView` 액세스할 수는 `CroppedBitmap` 속성입니다. 이 속성에 사용 되는 `Rect` 의 속성을 `CroppingRectangle` 자른된 크기의 새 비트맵을 만들 수입니다. 버전 `DrawBitmap` 대상 및 소스를 사용 하 여 사각형 다음 추출 원래 비트맵의 하위 집합:

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

## <a name="hosting-the-photo-cropper-canvas-view"></a>사진 cropper 캔버스 뷰를 호스팅

자르기 논리를 처리 하는 이러한 두 클래스를 사용 하 여는 **사진 자르기** 페이지에 **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** 응용 프로그램에 거의 작업을 수행 합니다. XAML 파일은는 `Grid` 호스트에는 `PhotoCropperCanvasView` 와 **수행** 단추:

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

합니다 `PhotoCropperCanvasView` 형식의 매개 변수가 필요 하기 때문에 XAML 파일에서 인스턴스화할 수 없습니다 `SKBitmap`합니다.

대신는 `PhotoCropperCanvasView` 리소스 비트맵 중 하나를 사용 하 여 코드 숨김 파일의 생성자에서 인스턴스화됩니다.

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

그런 다음 자르기 사각형을 조작할 수 있습니다.:

[![Cropper 1 사진](cropping-images/PhotoCropping1.png "Cropper 1 사진")](cropping-images/PhotoCropping1-Large.png#lightbox)

정의한 좋은 자르기 사각형을 클릭 합니다 **수행** 단추입니다. `Clicked` 처리기에서 자른된 비트맵을 가져옵니다 합니다 `CroppedBitmap` 속성을 `PhotoCropperCanvasView`를 새 페이지의 모든 콘텐츠를 대체 하 고 `SKCanvasView` 이 자른된 비트맵을 표시 하는 개체:

[![Cropper 2 사진](cropping-images/PhotoCropping2.png "Cropper 2 사진")](cropping-images/PhotoCropping2-Large.png#lightbox)

두 번째 인수를 설정 해 보려면 `PhotoCropperCanvasView` 1.78f (예:)을 합니다.

```csharp
photoCropper = new PhotoCropperCanvasView(bitmap, 1.78f);
```

높음-텔레비전의 특성에 16-9 가로 세로 비율 제한 자르기 사각형을 볼 수 있습니다.

<a name="tile-division" />

## <a name="dividing-a-bitmap-into-tiles"></a>비트맵을 타일로 분

책의 22 장에에서 표시 되는 유명한 Xamarin.Forms 버전 14 ~ 15 퍼즐 [ _Creating Mobile Apps with Xamarin.Forms_ ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) 로 다운로드할 수 있습니다 [  **XamagonXuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle)합니다. 그러나 퍼즐 됩니다 더 재미 있게 (및 더 까다로운 종종) 자신의 사진 라이브러리에서 이미지에 기반 하는 경우.

14 ~ 15 퍼즐의이 버전의 일부인 합니다 **[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** 응용 프로그램에는 일련의 페이지가 이라는 구성 됩니다 **사진 퍼즐**.

합니다 **PhotoPuzzlePage1.xaml** 구성 파일을 `Button`:

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

코드 숨김 파일을 구현 하는 `Clicked` 처리기를 사용 하는 `IPhotoLibrary` 종속성 서비스 사용자가 사진 라이브러리에서 사진을 선택:

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

메서드는 다음으로 이동 `PhotoPuzzlePage2`선택한 비트맵 생성자에 전달 합니다.

사진 라이브러리에 표시 되 고 있지만 회전 또는 거꾸로 라이브러리에서 선택한 사진 지향 아님을 가능성이 있습니다. (IOS 장치를 나열 하는 특히 문제가 됩니다.) 이런 이유로 `PhotoPuzzlePage2` 원하는 방향으로 이미지를 회전할 수 있습니다. XAML 파일에 레이블이 지정 된 세 가지 단추가 **90&#x00B0; 오른쪽** (즉 시계 방향으로), **90&#x00B0; 왼쪽** (시계 반대 방향으로), 및 **완료**.

이 문서에 표시 된 비트맵 회전 논리를 구현 하는 코드 숨김 파일을  **[만들고 SkiaSharp 비트맵에 드로잉](drawing.md#rotating-bitmaps)** 합니다. 사용자 이미지를 시계 방향 또는 시계 반대 방향으로 90도 원하는 횟수 만큼 회전 수 있습니다.: 

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

클릭할 때를 **수행** 단추를 `Clicked` 처리기로 이동 `PhotoPuzzlePage3`, 최종 회전된 비트맵 페이지의 생성자에 전달 합니다.

`PhotoPuzzlePage3` 잘라야 사진을 허용 합니다. 프로그램-4x4 표 형태 타일을 나누는 데 사각형 비트맵에 필요 합니다.

**PhotoPuzzlePage3.xaml** 파일에는 `Label`, `Grid` 호스트에는 `PhotoCropperCanvasView`, 또 다른 **수행** 단추:

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

코드 숨김 파일을 인스턴스화하는 `PhotoCropperCanvasView` 해당 생성자에 전달 된 비트맵입니다. 1은 두 번째 인수로 전달 되는 알림 `PhotoCropperCanvasView`합니다. 1의 가로 세로 비율이이 강제로 정사각형이 자르기 사각형:

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

합니다 **수행** 단추 처리기 (이 두 값 같아야) 자른된 비트맵의 높이 너비를 가져오고 다음 1/4는 각각 별도 비트맵 15 나눕니다 원래 높이 너비입니다. (가능한 16 비트맵의 마지막 생성 되지 않습니다.) `DrawBitmap` 원본 및 대상 사각형을 사용 하 여 메서드 비트맵을 더 큰 비트맵의 하위 집합에 따라 만들 수 있습니다.

## <a name="converting-to-xamarinforms-bitmaps"></a>Xamarin.Forms 비트맵으로 변환

에 `OnDoneButtonClicked` 메서드를 15 비트맵 만든 배열 유형임 [ `ImageSource` ](xref:Xamarin.Forms.ImageSource):

```csharp
ImageSource[] imgSources = new ImageSource[15];
```

`ImageSource` 비트맵을 캡슐화 하는 Xamarin.Forms 기본 형식이입니다. 다행 스럽게도 SkiaSharp 비트맵 Xamarin.Forms에서 SkiaSharp 비트맵 변환할 수 있습니다. **SkiaSharp.Views.Forms** 어셈블리 정의 [ `SKBitmapImageSource` ](xref:SkiaSharp.Views.Forms.SKBitmapImageSource) 에서 파생 된 클래스 `ImageSource` SkiaSharp을 따라 만들 수 있습니다 하지만 `SKBitmap` 개체입니다. `SKBitmapImageSource` 간의 변환도 정의 `SKBitmapImageSource` 및 `SKBitmap`, 및는 어떻게 `SKBitmap` 개체 Xamarin.Forms 비트맵으로 배열에 저장 됩니다:

```csharp
imgSources[4 * row + col] = (SKBitmapImageSource)bitmap;
```

이 배열을 비트맵을 생성자로 전달 됩니다 `PhotoPuzzlePage4`합니다. 해당 페이지는 Xamarin.Forms 완전히 및 모든 SkiaSharp 사용 하지 않습니다. 매우 비슷합니다 [ **XamagonXuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle)이므로 여기서 설명 하지는 않지만 15 정사각형 타일로 구분 하 여 선택한 사진 표시:

[![퍼즐 1 사진](cropping-images/PhotoPuzzle1.png "퍼즐 1 사진")](cropping-images/PhotoPuzzle1-Large.png#lightbox)

키를 눌러 합니다 **임의** 모든 타일을 혼합 하는 단추:

[![퍼즐 2 사진](cropping-images/PhotoPuzzle2.png "퍼즐 2 사진")](cropping-images/PhotoPuzzle2-Large.png#lightbox)

이제 올바른 순서로 이러한을 넣을 수 있습니다. 빈 사각형 폴더로 이동 하 동일한 행 또는 열을 빈 사각형의 모든 타일을 탭 할 수 있습니다. 

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
