---
title: 트랜잭션 및 확인
ms.prod: xamarin
ms.assetid: 84EDD2B9-3FAA-B3C7-F5E8-C1E5645B7C77
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: c8d86d0ce3119b3e104a65a170ab141484af44a7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="transactions-and-verification"></a>트랜잭션 및 확인

## <a name="restoring-past-transactions"></a>트랜잭션 지난 복원

응용 프로그램을 복원할 수 있는 제품 유형의 지 원하는 경우에 사용자가 구매를 복원할 수 있도록 몇 가지 사용자 인터페이스 요소를 포함 해야 합니다.
이 기능을 사용 하면 제품 추가 장치를 추가 하거나 삭제할 되 고 후 동일한 장치에 제품을 복원 하는 고객 또는 제거 하 고 응용 프로그램을 다시 설치 합니다. 다음 제품 유형은 복원할 수 있습니다.

-  비-사용 가능한 제품
-  자동 갱신 가능한 구독
-  무료 구독


복원 프로세스에서 레코드를 업데이트 해야 해당 제품을 처리 하는 데 장치에 유지 합니다. 고객은 언제 든 지 해당 장치에 복원 하도록 선택할 수 있습니다. 복원 프로세스에는 다시 사용자에 대 한 모든 이전 트랜잭션을 전송합니다 응용 프로그램 코드 (예를 들어는 이미 해당 구매 장치에 대 한 기록 및 구매 레코드를 만들어 not, 확인 및 사용자에 대 한 제품을 사용 하도록 설정)에 해당 정보로 수행할 작업을 확인 한 다음 해야 합니다.

### <a name="implementing-restore"></a>복원 구현

사용자 인터페이스 **복원** RestoreCompletedTransactions에서 트리거를 실행 하는 다음 메서드를 호출 하는 단추는 `SKPaymentQueue`합니다.

```csharp
public void Restore()
{
   // theObserver will be notified of when the restored transactions start arriving <- AppStore
   SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions();
}
```

StoreKit 송신할 복원 요청을 Apple 서버를 비동기적으로 합니다.   
   
   
   
 때문에 `CustomPaymentObserver` 등록 트랜잭션 관찰자도 해당 메시지를 수신할 Apple 서버 응답 합니다. 응답 (모든 장치)에서이 응용 프로그램에서이 사용자 수행한가 모든 트랜잭션이 포함 됩니다. 복원 된 상태 및 호출에서 검색 하는 코드에 대 한 반복 각 트랜잭션에 `UpdatedTransactions` 아래와 같이 처리 하기 위해 메서드:

```csharp
// called when the transaction status is updated
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
       case SKPaymentTransactionState.Restored:
           theManager.RestoreTransaction(transaction);
           break;
default:
           break;
       }
   }
}
```

사용자에 대 한 복원 가능 제품은 경우 `UpdatedTransactions` 호출 되지 않습니다.   
   
   
   
 샘플에 지정된 된 트랜잭션을 복원 하려면 가능한 가장 간단한 코드는 구매 점을 제외 하 고 위치를 사용 하는 경우와 같은 동작은 `OriginalTransaction` 속성 제품 ID에 액세스 하는 데 사용 됩니다.

```csharp
public void RestoreTransaction (SKPaymentTransaction transaction)
{
   // Restored Transactions always have an 'original transaction' attached
   var productId = transaction.OriginalTransaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId); // it's as though it was purchased again
   FinishTransaction(transaction, true);
}
```

다른 보다 정교한 구현 확인할 수 `transaction.OriginalTransaction` 원래 날짜와 수신 확인 번호 등의 속성입니다. 이 정보는 일부 제품 형식 (예: 구독의 경우)에 유용 합니다.

#### <a name="restore-completion"></a>복원 완료

`CustomPaymentObserver` 에 (성공적으로 또는 오류와 함께)을 복원 프로세스가 완료 되 면 StoreKit에서 호출할 수 있는 두 개의 추가 메서드가 아래에 표시 합니다.

```csharp
public override void PaymentQueueRestoreCompletedTransactionsFinished (SKPaymentQueue queue)
{
   Console.WriteLine(" ** RESTORE Finished ");
}
public override void RestoreCompletedTransactionsFailedWithError (SKPaymentQueue queue, NSError error)
{
   Console.WriteLine(" ** RESTORE FailedWithError " + error.LocalizedDescription);
}
```

