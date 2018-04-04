---
title: ScrollView
description: ScrollView 한 화면에 맞출 수 없으며 키보드의 공간을 만들기 위해 콘텐츠가 있는 레이아웃을 사용 하 여 합니다.
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2016
ms.openlocfilehash: 13f3d5c02b8451bcd52b355fb89f7931f50a0d39
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="scrollview"></a>ScrollView

[`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) 레이아웃을 포함 하 고 스크롤 스크린 수 있습니다. `ScrollView` 키보드 표시 될 때 자동으로 화면에 보이는 부분을 이동 하는 보기를 허용 하도록도 사용 됩니다.

[![](scroll-view-images/layouts-sml.png "Xamarin.Forms Layouts")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms Layouts")

이 문서에서는 다룹니다.

- **[용도](#Purpose)**  &ndash; 의 목적은 `ScrollView` 및 사용 하는 경우.
- **[사용 현황](#Usage)**  &ndash; 사용 하는 방법을 `ScrollView` 실제로 합니다.
- **[속성](#Properties)**  &ndash; 읽고 수정할 수 있는 공용 속성입니다.
- **[메서드](#Methods)**  &ndash; 화면을 스크롤합니다 하도록 호출 될 수 있는 공용 메서드.
- **[이벤트](#Events)**  &ndash; 보기의 상태 변경 내용을 수신에 사용할 수 있는 이벤트입니다.

## <a name="purpose"></a>용도

`ScrollView` 데 사용할 수 더 큰 뷰 더 작은 휴대폰에서 제대로 표시 되도록 할 수 있습니다. 예를 들어, 4s iPhone에서 iPhone 6s에서 작동 하는 레이아웃 잘릴 수도 있습니다. 사용 하는 `ScrollView` 더 작은 화면에 표시할 레이아웃의 잘린된 부분이 허용 합니다.

## <a name="usage"></a>사용법

> [!NOTE]
> `ScrollView`s는 중첩할 수 없습니다. 또한 `ScrollView`s 같은 스크롤을 제공 하는 다른 제어 기능과 함께 중첩 되어야 합니다 `ListView` 및 `WebView`합니다.

`ScrollView` 노출 된 `Content` 단일 뷰 또는 레이아웃을 설정할 수 있는 속성입니다. 이어서 하는 매우 큰 boxView 있는 레이아웃의 예는 `Entry`:

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
stack.Children.Add(new BoxView { BackgroundColor = Color.Red,   HeightRequest = 600, WidthRequest = 600 });
stack.Children.Add(new Entry());
```

사용자가 유일한 아래로 스크롤하기 전에 `BoxView` 표시 됩니다.

![](scroll-view-images/scroll-start.png "BoxView ScrollView에")

텍스트를 입력 하는 사용자가 시작할 때 표시 되는 `Entry`, 화면에 표시 되도록 스크롤 하는 보기:

![](scroll-view-images/scroll-end.png "ScrollView의 엔트리")

## <a name="properties"></a>속성

ScrollView에 다음과 같은 속성이 있습니다.

- **콘텐츠** &ndash; 에 표시할 보기를 가져오거나 설정 합니다.는 `ScrollView`합니다.
- **[ContentSize](https://developer.xamarin.com/api/type/Xamarin.Forms.Size/)**  &ndash; 읽기 전용, 너비 및 높이 구성 요소는 콘텐츠의 크기를 가져옵니다. 이 속성은 바인딩 가능한 속성
- **[방향](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/)**  &ndash; 이것이 [ `ScrollOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/)로 설정할 수 있는 열거형이 있는 `Horizontal`, `Vertical`, 또는 `Both`합니다.
- **ScrollX** &ndash; 읽기 전용 x 축 방향에서 현재 스크롤 위치를 가져옵니다.
- **ScrollY** &ndash; 읽기 전용 Y 차원의 현재 스크롤 위치를 가져옵니다.

## <a name="methods"></a>메서드

`ScrollView` 제공 된 `ScrollToAsync` 화면을 스크롤합니다를 사용할 수 있는 메서드 좌표를 사용 하거나 표시할 특정 보기를 지정 하 여 합니다.

좌표를 사용 하 여, 지정 된 `x` 및 `y` 해야 스크롤 애니메이션 여부를 나타내는 부울 값을 함께 좌표:

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

특정 요소를 스크롤할 수 있도록 하는 경우는 `ScrollToPosition` 열거형 지정 위치 보기에는 요소가 표시 됩니다.

- **Center** &ndash; 보기의 보이는 부분의 가운데에 요소 스크롤합니다.
- **최종** &ndash; 보기의 보이는 부분의 끝에 요소 스크롤합니다.
- **MakeVisible** &ndash; 보기 내에서 볼 수 있도록 요소를 스크롤합니다.
- **시작** &ndash; 보기의 보이는 부분의 시작 부분에 요소 스크롤합니다.

`IsAnimated` 속성 보기 스크롤할 수는 방법을 지정 합니다. 때 애니메이션을 부드럽게 true로 설정 대신 사용 됩니다, 즉시 콘텐츠가 보기로 이동 합니다.

## <a name="events"></a>이벤트

`ScrollView` 하나의 이벤트를 노출 `Scrolled`합니다. `Scrolled` 뷰 스크롤 완료 되 면 발생 합니다. 에 대 한 이벤트 처리기 `Scrolled` 사용 `ScrolledEventArgs`을는 `ScrollX` 및 `ScrollY` 속성입니다. 다음에는 레이블을의 현재 스크롤 위치를 업데이트 하는 방법을 보여 줍니다는 `ScrollView`:

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

Note 스크롤 위치를 목록의 끝에서 스크롤할 때 반송 효과 인해 음수 수 있습니다.


## <a name="related-links"></a>관련 링크

- [레이아웃 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble 예제 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
