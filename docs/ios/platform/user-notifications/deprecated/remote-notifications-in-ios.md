---
title: IOS의 푸시 알림
description: 이 문서에는 iOS 9 및 이전 버전에서 푸시 알림을 사용 하는 방법을 설명 합니다. 인증서를 사용 하 여 푸시 알림 게이트웨이 서비스 (APNS (Apple), 등록에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 64B3BE6A-A3E2-4B1B-95ED-02D27A8FDAAC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f11f5d1cbde0f5eae27215af8eb6544be46c0206
ms.sourcegitcommit: ee66db647ae9d94b54b1c5d9093075a620d0c6b4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2018
ms.locfileid: "39654817"
---
# <a name="push-notifications-in-ios"></a>IOS의 푸시 알림

> [!IMPORTANT]
> IOS 9에 관련 된이 섹션의 정보 및 prior 되지 않았습니다 여기 이전 iOS 버전을 지원 하도록 합니다. IOS 10 이상에서 참조 하십시오 합니다 [사용자 알림 프레임 워크 가이드](~/ios/platform/user-notifications/index.md) iOS 장치에서 로컬 및 원격 알림을 지원 하기 위해.

푸시 알림을 간략하게 유지 해야 하 고만 업데이트에 대 한 서버 응용 프로그램에 문의 해야 하는 모바일 응용 프로그램에 알리기 위해 충분 한 데이터를 포함 합니다. 예를 들어, 새 전자 메일이 도착 하는 경우에 서버 응용 프로그램을 새 이메일이 도착 하는 모바일 응용 프로그램을 알립니다만. 알림 자체는 새 전자 메일을 포함 하지 않으므로 합니다. 모바일 응용 프로그램을 검색 새 전자 메일 서버에서 적절 한 때

IOS에서 알림을 푸시의 중심이 되는 *Apple 푸시 알림 게이트웨이 서비스 (APNS)* 합니다. IOS 장치에 응용 프로그램 서버에서 라우팅 알림을 담당 하는 Apple에서 제공 하는 서비스입니다.
다음 그림에서는 iOS에 대 한 푸시 알림 토폴로지를 보여 줍니다: ![](remote-notifications-in-ios-images/image4.png "iOS에 대 한 푸시 알림 토폴로지를 보여 주는이 이미지")

원격 알림 자체는 JSON 형식을 준수 하는 문자열 형식 및 프로토콜에 지정 된 [The 알림 페이로드](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CreatingtheNotificationPayload.html#//apple_ref/doc/uid/TP40008194-CH10-SW1) 섹션을 [로컬 및 푸시 알림 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)에 [iOS 개발자 설명서](https://developer.apple.com/devcenter/ios/index.action)합니다.

Apple APNS의 두 가지 환경 유지 관리:는 *샌드박스* 와 *프로덕션* 환경입니다. 샌드박스 환경 개발 단계 중 테스트를 위한 것 및에서 찾을 수 있습니다 `gateway.sandbox.push.apple.com` TCP 포트에서 2195 합니다. 프로덕션 환경에서 찾을 수 있습니다 하 고 배포 된 응용 프로그램에서 사용 될 것 `gateway.push.apple.com` TCP 포트에서 2195 합니다.

## <a name="requirements"></a>요구 사항

푸시 알림 아키텍처 APNS의 지시 하는 다음 규칙을 준수 해야 합니다.

-  **256 바이트 메시지 제한** -알림의 전체 메시지 크기 256 바이트를 초과할 수 없습니다.
-  **수신 확인 하지 않습니다** -APNS 하는 메시지를 받는 사람된에 게 모든 알림 보낸 사람을 제공 하지 않습니다. 장치를 연결할 수 없는 여러 순차 알림이 전송 되 고 하는 경우 가장 최근의 제외한 모든 알림이 손실 됩니다. 가장 최신 알림만 장치로 배달 됩니다.
-  **각 응용 프로그램 보안 인증서가 필요한** -SSL을 통해 APNS와의 통신을 수행 해야 합니다.


## <a name="creating-and-using-certificates"></a>인증서 만들기 및 사용

이전 섹션에 언급 된 환경의 각 자체 인증서가 필요 합니다. 이 섹션에서는 인증서를 만들고, 프로 비전 프로필을 연결, PushSharp 사용에 대 한 개인 정보 교환 인증서를 가져오려면 다음 하는 방법을 설명 합니다.

1.  만들려는 인증서를 이동할 iOS 프로 비전 포털 Apple 웹 사이트에서 (이때 왼쪽의 앱 Id 메뉴 항목) 다음 스크린샷에 표시 된 대로.

    [![](remote-notifications-in-ios-images/image5new.png "IOS Apple 웹 사이트에서 프로 비전 포털")](remote-notifications-in-ios-images/image5new.png#lightbox)

2.  앱 ID 섹션으로 이동한 다음으로, 다음 스크린샷에 표시 된 것 처럼 새 앱 ID를 만듭니다.

    [![](remote-notifications-in-ios-images/image6new.png "앱 Id 섹션으로 이동 하 고 새 앱 ID 만들기")](remote-notifications-in-ios-images/image6new.png#lightbox)

3.  클릭 합니다 **+** 단추 수 앱 ID에 대 한 설명 및 번들 식별자를 입력 하는 다음 스크린샷에 표시 된 대로:

    [![](remote-notifications-in-ios-images/image7new.png "앱 ID에 대 한 설명 및 번들 식별자 입력")](remote-notifications-in-ios-images/image7new.png#lightbox)

4. 선택 되어 있는지 확인 **명시적 앱 ID** 번들 식별자로 끝나지 않습니다 하 고는 `*` 합니다. 이 경우 여러 응용 프로그램에 적합 하는 식별자 만들어지고 단일 응용 프로그램에 대 한 푸시 알림 인증서 여야 합니다.

1. App Services 아래에서 선택 **푸시 알림**:

    [![](remote-notifications-in-ios-images/image8new.png "푸시 알림 선택")](remote-notifications-in-ios-images/image8new.png#lightbox)

2. 키를 누릅니다 **제출** 새 앱 ID의 등록을 확인 하려면:

    [![](remote-notifications-in-ios-images/image9new.png "새 앱 ID 등록 확인")](remote-notifications-in-ios-images/image9new.png#lightbox)

3.  앱 ID에 대 한 인증서를 만들어야 합니다는 다음으로, 왼쪽 탐색에서 찾은 **인증서 > 모든** 선택 하 고는 `+` 단추, 다음 스크린샷에 표시 된 대로:

    [![](remote-notifications-in-ios-images/image10new.png "앱 ID에 대 한 인증서 만들기")](remote-notifications-in-ios-images/image8.png#lightbox)

4.  개발 또는 프로덕션 인증서를 사용 하려는 지 여부를 선택 합니다.

    [![](remote-notifications-in-ios-images/image11new.png "개발 또는 프로덕션 인증서를 선택 합니다.")](remote-notifications-in-ios-images/image11new.png#lightbox)

5. 선택한 다음 방금 만든 새 앱 ID:

    [![](remote-notifications-in-ios-images/image12new.png "방금 만든 새 앱 ID 선택")](remote-notifications-in-ios-images/image12new.png#lightbox)

6.  만드는 과정을 통해 사용 하는 명령 표시 됩니다는 *인증서 서명 요청* 를 사용 하는 **Keychain Access** mac에 응용 프로그램

7.  인증서를 만든 했으므로 APNs를 사용 하 여 등록할 수 있도록 응용 프로그램 서명에 빌드 프로세스의 일부로 사용 되어야 합니다. 이렇게 하려면 인증서를 사용 하는 프로 비전 프로필 만들기 및 설치 합니다.

8.  로 이동 하는 개발 프로 비전 프로필을 만들려면 합니다 **프로 비전 프로필** 섹션 및 방금 만든 앱 Id를 사용 하 여를 만드는 단계를 수행 합니다.

9.  프로 비전 프로필을 만든 후 열어 **Xcode 구성 도우미** 고 새로 고칩니다. 프로 비전 프로필을 만든 경우는 것은 iOS 프로 비전 포털에서에서 프로필을 다운로드 하 고 수동으로 가져올 필요가 표시 되지 않습니다. 다음 스크린 샷은 추가 프로 비전 프로필을 사용 하 여 구성의 예를 보여 줍니다.  
    [![](remote-notifications-in-ios-images/image13new.png "이 스크린 샷 추가 프로 비전 프로필을 사용 하 여 구성의 예를 보여 줍니다.")](remote-notifications-in-ios-images/image13new.png#lightbox)

10.  이 시점에서 새로 프로 비전 프로필을 만든이 사용 하 여 Xamarin.iOS 프로젝트를 구성 해야 합니다. 이렇게 **프로젝트 옵션** 대화 상자에서 **iOS 번들 서명** 다음 스크린 샷에서 처럼 탭:  
    [![](remote-notifications-in-ios-images/image11.png "새로 프로 비전 프로필을 만든이 사용 하 여 Xamarin.iOS 프로젝트 구성")](remote-notifications-in-ios-images/image11.png#lightbox)

이 시점에서 응용 프로그램 푸시 알림과 작동 하도록 구성 됩니다. 그러나 인증서를 사용 하 여 필요한 몇 단계가 더 계속 됩니다. 이 인증서는 DER 형식으로 개인 정보 교환 (PKCS12) 인증서를 요구 하는 PushSharp와 호환 되지 않습니다. PushSharp에서 사용할 수 있도록 인증서 변환할 최종 단계를 수행 합니다.

1.  **인증서 파일을 다운로드** -인증서 탭을 선택한 iOS Provisioning Portal에 로그인 선택 고 올바른 프로 비전 프로필을 사용 하 여 연결 된 인증서를 선택 합니다 **다운로드** 합니다.
1.  **Keychain Access를 열고** -응용 프로그램은 OS X에서 암호 관리 시스템에 GUI 인터페이스입니다.
1.  **인증서를 가져올** 인증서를 이미 설치 되어 있지 않으면,- **파일... 항목을 가져올** 키 집합 액세스 합니다. 위의 내보낸 인증서를 찾아서 선택 합니다.
1.  **인증서 내보내기** -하므로 연결 된 개인 키를 볼 수 인증서 확장, 키를 마우스 오른쪽 단추로 클릭 하 고 내보내기를 선택 합니다. 파일 이름 및 내보낸된 파일에 대 한 암호에 대 한 라는 메시지가 표시 됩니다.

이 시점에서는 인증서를 사용 하 여 수행 됩니다. IOS 응용 프로그램에 서명 하는 데 사용할 인증서를 생성 하 고 해당 인증서는 서버 응용 프로그램에서 PushSharp를 사용 하 여 사용할 수 있는 형식으로 변환 합니다. 다음 APNS를 사용 하 여 iOS 응용 프로그램 상호 작용 하는 방법에 대해 살펴보겠습니다.

## <a name="registering-with-apns"></a>APNS를 사용 하 여 등록

IOS 하기 전에 응용 프로그램 APNS를 사용 하 여 등록 해야 하는 원격 알림을 받을 수 있습니다. APNS는 고유한 장치 토큰을 생성 하 고 iOS 응용 프로그램에는 반환 됩니다. IOS 응용 프로그램은 다음 장치 토큰을 사용를 다음 응용 프로그램 서버를 사용 하 여 자체를 등록 합니다. 모든 이렇게 되 면, 등록이 완료 되 면 및 응용 프로그램 서버는 모바일 장치에 알림을 푸시할 수 있습니다.

하지만 이론적으로 장치 토큰 실제로이 발생 하지 않습니다는 종종 iOS 응용 프로그램에 자동으로 등록 APNS 때마다를 변경할 수 있습니다. 최적화로 응용 프로그램 가장 최근의 장치 토큰을 캐시 하 고만 해도 변경 하는 경우 응용 프로그램 서버를 업데이트할 수 있습니다. 다음 다이어그램에서는 등록 하 고 장치 토큰을 가져오는 과정을 보여 줍니다.

 ![](remote-notifications-in-ios-images/image12.png "이 다이어그램에서는 등록 및 장치 토큰을 가져오는 프로세스를 보여 줍니다.")

APNS 사용 하 여 등록에서 처리 되는 `FinishedLaunching` 메서드를 호출 하 여 응용 프로그램 대리자 클래스의 `RegisterForRemoteNotificationTypes` 현재 `UIApplication` 개체입니다. APNS를 사용 하 여 iOS 응용 프로그램을 등록할 때 어떤 유형의 원격 알림 하려는 수신 지정 해야 합니다. 이러한 원격 알림 유형을 열거형의 선언 된 `UIRemoteNotificationType`합니다. 다음 코드 조각은 다음과 같습니다. 원격 알림 및 배지 알림을 수신 하도록 iOS 응용 프로그램을 등록 하는 방법의 예

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

-백그라운드에서 발생 하는 APNS 등록 요청 응답 수신 되 면 iOS는 메서드를 호출 `RegisteredForRemoteNotifications` 에 `AppDelegate` 클래스 및 등록 된 장치 토큰을 전달 합니다. 토큰에 포함 될는 `NSData` 개체입니다. 다음 코드 조각은 장치 토큰을 검색 하는 방법을 제공 하는 APNS를 보여 줍니다.

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

IOS를 호출 합니다 (같은 장치는 인터넷에 연결 되지는) 어떤 이유로 등록이 실패 하면, `FailedToRegisterForRemoteNotifications` 응용 프로그램에서 대리자 클래스입니다. 다음 코드 조각은 등록 하지 못한 알리는 사용자에 게 경고를 표시 하는 방법을 보여 줍니다.

```csharp
public override void FailedToRegisterForRemoteNotifications (UIApplication application , NSError error)
{
    new UIAlertView("Error registering push notifications", error.LocalizedDescription, null, "OK", null).Show();
}
```

### <a name="device-token-housekeeping"></a>장치 토큰 관리

장치 토큰은 만료 되거나 시간이 지남에 따라 변경 됩니다. 따라서 서버 응용 프로그램 집 정리 작업을 수행한 및 이러한 만료 되거나 변경 된 토큰을 제거 해야 합니다. 응용 프로그램 만료 된 토큰에 있는 장치에 푸시 알림을 보내면 APNS 기록 되며 해당 만료 된 토큰을 저장 됩니다. 서버는 토큰 만료 알아보려면 APNS를 쿼리하여 다음 할 수 있습니다.

APNS 제공 하는 데는 *피드백 서비스* -푸시 알림 및 전송 데이터를 다시 보내려면 어떤 토큰 만료에 대 한 만들어진 인증서를 통해 인증 하는 HTTPS 끝점입니다. 이 Apple에 사용 중지 되었으며 제거 합니다.

대신는 피드백 서비스에서 이전에 보고 된 경우 새 HTTP 상태 코드:

> 410 장치 토큰-더 이상 활성 항목에 대 한 합니다.

또한 새 `timestamp` 응답 본문에 JSON 데이터 키가 됩니다.

> 경우 값은: 상태 헤더는 410,이 키의 값은 마지막은 APNs는 장치 토큰 항목에 대 한 더 이상 올바르지 확인 합니다.
>
> 장치는 토큰 공급자를 사용 하 여 타임 스탬프가 등록 될 때까지 알림을 푸시하는 것을 중지 합니다.

## <a name="summary"></a>요약

이 섹션에서는 iOS에 푸시 알림을 관련 핵심 개념을 소개 합니다. 역할의 푸시 알림 게이트웨이 서비스 (APNS (Apple) 설명 했습니다. 그런 다음 생성 및 APNS에 필수적인 보안 인증서의 사용을 설명 합니다. 응용 프로그램 서버를 사용 하 여 하는 방법에 대 한 설명과이 문서 완료 하는 마지막 합니다 *피드백 Services* 추적을 중지 하려면 장치 토큰을 만료 합니다.


## <a name="related-links"></a>관련 링크

- [알림-로컬 및 원격 알림이 (샘플)를 보여 줍니다.](https://developer.xamarin.com/samples/monotouch/Notifications/)
- [로컬 및 푸시 알림을 개발자를 위한](https://developer.apple.com/notifications/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UIRemoteNotificationType](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIRemoteNotificationType)
