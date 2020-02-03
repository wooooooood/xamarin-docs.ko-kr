---
title: SkiaSharp에서 손가락 페인팅
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에서 SkiaSharp 캔버스에 그릴 손가락을 사용 하는 방법에 설명 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 56929D74-8F2C-44C6-90E6-3FBABCDC0A4B
author: davidbritch
ms.author: dabritch
ms.date: 04/05/2017
ms.openlocfilehash: d5cf0927c64732d6d0a44204db9509fae77f0d1d
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724075"
---
# <a name="finger-painting-in-skiasharp"></a>SkiaSharp에서 손가락 페인팅

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_손가락을 사용 하 여 캔버스에 그립니다._

`SKPath` 개체를 지속적으로 업데이트 하 고 표시할 수 있습니다. 이 기능은 손가락 프로그램에서와 같은 대화형 그리기에 사용할 경로입니다.

![](finger-paint-images/fingerpaintsample.png "An exercise in finger painting")

Xamarin.Forms의 터치 지원을 Xamarin.Forms 터치 추적 효과 추가 터치 지원을 제공 하기 위해 개발 되었습니다 하므로 화면의 각 손가락을 추적 하는 것을 허용 하지 않습니다. 이 효과는 [**효과에서 이벤트를 호출**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)하는 문서에 설명 되어 있습니다. 샘플 프로그램 [**터치 추적 효과 데모**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/) 에는 손가락 그리기 프로그램을 포함 하 여 SkiaSharp를 사용 하는 두 페이지가 포함 되어 있습니다.

[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 솔루션에는이 터치 추적 이벤트가 포함 됩니다. .NET Standard library 프로젝트에는 `TouchEffect` 클래스, `TouchActionType` 열거형, `TouchActionEventHandler` 대리자 및 `TouchActionEventArgs` 클래스가 포함 됩니다. 각 플랫폼 프로젝트에는 해당 플랫폼에 대 한 `TouchEffect` 클래스가 포함 됩니다. iOS 프로젝트에는 `TouchRecognizer` 클래스도 포함 되어 있습니다.

**SkiaSharpFormsDemos** 의 **손가락 그리기** 페이지는 손가락 그리기의 단순화 된 구현입니다. 색을 선택할 수 있게 하거나 너비를 그리지 않습니다, 캔버스를 지울 수 및 물론 아트 워크를 저장할 수 없습니다.

[**FingerPaintPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/FingerPaintPage.xaml) 파일은 `SKCanvasView` `Grid` 단일 셀에 넣고 `TouchEffect`를 해당 `Grid`에 연결 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Paths.FingerPaintPage"
             Title="Finger Paint">

    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

`TouchEffect`를 `SKCanvasView`에 직접 연결 하는 것은 모든 플랫폼에서 작동 하지 않습니다.

[**FingerPaintPage.xaml.cs 코드**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/FingerPaintPage.xaml.cs) 파일은 `SKPath` 개체를 저장 하기 위한 두 개의 컬렉션과 이러한 경로를 렌더링 하기 위한 `SKPaint` 개체를 정의 합니다.

```csharp
public partial class FingerPaintPage : ContentPage
{
    Dictionary<long, SKPath> inProgressPaths = new Dictionary<long, SKPath>();
    List<SKPath> completedPaths = new List<SKPath>();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };

    public FingerPaintPage()
    {
        InitializeComponent();
    }
    ...
}
```

이름에서 알 수 있듯이 `inProgressPaths` 사전은 현재 하나 이상의 손가락으로 그려지는 경로를 저장 합니다. 사전 키에는 터치 이벤트와 함께 제공 되는 터치 ID입니다. `completedPaths` 필드는 경로를 그리는 손가락이 화면에서 리프트 될 때 완료 된 경로 컬렉션입니다.

`TouchAction` 처리기는 이러한 두 컬렉션을 관리 합니다. 손가락이 화면에 처음 터치 하면 새 `SKPath` `inProgressPaths`에 추가 됩니다. 손가락 이동 경로에 추가 점은 추가 됩니다. 손가락을 놓으면 경로가 `completedPaths` 컬렉션으로 전송 됩니다. 동시에 여러 손가락으로 그릴 수 있습니다. 경로 또는 컬렉션 중 하나를 변경한 후에는 `SKCanvasView` 무효화 됩니다.

```csharp
public partial class FingerPaintPage : ContentPage
{
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = new SKPath();
                    path.MoveTo(ConvertToPixel(args.Location));
                    inProgressPaths.Add(args.Id, path);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Moved:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = inProgressPaths[args.Id];
                    path.LineTo(ConvertToPixel(args.Location));
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    completedPaths.Add(inProgressPaths[args.Id]);
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Cancelled:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
    SKPoint ConvertToPixel(Point pt)
    {
        return new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                           (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));
    }
}
```

터치 추적 이벤트를 함께 제공 되는 지점은 Xamarin.Forms 좌표입니다. 이러한 SkiaSharp 좌표 (픽셀)를 변환할 수 있어야 합니다. 이는 `ConvertToPixel` 메서드의 용도입니다.

그러면 `PaintSurface` 처리기가 두 경로 컬렉션을 모두 렌더링 합니다. 완료 된 이전 경로 진행에서 경로 아래에 나타납니다.

```csharp
public partial class FingerPaintPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (SKPath path in completedPaths)
        {
            canvas.DrawPath(path, paint);
        }

        foreach (SKPath path in inProgressPaths.Values)
        {
            canvas.DrawPath(path, paint);
        }
    }
    ...
}
```

에 손가락 회화 재능에 의해서만 제한 됩니다.

[![](finger-paint-images/fingerpaint-small.png "Triple screenshot of the Finger Paint page")](finger-paint-images/fingerpaint-large.png#lightbox "Triple screenshot of the Finger Paint page")

이제 선을 그리려면 매개 방정식을 사용 하 여 곡선을 정의 하는 방법을 살펴봤습니다. [**SkiaSharp 곡선 및 경로**](../curves/index.md) 에 대 한 이후 섹션에서는 `SKPath`에서 지 원하는 다양 한 곡선 유형을 설명 합니다. 하지만 유용한 필수 구성 요소는 [**SkiaSharp 변환**](../transforms/index.md)에 대 한 탐색입니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [터치식 추적 효과 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/)
- [효과에서 이벤트 호출](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
