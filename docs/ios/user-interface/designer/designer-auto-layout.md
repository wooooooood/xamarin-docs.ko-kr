---
title: "IOS 용 Xamarin 디자이너로 자동 레이아웃"
description: "이 가이드에서는 iOS 자동 레이아웃 및 iOS에 대 한 Xamarin 디자이너에서 사용할 수 있는 새 제약 조건 워크플로 소개합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: CAC7A715-55BB-45E2-BB6D-2168D36D428F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 040a5979339ed12f212f932f3b7e51cf48a9d382
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="auto-layout-with-the-xamarin-designer-for-ios"></a>IOS 용 Xamarin 디자이너로 자동 레이아웃

_이 가이드에서는 iOS 자동 레이아웃 및 iOS에 대 한 Xamarin 디자이너에서 사용할 수 있는 새 제약 조건 워크플로 소개합니다._

자동 레이아웃 ("레이아웃 adaptive" 라고도 함)는 반응 형 디자인 방법입니다. 여기서 각 요소의 위치에는 화면에서 지점으로 하드 코딩, 전환 레이아웃 시스템에서와 달리 자동 레이아웃에 대 한는 *관계* -디자인 화면에서 다른 요소를 기준으로 요소의 위치입니다. 자동 레이아웃의 핵심 개념의 제약 조건 또는 규칙의 다른 요소를 화면에 컨텍스트에서 요소 배치 또는 요소 집합을 정의 하는 경우 요소 화면에서 특정 위치에 고정 되지 않은, 때문에 제약 조건을 다양 한 화면 크기와 방향 장치에서 제대로 보이는 적응 레이아웃을 만들 수 있습니다.

이 가이드에서는 제약 조건 및 Xamarin iOS 디자이너에서에서 사용 하는 방법을 소개 합니다. 이 가이드 제약 조건 작업에 프로그래밍 방식으로 다루지 않습니다. 자동 레이아웃을 프로그래밍 방식으로 사용 하는 방법은 참조는 [Apple 설명서](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html)합니다.

## <a name="requirements"></a>요구 사항

IOS 용 Xamarin 디자이너는 Windows에서 Visual Studio 2015 및 2017 Mac 용 Visual Studio에서 사용할 수 있습니다.

이 가이드에서는 디자이너의 구성 요소에서 지식이 있다고 가정은 [iOS 디자이너 소개](~/ios/user-interface/designer/introduction.md) 가이드입니다.

## <a name="introduction-to-constraints"></a>제약 조건에는 소개

제약 조건은 화면에 두 요소 사이의 관계에 대 한 수학적 표현을 합니다. 수학적 관계와 요소의 위치는 UI를 나타내는 하드 코딩 된 UI 요소의 위치와 관련 된 몇 가지 문제를 해결 합니다. 예를 들어 세로 모드의 화면 하단에서 단추 20px를 배치할 수 있었습니다 단추의 위치 가로 모드로 화면 밖 것입니다. 이 방지 하려면 보기의 아래쪽에서 단추 20px의 아래쪽 가장자리를 배치 하는 제약 조건을 설정 수 없습니다. 단추에 지 위치도 계산 될 것 *button.bottom view.bottom-20px =*, 가로 세로 모드에서 배치에서 보기의 아래쪽 단추 20px는 것입니다. 수학적 관계를 기준으로 배치를 계산 하는는 유용 제약 조건이 있으므로 UI 디자인에 합니다.

제약 조건, 설정할 때 만듭니다는 `NSLayoutConstraint` 제한 되는 개체 및 속성을 인수로 사용 하는 개체 또는 *특성*, 제약 조건 작업을 수행 하는 합니다. IOS 디자이너에서 특성에는 가장자리와 같은 *왼쪽*, *오른쪽*, *top*, 및 *아래쪽* 요소입니다. 이러한 특성을 포함할 수도 크기와 같은 *높이* 및 *너비*, 및 지점 위치, 센터 *centerX* 및 *centerY*합니다. 예를 들어, 두 단추의 왼쪽된 경계 위치에 대 한 제약 조건을 추가할 때 디자이너 내부적 아래에 다음 코드 생성:

```csharp
View.AddConstraint (NSLayoutConstraint.Create (Button1, NSLayoutAttribute.Left, NSLayoutRelation.Equal, Button2, NSLayoutAttribute.Left, 1, 10));
```

