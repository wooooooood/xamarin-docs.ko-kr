---
title: Android 컨트롤 (위젯)
description: Xamarin Android 사용자 인터페이스를 만들기 위한 구성 요소
ms.prod: xamarin
ms.assetid: B7A82166-B920-4672-B7A2-20DD5E0B5AEF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: 31f6c0dd0d4f5452ebc2cbde0cc44cd9c47eeb9a
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510312"
---
# <a name="xamarinandroid-controls-widgets"></a>Xamarin Android 컨트롤 (위젯)

Xamarin.ios는 Android에서 제공 하는 모든 네이티브 사용자 인터페이스 컨트롤 (위젯)을 노출 합니다. 이러한 컨트롤은 Android Designer를 사용 하 여 Xamarin Android 앱에 쉽게 추가 하거나 XML 레이아웃 파일을 통해 프로그래밍 방식으로 추가할 수 있습니다. 선택 하는 방법에 관계 없이 Xamarin Android는의 모든 사용자 인터페이스 개체 속성 및 메서드를 노출 합니다 C#. 다음 섹션에서는 가장 일반적인 Android 사용자 인터페이스 컨트롤을 소개 하 고 Xamarin Android 앱에 통합 하는 방법을 설명 합니다.

## <a name="action-barandroiduser-interfacecontrolsaction-barmd"></a>[작업 모음](~/android/user-interface/controls/action-bar.md) 

`ActionBar`는 활동 제목, 탐색 인터페이스 및 기타 대화형 항목을 표시 하는 도구 모음입니다. 일반적으로 작업 모음은 작업 창의 맨 위에 나타납니다.

![예 ActionBar](images/action-bar.png)


## <a name="auto-completeandroiduser-interfacecontrolsauto-completemd"></a>[자동 완성](~/android/user-interface/controls/auto-complete.md)

`AutoCompleteTextView`사용자가 입력 하는 동안 완성 제안을 자동으로 표시 하는 편집 가능한 텍스트 뷰 요소입니다. 사용자가 편집 상자의 내용을 바꿀 항목을 선택할 수 있는 드롭다운 메뉴에 제안 목록이 표시 됩니다.

![자동 완성 예제](images/auto-complete.png)


## <a name="buttonsandroiduser-interfacecontrolsbuttonsindexmd"></a>[단추](~/android/user-interface/controls/buttons/index.md)

단추는 사용자가 작업을 수행 하기 위해 탭 하는 UI 요소입니다.

![예제 단추](images/buttons.png)


## <a name="calendarandroiduser-interfacecontrolscalendarmd"></a>[일정](~/android/user-interface/controls/calendar.md)

클래스 `Calendar` 는 특정 인스턴스 (epoch에서 오프셋 되는 밀리초 값)를 연도, 월, 시간, 날짜 및 다음 주 날짜와 같은 값으로 변환 하는 데 사용 됩니다.
`Calendar`는 이벤트, 참석자 및 미리 알림을 읽고 쓰는 기능을 포함 하 여 일정 데이터를 통해 다양 한 상호 작용 옵션을 지원 합니다. 응용 프로그램에서 달력 공급자를 사용 하 여 API를 통해 추가 하는 데이터는 Android와 함께 제공 되는 기본 제공 일정 앱에 표시 됩니다.

![예 달력](images/calendar.png)


## <a name="cardviewandroiduser-interfacecontrolscard-viewmd"></a>[CardView](~/android/user-interface/controls/card-view.md)

`CardView`는 카드와 유사한 보기에서 텍스트 및 이미지 콘텐츠를 표시 하는 UI 구성 요소입니다. `CardView`는 모퉁이 및 그림자 `FrameLayout` 가 둥근 위젯에 구현 됩니다. 일반적으로는 `CardView` `ListView` 또는`GridView` 뷰 그룹에 단일 행 항목을 표시 하는 데 사용 됩니다.

![예제 카드 보기](images/cardview.png)


## <a name="edit-textandroiduser-interfacecontrolsedit-textmd"></a>[텍스트 편집](~/android/user-interface/controls/edit-text.md)

`EditText`텍스트를 입력 하 고 수정 하는 데 사용 되는 UI 요소입니다.

![텍스트 편집 예](images/edit-text.png)


