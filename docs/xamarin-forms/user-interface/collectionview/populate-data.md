---
title: Xamarin.Forms CollectionView 데이터로 채우기
description: CollectionView IEnumerable을 구현 하는 모든 컬렉션이 ItemsSource 속성을 설정 하 여 데이터를 사용 하 여 채워집니다.
ms.prod: xamarin
ms.assetid: E1783E34-1C0F-401A-80D5-B2BE5508F5F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/15/2019
ms.openlocfilehash: 70b241944376782ec4c9446878ee2a19dcee2bbd
ms.sourcegitcommit: 5d4e6677224971e2bc0268f405d192d0358c74b8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58330035"
---
# <a name="populate-xamarinforms-collectionview-with-data"></a>Xamarin.Forms CollectionView 데이터로 채우기

![미리 보기](~/media/shared/preview.png)

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

> [!IMPORTANT]
> `CollectionView` 는 현재 미리 보기, 및 일부 계획된 기능 부족 합니다. 또한 API 구현을 완료 되 면 변경할 수 있습니다.

`CollectionView` 해당 모양과 표시할 데이터를 정의 하는 다음 속성을 정의 합니다.

- `ItemsSource`형식의 `IEnumerable`에 표시 될 항목의 컬렉션을 지정의 기본 값이 `null`합니다.
- `ItemTemplate`를 형식 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)에 표시할 항목을 컬렉션의 각 항목에 적용할 템플릿을 지정 합니다.

이러한 속성에 의해 지원 됩니다 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 개체 속성을 데이터 바인딩의 대상 수 있음을 의미 합니다.

## <a name="populate-a-collectionview-with-data"></a>데이터 CollectionView 채우기

A `CollectionView` 설정 하 여 데이터를 채운 해당 `ItemsSource` 속성을 구현 하는 컬렉션 `IEnumerable`합니다. 초기화 하 여 XAML에서 항목을 추가할 수는 `ItemsSource` 문자열의 배열에서 속성:

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

해당 하는 C# 코드가입니다.

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
> 경우는 `CollectionView` 은 항목 추가, 제거 또는 내부 컬렉션에서 변경 된 새로 고침에 필요한 기본 컬렉션 이어야 합니다는 `IEnumerable` 와 같은 속성을 전송 하는 컬렉션 변경 알림 `ObservableCollection`합니다.

기본적으로 `CollectionView` 다음 스크린샷과에서 같이 세로 목록에 항목을 표시 합니다.

[![스크린 샷의 CollectionView iOS 및 Android에서 텍스트 항목이 포함 된](populate-data-images/text.png "수집 뷰의 텍스트 항목")](populate-data-images/text-large.png#lightbox "수집 뷰의 텍스트 항목")

변경 하는 방법에 대 한 정보에 대 한 합니다 `CollectionView` 레이아웃을 참조 하세요 [레이아웃을 지정할](layout.md)합니다. 각 항목의 모양을 정의 하는 방법에 대 한 정보에 대 한 합니다 `CollectionView`를 참조 하세요 [목록 항목 모양을 정의](#define-list-item-appearance).

### <a name="data-binding"></a>데이터 바인딩

`CollectionView` 바인딩할 데이터 바인딩을 사용 하 여 데이터로 채울 수 있습니다 해당 `ItemsSource` 속성을는 `IEnumerable` 컬렉션입니다. XAML을 사용 하 여 수행 됩니다이 `Binding` 태그 확장:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

이 예제에서는 `ItemsSource` 속성 데이터를 바인딩하는 `Monkeys` 연결된 보기 모델의 속성입니다.

> [!NOTE]
> 컴파일된 바인딩을 사용하면 Xamarin.Forms 응용 프로그램에서 데이터 바인딩 성능을 향상시킬 수 있습니다. 자세한 내용은 [컴파일된 바인딩](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)을 참조하십시오.

데이터 바인딩에 대한 자세한 내용은 [Xamarin.Forms 데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)을 참조하세요.

## <a name="define-item-appearance"></a>항목 모양을 정의합니다

각 항목의 모양을 합니다 `CollectionView` 설정 하 여 정의할 수 있습니다 합니다 `CollectionView.ItemTemplate` 속성을을 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

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

해당 하는 C# 코드가입니다.

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

에 지정 된 요소를 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 목록의 각 항목의 모양을 정의 합니다. 예제에서는 레이아웃 내에서 `DataTemplate` 에서 관리 되는 [ `Grid` ](xref:Xamarin.Forms.Grid)합니다. 합니다 `Grid` 포함을 [ `Image` ](xref:Xamarin.Forms.Image) 개체와 두 개의 [ `Label` ](xref:Xamarin.Forms.Label) 개체의 속성을 바인딩하는 모든는 `Monkey` 클래스:

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

다음 스크린샷에서 목록에서 템플릿 결과 각 항목을 표시 합니다.

[![스크린 샷의 CollectionView 여기서 각 항목은 iOS 및 Android에서 템플릿](populate-data-images/datatemplate.png "수집 뷰의 템플릿 항목")](populate-data-images/datatemplate-large.png#lightbox "수집 뷰의 템플릿 항목")

데이터 템플릿에 대한 자세한 내용은 [Xamarin.Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)을 참조하세요.

## <a name="related-links"></a>관련 링크

- [CollectionView (샘플)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Xamarin.Forms 데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin.Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)