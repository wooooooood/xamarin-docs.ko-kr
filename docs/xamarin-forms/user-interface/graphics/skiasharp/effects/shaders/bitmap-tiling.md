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
ms.openlocfilehash: 6a28dd20eb8978334365ac217df1241e5288fd28
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137426"
---
# <a name="skiasharp-bitmap-tiling"></a>SkiaSharp 비트맵 바둑판식 배열

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/catclock)

위의 두 문서에서 살펴본 것 처럼 [`SKShader`](xref:SkiaSharp.SKShader) 클래스는 선형 또는 원형 그라데이션을 만들 수 있습니다. 이 문서에서는 `SKShader` 비트맵을 사용 하 여 영역을 바둑판식으로 배열 하는 개체에 대해 중점적으로 설명 합니다. 비트맵은 원래 방향이 나 가로 및 세로로 대칭 이동 하 여 가로 및 세로로 반복할 수 있습니다. 대칭 이동은 타일 간 불연속성을 방지 합니다.

![비트맵 바둑판식 배열 샘플](bitmap-tiling-images/BitmapTilingSample.png "비트맵 바둑판식 배열 샘플")

[`SKShader.CreateBitmap`](xref:SkiaSharp.SKShader.CreateBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKShaderTileMode,SkiaSharp.SKShaderTileMode))이 셰이더를 만드는 정적 메서드에는 `SKBitmap` 열거형의 매개 변수와 두 개의 멤버가 있습니다 [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) .

```csharp
public static SKShader CreateBitmap (SKBitmap src, SKShaderTileMode tmx, SKShaderTileMode tmy)
```

두 매개 변수는 가로 바둑판식 배열 및 세로 바둑판식 배열에 사용 되는 모드를 표시 합니다. 이는 `SKShaderTileMode` 그라데이션 메서드와 함께 사용 되는 것과 동일한 열거형입니다.

[`CreateBitmap`](xref:SkiaSharp.SKShader.CreateBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKShaderTileMode,SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix))오버 로드에는 `SKMatrix` 바둑판식 비트맵에서 변환을 수행 하기 위한 인수가 포함 됩니다.

```csharp
public static SKShader CreateBitmap (SKBitmap src, SKShaderTileMode tmx, SKShaderTileMode tmy, SKMatrix localMatrix)
```

이 문서에는 바둑판식 비트맵으로이 행렬 변환을 사용 하는 몇 가지 예가 포함 되어 있습니다.

## <a name="exploring-the-tile-modes"></a>타일 모드 탐색

