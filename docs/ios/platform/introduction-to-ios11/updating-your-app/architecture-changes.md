---
title: IOS 11의에서 아키텍처 변경
description: 이 문서는 iOS 11의에서 32 비트 응용 프로그램의 사용 중단을 설명합니다. 대상 64 비트 아키텍처에 대 한 응용 프로그램을 업데이트 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 55F62F3F-8570-402B-B7D9-2875F76CB946
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: b7d1df6ed2a8bd480025681ebcbe48acd7592564
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61169352"
---
# <a name="architecture-changes-in-ios-11"></a>IOS 11의에서 아키텍처 변경

IOS 11에 알고 있어야 하는 가장 큰 변화 중 하나는에 설명 된 대로 앱에 대 한 32 비트 지원의 중단이 [Apple](https://developer.apple.com/news/?id=06282017b) 언론 발표 합니다. 모든 새 앱과 기존 앱 업데이트에는 64 비트를 지원 해야 합니다. 32 비트 응용 프로그램 **시작 되지 않는** iOS 11의에서 합니다.

Mac 용 Visual Studio에서 앱을 업데이트 하려면 다음 단계를 사용 합니다.

1. 열려는 프로젝트 이름을 마우스 오른쪽 단추로 클릭 **프로젝트 옵션**합니다.
2. 로 이동 합니다 **iOS 빌드** 탭 합니다.
3. 설정 합니다 **지원 되는 아키텍처** 드롭다운을 **x86_64** 에 대 한 합니다 **디버그 | iPhoneSimulator** 하 고 **릴리스 | iPhoneSimulator**:

    ![드롭 다운 시뮬레이터 아키텍처를 변경 합니다.](architecture-changes-images/image1.png)

4. IOS 장치에 대 한 구성을 변경 **디버그 | iPhone** 설정 된 **지원 되는 아키텍처** 드롭다운을 **ARM64**:

    ![드롭 다운 원하는 장치 아키텍처 변경](architecture-changes-images/image2.png)

5. 하도록 구성을 변경 **릴리스 | iPhone** 설정 된 **지원 되는 아키텍처** 드롭다운을 **ARM64**합니다.

32 비트 및 64 비트 아키텍처에 대 한 자세한 내용은 참조는 [32/64 비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64/index.md#ios) 가이드입니다.

## <a name="related-links"></a>관련 링크

- [새로운 iOS 11 (Apple)의 기능](https://developer.apple.com/ios/)
- [업데이트 된 앱 스토어 제품 페이지 (Apple)](https://developer.apple.com/app-store/product-page/)
- [IOS 11 (WWDC) (비디오)에 대 한 앱을 업데이트 하는 중입니다.](https://developer.apple.com/videos/play/wwdc2017/204/)
