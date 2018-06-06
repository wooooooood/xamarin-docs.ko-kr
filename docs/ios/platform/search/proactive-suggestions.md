---
title: Xamarin.iOS에서 사전 제안 소개
description: 이 문서에서는 시스템을 적극적으로 사용자에 게 유용한 정보를 자동으로 표시 함으로써 Xamarin.iOS 앱 드라이브 engagement에 제안을 자동 관리를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 8DDD084A-0D1E-4DF7-B686-6309DCEFF5D3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: f736e9dda00546ddef7cf03457813c7e3d10882b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788021"
---
# <a name="introduction-to-proactive-suggestions-in-xamarinios"></a>Xamarin.iOS에서 사전 제안 소개

_이 문서에서는 시스템을 적극적으로 사용자에 게 유용한 정보를 자동으로 표시 함으로써 Xamarin.iOS 앱 드라이브 engagement에 제안을 자동 관리를 사용 하는 방법을 설명 합니다._

새로운 iOS 10, 자동 관리 제안 있는 뉴스 방법 사용자에 게 적절 한 시간에 자동으로 사용자에 게 사전 있는 유용한 정보 여 Xamarin.iOS 앱과 의견을 교환 합니다.

iOS 10 시스템에 사전 유용한 정보를 자동으로 사용자에 게 표시 적절 한 시간에 허용 하 여 구동 engagement 응용 프로그램에 새로운 방법에 설명 합니다. 9 iOS와 마찬가지로 스포트라이트, 전달 및 Siri 제안을 사용 하 여 앱에 심층 검색을 추가 하는 기능을 제공 (참조 [새로운 검색 Api](~/ios/platform/search/index.md)), iOS 또는 10과 응용 프로그램 내에서 시스템에서 사용자에 게 제공할 수 있는 기능을 노출할 수는 다음 위치:

- 응용 프로그램 전환기
- 잠금 화면
- CarPlay
- 맵
- Siri 상호 작용
- QuickType 제안

와 같은 기술의 모음을 사용 하 여 시스템에이 기능을 노출할 응용 프로그램이 `NSUserActivity`, 코어 스포트라이트, MapKit, 미디어 플레이어 및 UIKit 웹 태그입니다. 또한 응용 프로그램에 대 한 사전 제안 지원 함으로써 가져옵니다 밀접 하 게 Siri 통합을 무료입니다.

## <a name="location-based-suggestions"></a>위치 기반 추천

10, iOS에 새는 `NSUserActivity` 클래스를 포함 한 `MapItem` 속성을 사용 하 고 개발자에 게 다른 컨텍스트에서 사용할 수 있는 위치 정보를 제공 합니다. 예를 들어 경우 음식점 리뷰를 표시 하는 응용 프로그램 개발자 설정할 수는 `MapItem` 속성을 사용자가 응용 프로그램에서 보고 하는 레스토랑의 위치입니다. 지도 앱으로 전환, 식당의 위치는 자동으로 사용할 수 있습니다.

새 주소 구성 요소를 사용할 수 있는 앱에서 앱 검색을 지 원하는 경우는 `CSSearchableItemAttributesSet` 사용자 방문 하려고 수 있는 위치를 지정 하는 클래스입니다. 설정 하 여는 `MapItem` 속성을 다른 속성은에 자동으로 채워집니다.

설정 뿐만 아니라는 `Latitude` 및 `Longitude` 주소 구성 요소 속성의 것이 좋습니다 응용 프로그램을 제공 합니다는 `NamedLocation` 및 `PhoneNumbers` 속성 구조를 따라서 Siri 수 전화를 걸기 시작 위치입니다.

## <a name="web-markup-based-suggestions"></a>웹 태그 기반 추천

