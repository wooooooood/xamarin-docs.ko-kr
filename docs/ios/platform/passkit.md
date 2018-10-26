---
title: Xamarin.iOS에서 PassKit
description: Wallet 앱 iOS 장치의 디지털 패스를 저장 하는 사용자를 수 있습니다. PassKit framework에는 개발자를 패스를 프로그래밍 방식으로 조작할 수 있습니다.
ms.prod: xamarin
ms.assetid: 74B9973B-C1E8-B727-3F6D-59C1F98BAB3A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/13/2018
ms.openlocfilehash: d1c640bef41e875b3bb427d657c9c239e4c3e16d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121401"
---
# <a name="passkit-in-xamarinios"></a>Xamarin.iOS에서 PassKit

IOS Wallet 앱을 사용 하면 장치의 디지털 전달을 저장할 수 있습니다.
이러한 전달 merchants에서 생성 되며 전자 메일에 Url 통해 또는 판매자의 iOS 앱을 통해 고객에 게 전송 됩니다. 이러한 전달 탑승권 로열티 카드에 영화 티켓에서 다양 한 항목을 나타낼 수 있습니다. PassKit framework에는 개발자를 패스를 프로그래밍 방식으로 조작할 수 있습니다.

이 문서는 Wallet 및 PassKit API를 사용 하 여 Xamarin.iOS와 함께 소개 합니다.

 [![](passkit-images/image1.png "Wallet을 저장 하 고 휴대폰에서 모든 단계를 구성 합니다.")](passkit-images/image1.png#lightbox)

## <a name="requirements"></a>요구 사항

이 문서에 설명 된 PassKit 기능에는 iOS 6 및 Xamarin.iOS 6.0 함께 Xcode 4.5 필요 합니다.

## <a name="introduction"></a>소개

PassKit 해결 하는 주요 문제를 배포 및 관리 바코드입니다. 바코드 현재 사용 하는 방법의 몇 가지 실제 예에는 다음이 포함 됩니다.

-   **영화 티켓 온라인 구매** – 고객은 해당 티켓을 나타내는 바코드 전자 메일로 일반적으로 합니다. 이 바코드는 인쇄 하 고는 영화를 검색 하 여 항목에 대 한 이동 됩니다.
-   **로열티 카드** – 상품을 구매할 때 검색 및 표시, 고객에 게 다양 한 해당 wallet 나 핸드백에 있는 서로 다른 저장소 특정 카드를 포함 합니다.
-   **쿠폰** – 쿠폰 인쇄할 수 있는 웹 페이지, 신문 및 잡지에서 바코드와 레터 박스를 통해 전자 메일을 통해 배포 됩니다. 고객에 게 반환 상품, 서비스 또는 할인을 받으려면 검색에 대 한 저장소에 맞게 합니다.
-   **전달 보드** – 영화 티켓을 구매 하는 경우와 유사 합니다.

PassKit는 이러한 각 시나리오에 대 한 대안을 제공합니다.

-   **영화 티켓** – 고객 구매 후 (전자 메일 또는 웹 사이트 링크)를 통해 이벤트 티켓 패스를를 추가 합니다. 동영상 방법에 대 한 시간을 참고로, 잠금 화면에서 단계를 자동으로 표시 됩니다와 영화 도착에 패스를 쉽게 검색 하 고 스캔 Wallet에 표시 합니다.
-   **로열티 카드** – 대신 (또는 외에) 실제 카드를 제공 저장소 실행할 수 있습니다 (전자 메일을 통해 또는 웹 사이트 로그인 한 후) 저장소 카드 전달 합니다. 저장소, 푸시 알림을 통해 패스에서 계좌의 잔고를 업데이트 하는 등 추가 기능을 제공할 수 있습니다 나타나고 지리적 위치 서비스를 사용 하 여 패스를 수 자동으로 잠금 화면에 고객은 저장소 위치 근처 하는 경우.
-   **쿠폰** – 쿠폰 전달 쉽게 추적을 사용 하는 데 고유한 특성을 사용 하 여 생성 및 전자 메일 또는 웹 사이트 링크를 통해 배포할 수 있습니다. 사용자가 특정 위치 주변 및/또는 지정된 된 날짜 (예: 때 만료 날짜가 임박)에 다운로드 한 쿠폰 수 자동으로 잠금 화면에 표시 됩니다. 쿠폰 사용자의 전화에 저장 되므로 항상 유용한 이며 위치가 가져오기 수행 합니다. 쿠폰에는 고객이 앱 스토어 링크 고객과 engagement 증가 단계를 통합할 수 있으므로 도우미 앱을 다운로드할 수 있습니다 것이 좋습니다.
-   **전달 보드** – 온라인 체크 인 프로세스를 고객 전자 메일 또는 링크를 통해 해당 탑승권 받게 됩니다. 전송 자가 제공한 도우미 앱을 체크 인 프로세스를 포함을 해당 사용자 또는 음식 선택과 같은 추가 기능을 수행 하는 고객을 허용할 수 있습니다. 전송 공급자를 사용 하 여 푸시 알림을 업데이트 pass 전송 지연 또는 취소 하는 경우. 탑승 시간 접근 방식으로 알림으로 및 패스에 빠르게 액세스할 수 패스에 잠금 화면에 표시 됩니다.

본질적으로 PassKit 저장 하 고 iOS 장치에서 바코드를 표시 하는 간단 하 고 편리한 방법을 제공 합니다. 추가 시간 및 위치 잠금 화면 통합을 사용 하 여 푸시 알림 및 관련 응용 프로그램 통합 매우 복잡 한 판매에 대 한 토대를 제공 티켓팅 및 서비스를 청구 합니다.

## <a name="passkit-ecosystem"></a>PassKit 에코 시스템

PassKit 아니며 CocoaTouch 내에서 API만, 대신 앱, 데이터 및 보안 공유에 도움이 되는 서비스 및 관리 바코드 및 기타 데이터를 더 큰 에코 시스템의 일부입니다. 이 높은 수준의 다이어그램을 만들고 패스를 사용 하 여 관련 될 수 있는 다양 한 엔터티를 보여 줍니다.

 [![](passkit-images/image2.png "이 높은 수준의 다이어그램에서 엔터티를 표시와 관련 된 만들기 및 전달 사용")](passkit-images/image2.png#lightbox)

에코 시스템의 각 부분을 명확 하 게 정의 된 역할에 있습니다.

-   **Wallet** – Apple의 기본 제공 iOS 앱을 저장 하 고 전달 표시 합니다. Pass (ie 바코드와 함께 표시 됩니다는 패스에서 지역화 된 모든 데이터) 실제 세계에서 사용 하기 위해 렌더링 되는 유일한 위치입니다.
-   **도우미 앱** –에서 제작한 iOS 6 응용 프로그램의 저장소 카드, 탑승권 이나 다른 비즈니스 관련 함수에 사용자를 변경 하려면 값을 추가 하는 등 실행 전달 기능을 확장 하는 공급자를 전달 합니다. 도우미 앱 유용 하 게 전달에 대 한 필요 하지 않습니다.
-   **서버** – 여기서 전달 수 생성 하 고 배포에 대 한 서명 된 보안 서버입니다. 도우미 앱은 새 패스를 생성 하거나 기존 전달에 대 한 업데이트 요청을 서버에 연결할 수 있습니다. 필요에 따라 Wallet은 업데이트를 호출 하는 서비스 API에 전달 하는 웹을 구현할 수 있습니다.
-   **APNS 서버** – 서버 Wallet APNS를 사용 하 여 지정된 된 장치에서 전달에 대 한 업데이트를 알릴 수 있습니다. 변경의 세부 정보에 대 한 서버 연락을 다음는 Wallet에 알림을 푸시하십시오. 도우미 앱이이 기능에 대 한 APNS를 구현할 필요가 없습니다 (대기할 수는 `PKPassLibraryDidChangeNotification` ).
-   **통로 앱** – 직접 조작 하지는 응용 프로그램 (예: 도우미 앱 수행 됨)를 통과 하지만 전달 인식 Wallet에 추가할 수 있도록 하는 유틸리티를 향상 시킬 수 있습니다. 메일 클라이언트, 소셜 네트워크 브라우저 및 기타 데이터 집계 앱 모두 발생할 첨부 파일 또는 전달에 대 한 링크입니다.

일부 구성 요소가 선택 사항임을 훨씬 PassKit 구현 가능한는 주목할 만한 가치가 전체 에코 시스템은 복잡 합니다.

## <a name="what-is-a-pass"></a>전달 이란?

패스를는 티켓, 쿠폰 또는 카드를 나타내는 데이터의 컬렉션입니다. 해당 개별 사용자가 단일 사용을 위한 것일 수 있습니다 (및 따라서 비행 수 및 사용자 할당 등의 세부 정보를 포함) 또는 원하는 수의 사용자 (예: 할인 쿠폰)에서 공유할 수 있는 여러 사용 하 여 토큰을 될 수 있습니다. Apple에 대 한 자세한 설명을 수 [전달에 대 한 파일](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html) 문서.

### <a name="types"></a>유형

현재 5 지원 형식인 패스의 레이아웃 및 위쪽 가장자리도 Wallet 앱에서 구분할 수 있습니다.

-  **이벤트 티켓** -작은 반원형 잘라낸 부분입니다.
-   **딩 전달** – 쪽, 전송별 아이콘에 대 한 조건 (예: 지정할 수 있습니다 버스, 기차, 항공기)입니다.
-   **카드 저장** 반올림 된-신용 카드나 직불 카드와 같은 상위 합니다.
-  **쿠폰** 맨 위에 있는 천공 – 합니다.
-  **제네릭** – 저장소 카드에 둥근된 top와 동일 합니다.


5 단계 형식을이 스크린샷에 표시 됩니다 (순서 대로: 쿠폰, 일반, 카드, 탑승권 및 이벤트 티켓 저장):

 [![](passkit-images/image3.png "5 단계 형식은이 스크린샷에 표시 된 것은")](passkit-images/image3.png#lightbox)

### <a name="file-structure"></a>파일 구조

Pass 파일을 사용 하 여 ZIP 보관 파일을 실제로 **.pkpass** 확장을 일부 특정 JSON 파일 (필수), 다양 한 이미지 파일 (선택 사항) 및 지역화 된 문자열 (옵션).

-   **pass.json** – 필수 항목입니다. Pass에 대 한 모든 정보를 포함합니다.
-   **manifest.json** – 필수 항목입니다. 서명 파일을 제외 하 고 패스에서 각 파일과이 파일 (은 manifest.json)에 대 한 SHA1 해시를 포함합니다.
-   **서명** – 필수 항목입니다. 서명 하 여 만들어진 여 `manifest.json` iOS 프로 비전 포털에서에서 생성 된 인증서를 사용 하 여 파일입니다.
-  **logo.png** – 선택 사항입니다.
-  **background.png** – 선택 사항입니다.
-  **icon.png** – 선택 사항입니다.
-  **지역화 가능한 문자열 파일** – 선택 사항입니다.

Pass 파일의 디렉터리 구조는 다음과 같습니다 (이 ZIP 보관 파일의 내용을):

 [![](passkit-images/image4.png "Pass 파일의 디렉터리 구조는 다음과 같습니다.")](passkit-images/image4.png#lightbox)

### <a name="passjson"></a>pass.json

전달 서버에서 일반적으로 만들어지기 때문에 JSON 형식은 – 즉, 생성 코드를 플랫폼 제약 없는 서버에 있습니다. 모든 패스에서 정보의 세 가지 주요 부분은 다음과 같습니다.

-   **teamIdentifier** –이 앱 스토어 계정에 생성 되는 모든 패스를 연결 합니다. 이 값은 iOS 프로 비전 포털에에서 표시 합니다.
-   **passTypeIdentifier** – (둘 이상의 형식 생성) 하는 경우 그룹에 프로 비전 포털에서 등록을 함께 전달 합니다. 예를 들어 커피숍 해당 고객 충성도 크레딧을 수 있도록 저장소 카드 전달 형식을 만들 수 있지만 개별 쿠폰 만들어 할인 쿠폰을 배포 하는 형식 전달 합니다. 도 해당 동일한 커피숍 라이브 뮤직 이벤트를 저장 하 고 대해서 이벤트 티켓 패스를 실행할 수 있습니다.
-   **serialNumber** –이 고유 문자열로 `passTypeidentifier` 합니다. 값은 Wallet에 불투명 있지만 특정 전달 서버와 통신할 때 추적에 대 한 중요 합니다.

많은 수의 예는 다음과 같습니다. 각 패스에서 다른 JSON 키 같습니다.

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

지원 되는 형식은 2D: PDF417, 아즈텍 QR 합니다. Apple 1d 바코드 백라이트 전화 화면에서 검색 하는 데 적합 하지는 클레임입니다.

바코드 아래에 표시 되는 대체 텍스트는 선택 사항 – 일부 merchants 수동으로 읽기/형식 수 있게 되기를 원하는 합니다.

ISO-8859-1 인코딩을 가장 일반적인 확인 패스를 읽어들일 검색 시스템으로는 인코딩이 사용 됩니다.

### <a name="relevancy-lock-screen"></a>관련성 (잠금 화면)

잠금 화면에 표시 되는 패스를 일으킬 수 있는 데이터는 다음과 같은 두 종류가 있습니다.

 **위치**

예를 들어 고객이 자주 방문 하는 저장소 또는 영화 또는 공항의 위치는 패스에서 최대 10 개의 위치를 지정할 수 있습니다. 고객 관련 앱을 통해 이러한 위치를 설정할 수 또는 공급자 수를 결정할 이러한 사용량 데이터에서 (수집 경우 고객의 권한으로).

패스에 잠금 화면에 표시 되 면 펜스를 패스에 잠금 화면에서 숨겨지도록 사용자 영역을 벗어날 때 계산 됩니다. 반지름은 남용을 방지 하기 위해 스타일 전달에 연결 됩니다.

 **날짜 및 시간**

패스에서 하나의 날짜/시간을 지정할 수 있습니다. 날짜 및 시간 탑승권 및 이벤트 티켓에 대 한 잠금 화면 알림을 트리거하지 유용 합니다.

업데이트할 수 푸시를 통해 또는 PassKit API를 통해 날짜/시간 (예: 극장식 또는 스포츠 복합 시즌 티켓) 다양 한 용도로 사용 티켓의 경우 업데이트 될 수 있도록 합니다.

### <a name="localization"></a>지역화

패스를 여러 언어로 번역 하는 것은 iOS 응용 프로그램을 지역화 비슷합니다 – 언어를 사용 하 여 특정 디렉터리 만들기를 `.lproj` 확장 하 고 내부 지역화 된 요소를 배치 합니다. 텍스트 번역을 입력 해야는 `pass.strings` 지역화 된 이미지 전달 루트에 있는 대체 이미지와 동일한 이름 이어야 하는 동안 파일입니다.

## <a name="security"></a>보안

전달 된 iOS 프로 비전 포털에서에서 생성 하는 개인 인증서로 서명 됩니다. 패스에 서명 하는 단계는:

1.  Pass 디렉터리에 각 파일의 SHA1 해시를 계산 (포함 되지 않습니다 합니다 `manifest.json` 또는 `signature` 파일을이 단계에서 계속 존재 해야 있으며 둘 다).
1.  쓰기 `manifest.json` JSON 키/값 목록을 해당 해시를 사용 하 여 각 파일 이름입니다.
1.  인증서를 사용 하 여 로그인 합니다 `manifest.json` 라는 파일에 결과 쓰고 파일 `signature` 합니다.
1.  모든 항목을 압축 하 고 결과 파일을 `.pkpass` 파일 확장명입니다.


개인 키를 패스에 로그인 하는 데 필요 하기 때문에이 프로세스 제어 하는 보안 서버에만 수행 되어야 합니다. 응용 프로그램에서 전달 생성 키를 배포 하지 마십시오.

 
## <a name="configuration-and-setup"></a>구성 및 설정

이 섹션에서는 첫 번째 단계를 만들고 프로 비전 세부 정보를 설정 하기 위한 지침을 포함 합니다.

### <a name="provisioning-passkit"></a>PassKit 프로 비전

앱 스토어를 입력 하는 단계에 대 한 순서로 개발자 계정에 연결 되어야 합니다. 이 두 단계가 필요합니다.

1.  전달 형식 ID 라는 고유 식별자를 사용 하 여 패스를 등록 해야 합니다.
1.  개발자의 디지털 서명 사용 하 여 패스에 로그인 하려면 유효한 인증서가 생성 되어야 합니다.

패스 유형 ID do 다음을 만듭니다.

#### <a name="create-a-pass-type-id"></a>패스 유형 ID를 만들려면

전달 형식 ID에 대 한 각각의 다른 설정 하는 첫 번째 단계입니다 _형식_ 지원 해야 하는 통과 합니다. 전달 ID (또는 전달 형식 식별자)는 Pass에 대 한 고유 식별자를 만듭니다. 이 ID는 인증서를 사용 하 여 개발자 계정을 사용 하 여 패스를 연결할 사용 됩니다.

1. 에 [iOS 프로 비전 포털의 인증서, 식별자 및 프로필 섹션](https://developer.apple.com/account/overview.action), 이동할 **식별자** 선택한 **유형 Id 전달** 합니다. 선택한 합니다 **+** 단추를 새 패스 유형 만들기: [ ![](passkit-images/passid.png "새 패스 유형 만들기")](passkit-images/passid.png#lightbox)

2.   제공 된 **설명** (이름) 및 **식별자** (고유 문자열) Pass에 대 한 합니다. 모든 전달할 유형 Id 문자열을 사용 하 여 시작 해야 하는 참고 `pass.` 사용 하 여이 예제의 `pass.com.xamarin.coupon.banana` : [ ![](passkit-images/register.png "설명과 식별자를 제공 합니다.")](passkit-images/register.png#lightbox)


3.   전달 ID 키를 눌러 확인 합니다 **등록** 단추입니다.

#### <a name="generate-a-certificate"></a>인증서 생성

이 패스 유형 ID에 대 한 새 인증서를 만들려면 다음을 수행 합니다.

1.  목록에서 새로 만든된 패스 ID를 선택 하 고 클릭 **편집할** : [ ![](passkit-images/pass-done.png "새 패스 ID 목록에서 선택")](passkit-images/pass-done.png#lightbox)

    그런 다음 선택 **Create Certificate...** :

    [![](passkit-images/cert-dist.png "인증서 만들기 선택")](passkit-images/cert-dist.png#lightbox)


2.  인증서 서명 요청 (CSR을)를 만드는 단계를 따릅니다.
  
3. 키를 눌러 합니다 **계속** 개발자 포털에서 단추 및 인증서를 생성 하 여 CSR을 업로드 합니다.

4. 인증서를 다운로드 하 고 키 집합에 설치 하는 데 두 번 클릭 합니다.


하이 패스 유형 ID에 대 한 인증서를 만든 다음 섹션 패스를 수동으로 빌드하는 방법을 설명 합니다.

Wallet에 대 한 프로 비전에 대 한 자세한 내용은 참조는 [기능을 사용 하 여 작업](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) 가이드입니다.

### <a name="create-a-pass-manually"></a>패스를 수동으로 만들기

전달 형식 만들었습니다 했으므로 시뮬레이터 또는 장치에서 테스트를 통과 수동으로 만들 수 있습니다 것입니다. 패스를 만드는 단계는:

-  Pass 파일을 포함할 디렉터리를 만듭니다.
-  필요한 모든 데이터를 포함 하는 pass.json 파일을 만듭니다.
-  (필요한 경우) 폴더에 이미지를 포함 합니다.
-  폴더의 모든 파일에 대 한 SHA1 해시를 계산 하 고은 manifest.json에 작성 합니다.
-  manifest.json. p12 파일을 다운로드 한 인증서를 사용 하 여 로그인 합니다.
-  디렉터리의 내용을 압축 하 고.pkpass 확장을 사용 하 여 이름을 바꿉니다.


에 대 한 원본 파일이 일부 합니다 [샘플 코드](https://developer.xamarin.com/samples/monotouch/PassKit/) 패스를 생성 하는이 문서에 대 한 합니다. 파일을 사용 합니다 `CouponBanana.raw` CreateAPassManually 디렉터리의 디렉터리입니다. 다음 파일이 있습니다.

 [![](passkit-images/image18.png "이러한 파일이 있습니다")](passkit-images/image18.png#lightbox)

Pass.json 열고 JSON을 편집 합니다. 적어도 업데이트 해야 합니다 `passTypeIdentifier` 및 `teamIdentifer` Apple 개발자 계정에 맞게 합니다.

```csharp
"passTypeIdentifier" : "pass.com.xamarin.coupon.banana",
"teamIdentifier" : "?????????",
```

그런 다음 각 파일에 대 한 해시를 계산 하 고 생성 해야 합니다 `manifest.json` 파일입니다. 완료 되 면 다음과 같이 표시 됩니다.

```csharp
{
  "icon@2x.png" : "30806547dcc6ee084a90210e2dc042d5d7d92a41",
  "icon.png" : "87e9ffb203beb2cce5de76113f8e9503aeab6ecc",
  "pass.json" : "c83cd1441c17ecc6c5911bae530d54500f57d9eb",
  "logo.png" : "b3cd8a488b0674ef4e7d941d5edbb4b5b0e6823f",
  "logo@2x.png" : "3ccd214765507f9eab7244acc54cc4ac733baf87"
}
```

패스 유형 id입니다.가에 대해 생성 된 인증서 (. p12 파일)를 사용 하 여이 파일에 대 한 시그니처를 생성 해야 다음

#### <a name="signing-on-a-mac"></a>Mac 서명

다운로드 합니다 **Wallet 초기값 지원 자료** 에서 합니다 [Apple 다운로드](https://developer.apple.com/downloads/index.action?name=Passbook) 사이트. 사용 된 `signpass` (이 또한 계산 SHA1 해시 및.pkpass 파일로 출력 ZIP) 한 전달에 폴더를 설정 하는 도구입니다.

#### <a name="testing"></a>테스트

(.Zip 파일 이름을 설정 하 고 다음 열기)에서 이러한 도구의 출력을 검사 하려는 경우 다음 파일이 표시 됩니다 (추가 합니다 `manifest.json` 고 `signature` 파일):

 [![](passkit-images/image19.png "이러한 도구의 출력 검사")](passkit-images/image19.png#lightbox)

ZIPped 서명 하 고 (예: 파일의 이름을 변경 후 `BananaCoupon.pkpass`)를 테스트 하려면 시뮬레이터 끌어온 또는 실제 장치에서 검색할 자신에 게 전자 메일 수 있습니다. 화면에 표시 되어야 **추가** 다음과 같은 단계를 합니다.

 [![](passkit-images/image20.png "Pass 화면 추가")](passkit-images/image20.png#lightbox)

하지만 일반적으로 수동 패스 만들기는 백 엔드 서버의 지원이 필요 하지 않은 쿠폰 만들기만 하는 소규모 기업 위한 수 있습니다 서버에서 해당 프로세스를 자동화할 수는 있습니다.

## <a name="wallet"></a>Wallet

Wallet은 PassKit 에코 시스템의 중앙 부분입니다. 이 스크린 샷 빈 지갑에 및 전달 목록 및 개별 패스의 모양을 보여 줍니다.

 [![](passkit-images/image21.png "빈 지갑에 및 전달 목록 및 개별 패스의 모양을 보여 주는이 스크린샷")](passkit-images/image21.png#lightbox)

Wallet의 기능은 다음과 같습니다.

-  전달 해당 바코드를 스캔을 사용 하 여 렌더링 되는 유일한 마련입니다.
-  사용자는 업데이트에 대 한 설정을 변경할 수 있습니다. 사용 하도록 설정 하는 경우 푸시 알림을 전달에서 데이터 업데이트를 트리거할 수 있습니다.
-  사용자는 사용 하도록 설정 하거나 잠금 화면 통합을 사용 하지 않도록 설정할 수 있습니다. 사용 하도록 설정 하는 경우 따라서 패스에 포함 된 관련 시간 및 위치 데이터를 기반으로 하는 잠금 화면에 표시할 자동으로 전달 합니다.
-  패스의 뒷면 JSON 패스에서 웹 서버 URL이 제공 하는 경우 끌어오기-새로 고침을 지원 합니다.
-  도우미 앱 열 (또는 다운로드할 수) JSON 패스에서 앱의 ID를 제공 합니다.
-  Pass (사용 하 여 쓴 귀여운 제 단편화 애니메이션)을 삭제할 수 있습니다.

## <a name="adding-passes-into-wallet"></a>Wallet에 추가 전달

다음과 같은 방법으로 Wallet 전달을 추가할 수 있습니다.

* **통로 앱** – 이러한 직접 조작 하지 전달, 단순히 통과 파일을 로드 하며 Wallet에 추가 하는 옵션을 사용 하 여 사용자를 제공 합니다. 

* **도우미 앱** – 이러한 패스를 배포 하 고 찾아보거나 편집 하려면 추가 기능을 제공 하는 공급자가 기록 됩니다. Xamarin.iOS 응용 프로그램 만들기 및 조작 전달 PassKit API에 대 한 전체 액세스 경우 전달 추가할 수 있습니다 Wallet 사용 하 여 `PKAddPassesViewController`입니다. 이 프로세스에서 더 자세히 설명 되어는 **도우미 응용 프로그램** 이 문서의 섹션입니다.

### <a name="conduit-applications"></a>통로 응용 프로그램

통로 응용 프로그램은 사용자를 대신 하 여 전달 받을 수 있습니다 및 해당 콘텐츠 형식을 인식 하 고는 Wallet에 추가 하는 기능을 제공 하도록 프로그래밍 해야 하는 중간 앱입니다. 통로 앱의 예입니다.

-   **메일** – 통과로 첨부 파일을 인식 합니다.
-   **Safari** – 전달 URL 링크를 클릭할 때 Content-type 패스를 인식 합니다.
-   **다른 사용자 지정 앱** – 링크 (소셜 미디어 클라이언트, 메일 판독기 등)를 열거나 첨부 파일을 수신 하는 모든 앱.


보여 주는이 스크린샷 어떻게 **메일** ios에서 6 인식 첨부 파일을 전달 (작업) 하는 경우 및 제공 **추가** Wallet 되도록 합니다.

 [![](passkit-images/image22.png "IOS 6에서에서 메일 전달 첨부 파일을 인식 하는 방법을 보여 주는이 스크린샷")](passkit-images/image22.png#lightbox)

 [![](passkit-images/image23.png "Wallet에 통과 첨부 파일을 추가 하려면 메일을 제공 하는 방법을 보여 주는이 스크린샷")](passkit-images/image23.png#lightbox)

Pass에 대 한 통로 수 있는 앱을 작성 하는 경우 의해 인식 될 수 있습니다.

-  **파일 확장명** -.pkpass
-  **MIME 형식** -application/vnd.apple.pkpass
-  **UTI** – com.apple.pkpass


통로 응용 프로그램의 기본 작업은 패스 파일을 가져와 PassKit의 호출 `PKAddPassesViewController` 패스를 해당 Wallet에 추가 하는 옵션이 사용자에 게 합니다. 이 뷰 컨트롤러의 구현에 다음 섹션에서 설명 됩니다 **도우미 응용 프로그램**합니다.

통로 응용 프로그램 도우미 응용 프로그램과 동일한 방식에서에 대 한 특정 패스 유형 ID를 프로 비전 할 필요가 없습니다.

## <a name="companion-applications"></a>도우미 응용 프로그램

도우미 응용 프로그램을 통과 패스를 만드는, 패스를 연관 된 정보를 업데이트, 그렇지 않으면 전달 응용 프로그램과 연결 된 관리 등을 사용 하 여 작업에 대 한 추가 기능을 제공 합니다.

도우미 응용 프로그램의 Wallet 기능을 복제 하지 않아야 합니다. 검색에 대 한 전달 표시 되지 않으므로 합니다.

이 섹션의 나머지이 부분에 사용 하는 기본 도우미 앱 상호 작용 하는 PassKit 빌드하는 방법을 설명 합니다.

### <a name="provisioning"></a>프로비전

Wallet 저장소 기술 이기 때문에 응용 프로그램을 개별적으로 프로 비전 해야 하 고 팀 프로 비전 프로필 또는 와일드 카드 앱 id입니다. 사용할 수 없습니다. 참조를 [기능을 사용 하 여 작업](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) Wallet 응용 프로그램에 대 한 고유한 앱 ID와 프로 비전 프로필 만들기 가이드입니다.

### <a name="entitlements"></a>권한 부여

합니다 **Entitlements.plist** 파일 최근에 사용한 모든 Xamarin.iOS 프로젝트에 포함 되어야 합니다. 새 Entitlements.plist 파일에 추가 하려면의 단계를 수행 합니다 [자격](~/ios/deploy-test/provisioning/entitlements.md) 가이드입니다.

설정 하려면 다음을 수행 하는 권한 부여:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

두 번 클릭 합니다 **Entitlements.plist** Entitlements.plist 편집기를 열려면 Solution Pad의 파일:

![](passkit-images/image31.png "Entitlements.plst 편집기")

Wallet 섹션을 선택 합니다 **Wallet 사용** 옵션

![](passkit-images/image32.png "Wallet 자격을 사용 하도록 설정")


모든 패스 유형을 허용할 앱에 대 한 기본 옵션이입니다. 그러나 앱을 제한 하 고 팀 패스 유형의 하위 집합만 허용 하는 것이 같습니다. 이 선택을 사용 하려면 합니다 **팀의 하위 집합 허용 형식을 전달** 허용 하려는 하위 집합의 패스 유형 식별자를 입력 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

두 번 클릭 합니다 **Entitlements.plist** 파일을 XML 소스 파일을 엽니다.

Wallet 자격을 추가 하려면 설정 합니다 **속성** 하 `Passbook Identifiers` 드롭다운 목록에서 자동으로 설정 됩니다는 **형식** `Array`. 그런 다음 문자열을 설정 **값** 에 `$(TeamIdentifierPrefix)*`:

![](passkit-images/image33.png "Wallet 자격을 사용 하도록 설정")

앱에서 모든 패스 유형을 허용할 수 있습니다. 앱을 제한 하 고 팀 패스 유형의 하위 집합만 허용 하는 문자열 값을 설정 합니다.

`$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)`

여기서 `pass.$(CFBundleIdentifier)` 만든 패스 id [위에](~/ios/platform/passkit.md)

-----

### <a name="debugging"></a>디버깅

에 응용 프로그램을 배포 하는 문제가 있는 경우 올바른 사용 중인지 확인 **프로 비전 프로필** 하 고는 `Entitlements.plist` 를 선택 합니다 **사용자 지정 자격** 파일에는 **iPhone 번들 서명** 옵션입니다.

배포 하는 경우이 오류가 발생 하면:

```csharp
Installation failed: Your code signing/provisioning profiles are not correctly configured (error: 0xe8008016)
```

그런 다음 `pass-type-identifiers` 자격 배열 올바르지 않습니다. (일치 하지 않습니다 합니다 **프로 비전 프로필**). 전달 형식 Id 및 팀 ID가 올바른지 확인 합니다.

## <a name="classes"></a>클래스

다음 PassKit 클래스는 전달 액세스 하는 앱에서 사용할 수 있습니다.

-  **PKPass** – 인스턴스를 전달 합니다.
-  **PKPassLibrary** – 장치에 전달에 액세스 하는 API를 제공 합니다.
-  **PKAddPassesViewController** – 해당 Wallet에 저장 하는 사용자에 대 한 패스를 표시 하는 데 사용 합니다.
-  **PKAddPassesViewControllerDelegate** – Xamarin.iOS 개발자

## <a name="example"></a>예제

PassLibrary 프로젝트에 참조를 [샘플 코드](https://developer.xamarin.com/samples/monotouch/PassKit/) 이 문서에 대 한 합니다. 해당 Wallet 자매 응용 프로그램에 필요한 다음과 같은 공통 기능을 보여 줍니다.

### <a name="check-that-wallet-is-available"></a>Wallet 사용 가능한 지 확인

Wallet 아니므로 iPad의 경우에서 사용할 수 있는 응용 프로그램 PassKit 기능에 액세스 하기 전에 확인 해야 합니다.

```csharp
if (PKPassLibrary.IsAvailable) {
    // create an instance and do stuff...
}
```

### <a name="creating-a-pass-library-instance"></a>Pass 라이브러리 인스턴스 만들기

PassKit 라이브러리가 단일 있지 않으면, 응용 프로그램 만들기 및 저장 하 고 인스턴스 PassKit API에 액세스할 수 있습니다.

```csharp
if (PKPassLibrary.IsAvailable) {
    library = new PKPassLibrary ();
    // do stuff...
}
```

### <a name="get-a-list-of-passes"></a>전달의 목록 가져오기

응용 프로그램에서 라이브러리 목록을 전달을 요청할 수 있습니다. 이 목록은 자격에 팀 ID를 사용 하 여 만든 나열 되 고 전달을만 볼 수 있도록에 자동으로 PassKit으로 필터링 됩니다.

```csharp
var passes = library.GetPasses ();  // returns PKPass[]
```

참고 시뮬레이터가이 메서드는 실제 장치에서 항상 테스트 해야 하므로 반환 전달 목록을 필터링 하지 않습니다. 이 목록에는 UITableView 표시할 수 있습니다. 합니다 [샘플 앱](https://developer.xamarin.com/samples/monotouch/PassKit/) 다음과 같은 두 가지 쿠폰을 추가한 후:

 [![](passkit-images/image29.png "두 가지 쿠폰을 추가한 후이 샘플에 표시 되는 앱 같은")](passkit-images/image29.png#lightbox)

### <a name="displaying-passes"></a>패스를 표시합니다.

정보의 제한 집합이 전달 도우미 앱 내에서 렌더링에 사용할 수 있습니다.

예제 코드와 마찬가지로이 표준 속성이 집합에서 통과 목록을 표시 하려면 선택 합니다.

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

이 문자열에 경고로 표시 됩니다는 [샘플](https://developer.xamarin.com/samples/monotouch/PassKit/):

 [![](passkit-images/image30.png "이 샘플에서 쿠폰 선택한 경고")](passkit-images/image30.png#lightbox)

사용할 수도 있습니다는 `LocalizedValueForFieldKey()` 디자인 단계에는 필드에서 데이터를 검색 하는 방법 (알 수 있으므로 어떤 필드 있어야) 합니다. 예제 코드에서는이 표시 되지 않습니다.

### <a name="loading-a-pass-from-a-file"></a>파일에서 로드 하는 과정을

패스를 사용자의 권한을 사용 하 여 Wallet에만 추가할 수 있습니다, 되므로 뷰 컨트롤러를 결정할 수 있도록 하기 위해 제공 해야 합니다. 이 코드를 사용 합니다 **추가** (으로 대체 해야이 사용자가 등록 하는) 앱에 포함 된 미리 빌드된 패스를 로드 하려면 예제에서는 단추:

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

패스에 표시 됩니다 **추가** 하 고 **취소** 옵션:

 [![](passkit-images/image20.png "단계를 추가 하 고 취소를 사용 하 여 표시 옵션")](passkit-images/image20.png#lightbox)

### <a name="replace-an-existing-pass"></a>기존 패스를 대체 합니다.

하지만 기존 패스를 교체는 패스에 아직 없는 경우 실패 사용자의 권한이 필요 하지 않습니다.

```csharp
if (library.Contains (newPass)) {
     library.Replace (newPass);
}
```

### <a name="editing-a-pass"></a>패스를 편집합니다.

코드에서 패스 개체를 업데이트할 수 없습니다 있도록 PKPass 변경할 수 없습니다. 패스에서 데이터를 변경 하려면 응용 프로그램에 전달의 레코드를 유지 하 고 응용 프로그램을 다운로드할 수 있는 업데이트 된 값으로 새 패스 파일을 생성할 수 있는 웹 서버에 액세스할 수 있어야 합니다.

Pass 파일 생성 전달 비공개 및 보안 유지 해야 하는 인증서로 서명 해야 하기 때문에 서버에서 수행 되어야 합니다.

업데이트 된 전달 파일로 생성 된 후 사용 하 여는 `Replace` 장치에서 이전 데이터를 덮어쓸지 메서드.

### <a name="display-a-pass-for-scanning"></a>패스를 검색 하는 것에 대 한 표시

앞에서 설명한 대로 Wallet 검색에 대 한 패스를 표시할 수만 있습니다. 사용 하 여 패스를 표시할 수는 `OpenUrl` 표시 된 것 처럼 메서드:

 `UIApplication.SharedApplication.OpenUrl (p.PassUrl);`

### <a name="receiving-notifications-of-changes"></a>변경 알림을 수신합니다.

응용 프로그램 전달 라이브러리에 대 한 변경 내용을 수신 대기할 수 있습니다를 사용 하 여 `PKPassLibraryDidChangeNotification`입니다. 변경 내용에 앱에서 수신 대기 하는 것이 좋습니다 하므로 백그라운드에서 업데이트를 트리거하는 알림을 때문일 수 있습니다.

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

PKPassLibrary 단일 아니므로 알림에 등록 하는 경우 라이브러리 인스턴스를 전달 하는 것이 반드시 합니다.

## <a name="server-processing"></a>서버 처리

PassKit 지원 서버 응용 프로그램을 구축 하는 자세한 내용은이 기초 문서의 범위를 벗어납니다.

참조 [dotnet passbook](https://github.com/tomasmcguinness/dotnet-passbook) 오픈 소스 C# 서버 쪽 코드입니다.

## <a name="push-notifications"></a>푸시 알림

푸시 알림을 전달 업데이트를 사용 하는 자세한 설명은이 기초 문서의 범위를 벗어납니다.

업데이트가 필요한 경우 전자 지갑에서 웹 요청에 응답 하는 Apple에서 정의 된 유사한 REST API를 구현 해야 합니다.

Apple의를 참조 하세요 [패스를 업데이트](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/PassKit_PG/Updating.html#//apple_ref/doc/uid/TP40012195-CH5-SW1) 자세한 가이드입니다.

## <a name="summary"></a>요약

이 문서에서는 PassKit 소개가 유용한 이유는 이유 중 일부를 설명 하며 전체 PassKit 솔루션 구현 해야 하는 여러 부분을 설명 합니다. Apple 개발자 계정의 통과 되도록 패스를 그리고 수동으로 PassKit Api Xamarin.iOS 응용 프로그램에서 액세스 하는 프로세스를 구성 하는 데 필요한 단계를 설명 합니다.

## <a name="related-links"></a>관련 링크

- [개발자를 위한 wallet](https://developer.apple.com/wallet/)
- [PassKit 샘플](https://developer.xamarin.com/samples/monotouch/PassKit/)
- [Wallet 개발자 가이드](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/PassKit_PG/index.html#//apple_ref/doc/uid/TP40012195-CH1-SW1)
- [프레임 워크 – Apple Pay 및 Wallet (WWDC 비디오)](https://developer.apple.com/videos/frameworks/apple-pay-and-wallet)
- [PassKit Framework 참조](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [Passbook Web Service Reference](https://developer.apple.com/library/prerelease/ios/#documentation/PassKit/Reference/PassKit_WebService/WebService.html)
- [Pass 파일에 대 한](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html)
- [dotnet passbook](https://github.com/tomasmcguinness/dotnet-passbook), iOS Wallet 패키지 생성에 대 한 오픈 소스 라이브러리
- [iOS 6 소개](~/ios/platform/introduction-to-ios6/index.md)
