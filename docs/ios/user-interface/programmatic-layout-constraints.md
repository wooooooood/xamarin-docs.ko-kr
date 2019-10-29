---
title: Xamarin.ios의 프로그래밍 레이아웃 제약 조건
description: 이 가이드에서는 ios 디자이너에서 iOS 자동 레이아웃 제약 C# 조건을 만드는 대신 코드에서 작업을 수행 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 119C8365-B470-4CD4-85F7-086F0A46DCBB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 2ec012f882d6bc721e657385db333fce7a9e1aaf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73002186"
---
# <a name="programmatic-layout-constraints-in-xamarinios"></a>Xamarin.ios의 프로그래밍 레이아웃 제약 조건

_이 가이드에서는 ios 디자이너에서 iOS 자동 레이아웃 제약 C# 조건을 만드는 대신 코드에서 작업을 수행 하는 방법을 보여 줍니다._

자동 레이아웃 ("적응 레이아웃"이 라고도 함)은 응답성이 뛰어난 디자인 방법입니다. 각 요소의 위치가 화면의 점으로 하드 코딩 되는 전환 레이아웃 시스템과 달리 자동 레이아웃은 디자인 화면에서 다른 요소를 기준으로 하는 요소의 위치와 *관계* 에 대 한 것입니다. 자동 레이아웃의 핵심은 화면에 있는 다른 요소의 컨텍스트에서 요소나 요소의 집합을 정의 하는 제약 조건이 나 규칙의 개념입니다. 요소가 화면에서 특정 위치에 연결 되어 있지 않기 때문에 제약 조건을 통해 다양 한 화면 크기 및 장치 방향에 적합 한 적응 레이아웃을 만들 수 있습니다.

일반적으로 iOS에서 자동 레이아웃으로 작업할 때 iOS 디자이너를 사용 하 여 UI 항목에 레이아웃 제약 조건을 그래픽으로 배치 합니다. 그러나 코드에서 C# 제약 조건을 만들고 적용 해야 하는 경우가 있을 수 있습니다. 예를 들어 동적으로 생성 된 UI 요소를 사용 하는 경우 `UIView`에 추가 됩니다.

이 가이드에서는 iOS 디자이너에서 그래픽을 만드는 대신 코드를 사용 C# 하 여 제약 조건을 만들고 작업 하는 방법을 보여 줍니다.

<a name="Creating-Constraints-Programmatically" />

## <a name="creating-constraints-programmatically"></a>프로그래밍 방식으로 제약 조건 만들기

위에서 설명한 것 처럼 일반적으로 iOS 디자이너에서 자동 레이아웃 제약 조건을 사용 합니다. 제약 조건을 프로그래밍 방식으로 만들어야 하는 경우 다음 세 가지 옵션 중에서 선택할 수 있습니다.

