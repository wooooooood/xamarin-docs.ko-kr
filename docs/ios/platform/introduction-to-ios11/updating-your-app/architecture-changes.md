---
title: IOS 11의 아키텍처 변경 내용
description: 이 문서에서는 iOS 11에서 32 비트 앱을 사용 하지 않는 방법을 설명 합니다. 64 비트 아키텍처를 대상으로 하는 응용 프로그램을 업데이트 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 55F62F3F-8570-402B-B7D9-2875F76CB946
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/13/2016
ms.openlocfilehash: 15cd6139cc83639146e6044d2b791d94ee30f2d9
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70286354"
---
# <a name="architecture-changes-in-ios-11"></a>IOS 11의 아키텍처 변경 내용

IOS 11에서 알고 있어야 하는 가장 큰 변경 내용 중 하나는 [Apple](https://developer.apple.com/news/?id=06282017b) press 보도에 설명 된 대로 앱에 대 한 32 비트 지원을 사용 하지 않는 것입니다. 모든 새 앱과 기존 앱에 대 한 업데이트는 64 비트를 지원 해야 합니다. 32 비트 앱은 iOS 11에서 **실행 되지** 않습니다.

Mac용 Visual Studio에서 앱을 업데이트 하려면 다음 단계를 사용 합니다.

1. 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 여 **프로젝트 옵션**을 엽니다.
2. **IOS 빌드** 탭으로 이동 합니다.
3. **디버그 | 드롭다운에서 iphonesimulator 대상을** 및 **릴리스 | 드롭다운에서 iphonesimulator 대상을**에 대해 **지원 되는 아키텍처** 드롭다운을 **x86_64** 로 설정 합니다.

    ![시뮬레이터 아키텍처 변경 드롭다운](architecture-changes-images/image1.png)

4. IOS 장치의 경우 구성을 **디버그 | iPhone** 으로 변경 하 고 **지원 되는 아키텍처** 드롭다운을 **ARM64**로 설정 합니다.

    ![장치 아키텍처 변경 드롭다운](architecture-changes-images/image2.png)

5. 구성을 **릴리스 | iPhone** 으로 변경 하 고 **지원 되는 아키텍처** 드롭다운을 **ARM64**로 설정 합니다.

32 비트 및 64 비트 아키텍처에 대 한 자세한 내용은 [32/64 비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64/index.md#ios) 가이드를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [IOS 11의 새로운 기능 (Apple)](https://developer.apple.com/ios/)
- [업데이트 된 앱 스토어 제품 페이지 (Apple)](https://developer.apple.com/app-store/product-page/)
- [IOS 11 용 앱 업데이트 (WWDC) (비디오)](https://developer.apple.com/videos/play/wwdc2017/204/)
