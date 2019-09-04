---
title: Xamarin.ios의 siri 바로 가기
description: 이 문서에서는 iOS 12에서 Siri 바로 가기를 사용 하는 방법을 설명 합니다. NSUserActivities, 사용자 지정 의도, 음성 바로 가기 할당, 관련 바로 가기 등을 설명 합니다.
ms.prod: xamarin
ms.assetid: 86424F79-3A7D-436E-927D-9A3267DA333B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
ms.openlocfilehash: f0927a6d6d5e3b9db6f203f779fbd50a026ce7e8
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70226573"
---
# <a name="siri-shortcuts-in-xamarinios"></a>Xamarin.ios의 siri 바로 가기

[IOS 10](~/ios/platform/sirikit/index.md)에서 Apple은 SiriKit를 도입 하 여 siri와 상호 작용 하는 메시지, VoIP 호출, 지불, 작업을 수행 하는 등의 작업을 수행할 수 있습니다.

[IOS 11](~/ios/platform/introduction-to-ios11/sirikit.md)에서는 sirikit에서 더 많은 유형의 앱을 지원 하 고 UI 사용자 지정을 위한 유연성을 높일 수 있습니다.

iOS 12는 Siri 바로 가기를 추가 하 여 모든 유형의 앱이 해당 기능을 Siri에 노출할 수 있도록 합니다. Siri는 특정 앱 기반 작업이 사용자와 가장 관련이 있는 경우를 학습 하 고이 정보를 사용 하 여 _바로 가기를_통해 잠재적 작업을 제안 합니다. 바로 가기를 누르거나 음성 명령을 사용 하 여 호출 하면 앱을 열거나 백그라운드 작업을 실행 합니다.

바로 가기를 사용 하 여 일반적인 작업을 수행 하는 사용자의 능력을 가속화 해야 합니다. 대부분의 경우에는 해당 앱을 열지 않아도 됩니다.

## <a name="sample-app-soup-chef"></a>샘플 앱: 수프 Chef

Siri 바로 가기를 더 잘 이해 하려면 [수프 Chef](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-soupchef) 샘플 앱을 살펴보세요. 수프 Chef를 사용 하면 사용자가 허수 수프 식당의 주문을 수행 하 고, 주문 기록을 확인 하 고, Siri와 상호 작용 하 여 수프를 주문할 때 사용할 구를 정의할 수 있습니다.

> [!TIP]
> IOS 12 시뮬레이터 또는 장치에서 수프 Chef을 테스트 하기 전에 바로 가기를 디버그할 때 유용한 다음 두 가지 설정을 사용 하도록 설정 합니다.
>
> - **설정** 앱에서 **개발자 > 최근 바로 가기를 표시**하도록 설정 합니다.
> - **설정** 앱에서 **개발자 > 잠금 화면에서 기부금을 표시**하도록 설정 합니다.
>
> 이러한 디버그 설정을 사용 하면 iOS 잠금 화면 및 검색 화면에서 최근에 만든 (예측 하지 않고) 바로 가기를 쉽게 찾을 수 있습니다.

샘플 앱을 사용 하려면 다음을 수행 합니다.

