---
title: Porter 임신 blend 모드
description: 원본 및 대상 이미지를 기반으로 하는 백그라운드에서 작성 하는 Porter 임신 blend 모드를 사용 합니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 57F172F8-BA03-43EC-A215-ED6B78696BB5
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: c15dd4606a75cc3cdffbad71f15299568157213a
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306514"
---
# <a name="porter-duff-blend-modes"></a>Porter 임신 blend 모드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Thomas Porter Tom 임신 Lucasfilm 근무 하면서 합성는 대 수를 개발한 후 Porter 임신 blend 모드 라고 합니다. [_디지털 이미지를 합성_](https://graphics.pixar.com/library/Compositing/paper.pdf) 하는 백서는 _컴퓨터 그래픽_의 7 월 1984, 253 페이지에서 259로 게시 되었습니다. 이러한 혼합 모드는 합치기를 복합 장면에 다양 한 이미지를 구축 하는 데 필요한:

![Porter-Duff 샘플](porter-duff-images/PorterDuffSample.png "Porter-Duff 샘플")

## <a name="porter-duff-concepts"></a>Porter 임신 개념

갈색 사각형의 왼쪽 및 위쪽의 2/3 화면에 표시를 차지 한다고 가정 합니다.

![Porter-Duff 대상](porter-duff-images/PorterDuffDst.png "Porter-Duff 대상")

이 영역을 _대상_ 또는 때때로 _배경 또는 배경_ 이라고 합니다 _._

대상의 크기가 같은 다음 영역을 그릴 하려고 합니다. 사각형은 오른쪽 아래 2 / 3 차지 하는 파랑 추가 영역을 제외 하 고 투명 합니다.

![Porter-Duff 원본](porter-duff-images/PorterDuffSrc.png "Porter-Duff 원본")

이를 _원본_ 또는 경우에 따라 _전경_이라고 합니다.

대상에 소스를 표시할 때 예상 대로 다음과 같습니다.

![Porter-Duff 원본](porter-duff-images/PorterDuffSrcOver.png "Porter-Duff 원본")

원본의 투명 한 픽셀 배경을 파랑 추가 소스 픽셀 백그라운드 모호 하 게 하는 동안를 통해 볼 수 있습니다. 이것은 일반적인 경우 이며 SkiaSharp에서 `SKBlendMode.SrcOver`로 참조 됩니다. 이 값은 `SKPaint` 개체를 처음 인스턴스화할 때 `BlendMode` 속성의 기본 설정입니다.

그러나 다른 효과 대 한 다른 blend 모드를 지정 하는 것이 같습니다. `SKBlendMode.DstOver`지정 하는 경우 원본과 대상이 교차 하는 영역에서 대상이 다음과 같이 표시 됩니다.

![Porter-Duff Destination Over](porter-duff-images/PorterDuffDstOver.png "Porter-Duff Destination Over")

`SKBlendMode.DstIn` blend 모드에는 대상 색을 사용 하 여 대상 및 원본이 교차 하는 영역만 표시 됩니다.

![Porter-Duff Destination In](porter-duff-images/PorterDuffDstIn.png "Porter-Duff Destination In")

`SKBlendMode.Xor` (배타적 OR)의 blend 모드에서는 두 영역이 겹치는 위치에 아무 것도 표시 되지 않습니다.

![Porter-Duff 배타 또는](porter-duff-images/PorterDuffXor.png "Porter-Duff 배타 또는")

색이 지정 된 대상 및 소스 사각형 대상 및 소스 사각형의 현재 상태에 해당 하는 다양 한 방법으로 색칠 할 수 있는 4 개의 고유 영역으로 화면을 효과적으로 나눕니다.

![Porter-Duff](porter-duff-images/PorterDuff.png "Porter 임신")

왼쪽 및 오른쪽 위 사각형은 해당 영역에 대상 및 원본 투명 한 있기 때문에 항상 비어 있습니다. 대상 색 영역 대상 색과 나타나거나 전혀 나타나지 색칠 할 하거나 수 있도록 왼쪽 영역을 차지 합니다. 마찬가지로, 소스 색 또는 전혀 소스 색으로 영역을 색칠 할 수 있도록 오른쪽 아래 영역을 차지 합니다. 대상 색, 소스 색 또는 전혀 사용 하지 않을 중간에서 원본과 대상의 교집합을 색칠 할 수 있습니다.

조합 총 수는 (센터용), 3 또는 12 시간 (오른쪽 아래)에 대 한 2 시간 (왼쪽 위)에 2입니다. 이들은 12 기본 Porter 임신 혼합 모드입니다.

_디지털 이미지 합성_ (256 페이지), Porter 및 duff를 사용 하 여 13 번째 모드를 추가 _합니다 (SkiaSharp_ `SKBlendMode.Plus` 멤버와 w3c _연하게_ _모드와_ 혼동 하지 않음). 이 `Plus` 모드는 대상 및 원본 색을 추가 합니다 .이 프로세스에 대 한 자세한 내용은 곧 설명 하겠습니다.

14 일 ia는 대상 및 원본 색에 곱할 때를 제외 하 고 `Plus`와 매우 유사한 `Modulate` 라는 모드를 추가 합니다. 추가 Porter 임신 blend 모드로 처리할 수 있습니다.

SkiaSharp에 정의 된 대로 14 Porter 임신 모드는 다음과 같습니다. 테이블 위의 다이어그램에는 세 가지 비어 있지 않은 영역이의 각 색은 방법을 보여 줍니다.