- [레이아웃 앵커](#Layout-Anchors) -이 API는 제한 되는 UI 항목의 앵커 속성 (예: `TopAnchor`, `BottomAnchor` 또는 `HeightAnchor`)에 대 한 액세스를 제공 합니다.
- [레이아웃 제약 조건](#Layout-Constraints) -`NSLayoutConstraint` 클래스를 사용 하 여 직접 제약 조건을 만들 수 있습니다.
- [시각적 서식 지정 언어](#Visual-Format-Language) -제약 조건을 정의 하는 메서드와 같은 ASCII 아트를 제공 합니다.

다음 섹션에서는 각 옵션을 자세히 설명 합니다.

<a name="Layout-Anchors" />

### <a name="layout-anchors"></a>레이아웃 앵커

`NSLayoutAnchor` 클래스를 사용 하 여 제한 되는 UI 항목의 앵커 속성을 기반으로 제약 조건을 만들기 위한 흐름 인터페이스가 있습니다. 예를 들어 뷰 컨트롤러의 위쪽 및 아래쪽 레이아웃 안내선은 `TopAnchor`, `BottomAnchor` 및 `HeightAnchor` 앵커 속성을 노출 하는 반면 뷰는 가장자리, 가운데, 크기 및 기준 속성을 노출 합니다.

> [!IMPORTANT]
> 표준 앵커 속성 집합 외에도 iOS 보기에는 `LayoutMarginsGuides` 및 `ReadableContentGuide` 속성이 포함 됩니다. 이러한 속성은 보기의 여백 및 읽을 수 있는 콘텐츠 가이드를 각각 사용 하기 위한 `UILayoutGuide` 개체를 노출 합니다.

레이아웃 앵커는 읽기 쉬운 간단한 형식으로 제약 조건을 만들 수 있는 몇 가지 메서드를 제공 합니다.

- **ConstraintEqualTo** -선택적으로 제공 되는 `constant` 오프셋 값을 사용 하 여 `first attribute = second attribute + [constant]` 하는 관계를 정의 합니다.
- **ConstraintGreaterThanOrEqualTo** -선택적으로 제공 되는 `constant` 오프셋 값을 사용 하 여 `first attribute >= second attribute + [constant]` 하는 관계를 정의 합니다.
- **ConstraintLessThanOrEqualTo** -선택적으로 제공 되는 `constant` 오프셋 값을 사용 하 여 `first attribute <= second attribute + [constant]` 하는 관계를 정의 합니다.

예를 들면,

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

일반적인 레이아웃 제약 조건은 단지 선형 식으로 표현 될 수 있습니다. 다음 예제를 참조하세요.

[![](programmatic-layout-constraints-images/graph01.png "A Layout Constraint expressed as a linear expression")](programmatic-layout-constraints-images/graph01.png#lightbox)

레이아웃 앵커를 사용 하 여 다음 C# 코드 줄로 변환 됩니다.

```csharp
PurpleView.LeadingAnchor.ConstraintEqualTo (OrangeView.TrailingAnchor, 10).Active = true; 
```

여기서 C# 코드 부분은 다음과 같이 수식의 지정 된 부분에 해당 합니다.

|인식|코드|
|---|---|
|항목 1|PurpleView|
|특성 1|LeadingAnchor|
|Relationship|ConstraintEqualTo|
|곱한|기본값은 1.0 이므로 지정 하지 않음|
|항목 2|OrangeView|
|특성 2|TrailingAnchor|
|상수|10.0|

지정 된 레이아웃 제약 조건 수식을 해결 하는 데 필요한 매개 변수만 제공 하는 것 외에도 각 레이아웃 앵커 메서드는 전달 된 매개 변수의 형식 안전성을 적용 합니다. 따라서 `LeadingAnchor` 또는 `TrailingAnchor`와 같은 가로 제약 조건 앵커는 다른 수평선 앵커 유형에만 사용할 수 있으며, 승수는 크기 제약 조건에만 제공 됩니다.

<a name="Layout-Constraints" />

### <a name="layout-constraints"></a>레이아웃 제약 조건

코드에서 C# `NSLayoutConstraint`를 직접 구성 하 여 자동 레이아웃 제약 조건을 수동으로 추가할 수 있습니다. 레이아웃 앵커를 사용 하는 것과 달리 정의 되는 제약 조건에 영향을 주지 않는 경우에도 모든 매개 변수에 대 한 값을 지정 해야 합니다. 결과적으로 상당한 양의 코드를 읽을 수 있는 상용구 코드를 생성 하 게 됩니다. 예를 들면,

```csharp
//// Pin the leading edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Leading, NSLayoutRelation.Equal, View, NSLayoutAttribute.LeadingMargin, 1.0f, 0.0f).Active = true;

//// Pin the trailing edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Trailing, NSLayoutRelation.Equal, View, NSLayoutAttribute.TrailingMargin, 1.0f, 0.0f).Active = true;

//// Give the view a 1:2 aspect ratio
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Height, NSLayoutRelation.Equal, OrangeView, NSLayoutAttribute.Width, 2.0f, 0.0f).Active = true;
```

여기서 `NSLayoutAttribute` 열거형은 뷰의 여백 값을 정의 하 고 `Left`, `Right`, `Top`, `Bottom` 등의 `LayoutMarginsGuide` 속성에 해당 하며, `NSLayoutRelation` 열거형은 지정 된 특성 간에 생성 되는 관계를 정의 합니다. `Equal`, `LessThanOrEqual` 또는 `GreaterThanOrEqual`합니다.

레이아웃 앵커 API와 달리 `NSLayoutConstraint` 만들기 메서드는 특정 제약 조건의 중요 한 측면을 강조 하지 않으며 제약 조건에 대해 수행 되는 컴파일 시간 검사는 없습니다. 따라서 런타임에 예외를 throw 하는 잘못 된 제약 조건을 쉽게 생성할 수 있습니다.

<a name="Visual-Format-Language" />

### <a name="visual-format-language"></a>시각적 서식 언어

시각적 서식 언어를 사용 하면 생성 되는 제약 조건에 대 한 시각적 표현을 제공 하는 문자열과 같은 ASCII 아트를 사용 하 여 제약 조건을 정의할 수 있습니다. 여기에는 다음과 같은 장점과 단점이 있습니다.

- 비주얼 서식 언어는 유효한 제약 조건만 만들도록 적용 합니다.
- 자동 레이아웃은 비주얼 서식 언어를 사용 하 여 콘솔에 제약 조건을 출력 하므로 디버깅 메시지는 제약 조건을 만드는 데 사용 되는 코드와 유사 합니다.
- 시각적 서식 언어를 사용 하면 매우 간결한 식으로 여러 제약 조건을 동시에 만들 수 있습니다.
- 시각적 서식 언어 문자열의 컴파일 쪽 유효성 검사는 없으므로 런타임에만 문제를 검색할 수 있습니다.
- 시각적 서식 언어는 완전 한 시각화를 강조 하므로 일부 제약 조건 형식 (예: 비율)을 만들 수 없습니다.

시각적 서식 언어를 사용 하 여 제약 조건을 만드는 경우 다음 단계를 수행 합니다.

1. 뷰 개체 및 레이아웃 안내선과 형식을 정의할 때 사용할 문자열 키를 포함 하는 `NSDictionary`를 만듭니다.
2. 필요에 따라 제약 조건에 대 한 상수 값으로 사용 되는 키 및 값 (`NSNumber`) 집합을 정의 하는 `NSDictionary`을 만듭니다.
3. 단일 열 또는 항목 행을 레이아웃 하는 형식 문자열을 만듭니다.
4. `NSLayoutConstraint` 클래스의 `FromVisualFormat` 메서드를 호출 하 여 제약 조건을 생성 합니다.
5. `NSLayoutConstraint` 클래스의 `ActivateConstraints` 메서드를 호출 하 여 제약 조건을 활성화 하 고 적용 합니다.

예를 들어, 시각적 서식 언어에서 선행 및 후행 제약 조건을 모두 만들려면 다음을 사용할 수 있습니다.

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

시각적 서식 언어는 기본 간격을 사용할 때 항상 부모 뷰의 여백에 연결 된 0 포인트 제약 조건을 만들기 때문에이 코드는 위에 표시 된 예제와 동일한 결과를 생성 합니다.

한 줄에 여러 자식 보기와 같은 보다 복잡 한 UI 디자인의 경우 시각적 서식 언어는 가로 간격과 세로 맞춤을 모두 지정 합니다. 위의 예제와 같이 `AlignAllTop`를 지정 하 `NSLayoutFormatOptions`는 행 또는 열의 모든 뷰를 위쪽에 맞춥니다.

일반 제약 조건 및 시각적 서식 문자열 문법을 지정 하는 몇 가지 예는 Apple의 [시각적 형식 언어 부록](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1) 을 참조 하세요.

<a name="Summary" />

## <a name="summary"></a>요약

이 가이드에서는 iOS 디자이너에서 그래픽을 그래픽으로 만드는 C# 것과는 반대로에서 자동 레이아웃 제약 조건을 만들고 사용 하는 방법을 설명 했습니다. 먼저 레이아웃 앵커 (`NSLayoutAnchor`)를 사용 하 여 자동 레이아웃을 처리 하는 방법을 살펴보았습니다. 다음으로`NSLayoutConstraint`(레이아웃 제약 조건)을 사용 하는 방법을 살펴보았습니다. 마지막으로, 자동 레이아웃에 대 한 시각적 서식 언어를 사용 하 여 표시 됩니다.

## <a name="related-links"></a>관련 링크

- [Storyboards 소개](~/ios/user-interface/storyboards/index.md)
- [iOS 디자인 가능한 컨트롤 연습](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Xamarin Designer for iOS 자동 레이아웃](~/ios/user-interface/designer/designer-auto-layout.md#modifying-in-code)
- [Apple-프로그래밍 방식으로 제약 조건 만들기](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html#//apple_ref/doc/uid/TP40010853-CH16-SW1)
- [Apple-Visual Format 언어 부록](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1)
