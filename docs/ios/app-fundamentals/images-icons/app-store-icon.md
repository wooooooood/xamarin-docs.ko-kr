---
title: Xamarin.iOS 앱 스토어 아이콘
description: 이 문서에서는 앱 스토어 아이콘 Xamarin.iOS 응용 프로그램을 관리 하려면 자산 카탈로그를 사용 하는 방법을 설명 합니다. 이전에 응용 프로그램 스토어 아이콘 iTunes Connect으로 관리 되었습니다.
ms.prod: xamarin
ms.assetid: BFB5665A-F557-46E1-B35E-870CC2026AD9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/26/2017
ms.openlocfilehash: 749dbf01af382a54fe24652706f6a605ac7b20b4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783612"
---
# <a name="app-store-icons-in-xamarinios"></a>Xamarin.iOS 앱 스토어 아이콘

Xcode 9 하기 전에 모든 앱 스토어 아이콘 iTunes Connect 통해 추가 되었습니다. 그러나이 경우 더 이상는 합니다. 앱 스토어 아이콘 이제 프로젝트 번들의 일부분으로 포함와 자산 카탈로그 내에 추가 합니다. 앱 스토어 아이콘을 포함 하지 않는 앱을 Apple에 의해 거부 됩니다.

앱 스토어 아이콘은 기억 하기 쉬운 이어야 하므로 사용자 및 소형 크기에서 잘 표시 하도록 응용 프로그램의 모양입니다. 기억하기 쉬운 아이콘은 명확하고 간단하며 즉시 인식할 수 있습니다.

응용 프로그램 아이콘을 디자인할 때 Apple에서 제안하는 지침은 다음과 같습니다.

- 응용 프로그램에 적합한 아이콘을 만듭니다.
- 응용 프로그램의 디자인과 일치하는 간단한 아이콘을 만듭니다.
- 단어는 아이콘에 사용하지 않습니다.
- 전 세계적으로 생각: 단일 앱 아이콘이 모든 스토어 지역에서 사용됩니다.

앱 스토어에 표시할 앱 아이콘에는 1024x1024 픽셀 이미지가 필요합니다.  Apple은 자산 카탈로그의 앱 스토어 아이콘이 투명하거나 알파 채널을 포함할 수 없다고 주장했습니다.

자세한 내용은 Apple의을 참조 하십시오. [iOS Human Interface Guidelines](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/image-size-and-resolution/)합니다.

## <a name="adding-an-app-store-icon"></a>앱 스토어 아이콘 추가

앱 스토어 아이콘은 이제 자산 카탈로그를 통해 제공됩니다. 

앱 스토어 아이콘 추가 하려면 다음을 수행 합니다.

1. 찾습니다는 **AppIcon** 이미지에서 설정 된 **Assets.xcassets** 프로젝트의 파일입니다. 
    - 모든 새 프로젝트를 가져와야 하는 **Assets.xcassets** AppIcon 이미지 집합을 포함 하는 파일입니다.
    - 새 자산 카탈로그를 추가 하려면 단추로 클릭 하 여 프로젝트 고 **추가 > 새 파일 > 자산 카탈로그**합니다.
    - 새 응용 프로그램 아이콘 이미지 집합을 추가 하려면 마우스 오른쪽 단추로 클릭 하 고 선택 아이콘 집합 영역의 **앱 아이콘 및 시작 이미지 > 새 응용 프로그램 아이콘**:
    
    ![새 이미지 set 옵션 추가](app-store-icon-images/image1.png)

2. 스크롤하여는 **앱 스토어** 목록에서 아이콘:

    ![앱 스토어 아이콘](app-store-icon-images/image2.png)

3. 1024 x 1024 픽셀 이미지에 대 한 찾아보기 고 아이콘을 클릭 합니다. 자산 카탈로그의 저장 합니다.




## <a name="related-links"></a>관련 링크

- [자산 카탈로그와 함께 관리 아이콘](~/ios/app-fundamentals/images-icons/app-icons.md#managing)
