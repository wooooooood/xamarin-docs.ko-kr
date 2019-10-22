---
title: Xamarin.ios ScrollView
description: 이 문서에서는 Xamarin.ios ScrollView 클래스를 사용 하 여 한 화면에만 맞지 않는 레이아웃을 제공 하 고 콘텐츠를 사용 하 여 키보드의 공간을 확보 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/17/2019
ms.openlocfilehash: 8d523c6da6ca7feaf6894123822f789f37455865
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696859"
---
# <a name="xamarinforms-scrollview"></a>Xamarin.ios ScrollView

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[`ScrollView`](xref:Xamarin.Forms.ScrollView) 는 레이아웃을 포함 하며 화면을 스크롤할 수 있습니다. `ScrollView`는 키보드를 표시할 때 보기가 화면에 표시 되는 부분으로 자동으로 이동 하도록 하는 데에도 사용 됩니다.

[![](scroll-view-images/layouts-sml.png "Xamarin.Forms Layouts")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms Layouts")

## <a name="purpose"></a>용도

[`ScrollView`](xref:Xamarin.Forms.ScrollView) 를 사용 하 여 더 큰 보기가 더 작은 휴대폰에서 잘 표시 되도록 할 수 있습니다. 예를 들어 iPhone 6s에서 작동 하는 레이아웃은 iPhone 4s에서 잘릴 수 있습니다. @No__t_0를 사용 하면 레이아웃의 잘린 부분이 더 작은 화면에 표시 될 수 있습니다.

## <a name="usage"></a>사용 현황

> [!NOTE]
> [`ScrollView`](xref:Xamarin.Forms.ScrollView) 개체는 중첩 될 수 없습니다. 또한 `ListView` 및 `WebView`와 같이 스크롤 기능을 제공 하는 다른 컨트롤과 `ScrollView`s 중첩 되어서는 안 됩니다.

[`ScrollView`](xref:Xamarin.Forms.ScrollView) 는 단일 뷰나 레이아웃으로 설정할 수 있는 `Content` 속성을 노출 합니다. 매우 큰 boxView를 사용 하 여 `Entry` 다음의 레이아웃 예제를 생각해 보세요.

```xaml
<ContentPage.Content>
    <ScrollView>
        <StackLayout>
            <BoxView BackgroundColor="Red" HeightRequest="600" WidthRequest="150" />
            <Entry />
        </StackLayout>
    </ScrollView>
</ContentPage.Content>
```

C#:

```csharp
var scroll = new ScrollView();
Content = scroll;
var stack = new StackLayout();
stack.Children.Add(new BoxView { BackgroundColor = Color.Red,    HeightRequest = 600, WidthRequest = 600 });
stack.Children.Add(new Entry());
```

사용자가 아래로 스크롤하면 `BoxView` 표시 됩니다.

![](scroll-view-images/scroll-start.png "BoxView in ScrollView")

사용자가 `Entry`에서 텍스트를 입력 하기 시작 하면 뷰가 화면에 계속 표시 되도록 스크롤됩니다.

![](scroll-view-images/scroll-end.png "Entry in ScrollView")

## <a name="properties"></a>데이터 액세스

[`ScrollView`](xref:Xamarin.Forms.ScrollView)는 다음 속성을 정의합니다.

- [`ContentSize`](xref:Xamarin.Forms.ScrollView.ContentSizeProperty) 콘텐츠의 크기를 나타내는 [`Size`](xref:Xamarin.Forms.Size) 값을 가져옵니다.
- [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) `ScrollView`의 스크롤 방향을 나타내는 [`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation) 열거형 값을 가져오거나 설정 합니다.
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollXProperty) 현재 X 스크롤 위치를 나타내는 `double`를 가져옵니다.
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollYProperty) 현재 Y 스크롤 위치를 나타내는 `double`를 가져옵니다.
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibilityProperty) 가로 스크롤 막대가 표시 되는 경우를 나타내는 [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) 값을 가져오거나 설정 합니다.
- 세로 스크롤 막대가 표시 될 때를 나타내는 [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) 값을 가져오거나 설정 [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.VerticalScrollBarVisibilityProperty) 합니다.

> [!NOTE]
> [@No__t_1](xref:Xamarin.Forms.ScrollView.OrientationProperty) 속성을 `Neither`로 설정 하면 스크롤을 사용 하지 않도록 설정할 수 있습니다.

## <a name="methods"></a>메서드

[`ScrollView`](xref:Xamarin.Forms.ScrollView) 는 좌표를 사용 하거나 표시 해야 하는 특정 뷰를 지정 하 여 뷰를 스크롤 하는 데 사용할 수 있는 `ScrollToAsync` 메서드를 제공 합니다.

좌표를 사용 하는 경우 스크롤에 애니메이션을 적용 해야 하는지 여부를 나타내는 부울과 함께 `x` 및 `y` 좌표를 지정 합니다.

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

> [!IMPORTANT]
> [@No__t_2](xref:Xamarin.Forms.ScrollView.OrientationProperty) 속성이 `Neither`으로 설정 된 경우 `ScrollToAsync` 메서드가 스크롤되지 않습니다.

특정 요소로 스크롤할 때 `ScrollToPosition` 열거형은 뷰에서 요소가 표시 되는 위치를 지정 합니다.

- **가운데** &ndash; 요소를 보기의 보이는 부분 가운데로 스크롤합니다.
- **끝** &ndash; 요소를 뷰의 표시 된 부분 끝으로 스크롤합니다.
- **Makevisible** &ndash; 뷰 내에 표시 되도록 요소를 스크롤합니다.
- **시작** &ndash; 요소를 뷰의 표시 되는 부분에 대 한 시작 부분으로 스크롤합니다.

@No__t_0 속성은 뷰를 스크롤 하는 방법을 지정 합니다. @No__t_0로 설정 하면 콘텐츠를 즉시 뷰로 이동 하는 것이 아니라 부드러운 애니메이션이 사용 됩니다.

## <a name="events"></a>이벤트

[`ScrollView`](xref:Xamarin.Forms.ScrollView) 는 `Scrolled` 하나의 이벤트만 정의 합니다. 뷰가 스크롤을 마치면 `Scrolled` 발생 합니다. @No__t_0에 대 한 이벤트 처리기는 `ScrollX` 및 `ScrollY` 속성이 있는 `ScrolledEventArgs`를 사용 합니다. 다음은 `ScrollView`의 현재 스크롤 위치를 사용 하 여 레이블을 업데이트 하는 방법을 보여 줍니다.

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

목록의 끝에서 스크롤할 때 바운스 효과 때문에 스크롤 위치는 음수일 수 있습니다.

## <a name="related-links"></a>관련 링크

- [레이아웃 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [BusinessTumble 예제 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
