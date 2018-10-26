---
title: 트랜잭션 및 Xamarin.iOS에서 확인
description: 이 문서에서는 Xamarin.iOS 앱에 대 한 과거 구매의 복원에 대 한 허용 하는 방법을 설명 합니다. 또한 구매 및 server 제공 제품을 보호 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 84EDD2B9-3FAA-B3C7-F5E8-C1E5645B7C77
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 83f5fd233c004271169a4d00d0a65e70aa925b95
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117657"
---
# <a name="transactions-and-verification-in-xamarinios"></a>트랜잭션 및 Xamarin.iOS에서 확인

## <a name="restoring-past-transactions"></a>트랜잭션 이전 복원

응용 프로그램 복원 가능 제품 유형에서는 사용자가 해당 구매를 복원할 수 있도록 일부 사용자 인터페이스 요소를 포함 해야 합니다.
이 기능을 통해 제품 추가 장치를 추가 하거나 새로 초기화 후 동일한 장치에 제품을 복원 하는 고객 또는 제거 하 고 앱을 다시 설치 합니다. 다음 제품 유형은 가능한 있습니다.

-  비-사용할 수 있는 제품
-  자동 갱신이 가능 구독
-  무료 구독

복원 프로세스에서 레코드를 업데이트 해야 제품을 충족 하는 장치에 유지 합니다. 고객은 자신의 장치에서 언제 든 복원 하도록 선택할 수 있습니다. 복원 프로세스에는 다시 사용자에 대 한 모든 이전 트랜잭션을 전송합니다 응용 프로그램 코드 (예를 들어, 이미 있는 경우 장치에서 해당 구매 레코드와 그렇지 않은 구매 레코드를 만드는 경우를 확인 및 사용자에 대 한 제품을 사용 하도록 설정) 정보를 사용 하 여 수행할 작업을 확인 한 다음 해야 합니다.

### <a name="implementing-restore"></a>복원 구현

사용자 인터페이스 **복원** RestoreCompletedTransactions에서 트리거하는 다음 메서드를 호출 하는 단추는 `SKPaymentQueue`합니다.

```csharp
public void Restore()
{
   // theObserver will be notified of when the restored transactions start arriving <- AppStore
   SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions();
}
```

StoreKit는 Apple의 서버에 비동기적으로 복원 요청을 보낼 됩니다.   
   
때문에 `CustomPaymentObserver` 등록으로 트랜잭션 observer, 해당 메시지를 수신할 Apple 서버에서 응답 하는 경우. 응답에는이 사용자 (모든 장치)에서이 응용 프로그램에서 수행한 적이 모든 트랜잭션이 포함 됩니다. 각 트랜잭션을 통해 루프 코드를 사용 하 여 검색 하 여 복원 상태와 호출 된 `UpdatedTransactions` 아래 표시 된 것 처럼 처리 하는 방법.

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

사용자에 대해 가능한 제품이 없습니다 경우 `UpdatedTransactions` 호출 되지 않습니다.   
   
샘플에서 지정된 된 트랜잭션을 복원 하려면 가장 간단한 가능한 코드는 구매는 점을 제외 하면 위치를 사용 하는 경우와 같은 동작을는 `OriginalTransaction` 속성은 제품 ID 액세스를 사용 합니다.

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

다른 고급 구현을 확인할 수 `transaction.OriginalTransaction` 원래 날짜와 수신 번호 등의 속성입니다. 이 정보는 일부 제품 유형 (예: 구독)에 유용 합니다.

#### <a name="restore-completion"></a>복원 완료

`CustomPaymentObserver` (성공적으로 또는 오류), 복원 프로세스가 완료 되 면 StoreKit 호출 되는 두 개의 추가 메서드가 있습니다 아래 참조:

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

예제에서 이러한 메서드는 아무것도 수행, 실제 응용 프로그램 메시지를 사용자 또는 일부 다른 기능을 구현 하도록 선택할 수 있지만.

## <a name="securing-purchases"></a>구매를 보호합니다.

이 두 가지 예제를 사용 하 여 문서화 `NSUserDefaults` 구매를 추적 하려면:   
   
 **소모품** – 크레딧 구매 '균형'은 단순 `NSUserDefaults` 구입한 각 제품을 사용 하 여 증가 하는 정수 값입니다.   
   
 **비-소모품** – 키-값 쌍으로 저장 된 각 사진 필터 구매 `NSUserDefaults`합니다.

사용 하 여 `NSUserDefaults` 예제 코드를 간단 하는 유지 하지만 (지불 메커니즘 건너뛰기) 설정을 업데이트 하려면 사용자가 기술적으로 필자의 될 수 있으므로 매우 안전한 솔루션을 제공 하지 않습니다.   
   
