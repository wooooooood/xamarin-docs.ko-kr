---
title: 지도의 지역 강조 표시
description: 이 문서에는 지도 영역을 강조 표시 하는 맵에 다각형 오버레이 추가 하는 방법을 설명 합니다. 다각형은 닫힌된 도형 및 만나게 입력 합니다.
ms.prod: xamarin
ms.assetid: E79EB2CF-8DD6-44A8-B47D-5F0A94FB0A63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 0a11e9c25922531727ad2fee3bbed9c8d4e2b80c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998136"
---
# <a name="highlighting-a-region-on-a-map"></a>지도의 지역 강조 표시

_이 문서에서는 지도 영역을 강조 표시 하는 맵에 다각형 오버레이 추가 하는 방법을 설명 합니다. 다각형은 닫힌된 도형 및 만나게 입력 합니다._

## <a name="overview"></a>개요

오버레이 지도에 계층화 된 그래픽입니다. 오버레이 확대/축소 하는 대로 지도 사용 하 여 크기가 조정 되는 그래픽 그리기 콘텐츠를 지원 합니다. 다음 스크린샷은 맵에 다각형 오버레이 추가 하는 결과 보여 줍니다.

![](polygon-map-overlay-images/screenshots.png)

경우는 [ `Map` ](xref:Xamarin.Forms.Maps.Map) iOS에서 Xamarin.Forms 응용 프로그램에서 컨트롤이 렌더링 되는 `MapRenderer` 클래스가 인스턴스화되면 네이티브를 다시 인스턴스화하는 `MKMapView` 제어 합니다. Android 플랫폼에는 `MapRenderer` 클래스에는 네이티브 인스턴스화합니다 `MapView` 컨트롤입니다. Windows 플랫폼 (UWP (유니버설)을 합니다 `MapRenderer` 클래스에는 네이티브 인스턴스화합니다 `MapControl`합니다. 렌더링 프로세스에 대 한 사용자 지정 렌더러를 만들어 플랫폼별 맵 사용자 지정을 구현 하 여 활용 취할 수 있습니다는 `Map` 각 플랫폼에서 합니다. 이 수행 하는 프로세스는 다음과 같습니다.

1. [만들](#Creating_the_Custom_Map) Xamarin.Forms 사용자 지정 지도입니다.
1. [사용할](#Consuming_the_Custom_Map) Xamarin.Forms에서 사용자 지정 맵.
1. [사용자 지정](#Customizing_the_Map) 각 플랫폼에서 맵에 대 한 사용자 지정 렌더러를 만들어 맵.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) 초기화 하 고 사용 하기 전에 구성 해야 합니다. 자세한 내용은 [`Maps Control`](~/xamarin-forms/user-interface/map.md)를 참조하세요.

사용자 지정 렌더러를 사용 하 여 지도 사용자 지정 하는 방법에 대 한 내용은 [지도 핀 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)합니다.

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>사용자 지정 맵 만들기

서브 클래스를 만든 합니다 [ `Map` ](xref:Xamarin.Forms.Maps.Map) 추가 하는 클래스는 `ShapeCoordinates` 속성:

```csharp
public class CustomMap : Map
{
  public List<Position> ShapeCoordinates { get; set; }

  public CustomMap ()
  {
    ShapeCoordinates = new List<Position> ();
  }
}
```

`ShapeCoordinates` 속성을 강조 표시 영역을 정의 하는 좌표 컬렉션을 저장 합니다.

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
    customMap.ShapeCoordinates.Add (new Position (37.797513, -122.402058));
    customMap.ShapeCoordinates.Add (new Position (37.798433, -122.402256));
    customMap.ShapeCoordinates.Add (new Position (37.798582, -122.401071));
    customMap.ShapeCoordinates.Add (new Position (37.797658, -122.400888));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (0.1)));
  }
}
```

이 초기화 강조 표시할 지도 영역을 정의 하려면 위도 및 경도 좌표 집합을 지정 합니다. 사용 하 여 지도의 보기를 배치 합니다는 [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) 메서드를 만들어 위치 및 지도 확대/축소 수준을 변경 하는 [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) 에서 [ `Position` ](xref:Xamarin.Forms.Maps.Position) 와 [ `Distance` ](xref:Xamarin.Forms.Maps.Distance)합니다.

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>지도 사용자 지정

사용자 지정 렌더러는 이제 맵에 다각형 오버레이 추가 하려면 각 응용 프로그램 프로젝트에 추가 되어야 합니다.

#### <a name="creating-the-custom-renderer-on-ios"></a>IOS에서 사용자 지정 렌더러 만들기

서브 클래스를 만든 합니다 `MapRenderer` 클래스 및 재정의 해당 `OnElementChanged` 다각형 오버레이 추가 하는 방법.

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKPolygonRenderer polygonRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    polygonRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;

                nativeMap.OverlayRenderer = GetOverlayRenderer;

                CLLocationCoordinate2D[] coords = new CLLocationCoordinate2D[formsMap.ShapeCoordinates.Count];

                int index = 0;
                foreach (var position in formsMap.ShapeCoordinates)
                {
                    coords[index] = new CLLocationCoordinate2D(position.Latitude, position.Longitude);
                    index++;
                }

                var blockOverlay = MKPolygon.FromCoordinates(coords);
                nativeMap.AddOverlay(blockOverlay);
            }
        }
        ...
    }
}

```

