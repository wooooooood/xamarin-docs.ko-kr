---
title: Microsoft에서 Jenkins을 지원 하지 않는 이유는 무엇 인가요?
description: 이 문서에서는 높은 수준에서 Xamarin의 Jenkins CI 시스템과의 상호 작용을 설명 합니다. Jenkins를 사용할 때 발생 하는 몇 가지 일반적인 문제에 대해서도 설명 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9951F980-2C6C-47C0-8A35-A78F06C20BEB
author: conceptdev
ms.author: crdun
ms.date: 06/05/2018
ms.openlocfilehash: a8dc27574dc9959cc375a98fc0d7a18aac8bd6b7
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70756964"
---
# <a name="why-isnt-jenkins-supported-by-microsoft"></a>Microsoft에서 Jenkins을 지원 하지 않는 이유는 무엇 인가요?

## <a name="jenkins-support-explanation"></a>Jenkins 지원 설명

Jenkins은 오픈 소스 CI 도구 모음입니다. 이러한 문제 때문에 Jenkins *자체* 에서 직접 발생 하는 많은 문제를 코드를 받은 위치에 대 한 문제로 정리 해야 합니다. [주 Jenkins 리포지토리](https://github.com/jenkinsci/jenkins), [Jenkins](https://github.com/stisti/jenkins-app)에 대 한 리포지토리 등이 있습니다.

이에 대 한 예외는 Xamarin 도구의 특정 버그로 격리할 수 있는 문제에 대 한 것입니다. [지원 옵션](~/cross-platform/troubleshooting/support-options.md)을 확인할 수 있다고 생각 되는 경우,이 문제는 Xamarin 지원 팀에서 *직접* 도울 수 있는 것 외에도 문제가 될 수 있습니다.

## <a name="setup-jenkins-with-xamarin"></a>Xamarin을 사용 하 여 Jenkins 설정

위에서 언급 한 것 처럼 Jenkins 문제는 팀에서 직접 지원 되지 않습니다. xamarin [과 함께 Jenkins 사용](~/tools/ci/jenkins-walkthrough.md) 가이드를 사용 하 여 xamarin과 통합 된 Jenkins CI 서버를 설정할 수 있습니다. 

## <a name="fixes-for-common-issues"></a>일반적인 문제에 대 한 수정

### <a name="jenkins-is-unable-to-find-the-android-sdk"></a>Jenkins에서 Android SDK를 찾을 수 없습니다.

이 문제에 대 한 오류 메시지는 다음과 같습니다.

> 오류 XA5205: Android SDK 디렉터리를 찾을 수 없습니다. /P: AndroidSdkDirectory을 통해 설정 하세요.

SDK 위치를 설정 하는 옵션은 사용 중인 정확한 Jenkins Android 플러그 인에 따라 달라질 수 있습니다. 이를 설정 하는 방법에 대 한 좋은 위치는 플러그 인 가이드에 있습니다. 예를 들면 [Android Emulator 플러그 인](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin#AndroidEmulatorPlugin-Systemconfiguration) 은 SDK를 자동으로 검색 하지만 찾을 수 없는 경우 해당 플러그 인에 대 한 Jenkins 시스템 구성 페이지를 통해 위치를 설정할 수도 있습니다. 

## <a name="deprecated-errors"></a>사용 되지 않는 오류

> [!IMPORTANT]
> 이 문제는 최신 버전의 Xamarin에서 해결 되었습니다. 그러나 최신 버전의 소프트웨어에서 문제가 발생 하는 경우 전체 버전 정보 및 전체 빌드 로그 출력을 사용 하 여 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 를 작성 하세요.

### <a name="jenkins-reports-an-invalid-xamarin-license"></a>Jenkins에서 잘못 된 Xamarin 라이선스를 보고 합니다.
이 문제에 대 한 오류 메시지는 일반적으로 다음과 같습니다.

> XA9008 오류: 명령줄에서 빌드하려면 비즈니스 라이선스가 필요 합니다.

로 구분하거나 여러

> 오류: Xamarin.ios의 스타터 버전은 Xamarin Studio 외부에서의 빌드를 지원 하지 않습니다. 

이 시나리오의 가장 일반적인 원인은 Xamarin 라이선스와 연결 되지 않은 사용자 계정으로 로그인 하 여 Jenkins를 사용 하는 것입니다. 이 문제를 해결 하는 가장 간단한 방법은 사용자 계정을 통해 직접 Jenkins를 앱으로 설치 하는 것입니다. 해당 프로세스 및 몇 가지 추가 고려 사항은 다음에 설명 되어 있습니다.[https://forums.xamarin.com/discussion/comment/99397/#Comment_99397](https://forums.xamarin.com/discussion/comment/99397/#Comment_99397)