iOS 9 스포트라이트 및 Safari 검색 결과에서 사용자에 게 표시 하는 콘텐츠를 강화 하는 웹 사이트에 구조화 된 데이터 태그를 포함 하는 기능에 추가 (참조 [웹 태그를 사용 하 여 검색](~/ios/platform/search/web-markup.md)). 위치 기반 태그를 포함 하는 기능을 추가 하는 iOS 10 (같은 [마쳤습니다](http://schema.org/PostalAddress) 에 정의 된 대로 [Schema.org](http://schema.org/)) 사용자 환경을 향상 합니다. 예를 들어 웹 사이트에서 페이지를 표시 하는 위치는 사용자가 뷰 시스템에 지도 열 때 동일한 위치 제안 받을 수 있습니다.

## <a name="text-based-suggestions"></a>텍스트 기반 추천

UIKit ios 10 포함 하도록 확장 되었습니다는 [TextContentType](https://developer.apple.com/reference/uikit/uitextinputtraits/1649656-textcontenttype) 의 속성은 [UITextInputTraits](https://developer.apple.com/reference/uikit/uitextinputtraits) 텍스트 영역에는 내용의 의미 체계 의미를 지정 하는 클래스입니다. 위치에이 정보를 시스템 수 일반적으로 자동으로 해당 키보드 종류, 자동 고침 제안을 향상 시킵니다 선택한 사전 다른 앱과 웹 사이트에서 정보를 통합 합니다.

예를 들어, 사용자가 표시 된 텍스트 필드에 텍스트를 입력 하는 경우 `UITextContentType.FullStreetAddress`, 시스템에 자동 채우기 필드는 사용자가 보고 최근에 위치와 제안 받을 수 있습니다.

## <a name="media-based-suggestions"></a>미디어 기반 추천

응용 프로그램 사용 하 여 미디어를 재생 하는 경우는 [MPPlayableContentManager](https://developer.xamarin.com/api/type/MediaPlayer.MPPlayableContentManager/) API iOS 10에는 사용자가 볼 앨범 및 잠금 화면에서 앱을 통해 미디어를 재생할 수 있습니다.

## <a name="contextual-siri-reminders"></a>상황에 맞는 Siri 미리 알림

사용자를 신속 하 게 콘텐츠를 보려면 나중에 응용 프로그램에서 현재 보고는 미리 알림을 만들려면 Siri를 사용할 수 있습니다. 예를 들어 음식점 리뷰 보았던은 앱의 경우 한 수 있습니다 Siri 호출 예를 들어 *"메시지를이 대 한 홈 받으면."* Siri 앱에서 알림을 검토 링크와 함께 발생 합니다.

## <a name="contact-based-suggestions"></a>연락처 기반 추천

응용 프로그램의 허용 연락처 (및 관련된 연락처 정보)에 **문의** 시스템에 저장 된 모든 사용자가 연락처와 함께 iOS 장치에서 앱입니다.

## <a name="ride-sharing-based-suggestions"></a>공유 안에서 기반 추천

안에서 공유 응용 프로그램을 사용 하는 경우는 [MKDirectionsRequest](https://developer.xamarin.com/api/type/MapKit.MKDirectionsRequest/) API iOS 10 됩니다 제공 응용 프로그램 전환기에 옵션으로 않을 때 사용자가을 안에서 싶을 것입니다. 응용 프로그램 에서도 등록 해야 안에서 공유 응용 프로그램으로 지정 하 여는 `MKDirectionsModeRideShare` 에 대 한는 [MKDirectionsApplicationSupportedModes](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html) 키에 해당 `Info.plist` 파일입니다.

시스템 제안 시작 합니다 앱만을 지 원하는 경우 안에서 공유 *"... 하 안에서 Get"*, 시스템에서 사용 됩니다 (예: Walking 또는 자전거) 라우팅 방향 다른 유형의 지원 되 면 *"위해 방향으로 Get..."*

> [!IMPORTANT]
> [MKMapItem](https://developer.xamarin.com/api/type/MapKit.MKMapItem/) 응용 프로그램을 수신 하는 개체 경도 위도 정보를 포함 하지 않을 수 있습니다 및 지 오 코딩 해야 합니다.

## <a name="implementing-proactive-suggestions"></a>사전 제안을 구현

추가 사전 제안 Xamarin.iOS 앱에 지원은 일반적으로 몇 가지 Api를 구현 하거나 응용 프로그램에서 이미 구현할 수 있는 몇 가지 Api에 확장 하는 것 만큼 쉽지 합니다.

사전 추천 앱으로 세 가지 주요 방법으로 작동합니다.

- **`NSUserActivity` 및 Schema.org**  -  `NSUserActivity` 화면에 사용자가 현재 사용 정보를 이해 하는 시스템을 사용 합니다. Schema.org 웹 페이지에 유사한 기능을 추가합니다.
- **위치 제안** -응용 프로그램을 제공 하거나 응용 프로그램 간에이 정보를 공유 하도록 기반 위치 정보를 이러한 API 확장 제공은 새로운 방법을 소비 하는 경우.
- **미디어 앱 제안** -시스템 응용 프로그램을 승격할 수 및 iOS 장치에서 사용자의 상호 작용의 컨텍스트를 기반으로 해당 미디어 콘텐츠입니다.

다음을 구현 하 여 응용 프로그램에서 지원 됩니다.

- **핸드 오프**  -  `NSUserActivity` iOS 8 사용 하면 한 장치에서 작업을 시작 하 고 다른 계속 개발자는 핸드 오프를 지원 하기 위해 추가 되었습니다 (참조 [핸드 오프 소개](~/ios/platform/handoff.md)).
- **스포트라이트 검색** -스포트라이트 검색 결과 사용 하 여 내에서 응용 프로그램 콘텐츠를 승격 하는 기능을 추가 하는 iOS 9 `NSUserActivity` (참조 [스포트라이트 코어를 사용 하 여 검색](~/ios/platform/search/corespotlight.md)).
- **상황에 맞는 Siri 미리 알림** -10, iOS에서 `NSUserActivity` 신속 하 게 응용 프로그램에서 현재 사용자가 보고 나중에 콘텐츠를 보려면 미리 알림을 만들려면 Siri 허용 하도록 확장 되었습니다.
- **위치 제안** -iOS 10 향상 `NSUserActivity` 를 응용 프로그램 내에서 표시 하는 위치를 캡처하고 시스템 전체에서 여러 위치에서 승격 합니다.
- **상황에 맞는 Siri 요청**  -  `NSUserActivity` 컨텍스트 사용자 얻을 수 있도록 호출 될 위치 또는 방향으로 응용 프로그램 내에서 호출 Siri Siri를 내부 응용 프로그램에서 제공 되는 정보를 제공 합니다.
- **상호 작용 문의** iOS 10의에서 새로운 기능- `NSUserActivity` 통신 앱을 대체 통신 방법으로 (연락처 앱)에서 대화 상대 카드에서 승격 될 수 있도록 허용 합니다.

이러한 모든 기능은 공통점이 하나, 모두 사용 `NSUserActivity` 한 형식이 나 다른 해당 기능을 제공 합니다. 

## <a name="nsuseractivity"></a>NSUserActivity

위의 설명 대로 `NSUserActivity` 화면에 사용자가 현재 사용 정보를 이해 하는 시스템을 사용 합니다. `NSUserActivity` 간단한 상태 캐싱 응용 프로그램을 탐색할 때 사용자의 활동을 캡처할 수는 메커니즘입니다. 예를 들어 식당 응용 프로그램을 살펴보면 다음과 같습니다.

[![](proactive-suggestions-images/activity02.png "캐싱 메커니즘 NSUserActivity 간단한 상태")](proactive-suggestions-images/activity02.png#lightbox)

다음과 같은 상호:

1. 사용자 앱과 함께 작동 한 `NSUserActivity` 를 나중에 응용 프로그램의 상태를 다시 생성 됩니다.
2. 사용자가 식당을 검색 하는 경우 동일한 패턴의 활동을 만드는 다음 됩니다.
3. 하 고 다시 사용자 결과 봅니다. 이 경우, 사용자가 위치를 보고 되며 10 ios에서는 시스템 (예: 위치 또는 통신 상호 작용) 특정 개념에 대해 알고 있습니다.

마지막 화면에 자세히 살펴보겠습니다.

[![](proactive-suggestions-images/activity03.png "NSUserActivity 세부 정보")](proactive-suggestions-images/activity03.png#lightbox)

응용 프로그램을 만드는 것 여기는 `NSUserActivity` 및 상태를 나중에 다시 정보로 구성 되었습니다. 응용 프로그램 해당 위치의 이름 및 주소와 같은 일부 메타 데이터도 포함 했습니다. 이 활동과 만든 응용 프로그램에 iOS를 사용자의 현재 상태를 나타내는 것을 알 수 있습니다.

응용 프로그램 경우 활동 전달 하기 위해 공중파 보급 알림, 위치 제안에 대 한 임시 값으로 저장 되거나 됩니다 검색 결과에 표시 하기 위한 장치 스포트라이트 인덱스에 추가 결정 합니다.

핸드 오프 및 스포트라이트 검색에 대 한 자세한 내용은 참조 하십시오 우리의 [핸드 오프 소개](~/ios/platform/handoff.md) 및 [iOS 9 새로운 검색 Api](~/ios/platform/search/index.md) 가이드입니다.

### <a name="creating-an-activity"></a>활동 만들기

활동을 만들기 전에 활동 유형 식별자 식별 하기 위해 만들어야 할 수 있습니다. 활동 형식 식별자는 짧은 문자열에 추가 된 `NSUserActivityTypes` 응용 프로그램의 배열을 `Info.plist` 지정된 된 사용자 활동 형식을 고유 하 게 식별 하는 데 사용 되는 파일입니다. 응용 프로그램을 지원 하 고 응용 프로그램 검색에 노출 하는 각 활동에 대 한 배열에 하나의 항목이 됩니다. 참조 우리의 [활동 형식 식별자 참조 만들기](~/ios/platform/search/nsuseractivity.md) 자세한 세부 정보에 대 한 합니다.

활동의 예를 확인 합니다.

```csharp
// Create App Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Enable capabilities
activity.EligibleForSearch = true;
activity.EligibleForHandoff = true;
activity.EligibleForPublicIndexing = true;

// Inform system of Activity
activity.BecomeCurrent();
```

새 활동은 활동 유형 식별자를 사용 하 여 생성 됩니다. 다음으로이 상태는 나중에 복원할 수는 활동을 정의 하는 일부 메타 데이터 생성 됩니다. 그런 다음 활동 의미 있는 제목이 지정 된를 사용자 정보에 연결 합니다. 마지막으로 일부 기능을 사용 하 고 활동 시스템에 전송 됩니다.

위의 코드를 추가로 다음과 같이 변경 하 여 활동에 대 한 컨텍스트를 제공 하는 메타 데이터를 포함 하도록 개선할 수 있습니다.

```csharp
...

// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
attributes.ThumbnailUrl = myThumbnailURL;
attributes.Keywords = new string [] { "software", "mobile", "language" }; 
activity.ContentAttributeSet = attributes;

// Inform system of Activity
activity.BecomeCurrent();
```

개발자는 앱과 동일한 정보를 표시할 수 있는 웹 사이트에 있는 경우 응용 프로그램 URL을 포함할 수 있습니다 및 (전달)를 통해 설치 된 응용 프로그램을 갖지 않는 다른 장치에 콘텐츠를 표시할 수 있습니다.

```csharp
// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");
```

### <a name="restoring-an-activity"></a>복원 작업

검색 결과에서 누르기 사용자에 게 응답 하도록 (`NSUserActivity`)는 앱에 대 한 편집는 **AppDelegate.cs** 파일을 재정의 하는 `ContinueUserActivity` 메서드. 예를 들어:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    }

    return true;
}
```

개발자 동일한 활동 유형 식별자 인지 확인 해야 합니다 (`com.xamarin.platform`) 위에서 만든 활동으로 합니다. 앱에 저장 된 정보를 사용 하는 `NSUserActivity` 사용자 중단 이동 상태를 복원 합니다.

### <a name="benefits-of-creating-an-activity"></a>활동 만들기의 이점

최소한의 코드만 위에서 설명한 앱은 이제 세 개의 새로운 iOS 10 기능을 활용할 수 있습니다:

- **Handoff**
- **스포트라이트 검색**
- **상황에 맞는 Siri 미리 알림**

다음 섹션에는 다른 두 개의 새 iOS 10 기능을 활성화를 살펴보면을 소요 됩니다.

- **위치 제안**
- **상황에 맞는 Siri 요청**

### <a name="location-based-suggestions"></a>위치 기반 추천 

위의 식당 검색 응용 프로그램의 예로 보겠습니다. 이 구현 하는 경우 `NSUserActivity` 하 고 올바르게 채워져 모든 메타 데이터와 특성을 사용자는 다음을 수행할 수 있습니다.

1. 식당을에서 친구를 충족 하고자 하는 앱에서 찾습니다.
2. 멀티태스킹 응용 프로그램 전환기를 사용 하 여 앱에서 사용자를 이동할 때 시스템 자동으로 표시 됩니다 제안 (화면 맨 위에 있음)의 즐겨 찾는 탐색 응용 프로그램을 사용 하 여 음식점를 합니다.
3. 사용자 메시지 앱으로 전환 하 고 입력을 시작 하는 경우 *"시에 회의가"*, QuickType 키보드는 식당의 주소에 붙여 넣어를 자동으로 제안 합니다.
4. 지도 앱으로 전환, 식당의 주소를 대상으로 자동으로 제안 됩니다.
5. 이 타사 앱에도 적합 (지 원하는 `NSUserActivity`), 식당의 주소는 자동으로 제안 있습니다를 대상으로도 하 고 사용자 안에서 공유 응용 프로그램으로 전환할 수 있도록 합니다.
6. 사용자는 식당 응용 프로그램 내에서 Siri를 호출 하 고 요청 수 있으므로 Siri에 컨텍스트 또한 제공 *"Get directions..."* Siri 사용자가 보고 하 고 음식점에 대 한 지침을 제공 합니다.

위의 기능을 모두 한 가지 공통, 추천에서 원래 생성 되는 위치를 나타내는 모든은 됩니다. 위의 예제에서는 가상의 식당 검토 앱은 것입니다.

iOS 10 몇 가지 작은 수정 및 기존 프레임 워크에 대 한 추가 통해 앱에 대 한이 기능을 사용 하도록 향상 되었습니다.

- `NSUserActivity` 앱 내에서 표시 되는 위치 정보를 캡처를 위한 추가 필드에 있습니다.
- 몇 가지 추가에 적용 된 MapKit 및 CoreSpotlight 위치를 캡처할 수 있습니다.
- 위치 인식 기능 Siri, 지도, 키보드, 다중 작업 및 시스템 내에서 다른 앱에 추가 되었습니다.

위치 기반 추천을 구현 하려면 위에 나와 있는 동일한 활동 코드로 시작:

```csharp
// Create App Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Enable capabilities
activity.EligibleForSearch = true;
activity.EligibleForHandoff = true;
activity.EligibleForPublicIndexing = true;

// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
attributes.ThumbnailUrl = myThumbnailURL;
attributes.Keywords = new string [] { "software", "mobile", "language" }; 
activity.ContentAttributeSet = attributes;

// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");

// Inform system of Activity
activity.BecomeCurrent();
```

응용 프로그램 MapKit를 사용 하는 경우 현재 지도 추가 하기만 `MKMapItem` 활동에:

```csharp
// Save MKMapItem location
activity.MapItem = myMapItem;
```

응용 프로그램 MapKit 사용 되지 않습니다 채택 하는 응용 프로그램 검색 있고 위치에 대 한 다음 새 특성을 지정 합니다.

```csharp
// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
...

attributes.NamedLocation = "Apple Inc.";
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

자세히 위의 코드를 살펴보세요. 첫째, 위치의 이름은 모든 상황에서 필요 합니다.

```csharp
attributes.NamedLocation = "Apple Inc.";
```

그런 다음 위치를 기반으로 텍스트 설명 (예: QuickType 키보드의 경우)를 기반으로 텍스트 인스턴스에 대 한 필요 합니다.

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

위도 및 경도 선택 사항 이지만 사용자가 앱을 포함 해야 하므로, 보낼 원하는 정확한 위치를 라우팅되는지 확인:

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

전화 번호를 설정 하 여 응용 프로그램에 액세스할 수 Siri 사용자 등의 제목을 한다는 것으로 앱에서 Siri를 호출할 수 있도록 *"이 이곳 호출할"*:

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

마지막으로, 응용 프로그램 인스턴스가 탐색 및 전화 통화에 대 한 적합 한 경우를 나타낼 수 있습니다.

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

### <a name="implementing-contact-interactions"></a>연락처의 상호 작용을 구현합니다.

새로운 ios 10에서는 통신 앱 대화 상대 카드에서 연락처 앱에 통합 밀접 하 게 됩니다. 연락처 상호 작용을 구현 하는 응용 프로그램의 경우 사용자 지정된 응용 프로그램의 정보 특정 사용자가 대화 상대에 게를 추가할 수 있습니다. 와 연결 된 응용 프로그램에서 메시지를 보낼 수 있는 옵션이으로 나열 됩니다 경우 예를 들어 메시지를 보낼 카드를 맨 위에 있는 빠른 실행 단추에 도달 합니다.

세 번째 파티 앱을 선택 하는 경우 기억 하 고 메시지는 지정 된 사용자에 게 연락 하는 사용자가 다음에 기본 방법으로 제공 수 있습니다.

연락처의 상호 작용 사용 하 여 응용 프로그램에서 구현 되는 `NSUserActivity` 및 iOS 10에에서 도입 된 새로운 의도 프레임 워크입니다. 작업 의도에 대 한 자세한 내용은 참조 하십시오 우리의 [SiriKit 개념 이해](~/ios/platform/sirikit/understanding-sirikit.md) 및 [구현 SiriKit](~/ios/platform/sirikit/implementing-sirikit.md) 가이드입니다.

#### <a name="donating-interactions"></a>기증 상호 작용

응용 프로그램 상호 작용을 지정할 수는 방법에 대해 살펴보겠습니다.

[![](proactive-suggestions-images/activity04.png "상호 작용 기증 개요")](proactive-suggestions-images/activity04.png#lightbox)

응용 프로그램을 만듭니다는 `INInteraction` 포함 된 개체는 **의도** (`INIntent`), **참가자** 및 **메타 데이터**합니다. **의도** 비디오를 호출한 또는 문자 메시지를 보내는 등의 사용자 작업을 나타냅니다. **참가자** 통신 받는 사람을 포함 합니다. **메타 데이터** 성공적으로 등 메시지를 보내는 것과 같은 추가 정보를 정의 합니다.

개발자 직접 만들지 않는다는 인스턴스의 `INIntent` 또는 `INIntentResponse`, 특정 자식 클래스 (응용 프로그램은 사용자를 대신해 서 수행 하는 작업에 따라) 중 하나 사용 합니다 이러한 부모 클래스에서 상속 하는 합니다. 예를 들어 `INSendMessageIntent` 및 `INSendMessageIntentResponse` 텍스트 메시지 전송 합니다. 

상호 작용을 완전히 채웠으므로 호출 하 여 `DonateInteraction` 상호 작용은 사용할 수 있는 시스템 알리기 위해 메서드.

사용자가 대화 상대 카드에서 앱과 상호 작용할 때 상호 작용 가져옵니다 번들로 제공 되는 `NSUserActivity`, 응용 프로그램을 실행에 사용 되는:

[![](proactive-suggestions-images/activity05.png "상호 작용 가져옵니다 응용 프로그램을 실행 하는 데 사용 되는 NSUserActivity 함께 제공 됩니다.")](proactive-suggestions-images/activity05.png#lightbox)

다음 예제에서는 보내는 메시지 의도 살펴보겠습니다.

```csharp
using System;
using Foundation;
using UIKit;
using Intents;

namespace MonkeyNotification
{
    public class DonateInteraction
    {
        #region Constructors
        public DonateInteraction ()
        {
        }
        #endregion

        #region Public Methods
        public void SendMessageIntent (string text, INPerson from, INPerson[] to)
        {

            // Create App Activity
            var activity = new NSUserActivity ("com.xamarin.message");

            // Define details
            var info = new NSMutableDictionary ();
            info.Add (new NSString ("message"), new NSString (text));

            // Populate Activity
            activity.Title = "Sent MonkeyChat Message";
            activity.UserInfo = info;

            // Enable capabilities
            activity.EligibleForSearch = true;
            activity.EligibleForHandoff = true;
            activity.EligibleForPublicIndexing = true;

            // Inform system of Activity
            activity.BecomeCurrent ();

            // Create message Intent
            var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);

            // Create Intent Reaction
            var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);

            // Create interaction
            var interaction = new INInteraction (intent, response);

            // Donate interaction to the system
            interaction.DonateInteraction ((err) => {
                // Handle donation error
                ...
            });
        }
        #endregion
    }
}
```

이 코드를 자세히 보기를 만들고의 인스턴스를 채웁니다 `NSUserActivity` (에서처럼는 [활동 만들기](#Creating-an-Activity) 위의 섹션). 인스턴스를 만들고 다음으로, `INSendMessageIntent` (에서 상속 되 `INIntent`) 보내는 메시지에 대 한 세부 정보를 채웁니다.

```csharp
var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);
```

`INSendMessageIntentResponse` 이 만들어지고 전달는 `NSUserActivity` 위에서 만든:

```csharp
var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);
```

`INInteraction` 메시지 보내기 의도에서 만들어집니다 (`INSendMessageIntent`) 및 응답 (`INSendMessageIntentResponse`) 방금 만든:

```csharp
var interaction = new INInteraction (intent, response);
```

마지막으로, 시스템은 상호 작용의 알림:

```csharp
// Donate interaction to the system
interaction.DonateInteraction ((err) => {
    // Handle donation error
    ...
});
```

완료 처리기에 응용 프로그램에 성공 하거나 실패 기부에 응답할 수 있는 전달 됩니다.

### <a name="activities-best-practices"></a>활동에 대 한 유용한 정보

활동을 작업할 때 다음 모범 사례를 제안 하는 Apple:

- 사용 하 여 `NeedsSave` 지연 페이로드 업데이트 합니다.
- 현재 활동에 대 한 강력한 참조를 확인 합니다.
- 상태를 복원 하는 데 충분 한 정보가 포함 된 작은 페이로드만 전송 됩니다.
- 활동 유형 식별자를 지정 역방향 DNS 표기법을 사용 하 여 고유 하 고 설명 있는지 확인 합니다. 

## <a name="schemaorg"></a>Schema.org

위에 나와 있는 것 처럼 `NSUserActivity` 화면에 사용자가 현재 사용 정보를 이해 하는 시스템을 사용 합니다. Schema.org 웹 페이지에 유사한 기능을 추가합니다.

Schema.org 동일한 유형의 웹 사이트에 위치 기반 상호 작용을 제공할 수 있습니다. Apple, 새 위치 제안을 네이티브 응용 프로그램에서와 마찬가지로 Safari에서 볼 때와 마찬가지로 작동 하도록 설계 되었습니다.

일부 Schema.org 배경:

- 열린 웹 태그 어휘 표준을 제공합니다.
- 웹 페이지에 구조적된 메타 데이터를 포함 하 여 작동 합니다.
- 사용할 수 있는 다양 한 개념을 나타내는 500 개 이상의 스키마가 있습니다.
- 웹 사이트에서 구현 하 여 개발자 획득할 수 있습니다 사용할 때의 이점 중 일부 `NSUserActivity` 네이티브 응용 프로그램에 있습니다.

스키마 트리 구조, 특정과 같은 형식이 같은 정렬 되는 *식당*와 같은 보다 일반적인 형식에서 상속할 *로컬 비즈니스*합니다. 자세한 내용은 참조 하십시오 [Schema.org](http://schema.org)합니다.

예를 들어, 웹 페이지는 다음 데이터를 포함 하는 경우:

```xml
<script type="application/ld+json>
{
    "@context":"http://schema.org",
    "@type":"Restaurant",
    "telephone":"(415) 781-1111",
    "url":"https://www.yanksing.com",
    "address":{
        "@type":"PostalAddress",
        "streetAddress":"101 Spear St",
        "addressLocality":"San Francisco",
        "postalCode":"94105",
        "addressRegion":"CA"
    },
    "aggregateRating":{
        "@type":"AggregateRating",
        "ratingValue":"3.5",
        "reviewCount":"2022"
    }
}
</script>
```

사용자가 Safari에서이 페이지를 방문 하 다른 앱으로 전환 하는 경우 페이지에서 위치 정보를 캡처할 하 고 (처럼 활동 위의) 시스템의 다른 부분에 위치 제안 값으로 제공 되 게 됩니다.

Safari는 다음과 같은 스키마 속성을 준수 하는 웹 페이지에 있는 모든 내용을 추출 합니다.

- **다음 표**
- **GeoCoordinates**
- 전화 속성입니다.

자세한 내용은 참조 하십시오 우리의 [웹 태그를 사용 하 여 검색](~/ios/platform/search/web-markup.md) 가이드입니다.

## <a name="consuming-location-suggestions"></a>사용 위치 제안

이 다음 섹션은 시스템 (예: 지도 앱) 또는 다른 타사 앱의 다른 부분과에서 왔 위치 제안 사용을 설명 합니다.

응용 프로그램 위치 제안을 사용할 수 있는 다음 두 가지가 있습니다.

- 통해 QuickType 키보드입니다.
- 라우팅 앱에서 위치 정보를 사용 했습니다가 직접.

### <a name="location-suggestions-and-the-quicktype-keyboard"></a>위치 제안 및 QuickType 키보드

앱 텍스트 기반 형식으로 주소를 다루는, 경우 응용 프로그램 위치 제안 QuickType UI를 통해 이용할 수 있습니다. iOS 10 다음 기능을 통해 QuickType UI를 확장합니다.

- 응용 프로그램 UI 텍스트 필드에 대 한 의미 체계 여부에 대 한 힌트를 추가할 수 있습니다.
- 응용 프로그램 응용 프로그램에서 사전 제안을 확인할 수 있습니다.
- 응용 프로그램에서 향상 된 자동 고침 이점을 얻을 수 있습니다.

새 `TextContentType` iOS 10에서에서 텍스트 필드 컨트롤의 속성을 사용 하면 개발자는 사용자가을 계산할 값을 지정된 된 필드에 입력에 대 한 의미 체계 여부를 정의할 수 있습니다. 예를 들어:

```csharp
var textField = new UITextField();
textField.TextContentType = UITextContentType.FullStreetAddress;
```

점수 시스템 응용 프로그램에 게 필요한 사용자 지정된 필드에 전체 주소를 입력할 수 있습니다. 따라서 QuickType 키보드를 자동으로 사용자를이 필드에 값을 입력할 때 바로 가기 키 위치 제안을 제공할 수 있습니다.

다음은 개발자에 게 제공 하는 보다 일반적인 유형 중 일부는 `UITextContentType` 정적 클래스:

* `Name`
* `GivenName`
* `FamilyName`
* `Location`
* `FullStreetAddress`
* `AddressCityAndState`
* `TelephoneNumber`
* `EmailAddress`

### <a name="routing-apps-and-locations-suggestions"></a>라우팅 앱 및 위치 제안

이 섹션에는 라우팅 앱 내에서 직접 위치 제안 소비를 살펴보면을 소요 됩니다. 기존 개발자는 이용 하는이 기능을 추가 하려면 라우팅 응용 프로그램에 대 한 `MKDirectionsRequest` 다음과 같이 프레임 워크:

- 멀티태스킹의 응용 프로그램을 승격 합니다.
- 라우팅 응용 프로그램으로 응용 프로그램을 등록 합니다.
- 처리는 MapKit와 앱을 시작 하기 `MKDirectionsRequest` 개체입니다.
- IOS 앱 사용자에 게 적절 한 시간에 제안 하는 방법을 배울 수 있는 기능을 부여 하려면 사용자 참여 기반으로 합니다.

MapKit와 응용 프로그램 시작 될 때 `MKDirectionsRequest` 개체, 자동으로 요청 된 위치에 사용자를 안내 시작 또는 쉽게 방향으로 시작 하는 사용자에 대 한 UI를 제공 해야 합니다. 예를 들어:


```csharp
using System;
using Foundation;
using UIKit;
using MapKit;
using CoreLocation;

namespace MonkeyChat
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate, IUISplitViewControllerDelegate
    {
        ...
        
        public override bool OpenUrl (UIApplication app, NSUrl url, NSDictionary options)
        {
            if (MKDirectionsRequest.IsDirectionsRequestUrl (url)) {
                var request = new MKDirectionsRequest (url);
                var coordinate = request.Destination?.Placemark.Location?.Coordinate;
                var address = request.Destination.Placemark.AddressDictionary;
                if (coordinate.IsValid()) {
                    var geocoder = new CLGeocoder ();
                    geocoder.GeocodeAddress (address, (place, err) => {
                        // Handle the display of the address

                    });
                }
            }

            return true;
        }
    }       
}
```

자세히이 코드를 살펴보세요. 올바른 대상 요청 인지 테스트 합니다.

```csharp
if (MKDirectionsRequest.IsDirectionsRequestUrl(url)) {
```

인 경우 생성 된 `MKDirectionsRequest` URL에서:

```csharp
var request = new MKDirectionsRequest(url);
```

새로운 10 iOS 앱 보낼 수 있습니다 주소를 인코딩하는 데 개발자가 야기 하는 지리적 좌표 없는 주소:

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="media-app-suggestions"></a>미디어 앱 제안

응용 프로그램 모든 유형의 미디어는 팟캐스트 응용 프로그램 또는 오디오 또는 비디오 10, ios 스트리밍 미디어 콘텐츠를 처리 하는 경우 시스템에서 미디어 제안 사항을 활용을 걸릴 수 있습니다.

IOS는 미디어를 처리 하는 응용 프로그램의 경우 다음과 같은 동작을 지원 합니다.

- iOS 앱의 이전 동작에 따라 사용자가을 사용할 것을 승격 합니다.
- 응용 프로그램에 관련 된 제안 스포트라이트와 오늘 보기에 표시 됩니다.
- 응용 프로그램을 충족 하는 다음 트리거 중 하나를 하는 경우 잠금 화면 제안으로 상승 될 수 있습니다.
    - 헤드폰 또는 Bluetooth 장치에 연결한 후에 연결을 만듭니다.
    - 자동차의 가져오기 후
    - 집에 도착 하거나 작업 삭제 됩니다. 

IOS 10에서에서 단순 API 호출을 포함할수록 개발자 미디어 응용 프로그램의 사용자에 대해 보다 매력적이 고 잠금 화면 환경을 만들 수 있습니다. 사용 하 여는 `MPPlayableContentManager` 미디어 재생, (예: 음악 응용 프로그램에서 제공 된 내용과) 완전 한 미디어 컨트롤을 관리 하는 클래스는 앱에 대 한 잠금 화면에서 표시 됩니다.


```csharp
using System;
using MediaPlayer;
using UIKit;

namespace MonkeyPlayer
{
    public class PlayableContentDelegate : MPPlayableContentDelegate
    {
        #region Constructors
        public PlayableContentDelegate ()
        {
        }
        #endregion

        #region Override methods
        public override void InitiatePlaybackOfContentItem (MPPlayableContentManager contentManager, Foundation.NSIndexPath indexPath, Action<Foundation.NSError> completionHandler)
        {
            // Access the media item to play
            var item = LoadMediaItem (indexPath);

            // Populate the lock screen
            PopulateNowPlayingItem (item);

            // Prep item to be played
            var status = PreparePlayback (item);

            // Call completion handler
            completionHandler (null);
        }
        #endregion

        #region Public Methods
        public MPMediaItem LoadMediaItem (Foundation.NSIndexPath indexPath)
        {
            var item = new MPMediaItem ();

            // Load item from media store
            ...

            return item;
        }

        public void PopulateNowPlayingItem (MPMediaItem item)
        {
            // Get Info Center and album art
            var infoCenter = MPNowPlayingInfoCenter.DefaultCenter;
            var albumArt = (item.Artwork == null) ? new MPMediaItemArtwork (UIImage.FromFile ("MissingAlbumArt.png")) : item.Artwork;

            // Populate Info Center
            infoCenter.NowPlaying.Title = item.Title;
            infoCenter.NowPlaying.Artist = item.Artist;
            infoCenter.NowPlaying.AlbumTitle = item.AlbumTitle;
            infoCenter.NowPlaying.PlaybackDuration = item.PlaybackDuration;
            infoCenter.NowPlaying.Artwork = albumArt;
        }

        public bool PreparePlayback (MPMediaItem item)
        {
            // Prepare media item for playback
            ...

            // Return results
            return true;
        }
        #endregion
    }
}
```

## <a name="summary"></a>요약

이 문서는 사전 제안 설명에 있으며 개발자 사용 방법으로 드라이브 트래픽을 Xamarin.iOS 앱에 설명 했습니다. 수 있습니다. 사전 제안 사항을 구현 하는 단계를 설명 하 고 사용 지침을 제공 합니다.



## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
- [SiriKit 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
