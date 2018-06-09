---
title: 지도에 순환 영역을 강조 표시
description: 이 문서 순환 오버레이 순환 지도의 영역을 강조 표시 하는 지도를 추가 하는 방법을 설명 합니다. IOS 및 Android를 순환 오버레이 지도를 추가 하기 위한 Api를 제공, UWP에 오버레이 다각형으로 렌더링 됩니다.
ms.prod: xamarin
ms.assetid: 6FF8BD15-074E-4E6A-9522-F9E2BE32EF12
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 06ea1e788add0064571f01dc1080147e64bb8397
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240287"
---
# <a name="highlighting-a-circular-area-on-a-map"></a>지도에 순환 영역을 강조 표시

_이 문서 순환 오버레이 순환 지도의 영역을 강조 표시 하는 지도를 추가 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

오버레이 지도에 계층된 그래픽 합니다. 오버레이 함에 따라 확장 지도와 확대 하는 그리기 그래픽 콘텐츠를 지원 합니다. 다음 스크린샷에서 지도에 순환 오버레이 추가의 결과 보여 줍니다.

![](circle-map-overlay-images/screenshots.png)

경우는 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) 컨트롤이 ios에서 Xamarin.Forms 응용 프로그램에서 렌더링 되는 `MapRenderer` 네이티브 다시 인스턴스화하는 클래스를 인스턴스화할 `MKMapView` 제어 합니다. Android 플랫폼의 `MapRenderer` 클래스 인스턴스화합니다 네이티브 `MapView` 제어 합니다. 에 플랫폼 UWP (유니버설 Windows)는 `MapRenderer` 클래스 인스턴스화합니다 네이티브 `MapControl`합니다. 렌더링 프로세스 활용 하기 위해 취할 수에 대 한 사용자 지정 렌더러를 만들어 플랫폼별 맵 사용자 지정을 구현는 `Map` 각 플랫폼에 있습니다. 이 수행 하는 프로세스는 다음과 같습니다.

1. [만들](#Creating_the_Custom_Map) Xamarin.Forms 사용자 지정 지도입니다.
1. [사용할](#Consuming_the_Custom_Map) Xamarin.Forms에서 사용자 지정 맵.
1. [사용자 지정](#Customizing_the_Map) 지도 지도 대 한 사용자 지정 렌더러는 각 플랫폼에서 만들어 합니다.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/") 초기화 하 고 사용 하기 전에 구성 해야 합니다. 자세한 내용은 참조 하세요. [`Maps Control`](~/xamarin-forms/user-interface/map.md)

사용자 지정 렌더러를 사용 하 여 지도 사용자 지정 하는 방법에 대 한 정보를 참조 하십시오. [지도 핀을 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)합니다.

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>사용자 지정 맵 만들기

만들기는 `CustomCircle` 를 가진 `Position` 및 `Radius` 속성:

```csharp
public class CustomCircle
{
  public Position Position { get; set; }
  public double Radius { get; set; }
}
```

그런 다음의 서브 클래스를 만듭니다는 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) 형식의 속성을 추가 하는 클래스 `CustomCircle`:

```csharp
public class CustomMap : Map
{
  public CustomCircle Circle { get; set; }
}
```

<a name="Consuming_the_Custom_Map" />

### <a name="consuming-the-custom-map"></a>사용자 지정 맵 사용

사용에서 `CustomMap` XAML 페이지 인스턴스에서 해당 형식의 인스턴스를 선언 하 여 제어 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MapOverlay;assembly=MapOverlay"
             x:Class="MapOverlay.MapPage">
    <ContentPage.Content>
        <local:CustomMap x:Name="customMap" MapType="Street" WidthRequest="{x:Static local:App.ScreenWidth}" HeightRequest="{x:Static local:App.ScreenHeight}" />
    </ContentPage.Content>
</ContentPage>
```

또는 사용할는 `CustomMap` C# 페이지 인스턴스에서 해당 형식의 인스턴스를 선언 하 여 제어:

```csharp
public class MapPageCS : ContentPage
{
    public MapPageCS ()
    {
        var customMap = new CustomMap {
            MapType = MapType.Street,
            WidthRequest = App.ScreenWidth,
            HeightRequest = App.ScreenHeight
        };
        ...
        Content = customMap;
    }
}
```

초기화는 `CustomMap` 필요에 따라 제어:

```csharp
public partial class MapPage : ContentPage
{
  public MapPage ()
  {
    ...
    var pin = new Pin {
      Type = PinType.Place,
      Position = new Position (37.79752, -122.40183),
      Label = "Xamarin San Francisco Office",
      Address = "394 Pacific Ave, San Francisco CA"
    };

    var position = new Position (37.79752, -122.40183);
    customMap.Circle = new CustomCircle {
      Position = position,
      Radius = 1000
    };

    customMap.Pins.Add (pin);
    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (position, Distance.FromMiles (1.0)));
  }
}
```

이 초기화 추가 [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) 및 `CustomCircle` 사용자 지정 지도를 인스턴스 및 위치를 지도의 보기는 [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) 확대/축소와 위치를 변경 하는 메서드 만들어서 지도 수준는 [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) 에서 [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) 및 [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/)합니다.

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>지도 사용자 지정

사용자 지정 렌더러는 이제 순환 오버레이 지도에 추가 하려면 각 응용 프로그램 프로젝트에 추가 되어야 합니다.

#### <a name="creating-the-custom-renderer-on-ios"></a>Ios 사용자 지정 렌더러 만들기

서브 클래스를 만든는 `MapRenderer` 클래스 다이어그램과 해당 `OnElementChanged` 순환 오버레이 추가 하려면 메서드:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKCircleRenderer circleRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    circleRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                var circle = formsMap.Circle;

                nativeMap.OverlayRenderer = GetOverlayRenderer;

                var circleOverlay = MKCircle.Circle(new CoreLocation.CLLocationCoordinate2D(circle.Position.Latitude, circle.Position.Longitude), circle.Radius);
                nativeMap.AddOverlay(circleOverlay);
            }
        }
        ...
    }
}

```

이 메서드는 사용자 지정 렌더러 새 Xamarin.Forms 요소에 연결 되어 있는 경우 다음 구성을 수행 합니다.

- `MKMapView.OverlayRenderer` 속성이 해당 대리자로 설정 되어 있습니다.
- 정적 설정 하 여 원을 만들어집니다 `MKCircle` 미터에서는 원의 중심 및 원의 반지름을 지정 하는 개체입니다.
- 원 호출 하 여 지도에 추가 되는 `MKMapView.AddOverlay` 메서드.

그런 다음 구현에서 `GetOverlayRenderer` 오버레이 렌더링을 사용자 지정 하려면:

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKCircleRenderer circleRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (circleRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          circleRenderer = new MKCircleRenderer(overlay as MKCircle) {
              FillColor = UIColor.Red,
              Alpha = 0.4f
          };
      }
      return circleRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Android 사용자 지정 렌더러 만들기

서브 클래스를 만든는 `MapRenderer` 클래스 다이어그램과 해당 `OnElementChanged` 및 `OnMapReady` 순환 오버레이 추가 하는 메서드:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        CustomCircle circle;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Xamarin.Forms.Maps.Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                circle = formsMap.Circle;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var circleOptions = new CircleOptions();
            circleOptions.InvokeCenter(new LatLng(circle.Position.Latitude, circle.Position.Longitude));
            circleOptions.InvokeRadius(circle.Radius);
            circleOptions.InvokeFillColor(0X66FF0000);
            circleOptions.InvokeStrokeColor(0X66FF0000);
            circleOptions.InvokeStrokeWidth(0);

            NativeMap.AddCircle(circleOptions);
        }
    }
}
```

`OnElementChanged` 메서드 호출의 `MapView.GetMapAsync` 메서드를 기본 `GoogleMap` 사용자 지정 렌더러 새 Xamarin.Forms 요소에 연결 되어 있는 경우는 보기와 연결 됩니다. 한 번의 `GoogleMap` 인스턴스를 사용할 수는 `OnMapReady` 메서드 호출 될, 인스턴스화하여 원 만들어지는 `CircleOptions` 미터에서는 원의 중심 및 원의 반지름을 지정 하는 개체입니다. 이후에 원이 호출 하 여 지도에 추가 됩니다는 `NativeMap.AddCircle` 메서드.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>유니버설 Windows 플랫폼에 사용자 지정 렌더러 만들기

서브 클래스를 만든는 `MapRenderer` 클래스 다이어그램과 해당 `OnElementChanged` 순환 오버레이 추가 하려면 메서드:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
        const int EarthRadiusInMeteres = 6371000;

        protected override void OnElementChanged(ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }
            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MapControl;
                var circle = formsMap.Circle;

                var coordinates = new List<BasicGeoposition>();
                var positions = GenerateCircleCoordinates(circle.Position, circle.Radius);
                foreach (var position in positions)
                {
                    coordinates.Add(new BasicGeoposition { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polygon = new MapPolygon();
                polygon.FillColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeThickness = 5;
                polygon.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polygon);
            }
        }
        // GenerateCircleCoordinates helper method (below)
    }
}
```

