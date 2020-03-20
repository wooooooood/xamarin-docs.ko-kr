---
title: 개발용 디바이스 설정
description: 이 문서에서는 디바이스를 사용하여 Xamarin.Android 애플리케이션을 실행하고 디버깅할 수 있도록 Android 디바이스를 설정하고 컴퓨터에 연결하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 9116A3AA-EA00-56AF-AE70-BAEEC045EF11
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/22/2018
ms.openlocfilehash: 72e0a2adc79796b3df7b6fb4eca62448f1a1a7a4
ms.sourcegitcommit: 997f7b6a1a1bc50b98c3ca5bbc75d6875ba2ae9a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79510733"
---
# <a name="set-up-device-for-development"></a>개발용 디바이스 설정

_이 문서에서는 디바이스를 사용하여 Xamarin.Android 애플리케이션을 실행하고 디버깅할 수 있도록 Android 디바이스를 설정하고 컴퓨터에 연결하는 방법을 설명합니다._

Android 에뮬레이터에서 테스트한 후 Android 디바이스에서 실행 중인 앱을 보고 테스트해야 합니다. 디버깅을 사용하도록 설정하고 디바이스를 컴퓨터에 연결해야 합니다.

이러한 각 단계는 아래 섹션에서 자세히 다룹니다.

## <a name="enable-debugging-on-the-device"></a>디바이스에서 디버깅 사용

Android 애플리케이션을 테스트하려면 디바이스에서 디버깅을 사용하도록 설정해야 합니다. Android의 개발자 옵션은 버전 4.2부터 기본적으로 숨겨져 있으며, Android 버전에 따라 다양한 옵션을 사용할 수 있습니다.

### <a name="android-90"></a>Android 9.0+

Android 9.0 이상에서는 다음 단계를 수행하여 디버깅을 사용하도록 설정합니다.

1. **설정** 화면으로 이동합니다.
2. **전화 정보**를 선택합니다.
3. **이제 개발자입니다!** 가 표시될 때까지 **빌드 번호**를 7회 탭합니다.

### <a name="android-80-and-android-81"></a>Android 8.0 및 Android 8.1

1. **설정** 화면으로 이동합니다.
2. **시스템**을 선택합니다.
3. **전화 정보**를 선택합니다.
4. **이제 개발자입니다!** 가 표시될 때까지 **빌드 번호**를 7회 탭합니다.

### <a name="android-71-and-lower"></a>Android 7.1 이하

1. **설정** 화면으로 이동합니다.
2. **전화 정보**를 선택합니다.
3. **이제 개발자입니다!** 가 표시될 때까지 **빌드 번호**를 7회 탭합니다.

