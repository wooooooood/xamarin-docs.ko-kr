---
title: "사용 될 제품을 구입"
ms.topic: article
ms.prod: xamarin
ms.assetid: E0CB4A0F-C3FA-3933-58A7-13246971D677
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 7366a4ce5cbb6a3026a7445a03f03b45d89d9210
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="purchasing-consumable-products"></a>사용 될 제품을 구입

사용 될 제품은 'restore' 않아도 이므로 구현 하기 가장 간단한입니다. 게임 내에서 통화 또는 일회용 궁극적으로 같은 제품에 대 한 유용합니다. 다시 사용자가 사용 될 제품 over 조치를 다시 구매할 수 있습니다.

## <a name="built-in-product-delivery"></a>기본 제공 제품 배달

이 문서와 함께 제공 되는 샘플 코드는 기본 제공 된 제품을 보여 줍니다.-'후 잠금을 해제' 기능 결제 하는 코드에 밀접 하 게 결합 된 때문에 제품 Id는 응용 프로그램에 하드 코딩 합니다. 구매 프로세스는 다음과 같은 시각화할 수 있습니다.   
   
[ ![구매 프로세스 시각화](purchasing-consumable-products-images/image26.png)](purchasing-consumable-products-images/image26.png)     
   
 기본 워크플로.   
   
   
   
 1. 앱 추가 `SKPayment` 큐에 있습니다. 사용자의 Apple ID를 묻는 메시지가 표시 되며 지불 확인 하도록 요청 필요한 경우.   
   
 2. StoreKit 처리를 위해 서버에 요청을 보냅니다.   
   
 3. 트랜잭션이 완료 되 면 서버 트랜잭션 수신 확인을 사용 하 여 응답 합니다.   
   
 4. `SKPaymentTransactionObserver` 하위 클래스 확인 메일을 받아 처리 합니다.   
   
 5. 응용 프로그램 제품을 사용 (업데이트 하 여 `NSUserDefaults` 나 일부 다른 메커니즘), StoreKit의 다음 호출 `FinishTransaction`합니다.

다른 유형의 워크플로 – *Server-Delivered 제품* – 즉이 문서 뒷부분에 설명 된 (섹션을 참조 *수신 확인 및 Server-Delivered 제품*).

## <a name="consumable-products-example"></a>사용 될 제품 예제

[InAppPurchaseSample 코드](https://developer.xamarin.com/samples/monotouch/StoreKit/) 이라는 프로젝트가 들어 *소모품* basic '게임 내에서 통화' ("크레딧 조정" 이라고 함)를 구현 하는입니다. 샘플에서는 사용자가 많은 "원숭이 크레딧"-원하는 대로 실제 응용 프로그램에도 것으로 소비 하는 방법이 구매를 허용 하려면 두 앱에서 바로 구매 제품을 구현 하는 방법을 보여 줍니다!   
   
   
   
 응용 프로그램은이 스크린 샷에 표시 된 – 사용자의 잔액에 더 많은 "원숭이 크레딧"를 추가 하는 각 구매:   
   
   
   
 [ ![사용자가 잔액에 더 많은 원숭이 크레딧을 추가 하는 각 구매](purchasing-consumable-products-images/image27.png)](purchasing-consumable-products-images/image27.png)   
   
   
   
 사용자 지정 클래스, StoreKit 및 앱 스토어 간의 상관 관계는 다음과 같습니다.   
   
   
   
 [ ![사용자 지정 클래스, StoreKit 및 앱 스토어 간의 상호 작용](purchasing-consumable-products-images/image28.png)](purchasing-consumable-products-images/image28.png)

&nbsp;

### <a name="viewcontroller-methods"></a>ViewController 메서드

속성 및 제품 정보를 검색 하는 데 필요한 메서드를 실행 하는 것 외에도 뷰 컨트롤러 구매와 관련 된 알림을 수신 하도록 추가 알림 관찰자가 필요 합니다. 방금 이들은 `NSObjects` 될 등록을에서 제거 `ViewWillAppear` 및 `ViewWillDisappear` 각각.

```csharp
NSObject succeededObserver, failedObserver;
```

생성자도 만듭니다는 `SKProductsRequestDelegate` 하위 클래스 ( `InAppPurchaseManager`)를 다시 만들고 등록 된 `SKPaymentTransactionObserver` ( `CustomPaymentObserver`).   
   
   
   
 앱에서 바로 구매 트랜잭션 처리의 첫 번째 부분 하고자 할 때 사용자를 무언가 구매 샘플 응용 프로그램에서 다음 코드에 나와 있는 것 처럼 단추 누름을 처리 하기 위한 것:

```csharp
buy5Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy5ProductId);
};
buy10Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy10ProductId);
};
```

   
   
 두 번째 부분은 사용자 인터페이스의 알림을 처리 하는 트랜잭션이 성공 했는지,이 경우에 표시 된 잔액을 업데이트 하 여:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionSucceededNotification,
(notification) => {
   balanceLabel.Text = CreditManager.Balance() + " monkey credits";
});
```

마지막 부분은 사용자 인터페이스의 몇 가지 원인에 대 한 트랜잭션이 취소 될 경우 메시지를 표시 합니다. 예제 코드에는 메시지가 단순히 출력 창에 기록 됩니다.

```csharp
failedObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionFailedNotification,
(notification) => {
   Console.WriteLine ("Transaction Failed");
});
```

보기 컨트롤러에서 이러한 방법 외에도 사용 될 제품 구매 트랜잭션에 필요 코드에는 `SKProductsRequestDelegate` 및 `SKPaymentTransactionObserver`합니다.

### <a name="inapppurchasemanager-methods"></a>InAppPurchaseManager 메서드

개수는 샘플 코드 구현 구입 InAppPurchaseManager 클래스에 관련 된 메서드 등는 `PurchaseProduct` 만드는 메서드를는 `SKPayment` 인스턴스 처리를 위해 큐에 추가 합니다:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKPayment payment = SKPayment.PaymentWithProduct (appStoreProductId);
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

결제를 큐에 추가 하는 비동기 작업입니다. 응용 프로그램을 다시 StoreKit 트랜잭션을 처리 하 고 Apple 서버에 전송 하는 동안 제어할 수 있습니다. 이 시점에서 이기는 iOS 사용자가 앱 스토어에 로그인 하 고 그녀는 메시지를 표시 하려면 Apple ID와 암호가 필요한 경우를 확인 합니다.   
   
   
   
 앱 스토어를 사용 하 여 인증 하 고 트랜잭션 동의 해야 성공적으로 사용자를 가정할 때는 `SKPaymentTransactionObserver` StoreKit의 응답을 수신 하 고 트랜잭션 처리를 완료 합니다. 하려면 다음 메서드를 호출 합니다.

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId);
   FinishTransaction(transaction, true);
}
```

