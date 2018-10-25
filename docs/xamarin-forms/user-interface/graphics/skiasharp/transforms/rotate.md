---
title: 회전 변환
description: 이 문서는 효과 애니메이션 SkiaSharp 회전 변환을 사용 가능한 탐색 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: CBB3CD72-4377-4EA3-A768-0C4228229FC2
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2017
ms.openlocfilehash: 3726a93ccf43fd9a2afdc2c46bb63e0f6ef7ad51
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "39615251"
---
# <a name="the-rotate-transform"></a>회전 변환

_효과 애니메이션 SkiaSharp 회전 변환 가능한 탐색_

SkiaSharp 그래픽 개체 회전 변환을 사용 하 여 가로 및 세로 축이 있는 맞춤 제약 조건의 무료가 중단:

![](rotate-images/rotateexample.png "중심으로 회전 된 텍스트")

SkiaSharp 둘 다를 지 원하는 지점 (0, 0), 그래픽 개체를 회전 하는 것에 대 한는 [ `RotateDegrees` ](xref:SkiaSharp.SKCanvas.RotateDegrees(System.Single)) 메서드 및 [ `RotateRadians` ](xref:SkiaSharp.SKCanvas.RotateRadians(System.Single)) 메서드:

```csharp
public void RotateDegrees (Single degrees)

public Void RotateRadians (Single radians)
```

360도의 원 단위 2 개와 간에 변환할 쉽게 twoπ 라디안와 같습니다. 더 편리 하 게 사용 합니다. .NET의 모든 삼각 함수 [ `Math` ](xref:System.Math) 라디안의 단위를 사용 하는 클래스입니다.

회전 각도 높이기 위한 시계 방향으로 됩니다. (규칙에 따라 시계 반대 방향으로 회전 데카르트 좌표계에 경우에 시계 방향 회전 일관성이 SkiaSharp와 같이 점차 증가 하는 Y 좌표를 사용 하 여.) 각도 및 각도 360도 허용 되는 보다 큰 음수입니다.

회전 변환 수식 변환 및 확장에 대 한 것 보다 더 복잡 한 경우 Α의 각도 대 한 변환 수식은 다음과 같습니다.

x' x•cos(α) – = y•sin(α)   

y` = x•sin(α) + y•cos(α)

합니다 **기본 회전** 페이지를 보여 줍니다는 `RotateDegrees` 메서드. 합니다 [ **BasicRotate.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicRotatePage.xaml.cs) 페이지의 가운데 기준선을 사용 하 여 일부 텍스트를 표시 하 고 기준으로 회전 하는 파일을 `Slider` 360 –360 범위를 사용 하 여 합니다. 관련 부분은 여기는 `PaintSurface` 처리기:

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Blue,
    TextAlign = SKTextAlign.Center,
    TextSize = 100
})
{
    canvas.RotateDegrees((float)rotateSlider.Value);
    canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
}
```

이 프로그램에서 설정 하는 대부분의 각도 캔버스의 왼쪽 위 모퉁이 중심으로 하는 회전 하기 때문에 화면에 텍스트의 회전 됩니다.

