---
title: Xamarin.ios CarouselView 데이터
description: CarouselView는 System.windows.controls.itemscontrol.itemssource 속성을 IEnumerable을 구현 하는 컬렉션으로 설정 하 여 데이터로 채워집니다.
ms.prod: xamarin
ms.assetid: 20DB2C57-CE3A-4D91-80DC-73AE361A3CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/27/2019
ms.openlocfilehash: 154d039e95ccc2de28e09a7162a32a19f8f84656
ms.sourcegitcommit: 5d22f37dfc358678df52a4d17c57261056a72cb7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77674535"
---
# <a name="xamarinforms-carouselview-data"></a>Xamarin.ios CarouselView 데이터

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

표시 되는 데이터와 표시 되는 데이터를 정의 하는 다음 속성을 포함 하 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 입니다.

- `IEnumerable`형식의 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)는 표시할 항목의 컬렉션을 지정 하 고 기본값은 `null`입니다.
- [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)형식의 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)는 표시할 항목 컬렉션의 각 항목에 적용할 템플릿을 지정 합니다.

이러한 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원 됩니다. 즉, 속성은 데이터 바인딩의 대상이 될 수 있습니다.

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView) 새 항목이 추가 될 때 `CarouselView`의 스크롤 동작을 나타내는 `ItemsUpdatingScrollMode` 속성을 정의 합니다. 이 속성에 대 한 자세한 내용은 [새 항목이 추가 될 때 스크롤 위치 제어](scrolling.md#control-scroll-position-when-new-items-are-added)를 참조 하세요.

사용자가 스크롤하면 데이터를 증분 로드할 수도 [`CarouselView`](xref:Xamarin.Forms.CarouselView) . 자세한 내용은 [데이터를 증분 로드](#load-data-incrementally)를 참조 하세요.

## <a name="populate-a-carouselview-with-data"></a>데이터를 사용 하 여 CarouselView 채우기

[`CarouselView`](xref:Xamarin.Forms.CarouselView) [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 속성을 `IEnumerable`를 구현 하는 컬렉션으로 설정 하 여 데이터로 채워집니다. 문자열 배열에서 `ItemsSource` 속성을 초기화 하 여 XAML에 항목을 추가할 수 있습니다.

```xaml
<CarouselView>
    <CarouselView.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Baboon</x:String>
            <x:String>Capuchin Monkey</x:String>
            <x:String>Blue Monkey</x:String>
            <x:String>Squirrel Monkey</x:String>
            <x:String>Golden Lion Tamarin</x:String>
            <x:String>Howler Monkey</x:String>
            <x:String>Japanese Macaque</x:String>
        </x:Array>
    </CarouselView.ItemsSource>
</CarouselView>
```

> [!NOTE]
> `x:Array` 요소는 배열의 항목 유형을 나타내는 `Type` 특성이 필요합니다.

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView();
carouselView.ItemsSource = new string[]
{
    "Baboon",
    "Capuchin Monkey",
    "Blue Monkey",
    "Squirrel Monkey",
    "Golden Lion Tamarin",
    "Howler Monkey",
    "Japanese Macaque"
};
```

> [!IMPORTANT]
> 기본 컬렉션에서 항목이 추가, 제거 또는 변경 될 때 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 를 새로 고쳐야 하는 경우 기본 컬렉션은 `ObservableCollection`와 같은 속성 변경 알림을 보내는 `IEnumerable` 컬렉션 이어야 합니다.

기본적으로 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 는 항목을 가로로 표시 합니다. 다음 스크린샷에는 iOS 및 Android에서 다른 문자열 항목을 표시 하는 `CarouselView` 표시 됩니다.

[![IOS 및 Android에서 텍스트 항목을 포함 하는 CarouselView의 스크린샷](populate-data-images/text.png "CarouselView의 텍스트 항목")](populate-data-images/text-large.png#lightbox "CarouselView의 텍스트 항목")

[`CarouselView`](xref:Xamarin.Forms.CarouselView) 방향을 변경 하는 방법에 대 한 자세한 내용은 [xamarin.ios CarouselView Layout](layout.md)을 참조 하세요. `CarouselView`에서 각 항목의 모양을 정의 하는 방법에 대 한 자세한 내용은 [항목 모양 정의](#define-item-appearance)를 참조 하세요.

### <a name="data-binding"></a>데이터 바인딩

데이터 바인딩을 사용 하 여 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 속성을 `IEnumerable` 컬렉션에 바인딩하면 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 데이터를 채울 수 있습니다. XAML에서이 작업은 `Binding` 태그 확장을 사용 하 여 구현 됩니다.

```xaml
<CarouselView ItemsSource="{Binding Monkeys}" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

이 예제에서 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 속성 데이터는 연결 된 viewmodel의 `Monkeys` 속성에 바인딩됩니다.

> [!NOTE]
> 컴파일된 바인딩을 사용하면 Xamarin.Forms 응용 프로그램에서 데이터 바인딩 성능을 향상시킬 수 있습니다. 자세한 내용은 [컴파일된 바인딩](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)을 참조하세요.

데이터 바인딩에 대한 자세한 내용은 [Xamarin.Forms 데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)을 참조하세요.

## <a name="define-item-appearance"></a>항목 모양 정의

[`CarouselView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) 속성을 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)로 설정 하 여 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 에 있는 각 항목의 모양을 정의할 수 있습니다.

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

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

carouselView.ItemTemplate = new DataTemplate(() =>
{
    Label nameLabel = new Label { ... };
    nameLabel.SetBinding(Label.TextProperty, "Name");

    Image image = new Image { ... };
    image.SetBinding(Image.SourceProperty, "ImageUrl");

    Label locationLabel = new Label { ... };
    locationLabel.SetBinding(Label.TextProperty, "Location");

    Label detailsLabel = new Label { ... };
    detailsLabel.SetBinding(Label.TextProperty, "Details");

    StackLayout stackLayout = new StackLayout
    {
        Children = { nameLabel, image, locationLabel, detailsLabel }
    };

    Frame frame = new Frame { ... };
    StackLayout rootStackLayout = new StackLayout
    {
        Children = { frame }
    };

    return rootStackLayout;
});
```

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 에 지정 된 요소는 `CarouselView`에서 각 항목의 모양을 정의 합니다. 이 예제에서 `DataTemplate` 내의 레이아웃은 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에 의해 관리 되 고 데이터는 [`Image`](xref:Xamarin.Forms.Image) 개체 및 세 개의 [`Label`](xref:Xamarin.Forms.Label) 개체와 함께 표시 됩니다 .이 개체는 모두 `Monkey` 클래스의 속성에 바인딩됩니다.

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

다음 스크린샷은 각 항목의 템플릿 결과를 보여 줍니다.

[![IOS 및 Android에서 각 항목이 템플릿 인 CarouselView의 스크린샷](populate-data-images/datatemplate.png "CarouselView의 템플릿 항목")](populate-data-images/datatemplate-large.png#lightbox "CarouselView의 템플릿 항목")

데이터 템플릿에 대한 자세한 내용은 [Xamarin.Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)을 참조하세요.

## <a name="choose-item-appearance-at-runtime"></a>런타임에 항목 모양 선택

[`CarouselView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) 속성을 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) 개체로 설정 하 여 항목 값을 기준으로 런타임에 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 에 있는 각 항목의 모양을 선택할 수 있습니다.

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CarouselViewDemos.Controls"
             x:Class="CarouselViewDemos.Views.HorizontalLayoutDataTemplateSelectorPage">
    <ContentPage.Resources>
        <DataTemplate x:Key="AmericanMonkeyTemplate">
            ...
        </DataTemplate>

        <DataTemplate x:Key="OtherMonkeyTemplate">
            ...
        </DataTemplate>

        <controls:MonkeyDataTemplateSelector x:Key="MonkeySelector"
                                             AmericanMonkey="{StaticResource AmericanMonkeyTemplate}"
                                             OtherMonkey="{StaticResource OtherMonkeyTemplate}" />
    </ContentPage.Resources>

    <CarouselView ItemsSource="{Binding Monkeys}"
                  ItemTemplate="{StaticResource MonkeySelector}" />
</ContentPage>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView
{
    ItemTemplate = new MonkeyDataTemplateSelector { ... }
};
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

[`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) 속성은 `MonkeyDataTemplateSelector` 개체로 설정 됩니다. 다음 예제에서는 `MonkeyDataTemplateSelector` 클래스를 보여 줍니다.

```csharp
public class MonkeyDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate AmericanMonkey { get; set; }
    public DataTemplate OtherMonkey { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((Monkey)item).Location.Contains("America") ? AmericanMonkey : OtherMonkey;
    }
}
```

`MonkeyDataTemplateSelector` 클래스는 다른 데이터 템플릿으로 설정 된 `AmericanMonkey` 및 `OtherMonkey` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 속성을 정의 합니다. `OnSelectTemplate` 재정의는 원숭이 이름에 "아메리카"가 포함 된 경우 `AmericanMonkey` 템플릿을 반환 합니다. 원숭이 이름에 "아메리카"가 포함 되지 않은 경우 `OnSelectTemplate` 재정의는 해당 데이터를 회색으로 표시 하는 `OtherMonkey` 템플릿을 반환 합니다.

[![IOS 및 Android에서 CarouselView 런타임 항목 템플릿 선택의 스크린샷](populate-data-images/datatemplateselector.png "CarouselView의 런타임 항목 템플릿 선택")](populate-data-images/datatemplateselector-large.png#lightbox "CarouselView의 런타임 항목 템플릿 선택")

데이터 템플릿 선택기에 대 한 자세한 내용은 [DataTemplateSelector 만들기](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)를 참조 하세요.

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)사용 하는 경우 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 개체의 루트 요소를 `ViewCell`로 설정 하지 마십시오. 이로 인해 `CarouselView`에는 셀 개념이 없으므로 예외가 throw 됩니다.

## <a name="display-indicators"></a>표시기 표시

`CarouselView`의 항목 수와 현재 위치를 나타내는 표시기는 `CarouselView`옆에 표시 될 수 있습니다. `IndicatorView` 컨트롤을 사용 하 여이 작업을 수행할 수 있습니다.

```xaml
<StackLayout>
    <CarouselView ItemsSource="{Binding Monkeys}"
                  IndicatorView="indicatorView">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView x:Name="indicatorView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="DarkGray"
                   HorizontalOptions="Center" />
</StackLayout>
```

이 예에서는 `IndicatorView` `CarouselView`아래에서 렌더링 되며 `CarouselView`의 각 항목에 대 한 표시기가 표시 됩니다. `IndicatorView` `CarouselView.IndicatorView` 속성을 `IndicatorView` 개체로 설정 하 여 데이터로 채워집니다. 각 표시기는 연한 회색 원 이지만 `CarouselView`의 현재 항목을 나타내는 표시기는 진한 회색입니다.

[![IOS 및 Android에서 CarouselView 및 IndicatorView의 스크린샷](populate-data-images/indicators.png "IndicatorView 원")](populate-data-images/indicators-large.png#lightbox "IndicatorView 원")

> [!IMPORTANT]
> `CarouselView.IndicatorView` 속성을 설정 하면 `IndicatorView.Position` 속성이 `CarouselView.Position` 속성에 바인딩하고 `IndicatorView.ItemsSource` 속성은 `CarouselView.ItemsSource` 속성에 바인딩합니다.

표시기에 대 한 자세한 내용은 [IndicatorView](~/xamarin-forms/user-interface/indicatorview.md)를 참조 하세요.

## <a name="pull-to-refresh"></a>새로 고치려면 끌어오기

[`CarouselView`](xref:Xamarin.Forms.CarouselView) 는 `RefreshView`를 통해 기능을 새로 고치는 기능을 지원 합니다 .이 기능을 사용 하면 항목을 축소 하 여 데이터를 새로 고칠 수 있습니다. `RefreshView`은 자식에서 스크롤 가능한 콘텐츠를 지 원하는 경우 해당 자식에 대 한 기능을 새로 고치는 가져오기를 제공 하는 컨테이너 컨트롤입니다. 따라서 `RefreshView`의 자식으로 설정 하 여 `CarouselView`에 대 한 끌어오기를 새로 고칩니다.

```xaml
<RefreshView IsRefreshing="{Binding IsRefreshing}"
             Command="{Binding RefreshCommand}">
    <CarouselView ItemsSource="{Binding Animals}">
        ...
    </CarouselView>
</RefreshView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
RefreshView refreshView = new RefreshView();
ICommand refreshCommand = new Command(() =>
{
    // IsRefreshing is true
    // Refresh data here
    refreshView.IsRefreshing = false;
});
refreshView.Command = refreshCommand;

CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
refreshView.Content = carouselView;
// ...
```

사용자가 새로 고침을 시작 하면 `Command` 속성으로 정의 된 `ICommand` 실행 되어 표시 되는 항목을 새로 고쳐야 합니다. 새로 고침이 발생 하는 동안 애니메이션 처리 원으로 구성 된 새로 고침 시각화가 표시 됩니다.

[![IOS 및 Android에서 CarouselView 끌어오기-새로 고침의 스크린샷](populate-data-images/pull-to-refresh.png "CarouselView 가져오기-새로 고침")](populate-data-images/pull-to-refresh-large.png#lightbox "CarouselView 가져오기-새로 고침")

`RefreshView.IsRefreshing` 속성의 값은 `RefreshView`의 현재 상태를 나타냅니다. 사용자가 새로 고침을 트리거하는 경우이 속성은 자동으로 `true`로 전환 됩니다. 새로 고침이 완료 되 면 속성을 `false`으로 다시 설정 해야 합니다.

`RefreshView`에 대 한 자세한 내용은 [Xamarin.ios RefreshView](~/xamarin-forms/user-interface/refreshview.md)를 참조 하세요.

## <a name="load-data-incrementally"></a>증분 방식으로 데이터 로드

사용자가 항목을 스크롤하면 데이터를 증분 로드 하는 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 지원 됩니다. 이렇게 하면 사용자가 스크롤할 때 웹 서비스에서 데이터 페이지를 비동기적으로 로드 하는 등의 시나리오를 사용할 수 있습니다. 또한 더 많은 데이터를 로드 하는 지점은 사용자가 빈 공간을 보거나 스크롤에서 중지 되도록 구성할 수 있습니다.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) 는 다음 속성을 정의 하 여 데이터의 증분 로드를 제어 합니다.

- `int`형식의 `RemainingItemsThreshold``RemainingItemsThresholdReached` 이벤트가 발생 하는 목록에 아직 표시 되지 않는 항목의 임계값입니다.
- `RemainingItemsThresholdReachedCommand``RemainingItemsThreshold`에 도달할 때 실행 되는 `ICommand`형식입니다.
- `RemainingItemsThresholdReachedCommandParameter` 형식의 `object` - `RemainingItemsThresholdReachedCommand`에 전달되는 매개 변수입니다.

또한 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 는 `RemainingItemsThreshold` 항목이 표시 되지 않을 만큼 `CarouselView` 스크롤 될 때 발생 하는 `RemainingItemsThresholdReached` 이벤트를 정의 합니다. 이 이벤트를 처리 하 여 더 많은 항목을 로드할 수 있습니다. 또한 `RemainingItemsThresholdReached` 이벤트가 발생 하면 `RemainingItemsThresholdReachedCommand` 실행 되어 viewmodel에서 증분 데이터 로드가 발생 하도록 할 수 있습니다.

`RemainingItemsThreshold` 속성의 기본값은-1입니다 .이 값은 `RemainingItemsThresholdReached` 이벤트가 발생 되지 않음을 나타냅니다. 속성 값이 0 이면 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 의 최종 항목이 표시 될 때 `RemainingItemsThresholdReached` 이벤트가 발생 합니다. 값이 0 보다 큰 경우에는 `ItemsSource`에 아직 스크롤되지 않는 항목 수가 포함 되어 있으면 `RemainingItemsThresholdReached` 이벤트가 발생 합니다.

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView) 은 `RemainingItemsThreshold` 속성의 유효성을 검사 하 여 해당 값이 항상-1 보다 크거나 같도록 합니다.

다음 XAML 예제에서는 데이터를 증분 로드 하는 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 보여 줍니다.

```xaml
<CarouselView ItemsSource="{Binding Animals}"
              RemainingItemsThreshold="2"
              RemainingItemsThresholdReached="OnCarouselViewRemainingItemsThresholdReached"
              RemainingItemsThresholdReachedCommand="{Binding LoadMoreDataCommand}">
    ...
</CarouselView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView
{
    RemainingItemsThreshold = 2
};
carouselView.RemainingItemsThresholdReached += OnCollectionViewRemainingItemsThresholdReached;
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
```

이 코드 예제에서는 두 개의 항목이 아직 스크롤되지 않는 경우 `RemainingItemsThresholdReached` 이벤트가 발생 하 고, 응답으로 `OnCollectionViewRemainingItemsThresholdReached` 이벤트 처리기를 실행 합니다.

```csharp
void OnCollectionViewRemainingItemsThresholdReached(object sender, EventArgs e)
{
    // Retrieve more data here and add it to the CollectionView's ItemsSource collection.
}
```

> [!NOTE]
> `RemainingItemsThresholdReachedCommand`를 viewmodel의 `ICommand` 구현에 바인딩하여 증분 방식으로 데이터를 로드할 수도 있습니다.

## <a name="related-links"></a>관련 링크

- [CarouselView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [Xamarin.ios IndicatorView](~/xamarin-forms/user-interface/indicatorview.md)
- [Xamarin.ios RefreshView](~/xamarin-forms/user-interface/refreshview.md)
- [Xamarin.Forms 데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin. Forms DataTemplateSelector 만들기](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
