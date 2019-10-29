---
title: IOS의 푸시 알림
description: 이 문서에서는 iOS 9 및 이전 버전에서 푸시 알림을 사용 하는 방법을 설명 합니다. 인증서에 대해 설명 하 고 APNS (Apple Push Notification Gateway Service) 등에 등록 합니다.
ms.prod: xamarin
ms.assetid: 64B3BE6A-A3E2-4B1B-95ED-02D27A8FDAAC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 468d0e16a3bd5745a243b2d7c09e642e3aeffd1d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031360"
---
# <a name="push-notifications-in-ios"></a>IOS의 푸시 알림

> [!IMPORTANT]
> 이 섹션의 정보는 iOS 9 및 이전 버전과 관련이 있으며 이전 iOS 버전을 지원 하기 위해 여기에 남아 있습니다. IOS 10 이상에서는 iOS 장치에서 로컬 알림과 원격 알림을 모두 지원 하기 위한 [사용자 알림 프레임 워크 가이드](~/ios/platform/user-notifications/index.md) 를 참조 하세요.

푸시 알림은 간단 하 게 유지 되 고 모바일 응용 프로그램에 업데이트를 위해 서버 응용 프로그램에 알려야 하는 충분 한 데이터만 포함 해야 합니다. 예를 들어 새 전자 메일이 도착할 때 서버 응용 프로그램은 새 전자 메일이 도착 했음을 모바일 응용 프로그램에 알려 줍니다. 알림은 새 전자 메일을 포함 하지 않습니다. 그러면 모바일 응용 프로그램이 적절 한 경우 서버에서 새 전자 메일을 검색 합니다.

IOS의 푸시 알림 중심에는 *APNS (Apple Push Notification Gateway Service)* 가 있습니다. Apple에서 제공 하는 서비스로, 응용 프로그램 서버에서 iOS 장치로 알림을 라우팅하는 역할을 담당 합니다.
다음 이미지는 iOS에 대 한 푸시 알림 토폴로지를 보여 줍니다.![](remote-notifications-in-ios-images/image4.png "이 이미지는 iOS에 대 한 푸시 알림 토폴로지를 보여 줍니다.")

