---
title: 지도의 순환 영역 강조 표시
description: 이 문서에서는 지도의 순환 영역 강조 표시 하는 지도를 순환 오버레이 추가 하는 방법에 설명 합니다. IOS 및 Android Api에 대 한 제공 맵에 순환 오버레이 추가 하는 동안 UWP의 오버레이 다각형으로 렌더링 됩니다.
ms.prod: xamarin
ms.assetid: 6FF8BD15-074E-4E6A-9522-F9E2BE32EF12
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 3064296d4c78a3342fb27afc971c37a029987e5e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998559"
---
# <a name="highlighting-a-circular-area-on-a-map"></a>지도의 순환 영역 강조 표시

_이 문서에서는 지도의 순환 영역 강조 표시 하는 지도를 순환 오버레이 추가 하는 방법에 설명 합니다._

## <a name="overview"></a>개요

오버레이 지도에 계층화 된 그래픽입니다. 오버레이 확대/축소 하는 대로 지도 사용 하 여 크기가 조정 되는 그래픽 그리기 콘텐츠를 지원 합니다. 다음 스크린샷은 맵에 순환 오버레이 추가 하는 결과 보여 줍니다.

![](circle-map-overlay-images/screenshots.png)

경우는 [ `Map` ](xref:Xamarin.Forms.Maps.Map) iOS에서 Xamarin.Forms 응용 프로그램에서 컨트롤이 렌더링 되는 `MapRenderer` 클래스가 인스턴스화되면 네이티브를 다시 인스턴스화하는 `MKMapView` 제어 합니다. Android 플랫폼에는 `MapRenderer` 클래스에는 네이티브 인스턴스화합니다 `MapView` 컨트롤입니다. Windows 플랫폼 (UWP (유니버설)을 합니다 `MapRenderer` 클래스에는 네이티브 인스턴스화합니다 `MapControl`합니다. 렌더링 프로세스에 대 한 사용자 지정 렌더러를 만들어 플랫폼별 맵 사용자 지정을 구현 하 여 활용 취할 수 있습니다는 `Map` 각 플랫폼에서 합니다. 이 수행 하는 프로세스는 다음과 같습니다.

1. [만들](#Creating_the_Custom_Map) Xamarin.Forms 사용자 지정 지도입니다.
1. [사용할](#Consuming_the_Custom_Map) Xamarin.Forms에서 사용자 지정 맵.
1. [사용자 지정](#Customizing_the_Map) 각 플랫폼에서 맵에 대 한 사용자 지정 렌더러를 만들어 맵.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) 초기화 하 고 사용 하기 전에 구성 해야 합니다. 자세한 내용은 참조 하세요. [`Maps Control`](~/xamarin-forms/user-interface/map.md)

사용자 지정 렌더러를 사용 하 여 지도 사용자 지정 하는 방법에 대 한 내용은 [지도 핀 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)합니다.

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>사용자 지정 맵 만들기

만들기는 `CustomCircle` 있는 클래스로 `Position` 고 `Radius` 속성:

```csharp
public class CustomCircle
{
  public Position Position { get; set; }
  public double Radius { get; set; }
}
```

서브 클래스를 만든 후에 [ `Map` ](xref:Xamarin.Forms.Maps.Map) 형식의 속성을 추가 하는 클래스 `CustomCircle`:

```csharp
public class CustomMap : Map
{
  public CustomCircle Circle { get; set; }
}
```

<a name="Consuming_the_Custom_Map" />

### <a name="consuming-the-custom-map"></a>사용자 지정 맵 사용

사용 된 `CustomMap` XAML 페이지 인스턴스에서 해당 인스턴스를 선언 하 여 제어 합니다.

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

또는 사용을 `CustomMap` C# 페이지 인스턴스에서 해당 인스턴스를 선언 하 여 제어 합니다.

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

초기화는 `CustomMap` 필요에 따라 제어 합니다.

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

이 초기화 추가 [ `Pin` ](xref:Xamarin.Forms.Maps.Pin) 하 고 `CustomCircle` 사용자 지정 지도 인스턴스를 사용 하 여 지도의 보기를 배치 합니다 [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) 확대/축소와 위치를 변경 하는 메서드 맵을 만들어 수준의 [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) 에서 [ `Position` ](xref:Xamarin.Forms.Maps.Position) 와 [ `Distance` ](xref:Xamarin.Forms.Maps.Distance)합니다.

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>지도 사용자 지정

사용자 지정 렌더러는 이제 맵에 순환 오버레이 추가 하려면 각 응용 프로그램 프로젝트에 추가 되어야 합니다.

#### <a name="creating-the-custom-renderer-on-ios"></a>IOS에서 사용자 지정 렌더러 만들기

서브 클래스를 만든 합니다 `MapRenderer` 클래스 및 재정의 해당 `OnElementChanged` 순환 오버레이 추가 하는 방법.

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

이 메서드는 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결할 때 다음 구성에서 수행 합니다.

- `MKMapView.OverlayRenderer` 해당 대리자를 설정 합니다.
- 정적 설정 하 여 원을 만들어집니다 `MKCircle` 미터에서는 원의 중심 및 원의 반지름을 지정 하는 개체입니다.
- 호출 하 여 맵에 원 추가 됩니다는 `MKMapView.AddOverlay` 메서드.

그런 다음 구현 된 `GetOverlayRenderer` 오버레이의 렌더링을 사용자 지정 하는 방법:

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

#### <a name="creating-the-custom-renderer-on-android"></a>Android에서 사용자 지정 렌더러 만들기

서브 클래스를 만든 합니다 `MapRenderer` 클래스를 재정의 해당 `OnElementChanged` 및 `OnMapReady` 순환 오버레이 추가 하는 방법.

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

`OnElementChanged` 메서드 호출을 `MapView.GetMapAsync` 메서드를 기본 `GoogleMap` 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결할 때 제공한 보기로 연결 됩니다. 한 번 합니다 `GoogleMap` 인스턴스 수를 `OnMapReady` 메서드를 호출할를 인스턴스화하여 원을 만들어지는 `CircleOptions` 미터에서는 원의 중심 및 원의 반지름을 지정 하는 개체. 호출 하 여 맵에 원은 다음 추가 `NativeMap.AddCircle` 메서드.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>유니버설 Windows 플랫폼에서 사용자 지정 렌더러 만들기

서브 클래스를 만든 합니다 `MapRenderer` 클래스 및 재정의 해당 `OnElementChanged` 순환 오버레이 추가 하는 방법.

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

이 메서드는 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결할 때 다음과 같은 작업을 수행 합니다.

- Radius와 원 위치에서 검색 되는 `CustomMap.Circle` 속성 전달할를 `GenerateCircleCoordinates` 위도 및 경도 생성 하는 방법을 원 경계에 대 한 좌표입니다. 이 도우미 메서드의 코드는 다음과 같습니다.
- 원 경계 좌표로 변환 된를 `List` 의 `BasicGeoposition` 좌표입니다.
- 원 인스턴스화하여 생성 되는 `MapPolygon` 개체입니다. 합니다 `MapPolygon` 클래스로 설정 하 여 다중 지점 셰이프를 지도에 표시할 해당 `Path` 속성을는 `Geopath` 셰이프는 좌표가 포함 된 개체입니다.
- 추가 하 여 지도에 다각형 렌더링 되는 `MapControl.MapElements` 컬렉션입니다.


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

이 문서에서는 지도의 순환 영역 강조 표시 하는 지도를 순환 오버레이 추가 하는 방법을 설명 합니다.


## <a name="related-links"></a>관련 링크

- [순환 맵 Ovlerlay (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/circle/)
- [지도 핀 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
