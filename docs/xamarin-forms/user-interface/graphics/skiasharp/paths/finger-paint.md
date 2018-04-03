---
title: 손가락 그리기
description: 손가락을 사용 하 여 캔버스에 그릴 합니다.
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 56929D74-8F2C-44C6-90E6-3FBABCDC0A4B
author: charlespetzold
ms.author: chape
ms.date: 04/05/2017
ms.openlocfilehash: dacb9f399ad044d2d5e9c960bce398092766020c
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2018
---
# <a name="finger-painting"></a>손가락 그리기

_손가락을 사용 하 여 캔버스에 그릴 합니다._

`SKPath` 개체를 지속적으로 업데이트 하 고 표시할 수 있습니다. 이 기능에는 경로를 finger-painting 프로그램에서와 같은 대화형 그리기에 사용할 수 있습니다.

![](finger-paint-images/fingerpaintsample.png "손가락 그리기에서의 실행")

Xamarin.Forms에 터치 조작 지원 하므로 Xamarin.Forms 터치 추적 효과 추가 터치 지원을 제공 하기 위해 개발 되었습니다 화면의 개별 손가락을 추적 하는 것을 허용 하지 않습니다. 이 효과 문서에 설명 되어 [ **호출 이벤트 효과를**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)합니다. 샘플 프로그램 [ **터치 추적 효과 데모** ](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/) finger-painting 프로그램을 포함 하 여 SkiaSharp를 사용 하는 두 개의 페이지가 포함 됩니다.

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 솔루션에이 터치 추적 이벤트를 포함 합니다. 이식 가능한 클래스 라이브러리 프로젝트에 포함 되어는 `TouchEffect` 클래스는 `TouchActionType` 열거형은 `TouchActionEventHandler` 대리자 및 `TouchActionEventArgs` 클래스입니다. 각 플랫폼 프로젝트를 포함 한 `TouchEffect` 해당 플랫폼에 대 한 클래스; iOS 프로젝트에 포함 됩니다는 `TouchRecognizer` 클래스입니다.

**손가락 페인트** 페이지 **SkiaSharpFormsDemos** 손가락 그리기의 간단한 구현입니다. 색을 선택할 수 있도록 하거나 스트로크 너비 하지 않습니다, 캔버스를 지울 수 있는 방법이 및 물론 아트 워크를 저장할 수 없습니다.

[ **FingerPaintPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml) 배치 파일의 `SKCanvasView` 단일 셀에 `Grid` 연결는 `TouchEffect` 되도록 `Grid`:

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

연결 된 `TouchEffect` 에 직접는 `SKCanvasView` 모든 플랫폼에서 작동 하지 않습니다.

[ **FingerPaintPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml.cs) 코드 숨김 파일을 저장 하기 위한 두 개의 컬렉션 정의 `SKPath` 개체 물론 `SKPaint` 이러한 경로 렌더링 하기 위한 개체:

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

이름이 제안으로는 `inProgressPaths` 사전에 하나 이상의 손가락으로 그려진 되 고 현재 경로 저장 합니다. 사전 키는 터치 이벤트와 함께 제공 되는 터치 ID를 설정 합니다. `completedPaths` 필드는 경우 리프트 경로 화면에서 그리기 손가락 완료 된 경로 컬렉션입니다.

`TouchAction` 처리기는이 두 컬렉션을 관리 합니다. 손가락이 먼저 화면을 터치 하면 새 `SKPath` 에 추가 `inProgressPaths`합니다. 해당 손가락 이동 경로에 추가 점은 추가 됩니다. 으로 전송 되는 경로 손가락 출시 되 면는 `completedPaths` 컬렉션입니다. 동시에 여러 손가락으로 채울 수 있습니다. 경로 또는 컬렉션 중 하나에 변경 될 때마다는 `SKCanvasView` 무효화 됩니다.

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

터치 추적 이벤트를 함께 나타날 사항이 Xamarin.Forms 좌표; 이러한 SkiaSharp 좌표 (픽셀)를으로 변환 해야 합니다. 용도 하는 `ConvertToPixel` 메서드.

`PaintSurface` 처리기 다음 렌더링 두 컬렉션의 경로입니다. 완료 된 이전 경로 진행에서 경로 아래에 나타납니다.

```csharp
public partial class FingerPaintPage : ContentPage
{
    ,,,
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

손가락 그림 프로그램 인력에 의해서만 제한 됩니다.

[![](finger-paint-images/fingerpaint-small.png "손가락 페인트 페이지의 삼중 스크린샷")](finger-paint-images/fingerpaint-large.png#lightbox "손가락 페인트 페이지의 삼중 스크린샷")


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [터치 추적 효과 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/)
- [효과의 이벤트를 호출합니다.](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
