---
title: Android 플랫폼-세부 사항
description: 플랫폼 비슷하므로 허용 사용자 지정 렌더러 또는 효과 구현 하지 않고도 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 Android 플랫폼-세부 사항 Xamarin.Forms에 기본 제공 되는 사용 하는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: C5D4AA65-9BAA-4008-8A1E-36CDB78A435D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: 705c3037505b61bfb364772187e0ce8ead14b27f
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847773"
---
# <a name="android-platform-specifics"></a>Android 플랫폼-세부 사항

_플랫폼 비슷하므로 허용 사용자 지정 렌더러 또는 효과 구현 하지 않고도 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 Android 플랫폼-세부 사항 Xamarin.Forms에 기본 제공 되는 사용 하는 방법을 보여줍니다._

Android에서는 Xamarin.Forms 다음 플랫폼-세부 정보가 들어 있습니다.

- 소프트 키보드의 운영 모드를 설정 합니다. 자세한 내용은 참조 [소프트 키보드 입력 모드 설정](#soft_input_mode)합니다.
- 빠른 스크롤을 사용 하도록 설정 된 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 자세한 내용은 참조 [ListView에서 빠른 스크롤을 사용 하도록 설정](#fastscroll)합니다.
- 넘기기가에서 페이지 사이 사용 하도록 설정 된 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)합니다. 자세한 내용은 참조 [활성화 넘기기가 사이에서 페이지는 TabbedPage](#enable_swipe_paging)합니다.
- Z-순서 제어 시각적 요소 그리기 순서를 결정 합니다. 자세한 내용은 참조 [시각적 요소 상승 제어](#elevation)합니다.
- 사용 하지 않도록 설정 된 [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) 및 [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) 일시 중지에 수명 주기 이벤트 페이지 하 고 각각 AppCompat를 사용 하는 응용 프로그램에 대 한 다시 시작 합니다. 자세한 내용은 참조 [Disappearing 및 페이지 수명 주기 이벤트가 표시 해제](#disable_lifecycle_events)합니다.
- 제어 여부는 [ `WebView` ](xref:Xamarin.Forms.WebView) 혼합 된 콘텐츠를 표시할 수 있습니다. 자세한 내용은 참조 [혼합 된 콘텐츠 보기에서 사용 하도록 설정](#webview-mixed-content)합니다.
- 입력된 방법에 대 한 소프트 키보드에 대 한 편집기 옵션 설정는 [ `Entry` ](xref:Xamarin.Forms.Entry)합니다. 자세한 내용은 참조 [설정 항목 입력 방법 편집기 옵션](#entry-imeoptions)합니다.
- 지원 되는 레거시 색 모드를 해제 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다. 자세한 내용은 참조 [레거시 색 모드 해제](#legacy-color-mode)합니다.
- 기본 안쪽 여백은 Android 단추의 섀도 값을 사용 합니다. 자세한 내용은 참조 [Android 단추를 사용 하 여](#button-padding-shadow)합니다.

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

[![](android-images/pan-resize.png "운영 모드 플랫폼별 소프트 키보드")](android-images/pan-resize-large.png#lightbox "Soft Keyboard Operating Mode Plaform-Specific")

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

[![](android-images/fastscroll.png "ListView FastScroll 플랫폼별")](android-images/fastscroll-large.png#lightbox "ListView FastScroll Plaform-Specific")

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

이 플랫폼별 크거나 API 21를 대상으로 하는 권한 상승 또는 응용 프로그램에 대 한 시각적 요소의 Z 순서를 제어 하는 데 사용 합니다. 시각적 요소 상승 Z 값이 높은 Z 값이 낮은 시각적 요소 occluding 시각적 요소와의 그리기 순서를 결정 합니다. 설정 하 여 XAML에서 사용 되는 `VisualElement.Elevation` 연결 된 속성을는 `boolean` 값:

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
            <Button Text="Button Above BoxView - Click Me" android:VisualElement.Elevation="10"/>
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

`Button.On<Android>` 메서드 지정이 플랫폼별 Android에만 실행 됩니다. `VisualElement.SetElevation` 메서드는 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) 네임 스페이스에는 시각적 요소의 null 허용으로 설정 하는 데 사용 됩니다 `float`합니다. 또한는 `VisualElement.GetElevation` 시각적 요소 상승 값을 검색할 메서드를 사용할 수 있습니다.

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

[![](android-images/keyboard-on-resume.png "수명 주기 이벤트 플랫폼별")](android-images/keyboard-on-resume-large.png#lightbox "수명 주기 이벤트 플랫폼별")

<a name="webview-mixed-content" />

## <a name="enabling-mixed-content-in-a-webview"></a>보기에서 혼합 된 콘텐츠를 사용 하도록 설정

이 플랫폼별 제어 여부는 [ `WebView` ](xref:Xamarin.Forms.WebView) 수 표시 혼합 된 콘텐츠로 응용 프로그램에서 API 21 이상를 대상으로 하는 합니다. 혼합된 콘텐츠는 HTTPS 연결을 통해 처음 로드 된 하지만 HTTP 연결을 통해 리소스 (예: 이미지, 오디오, 비디오, 스타일 시트, 스크립트)를 로드 하는 콘텐츠입니다. 설정 하 여 XAML에서 사용 되는 [ `WebView.MixedContentMode` ](x:ref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) 연결 된 속성의 값에는 [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) 열거형:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

`WebView.On<Android>` 메서드 지정이 플랫폼별 Android에만 실행 됩니다. [ `WebView.SetMixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.SetMixedContentMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.WebView},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 혼합 된 콘텐츠를 표시할 수 있는 네임 스페이스 제어에 사용 되기와 [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) 세 가지 가능한 값을 제공 하는 열거형:

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) – 나타냅니다는 [ `WebView` ](xref:Xamarin.Forms.WebView) HTTPS origin HTTP 원본에서 콘텐츠를 로드할 수 있습니다.
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) – 나타냅니다는 [ `WebView` ](xref:Xamarin.Forms.WebView) HTTPS origin HTTP 원본에서 콘텐츠를 로드할 수 없습니다.
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) – 나타냅니다는 [ `WebView` ](xref:Xamarin.Forms.WebView) 최신 장치 웹 브라우저의 접근 방식을과 호환 가능 하도록 시도 합니다. HTTPS origin에서 로드할 일부 HTTP 콘텐츠를 사용할 수 있습니다 및 다른 콘텐츠 형식의 차단 됩니다. 차단 또는 허용 되는 콘텐츠 유형의 각 운영 체제 릴리스에서 변경 될 수 있습니다.

결과 지정 된 [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) 값에 적용 됩니다는 [ `WebView` ](xref:Xamarin.Forms.WebView)를 혼합 된 콘텐츠를 표시할 수 있는지 여부를 제어 합니다.

[![WebView 혼합 콘텐츠 처리 플랫폼별](android-images/webview-mixedcontent.png "WebView 혼합 콘텐츠 처리 플랫폼별")](android-images/webview-mixedcontent-large.png#lightbox "WebView 혼합 콘텐츠 처리 플랫폼별")

<a name="entry-imeoptions" />

## <a name="setting-entry-input-method-editor-options"></a>설정 항목 입력 방법 편집기 옵션

이 플랫폼별 입력된 방법에 대 한 소프트 키보드에 대 한 (입력기) 옵션을 설정는 [ `Entry` ](xref:Xamarin.Forms.Entry)합니다. 사용자 작업 단추는 소프트 키보드와의 상호 작용 아래에 설정 된 `Entry`합니다. 설정 하 여 XAML에서 사용 되는 [ `Entry.ImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.ImeOptionsProperty) 연결 된 속성의 값에는 [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) 열거형:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Entry ... android:Entry.ImeOptions="Send" />
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

entry.On<Android>().SetImeOptions(ImeFlags.Send);
```

`Entry.On<Android>` 메서드 지정이 플랫폼별 Android에만 실행 됩니다. [ `Entry.SetImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.SetImeOptions(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Entry},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스에 대 한 소프트 키보드에 대 한 입력된 방법 작업 옵션을 설정 하는 [ `Entry` ](xref:Xamarin.Forms.Entry), 와 [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) 다음 값을 제공 하는 열거 합니다.

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Default) – 나타냅니다는 특정 동작 키가 필요 하며, 수 있는 내부 컨트롤 자체를 생성 합니다. 이거나 `Next` 또는 `Done`합니다.
- [`None`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.None) – 작업 키는 사용할 수 있도록 나타냅니다.
- [`Go`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Go) – 동작 키 "이동" 작업을 수행 합니다, 텍스트의 대상으로 지정할 사용자 라인 때 입력 한을 나타냅니다.
- [`Search`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Search) – "검색" 연산을 수행 하는 동작 키, 라인 텍스트 검색의 결과를 사용자 입력을 나타냅니다.
- [`Send`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Send) – 동작 키가 대상에 텍스트를 제공 하는 "send" 작업을 수행 하는 것을 나타냅니다.
- [`Next`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Next) – 나타냅니다 동작 키 텍스트를 허용 하는 다음 필드에 사용자를 수행 하는 "다음" 작업을 수행 합니다.
- [`Done`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Done) – 나타냅니다 동작 키 소프트 키보드 닫는 "완료" 작업을 수행 합니다.
- [`Previous`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Previous) – 나타냅니다 동작 키 텍스트를 허용 하는 이전 필드에 사용자를 수행 하는 "이전" 작업을 수행 합니다.
- [`ImeMaskAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.ImeMaskAction) – 작업 옵션을 선택 하는 마스크입니다.
- [`NoPersonalizedLearning`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoPersonalizedLearning) –는 맞춤법 검사기는 사용자에서 배울 아니고 올바른 사용자가 이전에 입력 한 내용에 따라 값을 제안 나타냅니다.
- [`NoFullscreen`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoFullscreen) – UI 해야 전체 화면 올리지 나타냅니다.
- [`NoExtractUi`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoExtractUi) – 추출 된 텍스트에 대 한 UI가 표시를 나타냅니다.
- [`NoAccessoryAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoAccessoryAction) – 사용자 지정 작업에 대 한 UI 표시 됨을 나타냅니다.

결과 지정 된 [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) 값에 대 한 소프트 키보드에 적용 되는 [ `Entry` ](xref:Xamarin.Forms.Entry), 입력된 방법 편집기 옵션 설정:

[![항목이 입력 방법 편집기 플랫폼별](android-images/entry-imeoptions.png "항목 입력 방법 편집기 플랫폼별")](android-images/entry-imeoptions-large.png#lightbox "항목 입력 방법 편집기 플랫폼별")

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>레거시 색 모드를 사용 하지 않도록 설정

일부 Xamarin.Forms 뷰 기능을 레거시 색 모드. 이 모드에서는 때는 [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) 보기의 속성이로 설정 되어 `false`, 보기에는 사용할 수 없는 상태에 대 한 기본 네이티브 색을 사용 하 여 사용자가 설정한 색 보다 우선 합니다. 이전 버전과 호환성을이 레거시 색 모드의 기본 동작을 지원 되는 뷰 상태가 유지 됩니다.

이 플랫폼별 보기를 사용 하지 않도록 설정 하는 경우에 사용자가 설정한 뷰에 색 유지 되도록이 레거시 색 모드를 해제 합니다. 설정 하 여 XAML에서 사용 되는 [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.IsLegacyColorModeEnabledProperty) 연결 된 속성을 `false`:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                android:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

_legacyColorModeDisabledButton.On<Android>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Android>` 메서드 지정이 플랫폼별 Android에만 실행 됩니다. [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement},System.Boolean)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스, 레거시 색 모드 사용 되지 않는지 여부를 제어에 사용 됩니다. 또한는 [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement})) 메서드를 사용 하 여 레거시 색 모드 되지 않는지 여부를 반환할 수 있습니다.

결과 보기 사용 하지 않는 경우 사용자가 설정한 뷰에 색도 계속 되도록 레거시 색 모드를 비활성화할 수 있습니다.

![](android-images/legacy-color-mode-disabled.png "레거시 색 모드 사용 안 함")

> [!NOTE]
> 설정할 때는 [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) 레거시 모드는 완전히 보기에서는 무시 됩니다. 시각적 상태에 대 한 자세한 내용은 참조 [The Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)합니다.

<a name="button-padding-shadow" />

## <a name="using-android-buttons"></a>Android 단추 사용

이 플랫폼별 Xamarin.Forms 단추 기본 안쪽 여백 및 Android 단추의 그림자의 값을 사용 하는지 여부를 제어 합니다. 설정 하 여 XAML에서 사용 되는 [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPaddingProperty) 및 [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadowProperty) 연결 된 속성을 `boolean` 값:

```xaml
<ContentPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button ...
                android:Button.UseDefaultPadding="true"
                android:Button.UseDefaultShadow="true" />         
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

button.On<Android>().SetUseDefaultPadding(true).SetUseDefaultShadow(true);
```

`Button.On<Android>` 메서드 지정이 플랫폼별 Android에만 실행 됩니다. [ `Button.SetUseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) 및[ `Button.SetUseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스에 기본값을 사용 하 여 Xamarin.Forms 단추가 있는지 여부를 제어 하는 데 사용 됩니다 안쪽 여백 및 Android 단추의 그림자의 값입니다. 또한는 [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) 및 [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) 메서드를 사용 하 여 단추 기본 각각 값과 기본값 그림자를 안쪽 사용 하는지 여부를 반환할 수 있습니다.

결과 기본 안쪽 여백 및 Android 단추의 그림자 값 Xamarin.Forms 단추를 사용할 수 있습니다.

![](android-images/button-padding-and-shadow.png "레거시 색 모드 사용 안 함")

위에 스크린샷에서 사항에 유의 [ `Button` ](xref:Xamarin.Forms.Button) 점을 제외 하 고 동일한 정의 갖추고 오른쪽 `Button` 기본 안쪽 여백 및 Android 단추의 섀도 값을 사용 합니다.

## <a name="summary"></a>요약

이 문서에는 Android 플랫폼-세부 사항 Xamarin.Forms에 기본 제공 되는 사용 하는 방법을 보여 줍니다. 플랫폼 비슷하므로 허용 사용자 지정 렌더러 또는 효과 구현 하지 않고도 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [AndroidSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)
- [AndroidSpecific.AppCompat](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/)
