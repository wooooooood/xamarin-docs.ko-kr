---
title: IOS 11에 앱을 업데이트 하는 중입니다.
description: 이 문서는 iOS 11의 릴리스를 사용 하 여 Xamarin.iOS 개발자에 게 사용할 수 있는 새 기능을 설명 하는 다양 한 가이드에 연결 합니다. 예를 들어, 시각적 디자인 업데이트를 App Store 변경 하 고 앱 아이콘 업데이트 합니다.
ms.prod: xamarin
ms.assetid: EC809504-9CF6-4949-B6EE-36384297E744
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: 3193f27c30080a4335abfe969acb3c8b33516469
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110780"
---
# <a name="updating-your-app-to-ios-11"></a>IOS 11에 앱을 업데이트 하는 중입니다.

Ios 11에서 Apple 아키텍처 업데이트, 새 visual 변경 내용 및 업데이트 된 iTunes Connect 프로세스를 도입 되었습니다. 이 가이드에서는 Xamarin.iOS 앱을 iOS 11에 대 한 업데이트를 활용할 수 있도록 돕는 이러한 변경 내용을 각 살펴봅니다.

## <a name="architecture-changesarchitecture-changesmd"></a>[아키텍처 변경 내용](architecture-changes.md)

IOS 11에 알고 있어야 하는 가장 큰 변화 중 하나는에 설명 된 대로 앱에 대 한 32 비트 지원의 중단이 [Apple](https://developer.apple.com/news/?id=06282017b) 언론 발표 합니다.

이 가이드는 64 비트 앱을 업데이트 하는 방법을 안내 합니다.

## <a name="visual-design-updatesvisual-designmd"></a>[비주얼 디자인 업데이트](visual-design.md)

Apple iOS 11의 경우 테이블 뷰, 검색 표시줄 및 탐색 모음에 대 한 업데이트를 포함 하 여 새 visual 변경 내용을 도입 했습니다. 또한 여백 및 전체 화면 콘텐츠를 통한 보다 유연 하 게 허용 개선이 이루어졌습니다. 이러한 변경 내용은이 가이드에 나와 있습니다.

## <a name="app-store-changesapp-store-changesmd"></a>[App Store 변경](app-store-changes.md)

IOS 앱 스토어 뿐 아니라 저장소를 효율적으로 탐색할 수 있습니다 뿐만 아니라, 사용자에 게 앱을 승격 하려면 개발자는 완전히 다시 디자인을 했습니다. 이러한 프로 모션에는 앱 내 구매에 대 한 업데이트 및 제품 페이지에 대 한 업데이트 포함 됩니다. iOS 11는 또한 사용자와 통신 하는 방법, 앱 아이콘에 추가 하는 방법 및 공개적으로 앱을 해제 하는 방법에 대 한 업데이트를 추가 합니다.

## <a name="app-icon-updates"></a>앱 아이콘 업데이트

> [!NOTE]
> 앱 아이콘으로 배달 되도록 이제는 _자산 카탈로그_합니다. 

자산 카탈로그를 사용 하는 방법은를 참조 합니다 [앱 스토어 아이콘](~/ios/app-fundamentals/images-icons/app-store-icon.md) 가이드입니다. 마이그레이션에 대 한 도움말 자산 카탈로그에는 Info.plist에서 아이콘 참조를 [Info.plist에서 자산 카탈로그로 마이그레이션](~/ios/app-fundamentals/images-icons/app-icons.md) 가이드입니다.

자산 카탈로그에 필요한 아이콘의 이름은 **앱 스토어** 및 1024x1024 크기에서 여야 합니다. Apple은 자산 카탈로그의 앱 스토어 아이콘이 투명하거나 알파 채널을 포함할 수 없다고 주장했습니다.

![자산 카탈로그의 아이콘 위치를 저장 하는 앱입니다.](images/image1.png)

## <a name="related-links"></a>관련 링크

- [새로운 iOS 11 (Apple)의 기능](https://developer.apple.com/ios/)
- [업데이트 된 앱 스토어 제품 페이지 (Apple)](https://developer.apple.com/app-store/product-page/)
- [IOS 11 (WWDC) (비디오)에 대 한 앱을 업데이트 하는 중입니다.](https://developer.apple.com/videos/play/wwdc2017/204/)