[![](rotate-images/basicrotate-small.png "기본 회전 페이지 스크린샷 삼중")](rotate-images/basicrotate-large.png#lightbox "삼중 기본 회전 페이지 스크린샷")

이러한 버전을 사용 하 여 지정한 피벗 점을 중심 무언가 회전 해야 하는 자주 합니다 [ `RotateDegrees` ](xref:SkiaSharp.SKCanvas.RotateDegrees(System.Single,System.Single,System.Single)) 하 고 [ `RotateRadians` ](xref:SkiaSharp.SKCanvas.RotateRadians(System.Single,System.Single,System.Single)) 메서드:

```csharp
public void RotateDegrees (Single degrees, Single px, Single py)

public void RotateRadians (Single radians, Single px, Single py)
```

합니다 **회전 중심** 페이지와 비슷합니다는 **기본 회전** 점을 제외 하 고 확장된 된 버전에는 `RotateDegrees` 회전 중심의 텍스트를 배치 하는 데 동일한 지점을 설정 하는:

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Blue,
    TextAlign = SKTextAlign.Center,
    TextSize = 100
})
{
    canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
    canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
}
```

이제 텍스트 나머지 아이콘 주위로 회전 텍스트의 기준선의 가로 가운데 텍스트를 배치 하는 데 사용 되는 지점:

[![](rotate-images/centeredrotate-small.png "회전 중심 페이지 스크린샷 삼중")](rotate-images/centeredrotate-large.png#lightbox "삼중 회전 중심 페이지 스크린샷")

가운데에 맞출지 버전과 마찬가지로 합니다 `Scale` 메서드, 가운데에 맞출지 버전을 `RotateDegrees` 호출 바로 가기입니다. 메서드는 다음과 같습니다.

```csharp
RotateDegrees (degrees, px, py);
```

호출 하는 다음과 같습니다.

```csharp
canvas.Translate(px, py);
canvas.RotateDegrees(degrees);
canvas.Translate(-px, -py);
```

경우에 따라 조합할 수 있는 알게 `Translate` 사용 하 여 호출 `Rotate` 호출 합니다. 예를 들어, 다음은 `RotateDegrees` 하 고 `DrawText` 에서 호출 합니다 **회전 중심** 페이지;

```csharp
canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

합니다 `RotateDegrees` 에 호출은 동일한 두 개의 `Translate` 호출과 가운데 아닌 `RotateDegrees`:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

합니다 `DrawText` 특정 위치에서 텍스트를 표시 하는 호출 하는 것을 `Translate` 뒤에 해당 위치에 대 한 호출 `DrawText` 지점 (0, 0):

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.DrawText(Title, 0, 0, textPaint);
```

연속 된 두 `Translate` 호출 서로 취소 합니다.

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.DrawText(Title, 0, 0, textPaint);
```

개념적으로 코드에 표시 되는 방식 반대 순서로 두 변환은 적용 됩니다. `DrawText` 호출 캔버스의 왼쪽 위 모서리에 있는 텍스트를 표시 합니다. `RotateDegrees` 호출 왼쪽 위 모퉁이 기준으로 해당 텍스트를 회전 합니다. 그런 다음 `Translate` 호출 캔버스의 가운데에 텍스트를 이동 합니다.

일반적으로 여러 가지 회전 및 변환이 결합 합니다. 합니다 **회전 텍스트** 페이지 다음 표시를 만듭니다.

[![](rotate-images/rotatedtext-small.png "회전 된 텍스트 페이지의 3 배가 스크린샷")](rotate-images/rotatedtext-large.png#lightbox "삼중 회전 텍스트 페이지 스크린샷")

다음은 `PaintSurface` 처리기는 [ `RotatedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/RotatedTextPage.cs) 클래스:

```csharp
static readonly string text = "    ROTATE";
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 72
    })
    {
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);
        float yText = yCenter - textBounds.Height / 2 - textBounds.Top;

        for (int degrees = 0; degrees < 360; degrees += 30)
        {
            canvas.Save();
            canvas.RotateDegrees(degrees, xCenter, yCenter);
            canvas.DrawText(text, xCenter, yText, textPaint);
            canvas.Restore();
        }
    }
}

```

합니다 `xCenter` 고 `yCenter` 값 캔버스의 중심을 나타냅니다. `yText` 값이 약간는 오프셋입니다. 이 값은 실제로 세로 방향으로 페이지의 가운데에 배치 되도록 텍스트를 배치 하는 데 필요한 Y 좌표입니다. `for` 루프 캔버스의 가운데에 따라 회전을 설정 합니다. 30도 단위로 회전이 됩니다. 사용 하 여 텍스트를 그리는 `yText` 값입니다. "회전" 이라는 단어 앞에 있는 공백의 수는 `text` 값 표시를 dodecagon 이러한 12 텍스트 문자열 간의 연결을 만드는 데 알려지고 실험적으로 확인 되었습니다.

이 코드를 단순화 한 가지 방법으로 30도 회전 각도 후 루프가 반복 될 때마다 증가 하는 것은 `DrawText` 호출 합니다. 이에 대 한 호출에 대 한 필요가 `Save` 고 `Restore`입니다. 다음에 유의 합니다 `degrees` 변수 본문 내에서 더 이상 사용 되지를 `for` 블록:

```csharp
for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, xCenter, yText, textPaint);
    canvas.RotateDegrees(30, xCenter, yCenter);
}

