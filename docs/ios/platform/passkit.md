---
title: PassKit
description: "작은 저장 하 고 바코드 및 고객의 거래 '실제 세계' 전화에 연결할 기타 정보를 표시 하는 시스템 iOS 앱입니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 74B9973B-C1E8-B727-3F6D-59C1F98BAB3A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: beff54d2b2bb72b2adf1e77819c56004b92e13f7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="passkit"></a>PassKit

_작은 저장 하 고 바코드 및 고객의 거래 '실제 세계' 전화에 연결할 기타 정보를 표시 하는 시스템 iOS 앱입니다._

Wallet Iphone에 대 한 앱 이며 iPod은 iOS 6와 연결 합니다. 저장 하 고 바코드 및 고객의 거래 '실제 세계' 전화에 연결할 기타 정보를 표시 합니다. 전달 merchants 여 생성 되어 내에서 또는 전자 메일, Url 통해 고객에 게 보낸 판매자의 iOS 앱. 전자 지갑 저장 하 고는 휴대폰의 모든 단계를 구성 하 고 날짜/시간 또는 장치의 위치에 따라 잠금 화면에서 암호 미리 알림을 표시 합니다.

이 문서는 작은, Xamarin.iOS와 전달 키트 API를 사용 하를 소개 하 고 전달 서버에서 구현 하는 방법에 설명 합니다.

 [ ![](passkit-images/image1.png "작은 저장 및 휴대폰에서 모든 단계를 구성 합니다.")](passkit-images/image1.png)


## <a name="requirements"></a>요구 사항

이 문서에 설명 된 저장소 키트 기능에는 iOS 6 및 Xcode 4.5 Xamarin.iOS 6.0 함께 필요 합니다.


## <a name="introduction"></a>소개

주요 문제는 전달 키트를 해결 하는 배포 및 관리 바코드입니다. 바코드 현재 사용법의 몇 가지 실제 예는 다음과 같습니다.

-   **영화 티켓 온라인 구매** – 고객은 일반적으로 전자 메일을 받는 들을 나타내는 바코드입니다. 이 바코드 인쇄 하 고는 영화를 검색 하 여 항목에 대 한 이동 됩니다.
-   **충성도 카드** – 고객 다양 한 전자 지갑 또는 지갑에 다른 저장소 특정 카드, 상품을 구매 표시 및 시기를 검색 합니다.
-   **쿠폰** – 쿠폰 letterboxes 및 신문 및 잡지에서 바코드로 인쇄할 수 있는 웹 페이지, 전자 메일을 통해 배포 됩니다. 고객 반환 상품, 서비스 또는 할인 받을 수 검색에 대 한 저장소를 이러한에 가져옵니다.
-   **패스 탑승객** – 영화 티켓을 구매 하는 유사 합니다.


패스 키트는 이러한 각 시나리오에 대 한 대안을 제공합니다.

