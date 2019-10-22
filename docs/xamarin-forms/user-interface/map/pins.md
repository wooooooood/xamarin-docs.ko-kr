---
title: Xamarin.ios 맵 핀
description: 이 문서에서는 Xamarin.ios 맵에 핀을 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: F8FC081B-A811-4FBB-B8F8-30D6FD36BD40
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 09/23/2019
ms.openlocfilehash: 76535f9c31a9dc138e132a3e582b986daf89bdb0
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697670"
---
# <a name="xamarinforms-map-pins"></a>Xamarin.ios 맵 핀

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Xamarin.ios `Maps` 컨트롤을 사용 하면 위치를 `Pin` 개체로 표시할 수 있습니다. @No__t_0는 클릭 하거나 탭 할 때 정보 창을 여는 지도 표식입니다.

@No__t_0 클래스에는 다음과 같은 속성이 있습니다.

- `Type`은 `PinType` 열거형 값입니다. 제네릭, 위치, SavedPin 또는 SearchResult입니다.
- `Position`는 핀의 위도 및 경도를 포함 하는 `Position` 인스턴스입니다.
- `Label`은 일반적으로 pin 제목으로 표시 되는 `string`입니다.
- `Address`는 정보 창에 표시 되는 `string`입니다. 주소가 아니라 모든 `string` 콘텐츠가 될 수 있습니다.

이러한 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원 됩니다. 즉, `Pin` 데이터 바인딩의 대상이 될 수 있습니다. 자세한 내용은 [데이터 바인딩을 사용 하 여 Pin 만들기](#create-pins-with-data-binding)를 참조 하세요.

## <a name="create-map-pins"></a>지도 핀 만들기

@No__t_0 인스턴스는 코드에서 만들고 지도에 추가할 수 있습니다.

```csharp
Pin pin1 = new Pin
{
    Type = PinType.Place,
    Position = new Position(47.6368678, -122.137305),
    Label = "Example Pin 1",
    Address = "Example custom details..."
};
map.Pins.Add(pin1);
```

> [!NOTE]
> @No__t_0 값은 플랫폼에 따라 핀이 렌더링 되는 방식에 영향을 줍니다. Pin의 모양을 사용자 지정 하려면 사용자 지정 렌더러를 만들어야 합니다. 자세한 내용은 [지도 Pin 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)을 참조 하세요.

## <a name="create-pins-with-data-binding"></a>데이터 바인딩을 사용 하 여 pin 만들기

[@No__t_1](xref:Xamarin.Forms.Maps.Map) 클래스는 다음 속성을 노출 합니다.

- `ItemsSource` – 표시 되는 `IEnumerable` 항목의 컬렉션을 지정 합니다.
- `ItemTemplate` – 표시 된 항목 컬렉션의 각 항목에 적용할 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 를 지정 합니다.
- `ItemTemplateSelector` – 런타임에 항목에 대 한 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 를 선택 하는 데 사용 되는 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) 를 지정 합니다.

> [!NOTE]
> @No__t_1 및 `ItemTemplateSelector` 속성이 모두 설정 된 경우 `ItemTemplate` 속성이 우선적으로 적용 됩니다.

데이터 바인딩을 사용 하 여 `ItemsSource` 속성을 `IEnumerable` 컬렉션에 바인딩하여 [`Map`](xref:Xamarin.Forms.Maps.Map) 를 데이터로 채울 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="WorkingWithMaps.PinItemsSourcePage">
    <Grid>
        ...
        <maps:Map x:Name="map"
                  MoveToLastRegionOnLayoutChange="false"
                  ItemsSource="{Binding Locations}">
            <maps:Map.ItemTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </maps:Map.ItemTemplate>
        </maps:Map>
        ...
    </Grid>
