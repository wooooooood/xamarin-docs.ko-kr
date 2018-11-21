---
title: 플랫폼별 iOS
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 Xamarin.Forms에 내장 된 iOS 플랫폼별을 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: C0837996-A1E8-47F9-B3A8-98EE43B4A675
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 12fd9e477e24058d36128e52b7b5dd9074598be8
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171796"
---
# <a name="ios-platform-specifics"></a>플랫폼별 iOS

_플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 Xamarin.Forms에 내장 된 iOS 플랫폼별을 사용 하는 방법에 설명 합니다._

## <a name="visualelements"></a>VisualElements

IOS에서 Xamarin.Forms 뷰, 페이지 및 레이아웃에 대해 다음 플랫폼 특정 기능을 제공 됩니다.

- 에 대 한 지원 흐리게 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다. 자세한 내용은 [흐리게 적용](#blur)합니다.
- 지원 되는 레거시 색 모드 사용 안 함 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다. 자세한 내용은 [레거시 색 모드 사용 안 함](#legacy-color-mode)합니다.
- 에 그림자를 사용 하도록 설정 된 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다. 자세한 내용은 [그림자를 사용 하도록 설정 하면](#drop-shadow)합니다.

<a name="blur" />

### <a name="applying-blur"></a>흐리게 효과 적용합니다.

이 플랫폼별, 아래에 계층화 된 콘텐츠를 흐리게 표시 하는 데 사용 되 고 설정 하 여 XAML에서 사용 되는 [ `VisualElement.BlurEffect` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.BlurEffectProperty) 연결 된 속성의 값에는 [ `BlurEffectStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle) 열거형:

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

`BoxView.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. 합니다 [ `VisualElement.UseBlurEffect` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.UseBlurEffect(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스를 사용 하 여 흐린 효과 적용 하는 합니다 [ `BlurEffectStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle) 4 개를 제공 하는 열거형 값: [ `None` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.None), [ `ExtraLight` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.ExtraLight)하십시오 [ `Light` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Light), 및 [ `Dark` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Dark).

결과 지정 된 [ `BlurEffectStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle) 에 적용 되는 [ `BoxView` ](xref:Xamarin.Forms.BoxView) 인스턴스에 흐림 합니다 [ `Image` ](xref:Xamarin.Forms.Image) 그 아래에 계층화 된:

![](ios-images/blur-effect.png "효과가 플랫폼별 흐리게 표시")

> [!NOTE]
> 흐림 효과를 추가 하는 경우는 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)에서 터치 이벤트를 수신 여전히는 `VisualElement`합니다.

<a name="legacy-color-mode" />

### <a name="disabling-legacy-color-mode"></a>레거시 색 모드를 사용 하지 않도록 설정

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

<a name="drop-shadow" />

### <a name="enabling-a-drop-shadow"></a>그림자를 사용 하도록 설정

이 플랫폼별에서 그림자를 사용 하도록 설정 되는 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다. 설정 하 여 XAML에서 사용 되는 [ `VisualElement.IsShadowEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.IsShadowEnabledProperty) 연결 된 속성을 `true`, 연결 된 그림자를 제어 하는 속성을 비롯 하 여 많은 추가 선택 사항:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <BoxView ...
                 ios:VisualElement.IsShadowEnabled="true"
                 ios:VisualElement.ShadowColor="Purple"
                 ios:VisualElement.ShadowOpacity="0.7"
                 ios:VisualElement.ShadowRadius="12">
            <ios:VisualElement.ShadowOffset>
                <Size>
                    <x:Arguments>
                        <x:Double>10</x:Double>
                        <x:Double>10</x:Double>
                    </x:Arguments>
                </Size>
            </ios:VisualElement.ShadowOffset>
         </BoxView>
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var boxView = new BoxView { Color = Color.Aqua, WidthRequest = 100, HeightRequest = 100 };
boxView.On<iOS>()
       .SetIsShadowEnabled(true)
       .SetShadowColor(Color.Purple)
       .SetShadowOffset(new Size(10,10))
       .SetShadowOpacity(0.7)
       .SetShadowRadius(12);
```

`VisualElement.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. 합니다 [ `VisualElement.SetIsShadowEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetIsShadowEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Boolean)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 그림자에 사용 되는지 여부를 제어 하려면 네임 스페이스는는 `VisualElement`합니다. 또한 그림자를 제어 하려면 다음 메서드를 호출할 수 있습니다.

- [`SetShadowColor`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetShadowColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},Xamarin.Forms.Color)) – 그림자의 색을 설정 합니다. 기본 색은 [ `Color.Default` ](xref:Xamarin.Forms.Color.Default*)합니다.
- [`SetShadowOffset`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetShadowOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},Xamarin.Forms.Size)) – 그림자의 오프셋을 설정 합니다. 그림자 캐스팅 된 및로 지정 된 방향을 변경 하는 오프셋 된 [ `Size` ](xref:Xamarin.Forms.Size) 값입니다. `Size` 구조 값의 첫 번째 값 (음수) 왼쪽 또는 오른쪽 (양수) 까지의 거리와 두 번째 되 위의 거리 값 (음수 값) 또는 (양수) 아래 장치 독립적 단위 표현 됩니다 . 이 속성의 기본값은 (0.0, 0.0)의 모든 관련 캐스팅은 섀도 있으며 그 결과 `VisualElement`합니다.
- [`SetShadowOpacity`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetShadowOpacity(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Double)) –는 범위의 0.0 (투명) 1.0 (불투명) 값을 사용 하 여 그림자의 불투명도 설정 합니다. 기본 불투명도 값은 0.5입니다.
- [`SetShadowRadius`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetShadowRadius(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Double)) – 그림자를 렌더링 하는 데 흐리게 반경이 설정 합니다. Radius 기본값은 10.0입니다.

> [!NOTE]
> 호출 하 여 그림자의 상태를 쿼리할 수 있습니다 합니다 [ `GetIsShadowEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetIsShadowEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}))를 [ `GetShadowColor` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetShadowColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}))를 [ `GetShadowOffset` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetShadowOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}))를 [ `GetShadowOpacity` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetShadowOpacity(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})), 및 [ `GetShadowRadius` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetShadowRadius(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})) 메서드.

결과에서 그림자를 사용할 수 있습니다는 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement):

![](ios-images/drop-shadow.png "사용 하도록 설정 하는 그림자")

## <a name="views"></a>보기

IOS에서 Xamarin.Forms 보기에 대 한 다음과 같은 플랫폼별 기능 제공 됩니다.

- 에 적합 한 텍스트를 입력 하는 보장 된 [ `Entry` ](xref:Xamarin.Forms.Entry) 글꼴 크기를 조정 하 여 합니다. 자세한 내용은 [항목의 글꼴 크기를 조정](#adjust_font_size)합니다.
- 커서 색 설정 된 [ `Entry` ](xref:Xamarin.Forms.Entry)합니다. 자세한 내용은 [항목이 커서 색 설정](#entry-cursorcolor)합니다.
- 구분 기호 스타일을 설정 된 [ `ListView` ](xref:Xamarin.Forms.ListView)합니다. 자세한 내용은 [구분 기호 스타일을 ListView에서 설정](#listview-separatorstyle)합니다.
- 선택 항목에서 발생 하는 경우 제어는 [ `Picker` ](xref:Xamarin.Forms.Picker)합니다. 자세한 내용은 [선택 항목 선택 제어](#picker_update_mode)입니다.
- 사용 하도록 설정 합니다 [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) 속성에는 위치에 탭 하 여 설정 될를 [ `Slider` ](xref:Xamarin.Forms.Slider) 끌어 필요가 하는 대신 가로 막대형,는 `Slider` thumb. 자세한 내용은 [탭으로 이동 하는 슬라이더 위치 조정 컨트롤을 사용 하도록 설정 하면](#slider-updateontap)합니다.

<a name="adjust_font_size" />

### <a name="adjusting-the-font-size-of-an-entry"></a>항목의 글꼴 크기를 조정합니다.

이 플랫폼별의 글꼴 크기를 조정할 되는 [ `Entry` ](xref:Xamarin.Forms.Entry) 입력된 텍스트 컨트롤에 맞는지 확인 합니다. 설정 하 여 XAML에서 사용 되는 [ `Entry.AdjustsFontSizeToFitWidth` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty) 연결 된 속성을 `boolean` 값:

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

`Entry.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. [ `Entry.EnableAdjustsFontSizeToFitWidth` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.EnableAdjustsFontSizeToFitWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry})) 메서드를 합니다 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스 맞출 수 있도록 입력된 텍스트의 글꼴 크기를 조정할 수는 [ `Entry` ](xref:Xamarin.Forms.Entry). 또한 합니다 [ `Entry` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry) 클래스를 `Xamarin.Forms.PlatformConfiguration.iOSSpecific` 네임 스페이스 역시를 [ `DisableAdjustsFontSizeToFitWidth` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.DisableAdjustsFontSizeToFitWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry})) 이 플랫폼별을 사용 하지 않도록 설정 하는 메서드 및 [ `SetAdjustsFontSizeToFitWidth` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetAdjustsFontSizeToFitWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry},System.Boolean)) 메서드를 호출 하 여 크기 조정 하는 글꼴 크기를 설정/해제를 사용할 수 있는 합니다 [ `AdjustsFontSizeToFitWidth` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry})) 메서드:

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