다음 섹션에서는 iOS 활성화 및 자동 레이아웃을 비활성화 및 제약 조건 도구 모음을 사용 하 여 포함 하 여 디자이너를 사용 하 여 제약 조건으로 작업을 설명 합니다.

## <a name="enable-auto-layout"></a>자동 레이아웃을 사용 하도록 설정

기본 iOS 디자이너 구성에 제약 조건 모드를 사용할 수 있습니다. 그러나 필요한 경우에 수동으로 해제 하거나 설정 하려면 다음 두 단계에서는 그렇게 할 수 있습니다.

1.  디자인 화면의 빈 영역을 클릭 합니다. 이 요소를 모두를 취소 하 고 스토리 보드 문서에 대 한 속성을 제공 합니다.
1.  선택 하거나 선택 취소할는 **사용 하 여 자동 레이아웃** 속성 패널에 확인란을 선택 합니다.

    ![](designer-auto-layout-images/image01.png "속성 패널에서 사용 하 여 자동 레이아웃 확인란")


기본적으로 제약 조건이 없으면는 만들거나 화면에 표시 합니다. 대신, 컴파일 타임에 프레임 정보에서 자동으로 유추 됩니다. 제약 조건을 추가할 하 디자인 화면에 요소를 선택 하 고 하 여 제약 조건을 추가 해야 합니다. 사용 하 여 수행할 수 있습니다는 **제약 조건 도구 모음**합니다.

## <a name="constraints-toolbar"></a>제약 조건 도구 모음

 [ ![](designer-auto-layout-images/toolbarnew.png "상황에 맞는 메뉴 명령")](designer-auto-layout-images/toolbarnew.png)

제약 조건 도구 모음 업데이트 되었습니다 및 이제 두 개의 주요 부분으로 구성 됩니다.

- **제약 조건 모드 단추 토글**: 이전에, 제약 조건 모드 다시 디자인 화면에서 선택한 보기를 클릭 하 여 입력 합니다. 이제이 토글 단추를 사용 하 여 제약 조건 표시줄에 해야:

  ![제약 조건 모드 설정/해제](designer-auto-layout-images/constraints.png)

- **"업데이트 제약 조건" 단추:** 제약 조건 편집 모드에에서는 방식에 따라 변경 해야 합니다.
  - 이 단추는 제약 조건 편집 모드에 요소 프레임 일치 하도록 제약 조건을 조정 합니다.
  - 편집 모드 프레임이이 단추는 제약 조건을 정의 하는 위치와 일치 하도록 요소 프레임을 조정 합니다.


## <a name="surface-based-constraint-editing"></a>화면 기반 제약 조건을 편집

이전 섹션에서 이전에 배운 것을 기본 제약 조건을 추가 하 고 제약 조건을 도구 모음을 사용 하 여 제약 조건을 제거 합니다. 더 세밀 하 게 제약 조건을 편집에 대 한 디자인 화면에서 직접 제약 조건으로 상호 작용할 수 있습니다. 이 섹션에서는 핀 간격 컨트롤, 끌어 놓기 영역 및 작업에 다양 한 유형의 제약 조건 포함 하 여 화면 기반 제약 조건을 편집 기본 사항을 소개 합니다.

### <a name="creating-constraints"></a>제약 조건 만들기

IOS 디자이너 도구는 두 종류의 디자인 화면에서 요소를 조작 하기 위한 컨트롤을 제공 합니다. *컨트롤을 끌어* 및 *핀 간격 컨트롤*다음 그림에 표시 된 것 처럼,:

![뷰 컨트롤](designer-auto-layout-images/controls.png)

이러한 제약 조건 표시줄에 제약 조건을 모드 단추를 선택 하 여 설정/해제 됩니다.

요소의 양쪽에 있는 4 T 모양의 핸들 정의 *top*, *오른쪽*, *아래쪽*, 및 *왼쪽* 가장자리에 대 한 요소에는 제약 조건입니다. 요소의 아래쪽과 오른쪽에 있는 두 개의 I-모양 핸들 정의 *높이* 및 *너비* 제약 조건 각각. 중간 제곱 둘 다를 처리 *centerX* 및 *centerY* 제약 조건입니다.

제약 조건을 만들려면, 선택 핸들을 디자인 화면에서 어딘가에 끕니다. 일련의 녹색 줄/상자 내용을 알리는 화면에 표시 됩니다 끌기 시작 하면을 제한할 수 있습니다. 예를 들어 아래 스크린샷에서 위쪽, 가운데 단추 제한 했습니다.

 [ ![](designer-auto-layout-images/image07.png "위쪽, 가운데 단추를 제한합니다.")](designer-auto-layout-images/image07.png)

