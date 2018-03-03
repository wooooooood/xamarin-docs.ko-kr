---
title: "StoreKit에 대 한 변경"
description: "iOS 6 저장소 키트 API에 두 가지 변경 내용이 도입 되었습니다.: iTunes (및 앱 스토어/iBookstore)를 표시 하는 기능 앱과는 새로운 앱 내에서 제품 구매 옵션 Apple 다운로드 가능한 파일을 호스트 합니다. 이 문서에서는 Xamarin.iOS와 해당 기능을 구현 하는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 253D37D7-44C7-D012-3641-E15DC41C2699
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: cbaa389e4a115be2face2b72db6108c836676dc7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="changes-to-storekit"></a>StoreKit에 대 한 변경

_iOS 6 저장소 키트 API에 두 가지 변경 내용이 도입 되었습니다.: iTunes (및 앱 스토어/iBookstore)를 표시 하는 기능 앱과는 새로운 앱 내에서 제품 구매 옵션 Apple 다운로드 가능한 파일을 호스트 합니다. 이 문서에서는 Xamarin.iOS와 해당 기능을 구현 하는 방법을 설명 합니다._

IOS6의 주요 변경 내용은 저장소 키트에는 이러한 두 가지 새로운 기능이:

-   **앱에서 콘텐츠 표시 및 Purchasing** – 사용자가 구입할 수 있고 응용 프로그램을 종료 하지 않고 다운로드 앱, 음악, 설명서 및 기타 iTunes 콘텐츠입니다. 수준 올리기 구매 또는 것 들이 의견 및 등급을 사용자 고유의 응용 프로그램에 연결할 수 있습니다. 
-   **앱에서 바로 구매 호스팅된 콘텐츠** -Apple에서 저장 하 고 파일을 호스트 하는 별도 서버에 대 한 필요성을 자동으로 배경 다운로드를 지원 하 고 있습니다는 앱에서 바로 구매 제품에 연결 된 콘텐츠를 제공 합니다 더 적은 코드를 작성 합니다. 


기존 Xamarin.iOS와 함께에서이 문서를 읽을 것이 좋습니다. [앱에서 바로 구매](~/ios/platform/in-app-purchasing/index.md) 설명서입니다.

## <a name="requirements"></a>요구 사항

이 문서에 설명 된 저장소 키트 기능에는 iOS 6 및 Xcode 4.5 Xamarin.iOS 6.0 함께 필요 합니다.


## <a name="in-app-content-display--purchasing"></a>앱에서 콘텐츠 표시 및 구매

IOS의 새로운 앱에서 바로 구매 기능을 사용 하면 제품 정보를 보거나 구입 하 고 앱에서 제품을 다운로드 수 있습니다.
이전에 응용 프로그램은 iTunes, 앱 스토어 또는 iBookstore 원래 응용 프로그램이 사용자를 발생 시키는 트리거 해야 합니다. 자동으로이 새로운 기능 끝날 때 사용자 응용 프로그램에 게 반환 합니다.

 [ ![](changes-to-storekit-images/image1.png "응용 프로그램 구매 후 자동으로 돌아가기")](changes-to-storekit-images/image1.png)

이 유용할 수 있는, 시나리오의 여러 가지 (않음에 제한 되지 않습니다).

-   **사용자가 앱을 평가 하도록 장려** – 사용자 평가 하 고 것를 벗어나지 않고 응용 프로그램을 검토할 수 있도록 앱 스토어 페이지를 열 수 있습니다. 
-   **응용 프로그램 간 승격** – 사용자 즉시 구입/다운로드를 게시 하는 다른 앱을 볼 수 있도록 합니다. 
-   **사용자 찾기 및 콘텐츠를 다운로드할** – 사용자가 앱 검색, 관리 또는 않음 (예: 집계 하는 콘텐츠 구입 음악 관련 응용 프로그램 수 곡을 재생 목록을 제공 하 고 각 노래 응용 프로그램 내에서 구입할 수를 허용). 


한 번의 `SKStoreProductViewController` 표시 된 iTunes, 앱 스토어 또는 iBookstore 것 처럼 사용자의 제품 정보와 상호 작용할 수 있습니다. 여기에는 다음이 포함됩니다.

-  (앱)에 대 한 보기 스크린샷
-  샘플링 음악 또는 동영상 (음악, TV 쇼 및 동영상)
-  읽기 (및 쓰기) 검토,
-  구입 및 다운로드, 뷰-컨트롤러 및 저장소 키트 내에 완전히 발생 합니다. 응용 프로그램이 작동 하려면이 옵션에 대 한 추가 코드를 제공할 필요는 없습니다. 


내에서 일부 옵션 유의 `SKStoreProductViewController` 은 여전히 force 앱을 유지 하 고 클릭 같은 관련 스토어 앱을 열고 사용자 **관련 제품** 또는 앱의 **지원** 링크 합니다.

### <a name="skstoreproductviewcontroller"></a>SKStoreProductViewController

모든 앱 내에서 제품을 표시 하는 API는 매우 간단: 사용자가 만들고 표시할 필요는 `SKStoreProductViewController`합니다. 다음이 단계를 만들고 표시할 제품을 따릅니다.

