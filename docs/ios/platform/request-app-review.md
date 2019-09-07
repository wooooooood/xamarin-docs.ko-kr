---
title: Xamarin.ios에서 앱 검토 요청
description: 이 문서에서는 Apple에서 iOS 10에 추가 하는 RequestReview 방법에 대해 설명 하 고 Xamarin.ios에서이를 구현 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 6408e707-b7dc-4557-b931-16a4d79b8930
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: f8dc533d1428d622d9b01b8b8dd879653f78fd56
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769523"
---
# <a name="request-app-review-in-xamarinios"></a>Xamarin.ios에서 앱 검토 요청

_이 문서에서는 Apple에서 iOS 10에 추가 하는 RequestReview 방법과 Xamarin.ios에서이를 구현 하는 방법에 대해 설명 합니다._

Ios 10.3의 새로운 기능으로 `RequestReview()` ,이 방법을 사용 하면 ios 앱에서 사용자에 게 평가 또는 검토를 요청할 수 있습니다. 사용자가 앱 스토어에서 설치한 배송 앱에서이 메서드를 호출 하면 iOS 10은 개발자에 대 한 전체 등급 및 검토 프로세스를 처리 합니다. 이 프로세스는 앱 스토어 정책에 의해 제어 되므로 경고가 표시 될 수도 있고 표시 되지 않을 수도 있습니다.

![](request-app-review-images/review01.png "샘플 검토 요청 경고")

## <a name="requesting-a-rating-or-review"></a>등급 또는 리뷰 요청

사용자 환경에 적합 한 위치 `SKStoreReviewController` 에서 클래스의 정적메서드를호출할수있지만,검토프로세스는앱스토어정책에의해제어되고처리됩니다.`RequestReview()` 결과적으로이 메서드는 경고를 표시 하거나 표시 하지 않을 수 있으며 단추 누르기와 같은 사용자 동작에 대 한 응답으로 호출 되 면 안 됩니다.

예를 들어 앱은 지정 된 횟수 만큼 시작 된 후에 검토를 요청 하거나, 플레이어가 수준을 완료 한 후에 검토를 요청할 수 있습니다.

Xamarin.ios 앱이 시작 되는 즉시 검토를 요청 하려면 `AppDelegate.cs` 파일을 다음과 같이 변경 합니다.

```csharp
using Foundation;
using StoreKit;
using UIKit;

namespace iOSTenThree
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        ...

        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Request a review from the user
            SKStoreReviewController.RequestReview ();

            return true;
        }

        ...

    }
}
```

> [!NOTE]
> 개발 `RequestReview()` 중인 앱에서를 호출 하면 항상 등급 및 검토 대화 상자가 표시 되므로 테스트를 수행할 수 있습니다. 이는 TestFlight를 통해 배포 된 앱에는 적용 되지 않습니다 .이 경우 메서드 호출은 무시 됩니다.

사용자가 앱 스토어에서 설치한 배송 앱에서 메서드를호출하면iOS10은개발자에대한전체등급및검토프로세스를처리합니다.`RequestReview()` 이 프로세스는 앱 스토어 정책에 의해 제어 되므로 경고가 표시 되거나 표시 되지 않을 수 있습니다.

## <a name="linking-to-an-app-store-product-page"></a>앱 스토어 제품 페이지에 연결 

새 `RequestReview` 방법 외에도 개발자는 앱 내에서 앱 스토어의 앱 제품 페이지에 대 한 심층 링크를 제공할 수 있습니다. 제품 페이지 `action=write-review` URL의 끝에를 추가 하면 사용자가 앱의 검토를 자동으로 작성할 수 있는 페이지가 열립니다. 

## <a name="summary"></a>요약

이 문서에서는 Apple에서 iOS 10에 추가 하 고 Xamarin.ios에서 구현 하는 방법에 대 한 RequestReview 방법에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [iOSTenThree 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-iostenthree/)
