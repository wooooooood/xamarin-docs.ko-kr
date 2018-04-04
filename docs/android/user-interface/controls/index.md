---
title: 컨트롤
description: Xamarin.Android 사용자 인터페이스를 만들기 위한 구성 요소
ms.prod: xamarin
ms.assetid: B7A82166-B920-4672-B7A2-20DD5E0B5AEF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 8994a8988c0e32e85aedcd9110e3583195843862
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="controls"></a>컨트롤


Xamarin.Android는 Android에서 제공 네이티브 사용자 인터페이스 컨트롤 (위젯)의 모든 노출 합니다. Android 디자이너를 사용 하 여 Xamarin.Android 앱 또는 XML 레이아웃 파일을 통해 프로그래밍 방식으로 이러한 컨트롤 쉽게 추가할 수 있습니다. Xamarin.Android를 선택 하는 방법에 관계 없이 모든 사용자 인터페이스 개체 속성 및 C#에서 메서드를 노출 합니다. 다음 섹션에서는 가장 일반적인 Android 사용자 인터페이스 컨트롤을 소개 하 고 Xamarin.Android 앱에 통합 하는 방법에 설명 합니다.

## <a name="action-barandroiduser-interfacecontrolsaction-barmd"></a>[작업 모음](~/android/user-interface/controls/action-bar.md) 

`ActionBar` 작업 제목, 탐색 인터페이스 및 기타 대화형 항목을 표시 하는 도구 모음이입니다. 일반적으로 작업 모음 활동의 창의 위쪽에 나타납니다.

![예제 작업 모음](images/action-bar.png)


## <a name="auto-completeandroiduser-interfacecontrolsauto-completemd"></a>[자동 완성](~/android/user-interface/controls/auto-complete.md)

`AutoCompleteTextView` 사용자가 입력 하는 동안 자동으로 완료 제안을 표시 하는 편집 가능한 텍스트 뷰 요소가입니다. 제안 목록 사용자 된 입력란의 내용을 바꾸려면 항목을 선택할 수 있는 메뉴 드롭다운에 표시 됩니다.

![자동 완성의 예](images/auto-complete.png)


## <a name="buttonsandroiduser-interfacecontrolsbuttonsindexmd"></a>[단추](~/android/user-interface/controls/buttons/index.md)

단추는 사용자가 작업을 수행할 수 있는 UI 요소입니다.

![예제에서는 단추](images/buttons.png)


## <a name="calendarandroiduser-interfacecontrolscalendarmd"></a>[일정](~/android/user-interface/controls/calendar.md)

`Calendar` 클래스의 특정 인스턴스를 변환에 사용 됩니다 연도, 월, 시, 월, 요일 및 다음 주 날짜 등의 값에는 시간 (epoch에서 5 만큼 오프셋 되는 밀리초 값).
`Calendar` 다양 한 읽기 및 쓰기 이벤트, 참석자 및 알림 기능을 비롯 한 일정 데이터와 상호 작용 옵션을 지원 합니다. 응용 프로그램에서 달력 공급자를 사용 하면 API를 통해 추가 데이터는 Android와 함께 제공 되는 기본 제공 일정 앱에 표시 됩니다.

![예제에서는 달력](images/calendar.png)


## <a name="cardviewandroiduser-interfacecontrolscard-viewmd"></a>[CardView](~/android/user-interface/controls/card-view.md)

`CardView` 뷰에서 카드 유사한 텍스트 및 이미지 콘텐츠를 표시 하는 UI 구성 요소가입니다. `CardView` 로 구현 됩니다는 `FrameLayout` 둥근된 모서리 및 그림자 위젯입니다. 일반적으로 `CardView` 의 단일 행 항목을 표시에 사용 되는 `ListView` 또는 `GridView` 보기 그룹입니다.

![예제 카드 보기](images/cardview.png)


## <a name="edit-textandroiduser-interfacecontrolsedit-textmd"></a>[텍스트 편집](~/android/user-interface/controls/edit-text.md)

`EditText` 입력 하 고 텍스트를 수정 하는 데 사용 되는 UI 요소가입니다.

