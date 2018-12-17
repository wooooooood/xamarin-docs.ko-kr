---
title: 맵의 경로 강조 표시
description: 이 문서에서는 맵에 다각형 오버레이를 추가하는 방법을 설명합니다. 다각형 오버레이는 일반적으로 맵에 경로를 표시하거나 필요한 모든 모양을 형성하는 데 사용되는 일련의 연결된 라인 세그먼트입니다.
ms.prod: xamarin
ms.assetid: FBFDC715-1654-4188-82A0-FC522548BCFF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 786f050495d4682b719178f2723c482929544678
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998723"
---
# <a name="highlighting-a-route-on-a-map"></a>맵의 경로 강조 표시

_이 문서에서는 맵에 다각형 오버레이를 추가하는 방법을 설명합니다. 다각형 오버레이는 일반적으로 맵에 경로를 표시하거나 필요한 모든 모양을 형성하는 데 사용되는 일련의 연결된 라인 세그먼트입니다._

## <a name="overview"></a>개요

오버레이는 맵의 계층화된 그래픽입니다. 오버레이는 확대/축소하는 대로 맵을 크기 조정하는 그래픽 그리기 콘텐츠를 지원합니다. 다음 스크린샷은 맵에 폴리라인 오버레이를 추가한 결과를 보여줍니다.

![](polyline-map-overlay-images/screenshots.png)

Xamarin.Forms 애플리케이션에서 [`Map`](xref:Xamarin.Forms.Maps.Map) 컨트롤을 렌더링하면 iOS에서 `MapRenderer` 클래스가 인스턴스화되며, 차례로 네이티브 `MKMapView` 컨트롤을 인스턴스화합니다. Android 플랫폼에서 `MapRenderer` 클래스는 네이티브 `MapView` 컨트롤을 인스턴스화합니다. UWP(유니버설 Windows 플랫폼)에서 `MapRenderer` 클래스는 네이티브 `MapControl`을 인스턴스화합니다. 렌더링 프로세스는 각 플랫폼에서 `Map`에 대한 사용자 지정 렌더러를 만들어 플랫폼별 맵 사용자 지정을 구현하는 데 활용할 수 있습니다. 이 작업을 수행하는 프로세스는 다음과 같습니다.

1. Xamarin.Forms 사용자 지정 맵을 [만듭니다](#Creating_the_Custom_Map).
1. Xamarin.Forms에서 사용자 지정 맵을 [사용합니다](#Consuming_the_Custom_Map).
1. 각 플랫폼의 맵에 대한 사용자 지정 렌더러를 만들어 맵을 [사용자 지정합니다](#Customizing_the_Map).

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps)는 사용 전에 초기화되고 구성되어야 합니다. 자세한 내용은 [`Maps Control`](~/xamarin-forms/user-interface/map.md)를 참조하세요.

사용자 지정 렌더러를 사용하여 맵 사용자 지정에 대한 내용은 [맵 핀 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)을 참조하세요.

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>사용자 지정 맵 만들기

`RouteCoordinates` 속성을 추가하는 [`Map`](xref:Xamarin.Forms.Maps.Map) 클래스의 서브클래스를 만듭니다.

```csharp
public class CustomMap : Map
{
  public List<Position> RouteCoordinates { get; set; }

  public CustomMap ()
  {
    RouteCoordinates = new List<Position> ();
  }
}
```

`RouteCoordinates` 속성은 강조 표시되는 경로를 정의하는 좌표의 컬렉션을 저장합니다.

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
    customMap.RouteCoordinates.Add (new Position (37.785559, -122.396728));
    customMap.RouteCoordinates.Add (new Position (37.780624, -122.390541));
    customMap.RouteCoordinates.Add (new Position (37.777113, -122.394983));
    customMap.RouteCoordinates.Add (new Position (37.776831, -122.394627));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
  }
}
```

이 초기화는 일련의 위도 및 경도 좌표를 지정하여 강조 표시되는 맵의 경로를 정의합니다. 그런 다음, [`Position`](xref:Xamarin.Forms.Maps.Position) 및 [`Distance`](xref:Xamarin.Forms.Maps.Distance)에서 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)을 생성하여 맵의 위치와 확대/축소 수준을 변경하는 [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) 메서드를 사용하여 맵 보기를 배치합니다.

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>맵 사용자 지정

사용자 지정 렌더러는 이제 맵에 폴리라인 오버레이를 추가하도록 각 애플리케이션 프로젝트에 추가되어야 합니다.

#### <a name="creating-the-custom-renderer-on-ios"></a>iOS에서 사용자 지정 렌더러 만들기

`MapRenderer` 클래스의 서브클래스를 만들고 폴리라인 오버레이를 추가하도록 해당 `OnElementChanged` 메서드를 재정의합니다.

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKPolylineRenderer polylineRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    polylineRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                nativeMap.OverlayRenderer = GetOverlayRenderer;

                CLLocationCoordinate2D[] coords = new CLLocationCoordinate2D[formsMap.RouteCoordinates.Count];
                int index = 0;
                foreach (var position in formsMap.RouteCoordinates)
                {
                    coords[index] = new CLLocationCoordinate2D(position.Latitude, position.Longitude);
                    index++;
                }

                var routeOverlay = MKPolyline.FromCoordinates(coords);
                nativeMap.AddOverlay(routeOverlay);
            }
        }
        ...
    }
}

```

