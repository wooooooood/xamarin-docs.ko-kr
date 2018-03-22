---
title: "프로그래밍 방식으로 레이아웃 제약 조건"
description: "이 가이드 iOS 디자이너에에서 새로 만드는 대신 C# 코드에서 iOS 작업 자동 레이아웃 제약 조건을 표시 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 119C8365-B470-4CD4-85F7-086F0A46DCBB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 774d6e6ecdb081650c6f008b1ac83c397f788d5b
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="programmatic-layout-constraints"></a>프로그래밍 방식으로 레이아웃 제약 조건

_이 가이드 iOS 디자이너에에서 새로 만드는 대신 C# 코드에서 iOS 작업 자동 레이아웃 제약 조건을 표시 합니다._

자동 레이아웃 ("레이아웃 adaptive" 라고도 함)는 반응 형 디자인 방법입니다. 여기서 각 요소의 위치에는 화면에서 지점으로 하드 코딩, 전환 레이아웃 시스템에서와 달리 자동 레이아웃에 대 한는 *관계* -디자인 화면에서 다른 요소를 기준으로 요소의 위치입니다. 자동 레이아웃의 핵심 개념의 제약 조건 또는 규칙의 다른 요소를 화면에 컨텍스트에서 요소 배치 또는 요소 집합을 정의 하는 경우 요소 화면에서 특정 위치에 고정 되지 않은, 때문에 제약 조건을 다양 한 화면 크기와 방향 장치에서 제대로 보이는 적응 레이아웃을 만들 수 있습니다.

일반적으로 사용할 때 자동 레이아웃에서 iOS를 사용 하 여 iOS 디자이너 UI 항목의 레이아웃 제약 조건을 그래픽으로 배치 합니다. 그러나를 만들고 C# 코드에서 제약 조건을 적용 해야 하는 경우 시간이 있을 수 있습니다. 예를 들어 동적으로 사용 하 여 만들 때에 추가 하는 UI 요소는 `UIView`합니다.

이 가이드에서는 만들고 C# 코드를 사용 하 여 iOS 디자이너에서에서 그래픽 방식으로 만드는 대신 제약 조건을 사용 하는 방법을 설명 합니다.

<a name="Creating-Constraints-Programmatically" />

## <a name="creating-constraints-programmatically"></a>프로그래밍 방식으로 제약 조건 만들기

위에서 설명한 대로 일반적으로 작업할 자동 레이아웃 제약 조건이 있는 디자이너는 ios입니다. 제약 조건이 배포를 프로그래밍 방식으로 만들 필요가, 시간에 대 한 세 가지 옵션이 있습니다에서 선택할 수 있습니다.

