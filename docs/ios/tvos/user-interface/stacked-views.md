---
title: Xamarin에서 tvOS 누적 보기 사용
description: 이 문서에서는 Xamarin으로 빌드된 앱에서 tvOS 누적 보기를 사용 하는 방법을 설명 합니다. 누적 보기에 대 한 개략적인 개요를 제공 하 고 자동 레이아웃, 누적 보기의 위치 지정 및 크기 조정, 일반적인 사용, 스토리 보드와의 통합 등에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 00B07F85-F30B-4DD4-8664-A61D0A1CDB0E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 646a26b4c9b2d44595315a6a32b7294b18c42d7a
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648957"
---
# <a name="working-with-tvos-stacked-views-in-xamarin"></a>Xamarin에서 tvOS 누적 보기 사용

Stack View control (`UIStackView`)은 Auto Layout 및 Size 클래스의 강력한 기능을 활용 하 여 Apple TV 장치의 콘텐츠 변경 내용 및 화면 크기에 동적으로 응답 하는 가로 또는 세로로 하위 뷰 스택을 관리 합니다.

스택 보기에 연결 된 모든 하위 뷰의 레이아웃은 축, 분포, 맞춤 및 간격과 같은 개발자 정의 속성을 기반으로 하 여 관리 됩니다.

