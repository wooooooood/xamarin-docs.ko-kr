---
title: "응용 프로그램 검토를 요청"
description: "이 문서는 Apple iOS 10 및 Xamarin.iOS에서 구현 하는 방법에 추가 RequestReview 방법에 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6408e707-b7dc-4557-b931-16a4d79b8930
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 2e63f2c47bbcd6da0f0d5370ebfc231d19a10e7d
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="request-app-review"></a>응용 프로그램 검토를 요청

_이 문서는 Apple iOS 10 및 Xamarin.iOS에서 구현 하는 방법에 추가 RequestReview 방법에 설명 합니다._

새 iOS 10.3에 `RequestReview()` 메서드를 사용 하면 iOS 앱을 평가 하거나 검토 하려면 사용자에 게 요청 합니다. 사용자가 앱 스토어에서 설치 된 배송 응용 프로그램에서이 메서드는, 10 iOS 전체 등급 처리 되 고 개발자에 대 한 검토 프로세스 됩니다. 이 프로세스는 앱 스토어 정책의 영향을 받는, 때문에 경고 수도 표시 될 수 있습니다.

![](request-app-review-images/review01.png "예제 검토 요청 알림")

## <a name="requesting-a-rating-or-review"></a>등급 또는 검토를 요청합니다.

동안는 `RequestReview()` 의 정적 메서드는 `SKStoreReviewController` 클래스를 호출할 수 있습니다를 언제 든 지 것이 적합 사용자 환경에는 검토 프로세스는 제한 되며 사용자 앱 스토어 정책에 의해 처리 됩니다. 결과적으로,이 메서드 수 있습니다 또는 경고 메시지가 표시 되지 않을 수 있습니다 및 단추를 누르는 것과 같이 사용자 작업에 대 한 응답으로 호출 되 면 안 합니다.

예를 들어 응용 프로그램 검토 한 후 지정 된 횟수를 시작 하거나 플레이어 수준을 완료 된 후 게임 검토를 요청할 수 있습니다를 요청할 수 있습니다.

요청에는 Xamarin.iOS 앱 시작을 완료 하는 즉시 검토를 다음과 같이 변경 하는 `AppDelegate.cs` 파일:

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
> 호출 `RequestReview()` 아래 개발에서 앱 항상 표시 등급 되며 테스트할 수 있도록 대화를 검토 합니다. 이이 메서드는 무시 됩니다 TestFlight를 통해 배포 된 앱에 적용 되지 않습니다.

경우는 `RequestReview()` 메서드는 사용자가 앱 스토어에서 설치 된 배송 응용 프로그램에서, 10 iOS 개발자 용 전체 등급 및 검토 프로세스를 처리 합니다. 다시이 프로세스는 앱 스토어 정책의 영향을 받는, 때문에 경고 수도 표시 될 수 있습니다.

## <a name="linking-to-an-app-store-product-page"></a>앱 스토어 제품 페이지에 연결 

새 외에도 `RequestReview` 메서드에서, 개발자 응용 프로그램 내에서 앱 스토어에 응용 프로그램의 제품 페이지를 딥 링크를 제공할 여전히 수 있습니다. 추가 하 여 `action=write-review` 제품 페이지 URL의 끝에 페이지를 열어야 사용자 자동으로 응용 프로그램의 검토를 쓸 수 있습니다. 

## <a name="summary"></a>요약

이 문서 검사가 Xamarin.iOS에서 구현 하는 방법과 iOS 10에 추가 하는 Apple RequestReview 메서드 수행 합니다.



## <a name="related-links"></a>관련 링크

- [iOSTenThree 샘플](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
