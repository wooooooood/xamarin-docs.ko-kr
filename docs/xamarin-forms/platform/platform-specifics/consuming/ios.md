---
title: iOS 플랫폼 세부 사항
description: 플랫폼 비슷하므로 허용 사용자 지정 렌더러 또는 효과 구현 하지 않고도 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 Xamarin.Forms에 기본 제공 되는 iOS 플랫폼 특성을 사용 하는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: C0837996-A1E8-47F9-B3A8-98EE43B4A675
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/16/2017
ms.openlocfilehash: 7826962cd3bf9595a63841e3f2d9fb377d1a0574
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="ios-platform-specifics"></a>iOS 플랫폼 세부 사항

_플랫폼 비슷하므로 허용 사용자 지정 렌더러 또는 효과 구현 하지 않고도 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 Xamarin.Forms에 기본 제공 되는 iOS 플랫폼 특성을 사용 하는 방법을 보여줍니다._

Ios, Xamarin.Forms 다음 플랫폼-세부 정보가 들어 있습니다.

- 에 대 한 지원 흐림 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)합니다. 자세한 내용은 참조 [흐림 효과 적용](#blur)합니다.
- 페이지 제목 페이지 탐색 모음에서 큰 제목으로 표시 되는지 여부를 제어 합니다. 자세한 내용은 참조 [큰 제목 표시](#large_title)합니다.
- 해당 페이지 콘텐츠를 확인 하 여 모든 iOS 장치에 대 한 화면 영역에 배치 됩니다. 자세한 내용은 참조 [안전 영역 레이아웃 지침을 활성화](#safe_area_layout)합니다.
- 반투명 탐색 모음입니다. 자세한 내용은 참조 [탐색 모음 반투명 만드는](#translucent_navigation_bar)합니다.
- 제어 상태 표시줄의 텍스트에 색 여부는 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 의 탐색 모음 광도 일치 하도록 조정 됩니다. 자세한 내용은 참조 [조정 상태 표시줄 텍스트 색 모드](#status_bar_color_mode)합니다.
- 에 적합 한지 확인 하는 텍스트를 입력 하면는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 글꼴 크기를 조정 하 여 합니다. 자세한 내용은 참조 [항목의 글꼴 크기를 조정](#adjust_font_size)합니다.
- 항목 선택에서 발생 하는 경우 제어는 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)합니다. 자세한 내용은 참조 [선택 항목 선택 제어](#picker_update_mode)합니다.
- 상태 표시줄 표시 여부에 설정 된 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)합니다. 자세한 내용은 참조 [페이지에서 상태 표시줄 표시 유형을 설정](#set_status_bar_visibility)합니다.
- 제어 여부는 [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) 터치 제스처를 처리 하거나 해당 내용에 전달 합니다. 자세한 내용은 참조 [를 ScrollView에 지연 콘텐츠 터치](#delay_content_touches)합니다.

<a name="blur" />

## <a name="applying-blur"></a>흐림 효과 적용합니다.

이 플랫폼별, 아래에 계층화 된 콘텐츠를 흐리게 하는 데 사용 되 고 설정 하 여 XAML에서 사용 되는 [ `VisualElement.BlurEffect` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.BlurEffectProperty/) 연결 된 속성의 값에는 [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) 열거형:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
  ...
  <AbsoluteLayout HorizontalOptions="Center">
    <Image Source="monkeyface.png" />
    <BoxView x:Name="boxView" ios:VisualElement.BlurEffect="ExtraLight" HeightRequest="300" WidthRequest="300" />
  </AbsoluteLayout>
  ...
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

boxView.On<iOS>().UseBlurEffect(BlurEffectStyle.ExtraLight);
```

`BoxView.On<iOS>` 메서드 지정이 플랫폼별 iOS에만 실행 됩니다. [ `VisualElement.UseBlurEffect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.UseBlurEffect/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) 메서드에는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) 네임 스페이스는 많은 흐림 효과 적용 하는 데 사용 되는 [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) 4 개를 제공 하는 열거형 값: [ `None` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.None/), [ `ExtraLight` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.ExtraLight/), [ `Light` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Light/), 및 [ `Dark` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Dark/)합니다.

결과 지정 된 [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) 에 적용 되는 [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) 인스턴스는 흐림은 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 그 아래의 계층:

![](ios-images/blur-effect.png "흐림 효과 플랫폼별")

<a name="large_title" />

## <a name="displaying-large-titles"></a>큰 제목 표시

이 플랫폼별 페이지 제목을 11 이상이 iOS를 사용 하는 장치에 대 한 탐색 모음에서 큰 제목으로 표시 하는 데 사용 됩니다. 큰 왼쪽 맞춤 하 고 큰 글꼴을 사용 하 여 제목과 화면 공간을 효율적으로 사용 되도록 사용자 지정 콘텐츠를 스크롤을 시작할 때 표준 제목으로 전환 합니다. 그러나, 가로 방향에서 제목 콘텐츠 레이아웃을 최적화 하기 위해 탐색 모음의 가운데에 반환 됩니다. 설정 하 여 XAML에서 사용 되는 `NavigationPage.PrefersLargeTitles` 연결 된 속성을는 `boolean` 값:

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

`NavigationPage.On<iOS>` 메서드 지정이 플랫폼별 iOS에만 실행 됩니다. `NavigationPage.SetPrefersLargeTitle` 메서드는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) 네임 스페이스를 큰 제목 사용 되는지 여부를 제어 합니다.

큰 제목에 설정 되어 있는 경우는 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), 탐색 스택의 모든 페이지에는 큰 제목 표시 됩니다. 설정 하 여 페이지에이 동작을 재정의할 수 있습니다는 `Page.LargeTitleDisplay` 연결 된 속성의 값에는 `LargeTitleDisplayMode` 열거형:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 페이지 동작을 재정의할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSLargeTitlePageCS : ContentPage
{
    public iOSLargeTitlePageCS(ICommand restore)
    {
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        ...
    }
    ...
}
```

`Page.On<iOS>` 메서드 지정이 플랫폼별 iOS에만 실행 됩니다. `Page.SetLargeTitleDisplay` 메서드는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) 에 큰 제목 동작을 제어 하는 네임 스페이스는 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)와 `LargeTitleDisplayMode` 세 가지 가능한을 제공 하는 열거형 값:

- `Always` – 탐색 모음 및 글꼴 강제로 큰 형식을 사용 하 여 크기입니다.
- `Automatic` – (크거나 작은) 같은 스타일 탐색 스택의에서 이전 항목으로 사용 합니다.
- `Never` – 보통, 작은 형식 탐색 모음을 사용 합니다.

또한는 `SetLargeTitleDisplay` 메서드를 호출 하 여 열거 값을 설정/해제를 사용할 수는 `LargeTitleDisplay` 현재를 반환 하는 메서드 `LargeTitleDisplayMode`:

```csharp
switch (On<iOS>().LargeTitleDisplay())
{
    case LargeTitleDisplayMode.Always:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Automatic);
        break;
    case LargeTitleDisplayMode.Automatic:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        break;
    case LargeTitleDisplayMode.Never:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Always);
        break;
}
```

결과 지정 된 `LargeTitleDisplayMode` 에 적용 되는 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)는 큰 제목 동작을 제어:

![](ios-images/large-title.png "흐림 효과 플랫폼별")

<a name="safe_area_layout" />

## <a name="enabling-the-safe-area-layout-guide"></a>안전 하 게 보호 영역 레이아웃 가이드를 사용 하도록 설정

이 플랫폼별 11 이상에 iOS를 사용 하는 모든 장치에 대 한 안전한 화면 영역 페이지 콘텐츠가 배치 되었는지 확인 하는 데 사용 됩니다. 있는지 확인 하는 데 도움이 됩니다 특히 모서리가 둥근된 장치, 홈 표시기 또는 센서 주택 X iPhone에서 해당 콘텐츠가 클리핑 되지 않습니다. 설정 하 여 XAML에서 사용 되는 `Page.UseSafeArea` 연결 된 속성을는 `boolean` 값:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Safe Area"
             ios:Page.UseSafeArea="true">
    <StackLayout>
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

`Page.On<iOS>` 메서드 지정이 플랫폼별 iOS에만 실행 됩니다. `Page.SetUseSafeArea` 메서드는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) 네임 스페이스를 안전 하 게 보호 영역 레이아웃 지침이 활성화 되어 있는지 여부를 제어 합니다.

결과 페이지 콘텐츠를 모든 Iphone 안전 하 게 되는 화면 영역에 배치 될 수 있습니다.

[![](ios-images/safe-area-layout.png "보호 영역 레이아웃 가이드")](ios-images/safe-area-layout-large.png#lightbox "보호 영역 레이아웃 가이드")

> [!NOTE]
> Apple에 의해 정의 되는 안전 하 게 보호 영역 Xamarin.Forms에는 데 사용 되는 [ `Page.Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/) 속성을이 속성의 설정 된 이전 값을 재정의 합니다.

