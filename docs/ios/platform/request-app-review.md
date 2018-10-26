---
title: Xamarin.iOS에서 앱 검토 요청
description: 이 문서는 iOS 10에 추가 하는 Apple RequestReview 메서드를 설명 하 고 Xamarin.iOS에서 구현 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 6408e707-b7dc-4557-b931-16a4d79b8930
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/29/2017
ms.openlocfilehash: f72aaa781b0712e206cf02725cfc434594287f41
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109785"
---
# <a name="request-app-review-in-xamarinios"></a>Xamarin.iOS에서 앱 검토 요청

_이 문서에서는 iOS 10과 Xamarin.iOS에서 구현 하는 방법에 추가 하는 Apple RequestReview 메서드를 설명 합니다._

Ios 10.3, 새를 `RequestReview()` 메서드를 사용 하면 iOS 앱이 사용자 속도 또는 검토 하도록 요청 합니다. 이 메서드는 사용자가 앱 스토어에서 설치 하는 배송 앱에서 호출 되 면 iOS 10 전체 등급 처리 개발자를 위한 프로세스를 검토 합니다. 이 프로세스는 앱 스토어 정책의 영향을 받는, 때문에 경고 수도 표시 될 수 있습니다.

![](request-app-review-images/review01.png "샘플 요청 검토 경고")

## <a name="requesting-a-rating-or-review"></a>평가 또는 리뷰를 요청합니다.

하는 동안 합니다 `RequestReview()` 의 정적 메서드를 `SKStoreReviewController` 클래스를 호출할 수 있습니다 언제 든 지는 것이 합리적 사용자 환경에 검토 프로세스는 제어 및 앱 스토어 정책에 의해 처리 됩니다. 결과적으로,이 될 수 있습니다 또는 경고가 표시 되지 메서드와 단추를 탭 하는 같은 사용자 작업에 대 한 응답으로 호출 되 면 안 합니다.

예를 들어, 앱을 검토 한 후 지정된 된 횟수 만큼 시작 또는 게임을 플레이어가 수준 완료 된 후 검토를 요청할 수 있습니다를 요청할 수 있습니다.

요청 검토를 실행 하 고, Xamarin.iOS 앱이 완료 되는 즉시 하려면 다음과 같이 변경 합니다 `AppDelegate.cs` 파일:

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
> 호출 `RequestReview()` 는 언더 개발 앱은 항상 등급을 표시 하 고 테스트할 수 있도록 대화를 검토 합니다. 이 메서드 호출을 무시할 TestFlight를 통해 배포 된 앱에 적용 되지 않습니다.

경우는 `RequestReview()` 메서드는 사용자가 앱 스토어에서 설치 하는 배송 앱, iOS 10는 개발자를 위한 전체 등급 및 검토 프로세스를 처리 합니다. 또한이 프로세스는 앱 스토어 정책의 영향을 받는, 때문에 경고 수 나 표시 될 수 있습니다.

## <a name="linking-to-an-app-store-product-page"></a>앱 스토어 제품 페이지에 연결 

새 외에도 `RequestReview` 메서드를 개발자 제품 페이지 앱 내에서 앱 스토어에 앱의 딥 링크를 제공할 여전히 수 있습니다. 추가 하 여 `action=write-review` 제품 페이지 URL 끝에 페이지 열립니다 사용자 자동으로 앱의 검토를 작성할 수 있습니다. 

## <a name="summary"></a>요약

이 문서에서는 iOS 10과 Xamarin.iOS에서 구현 하는 방법에 추가 하는 Apple RequestReview 메서드를 설명 했습니다.



## <a name="related-links"></a>관련 링크

- [iOSTenThree 샘플](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