마지막 단계는 알려주는 StoreKit 호출 하 여 트랜잭션이 성공적으로 충족 하는지 확인 하는 `FinishTransaction`:

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

제품 전달 되 면 `SKPaymentQueue.DefaultQueue.FinishTransaction` 트랜잭션을 지불 큐에서 제거를 호출 해야 합니다.

### <a name="skpaymenttransactionobserver-custompaymentobserver-methods"></a>(CustomPaymentObserver) SKPaymentTransactionObserver 메서드

StoreKit 호출은 `UpdatedTransactions` 메서드에 배열을 전달 하 고 Apple 서버에서 응답을 받을 때 `SKPaymentTransaction` 검사 하는 코드에 대 한 개체입니다. 메서드는 각 트랜잭션에 반복 하 고 (다음과 같이) 트랜잭션 상태에 따라 다른 기능을 수행:

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

`CompleteTransaction` 메서드는이 섹션의 앞부분 검사 된 – 구매 정보를 저장 `NSUserDefaults`StoreKit 사용 하 여 트랜잭션을 종료 하 고 마지막으로 업데이트 하도록 UI를에 알립니다.

### <a name="purchasing-multiple-products"></a>여러 제품을 구입

응용 프로그램에서 여러 제품을 구입 하는 것이 좋습니다를 사용 하 여는 `SKMutablePayment` 클래스 고 수량 필드 설정:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKMutablePayment payment = SKMutablePayment.PaymentWithProduct (appStoreProductId);
   payment.Quantity = 4; // hardcoded as an example
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

완료 된 트랜잭션 처리 코드는 구매를 올바르게 처리 하는 데 수량 속성을 쿼리하여도 해야 합니다.

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

여러 수량을 구매 하는 사용자 StoreKit 확인 경고 반영 합니다 수량, 단가 및 총 금액이 청구 될 수 다음 스크린샷에 표시 된 것 처럼:

[ ![구매를 확인합니다.](purchasing-consumable-products-images/image30.png)](purchasing-consumable-products-images/image30.png)

## <a name="handling-network-outages"></a>네트워크 중단 상태 처리

앱에서 바로 구매 StoreKit Apple 서버와 통신 하는 작업 네트워크 연결이 필요 합니다. 네트워크 연결을 사용할 수 없는 경우 다음 앱에서 바로 구매 사용할 수 없게 됩니다.

### <a name="product-requests"></a>제품 요청

네트워크를 수행 하는 동안 사용할 수 없는 경우는 `SKProductRequest`, `RequestFailed` 의 메서드는 `SKProductsRequestDelegate` 하위 클래스 ( `InAppPurchaseManager`) 호출 됩니다. 아래와 같이:

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

그런는 ViewController 알림을 수신 대기 하 고 구매 단추에 메시지를 표시 합니다.

```csharp
requestObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerRequestFailedNotification,
(notification) => {
   Console.WriteLine ("Request Failed");
   buy5Button.SetTitle ("Network down?", UIControlState.Disabled);
   buy10Button.SetTitle ("Network down?", UIControlState.Disabled);
});
```

