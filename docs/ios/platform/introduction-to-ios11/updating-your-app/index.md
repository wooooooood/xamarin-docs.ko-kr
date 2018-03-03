---
title: "IOS 11에 응용 프로그램 업데이트"
description: "IOS 11의 새로운 기능 탐색"
ms.topic: article
ms.prod: xamarin
ms.assetid: EC809504-9CF6-4949-B6EE-36384297E744
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: cced7cc3d1b0579c36d598ef0d05da872478c8dc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="updating-your-app-to-ios-11"></a>IOS 11에 응용 프로그램 업데이트

_IOS 11의 새로운 기능 탐색_

11 ios에서는 Apple 아키텍처 업데이트, 새 시각적 항목이 변경 및 업데이트 된 iTunes Connect 프로세스를 소개 했습니다. 이 가이드 Xamarin.iOS 앱 iOS 11에 대 한 업데이트를 활용할 수 있도록 돕는 각 이러한 변경 내용을 탐색 합니다.

## <a name="architecture-changesarchitecture-changesmd"></a>[아키텍처 변경](architecture-changes.md)

IOS 11에 알고 있어야 하는 가장 중요 한 변경 중 하나는에 설명 된 대로 앱에 대 한 32 비트 지원의 사용 중단 [Apple의](https://developer.apple.com/news/?id=06282017b) 보도 자료.

이 가이드 64 비트 응용 프로그램 업데이트에 대해 설명 합니다.

## <a name="visual-design-updatesvisual-designmd"></a>[시각적 디자인 업데이트](visual-design.md)

IOS 11, 사과 탐색 모음, 검색 표시줄을 및 테이블 보기에 대 한 업데이트를 포함 하 여 새 시각적 항목이 변경을 도입 했습니다. 또한 향상 된 기능에 적용 된 여백 및 전체 화면 콘텐츠를 통해 더욱 유연 하 게 허용 합니다. 이러한 변경 내용은이 가이드에 나와 있습니다.

## <a name="app-store-changesapp-store-changesmd"></a>[앱 스토어 변경 내용](app-store-changes.md)

IOS 앱 스토어에 뿐만 아니라 사용자가 저장소를 효율적으로 탐색할 수 있지만 수도 있습니다를 사용자에 게 앱을 승격 하려면 개발자가 완전히 다시 디자인이 되어 있습니다. 이러한 승격 앱에서 바로 구매에 대 한 업데이트 및 제품 페이지에 대 한 업데이트를 포함 합니다. 또한 11 iOS 사용자와 통신 하는 방법, 응용 프로그램 아이콘을 추가 하는 방법 및 공개적으로 응용 프로그램을 해제 하는 방법에 대 한 업데이트를 추가 합니다.

## <a name="app-icon-updates"></a>업데이트 하는 응용 프로그램 아이콘

> [!NOTE]
> 응용 프로그램 아이콘으로 배달 되도록 이제는 _자산 카탈로그_합니다. 

자산 카탈로그를 사용 하는 방법은 참조는 [앱 스토어 아이콘](~/ios/app-fundamentals/images-icons/app-store-icon.md) 가이드입니다. 마이그레이션에 대 한 도움말 자산 카탈로그에는 Info.plist에서 아이콘 참조는 [Info.plist에서 자산 카탈로그로 마이그레이션](~/ios/app-fundamentals/images-icons/app-icons.md) 가이드입니다.

자산 카탈로그에 필요한 아이콘 라는 **앱 스토어** 크기가 1024 x 1024 이어야 합니다. Apple은 자산 카탈로그의 앱 스토어 아이콘이 투명하거나 알파 채널을 포함할 수 없다고 주장했습니다.

![응용 프로그램 자산 카탈로그 아이콘 위치를 저장 합니다.](images/image1.png)

## <a name="related-links"></a>관련 링크

- [IOS (Apple) 11에서 새로운 이란](https://developer.apple.com/ios/)
- [업데이트 된 앱 스토어 제품 페이지 (Apple)](https://developer.apple.com/app-store/product-page/)
- [IOS 11 (WWDC) (비디오)에 대 한 응용 프로그램 업데이트](https://developer.apple.com/videos/play/wwdc2017/204/)
