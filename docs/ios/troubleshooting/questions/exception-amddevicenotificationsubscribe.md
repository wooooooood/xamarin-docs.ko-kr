---
title: System.Exception AMDeviceNotificationSubscribe가 반환되었습니다...
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7E4ACC7E-F4FB-46C1-8837-C7FBAAFB2DC7
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 8ae465c98dee25cd0f1fe635da45f4d399b42ee3
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70284477"
---
# <a name="systemexception-amdevicenotificationsubscribe-returned-"></a>System.Exception AMDeviceNotificationSubscribe가 반환되었습니다...

> [!IMPORTANT]
> 이 문제는 최신 버전의 Xamarin에서 해결 되었습니다. 그러나 최신 버전의 소프트웨어에서 문제가 발생 하는 경우 전체 버전 정보 및 전체 빌드 로그 출력을 사용 하 여 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 를 작성 하세요.


## <a name="fix"></a>문제 해결

1. 시스템이 다시 `usbmuxd` 시작 되도록 프로세스를 중지 합니다.

    ```csharp
    sudo killall -QUIT usbmuxd
    ```

2. 그래도 문제가 해결 되지 않으면 Mac을 다시 부팅 합니다.

## <a name="error-message"></a>오류 메시지

```csharp
Exception: Exception type: System.Exception
AMDeviceNotificationSubscribe returned: 3892314211
  at Xamarin.MacDev.IPhoneDeviceManager.SubscribeToNotifications () [0x00000] in <filename unknown="">:0
  at Xamarin.MacDev.IPhoneDeviceManager.StartWatching (Boolean useOwnRunloop) [0x00000] in <filename unknown="">:0
  at Mtb.Server.DeviceListener.Start () [0x00000] in <filename unknown="">:0
  at Mtb.Application.MainClass.RunDeviceListener () [0x00000] in <filename unknown="">:0
  at Mtb.Application.MainClass.Main (System.String[] args) [0x00000] in <filename unknown="">:0
```

이 메시지는 먼저 Mac용 Visual Studio를 시작 하거나 `mtbserver.log` xamarin.ios 빌드 호스트 앱 (**xamarin.ios 빌드 호스트 > 보기 빌드 호스트 로그**)의 파일에서 오류 대화 상자에 표시 될 수 있습니다.

일반적이 지 않은 문제입니다. Mac 빌드 호스트에 연결 하는 데 문제가 있는 경우 `mtbserver.log` 파일에 표시 될 가능성이 높은 다른 오류가 있습니다.

### <a name="errors-in-systemlog"></a>시스템 로그 오류

경우에 `/var/log/system.log`따라 다음과 같은 두 가지 오류 메시지가 반복적으로 표시 될 수도 있습니다.

```csharp
17:17:11.369 usbmuxd[55040]: dnssd_clientstub ConnectToServer: socket failed 24 Too many open files
17:17:11.369 com.apple.usbmuxd[55040]: _BrowseReplyReceivedCallback Error doing DNSServiceResolve(): -65539
```

## <a name="additional-information"></a>추가 정보

오류의 근본 원인이 되는 한 가지 추측은 iOS 장치 및 시뮬레이터 정보 보고를 담당 하는 OS X 시스템 서비스에서 예기치 않은 상태를 입력 하는 경우입니다. Xamarin은이 상태의 시스템 서비스와 제대로 상호 작용할 수 없습니다. 컴퓨터를 다시 부팅 하면 시스템 서비스가 다시 시작 되 고 문제가 해결 됩니다.

오류가 발생 `system.log` 하는 경우이 문제가 Bonjour (`mDNSResponder`)와 관련 될 수 있습니다. 서로 다른 WiFi 네트워크 간에 변경 하면 문제를 해결 하는 기회가 증가 하는 것 처럼 보입니다.

## <a name="references"></a>참조 항목

* [Bug 11789 - MonoTouch.MobileDevice.MobileDeviceException: 반환 된 AMDeviceNotificationSubscribe: 0xe8000063 [RESOLVED NORESPONSE]](https://bugzilla.xamarin.com/show_bug.cgi?id=11789)
