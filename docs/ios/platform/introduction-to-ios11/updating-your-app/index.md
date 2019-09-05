---
title: 앱을 iOS 11로 업데이트
description: 이 문서는 iOS 11 릴리스를 사용 하 여 Xamarin.ios 개발자가 사용할 수 있는 새로운 기능을 설명 하는 다양 한 가이드에 연결 됩니다. 예를 들어 시각적 디자인 업데이트, 앱 스토어 변경 및 앱 아이콘 업데이트가 있습니다.
ms.prod: xamarin
ms.assetid: EC809504-9CF6-4949-B6EE-36384297E744
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/13/2016
ms.openlocfilehash: 3f331c38d470ba77de7b3fc4ef0143bb673683e8
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70286337"
---
# <a name="updating-your-app-to-ios-11"></a>앱을 iOS 11로 업데이트

IOS 11에서 Apple은 아키텍처 업데이트, 새로운 시각적 변경 및 업데이트 된 iTunes Connect 프로세스를 도입 했습니다. 이 가이드는 이러한 각 변경 사항을 살펴보고 iOS 11에 대 한 Xamarin.ios 앱을 업데이트 하는 데 도움을 줍니다.

## <a name="architecture-changesarchitecture-changesmd"></a>[아키텍처 변경 내용](architecture-changes.md)

IOS 11에서 알고 있어야 하는 가장 큰 변경 내용 중 하나는 [Apple](https://developer.apple.com/news/?id=06282017b) press 보도에 설명 된 대로 앱에 대 한 32 비트 지원을 사용 하지 않는 것입니다.

이 가이드에서는 64 비트 앱을 업데이트 하는 과정을 안내 합니다.

## <a name="visual-design-updatesvisual-designmd"></a>[비주얼 디자인 업데이트](visual-design.md)

IOS 11에서 Apple은 탐색 모음, 검색 표시줄 및 테이블 보기의 업데이트를 포함 하 여 새로운 시각적 변경을 도입 했습니다. 또한 여백과 전체 화면 콘텐츠를 더욱 유연 하 게 만들 수 있도록 기능이 향상 되었습니다. 이러한 변경 내용에 대해서는이 가이드에서 설명 합니다.

## <a name="app-store-changesapp-store-changesmd"></a>[App Store 변경](app-store-changes.md)

IOS 앱 스토어는 사용자가 저장소를 효율적으로 탐색할 수 있을 뿐 아니라 개발자가 앱을 사용자에 게 승격할 수 있도록 하는 완전히 다시 설계 되었습니다. 이러한 판촉에는 앱 내 구매 및 제품 페이지에 대 한 업데이트에 대 한 업데이트가 포함 됩니다. 또한 iOS 11은 사용자와 통신 하는 방법, 앱 아이콘을 추가 하는 방법 및 앱을 공용으로 릴리스 하는 방법에 대 한 업데이트를 추가 합니다.

## <a name="app-icon-updates"></a>앱 아이콘 업데이트

> [!NOTE]
> 이제 _자산 카탈로그_에서 앱 아이콘을 배달 해야 합니다. 

자산 카탈로그를 사용 하는 방법에 대 한 자세한 내용은 [앱 스토어 아이콘](~/ios/app-fundamentals/images-icons/app-store-icon.md) 가이드를 참조 하세요. Info.plist에서 자산 카탈로그로 아이콘을 마이그레이션하는 방법에 대 한 도움말은 [info.plist에서 자산 카탈로그로 마이그레이션](~/ios/app-fundamentals/images-icons/app-icons.md) 가이드를 참조 하세요.

자산 카탈로그의 필수 아이콘은 **App Store** 로 명명 되며 1024 x 1024 여야 합니다. Apple은 자산 카탈로그의 앱 스토어 아이콘이 투명하거나 알파 채널을 포함할 수 없다고 주장했습니다.

![자산 카탈로그의 앱 스토어 아이콘 위치입니다.](images/image1.png)

## <a name="related-links"></a>관련 링크

- [IOS 11의 새로운 기능 (Apple)](https://developer.apple.com/ios/)
- [업데이트 된 앱 스토어 제품 페이지 (Apple)](https://developer.apple.com/app-store/product-page/)
- [IOS 11 용 앱 업데이트 (WWDC) (비디오)](https://developer.apple.com/videos/play/wwdc2017/204/)
