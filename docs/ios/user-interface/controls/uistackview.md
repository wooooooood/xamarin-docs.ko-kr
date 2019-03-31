---
title: Xamarin.iOS에서 스택 뷰
description: 이 문서에서는 스택을 사용 하 여 Xamarin.iOS 앱에 새 UIStackView 컨트롤의 하위 집합을 관리 하는 가로 또는 세로로 정렬에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 20246E87-2A49-438A-9BD7-756A1B50A617
ms.technology: xamarin-ios
ms.custom: xamu-video
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 6d5be72a9329675a65b0d6873d13894b314b50e7
ms.sourcegitcommit: 946ce514fd6575aa6b93ff24181e02a60b24b106
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58677848"
---
# <a name="stack-views-in-xamarinios"></a>Xamarin.iOS에서 스택 뷰

_이 문서에서는 스택을 사용 하 여 Xamarin.iOS 앱에 새 UIStackView 컨트롤의 하위 집합을 관리 하는 가로 또는 세로로 정렬에 대해 설명 합니다._

> [!IMPORTANT]
> StackView iOS 디자이너에서는 지원 되는 동안 발생할 수 있는 사용 편의성 버그 안정 채널을 사용 하는 경우 note 하십시오. 베타 또는 알파 채널을 전환 하면이 문제를 완화 해야 합니다. 필요한 수정 프로그램 안정 채널에서 구현 되는 때까지 Xcode를 사용 하 여이 연습을 제공 하도록 결정 했습니다.

스택 뷰 컨트롤 (`UIStackView`) 활용 자동 레이아웃 및 크기 클래스 가로 또는 세로로 하위의 스택을 관리 하는 화면 크기와 방향 iOS 장치에 동적으로 응답 합니다.

스택 보기에 연결 하는 모든 하위 뷰의 레이아웃을 축, 배포, 맞춤 간격 등 개발자 정의 속성에 따라 관리 합니다.