안전 하 게 보호 영역을 검색 하 여 사용자 지정할 수 있습니다는 [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) 값과 `Page.SafeAreaInsets` 에서 메서드는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) 네임 스페이스입니다. 다음으로 수정할 수 있습니다에 다시 할당 하 고 필요한는 `Padding` 페이지 생성자에서 속성 또는 [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) 재정의:

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    var safeInsets = On<iOS>().SafeAreaInsets();
    safeInsets.Left = 20;
    Padding = safeInsets;
}
```

<a name="translucent_navigation_bar" />

## <a name="making-the-navigation-bar-translucent"></a>탐색 모음 반투명 만들기

이 플랫폼별 탐색 모음의의 투명도 변경 하는 데 사용 되 고 설정 하 여 XAML에서 사용 되는 [ `NavigationPage.IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty/) 연결 된 속성을는 `boolean` 값:

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                BackgroundColor="Blue"
                ios:NavigationPage.IsNavigationBarTranslucent="true">
  ...
</NavigationPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

(App.Current.MainPage as Xamarin.Forms.NavigationPage).BackgroundColor = Color.Blue;
(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().EnableTranslucentNavigationBar();
```

`NavigationPage.On<iOS>` 메서드 지정이 플랫폼별 iOS에만 실행 됩니다. [ `NavigationPage.EnableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.EnableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) 반투명 탐색 모음을 만들려면 네임 스페이스에 사용 됩니다. 또한는 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage/) 클래스에 `Xamarin.Forms.PlatformConfiguration.iOSSpecific` 네임 스페이스에는 [ `DisableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.DisableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) 탐색 모음을 기본 상태로 복원 하는 메서드 및 [ `SetIsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetIsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/System.Boolean/) 호출 하 여 탐색 모음 투명도 설정/해제 하는 데 사용할 수 있는 메서드는 [ `IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) 메서드:

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

결과 탐색 모음의의 투명도 변경할 수 있습니다.

![](ios-images/translucent-navigation-bar.png "플랫폼별 반투명 탐색 모음")

<a name="status_bar_color_mode" />

## <a name="adjusting-the-status-bar-text-color-mode"></a>상태 표시줄 텍스트 색 모드를 조정합니다.

이 플랫폼별 제어 상태 표시줄의 텍스트에 색 여부는 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 의 탐색 모음 광도 일치 하도록 조정 됩니다. 설정 하 여 XAML에서 사용 되는 [ `NavigationPage.StatusBarTextColorMode` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty/) 연결 된 속성의 값에는 [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) 열거형:

```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    x:Class="PlatformSpecifics.iOSStatusBarTextColorModePage">
    <MasterDetailPage.Master>
        <ContentPage Title="Master Page Title" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <NavigationPage BarBackgroundColor="Blue" BarTextColor="White"
                        ios:NavigationPage.StatusBarTextColorMode="MatchNavigationBarTextLuminosity">
            <x:Arguments>
                <ContentPage>
                    <Label Text="Slide the master page to see the status bar text color mode change." />
                </ContentPage>
            </x:Arguments>
        </NavigationPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>

```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

IsPresentedChanged += (sender, e) =>
{
    var mdp = sender as MasterDetailPage;
    if (mdp.IsPresented)
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.DoNotAdjust);
    else
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.MatchNavigationBarTextLuminosity);
};
```

`NavigationPage.On<iOS>` 메서드 지정이 플랫폼별 iOS에만 실행 됩니다. [ `NavigationPage.SetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) 메서드에는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) 네임 스페이스, 컨트롤에서 상태 표시줄의 텍스트 색 여부는 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 일치 하도록 조정 됩니다는 탐색 모음 광도와 [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) 두 개의 가능한 값을 제공 하는 열거형.

- [`DoNotAdjust`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust/) -상태 표시줄 텍스트 색을 변경 하지 말아야 나타냅니다.
- [`MatchNavigationBarTextLuminosity`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity/) -상태 표시줄 텍스트 색의 탐색 모음 광도 일치 해야 함을 나타냅니다.

또한는 [ `GetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.GetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/) 의 현재 값을 검색할 메서드를 사용할 수는 [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) 에 적용 되는 열거형은 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/).

