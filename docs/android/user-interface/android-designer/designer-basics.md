---
title: 디자이너의 기본 사항
description: 이 항목 디자이너 기능을 소개, 디자이너를 시작 하는 방법을 설명, 디자인 화면을 설명 및 위젯 속성을 편집 하려면 속성 창을 사용 하는 방법에 자세히 설명 합니다.
ms.prod: xamarin
ms.assetid: 48B20C9A-B2A2-AE82-76B2-A3C1E5A4050D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 6bac16a8ce9859e819299689489d9aad982c1f7f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="designer-basics"></a>디자이너의 기본 사항

_이 항목 디자이너 기능을 소개, 디자이너를 시작 하는 방법을 설명, 디자인 화면을 설명 및 위젯 속성을 편집 하려면 속성 창을 사용 하는 방법에 자세히 설명 합니다._


## <a name="launching-the-designer"></a>디자이너를 시작합니다.

디자이너는 레이아웃 생성 되거나, 기존.axml 파일을 두 번 클릭 하 여 실행할 수 있습니다 때 자동으로 시작 됩니다. 예를 들어, 두 번 클릭 하면 **Main.axml** 에 **리소스 > 레이아웃** 폴더 아래와 같이 디자이너를 로드 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Visual Studio에서 디자이너 화면](designer-basics-images/vs/01-open-designer-sml.png)](designer-basics-images/vs/01-open-designer.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Mac 용 Visual Studio에서 디자이너 화면](designer-basics-images/xs/01-open-designer-sml.png)](designer-basics-images/xs/01-open-designer.png#lightbox)

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

마찬가지로, 마우스 오른쪽 단추로 클릭 하 여 새 레이아웃을 추가할 수 있습니다는 **레이아웃** 폴더에는 **솔루션 탐색기** 선택 하 고 **추가 > 새 항목 … > Android 레이아웃**:

[![새 항목 추가 대화 상자](designer-basics-images/vs/02-add-new-layout-sml.png)](designer-basics-images/vs/02-add-new-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

마찬가지로, 마우스 오른쪽 단추로 클릭 하 여 새 레이아웃을 추가할 수 있습니다는 **레이아웃** 폴더에는 **솔루션 패드** 선택 하 고 **추가 > 새 파일 > Android > 레이아웃**:

[![새 파일 대화 상자를 추가 합니다.](designer-basics-images/xs/02-add-new-layout-sml.png)](designer-basics-images/xs/02-add-new-layout.png#lightbox)

-----

새.axml 파일을 디자인 화면으로 로드 됩니다.



## <a name="designer-features"></a>디자이너 기능

다음 스크린샷에 표시 된 것 처럼 디자이너의 다양 한 기능을 지 원하는 몇 가지 섹션으로 구성 된:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![디자이너 창 다이어그램](designer-basics-images/vs/03-designer-features-sml.png)](designer-basics-images/vs/03-designer-features.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![디자이너 창 다이어그램](designer-basics-images/xs/03-designer-features-sml.png)](designer-basics-images/xs/03-designer-features.png#lightbox)

-----

디자이너에서 레이아웃을 편집 하면를 만들고 디자인 셰이프 다음 기능을 사용 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   **디자인 화면** &ndash; 레이아웃 장치에서 어떻게 나타나는지는 편집 가능한 표현을 제공 하 여 사용자 인터페이스를 시각적으로 제작할을 용이 하 게 합니다.

-   **도구 모음** &ndash; 선택기의 목록이 표시 됩니다: **장치**, **버전**, **테마**, 레이아웃 구성 및 작업 모음 설정 합니다. 도구 모음에는 테마 편집기를 시작 하는 것에 대 한 자료 디자인 눈금을 사용 하도록 설정 하는 것에 대 한 아이콘이 포함 되어 있습니다.

-   **도구 상자** &ndash; 위젯 및 끌어서 디자인 화면에 놓을 수 있는 레이아웃의 목록을 제공 합니다.

-   **속성 창** &ndash; 선택한 위젯의 보기 및 편집에 대 한 속성을 보여 줍니다.

-   **문서 개요** &ndash; 트리 레이아웃을 구성 하는 위젯을 표시 합니다. 디자이너에서 선택 트리 항목 클릭 수 있습니다. 또한 트리 항목을 클릭 하 여 속성 창에 항목의 속성을 로드 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   **디자인 화면** &ndash; 레이아웃 장치에서 어떻게 나타나는지는 편집 가능한 표현을 제공 하 여 사용자 인터페이스를 시각적으로 제작할을 용이 하 게 합니다.

-   **도구 모음** &ndash; 선택기의 목록이 표시 됩니다: **장치**, **버전**, **테마**, 레이아웃 구성 및 작업 모음 설정 합니다. 도구 모음에는 테마 편집기를 시작 하는 것에 대 한 자료 디자인 눈금을 사용 하도록 설정 하는 것에 대 한 아이콘이 포함 되어 있습니다.

-   **도구 상자** &ndash; 위젯 및 끌어서 디자인 화면에 놓을 수 있는 레이아웃의 목록을 제공 합니다.

-   **속성 패드** &ndash; 선택한 위젯의 보기 및 편집에 대 한 속성을 보여 줍니다.

-   **문서 개요** &ndash; 트리 레이아웃을 구성 하는 위젯을 표시 합니다. 디자이너에서 선택 트리 항목 클릭 수 있습니다. 또한 트리에서 항목을 클릭 하면 속성 패드에 항목의 속성을 로드 합니다.

-----



## <a name="toolbar"></a>Toolbar

도구 모음 (디자인 화면 위의 배치 방식) 구성 선택기 및 도구 메뉴를 제공 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![다이어그램 디자이너 도구 모음](designer-basics-images/vs/04-toolbar-sml.png)](designer-basics-images/vs/04-toolbar.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![다이어그램 디자이너 도구 모음](designer-basics-images/xs/04-toolbar-sml.png)](designer-basics-images/xs/04-toolbar.png#lightbox)

-----


도구 모음에서는 다음과 같은 기능에 대 한 액세스를 제공합니다.

-   **대체 레이아웃 선택기** &ndash; 다른 레이아웃 버전에서 선택할 수 있습니다.

-   **장치 선택기** &ndash; 화면 크기, 해상도 및 키보드 가용성 같은 특정 장치에 연결 된 한정자 집합을 정의 합니다. 추가 하 고 새 장치를 삭제할 수도 있습니다.

-   **Android 버전 선택기** &ndash; 레이아웃을 대상으로 하는 Android 버전입니다. 디자이너 레이아웃 선택한 Android 버전에 따라 렌더링 됩니다.

-   **테마 선택기** &ndash; 레이아웃에 대 한 UI 테마를 선택 합니다.

-   **구성 선택기** &ndash; 와 같은 장치 구성을 선택 *세로* 또는 *가로*합니다.

-   **리소스 한정자 옵션** &ndash; 선택을 위한 드롭다운 메뉴를 표시 하는 대화 상자가 열립니다 *언어*, *UI 모드*, *밤 모드*, 및 *라운드 화면* 옵션입니다.

-   **동작 모음 설정** &ndash; 레이아웃에 대 한 작업 모음 설정을 구성 합니다.

-   **테마 편집기** &ndash; 열립니다는 *테마 편집기*, 선택한 테마의 요소를 사용자 지정할 수 있습니다.

-   **자재 디자인 눈금** &ndash; 사용 하거나 사용 하지 않도록 설정 된 *자료 디자인 눈금*합니다. 자료 디자인 눈금으로 인접 한 드롭다운 메뉴 항목에는 눈금을 사용자 지정할 수 있도록 하는 대화 상자가 열립니다.

이 항목에서 자세히 설명 되어 각각의이 기능:

[리소스 한정자 및 시각화 옵션](~/android/user-interface/android-designer/resource-qualifiers.md) 에 대 한 자세한 정보를 제공는 **장치 선택기**, **Android 버전 선택기**, **테마 선택기**, **구성 선택기**, **리소스 자격 옵션**, 및 **작업 모음 설정**합니다.

[대체 레이아웃 보기](~/android/user-interface/android-designer/alternative-layout-views.md) 사용 하는 방법에 설명 된 **대체 레이아웃 선택기**합니다.

[자재 디자인 기능](~/android/user-interface/android-designer/material-design-features.md) 의 포괄적인 개요를 제공 된 **테마 편집기** 및 **자료 디자인 눈금**합니다.



## <a name="design-surface"></a>디자인 화면

디자이너를 사용 하면 디자인 화면으로 도구 상자에서 끌어서 위젯 수 있습니다. (새로운 위젯을 추가 하거나 기존 위치)에서 위젯을 디자이너에서 상호 작용할 때 사용할 수 있는 삽입 지점을 표시 하려면 세로 선과 가로 선 표시 됩니다. 다음 예에서 새 `Button` 디자인 화면으로 끌고 위젯:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![디자인 화면에서 예제 삽입 선](designer-basics-images/vs/05-insertion-points-sml.png)](designer-basics-images/vs/05-insertion-points.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![디자인 화면에서 예제 삽입 선](designer-basics-images/xs/05-insertion-points-sml.png)](designer-basics-images/xs/05-insertion-points.png#lightbox)

-----

또한 위젯을 복사할 수 있습니다: 복사본을 사용할 수 있습니다 및 붙여넣기 위젯, 복사를 끌어 놓은 수는 기존 위젯 누른 채는 <kbd>Ctrl</kbd> 키입니다.


### <a name="context-menu-commands"></a>상황에 맞는 메뉴 명령

상황에 맞는 메뉴는 문서 개요 및 디자인 화면에서 모두 사용할 수 있습니다. 이 메뉴에는 선택한 위젯 및 컨테이너 (하지 않은 항상 쉽게 디자인 화면에서 선택할 수)에 대 한 작업을 수행할 수 있는 보다 쉽게 해당 컨테이너에 사용할 수 있는 명령이 표시 됩니다. 상황에 맞는 메뉴의 예는 다음과 같습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![디자인 화면을 마우스 오른쪽 단추로 클릭 하는 경우 예제 상황에 맞는 메뉴](designer-basics-images/vs/06-context-menu-sml.png)](designer-basics-images/vs/06-context-menu.png#lightbox)

이 예제에서는 마우스 오른쪽 단추로 클릭 한 `TextView` 몇 가지 옵션을 제공 하는 상황에 맞는 메뉴를 엽니다.

-   **LinearLayout** &ndash; 편집 하기 위한 하위 메뉴가 열립니다는 `LinearLayout` 의 부모는 `TextView`합니다.

-   **삭제**, **복사**, 및 **잘라내기** &ndash; 적용 하 고 마우스 오른쪽 단추로 클릭 하는 작업 `TextView`합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![디자인 화면을 마우스 오른쪽 단추로 클릭 하는 경우 예제 상황에 맞는 메뉴](designer-basics-images/xs/06-context-menu-sml.png)](designer-basics-images/xs/06-context-menu.png#lightbox)

이 예제에서는 마우스 오른쪽 단추로 클릭 한 `TextView` 몇 가지 옵션을 제공 하는 상황에 맞는 메뉴를 엽니다.

-   **RelativeLayout** &ndash; 편집 하기 위한 하위 메뉴가 열립니다는 `RelativeLayout` 의 부모는 `TextView`합니다.

-   **LinearLayout** &ndash; 편집 하기 위한 하위 메뉴가 열립니다는 `LinearLayout` 의 부모는 `TextView`합니다.

-   **삭제**, **복사**, 및 **잘라내기** &ndash; 적용 하 고 마우스 오른쪽 단추로 클릭 하는 작업 `TextView`합니다.

-----

이 예제에서는 마우스 오른쪽 단추로 클릭 한 `TextView` 몇 가지 옵션을 제공 하는 상황에 맞는 메뉴를 엽니다.

-   **LinearLayout** &ndash; 편집 하기 위한 하위 메뉴가 열립니다는 `LinearLayout` 의 부모는 `TextView`합니다.

-   **삭제**, **복사**, 및 **잘라내기** &ndash; 적용 하 고 마우스 오른쪽 단추로 클릭 하는 작업 `TextView`합니다.



### <a name="zoom-controls"></a>확대/축소 컨트롤

디자인 화면에 표시 된 것 처럼 여러 컨트롤을 통해 확대/축소 지원 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![디자인 화면 확대/축소 컨트롤의 다이어그램](designer-basics-images/vs/07-zoom-controls-sml.png)](designer-basics-images/vs/07-zoom-controls.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![디자인 화면 확대/축소 컨트롤의 다이어그램](designer-basics-images/xs/07-zoom-controls-sml.png)](designer-basics-images/xs/07-zoom-controls.png#lightbox)

-----

이러한 컨트롤을 사용 하면 쉽게 디자이너에서 사용자 인터페이스의 특정 영역을 볼 수 있습니다.

-   **컨테이너의 강조 표시** &ndash; 보다 쉽게 확대 및 축소 하는 동안 찾을 수 있도록 디자인 화면에서 컨테이너를 강조 표시 합니다.

-   **보통 크기** &ndash; 레이아웃 모양을 선택한 장치의 해상도에서 볼 수 있도록의 레이아웃에 대 한 픽셀을 렌더링 합니다.

-   **창에 맞춤** &ndash; 확대/축소 수준을 설정 하는 전체 레이아웃을 디자인 화면에서 볼 수 있도록 합니다.

-   **확대/축소에** &ndash; 레이아웃 확대 누를 때마다 증분식으로 확대 합니다.

-   **축소** &ndash; 작게 디자인 화면에 표시 하는 레이아웃 만들기 누를 때마다 증분 방식으로 축소 합니다.

참고 선택한 확대/축소 설정을 런타임에 응용 프로그램의 사용자 인터페이스에 영향을 주지 않습니다.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="property-pad"></a>속성 채움

디자이너는 통해 위젯 속성을 편집 하는 지원의 **속성 패드**합니다. 디자이너 화면에서 선택한 위젯에 따라 속성 패드 변경에 나열 된 속성입니다. 때는 `Button` 앞의 예제에서를 선택 하에 대 한 속성 `Button` 위젯 표시 됩니다.

[![속성 패드의 스크린 샷](designer-basics-images/xs/08-property-pad-sml.png)](designer-basics-images/xs/08-property-pad.png#lightbox)

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="properties-window"></a>속성 창

디자이너는 통해 위젯 속성을 편집 하는 지원의 **속성 창**합니다. 디자인 화면에서 선택한 위젯에 따라 속성 창 변경에 나열 된 속성입니다.
때는 `Button` 앞의 예제에서를 선택 하에 대 한 속성 `Button` 위젯 표시 됩니다.

![속성 창의 스크린 샷](designer-basics-images/vs/08-property-pad.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="property-pad-sections"></a>속성 패드 섹션

속성 패드 유사한 속성을 그룹화 하는 여러 섹션으로 나누어져 &ndash; 이렇게 하면 보다 쉽게 원하는 속성을 찾을 수:

-   **위젯** &ndash; 위젯의 Main 속성이 같은 `id`, `visibility`, `text`등입니다. 위젯의 콘텐츠 관리에 대 한 속성 일반적으로 여기에 배치 됩니다.

-   **스타일** &ndash; 같이 위젯의 시각적 모양을 변경 하는 속성 `font`, `text color`, `background`등입니다.

-   **레이아웃** &ndash; 위젯의 크기와 위치를 설정 하는 속성입니다.

-   **스크롤** &ndash; 스크롤 속성입니다.

-   **동작** &ndash; 위젯의 동작 하는 방법을 설정 하는 플래그입니다.

-----



### <a name="default-values"></a>기본값

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

위젯 대부분의 속성에 비어 있게 됩니다는 **속성** 창 선택한 Android 테마에서 해당 값이 상속 때문에 있습니다.
**속성** 창에서 선택한 장치에 대 한 명시적으로 설정 된 값만 나타납니다; 테마에서 상속 되는 값을 표시 되지 것입니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

위젯 대부분의 속성에 비어 있게 됩니다는 **속성 패드** 선택한 Android 테마에서 해당 값이 상속 때문에 있습니다. **속성 패드** 선택한 장치에 대 한 명시적으로 설정 된 값만 표시 됩니다; 테마에서 상속 되는 값을 표시 되지 것입니다.

-----


### <a name="referencing-resources"></a>리소스 참조

일부 속성 레이아웃 이외의 파일에 정의 된 리소스를 참조할 수 **.axml** 파일입니다. 이 형식의 가장 일반적인 경우는 `string` 및 `drawable` 리소스입니다. 그러나 참조 데도 사용할 수 있습니다 다른 리소스와 같은 `Boolean` 값 및 차원입니다.
속성이 리소스 참조, 찾아보기 아이콘을 지원 하는 경우 (줄임표를 프로그램 &hellip;) 속성에 대 한 텍스트 항목 옆에 표시 됩니다.
이 단추 클릭 했을 때 리소스 선택기를 엽니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

예를 들어 다음 스크린샷은 사용할 수 있는 리소스에 대 한 텍스트 필드의 오른쪽에 있는 줄임표를 클릭 하면는 `Button` 에서 위젯의 **속성** 창:

[![나열 된 두 리소스가 포함 된 예제 리소스 스크린 샷](designer-basics-images/vs/09-resources-sml.png)](designer-basics-images/vs/09-resources.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

예를 들어 다음 스크린샷은 사용할 수 있는 리소스에 대 한 텍스트 필드의 오른쪽에 있는 줄임표를 클릭 하면는 `Button` 에서 위젯의 **속성 패드**:

[![나열 된 두 리소스가 포함 된 예제 리소스 스크린 샷](designer-basics-images/xs/09-resources-sml.png)](designer-basics-images/xs/09-resources.png#lightbox)

-----

다음 예제에 대 한 리소스 선택기는 `Src` 속성은 `ImageView`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ImageView에 대 한 아이콘 리소스를 나열 하는 리소스 선택기](designer-basics-images/vs/10-src-resource-sml.png)](designer-basics-images/vs/10-src-resource.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ImageView에 대 한 아이콘 리소스를 나열 하는 리소스 선택기](designer-basics-images/xs/10-src-resource-sml.png)](designer-basics-images/xs/10-src-resource.png#lightbox)

-----



### <a name="boolean-property-references"></a>부울 속성 참조

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

*부울* 속성이 속성 창에서 풀 다운 메뉴에서 일반적으로 선택 됩니다. 선택할 수는 `true` 또는 `false` 값 또는 선택할 수 있습니다 속성 참조를 클릭 하 여 **리소스 선택...** . 와 같은 값을 입력도 직접 수 `true` 또는 `false`합니다.

![부울 속성을 설정 하는 예](designer-basics-images/vs/11-boolean.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

*부울* 속성이 속성 패드에 확인란으로 정상적으로 표시 됩니다. 경우는 `Boolean` 속성 옆에 작은 확인란이 표시, 속성 리소스 참조를 지원 합니다. 선택 된 확인란 의미 `true` 하 고 빈 상자 의미 `false`합니다. 와 같은 값을 입력도 직접 수 `true` 또는 `false`합니다. 입력을 마우스로 가리키면 작은 텍스트 필드 아이콘은 표시 합니다. 값을 수동으로 입력 하려면에 클릭 수 있습니다.

[![부울 속성을 설정 하는 예](designer-basics-images/xs/12-boolean-sml.png)](designer-basics-images/xs/12-boolean.png#lightbox)


## <a name="grouped-properties"></a>그룹화 된 속성

일부 위젯이 함께 그룹화 된 다중 값 속성 (같은 `Padding`예를 들면). 이러한 속성 값에 나열 됩니다는 **속성 패드** 를 확장할 수 있는 단일 행에 있습니다. 이러한 속성 중 일부를 편집할 수 있습니다 그룹화 된 행에 직접와 같은 `Padding` 아래에 표시 된 속성:

[![Padding 속성에 대 한 예제 설정](designer-basics-images/xs/13-padding-property-sml.png)](designer-basics-images/xs/13-padding-property.png#lightbox)

-----


## <a name="editing-properties-inline"></a>속성 인라인 편집

Android 디자이너 (하므로 속성 목록에서 이러한 속성을 검색할 필요가 없습니다) 디자인 화면에서 특정 속성을 직접 편집을 지원 합니다. 직접 편집할 수 있는 속성에는 텍스트, 여백 및 크기 포함 됩니다.

### <a name="text"></a>텍스트

일부 위젯의 텍스트 속성 (예: `Button` 및 `TextView`), 디자인 화면에서 직접 편집할 수 있습니다. 위젯을 두 번 클릭 하는 모드로 설정 편집, 아래와 같이:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Hello 문자열에 대 한 텍스트 리소스](designer-basics-images/vs/12-text-resource.png "텍스트 리소스")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Hello 문자열에 대 한 텍스트 리소스](designer-basics-images/xs/14-text-resource-sml.png)](designer-basics-images/xs/14-text-resource.png#lightbox)

-----

새 텍스트 값을 입력 하거나 새 리소스 문자열을 입력할 수 있습니다. 다음 예제에서는 `@string/hello` 리소스는 텍스트를 바꾸려는 `CLICK THIS BUTTON`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Shift + Enter를 새 리소스에 텍스트를 자동으로 링크](designer-basics-images/vs/13-shift-enter-resource.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Shift + Enter를 새 리소스에 텍스트를 자동으로 링크](designer-basics-images/xs/15-shift-enter-resource-sml.png)](designer-basics-images/xs/15-shift-enter-resource.png#lightbox)

-----

이 변경 위젯의에 저장 됩니다 `text` 속성에 할당 된 값을 수정 하지 않습니다는 `@string/hello` 리소스입니다.
경우 새 텍스트 문자열의 키를 누르면 <kbd>Shift</kbd> +
<kbd>Enter</kbd> 에 자동으로 입력 한 텍스트를 새 리소스에 연결 합니다.


### <a name="margin"></a>여백

위젯을 선택 하면 디자이너는 크기 또는 위젯의 여백을 대화형으로 변경할 수 있도록 하는 핸들을 표시 합니다. 여백 편집 모드와 크기 편집 모드 간에 전환 선택 되어 위젯을 클릭 하면 됩니다.

처음에 대 한 위젯을 클릭할 때 여백 핸들이 표시 됩니다. 마우스를 이동 하는 핸들 중 하나를 하는 경우 디자이너는 핸들을 변경 하는 속성을 표시 (아래에 나와 있는 것 처럼는 `layout_marginLeft` 속성):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![디자이너에서 처리 하는 여백을 보여 주는 스크린샷](designer-basics-images/vs/15-margin-handles.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![디자이너에서 처리 하는 여백을 보여 주는 스크린샷](designer-basics-images/xs/16-margin-handles-sml.png)](designer-basics-images/xs/16-margin-handles.png#lightbox)

-----

여백을 이미 설정 된 경우 여백 차지 하는 간격을 나타내는 점선이 표시 됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![점선은 단추 주위의 공간을 표시의 예](designer-basics-images/vs/16-margins-set.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![점선은 단추 주위의 공간을 표시의 예](designer-basics-images/xs/17-margins-set-sml.png)](designer-basics-images/xs/17-margins-set.png#lightbox)

-----



### <a name="size"></a>크기

앞서 언급 했 듯이 이미 선택 되어 위젯을 클릭 하 여 크기 편집 모드로 전환할 수 있습니다. 삼각가 중 핸들에 표시 된 차원에 대 한 크기를 설정 하려면 클릭 `wrap_content`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![래핑 콘텐츠 및 크기 조정 핸들](designer-basics-images/vs/17-wrap-content.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![래핑 콘텐츠 및 크기 조정 핸들](designer-basics-images/xs/18-wrap-content-sml.png)](designer-basics-images/xs/18-wrap-content.png#lightbox)

-----

클릭 하는 **래핑할 콘텐츠** 핸들 축소 하도록 포함 된 콘텐츠를 래핑하는 데 필요한 보다 작은 해당 차원에 위젯입니다. 이 예제에서는 단추 텍스트를 가로로 다음 스크린샷에 표시 된 대로 축소 합니다.

크기 값은로 설정 되 면 **콘텐츠 래핑할**, 디자이너가 표시 크기를 변경 하는 데 반대 방향을 가리키는 삼각가 중 핸들 `match_parent`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![일치 항목이 부모 핸들](designer-basics-images/vs/18-match-parent.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![일치 항목이 부모 핸들](designer-basics-images/xs/19-match-parent-sml.png)](designer-basics-images/xs/19-match-parent.png#lightbox)

-----

클릭 하 고 **일치 부모** 핸들 복원 해당 차원에 대 한 크기를 부모 위젯와 같습니다.

또한 끌 수 있는 순환 크기 조정 핸들 (같이 위의 스크린샷에서) 위젯을 임의의 크기를 조정 `dp` 값입니다. 이렇게 하면 둘 다 **콘텐츠 래핑할** 및 **일치 부모** 핸들 해당 차원에 대해 제공 됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![순환 크기 조정 핸들](designer-basics-images/vs/19-resize-dp.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![순환 크기 조정 핸들](designer-basics-images/xs/20-resize-dp-sml.png)](designer-basics-images/xs/20-resize-dp.png#lightbox)

-----

편집할 수는 모든 컨테이너는 `Size` 위젯의 합니다. 예를 들어, 다음에 유의와 아래 스크린샷에서 `LinearLayout` 선택, 크기 조정 핸들 표시 되지 않습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![없음, 크기 조정 핸들](designer-basics-images/vs/20-no-resize-handles.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![없음, 크기 조정 핸들](designer-basics-images/xs/21-no-resize-handles-sml.png)](designer-basics-images/xs/20-no-resize-handles.png#lightbox)

-----



## <a name="document-outline"></a>문서 개요

**문서 개요** 레이아웃의 위젯 계층 구조를 표시 합니다.
포함 하는 다음 예에서 `LinearLayout` 위젯 선택:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![문서 개요](designer-basics-images/vs/21-document-outline.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![문서 개요](designer-basics-images/xs/22-outline-view-sml.png)](designer-basics-images/xs/22-outline-view.png#lightbox)

-----

선택한 위젯의 개요 (이 경우는 `LinearLayout`)도 디자인 화면에서 강조 표시 합니다. 문서 개요 창에서 선택한 위젯 디자인 화면에 상응 하는 동기화 된 상태로 유지 됩니다. 이것은 쉽게 디자인 화면에서 선택할 수 없는 보기 그룹을 선택 하면 유용 합니다.

문서 개요의 복사 / 붙여넣기를 지원 하거나 끌어서를 사용 하 고 삭제할 수 있습니다. 문서 개요 디자인 화면 뿐 아니라 디자인 화면에 문서 개요의 끌어서 놓기 지원 됩니다. 또한 문서 개요의 항목을 마우스 오른쪽 단추로 클릭 (동일한 상황에 맞는 메뉴 해당 동일한 위젯 디자인 화면에서 마우스 오른쪽 단추로 클릭할 때 표시 되는) 해당 항목에 대 한 상황에 맞는 메뉴를 표시 합니다.
