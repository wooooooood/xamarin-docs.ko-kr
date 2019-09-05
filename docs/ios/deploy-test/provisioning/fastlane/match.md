---
title: iOS용 fastlane – match
description: 이 문서에서는 iOS 개발에 대한 코드 서명 인증서 및 프로비저닝 프로필을 만들고 유지 관리하는 데 사용되는 fastlane의 일치 명령을 설명합니다.
ms.prod: xamarin
ms.assetid: C4A2A67E-0643-4CED-B1A9-79D65054F3CA
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: 8991ddc55069fad8c5f023f35ece0926f0f7e5b8
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70285723"
---
# <a name="fastlane-for-ios---match"></a>iOS용 fastlane – match

일반적으로 디바이스 프로비전은 Xcode를 통해 또는 Apple Developer 포털에서 개발 팀의 각 멤버가 수행하며 다음과 같은 몇 가지 단계로 구성됩니다.

- 개발 인증서 요청
- 포털에 디바이스 추가
- 앱 ID 만들기
- 프로비전 프로필 만들기
- 프로필 및 인증서 다운로드

수동으로 또는 Xcode를 통해 개발용 디바이스를 설정하는 데 필요한 단계에 대한 자세한 내용은 [디바이스 프로비저닝](~/ios/get-started/installation/device-provisioning/index.md) 가이드를 참조하세요.

이 가이드에서는 Xcode를 사용하는 대신 fastlane 도구를 사용하는 방법을 소개합니다.

## <a name="installation"></a>설치

fastlane 설치 및 업데이트에 대한 자세한 내용은 [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation) 가이드 소개를 참조하세요.

<a name="whatismatch" />

## <a name="what-is-match"></a>match란?

match는 코드 서명 인증서와 프로비전 프로필을 만들고 유지 관리하여 iOS 개발 팀의 모든 개발자가 하나의 코드 서명 ID를 공유할 수 있도록 합니다.

App Store에 앱을 배포하거나 베타 테스트를 수행하거나 디바이스에 앱을 설치할 때 개발 팀의 각 멤버에게 고유한 서명 ID가 있습니다. 이로 인해 ID 및 프로필이 충돌할 수 있고 따라서 프로필 및 앱 ID를 수동으로 생성, 순환 및 관리해야 할 수 있습니다.

대신 match는 모든 인증서와 프로필을 생성하고 유지 관리하며 개인 Git 리포지토리에 저장합니다. 이렇게 하면 팀의 모든 개발자가 자격 증명에 액세스하여 사용할 수 있습니다. 또한, 인증서를 개인 Git 리포지토리에 저장하는 것에도 인증서에 추가 보안을 제공하며, [암호](#passphrase)를 사용하여 암호화합니다. 리포지토리에 코드 서명 아티팩트를 저장하면 팀 에이전트와 관리자가 필요에 따라 인증서를 업데이트하고 순환할 수 있기 때문에 각 개발자에게 새 인증서를 배포하는 데 걸리는 시간이 줄어듭니다.

> [!IMPORTANT]
> match는 현재 In-House Enterprise(사내 엔터프라이즈) 프로필을 지원하지 않습니다.

<a name="initializing" />

## <a name="initializing-your-project-with-match"></a>match로 프로젝트 초기화

팀 관리자는 github.com 또는 bitbucket.com을 통해 개인 Git 리포지토리를 만들어서 모든 팀 멤버를 리포지토리에 contributor로 추가합니다.

터미널을 사용하여 디렉터리를 프로젝트 디렉터리로 변경하고 다음을 실행합니다.

```
fastlane match init
```

메시지가 표시되면 Git 리포지토리의 URL을 입력합니다.

 [![](match-images/fastlane-image7.png "Git 리포지토리의 URL 입력")](match-images/fastlane-image7.png#lightbox)

URL은 아래 그림과 같이 github.com에서 찾아서 **Clone or Download**(복제 또는 다운로드) 단추를 클릭하여 복사할 수 있습니다.

[![](match-images/fastlane-image6.png "github.com에서 복제 또는 다운로드 단추의 URL")](match-images/fastlane-image6.png#lightbox)

프로젝트를 초기화하면 matchfile(match 도구에 환경 변수를 전달하도록 편집할 수 있는 텍스트 파일)이 생성됩니다. matchfile 예제는 다음과 같습니다.

[![](match-images/fastlane-image8.png "matchfile 예제")](match-images/fastlane-image8.png#lightbox)

<a name="running" />

## <a name="running-match"></a>match 실행

> [!NOTE]
> match를 처음으로 실행하기 전에 [match nuke 명령](#using)을 사용하여 기존 프로필과 인증서를 지우는 것이 좋습니다.

필요한 환경이 무엇인지에 따라 다음 명령 중 하나를 사용하여 새 인증서와 프로비전 프로필을 생성하고 새로운 Git 리포지토리에 저장할 수 있습니다.

```
fastlane match appstore

fastlane match adhoc

fastlane match development
```

새 인증서와 프로필을 생성하는 것 외에 위의 명령을 사용하면 Git 리포지토리에 다음 항목이 추가됩니다.

- certificates 폴더
- profiles 폴더
- 기본 지침이 포함된 readme
- match version

[![](match-images/fastlane-image9.png "Git 리포지토리의 프로젝트 구조")](match-images/fastlane-image9.png#lightbox)

프로비전 프로필은 `~/Library/MobileDevice/Provisioning Profiles`에 설치됩니다. 인증서와 프라이빗 키는 키 집합에 바로 설치됩니다.

<a name="using" />

### <a name="using-the-nuke-command"></a>`nuke` 명령 사용

깔끔하지 않은 인증서가 있으면 다음 명령으로 `nuke`를 사용하여 각 환경에 대한 인증서와 프로필을 해지할 수 있습니다.

```
fastlane match nuke
```

특정 환경에 대한 모든 인증서 및 프로비전 프로필을 해지하려면:

```
fastlane match nuke development
```

 또는

```
fastlane match nuke distribution
```

fastlane은 삭제하기 전에 제거할 파일을 확인합니다.

<a name="passphrase" />

### <a name="passphrase"></a>암호

`match`를 처음 실행하면 Git 리포지토리에 대한 암호를 설정하라는 메시지가 나타납니다. 다른 컴퓨터에서 match를 실행할 때 필요하므로 암호를 기억해 두어야 합니다. 이것은 추가 보안 계층입니다. 각 파일은 OpenSSL을 사용하여 암호화됩니다. 새 컴퓨터에서 `match`를 실행할 때 마다 이 암호를 묻는 메시지가 표시됩니다. 암호를 처음 입력하면 로컬 키 집합에 암호가 추가됩니다.

환경 변수를 사용하여 프로필 암호를 해독할 암호를 설정하려면 `MATCH_PASSWORD`를 사용합니다.

<a name="options" />

## <a name="additional-options"></a>추가 옵션

다음 옵션을 사용하면 match를 사용할 때 추가 지원을 제공할 수 있습니다.

- 사용 가능한 모든 명령의 목록을 보려면 `-–help` 플래그를 사용합니다.

    ```
    fastlane match cert --help
    ```

- 출력의 자세한 정도를 높이려면 `-–verbose` 플래그를 사용합니다.

    ```
    fastlane match --development --verbose
    ```

- 개발자 포털의 디바이스 수가 변경된 경우 프로비전 프로필에 갱신을 적용하려면 `--force_for_new_devices` 플래그를 사용합니다.

    ```
    fastlane match development --force_for_new_devices
    ```

## <a name="related-links"></a>관련 링크

- [fastlane - match](https://github.com/fastlane/fastlane/blob/master/match/README.md)
