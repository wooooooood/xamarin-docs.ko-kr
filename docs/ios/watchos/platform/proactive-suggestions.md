---
title: Xamarin의 사전 예방적 제안 watchOS
description: 이 문서에서는 watchOS 3 앱에서 자동 관리 제안을 사용 하 여 시스템에서 자동으로 유용한 정보를 사용자에 게 자동으로 제공할 수 있도록 하 여 engagement를 구동 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 10CC9F16-963C-44F1-8B98-F09FB2310DFF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 95e0eb77719a9bcc642dfb1385d7371652943155
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2019
ms.locfileid: "70198811"
---
# <a name="watchos-proactive-suggestions-in-xamarin"></a>Xamarin의 사전 예방적 제안 watchOS

_이 문서에서는 watchOS 3 앱에서 자동 관리 제안을 사용 하 여 시스템에서 자동으로 유용한 정보를 사용자에 게 자동으로 제공할 수 있도록 하 여 engagement를 구동 하는 방법을 보여 줍니다._


WatchOS 3을 처음 사용 하는 경우 자동으로 사용자에 게 유용한 정보를 사용자에 게 자동으로 제공 하 여 사용자가 Xamarin.ios 앱에 참여할 수 있는 뉴스 방법을 제공 합니다.


## <a name="about-proactive-suggestions"></a>자동 관리 제안 정보

WatchOS 3 `NSUserActivity` 의 새로운 기능에는 `MapItem` 앱이 다른 컨텍스트에서 사용할 수 있는 위치 정보를 제공할 수 있도록 하는 속성이 포함 되어 있습니다. 예를 들어 호텔에 표시 된 앱에서 `MapItem` 위치를 검토 하 고 제공 하는 경우 사용자가 Maps 앱으로 전환 된 경우에는 자신이 보는 호텔의 위치를 사용할 수 있습니다.

앱은, mapkit, Media Player 및 uikit와 `NSUserActivity`같은 기술 컬렉션을 사용 하 여 시스템에이 기능을 노출 합니다. 또한 앱에 대 한 사전 제안 지원을 제공 하 여 더 심층적인 Siri 통합을 무료로 제공 합니다.

## <a name="location-based-suggestions"></a>위치 기반 제안

WatchOS 3의 새로운 기능으로 `NSUserActivity` , 클래스에 `MapItem` 는 개발자가 다른 컨텍스트에서 사용할 수 있는 위치 정보를 제공할 수 있는 속성이 포함 되어 있습니다. 예를 들어 앱에서 식당 리뷰를 표시 하는 경우 개발자는 사용자 `MapItem` 가 앱에서 보고 있는 식당의 위치로 속성을 설정할 수 있습니다. 사용자가 Maps 앱으로 전환 하는 경우 식당의 위치를 자동으로 사용할 수 있습니다.

앱이 앱 검색을 지 원하는 경우 `CSSearchableItemAttributesSet` 클래스의 새 주소 구성 요소를 사용 하 여 사용자가 방문할 수 있는 위치를 지정할 수 있습니다. `MapItem` 속성을 설정 하면 다른 속성이 자동으로 채워집니다.

주소 구성 요소 속성의 `Latitude` 및 `Longitude` 를 설정 하는 것 외에도 앱에서 `NamedLocation` 및 `PhoneNumbers` 속성을 제공 하는 것이 좋습니다. 그러면 siri가 해당 위치에 대 한 호출을 시작할 수 있습니다.

## <a name="contextual-siri-reminders"></a>상황별 Siri 미리 알림

사용자가 Siri를 사용 하 여 나중에 앱에서 현재 보고 있는 콘텐츠를 확인 하는 미리 알림을 신속 하 게 만들 수 있습니다. 예를 들어 앱에서 식당 리뷰를 보고 있는 경우 Siri를 호출 하 고 *"집에서이를 받을 때이에 대해 알림"* 이라고 말할 수 있습니다. Siri는 앱의 검토에 대 한 링크가 포함 된 미리 알림을 생성 합니다.

