---
title: System.Exception AMDeviceNotificationSubscribe가 반환 하는 중...
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7E4ACC7E-F4FB-46C1-8837-C7FBAAFB2DC7
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 4fb0712366422e8810a2db60d40c3b85d9f4cd82
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119256"
---
# <a name="systemexception-amdevicenotificationsubscribe-returned-"></a>System.Exception AMDeviceNotificationSubscribe가 반환 하는 중...

> [!IMPORTANT]
> Xamarin의 최신 버전에서이 문제가 해결 되었습니다. 그러나 소프트웨어의 최신 버전에 문제가 발생 하면 제출 하세요를 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 정보 및 전체 빌드 로그 출력 전체 버전을 사용 하 여 합니다.


## <a name="fix"></a>문제 해결

1.  Kill을 `usbmuxd` 처리 시스템을 다시 시작 되도록 합니다.

    ```csharp
    sudo killall -QUIT usbmuxd
    ```

2.  이 문제를 해결 하지 않습니다 하는 경우 mac를 다시 부팅

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

또는 Mac 용 Visual Studio를 처음 시작할 때 오류 대화 상자에이 메시지가 나타날 수는 `mtbserver.log` Xamarin.iOS 빌드 호스트 앱에서 파일 (**Xamarin.iOS 빌드 호스트 > 호스트 로그를 작성 하는 보기**).

이 일반적이 지 않은 문제는 note 합니다. 에 표시 될 가능성이 있는 다른 오류가 있는 Visual Studio는 Mac 빌드 호스트에 연결 하는 데 문제가 있는 경우는 `mtbserver.log` 파일입니다.

### <a name="errors-in-systemlog"></a>System.log의 오류

일부 경우 다음 두 가지 오류에서 메시지도 나타날 수 있습니다에 반복적으로 `/var/log/system.log`:

```csharp
17:17:11.369 usbmuxd[55040]: dnssd_clientstub ConnectToServer: socket failed 24 Too many open files
17:17:11.369 com.apple.usbmuxd[55040]: _BrowseReplyReceivedCallback Error doing DNSServiceResolve(): -65539
```

## <a name="additional-information"></a>추가 정보

오류의 근본 원인을 한 추측 드문 경우에서 iOS 장치 및 시뮬레이터 정보를 보고 하는 일을 담당 하는 시스템 서비스 수 있습니다. OS X 예기치 않은 상태를 입력 하는입니다. Xamarin이 상태의 시스템 서비스와 제대로 상호 작용할 수 없습니다. 컴퓨터를 다시 부팅 시스템 서비스를 다시 시작 하 고 문제가 해결 되었습니다.

오류를 기반으로 `system.log` Bonjour 하이 문제가 관련 될 수는 표시 됩니다 (`mDNSResponder`). 서로 다른 WiFi 네트워크 간의 변경 문제 발생의 가능성을 높이기 위해 것 같습니다.

## <a name="references"></a>참조

*   [버그 11789-MonoTouch.MobileDevice.MobileDeviceException: AMDeviceNotificationSubscribe가 반환 되었습니다: 0xe8000063 [해결 NORESPONSE]](https://bugzilla.xamarin.com/show_bug.cgi?id=11789)