네트워크 연결 일 수 있으므로 모바일 장치에서 임시 응용 프로그램 SystemConfiguration 프레임 워크를 사용 하 여 네트워크 상태를 모니터링할 수 있습니다 및 네트워크 연결을 사용할 수 있는 경우 다시 시도 하십시오. Apple의를 참조 하거나 사용 하 여 해당.

### <a name="purchase-transactions"></a>구입 트랜잭션

StoreKit 지불 큐는 저장 하 고 앞으로 구매 요청 가능 하면 되므로 네트워크 구매 프로세스 중 실패 한 경우 네트워크 중단의 영향에 따라 달라 집니다.   
   
   
   
 트랜잭션 동안 오류가 발생 하는 경우는 `SKPaymentTransactionObserver` 하위 클래스 ( `CustomPaymentObserver`) 갖습니다는 `UpdatedTransactions` 메서드 호출 및 `SKPaymentTransaction` 클래스 실패 상태가 됩니다.

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

`FailedTransaction` 메서드 사용자 취소 인해 오류가 발생 하는지 여부를 다음과 같이 검색:

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

트랜잭션이 실패 하면 경우에는 `FinishTransaction` 트랜잭션을 지불 큐에서 제거 하려면 메서드를 호출 해야 합니다.

```csharp
SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);
```

다음 예제 코드는 ViewController 메시지를 표시할 수 있도록 공지를 보냅니다. 응용 프로그램 추가 표시 하지 않고 트랜잭션을 취소 메시지입니다. 발생할 수 있는 기타 오류 코드는 다음과 같습니다.

```csharp
FailedTransaction Code=0 Cannot connect to iTunes Store
FailedTransaction Code=5002 An unknown error has occurred
FailedTransaction Code=5020 Forget Your Password?
Applications may detect and respond to specific error codes, or handle them in the same way.
```

## <a name="handling-restrictions"></a>처리 제한

**설정 > 일반 > 제한** iOS의 기능은 장치의 특정 기능을 잠글 수 있습니다.   
   
   
   
 사용자가을 통해 앱에서 바로 구매를 진행할 수 있는지 여부를 쿼리할 수 있습니다는 `SKPaymentQueue.CanMakePayments` 메서드. False를 반환이 앱에서 바로 구매 사용자 액세스할 수 없습니다. StoreKit 구매를 시도 하는 경우 사용자에 게 오류 메시지가 표시 자동으로 됩니다. 이 값을 선택 하 여 응용 프로그램는 구매 단추를 숨기 거 나 다른 작업을 대신 수 있습니다.   
   
   
   
 에 `InAppPurchaseManager.cs` 파일은 `CanMakePayments` 메서드는 다음과 같이 StoreKit 함수를 래핑합니다.

```csharp
public bool CanMakePayments()
{
   return SKPaymentQueue.CanMakePayments;
}
```

이 메서드를 테스트 하려면 사용 하는 **제한** 기능 사용 하지 않도록 설정 하는 iOS입니다 **앱에서 바로 구매**:   
   
   
   
 [ ![IOS의 제한 기능을 사용 하 여 앱에서 바로 구매를 사용 하지 않도록 설정 하려면](purchasing-consumable-products-images/image31.png)](purchasing-consumable-products-images/image31.png)   
   
   
   
 이 예제 코드에서 `ConsumableViewController` 에 반응 하는 `CanMakePayments` 표시 하 여 false를 반환 **AppStore 비활성화** 비활성화 된 단추에는 텍스트입니다.

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

다음과 같은이 경우가 처럼 보이는 응용 프로그램의 **앱에서 바로 구매** 기능은 제한 – 구매 단추가 비활성화 됩니다.   
   
   
   
 [ ![기능은 In 앱에서 바로 구매 단추가 비활성화 됩니다 구매를 제한 하면 다음과 같이 표시 되는 응용 프로그램](purchasing-consumable-products-images/image32.png)](purchasing-consumable-products-images/image32.png)   
   
   
   

제품 정보 이루어질 수 될 때 요청 된 `CanMakePayments` 이 false 이면 응용 프로그램 수 아직 검색 하 고 가격을 표시할 수 있도록 합니다. 즉, 제거 하는 경우는 `CanMakePayments` 있지만 사용자는 메시지가 표시 됩니다는 구매 하려고 할 때 확인 구매 단추 여전히 코드에서 활성화 되어야 하는 **앱에서 바로 구매 허용 되지 않습니다** (StoreKit에서 생성 지불 큐 액세스할 때):   
   
   
   
 [ ![앱에서 바로 구매 허용 되지 않습니다.](purchasing-consumable-products-images/image33.png)](purchasing-consumable-products-images/image33.png)   
   
   
   
 실제 응용 프로그램 단추를 완전히 숨기면 등 아마도 StoreKit 자동으로 표시 하는 경고 보다 더 자세한 메시지를 제공 하는 제한 처리 하는 다른 방법을 걸릴 수 있습니다.

