---
title: Xamarin.ios의 대화형 알림 사용자 인터페이스
description: IOS 12를 사용 하면 로컬 및 원격 알림에 대 한 대화형 사용자 인터페이스를 만들 수 있습니다. 이 가이드에서는 Xamarin.ios에서 이러한 기능을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: E3562E1B-E0EF-4C99-9F51-59DE22AFDE46
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/04/2018
ms.openlocfilehash: bc566cf3744b8d6ec05204153b7c731935f98b8a
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68652445"
---
# <a name="interactive-notification-user-interfaces-in-xamarinios"></a>Xamarin.ios의 대화형 알림 사용자 인터페이스

IOS 10에 도입 된 [알림 콘텐츠 확장](~/ios/platform/user-notifications/advanced-user-notifications.md)은 알림에 대 한 사용자 지정 사용자 인터페이스를 만들 수 있도록 합니다. IOS 12부터 알림 사용자 인터페이스에는 단추 및 슬라이더와 같은 대화형 요소가 포함 될 수 있습니다.

## <a name="sample-app-redgreennotifications"></a>샘플 앱: RedGreenNotifications

[RedGreenNotifications](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications) 샘플 앱에는 대화형 사용자 인터페이스를 사용 하는 알림 콘텐츠 확장이 포함 되어 있습니다.

이 가이드의 코드 조각은이 샘플에서 제공 됩니다.

## <a name="notification-content-extension-infoplist-file"></a>알림 콘텐츠 확장 정보. info.plist 파일

샘플 앱에서 **RedGreenNotificationsContentExtension** 프로젝트의 **info.plist** 파일에는 다음 구성이 포함 됩니다.

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

다음 기능에 유의 하십시오.

- `UNNotificationExtensionCategory` 배열은 콘텐츠 확장에서 처리 하는 알림 범주의 유형을 지정 합니다.
- 대화형 콘텐츠를 지원 하기 위해 알림 콘텐츠 확장은 `UNNotificationExtensionUserInteractionEnabled` 키를로 `true`설정 합니다.
- 키 `UNNotificationExtensionInitialContentSizeRatio` 는 콘텐츠 확장의 인터페이스에 대 한 초기 높이/너비 비율을 지정 합니다.

## <a name="interactive-interface"></a>대화형 인터페이스

알림 콘텐츠 확장에 대 한 인터페이스를 정의 하는 **Maininterface**는 단일 뷰 컨트롤러를 포함 하는 표준 스토리 보드입니다. 샘플 앱에서 뷰 컨트롤러는 형식이 `NotificationViewController`며 이미지 뷰, 3 개의 단추 및 슬라이더를 포함 합니다. 스토리 보드는 **NotificationViewController.cs**에 정의 된 처리기와 이러한 컨트롤을 연결 합니다.

- **앱 시작** 단추 처리기는 앱을 `PerformNotificationDefaultAction` 시작 하는 `ExtensionContext`에 대 한 작업 메서드를 호출 합니다.

    ```csharp
    partial void HandleLaunchAppButtonTap(UIButton sender)
    {
        ExtensionContext.PerformNotificationDefaultAction();
    }
    ```

    앱에서 사용자 알림 센터 `Delegate` (샘플 앱 `AppDelegate`)는 `DidReceiveNotificationResponse` 메서드의 상호 작용에 응답할 수 있습니다.

    ```csharp
    [Export("userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:")]
    public void DidReceiveNotificationResponse(UNUserNotificationCenter center, UNNotificationResponse response, System.Action completionHandler)
    {
        if (response.IsDefaultAction)
        {
            Console.WriteLine("ACTION: Default");
            // ...
    ```

- 알림 **해제** 단추 처리기는 알림을 `DismissNotificationContentExtension` 닫는 `ExtensionContext`에 대해를 호출 합니다.

    ```csharp
    partial void HandleDismissNotificationButtonTap(UIButton sender)
    {
        ExtensionContext.DismissNotificationContentExtension();
    }
    ```

- 알림 **제거** 단추 처리기는 알림을 해제 하 고 알림 센터에서이를 제거 합니다.

    ```csharp
    partial void HandleRemoveNotificationButtonTap(UIButton sender)
    {
        ExtensionContext.DismissNotificationContentExtension();
        UNUserNotificationCenter.Current.RemoveDeliveredNotifications(new string[] { notification.Request.Identifier });
    }
    ```

- 슬라이더에서 값 변경을 처리 하는 메서드는 알림 인터페이스에 표시 된 이미지의 알파를 업데이트 합니다.

    ```csharp
    partial void HandleSliderValueChanged(UISlider sender)
    {
        Xamagon.Alpha = sender.Value;
    }
    ```

## <a name="related-links"></a>관련 링크

- [샘플 앱-RedGreenNotifications](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [Xamarin.ios의 사용자 알림 프레임 워크](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [사용자 알림의 새로운 기능 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [모범 사례 및 사용자 알림의 새로운 기능 (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [풍부한 알림 (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/817/)
- [원격 알림 생성 (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
