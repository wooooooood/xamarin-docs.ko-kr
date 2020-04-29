---
title: Xamarin.ios 지도 다각형, 폴리라인 및 원
description: 이 문서에서는 Xamarin.ios 맵 인스턴스에서 다각형, 폴리라인 및 원을 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: CDAF0B02-1AA8-4AD6-94A7-ABFC18006A2D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2020
ms.openlocfilehash: e1edbc4d7376023c9d3051b0518c8dc7368e63a7
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517319"
---
# <a name="xamarinforms-map-polygons-and-polylines"></a>Xamarin.ios 지도 다각형 및 다중선

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

`Polygon`, `Polyline`및 `Circle` 요소를 사용 하면 지도의 특정 영역을 강조할 수 있습니다. 는 `Polygon` 스트로크 및 채우기 색을 가질 수 있는 완전히 둘러싸인 도형입니다. 는 `Polyline` 영역을 완전히 묶지 않는 선입니다. 는 `Circle` 지도의 원형 영역을 강조 표시 합니다.

[!["](polygons-images/polygon-polyline.png "지도의 다각형 및 다중선")](polygons-images/polygon-polyline-large.png#lightbox "지도의 다각형 및 다중선")
Ios 및 android[![에서 지도 원,](polygons-images/circle.png "지도의 원") ios 및 android의" 지도 다각형 및 다중선의 스크린샷 "](polygons-images/circle-large.png#lightbox "지도의 원")

`Polygon`, `Polyline`및 `Circle` 클래스는 다음과 같은 바인딩 가능한 `MapElement` 속성을 노출 하는 클래스에서 파생 됩니다.

- `StrokeColor`는 선 `Color` 색을 결정 하는 개체입니다.
- `StrokeWidth`선 두께 `float` 를 결정 하는 개체입니다.

클래스 `Polygon` 는 다음과 같은 추가 바인딩 가능 속성을 정의 합니다.

- `FillColor`다각형의 `Color` 배경색을 결정 하는 개체입니다.

`Polygon` 또한 및 `Polyline` 클래스는 모두 셰이프의 요소를 지정 `GeoPath` 하는 [`Position`](xref:Xamarin.Forms.Maps.Position) 개체 목록 인 속성을 정의 합니다.

클래스 `Circle` 는 다음과 같은 바인딩 가능한 속성을 정의 합니다.

- `Center`는 위도 [`Position`](xref:Xamarin.Forms.Maps.Position) 및 경도로 원의 중심을 정의 하는 개체입니다.
- `Radius`미터, [`Distance`](xref:Xamarin.Forms.Maps.Distance) 킬로미터 또는 마일 단위의 원 반지름을 정의 하는 개체입니다.
- `FillColor`는 원 `Color` 둘레에 있는 색을 결정 하는 속성입니다.

> [!NOTE]
> `StrokeColor` 속성이 지정 되지 않은 경우 스트로크는 기본적으로 검은색으로 지정 됩니다. `FillColor` 속성을 지정 하지 않으면 채우기가 기본적으로 투명 합니다. 따라서 두 속성을 모두 지정 하지 않으면 도형은 채우기가 없는 검정 윤곽선을 갖게 됩니다.

## <a name="create-a-polygon"></a>다각형 만들기

개체 `Polygon` 를 인스턴스화하고 map의 `MapElements` 컬렉션에 추가 하 여 맵에 추가할 수 있습니다. 이렇게 하면 다음과 같이 XAML로 수행할 수 있습니다.

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
     <maps:Map>
         <maps:Map.MapElements>
             <maps:Polygon StrokeColor="#FF9900"
                           StrokeWidth="8"
                           FillColor="#88FF9900">
                 <maps:Polygon.Geopath>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>47.6368678</x:Double>
                             <x:Double>-122.137305</x:Double>
                         </x:Arguments>
                     </maps:Position>
                     ...
                 </maps:Polygon.Geopath>
             </maps:Polygon>
         </maps:Map.MapElements>
     </maps:Map>
</ContentPage>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map
{
  // ...
};

// instantiate a polygon
Polygon polygon = new Polygon
{
    StrokeWidth = 8,
    StrokeColor = Color.FromHex("#1BA1E2"),
    FillColor = Color.FromHex("#881BA1E2"),
    Geopath =
    {
        new Position(47.6368678, -122.137305),
        new Position(47.6368894, -122.134655),
        new Position(47.6359424, -122.134655),
        new Position(47.6359496, -122.1325521),
        new Position(47.6424124, -122.1325199),
        new Position(47.642463,  -122.1338932),
        new Position(47.6406414, -122.1344833),
        new Position(47.6384943, -122.1361248),
        new Position(47.6372943, -122.1376912)
    }
};

// add the polygon to the map's MapElements collection
map.MapElements.Add(polygon);
```

`StrokeColor` 및 `StrokeWidth` 속성을 지정 하 여 다각형의 윤곽선을 사용자 지정 합니다. `FillColor` 속성 값은 `StrokeColor` 속성 값과 일치 하지만 투명 하 게 만들기 위해 지정 된 알파 값이 있으므로 셰이프를 통해 기본 지도를 볼 수 있습니다. 속성 `GeoPath` 에는 다각형 점의 지리적 `Position` 좌표를 정의 하는 개체의 목록이 포함 되어 있습니다. 개체 `Polygon` 는의 `MapElements` 컬렉션에 추가 된 경우 맵에 렌더링 됩니다 `Map`.

> [!NOTE]
> 는 `Polygon` 완전히 둘러싸인 도형입니다. 첫 번째 및 마지막 점이 일치 하지 않으면 자동으로 연결 됩니다.

## <a name="create-a-polyline"></a>다중선 만들기

개체 `Polyline` 를 인스턴스화하고 map의 `MapElements` 컬렉션에 추가 하 여 맵에 추가할 수 있습니다. 이렇게 하면 다음과 같이 XAML로 수행할 수 있습니다.

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
     <maps:Map>
         <maps:Map.MapElements>
             <maps:Polyline StrokeColor="Blue"
                            StrokeWidth="12">
                 <maps:Polyline.Geopath>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>47.6381401</x:Double>
                             <x:Double>-122.1317367</x:Double>
                         </x:Arguments>
                     </maps:Position>
                     ...
                 </maps:Polyline.Geopath>
             </maps:Polyline>
         </maps:Map.MapElements>
     </maps:Map>
</ContentPage>
```

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map
{
  // ...
};
// instantiate a polyline
Polyline polyline = new Polyline
{
    StrokeColor = Color.Blue,
    StrokeWidth = 12,
    Geopath =
    {
        new Position(47.6381401, -122.1317367),
        new Position(47.6381473, -122.1350841),
        new Position(47.6382847, -122.1353094),
        new Position(47.6384582, -122.1354703),
        new Position(47.6401136, -122.1360819),
        new Position(47.6403883, -122.1364681),
        new Position(47.6407426, -122.1377019),
        new Position(47.6412558, -122.1404056),
        new Position(47.6414148, -122.1418647),
        new Position(47.6414654, -122.1432702)
    }
};

// add the polyline to the map's MapElements collection
map.MapElements.Add(polyline);
```

`StrokeColor` 및 `StrokeWidth` 속성을 지정 하 여 줄을 사용자 지정 합니다. 속성 `GeoPath` 에는 다중선 점의 지리적 `Position` 좌표를 정의 하는 개체의 목록이 포함 되어 있습니다. 개체 `Polyline` 는의 `MapElements` 컬렉션에 추가 된 경우 맵에 렌더링 됩니다 `Map`.

## <a name="create-a-circle"></a>원 만들기

개체 `Circle` 를 인스턴스화하고 map의 `MapElements` 컬렉션에 추가 하 여 맵에 추가할 수 있습니다. 이렇게 하면 다음과 같이 XAML로 수행할 수 있습니다.

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
      <maps:Map>
          <maps:Map.MapElements>
              <maps:Circle StrokeColor="#88FF0000"
                           StrokeWidth="8"
                           FillColor="#88FFC0CB">
                  <maps:Circle.Center>
                      <maps:Position>
                          <x:Arguments>
                              <x:Double>37.79752</x:Double>
                              <x:Double>-122.40183</x:Double>
                          </x:Arguments>
                      </maps:Position>
                  </maps:Circle.Center>
                  <maps:Circle.Radius>
                      <maps:Distance>
                          <x:Arguments>
                              <x:Double>250</x:Double>
                          </x:Arguments>
                      </maps:Distance>
                  </maps:Circle.Radius>
              </maps:Circle>             
          </maps:Map.MapElements>
          ...
      </maps:Map>
</ContentPage>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map();

// Instantiate a Circle
Circle circle = new Circle
{
    Center = new Position(37.79752, -122.40183);,
    Radius = new Distance(250),
    StrokeColor = Color.FromHex("#88FF0000"),
    StrokeWidth = 8,
    FillColor = Color.FromHex("#88FFC0CB")
};

// Add the Circle to the map's MapElements collection
map.MapElements.Add(circle);
```

맵의 위치 `Circle` 는 `Center` 및 `Radius` 속성의 값에 따라 결정 됩니다. `Center` 속성은 원의 중심 (위도 및 경도)을 정의 하는 반면 속성은 `Radius` 원의 반지름을 미터 단위로 정의 합니다. `StrokeColor` 및 `StrokeWidth` 속성을 지정 하 여 원의 윤곽선을 사용자 지정 합니다. 속성 `FillColor` 값은 원 둘레에 있는 색을 지정 합니다. 두 색 값 모두 알파 채널을 지정 하 여 기본 맵이 원을 통해 표시 될 수 있도록 합니다. 개체 `Circle` 는의 `MapElements` 컬렉션에 추가 된 경우 맵에 렌더링 됩니다 `Map`.

> [!NOTE]
> `GeographyUtils` 클래스에는 및 `ToCircumferencePositions` `Radius` 속성 값을 정의 `Position` `Center` 하 `Circle` 는 개체를 원 둘레의 위도 및 경도 좌표를 구성 하는 개체의 목록으로 변환 하는 확장 메서드가 있습니다.

## <a name="related-links"></a>관련 링크

- [Maps 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