[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플의 **셰이더 및 기타 효과** 페이지의 **비트맵 바둑판식 배열** 섹션에 있는 첫 번째 프로그램은 두 인수의 효과를 보여 줍니다 `SKShaderTileMode` . **비트맵 타일 대칭 이동 모드** XAML 파일은 `SKCanvasView` 및 두 개의 `Picker` 뷰를 인스턴스화하고 `SKShaderTilerMode` 가로 및 세로 바둑판식 배열에 대 한 값을 선택할 수 있도록 합니다. 멤버의 배열은 `SKShaderTileMode` 섹션에 정의 되어 `Resources` 있습니다.

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.BitmapTileFlipModesPage"
             Title="Bitmap Tile Flip Modes">

    <ContentPage.Resources>
        <x:Array x:Key="tileModes"
                 Type="{x:Type skia:SKShaderTileMode}">
            <x:Static Member="skia:SKShaderTileMode.Clamp" />
            <x:Static Member="skia:SKShaderTileMode.Repeat" />
            <x:Static Member="skia:SKShaderTileMode.Mirror" />
        </x:Array>
    </ContentPage.Resources>

    <StackLayout>
        <skiaforms:SKCanvasView x:Name="canvasView"
                                VerticalOptions="FillAndExpand"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="xModePicker"
                Title="Tile X Mode"
                Margin="10, 0"
                ItemsSource="{StaticResource tileModes}"
                SelectedIndex="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged" />

        <Picker x:Name="yModePicker"
                Title="Tile Y Mode"
                Margin="10, 10"
                ItemsSource="{StaticResource tileModes}"
                SelectedIndex="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged" />

    </StackLayout>
</ContentPage>
```

코드를 표시 하는 파일의 생성자는 비트맵 리소스에 로드 되어 있는 원숭이를 표시 합니다. 먼저의 메서드를 사용 하 여 이미지를 잘라 [`ExtractSubset`](xref:SkiaSharp.SKBitmap.ExtractSubset(SkiaSharp.SKBitmap,SkiaSharp.SKRectI)) `SKBitmap` 헤드와 피트를 비트맵의 가장자리에 접촉 합니다. 그런 다음 생성자는 메서드를 사용 하 여 크기의 절반에 해당 하는 [`Resize`](xref:SkiaSharp.SKBitmap.Resize(SkiaSharp.SKImageInfo,SkiaSharp.SKBitmapResizeMethod)) 다른 비트맵을 만듭니다. 이러한 변경은 비트맵을 바둑판식으로 배열 하는 데 더 적합 합니다.

```csharp
public partial class BitmapTileFlipModesPage : ContentPage
{
    SKBitmap bitmap;

    public BitmapTileFlipModesPage ()
    {
        InitializeComponent ();

        SKBitmap origBitmap = BitmapExtensions.LoadBitmapResource(
            GetType(), "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

        // Define cropping rect
        SKRectI cropRect = new SKRectI(5, 27, 296, 260);

        // Get the cropped bitmap
        SKBitmap croppedBitmap = new SKBitmap(cropRect.Width, cropRect.Height);
        origBitmap.ExtractSubset(croppedBitmap, cropRect);

        // Resize to half the width and height
        SKImageInfo info = new SKImageInfo(cropRect.Width / 2, cropRect.Height / 2);
        bitmap = croppedBitmap.Resize(info, SKBitmapResizeMethod.Box);
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

        // Get tile modes from Pickers
        SKShaderTileMode xTileMode =
            (SKShaderTileMode)(xModePicker.SelectedIndex == -1 ?
                                        0 : xModePicker.SelectedItem);
        SKShaderTileMode yTileMode =
            (SKShaderTileMode)(yModePicker.SelectedIndex == -1 ?
                                        0 : yModePicker.SelectedItem);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateBitmap(bitmap, xTileMode, yTileMode);
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

`PaintSurface`처리기는 `SKShaderTileMode` 두 개의 뷰에서 설정을 가져오고 `Picker` `SKShader` 비트맵 및 이러한 두 값을 기반으로 개체를 만듭니다. 이 셰이더는 캔버스를 채우는 데 사용 됩니다.

[![비트맵 타일 대칭 이동 모드](bitmap-tiling-images/BitmapTileFlipModes.png "비트맵 타일 대칭 이동 모드")](bitmap-tiling-images/BitmapTileFlipModes-Large.png#lightbox)

왼쪽의 iOS 화면에는의 기본값 결과가 표시 `SKShaderTileMode.Clamp` 됩니다. 비트맵은 왼쪽 위 모퉁이에 있습니다. 비트맵 아래에서 픽셀의 아래쪽 행은 아래로 반복 됩니다. 비트맵의 오른쪽에 있는 픽셀의 가장 오른쪽 열은 전체에서 반복 됩니다. 캔버스의 나머지 부분은 비트맵의 오른쪽 아래 모퉁이에 있는 진한 갈색 픽셀을 기준으로 색이 지정 됩니다. `Clamp`옵션은 비트맵 바둑판식 배열에서 거의 사용 되지 않는 것이 분명 합니다.

가운데 Android 화면에는 `SKShaderTileMode.Repeat` 두 인수에 대 한의 결과가 표시 됩니다. 타일이 가로 및 세로 방향으로 반복 됩니다. 유니버설 Windows 플랫폼 화면은를 표시 `SKShaderTileMode.Mirror` 합니다. 타일이 반복 되지만 가로 및 세로로 대칭 이동 됩니다. 이 옵션의 장점은 타일 사이에 불연속성 없다는 것입니다.

가로 및 세로 반복에는 다른 옵션을 사용할 수 있습니다. `SKShaderTileMode.Mirror`을 두 번째 인수로 지정 하 고 `CreateBitmap` `SKShaderTileMode.Repeat` 세 번째 인수로 지정할 수 있습니다. 각 행에서 monkeys은 여전히 일반 이미지와 미러 이미지 사이를 대체 하지만 어떤 monkeys은 중단 되지 않습니다.

## <a name="patterned-backgrounds"></a>패턴화 배경

비트맵 바둑판식 배열은 일반적으로 비교적 작은 비트맵에서 패턴화 된 배경을 만드는 데 사용 됩니다. 클래식 예제는 brick 벽입니다.

**알고리즘 Brick 벽** 페이지는 전체 brick과 소매으로 구분 된 brick의 2 절반과 유사한 작은 비트맵을 만듭니다. 이 brick은 다음 샘플 에서도 사용 되므로 정적 생성자에 의해 생성 되 고 정적 속성을 사용 하 여 공용으로 설정 됩니다.

```csharp
public class AlgorithmicBrickWallPage : ContentPage
{
    static AlgorithmicBrickWallPage()
    {
        const int brickWidth = 64;
        const int brickHeight = 24;
        const int morterThickness = 6;
        const int bitmapWidth = brickWidth + morterThickness;
        const int bitmapHeight = 2 * (brickHeight + morterThickness);

        SKBitmap bitmap = new SKBitmap(bitmapWidth, bitmapHeight);

        using (SKCanvas canvas = new SKCanvas(bitmap))
        using (SKPaint brickPaint = new SKPaint())
        {
            brickPaint.Color = new SKColor(0xB2, 0x22, 0x22);

            canvas.Clear(new SKColor(0xF0, 0xEA, 0xD6));
            canvas.DrawRect(new SKRect(morterThickness / 2,
                                       morterThickness / 2,
                                       morterThickness / 2 + brickWidth,
                                       morterThickness / 2 + brickHeight),
                                       brickPaint);

            int ySecondBrick = 3 * morterThickness / 2 + brickHeight;

            canvas.DrawRect(new SKRect(0,
                                       ySecondBrick,
                                       bitmapWidth / 2 - morterThickness / 2,
                                       ySecondBrick + brickHeight),
                                       brickPaint);

            canvas.DrawRect(new SKRect(bitmapWidth / 2 + morterThickness / 2,
                                       ySecondBrick,
                                       bitmapWidth,
                                       ySecondBrick + brickHeight),
                                       brickPaint);
        }

        // Save as public property for other programs
        BrickWallTile = bitmap;
    }

    public static SKBitmap BrickWallTile { private set; get; }
    ···
}
```

결과 비트맵의 너비는 70 픽셀이 고 60 픽셀은 높습니다.

![알고리즘 Brick 벽 타일](bitmap-tiling-images/AlgorithmicBrickWallTile.png "알고리즘 Brick 벽 타일")

**알고리즘 Brick 벽** 페이지의 나머지 부분에서는 `SKShader` 이 이미지를 가로 및 세로 방향으로 반복 하는 개체를 만듭니다.

```csharp
public class AlgorithmicBrickWallPage : ContentPage
{
    ···
    public AlgorithmicBrickWallPage ()
    {
        Title = "Algorithmic Brick Wall";

        // Create SKCanvasView
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

        using (SKPaint paint = new SKPaint())
        {
            // Create bitmap tiling
            paint.Shader = SKShader.CreateBitmap(BrickWallTile,
                                                 SKShaderTileMode.Repeat,
                                                 SKShaderTileMode.Repeat);
            // Draw background
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

결과:

[![알고리즘 Brick 벽](bitmap-tiling-images/AlgorithmicBrickWall.png "알고리즘 Brick 벽")](bitmap-tiling-images/AlgorithmicBrickWall-Large.png#lightbox)

좀 더 현실적인 것을 선호할 수 있습니다. 이 경우 실제 brick 벽의 사진을 촬영 한 후 자를 수 있습니다. 이 비트맵은 300 픽셀 너비와 150 픽셀의 높이입니다.

![Brick 벽 타일](bitmap-tiling-images/BrickWallTile.jpg "Brick 벽 타일")

이 비트맵은 **사진 벽돌 담 벼 락** 페이지에서 사용 됩니다.

```csharp
public class PhotographicBrickWallPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(PhotographicBrickWallPage),
                        "SkiaSharpFormsDemos.Media.BrickWallTile.jpg");

    public PhotographicBrickWallPage()
    {
        Title = "Photographic Brick Wall";

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

        using (SKPaint paint = new SKPaint())
        {
            // Create bitmap tiling
            paint.Shader = SKShader.CreateBitmap(bitmap,
                                                 SKShaderTileMode.Mirror,
                                                 SKShaderTileMode.Mirror);
            // Draw background
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

`SKShaderTileMode`에 대 한 인수는 `CreateBitmap` 모두 `Mirror` 입니다. 이 옵션은 일반적으로 실제 이미지에서 만든 타일을 사용 하는 경우에 필요 합니다. 타일을 미러링 하면 불연속성 방지 됩니다.

[![사진 벽돌 벽](bitmap-tiling-images/PhotographicBrickWall.png "사진 벽돌 벽")](bitmap-tiling-images/PhotographicBrickWall-Large.png#lightbox)

타일에 대 한 적절 한 비트맵을 얻으려면 몇 가지 작업을 수행 해야 합니다. 이는 더 진한 brick이 너무 많이 작동 하기 때문에 제대로 작동 하지 않습니다. 반복 되는 이미지 내에 정기적으로 나타나므로이 brick 벽이 작은 비트맵에서 생성 되었다는 사실을 표시 합니다.

[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플의 **미디어** 폴더에는 다음과 같은 석재 벽 이미지도 포함 됩니다.

![석재 벽 타일](bitmap-tiling-images/StoneWallTile.jpg "석재 벽 타일")

그러나 원래 비트맵은 타일에 대 한 작은 크기입니다. 크기를 조정할 수 있지만 메서드는 `SKShader.CreateBitmap` 변환을 적용 하 여 타일의 크기를 조정할 수도 있습니다. 이 옵션은 **석재 담 벼 락** 페이지에서 보여 줍니다.

```csharp
public class StoneWallPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(StoneWallPage),
                        "SkiaSharpFormsDemos.Media.StoneWallTile.jpg");

    public StoneWallPage()
    {
        Title = "Stone Wall";

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

        using (SKPaint paint = new SKPaint())
        {
            // Create scale transform
            SKMatrix matrix = SKMatrix.MakeScale(0.5f, 0.5f);

            // Create bitmap tiling
            paint.Shader = SKShader.CreateBitmap(bitmap,
                                                 SKShaderTileMode.Mirror,
                                                 SKShaderTileMode.Mirror,
                                                 matrix);
            // Draw background
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

`SKMatrix`이미지 크기를 원래 크기의 절반으로 조정 하는 값이 생성 됩니다.

[![돌 성벽](bitmap-tiling-images/StoneWall.png "돌 성벽")](bitmap-tiling-images/StoneWall-Large.png#lightbox)

변환은 메서드에서 사용 되는 원래 비트맵에서 작동 `CreateBitmap` 하나요? 또는 타일의 결과 배열을 변환 하나요? 

이 질문에 대답 하는 쉬운 방법은 변환의 일부로 순환을 포함 하는 것입니다.

```csharp
SKMatrix matrix = SKMatrix.MakeScale(0.5f, 0.5f);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeRotationDegrees(15));
```

변환이 개별 타일에 적용 되는 경우 타일의 반복 되는 각 이미지를 회전 해야 하며 결과에는 많은 불연속성 포함 됩니다. 그러나이 스크린샷에서는 타일의 복합 배열이 변환 됨을 알 수 있습니다.

[![돌 벽 회전](bitmap-tiling-images/StoneWallRotated.png "돌 벽 회전")](bitmap-tiling-images/StoneWallRotated-Large.png#lightbox)

[**타일 맞춤**](#tile-alignment)섹션에서 셰이더에 적용 되는 변환 변환의 예를 확인할 수 있습니다.

독립 실행형 [**Cat Clock**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/catclock) 샘플 ( **SkiaSharpFormsDemos**의 일부가 아님)은이 240 픽셀 사각형 비트맵을 기반으로 하는 비트맵 바둑판식 배열을 사용 하 여 목재-그레인 배경을 시뮬레이션 합니다.

![목재 그레인](bitmap-tiling-images/WoodGrain.png "목재 그레인")

목조 바닥의 사진입니다. `SKShaderTileMode.Mirror`옵션을 사용 하면 훨씬 더 큰 목재 영역으로 표시 될 수 있습니다.

[![Cat 클록](bitmap-tiling-images/CatClock.png "Cat 클록")](bitmap-tiling-images/CatClock-Large.png#lightbox)

## <a name="tile-alignment"></a>타일 맞춤

지금까지 보여 주는 모든 예제는에서 만든 셰이더를 사용 `SKShader.CreateBitmap` 하 여 전체 캔버스를 포함 했습니다. 대부분의 경우 작은 영역을 파일링 하기 위해 비트맵 바둑판식 배열을 사용 하거나 굵은 선의 내부를 채우기 위해 (더 드물게) 사용 합니다. 작은 사각형에 사용 되는 사진 벽돌 벽 타일은 다음과 같습니다.

[![타일 맞춤](bitmap-tiling-images/TileAlignment.png "타일 맞춤")](bitmap-tiling-images/TileAlignment-Large.png#lightbox)

이는 사용자에 게 도움이 될 수도 있고 그렇지 않을 수도 있습니다. 바둑판식으로 배열 패턴이 사각형의 왼쪽 위 모퉁이에 있는 전체 brick으로 시작 하지 않을 수 있습니다. 이는 셰이더가 캔버스에 맞추고 장식할 그래픽 개체가 아닌 경우입니다.

해결 방법은 간단 합니다. `SKMatrix`번역 변환을 기반으로 값을 만듭니다. 변환은 바둑판식 패턴을 타일의 왼쪽 위 모퉁이가 맞춰지는 지점으로 효과적으로 이동 합니다. 이 접근 방식은 위에 표시 된 정렬 되지 않은 타일의 이미지를 만든 **타일 맞춤** 페이지에서 보여 줍니다.

```csharp
public class TileAlignmentPage : ContentPage
{
    bool isAligned;

    public TileAlignmentPage()
    {
        Title = "Tile Alignment";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;

        // Add tap handler
        TapGestureRecognizer tap = new TapGestureRecognizer();
        tap.Tapped += (sender, args) =>
        {
            isAligned ^= true;
            canvasView.InvalidateSurface();
        };
        canvasView.GestureRecognizers.Add(tap);

        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            SKRect rect = new SKRect(info.Width / 7,
                                     info.Height / 7,
                                     6 * info.Width / 7,
                                     6 * info.Height / 7);

            // Get bitmap from other program
            SKBitmap bitmap = AlgorithmicBrickWallPage.BrickWallTile;

            // Create bitmap tiling
            if (!isAligned)
            {
                paint.Shader = SKShader.CreateBitmap(bitmap,
                                                     SKShaderTileMode.Repeat,
                                                     SKShaderTileMode.Repeat);
            }
            else
            {
                SKMatrix matrix = SKMatrix.MakeTranslation(rect.Left, rect.Top);

                paint.Shader = SKShader.CreateBitmap(bitmap,
                                                     SKShaderTileMode.Repeat,
                                                     SKShaderTileMode.Repeat,
                                                     matrix);
            }

            // Draw rectangle
            canvas.DrawRect(rect, paint);
        }
    }
}
```

**타일 맞춤** 페이지에는가 포함 됩니다 `TapGestureRecognizer` . 화면을 탭 하거나 클릭 하면 프로그램에서 인수를 사용 하 여 메서드로 전환 합니다 `SKShader.CreateBitmap` `SKMatrix` . 이 변환은 왼쪽 위 모퉁이에 전체 brick이 포함 되도록 패턴을 이동 합니다.

[![타일 맞춤 탭](bitmap-tiling-images/TileAlignmentTapped.png "타일 맞춤 탭")](bitmap-tiling-images/TileAlignmentTapped-Large.png#lightbox)

이 방법을 사용 하 여 바둑판식 비트맵 패턴이 그리는 영역 내에서 가운데에 오도록 할 수도 있습니다. **가운데 맞춤 타일** 페이지에서 처리기는 `PaintSurface` 먼저 캔버스의 중앙에 단일 비트맵을 표시 하는 것 처럼 좌표를 계산 합니다. 그런 다음 이러한 좌표를 사용 하 여에 대 한 변환 변환을 만듭니다 `SKShader.CreateBitmap` . 이 변환은 타일이 가운데 맞춤 되도록 전체 패턴을 이동 합니다.

```csharp
public class CenteredTilesPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(CenteredTilesPage),
                        "SkiaSharpFormsDemos.Media.monkey.png");

    public CenteredTilesPage ()
    {
        Title = "Centered Tiles";

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

        // Find coordinates to center bitmap in canvas...
        float x = (info.Width - bitmap.Width) / 2f;
        float y = (info.Height - bitmap.Height) / 2f;

        using (SKPaint paint = new SKPaint())
        {
            // ... but use them to create a translate transform
            SKMatrix matrix = SKMatrix.MakeTranslation(x, y);
            paint.Shader = SKShader.CreateBitmap(bitmap, 
                                                 SKShaderTileMode.Repeat, 
                                                 SKShaderTileMode.Repeat, 
                                                 matrix);

            // Use that tiled bitmap pattern to fill a circle
            canvas.DrawCircle(info.Rect.MidX, info.Rect.MidY,
                              Math.Min(info.Width, info.Height) / 2,
                              paint);
        }
    }
}
```

`PaintSurface`처리기는 캔버스의 중심에 원을 그려 종료 합니다. 충분 한 타일 중 하나가 원 중심에 정확 하 고 다른 타일은 대칭 패턴으로 정렬 됩니다.

[![가운데 맞춤 타일](bitmap-tiling-images/CenteredTiles.png "가운데 맞춤 타일")](bitmap-tiling-images/CenteredTiles-Large.png#lightbox)

또 다른 중심 접근 방식은 실제로 약간 더 간단 합니다. 타일을 가운데에 배치 하는 변환 변환을 생성 하는 대신 바둑판식 패턴의 모퉁이를 가운데에 맞출 수 있습니다. 호출에서 `SKMatrix.MakeTranslation` 캔버스의 중심에 대 한 인수를 사용 합니다.

```csharp
SKMatrix matrix = SKMatrix.MakeTranslation(info.Rect.MidX, info.Rect.MidY);
```

패턴은 여전히 가운데에 맞춰지고 대칭 이지만 가운데에는 타일이 없습니다.

[![가운데 맞춤 타일 대체](bitmap-tiling-images/CenteredTilesAlternate.png "가운데 맞춤 타일 대체")](bitmap-tiling-images/CenteredTilesAlternate-Large.png#lightbox)

## <a name="simplification-through-rotation"></a>회전을 통한 단순화

경우에 따라 메서드에서 회전 변환을 사용 하면 `SKShader.CreateBitmap` 비트맵 타일이 간단해 집니다. 이는 체인 링크 fence의 타일을 정의 하려고 할 때 분명 하 게 드러납니다. **ChainLinkTile.cs** 파일은 다음과 같이 표시 되는 타일을 만듭니다 (명확성을 위해 분홍색 배경 포함).

![하드 체인-링크 타일](bitmap-tiling-images/HardChainLinkTile.png "하드 체인-링크 타일")

타일은 두 개의 링크를 포함 해야 코드에서 타일을 4 개의 사분면으로 나눕니다. 왼쪽 위와 오른쪽 아래 사분면은 동일 하지만 완전 하지 않습니다. 와이어나 왼쪽 위 사분면에서 몇 가지 추가 그리기를 통해 처리 해야 하는 노치는 거의 없습니다. 이 모든 작업을 수행 하는 파일의 길이는 174 줄입니다.

이 타일을 훨씬 더 쉽게 만들 수 있습니다.

![더 쉬워진 체인-링크 타일](bitmap-tiling-images/EasierChainLinkTile.png "더 쉬워진 체인-링크 타일")

비트맵 타일 셰이더가 90도 회전 하면 시각적 개체가 거의 동일 하 게 표시 됩니다.

더 쉬운 체인 연결 타일을 만드는 코드는 **체인 연결 타일** 페이지의 일부입니다. 생성자는 프로그램을 실행 하는 장치 유형에 따라 타일 크기를 결정 한 다음 `CreateChainLinkTile` 선, 경로 및 그라데이션 셰이더를 사용 하 여 비트맵에 그려지는를 호출 합니다.

```csharp
public class ChainLinkFencePage : ContentPage
{
    ···
    SKBitmap tileBitmap;

    public ChainLinkFencePage ()
    {
        Title = "Chain-Link Fence";

        // Create bitmap for chain-link tiling
        int tileSize = Device.Idiom == TargetIdiom.Desktop ? 64 : 128;
        tileBitmap = CreateChainLinkTile(tileSize);

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    SKBitmap CreateChainLinkTile(int tileSize)
    {
        tileBitmap = new SKBitmap(tileSize, tileSize);
        float wireThickness = tileSize / 12f;

        using (SKCanvas canvas = new SKCanvas(tileBitmap))
        using (SKPaint paint = new SKPaint())
        {
            canvas.Clear();
            paint.Style = SKPaintStyle.Stroke;
            paint.StrokeWidth = wireThickness;
            paint.IsAntialias = true;

            // Draw straight wires first
            paint.Shader = SKShader.CreateLinearGradient(new SKPoint(0, 0),
                                                         new SKPoint(0, tileSize),
                                                         new SKColor[] { SKColors.Silver, SKColors.Black },
                                                         new float[] { 0.4f, 0.6f },
                                                         SKShaderTileMode.Clamp);

            canvas.DrawLine(0, tileSize / 2,
                            tileSize / 2, tileSize / 2 - wireThickness / 2, paint);

            canvas.DrawLine(tileSize, tileSize / 2,
                            tileSize / 2, tileSize / 2 + wireThickness / 2, paint);

            // Draw curved wires
            using (SKPath path = new SKPath())
            {
                path.MoveTo(tileSize / 2, 0);
                path.LineTo(tileSize / 2 - wireThickness / 2, tileSize / 2);
                path.ArcTo(wireThickness / 2, wireThickness / 2,
                           0,
                           SKPathArcSize.Small,
                           SKPathDirection.CounterClockwise,
                           tileSize / 2, tileSize / 2 + wireThickness / 2);

                paint.Shader = SKShader.CreateLinearGradient(new SKPoint(0, 0),
                                                             new SKPoint(0, tileSize),
                                                             new SKColor[] { SKColors.Silver, SKColors.Black },
                                                             null,
                                                             SKShaderTileMode.Clamp);
                canvas.DrawPath(path, paint);

                path.Reset();
                path.MoveTo(tileSize / 2, tileSize);
                path.LineTo(tileSize / 2 + wireThickness / 2, tileSize / 2);
                path.ArcTo(wireThickness / 2, wireThickness / 2,
                           0,
                           SKPathArcSize.Small,
                           SKPathDirection.CounterClockwise,
                           tileSize / 2, tileSize / 2 - wireThickness / 2);

                paint.Shader = SKShader.CreateLinearGradient(new SKPoint(0, 0),
                                                             new SKPoint(0, tileSize),
                                                             new SKColor[] { SKColors.White, SKColors.Silver },
                                                             null,
                                                             SKShaderTileMode.Clamp);
                canvas.DrawPath(path, paint);
            }
            return tileBitmap;
        }
    }
    ···
}
```

와이어를 제외 하 고 타일은 투명 합니다. 즉, 다른 항목 위에 표시할 수 있습니다. 프로그램은 비트맵 리소스 중 하나에서 로드 하 고 캔버스를 채우도록 표시 한 다음 위쪽에 셰이더를 그립니다.

```csharp
public class ChainLinkFencePage : ContentPage
{
    SKBitmap monkeyBitmap = BitmapExtensions.LoadBitmapResource(
        typeof(ChainLinkFencePage), "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");
    ···

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.DrawBitmap(monkeyBitmap, info.Rect, BitmapStretch.UniformToFill, 
                            BitmapAlignment.Center, BitmapAlignment.Start);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateBitmap(tileBitmap, 
                                                 SKShaderTileMode.Repeat,
                                                 SKShaderTileMode.Repeat,
                                                 SKMatrix.MakeRotationDegrees(45));
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

셰이더가 45 각도로 회전 되어 실제 체인 링크 fence와 같이 표시 됩니다.

[![체인 링크 Fence](bitmap-tiling-images/ChainLinkFence.png "체인 링크 Fence")](bitmap-tiling-images/ChainLinkFence-Large.png#lightbox)

## <a name="animating-bitmap-tiles"></a>비트맵 타일에 애니메이션 적용

행렬 변환에 애니메이션 효과를 적용 하 여 전체 비트맵 타일 패턴에 애니메이션 효과를 적용할 수 있습니다. 패턴이 가로 또는 세로 또는 양쪽 모두로 이동 하려는 경우가 있습니다. 이동 좌표를 기준으로 변환 변환을 만들어이 작업을 수행할 수 있습니다.

작은 비트맵에서 그리거나 60 시간 (초)의 속도로 비트맵의 픽셀 비트를 조작 하는 것도 가능 합니다. 이 비트맵을 바둑판식으로 배열 하는 데 사용할 수 있으며 바둑판식으로 표시 된 전체 패턴에 애니메이션을 적용할 수 있습니다. 

애니메이션을 적용 하는 **비트맵 타일** 페이지에서이 방법을 보여 줍니다. 비트맵이 64 픽셀 정사각형으로 인스턴스화된 필드로 인스턴스화됩니다. 생성자는 `DrawBitmap` 를 호출 하 여 초기 모양을 지정 합니다. `angle`필드가 0 인 경우 (메서드가 처음 호출 될 때) 비트맵에는 X로 두 개의 줄이 포함 됩니다. 줄은 값에 관계 없이 항상 비트맵의 가장자리에 도달할 수 있습니다 `angle` . 

```csharp
public class AnimatedBitmapTilePage : ContentPage
{
    const int SIZE = 64;

    SKCanvasView canvasView;
    SKBitmap bitmap = new SKBitmap(SIZE, SIZE);
    float angle;
    ···

    public AnimatedBitmapTilePage ()
    {
        Title = "Animated Bitmap Tile";

        // Initialize bitmap prior to animation
        DrawBitmap();

        // Create SKCanvasView 
        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ···
    void DrawBitmap()
    {
        using (SKCanvas canvas = new SKCanvas(bitmap))
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = SIZE / 8;

            canvas.Clear();
            canvas.Translate(SIZE / 2, SIZE / 2);
            canvas.RotateDegrees(angle);
            canvas.DrawLine(-SIZE, -SIZE, SIZE, SIZE, paint);
            canvas.DrawLine(-SIZE, SIZE, SIZE, -SIZE, paint);
        }
    }
    ···
}
```

애니메이션 오버 헤드는 및 재정의에서 발생 합니다 `OnAppearing` `OnDisappearing` . 이 `OnTimerTick` 메서드는 `angle` 비트맵 내에서 X 그림을 회전 하기 위해 10 초 마다 0도에서 360도로 값에 애니메이션 효과를 적용 합니다.

```csharp
public class AnimatedBitmapTilePage : ContentPage
{
    ···
    // For animation
    bool isAnimating;
    Stopwatch stopwatch = new Stopwatch();
    ···

    protected override void OnAppearing()
    {
        base.OnAppearing();

        isAnimating = true;
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();

        stopwatch.Stop();
        isAnimating = false;
    }

    bool OnTimerTick()
    {
        const int duration = 10;     // seconds
        angle = (float)(360f * (stopwatch.Elapsed.TotalSeconds % duration) / duration);
        DrawBitmap();
        canvasView.InvalidateSurface();

        return isAnimating;
    }
    ···
}
```

X 그림의 대칭으로 인해이는 `angle` 2.5 초 마다 0에서 90도로 값을 회전 하는 것과 같습니다.

`PaintSurface`처리기는 비트맵에서 셰이더를 만들고 paint 개체를 사용 하 여 전체 캔버스를 색으로 만듭니다.

```csharp
public class AnimatedBitmapTilePage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateBitmap(bitmap,
                                                 SKShaderTileMode.Mirror,
                                                 SKShaderTileMode.Mirror);
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

`SKShaderTileMode.Mirror`옵션을 사용 하면 각 비트맵에서 x의 arm이 인접 비트맵의 x와 조인 하 여 단순한 애니메이션 보다 훨씬 복잡 하 게 보이는 전체적인 애니메이션 패턴을 만들 수 있습니다.

[![애니메이션 된 비트맵 타일](bitmap-tiling-images/AnimatedBitmapTile.png "애니메이션 된 비트맵 타일")](bitmap-tiling-images/AnimatedBitmapTile-Large.png#lightbox)

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [이상 클록 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/catclock)
