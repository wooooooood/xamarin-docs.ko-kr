---
title: "개발용 장치 설정"
description: "이 문서에서는 장치를 사용하여 Xamarin.Android 응용 프로그램을 실행하고 디버깅할 수 있도록 Android 장치를 설정하고 컴퓨터에 연결하는 방법을 설명합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9116A3AA-EA00-56AF-AE70-BAEEC045EF11
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/21/2017
ms.openlocfilehash: 64036af82ea49ad4d758a89767ff0da02eef094f
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="set-up-device-for-development"></a>개발용 장치 설정

_이 아티클에서는 장치를 사용하여 Xamarin.Android 응용 프로그램을 실행하고 디버깅할 수 있도록 Android 장치를 설정하고 컴퓨터에 연결하는 방법을 설명합니다._

지금까지 Android 에뮬레이터에서 실행되는 새 응용 프로그램을 확인했으며, Android 장치에서 실행되는 것을 확인하려고 합니다. 디버깅을 위해 컴퓨터에 장치를 연결하는 것과 관련된 단계는 다음과 같습니다.

1.  **장치에서 디버깅 사용** - 기본적으로 Android 장치에서 응용 프로그램을 디버깅할 수 없습니다.

2.  **USB 드라이버 설치** - 이 단계는 OS X 컴퓨터에서 필요하지 않습니다. Windows 컴퓨터에는 USB 드라이버를 설치해야 합니다.

3.  **컴퓨터에 장치 연결** -최종 단계에는 USB 또는 WiFi로 컴퓨터에 장치를 연결하는 것이 포함됩니다.

이러한 각 단계는 아래 섹션에서 자세히 다룹니다.


## <a name="enable-debugging-on-the-device"></a>장치에서 디버깅 사용

Android 장치를 사용하여 Android 응용 프로그램을 테스트할 수 있습니다. 그러나 디버깅하기 전에 장치를 올바르게 구성해야 합니다. 포함된 단계는 장치에서 실행되는 Android 버전에 따라 약간 다릅니다.


### <a name="android-40-to-android-41"></a>Android 4.0부터 Android 4.1까지

Android 4.0.x부터 Android 4.1.x까지는 다음과 같은 단계에서 디버깅을 사용합니다.

1.  **설정** 화면으로 이동합니다.
2.  **개발자 옵션**을 선택합니다.
3.  **USB 디버깅** 옵션을 확인합니다.

이 스크린샷에서는 Android 4.0.3을 실행하는 장치의 **개발자 옵션** 화면을 보여줍니다.