이 예제에서 이러한 메서드는 아무것도 수행, 실제 응용 프로그램 메시지를 사용자 또는 일부 다른 기능을 구현 하도록 선택할 수 있지만 합니다.

## <a name="securing-purchases"></a>구매 보안 설정

이 두 가지 예제를 사용 하 여 문서화 `NSUserDefaults` 구매 상황을 추적 하려면:   
   
 **소모품** – 신용 구매 '잔액'는 단순 `NSUserDefaults` 는 각 구매도 증가 하는 정수 값입니다.   
   
 **비 소모품** – 각 사진 필터 구매에 키-값 쌍으로 저장 됩니다 `NSUserDefaults`합니다.

사용 하 여 `NSUserDefaults` 단순, 예제 코드를 유지 하지만 사용자 (지불 메커니즘 건너뛰기)의 설정을 업데이트 하는 기술적으로 민감한 수 있으므로 매우 안전한 방법이 솔루션을 제공 하지 않습니다.   
   
참고: 실제 응용 프로그램 구입한 사용자 변조 될 하지 않은 내용을 저장 하는 데는 보안 메커니즘을 채택 해야 합니다. 암호화 및/또는 원격 서버 인증을 비롯 한 기타 기술을 눌러야 할 수 있습니다.   
   
 또한 iOS, iTunes 및 icloud와의 기본 제공 백업 및 복구 기능을 활용 하는 메커니즘을 설계 해야 합니다. 이렇게 하면 백업이 복원 되는 사용자가 이전 구매 즉시 사용할 수 있습니다.   
   
   
 가이드를 참조 하십시오 Apple의 보안 코딩 iOS 관련 지침을 더 합니다.

## <a name="receipt-verification-and-server-delivered-products"></a>수신 확인 및 서버 배달 제품

지금까지이 문서에 대 한 예제는 전적으로 응용 프로그램의 앱 스토어 서버와 직접 통신 기능이 나 응용 프로그램에 이미 코딩 하는 기능을 잠금 해제는 구매 트랜잭션을 수행할 구성 되었습니다.   
   
   
   
 Apple의 구매 보안 수준을 높이기 위해 구매 확인 메일을 구매의 일환으로 디지털 콘텐츠를 배달 하기 전에 요청 유효성을 검사 하는 데 유용할 수 있는 다른 서버에서 독립적으로 확인 하도록 허용 하 여 제공 (디지털 책 같이 또는 magazine)입니다.   
   
   
   
 **기본 제공 제품** -이 문서에이 예제에서는 같은 구매 중인 제품 응용 프로그램과 함께 제공 되는 기능으로 존재 합니다. 앱에서 바로 구매 해당 기능에 액세스할 수 있습니다.
제품 Id는 하드 코딩 합니다.   
   
 **제품 서버-배달 된** – 제품 성공적인 트랜잭션 하면 콘텐츠 다운로드 될 때까지 원격 서버에 저장 되어 있는 다운로드 가능한 콘텐츠로 구성 합니다.
예제도 서 나 잡지 포함 될 수 있습니다. 일반적으로 제품 Id (여기서 제품 콘텐츠의 호스트) 외부 서버에서 원본으로 제공 합니다. 응용 프로그램 콘텐츠 다운로드가 실패 하는 경우 해당 될 수 있도록 다시 시도 사용자를 혼동 하지 않고도 트랜잭션이 완료 된 시기를 기록 하는 강력한 방법을 구현 해야 합니다.

### <a name="server-delivered-products"></a>제품 서버 배달

일부 제품의 설명서 및 잡지 (또는 게임 수준을) 구매 프로세스 중에 원격 서버에서 다운로드 해야 할와 같은 콘텐츠를 합니다. 즉, 저장 및은 구입 후 제품 콘텐츠를 배달 하는 추가적인 서버가 필요 합니다.

#### <a name="getting-prices-for-server-delivered-products"></a>제품 서버 배달에 대 한 가격을 가져오기

