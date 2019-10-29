---
title: Xamarin.ios의 스택 뷰
description: 이 문서에서는 Xamarin.ios 앱에서 새 UIStackView 컨트롤을 사용 하 여 가로 또는 세로로 정렬 된 스택에 있는 하위 뷰 집합을 관리 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 20246E87-2A49-438A-9BD7-756A1B50A617
ms.technology: xamarin-ios
ms.custom: xamu-video
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: b4a8507d4d1497964f6b60307622ca3e1dc4cd90
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021798"
---
# <a name="stack-views-in-xamarinios"></a>Xamarin.ios의 스택 뷰

_이 문서에서는 Xamarin.ios 앱에서 새 UIStackView 컨트롤을 사용 하 여 가로 또는 세로로 정렬 된 스택에 있는 하위 뷰 집합을 관리 하는 방법을 설명 합니다._

> [!IMPORTANT]
> StackView는 iOS 디자이너에서 지원 되지만 안정적인 채널을 사용 하는 경우 유용성 버그가 발생할 수 있습니다. 베타 또는 알파 채널을 전환 하면이 문제가 완화 됩니다. 안정적인 채널에서 필요한 수정이 구현 될 때까지 Xcode를 사용 하 여이 연습을 제공 하기로 결정 했습니다.

`UIStackView`(Stack View control)는 Auto Layout 및 Size 클래스의 강력한 기능을 활용 하 여 iOS 장치의 방향 및 화면 크기에 동적으로 응답 하는 가로 또는 세로로 하위 뷰 스택을 관리 합니다.

스택 보기에 연결 된 모든 하위 뷰의 레이아웃은 축, 분포, 맞춤 및 간격과 같은 개발자 정의 속성을 기반으로 하 여 관리 됩니다.

