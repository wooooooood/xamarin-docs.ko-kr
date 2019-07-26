---
title: Xamarin.Android Designer Basics
description: 이 항목에서는 Xamarin.ios Android Designer 기능을 소개 하 고, 디자이너를 시작 하는 방법을 설명 하 고, Design Surface에 대해 설명 하며, 속성 창을 사용 하 여 위젯 속성을 편집 하는 방법에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 48B20C9A-B2A2-AE82-76B2-A3C1E5A4050D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/05/2018
ms.openlocfilehash: 383a49f9baa85d50c956efbdd2ce29e3d62977b4
ms.sourcegitcommit: c75c1d2132a4f46a7b38e454d5f24705165026bd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68485937"
---
# <a name="xamarinandroid-designer-basics"></a>Android Designer 기본 사항

_이 항목에서는 Xamarin.ios Android Designer 기능을 소개 하 고, 디자이너를 시작 하는 방법을 설명 하 고, Design Surface에 대해 설명 하며, 속성 창을 사용 하 여 위젯 속성을 편집 하는 방법에 대해 설명 합니다._


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="launching-the-designer"></a>디자이너 시작

디자이너는 레이아웃을 만들 때 자동으로 시작 되거나 기존 레이아웃 파일을 두 번 클릭 하 여 시작할 수 있습니다. 예를 들어 **리소스 > 레이아웃** 폴더에서 **activity_main** 를 두 번 클릭 하면 다음 스크린샷에 표시 된 것 처럼 디자이너가 로드 됩니다.

