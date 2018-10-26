---
title: Xamarin.iOS에서 대화형 알림 사용자 인터페이스
description: IOS 12 사용 하 여 로컬 및 원격 알림이 대 한 대화형 사용자 인터페이스를 만들 가능성이 있습니다. 이 가이드에서는 Xamarin.iOS를 사용 하 여 이러한 기능을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: E3562E1B-E0EF-4C99-9F51-59DE22AFDE46
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 9/4/2018
ms.openlocfilehash: 5a3a737360f898c2638bc5431cb8f8fc4882f3b0
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131435"
---
# <a name="interactive-notification-user-interfaces-in-xamarinios"></a>Xamarin.iOS에서 대화형 알림 사용자 인터페이스

[알림 콘텐츠 확장](~/ios/platform/user-notifications/advanced-user-notifications.md), 알림에 대 한 사용자 지정 사용자 인터페이스를 만들 수 있도록 iOS 10에에서 도입 된, 합니다. IOS 12부터 알림 사용자 인터페이스는 단추 및 슬라이더와 같은 대화형 요소를 포함할 수 있습니다.

## <a name="sample-app-redgreennotifications"></a>샘플 앱: RedGreenNotifications

합니다 [RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications) 샘플 앱에 대화형 사용자 인터페이스를 사용 하 여 알림 콘텐츠 확장을 포함 합니다.

이 샘플에서이 가이드에서 코드 조각을 제공 됩니다.

## <a name="notification-content-extension-infoplist-file"></a>알림 콘텐츠 확장의 Info.plist 파일

샘플 앱에는 **Info.plist** 파일을 **RedGreenNotificationsContentExtension** 프로젝트에는 다음 구성이 포함 되어 있습니다.:

```xml
<!-- ... -->
<key>NSExtension</key>
<dict>
    <key>NSExtensionAttributes</key>
    <dict>
        <key>UNNotificationExtensionCategory</key>
        <array>
            <string>red-category</string>
            <string>green-category</string>
        </array>
        <key>UNNotificationExtensionUserInteractionEnabled</key>
        <true/>
        <key>UNNotificationExtensionDefaultContentHidden</key>
        <true/>
        <key>UNNotificationExtensionInitialContentSizeRatio</key>
        <real>0.6</real>
    </dict>
    <key>NSExtensionMainStoryboard</key>
    <string>MainInterface</string>
    <key>NSExtensionPointIdentifier</key>
    <string>com.apple.usernotifications.content-extension</string>
    <key></key>
    <true/>
</dict>
<!-- ... -->
```

다음 기능은 note:

- `UNNotificationExtensionCategory` 알림 범주의 종류를 지정 하는 배열 콘텐츠 확장의 핸들입니다.
- 대화형 콘텐츠를 지원 하기 위해 알림 콘텐츠 확장을 설정 합니다 `UNNotificationExtensionUserInteractionEnabled` 키를 `true`입니다.
- `UNNotificationExtensionInitialContentSizeRatio` 키 콘텐츠 확장의 인터페이스에 대 한 초기 높이/너비 비율을 지정 합니다.

## <a name="interactive-interface"></a>대화형 인터페이스

**MainInterface.storyboard**, 단일 뷰 컨트롤러를 포함 하는 표준 스토리 보드는 알림 콘텐츠 확장에 대 한 인터페이스를 정의 하는 합니다. 샘플 앱에서 뷰 컨트롤러는 형식의 `NotificationViewController`, 이미지 뷰를, 세 가지 단추 및 슬라이더를 포함 합니다. 스토리 보드에 정의 된 처리기를 사용 하 여 이러한 컨트롤을 연결 **NotificationViewController.cs**:

- 합니다 **앱 시작** 처리기 호출 단추를 `PerformNotificationDefaultAction` 대 한 작업 메서드 `ExtensionContext`, 앱을 시작 하는:

    ```csharp
    partial void HandleLaunchAppButtonTap(UIButton sender)
    {
        ExtensionContext.PerformNotificationDefaultAction();
    }
    ```

    앱에서 사용자 알림 센터의 `Delegate` (샘플 앱에는 `AppDelegate`)에서 상호 작용에 응답할 수는 `DidReceiveNotificationResponse` 메서드:

    ```csharp
    [Export("userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:")]
    public void DidReceiveNotificationResponse(UNUserNotificationCenter center, UNNotificationResponse response, System.Action completionHandler)
    {
        if (response.IsDefaultAction)
        {
            Console.WriteLine("ACTION: Default");
            // ...
    ```

- 합니다 **해제 알림** 처리기 호출 단추 `DismissNotificationContentExtension` 에서 `ExtensionContext`, 알림을 닫습니다는:

    ```csharp
    partial void HandleDismissNotificationButtonTap(UIButton sender)
    {
        ExtensionContext.DismissNotificationContentExtension();
    }
    ```

- 합니다 **제거 알림** 단추 처리기는 알림을 해제 하 고 알림 센터에서 제거 합니다.

    ```csharp
    partial void HandleRemoveNotificationButtonTap(UIButton sender)
    {
        ExtensionContext.DismissNotificationContentExtension();
        UNUserNotificationCenter.Current.RemoveDeliveredNotifications(new string[] { notification.Request.Identifier });
    }
    ```

- 슬라이더의 값 변경 내용을 처리 하는 메서드는 알림 인터페이스에 표시할 이미지의 알파를 업데이트 합니다.

    ```csharp
    partial void HandleSliderValueChanged(UISlider sender)
    {
        Xamagon.Alpha = sender.Value;
    }
    ```

## <a name="related-links"></a>관련 링크

- [샘플 앱 – RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications)
- [Xamarin.iOS에서 사용자 알림 프레임 워크](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [사용자 알림 (WWDC 2018)의 새로운 기능](https://developer.apple.com/videos/play/wwdc2018/710/)
- [모범 사례 및 사용자 알림 (WWDC 2017)의 새로운 기능](https://developer.apple.com/videos/play/wwdc2017/708/)
- [다양 한 알림 (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/817/)
- [원격 알림을 (Apple) 생성](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
