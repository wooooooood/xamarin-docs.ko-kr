---
title: Xamarin.iOS에서 소모 성 제품 구매
description: 이 문서에서는 Xamarin.iOS에서 사용할 수 있는 제품을 설명 합니다. 소모 성 제품은 1 회용 조각 게임에서 통화 등의 기능입니다.
ms.prod: xamarin
ms.assetid: E0CB4A0F-C3FA-3933-58A7-13246971D677
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: b55465a700974e0ce5ceb8893d96311d920e04ae
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123650"
---
# <a name="purchasing-consumable-products-in-xamarinios"></a>Xamarin.iOS에서 소모 성 제품 구매

소모 성 제품은 'restore' 않아도 되므로 구현 하려면 매우 간단 합니다. 게임에 통화 또는 기능의 일회용 일부와 같은 제품에 대 한 유용합니다. 다시 사용자 소모 성 제품 over 조치를 다시 구매할 수 있습니다.

## <a name="built-in-product-delivery"></a>기본 제공 제품 제공

이 문서를 함께 제공 되는 샘플 코드는 기본 제공 제품을 보여 줍니다. – 기능을 지불 후 '해제' 코드에 밀접 하므로 제품 Id는 응용 프로그램에 하드 코딩 합니다. 구매 과정 같이 시각화할 수 있습니다.   
   
