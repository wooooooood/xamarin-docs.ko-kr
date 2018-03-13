---
title: "스택 보기"
description: "이 문서에서는 스택을 사용 중 Xamarin.iOS 앱의 새로운 UIStackView 컨트롤의 하위 집합을 관리 하는 가로 또는 세로로 정렬 되지 않습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 20246E87-2A49-438A-9BD7-756A1B50A617
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 82bcd29a201be01bc8123e313e5a76b82668cb85
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="stack-view"></a>스택 보기

_이 문서에서는 스택을 사용 중 Xamarin.iOS 앱의 새로운 UIStackView 컨트롤의 하위 집합을 관리 하는 가로 또는 세로로 정렬 되지 않습니다._

> [!IMPORTANT]
> StackView은 iOS 디자이너에서에서 지원 되지만, 발생할 수 있는 유용성 버그 안정적인 채널을 사용할 때 note 하십시오. 전환의 베타 또는 알파 채널이이 문제를 완화 해야 합니다. 필요한 수정 프로그램 안정적인 채널에서 구현 될 때까지 Xcode를 사용 하 여이 연습을 제공 하도록 결정 했습니다.

스택 뷰 컨트롤 (`UIStackView`) 이용 하 여 자동 레이아웃 및 크기 클래스 가로 또는 세로로 하위 뷰가, 스택에서 관리 하는 iOS 장치의 방향 및 화면 크기에 동적으로 응답 합니다.

스택 보기에 연결 된 모든 하위 뷰가 레이아웃을 같은 축, 배포, 정렬 및 간격 정의 개발자 속성에 따라 관리 합니다.

