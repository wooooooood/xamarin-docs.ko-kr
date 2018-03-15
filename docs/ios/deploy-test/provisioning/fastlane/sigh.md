---
title: "iOS용 fastlane – sigh"
ms.topic: article
ms.prod: xamarin
ms.assetid: CD17276F-2C8C-4A46-A54C-DD532EBD5720
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 2c6ac298ca2040bb2d3619be080fb1387fbfd3a0
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="fastlane-for-ios--sigh"></a>iOS용 fastlane – sigh

> [!IMPORTANT]
> fastlane에서는 프로비전 프로필 생성 및 유지 관리를 위해 [`match`](~/ios/deploy-test/provisioning/fastlane/match.md)를 사용하는 것이 좋습니다. 코드 서명에 대해 알고 있고 완전한 제어가 필요한 경우에만 sigh를 직접 사용합니다.

## <a name="overview"></a>개요

일반적으로 장치 프로비전은 Xcode를 통해 또는 Apple Developer 포털에서 개발 팀의 각 멤버가 수행하며 다음과 같은 몇 가지 단계로 구성됩니다.

- 개발 인증서 요청
- 포털에 장치 추가
- 앱 ID 만들기
- 프로비전 프로필 만들기
- 프로필 및 인증서 다운로드

간 단계에는 개발하는 응용 프로그램 유형에 따라 처리해야 하는 변수가 있습니다. 수동으로 또는 Xcode를 통해 개발용 장치를 설정하는 데 필요한 단계에 대한 자세한 내용은 [장치 프로비저닝](~/ios/get-started/installation/device-provisioning/index.md) 가이드를 참조하세요.

이 가이드에서는 Xcode를 사용하는 대신 fastlane 도구를 소개하고 다음 내용을 설명합니다.

- [sigh란?](#whatissigh)
- [앱 ID 만들기](#appid)
- [새 장치 추가](#newdevices)
- [sigh 사용](#using)
- [추가 옵션](#options)

> [!NOTE]
> 앱의 번들 ID와 일치하는 기존 앱 ID가 있고 개발자 포털에 장치가 있는 경우 [앱 ID 만들기](#appid) 및 [장치 추가](#newdevices)의 단계를 무시할 수 있습니다. 이 경우 [sigh 사용](#using)으로 바로 이동하여 시작합니다.

## <a name="installation"></a>설치

fastlane 설치 및 업데이트에 대한 자세한 내용은 [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation) 가이드 소개를 참조하세요.

<a name="whatissigh" />

## <a name="what-is-sigh"></a>sigh란?

sigh는 개발, 앱 스토어 배포, 임시 배포 및 엔터프라이즈 배포와 같은 모든 구성에 대한 프로비전 프로필을 생성하고 갱신할 수 있는 터미널 인터페이스를 제공합니다. 또한 프로비전 프로필을 다운로드하고 복구하는 간단한 방법을 제공합니다.

<a name="appid" />

## <a name="creating-an-app-id"></a>앱 ID 만들기

앱 ID는 다음 명령으로 만들 수 있습니다.

    fastlane produce -u your@appleid.com -a com.company.appname --skip_itc

`com.company.appname`은 앱의 번들 ID이며 아래 그림과 같이 Xamarin.iOS 응용 프로그램의 Info.plist 파일에서 찾을 수 있습니다.

[![](sigh-images/fastlane-image5.png "Xamarin.iOS 응용 프로그램의 Info.plist 파일")](sigh-images/fastlane-image5.png#lightbox)

고유한 앱 ID는 역방향 DNS 스타일 문자열이어야 합니다. ID를 만든 후에는 ID를 메모해 두었다가 이 가이드의 뒷부분 나오는 sigh를 사용할 때 사용해야 합니다.

[iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)에 앱을 만들어야 하는 경우에는 위의 명령에서 `--skip_itc` 플래그를 제거합니다.

<a name="newdevices" />

## <a name="adding-new-devices"></a>새 장치 추가

명령줄에서 개발자 포털에 단일 장치를 추가하려면 다음 명령을 입력합니다.

```bash
fastlane run register_device name:"Adam iPhone" udid:"abcdeg1234567"
```

둘 이상의 장치를 추가하려면 `register_devices` 명령을 사용합니다.

```bash
    register_devices(
        devices: {
            "iPhone 6" => "1234567890123456789012345678901234567890",
            "iPad Air 2" => "abcdefghijklmnopqrstvuwxyzabcdefghijklmn"
         }
    )
```

<a name="using" />

## <a name="using-sigh"></a>sigh 사용

sigh 유틸리티를 사용하려면 터미널에 다음 명령을 입력합니다.

```bash
fastlane sigh
```

기본적으로 [앱 스토어 배포](~/ios/deploy-test/app-distribution/app-store-distribution/index.md) 프로비전 프로필이 생성됩니다. 개발용 장치를 설정하려면 `--development`플래그를 전달합니다.

```bash
fastlane sigh --development
```

fastlane 프롬프트가 표시되면 Apple ID 사용자 이름을 입력합니다. fastlane을 처음 사용하는 경우 암호를 묻는 메시지가 표시될 수도 있습니다. 그렇지 않으면 키 집합에서 암호 환경 변수를 끌어옵니다.

Apple ID가 여러 팀에 연결되어 있으면 여기에 표시됩니다. 사용할 팀에 해당하는 번호를 선택합니다.

[![](sigh-images/fastlane-image2.png "사용하려는 팀 선택")](sigh-images/fastlane-image2.png#lightbox)

팀 ID는 다음과 같은 방법으로 CLI에 전달될 수도 있습니다.

```bash
fastlane sigh -l 2TU993NY9J
```

앱의 [앱 ID](#appid)를 입력합니다. 이 ID는 앱의 Info.plist에 있는 번들 식별자와 일치해야 합니다.

계정에 연결된 모든 장치는 프로비전 프로필에 추가됩니다.

그러면 fastlane에서 프로비전 프로필이 생성, 다운로드 및 설치됩니다.

개발자 센터를 탐색하면 아래 그림과 같이 새로 만든 프로비전 프로필을 볼 수 있습니다.

[![](sigh-images/fastlane-image10.png "새로 만든 프로비전 프로필 보기")](sigh-images/fastlane-image10.png#lightbox)

sigh는 기본적으로 현재 폴더에 프로비전 프로필을 저장합니다. 출력 디렉터리를 변경하려면 `output_path`를 편집하거나 다음을 수행합니다.

```bash
fastlane sigh -o "~/Library/MobileDevice/Provisioning Profiles"
```

<a name="options" />

## <a name="sigh-additional-options"></a>sigh 추가 옵션

다음 옵션을 사용하면 sigh를 사용할 때 추가 지원을 제공할 수 있습니다.

- 모든 프로비전 프로필을 다운로드하려면 다음을 사용합니다.

    ```bash
    fastlane sigh download_all
    ```

- 프로비전 프로필에 특정 서명 ID를 사용하려면 다음을 사용합니다.

    ```bash
    fastlane sigh -c "Amy cert"
    ```
    
    여기서 `Amy cert`는 코드 서명 ID 이름입니다.


## <a name="related-links"></a>관련 링크

- [fastlane - sigh](https://github.com/fastlane/fastlane/tree/master/sigh#readme)
