---
title: iOS 6의 StoreKit 변경 내용
description: iOS 6에는 저장소 키트 API에 대 한 두 가지 변경 사항이 도입 되었습니다. 앱 내에서 iTunes (및 App Store/iBookstore 점) 제품을 표시 하는 기능 및 Apple에서 다운로드 한 파일을 호스트 하는 새로운 앱 내 구매 옵션이 제공 됩니다. 이 문서에서는 Xamarin.ios를 사용 하 여 이러한 기능을 구현 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 253D37D7-44C7-D012-3641-E15DC41C2699
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 7cf18934c70acf59213a697ab57b6c5e308e7b2a
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725223"
---
# <a name="changes-to-storekit-in-ios-6"></a>iOS 6의 StoreKit 변경 내용

_iOS 6에는 저장소 키트 API에 대 한 두 가지 변경 사항이 도입 되었습니다. 앱 내에서 iTunes (및 App Store/iBookstore 점) 제품을 표시 하는 기능 및 Apple에서 다운로드 한 파일을 호스트 하는 새로운 앱 내 구매 옵션이 도입 되었습니다. 이 문서에서는 Xamarin.ios를 사용 하 여 이러한 기능을 구현 하는 방법을 설명 합니다._

IOS6에서 저장소 키트의 주요 변경 내용은 다음과 같은 두 가지 새로운 기능입니다.

- **앱 내 콘텐츠 표시 & 구매** – 사용자는 앱을 종료 하지 않고도 앱, 음악, 책 및 기타 iTunes 콘텐츠를 구매 하 고 다운로드할 수 있습니다. 사용자 고유의 앱에 연결 하 여 구매를 홍보 하거나 리뷰 및 등급을 장려 하도록 할 수도 있습니다.
- **앱에서 호스트** 되는 콘텐츠-Apple은 앱 내 구매 제품과 관련 된 콘텐츠를 저장 하 고 전달 합니다 .이는 별도의 서버가 파일을 호스팅할 필요가 없는 경우 자동으로 백그라운드 다운로드를 지원 하 고 코드를 더 작게 작성할 수 있도록 합니다.

기능 키트 Api에 대 한 자세한 내용은 [앱 내 구매](~/ios/platform/in-app-purchasing/index.md) 가이드를 참조 하세요.

## <a name="requirements"></a>요구 사항

이 문서에서 설명 하는 저장소 키트 기능을 사용 하려면 iOS 6 및 Xcode 4.5와 함께 Xamarin.ios 6.0이 필요 합니다.

## <a name="in-app-content-display--purchasing"></a>앱 내 콘텐츠 표시 & 구매

IOS의 새로운 앱 내 구매 기능을 통해 사용자는 제품 정보를 보고 앱 내에서 제품을 구입 하거나 다운로드할 수 있습니다.
이전에는 응용 프로그램에서 iTunes, App Store 또는 iBookstore를 트리거해야 합니다. 그러면 사용자가 원래 응용 프로그램을 종료 합니다. 이 새로운 기능은 작업이 완료 되 면 사용자를 앱에 자동으로 반환 합니다.

