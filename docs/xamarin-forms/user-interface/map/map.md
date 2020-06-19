---
title: Xamarin.Forms맵 컨트롤
description: 지도 컨트롤은 지도를 표시 하 고 주석을 추가 하는 플랫폼 간 뷰입니다. 각 플랫폼에 대해 네이티브 맵 컨트롤을 사용 하 여 사용자에 게 빠르고 친숙 한 지도 환경을 제공 합니다.
ms.prod: xamarin
ms.assetid: 22C99029-0B16-43A6-BF58-26B48C4AED38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1aee81b6988e1f3a7099c2722b6f336f071ad8c0
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84946366"
---
# <a name="xamarinforms-map-control"></a>Xamarin.Forms맵 컨트롤

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[`Map`](xref:Xamarin.Forms.Maps.Map)컨트롤은 지도를 표시 하 고 주석을 추가 하는 플랫폼 간 뷰입니다. 각 플랫폼에 대해 네이티브 맵 컨트롤을 사용 하 여 사용자에 게 빠르고 친숙 한 지도 환경을 제공 합니다.

[![IOS 및 Android에서 지도 컨트롤의 스크린샷](map-images/map-default.png "지도 컨트롤")](map-images/map-default-large.png#lightbox "지도 컨트롤")

[`Map`](xref:Xamarin.Forms.Maps.Map)클래스는 지도 모양 및 동작을 제어 하는 다음 속성을 정의 합니다.

- [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser)형식의는 `bool` map에서 사용자의 현재 위치를 표시 하는지 여부를 나타냅니다.
- [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)표시 되는 `IEnumerable` 항목의 컬렉션을 지정 하는 형식의입니다 `IEnumerable` .
- [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 표시 된 항목 컬렉션의 각 항목에 적용할를 지정 하는 형식의입니다.
- `ItemTemplateSelector`[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 런타임에 항목에 대 한를 선택 하는 데 사용 되는를 지정 하는 형식의입니다.
- [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled)형식의는 `bool` 맵을 스크롤할 수 있는지 여부를 결정 합니다.
- [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled)형식의은 `bool` 지도를 확대/축소할 수 있는지 여부를 결정 합니다.
- `MapElements`형식의는 `IList<MapElement>` 다각형 및 다중선과 같은 지도의 요소 목록을 나타냅니다.
- [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)형식의은 [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) 지도의 표시 스타일을 나타냅니다.
- `MoveToLastRegionOnLayoutChange`형식의는 `bool` 레이아웃 변경이 발생할 때 표시 되는 맵 영역을 현재 영역에서 이전 집합 영역으로 이동할지 여부를 제어 합니다.
- [`Pins`](xref:Xamarin.Forms.Maps.Map.Pins)형식의는 `IList<Pin>` 맵의 핀 목록을 나타냅니다.
- `TrafficEnabled`형식의는 `bool` 트래픽 데이터가 맵에 중첩 되는지 여부를 나타냅니다.
- [`VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion)형식의는 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) 맵의 현재 표시 된 영역을 반환 합니다.

, 및 속성을 제외 하 고 이러한 `MapElements` 속성 `Pins` `VisibleRegion` 은 개체에 의해 지원 되며이 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 는 데이터 바인딩의 대상이 될 수 있음을 의미 합니다.

[`Map`](xref:Xamarin.Forms.Maps.Map)또한 클래스는 `MapClicked` 맵을 누를 때 발생 하는 이벤트를 정의 합니다. `MapClickedEventArgs`이벤트와 함께 제공 되는 개체에는 형식의 이라는 단일 속성이 있습니다 `Position` [`Position`](xref:Xamarin.Forms.Maps.Position) . 이벤트가 발생 하면 속성은 탭 된 `Position` 지도 위치로 설정 됩니다. 구조체에 대 한 자세한 내용은 [`Position`](xref:Xamarin.Forms.Maps.Position) [지도 위치 및 거리](position-distance.md)를 참조 하세요.

, 및 속성에 대 한 자세한 내용은 [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource) [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate) `ItemTemplateSelector` [pin 컬렉션 표시](pins.md#display-a-pin-collection)를 참조 하세요.

## <a name="display-a-map"></a>지도 표시

는 [`Map`](xref:Xamarin.Forms.Maps.Map) 레이아웃 또는 페이지에 추가 하 여 표시할 수 있습니다.

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
    <maps:Map x:Name="map" />
</ContentPage>
```

> [!NOTE]
> 추가 `xmlns` 네임 스페이스 정의는를 참조 하는 데 필요 Xamarin.Forms 합니다. 컨트롤을 매핑합니다. 이전 예제에서 `Xamarin.Forms.Maps` 네임 스페이스는 키워드를 통해 참조 됩니다 `maps` .

해당하는 C# 코드는 다음과 같습니다.

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Maps;

namespace WorkingWithMaps
{
    public class MapTypesPageCode : ContentPage
    {
        public MapTypesPageCode()
        {
            Map map = new Map();
            Content = map;
        }
    }
}
```

이 예제에서는 [`Map`](xref:Xamarin.Forms.Maps.Map) 로마에서 지도의 가운데 맞춤을 설정 하는 기본 생성자를 호출 합니다.

[![IOS 및 Android의 기본 위치를 사용 하는 지도 컨트롤의 스크린샷](map-images/map-default.png "기본 위치를 사용 하 여 컨트롤 매핑")](map-images/map-default-large.png#lightbox "기본 위치를 사용 하 여 컨트롤 매핑")

또는 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) [`Map`](xref:Xamarin.Forms.Maps.Map) 로드 될 때 맵의 중심점 및 확대/축소 수준을 설정 하기 위해 인수를 생성자에 전달할 수 있습니다. 자세한 내용은 [지도의 특정 위치 표시](#display-a-specific-location-on-a-map)를 참조 하세요.

## <a name="map-types"></a>지도 유형

[`Map.MapType`](xref:Xamarin.Forms.Maps.Map.MapType)속성을 [`MapType`](xref:Xamarin.Forms.Maps.MapType) 열거형 멤버로 설정 하 여 지도의 표시 스타일을 정의할 수 있습니다. `MapType` 열거형은 다음 멤버를 정의합니다.

- `Street`동 지도의 표시를 지정 합니다.
- `Satellite`위성 이미지를 포함 하는 맵이 표시 되도록 지정 합니다.
- `Hybrid`거리와 위성 데이터를 결합 하는 지도를 표시 하도록 지정 합니다.

속성이 정의 되지 않은 경우 기본적으로에는 [`Map`](xref:Xamarin.Forms.Maps.Map) 번 지도가 표시 됩니다 [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) . 또는 속성을 `MapType` 열거형 멤버 중 하나로 설정할 수 있습니다 [`MapType`](xref:Xamarin.Forms.Maps.MapType) .

```xaml
<maps:Map MapType="Satellite" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
Map map = new Map
{
    MapType = MapType.Satellite
};
```

다음 스크린샷에는 [`Map`](xref:Xamarin.Forms.Maps.Map) [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) 속성이로 설정 된 경우이 표시 됩니다 `Street` .

[![IOS 및 Android에서 거리 맵 유형이 있는 지도 컨트롤의 스크린샷](map-images/maptype-street.png "거리 maptype을 사용 하 여 컨트롤 매핑")](map-images/maptype-street-large.png#lightbox "거리 맵 종류를 사용 하 여 컨트롤 매핑")

다음 스크린샷에는 [`Map`](xref:Xamarin.Forms.Maps.Map) [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) 속성이로 설정 된 경우이 표시 됩니다 `Satellite` .

[![IOS 및 Android에서 위성 지도 유형을 사용 하는 지도 컨트롤의 스크린샷](map-images/maptype-satellite.png "위성 maptype을 사용 하 여 컨트롤 매핑")](map-images/maptype-satellite-large.png#lightbox "위성 지도 형식으로 컨트롤 매핑")

다음 스크린샷에는 [`Map`](xref:Xamarin.Forms.Maps.Map) [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) 속성이로 설정 된 경우이 표시 됩니다 `Hybrid` .

[![IOS 및 Android에서 하이브리드 지도 유형을 사용한 지도 컨트롤의 스크린샷](map-images/maptype-hybrid.png "하이브리드 maptype을 사용 하 여 컨트롤 매핑")](map-images/maptype-hybrid-large.png#lightbox "하이브리드 지도 종류를 사용 하 여 컨트롤 매핑")

## <a name="display-a-specific-location-on-a-map"></a>지도에 특정 위치 표시

맵을 로드할 때 표시할 지도의 지역은 생성자에 인수를 전달 하 여 설정할 수 있습니다 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) [`Map`](xref:Xamarin.Forms.Maps.Map) .

```xaml
<maps:Map>
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
</maps:Map>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
Position position = new Position(36.9628066, -122.0194722);
MapSpan mapSpan = new MapSpan(position, 0.01, 0.01);
Map map = new Map(mapSpan);
```

이 예제에서는 [`Map`](xref:Xamarin.Forms.Maps.Map) 개체에 의해 지정 된 영역을 표시 하는 개체를 만듭니다 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) . 개체는 `MapSpan` 개체로 표시 되는 위도 및 경도를 중심으로 [`Position`](xref:Xamarin.Forms.Maps.Position) 하며 0.01 위도 및 0.01 경도도에 걸쳐 있습니다. 구조체에 대 한 자세한 내용은 [`Position`](xref:Xamarin.Forms.Maps.Position) [지도 위치 및 거리](position-distance.md)를 참조 하세요. XAML로 인수를 전달 하는 방법에 대 한 자세한 내용은 [xaml로 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)을 참조 하세요.

그 결과 지도를 표시 하면 특정 위치를 중심으로 하 고 특정 개수의 위도 및 경도 각도로 범위가 지정 됩니다.

[![IOS 및 Android에서 지정 된 위치를 포함 하는 지도 컨트롤의 스크린샷](map-images/map-region.png "지정 된 위치를 사용 하 여 컨트롤 매핑")](map-images/map-region-large.png#lightbox "지정 된 위치를 사용 하 여 컨트롤 매핑")

## <a name="create-a-mapspan-object"></a>MapSpan 개체 만들기

개체를 만드는 방법에는 여러 가지가 있습니다 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) . 일반적인 접근 방식은 생성자에 필요한 인수를 제공 하는 것입니다 `MapSpan` . 이러한 값은 개체로 표시 되는 위도 및 경도 [`Position`](xref:Xamarin.Forms.Maps.Position) 이며에 의해 확장 되는 `double` 위도 및 경도의 각도를 나타내는 값입니다 `MapSpan` . 구조체에 대 한 자세한 내용은 [`Position`](xref:Xamarin.Forms.Maps.Position) [지도 위치 및 거리](position-distance.md)를 참조 하세요.

또는 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) 클래스에 새 개체를 반환 하는 세 가지 메서드가 있습니다 `MapSpan` .

1. [`ClampLatitude`](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude*)`MapSpan` `LongitudeDegrees` 메서드의 클래스 인스턴스와 동일한 `north` 및 및 인수에 의해 정의 된 반지름을 사용 하 여를 반환 합니다 `south` .
1. [`FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius*)`MapSpan`해당 및 인수로 정의 된을 반환 [`Position`](xref:Xamarin.Forms.Maps.Position) [`Distance`](xref:Xamarin.Forms.Maps.Distance) 합니다.
1. [`WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom*)`MapSpan`메서드의 클래스 인스턴스와 동일한 중심을 가진를 반환 하지만, 반지름을 `double` 인수로 곱합니다.

구조체에 대 한 자세한 내용은 [`Distance`](xref:Xamarin.Forms.Maps.Distance) [지도 위치 및 거리](position-distance.md)를 참조 하세요.

을 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) 만든 후에는 다음 속성에 액세스 하 여 해당 데이터에 대 한 데이터를 검색할 수 있습니다.

- [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center)는 [`Position`](xref:Xamarin.Forms.Maps.Position) 의 지리적 위치에 있는를 나타냅니다 `MapSpan` .
- [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees)는의 범위에 해당 하는 위도의 각도를 나타냅니다 `MapSpan` .
- [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees)는의 범위에 해당 하는 경도의 각도를 나타냅니다 `MapSpan` .
- [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius)- `MapSpan` 반경을 나타냅니다.

## <a name="move-the-map"></a>지도 이동

[`Map.MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*)메서드를 호출 하 여 지도의 위치 및 확대/축소 수준을 변경할 수 있습니다. 이 메서드는 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) 표시할 지도의 영역 및 확대/축소 수준을 정의 하는 인수를 허용 합니다.

다음 코드에서는 지도에서 표시 된 영역을 이동 하는 예를 보여 줍니다.

```csharp
MapSpan mapSpan = MapSpan.FromCenterAndRadius(position, Distance.FromKilometers(0.444));
map.MoveToRegion(mapSpan);
```

## <a name="zoom-the-map"></a>맵 확대/축소

의 확대/축소 수준은 [`Map`](xref:Xamarin.Forms.Maps.Map) 해당 위치를 변경 하지 않고 변경할 수 있습니다. 이 작업은 맵 UI를 사용 하 여 수행 하거나 [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) 현재 위치를 인수로 사용 하는 인수를 사용 하 여 메서드를 프로그래밍 방식으로 호출할 수 있습니다 [`Position`](xref:Xamarin.Forms.Maps.Position) .

```csharp
double zoomLevel = 0.5;
double latlongDegrees = 360 / (Math.Pow(2, zoomLevel));
if (map.VisibleRegion != null)
{
    map.MoveToRegion(new MapSpan(map.VisibleRegion.Center, latlongDegrees, latlongDegrees));
}
```

이 예제에서는 [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) 속성을 통해 지도의 현재 위치를 지정 하는 인수를 사용 하 여 메서드를 호출 [`Map.VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion) 하 고 확대/축소 수준을 위도 및 경도 각도로 호출 합니다. 전체적인 결과로 지도의 확대/축소 수준이 변경 되지만 해당 위치는 변경 되지 않습니다. 지도에서 확대/축소를 구현 하는 다른 방법은 메서드를 사용 하 여 [`MapSpan.WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom*) 확대/축소 비율을 제어 하는 것입니다.

> [!IMPORTANT]
> 지도 UI를 통해 또는 프로그래밍 방식으로 지도를 확대/축소 하려면 [`Map.HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) 속성이 이어야 합니다 `true` . 이 속성에 대 한 자세한 내용은 [Zoom 사용 안 함](#disable-zoom)을 참조 하세요.

## <a name="customize-map-behavior"></a>맵 동작 사용자 지정

의 동작은 [`Map`](xref:Xamarin.Forms.Maps.Map) 해당 속성 중 일부를 설정 하 고 이벤트를 처리 하 여 사용자 지정할 수 있습니다 `MapClicked` .

> [!NOTE]
> 지도 사용자 지정 렌더러를 만들어 추가 맵 동작 사용자 지정을 수행할 수 있습니다. 자세한 내용은 [ Xamarin.Forms 맵 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)을 참조 하세요.

### <a name="show-traffic-data"></a>트래픽 데이터 표시

[`Map`](xref:Xamarin.Forms.Maps.Map)클래스는 `TrafficEnabled` 형식의 속성을 정의 합니다 `bool` . 기본적으로이 속성은 `false` 트래픽 데이터가 맵에 중첩 되지 않음을 나타내는입니다. 이 속성이로 설정 되 면 `true` 트래픽 데이터가 맵에 중첩 됩니다. 다음 예제에서는이 속성을 설정 하는 방법을 보여 줍니다.

```xaml
<maps:Map TrafficEnabled="true" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
Map map = new Map
{
    TrafficEnabled = true
};
```

### <a name="disable-scroll"></a>스크롤 사용 안 함

[`Map`](xref:Xamarin.Forms.Maps.Map)클래스는 [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled) 형식의 속성을 정의 합니다 `bool` . 기본적으로이 속성은 `true` 맵을 스크롤할 수 있음을 나타냅니다. 이 속성이로 설정 되 면 `false` 맵은 스크롤되지 않습니다. 다음 예제에서는이 속성을 설정 하는 방법을 보여 줍니다.

```xaml
<maps:Map HasScrollEnabled="false" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
Map map = new Map
{
    HasScrollEnabled = false
};
```

### <a name="disable-zoom"></a>확대/축소 사용 안 함

[`Map`](xref:Xamarin.Forms.Maps.Map)클래스는 [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) 형식의 속성을 정의 합니다 `bool` . 기본적으로이 속성은 `true` 맵에서 확대/축소를 수행할 수 있음을 나타냅니다. 이 속성이로 설정 되 면 `false` 지도를 확대할 수 없습니다. 다음 예제에서는이 속성을 설정 하는 방법을 보여 줍니다.

```xaml
<maps:Map HasZoomEnabled="false" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
Map map = new Map
{
    HasZoomEnabled = false
};
```

### <a name="show-the-users-location"></a>사용자의 위치 표시

[`Map`](xref:Xamarin.Forms.Maps.Map)클래스는 [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser) 형식의 속성을 정의 합니다 `bool` . 기본적으로이 속성은 `false` 맵에 사용자의 현재 위치가 표시 되지 않음을 나타냅니다. 이 속성이로 설정 되 면 `true` 맵은 사용자의 현재 위치를 표시 합니다. 다음 예제에서는이 속성을 설정 하는 방법을 보여 줍니다.

```xaml
<maps:Map IsShowingUser="true" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
Map map = new Map
{
    IsShowingUser = true
};
```

> [!IMPORTANT]
> IOS, Android 및 유니버설 Windows 플랫폼에서 사용자의 위치에 액세스 하려면 응용 프로그램에 대 한 위치 권한이 있어야 합니다. 자세한 내용은 [플랫폼 구성](setup.md#platform-configuration)을 참조 하세요.

### <a name="maintain-map-region-on-layout-change"></a>레이아웃 변경 시 지도 영역 유지 관리

[`Map`](xref:Xamarin.Forms.Maps.Map)클래스는 `MoveToLastRegionOnLayoutChange` 형식의 속성을 정의 합니다 `bool` . 기본적으로이 속성은입니다 .이 속성은 `true` 표시 된 지도 지역이 장치 회전과 같은 레이아웃 변경이 발생할 때 현재 영역에서 이전 집합 영역으로 이동 함을 나타냅니다. 이 속성을로 설정 하면 `false` 레이아웃 변경이 발생할 때 표시 되는 맵 지역이 가운데에 그대로 유지 됩니다. 다음 예제에서는이 속성을 설정 하는 방법을 보여 줍니다.

```xaml
<maps:Map MoveToLastRegionOnLayoutChange="false" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
Map map = new Map
{
    MoveToLastRegionOnLayoutChange = false
};
```

### <a name="map-clicks"></a>지도 클릭

[`Map`](xref:Xamarin.Forms.Maps.Map)클래스는 `MapClicked` 맵을 누를 때 발생 하는 이벤트를 정의 합니다. `MapClickedEventArgs`이벤트와 함께 제공 되는 개체에는 형식의 이라는 단일 속성이 있습니다 `Position` [`Position`](xref:Xamarin.Forms.Maps.Position) . 이벤트가 발생 하면 속성은 탭 된 `Position` 지도 위치로 설정 됩니다. 구조체에 대 한 자세한 내용은 [`Position`](xref:Xamarin.Forms.Maps.Position) [지도 위치 및 거리](position-distance.md)를 참조 하세요.

다음 코드 예제에서는 이벤트에 대 한 이벤트 처리기를 보여 줍니다 `MapClicked` .

```csharp
void OnMapClicked(object sender, MapClickedEventArgs e)
{
    System.Diagnostics.Debug.WriteLine($"MapClick: {e.Position.Latitude}, {e.Position.Longitude}");
}
```

이 예제에서 `OnMapClicked` 이벤트 처리기는 탭 맵 위치를 나타내는 위도 및 경도를 출력 합니다. 이벤트 처리기는 다음과 같이 이벤트를 사용 하 여 등록할 수 있습니다 `MapClicked` .

```xaml
<maps:Map MapClicked="OnMapClicked" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
Map map = new Map();
map.MapClicked += OnMapClicked;
```

## <a name="related-links"></a>관련 링크

- [Maps 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [지도 위치 및 거리](position-distance.md)
- [지도 사용자 지정 Xamarin.Forms](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)
- [XAML에서 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)
