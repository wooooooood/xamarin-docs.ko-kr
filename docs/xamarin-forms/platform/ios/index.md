---
title: Xamarin.ios의 iOS 플랫폼 기능
description: Xamarin.ios 응용 프로그램에 iOS 관련 기능을 추가 합니다.
ms.prod: xamarin
ms.assetid: 634AB62E-68C8-454C-838B-F1CC4E4E21BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
ms.openlocfilehash: 5d0e289ddeb7eabef6d96c8882c772c704c54b34
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75489728"
---
# <a name="ios-platform-features-in-xamarinforms"></a>Xamarin.ios의 iOS 플랫폼 기능

IOS 용 Xamarin Forms 응용 프로그램을 개발 하려면 Visual Studio가 필요 합니다. [요구 사항 페이지](~/get-started/requirements.md) 에는 필수 구성 요소에 대 한 자세한 정보가 포함 되어 있습니다.

## <a name="platform-specifics"></a>플랫폼 사양

플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다.

다음과 같은 플랫폼별 기능이 iOS의 Xamarin 보기, 페이지 및 레이아웃에 제공 됩니다.

- 에 대 한 지원 흐리게 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다. 자세한 내용은 [iOS에서 Visualelement 흐림 효과](visualelement-blur.md)를 참조 하세요.
- 지원 되는 레거시 색 모드 사용 안 함 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다. 자세한 내용은 [iOS의 Visualelement 레거시 색 모드](legacy-color-mode.md)를 참조 하세요.
- 에 그림자를 사용 하도록 설정 된 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다. 자세한 [내용은 항목을 참조 하세요.](visualelement-drop-shadow.md)

IOS의 Xamarin 양식 보기에 대해 다음과 같은 플랫폼별 기능이 제공 됩니다.

- [`Cell`](xref:Xamarin.Forms.Cell) 배경색 설정 자세한 내용은 [iOS의 셀 배경색](cell-background-color.md)을 참조 하세요.
- 에 적합 한 텍스트를 입력 하는 보장 된 [ `Entry` ](xref:Xamarin.Forms.Entry) 글꼴 크기를 조정 하 여 합니다. 자세한 내용은 [iOS의 항목 글꼴 크기](entry-font-size.md)를 참조 하세요.
- 커서 색 설정 된 [ `Entry` ](xref:Xamarin.Forms.Entry)합니다. 자세한 내용은 [iOS의 항목 커서 색](entry-cursor-color.md)을 참조 하세요.
- 스크롤 하는 동안 머리글 셀의 부동 [`ListView`](xref:Xamarin.Forms.ListView) 여부를 제어 합니다. 자세한 내용은 [iOS의 ListView 그룹 헤더 스타일](listview-group-header-style.md)을 참조 하세요.
- [`ListView`](xref:Xamarin.Forms.ListView) items 컬렉션을 업데이트할 때 행 애니메이션을 사용 하지 않도록 설정할지 여부를 제어 합니다. 자세한 내용은 [iOS의 ListView 행 애니메이션](listview-row-animations.md)을 참조 하세요.
- 구분 기호 스타일을 설정 된 [ `ListView` ](xref:Xamarin.Forms.ListView)합니다. 자세한 내용은 [iOS의 ListView 구분 기호 스타일](listview-separator-style.md)을 참조 하세요.
- 선택 항목에서 발생 하는 경우 제어는 [ `Picker` ](xref:Xamarin.Forms.Picker)합니다. 자세한 내용은 [iOS에서 선택 항목 선택 항목](picker-selection.md)을 참조 하세요.
- 사용 하도록 설정 합니다 [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) 속성에는 위치에 탭 하 여 설정 될를 [ `Slider` ](xref:Xamarin.Forms.Slider) 끌어 필요가 하는 대신 가로 막대형,는 `Slider` thumb. 자세한 내용은 [iOS의 슬라이더 엄지 단추 탭](slider-thumb.md)을 참조 하세요.
- `SwipeView`를 열 때 사용 되는 전환을 제어 합니다. 자세한 내용은 [SwipeView 살짝 밀기 전환 모드](swipeview-swipetransitionmode.md)를 참조 하세요.

IOS의 Xamarin.ios 페이지에 대해 다음과 같은 플랫폼 관련 기능이 제공 됩니다.

