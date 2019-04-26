---
title: 주석 및 Xamarin.iOS에서 오버레이
description: 이 문서에서는 Map 키트의 주석 및 오버레이 기능과 함께 작동 하는 방법을 보여 주는 단계별 연습을 제공 합니다. Xamarin 발전 2013 컨퍼런스에서 위치의 주석 및 오버레이 표시 하는 응용 프로그램에 맵을 추가 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 1BC4F7FC-AE3C-46D7-A4D3-18E142F55B8E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 445661513b0cf79df99d54ed0bb4b0261dd75c2a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61381502"
---
# <a name="annotations-and-overlays-in-xamarinios"></a>주석 및 Xamarin.iOS에서 오버레이

이 연습에서 빌드 하려는 응용 프로그램은 다음과 같습니다.

 [![](ios-maps-walkthrough-images/00-map-overlay.png "MapKit 앱의 예")](ios-maps-walkthrough-images/00-map-overlay.png#lightbox)
 
완성 된 코드를 찾을 수 있습니다 합니다 [Maps 연습 샘플](https://developer.xamarin.com/samples/monotouch/MapsWalkthrough/)합니다.

새 시작 해 보겠습니다 **iOS 빈 프로젝트**, 관련 된 이름을 지정 하 고 있습니다. MapView를 표시 하는 보기 컨트롤러에 코드를 추가 하 여 시작 하 고 우리의 MapDelegate 및 사용자 지정 주석에서는 그런 다음 새 클래스를 만듭니다. 이렇게 하려면 아래 단계를 수행합니다.

## <a name="viewcontroller"></a>ViewController


1. 다음 네임 스페이스를 추가 합니다 `ViewController`:

    ```csharp
    using MapKit;
    using CoreLocation;
    using UIKit
    using CoreGraphics
    ```

1. 추가 `MKMapView` 변수를 클래스에 인스턴스를 `MapDelegate` 인스턴스. 만들겠습니다는 `MapDelegate` 곧:

    ```csharp
    public partial class ViewController : UIViewController
    {
        MKMapView map;
        MapDelegate mapDelegate;
        ...
    ```

1. 컨트롤러의 `LoadView` 메서드를 추가 `MKMapView` 로 설정 된 `View` 컨트롤러의 속성:

    ```csharp
    public override void LoadView ()
    {
        map = new MKMapView (UIScreen.MainScreen.Bounds);
        View = map;
    }
    ```

    그런 다음에 map를 초기화 하는 코드를 추가한는 ' ViewDidLoad ' 메서드.

1. `ViewDidLoad` 지도 유형을 설정, 사용자 위치를 표시 및 확대/축소 및 이동을 허용 하는 코드를 추가 합니다.

    ```csharp
    // change map type, show user location and allow zooming and panning
    map.MapType = MKMapType.Standard;
    map.ShowsUserLocation = true;
    map.ZoomEnabled = true;
    map.ScrollEnabled = true;
    
    ```

1. 다음에서 지도 중심 및 it의 영역을 설정 하는 코드를 추가 합니다.

    ```csharp
    double lat = 30.2652233534254;
    double lon = -97.73815460962083;
    CLLocationCoordinate2D mapCenter = new CLLocationCoordinate2D (lat, lon);
    MKCoordinateRegion mapRegion = MKCoordinateRegion.FromDistance (mapCenter, 100, 100);
    map.CenterCoordinate = mapCenter;
    map.Region = mapRegion;
    
    ```

1. 새 인스턴스를 만듭니다 `MapDelegate` 에 할당 합니다 `Delegate` 의 `MKMapView`합니다. Implcodeent에서는 다시는 `MapDelegate` 곧:

    ```csharp
    mapDelegate = new MapDelegate ();
    map.Delegate = mapDelegate;     
    ```

1. IOS 8 기준으로 해야 요청 있습니다 권한 부여에서 사용자를 해당 위치를 사용 하 여, 샘플에 추가 해 보겠습니다. 먼저, 정의 `CLLocationManager` 클래스 수준 변수:

    ```csharp
    CLLocationManager locationManager = new CLLocationManager();
    ```

1. 에 `ViewDidLoad` 응용 프로그램을 실행 하는 장치에는 iOS 8을 사용 하는 경우 앱은 사용 권한 부여 요청 합니다에서는 확인 하고자 하는 메서드:

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion(8,0)){
                    locationManager.RequestWhenInUseAuthorization ();
                }
    ```

1. 마지막으로 편집 해야 합니다 **Info.plist** 파일을 사용자의 위치를 요청에 대 한 이유를 나타내는 것이 좋습니다. 에 **원본** 메뉴의 **Info.plist**, 다음 키를 추가:
    
    `NSLocationWhenInUseUsageDescription` 
    
    및 문자열을 추가 합니다. 

    `Maps Walkthrough Docs Sample`.


## <a name="conferenceannotationcs--a-class-for-custom-annotations"></a>ConferenceAnnotation.cs – 사용자 지정 주석에 대 한 클래스


1. 호출 된 주석에 대 한 사용자 지정 클래스를 사용 하겠습니다 `ConferenceAnnotation`합니다. 프로젝트에 다음 클래스를 추가 합니다.

    ```csharp
    using System;
    using CoreLocation;
    using MapKit;
    
    namespace MapsWalkthrough
    {
        public class ConferenceAnnotation : MKAnnotation
        {
            string title;
            CLLocationCoordinate2D coord;
    
            public ConferenceAnnotation (string title,
            CLLocationCoordinate2D coord)
            {
                this.title = title;
                this.coord = coord;
            }
    
            public override string Title {
                get {
                    return title;
                }
            }
    
            public override CLLocationCoordinate2D Coordinate {
                get {
                    return coord;
                }
            }
        }
    }   
    ```

## <a name="viewcontroller---adding-the-annotation-and-overlay"></a>ViewController-주석 및 오버레이 추가

1. 사용 하 여는 `ConferenceAnnotation` 위치에 추가할 수 있습니다 것 맵에 있습니다. 다시 합니다 `ViewDidLoad` 메서드는 `ViewController`, 지도의 중심 좌표에 주석 추가:

    ```csharp
    map.AddAnnotations (new ConferenceAnnotation ("Evolve Conference", mapCenter)); 
    ```

1. 또한 하고자 호텔의 오버레이 합니다. 다음 코드를 추가 합니다 `MKPolygon` 제공 된 호텔에 대 한 좌표를 사용 하 고 호출 하 여 지도에 추가 `AddOverlay`:

    ```csharp
    // add an overlay of the hotel
    MKPolygon hotelOverlay = MKPolygon.FromCoordinates(
        new CLLocationCoordinate2D[]{
        new CLLocationCoordinate2D(30.2649977168594, -97.73863627705),
        new CLLocationCoordinate2D(30.2648461170005, -97.7381627734755),
        new CLLocationCoordinate2D(30.2648355402574, -97.7381750192576),
        new CLLocationCoordinate2D(30.2647791309417, -97.7379872505988),
        new CLLocationCoordinate2D(30.2654525150319, -97.7377341711021),
        new CLLocationCoordinate2D(30.2654807195004, -97.7377994819399),
        new CLLocationCoordinate2D(30.2655089239607, -97.7377994819399),
        new CLLocationCoordinate2D(30.2656428950368, -97.738346460207),
        new CLLocationCoordinate2D(30.2650364981811, -97.7385709662122),
        new CLLocationCoordinate2D(30.2650470749025, -97.7386199493406)
    });
    
    map.AddOverlay (hotelOverlay);  
    ```
코드가 완성 되었으므로 `ViewDidLoad`합니다. 이제 구현 해야이 `MapDelegate` 주석 만들기를 처리 하 여 뷰를 각각 오버레이 하는 클래스입니다.


## <a name="mapdelegate"></a>MapDelegate

1. 라는 클래스를 만듭니다 `MapDelegate` 에서 상속 되는 `MKMapViewDelegate` 등과 `annotationId` 주석에 대 한 재사용 식별자로 사용 하는 변수:

    ```csharp
    class MapDelegate : MKMapViewDelegate
    {
        static string annotationId = "ConferenceAnnotation";
        ...
    }
    ```
    만 않았으므로 이상의 주석이 여기 다시 사용할 수 있도록 코드를 엄격 하 게 필요는 없지만 포함 하는 것이 좋습니다.

1. 구현 된 `GetViewForAnnotation` 에 대 한 뷰를 반환 하는 방법 합니다 `ConferenceAnnotation` 를 사용 하 여는 **conference.png** 이 연습에 포함 된 이미지:

    ```csharp
    public override MKAnnotationView GetViewForAnnotation (MKMapView mapView, NSObject annotation)
    {
        MKAnnotationView annotationView = null;
    
        if (annotation is MKUserLocation)
            return null; 
    
        if (annotation is ConferenceAnnotation) {
    
            // show conference annotation
            annotationView = mapView.DequeueReusableAnnotation (annotationId);
    
            if (annotationView == null)
                annotationView = new MKAnnotationView (annotation, annotationId);
        
            annotationView.Image = UIImage.FromFile ("images/conference.png");
            annotationView.CanShowCallout = true;
        } 
    
        return annotationView;
    }
    ```

1. 사용자 주석을 누르면 오스틴 city를 보여 주는 이미지를 표시 하려고 합니다. 다음 변수를 추가 합니다 `MapDelegate` 이미지를 표시 하도록 보기:

    ```csharp
    UIImageView venueView;
    UIImage venueImage;
    ```

1. 다음으로 이미지 주석에 탭 할 때를 표시 하려면 구현 합니다 `DidSelectAnnotation` 같이 메서드:

    ```csharp
    public override void DidSelectAnnotationView (MKMapView mapView, MKAnnotationView view)
    {
        // show an image view when the conference annotation view is selected
        if (view.Annotation is ConferenceAnnotation) {
    
            venueView = new UIImageView ();
            venueView.ContentMode = UIViewContentMode.ScaleAspectFit;
            venueImage = UIImage.FromFile ("image/venue.png");
            venueView.Image = venueImage;
            view.AddSubview (venueView);
    
            UIView.Animate (0.4, () => {
            venueView.Frame = new CGRect (-75, -75, 200, 200); });
        }
    }
    ```

1. 사용자는 주석을 맵에서 다른 곳을 탭 하 여 선택 취소 하는 경우 이미지를 숨기려면를 구현 합니다 `DidSelectAnnotationView` 같이 메서드:

    ```csharp
    public override void DidDeselectAnnotationView (MKMapView mapView, MKAnnotationView view)
    {
        // remove the image view when the conference annotation is deselected
        if (view.Annotation is ConferenceAnnotation) {
    
            venueView.RemoveFromSuperview ();
            venueView.Dispose ();
            venueView = null;
        }
    }
    ```
    이제 코드 주석에 대 한 위치에 있습니다. 남아 있는 모든 코드를 추가 하는 것은 `MapDelegate` 호텔 오버레이 대 한 보기를 만듭니다.

1. 다음 구현을 추가 `GetViewForOverlay` 에 `MapDelegate`:

    ```csharp
    public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
    {
        // return a view for the polygon
        MKPolygon polygon = overlay as MKPolygon;
        MKPolygonView polygonView = new MKPolygonView (polygon);
        polygonView.FillColor = UIColor.Blue;
        polygonView.StrokeColor = UIColor.Red;
        return polygonView;
    }
    ```

애플리케이션을 실행합니다. 이제 사용자 지정 주석 및 오버레이 사용 하 여 대화형 맵을 했습니다! 주석을 탭 하 고 아래와 같이, 오스틴의 이미지가 표시 됩니다.

 [![](ios-maps-walkthrough-images/01-map-image.png "주석을 누르고 오스틴의 이미지가 표시 됩니다.")](ios-maps-walkthrough-images/01-map-image.png#lightbox)

## <a name="summary"></a>요약

이 문서에서 지정 된 다각형에 대 한 오버레이 추가 하는 방법 뿐만 아니라 맵에 주석을 추가 하는 방법을 살펴보았습니다. 터치 지원 이미지 맵을 통해 애니메이션에 대 한 주석을 추가 하는 방법을 설명 했습니다.


## <a name="related-links"></a>관련 링크

- [지도 연습 샘플](https://developer.xamarin.com/samples/monotouch/MapsWalkthrough/)
- [맵 데모 샘플](https://developer.xamarin.com/samples/monotouch/MapDemo/)
- [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md)
