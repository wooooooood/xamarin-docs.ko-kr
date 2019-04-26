---
title: WatchOS에서 Xamarin 화면 크기를 사용 하 여 작업
description: 이 문서에서는 다양 한 watchOS 화면 크기를 사용 하는 방법을 설명 합니다. WatchOS 인터페이스 디자이너, watchOS 시뮬레이터에 설명 하 고 이미지 리소스입니다.
ms.prod: xamarin
ms.assetid: 840DF939-2F59-4ABA-87D8-92AAC8A92BC4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: b2f4cc71c1993e51ed55b51edd7c50d393e60873
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61412907"
---
# <a name="working-with-watchos-screen-sizes-in-xamarin"></a>WatchOS에서 Xamarin 화면 크기를 사용 하 여 작업

Apple Watch 두 화면 크기에서 제공 됩니다.

- **38mm**
  - 136 x 170 논리 픽셀 (272 x 340 물리적 픽셀)

- **42mm**
  - 156 x 195 논리 픽셀 (312 x 390 물리적 픽셀)입니다.

화면 크기 디자인 및 앱을 테스트 하는 경우 계정에 수행 해야 합니다.

## <a name="watchos-interface-designer"></a>watchOS 인터페이스 디자이너

Mac 디자이너에 대 한 Visual Studio에서 표시 하는 기본적으로에서 인터페이스 컨트롤러를 시청 **Any Apple Watch**합니다.

![](screen-sizes-images/screen-any-sml.png "디자이너를 표시 하는 모든 Apple Watch 인터페이스 컨트롤러를 시청 하세요.")

크기 메뉴를 사용 하 여 편집 및 사용 가능한 화면 크기 중 하나에서 스토리 보드를 미리 보려면: **(38mm** 나 **42 mm**:

![](screen-sizes-images/screen-menu-sml.png "(38mm 또는 42 mm 크기 선택")

화면 크기는 더 작은 화면에서 잘린/숨겨진 콘텐츠를 렌더링 경우에 따라 합니다.
두 크기에서 테스트 해야 합니다.


### <a name="interface-design"></a>인터페이스 디자인

앱 크기에 관계 없이 화면에서 동일한 콘텐츠를 표시 해야 하 고 확장 하거나 적절 하 게 요소 계약 해야 합니다. 특성 검사기에서 Mac 디자이너에 대 한 Visual Studio를 사용 하 여 **컨테이너에 상대적인** 하거나 **크기에 맞게 콘텐츠를** 고정된 크기 보다 우선적으로 합니다.

![](screen-sizes-images/sizeattributepanel-sml.png "컨테이너에 대 한 상대 또는 크기에 맞게 콘텐츠 고정된 크기 보다 우선적으로 사용 하 여")

보기 화면을 검은색 베젤에 둘러싸여, 때문에 사용자 인터페이스 주위에 안쪽 여백을 제공 권장 되지 않습니다. 요소를 화면 가장자리에 대 한 rest 하 고 앱 주위에 테두리를 자연스럽 게 형성 베젤에 사용 수 있습니다.


## <a name="watchos-simulator"></a>watchOS 시뮬레이터

경우 시뮬레이터에서 테스트 간에 쉽게 전환할 수를 사용 하 여 두 화면 크기를 **하드웨어 > 장치** 메뉴.

![](screen-sizes-images/simulator.png "시뮬레이터는 하드웨어 장치 메뉴를 사용 하 여 두 화면 크기 간에 전환할 수 있습니다.")


## <a name="image-resources"></a>이미지 리소스

단일 자산 다양 한 크기의 좋은 찾지 않습니다 하는 경우에 여러 이미지 자산을 사용 해야 합니다. 이미지 자산 카탈로그를 각 크기에 대해 지정할 별도 비트맵에 대 한 허용:

![](screen-sizes-images/images-xcassets.png "이미지 자산 카탈로그 편집기")

```csharp
// specify the asset name, the correct size will automatically be loaded
staticImage.SetImage(UIImage.FromBundle("Walkway"));
```

또는 화면 크기를 확인 하 고 다양 한 이미지를 완전히 로드 하려면 코드를 사용 합니다.

```csharp
bool large = WKInterfaceDevice.CurrentDevice.ScreenBounds.Size.Width > 136.0;
// Load image depending on screen size
using (var image = UIImage.FromBundle (large ? "42mm-Walkway" : "38mm-Walkway"))
{
   myImage.SetImage (image);

}
```

사용 하 여 자세히 알아보세요 합니다 [이미지 컨트롤](~/ios/watchos/user-interface/image.md)합니다.



## <a name="related-links"></a>관련 링크

- [watchOS 3 소개](~/ios/watchos/platform/introduction-to-watchos3/index.md)
