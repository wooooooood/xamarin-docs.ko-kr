---
title: 플랫폼별 iOS
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 Xamarin.Forms에 내장 된 iOS 플랫폼별을 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: C0837996-A1E8-47F9-B3A8-98EE43B4A675
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: be378a60a9d9a7b206b64f07ee70edb432cec8e3
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935656"
---
# <a name="ios-platform-specifics"></a>플랫폼별 iOS

_플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 Xamarin.Forms에 내장 된 iOS 플랫폼별을 사용 하는 방법에 설명 합니다._

Xamarin.Forms는 ios의 경우 다음 플랫폼별 포함 됩니다.

- 에 대 한 지원 흐리게 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)합니다. 자세한 내용은 [흐리게 적용](#blur)합니다.
- 페이지 탐색 모음에서 큰 제목으로 페이지 제목이 표시 되는지 여부를 제어 합니다. 자세한 내용은 [큰 제목 표시](#large_title)합니다.
- 콘텐츠 페이지를 확인 합니다. 모든 iOS 장치에 대 한 안전한 화면 영역에 배치 됩니다. 자세한 내용은 [안전 영역 레이아웃 안내선을 사용 하도록 설정 하면](#safe_area_layout)합니다.
- 반투명 탐색 모음입니다. 자세한 내용은 [탐색 표시줄 반투명 하](#translucent_navigation_bar)합니다.
- 제어 상태 표시줄 텍스트의 색 여부는 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 탐색 모음의 광도 맞게 조정 됩니다. 자세한 내용은 [상태 표시줄 텍스트 색 모드를 조정](#status_bar_color_mode)합니다.
- 에 적합 한 텍스트를 입력 하는 보장 된 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 글꼴 크기를 조정 하 여 합니다. 자세한 내용은 [항목의 글꼴 크기를 조정](#adjust_font_size)합니다.
- 선택 항목에서 발생 하는 경우 제어는 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)합니다. 자세한 내용은 [선택 항목 선택 제어](#picker_update_mode)입니다.
- 상태 표시줄 표시 여부 설정 된 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)합니다. 자세한 내용은 [페이지에서 상태 표시줄 표시 여부 설정을](#set_status_bar_visibility)합니다.
- 제어 여부는 [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) 터치 제스처를 처리 하거나 해당 콘텐츠를 전달 합니다. 자세한 내용은 [는 ScrollView에서 콘텐츠 터치 지연](#delay_content_touches)합니다.
- 구분 기호 스타일을 설정 된 [ `ListView` ](xref:Xamarin.Forms.ListView)합니다. 자세한 내용은 [구분 기호 스타일을 ListView에서 설정](#listview-separatorstyle)합니다.
- 지원 되는 레거시 색 모드 사용 안 함 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다. 자세한 내용은 [레거시 색 모드 사용 안 함](#legacy-color-mode)합니다.

<a name="blur" />

## <a name="applying-blur"></a>흐리게 효과 적용합니다.

이 플랫폼별, 아래에 계층화 된 콘텐츠를 흐리게 표시 하는 데 사용 되 고 설정 하 여 XAML에서 사용 되는 [ `VisualElement.BlurEffect` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.BlurEffectProperty/) 연결 된 속성의 값에는 [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) 열거형:

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

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

boxView.On<iOS>().UseBlurEffect(BlurEffectStyle.ExtraLight);
```

`BoxView.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. 합니다 [ `VisualElement.UseBlurEffect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.UseBlurEffect/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스를 사용 하 여 흐린 효과 적용 하는 합니다 [ `BlurEffectStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle) 4 개를 제공 하는 열거형 값: [ `None` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.None), [ `ExtraLight` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.ExtraLight)하십시오 [ `Light` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Light), 및 [ `Dark` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Dark).

이 플랫폼별 탐색 표시줄의 투명도 변경 하는 데 사용 되 고 설정 하 여 XAML에서 사용 되는 [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) 연결 된 속성을 [ 값:

![](ios-images/blur-effect.png "![ ] (ios-images/blur-effect.png " 메서드를를 ")   네임 스페이스 탐색 모음을 반투명 하 하기 위해 사용 됩니다.")

<a name="large_title" />

## <a name="displaying-large-titles"></a>또한 합니다    클래스를  네임 스페이스 역시를    탐색 모음을 기본 상태로 복원 하는 메서드 및    메서드를 호출 하 여 탐색 표시줄 투명도 설정/해제를 사용할 수 있는 합니다    메서드:

결과적으로 탐색 표시줄의 투명도 변경할 수 있습니다. 플랫폼별 반투명 탐색 모음 상태 표시줄 텍스트 색 모드를 조정합니다. 이 플랫폼별 제어 상태 표시줄의 텍스트 색에 있는지 여부를 `NavigationPage.PrefersLargeTitles` `boolean`  탐색 모음의 광도 맞게 조정 됩니다.

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

설정 하 여 XAML에서 사용 되는    연결 된 속성의 값에는    열거형:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

`NavigationPage.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. `NavigationPage.SetPrefersLargeTitle` [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` 메서드는 ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)   네임 스페이스를 제어 상태 표시줄의 텍스트 색에 있는지 여부를    에 맞게 조정 됩니다는 탐색 모음의 명도와 합니다    가능한 두 값을 제공 하는 열거형:

-상태 표시줄 텍스트 색을 변경 하지 말아야 나타냅니다. -상태 표시줄 텍스트 색의 탐색 모음 광도 일치 해야 함을 나타냅니다.

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

또한 합니다    의 현재 값을 검색할 메서드를 사용할 수는    열거형에 적용 되는 합니다   .

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

`Page.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. 결과 상태 표시줄의 텍스트 색을 `Page.SetLargeTitleDisplay` [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` 탐색 모음의 광도 맞게 조정할 수 있습니다.

- `Always` 이 예제에서는 사용자 상태 표시줄 텍스트 색 변경 간에 전환 합니다 `Always`   및    페이지를   :
- `Automatic` 상태 표시줄 텍스트 색 모드 플랫폼별
- `Never` 항목의 글꼴 크기를 조정합니다.

이 플랫폼별의 글꼴 크기를 조정할 되는 `SetLargeTitleDisplay` `LargeTitleDisplay` `LargeTitleDisplayMode` 입력된 텍스트 컨트롤에 맞는지 확인 합니다.

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

설정 하 여 XAML에서 사용 되는 `LargeTitleDisplayMode` [ `Page` 연결 된 속성을 ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 값:

![](ios-images/large-title.png "![ ] (ios-images/large-title.png " 메서드를를 ")   네임 스페이스 탐색 모음을 반투명 하 하기 위해 사용 됩니다.")

<a name="safe_area_layout" />

## <a name="enabling-the-safe-area-layout-guide"></a>  메서드를 합니다    네임 스페이스 맞출 수 있도록 입력된 텍스트의 글꼴 크기를 조정할 수는   .

또한 합니다    클래스를  네임 스페이스 역시를    이 플랫폼별을 사용 하지 않도록 설정 하는 메서드 및    메서드를 호출 하 여 크기 조정 하는 글꼴 크기를 설정/해제를 사용할 수 있는 합니다    메서드: 결과의 글꼴 크기를 `Page.UseSafeArea` `boolean`  크기가 조정 되는 입력된 텍스트 컨트롤에 맞는지 확인:

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

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

`Page.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. 항목 글꼴 크기, 플랫폼별 조정

제어 선택 항목 선택

[![](ios-images/safe-area-layout.png "이 플랫폼 특정 컨트롤에서 항목을 선택할 경우는 [ ![ ], 항목 선택 컨트롤에서 항목을 검색할 때 또는 한 번만 발생 하도록 지정할 수 있도록 허용 합니다 (ios-images/safe-area-layout.png "수행") 단추가 눌러져 있습니다.")

> [!NOTE]
> 설정 하 여 XAML에서 사용 되는 [ 연결 된 속성의 값으로는 `Page.Padding` 열거형:

[ 메서드는 `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) `Page.SafeAreaInsets` 네임 스페이스에는 제어 하는 데 항목을 선택할 경우 사용 하 여를 [ 가능한 두 값을 제공 하는 열거형: – 사용자가 항목을 탐색할 때 항목 선택이 발생 합니다 [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/)합니다.

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

## <a name="making-the-navigation-bar-translucent"></a>이것이 Xamarin.Forms의 기본 동작입니다.

– 항목 선택 눌렀음을 후에 발생 합니다 `NavigationPage.IsNavigationBarTranslucent`수행](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty/) 단추를 `boolean`  합니다.

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                BackgroundColor="Blue"
                ios:NavigationPage.IsNavigationBarTranslucent="true">
  ...
</NavigationPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

(App.Current.MainPage as Xamarin.Forms.NavigationPage).BackgroundColor = Color.Blue;
(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().EnableTranslucentNavigationBar();
```

`NavigationPage.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. 결과 지정한 [ 에 적용 되는 `NavigationPage.EnableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.EnableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) [, 항목을 선택할 경우 제어: 선택기 UpdateMode 플랫폼별

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

상태 표시줄 설정 페이지에 표시 유형

![](ios-images/translucent-navigation-bar.png "이 플랫폼별은 상태 표시줄의 표시 유형을 설정 하는 데 사용 되는 ![ ] (ios-images/translucent-navigation-bar.png ", 상태 표시줄의 진입 하거나 떠납니다 하는 방법을 제어 하는 기능을 포함 하 고 있습니다를 ")입니다.")

<a name="status_bar_color_mode" />

## <a name="adjusting-the-status-bar-text-color-mode"></a>설정 하 여 XAML에서 사용 되는  연결 된 속성의 값을 합니다  열거형 및 필요에 따라 합니다  연결 된 속성의 값을는  열거형:

[ 메서드, 합니다 `NavigationPage` 네임 스페이스는 상태 표시줄의 표시 유형을 설정 하는 데 사용 됩니다는 ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)   중 하나를 지정 하 여를  열거형 값: ,  또는 합니다. 합니다 [ 하 고 `NavigationPage.StatusBarTextColorMode` 장치 방향에 관계 없이 상태 표시줄 표시 여부를 설정 하는 값 및 ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty/) 값 세로로 compact 환경에서 상태 표시줄을 숨깁니다.

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

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

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

`NavigationPage.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. 결과 상태 표시줄의 표시 여부는 [ `NavigationPage.SetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) 설정할 수 있습니다.

- [`DoNotAdjust`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust) 상태 표시줄 표시 플랫폼별
- [`MatchNavigationBarTextLuminosity`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity) 에 [ `MatchNavigationBarTextLuminosity` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity)에 지정 된  열거형 값에는 모든 자식 페이지에 상태 표시줄도 업데이트 됩니다.

다른 모든 [ `GetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.GetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/)-파생 된 형식이 지정된 된 [ 열거형 값에는 현재 페이지의 상태 표시줄만 업데이트 됩니다.

[ 메서드 상태 표시줄의 진입 하거나 떠납니다 어떻게 설정 되는 `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)  중 하나를 지정 하 여는  열거형 값: 를 , 또는 합니다. 경우는 [ 또는 `Master` 애니메이션 실행 상태 표시줄 진입 하거나 떠납니다 0.25 초는 열거형 값을 지정 합니다 ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/)합니다.

![](ios-images/status-bar-text-color-mode.png "지연 콘텐츠 터치를 ScrollView에서")

<a name="adjust_font_size" />

## <a name="adjusting-the-font-size-of-an-entry"></a>암시적 타이머를 터치 제스처 시작 되 면 트리거됩니다를    ios 및  타이머 범위 내의 사용자 동작을 기준으로 제스처 처리 또는 해당 콘텐츠를 전달 해야 하는지 여부를 결정 합니다.

기본적으로 iOS [ 지연 콘텐츠 터치를 하지만 사용 하 여 일부 상황에서 문제를 일으킬 수이 있습니다는 `Entry` 하지 말아야 할 때 제스처를 최우선 콘텐츠입니다. 따라서이 플랫폼별 제어 하는지 여부를 [ 터치 제스처를 처리 하거나 해당 콘텐츠를 전달 합니다.

```xaml
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

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

entry.On<iOS>().EnableAdjustsFontSizeToFitWidth();
```

`Entry.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. [ 메서드, 합니다 `Entry.EnableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.EnableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) [ 네임 스페이스에는 컨트롤에 있는지 여부를 `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) [ 터치 제스처를 처리 하거나 해당 콘텐츠를 전달 합니다. 또한 합니다 [ 메서드를 호출 하 여 콘텐츠 터치 지연 설정/해제를 사용할 수는 `Entry` 콘텐츠 터치 지연 되 고 있는지 여부를 반환 하는 방법.

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

결과 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 지연 되므로 콘텐츠 터치를 수신 사용 하지 않도록 설정할 수 있습니다이 시나리오에는    제스처를 받는 대신    페이지의   :

![](ios-images/entry-font-size.png "ScrollView 지연 콘텐츠 건드리면 플랫폼별")

<a name="picker_update_mode" />

## <a name="controlling-picker-item-selection"></a>구분 기호 스타일을 ListView에서 설정

이 플랫폼별 사이의 구분 기호 셀에 있는지 여부를 제어를 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 의 전체 너비를 사용 하는 **합니다. `Picker.UpdateMode` `UpdateMode`  메서드는    사이의 구분 기호 셀에 있는지 여부를 네임 스페이스를 제어 하는 합니다    전체를 사용 하 여 너비를 를 사용 하 여 합니다    가능한 두 값을 제공 하는 열거형:

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

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`Picker.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. – 기본 iOS 구분 동작을 나타냅니다.

- `Immediately` -구분 기호의 한쪽 가장자리에서 그려짐을 나타냅니다는 ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 다른 합니다. 결과 지정한    값이 적용 되는   , 셀 사이 있는 구분선의 너비를 제어 하는:
- `WhenFinished` ListView SeparatorStyle 플랫폼별

이 플랫폼별의 글꼴 크기를 조정할 되는 `SetUpdateMode` `UpdateMode` `UpdateMode` 입력된 텍스트 컨트롤에 맞는지 확인 합니다.

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

구분 기호 스타일 설정한 후 `UpdateMode`를 다시 변경할 수 없습니다 [ 런타임 시.

[![](ios-images/picker-updatemode.png "레거시 색 모드를 사용 하지 않도록 설정")](ios-images/picker-updatemode-large.png#lightbox "Picker UpdateMode Plaform-Specific")

<a name="set_status_bar_visibility" />

## <a name="setting-the-status-bar-visibility-on-a-page"></a>Xamarin.Forms 뷰의 일부 레거시 색 모드를 기능입니다.

이 모드에서는 때 합니다 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 뷰의 속성 `Page`, 보기에는 색 사용 안 함된 상태에 대 한 기본 네이티브 색을 사용 하 여 사용자 설정 보다 우선 합니다. 이전 버전과 호환성을 위해이 레거시 색 모드는 지원 되는 보기에 대 한 기본 동작을 유지 합니다.

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

`Page.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. 이 플랫폼별 뷰를 사용 하지 않도록 설정 하는 경우에 사용자가 설정한 뷰에 색 유지 되도록이 레거시 색 모드를 해제 합니다. 설정 하 여 XAML에서 사용 되는 `StatusBarHiddenMode.True` `StatusBarHiddenMode.False` `StatusBarHiddenMode.Default` 연결 된 속성을 :

합니다 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 메서드는    레거시 색 모드가 비활성화 되어 있는지 여부를 제어 하려면 네임 스페이스는 합니다.

![](ios-images/hide-status-bar.png "또한 합니다 ![ ] (ios-images/hide-status-bar.png " 메서드를 사용 하 여 레거시 색 모드를 비활성화 되었는지 여부를 반환할 수 있습니다.")

> [!NOTE]
> 결과 보기를 비활성화 하는 경우 사용자가 설정한 뷰에 색도 계속 되도록 레거시 색 모드를 비활성화할 수 있습니다. 레거시 색 모드 사용 안 함

설정 하는 경우는 `Page.SetPreferredStatusBarUpdateAnimation` [ `Page` 레거시 색 모드는 완전히 뷰에서 무시 됩니다. 시각적 상태에 대 한 자세한 내용은 참조 하세요. `Fade`은 Xamarin.Forms Visual State Manager`Slide`합니다.

<a name="delay_content_touches" />

## <a name="delaying-content-touches-in-a-scrollview"></a>이 문서에서는 Xamarin.Forms에 내장 된 iOS 플랫폼별을 사용 하는 방법을 보여 줍니다.

PlatformSpecifics (샘플) 기본적으로 iOS `ScrollView` 지연 콘텐츠 터치를 하지만 사용 하 여 일부 상황에서 문제를 일으킬 수이 있습니다는 `ScrollView` 하지 말아야 할 때 제스처를 최우선 콘텐츠입니다. 따라서이 플랫폼별 제어 하는지 여부를 `ScrollView` 터치 제스처를 처리 하거나 해당 콘텐츠를 전달 합니다. 이 플랫폼별 제어 상태 표시줄의 텍스트 색에 있는지 여부를 `ScrollView.ShouldDelayContentTouches` `boolean`  탐색 모음의 광도 맞게 조정 됩니다.

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

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

`ScrollView.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. `ScrollView.SetShouldDelayContentTouches` 메서드, 합니다 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) 네임 스페이스에는 컨트롤에 있는지 여부를 [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) 터치 제스처를 처리 하거나 해당 콘텐츠를 전달 합니다. 또한 합니다 `SetShouldDelayContentTouches` 메서드를 호출 하 여 콘텐츠 터치 지연 설정/해제를 사용할 수는 `ShouldDelayContentTouches` 콘텐츠 터치 지연 되 고 있는지 여부를 반환 하는 방법.

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

결과 [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) 지연 되므로 콘텐츠 터치를 수신 사용 하지 않도록 설정할 수 있습니다이 시나리오에는 [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) 제스처를 받는 대신 [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) 페이지의 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

[![](ios-images/scrollview-delay-content-touches.png "ScrollView 지연 콘텐츠 건드리면 플랫폼별")](ios-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView Delay Content Touches Plaform-Specific")

<a name="listview-separatorstyle" />

## <a name="setting-the-separator-style-on-a-listview"></a>구분 기호 스타일을 ListView에서 설정

이 플랫폼별 사이의 구분 기호 셀에 있는지 여부를 제어를 [ `ListView` ](xref:Xamarin.Forms.ListView) 의 전체 너비를 사용 하는 `ListView`합니다. 합니다 [ 하 고 `ListView.SeparatorStyle` 장치 방향에 관계 없이 상태 표시줄 표시 여부를 설정 하는 값 및 ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SeparatorStyleProperty) 값 세로로 compact 환경에서 상태 표시줄을 숨깁니다.

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.SeparatorStyle="FullWidth">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetSeparatorStyle(SeparatorStyle.FullWidth);
```

`ListView.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. [ `ListView.SetSeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SetSeparatorStyle(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 사이의 구분 기호 셀에 있는지 여부를 네임 스페이스를 제어 하는 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 전체를 사용 하 여 너비를 `ListView`를 사용 하 여 합니다 [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) 가능한 두 값을 제공 하는 열거형:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.Default) – 기본 iOS 구분 동작을 나타냅니다. 결과 지정한    값이 적용 되는   , 셀 사이 있는 구분선의 너비를 제어 하는:
- [`FullWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.FullWidth) -구분 기호의 한쪽 가장자리에서 그려짐을 나타냅니다는 `ListView` 다른 합니다.

결과 지정한 [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) 값이 적용 되는 [ `ListView` ](xref:Xamarin.Forms.ListView), 셀 사이 있는 구분선의 너비를 제어 하는:

![](ios-images/listview-separatorstyle.png "ListView SeparatorStyle 플랫폼별")

> [!NOTE]
> 구분 기호 스타일 설정한 후 `FullWidth`를 다시 변경할 수 없습니다 `Default` 런타임 시.

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>레거시 색 모드를 사용 하지 않도록 설정

Xamarin.Forms 뷰의 일부 레거시 색 모드를 기능입니다. 이 모드에서는 때 합니다 [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) 뷰의 속성 `false`, 보기에는 색 사용 안 함된 상태에 대 한 기본 네이티브 색을 사용 하 여 사용자 설정 보다 우선 합니다. 이전 버전과 호환성을 위해이 레거시 색 모드는 지원 되는 보기에 대 한 기본 동작을 유지 합니다.

이 플랫폼별 뷰를 사용 하지 않도록 설정 하는 경우에 사용자가 설정한 뷰에 색 유지 되도록이 레거시 색 모드를 해제 합니다. 설정 하 여 XAML에서 사용 되는 [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.IsLegacyColorModeEnabledProperty) 연결 된 속성을 `false`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                ios:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

_legacyColorModeDisabledButton.On<iOS>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. 합니다 [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Boolean)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 레거시 색 모드가 비활성화 되어 있는지 여부를 제어 하려면 네임 스페이스는 합니다. 또한 합니다 [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})) 메서드를 사용 하 여 레거시 색 모드를 비활성화 되었는지 여부를 반환할 수 있습니다.

결과 보기를 비활성화 하는 경우 사용자가 설정한 뷰에 색도 계속 되도록 레거시 색 모드를 비활성화할 수 있습니다.

![](ios-images/legacy-color-mode-disabled.png "레거시 색 모드 사용 안 함")

> [!NOTE]
> 설정 하는 경우는 [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) 레거시 색 모드는 완전히 뷰에서 무시 됩니다. 시각적 상태에 대 한 자세한 내용은 참조 하세요. [은 Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)합니다.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Forms에 내장 된 iOS 플랫폼별을 사용 하는 방법을 보여 줍니다. 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [iOSSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)
