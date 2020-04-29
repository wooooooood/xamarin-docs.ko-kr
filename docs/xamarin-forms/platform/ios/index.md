---
title: Xamarin.ios의 iOS 플랫폼 기능
description: Xamarin.ios 응용 프로그램에 iOS 관련 기능을 추가 합니다.
ms.prod: xamarin
ms.assetid: 634AB62E-68C8-454C-838B-F1CC4E4E21BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2020
ms.openlocfilehash: 97b9b70a1df61beb7b064c99721b4277e2570b41
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517049"
---
# <a name="ios-platform-features-in-xamarinforms"></a>Xamarin.ios의 iOS 플랫폼 기능

IOS 용 Xamarin Forms 응용 프로그램을 개발 하려면 Visual Studio가 필요 합니다. [지원 되는 플랫폼 페이지](~/get-started/supported-platforms.md) 에는 필수 구성 요소에 대 한 자세한 정보가 포함 되어 있습니다.

## <a name="platform-specifics"></a>플랫폼 사양

플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다.

다음과 같은 플랫폼별 기능이 iOS의 Xamarin 보기, 페이지 및 레이아웃에 제공 됩니다.

- 모든 [`VisualElement`](xref:Xamarin.Forms.VisualElement)에 대 한 흐리게 지원. 자세한 내용은 [iOS에서 Visualelement 흐림 효과](visualelement-blur.md)를 참조 하세요.
- 지원 되 [`VisualElement`](xref:Xamarin.Forms.VisualElement)는에서 레거시 색 모드를 사용 하지 않도록 설정 합니다. 자세한 내용은 [iOS의 Visualelement 레거시 색 모드](legacy-color-mode.md)를 참조 하세요.
- 에서 그림자를 사용 하도록 설정 [`VisualElement`](xref:Xamarin.Forms.VisualElement)합니다. 자세한 [내용은 항목을 참조 하세요.](visualelement-drop-shadow.md)
- 개체가 터치 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 이벤트에 대 한 첫 번째 응답자가 될 수 있도록 합니다. 자세한 내용은 [Visualelement First 응답자](visualelement-first-responder.md)를 참조 하세요.

IOS의 Xamarin 양식 보기에 대해 다음과 같은 플랫폼별 기능이 제공 됩니다.

- [`Cell`](xref:Xamarin.Forms.Cell) 배경색 설정 자세한 내용은 [iOS의 셀 배경색](cell-background-color.md)을 참조 하세요.
- 에서 항목 선택을 수행 하는 시기 [`DatePicker`](xref:Xamarin.Forms.DatePicker)를 제어 합니다. 자세한 내용은 [DatePicker Item Selection In IOS 항목](datepicker-selection.md)을 참조 하세요.
- 글꼴 크기를 조정 하 여 입력 [`Entry`](xref:Xamarin.Forms.Entry) 텍스트가에 적합 한지 확인 자세한 내용은 [iOS의 항목 글꼴 크기](entry-font-size.md)를 참조 하세요.
- 에서 커서 색을 설정 [`Entry`](xref:Xamarin.Forms.Entry)합니다. 자세한 내용은 [iOS의 항목 커서 색](entry-cursor-color.md)을 참조 하세요.
- 스크롤 하 [`ListView`](xref:Xamarin.Forms.ListView) 는 동안 머리글 셀이 부동 되는지 여부를 제어 합니다. 자세한 내용은 [iOS의 ListView 그룹 헤더 스타일](listview-group-header-style.md)을 참조 하세요.
- Items 컬렉션을 [`ListView`](xref:Xamarin.Forms.ListView) 업데이트할 때 행 애니메이션을 사용 하지 않도록 설정할지 여부를 제어 합니다. 자세한 내용은 [iOS의 ListView 행 애니메이션](listview-row-animations.md)을 참조 하세요.
- 에 [`ListView`](xref:Xamarin.Forms.ListView)구분 기호 스타일 설정 자세한 내용은 [iOS의 ListView 구분 기호 스타일](listview-separator-style.md)을 참조 하세요.
- 에서 항목 선택을 수행 하는 시기 [`Picker`](xref:Xamarin.Forms.Picker)를 제어 합니다. 자세한 내용은 [iOS에서 선택 항목 선택 항목](picker-selection.md)을 참조 하세요.
- 에 배경이 있는지 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 여부를 제어 하는입니다. 자세한 내용은 [iOS의 Searchbar 스타일](searchbar-style.md)을 참조 하세요.
- 엄지 단추 [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) 를 끌 필요 없이 [`Slider`](xref:Xamarin.Forms.Slider) 막대의 위치를 눌러 속성을 설정할 수 있도록 합니다. `Slider` 자세한 내용은 [iOS의 슬라이더 엄지 단추 탭](slider-thumb.md)을 참조 하세요.
- 를 `SwipeView`열 때 사용 되는 전환을 제어 합니다. 자세한 내용은 [SwipeView 살짝 밀기 전환 모드](swipeview-swipetransitionmode.md)를 참조 하세요.
- 에서 항목 선택을 수행 하는 시기 [`TimePicker`](xref:Xamarin.Forms.TimePicker)를 제어 합니다. 자세한 내용은 [iOS에서 Timepicker 항목 선택](timepicker-selection.md)을 참조 하세요.