원격 알림 자체는 [IOS 개발자 설명서](https://developer.apple.com/devcenter/ios/index.action)의 [로컬 및 푸시 알림 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) 의 [알림 페이로드](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CreatingtheNotificationPayload.html#//apple_ref/doc/uid/TP40008194-CH10-SW1) 섹션에 지정 된 형식 및 프로토콜을 준수 하는 JSON 형식 문자열입니다.

Apple은 *Sandbox* 와 *프로덕션* 환경 이라는 두 가지 APNS 환경을 유지 관리 합니다. 샌드박스 환경은 개발 단계 중에 테스트를 위한 것 이며 TCP 포트 2195의 `gateway.sandbox.push.apple.com`에서 찾을 수 있습니다. 프로덕션 환경은 배포 된 응용 프로그램에서 사용 되 고 `gateway.push.apple.com` TCP 포트 2195에서 찾을 수 있습니다.

## <a name="requirements"></a>요구 사항

푸시 알림은 APNS의 아키텍처에 의해 결정 되는 다음 규칙을 준수 해야 합니다.

- **256 바이트 메시지 제한** -알림의 전체 메시지 크기는 256 바이트를 초과 하면 안 됩니다.
- **수신 확인 안 함** -APNS는 보낸 사람에 게 메시지를 보낸 사람에 게 보낸 알림을 제공 하지 않습니다. 장치에 연결할 수 없고 여러 순차적 알림이 전송 되는 경우 가장 최근 알림을 제외한 모든 알림이 손실 됩니다. 가장 최근의 알림만 장치에 전달 됩니다.
- **각 응용 프로그램에는 보안 인증서가 필요** 합니다. APNS와의 통신은 SSL을 통해 수행 해야 합니다.

## <a name="creating-and-using-certificates"></a>인증서 만들기 및 사용

이전 섹션에서 언급 한 각 환경에는 자체 인증서가 필요 합니다. 이 섹션에서는 인증서를 만들고, 프로 비전 프로필과 연결한 다음, PushSharp에서 사용할 개인 정보 교환 인증서를 가져오는 방법을 다룹니다.

1. 인증서를 만들려면 다음 스크린샷에 표시 된 것 처럼 Apple 웹 사이트의 iOS 프로 비전 포털로 이동 합니다 (왼쪽의 앱 Id 메뉴 항목 확인).

    [![](remote-notifications-in-ios-images/image5new.png "The iOS Provisioning Portal on Apples website")](remote-notifications-in-ios-images/image5new.png#lightbox)

2. 그런 다음 앱 ID의 섹션으로 이동 하 고 다음 스크린샷에 표시 된 것 처럼 새 앱 ID를 만듭니다.

    [![](remote-notifications-in-ios-images/image6new.png "Navigate to the App IDs section and create a new app ID")](remote-notifications-in-ios-images/image6new.png#lightbox)

3. **+** 단추를 클릭 하면 다음 스크린샷에 표시 된 것 처럼 앱 ID에 대 한 설명 및 번들 식별자를 입력할 수 있습니다.

    [![](remote-notifications-in-ios-images/image7new.png "Enter the description and a Bundle Identifier for the app ID")](remote-notifications-in-ios-images/image7new.png#lightbox)

4. **명시적 앱 ID** 를 선택 하 고 번들 식별자가 `*`로 끝나지 않는지 확인 합니다. 이렇게 하면 여러 응용 프로그램에 적합 한 식별자가 생성 되 고 단일 응용 프로그램에 대 한 푸시 알림 인증서가 필요 합니다.

5. App Services에서 **푸시 알림**을 선택 합니다.

    [![](remote-notifications-in-ios-images/image8new.png "Select Push Notifications")](remote-notifications-in-ios-images/image8new.png#lightbox)

6. **제출을** 눌러 새 앱 ID의 등록을 확인 합니다.

    [![](remote-notifications-in-ios-images/image9new.png "Confirm registration of the new App ID")](remote-notifications-in-ios-images/image9new.png#lightbox)

7. 다음으로, 앱 ID에 대 한 인증서를 만들어야 합니다. 왼쪽 탐색에서 **인증서 모두 >** 로 이동 하 고 다음 스크린샷에 표시 된 것 처럼 `+` 단추를 선택 합니다.

    [![](remote-notifications-in-ios-images/image10new.png "Create the certificate for the app ID")](remote-notifications-in-ios-images/image8.png#lightbox)

8. 개발 또는 프로덕션 인증서를 사용할지 여부를 선택 합니다.

    [![](remote-notifications-in-ios-images/image11new.png "Select a Development or Production certificate")](remote-notifications-in-ios-images/image11new.png#lightbox)

9. 그런 다음 방금 만든 새 앱 ID를 선택 합니다.

    [![](remote-notifications-in-ios-images/image12new.png "Select the new App ID just created")](remote-notifications-in-ios-images/image12new.png#lightbox)

10. 그러면 Mac에서 키 **집합 액세스** 응용 프로그램을 사용 하 여 *인증서 서명 요청* 을 만드는 과정을 안내 하는 지침이 표시 됩니다.

11. 이제 인증서를 만들었으므로 APNs에 등록할 수 있도록 응용 프로그램에 서명 하는 빌드 프로세스의 일부로 사용 해야 합니다. 이를 위해서는 인증서를 사용 하는 프로 비전 프로필을 만들고 설치 해야 합니다.

12. 개발 프로 비전 프로필을 만들려면 프로 **비전 프로필** 섹션으로 이동 하 고 단계에 따라 방금 만든 앱 Id를 사용 합니다.

13. 프로 비전 프로필을 만들었으면 **Xcode Organizer** 를 열고 새로 고칩니다. 만든 프로 비전 프로필이 표시 되지 않으면 iOS 프로 비전 포털에서 프로필을 다운로드 하 고 수동으로 가져와야 할 수 있습니다. 다음 스크린샷은 프로 비전 프로필이 추가 된 이끌이의 예를 보여 줍니다.  
    [![](remote-notifications-in-ios-images/image13new.png "This screen shot shows an example of the Organizer with the provision profile added")](remote-notifications-in-ios-images/image13new.png#lightbox)

14. 이 시점에서 새로 만든 프로 비전 프로필을 사용 하도록 Xamarin.ios 프로젝트를 구성 해야 합니다. 다음 스크린샷에 표시 된 것 처럼 **IOS 번들 서명** 탭의 **프로젝트 옵션** 대화 상자에서이 작업을 수행할 수 있습니다.  
    [![](remote-notifications-in-ios-images/image11.png "Configure the Xamarin.iOS project to use this newly created provisioning profile")](remote-notifications-in-ios-images/image11.png#lightbox)

이 시점에서 응용 프로그램은 푸시 알림과 함께 작동 하도록 구성 됩니다. 그러나 인증서에는 여전히 몇 가지 단계가 필요 합니다. 이 인증서는 PKCS12 (개인 정보 교환) 인증서가 필요한 PushSharp와 호환 되지 않는 DER 형식입니다. PushSharp에서 사용할 수 있도록 인증서를 변환 하려면 다음 최종 단계를 수행 합니다.

1. **인증서 파일** -로그인을 IOS 프로 비전 포털에 다운로드 하 고 인증서 탭을 선택한 후 올바른 프로 비전 프로필과 연결 된 인증서를 선택 하 고 **다운로드** 를 선택 합니다.
1. 키 **집합 액세스 열기** -응용 프로그램은 OS X의 암호 관리 시스템에 대 한 GUI 인터페이스입니다.
1. **인증서 가져오기** -인증서이 아직 설치 되지 않은 경우 **파일 ...** 키 집합 액세스 메뉴에서 항목을 가져옵니다. 위에서 내보낸 인증서로 이동 하 여 선택 합니다.
1. **인증서 내보내기** -연결 된 개인 키가 표시 되도록 인증서를 확장 하 고 키를 마우스 오른쪽 단추로 클릭 한 다음 내보내기를 선택 합니다. 내보낸 파일의 파일 이름 및 암호를 입력 하 라는 메시지가 표시 됩니다.

이 시점에서 인증서를 사용 하 여 작업을 수행 합니다. IOS 응용 프로그램에 서명 하 고 해당 인증서를 서버 응용 프로그램의 PushSharp에서 사용할 수 있는 형식으로 변환 하는 데 사용 되는 인증서를 만들었습니다. 다음으로 iOS 응용 프로그램이 APNS와 상호 작용 하는 방식을 살펴보겠습니다.

## <a name="registering-with-apns"></a>APNS에 등록

IOS 응용 프로그램이 원격 알림을 받을 수 있으려면 먼저 APNS에 등록 해야 합니다. APNS는 고유한 장치 토큰을 생성 하 고 iOS 응용 프로그램에 반환 합니다. 그러면 iOS 응용 프로그램은 장치 토큰을 가져온 다음 응용 프로그램 서버에 등록 합니다. 이러한 상황이 발생 하면 등록이 완료 되 고 응용 프로그램 서버가 모바일 장치에 알림을 푸시할 수 있습니다.

이론적으로 장치 토큰은 iOS 응용 프로그램이 APNS를 사용 하 여 자체적으로 등록 될 때마다 변경 될 수 있지만 실제로는 이런 경우가 발생 하지 않습니다. 최적화로 응용 프로그램은 최신 장치 토큰을 캐시 하 고 변경 되는 경우에만 응용 프로그램 서버를 업데이트할 수 있습니다. 다음 다이어그램에서는 장치 토큰을 등록 하 고 가져오는 과정을 보여 줍니다.

 ![](remote-notifications-in-ios-images/image12.png "This diagram illustrates the process of registration and obtaining a device token")

APNS를 사용 하 여 등록은 현재 `UIApplication` 개체에서 `RegisterForRemoteNotificationTypes`를 호출 하 여 응용 프로그램 대리자 클래스의 `FinishedLaunching` 메서드에서 처리 됩니다. IOS 응용 프로그램이 APNS에 등록 되 면 수신 하려는 원격 알림 유형도 지정 해야 합니다. 이러한 원격 알림 유형은 열거형 `UIRemoteNotificationType`선언 됩니다. 다음 코드 조각은 iOS 응용 프로그램이 원격 경고 및 배지 알림을 받도록 등록할 수 있는 방법의 예입니다.

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
    var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());

    UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
    UIApplication.SharedApplication.RegisterForRemoteNotifications ();
} else {
    UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
    UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
}
```

APNS 등록 요청은 백그라운드에서 발생 합니다. 응답이 수신 되 면 iOS는 `AppDelegate` 클래스에서 `RegisteredForRemoteNotifications` 메서드를 호출 하 고 등록 된 장치 토큰을 전달 합니다. 토큰은 `NSData` 개체에 포함 됩니다. 다음 코드 조각에서는 APNS에서 제공한 장치 토큰을 검색 하는 방법을 보여 줍니다.

```csharp
public override void RegisteredForRemoteNotifications (
UIApplication application, NSData deviceToken)
{
    // Get current device token
    var DeviceToken = deviceToken.Description;
    if (!string.IsNullOrWhiteSpace(DeviceToken)) {
        DeviceToken = DeviceToken.Trim('<').Trim('>');
    }

    // Get previous device token
    var oldDeviceToken = NSUserDefaults.StandardUserDefaults.StringForKey("PushDeviceToken");

    // Has the token changed?
    if (string.IsNullOrEmpty(oldDeviceToken) || !oldDeviceToken.Equals(DeviceToken))
    {
        //TODO: Put your own logic here to notify your server that the device token has changed/been created!
    }

    // Save new device token
    NSUserDefaults.StandardUserDefaults.SetString(DeviceToken, "PushDeviceToken");
}
```

일부 이유로 등록이 실패 하는 경우 (예: 장치가 인터넷에 연결 되지 않은 경우) iOS는 응용 프로그램 대리자 클래스에서 `FailedToRegisterForRemoteNotifications`를 호출 합니다. 다음 코드 조각에서는 등록에 실패 했다는 알림을 사용자에 게 표시 하는 방법을 보여 줍니다.

```csharp
public override void FailedToRegisterForRemoteNotifications (UIApplication application , NSError error)
{
    new UIAlertView("Error registering push notifications", error.LocalizedDescription, null, "OK", null).Show();
}
```

### <a name="device-token-housekeeping"></a>장치 토큰 정리

장치 토큰은 만료 되거나 시간이 지남에 따라 변경 됩니다. 이러한 이유로 서버 응용 프로그램은 일부 집을 치료 하 고 만료 되거나 변경 된 토큰을 제거 해야 합니다. 응용 프로그램이 만료 된 토큰이 있는 장치에 푸시 알림을 보내면 APNS에서 만료 된 토큰을 기록 하 고 저장 합니다. 그런 다음 서버는 APNS를 쿼리하여 만료 된 토큰을 확인할 수 있습니다.

*피드백 서비스* 를 제공 하는 데 사용 되는 APNS-푸시 알림을 전송 하기 위해 만든 인증서를 통해 인증 하 고 만료 된 토큰에 대 한 데이터를 다시 전송 하는 HTTPS 끝점입니다. 이는 Apple에서 더 이상 사용 되지 않으며 제거 되었습니다.

대신, 사용자 의견 서비스에서 이전에 보고 한 경우에 대 한 새 HTTP 상태 코드가 있습니다.

> 410-항목에 대 한 장치 토큰이 더 이상 활성 상태가 아닙니다.

또한 새 `timestamp` JSON 데이터 키가 응답 본문에 있습니다.

> : Status 헤더의 값이 410 이면 APNs에서 장치 토큰이 항목에 대해 더 이상 유효 하지 않은 것으로 확인 된 마지막 시간입니다.
>
> 장치에서 이후 타임 스탬프를 포함 하는 토큰을 공급자와 등록할 때까지 알림 푸시를 중지 합니다.

## <a name="summary"></a>요약

이 섹션에서는 iOS의 푸시 알림을 둘러싼 주요 개념을 소개 합니다. APNS (Apple Push Notification Gateway) 서비스의 역할에 대해 설명 했습니다. 그런 다음 APNS에 필수적인 보안 인증서의 생성 및 사용에 대해 설명 했습니다. 마지막으로,이 문서에서는 응용 프로그램 서버에서 *피드백 서비스* 를 사용 하 여 만료 된 장치 토큰 추적을 중지 하는 방법에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [알림-로컬 및 원격 알림 시연 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/notifications)
- [개발자를 위한 로컬 및 푸시 알림](https://developer.apple.com/notifications/)
- [UIApplication](https://docs.microsoft.com/dotnet/api/uikit.uiapplication)
- [UIRemoteNotificationType](https://docs.microsoft.com/dotnet/api/uikit.UIRemoteNotificationType)
