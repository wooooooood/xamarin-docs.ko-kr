---
title: Xamarin.Forms ScrollView
description: 이 문서에서는 Xamarin.Forms ScrollView 클래스를 사용 하 여 하나의 화면에 맞지는 및 키보드의 공간을 확보 하는 콘텐츠를 포함 하는 레이아웃을 제공 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2019
ms.openlocfilehash: bb10cda7c9899f176861ceee712cc876984c56ef
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75488272"
---
# <a name="xamarinforms-scrollview"></a>Xamarin.Forms ScrollView

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[`ScrollView`](xref:Xamarin.Forms.ScrollView) 레이아웃을 포함 하 고 스크롤 오프 스크린 수 있습니다. `ScrollView` 키보드를 표시할 때 화면에 보이는 부분을 자동으로 이동 하는 뷰를 허용 하도록도 사용 됩니다.

[![](scroll-view-images/layouts-sml.png "Xamarin.Forms Layouts")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms Layouts")

## <a name="purpose"></a>용도

[`ScrollView`](xref:Xamarin.Forms.ScrollView) 를 사용 하 여 더 큰 보기가 더 작은 휴대폰에서 잘 표시 되도록 할 수 있습니다. 예를 들어 iPhone 6s에서 작동 하는 레이아웃을 iPhone 4 초에서 잘릴 수도 있습니다. 사용 하는 `ScrollView` 잘린된 부분 레이아웃을 더 작은 화면에 표시 될 수 있습니다.

## <a name="usage"></a>용도

> [!NOTE]
> [`ScrollView`](xref:Xamarin.Forms.ScrollView) 개체는 중첩 될 수 없습니다. 또한 `ScrollView`s 해야 같은 스크롤을 제공 하는 다른 컨트롤을 사용 하 여 중첩 될 수 없습니다 `ListView` 고 `WebView`입니다.

[`ScrollView`](xref:Xamarin.Forms.ScrollView) 는 단일 뷰나 레이아웃으로 설정할 수 있는 `Content` 속성을 노출 합니다. 뒤에 매우 큰 boxView 사용 하 여 레이아웃의 예는 `Entry`:

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
public class ScrollingDemoCode : ContentPage
{
    public ScrollingDemoCode()
    {
        StackLayout stackLayout = new StackLayout();
        stackLayout.Children.Add(new BoxView { BackgroundColor = Color.Red, HeightRequest = 600, WidthRequest = 150 });
        stackLayout.Children.Add(new Entry());
        ScrollView scrollView = new ScrollView();
        scrollView.Content = stackLayout;
        Content = scrollView;
    }
}
```

사용자만 아래로 스크롤하여 전에 `BoxView` 표시 됩니다.

![](scroll-view-images/scroll-start.png "BoxView in ScrollView")

사용자의 텍스트를 입력 하기 시작 하는 경우 사용자에 게 있음을 `Entry`, 화면에 표시 되도록 스크롤 뷰:

![](scroll-view-images/scroll-end.png "Entry in ScrollView")

## <a name="properties"></a>속성

[`ScrollView`](xref:Xamarin.Forms.ScrollView)는 다음 속성을 정의합니다.

- [`ContentSize`](xref:Xamarin.Forms.ScrollView.ContentSizeProperty) 가져옵니다를 [ `Size` ](xref:Xamarin.Forms.Size) 콘텐츠의 크기를 나타내는 값입니다.
- [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) 가져오거나를 [ `ScrollOrientation` ](xref:Xamarin.Forms.ScrollOrientation) 스크롤 방향을 나타내는 열거형 값을 `ScrollView`.
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollXProperty) 가져옵니다는 `double` 현재 X 스크롤 위치를 나타내는입니다.
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollYProperty) 가져옵니다는 `double` 현재 Y 스크롤 위치를 나타내는입니다.
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibilityProperty) 가져오거나를 [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility) 가로 스크롤 막대에 표시 되는 경우를 나타내는 값입니다.
- [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.VerticalScrollBarVisibilityProperty) 가져오거나를 [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility) 세로 스크롤 막대에 표시 되는 경우를 나타내는 값입니다.

> [!NOTE]
> [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) 속성을 `Neither`로 설정 하면 스크롤을 사용 하지 않도록 설정할 수 있습니다.

## <a name="methods"></a>메서드

[`ScrollView`](xref:Xamarin.Forms.ScrollView) 는 좌표를 사용 하거나 표시 해야 하는 특정 뷰를 지정 하 여 뷰를 스크롤 하는 데 사용할 수 있는 `ScrollToAsync` 메서드를 제공 합니다.

좌표를 사용 하는 경우 지정 된 `x` 및 `y` 해야 스크롤 애니메이션 여부를 나타내는 부울 값을 함께 좌표:

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

> [!IMPORTANT]
> [`ScrollView.Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) 속성이 `Neither`으로 설정 된 경우 `ScrollToAsync` 메서드가 스크롤되지 않습니다.

특정 요소로 스크롤할 때 `ScrollToPosition` 열거형은 뷰에서 요소가 표시 되는 위치를 지정 합니다.

- **Center** &ndash; 중심 보기의 표시 부분에 요소를 스크롤합니다.
- **최종** &ndash; 보기의 표시 영역 끝에 요소를 스크롤합니다.
- **MakeVisible** &ndash; 뷰에서 볼 수 있도록 요소를 스크롤합니다.
- **시작** &ndash; 시작 보기의 표시 부분에 요소를 스크롤합니다.

`IsAnimated` 속성 보기는 스크롤 하는 방법을 지정 합니다. `true`로 설정 하면 콘텐츠를 즉시 뷰로 이동 하는 것이 아니라 부드러운 애니메이션이 사용 됩니다.

## <a name="events"></a>이벤트

[`ScrollView`](xref:Xamarin.Forms.ScrollView) 는 `Scrolled`하나의 이벤트만 정의 합니다. `Scrolled` 뷰 스크롤을 마쳤을 때 발생 합니다. 에 대 한 이벤트 처리기 `Scrolled` 사용 `ScrolledEventArgs`에 있는 `ScrollX` 및 `ScrollY` 속성입니다. 다음의 현재 스크롤 위치를 사용 하 여 레이블을 업데이트 하는 방법에 설명 된 `ScrollView`:

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

스크롤 위치 목록의 끝에서 스크롤할 때 바운스 효과 인해 음수가 될 수는 note 합니다.

## <a name="related-links"></a>관련 링크

- [레이아웃 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [BusinessTumble 예제 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
