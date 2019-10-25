---
title: Xamarin.ios 맵 핀
description: 이 문서에서는 Xamarin.ios 맵에 핀을 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: F8FC081B-A811-4FBB-B8F8-30D6FD36BD40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/23/2019
ms.openlocfilehash: a2fb0ba2036dfe34e85c7bebab6ecb55cd868ad5
ms.sourcegitcommit: 5c22097bed2a8d51ecaf6ca197bf4d449dfe1377
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72810526"
---
# <a name="xamarinforms-map-pins"></a>Xamarin.ios 맵 핀

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Xamarin.ios [`Map`](xref:Xamarin.Forms.Maps.Map) 컨트롤을 사용 하면 위치를 [`Pin`](xref:Xamarin.Forms.Maps.Pin) 개체로 표시할 수 있습니다. `Pin`은 탭 할 때 정보 창을 여는 지도 표식입니다.

[![IOS 및 Android에서 지도 핀 및 해당 정보 창의 스크린샷](pins-images/pin-and-information-window.png "정보 창을 사용 하 여 pin 매핑")](pins-images/pin-and-information-window-large.png#lightbox "정보 창을 사용 하 여 pin 매핑")

[`Pin`](xref:Xamarin.Forms.Maps.Pin) 개체가 [`Map.Pins`](xref:Xamarin.Forms.Maps.Pin) 컬렉션에 추가 되 면 핀이 맵에 렌더링 됩니다.

[`Pin`](xref:Xamarin.Forms.Maps.Pin) 클래스에는 다음과 같은 속성이 있습니다.

- [`Address`](xref:Xamarin.Forms.Maps.Pin.Address)-일반적으로 pin 위치의 주소를 나타내는 `string`형식입니다. 그러나 주소가 아니라 모든 `string` 콘텐츠가 될 수 있습니다.
- [`Label`](xref:Xamarin.Forms.Maps.Pin.Label)-일반적으로 pin 제목을 나타내는 `string`형식의입니다.
- 핀의 위도 및 경도를 나타내는 [`Position`](xref:Xamarin.Forms.Maps.Position)형식의 [`Position`](xref:Xamarin.Forms.Maps.Pin.Position)입니다.
- pin의 유형을 나타내는 [`PinType`](xref:Xamarin.Forms.Maps.PinType)형식의 [`Type`](xref:Xamarin.Forms.Maps.Pin.Type)입니다.

이러한 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원 됩니다. 즉, `Pin` 데이터 바인딩의 대상이 될 수 있습니다. 개체 `Pin` 데이터 바인딩에 대 한 자세한 내용은 [pin 컬렉션 표시](#display-a-pin-collection)를 참조 하세요.

또한 [`Pin`](xref:Xamarin.Forms.Maps.Pin) 클래스는 `MarkerClicked` 및 `InfoWindowClicked` 이벤트를 정의 합니다. `MarkerClicked` 이벤트는 pin을 누를 때 발생 하며, `InfoWindowClicked` 이벤트는 정보 창이 탭 될 때 발생 합니다. 두 이벤트를 함께 제공 하는 `PinClickedEventArgs` 개체에는 `bool`형식의 단일 `HideInfoWindow` 속성이 있습니다.

## <a name="display-a-pin"></a>Pin 표시

[`Pin`](xref:Xamarin.Forms.Maps.Pin) 를 XAML의 [`Map`](xref:Xamarin.Forms.Maps.Map) 에 추가할 수 있습니다.

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
     <maps:Map x:Name="map"
               IsShowingUser="True"
               MoveToLastRegionOnLayoutChange="False"
               HeightRequest="100"                  
               WidthRequest="960"
               VerticalOptions="FillAndExpand">
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

이 XAML은 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) 개체에 의해 지정 된 영역을 표시 하는 [`Map`](xref:Xamarin.Forms.Maps.Map) 개체를 만듭니다. `MapSpan` 개체는 0.01 위도 및 경도도를 연장 하는 [`Position`](xref:Xamarin.Forms.Maps.Position) 개체로 표시 되는 위도 및 경도를 중심으로 합니다. [`Pin`](xref:Xamarin.Forms.Maps.Pin) 개체는 [`Map.Pins`](xref:Xamarin.Forms.Maps.Pin) 컬렉션에 추가 되 고 [`Position`](xref:Xamarin.Forms.Maps.Pin.Position) 속성으로 지정 된 위치에 `Map`에 그려집니다. 기본 생성자가 없는 개체로 XAML의 인수를 전달 하는 방법에 대 한 자세한 내용은 [xaml로 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)을 참조 하세요.

> [!NOTE]
> [`Position`](xref:Xamarin.Forms.Maps.Position) 구조체는 `double`형식 모두 읽기 전용 [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude) 및 [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude) 속성을 정의 합니다. 생성자를 통해 `Position` 개체를 만들 때 위도 값은-90.0과 90.0 사이에 고정 경도 값은-180.0과 180.0 사이에 고정 됩니다.

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
> [`Pin.Label`](xref:Xamarin.Forms.Maps.Pin.Label) 속성을 설정 하지 않으면 [`Pin`](xref:Xamarin.Forms.Maps.Pin) [`Map`](xref:Xamarin.Forms.Maps.Map)에 추가 될 때 `ArgumentException` throw 됩니다.

이 예제 코드는 맵에 렌더링 되는 단일 pin을 생성 합니다.

[![IOS 및 Android에서 맵 핀의 스크린샷](pins-images/pin-only.png "지도 핀")](pins-images/pin-only-large.png#lightbox "지도 핀")

## <a name="interact-with-a-pin"></a>Pin과 상호 작용

기본적으로 [`Pin`](xref:Xamarin.Forms.Maps.Pin) 탭 하면 정보 창이 표시 됩니다.

[![IOS 및 Android에서 지도 핀 및 해당 정보 창의 스크린샷](pins-images/pin-and-information-window.png "정보 창을 사용 하 여 pin 매핑")](pins-images/pin-and-information-window-large.png#lightbox "정보 창을 사용 하 여 pin 매핑")

지도에서 다른 곳을 누르면 정보 창이 닫힙니다.

[`Pin`](xref:Xamarin.Forms.Maps.Pin) 클래스는 `Pin` 탭 할 때 발생 하는 `MarkerClicked` 이벤트를 정의 합니다. 이 이벤트를 처리 하 여 정보 창을 표시할 필요는 없습니다. 대신이 이벤트는 특정 pin이 탭 되었다는 알림이 필요한 경우에만 처리 해야 합니다.

또한 [`Pin`](xref:Xamarin.Forms.Maps.Pin) 클래스는 정보 창을 누를 때 발생 하는 `InfoWindowClicked` 이벤트를 정의 합니다. 이 이벤트는 특정 정보 창이 탭 되었다는 알림이 표시 되어야 하는 경우 처리 되어야 합니다.

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

두 이벤트를 함께 제공 하는 `PinClickedEventArgs` 개체에는 `bool`형식의 단일 `HideInfoWindow` 속성이 있습니다. 이벤트 처리기 내에서이 속성을 `true` 설정 하면 정보 창이 숨겨집니다.

## <a name="pin-types"></a>Pin 유형

[`Pin`](xref:Xamarin.Forms.Maps.Pin) 개체에는 pin의 유형을 나타내는 [`PinType`](xref:Xamarin.Forms.Maps.PinType)형식의 [`Type`](xref:Xamarin.Forms.Maps.Pin.Type) 속성이 포함 됩니다. `PinType` 열거형은 다음 멤버를 정의합니다.

- `Generic`일반 pin을 나타냅니다.
- `Place`는 위치의 pin을 나타냅니다.
- `SavedPin`는 저장 된 위치에 대 한 pin을 나타냅니다.
- `SearchResult`는 검색 결과에 대 한 pin을 나타냅니다.

그러나 [`Pin.Type`](xref:Xamarin.Forms.Maps.Pin.Type) 속성을 [`PinType`](xref:Xamarin.Forms.Maps.PinType) 멤버로 설정 하면 렌더링 된 핀의 모양이 변경 되지 않습니다. 대신 pin 모양을 사용자 지정 하는 사용자 지정 렌더러를 만들어야 합니다. 자세한 내용은 [지도 Pin 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)을 참조 하세요.

## <a name="display-a-pin-collection"></a>Pin 컬렉션 표시

[`Map`](xref:Xamarin.Forms.Maps.Map) 클래스는 다음 속성을 정의 합니다.

- 표시 될 `IEnumerable` 항목의 컬렉션을 지정 하는 `IEnumerable`형식의 [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)입니다.
- 표시 된 항목 컬렉션의 각 항목에 적용할 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 를 지정 하는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)형식의 [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)입니다.
- 런타임에 항목에 대 한 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 를 선택 하는 데 사용 되는 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) 를 지정 하는 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)형식의 `ItemTemplateSelector`입니다.

