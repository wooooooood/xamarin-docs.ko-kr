---
title: 앱 내 구매 기본 사항 및 Xamarin.ios의 구성
description: 이 문서에서는 Xamarin.ios의 앱에서의 구매에 대해 설명 하 고 규칙, 구성 및 iTunes Connect에 대 한 관련 정보를 설명 합니다.
ms.prod: xamarin
ms.assetid: 11FB7F02-41B3-2B34-5A4F-69F12897FE10
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 0c24c0f3a57847a7ecf1a1410a1745419517e0c6
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2019
ms.locfileid: "70198864"
---
# <a name="in-app-purchase-basics-and-configuration-in-xamarinios"></a>앱 내 구매 기본 사항 및 Xamarin.ios의 구성

앱에서 바로 구매를 구현 하려면 응용 프로그램이 장치에서 기능 키트 API를 활용 해야 합니다. 서버 키트는 Apple iTunes 서버와의 모든 통신을 관리 하 여 제품 정보를 얻고 트랜잭션을 수행 합니다. 앱 내 구매를 위해 프로 비전 프로필을 구성 해야 하 고, iTunes Connect에 제품 정보를 입력 해야 합니다.

 [![](in-app-purchase-basics-and-configuration-images/image1.png "이 차트에 표시 된 것 처럼 기능 키트는 Apple과의 모든 통신을 관리 합니다.")](in-app-purchase-basics-and-configuration-images/image1.png#lightbox)

앱 스토어를 사용 하 여 앱 내 구매를 제공 하려면 다음 설정 및 구성이 필요 합니다.

- **ITunes Connect** – 제품을 구성 하 여 구매 하 고 테스트 구매에 샌드박스 사용자 계정을 설정 합니다. 또한 사용자를 대신 하 여 수집 된 자금을 다시 만들 수 있도록 Apple에 은행 및 세금 정보를 제공 해야 합니다.
- **IOS 프로 비전 포털** – 번들 식별자를 만들고 앱에 대 한 앱 스토어 액세스를 사용 하도록 설정 합니다.
- **매장 키트** – 제품을 표시 하 고, 제품을 구매 하 고, 트랜잭션을 복원 하기 위해 앱에 코드를 추가 합니다.
- **사용자 지정 코드** – 고객이 구매한 구매를 추적 하 고 구매한 제품이 나 서비스를 제공 합니다. 제품이 서버에서 다운로드 된 콘텐츠 (예: 책 및 잡지 문제)로 구성 되어 있는지 확인 하기 위해 서버 쪽 프로세스를 구현 해야 할 수도 있습니다.


"서버 환경"의 두 가지 저장소 키트는 다음과 같습니다.

- **프로덕션** -실제 money를 사용 하는 트랜잭션입니다. Apple에서 제출 하 고 승인한 응용 프로그램을 통해서만 액세스할 수 있습니다. 앱 내 구매 제품 또한 프로덕션 환경에서 사용 하기 전에 검토 하 고 승인 해야 합니다.
- **Sandbox** – 테스트가 발생 하는 위치입니다. 제품을 만든 후 바로 여기에서 사용할 수 있습니다. 승인 프로세스는 프로덕션 환경에만 적용 됩니다. 샌드박스에서 트랜잭션을 수행 하려면 테스트 사용자 (실제 Apple Id 아님)가 필요 합니다.

## <a name="in-app-purchase-rules"></a>앱 내 구매 규칙

앱 내에서 디지털 제품 또는 서비스에 대 한 다른 형식의 지불을 수락 하거나 앱 내에서 사용자를 언급 하거나 사용자를 참조할 수는 없습니다. 즉, 앱 내 구매가 가장 적절 한 지불 메커니즘인 경우 신용 카드 또는 PayPal을 허용할 수 없습니다. 앱 외부에서 디지털 제품을 구매 하는 특별 한 경우가 있지만 앱에서 사용 하는 경우 (예: 특정 "로그인"과 연결 된 웹 사이트에서 설명서를 구매 하 고 앱에서 "로그인"을 사용 하 여) 사용자가 구매한 책에 액세스할 수 있습니다.
이러한 방식으로 작동 하는 응용 프로그램은 외부 구매 기능을 언급 하거나 연결할 수 없습니다. 개발자는 전자 메일 마케팅 이나 다른 직접 채널을 통해 다른 방식으로이 기능을 사용자에 게 전달 해야 합니다.

그러나 실제 상품에 대해 앱에서 바로 구매를 사용할 수 없기 때문에이 경우 대체 지불 메커니즘 (예: 신용 카드, PayPal)를 다운로드 합니다.

Apple은 판매 되기 전에 모든 제품을 승인 해야 합니다.-이름, 설명 및 ' 제품 '의 스크린샷 검토에 필요 합니다. 제품 검토 시간은 응용 프로그램 검토와 동일 합니다.

제품에 대 한 가격을 선택할 수 없습니다. Apple에서 지 원하는 각 국가/통화에 특정 값이 있는 ' 가격 책정 계층 '만 선택할 수 있습니다. 다른 시장에서 다른 가격 책정 계층을 사용할 수 없습니다.

## <a name="configuration"></a>Configuration

앱 내 구매 코드를 작성 하기 전에 iTunes Connect ( [itunesconnect.apple.com](http://itunesconnect.apple.com)) 및 IOS 프로 비전 포털 ( [developer.apple.com/iOS](https://developer.apple.com/iOS))에서 일부 설정 작업을 수행 해야 합니다.

코드를 작성 하기 전에 다음 세 단계를 완료 해야 합니다.

- **Apple 개발자 계정** – 은행에 세금 정보를 제출 합니다.
- **IOS 프로 비전 포털** – 앱에 유효한 앱 ID (별표 *가 있는 와일드 카드 아님)가 있고 앱 구매를 사용 하도록 설정 되어 있는지 확인 합니다.
- **ITunes Connect 응용 프로그램 관리** – 응용 프로그램에 제품을 추가 합니다.


### <a name="apple-developer-account"></a>Apple 개발자 계정

무료 앱을 빌드 및 배포 하려면 [ITunes Connect](https://itunesconnect.apple.com)에서 매우 적은 구성이 필요 하지만 유료 앱 또는 앱 내 구매를 판매 하려면 금융 및 세금 정보를 사용 하 여 Apple에 제공 해야 합니다. 여기에 표시 된 주 메뉴에서 **계약, 세금 및 뱅킹** 을 클릭 합니다.

 [![](in-app-purchase-basics-and-configuration-images/image2.png "주 메뉴에서 계약, 세금 및 뱅킹을 클릭 합니다.")](in-app-purchase-basics-and-configuration-images/image2.png#lightbox)

개발자 계정에는 다음 스크린샷에 표시 된 것 처럼 **IOS 유료 응용 프로그램** 계약이 적용 되어 있어야 합니다.

 [![](in-app-purchase-basics-and-configuration-images/image3.png "개발자 계정에 iOS 유료 응용 프로그램 계약이 적용 되어 있어야 합니다.")](in-app-purchase-basics-and-configuration-images/image3.png#lightbox)

**IOS 유료 응용 프로그램** 계약이 있는 경우에만 모든 기능 키트 기능을 테스트할 수 있습니다.-Apple에서 **계약, 세금 및 뱅킹** 정보를 처리할 때까지 코드에서 사용자 키트 호출이 실패 합니다.

### <a name="ios-provisioning-portal"></a>iOS 프로비전 포털

새 응용 프로그램은 **IOS 프로 비전 포털**의 **앱 id** 섹션에 설정 되어 있습니다. 새 앱 ID를 만들려면 [Ios 프로 비전 포털의 구성원 센터](https://developer.apple.com/membercenter/index.action)로 이동 하 여 포털의 **인증서, 식별자 및 프로필** 섹션으로 이동 하 고 *ios 앱*에서 **식별자** 를 클릭 합니다. 그런 다음 오른쪽 위에 있는 "+"를 클릭 하 여 새 앱 ID를 생성 합니다.


새 **앱 id** 를 만들기 위한 양식

 다음과 같습니다.

 [![](in-app-purchase-basics-and-configuration-images/image4.png "새 앱 Id를 만들기 위한 양식")](in-app-purchase-basics-and-configuration-images/image4.png#lightbox)

목록에서이 앱 ID를 쉽게 식별할 수 있도록 *설명*에 적절 한 항목을 입력 합니다. *앱 Id 접두사*에서 팀 id를 선택 합니다.

#### <a name="bundle-identifierapp-id-suffix-format"></a>번들 식별자/앱 ID 접미사 형식

사용자의 계정에서 고유 하기만 하면 **번들 식별자** 에 대해 원하는 모든 문자열을 사용할 수 있지만 Apple에서는 임의의 문자열을 사용 하는 대신 역방향 DNS 형식을 따르는 것이 좋습니다. 이 문서와 함께 제공 되는 샘플 응용 프로그램은 my_store_example를 사용 합니다. 번들 식별자에 대 한 테스트를 사용 하는 것이 좋습니다. 그러나 Apple에서 권장 하지 않는 경우에도 동일한 식별자를 사용할 수 있습니다.

> [!IMPORTANT]
> 또한 Apple에서는 단일 앱 ID를 여러 응용 프로그램에 사용할 수 있도록 **번들 식별자** 의 끝에 와일드 카드 별표가 추가 될 수 있지만, _와일드 카드 앱 Id는 apppurchase에 사용할 수 없습니다_. 와일드 카드 번들 식별자의 예는 com. xamarin. * 일 수 있습니다.

#### <a name="enabling-app-services"></a>App Services 사용

**앱 내 구매** 는 서비스 목록에서 자동으로 사용 하도록 설정 됩니다.

 [![](in-app-purchase-basics-and-configuration-images/image5.png "앱 내 구매는 서비스 목록에서 자동으로 사용 하도록 설정 됩니다.")](in-app-purchase-basics-and-configuration-images/image5.png#lightbox)

#### <a name="provisioning-profiles"></a>프로비전 프로필

일반적으로 앱 내 구매에 대해 설정한 앱 ID를 선택 하 여 개발 및 프로덕션 프로 비전 프로필을 만듭니다. 자세한 내용은 [IOS 장치 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 및 [앱 스토어에 게시](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) 가이드를 참조 하세요.

## <a name="itunes-connect"></a>iTunes Connect

ITunes Connect에서 **내 앱** 을 클릭 하 여 iOS 응용 프로그램 항목을 만들거나 편집 합니다. 응용 프로그램 개요 페이지가 다음과 같이 표시 됩니다.

 [![](in-app-purchase-basics-and-configuration-images/image6.png "응용 프로그램 개요 페이지")](in-app-purchase-basics-and-configuration-images/image6.png#lightbox)

**앱 내 구매** 를 클릭 하 여 판매 제품을 만들거나 편집 합니다. 이 스크린샷에서는 여러 제품이 이미 추가 된 샘플 앱을 보여 줍니다.

 [![](in-app-purchase-basics-and-configuration-images/image7.png "여러 제품이 이미 추가 된 샘플 앱")](in-app-purchase-basics-and-configuration-images/image7.png#lightbox)

새 제품을 추가 하는 프로세스에는 두 단계가 있습니다.

1. 제품 유형 선택:  [![](in-app-purchase-basics-and-configuration-images/image8.png "제품 유형 선택")](in-app-purchase-basics-and-configuration-images/image8.png#lightbox) 
2. 제품 Id, 가격 책정 계층 및 지역화 된 설명을 포함 하 여 제품의 특성을 입력 합니다.  [![](in-app-purchase-basics-and-configuration-images/image9.png "Products 특성 입력")](in-app-purchase-basics-and-configuration-images/image9.png#lightbox)

각 앱 내 구매 제품에 필요한 필드는 아래에 설명 되어 있습니다.


### <a name="reference-name"></a>참조 이름

참조 이름은 사용자에 게 표시 되지 않습니다. 내부 사용을 위한 것 이며 iTunes Connect에만 표시 됩니다.

### <a name="product-id-format"></a>제품 ID 형식

제품 식별자에는 영숫자 (a-z, a-z, 0-9), 밑줄 (_) 및 마침표 (.) 문자만 사용할 수 있습니다. 식별자에 대해 임의의 문자열을 사용할 수 있지만 Apple에서는 역방향 DNS 형식을 권장 합니다. 예를 들어 샘플 응용 프로그램은 다음 번들 식별자를 사용 합니다.

 `com.xamarin.storekit.testing`

따라서 앱 내 구매 제품을 식별 하는 규칙은 다음과 같습니다.

```csharp
com.xamarin.storekit.testing.consume5credits
com.xamarin.storekit.testing.consume10credits
com.xamarin.storekit.testing.sepia
com.xamarin.storekit.testing.greyscale
```

이 명명 규칙은 적용 되지 않고 제품을 관리 하는 데 도움이 되는 권장 사항입니다. 또한 동일한 역방향 DNS 규칙에도 불구 하 고, 제품 식별자는 번들 식별자와 *관련이* 없으며 동일한 문자열로 시작 하는 데 필요 하지 않습니다. Photo_product_greyscale와 같은 식별자를 사용할 수 있습니다 (Apple에서 권장 하지 않는 경우에도).

제품 ID는 사용자에 게 표시 되지 않지만 응용 프로그램 코드에서 제품을 참조 하는 데 사용 됩니다.

### <a name="product-type"></a>제품 유형

앱에서 제공 하는 다음과 같은 5 가지 유형의 앱 구매 제품이 있습니다.

1. 사용 가능 – 플레이어에서 소비할 수 있는 게임 내 통화와 같이 ' 사용 ' 되는 항목입니다. 사용자가 백업/복원을 수행 하거나 장치를 새로 고쳐야 하는 경우에는 사용 가능한 트랜잭션도 복원 되지 않습니다 (효과적으로 플레이어에 게 동일한 혜택을 제공 함). 응용 프로그램 코드는 트랜잭션이 완료 되는 즉시 ' 사용할 수 있는 항목 '을 제공 해야 합니다.
1. **사용 불가능** – 사용자가 구매한 제품 (예: digital magazine 문제 또는 게임 수준)을 구매한 제품입니다.
1. **자동 갱신 가능한 구독** -실제 매거진 구독과 마찬가지로 구독 기간이 끝나면 Apple에서 자동으로 고객에 게 다시 요금을 청구 하 고 구독 기간을 영구적으로 또는 고객이 명시적으로 취소할 때까지 연장 합니다. 이는 Newsstand apps에 대 한 기본 지불 방법입니다 (실제로 앱은 Newsstand 배포에 대해이 결제 방법을 승인 하도록 지원 해야 함).
1. **무료 구독** – Newsstand 사용 앱 에서만 제공할 수 있으며, 고객은 모든 장치에서 구독 콘텐츠에 액세스할 수 있습니다. 무료 구독은 만료 되지 않습니다.
1. **갱신 되지 않는 구독** – 한 달의 사진 보관에 대 한 액세스와 같이 고정 콘텐츠에 시간 제한 된 액세스를 판매 하는 데 사용 해야 합니다.


 *이 문서에는 현재 처음 두 가지 제품 유형 (사용할 때 사용할 경우)만 포함 되어 있습니다.*

 <a name="Price_Tiers" />

### <a name="price-tiers"></a>가격 책정 계층

앱 스토어에서는 제품에 대해 임의의 가격을 선택할 수 없습니다. Apple에서 선택할 수 있는 고정 가격 계층을 제공 합니다. 가격은 각 통화로 고정 되 고, Apple은 상대적 가격을 조정할 수 있는 권리를 보유 합니다 (예: 특정 통화와 미국 달러 간의 상대적 외부 환율이 지속적으로 변경 된 후).

Apple은 원하는 통화/가격에 대 한 올바른 계층을 선택 하는 데 도움이 되는 가격 매트릭스를 제공 합니다. 가격 책정 행렬 (8 월 2012)의 발췌는 다음과 같습니다.

 [![](in-app-purchase-basics-and-configuration-images/image10.png "가격 책정 행렬의 발췌 8 월 2012")](in-app-purchase-basics-and-configuration-images/image10.png#lightbox)

(6 월 2013) 작성 시점에 USD 0.99에서 USD 999.99 까지의 87 계층이 있습니다. 가격 책정 매트릭스는 고객이 지불 하는 가격 및 Apple에서 받게 되는 금액을 보여 줍니다 .이 비용은 30% 요금이 적고 수집 하는 데 필요한 모든 지역 세금이 있습니다 (미국 및 캐나다 판매자가 99 c p에 대해 70c를 수신 함). roduct는 반면 오스트레일리아 판매자는 판매 가격의 ' 상품 &amp; 서비스 세금 ' levied 인해 63c만 받습니다.

향후 날짜에 적용 되는 예정 된 가격 변경을 포함 하 여 언제 든 지 제품의 가격을 업데이트할 수 있습니다. 이 스크린샷에서는 이후 날짜의 가격 변화가 추가 되는 방식을 보여 줍니다. 가격은 9 월에 대 한 계층 1에서 계층 3으로 일시적으로 변경 됩니다.

 [![](in-app-purchase-basics-and-configuration-images/image11.png "월의 월에 대 한 가격이 일시적으로 계층 1에서 계층 3으로 변경 되는 이후 날짜가 변경 됨")](in-app-purchase-basics-and-configuration-images/image11.png#lightbox)

### <a name="free-products-not-supported"></a>무료 제품은 지원 되지 않음

Apple은 Newsstand apps에 대해 특별 한 무료 구독 옵션을 제공 했지만 다른 앱 내 구매 유형에 대해서는 0 (무료) 가격을 설정할 수 없습니다. 판매 홍보를 위해 (더 낮은 가격) 가격을 편집할 수 있지만 iTunes Connect를 통해 앱에서 ' 무료 '를 구매할 수 없습니다.

### <a name="localization"></a>지역화

ITunes Connect에서 지원 되는 여러 언어에 대해 다른 이름 및 설명 텍스트를 입력할 수 있습니다. 팝업을 통해에서 각 언어를 추가 하거나 편집할 수 있습니다.

 [![](in-app-purchase-basics-and-configuration-images/image12.png "각 언어는 팝업을 통해에서 추가/편집할 수 있습니다.")](in-app-purchase-basics-and-configuration-images/image12.png#lightbox)   
   
   
   
 앱에 제품 정보를 표시 하면 사용자가 사용자 키트 키트를 통해 지역화 된 텍스트를 표시할 수 있습니다. 올바른 기호 및 10 진수 형식을 표시 하도록 통화 표시도 지역화 해야 합니다 .이 형식은 문서의 뒷부분에서 다룹니다.

### <a name="app-store-review"></a>앱 스토어 검토

앱과 동일 – 판매 되기 전에 Apple에서 각 제품을 검토 합니다. 이름이 나 설명에서 부적절 한 콘텐츠에 대 한 제품이 거부 될 수도 있고, Apple에서 잘못 된 제품 유형 (예: 책 또는 잡지 문제를 만들어졌지만 사용 가능한 제품 유형을 사용 했습니다. 제품 검토는 앱 검토를 수행 하는 동안에만 수행할 수 있습니다.

앱을 처음으로 구매할 때 앱을 처음으로 제출 하는 경우 (새 앱이 든, 기존 앱에 기능이 추가 되었는지 여부에 관계 없이) 다른 제품을 사용 하 여 제출할 수도 있습니다. ITunes Connect 포털에는 다음 스크린샷에 표시 된 것 처럼이 작업을 수행 하 라는 메시지가 표시 됩니다.

 [![](in-app-purchase-basics-and-configuration-images/image13.png "ITunes Connect 포털에 일부 제품을 제출 하 라는 메시지가 표시 됩니다.")](in-app-purchase-basics-and-configuration-images/image13.png#lightbox)   
   
   
   
 응용 프로그램 및 앱 내 구매는 함께 검토 되므로 앱이 승인 된 제품이 없으면 앱이 스토어로 이동 하지 않습니다.

앱 내 구매 기능이 포함 된 첫 번째 버전이 승인 된 후에는 언제 든 지 추가 제품을 추가 하 고 제출할 수 있습니다. 표시 된 것 처럼 **버전 세부 정보** 페이지를 사용 하 여 특정 앱 내 구매 제품과 함께 새 버전을 제출 하도록 선택할 수도 있습니다.

자세한 내용은 [앱 스토어 검토 지침](https://developer.apple.com/appstore/guidelines.html) 을 참조 하세요.

 [2 부-스토어 키트 개요 및 제품 정보 검색](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)
