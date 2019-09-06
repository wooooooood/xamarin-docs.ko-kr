---
title: Xamarin.ios의 트랜잭션 및 확인
description: 이 문서에서는 Xamarin.ios 앱에서 과거 구매를 복원 하도록 허용 하는 방법을 설명 합니다. 또한 구매 및 서버에서 제공 하는 제품을 보호 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 84EDD2B9-3FAA-B3C7-F5E8-C1E5645B7C77
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: 537d804f1fa7e6ac95cb86a16849ed9fbc006507
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290196"
---
# <a name="transactions-and-verification-in-xamarinios"></a>Xamarin.ios의 트랜잭션 및 확인

## <a name="restoring-past-transactions"></a>이전 트랜잭션 복원

응용 프로그램이 복원 가능한 제품 유형을 지 원하는 경우 사용자가 해당 구매를 복원할 수 있도록 일부 사용자 인터페이스 요소를 포함 해야 합니다.
이 기능을 사용 하면 고객이 추가 장치에 제품을 추가 하거나 치료 또는 제거 후 앱을 다시 설치 하 여 동일한 장치로 제품을 복원할 수 있습니다. 다음 제품 유형은 복원 가능한입니다.

- 사용할 없는 제품
- 자동 갱신 가능한 구독
- 무료 구독

복원 프로세스는 제품을 충족 하기 위해 장치에 보관 하는 레코드를 업데이트 해야 합니다. 고객은 언제 든 지 모든 장치에서 복원 하도록 선택할 수 있습니다. 복원 프로세스는 해당 사용자에 대 한 모든 이전 트랜잭션을 다시 보냅니다. 그러면 응용 프로그램 코드에서 해당 정보를 사용 하 여 수행할 작업을 결정 해야 합니다. 예를 들어 장치에 해당 구매에 대 한 레코드가 이미 있는지 확인 하 고, 그렇지 않은 경우 구매 레코드를 만들고 해당 사용자에 대 한 제품을 사용 하도록 설정 합니다.

### <a name="implementing-restore"></a>복원 구현

사용자 인터페이스 **복원** 단추는에서 RestoreCompletedTransactions `SKPaymentQueue`를 트리거하는 다음 메서드를 호출 합니다.

```csharp
public void Restore()
{
   // theObserver will be notified of when the restored transactions start arriving <- AppStore
   SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions();
}
```

기능 키트는 복원 요청을 Apple의 서버에 비동기적으로 보냅니다.   
   
는 `CustomPaymentObserver` 트랜잭션 관찰자로 등록 되기 때문에 Apple의 서버가 응답할 때 메시지를 받게 됩니다. 응답에는이 사용자가이 응용 프로그램에서 수행한 모든 트랜잭션 (모든 장치에서)이 포함 됩니다. 이 코드는 각 트랜잭션을 반복 하 여 복원 된 상태를 검색 하 `UpdatedTransactions` 고 아래와 같이 메서드를 호출 하 여 처리 합니다.

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

사용자에 대 한 복원 가능한 제품이 없으면 `UpdatedTransactions` 이 호출 되지 않습니다.   
   
샘플에서 지정 된 트랜잭션을 복원 하는 가장 간단한 코드는 `OriginalTransaction` 속성이 제품 ID에 액세스 하는 데 사용 된다는 점을 제외 하 고는 구매가 수행 될 때와 동일한 작업을 수행 합니다.

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

보다 정교한 구현은 원래 날짜 및 수신 `transaction.OriginalTransaction` 번호와 같은 다른 속성을 확인할 수 있습니다. 이 정보는 일부 제품 유형 (예: 구독)에 유용 합니다.

#### <a name="restore-completion"></a>복원 완료

에 `CustomPaymentObserver` 는 다음과 같은 두 가지 추가 메서드가 있습니다. 복원 프로세스가 완료 되 면 (성공 또는 실패) 다음이 표시 됩니다.

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

예제에서 이러한 메서드는 아무 작업도 수행 하지 않지만 실제 응용 프로그램은 사용자 또는 다른 기능에 메시지를 구현 하도록 선택할 수 있습니다.

## <a name="securing-purchases"></a>구매 보안

이 문서의 두 예제는를 사용 `NSUserDefaults` 하 여 구매를 추적 합니다.   
   
 **소모품** – 크레딧 구매의 ' 잔액 '은 각 구매 시 `NSUserDefaults` 증가 하는 간단한 정수 값입니다.   
   
 **비 소비** -각 사진 필터 구매는의 `NSUserDefaults`키-값 쌍으로 저장 됩니다.

를 `NSUserDefaults` 사용 하면 예제 코드가 간단 하 게 유지 되지만, 기술적으로 관심이 있는 사용자가 설정을 업데이트 (지불 메커니즘 무시) 할 수 있으므로 매우 안전한 솔루션을 제공 하지 않습니다.   
   
