---
title: IOS에 대 한 푸시 알림
description: 이 섹션은 iOS에 푸시 알림을 설명 합니다. Apple 푸시 알림 게이트웨이 서비스 및 iOS 응용 프로그램에 게시 알림에서 수행 하는 역할을 소개 합니다. 이 작업에서는 푸시 알림을 사용 하도록 설정 하 고 논의 하는 데 필요한 보안 인증서를 만드는 방법을 설명 합니다. 마지막으로이 섹션은 응용 프로그램 서버에 클라이언트 모바일 장치를 추적 하기 위해 수행 해야 하는 관리 작업 중 일부 설명 합니다.
ms.prod: xamarin
ms.assetid: 64B3BE6A-A3E2-4B1B-95ED-02D27A8FDAAC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 3a86ce5e61576faec41b5fcddf899d731d2cc57a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="push-notifications-in-ios"></a>IOS에 대 한 푸시 알림

_이 섹션은 iOS에 푸시 알림을 설명 합니다. Apple 푸시 알림 게이트웨이 서비스 및 iOS 응용 프로그램에 게시 알림에서 수행 하는 역할을 소개 합니다. 이 작업에서는 푸시 알림을 사용 하도록 설정 하 고 논의 하는 데 필요한 보안 인증서를 만드는 방법을 설명 합니다. 마지막으로이 섹션은 응용 프로그램 서버에 클라이언트 모바일 장치를 추적 하기 위해 수행 해야 하는 관리 작업 중 일부 설명 합니다._

> [!IMPORTANT]
> IOS 9에 관련 된이 섹션의 정보 및 prior 되지 않았습니다 여기 이전 iOS 버전을 지원 하도록 합니다. IOS 10 이상에서 참조 하십시오는 [사용자 알림 프레임 워크 가이드](~/ios/platform/user-notifications/index.md) iOS 장치에서 로컬 및 원격 알림 지원에 대 한 합니다.

푸시 알림을 간략하게 유지 해야 하 고만 업데이트에 대 한 서버 응용 프로그램을 문의 해야 해당 모바일 응용 프로그램에 알리기 위해 충분 한 데이터를 포함 합니다. 예를 들어 새 전자 메일 도착, 서버 응용 프로그램이 새 전자 메일 도착 한 모바일 응용 프로그램에 알리는 것입니다. 알림 자체는 새 전자 메일을 포함 되지 않을 것입니다. 모바일 응용 프로그램을 검색 합니다는 새 전자 메일 서버에서 적절 한 되었을 때

IOS에서 알림을 푸시의 가운데에는 *푸시 알림 게이트웨이 서비스 APNS (Apple)*합니다. IOS 장치에 응용 프로그램 서버에서 라우팅 알림 담당 하는 Apple에서 제공한 서비스입니다.
다음 그림에서는 iOS에 대 한 푸시 알림 토폴로지를 보여 줍니다: ![ ] (remote-notifications-in-ios-images/image4.png "iOS에 대 한 푸시 알림 토폴로지를 보여 주는이 이미지")