| Mode       | 대상 | 교집합 | 원본 |
| ---------- |:-----------:|:------------:|:------:|
| `Clear`    |             |              |        |
| `Src`      |             | 원본       | X      |
| `Dst`      | X           | 대상  |        |
| `SrcOver`  | X           | 원본       | X      |
| `DstOver`  | X           | 대상  | X      |
| `SrcIn`    |             | 원본       |        |
| `DstIn`    |             | 대상  |        |
| `SrcOut`   |             |              | X      |
| `DstOut`   | X           |              |        |
| `SrcATop`  | X           | 원본       |        |
| `DstATop`  |             | 대상  | X      |
| `Xor`      | X           |              | X      |
| `Plus`     | X           | 합계          | X      |
| `Modulate` |             | Product      |        | 

이러한 혼합 모드가 대칭으로 나타납니다. 원본 및 대상 교환할 수 있으며 모든 모드는 계속 사용할 수 있습니다.

모드의 명명 규칙에는 몇 가지 간단한 규칙을 따릅니다.

- **Src** 또는 **Dst** 자체는 원본 또는 대상 픽셀만 표시 됨을 의미 합니다.
- **Over** 접미사는 교차에 표시 되는 항목을 나타냅니다. 원본 또는 대상 "끝" 다른 그려집니다.
- 접미사 **In** 접미사는 교차 부분만 색으로 지정 됨을 의미 합니다. 출력 소스 또는 대상 "에 있는" 다른 부분만으로 제한 됩니다.
- **출력** 접미사는 교집합이 색으로 지정 되지 않음을 의미 합니다. 출력은 부분에만 소스 또는 대상 "out" 교집합입니다.
- 가장 **위에** 있는 접미사는 **In** 및 **Out**의 합집합입니다. 여기에는 원본 또는 대상이 다른 영역을 포함 하는 영역이 포함 됩니다.

`Plus` 및 `Modulate` 모드의 차이를 확인 합니다. 이러한 모드는 원본 및 대상 픽셀에서 다른 유형의 계산을 수행 합니다. 곧 자세히 설명 되어는 있습니다.

**Porter-Duff 그리드** 페이지에는 표 형태의 한 화면에 있는 14 개의 모드가 모두 표시 됩니다. 각 모드는 `SKCanvasView`별도의 인스턴스입니다. 따라서 클래스는 `PorterDuffCanvasView`라는 `SKCanvasView`에서 파생 됩니다. 정적 생성자의 왼쪽 위 영역에 갈색 사각형이 하나 및 파랑 추가 사각형을 사용 하 여 동일한 크기의 두 비트맵을 만듭니다.

```csharp
class PorterDuffCanvasView : SKCanvasView
{
    static SKBitmap srcBitmap, dstBitmap;

    static PorterDuffCanvasView()
    {
        dstBitmap = new SKBitmap(300, 300);
        srcBitmap = new SKBitmap(300, 300);

        using (SKPaint paint = new SKPaint())
        {
            using (SKCanvas canvas = new SKCanvas(dstBitmap))
            {
                canvas.Clear();
                paint.Color = new SKColor(0xC0, 0x80, 0x00);
                canvas.DrawRect(new SKRect(0, 0, 200, 200), paint);
            }
            using (SKCanvas canvas = new SKCanvas(srcBitmap))
            {
                canvas.Clear();
                paint.Color = new SKColor(0x00, 0x80, 0xC0);
                canvas.DrawRect(new SKRect(100, 100, 300, 300), paint);
            }
        }
    }
    ···
}
```

인스턴스 생성자에 `SKBlendMode`형식의 매개 변수가 있습니다. 이 매개 변수를 필드에 저장합니다. 

