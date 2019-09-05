---
title: IOS 11의 MapKit의 새로운 기능
description: 이 문서에서는 iOS 11의 새로운 MapKit 기능에 대해 설명 합니다. 그룹화 표식, 나침반 단추, 눈금 보기 및 사용자 추적 단추
ms.prod: xamarin
ms.assetid: 304AE5A3-518F-422F-BE24-92D62CE30F34
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 08/30/2017
ms.openlocfilehash: c194f2c9f8ea974bf3d6a8798f8a12e246c7d75b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70286689"
---
# <a name="new-features-in-mapkit-on-ios-11"></a>IOS 11의 MapKit의 새로운 기능

iOS 11은 다음과 같은 새 기능을 MapKit에 추가 합니다.

- [주석 클러스터링](#clustering)
- [나침반 단추](#compass)
- [눈금 보기](#scale)
- [사용자 추적 단추](#user-tracking)

![클러스터형 표식 및 나침반 단추를 표시 하는 맵](mapkit-images/cyclemap-heading.png)

<a name="clustering" />

## <a name="automatically-grouping-markers-while-zooming"></a>확대/축소 하는 동안 자동으로 표식 그룹화

샘플 [Mapkit 샘플 "Tandm"](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-mapkitsample) 에서는 새 iOS 11 주석 클러스터링 기능을 구현 하는 방법을 보여 줍니다.

### <a name="1-create-an-mkpointannotation-subclass"></a>1. `MKPointAnnotation` 하위 클래스 만들기

Point annotation 클래스는 맵의 각 표식을 나타냅니다. 을 사용 `MapView.AddAnnotation()` `MapView.AddAnnotations()`하 여 또는 배열에서 개별적으로 추가할 수 있습니다.

Point annotation 클래스에는 시각적 표현이 없습니다. 표식 (가장 중요 `Coordinate` 한 것은 지도의 위도 및 경도 인 속성) 및 사용자 지정 속성을 나타내는 데만 필요 합니다.

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

### <a name="2-create-an-mkmarkerannotationview-subclass-for-single-markers"></a>2. 단일 표식에 대 한 하위클래스만들기`MKMarkerAnnotationView`

표식 주석 보기는 각 주석의 시각적 표시 이며 다음과 같은 속성을 사용 하 여 스타일이 지정 됩니다.

- **MarkerTintColor** – 표식의 색입니다.
- **GlyphText** – 표식에 표시 되는 텍스트입니다.
- **GlyphImage** – 표식에 표시 되는 이미지를 설정 합니다.
- **DisplayPriority** – map이 표식으로 복잡 한 경우 z 순서 (스택 동작)를 결정 합니다. `Required`, `DefaultHigh`또는 중`DefaultLow`하나를 사용 합니다.

자동 클러스터링을 지원 하려면 다음도 설정 해야 합니다.

- **Clusteringidentifier** – 함께 클러스터링 된 마커를 제어 합니다. 모든 표식에 동일한 식별자를 사용 하거나 서로 다른 식별자를 사용 하 여 그룹화 하는 방법을 제어할 수 있습니다.

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

### <a name="3-create-an-mkannotationview-to-represent-clusters-of-markers"></a>3. 표식의 클러스터 `MKAnnotationView` 를 나타내는를 만듭니다.

표식의 클러스터를 나타내는 주석 보기는 간단한 이미지인 반면, 사용자는 앱이 그룹화 된 _표식의 수에_ 대 한 시각적 표시를 제공 하는 것으로 간주 합니다.

이 [샘플 코드](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-mapkitsample) 에서는 CoreGraphics를 사용 하 여 클러스터의 마커 수와 각 표식 유형 비율의 원 그래프 표현을 렌더링 합니다.

또한 다음과 같이 설정 해야 합니다.

- **DisplayPriority** – map이 표식으로 복잡 한 경우 z 순서 (스택 동작)를 결정 합니다. `Required`, `DefaultHigh`또는 중`DefaultLow`하나를 사용 합니다.
- **CollisionMode** – `Circle` 또는`Rectangle`입니다.

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

### <a name="4-register-the-view-classes"></a>4. 뷰 클래스 등록

지도 보기 컨트롤이 만들어지고 뷰에 추가 될 때 맵이 확대 및 축소 될 때 자동 클러스터링 동작을 사용 하도록 설정 하려면 주석 보기 형식을 등록 합니다.

```csharp
MapView.Register(typeof(BikeView), MKMapViewDefault.AnnotationViewReuseIdentifier);
MapView.Register(typeof(ClusterView), MKMapViewDefault.ClusterAnnotationViewReuseIdentifier);
```

### <a name="5-render-the-map"></a>5. 지도를 렌더링 합니다.

지도를 렌더링 하면 확대/축소 수준에 따라 주석 표식이 클러스터링 되거나 렌더링 됩니다. 확대/축소 수준이 변경 됨에 따라 마커는 클러스터에 애니메이션을 적용 합니다.

![지도에서 클러스터형 표식을 보여 주는 시뮬레이터](mapkit-images/cyclemap-sml.png)

MapKit를 사용 하 여 데이터를 표시 하는 방법에 대 한 자세한 내용은 [맵 섹션](~/ios/user-interface/controls/ios-maps/index.md) 을 참조 하세요.

<a name="compass" />

## <a name="compass-button"></a>나침반 단추

iOS 11은 지도에서 나침반을 팝 하 고 뷰의 다른 위치에 렌더링 하는 기능을 추가 합니다. 예는 [Tandm 샘플 앱](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-mapkitsample) 을 참조 하세요.

나침반 (지도 방향이 변경 될 때 라이브 애니메이션 포함) 처럼 보이는 단추를 만들고 다른 컨트롤에 렌더링 합니다.

![탐색 모음의 나침반 단추](mapkit-images/compass-sml.png)

아래 코드는 나침반 단추를 만들고 탐색 모음에 렌더링 합니다.

```csharp
var compass = MKCompassButton.FromMapView(MapView);
compass.CompassVisibility = MKFeatureVisibility.Visible;
NavigationItem.RightBarButtonItem = new UIBarButtonItem(compass);
MapView.ShowsCompass = false; // so we don't have two compasses!
```

속성 `ShowsCompass` 은 지도 보기 내에서 기본 나침반의 표시 여부를 제어 하는 데 사용할 수 있습니다.

<a name="scale" />

## <a name="scale-view"></a>눈금 보기

뷰 계층의 다른 곳에 추가 하기 위해 `MKScaleView.FromMapView()` 메서드를 사용 하 여 뷰의 다른 위치에 눈금을 추가 합니다.

![지도에 중첩 된 눈금 보기](mapkit-images/scale-sml.png)

```csharp
var scale = MKScaleView.FromMapView(MapView);
scale.LegendAlignment = MKScaleViewAlignment.Trailing;
scale.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(scale); // constraints omitted for simplicity
MapView.ShowsScale = false; // so we don't have two scale displays!
```

속성 `ShowsScale` 은 지도 보기 내에서 기본 나침반의 표시 여부를 제어 하는 데 사용할 수 있습니다.

<a name="user-tracking" />

## <a name="user-tracking-button"></a>사용자 추적 단추

사용자 추적 단추는 사용자의 현재 위치에 대 한 지도의 가운데 맞춤을 설정 합니다. `MKUserTrackingButton.FromMapView()` 메서드를 사용 하 여 단추의 인스턴스를 가져오고, 형식 변경을 적용 하 고, 뷰 계층의 다른 위치에 추가 합니다.

![맵에 겹쳐진 사용자 위치 단추](mapkit-images/user-location-sml.png)

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

- [MapKit 샘플 ' Tandm '](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-mapkitsample)
- [MKCompassButton](https://developer.apple.com/documentation/mapkit/mkcompassbutton)
- [MapKit (WWDC)의 새로운 기능 (비디오)](https://developer.apple.com/videos/play/wwdc2017/237/)