결과의 글꼴 크기를 [ `Entry` ](xref:Xamarin.Forms.Entry) 크기가 조정 되는 입력된 텍스트 컨트롤에 맞는지 확인:

![](ios-images/entry-font-size.png "항목 글꼴 크기, 플랫폼별 조정")

<a name="entry-cursorcolor" />

### <a name="setting-the-entry-cursor-color"></a>항목 커서 색 설정

커서 색을 설정 하는이 플랫폼별을 [ `Entry` ](xref:Xamarin.Forms.Entry) 지정 된 색입니다. 설정 하 여 XAML에서 사용 되는 [ `Entry.CursorColor` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.CursorColorProperty) 바인딩 가능한 속성을 [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Entry ... ios:Entry.CursorColor="LimeGreen" />
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var entry = new Xamarin.Forms.Entry();
entry.On<iOS>().SetCursorColor(Color.LimeGreen);
```

`Entry.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. [ `Entry.SetCursorColor` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetCursorColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry},Xamarin.Forms.Color)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 커서 색 지정을 설정 하는 네임 스페이스 [ `Color` ](xref:Xamarin.Forms.Color)합니다. 또한 합니다 [ `Entry.GetCursorColor` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.GetCursorColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry})) 메서드를 사용 하 여 현재 커서 색을 검색할 수 있습니다.

