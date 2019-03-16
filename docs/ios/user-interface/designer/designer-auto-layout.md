---
title: IOS 용 Xamarin 디자이너를 사용 하 여 자동 레이아웃
description: 이 가이드는 iOS 자동 레이아웃을 소개 하 고 디자이너를 사용 하 여 Xamarin iOS 용 만들고 제약 조건을 사용 하 여 레이아웃을 편집 하는 방법을 설명 합니다. 또한 코드에서 제약 조건 변경 등을 애니메이션 수정 제약 조건을 설명 합니다.
ms.prod: xamarin
ms.assetid: CAC7A715-55BB-45E2-BB6D-2168D36D428F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 41a9ec90b4b734dde7a982ac3d4b2e7b2082321c
ms.sourcegitcommit: 650458de1d362cd7de174cacef7838f0e74426f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2019
ms.locfileid: "58070868"
---
# <a name="auto-layout-with-the-xamarin-designer-for-ios"></a>IOS 용 Xamarin 디자이너를 사용 하 여 자동 레이아웃

반응 형 디자인 방식은 자동 레이아웃 ("적응 레이아웃"이 라고도 함)입니다. 에 대 한 자동 레이아웃은 전환 레이아웃 시스템에 있는 각 요소의 위치를 화면에서 지점으로 하드 코딩 된 달리 *관계* -디자인 화면의 다른 요소를 기준으로 요소의 위치입니다. 자동 레이아웃의 핵심은 제약 조건 또는 화면의 다른 요소의 컨텍스트에서 요소 배치 또는 요소 집합을 정의 하는 규칙의 개념입니다. 요소 화면에서 특정 위치에 고정 되지 않은, 때문에 다양 한 화면 크기 및 장치 방향에서 보기 좋게 적응 레이아웃을 만들 제약 조건 도움말입니다.

이 가이드에서는 제약 조건 및 Xamarin iOS 디자이너에서에서 작업 하는 방법을 소개 하겠습니다. 이 가이드 제약 조건을 프로그래밍 방식으로 작업을 다루지 않습니다. 프로그래밍 방식으로 자동 레이아웃 사용에 대 한 내용은 참조는 [Apple 설명서](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html)합니다.

## <a name="requirements"></a>요구 사항

IOS 용 Xamarin 디자이너는 Visual Studio 2017 및 나중에 Windows에서 Mac 용 Visual Studio에서 사용할 수 있습니다.

이 가이드에서 디자이너의 구성 요소에 대 한 지식이 있다고 가정 합니다 [iOS 디자이너 소개](~/ios/user-interface/designer/introduction.md) 가이드입니다.

## <a name="introduction-to-constraints"></a>제약 조건 소개

제약 조건에는 화면에 두 요소 간 관계의 수학적 표현입니다. UI를 나타내는 요소의 위치가 수학적 관계를 하드 코딩 된 UI 요소의 위치와 관련 된 몇 가지 문제를 해결 합니다. 예를 들어, 화면의 맨 아래 에서부터 단추 20px를 세로 모드로 전환 되었으면 단추의 위치 가로 모드로 화면에서 벗어났는지 것입니다. 이 방지 하려면 뷰의 아래쪽 단추 20px의 아래쪽 가장자리를 배치 하는 제약 조건을 설정 수 없습니다. 단추에 지 위치로 계산 될 것 *button.bottom view.bottom-20px =*, 세로 및 가로 모드에서 보기의 맨 아래 에서부터 단추 20px을 배치 하는 합니다. 수학적 관계를 기반으로 하는 배치를 계산 하는 기능은 점에서 제약 조건이 있으므로 유용 UI 디자인에서입니다.

제약 조건, 설정 때 만듭니다는 `NSLayoutConstraint` 제한 되도록 하는 개체 및 속성을 인수로 사용 하는 개체 또는 *특성*, 제약 조건에서 작동할 합니다. IOS 디자이너에서 특성에는 가장자리와 같은 합니다 *왼쪽*, *오른쪽*, *위쪽*, 및 *아래쪽* 요소입니다. 이러한 특성을 포함할 수도 크기와 같은 *높이* 하 고 *너비*, 및 위치를 가리키고 센터 *centerX* 및 *centerY*합니다. 예를 들어, 두 단추의 왼쪽된 경계 위치에 대 한 제약 조건에 추가 했습니다 디자이너 내부적으로 다음 코드를 생성 합니다.

```csharp
View.AddConstraint (NSLayoutConstraint.Create (Button1, NSLayoutAttribute.Left, NSLayoutRelation.Equal, Button2, NSLayoutAttribute.Left, 1, 10));
```

