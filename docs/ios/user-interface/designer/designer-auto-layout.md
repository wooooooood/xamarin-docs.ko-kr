---
title: Xamarin Designer for iOS 자동 레이아웃
description: 이 가이드에서는 iOS 자동 레이아웃을 소개 하 고 Xamarin Designer for iOS를 사용 하 여 제약 조건을 사용 하 여 레이아웃을 만들고 편집 하는 방법을 설명 합니다. 또한 코드의 제약 조건 수정, 제약 조건 변경 내용 적용 등에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: CAC7A715-55BB-45E2-BB6D-2168D36D428F
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: f931397f50b6b7aece099efb775a6dda560bf0eb
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70279998"
---
# <a name="auto-layout-with-the-xamarin-designer-for-ios"></a>Xamarin Designer for iOS 자동 레이아웃

자동 레이아웃 ("적응 레이아웃"이 라고도 함)은 응답성이 뛰어난 디자인 방법입니다. 각 요소의 위치가 화면의 점으로 하드 코딩 되는 전환 레이아웃 시스템과 달리 자동 레이아웃은 디자인 화면에서 다른 요소를 기준으로 하는 요소의 위치와 *관계* 에 대 한 것입니다. 자동 레이아웃의 핵심은 화면에 있는 다른 요소의 컨텍스트에서 요소나 요소의 집합을 정의 하는 제약 조건이 나 규칙의 개념입니다. 요소가 화면에서 특정 위치에 연결 되어 있지 않기 때문에 제약 조건을 통해 다양 한 화면 크기 및 장치 방향에 적합 한 적응 레이아웃을 만들 수 있습니다.

이 가이드에서는 Xamarin iOS 디자이너에서 제약 조건과이를 사용 하는 방법을 소개 합니다. 이 가이드에서는 프로그래밍 방식으로 제약 조건 작업에 대해 다루지 않습니다. 프로그래밍 방식으로 자동 레이아웃을 사용 하는 방법에 대 한 자세한 내용은 [Apple 설명서](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html)를 참조 하세요.

## <a name="requirements"></a>요구 사항

Xamarin Designer for iOS은 Mac용 Visual Studio Visual Studio 2017 이상에서 사용할 수 있습니다.

이 가이드에서는 [IOS 디자이너 소개](~/ios/user-interface/designer/introduction.md) 가이드의 디자이너 구성 요소에 대 한 지식이 있다고 가정 합니다.

## <a name="introduction-to-constraints"></a>제약 조건 소개

제약 조건은 화면에 있는 두 요소 간의 관계를 수학적으로 표현한 것입니다. UI 요소의 위치를 수학적 관계로 표현 하면 UI 요소의 위치를 하드 코딩 하는 것과 관련 된 몇 가지 문제를 해결 합니다. 예를 들어 화면 아래쪽의 세로 모드에서 단추 20px를 배치 하는 경우 단추의 위치는 가로 모드에서 화면을 벗어날 것입니다. 이를 방지 하기 위해 20px 단추의 아래쪽 가장자리를 뷰의 아래쪽 가장자리에 배치 하는 제약 조건을 설정할 수 있습니다. 그러면 단추 가장자리의 위치가 button으로 계산 됩니다 *. bottom = 20px*.이 단추를 클릭 하면 뷰 맨 아래에서 세로 및 가로 모드로 단추 20px 배치 됩니다. 수학적 관계를 기준으로 배치를 계산 하는 기능은 UI 디자인에서 제약 조건을 유용 하 게 만드는 것입니다.

제약 조건을 설정할 때 제약 조건이 적용 되는 `NSLayoutConstraint` 개체와 제약 조건이 적용 되는 속성 또는 *특성*을 인수로 사용 하는 개체를 만듭니다. IOS 디자이너에서 특성에는 요소의 *왼쪽*, *오른쪽*, *위쪽*및 *아래쪽* 과 같은 가장자리가 포함 됩니다. 여기에는 *높이* 및 *너비*와 같은 크기 특성, 중심점 위치 ( *system.windows.media.rotatetransform.centerx* 및 *system.windows.media.rotatetransform.centery*)도 포함 됩니다. 예를 들어 두 단추의 왼쪽 경계 위치에 제약 조건을 추가 하는 경우 디자이너는 내부적으로 다음 코드를 생성 합니다.

```csharp
View.AddConstraint (NSLayoutConstraint.Create (Button1, NSLayoutAttribute.Left, NSLayoutRelation.Equal, Button2, NSLayoutAttribute.Left, 1, 10));
```