결과 커서에서 색인을 [ `Entry` ](xref:Xamarin.Forms.Entry) 특정으로 설정할 수 있습니다 [ `Color` ](xref:Xamarin.Forms.Color):

![](ios-images/entry-cursorcolor.png "커서 색 항목")

<a name="listview-separatorstyle" />

### <a name="setting-the-separator-style-on-a-listview"></a>구분 기호 스타일을 ListView에서 설정

이 플랫폼별 사이의 구분 기호 셀에 있는지 여부를 제어를 [ `ListView` ](xref:Xamarin.Forms.ListView) 의 전체 너비를 사용 하는 `ListView`합니다. 설정 하 여 XAML에서 사용 되는 [ `ListView.SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SeparatorStyleProperty) 연결 된 속성의 값에는 [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) 열거형:

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

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.Default) – 기본 iOS 구분 동작을 나타냅니다. 이것이 Xamarin.Forms의 기본 동작입니다.
- [`FullWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.FullWidth) -구분 기호의 한쪽 가장자리에서 그려짐을 나타냅니다는 `ListView` 다른 합니다.

결과 지정한 [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) 값이 적용 되는 [ `ListView` ](xref:Xamarin.Forms.ListView), 셀 사이 있는 구분선의 너비를 제어 하는:

![](ios-images/listview-separatorstyle.png "ListView SeparatorStyle 플랫폼별")

> [!NOTE]
> 구분 기호 스타일 설정한 후 `FullWidth`를 다시 변경할 수 없습니다 `Default` 런타임 시.

<a name="picker_update_mode" />

### <a name="controlling-picker-item-selection"></a>제어 선택 항목 선택

이 플랫폼 특정 컨트롤에서 항목을 선택할 경우는 [ `Picker` ](xref:Xamarin.Forms.Picker), 항목 선택 컨트롤에서 항목을 검색할 때 또는 한 번만 발생 하도록 지정할 수 있도록 허용 합니다 **수행** 단추가 눌러져 있습니다. 설정 하 여 XAML에서 사용 되는 `Picker.UpdateMode` 연결 된 속성의 값으로는 `UpdateMode` 열거형:

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

`Picker.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. `Picker.SetUpdateMode` 메서드는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스에는 제어 하는 데 항목을 선택할 경우 사용 하 여를 `UpdateMode` 가능한 두 값을 제공 하는 열거형:

- `Immediately` – 사용자가 항목을 탐색할 때 항목 선택이 발생 합니다 [ `Picker` ](xref:Xamarin.Forms.Picker)합니다. 이것이 Xamarin.Forms의 기본 동작입니다.
- `WhenFinished` – 항목 선택 눌렀음을 후에 발생 합니다 **수행** 단추를 [ `Picker` ](xref:Xamarin.Forms.Picker)합니다.

또한 합니다 `SetUpdateMode` 메서드를 호출 하 여 열거형 값을 설정/해제를 사용할 수는 `UpdateMode` 현재 반환 하는 메서드 `UpdateMode`:

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

결과 지정한 `UpdateMode` 에 적용 되는 [ `Picker` ](xref:Xamarin.Forms.Picker), 항목을 선택할 경우 제어:

[![](ios-images/picker-updatemode.png "선택기 UpdateMode 플랫폼별")](ios-images/picker-updatemode-large.png#lightbox "선택기 UpdateMode 플랫폼별")

<a name="slider-updateontap" />

### <a name="enabling-a-slider-thumb-to-move-on-tap"></a>탭으로 이동 하는 슬라이더 위치 조정 컨트롤을 사용 하도록 설정

이 플랫폼별 사용 하도록 설정 합니다 [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) 속성에는 위치에 탭 하 여 설정 될를 [ `Slider` ](xref:Xamarin.Forms.Slider) 끌어 필요가 하는 대신 가로 막대형,는 `Slider` thumb. 설정 하 여 XAML에서 사용 되는 [ `Slider.UpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.UpdateOnTapProperty) 바인딩 가능한 속성을 `true`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Slider ... ios:Slider.UpdateOnTap="true" />
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var slider = new Xamarin.Forms.Slider();
slider.On<iOS>().SetUpdateOnTap(true);
```

`Slider.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. [ `Slider.SetUpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.SetUpdateOnTap(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Slider},System.Boolean)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 여부 탭 컨트롤에 네임 스페이스는를 `Slider` 표시줄 설정를 [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) 속성입니다. 또한 합니다 [ `Slider.GetUpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.GetUpdateOnTap(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Slider})) 메서드를 사용 하 여 반환할 수 있습니다 탭 여부를 `Slider` 표시줄 설정 됩니다는 `Slider.Value` 속성.

