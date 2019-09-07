---
title: Xamarin.ios의 PassKit
description: 전자 지갑 앱을 사용 하면 iOS 사용자가 장치에 디지털 패스를 저장할 수 있습니다. 개발자는 PassKit 프레임 워크를 사용 하 여 패스를 프로그래밍 방식으로 조작할 수 있습니다.
ms.prod: xamarin
ms.assetid: 74B9973B-C1E8-B727-3F6D-59C1F98BAB3A
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/13/2018
ms.openlocfilehash: 150a4e3c1deafbabea892d5adb786374c3d97d12
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769585"
---
# <a name="passkit-in-xamarinios"></a>Xamarin.ios의 PassKit

IOS 전자 지갑 앱을 사용 하면 사용자가 장치에 디지털 패스를 저장할 수 있습니다.
이러한 패스는 상인에 의해 생성 되 고 전자 메일, Url 또는 판매자 고유의 iOS 앱을 통해 고객에 게 전송 됩니다. 이러한 패스는 영화 티켓에서 충성도 카드까지 다양 한 항목을 나타낼 수 있습니다. 개발자는 PassKit 프레임 워크를 사용 하 여 패스를 프로그래밍 방식으로 조작할 수 있습니다.

이 문서에서는 PassKit API를 사용 하 여 전자 지갑 및 사용을 소개 합니다.

 [![](passkit-images/image1.png "전자 지갑은 휴대폰의 모든 패스를 저장 하 고 구성 합니다.")](passkit-images/image1.png#lightbox)

## <a name="requirements"></a>요구 사항

이 문서에 설명 된 PassKit 기능에는 Xamarin.ios 6.0과 함께 iOS 6 및 Xcode 4.5가 필요 합니다.

## <a name="introduction"></a>소개

PassKit에서 해결 하는 주요 문제는 바코드의 배포 및 관리입니다. 현재 바코드를 사용 하는 방법에 대 한 몇 가지 실제 예는 다음과 같습니다.

- **온라인으로 영화 티켓 구매** – 고객은 일반적으로 티켓을 나타내는 바코드를 메일로 전송 합니다. 이 바코드는 인쇄 되어 항목을 검색 하는 데 사용 됩니다.
- **충성도 카드** – 고객은 상품을 구매할 때 표시 및 검색을 위해 다양 한 매장 특정 카드를 자신의 전자 지갑 또는 지갑에 운반 합니다.
- **쿠폰** – 쿠폰은 letterboxes 및 신문 및 잡지의 바코드를 통해 전자 메일을 통해 인쇄 가능한 웹 페이지를 통해 배포 됩니다. 고객은 상품, 서비스 또는 할인을 고객에 게 제공 하는 검색을 위해 스토어에 가져옵니다.
- **패스를 탑재** 하는 것과 유사 합니다.

PassKit는 이러한 각 시나리오에 대 한 대안을 제공 합니다.

- **영화 티켓** – 구매 후 고객이 이벤트 티켓 전달 (전자 메일 또는 웹 사이트 링크를 통해)을 추가 합니다. 영화에 대 한 시간이 되 면 패스가 자동으로 잠금 화면에 표시 되 고, 영화에 도착 하는 경우, 검색을 위해 쉽게 검색 하 여 전자 지갑에 표시할 수 있습니다.
- 물리적 카드를 제공 하는 것이 아니라 (또는 그 외에도 **) 카드는** 매장 카드를 통과 하는 (전자 메일을 통해 또는 웹 사이트 로그인 후) 문제를 발생 시킬 수 있습니다. 저장소는 푸시 알림을 통해 패스의 계정 잔액을 업데이트 하 고, 지리적 위치 서비스를 사용 하는 등의 추가 기능을 제공할 수 있습니다. 고객이 상점 위치에 가까워지면 패스가 잠금 화면에 자동으로 나타날 수 있습니다.
- **쿠폰** – 쿠폰 전달은 추적 및 전자 메일 또는 웹 사이트 링크를 통해 배포 하는 데 도움이 되는 고유한 특성을 사용 하 여 쉽게 생성할 수 있습니다. 다운로드 한 쿠폰은 사용자가 특정 위치에 가까이 있을 때 잠금 화면에 자동으로 표시 될 수 있으며, 또는 지정 된 날짜 (예: 만료 날짜에 근접 하는 경우)에도 자동으로 표시 됩니다. 쿠폰은 사용자의 휴대폰에 저장 되므로 항상 유용 하 고 위치가 잘못 됩니다. 앱 스토어 링크를 Pass에 통합 하 여 고객과의 참여를 높일 수 있기 때문에 쿠폰은 고객이 자매 앱을 다운로드 하도록 유도할 수 있습니다.
- **패스를 탑재** 하는 중 – 온라인 체크 인 프로세스 후 고객이 전자 메일 또는 링크를 통해 보드 전달 전달을 받게 됩니다. 전송 공급자가 제공 하는 도우미 앱에는 체크 인 프로세스가 포함 될 수 있으며 고객이 사용자 또는 식사 선택 등의 추가 기능을 수행할 수도 있습니다. 전송이 지연 되거나 취소 된 경우 전송 공급자는 푸시 알림을 사용 하 여 pass를 업데이트할 수 있습니다. 탑재 시간이 되 면 패스가 잠금 화면에 미리 알림으로 표시 되 고 패스에 빠르게 액세스할 수 있게 됩니다.

PassKit는 코어에서 iOS 장치에 바코드를 저장 하 고 표시 하는 간단 하 고 편리한 방법을 제공 합니다. 추가 시간 및 위치 잠금 화면 통합, 푸시 알림 및 부록 응용 프로그램 통합은 매우 정교한 판매, 티켓 및 청구 서비스를 위한 토대를 제공 합니다.

## <a name="passkit-ecosystem"></a>PassKit 에코 시스템

PassKit는 CocoaTouch 내에서 API가 아니라 바코드 및 기타 데이터를 안전 하 게 공유 하 고 관리할 수 있는 더 큰 앱, 데이터 및 서비스 에코 시스템의 일부입니다. 이 개략적인 다이어그램은 패스를 만들고 사용 하는 데 사용할 수 있는 여러 엔터티를 보여 줍니다.

 [![](passkit-images/image2.png "이 개략적인 다이어그램은 패스를 만들고 사용 하는 데 관련 된 엔터티를 보여줍니다.")](passkit-images/image2.png#lightbox)

에코 시스템의 각 부분에는 명확 하 게 정의 된 역할이 있습니다.

- **전자/** 반자 – Apple의 기본 제공 iOS 앱은 패스를 저장 하 고 표시 합니다. 이 유일한 장소는 실제 환경에서 사용할 수 있도록 렌더링 됩니다 (ie는 해당 패스의 모든 지역화 된 데이터와 함께 바코드를 표시 합니다).
- **동반 앱** – 저장소 카드에 값을 추가 하 여 탑재 된 패스 또는 다른 비즈니스 관련 함수에서 사용자를 변경 하는 것과 같이 통과 하는 패스의 기능을 확장 하기 위해 pass 공급자가 작성 한 iOS 6 앱입니다. 도우미 앱은 전달에 유용 하 게 사용할 필요가 없습니다.
- **서버** – 통과를 생성 하 고 배포용으로 서명할 수 있는 보안 서버입니다. 도우미 앱은 새 패스를 생성 하거나 기존 패스에 대 한 업데이트를 요청 하기 위해 서버에 연결할 수 있습니다. 필요에 따라 작은 사용자가 업데이트 통과를 위해 호출 하는 웹 서비스 API를 구현할 수 있습니다.
- **Apns 서버** – 서버에는 apns를 사용 하 여 지정 된 장치에 대 한 업데이트를 전자 지갑에 알릴 수 있는 기능이 있습니다. 변경 내용에 대 한 세부 정보를 확인 하려면 전자 메일에 전자 메일을 보낼 수 있습니다. 도우미 앱은 `PKPassLibraryDidChangeNotification` 이 기능에 대해 APNS를 구현할 필요가 없습니다 (를 수신할 수 있음).
- **응용 프로그램** (예: 좋아요 앱)을 직접 조작 하지 않는 응용 프로그램은 패스를 인식 하 고 작은 사람에 게 추가할 수 있도록 허용 하 여 유틸리티를 개선할 수 있는 응용 프로그램입니다. 메일 클라이언트, 소셜 네트워크 브라우저 및 기타 데이터 집계 앱은 모두 첨부 파일이 나 패스에 대 한 링크를 만날 수 있습니다.

전체 에코 시스템은 복잡 하므로 일부 구성 요소는 선택 사항이 며 훨씬 더 간단한 PassKit 구현도 가능 합니다.

## <a name="what-is-a-pass"></a>Pass 란?

Pass는 티켓, 쿠폰 또는 카드를 나타내는 데이터의 컬렉션입니다. 개인의 단일 사용을 위한 것일 수 있습니다. 따라서 항공편 번호 및 사용자 할당과 같은 세부 정보를 포함 하거나 할인 쿠폰과 같은 여러 사용자가 공유할 수 있는 여러 사용 토큰이 있을 수 있습니다. 자세한 설명은 Apple의 [파일 전달 정보](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html) 문서에 나와 있습니다.

### <a name="types"></a>유형

현재 5 개의 지원 되는 형식으로,이는 패스의 레이아웃 및 위쪽 가장자리를 통해 전자 지갑 앱에서 구분할 수 있습니다.

- **이벤트 티켓** – 작은 반원형 컷아웃.
- **Pass** – 노치를 나란히 탑재 하 고, 전송 관련 아이콘을 지정할 수 있습니다 (예: 버스, 학습, 비행기).
- **매장 카드** – 신용 카드 또는 직불 카드와 같이 위쪽으로 반올림 됩니다.
- **쿠폰** – 위쪽을 따라 perforated.
- **Generic** – 매장 카드와 같으며 위쪽으로 반올림 됩니다.

5 개의 패스 형식이이 스크린샷에서 표시 됩니다 (순서: 쿠폰, 일반, 매장 카드, 전달 패스 및 이벤트 티켓).

 [![](passkit-images/image3.png "이 스크린샷에는 다섯 가지 패스 형식이 나와 있습니다.")](passkit-images/image3.png#lightbox)

### <a name="file-structure"></a>파일 구조

패스 파일은 일부 특정 JSON 파일 (필수), 다양 한 이미지 파일 (옵션) 및 지역화 된 문자열 (선택 사항)을 포함 하는 확장명이 **pkpass** 인 ZIP 보관 파일입니다.

- **pass** – 필수 항목입니다. 패스에 대 한 모든 정보를 포함 합니다.
- **manifest.xml** – 필수 항목입니다. 서명 파일 및이 파일 (manifest.xml)을 제외 하 고 pass의 각 파일에 대 한 SHA1 해시를 포함 합니다.
- **signature** – 필수 항목입니다. IOS 프로 비전 포털 `manifest.json` 에서 생성 된 인증서를 사용 하 여 파일에 서명 하는 방식으로 만들어집니다.
- **로고 .png** – 선택 사항입니다.
- **background .png** – 선택 사항입니다.
- **icon .png** – 선택 사항입니다.
- **지역화할 수 있는 문자열 파일** – 선택 사항입니다.

패스 파일의 디렉터리 구조는 아래와 같습니다 (ZIP 보관 파일의 내용).

 [![](passkit-images/image4.png "여기에는 패스 파일의 디렉터리 구조가 표시 됩니다.")](passkit-images/image4.png#lightbox)

### <a name="passjson"></a>pass.json

JSON은 일반적으로 패스가 서버에서 만들어지기 때문에 형식입니다. 즉, 생성 코드는 서버에 대 한 플랫폼에 구애 받지 않습니다. 모든 패스의 세 가지 주요 정보는 다음과 같습니다.

- **Teamidentifier** – 앱 스토어 계정에 생성 하는 모든 패스를 연결 합니다. 이 값은 iOS 프로 비전 포털에 표시 됩니다.
- **passTypeIdentifier** – 프로 비전 포털에 등록 하 여 두 개 이상의 유형을 생성 하는 경우 함께 전달 합니다. 예를 들어 커피숍은 고객이 충성도 크레딧을 획득 하 고 할인 쿠폰을 만들고 배포 하기 위한 별도의 쿠폰 전달 유형을 얻을 수 있도록 매장 카드 패스 유형을 만들 수 있습니다. 동일한 커피숍에서 라이브 음악 이벤트를 보유 하 고 해당 이벤트에 대 한 이벤트 티켓이 전달 될 수도 있습니다.
- **일련** 의 고유 문자열 `passTypeidentifier` 입니다. 값은 작은 값으로 불투명 하지만 서버와 통신할 때 특정 패스를 추적 하는 데 중요 합니다.

각 패스에는 다음과 같은 여러 가지 다른 JSON 키가 있습니다.

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

### <a name="barcodes"></a>바코드가

2D 형식만 지원 됩니다. PDF417, Aztec, QR. Unsuited은 1D 바코드가 backlit 휴대폰 화면에서 스캔 하는 것을 말합니다.

바코드 아래에 표시 되는 대체 텍스트는 선택 사항입니다. 일부 상인은 수동으로 읽거나 입력할 수 있습니다.

ISO-8859-1 인코딩은 가장 일반적 이며, 패스를 읽을 검색 시스템에서 사용 하는 인코딩을 확인 합니다.

### <a name="relevancy-lock-screen"></a>관련성 (잠금 화면)

잠금이 화면에 표시 될 수 있는 데이터에는 두 가지 유형이 있습니다.

 **위치**

패스에는 최대 10 개의 위치를 지정할 수 있습니다. 예를 들어 고객에 게 자주 방문 하는 매장 또는 영화 또는 공항의 위치를 지정할 수 있습니다. 고객은 도우미 앱을 통해 이러한 위치를 설정 하거나 공급자가 사용 현황 데이터 (고객의 권한으로 수집 된 경우)에서 이러한 위치를 결정할 수 있습니다.

패스가 잠금 화면에 표시 되 면 사용자가 영역을 벗어나면 패스가 잠금 화면에서 숨겨집니다. 그러면 fence가 계산 됩니다. 악용을 방지 하기 위해 반지름이 패스 스타일에 연결 됩니다.

 **날짜 및 시간**

한 번에 하나의 날짜/시간만 지정할 수 있습니다. 날짜 및 시간은 보드 전달 및 이벤트 티켓에 대 한 잠금 화면 미리 알림을 트리거하는 데 유용 합니다.

푸시 또는 PassKit API를 통해 업데이트할 수 있으므로 다중 사용 티켓 (예: 극장식 또는 스포츠에 대 한 시즌 티켓)에서 날짜/시간을 업데이트할 수 있습니다.

### <a name="localization"></a>지역화

패스를 여러 언어로 변환 하는 것은 iOS 응용 프로그램을 지역화 하는 것과 비슷합니다. `.lproj` 확장을 사용 하 여 언어별 디렉터리를 만들고 지역화 된 요소를 내부에 저장 합니다. 텍스트 번역은 `pass.strings` 파일에 입력 해야 하는 반면, 지역화 된 이미지의 이름은 Pass root에서 대체 하는 이미지와 같아야 합니다.

## <a name="security"></a>보안

패스는 iOS 프로 비전 포털에서 생성 하는 개인 인증서를 사용 하 여 서명 됩니다. 패스에 서명 하는 단계는 다음과 같습니다.

1. Pass 디렉터리의 각 파일에 대 한 SHA1 해시를 계산 합니다. 또는 `manifest.json` `signature` 파일은 포함 하지 마세요 .이 단계에서는 존재 하지 않습니다.
1. 해시 `manifest.json` 를 사용 하 여 각 파일 이름의 JSON 키/값 목록으로 작성 합니다.
1. 인증서를 사용 하 여 `manifest.json` 파일에 서명 하 고 결과를 라는 `signature` 파일에 기록 합니다.
1. 모든 파일을 압축 하 고 결과 파일에 파일 `.pkpass` 확장명을 제공 합니다.

Pass에 서명 하는 데 개인 키가 필요 하므로이 프로세스는 사용자가 제어 하는 보안 서버 에서만 수행 해야 합니다. 응용 프로그램에서 패스를 시도 하 고 생성 하기 위해 키를 배포 하지 마십시오.

## <a name="configuration-and-setup"></a>구성 및 설정

이 섹션에서는 프로 비전 세부 정보를 설정 하 고 첫 번째 단계를 만드는 데 도움이 되는 지침을 제공 합니다.

### <a name="provisioning-passkit"></a>PassKit 프로 비전

패스를 앱 스토어에 입력 하려면 개발자 계정에 연결 해야 합니다. 이렇게 하려면 다음 두 단계가 필요 합니다.

1. Pass는 패스 형식 ID 라고 하는 고유 식별자를 사용 하 여 등록 해야 합니다.
1. 개발자의 디지털 서명으로 pass에 서명 하려면 유효한 인증서를 생성 해야 합니다.

패스 유형 ID를 만들려면 다음을 수행 합니다.

#### <a name="create-a-pass-type-id"></a>패스 유형 ID 만들기

첫 번째 단계는 지원 되는 각각의 서로 다른 _유형의_ 패스에 대해 패스 유형 ID를 설정 하는 것입니다. 패스 ID (또는 패스 유형 식별자)는 패스에 대 한 고유 식별자를 만듭니다. 이 ID를 사용 하 여 인증서를 사용 하 여 pass를 개발자 계정과 연결 합니다.

1. [IOS 프로 비전 포털의 인증서, 식별자 및 프로필 섹션](https://developer.apple.com/account/overview.action)에서 **식별자** 로 이동 하 여 **Pass 유형 id** 를 선택 합니다. 그런 다음 단추 **+** 를 선택 하 여 새 패스 유형을 만듭니다. [![](passkit-images/passid.png "새 패스 유형 만들기")](passkit-images/passid.png#lightbox)

2. 패스에 대 한 **설명** (이름) 및 **식별자** (고유 문자열)를 제공 합니다. 모든 패스 형식 id는이 예제에서 다음을 사용 `pass.` `pass.com.xamarin.coupon.banana` 하는 문자열로 시작 해야 합니다. [![](passkit-images/register.png "설명 및 식별자를 제공 합니다.")](passkit-images/register.png#lightbox)

3. **등록** 단추를 눌러 패스 ID를 확인 합니다.

#### <a name="generate-a-certificate"></a>인증서 생성

이 패스 유형 ID에 대 한 새 인증서를 만들려면 다음을 수행 합니다.

1. 목록에서 새로 만든 패스 ID를 선택 하 고 **편집** 을 클릭 합니다. [![](passkit-images/pass-done.png "목록에서 새 패스 ID를 선택 합니다.")](passkit-images/pass-done.png#lightbox)

    그런 다음 **인증서 만들기** ...를 선택 합니다. :

    [![](passkit-images/cert-dist.png "인증서 만들기 선택")](passkit-images/cert-dist.png#lightbox)

2. 단계에 따라 CSR (인증서 서명 요청)을 만듭니다.
  
3. 개발자 포털에서 **계속** 단추를 누르고 CSR을 업로드 하 여 인증서를 생성 합니다.

4. 인증서를 다운로드 하 고 키를 두 번 클릭 하 여 키 집합에 설치 합니다.

이제이 패스 유형 ID에 대 한 인증서를 만들었으므로 다음 섹션에서는 패스를 수동으로 빌드하는 방법을 설명 합니다.

전자 지갑 프로 비전에 대 한 자세한 내용은 [기능 사용](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) 가이드를 참조 하세요.

### <a name="create-a-pass-manually"></a>수동으로 패스 만들기

이제 Pass 형식을 만들었으므로 시뮬레이터 또는 장치에서 테스트 하는 패스를 수동으로 만들 수 있습니다. 패스를 만드는 단계는 다음과 같습니다.

- 패스 파일을 포함할 디렉터리를 만듭니다.
- 필요한 모든 데이터가 포함 된 pass 파일을 만듭니다.
- 필요한 경우 폴더에 이미지를 포함 합니다.
- 폴더의 모든 파일에 대 한 SHA1 해시를 계산 하 고,. n a s. json에 씁니다.
- 다운로드 한 인증서. p12 파일을 사용 하 여 매니페스트를 서명 합니다.
- 디렉터리의 내용을 압축 하 고. pkpass 확장으로 이름을 바꿉니다.

이 문서의 [샘플 코드](https://docs.microsoft.com/samples/xamarin/ios-samples/passkit) 에는 패스를 생성 하는 데 사용할 수 있는 몇 가지 소스 파일이 있습니다. Createapassmanually 디렉터리 `CouponBanana.raw` 의 디렉터리에 있는 파일을 사용 합니다. 다음 파일이 표시 됩니다.

 [![](passkit-images/image18.png "이러한 파일이 있음")](passkit-images/image18.png#lightbox)

Pass를 열고 JSON을 편집 합니다. Apple Developer 계정에 일치 하려면 `passTypeIdentifier` 적어도 `teamIdentifer` 및를 업데이트 해야 합니다.

```csharp
"passTypeIdentifier" : "pass.com.xamarin.coupon.banana",
"teamIdentifier" : "?????????",
```

그런 다음 각 파일에 대 한 해시를 계산 하 여 `manifest.json` 파일을 만들어야 합니다. 완료 되 면 다음과 같이 표시 됩니다.

```csharp
{
  "icon@2x.png" : "30806547dcc6ee084a90210e2dc042d5d7d92a41",
  "icon.png" : "87e9ffb203beb2cce5de76113f8e9503aeab6ecc",
  "pass.json" : "c83cd1441c17ecc6c5911bae530d54500f57d9eb",
  "logo.png" : "b3cd8a488b0674ef4e7d941d5edbb4b5b0e6823f",
  "logo@2x.png" : "3ccd214765507f9eab7244acc54cc4ac733baf87"
}
```

그런 다음이 패스 형식 ID에 대해 생성 된 인증서 (.p12 파일)를 사용 하 여이 파일에 대 한 서명을 생성 해야 합니다.

#### <a name="signing-on-a-mac"></a>Mac에서 서명

[Apple 다운로드](https://developer.apple.com/downloads/index.action?name=Passbook) 사이트에서 전자 **지갑 초기값 지원 자료** 를 다운로드 하세요. `signpass` 도구를 사용 하 여 폴더를 pass로 전환 합니다 .이는 SHA1 해시를 계산 하 고 출력을 pkpass 파일에 압축 합니다.

#### <a name="testing"></a>테스트

이러한 도구의 출력을 검토 하 고 (파일 이름을 .zip으로 설정 하 여 여는 `manifest.json` 경우) 다음 파일이 표시 됩니다 (및 `signature` 파일 추가).

 [![](passkit-images/image19.png "이러한 도구의 출력 검사")](passkit-images/image19.png#lightbox)

서명 된 후 파일의 이름을 바꾸고 (예: to `BananaCoupon.pkpass`)를 시뮬레이터로 끌어서 테스트 하거나 실제 장치에서 검색할 수 있도록 자신에 게 전자 메일로 보낼 수 있습니다. 다음과 같이 pass를 **추가** 하는 화면이 표시 됩니다.

 [![](passkit-images/image20.png "Pass 화면 추가")](passkit-images/image20.png#lightbox)

일반적으로이 프로세스는 서버에서 자동으로 수행 되지만, 수동 패스 생성은 백 엔드 서버를 지원 하지 않는 쿠폰만 생성 하는 소규모 기업을 위한 옵션 일 수 있습니다.

## <a name="wallet"></a>Wallet

PassKit 에코 시스템의 핵심이 되는 작은 부분입니다. 이 스크린샷에서는 빈 전자 지갑 및 패스 목록과 개별 패스가 어떻게 표시 되는지 보여 줍니다.

 [![](passkit-images/image21.png "이 스크린샷에서는 빈 전자 지갑 및 패스 목록과 개별 패스가 어떻게 표시 되는지 보여 줍니다.")](passkit-images/image21.png#lightbox)

작은 기능에는 다음이 포함 됩니다.

- 이 유일한 장소는 검색을 위해 바코드를 사용 하 여 렌더링 됩니다.
- 사용자가 업데이트에 대 한 설정을 변경할 수 있습니다. 사용 하도록 설정 하면 푸시 알림이 Pass의 데이터에 대 한 업데이트를 트리거할 수 있습니다.
- 사용자가 잠금 화면 통합을 사용 하거나 사용 하지 않도록 설정할 수 있습니다. 사용 하도록 설정 하면 패스에 포함 된 관련 시간 및 위치 데이터에 따라 패스가 잠금 화면에 자동으로 나타날 수 있습니다.
- 패스 JSON에서 웹 서버 URL이 제공 된 경우 패스의 반대 쪽에서 끌어오기-새로 고침을 지원 합니다.
- 앱 ID가 pass JSON에 제공 된 경우에는 도우미 앱을 열거나 다운로드할 수 있습니다.
- 귀여운 단편화 animation을 사용 하 여 패스를 삭제할 수 있습니다.

## <a name="adding-passes-into-wallet"></a>작은에 게 패스 추가

패스는 다음과 같은 방법으로 작은에 추가할 수 있습니다.

- **앱** 을 직접 조작 하지 않는 앱-패스 파일을 로드 하 고 사용자에 게 작은 사용자에 게 추가 하는 옵션을 제공 합니다. 

- **동반 앱** -공급자가 전달 된 앱을 배포 하 고 탐색 하거나 편집 하기 위한 추가 기능을 제공 합니다. Xamarin.ios 응용 프로그램은 PassKit API에 대 한 완전 한 액세스 권한을 가지 며 패스를 만들고 조작 합니다. 그런 다음를 `PKAddPassesViewController`사용 하 여 작은에 게 패스를 추가할 수 있습니다. 이 프로세스는이 문서의 **자매 응용 프로그램** 섹션에서 자세히 설명 합니다.

### <a name="conduit-applications"></a>응용 프로그램 통로

컨 트 응용 프로그램은 사용자를 대신 하 여 패스를 받을 수 있는 중간 앱 이며, 해당 콘텐츠 형식을 인식 하 고 전자 지갑에 추가할 기능을 제공 하도록 프로그래밍 해야 합니다. 응용 프로그램의 예는 다음과 같습니다.

- **Mail** – 첨부 파일을 패스로 인식 합니다.
- **Safari** – 패스 URL 링크를 클릭 하면 pass Content-type을 인식 합니다.
- **기타 사용자 지정 앱** -첨부 파일 또는 오픈 링크 (소셜 미디어 클라이언트, 메일 읽기 권한자 등)를 수신 하는 앱입니다.

이 스크린샷은 iOS 6의 메일이 메시지 첨부 파일을 인식 하 고 (작업 시) **전자 메일을 통해 전자 메일을 전자 메일** 에 **추가** 하는 방법을 보여 줍니다.

 [![](passkit-images/image22.png "이 스크린샷은 iOS 6의 메일이 pass 첨부 파일을 인식 하는 방법을 보여 줍니다.")](passkit-images/image22.png#lightbox)

 [![](passkit-images/image23.png "이 스크린샷은 메일에서 전자 메일을 제공 하 여 전자 메일에 전달 하는 방법을 보여 줍니다.")](passkit-images/image23.png#lightbox)

패스에 대 한 통로가 될 수 있는 앱을 빌드하는 경우 다음에서 인식할 수 있습니다.

- **파일 확장명** -pkpass
- **MIME 형식** -application/vnd. apple pkpass
- **UTI** – .com. pkpass

통로 응용 프로그램의 기본 작업은 pass 파일을 검색 하 고 PassKit `PKAddPassesViewController` 를 호출 하 여 사용자에 게 해당 사용자에 게 pass를 추가할 수 있는 옵션을 제공 하는 것입니다. 이 뷰 컨트롤러의 구현은 다음에 나오는 **응용 프로그램**의 섹션에서 설명 합니다.

보조 응용 프로그램에서 수행 하는 것과 동일한 방식으로 특정 패스 유형 ID에 대해 통로 응용 프로그램을 프로 비전 할 필요가 없습니다.

## <a name="companion-applications"></a>도우미 응용 프로그램

동반 응용 프로그램은 Pass 만들기, Pass와 관련 된 정보 업데이트 및 기타 응용 프로그램과 관련 된 pass 관리를 포함 하 여 패스 작업을 위한 추가 기능을 제공 합니다.

도우미 응용 프로그램은 작은 기능을 복제 하려고 해서는 안 됩니다. 검색에 대 한 패스를 표시 하기 위한 것은 아닙니다.

이 섹션의 나머지 부분에서는 PassKit와 상호 작용 하는 기본 자매 앱을 빌드하는 방법을 설명 합니다.

### <a name="provisioning"></a>프로비전

전자 지갑은 매장 기술 이므로 응용 프로그램을 별도로 프로 비전 해야 하며 팀 프로 비전 프로필 또는 와일드 카드 앱 ID를 사용할 수 없습니다. [기능 사용](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) 가이드를 참조 하 여 전자 지갑 응용 프로그램에 대 한 고유한 앱 ID 및 프로 비전 프로필을 만듭니다.

### <a name="entitlements"></a>권리가

**Info.plist** 파일은 모든 최신 xamarin.ios 프로젝트에 포함 되어야 합니다. 새 info.plist 파일을 추가 하려면 [자격 사용](~/ios/deploy-test/provisioning/entitlements.md) 가이드의 단계를 따르세요.

자격을 설정 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Solution Pad에서 **info.plist** 파일을 두 번 클릭 하 여 info.plist 편집기를 엽니다.

![](passkit-images/image31.png "자격 편집기")

전자 지갑 섹션에서 전자 **지갑 사용** 옵션을 선택 합니다.

![](passkit-images/image32.png "작은 자격 사용")

기본 옵션은 앱에서 모든 패스 유형을 허용 하는 것입니다. 그러나 앱을 제한 하 고 팀 패스 유형의 하위 집합만 허용할 수 있습니다. 이를 사용 하도록 설정 하려면 **팀 패스 유형의 하위 집합 허용** 을 선택 하 고 허용할 하위 집합의 패스 유형 식별자를 입력 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

**Info.plist** 파일을 두 번 클릭 하 여 XML 원본 파일을 엽니다.

작은 자격을 추가 하려면 드롭다운 목록에서 **속성** 을 `Passbook Identifiers` 로 설정 하 여 자동으로 **유형을** `Array`설정 합니다. 그런 다음 문자열 **값** 을로 `$(TeamIdentifierPrefix)*`설정 합니다.

![](passkit-images/image33.png "작은 자격 사용")

앱에서 모든 패스 유형을 허용할 수 있습니다. 앱을 제한 하 고 팀 패스 유형의 하위 집합만 허용 하려면 문자열 값을로 설정 합니다.

`$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)`

여기서 `pass.$(CFBundleIdentifier)` 는 [위에서](~/ios/platform/passkit.md) 만든 패스 ID입니다.

-----

### <a name="debugging"></a>디버깅

응용 프로그램을 배포 하는 데 문제가 있는 경우 올바른 **프로 비전 프로필** 을 사용 하 고 `Entitlements.plist` 있는지, **iPhone 번들 서명** 옵션에서 **사용자 지정 자격** 파일로 선택 되어 있는지 확인 합니다.

배포할 때이 오류가 발생 하는 경우:

```csharp
Installation failed: Your code signing/provisioning profiles are not correctly configured (error: 0xe8008016)
```

그런 다음 자격 배열이 잘못 되었거나 **프로 비전 프로필과**일치 하지 않습니다. `pass-type-identifiers` 패스 유형 Id와 팀 ID가 올바른지 확인 합니다.

## <a name="classes"></a>클래스

앱에서 액세스 하는 데 사용할 수 있는 PassKit 클래스는 다음과 같습니다.

- **Pkpass** – 패스의 인스턴스입니다.
- **PKPassLibrary** – 장치에서 패스에 액세스할 수 있는 API를 제공 합니다.
- **PKAddPassesViewController** – 사용자가 전자 지갑에 저장할 수 있도록 pass를 표시 하는 데 사용 됩니다.
- **PKAddPassesViewControllerDelegate** – xamarin.ios 개발자

## <a name="example"></a>예제

이 문서에 대 한 [샘플 코드](https://docs.microsoft.com/samples/xamarin/ios-samples/passkit) 의 PassLibrary 프로젝트를 참조 하세요. 다음은 작은 공격자 응용 프로그램에 필요한 다음과 같은 일반적인 함수를 보여 줍니다.

### <a name="check-that-wallet-is-available"></a>전자 지갑 사용 가능 여부 확인

IPad에서는 전자 지갑를 사용할 수 없으므로 응용 프로그램에서 PassKit 기능에 액세스 하기 전에 확인 해야 합니다.

```csharp
if (PKPassLibrary.IsAvailable) {
    // create an instance and do stuff...
}
```

### <a name="creating-a-pass-library-instance"></a>패스 라이브러리 인스턴스 만들기

PassKit 라이브러리는 단일 항목이 아닌 응용 프로그램에서 PassKit API에 액세스 하는 및 인스턴스를 만들고 저장 해야 합니다.

```csharp
if (PKPassLibrary.IsAvailable) {
    library = new PKPassLibrary ();
    // do stuff...
}
```

### <a name="get-a-list-of-passes"></a>패스 목록 가져오기

응용 프로그램은 라이브러리에서 패스 목록을 요청할 수 있습니다. 이 목록은 PassKit에 의해 자동으로 필터링 되므로, 사용자의 자격에 나열 된 팀 ID로 생성 된 패스만 볼 수 있습니다.

```csharp
var passes = library.GetPasses ();  // returns PKPass[]
```

시뮬레이터는 반환 되는 패스 목록을 필터링 하지 않으므로이 메서드는 항상 실제 장치에서 테스트 되어야 합니다. 이 목록은 UITableView에 표시 될 수 있습니다. [샘플 앱](https://docs.microsoft.com/samples/xamarin/ios-samples/passkit) 은 두 개의 쿠폰이 추가 된 후 다음과 같이 표시 됩니다.

 [![](passkit-images/image29.png "두 개의 쿠폰이 추가 된 후 샘플 앱이 다음과 같이 보입니다.")](passkit-images/image29.png#lightbox)

### <a name="displaying-passes"></a>패스 표시

제한 된 정보 집합은 동반 앱 내에서 패스를 렌더링 하는 데 사용할 수 있습니다.

예제 코드와 같이 일련의이 표준 속성을 선택 하 여 패스 목록을 표시 합니다.

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

이 문자열은 [샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/passkit)에서 경고로 표시 됩니다.

 [![](passkit-images/image30.png "샘플에서 선택한 쿠폰 경고")](passkit-images/image30.png#lightbox)

또한 메서드를 `LocalizedValueForFieldKey()` 사용 하 여 사용자가 디자인 한 패스의 필드에서 데이터를 검색할 수 있습니다 (어떤 필드가 표시 되어야 함). 예제 코드는이를 표시 하지 않습니다.

### <a name="loading-a-pass-from-a-file"></a>파일에서 패스 로드

사용자의 권한이 있는 전자 지갑에는 한 번만 추가 될 수 있으므로 사용자가 결정할 수 있도록 보기 컨트롤러가 표시 되어야 합니다. 이 코드는 예제에서 **추가** 단추를 사용 하 여 앱에 포함 된 미리 작성 된 패스를 로드 합니다 .이 코드는 사용자가 서명한 것으로 바꾸어야 합니다.

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

이 패스는 **추가** 및 **취소** 옵션과 함께 제공 됩니다.

 [![](passkit-images/image20.png "추가 및 취소 옵션과 함께 제공 되는 패스")](passkit-images/image20.png#lightbox)

### <a name="replace-an-existing-pass"></a>기존 패스 바꾸기

기존 패스를 교체 하는 경우 사용자의 권한이 필요 하지 않지만 패스가 아직 없는 경우 실패 합니다.

```csharp
if (library.Contains (newPass)) {
     library.Replace (newPass);
}
```

### <a name="editing-a-pass"></a>패스 편집

PKPass는 변경할 수 없으므로 코드에서 패스 개체를 업데이트할 수 없습니다. Pass의 데이터를 변경 하려면 응용 프로그램에서 통과 기록을 유지 하 고 응용 프로그램에서 다운로드할 수 있는 업데이트 된 값을 사용 하 여 새 패스 파일을 생성할 수 있는 웹 서버에 대 한 액세스 권한이 있어야 합니다.

통과는 개인 및 보안을 유지 해야 하는 인증서로 서명 해야 하므로 서버에서 패스 파일 만들기를 수행 해야 합니다.

업데이트 된 패스 파일이 생성 되 면 메서드를 `Replace` 사용 하 여 장치의 이전 데이터를 덮어씁니다.

### <a name="display-a-pass-for-scanning"></a>검색에 대 한 Pass 표시

앞에서 설명한 것 처럼, 작은 숫자만 검색에 대 한 통과를 표시할 수 있습니다. 다음과 같이 메서드를 `OpenUrl` 사용 하 여 패스를 표시할 수 있습니다.

 `UIApplication.SharedApplication.OpenUrl (p.PassUrl);`

### <a name="receiving-notifications-of-changes"></a>변경 알림 받기

응용 프로그램은를 `PKPassLibraryDidChangeNotification`사용 하 여 패스 라이브러리에 적용 되는 변경 내용을 수신할 수 있습니다. 백그라운드에서 업데이트를 트리거하는 알림으로 인해 변경 될 수 있으므로 앱에서 수신 대기 하는 것이 좋습니다.

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

PKPassLibrary가 singleton이 아니기 때문에 알림에 등록할 때 라이브러리 인스턴스를 전달 하는 것이 중요 합니다.

## <a name="server-processing"></a>서버 처리

PassKit을 지원 하기 위해 서버 응용 프로그램을 빌드하는 방법에 대 한 자세한 내용은이 소개 문서의 범위를 벗어나는 것입니다.

[Dotnet passbook](https://github.com/tomasmcguinness/dotnet-passbook) 오픈 소스 C# 서버 쪽 코드를 참조 하세요.

## <a name="push-notifications"></a>푸시 알림

업데이트 전달에 푸시 알림을 사용 하는 방법에 대 한 자세한 내용은이 소개 문서의 범위를 벗어나는 것입니다.

업데이트가 필요한 경우에는 전자 지갑에서 웹 요청에 응답 하기 위해 Apple에서 정의한 REST와 유사한 API를 구현 해야 합니다.

자세한 내용은 Apple의 [Pass 업데이트](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/PassKit_PG/Updating.html#//apple_ref/doc/uid/TP40012195-CH5-SW1) 가이드를 참조 하세요.

## <a name="summary"></a>요약

이 문서에서는 PassKit를 소개 했으며 전체 PassKit 솔루션에 대해 구현 해야 하는 다양 한 부분을 설명 하 고 설명 하는 몇 가지 이유를 설명 했습니다. 통과를 만들도록 Apple 개발자 계정을 구성 하는 데 필요한 단계, 수동으로 패스를 만드는 프로세스 및 Xamarin.ios 응용 프로그램에서 PassKit Api에 액세스 하는 방법을 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [개발자 용 전자 지갑](https://developer.apple.com/wallet/)
- [PassKit 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/passkit)
- [전자 지갑 개발자 가이드](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/PassKit_PG/index.html#//apple_ref/doc/uid/TP40012195-CH1-SW1)
- [프레임 워크 – Apple Pay 및 작은 (WWDC 비디오)](https://developer.apple.com/videos/frameworks/apple-pay-and-wallet)
- [PassKit Framework 참조](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [Passbook 웹 서비스 참조](https://developer.apple.com/library/prerelease/ios/#documentation/PassKit/Reference/PassKit_WebService/WebService.html)
- [패스 파일 정보](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html)
- [passbook](https://github.com/tomasmcguinness/dotnet-passbook), iOS 작은 패키지를 생성 하기 위한 오픈 소스 라이브러리
- [iOS 6 소개](~/ios/platform/introduction-to-ios6/index.md)
