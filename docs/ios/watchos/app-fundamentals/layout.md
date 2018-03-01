---
title: "레이아웃 작업"
ms.topic: article
ms.prod: xamarin
ms.assetid: BEDB62A1-2249-4459-986F-413A41E63DF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 71a192343800dc11f46c645c27ab8a74209c92e6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-layout"></a>레이아웃 작업

Apple Watch 대 한 레이아웃을 디자인 [크기 화면](~/ios/watchos/app-fundamentals/screen-sizes.md) 고유한 문제가 있습니다.

## <a name="design-tips"></a>디자인 팁

중요 한 점은: 읽을 수 있는 되며 큰 손가락으로 작은 watch 화면에서 사용 가능한 사용자 인터페이스를 확인 합니다. 디자인에 대 한 트랩에 포함 되지는 **iOS 시뮬레이터** (매우 큰 표시) 및 **마우스 포인터** (작동 하는 아주 작은 터치 대상이 있는)!

- 검은색 배경 사용-시계의 검정 베젤에으로 더 큰 화면의 효과 생성 합니다.

- 화면 레이아웃 주위의 패딩 되지 않는-는 베젤에 형성 자연 visual 여백으로 늘어납니다.

- 가독성에 집중 합니다. 텍스트를 읽을 수 있도록 글꼴 크기 및 색을 주의 해 서 사용 합니다. 기본 제공 텍스트 스타일을 사용 하 여 자동 동적 형식 지원.

![](layout-images/type.png "동적 형식 지원의 예")

- 터치 대상 크기에 집중 합니다. 텍스트 레이블이 있는 단추가/tappable 테이블 행에는 전체 화면을 확장 되어야 합니다. Apple "두지 마십시오 개 이상의 항목이 세 개인 병렬 하 여", 아이콘 및 텍스트 레이블 사용 하는 경우 코드 의미 합니다.

- 사용 하 여는 [ `Menu` 제어](~/ios/watchos/user-interface/menu.md) 간단 명료한 응용 프로그램 디자인 변경 하지 않으려면 노출 덜 자주 사용 되는 기능입니다.


## <a name="implementation"></a>구현

조사식 키트 매력적인 watch 앱 레이아웃을 빌드할 수 있도록 다음과 같은 컨트롤에 포함 됩니다.

### <a name="interface-controller"></a>인터페이스 컨트롤러

`WKInterfaceController` 장면 프로그램의 모든 기본 클래스입니다.

세로 처럼 작동 하며 인터페이스 컨트롤러에 대 한 디자인 화면 **그룹**: 인터페이스 컨트롤러에 다른 컨트롤을 끌어 수 되며 자동으로 레이아웃이 위와 다른 수 없게 됩니다.

![](layout-images/controller-scene.png "컨트롤은 자동으로 레이아웃이 위와 다른")

설정할 수 있습니다는 **위치** 및 **크기** 각 컨트롤의 모양을 제어 하는 속성:

![](layout-images/positionsize-attributes.png "각 컨트롤의 위치와 크기 속성을 설정 합니다.")

크기로 설정 되 면 **컨테이너에 대 한 상대** 오프셋된 조정 및 비율 값을 제공할 수 있습니다. 이 스크린샷은 조사식 화면 너비의 80%를 사용 하도록 설정 된 단추 (**0.8**):

![](layout-images/button-attributes.png "오프셋된 조정 및 비율 값을 제공 합니다.")


### <a name="group"></a>그룹화

`WKInterfaceGroup` 가로 또는 세로로 컨트롤을 쌓습니다 하도록 구성할 수 있습니다는 단순 레이아웃 컨테이너입니다. 기본적으로 각 컨트롤 사이의 간격을 포함 하지만에서 간격 (및 음각)를 수정할 수 있습니다는 **특성** 검사기입니다.

![](layout-images/group-attributes.png "간격 및 너비 특성 검사기에서 수정")

그룹 수 자체 크기가 정 해지고 주위의 컨트롤을 기준으로 배치 하 고 복잡 한 레이아웃을 만드는 그룹 중첩 될 수 있습니다.

![](layout-images/group-scene.png "복잡 한 레이아웃을 만드는 그룹 중첩 될 수 있습니다.")


### <a name="separator"></a>구분 기호

구분 기호 컨트롤 레이아웃에 대 한 시각적 지침을 제공 하는 데 도움이 됩니다. 구분 기호 (또는 배경색 또는 이미지)는 사용자 화면에 관련 된 콘텐츠를 이해 하는 데 사용 합니다.

![](layout-images/separator-scene.png "구분 기호 사용 예")

전체 화면 너비를 사용 하지 않는 녹색 및 파란색 구분 기호 사용 하 여 구성 된 참고 **Fixed** 또는 **컨테이너에 대 한 상대** 크기입니다.

### <a name="content-controls"></a>콘텐츠 컨트롤

레이아웃 없음 완벽 한 것은 `Label`, `Image`, `Button`, `Switch`, `Slider`, `Map`, 및 [다른 컨트롤](~/ios/watchos/user-interface/index.md)합니다.
사용 하 여 레이아웃에 배치 될 수 있습니다 이러한 **그룹** 또는 각 컨트롤의 위치 및 크기가 설정 합니다.



## <a name="related-links"></a>관련 링크

- [WatchKitCatalog (샘플)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Apple의 레이아웃 참조](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Layout.html)
- [Apple의 색 및 입력 체계 참조](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/ColorandTypography.html)