결과 상태 표시줄에서 텍스트 색을 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 의 탐색 모음 광도 맞게 조정할 수 있습니다. 이 예제에서는 사용자로 상태 표시줄 텍스트 색 변경 간의 전환에서 [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) 및 [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) 의 페이지는 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

![](ios-images/status-bar-text-color-mode.png "상태 표시줄 텍스트 색 모드 플랫폼별")

<a name="adjust_font_size" />

## <a name="adjusting-the-font-size-of-an-entry"></a>항목의 글꼴 크기를 조정합니다.

이 플랫폼별은의 글꼴 크기를 확장 하는 데 사용 되는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 입력된 텍스트 컨트롤에 맞는지 되도록 합니다. 설정 하 여 XAML에서 사용 되는 [ `Entry.AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty/) 연결 된 속성을는 `boolean` 값:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    <StackLayout Margin="20">
        <Entry x:Name="entry"
               Placeholder="Enter text here to see the font size change"
               FontSize="22"
               ios:Entry.AdjustsFontSizeToFitWidth="true" />
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

entry.On<iOS>().EnableAdjustsFontSizeToFitWidth();
```

`Entry.On<iOS>` 메서드 지정이 플랫폼별 iOS에만 실행 됩니다. [ `Entry.EnableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.EnableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) 네임 스페이스, 크기 맞출 수 있도록 입력된 텍스트의 글꼴 크기를 조정 하는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/). 또한는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry/) 클래스에 `Xamarin.Forms.PlatformConfiguration.iOSSpecific` 네임 스페이스에는 [ `DisableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.DisableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) 이 플랫폼별을 비활성화 하는 메서드 및 [ `SetAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/System.Boolean/) 호출 하 여 크기 조정 하는 글꼴 크기를 설정/해제 하는 데 사용할 수 있는 메서드는 [ `AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) 메서드:

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

결과의 글꼴 크기를 제공 하는는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 입력된 텍스트 컨트롤에 맞는지 확인을 위해 크기가 조정 됩니다.

![](ios-images/entry-font-size.png "항목 글꼴 크기 플랫폼별 조정")

<a name="picker_update_mode" />

## <a name="controlling-picker-item-selection"></a>선택 항목 선택 제어

이 플랫폼별 제어 항목 선택에서 발생 하는 경우는 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), 항목 선택 컨트롤에서 항목을 검색할 때 또는 한 번만 발생 하도록 지정 하는 **수행** 추가 합니다. 설정 하 여 XAML에서 사용 되는 `Picker.UpdateMode` 연결 된 속성의 값에는 `UpdateMode` 열거형:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <Picker ... Title="Select a monkey" ios:Picker.UpdateMode="WhenFinished">
          ...
        </Picker>
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`Picker.On<iOS>` 메서드 지정이 플랫폼별 iOS에만 실행 됩니다. `Picker.SetUpdateMode` 메서드에는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) 네임 스페이스, 항목 선택 발생 하는 시기를 제어 하는와 `UpdateMode` 두 개의 가능한 값을 제공 하는 열거형.

