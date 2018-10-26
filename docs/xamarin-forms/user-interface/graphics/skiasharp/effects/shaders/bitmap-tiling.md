---
title: SkiaSharp 비트맵 바둑판식 배열
description: 가로 및 세로 방향으로 반복 하는 비트맵을 사용 하 여 영역 바둑판식으로 배열 합니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 9ED14E07-4DC8-4B03-8A33-772838BF51EA
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 5bd063f82cc1d09c6b2e9100429889a23a2eda7f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111587"
---
# <a name="skiasharp-bitmap-tiling"></a>SkiaSharp 비트맵 바둑판식 배열

두 가지 이전 문서에서는 지금까지 살펴본 대로 합니다 [ `SKShader` ](xref:SkiaSharp.SKShader) 클래스는 선형 또는 순환 그라데이션을 만들 수 있습니다. 이 문서에서는 `SKShader` 타일 영역에 비트맵을 사용 하는 개체입니다. 가로 방향과 세로 방향으로 비트맵을 반복할 수 있습니다 원래 방향에서 가로 및 세로로 대칭 이동 또는 또는 합니다. 대칭 이동 타일 간에 불연속성을 방지합니다.

![바둑판식 배열 예제 비트맵](bitmap-tiling-images/BitmapTilingSample.png "Bitmap 바둑판식 배열 샘플")

정적 [ `SKShader.CreateBitmap` ](xref:SkiaSharp.SKShader.CreateBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKShaderTileMode,SkiaSharp.SKShaderTileMode)) 이 셰이더를 만드는 방법에는 `SKBitmap` 매개 변수 및 두 명의 멤버를 [ `SKShaderTileMode` ](xref:SkiaSharp.SKShaderTileMode) 열거형:

```csharp
public static SKShader CreateBitmap (SKBitmap src, SKShaderTileMode tmx, SKShaderTileMode tmy)
```

두 매개 변수는 가로 바둑판식 배열 및 세로 바둑판식 배열에 사용 되는 모드를 나타냅니다. 이 동일 `SKShaderTileMode` 그라데이션 메서드로 사용 되는 열거형입니다.

A [ `CreateBitmap` ](xref:SkiaSharp.SKShader.CreateBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKShaderTileMode,SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) 오버 로드는 `SKMatrix` 바둑판식으로 배열 된 비트맵에는 변환을 수행 하려면 인수:

```csharp
public static SKShader CreateBitmap (SKBitmap src, SKShaderTileMode tmx, SKShaderTileMode tmy, SKMatrix localMatrix)
```

이 매트릭스 변환을 사용 하 여 바둑판식으로 배열 된 비트맵을 사용 하 여 몇 가지 예가 나와 있습니다.

## <a name="exploring-the-tile-modes"></a>타일 모드를 탐색합니다.

첫 번째 프로그램 합니다 **비트맵 바둑판식 배열** 섹션을 **셰이더 및 기타 효과** 페이지를 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 샘플 두 개의 결과 보여 주는 `SKShaderTileMode` 인수입니다. 합니다 **비트맵 타일 대칭 이동 모드** XAML 파일은는 `SKCanvasView` 두 개의 `Picker` 선택할 수 있도록 하는 뷰는 `SKShaderTilerMode` 가로 및 세로 바둑판식 배열에 대 한 값입니다. 있음을 배열을 합니다 `SKShaderTileMode` 에 정의 된 멤버를 `Resources` 섹션:

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

코드 숨김 파일의 생성자에 앉아을 monkey를 보여 주는 비트맵 리소스를 로드 합니다. 처음 사용 하 여 이미지를 자릅니다 합니다 [ `ExtractSubset` ](xref:SkiaSharp.SKBitmap.ExtractSubset(SkiaSharp.SKBitmap,SkiaSharp.SKRectI)) 메서드의 `SKBitmap` 헤드 및 feet 비트맵의 가장자리를 터치 하 수 있도록 합니다. 생성자를 사용 하 여는 [ `Resize` ](xref:SkiaSharp.SKBitmap.Resize(SkiaSharp.SKImageInfo,SkiaSharp.SKBitmapResizeMethod)) 메서드를 다른 비트 멥의 절반 크기로 만듭니다. 이러한 변경 비트맵 바둑판식 배열에 대 한 좀 더 적합 합니다.

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

