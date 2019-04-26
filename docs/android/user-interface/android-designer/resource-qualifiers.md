---
title: 리소스 한정자 및 시각화 옵션
description: 이 항목에서는 일부 한정자 값을 일치 하는 경우에 사용 하는 리소스를 정의 하는 방법에 설명 합니다. 간단한 예제에는 언어의 정규화 된 문자열 리소스입니다. 추가 언어에 사용 되는 정의 된 다른 대체 리소스를 사용 하 여 기본적으로 문자열 리소스를 정의할 수 있습니다. 모든 리소스 유형에 자체 레이아웃을 포함 하 여 정규화 할 수 있습니다.
ms.prod: xamarin
ms.assetid: 2111C18A-3EDA-3787-25E1-3869FF4BE441
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 9d99e6a59b57b59d585b32befdadc0890d41448c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60951059"
---
# <a name="resource-qualifiers-and-visualization-options"></a>리소스 한정자 및 시각화 옵션

_이 항목에서는 일부 한정자 값을 일치 하는 경우에 사용 하는 리소스를 정의 하는 방법에 설명 합니다. 간단한 예제에는 언어의 정규화 된 문자열 리소스입니다. 추가 언어에 사용 되는 정의 된 다른 대체 리소스를 사용 하 여 기본적으로 문자열 리소스를 정의할 수 있습니다. 모든 리소스 유형에 자체 레이아웃을 포함 하 여 정규화 할 수 있습니다._


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="resource-qualifier-options"></a>리소스 한정자 옵션

**리소스 한정자 옵션** 오른쪽의 줄임표 (...) 아이콘을 클릭 하 여 액세스할 수 합니다 **가로** 모드 단추:

