---
title: 자동 관리 제안
description: 이 문서는 사전 예방적으로 사용자에 게 유용한 정보를 자동으로 표시 하려면 시스템을 허용 하 여 사전 제안 드라이브 engagement에 watchOS 3 응용 프로그램에서 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 10CC9F16-963C-44F1-8B98-F09FB2310DFF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: f22be43f814865c3c14e12aa2aec3a8dbce09b7a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="proactive-suggestions"></a>자동 관리 제안

_이 문서는 사전 예방적으로 사용자에 게 유용한 정보를 자동으로 표시 하려면 시스템을 허용 하 여 사전 제안 드라이브 engagement에 watchOS 3 응용 프로그램에서 사용 하는 방법을 보여 줍니다._


새로운 watchOS 3, 자동 관리 제안 있는 뉴스 방법 사용자에 게 적절 한 시간에 자동으로 사용자에 게 사전 있는 유용한 정보 여 Xamarin.iOS 앱과 의견을 교환 합니다.


## <a name="about-proactive-suggestions"></a>사전 제안에 대 한

WatchOS 3, 새 `NSUserActivity` 포함 한 `MapItem` 속성을 사용 하는 응용 프로그램에서 다른 컨텍스트에서 사용할 수 있는 위치 정보를 제공 합니다. 예를 들어, 앱이 표시 호텔을 검토 하 고 제공 하는 경우는 `MapItem` 지도 응용 프로그램에 사용자를 전환 하는 경우 위치를 표시 하기만 된 호텔의 위치는 액세스할 수 있습니다.

와 같은 기술의 모음을 사용 하 여 시스템에이 기능을 노출할 응용 프로그램이 `NSUserActivity`, MapKit, 미디어 플레이어 및 UIKit 합니다. 또한 응용 프로그램에 대 한 사전 제안 지원 함으로써 가져옵니다 밀접 하 게 Siri 통합을 무료입니다.

## <a name="location-based-suggestions"></a>위치 기반 추천

WatchOS 3, 새는 `NSUserActivity` 클래스를 포함 한 `MapItem` 속성을 사용 하 고 개발자에 게 다른 컨텍스트에서 사용할 수 있는 위치 정보를 제공 합니다. 예를 들어 경우 음식점 리뷰를 표시 하는 응용 프로그램 개발자 설정할 수는 `MapItem` 속성을 사용자가 응용 프로그램에서 보고 하는 레스토랑의 위치입니다. 지도 앱으로 전환, 식당의 위치는 자동으로 사용할 수 있습니다.

새 주소 구성 요소를 사용할 수 있는 앱에서 앱 검색을 지 원하는 경우는 `CSSearchableItemAttributesSet` 사용자 방문 하려고 수 있는 위치를 지정 하는 클래스입니다. 설정 하 여는 `MapItem` 속성을 다른 속성은에 자동으로 채워집니다.

설정 뿐만 아니라는 `Latitude` 및 `Longitude` 주소 구성 요소 속성의 것이 좋습니다 응용 프로그램을 제공 합니다는 `NamedLocation` 및 `PhoneNumbers` 속성 구조를 따라서 Siri 수 전화를 걸기 시작 위치입니다.

## <a name="contextual-siri-reminders"></a>상황에 맞는 Siri 미리 알림

사용자를 신속 하 게 콘텐츠를 보려면 나중에 응용 프로그램에서 현재 보고는 미리 알림을 만들려면 Siri를 사용할 수 있습니다. 예를 들어 음식점 리뷰 보았던은 앱의 경우 한 수 있습니다 Siri 호출 예를 들어 *"메시지를이 대 한 홈 받으면."* Siri 앱에서 알림을 검토 링크와 함께 발생 합니다.

## <a name="implementing-proactive-suggestions"></a>사전 제안을 구현

추가 사전 제안 Xamarin.iOS 앱에 지원은 일반적으로 몇 가지 Api를 구현 하거나 응용 프로그램에서 이미 구현할 수 있는 몇 가지 Api에 확장 하는 것 만큼 쉽지 합니다.

사전 추천 앱으로 세 가지 주요 방법으로 작동합니다.

- **`NSUserActivity`** -사용자가 현재 작업 화면에 정보를 이해 하는 시스템을 수 있습니다.
- **위치 제안** -응용 프로그램을 제공 하거나 응용 프로그램 간에이 정보를 공유 하도록 기반 위치 정보를 이러한 API 확장 제공은 새로운 방법을 소비 하는 경우.

다음을 구현 하 여 응용 프로그램에서 지원 됩니다.

- **상황에 맞는 Siri 미리 알림** -10, iOS에서 `NSUserActivity` 신속 하 게 콘텐츠를 보려면 나중에 응용 프로그램에서 현재 보고는 미리 알림을 만들려면 Siri 허용 하도록 확장 되었습니다.
- **위치 제안** -iOS 10 향상 `NSUserActivity` 를 응용 프로그램 내에서 표시 하는 위치를 캡처하고 시스템 전체에서 여러 위치에서 승격 합니다.
- **상황에 맞는 Siri 요청**  -  `NSUserActivity` 컨텍스트 사용자 얻을 수 있도록 호출 될 위치 또는 방향으로 응용 프로그램 내에서 호출 Siri Siri를 내부 응용 프로그램에서 제공 되는 정보를 제공 합니다.

이러한 모든 기능은 공통점이 하나, 모두 사용 `NSUserActivity` 한 형식이 나 다른 해당 기능을 제공 합니다. 

## <a name="nsuseractivity"></a>NSUserActivity

