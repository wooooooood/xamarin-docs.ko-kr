---
title: 누적된 보기 사용
description: 이 문서에서는 디자인 및 보기 누적 Xamarin.tvOS 응용 프로그램 내 사용을 설명 합니다.
ms.prod: xamarin
ms.assetid: 00B07F85-F30B-4DD4-8664-A61D0A1CDB0E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: a6300e4da47022199c0503e6be63b0c90f15654d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-stacked-view"></a>누적된 보기 사용

_이 문서에서는 디자인 및 보기 누적 Xamarin.tvOS 응용 프로그램 내 사용을 설명 합니다._


스택 뷰 컨트롤 (`UIStackView`) 이용 하 여 가로 또는 세로로 하위 뷰가, 스택을 관리 하려면 자동 레이아웃 및 크기 클래스의 콘텐츠 변경 내용 및 Apple TV 장치의 화면 크기에 동적으로 응답 하 합니다.

스택 보기에 연결 된 모든 하위 뷰가 레이아웃을 같은 축, 배포, 정렬 및 간격 정의 개발자 속성에 따라 관리 합니다.

[![](stacked-views-images/stacked01.png "하위 뷰 레이아웃 다이어그램")](stacked-views-images/stacked01.png#lightbox)

사용 하는 경우는 `UIStackView` Xamarin.tvOS 앱의 개발자 정의할 수 하위 스토리 보드 디자이너는 iOS 또는 추가 하 고 C# 코드에서 하위 뷰가 제거 하 여 내부 중 하나입니다.

## <a name="about-stacked-view-controls"></a>누적된 View 컨트롤 정보 

`UIStackView` 기능은의 다른 하위 클래스와 같은 캔버스에 그릴 없는 비 렌더링 컨테이너 보기로 이동 하 고 따라서 `UIView`합니다. 와 같은 속성을 설정 `BackgroundColor` 또는 재정의 `DrawRect` 시각적 영향을 미치지 것입니다.

스택 보기는 하위 뷰가의 컬렉션을 배열 하는 방법을 제어 하는 여러 속성이 있습니다.

- **축** – 스택 뷰 하위 뷰 들을 하거나 정렬 하는 경우 결정 **가로로** 또는 **세로로**합니다.
- **맞춤** –는 하위 스택 뷰 내에서 정렬 되는 방법을 제어 합니다.
- **배포** – 스택 뷰 내에서 하위 뷰 들 크기를 조정 하는 방법을 제어 합니다.
- **간격** – 스택 뷰에서 하위 각 뷰 사이의 최소 간격을 제어 합니다.
- **초기 상대** – 경우 `true`, 각 하위 보기의 세로 간격의 초기 계획에서 파생 됩니다.
- **레이아웃 여백 상대** – 하위 뷰 들 표준 레이아웃에 상대적으로 배치 합니다.

일반적으로 적은 수의 하위에 정렬 하는 스택 뷰를 사용 합니다. 하나 이상의 스택 뷰 각각의 내부에 중첩 하 여 더욱 복잡 한 사용자 인터페이스를 만들 수 있습니다.

추가 제약 조건 (예: 높이 또는 너비 제어)를 하위에 추가 하 여 Ui 모양을 미세 조정할 수 있습니다. 그러나 주의 해야를 자체 스택 보기에 의해 발생 하는 충돌 하는 제약 조건을 포함 하지 않도록 합니다.

<a name="Auto-Layout-and-Size-Classes" />

## <a name="auto-layout-and-size-classes"></a>자동 레이아웃 및 크기 클래스

하위 뷰 스택 보기에 추가 될 때 해당 레이아웃 위치를 지정 하 고 정렬 된 뷰 크기를 자동 레이아웃 및 크기 클래스를 사용 하 여 해당 스택 보기에 의해 완전히 제어 됩니다.

스택 뷰는 _pin_ 해당 컬렉션의 첫 번째 및 마지막 하위 뷰는 **Top** 및 **아래쪽** 세로 스택 뷰에 대 한 가장자리 또는 **왼쪽**및 **오른쪽** 가로 스택 뷰에 대 한 가장자리입니다. 설정 하는 경우는 `LayoutMarginsRelativeArrangement` 속성을 `true`, 다음 하위 가장자리 대신 관련 여백에 고정 하는 보기입니다.

스택 뷰는 하위 뷰를 사용 하 여 `IntrinsicContentSize` 속성 따라 정의 된 하위 크기를 계산할 때 `Axis` (제외 하 고는 `FillEqually Distribution`). `FillEqually Distribution` 동일한 크기가 되도록 모든 하위 뷰가 크기를 조정 하므로 따라 스택 뷰를 채우는 `Axis`합니다.

제외는 `Fill Alignment`, 스택 보기는 하위 뷰를 사용 하 여 `IntrinsicContentSize` 에 수직 보기의 크기를 계산 하는 것에 대 한 속성에서 지정 `Axis`합니다. 에 대 한는 `Fill Alignment`, 수직으로 스택 뷰를 채울 수 있도록 모든 하위 뷰가 크기는 주어진 `Axis`합니다.

<a name="Positioning-and-Sizing-the-Stack-View" />

## <a name="positioning-and-sizing-the-stack-view"></a>배치 및 스택 뷰 크기 조정

스택 보기에는 모든 하위 뷰 레이아웃 완전히 제어 하는 동안 (같은 속성에 따라 `Axis` 및 `Distribution`), 스택 뷰 위치를 지정 해야 (`UIStackView`) 자동 레이아웃 및 크기 클래스를 사용 하 여 부모 뷰에 내 합니다.

일반적으로이 확장 / 축소, 따라서 해당 위치를 정의 하는 데 스택 보기 두 개 이상의 가장자리 고정 의미 합니다. 모든 추가 제약 조건 없이 스택 뷰 자동으로 크기가 조정 됩니다에 맞게 모든 해당 하위 다음과 같습니다.

* 크기에 따라 해당 `Axis` 모든 하위 뷰 크기 및 각 하위 뷰 간에 정의 된 모든 공간의 합계가 됩니다.
* 경우는 `LayoutMarginsRelativeArrangement` 속성은 `true`, 스택 뷰 크기 여백을 위한 공간이 포함 됩니다.
* 에 수직 크기는 `Axis` 컬렉션의 가장 큰 서브로 설정 됩니다.

스택 보기에 대 한 제약 조건을 지정할 수는 또한 **높이** 및 **너비**합니다. 이 경우에 하위 배치 됩니다 (크기) 기준으로 스택 뷰로 지정 된 공간을 채우기 위해는 `Distribution` 및 `Alignment` 속성입니다.

경우는 `BaselineRelativeArrangement` 속성은 `true`, 하위 뷰 들으로 사용 하는 대신 첫 번째 또는 마지막 서브 기준에 따라 배치 됩니다는 **Top**, **아래쪽** 또는 **센터* -  **Y** 위치입니다. 이러한 계산 스택 보기의 내용에 다음과 같이:

* 세로 스택 뷰는 첫 번째 기준에 대 한 첫 번째 하위 뷰 및 지난 성을 반환 합니다. 이러한 하위 중 하나는 자체 스택 뷰, 첫 번째 또는 마지막 초기 사용 됩니다.
* 가로 스택 뷰 첫 번째와 마지막 기준에 대 한 가장 큰 해당 하위 뷰를 사용 합니다. 가장 높은 보기 스택 보기 이기도 한 경우 가장 긴 서브 기준선으로 사용 됩니다.

> [!IMPORTANT]
> 기준선 맞춤 잘못 된 위치에 초기 계획을 계산할 때 늘어난 또는 압축 된 하위 뷰 크기에서 작동 하지 않습니다. 기준선 맞춤에 대 한 확인 하는 하위 뷰 **높이** 내장 콘텐츠 보기와 일치 **높이**합니다.




<a name="Common-Stack-View-Uses" />

## <a name="common-stack-view-uses"></a>일반적으로 스택 뷰 사용

스택 뷰 컨트롤에서 제대로 작동 레이아웃 형식은 여러 가지가 있습니다. 다음은 Apple에 따라 몇 가지 일반적인 사용입니다.

- **축을 the 함께 크기 정의** – 스택 보기와 함께 가장자리를 고정 하 여 `Axis` 및 해당 하위에 정의 된 공간에 맞도록 축 보기 크기가 계속 커집니다 스택 위치를 설정 하는 인접 한 가장자리 중 하나입니다.
*  **하위 보기의 위치를 정의** – 부모 뷰를 스택 보기의 인접 한 가장자리를 고정 하 여 스택 뷰 크기 모두 포함 된 하위에 맞게 늘어납니다.
- **정의 크기와 위치는 스택의** – 스택 뷰는 부모 뷰에 스택 보기의 모든 네 가장자리를 고정 하 여 스택 뷰 내에 정의 된 공간에 따라 하위를 정렬 합니다.
*  **정의 된 크기 수직 축** – 스택 뷰 모두 가장자리 수직으로 고정 하 여 `Axis` 보기에 정의 된 공간에 맞도록 축에 수직 늘어나는지 스택 위치를 설정 하려면 축 따라 가장자리 중 하나를 해당 하위 뷰가 있습니다.

<a name="Stack-Views-and-Storyboards" />

## <a name="stack-views-and-storyboards"></a>스토리 보드와 스택 뷰

Xamarin.tvOS 응용 프로그램에서 스택 뷰를 사용 하는 가장 쉬운 방법은 iOS 디자이너를 사용 하 여 응용 프로그램의 UI를에 추가 하는 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 에 **솔루션 패드**, 클릭는 `Main.storyboard` 파일을 열어 편집 합니다.
1. 스택 보기에 추가 하는 것에 개별 요소의 레이아웃을 디자인: 

    [![](stacked-views-images/layout01.png "요소 레이아웃 예제")](stacked-views-images/layout01.png#lightbox)
1. 제대로 크기가 조정 되도록 요소에 필요한 모든 제약 조건을 추가 합니다. 스택 보기에 추가 되 면이 단계는 중요 합니다.
1. (이 경우 4 개) 복사본 필요한 개수를 확인 합니다. 

    [![](stacked-views-images/layout02.png "필요한 복사본의 수")](stacked-views-images/layout02.png#lightbox)
1. 끌어서는 **스택 뷰** 에서 **도구 상자** 보기에 놓습니다. 

    [![](stacked-views-images/layout03.png "스택 보기")](stacked-views-images/layout03.png#lightbox)
1. 스택 보기를 선택는 **위젯을 탭** 의 **속성 패드** 선택 **채우기** 에 대 한는 **맞춤**, **채우기 동일 하 게** 에 대 한는 **배포** 입력 `25` 에 대 한는 **간격**: 

    [![](stacked-views-images/layout04.png "위젯 탭")](stacked-views-images/layout04.png#lightbox)
1. 스택 뷰 원하는 하 고 필요한 위치에 보관 하는 제약 조건을 추가 화면에 놓습니다.
1. 개별 요소를 선택 하 고 스택 뷰로 끌어 오십시오. 

    [![](stacked-views-images/layout05.png "스택 보기에서 개별 요소")](stacked-views-images/layout05.png#lightbox)
1. 레이아웃을 조정 될 것 및 요소 이상으로 설정 하는 특성을 기준으로 스택 보기에 정렬 됩니다.
1. 할당 **이름** 에 **위젯을 탭** 의 **속성 탐색기** C# 코드에서 UI 컨트롤을 사용 하려면.
1. 변경 내용을 저장합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 에 **솔루션 탐색기**, 클릭는 `Main.storyboard` 파일을 열어 편집 합니다.
1. 스택 보기에 추가 하는 것에 개별 요소의 레이아웃을 디자인: 

    [![](stacked-views-images/layout01.png "요소 레이아웃의 예")](stacked-views-images/layout01.png#lightbox)
1. 제대로 크기가 조정 되도록 요소에 필요한 모든 제약 조건을 추가 합니다. 스택 보기에 추가 되 면이 단계는 중요 합니다.
1. (이 경우 4 개) 복사본 필요한 개수를 확인 합니다. 

    [![](stacked-views-images/layout02.png "필요한 복사본의 수")](stacked-views-images/layout02.png#lightbox)
1. 끌어서는 **스택 뷰** 에서 **도구 상자** 보기에 놓습니다. 

    [![](stacked-views-images/layout03-vs.png "스택 보기")](stacked-views-images/layout03-vs.png#lightbox)
1. 스택 보기를 선택는 **위젯을 탭** 의 **속성 탐색기** 선택 **채우기** 에 대 한는 **맞춤**, **채우기 동일 하 게** 에 대 한는 **배포** 입력 `25` 에 대 한는 **간격**: 

    [![](stacked-views-images/layout04-vs.png "위젯 탭")](stacked-views-images/layout04-vs.png#lightbox)
1. 스택 뷰 원하는 하 고 필요한 위치에 보관 하는 제약 조건을 추가 화면에 놓습니다.
1. 개별 요소를 선택 하 고 스택 뷰로 끌어 오십시오. 

    [![](stacked-views-images/layout05-vs.png "스택 보기에서 개별 요소")](stacked-views-images/layout05-vs.png#lightbox)
1. 레이아웃을 조정 될 것 및 요소 이상으로 설정 하는 특성을 기준으로 스택 보기에 정렬 됩니다.
1. 할당 **이름** 에 **위젯을 탭** 의 **속성 탐색기** C# 코드에서 UI 컨트롤을 사용 하려면.
1. 변경 내용을 저장합니다.

-----

> [!IMPORTANT]
> 수와 같은 동작을 할당 하는 `TouchUpInside` UI 요소에 (같은 `UIButton`) iOS 이벤트 처리기를 만들 때 디자이너에서에서이 호출 되지 것입니다 있으므로 Apple TV 터치 스크린 또는 터치 이벤트를 지원 하지 않습니다. 항상 기본값을 사용 해야 `Action Type` 사용자 인터페이스 요소 tvOS에 대 한 작업을 만들 때.

스토리 보드를 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Hello, tvOS 퀵 스타트 가이드](~/ios/tvos/get-started/hello-tvos.md)합니다.

이 예제의 경우는 콘센트 및 세그먼트 컨트롤에 대 한 작업 및 각 "플레이어 카드" 콘센트 노출 했습니다 했습니다. 코드에서 우리 표시 / 숨기기 플레이어 현재 세그먼트에 기반 합니다. 예를 들어:

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

앱이 실행 되는 4 개의 요소가 우리의 스택 보기에서 동일 하 게 배포 됩니다.

[![](stacked-views-images/layout06.png "4 개의 요소가 우리의 스택 보기에서 동일 하 게 배포 될 앱이 실행 되는 경우")](stacked-views-images/layout06.png#lightbox)

플레이어 수를 줄이면 사용 하지 않는 뷰는 숨겨져 있으며, 스택 보기에 맞게 레이아웃을 조정 합니다.

[![](stacked-views-images/layout07.png "플레이어의 수를 줄이면 사용 하지 않는 뷰는 숨겨져 있으며에 맞게 레이아웃을 조정 하는 스택 보기")](stacked-views-images/layout07.png#lightbox)

<a name="Populate-a-Stack-View-from-Code" />

### <a name="populate-a-stack-view-from-code"></a>코드에서 스택 보기를 채우기

IOS 디자이너에서에서 스택 보기의 레이아웃과 콘텐츠를 완전히 정의 외에도 수 만들고 C# 코드에서 동적으로 제거 합니다.

스택 뷰를 사용 하 여 "별표" 검토 (1-5)에서 처리 하는 다음 예제를 수행 합니다.

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

<a name="Dynamically-Changing-Content" />

## <a name="dynamically-changing-content"></a>동적으로 변경 내용

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

<a name="Summary" />

## <a name="summary"></a>요약

이 문서의 디자인 및 보기 누적 Xamarin.tvOS 앱 내에서 사용 검사가 수행 됩니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 응용 프로그램 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
