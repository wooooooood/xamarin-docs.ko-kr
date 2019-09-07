---
title: IOS 11의 앱 스토어 변경 내용
description: 이 문서에서는 iOS 11에서 앱 스토어에 대 한 변경 내용을 살펴봅니다. 응용 프로그램의 저장소 아이콘, 앱 내 구매, 다시 디자인 된 제품 페이지, 고객 통신 및 단계적 릴리스를 설명 합니다.
ms.prod: xamarin
ms.assetid: 4A7A03FD-B4F2-4969-8676-A17260730FD6
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/13/2016
ms.openlocfilehash: 0ac9b486defb74cac7ccd946d2b35b283e6aeca5
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70752323"
---
# <a name="app-store-changes-in-ios-11"></a>IOS 11의 앱 스토어 변경 내용

IOS 앱 스토어는 사용자가 저장소를 효율적으로 탐색할 수 있을 뿐 아니라 개발자가 앱을 사용자에 게 승격할 수 있도록 하는 완전히 다시 설계 되었습니다. 이러한 판촉에는 앱 내 구매 및 제품 페이지에 대 한 업데이트에 대 한 업데이트가 포함 됩니다. 또한 iOS 11은 사용자와 통신 하는 방법, 앱 아이콘을 추가 하는 방법 및 앱을 공용으로 릴리스 하는 방법에 대 한 업데이트를 추가 합니다.

![새 앱 스토어 레이아웃](app-store-changes-images/image3.jpg)

새로 디자인 된 앱 스토어에는 다음과 같은 섹션이 있습니다.