</ContentPage>
```

@No__t_0 속성 데이터는 연결 된 뷰 모델의 `Locations` 속성에 바인딩되고,이 속성은 사용자 지정 유형인 `Location` 개체의 `ObservableCollection` 반환 합니다. 각 `Location` 개체는 `string` 형식의 `Address` 및 `Description` 속성과 [`Position`](xref:Xamarin.Forms.Maps.Position)형식의 `Position` 속성을 정의 합니다.

@No__t_0 컬렉션에 있는 각 항목의 모양은 데이터를 적절 한 속성에 바인딩하는 [`Pin`](xref:Xamarin.Forms.Maps.Pin) 개체를 포함 하는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) `ItemTemplate` 속성을 설정 하 여 정의 됩니다.

다음 스크린샷에는 데이터 바인딩을 사용 하 여 [`Pin`](xref:Xamarin.Forms.Maps.Pin) 컬렉션을 표시 하는 [`Map`](xref:Xamarin.Forms.Maps.Map) 표시 됩니다.

[![IOS 및 Android에서 데이터 바인딩된 핀이 있는 지도의 스크린샷](map-images/pins-itemssource.png "데이터 바인딩된 pin을 사용 하는 맵")](map-images/pins-itemssource-large.png#lightbox "데이터 바인딩된 pin을 사용 하는 맵")

### <a name="choose-item-appearance-at-runtime"></a>런타임에 항목 모양 선택

@No__t_0 컬렉션에 있는 각 항목의 모양은 `ItemTemplateSelector` 속성을 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)로 설정 하 여 항목 값에 따라 런타임에 선택할 수 있습니다.

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:WorkingWithMaps"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
    <ContentPage.Resources>
        <local:MapItemTemplateSelector x:Key="MapItemTemplateSelector">
            <local:MapItemTemplateSelector.DefaultTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </local:MapItemTemplateSelector.DefaultTemplate>
            <local:MapItemTemplateSelector.XamarinTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="Xamarin!" />
                </DataTemplate>
            </local:MapItemTemplateSelector.XamarinTemplate>    
        </local:MapItemTemplateSelector>
    </ContentPage.Resources>

    <Grid>
        ...
        <maps:Map x:Name="map"
                  ItemsSource="{Binding Locations}"
                  ItemTemplateSelector="{StaticResource MapItemTemplateSelector}" />
        ...
    </Grid>
</ContentPage>
```

다음 예제에서는 `MapItemTemplateSelector` 클래스를 보여 줍니다.

```csharp
public class MapItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate XamarinTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((Location)item).Address.Contains("San Francisco") ? XamarinTemplate : DefaultTemplate;
    }
}
```

@No__t_0 클래스는 다른 데이터 템플릿으로 설정 된 `DefaultTemplate` 및 `XamarinTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 속성을 정의 합니다. @No__t_0 메서드는 "샌프란시스코"를 포함 하는 주소가 항목에 포함 된 경우 `Pin` 탭 할 때 "Xamarin"을 레이블로 표시 하는 `XamarinTemplate`을 반환 합니다. 항목에 "샌프란시스코"가 포함 된 주소가 없는 경우 `OnSelectTemplate` 메서드는 `DefaultTemplate` 반환 합니다.

데이터 템플릿 선택기에 대 한 자세한 내용은 [DataTemplateSelector 만들기](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)를 참조 하세요.

## <a name="pin-events"></a>이벤트 고정

@No__t_0 클래스는 두 가지 이벤트를 제공 합니다.

- `MarkerClicked` pin을 클릭 하거나 탭 할 때 발생 합니다.
- `InfoWindowClicked`는 정보 창을 클릭 하거나 탭 할 때 발생 합니다.

이벤트 처리기는 코드의 pin에 연결할 수 있습니다.

```csharp
public class PinEvents: ContentPage
{
    public PinEvents()
    {
        // ...

        pin1.MarkerClicked += OnMarkerClickedAsync;
        pin1.InfoWindowClicked += OnInfoWindowClickedAsync;
    }

    async void OnMarkerClickedAsync(object sender, PinClickedEventArgs e)
    {
        string pinName = ((Pin)sender).Label;
        await DisplayAlert("Pin Clicked", $"{pinName} was clicked.", "Ok");
    }

    async void OnInfoWindowClickedAsync(object sender, PinClickedEventArgs e)
    {
        string pinName = ((Pin)sender).Label;
        await DisplayAlert("Info Window Clicked", $"The info window was clicked for {pinName}.", "Ok");
    }
}
```

## <a name="related-links"></a>관련 링크

- [Maps 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [사용자 지정 렌더러 매핑](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Xamarin. Forms DataTemplateSelector 만들기](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
