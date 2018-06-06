---
title: Windows 플랫폼 특성
description: 플랫폼 비슷하므로 허용 사용자 지정 렌더러 또는 효과 구현 하지 않고도 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 Xamarin.Forms에 기본 제공 되는 Windows 플랫폼 특성을 사용 하는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: 7299de658a3491928e9bbeaa4dd192a8e95c435e
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732803"
---
# <a name="windows-platform-specifics"></a>Windows 플랫폼 특성

_플랫폼 비슷하므로 허용 사용자 지정 렌더러 또는 효과 구현 하지 않고도 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 Xamarin.Forms에 기본 제공 되는 Windows 플랫폼 특성을 사용 하는 방법을 보여줍니다._

에 플랫폼 UWP (유니버설 Windows), Xamarin.Forms 플랫폼 다음 내용을 포함 되어 있습니다.

- 도구 모음 배치 옵션을 설정 합니다. 자세한 내용은 참조 [도구 모음 배치를 바꾸기](#toolbar_placement)합니다.
- 축소 된 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) 탐색 모음입니다. 자세한 내용은 참조 [MasterDetailPage 탐색 모음 축소](#collapsable_navigation_bar)합니다.
- 사용 하도록 설정 된 [ `WebView` ](xref:Xamarin.Forms.WebView) UWP 메시지 대화 상자에서 JavaScript 경고를 표시 하도록 합니다. 자세한 내용은 참조 [JavaScript 경고 표시](#webview-javascript-alert)합니다.
- 사용 하도록 설정 된 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) 맞춤법 검사 엔진와 상호 작용할 수 있습니다. 자세한 내용은 참조 [SearchBar 맞춤법 검사를 사용 하도록 설정](#searchbar-spellcheck)합니다.
- 검색 텍스트의 콘텐츠를 읽는 순서 [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), 및 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스. 자세한 내용은 참조 [콘텐츠에서 읽기 순서를 감지](#inputview-readingorder)합니다.
- 지원 되는 레거시 색 모드를 해제 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다. 자세한 내용은 참조 [레거시 색 모드 해제](#legacy-color-mode)합니다.
- 탭 제스처 지원을 사용 하도록 설정 된 [ `ListView` ](xref:Xamarin.Forms.ListView)합니다. 자세한 내용은 참조 [ListView에서 탭 제스처 지원을 사용 하도록 설정](#listview-selectionmode)합니다.

<a name="toolbar_placement" />

## <a name="changing-the-toolbar-placement"></a>도구 모음 배치를 변경합니다.

이 플랫폼별은에서 도구 모음 배치를 변경 하는 데 사용 되는 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), 하 고 설정 하 여 XAML에서 사용 되는 [ `Page.ToolbarPlacement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty/) 연결 된 속성의 값에는 [ `ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) 열거형:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:Page.ToolbarPlacement="Bottom">
  ...
</TabbedPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetToolbarPlacement(ToolbarPlacement.Bottom);
```

`Page.On<Windows>` 메서드 지정이 플랫폼별 Windows에만 실행 됩니다. [ `Page.SetToolbarPlacement` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.SetToolbarPlacement/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.Page}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) 네임 스페이스, 하는 데 사용 하 여 도구 모음 배치를 설정는 [ `ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) 열거형 제공 3 개의 값: [ `Default` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default/), [ `Top` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top/), 및 [ `Bottom` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom/)합니다.

결과 지정 된 도구 모음 배치에 적용 되는 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 인스턴스:

[![](windows-images/toolbar-placement.png "도구 모음 배치 플랫폼별")](windows-images/toolbar-placement-large.png#lightbox "도구 모음 배치 플랫폼별")

<a name="collapsable_navigation_bar" />

## <a name="collapsing-a-masterdetailpage-navigation-bar"></a>축소 MasterDetailPage 탐색 모음

이 플랫폼별은 탐색 모음에서 축소 하는 데 사용 되는 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), 하 고 설정 하 여 XAML에서 사용 되는 [ `MasterDetailPage.CollapseStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty/) 및 [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty/)연결 된 속성:

```xaml
<MasterDetailPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:MasterDetailPage.CollapseStyle="Partial"
                  windows:MasterDetailPage.CollapsedPaneWidth="48">
  ...
</MasterDetailPage>

```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

`MasterDetailPage.On<Windows>` 메서드 지정이 플랫폼별 Windows에만 실행 됩니다. [ `Page.SetCollapseStyle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.SetCollapseStyle/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) 네임 스페이스, 축소 스타일을 지정 하는 [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) 두 개를 제공 하는 열거형 값: [ `Full` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full/) 및 [ `Partial` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial/)합니다. [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/System.Double/) 부분적으로 축소 된 탐색 모음의 너비를 지정 하려면 메서드를 사용 합니다.

결과 지정 된 [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) 에 적용 되는 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) 지정 되 고 너비가 인스턴스:

[![](windows-images/collapsed-navigation-bar.png "탐색 모음 플랫폼별 축소")](windows-images/collapsed-navigation-bar-large.png#lightbox "탐색 모음 플랫폼별 축소")

<a name="webview-javascript-alert" />

## <a name="displaying-javascript-alerts"></a>JavaScript 경고 표시

이 플랫폼별 사용 하면 한 [ `WebView` ](xref:Xamarin.Forms.WebView) UWP 메시지 대화 상자에서 JavaScript 경고를 표시 하도록 합니다. 설정 하 여 XAML에서 사용 되는 [ `WebView.IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabledProperty) 연결 된 속성을는 `boolean` 값:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <WebView ... windows:WebView.IsJavaScriptAlertEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var webView = new Xamarin.Forms.WebView
{
  Source = new HtmlWebViewSource
  {
    Html = @"<html><body><button onclick=""window.alert('Hello World from JavaScript');"">Click Me</button></body></html>"
  }
};
webView.On<Windows>().SetIsJavaScriptAlertEnabled(true);
```

`WebView.On<Windows>` 메서드 지정이 플랫폼별 유니버설 Windows 플랫폼에만 실행 됩니다. [ `WebView.SetIsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.SetIsJavaScriptAlertEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.WebView},System.Boolean)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 네임 스페이스, JavaScript 경고를 설정할지 여부를 제어에 사용 됩니다. 또한는 `WebView.SetIsJavaScriptAlertEnabled` 메서드를 호출 하 여 JavaScript 경고를 설정/해제를 사용할 수는 [ `IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabled*) 메서드를 사용 하는지 여부를 반환 합니다.

```csharp
_webView.On<Windows>().SetIsJavaScriptAlertEnabled(!_webView.On<Windows>().IsJavaScriptAlertEnabled());
```

결과 경고 JavaScript UWP 메시지 대화 상자에 표시할 수 있습니다.

![WebView JavaScript 경고 플랫폼별](windows-images/webview-javascript-alert.png "플랫폼별 WebView JavaScript 경고")

<a name="searchbar-spellcheck" />

## <a name="enabling-searchbar-spell-check"></a>SearchBar 맞춤법 검사를 사용 하도록 설정

이 플랫폼별 수 있도록는 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) 맞춤법 검사 엔진와 상호 작용할 수 있습니다. 설정 하 여 XAML에서 사용 되는 [ `SearchBar.IsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) 연결 된 속성을는 `boolean` 값:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

`SearchBar.On<Windows>` 메서드 지정이 플랫폼별 유니버설 Windows 플랫폼에만 실행 됩니다. [ `SearchBar.SetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.SetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar},System.Boolean)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 네임 스페이스에는 맞춤법 검사기 기능이 설정 및 해제 합니다. 또한는 `SearchBar.SetIsSpellCheckEnabled` 메서드를 호출 하 여 맞춤법 검사기를 설정/해제를 사용할 수는 [ `SearchBar.GetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.GetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar})) 맞춤법 검사기 사용 되는지 여부를 반환 하는 메서드:

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

결과 그 텍스트에 입력 된 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) 맞춤법으로 확인할 수 있습니다, 사용자에 게 표시 되 고 잘못 된 철자:

![SearchBar 맞춤법 검사 플랫폼 특정](windows-images/searchbar-spellcheck.png "SearchBar 맞춤법 검사 플랫폼 관련")

> [!NOTE]
> `SearchBar` 클래스에 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 네임 스페이스에도 [ `EnableSpellCheck` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) 및 [ `DisableSpellCheck` ](xre:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) 설정 및 해제 하는 데 사용할 수 있는 메서드 맞춤법 검사기는 `SearchBar`각각.

<a name="inputview-readingorder" />

## <a name="detecting-reading-order-from-content"></a>콘텐츠에서 읽기 순서를 검색 합니다.

이 플랫폼별에 양방향 텍스트의 읽기 순서 (왼쪽에서 오른쪽 또는 오른쪽에서 왼쪽)를 사용 하면 [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), 및 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스를 동적으로 검색할 수 있습니다. 설정 하 여 XAML에서 사용 되는 [ `InputView.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) (에 대 한 `Entry` 및 `Editor` 인스턴스) 또는 [ `Label.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty) 연결 된 속성을는 `boolean` 값:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

`Editor.On<Windows>` 메서드 지정이 플랫폼별 유니버설 Windows 플랫폼에만 실행 됩니다. [ `InputView.SetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.SetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView},System.Boolean)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 읽기 순서 내용에서 검색 되는지 네임 스페이스 제어에 사용 되는 [ `InputView` ](xref:Xamarin.Forms.InputView). 또한는 `InputView.SetDetectReadingOrderFromContent` 읽기 순서를 호출 하 여 내용에서 검색 되는 여부를 전환 하려면 메서드를 사용할 수는 [ `InputView.GetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.GetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView})) 현재 값을 반환 하는 메서드:

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

결과 [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), 및 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스가 동적으로 검색 하는 해당 콘텐츠의 읽기 순서를 가질 수 있습니다.

[![플랫폼별 콘텐츠에서 읽기 순서를 검색 하는 InputView](windows-images/editor-readingorder.png "InputView 플랫폼별 콘텐츠에서 읽기 순서를 검색")](windows-images/editor-readingorder-large.png#lightbox "InputView에서 읽기 순서를 검색 합니다. 플랫폼별 콘텐츠")

> [!NOTE]
> 설정할 때와 달리는 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성, 텍스트 내용에서 읽기 순서가 보기 내에서 텍스트의 맞춤 영향을 주지 것입니다를 검색 하는 뷰에 대 한 논리입니다. 대신, 양방향 텍스트 블록에 배치 되는 순서를 조정 합니다.

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>레거시 색 모드를 사용 하지 않도록 설정

일부 Xamarin.Forms 뷰 기능을 레거시 색 모드. 이 모드에서는 때는 [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) 보기의 속성이로 설정 되어 `false`, 보기에는 사용할 수 없는 상태에 대 한 기본 네이티브 색을 사용 하 여 사용자가 설정한 색 보다 우선 합니다. 이전 버전과 호환성을이 레거시 색 모드의 기본 동작을 지원 되는 뷰 상태가 유지 됩니다.

이 플랫폼별 보기를 사용 하지 않도록 설정 하는 경우에 사용자가 설정한 뷰에 색 유지 되도록이 레거시 색 모드를 해제 합니다. 설정 하 여 XAML에서 사용 되는 [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.IsLegacyColorModeEnabledProperty) 연결 된 속성을 `false`:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Editor Text="Enter text here"
                TextColor="Blue"
                BackgroundColor="Bisque"
                windows:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

_legacyColorModeDisabledEditor.On<Windows>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Windows>` 메서드 지정이 플랫폼별 Windows에만 실행 됩니다. [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Boolean)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 네임 스페이스, 레거시 색 모드 사용 되지 않는지 여부를 제어에 사용 됩니다. 또한는 [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) 메서드를 사용 하 여 레거시 색 모드 되지 않는지 여부를 반환할 수 있습니다.

결과 보기 사용 하지 않는 경우 사용자가 설정한 뷰에 색도 계속 되도록 레거시 색 모드를 비활성화할 수 있습니다.

![](windows-images/legacy-color-mode-disabled.png "레거시 색 모드 사용 안 함")

> [!NOTE]
> 설정할 때는 [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) 레거시 모드는 완전히 보기에서는 무시 됩니다. 시각적 상태에 대 한 자세한 내용은 참조 [The Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)합니다.

<a name="listview-selectionmode" />

## <a name="enabling-tap-gesture-support-in-a-listview"></a>ListView에서 탭 제스처 지원을 사용 하도록 설정

Xamarin.Forms는 기본적으로 유니버설 Windows 플랫폼에서 [ `ListView` ](xref:Xamarin.Forms.ListView) 네이티브를 사용 하 여 `ItemClick` 이벤트가 네이티브 보다는 상호 작용에 응답할 수 `Tapped` 이벤트입니다. Windows 내레이터 및 키보드 상호 작용할 수 있도록 내게 필요한 옵션 기능을 제공 된 `ListView`합니다. 그러나 렌더링 내 모든 탭 제스처는 `ListView` 작동 하지 않습니다.

이 플랫폼별 제어 여부를 항목에 [ `ListView` ](xref:Xamarin.Forms.ListView) 제스처를 탭에 응답할 수 있습니다 및이 따른 여부를 네이티브 `ListView` 발생은 `ItemClick` 또는 `Tapped` 이벤트입니다. 설정 하 여 XAML에서 사용 되는 [ `ListView.SelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty) 연결 된 속성의 값에는 [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) 열거형:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <ListView ... windows:ListView.SelectionMode="Inaccessible">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

listView.On<Windows>().SetSelectionMode(ListViewSelectionMode.Inaccessible);
```

`ListView.On<Windows>` 메서드 지정이 플랫폼별 유니버설 Windows 플랫폼에만 실행 됩니다. [ `ListView.SetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 네임 스페이스, 여부 제어에 사용 됩니다 항목에 [ `ListView` ](xref:Xamarin.Forms.ListView) 으로 제스처를 누르는 동작에 응답할 수는 [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) 열거 가능한 두 값을 제공 합니다.

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible) – 나타냅니다는 `ListView` 네이티브 트리거는 발생 `ItemClick` 이벤트를 상호 작용을 처리 하 고 따라서 내게 필요한 옵션 기능을 제공 합니다. 따라서 Windows 내레이터 및 키보드 상호 작용할 수는 `ListView`합니다. 그러나 항목에 `ListView` 제스처 탭에 응답할 수 없습니다. 이것이 기본 동작에 대 한 `ListView` 유니버설 Windows 플랫폼에 있는 인스턴스.
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible) – 나타냅니다는 `ListView` 네이티브 트리거는 발생 `Tapped` 상호 작용을 처리 하는 이벤트입니다. 따라서 항목에 `ListView` 제스처 탭에 응답할 수 있습니다. 그러나 내게 필요한 옵션 기능은 없습니다 및 Windows 내레이터 및 키보드 상호 작용할 수 없습니다 따라서는 `ListView`합니다.

> [!NOTE]
> `Accessible` 및 `Inaccessible` 는 동시에 선택 모드 및 액세스 가능 중 하나를 선택 해야 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 또는 `ListView` 제스처 탭에 응답할 수입니다.

또한는 [ `GetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.GetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView})) 메서드를 사용 하 여 현재 반환할 수 있습니다 [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)합니다.

결과 지정 된 [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) 에 적용 되는 [ `ListView` ](xref:Xamarin.Forms.ListView), 제어 하 있는지 여부를 항목에 `ListView` 제스처를 탭에 응답할 수 및이 따른 여부를 네이티브 `ListView` 발생은 `ItemClick` 또는 `Tapped` 이벤트입니다.

## <a name="summary"></a>요약

이 문서 Xamarin.Forms에 기본 제공 되는 Windows 플랫폼 특성을 사용 하는 방법을 보여 줍니다. 플랫폼 비슷하므로 허용 사용자 지정 렌더러 또는 효과 구현 하지 않고도 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)
