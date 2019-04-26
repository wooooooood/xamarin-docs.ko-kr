---
title: Xamarin.Android Designer Basics
description: 이 항목에서는 Xamarin.Android 디자이너 기능을 소개, 디자이너를 시작 하는 방법에 설명, 디자인 화면에 설명 및 위젯 속성을 편집 하려면 속성 창을 사용 하는 방법을 자세히 설명 합니다.
ms.prod: xamarin
ms.assetid: 48B20C9A-B2A2-AE82-76B2-A3C1E5A4050D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/05/2018
ms.openlocfilehash: fe909d72f3c6d6733318b5dcbd1858a1a9e28b37
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61165483"
---
# <a name="xamarinandroid-designer-basics"></a>Xamarin.Android Designer 기본 사항

_이 항목에서는 Xamarin.Android 디자이너 기능을 소개, 디자이너를 시작 하는 방법에 설명, 디자인 화면에 설명 및 위젯 속성을 편집 하려면 속성 창을 사용 하는 방법을 자세히 설명 합니다._


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="launching-the-designer"></a>디자이너를 시작합니다.

디자이너는 레이아웃을 만들어지거나 기존 레이아웃 파일을 두 번 클릭 하 여 시작할 수 있습니다 하는 경우 자동으로 시작 됩니다. 예를 들어, 두 번 클릭 **activity_main.axml** 에 **리소스 > 레이아웃** 이 스크린샷에 표시 된 것 처럼 폴더 디자이너를 로드 합니다.

