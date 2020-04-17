---
title: 독립적으로 게시
ms.prod: xamarin
ms.assetid: 6FB4DEF2-01AD-C5FE-0950-CE1BF088A9C6
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: dee49d556173ec0b9544820f129eef8a9cce404f
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73021316"
---
# <a name="publishing-independently"></a>독립적으로 게시

기존 Android 마켓플레이스를 사용하지 않고 애플리케이션을 게시할 수 있습니다. 이 섹션에서는 이러한 기타 게시 방법과 Xamarin.Android의 라이선스 수준을 설명합니다.

## <a name="xamarin-licensing"></a>Xamarin 라이선스

Xamarin.Android 앱의 개발, 배포 및 보급에 많은 라이선스를 사용할 수 있습니다.

- **Visual Studio Community** &ndash; Windows를 사용하는 학생, 소규모 팀, OSS 개발자용

- **Visual Studio Professional** &ndash; 개인 개발자 또는 소규모 팀용(Windows만 해당). 이 라이선스는 표준 또는 클라우드 구독을 제공하며 사용 제한이 없습니다.

- **Visual Studio Enterprise** &ndash; 모든 규모의 팀용(Windows만 해당). 이 라이선스에는 엔터프라이즈 기능, 표준 또는 클라우드 구독이 포함됩니다.

Community Edition을 다운로드하거나 Professional 및 Enterprise Edition 구매에 대한 자세한 내용을 알아보려면 [visualstudio.com](https://visualstudio.microsoft.com/xamarin/)을 방문하세요.

## <a name="allow-installation-from-unknown-sources"></a>알 수 없는 원본에서의 설치 허용

기본적으로 Android는 사용자가 Google Play 이외의 위치에서 애플리케이션을 다운로드하여 설치하는 것을 차단합니다. 마켓플레이스 이외 원본에서의 설치를 허용하려면 사용자가 애플리케이션을 설치하기 전에 먼저 디바이스에서 *알 수 없는 원본* 설정을 사용하도록 설정해야 합니다. 이에 대한 설정은 다음 그림처럼 **설정 > 보안**에 있습니다.

[![보안 설정 화면](publishing-independently-images/settings.png)](publishing-independently-images/settings.png#lightbox)

> [!IMPORTANT]
> 일부 네트워크 공급자는 이 설정과 관계없이 알 수 없는 원본의 애플리케이션 설치를 차단할 수 있습니다.

## <a name="publishing-by-e-mail"></a>이메일로 게시

이메일에 릴리스 APK를 첨부하면 애플리케이션을 쉽고 빠르게 사용자에게 배포할 수 있습니다. Android 지원 디바이스에서 사용자가 이메일을 열면 다음 이미지에서처럼 Android가 APK 첨부 파일을 인식하고 **설치** 단추를 표시합니다.

[![첨부 파일에 대한 설치 단추](publishing-independently-images/publishing-via-email.png)](publishing-independently-images/publishing-via-email.png#lightbox)

이메일을 통한 배포는 간단하지만 개인 정보나 무단 배포에 대한 보호가 부족합니다. 애플리케이션의 받는 사람이 극소수이며 해당 애플리케이션을 배포하지 않는다고 확신하는 경우에만 이 방법이 적합합니다.

## <a name="publishing-by-web"></a>웹으로 게시

웹 서버를 통해 애플리케이션을 배포할 수 있습니다. 이 작업은 웹 서버에 애플리케이션을 업로드한 다음 사용자에게 다운로드 링크를 제공하여 수행합니다. Android 지원 디바이스가 이 링크로 이동하면 애플리케이션을 다운로드하면 다운로드 완료 후 애플리케이션이 자동으로 설치됩니다.

## <a name="manually-installing-an-apk"></a>수동으로 APK 설치

수동 설치는 세 번째 애플리케이션 설치 옵션입니다. 애플리케이션 수동 설치를 적용하려면

1. **APK 사본을 사용자에게 배포** &ndash; 예를 들어 이 사본은 CD나 USB 플래스 드라이브에 배포할 수 있습니다.
1. **(사용자) Android 디바이스에 애플리케이션 설치** &ndash; 명령줄 *Android Debug Bridge*(**adb**) 도구를 사용합니다. **adb**는 에뮬레이터 인스턴스 또는 Android 지원 디바이스와의 커뮤니케이션을 구현하는 범용 명령줄 도구입니다. Android SDK는 **adb**를 포함하며 **\<sdk>/platform-tools/** 디렉터리에 있습니다.

Android 디바이스를 컴퓨터에 USB 케이블로 연결해야 합니다.
Windows 컴퓨터도 **adb**에서 인식하기 위해 전화 공급업체가 제공하는 추가 USB 드라이버가 필요할 수 있습니다. 이러한 추가 USB 드라이버 설치 지침은 이 문서에 해당하지 않습니다.

**adb** 명령을 실행하기 전에 해당하는 경우 어떤 에뮬레이터 인스턴스나 디바이스가 연결되었는지 알고 있으면 유용합니다. 다음 코드 조각에서처럼 `devices` 명령을 사용하여 연결된 장치 목록을 확인할 수 있습니다.

```shell
$ adb devices
List of devices attached
        0149B2EC03012005device
```

연결된 디바이스를 확인한 후 `install` 명령을 **adb**와 함께 실행하여 애플리케이션을 실행할 수 있습니다.

```shell
$ adb install <path-to-apk>
```

다음 코드 조각은 연결된 디바이스에 애플리케이션을 설치하는 예제를 보여 줍니다.

```shell
$ adb install helloworld.apk
3772 KB/s (3013594 bytes in 0.780s)
        pkg: /data/local/tmp/helloworld.apk
Success
```

애플리케이션이 이미 설치되었다면 `adb install`가 APK를 설치할 수 없고 다음 에제에서처럼 오류를 보고합니다.

```shell
$ adb install helloworld.apk
4037 KB/s (3013594 bytes in 0.728s)
        pkg: /data/local/tmp/helloworld.apk
Failure [INSTALL_FAILED_ALREADY_EXISTS]
```

디바이스에서 애플리케이션을 제거해야 합니다. 먼저 `adb uninstall` 명령을 실행합니다.

```shell
adb uninstall <package_name>
```

다음 코드 조각은 애플리케이션을 제거의 예제입니다.

```shell
$ adb uninstall mono.samples.helloworld
Success
```