1.  만들기는 `StoreProductParameters` 보기 컨트롤러에 매개 변수를 전달 하는 개체 포함 하는 `productId` 생성자에서 합니다. 
1.  인스턴스화하는 `SKProductViewController` 합니다. 클래스 수준 필드에 할당 합니다. 
1.  보기 컨트롤러에 처리기를 할당 `Finished` 뷰 컨트롤러를 해제 해야 하는 이벤트입니다. 이 이벤트는 사용자가 취소; 라고 합니다. 또는 그렇지 않은 경우 뷰 컨트롤러 내부의 트랜잭션에 작업을 마무리 합니다. 
1.  호출 된 `LoadProduct` 전달 메서드는 `StoreProductParameters` 및 완료 처리기입니다. 완료 처리기 해야 성공적으로 제품 요청 했음을 확인 하 고이 경우 제공 된 `SKProductViewController` 모달 형식으로. 제품을 검색할 수 없는 경우 적절 한 오류 처리를 추가 해야 합니다. 

### <a name="example"></a>예

*ProductView* 프로젝트에 *StoreKit* 이 문서에 대 한 샘플 코드를 구현 하는 `Buy` 모든 제품을 허용 하는 메서드 주의 Apple ID 및 표시는 `SKStoreProductViewController`합니다. 다음 코드는 주어진된 Apple ID에 대 한 제품 정보를 표시합니다.

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

실행-다운로드 하거나 구매 완전히 내 발생 하면 다음과 같이 표시 되는 앱의 `SKStoreProductViewController`:

 [ ![](changes-to-storekit-images/image2.png "실행 하면 다음과 같이 표시 되는 응용 프로그램")](changes-to-storekit-images/image2.png)

### <a name="supporting-older-operating-systems"></a>이전 운영 체제를 지원합니다.

샘플 응용 프로그램에는 이전 버전 iOS에서는 앱 스토어, iTunes 또는 iBookstore을 여는 방법을 보여 주는 코드가 포함 되어 있습니다. 사용 하 여는 `OpenUrl` 제대로 정교 하 여 메서드를 **itunes.com** URL입니다.

다음과 같이 실행할 코드를 결정 하는 버전 검사를 구현할 수 있습니다.

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

사용 하는 Apple ID 유효 하지 않을 경우 특정 유형의 네트워크 또는 인증 문제를 의미 있는 혼란 수 있으므로 다음 오류가 발생 합니다.

 `Error Domain=SKErrorDomain Code=5 "Cannot connect to iTunes Store"`

### <a name="reading-objective-c-documentation"></a>Objective C 문서 읽기

Apple 개발자 포털에서 저장소 키트에 대 한 읽기는 개발자가 프로토콜 – 나타납니다 [SKStoreProductViewControllerDelegate](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKITunesProductViewControllerDelegate_ProtocolRef/Reference/Reference.html) –이 새로운 기능을 기준으로 논의 합니다. 대리자 프로토콜에로 표시 된 하나의 메서드-productViewControllerDidFinish –는 `Finished` 이벤트에는 `SKStoreProductViewController` Xamarin.iOS에 있습니다.


## <a name="determining-apple-ids"></a>Apple Id를 확인합니다.

에 필요한 Apple ID는 `SKStoreProductViewController` 는 *번호* (일 수 없습니다 "com.xamarin.mwc2012"와 같은 번들 Id와 다름). 몇 가지 여러 가지를 표시 하려면, 아래에 나열 된 제품에 대 한 Apple ID를 확인할 수 있습니다.

### <a name="itunesconnect"></a>iTunesConnect

게시 하는 응용 프로그램에 대 한 것은 쉽게 찾을 수는 **Apple ID** iTunes Connect에서에서:

 [ ![](changes-to-storekit-images/image3.png "Apple ID를 iTunes Connect에서 찾기")](changes-to-storekit-images/image3.png)

 <a name="Search_API" />


### <a name="search-api"></a>검색 API

Apple 앱 스토어, iTunes 및는 iBookstore 모든 제품을 쿼리 하는 동적 검색 API를 제공 합니다. 검색 API에 액세스 하는 방법에 대 한 정보를 찾을 수 있습니다 [Apple의 관련 리소스](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html)(방금 등록된 되지 않은 계열사) 사람에 게는 API가 노출 하지만, 합니다. 검색 하 고 결과 JSON을 구문 분석할 수 있습니다는 `trackId` 을 사용 하면 Apple ID 즉 `SKStoreProductViewController`합니다.

결과 표시 정보 및 응용 프로그램에서 제품을 렌더링 하는 데 사용할 수 있는 아트 워크 Url을 비롯 한 다른 메타 데이터도 포함 됩니다.

다음은 몇 가지 예입니다.

