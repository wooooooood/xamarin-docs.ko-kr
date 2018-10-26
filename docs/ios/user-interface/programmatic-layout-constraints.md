---
title: Xamarin.iOS에서 프로그래밍 레이아웃 제약 조건
description: 이 가이드에서는 제공 iOS 사용 하 여 작업에 자동 레이아웃 제약 조건 C# iOS 디자이너에서에서 만드는 대신 코드입니다.
ms.prod: xamarin
ms.assetid: 119C8365-B470-4CD4-85F7-086F0A46DCBB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 3d8e69af7f790415343abf464ea2bb22e879e025
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106963"
---
# <a name="programmatic-layout-constraints-in-xamarinios"></a>Xamarin.iOS에서 프로그래밍 레이아웃 제약 조건

_이 가이드에서는 제공 iOS 사용 하 여 작업에 자동 레이아웃 제약 조건 C# iOS 디자이너에서에서 만드는 대신 코드입니다._

반응 형 디자인 방식은 자동 레이아웃 ("적응 레이아웃"이 라고도 함)입니다. 에 대 한 자동 레이아웃은 전환 레이아웃 시스템에 있는 각 요소의 위치를 화면에서 지점으로 하드 코딩 된 달리 *관계* -디자인 화면의 다른 요소를 기준으로 요소의 위치입니다. 자동 레이아웃의 핵심은 제약 조건 또는 화면의 다른 요소의 컨텍스트에서 요소 배치 또는 요소 집합을 정의 하는 규칙의 개념입니다. 요소 화면에서 특정 위치에 고정 되지 않은, 때문에 다양 한 화면 크기 및 장치 방향에서 보기 좋게 적응 레이아웃을 만들 제약 조건 도움말입니다.

일반적으로 자동 레이아웃 ios에서에서 작업할 때 사용 하 여 iOS 디자이너 UI 항목에 대 한 레이아웃 제약 조건을 그래픽으로 배치 합니다. 그러나 경우도 만들고 제약 조건을 적용 해야 할 때 C# 코드입니다. 예를 들어 동적으로 사용 하 여 만들 때 추가 UI 요소를 `UIView`입니다.

이 가이드를 만들고 사용 하 여 제약 조건으로 작업 하는 방법을 보여는 C# iOS 디자이너에서에서 그래픽 방식으로 만드는 대신 코드입니다.

<a name="Creating-Constraints-Programmatically" />

## <a name="creating-constraints-programmatically"></a>제약 조건을 프로그래밍 방식으로 만들기

위에서 설명한 대로 일반적으로에서는 사용 하 게 자동 레이아웃 제약 조건을 사용 하 여 iOS 디자이너. 제약 조건을 프로그래밍 방식으로 만들 필요가 있는 해당 시간에 세 가지 옵션을 선택 해야 합니다.

