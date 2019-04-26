---
title: Jenkins는 Microsoft에서 지원 되지 않는 이유는?
description: 이 문서는 Jenkins CI 시스템과 상호 작용 Xamarin의 높은 수준에서 설명합니다. 또한 Jenkins를 사용 하 여 작업할 때 발생 하는 몇 가지 일반적인 문제에 대해서도 설명 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9951F980-2C6C-47C0-8A35-A78F06C20BEB
author: asb3993
ms.author: amburns
ms.date: 06/05/2018
ms.openlocfilehash: c2e409b796d5ef2525079e02aafdd0c6e8db5d81
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61158938"
---
# <a name="why-isnt-jenkins-supported-by-microsoft"></a>Jenkins는 Microsoft에서 지원 되지 않는 이유는?

## <a name="jenkins-support-explanation"></a>Jenkins 지원 설명

Jenkins는 오픈 소스 CI 제품군; Jenkins에서 직접 발생 하는 많은 문제로 인해 *자체* 코드를 가져온 위치에 대해;와 같은 문제를 정리할 수 있도록 해야 합니다는 [기본 Jenkins 리포지토리의](https://github.com/jenkinsci/jenkins), 또는 대 한 리포지토리 [ Jenkins.app](https://github.com/stisti/jenkins-app)합니다.

이 예외는 Xamarin의 도구에서 특정 버그를 격리 시킬 수 있는 문제에 대해 경우 생각 하는 경우 확인할 수 있습니다 하 [옵션을 지원](~/cross-platform/troubleshooting/support-options.md)되지만 다시 팀 수 Xamarin 지원과 외부 무언가 문제가 발생할 수 있습니다 *직접* 도움.

## <a name="setup-jenkins-with-xamarin"></a>Xamarin 사용 하 여 Jenkins를 설치 합니다.

Jenkins 문제; 팀에서 직접 지원 되지 않습니다 위에서 언급 했 듯이 합니다 [Xamarin 사용 하 여 Jenkins를 사용 하 여](~/tools/ci/jenkins-walkthrough.md) 가이드를 사용 하 여 Xamarin을 사용 하 여 통합 된 Jenkins CI 서버를 설정 수 있습니다. 

## <a name="fixes-for-common-issues"></a>일반적인 문제에 대 한 수정

### <a name="jenkins-is-unable-to-find-the-android-sdk"></a>Jenkins를 Android SDK를 찾을 수 없는 경우

이 문제에 대 한 오류 메시지는 다음과 같습니다.

> error XA5205: Android SDK 디렉터리를 찾을 수 없습니다. /P:AndroidSdkDirectory 통해 설정 하세요.

SDK 위치를 설정 하기 위한 옵션; 사용 하는 정확한 Jenkins Android 플러그 인에 따라 달라질 수 있습니다. 이 설정 하는 방법에 대 한 확인 하는 것이 좋습니다 플러그 인 가이드입니다. 예를 들어, 합니다 [Android 에뮬레이터 플러그 인](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin#AndroidEmulatorPlugin-Systemconfiguration) 이루어지지만 SDK를 자동으로 찾습니다 찾을 수 없는 것; 위치는 플러그 인에 대해 Jenkins 시스템 구성 페이지를 통해 설정할 수도 있습니다. 


## <a name="deprecated-errors"></a>사용 되지 않는 오류

> [!IMPORTANT]
> Xamarin의 최신 버전에서이 문제가 해결 되었습니다. 그러나 소프트웨어의 최신 버전에 문제가 발생 하면 제출 하세요를 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 정보 및 전체 빌드 로그 출력 전체 버전을 사용 하 여 합니다.



### <a name="jenkins-reports-an-invalid-xamarin-license"></a>Jenkins는 잘못 된 Xamarin 라이선스를 보고합니다.
이 문제에 대 한 오류 메시지는 일반적으로 같은

> XA9008 오류: 명령줄에서 빌드하는 비즈니스 라이선스가 필요

또는

> 오류: 시작 버전의 Xamarin.iOS Xamarin Studio 외부에서 빌드를 지원 하지 않습니다. 

이 시나리오의 가장 일반적인 원인은 Xamarin 라이선스를 사용 하 여 연결 되지 않은 사용자 계정으로 로그인 하 여 Jenkins 사용 됩니다. 이 확인 하는 가장 간단한 방법은 Jenkins 사용자 계정을 통해 직접 앱을 설치 합니다. 일부 추가 고려 사항과 해당 프로세스는 다음과 같습니다. [https://forums.xamarin.com/discussion/comment/99397/#Comment_99397](https://forums.xamarin.com/discussion/comment/99397/#Comment_99397)