[![Visual Studio에서 디자이너 화면](designer-basics-images/vs/01-open-designer-sml.png)](designer-basics-images/vs/01-open-designer.png#lightbox)

마찬가지로, 마우스 오른쪽 단추로 클릭 하 여 새 레이아웃을 추가할 수 있습니다 합니다 **레이아웃** 폴더에는 **솔루션 탐색기** 를 선택 하 고 **추가 > 새 항목... > Android 레이아웃**:

[![새 항목 추가 대화 상자](designer-basics-images/vs/02-add-new-layout-sml.png)](designer-basics-images/vs/02-add-new-layout.png#lightbox)

이 새로 만들고 **.axml** 레이아웃 파일을 디자이너에 로드 합니다.

## <a name="designer-features"></a>디자이너 기능

다음 스크린샷에 표시 된 대로 디자이너는 다양 한 기능을 지 원하는 몇 가지 섹션으로 구성 된:

[![디자이너 창 다이어그램](designer-basics-images/vs/03-designer-features-sml.png)](designer-basics-images/vs/03-designer-features.png#lightbox)

디자이너에서 레이아웃을 편집할 때를 만들고 디자인 셰이프 다음 기능을 사용 합니다.

-   **디자인 화면** &ndash; 레이아웃 장치에 표시 되는 방식에 대 한 편집 가능한 표현이 제공 하 여 사용자 인터페이스의 시각적 생성을 용이 하 게 합니다. 합니다 **디자인 화면** 안에 표시 됩니다는 **디자인 창** (사용 하 여 합니다 **디자이너 도구 모음** 위에 배치).

-   **소스 창** &ndash; 보기에 표시 된 디자인에 해당 하는 내부 XML 소스를 제공 합니다 **디자인 화면**합니다.

-   **디자이너 도구 모음** &ndash; 선택기의 목록을 표시 합니다. **장치**, **버전**합니다 **테마**, 레이아웃 구성과 작업 모음 설정 합니다. 합니다 **디자이너 도구 모음** 재질 디자인 눈금을 사용 하도록 설정 하는 것에 대 한 테마 편집기를 시작 하는 것에 대 한 아이콘이 포함 되어 있습니다.

-   **도구 상자** &ndash; 위젯 및 레이아웃을 끌어서 놓을 수 목록을 제공 합니다 **디자인 화면**합니다.

-   **속성 창** &ndash; 보기 및 편집에 대 한 선택한 위젯에의 속성을 나열 합니다.

-   **문서 개요** &ndash; 트리 레이아웃을 구성 하는 위젯 수를 표시 합니다. 선택 하면 트리에서 항목을 클릭할 수는 **디자인 화면**합니다. 또한 항목의 속성을 로드 트리에서 항목을 클릭 하 여 **속성** 창입니다.

## <a name="design-surface"></a>디자인 화면

디자이너를 사용 하면 도구 상자에서 끌어서 위젯 수를 **디자인 화면**합니다. (새 위젯 추가 하거나 기존 위치)에서 위젯 디자이너에서 상호 작용을 사용할 수 있는 삽입 지점을 표시 하려면 세로 선과 가로 선 표시 됩니다. 다음 예제에서는에서 새 `Button` 위젯 위치로 끌 합니다 **디자인 화면**:

[![디자인 화면에서 예제 삽입 선](designer-basics-images/vs/05-insertion-points-sml.png)](designer-basics-images/vs/05-insertion-points.png#lightbox)

또한 위젯을 복사할 수 있습니다: 복사를 사용할 수 있으며 위젯을 복사 하 여 붙여넣기 끌어서 누른 채는 기존 위젯 합니다 <kbd>CTRL</kbd> 키입니다.

### <a name="designer-toolbar"></a>디자이너 도구 모음

합니다 **디자이너 도구 모음** (위에 배치 합니다 **디자인 화면**) 구성 선택기 및 도구 메뉴 표시:

[![다이어그램 디자이너 도구 모음](designer-basics-images/vs/04-toolbar-sml.png)](designer-basics-images/vs/04-toolbar.png#lightbox)

합니다 **디자이너 도구 모음** 다음 기능에 대 한 액세스를 제공 합니다.

-   **대체 레이아웃 선택기** &ndash; 레이아웃이 버전에서 선택할 수 있습니다.

-   **장치 선택기** &ndash; 특정 장치와 연결 된 한정자 (예: 화면 크기, 해상도 및 키보드 가용성)의 집합을 정의 합니다. 또한 추가 하 고 새 장치를 삭제할 수 있습니다.

-   **Android 버전 선택기** &ndash; 레이아웃의 대상으로 하는 Android 버전입니다. 디자이너에는 선택한 Android 버전에 따라 레이아웃을 렌더링 됩니다.

-   **테마 선택기** &ndash; 레이아웃에 대 한 UI 테마를 선택 합니다.

-   **구성 선택기** &ndash; 와 같은 장치 구성 선택 *세로* 하거나 *가로*합니다.

-   **리소스 한정자 옵션** &ndash; 를 선택 하기 위한 드롭다운 메뉴를 표시 하는 대화 상자를 엽니다 *언어*를 *UI 모드*를 *야간 모드*, 및 *둥근 화면* 옵션입니다.

-   **작업 모음 설정** &ndash; 레이아웃의 작업 모음 설정을 구성 합니다.

-   **테마 편집기** &ndash; 열립니다는 *테마 편집기*, 선택한 테마 요소를 사용자 지정할 수 있게 합니다.

-   **재질 디자인 눈금** &ndash; 사용 하거나 사용 하지 않도록 설정 된 *재질 디자인 눈금*합니다. 재질 디자인 눈금으로 인접 한 드롭다운 메뉴 항목의 표를 사용자 지정할 수 있도록 하는 대화 상자가 열립니다.

이 항목에서 자세히 설명 되어 각각의이 기능:

-   [리소스 한정자 및 시각화 옵션](~/android/user-interface/android-designer/resource-qualifiers.md) 에 대 한 자세한 정보를 제공 합니다 **장치 선택기**합니다 **Android 버전 선택기**, **테마 선택기**하십시오 **구성 선택기**, **리소스 한정자 옵션**, 및 **작업 모음 설정**.

-   [대체 레이아웃 보기](~/android/user-interface/android-designer/alternative-layout-views.md) 를 사용 하는 방법에 설명 합니다 **대체 레이아웃 선택기**합니다.

-   [Xamarin.Android 디자이너 재질 디자인 기능](~/android/user-interface/android-designer/material-design-features.md) 의 포괄적인 개요를 제공 합니다 **테마 편집기** 하며 **재질 디자인 눈금**합니다.

### <a name="context-menu-commands"></a>상황에 맞는 메뉴 명령

상황에 맞는 메뉴를 사용할 수 모두를 **디자인 화면** 및 합니다 **문서 개요**합니다. 이 메뉴에 선택한 위젯 및 해당 컨테이너를 컨테이너에서 작업을 수행 하기 더 쉽게 사용할 수 있는 명령이 표시 됩니다 (항상 쉽게에서 선택 하지 않은 합니다 **디자인 화면**). 상황에 맞는 메뉴의 예는 다음과 같습니다.

[![디자인 화면을 마우스 오른쪽 단추로 클릭 하는 경우 예제 상황에 맞는 메뉴](designer-basics-images/vs/06-context-menu-sml.png)](designer-basics-images/vs/06-context-menu.png#lightbox)

이 예제에서는 마우스 오른쪽 단추로 클릭 한 `TextView` 몇 가지 옵션을 제공 하는 상황에 맞는 메뉴를 엽니다.

-   **LinearLayout** &ndash; 편집에 대 한 하위 메뉴를 엽니다 합니다 `LinearLayout` 의 부모는 `TextView`합니다.

-   **삭제할**, **복사**, 및 **잘라내기** &ndash; 를 마우스 오른쪽 단추로 클릭에 적용 되는 작업 `TextView`합니다.


### <a name="zoom-controls"></a>확대/축소 컨트롤

합니다 **디자인 화면** 표시 된 것 처럼 여러 컨트롤을 통해 확대/축소를 지원 합니다.

[![디자인 화면 확대/축소 컨트롤의 다이어그램](designer-basics-images/vs/07-zoom-controls-sml.png)](designer-basics-images/vs/07-zoom-controls.png#lightbox)

이러한 컨트롤을 사용 하면 디자이너에서 사용자 인터페이스의 특정 영역을 보기 쉽게:

-   **컨테이너 강조 표시** &ndash; 컨테이너에서 강조 표시 된 **디자인 화면** 보다 쉽게 확대 및 축소 하는 동안 찾을 수 있도록 합니다.

-   **보통 크기** &ndash; 선택한 장치의 해상도 레이아웃 모양을 볼 수 있도록는 레이아웃에 대 한 픽셀을 렌더링 합니다.

-   **창에 맞춤** &ndash; 확대/축소 수준을 설정 하는 전체 레이아웃 디자인 화면에 표시 되도록 합니다.

-   **확대** &ndash; 클릭할 때마다 레이아웃 확대를 사용 하 여 증분 방식으로 확대 합니다.

-   **축소** &ndash; 클릭할 때마다 디자인 화면에서 더 작게 표시 레이아웃 만들기를 사용 하 여 증분 방식으로 축소 합니다.

참고 선택한 확대/축소 설정을 런타임 시 응용 프로그램의 사용자 인터페이스에 영향을 주지 않습니다.

## <a name="switching-between-design-and-source-panes"></a>디자인 창과 원본 창 간에 전환

간의 center 스트립에는 **디자인** 및 **원본** 창 단추가 수정 하는 데 사용 되는 여러 개 있습니다 하는 방법을 **디자인** 및 **원본**창이 표시 됩니다.

[![창 표시 단추 위치](designer-basics-images/vs/25-pane-buttons-sml.png)](designer-basics-images/vs/25-pane-buttons.png#lightbox)

이러한 단추는 다음을 수행합니다.

-   **디자인** &ndash; 이 맨 위에 있는 단추를 **디자인**를 선택 합니다 **디자인** 창입니다. 이 단추를 클릭할 때 합니다 **도구 상자** 하 고 **속성** 창이 활성화 됩니다 및 **텍스트 편집기 도구 모음** 표시 되지 않습니다. 경우는 **축소** 단추를 클릭 하면 (아래 참조)는 **디자인** 창이 없이 단독으로 표시 됩니다는 **원본** 창입니다.

-   **창 바꾸기** &ndash; 이 단추 (두 개의 화살표가 대립 되) 개의 교환이 지원 합니다 **디자인** 및 **원본** 창이 있도록를 **소스** 창에는 왼쪽 및 **디자인** 창 오른쪽에 표시 됩니다. 이러한 창을 원래 위치로 다시 다시 클릭 하 고 교환 합니다.

-   **소스** &ndash; (두 가지 대립 되 꺾쇠 괄호와 유사)는이 단추를 선택 합니다 **원본** 창입니다. 이 단추를 클릭할 때 합니다 **도구 상자** 및 **속성** 창이 비활성화 됩니다 및 **텍스트 편집기 도구 모음** Visual Studio의 위쪽에 표시 됩니다. 경우는 **축소** 단추를 클릭 (아래 참조)를 클릭 하는 **원본** 표시 단추를 **원본** 대신 창을 **디자인** 창입니다.

-   **수직 분할** &ndash; (세로 막대와 유사)는이 단추를 표시 합니다 **디자인** 하 고 **원본** 창-side-by-side. 이 기본 정렬입니다.

-   **수평 분할** &ndash; (가로 막대와 유사)는이 단추를 표시 합니다 **디자인** 창 위의 합니다 **원본** 창. **창 바꾸기** 배치를 클릭할 수는 **소스** 위에 **디자인** 창입니다.

-   **창 축소** &ndash; 이 단추 (두 개의 오른쪽 꺾쇠 괄호와 유사) "축소" 이중 창 표시 **디자인** 하 고 **원본** 중 하나의 단일 보기로 이러한 창이 있습니다.
    이 버튼을 **창 확장** 돌아가려면 뷰 이중 창 클릭할 수 있는 단추 (두 개의 왼쪽 꺾쇠 괄호와 비슷한), (**디자인** 및 **원본**) 모드를 표시 합니다.

때 **창 축소** 를 클릭 하면만 합니다 **디자인** 창이 표시 됩니다. 클릭 수 있습니다 합니다 **소스** 대신 표시 하려면 단추를 **원본** 창입니다. 클릭 합니다 **디자인** 단추를 다시 돌아가면 합니다 **디자인** 창입니다.

## <a name="source-pane"></a>소스 창

합니다 **원본** 창에 표시 된 디자인을 내부 XML 원본을 표시 합니다 **디자인 화면**합니다. 있기 때문에 두 보기를 사용할 수 있는 동시에 시각적으로 디자인 및 설계에 대 한 기본 XML 소스 간에 앞뒤로 이동 하 여 UI 디자인을 만들려면:

[![소스 창에서 예제 XML 원본](designer-basics-images/vs/22-source-pane-w158-sml.png)](designer-basics-images/vs/22-source-pane-w158.png#lightbox)

XML 원본에 대 한 변경 내용에 렌더링 하는 즉시를 **디자인 화면**;에서 변경 합니다 **디자인 화면** 에 표시 된 XML 원본 발생할를 **원본** 창 적절 하 게 업데이트할 수 있습니다. 경우 변경 하면 XML를 **원본** 창, 자동 완성 및 IntelliSense 기능은 다음에 설명 된 대로 XML 기반 UI 개발 속도를 사용할 수 있습니다.

탐색 간편한 긴 XML 파일을 사용 하 여 작업 하는 경우에 **원본** 창 (이전 스크린샷의 오른쪽에 표시) 된 대로 Visual Studio 스크롤 막대를 지원 합니다. 스크롤 막대에 대 한 자세한 내용은 참조 하세요. [스크롤 막대를 사용자 지정 하 여 코드 추적 하는 방법을](https://msdn.microsoft.com/library/dn237345.aspx)합니다.


### <a name="autocompletion"></a>자동 완성

눌러도 위젯 특성의 이름을 입력을 시작 하면 <kbd>CTRL + SPACE</kbd> 가능한 완성 목록을 확인 합니다. 예를 들어, 입력 한 후 `android:lay` 다음 예제에서 (뒤에 입력 하 여 <kbd>CTRL + SPACE</kbd>), 다음 목록에 표시 됩니다.

[![레이아웃 특성의 자동 완성](designer-basics-images/vs/23-autocompletion-w158-sml.png)](designer-basics-images/vs/23-autocompletion-w158.png#lightbox)

키를 눌러 <kbd>ENTER</kbd> 나열 된 첫 번째 완성을 수락 하거나 화살표 키를 사용 하 여 키를 눌러 원하는 완성을 스크롤합니다 <kbd>ENTER</kbd>합니다. 또는를 스크롤하여 원하는 완성 클릭 마우스를 사용할 수 있습니다.

### <a name="intellisense"></a>IntelliSense

위젯에 대 한 새 특성을 입력 하 고 값을 할당 하기 시작 후 IntelliSense 팝업 후 트리거 문자를 입력 하 고 해당 특성에 사용할 유효한 값 목록을 제공 합니다. 예를 들어에 대 한 첫 번째 큰따옴표를 입력 한 후 `android:layout_width` 다음 예제에서는 자동 완성 선택기 표시 되므로이 너비에 대 한 유효한 선택 항목의 목록을 제공 합니다.

[![레이아웃 너비에 대 한 IntelliSense 예제](designer-basics-images/vs/24-intellisense-w158-sml.png)](designer-basics-images/vs/24-intellisense-w158.png#lightbox)

이 팝업의 맨 아래에 (처럼 위의 스크린샷에서 빨간색 윤곽선이 있는)는 두 개의 단추가 있습니다. 클릭 하는 **프로젝트 리소스** 왼쪽의 단추를 클릭 하는 동안 앱 프로젝트의 일부인 리소스 목록이 제한 합니다 **프레임 워크 리소스** 단추 오른쪽의 목록 제한 프레임 워크에서 사용할 수 있는 리소스를 표시 합니다.
이러한 단추 켜기 또는 끄기로 전환: 필터링 작업을 사용 하지 않으려면 다시 클릭 수 있습니다 각 제공 합니다.



## <a name="properties-pane"></a>속성 창

디자이너를 통해 위젯 속성 편집을 지원 합니다 **속성** 창: 

![속성 창 스크린샷](designer-basics-images/vs/08-property-pad.png)

에 나열 된 속성을 **속성** 창 변경에 따라 위젯에서 선택한 합니다 **디자인 화면**.

### <a name="default-values"></a>기본값

대부분의 위젯의 속성에 비어 있게 됩니다는 **속성** 창 선택한 Android 테마에서 해당 값이 상속 때문입니다.
합니다 **속성** 창 선택한 위젯에 대 한 명시적으로 설정 된 값을 표시만 됩니다; 테마에서 상속 된 값을 표시 되지 것입니다.

### <a name="referencing-resources"></a>참조 리소스

일부 속성이 레이아웃 이외의 파일에 정의 된 리소스를 참조할 수 있습니다 **.axml** 파일입니다. 이 형식의 가장 일반적인 경우가 `string` 고 `drawable` 리소스입니다. 그러나 참조 데도 사용할 수 있습니다 다른 리소스에 대 한 같은 `Boolean` 값 및 차원입니다. 속성 리소스 참조에서는 찾아보기 아이콘 (사각형) 속성에 대 한 텍스트 항목 옆에 표시 됩니다. 이 단추를 클릭할 때 리소스 선택기를 엽니다.

예를 들어 다음 스크린샷은 사용 가능한 옵션에 대 한 텍스트 필드의 오른쪽에 어두운된 사각형을 클릭 합니다.는 `Text` 위젯의 합니다 **속성** 창:

[![텍스트 옵션의 예제 목록](designer-basics-images/vs/09-text-options-sml.png)](designer-basics-images/vs/09-text-options.png#lightbox)

때 **리소스...**  를 클릭 합니다 **리소스 선택** 대화 상자가 표시 됩니다.

[![나열 된 몇 가지 리소스를 사용 하 여 리소스 스크린샷 예제](designer-basics-images/vs/09b-resources-w158-sml.png)](designer-basics-images/vs/09b-resources-w158.png#lightbox)

이 목록에서 해당 위젯의 텍스트를 하드 코딩 하는 대신에 사용할 텍스트 리소스를 선택할 수 있습니다 합니다 **속성** 창입니다. 다음 예제에 대 한 리소스 선택기에는 `Src` 의 속성을 `ImageView`:

[![ImageView에 대 한 아이콘 리소스를 나열 하는 리소스 선택기](designer-basics-images/vs/10-src-resource-sml.png)](designer-basics-images/vs/10-src-resource.png#lightbox)

오른쪽에 빈 사각형을 클릭 하는 `Src` 속성이 열립니다는 **리소스 선택** 에서 까지의 색 (위와 같이) 드로어 블 리소스 목록 사용 하 여 대화 상자.


### <a name="boolean-property-references"></a>부울 속성 참조

*부울* 속성 창에서 속성 옆에 있는 확인 표시로 일반적으로 속성을 선택 합니다. 지정할 수 있습니다는 `true` 또는 `false` 하거나이 확인란을 선택 취소 하거나 취소 하 여 값 속성의 오른쪽에 있는 진한 채워진 사각형을 클릭 하 여 속성 참조를 선택할 수 있습니다. 다음 예에서 텍스트는 모두 대문자로 변경을 클릭 하 여 합니다 **텍스트 모두 대문자로** 선택한 내용과 연관 하는 부울 속성 참조 `TextView`:

![부울 속성을 설정 하는 예](designer-basics-images/vs/11-boolean.png)

## <a name="editing-properties-inline"></a>속성 인라인 편집

Android Designer에서 특정 속성을 직접 편집을 지원 합니다 **디자인 화면** (따라서 검색 속성 목록에서 이러한 속성에 대 한 필요가 없습니다). 직접 편집할 수 있는 속성에는 텍스트, 여백, 크기가 포함 됩니다.

### <a name="text"></a>텍스트

일부 위젯의 텍스트 속성 (같은 `Button` 하 고 `TextView`)에서 직접 편집할 수 있습니다 합니다 **디자인 화면**. 위젯을 두 번 클릭은 모드로 설정 편집, 아래와 같이:

![Hello 문자열에 대 한 텍스트 리소스가](designer-basics-images/vs/12-text-resource.png "텍스트 리소스")

새 텍스트 값을 입력 하거나 새 리소스 문자열을 입력할 수 있습니다. 다음 예제에서는 `@string/hello` 텍스트를 사용 하 여 리소스는 바꾸려는 `CLICK THIS BUTTON`:

![Shift + Enter를 자동으로 텍스트를 새 리소스 연결](designer-basics-images/vs/13-shift-enter-resource.png)

이 변경 위젯의에 저장 됩니다 `text` 속성에 할당 된 값을 수정 하지 않습니다는 `@string/hello` 리소스입니다.
경우 새 텍스트 문자열에서 키를 누르면 <kbd>Shift</kbd> +
<kbd>Enter</kbd> 를 자동으로 입력 한 텍스트를 새 리소스에 연결 합니다.

### <a name="margin"></a>여백

위젯을 선택 하면 디자이너 크기나 여백 위젯의 대화형으로 변경할 수 있는 핸들을 표시 합니다. 선택 상태 위젯을 클릭 하면 여백 편집 모드와 크기 편집 모드 간에 전환 합니다.

처음에 대 한 위젯을 클릭할 때 여백 핸들이 표시 됩니다. 디자이너 핸들 변경 되는 속성을 표시 하는 핸들 중 하나에 마우스를 이동 하는 경우 (아래에 나와 있는 것 처럼는 `layout_marginLeft` 속성):

![디자이너에서 처리 하는 여백을 보여 주는 스크린샷](designer-basics-images/vs/15-margin-handles.png)

여백을 이미 설정한 경우 여백 차지 하는 간격을 나타내는 점선이 표시 됩니다.

![단추 주위 공간을 표시 하는 점선의 예](designer-basics-images/vs/16-margins-set.png)

### <a name="size"></a>크기

앞에서 설명한 대로 이미 선택 되어 위젯을 클릭 하 여 크기 편집 모드로 전환할 수 있습니다. 지정 된 차원에 대해 크기를 설정 하려면 삼각형 모양의 핸들을 클릭 `wrap_content`:

![줄 바꿈 콘텐츠 및 크기 조정 핸들](designer-basics-images/vs/17-wrap-content.png)

클릭 하 여 **콘텐츠 줄 바꿈** 핸들 축소 합니다. 해당 차원에 위젯 포함된 콘텐츠를 래핑하는 데 필요한 보다 더 큽니다. 이 예제에서는 단추 텍스트는 다음 스크린샷에 표시 된 것 처럼 가로 방향으로 축소 합니다.

크기 값을로 설정 되 면 **콘텐츠 줄 바꿈**, 크기를 변경 하는 것에 대 한 반대 방향을 가리키는 삼각형 모양의 핸들을 표시 하는 디자이너 `match_parent`:

![일치 항목이 부모 핸들](designer-basics-images/vs/18-match-parent.png)

클릭 하 여 **일치 부모** 핸들 부모 위젯 동일 되도록 해당 차원의 크기를 복원 합니다.

또한 끌면 순환 크기 조정 핸들 (처럼 위의 스크린샷에서)에 있는 임의의 위젯 크기를 조정 하려면 `dp` 값입니다. 이렇게 하면 둘 다 **콘텐츠 줄 바꿈** 하 고 **일치 부모** 해당 차원에 대 한 핸들이 표시 됩니다.

![순환 크기 조정 핸들](designer-basics-images/vs/19-resize-dp.png)

모든 컨테이너는 편집할 수는 `Size` 위젯입니다. 예를 들어 아래 스크린샷에서 있음을 `LinearLayout` 선택 하면 크기 조정 핸들 표시 되지 않습니다.

![크기 조정 핸들이 없는](designer-basics-images/vs/20-no-resize-handles.png)


## <a name="document-outline"></a>문서 개요

합니다 **문서 개요** 레이아웃 위젯 계층을 보여 줍니다.
다음 예제를 포함 하는 `LinearLayout` 위젯을 선택 합니다.

![문서 개요 예제](designer-basics-images/vs/21-document-outline.png)

선택한 위젯에 개요 (이 경우는 `LinearLayout`)에서 강조 표시 되는 **디자인 화면**. 문서 개요의 선택한 위젯에에 상응 하는 동기화 상태를 유지 합니다 **디자인 화면**합니다. 쉽게에서 선택할 수 없는 보기 그룹을 선택 하면 유용 합니다 **디자인 화면**합니다.

합니다 **문서 개요** 지원 복사 및 붙여넣기, 끌어서를 사용 하 고 삭제할 수 있습니다. 끌어서 놓기는 지원 합니다 **문서 개요** 에 **디자인 화면** 뿐만 아니라를 **디자인 화면** 를 **문서 개요**. 또한에 있는 항목을 마우스 오른쪽 단추로 클릭 합니다 **문서 개요** 해당 항목에 대 한 상황에 맞는 메뉴를 표시 합니다. (에 동일한 위젯을 마우스 오른쪽 단추로 클릭할 때 표시 되는 동일한 상황에 맞는 메뉴를 **디자인 화면**).




# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="launching-the-designer"></a>디자이너를 시작합니다.

디자이너는 레이아웃을 만들어지거나 기존.axml 파일을 두 번 클릭 하 여 시작할 수 있습니다 하는 경우 자동으로 시작 됩니다. 예를 들어, 두 번 클릭 **Main.axml** 에 **리소스 > 레이아웃** 폴더 아래와 같이 디자이너를 로드 합니다.

[![Mac 용 Visual Studio에서 디자이너 화면](designer-basics-images/xs/01-open-designer-sml.png)](designer-basics-images/xs/01-open-designer.png#lightbox)

마찬가지로, 마우스 오른쪽 단추로 클릭 하 여 새 레이아웃을 추가할 수 있습니다 합니다 **레이아웃** 폴더에는 **Solution Pad** 를 선택 하 고 **추가 > 새 파일 > Android > 레이아웃**:

[![새 파일 대화 상자를 추가 합니다.](designer-basics-images/xs/02-add-new-layout-sml.png)](designer-basics-images/xs/02-add-new-layout.png#lightbox)

새.axml 파일을 만들고 디자인 화면으로 로드 합니다.

## <a name="designer-features"></a>디자이너 기능

다음 스크린샷에 표시 된 대로 디자이너는 다양 한 기능을 지 원하는 몇 가지 섹션으로 구성 된:

[![디자이너 창 다이어그램](designer-basics-images/xs/03-designer-features-sml.png)](designer-basics-images/xs/03-designer-features.png#lightbox)

디자이너에서 레이아웃을 편집할 때를 만들고 디자인 셰이프 다음 기능을 사용 합니다.

-   **디자인 화면** &ndash; 레이아웃 장치에 표시 되는 방식에 대 한 편집 가능한 표현이 제공 하 여 사용자 인터페이스의 시각적 생성을 용이 하 게 합니다.

-   **도구 모음** &ndash; 선택기의 목록을 표시 합니다. **장치**, **버전**합니다 **테마**, 레이아웃 구성과 작업 모음 설정 합니다. 도구 모음에는 재질 디자인 눈금을 사용 하도록 설정 하는 것에 대 한 테마 편집기를 시작 하는 것에 대 한 아이콘이 포함 됩니다.

-   **도구 상자** &ndash; 위젯 및 끌어서 디자인 화면에 놓을 수 있는 레이아웃의 목록을 제공 합니다.

-   **속성 패드** &ndash; 보기 및 편집에 대 한 선택한 위젯에의 속성을 나열 합니다.

-   **문서 개요** &ndash; 트리 레이아웃을 구성 하는 위젯 수를 표시 합니다. 디자이너에서 선택 하면 트리에서 항목을 클릭할 수 있습니다. 또한 트리에서 항목을 클릭 하면 속성 패드를 항목의 속성을 로드 합니다.

## <a name="toolbar"></a>ToolBar

구성 선택기 및 도구 메뉴 도구 모음 (디자인 화면 위의 배치)를 제공 합니다.

[![다이어그램 디자이너 도구 모음](designer-basics-images/xs/04-toolbar-sml.png)](designer-basics-images/xs/04-toolbar.png#lightbox)

도구 모음은 다음 기능에 대 한 액세스를 제공합니다.

-   **대체 레이아웃 선택기** &ndash; 레이아웃이 버전에서 선택할 수 있습니다.

-   **장치 선택기** &ndash; 화면 크기, 해상도 및 키보드 가용성 같은 특정 장치에 연결 된 한정자 집합을 정의 합니다. 또한 추가 하 고 새 장치를 삭제할 수 있습니다.

-   **Android 버전 선택기** &ndash; 레이아웃의 대상으로 하는 Android 버전입니다. 디자이너에는 선택한 Android 버전에 따라 레이아웃을 렌더링 됩니다.

-   **테마 선택기** &ndash; 레이아웃에 대 한 UI 테마를 선택 합니다.

-   **구성 선택기** &ndash; 와 같은 장치 구성 선택 *세로* 하거나 *가로*합니다.

-   **리소스 한정자 옵션** &ndash; 를 선택 하기 위한 드롭다운 메뉴를 표시 하는 대화 상자를 엽니다 *언어*를 *UI 모드*를 *야간 모드*, 및 *둥근 화면* 옵션입니다.

-   **작업 모음 설정** &ndash; 레이아웃의 작업 모음 설정을 구성 합니다.

-   **테마 편집기** &ndash; 열립니다는 *테마 편집기*, 선택한 테마 요소를 사용자 지정할 수 있게 합니다.

-   **재질 디자인 눈금** &ndash; 사용 하거나 사용 하지 않도록 설정 된 *재질 디자인 눈금*합니다. 재질 디자인 눈금으로 인접 한 드롭다운 메뉴 항목의 표를 사용자 지정할 수 있도록 하는 대화 상자가 열립니다.

이 항목에서 자세히 설명 되어 각각의이 기능:

[리소스 한정자 및 시각화 옵션](~/android/user-interface/android-designer/resource-qualifiers.md) 에 대 한 자세한 정보를 제공 합니다 **장치 선택기**합니다 **Android 버전 선택기**, **테마 선택기**하십시오 **구성 선택기**, **리소스 한정자 옵션**, 및 **작업 모음 설정**.

[대체 레이아웃 보기](~/android/user-interface/android-designer/alternative-layout-views.md) 를 사용 하는 방법에 설명 합니다 **대체 레이아웃 선택기**합니다.

[재질 디자인 기능](~/android/user-interface/android-designer/material-design-features.md) 의 포괄적인 개요를 제공 합니다 **테마 편집기** 하며 **재질 디자인 눈금**합니다.

## <a name="design-surface"></a>디자인 화면

디자이너를 도구 상자에서 디자인 화면에서 끌어서 위젯 할 수 있습니다. (새 위젯 추가 하거나 기존 위치)에서 위젯 디자이너에서 상호 작용을 사용할 수 있는 삽입 지점을 표시 하려면 세로 선과 가로 선 표시 됩니다. 다음 예에서 새 `Button` 위젯을 디자인 화면으로 끄는 동안:

[![디자인 화면에서 예제 삽입 선](designer-basics-images/xs/05-insertion-points-sml.png)](designer-basics-images/xs/05-insertion-points.png#lightbox)

또한 위젯을 복사할 수 있습니다: 복사를 사용할 수 있으며 위젯을 복사 하 여 붙여넣기 끌어서 누른 채는 기존 위젯 합니다 <kbd>Ctrl</kbd> 키입니다.

### <a name="context-menu-commands"></a>상황에 맞는 메뉴 명령

상황에 맞는 메뉴는 문서 개요 및 디자인 화면에서 모두 사용할 수 있습니다. 이 메뉴에는 선택한 위젯 및 컨테이너 (항상 이해 하기가 쉽지 않습니다 디자인 화면에서 선택)에서 작업을 수행 하기 쉽게 해당 컨테이너에 사용할 수 있는 명령이 표시 됩니다. 상황에 맞는 메뉴의 예는 다음과 같습니다.

[![디자인 화면을 마우스 오른쪽 단추로 클릭 하는 경우 예제 상황에 맞는 메뉴](designer-basics-images/xs/06-context-menu-sml.png)](designer-basics-images/xs/06-context-menu.png#lightbox)

이 예제에서는 마우스 오른쪽 단추로 클릭 한 `Button` 몇 가지 옵션을 제공 하는 상황에 맞는 메뉴를 엽니다.

-   **LinearLayout** &ndash; 편집에 대 한 하위 메뉴를 엽니다 합니다 `LinearLayout` 의 부모는 `Button`합니다.

-   **잘라내기**, **복사**, 및 **삭제** &ndash; 를 마우스 오른쪽 단추로 클릭에 적용 되는 작업 `Button`합니다.

### <a name="zoom-controls"></a>확대/축소 컨트롤

디자인 화면 표시 된 것 처럼 여러 컨트롤을 통해 확대/축소를 지원 합니다.

[![디자인 화면 확대/축소 컨트롤의 다이어그램](designer-basics-images/xs/07-zoom-controls-sml.png)](designer-basics-images/xs/07-zoom-controls.png#lightbox)

이러한 컨트롤을 사용 하면 디자이너에서 사용자 인터페이스의 특정 영역을 보기 쉽게:

-   **컨테이너 강조 표시** &ndash; 보다 쉽게 확대 및 축소 하는 동안 찾을 수 있도록 디자인 화면에서 컨테이너를 강조 표시 합니다.

-   **보통 크기** &ndash; 선택한 장치의 해상도 레이아웃 모양을 볼 수 있도록는 레이아웃에 대 한 픽셀을 렌더링 합니다.

-   **창에 맞춤** &ndash; 확대/축소 수준을 설정 하는 전체 레이아웃 디자인 화면에 표시 되도록 합니다.

-   **확대** &ndash; 클릭할 때마다 레이아웃 확대를 사용 하 여 증분 방식으로 확대 합니다.

-   **축소** &ndash; 클릭할 때마다 디자인 화면에서 더 작게 표시 레이아웃 만들기를 사용 하 여 증분 방식으로 축소 합니다.

참고 선택한 확대/축소 설정을 런타임 시 응용 프로그램의 사용자 인터페이스에 영향을 주지 않습니다.

## <a name="property-pad"></a>속성 패드

디자이너를 통해 위젯 속성 편집을 지원 합니다 **속성 패드**합니다. 위젯 디자이너 화면에서 선택한에 따라 속성 패드 변경에 나열 된 속성입니다. 경우는 `Button` 앞의 예제에서를 선택에 대 한 속성 `Button` 위젯에 표시 됩니다:

[![속성 패드의 스크린샷](designer-basics-images/xs/08-property-pad-sml.png)](designer-basics-images/xs/08-property-pad.png#lightbox)

## <a name="property-pad-sections"></a>속성 패드 섹션

속성 패드 비슷한 속성을 함께 그룹화 하는 몇 개의 섹션으로 나뉩니다 &ndash; 이 쉽게 관심 속성을 찾습니다.

-   **위젯** &ndash; 위젯의 주 속성 같은 `id`를 `visibility`, `text`등입니다. 위젯의 콘텐츠를 관리 하는 것에 대 한 속성 일반적으로 여기에 배치 됩니다.

-   **스타일** &ndash; 와 같은 위젯의 시각적 모양을 변경 하는 속성 `font`합니다 `text color`, `background`등입니다.

-   **레이아웃** &ndash; 위젯의 크기와 위치를 설정 하는 속성입니다.

-   **스크롤** &ndash; 스크롤 속성입니다.

-   **동작** &ndash; 위젯을 동작 하는 방식을 설정 하는 플래그입니다.

### <a name="default-values"></a>기본값

대부분의 위젯의 속성에 비어 있게 됩니다는 **속성 패드** 선택한 Android 테마에서 해당 값이 상속 때문입니다. 합니다 **속성 패드** 는 선택한 위젯에 대 한 명시적으로 설정 된 값에만 표시; 테마에서 상속 된 값을 표시 되지 것입니다.

### <a name="referencing-resources"></a>참조 리소스

일부 속성이 레이아웃 이외의 파일에 정의 된 리소스를 참조할 수 있습니다 **.axml** 파일입니다. 이 형식의 가장 일반적인 경우가 `string` 고 `drawable` 리소스입니다. 그러나 참조 데도 사용할 수 있습니다 다른 리소스에 대 한 같은 `Boolean` 값 및 차원입니다.
속성 참조 리소스 찾아보기 아이콘을 지 원하는 경우 (줄임표를는 &hellip;) 속성에 대 한 텍스트 항목 옆에 표시 됩니다.
클릭 하면이 단추는 리소스 선택기를 엽니다.

예를 들어 다음 스크린샷에서 사용할 수 있는 리소스에 대 한 텍스트 필드의 오른쪽에 있는 줄임표를 클릭 하는 경우는 `Button` 위젯의 합니다 **속성 패드**:

[![두 개의 리소스가 나열 된 예제 리소스 스크린샷](designer-basics-images/xs/09-resources-sml.png)](designer-basics-images/xs/09-resources.png#lightbox)

다음 예제에 대 한 리소스 선택기에는 `Src` 의 속성을 `ImageView`:

[![ImageView에 대 한 아이콘 리소스를 나열 하는 리소스 선택기](designer-basics-images/xs/10-src-resource-sml.png)](designer-basics-images/xs/10-src-resource.png#lightbox)

### <a name="boolean-property-references"></a>부울 속성 참조

*부울* 속성은 일반적으로 속성 패드에서 확인란으로 표시 됩니다. 경우는 `Boolean` 속성이 지원 리소스 참조, 속성 옆에 작은 확인란이 나타납니다. 선택 된 확인란 의미 `true` 빈 상자 의미 `false`합니다. 하면 직접 값을 입력할 수와 같은 `true` 또는 `false`합니다. 입력을 마우스로 가리키면 작은 텍스트 필드 아이콘을 표시 합니다. 값을 수동으로 입력 하려는 경우에 클릭 수 있습니다.

[![부울 속성을 설정 하는 예](designer-basics-images/xs/12-boolean-sml.png)](designer-basics-images/xs/12-boolean.png#lightbox)


## <a name="grouped-properties"></a>그룹화 된 속성

일부 위젯이 그룹화 되는 다중 값 속성 (같은 `Padding`예를 들어). 이러한 속성 값에 나열 됩니다는 **속성 패드** 단일, 확장 가능한 행에서입니다. 이러한 속성 중 일부를 편집할 수 있습니다 그룹화 된 행에 직접 같은 `Padding` 아래에 표시 된 속성:

[![Padding 속성에 대 한 예제 설정](designer-basics-images/xs/13-padding-property-sml.png)](designer-basics-images/xs/13-padding-property.png#lightbox)

## <a name="editing-properties-inline"></a>속성 인라인 편집

Android Designer (따라서 검색 속성 목록에서 이러한 속성에 대 한 필요가) 디자인 화면에서 특정 속성의 직접 편집을 지원 합니다. 직접 편집할 수 있는 속성에는 텍스트, 여백, 크기가 포함 됩니다.

### <a name="text"></a>텍스트

일부 위젯의 텍스트 속성 (같은 `Button` 고 `TextView`), 디자인 화면에서 직접 편집할 수 있습니다. 위젯을 두 번 클릭은 모드로 설정 편집, 아래와 같이:

[![Hello 문자열에 대 한 텍스트 리소스](designer-basics-images/xs/14-text-resource-sml.png)](designer-basics-images/xs/14-text-resource.png#lightbox)

새 텍스트 값을 입력 하거나 새 리소스 문자열을 입력할 수 있습니다. 다음 예제에서는 `@string/hello` 텍스트를 사용 하 여 리소스는 바꾸려는 `CLICK THIS BUTTON`:

[![Shift + Enter를 자동으로 텍스트를 새 리소스 연결](designer-basics-images/xs/15-shift-enter-resource-sml.png)](designer-basics-images/xs/15-shift-enter-resource.png#lightbox)

이 변경 위젯의에 저장 됩니다 `text` 속성에 할당 된 값을 수정 하지 않습니다는 `@string/hello` 리소스입니다.
경우 새 텍스트 문자열에서 키를 누르면 <kbd>Shift</kbd> +
<kbd>Enter</kbd> 를 자동으로 입력 한 텍스트를 새 리소스에 연결 합니다.

### <a name="margin"></a>여백

위젯을 선택 하면 디자이너 크기나 여백 위젯의 대화형으로 변경할 수 있는 핸들을 표시 합니다. 선택 상태 위젯을 클릭 하면 여백 편집 모드와 크기 편집 모드 간에 전환 합니다.

처음에 대 한 위젯을 클릭할 때 여백 핸들이 표시 됩니다. 디자이너 핸들 변경 되는 속성을 표시 하는 핸들 중 하나에 마우스를 이동 하는 경우 (아래에 나와 있는 것 처럼는 `layout_marginLeft` 속성):

[![디자이너에서 처리 하는 여백을 보여 주는 스크린샷](designer-basics-images/xs/16-margin-handles-sml.png)](designer-basics-images/xs/16-margin-handles.png#lightbox)

여백을 이미 설정한 경우 여백 차지 하는 간격을 나타내는 점선이 표시 됩니다.

[![단추 주위 공간을 표시 하는 점선의 예](designer-basics-images/xs/17-margins-set-sml.png)](designer-basics-images/xs/17-margins-set.png#lightbox)

### <a name="size"></a>크기

앞에서 설명한 대로 이미 선택 되어 위젯을 클릭 하 여 크기 편집 모드로 전환할 수 있습니다. 지정 된 차원에 대해 크기를 설정 하려면 삼각형 모양의 핸들을 클릭 `wrap_content`:

[![줄 바꿈 콘텐츠 및 크기 조정 핸들](designer-basics-images/xs/18-wrap-content-sml.png)](designer-basics-images/xs/18-wrap-content.png#lightbox)

클릭 하는 **콘텐츠 줄 바꿈** 핸들 정말 이하의 포함된 콘텐츠를 래핑하는 데 필요한 해당 차원에 대 한 위젯을 축소 합니다. 이 예제에서는 단추 텍스트는 다음 스크린샷에 표시 된 것 처럼 가로 방향으로 축소 합니다.

크기 값을로 설정 되 면 **콘텐츠 줄 바꿈**, 크기를 변경 하는 것에 대 한 반대 방향을 가리키는 삼각형 모양의 핸들을 표시 하는 디자이너 `match_parent`:

[![일치 항목이 부모 핸들](designer-basics-images/xs/19-match-parent-sml.png)](designer-basics-images/xs/19-match-parent.png#lightbox)

클릭 하 여 **일치 부모** 핸들 부모 위젯 동일 되도록 해당 차원의 크기를 복원 합니다.

또한 끌면 순환 크기 조정 핸들 (처럼 위의 스크린샷에서)에 있는 임의의 위젯 크기를 조정 하려면 `dp` 값입니다. 이렇게 하면 둘 다 **콘텐츠 줄 바꿈** 하 고 **일치 부모** 해당 차원에 대 한 핸들이 표시 됩니다.

[![순환 크기 조정 핸들](designer-basics-images/xs/20-resize-dp-sml.png)](designer-basics-images/xs/20-resize-dp.png#lightbox)

모든 컨테이너는 편집할 수는 `Size` 위젯입니다. 예를 들어 아래 스크린샷에서 있음을 `LinearLayout` 선택 하면 크기 조정 핸들 표시 되지 않습니다.

[![크기 조정 핸들이 없는](designer-basics-images/xs/21-no-resize-handles-sml.png)](designer-basics-images/xs/20-no-resize-handles.png#lightbox)

## <a name="document-outline"></a>문서 개요

합니다 **문서 개요** 레이아웃 위젯 계층을 보여 줍니다.
다음 예제를 포함 하는 `LinearLayout` 위젯을 선택 합니다.

[![문서 개요](designer-basics-images/xs/22-outline-view-sml.png)](designer-basics-images/xs/22-outline-view.png#lightbox)

선택한 위젯에 개요 (이 경우에 `LinearLayout`) 디자인 화면에서 강조 표시 됩니다. 문서 개요에서 선택한 위젯을 디자인 화면에 상응 하는 동기화 상태로 유지 됩니다. 이 항상 쉽게 디자인 화면에서 선택 하지 않은 보기 그룹을 선택 하면 유용 합니다.

문서 개요 복사 및 붙여넣기를 지원 하거나 끌어서를 사용 하 고 삭제할 수 있습니다. 문서 개요를 디자인 화면에서 뿐만 아니라 문서 개요를 디자인 화면에서 끌어서 놓기 지원 됩니다. 또한 문서 개요의 항목을 마우스 오른쪽 단추로 클릭 (동일한 상황에 맞는 메뉴는 동일한 위젯을 디자인 화면에서 마우스 오른쪽 단추로 클릭할 때 표시 되는) 해당 항목에 대 한 상황에 맞는 메뉴를 표시 합니다.

-----