[![](uistackview-images/stacked01.png "Stack View layout diagram")](uistackview-images/stacked01.png#lightbox)

Xamarin.ios 앱에서 `UIStackView`를 사용 하는 경우 개발자는 iOS 디자이너의 스토리 보드 내에서 또는 코드에서 C# 하위 뷰를 추가 및 제거 하 여 하위 뷰을 정의할 수 있습니다.

이 문서는 첫 번째 스택 뷰를 구현 하는 데 도움이 되는 빠른 시작 및 작동 방식에 대 한 몇 가지 자세한 기술 정보로 구성 됩니다.

> [!VIDEO https://youtube.com/embed/p3po6507Ip8]

**UIStackView 비디오**

## <a name="uistackview-quickstart"></a>UIStackView 빠른 시작

`UIStackView` 컨트롤에 대 한 간략 한 소개로, 사용자가 1 ~ 5의 등급을 입력할 수 있는 간단한 인터페이스를 만들어 보겠습니다. 두 개의 스택 뷰를 사용 합니다. 하나는 장치 화면에서 수직으로 인터페이스를 정렬 하는 것이 고 다른 하나는 화면 전체에서 1-5 등급 아이콘을 가로로 정렬 하는 것입니다.

### <a name="define-the-ui"></a>UI 정의

새 Xamarin.ios 프로젝트를 시작 하 고 Xcode의 Interface Builder에서 **주 storyboard** 파일을 편집 합니다. 먼저 **뷰 컨트롤러**에서 단일 **세로 스택 뷰** 를 끌어 옵니다.

[![](uistackview-images/quick01.png "Drag a single Vertical Stack View on the View Controller")](uistackview-images/quick01.png#lightbox)

**특성 검사자**에서 다음 옵션을 설정 합니다.

[![](uistackview-images/quick02.png "Set the Stack View options")](uistackview-images/quick02.png#lightbox)

여기서

- **축** – 스택 뷰에서 하위 뷰를 **가로** 또는 **세로로**정렬 하는지 결정 합니다.
- **Alignment** – 스택 뷰 내에서 하위 뷰를 정렬 하는 방법을 제어 합니다.
- **분포** – 스택 보기 내에서 하위 뷰 크기를 조정 하는 방법을 제어 합니다.
- **간격** – 스택 뷰에서 각 하위 뷰 사이의 최소 공간을 제어 합니다.
- 기준 **상대** – 선택 하는 경우 각 하위 뷰의 세로 간격이 해당 기준선에서 파생 됩니다.
- **레이아웃 여백 비례** – 표준 레이아웃 여백에 상대적으로 하위 뷰을 배치 합니다.

스택 뷰를 사용 하는 경우에는 **맞춤** 을 하위 뷰의 **X** 및 **Y** 위치로 생각 하 고 **높이** 와 **너비**를 **배분** 합니다.

> [!IMPORTANT]
> `UIStackView` 렌더링 되지 않는 컨테이너 뷰로 설계 되었으므로 `UIView`의 다른 서브 클래스와 같이 캔버스로 그려지지 않습니다. 따라서 `BackgroundColor`와 같은 속성을 설정 하거나 `DrawRect`를 재정의 해도 시각적 효과가 없습니다.

레이블, ImageView, 두 단추 및 가로 스택 뷰를 추가 하 여 다음과 같이 응용 프로그램의 인터페이스를 계속 레이아웃 합니다.

[![](uistackview-images/quick03.png "Laying out the Stack View UI")](uistackview-images/quick03.png#lightbox)

다음 옵션을 사용 하 여 가로 스택 보기를 구성 합니다.

[![](uistackview-images/quick04.png "Configure the Horizontal Stack View options")](uistackview-images/quick04.png#lightbox)

등급에서 각 "point"를 나타내는 아이콘이 가로 스택 보기에 추가 될 때 확장 될 것을 원하지 않으므로 맞춤을 **가운데** **맞춤** 으로 설정 하 고 **분포** 를 균등 하 게 **채웁니다**.

마지막으로 다음 **콘센트** 및 **작업**을 연결 합니다.

[![](uistackview-images/quick05.png "The Stack View Outlets and Actions")](uistackview-images/quick05.png#lightbox)

### <a name="populate-a-uistackview-from-code"></a>코드에서 UIStackView 채우기

Mac용 Visual Studio로 돌아가서 **ViewController.cs** 파일을 편집 하 고 다음 코드를 추가 합니다.

```csharp
public int Rating { get; set;} = 0;
...

partial void IncreaseRating (Foundation.NSObject sender) {

    // Maximum of 5 "stars"
    if (++Rating > 5 ) {
        // Abort
        Rating = 5;
        return;
    }

    // Create new rating icon and add it to stack
    var icon = new UIImageView (new UIImage("icon.png"));
    icon.ContentMode = UIViewContentMode.ScaleAspectFit;
    RatingView.AddArrangedSubview(icon);

    // Animate stack
    UIView.Animate(0.25, ()=>{
        // Adjust stack view
        RatingView.LayoutIfNeeded();
    });

}

partial void DecreaseRating (Foundation.NSObject sender) {

    // Minimum of zero "stars"
    if (--Rating < 0) {
        // Abort
        Rating =0;
        return;
    }

    // Get the last subview added
    var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];

    // Remove from stack and screen
    RatingView.RemoveArrangedSubview(icon);
    icon.RemoveFromSuperview();

    // Animate stack
    UIView.Animate(0.25, ()=>{
        // Adjust stack view
        RatingView.LayoutIfNeeded();
    });
}
```

이 코드에 대 한 몇 가지 자세한 내용을 살펴보겠습니다. 먼저 `if` 문을 사용 하 여 5 개 이하의 "별" 또는 0 보다 작은 것이 없는지 확인 합니다.

새 "star"를 추가 하려면 이미지를 로드 하 고 해당 **콘텐츠 모드** 를 **가로 세로 막대 크기 조정**으로 설정 합니다.

```csharp
var icon = new UIImageView (new UIImage("icon.png"));
icon.ContentMode = UIViewContentMode.ScaleAspectFit;
```

이렇게 하면 "별모양" 아이콘이 스택 뷰에 추가 될 때 왜곡 되지 않습니다.

다음으로 새 "star" 아이콘을 하위 뷰의 스택 뷰의 컬렉션에 추가 합니다.

```csharp
RatingView.AddArrangedSubview(icon);
```

`UIImageView` `UIStackView`의 `ArrangedSubviews` 속성에 추가 되었으며 `SubView`에 추가 되지 않았습니다. 스택 보기에서 레이아웃을 제어 하는 뷰를 `ArrangedSubviews` 속성에 추가 해야 합니다.

스택 뷰에서 하위 뷰를 제거 하려면 먼저 제거할 하위 뷰를 가져옵니다.

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

그런 다음 `ArrangedSubviews` 컬렉션과 슈퍼 뷰에서 모두 제거 해야 합니다.

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

`ArrangedSubviews` 컬렉션에서 하위 뷰를 제거 하면 스택 뷰의 컨트롤이 아니라 화면에서 하위 뷰를 제거 하지 않습니다.

### <a name="testing-the-ui"></a>UI 테스트

이제 필요한 모든 UI 요소와 코드를 사용 하 여 인터페이스를 실행 하 고 테스트할 수 있습니다. UI가 표시 되 면 세로 스택 뷰의 모든 요소가 위쪽에서 아래쪽으로 균등 하 게 배치 됩니다.

사용자가 [ **등급 높이기** ] 단추를 누르면 화면에 다른 "star"가 추가 됩니다 (최대 5 개).

[![](uistackview-images/intro01.png "The sample app run")](uistackview-images/intro01.png#lightbox)

"별"은 가로 스택 보기에서 자동으로 가운데 맞춤 되 고 균등 하 게 분산 됩니다. 사용자가 **등급 낮추기** 단추를 누르면 "star"가 제거 됩니다 (아무것도 남아 있지 않을 때까지).

## <a name="stack-view-details"></a>스택 보기 세부 정보

이제 `UIStackView` 제어의 정의와 작동 방법에 대 한 일반적인 아이디어가 있으므로 일부 기능 및 세부 정보를 자세히 살펴보겠습니다.

### <a name="auto-layout-and-size-classes"></a>자동 레이아웃 및 크기 클래스

위에서 언급 한 대로 하위 뷰가 스택 뷰에 추가 될 때 해당 레이아웃은 자동 레이아웃 및 크기 클래스를 사용 하 여 정렬 된 뷰의 위치 및 크기를 조정 하는 해당 스택 뷰에서 완전히 제어 됩니다.

스택 뷰에서는 컬렉션의 첫 번째 및 마지막 **하위** 뷰를 세로 스택 보기의 **위쪽** 및 아래쪽 가장자리에 고정 하거나 가로 스택 보기의 **왼쪽** 및 **오른쪽** 가장자리에 _고정_ 합니다. `LayoutMarginsRelativeArrangement` 속성을 `true`로 설정 하면 뷰가 가장자리가 아닌 관련 여백에 하위 뷰를 고정 합니다.

스택 뷰는 정의 된 `Axis` (`FillEqually Distribution`제외)를 따라 하위 뷰 크기를 계산할 때 하위 뷰의 `IntrinsicContentSize` 속성을 사용 합니다. `FillEqually Distribution`는 크기가 동일 하도록 모든 하위 뷰 크기를 조정 하 여 `Axis`따라 스택 뷰를 채웁니다.

`Fill Alignment`를 제외 하 고 스택 뷰에서는 지정 된 `Axis`에 수직인 뷰의 크기를 계산 하기 위해 하위 뷰의 `IntrinsicContentSize` 속성을 사용 합니다. `Fill Alignment`에 대해 모든 하위 뷰은 지정 된 `Axis`에 수직인 스택 뷰를 채우도록 크기가 지정 됩니다.

### <a name="positioning-and-sizing-the-stack-view"></a>스택 보기 위치 지정 및 크기 조정

스택 보기에는 `Axis` 및 `Distribution`같은 속성을 기반으로 하는 하위 뷰의 레이아웃에 대 한 전체 제어 권한이 있지만 Auto Layout 및 Size 클래스를 사용 하 여 부모 뷰 내에 스택 뷰 (`UIStackView`)를 배치 해야 합니다.

일반적으로이는 스택 뷰의 가장자리를 두 개 이상 고정 하 여 확장 및 축소 하 여 해당 위치를 정의 함을 의미 합니다. 추가 제약 조건이 없는 경우 스택 보기는 다음과 같이 모든 하위 뷰에 맞게 자동으로 크기가 조정 됩니다.

- `Axis` 따라 크기는 모든 하위 뷰 크기와 각 하위 뷰 사이에 정의 된 모든 공간을 합한 것입니다.
- `LayoutMarginsRelativeArrangement` 속성이 `true`이면 스택 보기 크기에도 여백의 공간이 포함 됩니다.
- `Axis`에 수직인 크기는 컬렉션에서 가장 큰 하위 뷰로 설정 됩니다.

또한 스택 뷰의 **높이** 및 **너비**에 대 한 제약 조건을 지정할 수 있습니다. 이 경우 하위 뷰는 `Distribution` 및 `Alignment` 속성에 따라 결정 되는 스택 뷰에 지정 된 공간을 채우도록 배치 (크기 조정) 됩니다.

`BaselineRelativeArrangement` 속성이 `true`인 경우 **위쪽**, 아래쪽 또는 **가운데**- **Y** 위치를 사용 하는 대신 첫 번째 또는 마지막 **하위** 뷰의 기준선을 기준으로 하위 뷰이 배치 됩니다. 이러한 항목은 스택 뷰의 콘텐츠에서 다음과 같이 계산 됩니다.

- 세로 스택 뷰는 첫 번째 기준선에 대해 첫 번째 하위 뷰를 반환 하 고 마지막에는 마지막 하위 뷰를 반환 합니다. 이러한 하위 뷰 중 하나가 스택 보기 이면 첫 번째 또는 마지막 기준이 사용 됩니다.
- 가로 스택 보기는 첫 번째 및 마지막 기준선 모두에 가장 긴 하위 뷰를 사용 합니다. 가장 긴 뷰도 스택 보기 이면 가장 긴 하위 뷰를 기준선으로 사용 합니다.

> [!IMPORTANT]
> 기준선 맞춤은 잘못 된 위치로 계산 되기 때문에 스트레치 되었거나 압축 된 하위 뷰 크기에서는 작동 하지 않습니다. 기준선 맞춤의 경우 하위 뷰의 **높이가** 내장 콘텐츠 뷰의 **높이**와 일치 하는지 확인 합니다.

### <a name="common-stack-view-uses"></a>공용 스택 뷰 사용

스택 뷰 컨트롤에서 잘 작동 하는 몇 가지 레이아웃 유형이 있습니다. Apple에 따라 다음과 같은 몇 가지 일반적인 사용 방법에 대해 살펴보겠습니다.

- **축을 따라 크기를 정의** 합니다. 즉, 스택 뷰의 `Axis`와 인접 한 가장자리 중 하나를 함께 고정 하 여 위치를 설정 하면 스택 뷰가 축에 따라 하위 뷰에서 정의 된 공간에 맞게 늘어납니다.
- 하위 뷰의 **위치 정의** – 부모 뷰에 스택 뷰의 인접 한 가장자리에 고정 하 여 하위 뷰 포함 하는 크기에 맞게 스택 뷰를 늘립니다.
- 스택의 **크기 및 위치를 정의** 합니다. 스택 보기의 네 가지 모든 가장자리를 부모 뷰에 고정 하 여 스택 뷰에서 스택 보기 내에 정의 된 공간을 기준으로 하위 뷰을 정렬 합니다.
- **축에 수직인 크기 정의** – 스택 뷰의 `Axis`에 수직인 가장자리와 축을 따라 가장자리 중 하나를 고정 하 여 위치를 설정 하면 스택 뷰가 축에 수직으로 증가 하 여 하위 뷰에 정의 된 공간에 맞게 조정 됩니다.

### <a name="managing-the-appearance"></a>모양 관리

`UIStackView` 렌더링 되지 않는 컨테이너 뷰로 설계 되었으므로 `UIView`의 다른 서브 클래스와 같이 캔버스로 그려지지 않습니다. `BackgroundColor` 또는 `DrawRect` 재정의와 같은 속성을 설정 해도 시각적 효과가 없습니다.

스택 뷰에서 하위 뷰의 컬렉션을 정렬 하는 방법을 제어 하는 몇 가지 속성이 있습니다.

- **축** – 스택 뷰에서 하위 뷰를 **가로** 또는 **세로로**정렬 하는지 결정 합니다.
- **Alignment** – 스택 뷰 내에서 하위 뷰를 정렬 하는 방법을 제어 합니다.
- **분포** – 스택 보기 내에서 하위 뷰 크기를 조정 하는 방법을 제어 합니다.
- **간격** – 스택 뷰에서 각 하위 뷰 사이의 최소 공간을 제어 합니다.
- 기준 **상대** – `true`경우 각 하위 뷰의 세로 간격이 해당 기준선에서 파생 됩니다.
- **레이아웃 여백 비례** – 표준 레이아웃 여백에 상대적으로 하위 뷰을 배치 합니다.

일반적으로 스택 뷰를 사용 하 여 적은 수의 하위 뷰를 정렬 하 게 됩니다. 위의 [Uistackview 빠른](#uistackview-quickstart) 시작에서와 같이 서로 하나 이상의 스택 뷰를 중첩 하 여 더 복잡 한 사용자 인터페이스를 만들 수 있습니다.

하위 뷰에 제약 조건을 추가 하 여 (예를 들어 높이 또는 너비 제어) Ui 모양을 세부적으로 조정할 수 있습니다. 그러나 스택 뷰 자체에서 도입 된 제약 조건에 충돌 하는 제약 조건을 포함 하지 않도록 주의 해야 합니다.

### <a name="maintaining-arranged-views-and-sub-views-consistency"></a>정렬 된 뷰 및 하위 뷰 일관성 유지 관리

스택 뷰에서는 `ArrangedSubviews` 속성이 항상 다음 규칙을 사용 하 여 해당 `Subviews` 속성의 하위 집합이 되도록 합니다.

- `ArrangedSubviews` 컬렉션에 하위 뷰가 추가 되 면 `Subviews` 컬렉션에 자동으로 추가 됩니다 (이미 해당 컬렉션에 속해 있지 않은 경우).
- 하위 뷰가 `Subviews` 컬렉션에서 제거 되 면 (표시에서 제거 됨) `ArrangedSubviews` 컬렉션 에서도 제거 됩니다.
- `ArrangedSubviews` 컬렉션에서 하위 뷰를 제거 해도 `Subviews` 컬렉션에서 제거 되지 않습니다. 따라서 더 이상 스택 뷰로 레이아웃 되지 않지만 화면에 계속 표시 됩니다.

`ArrangedSubviews` 컬렉션은 항상 `Subview` 컬렉션의 하위 집합 이지만 각 컬렉션 내의 개별 하위 뷰 순서는 별개 이며 다음에 의해 제어 됩니다.

- `ArrangedSubviews` 컬렉션 내의 하위 뷰 순서는 스택 내에서의 표시 순서를 결정 합니다.
- `Subview` 컬렉션 내에서 하위 뷰의 순서는 뷰 내에서 Z 순서 (또는 계층화)를 다시 front로 결정 합니다.

### <a name="dynamically-changing-content"></a>동적으로 콘텐츠 변경

뷰를 추가, 제거 또는 숨길 때마다 스택 뷰에서 하위 뷰의 레이아웃이 자동으로 조정 됩니다. 스택 뷰의 속성 (예: `Axis`)이 조정 된 경우에도 레이아웃이 조정 됩니다.

레이아웃 변경 내용은 애니메이션 블록 내에 배치 하 여 애니메이션 효과를 낼 수 있습니다. 예를 들면 다음과 같습니다.

```csharp
// Animate stack
UIView.Animate(0.25, ()=>{
    // Adjust stack view
    RatingView.LayoutIfNeeded();
});
```

대부분의 스택 보기 속성은 Storyboard 내의 Size 클래스를 사용 하 여 지정할 수 있습니다. 이러한 속성은 크기 또는 방향 변경에 대 한 응답으로 자동으로 적용 됩니다.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 앱에서 가로 또는 세로로 정렬 된 스택에 하위 뷰 집합을 관리 하는 새로운 `UIStackView` 컨트롤 (iOS 9의 경우)에 대해 설명 했습니다.
스택 뷰를 사용 하 여 UI를 만드는 간단한 예제부터 시작 하 여 스택 뷰와 해당 속성 및 기능에 대해 자세히 살펴봅니다.

## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0의 새로운 기능](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [UIStackView 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/)
- [UIStackView 소개 (비디오)](https://university.xamarin.com/lightninglectures/introducing-uistackview-on-ios9)
