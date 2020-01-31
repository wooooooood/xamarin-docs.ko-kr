---
title: Xamarin.ios에서 소비재 제품 구매
description: 이 문서에서는 Xamarin.ios의 소비재 제품에 대해 설명 합니다. 사용 가능한 제품은 게임 내 통화와 같은 단일 사용 기능입니다.
ms.prod: xamarin
ms.assetid: E0CB4A0F-C3FA-3933-58A7-13246971D677
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: fb8cd050c789e165c1774398a3a2cc8e0467bde1
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75489026"
---
# <a name="purchasing-consumable-products-in-xamarinios"></a>Xamarin.ios에서 소비재 제품 구매

' 복원 ' 요구 사항이 없기 때문에 사용할 수 있는 제품을 구현 하는 것이 가장 간단 합니다. 게임 내 통화 또는 단일 사용 기능 등의 제품에 유용 합니다. 사용자는 사용할 수 있는 제품을 다시 구매할 수 있습니다.

## <a name="built-in-product-delivery"></a>기본 제공 제품 제공

이 문서와 함께 제공 되는 샘플 코드는 기본 제공 제품을 보여 줍니다. 제품 Id는 지불 후 기능을 ' 잠금 해제 ' 하는 코드와 긴밀 하 게 결합 되므로 응용 프로그램에 하드 코딩 됩니다. 구매 프로세스는 다음과 같이 시각화할 수 있습니다.   
   