```csharp
class PorterDuffCanvasView : SKCanvasView
{
    ···
    SKBlendMode blendMode;

    public PorterDuffCanvasView(SKBlendMode blendMode)
    {
        this.blendMode = blendMode;
    }

    protected override void OnPaintSurface(SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find largest square that fits
        float rectSize = Math.Min(info.Width, info.Height);
        float x = (info.Width - rectSize) / 2;
        float y = (info.Height - rectSize) / 2;
        SKRect rect = new SKRect(x, y, x + rectSize, y + rectSize);

        // Draw destination bitmap
        canvas.DrawBitmap(dstBitmap, rect);

        // Draw source bitmap
        using (SKPaint paint = new SKPaint())
        {
            paint.BlendMode = blendMode;
            canvas.DrawBitmap(srcBitmap, rect, paint);
        }

        // Draw outline
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Black;
            paint.StrokeWidth = 2;
            rect.Inflate(-1, -1);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

`OnPaintSurface` 재정의는 두 개의 비트맵을 그립니다. 첫 번째 일반적으로 그려집니다.

```csharp
canvas.DrawBitmap(dstBitmap, rect);
```

두 번째는 `BlendMode` 속성이 생성자 인수로 설정 된 `SKPaint` 개체를 사용 하 여 그려집니다.

```csharp
using (SKPaint paint = new SKPaint())
{
    paint.BlendMode = blendMode;
    canvas.DrawBitmap(srcBitmap, rect, paint);
}
```

`OnPaintSurface` 재정의의 나머지 부분은 비트맵 주위에 사각형을 그려 크기를 표시 합니다.

`PorterDuffGridPage` 클래스는 `blendModes` 배열의 각 멤버에 대해 하나씩, `PorterDurffCanvasView`14 인스턴스를 만듭니다. 배열의 `SKBlendModes` 멤버 순서는 비슷한 모드를 서로 인접 하 게 배치 하기 위해 테이블과 약간 다릅니다. `PorterDuffCanvasView`의 14 개 인스턴스는 `Grid`의 레이블과 함께 구성 됩니다.

```csharp
public class PorterDuffGridPage : ContentPage
{
    public PorterDuffGridPage()
    {
        Title = "Porter-Duff Grid";

        SKBlendMode[] blendModes =
        {
            SKBlendMode.Src, SKBlendMode.Dst, SKBlendMode.SrcOver, SKBlendMode.DstOver,
            SKBlendMode.SrcIn, SKBlendMode.DstIn, SKBlendMode.SrcOut, SKBlendMode.DstOut,
            SKBlendMode.SrcATop, SKBlendMode.DstATop, SKBlendMode.Xor, SKBlendMode.Plus,
            SKBlendMode.Modulate, SKBlendMode.Clear
        };

        Grid grid = new Grid
        {
            Margin = new Thickness(5)
        };

        for (int row = 0; row < 4; row++)
        {
            grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
            grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Star });
        }

        for (int col = 0; col < 3; col++)
        {
            grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });
        }

        for (int i = 0; i < blendModes.Length; i++)
        {
            SKBlendMode blendMode = blendModes[i];
            int row = 2 * (i / 4);
            int col = i % 4;

            Label label = new Label
            {
                Text = blendMode.ToString(),
                HorizontalTextAlignment = TextAlignment.Center
            };
            Grid.SetRow(label, row);
            Grid.SetColumn(label, col);
            grid.Children.Add(label);

            PorterDuffCanvasView canvasView = new PorterDuffCanvasView(blendMode);

            Grid.SetRow(canvasView, row + 1);
            Grid.SetColumn(canvasView, col);
            grid.Children.Add(canvasView);
        }

        Content = grid;
    }
}
```

결과:

[![Porter-Duff 그리드](porter-duff-images/PorterDuffGrid.png "Porter-Duff 그리드")](porter-duff-images/PorterDuffGrid-Large.png#lightbox)

투명도 Porter 임신 blend 모드를 적절 하 게 작동 하는 데 중요 임을 직접 확인 해야 합니다. `PorterDuffCanvasView` 클래스에는 `Canvas.Clear` 메서드에 대 한 총 세 개의 호출이 포함 되어 있습니다. 모든 픽셀을 모두 투명 하 게 설정 하는 매개 변수가 없는 메서드를 사용 합니다.

```csharp
canvas.Clear();
```

픽셀 불투명 흰색으로 설정 되어 있도록 해당 호출을 변경해 보세요.

```csharp
canvas.Clear(SKColors.White);
```

해당 변경 내용을 다음 작동 하는 것 처럼 보이고 일부 blend 모드 있지만 일부는 그렇지 않습니다. 원본 비트맵의 배경을 흰색으로 설정 하면 소스 비트맵에 투명 한 픽셀이 없어 대상에 표시 될 수 있기 때문에 `SrcOver` 모드가 작동 하지 않습니다. 대상 비트맵 또는 캔버스의 배경을 흰색으로 설정 하는 경우 대상에 투명 픽셀이 없으므로 `DstOver` 작동 하지 않습니다.

**Porter-Duff 그리드** 페이지의 비트맵을 간단한 `DrawRect` 호출로 바꾸는 하려는 유혹 있을 수 있습니다. 소스 사각형 아니라 대상 사각형에 대 한 작동 합니다. 소스 사각형 이상의 파랑 추가 색 영역 범위가 포함 되어야 합니다. 소스 사각형에는 대상의 색이 지정 된 영역에 해당 하는 투명 한 영역을 포함 해야 합니다. 그런 후에 모드 작업을 조화 이러한 합니다.

## <a name="using-mattes-with-porter-duff"></a>Porter 임신 매트 사용

**Brick 합성** 페이지는 클래식 합성 작업의 예를 보여 줍니다. 제거 해야 하는 배경의 비트맵을 포함 하 여 여러 부분에서 그림을 조합 해야 합니다. 문제가 있는 배경의 **Seatedmonkey jpg** 비트맵은 다음과 같습니다.

![꽂혀 있는 원숭이](porter-duff-images/SeatedMonkey.jpg "꽂혀 있는 원숭이")

합성을 준비 하는 경우 이미지를 표시 하 고 그렇지 않은 경우 검정색 인 또 다른 비트맵 인 해당 _매트가_ 생성 됩니다. 이 파일의 이름은 **Seatedmonkeymatte .png** 이 고 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플의 **미디어** 폴더에 있는 리소스 중 하나입니다.

![꽂혀 있는 원숭이 무광택](porter-duff-images/SeatedMonkeyMatte.png "꽂혀 있는 원숭이 무광택")

Expertly 생성 된 매트가 _아닙니다_ . 최적으로 매트 부분적으로 투명 한 검정 픽셀의 가장자리 픽셀을 포함 해야 하 고이 매트 하지 않습니다.

**Brick** 구성 페이지에 대 한 XAML 파일은 최종 이미지를 작성 하는 프로세스를 통해 사용자에 게 안내 하는 `SKCanvasView` 및 `Button`를 인스턴스화합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.BrickWallCompositingPage"
             Title="Brick-Wall Compositing">

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Button Text="Show sitting monkey"
                HorizontalOptions="Center"
                Margin="0, 10"
                Clicked="OnButtonClicked" />

    </StackLayout>
</ContentPage>
```