결과 탭 합니다 [ `Slider` ](xref:Xamarin.Forms.Slider) 모음 이동할 수는 `Slider` thumb 및 설정 합니다 [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) 속성:

![](ios-images/slider-updateontap.png "사용 하도록 설정 하는 탭에 대 한 슬라이더 업데이트")

## <a name="pages"></a>인쇄할 페이지

IOS에서 Xamarin.Forms 페이지에 대 한 다음과 같은 플랫폼별 기능 제공 됩니다.

- 탐색 모음의 구분 기호를 숨기는 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)합니다. 자세한 내용은 [탐색 표시줄 구분 기호는 NavigationPage 숨기기](#navigationpage-hideseparatorbar)합니다.
- 탐색 모음 반투명 인지 여부를 제어 합니다. 자세한 내용은 [탐색 표시줄 반투명 하](#translucent_navigation_bar)합니다.
- 제어 상태 표시줄 텍스트의 색 여부는 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 탐색 모음의 광도 맞게 조정 됩니다. 자세한 내용은 [상태 표시줄 텍스트 색 모드를 조정](#status_bar_color_mode)합니다.
- 페이지 탐색 모음에서 큰 제목으로 페이지 제목이 표시 되는지 여부를 제어 합니다. 자세한 내용은 [큰 제목 표시](#large_title)합니다.
- 상태 표시줄 표시 여부 설정 된 [ `Page` ](xref:Xamarin.Forms.Page)합니다. 자세한 내용은 [페이지에서 상태 표시줄 표시 여부 설정을](#set_status_bar_visibility)합니다.
- 콘텐츠 페이지를 확인 합니다. 모든 iOS 장치에 대 한 안전한 화면 영역에 배치 됩니다. 자세한 내용은 [안전 영역 레이아웃 안내선을 사용 하도록 설정 하면](#safe_area_layout)합니다.
- IPad에서 모달 페이지 표시 스타일을 설정 합니다. 자세한 내용은 [iPad에서 모달 페이지 표시 스타일을 설정](#modal-page-presentation-style)합니다.

<a name="navigationpage-hideseparatorbar" />

### <a name="hiding-the-navigation-bar-separator-on-a-navigationpage"></a>탐색 모음을 숨기는 NavigationPage 구분 기호

이 플랫폼별 숨깁니다 구분 기호 선과에 탐색 모음의 맨 아래에 있는 섀도 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)합니다. 설정 하 여 XAML에서 사용 되는 [ `NavigationPage.HideNavigationBarSeparator` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.HideNavigationBarSeparatorProperty) 바인딩 가능한 속성을 `false`:

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ios:NavigationPage.HideNavigationBarSeparator="true">

</NavigationPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;

public class iOSTitleViewNavigationPageCS : Xamarin.Forms.NavigationPage
{
    public iOSTitleViewNavigationPageCS()
    {
        On<iOS>().SetHideNavigationBarSeparator(true);
    }
}
```

`NavigationPage.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. [ `NavigationPage.SetHideNavigationBarSeparator` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetHideNavigationBarSeparator(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage},System.Boolean)) 메서드의는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 탐색 표시줄 구분 기호 숨겨져 있는지 여부를 네임 스페이스를 컨트롤에 사용 됩니다. 또한 합니다 [ `NavigationPage.HideNavigationBarSeparator` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.HideNavigationBarSeparator(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage})) 메서드를 사용 하 여 탐색 표시줄 구분 기호를 숨길지 여부를 반환할 수 있습니다.

결과 탐색 표시줄 구분 기호는 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 숨길 수 있습니다.

![](ios-images/navigationpage-hideseparatorbar.png "숨겨진 NavigationPage 탐색 모음")

<a name="translucent_navigation_bar" />

### <a name="making-the-navigation-bar-translucent"></a>탐색 모음 반투명 만들기

이 플랫폼별 탐색 표시줄의 투명도 변경 하는 데 사용 되 고 설정 하 여 XAML에서 사용 되는 [ `NavigationPage.IsNavigationBarTranslucent` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty) 연결 된 속성을 `boolean` 값:

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

`NavigationPage.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. [ `NavigationPage.EnableTranslucentNavigationBar` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.EnableTranslucentNavigationBar(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage})) 메서드를를 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스 탐색 모음을 반투명 하 하기 위해 사용 됩니다. 또한 합니다 [ `NavigationPage` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage) 클래스를 `Xamarin.Forms.PlatformConfiguration.iOSSpecific` 네임 스페이스 역시를 [ `DisableTranslucentNavigationBar` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.DisableTranslucentNavigationBar(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage})) 탐색 모음을 기본 상태로 복원 하는 메서드 및 [ `SetIsNavigationBarTranslucent` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetIsNavigationBarTranslucent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage},System.Boolean)) 메서드를 호출 하 여 탐색 표시줄 투명도 설정/해제를 사용할 수 있는 합니다 [ `IsNavigationBarTranslucent` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage})) 메서드:

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

결과적으로 탐색 표시줄의 투명도 변경할 수 있습니다.

![](ios-images/translucent-navigation-bar.png "플랫폼별 반투명 탐색 모음")

<a name="status_bar_color_mode" />

### <a name="adjusting-the-status-bar-text-color-mode"></a>상태 표시줄 텍스트 색 모드를 조정합니다.

이 플랫폼별 제어 상태 표시줄의 텍스트 색에 있는지 여부를 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 탐색 모음의 광도 맞게 조정 됩니다. 설정 하 여 XAML에서 사용 되는 [ `NavigationPage.StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty) 연결 된 속성의 값에는 [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) 열거형:

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

`NavigationPage.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. [ `NavigationPage.SetStatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetStatusBarTextColorMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage},Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스를 제어 상태 표시줄의 텍스트 색에 있는지 여부를 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 에 맞게 조정 됩니다는 탐색 모음의 명도와 합니다 [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) 가능한 두 값을 제공 하는 열거형:

- [`DoNotAdjust`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust) -상태 표시줄 텍스트 색을 변경 하지 말아야 나타냅니다.
- [`MatchNavigationBarTextLuminosity`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity) -상태 표시줄 텍스트 색의 탐색 모음 광도 일치 해야 함을 나타냅니다.

또한 합니다 [ `GetStatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.GetStatusBarTextColorMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage})) 의 현재 값을 검색할 메서드를 사용할 수는 [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) 열거형에 적용 되는 합니다 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage).

결과 상태 표시줄의 텍스트 색을 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 탐색 모음의 광도 맞게 조정할 수 있습니다. 이 예제에서는 사용자 상태 표시줄 텍스트 색 변경 간에 전환 합니다 [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) 및 [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) 페이지를 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage):

![](ios-images/status-bar-text-color-mode.png "상태 표시줄 텍스트 색 모드 플랫폼별")

<a name="large_title" />

### <a name="displaying-large-titles"></a>큰 제목 표시

이 플랫폼별 iOS 11 이상를 사용 하는 장치에 대 한 탐색 모음에서 큰 제목으로 페이지 제목을 표시 하는 데 사용 됩니다. 큰 제목 왼쪽 맞춤은 큰 글꼴을 사용 하 여 및 화면 부동산 효율적으로 사용 되도록 사용자가 콘텐츠를 스크롤 하는 대로 표준 제목으로 전환 합니다. 그러나 가로 방향으로 제목 콘텐츠 레이아웃을 최적화 하기 위해 탐색 모음의 센터로 돌아갑니다. 설정 하 여 XAML에서 사용 되는 `NavigationPage.PrefersLargeTitles` 연결 된 속성을 `boolean` 값:

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

`NavigationPage.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. 합니다 `NavigationPage.SetPrefersLargeTitle` 메서드, 합니다 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스에는 큰 제목 사용 되는지 여부를 제어 합니다.

큰 제목에서 사용 되는 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), 탐색 스택의 모든 페이지에는 큰 제목 표시 됩니다. 설정 하 여 페이지에서이 동작을 재정의할 수 있습니다 합니다 `Page.LargeTitleDisplay` 연결 된 속성의 값으로는 `LargeTitleDisplayMode` 열거형:

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

`Page.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. `Page.SetLargeTitleDisplay` 메서드를 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스에서 큰 제목 동작을 제어를 [ `Page` ](xref:Xamarin.Forms.Page)를 사용 하 여를 `LargeTitleDisplayMode` 세 가지 가능한을 제공 하는 열거형 값:

- `Always` – 탐색 모음 및 글꼴을 강제로 큰 형식을 사용 하는 크기입니다.
- `Automatic` – 탐색 스택의 이전 항목으로 같은 스타일 (중소기업)을 사용 합니다.
- `Never` – 작은 일반 형식으로 탐색 모음을 사용 합니다.

또한 합니다 `SetLargeTitleDisplay` 메서드를 호출 하 여 열거형 값을 설정/해제를 사용할 수는 `LargeTitleDisplay` 현재 반환 하는 메서드 `LargeTitleDisplayMode`:

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

결과 지정한 `LargeTitleDisplayMode` 에 적용 되는 [ `Page` ](xref:Xamarin.Forms.Page), 큰 제목 동작을 제어:

![](ios-images/large-title.png "효과가 플랫폼별 흐리게 표시")

<a name="set_status_bar_visibility" />

### <a name="setting-the-status-bar-visibility-on-a-page"></a>상태 표시줄 설정 페이지에 표시 유형

이 플랫폼별은 상태 표시줄의 표시 유형을 설정 하는 데 사용 되는 [ `Page` ](xref:Xamarin.Forms.Page), 상태 표시줄의 진입 하거나 떠납니다 하는 방법을 제어 하는 기능을 포함 하 고 있습니다를 `Page`입니다. 설정 하 여 XAML에서 사용 되는 `Page.PrefersStatusBarHidden` 연결 된 속성의 값을 합니다 `StatusBarHiddenMode` 열거형 및 필요에 따라 합니다 `Page.PreferredStatusBarUpdateAnimation` 연결 된 속성의 값을는 `UIStatusBarAnimation` 열거형:

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

`Page.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. `Page.SetPrefersStatusBarHidden` 메서드, 합니다 `Xamarin.Forms.PlatformConfiguration.iOSSpecific` 네임 스페이스는 상태 표시줄의 표시 유형을 설정 하는 데 사용 됩니다는 [ `Page` ](xref:Xamarin.Forms.Page) 중 하나를 지정 하 여를 `StatusBarHiddenMode` 열거형 값: `Default`, `True` 또는 `False`합니다. 합니다 `StatusBarHiddenMode.True` 하 고 `StatusBarHiddenMode.False` 장치 방향에 관계 없이 상태 표시줄 표시 여부를 설정 하는 값 및 `StatusBarHiddenMode.Default` 값 세로로 compact 환경에서 상태 표시줄을 숨깁니다.

결과 상태 표시줄의 표시 여부는 [ `Page` ](xref:Xamarin.Forms.Page) 설정할 수 있습니다.

![](ios-images/hide-status-bar.png "상태 표시줄 표시 플랫폼별")

> [!NOTE]
> 에 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)에 지정 된 `StatusBarHiddenMode` 열거형 값에는 모든 자식 페이지에 상태 표시줄도 업데이트 됩니다. 다른 모든 [ `Page` ](xref:Xamarin.Forms.Page)-파생 된 형식이 지정된 된 `StatusBarHiddenMode` 열거형 값에는 현재 페이지의 상태 표시줄만 업데이트 됩니다.

`Page.SetPreferredStatusBarUpdateAnimation` 메서드 상태 표시줄의 진입 하거나 떠납니다 어떻게 설정 되는 [ `Page` ](xref:Xamarin.Forms.Page) 중 하나를 지정 하 여는 `UIStatusBarAnimation` 열거형 값: `None`를 `Fade`, 또는 `Slide`합니다. 경우는 `Fade` 또는 `Slide` 애니메이션 실행 상태 표시줄 진입 하거나 떠납니다 0.25 초는 열거형 값을 지정 합니다 `Page`합니다.

<a name="safe_area_layout" />

### <a name="enabling-the-safe-area-layout-guide"></a>안전 영역 레이아웃 안내선을 사용 하도록 설정

이 플랫폼별 iOS 11 이상를 사용 하는 모든 장치에 대 한 안전한 화면 영역 페이지 콘텐츠가 배치 되는 확인 하는 데 사용 됩니다. 있는지 확인 하는 데는 특히 장치 둥근된 모서리, 홈 표시기 또는 iPhone X에서 센서 하우징 하 여 해당 콘텐츠가 클리핑 되지 않습니다. 설정 하 여 XAML에서 사용 되는 `Page.UseSafeArea` 연결 된 속성을 `boolean` 값:

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

`Page.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. 합니다 `Page.SetUseSafeArea` 메서드, 합니다 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스, 안전 영역 레이아웃 안내선 사용 되는지 여부를 제어 합니다.

결과 모든 Iphone에 안전한 화면 영역을 페이지 콘텐츠를 배치 될 수 있습니다.

[![](ios-images/safe-area-layout.png "안전 영역 레이아웃 안내선")](ios-images/safe-area-layout-large.png#lightbox "안전 영역 레이아웃 안내선")

> [!NOTE]
> Apple에서 정의 되는 안전한 영역 xamarin.forms에서를 사용 설정 합니다 [ `Page.Padding` ](xref:Xamarin.Forms.Page.Padding) 속성인 고 설정 된이 속성의 이전 값을 재정의 합니다.

안전 영역을 검색 하 여 사용자 지정할 수 있습니다 해당 [ `Thickness` ](xref:Xamarin.Forms.Thickness) 값을 `Page.SafeAreaInsets` 메서드에서 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스입니다. 다음으로 수정 필요 하 고 다시 할당할 합니다 `Padding` page 생성자에서 속성 또는 [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) 재정의:

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    var safeInsets = On<iOS>().SafeAreaInsets();
    safeInsets.Left = 20;
    Padding = safeInsets;
}
```

<a name="modal-page-presentation-style" />

### <a name="setting-the-modal-page-presentation-style-on-an-ipad"></a>IPad에서 모달 페이지 표시 스타일 설정

이 플랫폼별 iPad에서 모달 페이지의 표시 스타일을 설정 하는 데 사용 됩니다. 설정 하 여 XAML에서 사용 되는 `Page.ModalPresentationStyle` 바인딩 가능한 속성을 `UIModalPresentationStyle` 열거형 값:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.ModalPresentationStyle="FormSheet">
    ...
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSModalFormSheetPageCS : ContentPage
{
    public iOSModalFormSheetPageCS()
    {
        On<iOS>().SetModalPresentationStyle(UIModalPresentationStyle.FormSheet);
        ...
    }
}
```

`Page.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. `Page.SetModalPresentationStyle` 메서드, 합니다 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스는 모달 표시 스타일을 설정 하는 데 사용 됩니다는 [ `Page` ](xref:Xamarin.Forms.Page) 중 하나를 지정 하 여 `UIModalPresentationStyle` 열거형 값:

- `FullScreen`를 전체 화면을 포함 하도록 모달 표시 스타일을 설정 하는 합니다. 기본적으로 모달 페이지는이 프레젠테이션 스타일을 사용 하 여 표시 됩니다.
- `FormSheet`를 설정 하는 모달 프레젠테이션 스타일에 집중 하 고 화면 보다 더 작은 수입니다.

또한 합니다 `GetModalPresentationStyle` 의 현재 값을 검색할 메서드를 사용할 수는 `UIModalPresentationStyle` 열거형에 적용 되는 [ `Page` ](xref:Xamarin.Forms.Page)합니다.

결과에서 모달 표시 스타일을 [ `Page` ](xref:Xamarin.Forms.Page) 설정할 수 있습니다.

[![](ios-images/modal-presentation-style-small.png "IPad에서 모달 프레젠테이션 스타일")](ios-images/modal-presentation-style-large.png#lightbox "iPad에서 모달 표시 스타일")

> [!NOTE]
> 모달 프레젠테이션 스타일을 설정 하려면이 플랫폼별을 사용 하는 페이지 모달 탐색을 사용 해야 합니다. 자세한 내용은 [Xamarin.Forms 모달 페이지](~/xamarin-forms/app-fundamentals/navigation/modal.md)합니다.

## <a name="layouts"></a>레이아웃

IOS에서 Xamarin.Forms 레이아웃에 대 한 다음과 같은 플랫폼별 기능 제공 됩니다.

- 제어 여부는 [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) 터치 제스처를 처리 하거나 해당 콘텐츠를 전달 합니다. 자세한 내용은 [는 ScrollView에서 콘텐츠 터치 지연](#delay_content_touches)합니다.

<a name="delay_content_touches" />

### <a name="delaying-content-touches-in-a-scrollview"></a>지연 콘텐츠 터치를 ScrollView에서

암시적 타이머를 터치 제스처 시작 되 면 트리거됩니다를 [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) ios 및 `ScrollView` 타이머 범위 내의 사용자 동작을 기준으로 제스처 처리 또는 해당 콘텐츠를 전달 해야 하는지 여부를 결정 합니다. 기본적으로 iOS `ScrollView` 지연 콘텐츠 터치를 하지만 사용 하 여 일부 상황에서 문제를 일으킬 수이 있습니다는 `ScrollView` 하지 말아야 할 때 제스처를 최우선 콘텐츠입니다. 따라서이 플랫폼별 제어 하는지 여부를 `ScrollView` 터치 제스처를 처리 하거나 해당 콘텐츠를 전달 합니다. 설정 하 여 XAML에서 사용 되는 `ScrollView.ShouldDelayContentTouches` 연결 된 속성을 `boolean` 값:

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

`ScrollView.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. `ScrollView.SetShouldDelayContentTouches` 메서드, 합니다 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스에는 컨트롤에 있는지 여부를 [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) 터치 제스처를 처리 하거나 해당 콘텐츠를 전달 합니다. 또한 합니다 `SetShouldDelayContentTouches` 메서드를 호출 하 여 콘텐츠 터치 지연 설정/해제를 사용할 수는 `ShouldDelayContentTouches` 콘텐츠 터치 지연 되 고 있는지 여부를 반환 하는 방법.

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

결과 [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) 지연 되므로 콘텐츠 터치를 수신 사용 하지 않도록 설정할 수 있습니다이 시나리오에는 [ `Slider` ](xref:Xamarin.Forms.Slider) 제스처를 받는 대신 [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) 페이지의 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage):

[![](ios-images/scrollview-delay-content-touches.png "ScrollView 지연 콘텐츠 건드리면 플랫폼별")](ios-images/scrollview-delay-content-touches-large.png#lightbox "지연을 ScrollView 콘텐츠 건드리면 플랫폼별")

## <a name="application"></a>응용 프로그램

Xamarin.Forms에 대 한 iOS 다음에서 플랫폼별 기능이 제공 됩니다 [ `Application` ](xref:Xamarin.Forms.Application) 클래스:

- 컨트롤 레이아웃을 사용 하도록 설정 하 고 주 스레드에서 수행할 업데이트를 렌더링 합니다. 자세한 내용은 [주 스레드에서 컨트롤 업데이트 처리](#update-on-main-thread)합니다.
- 사용을 [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) 스크롤 뷰에서 수집 하 고 스크롤 뷰를 사용 하 여 pan 제스처를 공유 합니다. 자세한 내용은 [동시 팬 제스처 인식 사용](#simultaneous-pan-gesture)합니다.

<a name="update-on-main-thread" />

### <a name="handling-control-updates-on-the-main-thread"></a>주 스레드에서 처리 컨트롤 업데이트

이 플랫폼 특정 컨트롤 레이아웃 및 렌더링 업데이트 백그라운드 스레드에서 수행 되는 대신 주 스레드에서 수행할 수 있습니다. 거의 필요 하지, 있지만 일부의 경우에서 충돌 하지 못할 수 있습니다. 설정 하 여 해당 사용된에서 XAML 합니다 `Application.HandleControlUpdatesOnMainThread` 바인딩 가능한 속성을 `true`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.HandleControlUpdatesOnMainThread="true">
    ...
</Application>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetHandleControlUpdatesOnMainThread(true);
```

`Application.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. `Application.SetHandleControlUpdatesOnMainThread` 메서드, 합니다 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스는 사용 제어 되는지 여부를 제어 레이아웃 및 렌더링 업데이트 백그라운드 스레드에서 수행 되는 대신 주 스레드에서 수행 됩니다. 또한는 `Application.GetHandleControlUpdatesOnMainThread` 메서드를 사용 하 여 컨트롤 레이아웃 및 렌더링 업데이트 주 스레드에서 수행 되는 여부를 반환할 수 있습니다.

<a name="simultaneous-pan-gesture" />

### <a name="enabling-simultaneous-pan-gesture-recognition"></a>사용 하도록 설정 하면 동시 팬 제스처 인식

경우는 [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) 스크롤 뷰를 모든 제스처에 의해 캡처되는 pan의 내 보기에 연결할 때를 `PanGestureRecognizer` 스크롤 보기에 전달 되지 않습니다. 따라서 스크롤 뷰가 더 이상 스크롤됩니다.

이 플랫폼별 수 있도록를 `PanGestureRecognizer` 스크롤 뷰에서 수집 하 고 스크롤 뷰를 사용 하 여 pan 제스처를 공유 합니다. 설정 하 여 XAML에서 사용 되는 [ `Application.PanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.PanGestureRecognizerShouldRecognizeSimultaneouslyProperty) 연결 된 속성을 `true`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.PanGestureRecognizerShouldRecognizeSimultaneously="true">
    ...
</Application>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetPanGestureRecognizerShouldRecognizeSimultaneously(true);
```

`Application.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. [ `Application.SetPanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.SetPanGestureRecognizerShouldRecognizeSimultaneously(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Application},System.Boolean)) 메서드를 합니다 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 스크롤 보기에서 이동 제스처 인식기는 팬 제스처를 캡처 또는 캡처 및 이동을 공유 하는지 여부를 네임 스페이스를 제어 하는 스크롤 뷰를 사용 하 여 제스처입니다. 또한 합니다 [ `Application.GetPanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.GetPanGestureRecognizerShouldRecognizeSimultaneously(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Application})) 팬 제스처를 포함 하는 스크롤 뷰를 사용 하 여 공유 되는지 여부를 반환 하려면 메서드를 사용할 수는 [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer)합니다.

따라서이 플랫폼별 경우 활성화를 사용 하 여는 [ `ListView` ](xref:Xamarin.Forms.ListView) 포함을 [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer)두는 `ListView` 및 `PanGestureRecognizer` 팬 제스처를 받게 됩니다 및 이 처리 합니다. 그러나 플랫폼 특정을 사용할 수 없을 때이 사용 하 여는 `ListView` 포함을 `PanGestureRecognizer`의 `PanGestureRecognizer` 팬 제스처를 캡처 및 처리 됩니다 및 `ListView` 팬 제스처를 받지 않습니다.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Forms에 내장 된 iOS 플랫폼별을 사용 하는 방법을 보여 줍니다. 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [iOSSpecific](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