[![](uistackview-images/stacked01.png "스택 보기 레이아웃 다이어그램")](uistackview-images/stacked01.png#lightbox)

사용 하는 경우는 `UIStackView` Xamarin.iOS 앱에서 개발자 하거나 iOS 디자이너 또는 추가 하 고 C# 코드에서 하위 뷰를 제거 하 여 스토리 보드 내 하위 정의 하거나 수 있습니다.

이 문서는 두 부분으로 구성 됩니다: 첫 번째 스택을 보고, 구현 및 다음 작동 하는 방법에 대 한 몇 가지 자세한 기술 정보를 빠르게 시작 합니다.

> [!VIDEO https://youtube.com/embed/p3po6507Ip8]

**UIStackView, [Xamarin University](https://university.xamarin.com/)**

## <a name="uistackview-quickstart"></a>UIStackView 빠른 시작

간략 한 소개로는 `UIStackView` 컨트롤 하겠습니다 사용자 1에서 5 등급을 입력할 수 있도록 하는 간단한 인터페이스를 만들 수 있습니다. 두 스택 뷰를 사용 합니다: 인터페이스 장치의 화면 및 화면에서 1 ~ 5 등급 아이콘을 가로로 정렬 하나에 세로로 정렬 하 합니다.

### <a name="define-the-ui"></a>UI를 정의 합니다.

새 Xamarin.iOS 프로젝트를 시작 하 고 편집 합니다 **Main.storyboard** Xcode의 Interface Builder에서 파일입니다. 첫째, 단일을 끌어 **세로 스택 뷰** 에 **뷰 컨트롤러**:

[![](uistackview-images/quick01.png "뷰 컨트롤러의 단일 세로 스택 뷰를 끌어 옵니다.")](uistackview-images/quick01.png#lightbox)

에 **특성 검사기**, 다음 옵션을 설정 합니다.

[![](uistackview-images/quick02.png "스택 보기 옵션 설정")](uistackview-images/quick02.png#lightbox)

여기서

- **축** – 스택 뷰 하위 뷰를 하거나 정렬 하는 경우 결정 **가로로** 또는 **세로로**합니다.
- **맞춤** – 스택 보기 내에서 하위 뷰는 정렬 하는 방법을 제어 합니다.
- **배포** – 스택 뷰 내에 있는 하위 뷰 크기를 조정 하는 방법을 제어 합니다.
- **간격** – 스택 보기에 각 하위 뷰 사이의 최소 공간을 제어 합니다.
- **기준 상대** – 각 하위 보기의 세로 간격의 기준에서 파생 되는 경우이 옵션을 선택 합니다.
- **레이아웃 여백 상대** – 표준 레이아웃 여백을 기준으로 하위 뷰를 배치 합니다.

스택 보기를 사용할 때는 생각할 수 있습니다 합니다 **맞춤** 으로 **X** 및 **Y** 서브의 위치 및 **배포** 으로 **높이** 하 고 **너비**합니다.

> [!IMPORTANT]
> `UIStackView` 다른 하위 클래스와 같은 캔버스에 그릴 없는 비 렌더링 컨테이너 보기 및 이와 같이 `UIView`합니다. 따라서 같은 속성을 설정 `BackgroundColor` 재정의 또는 `DrawRect` visual 영향을 주지 것입니다.

다음과 비슷하도록 레이블, ImageView, 두 개의 단추 및 가로 스택 뷰를 추가 하 여 앱의 인터페이스 레이아웃을 계속 합니다.

[![](uistackview-images/quick03.png "스택 보기 UI 레이아웃")](uistackview-images/quick03.png#lightbox)

다음 옵션을 사용 하 여 가로 스택 뷰를 구성 합니다.

[![](uistackview-images/quick04.png "가로 스택 뷰 옵션 구성")](uistackview-images/quick04.png#lightbox)

늘어나 등급에서 각 "point"를 나타내는 아이콘에는 원하지 추가 될 때 가로 스택 뷰로 설정 했습니다 합니다 **맞춤** 하 **Center** 및  **배포** 하 **동일 하 게 입력**합니다.

마지막으로 다음 연결 **출 선** 하 고 **작업**:

[![](uistackview-images/quick05.png "스택 보기 출 선 및 작업")](uistackview-images/quick05.png#lightbox)

### <a name="populate-a-uistackview-from-code"></a>코드에서 UIStackView 채우기

Mac 및 편집에 대 한 Visual Studio로 돌아가서 합니다 **ViewController.cs** 파일과 다음 코드를 추가 합니다.

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

자세히 살펴보고 몇 가지가이 코드를 살펴보겠습니다. 먼저 사용 하 여는 `if` 문을 5 개가 넘는 "별표" 없다는 점을 확인 하려면 0 보다 작거나 합니다.

새 "스타"를 추가 하려면 해당 이미지를 로드 설정 및 해당 **콘텐츠 모드** 하 **측면에 맞게 크기 조정**:

```csharp
var icon = new UIImageView (new UIImage("icon.png"));
icon.ContentMode = UIViewContentMode.ScaleAspectFit;
```

이 스택 뷰를 추가할 때 왜곡 되지에서 "스타" 아이콘을 유지 합니다.

다음으로, 새 "스타" 아이콘을 하위 뷰의 스택 뷰 컬렉션에 추가:

```csharp
RatingView.AddArrangedSubview(icon);
```

추가 했습니다 보면 합니다 `UIImageView` 에 `UIStackView`의 `ArrangedSubviews` 속성 아닌는 `SubView`합니다. 원하는 스택 보기 레이아웃을 제어 하는 모든 보기에 추가 해야 합니다 `ArrangedSubviews` 속성입니다.

스택 보기에서 하위 뷰를 제거 하려면 먼저 얻게 제거할 하위 뷰:

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

둘 다에서 제거 해야 합니다 `ArrangedSubviews` 컬렉션 및 슈퍼 뷰:

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

만에서 하위 뷰 제거 `ArrangedSubviews` 컬렉션 스택 뷰 컨트롤에서 사용 되지만 화면에서 제거 되지는 않습니다.

### <a name="testing-the-ui"></a>UI 테스트

모든 필수 UI 요소 및 코드를 실행 하 고 인터페이스를 테스트 이제 수 있습니다. UI 표시 되 면 세로 스택 보기에 있는 요소의 모든 하 게 유지될지 위쪽에서에서 아래쪽입니다.

사용자가 누를 때를 **등급 증가** 단추를 다른 "스타" (최대 5) 화면에 추가 됩니다.

[![](uistackview-images/intro01.png "샘플 앱 실행")](uistackview-images/intro01.png#lightbox)

"별표" 자동으로 가운데에 있고 가로 스택 뷰에서 동일 하 게 분산 합니다. 사용자가 누를 때 합니다 **등급 감소** "스타" 단추 (왼쪽은 없음)까지 제거 됩니다.

## <a name="stack-view-details"></a>스택 뷰 정보

일반적인 이해를 돕는 만들었으므로 `UIStackView` 컨트롤 및 해당 기능 및 세부 정보 중 일부를 자세히 살펴보겠습니다 살펴보겠습니다 작동 하는 방식입니다.

### <a name="auto-layout-and-size-classes"></a>자동 레이아웃 및 크기 클래스

위에서 말한 것 처럼 하위 뷰에 추가 되 면 스택 보기 레이아웃을 배치 하 고 정렬 된 뷰 크기를 자동 레이아웃 및 Size 클래스를 사용 하 여 해당 스택 보기에 의해 완전히 제어 됩니다.

스택 보기는 _pin_ 해당 컬렉션의 첫 번째 및 마지막 하위 뷰를 **위쪽** 하 고 **아래쪽** 세로 스택 보기에 대 한 가장자리 또는 **왼쪽**하 고 **오른쪽** 가로 스택 보기에 대 한 가장자리입니다. 설정 하는 경우는 `LayoutMarginsRelativeArrangement` 속성을 `true`, 지 하는 대신 관련 여백 하위 뷰를 고정 하는 뷰를 합니다.

스택 보기는 하위 뷰를 사용 하 여 `IntrinsicContentSize` 속성의 정의 따라 하위 뷰 크기를 계산할 때 `Axis` (제외 하 고는 `FillEqually Distribution`). `FillEqually Distribution` 크기가 동일 수 있도록 모든 하위 뷰 크기를 조정 하므로 상의 스택 뷰를 채우는 `Axis`합니다.

경우는 예외를 `Fill Alignment`, 스택 보기는 하위 뷰를 사용 하 여 `IntrinsicContentSize` 수직으로 보기의 크기를 계산 하는 것에 대 한 속성을 지정 `Axis`. 에 대 한 합니다 `Fill Alignment`를 수직으로 스택 뷰를 채우도록 모든 하위 크기가 조정 되는 지정 된 `Axis`합니다.

### <a name="positioning-and-sizing-the-stack-view"></a>위치 및 스택 뷰 크기 조정

스택 보기에는 총 제어의 모든 하위 뷰 레이아웃에 있는 동안 (같은 속성에 따라 `Axis` 및 `Distribution`), 스택 보기를 배치 해야 (`UIStackView`) 자동 레이아웃 및 Size 클래스를 사용 하 여 부모 뷰에 내입니다.

일반적으로이 두 개 이상의 가장자리 확장 / 축소, 따라서 해당 위치를 정의 하는 데 스택 보기 고정 의미 합니다. 추가 제약 조건이 없는 스택 보기는 자동으로 크기를 조정할 수에 맞게 모든 해당 하위 같이:

 - 크기에 따라 해당 `Axis` 모든 하위 뷰 크기 및 각 하위 뷰 간에 정의 된 모든 공간 합계 됩니다.
 - 경우는 `LayoutMarginsRelativeArrangement` 속성은 `true`, 스택 뷰 크기 여백에 대 한 공간이 포함 됩니다.
 - 수직으로 크기를 `Axis` 컬렉션의 가장 큰 서브로 설정 됩니다.

스택 보기에 대 한 제약 조건을 지정할 수는 또한 **높이** 하 고 **너비**합니다. 이 경우 하위 뷰 배치 됩니다 (크기)에 따른 스택 보기에 의해 지정 된 공간에 맞게 합니다 `Distribution` 및 `Alignment` 속성입니다.

경우는 `BaselineRelativeArrangement` 속성은 `true`, 하위 뷰는 배치를 사용 하는 대신 첫 번째 또는 마지막 서브의 기준에 따라 합니다 **위쪽**, **아래쪽** 또는 **Center** -  **Y** 위치 합니다. 스택 보기의 내용에 다음과 같이 계산 된 이러한:

 - 세로 스택 뷰는 첫 번째 기준선에 대 한 첫 번째 서브 및 마지막에 대 한 마지막에 반환 됩니다. 이러한 하위 뷰 중 하나는 스택 뷰 자체에 해당 첫 번째 또는 마지막 기준 사용 됩니다.
 - 첫 번째 및 마지막 기준에 대 한 가로 스택 뷰를 채우도록 맞춰지거나 해당 하위 뷰를 사용 합니다. 가장 높은 보기 스택 보기도 인 경우 가장 긴 서브 기준선으로 사용 됩니다.

> [!IMPORTANT]
> 잘못 된 위치로 기준선을 계산 하는 대로 기준선 맞춤 확대 또는 압축 된 하위 뷰 크기에서 작동 하지 않습니다. 기준선 맞춤에 대 한 확인을 하위 뷰 **높이** 내장 콘텐츠 뷰의 일치 **높이**합니다.

### <a name="common-stack-view-uses"></a>일반적으로 스택 뷰 사용

스택 뷰 컨트롤에서 잘 작동 하는 레이아웃 형식은 여러 가지입니다. Apple에 따라 보다 일반적인 사용의 몇 가지 다음과 같습니다.

- **축 the 따라 크기를 정의** – 스택 뷰의 따라에 지 모두 고정 `Axis` 및 뷰 하위 뷰를 정의한 공간에 맞게 축을 따라 커집니다 스택의 위치를 설정 하려면 인접 한 가장자리 중 하나입니다.
 -  **하위 뷰의 위치를 정의** – 부모 보기에 스택 보기의 인접 한 가장자리에 고정 하 여 스택 보기에 포함 된 하위에 맞게 크기를 모두 커집니다.
- **크기와 위치 스택의 정의** – 스택 뷰는 부모 보기에 스택 보기의 모든 가장자리에 고정 하 여 스택 뷰 내에 정의 된 공간에 따라 하위 뷰를 정렬 합니다.
 -  **정의 된 크기 수직 축** – 모두 가장자리 수직 스택 보기에 고정 하 여 `Axis` 보기 정의한 공간에 맞도록 축과 수직 커집니다 스택의 위치를 설정 하려면 축 따라 가장자리 중 해당 하위 뷰입니다.

### <a name="managing-the-appearance"></a>모양을 관리

`UIStackView` 은 다른 하위 클래스와 같은 캔버스에 그릴 없는 비 렌더링 컨테이너 보기 및 이와 같이 `UIView`합니다. 와 같은 속성을 설정 `BackgroundColor` 재정의 또는 `DrawRect` visual 영향을 주지 것입니다.

가지 스택 보기가 하위 컬렉션을 조정 하는 방법을 제어 하는 몇 가지 속성이 있습니다.

- **축** – 스택 뷰 하위 뷰를 하거나 정렬 하는 경우 결정 **가로로** 또는 **세로로**합니다.
- **맞춤** – 스택 보기 내에서 하위 뷰는 정렬 하는 방법을 제어 합니다.
- **배포** – 스택 뷰 내에 있는 하위 뷰 크기를 조정 하는 방법을 제어 합니다.
- **간격** – 스택 보기에 각 하위 뷰 사이의 최소 공간을 제어 합니다.
- **기준 상대** – `true`, 각 하위 보기의 세로 간격의 기준에서 파생 됩니다.
- **레이아웃 여백 상대** – 표준 레이아웃 여백을 기준으로 하위 뷰를 배치 합니다.

일반적으로 소수의 하위 뷰를 정렬 하는 스택 뷰를 사용 합니다. 하나 이상의 스택 뷰 서로의 내부에 중첩 하 여 더 복잡 한 사용자 인터페이스를 만들 수 있습니다 (에서 수행한 것 처럼 합니다 [UIStackView 퀵 스타트](#uistackview-quickstart) 위에).

추가 제약 조건 (예: 컨트롤의 높이 또는 너비)를 하위 뷰를 추가 하 여 추가 Ui 모양을 미세 조정할 수 있습니다. 그러나 주의 해야를 자체 스택 보기에 의해 발생 하는 충돌 하는 제약 조건을 포함 하지 않도록 합니다.

### <a name="maintaining-arranged-views-and-sub-views-consistency"></a>뷰와 하위 뷰 일관성 정렬 유지 관리

스택 보기를 사용 하면는 해당 `ArrangedSubviews` 속성의 하위 집합은 항상 해당 `Subviews` 다음 규칙을 사용 하 여 속성:

 - 하위 뷰를 추가할 경우 합니다 `ArrangedSubviews` 컬렉션을 자동으로 추가 됩니다 하는 `Subviews` 컬렉션 (있지 않는 해당 컬렉션의 일부).
 - 하위 뷰는에서 제거 된 경우는 `Subviews` (표시에서 제거) 하는 컬렉션에서 에서도 제거 됩니다는 `ArrangedSubviews` 컬렉션입니다.
 - 하위 뷰를 제거 합니다 `ArrangedSubviews` 컬렉션에서 제거 되지 않습니다는 `Subviews` 컬렉션입니다. 따라서 더 이상으로 레이아웃 해야 스택 뷰, 있지만 계속 화면에 표시 됩니다.

그러나 합니다 `ArrangedSubviews` 컬렉션의 하위 집합은 항상를 `Subview` 컬렉션 각 컬렉션 내의 개별 하위 순서는 다음에 의해 제어 되 고 별도:

 - 내에서 하위 순서는 `ArrangedSubviews` 컬렉션 스택 내에서 표시 순서를 결정 합니다.
 - 내에서 하위 순서는 `Subview` 컬렉션 뒤에서 앞으로 뷰에서 해당 Z 순서 (또는 계층)을 결정 합니다.

### <a name="dynamically-changing-content"></a>콘텐츠를 동적으로 변경

자동으로 스택 보기는 하위 뷰 추가, 제거 또는 숨겨진 때마다 하위 뷰의 레이아웃을 조정 합니다. 레이아웃 스택 보기의 속성을 조정 하는 경우에 조정 될 (같은 해당 `Axis`).

레이아웃 변경은 애니메이션 블록 내에서 예를 들어 배치 하 여 애니메이션을 적용할 수 있습니다.

```csharp
// Animate stack
UIView.Animate(0.25, ()=>{
    // Adjust stack view
    RatingView.LayoutIfNeeded();
});
```

스택 보기의 속성 중 상당수는 Storyboard 내 Size 클래스를 사용 하 여 지정할 수 있습니다. 이러한 속성을 자동으로 됩니다 크기나 방향 변경에 대 한 응답은 애니메이션 효과가 적용 됩니다.

## <a name="summary"></a>요약

이 문서에서는 이러한 새 부분이 `UIStackView` 컨트롤 (iOS 9) Xamarin.iOS 앱에서 가로 또는 세로로 정렬 스택의 하위 집합을 관리할 수 있습니다.
스택 뷰를 사용 하 여 UI를 만드는 간단한 예제로 시작 하 고 더 자세히 살펴보려면 스택 뷰 및 해당 속성 및 기능을 사용 하 여 완료 합니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [새로운 iOS 9.0에서](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [UIStackView 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/)
- [소개 UIStackView (비디오)](https://university.xamarin.com/lightninglectures/introducing-uistackview-on-ios9)
