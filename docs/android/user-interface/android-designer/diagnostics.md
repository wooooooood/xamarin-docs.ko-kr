---
title: Android 레이아웃 진단
description: Android 레이아웃 진단에 대해 설명 하 고 시작 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: BD252EA7-7E69-4DB4-96AB-D52CC0510C8F
ms.technology: xamarin-android
author: decriptor
ms.author: stepsha
ms.date: 03/24/2020
ms.openlocfilehash: 5c29a1a80d8c1f599f0bbc750d22d8334ddb3494
ms.sourcegitcommit: d83c6af42ed26947aa7c0ecfce00b9ef60f33319
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80247817"
---
# <a name="android-layout-diagnostics"></a>Android 레이아웃 진단

Android layout 진단은 일반적인 품질 문제와 유용한 최적화를 강조 표시 하 여 Android 레이아웃 파일의 품질을 향상 시킬 수 있도록 설계 되었습니다. 이 기능은 Visual Studio 16.5 + 및 Mac용 Visual Studio 8.5 이상에서 사용할 수 있습니다.

기본 분석기 집합은 다양 한 문제에 대해 제공 되며, 각 기능을 사용자 지정 하 여 프로젝트의 특정 요구 사항을 충족할 수 있습니다. 분석기는 Android lint 시스템을 기반으로 합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

## <a name="enable-android-layout-diagnostics-on-visual-studio-2019"></a>Visual Studio 2019에서 Android 레이아웃 진단 사용

레이아웃 진단 설정인 **레이아웃 진단 사용**이 설정 되어 있는지 확인 합니다. 이 옵션 페이지에 액세스 하려면 **도구** > **옵션**을 선택한 다음 **텍스트 편집기** > **Android XML** > **고급**을 선택 합니다.

![진단 옵션을 사용 하도록 설정 하는 방법을 보여 주는 옵션 대화 상자](diagnostics-images/AndroidDiagnosticsEnableOption.png)

사용 하도록 설정 되 면 Android 레이아웃 편집기에서 다음과 같은 문제가 표시 됩니다.

![Visual Studio 2019에서 Android 진단 사용](diagnostics-images/AndroidDiagnosticsEnabled.png)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

## <a name="enable-android-layout-diagnostics-on-visual-studio-for-mac"></a>Mac용 Visual Studio에서 Android 레이아웃 진단을 사용 하도록 설정

레이아웃 진단 설정인 **레이아웃 진단 사용**이 설정 되어 있는지 확인 합니다. 이 옵션 페이지에 액세스 하려면 **Visual Studio** > **기본 설정 ...** 을 선택한 다음 **텍스트 편집기** > **Android XML**을 선택 합니다.

![진단 옵션을 사용 하도록 설정 하는 방법을 보여 주는 기본 설정 대화 상자](diagnostics-images/AndroidDiagnosticsEnableOptionVSmac.png)

사용 하도록 설정 되 면 Android 레이아웃 편집기에서 다음과 같은 문제가 표시 됩니다.

![Mac용 Visual Studio에서 Android 진단 사용](diagnostics-images/AndroidDiagnosticsEnabledVSmac.png)

-----

## <a name="features"></a>기능

다음 섹션에서는 Android 레이아웃 진단에서 사용할 수 있는 기능을 간략하게 설명 합니다.

### <a name="analyzers"></a>분석기

분석기는 레이아웃 파일의 문제를 감지 하는 데 사용 됩니다. 일부는 하드 코드 된 값을 줄이고, 성능을 개선 하 고, 오류를 플래그를 지정 하는 방법입니다.

### <a name="diagnostic-configuration"></a>진단 구성

분석기는 XML 파일을 사용 하 여 구성할 수 있으므로 기본 심각도 수준을 변경 하 고, 특정 파일을 무시 하 고, 변수를 전달할 수 있습니다.

여러 Android 앱에서 공유 하려는 구성 집합이 있는 경우 기준 파일을 사용할 수 있습니다. 이 기능을 사용 하려면 새 구성 파일을 만들고 파일 이름에 `-baseline`을 추가 합니다. 기준선 구성이 먼저 적용 된 다음 나머지 구성 파일이 적용 됩니다.