다음 섹션에서는 자동 레이아웃을 설정 및 해제 하 고 제약 조건 도구 모음을 사용 하는 등 iOS Designer를 사용 하는 제약 조건 작업에 대해 설명 합니다.

## <a name="enable-auto-layout"></a>자동 레이아웃 사용

기본 iOS 디자이너 구성에는 제약 조건 모드가 사용 됩니다. 그러나이를 수동으로 활성화 하거나 비활성화 해야 하는 경우 다음 두 단계로 수행할 수 있습니다.

1. 디자인 화면에서 빈 공간을 클릭 합니다. 이렇게 하면 모든 요소를 선택 취소 하 고 스토리 보드 문서에 대 한 속성을 표시 합니다.
1. 속성 패널에서 **자동 레이아웃 사용** 확인란을 선택 하거나 선택 취소 합니다.

    ![](designer-auto-layout-images/image01.png "속성 패널의 자동 레이아웃 사용 확인란")


기본적으로 화면에는 제약 조건이 만들어지거나 표시 되지 않습니다. 대신 컴파일 시간에 프레임 정보에서 자동으로 유추 됩니다. 제약 조건을 추가 하려면 디자인 화면에서 요소를 선택 하 고 여기에 제약 조건을 추가 해야 합니다. **제약 조건 도구 모음**을 사용 하 여이 작업을 수행할 수 있습니다.

