---
title: "Android 플랫폼-세부 사항"
description: "플랫폼 비슷하므로 허용 사용자 지정 렌더러 또는 효과 구현 하지 않고도 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 Android 플랫폼-세부 사항 Xamarin.Forms에 기본 제공 되는 사용 하는 방법을 보여줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: C5D4AA65-9BAA-4008-8A1E-36CDB78A435D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/17/2017
ms.openlocfilehash: e26fcb81bb99e5a49d16731777171b4efa4163c6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="android-platform-specifics"></a>Android 플랫폼-세부 사항

_플랫폼 비슷하므로 허용 사용자 지정 렌더러 또는 효과 구현 하지 않고도 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 Android 플랫폼-세부 사항 Xamarin.Forms에 기본 제공 되는 사용 하는 방법을 보여줍니다._

Android에서는 Xamarin.Forms 다음 플랫폼-세부 정보가 들어 있습니다.

- 소프트 키보드의 운영 모드를 설정 합니다. 자세한 내용은 참조 [소프트 키보드 입력 모드 설정](#soft_input_mode)합니다.
- 빠른 스크롤을 사용 하도록 설정 된 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 자세한 내용은 참조 [ListView에서 빠른 스크롤을 사용 하도록 설정](#fastscroll)합니다.
- 넘기기가에서 페이지 사이 사용 하도록 설정 된 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)합니다. 자세한 내용은 참조 [활성화 넘기기가 사이에서 페이지는 TabbedPage](#enable_swipe_paging)합니다.
- Z-순서 제어 시각적 요소 그리기 순서를 결정 합니다. 자세한 내용은 참조 [시각적 요소 상승 제어](#elevation)합니다.
- 사용 하지 않도록 설정 된 [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) 및 [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) 일시 중지에 수명 주기 이벤트 페이지 하 고 각각 AppCompat를 사용 하는 응용 프로그램에 대 한 다시 시작 합니다. 자세한 내용은 참조 [Disappearing 및 페이지 수명 주기 이벤트가 표시 해제](#disable_lifecycle_events)합니다.

<a name="soft_input_mode" />

## <a name="setting-the-soft-keyboard-input-mode"></a>소프트 키보드 입력된 모드를 설정

이 플랫폼별 소프트 키보드 입력된 영역에 대 한 운영 모드를 설정 하는 데 사용 되 고 설정 하 여 XAML에서 사용 되는 [ `Application.WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty/) 연결 된 속성의 값에는 [ `WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) 열거형:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize">
  ...
</Application>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

App.Current.On<Android>().UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize);
```

`Application.On<Android>` 메서드 지정이 플랫폼별 Android에만 실행 됩니다. [ `Application.UseWindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.UseWindowSoftInputModeAdjust/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) 네임 스페이스는 소프트 키보드 입력된 영역 운영 모드를 사용 하 여 설정 하는 데 사용 되는 [ `WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) 두 값을 제공 하는 열거형: [ `Pan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan/) 및 [ `Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/)합니다. `Pan` 사용 하 여 값의 [ `AdjustPan` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustPan/) 입력된 컨트롤에 포커스가 있을 때 창 크기가 조정 되지 않는 조정 옵션을 합니다. 대신, 창의 내용은 이동 하는 현재 포커스 소프트 키보드에 의해 가려지는 되도록 합니다. `Resize` 사용 하 여 값의 [ `AdjustResize` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustResize/) 입력된 컨트롤에 포커스가 있으면 소프트 키보드에 대 한 공간을 만들기 위해 창 크기를 조정 하는 조정 옵션입니다.

결과 소프트 키보드 입력 입력된 컨트롤에 포커스가 있을 때 운영 모드를 설정할 수 있습니다는 영역을 사용 하는.

[![](android-images/pan-resize.png "운영 모드 플랫폼별 소프트 키보드")](android-images/pan-resize-large.png "Soft Keyboard Operating Mode Plaform-Specific")

<a name="fastscroll" />

## <a name="enabling-fast-scrolling-in-a-listview"></a>ListView에서 빠른 스크롤을 사용 하도록 설정

이 플랫폼별은 빠른 데이터를 통해 스크롤할 수 있게 하는 데 사용 되는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)합니다. 설정 하 여 XAML에서 사용 되는 `ListView.IsFastScrollEnabled` 연결 된 속성을는 `boolean` 값:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        ...
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  GroupDisplayBinding="{Binding Key}"
                  IsGroupingEnabled="true"
                  android:ListView.IsFastScrollEnabled="true">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var listView = new Xamarin.Forms.ListView { IsGroupingEnabled = true, ... };
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "GroupedEmployees");
listView.GroupDisplayBinding = new Binding("Key");
listView.On<Android>().SetIsFastScrollEnabled(true);
```

`ListView.On<Android>` 메서드 지정이 플랫폼별 Android에만 실행 됩니다. `ListView.SetIsFastScrollEnabled` 메서드는 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) 네임 스페이스, 데이터를 통해 빠른 스크롤할 수 있도록 사용 됩니다는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)합니다. 또한는 `SetIsFastScrollEnabled` 메서드를 호출 하 여 빠른 스크롤 설정/해제를 사용할 수는 `IsFastScrollEnabled` 빠른 스크롤을 사용할지 여부를 반환 하는 메서드:

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

결과 데이터를 통해 빠른 스크롤 하는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 스크롤 위치 조정 컨트롤의 크기 변경 내용을 사용할 수 있습니다.

[![](android-images/fastscroll.png "ListView FastScroll 플랫폼별")](android-images/fastscroll-large.png "ListView FastScroll Plaform-Specific")

<a name="enable_swipe_paging" />

## <a name="enabling-swiping-between-pages-in-a-tabbedpage"></a>넘기기가 TabbedPage에서 페이지 사이 사용 하도록 설정

이 플랫폼별 넘기기가에 페이지 간의 가로 손가락 제스처와 사용할 수 있도록 사용 되는 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)합니다. 설정 하 여 XAML에서 사용 되는 [ `TabbedPage.IsSwipePagingEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty/) 연결 된 속성을는 `boolean` 값:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

`TabbedPage.On<Android>` 메서드 지정이 플랫폼별 Android에만 실행 됩니다. [ `TabbedPage.SetIsSwipePagingEnabled` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetIsSwipePagingEnabled/p/Xamarin.Forms.BindableObject/System.Boolean/) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) 네임 스페이스를 사용 하는 페이지 사이의 넘기기가 활성화는 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)합니다. 또한는 `TabbedPage` 클래스에 `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` 네임 스페이스에는 [ `EnableSwipePaging` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.EnableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/) 이 플랫폼별 있는 메서드 및 [ `DisableSwipePaging` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.DisableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/) 비활성화 하는 메서드 이 플랫폼별 합니다. [ `TabbedPage.OffscreenPageLimit` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty/) 연결 된 속성 및 [ `SetOffscreenPageLimit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetOffscreenPageLimit/p/Xamarin.Forms.BindableObject/System.Int32/) 메서드를 현재 페이지의 어느 쪽에 유휴 상태에 유지 해야 하는 페이지의 수를 설정 하는 데 사용 됩니다.

