---
제목: "Porter-Duff blend 모드" 설명: "Porter-Duff blend 모드를 사용 하 여 원본 및 대상 이미지에 따라 장면을 작성 합니다."
ms. prod: xamarin. 기술: xamarin-skiasharp assetid: 57F172F8-BA03-43EC-A215-ED6B78696BB5 author: davidbritch: dabritch: 08/23/2018:-loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="porter-duff-blend-modes"></a>Porter-Duff blend 모드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Porter-Duff blend 모드의 이름은 Lucasfilm에 대 한 작업을 수행 하는 동안 Thomas Porter 및 Tom Duff로 지정 됩니다. [_디지털 이미지를 합성_](https://graphics.pixar.com/library/Compositing/paper.pdf) 하는 백서는 _컴퓨터 그래픽_의 7 월 1984, 253 페이지에서 259로 게시 되었습니다. 이러한 blend 모드는 합성 장면으로 다양 한 이미지를 어셈블하는 합성에 필수적입니다.

![Porter-Duff 샘플](porter-duff-images/PorterDuffSample.png "Porter-Duff 샘플")

## <a name="porter-duff-concepts"></a>Porter-Duff 개념

Brownish 사각형이 표시 표면의 왼쪽 및 위쪽 2/3를 차지 한다고 가정 합니다.

![Porter-Duff 대상](porter-duff-images/PorterDuffDst.png "Porter-Duff 대상")

이 영역을 _대상_ 또는 때때로 _배경 또는 배경_ 이라고 합니다 _._

대상의 크기와 같은 사각형을 그립니다. 사각형은 오른쪽 및 아래쪽 bluish을 차지 하는 영역을 제외 하 고는 투명 합니다.

![Porter-Duff 원본](porter-duff-images/PorterDuffSrc.png "Porter-Duff 원본")

이를 _원본_ 또는 경우에 따라 _전경_이라고 합니다.

대상에 소스를 표시 하는 경우 다음과 같은 것이 좋습니다.

![Porter-Duff 원본](porter-duff-images/PorterDuffSrcOver.png "Porter-Duff 원본")

원본의 투명 픽셀은 배경을 통해 표시 되는 반면 bluish 소스 픽셀은 배경을 모호 하 게 해줍니다. 이는 일반적인 경우 이며 SkiaSharp에서로 참조 됩니다 `SKBlendMode.SrcOver` . 이 값은 `BlendMode` 개체를 처음 인스턴스화할 때 속성의 기본 설정입니다 `SKPaint` .

그러나 다른 효과를 위해 다른 blend 모드를 지정할 수 있습니다. 을 지정 하는 경우 `SKBlendMode.DstOver` 원본 및 대상이 교차 하는 영역에서 대상이 다음과 같이 표시 됩니다.

![Porter-Duff Destination Over](porter-duff-images/PorterDuffDstOver.png "Porter-Duff Destination Over")

Blend 모드에는 대상 `SKBlendMode.DstIn` 색을 사용 하 여 대상 및 원본이 교차 하는 영역만 표시 됩니다.

![Porter-Duff Destination In](porter-duff-images/PorterDuffDstIn.png "Porter-Duff Destination In")

`SKBlendMode.Xor`(배타적 OR)의 blend 모드에서는 두 영역이 겹치는 위치에 아무 것도 표시 되지 않습니다.

![Porter-Duff 배타 또는](porter-duff-images/PorterDuffXor.png "Porter-Duff 배타 또는")

색이 지정 된 대상 및 소스 사각형은 표시 화면을 대상 및 소스 사각형의 존재에 해당 하는 다양 한 방법으로 색을 지정할 수 있는 4 개의 고유한 영역으로 효과적으로 나눕니다.

![Porter-Duff](porter-duff-images/PorterDuff.png "Porter-Duff")

대상과 소스가 모두 해당 영역에서 투명 하기 때문에 오른쪽 위 및 왼쪽 아래 사각형은 항상 비어 있습니다. 대상 색은 왼쪽 위 영역을 차지 하므로 대상 색으로 영역을 색으로 지정할 수 있습니다. 마찬가지로 소스 색은 오른쪽 아래 영역을 차지 하므로 소스 색으로 영역을 색으로 지정할 수 있습니다. 대상 색 이나 소스 색을 사용 하 여 중간에 있는 대상과 원본의 교차점을 색으로 지정할 수 있습니다.

총 조합 수는 2 (왼쪽 위)가 2 (오른쪽 아래) 시간 3 (가운데) 또는 12입니다. 12 가지 기본 Porter-Duff 합성 모드입니다.

_디지털 이미지 합성_ (256 페이지), Porter 및 duff를 사용 하 여 13 번째 모드를 추가 _합니다 (SKIASHARP_ `SKBlendMode.Plus` 멤버와 w3c _연하게_ 모드와 혼동 _Lighter_ 하지 않음). 이 `Plus` 모드는 더 자세히 설명 하는 프로세스 인 대상 및 원본 색을 추가 합니다.

지 수 ia는 `Modulate` `Plus` 대상 및 소스 색을 곱하는 것을 제외 하 고와 매우 유사한 라는 14 일 모드를 추가 합니다. 추가 Porter-Duff blend 모드로 취급할 수 있습니다.

SkiaSharp에 정의 된 14 Porter-Duff 모드는 다음과 같습니다. 다음 표에서는 위의 다이어그램에서 비어 있지 않은 세 영역 각각의 색을 표시 하는 방법을 보여 줍니다.

| 모드       | 대상 | 교집합 | 원본 |
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
| `Modulate` |             | 제품      |        | 

이러한 혼합 모드는 대칭입니다. 원본 및 대상을 교환할 수 있으며 모든 모드를 계속 사용할 수 있습니다.

모드의 명명 규칙은 몇 가지 간단한 규칙을 따릅니다.

- **Src** 또는 **Dst** 자체는 원본 또는 대상 픽셀만 표시 됨을 의미 합니다.
- **Over** 접미사는 교차에 표시 되는 항목을 나타냅니다. 원본 또는 대상이 다른에 "위로" 그려집니다.
- 접미사 **In** 접미사는 교차 부분만 색으로 지정 됨을 의미 합니다. 출력은 소스 또는 대상의 일부 ("")로만 제한 됩니다.
- **출력** 접미사는 교집합이 색으로 지정 되지 않음을 의미 합니다. 출력은 교차로 "out" 인 원본 또는 대상의 일부일 뿐입니다.
- 가장 **위에** 있는 접미사는 **In** 및 **Out**의 합집합입니다. 여기에는 원본 또는 대상이 다른 영역을 포함 하는 영역이 포함 됩니다.

및 모드의 차이를 확인 합니다 `Plus` `Modulate` . 이러한 모드는 원본 및 대상 픽셀에 대해 다른 유형의 계산을 수행 합니다. 이러한 항목에 대해서는 곧 자세히 설명 합니다.

**Porter-Duff 그리드** 페이지에는 표 형태의 한 화면에 있는 14 개의 모드가 모두 표시 됩니다. 각 모드는 별도의 인스턴스입니다 `SKCanvasView` . 따라서 클래스는 라는에서 파생 됩니다 `SKCanvasView` `PorterDuffCanvasView` . 정적 생성자는 동일한 크기의 두 비트맵을 만듭니다. 하나는 brownish 사각형은 왼쪽 위 영역에, 다른 하나는 bluish 사각형입니다.

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

인스턴스 생성자에 형식의 매개 변수가 `SKBlendMode` 있습니다. 이 매개 변수는 필드에 저장 됩니다. 

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

`OnPaintSurface`재정의는 두 개의 비트맵을 그립니다. 첫 번째는 정상적으로 그려집니다.

```csharp
canvas.DrawBitmap(dstBitmap, rect);
```

두 번째는 `SKPaint` `BlendMode` 속성이 생성자 인수로 설정 된 개체를 사용 하 여 그려집니다.

```csharp
using (SKPaint paint = new SKPaint())
{
    paint.BlendMode = blendMode;
    canvas.DrawBitmap(srcBitmap, rect, paint);
}
```

재정의의 나머지 부분은 `OnPaintSurface` 크기를 나타내기 위해 비트맵 주위에 사각형을 그립니다.

`PorterDuffGridPage`클래스는 `PorterDurffCanvasView` 배열의 각 멤버에 대해 하나씩 14 인스턴스를 만듭니다 `blendModes` . 배열의 멤버 순서는 `SKBlendModes` 비슷한 모드를 서로 인접 하 게 배치 하기 위해 테이블과 약간 다릅니다. 의 14 개 인스턴스는의 `PorterDuffCanvasView` 레이블과 함께 구성 됩니다 `Grid` .

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

결과는 다음과 같습니다.

[![Porter-Duff 그리드](porter-duff-images/PorterDuffGrid.png "Porter-Duff 그리드")](porter-duff-images/PorterDuffGrid-Large.png#lightbox)

Porter-Duff blend 모드가 적절 하 게 작동 하는 데 투명도가 중요 하다는 것을 이해 해야 합니다. 클래스에는 `PorterDuffCanvasView` 메서드에 대 한 총 세 개의 호출이 포함 되어 있습니다 `Canvas.Clear` . 모든 요소는 모든 픽셀을 투명 하 게 설정 하는 매개 변수가 없는 메서드를 사용 합니다.

```csharp
canvas.Clear();
```

이러한 호출을 변경 하 여 픽셀이 불투명 흰색으로 설정 되도록 시도 합니다.

```csharp
canvas.Clear(SKColors.White);
```

이러한 변경 후에는 일부 blend 모드가 작동 하는 것 처럼 보이지만 다른 일부는 그렇지 않습니다. 소스 비트맵의 배경을 흰색으로 설정 하면 `SrcOver` 소스 비트맵에 투명 픽셀이 없어 대상에가 표시 될 수 있으므로 모드가 작동 하지 않습니다. 대상 비트맵 또는 캔버스의 배경을 흰색으로 설정 하는 경우 `DstOver` 대상에 투명 픽셀이 없으므로가 작동 하지 않습니다.

**Porter-Duff 그리드** 페이지의 비트맵을 간단한 호출로 바꾸는 하려는 유혹 있을 수 있습니다 `DrawRect` . 대상 사각형에는 적용 되지만 소스 사각형에는 적용 되지 않습니다. 소스 사각형은 bluish 색 영역 이상 이어야 합니다. 소스 사각형은 대상의 색이 지정 된 영역에 해당 하는 투명 영역을 포함 해야 합니다. 그런 다음에만 이러한 blend 모드가 작동 합니다.

## <a name="using-mattes-with-porter-duff"></a>Porter와 함께 매트 사용-Duff

**Brick 합성** 페이지는 클래식 합성 작업의 예를 보여 줍니다. 제거 해야 하는 배경의 비트맵을 포함 하 여 여러 부분에서 그림을 조합 해야 합니다. 다음은 문제가 있는 배경의 **SeatedMonkey.jpg** 비트맵입니다.

![꽂혀 있는 원숭이](porter-duff-images/SeatedMonkey.jpg "꽂혀 있는 원숭이")

합성을 준비 하는 경우 이미지를 표시 하 고 그렇지 않은 경우 검정색 인 또 다른 비트맵 인 해당 _매트가_ 생성 됩니다. 이 파일은 **SeatedMonkeyMatte.png** 으로 이름이 지정 되며 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플의 **Media** 폴더에 있는 리소스 중 하나입니다.

![꽂혀 있는 원숭이 무광택](porter-duff-images/SeatedMonkeyMatte.png "꽂혀 있는 원숭이 무광택")

Expertly 생성 된 매트가 _아닙니다_ . 최적으로, 매트는 검정색 픽셀의 가장자리 주위에 부분적으로 투명 한 픽셀을 포함 해야 하며,이 매트는 그렇지 않습니다.

**Brick** 구성 페이지에 대 한 XAML 파일은 `SKCanvasView` `Button` 최종 이미지를 작성 하는 프로세스를 통해 사용자를 안내 하는 및를 인스턴스화합니다.

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

코드를 로드 하는 파일은 필요한 두 비트맵을 로드 하 고의 이벤트를 처리 합니다 `Clicked` `Button` . 클릭 마다 `Button` `step` 필드가 증가 하 고에 `Text` 대해 새 속성이 설정 됩니다 `Button` . `step`가 5에 도달 하면가 0으로 다시 설정 됩니다.

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

프로그램이 처음 실행 될 때 다음을 제외 하 고 아무 것도 표시 되지 않습니다 `Button` .

[![Brick-벽 합성 0 단계](porter-duff-images/BrickWallCompositing0.png "Brick-벽 합성 0 단계")](porter-duff-images/BrickWallCompositing0-Large.png#lightbox)

를 `Button` 한 번 누르면 `step` 1 씩 증가 하 고 처리기는 `PaintSurface` 이제 **SeatedMonkey.jpg**를 표시 합니다.

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

`SKPaint`개체가 없으므로 blend 모드가 없습니다. 비트맵이 화면 아래쪽에 표시 됩니다.

[![Brick-벽 합성 1 단계](porter-duff-images/BrickWallCompositing1.png "Brick-벽 합성 1 단계")](porter-duff-images/BrickWallCompositing1-Large.png#lightbox)

를 `Button` 다시 누르고 `step` 2로 증가 합니다. 이는 **SeatedMonkeyMatte.png** 파일을 표시 하는 중요 한 단계입니다.

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

Blend 모드는입니다 `SKBlendMode.DstIn` . 즉, 대상이 원본의 투명 하지 않은 영역에 해당 하는 영역에 유지 됩니다. 원본 비트맵에 해당 하는 대상 사각형의 나머지 부분은 투명 하 게 됩니다.

[![Brick 합성 2 단계](porter-duff-images/BrickWallCompositing2.png "Brick 합성 2 단계")](porter-duff-images/BrickWallCompositing2-Large.png#lightbox)

배경이 제거 되었습니다. 

다음 단계는 원숭이가 앉아 있는 sidewalk 비슷한 사각형을 그리는 것입니다. 이 sidewalk의 모양은 단색 셰이더와 Perlin 노이즈 셰이더의 두 가지 셰이더의 컴퍼지션을 기반으로 합니다.

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

이 sidewalk는 원숭이 뒤에 있어야 하므로 blend 모드는 `DstOver` 입니다. 대상은 배경이 투명 하 게 표시 되는 위치에만 표시 됩니다.

[![Brick 합성 3 단계](porter-duff-images/BrickWallCompositing3.png "Brick 합성 3 단계")](porter-duff-images/BrickWallCompositing3-Large.png#lightbox)

마지막 단계는 brick 벽을 추가 하는 것입니다. 프로그램은 클래스에서 정적 속성으로 사용할 수 있는 벽돌 벽 비트맵 타일을 사용 합니다 `BrickWallTile` `AlgorithmicBrickWallPage` . 번역 변환은 `SKShader.CreateBitmap` 하위 행이 전체 타일이 되도록 타일을 이동 하는 호출에 추가 됩니다.

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

편의를 위해 `DrawRect` 이 셰이더는 전체 캔버스에 대해이 셰이더를 표시 하지만 `DstOver` 모드는 출력을 여전히 투명 한 캔버스 영역으로 제한 합니다.

[![Brick 합성 4 단계](porter-duff-images/BrickWallCompositing4.png "Brick 합성 4 단계")](porter-duff-images/BrickWallCompositing4-Large.png#lightbox)

이 장면을 구성할 수 있는 다른 방법이 있습니다. 백그라운드에서 시작 하 여 포그라운드로 진행 될 수 있습니다. 하지만 blend 모드를 사용 하면 더 많은 유연성이 제공 됩니다. 특히 매트를 사용 하면 구성 된 장면에서 비트맵의 배경을 제외할 수 있습니다.

[경로 및 영역을 사용한 클리핑](../../curves/clipping.md)문서에서 배운 것 처럼 클래스는 `SKCanvas` , 및 메서드에 해당 하는 세 가지 유형의 클리핑을 [`ClipRect`](xref:SkiaSharp.SKCanvas.ClipRect*) 정의 [`ClipPath`](xref:SkiaSharp.SKCanvas.ClipPath*) [`ClipRegion`](xref:SkiaSharp.SKCanvas.ClipRegion*) 합니다. Porter-Duff blend 모드는 비트맵을 포함 하 여 그릴 수 있는 모든 항목에 대 한 이미지를 제한할 수 있도록 하는 다른 유형의 클리핑을 추가 합니다. **벽돌 벽 합성** 에 사용 되는 매트는 기본적으로 클리핑 영역을 정의 합니다.

## <a name="gradient-transparency-and-transitions"></a>그라데이션 투명도 및 전환

이 문서 앞부분에 표시 된 Porter-Duff blend 모드의 예제에는 불투명 픽셀과 투명 픽셀으로 구성 되었지만 부분적으로 투명 한 픽셀으로 구성 된 모든 이미지가 포함 되어 있습니다. 이러한 픽셀에 대해서도 blend 모드 함수를 정의 합니다. 다음 표는 Porter- [**Duarggendmode 참조**](https://skia.org/user/api/SkBlendMode_Reference)에 있는 표기법을 사용 하는-duff blend 모드의 보다 공식적인 정의입니다. (C # **참조** )는 c + + 구문을 사용 합니다.

개념적으로 각 픽셀의 빨강, 녹색, 파랑 및 알파 구성 요소는 바이트에서 0에서 1 사이의 부동 소수점 숫자로 변환 됩니다. 알파 채널의 경우 0은 완전히 투명 하 고 1은 완전히 불투명 합니다.

아래 표의 표기법은 다음 약어를 사용 합니다.

- **Da** 는 대상 알파 채널입니다.
- **Dc** 는 대상 RGB 색입니다.
- **Sa** 는 원본 알파 채널입니다.
- **Sc** 는 원본 RGB 색입니다.

RGB 색에는 알파 값이 미리 곱해집니다. 예를 들어 **Sc** 가 순수한 red를 나타내지만 **Sa** 가 0x80 인 경우 RGB 색은 **(0x80, 0, 0)** 입니다. **Sa** 가 0 인 경우 모든 RGB 구성 요소도 0입니다.

결과는 대괄호 안에 알파 채널과 함께 표시 되 고 RGB 색은 쉼표 ( **[alpha, color])** 로 구분 됩니다. 색의 경우 빨간색, 녹색 및 파랑 구성 요소에 대해 별도로 계산이 수행 됩니다.

| 모드       | 연산 |
| ---------- | --------- |
| `Clear`    | [0, 0]    |
| `Src`      | [Sa, Sc]  |
| `Dst`      | [Da, Dc]  |
| `SrcOver`  | [Sa + Da · (1-Sa), Sc + Dc · (1-Sa) | 
| `DstOver`  | [Da + Sa · (1 – Da), Dc + Sc · (1 – Da) |
| `SrcIn`    | Sa Da, Sc · Da |
| `DstIn`    | Da Sa, Dc · Sa |
| `SrcOut`   | Sa (1 – Da), Sc · (1 – Da)] |
| `DstOut`   | Da (1-Sa), Dc · (1 – Sa)] |
| `SrcATop`  | [Da, Sc · Da + Dc · (1 – Sa)] |
| `DstATop`  | [Sa, Dc · Sa + Sc · (1 – Da)] |
| `Xor`      | [Sa + Da – 2 · Sa Da, Sc · (1-Da) + Dc · (1 – Sa)] |
| `Plus`     | [Sa + Da, Sc + Dc] |
| `Modulate` | Sa Da, Sc · Dc | 

이러한 작업은 **Da** 와 **Sa** 가 0 또는 1 일 때 분석 하기가 더 쉽습니다. 예를 들어 기본 모드의 `SrcOver` 경우 **Sa** 가 0 이면 **Sc** 도 0이 고 결과는 **[Da, Dc]**, 대상 알파 및 색입니다. **Sa** 가 1 이면 결과는 **[Sa, sc]**, 원본 알파 및 색 또는 **[1, sc]** 입니다.

`Plus`및 `Modulate` 모드는 소스와 대상의 조합으로 인해 새 색이 생성 될 수 있다는 것과는 약간 다릅니다. `Plus`모드는 바이트 구성 요소나 부동 소수점 구성 요소를 사용 하 여 해석할 수 있습니다. 앞에 표시 된 **Porter-Duff 그리드** 페이지에서 대상 색은 **(0xc0, 0x80, 0x00)** 이 고 원본 색은 **(0X00, 0x80, 0xc0)** 입니다. 각 구성 요소 쌍이 추가 되지만 합계는 0xFF로 고정 됩니다. 결과는 색 **(0xc0, 0xff, 0xc0)** 입니다. 교차에 표시 되는 색입니다.

모드의 경우 `Modulate` RGB 값을 부동 소수점으로 변환 해야 합니다. 대상 색은 **(0.75, 0.5, 0)** 이 고 소스는 **(0, 0.5, 0.75)** 입니다. RGB 구성 요소는 각각 서로 곱하고 그 결과는 **(0, 0.25, 0)** 입니다. 이는이 모드에 대 한 **Porter-Duff 그리드** 페이지의 교차점에 표시 되는 색입니다.

**Porter-Duff 투명성** 페이지에서 Porter-duff blend 모드가 부분적으로 투명 한 그래픽 개체에서 작동 하는 방식을 검사할 수 있습니다. XAML 파일에는 `Picker` Porter-Duff 모드를 포함 하는가 포함 되어 있습니다.

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

코드를 사용 하는 파일은 선형 그라데이션을 사용 하 여 동일한 크기의 두 사각형을 채웁니다. 대상 그라데이션은 오른쪽 위에서 왼쪽 아래에 있습니다. 오른쪽 위 모서리에 brownish 가운데를 향해 투명 하 게 시작 되 고 왼쪽 아래 모퉁이에 투명 하 게 됩니다. 

소스 사각형의 왼쪽 위에서 오른쪽 아래로 향하는 그라데이션이 있습니다. 왼쪽 위 모퉁이는 bluish이 고, 다시 투명 하 게 페이드 되 고, 오른쪽 아래 모서리에 투명 하 게 됩니다. 

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

이 프로그램은 비트맵 이외의 그래픽 개체에 Porter-Duff blend 모드를 사용할 수 있음을 보여 줍니다. 그러나 소스는 투명 영역을 포함 해야 합니다. 이 경우에는 그라데이션이 사각형을 채우기만 하 고 그라데이션의 일부가 투명 하기 때문입니다.

다음은 세 가지 예입니다.

[![Porter-Duff 투명성](porter-duff-images/PorterDuffTransparency.png "Porter-Duff 투명성")](porter-duff-images/PorterDuffTransparency-Large.png#lightbox)

대상 및 원본의 구성은 원래의 Porter-Duff [_합성 디지털 이미지_](https://graphics.pixar.com/library/Compositing/paper.pdf) 용지의 255 페이지에 표시 된 다이어그램과 매우 유사 하지만,이 페이지에서는 부분 투명도 영역에 대해 blend 모드가 제대로 작동 하 고 있음을 보여 줍니다.

다른 효과에는 투명 그라데이션을 사용할 수 있습니다. 한 가지 가능성은 마스킹은 **SkiaSharp 원형 그라데이션 페이지**의 [**마스킹에 대 한 방사형 그라데이션**](../shaders/circular-gradients.md#radial-gradients-for-masking) 섹션에 표시 된 기술과 비슷합니다. **합성 마스크** 페이지의 대부분은 이전 프로그램과 유사 합니다. 비트맵 리소스를 로드 하 고이를 표시 하는 사각형을 결정 합니다. 방사형 그라데이션은 미리 결정 된 중심 및 반지름을 기반으로 생성 됩니다.

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

이 프로그램의 차이점은 그라데이션이 가운데에서 검은색으로 시작 하 고 투명도로 끝나는 것입니다. 이는 투명 모드를 사용 하 여 비트맵에 표시 되며,이는 `DstIn` 투명 하지 않은 원본 영역에 있는 대상만 표시 합니다.

호출 후 `DrawRect` 에는 방사형 그라데이션으로 정의 된 원을 제외 하 고 캔버스의 전체 표면이 투명 합니다. 최종 호출을 수행 합니다.

```csharp
canvas.DrawColor(SKColors.Pink, SKBlendMode.DstOver);
```

캔버스의 모든 투명 영역은 분홍색으로 색이 지정 됩니다.

[![합성 마스크](porter-duff-images/CompositingMask.png "합성 마스크")](porter-duff-images/CompositingMask-Large.png#lightbox)

한 이미지에서 다른 이미지로 전환 하는 데 Porter-Duff 모드와 부분적으로 투명 한 그라데이션을 사용할 수도 있습니다. **그라데이션 전환** 페이지에는 `Slider` 0에서 1로 전환 시 진행률 수준을 나타내는가 포함 되며, `Picker` 원하는 전환 유형을 선택 하는가 포함 됩니다.

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

코드를 보호 하는 파일은 두 개의 비트맵 리소스를 로드 하 여 전환을 보여 줍니다. 이러한 이미지는이 문서 앞부분의 **비트맵 흩어 뿌리기** 페이지에서 사용 되는 것과 동일한 두 이미지입니다. 또한이 코드는 세 가지 유형의 그라데이션 &mdash; 선형, 방사형 및 스윕에 해당 하는 세 개의 멤버를 포함 하는 열거형을 정의 합니다. 이러한 값은에 로드 됩니다 `Picker` .

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

코드 숨김이 생성 된 파일은 3 개의 개체를 만듭니다 `SKPaint` . `paint0`개체에서 blend 모드를 사용 하지 않습니다. 이 그리기 개체는 배열에 표시 된 대로 검정에서 투명으로 이동 하는 그라데이션을 사용 하 여 사각형을 그리는 데 사용 됩니다 `colors` . `positions`배열은의 위치를 기반으로 `Slider` 하지만 약간 조정 됩니다. `Slider`가 최소값이 나 최대값에 있으면 `progress` 값은 0 또는 1이 고, 두 비트맵 중 하나가 완전히 표시 되어야 합니다. `positions`이러한 값에 대해 배열을 적절 하 게 설정 해야 합니다.

`progress`값이 0 이면 `positions` 배열에-0.1 및 0 값이 포함 됩니다. SkiaSharp은 첫 번째 값이 0이 되도록 조정 합니다. 즉, 그라데이션이 0 에서만 검정 임을 의미 하 고 그렇지 않으면 투명 합니다. `progress`가 0.5 이면 배열에 0.45 및 0.55 값이 포함 됩니다. 그라데이션은 0에서 0.45 사이의 검정색 이며 투명 하 게 전환 되며 0.55에서 1로 완전히 투명 합니다. `progress`가 1 이면 `positions` 배열은 1과 1.1입니다. 즉, 그라데이션이 0에서 1 사이의 검정 임을 의미 합니다.

`colors`및 `position` 배열은 모두 그라데이션을 만드는 세 가지 메서드에 사용 됩니다 `SKShader` . 이러한 셰이더 중 하나만 선택 항목을 기반으로 생성 됩니다 `Picker` . 

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

이 그라데이션은 blend 모드 없이 사각형에 표시 됩니다. 이 호출 후에는 `DrawRect` 캔버스에 검은색에서 투명으로의 그라데이션이 포함 됩니다. 값이 높을수록 검정색의 양이 증가 합니다 `Slider` .

처리기의 마지막 4 개 문에서 `PaintSurface` 두 비트맵이 표시 됩니다. `SrcOut`Blend 모드는 첫 번째 비트맵이 배경의 투명 영역에만 표시 됨을 의미 합니다. `DstOver`두 번째 비트맵의 모드는 첫 번째 비트맵이 표시 되지 않는 영역에만 두 번째 비트맵이 표시 됨을 의미 합니다.

다음 스크린샷에는 각각 50% 표시의 세 가지 변환 형식이 나와 있습니다.

[![그라데이션 전환](porter-duff-images/GradientTransitions.png "그라데이션 전환")](porter-duff-images/GradientTransitions-Large.png#lightbox)

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
