---
title: 리소스 한정자 및 시각화 옵션
description: 이 항목에서는 일부 한정자 값이 일치 하는 경우에만 사용 되는 리소스를 정의 하는 방법을 설명 합니다. 간단한 예제는 언어에 한정 된 문자열 리소스입니다. 문자열 리소스는 추가 언어에 사용 하도록 정의 된 다른 대체 리소스를 사용 하 여 기본값으로 정의할 수 있습니다. 모든 리소스 종류는 레이아웃 자체를 포함 하 여 정규화 될 수 있습니다.
ms.prod: xamarin
ms.assetid: 2111C18A-3EDA-3787-25E1-3869FF4BE441
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: d9f624084c83b318487f1162a9a2350f9e2cc409
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510496"
---
# <a name="resource-qualifiers-and-visualization-options"></a>리소스 한정자 및 시각화 옵션

_이 항목에서는 일부 한정자 값이 일치 하는 경우에만 사용 되는 리소스를 정의 하는 방법을 설명 합니다. 간단한 예제는 언어에 한정 된 문자열 리소스입니다. 문자열 리소스는 추가 언어에 사용 하도록 정의 된 다른 대체 리소스를 사용 하 여 기본값으로 정의할 수 있습니다. 모든 리소스 종류는 레이아웃 자체를 포함 하 여 정규화 될 수 있습니다._


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="resource-qualifier-options"></a>리소스 한정자 옵션

**가로** 모드 단추 오른쪽에 있는 줄임표 아이콘을 클릭 하 여 **리소스 한정자 옵션** 에 액세스할 수 있습니다.

