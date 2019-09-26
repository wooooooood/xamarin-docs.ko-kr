---
title: Xamarin.ios CollectionView 데이터
description: CollectionView는 System.windows.controls.itemscontrol.itemssource 속성을 IEnumerable을 구현 하는 컬렉션으로 설정 하 여 데이터로 채워집니다.
ms.prod: xamarin
ms.assetid: E1783E34-1C0F-401A-80D5-B2BE5508F5F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/13/2019
ms.openlocfilehash: 6942baed6af2a2e9b2c713a8fe08cf4c8ed4416b
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2019
ms.locfileid: "69888538"
---
# <a name="xamarinforms-collectionview-data"></a>Xamarin.ios CollectionView 데이터

![](~/media/shared/preview.png "이 API는 현재 시험판임")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)표시할 데이터 및 모양을 정의 하는 다음 속성을 정의 합니다.

- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)형식의 `IEnumerable`는 표시 될 항목의 컬렉션을 지정 하 고 기본값 `null`은입니다.
- [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)형식의 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)는 표시할 항목 컬렉션의 각 항목에 적용할 템플릿을 지정 합니다.

이러한 속성은 개체에 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 의해 지원 됩니다. 즉, 속성은 데이터 바인딩의 대상이 될 수 있습니다.

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)새 항목이 `ItemsUpdatingScrollMode` 추가 `CollectionView` 될 때의 스크롤 동작을 나타내는 속성을 정의 합니다. 이 속성에 대 한 자세한 내용은 [새 항목이 추가 될 때 스크롤 위치 제어](scrolling.md#control-scroll-position-when-new-items-are-added)를 참조 하세요.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)사용자가 스크롤하면 데이터를 증분 로드할 수도 있습니다. 자세한 내용은 [데이터를 증분 로드](#load-data-incrementally)를 참조 하세요.

## <a name="populate-a-collectionview-with-data"></a>데이터를 사용 하 여 CollectionView 채우기

는 [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 속성을를 구현 `IEnumerable`하는 컬렉션으로 설정 하 여 데이터로 채워집니다. 문자열 배열에서 속성을 `ItemsSource` 초기화 하 여 XAML에 항목을 추가할 수 있습니다.

```xaml
<CollectionView>
  <CollectionView.ItemsSource>
    <x:Array Type="{x:Type x:String}">
      <x:String>Baboon</x:String>
      <x:String>Capuchin Monkey</x:String>
      <x:String>Blue Monkey</x:String>
      <x:String>Squirrel Monkey</x:String>
      <x:String>Golden Lion Tamarin</x:String>
      <x:String>Howler Monkey</x:String>
      <x:String>Japanese Macaque</x:String>
    </x:Array>
  </CollectionView.ItemsSource>
</CollectionView>
```

> [!NOTE]
> `x:Array` 요소는 배열의 항목 유형을 나타내는 `Type` 특성이 필요합니다.

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView();
collectionView.ItemsSource = new string[]
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
> 기본 컬렉션 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 에서 항목이 추가, 제거 또는 변경 될 때를 새로 고쳐야 하는 경우 기본 컬렉션 `IEnumerable` 은와 `ObservableCollection`같은 속성 변경 알림을 보내는 컬렉션 이어야 합니다.

기본적으로는 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 다음 스크린샷에 표시 된 것 처럼 세로 목록에 항목을 표시 합니다.

[![IOS 및 Android에서 텍스트 항목을 포함 하는 CollectionView의 스크린샷](populate-data-images/text.png "CollectionView의 텍스트 항목")](populate-data-images/text-large.png#lightbox "CollectionView의 텍스트 항목")

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 레이아웃을 변경 하는 방법에 대 한 자세한 내용은 [레이아웃 지정](layout.md)을 참조 하세요. 에서 `CollectionView`각 항목의 모양을 정의 하는 방법에 대 한 자세한 내용은 [항목 모양 정의](#define-item-appearance)를 참조 하세요.

### <a name="data-binding"></a>데이터 바인딩

[`CollectionView`](xref:Xamarin.Forms.CollectionView)데이터 바인딩을 사용 하 여 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 속성을 `IEnumerable` 컬렉션에 바인딩하면 데이터를 데이터로 채울 수 있습니다. XAML에서이 작업은 `Binding` 태그 확장을 사용 하 여 구현 됩니다.

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

이 예제에서 속성 데이터 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 는 연결 된 viewmodel의 `Monkeys` 속성에 바인딩됩니다.

> [!NOTE]
> 컴파일된 바인딩을 사용하면 Xamarin.Forms 응용 프로그램에서 데이터 바인딩 성능을 향상시킬 수 있습니다. 자세한 내용은 [컴파일된 바인딩](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)을 참조하세요.

데이터 바인딩에 대한 자세한 내용은 [Xamarin.Forms 데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)을 참조하세요.

## <a name="define-item-appearance"></a>항목 모양 정의

에서 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 각 항목의 모양은 [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) 속성을로 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)설정 하 여 정의할 수 있습니다.

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
    ...
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

collectionView.ItemTemplate = new DataTemplate(() =>
{
    Grid grid = new Grid { Padding = 10 };
    grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
    grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Auto });
    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Auto });

    Image image = new Image { Aspect = Aspect.AspectFill, HeightRequest = 60, WidthRequest = 60 };
    image.SetBinding(Image.SourceProperty, "ImageUrl");

    Label nameLabel = new Label { FontAttributes = FontAttributes.Bold };
    nameLabel.SetBinding(Label.TextProperty, "Name");

    Label locationLabel = new Label { FontAttributes = FontAttributes.Italic, VerticalOptions = LayoutOptions.End };
    locationLabel.SetBinding(Label.TextProperty, "Location");

    Grid.SetRowSpan(image, 2);

    grid.Children.Add(image);
    grid.Children.Add(nameLabel, 1, 0);
    grid.Children.Add(locationLabel, 1, 1);

    return grid;
});
```

에서 지정 된 요소는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 목록에 있는 각 항목의 모양을 정의 합니다. `DataTemplate` 이 예제에서 내의 레이아웃은를 [`Grid`](xref:Xamarin.Forms.Grid)통해 관리 됩니다. 는 `Grid` [`Image`](xref:Xamarin.Forms.Image) [`Label`](xref:Xamarin.Forms.Label) 모두 클래스`Monkey` 의 속성에 바인딩되는 개체와 두 개의 개체를 포함 합니다.

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

다음 스크린샷에는 목록의 각 항목에 대 한 템플릿 결과가 나와 있습니다.

[![IOS 및 Android에서 각 항목이 템플릿 인 CollectionView의 스크린샷](populate-data-images/datatemplate.png "CollectionView의 템플릿 항목")](populate-data-images/datatemplate-large.png#lightbox "CollectionView의 템플릿 항목")

데이터 템플릿에 대한 자세한 내용은 [Xamarin.Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)을 참조하세요.

## <a name="choose-item-appearance-at-runtime"></a>런타임에 항목 모양 선택

에서 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 각 항목의 모양은 [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) 속성 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) 을 개체로 설정 하 여 항목 값에 따라 런타임에 선택할 수 있습니다.

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CollectionViewDemos.Controls">
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

    <CollectionView ItemsSource="{Binding Monkeys}"
                    ItemTemplate="{StaticResource MonkeySelector}" />
</ContentPage>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ItemTemplate = new MonkeyDataTemplateSelector { ... }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

속성은 `MonkeyDataTemplateSelector` 개체로 설정 됩니다. [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) 다음 예제에서는 클래스를 `MonkeyDataTemplateSelector` 보여 줍니다.

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

클래스 `MonkeyDataTemplateSelector` 는 다른 `AmericanMonkey` 데이터 `OtherMonkey` 템플릿으로 설정 된 및 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 속성을 정의 합니다. 재정의 `OnSelectTemplate` 는 원숭이 이름 `AmericanMonkey` 및 위치를 청록색으로 표시 하는 템플릿 (원숭이 이름에 "아메리카"가 포함 된 경우)을 반환 합니다. 원숭이 이름에 "아메리카"가 포함 되어 있지 않으면 `OnSelectTemplate` 재정의는 다음 `OtherMonkey` 의 원숭이 이름과 위치를 은색에 표시 하는 템플릿을 반환 합니다.

[![IOS 및 Android에서 CollectionView 런타임 항목 템플릿 선택의 스크린샷](populate-data-images/datatemplateselector.png "CollectionView의 런타임 항목 템플릿 선택")](populate-data-images/datatemplateselector-large.png#lightbox "CollectionView의 런타임 항목 템플릿 선택")

데이터 템플릿 선택기에 대 한 자세한 내용은 [DataTemplateSelector 만들기](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)를 참조 하세요.

> [!IMPORTANT]
> 를 사용 [`CollectionView`](xref:Xamarin.Forms.CollectionView)하는 경우 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 개체 `ViewCell`의 루트 요소를으로 설정 하지 마십시오. 이로 인해에는 셀 개념이 없기 때문 `CollectionView` 에 예외가 throw 됩니다.

## <a name="load-data-incrementally"></a>증분 방식으로 데이터 로드

[`CollectionView`](xref:Xamarin.Forms.CollectionView)사용자가 항목을 스크롤할 때 증분 데이터 로드를 지원 합니다. 이렇게 하면 사용자가 스크롤할 때 웹 서비스에서 데이터 페이지를 비동기적으로 로드 하는 등의 시나리오를 사용할 수 있습니다. 또한 더 많은 데이터를 로드 하는 지점은 사용자가 빈 공간을 보거나 스크롤에서 중지 되도록 구성할 수 있습니다.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)데이터의 증분 로드를 제어 하는 다음 속성을 정의 합니다.

- `RemainingItemsThreshold`형식의 `int`, `RemainingItemsThresholdReached` 이벤트가 발생 되는 목록에 아직 표시 되지 않는 항목의 임계값입니다.
- `RemainingItemsThresholdReachedCommand`에 도달할 `ICommand` `RemainingItemsThreshold` 때 실행 되는 형식의입니다.
- `object` 형식의 `RemainingItemsThresholdReachedCommandParameter` - `RemainingItemsThresholdReachedCommand`에 전달되는 매개 변수입니다.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)또한 `RemainingItemsThreshold` 항목이 표시 `RemainingItemsThresholdReached` 되지 않을 정도로 `CollectionView` 가 충분히 스크롤 될 때 발생 하는 이벤트를 정의 합니다. 이 이벤트를 처리 하 여 더 많은 항목을 로드할 수 있습니다. 또한 `RemainingItemsThresholdReached` 이벤트가 발생 `RemainingItemsThresholdReachedCommand` 하면이 실행 되어 viewmodel에서 증분 데이터 로드가 발생 하도록 할 수 있습니다.

`RemainingItemsThreshold` 속성의 기본값은-1 이며이는 `RemainingItemsThresholdReached` 이벤트가 발생 하지 않음을 나타냅니다. 속성 값이 0 `RemainingItemsThresholdReached` 이면의 마지막 항목이 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 표시 될 때 이벤트가 발생 합니다. 0 `RemainingItemsThresholdReached` 보다 큰 값의 경우에 아직 스크롤되지 않는 항목 수가에 `ItemsSource` 포함 되어 있으면 이벤트가 발생 합니다.

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)속성의 `RemainingItemsThreshold` 값이 항상-1 보다 크거나 같도록 속성의 유효성을 검사 합니다.

다음 XAML 예제에서는 데이터를 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 증분 로드 하는을 보여 줍니다.

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                RemainingItemsThreshold="5"
                RemainingItemsThresholdReached="OnCollectionViewRemainingItemsThresholdReached">
    ...
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    RemainingItemsThreshold = 5
};
collectionView.RemainingItemsThresholdReached += OnCollectionViewRemainingItemsThresholdReached;
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
```

이 코드 예제에서 이벤트는 `RemainingItemsThresholdReached` 5 개의 항목이 아직 스크롤되지 않고 `OnCollectionViewRemainingItemsThresholdReached` 이벤트 처리기를 실행 하는 경우에 발생 합니다.

```csharp
void OnCollectionViewRemainingItemsThresholdReached(object sender, EventArgs e)
{
    // Retrieve more data here and add it to the CollectionView's ItemsSource collection.
}
```

> [!NOTE]
> 를 `RemainingItemsThresholdReachedCommand` viewmodel의 `ICommand` 구현에 바인딩하여 데이터를 증분 로드할 수도 있습니다.

## <a name="related-links"></a>관련 링크

- [CollectionView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin Forms 데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin. Forms DataTemplateSelector 만들기](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