[![Visual Studio의 디자이너 화면](designer-basics-images/vs/01-open-designer-sml.png)](designer-basics-images/vs/01-open-designer.png#lightbox)

마찬가지로 **솔루션 탐색기** 의 **레이아웃** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **> 새 항목 추가 ...를 선택 하 여 새 레이아웃을 추가할 수 있습니다. > Android 레이아웃**:

[![새 항목 추가 대화 상자](designer-basics-images/vs/02-add-new-layout-sml.png)](designer-basics-images/vs/02-add-new-layout.png#lightbox)

그러면 새 **. axml** 레이아웃 파일이 생성 되어 디자이너에 로드 됩니다.

> [!TIP]
> Visual Studio의 최신 릴리스에서는 Android Designer 내에서 .xml 파일을 여는 것을 지원 합니다.
>
> . Axml 및 .xml 파일은 모두 Android Designer에서 지원 됩니다.

## <a name="designer-features"></a>디자이너 기능

디자이너는 다음 스크린샷에 표시 된 것 처럼 다양 한 기능을 지 원하는 여러 섹션으로 구성 됩니다.

[![디자이너 창 다이어그램](designer-basics-images/vs/03-designer-features-sml.png)](designer-basics-images/vs/03-designer-features.png#lightbox)

디자이너에서 레이아웃을 편집 하는 경우 다음 기능을 사용 하 여 디자인을 만들고 모양을 만듭니다.

-   **Design Surface** &ndash; 는 레이아웃을 장치에 표시 하는 방법을 편집할 수 있는 방법을 제공 하 여 사용자 인터페이스의 시각적 생성을 용이 하 게 합니다. **Design Surface** **디자인 창** 안에 표시 됩니다 (위에 있는 **디자이너 도구 모음** 포함).

-   **소스 창** Design Surface에 표시 된 디자인에 해당 하는 기본 XML 원본에 대 한 뷰를 제공 합니다.  &ndash;

-   **디자이너 도구 모음** &ndash; 선택기 목록을 표시 합니다. **장치**, **버전**, **테마**, 레이아웃 구성 및 작업 모음 설정입니다. **디자이너 도구 모음** 에는 테마 편집기를 시작 하 고 재질 디자인 그리드를 사용 하기 위한 아이콘도 포함 되어 있습니다.

-   **도구 상자** Design Surface로 끌어서 놓을 수 있는 위젯 및 레이아웃 목록을 제공 합니다.  &ndash;

-   **속성 창** &ndash; 보기 및 편집을 위해 선택한 위젯의 속성을 나열 합니다.

-   **문서 개요** &ndash; 레이아웃을 구성 하는 위젯의 트리를 표시 합니다. 트리의 항목을 클릭 하 여 **Design Surface**에서 선택할 수 있습니다. 또한 트리에서 항목을 클릭 하면 항목의 속성이 **속성** 창에 로드 됩니다.

## <a name="design-surface"></a>디자인 화면

Designer를 사용 하면 도구 상자에서 **Design Surface**로 위젯을 끌어서 놓을 수 있습니다. 디자이너에서 위젯과 상호 작용 하는 경우 (새 위젯을 추가 하거나 기존 위젯을 위치를 조정 하 여) 사용 가능한 삽입 지점을 표시 하는 세로 및 가로 선이 표시 됩니다. 다음 예에서는 새 `Button` 위젯을 **Design Surface**로 끕니다.

[![Design Surface 삽입 줄의 예](designer-basics-images/vs/05-insertion-points-sml.png)](designer-basics-images/vs/05-insertion-points.png#lightbox)

또한 위젯을 복사할 수 있습니다. 즉, 복사 및 붙여넣기를 사용 하 여 위젯을 복사 하거나 <kbd>ctrl</kbd> 키를 누르는 동안 기존 위젯을 끌어서 놓을 수 있습니다.

### <a name="designer-toolbar"></a>디자이너 도구 모음

**디자이너 도구 모음** ( **Design Surface**위에 배치)은 구성 선택기 및 도구 메뉴를 제공 합니다.

[![디자이너 도구 모음 다이어그램](designer-basics-images/vs/04-toolbar-sml.png)](designer-basics-images/vs/04-toolbar.png#lightbox)

**디자이너 도구 모음은** 다음 기능에 대 한 액세스를 제공 합니다.

-   **대체 레이아웃 선택기** &ndash; 다른 레이아웃 버전에서 선택할 수 있습니다.

-   **장치 선택기** &ndash; 특정 장치와 관련 된 한정자 집합 (예: 화면 크기, 해상도 및 키보드 가용성)을 정의 합니다. 새 장치를 추가 하 고 삭제할 수도 있습니다.

-   **Android 버전 선택기** &ndash; 레이아웃을 대상으로 하는 Android 버전입니다. 디자이너에서 선택한 Android 버전에 따라 레이아웃이 렌더링 됩니다.

-   **테마 선택기** &ndash; 레이아웃에 대 한 UI 테마를 선택 합니다.

-   **구성 선택기** 세로 또는 *가로*와 같은 장치 구성을 선택 합니다.  &ndash;

-   **리소스 한정자 옵션**    언어, UI 모드, 야간 모드 및 둥근 화면 옵션을 선택할 드롭다운 메뉴를 표시 하는 대화 상자를 엽니다. &ndash;

-   **작업 모음 설정** &ndash; 레이아웃에 대 한 작업 모음 설정을 구성 합니다.

-   **테마 편집기** 테마 편집기를 엽니다 .이 편집기를 사용 하면 선택한 테마의 요소를 사용자 지정할 수 있습니다.  &ndash;

-   **재질 디자인 그리드** *재질 디자인 그리드*를 사용 하거나 사용 하지 않도록 설정 합니다. &ndash; 재질 디자인 그리드 옆에 있는 드롭다운 메뉴 항목은 표를 사용자 지정할 수 있는 대화 상자를 엽니다.

이러한 각 기능은 다음 항목에서 자세히 설명 합니다.

-   [리소스 한정자 및 시각화 옵션](~/android/user-interface/android-designer/resource-qualifiers.md) 은 **장치 선택기**, **Android 버전 선택기**, **테마 선택기**, **구성 선택기**, **리소스 자격에 대 한 자세한 정보를 제공 합니다. 옵션**및 **작업 모음 설정**

-   대체 레이아웃 [보기](~/android/user-interface/android-designer/alternative-layout-views.md) 는 **대체 레이아웃 선택기**를 사용 하는 방법을 설명 합니다.

-   [Xamarin.ios Android Designer 재질 디자인 기능은](~/android/user-interface/android-designer/material-design-features.md) **테마 편집기** 와 **재질 디자인 모눈**의 포괄적인 개요를 제공 합니다.

### <a name="context-menu-commands"></a>상황에 맞는 메뉴 명령

상황에 맞는 메뉴는 **Design Surface** 와 **문서 개요**에서 모두 사용할 수 있습니다. 이 메뉴는 선택한 위젯 및 해당 컨테이너에 사용할 수 있는 명령을 표시 하 여 컨테이너에 대 한 작업을 더 쉽게 수행할 수 있도록 합니다 ( **Design Surface**에서 선택 하기가 쉽지 않음). 상황에 맞는 메뉴의 예는 다음과 같습니다.

[![Design Surface를 마우스 오른쪽 단추로 클릭 하는 상황에 맞는 메뉴 예제](designer-basics-images/vs/06-context-menu-sml.png)](designer-basics-images/vs/06-context-menu.png#lightbox)

이 예제에서를 `TextView` 마우스 오른쪽 단추로 클릭 하면 여러 가지 옵션을 제공 하는 상황에 맞는 메뉴가 열립니다.

-   **LinearLayout** 의 부모를 `LinearLayout` 편집 하기 위한 하위 메뉴를 엽니다. `TextView` &ndash;

-   마우스 오른쪽 단추 클릭 &ndash;   에적용되는삭제,복사및`TextView`잘라내기 작업입니다.


### <a name="zoom-controls"></a>확대/축소 컨트롤

**Design Surface** 는 다음과 같이 여러 컨트롤을 통한 확대/축소를 지원 합니다.

[![Design Surface zoom 컨트롤의 다이어그램](designer-basics-images/vs/07-zoom-controls-sml.png)](designer-basics-images/vs/07-zoom-controls.png#lightbox)

이러한 컨트롤을 통해 디자이너에서 사용자 인터페이스의 특정 영역을 쉽게 확인할 수 있습니다.

-   **컨테이너 강조 표시** 확대 및 축소 하는 동안 쉽게 찾을 수 있도록 Design Surface의 컨테이너를 강조 표시 합니다.  &ndash;

-   **보통 크기** &ndash; 선택한 장치의 해상도에서 레이아웃이 어떻게 보이는지 확인할 수 있도록 픽셀의 레이아웃 픽셀을 렌더링 합니다.

-   **창에 맞춤** &ndash; 전체 레이아웃이 Design Surface 표시 되도록 확대/축소 수준을 설정 합니다.

-   **확대** &ndash; 각 클릭에 대해 증분식으로 확대 하 여 레이아웃을 확대 합니다.

-   **축소** &ndash; 각 클릭을 사용 하 여 증분식으로 축소 하 여 Design Surface에서 레이아웃을 작게 표시 합니다.

선택 된 확대/축소 설정은 런타임에 응용 프로그램의 사용자 인터페이스에 영향을 주지 않습니다.

## <a name="switching-between-design-and-source-panes"></a>디자인 창과 소스 창 간 전환

**디자인** 창과 **소스** 창 간의 가운데 스트립에는 **디자인** 창과 **소스** 창이 표시 되는 방식을 수정 하는 데 사용 되는 여러 단추가 있습니다.

[![창 표시 단추 위치](designer-basics-images/vs/25-pane-buttons-sml.png)](designer-basics-images/vs/25-pane-buttons.png#lightbox)

이러한 단추는 다음을 수행 합니다.

-   **디자인** 이 맨 위 단추 **디자인**은 디자인 창을 선택 합니다.  &ndash; 이 단추를 클릭 하면 **도구 상자** 및 **속성** 창이 활성화 되 고 **텍스트 편집기 도구 모음이** 표시 되지 않습니다. **축소** 단추를 클릭 하면 (아래 참조) **소스** 창이 없으면 **디자인** 창이 단독으로 표시 됩니다.

-   **창 바꾸기**     이 단추 (반대 방향 양방향 화살표)는 소스 창이 왼쪽에 있고 디자인 창이 오른쪽에 오도록 디자인 창과 소스 창을 바꿉니다. &ndash; 다시 클릭 하면 이러한 창이 원래 위치로 다시 바뀝니다.

-   **원본** 이 단추 (꺾쇠 괄호 두 개와 유사)는 원본 창을 선택 합니다.  &ndash; 이 단추를 클릭 하면 **도구 상자** 및 **속성** 창이 사용 하지 않도록 설정 되 고 **텍스트 편집기 도구 모음이** Visual Studio 위쪽에 표시 됩니다. **축소** 단추를 클릭할 때 (아래 참조) **원본** 단추를 클릭 하면 **디자인** 창 대신 **소스** 창이 표시 됩니다.

-   **세로 분할** 세로 막대와 유사 하 게이 단추를 클릭 하면 **디자인** 창과 소스 창이 나란히 표시 됩니다.  &ndash; 이는 기본 정렬입니다.

-   **가로 분할** 이 단추 (가로 막대와 유사)는 소스 창 위에 **디자인** 창을 표시 합니다.  &ndash; 창 **바꾸기** 를 클릭 하면 **디자인** 창 위에 **소스** 창이 추가 됩니다.

-   **창 축소** 이 단추 (오른쪽 꺾쇠 괄호 두 개와 유사)는 디자인 및 **소스의** 이중 창 표시를 이러한 창 중 하나의 단일 뷰로 "축소" 합니다.  &ndash;
    이 단추는 두 개의 왼쪽 꺾쇠 괄호와 비슷하게 **확장 창** 단추를 클릭 하 여 이중 창 (**디자인** 및 **원본**) 표시 모드로 다시 돌아갈 수 있습니다.

**축소 창을** 클릭 하면 **디자인** 창만 표시 됩니다. 그러나 **소스 단추를** 클릭 하 여 대신 **원본** 창만 볼 수 있습니다. 디자인 단추 **를** 다시 클릭 하 여 **디자인** 창으로 돌아갑니다.

## <a name="source-pane"></a>소스 창

**소스** 창에는 **Design Surface**에 표시 된 디자인의 기반이 되는 XML 소스가 표시 됩니다. 두 보기를 동시에 사용할 수 있기 때문에 디자인의 시각적 표현과 디자인의 기본 XML 원본 사이를 앞뒤로 이동 하 여 UI 디자인을 만들 수 있습니다.

[![소스 창의 예제 XML 원본](designer-basics-images/vs/22-source-pane-w158-sml.png)](designer-basics-images/vs/22-source-pane-w158.png#lightbox)

XML 원본에 대 한 변경 내용은 **Design Surface**에 즉시 렌더링 됩니다. **Design Surface** 에 대 한 변경 내용으로 인해 **소스** 창에 표시 되는 XML 소스가 그에 따라 업데이트 됩니다. **소스** 창에서 xml을 변경 하는 경우 다음에 설명 된 대로 자동 완성 및 IntelliSense 기능을 사용 하 여 XML 기반 UI 개발을 빠르게 수행할 수 있습니다.

긴 XML 파일로 작업할 때 쉽게 탐색할 수 있도록 **소스** 창에서 Visual Studio 스크롤 막대를 지원 합니다 (이전 스크린샷에서 오른쪽에 표시 됨). 스크롤 막대에 대 한 자세한 내용은 [스크롤 막대를 사용자 지정 하 여 코드를 추적 하는 방법](https://msdn.microsoft.com/library/dn237345.aspx)을 참조 하세요.


### <a name="autocompletion"></a>완성

위젯에 대 한 특성의 이름을 입력 하기 시작할 때 <kbd>ctrl + SPACE</kbd> 를 눌러 가능한 완료 목록을 볼 수 있습니다. 예를 들어 다음 예제 `android:lay` 를 입력 한 후 <kbd>CTRL + SPACE</kbd>를 입력 하면 다음 목록이 표시 됩니다.

[![레이아웃 특성의 자동 완성](designer-basics-images/vs/23-autocompletion-w158-sml.png)](designer-basics-images/vs/23-autocompletion-w158.png#lightbox)

<kbd>Enter</kbd> 키를 눌러 처음에 나열 된 완료를 수락 하거나 화살표 키를 사용 하 여 원하는 완료를 스크롤하고 <kbd>enter</kbd>키를 누릅니다. 또는 마우스를 사용 하 여 스크롤하여 원하는 완료를 클릭할 수 있습니다.

### <a name="intellisense"></a>IntelliSense

위젯의 새 특성을 입력 하 고 값을 할당 하기 시작 하면 트리거 문자를 입력 한 후 IntelliSense가 표시 되 고 해당 특성에 사용할 유효한 값 목록이 제공 됩니다. 예를 들어 다음 예제에서에 대 한 `android:layout_width` 첫 번째 큰따옴표를 입력 한 후에는이 너비에 대해 유효한 선택 항목 목록을 제공 하는 자동 완성 선택기 팝업이 표시 됩니다.

[![레이아웃 너비를 위한 IntelliSense 예제](designer-basics-images/vs/24-intellisense-w158-sml.png)](designer-basics-images/vs/24-intellisense-w158.png#lightbox)

위의 스크린 샷에서 빨간색으로 표시 된 두 개의 단추가이 팝업의 아래쪽에 표시 됩니다. 왼쪽의 **프로젝트 리소스** 단추를 클릭 하면 앱 프로젝트에 포함 되는 리소스로 목록이 제한 되지만 오른쪽의 **프레임 워크 리소스** 단추를 클릭 하면 프레임 워크에서 사용할 수 있는 리소스를 표시 하도록 목록이 제한 됩니다.
이러한 단추를 설정 하거나 해제 합니다 .이 단추를 다시 클릭 하 여 각에서 제공 하는 필터링 작업을 사용 하지 않도록 설정할 수 있습니다.



## <a name="properties-pane"></a>속성 창

디자이너는 **속성** 창을 통해 위젯 속성을 편집할 수 있도록 지원 합니다. 

![속성 창의 스크린샷](designer-basics-images/vs/08-property-pad.png)

**속성** 창에 나열 된 속성은 **Design Surface**에서 선택 된 위젯에 따라 달라 집니다.

### <a name="default-values"></a>기본값

속성이 선택 된 Android 테마에서 상속 되기 때문에 대부분의 widget 속성은 **속성** 창에서 비어 있습니다.
**속성** 창에는 선택한 위젯에 대해 명시적으로 설정 된 값만 표시 됩니다. 테마에서 상속 된 값은 표시 되지 않습니다.

### <a name="referencing-resources"></a>리소스 참조

일부 속성은 레이아웃 **. axml** 파일 이외의 파일에 정의 된 리소스를 참조할 수 있습니다. 이 형식의 `string` 가장 일반적인 사례는 및 `drawable` 리소스입니다. 그러나 `Boolean` 값 및 차원과 같은 다른 리소스에도 참조를 사용할 수 있습니다. 속성이 리소스 참조를 지 원하는 경우 속성에 대 한 텍스트 항목 옆에 찾아보기 아이콘 (사각형)이 표시 됩니다. 이 단추를 클릭 하면 리소스 선택 기가 열립니다.

예를 들어 다음 스크린샷에서는 `Text` **속성** 창에서 위젯의 텍스트 필드 오른쪽에 있는 어두운 사각형을 클릭할 때 사용할 수 있는 옵션을 보여 줍니다.

[![텍스트 옵션의 예제 목록](designer-basics-images/vs/09-text-options-sml.png)](designer-basics-images/vs/09-text-options.png#lightbox)

**리소스 ...** 를 클릭 하면 **리소스 선택** 대화 상자가 표시 됩니다.

[![여러 리소스가 나열 된 예제 리소스 스크린샷](designer-basics-images/vs/09b-resources-w158-sml.png)](designer-basics-images/vs/09b-resources-w158.png#lightbox)

이 목록에서 **속성** 창의 텍스트를 하드 코딩 하는 대신 해당 위젯에 사용할 텍스트 리소스를 선택할 수 있습니다. 다음 예제에서는 `Src` `ImageView`의 속성에 대 한 리소스 선택기를 보여 줍니다.

[![ImageView의 리소스 선택기 목록 아이콘 리소스](designer-basics-images/vs/10-src-resource-sml.png)](designer-basics-images/vs/10-src-resource.png#lightbox)

`Src` 속성의 오른쪽에 있는 빈 사각형을 클릭 하면 **리소스 선택** 대화 상자가 열리고, 위에 표시 된 색에서 drawables에 이르는 리소스 목록이 표시 됩니다.


### <a name="boolean-property-references"></a>부울 속성 참조

*부울* 속성은 일반적으로 속성 창의 속성 옆에 있는 확인 표시로 선택 됩니다. 이 확인란을 선택 `true` 하거나 `false` 선택 취소 하 여 또는 값을 지정 하거나, 속성의 오른쪽에 있는 짙은 채워진 사각형을 클릭 하 여 속성 참조를 선택할 수 있습니다. 다음 예에서는 선택한 `TextView`와 연결 된 **모든 대문자로** 된 부울 속성 참조 텍스트를 클릭 하 여 텍스트를 모두 대문자로 변경 합니다.

![부울 속성을 설정 하는 예](designer-basics-images/vs/11-boolean.png)

## <a name="editing-properties-inline"></a>속성 인라인 편집

Android Designer **Design Surface** 의 특정 속성에 대 한 직접 편집을 지원 하므로 속성 목록에서 이러한 속성을 검색할 필요가 없습니다. 직접 편집할 수 있는 속성에는 텍스트, 여백 및 크기가 포함 됩니다.

### <a name="text"></a>텍스트

일부 위젯의 텍스트 속성 (예: `Button` 및 `TextView`)은 **Design Surface**에서 직접 편집할 수 있습니다. 위젯을 두 번 클릭 하면 아래와 같이 편집 모드로 전환 됩니다.

![Hello 문자열의 텍스트 리소스](designer-basics-images/vs/12-text-resource.png "텍스트 리소스")

새 텍스트 값을 입력 하거나 새 리소스 문자열을 입력할 수 있습니다. 다음 예제에서 리소스는 `@string/hello` `CLICK THIS BUTTON`텍스트 (:)로 대체 됩니다.

![텍스트를 새 리소스에 자동으로 연결 하려면 Shift + Enter](designer-basics-images/vs/13-shift-enter-resource.png)

이 변경 내용은 위젯의 `text` 속성에 저장 되며 `@string/hello` 리소스에 할당 된 값을 수정 하지 않습니다.
새 텍스트 문자열에서 키를 누르면 <kbd>Shift</kbd> +
<kbd>enter</kbd> 를 눌러 입력 한 텍스트를 새 리소스에 자동으로 연결할 수 있습니다.

### <a name="margin"></a>여백

위젯을 선택 하는 경우 디자이너는 위젯의 크기 또는 여백을 대화형으로 변경할 수 있는 핸들을 표시 합니다. 선택 하는 동안 위젯을 클릭 하면 여백 편집 모드와 크기 편집 모드 간을 전환 합니다.

위젯을 처음 클릭 하면 여백 핸들이 표시 됩니다. 마우스를 핸들 중 하나로 이동 하면 디자이너에서 핸들이 변경 될 속성을 표시 합니다 (아래 `layout_marginLeft` 와 같이 속성의 경우).

![디자이너의 여백 핸들을 보여 주는 스크린샷](designer-basics-images/vs/15-margin-handles.png)

여백이 이미 설정 된 경우 여백이 차지 하는 공간을 나타내는 점선이 표시 됩니다.

![단추 주위의 공간을 표시 하는 점선 예](designer-basics-images/vs/16-margins-set.png)

### <a name="size"></a>크기

앞에서 설명한 것 처럼 이미 선택 되어 있는 위젯을 클릭 하 여 크기 편집 모드로 전환할 수 있습니다. 삼각형 핸들을 클릭 하 여 표시 된 차원의 크기를로 `wrap_content`설정 합니다.

![콘텐츠 줄 바꿈 및 크기 조정 핸들](designer-basics-images/vs/17-wrap-content.png)

**콘텐츠 래핑** 핸들을 클릭 하면 포함 된 콘텐츠를 래핑하는 데 필요한 것 보다 크지 않도록 해당 차원의 위젯을 축소 합니다. 이 예제에서 단추 텍스트는 다음 스크린샷에 표시 된 대로 가로로 축소 됩니다.

크기 값이 **내용 줄 바꿈**으로 설정 된 경우 디자이너는 크기를로 `match_parent`변경 하기 위한 반대 방향을 가리키는 삼각형 핸들을 표시 합니다.

![부모 핸들 일치](designer-basics-images/vs/18-match-parent.png)

부모 핸들 **일치** 를 클릭 하면 해당 차원의 크기가 부모 위젯에 같도록 복원 됩니다.

또한 위의 스크린샷에 표시 된 것 처럼 원형 크기 조정 핸들을 끌어 위젯의 크기를 임의 `dp` 값으로 조정할 수 있습니다. 이렇게 하면 해당 차원에 대 한 **콘텐츠 래핑** 및 **일치 부모** 핸들이 모두 표시 됩니다.

![원형 크기 조정 핸들](designer-basics-images/vs/19-resize-dp.png)

모든 컨테이너가 위젯의를 편집할 수 `Size` 있는 것은 아닙니다. 예를 들어,가 `LinearLayout` 선택 된 상태에서 아래 스크린샷에는 크기 조정 핸들이 표시 되지 않습니다.

![크기 조정 핸들 없음](designer-basics-images/vs/20-no-resize-handles.png)


## <a name="document-outline"></a>문서 개요

**문서 개요** 는 레이아웃의 위젯 계층 구조를 표시 합니다.
다음 예제에서는 포함 하 `LinearLayout` 는 위젯을 선택 합니다.

![문서 개요 예제](designer-basics-images/vs/21-document-outline.png)

선택한 위젯의 개요 (이 경우 `LinearLayout`)도 **Design Surface**강조 표시 됩니다. 문서 개요에서 선택한 위젯은 **Design Surface**와 동기화 된 상태로 유지 됩니다. 이는 보기 그룹을 선택 하는 데 유용 하며 **Design Surface**에서 선택 하기가 쉽지 않습니다.

**문서 개요** 는 복사 및 붙여넣기를 지원 하거나 끌어서 놓기를 사용할 수 있습니다. 끌어서 놓기는 문서 개요에서 **Design Surface** 뿐만 아니라 **문서 개요**의 **Design Surface** 에서 지원 됩니다. 또한 **문서 개요** 에서 항목을 마우스 오른쪽 단추로 클릭 하면 해당 항목에 대 한 상황에 맞는 메뉴가 표시 됩니다. 즉, **Design Surface**에서 동일한 위젯을 마우스 오른쪽 단추로 클릭 하면 표시 되는 상황에 맞는 메뉴가 표시 됩니다.




# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="launching-the-designer"></a>디자이너 시작

레이아웃을 만들거나 기존. axml 파일을 두 번 클릭 하 여 디자이너를 시작할 수 있습니다. 예를 들어 **Resources > Layout** 폴더에서 **Main. axml** 을 두 번 클릭 하면 다음과 같이 디자이너를 로드 합니다.

[![Mac용 Visual Studio의 디자이너 화면](designer-basics-images/xs/01-open-designer-sml.png)](designer-basics-images/xs/01-open-designer.png#lightbox)

마찬가지로 **Solution Pad** 의 **레이아웃** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 파일 > Android > 레이아웃**을 선택 하 여 새 레이아웃을 추가할 수 있습니다.

[![새 파일 추가 대화 상자](designer-basics-images/xs/02-add-new-layout-sml.png)](designer-basics-images/xs/02-add-new-layout.png#lightbox)

그러면 새. axml 파일이 생성 되어 Design Surface에 로드 됩니다.

> [!TIP]
> Visual Studio의 최신 릴리스에서는 Android Designer 내에서 .xml 파일을 여는 것을 지원 합니다.
>
> . Axml 및 .xml 파일은 모두 Android Designer에서 지원 됩니다.

## <a name="designer-features"></a>디자이너 기능

디자이너는 다음 스크린샷에 표시 된 것 처럼 다양 한 기능을 지 원하는 여러 섹션으로 구성 됩니다.

[![디자이너 창 다이어그램](designer-basics-images/xs/03-designer-features-sml.png)](designer-basics-images/xs/03-designer-features.png#lightbox)

디자이너에서 레이아웃을 편집 하는 경우 다음 기능을 사용 하 여 디자인을 만들고 모양을 만듭니다.

-   **Design Surface** &ndash; 는 레이아웃을 장치에 표시 하는 방법을 편집할 수 있는 방법을 제공 하 여 사용자 인터페이스의 시각적 생성을 용이 하 게 합니다.

-   **도구 모음** &ndash; 선택기 목록을 표시 합니다. **장치**, **버전**, **테마**, 레이아웃 구성 및 작업 모음 설정입니다. 도구 모음에는 테마 편집기를 시작 하 고 재질 디자인 그리드를 사용 하기 위한 아이콘도 포함 되어 있습니다.

-   **도구 상자** &ndash; Design Surface로 끌어서 놓을 수 있는 위젯 및 레이아웃 목록을 제공 합니다.

-   **속성 패드** &ndash; 보기 및 편집을 위해 선택한 위젯의 속성을 나열 합니다.

-   **문서 개요** &ndash; 레이아웃을 구성 하는 위젯의 트리를 표시 합니다. 트리의 항목을 클릭 하 여 디자이너에서 선택 되도록 할 수 있습니다. 또한 트리에서 항목을 클릭 하면 항목의 속성이 속성 패드에 로드 됩니다.

## <a name="toolbar"></a>도구 모음

도구 모음 (Design Surface 위에 배치)은 구성 선택기 및 도구 메뉴를 제공 합니다.

[![디자이너 도구 모음 다이어그램](designer-basics-images/xs/04-toolbar-sml.png)](designer-basics-images/xs/04-toolbar.png#lightbox)

도구 모음은 다음 기능에 대 한 액세스를 제공 합니다.

-   **대체 레이아웃 선택기** &ndash; 다른 레이아웃 버전에서 선택할 수 있습니다.

-   **장치 선택기** &ndash; 화면 크기, 해상도, 키보드 가용성 등 특정 장치와 연결 된 한정자 집합을 정의 합니다. 새 장치를 추가 하 고 삭제할 수도 있습니다.

-   **Android 버전 선택기** &ndash; 레이아웃을 대상으로 하는 Android 버전입니다. 디자이너에서 선택한 Android 버전에 따라 레이아웃이 렌더링 됩니다.

-   **테마 선택기** &ndash; 레이아웃에 대 한 UI 테마를 선택 합니다.

-   **구성 선택기** 세로 또는 *가로*와 같은 장치 구성을 선택 합니다.  &ndash;

-   **리소스 한정자 옵션**    언어, UI 모드, 야간 모드 및 둥근 화면 옵션을 선택할 드롭다운 메뉴를 표시 하는 대화 상자를 엽니다. &ndash;

-   **작업 모음 설정** &ndash; 레이아웃에 대 한 작업 모음 설정을 구성 합니다.

-   **테마 편집기** 테마 편집기를 엽니다 .이 편집기를 사용 하면 선택한 테마의 요소를 사용자 지정할 수 있습니다.  &ndash;

-   **재질 디자인 그리드** *재질 디자인 그리드*를 사용 하거나 사용 하지 않도록 설정 합니다. &ndash; 재질 디자인 그리드 옆에 있는 드롭다운 메뉴 항목은 표를 사용자 지정할 수 있는 대화 상자를 엽니다.

이러한 각 기능은 다음 항목에서 자세히 설명 합니다.

[리소스 한정자 및 시각화 옵션](~/android/user-interface/android-designer/resource-qualifiers.md) 은 **장치 선택기**, **Android 버전 선택기**, **테마 선택기**, **구성 선택기**, **리소스 자격에 대 한 자세한 정보를 제공 합니다. 옵션**및 **작업 모음 설정**

대체 레이아웃 [보기](~/android/user-interface/android-designer/alternative-layout-views.md) 는 **대체 레이아웃 선택기**를 사용 하는 방법을 설명 합니다.

[재질 디자인 기능](~/android/user-interface/android-designer/material-design-features.md) **테마 편집기** 와 **재질 디자인 모눈**의 포괄적인 개요를 제공 합니다.

## <a name="design-surface"></a>디자인 화면

Designer를 사용 하면 도구 상자에서 Design Surface로 위젯을 끌어서 놓을 수 있습니다. 디자이너에서 위젯과 상호 작용 하는 경우 (새 위젯을 추가 하거나 기존 위젯을 위치를 조정 하 여) 사용 가능한 삽입 지점을 표시 하는 세로 및 가로 선이 표시 됩니다. 다음 예에서는 새 `Button` 위젯을 Design Surface로 끕니다.

[![Design Surface 삽입 줄의 예](designer-basics-images/xs/05-insertion-points-sml.png)](designer-basics-images/xs/05-insertion-points.png#lightbox)

또한 위젯을 복사할 수 있습니다. 즉, 복사 및 붙여넣기를 사용 하 여 위젯을 복사 하거나 <kbd>ctrl</kbd> 키를 누르는 동안 기존 위젯을 끌어서 놓을 수 있습니다.

### <a name="context-menu-commands"></a>상황에 맞는 메뉴 명령

상황에 맞는 메뉴는 Design Surface와 문서 개요에서 모두 사용할 수 있습니다. 이 메뉴는 선택한 위젯 및 해당 컨테이너에 사용할 수 있는 명령을 표시 하 여 컨테이너에 대 한 작업을 더 쉽게 수행할 수 있도록 합니다 (Design Surface에서 선택 하기가 쉽지 않음). 상황에 맞는 메뉴의 예는 다음과 같습니다.

[![Design Surface를 마우스 오른쪽 단추로 클릭 하는 상황에 맞는 메뉴 예제](designer-basics-images/xs/06-context-menu-sml.png)](designer-basics-images/xs/06-context-menu.png#lightbox)

이 예제에서를 `Button` 마우스 오른쪽 단추로 클릭 하면 여러 가지 옵션을 제공 하는 상황에 맞는 메뉴가 열립니다.

-   **LinearLayout** 의 부모를 `LinearLayout` 편집 하기 위한 하위 메뉴를 엽니다. `Button` &ndash;

-   마우스 오른쪽 단추 클릭 &ndash;   에적용되는잘라내기,복사및`Button`삭제 작업입니다.

### <a name="zoom-controls"></a>확대/축소 컨트롤

Design Surface는 다음과 같이 여러 컨트롤을 통한 확대/축소를 지원 합니다.

[![Design Surface zoom 컨트롤의 다이어그램](designer-basics-images/xs/07-zoom-controls-sml.png)](designer-basics-images/xs/07-zoom-controls.png#lightbox)

이러한 컨트롤을 통해 디자이너에서 사용자 인터페이스의 특정 영역을 쉽게 확인할 수 있습니다.

-   **컨테이너 강조 표시** &ndash; 확대 및 축소 하는 동안 쉽게 찾을 수 있도록 Design Surface의 컨테이너를 강조 표시 합니다.

-   **보통 크기** &ndash; 선택한 장치의 해상도에서 레이아웃이 어떻게 보이는지 확인할 수 있도록 픽셀의 레이아웃 픽셀을 렌더링 합니다.

-   **창에 맞춤** &ndash; 전체 레이아웃이 Design Surface 표시 되도록 확대/축소 수준을 설정 합니다.

-   **확대** &ndash; 각 클릭에 대해 증분식으로 확대 하 여 레이아웃을 확대 합니다.

-   **축소** &ndash; 각 클릭을 사용 하 여 증분식으로 축소 하 여 Design Surface에서 레이아웃을 작게 표시 합니다.

선택 된 확대/축소 설정은 런타임에 응용 프로그램의 사용자 인터페이스에 영향을 주지 않습니다.

## <a name="property-pad"></a>속성 패드

디자이너는 **속성 패드**를 통해 위젯 속성을 편집할 수 있도록 지원 합니다. 속성 패드에 나열 된 속성은 디자이너 화면에서 선택 된 위젯에 따라 변경 됩니다. 이전 예제의 `Button` 를 선택 하면 해당 `Button` 위젯의 속성이 표시 됩니다.

[![속성 패드의 스크린샷](designer-basics-images/xs/08-property-pad-sml.png)](designer-basics-images/xs/08-property-pad.png#lightbox)

## <a name="property-pad-sections"></a>속성 패드 섹션

속성 패드는 비슷한 속성을 함께 &ndash; 그룹화 하는 여러 섹션으로 나뉘어 있으므로 관심 있는 속성을 쉽게 찾을 수 있습니다.

-   **위젯** 위젯의 기본 속성 (예 `id`:, `visibility` `text`, 등) &ndash; 위젯의 콘텐츠를 관리 하기 위한 속성은 일반적으로 여기에 배치 됩니다.

-   **스타일** 위젯의 시각적 모양 (예 `font`:, `text color` `background`, 등)을 변경 하는 속성입니다. &ndash;

-   **레이아웃** &ndash; 위젯의 위치와 크기를 설정 하는 속성입니다.

-   **스크롤** &ndash; 스크롤 속성

-   **동작** &ndash; 위젯이 동작 하는 방법을 설정 하는 플래그입니다.

### <a name="default-values"></a>기본값

속성이 선택 된 Android 테마에서 상속 되므로 대부분 위젯의 속성은 **속성 패드** 에서 비어 있습니다. **속성 패드** 에는 선택한 위젯에 대해 명시적으로 설정 된 값만 표시 됩니다. 테마에서 상속 된 값은 표시 되지 않습니다.

### <a name="referencing-resources"></a>리소스 참조

일부 속성은 레이아웃 **. axml** 파일 이외의 파일에 정의 된 리소스를 참조할 수 있습니다. 이 형식의 `string` 가장 일반적인 사례는 및 `drawable` 리소스입니다. 그러나 `Boolean` 값 및 차원과 같은 다른 리소스에도 참조를 사용할 수 있습니다.
속성이 리소스 참조를 지 원하는 경우 속성에 대 한 텍스트 항목 옆 &hellip;에 찾아보기 아이콘 (줄임표)이 표시 됩니다.
이 단추를 클릭 하면 리소스 선택 기가 열립니다.

예를 들어 다음 스크린샷에서는 `Button` **속성 패드**의 위젯에 대 한 텍스트 필드의 오른쪽에 있는 줄임표 (...)를 클릭할 때 사용할 수 있는 리소스를 보여 줍니다.

[![두 리소스를 나열 하는 예제 리소스 스크린샷](designer-basics-images/xs/09-resources-sml.png)](designer-basics-images/xs/09-resources.png#lightbox)

다음 예제에서는 `Src` `ImageView`의 속성에 대 한 리소스 선택기를 보여 줍니다.

[![ImageView의 리소스 선택기 목록 아이콘 리소스](designer-basics-images/xs/10-src-resource-sml.png)](designer-basics-images/xs/10-src-resource.png#lightbox)

### <a name="boolean-property-references"></a>부울 속성 참조

*부울* 속성은 일반적으로 속성 패드에 확인란으로 표시 됩니다. `Boolean` 속성이 리소스 참조를 지 원하는 경우 속성 옆에 작은 확인란이 표시 됩니다. 선택 된 확인란 `true` 은이 고 빈 상자는 `false`를 의미 합니다. `true` 또는`false`와 같은 값을 직접 입력할 수도 있습니다. 입력 위로 마우스를 가져가면 작은 텍스트 필드 아이콘이 표시 됩니다. 값을 수동으로 입력 하려는 경우이를 클릭 하면 됩니다.

[![부울 속성을 설정 하는 예](designer-basics-images/xs/12-boolean-sml.png)](designer-basics-images/xs/12-boolean.png#lightbox)


## <a name="grouped-properties"></a>그룹화 된 속성

일부 위젯에는 함께 그룹화 되는 다중 값 속성이 있습니다 (예: `Padding`). 이러한 속성 값은 확장 가능한 단일 행의 **속성 패드** 에 나열 됩니다. 이러한 속성 중 일부는 아래에 표시 된 `Padding` 속성과 같이 그룹화 된 행에서 직접 편집할 수 있습니다.

[![패딩 속성의 예제 설정](designer-basics-images/xs/13-padding-property-sml.png)](designer-basics-images/xs/13-padding-property.png#lightbox)

## <a name="editing-properties-inline"></a>속성 인라인 편집

Android Designer Design Surface의 특정 속성에 대 한 직접 편집을 지원 하므로 속성 목록에서 이러한 속성을 검색할 필요가 없습니다. 직접 편집할 수 있는 속성에는 텍스트, 여백 및 크기가 포함 됩니다.

### <a name="text"></a>텍스트

일부 위젯의 텍스트 속성 (예: `Button` 및 `TextView`)은 Design Surface에서 직접 편집할 수 있습니다. 위젯을 두 번 클릭 하면 아래와 같이 편집 모드로 전환 됩니다.

[![Hello 문자열의 텍스트 리소스](designer-basics-images/xs/14-text-resource-sml.png)](designer-basics-images/xs/14-text-resource.png#lightbox)

새 텍스트 값을 입력 하거나 새 리소스 문자열을 입력할 수 있습니다. 다음 예제에서 리소스는 `@string/hello` `CLICK THIS BUTTON`텍스트 (:)로 대체 됩니다.

[![텍스트를 새 리소스에 자동으로 연결 하려면 Shift + Enter](designer-basics-images/xs/15-shift-enter-resource-sml.png)](designer-basics-images/xs/15-shift-enter-resource.png#lightbox)

이 변경 내용은 위젯의 `text` 속성에 저장 되며 `@string/hello` 리소스에 할당 된 값을 수정 하지 않습니다.
새 텍스트 문자열에서 키를 누르면 <kbd>Shift</kbd> +
<kbd>enter</kbd> 를 눌러 입력 한 텍스트를 새 리소스에 자동으로 연결할 수 있습니다.

### <a name="margin"></a>여백

위젯을 선택 하는 경우 디자이너는 위젯의 크기 또는 여백을 대화형으로 변경할 수 있는 핸들을 표시 합니다. 선택 하는 동안 위젯을 클릭 하면 여백 편집 모드와 크기 편집 모드 간을 전환 합니다.

위젯을 처음 클릭 하면 여백 핸들이 표시 됩니다. 마우스를 핸들 중 하나로 이동 하면 디자이너에서 핸들이 변경 될 속성을 표시 합니다 (아래 `layout_marginLeft` 와 같이 속성의 경우).

[![디자이너의 여백 핸들을 보여 주는 스크린샷](designer-basics-images/xs/16-margin-handles-sml.png)](designer-basics-images/xs/16-margin-handles.png#lightbox)

여백이 이미 설정 된 경우 여백이 차지 하는 공간을 나타내는 점선이 표시 됩니다.

[![단추 주위의 공간을 표시 하는 점선 예](designer-basics-images/xs/17-margins-set-sml.png)](designer-basics-images/xs/17-margins-set.png#lightbox)

### <a name="size"></a>크기

앞에서 설명한 것 처럼 이미 선택 되어 있는 위젯을 클릭 하 여 크기 편집 모드로 전환할 수 있습니다. 삼각형 핸들을 클릭 하 여 표시 된 차원의 크기를로 `wrap_content`설정 합니다.

[![콘텐츠 줄 바꿈 및 크기 조정 핸들](designer-basics-images/xs/18-wrap-content-sml.png)](designer-basics-images/xs/18-wrap-content.png#lightbox)

**콘텐츠 래핑** 핸들을 클릭 하면 해당 차원의 위젯이 축소 되어 포함 된 콘텐츠를 래핑하는 데 필요한 보다 크지 않습니다. 이 예제에서 단추 텍스트는 다음 스크린샷에 표시 된 대로 가로로 축소 됩니다.

크기 값이 **내용 줄 바꿈**으로 설정 된 경우 디자이너는 크기를로 `match_parent`변경 하기 위한 반대 방향을 가리키는 삼각형 핸들을 표시 합니다.

[![부모 핸들 일치](designer-basics-images/xs/19-match-parent-sml.png)](designer-basics-images/xs/19-match-parent.png#lightbox)

부모 핸들 **일치** 를 클릭 하면 해당 차원의 크기가 부모 위젯에 같도록 복원 됩니다.

또한 위의 스크린샷에 표시 된 것 처럼 원형 크기 조정 핸들을 끌어 위젯의 크기를 임의 `dp` 값으로 조정할 수 있습니다. 이렇게 하면 해당 차원에 대 한 **콘텐츠 래핑** 및 **일치 부모** 핸들이 모두 표시 됩니다.

[![원형 크기 조정 핸들](designer-basics-images/xs/20-resize-dp-sml.png)](designer-basics-images/xs/20-resize-dp.png#lightbox)

모든 컨테이너가 위젯의를 편집할 수 `Size` 있는 것은 아닙니다. 예를 들어,가 `LinearLayout` 선택 된 상태에서 아래 스크린샷에는 크기 조정 핸들이 표시 되지 않습니다.

[![크기 조정 핸들 없음](designer-basics-images/xs/21-no-resize-handles-sml.png)](designer-basics-images/xs/20-no-resize-handles.png#lightbox)

## <a name="document-outline"></a>문서 개요

**문서 개요** 는 레이아웃의 위젯 계층 구조를 표시 합니다.
다음 예제에서는 포함 하 `LinearLayout` 는 위젯을 선택 합니다.

[![문서 개요](designer-basics-images/xs/22-outline-view-sml.png)](designer-basics-images/xs/22-outline-view.png#lightbox)

선택한 위젯의 개요 (이 경우 `LinearLayout`)도 Design Surface 강조 표시 됩니다. 문서 개요에서 선택한 위젯은 Design Surface와 동기화 된 상태로 유지 됩니다. 이는 보기 그룹을 선택 하는 데 유용 하며 Design Surface에서 선택 하기가 쉽지 않습니다.

문서 개요는 복사 및 붙여넣기를 지원 하거나 끌어서 놓기를 사용할 수 있습니다. 끌어서 놓기는 문서 개요에서 Design Surface 뿐만 아니라 문서 개요의 Design Surface에서 지원 됩니다. 또한 문서 개요에서 항목을 마우스 오른쪽 단추로 클릭 하면 해당 항목에 대 한 상황에 맞는 메뉴가 표시 됩니다. 즉, Design Surface에서 동일한 위젯을 마우스 오른쪽 단추로 클릭 하면 표시 되는 상황에 맞는 메뉴가 표시 됩니다.

-----
