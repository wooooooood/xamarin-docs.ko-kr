---
title: Xamarin.ios의 앱 스토어 아이콘
description: 이 문서에서는 자산 카탈로그를 사용 하 여 Xamarin.ios 응용 프로그램에 대 한 앱 스토어 아이콘을 관리 하는 방법을 설명 합니다. 이전에는 앱 스토어 아이콘이 iTunes Connect로 관리 되었습니다.
ms.prod: xamarin
ms.assetid: BFB5665A-F557-46E1-B35E-870CC2026AD9
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/26/2017
ms.openlocfilehash: b6bf2aa8925e29ed5120de5cc2d3146704259d87
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70282402"
---
# <a name="app-store-icons-in-xamarinios"></a>Xamarin.ios의 앱 스토어 아이콘

Xcode 9 이전에는 모든 앱 스토어 아이콘이 iTunes Connect를 통해 추가 되었습니다. 그러나이 방법은 더 이상 그렇지 않습니다. 이제 앱 스토어 아이콘을 프로젝트 번들의 일부로 포함 하 고 자산 카탈로그 내에 추가 해야 합니다. 앱 스토어 아이콘이 포함 되지 않은 앱은 Apple에서 거부 됩니다.

앱 스토어 아이콘은 사용자에 게 응용 프로그램의 얼굴 이므로 기억 하 고 작은 크기로 잘 표시 되어야 합니다. 기억하기 쉬운 아이콘은 명확하고 간단하며 즉시 인식할 수 있습니다.

애플리케이션 아이콘을 디자인할 때 Apple에서 제안하는 지침은 다음과 같습니다.

- 애플리케이션에 적합한 아이콘을 만듭니다.
- 애플리케이션의 디자인과 일치하는 간단한 아이콘을 만듭니다.
- 단어는 아이콘에 사용하지 않습니다.
- 생각은 세계적으로: 모든 스토어 지역에서 단일 앱 아이콘이 사용됩니다.

앱 스토어에 표시할 앱 아이콘에는 1024x1024 픽셀 이미지가 필요합니다.  Apple은 자산 카탈로그의 앱 스토어 아이콘이 투명하거나 알파 채널을 포함할 수 없다고 주장했습니다.

자세한 내용은 Apple의 [IOS 휴먼 인터페이스 지침](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/image-size-and-resolution/)을 참조 하세요.

## <a name="adding-an-app-store-icon"></a>앱 스토어 아이콘 추가

앱 스토어 아이콘은 이제 자산 카탈로그를 통해 제공됩니다. 

앱 스토어 아이콘을 추가 하려면 다음을 수행 합니다.

1. 프로젝트의 **assets.xcassets** 파일에 있는 **AppIcon** 이미지 집합을 찾습니다. 
    - 모든 새 프로젝트에는 AppIcon 이미지 집합이 포함 된 **assets.xcassets** 파일이 함께 제공 됩니다.
    - 새 자산 카탈로그를 추가 하려면 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 파일 > Asset catalog**를 선택 합니다.
    - 새 앱 아이콘 이미지 집합을 추가 하려면 아이콘 집합 영역을 마우스 오른쪽 단추로 클릭 하 고 **앱 아이콘 & 선택 하 여 새 앱 아이콘 > 이미지를 시작**합니다.

    ![새 이미지 집합 추가 옵션](app-store-icon-images/image1.png)

2. 목록에서 **앱 스토어** 아이콘으로 스크롤합니다.

    ![App Store 아이콘](app-store-icon-images/image2.png)

3. 아이콘을 클릭 하 고 1024 x 1024 픽셀 이미지를 찾습니다. 자산 카탈로그를 저장 합니다.




## <a name="related-links"></a>관련 링크

- [자산 카탈로그를 사용 하 여 아이콘 관리](~/ios/app-fundamentals/images-icons/app-icons.md#managing)
