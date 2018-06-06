---
title: 주석 및 Xamarin.iOS 오버레이
description: 이 문서에서는 맵 키트의 주석 및 오버레이 기능과 함께 작동 하는 방법을 보여 주는 단계별 연습을 제공 합니다. Xamarin 발전 2013 컨퍼런스에서의 위치에 주석 및 오버레이 표시 하는 응용 프로그램에 맵을 추가 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 1BC4F7FC-AE3C-46D7-A4D3-18E142F55B8E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 7d224f034afc9b841bbf82b2b15b92db2d7820c7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789582"
---
# <a name="annotations-and-overlays-in-xamarinios"></a>주석 및 Xamarin.iOS 오버레이

이 연습에서 작성 하 여 응용 프로그램은 다음과 같습니다.

 [![](ios-maps-walkthrough-images/00-map-overlay.png "예제 MapKit 앱")](ios-maps-walkthrough-images/00-map-overlay.png#lightbox)
 
완성 된 코드를 찾을 수 있습니다는 **MapsWalkthroughComplete** 내의 폴더는 [지도 데모 샘플](https://developer.xamarin.com/samples/monotouch/MapDemo/)합니다.

새 사용 되는지를 **iOS 빈 프로젝트**, 관련 된 이름을 지정 하 고 있습니다. MapView을 표시 하려면 보기 Controller에 코드를 추가 하 여 시작 합니다 하 고 우리의 MapDelegate 및 사용자 정의 주석을에서는 다음 새 클래스를 만듭니다. 이렇게 하려면 아래 단계를 수행합니다.

## <a name="viewcontroller"></a>ViewController


1. 다음 네임 스페이스에 추가 `ViewController`:

    ```csharp
    using MapKit;
    using CoreLocation;
    using UIKit
    using CoreGraphics
    ```

1. 추가 `MKMapView` 변수를 클래스에 인스턴스는 `MapDelegate` 인스턴스. 만들겠습니다는 `MapDelegate` 곧:

    ```csharp
    public partial class ViewController : UIViewController
    {
        MKMapView map;
        MapDelegate mapDelegate;
        ...
    ```

1. 컨트롤러의에서 `LoadView` 메서드를 추가 `MKMapView` 로 설정 하 고는 `View` 컨트롤러의 속성:

    ```csharp
    public override void LoadView ()
    {
        map = new MKMapView (UIScreen.MainScreen.Bounds);
        View = map;
    }
    ```

    맵을 초기화 코드를 추가 하겠습니다 다음으로 ' ViewDidLoad ' 메서드.

1. `ViewDidLoad` 지도 유형 설정, 사용자 위치를 표시 및 확대/축소 및 회전을 허용 하는 코드를 추가 합니다.

    ```csharp
    // change map type, show user location and allow zooming and panning
    map.MapType = MKMapType.Standard;
    map.ShowsUserLocation = true;
    map.ZoomEnabled = true;
    map.ScrollEnabled = true;
    
    ```

1. 그런 다음 지도 가운데 맞춤의 영역을 설정 하는 코드를 추가 합니다.

    ```csharp
    double lat = 30.2652233534254;
    double lon = -97.73815460962083;
    CLLocationCoordinate2D mapCenter = new CLLocationCoordinate2D (lat, lon);
    MKCoordinateRegion mapRegion = MKCoordinateRegion.FromDistance (mapCenter, 100, 100);
    map.CenterCoordinate = mapCenter;
    map.Region = mapRegion;
    
    ```

1. 새 인스턴스를 만들고 `MapDelegate` 에 할당 하는 `Delegate` 의 `MKMapView`합니다. Implcodeent त ु म च 다시는 `MapDelegate` 곧:

    ```csharp
    mapDelegate = new MapDelegate ();
    map.Delegate = mapDelegate;     
    ```

1. IOS 8 기준으로 해야 요청 있습니다 권한 부여에서 사용자가 직접의 위치를 사용 하 여, 이제이 샘플에 추가 하도록 합니다. 먼저 정의 `CLLocationManager` 클래스 수준 변수:

    ```csharp
    CLLocationManager locationManager = new CLLocationManager();
    ```

1. 에 `ViewDidLoad` 를 확인 하는 경우 응용 프로그램을 실행 하는 장치를 iOS 8을 실행 중인 경우 우리는 권한 부여를 요청 응용 프로그램 사용 중인 경우 원하는 메서드를:

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion(8,0)){
                    locationManager.RequestWhenInUseAuthorization ();
                }
    ```

1. 마지막으로 편집 해야는 **Info.plist** 파일의 위치를 요청 하기 위한 이유의 사용자가 알립니다. 에 **소스** 의 메뉴는 **Info.plist**, 다음 키를 추가 합니다.
    
    `NSLocationWhenInUseUsageDescription` 
    
    및 문자열: 

    `Maps Demo`.


## <a name="conferenceannotationcs--a-class-for-custom-annotations"></a>ConferenceAnnotation.cs – 사용자 지정 주석에 대 한 클래스


1. 호출 된 주석에 대 한 사용자 지정 클래스를 사용 하 여 `ConferenceAnnotation`합니다. 프로젝트에 다음 클래스를 추가 합니다.

    ```csharp
    using System;
    using CoreLocation;
    using MapKit;
    
    namespace MapDemo
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

1. 와 `ConferenceAnnotation` 준비에서를 추가할 수 있습니다는 지도에 있습니다. 에 `ViewDidLoad` 의 메서드는 `ViewController`, 지도의 센터 좌표에 주석을 추가:

    ```csharp
    map.AddAnnotations (new ConferenceAnnotation ("Evolve Conference", mapCenter)); 
    ```

1. 또한 호텔의 오버레이 하려고 합니다. 다음 코드를 추가 만듭니다는 `MKPolygon` 제공 호텔에 좌표를 사용 하 고 호출 하 여 지도에 추가 `AddOverlay`:

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
코드가 완성 `ViewDidLoad`합니다. 구현 해야 하는 이제 우리의 `MapDelegate` 클래스를 만드는 주석 처리 및 오버레이 뷰를 각각 사용 합니다.


## <a name="mapdelegate"></a>MapDelegate

1. 라는 클래스를 만들고 `MapDelegate` 에서 상속 되는 `MKMapViewDelegate` 포함는 `annotationId` 주석에 대해서는 다시 사용할 수 있도록 식별자로 사용 하는 변수:

    ```csharp
    class MapDelegate : MKMapViewDelegate
    {
        static string annotationId = "ConferenceAnnotation";
        ...
    }
    ```
    만 있습니다 하나 주석 여기 되므로 다시 사용할 수 있도록 코드를 엄격 하 게 필요 하지 않습니다. 그러나이 기능을 포함 하는 것이 좋습니다.

1. 구현 된 `GetViewForAnnotation` 에 대 한 뷰를 반환 하는 메서드는 `ConferenceAnnotation` 를 사용 하는 **conference.png** 이 연습에 포함 된 이미지:

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

1. 사용자는 주석을 누르면 Austin 도시를 보여 주는 이미지를 표시 하는 것이 좋습니다. 다음 변수를 추가 `MapDelegate` 이미지 및 것을 표시 하려면 보기:

    ```csharp
    UIImageView venueView;
    UIImage venueImage;
    ```

1. 그런 다음 주석을 탭이 수행 되는 경우 이미지를 표시 하도록 구현 된 `DidSelectAnnotation` 메서드를 다음과 같이 합니다.

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

1. 이미지를 숨기려면 지도에서 다른 곳을 탭 하 여 주석을 선택 취소 하는 경우 구현에서 `DidSelectAnnotationView` 메서드를 다음과 같이 합니다.

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
    이제 코드 주석에 대 한 위치에 있는 합니다. 남아 있는 모든 코드를 추가 하는 것은 `MapDelegate` 호텔 오버레이 대 한 보기를 만듭니다.

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

응용 프로그램을 실행합니다. 사용자 지정 주석 및 오버레이 대화형 지도 했습니다! 주석을 누르고 Austin 이미지 아래와 같이 표시 됩니다.

 [![](ios-maps-walkthrough-images/01-map-image.png "주석을 누르고 Austin 이미지가 표시 됩니다.")](ios-maps-walkthrough-images/01-map-image.png#lightbox)

## <a name="summary"></a>요약

이 문서에서 지정 된 다각형에 대 한 오버레이 추가 하는 방법 뿐만 아니라 지도에 주석을 추가 하는 방법을 살펴보았습니다. 터치 조작 지원 이미지 맵을 통해 애니메이션 효과를 주석으로 추가 하는 방법을 보여 줍니다.


## <a name="related-links"></a>관련 링크

- [지도 데모 (샘플)](https://developer.xamarin.com/samples/monotouch/MapDemo/)
- [iOS 맵](~/ios/user-interface/controls/ios-maps/index.md)