- 탐색 모음의 구분 기호를 숨기는 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)합니다. 자세한 내용은 [iOS의 Navigationpage Bar Separator](navigation-bar-separator.md)를 참조 하세요.
- 탐색 모음 반투명 인지 여부를 제어 합니다. 자세한 내용은 [iOS의 탐색 모음 반투명도](navigation-bar-translucent.md)을 참조 하세요.
- 제어 상태 표시줄 텍스트의 색 여부는 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 탐색 모음의 광도 맞게 조정 됩니다. 자세한 내용은 [iOS의 Navigationpage Bar 텍스트 색 모드](status-bar-text-color.md)를 참조 하세요.
- 페이지 탐색 모음에서 큰 제목으로 페이지 제목이 표시 되는지 여부를 제어 합니다. 자세한 내용은 [iOS의 큰 페이지 제목](page-large-title.md)을 참조 하세요.
- [`Page`](xref:Xamarin.Forms.Page)에서 홈 표시기의 표시 여부를 설정 합니다. 자세한 내용은 [iOS의 홈 표시기 표시 유형](page-home-indicator.md)을 참조 하세요.
- 상태 표시줄 표시 여부 설정 된 [ `Page` ](xref:Xamarin.Forms.Page)합니다. 자세한 내용은 [iOS의 페이지 상태 표시줄 표시 유형](page-status-bar-visibility.md)을 참조 하세요.
- 콘텐츠 페이지를 확인 합니다. 모든 iOS 장치에 대 한 안전한 화면 영역에 배치 됩니다. 자세한 내용은 [iOS의 안전 영역 레이아웃 가이드](page-safe-area-layout.md)를 참조 하세요.
- 모달 페이지의 프레젠테이션 스타일 설정 자세한 내용은 [모달 페이지 표시 스타일](page-presentation-style.md)을 참조 하세요.

IOS의 Xamarin.ios 레이아웃에 대해 다음과 같은 플랫폼 관련 기능이 제공 됩니다.

- 제어 여부는 [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) 터치 제스처를 처리 하거나 해당 콘텐츠를 전달 합니다. 자세한 내용은 [ScrollView Content 접촉 In iOS](scrollview-content-touches.md)를 참조 하세요.

IOS의 Xamarin.ios [`Application`](xref:Xamarin.Forms.Application) 클래스에 대해 다음과 같은 플랫폼별 기능이 제공 됩니다.

- 명명 된 글꼴 크기에 대 한 접근성 크기 조정을 비활성화 합니다. 자세한 내용은 [iOS의 명명 된 글꼴 크기에 대 한 접근성 크기 조정](named-font-size-scaling.md)을 참조 하세요.
- 컨트롤 레이아웃을 사용 하도록 설정 하 고 주 스레드에서 수행할 업데이트를 렌더링 합니다. 자세한 내용은 [iOS에서 주 스레드 제어 업데이트](main-thread-updates-ui.md)를 참조 하세요.
- 사용을 [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) 스크롤 뷰에서 수집 하 고 스크롤 뷰를 사용 하 여 pan 제스처를 공유 합니다. 자세한 내용은 [iOS에서 동시 이동 제스처 인식](application-pan-gesture.md)(영문)을 참조 하세요.

## <a name="ios-specific-formatting"></a>iOS 관련 서식 지정

Xamarin.ios를 사용 하면 플랫폼 간 사용자 인터페이스 스타일과 색을 설정할 수 있지만 iOS 프로젝트에서 플랫폼 Api를 사용 하 여 iOS의 테마를 설정 하는 다른 옵션도 있습니다.

**Info.plist** 구성 및 `UIAppearance` api와 같은 IOS 관련 api를 사용 하 여 사용자 인터페이스 서식 지정에 대해 [자세히](formatting.md) 알아보세요.

![](images/status-white-sml.png "iOS Theming")

## <a name="other-ios-features"></a>기타 iOS 기능

[사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md), [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)및 [messagingcenter](~/xamarin-forms/app-fundamentals/messaging-center.md)를 사용 하 여 다양 한 네이티브 기능을 iOS 용 xamarin.ios 응용 프로그램에 통합할 수 있습니다.