- IOS 12 시뮬레이터 또는 [장치](#testing-on-device)에서 수프 Chef 샘플 앱을 설치 하 고 실행 합니다.
- 오른쪽 위에 있는 단추를 클릭 하 여 새 주문을 만듭니다. **+**
- 수프 유형을 선택 하 고, 수량 및 옵션을 지정 하 고, **주문 주문**을 누릅니다.
- **주문 기록** 화면에서 새로 만든 주문을 탭 하 여 세부 정보를 확인 합니다.
- 주문 세부 정보 화면 아래쪽에서 **Siri에 추가**를 누릅니다.
- 주문과 연결할 음성 문구를 기록 하 고 **완료**를 누릅니다.
- 수프 Chef를 최소화 하 고, Siri를 호출 하 고, 방금 기록한 음성 구를 사용 하 여 주문을 다시 입력 합니다.
- Siri가 주문을 완료 한 후 수프 Chef를 다시 열고 **주문 기록** 화면에 새 주문이 나열 되는지 확인 합니다.

샘플 앱에서는 다음을 수행 하는 방법을 보여 줍니다.

- [NSUserActivity 바로 가기를 사용 하 여 앱을 엽니다](#using-an-nsuseractivity-shortcut-to-open-an-app).
- [사용자 지정 의도 바로 가기를 사용 하 여 작업을 수행](#using-a-custom-intent-shortcut-to-perform-a-task)합니다.
- 사용자 [지정 의도에 대 한 사용자 인터페이스를 제공](#providing-a-user-interface-for-a-custom-intent)합니다.
- [음성 바로 가기를 만듭니다](#creating-a-voice-shortcut).

## <a name="infoplist-and-entitlementsplist"></a>Info.plist 및 info.plist

수프 Chef 코드에 대해 자세히 살펴보기 전에 **info.plist** 및 **info.plist** 파일을 살펴보세요.

### <a name="infoplist"></a>Info.plist

**SoupChef** 프로젝트의 **Info.plist** 파일은 **번들 식별자** 를로 `com.xamarin.SoupChef`정의 합니다. 이 번들 식별자는이 문서의 뒷부분에서 설명 하는 의도 및 의도 UI 확장의 번들 식별자에 대 한 접두사로 사용 됩니다.

Info.plist 파일에는 다음 항목도 포함 **됩니다** .

```xml
<key>NSUserActivityTypes</key>
<array>
    <string>OrderSoupIntent</string>
    <string>com.xamarin.SoupChef.viewMenu</string>
</array>
```

이 `NSUserActivityTypes` 키/값 쌍은 수프 Chef이 `OrderSoupIntent` [`ActivityType`](xref:Foundation.NSUserActivity.ActivityType) 를 처리 하는 방법 및 [`NSUserActivity`](xref:Foundation.NSUserActivity) "SoupChef"가 있는를 알고 있음을 나타냅니다.

응용 프로그램 자체에 전달 된 활동 및 사용자 지정 의도는 확장과 달리에서 처리 `AppDelegate` 됩니다 [`UIApplicationDelegate`](xref:UIKit.UIApplicationDelegate) ( [`ContinueUserActivity`](xref:UIKit.UIApplicationDelegate.ContinueUserActivity*) 메서드를 통해).

### <a name="entitlementsplist"></a>Entitlements.plist

**SoupChef** 프로젝트의 **info.plist** 파일에는 다음이 포함 됩니다.

```xml
<key>com.apple.security.application-groups</key>
<array>
    <string>group.com.xamarin.SoupChef</string>
</array>
<key>com.apple.developer.siri</key>
<true/>
```

이 구성은 앱에서 "SoupChef" 앱 그룹을 사용 함을 나타냅니다. **SoupChefIntents** app 확장은 동일한 앱 그룹을 사용 하 여 두 프로젝트를 공유할 수 있도록 합니다.[`NSUserDefaults`](xref:Foundation.NSUserDefaults)
데이터

`com.apple.developer.siri` 키는 앱이 siri와 상호 작용 함을 나타냅니다.

> [!NOTE]
> **SoupChef** 프로젝트의 빌드 구성에서는 **사용자 지정 자격** 을 **info.plist**로 설정 합니다.

## <a name="using-an-nsuseractivity-shortcut-to-open-an-app"></a>NSUserActivity 바로 가기를 사용 하 여 앱 열기

특정 콘텐츠를 표시 하는 앱을 여는 바로 가기를 만들려면를 만들어 `NSUserActivity` 바로 가기로 열려는 화면에 대 한 뷰 컨트롤러에 연결 합니다.

### <a name="setting-up-an-nsuseractivity"></a>NSUserActivity 설정

메뉴 화면에서를 `SoupMenuViewController` `NSUserActivity` 만들어 뷰 컨트롤러의 [`UserActivity`](xref:UIKit.UIResponder.UserActivity) 속성에 할당 합니다.

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();
    UserActivity = NSUserActivityHelper.ViewMenuActivity;
}
```

속성을 donates로 설정 하면 작업이 siri로 설정 됩니다. `UserActivity` 이에 대 한 Siri는이 활동이 사용자와 관련 된 시기와 위치에 대 한 정보를 획득 하 고 나중에 더 잘 제안 하기 위해 학습 합니다.

`NSUserActivityHelper`는 **SoupChef** 솔루션의 **SoupKit** 클래스 라이브러리에 포함 된 유틸리티 클래스입니다. 를 만들고 `NSUserActivity` siri 및 검색과 관련 된 다양 한 속성을 설정 합니다.

```csharp
public static string ViewMenuActivityType = "com.xamarin.SoupChef.viewMenu";

public static NSUserActivity ViewMenuActivity {
    get
    {
        var userActivity = new NSUserActivity(ViewMenuActivityType)
        {
            Title = NSBundleHelper.SoupKitBundle.GetLocalizedString("ORDER_LUNCH_TITLE", "View menu activity title"),
            EligibleForSearch = true,
            EligibleForPrediction = true
        };

        var attributes = new CSSearchableItemAttributeSet(NSUserActivityHelper.SearchableItemContentType)
        {
            ThumbnailData = UIImage.FromBundle("tomato").AsPNG(),
            Keywords = ViewMenuSearchableKeywords,
            DisplayName = NSBundleHelper.SoupKitBundle.GetLocalizedString("ORDER_LUNCH_TITLE", "View menu activity title"),
            ContentDescription = NSBundleHelper.SoupKitBundle.GetLocalizedString("VIEW_MENU_CONTENT_DESCRIPTION", "View menu content description")
        };
        userActivity.ContentAttributeSet = attributes;

        var phrase = NSBundleHelper.SoupKitBundle.GetLocalizedString("ORDER_LUNCH_SUGGESTED_PHRASE", "Voice shortcut suggested phrase");
        userActivity.SuggestedInvocationPhrase = phrase;
        return userActivity;
    }
}
```

특히 다음과 같은 사항에 유의 하십시오.

- `EligibleForPrediction` 로`true` 설정 하면 siri가이 활동을 예측 하 여 바로 가기로 표시할 수 있음을 나타냅니다.
- 배열은 iOS 검색 결과 `NSUserActivity` 에 [`CSSearchableItemAttributeSet`](xref:CoreSpotlight.CSSearchableItemAttributeSet) 를 포함 하는 데 사용 되는 표준입니다. [`ContentAttributeSet`](xref:Foundation.NSUserActivity.ContentAttributeSet)
- [`SuggestedInvocationPhrase`](xref:Foundation.NSUserActivity.SuggestedInvocationPhrase)는 바로 가기에 구를 할당할 때 사용자에 게 적합 한 선택 항목으로 Siri가 제안 하는 문구입니다.

### <a name="handling-an-nsuseractivity-shortcut"></a>NSUserActivity 바로 가기 처리

사용자가 호출 `NSUserActivity` 하는 바로 가기를 처리 하려면 iOS 응용 프로그램이 `AppDelegate` 클래스의 `ContinueUserActivity` 메서드를 재정의 하 여 전달 `NSUserActivity` 된 개체의 `ActivityType` 필드를 기반으로 응답 해야 합니다.

```csharp
public override bool ContinueUserActivity(UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    // ...
    else if (userActivity.ActivityType == NSUserActivityHelper.ViewMenuActivityType)
    {
        HandleUserActivity();
        return true;
    }
    // ...
}
```

이 메서드는 `HandleUserActivity`를 호출 하 여 메뉴 화면에 segue를 찾고 호출 합니다.

```csharp
void HandleUserActivity()
{
    var rootViewController = Window?.RootViewController as UINavigationController;
    var orderHistoryViewController = rootViewController?.ViewControllers?.FirstOrDefault() as OrderHistoryTableViewController;
    if (orderHistoryViewController is null)
    {
        Console.WriteLine("Failed to access OrderHistoryTableViewController.");
        return;
    }
    var segue = OrderHistoryTableViewController.SegueIdentifiers.SoupMenu;
    orderHistoryViewController.PerformSegue(segue, null);
}
```

### <a name="assigning-a-phrase-to-an-nsuseractivity"></a>NSUserActivity에 구 할당

에 `NSUserActivity`구를 할당 하려면 iOS **설정** 앱을 열고 **siri & 선택 하 > 내 바로 가기를 검색**합니다. 그런 다음 바로 가기 (이 경우 "주문 점심")를 선택 하 고 구를 기록 합니다.

Siri를 호출 하 고이 구를 사용 하면 메뉴 화면에 수프 Chef가 열립니다.

## <a name="using-a-custom-intent-shortcut-to-perform-a-task"></a>사용자 지정 의도 바로 가기를 사용 하 여 작업 수행

### <a name="defining-a-custom-intent"></a>사용자 지정 의도 정의

사용자가 앱과 관련 된 특정 작업을 신속 하 게 완료할 수 있도록 하는 바로 가기를 제공 하려면 사용자 지정 의도를 만듭니다. 사용자 지정 의도는 사용자가 완료 해야 할 수 있는 작업, 해당 작업과 관련 된 매개 변수 및 작업 실행으로 인해 발생 하는 잠재적 응답을 나타냅니다. 사용자 지정 의도가 정의 된 방식에 따라 호출을 호출 하면 앱을 열거나 백그라운드 작업을 실행할 수 있습니다.

Xcode 10을 사용 하 여 사용자 지정 의도를 만듭니다. [SoupChef 리포지토리에서](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef)사용자 지정 의도는 목표-C 프로젝트인 **OrderSoupIntentCodeGen**에 정의 됩니다. 이 프로젝트를 열고 **OrderSoup** 의도를 보려면 **프로젝트 탐색기** 에서 해당 파일을 선택 합니다 **.**

다음 사항을 참고하십시오.

- 의도에는 Order **범주가** 있습니다. 사용자 지정 의도에 사용할 수 있는 다양 한 미리 정의 된 범주가 있습니다. 사용자 지정 의도를 사용 하도록 설정할 작업과 가장 일치 하는 항목을 선택 합니다. 이는 수프 주문 앱 이므로 **OrderSoupIntent** 는 **Order**를 사용 합니다.
- **확인** 확인란은 siri가 작업을 실행 하기 전에 확인을 요청 해야 하는지 여부를 나타냅니다. 수프 Chef의 **주문 수프** 의도에 대해이 옵션은 사용자가 구매를 수행 하기 때문에 사용 하도록 설정 됩니다.
- . Intentdefinition 파일의 **parameters** 섹션은 바로 가기와 관련 된 매개 변수를 정의 합니다. 수프 순서를 적용 하려면 수프 Chef는 수프의 유형, 수량 및 관련 옵션을 알아야 합니다.
각 매개 변수에는 형식이 있습니다. 미리 정의 된 형식으로 나타낼 수 없는 매개 변수는 **사용자 지정**으로 설정 됩니다.
- **바로 가기 형식** 인터페이스에서는 siri가 바로 가기를 제안할 때 사용할 수 있는 다양 한 매개 변수 조합을 설명 합니다. 연결 된 **제목** 및 **부제목** 섹션에서 사용자에 게 제안 된 바로 가기를 표시할 때 siri에서 사용할 메시지를 정의할 수 있습니다.
- 추가 사용자 조작을 위해 앱을 열지 않고 실행할 수 있는 바로 가기에 대해 **백그라운드 실행 지원** 확인란을 선택 해야 합니다.

### <a name="defining-custom-intent-responses"></a>사용자 지정 의도 응답 정의

**OrderSoup** 의도 아래에 중첩 된 **응답** 항목은 수프 order에서 발생 하는 잠재적 응답을 나타냅니다.

**OrderSoup** 의도의 응답 정의에서 다음에 유의 하십시오.

- 응답의 **속성** 을 사용 하 여 사용자에 게 다시 전달 되는 메시지를 사용자 지정할 수 있습니다. **OrderSoup** 내재 응답에는 **수프** 및 **waitTime** 속성이 있습니다.
- **응답 템플릿은** 의도의 작업이 완료 된 후 상태를 표시 하는 데 사용할 수 있는 다양 한 성공 및 실패 메시지를 지정 합니다.
- 성공을 나타내는 응답에 대해 **성공** 확인란을 선택 해야 합니다.
- **OrderSoupIntent** success 응답은 **수프** 및 **waitTime** 속성을 사용 하 여 수프 주문이 준비 되는 경우를 설명 하는 친숙 하 고 유용한 메시지를 제공 합니다.

### <a name="generating-code-for-the-custom-intent"></a>사용자 지정 의도에 대 한 코드 생성

이 사용자 지정 의도 정의를 포함 하는 Xcode 프로젝트를 빌드하면 Xcode에서 사용자 지정 의도 및 응답을 사용 하 여 프로그래밍 방식으로 상호 작용 하는 데 사용할 수 있는 코드를 생성 합니다.

이 생성 된 코드를 보려면 다음을 수행 합니다.

- **AppDelegate**를 엽니다.
- 사용자 지정 의도의 헤더 파일에 가져오기를 추가 합니다.`#import "OrderSoupIntent.h"`
- 클래스의 모든 메서드에서에 대 `OrderSoupIntent`한 참조를 추가 합니다.
- 를 마우스 오른쪽 단추로 `OrderSoupIntent` 클릭 하 고 **정의로 이동을**선택 합니다.
- 새로 열린 파일 ( **OrderSoupIntent**)을 마우스 오른쪽 단추로 클릭 하 고 **Finder에 표시**를 선택 합니다.
- 이렇게 하면 생성 된 코드를 포함 하는 .h 및. m 파일이 포함 된 **Finder** 창이 열립니다.

생성 되는 코드는 다음과 같습니다.

- `OrderSoupIntent`– 사용자 지정 의도를 나타내는 클래스입니다.
- `OrderSoupIntentHandling`– 의도를 실행 해야 하는지 여부를 확인 하는 데 사용 되는 메서드와 실제로이를 실행 하는 메서드를 정의 하는 프로토콜입니다.
- `OrderSoupIntentResponseCode`– 다양 한 응답 상태를 정의 하는 열거형입니다.
- `OrderSoupIntentResponse`– 의도 된 실행에 대 한 응답을 나타내는 클래스입니다.

### <a name="creating-a-binding-to-the-custom-intent"></a>사용자 지정 의도에 대 한 바인딩 만들기

Xamarin.ios 앱에서 Xcode에 의해 생성 된 코드를 사용 하려면 해당에 대 한 C# 바인딩을 만듭니다.

#### <a name="creating-a-static-library-and-c-binding-definitions"></a>정적 라이브러리 및 C# 바인딩 정의 만들기

[SoupChef 리포지토리에서](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef) **OrderSoupIntentStaticLib** 폴더를 살펴보고 **OrderSoupIntentStaticLib. xcodeproj** Xcode 프로젝트를 엽니다.

이 **Cocoa Touch 정적 라이브러리** 프로젝트에는 Xcode에 의해 생성 된 **OrderSoupIntent** 및 **OrderSoupIntent** 파일이 포함 되어 있습니다.

#### <a name="configuring-the-static-library-project-build-settings"></a>정적 라이브러리 프로젝트 빌드 설정 구성

Xcode **프로젝트 탐색기**에서 최상위 프로젝트 **OrderSoupIntentStaticLib**를 선택 하 고 **빌드 > 단계로**이동 하 여 소스를 컴파일합니다.
여기에 **OrderSoupIntent** ( **OrderSoupIntent**를 가져오는)가 나열 됩니다. 라이브러리를 사용 하는 **이진 파일 링크**에는 **의도 프레임 워크** 및 **기본 프레임** 워크가 포함 되어 있습니다.
이러한 설정이 적용 되 면 프레임 워크가 올바르게 빌드됩니다.

#### <a name="building-the-static-library-and-generating-c-bindings-definitions"></a>정적 라이브러리 빌드 및 바인딩 정의 C# 생성

정적 라이브러리를 빌드하고 해당 라이브러리에 C# 대 한 바인딩 정의를 생성 하려면 다음 단계를 수행 합니다.

- Xcode에서 만든 .h 및. m 파일에서 바인딩 정의를 생성 하는 데 사용 되는 도구인 [Install Sharpie](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/get-started?context=xamarin/mac#installing-objective-sharpie).

- Xcode 10 명령줄 도구를 사용 하도록 시스템을 구성 합니다.

  > [!WARNING]
  > 선택한 명령줄 도구를 업데이트 하면 시스템에 설치 된 Xcode의 모든 버전에 영향을 줍니다. 수프 Chef 샘플 앱 사용을 완료 한 후에는이 설정을 원래 구성으로 되돌려야 합니다.

  - Xcode에서 **Xcode > 기본 설정 > 위치** 를 선택 하 고, **명령줄 도구** 를 시스템에서 사용할 수 있는 최신 Xcode 10 설치로 설정 합니다.

- 터미널 `cd` 에서 **OrderSoupIntentStaticLib** 디렉터리를 대상으로 합니다.

- 다음 `make`을 작성 하는 형식:

  - 정적 라이브러리 **libOrderSoupIntentStaticLib**
  - **Bo** 출력 디렉터리에서 C# 바인딩 정의:
    - **ApiDefinitions.cs**
    - **StructsAndEnums.cs**

이 정적 라이브러리와 연결 된 바인딩 정의에 의존 하는 **OrderSoupIntentBindings** 프로젝트는 이러한 항목을 자동으로 빌드합니다.
그러나 위의 프로세스를 통해 수동으로 실행 하면 예상 대로 빌드됩니다.

정적 라이브러리를 만드는 방법 및 목표 Sharpie를 사용 하 여 C# 바인딩 정의를 만드는 방법에 대 한 자세한 내용은 [IOS 목표-C 라이브러리 바인딩](https://docs.microsoft.com/xamarin/ios/platform/binding-objective-c/walkthrough?tabs=vsmac#creating-a-static-library) 연습을 참조 하세요.

#### <a name="creating-a-bindings-library"></a>바인딩 라이브러리 만들기

정적 라이브러리 및 C# 바인딩 정의를 만든 후에는 xamarin.ios 프로젝트에서 Xcode 생성 된 의도 관련 코드를 사용 하는 데 필요한 나머지 부분이 바인딩 라이브러리입니다.

[수프 Chef 리포지토리에서](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef) **SoupChef** 파일을 엽니다. 무엇 보다도이 솔루션은 위에서 생성 된 정적 라이브러리에 대 한 바인딩 라이브러리인 **OrderSoupIntentBinding**를 포함 합니다.

특히이 프로젝트에는 다음이 포함 됩니다.

- **ApiDefinitions.cs** – 목표 Sharpie이 프로젝트에 추가 하 고이 프로젝트에 추가 된 파일입니다. 이 파일의 **빌드 작업** 은 **Objcbindingapidefinition**으로 설정 되어 있습니다.
- **StructsAndEnums.cs** – 목표 Sharpie 하 여 위에서 생성 되 고이 프로젝트에 추가 된 다른 파일입니다. 이 파일의 **빌드 작업** 은 **Objcbindingcor9로**설정 됩니다.
- 위에서 빌드한 정적 라이브러리인 **libOrderSoupIntentStaticLib**에 대 한 **네이티브 참조** 입니다.

> [!NOTE]
> **ApiDefinitions.cs** 및 **StructsAndEnums.cs** 모두와 `[Watch (5,0), iOS (12,0)]`같은 특성을 포함 합니다. 목표 Sharpie에서 생성 된 이러한 특성은이 프로젝트에 필요 하지 않기 때문에 주석으로 처리 되었습니다.

C# 바인딩 라이브러리를 만드는 방법에 대 한 자세한 내용은 [iOS 목표 가져오기-C 라이브러리](https://docs.microsoft.com/xamarin/ios/platform/binding-objective-c/walkthrough?tabs=vsmac#create-a-xamarinios-binding-project) 연습을 참조 하세요.

**SoupChef** 프로젝트에는 **OrderSoupIntentBinding**에 대 한 참조가 포함 되어 있습니다 .이는 이제 포함 된 클래스, C#인터페이스 및 열거형에 액세스할 수 있음을 의미 합니다.

- `OrderSoupIntent`
- `OrderSoupIntentHandling`
- `OrderSoupIntentResponse`
- `OrderSoupIntenseResponseCode`

### <a name="adding-the-intentdefinition-file-to-your-solution"></a>솔루션에 intentdefinition 파일 추가

C# **SoupChef** 솔루션에서 **SoupKit** 프로젝트는 앱과 해당 확장 간에 공유 되는 코드를 포함 합니다. **의도. intentdefinition** 파일은 **SoupKit**의 **Base. Lproj** 디렉터리에 배치 되었으며 **콘텐츠의** **빌드 작업** 을 포함 합니다. 빌드 프로세스는 응용 프로그램이 제대로 작동 하는 데 필요한 수프 Chef app 번들로이 파일을 복사 합니다.

### <a name="donating-an-intent"></a>기증 의도

Siri가 바로 가기를 제안 하려면 먼저 바로 가기가 관련 된 경우를 이해 해야 합니다.

Siri를 이해 하기 위해 수프 Chef는 사용자가 수프 주문을 배치할 때마다 siri에 대 한 의도를 제공 합니다. 이러한 기부에 기반 하 여 기증 된 곳에서 기증 된 경우 포함 된 매개 변수 – Siri는 나중에 바로 가기를 제안할 시기를 학습 합니다.

**SoupChef** 는 `SoupOrderDataManager` 클래스를 사용 하 여 기부금을 넣습니다.
사용자에 대 한 수프 순서를 설정 하기 위해 호출 되 `PlaceOrder` 면 메서드는를 [`DonateInteraction`](xref:Intents.INInteraction.DonateInteraction*)호출 합니다.

```csharp
void DonateInteraction(Order order)
{
    var interaction = new INInteraction(order.Intent, null);
    interaction.Identifier = order.Identifier.ToString();
    interaction.DonateInteraction((error) =>
    {
        // ...
    });
}
```

의도를 인출 한 후에는에 [`INInteraction`](xref:Intents.INInteraction)래핑됩니다.
에 `INInteraction` 는이 제공 됩니다.[`Identifier`](xref:Intents.INInteraction.Identifier*)
주문에 대 한 고유 ID와 일치 합니다 .이는 나중에 더 이상 유효 하지 않은 의도 기부를 삭제할 때 유용 합니다. 그런 다음 상호 작용이 Siri에 기증 됩니다.

`order.Intent` Getter에 대 한 호출은 `Quantity`, `Options` `OrderSoupIntent` `Soup`, 및 이미지를 설정 하 여 주문을 나타내는을 페치 하 고, 사용자가 siri가 연결 하기 위한 구를 기록 하는 경우 제안으로 사용할 호출 문구를 가져옵니다. 의도:

```csharp
public OrderSoupIntent Intent
{
    get
    {
        var orderSoupIntent = new OrderSoupIntent();
        orderSoupIntent.Quantity = new NSNumber(Quantity);
        orderSoupIntent.Soup = new INObject(MenuItem.ItemNameKey, MenuItem.LocalizedString);

        var image = UIImage.FromBundle(MenuItem.IconImageName);
        if (!(image is null))
        {
            var data = image.AsPNG();
            orderSoupIntent.SetImage(INImage.FromData(data), "soup");
        }

        orderSoupIntent.Options = MenuItemOptions
            .ToArray<MenuItemOption>()
            .Select<MenuItemOption, INObject>(arg => new INObject(arg.Value, arg.LocalizedString))
            .ToArray<INObject>();

        var comment = "Suggested phrase for ordering a specific soup";
        var phrase = NSBundleHelper.SoupKitBundle.GetLocalizedString("ORDER_SOUP_SUGGESTED_PHRASE", comment);
        orderSoupIntent.SuggestedInvocationPhrase = String.Format(phrase, MenuItem.LocalizedString);

        return orderSoupIntent;
    }
}
```

#### <a name="removing-invalid-donations"></a>잘못 된 기부 제거

Siri가 유용 하거나 혼동 하지 않는 바로 가기 제안을 하지 않도록 더 이상 유효 하지 않은 기부금을 제거 하는 것이 중요 합니다.

수프 Chef에서는 **구성 메뉴** 화면을 사용 하 여 메뉴 항목을 사용할 수 없는 것으로 표시할 수 있습니다. Siri는 더 이상 사용할 수 없는 메뉴 항목의 순서를 지정 하는 바로 `RemoveDonation` 가기를 제안 `SoupMenuManager` 하지 않습니다. 따라서의 메서드는 더 이상 사용할 수 없는 메뉴 항목에 대 한 기부금을 삭제 합니다. 이렇게 하려면 다음을 수행 합니다.

- 현재 사용할 수 없는 메뉴 항목과 연결 된 주문 찾기
- 식별자를 증명 합니다.
- 동일한 식별자를 가진 상호 작용을 삭제 합니다.

```csharp
void RemoveDonation(MenuItem menuItem)
{
    if (!menuItem.IsAvailable)
    {
        Order[] orderHistory = OrderManager?.OrderHistory.ToArray<Order>();
        if (orderHistory is null)
        {
            return;
        }

        string[] orderIdentifiersToRemove = orderHistory
            .Where<Order>((order) => order.MenuItem.ItemNameKey == menuItem.ItemNameKey)
            .Select<Order, string>((order) => order.Identifier.ToString())
            .ToArray<string>();

        INInteraction.DeleteInteractions(orderIdentifiersToRemove, (error) =>
        {
            if (!(error is null))
            {
                Console.WriteLine($"Failed to delete interactions with error {error.ToString()}");
            }
            else
            {
                Console.WriteLine("Successfully deleted interactions");
            }
        });
    }
}
```

### <a name="creating-an-intents-extension"></a>의도 확장 만들기

Siri가 의도를 호출할 때 실행 하는 코드는 수프 Chef 같은 기존 Xamarin.ios 앱과 동일한 솔루션에 새 프로젝트로 추가 될 수 있는 의도 확장에 배치 됩니다. **SoupChef** 솔루션에서 확장을 **SoupChefIntents**라고 합니다.

#### <a name="soupchefintents-infoplist-and-entitlementsplist"></a>SoupChefIntents – info.plist 및 info.plist

##### <a name="soupchefintents-infoplist"></a>SoupChefIntents – info.plist

**SoupChefIntents** 프로젝트의 **Info.plist** 는 **번들 식별자** 를로 `com.xamarin.SoupChef.SoupChefIntents`정의 합니다.

Info.plist 파일에는 다음 항목도 포함 **됩니다** .

```xml
<key>NSExtension</key>
<dict>
    <key>NSExtensionAttributes</key>
    <dict>
        <key>IntentsRestrictedWhileLocked</key>
        <array/>
        <key>IntentsSupported</key>
        <array>
            <string>OrderSoupIntent</string>
        </array>
        <key>IntentsRestrictedWhileProtectedDataUnavailable</key>
        <array/>
    </dict>
    <key>NSExtensionPointIdentifier</key>
    <string>com.apple.intents-service</string>
    <key>NSExtensionPrincipalClass</key>
    <string>IntentHandler</string>
</dict>
```

위의 info.plist에서 다음을 수행 **합니다**.

- `IntentsRestrictedWhileLocked`장치의 잠금이 해제 된 경우에만 처리 해야 하는 의도를 나열 합니다.
- `IntentsSupported`이 확장에 의해 처리 되는 의도를 나열 합니다.
- `NSExtensionPointIdentifier`앱 확장의 유형을 지정 합니다. 자세한 내용은 [Apple 설명서](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/AppExtensionKeys.html#//apple_ref/doc/uid/TP40014212-SW15) 를 참조 하세요.
- `NSExtensionPrincipalClass`이 확장에서 지 원하는 의도를 처리 하는 데 사용 해야 하는 클래스를 지정 합니다.

##### <a name="soupchefintents-entitlementsplist"></a>SoupChefIntents – info.plist

**SoupChefIntents** 프로젝트의 **Info.plist** 에는 **앱 그룹** 기능이 있습니다. 이 기능은 **SoupChef** 프로젝트와 동일한 앱 그룹을 사용 하도록 구성 됩니다.

```xml
<key>com.apple.security.application-groups</key>
<array>
    <string>group.com.xamarin.SoupChef</string>
</array>
```

수프 Chef는를 사용 `NSUserDefaults`하 여 데이터를 유지 합니다. 앱과 앱 확장 간에 데이터를 공유 하기 위해 **info.plist** 파일에서 동일한 앱 그룹을 참조 합니다.

> [!NOTE]
> **SoupChefIntents** 프로젝트의 빌드 구성에서는 **사용자 지정 자격** 을 **info.plist**로 설정 합니다.

#### <a name="handling-an-ordersoupintent-background-task"></a>OrderSoupIntent 백그라운드 작업 처리

의도 확장은 사용자 지정 의도에 따라 바로 가기에 필요한 백그라운드 작업을 실행 합니다.

Siri는 `IntentHandler` 클래스 [`GetHandler`](xref:Intents.INExtension.GetHandler*) ( **info.plist** `NSExtensionPrincipalClass`에 정의 됨)의 메서드를 호출 하 여를 처리 하는 데 사용할 수 있는를 확장 `OrderSoupIntentHandling` `OrderSoupIntent`하는 클래스의 인스턴스를 가져옵니다.

```csharp
[Register("IntentHandler")]
public class IntentHandler : INExtension
{
    public override NSObject GetHandler(INIntent intent)
    {
        if (intent is OrderSoupIntent)
        {
            return new OrderSoupIntentHandler();
        }
        throw new Exception("Unhandled intent type: ${intent}");
    }

    protected IntentHandler(IntPtr handle) : base(handle) { }
}
```

`OrderSoupIntentHandler`공유 코드의 **SoupKit** 프로젝트에 정의 된는 다음과 같은 두 가지 중요 한 메서드를 구현 합니다.

- `ConfirmOrderSoup`– 의도와 연결 된 작업이 실제로 실행 되어야 하는지 여부를 확인 합니다.
- `HandleOrderSoup`– 전달 된 완료 처리기를 호출 하 여 수프 순서를 배치 하 고 사용자에 게 응답 합니다.

#### <a name="handling-an-ordersoupintent-that-opens-the-app"></a>앱을 여는 OrderSoupIntent 처리

앱은 백그라운드에서 실행 되지 않는 의도를 올바르게 처리 해야 합니다.
이러한 `NSUserActivity` `ContinueUserActivity` 메서드 는다음과같은방법으로바로가기와동일한방식으로처리됩니다.`AppDelegate`

```csharp
public override bool ContinueUserActivity(UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    var intent = userActivity.GetInteraction()?.Intent as OrderSoupIntent;
    if (!(intent is null))
    {
        HandleIntent(intent);
        return true;
    }
    // ...
}  
```

## <a name="providing-a-user-interface-for-a-custom-intent"></a>사용자 지정 의도에 대 한 사용자 인터페이스 제공

의도 UI 확장은 의도 확장에 대 한 사용자 지정 사용자 인터페이스를 제공 합니다. **SoupChef** 솔루션에서 **SoupChefIntentsUI** 는 **SoupChefIntents**에 대 한 인터페이스를 제공 하는 의도 UI 확장입니다.

### <a name="soupchefintentsui--infoplist-and-entitlementsplist"></a>SoupChefIntentsUI – info.plist 및 info.plist

#### <a name="soupchefintentsui-infoplist"></a>SoupChefIntentsUI – info.plist

**SoupChefIntentsUI** 프로젝트의 **Info.plist** 는 **번들 식별자** 를로 `com.xamarin.SoupChef.SoupChefIntentsui`정의 합니다.

Info.plist 파일에는 다음 항목도 포함 **됩니다** .

```xml
<key>NSExtension</key>
<dict>
    <key>NSExtensionAttributes</key>
    <dict>
        <key>IntentsSupported</key>
        <array>
            <string>OrderSoupIntent</string>
        </array>
        <!-- ... -->
    </dict>
    <key>NSExtensionPointIdentifier</key>
    <string>com.apple.intents-ui-service</string>
    <key>NSExtensionMainStoryboard</key>
    <string>MainInterface</string>
</dict>
```

위의 info.plist에서 다음을 수행 **합니다**.

- `IntentsSupported``OrderSoupIntent` 가이 의도 UI 확장에 의해 처리 됨을 나타냅니다.
- `NSExtensionPointIdentifier`앱 확장의 유형을 지정 합니다. 자세한 내용은 [Apple 설명서](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/AppExtensionKeys.html#//apple_ref/doc/uid/TP40014212-SW15) 를 참조 하세요.
- `NSExtensionMainStoryboard`이 확장의 기본 인터페이스를 정의 하는 storyboard를 지정 합니다.

#### <a name="soupchefintentsui-entitlementsplist"></a>SoupChefIntentsUI – info.plist

**SoupChefIntentsUI** 프로젝트에는 **info.plist** 파일이 필요 하지 않습니다.

### <a name="creating-the-user-interface"></a>사용자 인터페이스 만들기

**SoupChefIntentsUI** 에 대 한 `NSExtensionMainStoryboard` **info.plist** 는 키를로 `MainInterface`설정 하므로 **MainInterace** 파일은 의도 UI 확장에 대 한 인터페이스를 정의 합니다.

이 스토리 보드에는 **Intentviewcontroller**형식의 단일 뷰 컨트롤러가 있습니다. 두 개의 뷰를 참조 합니다.

- 형식의 **invoiceView**`InvoiceView`
- 형식의 **confirmationView**`ConfirmOrderView`

> [!NOTE]
> **InvoiceView** 및 **confirmationView** 에 대 한 인터페이스는 **기본 storyboard** 에서 보조 뷰로 정의 됩니다. Mac용 Visual Studio 및 Visual Studio 2017의 iOS 디자이너는 보조 뷰를 보거나 편집할 수 있는 기능을 제공 하지 않습니다. 이렇게 하려면 Xcode의 Interface Builder에서 **기본 storyboard** 를 엽니다.

`IntentViewController`을 구현 합니다.[`IINUIHostedViewControlling`](xref:IntentsUI.IINUIHostedViewControlling)
Siri 의도를 사용할 때 사용자 지정 인터페이스를 제공 하는 데 사용 되는 인터페이스입니다. 여[`ConfigureView`](xref:IntentsUI.INUIHostedViewControlling_Extensions.ConfigureView*)
상호 작용이 확인 되는지 ([`INIntentHandlingStatus.Ready`](xref:Intents.INIntentHandlingStatus)) 아니면 성공적으로 실행 되었는지 ([`INIntentHandlingStatus.Success`](xref:Intents.INIntentHandlingStatus))에 따라 인터페이스를 사용자 지정 하 여 확인 또는 송장을 표시 하는 메서드가 호출 됩니다.

```csharp
[Export("configureViewForParameters:ofInteraction:interactiveBehavior:context:completion:")]
public void ConfigureView(
    NSSet<INParameter> parameters,
    INInteraction interaction,
    INUIInteractiveBehavior interactiveBehavior,
    INUIHostedViewContext context,
    INUIHostedViewControllingConfigureViewHandler completion)
{
    // ...
    if (interaction.IntentHandlingStatus == INIntentHandlingStatus.Ready)
    {
        desiredSize = DisplayInvoice(order, intent);
    }
    else if(interaction.IntentHandlingStatus == INIntentHandlingStatus.Success)
    {
        var response = interaction.IntentResponse as OrderSoupIntentResponse;
        if (!(response is null))
        {
            desiredSize = DisplayOrderConfirmation(order, intent, response);
        }
    }
    completion(true, parameters, desiredSize);
}
```

> [!TIP]
> `ConfigureView` 방법에 대 한 자세한 내용은 Apple의 wwdc 2017 프레젠테이션, [sirikit의 새로운 기능](https://developer.apple.com/videos/play/wwdc2017/214/)을 시청 하세요.

## <a name="creating-a-voice-shortcut"></a>음성 바로 가기 만들기

수프 Chef는 각 주문에 음성 바로 가기를 할당 하는 인터페이스를 제공 하므로 Siri를 사용 하 여 수프를 주문할 수 있습니다. 실제로 음성 바로 가기를 기록 하 고 할당 하는 데 사용 되는 인터페이스는 iOS에서 제공 되며 사용자 지정 코드가 거의 필요 하지 않습니다.

에서 `OrderDetailViewController`사용자가 테이블의 **siri 행에 추가** 를 탭 하면 메서드는 음성 [`RowSelected`](xref:UIKit.UITableViewSource.RowSelected*) 바로 가기를 추가 하거나 편집할 수 있는 화면을 표시 합니다.

```csharp
public override void RowSelected(UITableView tableView, NSIndexPath indexPath)
{
    // ...
    else if (TableConfiguration.Sections[indexPath.Section].Type == OrderDetailTableConfiguration.SectionType.VoiceShortcut)
    {
        INVoiceShortcut existingShortcut = VoiceShortcutDataManager?.VoiceShortcutForOrder(Order);
        if (!(existingShortcut is null))
        {
            var editVoiceShortcutViewController = new INUIEditVoiceShortcutViewController(existingShortcut);
            editVoiceShortcutViewController.Delegate = this;
            PresentViewController(editVoiceShortcutViewController, true, null);
        }
        else
        {
            // Since the app isn't yet managing a voice shortcut for
            // this order, present the add view controller
            INShortcut newShortcut = new INShortcut(Order.Intent);
            if (!(newShortcut is null))
            {
                var addVoiceShortcutVC = new INUIAddVoiceShortcutViewController(newShortcut);
                addVoiceShortcutVC.Delegate = this;
                PresentViewController(addVoiceShortcutVC, true, null);
            }
        }
    }
}
```

현재 표시 된 주문 `RowSelected` 에 대 한 기존 음성 단축키가 있는지 여부에 따라은 또는 [`INUIAddVoiceShortcutViewController`](xref:IntentsUI.INUIAddVoiceShortcutViewController)유형의 [`INUIEditVoiceShortcutViewController`](xref:IntentsUI.INUIEditVoiceShortcutViewController) 뷰 컨트롤러를 표시 합니다.
각각의 경우는 `OrderDetailViewController` 자체를 뷰 컨트롤러의 `Delegate`로 설정 합니다 .이 경우에도을 구현 합니다.[`IINUIAddVoiceShortcutViewControllerDelegate`](xref:IntentsUI.IINUIAddVoiceShortcutViewControllerDelegate)
및 [`IINUIEditVoiceShortcutViewControllerDelegate`](xref:IntentsUI.IINUIEditVoiceShortcutViewControllerDelegate)입니다.

## <a name="testing-on-device"></a>장치에서 테스트

장치에서 수프 Chef를 실행 하려면 아래 지침을 따르세요. [자동 프로 비전에 대 한 참고](#automatic-provisioning)사항도 읽어 보세요.

### <a name="app-group-app-ids-provisioning-profiles"></a>앱 그룹, 앱 Id, 프로 비전 프로필

[Apple 개발자 포털](https://developer.apple.com/)의 **인증서, id & 프로필** 섹션에서 다음을 수행 합니다.

- 수프 Chef 앱과 해당 확장 간에 데이터를 공유 하는 앱 그룹을 만듭니다. 예를 들면 다음과 같습니다. **SoupChef**

- 응용 프로그램 자체에 대 한 세 가지 앱 Id를 만듭니다. 하나는 의도 확장을 위한 것이 고 다른 하나는 의도 UI 확장에 대 한 Id입니다. 예를 들어:

  - App: **com.yourcompanyname.SoupChef**
    - 이 앱 ID로 SiriKit 및 **앱 그룹** 기능을 할당 합니다.

  - 의도 확장: **com. SoupChef.**
    - 이 앱 ID에 **앱 그룹** 기능을 할당 합니다.

  - 의도 UI 확장: **com. SoupChef. Intentsui**
    - 이 앱 ID에는 특별 한 기능이 필요 하지 않습니다.

- 위의 앱 Id를 만든 후 위에 만든 특정 앱 그룹을 지정 하 여 앱 및 의도 확장에 할당 된 **앱 그룹** 기능을 편집 합니다.

- 새 앱 Id 각각에 대해 하나씩, 세 개의 새로운 개발 프로 비전 프로필을 만듭니다.

- 이러한 프로 비전 프로필을 다운로드 하 고 각 프로필을 두 번 클릭 하 여 설치 합니다. Mac용 Visual Studio 또는 Visual Studio 2017가 이미 실행 중인 경우에는 다시 시작 하 여 새 프로 비전 프로필을 등록 해야 합니다.

### <a name="editing-infoplist-entitlementsplist-and-source-code"></a>Info.plist, info.plist 및 소스 코드 편집

Mac용 Visual Studio 또는 Visual Studio 2017에서 다음을 수행 합니다.

- 솔루션의 다양 한 **info.plist** 파일을 업데이트 합니다. 앱, 의도 확장 및 의도 UI 확장 **번들 식별자** 를 위에서 정의한 앱 id로 설정 합니다.

  - App: **com.yourcompanyname.SoupChef**
  - 의도 확장: **com. SoupChef.**
  - 의도 UI 확장: **com. SoupChef. Intentsui**

- **SoupChef** 프로젝트에 대 한 **info.plist** 파일을 업데이트 합니다.
  - **앱 그룹** 기능에 대해 위에서 만든 새 앱 그룹으로 그룹을 설정 합니다 (위의 예제에서는 **SoupChef**).
  - **Sirikit** 를 사용 하도록 설정 했는지 확인 합니다.

- **SoupChefIntents** 프로젝트에 대 한 **info.plist** 파일을 업데이트 합니다.
  - **앱 그룹** 기능에 대해 위에서 만든 새 앱 그룹으로 그룹을 설정 합니다 (위의 예제에서는 **SoupChef**).

- 마지막으로 **NSUserDefaultsHelper.cs**를 엽니다. 변수를 새 앱 그룹의 값으로 설정 합니다 (예:로 `group.com.yourcompanyname.SoupChef`설정). `AppGroup`

### <a name="configuring-the-build-settings"></a>빌드 설정 구성

Mac용 Visual Studio 또는 Visual Studio 2017:

- **SoupChef** 프로젝트에 대 한 옵션/속성을 엽니다. **IOS 번들 서명** 탭에서 **서명 id** 를 자동으로 설정 하 고 **프로 비전 프로필** 을 위에서 만든 새 앱 특정 프로 비전 프로필로 설정 합니다.

- **SoupChefIntents** 프로젝트에 대 한 옵션/속성을 엽니다. **IOS 번들 서명** 탭에서 **서명 id** 를 자동으로 설정 하 고 **프로 비전 프로필** 을 위에서 만든 새 의도 확장의 프로 비전 프로필로 설정 합니다.

- **SoupChefIntentsUI** 프로젝트에 대 한 옵션/속성을 엽니다. **IOS 번들 서명** 탭에서 **서명 id** 를 자동으로 설정 하 고 **프로 비전 프로필** 을 위에서 만든 새 의도 UI 확장 특정 프로 비전 프로필로 설정 합니다.

이러한 변경 내용을 적용 하면 앱이 iOS 장치에서 실행 됩니다.

### <a name="automatic-provisioning"></a>자동 프로 비전

[자동 프로 비전](https://docs.microsoft.com/xamarin/ios/get-started/installation/device-provisioning/automatic-provisioning) 을 사용 하 여 IDE에서 이러한 여러 프로 비전 작업을 직접 수행할 수 있습니다.
그러나 자동 프로 비전은 앱 그룹을 설정 하지 않습니다. 사용 하려는 앱 그룹의 이름으로 **info.plist** 파일을 수동으로 구성 해야 합니다. Apple 개발자 포털을 방문 하 여 앱 그룹을 만들고, 자동 프로 비전으로 만든 각 앱 ID에 해당 앱 그룹을 할당 하 고, 다시 생성 하세요. 새로 만든 앱 그룹을 포함 하 고 다운로드 하 여 설치 하는 프로 비전 프로필 (앱, 의도 확장, 의도 UI 확장)입니다.

## <a name="related-links"></a>관련 링크

- [수프 Chef (Xamarin)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-soupchef)
- [수프 Chef (Swift)](https://developer.apple.com/documentation/sirikit/accelerating_app_interactions_with_shortcuts?language=objc)
- [SiriKit (Apple)](https://developer.apple.com/sirikit/)
- [Siri 바로 가기 소개 – WWDC 2018](https://developer.apple.com/videos/play/wwdc2018/211/)
- [Siri 바로 가기를 사용 하 여 음성을 빌드하기 – WWDC 2018](https://developer.apple.com/videos/play/wwdc2018/214/)
- [Siri Watch 얼굴의 siri 바로 가기 – WWDC 2018](https://developer.apple.com/videos/play/wwdc2018/217/)
- [SiriKit의 새로운 기능 – WWDC 2017](https://developer.apple.com/videos/play/wwdc2017/214/)
- [의도 앱 확장 (Apple) 만들기](https://developer.apple.com/documentation/sirikit/creating_an_intents_app_extension?language=objc)
