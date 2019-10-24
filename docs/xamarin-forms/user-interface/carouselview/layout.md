---
title: Xamarin.ios CarouselView 레이아웃
description: 기본적으로 CarouselView에는 해당 항목이 가로로 표시 됩니다. 그러나 세로 방향도 가능 합니다.
ms.prod: xamarin
ms.assetid: fede0382-c972-4023-a4ea-fe5cadec91a6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/14/2019
ms.openlocfilehash: 051bbc1732dc1b074d27080f74621a57a80aaaa4
ms.sourcegitcommit: e71474f91639bb43159b22f5d534325c3270ba93
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/22/2019
ms.locfileid: "72749816"
---
# <a name="xamarinforms-carouselview-layout"></a>Xamarin.ios CarouselView 레이아웃

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView) 는 레이아웃을 제어 하는 다음 속성을 정의 합니다.

- `LinearItemsLayout` 형식의 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)는 사용할 레이아웃을 지정 합니다.
- [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemSizingStrategy)형식의 [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemsView.ItemSizingStrategy)은 사용할 항목 측정 전략을 지정 합니다.
- `int` 형식의 `NumberOfSideItems` 현재 항목에 인접 한 표시 되는 항목의 수입니다. 기본값은 0입니다.
- [`Thickness`](xref:Xamarin.Forms.Thickness)형식의 `PeekAreaInsets`는 인접 한 항목을에서 부분적으로 표시할 수 있는 정도를 지정 합니다.

이러한 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원 됩니다. 즉, 속성은 데이터 바인딩의 대상이 될 수 있습니다.

기본적으로 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 은 해당 항목을 가로 방향으로 표시 합니다. 단일 항목이 화면에 표시 되 고, 살짝 밀기 제스처를 사용 하 여 항목 컬렉션을 전달 하 고 뒤로 탐색 합니다. 그러나 세로 방향도 가능 합니다. 이는 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) 속성이 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) 클래스에서 상속 되는 `LinearItemsLayout` 형식 이기 때문입니다. @No__t_0 클래스는 다음 속성을 정의 합니다.

- [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)형식의 [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)항목이 추가 될 때 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 확장 되는 방향을 지정 합니다.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)형식의 [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)는 맞춤 지점이 항목에 정렬 되는 방법을 지정 합니다.
- [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)형식의 [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)은 스크롤할 때 스냅 지점의 동작을 지정 합니다.

이러한 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원 됩니다. 즉, 속성은 데이터 바인딩의 대상이 될 수 있습니다. 스냅 점에 대 한 자세한 내용은 [CollectionView 스크롤](scrolling.md) 가이드의 [중심점](scrolling.md#snap-points) 을 참조 하세요.

[@No__t_1](xref:Xamarin.Forms.ItemsLayoutOrientation) 열거형은 다음 멤버를 정의 합니다.

- `Vertical`는 항목이 추가 될 때 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 세로로 확장 됨을 나타냅니다.
- `Horizontal`는 항목이 추가 될 때 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 가로로 확장 됨을 나타냅니다.

@No__t_0 클래스는 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) 클래스에서 상속 되며 각 항목 주위의 빈 공간을 나타내는 `double` 형식의 `ItemSpacing` 속성을 정의 합니다. 이 속성의 기본값은 0이 고 해당 값은 항상 0 보다 크거나 같아야 합니다. 또한 `LinearItemsLayout` 클래스는 정적 `Vertical` 및 `Horizontal` 멤버를 정의 합니다. 이러한 멤버를 사용 하 여 각각 세로 또는 가로 목록을 만들 수 있습니다. 또는 [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) 열거형 멤버를 인수로 지정 하 여 `LinearItemsLayout` 개체를 만들 수 있습니다.

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView) 는 기본 레이아웃 엔진을 사용 하 여 레이아웃을 수행 합니다.

## <a name="horizontal-layout"></a>가로 레이아웃

기본적으로 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 는 항목을 가로로 표시 합니다. 따라서이 레이아웃을 사용 하도록 [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) 속성을 설정 하지 않아도 됩니다.

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <Frame HasShadow="True"
                       BorderColor="DarkGray"
                       CornerRadius="5"
                       Margin="20"
                       HeightRequest="300"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand">
                    <StackLayout>
                        <Label Text="{Binding Name}"
                               FontAttributes="Bold"
                               FontSize="Large"
                               HorizontalOptions="Center"
                               VerticalOptions="Center" />
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="150"
                               WidthRequest="150"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Location}"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Details}"
                               FontAttributes="Italic"
                               HorizontalOptions="Center"
                               MaxLines="5"
                               LineBreakMode="TailTruncation" />
                    </StackLayout>
                </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

