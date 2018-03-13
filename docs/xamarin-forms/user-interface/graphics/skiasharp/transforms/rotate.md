---
title: "회전 변환"
description: "및 SkiaSharp 회전 변환을 사용 하 여 가능한 애니메이션 효과 탐색 합니다."
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: CBB3CD72-4377-4EA3-A768-0C4228229FC2
author: charlespetzold
ms.author: chape
ms.date: 03/23/2017
ms.openlocfilehash: 146093e15651316e84947e2bd81eeee3bf55cedb
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="the-rotate-transform"></a>회전 변환

_및 SkiaSharp 회전 변환을 사용 하 여 가능한 애니메이션 효과 탐색 합니다._

회전 변환을 사용 하 여 SkiaSharp 그래픽 개체 중단 가로 및 세로 축이 있는 무료 맞춤의 제약 조건입니다.

![](rotate-images/rotateexample.png "가운데를 중심으로 회전 된 텍스트")

모두 지원 하 여 SkiaSharp 지점 (0, 0), 그래픽 개체를 회전 하기 위한는 [ `RotateDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateDegrees/p/System.Single/) 메서드 및 [ `RotateRadians` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateRadians/p/System.Single/) 메서드:

```csharp
public void RotateDegrees (Single degrees)

public Void RotateRadians (Single radians)
```

360 원을 두 명의 단위 사이 변환할 쉽게 2 π 라디안와 같습니다. 알맞은 형식를 사용 합니다. 모든 삼각 함수에는 정적 [ `Math` ](https://developer.xamarin.com/api/type/System.Math/) 클래스 라디안의 단위를 사용 합니다.

회전 각도 높이기 위한 시계 반대 방향입니다. (규칙에 따라 시계 반대 방향으로 회전 데카르트 좌표계에 경우에 시계 방향 회전은 진행 중인 아래로 증가 Y 좌표와 일치 합니다.) 각도 각도가 360도 허용 하는 것 보다 큰 음수입니다.

회전에 대 한 변형 수식은 번역 및 크기 조정에 대 한 보다 더 복잡 합니다. Α 각도로 변환 수식은 다음과 같습니다.

x' = x•cos(α) – y•sin(α)   

y` = x•sin(α) + y•cos(α)

**기본 회전** 페이지에서는 `RotateDegrees` 메서드. [ `BasicRotate.xaml.cs` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BasicRotatePage.xaml.cs) 와 해당 페이지의 중앙에 일부 텍스트를 표시 하 고 기반으로 회전 하는 파일을 `Slider` 360 –360의 범위를 합니다. 다음의 관련 부분을은 `PaintSurface` 처리기:

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

이 프로그램에서 설정 하는 대부분 각도 캔버스의 왼쪽 위 모서리를 중심으로 하는 회전 하기 때문에 화면 밖 텍스트의 회전 합니다.

[![](rotate-images/basicrotate-small.png "페이지 기본 회전의 삼중 스크린 샷")](rotate-images/basicrotate-large.png#lightbox "페이지 기본 회전의 삼중 스크린 샷")

이러한 버전을 사용 하 여 지정한 피벗 포인트를 중심 무언가 회전 해야 하는 경우가 매우 자주는 [ `RotateDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateDegrees/p/System.Single/System.Single/System.Single/) 및 [ `RotateRadians` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateRadians/p/System.Single/System.Single/System.Single/) 메서드:

```csharp
public void RotateDegrees (Single degrees, Single px, Single py)

public void RotateRadians (Single radians, Single px, Single py)
```

**회전 중심** 페이지와 동일 하 게 되는 **기본 회전** 점을 제외 하 고 확장 된 버전의는 `RotateDegrees` 회전 중심의 텍스트를 배치 하는 데도 동일한 지점으로 설정 하는 데 사용은:

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

이제 텍스트 텍스트는 텍스트의 기준의 가로 중앙 위치를 지정 하는 데 사용 되는 지점 중심으로 회전 합니다.

[![](rotate-images/centeredrotate-small.png "회전 중심 페이지의 삼중 스크린샷")](rotate-images/centeredrotate-large.png#lightbox "페이지 회전 중심의 삼중 스크린 샷")

가운데에 맞출지 버전의 경우와 마찬가지로 `Scale` 메서드, 가운데에 맞출지 버전은 `RotateDegrees` 호출 바로 가기는:

```csharp
RotateDegrees (degrees, px, py);
```

이 식은 다음 식과 같습니다.

```csharp
canvas.Translate(px, py);
canvas.RotateDegrees(degrees);
canvas.Translate(-px, -py);
```

경우에 따라 결합할 수 있는지 알 수 `Translate` 으로 호출 하 여 `Rotate` 호출 합니다. 예를 들어 다음은 `RotateDegrees` 및 `DrawText` 에서 호출 된 **회전 중심** 페이지;

```csharp
canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`RotateDegrees` 호출 두 하는 것 `Translate` 호출과 중심 비 `RotateDegrees`:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`DrawText` 특정 위치에 텍스트를 표시 하는 호출 하는 것을 `Translate` 뒤 해당 위치에 대 한 호출 `DrawText` 지점 (0, 0):

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.DrawText(Title, 0, 0, textPaint);
```

연속 된 두 `Translate` 호출 서로 위배:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.DrawText(Title, 0, 0, textPaint);
```

개념적으로, 두 가지 변환은 코드에 표시 되는 방식을 반대 순서로 적용 됩니다. `DrawText` 호출 캔버스의 왼쪽 위 모서리에 텍스트를 표시 합니다. `RotateDegrees` 왼쪽 위 모퉁이 기준으로 해당 텍스트를 회전 하는 호출 합니다. 그런 다음 `Translate` 호출 캔버스의 가운데에 텍스트를 이동 합니다.

회전 및 번역을 결합 하는 여러 가지 방법으로 일반적으로 합니다. **텍스트 회전** 페이지 다음 디스플레이 만듭니다.

[![](rotate-images/rotatedtext-small.png "회전 된 텍스트 페이지의 삼중 스크린샷")](rotate-images/rotatedtext-large.png#lightbox "회전 된 텍스트 페이지의 삼중 스크린 샷")

다음은 `PaintSurface` 의 처리기는 [ `RotatedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/RotatedTextPage.cs) 클래스:

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

`xCenter` 및 `yCenter` 캔버스의 가운데 값을 나타냅니다. `yText` 값이 약간 하에서 5 만큼 오프셋 합니다. 이 페이지에서 세로로 진정으로 가운데에 배치 되므로 텍스트를 배치 하는 데 필요한 Y 좌표를 나타냅니다. `for` 루프 캔버스의 가운데에 중심이 회전을 설정 합니다. 30 도씩에서 회전이입니다. 사용 하 여 텍스트를 그리는 `yText` 값입니다. "회전" 이라는 단어 앞에 있는 공백의 수는 `text` 값은 dodecagon 것 처럼 이러한 12 텍스트 문자열 간의 연결을 만들 실험적으로 확인 되었습니다.

이 코드를 단순화 하는 한 가지 방법은 회전 각도를 30도 후 루프를 순환할 때마다 증가 하는 `DrawText` 호출 합니다. 이에 대 한 호출 않아도 `Save` 및 `Restore`합니다. 에 `degrees` 변수 본문 내에서 더 이상 사용 되는 `for` 블록:

```csharp
for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, xCenter, yText, textPaint);
    canvas.RotateDegrees(30, xCenter, yCenter);
}

```

간단한 형태를 사용 하 여 이기도 `RotateDegrees` 에 대 한 호출을 사용 하 여 루프 앞으로 `Translate` 캔버스의 가운데에 모든 항목을 이동 하려면:

```csharp
float yText = -textBounds.Height / 2 - textBounds.Top;

canvas.Translate(xCenter, yCenter);

for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, 0, yText, textPaint);
    canvas.RotateDegrees(30);
}
```

수정 된 `yText` 계산을 더 이상 통합 `yCenter`합니다. 이제는 `DrawText` 호출 캔버스의 위쪽에 세로로 텍스트 가운데에 맞춥니다.

변환을 반대 코드에 표시 되는 방식을 개념적으로 적용 되므로, 것이 가능한 더 포괄적으로 시작 변환, 더 많은 로컬 변형 옵니다. 이것이 회전 및 번역을 결합 하는 가장 쉬운 방법은 종종입니다.

예를 들어 해당 축에 회전 하 여 전 세계 매우 유사 하 게의 중심을 기준으로 회전 하는 그래픽 개체를 그리는 데 한다고 가정 합니다. 하지만이 개체를 선 주위를 회전 하 여 전 세계 매우 유사 하 게 화면의 가운데를 중심으로 할 수도 있습니다.

개체를 캔버스의 왼쪽 위 모퉁이에 배치 하 고 다음 애니메이션을 사용 하 여 해당 모퉁이 중심으로 회전 하 여이 수행할 수 있습니다. 궤도 radius 가로로 처럼 개체를 다음으로 변환 합니다. 이제 원점을 두 번째 애니메이션된 회전을 적용 합니다. 이것은 모퉁이 중심으로 하는 개체입니다. 이제 캔버스의 가운데에 변환 합니다.

다음은 `PaintSurface` 이러한를 포함 하는 처리기 호출을 역순으로 변환 합니다.

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

`revolveDegrees` 및 `rotateDegrees` 필드 애니메이션을 적용 합니다. 이 프로그램에서는 다른 애니메이션의 Xamarin.Forms에 따라 `Animation` 클래스입니다. (이 클래스는에 설명 된 [의 22 장 *Xamarin.Forms 사용 하 여 모바일 앱 만들기*](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf))는 `OnAppearing` 재정의에서는 두 개의 `Animation` 콜백 메서드와 개체를 다음 호출`Commit` 애니메이션 기간에 대 한에:

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

첫 번째 `Animation` 개체 애니메이션 효과 적용 `revolveDegrees` 0 ~ 360도 10 초 이상. 두 번째 애니메이션 효과 적용 `rotateDegrees` 360도 0에서 1 그리고 두 번째 화면을 무효화에 대 한 호출이 생성 하는 `PaintSurface` 처리기입니다. `OnDisappearing` 재정의 이러한 두 애니메이션을 취소 합니다.

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    this.AbortAnimation("revolveAnimation");
    this.AbortAnimation("rotateAnimation");
}
```

**까다로운 아날로그 시계의** 프로그램 (좋게 아날로그 시계의 이후 문서에서 설명 합니다 때문에 이렇게 부름)를 사용 하 여 회전 클록의 분, 시간 표시를 그립니다 바늘 회전 합니다. 프로그램에서 가운데에 점 (0, 0)는 반지름이 인 100 하는 원에 따라 임의의 좌표 시스템을 사용 하는 클록을 그립니다. 확장 하 여 페이지에서 해당 원의 중심 번역 및 배율을 사용 합니다.

`Translate` 및 `Scale` 호출에 전체적으로 적용 클록 첫 번째 집합을 초기화 하는 다음 호출 하는 `SKPaint` 개체:

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

```csharp
There are 60 marks of two different sizes that must be drawn in a circle around the clock. The `DrawCircle` call draws that circle at the point (0, –90), which relative to the center of the clock corresponds to 12:00. The `RotateDegrees` call increments the rotation angle by 6 degrees after every tick mark. The `angle` variable is used solely to determine if a large circle or a small circle is drawn:

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

마지막으로 `PaintSurface` 처리기는 현재 시간을 가져오고 시간, 분 및 두 번째 포인터에 대 한 회전 각도 계산 합니다. 회전 각도 기준으로 않도록 각 포인터 12시 위치에 그려집니다.

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

클록이 바늘은 다소 조잡 하지만 확실히 기능:

[![](rotate-images/uglyanalogclock-small.png "삼중 까다로운 아날로그 클록 텍스트 페이지의 스크린샷")](rotate-images/uglyanalogclock-large.png#lightbox "Triple screenshot of the Ugly Analog page")


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