```

단순 형식으로 사용할 수 이기도 `RotateDegrees` 에 대 한 호출을 사용 하 여 루프를 앞으로 `Translate` 캔버스의 가운데에 모든 항목을 이동 하려면:

```csharp
float yText = -textBounds.Height / 2 - textBounds.Top;

canvas.Translate(xCenter, yCenter);

for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, 0, yText, textPaint);
    canvas.RotateDegrees(30);
}
```

수정 된 `yText` 계산을 더 이상 통합 `yCenter`합니다. 이제는 `DrawText` 호출 캔버스의 맨 위에 있는 세로 텍스트 가운데에 맞춥니다.

있기 때문에 변환 코드에 표시 되는 방식을는 달리 개념적으로 적용 되며, 종종 뒤에 자세한 로컬 변환 가능한 시작 하려면 더 많은 글로벌 변환 합니다. 회전 및 변환이 결합 하는 가장 쉬운 방법은 경우가 있습니다.

예를 들어 축에서 회전 하 여 전 세계 마찬가지로 중심으로 회전 하는 그래픽 개체를 그릴 한다고 가정 합니다. 하지만이 개체는 전 세계 태양 둘러싼 매우 유사 하 게 화면 가운데를 중심으로 합니다.

개체를 캔버스의 왼쪽 위 모퉁이에 배치 하 고 해당 모퉁이 중심으로 회전에 애니메이션을 사용 하 여이 수행할 수 있습니다. 다음으로 궤도 radius 가로로 같은 개체를 변환 합니다. 이제는 원점 기준도 두 번째 애니메이션된 회전을 적용 합니다. 이 통해 개체의 모퉁이 중심으로 합니다. 이제 캔버스의 가운데에 변환 합니다.

같습니다는 `PaintSurface` 이러한를 포함 하는 처리기 호출을 역순으로 변환 합니다.

```csharp
float revolveDegrees, rotateDegrees;
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Red
    })
    {
        // Translate to center of canvas
        canvas.Translate(info.Width / 2, info.Height / 2);

        // Rotate around center of canvas
        canvas.RotateDegrees(revolveDegrees);

        // Translate horizontally
        float radius = Math.Min(info.Width, info.Height) / 3;
        canvas.Translate(radius, 0);

        // Rotate around center of object
        canvas.RotateDegrees(rotateDegrees);

        // Draw a square
        canvas.DrawRect(new SKRect(-50, -50, 50, 50), fillPaint);
    }
}
```

합니다 `revolveDegrees` 및 `rotateDegrees` 필드 애니메이션을 적용 합니다. 이 프로그램은 Xamarin.Forms에 따라 다양 한 애니메이션 기술을 사용 하 여 [ `Animation` ](xref:Xamarin.Forms.Animation) 클래스입니다. (이 클래스에 설명 되어 [의 22 장 *Creating Mobile Apps with Xamarin.Forms*](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf))를 `OnAppearing` 재정의 만들고 두 `Animation` 콜백 메서드를 사용 하 여 개체를 설정한 다음 `Commit` 에 애니메이션 지속 시간에 대 한 합니다.

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    new Animation((value) => revolveDegrees = 360 * (float)value).
        Commit(this, "revolveAnimation", length: 10000, repeat: () => true);

    new Animation((value) =>
    {
        rotateDegrees = 360 * (float)value;
        canvasView.InvalidateSurface();
    }).Commit(this, "rotateAnimation", length: 1000, repeat: () => true);
}
```

