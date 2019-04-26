---
title: Xamarin.iOS에서 자동 제안 소개
description: 이 문서에서는 시스템을 사전에 사용자에 게 유용한 정보를 자동으로 제공 함으로써 자동 제안 드라이브 engagement Xamarin.iOS 앱에서 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 8DDD084A-0D1E-4DF7-B686-6309DCEFF5D3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 2ab0147f918b36dc47ef6eed7d9bf1b6295d9733
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61408201"
---
# <a name="introduction-to-proactive-suggestions-in-xamarinios"></a>Xamarin.iOS에서 자동 제안 소개

_이 문서에서는 시스템을 사전에 사용자에 게 유용한 정보를 자동으로 제공 함으로써 자동 제안 드라이브 engagement Xamarin.iOS 앱에서 사용 하는 방법을 보여 줍니다._

IOS 10, 자동 제안 있는 뉴스 방법 사용자에 게 적절 한 시간에 자동으로 사용자에 게 사전에 있는 유용한 정보에서 Xamarin.iOS 앱을 이용 하려면 새입니다.

iOS 10 시스템 사전에 존재 하는 유용한 정보를 자동으로 사용자에 게 적절 한 시간을 허용 하 여 주행 engagement 앱에는 새로운 방법을 제공 합니다. 9 iOS 처럼 추천, 전달 및 제안 Siri를 사용 하 여 앱에 심층 검색을 추가 하는 기능을 제공 (참조 [새 검색 Api](~/ios/platform/search/index.md)), iOS 10 사용 하 여 앱 내에서 시스템에서 사용자에 게 표시할 수 있는 기능을 노출할 수는 다음 위치:

- 응용 프로그램 전환기
- 잠금 화면
- CarPlay
- 맵
- Siri 상호 작용
- QuickType 제안

앱에서와 같은 기술의 모음을 사용 하 여 시스템에이 기능을 노출 `NSUserActivity`, 핵심 스포트라이트, MapKit, Media Player 및 UIKit 웹 태그입니다. 또한 앱에 대 한 사전 제안 지원을 제공 하 여 가져옵니다 밀접 하 게 Siri 통합을 무료.

## <a name="location-based-suggestions"></a>위치 기반 추천

새 iOS 10에는 `NSUserActivity` 클래스에 포함 되어는 `MapItem` 속성을 사용 하는 다른 컨텍스트에서 사용할 수 있는 위치 정보를 제공 하는 개발자. 예를 들어, 앱 음식점 리뷰를 표시 하는 경우 개발자 설정할 수는 `MapItem` 속성을 앱에서 사용자가 보고 하는 레스토랑의 위치입니다. 사용자가 Maps 앱 전환, 식당의 위치는 자동으로 가능 합니다.

새 주소 구성 요소를 사용할 수 있는 앱을 앱 검색을 지 원하는 경우는 `CSSearchableItemAttributesSet` 사용자 방문 하려고 수 있는 위치를 지정 하는 클래스입니다. 설정 하 여는 `MapItem` 속성을 다른 속성은에 자동으로 채워집니다.

설정 외에도 합니다 `Latitude` 및 `Longitude` 주소 구성 요소 속성의 것이 좋습니다는 앱 제공 합니다 `NamedLocation` 및 `PhoneNumbers` 속성도 따라서 Siri를 시작할 수 있습니다 위치에 대 한 호출 합니다.

## <a name="web-markup-based-suggestions"></a>웹 태그 기반 추천

