---
title: IOS 6에에서 StoreKit 변경
description: 'iOS 6 저장소 키트 API에 두 가지 변경 사항을 소개: iTunes (및 앱 스토어/ibookstore에서)를 표시 하는 기능을 새 앱 및 앱 내에서 제품을 구매 Apple 다운로드 가능한 파일을 호스트 하는 옵션입니다. 이 문서에서는 Xamarin.iOS 사용 하 여 이러한 기능을 구현 하는 방법을 설명 합니다.'
ms.prod: xamarin
ms.assetid: 253D37D7-44C7-D012-3641-E15DC41C2699
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 35bac91e54181753bd1f3fd8b4cf0b851bfa1882
ms.sourcegitcommit: 2eb8961dd7e2a3e06183923adab6e73ecb38a17f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66827499"
---
# <a name="changes-to-storekit-in-ios-6"></a>IOS 6에에서 StoreKit 변경

_iOS 6 저장소 키트 API를 두 가지 변경 사항을 소개: iTunes (및 앱 스토어/ibookstore에서)를 표시 하는 기능을 새 앱 및 앱 내에서 제품을 구매 Apple 다운로드 가능한 파일을 호스트 하는 옵션입니다. 이 문서에서는 Xamarin.iOS 사용 하 여 이러한 기능을 구현 하는 방법을 설명 합니다._

IOS6에서 저장소 키트에 주요 변경 내용은 다음과 같습니다. 이러한 두 가지 새로운 기능

- **앱에서 콘텐츠 표시 및 Purchasing** – 사용자가 구입할 수 및 앱 다운로드, 음악, 서적 및 다른 iTunes 앱을 종료 하지 않고 콘텐츠입니다. 구매 승격만 검토 및 등급을 장려 하 여 자신의 앱에 연결할 수 있습니다.
- **앱 내 구매 호스팅된 콘텐츠** – Apple 저장 되며 파일을 호스트 하는 별도 서버에 대 한 필요가 자동으로 백그라운드에서 다운로드를 지원 하 고는 사용 하면 앱에서 바로 구매 제품에 연관 된 콘텐츠를 제공 합니다. 더 적은 코드를 작성 합니다.

참조 된 [인 앱 구매](~/ios/platform/in-app-purchasing/index.md) 자세하게 StoreKit Api에 대 한 가이드입니다.

## <a name="requirements"></a>요구 사항

이 문서에 설명 된 저장소 키트 기능을 iOS 6 및 Xamarin.iOS 6.0 함께 Xcode 4.5 필요 합니다.

## <a name="in-app-content-display--purchasing"></a>앱에서 콘텐츠 표시 및 구매

IOS에서 새로운 앱 내 구매 기능을 사용 하면 제품 정보를 보거나 구매 및 앱 내에서 제품을 다운로드할 수 있습니다.
이전에 응용 프로그램은 iTunes App Store, 원래 응용 프로그램 사용자는 ibookstore에서 트리거를 해야 합니다. 자동으로이 새로운 기능은 끝날 때 사용자 앱에 게 반환 합니다.