나머지 두 단추에서 세 가지 녹색 파선 note 합니다. 녹색 선은 나타냅니다 *끌어 놓기 영역*, 또는 특성의 다른 요소를 제한할 수 있습니다. 또한 위의 스크린 샷에서 나머지 두 단추 3 세로 끌어 놓기 영역을 제공 ( *아래쪽*, *centerY*, *top*)이 단추를 제한 하 합니다. 뷰의 맨 위에 있는 녹색 파선 뷰 컨트롤러 보기의 맨 위쪽에는 제약 조건을 제공 하 고 단색 녹색 상자 뷰 컨트롤러 최상위 레이아웃 가이드 아래 제약 조건을 제공 의미를 의미 합니다.

> [!IMPORTANT]
> **참고**: 레이아웃 안내선은 특수 한 유형의 시스템 모음, 상태 표시줄에서 도구 모음 등의 존재 여부를 고려 하는 위쪽 및 아래쪽 제약 조건을 만들 수 있는 제약 조건 대상으로 합니다. 주요 용도 중 하나는 최신 버전은 상태 표시줄 아래로 확장 하는 컨테이너 보기에 있으므로 iOS 6 및 iOS 7 간에 호환 되는 응용 프로그램을 것입니다. 최상위 레이아웃 가이드에 대 한 자세한 내용은 참조는 [Apple 설명서](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/transitionguide/AppearanceCustomization.html#//apple_ref/doc/uid/TP40013174-CH15-SW2)합니다.



다음 3 개의 섹션에는 다양 한 유형의 제약 조건 사용을 소개합니다.

### <a name="size-constraints"></a>크기 제약 조건

크기 제약 조건- *높이* 및 *너비* -두 가지 옵션이 있습니다. 위의 예제에서와 같이 인접 한 항목 요소 크기를 제한에 대 한 핸들을 끌어 첫 번째 옵션이입니다. 다른 옵션은 자체 제약 조건을 만들려면 핸들을 두 번 클릭 합니다. 이렇게 하면 아래 스크린샷에서 예와 같이 상수 크기 값을 지정 하면:

 [ ![](designer-auto-layout-images/sizec.png "다음과 같이 인접 한 항목 요소 크기를 제한 하려면 핸들을 끌어")](designer-auto-layout-images/sizec.png)

### <a name="center-constraints"></a>Center 제약 조건

정사각형 핸들을 만듭니다는 *centerX* 또는 *centerY* 컨텍스트에 따라 제약 조건입니다. 정사각형 핸들을 끌어 아래 스크린샷에서 같이 두 세로 및 가로 끌어 놓기 영역을 제공 하도록 다른 요소를 켜 집니다.

 [ ![](designer-auto-layout-images/centerc.png "Center 제약 조건")](designer-auto-layout-images/centerc.png)

세로 끌어 놓기 영역을 선택 하는 경우는 *centerY* 제약 조건이 생성 됩니다. 제약 조건에 따라 달라 집니다 가로 끌어 놓기 영역을 선택 하면 *centerX*합니다.

### <a name="combinational-constraints"></a>Combinational 제약 조건

맞춤 및 두 요소 사이의 크기 같음 제약 조건을 모두를 만들려면 아래 스크린샷에서 표시 된 것 처럼-순서로-가로 맞춤, 세로 맞춤 및 크기 같은지 여부를 지정 하려면 맨 위의 도구 모음에서 항목을 선택할 수 있습니다.

 [ ![](designer-auto-layout-images/image06.png "Combinational 제약 조건")](designer-auto-layout-images/image06.png)

### <a name="visualizing-and-editing-constraints"></a>시각화 및 제약 조건 편집

제약 조건을 추가 하면 표시 됩니다 디자인 화면에는 파란색 선으로 항목을 선택 합니다.

 [ ![](designer-auto-layout-images/image09.png "제약 조건 시각화")](designer-auto-layout-images/image09.png)

제약 조건에 파란색 선을 클릭 하 고 속성 패널에서 직접 제약 조건 값을 편집 하 여 선택할 수 있습니다. 또는 파란색 선이 두 번 클릭 하면 디자인 화면에서 직접 값을 편집할 수 있는 popover 표시 됩니다.

 [ ![](designer-auto-layout-images/image08.png "제약 조건 편집")](designer-auto-layout-images/image08.png)

## <a name="constraint-issues"></a>제약 조건 문제

여러 가지 유형의 문제는 제약 조건을 사용 하는 경우에 발생할 수 있습니다.

-  **제약 조건이 충돌** -여러 개의 제약 조건을 강제로 요소의 특성에 대 한 값이 충돌 및 제약 조건 엔진 조정 수 없는 경우 이러한 문제가 발생 합니다.
-  **항목 underconstrained** -요소 속성 (위치 + 크기) 제약 조건 및 유효 하려면 제약 조건에 대 한 기본 크기는 집합이 한 완전히 포함 되어야 합니다. 이러한 값은 모호 하는 경우 underconstrained 될 항목 이라고 합니다.
-  **Misplacement 프레임** -이 요소의 프레임 및 제약 조건 집합이 두 개의 서로 다른 결과 사각형을 정의할 때 발생 합니다.


이 섹션 위에 나열 된 세 가지 문제를 설명 하 고 처리 하는 방법에 자세히 설명 합니다.

### <a name="conflicting-constraints"></a>충돌 하는 제약 조건

제약 조건이 충돌 빨간색으로 표시 되 고 경고 기호. 마우스로 가리키면 경고 기호 충돌 하는 방법에 대 한 정보로 popover를 나타납니다.

 [ ![](designer-auto-layout-images/image11.png "제약 조건이 충돌 경고")](designer-auto-layout-images/image11.png)

### <a name="underconstrained-items"></a>Underconstrained 항목

Underconstrained 항목 주황색으로 표시 되어 있으며 보기 컨트롤러 개체 막대에서 주황색 표식 아이콘의 모양 트리거.

 [ ![](designer-auto-layout-images/image02.png "주황색으로 표시 되어 underconstrained 항목")](designer-auto-layout-images/image02.png)

해당 마커 아이콘을 클릭할 경우 장면의 underconstrained 항목에 대 한 정보를 가져올 수 있고 아래 스크린샷에서 표시 된 것 처럼 하거나 완전 하 게 제한 하 여 나 해당 제약 조건을 제거 하 여 문제를 해결 됩니다.

 [ ![](designer-auto-layout-images/image10.png "Underconstrained 항목 수정")](designer-auto-layout-images/image10.png)

### <a name="frame-misplacement"></a>프레임 Misplacement

프레임 misplacement underconstrained 항목으로 같은 색상 코드를 사용합니다. 항목이 항상 해당 네이티브 프레임을 사용 하 여 화면에 렌더링할 하지만 프레임 misplacement의 경우 빨간색 직사각형 위치 항목 결국 응용 프로그램이 실행 되 면 아래 스크린샷에서 표시 된 것 처럼 표시 됩니다.

 [ ![](designer-auto-layout-images/image05.png "샘플 프레임 Misplacement 보기")](designer-auto-layout-images/image05.png)

프레임 misplacement 오류를 해결 하려면 선택은 **업데이트 프레임 제약 조건에 따라** 제약 조건 (맨 오른쪽 단추) 도구 모음에서 단추:

 [ ![](designer-auto-layout-images/image03.png "도구 모음 단추 제약 조건에 따라 업데이트 프레임")](designer-auto-layout-images/image03.png)

컨트롤에 의해 정의 된 위치와 일치 하도록 요소 프레임 자동으로 조정 합니다.

<a name="modifying-in-code" />

## <a name="modifying-constraints-in-code"></a>코드에서 제약 조건 수정

응용 프로그램의 요구에 따라, 경우도 코드에서 제약 조건을 수정 하는 경우가 있습니다. 예를 들어 크기나 위치 보기를 변경 하는 제약 조건에 연결 됩니다, 제약 조건을 우선 순위를 변경 하거나 완전히 제약 조건 비활성화.

코드에서 제약 조건에 액세스 하려면 먼저 다음을 수행 하 여 iOS 디자이너에에서 노출 해야 합니다.

1. (사용 하 여 위에 나열 된 방법 중 하나) 일반적인 방법으로 제약 조건을 만듭니다.
2. 에 **문서 개요 탐색기**원하는 제약 조건을 찾아 선택 합니다.

    [ ![](designer-auto-layout-images/modify01.png "문서 개요 탐색기")](designer-auto-layout-images/modify01.png)
3. 그런 다음 할당할는 **이름** 의 제약 조건으로는 **위젯** 탭은 **속성 탐색기**:

    [ ![](designer-auto-layout-images/modify02.png "위젯 탭")](designer-auto-layout-images/modify02.png)
4. 변경 내용을 저장합니다.

위치에 위의 변경과 코드에서 제약 조건을 액세스 하 고 해당 속성을 수정할 수 있습니다. 예를 들어 0 연결 된 보기의 높이 설정 하려면 다음을 사용할 수 있습니다.

```csharp
ViewInfoHeight.Constant = 0;
```

IOS 디자이너에에서 제약 조건에 대 한 다음과 같은 설정을 가정합니다.

[ ![](designer-auto-layout-images/modify03.png "제약 조건 속성 탐색기에서 편집")](designer-auto-layout-images/modify03.png)

### <a name="the-deferred-layout-pass"></a>지연 된 레이아웃 단계

제약 조건 변경 사항에 따라 연결 된 보기를 즉시 업데이트 하는 대신 자동 레이아웃 엔진 예약는 _지연 된 레이아웃 단계_ 가까운 미래에 대 한 합니다. 이 지연 된 단계 뿐만 아니라입니다 지정된 된 보기의 제약 조건 업데이트 제약 조건을 계층의 모든 보기 다시 계산 하 고 새 레이아웃에 맞게 업데이트 합니다.

언제 든 고유한 지연 된 레이아웃 단계를 호출 하 여 예약할 수 있습니다는 `SetNeedsLayout` 또는 `SetNeedsUpdateConstraints` 부모 보기의 메서드. 

지연 된 레이아웃 단계 계층 구조 보기를 통해 두 개의 고유 전달 이루어져 있습니다.

- **업데이트 패스** -이 단계에서는 자동 레이아웃 엔진 보기 계층 구조를 순회 하 고 호출는 `UpdateViewConstraints` 모든 컨트롤러의 보기에는 메서드 및 `UpdateConstraints` 모든 뷰에서도 메서드.
- **레이아웃 단계** -다시 자동 레이아웃 엔진 뷰 계층 구조를 통과 하지만이 이번 호출는 `ViewWillLayoutSubviews` 모든 컨트롤러의 보기에는 메서드 및 `LayoutSubviews` 모든 뷰에서도 메서드. `LayoutSubviews` 메서드가 업데이트는 `Frame` 자동 레이아웃 엔진에 의해 속성의 각 서브 사각형을 계산 합니다.

### <a name="animating-constraint-changes"></a>제약 조건이 변경 애니메이션 적용

제약 조건 속성을 수정 하는 것 외에도 변경 내용 보기의 제약 조건에 애니메이션 효과를 주는 핵심 애니메이션을 사용할 수 있습니다. 예:

```csharp
UIView.BeginAnimations("OpenInfo");
UIView.SetAnimationDuration(1.0f);
ViewInfoHeight.Constant = 237;
View.LayoutIfNeeded();

//Execute Animation
UIView.CommitAnimations();
```

여기에서 키를 호출 하는 `LayoutIfNeeded` 애니메이션 블록 내부는 부모 뷰의 메서드. 애니메이션 효과 준 위치나 크기 변경의 각 "frame" 그릴 보기를 인지를 나타냅니다. 이 줄이 없는 뷰는에 간단히 맞아 최종 버전으로 애니메이션을 적용 하지 않고 있습니다.

## <a name="summary"></a>요약

이 가이드에는 iOS 자동 (또는 "adaptive") 도입 된 레이아웃 및 제약 조건 디자인 화면에서 요소 간의 관계의 수학 표시로의 개념입니다. 작업 iOS 디자이너에서 자동 레이아웃을 사용 하도록 설정 하는 방법을 설명 하기는 **제약 조건 도구 모음**, 및 제약 조건을 개별적으로 디자인 화면에서 편집 합니다. 그런 다음 세 가지 일반적인 제약 조건을 문제를 해결 하는 방법을 설명 했습니다. 마지막으로, 코드에서 제약 조건을 수정 하는 방법에 알아보았습니다.

## <a name="related-links"></a>관련 링크

- [스토리 보드에는 소개](~/ios/user-interface/storyboards/index.md)
- [iOS 디자인할 수 있는 컨트롤 연습](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Android 디자이너 개요](~/android/user-interface/android-designer/index.md)
- [프로그래밍 방식으로 제약 조건](~/ios/user-interface/programmatic-layout-constraints.md)
- [Apple-자동 레이아웃 가이드](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/AutolayoutPG/Introduction/Introduction.html#/apple_ref/doc/uid/TP40010853-CH13-SW1)