[![구매 프로세스 시각화](purchasing-consumable-products-images/image26.png)](purchasing-consumable-products-images/image26.png#lightbox)     
   
 기본 워크플로는 다음과 같습니다.   
   
 1. 앱이 큐에 `SKPayment`를 추가 합니다. 필요한 경우 사용자에 게 Apple ID를 입력 하 라는 메시지가 표시 되 고 지불을 확인 하 라는 메시지가 표시 됩니다.   
   
 2. 서버를 처리 하기 위해 서버에 요청을 보냅니다.   
   
 3. 트랜잭션이 완료 되 면 서버는 트랜잭션 수신으로 응답 합니다.   
   
 4. `SKPaymentTransactionObserver` 서브 클래스는 수신을 수신 하 고 처리 합니다.   
   
 5. 응용 프로그램은 `NSUserDefaults` 또는 다른 메커니즘을 업데이트 하 여 제품을 사용 하도록 설정 하 고,이를 위해 기능 `FinishTransaction`를 호출 합니다.

*서버에서 제공* 하는 제품의 다른 유형이 있습니다 .이는 문서 뒷부분에 설명 되어 있습니다 ( *수신 확인 및 서버에서 제공*하는 제품 섹션 참조).

## <a name="consumable-products-example"></a>사용할 때 제품 예

[InAppPurchaseSample 코드](https://docs.microsoft.com/samples/xamarin/ios-samples/storekit) 에는 기본 ' 게임 내 통화 ' ("원숭이 크레딧" 이라고 함)를 구현 하는 *소모품* 이라는 프로젝트가 포함 되어 있습니다. 이 샘플에서는 사용자가 원하는 수 만큼 "원숭이 크레딧"을 구입할 수 있도록 두 개의 앱 내 구매 제품을 구현 하는 방법을 보여 줍니다. 실제 응용 프로그램에서는 이러한 기능을 사용 하는 방법도 있습니다.   

응용 프로그램은 이러한 스크린샷에 표시 됩니다. 각 구매는 사용자 잔액에 "원숭이 크레딧"을 추가 합니다.   

 [![각 구매 사용자 잔액에 더 많은 원숭이 크레딧을 추가 합니다.](purchasing-consumable-products-images/image27.png)](purchasing-consumable-products-images/image27.png#lightbox)   

사용자 지정 클래스, 사용자 키트 키트 및 앱 스토어 간의 상호 작용은 다음과 같습니다.   

 [![사용자 지정 클래스, 사용자 키트 키트 및 앱 스토어 간의 상호 작용](purchasing-consumable-products-images/image28.png)](purchasing-consumable-products-images/image28.png#lightbox)

### <a name="viewcontroller-methods"></a>ViewController 메서드

제품 정보를 검색 하는 데 필요한 속성 및 메서드 외에도 뷰 컨트롤러는 구매 관련 알림을 수신 하기 위해 추가 알림 관찰자가 필요 합니다. `ViewWillAppear` 및 `ViewWillDisappear`에서 각각 등록 및 제거 되는 `NSObjects`입니다.

```csharp
NSObject succeededObserver, failedObserver;
```

생성자는 또한 `CustomPaymentObserver`(`SKPaymentTransactionObserver`)를 만들고 등록 하는 `SKProductsRequestDelegate` 하위 클래스 (`InAppPurchaseManager`)를 만듭니다.   

앱 내 구매 트랜잭션을 처리 하는 첫 번째 단계는 샘플 응용 프로그램의 다음 코드와 같이 사용자가 무언가를 구매 하려는 경우 단추 누르기를 처리 하는 것입니다.

```csharp
buy5Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy5ProductId);
};
buy10Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy10ProductId);
};
```

사용자 인터페이스의 두 번째 부분에서는 트랜잭션이 성공 했다는 알림을 처리 합니다 .이 경우에는 표시 된 잔액을 업데이트 합니다.

```csharp
succeededObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionSucceededNotification,
(notification) => {
   balanceLabel.Text = CreditManager.Balance() + " monkey credits";
});
```

일부 이유로 트랜잭션이 취소 되는 경우 사용자 인터페이스의 마지막 부분에서 메시지를 표시 합니다. 예제 코드에서 메시지는 단순히 출력 창에 기록 됩니다.

```csharp
failedObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionFailedNotification,
(notification) => {
   Console.WriteLine ("Transaction Failed");
});
```

뷰 컨트롤러의 이러한 메서드 외에도 제품 구매 거래에는 `SKProductsRequestDelegate` 및 `SKPaymentTransactionObserver`에 대 한 코드가 필요 합니다.

### <a name="inapppurchasemanager-methods"></a>InAppPurchaseManager 메서드

이 샘플 코드는 `SKPayment` 인스턴스를 만들고 처리를 위해 큐에 추가 하는 `PurchaseProduct` 메서드를 포함 하 여 InAppPurchaseManager 클래스에서 많은 구매 관련 메서드를 구현 합니다.

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKPayment payment = SKPayment.PaymentWithProduct (appStoreProductId);
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

큐에 지불을 추가 하는 작업은 비동기 작업입니다. 응용 프로그램은 트랜잭션을 처리 하 고 Apple의 서버에 전송 하는 동안 제어를 다시 가집니다. 이 시점에서 iOS는 사용자가 앱 스토어에 로그인 했는지 확인 하 고 필요한 경우 Apple ID 및 암호를 입력 하 라는 메시지를 표시 합니다.   

사용자가 앱 스토어를 사용 하 여 성공적으로 인증 하 고 트랜잭션에 동의한 것으로 가정 하면 `SKPaymentTransactionObserver`는 사용자 키트의 응답을 수신 하 고 다음 메서드를 호출 하 여 트랜잭션을 수행 하 고 종료 합니다.

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId);
   FinishTransaction(transaction, true);
}
```

마지막 단계에서는 `FinishTransaction`를 호출 하 여 트랜잭션을 성공적으로 완료 했음을 사용자에 게 알립니다.

```csharp
public void FinishTransaction(SKPaymentTransaction transaction, bool wasSuccessful)
{
   // remove the transaction from the payment queue.
   SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);  // THIS IS IMPORTANT - LET'S APPLE KNOW WE'RE DONE !!!!
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {transaction},new NSObject[] {new NSString("transaction")});
       if (wasSuccessful) {
           // send out a notification that we've finished the transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionSucceededNotification, this, userInfo);
       } else {
           // send out a notification for the failed transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionFailedNotification, this, userInfo);
       }
   }
}
```

제품이 배달 되 면 지불 큐에서 트랜잭션을 제거 하기 위해 `SKPaymentQueue.DefaultQueue.FinishTransaction`를 호출 해야 합니다.

### <a name="skpaymenttransactionobserver-custompaymentobserver-methods"></a>SKPaymentTransactionObserver (CustomPaymentObserver) 메서드

파일 키트는 Apple 서버에서 응답을 받을 때 `UpdatedTransactions` 메서드를 호출 하 고 코드에서 검사할 `SKPaymentTransaction` 개체 배열을 전달 합니다. 메서드는 각 트랜잭션을 반복 하 고 다음과 같이 트랜잭션 상태에 따라 다른 기능을 수행 합니다.

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
   foreach (SKPaymentTransaction transaction in transactions)
   {
       switch (transaction.TransactionState)
       {
           case SKPaymentTransactionState.Purchased:
              theManager.CompleteTransaction(transaction);
               break;
           case SKPaymentTransactionState.Failed:
              theManager.FailedTransaction(transaction);
               break;
           default:
               break;
       }
   }
}
```

