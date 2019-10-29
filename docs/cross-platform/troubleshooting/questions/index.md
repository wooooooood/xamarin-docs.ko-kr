---
title: 일반적인 질문과 대답
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
ms.openlocfilehash: 0e49ef8fa0bf00d5ed41f3411393ffaf4891c1b8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73013986"
---
# <a name="general-frequently-asked-questions"></a>일반적인 질문과 대답

## <a name="portable-class-libraries"></a>이식 가능한 클래스 라이브러리

### <a name="how-can-i-view-what-libraries-are-supported-in-a-pclpcl-support-librariesmd"></a>[PCL에서 지원되는 라이브러리를 어떻게 볼 수 있나요?](pcl-support-libraries.md)
이 가이드에는 기존 라이브러리가 다양 한 PCL 대상 플랫폼에서 지원 되는지 또는 PCL 프로필로 변환 될 수 있는지를 확인 하는 리소스 및 방법이 나열 되어 있습니다.

### <a name="pcl-reflection-apipcl-reflectionmd"></a>[PCL 리플렉션 API](pcl-reflection.md)
Microsoft는 이식 가능한 클래스 라이브러리에서 사용할 새 리플렉션 API를 개발 했습니다. PCL로 이동 하려는 기존 리플렉션 코드가 있으면 작동 하지 않을 수 있습니다.

### <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-packagepcl-case-studymd"></a>[PCL 사례 연구: Microsoft TPL 데이터 흐름 NuGet 패키지용 System.Diagnostics.Tracing과 관련된 문제를 해결하려면 어떻게 해야 하나요?](pcl-case-study.md)
Xamarin.ios 및 Xamarin은 참조로 허용 하는 모든 PCL 프로필의 100%를 구현 하지 않습니다. Mac용 Visual Studio, Visual Studio 및 NuGet 패키지 관리자의 실용적인 편의를 위해 Xamarin 프로젝트를 사용 하면 불완전 한 구현만 있는 여러 프로필을 사용할 수 있습니다. 예를 들어 Xamarin.ios와 Xamarin.ios는 현재 `System.Diagnostics.Tracing` PCL 네임 스페이스에 있는 형식의 완전 한 구현을 포함 하지 않습니다. Net45 + win8 + wp8 + wpa81 버전의 TPL 데이터 흐름 라이브러리를 참조 하도록 앱 프로젝트를 전환 하면이 문제를 해결할 수 있습니다.

## <a name="nuget-packages--xamarin-components"></a>Xamarin 구성 요소 & NuGet 패키지
### <a name="how-can-i-update-nugetnuget-updatemd"></a>[NuGet을 업데이트하려면 어떻게 해야 하나요?](nuget-update.md)
Nuget 업데이트, 확장 및 추가 기능은 **Nuget 패키지 관리자**의 **업데이트** 탭에서 찾을 수 있습니다. Visual Studio Mac용 Visual Studio &에서 업데이트를 찾기 위한 자세한 탐색은이 가이드에 나와 있습니다.

### <a name="how-do-i-downgrade-a-nuget-packagenuget-package-downgrademd"></a>[NuGet 패키지를 다운그레이드하려면 어떻게 해야 하나요?](nuget-package-downgrade.md)
Mac용 Visual Studio & Visual Studio에는 이전 버전의 패키지를 선택 하 고 자동으로 설치 하는 기능이 있습니다. 패키지 업데이트의 작동 방법과 비슷합니다.

### <a name="missing-packages-error-after-updating-nuget-packagesnuget-packages-missingmd"></a>[Nuget 패키지를 업데이트한 후 패키지 오류 없음](nuget-packages-missing.md)
이 문제는 주로 Xamarin.ios 샘플 앱 솔루션에 대해 보고 되었지만 NuGet 패키지를 사용 하는 모든 프로젝트에서이 문제가 발생할 수 있습니다.

### <a name="unifying-google-play-services-components-and-nugetgps-components-nugetmd"></a>[Google Play 서비스 구성 요소 및 NuGet 통합](gps-components-nuget.md)
몇 가지 Google Play 서비스 구성 요소와 NuGet 패키지를 사용 했지만 개발자의 작업을 더 쉽게 수행 하기 위해 이제 구성 요소와 NuGet 패키지를 둘로 통합 했습니다. 거의 모든 경우에 Google Play 서비스를 사용 해야 합니다. (안 됨) 패키지를 사용 하는 유일한 이유는 대기를 대상으로 하는 경우입니다.

### <a name="where-are-the-components-stored-on-my-machinecomponent-storagemd"></a>[컴퓨터에 구성 요소가 저장된 위치는 어디인가요?](component-storage.md)
Xamarin 구성 요소를 앱 프로젝트에 설치할 때마다이 가이드에 나열 된 두 위치에 배치 됩니다.

## <a name="troubleshooting"></a>문제 해결
### <a name="where-can-i-find-my-version-information-and-logsversion-logsmd"></a>[버전 정보 및 로그는 어디에서 확인할 수 있나요?](version-logs.md)
이 가이드에서는 Xamarin 문제를 해결 하는 데 사용할 수 있는 대부분의 진단 정보를 찾을 수 있는 위치에 대해 자세히 설명 합니다.

### <a name="when-and-how-should-i-file-a-bug-reporthowto-file-bugmd"></a>[버그 보고서를 언제 어떻게 제출해야 하나요?](howto-file-bug.md)
이 가이드에서는 고품질 버그 보고서를 작성 하는 데 유용한 정보를 제공 하므로 엔지니어가 문제에 대 한 원인과 잠재적 해결 방법을 더 효율적으로 파악할 수 있습니다.

### <a name="why-isnt-jenkins-supported-by-xamarinxamarin-jenkinsmd"></a>[Xamarin에서 Jenkins가 지원되지 않는 이유는 무엇인가요?](xamarin-jenkins.md)
Jenkins은 오픈 소스 CI 도구 모음입니다. 이러한 문제 때문에 Jenkins *자체* 에서 직접 발생 하는 많은 문제를 코드를 받은 위치에 대 한 문제로 정리 해야 합니다. [주 Jenkins 리포지토리](https://github.com/jenkinsci/jenkins), [Jenkins](https://github.com/stisti/jenkins-app)에 대 한 리포지토리 등이 있습니다.

### <a name="what-project-settings-are-required-for-the-debuggerdebugger-settingsmd"></a>[디버거에 필요한 프로젝트 설정은 무엇인가요?](debugger-settings.md)
디버거가 정상적으로 작동 하려면 (중단점 적중, 디버그 로그 표시 등), 개발자 계측 및 디버그 정보 표시를 모두 사용 하도록 설정 해야 합니다. 이 가이드에서는 이러한 설정을 찾고 활성화 하는 방법에 대해 자세히 설명 합니다.