[![](uistackview-images/stacked01.png "스택 뷰 레이아웃 다이어그램")](uistackview-images/stacked01.png#lightbox)

사용 하는 경우는 `UIStackView` Xamarin.iOS 앱의 개발자 정의할 수 하위 스토리 보드 디자이너는 iOS 또는 추가 하 고 C# 코드에서 하위 뷰가 제거 하 여 내부 중 하나입니다.

이 문서는 두 부분으로 구성 됩니다: 첫 번째 스택을 볼 구현 하 고 일부 작동 방법에 대 한 기술적 세부 다음 수 있도록 빠른 시작 합니다.

> [!VIDEO https://youtube.com/embed/p3po6507Ip8]

**UIStackView, 만든 사람 [Xamarin 대학](https://university.xamarin.com/)**

## <a name="uistackview-quickstart"></a>UIStackView 빠른 시작

간략 한 소개도는 `UIStackView` 컨트롤 하겠습니다 1에서 5에 대 한 등급을 입력 하는 데 사용할 수 있는 간단한 인터페이스를 만들 수 있습니다. 두 스택 뷰를 사용해 보겠습니다: 인터페이스 장치 화면 및 화면에서 1-5 등급 아이콘을 가로 방향으로 정렬 하는 하나에서 세로로 정렬 하는 하나입니다.

### <a name="define-the-ui"></a>UI를 정의 합니다.

새 Xamarin.iOS 프로젝트를 시작 하 고 편집 된 **Main.storyboard** Xcode의 인터페이스 작성기에는 파일입니다. 첫째, 단일을 끌어 **세로 스택 뷰** 에 **뷰-컨트롤러**:

[![](uistackview-images/quick01.png "뷰 컨트롤러의 단일 세로 스택 뷰를 끌어 옵니다.")](uistackview-images/quick01.png#lightbox)

에 **특성 검사기**, 다음 옵션을 설정 합니다.

[![](uistackview-images/quick02.png "스택 보기 옵션 설정")](uistackview-images/quick02.png#lightbox)

여기서

- **축** – 스택 뷰 하위 뷰 들을 하거나 정렬 하는 경우 결정 **가로로** 또는 **세로로**합니다.
- **맞춤** –는 하위 스택 뷰 내에서 정렬 되는 방법을 제어 합니다.
- **배포** – 스택 뷰 내에서 하위 뷰 들 크기를 조정 하는 방법을 제어 합니다.
- **간격** – 스택 뷰에서 하위 각 뷰 사이의 최소 간격을 제어 합니다.
- **초기 상대** – 각 하위 보기의 세로 간격의 초기 계획에서 파생 될 경우이 옵션을 선택 합니다.
- **레이아웃 여백 상대** – 하위 뷰 들 표준 레이아웃에 상대적으로 배치 합니다.

스택 뷰를 사용할 때의 생각할 수 있습니다는 **맞춤** 로 **X** 및 **Y** 서브의 위치 및 **배포** 으로 **높이** 및 **너비**합니다.

> [!IMPORTANT]
> **참고:** `UIStackView` 기능은의 다른 하위 클래스와 같은 캔버스에 그릴 없는 비 렌더링 컨테이너 보기로 이동 하 고 따라서 `UIView`합니다. 따라서 같은 속성을 설정 `BackgroundColor` 또는 재정의 `DrawRect` 시각적 영향을 미치지 것입니다.

다음과 유사한 되도록 레이블, ImageView, 두 개의 단추 및 가로 스택 뷰를 추가 하 여 응용 프로그램의 인터페이스 레이아웃을 계속 합니다.

[![](uistackview-images/quick03.png "스택 뷰 UI 배치")](uistackview-images/quick03.png#lightbox)

다음 옵션으로 가로 스택 뷰를 구성 합니다.

[![](uistackview-images/quick04.png "가로 스택 뷰 옵션 구성")](uistackview-images/quick04.png#lightbox)

않기 때문에 각 "요소"를 나타내는 아이콘 늘이기 등급에 추가 하면 가로 스택 뷰를 설정 했습니다는 **맞춤** 를 **센터** 및  **배포** 를 **동일 하 게 채울**합니다.

마지막으로, 다음 연결 **콘센트** 및 **동작**:

[![](uistackview-images/quick05.png "스택 뷰 콘센트 및 동작")](uistackview-images/quick05.png#lightbox)

### <a name="populate-a-uistackview-from-code"></a>코드에서 UIStackView 채우기

Mac 및 편집에 대 한 Visual Studio로 되돌아가려면는 **ViewController.cs** 파일을 다음 코드를 추가 합니다.

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

살펴보겠습니다이 코드의 몇 가지 부분에서 자세히 설명에서 합니다. 사용 하 여 먼저는 `if` 6 개 이상의 "별표" 없음을 확인 하는 문을 0 보다 작거나 합니다.

새 "별모양"를 추가 하려면 해당 이미지 로드 설정 하 고 해당 **콘텐츠 모드** 를 **측면에 맞게 크기 조정**:

```csharp
var icon = new UIImageView (new UIImage("icon.png"));
icon.ContentMode = UIViewContentMode.ScaleAspectFit;
```

그러면 "별표" 아이콘 스택 보기에 추가 될 때 왜곡 되지에서 유지 됩니다.

다음으로 새 "별표" 아이콘 하위 뷰가의 스택 뷰 컬렉션에 추가합니다.

```csharp
RatingView.AddArrangedSubview(icon);
```

추가한 이유를 알 수는 `UIImageView` 에 `UIStackView`의 `ArrangedSubviews` 속성 및 아니라는 `SubView`합니다. 스택 뷰 해당 레이아웃을 제어 하려는 모든 보기에 추가 되어야 합니다는 `ArrangedSubviews` 속성입니다.

먼저 스택 뷰에서 하위 뷰를 제거 하려면 제거할 서브를 가져올 있습니다.

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

둘 다에서 제거 해야 합니다는 `ArrangedSubviews` 컬렉션과 슈퍼 보기:

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

요소만 제거 하는 하위 뷰 `ArrangedSubviews` 컬렉션 스택 뷰 컨트롤에서 사용 되지만 화면에서 제거 되지 않습니다.

### <a name="testing-the-ui"></a>UI 테스트

모든 필수 UI 요소와 코드 위치에서 이제 실행 하 고 사용할 수는 인터페이스를 테스트 합니다. UI가 표시 되 면 모든 세로 스택 뷰에서 요소 됩니다 간격이 일정 하 게 위에서 아래로 합니다.

사용자가 누를 때는 **등급 올리기** 단추를 다른 "별모양" (최대 5) 화면에 추가 됩니다.

[![](uistackview-images/intro01.png "샘플 응용 프로그램 실행")](uistackview-images/intro01.png#lightbox)

"별표" 자동으로 가운데 맞춤 되며 가로 스택 보기에서 동일 하 게 분산 합니다. 사용자가 누를 때는 **등급 낮추기** , "별모양" 메일로 (될 때까지 남아 없음).

## <a name="stack-view-details"></a>스택 세부 정보 보기

기능에 대해 이해 했으므로 `UIStackView` 컨트롤의 정의와 기능 및 세부 정보 중 일부에 보겠습니다 작동 방법 및 합니다.

### <a name="auto-layout-and-size-classes"></a>자동 레이아웃 및 크기 클래스

위에서 설명한 것 처럼 하위 뷰에 추가 될 때 스택 뷰 레이아웃 위치를 지정 하 고 정렬 된 뷰 크기를 자동 레이아웃 및 크기 클래스를 사용 하 여 해당 스택 보기에 의해 완전히 제어 됩니다.

스택 뷰는 _pin_ 해당 컬렉션의 첫 번째 및 마지막 하위 뷰는 **Top** 및 **아래쪽** 세로 스택 뷰에 대 한 가장자리 또는 **왼쪽**및 **오른쪽** 가로 스택 뷰에 대 한 가장자리입니다. 설정 하는 경우는 `LayoutMarginsRelativeArrangement` 속성을 `true`, 다음 하위 가장자리 대신 관련 여백에 고정 하는 보기입니다.

스택 뷰는 하위 뷰를 사용 하 여 `IntrinsicContentSize` 속성 따라 정의 된 하위 크기를 계산할 때 `Axis` (제외 하 고는 `FillEqually Distribution`). `FillEqually Distribution` 동일한 크기가 되도록 모든 하위 뷰가 크기를 조정 하므로 따라 스택 뷰를 채우는 `Axis`합니다.

제외는 `Fill Alignment`, 스택 보기는 하위 뷰를 사용 하 여 `IntrinsicContentSize` 에 수직 보기의 크기를 계산 하는 것에 대 한 속성에서 지정 `Axis`합니다. 에 대 한는 `Fill Alignment`, 수직으로 스택 뷰를 채울 수 있도록 모든 하위 뷰가 크기는 주어진 `Axis`합니다.

### <a name="positioning-and-sizing-the-stack-view"></a>배치 및 스택 뷰 크기 조정

스택 보기에는 모든 하위 뷰 레이아웃 완전히 제어 하는 동안 (같은 속성에 따라 `Axis` 및 `Distribution`), 스택 뷰 위치를 지정 해야 (`UIStackView`) 자동 레이아웃 및 크기 클래스를 사용 하 여 부모 뷰에 내 합니다.

일반적으로이 확장 / 축소, 따라서 해당 위치를 정의 하는 데 스택 보기 두 개 이상의 가장자리 고정 의미 합니다. 모든 추가 제약 조건 없이 스택 뷰 자동으로 크기가 조정 됩니다에 맞게 모든 해당 하위 다음과 같습니다.

 - 크기에 따라 해당 `Axis` 모든 하위 뷰 크기 및 각 하위 뷰 간에 정의 된 모든 공간의 합계가 됩니다.
 - 경우는 `LayoutMarginsRelativeArrangement` 속성은 `true`, 스택 뷰 크기 여백을 위한 공간이 포함 됩니다.
 - 에 수직 크기는 `Axis` 컬렉션의 가장 큰 서브로 설정 됩니다.

스택 보기에 대 한 제약 조건을 지정할 수는 또한 **높이** 및 **너비**합니다. 이 경우에 하위 배치 됩니다 (크기) 기준으로 스택 뷰로 지정 된 공간을 채우기 위해는 `Distribution` 및 `Alignment` 속성입니다.

경우는 `BaselineRelativeArrangement` 속성은 `true`, 하위 뷰 들으로 사용 하는 대신 첫 번째 또는 마지막 서브 기준에 따라 배치 됩니다는 **Top**, **아래쪽** 또는 **센터* -  **Y** 위치입니다. 이러한 계산 스택 보기의 내용에 다음과 같이:

 - 세로 스택 뷰는 첫 번째 기준에 대 한 첫 번째 하위 뷰 및 지난 성을 반환 합니다. 이러한 하위 중 하나는 자체 스택 뷰, 첫 번째 또는 마지막 초기 사용 됩니다.
 - 가로 스택 뷰 첫 번째와 마지막 기준에 대 한 가장 큰 해당 하위 뷰를 사용 합니다. 가장 높은 보기 스택 보기 이기도 한 경우 가장 긴 서브 기준선으로 사용 됩니다.

> [!IMPORTANT]
> **참고:** 잘못 된 위치에 초기 계획을 계산할 때 기준선 맞춤 늘어난 또는 압축 된 하위 뷰 크기에서 작동 하지 않습니다. 기준선 맞춤에 대 한 확인 하는 하위 뷰 **높이** 내장 콘텐츠 보기와 일치 **높이**합니다.

### <a name="common-stack-view-uses"></a>일반적으로 스택 뷰 사용

스택 뷰 컨트롤에서 제대로 작동 레이아웃 형식은 여러 가지가 있습니다. 다음은 Apple에 따라 몇 가지 일반적인 사용입니다.

- **축을 the 함께 크기 정의** – 스택 보기와 함께 가장자리를 고정 하 여 `Axis` 및 해당 하위에 정의 된 공간에 맞도록 축 보기 크기가 계속 커집니다 스택 위치를 설정 하는 인접 한 가장자리 중 하나입니다.
 -  **하위 보기의 위치를 정의** – 부모 뷰를 스택 보기의 인접 한 가장자리를 고정 하 여 스택 뷰 크기 모두 포함 된 하위에 맞게 늘어납니다.
- **정의 크기와 위치는 스택의** – 스택 뷰는 부모 뷰에 스택 보기의 모든 네 가장자리를 고정 하 여 스택 뷰 내에 정의 된 공간에 따라 하위를 정렬 합니다.
 -  **정의 된 크기 수직 축** – 스택 뷰 모두 가장자리 수직으로 고정 하 여 `Axis` 보기에 정의 된 공간에 맞도록 축에 수직 늘어나는지 스택 위치를 설정 하려면 축 따라 가장자리 중 하나를 해당 하위 뷰가 있습니다.

### <a name="managing-the-appearance"></a>모양 관리

`UIStackView` 기능은의 다른 하위 클래스와 같은 캔버스에 그릴 없는 비 렌더링 컨테이너 보기로 이동 하 고 따라서 `UIView`합니다. 와 같은 속성을 설정 `BackgroundColor` 또는 재정의 `DrawRect` 시각적 영향을 미치지 것입니다.

스택 보기는 하위 뷰가의 컬렉션을 배열 하는 방법을 제어 하는 여러 속성이 있습니다.

- **축** – 스택 뷰 하위 뷰 들을 하거나 정렬 하는 경우 결정 **가로로** 또는 **세로로**합니다.
- **맞춤** –는 하위 스택 뷰 내에서 정렬 되는 방법을 제어 합니다.
- **배포** – 스택 뷰 내에서 하위 뷰 들 크기를 조정 하는 방법을 제어 합니다.
- **간격** – 스택 뷰에서 하위 각 뷰 사이의 최소 간격을 제어 합니다.
- **초기 상대** – 경우 `true`, 각 하위 보기의 세로 간격의 초기 계획에서 파생 됩니다.
- **레이아웃 여백 상대** – 하위 뷰 들 표준 레이아웃에 상대적으로 배치 합니다.

일반적으로 적은 수의 하위에 정렬 하는 스택 뷰를 사용 합니다. 하나 이상의 스택 뷰 각각의 내부에 중첩 하 여 더욱 복잡 한 사용자 인터페이스를 만들 수 있습니다 (에서 같이 [UIStackView 퀵 스타트](#UIStackView-Quickstart) 위에).

추가 제약 조건 (예: 높이 또는 너비 제어)를 하위에 추가 하 여 Ui 모양을 미세 조정할 수 있습니다. 그러나 주의 해야를 자체 스택 보기에 의해 발생 하는 충돌 하는 제약 조건을 포함 하지 않도록 합니다.

### <a name="maintaining-arranged-views-and-sub-views-consistency"></a>보기와 하위 뷰 일관성 정렬 유지 관리

스택 뷰로 됩니다 되도록 해당 `ArrangedSubviews` 속성의 하위 집합은 항상 해당 `Subviews` 다음 규칙을 사용 하 여 속성:

 - 하위 뷰 추가 되는 `ArrangedSubviews` 컬렉션을 자동으로 추가 됩니다에 `Subviews` 컬렉션 (않은 경우 이미 해당 컬렉션에 포함).
 - 하위 뷰에서 제거 된 경우는 `Subviews` (표시에서 제거) 하는 컬렉션에서 제거 됩니다는 `ArrangedSubviews` 컬렉션입니다.
 - 하위 뷰 제거는 `ArrangedSubviews` 컬렉션에서 제거 되지 않습니다는 `Subviews` 컬렉션입니다. 따라서 더 이상 레이아웃 되 스택 보기, 하지만 계속 화면에 표시 됩니다.

그러나 `ArrangedSubviews` 컬렉션의 하위 집합은 항상는 `Subview` 컬렉션 각 컬렉션 내에서 개별 하위 순서는 다음에 의해 제어 되 고 별도:

 - 내에서 하위 순서는 `ArrangedSubviews` 컬렉션은 스택 내에서 표시 순서를 결정 합니다.
 - 내에서 하위 순서는 `Subview` 뒤에서 앞으로 보기 내에서 Z 순서 (또는 쌓기)를 결정 하는 컬렉션입니다.

### <a name="dynamically-changing-content"></a>동적으로 변경 내용

자동으로 스택 보기는 하위 뷰 추가, 제거 또는 숨겨진 될 때마다 하위 뷰 들의 레이아웃을 조정 됩니다. 레이아웃 스택 보기의 속성을 조정 하는 경우에 조정 될 (예: 해당 `Axis`).

예를 들어 애니메이션 블록 내에서 배치 하 여 애니메이션을 레이아웃 변경 내용은 적용할 수 있습니다.:

```csharp
// Animate stack
UIView.Animate(0.25, ()=>{
    // Adjust stack view
    RatingView.LayoutIfNeeded();
});
```

스택 보기의 속성 중 상당수에 스토리 보드 내의 크기 클래스를 사용 하 여 지정할 수 있습니다. 이러한 속성은 자동으로 됩니다 크기나 방향 변경 내용에 대 한 응답은 애니메이션 효과가 적용 합니다.

## <a name="summary"></a>요약

이 문서의 새 검사가 수행 `UIStackView` Xamarin.iOS 앱에서 가로 또는 세로로 정렬 스택의 하위 집합을 관리할 (iOS에 대 한 9)를 제어 합니다.
스택 뷰를 사용 하 여 UI를 만드는의 간단한 예제로 시작 하 고 스택 뷰 및 해당 속성 및 기능에 대해 보다 자세한 완료 합니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0에 새로 이란](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [UIStackView 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/)
- [소개 UIStackView (비디오)](https://university.xamarin.com/lightninglectures/introducing-uistackview-on-ios9)
