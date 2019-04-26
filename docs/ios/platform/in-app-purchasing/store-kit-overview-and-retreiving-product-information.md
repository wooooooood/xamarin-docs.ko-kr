---
title: StoreKit 개요 및 Xamarin.iOS에서 제품 정보 검색
description: 이 문서에서는 StoreKit 개요를 제공 합니다. StoreKit, StoreKit 상호 작용을 테스트, 제품 판매에 대 한 표시 하 고, 잘못 된 제품을 처리 하 고 지역화 된 가격을 표시를 사용 하 여 사용 되는 클래스를 설명 합니다.
ms.prod: xamarin
ms.assetid: FC21192E-6325-4389-C060-E92DBB5EBD87
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 0dcda2e4fd1ca7773668a0a6fdf46e01f2f0841d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61367019"
---
# <a name="storekit-overview-and-retrieving-product-info-in-xamarinios"></a>StoreKit 개요 및 Xamarin.iOS에서 제품 정보 검색

아래 스크린샷에서 인 앱 구매에 대 한 사용자 인터페이스 표시 됩니다.
트랜잭션 수행 되기 전에 응용 프로그램은 제품의 가격 및 표시에 대 한 설명을 검색 해야 합니다. 다음 사용자가 누르면 **구입**, 응용 프로그램 StoreKit 확인 대화 상자 및 Apple ID 로그인을 관리 하는 요청을 만듭니다. 트랜잭션은 성공 하면 StoreKit 알립니다 응용 프로그램 코드를 가정 하는 트랜잭션 결과 저장 하 고 해당 구매에 대 한 액세스를 사용 하 여 사용자를 제공 해야 합니다.   

   
 [![](store-kit-overview-and-retreiving-product-information-images/image14.png "StoreKit은 트랜잭션 결과 저장 하 고 해당 구매에 대 한 액세스를 사용 하 여 사용자를 제공 해야 하는 응용 프로그램 코드를에 알립니다.")](store-kit-overview-and-retreiving-product-information-images/image14.png#lightbox)

## <a name="classes"></a>클래스

앱 내 구매를 구현 하려면 StoreKit 프레임 워크에서는 다음과 같은 클래스가 필요 합니다.   
   
 **SKProductsRequest** – storekit (스토어 앱)을 판매 하도록 승인 된 제품에 대 한 요청을 합니다. 다양 한 제품 Id 사용 하 여 구성할 수 있습니다.

-   **SKProductsRequestDelegate** – 제품 요청 및 응답을 처리 하는 메서드를 선언 합니다. 
-   **SKProductsResponse** – StoreKit (스토어 앱)에서 대리자에 게 다시 전송 합니다. 제품 Id는 요청과 함께 전송 일치 하는 SKProducts를 포함 되어 있습니다. 
-   **SKProduct** – StoreKit (구성한 iTunes Connect에서에서)에서 제품을 검색 합니다. 제품 ID, 제목, 설명 및 가격 등 제품에 대 한 정보를 포함합니다. 
-   **SKPayment** -제품 ID를 사용 하 여 만든 제품을 구매 하는 데 결제 큐에 추가 합니다. 
-   **SKPaymentQueue** – 큐에 대기 지불 요청을 Apple에 보낼 수 있습니다. 알림 처리 중인 각 지불 결과로 트리거됩니다. 
-   **SKPaymentTransaction** – 완료 된 트랜잭션 (앱 스토어에 의해 처리 되 고 StoreKit 통해 응용 프로그램으로 다시 전송 된 구매 요청)을 나타냅니다. Purchased, 복원 됨 또는 실패 한 트랜잭션 수 수 있습니다. 
-   **SKPaymentTransactionObserver** – StoreKit 결제 큐에서 생성 된 이벤트에 응답 하는 사용자 지정 하위 클래스입니다. 
-   **StoreKit 작업은 비동기적** –를 SKProductRequest를 시작 하거나는 SKPayment 코드에 제어를 반환 하는 큐에 추가 됩니다. Apple 서버에서 데이터를 받을 때 StoreKit SKProductsRequestDelegate 또는 SKPaymentTransactionObserver 서브 클래스에서 메서드를 호출 합니다. 


다음 다이어그램은 다양 한 StoreKit 클래스 (응용 프로그램에서 추상 클래스 구현 해야 합니다) 간의 관계를 보여줍니다.   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image15.png "앱에서 다양 한 StoreKit 클래스의 추상 클래스 간의 관계를 구현 해야 합니다.")](store-kit-overview-and-retreiving-product-information-images/image15.png#lightbox)   
   
   
   
 이러한 클래스는이 문서의 뒷부분에 자세히 설명 되어 있습니다.

## <a name="testing"></a>테스트

대부분의 StoreKit 작업 테스트에 대 한 실제 장치를 필요합니다. (즉 제품 정보 검색 가격 &amp; 설명) 구매 시뮬레이터 에서만 작동 하 고 복원 작업에서 오류를 반환 합니다 (FailedTransaction 코드와 같은 5002 = 알 수 없는 오류가 발생 했습니다).

참고: StoreKit은 iOS 시뮬레이터에서에서 작동 하지 않습니다. IOS 시뮬레이터에서에서 응용 프로그램을 실행 하는 경우 StoreKit 응용 프로그램이 결제 큐를 검색 하려고 시도할 경우 경고를 기록 합니다. 저장소 테스트를 실제 장치에서 수행 되어야 합니다.   
   
   
   
 중요: 설정 응용 프로그램에서 테스트 계정으로 로그인 수행 되지 않습니다. 모든 기존 Apple ID 계정 로그 아웃 로그인 설정 응용 프로그램을 사용할 수 있습니다 다음 라는 메시지가 표시 될 때까지 기다려야 *인 앱 구매 시퀀스 내에서* 테스트 Apple ID를 사용 하 여 로그인   
   
   
   

테스트 계정 사용 하 여 실제 스토어에 로그인을 시도 하면 자동으로 변환 됩니다는 실제 Apple id와 같습니다. 계정이 더 이상 테스트를 위해 사용 됩니다.

StoreKit 코드를 테스트 하려면 테스트 스토어로 연결 되는 특별 한 테스트 계정 (iTunes Connect에서에서 만든)를 사용 하 여 일반 iTunes 테스트 계정 및 로그인의 로그 아웃을 해야 합니다. 현재 계정 방문 로그 아웃 **설정 > iTunes 및 App Store** 다음과 같이 합니다.

 [![](store-kit-overview-and-retreiving-product-information-images/image16.png "현재 계정 방문 설정 iTunes 및 App Store를 로그 아웃")](store-kit-overview-and-retreiving-product-information-images/image16.png#lightbox)
 
그런 다음 테스트 계정으로 로그인 *StoreKit 앱 내에서 요청할 때*:



ITunes Connect에서에서 테스트 사용자가 클릭할 만들려는 **사용자 및 역할** 기본 페이지입니다.

 [![](store-kit-overview-and-retreiving-product-information-images/image17.png "ITunes에서 테스트 사용자를 만들려면 연결 하려면 사용자 및 역할에 기본 페이지에서")](store-kit-overview-and-retreiving-product-information-images/image17.png#lightbox)

선택 **샌드박스 테스터**

 [![](store-kit-overview-and-retreiving-product-information-images/image18.png "샌드박스 테스터를 선택합니다.")](store-kit-overview-and-retreiving-product-information-images/image18.png#lightbox)

기존 사용자의 목록이 표시 됩니다. 새 사용자를 추가 하거나 기존 레코드를 삭제할 수 있습니다. 포털을 하지 않습니다 (현재) 보기 또는 편집 기존 하므로 (특히: 지정한 암호)를 생성 된 각 테스트 사용자의 적절 한 레코드를 유지 하는 것이 좋습니다. 테스트 사용자를 수 있습니다. 테스트 사용자를 삭제 하면 전자 메일 주소를 다른 테스트 계정에 대 한 다시 사용할 수 없습니다.  
   
 [![](store-kit-overview-and-retreiving-product-information-images/image19.png "기존 사용자의 목록이 표시 됩니다.")](store-kit-overview-and-retreiving-product-information-images/image19.png#lightbox)   
   
 새 테스트 사용자 (예: 이름, 암호, 비밀 질문 및 답변) 실제 Apple ID를 유사한 특성이 있습니다. 여기에 입력 된 모든 세부 정보를 기록해를 두어야 합니다. 합니다 **iTunes Store 선택** 필드는 통화를 결정 합니다 하 고 앱에서 바로 구매 하는 언어는 사용 될 때 해당 사용자로 로그인 합니다.

 [![](store-kit-overview-and-retreiving-product-information-images/image20.png "사용자의 통화 및 해당 앱에서 바로 구매에 대 한 언어 선택 iTunes 스토어 필드 결정")](store-kit-overview-and-retreiving-product-information-images/image20.png#lightbox)

## <a name="retrieving-product-information"></a>제품 정보 검색

인 앱 구매 제품을 판매 하는 첫 번째 단계는 표시 되는 것: 표시에 대 한 앱 스토어에서의 현재 가격 및 설명을 검색 합니다.   
   
   
   
 어떤 유형의 제품 앱 판매 (사용할 수 있는, 비-사용할 수 있는 또는 구독 유형)에 관계 없이 표시에 대 한 제품 정보를 가져오는 프로세스는 동일 합니다. 이 문서와 함께 제공 되는 InAppPurchaseSample 코드 라는 프로젝트가 포함 되어 *소모품* 표시에 대 한 프로덕션 정보를 검색 하는 방법을 보여 주는 합니다. 표시 방법:

-  구현을 만드는 `SKProductsRequestDelegate` 하 고 구현 된 `ReceivedResponse` 추상 메서드. 이 예제 코드를 호출 합니다 `InAppPurchaseManager` 클래스입니다. 
-  StoreKit 지불 허용 되는지 여부를 확인 하려면를 사용 하 여 확인 (사용 하 여 `SKPaymentQueue.CanMakePayments` ). 
-  인스턴스화하는 `SKProductsRequest` iTunes Connect에서에서 정의 된 제품 Id를 사용 하 여 합니다. 이 예제의 이루어집니다 `InAppPurchaseManager.RequestProductData` 메서드. 
-  Start 메서드를 호출 합니다 `SKProductsRequest` 합니다. 앱 스토어 서버에 대 한 비동기 호출을 트리거합니다. 대리자 ( `InAppPurchaseManager` ) 결과 사용 하 여 다시 호출 됩니다. 
-  대리자의 ( `InAppPurchaseManager` ) `ReceivedResponse` 메서드가 앱 스토어 (제품 가격 설명 또는 잘못 된 제품에 대 한 메시지)에서 반환 되는 데이터를 사용 하 여 UI를 업데이트 합니다. 

전체 상호 작용은 다음과 같습니다 ( **StoreKit** 하 여 iOS, 기본 제공 하며 **앱 스토어** Apple 서버를 나타냅니다.):

 [![](store-kit-overview-and-retreiving-product-information-images/image21.png "제품 정보 그래프를 검색합니다.")](store-kit-overview-and-retreiving-product-information-images/image21.png#lightbox)

### <a name="displaying-product-information-example"></a>제품 정보의 예 표시

합니다 [InAppPurchaseSample](https://developer.xamarin.com/samples/monotouch/StoreKit/) *소모품* 샘플 코드 제품 정보를 검색할 수 있는 방법을 보여 줍니다. 이 샘플의 주 화면에는 앱 스토어에서 검색 되는 두 제품에 대 한 정보가 표시 됩니다.   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image23.png "앱 스토어에서 검색할 정보 제품을 표시 하는 주 화면")](store-kit-overview-and-retreiving-product-information-images/image23.png#lightbox)   
   
   
   
 검색 하 고 제품 정보를 표시 하도록 샘플 코드를 아래에 자세히 설명 되어 있습니다.


#### <a name="viewcontroller-methods"></a>ViewController 메서드

`ConsumableViewController` 클래스 제품 Id를 가진 클래스에 하드 코딩 되는 두 제품에 대 한 가격의 표시를 관리 합니다.

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

클래스 수준 있어야 NSObject를 선언 하는 데 사용할 설치를 `NSNotificationCenter` 관찰자.

```csharp
NSObject priceObserver;
```

ViewWillAppear 메서드의 관찰자 만들어지고 기본 알림 센터를 사용 하 여 할당 합니다.

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
}
```

   
   
 끝에 `ViewWillAppear` 메서드를 호출 합니다 `RequestProductData` StoreKit 요청을 시작 하는 방법입니다. 이 요청을 이루어지면 StoreKit와 Apple 서버 정보를 가져오고 앱에 다시 피드를 비동기적으로 연락 합니다. 이렇게 합니다 `SKProductsRequestDelegate` 서브 클래스 ( `InAppPurchaseManager`) 다음 섹션에서 설명 하는 합니다.

```csharp
iap.RequestProductData(products);
```

여 가격과 설명을 표시 하는 코드에서 단순히는 SKProduct에서 정보를 검색 하 고 UIKit 컨트롤에 할당 (표시 하는 `LocalizedTitle` 고 `LocalizedDescription` – StoreKit 올바른 텍스트 및 가격에 따라 자동으로 해결 사용자의 계정 설정)입니다. 다음 코드는 위에서 만든 알림에 속합니다.

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

마지막으로 `ViewWillDisappear` 메서드 관찰자 제거 되었는지 확인 해야 합니다.

```csharp
NSNotificationCenter.DefaultCenter.RemoveObserver (priceObserver);
```

#### <a name="skproductrequestdelegate-inapppurchasemanager-methods"></a>(InAppPurchaseManager) SKProductRequestDelegate 메서드

`RequestProductData` 메서드는 응용 프로그램에서 제품 가격 및 기타 정보를 검색할 때 호출 됩니다. 올바른 데이터 형식으로 제품 Id의 컬렉션을 구문 분석 그런 만듭니다는 `SKProductsRequest` 해당 정보를 사용 하 여 합니다. Start 메서드를 호출 하면 Apple 서버에 대 한 네트워크 요청을 합니다. 요청이 비동기적으로 실행 되 고 호출 된 `ReceivedResponse` 성공적으로 완료 되 면 대리자의 메서드.

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

iOS 자동으로 라우팅하므로 요청-를 사용 하 여 응용 프로그램이 실행 되는 프로 비전 프로필에 따라 앱 스토어의 '샌드박스' 또는 '프로덕션' 버전을 개발 하거나 앱을 테스트 하는 요청 하는 경우 모든 제품에 대 한 액세스 iTunes Connect (않더라도 아직 제출 또는 Apple에서 승인 된)에서 구성 합니다. StoreKit 요청에 대 한 정보만 반환은 프로덕션 환경에서 응용 프로그램 이면 **Approved** 제품입니다.   
   
   
   
 `ReceivedResponse` Apple 서버 데이터를 사용 하 여 응답 한 후 재정의 된 메서드를 호출 합니다. 백그라운드에서 이것을, 때문에 코드를 올바른 데이터를 구문 분석 하 고 알림을 수신 하는 ''는 알림에 대 한 모든 ViewControllers 제품 정보를 보내는 데 사용할 해야 합니다. 유효한 제품 정보를 수집 하 고 알림을 보내는 코드는 다음과 같습니다.

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

다이어그램에 표시 되지 않지만 `RequestFailed` 메서드 메서드도 재정의 해야 하는 경우 앱 스토어 서버에 연결할 수 없는 (또는 일부 다른 오류로 발생) 사용자에 게 몇 가지 피드백을 제공할 수 있도록 합니다. 예제 코드는 단순히 콘솔에 기록 하지만 실제 응용 프로그램에 대 한 쿼리 수도 `error.Code` 속성과 구현 사용자 지정 동작 (예: 사용자에 게 경고).

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   Console.WriteLine (" ** InAppPurchaseManager RequestFailed() " + error.LocalizedDescription);
}
```

이 스크린샷은 (사용 가능한 제품 정보가 없는 경우)를 로드 한 직후 응용 프로그램을 예제를 보여 줍니다.

 [![](store-kit-overview-and-retreiving-product-information-images/image24.png "제품 정보가 없는 경우 로드 한 직후 샘플 앱")](store-kit-overview-and-retreiving-product-information-images/image24.png#lightbox)

## <a name="invalid-products"></a>잘못 된 제품

`SKProductsRequest` 잘못 된 제품 Id 목록을 반환할 수도 있습니다. 잘못 된 제품 중 하나로 인해 일반적으로 반환 됩니다.   
   
   
   
 **제품 ID를 잘못 입력** – 올바른 제품 Id만 허용 됩니다.   
   
 **제품 승인 되지 않은** – 테스트 하는 동안 모든 제품 판매에 대 한 선택이 취소 된 하 여 반환할는 `SKProductsRequest`그러나 프로덕션 환경에서 승인 된 제품만 반환 됩니다.   
   
 **앱 ID가 명시적** -와일드 카드 앱 Id (별표) 앱에서 바로 구매 허용 하지 않습니다.   
   
 **잘못 된 프로 비전 프로필** – 변경한 경우 (예: 앱 내 구매 가능)을 프로 비전 포털에서 응용 프로그램 구성이 다시 생성 하 고 앱을 빌드할 때 올바른 프로 비전 프로필을 사용 해야 합니다.   
   
 **iOS 응용 프로그램 유료 계약 준비 되어 있지** – Apple 개발자 계정에 대 한 유효한 계약 없는 StoreKit 기능이 전혀 작동 하지 것입니다.   
   
 **이진 파일이 거부 됨 상태의** StoreKit 기능이 작동 하지 것입니다 (또는 앱 스토어 팀에서 개발자가) 거부 됨 상태에서 이전에 제출 된 이진 있으면 – 합니다.

`ReceivedResponse` 샘플 코드에서 메서드 출력을 콘솔에 잘못 된 제품:

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

가격 계층 모든 국제 앱 스토어에서 각 제품에 대 한 특정 가격을 지정합니다. 다음 확장 메서드를 사용 하 여 각 통화에 대 한 가격은 올바르게 표시 되도록 (에 정의 된 `SKProductExtension.cs`)의 각 가격 속성 대신 `SKProduct`:

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

다음 스크린샷에서 (American 스토어 용 하나) 및 일본어 저장소에 대 한 결과 두 개의 다른 iTunes 테스트 계정을 사용 하 여:   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image25.png "두 개의 다른 iTunes 테스트 결과 특정 언어를 표시 하는 계정")](store-kit-overview-and-retreiving-product-information-images/image25.png#lightbox)   
   
   
   
 저장소 장치의 언어 설정은 레이블 및 기타 지역화 된 콘텐츠는 동안 제품 정보 및 가격 통화에 대 한 사용 되는 언어에 영향을 준다는 것을 확인 합니다.   
   
   
   
 다른 저장소를 사용 하는 테스트 해야 하는 계정이 회수 **로그 아웃** 에 **설정 > iTunes 및 앱 스토어** 다른 계정으로 로그인 하려면 응용 프로그램을 다시 시작 합니다. 변경 하려면 장치의 언어도 이동 **설정 > 일반 > International > 언어**합니다.

