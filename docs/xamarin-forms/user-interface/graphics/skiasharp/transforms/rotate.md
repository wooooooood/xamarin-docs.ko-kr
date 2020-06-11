---
제목: "회전 변환" 설명: "이 문서에서는 SkiaSharp 회전 변환으로 가능한 효과와 애니메이션을 알아보고 샘플 코드를 사용 하 여이를 보여 줍니다."
ms. prod: xamarin. 기술: xamarin-skiasharp assetid: CBB3CD72-4377-4EA3-A768-0C4228229FC2 author: davidbritch: dabritch: 03/23/2017:-loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="the-rotate-transform"></a>회전 변환

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp 회전 변환으로 가능한 효과 및 애니메이션 탐색_

회전 변환을 사용 하면 SkiaSharp graphics 개체가 가로 축과 세로 축에 대 한 정렬의 제약을 해제 합니다.

![](rotate-images/rotateexample.png "Text rotated around a center")

점 (0, 0)을 중심으로 그래픽 개체를 회전 하는 경우 SkiaSharp는 메서드와 메서드를 모두 지원 합니다 [`RotateDegrees`](xref:SkiaSharp.SKCanvas.RotateDegrees(System.Single)) [`RotateRadians`](xref:SkiaSharp.SKCanvas.RotateRadians(System.Single)) .

```csharp
public void RotateDegrees (Single degrees)

public Void RotateRadians (Single radians)
```

360 각도의 원은 twoπ radians와 동일 하므로 두 단위 간에 쉽게 변환할 수 있습니다. 편리한 방법을 사용 합니다. .NET 클래스의 모든 삼각 함수는 [`Math`](xref:System.Math) 라디안 단위를 사용 합니다.

각도를 높이기 위해 회전이 시계 방향으로 있습니다. 데카르트 좌표계의 회전은 규칙에 따라 시계 반대 이지만 시계 방향 회전은 SkiaSharp에서와 같이 아래로 이동 하는 Y 좌표와 일치 합니다. 360도를 초과 하는 각도와 각도를 사용할 수 있습니다.

회전을 위한 변환 수식은 변환 및 크기 조정에 사용할 수 있는 것 보다 더 복잡 합니다. Α 각도의 경우 변형 수식은 다음과 같습니다.

x ' = x • cos (α) – y • sin (α)   

y ' = x • sin (α) + y • cos (α)

**기본 회전** 페이지에서는 메서드를 보여 줍니다 `RotateDegrees` . [**BasicRotate.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicRotatePage.xaml.cs) 파일은 페이지 중심의 기준선과 함께 일부 텍스트를 표시 하 고 `Slider` -360 ~ 360 범위의을 기준으로 회전 합니다. 처리기의 관련 부분은 `PaintSurface` 다음과 같습니다.

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

회전은 캔버스의 왼쪽 위 모퉁이를 중심으로 하므로이 프로그램에 설정 된 대부분의 각도에 대해 텍스트가 화면에서 회전 합니다.

