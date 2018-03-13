---
title: "아키텍처 변경"
description: "IOS 11의 새로운 기능 탐색"
ms.topic: article
ms.prod: xamarin
ms.assetid: 55F62F3F-8570-402B-B7D9-2875F76CB946
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 6e233d83eb9c5cb360add36da100963b95e54514
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="architecture-changes"></a>아키텍처 변경

_IOS 11의 새로운 기능 탐색_

IOS 11에 알고 있어야 하는 가장 중요 한 변경 중 하나는에 설명 된 대로 앱에 대 한 32 비트 지원의 사용 중단 [Apple의](https://developer.apple.com/news/?id=06282017b) 보도 자료. 모든 새 앱 및 앱에 대 한 업데이트는 64 비트를 지원 해야 합니다. 32 비트 응용 프로그램 **시작 되지 않는** iOS 11에서에서.

Mac 용 Visual Studio에서 앱을 업데이트 하려면 다음 단계를 사용 합니다.

1. 열려는 프로젝트 이름을 마우스 오른쪽 단추로 클릭 **프로젝트 옵션**합니다.
2. 찾아는 **iOS 빌드** 탭 합니다.
3. 설정의 **지원 되는 아키텍처** 드롭다운을 **x86_64** 에 대 한는 **디버그 | iPhoneSimulator** 및 **릴리스 | iPhoneSimulator**:

    ![드롭 다운 시뮬레이터 아키텍처를 변경 합니다.](architecture-changes-images/image1.png)

4. IOS 장치에 대 한 구성을 변경 **디버그 | iPhone** 설정 하 고는 **지원 되는 아키텍처** 드롭다운을 **ARM64**:

    ![드롭 다운 장치 아키텍처를 변경 합니다.](architecture-changes-images/image2.png)

5. 하도록 구성을 변경 **릴리스 | iPhone** 설정 하 고는 **지원 되는 아키텍처** 드롭다운을 **ARM64**합니다.

32 비트 및 64 비트 아키텍처에 대 한 자세한 내용은 참조는 [32/64 비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64/index.md#ios) 가이드입니다.

## <a name="related-links"></a>관련 링크

- [IOS (Apple) 11에서 새로운 이란](https://developer.apple.com/ios/)
- [업데이트 된 앱 스토어 제품 페이지 (Apple)](https://developer.apple.com/app-store/product-page/)
- [IOS 11 (WWDC) (비디오)에 대 한 응용 프로그램 업데이트](https://developer.apple.com/videos/play/wwdc2017/204/)