[![리소스 한정자 옵션](resource-qualifiers-images/vs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/vs/08-resource-qual-opt.png#lightbox)

이 대화 상자에는 다음과 같은 리소스 한정자의 풀 다운 메뉴가 표시 됩니다.

-   **언어** &ndash; 사용 가능한 언어 리소스를 표시 하 고 새 언어/지역 리소스를 추가 하는 옵션을 제공 합니다.

-   **UI 모드** 표시 모드 (예: **자동차 도크** 및 **책상 도크**) 뿐만 아니라 레이아웃 방향을 나열 합니다. &ndash;

이러한 각 풀 다운 메뉴에는 다음에 설명 된 대로 리소스 한정자를 선택 하 고 구성할 수 있는 새 대화 상자가 열립니다.

### <a name="language"></a>언어

**언어** 풀 다운 메뉴에는 정의 된 리소스가 있는 언어 (또는 기본값 인 **모든 언어**)만 나열 됩니다. 그러나 목록에 새 언어를 추가 하는 데 사용할 수 있는 **언어/지역 추가** ... 옵션도 있습니다.

[![언어/지역 추가](resource-qualifiers-images/vs/09-add-language-region-sml.png)](resource-qualifiers-images/vs/09-add-language-region.png#lightbox)

**언어/지역 추가 ...** 를 클릭 하면 사용 가능한 언어 및 지역의 드롭다운 목록을 표시 하는 **언어 선택** 대화 상자가 열립니다.

![언어 목록](resource-qualifiers-images/vs/10-languages.png "언어 목록")

이 예제에서는 언어에 대해 **fr (프랑스어)** 를 선택 **하 고 프랑스어** 지역 언어의 경우 (벨기에)로 설정 했습니다. 특정 지역에 관계 없이 많은 언어를 지정할 수 있으므로 **지역** 필드는 선택 사항입니다. **언어** 풀 다운 메뉴가 다시 열리면 새로 추가 된 언어/지역 리소스가 표시 됩니다.

![선택한 언어 및 지역](resource-qualifiers-images/vs/11-language-region-added.png "선택한 언어 및 지역")

새 언어를 추가 하지만 새 언어에 대 한 리소스를 만들지 않는 경우 추가 된 언어는 다음에 프로젝트를 열 때 더 이상 표시 되지 않습니다.

### <a name="ui-mode"></a>UI 모드

**UI 모드** 풀 다운 메뉴를 클릭 하면 모드 목록 (예: **정상**, **자동차 도크**, **책상 도크**, **텔레비전**, **어플라이언스**및 **조사식**)이 표시 됩니다.


[![UI 모드 메뉴](resource-qualifiers-images/vs/12-ui-mode-sml.png)](resource-qualifiers-images/vs/12-ui-mode.png#lightbox)

이 목록 아래에는 **야간 모드와** **야간**모드, **왼쪽** 에서 오른쪽 및 오른쪽에서 왼쪽으로의 레이아웃 방향이 차례로 표시 **됩니다 (** 왼쪽에서 **오른쪽** 및 **오른쪽에서 왼쪽** 으로의 옵션에 대 한 자세한 내용은 참조 [). LayoutDirection](xref:Android.Util.LayoutDirection)).
**리소스 한정자 옵션** 대화 상자의 마지막 항목은 라운드 **화면** (Android 마모와 함께 사용) 또는 **라운드 스크린이 아닌 화면**입니다.
라운드 및 비연속 화면에 대 한 자세한 내용은 [레이아웃](https://developer.android.com/training/wearables/ui/layouts.html)을 참조 하세요.
Android UI 모드에 대 한 자세한 내용은 [Uimodemanager](xref:Android.App.UiModeManager)를 참조 하세요.

## <a name="action-bar-settings"></a>작업 모음 설정

**작업 모음 설정** 아이콘은 페인트 브러시 (테마 편집기) 아이콘 왼쪽에 있습니다.

![작업 모음 설정](resource-qualifiers-images/vs/14-action-bar.png "작업 모음 설정")

이 아이콘은 세 가지 작업 모음 모드 중 하나를 선택 하는 방법을 제공 하는 대화 상자 팝 오버 엽니다.

-   **표준** &ndash; 는 선택 부제목이 있는 로고 또는 아이콘 및 제목 텍스트로 구성 됩니다.

-   **목록** &ndash; 목록 탐색 모드입니다. 이 모드는 정적 제목 텍스트 대신 활동 내 탐색을 위한 목록 메뉴를 표시 합니다 (즉, 사용자에 게 드롭다운 목록으로 표시 될 수 있음).

-   **탭** &ndash; 탭 탐색 모드입니다. 이 모드는 정적 제목 텍스트 대신 활동 내 탐색을 위한 일련의 탭을 제공 합니다.

## <a name="themes"></a>테마

**테마** 드롭다운 메뉴는 프로젝트에 정의 된 모든 테마를 표시 합니다. **추가 테마** 를 선택 하면 아래와 같이 설치 된 Android SDK에서 사용할 수 있는 모든 테마 목록이 포함 된 대화 상자가 열립니다.

[![추가 테마 목록](resource-qualifiers-images/vs/15-theme-menu-sml.png "추가 테마 목록")](resource-qualifiers-images/vs/15-theme-menu.png#lightbox)

테마를 선택 하면 새 테마의 효과를 표시 하도록 Design Surface 업데이트 됩니다. 이 변경은 **테마** 대화 상자에서 **확인** 단추를 클릭 한 경우에만 영구적으로 적용 됩니다. 테마를 선택 하면 아래와 같이 **테마** 드롭다운 메뉴에 포함 됩니다.

![이제 밝은 테마를 사용할 수 있습니다] . (resource-qualifiers-images/vs/16-light-theme.png "이제 밝은 테마를 사용할 수 있습니다") .

## <a name="android-version"></a>Android 버전

Android **버전** 선택기는 디자이너에서 레이아웃을 렌더링 하는 데 사용 되는 android 버전을 설정 합니다. 선택기는 프로젝트의 대상 프레임 워크 버전과 호환 되는 모든 버전을 표시 합니다.

![Android 버전 목록](resource-qualifiers-images/vs/17-android-version.png "Android 버전 목록")

대상 프레임 워크 버전은 프로젝트의 설정 **> 응용 프로그램 > Android 버전을 사용 하 여 컴파일을 사용 하**여 설정할 수 있습니다. 대상 프레임 워크 버전에 대 한 자세한 내용은 [ANDROID API 수준 이해](~/android/app-fundamentals/android-api-levels.md)를 참조 하세요.

도구 상자에서 사용할 수 있는 위젯 집합은 프로젝트의 대상 프레임 워크 버전에 따라 결정 됩니다. **속성 창**에서 사용할 수 있는 속성에도 마찬가지입니다. 사용 가능한 위젯 목록은 도구 모음의 **버전** 선택기에서 선택한 값에 따라 결정 *되지 않습니다* . 예를 들어 프로젝트의 대상 버전을 Android 4.4로 설정 하는 경우 도구 모음 버전 선택기에서 Android 6.0를 선택 하 여 Android 6.0에서 프로젝트가 표시 되는 모양을 확인할 수 있지만 Android 6.0 &ndash; 에 특정 된 위젯을 추가할 수는 없습니다.  여전히 Android 4.4에서 사용할 수 있는 위젯로 제한 됩니다.

리소스 유형에 대 한 자세한 내용은 [Android 리소스](~/android/app-fundamentals/resources-in-android/index.md)를 참조 하세요.



# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="resource-qualifier-options"></a>리소스 한정자 옵션

**가로** 모드 단추 오른쪽에 있는 줄임표 아이콘을 클릭 하 여 **리소스 한정자 옵션** 에 액세스할 수 있습니다.

[![리소스 한정자 옵션](resource-qualifiers-images/xs/08-resource-qual-opt-m75-sml.png)](resource-qualifiers-images/xs/08-resource-qual-opt-m75.png#lightbox)

이 대화 상자에는 다음과 같은 리소스 한정자의 풀 다운 메뉴가 표시 됩니다.

-   **언어** &ndash; 사용 가능한 언어 리소스를 표시 하 고 새 언어/지역 리소스를 추가 하는 옵션을 제공 합니다.

-   **UI 모드** 표시 모드 (예: **자동차 도크** 및 **책상 도크**) 뿐만 아니라 레이아웃 방향을 나열 합니다. &ndash;

이러한 각 풀 다운 메뉴에는 다음에 설명 된 대로 리소스 한정자를 선택 하 고 구성할 수 있는 새 대화 상자가 열립니다.

### <a name="language"></a>언어

**언어** 풀 다운 메뉴에는 정의 된 리소스가 있는 언어 (또는 기본값 인 **모든 언어**)만 나열 됩니다. 그러나 목록에 새 언어를 추가 하는 데 사용할 수 있는 **언어/지역 추가** ... 옵션도 있습니다.

[![언어/지역 추가](resource-qualifiers-images/xs/09-add-language-region-m75-sml.png)](resource-qualifiers-images/xs/09-add-language-region-m75.png#lightbox)

**언어/지역 추가 ...** 를 클릭 하면 사용 가능한 언어 및 지역의 드롭다운 목록을 표시 하는 **언어 선택** 대화 상자가 열립니다.

[![언어 목록](resource-qualifiers-images/xs/10-languages-m75-sml.png)](resource-qualifiers-images/xs/10-languages-m75.png#lightbox)

이 예제에서는 언어에 대해 **fr (프랑스어)** 를 선택 **하 고 프랑스어** 지역 언어의 경우 (벨기에)로 설정 했습니다. 특정 지역에 관계 없이 많은 언어를 지정할 수 있으므로 **지역** 필드는 선택 사항입니다. **언어** 풀 다운 메뉴가 다시 열리면 새로 추가 된 언어/지역 리소스가 표시 됩니다.

[![선택한 언어 및 지역](resource-qualifiers-images/xs/11-language-region-added-m75-sml.png)](resource-qualifiers-images/xs/11-language-region-added-m75.png#lightbox)

새 언어를 추가 하지만 새 언어에 대 한 리소스를 만들지 않는 경우 추가 된 언어는 다음에 프로젝트를 열 때 더 이상 표시 되지 않습니다.

### <a name="ui-mode"></a>UI 모드

**UI 모드** 풀 다운 메뉴를 클릭 하면 모드 목록 (예: **정상**, **자동차 도크**, **책상 도크**, **텔레비전**, **어플라이언스**및 **조사식**)이 표시 됩니다.

[![UI 모드 메뉴](resource-qualifiers-images/xs/12-ui-mode-m75-sml.png)](resource-qualifiers-images/xs/12-ui-mode-m75.png#lightbox)

이 목록 아래에는 **야간 모드와** 야간 모드 **,** 왼쪽에서 **오른쪽** 및 **오른쪽으로**의 레이아웃 방향이 차례로 표시 됩니다. 마지막 옵션 쌍을 사용 하 여 **원형 화면** 또는 **사각형 화면** (Android 착용 장치에 유용)을 선택할 수 있습니다.

Android UI 모드에 대 한 자세한 내용은 [Uimodemanager](xref:Android.App.UiModeManager)를 참조 하세요.
**왼쪽에서 오른쪽** 및 **오른쪽에서 왼쪽** 으로의 옵션에 대 한 자세한 내용은 [layoutdirection](xref:Android.Util.LayoutDirection)을 참조 하세요.


## <a name="action-bar-settings"></a>작업 모음 설정

**작업 모음 설정** 아이콘은 페인트 브러시 (테마 편집기) 아이콘 왼쪽에 있습니다.

[![작업 모음 설정](resource-qualifiers-images/xs/13-action-bar-m75-sml.png)](resource-qualifiers-images/xs/13-action-bar-m75.png#lightbox)

이 아이콘은 세 가지 작업 모음 모드 중 하나를 선택 하는 방법을 제공 하는 대화 상자 팝 오버 엽니다.

-   **표준** &ndash; 는 선택 부제목이 있는 로고 또는 아이콘 및 제목 텍스트로 구성 됩니다.

-   **목록** &ndash; 목록 탐색 모드입니다. 이 모드는 정적 제목 텍스트 대신 활동 내 탐색을 위한 목록 메뉴를 표시 합니다 (즉, 사용자에 게 드롭다운 목록으로 표시 될 수 있음).

-   **탭** &ndash; 탭 탐색 모드입니다. 이 모드는 정적 제목 텍스트 대신 활동 내 탐색을 위한 일련의 탭을 제공 합니다.

## <a name="themes"></a>테마

**테마** 드롭다운 메뉴는 프로젝트에 정의 된 모든 테마를 표시 합니다. **추가 테마** 를 선택 하면 아래와 같이 설치 된 Android SDK에서 사용할 수 있는 모든 테마 목록이 포함 된 대화 상자가 열립니다.

[![추가 테마 목록](resource-qualifiers-images/xs/14-theme-menu-m75-sml.png)](resource-qualifiers-images/xs/14-theme-menu-m75.png#lightbox)

테마를 선택 하면 새 테마의 효과를 표시 하도록 Design Surface 업데이트 됩니다. 이 변경은 **테마** 대화 상자에서 **확인** 단추를 클릭 한 경우에만 영구적으로 적용 됩니다. 테마를 선택 하면 아래와 같이 **테마** 드롭다운 메뉴에 포함 됩니다.

[![이제 밝은 테마를 사용할 수 있습니다.](resource-qualifiers-images/xs/15-light-theme-m75-sml.png)](resource-qualifiers-images/xs/15-light-theme-m75.png#lightbox)

## <a name="android-version"></a>Android 버전

Android **버전** 선택기는 디자이너에서 레이아웃을 렌더링 하는 데 사용 되는 android 버전을 설정 합니다. 선택기는 프로젝트의 대상 프레임 워크 버전과 호환 되는 모든 버전을 표시 합니다.

[![Android 버전 목록](resource-qualifiers-images/xs/16-android-version-m75-sml.png)](resource-qualifiers-images/xs/16-android-version-m75.png#lightbox)

대상 프레임 워크 버전은 프로젝트의 설정 **> 빌드 > 일반** 섹션에서 설정할 수 있습니다. 대상 프레임 워크 버전에 대 한 자세한 내용은 [ANDROID API 수준 이해](~/android/app-fundamentals/android-api-levels.md)를 참조 하세요.

도구 상자에서 사용할 수 있는 위젯 집합은 프로젝트의 대상 프레임 워크 버전에 따라 결정 됩니다. **속성 패드**에서 사용할 수 있는 속성에도 마찬가지입니다. 사용 가능한 위젯 목록은 도구 모음의 **버전** 선택기에서 선택한 값에 따라 결정 *되지 않습니다* . 예를 들어 프로젝트의 대상 버전을 Android 4.4로 설정 하는 경우 도구 모음 버전 선택기에서 Android 6.0를 선택 하 여 Android 6.0에서 프로젝트가 표시 되는 모양을 확인할 수 있지만 Android 6.0 &ndash; 에 특정 된 위젯을 추가할 수는 없습니다.  여전히 Android 4.4에서 사용할 수 있는 위젯로 제한 됩니다.

리소스 유형에 대 한 자세한 내용은 [Android 리소스](~/android/app-fundamentals/resources-in-android/index.md)를 참조 하세요.

-----