## <a name="implementing-proactive-suggestions"></a>자동 관리 제안 구현

Xamarin.ios 앱에 사전 제안 지원을 추가 하는 것은 일반적으로 몇 가지 Api를 구현 하거나 앱이 이미 구현할 수 있는 몇 가지 Api를 확장 하는 것 만큼 쉽습니다.

사전 권장 사항은 다음과 같은 세 가지 주요 방법으로 앱과 함께 작동 합니다.

- **`NSUserActivity`** -시스템에서 사용자가 현재 화면에서 작업 하 고 있는 정보를 이해 하는 데 도움이 됩니다.
- **위치 제안** -앱에서 위치 기반 정보를 제공 하거나 사용 하는 경우 이러한 API 확장을 통해 앱 간에이 정보를 공유할 수 있는 새로운 방법을 제공 합니다.

및은 다음을 구현 하 여 앱에서 지원 됩니다.

- IOS 10 `NSUserActivity` 의 **상황별 siri 미리 알림** -siri가 나중에 앱에서 현재 보고 있는 콘텐츠를 확인 하는 미리 알림을 신속 하 게 만들 수 있도록 확장 되었습니다.
- **위치 제안** -iOS 10은 `NSUserActivity` 앱 내에서 볼 수 있는 캡처 위치를 개선 하 고 시스템 전체의 여러 위치에서 승격 합니다.
- **컨텍스트 siri 요청**  -  `NSUserActivity` 은 응용 프로그램 내에서 siri에 게 제공 되는 정보에 대 한 컨텍스트를 제공 하므로 사용자는 지침을 얻거나 앱 내에서 siri를 호출 하는 호출을 수행할 수 있습니다.

이러한 모든 기능은 공통적으로 하나를 사용 `NSUserActivity` 하 여 해당 기능을 제공 합니다. 

## <a name="nsuseractivity"></a>NSUserActivity

위에서 설명한 것 처럼 `NSUserActivity` 시스템에서 사용자가 현재 화면에서 작업 하 고 있는 정보를 이해 하는 데 도움을 줍니다. `NSUserActivity`는 앱을 탐색할 때 사용자의 작업을 캡처하는 경량 상태 캐싱 메커니즘입니다. 예를 들어 식당 앱을 살펴보면 다음과 같습니다.

