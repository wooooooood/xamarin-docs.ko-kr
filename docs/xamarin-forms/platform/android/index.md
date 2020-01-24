---
title: Android 플랫폼 기능
description: 이 문서에서는 Xamarin.ios 응용 프로그램에 Android 관련 기능을 추가 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
ms.openlocfilehash: ce8d0b834cff5b2eee46b4ace5de4a95d196726d
ms.sourcegitcommit: a3b7e016fb25584dbf57bae89b64a9f98031e7c9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76549996"
---
# <a name="android-platform-features"></a>Android 플랫폼 기능

Android 용 Xamarin Forms 응용 프로그램을 개발 하려면 Visual Studio가 필요 합니다. [지원 되는 플랫폼 페이지](~/get-started/supported-platforms.md) 에는 필수 구성 요소에 대 한 자세한 정보가 포함 되어 있습니다.

## <a name="platform-specifics"></a>플랫폼 사양

플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다.

Android에서 Xamarin.ios 보기, 페이지 및 레이아웃에 대해 다음과 같은 플랫폼 관련 기능이 제공 됩니다.

- Z-순서 시각적 요소의 그리기 순서를 결정을 제어 합니다. 자세한 내용은 [Android에서 Visualelement 권한 상승](visualelement-elevation.md)을 참조 하세요.
- 지원 되는 레거시 색 모드 사용 안 함 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다. 자세한 내용은 [Android의 Visualelement 레거시 색 모드](legacy-color-mode.md)를 참조 하세요.

Android의 Xamarin 양식 보기에 대해 다음과 같은 플랫폼별 기능이 제공 됩니다.

- 기본 안쪽 여백 및 Android 단추의 섀도 값을 사용합니다. 자세한 내용은 [Android의 단추 패딩 및 그림자](button-padding-shadow.md)를 참조 하세요.
- 입력된 방법에 대 한 소프트 키보드에 대 한 편집기 옵션 설정 된 [ `Entry` ](xref:Xamarin.Forms.Entry)합니다. 자세한 내용은 [Android의 입력 입력 방법 편집기 옵션](entry-ime-options.md)을 참조 하세요.
- 에 그림자를 사용 하도록 설정 된 `ImageButton`합니다. 자세한 내용은 [Android의 ImageButton 드롭 그림자](imagebutton-drop-shadow.md)를 참조 하세요.
- [`ListView`](xref:Xamarin.Forms.ListView) 에서 빠른 스크롤 사용에 대 한 자세한 내용은 [ListView의 빠른 스크롤 (Android](listview-fast-scrolling.md))을 참조 하세요.
- `SwipeView`를 열 때 사용 되는 전환을 제어 합니다. 자세한 내용은 [SwipeView 살짝 밀기 전환 모드](swipeview-swipetransitionmode.md)를 참조 하세요.
- 제어 여부는 [ `WebView` ](xref:Xamarin.Forms.WebView) 혼합 된 콘텐츠를 표시할 수 있습니다. 자세한 내용은 [Android의 웹 보기 혼합 콘텐츠](webview-mixed-content.md)를 참조 하세요.
- [`WebView`](xref:Xamarin.Forms.WebView)확대/축소를 사용 하도록 설정 합니다. 자세한 내용은 [Android에서 웹 보기 확대/축소](webview-zoom-controls.md)를 참조 하세요.

Android의 Xamarin.ios 셀에 대해 다음과 같은 플랫폼별 기능이 제공 됩니다.

- [`ListView`](xref:Xamarin.Forms.ListView) 에서 선택한 항목이 변경 될 때 컨텍스트 작업 메뉴가 업데이트 되지 않도록 [`ViewCell`](xref:Xamarin.Forms.ViewCell) 컨텍스트 작업 레거시 모드를 사용 하도록 설정 합니다. 자세한 내용은 [Android의 Viewcell 컨텍스트 작업](viewcell-context-actions.md)을 참조 하세요.

Android에서 Xamarin Forms 페이지에 대해 다음과 같은 플랫폼별 기능이 제공 됩니다.

- 탐색 막대의 높이에 설정 된 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)합니다. 자세한 내용은 [Android의 Navigationpage Bar Height](navigationpage-bar-height.md)를 참조 하세요.
- 페이지 사이 탐색할 때 전환 애니메이션을 사용 하지 않도록 설정 된 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)합니다. 자세한 내용은 [TabbedPage 페이지 전환 애니메이션 (Android](tabbedpage-transition-animations.md))을 참조 하세요.
- 내 페이지 간 살짝 사용 하도록 설정 된 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)합니다. 자세한 내용은 [TabbedPage Page 살짝 밀기 On Android](tabbedpage-page-swiping.md)을 참조 하세요.
- 도구 모음 위치와 색에 설정 된 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)합니다. 자세한 내용은 [TabbedPage Toolbar Placement And Color In Android](tabbedpage-toolbar-placement-color.md)항목을 참조 하세요.

Android의 Xamarin.ios [`Application`](xref:Xamarin.Forms.Application) 클래스에 대해 다음과 같은 플랫폼별 기능이 제공 됩니다.

- 소프트 키보드의 운영 모드를 설정 합니다. 자세한 내용은 [Android의 소프트 키보드 입력 모드](soft-keyboard-input-mode.md)를 참조 하세요.
- 사용 하지 않도록 설정 합니다 [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) 하 고 [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) 페이지 수명 주기 이벤트를 일시 중지 하 고 각각 AppCompat를 사용 하는 응용 프로그램에 대 한 다시 시작 합니다. 자세한 내용은 [Android의 페이지 수명 주기 이벤트](page-lifecycle-events.md)를 참조 하세요.

## <a name="platform-support"></a>플랫폼 지원

원래, 기본 Xamarin.ios Android 프로젝트는 Android 5.0 이전에 일반적인 컨트롤 렌더링의 이전 스타일을 사용 했습니다. 이 템플릿을 가지고 만들어진 앱은 `FormsApplicationActivity`를 메인 액티비티의 베이스 클래스로 가지고 있습니다.

## <a name="material-design-via-appcompat"></a>AppCompat을 통한 재질 디자인

이제 xamarin.ios Android 프로젝트는 기본 활동의 기본 클래스로 `FormsAppCompatActivity`를 사용 합니다. 이 클래스는 Android에서 제공 하는 **AppCompat** 기능을 사용 하 여 재질 디자인 테마를 구현 합니다.

머티리얼 디자인 테마를 자신의 Xamarin.Forms 안드로이드 프로젝트에 추가하려면 [AppCompat 지원에 대한 설치 지침](appcompat-material-design.md)을 참고하세요.

다음은 기본 `FormsApplicationActivity`를 포함한 **Todo** 샘플입니다.

[![](images/before-appcompat-sml.png "Todo Sample Application Without AppCompat")](images/before-appcompat.png#lightbox "Todo Sample Application Without AppCompat")

그리고 다음은 동일한 코드를 `FormsAppCompatActivity`를 사용하게 하고, 추가 테마 정보를 추가하여 프로젝트를 업그레이드한 결과입니다.

[![](images/post-appcompat-sml.png "Todo Sample Application With AppCompat and Theming")](images/post-appcompat.png#lightbox "Todo Sample Application With AppCompat and Theming")

> [!NOTE]
> `FormsAppCompatActivity`를 사용하는 경우, [몇몇 안드로이드 커스텀 랜더러들의 베이스 클래스](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)가 달라질 수 있습니다.

## <a name="related-links"></a>관련 링크

- [재질 디자인 지원 추가](appcompat-material-design.md)