`PaintSurface` 처리기를 가져옵니다 합니다 `SKShaderTileMode` 설정의 두에서 `Picker` 만들고 뷰는 `SKShader` 비트맵 및이 두 값을 기준으로 개체입니다. 이 셰이더는 캔버스를 채우도록 사용 됩니다.

[![플립 타일 모드를 비트맵](bitmap-tiling-images/BitmapTileFlipModes.png "비트맵 플립 타일 모드")](bitmap-tiling-images/BitmapTileFlipModes-Large.png#lightbox)

IOS 화면 왼쪽에 있는 기본 값의 효과 보여 줍니다. `SKShaderTileMode.Clamp`합니다. 비트맵 왼쪽 위 모퉁이에 배치 됩니다. 아래 비트맵 픽셀의 맨 아래쪽 행 아래쪽으로 반복 됩니다. 비트맵의 오른쪽으로 픽셀의 맨 오른쪽 열은 모든 과정에서 반복 됩니다. 캔버스의 나머지 부분으로 비트맵의 오른쪽 아래 모서리에 있는 어두운 밤색 픽셀 색입니다. 확실 한 것입니다는 `Clamp` 옵션 거의 사용 되지 비트맵 바둑판식 배열을 사용 하 여!

Android 화면 가운데에 결과 보여 줍니다. `SKShaderTileMode.Repeat` 두 인수에 대 한 합니다. 타일 세로 및 가로로 반복 됩니다. 유니버설 Windows 플랫폼 화면 표시 `SKShaderTileMode.Mirror`합니다. 타일 반복 되지만 또는 가로 및 세로로 대칭 이동 합니다. 이 옵션의 장점은 타일 간에 없습니다 연속 되지 않을 한다는 점입니다.

가로 및 세로 반복에 대 한 다양 한 옵션을 사용할 수 있는 것을 염두에 두십시오. 지정할 수 있습니다 `SKShaderTileMode.Mirror` 두 번째 인수로 `CreateBitmap` 있지만 `SKShaderTileMode.Repeat` 세 번째 인수입니다. 각 행에 여전히 일반 이미지 및 미러 이미지 되지만 원숭이 번갈아 원숭이 거꾸로 있습니다.

## <a name="patterned-backgrounds"></a>배경 무늬

비트맵 바둑판식 배열에서 비교적 작은 비트맵을 배경 무늬를 만들려면 일반적으로 사용 됩니다. 클래식 예로 brick 벽을 보여 줍니다.

합니다 **알고리즘 Brick 벽** 페이지 전체 brick 및 구성 요소를 구분 하는 brick의 반 유사한 작은 비트맵을 만듭니다. 이 brick에서 다음 샘플 에서도 사용 되므로 정적 생성자가 만든 고 공용 정적 속성으로 설정 합니다.

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

결과 비트맵 60 픽셀 높이 70 픽셀입니다.

![알고리즘 Brick 타일 벽](bitmap-tiling-images/AlgorithmicBrickWallTile.png "알고리즘 Brick 벽 타일")

나머지는 **알고리즘 Brick 벽** 를 만들고이 `SKShader` 가로 세로 방향으로이 이미지를 반복 하는 개체:

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

결과 다음과 같습니다.

[![알고리즘 Brick 벽](bitmap-tiling-images/AlgorithmicBrickWall.png "알고리즘 Brick 벽")](bitmap-tiling-images/AlgorithmicBrickWall-Large.png#lightbox)

좀 더 현실적인 것을 선호할 수 있습니다. 이 경우 실제 brick 벽의 사진을 걸릴 수 있으며 다음 자를 수 있습니다. 이 비트맵은 300 픽셀 너비 및 높이 150 픽셀입니다.

![Brick 타일 벽](bitmap-tiling-images/BrickWallTile.jpg "Brick 벽 타일")

이 비트맵에 사용 되는 **Photographic Brick 벽** 페이지:

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

다음에 유의 합니다 `SKShaderTileMode` 인수를 `CreateBitmap` 둘 다 `Mirror`합니다. 실제 이미지에서 만든 타일을 사용 하는 경우이 옵션은 일반적으로 필요 합니다. 타일을 미러링 불연속성을 방지 합니다.

[![사진 Brick 벽](bitmap-tiling-images/PhotographicBrickWall.png "사진 Brick 벽")](bitmap-tiling-images/PhotographicBrickWall-Large.png#lightbox)

타일에 대 한 적절 한 비트맵을 가져오려면 몇 가지 작업이 필요 합니다. 음영이 짙을 수록 brick 눈에 띄는 때문에 매우 잘 작동 하지 않고이 너무 많이 있습니다. Brick 벽이 작은 비트맵에서 생성 된는 사실을 드러내지 반복된 이미지 내에서 정기적으로 나타납니다.

**미디어** 폴더를 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 샘플 돌 벽이 이미지에도 포함 되어 있습니다.

![Stone 타일 벽](bitmap-tiling-images/StoneWallTile.jpg "Stone 벽 타일")

그러나 원래 비트맵이 타일에 대 한 약간 너무 큽니다. 크기를 조정할 수 없습니다, 있지만 `SKShader.CreateBitmap` 메서드에 변환을 적용 하 여 타일 크기 조정할 수도 있습니다. 이 옵션에 설명 되어는 **Stone 벽** 페이지:

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

`SKMatrix` 를 원래 크기의 절반 이미지 배율을 조정 하려면 값이 만들어집니다.

[![벽 석재](bitmap-tiling-images/StoneWall.png "벽 석재")](bitmap-tiling-images/StoneWall-Large.png#lightbox)

변환에 사용 된 원래 비트맵에서 작동 합니다 `CreateBitmap` 메서드? 타일의 결과 배열을 변환 여부 

이 질문에 대답 하는 간편한 방법은 회전 변환의 일부로 포함 하는 것:

```csharp
SKMatrix matrix = SKMatrix.MakeScale(0.5f, 0.5f);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeRotationDegrees(15));
```

변환은 개별 타일에 적용 되는 경우 다음 타일의 각 반복 된 이미지를 회전 해야 하는지, 및 결과는 여러 불연속 포함 됩니다. 하지만 타일의 복합 배열을 변환 된이 스크린샷에서 명확 합니다.

[![회전 벽 석재](bitmap-tiling-images/StoneWallRotated.png "석재 벽 회전")](bitmap-tiling-images/StoneWallRotated-Large.png#lightbox)

단원의 [ **맞춤 타일**](#tile-alignment), 셰이더를 적용할 좌표 이동 변환 예를 볼 수 있습니다.

독립 실행형 [ **Cat 클록** ](https://developer.xamarin.com/samples/xamarin-forms/CatClock) 샘플 (의 일부가 아닌 **SkiaSharpFormsDemos**) wood 수준 배경이 비트맵 바둑판식 배열이 240 픽셀 사각형 비트맵 기준으로 사용 하 여 시뮬레이션 합니다.

![조직 나무](bitmap-tiling-images/WoodGrain.png "세분화 나무")

목재 floor 사진입니다. `SKShaderTileMode.Mirror` 옵션 목재 훨씬 더 큰 영역을 표시 하도록 허용 합니다.

[![이 모의 고양이 인 클록](bitmap-tiling-images/CatClock.png "클록이 모의 고양이 인")](bitmap-tiling-images/CatClock-Large.png#lightbox)

## <a name="tile-alignment"></a>타일 맞춤

앞에서 설명한 모든 예제에서 만든 셰이더 사용한 `SKShader.CreateBitmap` 전체 캔버스를 포함 하도록 합니다. 대부분의 경우에서 사용할 비트맵 바둑판식 배열 더 작은 영역 이상 (거의)를 작성 하기 위한 두꺼운 선의 내부를 채우는 합니다. 더 작은 사각형에 사용 되는 사진 brick-wall 타일은 다음과 같습니다.

[![맞춤 타일](bitmap-tiling-images/TileAlignment.png "맞춤 타일")](bitmap-tiling-images/TileAlignment-Large.png#lightbox)

이 많다는 것을 세밀 하 게 보일 수 있습니다. 아마도 여러분 바둑판식 배열 패턴 사각형의 왼쪽 위 모서리에 있는 전체 brick을 시작 하지는 것입니다. 이것은 셰이더 캔버스 및 그래픽 개체는 장식 하는 정렬 됩니다.

문제를 해결 하는 것은 간단 합니다. 만들기는 `SKMatrix` 값 이동 변환을 기반으로 합니다. 변환 정렬 하려면 타일의 왼쪽 위 모퉁이 하려는 지점에 바둑판식 배열된 패턴을 효과적으로 이동 합니다. 이 방법을 설명 합니다 **타일 맞춤** 위에 표시 된 정렬 되지 않은 타일의 이미지를 생성 하는 페이지:

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

합니다 **타일 맞춤** 페이지에는 `TapGestureRecognizer`합니다. 화면 및 프로그램 스위치를 클릭 또는 탭의 `SKShader.CreateBitmap` 메서드는 `SKMatrix` 인수입니다. 이 변환은 왼쪽 위 모퉁이 전체 brick이 포함 되도록 패턴을 이동 합니다.

[![맞춤 탭 타일](bitmap-tiling-images/TileAlignmentTapped.png "맞춤 탭 타일")](bitmap-tiling-images/TileAlignmentTapped-Large.png#lightbox)

또한 비트맵을 바둑판식으로 배열 된 패턴에 그려질 내에서 가운데 정렬 되어 있음을 확인이 기법을 사용할 수 있습니다. 에 **가운데 타일** 페이지를 `PaintSurface` 처리기는 먼저 좌표를 계산 단일 비트맵 캔버스의 가운데에 표시 하려는 경우. 그런 다음 이러한 좌표에 대 한 좌표 이동 변환을 만들려면 `SKShader.CreateBitmap`합니다. 이 변환은 전체 패턴을 이동 타일을 가운데에 표시 됩니다.

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

`PaintSurface` 캔버스의 가운데에서 원 그리기 여 처리기가 종료 됩니다. 에 타일 중 하나는 원의 중심으로 정확 하 게 되며 대칭 패턴에 따라 정렬 되며 나머지:

[![타일을 가운데](bitmap-tiling-images/CenteredTiles.png "타일을 가운데 맞춤")](bitmap-tiling-images/CenteredTiles-Large.png#lightbox)

또 다른 가운데 맞춤 방법은 실제로 약간 더 편리 합니다. 타일을 가운데에 배치 하는 좌표 이동 변환을 만드는 것이 아니라 바둑판식 배열된 패턴 상단을 가운데 놓을 수 있습니다. 에 `SKMatrix.MakeTranslation` 호출 캔버스의 가운데에 대 한 인수를 사용 합니다.

```csharp
SKMatrix matrix = SKMatrix.MakeTranslation(info.Rect.MidX, info.Rect.MidY);
```

패턴은 여전히 가운데 맞춤 및 대칭, 이지만 없습니다 타일 센터에서:

[![타일 대체 가운데](bitmap-tiling-images/CenteredTilesAlternate.png "타일 대체 가운데 맞춤")](bitmap-tiling-images/CenteredTilesAlternate-Large.png#lightbox)

## <a name="simplification-through-rotation"></a>회전을 통해 단순화

경우에 따라에서 회전 변환을 사용 하는 `SKShader.CreateBitmap` 메서드 비트맵 타일을 간소화할 수 있습니다. 이 연결 고리 fence 타일을 정의 하는 경우에 분명해 합니다. 합니다 **ChainLinkTile.cs** 파일 (배경의 분홍색 명확히 전달 하기 위해) 여기에 표시 된 타일을 만듭니다.

![하드 링크 체인 타일](bitmap-tiling-images/HardChainLinkTile.png "하드 체인 연결 타일")

타일 코드 4 개의 사분원으로 타일을 나누는 수 있도록 두 개의 링크를 포함 해야 합니다. 왼쪽 및 오른쪽 아래 사분면은 동일 하지만 완전 하지 않습니다. 선을 추가 그리기 오른쪽 및 왼쪽 아래 사분면에 거의 노치 처리 해야 하는 경우 이 모든 작업을 수행 하는 파일이 174 줄입니다.

판명 훨씬 쉽게이 타일을 만들 수 있게 합니다.

![쉽게 연결 고리 타일](bitmap-tiling-images/EasierChainLinkTile.png "쉽게 연결 고리 타일")

비트맵 타일 셰이더 90도 회전된 인 경우 시각적 개체는 거의 동일 합니다.

일부인 쉽게 연결 고리 타일을 만드는 코드를 **체인 연결 타일** 페이지입니다. 프로그램을 실행 하는 장치 및 호출의 형식에 따라 타일 크기를 결정 하는 생성자 `CreateChainLinkTile`, 줄, 경로 및 그라데이션 셰이더를 사용 하 여 비트맵에는 그립니다.

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

와이어를 제외 하 고 타일은 투명 하 게 하는 다른 것을 기반으로 표시할 수 있습니다. 프로그램은 비트맵 리소스 중 하나에서 로드 하 고, 캔버스를 채우도록 표시, 위쪽에서 셰이더를 그립니다.

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

셰이더 45도 회전된 하는 실제 연결 고리 펜스를 같은 방향 이므로 알 수 있습니다.

[![연결 고리 Fence](bitmap-tiling-images/ChainLinkFence.png "연결 고리 펜스")](bitmap-tiling-images/ChainLinkFence-Large.png#lightbox)

## <a name="animating-bitmap-tiles"></a>애니메이션 비트맵 타일

매트릭스 변환 애니메이션을 통해 전체 비트맵 바둑판식 배열 패턴을 애니메이션을 적용할 수 있습니다. 아마도 가로 또는 세로로 이동 하는 패턴 또는 둘 다는 것이 해야 합니다. 변화 하는 좌표를 따라 이동 변환을 만들어 수행할 수 있습니다.

초당 60 번 속도로 비트맵의 픽셀 비트를 조작 하거나 작은 비트맵을 그릴 수 이기도 합니다. 바둑판식 배열에 대 한 해당 비트맵 다음 사용할 수 있으며 전체 바둑판식 배열된 패턴에 애니메이션을 적용할 것 처럼 보일 수 있습니다. 

합니다 **애니메이션 비트맵 타일** 페이지에는이 방법을 보여 줍니다. 비트맵 64 픽셀 사각형이 되도록 필드로 인스턴스화됩니다. 생성자 호출 `DrawBitmap` 초기 모양을 제공 합니다. 경우는 `angle` 필드는 0 (이므로 메서드가 먼저 호출 되 면) 비트맵 X로 교차 하는 두 줄을 포함 합니다. 줄 내용이 항상 관계 없이 비트맵의 가장자리에 도달할 수 있을 정도로 `angle` 값: 

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

애니메이션 오버 헤드가 발생 합니다 `OnAppearing` 및 `OnDisappearing` 재정의 합니다. `OnTimerTick` 메서드 애니메이션을 적용 합니다 `angle` 값 0도에서 360도 10 초 마다 비트맵 내 X 그림을 회전 하려면:

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

X 그림의 대칭으로 인해이 동일 회전을 `angle` 0도에서 90도 2.5 초 마다 값입니다.

`PaintSurface` 처리기가 비트맵에서 셰이더를 만들고 그리기 개체를 사용 하 여 전체 캔버스 색:

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

`SKShaderTileMode.Mirror` 옵션은 각 비트맵에 있는 X의 가입 해야 대부분 보이는 전체적인 애니메이션된 패턴을 만들려면 인접 한 비트맵에 있는 X를 사용 하 여 간단한 애니메이션 제안할 것 보다 더 복잡 합니다.

[![비트맵 타일 애니메이션](bitmap-tiling-images/AnimatedBitmapTile.png "애니메이션 비트맵 타일")](bitmap-tiling-images/AnimatedBitmapTile-Large.png#lightbox)

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [CatClock (샘플)](https://developer.xamarin.com/samples/xamarin-forms/CatClock/)
