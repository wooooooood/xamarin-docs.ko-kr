---
제목: " Xamarin.Forms CarouselView Layout" description: "기본적으로 CarouselView는 항목을 가로로 표시 합니다. 그러나 세로 방향도 가능 합니다. "
assetid: fede0382-c972-4023-a4ea-fe5cadec91a6: xamarin-forms author: davidbritch: dabritch:: 01/28/2020-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xamarinforms-carouselview-layout"></a>Xamarin.FormsCarouselView 레이아웃

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)는 레이아웃을 제어 하는 다음 속성을 정의 합니다.

- [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout), 형식의, `LinearItemsLayout` 사용할 레이아웃을 지정 합니다.
- `PeekAreaInsets`형식의는 [`Thickness`](xref:Xamarin.Forms.Thickness) 인접 한 항목을에서 부분적으로 표시할 수 있는 정도를 지정 합니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 속성은 데이터 바인딩의 대상이 될 수 있습니다.

기본적으로는 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 가로 방향으로 해당 항목을 표시 합니다. 단일 항목이 화면에 표시 되 고, 살짝 밀기 제스처를 사용 하 여 항목 컬렉션을 전달 하 고 뒤로 탐색 합니다. 그러나 세로 방향도 가능 합니다. 이는 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) 속성이 `LinearItemsLayout` 클래스에서 상속 되는 형식 이기 때문입니다 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) . `ItemsLayout`클래스는 다음 속성을 정의 합니다.