`CompleteTransaction` 방법은이 섹션의 앞부분에서 설명 했습니다. 구매 세부 정보를 `NSUserDefaults`에 저장 하 고, 저장 키트를 사용 하 여 트랜잭션을 종료 하 고 마지막으로 UI에 업데이트를 알립니다.

### <a name="purchasing-multiple-products"></a>여러 제품 구매

응용 프로그램에서 여러 제품을 구매 하는 것이 적합 한 경우 `SKMutablePayment` 클래스를 사용 하 여 수량 필드를 설정 합니다.

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKMutablePayment payment = SKMutablePayment.PaymentWithProduct (appStoreProductId);
   payment.Quantity = 4; // hardcoded as an example
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

또한 완료 된 트랜잭션을 처리 하는 코드는 Quantity 속성을 쿼리하여 구매를 올바르게 수행 해야 합니다.

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   var qty = transaction.Payment.Quantity;
   if (productId == ConsumableViewController.Buy5ProductId)
       CreditManager.Add(5 * qty);
   else if (productId == ConsumableViewController.Buy10ProductId)
       CreditManager.Add(10 * qty);
   else
       Console.WriteLine ("Shouldn't happen, there are only two products");
   FinishTransaction(transaction, true);
}
```

사용자가 여러 수량을 구매 하면 다음 스크린샷에 표시 된 것 처럼, 수량, 단가 및 총 가격이 청구 됩니다.

[![구매를 확인 하는](purchasing-consumable-products-images/image30.png)](purchasing-consumable-products-images/image30.png#lightbox)

## <a name="handling-network-outages"></a>네트워크 중단 처리

앱에서 바로 구매 기능을 사용 하려면 연동 키트에 대해 Apple의 서버와 통신 하기 위해 작동 하는 네트워크 연결이 필요 합니다. 네트워크 연결을 사용할 수 없는 경우 앱 내 구매를 사용할 수 없습니다.

### <a name="product-requests"></a>제품 요청

`SKProductRequest`하는 동안 네트워크를 사용할 수 없는 경우 아래와 같이 `SKProductsRequestDelegate` 하위 클래스 (`InAppPurchaseManager`)의 `RequestFailed` 메서드가 호출 됩니다.

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {error},new NSObject[] {new NSString("error")});
       // send out a notification for the failed transaction
       NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerRequestFailedNotification, this, userInfo);
   }
}
```

그러면 ViewController는 알림을 수신 하 고 구매 단추에 메시지를 표시 합니다.

```csharp
requestObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerRequestFailedNotification,
(notification) => {
   Console.WriteLine ("Request Failed");
   buy5Button.SetTitle ("Network down?", UIControlState.Disabled);
   buy10Button.SetTitle ("Network down?", UIControlState.Disabled);
});
```

네트워크 연결은 모바일 장치에서 일시적일 수 있으므로 응용 프로그램은 SystemConfiguration 프레임 워크를 사용 하 여 네트워크 상태를 모니터링 하 고, 네트워크 연결을 사용할 수 있는 경우 다시 시도할 수 있습니다. Apple의 또는이를 사용 하는를 참조 하세요.

### <a name="purchase-transactions"></a>트랜잭션 구매

저장소 키트 지불 큐는 가능 하면 구매 요청을 저장 하 고 전달 하므로 네트워크 중단의 영향은 구매 프로세스 중에 네트워크에 오류가 발생 한 시기에 따라 달라 집니다.   