* [레이아웃 앵커](#Layout-Anchors) -이 API는 앵커 속성에 대 한 액세스를 제공 (예: `TopAnchor`, `BottomAnchor` 또는 `HeightAnchor`) 제약 되 UI 항목의 합니다.
* [레이아웃 제약 조건을](#Layout-Constraints) -제약 조건을 사용 하 여 직접 만들 수는 `NSLayoutConstraint` 클래스입니다.
* [시각적 서식 지정 언어](#Visual-Format-Language) -메서드처럼 제약 조건을 정의 하는 ASCII 기술을 제공 합니다.

다음 섹션에서는 각 옵션을 자세히 살펴봅니다.

<a name="Layout-Anchors" />

### <a name="layout-anchors"></a>레이아웃 앵커

사용 하 여는 `NSLayoutAnchor` 제약 되 UI 항목의 앵커 속성에 따라 제약을 만들기 위한 fluent 인터페이스 있는 클래스입니다. 보기 컨트롤러의 위쪽 및 아래쪽 레이아웃 노출을 안내 하는 예를 들어는 `TopAnchor`, `BottomAnchor` 및 `HeightAnchor` 보기 가장자리, 가운데, 크기 및 초기 계획 속성을 표시 하는 동안 속성에 앵커를 지정 합니다.

> [!IMPORTANT]
> 뿐만 아니라 앵커 속성 집합이 표준 iOS 뷰가 포함 된 `LayoutMarginsGuides` 및 `ReadableContentGuide` 속성. 이러한 속성을 노출 `UILayoutGuide` 개체 보기의 여백 및 읽을 수 있는 작업에 대 한 지침을 각각 콘텐츠입니다.

레이아웃 앵커는 읽기 쉬운, compact 형식에서 제약 조건을 만드는 여러 가지 방법을 제공 합니다.

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

일반적인 레이아웃 제약 조건은 선형 식 처럼 간단 하 게 표현할 수 있습니다. 다음 예제를 참조하세요.

[![](programmatic-layout-constraints-images/graph01.png "선형 식으로 표시 되는 레이아웃 제약 조건")](programmatic-layout-constraints-images/graph01.png#lightbox)

레이아웃 앵커를 사용 하 여 C# 코드의 다음 줄으로 변환 됩니다.

```csharp
PurpleView.LeadingAnchor.ConstraintEqualTo (OrangeView.TrailingAnchor, 10).Active = true; 
```

여기서 C# 코드의 일부에 해당 수식의 주어진된 부분 다음과 같습니다.

|수식|코드|
|---|---|
|항목 1|PurpleView|
|1 특성|LeadingAnchor|
|Relationship|ConstraintEqualTo|
|승수|기본값을 지정 하지 않으므로 1.0|
|항목 2|OrangeView|
|특성 2|TrailingAnchor|
|상수|10.0|

지정 된 레이아웃 제약 조건 수식 해결 하는 데 필요한 매개 변수만 제공할 뿐만 아니라 형식에 전달 된 매개 변수 형식 안전성 적용 레이아웃 앵커 메서드는 각각. 따라서 가로 제약 조건 같은 고정 `LeadingAnchor` 또는 `TrailingAnchor` 에서만 사용할 수 있습니다 다른 가로 앵커와 형식 및 승수만 제공 크기 제약 조건입니다.

<a name="Layout-Constraints" />

### <a name="layout-constraints"></a>레이아웃 제약 조건

직접 구성 하 여 자동 레이아웃 제약 조건을 수동으로 추가할 수 있습니다는 `NSLayoutConstraint` C# 코드에서입니다. 레이아웃 앵커를 사용 하 여 달리 정의 하 고 제약 조건에 영향을 주지 미치게 될 경우에 모든 매개 변수에 대해 값을 지정 해야 합니다. 결과적으로, 상당한 읽기가 상용구 코드 생성 결국 것입니다. 예를 들어:

```csharp
//// Pin the leading edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Leading, NSLayoutRelation.Equal, View, NSLayoutAttribute.LeadingMargin, 1.0f, 0.0f).Active = true;

//// Pin the trailing edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Trailing, NSLayoutRelation.Equal, View, NSLayoutAttribute.TrailingMargin, 1.0f, 0.0f).Active = true;

//// Give the view a 1:2 aspect ratio
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Height, NSLayoutRelation.Equal, OrangeView, NSLayoutAttribute.Width, 2.0f, 0.0f).Active = true;
```

여기서는 `NSLayoutAttribute` enum 보기의 여백 값을 정의 하 고 해당 하는 `LayoutMarginsGuide` 와 같은 `Left`, `Right`, `Top` 및 `Bottom` 및 `NSLayoutRelation` enum 관계를 정의 하는 로 지정 된 특성 사이 작성 됩니다 `Equal`, `LessThanOrEqual` 또는 `GreaterThanOrEqual`합니다.

달리 레이아웃 앵커 API와는 `NSLayoutConstraint` 만들기 메서드는 특정 제약 조건의 중요 한 측면 강조 표시 되지 않습니다 되며 컴파일 없이 제약 조건에 수행 하는 확인 합니다. 결과적으로, 런타임 시 예외를 throw 합니다 잘못 된 제약 조건을 생성 하는 것이 쉽습니다.

<a name="Visual-Format-Language" />

### <a name="visual-format-language"></a>시각적 형식 언어

비주얼 형식 언어를 사용 하면 시각적 만들어지는 제약 조건으로 제공 하는 문자열 처럼 ASCII 기술을 사용 하 여 제약 조건을 정의할 수 있습니다. 이 장점과 단점은 다음과 같습니다.

- 시각적 언어 형식을 유효한 제약 조건 생성을 적용합니다.
 - 자동 레이아웃 제약 조건에 디버그 메시지를 제약 조건을 만드는 데 사용 하는 코드와 비슷합니다 Visual 언어 형식을 사용 하 여 콘솔에 출력 합니다.
 - 시각적 형식 언어를 사용 하면 매우 간단한 식 사용 하 여 동시에 여러 개의 제약 조건을 만들 수 있습니다.
 - 시각적 형식 언어 문자열의 컴파일 측 유효성 이므로 문제 런타임에 검색할 수 있습니다.
 - 완결성을 통해 시각화를 강조 하는 시각적 언어 형식을 있으므로 (예: 비율) 된 몇 가지 제약 조건 유형의 만들 수 없습니다.

제약 조건을 만드는 시각적 서식 언어를 사용 하는 경우 다음 단계를 수행 합니다.

1. 만들기는 `NSDictionary` 개체 보기 및 레이아웃 설명서 형식을 정의할 때 사용 될 문자열 키를 포함 합니다.
2. 필요에 따라 만들는 `NSDictionary` 키와 값의 집합을 정의 하는 (`NSNumber`) 제약 조건에 대 한 상수 값으로 사용 합니다.
3. 단일 열 이나 행 항목의 레이아웃에 형식 문자열을 만듭니다.
4. 호출 된 `FromVisualFormat` 의 메서드는 `NSLayoutConstraint` 제약 조건을 생성 하는 클래스입니다.
5. 호출 된 `ActivateConstraints` 의 메서드는 `NSLayoutConstraint` 클래스를 활성화 및 제약 조건을 적용 합니다.

예를 들어 선행 및 후행 제약 조건을 모두 시각적 서식 언어에서를 만들려면 사용할 수 있습니다 다음.

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

언어 시각적 형식을 기본 간격을 사용 하는 경우 부모 보기의 여백에 연결 된 0 개 지점 제약 항상 만들어지므로이 코드는 위에서 설명한 예제를 동일한 결과 생성 합니다.

한 줄에 여러 개의 자식 뷰 등의 더 복잡 한 UI 디자인에 대 한 시각적 언어 형식을 가로 간격과 세로 맞춤을 지정합니다. 여기서 지정 위의 예제와 같이 `AlignAllTop` `NSLayoutFormatOptions` 모든 뷰의 행 또는 열의 위쪽을 맞춥니다.

Apple의 참조 [시각적 서식 언어 부록](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1) 일반적인 제약 조건 및 시각적 서식 문자열 문법을 지정 하는 몇 가지 예제입니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 가이드 만들고 iOS 디자이너에서에서 그래픽 방식으로 만드는 대신 C#의 자동 레이아웃 제약 조건으로 작업을 제공 합니다. 레이아웃 앵커를 사용 하 여 살펴보고 먼저 (`NSLayoutAnchor`) 자동 레이아웃을 처리 하도록 합니다. 다음으로 레이아웃 제약 조건으로 작업 하는 방법에 알아보았습니다 (`NSLayoutConstraint`). 마지막으로, 자동 레이아웃에 대 한 시각적 서식 언어를 사용 하 여 표시 합니다.

## <a name="related-links"></a>관련 링크

- [Storyboards 소개](~/ios/user-interface/storyboards/index.md)
- [iOS 디자인할 수 있는 컨트롤 연습](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [IOS 용 Xamarin 디자이너로 자동 레이아웃](~/ios/user-interface/designer/designer-auto-layout.md#modifying-in-code)
- [Apple-프로그래밍 방식으로 제약 조건 만들기](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html#//apple_ref/doc/uid/TP40010853-CH16-SW1)
- [Apple-시각적 형식 언어 부록](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1)