> [!TIP]
> 이는 새 Android 앱 또는 기존 Android 앱에서 문제 집합을 무시 하려는 경우에 유용할 수 있습니다.

형식:

```xml
<?xml version="1.0" encoding="utf-8" ?> 
<configuration>
    <issue id="DuplicateIDs" severity="warning">
        <ignore path="Resources/layout/layout1.xml" />
    </issue>
    <issue id="HardcodedText" severity="informational">
        <ignore path="Resources/layout/layout1.xml" />
        <ignore path="Resource/layout/layout2.xml" />
    </issue>
    <issue id="TooManyViews">
        <variable name="MAX_VIEW_COUNT" value="12" />
    </issue>
    <issue id="TooDeepLayout">
        <variable name="MAX_DEPTH" value="12" />
    </issue>
</configuration>
```

> [!NOTE]
> 현재 유일한 변수 (기본값: 80)와 `TooManyViews` 및 `TooDeepLayout`에 대 한 `MAX_DEPTH` (기본값: 10)이 `MAX_VIEW_COUNT` 됩니다.

경고 심각도 수준은 다음과 같습니다.

- 제안
- 정보
- Warning
- Error
- 무시

### <a name="add-a-configuration-file"></a>구성 파일 추가

Android 앱 프로젝트의 루트에 새 XML 파일을 만듭니다. 파일 이름은 중요 하지 않지만이 예제에서는 `AndroidLayoutDiagnostics.xml`를 사용 합니다.

![새 항목 추가](diagnostics-images/AndroidDiagnosticsNewFileDialog.png)

새 XML 파일이 추가 되 면 Android 앱 프로젝트 트리에 표시 됩니다.

![Android 앱 프로젝트 트리](diagnostics-images/AndroidDiagnosticsFileAddToTree.png)

속성 패널에서 빌드 작업을 **AndroidResourceAnalysisConfig** 로 설정 했는지 확인 합니다.
새 파일에 대 한 속성 패널을 가져오는 가장 쉬운 방법은 파일을 마우스 오른쪽 단추로 클릭 하 고 속성을 선택 하는 것입니다. 속성 패널이 표시 되 면 **빌드 작업** 을 **AndroidResourceAnalysisConfig**로 변경 해야 합니다.

![항목 속성에서 빌드 작업 설정](diagnostics-images/AndroidDiagnosticsSetBuildAction.png)

이제 빈 XML 파일이 있으므로 `<configuration>` 루트 요소를 추가 해야 합니다. 이제 지원 되는 모든 문제의 기본 동작을 조정할 수 있습니다.
하드 코드 된 문자열이 오류 추가로 처리 되는지 확인 하려면 다음을 수행 합니다.

```xml
<issue="HardcodedText" severity="error">
</issue>
```

![진단 구성 파일](diagnostics-images/AndroidDiagnosticsConfigurationFileExample.png)

하드 코드 된 텍스트가 오류로 간주 되므로 이제 레이아웃 편집기에서 빨간색 물결선으로 플래그가 지정 됩니다.

![진단 구성을 사용 하는 레이아웃](diagnostics-images/AndroidDiagnosticsUsingConfiguration.png)

> [!NOTE]
> 새 구성 파일 변경 내용을 적용 하려면 현재 열려 있는 레이아웃 파일을 다시 열어야 합니다.
>

## <a name="troubleshooting"></a>문제 해결

가능한 몇 가지 일반적인 문제는 다음과 같습니다.

- XML 형식 오류가 없는지 확인 합니다.
- 빌드 작업이 **AndroidResourceAnalysisConfig**로 올바르게 설정 되었습니다.

## <a name="known-issues"></a>알려진 문제

- 오류 패드는 파일이 처음으로 변경 될 때까지 채워지지 않습니다.

## <a name="related-links"></a>관련 링크

- [Android의 보풀이 검사](http://tools.android.com/tips/lint-checks)
- [보풀이 없는 검사를 통해 코드 향상](https://developer.android.com/studio/write/lint)
