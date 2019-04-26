---
title: Watchos에서 Xamarin 레이아웃 작업
description: 이 문서에서는 Xamarin을 사용 하 여 watchOS 레이아웃을 만드는 방법을 설명 합니다. 인터페이스 컨트롤러, 그룹, 구분 기호 및 콘텐츠 컨트롤에 설명 합니다.
ms.prod: xamarin
ms.assetid: BEDB62A1-2249-4459-986F-413A41E63DF0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 9a9bec364990f683a59e6ddce1a536950cdf3861
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61392693"
---
# <a name="working-with-watchos-layout-in-xamarin"></a>Watchos에서 Xamarin 레이아웃 작업

Apple Watch 대 한 레이아웃을 디자인 [화면 크기](~/ios/watchos/app-fundamentals/screen-sizes.md) 고유한 과제를 제공 합니다.

## <a name="design-tips"></a>디자인 팁

중요 한 점은: 읽을 수 있으며 큰 손가락을 사용 하 여 작은 watch 화면의 사용 가능한 사용자 인터페이스를 확인 합니다. 에 대 한 디자인의 트랩에 포함 되지 합니다 **iOS 시뮬레이터** (매우 큰 표시 됨) 및 **포인터가** (에서 작동 하는 작은 터치 대상)!

- 검은색 배경 사용-시계의 black 베젤에 사용 하 여 더 큰 화면 처럼 보이게 만듭니다.

- 화면 레이아웃 주위 패딩하지 수행-는 베젤에 forms 자연 스러운 visual 안쪽 여백입니다.

- 가독성에 집중 합니다. 텍스트를 읽을 수 있도록 글꼴 크기 및 색을 주의 해 서 사용 합니다. 자동 동적 형식 지원을 받기 위해 기본 제공 텍스트 스타일을 사용 합니다.

![](layout-images/type.png "동적 형식 지원의 예")

- 터치 대상 크기에 집중 합니다. 단추/tappable 테이블 행을 텍스트 레이블로 화면 전체를 포괄 해야 합니다. Apple "되지 세 항목-side-by-side 이상" 등을 사용 하는 경우 아이콘 및 텍스트 레이블 표시 됩니다.

- 사용 된 [ `Menu` 컨트롤](~/ios/watchos/user-interface/menu.md) 노출 자주 사용 되는 앱 디자인 깔끔하고 간결 하 게 유지 하는 기능을 합니다.


## <a name="implementation"></a>구현

키트에는 매력적인 watch 앱 레이아웃을 빌드할 수 있도록 다음 컨트롤이 포함 되어 있습니다. 시청 하세요.

### <a name="interface-controller"></a>인터페이스 컨트롤러

`WKInterfaceController` 에 백그라운드에서의 모든 기본 클래스입니다.

인터페이스 컨트롤러에 대 한 디자인 화면 세로 처럼 **그룹**: 인터페이스 컨트롤러에 다른 컨트롤을 끌어올 수 있습니다 하 고 자동으로 배치 된 위와 다른 됩니다.

![](layout-images/controller-scene.png "컨트롤은 자동으로 배치 된 위와 다른")

설정할 수 있습니다 합니다 **위치** 하 고 **크기** 모양을 제어 하려면 각 컨트롤의 속성:

![](layout-images/positionsize-attributes.png "각 컨트롤의 위치와 크기 속성을 설정")

크기로 설정 되 면 **컨테이너에 상대적인** 오프셋된 조정 및 비율 값을 제공할 수 있습니다. 이 스크린샷은 watch 화면의 너비의 80%를 사용 하도록 설정 된 단추 (**0.8**):

![](layout-images/button-attributes.png "오프셋된 조정 및 비율 값을 제공 합니다.")


### <a name="group"></a>그룹화

`WKInterfaceGroup` 가로 또는 세로로 단순형 레이아웃 컨테이너에 있는 stack 구성할 수 있는 컨트롤입니다. 기본적으로 각 컨트롤 사이의 간격을 포함 하지만에서 간격 (및 음각)를 수정할 수 있습니다 합니다 **특성** 검사기입니다.

![](layout-images/group-attributes.png "간격 및 너비 특성 검사기에서 수정")

그룹 자체 크기 및 수, 그와 관련 컨트롤을 기준으로 배치 하 고 복잡 한 레이아웃을 만들 그룹 중첩 될 수 있습니다.

![](layout-images/group-scene.png "복잡 한 레이아웃을 만들 그룹 중첩 될 수 있습니다.")


### <a name="separator"></a>구분 기호

구분 기호 컨트롤 레이아웃에서 시각적 지침을 제공 하기 위한 것입니다. 구분 기호 (배경색 또는 이미지)는 사용자 화면에 관련 된 콘텐츠를 이해 하는 데 사용 합니다.

![](layout-images/separator-scene.png "구분 기호 사용의 예")

화면의 전체 너비를 사용 하지 않는 파랑 및 녹색 구분 기호를 사용 하 여 구성한 유의 **Fixed** 또는 **컨테이너에 상대적인** 크기입니다.

### <a name="content-controls"></a>콘텐츠 컨트롤

없는 레이아웃에 없어서는 안 합니다 `Label`, `Image`, `Button`, `Switch`, `Slider`를 `Map`, 및 [다른 컨트롤](~/ios/watchos/user-interface/index.md)합니다.
사용 하 여 레이아웃에 배치 될 수 있습니다 이러한 **그룹** 또는 각 컨트롤의 위치와 크기로 설정 합니다.



## <a name="related-links"></a>관련 링크

- [WatchKitCatalog (샘플)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple의 레이아웃 참조](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Layout.html)
- [Apple의 색 및 입력 체계 참조](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/ColorandTypography.html)
