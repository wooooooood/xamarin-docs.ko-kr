---
title: Jenkins는 Xamarin에서 지원 되지 않는 이유는?
description: 이 문서 Jenkins CI 시스템과 상호 작용 Xamarin의 상위 수준에서 설명합니다. 또한 Jenkins 작업할 때 제기 되는 몇 가지 일반적인 문제를 설명 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9951F980-2C6C-47C0-8A35-A78F06C20BEB
author: asb3993
ms.author: amburns
ms.openlocfilehash: cf1a59d3084f178187209fdf3999af10efe6203a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782452"
---
# <a name="why-isnt-jenkins-supported-by-xamarin"></a>Jenkins는 Xamarin에서 지원 되지 않는 이유는?

## <a name="jenkins-support-explanation"></a>Jenkins 지원 설명

Jenkins는 오픈 소스 CI 제품군; Jenkins로 인해 직접이 많은 문제로 인해 *자체* 문제는 코드를 가져온 위치에 대해;와 같은 정리할 수 있도록 해야 합니다는 [주 Jenkins 리포지토리](https://github.com/jenkinsci/jenkins), 또는 대 한 리포지토리 [ Jenkins.app](https://github.com/stisti/jenkins-app)합니다.

Xamarin 도구에서 특정 버그를 격리 시킬 수 있는 문제에 대해이 예외는 이것이 경우 라고 생각 되는 경우 확인할 수 있습니다 프로그램 [지원 옵션](~/cross-platform/troubleshooting/support-options.md)경우에 다시, Xamarin 지원 사항이 외부 팀 수 문제가 발생할 수 있습니다 *직접* 데 도움을 줍니다.

## <a name="setup-jenkins-with-xamarin"></a>Xamarin 사용한 설치 Jenkins

Jenkins 문제 우리 팀;에서 직접 지원 되지 않습니다 위에서 언급 한 대로 동안 [Xamarin 사용한 Jenkins를 사용 하 여](~/tools/ci/jenkins-walkthrough.md) 가이드를 사용 하 여 Xamarin와 통합 된 Jenkins CI 서버를 설정할 수 있습니다. 

## <a name="fixes-for-common-issues"></a>일반적인 문제에 대 한 수정

### <a name="jenkins-is-unable-to-find-the-android-sdk"></a>Jenkins는 Android SDK를 찾을 수 없습니다.

이 문제에 대 한 오류 메시지는 다음과 같습니다.

> 오류 XA5205: Android SDK 디렉터리에 찾을 수 없습니다. /P:AndroidSdkDirectory 통해 설정 하십시오.

SDK 위치를 설정 하기 위한 옵션; 사용 중인 정확한 Jenkins Android 플러그 인에 따라 다를 수 있습니다. 이 설정 하는 방법에 대 한 확인 하려면 먼저 플러그 인 가이드입니다. 예를 들어 [Android 에뮬레이터 플러그 인](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin#AndroidEmulatorPlugin-Systemconfiguration) 경우 하지만 SDK에 대 한 자동으로 찾습니다 찾을 수 없는, 해당 플러그 인에 대해 Jenkins 시스템 구성 페이지를 통해 위치를 설정할 수도 있습니다. 


## <a name="deprecated-errors"></a>사용 되지 않는 오류

> [!IMPORTANT]
> 최신 버전의 Xamarin에이 문제가 해결 되었습니다. 그러나 최신 버전의 소프트웨어에 문제가 발생 하면 보관는 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 정보 및 전체 빌드 로그 출력 전체 프로그램 버전 관리를 사용 합니다.



### <a name="jenkins-reports-an-invalid-xamarin-license"></a>Jenkins 잘못 된 Xamarin 라이선스를 보고합니다.
이 문제에 대 한 오류 메시지는 일반적으로 다음과 같이

> XA9008 오류: 명령줄에서 빌드하는 비즈니스 라이선스 필요

또는

> 오류: 스타터 버전의 Xamarin.iOS Xamarin Studio 외부에서 빌드를 지원 하지 않습니다. 

이 시나리오의 가장 일반적인 원인은 Xamarin 라이선스와 연결 되지 않은 사용자 계정으로 로그인 하 여 Jenkins의 사용을입니다. 이 해결 하는 가장 간단한 방법은 사용자 계정을 통해 직접 앱을 Jenkins를 설치 하는 것입니다. 해당 프로세스 및 일부 추가적인 고려 사항이 아래에서 설명 됩니다. [https://forums.xamarin.com/discussion/comment/99397/#Comment_99397](https://forums.xamarin.com/discussion/comment/99397/#Comment_99397)