-   **iBooks app*- [http://itunes.apple.com/search?term=ibooks&amp;entity=software&amp;country=us](http://itunes.apple.com/search?term=ibooks&amp;entity=software&amp;country=us) 
-   **점 및 캥거루 iBook*- [http://itunes.apple.com/search?term=dot+and+the+kangaroo&amp;엔터티 ebook =&amp;국가 = us](http://itunes.apple.com/search?term=dot+and+the+kangaroo&amp;entity=ebook&amp;country=us) 


### <a name="enterprise-partner-feed"></a>엔터프라이즈 파트너 피드

Apple에서 다운로드 가능한 데이터베이스 준비 플랫 파일의 형태로 해당 모든 제품의 전체 데이터 덤프와 승인 된 파트너를 제공합니다. 자격에 대 한 액세스는 [엔터프라이즈 파트너 피드](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-enterprise-partner-feed.html) 다음 데이터 집합에 있는 모든 제품에 대 한 Apple ID를 찾을 수 있습니다.

엔터프라이즈 파트너 피드의 많은 사용자의 멤버는는 [관련 프로그램](http://www.apple.com/itunes/affiliates) 커미션 제품 판매에서 발생 수를 허용 하 합니다. `SKStoreProductViewController` 관련 Id를 지원 하지 않습니다 (쓰기 시이 추가할 수도 있습니다 Apple 나중에).

### <a name="direct-product-links"></a>직접 제품 링크

미리 보기 URL 링크는 iTunes에서 제품에 대 한 Apple ID는 유추할 수 있습니다.
제품 링크 (앱, 음악 또는 설명서)에 iTunes에서로 시작 하는 URL의 일부를 찾을 `id` 뒤에 오는 번호를 사용 하 고 있습니다.

예를 들어는 iBooks에 대 한 직접 링크

```csharp
http://itunes.apple.com/us/app/ibooks/id364709193?mt=8
```

Apple ID는 **364709193**합니다. 마찬가지로 MWC2012 앱에 대 한 직접 연결이

```csharp
http://itunes.apple.com/us/app/mwc-2012-unofficial/id496963922?mt=8
```

Apple ID는 **496963922**합니다.

## <a name="in-app-purchase-hosted-content"></a>호스트 된 콘텐츠를 앱에서 바로 구매

앱에서 바로 구매 다운로드 가능한 콘텐츠 (예: 설명서 또는 기타 미디어, 게임 수준 아트 및 구성 또는 다른 큰 파일)으로 구성 하는 경우 웹 서버에서 호스트 되어야 이러한 파일을 사용 하는 다음 및 앱을 안전 하 게 다운로드 후 코드를 통합 해야 합니다. 구입 합니다. Ios에서 6 Apple가 도입 해당 서버에 파일을 호스트할 됩니다 있는 옵션 별도 서버에 대 한 필요성을 제거 합니다. 기능은 비 소모품 제품 (소모품 또는 구독)에 사용할 수 있습니다. Apple의 호스팅 서비스를 사용 하 여의 장점은 다음과 같습니다.

-  호스팅 및 대역폭 비용을 절감 합니다.
-  아마도 확장성이 뛰어난 현재 사용 중인 모든 서버 호스트 보다 합니다. 
-  더 적은 코드 때문에 쓸 수, 모든 서버 쪽 처리를 작성할 필요가 없습니다. 
-  배경 다운로드 구현 합니다.


참고: 테스트 실제 장치와 함께 테스트 해야 하므로 ios 시뮬레이터 지원 되지 않는 앱에서 바로 구매 콘텐츠를 호스트 합니다.

### <a name="hosted-content-basics"></a>호스팅된 기본 사항

IOS 6, 이전 된 제품을 제공 하는 두 가지 (에서 더 자세하게 설명 [Xamarin 앱에서 바로 구매](~/ios/platform/in-app-purchasing/index.md) 설명서):

-   **기본 제공 제품** – 하는 '잠금을'를 구입 하 여 있지만 (코드 또는 포함 된 리소스 중 하나) 응용 프로그램에 기본 제공 기능입니다. 기본 제공 된 제품의 예로 잠금 해제 된 사진 필터 또는 게임 전원 ups를 들 수 있습니다. 
-   **제품 서버-배달 된** – 구매 후 응용 프로그램에서에서 다운로드 해야 콘텐츠를 운영 하는 서버입니다. 이 콘텐츠 구매 하는 동안 다운로드, 장치에 저장 및 다음 제품을 제공 하는 과정을 렌더링 합니다. 책, 잡지 또는 배경 아트 및 구성 파일의 구성 된 수준 게임을 예로 들 수 있습니다. 


6 Apple iOS 서버 배달 제품의 변형에는 제공: 서버에 콘텐츠 파일을 호스트 합니다. 이렇게 하면 별도 서버를 작동 하지 않아도 됩니다 Store Kit 이전에 사용자가 직접 작성 해야 했던 배경 다운로드 기능을 제공 하기 때문에 서버 배달 제품 빌드를 훨씬 더 간단 합니다. 를 이용 하려면 Apple의 호스팅 새 앱에서 바로 구매 제품에 대 한 콘텐츠를 호스팅할 수 있도록 하 고 활용 하도록 저장소 키트 코드를 수정 합니다. 제품 콘텐츠 파일은 다음 Xcode를 사용 하 여 빌드된로 Apple 서버를 검토 하 고 릴리스에 업로드 됩니다.

 [ ![](changes-to-storekit-images/image4.png "빌드 및 배달 프로세스")](changes-to-storekit-images/image4.png)

앱에서 바로 구매를 제공 하는 앱 스토어를 사용 하 여 *호스트 된 콘텐츠를* 다음 설정 및 구성 필요:

-   **iTunes Connect** – 있습니다 *해야* 사용자 대신 수집 자금을 remit 수 있도록 귀하의 은행 및 세금 정보 Apple에 제공 합니다. 제품을 판매, 구매 테스트 샌드박스 사용자 계정을 설정할 및 구성할 수 있습니다.  *호스트 된 콘텐츠 구성 해야**Apple 인 호스트를 해당 소모품 아닌 제품에 대 한* *합니다.*  
-   **iOS 프로 비전 포털** – 번들 식별자를 만들고 있는 앱에서 바로 구매를 지 원하는 모든 응용 프로그램에 대해서와 마찬가지로 응용 프로그램에 대 한 앱 스토어 액세스를 활성화 합니다. 
-   **키트 저장** – 제품을 표시, 제품 구매 및 트랜잭션을 복원에 대 한 앱에 코드를 추가 합니다.  *IOS 6 Store Kit 됩니다 또한 관리할 진행률 업데이트를 백그라운드로 제품 콘텐츠를 다운로드 합니다.* 
-   **사용자 지정 코드** – 구매 고객 정보가 추적을 제공 하는 제품 또는 서비스 구입 했습니다. 와 같은 새 iOS 6 Store Kit 클래스 활용 `SKDownload` Apple에서 호스트 된 콘텐츠를 검색 합니다. 


다음 섹션에서 만들어 구매를 관리 하는 데 패키지를 업로드 하는 호스트 된 콘텐츠를 구현 하 고이 문서에 대 한 샘플 코드를 사용 하 여 프로세스를 다운로드 하는 방법을 설명 합니다.


### <a name="sample-code"></a>샘플 코드

샘플 프로젝트 *HostedNonConsumables* (StoreKitiOS6.zip)에서 해당 호스트를 사용 하 여 콘텐츠 응용 프로그램을 구축 하는 방법을 보여 줍니다. 앱은 두 "설명서 장"를 콘텐츠가 Apple 서버에서 호스트 되는 판매를 제공 합니다. 콘텐츠는 훨씬 더 복잡 한 내용을 실제 응용 프로그램에 사용 될 수 있지만 이미지 및 텍스트 파일의 구성 됩니다.

이전, 도중 및 구입 후 응용 프로그램은 다음과 같습니다.

 [ ![](changes-to-storekit-images/image5.png "이전, 도중 및 구입 후 응용 프로그램은 다음과 같습니다.")](changes-to-storekit-images/image5.png)

텍스트 파일 및 이미지 다운로드 되 고 응용 프로그램의 문서 디렉터리에 복사 합니다. 참조는 [파일 시스템 설명서 작업](~/ios/app-fundamentals/file-system.md) 응용 프로그램 저장소에 사용할 수 있는 다른 디렉터리에 대 한 자세한 내용은 합니다.

## <a name="itunes-connect"></a>iTunes Connect

콘텐츠의 새로운 제품 ´ ֲ Apple을 만들 때 호스트를 선택 해야는 **비 소모품** 제품 유형입니다. 다른 제품 유형 콘텐츠를 호스트 하는 것을 지원 하지 않습니다. 또한 사용 하지 않아야에 대 한 콘텐츠 호스팅 *기존* 제품을 판매; 새 제품에 대 한 콘텐츠를 호스트에 설정 합니다.

 [ ![](changes-to-storekit-images/image6.png "비-사용 가능한 제품 유형 선택")](changes-to-storekit-images/image6.png)

입력 한 **제품 ID**합니다. 나중에이 제품에 대 한 콘텐츠를 만드는 경우이 방법을 필요할 수 됩니다.

 [ ![](changes-to-storekit-images/image7.png "제품 ID를 입력 합니다.")](changes-to-storekit-images/image7.png)

콘텐츠를 호스팅하는 세부 정보 구역에서 설정 됩니다. 실제 앱에서 바로 구매 하기 전에 선택을 취소 하면 "호스트 콘텐츠를 Apple" 확인란 (경우에 일부 테스트 콘텐츠를 업로드 한)를 취소 하려는 경우. 그러나 콘텐츠 호스팅 제거할 수 없습니다 앱에서 바로 구매 라인이 된 후.

 [ ![](changes-to-storekit-images/image8.png "Apple 콘텐츠 호스팅")](changes-to-storekit-images/image8.png)

콘텐츠를 호스트에 설정 된, 일단 제품으로 전환 됩니다 **업로드에 대 한 대기** 상태가이 메시지를 표시 하 고 있습니다.

 [ ![](changes-to-storekit-images/image9.png "제품은 업로드 상태에 대 한 대기 중인 입력 하 고이 메시지를 표시 합니다.")](changes-to-storekit-images/image9.png)

콘텐츠는 이제 xcode 만들어야 하며 보관 도구를 사용 하 여 업로드 합니다. 다음 섹션에 나와 콘텐츠 패키지를 만들기 위한 지침 **만들기. PKG 파일**합니다.

## <a name="creating-pkg-files"></a>만드는 중입니다. PKG 파일

Apple에 업로드 된 콘텐츠 파일에 다음 제한 사항을 충족 해야 합니다.

-  크기가 2GB를 초과할 수 없습니다.
-  실행 코드 (또는 외부 콘텐츠를 가리키는 symlink)를 포함할 수 없습니다. 
-  (.Plist 파일 포함) 올바르게 포맷 되어 있어야 하며.pkg 파일 확장자가 있습니다. 이 자동으로 Xcode를 사용 하 여 이러한 지침을 따를 경우. 


이러한 제한 사항을 충족 하는 여러 다른 파일 및 형식의 파일을 추가할 수 있습니다. 콘텐츠 응용 프로그램에 배달 되기 전에 압축 하 고 코드에 액세스 하기 전에 저장소 키트에서 압축을 푼 됩니다.

콘텐츠 패키지를 업로드 한 후 최신 내용으로 바꿀 수 있습니다. 새 콘텐츠 업로드 하 고 일반 프로세스를 통해 검토 및 승인을 위해 제출 해야 합니다. 만큼 증가 시켜야는 `ContentVersion` 필드 업데이트 된 콘텐츠 패키지에 있습니다.

### <a name="xcode-in-app-purchase-content-projects"></a>앱에서 바로 구매 콘텐츠 Xcode 프로젝트

앱에서 바로 구매 제품에 대 한 콘텐츠 패키지를 현재 만들려면 Xcode 필요 합니다. 아니요 Objective-c 코딩 필요 합니다. Xcode에 방금 파일과 plist를 포함 하는 이러한 패키지에 대 한 새 프로젝트 형식이 있습니다.

샘플 응용 프로그램에 판매에 대 한 설명서 장-각 장 콘텐츠 패키지에 포함 됩니다.

-  텍스트 파일 및
-  장을 나타내는 데 사용 되는 이미지입니다.


선택 하 여 시작 **파일 > 새 프로젝트** 메뉴에서 선택 하 고 **앱에서 바로 구매 콘텐츠**:

 [ ![](changes-to-storekit-images/image10.png "앱에서 바로 구매 콘텐츠 선택")](changes-to-storekit-images/image10.png)

입력은 **제품 이름** 및 **회사 식별자** 되도록는 **번들 식별자** 일치는 **제품 ID** iTunes에 입력 한 이 제품에 대 한 연결 합니다.

 [ ![](changes-to-storekit-images/image11.png "이름 및 식별자를 입력 합니다.")](changes-to-storekit-images/image11.png)

빈 값을 갖습니다. 이제 **앱에서 바로 구매 콘텐츠** 프로젝트. 마우스 오른쪽 단추로 클릭 수 및 **파일 추가 중...** 하거나 끌어 오십시오.에 **Project Navigator**합니다. 확인는 **있는 ContentVersion** 올바른지 (해야 1.0에서 시작 하지만 나중에 콘텐츠를 업데이트 하도록 선택 하는 경우 증가 해야) 합니다.

이 스크린 샷 프로젝트와 주 창에 표시 plist 항목에 포함 된 콘텐츠 파일 함께 Xcode를 보여 줍니다.

 [ ![](changes-to-storekit-images/image12.png "이 스크린샷은 Xcode 프로젝트와 주 창에 표시 plist 항목에 포함 된 콘텐츠 파일 사용")](changes-to-storekit-images/image12.png)

모든 콘텐츠 파일을 추가한 후이 프로젝트를 저장할를 하 고 나중에 다시 편집 하거나 업로드 프로세스를 시작 합니다.

## <a name="uploading-pkg-files"></a>업로드 합니다. PKG 파일

콘텐츠 패키지를 업로드 하는 가장 쉬운 방법은 **Xcode 보관 도구**합니다. 선택 **제품 > 보관** 시작 메뉴에서:

 ![](changes-to-storekit-images/image13.png "Archiven 선택")

아래와 같이 콘텐츠 패키지 보관 파일에 표시 됩니다. 보관 파일 유형 및 아이콘 표시 이것이 공지는 **앱에서 바로 구매 콘텐츠 보관**합니다. 클릭 **유효성 검사 중...** 실제로 업로드가 preforming 하지 않고 오류에 대 한 콘텐츠 패키지를 확인 합니다.

 [ ![](changes-to-storekit-images/image14.png "패키지 유효성 검사")](changes-to-storekit-images/image14.png)

프로그램 iTunes Connect 자격 증명을 사용 하 여 로그인:

 [ ![](changes-to-storekit-images/image15.png "프로그램 iTunes Connect 자격 증명을 사용 하 여 로그인")](changes-to-storekit-images/image15.png)

올바른 응용 프로그램 및 앱에서 구매 된이 콘텐츠를 연결 하려면 선택 합니다.

 [ ![](changes-to-storekit-images/image16.png "이 콘텐츠를 연결 하려면 올바른 응용 프로그램을 앱에서 바로 구매를 선택 합니다.")](changes-to-storekit-images/image16.png)

다음과 같은 메시지가 나타납니다.

 ![](changes-to-storekit-images/image17.png "예제 문제 메시지 없음")

비슷한 프로세스를 수행 하지만 클릭 하면 이제 **분포 중...** 콘텐츠를 업로드 실제로 합니다.

 [ ![](changes-to-storekit-images/image18.png "앱 배포")](changes-to-storekit-images/image18.png)

콘텐츠를 업로드 하려면 첫 번째 옵션을 선택 합니다.

 ![](changes-to-storekit-images/image19.png "콘텐츠를 업로드 합니다.")

다시 로그인 합니다.

 [ ![](changes-to-storekit-images/image15.png "에 로그인")](changes-to-storekit-images/image15.png)

콘텐츠를 업로드 하려면 올바른 응용 프로그램 및 앱에서 구매 레코드 선택:

 [ ![](changes-to-storekit-images/image20.png "응용 프로그램 및 앱에서 구매 레코드를 선택 합니다.")](changes-to-storekit-images/image20.png)

파일이 업로드 되는 동안 기다립니다.

 [ ![](changes-to-storekit-images/image21.png "콘텐츠 업로드 대화 상자")](changes-to-storekit-images/image21.png)

업로드가 완료 되 면 콘텐츠 앱 스토어에 제출 된 되었습니다을 설명 하는 메시지가 표시 됩니다.

 [ ![](changes-to-storekit-images/image22.png "예제 업로드에 성공 메시지")](changes-to-storekit-images/image22.png)

작업이 완료 되 면 되 면 돌아가서 제품 페이지 iTunes Connect 패키지 세부 정보를 표시 하 고에에서 **전송 준비가** 상태입니다. 이 상태에 제품이 있으면 샌드박스 환경에서 테스트를 시작할 수 있습니다. '제출' 테스트 샌드박스를 위한 제품 필요가 없습니다.

 [ ![](changes-to-storekit-images/image23.png "iTunes Connect 패키지 세부 정보를 표시 하 고 전송 상태를 준비 수")](changes-to-storekit-images/image23.png)

(예: 약간의 시간이 걸릴 수 있습니다. 몇 분 정도) 간의 보관 및 iTunes Connect 상태 업데이트를 업로드 합니다. 검토를 위해 제품을 별도로, 제출 하거나 응용 프로그램 이진와 함께에서 전송할 수 있습니다. Apple에서 공식적으로 콘텐츠를 승인한 후에이 사용할 수 있습니다는 프로덕션 환경에서 앱 스토어 앱에서 구매에 대 한.

### <a name="pkg-file-format"></a>PKG 파일 형식

Xcode 및 보관 도구를 사용 하 여 호스트 된 콘텐츠 패키지 만들기 및 업로드를 패키지 자체의 내용을 표시 되지 것을 의미 합니다. 파일 및 디렉터리 샘플 응용 프로그램에 대해 만든 패키지에서 다음과 같은는 `plist` 루트와에 제품 파일에서 파일에는 `Contents` 하위 디렉터리:

 [ ![](changes-to-storekit-images/image24.png "루트 및 콘텐츠 하위 디렉터리에 제품 파일에서 plist 파일")](changes-to-storekit-images/image24.png)

패키지의 디렉터리 구조를 확인 (특히에 있는 파일의 위치는 `Contents` 하위 디렉터리) 장치에 패키지에서 파일을 추출 하려면이 정보를 이해 해야 하기 때문에 있습니다.

### <a name="updating-package-content"></a>패키지 콘텐츠를 업데이트합니다.

승인 된 후 콘텐츠를 업데이트 하기 위한 절차:

-  Xcode에서 앱에 구매 콘텐츠 프로젝트를 편집 합니다.
-  버전 번호를 늘립니다.
-  ITunes Connect 다시 업로드 합니다. 후속 구매자 최신 버전을 자동으로 부여지 않습니다 하지만 이전 버전을 이미 있는 사용자는 알림을 받지 않습니다. 
-  앱은 사용자에 게 알리는를 최신 버전의 콘텐츠를 검색 하도록 장려 합니다. 응용 프로그램에 빌드해야 새 버전을 다운로드 하는 함수도 합니다. 이렇게 해야 저장소 키트의 복원 기능을 사용 합니다. 
-  새 버전이 있습니다 있는지 확인 하기 위해를 작성 한 후 기능을 인출 SKProducts 않음 (예: 응용 프로그램 제품 가격을 검색 하는 데 사용 되는 과정과 동일) 하 고 ContentVersion 속성이 비교 합니다. 

## <a name="purchasing-overview"></a>구매 개요

이 섹션을 읽기 전에 기존 검토 [앱에서 바로 구매 문서](~/ios/platform/in-app-purchasing/index.md)합니다.

호스팅된 콘텐츠에 포함 된 제품 될 때 발생 하는 이벤트 순서를 구매 하 고 다운로드가이 다이어그램에 나와 합니다.

 [ ![](changes-to-storekit-images/image25.png "호스팅된 콘텐츠에 포함 된 제품 될 때 발생 하는 이벤트 순서는 구입 및 다운로드")](changes-to-storekit-images/image25.png)

1.  호스트 된 콘텐츠 사용를 사용 하 여 연결 iTunes에서 새로운 제품을 만들 수 있습니다. 실제 콘텐츠의 폴더에 단순히으로 끌기 파일) (으로 Xcode에서 별도로 구성 다음 보관 및 iTunes (코딩이 필요 함)에 업로드 합니다. 각 제품 구매에 사용할 수 있는 승인을 위해 제출 다음 됩니다. 샘플 코드에서 이러한 제품 Id는 하드 코드 된, 하지만 apple 콘텐츠를 호스트 하는 것은 새로운 제품 및 콘텐츠를 iTunes Connect 제출할 때 업데이트 될 수 있도록 원격 서버에서 사용 가능한 제품 목록을 저장 하는 경우 보다 유연 합니다. 
1.  사용자는 제품을 구매 하는 경우 트랜잭션이 처리를 위해 결제 큐에 배치 됩니다. 
1.  Store Kit iTunes 서버 처리에 대 한 구매 요청을 전달합니다. 
1.  트랜잭션이는 iTunes 서버에서 완료 됩니다 (예:. 고객 청구 됩니다) 다운로드 가능한 인지 비롯 하 여 연결 된 제품 정보 수신 확인, 앱에 반환 됩니다 (그리고 있다면 파일 크기 및 기타 메타 데이터). 
1.  코드는 제품 downloadble, 인지 확인 하 고 그렇다면 또한 지불 큐에 있는 콘텐츠 다운로드 요청을 수행 해야 합니다. Store Kit iTunes 서버에이 요청을 보냅니다. 
1.  서버는 다운로드 진행률 및 예측 결과 코드를 남은 시간을 반환 하는 콜백을 제공 하는 저장소 키트에 콘텐츠 파일을 반환 합니다. 
1.  완료 되 면 하면 알림을 가져오고 캐시 폴더에 파일 위치를 전달 합니다. 
1.  코드 파일을 복사 하 고 제품을 구입 했습니다 점을 염두에 두어야 하는 모든 상태 저장, 확인 해야 합니다. 그러면 새 파일에 백업 플래그를 올바르게 설정 (힌트: 서버에서 제공 및 사용자가 편집 하지 않습니다, 아마도 건너뛰어야 백업 사용자 검색할 수 있습니다 항상 Apple 서버에서 나중에 이후). 
1.  FinishTransaction를 호출 합니다. 이 지불 큐에서 트랜잭션을 삭제 되므로 중요 합니다. 캐시 디렉터리에서 콘텐츠를 복사한 후 FinishTransaction 될 때까지 호출 하지 않으면 중요 한 이기도 합니다. FinishTransaction 호출한 후 캐시 된 파일이 신속 하 게 제거할 수 있습니다. 


## <a name="implementing-hosted-content-purchase"></a>호스트 된 콘텐츠 구매를 구현합니다.

전체와 함께에서 다음 정보를 읽어야 [앱에서 바로 구매 설명서](~/ios/platform/in-app-purchasing/index.md)합니다. 이 문서의 정보에에서 호스트 된 콘텐츠 및 이전 구현 간의 차이점에 중점을 둡니다.


### <a name="classes"></a>클래스

다음 클래스 추가 되었거나 iOS 6 지원 호스팅된 콘텐츠를 변경 합니다.

-   **SKDownload** -진행 중인 다운로드를 나타내는 새 클래스입니다. 그러나 API를 통해 둘 이상의 당-제품에 대 한 하나에 처음에 구현 되었습니다. 
-   **SKProduct** -추가 된 새 속성: `Downloadable` , `ContentVersion` , `ContentLengths` 배열입니다. 
-   **SKPaymentTransaction** – 새 속성 추가: `Downloads` 의 컬렉션이 들어 있는 `SKDownload` 이 제품에 호스트 된 콘텐츠를 다운로드할 수 있는 경우 개체입니다. 
-   **SKPaymentQueue** – 새 메서드 추가: `StartDownloads` 합니다. 이 메서드를 호출 `SKDownload` 호스팅된 콘텐츠를 인출 하는 개체입니다. 다운로드는 백그라운드에서 발생할 수 있습니다. 
-   **SKPaymentTransactionObserver** – 새 메서드: `UpdateDownloads` 합니다. Store Kit 현재 다운로드 작업에 대 한 진행률 정보로이 메서드를 호출합니다. 


새의 세부 정보 `SKDownload` 클래스:

-   **진행률** – 사용자에 게는 완료율 표시기를 표시 하는 데 사용할 수 있는 0-1 사이의 값입니다. 진행률을 사용 하지 않는 다운로드가 완료 되 면 있는지 여부를 검색, 상태를 확인 하려면 1 = = 마침 = = 합니다. 
-   **TimeRemaining** – 다운로드 남은 기간을 초 단위로 예상 합니다. -1은 예상 값을 계산 하는 여전히 의미 합니다. 
-   **상태** – 활성 대기 중, 완료, 실패, 일시 중지 됨, 취소 합니다. 
-   **ContentURL** – 콘텐츠를 저장할 위치 디스크에 파일의 `Cache` 디렉터리입니다. 다운로드가 완료 되 면에 채워집니다. 
-   **오류** -상태에 실패 한 경우이 속성을 확인 합니다. 


샘플 코드의 클래스 간의 상호 작용 (녹색으로 표시 되어 호스트 된 콘텐츠 구매 특정 코드)이이 다이어그램에 나와 있습니다.

 [ ![](changes-to-storekit-images/image26.png "호스트 된 콘텐츠 구매가이 다이어그램에는 녹색으로 표시 됩니다.")](changes-to-storekit-images/image26.png)

이러한 클래스 사용 된 예제 코드는이 섹션의 나머지 부분에 표시 됩니다.

### <a name="custompaymentobserver-skpaymenttransactionobserver"></a>CustomPaymentObserver (SKPaymentTransactionObserver)

기존 변경 `UpdatedTransactions` 호출 및 다운로드 가능한 콘텐츠를 확인 하는 재정의 `StartDownloads` 필요한 경우:

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

새 재정의 된 메서드가 `UpdatedDownloads` 아래에 표시 됩니다. 후에이 메서드를 호출 하는 저장소 키트 `StartDownloads` 에서 트리거될 `UpdatedTransactions`합니다. 이 메서드는 *여러 번* 다운로드 진행률을 제공 하기 위해 확인할 수 없는으로 다시 다운로드 마쳤음을 하 고 있습니다. 공지 메서드의 배열을 받는 `SKDownload` 개체, 메서드를 호출할 때마다 큐에 있는 여러 다운로드가의 상태를 제공할 수 있도록 합니다. 에 표시 된 대로 다운로드 상태는 아래 구현은 모든 시간 및 수행 하는 적절 한 동작을 검사 합니다.

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
            Console.WriteLine ("Cancelled"); // TODO: UI?
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

이 클래스는 새로운 방법이 포함 `SaveDownload` 각 다운로드가 성공적으로 완료 된 후 라고 합니다.

호스팅된 콘텐츠를 성공적으로 다운로드 되었으며에 압축을 푼는 `Cache` 디렉터리입니다. 구조는 합니다. PKG 파일에 저장할 수 있는 모든 파일에 필요한는 `Contents` 아래 코드 내에서 파일을 추출 하도록 하위 디렉터리는 `Contents` 하위 디렉터리입니다.

코드는 콘텐츠 패키지에 있는 모든 파일을 반복 하 고에 복사 합니다는 `Documents` 디렉터리에 대 한 명명 된 하위 폴더에는 `ProductIdentifier`합니다. 마지막 호출 하 여 `CompleteTransaction`, 되는 호출 `FinishTransaction` 트랜잭션을 지불 큐에서 제거 하려면.

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

때 `FinishTransaction` 다운로드 한 라고 파일은 더 이상에 포함 되도록 보장할 수는 `Cache` 디렉터리입니다. 호출 하기 전에 모든 파일을 복사할 `FinishTransaction`합니다.


## <a name="other-considerations"></a>기타 고려 사항

위의 예제에서는 코드에 호스팅된 콘텐츠 구매의 비교적 간단한 구현을 보여 줍니다. 몇 가지 추가 사항을 고려해 야 할 가지가 있습니다.

### <a name="detecting-updated-content"></a>업데이트 된 콘텐츠를 검색합니다.

호스트 된 콘텐츠 패키지를 업데이트할 수 있지만 저장소 키트에 이미 다운로드 하 고 제품을 구입 하는 사용자에 게 이러한 업데이트 내보내는 어떤 메커니즘을 제공 하지 않습니다. 이 기능을 구현할 코드가 새 확인할 수 `SKProduct.ContentVersion` 속성 (하는 경우는 `SKProduct` 은 `Downloadable`) 정기적으로 값이 증가 하는 경우를 검색 합니다. 또한 푸시 알림 시스템을 작성할 수 있습니다.

### <a name="installing-updated-content-versions"></a>콘텐츠 업데이트 된 버전 설치

위의 샘플 코드 파일이 이미 있으면 파일 복사를 건너뜁니다. 이 좋지 않습니다 최신 버전의 다운로드 되 고 콘텐츠를 지원 하려는 경우.

다른 방법 콘텐츠 버전은 이라는 폴더에 복사한 다음의 현재 버전 (예: 한 추적 수 있습니다. `NSUserDefaults` 완료 된 구매 레코드를 저장 하는 아무 곳에 나 또는).

### <a name="restoring-transactions"></a>복원 중인 트랜잭션

때 `SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions` 가 Store Kit 라는 사용자에 대 한 이전의 모든 트랜잭션을 반환 합니다. 많은 수의 항목을 구매한 또는 각 구매 매우 큰 콘텐츠 패키지에 있으면 다음 복원이 발생할 수 많은 네트워크 트래픽 모든 가져옵니다에 큐 다운로드 한 번에 있습니다.

제품에서 연결 된 콘텐츠 패키지를 다운로드 하는 실제 별도로 구입 했는지 여부를 추적 하십시오.

### <a name="pausing-restarting-and-canceling-downloads"></a>일시 중지, 다시 시작 및 취소 다운로드

샘플 코드는이 기능을 보여 주기 하지 않지만 일시 중지 하 고 호스팅된 콘텐츠 다운로드를 다시 시작 하는 것이 같습니다. `SKPaymentQueue.DefaultQueue` 메서드와 `PauseDownloads`, `ResumeDownloads` 및 `CancelDownloads`합니다.

코드에서 호출 하는 경우 `FinishTransaction` 되 고 다운로드 하기 전에 지불 큐에 `Finished` 해당 다운로드를 자동으로 취소 합니다.

### <a name="setting-the-skip-backup-flag-on-the-downloaded-content"></a>다운로드 된 콘텐츠에 SKIP 백업 플래그를 설정.

Apple의 iCloud 백업을 지침 쉽게 하는 서버에서 복원 되는 비 사용자 콘텐츠 해야 하는 제안 *하지* 백업할 (하기 때문에 불필요 하 게 iCloud 저장소를 사용 하 여 것). 참조는 [파일 시스템 작업](~/ios/app-fundamentals/file-system.md) 설명서에 대 한 자세한 내용은 백업 특성을 설정 합니다.

## <a name="summary"></a>요약

이 문서에 iOS6에 저장소 키트의 두 가지 새로운 기능이 도입 되었습니다: iTunes 및 응용 프로그램 내에서 다른 콘텐츠를 구입 하 고 자신의 앱에서 바로 구매를 호스트 하는 Apple 서버를 활용 합니다. 이 소개 기존과 함께에서 읽어야 [앱에서 바로 구매 문서](~/ios/platform/in-app-purchasing/index.md) 전체 범위의 Store Kit 기능을 구현 합니다.

## <a name="related-links"></a>관련 링크

- [StoreKit (샘플)](https://developer.xamarin.com/samples/StoreKit/)
- [앱에서 바로 구매](~/ios/platform/in-app-purchasing/index.md)
- [StoreKit 프레임 워크 참조](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/StoreKit_Collection/_index.html)
- [SKStoreProductViewController 클래스 참조](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/SKITunesProductViewController_Ref/SKStoreProductViewController.html)
- [iTunes 검색 API 참조](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html)
- [SKDownload](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKDownload_Ref/Introduction/Introduction.html)
- [SKPaymentQueue](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKPaymentQueue_Class/Reference/Reference.html#/apple_ref/occ/instm/SKPaymentQueue/cancelDownloads:)
- [SKProduct](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKProduct_Reference/Reference/Reference.html#/apple_ref/occ/instp/SKProduct/downloadable)
- [저장소 키트를 사용 하는 제품을 판매 중인 WWDC 비디오:](https://developer.apple.com/videos/wwdc/2012/?include=302#302)
