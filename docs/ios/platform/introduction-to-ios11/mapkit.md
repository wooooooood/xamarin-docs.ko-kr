---
title: IOS 11에서 MapKit의 새로운 기능
description: '이 문서에서는 iOS 11의에서 새로운 MapKit 기능 설명: 표식, 나침반 단추, 확장 보기 및 사용자 추적 단추를 그룹화 합니다.'
ms.prod: xamarin
ms.assetid: 304AE5A3-518F-422F-BE24-92D62CE30F34
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/30/2017
ms.openlocfilehash: 3c1b29a4aba03ffe8a3131625ef29cf64766bb6c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61342382"
---
# <a name="new-features-in-mapkit-on-ios-11"></a>IOS 11에서 MapKit의 새로운 기능

iOS 11 MapKit를 다음과 같은 새로운 기능을 추가합니다.

- [클러스터링 하는 주석](#clustering)
- [나침반 단추](#compass)
- [확장 보기](#scale)
- [사용자 추적 단추](#user-tracking)

![나침반 단추 및 클러스터형된 마커를 보여 주는 지도](mapkit-images/cyclemap-heading.png)

<a name="clustering" />

## <a name="automatically-grouping-markers-while-zooming"></a>확대/축소 하는 동안 자동으로 그룹화 표식

샘플 [MapKit 샘플 "Tandm"](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) 새 iOS 11 주석의 클러스터링 기능을 구현 하는 방법을 보여 줍니다.

### <a name="1-create-an-mkpointannotation-subclass"></a>1. 만들기는 `MKPointAnnotation` 서브 클래스

지점 annotation 클래스 맵에서 각 표식을 나타냅니다. 개별적으로 사용 하 여 추가할 수 있습니다 `MapView.AddAnnotation()` 사용 하 여 배열 또는 `MapView.AddAnnotations()`합니다.

시각적 지점 주석을 클래스에 없는, 표식과 연결 된 데이터를 나타내는 필요 (가장 중요 한 점은 `Coordinate` 해당 위도 및 경도 맵에서 속성), 및 사용자 지정 속성:

```csharp
public class Bike : MKPointAnnotation
{
  public BikeType Type { get; set; } = BikeType.Tricycle;
  public Bike(){}
  public Bike(NSNumber lat, NSNumber lgn, NSNumber type)
  {
    Coordinate = new CLLocationCoordinate2D(lat.NFloatValue, lgn.NFloatValue);
    switch(type.NUIntValue) {
      case 0:
        Type = BikeType.Unicycle;
        break;
      case 1:
        Type = BikeType.Tricycle;
        break;
    }
  }
}
```

### <a name="2-create-an-mkmarkerannotationview-subclass-for-single-markers"></a>2. 만들기는 `MKMarkerAnnotationView` 단일 표식에 대 한 하위 클래스입니다.

표식 주석 각 주석의 시각적 표시는 뷰와 같은 속성을 사용 하 여 스타일 지정:

- **MarkerTintColor** – 표식의 색입니다.
- **GlyphText** – 표식에 텍스트를 표시 합니다.
- **GlyphImage** – 표식에 표시 되는 이미지를 설정 합니다.
- **DisplayPriority** – z-순서 (스택 동작)를 결정 지도 표식으로 꽉 찰 경우. 중 하나를 사용 `Required`하십시오 `DefaultHigh`, 또는 `DefaultLow`합니다.

자동 클러스터링을 지원 하려면 설정 해야 합니다.

- **ClusteringIdentifier** – 마커 함께 클러스터를 제어 합니다. 동일한 식별자를 사용 하 여 모든 표식에 수도 있고 서로 다른 식별자를 사용 하 여 함께 그룹화 하는 방식을 제어할 수 있습니다.

```csharp
[Register("BikeView")]
public class BikeView : MKMarkerAnnotationView
{
  public static UIColor UnicycleColor = UIColor.FromRGB(254, 122, 36);
  public static UIColor TricycleColor = UIColor.FromRGB(153, 180, 44);
  public override IMKAnnotation Annotation
  {
    get {
      return base.Annotation;
    }
    set {
      base.Annotation = value;

      var bike = value as Bike;
      if (bike != null){
        ClusteringIdentifier = "bike";
        switch(bike.Type){
          case BikeType.Unicycle:
            MarkerTintColor = UnicycleColor;
            GlyphImage = UIImage.FromBundle("Unicycle");
            DisplayPriority = MKFeatureDisplayPriority.DefaultLow;
            break;
          case BikeType.Tricycle:
            MarkerTintColor = TricycleColor;
            GlyphImage = UIImage.FromBundle("Tricycle");
            DisplayPriority = MKFeatureDisplayPriority.DefaultHigh;
            break;
        }
      }
    }
  }
```

### <a name="3-create-an-mkannotationview-to-represent-clusters-of-markers"></a>3. 만들기는 `MKAnnotationView` 표식의 클러스터를 나타내는

표식의 클러스터를 나타내는 주석을 보기 동안 _수_ 단순 이미지 수를 사용자가 예상 응용 프로그램에서 얼마나 많은 표식에 함께 그룹화 된 하는 방법에 대 한 시각 신호를 제공 합니다.

합니다 [샘플 코드](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) CoreGraphics를 사용 하 여 각 표식 유형의 비율을 원 그래프 표현 뿐만 아니라 클러스터 표식의 수를 렌더링 합니다.

또한 설정 해야 합니다.

- **DisplayPriority** – z-순서 (스택 동작)를 결정 지도 표식으로 꽉 찰 경우. 중 하나를 사용 `Required`하십시오 `DefaultHigh`, 또는 `DefaultLow`합니다.
- **CollisionMode** – `Circle` 또는 `Rectangle`합니다.

```csharp
[Register("ClusterView")]
public class ClusterView : MKAnnotationView
{
  public static UIColor ClusterColor = UIColor.FromRGB(202, 150, 38);
  public override IMKAnnotation Annotation
  {
    get {
      return base.Annotation;
    }
    set {
      base.Annotation = value;
      var cluster = MKAnnotationWrapperExtensions.UnwrapClusterAnnotation(value);
      if (cluster != null)
      {
        var renderer = new UIGraphicsImageRenderer(new CGSize(40, 40));
        var count = cluster.MemberAnnotations.Length;
        var unicycleCount = CountBikeType(cluster.MemberAnnotations, BikeType.Unicycle);

        Image = renderer.CreateImage((context) => {
          // Fill full circle with tricycle color
          BikeView.TricycleColor.SetFill();
          UIBezierPath.FromOval(new CGRect(0, 0, 40, 40)).Fill();
          // Fill pie with unicycle color
          BikeView.UnicycleColor.SetFill();
          var piePath = new UIBezierPath();
          piePath.AddArc(new CGPoint(20,20), 20, 0, (nfloat)(Math.PI * 2.0 * unicycleCount / count), true);
          piePath.AddLineTo(new CGPoint(20, 20));
          piePath.ClosePath();
          piePath.Fill();
          // Fill inner circle with white color
          UIColor.White.SetFill();
          UIBezierPath.FromOval(new CGRect(8, 8, 24, 24)).Fill();
          // Finally draw count text vertically and horizontally centered
          var attributes = new UIStringAttributes() {
            ForegroundColor = UIColor.Black,
            Font = UIFont.BoldSystemFontOfSize(20)
          };
          var text = new NSString($"{count}");
          var size = text.GetSizeUsingAttributes(attributes);
          var rect = new CGRect(20 - size.Width / 2, 20 - size.Height / 2, size.Width, size.Height);
          text.DrawString(rect, attributes);
        });
      }
    }
  }
  public ClusterView(){}
  public ClusterView(MKAnnotation annotation, string reuseIdentifier) : base(annotation, reuseIdentifier)
  {
    DisplayPriority = MKFeatureDisplayPriority.DefaultHigh;
    CollisionMode = MKAnnotationViewCollisionMode.Circle;
    // Offset center point to animate better with marker annotations
    CenterOffset = new CoreGraphics.CGPoint(0, -10);
  }
  private nuint CountBikeType(IMKAnnotation[] members, BikeType type) {
    nuint count = 0;
    foreach(Bike member in members){
      if (member.Type == type) ++count;
    }
    return count;
  }
}
```

### <a name="4-register-the-view-classes"></a>4. 뷰 클래스를 등록 합니다.

경우 지도 보기 컨트롤 만들어집니다와 지도 입출력 확대/축소 하는 대로 자동 클러스터링 동작을 사용 하도록 설정 하려면 주석 보기 유형에 등록 보기에 추가 합니다.

```csharp
MapView.Register(typeof(BikeView), MKMapViewDefault.AnnotationViewReuseIdentifier);
MapView.Register(typeof(ClusterView), MKMapViewDefault.ClusterAnnotationViewReuseIdentifier);
```

### <a name="5-render-the-map"></a>5. 지도 렌더링!

지도 렌더링할 때 위에 주석 마커가 클러스터형 또는 확대/축소 수준에 따라 렌더링 됩니다. 확대/축소 수준을 변경 되 면 클러스터 내부 및 외부로 표식 애니메이트합니다.

![클러스터 된 표식 지도에서 보여 주는 시뮬레이터](mapkit-images/cyclemap-sml.png)

참조를 [섹션을 매핑합니다](~/ios/user-interface/controls/ios-maps/index.md) MapKit 사용 하 여 데이터를 표시 하는 방법은 합니다.

<a name="compass" />

## <a name="compass-button"></a>나침반 단추

iOS 11 지도에서 나침반 꺼내고 뷰의 다른 위치를 렌더링 하는 기능을 추가 합니다. 참조 된 [Tandm 샘플 앱](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) 예입니다.

컴퍼스 (맵 방향이 변경 될 때 실시간 애니메이션 포함), 같은 단추를 만들려면 다른 컨트롤에 렌더링 합니다.

![탐색 모음에서 나침반 단추](mapkit-images/compass-sml.png)

아래 코드 나침반 단추를 만들고 탐색 모음에서 렌더링 합니다.

```csharp
var compass = MKCompassButton.FromMapView(MapView);
compass.CompassVisibility = MKFeatureVisibility.Visible;
NavigationItem.RightBarButtonItem = new UIBarButtonItem(compass);
MapView.ShowsCompass = false; // so we don't have two compasses!
```

`ShowsCompass` 지도 보기 내에서 기본 나침반의 표시 유형을 제어 하려면 속성을 사용할 수 있습니다.

<a name="scale" />

## <a name="scale-view"></a>확장 보기

뷰를 통해 다른 위치에 확장을 추가 합니다 `MKScaleView.FromMapView()` 메서드 뷰 계층 구조에서 다른 곳에서 추가할 눈금 뷰의 인스턴스를 가져옵니다.

![지도에 오버레이 하는 확장 보기](mapkit-images/scale-sml.png)

```csharp
var scale = MKScaleView.FromMapView(MapView);
scale.LegendAlignment = MKScaleViewAlignment.Trailing;
scale.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(scale); // constraints omitted for simplicity
MapView.ShowsScale = false; // so we don't have two scale displays!
```

`ShowsScale` 지도 보기 내에서 기본 나침반의 표시 유형을 제어 하려면 속성을 사용할 수 있습니다.

<a name="user-tracking" />

## <a name="user-tracking-button"></a>사용자 추적 단추

사용자 추적 단추를 사용자의 현재 위치에서 지도를 가운데로 맞춥니다. 사용 된 `MKUserTrackingButton.FromMapView()` 단추 인스턴스를 가져올 서식 변경 사항을 적용 하 고 뷰 계층 구조에서 다른 곳에서 추가 방법입니다.

![지도에 오버레이 하는 사용자 위치 단추](mapkit-images/user-location-sml.png)

```csharp
var button = MKUserTrackingButton.FromMapView(MapView);
button.Layer.BackgroundColor = UIColor.FromRGBA(255,255,255,80).CGColor;
button.Layer.BorderColor = UIColor.White.CGColor;
button.Layer.BorderWidth = 1;
button.Layer.CornerRadius = 5;
button.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(button); // constraints omitted for simplicity
```


## <a name="related-links"></a>관련 링크

- [MapKit 샘플 'Tandm'](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/)
- [MKCompassButton](https://developer.apple.com/documentation/mapkit/mkcompassbutton)
- [새로운 기능에 MapKit (WWDC) (비디오)](https://developer.apple.com/videos/play/wwdc2017/237/)
