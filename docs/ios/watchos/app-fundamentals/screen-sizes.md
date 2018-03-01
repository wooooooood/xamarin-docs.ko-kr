---
title: "화면 크기와 작업"
ms.topic: article
ms.prod: xamarin
ms.assetid: 156D6D1C-83CA-4088-BA08-40B22312269C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: c64e5821c1067b64caf3afc85c0e290a354e914d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-screen-sizes"></a>화면 크기와 작업

Apple Watch 두 개의 화면 크기에서 제공 됩니다.

- **38mm**
  - 136 x 170 논리 픽셀 (272 x 340 물리적 픽셀)

- **42mm**
  - 156 x 195 논리 픽셀 (312 x 390 물리적 픽셀)입니다.

화면 크기를 디자인 하 고 앱을 테스트할 때 취해야 합니다.

## <a name="watchos-interface-designer"></a>watchOS 인터페이스 디자이너

Mac 디자이너에 대 한 Visual Studio에서는 기본적으로 감시에 인터페이스 컨트롤러 **Any Apple Watch**합니다.

![](screen-sizes-images/screen-any-sml.png "Any Apple Watch 디자이너 표시 조사식 인터페이스 컨트롤러")

크기 메뉴를 사용 하 여 편집 하 고 사용 가능한 화면 크기 중 하나에 스토리 보드를 미리 볼: **38 mm** 또는 **42 mm**:

![](screen-sizes-images/screen-menu-sml.png "38 mm 또는 42 mm 크기를 선택합니다.")

화면 크기 보다 큰 경우에 따라 콘텐츠는 더 작은 화면에 잘린/숨겨진 것으로 렌더링 합니다.
두 크기에서 테스트 해야 합니다.


### <a name="interface-design"></a>인터페이스 디자인

앱 크기에 관계 없이 화면에서 동일한 콘텐츠를 표시 해야 하 고 확장 또는 축소 요소를 적절 하 게 해야 합니다. 속성 검사자의 Mac 디자이너에 대 한 Visual Studio에서 사용 해야 **컨테이너에 대 한 상대** 또는 **에 맞게 콘텐츠 크기** 고정된 크기 대신 합니다.

![](screen-sizes-images/sizeattributepanel-sml.png "컨테이너에 대 한 상대 또는에 맞게 콘텐츠 크기를 사용 하 여 고정된 크기 대신")

조사식 화면 주위에 검은색 베젤에 때문에 인터페이스 주위의 여백 제공 권장 되지 않습니다. 화면 가장자리에 고 앱 주위에 테두리를 자연 스러운 형성 베젤에 요소를 사용 합니다.


## <a name="watchos-simulator"></a>시뮬레이터 watchOS

경우 시뮬레이터에서 테스트 간을 쉽게 전환할 수를 사용 하 여 두 개의 화면 크기는 **하드웨어 > 장치** 메뉴.

![](screen-sizes-images/simulator.png "시뮬레이터의 하드웨어 장치 메뉴를 사용 하 여 두 개의 화면 크기 사이 전환할 수 있습니다.")


## <a name="image-resources"></a>이미지 리소스

단일 자산 다양 한 크기 제대로 표시 하지 않는 경우에 여러 개의 이미지 자산을 사용 해야 합니다. 이미지 자산 카탈로그를 각 크기에 대해 지정할 별도 비트맵에 대 한 허용:

![](screen-sizes-images/images-xcassets.png "이미지 자산 카탈로그 편집기")

```csharp
// specify the asset name, the correct size will automatically be loaded
staticImage.SetImage(UIImage.FromBundle("Walkway"));
```

또는 코드를 사용 하 여 화면 크기를 결정 하 여 서로 다른 이미지를 모두 로드 합니다.

```csharp
bool large = WKInterfaceDevice.CurrentDevice.ScreenBounds.Size.Width > 136.0;
// Load image depending on screen size
using (var image = UIImage.FromBundle (large ? "42mm-Walkway" : "38mm-Walkway"))
{
   myImage.SetImage (image);

}
```

사용 하 여에 대 한 자세한는 [이미지 컨트롤](~/ios/watchos/user-interface/image.md)합니다.



## <a name="related-links"></a>관련 링크

- [WatchOS 3 소개](~/ios/watchos/platform/introduction-to-watchos3/index.md)