## <a name="constraints-toolbar"></a>제약 조건 도구 모음

 [![](designer-auto-layout-images/toolbarnew.png "상황에 맞는 메뉴 명령")](designer-auto-layout-images/toolbarnew.png#lightbox)

제약 조건 도구 모음이 업데이트 되어 이제 다음 두 가지 주요 부분으로 구성 됩니다.

- **제약 조건 모드 단추 토글**: 이전에는 디자인 화면에서 선택한 뷰를 다시 클릭 하 여 제약 조건 모드를 시작 했습니다. 이제 제약 조건 표시줄에서이 토글 단추를 사용 해야 합니다.

  ![제약 조건 모드 설정/해제](designer-auto-layout-images/constraints.png)

- **"제약 조건 업데이트" 단추:** 제약 조건 편집 모드에 있는지에 따라 변경 내용이 적용 되는 것을 확인 하는 것이 중요 합니다.
  - 제약 조건 편집 모드에서이 단추는 요소 프레임과 일치 하도록 제약 조건을 조정 합니다.
  - 프레임 편집 모드에서이 단추는 제약 조건이 정의 하는 위치와 일치 하도록 요소 프레임을 조정 합니다.


## <a name="surface-based-constraint-editing"></a>화면 기반 제약 조건 편집

이전 섹션에서는 제약 조건 도구 모음을 사용 하 여 기본 제약 조건을 추가 하 고 제약 조건을 제거 하는 방법을 배웠습니다. 보다 세부적으로 조정 된 제약 조건 편집을 위해 디자인 화면에서 직접 제약 조건과 상호 작용할 수 있습니다. 이 섹션에서는 pin 간격 컨트롤, 끌어 놓기 영역, 다양 한 형식의 제약 조건 작업을 포함 하 여 화면 기반 제약 조건 편집의 기본 사항을 소개 합니다.

### <a name="creating-constraints"></a>제약 조건 만들기

IOS Designer 도구는 디자인 화면에서 요소를 조작 하기 위한 두 가지 형식의 컨트롤을 제공 합니다. 다음 이미지에 나와 있는 것 처럼 컨트롤 및 *고정 간격 컨트롤*을 *끕니다* .

![뷰 컨트롤](designer-auto-layout-images/controls.png)

제약 조건 표시줄에서 제약 조건 모드 단추를 선택 하 여 전환 합니다.

요소의 양쪽에 있는 4 개의 T 모양 핸들은 제약 조건에 대 한 요소의 *위쪽*, *오른쪽*, *아래쪽*및 *왼쪽* 가장자리를 정의 합니다. 요소의 오른쪽 아래에 있는 두 개의 셰이프 핸들이 각각 *height* 및 *width* 제약 조건을 정의 합니다. 중간 사각형은 *system.windows.media.rotatetransform.centerx* 및 *system.windows.media.rotatetransform.centery* 제약 조건을 모두 처리 합니다.

제약 조건을 만들려면 핸들을 선택 하 고 디자인 화면에서 원하는 위치로 끕니다. 끌기를 시작 하면 제한할 수 있는 항목을 나타내는 녹색 선/상자가 화면에 표시 됩니다. 예를 들어 아래 스크린샷에서 가운데 단추의 위쪽을 제약 합니다.

 [![](designer-auto-layout-images/image07.png "가운데 단추의 위쪽 제한")](designer-auto-layout-images/image07.png#lightbox)

다른 두 단추에 있는 세 개의 파선 녹색 줄을 확인 합니다. 녹색 선은 *드롭 영역*또는 제한할 수 있는 다른 요소의 특성을 표시 합니다. 위의 스크린샷에서 다른 두 단추는 단추를 제한 하기 위한 3 개의 세로 끌어 놓기 영역 ( *bottom*, *system.windows.media.rotatetransform.centery*, *top*)을 제공 합니다. 뷰의 위쪽에 있는 녹색 파선은 뷰 컨트롤러가 보기 위쪽에 제약 조건을 제공 하 고 녹색 녹색 상자는 뷰 컨트롤러에서 위쪽 레이아웃 안내선 아래에 제약 조건을 제공 한다는 것을 의미 합니다.

> [!IMPORTANT]
> 레이아웃 안내선은 상태 표시줄 또는 도구 모음과 같은 시스템 바의 존재를 고려 하는 상위 및 하위 제약 조건을 만들 수 있도록 하는 특별 한 유형의 제약 조건 대상입니다. 주된 용도 중 하나는 최신 버전의 컨테이너 보기가 상태 표시줄 아래에서 확장 되기 때문에 iOS 6 및 iOS 7 사이에 앱이 호환 되도록 하는 것입니다. 위쪽 레이아웃 가이드에 대 한 자세한 내용은 [Apple 설명서](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/transitionguide/AppearanceCustomization.html#//apple_ref/doc/uid/TP40013174-CH15-SW2)를 참조 하세요.



다음 세 단원에서는 다양 한 유형의 제약 조건을 사용 하는 방법을 소개 합니다.

### <a name="size-constraints"></a>크기 제약 조건

크기 제약 조건- *높이* 및 *너비* 를 사용 하는 경우 두 가지 옵션이 있습니다. 첫 번째 옵션은 위의 예제에서 설명한 것 처럼 핸들을 끌어 인접 요소 크기로 제한 하는 것입니다. 다른 옵션은 핸들을 두 번 클릭 하 여 자체 제약 조건을 만드는 것입니다. 이를 통해 아래 스크린샷에 나와 있는 것 처럼 상수 크기 값을 지정할 수 있습니다.

 [![](designer-auto-layout-images/sizec.png "여기에 나와 있는 것 처럼 핸들을 끌어 인접 요소 크기로 제한 합니다.")](designer-auto-layout-images/sizec.png#lightbox)

### <a name="center-constraints"></a>센터 제약 조건

사각형 핸들은 컨텍스트에 따라 *system.windows.media.rotatetransform.centerx* 또는 *system.windows.media.rotatetransform.centery* 제약 조건을 만듭니다. 사각형 핸들을 끌면 아래 스크린샷에 표시 된 대로 세로 및 가로 끌어 놓기 영역을 모두 제공 하는 다른 요소가 표시 됩니다.

 [![](designer-auto-layout-images/centerc.png "센터 제약 조건")](designer-auto-layout-images/centerc.png#lightbox)

세로 끌어 놓기 영역을 선택 하면 *system.windows.media.rotatetransform.centery* 제약 조건이 생성 됩니다. 수평 끌어 놓기 영역을 선택 하는 경우 제약 조건은 *system.windows.media.rotatetransform.centerx*을 기반으로 합니다.

### <a name="combinational-constraints"></a>Combinational 제약 조건

두 요소 간에 맞춤 및 크기 같음 제약 조건을 모두 만들려면 아래 스크린샷에 표시 된 것 처럼 맨 위 도구 모음에서 항목을 선택 하 여 순서-가로 맞춤, 세로 맞춤 및 크기 equalities 지정할 수 있습니다.

 [![](designer-auto-layout-images/image06.png "Combinational 제약 조건")](designer-auto-layout-images/image06.png#lightbox)

### <a name="visualizing-and-editing-constraints"></a>제약 조건 시각화 및 편집

제약 조건을 추가 하면 항목을 선택할 때 디자인 화면에 파란색 선으로 표시 됩니다.

 [![](designer-auto-layout-images/image09.png "제약 조건 시각화")](designer-auto-layout-images/image09.png#lightbox)

파란색 선을 클릭 하 고 속성 패널에서 직접 제약 조건 값을 편집 하 여 제약 조건을 선택할 수 있습니다. 또는 파란색 선을 두 번 클릭 하면 디자인 화면에서 직접 값을 편집할 수 있는 팝 오버 표시 됩니다.

 [![](designer-auto-layout-images/image08.png "제약 조건 편집")](designer-auto-layout-images/image08.png#lightbox)

## <a name="constraint-issues"></a>제약 조건 문제

제약 조건을 사용 하는 경우 다음과 같은 몇 가지 유형의 문제가 발생할 수 있습니다.

- **충돌 하는 제약 조건** -여러 제약 조건에서 요소에 충돌 하는 특성 값이 적용 되 고 제약 조건 엔진이이를 조정할 수 없는 경우에 발생 합니다.
- 제약 조건이 가장 많은 **항목** -요소의 속성 (위치 + 크기)은 제약 조건 집합 및 제약 조건이 유효 하도록 기본 크기에 완전히 포함 되어야 합니다. 이러한 값이 모호한 경우 항목의 제약을 받는 것으로 간주 됩니다.
- **프레임 잘못 배치** -이는 요소의 프레임과 제약 조건 집합이 서로 다른 두 개의 결과 사각형을 정의 하는 경우에 발생 합니다.


이 섹션에서는 위에 나열 된 세 가지 문제를 대해 중점적 처리 하는 방법에 대 한 세부 정보를 제공 합니다.

### <a name="conflicting-constraints"></a>충돌 하는 제약 조건

충돌 하는 제약 조건은 빨간색으로 표시 되 고 경고 기호가 있습니다. 경고 기호를 마우스로 가리키면 충돌에 대 한 정보가 포함 된 팝 오버이 표시 됩니다.

 [![](designer-auto-layout-images/image11.png "충돌 제약 조건 경고")](designer-auto-layout-images/image11.png#lightbox)

### <a name="underconstrained-items"></a>제약 된 항목 미달

제약 조건이 있는 항목이 주황색으로 표시 되 고 뷰 컨트롤러 개체 모음에서 주황색 표식 아이콘이 표시 됩니다.

 [![](designer-auto-layout-images/image02.png "제약 조건 미달 항목이 주황색으로 표시 됩니다.")](designer-auto-layout-images/image02.png#lightbox)

해당 표식 아이콘을 클릭 하면 아래 스크린샷에 표시 된 것 처럼, 장면에서 미달 제약 된 항목에 대 한 정보를 확인 하 고 완전히 제한 하거나 해당 제약 조건을 제거 하 여 문제를 해결할 수 있습니다.

 [![](designer-auto-layout-images/image10.png "미달 제약 된 항목 수정")](designer-auto-layout-images/image10.png#lightbox)

### <a name="frame-misplacement"></a>프레임 잘못 배치

프레임 잘못 배치는 제약 조건이 있는 항목과 동일한 색 코드를 사용 합니다. 항목은 항상 해당 네이티브 프레임을 사용 하 여 화면에 렌더링 됩니다. 하지만 프레임이 잘못 배치 된 경우에는 아래 스크린샷에 표시 된 것 처럼 응용 프로그램이 실행 될 때 항목이 종료 되는 위치에 빨간색 사각형이 표시 됩니다.

 [![](designer-auto-layout-images/image05.png "샘플 프레임 잘못 배치 뷰")](designer-auto-layout-images/image05.png#lightbox)

프레임 잘못 배치 오류를 해결 하려면 제약 조건 도구 모음 (맨 오른쪽 단추)에서 **제약 조건에 따라 프레임 업데이트** 단추를 선택 합니다.

 [![](designer-auto-layout-images/image03.png "제약 조건에 따라 프레임 업데이트 도구 모음 단추")](designer-auto-layout-images/image03.png#lightbox)

그러면 컨트롤에 정의 된 위치와 일치 하도록 요소 프레임이 자동으로 조정 됩니다.

<a name="modifying-in-code" />

## <a name="modifying-constraints-in-code"></a>코드의 제약 조건 수정

앱의 요구 사항에 따라 코드에서 제약 조건을 수정 해야 하는 경우가 있을 수 있습니다. 예를 들어 제약 조건이 연결 된 뷰의 크기를 조정 하거나 위치를 변경 하는 경우 제약 조건의 우선 순위를 변경 하거나 제약 조건을 모두 비활성화 합니다.

코드의 제약 조건에 액세스 하려면 먼저 다음을 수행 하 여 iOS 디자이너에서이를 노출 해야 합니다.

1. 위에 나열 된 방법 중 하나를 사용 하 여 제약 조건을 정상적으로 만듭니다.
2. **문서 개요 탐색기**에서 원하는 제약 조건을 찾고 선택 합니다.

    [![](designer-auto-layout-images/modify01.png "문서 개요 탐색기")](designer-auto-layout-images/modify01.png#lightbox)
3. 그런 다음 **속성 탐색기**의 **위젯** 탭에서 제약 조건에 **이름을** 할당 합니다.

    [![](designer-auto-layout-images/modify02.png "위젯 탭")](designer-auto-layout-images/modify02.png#lightbox)
4. 변경 내용을 저장합니다.

위의 변경 내용을 적용 하면 코드에서 제약 조건에 액세스 하 고 해당 속성을 수정할 수 있습니다. 예를 들어 다음을 사용 하 여 연결 된 보기의 높이를 0으로 설정할 수 있습니다.

```csharp
ViewInfoHeight.Constant = 0;
```

IOS 디자이너에서 제약 조건에 대해 다음과 같은 설정이 제공 됩니다.

[![](designer-auto-layout-images/modify03.png "속성 탐색기에서 제약 조건 편집")](designer-auto-layout-images/modify03.png#lightbox)

### <a name="the-deferred-layout-pass"></a>지연 된 레이아웃 패스

제약 조건 변경에 대 한 응답으로 연결 된 보기를 즉시 업데이트 하는 대신 자동 레이아웃 엔진은 가까운 장래에 _지연 된 레이아웃 패스_ 를 예약 합니다. 이 지연 된 단계 중에는 지정 된 뷰의 제약 조건이 업데이트 되는 것 뿐만 아니라 계층의 모든 뷰에 대 한 제약 조건이 다시 계산 되어 새 레이아웃에 맞게 조정 됩니다.

언제 든 지 부모 뷰의 또는 `SetNeedsLayout` `SetNeedsUpdateConstraints` 메서드를 호출 하 여 사용자 고유의 지연 된 레이아웃 패스를 예약할 수 있습니다. 

지연 된 레이아웃 패스는 뷰 계층 구조를 통과 하는 두 개의 고유한 패스로 구성 됩니다.

- **업데이트 통과** -이 패스에서 자동 레이아웃 엔진은 뷰 계층 구조를 트래버스하 고 모든 뷰 컨트롤러 `UpdateViewConstraints` 및 `UpdateConstraints` 모든 뷰의 메서드에서 메서드를 호출 합니다.
- **레이아웃이 다시 전달** 됩니다. 자동 레이아웃 엔진은 뷰 계층 구조를 순회 하지만 이번에는 모든 뷰 `ViewWillLayoutSubviews` 컨트롤러에서 메서드를 호출 하 고 `LayoutSubviews` 모든 뷰의 메서드를 호출 합니다. 메서드 `LayoutSubviews` 는 자동 레이아웃 `Frame` 엔진에서 계산 되는 사각형을 사용 하 여 각 하위 뷰의 속성을 업데이트 합니다.

### <a name="animating-constraint-changes"></a>제약 조건 변경 내용 적용

제약 조건 속성을 수정 하는 것 외에도 핵심 애니메이션을 사용 하 여 뷰의 제약 조건에 대 한 변경 내용을 적용할 수 있습니다. 예를 들어:

```csharp
UIView.BeginAnimations("OpenInfo");
UIView.SetAnimationDuration(1.0f);
ViewInfoHeight.Constant = 237;
View.LayoutIfNeeded();

//Execute Animation
UIView.CommitAnimations();
```

여기서 키는 애니메이션 블록 내 `LayoutIfNeeded` 에서 부모 뷰의 메서드를 호출 하는 것입니다. 이렇게 하면 애니메이션 된 위치나 크기 변화에 대 한 각 "프레임"을 그리도록 뷰에 알려 줍니다. 이 줄이 없으면 뷰가 애니메이션 효과 없이 최종 버전에만 맞춰집니다.

## <a name="summary"></a>요약

이 가이드에서는 iOS 자동 (또는 "적응") 레이아웃과 제약 조건의 개념이 디자인 화면에서 요소 간의 관계에 대 한 수학적 표현으로 도입 되었습니다. IOS 디자이너에서 자동 레이아웃을 사용 하도록 설정 하 고, **제약 조건 도구 모음**을 사용 하 고, 디자인 화면에서 개별적으로 제약 조건을 편집 하는 방법에 대해 설명 했습니다. 다음에는 세 가지 일반적인 제약 조건 문제를 해결 하는 방법을 설명 했습니다. 마지막으로 코드에서 제약 조건을 수정 하는 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [Storyboards 소개](~/ios/user-interface/storyboards/index.md)
- [iOS 디자인 가능한 컨트롤 연습](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Android Designer 개요](~/android/user-interface/android-designer/index.md)
- [프로그래밍 제약 조건](~/ios/user-interface/programmatic-layout-constraints.md)
- [Apple-자동 레이아웃 가이드](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/AutolayoutPG/Introduction/Introduction.html#/apple_ref/doc/uid/TP40010853-CH13-SW1)