코드를 로드 하는 파일은 필요한 두 비트맵을 로드 하 고 `Button`의 `Clicked` 이벤트를 처리 합니다. `Button` 클릭할 때마다 `step` 필드가 증가 하 고 `Button`에 대해 새 `Text` 속성이 설정 됩니다. `step` 5에 도달 하면 다시 0으로 설정 됩니다.

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    SKBitmap monkeyBitmap = BitmapExtensions.LoadBitmapResource(
        typeof(BrickWallCompositingPage), 
        "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    SKBitmap matteBitmap = BitmapExtensions.LoadBitmapResource(
        typeof(BrickWallCompositingPage), 
        "SkiaSharpFormsDemos.Media.SeatedMonkeyMatte.png");

    int step = 0;

    public BrickWallCompositingPage ()
    {
        InitializeComponent ();
    }

    void OnButtonClicked(object sender, EventArgs args)
    {
        Button btn = (Button)sender;
        step = (step + 1) % 5;

        switch (step)
        {
            case 0: btn.Text = "Show sitting monkey"; break;
            case 1: btn.Text = "Draw matte with DstIn"; break;
            case 2: btn.Text = "Draw sidewalk with DstOver"; break;
            case 3: btn.Text = "Draw brick wall with DstOver"; break;
            case 4: btn.Text = "Reset"; break;
        }

        canvasView.InvalidateSurface();
    }
    
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        ···
    }
}
```

프로그램을 처음 실행 하면 `Button`를 제외 하 고 아무 것도 표시 되지 않습니다.

[![Brick-벽 합성 0 단계](porter-duff-images/BrickWallCompositing0.png "Brick-벽 합성 0 단계")](porter-duff-images/BrickWallCompositing0-Large.png#lightbox)

`Button`를 한 번 누르면 `step` 1 씩 증가 하 고 `PaintSurface` 처리기는 이제 **Seatedmonkey jpg**를 표시 합니다.

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        float x = (info.Width - monkeyBitmap.Width) / 2;
        float y = info.Height - monkeyBitmap.Height;

        // Draw monkey bitmap
        if (step >= 1)
        {
            canvas.DrawBitmap(monkeyBitmap, x, y);
        }
        ···
    }
}
```

`SKPaint` 개체가 없으므로 blend 모드가 없습니다. 비트맵 화면 맨 아래에 나타납니다.

[![Brick-벽 합성 1 단계](porter-duff-images/BrickWallCompositing1.png "Brick-벽 합성 1 단계")](porter-duff-images/BrickWallCompositing1-Large.png#lightbox)

`Button`를 다시 누르고 `step`를 2로 늘립니다. 이는 **Seatedmonkeymatte .png** 파일을 표시 하는 중요 한 단계입니다.

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        // Draw matte to exclude monkey's surroundings
        if (step >= 2)
        {
            using (SKPaint paint = new SKPaint())
            {
                paint.BlendMode = SKBlendMode.DstIn;
                canvas.DrawBitmap(matteBitmap, x, y, paint);
            }
        }
        ···
    }
}
```

Blend 모드는 `SKBlendMode.DstIn`됩니다. 즉, 대상이 원본의 투명 하지 않은 영역에 해당 하는 영역에 유지 됩니다. 원래 비트맵에 해당 하는 대상 사각형의 나머지 부분 투명해 집니다.

[![Brick 합성 2 단계](porter-duff-images/BrickWallCompositing2.png "Brick 합성 2 단계")](porter-duff-images/BrickWallCompositing2-Large.png#lightbox)

배경 제거 되었습니다. 

다음 단계는 monkey 하나 더 있는데요는 인도 유사한 사각형을 그리려면입니다. 이 인도의 모양을 기반으로 두 셰이더 조합: 단색 셰이더 및 Perlin 노이즈 셰이더:

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        const float sidewalkHeight = 80;
        SKRect rect = new SKRect(info.Rect.Left, info.Rect.Bottom - sidewalkHeight,
                                 info.Rect.Right, info.Rect.Bottom);

        // Draw gravel sidewalk for monkey to sit on
        if (step >= 3)
        {
            using (SKPaint paint = new SKPaint())
            {
                paint.Shader = SKShader.CreateCompose(
                                    SKShader.CreateColor(SKColors.SandyBrown),
                                    SKShader.CreatePerlinNoiseTurbulence(0.1f, 0.3f, 1, 9));

                paint.BlendMode = SKBlendMode.DstOver;
                canvas.DrawRect(rect, paint);
            }
        }
        ···
    }
}
```

이 sidewalk는 원숭이 뒤에 있어야 하므로 blend 모드가 `DstOver`됩니다. 대상만 있는 배경은 투명 하 게 표시 됩니다.

