---
title: watchOS에서 Xamarin 자동 제안
description: 이 문서에서는 시스템을 사전에 사용자에 게 유용한 정보를 자동으로 제공 함으로써 자동 제안 드라이브 engagement 3 watchOS 앱을 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 10CC9F16-963C-44F1-8B98-F09FB2310DFF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 979b103db478e3888d3a3c20df6afbd91d0c37d8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61386541"
---
# <a name="watchos-proactive-suggestions-in-xamarin"></a>watchOS에서 Xamarin 자동 제안

_이 문서에서는 시스템을 사전에 사용자에 게 유용한 정보를 자동으로 제공 함으로써 자동 제안 드라이브 engagement 3 watchOS 앱을 사용 하는 방법을 보여 줍니다._


WatchOS 3, 자동 제안 있는 뉴스 방법 사용자에 게 적절 한 시간에 자동으로 사용자에 게 사전에 있는 유용한 정보에서 Xamarin.iOS 앱을 이용 하려면 새입니다.


## <a name="about-proactive-suggestions"></a>자동 제안에 대 한

WatchOS 3, 새 `NSUserActivity` 포함을 `MapItem` 속성을 사용 하는 응용 프로그램에서 다른 컨텍스트에서 사용할 수 있는 위치 정보를 제공 합니다. 예를 들어 앱이 표시 호텔 검토 하 고 제공 된 `MapItem` 위치, 사용자 맵 앱으로 전환 하는 경우, 보기만 된 호텔의 위치로 사용할 수 있습니다.

앱에서와 같은 기술의 모음을 사용 하 여 시스템에이 기능을 노출 `NSUserActivity`, MapKit, Media Player 및 UIKit 합니다. 또한 앱에 대 한 사전 제안 지원을 제공 하 여 가져옵니다 밀접 하 게 Siri 통합을 무료.

## <a name="location-based-suggestions"></a>위치 기반 추천

WatchOS 3에는 `NSUserActivity` 클래스에 포함 되어는 `MapItem` 속성을 사용 하는 다른 컨텍스트에서 사용할 수 있는 위치 정보를 제공 하는 개발자. 예를 들어, 앱 음식점 리뷰를 표시 하는 경우 개발자 설정할 수는 `MapItem` 속성을 앱에서 사용자가 보고 하는 레스토랑의 위치입니다. 사용자가 Maps 앱 전환, 식당의 위치는 자동으로 가능 합니다.

새 주소 구성 요소를 사용할 수 있는 앱을 앱 검색을 지 원하는 경우는 `CSSearchableItemAttributesSet` 사용자 방문 하려고 수 있는 위치를 지정 하는 클래스입니다. 설정 하 여는 `MapItem` 속성을 다른 속성은에 자동으로 채워집니다.

설정 외에도 합니다 `Latitude` 및 `Longitude` 주소 구성 요소 속성의 것이 좋습니다는 앱 제공 합니다 `NamedLocation` 및 `PhoneNumbers` 속성도 따라서 Siri를 시작할 수 있습니다 위치에 대 한 호출 합니다.

## <a name="contextual-siri-reminders"></a>상황에 맞는 Siri 미리 알림

Siri를 사용 하 여 나중에 앱에서 현재 표시 되어 콘텐츠를 보려면 미리 알림을 신속 하 게 사용할 수 있도록 할 수 있습니다. 예를 들어 앱에서 음식점 리뷰를 보고 있던 것을 하는 경우 없습니다 Siri를 호출 하며 예를 들어 *"생각나이 대 한 홈 받으면."* Siri는 앱에서 검토에 대 한 링크가 포함 된 미리 알림을 생성 합니다.

## <a name="implementing-proactive-suggestions"></a>자동 제안 구현

추가 사전 제안 Xamarin.iOS 앱에 지원은 일반적으로 몇 가지 Api를 구현 하거나 앱에서 이미 구현할 수 있는 몇 가지 Api를 확장 하는 것 만큼 쉽습니다.

자동 제안 세 가지 주요 방법으로 앱을 사용 하 여 작동 합니다.

- **`NSUserActivity`** -화면에서 사용 하 여 사용자가 현재 작업 정보를 이해 하는 시스템 수 있습니다.
- **위치 제안을** -앱을 제공 하거나 앱에서이 정보를 공유 하는 데 위치 기반 정보를 이러한 API 확장 제품의 새로운 방법을 사용 하는 경우.

다음을 구현 하 여 앱에서 지원 됩니다.

- **상황에 맞는 Siri 미리** -iOS 10에서에서 `NSUserActivity` 나중에 앱에서 현재 표시 되어 콘텐츠를 보려면 미리 알림을 신속 하 게 사용할 수 있도록 Siri 허용 하도록 확장 되었습니다.
- **위치 제안을** -iOS 10을 향상 시킵니다 `NSUserActivity` 앱 내에서 표시 하는 위치를 캡처하고 시스템 전체에서 여러 위치에서 승격 해야 합니다.
- **상황에 맞는 Siri 요청**  -  `NSUserActivity` 컨텍스트를 받을 수 있도록 지침 또는 호출 되어야 하는 위치 앱 내에서 호출 Siri Siri를 앱 내에서 제공 되는 정보를 제공 합니다.

이러한 기능을 모두 한 가지 공통가 모두 사용 하 여 `NSUserActivity` 하나의 형태로 든 해당 기능을 제공 합니다. 

## <a name="nsuseractivity"></a>NSUserActivity