참고: 실제 응용 프로그램에는 구매한 사용자 변조의 없는 콘텐츠를 저장 하기 위한 보안 메커니즘을 채택 해야 합니다. 이 암호화 및/또는 원격 서버 인증을 비롯 한 다른 기술을 포함할 수 있습니다.   
   
 메커니즘은 iOS, iTunes 및 iCloud의 기본 제공 백업 및 복구 기능을 활용 하려면 또한 설계 되어야 합니다. 이렇게 하면 사용자 백업을 복원 하는 다음 이전 구입 즉시 사용할 수 있습니다.   
   
Apple의 보안 코딩 가이드 iOS 관련 지침은 참조 하십시오.

## <a name="receipt-verification-and-server-delivered-products"></a>수신 확인 및 제품 서버 제공

지금까지이 문서의 예제에서는 전적으로 응용 프로그램의 앱 스토어 서버와 직접 통신 이미 앱에 코딩 하는 기능을 잠금 해제 하는 구매 트랜잭션을 수행할 구성 되었습니다.   
   
Apple은 구매 확인 메일 요청을 구매의 일부로 디지털 콘텐츠를 배달 하기 전에 유효성을 검사 하는 데 유용할 수 있는 다른 서버에 의해 독립적으로 확인 될을 허용 하 여 구매 보안 강화를 위해 제공 (디지털 책 같은 또는 magazine)   
   
 **기본 제공 제품** –이 문서의 예제에서는 같은 제품 구매 중인 응용 프로그램과 함께 제공 되는 기능으로 존재 합니다. 인 앱 구매를 사용 하면 기능에 액세스할 수 있습니다.
제품 Id는 하드 코딩 합니다.   
   
 **서버 제공 제품** – 성공적인 트랜잭션 하면 콘텐츠를 다운로드할 때까지 원격 서버에 저장 된 콘텐츠를 다운로드할 수 있는 제품으로 구성 됩니다.
예제에서는 책 이나 잡지를 포함할 수 있습니다. 제품 Id는 일반적으로 (제품 콘텐츠의 호스팅되는) 외부 서버에서 제공 됩니다. 응용 프로그램 콘텐츠 다운로드가 실패 하는 경우 해당 될 수 있도록 다시 시도 혼동 하지 않고는 트랜잭션이 완료 된 경우를 기록 하는 강력한 방법을 구현 해야 합니다.

### <a name="server-delivered-products"></a>서버 제공 제품

일부 제품의 설명서 및 잡지 (또는 게임 수준을) 구매 과정 원격 서버에서 다운로드 해야 같은 콘텐츠를 합니다. 즉, 저장 하 고를 구매한 후에 제품 콘텐츠를 배달 하는 추가 서버가 필요 합니다.

#### <a name="getting-prices-for-server-delivered-products"></a>서버 제공 제품에 대 한 가격 가져오기

