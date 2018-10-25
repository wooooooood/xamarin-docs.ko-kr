---
title: Android 컨트롤 (위젯)
description: Xamarin.Android 사용자 인터페이스를 만들기 위한 구성 요소
ms.prod: xamarin
ms.assetid: B7A82166-B920-4672-B7A2-20DD5E0B5AEF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: 842fb1df2c9cc1aaf1a106687179a3730c2503bd
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "32436715"
---
# <a name="android-controls-widgets"></a>Android 컨트롤 (위젯)

Xamarin.Android는 모든 Android에서 제공 된 기본 사용자 인터페이스 컨트롤 (위젯)를 노출 합니다. Android Designer를 사용 하 여 Xamarin.Android 앱에 또는 XML 레이아웃 파일을 통해 프로그래밍 방식으로 이러한 컨트롤을 쉽게에 추가할 수 있습니다. Xamarin.Android를 선택 하는 방법에 관계 없이 모든 사용자 인터페이스 개체 속성 및 C#에서 메서드를 노출 합니다. 다음 섹션에서는 대부분의 Android 사용자 인터페이스 컨트롤을 소개 하 고 Xamarin.Android 앱에 통합 하는 방법에 설명 합니다.

## <a name="action-barandroiduser-interfacecontrolsaction-barmd"></a>[작업 모음](~/android/user-interface/controls/action-bar.md) 

`ActionBar` 작업 제목, 탐색 인터페이스 및 기타 대화형 항목을 표시 하는 도구 모음이입니다. 일반적으로 작업 모음 활동 창의 위쪽에 표시 됩니다.

![예제 작업 모음](images/action-bar.png)


## <a name="auto-completeandroiduser-interfacecontrolsauto-completemd"></a>[자동 완성](~/android/user-interface/controls/auto-complete.md)

`AutoCompleteTextView` 사용자가 입력 하는 동안 자동으로 완성 제안을 표시 하는 편집 가능한 텍스트 뷰 요소가입니다. 추천 단어 목록은 사용자와 입력란의 내용을 바꾸려면 항목을 선택할 수 있는 메뉴 드롭다운에 표시 됩니다.

![자동 완성의 예제](images/auto-complete.png)


## <a name="buttonsandroiduser-interfacecontrolsbuttonsindexmd"></a>[단추](~/android/user-interface/controls/buttons/index.md)

단추는 사용자가 작업을 수행 하는 UI 요소입니다.

![예제에서는 단추](images/buttons.png)


## <a name="calendarandroiduser-interfacecontrolscalendarmd"></a>[일정](~/android/user-interface/controls/calendar.md)

`Calendar` 클래스의 특정 인스턴스를 변환 하는 데는 연도, 월, 시간, 월, 일 및 다음 주의 날짜와 같은 값으로 시간 (epoch에서 오프셋 되는 밀리초 값).
`Calendar` 다양 한 이벤트, 참석자 및 미리 읽기 및 쓰기 수를 포함 하 여 일정 데이터와 상호 작용 옵션을 지원 합니다. 달력 공급자를 응용 프로그램에서 사용 하 여 API를 통해 추가 하는 데이터는 Android 함께 제공 되는 기본 제공 일정 앱에 표시 됩니다.

![예제에서는 달력](images/calendar.png)


## <a name="cardviewandroiduser-interfacecontrolscard-viewmd"></a>[CardView](~/android/user-interface/controls/card-view.md)

`CardView` 카드와 비슷한 뷰에서 텍스트 및 이미지 콘텐츠를 제공 하는 UI 구성 요소가입니다. `CardView` 으로 구현 되는 `FrameLayout` 둥근된 모서리와 그림자를 사용 하 여 위젯. 일반적으로 `CardView` 의 단일 행 항목을 제공 하는 데 사용 되는 `ListView` 또는 `GridView` 보기 그룹입니다.

![예제에서는 카드 보기](images/cardview.png)


## <a name="edit-textandroiduser-interfacecontrolsedit-textmd"></a>[텍스트 편집](~/android/user-interface/controls/edit-text.md)

`EditText` 입력 텍스트를 수정 하기 위한 사용 되는 UI 요소가입니다.

![텍스트를 편집 하는 예제](images/edit-text.png)