결과 해당 살짝 페이징 작업을 하 여 표시 되는 페이지는 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) 사용 됨:

![](android-images/tabbedpage-swipe.png)

<a name="elevation" />

## <a name="controlling-the-elevation-of-visual-elements"></a>상승의 경우 시각적 요소를 제어합니다.

이 플랫폼별 크거나 API 21를 대상으로 하는 권한 상승 또는 응용 프로그램에 대 한 시각적 요소의 Z 순서를 제어 하는 데 사용 합니다. 시각적 요소 상승 Z 값이 높은 Z 값이 낮은 시각적 요소 occluding 시각적 요소와의 그리기 순서를 결정 합니다. 설정 하 여 XAML에서 사용 되는 `Elevation.Elevation` 연결 된 속성을는 `boolean` 값:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             Title="Elevation">
    <StackLayout>
        <Grid>
            <Button Text="Button Beneath BoxView" />
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>        
        <Grid Margin="0,20,0,0">
            <Button Text="Button Above BoxView - Click Me" android:Elevation.Elevation="10"/>
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

public class AndroidElevationPageCS : ContentPage
{
    public AndroidElevationPageCS()
    {
        ...
        var aboveButton = new Button { Text = "Button Above BoxView - Click Me" };
        aboveButton.On<Android>().SetElevation(10);

        Content = new StackLayout
        {
            Children =
            {
                new Grid
                {
                    Children =
                    {
                        new Button { Text = "Button Beneath BoxView" },
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                },
                new Grid
                {
                    Margin = new Thickness(0,20,0,0),
                    Children =
                    {
                        aboveButton,
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                }
            }
        };
    }
}
```

`Button.On<Android>` 메서드 지정이 플랫폼별 Android에만 실행 됩니다. `Elevation.SetElevation` 메서드는 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) 네임 스페이스에는 시각적 요소의 null 허용으로 설정 하는 데 사용 됩니다 `float`합니다. 또한는 `Elevation.GetElevation` 시각적 요소 상승 값을 검색할 메서드를 사용할 수 있습니다.

결과 상승의 경우 시각적 요소의 Z 값이 높은 시각적 요소 채워집니다 Z 값이 낮은 시각적 요소를 제어할 수 있습니다. 따라서이 예제에서에서 두 번째 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) 위에 렌더링 되는 [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) 상승 값이 높을수록 있기 때문에:

![](android-images/elevation.png)

<a name="disable_lifecycle_events" />

## <a name="disabling-the-disappearing-and-appearing-page-lifecycle-events"></a>소멸 / 페이지 수명 주기 이벤트를 사용 하지 않도록 설정

이 플랫폼별은 사용 하지 않도록 설정 하는 데 사용 되는 [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) 및 [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) 페이지 이벤트 응용 프로그램에 일시 중지 하 고 각각 AppCompat를 사용 하는 응용 프로그램에 대 한 다시 시작 합니다. 소프트 키보드 일시 중지에 표시 된 소프트 키보드의 운영 모드 설정 되어 있는 경우 다시 시작할 때 표시 되는지 여부를 제어 하는 기능 포함 하는 또한 [ `WindowSoftInputModeAdjust.Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/)합니다.

> [!NOTE]
> Note 이벤트를 사용 하는 응용 프로그램에 대 한 기존 동작을 유지 하기 위해 이러한 이벤트는 기본적으로 사용 합니다. 이러한 이벤트를 사용 하지 않도록 설정 하면 이전 AppCompat 이벤트 주기가 일치 AppCompat 이벤트 주기가 있습니다.

이 플랫폼별으로 설정 하 여 XAML에서 사용 될 수는 [ `Application.SendDisappearingEventOnPause` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPauseProperty/), [ `Application.SendAppearingEventOnResume` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResumeProperty/), 및 [ `Application.ShouldPreserveKeyboardOnResume` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResumeProperty/) 에연결된속성`boolean` 값:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"             xmlns:androidAppCompat="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize"
             androidAppCompat:Application.SendDisappearingEventOnPause="false"
             androidAppCompat:Application.SendAppearingEventOnResume="false"
             androidAppCompat:Application.ShouldPreserveKeyboardOnResume="true">
  ...
</Application>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

Xamarin.Forms.Application.Current.On<Android>()
     .UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize)
     .SendDisappearingEventOnPause(false)
     .SendAppearingEventOnResume(false)
     .ShouldPreserveKeyboardOnResume(true);