있기 때문에 제품을 원격으로 전달할지, 또한 자세한 책 이나 잡지의 새로운 문제 추가 같은 시간에 따른 앱 코드 업데이트), (없이 다른 제품을 추가 합니다. 응용 프로그램이 이러한 뉴스 제품을 검색 하 고 사용자에 게 표시할 수, 있도록 추가 서버를 저장 하 고이 정보를 제공 해야 합니다.   
   
[![](transactions-and-verification-images/image38.png "서버 제공 제품에 대 한 가격 가져오기")](transactions-and-verification-images/image38.png#lightbox)   
   
1. 제품 정보는 여러 위치에 저장 되어야 합니다: iTunes Connect에서에서 서버에 있습니다. 또한 각 제품에 연결 된 콘텐츠 파일 해야 합니다. 이러한 파일을 성공적으로 구입 후 배달 됩니다.   
   
2. 제품을 구매 하고자 하는 경우 응용 프로그램 제품 수를 결정 해야 합니다. 이 정보를 캐시할 수 있습니다 하지만 제품의 마스터 목록을 저장 된 원격 서버에서 배달 되도록 합니다.   
   
3. 서버 응용 프로그램을 구문 분석에 대 한 제품 Id의 목록을 반환 합니다.   
   
4. 그런 다음 응용 프로그램의 가격 및 설명을 검색할 storekit 보내도록 이러한 제품 Id는 결정 합니다.   
   
5. StoreKit은 Apple 서버 제품 Id 목록을 보냅니다.   
   
6. ITunes 서버 유효한 제품 정보 (예: 설명 및 현재 가격)를 사용 하 여 응답합니다.   
   
7. 응용 프로그램의 `SKProductsRequestDelegate` 표시에 대 한 제품 정보를 사용자에 게 전달 됩니다.

#### <a name="purchasing-server-delivered-products"></a>서버 제공 제품 구매

원격 서버에는 콘텐츠 요청을 올바른지 유효성을 검사 하는 방법이 필요 하기 때문에 (ie.에 대해 지불 된), 인증에 대 한 읽음 확인 정보 전달 됩니다. 원격 서버 인증에 대 한 iTunes에 해당 데이터를 전달 하 고 성공 하면 응용 프로그램에 대 한 응답에서 제품 콘텐츠를 포함 합니다.   
   
 [![](transactions-and-verification-images/image39.png "서버 제공 제품 구매")](transactions-and-verification-images/image39.png#lightbox)   
   
1. 앱 추가 `SKPayment` 큐에 있습니다. 필요한 경우 사용자를 해당 Apple ID를 묻는 메시지가 표시 및 지불을 확인 하 라는 메시지가 표시 됩니다.   
   
2. StoreKit 처리를 위해 서버에 요청을 보냅니다.   
   
3. 트랜잭션이 완료 되 면 서버는 트랜잭션 수신 확인을 사용 하 여 응답 합니다.   
   
4. `SKPaymentTransactionObserver` 확인 메일을 수신 하 고 처리 하는 하위 클래스입니다. 서버에서 제품을 다운로드 해야, 하므로 응용 프로그램에는 원격 서버에 네트워크 요청을 시작 합니다.   
   
5. 원격 서버의 콘텐츠에 액세스할 권한이 부여 된 확인할 수 있도록 수신 데이터에 따라 다운로드 요청에 포함 되어 있습니다. 응용 프로그램의 네트워크 클라이언트는이 요청에 응답을 기다립니다.   
   
6. 서버에서 콘텐츠 요청을 받으면 수신 데이터 구문 분석 하 고 유효한 트랜잭션에 대 한 확인 메일을 확인 하려면 iTunes 서버에 직접 요청을 보냅니다. 서버는 프로덕션 또는 샌드박스 URL로 요청을 보낼지 여부를 확인 하려면 일부 논리를 사용 해야 합니다. 항상 프로덕션 URL을 사용 하 여 및 경우 샌드박스에 전환 Apple에서 제안 하 여 21007 (프로덕션 서버에 전송 되는 샌드박스 수신 확인) 상태를 수신 합니다. Apple 참조 [수신 유효성 검사 프로그래밍 가이드](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html) 대 한 자세한 내용은 합니다.
   
7. iTunes 수신을 확인 하 고 유효한 경우 0의 상태를 반환 합니다.   
   
8. 서버는 iTunes의 응답을 기다립니다. 유효한 응답을 수신 하는 경우 코드를 응용 프로그램에 대 한 응답에 포함할 관련된 제품 콘텐츠 파일을 찾습니다.   
  
9. 응용 프로그램 받고 제품 콘텐츠의 장치의 파일 시스템에 저장 하 고 응답을 구문 분석 합니다.   
   
10. 응용 프로그램 제품을 사용 하도록 설정 하 고 StoreKit의 호출 `FinishTransaction`합니다. 그런 다음 응용 프로그램 구입한 콘텐츠 (예를 들어 구매한 책 이나 잡지 문제의 첫 번째 표시 페이지)를 필요에 따라 표시할 수 있습니다.

매우 큰 제품 콘텐츠 파일에 대 한 대체 구현에는 트랜잭션이 신속 하 게 완료할 수 있도록 트랜잭션 수신을 #9 단계에서 저장 및 실제 제품 콘텐츠를 다운로드 하는 사용자에 대 한 사용자 인터페이스를 제공 포함 될 수 있습니다. 나중입니다. 후속 다운로드 요청이 필요한 제품 콘텐츠 파일에 액세스 하는 저장된 확인을 보낼 다시 수 있습니다.

### <a name="writing-server-side-receipt-verification-code"></a>서버 쪽 수신 확인 코드를 작성합니다.

단순 HTTP POST 요청/응답으로 #5-워크플로 다이어그램의 8 단계를 포함 하는 서버 쪽 코드에서 확인 메일 유효성 검사를 수행할 수 있습니다.   
   
추출 된 `SKPaymentTansaction.TransactionReceipt` 앱의 속성입니다. 확인 (5 단계)에 대 한 iTunes에 전송 해야 하는 데이터입니다.

(#5 또는 6 단계의 하나) 트랜잭션 수신 데이터를 Base64로 인코딩되어야 합니다.

다음과 같은 간단한 JSON 페이로드를 만듭니다.

```csharp
{
   "receipt-data" : "(base-64 encoded receipt here)"
}
```

HTTP POST JSON을 [ https://buy.itunes.apple.com/verifyReceipt ](https://buy.itunes.apple.com/verifyReceipt) 프로덕션용 또는 [ https://sandbox.itunes.apple.com/verifyReceipt ](https://sandbox.itunes.apple.com/verifyReceipt) 테스트 합니다.   
   
 JSON 응답은 다음 키가 포함 됩니다.

```csharp
{
   "status" : 0,
   "receipt" : { (receipt repeated here) }
}
```

0의 상태는 유효한 수신 확인을 나타냅니다. 구매한 제품의 콘텐츠를 처리 하는 서버에 진행할 수 있습니다. 수신 키와 같은 속성을 사용 하 여 JSON 사전에 포함 된 `SKPaymentTransaction` 서버 코드 product_id 구매 수량 등의 정보를 검색 하려면이 사전에 쿼리할 수 있도록 앱에서 받은 개체입니다.

Apple의를 참조 하세요 [수신 유효성 검사 프로그래밍 가이드](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Introduction.html) 추가 정보에 대 한 설명서입니다.