위의 설명 대로 `NSUserActivity` 화면에 사용자가 현재 사용 정보를 이해 하는 시스템을 사용 합니다. `NSUserActivity` 간단한 상태 캐싱 응용 프로그램을 탐색할 때 사용자의 활동을 캡처할 수는 메커니즘입니다. 예를 들어 식당 응용 프로그램을 살펴보면 다음과 같습니다.

[![](proactive-suggestions-images/activity02.png "식당 응용 프로그램")](proactive-suggestions-images/activity02.png#lightbox)

다음과 같은 상호:

1. 사용자 앱과 함께 작동 한 `NSUserActivity` 를 나중에 응용 프로그램의 상태를 다시 생성 됩니다.
2. 사용자가 식당을 검색 하는 경우 동일한 패턴의 활동을 만드는 다음 됩니다.
3. 하 고 다시 사용자 결과 봅니다. 이 경우, 사용자가 위치를 보고 되며 10 ios에서는 시스템 (예: 위치 또는 통신 상호 작용) 특정 개념에 대해 알고 있습니다.

마지막 화면에 자세히 살펴보겠습니다.

[![](proactive-suggestions-images/activity03.png "NSUserActivity 페이로드")](proactive-suggestions-images/activity03.png#lightbox)

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

동일한 활동 유형 식별자 인지 확인 (`com.xamarin.platform`) 위에서 만든 활동으로 합니다. 앱에 저장 된 정보를 사용 하는 `NSUserActivity` 사용자 중단 이동 상태를 복원 합니다.

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
4. 지도 앱으로 전환, 식당의 주소를 대상으로 자동으로 제안 됩니다.
5. 이 타사 앱에도 적합 (지 원하는 `NSUserActivity`), 식당의 주소는 자동으로 제안 있습니다를 대상으로도 하 고 사용자 안에서 공유 응용 프로그램으로 전환할 수 있도록 합니다.
6. 사용자는 식당 응용 프로그램 내에서 Siri를 호출 하 고 요청 수 있으므로 Siri에 컨텍스트 또한 제공 *"Get directions..."* Siri 사용자가 보고 하 고 음식점에 대 한 지침을 제공 합니다.

위의 기능을 모두 한 가지 공통, 추천에서 원래 생성 되는 위치를 나타내는 모든은 됩니다. 위의 예제에서는 가상의 식당 검토 앱은 것입니다.

3 watchOS 몇 가지 작은 수정 및 기존 프레임 워크에 대 한 추가 통해 앱에 대 한이 기능을 사용 하도록 향상 되었습니다.

- `NSUserActivity` 앱 내에서 표시 되는 위치 정보를 캡처를 위한 추가 필드에 있습니다.
- 몇 가지 추가에 적용 된 MapKit 및 CoreSpotlight 위치를 캡처할 수 있습니다.
- 위치 인식 기능 Siri, 지도, 다중 작업 및 시스템 내에서 다른 앱에 추가 되었습니다.

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

그런 다음에 따라 설명 텍스트에 필요한 텍스트 기반 인스턴스 (예: QuickType 키보드의 경우):

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

위도 및 경도 선택 사항 이지만 사용자가 앱을 보내기 위한를 원하는 정확한 위치를 라우팅되는지 확인:

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

전화 번호를 설정 하 여 응용 프로그램에 액세스할 수 Siri 사용자 등의 제목을 한다는 것으로 앱에서 Siri를 호출할 수 있도록 * "이 이곳 호출할":

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

마지막으로, 응용 프로그램 인스턴스가 탐색 및 전화 통화에 대 한 적합 한 경우를 나타낼 수 있습니다.

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```
## <a name="activities-best-practices"></a>활동에 대 한 유용한 정보

활동을 작업할 때 다음 모범 사례를 제안 하는 Apple:

- 사용 하 여 `NeedsSave` 지연 페이로드 업데이트 합니다.
- 현재 활동에 대 한 강력한 참조를 확인 합니다.
- 상태를 복원 하는 데 충분 한 정보가 포함 된 작은 페이로드만 전송 됩니다.
- 활동 유형 식별자를 지정 역방향 DNS 표기법을 사용 하 여 고유 하 고 설명 있는지 확인 합니다. 

## <a name="consuming-location-suggestions"></a>사용 위치 제안

이 다음 섹션은 시스템 (예: 지도 앱) 또는 다른 타사 앱의 다른 부분과에서 왔 위치 제안 사용을 설명 합니다.

## <a name="routing-apps-and-locations-suggestions"></a>라우팅 앱 및 위치 제안

이 섹션에는 라우팅 앱 내에서 직접 위치 제안 소비를 살펴보면을 소요 됩니다. 기존 개발자는 이용 하는이 기능을 추가 하려면 라우팅 응용 프로그램에 대 한 `MKDirectionsRequest` 다음과 같이 프레임 워크:

- 멀티태스킹의 응용 프로그램을 승격 합니다.
- 라우팅 응용 프로그램으로 응용 프로그램을 등록 합니다.
- 처리는 MapKit와 앱을 시작 하기 `MKDirectionsRequest` 개체입니다.
- WatchOS 사용자 참여에 따라 앱을 제안 하는 방법을 배울 수 있는 기능을 제공 합니다.

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

새로운 3 watchOS 앱 보낼 수 있습니다는 주소를 인코딩하는 데 개발자가 야기 하는 지리적 좌표 없는 주소:

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="summary"></a>요약

이 문서는 사전 제안 설명에 있으며 방법을 개발자에 사용할 수 Xamarin.iOS 앱에 대 한 드라이브 트래픽에 watchOS 보인 수 있습니다. 사전 제안 사항을 구현 하는 단계를 설명 하 고 사용 지침을 제공 합니다.


## <a name="related-links"></a>관련 링크

- [watchOS 샘플](https://developer.xamarin.com/samples/watchos/all/)
- [SiriKit 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