이 메서드는 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결할 때 다음 구성에서 수행 합니다.

- `MKMapView.OverlayRenderer` 해당 대리자를 설정 합니다.
- 위도 및 경도 좌표 컬렉션에서 검색 되는 `CustomMap.ShapeCoordinates` 속성의 배열로 저장 `CLLocationCoordinate2D` 인스턴스.
- 다각형 정적 호출 하 여 만든 `MKPolygon.FromCoordinates` 및 각 점의 위도 지정 하는 메서드.
- 호출 하 여 다각형을 맵에 추가 됩니다는 `MKMapView.AddOverlay` 메서드. 이 메서드는 자동으로 첫 번째 및 마지막 요소를 연결 하는 선을 그려 다각형을 닫습니다.

그런 다음 구현 된 `GetOverlayRenderer` 오버레이의 렌더링을 사용자 지정 하는 방법:

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKPolygonRenderer polygonRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (polygonRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          polygonRenderer = new MKPolygonRenderer(overlay as MKPolygon) {
              FillColor = UIColor.Red,
              StrokeColor = UIColor.Blue,
              Alpha = 0.4f,
              LineWidth = 9
          };
      }
      return polygonRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Android에서 사용자 지정 렌더러 만들기

서브 클래스를 만든 합니다 `MapRenderer` 클래스 재정의 및 해당 `OnElementChanged` 및 `OnMapReady` 다각형 오버레이 추가 하는 방법:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        List<Position> shapeCoordinates;

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
                shapeCoordinates = formsMap.ShapeCoordinates;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var polygonOptions = new PolygonOptions();
            polygonOptions.InvokeFillColor(0x66FF0000);
            polygonOptions.InvokeStrokeColor(0x660000FF);
            polygonOptions.InvokeStrokeWidth(30.0f);

            foreach (var position in shapeCoordinates)
            {
                polygonOptions.Add(new LatLng(position.Latitude, position.Longitude));
            }
            NativeMap.AddPolygon(polygonOptions);
        }
    }
}
```

합니다 `OnElementChanged` 위도 및 경도 좌표에서의 컬렉션을 검색 하는 메서드는 `CustomMap.ShapeCoordinates` 속성 멤버 변수에 저장 합니다. 그런 다음 호출 하는 `MapView.GetMapAsync` 메서드를 기본 `GoogleMap` 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결할 때 제공한 보기로 연결 됩니다. 한 번 합니다 `GoogleMap` 인스턴스가 사용할 수는 `OnMapReady` 메서드를 호출할를 인스턴스화하여 다각형 만들어지는 `PolygonOptions` 및 각 점의 위도 지정 하는 개체입니다. 호출 하 여 맵에 다각형은 다음 추가 `NativeMap.AddPolygon` 메서드. 이 메서드는 자동으로 첫 번째 및 마지막 요소를 연결 하는 선을 그려 다각형을 닫습니다.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>유니버설 Windows 플랫폼에서 사용자 지정 렌더러 만들기

서브 클래스를 만든 합니다 `MapRenderer` 클래스 및 재정의 해당 `OnElementChanged` 다각형 오버레이 추가 하는 방법.

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
                foreach (var position in formsMap.ShapeCoordinates)
                {
                    coordinates.Add(new BasicGeoposition() { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polygon = new MapPolygon();
                polygon.FillColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeColor = Windows.UI.Color.FromArgb(128, 0, 0, 255);
                polygon.StrokeThickness = 5;
                polygon.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polygon);
            }
        }
    }
}
```

이 메서드는 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결할 때 다음과 같은 작업을 수행 합니다.

- 위도 및 경도 좌표 컬렉션에서 검색 되는 `CustomMap.ShapeCoordinates` 속성 및 변환할를 `List` 의 `BasicGeoposition` 좌표입니다.
- 다각형 인스턴스화하여 생성 되는 `MapPolygon` 개체입니다. 합니다 `MapPolygon` 클래스로 설정 하 여 다중 지점 셰이프를 지도에 표시할 해당 `Path` 속성을는 `Geopath` 셰이프는 좌표가 포함 된 개체입니다.
- 추가 하 여 지도에 다각형 렌더링 되는 `MapControl.MapElements` 컬렉션입니다. 참고 다각형의 첫 번째 및 마지막 지점을 연결 하는 선을 그리기 하 여 자동으로 종료 됩니다.

## <a name="summary"></a>요약

이 문서에서는 지도의 영역을 강조 표시 하는 맵에 다각형 오버레이 추가 하는 방법을 설명 합니다. 다각형은 닫힌된 도형 및 만나게 입력 합니다.


## <a name="related-links"></a>관련 링크

- [다각형 맵 오버레이 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polygon/)
- [지도 핀 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