사용자 지정 렌더러 새 Xamarin.Forms 요소에 연결 되어 있는 경우이 메서드는 다음 작업을 수행 합니다.

- 원 위치와 반지름에서 검색 되며는 `CustomMap.Circle` 속성에 전달 하 고는 `GenerateCircleCoordinates` 위도 및 경도 생성 하는 방법을 좌표의 원 둘레를 합니다. 이 도우미 메서드에 대 한 코드는 다음과 같습니다.
- 로 변환 하는 원 경계 좌표는 `List` 의 `BasicGeoposition` 좌표입니다.
- 원 인스턴스화하여 만들어집니다는 `MapPolygon` 개체입니다. `MapPolygon` 클래스로 설정 하 여 다중 지점 도형 지도에 표시할 해당 `Path` 속성을는 `Geopath` 셰이프 좌표가 포함 된 개체입니다.
- 다각형은 지도에 추가 하 여 렌더링 되는 `MapControl.MapElements` 컬렉션입니다.


```
List<Position> GenerateCircleCoordinates(Position position, double radius)
{
    double latitude = position.Latitude.ToRadians();
    double longitude = position.Longitude.ToRadians();
    double distance = radius / EarthRadiusInMeteres;
    var positions = new List<Position>();

    for (int angle = 0; angle <=360; angle++)
    {
        double angleInRadians = ((double)angle).ToRadians();
        double latitudeInRadians = Math.Asin(Math.Sin(latitude) * Math.Cos(distance) + Math.Cos(latitude) * Math.Sin(distance) * Math.Cos(angleInRadians));
        double longitudeInRadians = longitude + Math.Atan2(Math.Sin(angleInRadians) * Math.Sin(distance) * Math.Cos(latitude), Math.Cos(distance) - Math.Sin(latitude) * Math.Sin(latitudeInRadians));

        var pos = new Position(latitudeInRadians.ToDegrees(), longitudeInRadians.ToDegrees());
        positions.Add(pos);
    }

    return positions;
}
```

## <a name="summary"></a>요약

이 문서 순환 오버레이 순환 지도의 영역을 강조 표시 하는 지도를 추가 하는 방법을 설명 합니다.


## <a name="related-links"></a>관련 링크

- [순환 맵 Ovlerlay (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/circle/)
- [지도 핀 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)