-   **영화 티켓** – 고객 구매 후 전자 메일 또는 웹 사이트 링크) (통해는 이벤트 티켓 패스를 추가 합니다. 동영상 방법의 시간으로 알림으로, 잠금 화면에서의 단계는 자동으로 표시 하 고는 영화에 도착 시은 패스 쉽게 검색 및 검사에 대 한 전자 지갑에 표시 합니다.
-   **충성도 카드** – 보다는 (또는 외에) 실제 카드를 제공 하는, 저장소 발급할 수 있습니다 (전자 메일을 통해 또는 웹 사이트 로그인 후) 저장소 카드 통과 합니다. 스토어를 통해 푸시 알림 패스에서 계정의 잔액을 업데이트 하는 등 추가 기능을 제공할 수 아니며 지리적 위치 서비스를 사용 하 여 패스 수 자동으로 잠금 화면에서 고객은 저장소 위치에 가까운 경우.
-   **쿠폰** – 쿠폰 전달 수 쉽게 추적을 사용 하려면 고유한 특성을 사용 하 여 생성 하 고 전자 메일 또는 웹 사이트 링크를 통해 배포 합니다. 사용자가을 특정 위치 근처 및/또는 특정된 날짜 (예: 때 만료 날짜에 도달 하 고)에 다운로드 한 쿠폰 수 자동으로 잠금 화면에 표시 됩니다. 사용자의 전화 번호에는 쿠폰 저장 되므로, 항상 편리 이며 잘못 배치 하지 않습니다. 쿠폰 고객이 앱 스토어 링크는 고객의 커뮤니케이션을 늘리면 패스에 통합할 수 때문에 도우미 앱을 다운로드할 수 의견을 교환하실 수 있습니다.
-   **패스 탑승객** –에서는 온라인 인 프로세스 후 고객 전자 메일 또는 링크를 통해 자신의 딩 패스를 받습니다. 전송 공급자가 제공 하는 도우미 응용 프로그램 체크 인 프로세스를 포함 하 고 자신의 사용자 단위 또는 식사 선택와 같은 추가 기능을 수행 하는 고객 허용할 수 없습니다. 전송 공급자 푸시 알림을 사용 전송 지연 되거나 취소 하는 경우의 단계를 업데이트할 수 있습니다. 탑 시간으로 이러한 접근 방법의 단계는 단계에 빠르게 액세스할 수 및 알림으로 잠금 화면에서 나타납니다.


본질적으로 전달 키트에는 저장 하 고 iPhone 또는 iPod touch 장치에서 찾은 바코드를 표시할는 간단 하 고 편리한 방법을 제공 합니다. 추가 시간 및 잠금 화면 통합 위치, 푸시 알림 및 도우미 응용 프로그램 통합 매우 정교한 sales 위한 토대를 제공 티켓 발행 및 서비스를 청구 합니다.


## <a name="pass-kit-ecosystem"></a>키트 생태계를 전달 합니다.

패스 키트가 아닙니다. CocoaTouch 내 API 뿐 아니라 앱, 데이터 및 보안 공유 용이 하 게 하는 서비스 및 관리 바코드 및 기타 데이터의 더 큰 생태계의 일부입니다. 이 높은 수준의 다이어그램을 만들고 패스를 사용 하 여가 포함 될 수 있는 서로 다른 엔터티를 보여 줍니다.

 [ ![](passkit-images/image2.png "높은 수준의이 다이어그램에서는 엔터티 관련 된 만들기와 패스를 사용 하 여")](passkit-images/image2.png)

각 조각 생태계는 명확 하 게 정의 된 역할에 있습니다.

-   **Wallet** – Apple의 기본 제공 ios (iPhone 및 iPod touch)에 대 한 저장 하 고 패스를 표시 합니다. 패스 (ie 바코드와 함께 표시 됩니다는 패스에서 지역화 된 모든 데이터)는 실제 세계에서 사용 하기 위해 렌더링 되는 유일한 자리입니다.
-   **앱을 동반** – 패스 공급자 탑 통과 또는 기타 비즈니스 관련 함수에 사용자를 변경 하는 저장소 카드에 값을 추가 하는 등 발급 전달 기능을 확장 하 여 빌드한 iOS 6 응용 프로그램입니다. 부록 앱 유용 하 게 되려면 단계에 대 한 필요 하지 않습니다.
-   **서버** – 전달 생성 끌어다 배포에 대 한 서명 수 있는 보안 서버입니다. 도우미 응용 프로그램은 새 패스를 생성 하거나 기존 패스에 대 한 업데이트를 요청 하기 위해 서버에 연결할 수 있습니다. 필요에 따라 전자 지갑 패스를 업데이트 하려면 호출 웹 서비스 API에서 구현할 수 있습니다.
-   **APNS 서버** -서버에 작은 단계 APNS를 사용 하 여 지정된 된 장치에 대 한 업데이트를 알릴 수 있습니다. 변경의 세부 정보 제공을 위해 다음 서버에 연락 하는 작은 알림을 푸시하여 합니다. 부록 앱이이 기능에 대 한 APNS를 구현할 필요가 없습니다 (수신 대기할 수 있습니다는 `PKPassLibraryDidChangeNotification` ).
-   **통로 앱** – 과정 (예: 부록 앱 수행 됨)을 직접 조작 하지는 않지만 패스를 인식 하 고 작은에 추가할 수 있도록 하 여 활용도을 향상할 수 있는 응용 프로그램입니다. 메일 클라이언트, 소셜 네트워크 브라우저 및 다른 데이터 집계 앱 모두 발생할 수 전달에 대 한 링크 또는 첨부 합니다.


일부 구성 요소는 선택적 및 보다 간단 하 게 전달 키트 구현 가지 주목할 전체 에코 시스템이 복잡 합니다.

## <a name="what-is-a-pass"></a>패스를 이란?

패스를은 티켓, 쿠폰 또는 카드를 나타내는 데이터의 컬렉션입니다. 해당 개별 1 회 사용에 대 한 것일 수 있습니다 (및 따라서 항공편 번호 및 사용자 단위 할당 등의 세부 정보를 포함) 또는 개수에 관계 없이 사용자 (예: 할인 쿠폰)에서 공유할 수 있는 여러 사용 하 여 토큰 수 있습니다. 자세한 설명은 Apple의 영어로 [전달할 파일에 대 한](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html) 문서.


### <a name="types"></a>유형

현재 5 지원 형식에서 레이아웃의 위쪽 가장자리와 패스는 작은 응용 프로그램에서 식별할 수 있습니다.

-  **이벤트 티켓** -작은 반원형 잘라낸 부분입니다.
-   **딩 패스** – 쪽, 전송별 아이콘에는 노치를 않음 (예: 지정할 수 있습니다. 버스, 학습, 비행기)입니다.
-   **카드 저장** 둥근 –는 신용 카드 또는 직불 카드와 같은 상위입니다.
-  **쿠폰** : 위쪽 천공 – 합니다.
-  **제네릭** – 저장소 카드, 둥근된 top와 동일 합니다.


5 단계 형식이이 스크린 샷에 표시 됩니다 (순서 대로: 일반, 쿠폰 카드, 탑 패스 및 이벤트 티켓 저장):

 [ ![](passkit-images/image3.png "5 단계 형식이이 스크린 샷에 표시 됩니다.")](passkit-images/image3.png)

### <a name="file-structure"></a>파일 구조

패스 파일이와 ZIP 보관 파일 실제로 **.pkpass** 확장을 일부 특정 JSON 파일 (필수), 다양 한 이미지 파일 (옵션)와 지역화 된 문자열 (옵션).

-   **pass.json** – 필요 합니다. 단계에 대 한 모든 정보를 포함합니다.
-   **manifest.json** – 필요 합니다. 서명 파일을 제외한 패스에서 각 파일과이 파일 (manifest.json)에 대 한 SHA1 해시를 포함합니다.
-   **서명** – 필요 합니다. 서명 하 여 만들어진는 `manifest.json` 인증서가 iOS 프로 비전 포털에서에서 생성 되는 파일입니다.
-  **logo.png** – 선택 사항입니다.
-  **background.png** – 선택 사항입니다.
-  **icon.png** – 선택 사항입니다.
-  **지역화 가능한 문자열 파일** – 선택 사항입니다.


패스 파일의 디렉터리 구조는 다음과 같습니다 (이것이 ZIP 보관 파일의 내용을):

 [ ![](passkit-images/image4.png "패스 파일의 디렉터리 구조는 같습니다.")](passkit-images/image4.png)

### <a name="passjson"></a>pass.json

전달 서버에서 일반적으로 만들어지기 때문에 JSON 형식은 – 생성 코드에서 중인 플랫폼 제약 없는 서버에 있습니다. 모든 패스에서 정보의 세 가지 주요 정보가 됩니다.

-   **teamIdentifier** –이 앱 스토어 계정에 생성 되는 모든 패스를 연결 합니다. 이 값은 iOS 프로 비전 포털에에서 표시 합니다.
-   **passTypeIdentifier** – 그룹에 프로 비전 포털에서 레지스터 (개 이상의 형식이 생성 하는 경우) 하는 경우 함께 전달 합니다. 예를 들어 해당 고객 충성도 크레딧 달성할 수 있도록 저장소 카드 전달 형식 뿐만 아니라 별도 쿠폰 전달 하는 형식을 만들고 할인 쿠폰 배포 커피숍 만들 수 있습니다. 해당 동일한 커피숍 수도 라이브 뮤직 이벤트 저장할 수 있으며 하는 것에 대 한 이벤트 티켓 패스를 발급할 수 있습니다.
-   **serialNumber** –이 고유 문자열 `passTypeidentifier` 합니다. 값은 Wallet에 불투명 있지만 서버와 통신할 때 특정 패스를 추적 하기 위한 중요 합니다.


많은 수의 예는 다음과 같습니다. 각 패스에서 다른 JSON 키 있습니다.

``` 
{
   "passTypeIdentifier":"com.xamarin.passkitdoc.banana",  //Type Identifier (iOS Provisioning Portal)
   "formatVersion":1,                                     //Always 1 (for now)
   "organizationName":"Xamarin",                          //The name which appears on push notifications
   "serialNumber":"12345436XYZ",                          //A number for you to identify this pass
   "teamIdentifier":"XXXAAA1234",                         //Your Team ID
   "description":"Xamarin Demo",                          //
   "foregroundColor":"rgb(54,80,255)",                    //color of the data text (note the syntax)
   "backgroundColor":"rgb(209,255,247)",                  //color of the background
   "labelColor":"rgb(255,15,15)",                         //color of label text and icons
   "logoText":"Banana ",                                  //Text that appears next to logo on top
   "barcode":{                                            //Specification of the barcode (optional)
      "format":"PKBarcodeFormatQR",                       //Format can be QR, Text, Aztec, PDF417
      "message":"FREE-BANANA",                            //What to encode in barcode
      "messageEncoding":"iso-8859-1"                      //Encoding of the message
   },
   "relevantDate":"2012-09-15T15:15Z",                    //When to show pass on screen. ISO8601 formatted.
  /* The following fields are specific to which type of pass. The name of this object specifies the type, e.g., boardingPass below implies this is a boarding pass. Other options include storeCard, generic, coupon, and eventTicket */
   "boardingPass":{
/*headerFields, primaryFields, secondaryFields, and auxiliaryFields are arrays of field object. Each field has a key, label, and value*/
      "headerFields":[          //Header fields appear next to logoText
         {
            "key":"h1-label",   //Must be unique. Used by iOS apps to get the data.
            "label":"H1-label", //Label of the field
            "value":"H1"        //The actual data in the field
         },
         {
            "key":"h2-label",
            "label":"H2-label",
            "value":"H2"
         }
      ],
      "primaryFields":[       //Appearance differs based on pass type
         {
            "key":"p1-label",
            "label":"P1-label",
            "value":"P1"
         }
      ],
      "secondaryFields":[     //Typically appear below primaryFields
         {
            "key":"s1-label",
            "label":"S1-label",
            "value":"S1"
         }
      ],
      "auxiliaryFields":[    //Appear below secondary fields
         {
            "key":"a1-label",
            "label":"A1-label",
            "value":"A1"
         }
      ],
      "transitType":"PKTransitTypeAir"  //Only present in boradingPass type. Value can
                                        //Air, Bus, Boat, or Train. Impacts the picture
                                        //that shows in the middle of the pass.
   }
}
```

### <a name="barcodes"></a>바코드

지원 되는 2D 형식은: PDF417, 아즈텍, QR 합니다. Apple 클레임 1d 바코드 후면 발광 전화 화면의 검색에 적합 하지 않습니다.

바코드 아래에 표시 되는 대체 텍스트는 선택 사항 – 일부 merchants 수동으로 읽기/형식으로 수 있게 되기를 원하는 합니다.

ISO 8859-1 인코딩이 가장 일반적인 검사 프로그램 패스 읽을 검색 시스템에 의해 어떤 인코딩이 사용 됩니다.

### <a name="relevancy-lock-screen"></a>관련성 (잠금 화면)

잠금 화면에 표시할 통과 시킬 수 있는 데이터는 다음과 같은 두 종류가 있습니다.

 **위치**

예: 고객을 자주 방문 하는 저장소 또는 시네마 또는 공항 위치 패스에서 최대 10 개의 위치를 지정할 수 있습니다. 고객 도우미 앱을 통해 이러한 위치를 설정할 수 또는 공급자를 확인할 수에 사용 현황 데이터에서 (수집 경우 고객의 권한으로) 합니다.

패스 잠금 화면에 표시 되 면 해주기는 패스 잠금 화면에서 숨겨지도록 사용자 영역을 벗어날 때 계산 됩니다. 반지름은 남용 하지 않으려면 스타일 전달에 연결 됩니다.

 **날짜 및 시간**

한 번에 하나만 날짜/시간을 지정할 수 있습니다. 날짜 및 시간에는 탑 전달 및 이벤트 티켓에 대 한 잠금 화면 알림을 트리거하지에 유용 합니다.

업데이트할 수 있습니다 푸시를 통해 또는 PassKit API를 통해 날짜/시간 (예: 극장식 또는 복잡 한 스포츠 시즌 티켓)는 다양 한 용도로 사용 티켓의 경우 업데이트할 수 없습니다.

### <a name="localization"></a>지역화

IOS 응용 프로그램을 지역화 하는 방법과 비슷합니다 패스를 여러 언어로 번역 언어를 사용 하 여 특정 디렉터리 만들기 –는 `.lproj` 확장 내부에 지역화 된 요소를 배치 합니다. 텍스트 번역에 입력 해야는 `pass.strings` 패스 루트에서 대체 이미지와 같은 이름의 보유 해야 하는 지역화 된 이미지 파일입니다.

## <a name="security"></a>보안

전달은 iOS 프로 비전 포털에서에서 생성 하는 개인 인증서로 서명 됩니다. 단계에 서명 하는 단계는:

1.  패스 디렉터리에 있는 각 파일에 대 한 SHA1 해시를 계산 (포함 되지 않습니다는 `manifest.json` 또는 `signature` 둘 다 있어야이 단계에서 그래도 파일).
1.  쓰기 `manifest.json` 해시와 각 파일의 JSON 키/값 목록으로 합니다.
1.  인증서를 사용 하 여 서명 하는 `manifest.json` 파일을 라는 파일에 결과 쓰는 `signature` 합니다.
1.  결과 파일을 압축 everything 주며는 `.pkpass` 파일 확장명입니다.


사용자의 개인 키를은 패스에 서명 하는 데 필요 하기 때문에이 프로세스 사용자가 제어 하는 보안 서버에만 수행 되어야 합니다. 전달 응용 프로그램에서 생성 하 고 키를 배포 하지 마십시오.

 
## <a name="configuration-and-setup"></a>구성 및 설정

이 섹션에서는 프로 비전 정보를 설정 하 고는 첫 번째 단계를 만들 수 있도록 설명 합니다.

### <a name="provisioning-passkit"></a>PassKit 프로 비전

앱 스토어를 입력 하는 단계에 대 한 순서로 개발자 계정에 연결 되어야 합니다. 이 두 단계가 필요합니다.

1.  전달 유형 id입니다. 이라는 고유 식별자를 사용 하 여 패스를 등록 해야 합니다.
1.  개발자의 디지털 서명 사용 하 여 패스를 서명 하는 유효한 인증서를 생성 되어야 합니다.

다음 형식 ID 전달 수행을 만듭니다.


<a name="create-passid"/>

#### <a name="create-a-pass-type-id"></a>패스 유형 ID 만들기

첫 번째 단계는 각각의 다른에 대 한 전달 유형 ID를 설정 하는 _형식_ 패스를 지원할 수 있도록 합니다. 전달 ID (또는 전달 유형 식별자)의 단계에 대 한 고유 식별자를 만듭니다. 인증서를 사용 하 여 개발자 계정을 사용 하 여 패스를 연결 하려면이 ID를 사용 합니다.

1. 에 [iOS 프로 비전 포털의 인증서, 식별자 및 프로필 섹션](https://developer.apple.com/account/overview.action)로 이동 **식별자** 선택 **유형 Id 전달** 합니다. 다음 선택에서  **+**  단추를 새 패스 형식을 만들: [ ![ ] (passkit-images/passid.png "새 패스 형식 만들기")](passkit-images/passid.png)

2.   제공 된 **설명** (이름) 및 **식별자** (고유 문자열)의 단계에 대 한 합니다. 모든 전달 유형 Id 문자열으로 시작 해야 하는 참고 `pass.` 사용 하 여이 예에서 `pass.com.xamarin.coupon.banana` : [ ![ ] (passkit-images/register.png "설명과 식별자 제공")](passkit-images/register.png)


3.   전달 ID 키를 눌러 확인는 **등록** 단추입니다.


<a name="generate" />

#### <a name="generate-a-certificate"></a>인증서 생성

이 전달 유형 ID에 대 한 새 인증서를 만들려면 다음을 수행 합니다.

1.  새로 만든된 전달 ID 목록에서 선택 하 고 클릭 **편집** : [ ![ ] (passkit-images/pass-done.png "새 전달 ID 목록에서 선택")](passkit-images/pass-done.png)

    그런 다음 선택 **인증서 만들기...** :

    [ ![](passkit-images/cert-dist.png "인증서 만들기 선택")](passkit-images/cert-dist.png)


2.  요청 CSR (인증서 서명)를 만드는 단계를 따릅니다.
  
3. 키를 눌러는 **계속** 개발자 포털에서 단추 및 인증서를 생성 CSR을 업로드 합니다.

4. 인증서를 다운로드 하 고 두 번 클릭 하 여 키 집합에 설치 합니다.


이 전달 유형 ID에 대 한 인증서를 만든 것 다음 섹션 패스를 수동으로 작성 하는 방법을 설명 합니다.

에 대 한 전자 지갑 프로 비전에 대 한 자세한 내용은 참조는 [기능 작업](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) 가이드입니다.

 <a name="Create_a_Pass_Manually" />

### <a name="create-a-pass-manually"></a>패스를 수동으로 만들기

수동으로 실행할 수 전달 형식 만들었습니다 했으므로 시뮬레이터 또는 장치에서 테스트 하려면 패스를 만들 있습니다. 패스를 만드는 단계는.

-  패스 파일을 포함할 수 있는 디렉터리를 만듭니다.
-  필요한 모든 데이터를 포함 하는 pass.json 파일을 만듭니다.
-  (필요한 경우)에 폴더에 이미지를 포함 합니다.
-  폴더의 모든 파일에 대 한 SHA1 해시를 계산 하 고 manifest.json 쓰려고 합니다.
-  다운로드 한 인증서.p12 파일 manifest.json을 등록 합니다.
-  디렉터리의 내용을 압축 하 고.pkpass 확장명으로 이름을 바꿉니다.


패스를 생성 하는 데 사용할 수 있는이 문서에 대 한 샘플 코드에 소스 파일이 몇 가지 있습니다. 에 있는 파일을 사용 하 여는 `CouponBanana.raw` CreateAPassManually 디렉터리의 디렉터리입니다. 다음 파일이 있습니다.

 [ ![](passkit-images/image18.png "이 파일이 있습니다")](passkit-images/image18.png)

Pass.json 열고 JSON을 편집 합니다. 적어도 업데이트 해야 합니다는 `passTypeIdentifier` 및 `teamIdentifer` Apple 개발자 계정을 일치 하도록 합니다.

```csharp
"passTypeIdentifier" : "pass.com.xamarin.coupon.banana",
"teamIdentifier" : "?????????",
```

그런 다음 각 파일에 대 한 해시를 계산 하 고 생성 해야는 `manifest.json` 파일입니다. 완료 되 면이 다음과 같이 오류를 표시 됩니다.

```csharp
{
  "icon@2x.png" : "30806547dcc6ee084a90210e2dc042d5d7d92a41",
  "icon.png" : "87e9ffb203beb2cce5de76113f8e9503aeab6ecc",
  "pass.json" : "c83cd1441c17ecc6c5911bae530d54500f57d9eb",
  "logo.png" : "b3cd8a488b0674ef4e7d941d5edbb4b5b0e6823f",
  "logo@2x.png" : "3ccd214765507f9eab7244acc54cc4ac733baf87"
}
```

서명을 생성 해야 패스 유형 id입니다.가에 대해 생성 된 인증서 (.p12 파일)를 사용 하 여이 파일에 대 한 다음

 <a name="Signing_On_a_Mac" />


#### <a name="signing-on-a-mac"></a>Mac에서 서명

다운로드는 **전자 지갑 시드 지원 자료** 에서 [Apple 다운로드](https://developer.apple.com/downloads/index.action?name=Passbook) 사이트입니다. 사용 하 여는 `signpass` 도구 폴더에는 성공 (이 또한 계산 SHA1 해시 및.pkpass 파일로 출력을 ZIP)으로 바꿀 수 있습니다.

 <a name="Signing_On_a_PC" />


#### <a name="signing-on-a-pc"></a>PC에 로그인

샘플 있는이 문서에 대 한 코드는 라는 프로젝트 `signpassnet` .NET에서 Windows에서 실행 하 합니다. 하지만 훨씬 더 적은 유효성 검사 코드를 포함 Apple의 도구와 유사 하 게 시도 합니다.

 <a name="Testing" />


#### <a name="testing"></a>테스트

(.Zip 파일 이름을 설정 하 고 다음 열어)에서 이러한 도구의 출력을 검사 하는 경우에 다음 파일을 볼 수 있습니다 (추가 `manifest.json` 및 `signature` 파일):

 [ ![](passkit-images/image19.png "이러한 도구의 출력 검사")](passkit-images/image19.png)

서명 됨, ZIPped 하 고 (예: 파일의 이름을 변경 후. `BananaCoupon.pkpass`)를 테스트 하려면 시뮬레이터도 끌어 하거나 검색 실제 장치에 자신에 게 전자 메일로 보낼 수 있습니다. 표시 되어야 하는 화면 **추가** 다음과 같이 통과:

 [ ![](passkit-images/image20.png "패스 화면 추가")](passkit-images/image20.png)

그러나 일반적으로 서버에서 수동으로 패스 만들기 백 엔드 서버의 지원이 필요 하지 않은 쿠폰 만들기만 하는 소규모 기업 위한 옵션을 수 있습니다 해당 프로세스를 자동화할 수는 있습니다.

 <a name="Wallet" />

## <a name="wallet"></a>Wallet

작은 전달 키트 에코 시스템의 중앙 부분입니다. 이 스크린 샷 빈 전자 지갑과 패스 목록 및 개별 패스의 모양을 보여줍니다.

 [ ![](passkit-images/image21.png "이 스크린 샷 빈 전자 지갑 및 패스 목록 및 개별 패스의 모양을 보여 줍니다.")](passkit-images/image21.png)

Wallet의 기능은 다음과 같습니다.

-  것 전달 해당 바코드를 스캔 하는 데 렌더링 되는 유일한 위치입니다.
-  업데이트에 대 한 설정을 변경할 수 있습니다. 설정 된 경우 푸시 알림을 전달에서 데이터에 대 한 업데이트를 트리거할 수 있습니다.
-  사용자가을 사용 하도록 설정 하거나 잠금 화면 통합을 사용 하지 않도록 설정 합니다. 설정 된 경우이 단계에 포함 된 관련 시간 및 위치 데이터에 따라은 패스의 잠금 화면에 자동으로 표시를 수 있습니다.
-  패스의 뒷면 전달 JSON에서 웹 서버 URL를 제공 하는 경우 끌어오기-새로 고침을 지원 합니다.
-  도우미 응용 프로그램 (열거나 수 다운로드)은 전달 JSON에서 응용 프로그램의 ID를 제공 하는 경우.
-  패스 그다지 shredding 애니메이션) (으로 삭제할 수 있습니다.


 <a name="Getting_Passes_into_Wallet" />

## <a name="adding-passes-into-wallet"></a>추가 전자 지갑에 전달

전달 다음과 같은 방법으로 작은에 추가할 수 있습니다.

* **통로 앱** -이러한 직접 조작 하지 전달, 단순히 통과 파일 로드 및 전자 지갑 추가 하는 옵션과 함께 표시 합니다. 

* **앱 동반** -이러한 패스를 배포 하 고을 탐색 하거나 편집 하 여 추가 기능을 제공 하는 공급자가 기록 됩니다. Xamarin.iOS 응용 프로그램 만들고 패스를 조작 하는 전달 키트 API에 액세스할 수 있어야 합니다. 패스 추가할 수 있습니다를 사용 하 여 작은 `PKAddPassesViewController`합니다. 이 프로세스에서 보다 자세히 설명 되어는 **도우미 응용 프로그램** 이 문서의 섹션.

### <a name="conduit-applications"></a>통로 응용 프로그램

통로 응용 프로그램은 사용자를 대신 하 여 패스를 받을 수 있습니다 콘텐츠 형식을 인식 하 고는 작은에 추가할 기능을 제공 하도록 프로그래밍할 수 해야 하는 중간 응용 프로그램. 통로 앱의 예입니다.

-   **메일** – 첨부 파일로 패스를 인식 합니다.
-   **Safari** – 전달 URL 링크를 클릭 하면 전달 콘텐츠 형식을 인식 합니다.
-   **다른 사용자 지정 앱** – 링크 (소셜 미디어 클라이언트, 메일 판독기 등)을 열거나 첨부 파일을 수신 하는 앱입니다.


이 스크린샷은 방법을 **메일** ios에서 6 인식 패스 첨부 파일 (처리) 하는 경우 및 기능을 제공 **추가** 전자 지갑 되 게 합니다.

 [ ![](passkit-images/image22.png "이 스크린샷은 iOS 6의에서 메일 패스 첨부 파일을 인식 하는 방법을 보여 줍니다.")](passkit-images/image22.png)

 [ ![](passkit-images/image23.png "이 스크린샷은 전자 지갑을 패스 첨부 파일을 추가 하려면 메일을 제공 하는 방법을 보여 줍니다.")](passkit-images/image23.png)

패스 실행 시킬 수 있는 응용 프로그램을 작성 하는 경우 의해 인식 될 수 있습니다.

-  **파일 확장명** -.pkpass
-  **MIME 형식을** -application/vnd.apple.pkpass
-  **유틸리티** – com.apple.pkpass


통로 응용 프로그램의 기본 동작 패스 파일을 검색 하 고 전달 키트를 호출 하는 것 `PKAddPassesViewController` 사용자에 게 자신의 전자 지갑을의 단계를 추가 하는 옵션입니다. 이 뷰-컨트롤러의 구현에는 다음 섹션에 대해서는 **도우미 응용 프로그램**합니다.

통로 응용 프로그램 특정 전달 유형 ID에 대 한 도우미 응용 프로그램과 동일한 방식으로 프로 비전 할 필요가 없습니다.

## <a name="companion-applications"></a>도우미 응용 프로그램

도우미 응용 프로그램을 응용 프로그램와 연결 된 작업 과정을 패스를 만들기, 업데이트 하는 단계와 관련 된 정보 전달 관리 등을 위한 추가 기능을 제공 합니다.

작은의 기능을 복제 하는 도우미 응용 프로그램 하지 않아야 합니다. 검색에 대 한 패스가 표시 하려면 없습니다.

이 섹션의 나머지 부분이에서는이 기본 도우미 앱을 빌드할 상호 작용 하는 전달 키트를 사용 하는 방법을 설명 합니다.

### <a name="provisioning"></a>프로 비전

작은 저장소 기술 이기 때문에 응용 프로그램 별도로 제공 해야 하 고 팀 프로 비전 프로필 또는 와일드 카드 응용 프로그램 ID를 사용할 수 없습니다. 참조는 [기능 작업](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) 가이드는 작은 응용 프로그램에 대 한 고유 앱 ID 및 프로비저닝 프로필을 만들려고 합니다.

### <a name="entitlements"></a>권한 부여

**Entitlements.plist** 최근에 사용한 모든 Xamarin.iOS 프로젝트에 파일을 포함 해야 합니다. 단계에 따라 새 Entitlements.plist 파일을 추가 하려면는 [자격과 작업](~/ios/deploy-test/provisioning/entitlements.md) 가이드입니다.

설정 하려면 다음을 수행 하는 자격:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

두 번 클릭 하 고 **Entitlements.plist** Entitlements.plist 편집기를 열려면 솔루션 패드에서 파일:

![](passkit-images/image31.png "Entitlements.plst 편집기")

작은 섹션에서 선택 된 **활성화 전자 지갑** 옵션

![](passkit-images/image32.png "Wallet 자격을 사용 하도록 설정")


모든 형식을 전달할 수 있도록 앱에 대 한 기본 옵션이입니다. 그러나 앱을 제한 하 고 팀 패스 형식의 하위 집합을 허용 하는 데 수는 있습니다. 이 select를 사용 하도록 설정 하려면는 **팀의 허용 하위 집합에 형식을 전달** 허용할 하위 집합의 패스 유형 식별자를 입력 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

두 번 클릭은 **Entitlements.plist** XML 소스 파일을 열 파일입니다.

Wallet 자격을 추가 하려면 설정는 **속성** 를 `Passbook Identifiers` 드롭다운에는 자동으로 설정 하는 **형식** `Array`합니다. 그런 다음 문자열을 설정 **값** 를 `$(TeamIdentifierPrefix)*`:

![](passkit-images/image33.png "Wallet 자격을 사용 하도록 설정")

앱에서 모든 패스 유형을 허용할 수 있습니다. 앱 제한 팀 패스 형식의 하위 집합을 허용 하는 문자열 값을 설정 합니다.

`$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)`

여기서 `pass.$(CFBundleIdentifier)` 만든 전달 id [위에](~/ios/platform/passkit.md)

-----

### <a name="debugging"></a>디버깅

응용 프로그램을 배포 하는 데 문제가 있으면 확인을 사용 하는 올바른 **프로 비전 프로필** 하 고는 `Entitlements.plist` 으로 선택한는 **사용자 지정 자격** 파일에는 **번들 서명 iPhone** 옵션입니다.

배포 하는 경우이 오류가 발생 하면:

```csharp
Installation failed: Your code signing/provisioning profiles are not correctly configured (error: 0xe8008016)
```

그런 다음 `pass-type-identifiers` 자격 배열이 잘못 되었습니다. (일치 하지 않습니다는 **프로 비전 프로필**). 전달 유형 Id 및 팀 ID가 올바른지 확인 합니다.

 <a name="Classes" />

## <a name="classes"></a>클래스

다음 키트 전달 클래스 패스를 액세스 하는 앱에 대 한 사용할 수 있습니다.

-  **PKPass** – 패스를의 인스턴스는 합니다.
-  **PKPassLibrary** – 전달 장치에 액세스 하는 API를 제공 합니다.
-  **PKAddPassesViewController** – 패스의 작은에 저장 하려면 사용자에 대 한 표시 하는 데 사용 합니다.
-  **PKAddPassesViewControllerDelegate** – Xamarin.iOS 개발자


## <a name="example"></a>예

이 문서에 대 한 샘플 코드에서 PassLibrary 프로젝트를 참조 하십시오. 작은 도우미 응용 프로그램에 필요한 다음과 같은 공통 기능을 보여 줍니다.

### <a name="check-that-wallet-is-available"></a>작은 사용 가능 인지 확인

작은에서 사용할 수 없으면 iPad의 경우 있으므로 응용 프로그램에 전달할 키트 기능에 액세스 하기 전에 확인 해야 합니다.

```csharp
if (PKPassLibrary.IsAvailable) {
    // create an instance and do stuff...
}
```

### <a name="creating-a-pass-library-instance"></a>패스 라이브러리 인스턴스 만들기

키트 전달 라이브러리가 아닙니다. 단일 응용 프로그램 해야 만들기 및 저장 하 고 인스턴스 전달 키트 API에 액세스할 수 있습니다.

```csharp
if (PKPassLibrary.IsAvailable) {
    library = new PKPassLibrary ();
    // do stuff...
}
```

### <a name="get-a-list-of-passes"></a>패스의 목록 가져오기

응용 프로그램 라이브러리에서 전달 목록이 요청할 수 있습니다. 이 목록은 권한에만 전달 하는 팀 ID로 만들고 나열 되어 있습니다를 볼 수 있도록에 자동으로 키트 전달 하 여 필터링 됩니다.

```csharp
var passes = library.GetPasses ();  // returns PKPass[]
```

참고 시뮬레이터 항상 실제 장치에서이 메서드를 테스트 해야 하므로 반환 패스의 목록 필터링 하지 않습니다. 이 목록은 두 쿠폰 추가 되 고 나면 UITableView, 다음과 같은 샘플 응용 프로그램 모양에에서 표시할 수 있습니다.

 [ ![](passkit-images/image29.png "두 개의 쿠폰 추가 된 후이 샘플에 표시 되는 앱 선택")](passkit-images/image29.png)


### <a name="displaying-passes"></a>패스를 표시합니다.

정보의 제한 집합이 전달 도우미 앱 내에서 렌더링에 사용할 수 있는 합니다.

예제 코드 에서처럼이 표준 속성이 집합에서 패스의 목록을 표시 하려면 선택 합니다.

```csharp
string passInfo =
                "Desc:" + pass.LocalizedDescription
                + "\nOrg:" + pass.OrganizationName
                + "\nID:" + pass.PassTypeIdentifier
                + "\nDate:" + pass.RelevantDate
                + "\nWSUrl:" + pass.WebServiceUrl
                + "\n#" + pass.SerialNumber
                + "\nPassUrl:" + pass.PassUrl;
```

이 문자열 샘플에 경고로 표시 됩니다.

 [ ![](passkit-images/image30.png "이 샘플에서 쿠폰 선택한 경고")](passkit-images/image30.png)

사용할 수도 있습니다는 `LocalizedValueForFieldKey()` 디자인 단계에 필드의 데이터를 검색할 메서드 (알 수 있으므로 필드 있어야) 합니다. 예제 코드에서는이 표시 되지 않습니다.

### <a name="loading-a-pass-from-a-file"></a>패스를 파일에서 로드

패스를 사용자의 권한을 함께 전자 지갑만 추가할 수 있습니다, 때문에 뷰 컨트롤러 결정할 수 있도록 하기 위해 제공 해야 합니다. 이 코드는 사용 된 **추가** 예에서 (이 바꿔야 등록 한 것으로) 응용 프로그램에 포함 된 미리 빌드된 패스를 로드 하는 단추:

```csharp
NSData nsdata;
using ( FileStream oStream = File.Open (newFilePath, FileMode.Open ) ) {
        nsdata = NSData.FromStream ( oStream );
}
var err = new NSError(new NSString("42"), -42);
var newPass = new PKPass(nsdata,out err);
var pkapvc = new PKAddPassesViewController(newPass);
NavigationController.PresentModalViewController (pkapvc, true);
```

단계에 게 제공 됩니다 **추가** 및 **취소** 옵션:

 [ ![](passkit-images/image20.png "추가 및 취소 옵션이 제공 하는 단계")](passkit-images/image20.png)

### <a name="replace-an-existing-pass"></a>기존 패스를 대체 합니다.

하지만 기존 패스를 교체는 패스가 아직 없는 경우 실패 합니다 사용자의 권한이 필요 하지 않습니다.

```csharp
if (library.Contains (newPass)) {
     library.Replace (newPass);
}
```

### <a name="editing-a-pass"></a>패스를 편집합니다.

PKPass은 변경 불가능 하지 않으므로 코드의 통과 개체를 업데이트할 수 없습니다. 패스 응용 프로그램에서 데이터 변경 하기 위해 전달의 기록을 유지 하 고 응용 프로그램을 다운로드할 수 있는 업데이트 된 값으로 새로운 패스 파일을 생성할 수 있는 웹 서버에 액세스할 수 있어야 합니다.

패스 파일 만들기 전달 개인 및 보안 유지 해야 하는 인증서로 서명 해야 하기 때문에 서버에서 수행 되어야 합니다.

사용 하 여 업데이트 된 통과 파일 생성 되 면는 `Replace` 메서드 장치에서 이전 데이터를 덮어씁니다.

### <a name="display-a-pass-for-scanning"></a>패스를 스캔 하는 데 표시

앞에서 설명한 대로 스캔에 대 한 전자 지갑 패스를를 표시할 수만 있습니다. 사용 하 여 패스를 표시할 수는 `OpenUrl` 표시 된 것 처럼 메서드:

 `UIApplication.SharedApplication.OpenUrl (p.PassUrl);`

### <a name="receiving-notifications-of-changes"></a>변경 알림을 수신합니다.

응용 프로그램 전달 라이브러리 변경 되 고 수신 대기할 수를 사용 하는 `PKPassLibraryDidChangeNotification`합니다. 변경 알림을 수신 하기 위해 해당 응용 프로그램에서 것이 좋습니다 되기 때문 백그라운드에서 업데이트를 트리거하지 때문일 수 있습니다.

```csharp
noteCenter = NSNotificationCenter.DefaultCenter.AddObserver (PKPassLibrary.DidChangeNotification, (not) => {
    BeginInvokeOnMainThread (() => {
        new UIAlertView("Pass Library Changed", "Notification Received", null, "OK", null).Show();
        // refresh the list
        var passlist = library.GetPasses ();
        table.Source = new TableSource (passlist, library);
        table.ReloadData ();
    });
}, library);  // IMPORTANT: must pass the library in
```

PKPassLibrary 단일 없기 때문에 알림에 등록 하는 경우 라이브러리 인스턴스를 전달 하는 것이 유용 합니다.

## <a name="server-processing"></a>서버에서 처리

대 한 자세한 내용은 키트 통과 지원 하기 위해 서버 응용 프로그램 빌드 소개이 문서의 범위를 벗어납니다.

제공 하는.NET 코드의 *signpassnet* 패스를 생성할 수 있는 서버 쪽 메서드에 대 한 샘플을 기준으로 사용 될 수 있습니다.

보기 [WWDC 비디오: Passbook 소개, 2 부](https://developer.apple.com/videos/wwdc/2012/?include=309#309) 27:00 분에 대 한 자세한 내용은에서 합니다.

### <a name="other-resources"></a>기타 리소스

참조 [dotnet passbook](https://github.com/tomasmcguinness/dotnet-passbook) 소스 C# 서버 쪽 코드를 엽니다.

## <a name="push-notifications"></a>푸시 알림

대 한 자세한 내용은 푸시 알림을 사용 하 여 패스를 업데이트 하려면 소개이 문서의 범위를 벗어납니다.

업데이트가 필요한 지 때 작은 웹 요청에 응답 하기 Apple에 의해 정의 된 유사한 REST API를 구현 해야 합니다. 제공 하는.NET 코드의 *signpassnet* 해당 요청으로 인해 새 패스를 생성 하기 위한 샘플을 기준으로 사용 될 수 있습니다.

보기 [WWDC 비디오: Passbook 소개, 2 부](https://developer.apple.com/videos/wwdc/2012/?include=309#309) 27:00 분에 대 한 자세한 내용은에서 합니다.

## <a name="summary"></a>요약

이 문서 전달 키트를 도입 하 고 몇 가지 그것이 유용한 이유는 이유를 설명 전체 전달 키트 솔루션에 대 한 구현 해야 하는 다양 한 부분을 설명 합니다. 이를 만드는 과정을 만드는 단계를 수동으로 Xamarin.iOS 응용 프로그램에서 전달 키트 Api에 액세스 하는 방법을 프로세스 Apple 개발자 계정을 구성 하는 데 필요한 단계를 설명 합니다.

## <a name="related-links"></a>관련 링크

- [CreateAPassManually (샘플)](https://developer.xamarin.com/samples/PassKit/)
- [PassKit 샘플](https://developer.xamarin.com/samples/monotouch/PassKit/)
- [IOS 6 소개](~/ios/platform/introduction-to-ios6/index.md)
- [Passbook 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Conceptual/PassKit_PG/Chapters/Introduction.html)
- [개발자를 위한 passbook](https://developer.apple.com/passbook/)
- [패스 파일에 대 한](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html)
- [키트 프레임 워크 참조를 전달 합니다.](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [키트 프레임 워크 참조를 전달 합니다.](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [Passbook 웹 서비스 참조](https://developer.apple.com/library/prerelease/ios/#documentation/PassKit/Reference/PassKit_WebService/WebService.html)