다음 섹션에서는 iOS 디자이너를 사용 하도록 설정 하 고 자동 레이아웃을 사용 하지 않도록 설정, 제약 조건 도구 모음을 사용 하 여 등을 사용 하 여 제약 조건 사용 하 여 작업을 설명 합니다.

## <a name="enable-auto-layout"></a>자동 레이아웃 사용 하도록 설정

기본 iOS 디자이너 구성에 제약 조건 모드를 사용 합니다. 그러나 해야 할 사용 하도록 설정 하거나 수동으로 비활성화할 두 단계로 수행할 수 있습니다.

1.  디자인 화면의 빈 영역을 클릭 합니다. 이 요소를 취소 하 고 스토리 보드 문서에 대 한 속성을 표시 합니다.
1.  선택 또는 선택 취소 합니다 **사용 하 여 자동 레이아웃** 속성 패널에서 확인란을 선택:

    ![](designer-auto-layout-images/image01.png "속성 패널에서 확인란을 사용 하 여 자동 레이아웃")


기본적으로 제약 조건이 없는 만들거나 화면에 표시 됩니다. 대신 컴파일 시에 프레임 정보에서 자동으로 유추 됩니다. 제약 조건에 추가 하려면 디자인 화면에서 요소를 선택 하 고 제약 조건을 추가 해야 합니다. 사용 하 여 해당 하는 것이 가능 합니다 **제약 조건 도구 모음**합니다.

