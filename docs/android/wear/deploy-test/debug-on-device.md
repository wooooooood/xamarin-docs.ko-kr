---
title: 마모 장치에서 디버깅
description: 이 문서에서는 마모 장치에서 Xamarin.Android 마모 응용 프로그램을 디버깅 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 01668E4B-BB83-4C26-B23A-F788173FB823
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 3f3143dcda4017bbabfbd34a58a40665beea6f75
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30768053"
---
# <a name="debug-on-a-wear-device"></a>마모 장치에서 디버깅

_이 문서에서는 마모 장치에서 Xamarin.Android 마모 응용 프로그램을 디버깅 하는 방법을 설명 합니다._


## <a name="overview"></a>개요

Android 착용 Smartwatch 같은 쓰는 유형 Android 장치를 설정한 경우에 에뮬레이터를 사용 하는 대신 장치에 응용 프로그램을 실행할 수 있습니다. (쓰는 유형 Android 앱을 실행 및 배포 프로세스에 잘 참조 아직 없는 경우 [안녕하세요, 마모](~/android/wear/get-started/hello-wear.md).)

## <a name="prepare-the-wear-device"></a>마모 장치를 준비 합니다.

다음 단계를 사용 하 여 쓰는 유형 Android 장치에서 디버깅을 활성화 하려면:

1.  열기는 **설정을** 쓰는 유형 Android 장치에 대 한 메뉴입니다.

2.  메뉴 및 탭의 아래쪽으로 스크롤하여 **에 대 한**합니다.

3.  빌드 번호를 7 번 누릅니다.

4.  에 **설정** 메뉴, tap **개발자 옵션**합니다.

5.  확인 **ADB 디버깅** 를 사용할 수 있습니다.


## <a name="debugging-over-usb"></a>USB를 통해 디버깅

마모 장치에 USB 포트가, 마모 장치를 컴퓨터에 연결를 배포 하 수 실행/디버그 앱 Android 휴대폰을 사용 하는 것 처럼 (자세한 내용은 참조 [장치에서 디버깅](~/android/deploy-test/debugging/debug-on-device.md)).


## <a name="debugging-over-bluetooth"></a>Bluetooth를 통해 디버깅

마모 장치에 USB 포트를 찾을 수 없는 경우 컴퓨터에 연결 된 Android 휴대폰에 응용 프로그램의 디버그 출력을 라우팅하여 라이브러리 마모 장치에 앱을 Bluetooth를 통해 배포할 수 있습니다. 

### <a name="prepare-your-phone"></a>전화를 준비 합니다.

Bluetooth 연결 마모 장치를 만들기 위한 휴대폰을 준비 하려면 다음 단계를 사용 합니다. 

1.  아직 수행 경우 설정 Xamarin.Android 개발을 위한 휴대폰에 설명 된 대로 [개발에 대 한 설정으로 장치](~/android/get-started/installation/set-up-device-for-development.md)합니다.

