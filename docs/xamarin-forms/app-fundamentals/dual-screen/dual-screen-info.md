---
title: Xamarin.Forms DualScreenInfo
description: 이 가이드에서는 Xamarin.Forms DualScreenInfo 클래스를 사용하여 Surface Duo 및 Surface Neo와 같은 이중 화면 디바이스의 앱 환경을 최적화하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: dd5eb074-f4cb-4ab4-b47d-76f862ac7cfa
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: e9a01ed3720f1501423eb1c0746d311918af82fb
ms.sourcegitcommit: 524fc148bad17272bda83c50775771daa45bfd7e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77480591"
---
# <a name="xamarinforms-dualscreeninfo"></a>Xamarin.Forms DualScreenInfo

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/UserInterface/DualScreenDemos)

`DualScreenInfo` 클래스를 사용하면 보기가 표시된 창과 그 크기, 디바이스의 방향, 힌지 각도 등을 확인할 수 있습니다.

## <a name="properties"></a>속성

- 두 화면에 걸쳐 표시된 경우 `SpanningBounds`는 각 가시 영역의 범위를 나타내는 두 개의 사각형을 반환합니다. 창이 걸쳐 있지 않으면 빈 배열을 반환합니다.
- `HingeBounds`는 화면에서 힌지의 위치를 나타냅니다.
- `IsLandscape` - 디바이스가 가로인지 여부를 나타냅니다. 애플리케이션이 두 화면에 걸쳐 있는 경우 기본 방향 API가 방향을 올바르게 보고하지 않기 때문에 이 속성이 유용합니다.
- `SpanMode` - 레이아웃이 세로, 가로 또는 단일 창 중 어떤 모드인지를 나타냅니다.

이에 더해, 속성이 하나라도 변경되면 `PropertyChanged` 이벤트가 발생합니다.

## <a name="poll-hinge-angle-on-android"></a>Android의 힌지 각도 확인

Android 플랫폼 프로젝트에서 `DualScreenInfo`에 액세스할 때 다음과 같은 속성을 사용할 수 있습니다.

- `GetHingeAngleAsync` - 디바이스 힌지의 현재 각도를 검색합니다. 시뮬레이터를 사용하는 경우 압력 센서를 수정하여 HingeAngle을 설정할 수 있습니다.

이 속성은 다음과 같이 Android 사용자 지정 렌더러에서 사용할 수 있습니다.

```csharp
public class HingeAngleLabelRenderer : Xamarin.Forms.Platform.Android.FastRenderers.LabelRenderer
{
    System.Timers.Timer _hingeTimer;
    public HingeAngleLabelRenderer(Context context) : base(context)
    {
    }

    async void OnTimerElapsed(object sender, System.Timers.ElapsedEventArgs e)
    {
        if (_hingeTimer == null)
            return;

        _hingeTimer.Stop();
        var hingeAngle = await DualScreenInfo.Current.GetHingeAngleAsync();

        Device.BeginInvokeOnMainThread(() =>
        {
            if (_hingeTimer != null)
                Element.Text = hingeAngle.ToString();
        });

        if (_hingeTimer != null)
            _hingeTimer.Start();
    }

    protected override void OnElementChanged(ElementChangedEventArgs<Label> e)
    {
        base.OnElementChanged(e);

        if (_hingeTimer == null)
        {
            _hingeTimer = new System.Timers.Timer(100);
            _hingeTimer.Elapsed += OnTimerElapsed;
            _hingeTimer.Start();
        }
    }

    protected override void Dispose(bool disposing)
    {
        if (_hingeTimer != null)
        {
            _hingeTimer.Elapsed -= OnTimerElapsed;
            _hingeTimer.Stop();
            _hingeTimer = null;
        }

        base.Dispose(disposing);
    }
}
```

## <a name="access-dualscreeninfo-in-your-application-window"></a>애플리케이션 창의 DualScreenInfo에 액세스

다음 코드는 애플리케이션 창의 `DualScreenInfo`에 액세스하는 방법을 보여 줍니다.

```csharp
DualScreenInfo currentWindow = DualScreenInfo.Current;

// Retrieve absolute position of the hinge on the screen
var hingeBounds = currentWindow.HingeBounds;

// check if app window is spanned across two screens
if(currentWindow.SpanMode == TwoPaneViewMode.SinglePane)
{
    // window is only on one screen
}
else if(currentWindow.SpanMode == TwoPaneViewMode.Tall)
{
    // window is spanned across two screens and oriented top-bottom
}
else if(currentWindow.SpanMode == TwoPaneViewMode.Wide)
{
    // window is spanned across two screens and oriented side-by-side
}

// Detect if any of the properties on DualScreenInfo change.
// This is useful to detect if the app window gets spanned
// across two screens or put on only one  
currentWindow.PropertyChanged += OnDualScreenInfoChanged;
```

## <a name="apply-dualscreeninfo-to-layouts"></a>레이아웃에 DualScreenInfo 적용

`DualScreenInfo` 클래스에는 하나의 레이아웃을 받아서 디바이스의 두 화면에 대해 상대적으로 해당 레이아웃에 대한 정보를 제공하는 생성자가 있습니다.

```xaml
<Grid x:Name="grid" ColumnSpacing="0">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="{Binding Column1Width}" />
        <ColumnDefinition Width="{Binding Column2Width}" />
        <ColumnDefinition Width="{Binding Column3Width}" />
    </Grid.ColumnDefinitions>
    <Label FontSize="Large"
           VerticalOptions="Center"
           HorizontalOptions="End"
           Text="I should be on the left side of the hinge" />
    <Label FontSize="Large"
           VerticalOptions="Center"
           HorizontalOptions="Start"
           Grid.Column="2"
           Text="I should be on the right side of the hinge" />
</Grid>
```

```csharp
public partial class GridUsingDualScreenInfo : ContentPage
{
    public DualScreenInfo DualScreenInfo { get; }
    public double Column1Width { get; set; }
    public double Column2Width { get; set; }
    public double Column3Width { get; set; }

    public GridUsingDualScreenInfo()
    {
        InitializeComponent();
        DualScreenInfo = new DualScreenInfo(grid);
        BindingContext = this;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        DualScreenInfo.PropertyChanged += OnInfoPropertyChanged;
        UpdateColumns();
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        DualScreenInfo.PropertyChanged -= OnInfoPropertyChanged;
    }

    void UpdateColumns()
    {
        // Check if grid is on two screens
        if (DualScreenInfo.SpanningBounds.Length > 0)
        {
            // set the width of the first column to the width of the layout
            // that's on the left screen
            Column1Width = DualScreenInfo.SpanningBounds[0].Width;

            // set the middle column to the width of the hinge
            Column2Width = DualScreenInfo.HingeBounds.Width;

            // set the width of the third column to the width of the layout
            // that's on the right screen
            Column3Width = DualScreenInfo.SpanningBounds[1].Width;
        }
        else
        {
            Column1Width = 100;
            Column2Width = 0;
            Column3Width = 100;
        }

        OnPropertyChanged(nameof(Column1Width));
        OnPropertyChanged(nameof(Column2Width));
        OnPropertyChanged(nameof(Column3Width));

    }

    void OnInfoPropertyChanged(object sender, System.ComponentModel.PropertyChangedEventArgs e)
    {
        UpdateColumns();
    }
}
```

다음 스크린샷은 결과 레이아웃을 보여 줍니다.

![](dual-screen-info-images/grid-on-two-screens.png "Positioning Grid on Two Screens")

## <a name="related-links"></a>관련 링크

- [DualScreen(sample)](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/UserInterface/DualScreenDemos)(DualScreen(샘플))