[![Android 9.0의 개발자 옵션 화면](set-up-device-for-development-images/build-version-sml.png)](set-up-device-for-development-images/build-version.png#lightbox)

### <a name="verify-that-usb-debugging-is-enabled"></a>USB 디버깅이 사용되는지 확인

디바이스에서 개발자 모드를 사용하도록 설정한 후 USB 디버깅을 디바이스에서 사용하도록 설정했는지 확인해야 합니다. 이는 Android 버전에 따라서도 달라집니다.

### <a name="android-90"></a>Android 9.0+

**설정 > 시스템 > 고급 > 개발자 옵션**으로 이동하여 **USB 디버깅**을 사용하도록 설정합니다.

### <a name="android-80-and-android-81"></a>Android 8.0 및 Android 8.1

**설정 > 시스템 > 개발자 옵션**으로 이동하여 **USB 디버깅**을 사용하도록 설정합니다.

### <a name="android-71-and-lower"></a>Android 7.1 이하

**설정 > 개발자 옵션**으로 이동하여 **USB 디버깅**을 사용하도록 설정합니다.

**개발자 옵션** 탭을 **설정 > 시스템** 아래에서 사용할 수 있게 되면 개발자 설정을 표시하도록 엽니다.

[![Android 9.0의 개발자 옵션 화면](set-up-device-for-development-images/usb-debugging-sml.png)](set-up-device-for-development-images/usb-debugging.png#lightbox)

USB 디버깅 등 개발자 옵션을 사용하도록 설정하고 절전 모드를 유지하는 위치입니다.

## <a name="connect-the-device-to-the-computer"></a>컴퓨터에 디바이스 연결

마지막 단계는 컴퓨터에 디바이스를 연결하는 것입니다. 가장 쉽고 안정적인 방법은 USB를 이용하는 것입니다.

디버깅에 사용한 적이 없는 경우 디바이스에서 컴퓨터를 신뢰하라는 메시지가 표시됩니다. 디바이스를 연결할 때마다 이 메시지가 표시되지 않도록 **이 컴퓨터에서 항상 허용**을 선택할 수도 있습니다.

![](set-up-device-for-development-images/trust-computer-for-usb-debugging.png "Google USB")

## <a name="alternate-connection-via-wifi"></a>Wifi를 통한 대체 연결

USB 케이블을 사용하지 않고 WiFi를 통해 컴퓨터에 Android 디바이스를 연결할 수 있습니다. 이 기술은 더 많은 노력이 필요하지만 디바이스가 컴퓨터에서 너무 멀리 떨어져 있어 케이블을 통해 지속적으로 플러그 인되어 있을 때 유용할 수 있습니다. 

### <a name="connecting-over-wifi"></a>WiFi를 통한 연결

기본적으로 *ADB*([Android Debug Bridge](https://developer.android.com/tools/help/adb.html))는 USB를 통해 Android 디바이스와 통신하도록 구성됩니다. USB 대신 TCP/IP를 사용하도록 다시 구성할 수 있습니다. 이렇게 하려면 디바이스와 컴퓨터가 모두 동일한 WiFi 네트워크에 위치해야 합니다. WiFi를 통해 디버그할 환경을 설정하려면 명령줄에서 다음 단계를 수행합니다.

1. Android 디바이스의 IP 주소를 확인합니다. IP 주소를 확인하는 한 가지 방법은 **설정 > 네트워크 및 인터넷 > Wi-Fi**를 확인한 다음 디바이스가 연결된 WiFi 네트워크를 탭하고 **고급**을 탭하는 것입니다. 그러면 아래 스크린샷에 표시된 것과 비슷하게 네트워크 연결에 대한 정보를 보여주는 드롭다운이 열립니다.

    [![IP 주소](set-up-device-for-development-images/ip-settings-sml.png)](set-up-device-for-development-images/ip-settings.png#lightbox)

    일부 버전의 Android에서는 IP 주소가 표시되지 않습니다. 하지만 대신 **설정 > 휴대폰 정보 > 상태** 아래에서 찾을 수 있습니다.

2. USB를 통해 Android 디바이스를 컴퓨터에 연결합니다.

3. 다음으로 5555 포트에서 TCP를 사용하도록 ADB를 다시 시작합니다. 명령 프롬프트에서 다음 명령을 입력합니다.

    ```command
    adb tcpip 5555
    ```

    이 명령이 실행된 후에 컴퓨터에서는 USB를 통해 연결된 디바이스를 수신할 수 없습니다.

4. 디바이스를 컴퓨터에 연결하는 USB 케이블을 분리합니다.

5. 위의 1단계에서 지정된 포트의 Android 디바이스에 연결할 수 있도록 ADB를 구성합니다.

    ```command
    adb connect 192.168.1.28:5555
    ```

    이 명령이 완료되면 Android 디바이스는 WiFi를 통해 컴퓨터에 연결됩니다.

    WiFi를 통해 디버깅을 완료하면 다음 명령을 사용하여 ADB를 USB 모드로 다시 설정할 수 있습니다.
    
    ```command
    adb usb
    ```
    
    컴퓨터에 연결되는 디바이스를 나열하도록 ADB에 요청할 수 있습니다. 디바이스가 연결되는 방식에 관계 없이 명령 프롬프트에서 다음 명령을 실행하여 연결된 항목을 확인할 수 있습니다.
    
    ```command
    adb devices
    ```

## <a name="troubleshooting"></a>문제 해결

경우에 따라 디바이스를 컴퓨터에 연결하지 못할 수도 있습니다. 이 경우 USB 드라이버가 설치되어 있는지 확인하는 것이 좋습니다.

## <a name="install-usb-drivers"></a>USB 드라이버 설치

이 단계는 macOS에 필요하지 않습니다. USB 케이블을 사용하여 디바이스를 Mac에 연결하기만 하면 됩니다.

Windows 컴퓨터에서 USB로 연결된 Android 디바이스를 인식하기 전에 몇 가지 추가 드라이버를 설치해야 할 수도 있습니다.

> [!NOTE]
> Google Nexus 디바이스를 설정하는 단계이며 참조로 제공됩니다. 특정 디바이스에 대한 단계가 다를 수 있지만 비슷한 패턴을 따릅니다. 문제가 발생하는 경우 디바이스에 대해 인터넷을 검색합니다.

**[Android SDK install path]\tools** 디렉터리에서 **android.bat** 애플리케이션을 실행합니다. 기본적으로 Xamarin.Android 설치 관리자는 Windows 컴퓨터의 다음과 같은 위치에 Android SDK를 배치합니다.

`C:\Users\[username]\AppData\Local\Android\android-sdk`

### <a name="download-the-usb-drivers"></a>USB 드라이버를 다운로드합니다.

Google Nexus 디바이스(Galaxy Nexus 제외)에는 Google USB 드라이버가 필요합니다. Galaxy Nexus용 드라이버가 [Samsung에서 배포](https://www.samsung.com/us/support/downloads/)됩니다.
다른 모든 Android 디바이스는 [해당 제조업체의 USB 드라이버](https://developer.android.com/tools/extras/oem-usb.html#Drivers)를 사용해야 합니다.

다음 스크린샷에서 볼 수 있듯이 Android SDK Manager를 시작하고 **Extras** 폴더를 확장하여 **Google USB 드라이버** 패키지를 설치합니다.

![](set-up-device-for-development-images/google-usb-driver.png "Google USB driver selected")

**Google USB 드라이버** 상자를 확인하고 **변경 내용 적용** 단추를 클릭합니다.
드라이버 파일은 다음 위치에 다운로드됩니다.

`[Android SDK install path]\extras\google\usb\_driver`

Xamarin.Android 설치의 기본 경로는 다음과 같습니다.

`C:\Users\[username]\AppData\Local\Android\android-sdk\extras\google\usb_driver`

### <a name="installing-the-usb-driver"></a>USB 드라이버 설치

USB 드라이버를 다운로드한 후에 설치해야 합니다.
Windows 7에서 드라이버를 설치하려면:

1. USB 케이블을 사용하여 컴퓨터에 디바이스를 연결합니다.

2. 바탕 화면 또는 Windows 탐색기에서 컴퓨터를 마우스 오른쪽 단추로 클릭하고 **관리**를 선택합니다.

3. 왼쪽 창에서 **디바이스**를 선택합니다.

4. 오른쪽 창에서 **기타 디바이스**를 찾아 확장합니다.

5. 디바이스 이름을 마우스 오른쪽 단추로 클릭하고 **드라이버 소프트웨어 업데이트**를 선택합니다.
    하드웨어 업데이트 마법사를 시작합니다.

6. **내 컴퓨터에서 드라이버 소프트웨어 찾아보기**를 선택하고 **다음**을 클릭합니다.

7. **찾아보기**를 클릭하고 USB 드라이버 폴더를 찾습니다. Google USB 드라이버는 **[Android SDK 설치 경로]\extras\google\usb_driver**에 위치합니다.

8. **다음**을 클릭하여 드라이버를 설치합니다.

## <a name="summary"></a>요약

이 문서는 디바이스에서 디버깅을 사용하여 개발할 Android 디바이스를 구성하는 방법을 설명합니다. USB 또는 WiFi 중 하나를 사용하여 컴퓨터에 디바이스를 연결하는 방법을 설명합니다.

## <a name="related-links"></a>관련 링크

- [Android Debug Bridge](https://developer.android.com/tools/help/adb.html)
- [하드웨어 디바이스 사용](https://developer.android.com/tools/device.html)
- [Samsung 드라이버 다운로드](https://www.samsung.com/us/support/downloads/)
- [OEM USB 드라이버](https://developer.android.com/tools/extras/oem-usb.html#Drivers)
- [Google USB 드라이버](https://developer.android.com/sdk/win-usb.html)
- [XDA 개발자: Windows 8 - ADB/빠른 부팅 드라이버 문제 해결](https://forum.xda-developers.com/showthread.php?t=1583801)