[![](rotate-images/basicrotate-small.png "Triple screenshot of the Basic Rotate page")](rotate-images/basicrotate-large.png#lightbox "Triple screenshot of the Basic Rotate page")

이러한 버전의 및 메서드를 사용 하 여 지정 된 피벗 점을 중심으로 한 항목을 회전 하는 경우가 많습니다 [`RotateDegrees`](xref:SkiaSharp.SKCanvas.RotateDegrees(System.Single,System.Single,System.Single)) [`RotateRadians`](xref:SkiaSharp.SKCanvas.RotateRadians(System.Single,System.Single,System.Single)) .

```csharp
public void RotateDegrees (Single degrees, Single px, Single py)

public void RotateRadians (Single radians, Single px, Single py)
```

**가운데 회전** 페이지는 확장 된 버전의를 사용 하 여 회전 중심을 텍스트를 배치 하는 데 사용 되는 것과 동일한 지점으로 설정 한다는 점을 제외 하 고는 **기본 회전과** 동일 합니다 `RotateDegrees` .

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

이제 텍스트를 배치 하는 데 사용 된 지점을 중심으로 텍스트를 회전 하며 텍스트 기준선의 가로 가운데입니다.

[![](rotate-images/centeredrotate-small.png "Triple screenshot of the Centered Rotate page")](rotate-images/centeredrotate-large.png#lightbox "Triple screenshot of the Centered Rotate page")

메서드를 중심으로 사용 하는 것과 마찬가지로 `Scale` , 호출의 중심 버전은 `RotateDegrees` 바로 가기입니다. 메서드는 다음과 같습니다.

```csharp
RotateDegrees (degrees, px, py);
```

이 호출은 다음에 해당 합니다.

```csharp
canvas.Translate(px, py);
canvas.RotateDegrees(degrees);
canvas.Translate(-px, -py);
```

호출 `Translate` 을 사용 하 여 호출을 결합할 수도 있습니다 `Rotate` . 예를 들어, 다음은 `RotateDegrees` `DrawText` **가운데 회전** 페이지의 및 호출입니다.

```csharp
canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`RotateDegrees`호출은 두 `Translate` 호출 및 가운데에 있지 않은와 동일 합니다 `RotateDegrees` .

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`DrawText`특정 위치에 텍스트를 표시 하는 호출은 `Translate` 해당 위치에 대 한 호출과 그 뒤의 `DrawText` 지점 (0, 0)에 해당 합니다.

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.DrawText(Title, 0, 0, textPaint);
```

연속 된 두 개의 `Translate` 호출은 서로를 취소 합니다.

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.DrawText(Title, 0, 0, textPaint);
```

개념적으로 두 변환은 코드에 표시 되는 순서와 반대 방향으로 적용 됩니다. 이 `DrawText` 호출은 캔버스의 왼쪽 위 모퉁이에 텍스트를 표시 합니다. `RotateDegrees`이 호출은 왼쪽 위 모퉁이를 기준으로 해당 텍스트를 회전 합니다. 그러면 `Translate` 호출은 텍스트를 캔버스의 가운데로 이동 합니다.

일반적으로 회전 및 변환을 결합 하는 방법에는 여러 가지가 있습니다. **회전 된 텍스트** 페이지는 다음과 같이 표시 됩니다.

[![](rotate-images/rotatedtext-small.png "Triple screenshot of the Rotated Text page")](rotate-images/rotatedtext-large.png#lightbox "Triple screenshot of the Rotated Text page")

`PaintSurface`클래스의 처리기는 [`RotatedTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/RotatedTextPage.cs) 다음과 같습니다.

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

`xCenter`및 `yCenter` 값은 캔버스의 중심을 표시 합니다. `yText`값은 그와는 약간의 오프셋입니다. 이 값은 페이지의 가운데에 세로로 배치 되도록 텍스트를 배치 하는 데 필요한 Y 좌표입니다. `for`그런 다음, 루프는 캔버스의 중심을 기준으로 회전을 설정 합니다. 회전이 30도 단위로 증가 합니다. 값을 사용 하 여 텍스트를 그립니다 `yText` . 값의 단어 "회전" 앞에 있는 공백 수는 `text` 이 12 텍스트 문자열 간의 연결이 dodecagon 알려지고 실험적으로 결정 되었습니다.

이 코드를 간소화 하는 한 가지 방법은 호출 후 루프를 통해 매번 회전 각도를 30도 씩 늘리는 것입니다 `DrawText` . 이렇게 하면 및에 대 한 호출이 필요 하지 `Save` `Restore` 않습니다. `degrees`변수는 블록의 본문 내에서 더 이상 사용 되지 않습니다 `for` .

```csharp
for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, xCenter, yText, textPaint);
    canvas.RotateDegrees(30, xCenter, yCenter);
}

```

`RotateDegrees`을 호출 하 여 루프를 앞 `Translate` 캔버스의 중심으로 모든 항목을 이동 하는 방법으로 간단한 형식을 사용할 수도 있습니다.

```csharp
float yText = -textBounds.Height / 2 - textBounds.Top;

canvas.Translate(xCenter, yCenter);

for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, 0, yText, textPaint);
    canvas.RotateDegrees(30);
}
```

수정 된 `yText` 계산은 더 이상 통합 되지 않습니다 `yCenter` . 이제 `DrawText` 호출은 캔버스 위쪽에서 텍스트를 세로 방향으로 가운데에 맞춥니다.

변환은 코드에 표시 되는 방식과 반대 되는 방식으로 적용 되기 때문에 일반적으로 더 많은 전역 변환을 시작한 후 더 많은 로컬 변환을 수행할 수 있습니다. 이는 대개 회전 및 변환을 결합 하는 가장 쉬운 방법입니다.

예를 들어 축을 중심으로 회전 하는 것 처럼 중심을 중심으로 회전 하는 그래픽 개체를 그리려는 경우를 가정 하겠습니다. 그러나이 개체는 태양 주위에 있는 전 세계의 중심을 기준으로 화면의 중심을 기준으로 회전 하려고 합니다.

캔버스의 왼쪽 위 모퉁이에 개체를 배치 하 고 애니메이션을 사용 하 여 해당 모퉁이 주위로 회전 하 여이 작업을 수행할 수 있습니다. 그런 다음 개체를 궤도 반지름과 같이 가로로 변환 합니다. 이제 원본 주위에도 두 번째 애니메이션 회전을 적용 합니다. 이렇게 하면 개체가 모퉁이를 중심으로 회전 합니다. 이제 캔버스의 중심으로 변환 합니다.

`PaintSurface`다음은 이러한 변환 호출을 역순으로 포함 하는 처리기입니다.

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

`revolveDegrees`및 `rotateDegrees` 필드에 애니메이션 효과가 적용 됩니다. 이 프로그램은 클래스를 기반으로 하는 다른 애니메이션 기술을 사용 Xamarin.Forms [`Animation`](xref:Xamarin.Forms.Animation) 합니다. 이 클래스는 [ *를 사용 하 Xamarin.Forms 여 Mobile Apps 만들기 *의 22 장 ](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)에서 설명 합니다 `OnAppearing` . 재정의는 `Animation` 콜백 메서드를 사용 하 여 두 개의 개체를 만든 다음 `Commit` 애니메이션 기간 동안 호출 합니다.

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

첫 번째 `Animation` 개체는 `revolveDegrees` 10 초 동안 0에서 360도까지 애니메이션 효과를 적용 합니다. 두 번째는 1 초 마다 0도에서 360까지 애니메이션 효과를 적용 하 `rotateDegrees` 고, 화면을 무효화 하 여 처리기에 대 한 다른 호출을 생성 합니다 `PaintSurface` . `OnDisappearing`재정의는 다음 두 가지 애니메이션을 취소 합니다.

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    this.AbortAnimation("revolveAnimation");
    this.AbortAnimation("rotateAnimation");
}
```

사용이 **편리한 아날로그 클록 프로그램 (** 이후 문서에서 더 매력적인 아날로그 클록이 설명 됨)은 회전을 사용 하 여 시계의 분 및 시간 표시를 그리고 손으로 회전 합니다. 이 프로그램은 반지름이 100 인 지점 (0, 0)을 중심으로 하는 원을 기반으로 하는 임의의 좌표계를 사용 하 여 시계를 그립니다. 번역 및 크기 조정을 사용 하 여 페이지에서 해당 원을 확장 하 고 가운데에 맞춥니다.

`Translate`및 `Scale` 호출은 클록에 전체적으로 적용 되므로 개체가 초기화 된 후에 호출 되는 첫 번째 호출입니다 `SKPaint` .

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

시계 주위에 원 안에 그려야 하는 두 가지 크기의 60 표시가 있습니다. `DrawCircle`이 호출은 클록의 중심을 기준으로 12:00에 해당 하는 점 (0, – 90)에 원을 그립니다. 이 `RotateDegrees` 호출은 모든 눈금 표시 후에 회전 각도를 6 도씩 늘립니다. `angle`변수는 크거나 작은 원이 그려지는 지를 결정 하는 데만 사용 됩니다.

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

마지막으로 `PaintSurface` 처리기는 현재 시간을 가져오고 시간, 분 및 초 바늘의 회전 각도를 계산 합니다. 각 손을 12:00 위치에 그리면 회전 각도가 해당에 상대적입니다.

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

시계는 다음과 같이 조잡 됩니다.

[![](rotate-images/uglyanalogclock-small.png "Triple screenshot of the Ugly Analog Clock Text page")](rotate-images/uglyanalogclock-large.png#lightbox "Triple screenshot of the Ugly Analog page")

더 매력적인 clock은 [**SkiaSharp의 SVG 경로 데이터**](../curves/path-data.md)문서를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
