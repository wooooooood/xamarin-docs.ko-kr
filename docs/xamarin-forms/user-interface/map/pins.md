---
title: Xamarin.Forms지도 핀
description: 이 문서에서는 맵에 핀을 만드는 방법을 설명 Xamarin.Forms 합니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5e22888291a430863b8e45ee21d359a5acec750f
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138439"
---
# <a name="xamarinforms-map-pins"></a>Xamarin.Forms지도 핀

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

컨트롤을 사용 하면 Xamarin.Forms [`Map`](xref:Xamarin.Forms.Maps.Map) 위치가 개체로 표시 될 수 있습니다 [`Pin`](xref:Xamarin.Forms.Maps.Pin) . 는 `Pin` 탭 할 때 정보 창을 여는 지도 표식입니다.

[![IOS 및 Android에서 지도 핀 및 해당 정보 창의 스크린샷](pins-images/pin-and-information-window.png "정보 창을 사용 하 여 pin 매핑")](pins-images/pin-and-information-window-large.png#lightbox "정보 창을 사용 하 여 pin 매핑")

개체를 [`Pin`](xref:Xamarin.Forms.Maps.Pin) 컬렉션에 추가 하면 해당 [`Map.Pins`](xref:Xamarin.Forms.Maps.Pin) 핀이 맵에 렌더링 됩니다.

클래스에는 [`Pin`](xref:Xamarin.Forms.Maps.Pin) 다음과 같은 속성이 있습니다.

- [`Address`](xref:Xamarin.Forms.Maps.Pin.Address)`string`-일반적으로 pin 위치의 주소를 나타내는 형식입니다. 그러나 주소가 아니라 모든 콘텐츠가 될 수 있습니다 `string` .
- [`Label`](xref:Xamarin.Forms.Maps.Pin.Label)`string`-일반적으로 pin 제목을 나타내는 형식의입니다.
- [`Position`](xref:Xamarin.Forms.Maps.Pin.Position)[`Position`](xref:Xamarin.Forms.Maps.Position)-핀의 위도 및 경도를 나타내는 형식의입니다.
- [`Type`](xref:Xamarin.Forms.Maps.Pin.Type)는 [`PinType`](xref:Xamarin.Forms.Maps.PinType) pin의 유형을 나타내는 형식입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉,이 `Pin` 데이터 바인딩의 대상이 될 수 있습니다. 데이터 바인딩 개체에 대 한 자세한 내용은 `Pin` [Pin 컬렉션 표시](#display-a-pin-collection)를 참조 하세요.

또한 [`Pin`](xref:Xamarin.Forms.Maps.Pin) 클래스는 `MarkerClicked` 및 이벤트를 정의 `InfoWindowClicked` 합니다. 이 `MarkerClicked` 이벤트는 pin을 누를 때 발생 하며 `InfoWindowClicked` 정보 창이 탭 될 때 이벤트가 발생 합니다. `PinClickedEventArgs`두 이벤트를 함께 제공 하는 개체에는 `HideInfoWindow` 형식의 단일 속성이 있습니다 `bool` .

## <a name="display-a-pin"></a>Pin 표시

는 [`Pin`](xref:Xamarin.Forms.Maps.Pin) XAML의에 추가할 수 있습니다 [`Map`](xref:Xamarin.Forms.Maps.Map) .

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
     <maps:Map x:Name="map"
               IsShowingUser="True"
               MoveToLastRegionOnLayoutChange="False">
         <x:Arguments>
             <maps:MapSpan>
                 <x:Arguments>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>36.9628066</x:Double>
                             <x:Double>-122.0194722</x:Double>
                         </x:Arguments>
                     </maps:Position>
                     <x:Double>0.01</x:Double>
                     <x:Double>0.01</x:Double>
                 </x:Arguments>
             </maps:MapSpan>
         </x:Arguments>
         <maps:Map.Pins>
             <maps:Pin Label="Santa Cruz"
                       Address="The city with a boardwalk"
                       Type="Place">
                 <maps:Pin.Position>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>36.9628066</x:Double>
                             <x:Double>-122.0194722</x:Double>
                         </x:Arguments>
                     </maps:Position>
                 </maps:Pin.Position>
             </maps:Pin>
         </maps:Map.Pins>
     </maps:Map>
</ContentPage>
```

이 XAML은 [`Map`](xref:Xamarin.Forms.Maps.Map) 개체에 의해 지정 된 영역을 표시 하는 개체를 만듭니다 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) . `MapSpan`개체는 개체에 의해 표시 되는 위도 및 경도를 중심으로 하며 [`Position`](xref:Xamarin.Forms.Maps.Position) , 0.01 위도 및 경도도를 확장 합니다. [`Pin`](xref:Xamarin.Forms.Maps.Pin)개체를 컬렉션에 추가 하 [`Map.Pins`](xref:Xamarin.Forms.Maps.Pin) 고 해당 `Map` 속성으로 지정 된 위치에서에 그립니다 [`Position`](xref:Xamarin.Forms.Maps.Pin.Position) . 구조체에 대 한 자세한 내용은 [`Position`](xref:Xamarin.Forms.Maps.Position) [지도 위치 및 거리](position-distance.md)를 참조 하세요. 기본 생성자가 없는 개체로 XAML의 인수를 전달 하는 방법에 대 한 자세한 내용은 [xaml로 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)을 참조 하세요.

해당하는 C# 코드는 다음과 같습니다.

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map
{
  // ...
};
Pin pin = new Pin
{
  Label = "Santa Cruz",
  Address = "The city with a boardwalk",
  Type = PinType.Place,
  Position = new Position(36.9628066, -122.0194722)
};
map.Pins.Add(pin);
```

> [!WARNING]
> 속성을 설정 하지 않으면 [`Pin.Label`](xref:Xamarin.Forms.Maps.Pin.Label) `ArgumentException` 이에 추가 될 때이 throw 됩니다 [`Pin`](xref:Xamarin.Forms.Maps.Pin) [`Map`](xref:Xamarin.Forms.Maps.Map) .

이 예제 코드는 맵에 렌더링 되는 단일 pin을 생성 합니다.

[![IOS 및 Android에서 맵 핀의 스크린샷](pins-images/pin-only.png "지도 핀")](pins-images/pin-only-large.png#lightbox "지도 핀")

## <a name="interact-with-a-pin"></a>Pin과 상호 작용

기본적으로를 탭 하면 [`Pin`](xref:Xamarin.Forms.Maps.Pin) 정보 창이 표시 됩니다.

[![IOS 및 Android에서 지도 핀 및 해당 정보 창의 스크린샷](pins-images/pin-and-information-window.png "정보 창을 사용 하 여 pin 매핑")](pins-images/pin-and-information-window-large.png#lightbox "정보 창을 사용 하 여 pin 매핑")

지도에서 다른 곳을 누르면 정보 창이 닫힙니다.

[`Pin`](xref:Xamarin.Forms.Maps.Pin)클래스는 `MarkerClicked` 를 누를 때 발생 하는 이벤트를 정의 합니다 `Pin` . 이 이벤트를 처리 하 여 정보 창을 표시할 필요는 없습니다. 대신이 이벤트는 특정 pin을 탭 했음을 알리는 요구 사항이 있을 때 처리 되어야 합니다.

[`Pin`](xref:Xamarin.Forms.Maps.Pin)또한 클래스는 `InfoWindowClicked` 정보 창을 누를 때 발생 하는 이벤트를 정의 합니다. 이 이벤트는 특정 정보 창이 탭 되었다는 알림이 표시 되어야 하는 경우 처리 되어야 합니다.

다음 코드에서는 이러한 이벤트를 처리 하는 예를 보여 줍니다.

```csharp
using Xamarin.Forms.Maps;
// ...
Pin boardwalkPin = new Pin
{
    Position = new Position(36.9641949, -122.0177232),
    Label = "Boardwalk",
    Address = "Santa Cruz",
    Type = PinType.Place
};
boardwalkPin.MarkerClicked += async (s, args) =>
{
    args.HideInfoWindow = true;
    string pinName = ((Pin)s).Label;
    await DisplayAlert("Pin Clicked", $"{pinName} was clicked.", "Ok");
};

Pin wharfPin = new Pin
{
    Position = new Position(36.9571571, -122.0173544),
    Label = "Wharf",
    Address = "Santa Cruz",
    Type = PinType.Place
};
wharfPin.InfoWindowClicked += async (s, args) =>
{
    string pinName = ((Pin)s).Label;
    await DisplayAlert("Info Window Clicked", $"The info window was clicked for {pinName}.", "Ok");
};
```

`PinClickedEventArgs`두 이벤트를 함께 제공 하는 개체에는 `HideInfoWindow` 형식의 단일 속성이 있습니다 `bool` . 이 속성을 이벤트 처리기 내에서로 설정 하면 `true` 정보 창이 숨겨집니다.

## <a name="pin-types"></a>Pin 유형

[`Pin`](xref:Xamarin.Forms.Maps.Pin)개체에는 [`Type`](xref:Xamarin.Forms.Maps.Pin.Type) [`PinType`](xref:Xamarin.Forms.Maps.PinType) pin의 유형을 나타내는 형식의 속성이 포함 됩니다. `PinType` 열거형은 다음 멤버를 정의합니다.

- `Generic`는 일반 pin을 나타냅니다.
- `Place`는 특정 위치의 pin을 나타냅니다.
- `SavedPin`는 저장 된 위치에 대 한 pin을 나타냅니다.
- `SearchResult`는 검색 결과에 대 한 pin을 나타냅니다.

그러나 [`Pin.Type`](xref:Xamarin.Forms.Maps.Pin.Type) 속성을 [`PinType`](xref:Xamarin.Forms.Maps.PinType) 멤버로 설정 해도 렌더링 된 pin의 모양은 변경 되지 않습니다. 대신 pin 모양을 사용자 지정 하는 사용자 지정 렌더러를 만들어야 합니다. 자세한 내용은 [지도 Pin 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)을 참조 하세요.

## <a name="display-a-pin-collection"></a>고정 컬렉션 표시

[`Map`](xref:Xamarin.Forms.Maps.Map)클래스는 다음 속성을 정의 합니다.

- [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)표시 되는 `IEnumerable` 항목의 컬렉션을 지정 하는 형식의입니다 `IEnumerable` .
- [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 표시 된 항목 컬렉션의 각 항목에 적용할를 지정 하는 형식의입니다.
- `ItemTemplateSelector`[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 런타임에 항목에 대 한를 선택 하는 데 사용 되는를 지정 하는 형식의입니다.

> [!IMPORTANT]
> [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)및 속성이 모두 설정 된 경우 속성이 우선적으로 적용 `ItemTemplate` `ItemTemplateSelector` 됩니다.

는 [`Map`](xref:Xamarin.Forms.Maps.Map) 데이터 바인딩을 사용 하 여 해당 속성을 컬렉션에 바인딩하는 핀으로 채워질 수 있습니다 [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource) `IEnumerable` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="WorkingWithMaps.PinItemsSourcePage">
    <Grid>
        ...
        <maps:Map x:Name="map"
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

[`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)속성 데이터는 `Locations` `ObservableCollection` `Location` 사용자 지정 형식인 개체의을 반환 하는 연결 된 viewmodel의 속성에 바인딩됩니다. 각 `Location` 개체는 형식의 `Address` 및 속성과 형식의 속성을 정의 `Description` `string` `Position` [`Position`](xref:Xamarin.Forms.Maps.Position) 합니다.

컬렉션에 있는 각 항목의 모양은 `IEnumerable` [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 해당 속성 [`Pin`](xref:Xamarin.Forms.Maps.Pin) 에 데이터를 바인딩하는 개체를 포함 하는로 설정 하 여 정의 됩니다.

다음 스크린샷에는 [`Map`](xref:Xamarin.Forms.Maps.Map) [`Pin`](xref:Xamarin.Forms.Maps.Pin) 데이터 바인딩을 사용 하 여 컬렉션을 표시 하는 방법을 보여 줍니다.

[![IOS 및 Android에서 데이터 바인딩된 핀이 있는 지도의 스크린샷](pins-images/pins-itemsource.png "데이터 바인딩된 pin을 사용 하는 맵")](pins-images/pins-itemsource-large.png#lightbox "데이터 바인딩된 pin을 사용 하는 맵")

### <a name="choose-item-appearance-at-runtime"></a>런타임에 항목 모양 선택

`IEnumerable`속성을로 설정 하 여 항목 값을 기준으로 런타임에 컬렉션에 있는 각 항목의 모양을 선택할 수 있습니다 `ItemTemplateSelector` [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) .

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
                    <!-- Change the property values, or the properties that are bound to. -->
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

다음 예제에서는 클래스를 보여 줍니다 `MapItemTemplateSelector` .

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

`MapItemTemplateSelector`클래스는 `DefaultTemplate` `XamarinTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 다른 데이터 템플릿으로 설정 된 및 속성을 정의 합니다. `OnSelectTemplate`이 메서드는 `XamarinTemplate` `Pin` 항목에 "샌프란시스코"가 포함 된 주소가 있는 경우 "Xamarin"을 탭으로 표시 하는을 반환 합니다. 항목에 "샌프란시스코"가 포함 된 주소가 없는 경우 `OnSelectTemplate` 메서드는을 반환 합니다 `DefaultTemplate` .

> [!NOTE]
> 이 기능에 대 한 사용 사례는 하위 분류 개체의 속성을 [`Pin`](xref:Xamarin.Forms.Maps.Pin) 하위 형식에 따라 다른 속성에 바인딩하는 것입니다 `Pin` .

데이터 템플릿 선택기에 대 한 자세한 내용은 [ Xamarin.Forms DataTemplateSelector 만들기](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [Maps 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [사용자 지정 렌더러 매핑](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)
- [XAML에서 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)
- [DataTemplateSelector 만들기 Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
