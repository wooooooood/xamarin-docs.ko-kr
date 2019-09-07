---
title: Xamarin.ios의 사전 권장 사항 소개
description: 이 문서에서는 사용자에 게 유용한 정보를 자동으로 제공할 수 있도록 Xamarin.ios 앱에서 자동 관리 제안을 사용 하 여 참여를 유도 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 8DDD084A-0D1E-4DF7-B686-6309DCEFF5D3
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: b79f64f154dbd7dde623d13385f111d3d5a5d3f2
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769545"
---
# <a name="introduction-to-proactive-suggestions-in-xamarinios"></a>Xamarin.ios의 사전 권장 사항 소개

_이 문서에서는 사용자에 게 유용한 정보를 자동으로 제공할 수 있도록 Xamarin.ios 앱에서 자동 관리 제안을 사용 하 여 참여를 유도 하는 방법을 보여 줍니다._

IOS 10을 처음 사용 하는 경우 자동으로 사용자에 게 유용한 정보를 사용자에 게 자동으로 제공 하 여 사용자가 Xamarin.ios 앱에 참여할 수 있는 새로운 방법을 제공 합니다.

iOS 10은 시스템이 적절 한 시간에 자동으로 유용한 정보를 사용자에 게 자동으로 제공할 수 있도록 하 여 앱에 대 한 참여를 유도 하는 새로운 방법을 제공 합니다. IOS 9에서 스포트라이트, 전달 및 Siri 제안을 사용 하 여 앱에 심층 검색을 추가 하는 기능을 제공 하는 경우 ( [새 검색 api](~/ios/platform/search/index.md)참조) ios 10을 사용 하 여 앱은 다음 위치에서 시스템을 통해 사용자에 게 제공할 수 있는 기능을 노출할 수 있습니다. :

- 앱 전환기
- 잠금 화면
- CarPlay
- 지도
- Siri 상호 작용
- QuickType 제안

앱은, 웹 태그, 핵심 스포트라이트, mapkit, Media Player 및 uikit와 `NSUserActivity`같은 기술의 컬렉션을 사용 하 여 시스템에이 기능을 노출 합니다. 또한 앱에 대 한 사전 제안 지원을 제공 하 여 더 심층적인 Siri 통합을 무료로 제공 합니다.

## <a name="location-based-suggestions"></a>위치 기반 제안

IOS 10의 새로운 기능으로 `NSUserActivity` , 클래스에 `MapItem` 는 개발자가 다른 컨텍스트에서 사용할 수 있는 위치 정보를 제공할 수 있는 속성이 포함 되어 있습니다. 예를 들어 앱에서 식당 리뷰를 표시 하는 경우 개발자는 사용자 `MapItem` 가 앱에서 보고 있는 식당의 위치로 속성을 설정할 수 있습니다. 사용자가 Maps 앱으로 전환 하는 경우 식당의 위치를 자동으로 사용할 수 있습니다.

앱이 앱 검색을 지 원하는 경우 `CSSearchableItemAttributesSet` 클래스의 새 주소 구성 요소를 사용 하 여 사용자가 방문할 수 있는 위치를 지정할 수 있습니다. `MapItem` 속성을 설정 하면 다른 속성이 자동으로 채워집니다.

주소 구성 요소 속성의 `Latitude` 및 `Longitude` 를 설정 하는 것 외에도 앱에서 `NamedLocation` 및 `PhoneNumbers` 속성을 제공 하는 것이 좋습니다. 그러면 siri가 해당 위치에 대 한 호출을 시작할 수 있습니다.

## <a name="web-markup-based-suggestions"></a>웹 태그 기반 제안

