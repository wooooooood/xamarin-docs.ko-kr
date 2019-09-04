---
title: Xamarin.ios의 주석 및 오버레이
description: 이 문서에서는 지도 키트의 주석 및 오버레이 기능을 사용 하는 방법을 보여 주는 단계별 연습을 제공 합니다. Xamarin 진화 2013 회의 위치에 주석과 오버레이를 표시 하는 응용 프로그램에 지도를 추가 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 1BC4F7FC-AE3C-46D7-A4D3-18E142F55B8E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: c768003e2737fef191a1afb24b7ac50b28ace9b0
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70226271"
---
# <a name="annotations-and-overlays-in-xamarinios"></a>Xamarin.ios의 주석 및 오버레이

이 연습에서 빌드할 응용 프로그램은 다음과 같습니다.

 [![](ios-maps-walkthrough-images/00-map-overlay.png "MapKit 앱 예제")](ios-maps-walkthrough-images/00-map-overlay.png#lightbox)

[맵 연습 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/mapswalkthrough)에서 완성 된 코드를 찾을 수 있습니다.

먼저 새 **IOS 빈 프로젝트**를 만들고 관련 이름을 지정 합니다. 먼저 뷰 컨트롤러에 MapView를 표시 하는 코드를 추가 하 고 Mapview 및 사용자 지정 주석에 대 한 새 클래스를 만듭니다. 이렇게 하려면 아래 단계를 수행합니다.

## <a name="viewcontroller"></a>ViewController


1. 에 다음 네임 스페이스를 추가 `ViewController`합니다.

    ```csharp
    using MapKit;
    using CoreLocation;
    using UIKit
    using CoreGraphics
    ```

1. 인스턴스와 함께 `MKMapView` `MapDelegate` 인스턴스 변수를 클래스에 추가 합니다. `MapDelegate` 곧 만들 예정입니다.

    ```csharp
    public partial class ViewController : UIViewController
    {
        MKMapView map;
        MapDelegate mapDelegate;
        ...
    ```

1. 컨트롤러 `LoadView` 의 메서드에서를 `MKMapView` 추가 하 고이를 컨트롤러의 속성으로 `View` 설정 합니다.

    ```csharp
    public override void LoadView ()
    {
        map = new MKMapView (UIScreen.MainScreen.Bounds);
        View = map;
    }
    ```

    다음에는 ' ViewDidLoad ' ' 메서드에서 맵을 초기화 하는 코드를 추가 합니다.

1. 지도 `ViewDidLoad` 유형을 설정 하는 코드 추가에서 사용자 위치를 표시 하 고 확대/축소 및 이동을 허용 합니다.

    ```csharp
    // change map type, show user location and allow zooming and panning
    map.MapType = MKMapType.Standard;
    map.ShowsUserLocation = true;
    map.ZoomEnabled = true;
    map.ScrollEnabled = true;

    ```

1. 다음으로 맵을 가운데에 배치 하 고 해당 지역을 설정 하는 코드를 추가 합니다.

    ```csharp
    double lat = 30.2652233534254;
    double lon = -97.73815460962083;
    CLLocationCoordinate2D mapCenter = new CLLocationCoordinate2D (lat, lon);
    MKCoordinateRegion mapRegion = MKCoordinateRegion.FromDistance (mapCenter, 100, 100);
    map.CenterCoordinate = mapCenter;
    map.Region = mapRegion;

    ```

1. 의 `MapDelegate` 새 인스턴스를 만들어의에 할당 `Delegate` `MKMapView`합니다. 잠시 후에 `MapDelegate` implcodeent.

    ```csharp
    mapDelegate = new MapDelegate ();
    map.Delegate = mapDelegate;
    ```

1. IOS 8부터 사용자가 자신의 위치를 사용 하도록 권한 부여를 요청 해야 하므로 샘플에 추가 하겠습니다. 먼저 `CLLocationManager` 클래스 수준 변수를 정의 합니다.

    ```csharp
    CLLocationManager locationManager = new CLLocationManager();
    ```

1. `ViewDidLoad` 메서드에서 응용 프로그램을 실행 하는 장치에서 iOS 8을 사용 하 고 있는지 확인 하 고, 앱을 사용 하는 경우 권한 부여를 요청 합니다.

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion(8,0)){
        locationManager.RequestWhenInUseAuthorization ();
    }
    ```

1. 마지막으로 **info.plist** 파일을 편집 하 여 사용자에 게 위치를 요청 하는 이유를 알려 주어 야 합니다. **Info.plist**의 **원본** 메뉴에서 다음 키를 추가 합니다.

    `NSLocationWhenInUseUsageDescription`

    및 문자열:

    `Maps Walkthrough Docs Sample`.


## <a name="conferenceannotationcs--a-class-for-custom-annotations"></a>ConferenceAnnotation.cs – 사용자 지정 주석에 대 한 클래스입니다.


1. 이라는 `ConferenceAnnotation`주석에 대해 사용자 지정 클래스를 사용할 예정입니다. 프로젝트에 다음 클래스를 추가 합니다.

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

## <a name="viewcontroller---adding-the-annotation-and-overlay"></a>ViewController-주석과 오버레이 추가

1. 이 준비 `ConferenceAnnotation` 되 면 지도에 추가할 수 있습니다. `ViewDidLoad` 의메서드로돌아가서맵의중심좌표`ViewController`에 주석을 추가 합니다.

    ```csharp
    map.AddAnnotations (new ConferenceAnnotation ("Evolve Conference", mapCenter));
    ```

1. 또한 호텔의 오버레이가 필요 합니다. 제공 된 호텔의 좌표를 사용 `MKPolygon` 하 여를 만들고를 호출 `AddOverlay`하 여 맵에 추가 하는 다음 코드를 추가 합니다.

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

그러면의 `ViewDidLoad`코드가 완성 됩니다. 이제 `MapDelegate` 클래스를 구현 하 여 각각 주석과 오버레이 뷰를 만드는 과정을 처리 해야 합니다.


## <a name="mapdelegate"></a>MapDelegate

1. 에서 `annotationId` `MapDelegate` 상속되고주석에재사용식별자로사용할변수를포함하는라는클래스를만듭니다.`MKMapViewDelegate`

    ```csharp
    class MapDelegate : MKMapViewDelegate
    {
        static string annotationId = "ConferenceAnnotation";
        ...
    }
    ```

    여기에는 하나의 주석만 있으므로 재사용 코드가 반드시 필요한 것은 아니지만이를 포함 하는 것이 좋습니다.

1. 이 연습 `GetViewForAnnotation` 에 포함 된 **컨퍼런스 .png** 이미지를 `ConferenceAnnotation` 사용 하 여에 대 한 뷰를 반환 하려면 메서드를 구현 합니다.

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

1. 사용자가 주석을 탭 할 때 오스틴의 도시를 표시 하는 이미지를 표시 하려고 합니다. 이미지 및 뷰에 `MapDelegate` 대해 다음 변수를에 추가 하 여 표시 합니다.

    ```csharp
    UIImageView venueView;
    UIImage venueImage;
    ```

1. 다음으로 주석을 누를 때 이미지를 표시 하려면 다음과 같이 메서드를 구현 `DidSelectAnnotation` 합니다.

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

1. 사용자가 맵의 다른 위치를 눌러 주석을 선택 취소 했을 때 이미지를 숨기려면 다음과 같이 메서드를 `DidSelectAnnotationView` 구현 합니다.

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

    이제 주석에 대 한 코드가 준비 되었습니다. 에 코드 `MapDelegate` 를 추가 하 여 호텔 오버레이에 대 한 뷰를 만들 수 있습니다.

1. 의 `GetViewForOverlay` 다음 구현을에 추가 합니다 `MapDelegate`.

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

애플리케이션을 실행합니다. 이제 사용자 지정 주석과 오버레이가 포함 된 대화형 맵이 있습니다. 아래와 같이 주석을 탭 하면 오스틴 이미지가 표시 됩니다.

 [![](ios-maps-walkthrough-images/01-map-image.png "주석을 탭 하면 오스틴 이미지가 표시 됩니다.")](ios-maps-walkthrough-images/01-map-image.png#lightbox)

## <a name="summary"></a>요약

이 문서에서는 지도에 주석을 추가 하는 방법 뿐만 아니라 지정 된 다각형에 대 한 오버레이를 추가 하는 방법을 살펴보았습니다. 또한 지도 위에 이미지에 애니메이션 효과를 주는 주석에 터치 지원을 추가 하는 방법도 살펴보았습니다.


## <a name="related-links"></a>관련 링크

- [Maps 연습 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/mapswalkthrough)
- [지도 데모 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/mapdemo)
- [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md)