* [레이아웃 앵커](#Layout-Anchors) -이 API는 앵커 속성에 대 한 액세스를 제공 (같은 `TopAnchor`를 `BottomAnchor` 또는 `HeightAnchor`) 제한 되 고 UI 항목의 합니다.
* [레이아웃 제약 조건](#Layout-Constraints) -제약 조건을 사용 하 여 직접 만들 수 있습니다는 `NSLayoutConstraint` 클래스입니다.
* [시각적 서식 지정 언어](#Visual-Format-Language) -ASCII 정교한 작업에서 제약 조건을 정의 하는 방법 등을 제공 합니다.

다음 섹션에서는 각 옵션을 자세히 살펴봅니다.

<a name="Layout-Anchors" />

### <a name="layout-anchors"></a>레이아웃 앵커

사용 하 여는 `NSLayoutAnchor` 제한 되 고 UI 항목의 앵커 속성을 기반으로 하는 제약 조건을 만들기 위한 흐름 인터페이스 있는 클래스입니다. 예를 들어 뷰 컨트롤러의 위쪽과 아래쪽 레이아웃 안내선 노출 합니다 `TopAnchor`, `BottomAnchor` 및 `HeightAnchor` 뷰 가장자리, 가운데, 크기 및 기준 속성을 표시 하는 동안 속성을 고정 합니다.

> [!IMPORTANT]
> 앵커 속성을 표준 집합 외에도 iOS 보기도 포함 합니다 `LayoutMarginsGuides` 및 `ReadableContentGuide` 속성입니다. 이러한 속성을 노출 `UILayoutGuide` 개체 뷰의 여백 및 읽을 수 있는 작업에 대 한 가이드를 각각 콘텐츠입니다.

레이아웃 앵커 읽기 쉽게 하 고 간단한 형식 제약 조건을 만들기 위한 여러 가지 방법을 제공 합니다.

- **ConstraintEqualTo** -관계를 정의 합니다. 여기서 `first attribute = second attribute + [constant]` 필요에 따라 제공 된 `constant` 오프셋 값입니다.
- **ConstraintGreaterThanOrEqualTo** -관계를 정의 합니다. 여기서 `first attribute >= second attribute + [constant]` 필요에 따라 제공 된 `constant` 오프셋 값입니다.
- **ConstraintLessThanOrEqualTo** -관계를 정의 합니다. 여기서 `first attribute <= second attribute + [constant]` 필요에 따라 제공 된 `constant` 오프셋 값입니다.

예를 들어:

```csharp
// Get the parent view's layout
var margins = View.LayoutMarginsGuide;

// Pin the leading edge of the view to the margin
OrangeView.LeadingAnchor.ConstraintEqualTo (margins.LeadingAnchor).Active = true;

// Pin the trailing edge of the view to the margin
OrangeView.TrailingAnchor.ConstraintEqualTo (margins.TrailingAnchor).Active = true;

// Give the view a 1:2 aspect ratio
OrangeView.HeightAnchor.ConstraintEqualTo (OrangeView.WidthAnchor, 2.0f);
```

일반적인 레이아웃 제약 조건은 선형 식으로 간단히 표현할 수 있습니다. 다음 예제를 참조하세요.

[![](programmatic-layout-constraints-images/graph01.png "선형 식으로 표현 되는 레이아웃 제약 조건")](programmatic-layout-constraints-images/graph01.png#lightbox)

다음 줄으로 변환 됩니다는 C# 레이아웃 앵커를 사용 하 여 코드:

```csharp
PurpleView.LeadingAnchor.ConstraintEqualTo (OrangeView.TrailingAnchor, 10).Active = true; 
```

여기서의 일부는 C# 코드 부분에 해당 하는 지정 된 수식의 같이:

|수식|코드|
|---|---|
|항목 1|PurpleView|
|특성 1|LeadingAnchor|
|Relationship|ConstraintEqualTo|
|승수|따라서 지정 되지 않은 1.0 기본값|
|항목 2|OrangeView|
|특성 2|TrailingAnchor|
|상수|10.0|

지정 된 레이아웃 제약 조건 수식을 해결 하는 데 필요한 매개 변수만 제공 하는 것 외에도 각 레이아웃 앵커 메서드에 전달 된 매개 변수의 형식 안전성을 적용 합니다. 따라서 가로 제약 조건 같은 앵커 `LeadingAnchor` 또는 `TrailingAnchor` 에서만 사용할 수 있습니다 다른 가로 앵커를 사용 하 여 형식 및 승수만 제공 크기 제약 조건입니다.

<a name="Layout-Constraints" />

### <a name="layout-constraints"></a>레이아웃 제약 조건

직접 생성 하 여 자동 레이아웃 제약을 수동으로 추가할 수 있습니다는 `NSLayoutConstraint` 에서 C# 코드입니다. 레이아웃 앵커를 사용 하 여 달리 정의 되는 제약 조건에 영향을 주지는 해당 하는 경우에 모든 매개 변수에 대해 값을 지정 해야 합니다. 결과적으로, 하면 결국 상당한 읽기가 상용구 코드를 생성 합니다. 예를 들어:

```csharp
//// Pin the leading edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Leading, NSLayoutRelation.Equal, View, NSLayoutAttribute.LeadingMargin, 1.0f, 0.0f).Active = true;

//// Pin the trailing edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Trailing, NSLayoutRelation.Equal, View, NSLayoutAttribute.TrailingMargin, 1.0f, 0.0f).Active = true;

//// Give the view a 1:2 aspect ratio
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Height, NSLayoutRelation.Equal, OrangeView, NSLayoutAttribute.Width, 2.0f, 0.0f).Active = true;
```

여기서는 `NSLayoutAttribute` enum 뷰의 여백 값을 정의 하 고 해당 하는 `LayoutMarginsGuide` 와 같은 속성 `Left`, `Right`를 `Top` 및 `Bottom` 및 `NSLayoutRelation` 관계를 정의 하는 열거형은 으로 지정 된 특성 간의 만들어집니다 `Equal`, `LessThanOrEqual` 또는 `GreaterThanOrEqual`합니다.

와 달리 레이아웃 앵커 API를 사용 하 여는 `NSLayoutConstraint` 생성 방법에 특정 제약 조건의 중요 한 측면을 강조 표시 되지 않습니다 되며 컴파일되지 않은 제약 조건에서 수행 되는 검사 시간입니다. 결과적으로, 런타임 시 예외가 발생 하는 잘못 된 제약 조건을 구성 하기 쉽습니다.

<a name="Visual-Format-Language" />

### <a name="visual-format-language"></a>시각적 형식으로 언어

Visual 형식의 언어를 사용 하면 ASCII 아트가 만들어지는 제약 조건의 시각적 표현을 제공 하는 문자열 등을 사용 하 여 제약 조건을 정의할 수 있습니다. 여기에 다음과 같은 장점 및 단점이 있습니다.

- 시각적 언어 형식을 유효한 제약 조건만 만들을 적용합니다.
 - 자동 레이아웃 제약 조건에 디버그 메시지는 코드와 유사 합니다 제약 조건을 만드는 데 Visual 언어 형식을 사용 하 여 콘솔에 출력 합니다.
 - Visual 형식의 언어를 사용 하면 매우 간단한 식 사용 하 여 동시에 여러 제약 조건을 만들 수 있습니다.
 - 시각적 형식 언어 문자열에 대 한 컴파일 쪽 유효성 이므로 문제 런타임에 검색할 수 있습니다.
 - 시각적 언어 형식을 완결성을 통해 시각화 강조 되므로 (예: 비율) 된 일부 제약 조건 형식에 만들 수 없습니다.

제약 조건을 만들려면 비주얼 형식 언어를 사용 하는 경우에 다음 단계를 수행 합니다.

1. 만들기는 `NSDictionary` 개체 보기 및 레이아웃 안내선 형식을 정의할 때 사용할 문자열 키를 포함 하는 합니다.
2. 필요에 따라 만듭니다는 `NSDictionary` 키와 값의 집합을 정의 하는 (`NSNumber`) 제약 조건에 대 한 상수 값으로 사용 합니다.
3. 단일 열 또는 행 항목을 레이아웃에 형식 문자열을 만듭니다.
4. 호출을 `FromVisualFormat` 메서드는 `NSLayoutConstraint` 제약 조건을 생성 하는 클래스입니다.
5. 호출을 `ActivateConstraints` 메서드는 `NSLayoutConstraint` 활성화 제약 조건을 적용 하는 클래스입니다.

예를 들어, 선행 및 후행 제약 조건을 둘 다 Visual 형식 언어에서를 만들려면 사용할 수 있습니다 다음:

```csharp
// Get views being constrained
var views = new NSMutableDictionary (); 
views.Add (new NSString ("orangeView"), OrangeView);

// Define format and assemble constraints
var format = "|-[orangeView]-|";
var constraints = NSLayoutConstraint.FromVisualFormat (format, NSLayoutFormatOptions.AlignAllTop, null, views);

// Apply constraints
NSLayoutConstraint.ActivateConstraints (constraints);
```

Visual 언어 형식을 기본 간격을 사용 하는 경우 부모 보기의 여백으로 연결 된 0 개 지점 제약 조건 항상 만들어지므로이 코드는 위의 예와 동일한 결과 생성 합니다.

단일 줄에 여러 자식 뷰 같은 더 복잡 한 UI 디자인에 대 한 시각적 언어 형식을 가로 간격과 세로 맞춤을 지정합니다. 위의 예와 여기서 지정 된 `AlignAllTop` `NSLayoutFormatOptions` 의 모든 행 또는 열을 해당 위쪽에서 보기를 맞춥니다.

Apple의를 참조 하세요 [비주얼 형식 언어 부록](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1) Visual 형식의 문자열 문법 및 일반적인 제약 조건을 지정 하는 몇 가지 예입니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 가이드는 제약 조건을 자동 레이아웃을 만들고 작업 표시 C# iOS 디자이너에서에서 그래픽 방식으로 만드는 대신 합니다. 첫째,이 레이아웃 앵커를 사용 하 여 확인할 수 (`NSLayoutAnchor`) 자동 레이아웃을 처리 하도록 합니다. 다음으로, 레이아웃 제약 조건을 사용 하는 방법에 알아보았습니다 (`NSLayoutConstraint`). 마지막으로, 자동 레이아웃에 대 한 비주얼 형식 언어를 사용 하 여 표시 합니다.

## <a name="related-links"></a>관련 링크

- [Storyboards 소개](~/ios/user-interface/storyboards/index.md)
- [iOS 디자인할 수 있는 컨트롤 연습](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [IOS 용 Xamarin 디자이너를 사용 하 여 자동 레이아웃](~/ios/user-interface/designer/designer-auto-layout.md#modifying-in-code)
- [Apple-프로그래밍 방식으로 제약 조건 만들기](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html#//apple_ref/doc/uid/TP40010853-CH16-SW1)
- [Apple-시각적 형식으로 언어 부록](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1)
