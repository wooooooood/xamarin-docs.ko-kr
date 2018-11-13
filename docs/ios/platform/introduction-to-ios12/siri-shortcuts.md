---
title: Xamarin.iOS에서 Siri 바로 가기
description: 이 문서에서는 iOS 12에서에서 Siri 바로 가기 키를 사용 하는 방법을 설명 합니다. NSUserActivities, 사용자 지정 의도 음성 바로 가기, 관련 바로 가기 등 할당에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 86424F79-3A7D-436E-927D-9A3267DA333B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
ms.openlocfilehash: f9034799355d01a3ade20a78540d6ecac43d9cc8
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/11/2018
ms.locfileid: "51526899"
---
# <a name="siri-shortcuts-in-xamarinios"></a>Xamarin.iOS에서 Siri 바로 가기

[iOS 10](~/ios/platform/sirikit/index.md)Apple SiriKit 소개, 수 있도록 빌드 메시징, VoIP 호출, 결제, 달리기, 예약, 재정의 및 사진 Siri 상호 작용 하는 앱을 검색 합니다.

[iOS 11](~/ios/platform/introduction-to-ios11/sirikit.md), SiriKit 더 다양 한 유형의 앱에 대 한 지원 및 UI 사용자 지정에 대 한 유연성을 얻게 됩니다.

iOS 12 추가 Siri 바로 가기, Siri 해당 기능을 노출 하는 앱의 모든 형식을 허용 합니다. 특정 앱 기반 작업 사용자에 게 가장 관련 된 시점과 통해 잠재적인 작업을 제안 하려면이 정보를 사용 하 여 Siri 학습 _바로 가기를_합니다. 바로 가기를 눌러 음성 명령을 사용 하 여 호출 됩니다 앱 열기 또는 백그라운드 작업을 실행 합니다.

바로 가기 – 의심 스러운 앱을 열지 않고도 대부분의 일반적인 작업을 수행 하는 사용자의 능력을 가속화 하는 것 같습니다.

## <a name="sample-app-soup-chef"></a>샘플 앱: Soup Chef

