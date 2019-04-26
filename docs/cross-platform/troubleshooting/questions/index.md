---
title: 일반 질문과 대답
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 06aa6569301d1bfdbf9f6fd1e7397a38a9beb6f6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61157195"
---
# <a name="general-frequently-asked-questions"></a>일반 질문과 대답

## <a name="portable-class-libraries"></a>이식 가능한 클래스 라이브러리

### <a name="how-can-i-view-what-libraries-are-supported-in-a-pclpcl-support-librariesmd"></a>[PCL에서 지원되는 라이브러리를 어떻게 볼 수 있나요?](pcl-support-libraries.md)
이 가이드는 리소스 및 기존 라이브러리는 다양 한 PCL 플랫폼을 대상으로 사용할 수 있거나 PCL 프로필에 변환 될 수 있는지 확인 하는 메서드를 나열 합니다.

### <a name="pcl-reflection-apipcl-reflectionmd"></a>[PCL 리플렉션 API](pcl-reflection.md)
Microsoft 이식 가능한 클래스 라이브러리의 사용을 위한 새로운 리플렉션 API를 개발 합니다. 일부 기존 리플렉션 코드는 PCL을 이동 하려는 경우 작동 하지 않을 수 있습니다.

### <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-packagepcl-case-studymd"></a>[PCL 사례 연구: Microsoft TPL 데이터 흐름 NuGet 패키지용 system.diagnostics.tracing과 관련 된 문제를 해결 하는 방법](pcl-case-study.md)
Xamarin.iOS 및 Xamarin.Android에는 참조로 허용 되는 모든 PCL 프로필의 100%를 구현 하지 않습니다. Visual Studio for Mac을 Visual Studio 및 NuGet 패키지 관리자에서에서 실제 편의 위해 Xamarin 프로젝트만 불완전 한 구현이 있는 몇 가지 프로필의 사용을 허용 합니다. 예를 들어 Xamarin.iOS 아니고 Xamarin.Android 현재 포함 형식에 완전 한 구현 된 `System.Diagnostics.Tracing` PCL 네임 스페이스입니다. 앱 프로젝트를 이식 가능 net45 + win8 + wp8 + wpa81 참조를 전환 하 여이 문제를 해결 해도 TPL 데이터 흐름 라이브러리의 버전입니다.

## <a name="nuget-packages--xamarin-components"></a>NuGet 패키지 및 Xamarin 구성 요소
### <a name="how-can-i-update-nugetnuget-updatemd"></a>[NuGet을 업데이트하려면 어떻게 해야 하나요?](nuget-update.md)
NuGet 업데이트, 확장 및 추가 기능에서 찾을 수 있습니다 합니다 **업데이트** 탭에 **NuGet 패키지 관리자**합니다. Visual Studio에서 Mac 및 Visual Studio에 대 한 업데이트를 찾으려면 자세한 탐색이이 가이드의 경우

### <a name="how-do-i-downgrade-a-nuget-packagenuget-package-downgrademd"></a>[NuGet 패키지를 다운그레이드하려면 어떻게 해야 하나요?](nuget-package-downgrade.md)
Mac 용 visual Studio 및 Visual Studio가 이전 버전의 패키지를 선택 하 고 자동으로 설치 하는 기능 어떻게 작동 패키지 업데이트와 유사 합니다.

### <a name="missing-packages-error-after-updating-nuget-packagesnuget-packages-missingmd"></a>[Nuget 패키지를 업데이트한 후 패키지 오류 없음](nuget-packages-missing.md)
이 문제는 주로 Xamarin.Forms 샘플 앱 솔루션에 보고 되었습니다 하지만 NuGet 패키지를 사용 하는 모든 프로젝트에서이 문제에 대 한 잠재적인 발생할 수 있습니다.

### <a name="unifying-google-play-services-components-and-nugetgps-components-nugetmd"></a>[Google Play 서비스 구성 요소 및 NuGet 통합](gps-components-nuget.md)
여러 Google Play 서비스 구성 요소 및 NuGet 패키지를 수 있지만 쉽게 하려면 개발자를 위한 사용에서는 했으므로 이제 통합이 구성 요소 및 NuGet 패키지를 두 개 있습니다. 거의 모든 경우 Google Play 서비스를 사용 해야 합니다. (Froyo) 패키지를 사용 하는 유일한 이유는 적극적으로 Froyo를 대상으로 하는 경우.

### <a name="where-are-the-components-stored-on-my-machinecomponent-storagemd"></a>[컴퓨터에 구성 요소가 저장된 위치는 어디인가요?](component-storage.md)
앱 프로젝트로 Xamarin 구성 요소를 설치할 때마다이 가이드에 나열 된 두 위치에서이 배치를 가져옵니다.


## <a name="troubleshooting"></a>문제 해결
### <a name="where-can-i-find-my-version-information-and-logsversion-logsmd"></a>[버전 정보 및 로그는 어디에서 확인할 수 있나요?](version-logs.md)
Xamarin 문제를 해결 하는 데 사용할 수 있는 대부분의 진단 정보를 찾을 수 있는 위치에 설명 합니다.

### <a name="when-and-how-should-i-file-a-bug-reporthowto-file-bugmd"></a>[버그 보고서를 언제 어떻게 제출해야 하나요?](howto-file-bug.md)
이 가이드는 엔지니어는 문제에 대 한 원인을 (및 잠재적 수정)를 보다 효율적으로 확인할 수 있도록 고품질 버그 보고서를 작성 하기 위한 팁을 제공 합니다.

### <a name="why-isnt-jenkins-supported-by-xamarinxamarin-jenkinsmd"></a>[Xamarin에서 Jenkins가 지원되지 않는 이유는 무엇인가요?](xamarin-jenkins.md)
Jenkins는 오픈 소스 CI 제품군; Jenkins에서 직접 발생 하는 많은 문제로 인해 *자체* 코드를 가져온 위치에 대해;와 같은 문제를 정리할 수 있도록 해야 합니다는 [기본 Jenkins 리포지토리의](https://github.com/jenkinsci/jenkins), 또는 대 한 리포지토리 [ Jenkins.app](https://github.com/stisti/jenkins-app)합니다.

### <a name="what-project-settings-are-required-for-the-debuggerdebugger-settingsmd"></a>[디버거에 필요한 프로젝트 설정은 무엇인가요?](debugger-settings.md)
(중단점 적중 횟수, 디버그 로그 표시 등)를 예상 대로 작동 하도록 디버거에 대 한 순서로 개발자 계측 및 디버그 정보 표시가 모두 활성화 되어야 합니다. 이 가이드에는 찾아 이러한 설정을 활성화 하는 방법을 자세히 설명 합니다.

