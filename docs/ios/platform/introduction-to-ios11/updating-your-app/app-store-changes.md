---
title: IOS 11에서에서 app Store 변경
description: 이 문서에서는 iOS 11에서에서 앱 스토어에 변경 내용을 살펴봅니다. 응용 프로그램의 스토어 아이콘, 승격 된 앱 내 구매, 새로 디자인 된 제품 페이지, 고객 통신 및 단계별된 릴리스 설명합니다.
ms.prod: xamarin
ms.assetid: 4A7A03FD-B4F2-4969-8676-A17260730FD6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: 022d6b5c3f85863352dd1343752e934240b357aa
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61229657"
---
# <a name="app-store-changes-in-ios-11"></a>IOS 11에서에서 app Store 변경

IOS 앱 스토어 뿐 아니라 저장소를 효율적으로 탐색할 수 있습니다 뿐만 아니라, 사용자에 게 앱을 승격 하려면 개발자는 완전히 다시 디자인을 했습니다. 이러한 프로 모션에는 앱 내 구매에 대 한 업데이트 및 제품 페이지에 대 한 업데이트 포함 됩니다. iOS 11는 또한 사용자와 통신 하는 방법, 앱 아이콘에 추가 하는 방법 및 공개적으로 앱을 해제 하는 방법에 대 한 업데이트를 추가 합니다.

![새 앱 저장소 레이아웃](app-store-changes-images/image3.jpg)

새로 디자인 된 앱 스토어에는 다음 섹션에 있습니다.