첫 번째 `Animation` 개체에 애니메이션을 적용 `revolveDegrees` 0도에서 360도 10 초 이상에서. 두 번째 애니메이션 효과 줍니다 `rotateDegrees` 360도 0도에서 1 개당 그리고 두 번째 화면을 무효화 합니다에 다른 호출을 생성 하는 `PaintSurface` 처리기입니다. `OnDisappearing` 재정의 이러한 두 애니메이션을 취소 합니다.

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    this.AbortAnimation("revolveAnimation");
    this.AbortAnimation("rotateAnimation");
}
```

합니다 **까다로운 아날로그 시계의** 프로그램 (이후 문서를 더 매력적인 아날로그 시계의 설명 되어 있으므로 소위 함) 사용 하 여 회전 시계의 분, 시간 표시를 그립니다 바늘 회전 합니다. 프로그램 지점 (0, 0)는 radius 사용 하 여 100에 중점을 두는 원에 따라 임의의 좌표 시스템을 사용 하는 시계를 그립니다. 확장 하 고 해당 원 페이지의 가운데를 변환 및 크기 조정을 사용 합니다.

합니다 `Translate` 하 고 `Scale` 호출에 전체적으로 적용 클록을 먼저 초기화 하는 다음 호출 되는 `SKPaint` 개체:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint strokePaint = new SKPaint())
    using (SKPaint fillPaint = new SKPaint())
    {
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.Color = SKColors.Black;
        strokePaint.StrokeCap = SKStrokeCap.Round;

        fillPaint.Style = SKPaintStyle.Fill;
        fillPaint.Color = SKColors.Gray;

        // Transform for 100-radius circle centered at origin
        canvas.Translate(info.Width / 2f, info.Height / 2f);
        canvas.Scale(Math.Min(info.Width / 200f, info.Height / 200f));
        ...
    }
}
```

내내 원 안에 그려져 야 하는 두 가지 크기의 60 표시가 있습니다. `DrawCircle` 호출 시계의 가운데를 기준으로 12:00에 해당 하는 지점 (0, –90)에서 해당 원을 그립니다. `RotateDegrees` 호출 마다 눈금 후 6 도씩 회전 각도 증가 시킵니다. `angle` 변수는 데만 큰 원이나 작은 원을 그릴 경우를 결정 합니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
        // Hour and minute marks
        for (int angle = 0; angle < 360; angle += 6)
        {
            canvas.DrawCircle(0, -90, angle % 30 == 0 ? 4 : 2, fillPaint);
            canvas.RotateDegrees(6);
        }
    ...
    }
}
```

마지막으로 `PaintSurface` 처리기 현재 시간 가져오고 1 시간, 분 및 두 번째 실습에 대 한 회전 각도 계산 합니다. 회전 각도 기준으로 되도록 각 포인터 12:00 위치에 그려집니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
        DateTime dateTime = DateTime.Now;

        // Hour hand
        strokePaint.StrokeWidth = 20;
        canvas.Save();
        canvas.RotateDegrees(30 * dateTime.Hour + dateTime.Minute / 2f);
        canvas.DrawLine(0, 0, 0, -50, strokePaint);
        canvas.Restore();

        // Minute hand
        strokePaint.StrokeWidth = 10;
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Minute + dateTime.Second / 10f);
        canvas.DrawLine(0, 0, 0, -70, strokePaint);
        canvas.Restore();

        // Second hand
        strokePaint.StrokeWidth = 2;
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Second);
        canvas.DrawLine(0, 10, 0, -80, strokePaint);
        canvas.Restore();
    }
}
```

클록이 게는 다소 조잡 하지만 분명히 기능:

[![](rotate-images/uglyanalogclock-small.png "Triple 까다로운 아날로그 클록 텍스트 페이지의 스크린샷")](rotate-images/uglyanalogclock-large.png#lightbox "Triple screenshot of the Ugly Analog page")

더 매력적인 클록에 대 한 문서를 참조 [ **SVG 경로 데이터에서 SkiaSharp**](../curves/path-data.md)합니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