IOS의 Xamarin.ios 페이지에 대해 다음과 같은 플랫폼 관련 기능이 제공 됩니다.

- 마스터 페이지를 표시할 때의 세부 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 정보 페이지에 그림자가 적용 되는지 여부를 제어 합니다. 자세한 내용은 [MasterDetailPage Shadow](masterdetailpage-shadow.md)를 참조 하세요.
- 에 탐색 모음 구분 기호를 숨깁니다 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage). 자세한 내용은 [iOS의 Navigationpage Bar Separator](navigation-bar-separator.md)를 참조 하세요.
- 탐색 모음이 반투명 인지 여부를 제어 합니다. 자세한 내용은 [iOS의 탐색 모음 반투명도](navigation-bar-translucent.md)을 참조 하세요.
- 탐색 모음의 광도와 일치 하도록의 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 상태 표시줄 텍스트 색을 조정할 것인지 여부를 제어 합니다. 자세한 내용은 [iOS의 Navigationpage Bar 텍스트 색 모드](status-bar-text-color.md)를 참조 하세요.
- 페이지 탐색 모음에서 페이지 제목이 크게 표시 되는지 여부를 제어 합니다. 자세한 내용은 [iOS의 큰 페이지 제목](page-large-title.md)을 참조 하세요.
- 에서 홈 표시기의 표시 여부를 설정 [`Page`](xref:Xamarin.Forms.Page)합니다. 자세한 내용은 [iOS의 홈 표시기 표시 유형](page-home-indicator.md)을 참조 하세요.
- 에서 상태 표시줄의 표시 여부를 [`Page`](xref:Xamarin.Forms.Page)설정 합니다. 자세한 내용은 [iOS의 페이지 상태 표시줄 표시 유형](page-status-bar-visibility.md)을 참조 하세요.
- 페이지 콘텐츠가 모든 iOS 장치에 안전 하 게 화면 영역에 배치 되도록 합니다. 자세한 내용은 [iOS의 안전 영역 레이아웃 가이드](page-safe-area-layout.md)를 참조 하세요.
- 모달 페이지의 프레젠테이션 스타일 설정 자세한 내용은 [모달 페이지 표시 스타일](page-presentation-style.md)을 참조 하세요.
- 에서 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)탭 모음의 반투명도 모드 설정 자세한 내용은 [TabbedPage 반투명 TabBar In iOS](tabbedpage-translucent-tabbar.md)를 참조 하세요.

IOS의 Xamarin.ios 레이아웃에 대해 다음과 같은 플랫폼 관련 기능이 제공 됩니다.

- 가 [`ScrollView`](xref:Xamarin.Forms.ScrollView) 터치 제스처를 처리 하는지 아니면 콘텐츠에 전달 하는지 여부를 제어 합니다. 자세한 내용은 [ScrollView Content 접촉 In iOS](scrollview-content-touches.md)를 참조 하세요.

IOS의 Xamarin.ios [`Application`](xref:Xamarin.Forms.Application) 클래스에 대해 다음과 같은 플랫폼별 기능이 제공 됩니다.

- 명명 된 글꼴 크기에 대 한 접근성 크기 조정을 비활성화 합니다. 자세한 내용은 [iOS의 명명 된 글꼴 크기에 대 한 접근성 크기 조정](named-font-size-scaling.md)을 참조 하세요.
- 주 스레드에서 수행할 수 있도록 컨트롤 레이아웃 및 렌더링 업데이트를 사용 합니다. 자세한 내용은 [iOS에서 주 스레드 제어 업데이트](main-thread-updates-ui.md)를 참조 하세요.
- 스크롤 뷰에서 [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) 를 사용 하도록 설정 하 여 스크롤 뷰를 사용 하 여 이동 제스처를 캡처 및 공유 합니다. 자세한 내용은 [iOS에서 동시 이동 제스처 인식](application-pan-gesture.md)(영문)을 참조 하세요.

## <a name="ios-specific-formatting"></a>iOS 관련 서식 지정

Xamarin.ios를 사용 하면 플랫폼 간 사용자 인터페이스 스타일과 색을 설정할 수 있지만 iOS 프로젝트에서 플랫폼 Api를 사용 하 여 iOS의 테마를 설정 하는 다른 옵션도 있습니다.

**Info.plist** 구성 및 `UIAppearance` API와 같은 iOS 관련 api를 사용 하 여 사용자 인터페이스 서식 지정에 대해 [자세히](formatting.md) 알아보세요.

![](images/status-white-sml.png "iOS Theming")

## <a name="other-ios-features"></a>기타 iOS 기능

[사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md), [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)및 [messagingcenter](~/xamarin-forms/app-fundamentals/messaging-center.md)를 사용 하 여 다양 한 네이티브 기능을 iOS 용 xamarin.ios 응용 프로그램에 통합할 수 있습니다.
