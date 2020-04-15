---
title: 맵의 원형 영역 강조 표시
description: 이 문서에서는 원형 오버레이를 맵에 추가하여 맵의 원형 영역을 강조 표시하는 방법을 설명합니다. iOS 및 Android는 맵에 원형 오버레이를 추가하기 위해 API를 제공하는 반면 UWP에서 오버레이는 다각형으로 렌더링됩니다.
ms.prod: xamarin
ms.assetid: 6FF8BD15-074E-4E6A-9522-F9E2BE32EF12
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 1fe2611e26d357d910cc85800355b42d11e1104b
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "72697177"
---
# <a name="highlighting-a-circular-area-on-a-map"></a>맵의 원형 영역 강조 표시

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-map-circle)

_이 문서에서는 원형 오버레이를 맵에 추가하여 맵의 원형 영역을 강조 표시하는 방법을 설명합니다._

## <a name="overview"></a>개요

오버레이는 맵의 계층화된 그래픽입니다. 오버레이는 확대/축소하는 대로 맵을 크기 조정하는 그래픽 그리기 콘텐츠를 지원합니다. 다음 스크린샷은 맵에 원형 오버레이를 추가한 결과를 보여줍니다.

![](circle-map-overlay-images/screenshots.png)

Xamarin.Forms 애플리케이션에서 [`Map`](xref:Xamarin.Forms.Maps.Map) 컨트롤을 렌더링하면 iOS에서 `MapRenderer` 클래스가 인스턴스화되며, 차례로 네이티브 `MKMapView` 컨트롤을 인스턴스화합니다. Android 플랫폼에서 `MapRenderer` 클래스는 네이티브 `MapView` 컨트롤을 인스턴스화합니다. UWP(유니버설 Windows 플랫폼)에서 `MapRenderer` 클래스는 네이티브 `MapControl`을 인스턴스화합니다. 렌더링 프로세스는 각 플랫폼에서 `Map`에 대한 사용자 지정 렌더러를 만들어 플랫폼별 맵 사용자 지정을 구현하는 데 활용할 수 있습니다. 이 작업을 수행하는 프로세스는 다음과 같습니다.

1. Xamarin.Forms 사용자 지정 맵을 [만듭니다](#Creating_the_Custom_Map).
1. Xamarin.Forms에서 사용자 지정 맵을 [사용합니다](#Consuming_the_Custom_Map).
1. 각 플랫폼의 맵에 대한 사용자 지정 렌더러를 만들어 맵을 [사용자 지정합니다](#Customizing_the_Map).

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps)는 사용 전에 초기화되고 구성되어야 합니다. 자세한 내용은 [`Maps Control`](~/xamarin-forms/user-interface/map/index.md)을 참조하세요.

사용자 지정 렌더러를 사용하여 맵 사용자 지정에 대한 내용은 [맵 핀 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)을 참조하세요.

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>사용자 지정 맵 만들기

`Position` 및 `Radius` 속성이 있는 `CustomCircle` 클래스를 만듭니다.

```csharp
public class CustomCircle
{
  public Position Position { get; set; }
  public double Radius { get; set; }
}
```

그런 다음, `CustomCircle` 형식의 속성을 추가하는 [`Map`](xref:Xamarin.Forms.Maps.Map) 클래스의 서브클래스를 만듭니다.

```csharp
public class CustomMap : Map
{
  public CustomCircle Circle { get; set; }
}
```

<a name="Consuming_the_Custom_Map" />

### <a name="consuming-the-custom-map"></a>사용자 지정 맵 사용

XAML 페이지 인스턴스에서 해당 인스턴스를 선언하여 `CustomMap` 컨트롤을 사용합니다.

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

또는 C# 페이지 인스턴스에서 해당 인스턴스를 선언하여 `CustomMap` 컨트롤을 사용합니다.

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

필요에 따라 `CustomMap` 컨트롤을 초기화합니다.

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

이 초기화에서는 사용자 지정 맵에 [`Pin`](xref:Xamarin.Forms.Maps.Pin) 및 `CustomCircle` 인스턴스를 추가하고, [`Position`](xref:Xamarin.Forms.Maps.Position) 및 [`Distance`](xref:Xamarin.Forms.Maps.Distance)에서 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)을 생성하여 맵의 위치와 확대/축소 수준을 변경하는 [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) 메서드를 사용하여 맵 보기를 배치합니다.

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>맵 사용자 지정

사용자 지정 렌더러는 이제 맵에 원형 오버레이를 추가하도록 각 애플리케이션 프로젝트에 추가되어야 합니다.

#### <a name="creating-the-custom-renderer-on-ios"></a>iOS에서 사용자 지정 렌더러 만들기

`MapRenderer` 클래스의 서브클래스를 만들고 원형 오버레이를 추가하도록 해당 `OnElementChanged` 메서드를 재정의합니다.

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

사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결되어 있는 경우 이 메서드는 다음 구성을 수행합니다.

- `MKMapView.OverlayRenderer` 속성은 해당 대리자로 설정됩니다.
- 원의 중심 및 원의 반지름(미터)을 지정하는 정적 `MKCircle` 개체를 설정하여 원이 만들어집니다.
- 원은 `MKMapView.AddOverlay` 메서드를 호출하여 맵에 추가됩니다.

그런 다음, `GetOverlayRenderer` 메서드를 구현하여 오버레이의 렌더링을 사용자 지정합니다.

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

`MapRenderer` 클래스의 서브클래스를 만들고 원형 오버레이를 추가하도록 해당 `OnElementChanged` 및 `OnMapReady` 메서드를 재정의합니다.

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

사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결되어 있는 경우 `OnElementChanged` 메서드는 사용자 지정 원형 데이터를 검색합니다. `GoogleMap` 인스턴스를 사용할 수 있으면 `OnMapReady` 메서드가 호출됩니다. 여기에서 원은 원의 중심 및 원의 반지름(미터)을 지정하는 `CircleOptions` 개체를 인스턴스화하여 만들어집니다. 그런 다음, 원은 `NativeMap.AddCircle` 메서드를 호출하여 맵에 추가됩니다.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>유니버설 Windows 플랫폼에서 사용자 지정 렌더러 만들기

`MapRenderer` 클래스의 서브클래스를 만들고 원형 오버레이를 추가하도록 해당 `OnElementChanged` 메서드를 재정의합니다.

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

사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결되어 있는 경우 이 메서드는 다음 작업을 수행합니다.

- 원 위치 및 반지름은 `CustomMap.Circle` 속성에서 검색되며 원 경계에 대한 위도 및 경도 좌표를 생성하는 `GenerateCircleCoordinates` 메서드에 전달됩니다. 이 도우미 메서드의 코드는 아래와 같습니다.
- 원 경계 좌표는 `BasicGeoposition` 좌표의 `List`로 변환됩니다.
- 원은 `MapPolygon` 개체를 인스턴스화하여 만들어집니다. `MapPolygon` 클래스는 해당 `Path` 속성을 셰이프 좌표를 포함하는 `Geopath` 개체로 설정하여 맵에 다중 지점 셰이프를 표시하는 데 사용됩니다.
- 다각형은 `MapControl.MapElements` 컬렉션에 추가하여 맵에서 렌더링됩니다.

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

이 문서에서는 원형 오버레이를 맵에 추가하여 맵의 원형 영역을 강조 표시하는 방법을 설명했습니다.

## <a name="related-links"></a>관련 링크

- [원형 맵 오버레이(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-map-circle)
- [지도 핀 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