원격 알림 자체는 JSON 형식의 형식을 준수 하는 문자열에 지정 된 프로토콜 및 [The 알림 페이로드](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CreatingtheNotificationPayload.html#//apple_ref/doc/uid/TP40008194-CH10-SW1) 의 섹션은 [로컬 및 푸시 알림 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)에 [iOS 개발자 설명서](https://developer.apple.com/devcenter/ios/index.action)합니다.

Apple APNS의 두 가지 환경 유지 관리:는 *샌드박스* 및 *프로덕션* 환경입니다. 샌드박스 환경 개발 단계 중 테스트 하기 위해 하며에서 찾을 수 있습니다 `gateway.sandbox.push.apple.com` 2195 포트 TCP에 있습니다. 프로덕션 환경 배포 된 및에서 찾을 수 있는 응용 프로그램에서 사용 하는 것은 `gateway.push.apple.com` 2195 포트 TCP에 있습니다.

## <a name="requirements"></a>요구 사항

푸시 알림 APNS 아키텍처에서 지시 하는 다음 규칙을 준수 해야 합니다.

-  **메시지 제한 256 바이트** -알림 메시지의 전체 메시지 크기는 256 바이트를 넘지 않아야 합니다.
-  **수신 확인 하지** -APNS 메시지 받는 사람된에 게 적용 하는 모든 알림 사용 하 여 보낸 사람을 제공 하지 않습니다. 장치에 연결할 수 없습니다 여러 순차적 알림을 보내는 경우 가장 최근의 제외한 모든 알림이 손실 됩니다. 가장 최근의 알림만 장치에 배달 됩니다.
-  **각 응용 프로그램에 보안 인증서가 필요한** -SSL을 통해 APNS와의 통신을 수행 해야 합니다.


## <a name="creating-and-using-certificates"></a>만들기 및 인증서 사용

각각의 이전 단원에서 설명한 환경 자신의 인증서가 필요 합니다. 이 섹션에는 인증서를 만들고 프로 비전 프로필을 연결 하 PushSharp와 함께 사용할 개인 정보 교환 인증서를 가져온 후 하는 방법을 설명 합니다.

1.  만들 인증서는 iOS 프로 비전 포털으로 이동 Apple 웹 사이트 (왼쪽에서의 앱 Id 메뉴 항목 표시) 다음 스크린샷에 표시 된 것 처럼:

    [![](remote-notifications-in-ios-images/image5new.png "IOS 사과 웹 사이트에서 프로 비전 포털")](remote-notifications-in-ios-images/image5new.png#lightbox)

2.  다음으로 응용 프로그램 ID 섹션으로 이동 하 고 다음 스크린샷에 표시 된 대로 새 응용 프로그램 ID를 만들려면:

    [![](remote-notifications-in-ios-images/image6new.png "앱 Id 섹션을 찾아 새 앱 ID를 만들려면")](remote-notifications-in-ios-images/image6new.png#lightbox)

3.  클릭는 **+** 단추를 수 있습니다는 응용 프로그램 ID에 대 한 설명과 번들 식별자를 입력 하려면 다음 스크린샷에 표시 된 대로:

    [![](remote-notifications-in-ios-images/image7new.png "응용 프로그램 ID에 대 한 설명 및 번들 식별자 입력")](remote-notifications-in-ios-images/image7new.png#lightbox)

4. 선택 되어 있는지 확인 **명시적 앱 ID** 및 번들 식별자로 끝나지 않습니다는 `*` 합니다. 여러 응용 프로그램에 대 한 좋은 식별자를 만들어집니다 및 단일 응용 프로그램에 대 한 푸시 알림 인증서 이어야 합니다.

1. 앱 서비스에서 선택 **푸시 알림을**:

    [![](remote-notifications-in-ios-images/image8new.png "푸시 알림 선택")](remote-notifications-in-ios-images/image8new.png#lightbox)

2. 키를 누릅니다 **전송** 새 앱 ID의 등록을 확인 하려면:

    [![](remote-notifications-in-ios-images/image9new.png "새 앱 ID의 등록을 확인 합니다.")](remote-notifications-in-ios-images/image9new.png#lightbox)

3.  다음으로 만들어야 합니다 인증서에 대 한 응용 프로그램 id입니다. 왼쪽 탐색 영역에서 찾은 **인증서 > 모든** 선택 하 고는 `+` 다음 스크린샷에 표시 된 것 처럼 단추:

    [![](remote-notifications-in-ios-images/image10new.png "응용 프로그램 ID에 대 한 인증서를 만듭니다.")](remote-notifications-in-ios-images/image8.png#lightbox)

4.  개발 또는 프로덕션 인증서를 사용 하도록 여부를 선택 합니다.

    [![](remote-notifications-in-ios-images/image11new.png "개발 또는 프로덕션 인증서를 선택 합니다.")](remote-notifications-in-ios-images/image11new.png#lightbox)

5. 한 다음 방금 작성 한 새 앱 ID를 선택 합니다.

    [![](remote-notifications-in-ios-images/image12new.png "방금 만든 새 응용 프로그램 ID 선택")](remote-notifications-in-ios-images/image12new.png#lightbox)

6.  이 만드는 과정을 통해 사용 하는 명령에 표시 됩니다는 *인증서 서명 요청* 를 사용 하는 **키 집합 접근** mac 응용 프로그램

7.  인증서를 만든 후, apns 등록할 수 있도록 응용 프로그램에 서명 하려면 빌드 프로세스의 일부로 사용 되어야 합니다. 그러려면, 작성 및 인증서를 사용 하는 프로 비전 프로필을 설치 합니다.

8.  프로비저닝 프로필 개발을 만들려면로 이동는 **프로 비전 프로필** 섹션 및 만들에서는 방금 만든 응용 프로그램 Id를 사용 하 여 단계를 수행 합니다.

9.  프로비저닝 프로필을 만든 후에 개방 **Xcode 구성 도우미** 고 새로 고칩니다. 프로비저닝 프로필을 만든 경우 나타나지 않습니다를 iOS 프로비저닝 포털에서에서 프로필을 다운로드 하 여 수동으로 가져와야 할 수도 있습니다. 다음 스크린 샷에서 추가 프로 비전 프로필을 가진 구성의 예를 보여 줍니다.

    [![](remote-notifications-in-ios-images/image13new.png "이 스크린 샷 추가 프로 비전 프로필을 가진 구성의 예를 보여 줍니다.")](remote-notifications-in-ios-images/image13new.png#lightbox)

10.  이 시점에서 Xamarin.iOS 프로젝트가 새로 프로 비전 프로필을 만든이 사용 하도록 구성 해야 합니다. 이렇게 **프로젝트 옵션** 대화에서 **iOS 번들 서명** 으로 다음 스크린샷에 표시 된 탭:

    [![](remote-notifications-in-ios-images/image11.png "Xamarin.iOS 프로젝트가 새로 프로 비전 프로필을 만든이 사용 하도록 구성")](remote-notifications-in-ios-images/image11.png#lightbox)



이 시점에서 응용 프로그램이 푸시 알림을 사용 하도록 구성 됩니다. 그러나 추가 단계가 있습니다 여전히 몇 가지 인증서와 함께 필요 합니다. 이 인증서는 DER 형식으로 개인 정보 교환 (PKCS12) 인증서를 요구 하는 PushSharp와 호환 되지 않습니다. PushSharp에서 사용할 수 있도록 인증서 변환할 최종 이러한 단계를 수행 합니다.

1.  **인증서 파일을 다운로드** -인증서 탭을 선택한 iOS 프로 비전 포털에 로그인을 연결 된 올바른 프로비저닝 프로필 및 선택한 인증서 선택 **다운로드** 합니다.
1.  **키 집합 접근을 열고** -이 응용 프로그램은 OS X에서 암호 관리 시스템을 GUI 인터페이스입니다.
1.  **인증서를 가져올** 인증서는 이미 설치 되어 있지 않으면,- **파일... 항목 가져오기** 키 집합 접근 메뉴에서 합니다. 위의 내보낸 인증서를 찾아서 선택 합니다.
1.  **인증서 내보내기** -하므로 연결 된 개인 키를 볼 수 인증서 확장, 키를 마우스 오른쪽 단추로 클릭 하 고 내보내기를 선택 합니다. 파일 이름 및 내보낸된 파일에 대 한 암호를 묻는 메시지가 나타납니다.

이 시점에서 우리는 인증서와 함께 수행 됩니다. IOS 응용 프로그램에 서명 하는 데 사용할 인증서를 만들었거나 했으며 해당 인증서는 서버 응용 프로그램에서 PushSharp 함께 사용할 수 있는 형식으로 변환 합니다. 다음 APNS와 iOS 응용 프로그램 상호 작용 하는 방법에 대해 살펴보겠습니다.

## <a name="registering-with-apns"></a>Apns 등록

IOS 하기 전에 응용 프로그램 apns 등록 해야 하는 원격 알림을 받을 수 있습니다. APNS는 고유한 장치 토큰을 생성 되 고 있는 iOS 응용 프로그램에 반환 됩니다. IOS 응용 프로그램 다음 장치 토큰을 수행 하 고 응용 프로그램 서버에 자체 등록 됩니다. 이 모든 등록이 완료 되 면 하 고 응용 프로그램 서버에서 모바일 장치에 알림을 푸시할 수 있지만 합니다.

이론적으로, 장치 토큰 실제로이 문제가 발생 하지는 종종 있지만 iOS 응용 프로그램에 자동으로 등록 APNS, 때마다를 변경할 수 있습니다. 최적화 방법으로 응용 프로그램 가장 최근의 장치 토큰을 캐시 하 고이 인해 변경만 응용 프로그램 서버를 업데이트 합니다. 다음 다이어그램은 등록 및 장치 토큰을 가져오려면 프로세스를 보여 줍니다.

 ![](remote-notifications-in-ios-images/image12.png "이 다이어그램에서는 등록 및 장치 토큰을 가져오려면 프로세스를 보여 줍니다.")

Apns 등록에서 처리 되는 `FinishedLaunching` 메서드를 호출 하 여 응용 프로그램 대리자 클래스의 `RegisterForRemoteNotificationTypes` 현재 `UIApplication` 개체입니다. APNS와 iOS 응용 프로그램을 등록 하는 경우 어떤 유형의 원격 알림 것을 수신 하겠다고 지정 해야 합니다. 이러한 원격 알림 유형을 열거에 선언 된 `UIRemoteNotificationType`합니다. 다음 코드 조각은 원격 경고 및 배지 알림을 수신 하도록 iOS 응용 프로그램 수 등록 하는 방법의 예입니다.

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

-백그라운드에서 발생 하는 APNS 등록 요청 응답 수신 되 면 iOS에서는 메서드를 호출 `RegisteredForRemoteNotifications` 에 `AppDelegate` 클래스 및 등록 된 장치 토큰을 전달 합니다. 토큰에 포함 됩니다는 `NSData` 개체입니다. 다음 코드 조각 장치 토큰을 검색 하는 방법을 제공 하는 APNS를 보여 줍니다.

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

IOS를 호출 합니다 (예: 장치에 연결 되지 않은 인터넷) 어떤 이유로 등록에 실패 하는 경우 `FailedToRegisterForRemoteNotifications` 대리자 클래스는 응용 프로그램에 있습니다. 다음 코드 조각 등록 실패 했음을 알리는 사용자에 게 경고를 표시 하는 방법을 보여 줍니다.

```csharp
public override void FailedToRegisterForRemoteNotifications (UIApplication application , NSError error)
{
    new UIAlertView("Error registering push notifications", error.LocalizedDescription, null, "OK", null).Show();
}
```

### <a name="device-token-housekeeping"></a>장치 토큰 정리 작업

장치 토큰 만료 되거나 시간이 지남에 따라 변경 됩니다. 이 인해 서버 응용 프로그램 집 정리 작업을 수행한 및 이러한 만료 되거나 변경 된 토큰을 제거 해야 합니다. 응용 프로그램으로 만료 된 토큰을가 하는 장치에 푸시 알림을 보내면 APNS 기록 되 고 해당 만료 된 토큰에 저장 됩니다. 서버 어떤 토큰 만료 알아보려면 APNS를 쿼리하여 다음 수 있습니다.

APNS 제공 하는 데는 *피드백 서비스* -데이터를 보내는 푸시 알림 및 보냅니다 백 어떤 토큰 만료에 대 한 생성 된 인증서를 통해 인증 하는 HTTPS 끝점입니다. 이 Apple에 의해 사용 되지 않는 되었으며 제거 합니다.

대신, 피드백 서비스에서 이전에 보고 된 사례에 대 한 새 HTTP 상태 코드가 있습니다.

> 410-장치 토큰은 항목에 대 한 활성 더 이상.

또한 새 `timestamp` 데이터 키 JSON 응답 본문에 포함 됩니다.

> 경우에 있는 값의: 상태 헤더는 410,이 키의 값은 마지막으로는 APNs 장치 토큰의 항목에 대 한 유효한는 더 이상 확인 합니다.
>
> 장치 공급 업체 타임 스탬프가 토큰 등록 될 때까지 알림을 푸시할 중지 합니다.

## <a name="summary"></a>요약

이 섹션 iOS에서 푸시 알림을 둘러싼 주요 개념을 소개 합니다. 이 역할의 푸시 알림 게이트웨이 서비스 APNS (Apple)을 설명합니다. 그런 다음 생성 및 APNS에 꼭 필요한 보안 인증서의 사용을 다룹니다. 응용 프로그램 서버 사용 방법에 대 한 설명을와이 문서 완료 하는 마지막으로 *피드백 서비스* 추적을 중지 하려면 장치 토큰을 만료 합니다.


## <a name="related-links"></a>관련 링크

- [알림-로컬 및 원격 알림 (샘플)를 시연](https://developer.xamarin.com/samples/monotouch/Notifications/)
- [로컬 및 개발자에 대 한 알림을 푸시](https://developer.apple.com/notifications/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UIRemoteNotificationType](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIRemoteNotificationType)