[![Brick 합성 3 단계](porter-duff-images/BrickWallCompositing3.png "Brick 합성 3 단계")](porter-duff-images/BrickWallCompositing3-Large.png#lightbox)

마지막 단계는 brick 벽을 추가 합니다. 프로그램은 `AlgorithmicBrickWallPage` 클래스에서 정적 속성 `BrickWallTile` 사용할 수 있는 벽돌 벽 비트맵 타일을 사용 합니다. 번역 변환은 `SKShader.CreateBitmap` 호출에 추가 되어 아래쪽 행이 전체 타일이 되도록 타일을 이동 합니다.

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        // Draw bitmap tiled brick wall behind monkey
        if (step >= 4)
        {
            using (SKPaint paint = new SKPaint())
            {
                SKBitmap bitmap = AlgorithmicBrickWallPage.BrickWallTile;
                float yAdjust = (info.Height - sidewalkHeight) % bitmap.Height;

                paint.Shader = SKShader.CreateBitmap(bitmap,
                                                     SKShaderTileMode.Repeat,
                                                     SKShaderTileMode.Repeat,
                                                     SKMatrix.MakeTranslation(0, yAdjust));
                paint.BlendMode = SKBlendMode.DstOver;
                canvas.DrawRect(info.Rect, paint);
            }
        }
    }
}
```

편의를 위해 `DrawRect` 호출은 전체 캔버스에이 셰이더를 표시 하지만 `DstOver` 모드는 출력을 계속 해 서 투명 한 캔버스 영역으로 제한 합니다.

[![Brick 합성 4 단계](porter-duff-images/BrickWallCompositing4.png "Brick 합성 4 단계")](porter-duff-images/BrickWallCompositing4-Large.png#lightbox)

물론이 장면을 작성 하는 다른 방법은 있습니다. 이 백그라운드에서 시작 및 앞으로 진행 되 고 빌드될 수 있습니다. 하지만 혼합 모드를 사용 하 여 좀 더 유연 합니다. 특히 매트를 사용 하면 구성 된 장면에서 제외할 비트맵의 배경색입니다.

[경로 및 영역을 사용한 클리핑](../../curves/clipping.md)문서에서 배운 대로 `SKCanvas` 클래스는 [`ClipRect`](xref:SkiaSharp.SKCanvas.ClipRect*), [`ClipPath`](xref:SkiaSharp.SKCanvas.ClipPath*)및 [`ClipRegion`](xref:SkiaSharp.SKCanvas.ClipRegion*) 메서드에 해당 하는 세 가지 유형의 클리핑을 정의 합니다. Porter 임신 blend 모드는 다른 형식의 이미지를 그릴 수 있는, 비트맵을 포함 하 여 아무 것도 제한 수 있는 클리핑을 추가 합니다. **벽돌 벽 합성** 에 사용 되는 매트는 기본적으로 클리핑 영역을 정의 합니다.

## <a name="gradient-transparency-and-transitions"></a>그라데이션 투명도 및 전환

Porter 임신 예가이 문서의 모든 것을 의미 했습니다 불투명 한 픽셀 및 투명 한 픽셀을 부분적으로 투명 한 픽셀의로 구성 된 이미지는 앞에 표시 된 모드를 혼합 됩니다. Blend 모드 함수 픽셀 들도 정의 됩니다. 다음 표는 Porter- [**Duarggendmode 참조**](https://skia.org/user/api/SkBlendMode_Reference)에 있는 표기법을 사용 하는-duff blend 모드의 보다 공식적인 정의입니다. (C # **참조** )은 (는) 서 기 ia 참조 C++ 이므로 구문이 사용 됩니다.)

개념적으로 각 픽셀의 빨간색, 녹색, 파랑 및 알파 구성 요소 범위에서 부동 소수점 숫자 0에서 1 바이트에서 변환 됩니다. 알파 채널의 0은 완전히 투명 하 고 1은 완전히 불투명

아래 표에 표기법은 다음 약어를 사용 합니다.

- **Da** 는 대상 알파 채널입니다.
- **Dc** 는 대상 RGB 색입니다.
- **Sa** 는 원본 알파 채널입니다.
- **Sc** 는 원본 RGB 색입니다.

RGB 색은 알파 값이 미리 곱해집니다. 예를 들어 **Sc** 가 순수한 red를 나타내지만 **Sa** 가 0x80 인 경우 RGB 색은 **(0x80, 0, 0)** 입니다. **Sa** 가 0 인 경우 모든 RGB 구성 요소도 0입니다.

결과는 대괄호 안에 알파 채널과 함께 표시 되 고 RGB 색은 쉼표 ( **[alpha, color])** 로 구분 됩니다. 색에 대 한 계산 빨강, 녹색 및 파랑 구성 요소에 대해 개별적으로 수행 됩니다.

| Mode       | 작업(Operation) |
| ---------- | --------- |
| `Clear`    | [0, 0]    |
| `Src`      | [Sa를 Sc]  |
| `Dst`      | [Da Dc]  |
| `SrcOver`  | [Sa + Da· (1 일 – Sa) Sc + Dc· (1-Sa) | 
| `DstOver`  | [Da + Sa· (1-Da) Dc + Sc· (1-Da) |
| `SrcIn`    | [Sa· Da Sc· Da] |
| `DstIn`    | [Da· Sa, Dc· Sa] |
| `SrcOut`   | [Sa· (1-Da) Sc· (1-Da)] |
| `DstOut`   | [Da· (1 일 – Sa) Dc· (1 일 – Sa)] |
| `SrcATop`  | [Da Sc· Da + Dc· (1 일 – Sa)] |
| `DstATop`  | [Sa Dc· Sa + Sc· (1-Da)] |
| `Xor`      | [Sa + Da – 2· Sa· Da Sc· (1-Da) + Dc· (1 일 – Sa)] |
| `Plus`     | [Sa + Da, Sc + Dc] |
| `Modulate` | [Sa· Da Sc· Dc] | 

이러한 작업은 **Da** 와 **Sa** 가 0 또는 1 일 때 분석 하기가 더 쉽습니다. 예를 들어 기본 `SrcOver` 모드의 경우 **Sa** 가 0 이면 **Sc** 도 0이 고 결과는 **[Da, Dc]** , 대상 알파 및 색입니다. **Sa** 가 1 이면 결과는 **[Sa, sc]** , 원본 알파 및 색 또는 **[1, sc]** 입니다.

`Plus` 및 `Modulate` 모드는 원본과 대상의 조합으로 인해 새 색이 생성 될 수 있다는 것과는 약간 다릅니다. `Plus` 모드는 바이트 구성 요소나 부동 소수점 구성 요소를 사용 하 여 해석할 수 있습니다. 앞에 표시 된 **Porter-Duff 그리드** 페이지에서 대상 색은 **(0xc0, 0x80, 0x00)** 이 고 원본 색은 **(0X00, 0x80, 0xc0)** 입니다. 각 쌍 구성 요소에 추가 되지만 합계 0xFF에서 범위로 제한 됩니다. 결과는 색 **(0xc0, 0xff, 0xc0)** 입니다. 교차 부분에서 표시 된 색입니다.

`Modulate` 모드의 경우 RGB 값을 부동 소수점으로 변환 해야 합니다. 대상 색은 **(0.75, 0.5, 0)** 이 고 소스는 **(0, 0.5, 0.75)** 입니다. RGB 구성 요소는 각각 서로 곱하고 그 결과는 **(0, 0.25, 0)** 입니다. 이는이 모드에 대 한 **Porter-Duff 그리드** 페이지의 교차점에 표시 되는 색입니다.

**Porter-Duff 투명성** 페이지에서 Porter-duff blend 모드가 부분적으로 투명 한 그래픽 개체에서 작동 하는 방식을 검사할 수 있습니다. XAML 파일에는 Porter-Duff 모드를 사용 하는 `Picker` 포함 되어 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaviews="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.PorterDuffTransparencyPage"
             Title="Porter-Duff Transparency">

    <StackLayout>
        <skiaviews:SKCanvasView x:Name="canvasView"
                                VerticalOptions="FillAndExpand"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="blendModePicker"
                Title="Blend Mode"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKBlendMode}">
                    <x:Static Member="skia:SKBlendMode.Clear" />
                    <x:Static Member="skia:SKBlendMode.Src" />
                    <x:Static Member="skia:SKBlendMode.Dst" />
                    <x:Static Member="skia:SKBlendMode.SrcOver" />
                    <x:Static Member="skia:SKBlendMode.DstOver" />
                    <x:Static Member="skia:SKBlendMode.SrcIn" />
                    <x:Static Member="skia:SKBlendMode.DstIn" />
                    <x:Static Member="skia:SKBlendMode.SrcOut" />
                    <x:Static Member="skia:SKBlendMode.DstOut" />
                    <x:Static Member="skia:SKBlendMode.SrcATop" />
                    <x:Static Member="skia:SKBlendMode.DstATop" />
                    <x:Static Member="skia:SKBlendMode.Xor" />
                    <x:Static Member="skia:SKBlendMode.Plus" />
                    <x:Static Member="skia:SKBlendMode.Modulate" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                3
            </Picker.SelectedIndex>
        </Picker>
    </StackLayout>
</ContentPage>
```

