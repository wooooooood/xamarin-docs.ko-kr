---
title: Xamarin. Android Designer 재질 디자인 기능
description: 이 항목에서는 개발자가 재질 디자인 호환 레이아웃을 보다 쉽게 만들 수 있도록 하는 디자이너 기능에 대해 설명 합니다. 이 섹션에서는 재질 표, 재질 색상표, 인쇄 단위 배율 및 테마 편집기를 사용 하는 방법을 소개 하 고 설명 합니다.
ms.prod: xamarin
ms.assetid: AC55E1B2-C239-4019-B0C3-A16F6CF0D6E0
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/25/2018
ms.openlocfilehash: 43397fb855bdf872cf17b315044f34a468c22d00
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029443"
---
# <a name="xamarinandroid-designer-material-design-features"></a>Xamarin. Android Designer 재질 디자인 기능

_이 항목에서는 개발자가 재질 디자인 호환 레이아웃을 보다 쉽게 만들 수 있도록 하는 디자이너 기능에 대해 설명 합니다. 이 섹션에서는 재질 표, 재질 색상표, 인쇄 단위 배율 및 테마 편집기를 사용 하는 방법을 소개 하 고 설명 합니다._

> [!Video https://youtube.com/embed/E3_ZjIOzVzY]

**진화 2016: 누구나 재질 디자인을 사용 하 여 멋진 앱을 만들 수 있습니다.**

## <a name="overview"></a>개요

Android Designer에는 재질 디자인 호환 레이아웃을 보다 쉽게 만들 수 있도록 하는 기능이 포함 되어 있습니다. 재질 디자인에 익숙하지 않은 경우 [재질 디자인 소개](https://material.io/design/introduction)를 참조 하세요.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

이 가이드에서는 다음과 같은 디자이너 기능을 살펴봅니다.

- *재질 그리드* 는 재질 디자인 지침에 따라 레이아웃 위젯을 배치 하는 데 도움이 되는 표, 간격 및 keylines를 표시 하는 Design Surface의 오버레이를 &ndash; 합니다.

- *테마 편집기* &ndash; 테마의 하위 집합에 대 한 색 정보를 설정할 수 있는 작은 색 리소스 편집기입니다. 예를 들어 `colorPrimary`, `colorPrimaryDark`, `colorAccent`등의 재질 색을 미리 보고 수정할 수 있습니다.

이러한 각 기능을 살펴보고 사용 방법에 대 한 예제를 제공 합니다.

## <a name="material-design-grid"></a>재질 디자인 그리드

재질 디자인 그리드 메뉴는 디자이너 맨 위에 있는 도구 모음에서 사용할 수 있습니다.

[![재질 디자인 그리드](material-design-features-images/vs/01-material-design-grid-w158-sml.png)](material-design-features-images/vs/01-material-design-grid-w158.png#lightbox)

재질 디자인 그리드 아이콘을 클릭 하면 디자이너는 다음 요소를 포함 하는 Design Surface에 오버레이를 표시 합니다.

- Keylines (주황색 선)

- 간격 (녹색 영역)

- 표 (파란색 선)

이러한 요소는 이전 스크린샷에서 볼 수 있습니다. 이러한 각 오버레이 항목을 구성할 수 있습니다. 재질 디자인 그리드 메뉴 옆의 줄임표 (...)를 클릭 하면 표를 비활성화/활성화 하 고, keylines 배치를 구성 하 고, 간격를 설정할 수 있는 대화 상자 팝 오버 열립니다. 모든 값은 `dp` (밀도 독립적 픽셀)로 표시 됩니다.

[![Grid, keyline 및 간격 구성](material-design-features-images/vs/03-grid-configuration-w158-sml.png)](material-design-features-images/vs/03-grid-configuration-w158.png#lightbox)

새 keyline을 추가 하려면 **오프셋** 상자에 새 오프셋 값을 입력 하 고 위치 (**왼쪽**, **위쪽**, **오른쪽**또는 **아래쪽**)를 선택한 다음 + 아이콘을 클릭 하 여 새 keyline를 추가 합니다. 마찬가지로, 새 간격을 추가 **하려면 크기와** **오프셋 상자에 각각 크기와 오프셋** (dp의 경우)을 입력 합니다. 위치 (**왼쪽**, **위쪽**, **오른쪽**또는 **아래쪽**)를 선택 하 고 + 아이콘을 클릭 하 여 새 간격을 추가 합니다.

이러한 구성 값을 변경 하면 레이아웃 XML 파일에 저장 되 고 레이아웃을 다시 열 때 다시 사용 됩니다.

## <a name="theme-editor"></a>테마 편집기

**테마 편집기** 를 사용 하 여 테마 특성의 하위 집합에 대 한 색 정보를 사용자 지정할 수 있습니다. **테마 편집기**를 열려면 도구 모음에서 페인트 브러시 아이콘을 클릭 합니다.

[![테마 편집기 아이콘](material-design-features-images/vs/04-theme-editor-icon-w158-sml.png)](material-design-features-images/vs/04-theme-editor-icon-w158.png#lightbox)

모든 대상 Android 버전 및 API 수준에 대 한 도구 모음에서 **테마 편집기** 에 액세스할 수 있지만, 대상 API 수준이 api 21 (Android 5.0 롤리팝) 이전인 경우 아래 설명 된 기능의 하위 집합만 사용할 수 있습니다.

**테마 편집기** 의 왼쪽 패널에는 현재 선택 된 테마를 구성 하는 색 목록이 표시 됩니다 (이 예제에서는 `Default Theme`사용).

[![테마 편집기](material-design-features-images/vs/05-theme-editor-w158-sml.png)](material-design-features-images/vs/05-theme-editor-w158.png#lightbox)

왼쪽에서 색을 선택 하는 경우 오른쪽 패널은 해당 색을 편집 하는 데 도움이 되는 다음 탭을 제공 합니다.

- **상속** &ndash; 선택한 색에 대 한 스타일 상속 다이어그램을 표시 하 고 해당 테마 색에 할당 된 확인 된 색 및 색 코드를 나열 합니다.

- **색 선택** &ndash; 선택 된 색을 임의의 값으로 변경할 수 있습니다.

- **재질 팔레트** &ndash; 선택 된 색을 재질 디자인을 준수 하는 값으로 변경할 수 있습니다.

- **리소스** &ndash;를 사용 하 여 선택한 색을 테마의 다른 기존 색 리소스 중 하나로 변경할 수 있습니다.

이러한 탭의 각 항목에 대해 자세히 살펴보겠습니다.

### <a name="inherit-tab"></a>상속 탭

다음 예제에 표시 된 것 처럼 **상속** 탭에는 **기본 테마**의 **배경색** 에 대 한 스타일 상속이 나열 됩니다.

[![상속 탭](material-design-features-images/vs/06-inherit-tab-w158-sml.png)](material-design-features-images/vs/06-inherit-tab-w158.png#lightbox)

이 예제에서는 **기본 테마가** `@color/background_material_light`를 사용 하는 스타일에서 상속 하지만 색 코드 값이 `#fffafafa`인 `color/material_grey_50`를 재정의 합니다.
스타일 상속에 대 한 자세한 내용은 [스타일 및 테마](https://developer.android.com/guide/topics/ui/themes.html#Inheritance)를 참조 하세요.

### <a name="color-picker"></a>색 선택

다음 스크린샷에는 **색 선택**이 나와 있습니다.

[![색 선택](material-design-features-images/vs/07-color-picker-w158-sml.png)](material-design-features-images/vs/07-color-picker-w158.png#lightbox)

이 예제에서는 다양 한 방법을 통해 **배경색** 을 원하는 값으로 변경할 수 있습니다.

- 색을 직접 클릭 합니다.
- 색상, 채도 및 밝기 값을 입력 합니다.
- Decimal의 RGB (빨강, 녹색, 파랑) 값을 입력 합니다.
- 선택한 색의 알파 (불투명도)를 설정 합니다.
- 16 진수 색 코드를 직접 입력 합니다.

색 선택에서 선택한 색은 재질 디자인 지침이 나 사용 가능한 색 리소스 집합으로 제한 *되지 않습니다* .

### <a name="resources"></a>자료

**리소스** 탭에서는 테마에 이미 있는 색 리소스의 목록을 제공 합니다.

[![리소스](material-design-features-images/vs/08-resources-w158-sml.png)](material-design-features-images/vs/08-resources-w158.png#lightbox)

**리소스** 탭을 사용 하 여 선택 항목을이 색 목록으로 제한 합니다. 테마의 다른 부분에 이미 할당 된 색 리소스를 선택 하는 경우 두 개의 인접 한 UI 요소가 동일한 색을 사용 하 여 "함께 실행" 될 수 있으며 사용자가 구분 하기 어려워집니다.

### <a name="material-palette"></a>재질 팔레트

**재질 팔레트** 탭은 **재질 디자인**색상표를 엽니다. 이 색상표에서 색 값을 선택 하면 재질 디자인 지침과 일치 하도록 색 선택이 제한 됩니다.

[![재질 팔레트](material-design-features-images/vs/09-material-palette-w158-sml.png)](material-design-features-images/vs/09-material-palette-w158.png#lightbox)

색상표의 위쪽에는 주 재질 디자인 색이 표시 되 고, 색상표 아래쪽에는 선택한 기본 색의 다양 한 색이 표시 됩니다. 예를 들어, **indigo**를 선택 하면 대화 상자 아래쪽에 **indigo** 색상 컬렉션이 표시 됩니다.
색상을 선택 하면 속성의 색이 선택한 색조로 변경 됩니다. 다음 예제에서 단추의 `Background Tint`는 *Indigo 500*로 변경 됩니다.

![Indigo 500 선택](material-design-features-images/vs/10-indigo-w158.png)

`Background Tint`는 *Indigo 500* (`#ff3f51b5`)에 대 한 색 코드로 설정 되 고 디자이너는이 변경 내용을 반영 하도록 배경색을 업데이트 합니다.

[![배경 색조가 변경 됨](material-design-features-images/vs/11-background-tint-w158-sml.png)](material-design-features-images/vs/11-background-tint-w158.png#lightbox)

재질 디자인 색상표에 대 한 자세한 내용은 재질 디자인 색상표 [가이드](https://material.io/design/color/)를 참조 하세요.

### <a name="creating-a-new-theme"></a>새 테마 만들기

다음 예제에서는 재질 색상표를 사용 하 여 새 사용자 지정 테마를 만듭니다. 먼저 **배경색** 을 *파란색 900*로 변경 합니다.

![배경색을 Blue 900로 변경 합니다.](material-design-features-images/vs/12-change-background-to-blue-w158.png)

색 리소스가 변경 되 면 메시지가 표시 되 고 *현재 테마에 저장 되지 않은 변경 내용이 있습니다*.

[저장 하지 않은 변경 내용 경고![](material-design-features-images/vs/13-unsaved-changes-w158-sml.png)](material-design-features-images/vs/13-unsaved-changes-w158.png#lightbox)

디자이너의 **배경색이** 새 색 선택으로 변경 되었지만이 변경 내용은 아직 저장 되지 않았습니다. 이 시점에서 다음 중 하나를 수행할 수 있습니다.

- **변경 내용 취소** 를 클릭 하 여 새 색 선택 (또는 선택 항목)을 취소 하 고 테마를 원래 상태로 되돌립니다.

- <kbd>Ctrl + S</kbd> 를 눌러 현재 테마에 대 한 변경 내용을 저장 합니다.

다음 예제에서는 변경 내용이 **Apptheme**에 저장 되도록 <kbd>CTRL + S</kbd> 를 눌렀습니다.

[AppTheme에 저장 된![변경 내용](material-design-features-images/vs/14-custom-theme-w158-sml.png)](material-design-features-images/vs/14-custom-theme-w158.png#lightbox)

## <a name="summary"></a>요약

이 항목에서는 Android Designer에서 사용할 수 있는 재질 디자인 기능에 대해 설명 했습니다. 재질 디자인 그리드를 사용 하도록 설정 하 고 구성 하는 방법에 대해 설명 하 고, 테마 편집기를 사용 하 여 재질 디자인 지침을 따르는 새 사용자 지정 테마를 만드는 방법을 설명 했습니다.
재질 디자인을 위한 Xamarin Android 지원에 대 한 자세한 내용은 [재질 테마](~/android/user-interface/material-theme.md)를 참조 하세요.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

이 가이드에서는 다음과 같은 디자이너 기능을 살펴보겠습니다.

- *재질 디자인 모눈* 은 재질 디자인 지침에 따라 레이아웃 위젯을 배치 하는 데 도움이 되는 표, 간격 및 keylines를 표시 하는 Design Surface의 오버레이를 &ndash; 합니다.

- *재질 디자인* 색상표는 공식 재질 디자인 팔레트에서 색을 선택 하는 데 도움이 되는 속성 패드 대화 상자를 &ndash; 합니다.

- 텍스트 필드의 `textAppearance` 속성에 대해 선택 하는 재질 디자인 호환 설정을 제공 하는 속성 패드 대화 상자 *를 &ndash; 수* 있습니다.

- *테마 편집기* &ndash; 테마의 하위 집합에 대 한 색 정보를 설정할 수 있는 작은 색 리소스 편집기입니다. 예를 들어 `colorPrimary`, `colorPrimaryDark`, `colorAccent`등의 재질 색을 미리 보고 수정할 수 있습니다.

이러한 각 기능을 살펴보고 사용 방법에 대 한 예제를 제공 합니다.

## <a name="material-design-grid"></a>재질 디자인 그리드

재질 디자인 그리드 메뉴는 디자이너 맨 위에 있는 도구 모음에서 사용할 수 있습니다.

[![재질 디자인 그리드](material-design-features-images/xs/01-material-design-grid-sml.png)](material-design-features-images/xs/01-material-design-grid.png#lightbox)

재질 디자인 그리드 아이콘을 클릭 하면 디자이너는 다음 요소를 포함 하는 Design Surface에 오버레이를 표시 합니다.

- Keylines (주황색 선)

- 간격 (녹색 영역)

- 표 (파란색 선)

이러한 요소는 다음 스크린샷에서 볼 수 있습니다.

[![Keyline, 간격 및 그리드](material-design-features-images/xs/02-grid-and-keylines-sml.png)](material-design-features-images/xs/02-grid-and-keylines.png#lightbox)

이러한 각 오버레이 항목을 구성할 수 있습니다. 재질 디자인 그리드 메뉴 옆의 줄임표 (&hellip;)를 클릭 하면 표를 비활성화/활성화 하 고, keylines 배치를 구성 하 고, 간격를 설정할 수 있는 대화 상자 팝 오버 열립니다. 모든 값은 `dp` (밀도 독립적 픽셀)로 표시 됩니다.

[![Grid, keyline 및 간격 구성](material-design-features-images/xs/03-grid-configuration-sml.png)](material-design-features-images/xs/03-grid-configuration.png#lightbox)

새 keyline을 추가 하려면 **오프셋** 상자에 새 오프셋 값을 입력 하 고, 위치 (**왼쪽**, **위쪽**, **오른쪽**또는 **아래쪽**)를 선택 하 고, + 아이콘 (값이 입력 될 때 오른쪽에 표시 됨)을 클릭 하 여 새 keyline를 추가 합니다. 마찬가지로, 새 간격을 추가 **하려면 크기와** **오프셋 상자에 각각 크기와 오프셋** (dp의 경우)을 입력 합니다. 위치 (**왼쪽**, **위쪽**, **오른쪽**또는 **아래쪽**)를 선택 하 고 + 아이콘을 클릭 하 여 새 간격을 추가 합니다.

이러한 구성 값을 변경 하면 레이아웃 XML 파일에 저장 되 고 레이아웃을 다시 열 때 다시 사용 됩니다.

## <a name="material-design-color-palette"></a>재질 디자인 색상표

이제 색을 허용 하는 모든 속성 패널 항목에는 다음 스크린샷에 표시 된 것 처럼 재질 디자인 색상표를 여는 데 사용할 수 있는 추가 색상표 아이콘이 있습니다.

[![색 아이콘](material-design-features-images/xs/04-new-color-icon-sml.png)](material-design-features-images/xs/04-new-color-icon.png#lightbox)

이 아이콘을 클릭 하면 재질 디자인 색상표에서 해당 속성의 색을 구성할 수 있도록 하는 대화 팝 오버 열립니다.

[![재질 디자인 색상표](material-design-features-images/xs/05-material-palette-sml.png)](material-design-features-images/xs/05-material-palette.png#lightbox)

색상표의 위쪽에는 주 재질 디자인 색이 표시 되 고, 색상표 아래쪽에는 선택한 기본 색의 다양 한 색이 표시 됩니다. 예를 들어, **indigo**를 선택 하면 대화 상자 아래쪽에 **indigo** 색상 컬렉션이 표시 됩니다.
색상을 선택 하면 속성의 색이 선택한 색조로 변경 됩니다. 다음 예제에서 단추의 `Background Tint`는 *Indigo 500*로 변경 됩니다.

[![Indigo 500을 선택 합니다.](material-design-features-images/xs/06-indigo-sml.png)](material-design-features-images/xs/06-indigo.png#lightbox)

`Background Tint`는 *Indigo 500* (`#ff3f51b5`)에 대 한 색 코드로 설정 되 고 디자이너는이 변경 내용을 반영 하도록 단추의 배경색을 업데이트 합니다.

[배경 색조 변경 내용![](material-design-features-images/xs/07-background-tint-sml.png)](material-design-features-images/xs/07-background-tint.png#lightbox)

재질 디자인 색상표에 대 한 자세한 내용은 재질 디자인 색상표 [가이드](https://material.io/design/color/)를 참조 하세요.

## <a name="typographic-scale"></a>인쇄 크기 조정

**속성** 패드 **스타일** 탭의 **텍스트 모양** 섹션에는 재질 디자인 사양에 맞는 `TextAppearance` 스타일에서 선택할 수 있는 아이콘이 있습니다.

[![스타일 탭](material-design-features-images/xs/08-typo-scale-icon-sml.png)](material-design-features-images/xs/08-typo-scale-icon.png#lightbox)

이 아이콘을 클릭 하면 다음과 같이 선택할 수 있는 미리 구성 된 텍스트 스타일의 목록이 표시 되는 **인쇄 간격** 대화 상자 팝 오버 열립니다.

[![텍스트 스타일 선택](material-design-features-images/xs/09-text-appearance-sml.png)](material-design-features-images/xs/09-text-appearance.png#lightbox)

다음 예제에서 **1 표시** 를 클릭 하면 단추의 텍스트가 **표시 1**의 더 큰 글꼴로 변경 됩니다.

[![1 스타일 표시](material-design-features-images/xs/10-display-1-sml.png)](material-design-features-images/xs/10-display-1.png#lightbox)

[ **인쇄 크기 조정** ] 대화 상자의 텍스트 스타일은 **테마** 설정을 따릅니다. 예를 들어, 디자이너에서 **밝은** 테마를 선택 하면 사용 가능한 텍스트 스타일 목록이 **밝은** 테마를 미러링합니다.

[![밝은 테마](material-design-features-images/xs/11-light-theme-sml.png)](material-design-features-images/xs/11-light-theme.png#lightbox)

## <a name="theme-editor"></a>테마 편집기

**테마 편집기** 를 사용 하 여 테마 특성의 하위 집합에 대 한 색 정보를 사용자 지정할 수 있습니다. **테마 편집기**를 열려면 도구 모음에서 페인트 브러시 아이콘을 클릭 합니다.

[![테마 편집기 아이콘](material-design-features-images/xs/12a-theme-editor-icon-sml.png)](material-design-features-images/xs/12a-theme-editor-icon.png#lightbox)

모든 대상 Android 버전 및 API 수준에 대 한 도구 모음에서 **테마 편집기** 에 액세스할 수 있지만, 대상 API 수준이 api 21 (Android 5.0 롤리팝) 이전인 경우 아래 설명 된 기능의 하위 집합만 사용할 수 있습니다.

**테마 편집기** 의 왼쪽 패널에는 현재 선택 된 테마를 구성 하는 색 목록이 표시 됩니다 (이 예제에서는 `Default Theme`사용).

[![테마 편집기](material-design-features-images/xs/12b-theme-editor-sml.png)](material-design-features-images/xs/12b-theme-editor.png#lightbox)

왼쪽에서 색을 선택 하는 경우 오른쪽 패널은 해당 색을 편집 하는 데 도움이 되는 다음 탭을 제공 합니다.

- **상속** &ndash; 선택한 색에 대 한 스타일 상속 다이어그램을 표시 하 고 해당 테마 색에 할당 된 확인 된 색 및 색 코드를 나열 합니다.

- **색 선택** &ndash; 선택 된 색을 임의의 값으로 변경할 수 있습니다.

- **재질 팔레트** &ndash; 선택 된 색을 재질 디자인을 준수 하는 값으로 변경할 수 있습니다.

- **리소스** &ndash;를 사용 하 여 선택한 색을 테마의 다른 기존 색 리소스 중 하나로 변경할 수 있습니다.

이러한 탭의 각 항목에 대해 자세히 살펴보겠습니다.

### <a name="inherit-tab"></a>상속 탭

다음 예제에 표시 된 것 처럼 **상속** 탭에는 **기본 테마**의 **배경색** 에 대 한 스타일 상속이 나열 됩니다.

[![상속 탭](material-design-features-images/xs/13-inherit-sml.png)](material-design-features-images/xs/13-inherit.png#lightbox)

이 예제에서는 **기본 테마가** `@color/background_material_dark`를 사용 하는 스타일에서 상속 하지만 색 코드 값이 `#ff303030`인 `color/material_grey_850`를 재정의 합니다.
스타일 상속에 대 한 자세한 내용은 [스타일 및 테마](https://developer.android.com/guide/topics/ui/themes.html#Inheritance)를 참조 하세요.

### <a name="color-picker"></a>색 선택

다음 스크린샷에는 **색 선택**이 나와 있습니다.

[![색 선택](material-design-features-images/xs/14-color-picker-sml.png)](material-design-features-images/xs/14-color-picker.png#lightbox)

이 예제에서는 다양 한 방법을 통해 **배경색** 을 원하는 값으로 변경할 수 있습니다.

- 색을 직접 클릭 합니다.
- 색상, 채도 및 밝기 값을 입력 합니다.
- Decimal의 RGB (빨강, 녹색, 파랑) 값을 입력 합니다.
- 선택한 색의 알파 (불투명도)를 설정 합니다.
- 16 진수 색 코드를 직접 입력 합니다.

색 선택에서 선택한 색은 재질 디자인 지침이 나 사용 가능한 색 리소스 집합으로 제한 *되지 않습니다* .

### <a name="resources"></a>자료

**리소스** 탭에서는 테마에 이미 있는 색 리소스의 목록을 제공 합니다.

[![리소스](material-design-features-images/xs/15-resources-sml.png)](material-design-features-images/xs/15-resources.png#lightbox)

**리소스** 탭을 사용 하 여 선택 항목을이 색 목록으로 제한 합니다. 테마의 다른 부분에 이미 할당 된 색 리소스를 선택 하는 경우 두 개의 인접 한 UI 요소가 동일한 색을 사용 하 여 "함께 실행" 될 수 있으며 사용자가 구분 하기 어려워집니다.

### <a name="material-palette"></a>재질 팔레트

**재질 팔레트** 탭은 [앞](#material-design-color-palette)에서 설명한 **재질 디자인** 색상표를 엽니다. 이 색상표에서 색 값을 선택 하면 재질 디자인 지침과 일치 하도록 색 선택이 제한 됩니다.

[![재질 팔레트](material-design-features-images/xs/16-material-palette-sml.png)](material-design-features-images/xs/16-material-palette.png#lightbox)

### <a name="creating-a-new-theme"></a>새 테마 만들기

다음 예제에서는 재질 색상표를 사용 하 여 새 사용자 지정 테마를 만듭니다. 먼저 **배경색** 을 *파란색 900*로 변경 합니다.

[배경![파란색 900으로 변경](material-design-features-images/xs/17-change-background-to-blue-sml.png)](material-design-features-images/xs/17-change-background-to-blue.png#lightbox)

색 리소스가 변경 되 면 메시지가 표시 되 고 *현재 테마에 저장 되지 않은 변경 내용이 있습니다*.

[저장 하지 않은 변경 내용 경고![](material-design-features-images/xs/18-unsaved-changes-sml.png)](material-design-features-images/xs/18-unsaved-changes.png#lightbox)

디자이너에서 색이 변경 되었지만 변경 내용이 아직 저장 되지 않았습니다. 이 시점에서 다음 중 하나를 수행할 수 있습니다.

- **변경 내용 취소** 를 클릭 하 여 새 색 선택 (또는 선택 항목)을 취소 하 고 테마를 원래 상태로 되돌립니다.

- **&#8984; + S** 를 눌러 **사용자 지정**이라는 새 테마에 변경 내용을 저장 합니다.

## <a name="summary"></a>요약

이 항목에서는 Android Designer에서 사용할 수 있는 재질 디자인 기능에 대해 설명 했습니다. 재질 디자인 그리드를 사용 하도록 설정 하 고 구성 하는 방법, 재질 디자인 색상표를 사용 하 여 색 속성을 편집 하는 방법 및 인쇄 크기 조정 선택기를 사용 하 여 텍스트 속성을 구성 하는 방법을 설명 했습니다. 또한 테마 편집기를 사용 하 여 재질 디자인 지침을 따르는 새 사용자 지정 테마를 만드는 방법도 보여 줍니다. 재질 디자인을 위한 Xamarin Android 지원에 대 한 자세한 내용은 [재질 테마](~/android/user-interface/material-theme.md)를 참조 하세요.

-----

## <a name="related-links"></a>관련 링크

- [재질 테마](~/android/user-interface/material-theme.md)
- [재질 디자인 소개](https://material.io/design/introduction)