위에서 설명한 것 처럼 `NSUserActivity` 화면에서 사용 하 여 사용자가 현재 작업 정보를 이해 하는 시스템 수 있습니다. `NSUserActivity` 간단한 상태가 앱을 통해 탐색 하는 동안 사용자의 활동을 캡처하는 메커니즘을 캐시 합니다. 예를 들어 식당 응용 프로그램을 살펴보면 다음과 같습니다.

[![](proactive-suggestions-images/activity02.png "식당 앱")](proactive-suggestions-images/activity02.png#lightbox)

다음 상호:

1. 앱을 사용 하 여 작동 하므로 사용자는 `NSUserActivity` 나중에 앱의 상태를 다시 생성 됩니다.
2. 사용자를 식당에 대 한 검색 작업을 만드는 동일한 패턴 수행 됩니다.
3. 및 사용자는 결과 볼 때 다시 합니다. 이 마지막 경우 사용자가 보는 위치 이며 ios 10에서 시스템 (예: 위치 또는 통신 상호 작용) 특정 개념에 대해 알고 합니다.

마지막 화면에서 좀 더 자세히 살펴보겠습니다.

[![](proactive-suggestions-images/activity03.png "NSUserActivity 페이로드")](proactive-suggestions-images/activity03.png#lightbox)

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

동일한 활동 유형 식별자 인지 확인 (`com.xamarin.platform`) 위에서 만든 활동으로 합니다. 앱에 저장 된 정보를 사용 하는 `NSUserActivity` 다시 사용자 중단으로 상태를 복원 합니다.

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
4. 맵 앱으로 전환 하는 사용자, 경우 식당의 주소를 대상으로 자동으로 제안 됩니다.
5. 이 타사 앱에도 적합 (지 `NSUserActivity`) 식당의 주소는 자동으로 제안 있습니다 대상으로도 하 고 사용자는 공유 앱으로 전환할 수 있도록 합니다.
6. 또한 컨텍스트를 제공 Siri 사용자는 식당 앱 내에서 Siri를 호출 하 고 요청할 수 있도록 *"Get directions..."* Siri를 사용자가 보고 식당에는 지침을 제공 합니다.

위의 기능을 모두 한 가지 공통에 제안을에서 처음 제공 되는 위치를 나타내는 모두 합니다. 위 예의 경우이 가상의 식당 검토 앱입니다.

watchOS 3 몇 가지 작은 수정 및 기존 프레임 워크에 대 한 추가 통해 앱에 대 한이 기능을 사용 하도록 향상 되었습니다.

- `NSUserActivity` 앱 내에서 표시 되는 위치 정보를 캡처에 대 한 추가 필드에 있습니다.
- 위치를 캡처하려면 몇 가지 추가 MapKit를 CoreSpotlight 이루어졌습니다.
- 위치 인식 기능 Siri, 맵, 멀티태스킹 및 시스템 내에서 다른 앱에 추가 되었습니다.

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

그런 다음에 따라 설명 텍스트에 필요한 텍스트 기반 인스턴스 (예: QuickType 키보드):

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

위도 및 경도 선택적 이지만 사용자 앱은 보내 하려는 정확한 위치로 라우팅되는지 확인 합니다.

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

전화 번호를 설정 하 여 앱에 액세스할 수 있습니다 Siri 사용자와 같은 것을 알리는 하 여 앱에서 Siri를 호출할 수 있도록 * "이이 위치가 호출":

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

마지막으로 앱 인스턴스를 탐색 하 고 전화 통화 하는 데 적합 한지를 나타낼 수 있습니다.

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```
## <a name="activities-best-practices"></a>활동에 대 한 유용한 정보

Apple 활동을 사용 하 여 작업할 때 다음 모범 사례를 제안 합니다.

- 사용 하 여 `NeedsSave` 지연 페이로드 업데이트 합니다.
- 현재 활동에 대 한 강력한 참조를 유지 해야 합니다.
- 만 상태를 복원 하려면 충분 한 정보를 포함 하는 작은 페이로드를 전송 합니다.
- 활동 형식 식별자가 고유 하 고 설명이 포함 된 역방향 DNS 표기법을 사용 하 여이 지정 하 여 확인 합니다. 

## <a name="consuming-location-suggestions"></a>소비 위치 제안

이 다음 섹션에서는 사용 위치 제안 (예: 맵 앱) 시스템 또는 다른 타사 앱의 다른 부분에서을 설명 합니다.

## <a name="routing-apps-and-locations-suggestions"></a>라우팅 앱 및 위치 제안

이 섹션에는 라우팅 앱 내에서 직접 위치 제안을 사용 살펴보겠습니다를 걸립니다. 이 기능을 추가 하려면 라우팅 앱에 대 한 개발자가 활용 하 여 기존 `MKDirectionsRequest` 같이 프레임 워크:

- 멀티태스킹에서 앱을 승격 합니다.
- 라우팅 앱으로 앱을 등록 합니다.
- MapKit 사용 하 여 앱을 시작을 처리 하도록 `MKDirectionsRequest` 개체입니다.
- WatchOS 사용자 참여에 따라 앱을 제안 하는 방법을 배울 수 있는 기능을 제공 합니다.

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

새 3 watchOS 앱 보낼 수 있습니다이 없는 지역 좌표를 야기 하는 개발자의 주소를 인코드 해야 하는 주소:

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="summary"></a>요약

이 문서에 자동 제안 설명 및 방법을 개발자에 사용할 수는 Xamarin.iOS 앱에 트래픽을 운용 하기 watchOS에 설명 했습니다. 자동 제안을 구현 하는 단계를 설명 하 고 사용 지침을 제공 합니다.


## <a name="related-links"></a>관련 링크

- [watchOS 샘플](https://developer.xamarin.com/samples/watchos/all/)
- [SiriKit 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