코드 숨김 파일 선형 그라데이션을 사용 하 여 동일한 크기의 사각형 두 개를 채웁니다. 대상 그라데이션 왼쪽 아래 오른쪽 위에서 것입니다. 오른쪽 위 모서리에서 갈색 하지만 다음 중심 쪽으로 투명으로 페이드 시작 되며 왼쪽 아래 모서리에서 이루어집니다. 

소스 사각형 왼쪽 위에서 오른쪽 아래로 그라데이션을 있습니다. 왼쪽 위 모퉁이 파랑 추가 하지만 다시 투명으로 흐려 지 이며 오른쪽 아래 모서리에 투명 합니다. 

```csharp
public partial class PorterDuffTransparencyPage : ContentPage
{
    public PorterDuffTransparencyPage()
    {
        InitializeComponent();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Make square display rectangle smaller than canvas
        float size = 0.9f * Math.Min(info.Width, info.Height);
        float x = (info.Width - size) / 2;
        float y = (info.Height - size) / 2;
        SKRect rect = new SKRect(x, y, x + size, y + size);

        using (SKPaint paint = new SKPaint())
        {
            // Draw destination
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(rect.Right, rect.Top),
                                new SKPoint(rect.Left, rect.Bottom),
                                new SKColor[] { new SKColor(0xC0, 0x80, 0x00),
                                                new SKColor(0xC0, 0x80, 0x00, 0) },
                                new float[] { 0.4f, 0.6f },
                                SKShaderTileMode.Clamp);

            canvas.DrawRect(rect, paint);

            // Draw source
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(rect.Left, rect.Top),
                                new SKPoint(rect.Right, rect.Bottom),
                                new SKColor[] { new SKColor(0x00, 0x80, 0xC0), 
                                                new SKColor(0x00, 0x80, 0xC0, 0) },
                                new float[] { 0.4f, 0.6f },
                                SKShaderTileMode.Clamp);

            // Get the blend mode from the picker
            paint.BlendMode = blendModePicker.SelectedIndex == -1 ? 0 :
                                    (SKBlendMode)blendModePicker.SelectedItem;

            canvas.DrawRect(rect, paint);

            // Stroke surrounding rectangle
            paint.Shader = null;
            paint.BlendMode = SKBlendMode.SrcOver;
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Black;
            paint.StrokeWidth = 3;
            canvas.DrawRect(rect, paint);
        }
    }
}
```