```

`Application.Current.On<Android>` 메서드 지정이 플랫폼별 Android에만 실행 됩니다. [ `Application.SendDisappearingEventOnPause` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPause/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/) 네임 스페이스, 하는 데 사용 하도록 설정 하거나 발생 하지 않도록 설정는 [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) 페이지 이벤트, 응용 프로그램 백그라운드를 입력합니다. [ `Application.SendAppearingEventOnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) 메서드 실행을 사용할지를 사용 하는 [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) 이벤트 페이지 백그라운드에서 응용 프로그램 다시 시작 합니다. [ `Application.ShouldPreserveKeyboardOnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) 메서드 사용 제어 소프트 키보드 일시 중지에 표시 된 경우 다시 시작할 때 표시 되는지 여부를 제공 소프트 키보드의 운영 모드로 설정 되어 있는지 [ `WindowSoftInputModeAdjust.Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/).

결과 [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) 및 [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) 페이지 이벤트는 응용 프로그램 일시 중지에 발생 하지 않습니다 하 고 각각 다시 시작 되었으며 되는 경우 소프트 키보드 선택할 때 표시 된 응용 프로그램 가 일시 중지 된 것도 표시 됩니다는 응용 프로그램 다시 시작 될 때:

[![](android-images/keyboard-on-resume.png "수명 주기 이벤트 플랫폼별")](android-images/keyboard-on-resume-large.png "수명 주기 이벤트 플랫폼별")

## <a name="summary"></a>요약

이 문서에는 Android 플랫폼-세부 사항 Xamarin.Forms에 기본 제공 되는 사용 하는 방법을 보여 줍니다. 플랫폼 비슷하므로 허용 사용자 지정 렌더러 또는 효과 구현 하지 않고도 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [플랫폼 특성 만들기](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [AndroidSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)
- [AndroidSpecific.AppCompat](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/)