iOS 9는 사용자에 게 스포트라이트 및 Safari 검색 결과에 표시 되는 콘텐츠를 강화 웹 사이트에 구조화 된 데이터 태그를 포함 하는 기능을 추가 했습니다 ( [웹 마크업으로 검색](~/ios/platform/search/web-markup.md)참조). iOS 10은 사용자 환경을 더욱 향상 시키기 위해 위치 기반 태그 (예: [Schema.org](http://schema.org/)에 정의 된 [PostalAddress](http://schema.org/PostalAddress) )를 포함 하는 기능을 추가 합니다. 예를 들어 사용자가 웹 사이트에서 페이지가 표시 된 위치를 보는 경우 맵을 열 때 시스템에서 동일한 위치를 제안할 수 있습니다.

## <a name="text-based-suggestions"></a>텍스트 기반 제안

UIKit는 [Uitextinputtraits](https://developer.apple.com/reference/uikit/uitextinputtraits) 클래스의 [textcontenttype](https://developer.apple.com/reference/uikit/uitextinputtraits/1649656-textcontenttype) 속성을 포함 하 여 텍스트 영역의 내용에 대 한 의미 체계를 지정 하기 위해 iOS 10에서 확장 되었습니다. 이 정보를 사용 하는 경우 시스템은 일반적으로 적절 한 키보드 유형을 자동으로 선택 하 고, 자동 고침 제안을 향상 시키고, 다른 앱 및 웹 사이트의 정보를 사전에 통합할 수 있습니다.

예를 들어 사용자가 표시 `UITextContentType.FullStreetAddress`된 텍스트 필드에 텍스트를 입력 하는 경우 시스템은 사용자가 최근에 보고 있던 위치로 필드를 자동으로 채우는 것을 제안할 수 있습니다.

## <a name="media-based-suggestions"></a>미디어 기반 제안

앱이 [MPPlayableContentManager](xref:MediaPlayer.MPPlayableContentManager) API를 사용 하 여 미디어를 재생 하는 경우 iOS 10에서는 사용자가 잠금 화면에서 앱을 통해 앨범 아트를 보고 미디어를 재생할 수 있습니다.

## <a name="contextual-siri-reminders"></a>상황별 Siri 미리 알림

사용자가 Siri를 사용 하 여 나중에 앱에서 현재 보고 있는 콘텐츠를 확인 하는 미리 알림을 신속 하 게 만들 수 있습니다. 예를 들어 앱에서 식당 리뷰를 보고 있는 경우 Siri를 호출 하 고 *"집에서이를 받을 때이에 대해 알림"* 이라고 말할 수 있습니다. Siri는 앱의 검토에 대 한 링크가 포함 된 미리 알림을 생성 합니다.

## <a name="contact-based-suggestions"></a>연락처 기반 제안

앱의 연락처 (및 관련 정보)가 시스템에 저장 된 모든 사용자 연락처와 함께 iOS 장치의 **연락처** 앱에 표시 되도록 허용 합니다.

## <a name="ride-sharing-based-suggestions"></a>공유 기반 제안 사항

[MKDirectionsRequest](xref:MapKit.MKDirectionsRequest) API를 사용 하는 사용자가 공유 하는 앱이 있는 경우, iOS 10은 사용자가 원하는 경우에 앱 전환기에서 옵션으로 표시 합니다. 또한 해당 `MKDirectionsModeRideShare` `Info.plist` 파일에 [MKDirectionsApplicationSupportedModes](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html) 키에 대 한를 지정 하 여 해당 앱을 타는 공유 앱으로 등록 해야 합니다.

앱에서 타는 공유만 지 원하는 경우에는 시스템 제안이 "연락 *하기*..."로 시작 하 고, 다른 유형의 라우팅 방향 (예: 움직이는 또는 자전거)이 지원 되는 경우 시스템은 *"지침 가져오기 ..."* 를 사용 합니다.

> [!IMPORTANT]
> 앱이 받는 [MKMapItem](xref:MapKit.MKMapItem) 개체는 경도 및 위도 정보를 포함 하지 않을 수 있으며 지 오 코딩가 필요 합니다.

## <a name="implementing-proactive-suggestions"></a>자동 관리 제안 구현

Xamarin.ios 앱에 사전 제안 지원을 추가 하는 것은 일반적으로 몇 가지 Api를 구현 하거나 앱이 이미 구현할 수 있는 몇 가지 Api를 확장 하는 것 만큼 쉽습니다.

사전 권장 사항은 다음과 같은 세 가지 주요 방법으로 앱과 함께 작동 합니다.

- **`NSUserActivity`및**  -  Schema.org`NSUserActivity` 는 시스템에서 사용자가 현재 화면에서 작업 하 고 있는 정보를 이해 하는 데 도움이 됩니다. Schema.org는 웹 페이지에 비슷한 기능을 추가 합니다.
- **위치 제안** -앱에서 위치 기반 정보를 제공 하거나 사용 하는 경우 이러한 API 확장을 통해 앱 간에이 정보를 공유할 수 있는 새로운 방법을 제공 합니다.
- **미디어 앱 제안** -시스템은 iOS 장치와 사용자의 상호 작용 컨텍스트를 기준으로 앱 및 미디어 콘텐츠를 승격할 수 있습니다.

및은 다음을 구현 하 여 앱에서 지원 됩니다.

- **전달** - `NSUserActivity`이 지원되도록 iOS 8에 추가되었습니다. 이를 통해 개발자는 한 장치에서 작업을 시작하고 다른 장치에서 작업을 계속할 수 있습니다([핸드오프 전달 소개](~/ios/platform/handoff.md) 참조).
- **스포트라이트 검색** -iOS 9는를 사용 하 여 `NSUserActivity` 스포트라이트 검색 결과 내에서 앱 콘텐츠를 승격 하는 기능을 추가 했습니다 ( [Core 스포트라이트를 사용 하 여 검색](~/ios/platform/search/corespotlight.md)참조).
- IOS 10 `NSUserActivity` 의 **상황별 siri 미리 알림** -siri가 나중에 앱에서 현재 보고 있는 콘텐츠를 확인 하는 미리 알림을 신속 하 게 만들 수 있도록 확장 되었습니다.
- **위치 제안** -iOS 10은 `NSUserActivity` 앱 내에서 볼 수 있는 캡처 위치를 개선 하 고 시스템 전체의 여러 위치에서 승격 합니다.
- **컨텍스트 siri 요청**  -  `NSUserActivity` 은 응용 프로그램 내에서 siri에 게 제공 되는 정보에 대 한 컨텍스트를 제공 하므로 사용자는 지침을 얻거나 앱 내에서 siri를 호출 하는 호출을 수행할 수 있습니다.
- **연락처 상호 작용** -iOS 10의 새로운 `NSUserActivity` 기능으로, 통신 앱은 연락처 카드 (연락처 앱)에서 대체 통신 방법으로 승격할 수 있습니다.

이러한 모든 기능은 공통적으로 하나를 사용 `NSUserActivity` 하 여 해당 기능을 제공 합니다. 

## <a name="nsuseractivity"></a>NSUserActivity

위에서 설명한 것 처럼 `NSUserActivity` 시스템에서 사용자가 현재 화면에서 작업 하 고 있는 정보를 이해 하는 데 도움을 줍니다. `NSUserActivity`는 앱을 탐색할 때 사용자의 작업을 캡처하는 경량 상태 캐싱 메커니즘입니다. 예를 들어 식당 앱을 살펴보면 다음과 같습니다.

[![](proactive-suggestions-images/activity02.png "NSUserActivity 경량 상태 캐싱 메커니즘")](proactive-suggestions-images/activity02.png#lightbox)

다음과 같은 상호 작용을 사용 합니다.

1. 사용자가 앱 `NSUserActivity` 에서 작업할 때 나중에 앱의 상태를 다시 만들기 위해가 만들어집니다.
2. 사용자가 식당을 검색 하는 경우 동일한 유형의 활동을 만듭니다.
3. 그리고 사용자가 결과를 볼 때를 다시 사용 합니다. 이 마지막 사례에서 사용자는 위치를 보고 iOS 10에서는 특정 개념 (예: 위치 또는 통신 상호 작용)을 더 잘 인식 합니다.

마지막 화면을 자세히 살펴보겠습니다.

[![](proactive-suggestions-images/activity03.png "NSUserActivity 세부 정보")](proactive-suggestions-images/activity03.png#lightbox)

여기서 앱은를 `NSUserActivity` 만들고 나중에 상태를 다시 만들기 위한 정보로 채워져 있습니다. 앱에는 위치 이름 및 주소와 같은 메타 데이터도 포함 되어 있습니다. 이 활동을 만든 후 앱에서 iOS에서 사용자의 현재 상태를 나타내는지 알 수 있습니다.

그런 다음 앱은 위치 제안에 대 한 임시 값으로 저장 되거나, 검색 결과에 표시 하기 위해 장치에서 스포트라이트 인덱스에 추가 되는 활동을 무선으로 알릴지 여부를 결정 합니다.

핸드 오프 및 스포트라이트 검색에 대 한 자세한 내용은 전달 [소개](~/ios/platform/handoff.md) 및 [IOS 9 새 검색 api](~/ios/platform/search/index.md) 가이드를 참조 하세요.

### <a name="creating-an-activity"></a>활동 만들기

활동을 만들기 전에 활동 유형 식별자를 생성 하 여 해당 활동을 식별 해야 합니다. 활동 유형 식별자는 지정 된 사용자 활동 유형을 고유 하 `NSUserActivityTypes` 게 식별 하는 데 `Info.plist` 사용 되는 앱 파일의 배열에 추가 되는 짧은 문자열입니다. 앱이 지원 하 고 앱 검색에 노출 하는 각 작업에 대해 배열에 하나의 항목이 있습니다. 자세한 내용은 [활동 형식 식별자 참조 만들기](~/ios/platform/search/nsuseractivity.md) 를 참조 하세요.

활동의 예를 살펴보세요.

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

활동 형식 식별자를 사용 하 여 새 활동을 만듭니다. 그런 다음 작업을 정의 하는 일부 메타 데이터가 생성 되므로 나중에이 상태를 복원할 수 있습니다. 그러면 활동에 의미 있는 제목이 지정 되 고 사용자 정보에 연결 됩니다. 마지막으로 일부 기능은 사용 하도록 설정 되 고 작업은 시스템으로 전송 됩니다.

위의 코드를 다음과 같이 변경 하 여 활동에 컨텍스트를 제공 하는 메타 데이터를 포함 하도록 추가로 향상 시킬 수 있습니다.

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

개발자에 게 앱과 동일한 정보를 표시할 수 있는 웹 사이트가 있는 경우 앱은 URL을 포함할 수 있으며 앱이 설치 되지 않은 다른 장치 (전달을 통해)에 콘텐츠가 표시 될 수 있습니다.

```csharp
// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");
```

### <a name="restoring-an-activity"></a>활동 복원

앱에 대 한 검색 결과 (`NSUserActivity`)를 누르는 사용자에 게 응답 하려면 **AppDelegate.cs** `ContinueUserActivity` 파일을 편집 하 고 메서드를 재정의 합니다. 예:

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

개발자가 위에서 만든 활동과 동일한 활동 유형 식별자 (`com.xamarin.platform`) 인지 확인 해야 합니다. 앱은에 저장 `NSUserActivity` 된 정보를 사용 하 여 사용자가 남은 위치로 상태를 다시 복원 합니다.

### <a name="benefits-of-creating-an-activity"></a>활동을 만들 때의 이점

앱은 위에 표시 된 최소한의 코드를 사용 하 여 다음과 같은 세 가지 새로운 iOS 10 기능을 활용할 수 있습니다.

- **Handoff**
- **스포트라이트 검색**
- **상황별 Siri 미리 알림**

다음 섹션에서는 두 가지 다른 새로운 iOS 10 기능을 사용 하도록 설정 하는 방법을 살펴봅니다.

- **위치 제안**
- **컨텍스트 Siri 요청**

### <a name="location-based-suggestions"></a>위치 기반 제안 

위의 식당 검색 앱 예제를 예로 들어 보겠습니다. 메타 데이터와 특성 `NSUserActivity` 을 모두 구현 하 고 올바르게 채우면 사용자가 다음을 수행할 수 있습니다.

1. 앱에서 친구를 충족 하고자 하는 식당을 찾습니다.
2. 사용자가 멀티태스킹 앱 전환기를 사용 하 여 앱에서 밖으로 이동 하는 경우, 시스템은 즐겨 찾는 탐색 앱을 사용 하 여 식당에 대 한 지침을 얻기 위해 자동으로 화면 맨 아래에 제안을 표시 합니다.
3. 사용자가 메시지 앱으로 전환 하 고 "을 ( *를) 사용해 보세요."* 입력을 시작 하는 경우 quicktype 키보드는 식당의 주소에 붙여넣기를 자동으로 제안 합니다.
4. 사용자가 Maps 앱으로 전환 하는 경우 식당의 주소가 자동으로 대상으로 제안 됩니다.
5. 이는 타사 앱 (지원 `NSUserActivity`되는)에도 적용 되므로 사용자가 자동으로 실행 되는 앱으로 전환할 수 있으며 식당의 주소도 자동으로 대상으로 제안 됩니다.
6. 또한 Siri에 대 한 컨텍스트를 제공 하므로 사용자는 식당 앱 내에서 Siri를 호출 하 고 *"방향 가져오기 ..."* 를 요청 하 고 siri는 사용자가 보고 있는 식당에 방향을 제공 합니다.

위의 모든 기능에는 공통적인 내용이 하나 있으며,이는 모두 제안 내용이 원래 제공 되는 위치를 표시 합니다. 위의 예에서는 가상의 식당 리뷰 앱입니다.

iOS 10은 몇 가지 작은 수정과 기존 프레임 워크에 대 한 추가 기능을 통해 앱에서이 기능을 사용할 수 있도록 향상 되었습니다.

- `NSUserActivity`에는 앱 내에서 볼 수 있는 위치 정보를 캡처하기 위한 추가 필드가 있습니다.
- MapKit 및 CoreSpotlight location에 대 한 몇 가지 추가 기능이 추가 되었습니다.
- 위치 인식 기능이 시스템 내의 Siri, 지도, 키보드, 멀티태스킹 및 기타 앱에 추가 되었습니다.

위치 기반 제안을 구현 하려면 위에서 설명한 것과 동일한 활동 코드를 사용 하 여 시작 합니다.

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

앱이 mapkit를 사용 하는 경우 현재 지도 `MKMapItem` 를 활동에 추가 하는 것 만큼 간단 합니다.

```csharp
// Save MKMapItem location
activity.MapItem = myMapItem;
```

앱이 MapKit를 사용 하지 않는 경우 앱 검색을 채택 하 고 위치에 대해 다음과 같은 새 특성을 지정할 수 있습니다.

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

위의 코드를 자세히 살펴보세요. 먼저, 위치 이름은 모든 인스턴스에서 필요 합니다.

```csharp
attributes.NamedLocation = "Apple Inc.";
```

그런 다음 텍스트 기반 인스턴스 (예: QuickType 키보드)에 대해 위치에 대 한 텍스트 기반 설명이 필요 합니다.

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

위도 및 경도는 선택 사항 이지만, 사용자가 앱을 보내려는 정확한 위치로 라우팅해야 합니다. 따라서 포함 해야 합니다.

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

전화 번호를 설정 하 여 앱은 Siri에 대 한 액세스 권한을 얻을 수 있으므로 사용자가 앱에서 Siri를 호출할 수 있습니다. 예를 들어 *"이 위치를 호출"* 합니다.

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

마지막으로 앱은 인스턴스가 탐색 및 전화 통화에 적합 한지 여부를 나타낼 수 있습니다.

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

### <a name="implementing-contact-interactions"></a>연락처 상호 작용 구현

IOS 10의 새로운 기능은 통신 앱이 연락처 카드에서 연락처 앱에 긴밀 하 게 통합 됩니다. 연락처 상호 작용이 구현 된 앱의 경우 사용자는 지정 된 앱 정보를 연락처의 특정 사용자에 게 추가할 수 있습니다. 예를 들어 카드 맨 위에 있는 빠른 작업 단추를 클릭 하 여 메시지를 보내는 경우 첨부 된 앱은에서 메시지를 보내는 옵션으로 나열 됩니다.

타사 앱을 선택 하면 다음 번에 사용자가 연락할 때 지정 된 사용자에 게 메시지를 표시 하는 기본 방법으로 해당 앱을 기억 하 고 표시할 수 있습니다.

담당자는를 사용 하 여 앱에서 `NSUserActivity` 구현 되며 iOS 10에 도입 된 새로운 의도 프레임 워크를 사용 합니다. 의도로 작업 하는 방법에 대 한 자세한 내용은 [Sirikit 개념 이해](~/ios/platform/sirikit/understanding-sirikit.md) 및 [Sirikit 구현](~/ios/platform/sirikit/implementing-sirikit.md) 가이드를 참조 하세요.

#### <a name="donating-interactions"></a>기증 상호 작용

앱이 상호 작용을 기부 하는 방법을 살펴보세요.

[![](proactive-suggestions-images/activity04.png "기증 상호 작용 개요")](proactive-suggestions-images/activity04.png#lightbox)

앱 `INInteraction` 은 **의도** (`INIntent`), **참가자** 및 **메타 데이터**를 포함 하는 개체를 만듭니다. **의도** 는 비디오 통화 또는 문자 메시지 전송과 같은 사용자 작업을 나타냅니다. **참가자** 에 게는 통신을 받는 사람이 포함 됩니다. **메타 데이터** 는 메시지를 성공적으로 보낸 등의 추가 정보를 정의 합니다.

개발자는 또는 `INIntent` `INIntentResponse`의 인스턴스를 직접 만들지 않습니다. 이러한 부모 클래스에서 상속 되는 특정 자식 클래스 (앱이 사용자를 대신 하 여 수행 하는 작업을 기반으로 함) 중 하나를 사용 합니다. 예를 `INSendMessageIntent` 들어 문자 `INSendMessageIntentResponse` 메시지를 보내기 위한 및입니다. 

상호 작용이 완전히 채워지면 `DonateInteraction` 메서드를 호출 하 여 상호 작용을 사용할 수 있다는 사실을 시스템에 알립니다.

사용자가 연락처 카드에서 앱과 상호 작용할 때 상호 작용은와 함께 `NSUserActivity`제공 되며,이는 앱을 시작 하는 데 사용 됩니다.

[![](proactive-suggestions-images/activity05.png "상호 작용은 앱을 시작 하는 데 사용 되는 NSUserActivity와 함께 제공 됩니다.")](proactive-suggestions-images/activity05.png#lightbox)

메시지 보내기 의도에 대 한 다음 예를 살펴보겠습니다.

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

이 코드를 자세히 살펴보면의 `NSUserActivity` 인스턴스를 만들고 채웁니다. 위의 [작업 만들기](#creating-an-activity) 섹션에서 볼 수 있습니다. 그런 다음에서 `INIntent`상속 되는의 `INSendMessageIntent` 인스턴스를 만들고 전송 중인 메시지의 세부 정보로 채웁니다.

```csharp
var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);
```

가 만들어지고 위에서 만든를 전달 `NSUserActivity`합니다. `INSendMessageIntentResponse`

```csharp
var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);
```

는 전송 메시지 의도 (`INSendMessageIntent`) 및 응답 (`INSendMessageIntentResponse`)을 통해 작성 됩니다.`INInteraction`

```csharp
var interaction = new INInteraction (intent, response);
```

마지막으로 시스템은 상호 작용에 대 한 알림입니다.

```csharp
// Donate interaction to the system
interaction.DonateInteraction ((err) => {
  // Handle donation error
  ...
});
```

응용 프로그램이 성공 또는 실패에 대 한 응답을 처리할 수 있는 완료 처리기가 전달 됩니다.

### <a name="activities-best-practices"></a>활동 모범 사례

Apple은 활동 작업을 수행할 때 다음과 같은 모범 사례를 제안 합니다.

- 지연 `NeedsSave` 페이로드 업데이트에 사용 합니다.
- 현재 활동에 대 한 강력한 참조를 유지 해야 합니다.
- 상태를 복원 하는 데 충분 한 정보만 포함 하는 작은 페이로드를 전송 합니다.
- 활동 유형 식별자가 고유한 지 확인 하 고 역방향 DNS 표기법을 사용 하 여 지정 합니다. 

## <a name="schemaorg"></a>Schema.org

위와 같이를 사용 `NSUserActivity` 하면 시스템에서 사용자가 현재 화면에서 작업 하 고 있는 정보를 이해할 수 있습니다. Schema.org는 웹 페이지에 비슷한 기능을 추가 합니다.

Schema.org는 웹 사이트와 동일한 유형의 위치 기반 상호 작용을 제공할 수 있습니다. Apple은 네이티브 앱에서와 같이 Safari에서 볼 때 뿐만 아니라 새로운 위치 제안도 사용할 수 있도록 설계 되었습니다.

일부 Schema.org 배경:

- 개방형 웹 태그 어휘 표준을 제공 합니다.
- 웹 페이지에 구조화 된 메타 데이터를 포함 하는 방식으로 작동 합니다.
- 사용 가능한 다양 한 개념을 나타내는 500 개 이상의 스키마가 있습니다.
- 개발자는 웹 사이트에서 구현 하 여 네이티브 앱에서를 사용 하 `NSUserActivity` 는 몇 가지 이점을 얻을 수 있습니다.

스키마는 *식당*등의 특정 형식이 *로컬 비즈니스*와 같은 보다 일반적인 형식에서 상속 하는 구조와 같은 트리로 정렬 됩니다. 자세한 내용은 [Schema.org](http://schema.org)를 참조 하세요.

예를 들어 웹 페이지에 다음 데이터가 포함 되어 있는 경우:

```html
<script type="application/ld+json">
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

사용자가 Safari에서이 페이지를 방문한 후 다른 앱으로 전환 하는 경우 페이지의 위치 정보가 캡처되고 위의 작업에서 볼 수 있듯이 시스템의 다른 부분에서 위치 제안으로 제공 됩니다.

Safari는 웹 페이지에서 다음 스키마 속성을 따르는 모든 항목을 추출 합니다.

- **PostalAddress**
- **GeoCoordinates**
- 전화 속성입니다.

자세한 내용은 [웹 마크업으로 검색](~/ios/platform/search/web-markup.md) 가이드를 참조 하세요.

## <a name="consuming-location-suggestions"></a>위치 제안 사용

다음 섹션에서는 시스템의 다른 부분 (예: 맵 앱) 또는 기타 타사 앱에서 제공 하는 위치 제안을 다룹니다.

앱에서 위치 제안을 사용 하는 두 가지 주요 방법이 있습니다.

- QuickType 키보드를 통해
- 라우팅 앱에서 위치 정보를 사용 하 여 직접.

### <a name="location-suggestions-and-the-quicktype-keyboard"></a>위치 제안 및 QuickType 키보드

앱에서 텍스트 기반 형식의 주소를 처리 하는 경우 응용 프로그램은 QuickType UI를 통해 위치 제안을 활용할 수 있습니다. iOS 10은 다음과 같은 기능으로 QuickType UI를 확장 합니다.

- 앱은 UI의 텍스트 필드에 대 한 의미 체계 의도에 대 한 힌트를 추가할 수 있습니다.
- 앱은 앱에서 사전 제안을 받을 수 있습니다.
- 앱은 향상 된 자동 고침의 이점을 누릴 수 있습니다.

IOS 10 `TextContentType` 에 있는 텍스트 필드 컨트롤의 새 속성을 사용 하면 개발자는 지정 된 필드에 사용자가 입력 하는 값에 대 한 의미 체계 의도를 정의할 수 있습니다. 예를 들어:

```csharp
var textField = new UITextField();
textField.TextContentType = UITextContentType.FullStreetAddress;
```

는 앱이 사용자에 게 지정 된 필드에 전체 주소를 입력 하는 것을 기대 한다는 사실을 시스템에 알립니다. 이렇게 하면 사용자가이 필드에 값을 입력 하는 경우 QuickType 키보드에서 키보드의 위치 제안을 자동으로 제공할 수 있습니다.

다음은 `UITextContentType` 정적 클래스의 개발자가 사용할 수 있는 몇 가지 일반적인 형식입니다.

- `Name`
- `GivenName`
- `FamilyName`
- `Location`
- `FullStreetAddress`
- `AddressCityAndState`
- `TelephoneNumber`
- `EmailAddress`

### <a name="routing-apps-and-locations-suggestions"></a>라우팅 앱 및 위치 제안

이 섹션에서는 라우팅 앱 내에서 직접 위치 제안을 사용 하는 방법을 살펴봅니다. 이 기능을 추가 하기 위한 라우팅 앱의 경우 개발자는 다음과 같이 기존 `MKDirectionsRequest` 프레임 워크를 활용 합니다.

- 멀티태스킹에서 앱을 승격 합니다.
- 앱을 라우팅 앱으로 등록 합니다.
- Mapkit `MKDirectionsRequest` 개체를 사용 하 여 앱을 시작 하는 것을 처리 합니다.
- IOS에 게 사용자 참여를 기준으로 적절 한 시간에 앱을 제안 하는 방법을 배울 수 있는 기능을 제공 합니다.

Mapkit `MKDirectionsRequest` 개체를 사용 하 여 앱을 시작 하는 경우 자동으로 사용자에 게 요청 된 위치에 대 한 지침을 제공 하거나 사용자가 쉽게 지침을 얻기 위해 UI를 제공 해야 합니다. 예를 들어:

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

이 코드에 대 한 자세한 내용을 살펴보세요. 올바른 대상 요청 인지 테스트 합니다.

```csharp
if (MKDirectionsRequest.IsDirectionsRequestUrl(url)) {
```

인 경우 URL에서을 `MKDirectionsRequest` 만듭니다.

```csharp
var request = new MKDirectionsRequest(url);
```

IOS 10의 새로운 기능으로, 지역 좌표가 없는 주소를 앱에 전송할 수 있습니다. 그러면 개발자가 주소를 인코딩해야 합니다.

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
  // Handle the display of the address
  
});

```

## <a name="media-app-suggestions"></a>미디어 앱 제안

IOS 10을 사용 하 여 앱이 포드캐스트 앱 이나 오디오 또는 비디오와 같은 스트리밍 미디어 콘텐츠를 처리 하는 경우에는 시스템에서 미디어 제안을 활용할 수 있습니다.

미디어를 처리 하는 앱의 경우 iOS는 다음 동작을 지원 합니다.

- iOS는 이전 동작에 따라 사용자가 사용할 가능성이 높은 앱을 승격 합니다.
- 앱과 관련 된 제안이 스포트라이트 및 오늘 보기에 표시 됩니다.
- 앱이 다음 트리거 중 하나를 충족 하는 경우 잠금 화면 제안으로 상승 될 수 있습니다.
  - 헤드폰 또는 Bluetooth 장치를 연결한 후 연결을 만듭니다.
  - 자동차를 가져온 후
  - 집 또는 직장에 도착 한 후 

개발자는 iOS 10에 간단한 API 호출을 포함 하 여 미디어 앱의 사용자에 게 더욱 매력적인 잠금 화면 환경을 만들 수 있습니다. 미디어 재생을 `MPPlayableContentManager` 관리 하기 위해 클래스를 사용 하 여 전체 미디어 컨트롤 (음악 앱에서 제공 하는 것과 같은)이 앱의 잠금 화면에 표시 됩니다.

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

이 문서에서는 사전 권장 사항에 대해 설명 하 고 개발자가 Xamarin.ios 앱에 대 한 트래픽을 구동 하는 데 사용할 수 있는 방법을 살펴보았습니다. 사전 제안 및 제시 된 사용 지침을 구현 하는 단계를 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [SiriKit 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
