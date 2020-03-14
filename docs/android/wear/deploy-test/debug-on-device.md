---
title: Wear 디바이스에서 디버그
description: 이 문서에서는 마모 된 장치에서 Xamarin Android 마모 응용 프로그램을 디버그 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 01668E4B-BB83-4C26-B23A-F788173FB823
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 965ed4e802c05f8450192c0fec17fe31e464c779
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305968"
---
# <a name="debug-on-a-wear-device"></a>Wear 디바이스에서 디버그

_이 문서에서는 마모 된 장치에서 Xamarin Android 마모 응용 프로그램을 디버그 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Android 마모 Smartwatch 같은 Android 장치를 사용 하는 경우 에뮬레이터를 사용 하는 대신 장치에서 앱을 실행할 수 있습니다. Android 앱을 배포 하 고 실행 하는 프로세스에 익숙하지 않은 경우 [Hello, 마모](~/android/wear/get-started/hello-wear.md)를 참조 하세요.

## <a name="prepare-the-wear-device"></a>마모 장치 준비:

Android 마모 장치에서 디버깅을 사용 하도록 설정 하려면 다음 단계를 사용 합니다.

1. Android 마모 장치에서 **설정** 메뉴를 엽니다.

2. 메뉴의 아래쪽으로 스크롤하고 **정보**를 탭 합니다.

3. 빌드 번호를 7 번 누릅니다.

4. **설정** 메뉴에서 **개발자 옵션**을 탭 합니다.

5. **Adb 디버깅이** 사용 되는지 확인 합니다.

## <a name="debugging-over-usb"></a>USB를 통한 디버깅

사용자의 장치에 USB 포트가 있는 경우, Android 휴대폰을 사용 하는 것 처럼 장치를 컴퓨터에 연결 하 여 배포 하 고 앱을 실행/디버그할 수 있습니다. 자세한 내용은 [장치에서 디버그](~/android/deploy-test/debugging/debug-on-device.md)를 참조 하세요.

## <a name="debugging-over-bluetooth"></a>Bluetooth를 통한 디버깅

사용자의 장치에 USB 포트가 없는 경우 앱의 디버그 출력을 컴퓨터에 연결 된 Android 휴대폰으로 라우팅하여 Bluetooth를 통해 장치에 앱을 배포할 수 있습니다. 

### <a name="prepare-your-phone"></a>휴대폰 준비

다음 단계를 사용 하 여 장치에 대 한 Bluetooth 연결을 준비 하려면 다음 단계를 수행 합니다. 

1. 아직 수행 하지 않은 경우 [개발용 장치 설정](~/android/get-started/installation/set-up-device-for-development.md)에 설명 된 대로 xamarin.ios 개발용으로 전화를 설정 합니다.

2. Google Play 스토어에서 무료 [Android](https://play.google.com/store/apps/details?id=com.google.android.wearable.app) 앱을 다운로드 하 여 설치 합니다.

### <a name="connect-the-device"></a>장치 연결

다음 단계를 사용 하 여 장치를 휴대폰에 연결 합니다.

1. Bluetooth 중개자 역할을 하는 휴대폰 (위에서 구성 됨)에서 Android 앱을 시작 합니다. 

2. **설정** 아이콘을 탭 합니다.

3. **Bluetooth를 통해 디버깅**을 사용 하도록 설정 합니다. Android 앱의 화면에 다음과 같은 상태가 표시 되어야 합니다.

    ```
    Host: disconnected
    Target: connected
    ```

4. USB를 통해 컴퓨터에 휴대폰을 연결 합니다. 컴퓨터에서 다음 명령을 입력 합니다.

    ```shell
    adb forward tcp:4444 localabstract:/adb-hub
    adb connect 127.0.0.1:4444
    ```

    4444 포트를 사용할 수 없는 경우 액세스할 수 있는 다른 모든 포트를 사용할 수 있습니다. 

    > [!NOTE]
    > Visual Studio 또는 Mac용 Visual Studio을 다시 시작 하는 경우 이러한 명령을 다시 실행 하 여 마모 된 장치에 대 한 연결을 설정 해야 합니다.

5. 마모 장치에 메시지가 표시 되 면 **Adb 디버깅**을 허용 하는지 확인 합니다. Android 마모 앱에서 상태가 다음과 같이 변경 됩니다.

    ```
    Host: connected
    Target: connected
    ```

6. 위의 단계를 완료 한 후 `adb devices`를 실행 하면 전화와 Android 장치 둘 다의 상태가 표시 됩니다.

    ```
    List of devices attached
    127.0.0.1:4444    device
    019ad61df0a69399  device
    ```

이제 앱을 마모 장치에 배포할 수 있습니다.

<a name="screenshots" />

### <a name="taking-screenshots"></a>스크린샷 작성

다음 명령을 입력 하 여 마모 된 장치의 스크린샷을 만들 수 있습니다. 

```shell
adb -s 127.0.0.1:4444 shell screencap -p /sdcard/DCIM/screencap.png
```

다음 명령을 입력 하 여 컴퓨터에 스크린샷을 복사 합니다.

```shell
adb -s 127.0.0.1:4444 pull /sdcard/DCIM/screencap.png
```

다음 명령을 입력 하 여 장치에서 스크린샷 삭제 합니다.

```shell
adb -s 127.0.0.1:4444 shell rm /sdcard/DCIM/screencap.png
```

### <a name="uninstalling-an-app"></a>앱 제거

다음 명령을 입력 하 여 마모 장치에서 앱을 제거할 수 있습니다.

```shell
adb -s 127.0.0.1:4444 uninstall <package name>
```

예를 들어 `com.xamarin.weartest`패키지 이름으로 앱을 제거 하려면 다음 명령을 입력 합니다.

```shell
adb -s 127.0.0.1:4444 uninstall com.xamarin.weartest
```

Bluetooth를 통해 Android 장치를 디버그 하는 방법에 대 한 자세한 내용은 [bluetooth를 통한 디버그](https://developer.android.com/training/wearables/apps/bt-debugging.html)를 참조 하세요.

## <a name="debugging-a-wear-app-with-a-companion-phone-app"></a>동반 휴대폰 앱을 사용 하 여 마모 된 앱 디버그

Android 제품 앱은 Google Play에 배포할 수 있도록 도우미 Android 휴대폰 앱과 함께 패키지 됩니다. 자세한 내용은 [패키징을 사용한 작업](~/android/wear/deploy-test/packaging.md)을 참조 하세요. 그러나 개발 앱 및 해당 도우미 앱은 별도로 개발 합니다. Google Play 스토어를 통해 앱을 릴리스할 때, 앱이 함께 제공 되는 앱과 함께 패키지 되 고 가능 하면 자동으로 설치 됩니다.

도우미 앱을 사용 하 여 마모 된 앱을 디버깅 하려면 다음을 수행 합니다. 

1. 도우미 앱을 빌드하고 휴대폰에 배포 합니다.

2. 마모 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 기본 시작 프로젝트로 설정 합니다.

3. Wearable 장치에 마모 프로젝트를 배포 합니다.

4. 장치에서 마모 된 앱을 실행 하 고 디버그 합니다.

## <a name="summary"></a>요약

이 문서에서는 Bluetooth를 통해 Visual Studio에서 마모 된 디버그를 위해 Android 장치를 구성 하는 방법 및 자매 phone 앱을 사용 하 여 마모 된 앱을 디버그 하는 방법을 설명 했습니다. 또한 Bluetooth를 통해 마모 된 앱을 디버깅 하기 위한 일반적인 디버깅 팁을 제공 합니다.
