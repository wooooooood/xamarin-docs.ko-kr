---
title: Xamarin.ios의 어두운 모드
description: 어두운 모드는 밝은 테마와 어두운 테마에 대 한 새로운 시스템 차원의 옵션입니다. 이제 iOS 사용자가 테마를 선택 하거나 iOS에서 동적으로 모양을 변경할 수 있습니다.
ms.prod: xamarin
ms.assetid: 4F44446E-36B6-4743-9B4D-32278D1D3D66
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/28/2019
ms.openlocfilehash: be487ab839e2fb4d21b85719a56dc34303317a5f
ms.sourcegitcommit: 09bc69d7119a04684c9e804c5cb113b8b1bb7dfc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71206393"
---
# <a name="dark-mode-in-xamarinios"></a>Xamarin.ios의 어두운 모드

어두운 모드는 밝은 테마와 어두운 테마에 대 한 시스템 차원의 옵션입니다. 이제 iOS 사용자가 테마를 선택 하거나, 환경 및 시간에 따라 iOS에서 동적으로 모양을 변경할 수 있습니다.

이 문서에서는 진한 모드를 소개 하 고 iOS 13 응용 프로그램에서 어두운 모드를 지원 합니다.

## <a name="requirements"></a>요구 사항

어두운 모드를 사용 하려면 iOS 13 및 Xcode 11, Xamarin.ios 12.99, Visual Studio 2019 또는 Mac 용 Visual Studio 2019 (Xcode 11 지원 포함)가 필요 합니다.

## <a name="turning-on-dark-mode"></a>어둡게 모드 설정

Apple은 진한 모드와 밝은 모드 간을 전환 하기 위해 iOS 13의 개발자 메뉴를 제공 합니다. IOS 13 시뮬레이터에서 **설정** 을 열고 **개발자** 섹션을 선택한 다음 **짙은 모양** 전환으로 스크롤합니다. 변경 내용은 전체 시뮬레이터 환경에서 반영 됩니다.

![어둡게 모드 설정](dark-mode-images/LightAndDark_DeveloperSetting.png)

## <a name="assets-for-light-and-dark-modes"></a>밝은 모드 및 어두운 모드의 자산

이제 Visual Studio의 Asset Catalog는 각 모양 모드에 대해 선택적 이미지 및 색을 지원 합니다. Universal, 어두움 및 Light가 있습니다. 이러한 방식으로 이미지와 색을 정의 하는 경우 iOS에서 적절 한 이미지 및 색을 자동으로 선택 합니다.

IOS 프로젝트에서 **assets.xcassets** 파일을 열고 새 이미지 집합을 추가 합니다. 대상 해상도에서 universal, 짙은 및 light 이미지를 지정할 수 있습니다. 아래 스크린샷에서는 이름이 "MicrosoftLogo" 인 짙은 조명 이미지를 사용할 수 있습니다.

![밝은 모드 및 어두운 모드의 자산](dark-mode-images/LightAndDark_AssetCatalog2.png)

**Assets.xcassets** 에는 또한 색 정의 인 **BackgroundColor** 및 **TitleColor**에 대 한 항목이 포함 되어 있습니다. 이러한 색은 이제 응용 프로그램 전체에서 사용할 수 있도록 이름으로 제공 됩니다. 다음 스크린샷에 표시 된 것 처럼 **BackgroundColor** 가 보기의 배경에 할당 되 고 **TitleColor** 가 레이블에 할당 됩니다.

![밝은 모드 및 어두운 모드의 자산](dark-mode-images/LightAndDark_01.png)

## <a name="dynamic-system-colors"></a>동적 시스템 색

Apple에는 새로운 짙은 모드 설정에 따라 모양을 동적으로 조정 하는 새로운 의미 체계 색이 도입 되었습니다.

## <a name="summary"></a>요약

이 문서에서는 iOS에 대 한 어두운 모드를 소개 하 고 asset catalog를 사용 하 여 각 모드의 이미지 및 색을 지정 합니다.

## <a name="related-links"></a>관련 링크

- [어두운 모드 디자인 지침](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/dark-mode/)
- [의미 체계 색](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/color/#dynamic-system-colors)