[![구매 프로세스 시각화](purchasing-consumable-products-images/image26.png)](purchasing-consumable-products-images/image26.png#lightbox)     
   
 기본 워크플로 다음과 같습니다.   
   
 1. 앱 추가 `SKPayment` 큐에 있습니다. 필요한 경우 사용자를 해당 Apple ID를 묻는 메시지가 표시 및 지불을 확인 하 라는 메시지가 표시 됩니다.   
   
 2. StoreKit 처리를 위해 서버에 요청을 보냅니다.   
   
 3. 트랜잭션이 완료 되 면 서버는 트랜잭션 수신 확인을 사용 하 여 응답 합니다.   
   
 4. `SKPaymentTransactionObserver` 확인 메일을 수신 하 고 처리 하는 하위 클래스입니다.   
   
 5. 응용 프로그램 제품을 사용 (업데이트 하 여 `NSUserDefaults` 또는 다른 메커니즘)를 StoreKit의 호출 및 `FinishTransaction`합니다.

다른 유형의 워크플로의 *Server-Delivered 제품* – 즉 문서 뒷부분에서 설명 (섹션을 참조 하세요 *수신 확인 및 Server-Delivered 제품*).

## <a name="consumable-products-example"></a>사용할 수 있는 제품 예제

합니다 [InAppPurchaseSample 코드](https://developer.xamarin.com/samples/monotouch/StoreKit/) 라는 프로젝트가 포함 되어 *소모품* 기본 '게임에서 통화' ("크레딧 조정" 이라고 함)를 구현 하는 합니다. 이 샘플에서는 "monkey 크레딧" – 원하는 대로 실제 응용 프로그램에도 것으로 소비 하는 방법이 구입할 수 있도록 두 가지 인 앱 구매 제품을 구현 하는 방법을 보여줍니다.   
   
   
   
 응용 프로그램은이 스크린 샷에 표시 된 – 각 구매 사용자의 잔액에 더 많은 "monkey 크레딧을"를 추가 합니다.   
   
   
   
 [![각 구매 사용자 잔액에 더 많은 monkey 크레딧을 추가합니다](purchasing-consumable-products-images/image27.png)](purchasing-consumable-products-images/image27.png#lightbox)   
   
   
   
 사용자 지정 클래스, StoreKit, 앱 스토어 간의 상호 작용은 다음과 같습니다.   
   
   
   
 [![사용자 지정 클래스, StoreKit, 앱 스토어 간의 상호 작용](purchasing-consumable-products-images/image28.png)](purchasing-consumable-products-images/image28.png#lightbox)

&nbsp;

### <a name="viewcontroller-methods"></a>ViewController 메서드

속성 및 제품 정보를 검색 하는 데 필요한 메서드를 실행 하는 것 외에도 뷰 컨트롤러는 구매와 관련 된 알림을 수신 하려면 추가 알림이 관찰자가 필요 합니다. 방금 이들은 `NSObjects` 하는 등록 되며 제거 `ViewWillAppear` 및 `ViewWillDisappear` 각각.

```csharp
NSObject succeededObserver, failedObserver;
```

생성자도 만들어집니다 합니다 `SKProductsRequestDelegate` 하위 클래스입니다 ( `InAppPurchaseManager`)를 다시 만들고 등록 합니다 `SKPaymentTransactionObserver` ( `CustomPaymentObserver`).   
   
   
   
 인 앱 구매 트랜잭션을 처리의 첫 번째 부분은 샘플 응용 프로그램에서 다음 코드 에서처럼 사용자 무언가 구입 하려는 경우 단추 누름을 처리 하려면:

```csharp
buy5Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy5ProductId);
};
buy10Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy10ProductId);
};
```

   
   
 사용자 인터페이스의 두 번째 부분은 알림을 처리 하는 트랜잭션이 성공 했는지,이 경우에 표시 된 잔액을 업데이트 하 여:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionSucceededNotification,
(notification) => {
   balanceLabel.Text = CreditManager.Balance() + " monkey credits";
});
```

마지막 부분은 사용자 인터페이스를 이유로 트랜잭션이 취소 될 경우 메시지를 표시 합니다. 예제 코드에서 메시지를 단순히 출력 창에 기록 됩니다.

```csharp
failedObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionFailedNotification,
(notification) => {
   Console.WriteLine ("Transaction Failed");
});
```

뷰 컨트롤러에서 이러한 방법 외에도 사용할 수 있는 제품 구입 거래가 해야 코드에는 `SKProductsRequestDelegate` 하며 `SKPaymentTransactionObserver`합니다.

### <a name="inapppurchasemanager-methods"></a>InAppPurchaseManager 메서드

InAppPurchaseManager 클래스에 대해 관련된 메서드를 구매 하는 많은 구현 하는 샘플 코드 포함 하 여는 `PurchaseProduct` 만드는 메서드를는 `SKPayment` 인스턴스 처리를 위해 큐에 추가 합니다.

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKPayment payment = SKPayment.PaymentWithProduct (appStoreProductId);
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

비동기 작업을 큐에 결제를 추가 합니다. 응용 프로그램을 다시 제어할 StoreKit 트랜잭션을 처리 하 고 Apple 서버에 보냅니다. 이 시점에서 이기는 iOS 사용자가 앱 스토어에 로그인 및 필요한 경우 그녀는 Apple ID와 암호에 대 한 프롬프트를 확인 합니다.   
   
   
   
 앱 스토어가 인증 하 고 트랜잭션을 동의 사용자를 성공적으로 가정 합니다 `SKPaymentTransactionObserver` StoreKit의 응답을 수신 되며 트랜잭션 처리를 완료 합니다. 다음 메서드를 호출 합니다.

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId);
   FinishTransaction(transaction, true);
}
```

알려주는 StoreKit 호출 하 여 트랜잭션이 성공적으로 수행 되었음을 확인 하는 마지막 단계입니다 `FinishTransaction`:

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

제품, 전달 되 면 `SKPaymentQueue.DefaultQueue.FinishTransaction` 트랜잭션이 결제 큐에서 제거할 호출 해야 합니다.

### <a name="skpaymenttransactionobserver-custompaymentobserver-methods"></a>(CustomPaymentObserver) SKPaymentTransactionObserver 메서드

StoreKit 호출을 `UpdatedTransactions` Apple 서버에서 응답을 수신 하 고 배열을 전달 하는 경우 메서드 `SKPaymentTransaction` 검사 하기 위해 코드에 대 한 개체입니다. 메서드를 각 트랜잭션을 통해 반복 하 고 다른 함수에 따라 트랜잭션 상태 (여기에 표시)를 수행 합니다.

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

합니다 `CompleteTransaction` 메서드는이 섹션에서 앞서 설명한 된 – 구매 세부 정보를 저장 `NSUserDefaults`StoreKit 사용 하 여 트랜잭션을 종료 하 고 마지막으로 업데이트 하려면 UI에 알립니다.

### <a name="purchasing-multiple-products"></a>여러 제품 구매

사용 하 여 응용 프로그램에서 여러 제품을 구매 하는 것이 좋습니다, 하는 경우는 `SKMutablePayment` 클래스 및 수량 필드를 설정 합니다.

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKMutablePayment payment = SKMutablePayment.PaymentWithProduct (appStoreProductId);
   payment.Quantity = 4; // hardcoded as an example
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

완료 된 트랜잭션을 처리 하는 코드가 구매를 올바르게 충족 수량 속성을 쿼리할 수도 해야 합니다.

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

사용자가 여러를 구입한 경우 StoreKit 확인 경고 반영 됩니다 수량, 단가 및 부과 수 것 총 가격은 다음 스크린샷에 표시 된 대로:

[![구매를 확인합니다.](purchasing-consumable-products-images/image30.png)](purchasing-consumable-products-images/image30.png#lightbox)

## <a name="handling-network-outages"></a>처리 네트워크 중단

앱에서 바로 구매 StoreKit Apple 서버와 통신할 수에 대 한 네트워크 연결이 필요 합니다. 네트워크 연결을 사용할 수 없는 경우 다음 앱에서 바로 구매 사용할 수 없게 됩니다.

### <a name="product-requests"></a>제품 요청

네트워크를 수행 하는 동안 사용할 수 없는 경우는 `SKProductRequest`는 `RequestFailed` 메서드는 `SKProductsRequestDelegate` 하위 클래스입니다 ( `InAppPurchaseManager`) 아래와 같이 호출 될:

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

그런 다음 ViewController 알림에 대 한 수신 대기 하 고 구매 단추에서 메시지가 표시 됩니다.

```csharp
requestObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerRequestFailedNotification,
(notification) => {
   Console.WriteLine ("Request Failed");
   buy5Button.SetTitle ("Network down?", UIControlState.Disabled);
   buy10Button.SetTitle ("Network down?", UIControlState.Disabled);
});
```

네트워크 연결을 일 수 있으므로 모바일 장치에서 임시 응용 프로그램 SystemConfiguration 프레임 워크를 사용 하 여 네트워크 상태를 모니터링 하려고 하 고 네트워크 연결을 사용할 수 있는 경우 다시 시도 수 있습니다. Apple의를 참조 하거나 사용 하는 합니다.

### <a name="purchase-transactions"></a>트랜잭션 구매

StoreKit 결제 큐 저장 및 정방향 구매 요청 가능 하면 하므로 네트워크 구매 프로세스 중 실패 한 경우 네트워크 중단의 영향에 따라 달라 집니다.   
   
   
   
 트랜잭션 도중 오류가 발생 하는 경우는 `SKPaymentTransactionObserver` 하위 클래스입니다 ( `CustomPaymentObserver`) 해야 합니다 `UpdatedTransactions` 메서드 호출 및 `SKPaymentTransaction` 클래스 실패 상태가 됩니다.

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

`FailedTransaction` 메서드 오류가 사용자 취소 발생 여부를 다음과 같이 검색:

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

트랜잭션이 실패 하는 경우에는 `FinishTransaction` 트랜잭션이 결제 큐에서 제거할 메서드를 호출 해야 합니다.

```csharp
SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);
```

예제 코드에서는 알림을 보냅니다 ViewController 메시지를 표시할 수 있도록 합니다. 응용 프로그램 추가 표시 하지 않도록 사용자가 트랜잭션을 취소 하는 경우. 발생할 수 있는 다른 오류 코드는 다음과 같습니다.

```csharp
FailedTransaction Code=0 Cannot connect to iTunes Store
FailedTransaction Code=5002 An unknown error has occurred
FailedTransaction Code=5020 Forget Your Password?
Applications may detect and respond to specific error codes, or handle them in the same way.
```

## <a name="handling-restrictions"></a>처리 제한

합니다 **설정 > 일반 > 제한** 기능의 iOS 장치의 특정 기능을 잠글 수 있습니다.   
   
   
   
 사용자를 통해 앱 내 구매를 허용 되는지 여부를 쿼리할 수 있습니다는 `SKPaymentQueue.CanMakePayments` 메서드. False 반환 인 앱 구매를 액세스할 수 없습니다. StoreKit 구매를 시도 하는 경우 사용자에 게 오류 메시지가 표시 자동으로 됩니다. 이 값을 확인 하 여 응용 프로그램 수 대신 구매 단추를 숨기 거 나 사용자를 다른 동작을 수행 합니다.   
   
   
   
 에 `InAppPurchaseManager.cs` 파일을 `CanMakePayments` 메서드 같이 StoreKit 함수를 래핑합니다.

```csharp
public bool CanMakePayments()
{
   return SKPaymentQueue.CanMakePayments;
}
```

이 메서드를 테스트 하려면 사용 합니다 **제한을** 사용 하지 않도록 설정 하는 iOS의 기능 **앱 내 구매**:   
   
   
   
 [![IOS의 제한 기능을 사용 하 여 앱 내 구매를 사용 하지 않도록 설정](purchasing-consumable-products-images/image31.png)](purchasing-consumable-products-images/image31.png#lightbox)   
   
   
   
 이 예제 코드 `ConsumableViewController` 에 반응 `CanMakePayments` 표시 하 여 false를 반환 **AppStore 비활성화** 비활성화 된 단추가 텍스트입니다.

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

이 경우 같은 응용 프로그램에서는 합니다 **앱에서 바로 구매** 기능은 제한 – 구매 단추가 비활성화 됩니다.   
   
   
   
 [![응용 프로그램이 다음과 같은 기능은 앱 내 구매 단추가 비활성화 되는 구매를 제한 하는 경우](purchasing-consumable-products-images/image32.png)](purchasing-consumable-products-images/image32.png#lightbox)   
   
   
   

제품 정보 될 때 요청 된 `CanMakePayments` 이 false 인 경우 앱 수 계속 검색 하 고 가격을 표시할 수 있도록 합니다. 즉, 제거 하는 경우는 `CanMakePayments` 구매를 시도 하는 사용자는 메시지가 표시 됩니다 있지만 확인 구매 단추는 계속 해 서 코드에서 활성화 될입니다 **앱에서 바로 구매 허용 되지 않습니다** (StoreKit에서 생성 합니다. 결제 큐 액세스할 때).   
   
   
   
 [![앱에서 바로 구매 허용 되지 않습니다.](purchasing-consumable-products-images/image33.png)](purchasing-consumable-products-images/image33.png#lightbox)   
   
   
   
 실제 응용 프로그램 단추를 완전히 숨기 아마도 StoreKit를 자동으로 보여 주는 경고 보다 자세한 메시지의 제공 등 제한은 처리는 다른 방법을 걸릴 수 있습니다.