- [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)형식의는 [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) [`CarouselView`](xref:Xamarin.Forms.CarouselView) 항목이 추가 될 때가 확장 되는 방향을 지정 합니다.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)형식의는 [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment) 맞춤 지점이 항목에 정렬 되는 방법을 지정 합니다.
- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)형식의는 [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType) 스크롤할 때 중심점의 동작을 지정 합니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 속성은 데이터 바인딩의 대상이 될 수 있습니다. 맞추기 요소에 대 한 자세한 내용은 [ Xamarin.Forms CollectionView 스크롤](scrolling.md) 가이드의 [snap points](scrolling.md#snap-points) 을 참조 하세요.

[`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)열거형은 다음 멤버를 정의 합니다.

- `Vertical`[`CarouselView`](xref:Xamarin.Forms.CarouselView)항목이 추가 될 때이 세로로 확장 됨을 나타냅니다.
- `Horizontal`[`CarouselView`](xref:Xamarin.Forms.CarouselView)항목이 추가 될 때가 가로로 확장 됨을 나타냅니다.

`LinearItemsLayout`클래스는 클래스에서 상속 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) 되며 `ItemSpacing` `double` 각 항목 주위의 빈 공간을 나타내는 형식의 속성을 정의 합니다. 이 속성의 기본값은 0이 고 해당 값은 항상 0 보다 크거나 같아야 합니다. `LinearItemsLayout`또한 클래스는 정적 `Vertical` 및 멤버를 정의 `Horizontal` 합니다. 이러한 멤버를 사용 하 여 각각 세로 또는 가로 목록을 만들 수 있습니다. 또는 `LinearItemsLayout` 열거형 멤버를 인수로 지정 하 여 개체를 만들 수 있습니다 [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) .

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)에서는 네이티브 레이아웃 엔진을 사용 하 여 레이아웃을 수행 합니다.

## <a name="horizontal-layout"></a>가로 레이아웃

기본적으로는 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 해당 항목을 가로로 표시 합니다. 따라서 [`ItemsLayout`](xref:Xamarin.Forms.CarouselView.ItemsLayout) 이 레이아웃을 사용 하도록 속성을 설정할 필요는 없습니다.

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

또는 속성을 개체로 설정 하 여 [`ItemsLayout`](xref:Xamarin.Forms.CarouselView.ItemsLayout) `LinearItemsLayout` `Horizontal` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) 열거형 멤버를 `Orientation` 속성 값으로 지정 하 여이 레이아웃을 수행할 수도 있습니다.

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

그러면 새 항목이 추가 될 때 레이아웃이 가로 방향으로 증가 합니다.

[![IOS 및 Android에서 CarouselView 가로 레이아웃의 스크린샷](layout-images/horizontal.png "CarouselView 가로 레이아웃")](layout-images/horizontal-large.png#lightbox "CarouselView 가로 레이아웃")

## <a name="vertical-layout"></a>세로 레이아웃

[`CarouselView`](xref:Xamarin.Forms.CarouselView)속성을 개체로 설정 하 여 [`ItemsLayout`](xref:Xamarin.Forms.CarouselView.ItemsLayout) `LinearItemsLayout` `Vertical` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) 열거형 멤버를 `Orientation` 속성 값으로 지정 하 여 해당 항목을 세로로 표시할 수 있습니다.

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

[![IOS 및 Android에서 CarouselView 세로 레이아웃의 스크린샷](layout-images/vertical.png "CarouselView 세로 레이아웃")](layout-images/vertical-large.png#lightbox "CarouselView 세로 레이아웃")

## <a name="partially-visible-adjacent-items"></a>부분적으로 표시 되는 인접 항목

기본적으로에서는 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 전체 항목을 한 번에 표시 합니다. 그러나이 동작은 `PeekAreaInsets` `Thickness` 인접 한 항목을에서 부분적으로 표시할 수 있는 정도를 지정 하는 값으로 속성을 설정 하 여 변경할 수 있습니다. 이는 볼 추가 항목이 있음을 사용자에 게 표시 하는 데 유용할 수 있습니다. 다음 XAML은이 속성을 설정 하는 예를 보여 줍니다.

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

[![IOS 및 Android에서 부분적으로 표시 되는 인접 항목을 포함 하는 CollectionView의 스크린샷](layout-images/peek-items.png "CarouselView 피킹 (peeking) 영역 인세트")](layout-images/peek-items-large.png#lightbox "CarouselView 피크 영역 인세트")

## <a name="item-spacing"></a>항목 간격

기본적으로의 각 항목 사이에는 공백이 없습니다 [`CarouselView`](xref:Xamarin.Forms.CarouselView) . 에서 `ItemSpacing` 사용 하는 항목 레이아웃에 대 한 속성을 설정 하 여이 동작을 변경할 수 있습니다 `CarouselView` .

에서 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 해당 속성을 개체로 설정 하는 경우 [`ItemsLayout`](xref:Xamarin.Forms.CarouselView.ItemsLayout) 속성을 `LinearItemsLayout` `LinearItemsLayout.ItemSpacing` `double` 항목 사이의 간격을 나타내는 값으로 설정할 수 있습니다.

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
> 속성에는 `LinearItemsLayout.ItemSpacing` 속성 값이 항상 0 보다 크거나 같도록 확인 하는 유효성 검사 콜백 집합이 있습니다.

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

이 코드는 항목 사이에 20 개의 간격이 있는 세로 레이아웃을 생성 합니다.

## <a name="dynamic-resizing-of-items"></a>항목의 동적 크기 조정

[`CarouselView`](xref:Xamarin.Forms.CarouselView)내에서 요소의 레이아웃 관련 속성을 변경 하 여의 항목을 런타임에 동적으로 조정할 수 있습니다 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . 예를 들어 다음 코드 예제에서는 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 개체의 및 속성과 [`Image`](xref:Xamarin.Forms.Image) `HeightRequest` 부모의 속성을 변경 합니다 [`Frame`](xref:Xamarin.Forms.Frame) .

```csharp
void OnImageTapped(object sender, EventArgs e)
{
    Image image = sender as Image;
    image.HeightRequest = image.WidthRequest = image.HeightRequest.Equals(150) ? 200 : 150;
    Frame frame = ((Frame)image.Parent.Parent);
    frame.HeightRequest = frame.HeightRequest.Equals(300) ? 350 : 300;
}
```

`OnImageTapped`이벤트 처리기는 탭 하는 개체에 대 한 응답으로 실행 되 [`Image`](xref:Xamarin.Forms.Image) 고 이미지의 크기와 부모를 변경 하 여 `Frame` 더 쉽게 볼 수 있도록 합니다.

[![IOS 및 Android에서 동적 항목 크기 조정을 사용 하는 CarouselView의 스크린샷](layout-images/runtime-resizing.png "CarouselView 동적 항목 크기 조정")](layout-images/runtime-resizing-large.png#lightbox "CarouselView 동적 항목 크기 조정")

## <a name="right-to-left-layout"></a>오른쪽에서 왼쪽 레이아웃

[`CarouselView`](xref:Xamarin.Forms.CarouselView)속성을로 설정 하 여 오른쪽에서 왼쪽 흐름 방향으로 해당 콘텐츠를 레이아웃 할 수 있습니다 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft) . 그러나 페이지 `FlowDirection` 또는 루트 레이아웃에 속성을 설정 하는 것이 가장 좋습니다. 이렇게 하면 페이지 또는 루트 레이아웃 내의 모든 요소가 흐름 방향에 응답 합니다.

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

[`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)부모가 있는 요소의 기본값은 [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent) 입니다. 따라서는에서 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 속성 값을 상속 합니다 `FlowDirection` [`ContentPage`](xref:Xamarin.Forms.ContentPage) .

흐름 방향에 대 한 자세한 내용은 [오른쪽에서 왼쪽 지역화](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [CarouselView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [오른쪽에서 왼쪽으로 쓰는 언어 지역화](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [Xamarin.FormsCarouselView 스크롤](scrolling.md)
