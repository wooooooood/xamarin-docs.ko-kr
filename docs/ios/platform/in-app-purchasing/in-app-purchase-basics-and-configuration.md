---
title: Xamarin.iOS에서 구성과 앱에서 바로 구매 기본 사항
description: 이 문서에서는 Xamarin.iOS, 규칙, 구성 및 iTunes Connect에 대 한 관련 정보를 논의에서 앱 내 구매를 설명 합니다.
ms.prod: xamarin
ms.assetid: 11FB7F02-41B3-2B34-5A4F-69F12897FE10
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 267dac5b6aec263f1d8b69d81f34f732118c1802
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671986"
---
# <a name="in-app-purchase-basics-and-configuration-in-xamarinios"></a>Xamarin.iOS에서 구성과 앱에서 바로 구매 기본 사항

앱에서 바로 구매 구현 장치의 StoreKit API를 활용 하도록 응용 프로그램에 필요 합니다. StoreKit 제품 정보를 가져오고 트랜잭션을 수행 하려면 Apple iTunes 서버와의 모든 통신을 관리 합니다. 인 앱 구매에 대 한 프로 비전 프로필을 구성 해야 하 고 iTunes Connect에서에서 제품 정보를 입력 해야 합니다.

 [![](in-app-purchase-basics-and-configuration-images/image1.png "StoreKit이이 차트에 표시 된 대로 apple의 모든 통신을 관리")](in-app-purchase-basics-and-configuration-images/image1.png#lightbox)

앱 스토어를 사용 하 여 앱에서 바로 구매 수 있도록 다음 설치 및 구성에 필요 합니다.

-  **iTunes Connect** – 제품을 판매를 구성 하 고 구매 테스트할 샌드박스 사용자 계정을 설정 합니다. 또한 제공 했어야 합니다 은행 및 세금 정보를 Apple에 사용자를 대신해 수집 자금을 송금할 수 있도록 합니다.
-   **iOS Provisioning Portal** – 번들 식별자를 만들고 앱에 대 한 앱 스토어 액세스를 사용 하도록 설정 합니다.
-  **키트 저장** – 제품을 표시, 제품을 구입 및 트랜잭션을 복원에 대 한 앱에 코드를 추가 합니다.
-  **사용자 지정 코드** – 진행을 고객이 구매한 제품 또는 이러한 구입한 서비스를 제공 합니다. 제품 (예: 서적과 잡지) 서버에서 다운로드 콘텐츠로 구성 해야 하는 경우 수신 확인의 유효성을 검사 하는 서버 쪽 프로세스를 구현할 수도 할 수 있습니다.


두 가지 저장소 키트 "서버 환경" 가지가 있습니다.

-  **프로덕션** – 실제 비용을 사용 하 여 트랜잭션. 제출 되 고 Apple에서 승인 된 응용 프로그램을 통해 액세스할 수 있습니다. 또한 인 앱 구매 제품을 검토 및 프로덕션 환경에서 사용 되기 전에 먼저 승인 해야 합니다.
-  **샌드박스** – 발생 하는 테스트 하는 위치입니다. 제품은 여기 (승인 프로세스에만 적용 됩니다 프로덕션 환경)을 만든 후 즉시 합니다. 샌드박스에서 트랜잭션은 트랜잭션을 수행 하려면 테스트 사용자 (없습니다 실제 Apple Id) 필요 합니다.

## <a name="in-app-purchase-rules"></a>인 앱 구매 규칙

앱 내에서 다른 형태의 디지털 제품 또는 서비스에 대 한 지불을 수락 하거나 힘들 정도로 종류가 없거나 앱 내에서 사용자를 참조 합니다. 즉, 가장 적합 한 지불 메커니즘은 앱에서 바로 구매 하는 경우 신용 카드 또는 PayPal를 수락할 수 없습니다. 앱 외부 디지털 제품 구입에 대 한 특별 한 경우 이지만 사용 하 여 같은 특정 "로그인"와 연결 된 웹 사이트에서 책을 구입 하 고 "로그인" 앱에 대 한 사용자 액세스 수는 사용 하 여 앱에서 구매한 책입니다.
이러한 방식으로 작동 하는 응용 프로그램 언급 또는 외부 구매 기능을 연결할 수 없습니다 – 개발자 (아마도 통해 전자 메일 마케팅 또는 일부 다른 직접 채널) 다른 방법으로 사용자에 게이 기능을 통신 해야 합니다.

그러나를 사용할 수 없으므로 앱 내 구매 실제 상품에 대 한 허용 되는 경우 (예: 다른 지불 메커니즘을 사용 하 여에 신용 카드, PayPal)에서 앱 내에서.

Apple에서 판매 – 이름, 설명 들어가고 'product' 스크린샷 검토에 필요 하기 전에 모든 제품을 승인 해야 합니다. 제품 검토 시간이 응용 프로그램 검토 동일 합니다.

제품에 대 한 모든 가격을 선택할 수 없습니다.-'가격 계층을 ' Apple 지 각 국가/통화에 특정 값만 선택할 수 있습니다. 다양 한 시장에서 다른 가격 계층 수는 없습니다.

## <a name="configuration"></a>구성하기

앱 내 구매 코드를 작성 하기 전에 iTunes Connect에서에서 일부 설정 작업을 수행 해야 합니다 ( [itunesconnect.apple.com](http://itunesconnect.apple.com)) 및 iOS Provisioning Portal ( [developer.apple.com/iOS](https://developer.apple.com/iOS)).

다음 세 단계는 코드를 작성 하기 전에 완료 해야 합니다.

-  **Apple 개발자 계정** – Apple에 은행 및 세금 정보를 제출 합니다.
-  **iOS Provisioning Portal** – 앱에 유효한 앱 ID를 확인 (없습니다 별표가 와일드 카드 *에) 및 앱 구입에 사용 하도록 설정한 합니다.
-  **iTunes Connect 응용 프로그램 관리** – 제품 응용 프로그램을 추가 합니다.


### <a name="apple-developer-account"></a>Apple 개발자 계정

빌드 및 사용 가능한 앱 배포에에서 아주 적은 구성이 필요로 [iTunes Connect](https://itunesconnect.apple.com)이지만 판매 유료 앱 또는 앱 내 구매 하려면 은행 및 세금 정보를 사용 하 여 Apple를 제공할 수 있습니다. 클릭할 **계약, 세금 및 뱅킹** 여기에 표시 된 주 메뉴에서:

 [![](in-app-purchase-basics-and-configuration-images/image2.png "계약, 세금 및 뱅킹 주 메뉴에서 클릭")](in-app-purchase-basics-and-configuration-images/image2.png#lightbox)

개발자 계정이 있어야 합니다는 **iOS 유료 응용 프로그램** 실제로이 스크린샷에 표시 된 대로 계약:

 [![](in-app-purchase-basics-and-configuration-images/image3.png "개발자 계정 유료 응용 프로그램 계약에 적용 하는 iOS 있어야 합니다.")](in-app-purchase-basics-and-configuration-images/image3.png#lightbox)

될 때까지 모든 StoreKit 기능을 테스트할 수 없습니다는 **iOS 유료 응용 프로그램** 계약 – StoreKit 호출 코드에서 Apple에 처리 될 때까지 실패 합니다 하 **계약, 세금 및 뱅킹** 정보입니다.

### <a name="ios-provisioning-portal"></a>iOS 프로비전 포털

새 응용 프로그램에서 설정 하는 합니다 **앱 Id** 섹션을 **iOS Provisioning Portal**합니다. 로 새 앱 ID를 만들려면 합니다 [ios Provisioning Portal Member Center](https://developer.apple.com/membercenter/index.action), 이동할 **인증서, 식별자 및 프로필** 클릭 확인 하 고 포털의 섹션 **식별자** 아래에서 *iOS 앱*합니다. 그런 다음 새 앱 ID를 생성 하려면 위쪽에서 "+" 오른쪽을 클릭


새로 만들기 폼 **앱 Id**

 다음과 같습니다.

 [![](in-app-purchase-basics-and-configuration-images/image4.png "새 앱 Id를 만들기 위한 양식")](in-app-purchase-basics-and-configuration-images/image4.png#lightbox)

에 대 한 적절 한 값을 입력 합니다 *설명을*이므로이 앱 ID 목록에서 쉽게 식별할 수 있습니다. 에 대 한 합니다 *앱 ID 접두사*, id입니다. 팀 선택

#### <a name="bundle-identifierapp-id-suffix-format"></a>번들 식별자/앱 ID 접미사 형식

에 대해 원하는 모든 문자열을 사용할 수 있습니다 하 **번들 식별자** (으로 계정에 고유한 것) 이지만 Apple 하면 역방향 DNS 형식으로 사용 하지 않고 임의 문자열로 것이 좋습니다. 하지만이 기사에 나와 있는 샘플 응용 프로그램 유효 (경우에 Apple 권장 하지 않습니다) my_store_example 같은 식별자를 사용 하는 것 번들 식별자에 대 한 com.xamarin.storekit.testing를 사용 합니다.

> [!IMPORTANT]
> 하지만 또한 Apple에 와일드 카드 별표의 끝에 추가할 수 있습니다는 **번들 식별자** 단일 앱 ID를 여러 응용 프로그램에 대 한 사용할 수 있도록 _AppPurchase에대한와일드카드앱Id를사용할수없습니다_. 와일드 카드 번들 식별자 com.xamarin.* 않을 예제

#### <a name="enabling-app-services"></a>App Services를 사용 하도록 설정

사실은 **인 앱 구매** 서비스 목록에서 자동으로 활성화 됩니다.

 [![](in-app-purchase-basics-and-configuration-images/image5.png "인 앱 구매 서비스 목록에서 자동으로 설정 됩니다.")](in-app-purchase-basics-and-configuration-images/image5.png#lightbox)

#### <a name="provisioning-profiles"></a>프로비전 프로필

일반적으로 앱에서 바로 구매에 대해 설정한 앱 ID를 선택 하 고으로 개발 및 프로덕션 프로 비전 프로필을 만듭니다. 참조를 [iOS 장치 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 하 고 [앱 스토어에 게시](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) 자세한 가이드입니다.

## <a name="itunes-connect"></a>iTunes Connect

클릭 **My Apps** 에서 iTunes Connect 만들거나 iOS 응용 프로그램 항목을 편집 합니다. 응용 프로그램 개요 페이지는 다음과 같습니다.

 [![](in-app-purchase-basics-and-configuration-images/image6.png "응용 프로그램 개요 페이지")](in-app-purchase-basics-and-configuration-images/image6.png#lightbox)

클릭 **앱에서 바로 구매** 만들거나 판매에 대 한 제품을 편집 합니다. 이 스크린샷은 이미 추가 하는 여러 제품을 사용 하 여 샘플 앱을 보여 줍니다.

 [![](in-app-purchase-basics-and-configuration-images/image7.png "이미 추가 하는 여러 제품을 사용 하 여 샘플 앱")](in-app-purchase-basics-and-configuration-images/image7.png#lightbox)

새 제품을 추가 하는 프로세스에는 두 단계가 있습니다.

1.   제품 유형을 선택 합니다. [![](in-app-purchase-basics-and-configuration-images/image8.png "제품 유형 선택")](in-app-purchase-basics-and-configuration-images/image8.png#lightbox) 
2.   제품의 특성을 제품 Id를 포함 하 여, 가격 책정 계층 및 지역화 된 설명을 입력 합니다. [![](in-app-purchase-basics-and-configuration-images/image9.png "제품 특성을 입력합니다.")](in-app-purchase-basics-and-configuration-images/image9.png#lightbox)

각 앱에서 바로 구매 제품에 필요한 필드는 아래 설명 되어 있습니다.


### <a name="reference-name"></a>참조 이름

참조 이름은; 사용자에 게 표시 되지 않습니다. 내부에 사용 되 고 iTunes Connect에에서만 표시 됩니다.

### <a name="product-id-format"></a>제품 ID 형식

제품 식별자에는 영숫자 (A-z, a-z 0-9)만 포함할 수 있습니다 (_), 밑줄 및 마침표 (.) 문자입니다. 사용자 식별자에 대 한 모든 문자열을 사용할 수 있습니다, 있지만 Apple 역방향 DNS 형식으로 권장 합니다. 예를 들어, 샘플 응용 프로그램에서는이 번들 식별자를 사용합니다.

 `com.xamarin.storekit.testing`

따라서 인 앱 구매 제품을 식별 하는 규칙은 다음과 같을 수 있습니다.

```csharp
com.xamarin.storekit.testing.consume5credits
com.xamarin.storekit.testing.consume10credits
com.xamarin.storekit.testing.sepia
com.xamarin.storekit.testing.greyscale
```

이러한 명명 규칙이 적용 되지 않습니다 제품을 관리할 수 있도록 권장 하기만 하면 됩니다. 또한 동일한 역방향 DNS 규칙 다음에 불구 하 고 제품 식별자를 *무관* 번들 식별자에는 필요가 없습니다 동일한 문자열로 시작 합니다. 여전히 (경우에 Apple 권장 하지 않습니다) photo_product_greyscale 같은 식별자를 사용 하려면 유효한 것입니다.

제품 ID 사용자에 게 표시 되지 않지만 응용 프로그램 코드에서 제품을 참조 하는 것입니다.

### <a name="product-type"></a>제품 유형

5 가지 방법으로 앱에서 바로 구매 제품을 제공할 수 있습니다.

1.  **읽은** – 가지 '사용 을' 예: 플레이어 수 소비 하는 게임 내 통화. 사용자가 수행 하는 백업/복원 하거나 그렇지 않은 경우 장치의 새로 고쳐지지, 사용할 수 있는 트랜잭션 않습니다 가져오기 복원 되지도 (제공 하는 효과적으로 플레이어 다시 동일한이 점으로). 응용 프로그램 코드는 트랜잭션이 완료 되는 즉시 '소모품' 수 있도록 확인 해야 합니다.
1.  **비-사용할 수 있는** – 디지털 잡지 문제를 등 게임 수준을 한 번 구매한 사용자 '소유'는 제품입니다.
1.  **자동 갱신 구독** –만 실제 잡지를 구독, 같은 구독 기간이 끝날 때 Apple 자동으로 다시 고객 청구 및 구독 기간을 영구적으로 또는 고객까지 명시적으로 확장 이 취소합니다. 이것이 Newsstand 앱에 대 한 기본 결제 방법 (실제로 지 원하는 앱이 결제 방법을 Newsstand 배포에 대 한 승인으로).
1.  **무료 구독** – Newsstand 지원 앱에만 제공할 수 있습니다 하 고 모든 장치에서 고객 구독 콘텐츠에 액세스할 수 있습니다. 사용 가능한 구독이 만료 되지 않도록 합니다.
1.  **비 갱신 구독** – 판매 사진 보관에 대 한 한 달의 액세스와 같은 정적 콘텐츠에 대 한 시간 제한 액세스를 사용 해야 합니다.


 *이 문서에는 현재 (사용할 수 있 및 비에서 사용할 수 있는)만 첫 번째 두 개의 제품 유형을 설명합니다.*

 <a name="Price_Tiers" />

### <a name="price-tiers"></a>가격 계층

앱 스토어 수 없다는 제품에는 임의의 가격 선택 – Apple에서 선택할 수 있는 고정된 가격 계층을 제공 합니다. 가격은 각 통화에 고정 및 Apple 권리를 (예를 들어, 특정 통화 사이의 미국 달러 환율 상대 속도 지속적으로 변경) 한 후 상대 가격을 조정 합니다.

Apple는 원하는 통화/가격에 대 한 올바른 계층을 선택할 수 있도록 가격 매트릭스를 제공 합니다. 가격 행렬 (2012 년 8 월)의 일부는 다음과 같습니다.

 [![](in-app-purchase-basics-and-configuration-images/image10.png "2012 년 8 월 가격 행렬의 발췌")](in-app-purchase-basics-and-configuration-images/image10.png#lightbox)

(2013 년 6 월)를 작성할 당시에는 USD에서 87 계층 USD 999.99에는 0.99입니다. 고객에 게 요금을 지불 합니다 및 또한 apple – 받게이 용량은 해당 30% 요금이 덜 되며 또한 모든 세금 (이때는 미국 및 캐나다 판매자가 70 c는 99 c p에 대 한 예제에서를 수집 하는 데 필요한 가격 책정 행렬에 가격 표시 제품, 오스트레일리아 판매자로 인해 63 c만 수신 하는 동안 ' 상품 &amp; 서비스 세금 ' 판매 가격에 따라 징수).

언제 든 지 나중에 적용 되는 예약 된 가격 변경을 비롯 한 제품의 가격 책정을 업데이트할 수 있습니다. 이 스크린샷에서 미래 날짜가 지정 된 가격 변경을 추가 하는 방법을 보여 줍니다. – 가격을 일시적으로 변경할 계층 1에서에서 3 계층으로 9 월만 합니다.

 [![](in-app-purchase-basics-and-configuration-images/image11.png "여기서 가격을 일시적으로 변경할 계층 1에서에서 3 계층으로 9 월만 미래 날짜가 지정 된 가격 변경")](in-app-purchase-basics-and-configuration-images/image11.png#lightbox)

### <a name="free-products-not-supported"></a>무료 제품 지원 되지 않습니다

Apple에서는 Newsstand 앱에 대 한 특별 한 무료 구독 옵션을 제공 하지만 다른 앱에서 바로 구매 형식에 대 한 0 개 (무료) 가격을 설정 하는 것이 불가능 합니다. 편집할 수 있지만 (ie. 낮은) 판매 프로 모션 가격으로 앱 내 구매 '무료' iTunes Connect 통해 만들 수 없습니다.

### <a name="localization"></a>지역화

ITunes Connect에서에서 지원 되는 언어의 여러 다른 이름 및 설명 텍스트를 입력할 수 있습니다. 각 언어 추가/편집에 팝업을 통해 수 있습니다.

 [![](in-app-purchase-basics-and-configuration-images/image12.png "추가/편집에 팝업을 통해 각 언어 수 있습니다.")](in-app-purchase-basics-and-configuration-images/image12.png#lightbox)   
   
   
   
 앱에서 제품 정보를 표시 하는 경우 지역화 된 텍스트가입니다 StoreKit 통해 표시할 수 있습니다. 통화 표시는 문서의 뒷부분에 설명 형식을 올바른 기호 및 소수 형식 – 표시도 지역화 해야 합니다.

### <a name="app-store-review"></a>앱 스토어 검토

앱 – 동일 각 제품을 검토 Apple에서 판매에서 이동 하도록 허용 하기 전에 합니다. 제품 이름 또는 설명을, 부적절 한 콘텐츠에 대 한 거부 될 수 있습니다. 또는 선택 했으며 하는 잘못 된 제품 유형 (예: Apple 결정할 수 있습니다. 한 책 이나 잡지 문제 만들었지만 사용할 수 있는 제품 유형을 사용). 제품 사용 후기 앱 검토를 때 만큼 걸릴 수 있습니다.

처음으로 앱 인 앱 구매 (새 앱 또는 기능을 기존 구독에 추가 되었습니다) 여부를 사용 하도록 설정 하는 함께 전송 되도 함께 전송할 일부 제품 선택 해야 합니다. ITunes Connect 포털 묻는 이렇게 하려면이 스크린샷에 표시 된 대로:

 [![](in-app-purchase-basics-and-configuration-images/image13.png "ITunes Connect 포털 에서도 일부 제품을 제출 하 라는 메시지가 나타납니다.")](in-app-purchase-basics-and-configuration-images/image13.png#lightbox)   
   
   
   
 응용 프로그램 및 앱 내 구매는 검토 함께 해당 앱 승인 된 모든 제품 없이 저장소로 이동 하지 않습니다.) (따라서 한 번에 승인 받기 모두에 있도록 합니다.

첫 번째 버전의 앱에서 바로 구매 기능을 사용 하 여 승인 된 후에 제품을 추가 하 고 언제 든 지 검토를 위해 제출 수 있습니다. 수도 있습니다 인 앱 구매를 특정 제품과 함께 새 버전을 제출 하려면 사용 하 여 **버전 세부 정보** 프롬프트를 알 수 있듯이 페이지.

참조 된 [앱 스토어 검토 지침](https://developer.apple.com/appstore/guidelines.html) 자세한 내용은 합니다.

 [2 부-스토어 키트 개요 및 검색 제품 정보](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)