사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결되어 있는 경우 이 메서드는 다음 구성을 수행합니다.

- `MKMapView.OverlayRenderer` 속성은 해당 대리자로 설정됩니다.
- 위도 및 경도 좌표의 컬렉션은 `CustomMap.RouteCoordinates` 속성에서 검색되며 `CLLocationCoordinate2D` 인스턴스의 배열로 저장됩니다.
- 폴리라인은 각 지점의 위도 및 경도를 지정하는 정적 `MKPolyline.FromCoordinates` 메서드를 호출하여 만들어집니다.
- 폴리라인은 `MKMapView.AddOverlay` 메서드를 호출하여 맵에 추가됩니다.

그런 다음, `GetOverlayRenderer` 메서드를 구현하여 오버레이의 렌더링을 사용자 지정합니다.

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKPolylineRenderer polylineRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (polylineRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          polylineRenderer = new MKPolylineRenderer(overlay as MKPolyline) {
              FillColor = UIColor.Blue,
              StrokeColor = UIColor.Red,
              LineWidth = 3,
              Alpha = 0.4f
          };
      }
      return polylineRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Android에서 사용자 지정 렌더러 만들기

`MapRenderer` 클래스의 서브클래스를 만들고 폴리라인 오버레이를 추가하도록 해당 `OnElementChanged` 및 `OnMapReady` 메서드를 재정의합니다.

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        List<Position> routeCoordinates;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                routeCoordinates = formsMap.RouteCoordinates;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var polylineOptions = new PolylineOptions();
            polylineOptions.InvokeColor(0x66FF0000);

            foreach (var position in routeCoordinates)
            {
                polylineOptions.Add(new LatLng(position.Latitude, position.Longitude));
            }

            NativeMap.AddPolyline(polylineOptions);
        }
    }
}
```

`OnElementChanged` 메서드는 `CustomMap.RouteCoordinates` 속성에서 위도 및 경도 좌표의 컬렉션을 검색하고 멤버 변수에 저장합니다. 그런 다음, 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결되어 있는 경우 보기에 연결된 내부 `GoogleMap`을 가져오는 `MapView.GetMapAsync` 메서드를 호출합니다. `GoogleMap` 인스턴스를 사용할 수 있으면 `OnMapReady` 메서드가 호출됩니다. 여기에서 폴리라인은 각 지점의 위도 및 경도를 지정하는 `PolylineOptions` 개체를 인스턴스화하여 만들어집니다. 그런 다음, 폴리라인은 `NativeMap.AddPolyline` 메서드를 호출하여 맵에 추가됩니다.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>유니버설 Windows 플랫폼에서 사용자 지정 렌더러 만들기

`MapRenderer` 클래스의 서브클래스를 만들고 폴리라인 오버레이를 추가하도록 해당 `OnElementChanged` 메서드를 재정의합니다.

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
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

                var coordinates = new List<BasicGeoposition>();
                foreach (var position in formsMap.RouteCoordinates)
                {
                    coordinates.Add(new BasicGeoposition() { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polyline = new MapPolyline();
                polyline.StrokeColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polyline.StrokeThickness = 5;
                polyline.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polyline);
            }
        }
    }
}
```

사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결되어 있는 경우 이 메서드는 다음 작업을 수행합니다.

- 위도 및 경도 좌표의 컬렉션은 `CustomMap.RouteCoordinates` 속성에서 검색되며 `BasicGeoposition` 좌표의 `List`로 변환됩니다.
- 폴리라인은 `MapPolyline` 개체를 인스턴스화하여 만들어집니다. `MapPolygon` 클래스는 해당 `Path` 속성을 선 좌표를 포함하는 `Geopath` 개체로 설정하여 맵에 선을 표시하는 데 사용됩니다.
- 폴리라인은 `MapControl.MapElements` 컬렉션에 추가하여 맵에서 렌더링됩니다.

## <a name="summary"></a>요약

이 문서에서는 맵에 폴리라인 오버레이를 추가하여 맵에 경로를 표시하거나 필요한 모든 모양을 형성하는 방법을 설명했습니다.


## <a name="related-links"></a>관련 링크

- [폴리라인 맵 오버레이(샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polyline/)
- [지도 핀 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