이 프로그램 이외의 비트맵 그래픽 개체를 사용 하 여 Porter 임신 blend 모드를 사용할 수 있도록 하는 방법을 보여 줍니다. 그러나 소스는 투명 한 영역을 포함 해야 합니다. 여기서 그라데이션의 사각형을 채우는 하지만 그라데이션의 부분은 투명 때문입니다.

세 가지 예는 다음과 같습니다.

[![Porter-Duff 투명성](porter-duff-images/PorterDuffTransparency.png "Porter-Duff 투명성")](porter-duff-images/PorterDuffTransparency-Large.png#lightbox)

대상 및 원본의 구성은 원래의 Porter-Duff [_합성 디지털 이미지_](https://graphics.pixar.com/library/Compositing/paper.pdf) 용지의 255 페이지에 표시 된 다이어그램과 매우 유사 하지만,이 페이지에서는 부분 투명도 영역에 대해 blend 모드가 제대로 작동 하 고 있음을 보여 줍니다.

몇 가지 다양 한 효과 대 한 투명 한 그라데이션에 사용할 수 있습니다. 한 가지 가능성은 마스킹은 **SkiaSharp 원형 그라데이션 페이지**의 [**마스킹에 대 한 방사형 그라데이션**](../shaders/circular-gradients.md#radial-gradients-for-masking) 섹션에 표시 된 기술과 비슷합니다. **합성 마스크** 페이지의 대부분은 이전 프로그램과 유사 합니다. 비트맵 리소스를 로드 하 고 표시 하는 사각형을 결정 합니다. 방사형 그라데이션 미리 결정된 된 중심 및 반지름에 따라 만들어집니다.

```csharp
public class CompositingMaskPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
        typeof(CompositingMaskPage),
        "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

    static readonly SKPoint CENTER = new SKPoint(180, 300);
    static readonly float RADIUS = 120;

    public CompositingMaskPage ()
    {
        Title = "Compositing Mask";

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

        // Find rectangle to display bitmap
        float scale = Math.Min((float)info.Width / bitmap.Width,
                               (float)info.Height / bitmap.Height);

        SKRect rect = SKRect.Create(scale * bitmap.Width, scale * bitmap.Height);

        float x = (info.Width - rect.Width) / 2;
        float y = (info.Height - rect.Height) / 2;
        rect.Offset(x, y);

        // Display bitmap in rectangle
        canvas.DrawBitmap(bitmap, rect);

        // Adjust center and radius for scaled and offset bitmap
        SKPoint center = new SKPoint(scale * CENTER.X + x,
                                        scale * CENTER.Y + y);
        float radius = scale * RADIUS;

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateRadialGradient(
                                center,
                                radius,
                                new SKColor[] { SKColors.Black,
                                                SKColors.Transparent },
                                new float[] { 0.6f, 1 },
                                SKShaderTileMode.Clamp);

            paint.BlendMode = SKBlendMode.DstIn;

            // Display rectangle using that gradient and blend mode
            canvas.DrawRect(rect, paint);
        }

        canvas.DrawColor(SKColors.Pink, SKBlendMode.DstOver);
    }
}
```

이 프로그램을 사용 하 여 차이가 그라데이션의 가운데에 검은색을 사용 하 여 시작 하 고 투명도 사용 하 여 종료 합니다. 이는 투명 모드의 `DstIn`사용 하 여 비트맵에 표시 되며,이는 투명 하지 않은 원본 영역 에서만 대상을 보여 줍니다.

`DrawRect` 호출한 후에는 방사형 그라데이션으로 정의 된 원을 제외 하 고 캔버스의 전체 표면이 투명 합니다. 마지막 호출 됩니다.

```csharp
canvas.DrawColor(SKColors.Pink, SKBlendMode.DstOver);
```

캔버스의 모든 투명 한 영역에 분홍색이 지정 됩니다.

[![합성 마스크](porter-duff-images/CompositingMask.png "합성 마스크")](porter-duff-images/CompositingMask-Large.png#lightbox)

또한 한 이미지의 전환에 대 한 Porter 임신 모드 및 부분적으로 투명 한 그라데이션을 사용할 수 있습니다. **그라데이션 전환** 페이지에는 0에서 1로 전환 시 진행률 수준을 나타내는 `Slider`와 원하는 전환 유형을 선택 하는 `Picker` 포함 되어 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.GradientTransitionsPage"
             Title="Gradient Transitions">
    
    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="progressSlider"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference progressSlider},
                              Path=Value,
                              StringFormat='Progress = {0:F2}'}"
               HorizontalTextAlignment="Center" />

        <Picker x:Name="transitionPicker" 
                Title="Transition" 
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged" />
        
    </StackLayout>
</ContentPage>
```

코드 숨김 파일 전환을 보여 주기 위해 두 비트맵 리소스를 로드 합니다. 이러한 이미지는이 문서 앞부분의 **비트맵 흩어 뿌리기** 페이지에서 사용 되는 것과 동일한 두 이미지입니다. 또한이 코드는 선형, 방사형 및 스윕의 3 가지 &mdash; 그라데이션 유형에 해당 하는 세 개의 멤버를 포함 하는 열거형을 정의 합니다. 이러한 값은 `Picker`로드 됩니다.

```csharp
public partial class GradientTransitionsPage : ContentPage
{
    SKBitmap bitmap1 = BitmapExtensions.LoadBitmapResource(
        typeof(GradientTransitionsPage),
        "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    SKBitmap bitmap2 = BitmapExtensions.LoadBitmapResource(
        typeof(GradientTransitionsPage),
        "SkiaSharpFormsDemos.Media.FacePalm.jpg");

    enum TransitionMode
    {
        Linear,
        Radial,
        Sweep
    };

    public GradientTransitionsPage ()
    {
        InitializeComponent ();

        foreach (TransitionMode mode in Enum.GetValues(typeof(TransitionMode)))
        {
            transitionPicker.Items.Add(mode.ToString());
        }

        transitionPicker.SelectedIndex = 0;
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }
    ···
}
```

코드 숨김이 생성 된 파일은 3 개의 `SKPaint` 개체를 만듭니다. `paint0` 개체는 blend 모드를 사용 하지 않습니다. 이 그리기 개체는 `colors` 배열에 표시 된 대로 검정에서 투명으로 이동 하는 그라데이션을 사용 하 여 사각형을 그리는 데 사용 됩니다. `positions` 배열은 `Slider`의 위치를 기반으로 하지만 약간 조정 됩니다. `Slider` 최소값이 나 최대값 이면 `progress` 값은 0 또는 1이 고 두 비트맵 중 하나가 완전히 표시 되어야 합니다. 이러한 값에 대해 `positions` 배열을 적절 하 게 설정 해야 합니다.

`progress` 값이 0 이면 `positions` 배열에-0.1 및 0 값이 포함 됩니다. SkiaSharp는 그렇지 않으면 해당 첫 번째 값을 0에만 black 그라데이션의 즉 0과 투명을 조정 합니다. `progress`가 0.5 이면 배열에 0.45 및 0.55 값이 포함 됩니다. 그라데이션의는 0.45, 0에서 검은색을 투명 하 게 전환 되 고 1 0.55에서 완전히 투명 하다. `progress` 1 이면 `positions` 배열은 1과 1.1입니다. 즉, 그라데이션이 0에서 1 사이의 검정 임을 의미 합니다.

`colors` 및 `position` 배열은 모두 그라데이션을 만드는 `SKShader`의 세 가지 메서드에 사용 됩니다. 이러한 셰이더 중 하나만 `Picker` 선택 항목을 기반으로 생성 됩니다. 

```csharp
public partial class GradientTransitionsPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Assume both bitmaps are square for display rectangle
        float size = Math.Min(info.Width, info.Height);
        SKRect rect = SKRect.Create(size, size);
        float x = (info.Width - size) / 2;
        float y = (info.Height - size) / 2;
        rect.Offset(x, y);

        using (SKPaint paint0 = new SKPaint())
        using (SKPaint paint1 = new SKPaint())
        using (SKPaint paint2 = new SKPaint())
        {
            SKColor[] colors = new SKColor[] { SKColors.Black,
                                               SKColors.Transparent };

            float progress = (float)progressSlider.Value;

            float[] positions = new float[]{ 1.1f * progress - 0.1f,
                                             1.1f * progress };

            switch ((TransitionMode)transitionPicker.SelectedIndex)
            {
                case TransitionMode.Linear:
                    paint0.Shader = SKShader.CreateLinearGradient(
                                        new SKPoint(rect.Left, 0),
                                        new SKPoint(rect.Right, 0),
                                        colors,
                                        positions,
                                        SKShaderTileMode.Clamp);
                    break;

                case TransitionMode.Radial:
                    paint0.Shader = SKShader.CreateRadialGradient(
                                        new SKPoint(rect.MidX, rect.MidY),
                                        (float)Math.Sqrt(Math.Pow(rect.Width / 2, 2) +
                                                         Math.Pow(rect.Height / 2, 2)),
                                        colors,
                                        positions,
                                        SKShaderTileMode.Clamp);
                    break;

                case TransitionMode.Sweep:
                    paint0.Shader = SKShader.CreateSweepGradient(
                                        new SKPoint(rect.MidX, rect.MidY),
                                        colors,
                                        positions);
                    break;
            }

            canvas.DrawRect(rect, paint0);

            paint1.BlendMode = SKBlendMode.SrcOut;
            canvas.DrawBitmap(bitmap1, rect, paint1);

            paint2.BlendMode = SKBlendMode.DstOver;
            canvas.DrawBitmap(bitmap2, rect, paint2);
        }
    }
}
```

기울기는 blend 모드 없이 사각형에 표시 됩니다. 이 `DrawRect` 호출한 후 캔버스에는 검정에서 투명으로 향하는 그라데이션이 포함 됩니다. `Slider` 값이 높을수록 검정색의 양이 증가 합니다.

`PaintSurface` 처리기의 마지막 4 개 문에서 두 비트맵이 표시 됩니다. `SrcOut` blend 모드는 첫 번째 비트맵이 배경의 투명 영역에만 표시 됨을 의미 합니다. 두 번째 비트맵의 `DstOver` 모드는 첫 번째 비트맵이 표시 되지 않는 영역에만 두 번째 비트맵이 표시 됨을 의미 합니다.

다음 스크린샷에서 50% 표시 지점에서 각각 세 개의 다른 전환 형식을 보여 줍니다.

[![그라데이션 전환](porter-duff-images/GradientTransitions.png "그라데이션 전환")](porter-duff-images/GradientTransitions-Large.png#lightbox)

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