> [!IMPORTANT]
> `ItemTemplate` 및 `ItemTemplateSelector` 속성이 모두 설정 된 경우 [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate) 속성이 우선적으로 적용 됩니다.

데이터 바인딩을 사용 하 여 [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource) 속성을 `IEnumerable` 컬렉션에 바인딩하여 [`Map`](xref:Xamarin.Forms.Maps.Map) 를 pin으로 채울 수 있습니다.

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

[`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource) 속성 데이터는 연결 된 viewmodel의 `Locations` 속성에 바인딩되고,이 속성은 사용자 지정 유형인 `Location` 개체의 `ObservableCollection` 반환 합니다. 각 `Location` 개체는 `string` 형식의 `Address` 및 `Description` 속성과 [`Position`](xref:Xamarin.Forms.Maps.Position)형식의 `Position` 속성을 정의 합니다.

`IEnumerable` 컬렉션에 있는 각 항목의 모양은 데이터를 적절 한 속성에 바인딩하는 [`Pin`](xref:Xamarin.Forms.Maps.Pin) 개체를 포함 하는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate) 속성을 설정 하 여 정의 됩니다.

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

## <a name="related-links"></a>관련 링크

- [Maps 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [사용자 지정 렌더러 매핑](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [XAML로 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)
- [Xamarin. Forms DataTemplateSelector 만들기](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