Siri 바로 가기 키를 더 잘 이해 하려면 잠시 살펴 합니다 [Soup Chef](https://developer.xamarin.com/samples/monotouch/ios12/SoupChef/) 샘플 앱입니다. Soup Chef를 사용 하면는 허수 soup 음식점에서 주문 하 고 해당 주문 기록 보고 soup Siri 상호 작용 하 여 정렬할 때 사용 하는 문구를 정의할 수 있습니다.

> [!TIP]
> Soup Chef 12 iOS 시뮬레이터 또는 장치에서 테스트 하기 전에 바로 가기 키를 디버깅할 때 유용 하는 다음 두 설정을 사용 하도록 설정 합니다.
>
> - 에 **설정을** 앱을 사용 하도록 설정 **개발자 > 최근 바로 가기 표시**합니다.
> - 에 **설정을** 앱을 사용 하도록 설정 **개발자 > 잠금 화면에서 표시 기부**합니다.
>
> 이러한 디버그 쉽게 찾을 최근에 만든 설정 확인 (대신 예측) 바로 가기는 ios 잠금 화면 및 검색 화면입니다.

샘플 앱을 사용 합니다.

- 설치 하 고 12 iOS 시뮬레이터에서 Soup Chef 샘플 앱을 실행 하거나 [장치](#testing-on-device)합니다.
- 클릭 합니다 **+** 새 주문을 만들려면 오른쪽 위에 있는 단추입니다.
- Soup 유형을 선택 하 고, 수량 및 옵션을 지정 하 고, 탭 **주문**합니다.
- 에 **Order History** 화면에서 해당 정보를 보려면 새로 만든 순서를 탭 합니다.
- 아래쪽에 주문 세부 정보 화면을 누릅니다 **Siri 추가할**합니다.
- 기록 순서를 사용 하 여 연결 하 고 탭 음성 구 **수행**합니다.
- Siri 호출을 최소화 Soup Chef 입력 하 고 방금 기록 된 음성 라는 문구를 사용 하 여 순서를 다시 배치 합니다.
- Siri 순서가 완료 된 후 다시 Soup Chef를 열고 새 주문에 나열 되어 있는지 확인 합니다 **Order History** 화면.

샘플 앱을 보여 줍니다. 방법:

- [사용 하 여 NSUserActivity 바로 앱을 여는 데](#using-an-nsuseractivity-shortcut-to-open-an-app)합니다.
- [작업을 수행 하는 사용자 지정 의도 바로 가기를 사용할](#using-a-custom-intent-shortcut-to-perform-a-task)합니다.
- [사용자 지정 의도 대 한 사용자 인터페이스를 제공](#providing-a-user-interface-for-a-custom-intent)합니다.
- [음성 바로 가기를 만들](#creating-a-voice-shortcut)합니다.

## <a name="infoplist-and-entitlementsplist"></a>Info.plist 및 Entitlements.plist

잠시 살펴 Soup Chef 코드 좀더 자세히를 하기 전에 해당 **Info.plist** 하 고 **Entitlements.plist** 파일입니다.

### <a name="infoplist"></a>Info.plist

**Info.plist** 파일을 **SoupChef** 프로젝트 정의 **번들 식별자** 으로 `com.xamarin.SoupChef`입니다. 이 번들 식별자는 인 텐트와 인 텐트 UI의 번들 식별자에 대 한 접두사로 사용 됩니다이 문서의 뒷부분에서 설명 하는 확장 합니다.

합니다 **Info.plist** 파일 다음에도 포함 되어 있습니다.

```xml
<key>NSUserActivityTypes</key>
<array>
    <string>OrderSoupIntent</string>
    <string>com.xamarin.SoupChef.viewMenu</string>
</array>
```

이 `NSUserActivityTypes` Soup Chef가 처리 하는 방법을 알고 있는 키/값 쌍 나타냅니다는 `OrderSoupIntent`, 및 [ `NSUserActivity` ](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/) 하지는 [ `ActivityType` ](https://developer.xamarin.com/api/property/Foundation.NSUserActivity.ActivityType/) "com.xamarin.SoupChef.viewMenu"의 합니다.

활동 및 확장, 아니라 앱 자체에 전달 하는 사용자 지정 의도에서 처리 되는 `AppDelegate` (을 [ `UIApplicationDelegate` ](https://developer.xamarin.com/api/type/UIKit.UIApplicationDelegate/)) 여는 [ `ContinueUserActivity` ](https://developer.xamarin.com/api/member/UIKit.UIApplicationDelegate.ContinueUserActivity/) 메서드.

### <a name="entitlementsplist"></a>Entitlements.plist

합니다 **Entitlements.plist** 파일을 **SoupChef** 다음을 포함 하는 프로젝트:

```xml
<key>com.apple.security.application-groups</key>
<array>
    <string>group.com.xamarin.SoupChef</string>
</array>
<key>com.apple.developer.siri</key>
<true/>
```

이 구성은 앱 "group.com.xamarin.SoupChef" 앱 그룹을 사용 하는 것을 나타냅니다. 합니다 **SoupChefIntents** 두 프로젝트 공유를 허용 하는이 같은 앱 그룹을 사용 하 여 앱 확장 [`NSUserDefaults`](https://developer.xamarin.com/api/type/Foundation.NSUserDefaults/)
데이터

`com.apple.developer.siri` 키 Siri를 사용 하 여 앱 상호 작용 한다고 나타냅니다.

> [!NOTE]
> 합니다 **SoupChef** 프로젝트의 빌드 구성 집합 **사용자 지정 자격** 하 **Entitlements.plist**합니다.

## <a name="using-an-nsuseractivity-shortcut-to-open-an-app"></a>사용 하 여 NSUserActivity 바로 사용 하 여 앱을 열려면

특정 콘텐츠를 표시 하는 앱이 열리는 바로 가기를 만들려면 만들기는 `NSUserActivity` 바로 가기를 열고 싶은 화면에 대 한 뷰 컨트롤러에 연결 합니다.

### <a name="setting-up-an-nsuseractivity"></a>NSUserActivity 설정

메뉴 화면의 `SoupMenuViewController` 만듭니다는 `NSUserActivity` 뷰 컨트롤러의 할당 [ `UserActivity` ](https://developer.xamarin.com/api/property/UIKit.UIResponder.UserActivity/) 속성:

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();
    UserActivity = NSUserActivityHelper.ViewMenuActivity;
}
```

설정 된 `UserActivity` 속성 _donates_ 활동 Siri입니다. Siri이이 기부에서이 작업은 사용자에 게 유용한 및 더 앞으로 제안 하려면 학습 시기 및 위치에 대 한 정보를 얻게 됩니다.

`NSUserActivityHelper` 에 포함 하는 유틸리티 클래스는 **SoupChef** 솔루션에서 합니다 **SoupKit** 클래스 라이브러리. 만듭니다는 `NSUserActivity` Siri 및 검색과 관련 된 다양 한 속성을 가져오거나 설정 합니다.

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

다음을 특히 note:

- 설정 `EligibleForPrediction` 에 `true` Siri이이 활동을 예측 하 고 바로 가기로 화면 수 있는지를 나타냅니다.
- 합니다 [ `ContentAttributeSet` ](https://developer.xamarin.com/api/property/Foundation.NSUserActivity.ContentAttributeSet/) 배열이 표준 [ `CSSearchableItemAttributeSet` ](https://developer.xamarin.com/api/type/CoreSpotlight.CSSearchableItemAttributeSet/) 포함 하는 데는 `NSUserActivity` iOS 검색 결과에서.
- [`SuggestedInvocationPhrase`](https://developer.xamarin.com/api/property/Foundation.NSUserActivity.SuggestedInvocationPhrase/) Siri가 제안 사용자에 게 잠재적인 선택 항목으로 바로 가기는 문구를 할당할 때 하는 구문이입니다.

### <a name="handling-an-nsuseractivity-shortcut"></a>사용 하 여 NSUserActivity 바로 처리

처리 하는 `NSUserActivity` 바로 가기를 사용자 호출한 iOS 응용 프로그램을 재정의 해야 합니다는 `ContinueUserActivity` 메서드의 `AppDelegate` 클래스를 기반으로 응답 합니다 `ActivityType` 전달-의 필드 `NSUserActivity` 개체:

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

이 메서드를 호출 `HandleUserActivity`, 메뉴 화면으로 segue를 찾아서 호출입니다.

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

### <a name="assigning-a-phrase-to-an-nsuseractivity"></a>NSUserActivity 구 할당

할당 되는 구문에는 `NSUserActivity`, iOS 엽니다 **설정** 앱 선택 **Siri 및 검색 > 바로 가기 키**. 그런 다음 (이 여기서는 "순서 점심")에서 바로 가기 키를 선택 하 고 구를 기록 합니다.

Siri를 호출 하 고이 구를 사용 하 여 열립니다 Soup Chef 메뉴 화면.

## <a name="using-a-custom-intent-shortcut-to-perform-a-task"></a>사용자 지정 의도 바로 가기의 사용 하 여 작업을 수행 하려면

### <a name="defining-a-custom-intent"></a>사용자 지정 의도 정의합니다.

신속 하 게 앱에 관련 된 특정 작업을 완료할 수 있도록 허용 하는 바로 가기의 제공 하도록 사용자 지정 의도 만듭니다. 사용자 지정 여부는 해당 작업 및 잠재적인 응답 작업의 실행 결과 관련 된 매개 변수를 완료 하려는 사용자 작업을 나타냅니다. 사용자 지정 의도 정의 되는 방식에 따라 호출 앱을 열고 하거나 백그라운드 작업을 실행 합니다.

사용자 지정 의도 만들려면 Xcode 10을 사용 합니다. 에 [SoupChef 리포지토리](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef)에 정의 되어 있는 사용자 지정 의도 **OrderSoupIntentCodeGen**, Objective-c로 프로젝트를 합니다. 이 프로젝트를 열고 선택 합니다 **Intents.intentdefinition** 파일을 **Project Navigator** 보려는 **OrderSoup** 의도.

다음 사항을 참고하십시오.

- 의도는 **범주** 의 **순서**합니다. 사용자 지정 의도;에 사용할 수 있는 다양 한 미리 정의 된 범주 사용자 지정 의도 사용 하는 작업에 가장 잘 맞는 하나를 선택 합니다. 때문에이 앱을 정렬 하는 soup **OrderSoupIntent** 사용 하 여 **순서**합니다.
- 합니다 **확인** 확인란 Siri 작업을 실행 하기 전에 확인을 요청 해야 여부를 나타냅니다. 에 대 한 합니다 **순서 Soup** 의도 Soup Chef에서,이 옵션을 사용 하므로 사용자가 구매 합니다.
- 합니다 **매개 변수** .intentdefinition 파일의 섹션에는 바로 가기와 관련 된 매개 변수를 정의 합니다. Soup 주문에 Soup Chef soup, 해당 수량 및 모든 관련된 옵션의 유형을 알고 있어야 합니다.
각 매개 변수에 형식입니다. 로 설정 됩니다. 미리 정의 된 형식으로 표현할 수 없는 매개 변수 **사용자 지정**합니다.
- 합니다 **바로 가기 형식** 인터페이스 Siri 프로그램 바로 가기를 제안 하는 경우 사용할 수 있습니다 다양 한 매개 변수 조합을 설명 합니다. 연결 된 **제목** 하 고 **부제목** 섹션을 사용 하면 제안 된 바로 가기를 사용자에 게 표시할 때 Siri를 사용 하 여 메시지를 정의할 수 있습니다.
- 합니다 **백그라운드 실행을 지원** 추가 사용자 상호 작용에 대 한 앱을 열지 않고 실행할 수 있는 모든 바로 가기에 대 한 확인란을 선택 해야 합니다.

### <a name="defining-custom-intent-responses"></a>사용자 지정 의도 응답 정의

합니다 **응답** 항목 아래에 중첩 된 **OrderSoup** 의도 soup 주문에서 발생 하는 잠재적인 응답을 나타냅니다.

에 **OrderSoup** 의도 응답 정의 다음을 유의 하십시오.

- 응답의 **속성** 다시 사용자에 게 전달 하는 메시지를 사용자 지정에 사용할 수 있습니다. 합니다 **OrderSoup** 의도 응답 **soup** 하 고 **waitTime** 속성입니다.
- 합니다 **응답 템플릿** 의도 작업 완료 상태를 나타내기 위해 사용할 수 있는 다양 한 성공 및 실패 메시지를 지정 합니다.
- 합니다 **성공** 성공을 나타내는 응답에 대 한 확인란을 선택 해야 합니다.
 - **OrderSoupIntent** 성공 응답을 사용 합니다 **soup** 및 **waitTime** soup 주문 수를 설명 하는 친숙 하 고 유용한 메시지를 제공 하는 속성입니다.

### <a name="generating-code-for-the-custom-intent"></a>사용자 지정 의도 대 한 코드를 생성합니다.

이 사용자 지정 의도 정의가 포함 된 Xcode 프로젝트를 빌드할 Xcode 사용자 지정 의도 및 해당 응답을 사용 하 여 프로그래밍 방식으로 상호 작용에 사용할 수 있는 코드를 생성 하면 됩니다.

이 항목을 보려면 코드를 생성 합니다.

- 오픈 **AppDelegate.m**합니다.
- 사용자 지정 의도 헤더 파일에 가져오기를 추가 합니다. `#import "OrderSoupIntent.h"`
- 클래스의 모든 메서드를 추가에 대 한 참조 `OrderSoupIntent`합니다.
- 마우스 오른쪽 단추로 클릭 `OrderSoupIntent` 선택한 **정의로 이동**합니다.
- 새로 열린 파일을 마우스 오른쪽 단추로 클릭 **OrderSoupIntent.h**, 선택한 **Finder에 표시**합니다.
- 열립니다는 **Finder** 생성된 된 코드를 포함 하는.h 및.m 파일을 포함 하는 창입니다.

이 생성 된 코드에는 다음이 포함 됩니다.

- `OrderSoupIntent` – 사용자 지정 의도 나타내는 클래스입니다.
- `OrderSoupIntentHandling` 의도 실행할지를 확인 하는 데 사용할 메서드와 실제로 실행 하는 메서드를 정의 하는 – 프로토콜입니다.
- `OrderSoupIntentResponseCode` -다양 한 응답 상태를 정의 하는 열거형입니다.
- `OrderSoupIntentResponse` – 의도 실행에 대 한 응답을 나타내는 클래스입니다.

### <a name="creating-a-binding-to-the-custom-intent"></a>사용자 지정 의도 바인딩 만들기

Xamarin.iOS 앱의 Xcode에서 생성 된 코드를 사용 하려면 만들기는 C# 에 대 한 바인딩.

#### <a name="creating-a-static-library-and-c-binding-definitions"></a>정적 라이브러리 만들기 및 C# 바인딩 정의

에 [SoupChef 리포지토리](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef)를 살펴보세요 합니다 **OrderSoupIntentStaticLib** 폴더를 **OrderSoupIntentStaticLib.xcodeproj** Xcode 프로젝트.

이 **Cocoa Touch 정적 라이브러리** 프로젝트에 포함 된 **OrderSoupIntent.h** 및 **OrderSoupIntent.m** Xcode에서 생성 된 파일입니다.

#### <a name="configuring-the-static-library-project-build-settings"></a>정적 라이브러리 프로젝트 빌드 설정 구성

Xcode에서 **Project Navigator**에서 최상위 프로젝트를 **OrderSoupIntentStaticLib**, 이동할 **빌드 단계 > 컴파일할 소스**합니다.
있음을 **OrderSoupIntent.m** (가져오는 **OrderSoupIntent.h**) 여기에 나열 됩니다. **이진 파일과 라이브러리 연결**, 있음을 **Intents.framework** 하 고 **Foundation.framework** 포함 됩니다.
현재 위치에서 이러한 설정을 사용 하 여 프레임 워크는 올바르게 작성 합니다.

#### <a name="building-the-static-library-and-generating-c-bindings-definitions"></a>정적 라이브러리를 빌드 및 생성 C# 바인딩 정의

정적 라이브러리를 빌드 및 생성 하려면 C# 바인딩 정의를 다음이 단계를 수행 합니다.

- [목표 Sharpie 설치](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/get-started?context=xamarin/mac#installing-objective-sharpie), Xcode로 만든.h 및.m 파일에서 바인딩 정의 생성 하는 데 사용 하는 도구입니다.

- Xcode 10 명령줄 도구를 사용 하도록 시스템을 구성 합니다.

    > [!WARNING]
    > 선택한 명령줄 도구를 업데이트 하는 중 모든 설치 된 버전의 Xcode 시스템에 영향을 줍니다. 완료 되 면 Soup Chef를 사용 하 여 샘플 앱을 반드시이 되돌리려면 원래 구성으로 설정 합니다.

    - Xcode에서 선택 **Xcode > 기본 설정 > 위치** 설정 **명령줄 도구** 최신 Xcode 10 설치 시스템에서 사용할 수 있습니다.

- 터미널에서 `cd` 에 **OrderSoupIntentStaticLib** 디렉터리입니다.

- 형식 `make`, 빌드는:

    - 정적 라이브러리, **libOrderSoupIntentStaticLib.a**
    - 에 **bo** 출력 디렉터리에 C# 바인딩 정의 합니다.
        - **ApiDefinitions.cs**
        - **StructsAndEnums.cs**

합니다 **OrderSoupIntentBindings** 이 정적 라이브러리 및 해당 연관 된 바인딩 정의 의존 하는 프로젝트에 이러한 항목을 자동으로 빌드됩니다.
그러나 위의 프로세스를 통해 수동으로 실행 되도록 프로그램을 빌드할 때 예상 대로입니다.

정적 라이브러리 만들기 및 만드는 목표 Sharpie를 사용 하는 방법에 대 한 자세한 내용은 C# 바인딩 정의 살펴보겠습니다 합니다 [는 iOS Objective-c 라이브러리 바인딩](https://docs.microsoft.com/xamarin/ios/platform/binding-objective-c/walkthrough?tabs=vsmac#creating-a-static-library) 연습 합니다.

#### <a name="creating-a-bindings-library"></a>바인딩 라이브러리 만들기

정적 라이브러리를 사용 하 여 및 C# 바인딩을 정의 생성을 나머지 부분 Xcode 생성을 사용 하는 데 필요한 의도 관련 코드를 Xamarin.iOS 프로젝트에서 바인딩 라이브러리입니다.

에 [Soup Chef 리포지토리](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef)열고 합니다 **SoupChef.sln** 파일입니다. 이 솔루션에 포함 된 가장 효과적인 **OrderSoupIntentBinding**, 위에서 생성 된 정적 라이브러리에 대 한 바인딩 라이브러리입니다.

특히이 프로젝트에 포함 되어 있는지 확인 합니다.

- **ApiDefinitions.cs** – 파일 목표 Sharpie 하 여 위에서 생성 및이 프로젝트에 추가 합니다. 이 파일의 **빌드 작업** 로 설정 된 **ObjcBindingApiDefinition**합니다.
- **StructsAndEnums.cs** – 다른 목표 Sharpie 하 여 위에서 생성 된 파일과이 프로젝트에 추가 합니다. 이 파일의 **빌드 작업** 로 설정 된 **ObjcBindingCoreSource**합니다.
- A **네이티브 참조** 하 **libOrderSoupIntentStaticLib.a**, 위에서 빌드한 정적 라이브러리입니다.

> [!NOTE]
> 둘 다 **ApiDefinitions.cs** 하 고 **StructsAndEnums.cs** 와 같은 특성을 포함할 `[Watch (5,0), iOS (12,0)]`합니다. 목표 Sharpie에서 생성 하는 이러한 특성을 한 주석 처리 된이 프로젝트에 필요한 수 없기 때문입니다.

만들기에 대 한 자세한 내용은 C# 바인딩 라이브러리를 살펴보기 합니다 [iOS Objective-c 라이브러리 바인딩](https://docs.microsoft.com/xamarin/ios/platform/binding-objective-c/walkthrough?tabs=vsmac#create-a-xamarinios-binding-project) 연습 합니다.

있음을 합니다 **SoupChef** 프로젝트에 대 한 참조를 포함 **OrderSoupIntentBinding**, 즉, 액세스할 수 있는 지금,에 C#, 클래스, 인터페이스 및 열거형을 포함:

- `OrderSoupIntent`
- `OrderSoupIntentHandling`
- `OrderSoupIntentResponse`
- `OrderSoupIntenseResponseCode`

### <a name="adding-the-intentdefinition-file-to-your-solution"></a>솔루션에.intentdefinition 파일 추가

에 C# **SoupChef** 솔루션을 **SoupKit** 프로젝트에는 앱 및 해당 확장 프로그램 간에 공유 하는 코드가 포함 되어 있습니다. 합니다. **Intents.intentdefinition** 파일에 배치 되는 **Base.lproj** 디렉터리 **SoupKit**, 있고는 **빌드 작업** 의 **콘텐츠**합니다. 빌드 프로세스는 앱이 제대로 작동 하는 데 필요한 경우 Soup Chef 앱 번들에이 파일을 복사 합니다.

### <a name="donating-an-intent"></a>의도 기증

Siri가 바로 가기를 제안에 대 한 순서 대로 바로 가기 관련이 먼저 이해 해야 할 합니다.

이 이해 Soup Chef Siri 제공할 _donates_ 하려는 의도 Siri 될 때마다 사용자 soup 주문 합니다. 이 된 기부를 기부 있는 경우이 기부 – 기반 포함 하는 매개 변수를 – Siri 나중에 바로 가기를 제안 하는 경우 학습 합니다.

**SoupChef** 사용 하는 `SoupOrderDataManager` 기부를 배치 하는 클래스입니다.
사용자에 대해 soup 주문 하기 위해 호출 하는 경우는 `PlaceOrder` 메서드 호출 [ `DonateInteraction` ](https://developer.xamarin.com/api/member/Intents.INInteraction.DonateInteraction/):

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

의도 페치를 후에 래핑된를 [ `INInteraction` ](https://developer.xamarin.com/api/type/Intents.INInteraction/)합니다.
`INInteraction` 지정 되는 [`Identifier`](https://developer.xamarin.com/api/property/Intents.INInteraction.Identifier/)
(이 때 도움이 되는 나중에 유효 하지 않은 의도 기부 삭제) 주문의 고유 ID와 일치 하는 합니다. 그런 다음 상호 작용은 단체에 기부 Siri 합니다.

에 대 한 호출을 `order.Intent` getter 인출을 `OrderSoupIntent` 설정 하 여 순서를 나타내는 해당 `Quantity`를 `Soup`, `Options`, 이미지 및 사용자 레코드를 연결할 Siri에 대 한 구 때 제안으로 사용 하는 호출 구 의도로:

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

Siri 도움이 되지 않는 또는 혼란 스러운 바로 가기 제안을 있도록 유효한 더 이상 없는 기부를 제거 하는 것이 반드시 합니다.

Soup Chef에는 **구성 메뉴** 화면 메뉴 항목 기간은 사용 불가로 표시 데 사용할 수 있습니다. Siri 해야 사용할 수 없는 메뉴 항목을 주문 하려면 바로 가기를 제안 이상 이므로 `RemoveDonation` 메서드의 `SoupMenuManager` 기부 더 이상 사용할 수 있는 메뉴 항목을 삭제 합니다. 이를 위해:

- 이제 사용할 수 없는 메뉴 항목과 연결 된 주문 찾기
- 해당 식별자를 수집 합니다.
- 동일한 식별자가 있는 상호 작용을 삭제 합니다.

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

### <a name="creating-an-intents-extension"></a>인 텐트 확장 만들기

Siri 의도 호출할 때 실행 되는 코드를 새 프로젝트로 기존 Xamarin.iOS 앱 Soup Chef 등으로 동일한 솔루션에 추가할 수 있는 인 텐트 확장에 배치 됩니다. 에 **SoupChef** 솔루션을 확장 이라고 **SoupChefIntents**합니다.

#### <a name="soupchefintents-infoplist-and-entitlementsplist"></a>SoupChefIntents – Info.plist 및 Entitlements.plist

##### <a name="soupchefintents-infoplist"></a>SoupChefIntents – Info.plist

**Info.plist** 에 **SoupChefIntents** 프로젝트 정의 **번들 식별자** 으로 `com.xamarin.SoupChef.SoupChefIntents`입니다.

합니다 **Info.plist** 파일 다음에도 포함 되어 있습니다.

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

위의 **Info.plist**:

- `IntentsRestrictedWhileLocked` 장치가 잠긴 경우에 처리 해야 하는 의도 나열 합니다.
- `IntentsSupported` 이 확장 프로그램이 처리 의도 나열 합니다.
- `NSExtensionPointIdentifier` 앱 확장의 유형을 지정 합니다 (참조 [Apple 설명서](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/AppExtensionKeys.html#//apple_ref/doc/uid/TP40014212-SW15) 자세한).
- `NSExtensionPrincipalClass` 이 확장 프로그램에서 지원 되는 인 텐트 처리를 사용 해야 하는 클래스를 지정 합니다.

##### <a name="soupchefintents-entitlementsplist"></a>SoupChefIntents – Entitlements.plist

합니다 **Entitlements.plist** 에 **SoupChefIntents** 프로젝트에는 **앱 그룹** 기능입니다. 이 기능은 동일한 앱 그룹을 사용 하도록 구성 되어 합니다 **SoupChef** 프로젝트:

```xml
<key>com.apple.security.application-groups</key>
<array>
    <string>group.com.xamarin.SoupChef</string>
</array>
```

사용 하 여 데이터를 유지 하는 soup Chef `NSUserDefaults`합니다. 앱에서 앱 확장 사이 데이터를 공유 하기 위해 참조 하는 동일한 응용 프로그램 그룹에 해당 **Entitlements.plist** 파일입니다.

> [!NOTE]
> 합니다 **SoupChefIntents** 프로젝트의 빌드 구성 집합 **사용자 지정 자격** 하 **Entitlements.plist**합니다.

#### <a name="handling-an-ordersoupintent-background-task"></a>OrderSoupIntent 백그라운드 작업을 처리합니다.

인 텐트 확장을 사용자 지정 의도에 따라 바로 가기에 대 한 필요한 백그라운드 작업을 실행 합니다.

Siri 호출을 [ `GetHandler` ](https://developer.xamarin.com/api/member/Intents.INExtension.GetHandler/) 메서드를 `IntentHandler` 클래스 (에 정의 된 **Info.plist** 으로 `NSExtensionPrincipalClass`) 확장 하는 클래스의 인스턴스를 가져옵니다 `OrderSoupIntentHandling`, 사용할 수 있는 처리 하는 `OrderSoupIntent`:

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

`OrderSoupIntentHandler`에 정의 된 **SoupKit** 공유 코드에 중요 한 메서드를 구현 두 프로젝트:

- `ConfirmOrderSoup` – 의도 사용 하 여 연결 된 작업 해야 실제로 실행할 수 있는지 여부 확인
- `HandleOrderSoup` – Soup 순서를 배치 하 고 전달 된 완료 처리기를 호출 하 여 사용자에 게 응답

#### <a name="handling-an-ordersoupintent-that-opens-the-app"></a>앱이 열리는 OrderSoupIntent 처리

앱을 백그라운드에서 실행 하지 않는 의도 제대로 처리 해야 합니다.
이러한 동일한 방식으로 처리 됩니다 `NSUserActivity` 바로 가기, 합니다 `ContinueUserActivity` 메서드의 `AppDelegate`:

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

## <a name="providing-a-user-interface-for-a-custom-intent"></a>사용자 지정 의도 대 한 사용자 인터페이스를 제공합니다.

인 텐트 확장에 대 한 사용자 지정 사용자 인터페이스를 제공 하는 인 텐트 UI 확장 합니다. 에 **SoupChef** 솔루션 **SoupChefIntentsUI** 에 대 한 인터페이스를 제공 하는 인 텐트 UI 확장 프로그램 **SoupChefIntents**합니다.

### <a name="soupchefintentsui--infoplist-and-entitlementsplist"></a>SoupChefIntentsUI – Info.plist 및 Entitlements.plist

#### <a name="soupchefintentsui-infoplist"></a>SoupChefIntentsUI – Info.plist

**Info.plist** 에 **SoupChefIntentsUI** 프로젝트 정의 **번들 식별자** 으로 `com.xamarin.SoupChef.SoupChefIntentsui`입니다.

합니다 **Info.plist** 파일 다음에도 포함 되어 있습니다.

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

위의 **Info.plist**:

- `IntentsSupported` 나타내는 `OrderSoupIntent` 이 인 텐트 UI 확장에 의해 처리 됩니다.
- `NSExtensionPointIdentifier` 앱 확장의 유형을 지정 합니다 (참조 [Apple 설명서](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/AppExtensionKeys.html#//apple_ref/doc/uid/TP40014212-SW15) 자세한).
- `NSExtensionMainStoryboard` 이 확장의 기본 인터페이스를 정의 하는 스토리 보드를 지정 합니다.

#### <a name="soupchefintentsui-entitlementsplist"></a>SoupChefIntentsUI – Entitlements.plist

합니다 **SoupChefIntentsUI** 프로젝트는 필요 하지 않습니다는 **Entitlements.plist** 파일입니다.

### <a name="creating-the-user-interface"></a>사용자 인터페이스 만들기

이후를 **Info.plist** 에 대 한 **SoupChefIntentsUI** 설정 합니다 `NSExtensionMainStoryboard` 키를 `MainInterface`, **MainInterace.storyboard** 파일에 대 한 인터페이스를 정의 합니다. 인 텐트 UI 확장입니다.

이 스토리 보드에는 형식의 단일 뷰 컨트롤러 **IntentViewController**합니다. 두 보기를 참조합니다.

- **invoiceView**, 형식 `InvoiceView`
- **confirmationView**, 형식 `ConfirmOrderView`

> [!NOTE]
> 에 대 한 인터페이스 **invoiceView** 하 고 **confirmationView** 에 정의 된 **Main.storyboard** 보조 뷰로 합니다. IOS Mac 및 Visual Studio 2017 용 Visual Studio의 디자이너 보기 또는 편집 보조 뷰;에 대 한 지원 하지 않습니다. 이렇게 하려면 엽니다 **Main.storyboard** Xcode의 Interface Builder에서.

`IntentViewController` 구현 하는 [`IINUIHostedViewControlling`](https://developer.xamarin.com/api/type/IntentsUI.IINUIHostedViewControlling/)
인터페이스, Siri 의도 사용 하 여 작업 하는 경우 사용자 지정 인터페이스를 제공 하는 데 사용 합니다. 는 [`ConfigureView`](https://developer.xamarin.com/api/member/IntentsUI.INUIHostedViewControlling_Extensions.ConfigureView/)
메서드를 호출 하는 인터페이스를 확인 하거나 상호 작용 확인 되는 여부에 따라 청구서에 표시를 사용자 지정 ([`INIntentHandlingStatus.Ready`](https://developer.xamarin.com/api/type/Intents.INIntentHandlingStatus/)) 또는 성공적으로 실행 되었습니다 ([ `INIntentHandlingStatus.Success`](https://developer.xamarin.com/api/type/Intents.INIntentHandlingStatus/)):

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
> 에 대 한 자세한 내용은 합니다 `ConfigureView` 메서드를 Apple WWDC 2017 프레젠테이션을 시청 [What's New in SiriKit](https://developer.apple.com/videos/play/wwdc2017/214/)합니다.

## <a name="creating-a-voice-shortcut"></a>음성 바로 가기 만들기

Soup Chef Siri 사용 하 여 주문 soup 수 있도록 음성 바로 가기를 할당 하는 각 주문에 대 한 인터페이스를 제공 합니다. 사실, 기록 및 음성 바로 가기를 할당 하는 데 사용 하는 인터페이스를 iOS에서 제공 하는 및 작은 사용자 지정 코드가 필요 합니다.

`OrderDetailViewController`은 사용자가 테이블의 경우 **Siri 추가할** 행을 [ `RowSelected` ](https://developer.xamarin.com/api/member/UIKit.UITableViewSource.RowSelected/) 메서드를 추가 하거나 음성 바로 가기를 편집 화면 표시:

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

현재 표시 된 주문에 대 한 기존 음성 바로 가기 있는지 여부에 따라 `RowSelected` 형식의 뷰 컨트롤러를 제시 [ `INUIEditVoiceShortcutViewController` ](https://developer.xamarin.com/api/type/IntentsUI.INUIEditVoiceShortcutViewController/) 하거나 [ `INUIAddVoiceShortcutViewController` ](https://developer.xamarin.com/api/type/IntentsUI.INUIAddVoiceShortcutViewController/).
각 예에서 `OrderDetailViewController` 뷰 컨트롤러의 자체 설정 `Delegate`, 또한 구현 하는 이유는 [`IINUIAddVoiceShortcutViewControllerDelegate`](https://developer.xamarin.com/api/type/IntentsUI.IINUIAddVoiceShortcutViewControllerDelegate/)
및 [ `IINUIEditVoiceShortcutViewControllerDelegate` ](https://developer.xamarin.com/api/type/IntentsUI.IINUIEditVoiceShortcutViewControllerDelegate/)합니다.

## <a name="testing-on-device"></a>장치에서 테스트

장치의 Soup Chef를 실행 하려면 아래 지침을 따릅니다. 또한 읽기를 [자동 프로 비전 하는 방법에 대 한 참고](#automatic-provisioning)합니다.

### <a name="app-group-app-ids-provisioning-profiles"></a>앱 그룹, 앱 Id 프로 비전 프로필

에 **인증서, Id 및 프로필** 섹션을 [Apple Developer 포털](https://developer.apple.com/), 다음을 수행:

- Soup Chef 앱 및 해당 확장 간에 데이터를 공유할 앱 그룹을 만듭니다. 예를 들어: **group.com.yourcompanyname.SoupChef**

- 세 가지 앱 Id 만들기: 앱 자체에 대 한 고은 인 텐트 확장에 대 한 인 텐트 UI 확장 프로그램용입니다. 예를 들어:

    - 앱: **com.yourcompanyname.SoupChef**
        - 이 앱 id에 SiriKit을 할당 하 고 **앱 그룹** 기능입니다.

    - 인 텐트 확장: **com.yourcompanyname.SoupChef.Intents**
        - 이 앱 id에 할당 합니다 **앱 그룹** 기능입니다.

    - 인 텐트 UI 확장: **com.yourcompanyname.SoupChef.Intentsui**
        - 이 앱 ID가 없습니다 특별 한 기능이 필요 합니다.

- 위의 앱 Id를 만든 후 편집 합니다 **앱 그룹** 위에서 만든 앱의 특정 앱 그룹을 지정 하는 인 텐트 확장을 할당 하는 기능입니다.

- 세 개의 새 개발 프로 비전 프로필을 하나씩 새 앱 Id를 만듭니다.

- 이러한 프로 비전 프로필을 다운로드 및 설치에 각각 두 번 클릭 합니다. Visual Studio 2017 또는 Mac 용 Visual Studio가 이미 실행 중이면 새 프로 비전 프로필을 등록 하는지 다시 시작 합니다.

### <a name="editing-infoplist-entitlementsplist-and-source-code"></a>Info.plist, Entitlements.plist, 및 소스 코드를 편집합니다.

Mac 또는 Visual Studio 2017에 대 한 Visual Studio에서 다음을 수행 합니다.

- 다양 한 업데이트 **Info.plist** 솔루션의 파일입니다. 앱, 인 텐트 확장 및 인 텐트 UI 확장을 설정할 **번들 식별자** 앞에서 정의한 앱 id:

    - 앱: **com.yourcompanyname.SoupChef**
    - 인 텐트 확장: **com.yourcompanyname.SoupChef.Intents**
    - 인 텐트 UI 확장: **com.yourcompanyname.SoupChef.Intentsui**

- 업데이트를 **Entitlements.plist** 에 대 한 파일을 **SoupChef** 프로젝트:
    - 에 대 한 합니다 **앱 그룹** 기능을 위에서 만든 새 앱 그룹으로 그룹 설정 (위의 예제에 **group.com.yourcompanyname.SoupChef**).
    - 했는지 **SiriKit** 사용 가능 합니다.

- 업데이트를 **Entitlements.plist** 에 대 한 파일을 **SoupChefIntents** 프로젝트:
    - 에 대 한 합니다 **앱 그룹** 기능을 위에서 만든 새 앱 그룹으로 그룹 설정 (위의 예제에 **group.com.yourcompanyname.SoupChef**).

- 마지막으로 열려면 **NSUserDefaultsHelper.cs**합니다. 설정 된 `AppGroup` 새 앱 그룹의 값으로 변수 (예를 들어로 `group.com.yourcompanyname.SoupChef`).

### <a name="configuring-the-build-settings"></a>빌드 설정 구성

Visual Studio에서 Mac 또는 Visual Studio 2017에 대 한 합니다.

- 옵션/속성을 엽니다는 **SoupChef** 프로젝트입니다. 에 **iOS Bundle Signing** 탭에서 **서명 Id** 자동으로 및 **프로 비전 프로필** 새 앱 별 프로비저닝 프로필에 위에서 만든 합니다.

- 옵션/속성을 엽니다는 **SoupChefIntents** 프로젝트입니다. 에 **iOS Bundle Signing** 탭에서 **서명 Id** 자동으로 및 **프로 비전 프로필** 만든 새 의도 확장 프로그램별 프로 비전 프로필을 위의 합니다.

- 옵션/속성을 엽니다는 **SoupChefIntentsUI** 프로젝트입니다. 에 **iOS Bundle Signing** 탭에서 **서명 Id** 자동으로 및 **프로 비전 프로필** 새로운 인 텐트 ui 확장 프로그램별 프로 비전 프로필 위에서 만든 합니다.

위치에서 이러한 변경으로 앱이 iOS 장치에서 실행 됩니다.

### <a name="automatic-provisioning"></a>자동 프로 비전

사용할 수 있는 참고 [자동 프로 비전](https://docs.microsoft.com/en-us/xamarin/ios/get-started/installation/device-provisioning/automatic-provisioning) 프로 비전 작업은 IDE에서 직접 이들 중 대부분을 수행 하 합니다.
그러나 자동 프로 비전 앱 그룹을 설정 하지 않습니다. 수동으로 구성 해야 합니다 **Entitlements.plist** 파일을 사용 하려는 app 그룹의 이름으로 앱 그룹을 자동으로 만든 각 앱 ID에 해당 앱 그룹을 할당 하려면 Apple 개발자 포털을 방문 프로 비전을 프로 비전 프로필 (앱, 인 텐트 확장, 인 텐트 UI 확장) 및 새로 만든된 앱 그룹을 포함 하 고, 다운로드 하 고, 설치를 다시 생성 합니다.

## <a name="related-links"></a>관련 링크

- [Soup Chef (Xamarin)](https://developer.xamarin.com/samples/monotouch/ios12/SoupChef/)
- [Soup Chef (Swift)](https://developer.apple.com/documentation/sirikit/accelerating_app_interactions_with_shortcuts?language=objc)
- [SiriKit (Apple)](https://developer.apple.com/sirikit/)
- [Siri 바로 가기 – WWDC 2018 소개](https://developer.apple.com/videos/play/wwdc2018/211/)
- [Siri 바로 가기를 – WWDC 2018 음성에 대 한 빌드](https://developer.apple.com/videos/play/wwdc2018/214/)
- [Siri 시계 화면의 – WWDC 2018에서 Siri 바로 가기](https://developer.apple.com/videos/play/wwdc2018/217/)
- [SiriKit – WWDC 2017의에서 새로운 기능](https://developer.apple.com/videos/play/wwdc2017/214/)
- [인 텐트 앱 확장 (Apple) 만들기](https://developer.apple.com/documentation/sirikit/creating_an_intents_app_extension?language=objc)