iOS 9 사용자 추천 및 Safari 검색 결과에 표시 되는 콘텐츠를 보강 하는 웹 사이트의 구조화 된 데이터 태그를 포함 하는 기능에 추가 (참조 [웹 태그를 사용 하 여 검색](~/ios/platform/search/web-markup.md)). 위치 기반 태그를 포함 하는 기능을 추가 하는 iOS 10 (같은 [마쳤습니다](http://schema.org/PostalAddress) 정의 된 대로 [Schema.org](http://schema.org/))을 더욱 사용자 환경을 향상 시킵니다. 예를 들어, 웹 사이트에서 페이지를 표시 하는 위치는 사용자가 뷰 맵을 열 때 시스템 동일한 위치를 제안할 수 있습니다.

## <a name="text-based-suggestions"></a>텍스트 기반 추천

UIKit ios 10 포함 하도록 확장 되었습니다를 [TextContentType](https://developer.apple.com/reference/uikit/uitextinputtraits/1649656-textcontenttype) 의 속성을 [UITextInputTraits](https://developer.apple.com/reference/uikit/uitextinputtraits) 텍스트 영역에서 내용의 의미 체계 의미를 지정 하는 클래스입니다. 이 정보를 사용 하 여 시스템 수 일반적으로 자동으로 해당 키보드 종류를 선택, 자동 고침 제안을 향상 시킵니다 및 사전에 다른 앱 및 웹 사이트에서 정보를 통합 합니다.

예를 들어, 사용자가 표시 된 텍스트 필드에 텍스트를 입력 하는 경우 `UITextContentType.FullStreetAddress`, 시스템에서 자동 채우기를 필드는 사용자가 최근에 보기 위치를 사용 하 여를 제안할 수 있습니다.

## <a name="media-based-suggestions"></a>미디어 기반 추천

앱을 사용 하 여 미디어를 재생 하는 경우는 [MPPlayableContentManager](xref:MediaPlayer.MPPlayableContentManager) API iOS 10에는 사용자가 앨범 표지의 보기 및 잠금 화면에서 앱을 통해 미디어를 재생할 수 있습니다.

## <a name="contextual-siri-reminders"></a>상황에 맞는 Siri 미리 알림

Siri를 사용 하 여 나중에 앱에서 현재 표시 되어 콘텐츠를 보려면 미리 알림을 신속 하 게 사용할 수 있도록 할 수 있습니다. 예를 들어 앱에서 음식점 리뷰를 보고 있던 것을 하는 경우 없습니다 Siri를 호출 하며 예를 들어 *"생각나이 대 한 홈 받으면."* Siri는 앱에서 검토에 대 한 링크가 포함 된 미리 알림을 생성 합니다.

## <a name="contact-based-suggestions"></a>연락처 기반 추천

앱의 허용 연락처와 연락처 관련된 정보에 표시 된 **에 문의** 시스템에 저장 된 모든 사용자가 대화 상대와 함께 iOS 장치에서 앱.

## <a name="ride-sharing-based-suggestions"></a>공유 승객 기반 추천

공유 앱을 사용 하는 경우는 [MKDirectionsRequest](xref:MapKit.MKDirectionsRequest) API iOS 10 됩니다 표시 앱 전환기에 옵션으로 사용자는 승객 가능성이 높은 경우에 가끔 있습니다. 앱을 등록 해야 합니다도 공유 앱으로 지정 하 여는 `MKDirectionsModeRideShare` 에 대 한는 [MKDirectionsApplicationSupportedModes](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html) 키에 해당 `Info.plist` 파일.

앱만을 지 원하는 경우 승객 공유 시스템 제안 시작 *"에 승객 Get..."*, 시스템에서 사용 됩니다 (예: Walking 또는 자전거) 라우팅 방향 다른 유형의 지원 되 면 *"지침에 Get..."*

> [!IMPORTANT]
> 합니다 [MKMapItem](xref:MapKit.MKMapItem) 앱을 받는 개체 위도 및 경도 정보를 포함 될 수 있습니다 및 지 오 코딩 해야 합니다.

## <a name="implementing-proactive-suggestions"></a>자동 제안 구현

추가 사전 제안 Xamarin.iOS 앱 지원은 일반적으로 몇 가지 Api를 구현 하거나 앱에서 이미 구현할 수 있는 몇 가지 Api를 확장 하는 것 만큼 쉽습니다.

자동 제안 세 가지 주요 방법으로 앱을 사용 하 여 작동 합니다.

- **`NSUserActivity` 및 Schema.org**  -  `NSUserActivity` 화면에서 사용 하 여 사용자가 현재 작업 정보를 이해 하는 시스템 수 있습니다. Schema.org 웹 페이지에 유사한 기능을 추가합니다.
- **위치 제안을** -앱을 제공 하거나 앱에서이 정보를 공유 하는 데 위치 기반 정보를 이러한 API 확장 제품의 새로운 방법을 사용 하는 경우.
- **미디어 앱 제안을** -시스템 응용 프로그램을 승격할 수 및 해당 미디어 콘텐츠 컨텍스트를 기반으로 iOS 장치를 사용 하 여 사용자의 상호 작용 합니다.

다음을 구현 하 여 앱에서 지원 됩니다.

- **핸드 오프**  -  `NSUserActivity` iOS 8 사용 하면 한 장치에서 작업을 시작 하 고 다른 계속 개발자는 핸드 오프를 지원 하기 위해 추가 되었습니다 (참조 [핸드 오프 소개](~/ios/platform/handoff.md)).
- **스포트라이트 검색** -iOS 9를 사용 하 여 Spotlight 검색 결과 내에서 앱 콘텐츠를 홍보 하는 기능 추가 `NSUserActivity` (참조 [핵심 스포트라이트를 사용 하 여 검색](~/ios/platform/search/corespotlight.md)).
- **상황에 맞는 Siri 미리** -iOS 10에서에서 `NSUserActivity` 앱에서 현재 사용자가 보고 나중에 콘텐츠를 보려면 미리 알림을 신속 하 게 사용할 수 있도록 Siri 허용 하도록 확장 되었습니다.
- **위치 제안을** -iOS 10을 향상 시킵니다 `NSUserActivity` 앱 내에서 표시 하는 위치를 캡처하고 시스템 전체에서 여러 위치에서 승격 해야 합니다.
- **상황에 맞는 Siri 요청**  -  `NSUserActivity` 컨텍스트를 받을 수 있도록 지침 또는 호출 되어야 하는 위치 앱 내에서 호출 Siri Siri를 앱 내에서 제공 되는 정보를 제공 합니다.
- **상호 작용에 문의** iOS 10의에서 새로운 기능- `NSUserActivity` 통신 앱 대체 통신 방법으로 (연락처 앱)에 대화 상대 카드에서 승격 될 수 있도록 허용 합니다.

이러한 기능을 모두 한 가지 공통가 모두 사용 하 여 `NSUserActivity` 하나의 형태로 든 해당 기능을 제공 합니다. 

## <a name="nsuseractivity"></a>NSUserActivity

위에서 설명한 것 처럼 `NSUserActivity` 화면에서 사용 하 여 사용자가 현재 작업 정보를 이해 하는 시스템 수 있습니다. `NSUserActivity` 간단한 상태가 앱을 통해 탐색 하는 동안 사용자의 활동을 캡처하는 메커니즘을 캐시 합니다. 예를 들어, 앱을 식당을 살펴보면 다음과 같습니다.

[![](proactive-suggestions-images/activity02.png "캐싱 메커니즘 NSUserActivity 간단한 상태")](proactive-suggestions-images/activity02.png#lightbox)

다음 상호:

1. 앱을 사용 하 여 작동 하므로 사용자는 `NSUserActivity` 나중에 앱의 상태를 다시 생성 됩니다.
2. 사용자를 식당에 대 한 검색 작업을 만드는 동일한 패턴 수행 됩니다.
3. 및 사용자는 결과 볼 때 다시 합니다. 이 마지막 경우 사용자가 보는 위치 이며 ios 10에서 시스템 (예: 위치 또는 통신 상호 작용) 특정 개념에 대해 알고 합니다.

마지막 화면에서 좀 더 자세히 살펴보겠습니다.

[![](proactive-suggestions-images/activity03.png "NSUserActivity 세부 정보")](proactive-suggestions-images/activity03.png#lightbox)

앱을 만드는 여기는 `NSUserActivity` 상태를 나중에 다시 정보로 채워진 및 합니다. 앱이 해당 위치의 이름 및 주소와 같은 일부 메타 데이터도 포함 됩니다. 만든이 동작을 사용 하 여 앱에는 iOS를 사용자의 현재 상태를 나타내는 것을 알 수 있습니다.

앱에는 경우 활동 핸드 오프에 대 한 무선 보급, 임시 위치 제안 값으로 저장 또는 추가 될 검색 결과에 표시 하기 위한 장치 Spotlight 인덱스를 결정 합니다.

핸드 오프 및 스포트라이트 검색에 대 한 자세한 내용은 참조 하십시오 우리의 [핸드 오프 소개](~/ios/platform/handoff.md) 하 고 [iOS 9 새 Search Api](~/ios/platform/search/index.md) 가이드.

### <a name="creating-an-activity"></a>활동 만들기

활동을 만들기 전에 활동 형식 식별자는 식별 하기 위해 필요 합니다. 활동 형식 식별자가 짧은 문자열에 추가 합니다 `NSUserActivityTypes` 앱의 배열을 `Info.plist` 지정된 된 사용자 활동 형식에 고유 하 게 식별 하는 데 사용 되는 파일입니다. 앱을 지원 하 고 앱 검색에 노출 하는 각 작업에 대 한 배열에 하나의 항목이 됩니다. 참조 우리의 [활동 유형 식별자 참조를 만드는](~/ios/platform/search/nsuseractivity.md) 대 한 자세한 내용은 합니다.

작업의 예제를 살펴보겠습니다.

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

새 활동을 활동 형식 식별자를 사용 하 여 생성 됩니다. 그런 다음 나중에이 상태를 복원할 수 있도록 활동을 정의 하는 일부 메타 데이터 생성 됩니다. 그런 다음 활동 이며 의미 있는 제목이 지정 된 사용자 정보에 연결 합니다. 마지막으로 일부 기능이 활성화 됩니다 하 고 작업 시스템에 전송 됩니다.

위의 코드를 다음과 같이 변경 하 여 작업 컨텍스트를 제공 하는 메타 데이터를 포함 하도록 추가로 개선할 수 있습니다.

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

개발자는 앱과 동일한 정보를 표시할 수 있는 웹 사이트에 앱 URL을 포함할 수 있습니다 하 고 (전달)를 통해 설치 된 앱을 갖지 않는 다른 장치에서 콘텐츠를 표시할 수 있습니다.

```csharp
// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");
```

### <a name="restoring-an-activity"></a>복원 작업

검색 결과 탭 하거나 사용자에 게 응답할 (`NSUserActivity`) 앱을 편집 합니다 **AppDelegate.cs** 파일을 재정의 `ContinueUserActivity` 메서드. 예를 들어:

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

개발자의 동일한 작업 형식 식별자 인지 확인 해야 합니다 (`com.xamarin.platform`) 위에서 만든 활동으로 합니다. 앱에 저장 된 정보를 사용 하는 `NSUserActivity` 다시 사용자 중단으로 상태를 복원 합니다.

### <a name="benefits-of-creating-an-activity"></a>활동을 만들 경우의 이점

최소한 위에 제시 된 코드를 사용 하 여 앱이 이제 세 가지 새 iOS 10 기능을 활용 하려면 수 있습니다:

- **Handoff**
- **스포트라이트 검색**
- **상황에 맞는 Siri 미리 알림**

다음 섹션은 살펴보겠습니다 다른 두 가지 새로운 iOS 10 기능을 사용 하도록 설정:

- **위치 제안**
- **상황에 맞는 Siri 요청**

### <a name="location-based-suggestions"></a>위치 기반 추천 

위의 식당 검색 앱을 예로 들어보겠습니다. 가 구현 하는 경우 `NSUserActivity` 고 올바르게 채워집니다 모든 메타 데이터 및 특성, 사용자는 다음 작업을 수행할 수 있습니다.

1. 친구에 게 충족 하 고 싶은 앱의 식당을 찾습니다.
2. 멀티태스킹 앱 전환기를 사용 하 여 앱에서 사용자가 이동, 시스템 자동으로 표시 됩니다 제안 (화면 맨 위에 있음) 탐색을 즐겨 찾는 앱을 사용 하 여 식당에는 지침을 가져오려고 합니다.
3. 사용자 메시지 앱으로 전환 하 고 입력을 시작 하는 경우 *"언제 만나서"*, QuickType 키보드 식당의 주소에 붙여 넣어 자동 제안 됩니다.
4. 맵 앱으로 전환 하는 사용자, 경우 식당의 주소를 대상으로 자동으로 제안 됩니다.
5. 이 타사 앱에도 적합 (지 `NSUserActivity`) 식당의 주소는 자동으로 제안 있습니다 대상으로도 하 고 사용자는 공유 앱으로 전환할 수 있도록 합니다.
6. 또한 컨텍스트를 제공 Siri 사용자는 식당 앱 내에서 Siri를 호출 하 고 요청할 수 있도록 *"Get directions..."* Siri를 사용자가 보고 식당에는 지침을 제공 합니다.

위의 기능을 모두 한 가지 공통에 제안을에서 처음 제공 되는 위치를 나타내는 모두 합니다. 위 예의 경우이 가상의 식당 검토 앱입니다.

iOS 10 몇 가지 작은 수정 및 기존 프레임 워크에 대 한 추가 통해 앱에 대 한이 기능을 사용 하도록 향상 되었습니다.

- `NSUserActivity` 앱 내에서 표시 되는 위치 정보를 캡처에 대 한 추가 필드에 있습니다.
- 위치를 캡처하려면 몇 가지 추가 MapKit를 CoreSpotlight 이루어졌습니다.
- 위치 인식 기능 Siri, 맵, 키보드, 멀티태스킹 및 시스템 내에서 다른 앱에 추가 되었습니다.

위치 기반 추천 단어를 구현 하려면 위에서 설명한 동일한 작업 코드를 사용 하 여 시작 합니다.

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

앱 MapKit를 사용 하는 경우 현재 map를 추가 하기만 `MKMapItem` 활동:

```csharp
// Save MKMapItem location
activity.MapItem = myMapItem;
```

앱 MapKit 사용 되지 않습니다 앱 검색을 채택 하 위치에 대 한 다음 새 특성을 지정 합니다.

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

세부 정보에서 위의 코드를 살펴보세요. 먼저 모든 인스턴스에 위치의 이름이 필요 합니다.

```csharp
attributes.NamedLocation = "Apple Inc.";
```

그런 다음 텍스트 기반에 대 한 설명은 위치는 텍스트 기반 인스턴스 (예: QuickType 키보드)에 대 한 필요 합니다.

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

위도 및 경도 선택적 이지만 사용자 포함 해야 하므로 앱으로 보낼 하려는 정확한 위치로 라우팅되는지 확인 합니다.

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

전화 번호를 설정 하 여 앱에 액세스할 수 있습니다 Siri 사용자와 같은 것을 알리는 하 여 앱에서 Siri를 호출할 수 있도록 *"이이 위치가 호출"*:

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

마지막으로 앱 인스턴스를 탐색 하 고 전화 통화 하는 데 적합 한지를 나타낼 수 있습니다.

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

### <a name="implementing-contact-interactions"></a>연락처의 상호 작용을 구현합니다.

새 ios 10에서 통신 앱 대화 상대 카드에서 연락처 앱에 통합 긴밀 하 게 됩니다. 연락처 상호 작용 구현 된 앱에 대 한 사용자 연락처에서 특정 사용자에 지정 된 앱의 정보를 추가할 수 있습니다. 와 연결 된 앱에서 메시지를 송신 하기 위한 옵션으로 나열 됩니다, 예를 들어, 이러한 적중 하는 경우 메시지를 보낼 카드의 맨 위에 있는 빠른 작업 단추입니다.

타사 타사 앱을 선택 하면 기억 하 고 지정 된 사용자에 게 연락 하는 사용자가 다음에 메시지에 기본 방법으로 표시 될 수 있습니다.

연락처 상호 작용을 사용 하 여 앱에 구현 되어 `NSUserActivity` 및 iOS 10에에서 도입 된 새로운 인 텐트 프레임 워크입니다. 의도 사용 하 여 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [SiriKit 개념 이해](~/ios/platform/sirikit/understanding-sirikit.md) 하 고 [SiriKit 구현](~/ios/platform/sirikit/implementing-sirikit.md) 가이드입니다.

#### <a name="donating-interactions"></a>기증 상호 작용

앱 상호 작용을 지정할 수 있습니다 하는 방법에 대해 살펴보겠습니다.

[![](proactive-suggestions-images/activity04.png "기증 상호 작용 개요")](proactive-suggestions-images/activity04.png#lightbox)

앱은는 `INInteraction` 포함 된 개체를 **의도** (`INIntent`), **참가자** 하 고 **메타 데이터**. 합니다 **의도** 비디오 호출 또는 텍스트 메시지와 같은 사용자 동작을 나타냅니다. 합니다 **참가자** 통신을 수신 하는 사람들을 포함 합니다. 합니다 **메타 데이터** 성공적으로 등 메시지를 보내는 등의 추가 정보를 정의 합니다.

개발자 되지 직접의 인스턴스를 만듭니다 `INIntent` 또는 `INIntentResponse`합니다 (앱 사용자를 대신해 서 수행 되는 작업에 따라) 특정 자식 클래스 중 하나를 사용 이러한 부모 클래스에서 상속 되는 합니다. 예를 들어 `INSendMessageIntent` 고 `INSendMessageIntentResponse` 텍스트 메시지 전송에 대 한 합니다. 

상호 작용을 완전히 채웠으므로 호출을 `DonateInteraction` 메서드를 상호 작용을 사용 하 여 사용할 수 있는 시스템을 알려줍니다.

상호 작용 가져옵니다 함께 대화 상대 카드에서 앱을 사용 하 여 사용자 상호 작용을 하는 경우는 `NSUserActivity`는 다음 응용 프로그램을 실행 하는 데 사용 됩니다.

[![](proactive-suggestions-images/activity05.png "상호 작용 가져옵니다 응용 프로그램을 실행 하는 데 사용 되는 NSUserActivity 함께 제공 됩니다.")](proactive-suggestions-images/activity05.png#lightbox)

메시지 전송 여부는 다음 예제를 살펴보십시오.

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

세부 정보에서이 코드를 살펴보면 만들고의 인스턴스를 채우는 `NSUserActivity` (에서처럼 합니다 [활동 만들기](#creating-an-activity) 위의 섹션). 인스턴스를 만들고이 어 `INSendMessageIntent` (에서 상속 하는 `INIntent`) 송신할 메시지의 세부 정보를 사용 하 여 채웁니다.

```csharp
var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);
```

`INSendMessageIntentResponse` 전달 된 `NSUserActivity` 위에서 만든:

```csharp
var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);
```

`INInteraction` 보낼 메시지 의도 로부터 빌드됩니다 (`INSendMessageIntent`) 및 응답 (`INSendMessageIntentResponse`) 방금 만든:

```csharp
var interaction = new INInteraction (intent, response);
```

마지막으로, 시스템의 상호 작용의 알림:

```csharp
// Donate interaction to the system
interaction.DonateInteraction ((err) => {
    // Handle donation error
    ...
});
```

완료 처리기는 앱에 성공 하거나 실패 한 기부 응답할 수 있는 위치에 전달 됩니다.

### <a name="activities-best-practices"></a>활동에 대 한 유용한 정보

Apple 활동을 사용 하 여 작업할 때 다음 모범 사례를 제안 합니다.

- 사용 하 여 `NeedsSave` 지연 페이로드 업데이트 합니다.
- 현재 활동에 대 한 강력한 참조를 유지 해야 합니다.
- 만 상태를 복원 하려면 충분 한 정보를 포함 하는 작은 페이로드를 전송 합니다.
- 활동 형식 식별자가 고유 하 고 설명이 포함 된 역방향 DNS 표기법을 사용 하 여이 지정 하 여 확인 합니다. 

## <a name="schemaorg"></a>Schema.org

위와 같이 `NSUserActivity` 화면에서 사용 하 여 사용자가 현재 작업 정보를 이해 하는 시스템을 사용 하면 됩니다. Schema.org 웹 페이지에 유사한 기능을 추가합니다.

Schema.org 같은 종류의 웹 사이트에 위치 기반 상호 작용을 제공할 수 있습니다. Apple에 새 위치 제안을 네이티브 앱 에서도 Safari에서 볼 때와 마찬가지로 작동 하도록 설계 되었습니다.

일부 Schema.org 배경:

- 공개 웹 태그 어휘 표준을 제공합니다.
- 웹 페이지에 구조적된 메타 데이터를 포함 하 여 작동 합니다.
- 사용 가능한 다양 한 개념을 나타내는 500 개 이상의 스키마가 있습니다.
- 웹 사이트에서 구현 하 여 개발자 얻을 수 있습니다 사용 하는 이점의 일부 `NSUserActivity` 네이티브 앱에서는 합니다.

스키마 트리 구조, 관련과 같은 형식이 같은 정렬 됩니다 *식당*와 같은 보다 일반적인 형식에서 상속할 *로컬 비즈니스*합니다. 자세한 내용은 참조 하십시오 [Schema.org](http://schema.org)합니다.

예를 들어, 웹 페이지에는 다음 데이터를 포함 하는 경우:

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

사용자 Safari에서이 페이지를 방문 하 고 다른 앱으로 전환 하는 경우 페이지에서 위치 정보를 캡처할 고 (처럼 위의 활동) 시스템의 다른 부분에 위치 제안으로 제공 됩니다.

Safari는 다음과 같은 스키마 속성을 준수 하는 웹 페이지에서 아무 것도 추출 됩니다.

- **PostalAddress**
- **GeoCoordinates**
- 전화 속성입니다.

자세한 내용은 참조 하십시오 우리의 [웹 태그를 사용 하 여 검색](~/ios/platform/search/web-markup.md) 가이드입니다.

## <a name="consuming-location-suggestions"></a>소비 위치 제안

이 다음 섹션에서는 사용 위치 제안 (예: 맵 앱) 시스템 또는 다른 타사 앱의 다른 부분에서을 설명 합니다.

두 가지 주요 앱 위치 제안을 사용할 수 있습니다.

- QuickType 키보드의 경우.
- 직접 라우팅 앱에서 위치 정보를 사용에서 합니다.

### <a name="location-suggestions-and-the-quicktype-keyboard"></a>위치 제안과 QuickType 키보드

앱을 기반으로 하는 텍스트 형식으로 주소를 사용 하 여 처리 하는 경우 앱 QuickType UI를 통해 위치 제안을 활용을 걸릴 수 있습니다. iOS 10에는 다음과 같은 기능을 사용 하 여 QuickType UI를 확장합니다.

- 앱 UI에서 텍스트 필드에 대 한 의미 체계 의도 대 한 힌트를 추가할 수 있습니다.
- 앱은 앱에서 자동 제안을 가져올 수 있습니다.
- 앱에서 향상 된 자동 고침 이점을 얻을 수 있습니다.

새 `TextContentType` iOS 10에서에서 텍스트 필드 컨트롤의 속성에는 개발자가 하려는 사용자 지정된 필드에 입력 값에 대 한 의미 체계 의도 정의할 수 있습니다. 예를 들어:

```csharp
var textField = new UITextField();
textField.TextContentType = UITextContentType.FullStreetAddress;
```

앱에서는 사용자 지정된 필드에 전체 주소를 입력 하는 시스템이 설명 합니다. 이렇게 하면 QuickType 키보드를 자동으로 사용자가이 필드에 값을 입력 하는 경우 바로 가기 위치 제안을 제공 합니다.

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

이 섹션에는 라우팅 앱 내에서 직접 위치 제안을 사용 살펴보겠습니다를 걸립니다. 이 기능을 추가 하려면 라우팅 앱에 대 한 개발자가 활용 하 여 기존 `MKDirectionsRequest` 같이 프레임 워크:

- 멀티태스킹에서 앱을 승격 합니다.
- 라우팅 앱으로 앱을 등록 합니다.
- MapKit 사용 하 여 앱을 시작을 처리 하도록 `MKDirectionsRequest` 개체입니다.
- IOS 앱 사용자에 게 적절 한 시간에서 제안 하는 방법을 배울 수 있도록 사용자 참여 기반으로 합니다.

MapKit를 사용 하 여 앱이 시작 하는 경우 `MKDirectionsRequest` 개체를 자동으로 시작 요청 된 위치에 사용자 지침을 제공 하거나 쉽게 지침을 시작 하려면 사용자에 대 한 UI를 제공 해야 합니다. 예를 들어:


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

세부 정보에서이 코드를 살펴보세요. 테스트 하 여 유효한 대상 요청 인지 확인 합니다.

```csharp
if (MKDirectionsRequest.IsDirectionsRequestUrl(url)) {
```

인 경우 생성 된 `MKDirectionsRequest` URL에서:

```csharp
var request = new MKDirectionsRequest(url);
```

새 ios 10에서 앱을 보낼 수 있습니다이 없는 지역 좌표를 야기 하는 개발자의 주소를 인코드 해야 하는 주소:

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="media-app-suggestions"></a>미디어 앱 제안

앱을 모든 유형의 미디어 팟캐스트 앱 또는 오디오 또는 비디오를 iOS 10 사용 하 여 스트리밍 미디어 콘텐츠 등을 처리 하는 경우 시스템에서 미디어 제안을 활용을 걸릴 수 있습니다.

미디어를 처리 하는 앱에 대 한 iOS에서는 다음 동작을 지원 합니다.

- iOS의 이전 동작에 따라 사용자가 사용 하는 앱을 승격 합니다.
- 앱에 관련 된 제안 추천에서 오늘 보기에 표시 됩니다.
- 앱을 충족 하는 다음 트리거 중 하나를 하는 경우 잠금 화면 제안으로 상승 될 수 있습니다.
    - 헤드폰 또는 Bluetooth 장치에 연결한 후 연결을 만듭니다.
    - 자동차의 가져오기 후
    - 집에 도착 하거나 작업 후 

IOS 10에서에서 간단한 API 호출 등에서 개발자는 미디어 앱의 사용자에 대 한 보다 매력적이 고 잠금 화면 환경을 만들 수 있습니다. 사용 하 여는 `MPPlayableContentManager` 미디어 재생, 완전 한 미디어 컨트롤 (예: 음악 앱에서 제시 된 것)를 관리 하는 클래스는 앱에 대 한 잠금 화면에 표시 됩니다.


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

이 문서에서는 자동 제안 설명 있으며 개발자를 사용 하는 이러한 Xamarin.iOS 앱으로 트래픽을 유도 하기에 대해 설명 했습니다. 자동 제안을 구현 하는 단계를 설명 하 고 사용 지침을 제공 합니다.



## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
- [SiriKit 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