또는 `Horizontal` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) 열거형 멤버를 `Orientation` 속성 값으로 지정 하 여 [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) 속성을 `LinearItemsLayout` 개체로 설정 하 여이 레이아웃을 수행할 수도 있습니다.

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    ItemsLayout = LinearItemsLayout.Horizontal
};
```

그러면 새 항목이 추가 될 때 레이아웃이 가로로 증가 합니다.

## <a name="vertical-layout"></a>세로 레이아웃

[`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) 속성을 `LinearItemsLayout` 개체로 설정 하 여 `Vertical` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) 열거형 멤버를 `Orientation` 속성 값으로 지정 하 여 해당 항목을 세로로 표시할 수 [`CarouselView`](xref:Xamarin.Forms.CarouselView) .

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical" />
    </CarouselView.ItemsLayout>
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <Frame HasShadow="True"
                       BorderColor="DarkGray"
                       CornerRadius="5"
                       Margin="20"
                       HeightRequest="300"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand">
                    <StackLayout>
                        <Label Text="{Binding Name}"
                               FontAttributes="Bold"
                               FontSize="Large"
                               HorizontalOptions="Center"
                               VerticalOptions="Center" />
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="150"
                               WidthRequest="150"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Location}"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Details}"
                               FontAttributes="Italic"
                               HorizontalOptions="Center"
                               MaxLines="5"
                               LineBreakMode="TailTruncation" />
                    </StackLayout>
                </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    ItemsLayout = LinearItemsLayout.Vertical
};
```

이로 인해 새 항목이 추가 될 때 레이아웃이 세로로 증가 합니다.

## <a name="partially-visible-adjacent-items"></a>부분적으로 표시 되는 인접 항목

기본적으로 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 는 전체 항목을 한 번에 표시 합니다. 그러나 `PeekAreaInsets` 속성을에 의해 부분적으로 표시 되는 인접 한 항목의 크기를 지정 하는 `Thickness` 값으로 설정 하 여이 동작을 변경할 수 있습니다. 이는 볼 추가 항목이 있음을 사용자에 게 표시 하는 데 유용할 수 있습니다. 다음 XAML은이 속성을 설정 하는 예를 보여 줍니다.

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    ...
</CarouselView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    PeekAreaInsets = new Thickness(100)
};
```

결과적으로 인접 한 항목이 화면에 부분적으로 노출 됩니다.

## <a name="fully-visible-adjacent-items"></a>완전히 표시 되는 인접 항목

기본적으로 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 은 한 번에 하나의 항목을 표시 합니다. 그러나이 동작은 현재 항목에 인접 하 여 표시할 항목 수를 나타내는 정수로 `NumberOfSideItems` 속성을 설정 하 여 변경할 수 있습니다. 다음 XAML은이 속성을 설정 하는 예를 보여 줍니다.

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              NumberOfSideItems="1">
    ...
</CarouselView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    NumberOfSideItems = 1
};
```

이 예에서는 한 인접 항목이 현재 항목의 양쪽에 표시 됩니다.

> [!NOTE]
> @No__t_0 속성을 설정 하는 경우에도 모든 `PeekAreaInsets` 값이 적용 됩니다.

## <a name="item-spacing"></a>항목 간격

기본적으로 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 의 각 항목 주위에는 빈 공간이 없습니다. @No__t_0에서 사용 하는 항목 레이아웃에 대 한 속성을 설정 하 여이 동작을 변경할 수 있습니다.

[@No__t_1](xref:Xamarin.Forms.CarouselView) [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) 속성을 `LinearItemsLayout` 개체로 설정 하면 `LinearItemsLayout.ItemSpacing` 속성을 각 항목 주위의 빈 공간을 나타내는 `double` 값으로 설정할 수 있습니다.

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           ItemSpacing="20" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

> [!NOTE]
> @No__t_0 속성에는 속성 값이 항상 0 보다 크거나 같도록 확인 하는 유효성 검사 콜백 집합이 있습니다.

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        ItemSpacing = 20
    }
};
```

이 코드는 세로 레이아웃을 생성 하며 각 항목 주위에는 20 개의 간격이 있습니다.

## <a name="dynamic-resizing-of-items"></a>항목의 동적 크기 조정

[@No__t_3](xref:Xamarin.Forms.DataTemplate)내에서 요소의 레이아웃 관련 속성을 변경 하 여 런타임에 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 항목의 크기를 동적으로 조정할 수 있습니다. 예를 들어 다음 코드 예제에서는 [`Image`](xref:Xamarin.Forms.Image) 개체의 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 및 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 속성과 부모 [`Frame`](xref:Xamarin.Forms.Frame)의 `HeightRequest` 속성을 변경 합니다.

```csharp
void OnImageTapped(object sender, EventArgs e)
{
    Image image = sender as Image;
    image.HeightRequest = image.WidthRequest = image.HeightRequest.Equals(150) ? 200 : 150;
    Frame frame = ((Frame)image.Parent.Parent);
    frame.HeightRequest = frame.HeightRequest.Equals(300) ? 350 : 300;
}
```

@No__t_0 이벤트 처리기는 탭 하는 [`Image`](xref:Xamarin.Forms.Image) 개체에 대 한 응답으로 실행 되 고 이미지의 크기 (및 부모 프레임)를 변경 하 여 쉽게 볼 수 있도록 합니다.

## <a name="right-to-left-layout"></a>오른쪽에서 왼쪽 레이아웃

[`CarouselView`](xref:Xamarin.Forms.CarouselView) [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성을 [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)로 설정 하 여 해당 콘텐츠를 오른쪽에서 왼쪽 흐름 방향으로 레이아웃을 지정할 수 있습니다. 그러나 페이지 또는 루트 레이아웃에는 `FlowDirection` 속성을 설정 하는 것이 가장 좋습니다. 이렇게 하면 페이지 또는 루트 레이아웃의 모든 요소가 흐름 방향에 응답 하 게 됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CarouselViewDemos.Views.HorizontalTemplateLayoutRTLPage"
             Title="Horizontal layout (RTL FlowDirection)"
             FlowDirection="RightToLeft">    
    <CarouselView ItemsSource="{Binding Monkeys}">
        ...
    </CarouselView>
</ContentPage>
```

부모가 있는 요소의 기본 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 은 [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)입니다. 따라서 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 는 [`ContentPage`](xref:Xamarin.Forms.ContentPage)에서 `FlowDirection` 속성 값을 상속 합니다.

흐름 방향에 대 한 자세한 내용은 [오른쪽에서 왼쪽 지역화](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [CarouselView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [오른쪽에서 왼쪽 지역화](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [Xamarin.ios CarouselView 스크롤](scrolling.md)
