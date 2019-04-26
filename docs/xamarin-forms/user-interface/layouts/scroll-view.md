---
title: Xamarin.Forms ScrollView
description: 이 문서에서는 Xamarin.Forms ScrollView 클래스를 사용 하 여 하나의 화면에 맞지는 및 키보드의 공간을 확보 하는 콘텐츠를 포함 하는 레이아웃을 제공 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 34339b9ca3a15c7f7f24edee5401c542fd09ba74
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61370794"
---
# <a name="xamarinforms-scrollview"></a>Xamarin.Forms ScrollView

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)

[`ScrollView`](xref:Xamarin.Forms.ScrollView) 레이아웃을 포함 하 고 스크롤 오프 스크린 수 있습니다. `ScrollView` 키보드를 표시할 때 화면에 보이는 부분을 자동으로 이동 하는 뷰를 허용 하도록도 사용 됩니다.

[![](scroll-view-images/layouts-sml.png "Xamarin.Forms 레이아웃")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms 레이아웃")

이 문에서는 다음과 같은 내용을 설명합니다.

- **[목적은](#purpose)**  &ndash; 목적 `ScrollView` 및 사용 하는 경우.
- **[사용 현황](#usage)**  &ndash; 사용 하는 방법을 `ScrollView` 실제로 합니다.
- **[속성](#properties)**  &ndash; 읽고 수정할 수 있는 공용 속성입니다.
- **[메서드](#methods)**  &ndash; 스크롤 뷰를 호출할 수 있는 공용 메서드.
- **[이벤트](#events)**  &ndash; 뷰 상태의 변경 내용을 수신 하는 이벤트입니다.

## <a name="purpose"></a>용도

`ScrollView` 더 큰 뷰 작은 휴대폰에서 잘 표시에 사용할 수 있습니다. 예를 들어 iPhone 6s에서 작동 하는 레이아웃을 iPhone 4 초에서 잘릴 수도 있습니다. 사용 하는 `ScrollView` 잘린된 부분 레이아웃을 더 작은 화면에 표시 될 수 있습니다.

## <a name="usage"></a>사용법

> [!NOTE]
> `ScrollView`s는 중첩 될 수 없습니다. 또한 `ScrollView`s 해야 같은 스크롤을 제공 하는 다른 컨트롤을 사용 하 여 중첩 될 수 없습니다 `ListView` 고 `WebView`입니다.

`ScrollView` 노출 된 `Content` 단일 뷰 또는 레이아웃을 설정할 수 있는 속성입니다. 뒤에 매우 큰 boxView 사용 하 여 레이아웃의 예는 `Entry`:

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

사용자만 아래로 스크롤하여 전에 `BoxView` 표시 됩니다.

![](scroll-view-images/scroll-start.png "ScrollView에서 BoxView")

사용자의 텍스트를 입력 하기 시작 하는 경우 사용자에 게 있음을 `Entry`, 화면에 표시 되도록 스크롤 뷰:

![](scroll-view-images/scroll-end.png "ScrollView 항목")

## <a name="properties"></a>속성

`ScrollView` 다음 속성을 정의합니다.

- [`ContentSize`](xref:Xamarin.Forms.ScrollView.ContentSizeProperty) 가져옵니다를 [ `Size` ](xref:Xamarin.Forms.Size) 콘텐츠의 크기를 나타내는 값입니다.
- [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) 가져오거나를 [ `ScrollOrientation` ](xref:Xamarin.Forms.ScrollOrientation) 스크롤 방향을 나타내는 열거형 값을 `ScrollView`.
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollXProperty) 가져옵니다는 `double` 현재 X 스크롤 위치를 나타내는입니다.
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollYProperty) 가져옵니다는 `double` 현재 Y 스크롤 위치를 나타내는입니다.
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibilityProperty) 가져오거나를 [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility) 가로 스크롤 막대에 표시 되는 경우를 나타내는 값입니다.
- [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.VerticalScrollBarVisibilityProperty) 가져오거나를 [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility) 세로 스크롤 막대에 표시 되는 경우를 나타내는 값입니다.

## <a name="methods"></a>메서드

`ScrollView` 제공 된 `ScrollToAsync` 스크롤 뷰를 사용할 수 있는 메서드를 좌표를 사용 하거나 표시 되어야 합니다는 특정 뷰를 지정 하 여 합니다.

좌표를 사용 하는 경우 지정 된 `x` 및 `y` 해야 스크롤 애니메이션 여부를 나타내는 부울 값을 함께 좌표:

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

특정 요소를 스크롤할 때는 `ScrollToPosition` 열거형 지정 위치 보기에서 요소가 표시 됩니다.

- **Center** &ndash; 중심 보기의 표시 부분에 요소를 스크롤합니다.
- **최종** &ndash; 보기의 표시 영역 끝에 요소를 스크롤합니다.
- **MakeVisible** &ndash; 뷰에서 볼 수 있도록 요소를 스크롤합니다.
- **시작** &ndash; 시작 보기의 표시 부분에 요소를 스크롤합니다.

`IsAnimated` 속성 보기는 스크롤 하는 방법을 지정 합니다. 때 부드러운 애니메이션을 true로 설정 대신 사용 됩니다, 즉시 보기에 콘텐츠를 이동 합니다.

## <a name="events"></a>이벤트

`ScrollView` 하나의 이벤트를 정의 `Scrolled`합니다. `Scrolled` 뷰 스크롤을 마쳤을 때 발생 합니다. 에 대 한 이벤트 처리기 `Scrolled` 사용 `ScrolledEventArgs`에 있는 `ScrollX` 및 `ScrollY` 속성입니다. 다음의 현재 스크롤 위치를 사용 하 여 레이블을 업데이트 하는 방법에 설명 된 `ScrollView`:

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

스크롤 위치 목록의 끝에서 스크롤할 때 바운스 효과 인해 음수가 될 수는 note 합니다.


## <a name="related-links"></a>관련 링크

- [레이아웃 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble 예제 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
