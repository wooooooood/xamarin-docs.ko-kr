---
title: iOS용 fastlane – cert
description: 이 문서에서는 iOS 응용 프로그램 프로비저닝 프로세스, 인증서 요청, Apple의 개발자 포털에 장치 추가, 앱 ID 만들기 등의 많은 부분을 자동화하는 도구인 fastlane을 설명합니다.
ms.prod: xamarin
ms.assetid: 900FA6FF-F3C9-4D35-993E-B0D88E6B1883
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: fac90f4738046f91183a1acd080a0d0e480c6cbf
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785199"
---
# <a name="fastlane-for-ios--cert"></a>iOS용 fastlane – cert

> [!IMPORTANT]
> fastlane에서는 인증서 생성 및 유지 관리를 위해 [`match`](~/ios/deploy-test/provisioning/fastlane/match.md) 도구를 사용하는 것이 좋습니다. 코드 서명에 대해 알고 있고 완전한 제어가 필요한 경우에만 `cert`를 직접 사용합니다. 이 작업을 사용하여 최신 코드 서명 ID를 다운로드합니다.

## <a name="overview"></a>개요

일반적으로 장치 프로비전은 Xcode를 통해 또는 Apple Developer 포털에서 개발 팀의 각 멤버가 수행하며 다음과 같은 몇 가지 단계로 구성됩니다.

- 개발 인증서 요청
- 포털에 장치 추가
- 앱 ID 만들기
- 프로비전 프로필 만들기
- 프로필 및 인증서 다운로드

간 단계에는 개발하는 응용 프로그램 유형에 따라 처리해야 하는 변수가 있습니다. 수동으로 또는 Xcode를 통해 개발용 장치를 설정하는 데 필요한 단계에 대한 자세한 내용은 [장치 프로비저닝](~/ios/get-started/installation/device-provisioning/index.md) 가이드를 참조하세요.

이 가이드에서는 Xcode를 사용하는 대신 fastlane 도구를 소개하고 다음 내용을 설명합니다.

- [cert란?](#whatiscert)
- [cert 사용](#using)
- [추가 옵션](#options)

## <a name="installation"></a>설치

fastlane 설치 및 업데이트에 대한 자세한 내용은 [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation) 가이드에 대한 소개를 참조하세요.

<a name="whatiscert" />

## <a name="what-is-cert"></a>cert란?

cert는 개발 및 배포 환경 모두에 새로운 코드 서명 ID(개발자 _인증서_라고도 함)를 만드는 터미널 인터페이스를 제공합니다.

<a name="using" />

## <a name="using-cert"></a>cert 사용

cert 유틸리티를 사용하려면 터미널 CLI에 다음 명령을 입력합니다.

    fastlane cert

기본적으로 배포 인증서가 만들어집니다. 개발 인증서를 만들려면 `--development` 플래그를 전달합니다.

    fastlane cert --development

cert에 Apple ID와 암호를 묻는 메시지가 표시되면 입력합니다.

[![](cert-images/fastlane-image1.png "cert에 Apple ID와 암호를 묻는 메시지 표시")](cert-images/fastlane-image1.png#lightbox)

> [!IMPORTANT]
> 암호를 처음 입력하면 로컬 macOS 키 집합에 저장됩니다. 또는 환경 변수를 사용하여 사용자 이름과 암호를 저장하거나 키 집합에 암호를 저장하지 않으려는 경우에는 `export fastlane_DONT_STORE_PASSWORD=1`을 사용할 수 있습니다. fastlane으로 자격 증명을 관리하는 방법에 대한 자세한 내용은 fastlane의 [자격 증명 관리자 가이드](https://github.com/fastlane/fastlane/blob/master/credentials_manager/README.md)를 참조하세요.

Apple ID는 다음 명령을 사용하여 인수로 전달할 수도 있습니다.

    fastlane cert -u myemailadress@domain.com

Apple ID가 여러 팀에 연결되어 있으면 여기에 표시됩니다. 사용할 팀에 해당하는 번호를 선택합니다.

[![](cert-images/fastlane-image2.png "사용하려는 팀 선택")](cert-images/fastlane-image2.png#lightbox)

팀 ID는 다음 플래그를 사용하여 전달할 수도 있습니다.

    fastlane cert -l 2TU993NY9J

fastlane은 사용 가능한 서명 인증서가 로컬 컴퓨터에 설치되어 있는지 확인하고, 있으면 그것을 사용합니다.

서명 인증서가 없으면 cert는 다음을 수행합니다.

- 새 개인 키 및 서명 요청 만들기
- 인증서 생성, 다운로드 및 설치
- 인증서 및 개인 키를 키 집합으로 가져오기

계정에 허용된 최대 서명 ID 수에 도달하면 예외가 발생합니다. 새 서명 ID를 만들려면 개발자 센터를 통해 기존 인증서 중 하나를 수동으로 해지하고 다시 시도해야 합니다.

> [!NOTE]
> fastlane은 서명 ID가 키 집합에 없는 경우 개발자 센터에서 기존 서명 ID를 다운로드 할 수 없습니다. 개인 키는 컴퓨터 또는 내보낸(*.p12) 인증서 버전에만 존재하며 개발자 센터에는 존재하지 않기 때문입니다.

<a name="options" />

## <a name="additional-options"></a>추가 옵션

다음 옵션을 사용하면 cert를 사용할 때 추가 지원을 제공할 수 있습니다.

- 사용 가능한 모든 명령의 목록을 보려면 `-–help` 플래그를 사용합니다.

        fastlane cert --help

- 출력의 자세한 정도를 높이려면 `-–verbose` 플래그를 사용합니다.

        fastlane cert --development --verbose


## <a name="related-links"></a>관련 링크

- [fastlane – cert](https://github.com/fastlane/fastlane/blob/master/cert/README.md)
