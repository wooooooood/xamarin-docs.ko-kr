---
title: "앱 스토어 변경 내용"
description: "IOS 11의 새로운 기능 탐색"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A7A03FD-B4F2-4969-8676-A17260730FD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 78528c750c6350d113b34a07d166a03773119a8b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="app-store-changes"></a>앱 스토어 변경 내용

_IOS 11의 새로운 기능 탐색_

IOS 앱 스토어에 뿐만 아니라 사용자가 저장소를 효율적으로 탐색할 수 있지만 수도 있습니다를 사용자에 게 앱을 승격 하려면 개발자가 완전히 다시 디자인이 되어 있습니다. 이러한 승격 앱에서 바로 구매에 대 한 업데이트 및 제품 페이지에 대 한 업데이트를 포함 합니다. 또한 11 iOS 사용자와 통신 하는 방법, 응용 프로그램 아이콘을 추가 하는 방법 및 공개적으로 응용 프로그램을 해제 하는 방법에 대 한 업데이트를 추가 합니다.

![새 응용 프로그램 저장소 레이아웃](app-store-changes-images/image3.jpg)

다시 디자인 된 응용 프로그램 저장소에는 다음 섹션에 있습니다.

- **오늘** -이 탭에는 "편집기의 선택" 하거나 응용 프로그램을 갖춘 앱을 포함 합니다. 승격 하려면이 응용 프로그램 정보를 입력에 [승격](https://developer.apple.com//contact/app-store/promote/) 페이지.
- **게임** –로 설정 하는 모든 앱은 **게임** iTunes Connect에서에서 범주는이 탭에서 확인할 수 있습니다.
- **앱** -이 탭에는 다른 모든 앱 기능입니다. 추천된 앱, 범주, 상위 유료/무료 사용자를 찾을 수 있습니다.
- **업데이트** –이 앱에는 앱에 업데이트가 표시 됩니다. 통해 앱을 해제 하는 경우에 [릴리스 별](#Phased_Release), 사용자 수 여전히이 탭으로 이동 하 고 최신 버전을 다운로드 합니다.
- **검색** -이 탭에서는 사용자가 앱에 대 한 검색할 수 있습니다.

## <a name="store-icon"></a>저장소 아이콘

스토어 아이콘 (또는 마케팅 아이콘) iTunes Connect에서에서 더 이상 관리 하 고 대신로 포함 되어야 합니다는 [자산 카탈로그](~/ios/app-fundamentals/images-icons/app-icons.md) 에 응용 프로그램 이진 파일, 응용 프로그램 아이콘 비슷합니다. PNG 형식에서 1024 x 1024 저장소 아이콘을 11 iOS 앱의 성공적인 전송에는 자산 카탈로그에 포함 되어야 합니다.

응용 프로그램 감소 하면이 추가 자산 카탈로그의 응용 프로그램 크기를 증가 하지 않습니다.


## <a name="in-app-purchases-promoted-in-the-app-store"></a>앱 스토어에서 승격 하는 앱에서 바로 구매

Apple 했습니다 앱에서 바로 구매를 더 쉽게 검색할 수 앱 스토어에 게시 합니다. 최대 20 개를 추가할 수 있습니다 _승격 하는 앱 스토어_ 이제 앱의 제품 페이지에서 또는 검색 하 여 응용 프로그램 페이지에서 찾을 수 있는 앱에서 바로 구매 합니다.

앱 스토어에 표시 하 여 앱에서 바로 구매 하려면 다음 데이터를 포함 해야 합니다.

- **이미지** – 앱에서 바로 구매 용도 설명 하는 아이콘에 대 한 특별히 설계 된 이미지를 제공 해야 합니다. 이 이미지 앱 아이콘와에서 달라 야 하 고 스크린샷을 일 수 없습니다.
- **이름** -이름에 최대 30 자 까지만 사용할 수 있습니다.
- **설명** – 설명은 최대 45 자까지 입력할 수만 있습니다.

모든 앱에서 바로 구매 홍보 게시 하기 전에 앱 스토어 검토 영향을 받습니다.

승격을 사용할 수 있는 앱에서 바로 구매를 하려면 앱 열고 **기능 > 앱에서 바로 구매**합니다. 이동 하는 **앱 스토어 프로 모션 (선택 사항)** 섹션 및 1024 x 1024 이미지를 추가 하 고 **저장**다음 그림에 표시 된 것 처럼,:

![앱 스토어 프로 모션 섹션인 iTune 연결](app-store-changes-images/image4.png)

추가 해야는 `ShouldAddStorePayment` 메서드는 `SKPaymentTransactionObserver` 응용 프로그램에서 프로토콜입니다.

앱에서 바로 구매 승격에 대 한 자세한 내용은 Apple의을 참조 하십시오. [Your 앱에서 바로 구매 승격](https://developer.apple.com/app-store/promoting-in-app-purchases/) 페이지.

## <a name="redesigned-product-page"></a>다시 디자인 된 제품 페이지

제품 페이지에 다음 변경 내용은 적용 했습니다.

- 이제 제목 최대 30 자일으로 설정 됩니다.
- 추가 된 부제목
    - 제목을 보완 하는 간단한 요약을 이어야 합니다.
    - 부제목 최대 30 자일 있어야 합니다.
- 응용 프로그램 미리 보기
    - 이제 세 개의 비디오 또는 스크린샷을 사용할 수 있습니다.
    - 비디오 자동 실행 사용자가 페이지를 방문 합니다.
    - 자세한 내용은 Apple의을 참조 하십시오. [앱 미리 보기](https://developer.apple.com/app-store/app-previews/) 페이지.
- 프로 모션 텍스트
    - 이 새로운 기능 170 자의 응용 프로그램에 대 한 자주 변경 되는 정보를 설명 하기 위해 사용할 수 있도록 하는 텍스트를 제공 합니다.
    - 새 버전의 앱을 제출 하지 않고도 언제 든 지 업데이트할 수 있습니다.

## <a name="customer-communication"></a>고객 통신

10.3, 사과 검토에 응답 하는 기능을 통해 응용 프로그램 사용자와 직접 통신 하는 개발자를 위한 새로운 방법을 시작 합니다. ITunes에서이 새 기능에 액세스할 수 있습니다로 이동 하 여 연결 **앱 > 활동 > 등급 및 검토**다음 그림에 표시 된 것 처럼,:

![회신 한 의견을 입력 하는 영역을 보여 주는 대화 상자](app-store-changes-images/image5.png)

몇 가지 방법으로 사용자에 게 회신할 때 알고 있어야 합니다.

- 한 번에 회신할 수 있습니다. 있지만 양 당사자 원하는 만큼 주석을 편집할 수 있습니다.
- 사용자 메모에 응답 하는 경우 알림을 받습니다.
- 역할 만들기 완료 iTunes에서 새로운 고객 지원에 연결 합니다. 이 역할 또는 관리자 역할이 있는 사용자 의견에 응답할 수 있습니다.

자세한 내용은 Apple의을 참조 하십시오. [검토에 응답](https://developer.apple.com/app-store/responding-to-reviews/) 페이지.

<a name="Phased_Release"/>

## <a name="phased-release"></a>단계적된 출시

11 iOS와 Apple 응용 프로그램 업데이트에 대 한 단계별된 릴리스 옵션을 구현 했습니다. 확인 요청에서 프로덕션 환경의 재정의 하지 않은 고객에 게 점진적으로 릴리스될 응용 프로그램 업데이트를 허용 하도록 단계적된 릴리스를 사용할 수 있습니다.

단계적된 릴리스 iTunes Connect에서에서 활성화 됩니다. 사이드바에서 응용 프로그램에서 클릭으로 스크롤한는 **자동 업데이트에 대 한 릴리스 별** 섹션 아래에서 선택한 **7 일 기간 사용을 통해 릴리스 업데이트 릴리스 별**:

![옵션 표시 된 자동 업데이트에 대 한 릴리스를 더 이상 사용](app-store-changes-images/image6.png)

앱 스토어의 업데이트 탭에는 다운로드에 대해 즉시 업데이트 ´ ù. 단계적된 릴리스 선택 자동 다운로드를 설치한 사용자 수만 있습니다.


## <a name="related-links"></a>관련 링크

- [IOS (Apple) 11에서 새로운 이란](https://developer.apple.com/ios/)
- [업데이트 된 앱 스토어 제품 페이지 (Apple)](https://developer.apple.com/app-store/product-page/)
- [IOS 11 (WWDC) (비디오)에 대 한 응용 프로그램 업데이트](https://developer.apple.com/videos/play/wwdc2017/204/)