제품에 원격으로 배달 하기 때문에 더 많은 책 또는 잡지의 새로운 문제를 추가 하는 등 (하지 않고 응용 프로그램 코드에서 업데이트) 시간에 따라 더 많은 제품을 추가할 수 이기도 합니다. 응용 프로그램이 이러한 뉴스 제품을 검색 하 고 사용자에 게 표시할 수, 되도록 추가 서버를 저장 하 고이 정보를 제공 해야 합니다.   
   
   
   
 [![](transactions-and-verification-images/image38.png "제품 서버 배달에 대 한 가격을 가져오기")](transactions-and-verification-images/image38.png#lightbox)   
   
   
   
 1. 제품 정보는 여러 위치에 저장 되어야 합니다: 서버에서를 iTunes Connect에서에서. 또한 각 제품 연관 된 콘텐츠 파일을 갖습니다. 성공적인 구입 후 이러한 파일을 배달 됩니다.   
   
 2. 로 사용할 때 제품을 구매, 제품을 사용할 수 있는 응용 프로그램 확인 해야 합니다. 이 정보는 캐시 될 않지만 제품의 마스터 목록을 저장 된 원격 서버에서 배달 되도록 합니다.   
   
 3. 서버를 구문 분석을 응용 프로그램에 대 한 제품 Id 목록을 반환 합니다.   
   
 4. 그런 다음 응용 프로그램을 결정 하는 이러한 제품 Id로 전달할 StoreKit 가격과 설명을 검색 합니다.   
   
 5. StoreKit은 Apple 서버를 제품 Id 목록을 보냅니다.   
   
 6. ITunes 서버 (예: 설명 및 현재 가격) 유효한 제품 정보로 응답합니다.   
   
 7. 응용 프로그램의 `SKProductsRequestDelegate` 표시에 대 한 제품 정보를 사용자에 게 전달 됩니다.

#### <a name="purchasing-server-delivered-products"></a>서버 배달 제품 구매

원격 서버에 콘텐츠 요청 올바른지 유효성을 검사 하는 방법이 필요 하기 때문에 (ie.에 대해 지불), 인증을 위해 읽음 확인 정보를 따라 전달 됩니다. 원격 서버는 해당 데이터를 확인 하기 위해 iTunes 전달 및 성공 하면 응용 프로그램에 대 한 응답에 제품 내용을 포함 합니다.   
   
 [![](transactions-and-verification-images/image39.png "서버 배달 제품 구매")](transactions-and-verification-images/image39.png#lightbox)   
   
 1. 앱 추가 `SKPayment` 큐에 있습니다. 사용자의 Apple ID를 묻는 메시지가 표시 되며 지불 확인 하도록 요청 필요한 경우.   
   
 2. StoreKit 처리를 위해 서버에 요청을 보냅니다.   
   
 3. 트랜잭션이 완료 되 면 서버 트랜잭션 수신 확인을 사용 하 여 응답 합니다.   
   
 4. `SKPaymentTransactionObserver` 하위 클래스 확인 메일을 받아 처리 합니다. 서버에서 제품을 다운로드 해야 하기 때문에 응용 프로그램에서는 네트워크 요청을 원격 서버를 시작 합니다.   
   
 5. 원격 서버 콘텐츠 액세스 권한이 부여 된 것을 확인할 수 있도록 다운로드 요청 수신 데이터 포함 되어 있습니다. 응용 프로그램의 네트워크 클라이언트는이 요청에 대 한 응답을 대기합니다.   
   
 6. 서버 콘텐츠에 대 한 요청을 받으면 확인 메일 데이터를 구문 분석 하 고 확인 메일을 확인 하려면 iTunes 서버에 직접 요청은 올바른 트랜잭션 하기 위한 보냅니다. 서버는 샌드박스 또는 프로덕션 URL로 요청을 보낼 것인지 결정 일부 논리를 사용 해야 합니다. 항상 프로덕션 URL을 사용 하 고 경우에 샌드박스를 전환할 Apple 제안 상태: 21007 (샌드박스 확인 프로덕션 서버에 전송)-참조 [기술 참고 2259 FAQ #16](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)합니다.   
   
 7. iTunes 확인 메일을 확인 하 고 유효한 경우 상태 0 반환 합니다.   
   
 8. 서버는 iTunes' 응답을 기다립니다. 올바른 응답을 수신 하는 경우 코드 응용 프로그램에 대 한 응답에 포함할 관련 된 제품 콘텐츠 파일을 찾아야 합니다.   
   
 9. 응용 프로그램 받고 제품 콘텐츠의 장치의 파일 시스템에 저장 하 고 응답을 구문 분석 합니다.   
   
 10. 응용 프로그램 제품을 사용 하도록 설정 하 고 다음 StoreKit의 호출 `FinishTransaction`합니다. 그런 다음 응용 프로그램 구입한 콘텐츠 (예를 들어 구매한 책 또는 잡지 문제 첫 번째 표시 페이지)를 필요에 따라 표시할 수 있습니다.

단순히 트랜잭션이 신속 하 게 완료 될 수 있도록 트랜잭션 수신을 #9 단계에서 저장 하 고 실제 제품 콘텐츠를 다운로드 하는 사용자에 대 한 사용자 인터페이스를 제공 하 여 대체 구현을 매우 큰 제품 콘텐츠 파일에 포함 될 수 있습니다. 시간이 있습니다. 후속 다운로드 요청이 필요한 제품 콘텐츠 파일에 액세스 하려면 저장 된 확인을 보낼 다시 수 있습니다.

### <a name="writing-server-side-receipt-verification-code"></a>서버 쪽 확인 메일 확인 코드를 작성합니다.

단순 HTTP POST 요청/응답으로 #5-워크플로 다이어그램의 8 단계를 포함 하는 서버 쪽 코드에 수신 확인의 유효성 검사를 수행할 수 있습니다.   
   
   
   
 추출 된 `SKPaymentTansaction.TransactionReceipt` 앱의 속성입니다. (5 단계) 확인을 위해 iTunes에 보내야 하는 데이터입니다.

트랜잭션 수신 데이터 (#5 또는 6 단계에서 하나) Base64 인코딩합니다.

다음과 같은 간단한 JSON 페이로드를 만듭니다.

```csharp
{
   "receipt-data" : "(base-64 encoded receipt here)"
}
```

HTTP POST를 JSON [ https://buy.itunes.apple.com/verifyReceipt ](https://buy.itunes.apple.com/verifyReceipt) 프로덕션 또는 [ https://sandbox.itunes.apple.com/verifyReceipt ](https://sandbox.itunes.apple.com/verifyReceipt) 테스트 합니다.   
   
 JSON 응답 다음 키가 포함 됩니다.

```csharp
{
   "status" : 0,
   "receipt" : { (receipt repeated here) }
}
```

상태 0 유효한 수신 확인을 나타냅니다. 서버 구매한 제품의 콘텐츠를 처리 하기 위해 진행할 수 있습니다. 확인 키에 동일한 속성을 사용 하 여 JSON 사전은 `SKPaymentTransaction` 서버 코드에서이 사전 product_id 및 구매 수량이 같은 정보를 검색할 수를 쿼리할 수 있도록 응용 프로그램에서 받은 개체입니다.

Apple의 참조 [저장소 확인 메일 확인](https://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/StoreKitGuide/VerifyingStoreReceipts/VerifyingStoreReceipts.html#//apple_ref/doc/uid/TP40008267-CH104-SW1) 추가 정보에 대 한 설명서입니다.

#### <a name="using-aspnet"></a>ASP.NET을 사용 하 여

C# 개발자는 호출 유용한 오픈 소스 프로젝트 [APNS 형](https://github.com/Redth/APNS-Sharp) ASP.NET에서 작동 하는 수신 확인 코드를 포함 하 합니다. 특히는 `Receipt.cs` 및 `ReceiptVerification.cs` 파일에 [ `Jdsoft.Apple.AppStore` ](https://github.com/Redth/APNS-Sharp/tree/master/JdSoft.Apple.AppStore) 쉽게 Server-Delivered 제품 워크플로 다이어그램에서 #6-8 단계를 구현 하 여.NET 웹 사이트에 디렉터리를 추가할 수 있습니다.   
   
   
   
 ITunes 서버와의 통신이 C#에서 하 게 처리 하기가 쉽습니다 JSON을 사용 합니다.   
   
   
   
 Apple의 앱에서 바로 구매를 참조 [확인 메일 유효성 검사](https://developer.apple.com/library/ios/#releasenotes/StoreKit/IAP_ReceiptValidation/_index.html) 수신 확인이 유효한 지 확인 하는 방법에 대 한 자세한 정보에 대 한 설명서입니다.

### <a name="3rd-party-receipt-verification"></a>3rd 파티 확인 메일 확인

수신 확인 (및 다른 작업)에 대 한 사용자의 서버를 작성 하는 대신 사용할 수 있는 플랫폼을 제공 하는 회사 있습니다. Xamarin 이러한 서비스를 인증 하지 않습니다. 이러한는 단순히 여기에 언급 된 참조에 대 한 합니다.

#### <a name="urban-airship"></a>도시 Airship

도시 Airship 많은 확인 및 푸시 알림 수신을 포함 하 여 iOS 앱에 대 한 다른 백 엔드 서비스를 제공 합니다.   
   
 [http://urbanairship.com/products/in-app-purchase/](http://urbanairship.com/products/in-app-purchase/)