[![](stacked-views-images/stacked01.png "하위 뷰 레이아웃 다이어그램")](stacked-views-images/stacked01.png#lightbox)

TvOS 앱에서 `UIStackView` 를 사용 하는 경우 개발자는 iOS 디자이너의 스토리 보드 내에서 또는 코드에서 C# 하위 뷰를 추가 및 제거 하 여 하위 뷰를 정의할 수 있습니다.

## <a name="about-stacked-view-controls"></a>누적 보기 컨트롤 정보 

는 `UIStackView` 비 렌더링 컨테이너 뷰로 설계 되었으므로의 `UIView`다른 서브 클래스와 같이 캔버스로 그려지지 않습니다. 등 `BackgroundColor` 의 속성을 설정 하거나 `DrawRect` 재정의 하면 시각적 효과가 없습니다.

스택 뷰에서 하위 뷰의 컬렉션을 정렬 하는 방법을 제어 하는 몇 가지 속성이 있습니다.

- **축** – 스택 뷰에서 하위 뷰를 **가로** 또는 **세로로**정렬 하는지 결정 합니다.
- **Alignment** – 스택 뷰 내에서 하위 뷰를 정렬 하는 방법을 제어 합니다.
- **분포** – 스택 보기 내에서 하위 뷰 크기를 조정 하는 방법을 제어 합니다.
- **간격** – 스택 뷰에서 각 하위 뷰 사이의 최소 공간을 제어 합니다.
- 기준 **기준** – 이면 `true`각 하위 뷰의 세로 간격이 해당 기준선에서 파생 됩니다.
- **레이아웃 여백 비례** – 표준 레이아웃 여백에 상대적으로 하위 뷰을 배치 합니다.

일반적으로 스택 뷰를 사용 하 여 적은 수의 하위 뷰를 정렬 하 게 됩니다. 하나 이상의 스택 뷰를 서로 중첩 하 여 더 복잡 한 사용자 인터페이스를 만들 수 있습니다.

하위 뷰에 제약 조건을 추가 하 여 (예를 들어 높이 또는 너비 제어) Ui 모양을 세부적으로 조정할 수 있습니다. 그러나 스택 뷰 자체에서 도입 된 제약 조건에 충돌 하는 제약 조건을 포함 하지 않도록 주의 해야 합니다.

<a name="Auto-Layout-and-Size-Classes" />

## <a name="auto-layout-and-size-classes"></a>자동 레이아웃 및 크기 클래스

하위 뷰가 스택 뷰에 추가 될 때 해당 레이아웃은 자동 레이아웃 및 크기 클래스를 사용 하 여 정렬 된 뷰의 위치 및 크기를 조정 하는 해당 스택 뷰에서 완전히 제어 됩니다.

스택 뷰에서는 컬렉션의 첫 번째 및 마지막 **하위** 뷰를 세로 스택 보기의 **위쪽** 및 아래쪽 가장자리에 고정 하거나 가로 스택 보기의 **왼쪽** 및 **오른쪽** 가장자리에 _고정_ 합니다. `LayoutMarginsRelativeArrangement` 속성을로 `true`설정 하면 뷰가 가장자리가 아닌 관련 여백에 하위 뷰를 고정 합니다.

스택 뷰는 정의 `IntrinsicContentSize` `Axis` 된를 따라 하위 뷰 크기를 계산할 때 하위 뷰 속성을 사용 합니다 (제외 `FillEqually Distribution`). 는 `FillEqually Distribution` 크기가 동일 하도록 모든 하위 뷰의 크기를 조정 하므로를 `Axis`따라 스택 뷰를 채웁니다.

예외를 제외 `Fill Alignment`하 고 스택 뷰에서는 지정 `Axis`된에 대 한 `IntrinsicContentSize` 뷰의 크기를 계산 하기 위해 하위 뷰의 속성을 사용 합니다. 의 경우 모든 하위 뷰은 지정 된와 수직인 스택 뷰를 채우도록 크기가 지정 `Axis`됩니다. `Fill Alignment`

<a name="Positioning-and-Sizing-the-Stack-View" />

## <a name="positioning-and-sizing-the-stack-view"></a>스택 보기 위치 지정 및 크기 조정

스택 보기에는 및 `Axis` `Distribution`와 같은 속성을 기반으로 하는 하위 뷰의 레이아웃에 대 한 전체 제어 권한이 있지만 Auto layout 및 Size 클래스를 사용 하`UIStackView`여 부모 뷰 내에 스택 뷰 ()를 배치 해야 합니다.

일반적으로이는 스택 뷰의 가장자리를 두 개 이상 고정 하 여 확장 및 축소 하 여 해당 위치를 정의 함을 의미 합니다. 추가 제약 조건이 없는 경우 스택 보기는 다음과 같이 모든 하위 뷰에 맞게 자동으로 크기가 조정 됩니다.

* 크기는 모든 하위 `Axis` 뷰 크기와 각 하위 뷰 사이에 정의 된 공간을 더한 값이 됩니다.
* `LayoutMarginsRelativeArrangement` 속성이이면스택보기크기에도`true`여백의 공간이 포함 됩니다.
* 에 수직인 `Axis` 크기는 컬렉션에서 가장 큰 서브 집합으로 설정 됩니다.

또한 스택 뷰의 **높이** 및 **너비**에 대 한 제약 조건을 지정할 수 있습니다. 이 경우 하위 뷰는 `Distribution` 및 `Alignment` 속성에 의해 결정 되는 스택 뷰에 지정 된 공간을 채우도록 (크기 조정) 배치 됩니다.

-   속성이 이면 하위 뷰는 Top, bottom 또는 * Center Y 위치를 사용 하는 대신 첫 번째 또는 마지막 하위 뷰의 기준선을 기준으로 배치 됩니다. `true` `BaselineRelativeArrangement` 이러한 항목은 스택 뷰의 콘텐츠에서 다음과 같이 계산 됩니다.

* 세로 스택 뷰는 첫 번째 기준선에 대해 첫 번째 하위 뷰를 반환 하 고 마지막에는 마지막 하위 뷰를 반환 합니다. 이러한 하위 뷰 중 하나가 스택 보기 이면 첫 번째 또는 마지막 기준이 사용 됩니다.
* 가로 스택 보기는 첫 번째 및 마지막 기준선 모두에 가장 긴 하위 뷰를 사용 합니다. 가장 긴 뷰도 스택 보기 이면 가장 긴 하위 뷰를 기준선으로 사용 합니다.

> [!IMPORTANT]
> 기준선 맞춤은 잘못 된 위치로 계산 되기 때문에 스트레치 되었거나 압축 된 하위 뷰 크기에서는 작동 하지 않습니다. 기준선 맞춤의 경우 하위 뷰의 **높이가** 내장 콘텐츠 뷰의 **높이**와 일치 하는지 확인 합니다.




<a name="Common-Stack-View-Uses" />

## <a name="common-stack-view-uses"></a>공용 스택 뷰 사용

스택 뷰 컨트롤에서 잘 작동 하는 몇 가지 레이아웃 유형이 있습니다. Apple에 따라 다음과 같은 몇 가지 일반적인 사용 방법에 대해 살펴보겠습니다.

- **축을 따라 크기를 정의** 합니다. 스택 보기의 `Axis` 가장자리와 인접 한 가장자리 중 하나를 고정 하 여 위치를 설정 하면 스택 뷰가 축을 따라 하위 뷰에서 정의 된 공간에 맞게 늘어납니다.
*  하위 뷰의 **위치 정의** – 부모 뷰에 스택 뷰의 인접 한 가장자리에 고정 하 여 하위 뷰 포함 하는 크기에 맞게 스택 뷰를 늘립니다.
- 스택의 **크기 및 위치를 정의** 합니다. 스택 보기의 네 가지 모든 가장자리를 부모 뷰에 고정 하 여 스택 뷰에서 스택 보기 내에 정의 된 공간을 기준으로 하위 뷰을 정렬 합니다.
*  **축에 수직인 크기 정의** – 스택 보기 `Axis` 에 수직인 가장자리와 축을 따라 가장자리 중 하나를 고정 하 여 위치를 설정 하면 스택 뷰가 축에 수직으로 증가 하 여 하위 뷰에 의해 정의 된 공간에 맞게 조정 됩니다.

<a name="Stack-Views-and-Storyboards" />

## <a name="stack-views-and-storyboards"></a>스택 뷰 및 Storyboard

TvOS 앱에서 스택 뷰를 사용 하는 가장 쉬운 방법은 iOS Designer를 사용 하 여 앱의 UI에 추가 하는 것입니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**에서 `Main.storyboard` 파일을 두 번 클릭 하 여 편집을 위해 엽니다.
1. 스택 보기에 추가 하려는 개별 요소의 레이아웃을 디자인 합니다. 

    [![](stacked-views-images/layout01.png "요소 레이아웃 예제")](stacked-views-images/layout01.png#lightbox)
1. 요소에 필요한 제약 조건을 추가 하 여 올바르게 확장 되도록 합니다. 요소가 스택 뷰에 추가 되 면이 단계가 중요 합니다.
1. 필요한 수의 복사본을 만듭니다 (이 경우 4). 

    [![](stacked-views-images/layout02.png "필요한 복사본 수")](stacked-views-images/layout02.png#lightbox)
1. **도구 상자** 에서 **스택 뷰** 를 끌어 뷰에 놓습니다. 

    [![](stacked-views-images/layout03.png "스택 뷰")](stacked-views-images/layout03.png#lightbox)
1. 스택 뷰를 선택 하 고, **Properties Pad** 의 **위젯 탭** 에서 **맞춤**에 대해 **채우기** 를 선택 하 고, **균등** 하 게 입력 `25` **하 고,** **간격**을 입력 합니다. 

    [![](stacked-views-images/layout04.png "위젯 탭")](stacked-views-images/layout04.png#lightbox)
1. 화면에서 원하는 위치에 스택 보기를 배치 하 고 필요한 위치에 유지 하기 위한 제약 조건을 추가 합니다.
1. 개별 요소를 선택 하 여 스택 보기로 끌어 옵니다. 

    [![](stacked-views-images/layout05.png "스택 뷰의 개별 요소")](stacked-views-images/layout05.png#lightbox)
1. 위에서 설정한 특성에 따라 레이아웃이 조정 되 고 요소가 스택 뷰에 정렬 됩니다.
1. 코드에서 C# UI 컨트롤을 사용 하려면 **속성 탐색기** 의 **위젯 탭** 에서 **이름을** 할당 합니다.
1. 변경 내용을 저장합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **솔루션 탐색기**에서 `Main.storyboard` 파일을 두 번 클릭 하 여 편집을 위해 엽니다.
1. 스택 보기에 추가 하려는 개별 요소의 레이아웃을 디자인 합니다. 

    [![](stacked-views-images/layout01.png "예제 요소 레이아웃")](stacked-views-images/layout01.png#lightbox)
1. 요소에 필요한 제약 조건을 추가 하 여 올바르게 확장 되도록 합니다. 요소가 스택 뷰에 추가 되 면이 단계가 중요 합니다.
1. 필요한 수의 복사본을 만듭니다 (이 경우 4). 

    [![](stacked-views-images/layout02.png "필요한 복사본 수")](stacked-views-images/layout02.png#lightbox)
1. **도구 상자** 에서 **스택 뷰** 를 끌어 뷰에 놓습니다. 

    [![](stacked-views-images/layout03-vs.png "스택 뷰")](stacked-views-images/layout03-vs.png#lightbox)
1. 스택 뷰를 선택 하 고, **속성 탐색기** 의 **위젯 탭** 에서 맞춤 **을 선택 하** 고, **균등** 하 게 입력 **하 고** , `25` **간격**을 입력 합니다. 

    [![](stacked-views-images/layout04-vs.png "위젯 탭")](stacked-views-images/layout04-vs.png#lightbox)
1. 화면에서 원하는 위치에 스택 보기를 배치 하 고 필요한 위치에 유지 하기 위한 제약 조건을 추가 합니다.
1. 개별 요소를 선택 하 여 스택 보기로 끌어 옵니다. 

    [![](stacked-views-images/layout05-vs.png "스택 뷰의 개별 요소")](stacked-views-images/layout05-vs.png#lightbox)
1. 위에서 설정한 특성에 따라 레이아웃이 조정 되 고 요소가 스택 뷰에 정렬 됩니다.
1. 코드에서 C# UI 컨트롤을 사용 하려면 **속성 탐색기** 의 **위젯 탭** 에서 **이름을** 할당 합니다.
1. 변경 내용을 저장합니다.

-----

> [!IMPORTANT]
> 이벤트 처리기를 만들 때 iOS 디자이너에서 UI `TouchUpInside` 요소 (예: `UIButton`)와 같은 작업을 할당 하는 것은 가능 하지만, Apple TV에 터치 스크린이 없거나 터치 이벤트가 지원 되기 때문에 호출 되지 않습니다. TvOS 사용자 인터페이스 요소에 대 `Action Type` 한 작업을 만들 때 항상 기본값을 사용 해야 합니다.

스토리 보드 사용에 대 한 자세한 내용은 [Hello, tvOS 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md)를 참조 하세요.

이 예제의 경우 세그먼트 컨트롤에 대 한 유출 및 작업과 각 "플레이어 카드"에 대 한 콘센트가 제공 됩니다. 코드에서 현재 세그먼트를 기준으로 플레이어를 숨기고 표시 합니다. 예를 들어:

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

앱이 실행 될 때 4 개의 요소가 스택 보기에 동일 하 게 배포 됩니다.

[![](stacked-views-images/layout06.png "앱이 실행 될 때 4 개의 요소가 스택 보기에 동일 하 게 배포 됩니다.")](stacked-views-images/layout06.png#lightbox)

플레이어 수가 줄어들면 사용 하지 않는 보기가 숨겨지고 스택 보기에서 레이아웃을 조정 합니다.

[![](stacked-views-images/layout07.png "플레이어 수가 줄어들면 사용 하지 않는 보기가 숨겨지고 스택 보기에서 레이아웃을 조정 합니다.")](stacked-views-images/layout07.png#lightbox)

<a name="Populate-a-Stack-View-from-Code" />

### <a name="populate-a-stack-view-from-code"></a>코드에서 스택 뷰 채우기

IOS 디자이너에서 스택 보기의 내용과 레이아웃을 완전히 정의 하는 것 외에도 코드에서 C# 동적으로 만들고 제거할 수 있습니다.

스택 뷰를 사용 하 여 검토에서 "별모양"을 처리 하는 다음 예제를 사용 합니다 (1-5).

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

이 코드에 대 한 몇 가지 자세한 내용을 살펴보겠습니다. 첫째, `if` 문을 사용 하 여 5 개 이하의 "별" 또는 0 보다 작은 것이 없는지 확인 합니다.

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

`UIImageView` 가 아닌 `UIStackView` 의`ArrangedSubviews`속성 에를 추가 했습니다. `SubView` 스택 보기에서 레이아웃을 제어 하는 뷰를 `ArrangedSubviews` 속성에 추가 해야 합니다.

스택 뷰에서 하위 뷰를 제거 하려면 먼저 제거할 하위 뷰를 가져옵니다.

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

그런 다음 컬렉션과 슈퍼 뷰에서이를 제거 해야 `ArrangedSubviews` 합니다.

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

단지 `ArrangedSubviews` 컬렉션에서 하위 뷰를 제거 하면 스택 뷰의 컨트롤이 아니라 화면에서 하위 뷰를 제거 하지 않습니다.

<a name="Dynamically-Changing-Content" />

## <a name="dynamically-changing-content"></a>동적으로 콘텐츠 변경

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

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 tvOS 앱 내부에서 누적 보기를 디자인 하 고 작업 하는 방법에 대해 설명 했습니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
