---
title: Xamarin.Forms의 iOS 플랫폼 기능
description: Xamarin.Forms 응용 프로그램에 특정 iOS 기능을 추가 합니다.
ms.prod: xamarin
ms.assetid: 634AB62E-68C8-454C-838B-F1CC4E4E21BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/22/2019
ms.openlocfilehash: 471e09f236be505190ad2c08169bd445dcfca0a3
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61365670"
---
# <a name="ios-platform-features-in-xamarinforms"></a>Xamarin.Forms의 iOS 플랫폼 기능

IOS 용 Xamarin.Forms 응용 프로그램 개발에 Visual Studio가 필요 합니다. 합니다 [요구 사항 페이지](~/get-started/requirements.md) 필수 조건에 대 한 자세한 정보를 포함 합니다.

## <a name="platform-specifics"></a>플랫폼별

플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다.

Xamarin.Forms 뷰, 페이지 및 iOS에서 레이아웃에 대해 다음 플랫폼 특정 기능을 제공 됩니다.

- 에 대 한 지원 흐리게 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다. 자세한 내용은 [iOS에서 VisualElement 흐리게](visualelement-blur.md)합니다.
- 지원 되는 레거시 색 모드 사용 안 함 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다. 자세한 내용은 [VisualElement 레거시 색 모드의 경우 iOS](legacy-color-mode.md)합니다.
- 에 그림자를 사용 하도록 설정 된 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다. 자세한 내용은 [iOS에서 Drop 그림자 VisualElement](visualelement-drop-shadow.md)합니다.

IOS에서 Xamarin.Forms 보기에 대 한 다음과 같은 플랫폼별 기능 제공 됩니다.

- 설정 된 [ `Cell` ](xref:Xamarin.Forms.Cell) 배경색입니다. 자세한 내용은 [iOS에서 셀 배경색](cell-background-color.md)합니다.
- 에 적합 한 텍스트를 입력 하는 보장 된 [ `Entry` ](xref:Xamarin.Forms.Entry) 글꼴 크기를 조정 하 여 합니다. 자세한 내용은 [항목의 글꼴 크기를 iOS](entry-font-size.md)합니다.
- 커서 색 설정 된 [ `Entry` ](xref:Xamarin.Forms.Entry)합니다. 자세한 내용은 [iOS에서 커서 색 항목](entry-cursor-color.md)합니다.
- 제어 여부 [ `ListView` ](xref:Xamarin.Forms.ListView) 스크롤 하는 동안 머리글 셀 float입니다. 자세한 내용은 [iOS에서 ListView 그룹 헤더 스타일](listview-group-header-style.md)합니다.
- 행 애니메이션 비활성화 되는지 여부를 제어 하면 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 항목 컬렉션을 업데이트 하는 중입니다. 자세한 내용은 [iOS에서 ListView 행 애니메이션](listview-row-animations.md)합니다.
- 구분 기호 스타일을 설정 된 [ `ListView` ](xref:Xamarin.Forms.ListView)합니다. 자세한 내용은 [iOS에서 ListView 구분 기호 스타일](listview-separator-style.md)합니다.
- 선택 항목에서 발생 하는 경우 제어는 [ `Picker` ](xref:Xamarin.Forms.Picker)합니다. 자세한 내용은 [iOS에서 선택 항목을 선택할](picker-selection.md)합니다.
- 사용 하도록 설정 합니다 [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) 속성에는 위치에 탭 하 여 설정 될를 [ `Slider` ](xref:Xamarin.Forms.Slider) 끌어 필요가 하는 대신 가로 막대형,는 `Slider` thumb. 자세한 내용은 [슬라이더 위치 조정 컨트롤 탭 iOS](slider-thumb.md)합니다.

IOS에서 Xamarin.Forms 페이지에 대 한 다음과 같은 플랫폼별 기능 제공 됩니다.

- 탐색 모음의 구분 기호를 숨기는 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)합니다. 자세한 내용은 [iOS의 NavigationPage Bar Separator](navigation-bar-separator.md)합니다.
- 탐색 모음 반투명 인지 여부를 제어 합니다. 자세한 내용은 [iOS에서 탐색 모음 반투명도](navigation-bar-translucent.md)합니다.
- 제어 상태 표시줄 텍스트의 색 여부는 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 탐색 모음의 광도 맞게 조정 됩니다. 자세한 내용은 [NavigationPage 막대 텍스트 색 모드 iOS에서](status-bar-text-color.md)합니다.
- 페이지 탐색 모음에서 큰 제목으로 페이지 제목이 표시 되는지 여부를 제어 합니다. 자세한 내용은 [iOS에서 큰 페이지 제목](page-large-title.md)합니다.
- 상태 표시줄 표시 여부 설정 된 [ `Page` ](xref:Xamarin.Forms.Page)합니다. 자세한 내용은 [iOS에서 상태 표시줄 표시 유형 페이지](page-status-bar-visibility.md)합니다.
- 콘텐츠 페이지를 확인 합니다. 모든 iOS 장치에 대 한 안전한 화면 영역에 배치 됩니다. 자세한 내용은 [iOS에서 안전 영역 레이아웃 안내선](page-safe-area-layout.md)합니다.
- IPad에서 모달 페이지 표시 스타일을 설정 합니다. 자세한 내용은 [iPad 모달 페이지 표시 스타일](ipad-page-presentation-style.md)합니다.

IOS에서 Xamarin.Forms 레이아웃에 대해 다음 플랫폼 특정 기능을 제공 됩니다.

- 제어 여부는 [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) 터치 제스처를 처리 하거나 해당 콘텐츠를 전달 합니다. 자세한 내용은 [iOS에서 ScrollView 콘텐츠 터치](scrollview-content-touches.md)합니다.

Xamarin.Forms에 대 한 다음과 같은 플랫폼별 기능 제공 됩니다 [ `Application` ](xref:Xamarin.Forms.Application) iOS 클래스:

- 컨트롤 레이아웃을 사용 하도록 설정 하 고 주 스레드에서 수행할 업데이트를 렌더링 합니다. 자세한 내용은 [iOS에서 주 스레드 컨트롤 업데이트](main-thread-updates-ui.md)합니다.
- 사용을 [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) 스크롤 뷰에서 수집 하 고 스크롤 뷰를 사용 하 여 pan 제스처를 공유 합니다. 자세한 내용은 [iOS에서 동시 팬 제스처 인식](application-pan-gesture.md)합니다.

## <a name="ios-specific-formatting"></a>iOS 별 서식 지정

Xamarin.Forms는 플랫폼 간 사용자 인터페이스 스타일 및 색을 설정할 수 있습니다도 iOS 프로젝트에 플랫폼 Api를 사용 하 여 iOS의 테마를 설정 하기 위한 다른 옵션이 있습니다.

[자세히 알아보세요](formatting.md) 와 같은 특정 iOS Api를 사용 하 여 사용자 인터페이스 형식에 대 한 **Info.plist** 구성 및 `UIAppearance` API.

![](images/status-white-sml.png "iOS 테마 설정")

## <a name="other-ios-features"></a>다른 iOS 기능

사용 하 여 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)의 [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md), 및 [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md), 다양 한 기본 기능을 통합 하는 것이 가능 IOS 용 Xamarin.Forms 응용 프로그램입니다.
