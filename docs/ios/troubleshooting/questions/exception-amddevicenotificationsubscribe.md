---
title: System.Exception AMDeviceNotificationSubscribe 반환 되 고 있습니다...
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7E4ACC7E-F4FB-46C1-8837-C7FBAAFB2DC7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 257e0c14de3c23825b6abe6601c25438db81c58e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30776484"
---
# <a name="systemexception-amdevicenotificationsubscribe-returned-"></a>System.Exception AMDeviceNotificationSubscribe 반환 되 고 있습니다...

> [!IMPORTANT]
> 최신 버전의 Xamarin에이 문제가 해결 되었습니다. 그러나 최신 버전의 소프트웨어에 문제가 발생 하면 보관는 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 정보 및 전체 빌드 로그 출력 전체 프로그램 버전 관리를 사용 합니다.


## <a name="fix"></a>문제 해결

1.  Kill는 `usbmuxd` 처리 하는 시스템이 다시 시작 됩니다.

    ```csharp
    sudo killall -QUIT usbmuxd
    ```

2.  문제가 해결 되지, mac 다시 부팅

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

또는, Mac 용 Visual Studio를 처음 시작 오류 대화 상자에이 메시지가 나타날 수는 `mtbserver.log` Xamarin.iOS 빌드 호스트 응용 프로그램에서 파일 (**Xamarin.iOS 빌드 호스트 > 호스트 로그 빌드 보기**).

일반적이 지 않은 문제 인지 note 합니다. Visual Studio는 Mac 빌드 호스트 연결을 문제가 많은 경우에 발생할 가능성이 높은 다른 오류는 `mtbserver.log` 파일입니다.

### <a name="errors-in-systemlog"></a>System.log의 오류

일부의 경우 다음 두 개의 오류에서 메시지가 나타날 수 있습니다도 반복 해 서 `/var/log/system.log`:

```csharp
17:17:11.369 usbmuxd[55040]: dnssd_clientstub ConnectToServer: socket failed 24 Too many open files
17:17:11.369 com.apple.usbmuxd[55040]: _BrowseReplyReceivedCallback Error doing DNSServiceResolve(): -65539
```

## <a name="additional-information"></a>추가 정보

오류의 근본 원인에 하나의 추측 드문 경우 지만에서 할 수 있는 iOS 장치 및 시뮬레이터 정보를 보고 하는 일을 담당 하는 시스템 서비스 OS X 예기치 않은 상태를 입력입니다. Xamarin이 상태에서 시스템 서비스와 제대로 상호 작용할 수 없습니다. 컴퓨터를 다시 부팅 시스템 서비스를 다시 시작 되 고 문제가 해결 되었습니다.

오류를 기반으로 `system.log` 나타나는이 문제가 Bonjour 관련 될 수 있습니다 (`mDNSResponder`). 서로 다른 WiFi 네트워크 간 변경 문제 부하량의 가능성을 높이기 위해 처럼 보입니다.

## <a name="references"></a>참조

*   [버그 11789-MonoTouch.MobileDevice.MobileDeviceException: AMDeviceNotificationSubscribe 반환: 0xe8000063 [해결 NORESPONSE]](https://bugzilla.xamarin.com/show_bug.cgi?id=11789)