[![리소스 한정자 옵션](resource-qualifiers-images/vs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/vs/08-resource-qual-opt.png#lightbox)

이 대화 상자는 다음 리소스 한정자에 대 한 풀 다운 메뉴를 제공합니다.

-   **언어** &ndash; 사용 가능한 언어 리소스를 표시 하 고 새 언어/지역 리소스를 추가 하는 옵션을 제공 합니다.

-   **UI 모드** &ndash; 목록 디스플레이 모드 (같은 **자동차 도크** 하 고 **데스크 도크**) 레이아웃 지침 뿐만 아니라 합니다.

이러한 풀 다운 메뉴의 각 선택 하 고 (설명 했 듯이 다음) 리소스 한정자를 구성할 수 있는 새 대화 상자가 열립니다.

### <a name="language"></a>언어

합니다 **언어** 정의 된 리소스가 있는 해당 언어를 나열 하는 풀 다운 메뉴 (또는 **모든 언어**, 기본값). 그러나 이기도 한 **언어/지역 추가...**  옵션 목록에 새 언어를 추가할 수 있습니다.

[![언어/지역 추가](resource-qualifiers-images/vs/09-add-language-region-sml.png)](resource-qualifiers-images/vs/09-add-language-region.png#lightbox)

클릭할 때 **언어/지역 추가...** 서 **언어 선택** 사용 가능한 언어 및 지역 드롭다운 목록을 표시할 대화 상자가 열립니다.

![언어 목록이](resource-qualifiers-images/vs/10-languages.png "언어 목록")

이 예제에서는 했습니다 **fr (프랑스어)** 언어용 및 **BE** (벨기에) 프랑스어의 국가별 언어에 대 한 합니다. 유의 합니다 **지역** 특정 지역에 관계 없이 다양 한 언어를 지정할 수 있으므로 필드는 선택 사항입니다. 경우는 **언어** 풀 다운 메뉴를 다시 열, 새로 추가 된 언어/지역 리소스를 표시 합니다.

![언어 및 지역 선택](resource-qualifiers-images/vs/11-language-region-added.png "언어 및 지역 선택")

새 언어를 추가할 경우 다음에 표시 될 추가 언어는 더 이상이 대 한 새 리소스 만들지 열면 프로젝트를 참고 합니다.

### <a name="ui-mode"></a>UI 모드

클릭할 때 합니다 **UI 모드** 같은의 모드 목록을 풀 다운 메뉴에서 표시 됩니다 **Normal**, **자동차 도크**, **데스크 도크**, **텔레비전**하십시오 **어플라이언스**, 및 **조사식**:


[![UI 모드 메뉴](resource-qualifiers-images/vs/12-ui-mode-sml.png)](resource-qualifiers-images/vs/12-ui-mode.png#lightbox)

이 목록 아래에 야간 모드 **하지 밤** 및 **밤**레이아웃 지침 뒤 **왼쪽에서 오른쪽** 및 **오른쪽에서 왼쪽으로** ( 에 대 한 정보 **왼쪽에서 오른쪽** 하 고 **오른쪽에서 왼쪽** 옵션을 참조 하십시오 [LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/)).
마지막 항목에 **리소스 한정자 옵션** 대화 상자는를 **화면을 반올림** (사용 하 여 사용 하 여 Android Wear) 용 또는 **하지 둥근 화면**합니다.
Round 및 비 왕복 화면에 대 한 정보를 참조 하세요 [레이아웃](https://developer.android.com/training/wearables/ui/layouts.html)합니다.
Android UI 모드에 대 한 자세한 내용은 참조 하십시오 [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/)합니다.

## <a name="action-bar-settings"></a>작업 모음 설정

합니다 **작업 모음 설정** 아이콘은 페인트 브러시 (테마 편집기) 아이콘의 왼쪽에 사용할 수 있습니다.

![작업 모음 설정](resource-qualifiers-images/vs/14-action-bar.png "작업 모음 설정")

이 아이콘에는 세 가지 작업 모음 모드 중 하나를 선택 하는 방법을 제공 하는 대화 팝 오버를 열립니다.

-   **표준** &ndash; 로고 또는 선택적 자막을 사용 하 여 아이콘 및 제목 텍스트를 구성 합니다.

-   **목록** &ndash; 목록 탐색 모드입니다. 정적 제목 텍스트 대신이 모드는 활동 내에서 탐색에 대 한 목록 메뉴를 표시 (즉, 제공할 수 있습니다 드롭다운 목록으로 사용자에 게).

-   **탭** &ndash; 탭 탐색 모드입니다. 이 모드는 정적 제목 텍스트 대신 일련의 작업 내에서 탐색에 대 한 탭 표시합니다.

## <a name="themes"></a>테마

합니다 **테마** 모든 프로젝트에 정의 된 테마의 드롭다운 메뉴 표시 합니다. 선택 **테마 보다** 아래와 같이 설치 된 Android SDK에서 사용할 수 있는 모든 테마의 목록을 사용 하 여 대화 상자를 엽니다.

[![자세한 테마 목록의](resource-qualifiers-images/vs/15-theme-menu-sml.png "더 테마 목록")](resource-qualifiers-images/vs/15-theme-menu.png#lightbox)

테마를 선택 하면 디자인 화면 새 테마의 효과 표시 하도록 업데이트 됩니다. 이 변경에 영구 이루어집니다은 경우에만 **확인** 에서 단추를 클릭 합니다 **테마** 대화 합니다. 포함에 테마를 선택 합니다 **테마** 드롭 다운 메뉴 표시 된 것 처럼 아래:

![밝은 테마에 출시 되었습니다](resource-qualifiers-images/vs/16-light-theme.png "밝은 테마 출시 되었습니다.")

## <a name="android-version"></a>Android 버전

Android **버전** 선택기 디자이너에서 레이아웃을 렌더링 하는 데 사용 되는 Android 버전을 설정 합니다. 선택기는 프로젝트의 대상 프레임 워크 버전을 사용 하 여 호환 되는 모든 버전을 표시 합니다.

![Android 버전의 목록은](resource-qualifiers-images/vs/17-android-version.png "Android 목록 버전")

프로젝트의 설정에서 대상 프레임 워크 버전을 설정할 수 있습니다 **속성 > 응용 프로그램 > Android 버전을 사용 하 여 컴파일**합니다. 대상 프레임 워크 버전에 대 한 자세한 내용은 참조 하세요. [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)합니다.

프로젝트의 대상 프레임 워크 버전에서 도구 상자에서 사용할 수 있는 위젯 집합이 결정 됩니다. 사용할 수 있는 속성에도 마찬가지 합니다 **속성 창**합니다. 위젯의 사용 가능한 목록이 *되지* 에서 선택한 값에 의해 결정 합니다 **버전** 도구 모음의 선택기. 예를 들어 Android 4.4를 프로젝트의 대상 버전을 설정 하면 Android 6.0에서 프로젝트 모양을 확인 하려면 도구 모음 버전 선택기에 Android 6.0을 선택할 수 있지만 Android 6.0에만 적용 되는 위젯을 추가할 수 없습니다 &ndash;  아직 Android 4.4에서 사용할 수 있는 위젯으로 제한 됩니다.

리소스 종류에 대 한 자세한 내용은 참조 하세요. [Android 리소스](~/android/app-fundamentals/resources-in-android/index.md)합니다.



# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="resource-qualifier-options"></a>리소스 한정자 옵션

**리소스 한정자 옵션** 오른쪽의 줄임표 (...) 아이콘을 클릭 하 여 액세스할 수 합니다 **가로** 모드 단추:

[![리소스 한정자 옵션](resource-qualifiers-images/xs/08-resource-qual-opt-m75-sml.png)](resource-qualifiers-images/xs/08-resource-qual-opt-m75.png#lightbox)

이 대화 상자는 다음 리소스 한정자에 대 한 풀 다운 메뉴를 제공합니다.

-   **언어** &ndash; 사용 가능한 언어 리소스를 표시 하 고 새 언어/지역 리소스를 추가 하는 옵션을 제공 합니다.

-   **UI 모드** &ndash; 목록 디스플레이 모드 (같은 **자동차 도크** 하 고 **데스크 도크**) 레이아웃 지침 뿐만 아니라 합니다.

이러한 풀 다운 메뉴의 각 선택 하 고 (설명 했 듯이 다음) 리소스 한정자를 구성할 수 있는 새 대화 상자가 열립니다.

### <a name="language"></a>언어

합니다 **언어** 정의 된 리소스가 있는 해당 언어를 나열 하는 풀 다운 메뉴 (또는 **모든 언어**, 기본값). 그러나 이기도 한 **언어/지역 추가...**  옵션 목록에 새 언어를 추가할 수 있습니다.

[![언어/지역 추가](resource-qualifiers-images/xs/09-add-language-region-m75-sml.png)](resource-qualifiers-images/xs/09-add-language-region-m75.png#lightbox)

클릭할 때 **언어/지역 추가...** 서 **언어 선택** 사용 가능한 언어 및 지역 드롭다운 목록을 표시할 대화 상자가 열립니다.

[![언어 목록](resource-qualifiers-images/xs/10-languages-m75-sml.png)](resource-qualifiers-images/xs/10-languages-m75.png#lightbox)

이 예제에서는 했습니다 **fr (프랑스어)** 언어용 및 **BE** (벨기에) 프랑스어의 국가별 언어에 대 한 합니다. 유의 합니다 **지역** 특정 지역에 관계 없이 다양 한 언어를 지정할 수 있으므로 필드는 선택 사항입니다. 경우는 **언어** 풀 다운 메뉴를 다시 열, 새로 추가 된 언어/지역 리소스를 표시 합니다.

[![언어 및 지역 선택](resource-qualifiers-images/xs/11-language-region-added-m75-sml.png)](resource-qualifiers-images/xs/11-language-region-added-m75.png#lightbox)

새 언어를 추가할 경우 다음에 표시 될 추가 언어는 더 이상이 대 한 새 리소스 만들지 열면 프로젝트를 참고 합니다.

### <a name="ui-mode"></a>UI 모드

클릭할 때 합니다 **UI 모드** 같은의 모드 목록을 풀 다운 메뉴에서 표시 됩니다 **Normal**, **자동차 도크**, **데스크 도크**, **텔레비전**하십시오 **어플라이언스**, 및 **조사식**:

[![UI 모드 메뉴](resource-qualifiers-images/xs/12-ui-mode-m75-sml.png)](resource-qualifiers-images/xs/12-ui-mode-m75.png#lightbox)

이 목록 아래에 야간 모드 **하지 밤** 및 **밤**레이아웃 지침 뒤 **왼쪽에서 오른쪽** 및 **오른쪽에서 왼쪽으로**입니다. 마지막 옵션 쌍을 사용 하면 하나를 선택할 수 있습니다 **화면을 반올림** 또는 **직사각형 화면** (Android Wear 장치에 유용함).

Android UI 모드에 대 한 자세한 내용은 참조 하십시오 [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/)합니다.
에 대 한 자세한 **왼쪽에서 오른쪽** 하 고 **오른쪽에서 왼쪽** 옵션을 참조 하십시오 [LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/)합니다.


## <a name="action-bar-settings"></a>작업 모음 설정

합니다 **작업 모음 설정** 아이콘은 페인트 브러시 (테마 편집기) 아이콘의 왼쪽에 사용할 수 있습니다.

[![작업 모음 설정](resource-qualifiers-images/xs/13-action-bar-m75-sml.png)](resource-qualifiers-images/xs/13-action-bar-m75.png#lightbox)

이 아이콘에는 세 가지 작업 모음 모드 중 하나를 선택 하는 방법을 제공 하는 대화 팝 오버를 열립니다.

-   **표준** &ndash; 로고, 아이콘 및 제목 텍스트는 선택적 자막을 사용 하 여 두 이루어져 있습니다.

-   **목록** &ndash; 목록 탐색 모드입니다. 정적 제목 텍스트 대신이 모드는 활동 내에서 탐색에 대 한 목록 메뉴를 표시 (즉, 제공할 수 있습니다 드롭다운 목록으로 사용자에 게).

-   **탭** &ndash; 탭 탐색 모드입니다. 이 모드는 정적 제목 텍스트 대신 일련의 작업 내에서 탐색에 대 한 탭 표시합니다.

## <a name="themes"></a>테마

합니다 **테마** 모든 프로젝트에 정의 된 테마의 드롭다운 메뉴 표시 합니다. 선택 **테마 보다** 아래와 같이 설치 된 Android SDK에서 사용할 수 있는 모든 테마의 목록을 사용 하 여 대화 상자를 엽니다.

[![자세한 테마 목록](resource-qualifiers-images/xs/14-theme-menu-m75-sml.png)](resource-qualifiers-images/xs/14-theme-menu-m75.png#lightbox)

테마를 선택 하면 디자인 화면 새 테마의 효과 표시 하도록 업데이트 됩니다. 이 변경에 영구 이루어집니다은 경우에만 **확인** 에서 단추를 클릭 합니다 **테마** 대화 합니다. 포함에 테마를 선택 합니다 **테마** 드롭 다운 메뉴 표시 된 것 처럼 아래:

[![밝은 테마 출시 되었습니다.](resource-qualifiers-images/xs/15-light-theme-m75-sml.png)](resource-qualifiers-images/xs/15-light-theme-m75.png#lightbox)

## <a name="android-version"></a>Android 버전

Android **버전** 선택기 디자이너에서 레이아웃을 렌더링 하는 데 사용 되는 Android 버전을 설정 합니다. 선택기는 프로젝트의 대상 프레임 워크 버전을 사용 하 여 호환 되는 모든 버전을 표시 합니다.

[![Android 버전 목록](resource-qualifiers-images/xs/16-android-version-m75-sml.png)](resource-qualifiers-images/xs/16-android-version-m75.png#lightbox)

프로젝트의 설정에서 대상 프레임 워크 버전을 설정할 수 있습니다 합니다 **프로젝트 옵션 > 빌드 > 일반** 섹션입니다. 대상 프레임 워크 버전에 대 한 자세한 내용은 참조 하세요. [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)합니다.

프로젝트의 대상 프레임 워크 버전에서 도구 상자에서 사용할 수 있는 위젯 집합이 결정 됩니다. 사용할 수 있는 속성에 대 한 true 이기도 합니다 **속성 패드**합니다. 위젯의 사용 가능한 목록이 *되지* 에서 선택한 값에 의해 결정 합니다 **버전** 도구 모음의 선택기. 예를 들어 Android 4.4를 프로젝트의 대상 버전을 설정 하면 Android 6.0에서 프로젝트 모양을 확인 하려면 도구 모음 버전 선택기에 Android 6.0을 선택할 수 있지만 Android 6.0에만 적용 되는 위젯을 추가할 수 없습니다 &ndash;  아직 Android 4.4에서 사용할 수 있는 위젯으로 제한 됩니다.

리소스 종류에 대 한 자세한 내용은 참조 하세요. [Android 리소스](~/android/app-fundamentals/resources-in-android/index.md)합니다.

-----