트랜잭션을 수행 하는 동안 오류가 발생 하는 경우 `SKPaymentTransactionObserver` 서브 클래스 (`CustomPaymentObserver`)는 `UpdatedTransactions` 메서드를 호출 하 고 `SKPaymentTransaction` 클래스는 실패 상태에 있습니다.

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
   foreach (SKPaymentTransaction transaction in transactions)
   {
       switch (transaction.TransactionState)
       {
           case SKPaymentTransactionState.Purchased:
               theManager.CompleteTransaction(transaction);
               break;
           case SKPaymentTransactionState.Failed:
               theManager.FailedTransaction(transaction);
               break;
           default:
               break;
       }
   }
}
```

`FailedTransaction` 메서드는 다음과 같이 오류가 사용자 취소로 인 한 것인지 여부를 검색 합니다.

```csharp
public void FailedTransaction (SKPaymentTransaction transaction)
{
   //SKErrorPaymentCancelled == 2
   if (transaction.Error.Code == 2) // user cancelled
       Console.WriteLine("User CANCELLED FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   else // error!
       Console.WriteLine("FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   FinishTransaction(transaction,false);
}
```

트랜잭션이 실패 하더라도 지불 큐에서 트랜잭션을 제거 하려면 `FinishTransaction` 메서드를 호출 해야 합니다.

```csharp
SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);
```

그런 다음 예제 코드는 ViewController가 메시지를 표시할 수 있도록 알림을 보냅니다. 사용자가 트랜잭션을 취소 한 경우 응용 프로그램에서 추가 메시지를 표시 해서는 안 됩니다. 발생할 수 있는 다른 오류 코드는 다음과 같습니다.

```csharp
FailedTransaction Code=0 Cannot connect to iTunes Store
FailedTransaction Code=5002 An unknown error has occurred
FailedTransaction Code=5020 Forget Your Password?
Applications may detect and respond to specific error codes, or handle them in the same way.
```

## <a name="handling-restrictions"></a>처리 제한

IOS의 **일반 > 제한 기능 > 설정** 에 따라 사용자는 장치의 특정 기능을 잠글 수 있습니다.   

사용자가 `SKPaymentQueue.CanMakePayments` 메서드를 통해 앱에서 바로 구매를 수행할 수 있는지 여부를 쿼리할 수 있습니다. False를 반환 하는 경우 사용자는 앱 내 구매에 액세스할 수 없습니다. 사용자를 구매 하려고 하면 자동으로 사용자에 게 오류 메시지가 표시 됩니다. 이 값을 선택 하면 응용 프로그램에서 대신 구매 단추를 숨기 거 나 사용자에 게 도움을 주는 다른 작업을 수행할 수 있습니다.   

`InAppPurchaseManager.cs` 파일에서 `CanMakePayments` 메서드는 다음과 같이 파일 형식 키트 함수를 래핑합니다.

```csharp
public bool CanMakePayments()
{
   return SKPaymentQueue.CanMakePayments;
}
```

이 방법을 테스트 하려면 iOS의 **제한** 기능을 사용 하 여 **앱에서 바로 구매**를 사용 하지 않도록 설정 합니다.   

 [![iOS의 제한 기능을 사용 하 여 앱에서 바로 구매를 사용 하지 않도록 설정](purchasing-consumable-products-images/image31.png)](purchasing-consumable-products-images/image31.png#lightbox)   

`ConsumableViewController`의이 예제 코드는 사용 안 함 단추에 **Appstore 사용 안 함** 텍스트를 표시 하 여 false를 반환 `CanMakePayments`에 반응 합니다.

```csharp
// only if we can make payments, request the prices
if (iap.CanMakePayments()) {
   // now go get prices, if we don't have them already
   if (!pricesLoaded)
       iap.RequestProductData(products); // async request via StoreKit -> App Store
} else {
   // can't make payments (purchases turned off in Settings?)
   // the buttons are disabled by default, and only enabled when prices are retrieved
   buy5Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
   buy10Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
}
```

**앱에서 바로 구매** 기능이 제한 되는 경우 응용 프로그램은 다음과 같이 표시 됩니다. 구매 단추를 사용할 수 없습니다.   

 [![앱 내 구매 기능이 제한 되는 경우 응용 프로그램은 다음과 같이 표시 됩니다. 구매 단추를 사용할 수 없습니다.](purchasing-consumable-products-images/image32.png)](purchasing-consumable-products-images/image32.png#lightbox)   

`CanMakePayments` false 인 경우에도 제품 정보를 요청할 수 있으므로 앱에서 가격을 검색 하 고 표시할 수 있습니다. 즉, 코드에서 `CanMakePayments` 확인을 제거 했을 때 구매 단추는 계속 활성화 되어 있지만 구매가 시도 될 때 사용자에 게 **앱에서의 구매가 허용 되지** 않는다는 메시지가 표시 됩니다 (지불 큐에 액세스 하는 경우에는 사용자 키트 키트에 의해 생성 됨).   

 [![앱에서의 구매는 허용 되지 않습니다.](purchasing-consumable-products-images/image33.png)](purchasing-consumable-products-images/image33.png#lightbox)   

실제 응용 프로그램에서는 단추를 모두 숨기는 것과 같은 다른 방법을 사용 하 여 기능을 자동으로 표시 하는 경고 보다 더 자세한 메시지를 제공할 수 있습니다.