- **오늘** -이 탭에 "편집기의 선택"은 앱을 추천 하는 앱에 포함 됩니다. 승격 하려면 다음이 응용 프로그램 정보를 입력에 [승격](https://developer.apple.com//contact/app-store/promote/) 페이지입니다.
- **게임** –로 설정 된 모든 앱은 **게임** iTunes Connect에서에서 범주를이 탭에서 찾을 수 있습니다.
- **앱** -이 탭에는 다른 모든 앱 기능입니다. 사용자 추천된 앱, 범주, 상위 유료/free 찾아볼 수 있습니다.
- **업데이트** –이 앱이 앱에 업데이트를 표시 합니다. 통해 앱을 릴리스 하는 경우에 [릴리스 단계적](#Phased_Release), 사용자가 여전히이 탭으로 이동한 다음를 최신 버전을 다운로드 합니다.
- **검색** -이 탭에는 앱에 대 한 검색할 수 있습니다.

## <a name="store-icon"></a>스토어 아이콘

스토어 아이콘 (또는 마케팅 아이콘) 더 이상 iTunes Connect에서에서 관리 되 고 대신로 포함 되어야 합니다는 [자산 카탈로그](~/ios/app-fundamentals/images-icons/app-icons.md) 에 앱, 앱 아이콘 비슷합니다. PNG 형식으로 1024x1024 스토어 아이콘 제출이 iOS 11 앱에 대 한 자산 카탈로그에 포함 되어야 합니다.

앱 감소 하면이 추가 자산 카탈로그 앱 크기를 늘어나지 않습니다.


## <a name="in-app-purchases-promoted-in-the-app-store"></a>앱 스토어에서 승격 하는 앱 내 구매

Apple에 대 한 앱 내 구매 더 쉽게 검색할 수 앱 스토어에서. 이제 최대 20 개를 추가할 수 있습니다 _승격 하는 앱 스토어_ 이제 앱의 제품 페이지에서 또는 검색 하 여 앱 페이지에서 찾을 수 있는 앱 내 구매 합니다.

앱 스토어에 표시 된 앱 내 구매 하도록 다음 데이터를 포함 해야 합니다.

- **이미지** – 인 앱 구매의 용도 설명 하는 아이콘에 대 한 특별히 설계 된 이미지를 제공 해야 합니다. 이 이미지는 앱 아이콘에서 다른 이어야 하며 스크린 샷 일 수 없습니다.
- **이름** – 이름에는 최대 30 자 까지만 사용할 수 있습니다.
- **설명** – 설명은 45 자까지 입력할 수만 있습니다.

모든 앱에서 바로 구매 홍보 게시 하기 전에 앱 스토어 검토를 적용 됩니다.

앱 내 구매 승격에 사용할 수 있도록 앱을 열고 이동할 **기능 > 앱에서 바로 구매**합니다. 로 이동 합니다 **앱 스토어 프로 모션 (선택 사항)** 섹션 및 1024x1024 이미지를 추가 하 고 **저장**와 다음 이미지와 같이:

![ITune Connect에서에서 앱 스토어 프로 모션 섹션](app-store-changes-images/image4.png)

추가 해야 합니다 `ShouldAddStorePayment` 메서드를는 `SKPaymentTransactionObserver` 앱의 프로토콜입니다.

인 앱 구매 승격에 대 한 자세한 내용은 Apple의을 참조 하세요 [Your 앱에서 바로 구매 승격](https://developer.apple.com/app-store/promoting-in-app-purchases/) 페이지입니다.

## <a name="redesigned-product-page"></a>새로 디자인 된 제품 페이지

제품 페이지에 다음 변경 내용은 적용 했습니다.

- 이제 설정 된 최대 30 자
- 추가한 부제목
    - 제목을 보완 하는 간단한 요약을 해야 합니다.
    - 부제목을 최대 30 자 있어야 합니다.
- 앱 미리 보기
    - 이제 세 가지 비디오 또는 스크린 샷을 할 수 있습니다.
    - 비디오 재생 페이지를 방문 하는 사용자입니다.
    - 자세한 내용은 Apple의을 참조 하세요 [앱 미리 보기](https://developer.apple.com/app-store/app-previews/) 페이지입니다.
- 프로 모션 텍스트
    - 이 새로운 기능 또는 170 자 앱에 대 한 자주 변경 되는 정보를 설명 하는 데 허용 하는 텍스트를 제공 합니다.
    - 새 버전의 앱을 제출 하지 않고도 언제 든 지 업데이트할 수 있습니다.

## <a name="customer-communication"></a>고객 통신

10.3, Apple 검토에 응답 하는 기능을 통해 앱 사용자와 직접 통신 하는 개발자를 위한 새로운 방식으로 시작 합니다. ITunes에서이 새 기능에 액세스할 수 있습니다로 이동 하 여 연결 **앱 > 활동 > 등급 및 리뷰**와 다음 이미지와 같이:

![주석에 회신을 입력 하는 영역을 보여 주는 대화 상자](app-store-changes-images/image5.png)

몇 가지 방법으로 사용자에 게 회신할 때 알아야 할 수 있습니다.

- 한 번에 응답할 수 있지만 양쪽 원하는 만큼 주석을 편집할 수 있습니다.
- 사용자 의견에 응답 하는 경우 알림을 받습니다.
- ITunes에서 역할을 만들었습니다. 새 고객 지원에 연결 합니다. 이 역할 또는 관리자 역할을 사용 하 여 사용자 의견에 응답할 수 있습니다.

자세한 내용은 Apple의을 참조 하세요 [검토에 응답](https://developer.apple.com/app-store/responding-to-reviews/) 페이지입니다.

<a name="Phased_Release"/>

## <a name="phased-release"></a>단계별된 릴리스

IOS 11 사용 하 여 Apple에 앱 업데이트에 대 한 단계별 릴리스 옵션을 구현 했습니다. 앱 업데이트 요청이 프로덕션 환경의 오버 로드 하지 않으므로 보장 하는 고객에 게 점진적으로 출시 될 수 있도록 단계별된 릴리스를 사용할 수 있습니다.

단계별된 릴리스는 iTunes Connect에서에서 사용할 수 있습니다. 세로 막대에서 앱의 클릭으로 스크롤한 합니다 **자동 업데이트에 대 한 릴리스 단계적** 선택한 아래쪽의 섹션 **릴리스 업데이트 7 일 간의 기간을 사용 하는 동안 단계적 릴리스**:

![옵션 표시 단계적 자동 업데이트에 대 한 릴리스](app-store-changes-images/image6.png)

업데이트는 다운로드 앱 스토어의 [업데이트] 탭에서 즉시 사용할 수 있습니다. 단계별된 릴리스 에서만 자동 다운로드를 선택 하는 사용자에 대해 제공 됩니다.


## <a name="related-links"></a>관련 링크

- [새로운 iOS 11 (Apple)의 기능](https://developer.apple.com/ios/)
- [업데이트 된 앱 스토어 제품 페이지 (Apple)](https://developer.apple.com/app-store/product-page/)
- [IOS 11 (WWDC) (비디오)에 대 한 앱을 업데이트 하는 중입니다.](https://developer.apple.com/videos/play/wwdc2017/204/)
