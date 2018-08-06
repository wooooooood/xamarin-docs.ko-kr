---
title: 앱에서 바로 구매 기본 사항 및 구성에 Xamarin.iOS
description: 이 문서에서 규칙, 구성 및 iTunes Connect에 대 한 관련 정보에 논의 Xamarin.iOS 앱에서 바로 구매를 설명 합니다.
ms.prod: xamarin
ms.assetid: 11FB7F02-41B3-2B34-5A4F-69F12897FE10
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 9ded160ad4b31346c400e63d739a3dc21f6304d3
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787245"
---
# <a name="in-app-purchase-basics-and-configuration-in-xamarinios"></a>앱에서 바로 구매 기본 사항 및 구성에 Xamarin.iOS

앱에서 바로 구매를 구현 하려면 응용 프로그램을 장치에는 StoreKit API를 사용 합니다. StoreKit 제품 정보를 가져오고 트랜잭션을 수행 하는 Apple iTunes 서버와의 모든 통신을 관리 합니다. 앱에서 바로 구매에 대 한 프로 비전 프로필을 구성 해야 하 고 iTunes Connect에서에서 제품 정보를 입력 해야 합니다.

 [![](in-app-purchase-basics-and-configuration-images/image1.png "StoreKit이이 차트에 표시 된 대로 apple의 모든 통신을 관리")](in-app-purchase-basics-and-configuration-images/image1.png#lightbox)

앱에서 바로 구매를 제공 하는 앱 스토어를 사용 하 여 다음과 같은 설정 및 구성 필요 합니다.

-  **iTunes Connect** – 판매 제품을 구성 하 고 구매를 테스트 하려면 샌드박스 사용자 계정 설정 합니다. 또한을 제공 해야 귀하의 은행 및 세금 정보 Apple에 사용자 대신 수집 자금을 remit 수 있도록 합니다.
-   **iOS 프로 비전 포털** – 번들 식별자를 만들고 응용 프로그램에 대 한 앱 스토어 액세스를 활성화 합니다.
-  **키트 저장** – 제품을 표시, 제품 구매 및 트랜잭션을 복원에 대 한 앱에 코드를 추가 합니다.
-  **사용자 지정 코드** – 구매 고객 정보가 추적을 제공 하는 제품 또는 서비스 구입 했습니다. 콘텐츠 서버 (예: 설명서 및 잡지)에서 다운로드 한 제품으로 구성 하는 경우 수신 확인의 유효성을 검사 하는 서버 쪽 프로세스를 구현할 수도 할 수 있습니다.


두 개의 저장소 키트 "서버 환경" 가지가 있습니다.

-  **프로덕션** – 실제 비용을 사용 하 여 트랜잭션. 전송 되 고 Apple에서 승인 된 응용 프로그램을 통해 액세스할 수 있습니다. 또한 앱에서 바로 구매 제품 검토 고 프로덕션 환경에서 사용 가능한 상태가 되기 전에 승인 해야 합니다.
-  **샌드박스** 테스트 발생 하는 경우 – 합니다. 제품은 여기에 있습니다 (승인 프로세스에만 적용 프로덕션 환경)을 만든 후 즉시 합니다. 샌드박스에서 트랜잭션은 트랜잭션을 수행 하는 테스트 사용자 (하지 실제 Apple Id) 필요 합니다.

## <a name="in-app-purchase-rules"></a>앱에서 바로 구매 규칙

응용 프로그램 내 다른 형태의 디지털 제품 또는 서비스에 대 한 지불을 수락 하 또는 언급 하거나 응용 프로그램 내에서 사용자가 참조 수 없습니다. 즉, 가장 적절 한 지불 메커니즘은 앱에서 바로 구매 신용 카드 또는 PayPal 수락할 수 없습니다. 앱 외부 디지털 제품 구매에 대 한 특별 한 경우 않으며 사용 예: 웹 사이트의 특정 "login"와 연결 된 책을 구입 및 "로그인" 응용 프로그램에 대 한 사용자 액세스를 통해 사용 하 여 앱에서 구매한 설명서입니다.
이런 방식이으로 작업 하는 응용 프로그램을 언급 하거나 외부 구매 기능을 연결할 수 없습니다-개발자가 다른 방법으로 (아마도 통해 전자 메일 마케팅 또는 일부 다른 직접 채널) 사용자에 게이 기능을 통신 해야 합니다.

그러나 사용할 수 없으며 앱에서 바로 구매 실제 제품에 대 한 허용 되는 경우 (예: 대체 결제 메커니즘을 사용 한다는 점에서. 신용 카드, PayPal)에서 응용 프로그램 내에서.

Apple에 판매 – 이름, 설명 되는 것 이며 'product'의 스크린샷을 필수 검토용으로 공개 하기 전에 모든 제품을 승인 해야 합니다. 제품 검토 번 검토 응용 프로그램의 경우와 동일합니다.

제품에 대 한 모든 가격을 선택할 수 없습니다.-Apple에서 지 원하는 각 국가/화폐에 특정 값을 가진 '가격 계층' 선택할 수 있습니다. 다양 한 시장에서 여러 가격 계층을 사용할 수 없습니다.

## <a name="configuration"></a>구성

앱에서 바로 구매 코드를 작성 하기 전에 iTunes Connect에서에서 일부 설치 작업을 수행 해야 합니다 ( [itunesconnect.apple.com](http://itunesconnect.apple.com)) 및 iOS 프로 비전 포털 ( [developer.apple.com/iOS](http://developer.apple.com/iOS)).

이 세 단계는 코드를 작성 하기 전에 완료 해야 합니다.

-  **Apple 개발자 계정** – Apple에 귀하의 은행 및 세금 정보를 제출 합니다.
-  **iOS 프로 비전 포털** – 응용 프로그램에 올바른 응용 프로그램 ID를 확인 (하지에 별표 와일드 카드 *에) 하 고 활성화 응용 프로그램에 구매 합니다.
-  **iTunes Connect 응용 프로그램 관리** – 제품 응용 프로그램을 추가 합니다.


### <a name="apple-developer-account"></a>Apple 개발자 계정

빌드 및 무료 앱 배포에 아주 적은 구성이 필요로 [iTunes Connect](https://itunesconnect.apple.com)있지만 판매 유료 앱 또는 앱에서 바로 구매 하려면 Apple 은행 및 세금 정보를 제공할 수 있습니다. 클릭 **계약, 세금 및 뱅킹** 여기에 표시 된 주 메뉴에서:

 [![](in-app-purchase-basics-and-configuration-images/image2.png "계약, 세금 및 주 메뉴에서 뱅킹 클릭")](in-app-purchase-basics-and-configuration-images/image2.png#lightbox)

개발자 계정이 있어야는 **iOS 유료 응용 프로그램** 실제로이 스크린 샷에 표시 된 것 처럼 계약:

 [![](in-app-purchase-basics-and-configuration-images/image3.png "개발자 계정을 유료 응용 프로그램 계약 적용 iOS 있어야 합니다.")](in-app-purchase-basics-and-configuration-images/image3.png#lightbox)

구성할 때까지 모든 StoreKit 기능을 테스트할 수 없습니다는 **iOS 유료 응용 프로그램** 계약 – Apple을 처리할 때까지 코드에서 StoreKit 호출이 실패 합니다 프로그램 **계약, 세금, 및 뱅킹** 정보입니다.

### <a name="ios-provisioning-portal"></a>iOS 프로비전 포털

새 응용 프로그램에서 설정 되는 **의 앱 Id** 섹션은 **iOS 프로 비전 포털**합니다. 새 앱 ID를 만들려면는 [iOS 프로 비전 포털의 회원 센터](https://developer.apple.com/membercenter/index.action)로 이동 **인증서, 식별자 및 프로필** 클릭 하 고 포털의 섹션 **식별자** 아래 *iOS 앱*합니다. 그런 다음 상단 "+" 오른쪽 새 앱 ID를 생성 하려면를 클릭합니다


새로 만들기 폼 **의 앱 Id**

 다음과 같이 보입니다.

 [![](in-app-purchase-basics-and-configuration-images/image4.png "새 앱 Id를 만들기 위한 폼")](in-app-purchase-basics-and-configuration-images/image4.png#lightbox)

에 대 한 적절 한 값을 입력에서 *설명*이므로 목록에이 응용 프로그램 ID를 쉽게 식별할 수 있습니다. 에 대 한는 *앱 ID 접두사*, id입니다. 팀 선택

#### <a name="bundle-identifierapp-id-suffix-format"></a>번들 식별자/응용 프로그램 ID 접미사 형식

에 대해 원하는 모든 문자열을 사용할 수 있습니다 프로그램 **번들 식별자** (으로 계정에서 고유함)을 Apple 있습니다 역방향 DNS 형식에 따라 보다는 임의의 문자열을 사용 하는 것이 좋습니다. 이 문서를 함께 제공 되는 샘플 응용 프로그램 사용 하 여 com.xamarin.storekit.testing 번들 식별자에 대 한 (경우에 Apple 권장 하지 않습니다) my_store_example 같은 식별자를 사용 하는 데 유효한 동일 하 게 있을 것입니다.

> [!IMPORTANT]
> 그러나 또한 Apple에 와일드 카드 별표의 끝에 추가할 수 있습니다는 **번들 식별자** 여러 응용 프로그램에 대 한 단일 응용 프로그램 ID를 사용할 수 있도록 _AppPurchase에와일드카드의앱Id를사용할수없습니다_. 와일드 카드 번들 식별자 com.xamarin.* 수도 예

#### <a name="enabling-app-services"></a>사용할 수 있도록 응용 프로그램 서비스

**앱에서 바로 구매** 서비스 목록에서 자동으로 설정 됩니다.

 [![](in-app-purchase-basics-and-configuration-images/image5.png "서비스 목록에서 앱에서 바로 구매를 자동으로 설정 됩니다.")](in-app-purchase-basics-and-configuration-images/image5.png#lightbox)

#### <a name="provisioning-profiles"></a>프로비전 프로필

일반적으로 앱에서 바로 구매에 대 한 설정한 응용 프로그램 ID 선택 적절 하 게 하는 대로 개발 및 프로덕션 프로 비전 프로필을 만듭니다. 참조는 [iOS 장치를 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 및 [앱 스토어에 게시](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) 자세한 정보에 대 한 가이드입니다.

## <a name="itunes-connect"></a>iTunes Connect

클릭 **My Apps** 에서 iTunes 만들기 또는 편집 iOS 응용 프로그램 항목에 연결 합니다. 응용 프로그램의 개요 페이지는 다음과 같습니다.

 [![](in-app-purchase-basics-and-configuration-images/image6.png "응용 프로그램의 개요 페이지")](in-app-purchase-basics-and-configuration-images/image6.png#lightbox)

클릭 **앱에서 바로 구매** 를 작성 하거나 편집할 제품 판매 종료 합니다. 이 스크린샷은 여러 제품이 이미 추가 된 샘플 응용 프로그램을 보여 줍니다.

 [![](in-app-purchase-basics-and-configuration-images/image7.png "이미 추가 하는 여러 제품을 사용한 샘플 앱")](in-app-purchase-basics-and-configuration-images/image7.png#lightbox)

새 제품을 추가 하는 프로세스에 두 개의 단계가 있습니다.

1.   제품 유형 선택: [![](in-app-purchase-basics-and-configuration-images/image8.png "제품 종류를 선택 합니다.")](in-app-purchase-basics-and-configuration-images/image8.png#lightbox) 
2.   가격 책정 계층 및 지역화 된 설명, 제품 Id를 포함 한 제품의 속성을 입력: [![](in-app-purchase-basics-and-configuration-images/image9.png "제품 특성을 입력")](in-app-purchase-basics-and-configuration-images/image9.png#lightbox)

각 앱에서 바로 구매 제품에 필요한 필드는 다음과 같습니다.


### <a name="reference-name"></a>참조 이름

참조 이름이; 사용자에 게 표시 되지 않습니다. 내부에 사용 되며 iTunes Connect에서에서에 나타납니다.

### <a name="product-id-format"></a>제품 ID 형식

제품 식별자에는 영숫자 (A-z, a ~ z, 0-9)만 포함할 수 밑줄 (_) 및 마침표 (.) 문자. 사용자 식별자에 대 한 모든 문자열을 사용 하 여 있지만 Apple 역방향 DNS 형식을 권장 합니다. 예를 들어 샘플 응용 프로그램에서는이 번들 Id를 사용합니다.

 `com.xamarin.storekit.testing`

따라서 앱에서 바로 구매 제품을 식별 하는 규칙은 다음과 같이 합니다.

```csharp
com.xamarin.storekit.testing.consume5credits
com.xamarin.storekit.testing.consume10credits
com.xamarin.storekit.testing.sepia
com.xamarin.storekit.testing.greyscale
```

이 명명 규칙은 적용 하지 않으면 제품을 관리할 수 있도록 권장 사항을 간단히 합니다. 또한 불구 하 고 동일한 역방향 DNS 규칙을 따르면 제품 식별자가 *무관* 는 번들 식별자에 동일한 문자열로 시작 하는 데 필요하지 없습니다. 여전히 (경우에 Apple 권장 하지 않습니다) photo_product_greyscale 같은 식별자를 사용 하는 데 유효한 것입니다.

제품 ID 사용자에 게 표시 되지 않지만 제품 응용 프로그램 코드에서 참조 하는 데 사용 됩니다.

### <a name="product-type"></a>제품 유형

5 가지 방법으로 앱에서 바로 구매 제품을 제공할 수 있습니다.

1.  **사용 될** – 사항 들 '모두 사용', 게임 내에서 통화 플레이어 수 소비 하는 등입니다. 사용자가 수행 하는 백업/복원, 그렇지 않으면 장치 새로 사용 될 트랜잭션 않습니다 가져올 복원 되지도 (제공 하는 것 효과적으로 플레이어를 다시 동일한 혜택). 응용 프로그램 코드 트랜잭션이 완료 되는 즉시 '소비 가능한 항목'을 제공 해야 합니다.
1.  **비 소모품** – 디지털 잡지 문제 또는 경기 수준 같은 한 번 구매한 사용자 '가 소유한 ' 제품입니다.
1.  **자동 갱신 가능한 구독** – 방금 실제 잡지 구독과 같은 구독 기간이 끝날 때 Apple 자동으로 다시 고객 요금을 하 고 확장 구독 기간 고객 때까지 계속 또는 명시적으로 이 취소합니다. 이 뉴스 스탠드 앱에 대 한 기본 지불 방법 (실제로 지 원하는 앱이 결제 방법을 뉴스 스탠드 배포 승인을 받지으로).
1.  **무료 구독** – 뉴스 스탠드 지원 응용 프로그램에 제공 될 수 하 고 모든 장치에서 구독 콘텐츠에 액세스 하기 위한 고객을 허용 합니다. 무료 구독은 만료 되지 않습니다.
1.  **비 갱신 구독** – 시간이 제한 된 액세스는 사진 보관 파일에 대 한 한 달의 액세스와 같은 정적 콘텐츠를 판매 하는 데 사용 해야 합니다.


 *이 문서는 현재 (소모품 및 비 소모품)만 첫 번째 두 개의 제품 종류를 설명합니다.*

 <a name="Price_Tiers" />

### <a name="price-tiers"></a>가격 계층

앱 스토어 사용 하는 임의의 가격 제품에 대 한 선택 – Apple의 고정된 가격 계층을 선택할 수 있는 제공 합니다. 가격 각 통화에서 수정 및 Apple (예를 들어 특정 통화 사이의 미국 달러 환율 상대 속도가 지속적인된 변경) 이후 상대 가격을 조정할 수 있는 권리를 보유 합니다.

Apple 원하는 통화/가격에 대 한 올바른 계층을 선택할 수 있도록 하기 위해 가격 매트릭스를 제공 합니다. 가격 행렬 (2012 년 8 월)의 일부는 다음과 같습니다.

 [![](in-app-purchase-basics-and-configuration-images/image10.png "2012 년 8 월 가격 매트릭스의 일부")](in-app-purchase-basics-and-configuration-images/image10.png#lightbox)

시점 (2013 년 6 월) 작성에 USD에서 87 계층 USD 999.99에는 0.99입니다. 가격 행렬에서는 가격 고객이 지불 하 고도-Apple에서 수신할이 용량은 해당 30% 충전 덜 고도 모든 로컬 세금은 수집 (이 예에서 미국 및 캐나다 sellers 99 c p에 대 한 70 c를 수신 하는 데 필요한 오스트레일리아 판매자로 인해만 63 c를 수신 하는 동안 구입 시 ' 상품 &amp; 서비스 세금 ' 판매 가격에 따라 징수).

언제 든 지 날짜는 미래의 날짜에 적용 하는 예약 된 가격 변경 내용을 포함 하 여 제품의 가격을 업데이트할 수 있습니다. 이 스크린샷은 미래 날짜가 지정 된 가격 변경을 어떻게 추가 되었는지를 보여 줍니다. – 가격 일시적으로 계층 1로 변경 계층 3 년 9 월에 대 한만:

 [![](in-app-purchase-basics-and-configuration-images/image11.png "여기서 가격 일시적으로 변경 되 계층 1에서에서 3 계층으로 9 월에 대 한만 미래 날짜가 지정 된 가격 변경")](in-app-purchase-basics-and-configuration-images/image11.png#lightbox)

### <a name="free-products-not-supported"></a>무료 제품 지원 되지 않습니다

Apple에 제공 되지만 뉴스 스탠드 앱에 대 한 특별 한 무료 구독 옵션, 다른 앱에서 바로 구매 유형에 대 한 가격 (무료)을 설정 하는 것이 불가능 합니다. 편집할 수는 있지만 (ie. 낮은) 판매 홍보에 대 한 가격을 iTunes Connect 통해 ' 무료' 앱에서 바로 구매를 만들 수 없습니다.

### <a name="localization"></a>지역화

ITunes Connect에서에서 임의 개수의 지원 되는 언어에 대 한 다양 한 이름 및 설명 텍스트를 입력할 수 있습니다. 각 언어 추가/편집에 팝업을 통해 사용할 수 있습니다.

 [![](in-app-purchase-basics-and-configuration-images/image12.png "추가/편집에 팝업을 통해 각 언어 수 있습니다.")](in-app-purchase-basics-and-configuration-images/image12.png#lightbox)   
   
   
   
 응용 프로그램에서 제품 정보를 표시할 때 지역화 된 텍스트 StoreKit 통해 표시할 수 있습니다. 통화 표시도이 문서 뒷부분에 설명 않는 서식 올바른 기호 및 10 진수 서식 – 표시 하기 위해 지역화 해야 합니다.

### <a name="app-store-review"></a>앱 스토어 검토

앱 – 동일 각 제품은 검토 하 여 Apple에 판매를 이동 하도록 허용 하기 전에. 제품 이름이 나 설명의 부적절 한 콘텐츠에 대 한 거부 될 수 있습니다. 또는 Apple 않음 (예: 잘못 된 제품 유형을 선택 했으면 결정할 수 있습니다. 한 책 또는 잡지 문제 만들었지만 사용 될 제품 종류 사용). 제품 사용 후기 응용 프로그램 검토 만큼 걸릴 수 있습니다.

응용 프로그램 앱에서 바로 구매 (새 앱 또는 기존에는 기능이 추가 되었습니다) 여부를 사용 하도록 설정 하는 함께 전송 되는 처음으로 함께 제출 하는 일부 제품 선택 해야 합니다. ITunes Connect 포털 하 라는 메시지가 나타납니다 이렇게이 스크린 샷에 표시 된 것 처럼:

 [![](in-app-purchase-basics-and-configuration-images/image13.png "ITunes Connect 포털 일부 제품을 제출 하 라는 메시지가 나타납니다.")](in-app-purchase-basics-and-configuration-images/image13.png#lightbox)   
   
   
   
 앱에서 바로 구매 및 응용 프로그램을 검토 함께 (하므로 이러한 앱 승인 된 모든 제품 없이 저장소로 이동 하지 않습니다!) 한 번에 승인을 받는 모두 되도록 합니다.

앱에서 바로 구매 기능을 사용 하면 첫 번째 버전 승인 된 후 제품을 더 추가할 수 있으며 언제 든 지 검토를 위해 제출할. 수도 있습니다 제출 특정 앱에서 바로 구매 제품에 함께 새 버전을 사용 하는 **버전 정보** 프롬프트 알 수 있듯이 페이지입니다.

참조는 [앱 스토어 검토 지침](https://developer.apple.com/appstore/guidelines.html) 자세한 정보에 대 한 합니다.

 [파트 2-저장소 키트 개요 및 인쇄용 제품 정보](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)