- **오늘** -이 탭은 "편집기의 선택" 또는 추천 앱 인 앱을 포함 합니다. 앱을 승격 하려면 [수준 올리기](https://developer.apple.com//contact/app-store/promote/) 페이지에서 정보를 입력 합니다.
- **게임** -iTunes Connect의 **게임** 범주로 설정 된 모든 앱은이 탭 아래에서 찾을 수 있습니다.
- **앱** -이 탭은 다른 모든 앱을 기능 합니다. 사용자는 주요 앱, 범주, 최고 유료/무료로 찾아볼 수 있습니다.
- **업데이트** -이 앱은 앱에 대 한 업데이트를 표시 합니다. [단계별 릴리스](#Phased_Release)를 통해 앱을 출시 하더라도 사용자는이 탭으로 이동 하 여 최신 버전을 다운로드할 수 있습니다.
- **검색** -이 탭에서는 사용자가 앱을 검색할 수 있습니다.

## <a name="store-icon"></a>저장소 아이콘

저장소 아이콘 (또는 마케팅 아이콘)은 더 이상 iTunes Connect에서 관리 되지 않으며, 대신 앱 아이콘과 마찬가지로 앱 이진에 [자산 카탈로그로](~/ios/app-fundamentals/images-icons/app-icons.md) 포함 되어야 합니다. IOS 11 앱을 성공적으로 제출 하려면 PNG 형식의 1024 x 1024 저장소 아이콘이 자산 카탈로그에 포함 되어 있어야 합니다.

앱 thinning이 추가 asset catalog가 앱 크기를 늘리지 않도록 합니다.

## <a name="in-app-purchases-promoted-in-the-app-store"></a>앱 스토어에서 승격 된 앱 내 구매

Apple은 앱 스토어에서 앱 내 구매를 더 검색 가능 하 게 했습니다. 이제 앱 페이지, 앱의 제품 페이지 또는 검색을 통해 찾을 수 있는 앱 _스토어_ 를 최대 20 개까지 추가할 수 있습니다.

앱 스토어에 앱 내 구매를 표시 하려면 다음 데이터를 포함 해야 합니다.

- **이미지** – 앱에서 구매한 기능을 설명 하는 아이콘에 대해 특별히 디자인 된 이미지를 제공 해야 합니다. 이 이미지는 앱 아이콘과 달라 야 하며 스크린샷 일 수 없습니다.
- **이름** – 이름은 최대 30 자만 될 수 있습니다.
- **설명** – 설명은 최대 45 자 까지만 가능 합니다.

앱 내 구매 프로 모션은 게시 하기 전에 앱 스토어 검토의 적용을 받습니다.

앱 내 구매를 승격할 수 있도록 하려면 앱을 열고 **앱 내 구매 > 기능**으로 이동 합니다. 다음 그림에 나와 있는 것 처럼 **앱 스토어 프로 모션 (선택 사항)** 섹션으로 이동 하 고 1024 x 1024 이미지를 추가 하 고 **저장**합니다.

![Itunes Connect의 앱 스토어 프로 모션 섹션](app-store-changes-images/image4.png)

또한 응용 프로그램에서 `ShouldAddStorePayment` `SKPaymentTransactionObserver` 프로토콜에 메서드를 추가 해야 합니다.

앱 내 구매 승격에 대 한 자세한 내용은 Apple의 [앱에서 바로 구매 기능 수준 올리기](https://developer.apple.com/app-store/promoting-in-app-purchases/) 페이지를 참조 하세요.

## <a name="redesigned-product-page"></a>새로 디자인 한 제품 페이지

제품 페이지가 다음과 같이 변경 되었습니다.

- 이제 제목이 최대 30 자로 설정 됩니다.
- 부제목이 추가 되었습니다.
  - 제목에 대 한 간략 한 요약 이어야 합니다.
  - 부제목은 최대 30 자 여야 합니다.
- 앱 미리 보기
  - 이제 비디오 또는 스크린샷 3 개를 사용할 수 있습니다.
  - 사용자가 페이지를 방문 하면 비디오가 자동으로 실행 됩니다.
  - 자세한 내용은 Apple의 [앱 미리 보기](https://developer.apple.com/app-store/app-previews/) 페이지를 참조 하세요.
- 판촉 텍스트
  - 이 새로운 기능은 앱에 대 한 자주 변경 되는 정보를 설명 하는 데 사용할 수 있는 170 문자 텍스트를 제공 합니다.
  - 새 버전의 앱을 제출 하지 않고도 언제 든 지 업데이트할 수 있습니다.

## <a name="customer-communication"></a>고객 통신

10.3에서 Apple은 개발자가 리뷰에 응답할 수 있는 기능을 통해 앱 사용자와 직접 통신할 수 있는 새로운 방법을 시작 했습니다. 다음 이미지에 나와 있는 것 처럼 **앱 > 활동 > 등급 및 검토**를 탐색 하 여 iTunes connect에서이 새로운 기능에 액세스할 수 있습니다.

![주석에 회신을 입력할 영역을 표시 하는 대화 상자](app-store-changes-images/image5.png)

사용자에 게 회신할 때 알아야 할 몇 가지 사항이 있습니다.

- 한 번만 회신할 수 있지만 두 당사자 모두 원하는 만큼 주석을 편집할 수 있습니다.
- 사용자는 주석에 응답할 때 알림을 받습니다.
- ITunes connect에 새 고객 지원 역할이 만들어졌습니다. 이 역할 또는 관리자 역할이 있는 사용자는 주석에 응답할 수 있습니다.

자세한 내용은 Apple의 [검토에 대 한 응답](https://developer.apple.com/app-store/responding-to-reviews/) 페이지를 참조 하세요.

<a name="Phased_Release"/>

## <a name="phased-release"></a>단계적 릴리스

IOS 11을 사용 하 여 Apple은 앱의 업데이트에 대 한 단계적 릴리스 옵션을 구현 했습니다. 단계적 릴리스를 사용 하 여 앱 업데이트를 고객에 게 점진적으로 릴리스할 수 있도록 하 여 수요를 프로덕션 환경에서 오버 로드 하지 않도록 합니다.

단계적 릴리스는 iTunes Connect에서 사용 하도록 설정 됩니다. 사이드바에서 앱을 클릭 하 고 맨 아래에 있는 **자동 업데이트에 대 한 단계적 릴리스** 섹션으로 스크롤한 다음 **단계적 릴리스를 사용 하 여 7 일 동안 릴리스 업데이트**를 선택 합니다.

![자동 업데이트에 대 한 단계별 릴리스를 표시 하는 옵션](app-store-changes-images/image6.png)

앱 스토어의 업데이트 탭에서 즉시 업데이트를 다운로드할 수 있습니다. 단계적 릴리스는 자동 다운로드를 선택한 사용자 에게만 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [IOS 11의 새로운 기능 (Apple)](https://developer.apple.com/ios/)
- [업데이트 된 앱 스토어 제품 페이지 (Apple)](https://developer.apple.com/app-store/product-page/)
- [IOS 11 용 앱 업데이트 (WWDC) (비디오)](https://developer.apple.com/videos/play/wwdc2017/204/)