[![개발자 옵션](set-up-device-for-development-images/developer-options-sml.png)](set-up-device-for-development-images/developer-options.png#lightbox)


### <a name="android-42-and-higher"></a>Android 4.2 이상

Android 4.2 이상 버전부터는 **개발자 옵션**이 기본적으로 숨겨집니다. 개발자 옵션을 사용할 수 있도록 하려면 **설정 > 휴대폰 정보**로 이동하고, **빌드 번호** 항목을 7번 눌러서 **개발자 옵션** 탭을 표시합니다.

[![빌드 번호 항목](set-up-device-for-development-images/about-phone-sml.png)](set-up-device-for-development-images/about-phone.png#lightbox)

**개발자 옵션** 탭을 **설정 > 시스템** 아래에서 사용할 수 있게 되면 개발자 설정을 표시하도록 엽니다.

[![개발자 설정 화면](set-up-device-for-development-images/developer3.png)](set-up-device-for-development-images/developer3.png#lightbox)

USB 디버깅 등 개발자 옵션을 사용하도록 설정하고 절전 모드를 유지하는 위치입니다.


## <a name="install-usb-drivers"></a>USB 드라이버 설치

이 단계는 OS X에 필요하지 않습니다. USB 케이블을 사용하여 장치를 Mac에 연결하기만 하면 됩니다.

Windows 컴퓨터에서 USB로 연결된 Android 장치를 인식하기 전에 몇 가지 추가 드라이버를 설치해야 할 수도 있습니다.

> [!NOTE]
> Google Nexus 장치를 설정하는 단계이며 참조로 제공됩니다. 특정 장치에 대한 단계가 다를 수 있지만 비슷한 패턴을 따릅니다. 문제가 발생하는 경우 장치에 대해 인터넷을 검색합니다.

**[Android SDK install path]\tools** 디렉터리에서 **android.bat** 응용 프로그램을 실행합니다. 기본적으로 Xamarin.Android 설치 관리자는 Windows 컴퓨터의 다음과 같은 위치에 Android SDK를 배치합니다.

    C:\Users\[username]\AppData\Local\Android\android-sdk


### <a name="download-the-usb-drivers"></a>USB 드라이버를 다운로드합니다.

Google Nexus 장치(Galaxy Nexus 제외)에는 Google USB 드라이버가 필요합니다. Galaxy Nexus용 드라이버가 [Samsung에서 배포](http://www.samsung.com/us/support/downloads/)됩니다.
다른 모든 Android 장치는 [해당 제조업체의 USB 드라이버](http://developer.android.com/tools/extras/oem-usb.html#Drivers)를 사용해야 합니다.

다음 스크린샷에서 볼 수 있듯이 Android SDK Manager를 시작하고 **Extras** 폴더를 확장하여 **Google USB 드라이버** 패키지를 설치합니다.

[![선택한 Google USB 드라이버 패키지](set-up-device-for-development-images/usbdriverpackage.png)](set-up-device-for-development-images/usbdriverpackage.png#lightbox)

**Google USB 드라이버** 상자를 확인하고 **설치** 단추를 클릭합니다.
드라이버 파일은 다음 위치에 다운로드됩니다.

    [Android SDK install path]\extras\google\usb\_driver

Xamarin.Android 설치의 기본 경로는 다음과 같습니다.

    C:\Users\[username]\AppData\Local\Android\android-sdk\extras\google\usb_driver



### <a name="installing-the-usb-driver"></a>USB 드라이버 설치

USB 드라이버를 다운로드한 후에 설치해야 합니다.
Windows 7에서 드라이버를 설치하려면:

1.  USB 케이블을 사용하여 컴퓨터에 장치를 연결합니다.

2.  바탕 화면 또는 Windows 탐색기에서 컴퓨터를 마우스 오른쪽 단추로 클릭하고 **관리**를 선택합니다.

3.  왼쪽 창에서 **장치**를 선택합니다.

4.  오른쪽 창에서 **기타 장치**를 찾아 확장합니다.

5.  장치 이름을 마우스 오른쪽 단추로 클릭하고 **드라이버 소프트웨어 업데이트**를 선택합니다.
    하드웨어 업데이트 마법사를 시작합니다.

6.  **컴퓨터에서 드라이버 소프트웨어 찾아보기**를 선택하고 **다음**을 클릭합니다.

7.  **찾아보기**를 클릭하고 USB 드라이버 폴더를 찾습니다. Google USB 드라이버는 **[Android SDK install path]\extras\google\usb_driver**에 위치합니다.

8.  **다음**을 클릭하여 드라이버를 설치합니다.


### <a name="installing-unverified-drivers-in-windows-8"></a>Windows 8에서 확인되지 않은 드라이버 설치

Windows에서 확인되지 않은 드라이버를 설치하기 위해 추가 단계가 필요할 수 있습니다.
8. 다음 단계에서는 Galaxy Nexus용 드라이버를 설치하는 방법에 대해 설명합니다.

1.  **Windows 8 고급 부팅 옵션에 액세스** - 이 단계에는 고급 부팅 옵션에 액세스하기 위해 컴퓨터를 다시 부팅하는 작업이 포함됩니다. 명령줄 프롬프트를 시작하고 다음 명령을 사용하여 컴퓨터를 다시 부팅합니다.

        shutdown.exe /r /o

2.  **장치 연결** - 컴퓨터에 장치를 연결합니다.

3.  **장치 관리자 시작** - **devmgmt.msc**를 실행합니다. 그러면 위에 노란색 삼각형이 나열된 장치가 표시됩니다.

4.  **장치 드라이버 설치** - 위에 설명한 대로 장치 드라이버를 설치합니다.



## <a name="connect-the-device-to-the-computer"></a>컴퓨터에 장치 연결

마지막 단계는 컴퓨터에 장치를 연결하는 것입니다. 여기에는 두 가지 방법이 있습니다.

-   **USB 케이블** - 가장 쉽고 일반적인 방법입니다. 장치에 USB 케이블을 연결한 다음, 컴퓨터에 연결하면 됩니다.

-   **WiFi** - USB 케이블을 사용하지 않고 WiFi를 통해 컴퓨터에 Android 장치를 연결할 수 있습니다. 이 기술은 약간 더 많은 노력이 필요하지만 USB 케이블이 없거나 장치가 USB 케이블에서 멀리 떨어져 있는 경우에 유용할 수 있습니다. WiFi를 통한 연결은 다음 섹션에서 설명합니다.


### <a name="connecting-over-wifi"></a>WiFi를 통한 연결

기본적으로 *ADB*([Android Debug Bridge](http://developer.android.com/tools/help/adb.html))는 USB를 통해 Android 장치와 통신하도록 구성됩니다. USB 대신 TCP/IP를 사용하도록 다시 구성할 수 있습니다. 이렇게 하려면 장치와 컴퓨터가 모두 동일한 WiFi 네트워크에 위치해야 합니다. 명령줄에서 다음 단계를 통해 WiFi 문제를 디버깅하도록 환경을 설정하려면:

1.  Android 장치의 IP 주소를 확인합니다. IP 주소를 확인하려면 **설정 > Wi-Fi**를 찾은 다음, 장치를 연결할 WiFi 네트워크를 누릅니다. 그러면 아래 스크린샷에 표시된 것과 비슷하게 네트워크 연결에 대한 정보를 보여주는 설정 화면이 표시됩니다.

    ![IP 주소](set-up-device-for-development-images/ip-settings.png)

    일부 버전의 Android에서는 IP 주소가 표시되지 않습니다. 하지만 대신 **설정 > 휴대폰 정보 > 상태** 아래에서 찾을 수 있습니다.

2.  USB를 통해 Android 장치를 컴퓨터에 연결합니다.

3.  다음으로 5555 포트에서 TCP를 사용하도록 ADB를 다시 시작합니다. 명령 프롬프트에서 다음 명령을 입력합니다.

        adb tcpip 5555

    이 명령이 실행된 후에 컴퓨터에서는 USB를 통해 연결된 장치를 수신할 수 없습니다.

4.  장치를 컴퓨터에 연결하는 USB 케이블을 분리합니다.

5.  위의 1단계에서 지정된 포트의 Android 장치에 연결할 수 있도록 ADB를 구성합니다.

        adb connect 192.168.1.28:5555

    이 명령이 완료되면 Android 장치는 WiFi를 통해 컴퓨터에 연결되어 있습니다.

WiFi를 통해 디버깅을 완료하면 다음 명령을 사용하여 ADB를 USB 모드로 다시 설정할 수 있습니다.

    adb usb

컴퓨터에 연결되는 장치를 나열하도록 ADB에 요청할 수 있습니다. 장치가 연결되는 방식에 관계 없이 명령 프롬프트에서 다음 명령을 실행하여 연결된 항목을 확인할 수 있습니다.

    adb devices


## <a name="summary"></a>요약

이 문서는 장치에서 디버깅을 사용하여 개발할 Android 장치를 구성하는 방법을 설명합니다. USB 또는 WiFi 중 하나를 사용하여 컴퓨터에 장치를 연결하는 방법을 설명합니다.


## <a name="related-links"></a>관련 링크

- [Android Debug Bridge](http://developer.android.com/tools/help/adb.html)
- [하드웨어 장치 사용](http://developer.android.com/tools/device.html)
- [Samsung 드라이버 다운로드](http://www.samsung.com/us/support/downloads/)
- [OEM USB 드라이버](http://developer.android.com/tools/extras/oem-usb.html#Drivers)
- [Google USB 드라이버](http://developer.android.com/sdk/win-usb.html)
- [XDA 개발자: Windows 8 - ADB/빠른 부팅 드라이버 문제 해결](http://forum.xda-developers.com/showthread.php?t=1583801)