## <a name="galleryandroiduser-interfacecontrolsgallerymd"></a>[갤러리](~/android/user-interface/controls/gallery.md)

`Gallery`는 가로 스크롤 목록에 항목을 표시 하는 데 사용 되는 레이아웃 위젯입니다. 뷰의 가운데에 현재 선택 영역을 배치 합니다.

![예제 갤러리](images/gallery.png)


## <a name="navigation-barandroiduser-interfacecontrolsnavigation-barmd"></a>[탐색 모음](~/android/user-interface/controls/navigation-bar.md)

*탐색 모음은* **홈**, **뒤로**및 **메뉴**를 위한 하드웨어 단추가 포함 되지 않은 장치에 대 한 탐색 컨트롤을 제공 합니다.

![예제 탐색 모음](images/navigation-bar.png)


## <a name="pickersandroiduser-interfacecontrolspickersindexmd"></a>[선택기](~/android/user-interface/controls/pickers/index.md)

선택은 사용자가 Android에서 제공 하는 대화 상자를 사용 하 여 날짜 또는 시간을 선택할 수 있는 UI 요소입니다.

![예제 선택](images/picker.png)


## <a name="popup-menuandroiduser-interfacecontrolspopup-menumd"></a>[팝업 메뉴](~/android/user-interface/controls/popup-menu.md)

`PopupMenu`는 특정 뷰에 연결 된 팝업 메뉴를 표시 하는 데 사용 됩니다.

![팝업 메뉴 예](images/popup-menu.png)


## <a name="ratingbarandroiduser-interfacecontrolsratingbarmd"></a>[RatingBar](~/android/user-interface/controls/ratingbar.md)

는 `RatingBar` 별 등급을 표시 하는 UI 요소입니다.

![RatingBar의 예](ratingbar-images/01-ratingbar.png)


## <a name="spinnerandroiduser-interfacecontrolsspinnermd"></a>[회전자](~/android/user-interface/controls/spinner.md)

`Spinner`는 집합에서 하나의 값을 신속 하 게 선택 하는 방법을 제공 하는 UI 요소입니다. 드롭다운 목록으로 simmilar. 

![예제 회전자](images/spinner.png)


## <a name="switchandroiduser-interfacecontrolsswitchmd"></a>[스위치](~/android/user-interface/controls/switch.md)

`Switch`사용자가 설정 또는 해제와 같은 두 상태를 토글할 수 있도록 하는 UI 요소입니다. `Switch` 기본값은 OFF입니다.

![예제 스위치](images/switch.png)


## <a name="textureviewandroiduser-interfacecontrolstexture-viewmd"></a>[TextureView](~/android/user-interface/controls/texture-view.md)

`TextureView`는 비디오 또는 OpenGL 콘텐츠 스트림을 표시 하기 위해 하드웨어 가속 2D 렌더링을 사용 하는 뷰입니다.

![질감 보기 예](images/texture-view.png)


## <a name="toolbarandroiduser-interfacecontrolstool-barindexmd"></a>[툴바](~/android/user-interface/controls/tool-bar/index.md)

위젯 `Toolbar` (Android 5.0 롤리팝에 도입 됨)은 작업 모음 인터페이스 &ndash; 의 일반화로 간주할 수 있습니다 .이는 작업 모음을 대체 하기 위한 것입니다. 는 `Toolbar` 앱 레이아웃의 어디에서 나 사용할 수 있으며, 작업 모음 보다 훨씬 더 쉽게 사용자 지정할 수 있습니다.

![예제 도구 모음](images/toolbar.png)


## <a name="viewpagerandroiduser-interfacecontrolsview-pagerindexmd"></a>[ViewPager](~/android/user-interface/controls/view-pager/index.md) 

는 `ViewPager` 사용자가 데이터 페이지에서 왼쪽과 오른쪽으로 대칭 이동 하는 데 사용할 수 있는 레이아웃 관리자입니다.

![예제 ViewPager](images/viewpager.png)


## <a name="webviewandroiduser-interfacecontrolsweb-viewmd"></a>[웹 보기](~/android/user-interface/controls/web-view.md)

`WebView`는 웹 페이지를 볼 수 있는 사용자 고유의 창을 만들 수 있는 UI 요소입니다 (또는 전체 브라우저를 개발할 수도 있음).

![예제 웹 보기](images/web-view.png)