참고: 실제 응용 프로그램은 사용자 변조에 영향을 받지 않는 구매 된 콘텐츠를 저장 하는 보안 메커니즘을 채택 해야 합니다. 여기에는 암호화 및/또는 원격 서버 인증을 비롯 한 다른 방법이 포함 될 수 있습니다.   
   
 또한이 메커니즘은 iOS, iTunes 및 iCloud의 기본 제공 백업 및 복구 기능을 활용 하도록 설계 되어야 합니다. 이렇게 하면 사용자가 백업을 복원한 후 이전 구매를 즉시 사용할 수 있게 됩니다.   
   
자세한 iOS 관련 지침은 Apple의 보안 코딩 가이드를 참조 하세요.

## <a name="receipt-verification-and-server-delivered-products"></a>수신 확인 및 서버에서 제공 하는 제품

지금까지이 문서의 예제는 앱 스토어 서버와 직접 통신 하 여 앱에 이미 코딩 된 기능 또는 기능의 잠금을 해제 하는 응용 프로그램 으로만 이루어져 있습니다.   
   
Apple은 다른 서버에서 구매 확인을 독립적으로 확인할 수 있도록 하 여 추가 수준의 구매 보안을 제공 합니다 .이를 통해 구매의 일부로 디지털 콘텐츠를 전달 하기 전에 요청을 유효성 검사 하는 데 유용할 수 있습니다 (예: 디지털 설명서 또는 잡지).   
   
 **기본 제공 제품** –이 문서의 예제와 같이 구매한 제품은 응용 프로그램과 함께 제공 되는 기능으로 존재 합니다. 앱 내 구매를 통해 사용자는 기능에 액세스할 수 있습니다.
제품 Id는 하드 코드 되어 있습니다.   
   
 **서버에서 제공** 하는 제품 –이 제품은 성공적인 트랜잭션으로 인해 콘텐츠가 다운로드 될 때까지 원격 서버에 저장 된 다운로드 가능한 콘텐츠로 구성 됩니다.
예제에는 책 또는 잡지 문제가 포함 될 수 있습니다. 제품 Id는 일반적으로 외부 서버에서 소싱 됩니다 (제품 콘텐츠도 호스트 됨). 응용 프로그램은 트랜잭션이 완료 된 시기를 기록 하는 강력한 방법을 구현 해야 하므로 콘텐츠 다운로드에 실패 하면 사용자를 혼동 하지 않고 다시 시도할 수 있습니다.

### <a name="server-delivered-products"></a>서버에서 제공 하는 제품

책, 잡지 (또는 게임 수준)와 같은 일부 제품 콘텐츠는 구매 프로세스 중에 원격 서버에서 다운로드 해야 합니다. 즉, 제품 콘텐츠를 구매한 후 저장 하 고 배달 하려면 추가 서버가 필요 합니다.

#### <a name="getting-prices-for-server-delivered-products"></a>서버에서 제공 하는 제품 가격 얻기

제품이 원격으로 전달 되기 때문에 책을 추가 하거나 잡지의 새로운 문제를 추가 하는 등, 시간에 따라 제품을 더 추가할 수 있습니다 (앱 코드를 업데이트 하지 않고). 응용 프로그램이 이러한 뉴스 제품을 검색 하 여 사용자에 게 표시할 수 있도록 추가 서버는이 정보를 저장 하 고 제공 해야 합니다.   
   
