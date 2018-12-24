---
title: Wear 디바이스에서 디버그
description: 이 문서에서는 Wear 장치에서 Xamarin.Android Wear 응용 프로그램을 디버깅 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 01668E4B-BB83-4C26-B23A-F788173FB823
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 232fcd1d369eba1daad170986f2e2c4c913a3649
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112471"
---
# <a name="debug-on-a-wear-device"></a>Wear 디바이스에서 디버그

_이 문서에서는 Wear 장치에서 Xamarin.Android Wear 응용 프로그램을 디버깅 하는 방법에 설명 합니다._


## <a name="overview"></a>개요

Android Wear Smartwatch 같은 Android Wear 장치를 사용 하는 경우에 에뮬레이터를 사용 하는 대신 장치에서 앱을 실행할 수 있습니다. (Android Wear 앱 배포 및 실행 프로세스에 익숙해 참조 아직 없는 경우 [안녕하세요, Wear](~/android/wear/get-started/hello-wear.md).)

## <a name="prepare-the-wear-device"></a>Wear 장치를 준비 합니다.

Android Wear 장치에서 디버깅을 사용 하도록 설정 하려면 다음 단계를 사용 합니다.

1.  엽니다는 **설정을** Android Wear 장치에서 메뉴.

2.  메뉴 및 탭의 아래쪽으로 스크롤하여 **에 대 한**합니다.

3.  빌드 번호를 7 번 누릅니다.

4.  에 **설정을** 메뉴의 탭 **개발자 옵션**합니다.

5.  확인 **ADB 디버깅** 사용 가능 합니다.


## <a name="debugging-over-usb"></a>USB를 통해 디버깅

Wear 장치에 USB 포트가, Wear 장치를 컴퓨터에 연결를 배포 하 수 실행/디버그 앱 Android 휴대폰을 사용 하는 (자세한 내용은 [장치에서 디버그](~/android/deploy-test/debugging/debug-on-device.md)).


## <a name="debugging-over-bluetooth"></a>Bluetooth를 통한 디버깅

Wear 장치에 USB 포트를 찾을 수 없는 경우 컴퓨터에 연결 된 Android 휴대폰에 앱의 디버그 출력을 라우팅하여 라이브러리 Wear 장치에 앱을 Bluetooth를 통해 배포할 수 있습니다. 

### <a name="prepare-your-phone"></a>전화를 준비 합니다.

Wear 장치에 Bluetooth 연결에 대 한 휴대폰을 준비 하려면 다음 단계를 사용 합니다. 

1.  아직 수행 하는 경우 설정 Xamarin.Android 개발을 위한 휴대폰에 설명 된 대로 [장치 개발을 위한 설정](~/android/get-started/installation/set-up-device-for-development.md)합니다.

2.  다운로드 및 설치 무료 [Android Wear](https://play.google.com/store/apps/details?id=com.google.android.wearable.app) Google Play 스토어에서 앱.

### <a name="connect-the-device"></a>장치 연결

다음 단계를 사용 하 여 사용자의 휴대폰에 Wear 장치를 연결 합니다.

1.  휴대폰는 역할을 Bluetooth 중간 (위에서 구성한)을 Android Wear 앱을 시작 합니다. 

2.  탭의 **설정을** 아이콘입니다.

3.  사용 하도록 설정 **Bluetooth를 통한 디버깅**합니다. Android Wear 앱의 화면에 표시 되는 다음 상태를 표시 됩니다.

        Host: disconnected
        Target: connected

4.  휴대폰을 USB를 통해 컴퓨터에 연결 합니다. 컴퓨터에서 다음 명령을 입력 합니다.

    ```shell
    adb forward tcp:4444 localabstract:/adb-hub
    adb connect 127.0.0.1:4444
    ```

    4444 포트를 사용할 수 없는 경우 액세스 권한이 있는 다른 모든 사용 가능한 포트를 사용할 수 있습니다. 

    **참고**: Mac 용 Visual Studio 또는 Visual Studio 다시 시작, 다시 Wear 장치로 연결을 설정 하려면 다음이 명령을 실행 해야 합니다.

5.  Wear 장치 메시지를 표시 하면 허용 하는지 확인 **ADB 디버깅**합니다. Android Wear 앱에서 변경 상태를 표시 됩니다.

        Host: connected
        Target: connected

6.  위의 단계를 완료 하면 실행 `adb devices` 휴대폰 및 Android Wear 장치 둘 다의 상태를 보여 줍니다.

        List of devices attached
        127.0.0.1:4444    device
        019ad61df0a69399  device

이 시점에서 Wear 장치에 앱을 배포할 수 있습니다.

<a name="screenshots" />

### <a name="taking-screenshots"></a>스크린샷

Wear 장치 스크린샷 다음 명령을 입력 하 여 수행할 수 있습니다. 

```shell
adb -s 127.0.0.1:4444 shell screencap -p /sdcard/DCIM/screencap.png
```

다음 명령을 입력 하 여 컴퓨터에 스크린 샷 복사 합니다.

```shell
adb -s 127.0.0.1:4444 pull /sdcard/DCIM/screencap.png
```

다음 명령을 입력 하 여 장치의 스크린 샷을 삭제 합니다.

```shell
adb -s 127.0.0.1:4444 shell rm /sdcard/DCIM/screencap.png
```


### <a name="uninstalling-an-app"></a>앱 제거

Wear 장치에서 다음 명령을 입력 하 여 앱을 제거할 수 있습니다.

```shell
adb -s 127.0.0.1:4444 uninstall <package name>
```

예를 들어 패키지 이름을 사용 하 여 앱을 제거 하려면 `com.xamarin.weartest`, 다음 명령을 입력 합니다.

```shell
adb -s 127.0.0.1:4444 uninstall com.xamarin.weartest
```

Android Wear 장치 Bluetooth를 통해 디버깅 하는 방법에 대 한 자세한 내용은 참조 하세요. [Bluetooth를 통한 디버깅](https://developer.android.com/training/wearables/apps/bt-debugging.html)합니다.


## <a name="debugging-a-wear-app-with-a-companion-phone-app"></a>도우미 전화 앱을 사용 하는 Wear 앱 디버깅

Google Play에서 배포를 위한 도우미 Android 휴대폰 앱을 android Wear 앱 패키징 됩니다 (자세한 내용은 [패키징 작업](~/android/wear/deploy-test/packaging.md)). 그러나 여전히 개발 Wear 앱 및 해당 도우미 앱 개별적으로 합니다. Google Play 스토어를 통해 앱을 릴리스할 때 Wear 앱 도우미 앱을 사용 하 여 패키지 및 가능한 경우에 자동으로 설치 됩니다.

도우미 앱을 사용 하 여 Wear 앱을 디버그 합니다. 

1.  빌드하고 휴대폰 도우미 앱을 배포 합니다.

2.  Wear 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 기본 시작 프로젝트로 설정 합니다.

3.  Wear 프로젝트 착용 식 장치를 배포 합니다.

4.  실행 하 고 장치의 Wear 앱을 디버그 합니다.

 
## <a name="summary"></a>요약

이 문서에서는 Bluetooth 통해 Visual Studio에서 디버그 Wear Android Wear 장치를 구성 하는 방법 및 도우미 전화 앱을 사용 하 여 Wear 앱을 디버그 하는 방법을 설명 합니다. 또한 Bluetooth 통한 Wear 앱을 디버깅 하는 것에 대 한 일반적인 디버깅 팁을 제공 합니다.
