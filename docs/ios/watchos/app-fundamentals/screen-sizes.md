---
title: Xamarin에서 watchOS 화면 크기 작업
description: 이 문서에서는 다양 한 watchOS 화면 크기를 사용 하는 방법을 설명 합니다. WatchOS 인터페이스 디자이너, watchOS 시뮬레이터 및 이미지 리소스에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 840DF939-2F59-4ABA-87D8-92AAC8A92BC4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: aeaa1bb1273bc062e0ac76eaa09722827f15797f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028399"
---
# <a name="working-with-watchos-screen-sizes-in-xamarin"></a>Xamarin에서 watchOS 화면 크기 작업

Apple Watch는 다음과 같은 두 가지 화면 크기로 사용할 수 있습니다.

- **38mm**
  - 136 x 170 논리 픽셀 (272 x 340 실제 픽셀)

- **448mm**
  - 156 x 195 논리 픽셀 (312 x 390 실제 픽셀)

앱을 디자인 하 고 테스트할 때는 화면 크기를 고려해 야 합니다.

## <a name="watchos-interface-designer"></a>watchOS Interface 디자이너

기본적으로 Mac용 Visual Studio 디자이너는 **모든 Apple Watch**에서 조사식 인터페이스 컨트롤러를 표시 합니다.

![](screen-sizes-images/screen-any-sml.png "The Designer displays watch interface controllers at Any Apple Watch")

크기 메뉴를 사용 하 여 사용 가능한 화면 크기 ( **38mm** 또는 **42mm**) 중 하나에서 스토리 보드를 편집 하 고 미리 봅니다.

![](screen-sizes-images/screen-menu-sml.png "Selecting the 38mm or 42mm size")

화면 크기가 클수록 작은 화면에서 잘리거나 숨겨지는 콘텐츠가 렌더링 되는 경우도 있습니다.
두 크기를 모두 테스트 해야 합니다.

### <a name="interface-design"></a>인터페이스 디자인

앱은 크기에 관계 없이 화면에 동일한 콘텐츠를 표시 하 고 필요에 따라 요소를 확장 하거나 축소 해야 합니다. Mac용 Visual Studio 디자이너의 특성 검사자에서는 컨테이너 또는 크기 **를 기준** 으로 기본 설정의 콘텐츠를 고정 크기에 **맞게** 사용 해야 합니다.

![](screen-sizes-images/sizeattributepanel-sml.png "Use Relative to Container or Size to Fit Content in preference to fixed sizes")

조사식 화면이 검정색 베젤을 묶으 므로 인터페이스 주위의 안쪽 여백을 제공 하지 않는 것이 좋습니다. 화면 가장자리에 대 한 요소를 구성 하 고 베젤을 앱 주위에서 자연 스러운 테두리를 만들도록 합니다.

## <a name="watchos-simulator"></a>watchOS 시뮬레이터

시뮬레이터에서 테스트할 때 **하드웨어 > 장치** 메뉴를 사용 하 여 두 화면 크기 간을 쉽게 전환할 수 있습니다.

![](screen-sizes-images/simulator.png "The simulator can switch between the two screen sizes using the Hardware Device menu")

## <a name="image-resources"></a>이미지 리소스

단일 자산이 여러 크기에서 잘 보이지 않는 경우 여러 이미지 자산을 사용 해야 합니다. 이미지 자산 카탈로그를 사용 하면 각 크기에 대해 별도의 비트맵이 지정 됩니다.

![](screen-sizes-images/images-xcassets.png "Image asset catalog editor")

```csharp
// specify the asset name, the correct size will automatically be loaded
staticImage.SetImage(UIImage.FromBundle("Walkway"));
```

또는 코드를 사용 하 여 화면 크기를 확인 하 고 다른 이미지를 모두 로드 합니다.

```csharp
bool large = WKInterfaceDevice.CurrentDevice.ScreenBounds.Size.Width > 136.0;
// Load image depending on screen size
using (var image = UIImage.FromBundle (large ? "42mm-Walkway" : "38mm-Walkway"))
{
   myImage.SetImage (image);

}
```

[이미지 컨트롤](~/ios/watchos/user-interface/image.md)사용에 대해 자세히 알아보세요.

## <a name="related-links"></a>관련 링크

- [watchOS 3 소개](~/ios/watchos/platform/introduction-to-watchos3/index.md)