[![](changes-to-storekit-images/image1.png "Automatically returning to an app after purchase")](changes-to-storekit-images/image1.png#lightbox)

이를 사용 하는 방법의 예는 다음과 같습니다.

- **사용자가 앱을 평가 하도록 장려** – 앱 스토어 페이지를 열어 사용자가 앱을 종료 하지 않고 평가 하 고 검토할 수 있습니다.
- **앱 교차 수준 올리기** – 사용자에 게 게시 하는 다른 앱을 볼 수 있으며, 즉시 구입/다운로드 하는 기능을 제공 합니다.
- **사용자가 콘텐츠를 찾고 다운로드 하도록 지원** – 사용자가 앱에서 검색, 관리 또는 집계 하는 콘텐츠 (예: 음악 관련 앱은 곡의 재생 목록을 제공 하 고 앱 내에서 각 노래를 구입할 수 있습니다.

`SKStoreProductViewController` 표시 된 후에는 사용자가 iTunes, App Store 또는 iBookstore 점에서와 같이 제품 정보를 조작할 수 있습니다. 사용자는 다음을 수행할 수 있습니다.

- 스크린샷 보기 (앱),
- 샘플 노래 또는 비디오 (음악, TV 쇼 및 동영상의 경우)
- 읽기 (및 쓰기) 검토,
- 보기 컨트롤러 및 스토어 키트 내에서 완전히 발생 하는 & 다운로드를 구매 합니다.

`SKStoreProductViewController` 내의 일부 옵션은 계속 해 서 사용자가 앱을 종료 하 고 **관련 된 제품** 또는 앱의 **지원** 링크를 클릭 하는 등의 관련 스토어 앱을 열도록 합니다.

### <a name="skstoreproductviewcontroller"></a>SKStoreProductViewController

앱 내에 제품을 표시 하는 API는 간단 합니다. `SKStoreProductViewController`을 만들고 표시 하기만 하면 됩니다. 제품을 만들고 표시 하려면 다음 단계를 따르세요.

1. 생성자의 `productId`를 포함 하 여 매개 변수를 뷰 컨트롤러에 전달 하는 `StoreProductParameters` 개체를 만듭니다.
1. `SKProductViewController`를 인스턴스화합니다. 클래스 수준 필드에 할당 합니다.
1. 뷰 컨트롤러의 `Finished` 이벤트에 처리기를 할당 합니다 .이 이벤트는 뷰 컨트롤러를 해제 해야 합니다. 이 이벤트는 사용자가 취소를 누를 때 호출 됩니다. 또는 뷰 컨트롤러 내에서 트랜잭션을 마무리 합니다.
1. `StoreProductParameters` 및 완료 처리기를 전달 하는 `LoadProduct` 메서드를 호출 합니다. 완료 처리기는 제품 요청이 성공적으로 완료 되었는지 확인 하 고, 있는 경우 `SKProductViewController`를 모달 형식으로 표시 합니다. 제품을 검색할 수 없는 경우 적절 한 오류 처리를 추가 해야 합니다.

### <a name="example"></a>예제

이 문서의 지 항목 *키트* 샘플 코드에 있는 제품 *뷰* 프로젝트는 모든 제품의 Apple ID를 수락 하 고 `SKStoreProductViewController`를 표시 하는 `Buy` 메서드를 구현 합니다. 다음 코드는 지정 된 Apple ID에 대 한 제품 정보를 표시 합니다.

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

앱이 실행 될 때 아래의 스크린샷 처럼 보입니다. 다운로드 또는 구매는 전적으로 `SKStoreProductViewController`내에서 발생 합니다.

[![](changes-to-storekit-images/image2.png "The app looks like this when running")](changes-to-storekit-images/image2.png#lightbox)

### <a name="supporting-older-operating-systems"></a>이전 운영 체제 지원

샘플 응용 프로그램에는 이전 버전의 iOS에서 앱 스토어, iTunes 또는 iBookstore 서를 여는 방법을 보여 주는 코드가 포함 되어 있습니다. `OpenUrl` 메서드를 사용 하 여 올바르게 제작 된 **Itunes.com** URL을 엽니다.

다음과 같이 버전 검사를 구현 하 여 실행할 코드를 결정할 수 있습니다.

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

사용 하는 Apple ID가 유효 하지 않은 경우 다음 오류가 발생 합니다 .이는 일종의 네트워크 또는 인증 문제를 의미 하므로 혼동 될 수 있습니다.

 `Error Domain=SKErrorDomain Code=5 "Cannot connect to iTunes Store"`

### <a name="reading-objective-c-documentation"></a>목표 읽기-C 설명서

Apple의 개발자 포털에서 스토어 키트에 대 한 정보를 읽고 있는 개발자 [SKStoreProductViewControllerDelegate](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKITunesProductViewControllerDelegate_ProtocolRef/Reference/Reference.html) 는이 새로운 기능과 관련 하 여 설명 하는 프로토콜을 볼 수 있습니다. 대리자 프로토콜에는 Xamarin.ios의 `SKStoreProductViewController`에서 `Finished` 이벤트로 노출 된 productViewControllerDidFinish – 메서드가 하나만 있습니다.

## <a name="determining-apple-ids"></a>Apple Id 확인

`SKStoreProductViewController`에 필요한 Apple ID는 *숫자* 입니다 ("mwc2012"와 같은 번들 id와 혼동 하지 않음). 아래에 나열 된 제품에 대 한 Apple ID를 확인할 수 있는 몇 가지 방법이 있습니다.

### <a name="itunesconnect"></a>iTunesConnect

게시 하는 응용 프로그램의 경우 iTunes Connect에서 **APPLE ID** 를 쉽게 찾을 수 있습니다.

[![](changes-to-storekit-images/image3.png "Finding the Apple ID in iTunes Connect")](changes-to-storekit-images/image3.png#lightbox)

 <a name="Search_API" />

### <a name="search-api"></a>검색 API

Apple은 앱 스토어, iTunes 및 iBookstore 점에서 모든 제품을 쿼리 하는 동적 검색 API를 제공 합니다. 검색 API에 액세스 하는 방법에 대 한 정보는 Apple의 관련 리소스에서 찾을 수 있습니다. 단, API는 등록 된 계열사가 아니라 누구나 노출 됩니다. 결과 JSON을 구문 분석 하 여 `SKStoreProductViewController`에 사용할 Apple ID 인 `trackId` 검색할 수 있습니다.

결과에는 앱에서 제품을 렌더링 하는 데 사용할 수 있는 표시 정보 및 아트 워크 Url을 포함 하는 다른 메타 데이터도 포함 됩니다.

예를 들어 다음과 같은 노래를 선택할 수 있다.

- **Ibooks app** – [https://itunes.apple.com/search?term=ibooks&amp; entity = software&amp;country = us](https://itunes.apple.com/search?term=ibooks&amp;entity=software&amp;country=us)
- **Dot 및 Kangaroo iBook** – [https://itunes.apple.com/search?term=dot+and+the+kangaroo&amp; entity = 전자책&amp;country = us](https://itunes.apple.com/search?term=dot+and+the+kangaroo&amp;entity=ebook&amp;country=us)

### <a name="enterprise-partner-feed"></a>엔터프라이즈 파트너 피드

Apple은 다운로드 가능한 데이터베이스 지원 플랫 파일 형식으로 모든 제품에 대 한 완전 한 데이터 덤프를 승인 된 파트너에 게 제공 합니다. 엔터프라이즈 파트너 피드에 대 한 액세스 권한이 있는 경우 모든 제품에 대 한 Apple ID를 해당 데이터 집합에서 찾을 수 있습니다.

엔터프라이즈 파트너 피드의 많은 사용자는 제품 판매를 커미션 수 있도록 하는 관련 [프로그램](https://www.apple.com/itunes/affiliates) 의 멤버입니다. `SKStoreProductViewController`는 관련 Id (작성 시점)를 지원 하지 않습니다.

### <a name="direct-product-links"></a>직접 제품 링크

제품의 Apple ID는 iTunes Preview URL 링크에서 유추할 수 있습니다.
응용 프로그램, 음악 또는 책의 경우 모든 iTunes 제품 링크에서 `id` 시작 하 여 URL의 일부를 찾아 다음의 숫자를 사용 합니다.

예를 들어 iBooks에 대 한 직접 링크는

```csharp
http://itunes.apple.com/us/app/ibooks/id364709193?mt=8
```

그리고 Apple ID는 **364709193**입니다. 마찬가지로 MWC2012 앱의 경우 직접 링크는

```csharp
http://itunes.apple.com/us/app/mwc-2012-unofficial/id496963922?mt=8
```

그리고 Apple ID는 **496963922**입니다.

## <a name="in-app-purchase-hosted-content"></a>앱에서 호스팅된 콘텐츠

앱에서 바로 구매를 다운로드할 수 있는 콘텐츠 (예: 서적 또는 기타 미디어, 게임 수준 아트 및 구성 또는 기타 많은 파일)로 구성 된 경우 이러한 파일은 웹 서버에서 호스트 되는 데 사용 되며 앱은 다음에 안전 하 게 다운로드 하기 위해 코드를 통합 해야 했습니다. 구입처나. IOS 6부터 Apple은 서버에서 파일을 호스팅하고 별도의 서버에 대 한 필요성을 제거 합니다. 이 기능은 사용할 수 없는 제품 (사용할 수 없음 또는 구독)에 대해서만 사용할 수 있습니다. Apple의 호스팅 서비스를 사용 하는 이점은 다음과 같습니다.

- 호스팅 & 대역폭 비용을 절감 합니다.
- 현재 사용 중인 모든 서버 호스트 보다 확장성이 더 높습니다.
- 서버 쪽 처리를 빌드할 필요가 없기 때문에 작성할 코드는 적습니다.
- 백그라운드 다운로드는 사용자를 위해 구현 됩니다.

참고: iOS 시뮬레이터에서 호스팅된 앱 내 구매 콘텐츠 테스트는 지원 되지 않으므로 실제 장치로 테스트 해야 합니다.

### <a name="hosted-content-basics"></a>호스팅된 콘텐츠 기본 사항

IOS 6 이전에는 제품을 제공 하는 두 가지 방법이 있습니다 ( [Xamarin의 앱 내 구매](~/ios/platform/in-app-purchasing/index.md) 설명서에 자세히 설명 되어 있습니다).

- **기본 제공 제품** – 구매에 의해 ' 잠금 해제 ' 된 기능이 며 응용 프로그램 (코드 또는 포함 된 리소스)에 기본 제공 됩니다. 기본 제공 제품의 예로는 잠금 해제 된 사진 필터 또는 게임 내 전원이 있습니다.
- **서버에서 제공** 하는 제품 – 구매 후 응용 프로그램은 운영 하는 서버에서 콘텐츠를 다운로드 해야 합니다. 이 콘텐츠는 구매 중에 다운로드 되어 장치에 저장 된 다음 제품 제공의 일부로 렌더링 됩니다. 예제에는 책, 잡지 문제 또는 배경 아트와 구성 파일로 구성 된 게임 수준이 포함 됩니다.

IOS 6 Apple에서는 서버에서 제공 하는 콘텐츠 파일을 호스트 하는 서버에서 제공 하는 제품의 변형을 제공 합니다. 이렇게 하면 별도의 서버를 운영할 필요가 없기 때문에 서버에서 제공 하는 제품을 훨씬 간단 하 게 작성할 수 있으며, 스토어 키트는 이전에 직접 작성 해야 했던 백그라운드 다운로드 기능을 제공 합니다. Apple의 호스팅을 활용 하려면 새로운 앱 내 구매 제품에 콘텐츠 호스팅을 사용 하도록 설정 하 고 저장소 키트 코드를 수정 하 여 활용 하세요. 그런 다음 Xcode를 사용 하 여 제품 콘텐츠 파일을 빌드하고 검토 및 릴리스를 위해 Apple의 서버에 업로드 합니다.

[![](changes-to-storekit-images/image4.png "The build and deliver process")](changes-to-storekit-images/image4.png#lightbox)

앱 스토어를 사용 하 여 *호스트 된 콘텐츠와* 앱 내 구매를 제공 하려면 다음 설정 및 구성이 필요 합니다.

- **ITunes Connect** – 사용자를 대신 하 여 수집 된 자금을 다시 만들 수 있도록 Apple에 은행 및 세금 정보를 제공 *해야* 합니다. 그런 다음 제품을 판매 하도록 구성 하 고 샌드박스 사용자 계정을 설정 하 여 구매를 테스트할 수 있습니다.  _또한 Apple에서 호스트 하려는 사용 불가능 제품에 대해 호스트 된 콘텐츠를 구성 해야 합니다_.
- **IOS 프로 비전 포털** – 앱 내 구매를 지 원하는 응용 프로그램에서와 마찬가지로 번들 식별자를 만들고 앱에 대 한 앱 스토어 액세스를 사용 하도록 설정 합니다.
- **매장 키트** – 제품을 표시 하 고, 제품을 구매 하 고, 트랜잭션을 복원 하기 위해 앱에 코드를 추가 합니다.  _IOS 6 스토어 키트는 백그라운드에서 진행 중인 업데이트를 포함 하 여 제품 콘텐츠의 다운로드도 관리 합니다._
- **사용자 지정 코드** – 고객이 구매한 구매를 추적 하 고 구매한 제품이 나 서비스를 제공 합니다. `SKDownload`와 같은 새 iOS 6 스토어 키트 클래스를 활용 하 여 Apple에서 호스트 되는 콘텐츠를 검색 합니다.

다음 섹션에서는이 문서의 샘플 코드를 사용 하 여 패키지를 만들고 업로드 하 여 구매 및 다운로드 프로세스를 관리 하는 방법을 설명 합니다.

### <a name="sample-code"></a>샘플 코드

샘플 프로젝트 *HostedNonConsumables* (StoreKitiOS6)는 호스트 된 콘텐츠를 사용 합니다. 앱은 판매에 대 한 두 개의 "책 장"을 제공 하며,이 콘텐츠는 Apple 서버에서 호스팅됩니다. 콘텐츠는 긴 응용 프로그램에서 훨씬 더 복잡 한 콘텐츠를 사용할 수 있지만 텍스트 파일과 이미지로 구성 되어 있습니다.

앱은 구매 도중 및 후에 다음과 같이 표시 됩니다.

 [![](changes-to-storekit-images/image5.png "The app looks like this before, during and after a purchase")](changes-to-storekit-images/image5.png#lightbox)

텍스트 파일 및 이미지가 다운로드 되어 응용 프로그램의 Documents 디렉터리에 복사 됩니다. 응용 프로그램 저장소에 사용할 수 있는 다른 디렉터리에 대 한 자세한 내용은 [파일 시스템 설명서](~/ios/app-fundamentals/file-system.md)를 참조 하세요.

## <a name="itunes-connect"></a>iTunes Connect

Apple의 콘텐츠 호스팅을 사용 하는 새 제품을 만들 때 사용할 수 **없는** 제품 유형을 선택 해야 합니다. 다른 제품 유형은 콘텐츠 호스팅을 지원 하지 않습니다. 또한 판매 하는 *기존* 제품에 대 한 콘텐츠 호스팅을 사용 하도록 설정 하면 안 됩니다. 새 제품에 대 한 콘텐츠 호스팅을 켭니다.

 [![](changes-to-storekit-images/image6.png "Select the Non-Consumable product type")](changes-to-storekit-images/image6.png#lightbox)

**제품 ID**를 입력 하십시오. 이 ID는 나중에이 제품에 대 한 콘텐츠를 만들 때 필요 합니다.

 [![](changes-to-storekit-images/image7.png "Enter a Product ID")](changes-to-storekit-images/image7.png#lightbox)

콘텐츠 호스팅은 세부 정보 섹션에 설정 되어 있습니다. 앱 내 구매를 계속 진행 하기 전에 취소 하려는 경우 **Apple에서 콘텐츠 호스트** 확인란의 선택을 취소 합니다 (일부 테스트 콘텐츠를 업로드 한 경우에도). 그러나 앱 내 구매가 라이브 된 후에는 콘텐츠 호스팅을 제거할 수 없습니다.

 [![](changes-to-storekit-images/image8.png "Hosting content with Apple")](changes-to-storekit-images/image8.png#lightbox)

콘텐츠를 호스팅한 후에는 제품에서 업로드 상태를 **대기 중** 으로 전환 하 고 다음 메시지를 표시 합니다.

 [![](changes-to-storekit-images/image9.png "The product will enter Waiting for Upload status and show this message")](changes-to-storekit-images/image9.png#lightbox)

Xcode를 사용 하 여 콘텐츠 패키지를 만들고 보관 도구를 사용 하 여 업로드 해야 합니다. 콘텐츠 패키지 만들기에 대 한 지침은 다음 섹션인 만들기에서 제공 됩니다 **. PKG 파일**.

## <a name="creating-pkg-files"></a>만드는. PKG 파일

Apple에 업로드 하는 콘텐츠 파일은 다음 제한 사항을 충족 해야 합니다.

- 크기는 2gb를 초과할 수 없습니다.
- 는 실행 코드 (또는 콘텐츠 외부를 가리키는 symlink)를 포함할 수 없습니다.
- 은 (는) **info.plist** 파일을 포함 하 여 올바른 형식으로 지정 되어야 하며 **pkg** 파일 확장명이 있어야 합니다. Xcode를 사용 하 여 이러한 지침을 따르는 경우 자동으로 수행 됩니다.

이러한 제한을 충족 하는 한 여러 파일 및 파일 유형을 추가할 수 있습니다. 콘텐츠는 응용 프로그램에 배달 하기 전에 압축 되며 코드에서 액세스 하기 전에 스토어 키트에 의해 압축이 풀립니다.

콘텐츠 패키지를 업로드 한 후에는 최신 콘텐츠로 바뀔 수 있습니다. 일반 프로세스를 통해 검토/승인을 위해 새 콘텐츠를 업로드 하 고 제출 해야 합니다. 업데이트 된 콘텐츠 패키지의 `ContentVersion` 필드를 증분 하 여 최신으로 표시 합니다.

### <a name="xcode-in-app-purchase-content-projects"></a>Xcode 앱 내 구매 콘텐츠 프로젝트

앱 내 구매 제품의 콘텐츠 패키지를 만들려면 현재 Xcode가 필요 합니다. 목표 C 코딩은 필요 하지 않습니다. Xcode에는 파일 및 info.plist만 포함 하는 이러한 패키지에 대 한 새 프로젝트 형식이 있습니다.

샘플 응용 프로그램에는 판매에 대 한 책 장이 있습니다. 각 챕터 콘텐츠 패키지에는 다음이 포함 됩니다.

- 텍스트 파일
- 챕터를 나타내는 이미지입니다.

메뉴에서 **파일 > 새 프로젝트** 를 선택 하 고 **앱 내 구매 콘텐츠**를 선택 하 여 시작 합니다.

 [![](changes-to-storekit-images/image10.png "Choose In-App Purchase Content")](changes-to-storekit-images/image10.png#lightbox)

**번들 식별자** 가이 제품에 대 한 iTunes Connect에서 입력 한 **제품 ID** 와 일치 하도록 **제품 이름** 및 **회사 식별자** 를 입력 합니다.

[![](changes-to-storekit-images/image11.png "Enter the  Name and Identifier")](changes-to-storekit-images/image11.png#lightbox)

이제 비어 **있는 앱 내 구매 콘텐츠** 프로젝트가 표시 됩니다. 마우스 오른쪽 단추를 클릭 하 고 **파일을 추가할** 수 있습니다. 또는 **프로젝트 탐색기**로 끌어 놓습니다. **Contentversion** 이 정확한 지 확인 합니다 (1.0에서 시작 해야 하지만 나중에 콘텐츠를 업데이트 하도록 선택한 경우에는 증가 해야 함).

이 스크린샷에서는 프로젝트에 포함 된 콘텐츠 파일과 주 창에 표시 되는 info.plist 항목을 사용 하 여 Xcode를 보여 줍니다.

[![](changes-to-storekit-images/image12.png "This screenshot shows Xcode with the content files included in the project and the plist entries visible in the main window")](changes-to-storekit-images/image12.png#lightbox)

모든 콘텐츠 파일을 추가한 후에는이 프로젝트를 저장 하 고 나중에 다시 편집 하거나 업로드 프로세스를 시작할 수 있습니다.

## <a name="uploading-pkg-files"></a>보십시오. PKG 파일

콘텐츠 패키지를 업로드 하는 가장 쉬운 방법은 **Xcode Archive 도구**를 사용 하는 것입니다. 메뉴에서 **제품 > 보관** 을 선택 하 여 시작 합니다.

![](changes-to-storekit-images/image13.png "Choose Archiven")

그러면 아래와 같이 콘텐츠 패키지가 보관 위치에 표시 됩니다.
보관 유형 및 아이콘은이 줄을 **앱 내 구매 콘텐츠 보관**으로 표시 합니다. **유효성 검사** ...를 클릭 합니다. 실제로 업로드를 수행 하지 않고 콘텐츠 패키지에서 오류를 확인 합니다.

[![](changes-to-storekit-images/image14.png "Validate the package")](changes-to-storekit-images/image14.png#lightbox)

ITunes Connect 자격 증명으로 로그인 합니다.

[![](changes-to-storekit-images/image15.png "Login with your iTunes Connect credentials")](changes-to-storekit-images/image15.png#lightbox)

이 콘텐츠를 연결할 올바른 응용 프로그램 및 앱 내 구매를 선택 합니다.

[![](changes-to-storekit-images/image16.png "Choose the correct application and in-app purchase to associate this content with")](changes-to-storekit-images/image16.png#lightbox)

다음과 같은 메시지가 표시 됩니다.

![예: 문제 메시지 없음](changes-to-storekit-images/image17.png "예: 문제 메시지 없음")

이제 비슷한 프로세스를 진행 하면서 **배포** ...를 클릭 합니다. 는 실제로 콘텐츠를 업로드 합니다.

[![앱 배포](changes-to-storekit-images/image18.png "앱 배포")](changes-to-storekit-images/image18.png#lightbox)

첫 번째 옵션을 선택 하 여 콘텐츠를 업로드 합니다.

![콘텐츠 업로드](changes-to-storekit-images/image19.png "콘텐츠 업로드")

다시 로그인 합니다.

[![](changes-to-storekit-images/image15.png "Login in")](changes-to-storekit-images/image15.png#lightbox)

올바른 응용 프로그램 및 앱 내 구매 레코드를 선택 하 여 콘텐츠를 업로드 합니다.

[![](changes-to-storekit-images/image20.png "Choose the application and in-app purchase record")](changes-to-storekit-images/image20.png#lightbox)

파일이 업로드 될 때까지 기다립니다.

[![](changes-to-storekit-images/image21.png "The content upload dialog")](changes-to-storekit-images/image21.png#lightbox)

업로드가 완료 되 면 콘텐츠가 앱 스토어에 전송 되었음을 알리는 메시지가 표시 됩니다.

[![](changes-to-storekit-images/image22.png "An example successful upload message")](changes-to-storekit-images/image22.png#lightbox)

작업이 완료 되 면 iTunes Connect의 제품 페이지로 돌아가면 패키지 세부 정보를 표시 하 고 상태 **를 제출할 준비가** 됩니다. 제품이이 상태 이면 샌드박스 환경에서 테스트를 시작할 수 있습니다. 샌드박스에서 테스트를 위해 제품을 ' 제출 ' 할 필요는 없습니다.

[![](changes-to-storekit-images/image23.png "iTunes Connect it will show the package details and be in Ready to Submit status")](changes-to-storekit-images/image23.png#lightbox)

약간의 시간이 걸릴 수 있습니다 (예: 업데이트 하는 데 몇 분이 걸립니다. 검토를 위해 제품을 별도로 제출 하거나 응용 프로그램 바이너리와 함께 제출할 수 있습니다. Apple에서 공식적으로 승인한 후에만 앱에서 구매할 수 있도록 프로덕션 앱 스토어에서 콘텐츠를 사용할 수 있습니다.

### <a name="pkg-file-format"></a>PKG 파일 형식

Xcode 및 Archive 도구를 사용 하 여 호스트 된 콘텐츠 패키지를 만들고 업로드 하면 패키지 자체의 내용이 표시 되지 않습니다. 샘플 앱에 대해 만들어진 패키지의 파일 및 디렉터리는 아래 스크린샷에서와 같이 루트의 **info.plist** 파일과 **내용** 하위 디렉터리의 제품 파일을 사용 하 여 표시 됩니다.

[![](changes-to-storekit-images/image24.png "The plist file in the root and the product files in a Contents subdirectory")](changes-to-storekit-images/image24.png#lightbox)

장치의 패키지에서 파일을 추출 하는 데이 정보를 이해 해야 하므로 패키지의 디렉터리 구조 (특히 `Contents` 하위 디렉터리에 있는 파일의 위치)를 확인 합니다.

### <a name="updating-package-content"></a>패키지 콘텐츠 업데이트

승인 된 후 콘텐츠를 업데이트 하는 절차는 다음과 같습니다.

- Xcode에서 앱 내 구매 콘텐츠 프로젝트를 편집 합니다.
- 범프 버전 번호입니다.
- ITunes에 업로드 다시 연결 합니다. 이후 구매자는 자동으로 최신 버전을 가져오지만 이전 버전이 이미 있는 사용자는 알림을 받지 않습니다.
- 앱은 사용자에 게 알리고 최신 버전의 콘텐츠를 검색 하도록 하는 역할을 담당 합니다. 또한 앱은 스토어 키트의 복원 기능을 사용 하 여 새 버전을 다운로드 하는 함수를 작성 해야 합니다.
- 최신 버전이 있는지 확인 하려면 앱에 기능을 빌드하여 앱에 기능을 구축 하 여 제품 가격을 검색 하는 데 사용 되는 것과 동일한 프로세스 이며 ContentVersion 속성을 비교 합니다.

## <a name="purchasing-overview"></a>구매 개요

이 섹션을 읽기 전에 기존 [앱 내 구매 설명서](~/ios/platform/in-app-purchasing/index.md)를 검토 하세요.

호스팅된 콘텐츠를 사용 하는 제품을 구매 하 여 다운로드할 때 발생 하는 이벤트 시퀀스는 다음 다이어그램에 나와 있습니다.

[![](changes-to-storekit-images/image25.png "The sequence of events that occurs when a product with hosted content is purchased and download")](changes-to-storekit-images/image25.png#lightbox)

1. 새 제품은 호스트 된 콘텐츠가 사용 하도록 설정 된 iTunes Connect에서 만들 수 있습니다. 실제 콘텐츠는 Xcode에서 개별적으로 생성 되 고 (파일을 폴더로 끌어 놓은 후) iTunes에 보관 및 업로드 됩니다 (코딩이 필요 하지 않음). 그런 다음 각 제품은 승인을 위해 제출 되며 이후에는 구매할 수 있게 됩니다. 샘플 코드에서 이러한 제품 Id는 하드 코드 되지만 Apple을 사용 하 여 콘텐츠를 호스트 하는 것은 새 제품 및 콘텐츠를 iTunes Connect에 제출할 때 업데이트 될 수 있도록 사용 가능한 제품 목록을 원격 서버에 저장 하는 경우 더욱 유연 합니다.
1. 사용자가 제품을 구매 하면 처리를 위해 거래가 지불 큐에 배치 됩니다.
1. 스토어 키트는 처리를 위해 iTunes 서버에 구매 요청을 전달 합니다.
1. ITunes 서버에서 트랜잭션이 완료 됩니다 (예: 고객에 게 요금이 청구 되 고, 앱에 대 한 수령이 반환 되 고, 다운로드 되었는지 여부를 포함 하 여 제품 정보가 첨부 됩니다 (파일 크기 및 기타 메타 데이터).
1. 코드에서 제품이 다운로드 가능한 지 확인 하 고, 해당 제품이 지불 큐에도 배치 된 콘텐츠 다운로드 요청을 수행 하는지 확인 해야 합니다. 스토어 키트는 iTunes 서버에이 요청을 보냅니다.
1. 서버는 저장소 키트에 콘텐츠 파일을 반환 합니다 .이 키트는 다운로드 진행률 및 남은 예상 시간을 코드에 반환 하는 콜백을 제공 합니다.
1. 완료 되 면 알림 메시지를 받고 캐시 폴더에서 파일 위치를 전달 합니다.
1. 코드에서 파일을 복사 하 고 확인 한 후에는 제품 구매를 염두에 두어야 하는 모든 상태를 저장 해야 합니다. 이 기회를 통해 새 파일 (힌트: 서버에서 제공 되 고 사용자가 편집한 적이 없는 경우)에서 백업 플래그를 올바르게 설정할 수 있습니다. 사용자는 나중에 Apple의 서버에서 항상 해당 서버를 검색할 수 있기 때문입니다.
1. 다음을 호출 합니다. 이 단계는 지불 큐에서 트랜잭션을 제거 하므로 중요 합니다. 또한 캐시 디렉터리에서 콘텐츠를 복사한 후에는 다음을 수행 하는 것이 중요 합니다. Filetransaction을 호출 하면 캐시 된 파일이 빠르게 제거 될 가능성이 높습니다.

## <a name="implementing-hosted-content-purchase"></a>호스팅된 콘텐츠 구매 구현

다음 정보는 전체 [앱에서 전체 구매 문서](~/ios/platform/in-app-purchasing/index.md)와 함께 읽어야 합니다. 이 문서의 정보는 호스팅된 콘텐츠와 이전 구현 간의 차이점을 중점적으로 다룹니다.

### <a name="classes"></a>클래스

다음 클래스는 iOS 6에서 호스트 된 콘텐츠를 지원 하도록 추가 또는 변경 되었습니다.

- 이상 **다운로드** – 진행 중인 다운로드를 나타내는 새 클래스입니다. API는 제품별을 하나 이상 허용 하지만 처음에는 하나만 구현 되었습니다.
- 이상 **제품** – 새 속성 추가 됨: `Downloadable`, `ContentVersion`, `ContentLengths` 배열.
- **SKPaymentTransaction** – 새 속성 추가 됨:이 제품에 다운로드할 수 있는 호스트 된 콘텐츠가 있는 경우 `SKDownload` 개체의 컬렉션이 포함 된 `Downloads`.
- **SKPaymentQueue** – 새 메서드가 추가 됨: `StartDownloads`. `SKDownload` 개체를 사용 하 여이 메서드를 호출 하 여 호스팅된 콘텐츠를 페치합니다. 다운로드는 백그라운드에서 수행 될 수 있습니다.
- **SKPaymentTransactionObserver** – New 메서드: `UpdateDownloads`. 스토어 키트는 현재 다운로드 작업에 대 한 진행률 정보를 사용 하 여이 메서드를 호출 합니다.

새 `SKDownload` 클래스의 세부 정보:

- **진행률** – 사용자에 게 완료율 표시기를 표시 하는 데 사용할 수 있는 0-1 사이의 값입니다. Progress = = 1을 사용 하 여 다운로드가 완료 되었는지 여부를 검색 하지 말고 상태 = = 완료를 확인 합니다.
- **TimeRemaining** – 남은 다운로드 시간 (초)입니다. -1은 여전히 예상 값을 계산 하 고 있음을 의미 합니다.
- **상태** – 활성, 대기 중, 완료, 실패, 일시 중지 됨, 취소 됨
- **Contenturl** – 콘텐츠가 디스크에 저장 된 파일 위치 `Cache` 디렉터리입니다. 다운로드가 완료 된 후에만 채워집니다.
- **Error** – 상태가 실패 인 경우이 속성을 확인 합니다.

샘플 코드의 클래스 간 상호 작용은이 다이어그램에 표시 됩니다 (호스팅된 콘텐츠 구매 관련 코드는 녹색으로 표시 됨).

[![](changes-to-storekit-images/image26.png "Hosted content purchases is shown in green in this diagram")](changes-to-storekit-images/image26.png#lightbox)

이러한 클래스가 사용 된 샘플 코드는이 섹션의 나머지 부분에 나와 있습니다.

### <a name="custompaymentobserver-skpaymenttransactionobserver"></a>CustomPaymentObserver (SKPaymentTransactionObserver)

기존 `UpdatedTransactions` 재정의를 변경 하 여 다운로드 가능한 콘텐츠를 확인 하 고 필요한 경우 `StartDownloads`를 호출 합니다.

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

새 재정의 된 메서드 `UpdatedDownloads`는 다음과 같습니다. 스토어 키트는 `UpdatedTransactions`에서 `StartDownloads` 트리거된 후이 메서드를 호출 합니다. 이 메서드는 다운로드 진행률을 제공 하 고 다운로드가 완료 되 면 다시 제공 하기 위해 확정 되지 않은 간격으로 *여러 번* 호출 됩니다. 메서드는 `SKDownload` 개체의 배열을 허용 하므로 각 메서드 호출은 큐의 여러 다운로드 상태를 제공할 수 있습니다. 아래 구현에서 볼 수 있듯이 다운로드 상태는 매번 확인 되며 적절 한 조치를 취할 수 있습니다.

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

이 클래스에는 각 다운로드가 성공적으로 완료 된 후 호출 되는 `SaveDownload` 새 메서드가 포함 되어 있습니다.

호스트 된 콘텐츠가 성공적으로 다운로드 되 고 `Cache` 디렉터리로 압축을 풀었습니다. 의 구조입니다. PKG 파일은 모든 파일을 `Contents` 하위 디렉터리에 저장 해야 하므로 아래 코드는 `Contents` 하위 디렉터리 내에서 파일을 추출 합니다.

이 코드는 콘텐츠 패키지의 모든 파일을 반복 하 여 `ProductIdentifier`에 대해 이름이 지정 된 하위 폴더에 `Documents` 디렉터리로 복사 합니다. 마지막으로 `CompleteTransaction`를 호출 합니다 .이 메서드는 `FinishTransaction`를 호출 하 여 지불 큐에서 트랜잭션을 제거 합니다.

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

`FinishTransaction`를 호출 하면 다운로드 한 파일이 더 이상 `Cache` 디렉터리에 있지 않을 수 있습니다. `FinishTransaction`를 호출 하기 전에 모든 파일을 복사 해야 합니다.

## <a name="other-considerations"></a>기타 고려 사항

위의 예제 코드에서는 호스팅된 콘텐츠 구매를 매우 간단 하 게 구현 하는 방법을 보여 줍니다. 고려해 야 할 몇 가지 추가 사항이 있습니다.

### <a name="detecting-updated-content"></a>업데이트 된 콘텐츠 검색

호스트 된 콘텐츠 패키지를 업데이트할 수는 있지만 스토어 키트는 제품을 이미 다운로드 하 고 구매한 사용자에 게 이러한 업데이트를 푸시할 수 있는 메커니즘을 제공 하지 않습니다. 이 기능을 구현 하기 위해 코드는 새 `SKProduct.ContentVersion` 속성 (`SKProduct` `Downloadable`)을 정기적으로 확인 하 고 값이 증가 하는지 여부를 검색할 수 있습니다. 또는 푸시 알림 시스템을 빌드할 수 있습니다.

### <a name="installing-updated-content-versions"></a>업데이트 된 콘텐츠 버전 설치

위의 샘플 코드는 파일이 이미 있는 경우 파일 복사를 건너뜁니다. 다운로드 되는 콘텐츠의 최신 버전을 지원 하려는 경우에는이 방법이 적합 하지 않습니다.

또는 버전에 대 한 폴더에 콘텐츠를 복사 하 고 현재 버전을 추적 하는 방법 (예: `NSUserDefaults` 또는 완료 된 구매 레코드를 저장 하는 모든 위치).

### <a name="restoring-transactions"></a>트랜잭션 복원

`SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions`가 호출 되 면 스토어 키트는 사용자의 이전 트랜잭션을 모두 반환 합니다. 많은 수의 항목을 구입 했거나 각 구매에 큰 콘텐츠 패키지가 있는 경우 한 번에 다운로드 하기 위해 모든 항목을 큐에 배치 하면 복원으로 인해 많은 네트워크 트래픽이 발생할 수 있습니다.

제품이 연결 된 콘텐츠 패키지의 실제 다운로드와 별도로 구매 되었는지 여부를 추적 하는 것이 좋습니다.

### <a name="pausing-restarting-and-canceling-downloads"></a>다운로드 일시 중지, 다시 시작 및 취소

샘플 코드에서는이 기능을 보여 주지 않지만 호스팅된 콘텐츠 다운로드를 일시 중지 하 고 다시 시작할 수 있습니다. `SKPaymentQueue.DefaultQueue`에 `PauseDownloads`, `ResumeDownloads` 및 `CancelDownloads`에 대 한 메서드가 있습니다.

`Finished` 다운로드 하기 전에 코드에서 지불 큐에 대 한 `FinishTransaction`를 호출 하면 다운로드가 자동으로 취소 됩니다.

### <a name="setting-the-skip-backup-flag-on-the-downloaded-content"></a>다운로드 한 콘텐츠에 대 한 백업 건너뛰기 플래그 설정

Apple의 iCloud 백업 지침은 서버에서 쉽게 복원 되는 사용자가 아닌 콘텐츠를 백업 *하지* 않아야 하는 것을 제안 합니다 (불필요 하 게 iCloud 저장소를 사용 하기 때문에). 백업 특성 설정에 대 한 자세한 내용은 [파일 시스템](~/ios/app-fundamentals/file-system.md) 설명서를 참조 하세요.

## <a name="summary"></a>요약

이 문서에는 앱 내에서 iTunes 및 기타 콘텐츠를 구매 하 고 Apple의 서버를 활용 하 여 자신의 앱 내 구매를 호스트 하는 iOS6에서 저장소 키트의 두 가지 새로운 기능이 도입 되었습니다. 이 소개는 스토어 키트 기능의 구현에 대 한 전체 적용을 위해 기존 [앱 내 구매 문서](~/ios/platform/in-app-purchasing/index.md) 와 함께 읽어야 합니다.

## <a name="related-links"></a>관련 링크

- [에이 키트 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/storekit)
- [앱에서 바로 구매](~/ios/platform/in-app-purchasing/index.md)
- [나이 키트 프레임 워크 참조](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/StoreKit_Collection/_index.html)
- [이 클래스 참조](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/SKITunesProductViewController_Ref/SKStoreProductViewController.html)
- [이상 다운로드](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKDownload_Ref/Introduction/Introduction.html)
- [SKPaymentQueue](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKPaymentQueue_Class/Reference/Reference.html#/apple_ref/occ/instm/SKPaymentQueue/cancelDownloads:)
- [고 제품](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKProduct_Reference/Reference/Reference.html#/apple_ref/occ/instp/SKProduct/downloadable)
- [WWDC 비디오: 스토어 키트를 사용 하 여 제품 판매](https://developer.apple.com/videos/wwdc/2012/?include=302#302)