- `Immediately` – 항목 선택/시간이 사용자가 항목에 탐색으로 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)합니다. 이것이 Xamarin.Forms에 기본 동작입니다.
- `WhenFinished` – 눌렀음이 되 면 선택 항목에만 발생는 **수행** 단추는 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)합니다.

또한는 `SetUpdateMode` 메서드를 호출 하 여 열거 값을 설정/해제를 사용할 수는 `UpdateMode` 현재를 반환 하는 메서드 `UpdateMode`:

```csharp
switch (picker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        picker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

결과 지정 된 `UpdateMode` 에 적용 되는 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), 항목 선택 시기를 제어 합니다.

[![](ios-images/picker-updatemode.png "선택기 UpdateMode 플랫폼별")](ios-images/picker-updatemode-large.png#lightbox "Picker UpdateMode Plaform-Specific")

<a name="set_status_bar_visibility" />

## <a name="setting-the-status-bar-visibility-on-a-page"></a>상태 표시줄 설정 페이지에 표시 유형

이 플랫폼별은에서 상태 표시줄의 표시 유형을 설정 하는 데 사용 되는 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), 상태 표시줄 들어오거나 나갈 제어 하는 기능이 포함 된 `Page`합니다. 설정 하 여 XAML에서 사용 되는 `Page.PrefersStatusBarHidden` 연결 된 속성의 값에는 `StatusBarHiddenMode` 열거형 및 필요에 따라는 `Page.PreferredStatusBarUpdateAnimation` 연결 된 속성의 값에는 `UIStatusBarAnimation` 열거형:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

`Page.On<iOS>` 메서드 지정이 플랫폼별 iOS에만 실행 됩니다. `Page.SetPrefersStatusBarHidden` 메서드에는 `Xamarin.Forms.PlatformConfiguration.iOSSpecific` 네임 스페이스를 사용 하에서 상태 표시줄의 표시 여부를 설정 하는 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 중 하나를 지정 하 여는 `StatusBarHiddenMode` 열거형 값: `Default`, `True` 또는 `False`합니다. `StatusBarHiddenMode.True` 및 `StatusBarHiddenMode.False` 장치 방향에 관계 없이 상태 표시줄 표시 여부를 설정 하는 값 및 `StatusBarHiddenMode.Default` 값 세로로 compact 환경에서 상태 표시줄을 숨깁니다.

결과 상태 표시줄을 표시 하는 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 설정할 수 있습니다.

![](ios-images/hide-status-bar.png "상태 표시줄 표시 유형 플랫폼별")

> [!NOTE]
> 에 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), 지정 된 `StatusBarHiddenMode` 열거형 값에는 모든 자식 페이지에 상태 표시줄도 업데이트 됩니다. 다른 모든 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-파생 된 형식이 지정된 된 `StatusBarHiddenMode` 열거형 값은 현재 페이지의 상태 표시줄 업데이트 됩니다.

`Page.SetPreferredStatusBarUpdateAnimation` 메서드를 사용 하는 상태 표시줄 들어오거나 나갈를 설정 하는 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 중 하나를 지정 하 여는 `UIStatusBarAnimation` 열거형 값: `None`, `Fade`, 또는 `Slide`합니다. 경우는 `Fade` 또는 `Slide` 열거형 값이 지정 되는 상태 표시줄에 들어가거나 같은 방식으로 애니메이션 실행 0.25 초당은 `Page`합니다.

<a name="delay_content_touches" />

## <a name="delaying-content-touches-in-a-scrollview"></a>지연 콘텐츠 터치를 ScrollView에

암시적 타이머 터치 제스처에 시작 될 때 트리거되는 [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) iOS에서 및 `ScrollView` 제스처 처리 또는 해당 콘텐츠를 전달 해야 타이머 범위 내에서 사용자 작업에 따라 결정 합니다. 기본적으로 iOS `ScrollView` 지연 콘텐츠 터치 하지만 문제가 있는 경우에 발생할 수이 있습니다는 `ScrollView` 하지 말아야 할 때 제스처를 우세한 콘텐츠입니다. 따라서이 플랫폼별 제어 여부는 `ScrollView` 터치 제스처를 처리 하거나 해당 내용에 전달 합니다. 설정 하 여 XAML에서 사용 되는 `ScrollView.ShouldDelayContentTouches` 연결 된 속성을는 `boolean` 값:

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <MasterDetailPage.Master>
        <ContentPage Title="Menu" BackgroundColor="Blue" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <ContentPage>
            <ScrollView x:Name="scrollView" ios:ScrollView.ShouldDelayContentTouches="false">
                <StackLayout Margin="0,20">
                    <Slider />
                    <Button Text="Toggle ScrollView DelayContentTouches" Clicked="OnButtonClicked" />
                </StackLayout>
            </ScrollView>
        </ContentPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

`ScrollView.On<iOS>` 메서드 지정이 플랫폼별 iOS에만 실행 됩니다. `ScrollView.SetShouldDelayContentTouches` 메서드에는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) 네임 스페이스, 하는 데 제어 하는지 여부는 [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) 터치 제스처를 처리 하거나 해당 내용에 전달 합니다. 또한는 `SetShouldDelayContentTouches` 메서드를 호출 하 여 콘텐츠 작업을 지연 설정/해제 데 사용할 수는 `ShouldDelayContentTouches` 콘텐츠 터치 지연 되 고 있는지 여부를 반환 하는 메서드:

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

결과 [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) 콘텐츠 터치 하므로 수신 연기를 비활성화할 수는이 시나리오에서는 [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) 제스처를 받는 보다는 [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) 의 페이지는 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

[![](ios-images/scrollview-delay-content-touches.png "ScrollView 지연 콘텐츠 플랫폼별와 연결")](ios-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView Delay Content Touches Plaform-Specific")

## <a name="summary"></a>요약

이 문서 Xamarin.Forms에 기본 제공 되는 iOS 플랫폼 특성을 사용 하는 방법을 보여 줍니다. 플랫폼 비슷하므로 허용 사용자 지정 렌더러 또는 효과 구현 하지 않고도 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [iOSSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)