2.  다운로드 및 설치는 무료 [Android 착용](https://play.google.com/store/apps/details?id=com.google.android.wearable.app) Google Play 스토어에서 응용 프로그램입니다.

### <a name="connect-the-device"></a>장치 연결

다음 단계를 사용 하 여 장치에 연결 하려면 마모 ल ा:

1.  전화를 됩니다 역할 Bluetooth 중간 (위에 구성 된)을에서 쓰는 유형 Android 응용 프로그램을 시작 합니다. 

2.  탭의 **설정을** 아이콘입니다.

3.  사용 하도록 설정 **Bluetooth를 통해 디버깅**합니다. Android 착용 응용 프로그램의 화면에 표시 되는 다음과 같은 상태를 표시 되어야 합니다.

        Host: disconnected
        Target: connected

4.  컴퓨터에 USB를 통해 전화를 연결 합니다. 컴퓨터에서 다음 명령을 입력 합니다.

    ```shell
    adb forward tcp:4444 localabstract:/adb-hub
    adb connect 127.0.0.1:4444
    ```

    4444 포트를 사용할 수 없는 경우 액세스 권한이 있는 다른 모든 사용 가능한 포트를 사용할 수 있습니다. 

    **참고**:를 다시 시작 하면 Visual Studio 또는 Visual Studio Mac 용 이들이 명령을 마모 장치에 대 한 연결 설정 다시 실행 해야 합니다.

5.  마모 장치 메시지를 표시 하면 확인 허용 하는 **ADB 디버깅**합니다. Android 착용 응용 프로그램에서 변경 상태를 표시 되어야 합니다.

        Host: connected
        Target: connected

6.  위의 단계를 완료 한 후 실행 `adb devices` 전화와 쓰는 유형 Android 장치 둘 다의 상태를 표시 합니다.

        List of devices attached
        127.0.0.1:4444    device
        019ad61df0a69399  device

이 시점에서 마모 장치에 앱을 배포할 수 있습니다.

<a name="screenshots" />

### <a name="taking-screenshots"></a>만들기 스크린 샷

다음 명령을 입력 하 여 마모 장치의 스크린 샷을 수행할 수 있습니다. 

```shell
adb -s 127.0.0.1:4444 shell screencap -p /sdcard/DCIM/screencap.png
```

다음 명령을 입력 하 여 컴퓨터에 스크린 샷 복사:

```shell
adb -s 127.0.0.1:4444 pull /sdcard/DCIM/screencap.png
```

다음 명령을 입력 하 여 장치에 스크린 샷을 삭제 합니다.

```shell
adb -s 127.0.0.1:4444 shell rm /sdcard/DCIM/screencap.png
```


### <a name="uninstalling-an-app"></a>앱 제거

다음 명령을 입력 하 여 마모 장치에서 앱을 제거할 수 있습니다.

```shell
adb -s 127.0.0.1:4444 uninstall <package name>
```

예를 들어 패키지 이름으로 앱을 제거 하려면 `com.xamarin.weartest`, 다음 명령을 입력 합니다.

```shell
adb -s 127.0.0.1:4444 uninstall com.xamarin.weartest
```

Bluetooth를 통해 쓰는 유형 Android 장치를 디버깅 하는 방법에 대 한 자세한 내용은 참조 [Bluetooth를 통해 디버깅](https://developer.android.com/training/wearables/apps/bt-debugging.html)합니다.


## <a name="debugging-a-wear-app-with-a-companion-phone-app"></a>도우미 전화 앱과 마모 응용 프로그램 디버깅

Google Play에서 배포에 대 한 도우미 Android 휴대폰 앱 android 마모 앱 패키징 됩니다 (자세한 내용은 참조 [패키징 작업](~/android/wear/deploy-test/packaging.md)). 그러나 여전히 개발할 마모 앱 및 해당 도우미 앱 별도로 합니다. Google Play 스토어를 통해 응용 프로그램을 릴리스할 때 마모 앱을 함께 앱과 함께 패키지 하 고 가능한 경우에 자동으로 설치 됩니다.

와 함께 응용 마모 앱을 디버그 합니다. 

1.  빌드 및 휴대폰에 도우미 앱을 배포 합니다.

2.  마모 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 기본 시작 프로젝트로 설정 합니다.

3.  마모 프로젝트 착용 식 장치를 배포 합니다.

4.  실행 하 고 장치에서 마모 응용 프로그램을 디버깅 합니다.

 
## <a name="summary"></a>요약

이 문서는 Bluetooth 통해 Visual Studio에서 마모 디버깅용 쓰는 유형 Android 장치를 구성 하는 방법과 도우미 전화 앱과 마모 응용 프로그램을 디버깅 하는 방법을 설명 합니다. 또한 Bluetooth 통해 마모 응용 프로그램 디버깅에 대 한 일반적인 디버깅 팁을 제공 합니다.
