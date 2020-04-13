---
title: 안드로이드 레이아웃 진단
description: Android 레이아웃 진단 및 시작 하는 방법에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: BD252EA7-7E69-4DB4-96AB-D52CC0510C8F
ms.technology: xamarin-android
author: decriptor
ms.author: stepsha
ms.date: 03/24/2020
ms.openlocfilehash: 746f74e68fa4816f1f7979980af9506dc0173542
ms.sourcegitcommit: 765b69ed451a0f48625ea597c3f39de95f3ae693
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80987584"
---
# <a name="android-layout-diagnostics"></a>안드로이드 레이아웃 진단

Android 레이아웃 진단은 일반적인 품질 문제와 유용한 최적화를 강조하여 Android 레이아웃 파일의 품질을 개선하는 데 도움이 되도록 설계되었습니다. 이 기능은 Visual Studio 16.5+ 및 Mac 8.5 이상용 비주얼 스튜디오모두에서 사용할 수 있습니다.

다양한 문제에 대해 기본 분석기 집합이 제공되며 각 분석기는 프로젝트의 특정 요구 사항을 충족하도록 사용자 지정할 수 있습니다. 분석기는 느슨하게 안드로이드 리팅 시스템을 기반으로합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

## <a name="enable-android-layout-diagnostics-on-visual-studio-2019"></a>비주얼 스튜디오 2019에서 안드로이드 레이아웃 진단 사용

레이아웃 진단 설정, 레이아웃 진단 활성화가 활성화되어 있는지 **확인합니다.** 이 옵션 페이지에 액세스하려면 **도구** > **옵션을**선택한 다음 **텍스트 편집기** > **Android XML** > **Advanced를**선택합니다.

![진단 옵션을 활성화하는 방법을 보여 주는 옵션 대화 상자](diagnostics-images/AndroidDiagnosticsEnableOption.png)

일단 활성화, 안 드 로이드 레이아웃 편집기 문제를 표시 합니다.:

![비주얼 스튜디오 2019에서 사용 가능한 안드로이드 진단](diagnostics-images/AndroidDiagnosticsEnabled.png)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

## <a name="enable-android-layout-diagnostics-on-visual-studio-for-mac"></a>Mac용 비주얼 스튜디오에서 안드로이드 레이아웃 진단 사용

레이아웃 진단 설정, 레이아웃 진단 활성화가 활성화되어 있는지 **확인합니다.** 이 옵션 페이지에 액세스하려면 **Visual Studio** > **환경 설정을**선택한 다음 텍스트 **편집기** > **Android XML을**선택합니다.

![진단 옵션을 활성화하는 방법을 보여 주는 기본 설정 대화 상자](diagnostics-images/AndroidDiagnosticsEnableOptionVSmac.png)

일단 활성화, 안 드 로이드 레이아웃 편집기 문제를 표시 합니다.:

![Mac용 비주얼 스튜디오에서 안드로이드 진단 사용 가능](diagnostics-images/AndroidDiagnosticsEnabledVSmac.png)

-----

## <a name="features"></a>기능

다음 섹션에서는 Android 레이아웃 진단에서 사용 가능한 기능을 간략하게 설명합니다.

### <a name="analyzers"></a>분석기

분석기는 레이아웃 파일의 문제를 감지하고, 하드 코딩된 값을 줄이고, 성능을 향상시키고, 오류를 표시하는 데 사용됩니다. 분석기 목록은 [Android 디자이너 진단 분석기](diagnostic-analyzers.md) 참조

### <a name="diagnostic-configuration"></a>진단 구성

분석기는 XML 파일을 사용하여 구성할 수 있으므로 기본 심각도 수준을 변경하고 특정 파일을 무시하고 변수를 전달할 수 있습니다.

여러 Android 앱에서 공유하려는 구성 집합이 있는 경우 기준 파일을 사용할 수 있습니다. 이 기능을 사용하려면 새 구성 파일을 `-baseline` 만들고 파일 이름에 부가합니다. 기준 구성이 먼저 적용된 다음 나머지 구성 파일이 적용됩니다.

> [!TIP]
> 이 기능은 새 Android 앱또는 기존 Android 앱에서 일련의 문제를 무시하려는 경우에 유용할 수 있습니다.

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
> 현재 유일한 변수는 `MAX_VIEW_COUNT` (기본값: 80) 및 `MAX_DEPTH` (기본값: 10) `TooManyViews` 각각입니다. `TooDeepLayout`

경고 심각도 수준은 다음과 같습니다.

- 제안
- 정보
- Warning
- Error
- 무시

### <a name="add-a-configuration-file"></a>구성 파일 추가

Android 앱 프로젝트의 루트에 새 XML 파일을 만듭니다. 파일 이름은 중요하지 않지만 이 예제에서는 다음을 사용합니다. `AndroidLayoutDiagnostics.xml`

![새 항목 추가](diagnostics-images/AndroidDiagnosticsNewFileDialog.png)

새 XML 파일이 추가되면 Android 앱 프로젝트 트리에 나타납니다.

![안드로이드 앱 프로젝트 트리](diagnostics-images/AndroidDiagnosticsFileAddToTree.png)

빌드 작업이 속성 패널에서 **AndroidResourceAnalysisConfig로** 설정되어 있는지 확인합니다.
새 파일의 속성 패널을 가져오는 가장 쉬운 방법은 파일을 마우스 오른쪽 단추로 클릭하고 속성을 선택하는 것입니다. 속성 패널이 표시되면 **AndroidResourceAnalysisConfig로** **빌드 작업을** 변경해야 합니다.

![항목 속성에서 빌드 작업 설정](diagnostics-images/AndroidDiagnosticsSetBuildAction.png)

이제 빈 XML 파일이 있으므로 `<configuration>` 루트 요소를 추가해야합니다. 이 시점에서 지원되는 모든 문제의 기본 동작을 조정할 수 있습니다.
하드 코딩된 문자열이 오류로 처리되도록 하려면 다음을 추가합니다.

```xml
<issue="HardcodedText" severity="error">
</issue>
```

![진단 구성 파일](diagnostics-images/AndroidDiagnosticsConfigurationFileExample.png)

이제 하드 코딩 된 텍스트가 오류로 간주되었으므로 레이아웃 편집기에서 빨간색 물결선이 표시됩니다.

![진단 구성을 사용한 레이아웃](diagnostics-images/AndroidDiagnosticsUsingConfiguration.png)

> [!NOTE]
> 새 구성 파일 변경 사항이 적용되려면 현재 열려 있는 레이아웃 파일을 다시 열어야 합니다.
>

## <a name="troubleshooting"></a>문제 해결

다음은 몇 가지 일반적인 문제입니다.

- XML 형식 오류가 없는지 확인합니다.
- 빌드 작업이 **AndroidResourceAnalysisConfig로**올바르게 설정됩니다.

## <a name="known-issues"></a>알려진 문제

- 파일이 처음 변경될 때까지 오류 패드가 채워지지 않습니다.

## <a name="related-links"></a>관련 링크

- [안드로이드 보풀 검사](http://tools.android.com/tips/lint-checks)
- [보풀 검사로 코드 개선](https://developer.android.com/studio/write/lint)
