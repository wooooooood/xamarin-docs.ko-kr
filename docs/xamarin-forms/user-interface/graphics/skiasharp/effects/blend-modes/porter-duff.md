---
title: Porter 임신 blend 모드
description: 원본 및 대상 이미지를 기반으로 하는 백그라운드에서 작성 하는 Porter 임신 blend 모드를 사용 합니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 57F172F8-BA03-43EC-A215-ED6B78696BB5
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: ebd4db28b2c20bd2b9e1d93e03dd101ebc5da663
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50132064"
---
# <a name="porter-duff-blend-modes"></a>Porter 임신 blend 모드

Thomas Porter Tom 임신 Lucasfilm 근무 하면서 합성는 대 수를 개발한 후 Porter 임신 blend 모드 라고 합니다. 백서 [ _합치기 디지털 이미지_ ](https://graphics.pixar.com/library/Compositing/paper.pdf) 1984 년 7 월호에 게시 된 _컴퓨터 그래픽_, 253을 259 페이지입니다. 이러한 혼합 모드는 합치기를 복합 장면에 다양 한 이미지를 구축 하는 데 필요한:

![샘플 Porter 임신](porter-duff-images/PorterDuffSample.png "Porter 임신 샘플")

## <a name="porter-duff-concepts"></a>Porter 임신 개념

갈색 사각형의 왼쪽 및 위쪽의 2/3 화면에 표시를 차지 한다고 가정 합니다.

![대상 Porter 임신](porter-duff-images/PorterDuffDst.png "Porter 임신 대상")

이 영역 라고 합니다 _대상_ 또는 경우에 따라 합니다 _백그라운드_ 또는 _밑그림_.

대상의 크기가 같은 다음 영역을 그릴 하려고 합니다. 사각형은 오른쪽 아래 2 / 3 차지 하는 파랑 추가 영역을 제외 하 고 투명 합니다.

![원본 Porter 임신](porter-duff-images/PorterDuffSrc.png "Porter 임신 원본")

이것을 합니다 _원본_ 또는 경우에 따라 합니다 _포그라운드_합니다.

대상에 소스를 표시할 때 예상 대로 다음과 같습니다.

![소스 Porter 임신](porter-duff-images/PorterDuffSrcOver.png "Porter 임신 소스 위에 있음")

원본의 투명 한 픽셀 배경을 파랑 추가 소스 픽셀 백그라운드 모호 하 게 하는 동안를 통해 볼 수 있습니다. 일반적인 경우는 및에서 SkiaSharp로 라고 `SKBlendMode.SrcOver`합니다. 값의 기본 설정은 인지 합니다 `BlendMode` 속성 때는 `SKPaint` 개체가 처음 인스턴스화입니다.

그러나 다른 효과 대 한 다른 blend 모드를 지정 하는 것이 같습니다. 지정 하는 경우 `SKBlendMode.DstOver`, 원본 및 대상의 교차 영역에서 대상 소스 대신 나타납니다.

![통해 대상 Porter 임신](porter-duff-images/PorterDuffDstOver.png "통해 Porter 임신 대상")

`SKBlendMode.DstIn` blend 모드에서는 원본과 대상 교차 하는 대상 색을 사용 하는 영역만 표시 됩니다.

![대상에 Porter 임신](porter-duff-images/PorterDuffDstIn.png "Porter 임신 대상")

혼합 모드 `SKBlendMode.Xor` (배타적 OR) 하면 두 가지 영역 겹치는 표시할 아무 작업도 수행 합니다.

![Porter 임신 전용 또는](porter-duff-images/PorterDuffXor.png "Porter 임신 전용 또는")

색이 지정 된 대상 및 소스 사각형 대상 및 소스 사각형의 현재 상태에 해당 하는 다양 한 방법으로 색칠 할 수 있는 4 개의 고유 영역으로 화면을 효과적으로 나눕니다.

![Porter 임신](porter-duff-images/PorterDuff.png "Porter 임신")

왼쪽 및 오른쪽 위 사각형은 해당 영역에 대상 및 원본 투명 한 있기 때문에 항상 비어 있습니다. 대상 색 영역 대상 색과 나타나거나 전혀 나타나지 색칠 할 하거나 수 있도록 왼쪽 영역을 차지 합니다. 마찬가지로, 소스 색 또는 전혀 소스 색으로 영역을 색칠 할 수 있도록 오른쪽 아래 영역을 차지 합니다. 대상 색, 소스 색 또는 전혀 사용 하지 않을 중간에서 원본과 대상의 교집합을 색칠 할 수 있습니다.

조합 총 수는 (센터용), 3 또는 12 시간 (오른쪽 아래)에 대 한 2 시간 (왼쪽 위)에 2입니다. 이들은 12 기본 Porter 임신 혼합 모드입니다.

끝부분 _합치기 디지털 이미지_ (256 페이지) Porter 임신 라는 13 모드를 추가 _plus_ (해당 하는 SkiaSharp `SKBlendMode.Plus` 멤버 및 W3C _더 밝게_  모드 (W3C와 혼동 하지입니다 _밝게_ 모드입니다.) 이 `Plus` 모드 추가 된 대상 및 원본 색상으로 프로세스를 곧 자세히 설명 합니다.

Skia 추가 호출을 14 모드 `Modulate` 매우 비슷한 `Plus` 제외 하 고 대상 및 소스 색을 곱합니다. 추가 Porter 임신 blend 모드로 처리할 수 있습니다.

SkiaSharp에 정의 된 대로 14 Porter 임신 모드는 다음과 같습니다. 테이블 위의 다이어그램에는 세 가지 비어 있지 않은 영역이의 각 색은 방법을 보여 줍니다.

| 모드       | 대상 | 교집합 | 소스 |
| ---------- |:-----------:|:------------:|:------:|
| `Clear`    |             |              |        |
| `Src`      |             | 소스       | X      |
| `Dst`      | X           | 대상  |        |
| `SrcOver`  | X           | 소스       | X      |
| `DstOver`  | X           | 대상  | X      |
| `SrcIn`    |             | 소스       |        |
| `DstIn`    |             | 대상  |        |
| `SrcOut`   |             |              | X      |
| `DstOut`   | X           |              |        |
| `SrcATop`  | X           | 소스       |        |
| `DstATop`  |             | 대상  | X      |
| `Xor`      | X           |              | X      |
| `Plus`     | X           | Sum          | X      |
| `Modulate` |             | 제품      |        | 

이러한 혼합 모드가 대칭으로 나타납니다. 원본 및 대상 교환할 수 있으며 모든 모드는 계속 사용할 수 있습니다.

모드의 명명 규칙에는 몇 가지 간단한 규칙을 따릅니다.

- **Src** 나 **Dst** 자체로 원본 또는 대상 픽셀만 표시 되는 것을 의미 합니다.
- 합니다 **위에** 접미사의 교차 부분에서 표시 되는 사항을 나타냅니다. 원본 또는 대상 "끝" 다른 그려집니다.
- 합니다 **에서** 접미사 의미 교차 부분만 색입니다. 출력 소스 또는 대상 "에 있는" 다른 부분만으로 제한 됩니다.
- 합니다 **Out** 접미사 의미 교집합 색상이 지정 되지 않습니다. 출력은 부분에만 소스 또는 대상 "out" 교집합입니다.
- **위에** 접미사는 합한 **에서** 하 고 **아웃**. 다른 원본 또는 대상 "위에"는 영역을 포함 합니다.

사용 하 여 차이점을 확인 합니다 `Plus` 및 `Modulate` 모드입니다. 이러한 모드는 원본 및 대상 픽셀에서 다른 유형의 계산을 수행 합니다. 곧 자세히 설명 되어는 있습니다.

합니다 **Porter 임신 표** 페이지 표 형태로 한 화면에 모든 14 모드를 보여 줍니다. 각 모드는 별도의 인스턴스가 `SKCanvasView`합니다. 따라서 클래스에서 파생 됩니다 `SKCanvasView` 라는 `PorterDuffCanvasView`합니다. 정적 생성자의 왼쪽 위 영역에 갈색 사각형이 하나 및 파랑 추가 사각형을 사용 하 여 동일한 크기의 두 비트맵을 만듭니다.

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

인스턴스 생성자에는 매개 변수 형식 `SKBlendMode`합니다. 이 매개 변수를 필드에 저장합니다. 

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

`OnPaintSurface` 재정의 두 비트맵을 그립니다. 첫 번째 일반적으로 그려집니다.

```csharp
canvas.DrawBitmap(dstBitmap, rect);
```

두 번째 그릴를 `SKPaint` 개체 위치를 `BlendMode` 생성자 인수에 속성이 설정 되어:

```csharp
using (SKPaint paint = new SKPaint())
{
    paint.BlendMode = blendMode;
    canvas.DrawBitmap(srcBitmap, rect, paint);
}
```

나머지는 `OnPaintSurface` 재정의를 해당 크기를 나타내는 비트맵 주위에 사각형을 그립니다.

`PorterDuffGridPage` 클래스의 14 인스턴스를 만듭니다 `PorterDurffCanvasView`, 각 멤버에 대 한 하나는 `blendModes` 배열입니다. 순서는 `SKBlendModes` 서로 인접 한 비슷한 모드를 배치 하기 위해 배열의 멤버는 테이블 보다 약간 다릅니다. 14 인스턴스의 `PorterDuffCanvasView` 의 레이블과 함께 구성 되는 `Grid`:

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

결과 다음과 같습니다.

[![Porter 임신 그리드](porter-duff-images/PorterDuffGrid.png "Porter 임신 표")](porter-duff-images/PorterDuffGrid-Large.png#lightbox)

투명도 Porter 임신 blend 모드를 적절 하 게 작동 하는 데 중요 임을 직접 확인 해야 합니다. 합니다 `PorterDuffCanvasView` 를 세 번 호출의 합계를 포함 하는 클래스는 `Canvas.Clear` 메서드. 모든 픽셀을 모두 투명 하 게 설정 하는 매개 변수가 없는 메서드를 사용 합니다.

```csharp
canvas.Clear();
```

픽셀 불투명 흰색으로 설정 되어 있도록 해당 호출을 변경해 보세요.

```csharp
canvas.Clear(SKColors.White);
```

해당 변경 내용을 다음 작동 하는 것 처럼 보이고 일부 blend 모드 있지만 일부는 그렇지 않습니다. 소스 비트맵의 배경색을 흰색으로 설정 하는 경우 해당 `SrcOver` 모드에에서 있기 때문에 투명 한 픽셀 없습니다 소스 비트맵을 통과해 표시 대상 수 있도록 작동 하지 않습니다. 대상 비트맵을 다음 흰색 캔버스의 배경색을 설정 하는 경우 `DstOver` 대상에는 모든 투명 한 픽셀 없기 때문에 작동 하지 않습니다.

비트맵을 바꾸려면 유혹이 있을 수 있습니다는 **Porter 임신 그리드** 페이지에 간단한 `DrawRect` 호출 합니다. 소스 사각형 아니라 대상 사각형에 대 한 작동 합니다. 소스 사각형 이상의 파랑 추가 색 영역 범위가 포함 되어야 합니다. 소스 사각형에는 대상의 색이 지정 된 영역에 해당 하는 투명 한 영역을 포함 해야 합니다. 그런 후에 모드 작업을 조화 이러한 합니다.

## <a name="using-mattes-with-porter-duff"></a>Porter 임신 매트 사용

**Brick-Wall 합성** 페이지 클래식 합치기 작업의 예를 보여 줍니다: 그림 제거 해야 하는 백그라운드를 사용 하 여 비트맵을 포함 하 여 여러 조각에서 조합할 수 해야 합니다. 다음은 **SeatedMonkey.jpg** 문제가 있는 배경 비트맵:

![Monkey 장착](porter-duff-images/SeatedMonkey.jpg "Monkey를 장착 합니다.")

해당으로 합치기를 위한 준비 _매트_ 만들어진 있는 이미지를 표시 하려는 경우에 검은색 및 투명 하 게 다른 비트 멥 인 합니다. 이 파일의 이름은 **SeatedMonkeyMatte.png** 의 리소스 간에 이며 합니다 **미디어** 폴더에는 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 샘플 :

![Monkey 매트 장착](porter-duff-images/SeatedMonkeyMatte.png "Monkey 매트를 장착 합니다.")

이것이 _되지_ 는 전문적 만든된 매트 합니다. 최적으로 매트 부분적으로 투명 한 검정 픽셀의 가장자리 픽셀을 포함 해야 하 고이 매트 하지 않습니다.

에 대 한 XAML 파일을 **Brick-Wall 합치기** 페이지를 인스턴스화하는 `SKCanvasView` 및 `Button` 사용자를 최종 이미지를 작성 하는 과정을 안내 하는:

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

코드 숨김 파일을 로드 해야 하 고 처리 하는 두 비트맵 합니다 `Clicked` 의 이벤트는 `Button`합니다. 에 대 한 모든 `Button` 를 클릭 합니다 `step` 필드는 증가 하며 새 `Text` 속성에 대 한는 `Button`합니다. 때 `step` 5에 도달 하면 0으로 다시 설정 됩니다.

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

프로그램을 처음 실행 하면 아무 것 제외 하 고 표시 된 `Button`:

[![Brick-Wall 혼합 단계 0](porter-duff-images/BrickWallCompositing0.png "Brick-Wall 혼합 단계 0")](porter-duff-images/BrickWallCompositing0-Large.png#lightbox)

키를 눌러 합니다 `Button` 하면 한 번 `step` 1로 증가 하며 `PaintSurface` 처리기는 이제 표시 **SeatedMonkey.jpg**:

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

방법이 없는 `SKPaint` 개체 이므로 blend 모드 없음. 비트맵 화면 맨 아래에 나타납니다.

[![Brick-Wall 합치기 1 단계](porter-duff-images/BrickWallCompositing1.png "Brick-Wall 혼합 단계 1")](porter-duff-images/BrickWallCompositing1-Large.png#lightbox)

키를 눌러 합니다 `Button` 다시 및 `step` 2로 증가 합니다. 이 중요 한 단계를 표시 합니다 **SeatedMonkeyMatte.png** 파일:

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

Blend 모드가 `SKBlendMode.DstIn`, 즉, 대상은 원본의 불투명 영역에 해당 하는 영역에서 유지 됩니다. 원래 비트맵에 해당 하는 대상 사각형의 나머지 부분 투명해 집니다.

[![Brick-Wall 합치기 2 단계](porter-duff-images/BrickWallCompositing2.png "Brick-Wall 혼합 단계 2")](porter-duff-images/BrickWallCompositing2-Large.png#lightbox)

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

이 인도 monkey 뒤 야, blend 모드 이므로 `DstOver`합니다. 대상만 있는 배경은 투명 하 게 표시 됩니다.

[![Brick-Wall 합치기 3 단계](porter-duff-images/BrickWallCompositing3.png "Brick-Wall 혼합 단계 3")](porter-duff-images/BrickWallCompositing3-Large.png#lightbox)

마지막 단계는 brick 벽을 추가 합니다. 정적 속성으로 사용 가능한 brick-wall 비트맵 타일 사용 `BrickWallTile` 에 `AlgorithmicBrickWallPage` 클래스입니다. 좌표에 추가 되는 `SKShader.CreateBitmap` 아래쪽 행에는 전체 타일 있도록 타일을 이동할 호출:

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

편의 위해 합니다 `DrawRect` 호출 전체 캔버스를 통해이 셰이더를 표시 하지만 `DstOver` 모드만 여전히 투명 한 캔버스 영역에는 출력 제한:

[![Brick-Wall 합치기 4 단계](porter-duff-images/BrickWallCompositing4.png "Brick-Wall 혼합 단계 4")](porter-duff-images/BrickWallCompositing4-Large.png#lightbox)

물론이 장면을 작성 하는 다른 방법은 있습니다. 이 백그라운드에서 시작 및 앞으로 진행 되 고 빌드될 수 있습니다. 하지만 혼합 모드를 사용 하 여 좀 더 유연 합니다. 특히 매트를 사용 하면 구성 된 장면에서 제외할 비트맵의 배경색입니다.

문서에서 설명한 대로 [경로 및 지역 클리핑](../../curves/clipping.md)의 `SKCanvas` 클리핑에 해당 하는 클래스를 정의 합니다 [ `ClipRect` ](xref:SkiaSharp.SKCanvas.ClipRect*), [ `ClipPath` ](xref:SkiaSharp.SKCanvas.ClipPath*), 및 [ `ClipRegion` ](xref:SkiaSharp.SKCanvas.ClipRegion*) 메서드. Porter 임신 blend 모드는 다른 형식의 이미지를 그릴 수 있는, 비트맵을 포함 하 여 아무 것도 제한 수 있는 클리핑을 추가 합니다. 에 사용 되는 매트 **Brick-Wall 합치기** 기본적으로 클리핑 영역을 정의 합니다.

## <a name="gradient-transparency-and-transitions"></a>그라데이션 투명도 및 전환

Porter 임신 예가이 문서의 모든 것을 의미 했습니다 불투명 한 픽셀 및 투명 한 픽셀을 부분적으로 투명 한 픽셀의로 구성 된 이미지는 앞에 표시 된 모드를 혼합 됩니다. Blend 모드 함수 픽셀 들도 정의 됩니다. 다음 표는는 Skia 있는 표기법을 사용 하는 좀 더 공식적인 Porter 임신 blend 모드 정의 [ **SkBlendMode 참조**](https://skia.org/user/api/SkBlendMode_Reference)합니다. (때문 **SkBlendMode 참조** Skia 참조는 c + + 구문을 사용 합니다.)

개념적으로 각 픽셀의 빨간색, 녹색, 파랑 및 알파 구성 요소 범위에서 부동 소수점 숫자 0에서 1 바이트에서 변환 됩니다. 알파 채널의 0은 완전히 투명 하 고 1은 완전히 불투명

아래 표에 표기법은 다음 약어를 사용 합니다.

- **Da** 대상의 알파 채널
- **Dc** 대상인 RGB 색
- **Sa** 는 원본 알파 채널
- **Sc** 원본인 RGB 색

RGB 색은 알파 값이 미리 곱해집니다. 예를 들어 경우 **Sc** 순수한 빨간색을 나타내는 있지만 **Sa** 가 0x80, RGB 색 **(0x80, 0, 0)** 합니다. 하는 경우 **Sa** 가 0 이면 모든 RGB 구성 요소는 0입니다.

결과 알파 채널 및 쉼표로 구분 된 RGB 색을 사용 하 여는 대괄호 안에 표시 됩니다. **[알파, 색상]** 합니다. 색에 대 한 계산 빨강, 녹색 및 파랑 구성 요소에 대해 개별적으로 수행 됩니다.

| 모드       | 작업 |
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

이러한 작업은 분석할 때 쉽게 **Da** 하 고 **Sa** 는 0 또는 1입니다. 기본값에 대 한 예를 들어 `SrcOver` 모드 경우 **Sa** 0 이면 **Sc** 가 0이 고, 결과 이기도 **[Da, Dc]**, 대상 알파 및 색입니다. 하는 경우 **Sa** 가 1 이면 결과 **[Sa, Sc]**, 원본 알파 및 색, 또는 **[1, Sc]** 합니다.

합니다 `Plus` 고 `Modulate` 모드는 다른 기존과 약간 다를 새 색의 원본 및 대상 조합에서 발생할 수 있습니다. `Plus` 바이트 구성 요소 또는 부동 소수점 구성 요소를 사용 하 여 모드를 해석할 수 있습니다. 에 **Porter 임신 그리드** 대상 색이 이전에 표시 되는 페이지 **(0xC0, 0x80, 0x00)** 소스 색 이며 **(0x00, 0x80, 0xC0)**. 각 쌍 구성 요소에 추가 되지만 합계 0xFF에서 범위로 제한 됩니다. 결과 색인 **(0xC0, 0xFF 0xC0)** 합니다. 교차 부분에서 표시 된 색입니다.

에 대 한는 `Modulate` 모드 RGB 값에 부동 소수점 변환 해야 합니다. 대상 색이 **(0.75, 0.5, 0)** 소스 이며 **(0, 0.5, 0.75)** 합니다. RGB 구성 요소는 각 함께 곱하고 결과가 **(0, 0.25, 0)** 합니다. 교차 부분에서 표시 된 색상입니다 합니다 **Porter 임신 표** 이 모드에 대 한 페이지.

합니다 **Porter 임신 투명도** 페이지를 사용 하면 Porter 임신 blend 모드 부분적으로 투명 한 그래픽 개체에 대해 작동 하는 방법을 검사할 수 있습니다. XAML 파일에 포함 된 `Picker` Porter 임신 모드를 사용 하 여:

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

[![Porter 임신 투명도](porter-duff-images/PorterDuffTransparency.png "Porter 임신 투명도")](porter-duff-images/PorterDuffTransparency-Large.png#lightbox)

원본과 대상의 구성은 원래 Porter 임신 255 페이지에 표시 된 다이어그램 매우 비슷합니다 [ _합치기 디지털 이미지_ ](https://graphics.pixar.com/library/Compositing/paper.pdf) 것을 보여 줍니다 문서 있지만이 페이지는 혼합 모드는 투명도 부분 영역에 대 한 잘 작동 합니다.

몇 가지 다양 한 효과 대 한 투명 한 그라데이션에 사용할 수 있습니다. 설명한 기술을 비슷합니다는 한 가지 방법은 마스킹하는 [ **마스킹에 대 한 방사형 그라데이션을** ](../shaders/circular-gradients.md#radial-gradients-for-masking) 섹션의 **SkiaSharp 순환 그라데이션 페이지**합니다. 많은 합니다 **합치기 마스크** 페이지는 이전 프로그램와 유사 합니다. 비트맵 리소스를 로드 하 고 표시 하는 사각형을 결정 합니다. 방사형 그라데이션 미리 결정된 된 중심 및 반지름에 따라 만들어집니다.

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

이 프로그램을 사용 하 여 차이가 그라데이션의 가운데에 검은색을 사용 하 여 시작 하 고 투명도 사용 하 여 종료 합니다. blend 모드를 사용 하 여 비트맵에 표시 됩니다 `DstIn`를 투명 하 게 되지 않는 원본 영역에만 대상을 보여 줍니다.

뒤를 `DrawRect` 호출 캔버스의 전체 화면 방사형 그라데이션 정의한 원 제외 하 고 투명 하 게 됩니다. 마지막 호출 됩니다.

```csharp
canvas.DrawColor(SKColors.Pink, SKBlendMode.DstOver);
```

캔버스의 모든 투명 한 영역에 분홍색이 지정 됩니다.

[![합성 마스크](porter-duff-images/CompositingMask.png "합치기 마스크")](porter-duff-images/CompositingMask-Large.png#lightbox)

또한 한 이미지의 전환에 대 한 Porter 임신 모드 및 부분적으로 투명 한 그라데이션을 사용할 수 있습니다. **그라데이션 전환** 페이지에는 `Slider` 0에서 1로의 전환에 진행률 수준을 나타낼 및 `Picker` 원하는 전환의 유형을 선택할 수:

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

코드 숨김 파일 전환을 보여 주기 위해 두 비트맵 리소스를 로드 합니다. 이들은에 사용 된 동일한 두 개의 이미지를 **비트맵 주만에** 이 문서의 앞부분에서 페이지입니다. 또한 코드를 세 가지 유형의 그라데이션에 해당 하는 세 멤버가 포함 된 열거형을 정의 &mdash; 선형 이며, 방사형 및 스윕 합니다. 이러한 값이 로드 되는 `Picker`:

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

코드 숨김 파일을 만드는 세 가지 `SKPaint` 개체입니다. `paint0` 개체 blend 모드를 사용 하지 않습니다. 이 그리기 개체에 표시 된 대로 투명 검정으로 이동 하는 그라데이션 사용 하 여 사각형을 그리는 데 사용 되는 `colors` 배열입니다. 합니다 `positions` 의 위치를 기반으로 하는 배열은 `Slider`, 되지만 어느 정도 조정 합니다. 경우는 `Slider` 해당 최소값 또는 최대값에 `progress` 값은 0 또는 1 및 두 비트맵 중 완벽 하 게 표시 되어야 합니다. `positions` 배열 적절 하 게 설정 해야 해당 값에 대 한 합니다.

경우는 `progress` 값이 0 인 경우 `positions` 배열-0.1 및 0 값을 포함 합니다. SkiaSharp는 그렇지 않으면 해당 첫 번째 값을 0에만 black 그라데이션의 즉 0과 투명을 조정 합니다. 때 `progress` 0.5이 하 이면 배열 0.45 및 0.55 값을 포함 합니다. 그라데이션의는 0.45, 0에서 검은색을 투명 하 게 전환 되 고 1 0.55에서 완전히 투명 하다. 때 `progress` 가 1 이면는 `positions` 배열이 1 및 0에서 1로 black 그라데이션의 1.1입니다.

합니다 `colors` 하 고 `position` 배열 모두의 세 가지 방법에서 사용 되 `SKShader` 그라데이션 만들기는 합니다. 에 따라 만들어집니다 이러한 셰이더 중 전용 된 `Picker` 선택: 

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

기울기는 blend 모드 없이 사각형에 표시 됩니다. 그 후 `DrawRect` 호출을 캔버스에 투명 검정에서 그라데이션 단순히 포함 되어 있습니다. 검정 양이 이상을 사용 하 여 증가 `Slider` 값입니다.

마지막 4 개 문에서 `PaintSurface` 처리기 두 비트맵 표시 됩니다. `SrcOut` blend 모드 첫 번째 비트맵의 배경 투명 영역에만 표시 되는 것을 의미 합니다. `DstOver` 모드 두 번째 비트맵에 대 한 두 번째 비트맵 첫 번째 비트맵에 표시 되지 않습니다 해당 영역에만 표시 되는 것을 의미 합니다.

다음 스크린샷에서 50% 표시 지점에서 각각 세 개의 다른 전환 형식을 보여 줍니다.

[![그라데이션 전환을](porter-duff-images/GradientTransitions.png "그라데이션 전환")](porter-duff-images/GradientTransitions-Large.png#lightbox)

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)