[![](changes-to-storekit-images/image1.png "앱 구입 후 자동으로 돌아가기")](changes-to-storekit-images/image1.png#lightbox)

수 사용 방법의 예입니다.

- **사용자가 앱을 평가 하도록 장려** – 사용자를 평가 하 고 유지 하지 않고도 앱을 검토할 수 있도록 앱 스토어 페이지를 열 수 있습니다.
- **앱 간 승격** – 즉시 구입/다운로드 하는 기능을 사용 하 여 게시할 수 있는 다른 앱을 볼 수 있도록 합니다.
- **찾기 및 콘텐츠 다운로드를 지원할** – 콘텐츠를 앱 검색, 관리 또는 집계 (예: 구입 하는 데 도움이 음악 관련 앱을 수 수백 곡의 재생 목록을 제공 하며 각 곡 앱 내에서 구매를 허용)

한 번의 `SKStoreProductViewController` 표시 되기는 ibookstore에서 iTunes, 앱 스토어에 있는 것 처럼 사용자가 제품 정보를 상호 작용할 수 있습니다. 사용자는 다음 작업을 수행할 수 있습니다.

- (앱)에 대 한 보기 스크린샷
- 샘플 음악이 나 비디오 (음악, TV 쇼 및 동영상)
- 읽기 (및 쓰기) 검토
- 구매 및 다운로드는 뷰 컨트롤러 및 저장소 키트 내에서 완전히 수행 됨.

내에서 일부 옵션은 `SKStoreProductViewController` 은 여전히 force 사용자 앱을 클릭 하면 같은 관련 스토어 앱을 엽니다 **관련 제품** 또는 앱의 **지원** 링크 합니다.

### <a name="skstoreproductviewcontroller"></a>SKStoreProductViewController

모든 앱 내에서 제품을 표시 하는 API는 간단: 하기만 만든 표시 하는 `SKStoreProductViewController`합니다. 만들고 제품을 표시 하려면 다음이 단계를 수행 합니다.

1. 만들기를 `StoreProductParameters` 보기 컨트롤러에 매개 변수를 전달 하는 개체 등을 `productId` 생성자에서.
1. 인스턴스화하는 `SKProductViewController`합니다. 클래스 수준 필드를 할당 합니다.
1. 뷰 컨트롤러의 처리기를 할당할 `Finished` 뷰 컨트롤러를 해제 해야 하는 이벤트입니다. 이 이벤트는 사용자가 취소; 라고 합니다. 또는 그렇지 않은 경우 뷰 컨트롤러 내에서 트랜잭션을 종료 합니다.
1. 호출을 `LoadProduct` 전달 하는 메서드는 `StoreProductParameters` 및 완료 처리기입니다. 완료 처리기는 제품 요청이 성공적으로 확인 하며 그렇다면 제공 된 `SKProductViewController` 모달 형식으로 합니다. 제품을 검색할 수 없는 경우에 적절 한 오류 처리 추가 되어야 합니다.

### <a name="example"></a>예제

*ProductView* 프로젝트를 *StoreKit* 이 기사의 샘플 코드를 구현 하는 `Buy` 메서드는 모든 제품을 허용 하는 Apple id 및 표시를 `SKStoreProductViewController`합니다. 다음 코드는 지정 된 Apple ID에 대 한 제품 정보를 표시합니다.

```csharp
void Buy (int productId)
{
    var spp = new StoreProductParameters(productId);
    var productViewController = new SKStoreProductViewController ();
    // must set the Finished handler before displaying the view controller
    productViewController.Finished += (sender, err) => {
        // Apple's docs says to use this method to close the view controller
        this.DismissModalViewControllerAnimated (true);
    };
    productViewController.LoadProduct (spp, (ok, err) => { // ASYNC !!!
        if (ok) {
            PresentModalViewController (productViewController, true);
        } else {
            Console.WriteLine (" failed ");
            if (err != null)
                Console.WriteLine (" with error " + err);
        }
    });
}
```

앱 실행-다운로드 하거나 구매 완전히 내에서 발생 하는 경우 아래 스크린샷 처럼 표시 된 `SKStoreProductViewController`:

[![](changes-to-storekit-images/image2.png "실행 하는 경우 앱을 다음과 같이 표시 됩니다.")](changes-to-storekit-images/image2.png#lightbox)

### <a name="supporting-older-operating-systems"></a>이전 운영 체제를 지원합니다.

샘플 응용 프로그램에는 이전 버전의 iOS 앱 스토어, iTunes 또는 ibookstore에서 여는 방법을 보여 주는 코드를 포함 합니다. 사용 하 여는 `OpenUrl` 메서드를 제대로 작성 된 엽니다 **itunes.com** URL.

다음과 같이 실행 하려면 코드를 확인 하는 버전 검사를 구현할 수 있습니다.

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (6,0)) {
    // do iOS6+ stuff, using SKStoreProductViewController as shown above
} else {
    // don't do stuff requiring iOS 6.0, use the old syntax 
    // (which will take the user out of your app)
    var nsurl = new NSUrl("http://itunes.apple.com/us/app/angry-birds/id343200656?mt=8");
    UIApplication.SharedApplication.OpenUrl (nsurl);
}
```

### <a name="errors"></a>오류

사용 하는 Apple ID 올바르지 않으면 일종의 네트워크 또는 인증 문제를 의미 하므로 혼동을 줄 수 있습니다 다음 오류가 발생 합니다.

 `Error Domain=SKErrorDomain Code=5 "Cannot connect to iTunes Store"`

### <a name="reading-objective-c-documentation"></a>Objective-c로 문서 읽기

Apple 개발자 포털에서 저장소 키트에 대 한 개발자 protocol – 나타납니다 [SKStoreProductViewControllerDelegate](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKITunesProductViewControllerDelegate_ProtocolRef/Reference/Reference.html) –이 새로운 기능을 기준으로 설명 합니다. 대리자 프로토콜의로 표시 된 하나의 메서드 – productViewControllerDidFinish – 합니다 `Finished` 이벤트에는 `SKStoreProductViewController` Xamarin.iOS에서.

## <a name="determining-apple-ids"></a>Apple Id를 확인합니다.

에 필요한 Apple ID를 `SKStoreProductViewController` 은 *번호* (필요가 "com.xamarin.mwc2012"과 같은 번들 Id와 혼동 해서는). 몇 가지 여러 가지 하려는 표시 하려면 아래에 나열 된 제품에 대 한 Apple ID를 확인할 수 있습니다.

### <a name="itunesconnect"></a>iTunesConnect

게시 하는 응용 프로그램의 경우 찾기 쉬운 것은 **Apple ID** iTunes Connect에서에서:

[![](changes-to-storekit-images/image3.png "ITunes Connect에서에서 Apple ID 찾기")](changes-to-storekit-images/image3.png#lightbox)

 <a name="Search_API" />

### <a name="search-api"></a>검색 API

Apple 앱 스토어, iTunes 및는 ibookstore에서의 모든 제품을 쿼리 하는 동적 검색 API를 제공 합니다. search API에 액세스 하는 방법에 대 한 정보를 찾을 수 있습니다 [Apple의 관련 리소스](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html)이지만 모든 사용자 (방금 등록된 되지 않은 계열사)에 게 API 노출 됩니다. 검색 결과 JSON 구문 분석 될 수는 `trackId` 를 사용 하는 Apple ID `SKStoreProductViewController`.

결과는 앱에서 제품을 렌더링 하는 아트 워크 Url 표시 정보 등의 다른 메타 데이터도 포함 됩니다.

다음은 몇 가지 예입니다.

- **iBooks 앱** – [ http://itunes.apple.com/search?term=ibooks&amp; 엔터티 소프트웨어 =&amp;국가 = us](http://itunes.apple.com/search?term=ibooks&amp;entity=software&amp;country=us)
- **점 및 캥거루 iBook** – [ http://itunes.apple.com/search?term=dot+and+the+kangaroo&amp; 엔터티 전자책 =&amp;국가 = us](http://itunes.apple.com/search?term=dot+and+the+kangaroo&amp;entity=ebook&amp;country=us)

### <a name="enterprise-partner-feed"></a>엔터프라이즈 파트너 피드

Apple에서 다운로드 가능한 데이터베이스 준비 플랫 파일의 형태로, 모든 제품의 전체 데이터 덤프를 사용 하 여 승인 된 파트너를 제공합니다. 에 대 한 액세스에 대 한 자격이 있는지 합니다 [엔터프라이즈 파트너 피드](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-enterprise-partner-feed.html), 데이터 집합에 있는 모든 제품에 대 한 Apple ID를 찾을 수 있습니다.

많은 사용자가 엔터프라이즈 파트너 피드의 멤버인 합니다 [제휴 프로그램](http://www.apple.com/itunes/affiliates) 커미션 제품 판매에서 발생 수를 허용 하는 합니다. `SKStoreProductViewController` (작성 당시) 관련 Id를 지원 하지 않습니다.

### <a name="direct-product-links"></a>직접 제품 링크

미리 보기 URL 링크가 해당 iTunes에서 제품에 대 한 Apple ID는 유추할 수 있습니다.
제품 링크 (앱, 음악 또는 책)에 모든 iTunes에서 시작 하는 URL 부분을 발견 `id` 뒤에 오는 번호를 사용 하 고 있습니다.

IBooks에 대 한 직접 링크는 예를 들어,

```csharp
http://itunes.apple.com/us/app/ibooks/id364709193?mt=8
```

Apple ID 이며 **364709193**합니다. 마찬가지로 MWC2012 앱에 대 한 직접 연결이

```csharp
http://itunes.apple.com/us/app/mwc-2012-unofficial/id496963922?mt=8
```

Apple ID 이며 **496963922**합니다.

## <a name="in-app-purchase-hosted-content"></a>호스트 된 콘텐츠를 앱에서 바로 구매

이러한 파일을 웹 서버에서 호스팅될 사용 앱을 안전 하 게 후를 다운로드 하기 위한 코드를 통합 해야 하 고 사용자 앱에서 구매 하는 경우 다운로드 가능한 콘텐츠 (예: 책 또는 기타 미디어, 게임 레벨 아트로 및 구성 또는 기타 대형 파일)의 구성 구매 합니다. IOS 6부터, Apple 호스트 하는 해당 서버의 파일 별도 서버에 대 한 필요성을 제거 합니다. 이 기능은 비에서 사용할 수 있는 제품 (사용할 수 있는 또는 구독)에 사용할 수만 있습니다. Apple의 호스팅 서비스를 사용 하 여의 장점은 다음과 같습니다.

- 호스팅 및 대역폭 비용을 절감 합니다.
- 것 보다 더 확장성이 현재 사용 중인 모든 서버 호스트 합니다. 
- 서버 쪽 처리를 빌드할 필요가 없으므로 작성할 코드입니다. 
- 백그라운드 다운로드 구현 됩니다.

참고: 실제 장치를 사용 하 여 테스트 해야 하므로 앱에서 바로 구매한 콘텐츠가 ios 시뮬레이터에서 지원 되지 않습니다 호스트 테스트.

### <a name="hosted-content-basics"></a>호스팅된 콘텐츠 기본 사항

IOS 6, 이전 된 제품을 제공 하는 두 가지 방법 (에서 자세히 설명한 [Xamarin 앱에서 바로 구매](~/ios/platform/in-app-purchasing/index.md) 설명서):

- **기본 제공 제품** – 하는 '해제할' 구매에 있지만 (코드 또는 포함 된 리소스 중 하나) 응용 프로그램에 기본 제공 되는 기능입니다. 기본 제공 제품의 예로 잠금 해제 된 사진 필터 또는 게임에서 파워업을 들 수 있습니다.
- **서버 제공 제품** – 구입 후 응용 프로그램 해야에서 콘텐츠를 다운로드 하면 작동 하는 서버입니다. 이 콘텐츠 다운로드 구매 하는 동안, 장치에 저장 되어 다음 제품을 제공 하는 일환으로 렌더링 합니다. 책, 잡지 문제 또는 백그라운드 아트 및 구성 파일의 구성 된 게임 레벨을 예로 들 수 있습니다.

6 Apple iOS에서 서버 제공 제품 변형을 제공 합니다: 해당 서버에 콘텐츠 파일을 호스트 하는 것입니다. 따라서 별도 서버를 작동 하지 않아도 됩니다 Store Kit 이전에 작성 했던 백그라운드 다운로드 기능을 제공 하기 때문에 server 제공 제품 빌드를 훨씬 더 간단 합니다. 를 이용 하려면 Apple의 호스팅의 새 인 앱 구매 제품에 대 한 콘텐츠를 호스팅할 수 있도록 하 고 활용 하기 위해 저장소 키트 코드를 수정 합니다. 그런 다음 제품 콘텐츠 파일 Xcode를 사용 하 여 작성 하 고 검토 및 릴리스 Apple 서버에 업로드 됩니다.

[![](changes-to-storekit-images/image4.png "빌드 및 배달 프로세스")](changes-to-storekit-images/image4.png#lightbox)

앱 스토어를 사용 하 여 앱에서 바로 구매 수 있도록 *호스트 된 콘텐츠를 사용 하 여* 다음 설치 및 구성 필요:

- **iTunes Connect** – 있습니다 *해야* 제공한 은행 및 세금 정보를 Apple에 사용자를 대신해 수집 자금을 송금할 수 있도록 합니다. 제품을 판매 하 고 구매 테스트할 샌드박스 사용자 계정을 설정 구성할 수 있습니다.  _구성 해야 호스트 된 콘텐츠를 Apple과 호스트할 영구 제품에_입니다.
- **iOS Provisioning Portal** – 번들 식별자를 만들고 앱에서 바로 구매를 지 원하는 응용 프로그램에 대 한 방법으로 앱에 대 한 앱 스토어 액세스를 사용 하도록 설정 합니다.
- **키트 저장** – 제품을 표시, 제품을 구매 및 트랜잭션 복원에 대 한 앱에 코드를 추가 합니다.  _IOS 6 저장소 키트도 관리 됩니다 진행률 업데이트를 사용 하 여 백그라운드에서 제품 콘텐츠를 다운로드 합니다._
- **사용자 지정 코드** – 진행을 고객이 구매한 제품 또는 이러한 구입한 서비스를 제공 합니다. 와 같은 새 iOS 6 Store Kit 클래스를 활용 `SKDownload` Apple에서 호스트 된 콘텐츠를 검색 합니다.

다음 섹션에서 만들어 구매를 관리 하는 패키지를 업로드 하는 호스트 된 콘텐츠를 구현 하 고 다운로드 프로세스를이 문서에 대 한 샘플 코드를 사용 하는 방법을 설명 합니다.

### <a name="sample-code"></a>샘플 코드

샘플 프로젝트 *HostedNonConsumables* (StoreKitiOS6.zip)에서 사용 하 여 콘텐츠를 호스트 합니다. 앱은 두 "설명서 장" 판매, Apple 서버에서 호스트 되는 콘텐츠를 제공 합니다. 콘텐츠를 훨씬 더 복잡 한 내용을 실제 응용 프로그램에 사용 될 수 있지만 텍스트 파일 및 이미지 구성 됩니다.

전에 중 그리고 구입 후 앱은 다음과 같습니다.

 [![](changes-to-storekit-images/image5.png "전에 중 그리고 구입 후 앱을 다음과 같이 표시 됩니다.")](changes-to-storekit-images/image5.png#lightbox)

텍스트 파일 및 이미지를 다운로드 하 고 응용 프로그램의 문서 디렉터리에 복사 됩니다. 응용 프로그램 저장소에 사용할 수 있는 다른 디렉터리에 대 한 자세한 내용은 참조는 [파일 시스템 설명서](~/ios/app-fundamentals/file-system.md)합니다.

## <a name="itunes-connect"></a>iTunes Connect

Apple 사용 하는 새 제품 만들기의 콘텐츠 경우 호스트를 선택 해야 합니다 **비에서 사용할 수 있는** 제품 유형입니다. 다른 제품 유형 콘텐츠를 호스트 하는 것을 지원 하지 않습니다. 또한 사용 하지 않아야에 대 한 콘텐츠 호스팅 *기존* 제품 판매에 새 제품에 대 한 콘텐츠 호스팅 설정 합니다.

 [![](changes-to-storekit-images/image6.png "비-사용할 수 있는 제품 유형을 선택 합니다.")](changes-to-storekit-images/image6.png#lightbox)

입력 한 **제품 ID**합니다. 이 ID 해야 나중에이 제품에 대 한 콘텐츠를 만들 때.

 [![](changes-to-storekit-images/image7.png "제품 ID를 입력 합니다.")](changes-to-storekit-images/image7.png#lightbox)

콘텐츠 호스팅의 세부 정보 섹션에서 설정 됩니다. 라이브 상태로 전환 하 여 앱 내 구매를 하기 전에 선택 취소 합니다 **Apple 사용 하 여 콘텐츠를 호스트** (경우에 일부 테스트 콘텐츠 업로드)를 취소 하려는 경우 확인란을 선택 합니다. 그러나 콘텐츠 호스팅 제거할 수 없습니다 인 앱 구매 라인이 된 후.

 [![](changes-to-storekit-images/image8.png "Apple과 콘텐츠 호스팅")](changes-to-storekit-images/image8.png#lightbox)

제품 입력에 콘텐츠를 호스트에 설정 되 면 **업로드 대기 중** 상태가이 메시지를 표시 합니다.

 [![](changes-to-storekit-images/image9.png "제품은 업로드 상태에 대 한 대기를 입력 하 고이 메시지를 표시 합니다.")](changes-to-storekit-images/image9.png#lightbox)

콘텐츠 패키지 Xcode를 사용 하 여 만들고 보관 도구를 사용 하 여 업로드 합니다. 콘텐츠 패키지를 만들기 위한 지침은 다음 섹션에 제공 됩니다 **만들기. 패키지 파일**합니다.

## <a name="creating-pkg-files"></a>만드는 중입니다. 패키지 파일

Apple에 업로드 된 콘텐츠 파일에는 다음 제한 사항을 충족 해야 합니다.

- 크기가 2GB를 초과할 수 없습니다.
- 실행 코드 (또는 콘텐츠 외부 가리키는 symlink)를 포함할 수 없습니다.
- 올바르게 포맷 되어야 합니다 (포함 하는 **.plist** 파일) 있고를 **.pkg** 파일 확장명입니다. 이 자동으로 Xcode를 사용 하 여 이러한 지침을 따를 경우.

이러한 제한 사항을 충족 하는 여러 다른 파일 및 형식의 파일을 추가할 수 있습니다. 콘텐츠 응용 프로그램에 배달 하기 전에 압축 이며 코드에 액세스 하기 전에 저장소 키트에서 압축을 해제 합니다.

콘텐츠 패키지를 업로드 한 후 최신 콘텐츠를 사용 하 여 바꿀 수 있습니다. 새 콘텐츠 업로드 하 고 일반 프로세스를 통해 검토 및 승인을 위해 제출 해야 합니다. 증가 된 `ContentVersion` 최신 임을 업데이트 된 콘텐츠 패키지 필드입니다.

### <a name="xcode-in-app-purchase-content-projects"></a>Xcode에서 앱 콘텐츠 구매 프로젝트

인 앱 구매 제품에 대 한 콘텐츠 패키지를 현재 만들려면 Xcode 필요 합니다. 필요 없음 Objective-c로 코딩 Xcode에서 파일 및 plist만 포함 하는 이러한 패키지에 대 한 새 프로젝트 형식.

샘플 응용 프로그램에 판매에 대 한 서적 내용 일부 – 각 장의 콘텐츠 패키지에 포함 됩니다.

-  텍스트 파일 및
-  챕터를 나타내는 데 사용 되는 이미지입니다.


선택 하 여 시작 **파일 > 새 프로젝트** 메뉴에서 선택한 **앱 내 구매 콘텐츠**:

 [![](changes-to-storekit-images/image10.png "인 앱 구매 콘텐츠를 선택 합니다.")](changes-to-storekit-images/image10.png#lightbox)

입력 합니다 **제품 이름** 및 **회사 식별자** 되도록를 **번들 식별자** 일치 하는 **제품 ID** iTunes에서 입력 한 이 제품에 대 한 연결 합니다.

[![](changes-to-storekit-images/image11.png "이름 및 식별자를 입력 합니다.")](changes-to-storekit-images/image11.png#lightbox)

빈 값은 이제 **인 앱 구매 콘텐츠** 프로젝트입니다. 마우스 오른쪽 단추로 클릭 수 및 **파일 추가...** 에 끌어 또는 합니다 **Project Navigator**합니다. 있는지 확인 합니다 **ContentVersion** 올바른지 (해야 1.0에서 시작 되지만 나중에 콘텐츠를 업데이트 하려는 경우 증가 해야) 합니다.

이 스크린샷에서 Xcode 프로젝트와 주 창에 표시 plist 항목에 포함 된 콘텐츠 파일을 사용 하 여 보여줍니다.

[![](changes-to-storekit-images/image12.png "Xcode 프로젝트와 주 창에 표시 plist 항목에 포함 된 콘텐츠 파일을 사용 하 여 보여 주는이 스크린샷")](changes-to-storekit-images/image12.png#lightbox)

모든 콘텐츠 파일을 추가한 후이 프로젝트를 저장 하 고 및 나중에 다시 편집 하 하거나, 업로드 프로세스를 시작할 수 있습니다.

## <a name="uploading-pkg-files"></a>업로드 중입니다. 패키지 파일

콘텐츠 패키지를 업로드 하는 가장 쉬운 방법은 된 합니다 **Xcode 보관 도구**합니다. 선택할 **제품 > 보관** 시작 메뉴에서:

![](changes-to-storekit-images/image13.png "Archiven 선택")

콘텐츠 패키지 아래와 같이 다음 보관 파일에 나타납니다. 이 줄은 보관 파일 형식과 아이콘 표시는 **인 앱 구매 콘텐츠 보관소**합니다. 클릭 **의 유효성을 검사 하는 중...** 실제로 업로드를 수행 하지 않고 오류에 대 한 콘텐츠 패키지를 확인 합니다.

[![](changes-to-storekit-images/image14.png "패키지 유효성 검사")](changes-to-storekit-images/image14.png#lightbox)

ITunes Connect 자격 증명을 사용 하 여 로그인 합니다.

[![](changes-to-storekit-images/image15.png "ITunes Connect 자격 증명을 사용 하 여 로그인")](changes-to-storekit-images/image15.png#lightbox)

사용 하 여이 콘텐츠를 연결 하려면 올바른 응용 프로그램 및 앱 내 구매를 선택 합니다.

[![](changes-to-storekit-images/image16.png "사용 하 여이 콘텐츠를 연결 하려면 올바른 응용 프로그램 및 앱 내 구매를 선택 합니다.")](changes-to-storekit-images/image16.png#lightbox)

이 스크린샷과 같은 메시지가 표시 됩니다.

![예제 문제 메시지가](changes-to-storekit-images/image17.png "예로 문제 메시지 없음")

이제 유사한 프로세스를 통해 이동 되지만 클릭 **배포 하는 중...** 콘텐츠를 실제로 업로드 됩니다.

[![앱을 배포](changes-to-storekit-images/image18.png "앱 배포")](changes-to-storekit-images/image18.png#lightbox)

콘텐츠를 업로드 하려면 첫 번째 옵션을 선택 합니다.

![콘텐츠를 업로드](changes-to-storekit-images/image19.png "콘텐츠 업로드")

다시 로그인 합니다.

[![](changes-to-storekit-images/image15.png "로그인")](changes-to-storekit-images/image15.png#lightbox)

콘텐츠를 업로드 하려면 올바른 응용 프로그램 및 앱 내 구매 레코드를 선택 합니다.

[![](changes-to-storekit-images/image20.png "응용 프로그램 및 앱 내 구매 레코드를 선택 합니다.")](changes-to-storekit-images/image20.png#lightbox)

파일이 업로드 되는 동안 기다립니다.

[![](changes-to-storekit-images/image21.png "콘텐츠 업로드 대화 상자")](changes-to-storekit-images/image21.png#lightbox)

업로드가 완료 되 면 콘텐츠 앱 스토어에 제출 되었습니다를 알리기 위해 메시지가 나타납니다.

[![](changes-to-storekit-images/image22.png "예제 업로드 성공 메시지")](changes-to-storekit-images/image22.png#lightbox)

작업이 완료 되 면 되 면로 돌아가면 제품 페이지에서 iTunes Connect 패키지 세부 정보를 표시 하 고 이어야 **제출 준비가** 상태입니다. 제품이이 상태 이면 샌드박스 환경에서 테스트를 시작할 수 있습니다. '제출' 테스트 샌드박스를 위한 제품 필요가 없습니다.

[![](changes-to-storekit-images/image23.png "iTunes Connect 패키지 세부 정보를 표시 하 고 준비 완료에서 제출 상태 수")](changes-to-storekit-images/image23.png#lightbox)

(예: 약간의 시간이 걸릴 수 있습니다. 잠시 후) 간의 보관 및 iTunes Connect 상태 업데이트를 업로드 합니다. 별도로 검토를 위해 제품을 제출할 수도 있고 응용 프로그램 이진과 함께에서 제출할 수 있습니다. Apple 공식적으로 콘텐츠 승인 후에 사용할 수는 프로덕션 환경에서 앱 스토어 앱에서 구매에 대 한 합니다.

### <a name="pkg-file-format"></a>패키지 파일 형식

Xcode 및 보관 도구를 사용 하 여 만들고 호스팅된 콘텐츠 패키지를 업로드 하는 표시 되지 패키지 자체의 내용을 의미 합니다. 파일 및 디렉터리 아래 스크린샷 처럼 샘플 앱 모양에 만든 패키지에서 사용 하 여 합니다 **plist** 루트에 있는 제품 파일에 파일을 **내용을** 하위 디렉터리:

[![](changes-to-storekit-images/image24.png "루트 및 콘텐츠 하위 디렉터리에 제품 파일 plist 파일")](changes-to-storekit-images/image24.png#lightbox)

패키지의 디렉터리 구조를 확인 (특히 파일의 위치는 `Contents` 하위 디렉터리) 장치에 패키지에서 파일을 추출 하려면이 정보를 이해 해야 하기 때문입니다.

### <a name="updating-package-content"></a>패키지 콘텐츠를 업데이트 하는 중

승인 된 후 콘텐츠를 업데이트 하기 위한 절차:

- Xcode에서 앱 내 구매 Content 프로젝트를 편집 합니다.
- 버전 번호를 높입니다.
- ITunes Connect에 다시 업로드 합니다. 후속 구매자가 자동으로 최신 버전 가져오지만, 이전 버전에 이미 있는 사용자는 모든 알림을 받지 않습니다.
- 앱이 사용자에 게 알림 및 콘텐츠의 최신 버전을 검색 하도록 장려 하는 일을 담당 합니다. 앱은 스토어 키트의 복원 기능을 사용 하 여 새 버전을 다운로드 하는 함수를 빌드할 수도 해야 합니다.
- 최신 버전이 있는지를 확인 하려면 SKProducts (예: 가져올 앱에 기능을 빌드할 수 있습니다. 제품 가격을 검색 하는 데 사용 되는 동일한 프로세스) 및 ContentVersion 속성을 비교 합니다.

## <a name="purchasing-overview"></a>개요를 구매합니다.

이 섹션을 읽기 전에 기존 검토 [인 앱 구매 설명서](~/ios/platform/in-app-purchasing/index.md)합니다.

호스트 된 콘텐츠를 사용 하 여 제품을 때 발생 하는 이벤트 시퀀스를 구매 하 고 다운로드가이 다이어그램에 설명 되어 있습니다.

[![](changes-to-storekit-images/image25.png "호스트 된 콘텐츠를 사용 하 여 제품을 때 발생 하는 이벤트 시퀀스를 구매 하 고 다운로드")](changes-to-storekit-images/image25.png#lightbox)

1. ITunes Connect와 콘텐츠 호스트 설정에서 새 제품을 만들 수 있습니다. 실제 콘텐츠 (파일로 단순히으로 끌어 놓고 폴더로) Xcode에서 별도로 생성 및 보관 한 다음는 iTunes (코딩 없이 필수임)에 업로드 합니다. 각 제품 구매에 사용할 수 있는 승인에 대 한 다음 제출 됩니다. 샘플 코드에서 이러한 제품 Id는 하드 코드 된, 하지만 Apple 사용 하 여 콘텐츠를 호스트 하는 것은 iTunes Connect에 콘텐츠와 새 제품을 제출할 때 업데이트할 수 있도록 원격 서버에서 사용 가능한 제품 목록을 저장 하는 경우 보다 유연 합니다.
1. 사용자는 product를 구입한 경우 트랜잭션 처리를 위해 결제 큐에 배치 됩니다.
1. 저장소 키트 구매 요청 처리를 위해 iTunes 서버로 전달합니다.
1. (예: iTunes 서버에서 완료 된 트랜잭션 고객은 비용을 지불) 수신 확인을 다운로드할 수 있는 인지 비롯 하 여 연결 된 제품 정보를 사용 하 여 앱에 반환 됩니다 (그렇다면 파일 크기 및 기타 메타 데이터).
1. 코드는 제품을 다운로드할 수 있는지 확인 하 고 그렇다면도 지불 큐에 배치 되는 콘텐츠 다운로드 요청을 수행 해야 합니다. 저장소 키트 iTunes 서버로이 요청을 보냅니다.
1. 서버는 저장소 키트 다운로드 진행률 및 코드에 대 한 예상 남은 시간을 반환 하는 콜백을 제공 하는 콘텐츠 파일을 반환 합니다.
1. 완료 되 면 알림을 얻고 캐시 폴더에 파일 위치를 전달 합니다.
1. 코드 파일을 복사 하 고 제품을 구매한 적이 염두에 두어야 하는 모든 상태 저장을 확인 해야 합니다. 새 파일에 대해 올바르게 백업 플래그를 설정 하려면이 기회 (힌트: 서버에서 제공 하며 사용자가 편집 하지 않습니다 하는 경우 아마도 건너뛰어야 백업 사용자 항상 검색할 수 Apple 서버에서 나중에 있으므로).
1. FinishTransaction를 호출 합니다. 이 단계 트랜잭션이 결제 큐에서 제거 되므로 중요 한 것입니다. 캐시 디렉터리에서 콘텐츠를 복사한 후까지 FinishTransaction을 호출 하지 않으면 중요 한 이기도 합니다. FinishTransaction를 호출 하면 후 캐시 된 파일은 신속 하 게 제거할 수 있습니다.

## <a name="implementing-hosted-content-purchase"></a>호스팅된 콘텐츠 구매를 구현합니다.

전체와 함께 다음 정보를 읽어야 [앱에서 바로 구매 설명서](~/ios/platform/in-app-purchasing/index.md)합니다. 이 문서의 정보는 호스트 된 콘텐츠 및 이전 구현 간의 차이점에 중점을 둡니다.

### <a name="classes"></a>클래스

다음 클래스를 추가 되었거나 iOS 6 호스트 지원 콘텐츠를 변경 합니다.

- **SKDownload** – 다운로드에서 진행률을 나타내는 새 클래스입니다. 하지만 API를 통해 둘 이상의 당-제품에 대 한 처음에 하나만 구현 되었습니다.
- **SKProduct** -새 속성 추가: `Downloadable`를 `ContentVersion`, `ContentLengths` 배열입니다.
- **SKPaymentTransaction** -새 속성 추가: `Downloads`의 컬렉션이 들어 있는 `SKDownload` 경우이 제품에 호스트 된 콘텐츠를 다운로드할 수 있는 개체입니다.
- **SKPaymentQueue** – 새 메서드 추가: `StartDownloads`합니다. 이 메서드를 사용 하 여 `SKDownload` 해당 호스트 된 콘텐츠를 가져올 개체입니다. 다운로드는 백그라운드에서 발생할 수 있습니다.
- **SKPaymentTransactionObserver** – 새 메서드: `UpdateDownloads`합니다. 저장소 키트 현재 다운로드 작업에 대 한 진행률 정보를 사용 하 여이 메서드를 호출합니다.

새로운 세부 정보 `SKDownload` 클래스:

- **진행률** – 완료율 표시기를 사용자에 게 표시 하는 데 사용할 수 있는 0 ~ 1 사이의 값입니다. 진행률을 사용 하지 않는 = = 1 다운로드 완료 되었는지 여부를 검색, 상태를 확인 하려면 완료 됨 = =.
- **TimeRemaining** – 다운로드 남은 기간을 초 단위로 예상 합니다. -1은 예상치를 계산 하는 것도 의미 합니다.
- **상태** – 활성 상태로 대기 중, 완료, 실패, 일시 중지, 취소 합니다.
- **ContentURL** – 파일 콘텐츠를에서 어디에 넣 디스크의 위치는 `Cache` 디렉터리입니다. 다운로드가 완료 된 후에 채워집니다.
- **오류** – 상태는 실패 한 경우이 속성을 확인 합니다.

샘플 코드의 클래스 간의 상호 작용 (호스팅된 콘텐츠 구매에 관련 코드는 녹색으로 표시 됨)이이 다이어그램에 나와 있습니다.

[![](changes-to-storekit-images/image26.png "호스팅된 콘텐츠 구입이이 다이어그램에서 녹색으로 표시 됩니다.")](changes-to-storekit-images/image26.png#lightbox)

이러한 클래스는 사용 된 샘플 코드는이 섹션의 나머지 부분에 표시 됩니다.

### <a name="custompaymentobserver-skpaymenttransactionobserver"></a>CustomPaymentObserver (SKPaymentTransactionObserver)

기존 변경 `UpdatedTransactions` 재정의 다운로드 가능한 콘텐츠 및 호출을 확인 하려면 `StartDownloads` 필요한 경우:

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
    foreach (SKPaymentTransaction transaction in transactions) {
        switch (transaction.TransactionState) {
        case SKPaymentTransactionState.Purchased:
            // UPDATED FOR iOS 6
            if (transaction.Downloads != null && transaction.Downloads.Length > 0) {
                // Purchase complete, and it has downloads... so download them!
                SKPaymentQueue.DefaultQueue.StartDownloads (transaction.Downloads);
                // CompleteTransaction() call has moved after downloads complete
            } else {
                // complete the transaction now
                theManager.CompleteTransaction(transaction);
            }
            break;
        case SKPaymentTransactionState.Failed:
            theManager.FailedTransaction(transaction);
            break;
        case SKPaymentTransactionState.Restored:
            // TODO: you must decide how to handle restored transactions.
            // Triggering all the downloads at once is not advisable.
            theManager.RestoreTransaction(transaction);
            break;
        default:
            break;
        }
    }
}
```

새 재정의 된 메서드에 `UpdatedDownloads` 아래에 표시 됩니다. 이 메서드를 호출 하는 저장소 키트 `StartDownloads` 에서 트리거되는 `UpdatedTransactions`합니다. 이 메서드는 *여러 번* 진행률을 다운로드할 수 있도록 결정 되지 않은 간격 다시 다운로드가 완료 된 시기 및 합니다. 알림 메서드 배열을 받는 `SKDownload` 개체, 메서드를 호출할 때마다 큐에 여러 다운로드의 상태를 사용 하 여 제공할 수 있도록 합니다. 에 표시 된 대로 구현 아래 다운로드 상태는 모든 시간 및 수행 하는 적절 한 동작을 확인 합니다.

```csharp
// ENTIRELY NEW METHOD IN iOS6
public override void PaymentQueueUpdatedDownloads (SKPaymentQueue queue, SKDownload[] downloads)
{
    Console.WriteLine (" -- PaymentQueueUpdatedDownloads");
    foreach (SKDownload download in downloads) {
        switch (download.DownloadState) {
        case SKDownloadState.Active:
            // TODO: implement a notification to the UI (progress bar or something?)
            Console.WriteLine ("Download progress:" + download.Progress);
            Console.WriteLine ("Time remaining:   " + download.TimeRemaining); // -1 means 'still calculating'
            break;
        case SKDownloadState.Finished:
            Console.WriteLine ("Finished!!!!");
            Console.WriteLine ("Content URL:" + download.ContentUrl);

            // UNPACK HERE! Calls FinishTransaction when it's done
            theManager.SaveDownload (download);

            break;
        case SKDownloadState.Failed:
            Console.WriteLine ("Failed"); // TODO: UI?
            break;
        case SKDownloadState.Cancelled:
            Console.WriteLine ("Canceled"); // TODO: UI?
            break;
        case SKDownloadState.Paused:
        case SKDownloadState.Waiting:
            break;
        default:
            break;
        }
    }
}
```

### <a name="inapppurchasemanager-skproductsrequestdelegate"></a>InAppPurchaseManager (SKProductsRequestDelegate)

이 클래스는 새 메서드가 포함 `SaveDownload` 각 다운로드는 성공적으로 완료 된 후 호출 되는 합니다.

호스트 된 콘텐츠를 성공적으로 다운로드 되었으며에 압축을 푼는 `Cache` 디렉터리입니다. 구조의 합니다. 패키지 파일에 필요한 모든 파일을 저장할 수는 `Contents` 하위 디렉터리 아래 코드 내에서 파일을 추출 하도록는 `Contents` 하위 디렉터리입니다.

코드는 콘텐츠 패키지에 있는 모든 파일을 반복 하 고에 복사 합니다는 `Documents` 디렉터리에 대 한 명명 된 하위 폴더에는 `ProductIdentifier`합니다. 마지막으로 호출한 `CompleteTransaction`를 호출 하는 `FinishTransaction` 트랜잭션이 결제 큐에서 제거할 합니다.

```csharp
// ENTIRELY NEW METHOD IN iOS 6
public void SaveDownload (SKDownload download)
{
    var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
    var targetfolder = System.IO.Path.Combine (documentsPath, download.Transaction.Payment.ProductIdentifier);
    // targetfolder will be "/Documents/com.xamarin.storekitdoc.montouchimages/" or something like that
    if (!System.IO.Directory.Exists (targetfolder))
        System.IO.Directory.CreateDirectory (targetfolder);
    foreach (var file in System.IO.Directory.EnumerateFiles 
             (System.IO.Path.Combine(download.ContentUrl.Path, "Contents"))) { // Contents directory is the default in .PKG files
        var fileName = file.Substring (file.LastIndexOf ("/") + 1);
        var newFilePath = System.IO.Path.Combine(targetfolder, fileName);
        if (!System.IO.File.Exists(newFilePath)) // HACK: this won't support new versions...
            System.IO.File.Copy (file, newFilePath);
        else
            Console.WriteLine ("already exists " + newFilePath);
    }
    CompleteTransaction (download.Transaction); // so it gets 'finished'
}
```

때 `FinishTransaction` 호출 되는 다운로드 한 파일은 더 이상에 포함 되도록 보장 합니다 `Cache` 디렉터리입니다. 호출 하기 전에 모든 파일을 복사할 `FinishTransaction`합니다.


## <a name="other-considerations"></a>기타 고려 사항

위의 예제 코드에 호스팅된 콘텐츠 구매 하는 매우 간단한 구현을 보여 줍니다. 가지 고려해 야 할 몇 가지 추가 사항이 있습니다.

### <a name="detecting-updated-content"></a>업데이트 된 콘텐츠를 검색합니다.

호스트 된 콘텐츠 패키지를 업데이트할 수 있지만, Store Kit 이러한 업데이트가 이미 다운로드 하 고 제품을 구매 하는 사용자에 게 푸시 메커니즘을 제공 하지 않습니다. 이 기능을 구현할 코드가 새 확인할 수 `SKProduct.ContentVersion` 속성 (경우 합니다 `SKProduct` 는 `Downloadable`) 정기적으로 하 고 값이 증가 하는 경우를 검색 합니다. 또는 푸시 알림 시스템을 빌드할 수 있습니다.

### <a name="installing-updated-content-versions"></a>업데이트 된 콘텐츠 버전 설치

위의 샘플 코드 파일이 이미 있으면 파일을 복사를 건너뜁니다. 다운로드 되 고 콘텐츠의 최신 버전을 지원 하려는 경우 좋은 방법이 아닙니다.

대신 버전의 경우 이라는 폴더에 콘텐츠를 복사 하 여 (예: 현재 버전인 기록해 수 있습니다. `NSUserDefaults` 또는 완료 된 구매 레코드를 저장 하는 위치).

### <a name="restoring-transactions"></a>복원 중인 트랜잭션

때 `SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions` 는 Store Kit 호출 사용자에 대 한 이전의 모든 트랜잭션을 반환 합니다. 많은 수의 항목을 구매한, 각 구매 큰 콘텐츠 패키지에 다음 복원 될 수 많은 네트워크 트래픽 모든 가져옵니다 큐에 대기 된 다운로드에 대 한 한 번에 있습니다.

연결 된 콘텐츠 패키지의 실제 다운로드에서 제품을 별도로 구입 했는지 여부를 추적을 고려 합니다.

### <a name="pausing-restarting-and-canceling-downloads"></a>일시 중지, 다시 시작 및 다운로드를 취소 합니다.

샘플 코드는이 기능을 보여 주지 않지만, 이지만 일시 중지 하 고 호스팅된 콘텐츠 다운로드를 다시 시작할 수 있습니다. `SKPaymentQueue.DefaultQueue` 에 대 한 메서드가 `PauseDownloads`를 `ResumeDownloads` 고 `CancelDownloads`합니다.

코드를 호출 하는 경우 `FinishTransaction` 되는 다운로드 하기 전에 결제 큐에 `Finished` 해당 다운로드를 자동으로 취소 합니다.

### <a name="setting-the-skip-backup-flag-on-the-downloaded-content"></a>다운로드 한 콘텐츠 건너뛰기 백업 플래그를 설정합니다.

Apple iCloud 백업 지침은 사용자가 아닌 콘텐츠 서버에서 쉽게 복원 해야 하는 것이 좋습니다 *되지* 백업할 (사용 하기 때문에는 불필요 하 게 iCloud 저장소). 백업 특성 설정에 대 한 자세한 내용은 참조는 [파일 시스템](~/ios/app-fundamentals/file-system.md) 설명서.

## <a name="summary"></a>요약

이 문서에는 iOS6에서 저장소 키트의 두 가지 새로운 기능이 도입 되었습니다: iTunes 및 앱 내에서 다른 콘텐츠를 구입 하 고 고유한 앱 내 구매를 호스트 하는 Apple의 서버를 활용 하 합니다. 이 소개를 기존과 함께에서 읽어야 [인 앱 구매 문서](~/ios/platform/in-app-purchasing/index.md) 전체 범위의 저장소 키트 기능을 구현 합니다.

## <a name="related-links"></a>관련 링크

- [StoreKit (샘플)](https://developer.xamarin.com/samples/monotouch/StoreKit/)
- [앱에서 바로 구매](~/ios/platform/in-app-purchasing/index.md)
- [StoreKit 프레임 워크 참조](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/StoreKit_Collection/_index.html)
- [SKStoreProductViewController 클래스 참조](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/SKITunesProductViewController_Ref/SKStoreProductViewController.html)
- [iTunes 검색 API 참조](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html)
- [SKDownload](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKDownload_Ref/Introduction/Introduction.html)
- [SKPaymentQueue](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKPaymentQueue_Class/Reference/Reference.html#/apple_ref/occ/instm/SKPaymentQueue/cancelDownloads:)
- [SKProduct](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKProduct_Reference/Reference/Reference.html#/apple_ref/occ/instp/SKProduct/downloadable)
- [WWDC 비디오: 저장소 키트를 사용 하 여 제품을 판매](https://developer.apple.com/videos/wwdc/2012/?include=302#302)