## <a name="galleryandroiduser-interfacecontrolsgallerymd"></a>[갤러리](~/android/user-interface/controls/gallery.md)

`Gallery` 가로 스크롤 목록의 항목을 표시 하는 데 사용 되는 레이아웃 위젯 현재 선택 영역 뷰의 가운데에 배치 됩니다.

![예제 갤러리](images/gallery.png)


## <a name="navigation-barandroiduser-interfacecontrolsnavigation-barmd"></a>[탐색 모음](~/android/user-interface/controls/navigation-bar.md)

합니다 *탐색 모음* 탐색 컨트롤에 대 한 하드웨어 단추를 포함 하지 않는 장치에서 제공 **홈**에 **다시**, 및 **메뉴**합니다.

![예제에서는 탐색 모음](images/navigation-bar.png)


## <a name="pickersandroiduser-interfacecontrolspickersindexmd"></a>[선택기](~/android/user-interface/controls/pickers/index.md)

*선택기* Android에서 제공 되는 대화 상자를 사용 하 여 날짜 또는 시간을 선택할 수 있는 UI 요소입니다.

![예 선택](images/picker.png)


## <a name="popup-menuandroiduser-interfacecontrolspopup-menumd"></a>[팝업 메뉴](~/android/user-interface/controls/popup-menu.md)

`PopupMenu` 특정 보기에 연결 된 팝업 메뉴를 표시 하기 위해 사용 됩니다.

![예제에서는 팝업 메뉴](images/popup-menu.png)


## <a name="ratingbarandroiduser-interfacecontrolsratingbarmd"></a>[RatingBar](~/android/user-interface/controls/ratingbar.md)

`RatingBar` 는 별 등급을 표시 하는 UI 요소입니다.

![RatingBar의 예](ratingbar-images/01-ratingbar.png)


## <a name="spinnerandroiduser-interfacecontrolsspinnermd"></a>[회전자](~/android/user-interface/controls/spinner.md)

`Spinner` 집합에서 하나의 값을 선택 하는 빠른 방법을 제공 하는 UI 요소가입니다. 드롭다운 목록에 simmilar 이며 

![스핀 상자 예제](images/spinner.png)


## <a name="switchandroiduser-interfacecontrolsswitchmd"></a>[스위치](~/android/user-interface/controls/switch.md)

`Switch` 사용자가 같은 두 상태 간의 전환 또는 해제 하도록 허용 하는 UI 요소가입니다. `Switch` 기본값은 OFF입니다.

![예제 스위치](images/switch.png)


## <a name="textureviewandroiduser-interfacecontrolstexture-viewmd"></a>[TextureView](~/android/user-interface/controls/texture-view.md)

`TextureView` 비디오 또는 OpenGL 콘텐츠 스트림을 표시할 수 있도록 2D 렌더링이 하드웨어 가속을 사용 하는 뷰입니다.

![예제에서는 텍스처 보기](images/texture-view.png)


## <a name="toolbarandroiduser-interfacecontrolstool-barindexmd"></a>[툴바](~/android/user-interface/controls/tool-bar/index.md)

합니다 `Toolbar` (Android 5.0 Lollipop에 도입 됨) 하는 위젯을 작업 모음 인터페이스의 일반화로 생각할 수 있습니다 &ndash; 작업 모음을 대체 하기 위한 것입니다. `Toolbar` 인 앱 레이아웃에서는 어디서 나 사용할 수 있으며 작업 모음 보다 훨씬 더 사용자 지정 가능한 것입니다.

![예제에서는 도구 모음](images/toolbar.png)


## <a name="viewpagerandroiduser-interfacecontrolsview-pagerindexmd"></a>[ViewPager](~/android/user-interface/controls/view-pager/index.md) 

`ViewPager` 는 왼쪽과 오른쪽 페이지 데이터를 대칭 이동할 수 있는 레이아웃 관리자.

![예제에서는 ViewPager](images/viewpager.png)


## <a name="webviewandroiduser-interfacecontrolsweb-viewmd"></a>[WebView](~/android/user-interface/controls/web-view.md)

`WebView` 웹 페이지 보기에 대 한 고유한 창을 만드는 (또는 심지어 전체 브라우저를 개발) 할 수 있는 UI 요소가입니다.

![예제 웹 보기](images/web-view.png)

