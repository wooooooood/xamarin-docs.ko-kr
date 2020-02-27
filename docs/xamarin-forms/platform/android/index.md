---
title: Android 플랫폼 기능
description: 이 문서에서는 Xamarin.ios 응용 프로그램에 Android 관련 기능을 추가 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
ms.openlocfilehash: 7ad7349c89913129cccdd77ac843188cbe668571
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635542"
---
# <a name="android-platform-features"></a>Android 플랫폼 기능

Android 용 Xamarin Forms 응용 프로그램을 개발 하려면 Visual Studio가 필요 합니다. [지원 되는 플랫폼 페이지](~/get-started/supported-platforms.md) 에는 필수 구성 요소에 대 한 자세한 정보가 포함 되어 있습니다.

## <a name="platform-specifics"></a>플랫폼 사양

플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다.

Android에서 Xamarin.ios 보기, 페이지 및 레이아웃에 대해 다음과 같은 플랫폼 관련 기능이 제공 됩니다.

- Z-순서 시각적 요소의 그리기 순서를 결정을 제어 합니다. 자세한 내용은 [Android에서 Visualelement 권한 상승](visualelement-elevation.md)을 참조 하세요.
- 지원 되는 [`VisualElement`](xref:Xamarin.Forms.VisualElement)에서 레거시 색 모드를 사용 하지 않도록 설정 합니다. 자세한 내용은 [Android의 Visualelement 레거시 색 모드](legacy-color-mode.md)를 참조 하세요.

Android의 Xamarin 양식 보기에 대해 다음과 같은 플랫폼별 기능이 제공 됩니다.

- 기본 안쪽 여백 및 Android 단추의 섀도 값을 사용합니다. 자세한 내용은 [Android의 단추 패딩 및 그림자](button-padding-shadow.md)를 참조 하세요.
- [`Entry`](xref:Xamarin.Forms.Entry)에 대 한 소프트 키보드의 입력 방법 편집기 옵션을 설정 합니다. 자세한 내용은 [Android의 입력 입력 방법 편집기 옵션](entry-ime-options.md)을 참조 하세요.
- `ImageButton`에서 그림자를 사용 하도록 설정 합니다. 자세한 내용은 [Android의 ImageButton 드롭 그림자](imagebutton-drop-shadow.md)를 참조 하세요.
- [`ListView`](xref:Xamarin.Forms.ListView) 에서 빠른 스크롤 사용에 대 한 자세한 내용은 [ListView의 빠른 스크롤 (Android](listview-fast-scrolling.md))을 참조 하세요.
- `SwipeView`를 열 때 사용 되는 전환을 제어 합니다. 자세한 내용은 [SwipeView 살짝 밀기 전환 모드](swipeview-swipetransitionmode.md)를 참조 하세요.
- [`WebView`](xref:Xamarin.Forms.WebView) 혼합 된 콘텐츠를 표시할 수 있는지 여부를 제어 합니다. 자세한 내용은 [Android의 웹 보기 혼합 콘텐츠](webview-mixed-content.md)를 참조 하세요.
- [`WebView`](xref:Xamarin.Forms.WebView)확대/축소를 사용 하도록 설정 합니다. 자세한 내용은 [Android에서 웹 보기 확대/축소](webview-zoom-controls.md)를 참조 하세요.

Android의 Xamarin.ios 셀에 대해 다음과 같은 플랫폼별 기능이 제공 됩니다.

- [`ListView`](xref:Xamarin.Forms.ListView) 에서 선택한 항목이 변경 될 때 컨텍스트 작업 메뉴가 업데이트 되지 않도록 [`ViewCell`](xref:Xamarin.Forms.ViewCell) 컨텍스트 작업 레거시 모드를 사용 하도록 설정 합니다. 자세한 내용은 [Android의 Viewcell 컨텍스트 작업](viewcell-context-actions.md)을 참조 하세요.

Android에서 Xamarin Forms 페이지에 대해 다음과 같은 플랫폼별 기능이 제공 됩니다.

- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)탐색 모음의 높이를 설정 합니다. 자세한 내용은 [Android의 Navigationpage Bar Height](navigationpage-bar-height.md)를 참조 하세요.
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)페이지를 탐색할 때 전환 애니메이션 사용 안 함 자세한 내용은 [TabbedPage 페이지 전환 애니메이션 (Android](tabbedpage-transition-animations.md))을 참조 하세요.
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)페이지 간에 살짝 밀기를 사용 하도록 설정 합니다. 자세한 내용은 [TabbedPage Page 살짝 밀기 On Android](tabbedpage-page-swiping.md)을 참조 하세요.
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)도구 모음 배치 및 색을 설정 합니다. 자세한 내용은 [TabbedPage Toolbar Placement And Color In Android](tabbedpage-toolbar-placement-color.md)항목을 참조 하세요.

Android의 Xamarin.ios [`Application`](xref:Xamarin.Forms.Application) 클래스에 대해 다음과 같은 플랫폼별 기능이 제공 됩니다.

- 소프트 키보드의 운영 모드를 설정 합니다. 자세한 내용은 [Android의 소프트 키보드 입력 모드](soft-keyboard-input-mode.md)를 참조 하세요.
- AppCompat을 사용 하는 응용 프로그램에 대해 [`Disappearing`](xref:Xamarin.Forms.Page.Appearing) 를 사용 하지 않도록 설정 하 고 각각 일시 중지 및 다시 시작할 때 페이지 수명 주기 이벤트를 [`Appearing`](xref:Xamarin.Forms.Page.Appearing) 자세한 내용은 [Android의 페이지 수명 주기 이벤트](page-lifecycle-events.md)를 참조 하세요.

## <a name="platform-support"></a>플랫폼 지원

원래, 기본 Xamarin.ios Android 프로젝트는 Android 5.0 이전에 일반적인 컨트롤 렌더링의 이전 스타일을 사용 했습니다. 템플릿을 사용 하 여 빌드한 응용 프로그램은 기본 활동의 기본 클래스로 `FormsApplicationActivity` 있습니다.

## <a name="material-design-via-appcompat"></a>AppCompat을 통한 재질 디자인

이제 xamarin.ios Android 프로젝트는 기본 활동의 기본 클래스로 `FormsAppCompatActivity`를 사용 합니다. 이 클래스는 Android에서 제공 하는 **AppCompat** 기능을 사용 하 여 재질 디자인 테마를 구현 합니다.

Xamarin. Forms Android 프로젝트에 재질 디자인 테마를 추가 하려면 [AppCompat 지원에 대 한 설치 지침](appcompat-material-design.md) 을 따르세요.

기본 `FormsApplicationActivity`를 사용 하는 **Todo** 샘플은 다음과 같습니다.

[![](images/before-appcompat-sml.png "Todo Sample Application Without AppCompat")](images/before-appcompat.png#lightbox "Todo Sample Application Without AppCompat")

이 코드는 `FormsAppCompatActivity`를 사용 하 고 추가 테마 정보를 추가 하 여 프로젝트를 업그레이드 한 후의 코드와 동일 합니다.

[![](images/post-appcompat-sml.png "Todo Sample Application With AppCompat and Theming")](images/post-appcompat.png#lightbox "Todo Sample Application With AppCompat and Theming")

> [!NOTE]
> `FormsAppCompatActivity`사용 하는 경우 [일부 Android 사용자 지정 렌더러의 기본 클래스](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md) 는 다릅니다.

## <a name="androidx-migration"></a>AndroidX 마이그레이션

AndroidX는 Android 지원 라이브러리를 대체 합니다. AndroidX 및 AndroidX 라이브러리를 사용 하도록 Xamarin.ios 앱을 마이그레이션하는 방법에 대 한 자세한 내용은 [xamarin.ios의 androidx 마이그레이션](~/xamarin-forms/platform/android/androidx-migration.md)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [재질 디자인 지원 추가](appcompat-material-design.md)