## <a name="constraints-toolbar"></a>제약 조건 도구 모음

 [![](designer-auto-layout-images/toolbarnew.png "상황에 맞는 메뉴 명령")](designer-auto-layout-images/toolbarnew.png#lightbox)

제약 조건 도구 모음 업데이트 되었습니다 및 이제 두 주요 부분으로 구성 됩니다.

- **제약 조건 모드 단추 토글**: 이전에 디자인 화면에서 선택한 뷰를 다시 클릭 하 여 제약 조건 모드를 시작 해야 합니다. 제약 조건 표시줄에서이 토글 단추를 사용 하면 이제 됩니다.

  ![제약 조건 모드 설정/해제](designer-auto-layout-images/constraints.png)

- **"업데이트 제약 조건" 단추를 합니다.** 경우에 따라 변경 하는 제약 조건 편집 모드에에서 두는 것이 반드시 합니다.
  - 제약 조건 편집 모드에서이 단추 요소 프레임에 맞게 제약 조건을 조정 합니다.
  - 프레임 편집 모드에서에서이 단추 요소 프레임 제약 조건을 정의 하는 위치와 일치 하도록 조정 합니다.


## <a name="surface-based-constraint-editing"></a>화면 기반 제약 조건 편집

이전 섹션에서 기본 제약 조건을 추가 제약 조건 도구 모음을 사용 하 여 제약 조건을 제거를 알게 되었습니다. 더 세밀 하 게 조정할된 제약 조건 편집을 위해 디자인 화면에서 직접 제약 조건 조작할 수 있습니다. 이 섹션에서는 화면 기반 제약 조건을 편집, 고정 간격 컨트롤, 끌어 놓기 영역 및 다른 유형의 제약 조건 사용 기본 사항을 소개 합니다.

### <a name="creating-constraints"></a>제약 조건 만들기

IOS 디자이너 도구는 두 종류의 디자인 화면에서 요소를 조작 하기 위한 컨트롤을 제공 합니다. *컨트롤을 끌어서 놓는* 하 고 *pin 간격 컨트롤*와 다음 이미지와 같이:

![뷰 컨트롤](designer-auto-layout-images/controls.png)

이러한 제약 조건 표시줄에 제약 조건 모드 단추를 선택 하 여 설정/해제 됩니다.

요소의 양쪽에 4 T 모양의 핸들을 정의 합니다 *위쪽*를 *오른쪽*를 *아래쪽*, 및 *왼쪽* 요소의 가장자리를 제약 조건입니다. 요소의 아래쪽 및 오른쪽에서 두 개의 I-모양 핸들 정의 *높이* 하 고 *너비* 제약 조건 각각. 중간 사각형을 모두 다룹니다 *centerX* 하 고 *centerY* 제약 조건입니다.

제약 조건을 만들려면 선택 핸들을 디자인 화면의 어딘가에 끕니다. 끌기 시작 하면 일련의 녹색 줄/상자 무엇을 알리는 화면에 표시 됩니다 제한할 수 있습니다. 예를 들어 아래 스크린샷에서 위쪽 가운데 단추 제한 했습니다.

 [![](designer-auto-layout-images/image07.png "위쪽 가운데 단추를 제한합니다.")](designer-auto-layout-images/image07.png#lightbox)

다른 두 개의 단추에서 세 가지 녹색 파선 note 합니다. 녹색 선은 나타냅니다 *끌어 놓기 영역*, 또는에서는 제한할 수는 다른 요소의 특성입니다. 다른 두 개의 단추 위의 스크린 샷에서 3 세로 끌어 놓기 영역을 제공 합니다 ( *아래쪽*, *centerY*, *위쪽*) 단추가 제한. 보기의 맨 위에 있는 녹색 파선 뷰 컨트롤러는 보기의 맨 위에 있는 제약 조건을 제공 하 고 단색 녹색 상자 뷰 컨트롤러 위쪽 레이아웃 안내선 아래 제약 조건을 제공 의미를 의미 합니다.

> [!IMPORTANT]
> 레이아웃 안내선은 특수 한 유형의 상위를 만들도록 허용 하는 제약 조건 대상 및 상태 표시줄에서 도구 모음 등의 시스템 막대의 존재를 고려해 야 하는 아래쪽 제약 조건입니다. 최신 버전은 아래 상태 표시줄 확장 컨테이너 보기에 있으므로 iOS 6 및 iOS 7 간에 호환 되는 앱이 있는 주요 용도 중 하나입니다. 위쪽 레이아웃 안내선에 대 한 자세한 내용은 참조는 [Apple 설명서](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/transitionguide/AppearanceCustomization.html#//apple_ref/doc/uid/TP40013174-CH15-SW2)합니다.



다음 세 섹션 다양 한 유형의 제약 조건 사용 하 여 작업을 소개합니다.

### <a name="size-constraints"></a>크기 제약 조건

크기 제약 조건-를 사용 하 여 *높이* 하 고 *너비* -두 가지 옵션이 있습니다. 위의 예제에서와 같이 인접 요소 크기를 제한에 대 한 핸들을 끌어 첫 번째 옵션이입니다. 다른 옵션은 자체-제약 조건을 만들려면 핸들을 두 번 클릭입니다. 이렇게 하면 아래 스크린샷에 나온 것 처럼 상수 크기 값을 지정 하기.

 [![](designer-auto-layout-images/sizec.png "다음과 같이 인접 요소 크기를 제한 하려면 핸들을 끌어")](designer-auto-layout-images/sizec.png#lightbox)

### <a name="center-constraints"></a>Center 제약 조건

정사각형 핸들을 만듭니다는 *centerX* 또는 *centerY* 컨텍스트에 따라 제약 조건입니다. 정사각형 핸들을 끌어가 켜 집니다 다른 요소를 아래 스크린샷에서 나온 것 처럼 두 세로 및 가로 끌어 놓기 영역을 제공 합니다.

 [![](designer-auto-layout-images/centerc.png "Center 제약 조건")](designer-auto-layout-images/centerc.png#lightbox)

세로 끌어 놓기 영역을 선택한 경우는 *centerY* 제약 조건이 만들어집니다. 가로 끌어 놓기 영역을 선택 하면 제약 조건에 따라 달라 집니다 *centerX*합니다.

### <a name="combinational-constraints"></a>Combinational 제약 조건

맞춤 및 두 요소 사이의 크기 일치 제약 조건을 만들려면 아래 스크린샷에 나온 것 처럼-순서로-가로 맞춤, 세로 맞춤 및 크기 같은지 여부를 지정 하려면 최상위 도구 모음에서 항목 선택할 수 있습니다.

 [![](designer-auto-layout-images/image06.png "Combinational 제약 조건")](designer-auto-layout-images/image06.png#lightbox)

### <a name="visualizing-and-editing-constraints"></a>시각화 및 제약 조건 편집

제약 조건에 추가 하면 표시 됩니다 디자인 화면에서 파란색 선으로 항목을 선택 합니다.

 [![](designer-auto-layout-images/image09.png "시각화 제약 조건")](designer-auto-layout-images/image09.png#lightbox)

파란색 선을 클릭 하 고 [속성] 패널에서 직접 제약 조건 값을 편집 하 여 제약 조건을 선택할 수 있습니다. 또는 파란색 선을 두 번 클릭 하면 디자인 화면에서 직접 값을 편집할 수 있도록를 팝 오버 표시 됩니다.

 [![](designer-auto-layout-images/image08.png "제약 조건 편집")](designer-auto-layout-images/image08.png#lightbox)

## <a name="constraint-issues"></a>제약 조건 문제

여러 유형의 문제는 제약 조건을 사용 하는 경우에 발생할 수 있습니다.

-  **충돌 하는 제약 조건** -여러 제약 조건을 강제로 특성의 값이 충돌 하는 요소 및 제약 조건 엔진 조정 수 없는 경우에 발생 합니다.
-  **항목 underconstrained** -요소의 속성 (위치 + 크기) 제약 조건 및 유효한 제약 조건에 대 한 내장 크기 집합이 완전히 적용 되어야 합니다. 이러한 값이 모호한 경우 underconstrained 될 항목 이라고 합니다.
-  **Misplacement 프레임** -이 요소의 프레임 및 제약 조건 집합이 두 개의 서로 다른 결과 사각형을 정의 하는 경우에 발생 합니다.


이 섹션에서는 위에 나열 된 세 가지 문제에 설명 하 고 처리 하는 방법에 자세히 설명 합니다.

### <a name="conflicting-constraints"></a>충돌 하는 제약 조건

충돌 하는 제약 조건 빨간색으로 표시 되 고 경고 기호가 있어야 합니다. 경고 기호를 마우스로 가리키면 충돌에 대 한 정보를 팝 오버 나타납니다.

 [![](designer-auto-layout-images/image11.png "제약 조건이 충돌 경고")](designer-auto-layout-images/image11.png#lightbox)

### <a name="underconstrained-items"></a>Underconstrained 항목

Underconstrained 항목 주황색으로 표시 및 보기 컨트롤러 개체 막대에서 주황색 표식 아이콘의 모양을 트리거:

 [![](designer-auto-layout-images/image02.png "Underconstrained 항목 주황색으로 표시")](designer-auto-layout-images/image02.png#lightbox)

표식 아이콘을 클릭 하면 경우 장면에서 underconstrained 항목에 대 한 정보를 얻을 수 있으며 아래 스크린샷에 나온 것 처럼 해당 제약 조건을 제거 하거나 두 완벽 하 게 제한 하 여 문제를 해결 합니다.

 [![](designer-auto-layout-images/image10.png "Underconstrained 항목을 수정")](designer-auto-layout-images/image10.png#lightbox)

### <a name="frame-misplacement"></a>프레임 Misplacement

프레임 misplacement underconstrained 항목으로 같은 색상 코드를 사용합니다. 항목이 항상 해당 네이티브 프레임을 사용 하 여 화면에 렌더링할 하지만 프레임 misplacement 경우 빨간색 직사각형 위치 항목 결국 응용 프로그램을 실행 하면 아래 스크린샷에 나온 것 처럼 표시 됩니다.

 [![](designer-auto-layout-images/image05.png "샘플 프레임 Misplacement 보기")](designer-auto-layout-images/image05.png#lightbox)

프레임 misplacement 오류를 해결 하려면 선택 합니다 **제약 조건에 따라 프레임을 업데이트** (맨 오른쪽 단추) 제약 조건 도구 모음에서 단추:

 [![](designer-auto-layout-images/image03.png "제약 조건 도구 모음 단추에 따라 프레임 업데이트")](designer-auto-layout-images/image03.png#lightbox)

이 컨트롤에 의해 정의 된 위치에 맞게 요소 프레임 자동으로 조정 합니다.

<a name="modifying-in-code" />

## <a name="modifying-constraints-in-code"></a>코드에서 제약 조건 수정

앱의 요구 사항에 따라 경우가 있을 코드에서 제약 조건을 수정 해야 합니다. 예를 들어, 크기를 조정 하거나 뷰 위치 제약 조건에 연결 됩니다에 제약 조건 우선 순위를 변경 하거나 완전히 제약 조건을 비활성화.

코드에서 제약 조건에 액세스 하려면 먼저 다음을 수행 하 여 iOS 디자이너에에서 노출 해야 합니다.

1. (위에 나열 된 방법 중 하나 사용) 일반적으로 제약 조건을 만듭니다.
2. 에 **문서 개요 탐색**, 원하는 제약 조건 키를 찾아 선택 합니다.

    [![](designer-auto-layout-images/modify01.png "문서 개요 탐색")](designer-auto-layout-images/modify01.png#lightbox)
3. 다음으로, 할당을 **이름** 에서 제약 조건에는 **위젯** 탭의 **속성 탐색기**:

    [![](designer-auto-layout-images/modify02.png "위젯 탭")](designer-auto-layout-images/modify02.png#lightbox)
4. 변경 내용을 저장합니다.

위의 변경 내용을 준비에서를 사용 하 여 코드에서 제약 조건을 액세스할 수 있으며 해당 속성을 수정할 키를 누릅니다. 예를 들어 0 연결 된 뷰의 높이 설정 하려면 다음을 사용할 수 있습니다.

```csharp
ViewInfoHeight.Constant = 0;
```

IOS 디자이너에에서는 제약 조건에 대해 다음 설정을 지정:

[![](designer-auto-layout-images/modify03.png "속성 탐색기에서 제약 조건 편집")](designer-auto-layout-images/modify03.png#lightbox)

### <a name="the-deferred-layout-pass"></a>지연 된 레이아웃 단계

제약 조건 변경에 대 한 응답에서 연결 된 뷰를 즉시 업데이트 하는 대신 자동 레이아웃 엔진 예약을 _지연 된 레이아웃 단계_ 가까운 미래에 대 한 합니다. 이 지연 단계에서 뿐만 아니라입니다 지정한 보기의 제약 조건 업데이트 제약 조건을 계층의 모든 뷰에 다시 계산 되 고 새 레이아웃에 맞게 업데이트 됩니다.

언제 든 지 호출 하 여 사용자 고유의 지연 된 레이아웃 단계를 예약할 수는 `SetNeedsLayout` 또는 `SetNeedsUpdateConstraints` 부모 보기 메서드. 

지연 된 레이아웃 단계 계층 구조 보기를 통해 두 개의 고유 전달 이루어져 있습니다.

- **Update 통과** -이 단계에서는 자동 레이아웃 엔진 뷰 계층 구조를 트래버스하 고 호출를 `UpdateViewConstraints` 모든 뷰 컨트롤러 메서드 및 `UpdateConstraints` 모든 보기에는 메서드.
- **레이아웃 단계** -다시 자동 레이아웃 엔진 뷰 계층 구조를 통과 하지만이 이번을 호출 하는 `ViewWillLayoutSubviews` 모든 뷰 컨트롤러 메서드 및 `LayoutSubviews` 모든 보기에는 메서드. `LayoutSubviews` 메서드가 업데이트는 `Frame` 자동 레이아웃 엔진에서 계산 된 사각형을 사용 하 여 각 하위 보기의 속성입니다.

### <a name="animating-constraint-changes"></a>제약 조건 변경에 애니메이션 적용

제약 조건 속성을 수정 하는 것 외에도 보기의 제약 조건에 대 한 변경을 애니메이션화 하는 핵심 애니메이션을 사용할 수 있습니다. 예를 들어:

```csharp
UIView.BeginAnimations("OpenInfo");
UIView.SetAnimationDuration(1.0f);
ViewInfoHeight.Constant = 237;
View.LayoutIfNeeded();

//Execute Animation
UIView.CommitAnimations();
```

이 키를 호출 하는 것은 `LayoutIfNeeded` 애니메이션 블록 내에서 부모 뷰의 메서드. 이 애니메이션이 적용 된 위치 또는 크기 변경의 각 "frame" 그릴를 뷰에 알려 줍니다. 이 줄이 없는 뷰는 단순히 최종 버전으로 애니메이션 효과 적용 하지 않고 맞춤 됩니다.

## <a name="summary"></a>요약

이 가이드에서는 iOS 자동 (또는 "adaptive") 소개 했습니다. 레이아웃 및 디자인 화면에서 요소 간의 관계의 수학적 표현으로 제약 조건의 개념입니다. 자동 레이아웃 사용 하 여 iOS 디자이너를 사용 하도록 설정 하는 방법을 설명 합니다 **제약 조건 도구 모음**, 및 제약 조건 디자인 화면에서 개별적으로 편집 합니다. 그런 다음 세 가지 일반적인 제약 조건 문제를 해결 하는 방법을 설명 했습니다. 마지막으로 코드에서 제약 조건을 수정 하는 방법에 알아보았습니다.

## <a name="related-links"></a>관련 링크

- [Storyboards 소개](~/ios/user-interface/storyboards/index.md)
- [iOS 디자인할 수 있는 컨트롤 연습](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Android 디자이너 개요](~/android/user-interface/android-designer/index.md)
- [프로그래밍 방식으로 제약 조건](~/ios/user-interface/programmatic-layout-constraints.md)
- [Apple-자동 레이아웃 안내선](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/AutolayoutPG/Introduction/Introduction.html#/apple_ref/doc/uid/TP40010853-CH13-SW1)
