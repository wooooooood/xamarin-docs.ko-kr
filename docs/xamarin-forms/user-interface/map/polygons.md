---
title: Xamarin.ios 지도 다각형 및 다중선
description: 이 문서에서는 Xamarin.ios 맵 인스턴스에서 다각형 및 다중선을 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: CDAF0B02-1AA8-4AD6-94A7-ABFC18006A2D
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 09/20/2019
ms.openlocfilehash: 42c00a060e0477aff4ea02f6213fa751b2adf4dd
ms.sourcegitcommit: 5c22097bed2a8d51ecaf6ca197bf4d449dfe1377
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72810486"
---
# <a name="xamarinforms-map-polygons-and-polylines"></a>Xamarin.ios 지도 다각형 및 다중선

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

["iOS 및 Android에서 Polygon 및 다중선 데모"를![합니다.](polygons-images/polygon-app-cropped.png)](polygons-images/polygon-app.png#lightbox)

`Polygon` 및 `Polyline` 요소를 사용 하면 지도의 특정 영역을 강조 표시할 수 있습니다. @No__t_0는 스트로크 및 채우기 색을 가질 수 있는 완전히 둘러싸인 도형입니다. @No__t_0는 영역을 완전히 묶지 않는 줄입니다.

> [!NOTE]
> @No__t_0 및 `Polyline`의 예제는 샘플 프로젝트의 **PolygonsPage** 에 있습니다.

@No__t_0 및 `Polyline` 클래스는 다음 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 속성을 노출 하는 `MapElement`에서 파생 됩니다.

- `StrokeColor`는 선 색을 결정 하는 `Color` 속성입니다.
- `StrokeWidth`는 선 두께를 결정 하는 `float` 속성입니다.
- `Geopath`은 `Polygon`와 `Polyline` 모두에 대해 정의 되며,는 도형의 요소를 지정 하는 [`Position`](xref:Xamarin.Forms.Maps.Position) 개체의 목록입니다.

@No__t_0 클래스는 추가 속성을 정의 합니다.

- `FillColor`은 다각형의 배경색을 결정 하는 `Color` 속성입니다.

> [!NOTE]
> @No__t_0 속성이 지정 되지 않은 경우 스트로크가 기본적으로 black이 됩니다. @No__t_0 속성을 지정 하지 않으면 채우기가 기본적으로 투명 합니다. 따라서 두 속성을 모두 지정 하지 않으면 도형은 채우기가 없는 검정 윤곽선을 갖게 됩니다.

## <a name="create-a-polygon"></a>다각형 만들기

`Polygon` 개체를 인스턴스화하고 맵의 `MapElements` 컬렉션에 추가 하 여 맵에 추가할 수 있습니다. 이렇게 하면 다음과 같이 XAML로 수행할 수 있습니다.

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

다각형의 윤곽선을 사용자 지정 하기 위해 `StrokeColor` 및 `StrokeWidth` 속성이 지정 됩니다. @No__t_0 속성 값은 `StrokeColor` 속성 값과 일치 하지만 투명 하 게 만들기 위해 지정 된 알파 값이 있으므로 셰이프를 통해 기본 지도를 볼 수 있습니다. @No__t_0 속성에는 다각형 지점의 지리적 좌표를 정의 하는 `Position` 개체의 목록이 포함 되어 있습니다. @No__t_0 개체는 `Map`의 `MapElements` 컬렉션에 추가 된 경우 맵에 렌더링 됩니다.

> [!NOTE]
> @No__t_0은 완전히 포함 된 도형입니다. 첫 번째 및 마지막 점이 일치 하지 않으면 자동으로 연결 됩니다.

## <a name="create-a-polyline"></a>다중선 만들기

`Polyline` 개체를 인스턴스화하고 맵의 `MapElements` 컬렉션에 추가 하 여 맵에 추가할 수 있습니다. 이렇게 하면 다음과 같이 XAML로 수행할 수 있습니다.

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

@No__t_0 및 `StrokeWidth` 속성을 지정 하 여 줄을 사용자 지정 합니다. @No__t_0 속성에는 다중선 점의 지리적 좌표를 정의 하는 `Position` 개체의 목록이 포함 되어 있습니다. @No__t_0 개체는 `Map`의 `MapElements` 컬렉션에 추가 된 경우 맵에 렌더링 됩니다.

## <a name="related-links"></a>관련 링크

- [Maps 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
