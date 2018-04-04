---
title: 자재 디자인 기능
description: 이 항목을 쉽게 디자인을 규격 자재 레이아웃을 만드는 개발자를 위한 디자이너 기능에 설명 합니다. 이 여기서 소개 하 고 자료 그리드, 자료 색상표, 인쇄 확장 및 테마 편집기를 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: AC55E1B2-C239-4019-B0C3-A16F6CF0D6E0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: a764efe7f2cadd8c777f8427c0220e45eec662c9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="material-design-features"></a>자재 디자인 기능

_이 항목을 쉽게 디자인을 규격 자재 레이아웃을 만드는 개발자를 위한 디자이너 기능에 설명 합니다. 이 여기서 소개 하 고 자료 그리드, 자료 색상표, 인쇄 확장 및 테마 편집기를 사용 하는 방법에 설명 합니다._


> [!Video https://youtube.com/embed/E3_ZjIOzVzY]

**2016 개선: 모든 앱을 만들 수 Beautiful 자재 디자인**

## <a name="overview"></a>개요

Xamarin.Android 디자이너 쉽게 자료 디자인-규격 레이아웃을 만들 수 있도록 하는 기능이 포함 되어 있습니다. 자료 디자인에 잘 알고 경우 참조는 [디자인 소개 자료](https://www.google.com/design/spec/material-design/introduction.html)합니다.

이 가이드에서는 다음 디자이너 기능을 참조를 해야 했습니다.

-   *자재 그리드* &ndash; 그리드, 간격 및 레이아웃 위젯 자료 디자인 지침에 따라 배치할 수 있도록 keylines 보여 주는 디자인 화면에서 중첩 합니다.

-   *자재 색상표* &ndash; 공식 자료 디자인 팔레트에서 색을 선택 하는 데 도움이 되는 속성 패드 대화 상자.

-   *인쇄 눈금* &ndash; 자재 디자인-호환 되는 설정에 대 한 선택 옵션이 제공 하는 속성 패드 대화는 `textAppearance` 텍스트 필드의 속성이 모두 있습니다.

-   *테마 편집기* &ndash; 수 있는 작은 색 리소스 편집기 테마의 하위 집합에 대 한 색 정보를 설정 합니다. 미리 보기 및 재질 색과 같은 수정 수는 예를 들어 `colorPrimary`, `colorPrimaryDark`, 및 `colorAccent`합니다.

이러한 각 기능 확인 했으며 사용 하는 방법의 예를 제공 합니다.



## <a name="material-design-grid"></a>자재 디자인 눈금

자료 디자인 눈금 메뉴는 디자이너 맨 위에 있는 도구 모음에서 제공 됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![자재 디자인 눈금](material-design-features-images/vs/01-material-design-grid-sml.png)](material-design-features-images/vs/01-material-design-grid.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![자재 디자인 눈금](material-design-features-images/xs/01-material-design-grid-sml.png)](material-design-features-images/xs/01-material-design-grid.png#lightbox)

-----

자료 디자인 눈금 아이콘을 클릭 하면 디자이너는 다음과 같은 요소가 포함 된 디자인 화면에 오버레이 표시:

-   Keylines (주황색 줄)

-   간격 (녹색 영역)

-   표 (파랑 선)

다음 스크린샷에서 이러한 요소를 볼 수 있습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![도련을, 간격 및 그리드](material-design-features-images/vs/02-grid-and-keylines-sml.png)](material-design-features-images/vs/02-grid-and-keylines.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![도련을, 간격 및 그리드](material-design-features-images/xs/02-grid-and-keylines-sml.png)](material-design-features-images/xs/02-grid-and-keylines.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

이러한 각 오버레이 항목은 구성할 수 있습니다. 자료 디자인 눈금 메뉴 옆에 있는 줄임표를 클릭 하면 대화 popover 표를 사용/사용 안 함, keylines의 배치를 구성 하 고는 간격을 설정할 수 있도록 열립니다. 모든 값에 표시 되는 `dp` (밀도 독립적 픽셀):

![그리드, 도련을, 및 간격 구성](material-design-features-images/vs/03-grid-configuration.png)

새 도련을 추가 하려면에 새 오프셋된 값을 입력에서 **오프셋** 상자 위치를 선택 합니다 (**왼쪽**, **top**, **오른쪽**, 또는  **아래쪽**)를 클릭 하 고는 + 새 도련을 추가 하는 아이콘입니다.

마찬가지로, 새 간격을 추가 하려면 크기와 오프셋 (dp)에 입력 된 **크기** 및 **오프셋** 상자에 각각 있습니다. 위치를 선택 (**왼쪽**, **top**, **오른쪽**, 또는 **아래쪽**)를 클릭 하 고는 + 새 간격을 추가 하는 아이콘입니다.

이러한 구성 값을 변경할 때 레이아웃 XML 파일에 저장 되며 레이아웃을 다시 열 때 다시 사용 됩니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

이러한 각 오버레이 항목은 구성할 수 있습니다. 자료 디자인 눈금 메뉴 옆에 있는 아래쪽 삼각형을 클릭 하 여 표를 사용/사용 안 함, keylines의 배치를 구성 하 고는 간격을 설정 할 수 있는 대화 popover가 열립니다. 모든 값에 표시 되는 `dp` (밀도 독립적 픽셀):

[![그리드, 도련을, 및 간격 구성](material-design-features-images/xs/03-grid-configuration-sml.png)](material-design-features-images/xs/03-grid-configuration.png#lightbox)

새 도련을 추가 하려면에 새 오프셋된 값을 입력에서 **오프셋** 상자 위치를 선택 합니다 (**왼쪽**, **top**, **오른쪽**, 또는  **아래쪽**)를 클릭 하 고는 + 새 도련을 추가 하는 아이콘입니다.

마찬가지로, 새 간격을 추가 하려면 크기와 오프셋 (dp)에 입력 된 **크기** 및 **오프셋** 상자에 각각 있습니다. 위치를 선택 (**왼쪽**, **top**, **오른쪽**, 또는 **아래쪽**)를 클릭 하 고는 + 새 간격을 추가 하는 아이콘입니다.

이러한 구성 값을 변경할 때 레이아웃 XML 파일에 저장 되며 레이아웃을 다시 열 때 다시 사용 됩니다.


## <a name="material-design-color-palette"></a>자재 디자인 색상표

이제 색을 허용 하는 모든 속성 패널 항목이이 스크린 샷에 표시 된 것 처럼를 열려면 디자인 자료 색상표를 사용할 수 있는 추가 아이콘에 있습니다.

[![색 아이콘](material-design-features-images/xs/04-new-color-icon-sml.png)](material-design-features-images/xs/04-new-color-icon.png#lightbox)

이 아이콘을 클릭할 때 대화 popover 자료 디자인 색상표에서 해당 속성의 색을 구성할 수 있게 해 주는 열립니다.

[![자재 디자인 색상표](material-design-features-images/xs/05-material-palette-sml.png)](material-design-features-images/xs/05-material-palette.png#lightbox)

색 색상표 색상표의 맨 아래 선택 된 기본 색에 대 한 색상의 범위를 표시 하는 동안 기본 자료 디자인 색을 표시 합니다. 예를 들어 선택 **Indigo**의 컬렉션인 **Indigo** 색조 대화 상자의 맨 아래에 표시 됩니다.
색상을 선택 하면 속성의 색 선택 된 색상으로 변경 됩니다. 다음 예제에서는 `Background Tint` 으로 변경 되는 단추의 *Indigo 500*:

[![Indigo 500 선택](material-design-features-images/xs/06-indigo-sml.png)](material-design-features-images/xs/06-indigo.png#lightbox)

`Background Tint` 에 대 한 색 코드로 설정 되어 *Indigo 500* (`#ff3f51b5`),이 변경 내용을 적용 하는 단추의 배경색을 업데이트 하는 디자이너 및:

[![배경 tint 변경 내용](material-design-features-images/xs/07-background-tint-sml.png)](material-design-features-images/xs/07-background-tint.png#lightbox)

자료 디자인 색상표에 대 한 자세한 내용은 참조 자료 디자인 [색 색상표 가이드](http://www.google.com/design/spec/style/color.html#color-color-palette)합니다.

## <a name="typographic-scale"></a>인쇄 크기 조정

**텍스트 모양** 의 섹션은 **속성** 패드 **스타일** 탭 수 있는 아이콘에 중에서 선택는 `TextAppearance` 준수 자료 디자인 하는 스타일 사양입니다.

[![스타일 탭](material-design-features-images/xs/08-typo-scale-icon-sml.png)](material-design-features-images/xs/08-typo-scale-icon.png#lightbox)

이 아이콘을 클릭 하면 열립니다는 **인쇄 눈금** 을 선택할 수 있는 미리 구성 된 텍스트 스타일의 목록을 나타내는 대화 popover:

[![텍스트 스타일 선택](material-design-features-images/xs/09-text-appearance-sml.png)](material-design-features-images/xs/09-text-appearance.png#lightbox)

클릭 하면 다음 예제에서는 **디스플레이 1** 단추의 텍스트를 큰 글꼴로 변경 **디스플레이 1**:

[![1 표시 스타일](material-design-features-images/xs/10-display-1-sml.png)](material-design-features-images/xs/10-display-1.png#lightbox)

텍스트 스타일에는 **인쇄 눈금** 따르는 **테마** 설정 합니다. 예를 들어 경우는 **Light** 는 미러의 목록에 사용 가능한 텍스트 스타일 디자이너 테마를 선택는 **Light** 테마:

[![밝은 테마](material-design-features-images/xs/11-light-theme-sml.png)](material-design-features-images/xs/11-light-theme.png#lightbox)

-----



## <a name="theme-editor"></a>테마 편집기

**테마 편집기** 테마 특성의 하위 집합에 대 한 색 정보를 사용자 지정할 수 있습니다. 열려는 **테마 편집기**, 도구 모음에서 페인트 브러시 아이콘을 클릭 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![테마 편집기 아이콘](material-design-features-images/vs/04-theme-editor-icon.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![테마 편집기 아이콘](material-design-features-images/xs/12a-theme-editor-icon-sml.png)](material-design-features-images/xs/12a-theme-editor-icon.png#lightbox)

-----

하지만 **테마 편집기** 은 도구 모음에서 액세스할 수 있는 모든 대상 Android 버전 및 API 수준 아래에 설명 된 기능의 하위 집합 대상 API 수준 (Android 5.0 API 21 보다 이전 이면 사용할 수 있는 롤리팝을) 합니다.

왼쪽 패널에서 **테마 편집기** 현재 선택 된 테마를 구성 하는 색 목록이 표시 됩니다 (이 예제에서 사용 하 여는 `Default Theme`):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![테마 편집기](material-design-features-images/vs/05-theme-editor-sml.png)](material-design-features-images/vs/05-theme-editor.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![테마 편집기](material-design-features-images/xs/12b-theme-editor-sml.png)](material-design-features-images/xs/12b-theme-editor.png#lightbox)

-----

왼쪽에서 색을 선택 하면 해당 색상을 편집할 수 있도록 다음과 같은 탭을 오른쪽 패널에 제공 합니다.

-   **상속** &ndash; 선택한 색에 대 한 스타일 상속 다이어그램을 표시 하 고 확인 된 색과 해당 테마 색에 할당 된 색상 코드를 나열 합니다.

-   **색 선택기** &ndash; 선택한 임의의 값으로 색을 변경할 수 있습니다.

-   **자재 색상표** &ndash; 변경할 자료 디자인을 준수 하는 값을 색으로 선택한 수 있습니다.

-   **리소스** &ndash; 색 테마에는 리소스를 다른 기존 중 하나에 선택한 색을 변경할 수 있습니다.

자세히 이러한 탭 중 하나를 각를 살펴 보겠습니다.



### <a name="inherit-tab"></a>탭을 상속 합니다.

다음 예에서 같이 **상속** 탭에 대 한 스타일 상속에 나열 된 **배경** 의 색은 **기본 테마**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![탭을 상속 합니다.](material-design-features-images/vs/06-inherit-tab-sml.png)](material-design-features-images/vs/06-inherit-tab.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![탭을 상속 합니다.](material-design-features-images/xs/13-inherit-sml.png)](material-design-features-images/xs/13-inherit.png#lightbox)

-----

이 예제는 **기본 테마** 사용 하는 스타일에서 상속 `@color/background_material_dark` 사용 하 여 재정의 하지만 `color/material_grey_850`, 색상 코드 값이 있는 `#ff303030`합니다.
스타일 상속 하는 방법에 대 한 자세한 내용은 참조 [스타일 및 테마](http://developer.android.com/guide/topics/ui/themes.html#Inheritance)합니다.



### <a name="color-picker"></a>색 선택

에서는 다음 스크린 샷에서 **색상 선택기**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![색 선택](material-design-features-images/vs/07-color-picker-sml.png)](material-design-features-images/vs/07-color-picker.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![색 선택](material-design-features-images/xs/14-color-picker-sml.png)](material-design-features-images/xs/14-color-picker.png#lightbox)

-----

이 예제는 **배경** 다양 한 방법을 통해 값으로 색을 변경할 수 있습니다.

-   색을 직접 클릭합니다.
-   색상, 채도 및 밝기 값을 입력 합니다.
-   10 진수에서 RGB (빨강, 녹색, 파랑) 값을 입력 합니다.
-   선택한 색의 알파 (불투명도)을 설정 합니다.
-   16 진수 색상 코드를 직접 입력 합니다.

색 선택에서 선택한 색은 *하지* 자료 디자인 지침 또는 사용 가능한 색 리소스 집합을 제한 합니다.


### <a name="resources"></a>자료

**리소스** 탭 테마에 존재 하는 색 리소스의 목록을 제공 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![리소스](material-design-features-images/vs/08-resources-sml.png)](material-design-features-images/vs/08-resources.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![리소스](material-design-features-images/xs/15-resources-sml.png)](material-design-features-images/xs/15-resources.png#lightbox)

-----

사용 하는 **리소스** 탭 색이이 목록에 선택한 내용을 제한 합니다. 테마의 다른 부분에 이미 할당 된 색 리소스를 선택 하면 UI의 인접 요소 두 개 있습니다 "함께 실행" (했기 때문에 동일한 색) 염두에서에 둬야 구분 하기 위해 사용자에 대 한 어려워질 및 합니다.



### <a name="material-palette"></a>자재 색상표

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**자료 색상표** 탭이 열리고는 **자료 디자인 색상표**합니다. 색 선택을 자료 디자인 지침과 일치 되도록 제한이 색상표에서 색 값을 선택 합니다.

[![자재 색상표](material-design-features-images/vs/09-material-palette-sml.png)](material-design-features-images/vs/09-material-palette.png#lightbox)

색 색상표 색상표의 맨 아래 선택 된 기본 색에 대 한 색상의 범위를 표시 하는 동안 기본 자료 디자인 색을 표시 합니다. 예를 들어 선택 **Indigo**의 컬렉션인 **Indigo** 색조 대화 상자의 맨 아래에 표시 됩니다.
색상을 선택 하면 속성의 색 선택 된 색상으로 변경 됩니다. 다음 예제에서는 `Background Tint` 으로 변경 되는 단추의 *Indigo 500*:

![Indigo 500 선택](material-design-features-images/vs/10-indigo.png)

`Background Tint` 에 대 한 색 코드로 설정 되어 *Indigo 500* (`#ff3f51b5`), 디자이너 배경 색이이 변경 내용을 반영 하도록 업데이트 합니다.

[![배경 tint 변경](material-design-features-images/vs/11-background-tint-sml.png)](material-design-features-images/vs/11-background-tint.png#lightbox)

자료 디자인 색상표에 대 한 자세한 내용은 참조 자료 디자인 [색 색상표 가이드](http://www.google.com/design/spec/style/color.html#color-color-palette)합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**자료 색상표** 탭이 열리고는 **자료 디자인 색상표** 설명 [이전](#material_palette)합니다. 색 선택을 자료 디자인 지침과 일치 되도록 제한이 색상표에서 색 값을 선택 합니다.

[![자재 색상표](material-design-features-images/xs/16-material-palette-sml.png)](material-design-features-images/xs/16-material-palette.png#lightbox)

-----



### <a name="creating-a-new-theme"></a>새 테마 만들기

다음 예에서 새 사용자 지정 테마를 만들려면 자료 색상표 사용 합니다. 첫째, 이번에 변경 된 **배경** 색상을 *파란색 900*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![배경이 파란색 900 변경](material-design-features-images/vs/12-change-background-to-blue.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![배경이 파란색 900 변경](material-design-features-images/xs/17-change-background-to-blue-sml.png)](material-design-features-images/xs/17-change-background-to-blue.png#lightbox)

-----


색 리소스 변경 되 면 팝업 메시지가 메시지와 함께 *현재 테마에 변경 내용을 저장 하지 않은*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![경고 저장 되지 않은 변경](material-design-features-images/vs/13-unsaved-changes-sml.png)](material-design-features-images/vs/13-unsaved-changes.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![경고 저장 되지 않은 변경](material-design-features-images/xs/18-unsaved-changes-sml.png)](material-design-features-images/xs/18-unsaved-changes.png#lightbox)

-----


**배경** 새 색 선택 항목에 디자이너에서 색이 변경 되었지만이 변경 내용을 아직 저장 되지 않았습니다. 변경 내용을 처리 하는 방법에 대 한 두 가지 선택 사항은 제공 됩니다.

-   **변경 내용을 취소** &ndash; 새 색 선택 항목 (또는 선택 사항)을 삭제 하 고 테마를 원래 상태로 되돌립니다.

-   **새 테마 만들기** &ndash; 현재 선택 된 테마에서 파생 되 고에서 색 재정의 사용 하는 새 테마를 만듭니다는 **테마 편집기**합니다.

클릭할 때 **새 테마 만들기**, 선택한 테마에 따라 다음 중 하나가 발생 합니다.

-   디자이너에서 현재 선택 된 테마 프로젝트 테마 없으면 (사용자 지정된 테마의 부모로 선택한 테마 사용) 사용자 지정 된 테마와 새 리소스 파일을 만들 수 있는 옵션이 표시 됩니다. 디자이너를 만든 후 사용자 지정된 테마를 전환 합니다.

-   현재 선택한 테마는 한 위치에 정의 된 프로젝트 테마, 있으면으로 표시 됩니다는 **업데이트 테마** 옵션도 있습니다;이 옵션을 클릭 하면 새로 만드는 것 보다는 현재 선택 된 테마를 업데이트 합니다.

-   현재 선택한 테마는 여러 위치에 정의 된 프로젝트 테마 하는 경우 (예를 들어에서 다르게-접미사로 **값** 폴더와 같은 **값 v21**)를 묻는 대화 상자가 표시 됩니다 에 대 한 사용자 지정된 테마를 저장 하기 위한 새 위치입니다.

계속 클릭 하면 앞의 예제 **새 테마 만들기** 새 테마 만들기의 결과 호출 **사용자 지정**:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![추가 사용자 지정 테마](material-design-features-images/vs/14-custom-theme-sml.png)](material-design-features-images/vs/14-custom-theme.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![추가 사용자 지정 테마](material-design-features-images/xs/19-custom-theme-sml.png)](material-design-features-images/xs/19-custom-theme.png#lightbox)

-----


현재 선택한 테마 프로젝트 테마 아니므로 대화 상자가 없습니다 선택한 테마를 업데이트 하거나 새 위치를 지정 합니다.


## <a name="summary"></a>요약

이 항목 Xamarin.Android 디자이너에서 제공 되는 자료 디자인 기능을 설명합니다. 이 자료 디자인 눈금을 구성 하는 방법, 자료 디자인 색상표를 사용 하 여 색 속성을 편집 하는 방법 및 텍스트 속성을 구성 하려면 인쇄 배율 선택기를 사용 하는 방법을 설명 합니다. 도 자료 디자인 지침을 따르는 새 사용자 지정 테마를 만들려면 테마 편집기를 사용 하는 방법을 보여 줍니다. 자료 디자인에 대 한 Xamarin.Android 지원에 대 한 자세한 내용은 참조 [자료 테마](~/android/user-interface/material-theme.md)합니다.



## <a name="related-links"></a>관련 링크

- [재질 테마](~/android/user-interface/material-theme.md)
- [자재 디자인 소개](https://www.google.com/design/spec/material-design/introduction.html)