[![](transactions-and-verification-images/image38.png "서버에서 제공 하는 제품 가격 얻기")](transactions-and-verification-images/image38.png#lightbox)   
   
1. 제품 정보는 서버와 iTunes Connect에서 여러 위치에 저장 해야 합니다. 또한 각 제품에는 연결 된 콘텐츠 파일이 있습니다. 이러한 파일은 성공적으로 구매한 후에 배달 됩니다.   
   
2. 사용자가 제품을 구입 하려는 경우 응용 프로그램에서 사용할 수 있는 제품을 결정 해야 합니다. 이 정보는 캐시 될 수 있지만, 제품의 마스터 목록이 저장 된 원격 서버에서 배달 되어야 합니다.   
   
3. 서버는 구문 분석할 응용 프로그램의 제품 Id 목록을 반환 합니다.   
   
4. 그런 다음 응용 프로그램은이 제품 Id 중에서 지 수 키트에 전송 하 여 가격 및 설명을 검색 합니다.   
   
5. 서버 키트는 Apple의 서버에 제품 Id 목록을 보냅니다.   
   
6. ITunes 서버는 유효한 제품 정보 (설명 및 현재 가격)를 사용 하 여 응답 합니다.   
   
7. 응용 프로그램의 `SKProductsRequestDelegate` 사용자에 게 표시 하기 위해 제품 정보를 전달 합니다.

#### <a name="purchasing-server-delivered-products"></a>서버에서 제공 하는 제품 구매

원격 서버에서 콘텐츠 요청이 유효한 지 확인 하는 몇 가지 방법이 필요 하기 때문에 (ie .는에 대해 지불 됨) 확인 정보는 인증을 위해 함께 전달 됩니다. 원격 서버는 확인을 위해 해당 데이터를 iTunes에 전달 하 고 성공 하면 응용 프로그램에 대 한 응답에 제품 콘텐츠를 포함 합니다.   
   
 [![](transactions-and-verification-images/image39.png "서버에서 제공 하는 제품 구매")](transactions-and-verification-images/image39.png#lightbox)   
   
1. 앱이 큐 `SKPayment` 에를 추가 합니다. 필요한 경우 사용자에 게 Apple ID를 입력 하 라는 메시지가 표시 되 고 지불을 확인 하 라는 메시지가 표시 됩니다.   
   
2. 서버를 처리 하기 위해 서버에 요청을 보냅니다.   
   
3. 트랜잭션이 완료 되 면 서버는 트랜잭션 수신으로 응답 합니다.   
   
4. 서브 `SKPaymentTransactionObserver` 클래스는 수신을 수신 하 고 처리 합니다. 제품을 서버에서 다운로드 해야 하기 때문에 응용 프로그램은 원격 서버에 대 한 네트워크 요청을 시작 합니다.   
   
5. 다운로드 요청은 원격 서버에서 콘텐츠에 액세스할 수 있는 권한이 있는지 확인할 수 있도록 수신 데이터와 함께 제공 됩니다. 응용 프로그램의 네트워크 클라이언트는이 요청에 대 한 응답을 기다립니다.   
   
6. 서버는 콘텐츠에 대 한 요청을 받으면 수신 데이터를 구문 분석 하 고 iTunes 서버에 직접 요청을 보내 올바른 트랜잭션에 대 한 수신 인지 확인 합니다. 서버는 일부 논리를 사용 하 여 프로덕션 또는 샌드박스 URL에 요청을 보낼지 여부를 결정 해야 합니다. Apple은 항상 프로덕션 URL을 사용 하 고 수신 상태 21007 (샌드박스 확인이 프로덕션 서버에 전송 된 경우)로 전환 하는 것을 제안 합니다. 자세한 내용은 Apple의 [수신 확인 프로그래밍 가이드](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html) 를 참조 하세요.
   
7. iTunes는 수신 확인을 확인 하 고 유효한 경우 상태를 0으로 반환 합니다.   
   
8. 서버에서 iTunes 응답을 기다립니다. 유효한 응답을 수신 하는 경우 코드는 응용 프로그램에 대 한 응답에 포함할 연결 된 제품 콘텐츠 파일을 찾습니다.   
  
9. 응용 프로그램은 응답을 받아서 구문 분석 하 여 제품 콘텐츠를 장치의 파일 시스템에 저장 합니다.   
   
10. 이 응용 프로그램은 제품을 사용 하도록 설정한 다음,이 `FinishTransaction`기능을 호출 합니다. 그런 다음 응용 프로그램은 구매 된 콘텐츠를 선택적으로 표시할 수 있습니다. 예를 들어 구매한 책 또는 잡지 문제의 첫 번째 페이지를 표시 합니다.

매우 큰 제품 콘텐츠 파일의 대체 구현에는 트랜잭션이 신속 하 게 완료 될 수 있도록 #9 단계에서 트랜잭션 수신 확인을 저장 하 고 사용자가 실제 제품 콘텐츠를 다운로드할 수 있는 사용자 인터페이스를 제공 하는 것만 포함 될 수 있습니다. 나중에 후속 다운로드 요청은 저장 된 수신 확인을 다시 전송 하 여 필요한 제품 콘텐츠 파일에 액세스할 수 있습니다.

### <a name="writing-server-side-receipt-verification-code"></a>서버 쪽 수신 확인 코드 작성

서버 쪽 코드에서 수신 확인은 워크플로 다이어그램의 #8를 통해 #5 단계를 포함 하는 간단한 HTTP POST 요청/응답을 사용 하 여 수행할 수 있습니다.   
   
앱에서 `SKPaymentTansaction.TransactionReceipt` 속성을 추출 합니다. 확인을 위해 iTunes로 보내야 하는 데이터입니다 (단계 #5).

트랜잭션 수신 데이터 (단계 #5 또는 #6)를 Base64로 인코딩합니다.

다음과 같이 간단한 JSON 페이로드를 만듭니다.

```csharp
{
   "receipt-data" : "(base-64 encoded receipt here)"
}
```

프로덕션 또는 [https://buy.itunes.apple.com/verifyReceipt](https://buy.itunes.apple.com/verifyReceipt) [https://sandbox.itunes.apple.com/verifyReceipt](https://sandbox.itunes.apple.com/verifyReceipt) 테스트용으로 JSON을 게시 합니다.   
   
 JSON 응답에는 다음 키가 포함 됩니다.

```csharp
{
   "status" : 0,
   "receipt" : { (receipt repeated here) }
}
```

상태 0은 올바른 수신 여부를 나타냅니다. 서버에서 구매한 제품의 콘텐츠를 계속 처리할 수 있습니다. 수신 키에는 앱에서 받은 `SKPaymentTransaction` 개체와 동일한 속성을 가진 JSON 사전이 포함 되어 있으므로 서버 코드는이 사전을 쿼리하여 구매 product_id 및 수량 등의 정보를 검색할 수 있습니다.

자세한 내용은 Apple의 [영수증 유효성 검사 프로그래밍 가이드](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Introduction.html) 설명서를 참조 하세요.