![텍스트를 편집 하는 예제](images/edit-text.png)


## <a name="galleryandroiduser-interfacecontrolsgallerymd"></a>[갤러리](~/android/user-interface/controls/gallery.md)

`Gallery` 가로 스크롤 막대가 목록의 항목을 표시 하는 데 사용 되는 레이아웃 위젯은 현재 선택 영역 보기의 가운데에 배치 합니다.

![예제 갤러리](images/gallery.png)


## <a name="navigation-barandroiduser-interfacecontrolsnavigation-barmd"></a>[탐색 모음](~/android/user-interface/controls/navigation-bar.md)

*탐색 모음* 탐색 컨트롤에 대 한 하드웨어 단추를 포함 하지 않는 장치에서 제공 **홈**, **다시**, 및 **메뉴**합니다.

![예제 탐색 모음](images/navigation-bar.png)


## <a name="pickersandroiduser-interfacecontrolspickersindexmd"></a>[선택기](~/android/user-interface/controls/pickers/index.md)

*선택* 는 Android에서 제공 되는 대화 상자를 사용 하 여 날짜 또는 시간을 선택 하는 데 사용할 수 있는 UI 요소입니다.

![예제 선택](images/picker.png)


## <a name="popup-menuandroiduser-interfacecontrolspopup-menumd"></a>[팝업 메뉴](~/android/user-interface/controls/popup-menu.md)

`PopupMenu` 특정 보기에 연결 된 팝업 메뉴 표시에 사용 됩니다.

![예제에서는 팝업 메뉴](images/popup-menu.png)


## <a name="spinnerandroiduser-interfacecontrolsspinnermd"></a>[회전자](~/android/user-interface/controls/spinner.md)

`Spinner` 집합에서 값을 하나 선택 하는 빠른 방법을 제공 하는 UI 요소가입니다. Simmilar 드롭 다운 목록에는 

![스핀 상자 예제](images/spinner.png)


## <a name="switchandroiduser-interfacecontrolsswitchmd"></a>[스위치](~/android/user-interface/controls/switch.md)

`Switch` 사용자 같은 두 가지 상태 사이 전환 설정 또는 해제 하도록 허용 하는 UI 요소가입니다. `Switch` 기본값은 OFF입니다.

![예제 스위치](images/switch.png)


## <a name="textureviewandroiduser-interfacecontrolstexture-viewmd"></a>[TextureView](~/android/user-interface/controls/texture-view.md)

`TextureView` 비디오 또는 OpenGL 콘텐츠 스트림을 표시할 수 있도록 2D 하드웨어 가속 렌더링을 사용 하는 뷰입니다.

![예제에서는 텍스처 보기](images/texture-view.png)


## <a name="toolbarandroiduser-interfacecontrolstool-barindexmd"></a>[툴바](~/android/user-interface/controls/tool-bar/index.md)

`Toolbar` (Android 5.0 롤리팝에 도입 된) 위젯 작업 모음 인터페이스의 개념으로 생각할 수 있습니다 &ndash; 작업 모음을 대체 하기 위한 것입니다. `Toolbar` 앱 레이아웃에서 어디서 나 사용할 수 있고 그것이 작업 모음 보다 훨씬 더 사용자 지정할 수 있습니다.

![예제에서는 도구 모음](images/toolbar.png)


## <a name="viewpagerandroiduser-interfacecontrolsview-pagerindexmd"></a>[ViewPager](~/android/user-interface/controls/view-pager/index.md) 

`ViewPager` 는 사용자 데이터의 페이지를 통해 좌우로 대칭 이동 수 있게 하는 레이아웃 관리자.

![예제 ViewPager](images/viewpager.png)


## <a name="webviewandroiduser-interfacecontrolsweb-viewmd"></a>[WebView](~/android/user-interface/controls/web-view.md)

`WebView` 웹 페이지 보기에 대 한 고유한 창을 만들 (또는 개발 하는 전체 브라우저) 할 수 있는 UI 요소가입니다.

![예제 웹 보기](images/web-view.png)