[![](proactive-suggestions-images/activity02.png "식당 앱")](proactive-suggestions-images/activity02.png#lightbox)

다음과 같은 상호 작용을 사용 합니다.

1. 사용자가 앱 `NSUserActivity` 에서 작업할 때 나중에 앱의 상태를 다시 만들기 위해가 만들어집니다.
2. 사용자가 식당을 검색 하는 경우 동일한 유형의 활동을 만듭니다.
3. 그리고 사용자가 결과를 볼 때를 다시 사용 합니다. 이 마지막 사례에서 사용자는 위치를 보고 iOS 10에서는 특정 개념 (예: 위치 또는 통신 상호 작용)을 더 잘 인식 합니다.

마지막 화면을 자세히 살펴보겠습니다.

[![](proactive-suggestions-images/activity03.png "NSUserActivity 페이로드")](proactive-suggestions-images/activity03.png#lightbox)

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

앱에 대 한 검색 결과 (`NSUserActivity`)를 누르는 사용자에 게 응답 하려면 **AppDelegate.cs** `ContinueUserActivity` 파일을 편집 하 고 메서드를 재정의 합니다. 예를 들어:

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

위에서 만든 활동과 동일한 활동 유형 식별자 (`com.xamarin.platform`)를 확인 합니다. 앱은에 저장 `NSUserActivity` 된 정보를 사용 하 여 사용자가 남은 위치로 상태를 다시 복원 합니다.

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
2. 사용자가 Maps 앱으로 전환 하는 경우 식당의 주소가 자동으로 대상으로 제안 됩니다.
3. 이는 타사 앱 (지원 `NSUserActivity`되는)에도 적용 되므로 사용자가 자동으로 실행 되는 앱으로 전환할 수 있으며 식당의 주소도 자동으로 대상으로 제안 됩니다.
4. 또한 Siri에 대 한 컨텍스트를 제공 하므로 사용자는 식당 앱 내에서 Siri를 호출 하 고 *"방향 가져오기 ..."* 를 요청 하 고 siri는 사용자가 보고 있는 식당에 방향을 제공 합니다.

위의 모든 기능에는 공통적인 내용이 하나 있으며,이는 모두 제안 내용이 원래 제공 되는 위치를 표시 합니다. 위의 예에서는 가상의 식당 리뷰 앱입니다.

watchOS 3은 몇 가지 작은 수정과 기존 프레임 워크에 대 한 추가 기능을 통해 앱에서이 기능을 사용할 수 있도록 향상 되었습니다.

- `NSUserActivity`에는 앱 내에서 볼 수 있는 위치 정보를 캡처하기 위한 추가 필드가 있습니다.
- MapKit 및 CoreSpotlight location에 대 한 몇 가지 추가 기능이 추가 되었습니다.
- 위치 인식 기능이 시스템 내의 Siri, Maps, 멀티태스킹 및 기타 앱에 추가 되었습니다.

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

텍스트 기반 인스턴스에 필요한 텍스트 기반 설명 (예: QuickType 키보드):

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

위도 및 경도는 선택 사항 이지만, 사용자가 앱이 보내려는 정확한 위치로 라우팅됩니다.

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

전화 번호를 설정 하 여 앱은 Siri에 대 한 액세스 권한을 얻을 수 있으므로 사용자가 앱에서 Siri를 호출할 수 있습니다 (예: * "이 위치 호출").

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

마지막으로 앱은 인스턴스가 탐색 및 전화 통화에 적합 한지 여부를 나타낼 수 있습니다.

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

## <a name="activities-best-practices"></a>활동 모범 사례

Apple은 활동 작업을 수행할 때 다음과 같은 모범 사례를 제안 합니다.

- 지연 `NeedsSave` 페이로드 업데이트에 사용 합니다.
- 현재 활동에 대 한 강력한 참조를 유지 해야 합니다.
- 상태를 복원 하는 데 충분 한 정보만 포함 하는 작은 페이로드를 전송 합니다.
- 활동 유형 식별자가 고유한 지 확인 하 고 역방향 DNS 표기법을 사용 하 여 지정 합니다. 

## <a name="consuming-location-suggestions"></a>위치 제안 사용

다음 섹션에서는 시스템의 다른 부분 (예: 맵 앱) 또는 기타 타사 앱에서 제공 하는 위치 제안을 다룹니다.

## <a name="routing-apps-and-locations-suggestions"></a>라우팅 앱 및 위치 제안

이 섹션에서는 라우팅 앱 내에서 직접 위치 제안을 사용 하는 방법을 살펴봅니다. 이 기능을 추가 하기 위한 라우팅 앱의 경우 개발자는 다음과 같이 기존 `MKDirectionsRequest` 프레임 워크를 활용 합니다.

- 멀티태스킹에서 앱을 승격 합니다.
- 앱을 라우팅 앱으로 등록 합니다.
- Mapkit `MKDirectionsRequest` 개체를 사용 하 여 앱을 시작 하는 것을 처리 합니다.
- 사용자 참여를 기준으로 앱을 제안 하는 방법에 대 한 watchOS을 제공 합니다.

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

WatchOS 3의 새로운 기능으로, 지역 좌표가 없는 주소를 앱에 전송할 수 있습니다. 그러면 개발자가 주소를 인코딩해야 합니다.

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address

});

```

## <a name="summary"></a>요약

이 문서에서는 사전 권장 사항에 대해 설명 하 고 개발자가 watchOS 앱에 대 한 트래픽을 구동 하는 데 사용할 수 있는 방법을 보여 주었습니다. 사전 제안 및 제시 된 사용 지침을 구현 하는 단계를 설명 했습니다.


## <a name="related-links"></a>관련 링크

- [watchOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+watchOS)
- [SiriKit 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
