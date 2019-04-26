---
title: Xamarin에서 tvOS 누적 뷰 작업
description: 이 문서에서는 Xamarin으로 빌드된 앱에서 누적된 뷰 tvOS를 사용 하 여 작동 하는 방법을 설명 합니다. 누적 세로 보기의 대략적인 개요를 제공 하 고 자동 레이아웃을 배치 및 누적 세로 보기, 일반적인 사용, 스토리 보드를 사용 하 여 통합 등을 크기 조정에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 00B07F85-F30B-4DD4-8664-A61D0A1CDB0E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: f51ed3d6dbbfc8a7e430c2949485838a7471e545
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61161460"
---
# <a name="working-with-tvos-stacked-views-in-xamarin"></a>Xamarin에서 tvOS 누적 뷰 작업

스택 뷰 컨트롤 (`UIStackView`) 활용 자동 레이아웃 및 크기 클래스 가로 또는 세로로 하위 스택을 관리 하려면 콘텐츠 변경 내용 및 Apple TV 장치의 화면 크기를 동적으로 응답 합니다.

스택 보기에 연결 하는 모든 하위 뷰의 레이아웃을 축, 배포, 맞춤 간격 등 개발자 정의 속성에 따라 관리 합니다.

[![](stacked-views-images/stacked01.png "하위 뷰 레이아웃 다이어그램")](stacked-views-images/stacked01.png#lightbox)

사용 하는 경우는 `UIStackView` 개발자 Xamarin.tvOS 앱에서 하거나 iOS 디자이너 또는 추가 하 고에서 하위 뷰를 제거 하 여 스토리 보드 내에서 하위 뷰를 정의 하거나 수 C# 코드입니다.

## <a name="about-stacked-view-controls"></a>누적된 뷰 컨트롤에 대 한 

`UIStackView` 은 다른 하위 클래스와 같은 캔버스에 그릴 없는 비 렌더링 컨테이너 보기 및 이와 같이 `UIView`합니다. 와 같은 속성을 설정 `BackgroundColor` 재정의 또는 `DrawRect` visual 영향을 주지 것입니다.

가지 스택 보기가 하위 컬렉션을 조정 하는 방법을 제어 하는 몇 가지 속성이 있습니다.

- **축** – 스택 뷰 하위 뷰를 하거나 정렬 하는 경우 결정 **가로로** 또는 **세로로**합니다.
- **맞춤** – 스택 보기 내에서 하위 뷰는 정렬 하는 방법을 제어 합니다.
- **배포** – 스택 뷰 내에 있는 하위 뷰 크기를 조정 하는 방법을 제어 합니다.
- **간격** – 스택 보기에 각 하위 뷰 사이의 최소 공간을 제어 합니다.
- **기준 상대** – `true`, 각 하위 보기의 세로 간격의 기준에서 파생 됩니다.
- **레이아웃 여백 상대** – 표준 레이아웃 여백을 기준으로 하위 뷰를 배치 합니다.

일반적으로 소수의 하위 뷰를 정렬 하는 스택 뷰를 사용 합니다. 하나 이상의 스택 뷰 서로의 내부에 중첩 하 여 더 복잡 한 사용자 인터페이스를 만들 수 있습니다.

추가 제약 조건 (예: 컨트롤의 높이 또는 너비)를 하위 뷰를 추가 하 여 추가 Ui 모양을 미세 조정할 수 있습니다. 그러나 주의 해야를 자체 스택 보기에 의해 발생 하는 충돌 하는 제약 조건을 포함 하지 않도록 합니다.

<a name="Auto-Layout-and-Size-Classes" />

## <a name="auto-layout-and-size-classes"></a>자동 레이아웃 및 크기 클래스

스택 뷰 하위 뷰를 추가할 때 해당 레이아웃을 배치 하 고 정렬 된 뷰 크기를 자동 레이아웃 및 Size 클래스를 사용 하 여 해당 스택 보기에 의해 완전히 제어 됩니다.

스택 보기는 _pin_ 해당 컬렉션의 첫 번째 및 마지막 하위 뷰를 **위쪽** 하 고 **아래쪽** 세로 스택 보기에 대 한 가장자리 또는 **왼쪽**하 고 **오른쪽** 가로 스택 보기에 대 한 가장자리입니다. 설정 하는 경우는 `LayoutMarginsRelativeArrangement` 속성을 `true`, 지 하는 대신 관련 여백 하위 뷰를 고정 하는 뷰를 합니다.

스택 보기는 하위 뷰를 사용 하 여 `IntrinsicContentSize` 속성의 정의 따라 하위 뷰 크기를 계산할 때 `Axis` (제외 하 고는 `FillEqually Distribution`). `FillEqually Distribution` 크기가 동일 수 있도록 모든 하위 뷰 크기를 조정 하므로 상의 스택 뷰를 채우는 `Axis`합니다.

경우는 예외를 `Fill Alignment`, 스택 보기는 하위 뷰를 사용 하 여 `IntrinsicContentSize` 수직으로 보기의 크기를 계산 하는 것에 대 한 속성을 지정 `Axis`. 에 대 한 합니다 `Fill Alignment`를 수직으로 스택 뷰를 채우도록 모든 하위 크기가 조정 되는 지정 된 `Axis`합니다.

<a name="Positioning-and-Sizing-the-Stack-View" />

## <a name="positioning-and-sizing-the-stack-view"></a>위치 및 스택 뷰 크기 조정

스택 보기에는 총 제어의 모든 하위 뷰 레이아웃에 있는 동안 (같은 속성에 따라 `Axis` 및 `Distribution`), 스택 보기를 배치 해야 (`UIStackView`) 자동 레이아웃 및 Size 클래스를 사용 하 여 부모 뷰에 내입니다.

일반적으로이 두 개 이상의 가장자리 확장 / 축소, 따라서 해당 위치를 정의 하는 데 스택 보기 고정 의미 합니다. 추가 제약 조건이 없는 스택 보기는 자동으로 크기를 조정할 수에 맞게 모든 해당 하위 같이:

* 크기에 따라 해당 `Axis` 모든 하위 뷰 크기 및 각 하위 뷰 간에 정의 된 모든 공간 합계 됩니다.
* 경우는 `LayoutMarginsRelativeArrangement` 속성은 `true`, 스택 뷰 크기 여백에 대 한 공간이 포함 됩니다.
* 수직으로 크기를 `Axis` 컬렉션의 가장 큰 서브로 설정 됩니다.

스택 보기에 대 한 제약 조건을 지정할 수는 또한 **높이** 하 고 **너비**합니다. 이 경우 하위 뷰 배치 됩니다 (크기)에 따른 스택 보기에 의해 지정 된 공간에 맞게 합니다 `Distribution` 및 `Alignment` 속성입니다.

경우는 `BaselineRelativeArrangement` 속성은 `true`, 하위 뷰는 배치를 사용 하는 대신 첫 번째 또는 마지막 서브의 기준에 따라 합니다 **위쪽**, **아래쪽** 또는 **Center* -  **Y** 위치 합니다. 스택 보기의 내용에 다음과 같이 계산 된 이러한:

* 세로 스택 뷰는 첫 번째 기준선에 대 한 첫 번째 서브 및 마지막에 대 한 마지막에 반환 됩니다. 이러한 하위 뷰 중 하나는 스택 뷰 자체에 해당 첫 번째 또는 마지막 기준 사용 됩니다.
* 첫 번째 및 마지막 기준에 대 한 가로 스택 뷰를 채우도록 맞춰지거나 해당 하위 뷰를 사용 합니다. 가장 높은 보기 스택 보기도 인 경우 가장 긴 서브 기준선으로 사용 됩니다.

> [!IMPORTANT]
> 잘못 된 위치로 기준선을 계산 하는 대로 기준선 맞춤 확대 또는 압축 된 하위 뷰 크기에서 작동 하지 않습니다. 기준선 맞춤에 대 한 확인을 하위 뷰 **높이** 내장 콘텐츠 뷰의 일치 **높이**합니다.




<a name="Common-Stack-View-Uses" />

## <a name="common-stack-view-uses"></a>일반적으로 스택 뷰 사용

스택 뷰 컨트롤에서 잘 작동 하는 레이아웃 형식은 여러 가지입니다. Apple에 따라 보다 일반적인 사용의 몇 가지 다음과 같습니다.

- **축 the 따라 크기를 정의** – 스택 뷰의 따라에 지 모두 고정 `Axis` 및 뷰 하위 뷰를 정의한 공간에 맞게 축을 따라 커집니다 스택의 위치를 설정 하려면 인접 한 가장자리 중 하나입니다.
*  **하위 뷰의 위치를 정의** – 부모 보기에 스택 보기의 인접 한 가장자리에 고정 하 여 스택 보기에 포함 된 하위에 맞게 크기를 모두 커집니다.
- **크기와 위치 스택의 정의** – 스택 뷰는 부모 보기에 스택 보기의 모든 가장자리에 고정 하 여 스택 뷰 내에 정의 된 공간에 따라 하위 뷰를 정렬 합니다.
*  **정의 된 크기 수직 축** – 모두 가장자리 수직 스택 보기에 고정 하 여 `Axis` 보기 정의한 공간에 맞도록 축과 수직 커집니다 스택의 위치를 설정 하려면 축 따라 가장자리 중 해당 하위 뷰입니다.

<a name="Stack-Views-and-Storyboards" />

## <a name="stack-views-and-storyboards"></a>스택 뷰 및 스토리 보드

Xamarin.tvOS 앱에서 스택 뷰를 사용 하는 가장 쉬운 방법은 iOS 디자이너를 사용 하 여 앱의 UI에 추가할 경우

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 에 **Solution Pad**을 `Main.storyboard` 파일을 편집용으로 엽니다.
1. 스택 보기를 추가 하는 것에 개별 요소의 레이아웃을 디자인 하는: 

    [![](stacked-views-images/layout01.png "요소 레이아웃 예제")](stacked-views-images/layout01.png#lightbox)
1. 모든 필요한 제약 조건을 올바르게 크기 조정 되도록 요소에 추가 합니다. 요소 스택 보기에 추가 되 면이 단계는 중요 합니다.
1. 필요한 개수의 복사본 (이 경우 4 개)를 확인 합니다. 

    [![](stacked-views-images/layout02.png "필요한 개수의 복사본")](stacked-views-images/layout02.png#lightbox)
1. 끌어서를 **스택 보기** 에서 **도구 상자** 보기에 놓습니다. 

    [![](stacked-views-images/layout03.png "스택 보기")](stacked-views-images/layout03.png#lightbox)
1. 스택 뷰를 선택 합니다 **위젯을 탭** 의 **Properties Pad** 선택 **채우기** 에 대 한를 **맞춤**, **채우기 동일 하 게** 에 대 한 합니다 **배포** 입력 `25` 에 대 한 합니다 **간격**: 

    [![](stacked-views-images/layout04.png "위젯 탭")](stacked-views-images/layout04.png#lightbox)
1. 원하는 하 고 필요한 위치를 유지 하도록 제약 조건을 추가할 화면의 스택 보기를 배치 합니다.
1. 개별 요소를 선택 하 고 스택 뷰로 끌어: 

    [![](stacked-views-images/layout05.png "스택 보기에 개별 요소")](stacked-views-images/layout05.png#lightbox)
1. 레이아웃을 조정할 수는 및 요소 위에서 설정한 특성을 기준으로 스택 보기에 정렬 됩니다.
1. 할당 **이름을** 에 **위젯을 탭** 의 **속성 탐색기** 에서 UI 컨트롤을 사용 하 C# 코드입니다.
1. 변경 내용을 저장합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 에 **솔루션 탐색기**을 `Main.storyboard` 파일을 편집용으로 엽니다.
1. 스택 보기를 추가 하는 것에 개별 요소의 레이아웃을 디자인 하는: 

    [![](stacked-views-images/layout01.png "요소 레이아웃의 예")](stacked-views-images/layout01.png#lightbox)
1. 모든 필요한 제약 조건을 올바르게 크기 조정 되도록 요소에 추가 합니다. 요소 스택 보기에 추가 되 면이 단계는 중요 합니다.
1. 필요한 개수의 복사본 (이 경우 4 개)를 확인 합니다. 

    [![](stacked-views-images/layout02.png "필요한 개수의 복사본")](stacked-views-images/layout02.png#lightbox)
1. 끌어서를 **스택 보기** 에서 **도구 상자** 보기에 놓습니다. 

    [![](stacked-views-images/layout03-vs.png "스택 보기")](stacked-views-images/layout03-vs.png#lightbox)
1. 스택 뷰를 선택 합니다 **위젯을 탭** 의 **속성 탐색기** 선택 **채우기** 에 대 한를 **맞춤**, **채우기 동일 하 게** 에 대 한 합니다 **배포** 입력 `25` 에 대 한 합니다 **간격**: 

    [![](stacked-views-images/layout04-vs.png "위젯 탭")](stacked-views-images/layout04-vs.png#lightbox)
1. 원하는 하 고 필요한 위치를 유지 하도록 제약 조건을 추가할 화면의 스택 보기를 배치 합니다.
1. 개별 요소를 선택 하 고 스택 뷰로 끌어: 

    [![](stacked-views-images/layout05-vs.png "스택 보기에 개별 요소")](stacked-views-images/layout05-vs.png#lightbox)
1. 레이아웃을 조정할 수는 및 요소 위에서 설정한 특성을 기준으로 스택 보기에 정렬 됩니다.
1. 할당 **이름을** 에 **위젯을 탭** 의 **속성 탐색기** 에서 UI 컨트롤을 사용 하 C# 코드입니다.
1. 변경 내용을 저장합니다.

-----

> [!IMPORTANT]
> 와 같은 작업을 할당할 수 있지만 `TouchUpInside` UI 요소에 (같은 `UIButton`) iOS 디자이너에 있는 이벤트 처리기를 만들 때,이 호출 되지 것입니다 Apple TV 화면 또는 터치 이벤트를 지 원하는 터치 없기 때문입니다. 항상 기본값을 사용 해야 `Action Type` 사용자 인터페이스 요소 tvOS에 대 한 작업을 만들 때.

스토리 보드를 사용 하 여 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Tvos 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md)합니다.

이 예의 경우는 출 선 및 세그먼트 컨트롤에 대 한 작업 및 "플레이어 카드" 각 출 선 공개 했습니다. 코드에서 숨기기 하 고 현재 세그먼트를 기반으로 하는 플레이어를 표시 합니다. 예를 들어:

```csharp
partial void PlayerCountChanged (Foundation.NSObject sender) {

    // Take Action based on the segment
    switch(PlayerCount.SelectedSegment) {
    case 0:
        Player1.Hidden = false;
        Player2.Hidden = true;
        Player3.Hidden = true;
        Player4.Hidden = true;
        break;
    case 1:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = true;
        Player4.Hidden = true;
        break;
    case 2:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = false;
        Player4.Hidden = true;
        break;
    case 3:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = false;
        Player4.Hidden = false;
        break;
    }
}
```

앱이 실행 되는 4 개의 요소가 스택 뷰에 동일 하 게 분산 됩니다.

[![](stacked-views-images/layout06.png "앱을 실행 하는 경우 4 개 요소가 동일 하 게 배포 되는데 스택 보기")](stacked-views-images/layout06.png#lightbox)

플레이어 수를 줄이면 사용 하지 않는 뷰는 숨겨져 있으며, 스택 보기에 맞게 레이아웃을 조정 합니다.

[![](stacked-views-images/layout07.png "플레이어의 수를 줄이면 사용 하지 않는 뷰는 숨겨져 있으며에 맞게 레이아웃을 조정 하는 스택 보기")](stacked-views-images/layout07.png#lightbox)

<a name="Populate-a-Stack-View-from-Code" />

### <a name="populate-a-stack-view-from-code"></a>코드에서 스택 뷰 채우기

IOS 디자이너에 있는 스택 보기의 레이아웃과 콘텐츠를 완전히 정의 하는 것 외에도 만들기 및에서 동적으로 제거할 수 있습니다 C# 코드입니다.

스택 보기를 사용 하 여 검토 (1-5)에서 "별"를 처리 하는 다음 예제를 수행 합니다.

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

<a name="Dynamically-Changing-Content" />

## <a name="dynamically-changing-content"></a>콘텐츠를 동적으로 변경

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

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 디자인 및 보기 누적 Xamarin.tvOS 앱 내에서 사용 설명 했습니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
