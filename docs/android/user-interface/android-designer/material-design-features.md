---
title: Xamarin.Android 디자이너 재질 디자인 기능
description: 이 항목에서는 쉽게 재료 디자인 규격 레이아웃을 만드는 개발자를 위한 디자이너 기능을 설명 합니다. 이 섹션에서는 소개 하 고 자료 표, 자료 색상표, 인쇄 확장 및 테마 편집기를 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: AC55E1B2-C239-4019-B0C3-A16F6CF0D6E0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 5e6d7b4bdfdf7ea48d26537cb41c763656b050e0
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669656"
---
# <a name="xamarinandroid-designer-material-design-features"></a>Xamarin.Android 디자이너 재질 디자인 기능

_이 항목에서는 쉽게 재료 디자인 규격 레이아웃을 만드는 개발자를 위한 디자이너 기능을 설명 합니다. 이 섹션에서는 소개 하 고 자료 표, 자료 색상표, 인쇄 확장 및 테마 편집기를 사용 하는 방법에 설명 합니다._

> [!Video https://youtube.com/embed/E3_ZjIOzVzY]

**Evolve 2016: 재질 디자인을 사용 하 여 멋진 앱을 만들 수 있습니다 모든 사람**

## <a name="overview"></a>개요

Xamarin.Android 디자이너에는 쉽게 재료 디자인 규격이 아닌 레이아웃을 만들 수 있는 기능이 포함 됩니다. 재질 디자인을 잘 모르는 경우 참조를 [재질 디자인 소개](https://material.io/design/introduction)합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

이 가이드에서는 다음과 같은 디자이너 기능을 확인을 해야 합니다.

-   *재질 그리드* &ndash; 표, 간격 및 재료 디자인 지침에 따라 레이아웃 위젯을 배치 하는 데 keylines 보여 주는 디자인 화면에 오버레이 합니다.

-   *테마 편집기* &ndash; 수 있는 작은 색 리소스 편집기 테마의 하위 집합에 대 한 색 정보를 설정 합니다. 미리 보기 같은 재질 색을 수정 하는 예를 들어 `colorPrimary`, `colorPrimaryDark`, 및 `colorAccent`합니다.

에서는 이러한 각 기능 확인 하 고 사용 하는 방법의 예제를 제공 합니다.

## <a name="material-design-grid"></a>재질 디자인 눈금

재질 디자인 눈금 메뉴는 디자이너의 맨 위에 있는 도구 모음에서 제공 됩니다.

[![재질 디자인 눈금](material-design-features-images/vs/01-material-design-grid-w158-sml.png)](material-design-features-images/vs/01-material-design-grid-w158.png#lightbox)

재질 디자인 눈금 아이콘을 클릭 하면 다음 요소가 포함 된 디자인 화면에 오버레이 표시 하는 디자이너:

-   Keylines (주황색 선)

-   간격 (녹색 영역)

-   표 (파란색 선)

이러한 요소는 이전 스크린샷에서 볼 수 있습니다. 이러한 각 오버레이 항목은 구성할 수 있습니다. 재질 디자인 눈금 메뉴 옆의 줄임표를 클릭 하면 대화 팝 오버는 표를 사용/사용 안 함, keylines, 배치를 구성 하 고 간격을 설정할 수 있는 열립니다. 모든 값으로 표현 되는 참고 `dp` (밀도 독립적 픽셀).

[![표, 도련을, 및 간격 구성](material-design-features-images/vs/03-grid-configuration-w158-sml.png)](material-design-features-images/vs/03-grid-configuration-w158.png#lightbox)

새 도련을 추가 하려면 새 오프셋된 값을 입력 합니다 **오프셋** 상자에서 위치를 선택 (**왼쪽**를 **위쪽**, **오른쪽**, 또는  **아래쪽**) 클릭 하 고는 + 새 도련을 추가 하는 아이콘입니다. 마찬가지로, 새 간격을 추가 하려면 입력 크기와 오프셋 (dp)에 **크기** 하 고 **오프셋** 상자에 각각. 위치를 선택 (**왼쪽**, **위쪽**를 **오른쪽**, 또는 **아래쪽**) 클릭 하 고는 + 새 간격을 추가 하려면 아이콘을 합니다.

이러한 구성 값을 변경 하면 레이아웃 XML 파일에 저장 되며 레이아웃을 다시 열 때 다시 사용 합니다.


## <a name="theme-editor"></a>테마 편집기

합니다 **테마 편집기** 테마 특성의 하위 집합에 대 한 색 정보를 사용자 지정할 수 있습니다. 여는 **테마 편집기**, 도구 모음에서 페인트 브러시 아이콘을 클릭 합니다.

[![테마 편집기 아이콘](material-design-features-images/vs/04-theme-editor-icon-w158-sml.png)](material-design-features-images/vs/04-theme-editor-icon-w158.png#lightbox)

하지만 합니다 **테마 편집기** 은 도구 모음에서 액세스할 수 있는 모든 대상 Android 버전 및 API 수준에 대 한 아래에 설명 된 기능의 하위 집합이 대상 API 수준 (Android 5.0 API 21 보다 이전인 경우에 사용할 수 있습니다. Lollipop)입니다.

왼쪽 패널의 **테마 편집기** 현재 선택된 된 테마를 구성 하는 색 목록이 표시 됩니다 (이 예에서는 사용를 `Default Theme`):

[![테마 편집기](material-design-features-images/vs/05-theme-editor-w158-sml.png)](material-design-features-images/vs/05-theme-editor-w158.png#lightbox)

왼쪽의 색을 선택 하면 해당 색을 편집 하는 데는 다음과 같은 탭을 오른쪽 패널에 제공 합니다.

-   **상속** &ndash; 선택한 색에 대 한 스타일 상속 다이어그램을 표시 하 고 해결된 색 및 해당 테마 색을 할당 하는 색 코드를 나열 합니다.

-   **색 선택기** &ndash; 임의의 값으로 선택한 색을 변경할 수 있습니다.

-   **재질 팔레트** &ndash; 재료 디자인에 맞는 값으로 선택한 색을 변경할 수 있습니다.

-   **리소스** &ndash; 테마의 다른 기존 색 리소스 중 하나에 선택한 색을 변경할 수 있습니다.

이러한 세부 정보이 탭 각각 살펴보겠습니다.

### <a name="inherit-tab"></a>탭 상속

다음 예에서 볼 수 있듯이 **상속** 탭에 대 한 스타일 상속을 나열 합니다 **백그라운드** 의 색을 **기본 테마**:

[![탭 상속](material-design-features-images/vs/06-inherit-tab-w158-sml.png)](material-design-features-images/vs/06-inherit-tab-w158.png#lightbox)

이 예는 **기본 테마** 사용 하는 스타일에서 상속 `@color/background_material_light` 사용 하 여 재정의 하지만 `color/material_grey_50`, 색 코드 값은 `#fffafafa`합니다.
스타일 상속에 대 한 자세한 내용은 참조 하세요. [스타일과 테마](https://developer.android.com/guide/topics/ui/themes.html#Inheritance)합니다.

### <a name="color-picker"></a>색 선택

다음 스크린샷에서 보여 줍니다 합니다 **색 선택**:

[![색 선택](material-design-features-images/vs/07-color-picker-w158-sml.png)](material-design-features-images/vs/07-color-picker-w158.png#lightbox)

이 예제는 **백그라운드** 다양 한 수단을 통해 값으로 색을 변경할 수 있습니다.

-   직접 색을 클릭합니다.
-   색상, 채도 및 밝기 값을 입력 합니다.
-   10 진수에서 RGB (빨강, 녹색, 파랑) 값을 입력 합니다.
-   선택한 색의 알파 (불투명)를 설정 합니다.
-   16 진수 색 코드를 직접 입력 합니다.

색 선택에서 선택한 색은 *되지* 재료 디자인 지침 또는 사용할 수 있는 색 리소스 집합을 제한 합니다.

### <a name="resources"></a>자료

합니다 **리소스** 탭 테마에 존재 하는 색 리소스의 목록을 제공 합니다.

[![Resources](material-design-features-images/vs/08-resources-w158-sml.png)](material-design-features-images/vs/08-resources-w158.png#lightbox)

사용 하는 **리소스** 탭 색이이 목록에 선택 항목을 제한 합니다. 염두에 테마의 다른 부분에 이미 할당 되어 있는 색 리소스를 선택 하면 두 인접 요소의 UI "겹칠 수도" (있기 때문에 동일한 색)와 구분 하기 위해 사용자에 대 한 어렵습니다.

### <a name="material-palette"></a>재질 팔레트

합니다 **재질 팔레트** 탭이 열립니다 합니다 **재료 디자인 색상표**합니다. 재료 디자인 지침에 부합 되도록에 색 선택을 제한이 색상표에서 색 값을 선택 합니다.

[![재질 팔레트](material-design-features-images/vs/09-material-palette-w158-sml.png)](material-design-features-images/vs/09-material-palette-w158.png#lightbox)

색상표에서 맨 팔레트의 맨 아래 선택한 기본 색에 대 한 색상의 범위를 표시 하는 동안 기본 재질 디자인 색을 표시 합니다. 예를 들어, 선택 하면 **Indigo**, 컬렉션인 **Indigo** 색상 대화 상자의 맨 아래에 표시 됩니다.
색상을 선택 하면 색 속성을 선택한 색상으로 변경 됩니다. 다음 예제에서는 `Background Tint` 로 변경 되는 단추의 *Indigo 500*:

![Indigo 500를 선택 합니다.](material-design-features-images/vs/10-indigo-w158.png)

`Background Tint` 에 대 한 색 코드로 설정 됩니다 *Indigo 500* (`#ff3f51b5`), 디자이너 배경 색이이 변경 내용을 반영 하도록 업데이트 합니다.

[![변경 하는 백그라운드 색조](material-design-features-images/vs/11-background-tint-w158-sml.png)](material-design-features-images/vs/11-background-tint-w158.png#lightbox)

재질 디자인 색상표에 대 한 자세한 내용은 참조 자료 디자인 [색 색상표 가이드](https://material.io/design/color/)합니다.

### <a name="creating-a-new-theme"></a>새 테마 만들기

다음 예에서 새 사용자 지정 테마를 만들려면 자료 색상표를 사용 하겠습니다. 에서는 변경 먼저 합니다 **백그라운드** 색 *파란색 900*:

![파란색 900 배경 변경](material-design-features-images/vs/12-change-background-to-blue-w158.png)

색 리소스 변경 되 면 팝업 메시지가 메시지와 함께 *현재 테마에 저장 되지 않은 변경 내용을*:

[![경고 저장 되지 않은 변경 내용](material-design-features-images/vs/13-unsaved-changes-w158-sml.png)](material-design-features-images/vs/13-unsaved-changes-w158.png#lightbox)

합니다 **백그라운드** 새 색 선택 항목에 디자이너에서 색 변경 되었지만이 변경 아직 저장 되지 않았습니다. 이 시점에서 다음 중 하나를 수행할 수 있습니다.

-   클릭 **변경 내용 취소** 에 새 색 선택 (또는 선택)를 삭제 하 고 테마를 원래 상태로 되돌립니다. 

-   키를 눌러 <kbd>CTRL + S</kbd> 에 변경 내용을 저장 하는 현재 테마입니다. 

다음 예에서 <kbd>CTRL + S</kbd> 에 변경 내용을 저장할 수 있도록 눌렀습니다 **AppTheme**:

[![AppTheme에 저장 하는 변경 내용](material-design-features-images/vs/14-custom-theme-w158-sml.png)](material-design-features-images/vs/14-custom-theme-w158.png#lightbox)

## <a name="summary"></a>요약

이 항목에서는 Xamarin.Android 디자이너에서 제공 되는 재질 디자인 기능을 설명합니다. 사용 하도록 설정 하 여 재질 디자인 눈금을 구성 하는 방법을 설명 했습니다 및 재료 디자인 지침을 따르는 새 사용자 지정 테마를 만들려면 테마 편집기를 사용 하는 방법을 설명 했습니다.
재질 디자인에 대 한 Xamarin.Android 지원에 대 한 자세한 내용은 참조 하세요. [재질 테마](~/android/user-interface/material-theme.md)합니다.




# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

에서는이 가이드에서는 다음과 같은 디자이너 기능을 살펴보겠습니다.

-   *재질 디자인 눈금* &ndash; 표, 간격 및 재료 디자인 지침에 따라 레이아웃 위젯을 배치 하는 데 keylines 보여 주는 디자인 화면에 오버레이 합니다.

-   *재질 디자인 색상표* &ndash; 공식 재료 디자인 색상표에서 색을 선택 하는 데 도움이 되는 속성 패드 대화 합니다.

-   *글꼴 배율* &ndash; 재료 디자인-호환 되는 설정에 대 한 선택 옵션이 제공 되는 속성 패드 대화 상자는 `textAppearance` 텍스트 필드의 속성입니다.

-   *테마 편집기* &ndash; 수 있는 작은 색 리소스 편집기 테마의 하위 집합에 대 한 색 정보를 설정 합니다. 미리 보기 같은 재질 색을 수정 하는 예를 들어 `colorPrimary`, `colorPrimaryDark`, 및 `colorAccent`합니다.

에서는 이러한 각 기능 확인 하 고 사용 하는 방법의 예제를 제공 합니다.

## <a name="material-design-grid"></a>재질 디자인 눈금

재질 디자인 눈금 메뉴는 디자이너의 맨 위에 있는 도구 모음에서 제공 됩니다.

[![재질 디자인 눈금](material-design-features-images/xs/01-material-design-grid-sml.png)](material-design-features-images/xs/01-material-design-grid.png#lightbox)

재질 디자인 눈금 아이콘을 클릭 하면 다음 요소가 포함 된 디자인 화면에 오버레이 표시 하는 디자이너:

-   Keylines (주황색 선)

-   간격 (녹색 영역)

-   표 (파란색 선)

이러한 요소는 다음 스크린샷에 확인할 수 있습니다.

[![도련을, 간격 및 그리드](material-design-features-images/xs/02-grid-and-keylines-sml.png)](material-design-features-images/xs/02-grid-and-keylines.png#lightbox)

이러한 각 오버레이 항목은 구성할 수 있습니다. 줄임표를 클릭 하면 (&hellip;) 재질 디자인 눈금 메뉴 옆에 대화 팝 오버 표를 사용/사용 안 함, keylines, 배치를 구성 하 고는 간격을 설정할 수 있도록 열립니다. 모든 값으로 표현 되는 참고 `dp` (밀도 독립적 픽셀).

[![표, 도련을, 및 간격 구성](material-design-features-images/xs/03-grid-configuration-sml.png)](material-design-features-images/xs/03-grid-configuration.png#lightbox)

새 도련을 추가 하려면 새 오프셋된 값을 입력 합니다 **오프셋** 상자에서 위치를 선택 (**왼쪽**를 **위쪽**, **오른쪽**, 또는  **아래쪽**) 클릭 하 고는 + 아이콘 (값을 입력 한 경우에는 오른쪽에 표시 됨) 새 도련을 추가할 합니다. 마찬가지로, 새 간격을 추가 하려면 입력 크기와 오프셋 (dp)에 **크기** 하 고 **오프셋** 상자에 각각. 위치를 선택 (**왼쪽**, **위쪽**를 **오른쪽**, 또는 **아래쪽**) 클릭 하 고는 + 새 간격을 추가 하려면 아이콘을 합니다.

이러한 구성 값을 변경 하면 레이아웃 XML 파일에 저장 되며 레이아웃을 다시 열 때 다시 사용 합니다.

## <a name="material-design-color-palette"></a>재질 디자인 색상표

이제 색을 허용 하는 모든 속성 패널 항목에이 스크린샷에 표시 된 대로 재료 디자인 색 팔레트를 열려면 사용할 수 있는 추가 색상표 아이콘:

[![색 아이콘](material-design-features-images/xs/04-new-color-icon-sml.png)](material-design-features-images/xs/04-new-color-icon.png#lightbox)

이 아이콘을 클릭 하면 색 재료 디자인 색상표에서 해당 속성을 구성할 수 있게 해 주는 대화 팝 오버가 열립니다.

[![재질 디자인 색상표](material-design-features-images/xs/05-material-palette-sml.png)](material-design-features-images/xs/05-material-palette.png#lightbox)

색상표에서 맨 팔레트의 맨 아래 선택한 기본 색에 대 한 색상의 범위를 표시 하는 동안 기본 재질 디자인 색을 표시 합니다. 예를 들어, 선택 하면 **Indigo**, 컬렉션인 **Indigo** 색상 대화 상자의 맨 아래에 표시 됩니다.
색상을 선택 하면 색 속성을 선택한 색상으로 변경 됩니다. 다음 예제에서는 `Background Tint` 로 변경 되는 단추의 *Indigo 500*:

[![Choose Indigo 500](material-design-features-images/xs/06-indigo-sml.png)](material-design-features-images/xs/06-indigo.png#lightbox)

`Background Tint` 에 대 한 색 코드로 설정 됩니다 *Indigo 500* (`#ff3f51b5`), 디자이너가이 변경 내용을 반영 하는 단추의 배경색을 업데이트 합니다.

[![백그라운드 tint 변경](material-design-features-images/xs/07-background-tint-sml.png)](material-design-features-images/xs/07-background-tint.png#lightbox)

재질 디자인 색상표에 대 한 자세한 내용은 참조 자료 디자인 [색 색상표 가이드](https://material.io/design/color/)합니다.

## <a name="typographic-scale"></a>입력 체계 확장

**텍스트 모양을** 섹션을 **속성** 패드 **스타일** 탭 하면 아이콘이 있습니다에서 선택를 `TextAppearance` 준수 재료 디자인 하는 스타일 사양:

[![스타일 탭](material-design-features-images/xs/08-typo-scale-icon-sml.png)](material-design-features-images/xs/08-typo-scale-icon.png#lightbox)

이 아이콘을 클릭 하면 열립니다는 **입력 체계 확장** 대화 팝 오버에서 선택할 수 있는 미리 구성 된 텍스트 스타일의 목록을 제공 하는:

[![텍스트 스타일 선택](material-design-features-images/xs/09-text-appearance-sml.png)](material-design-features-images/xs/09-text-appearance.png#lightbox)

클릭 하면 다음 예와에서 **디스플레이 1** 단추의 텍스트의 큰 글꼴로 변경 **디스플레이 1**:

[![1 스타일 표시](material-design-features-images/xs/10-display-1-sml.png)](material-design-features-images/xs/10-display-1.png#lightbox)

텍스트 스타일을 **입력 체계 확장** 따르는 **테마** 설정 합니다. 예를 들어 경우는 **Light** 디자이너를 사용할 수 있는 텍스트 스타일 미러의 목록에서 선택 하는 테마를 **Light** 테마:

[![밝은 테마](material-design-features-images/xs/11-light-theme-sml.png)](material-design-features-images/xs/11-light-theme.png#lightbox)

## <a name="theme-editor"></a>테마 편집기

합니다 **테마 편집기** 테마 특성의 하위 집합에 대 한 색 정보를 사용자 지정할 수 있습니다. 여는 **테마 편집기**, 도구 모음에서 페인트 브러시 아이콘을 클릭 합니다.

[![테마 편집기 아이콘](material-design-features-images/xs/12a-theme-editor-icon-sml.png)](material-design-features-images/xs/12a-theme-editor-icon.png#lightbox)

하지만 합니다 **테마 편집기** 은 도구 모음에서 액세스할 수 있는 모든 대상 Android 버전 및 API 수준에 대 한 아래에 설명 된 기능의 하위 집합이 대상 API 수준 (Android 5.0 API 21 보다 이전인 경우에 사용할 수 있습니다. Lollipop)입니다.

왼쪽 패널의 **테마 편집기** 현재 선택된 된 테마를 구성 하는 색 목록이 표시 됩니다 (이 예에서는 사용를 `Default Theme`):

[![테마 편집기](material-design-features-images/xs/12b-theme-editor-sml.png)](material-design-features-images/xs/12b-theme-editor.png#lightbox)

왼쪽의 색을 선택 하면 해당 색을 편집 하는 데는 다음과 같은 탭을 오른쪽 패널에 제공 합니다.

-   **상속** &ndash; 선택한 색에 대 한 스타일 상속 다이어그램을 표시 하 고 해결된 색 및 해당 테마 색을 할당 하는 색 코드를 나열 합니다.

-   **색 선택기** &ndash; 임의의 값으로 선택한 색을 변경할 수 있습니다.

-   **재질 팔레트** &ndash; 재료 디자인에 맞는 값으로 선택한 색을 변경할 수 있습니다.

-   **리소스** &ndash; 테마의 다른 기존 색 리소스 중 하나에 선택한 색을 변경할 수 있습니다.

이러한 세부 정보이 탭 각각 살펴보겠습니다.

### <a name="inherit-tab"></a>탭 상속

다음 예에서 볼 수 있듯이 **상속** 탭에 대 한 스타일 상속을 나열 합니다 **백그라운드** 의 색을 **기본 테마**:

[![탭 상속](material-design-features-images/xs/13-inherit-sml.png)](material-design-features-images/xs/13-inherit.png#lightbox)

이 예는 **기본 테마** 사용 하는 스타일에서 상속 `@color/background_material_dark` 사용 하 여 재정의 하지만 `color/material_grey_850`, 색 코드 값은 `#ff303030`합니다.
스타일 상속에 대 한 자세한 내용은 참조 하세요. [스타일과 테마](https://developer.android.com/guide/topics/ui/themes.html#Inheritance)합니다.

### <a name="color-picker"></a>색 선택

다음 스크린샷에서 보여 줍니다 합니다 **색 선택**:

[![색 선택](material-design-features-images/xs/14-color-picker-sml.png)](material-design-features-images/xs/14-color-picker.png#lightbox)


이 예제는 **백그라운드** 다양 한 수단을 통해 값으로 색을 변경할 수 있습니다.

-   직접 색을 클릭합니다.
-   색상, 채도 및 밝기 값을 입력 합니다.
-   10 진수에서 RGB (빨강, 녹색, 파랑) 값을 입력 합니다.
-   선택한 색의 알파 (불투명)를 설정 합니다.
-   16 진수 색 코드를 직접 입력 합니다.

색 선택에서 선택한 색은 *되지* 재료 디자인 지침 또는 사용할 수 있는 색 리소스 집합을 제한 합니다.

### <a name="resources"></a>자료

합니다 **리소스** 탭 테마에 존재 하는 색 리소스의 목록을 제공 합니다.

[![Resources](material-design-features-images/xs/15-resources-sml.png)](material-design-features-images/xs/15-resources.png#lightbox)

사용 하는 **리소스** 탭 색이이 목록에 선택 항목을 제한 합니다. 염두에 테마의 다른 부분에 이미 할당 되어 있는 색 리소스를 선택 하면 두 인접 요소의 UI "겹칠 수도" (있기 때문에 동일한 색)와 구분 하기 위해 사용자에 대 한 어렵습니다.

### <a name="material-palette"></a>재질 팔레트

합니다 **재질 팔레트** 탭이 열립니다 합니다 **재료 디자인 색상표** 설명 [이전](#material_palette)합니다. 재료 디자인 지침에 부합 되도록에 색 선택을 제한이 색상표에서 색 값을 선택 합니다.

[![재질 팔레트](material-design-features-images/xs/16-material-palette-sml.png)](material-design-features-images/xs/16-material-palette.png#lightbox)

### <a name="creating-a-new-theme"></a>새 테마 만들기

다음 예에서 새 사용자 지정 테마를 만들려면 자료 색상표를 사용 하겠습니다. 에서는 변경 먼저 합니다 **백그라운드** 색 *파란색 900*:

[![파란색 900 배경 변경](material-design-features-images/xs/17-change-background-to-blue-sml.png)](material-design-features-images/xs/17-change-background-to-blue.png#lightbox)

색 리소스 변경 되 면 팝업 메시지가 메시지와 함께 *현재 테마에 저장 되지 않은 변경 내용을*:

[![경고 저장 되지 않은 변경 내용](material-design-features-images/xs/18-unsaved-changes-sml.png)](material-design-features-images/xs/18-unsaved-changes.png#lightbox)

디자이너에서 색 변경 내용이 있지만이 변경 아직 저장 되지 않았습니다. 이 시점에서 다음 중 하나를 수행할 수 있습니다.

-   클릭 **변경 내용 취소** 에 새 색 선택 (또는 선택)를 삭제 하 고 테마를 원래 상태로 되돌립니다. 

-   키를 눌러  **&#8984; + S** 새 테마에 변경 내용을 저장 하려면 호출 **Custom**합니다. 


## <a name="summary"></a>요약

이 항목에서는 Xamarin.Android 디자이너에서 제공 되는 재질 디자인 기능을 설명합니다. 재질 디자인 눈금을 구성 하는 방법, 재질 디자인 색상표를 사용 하 여 색 속성을 편집 하는 방법 및 입력 체계 확장 선택기를 사용 하 여 텍스트 속성을 구성 하는 방법을 설명 했습니다. 또한 재료 디자인 지침을 따르는 새 사용자 지정 테마를 만들려면 테마 편집기를 사용 하는 방법을 보여 줍니다. 재질 디자인에 대 한 Xamarin.Android 지원에 대 한 자세한 내용은 참조 하세요. [재질 테마](~/android/user-interface/material-theme.md)합니다.

-----


## <a name="related-links"></a>관련 링크

- [재질 테마](~/android/user-interface/material-theme.md)
- [재질 디자인 소개](https://material.io/design/introduction)
