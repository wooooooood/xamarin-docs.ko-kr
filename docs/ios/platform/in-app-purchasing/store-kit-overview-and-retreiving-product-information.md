---
title: Xamarin.ios에서 제품 정보를 보관 하 고 검색 합니다.
description: 이 문서에서는 기능 키트의 개요를 제공 합니다. 이 예에서는 파일 키트에 사용 되는 클래스를 설명 하 고, 파일 키트 상호 작용을 테스트 하 고, 판매 제품을 표시 하 고, 잘못 된 제품을 처리 하 고,
ms.prod: xamarin
ms.assetid: FC21192E-6325-4389-C060-E92DBB5EBD87
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 4eb115889b65819e969b8024fc9fbcdc02b566fb
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648198"
---
# <a name="storekit-overview-and-retrieving-product-info-in-xamarinios"></a>Xamarin.ios에서 제품 정보를 보관 하 고 검색 합니다.

앱 내 구매의 사용자 인터페이스는 아래 스크린샷에 표시 됩니다.
트랜잭션이 발생 하기 전에 응용 프로그램은 표시를 위해 제품의 가격과 설명을 검색 해야 합니다. 그런 다음 사용자가 **구매**를 누르면 응용 프로그램은 확인 대화 상자 및 Apple ID 로그인을 관리 하는 사용자 키트에 요청을 만듭니다. 트랜잭션이 성공 했다고 가정 하면, 사용자는 트랜잭션 결과를 저장 하 고 해당 구매에 대 한 액세스 권한을 사용자에 게 제공 해야 하는 응용 프로그램 코드에 알립니다.   

   
 [![](store-kit-overview-and-retreiving-product-information-images/image14.png "저장소 키트는 응용 프로그램 코드에 알립니다 .이 코드는 트랜잭션 결과를 저장 하 고 사용자에 게 해당 구매에 대 한 액세스를 제공 해야 합니다.")](store-kit-overview-and-retreiving-product-information-images/image14.png#lightbox)

## <a name="classes"></a>클래스

앱에서 바로 구매를 구현 하려면 다음 클래스를 사용 해야 합니다.   
   
 **SKProductsRequest** – 승인 된 제품을 판매 하는 (앱 스토어) 키트에 대 한 요청입니다. 다양 한 제품 Id를 사용 하 여 구성할 수 있습니다.

-   **SKProductsRequestDelegate** – 제품 요청 및 응답을 처리 하는 메서드를 선언 합니다. 
-   **SKProductsResponse** – 저장소 키트 (앱 스토어)에서 대리자로 다시 전송 됩니다. 요청과 함께 전송 된 제품 Id와 일치 하는 제품 Id를 포함 합니다. 
-   이상 **제품** – 사용자가 iTunes Connect에서 구성한 제품입니다. 제품 ID, 제목, 설명 및 가격과 같은 제품에 대 한 정보를 포함 합니다. 
-   비 **결제** -제품 ID로 생성 되 고 구매를 수행 하기 위해 지불 큐에 추가 됩니다. 
-   **SKPaymentQueue** – Apple에 보낼 대기 중인 지불 요청 알림이 처리 되는 각 지불의 결과로 알림이 트리거됩니다. 
-   **SKPaymentTransaction** – 완료 된 트랜잭션 (앱 스토어에서 처리 하 고, 사용자 키트 키트를 통해 응용 프로그램으로 다시 전송 된 구매 요청)을 나타냅니다. 트랜잭션을 구매, 복원 또는 실패할 수 있습니다. 
-   **SKPaymentTransactionObserver** – 사용자 키트 지불 큐에 의해 생성 된 이벤트에 응답 하는 사용자 지정 하위 클래스입니다. 
-   사용자 **키트 작업은 비동기적** 으로 수행 됩니다.-가 나 제품 요청이 시작 되거나 큐에 대기 중인 지불이 추가 된 후에는 컨트롤이 코드로 반환 됩니다. 데이터 파일 키트는 Apple 서버에서 데이터를 받을 때 SKProductsRequestDelegate 또는 SKPaymentTransactionObserver 하위 클래스에서 메서드를 호출 합니다. 


다음 다이어그램에서는 다양 한 사용자 키트 클래스 간의 관계를 보여 줍니다 (추상 클래스는 응용 프로그램에서 구현 해야 함).   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image15.png "다양 한 클래스 키트 클래스 간 관계 추상 클래스는 앱에서 구현 해야 합니다.")](store-kit-overview-and-retreiving-product-information-images/image15.png#lightbox)   
   
   
   
 이러한 클래스는이 문서의 뒷부분에서 자세히 설명 합니다.

## <a name="testing"></a>테스트

대부분의 기능 키트 작업에는 테스트용 실제 장치가 필요 합니다. 제품 정보 검색 (ie. 가격 &amp; 설명)은 시뮬레이터에서 작동 하지만 구매 및 복원 작업은 오류를 반환 합니다 (예: failedtransaction Code = 5002 unknown 오류가 발생 함).

참고: 지 수 키트는 iOS 시뮬레이터에서 작동 하지 않습니다. IOS 시뮬레이터에서 응용 프로그램을 실행 하는 경우 응용 프로그램에서 지불 큐를 검색 하려고 하면이 사용자 키트는 경고를 기록 합니다. 저장소 테스트는 실제 장치에서 수행 해야 합니다.   
   
   
   
 중요: 설정 응용 프로그램에서 테스트 계정으로 로그인 하지 마십시오. 설정 응용 프로그램을 사용 하 여 기존 Apple ID 계정에서 로그 아웃할 수 있습니다. *앱 내 구매 시퀀스 내* 에서 테스트 Apple id를 사용 하 여 로그인 하 라는 메시지가 표시 될 때까지 기다려야 합니다.   
   
   
   

테스트 계정을 사용 하 여 실제 저장소에 로그인 하려고 하면 자동으로 실제 Apple ID로 변환 됩니다. 이 계정은 더 이상 테스트에 사용할 수 없습니다.

저장소 키트 코드를 테스트 하려면 일반 iTunes 테스트 계정에서 로그 아웃 하 고 테스트 저장소에 연결 된 특수 테스트 계정 (iTunes Connect에서 만들어짐)을 사용 하 여 로그인 해야 합니다. 현재 계정에서 로그 아웃 하려면 다음에 표시 된 것 처럼 **iTunes 및 App Store > 설정** 을 방문 하세요.

 [![](store-kit-overview-and-retreiving-product-information-images/image16.png "현재 계정에서 로그 아웃 하려면 설정 iTunes 및 App Store를 방문 하세요.")](store-kit-overview-and-retreiving-product-information-images/image16.png#lightbox)
 
그런 다음 *앱 내의 사용자 키트 키트에서 요청*하는 경우 테스트 계정으로 로그인 합니다.



ITunes Connect에서 테스트 사용자를 만들려면 기본 페이지에서 **사용자 및 역할** 을 클릭 합니다.

 [![](store-kit-overview-and-retreiving-product-information-images/image17.png "ITunes Connect에서 테스트 사용자를 만들려면 기본 페이지에서 사용자 및 역할을 클릭 합니다.")](store-kit-overview-and-retreiving-product-information-images/image17.png#lightbox)

**샌드박스 테스터** 선택

 [![](store-kit-overview-and-retreiving-product-information-images/image18.png "샌드박스 테스터 선택")](store-kit-overview-and-retreiving-product-information-images/image18.png#lightbox)

기존 사용자의 목록이 표시 됩니다. 새 사용자를 추가 하거나 기존 레코드를 삭제할 수 있습니다. 포털에서는 현재 사용자가 기존 테스트 사용자를 보거나 편집할 수 없으므로 생성 되는 각 테스트 사용자 (특히 사용자가 할당 한 암호)에 대 한 적절 한 기록을 유지 하는 것이 좋습니다. 테스트 사용자를 삭제 한 후에는 다른 테스트 계정에 대 한 전자 메일 주소를 다시 사용할 수 없습니다.  
   
 [![](store-kit-overview-and-retreiving-product-information-images/image19.png "기존 사용자의 목록이 표시 됩니다.")](store-kit-overview-and-retreiving-product-information-images/image19.png#lightbox)   
   
 새 테스트 사용자에 게는 실제 Apple ID (예: 이름, 암호, 본인 확인 질문 및 답변)와 유사한 특성이 있습니다. 여기에 입력 한 모든 세부 정보에 대 한 기록을 유지 합니다. **ITunes 스토어 선택** 필드에는 해당 사용자로 로그인 할 때 앱에서 구매할 때 사용 하는 통화 및 언어가 결정 됩니다.

 [![](store-kit-overview-and-retreiving-product-information-images/image20.png "ITunes 스토어 선택 필드는 앱 내 구매에 대 한 사용자의 통화 및 언어를 결정 합니다.")](store-kit-overview-and-retreiving-product-information-images/image20.png#lightbox)

## <a name="retrieving-product-information"></a>제품 정보 검색

앱에서 구매한 제품을 판매 하는 첫 번째 단계는 표시 하기 위해 앱 스토어에서 현재 가격 및 설명 검색을 표시 하는 것입니다.   
   
   
   
 앱에서 판매 하는 제품 유형 (사용할 수 있는 제품 또는 구독 유형)에 관계 없이 표시를 위한 제품 정보를 검색 하는 프로세스는 동일 합니다. 이 문서와 함께 제공 되는 InAppPurchaseSample 코드에는 표시를 위해 프로덕션 정보를 검색 하는 방법을 보여 주는 *소모품* 이라는 프로젝트가 포함 되어 있습니다. 다음 작업을 수행 하는 방법을 보여 줍니다.

-  의 `SKProductsRequestDelegate` 구현을 만들고 추상 메서드를 `ReceivedResponse` 구현 합니다. 예제 코드는이 클래스를 `InAppPurchaseManager` 호출 합니다. 
-  을 사용 하 여 지불이 허용 되는지 (사용 `SKPaymentQueue.CanMakePayments` ) 확인 합니다. 
-  ITunes Connect `SKProductsRequest` 에 정의 된 제품 id를 사용 하 여를 인스턴스화합니다. 예제의 `InAppPurchaseManager.RequestProductData` 메서드에서이 작업을 수행 합니다. 
-  에서 Start 메서드를 호출 합니다 `SKProductsRequest` . 그러면 앱 저장소 서버에 대 한 비동기 호출이 트리거됩니다. 대리자 ( `InAppPurchaseManager` )는 결과를 사용 하 여 다시 호출 됩니다. 
-  대리자의 ( `InAppPurchaseManager` ) `ReceivedResponse` 메서드는 앱 스토어에서 반환 된 데이터 (제품 가격 & 설명 또는 잘못 된 제품에 대 한 메시지)로 UI를 업데이트 합니다. 

전반적인 상호 작용은 다음과 같이 표시 됩니다. 즉, 저장소 **키트가** iOS에 기본 제공 되 고 **앱 스토어** 는 Apple의 서버를 나타냅니다.

 [![](store-kit-overview-and-retreiving-product-information-images/image21.png "제품 정보 그래프 검색")](store-kit-overview-and-retreiving-product-information-images/image21.png#lightbox)

### <a name="displaying-product-information-example"></a>제품 정보 표시 예

[InAppPurchaseSample](https://docs.microsoft.com/samples/xamarin/ios-samples/storekit) *소모품* 샘플 코드는 제품 정보를 검색 하는 방법을 보여 줍니다. 샘플의 주 화면은 앱 스토어에서 검색 된 두 제품에 대 한 정보를 표시 합니다.   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image23.png "주 화면은 앱 스토어에서 검색 된 정보 제품을 표시 합니다.")](store-kit-overview-and-retreiving-product-information-images/image23.png#lightbox)   
   
   
   
 제품 정보를 검색 하 고 표시 하는 샘플 코드에 대해서는 아래에서 자세히 설명 합니다.


#### <a name="viewcontroller-methods"></a>ViewController 메서드

클래스 `ConsumableViewController` 는 제품 id가 클래스에 하드 코딩 되어 있는 두 제품의 가격 표시를 관리 합니다.

```csharp
public static string Buy5ProductId = "com.xamarin.storekit.testing.consume5credits",
   Buy10ProductId = "com.xamarin.storekit.testing.consume10credits";
List<string> products;
InAppPurchaseManager iap;
public ConsumableViewController () : base()
{
   // two products for sale on this page
   products = new List<string>() {Buy5ProductId, Buy10ProductId};
   iap = new InAppPurchaseManager();
}
```

클래스 수준에서는 `NSNotificationCenter` 관찰자를 설정 하는 데 사용 되는 nsobject도 선언 해야 합니다.

```csharp
NSObject priceObserver;
```

ViewWillAppear 메서드에서 관찰자가 만들어지고 기본 알림 센터를 사용 하 여 할당 됩니다.

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
}
```

   
   
 `ViewWillAppear` 메서드 끝에서 `RequestProductData` 메서드를 호출 하 여 복사본 키트 요청을 시작 합니다. 이 요청을 만든 후에는 사용자가 Apple의 서버에 비동기적으로 연결 하 여 정보를 가져오고 앱에 다시 피드 합니다. 이는 다음 섹션에서 `SKProductsRequestDelegate` 설명 하 `InAppPurchaseManager`는 하위 클래스 ()에 의해 수행 됩니다.

```csharp
iap.RequestProductData(products);
```

가격 및 설명을 표시 하는 코드는 해당 제품에서 정보를 검색 하 여 uikit 컨트롤 `LocalizedTitle` 에 할당 합니다. 즉, 및 – 파일 키트는 다음 `LocalizedDescription` 을 기준으로 올바른 텍스트 및 가격을 자동으로 확인 합니다. 사용자의 계정 설정). 다음 코드는 위에서 만든 알림에 속합니다.

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
   var info = notification.UserInfo;
   if (info.ContainsKey(NSBuy5ProductId)) {
       var product = (SKProduct) info.ObjectForKey(NSBuy5ProductId);
       buy5Button.Enabled = true;
       buy5Title.Text = product.LocalizedTitle;
       buy5Description.Text = product.LocalizedDescription;
       buy5Button.SetTitle("Buy " + product.Price, UIControlState.Normal); // price display should be localized
   }
}
```

마지막으로 메서드 `ViewWillDisappear` 는 관찰자가 제거 되었는지 확인 해야 합니다.

```csharp
NSNotificationCenter.DefaultCenter.RemoveObserver (priceObserver);
```

#### <a name="skproductrequestdelegate-inapppurchasemanager-methods"></a>InAppPurchaseManager (c # 제품 Requestdelegate) 메서드

`RequestProductData` 메서드는 응용 프로그램에서 제품 가격과 기타 정보를 검색 하려고 할 때 호출 됩니다. 제품 id의 컬렉션을 올바른 데이터 형식으로 구문 분석 한 다음 해당 정보 `SKProductsRequest` 를 사용 하 여을 만듭니다. Start 메서드를 호출 하면 Apple의 서버에 대 한 네트워크 요청이 발생 합니다. 요청은 비동기적으로 실행 되 고 성공적 `ReceivedResponse` 으로 완료 되 면 대리자의 메서드를 호출 합니다.

```csharp
public void RequestProductData (List<string> productIds)
{
   var array = new NSString[productIds.Count];
   for (var i = 0; i < productIds.Count; i++) {
       array[i] = new NSString(productIds[i]);
   }
   NSSet productIdentifiers = NSSet.MakeNSObjectSet<NSString>(array);
   productsRequest = new SKProductsRequest(productIdentifiers);
   productsRequest.Delegate = this; // for SKProductsRequestDelegate.ReceivedResponse
   productsRequest.Start();
}
```

iOS는 응용 프로그램이 실행 되는 프로 비전 프로필에 따라 앱 스토어의 ' sandbox ' 또는 ' 프로덕션 ' 버전으로 요청을 자동으로 라우팅합니다. 따라서 앱을 개발 하거나 테스트 하는 경우 요청에 모든 제품에 대 한 액세스 권한이 포함 됩니다. iTunes Connect에서 구성 됩니다 (Apple에서 아직 제출 하거나 승인 하지 않은 경우에도). 응용 프로그램이 프로덕션 환경에 있는 경우 사용자 키트 요청은 **승인** 된 제품에 대 한 정보만 반환 합니다.   
   
   
   
 재정의 `ReceivedResponse` 된 메서드는 Apple의 서버에서 데이터에 응답 한 후에 호출 됩니다. 이는 백그라운드에서 호출 되기 때문에 코드는 유효한 데이터를 구문 분석 하 고 알림을 사용 하 여 해당 알림에 대 한 ' 수신 ' 인 모든 ViewControllers로 제품 정보를 전송 해야 합니다. 유효한 제품 정보를 수집 하 고 알림을 보내는 코드는 다음과 같습니다.

```csharp
public override void ReceivedResponse (SKProductsRequest request, SKProductsResponse response)
{
   SKProduct[] products = response.Products;
   NSDictionary userInfo = null;
   if (products.Length > 0) {
       NSObject[] productIdsArray = new NSObject[response.Products.Length];
       NSObject[] productsArray = new NSObject[response.Products.Length];
       for (int i = 0; i < response.Products.Length; i++) {
           productIdsArray[i] = new NSString(response.Products[i].ProductIdentifier);
           productsArray[i] = response.Products[i];
       }
       userInfo = NSDictionary.FromObjectsAndKeys (productsArray, productIdsArray);
   }
   NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerProductsFetchedNotification, this, userInfo);
}
```

다이어그램에는 표시 되지 않지만 응용 프로그램 `RequestFailed` 저장소 서버에 연결할 수 없는 경우 또는 일부 다른 오류가 발생 하는 경우 사용자에 게 피드백을 제공할 수 있도록 메서드를 재정의 해야 합니다. 예제 코드는 단순히 콘솔에 작성 하지만 실제 응용 프로그램은 속성을 `error.Code` 쿼리하고 사용자 지정 동작 (예: 사용자에 대 한 경고)을 구현 하도록 선택할 수 있습니다.

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   Console.WriteLine (" ** InAppPurchaseManager RequestFailed() " + error.LocalizedDescription);
}
```

이 스크린샷에서는 로드 직후 샘플 응용 프로그램을 보여 줍니다 (제품 정보를 사용할 수 없는 경우).

 [![](store-kit-overview-and-retreiving-product-information-images/image24.png "제품 정보를 사용할 수 없을 때 로드 직후 샘플 앱")](store-kit-overview-and-retreiving-product-information-images/image24.png#lightbox)

## <a name="invalid-products"></a>잘못 된 제품

는 `SKProductsRequest` 잘못 된 제품 id의 목록을 반환할 수도 있습니다. 잘못 된 제품은 일반적으로 다음 중 하나로 인해 반환 됩니다.   
   
   
   
 **제품 id의 철자가** 잘못 되었습니다. 유효한 제품 id만 허용 됩니다.   
   
 **제품이 승인 되지** 않았습니다 `SKProductsRequest`. 테스트 하는 동안에서 판매를 지우는 모든 제품은에서 반환 되어야 하지만 프로덕션 환경에서는 승인 된 제품만 반환 됩니다.   
   
 **앱 id는 명시적이 지 않습니다** . 와일드 카드 앱 id (별표 포함)는 앱 내 구매를 허용 하지 않습니다.   
   
 **잘못 된 프로 비전 프로필** – 프로 비전 포털에서 응용 프로그램 구성을 변경 하는 경우 (예: 앱에서 바로 구매 사용) 앱을 빌드할 때 올바른 프로 비전 프로필을 다시 생성 하 고 사용 해야 합니다.   
   
 **IOS 유료 응용 프로그램 계약은 적절 하지** 않습니다. Apple Developer 계정에 대 한 유효한 계약이 없는 경우에는 사용자 키트 기능이 전혀 작동 하지 않습니다.   
   
 **이진이 거부 된 상태입니다** .-이전에 전송 된 이진 파일이 거부 된 상태 (App Store 팀 또는 개발자에 의해)에 있는 경우에는 저장소 키트 기능이 작동 하지 않습니다.

샘플 `ReceivedResponse` 코드의 메서드는 잘못 된 제품을 콘솔에 출력 합니다.

```csharp
public override void ReceivedResponse (SKProductsRequest request, SKProductsResponse response)
{
   // code removed for clarity
   foreach (string invalidProductId in response.InvalidProducts) {
       Console.WriteLine("Invalid product id: " + invalidProductId );
   }
}
```

## <a name="displaying-localized-prices"></a>지역화 된 가격 표시

가격 계층은 모든 국제 앱 스토어에서 각 제품의 특정 가격을 지정 합니다. 각 통화에 대 한 가격이 올바르게 표시 되도록 하려면 각각 `SKProductExtension.cs` `SKProduct`의 Price 속성 대신 다음 확장 메서드 (에 정의 됨)를 사용 합니다.

```csharp
public static class SKProductExtension {
   public static string LocalizedPrice (this SKProduct product)
   {
       var formatter = new NSNumberFormatter ();
       formatter.FormatterBehavior = NSNumberFormatterBehavior.Version_10_4;  
       formatter.NumberStyle = NSNumberFormatterStyle.Currency;
       formatter.Locale = product.PriceLocale;
       var formattedString = formatter.StringFromNumber(product.Price);
       return formattedString;
   }
}
```

단추의 제목을 설정 하는 코드는 다음과 같은 확장 메서드를 사용 합니다.

```csharp
string Buy = "Buy {0}"; // or a localizable string
buy5Button.SetTitle(String.Format(Buy, product.LocalizedPrice()), UIControlState.Normal);
```

서로 다른 두 iTunes 테스트 계정 (아메리카 스토어 및 일본어 스토어 용 하나)을 사용 하면 다음과 같은 스크린샷을 생성 합니다.   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image25.png "언어별 결과를 표시 하는 두 가지 iTunes 테스트 계정")](store-kit-overview-and-retreiving-product-information-images/image25.png#lightbox)   
   
   
   
 저장소는 제품 정보 및 가격 통화에 사용 되는 언어에 영향을 주지만 장치의 언어 설정은 레이블 및 기타 지역화 된 콘텐츠에 영향을 줍니다.   
   
   
   
 다른 저장소 테스트 계정을 사용 하려면 **iTunes 및 App store > 설정** 에서 **로그 아웃** 하 고 다른 계정으로 로그인 하도록 응용 프로그램을 다시 시작 해야 합니다. 장치의 언어를 변경 하려면 **설정 > 일반 > 국가별 > 언어**로 이동 합니다.

