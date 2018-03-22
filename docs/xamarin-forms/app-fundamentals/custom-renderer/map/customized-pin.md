---
title: "지도 핀을 사용자 지정"
description: "이 문서에는 각 플랫폼에 사용자 지정 된 pin과 pin 데이터의 사용자 지정된 보기를 사용 하 여 네이티브 map을 표시 하는 맵 컨트롤에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: C5481D86-80E9-4E3D-9FB6-57B0F93711A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 312bda44a6b390c6ba486d5a3d60dfe4fb770a2e
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="customizing-a-map-pin"></a>지도 핀을 사용자 지정

_이 문서에는 각 플랫폼에 사용자 지정 된 pin과 pin 데이터의 사용자 지정된 보기를 사용 하 여 네이티브 map을 표시 하는 맵 컨트롤에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다._

모든 Xamarin.Forms 보기에 네이티브 컨트롤의 인스턴스를 생성 하는 각 플랫폼에 대 한 함께 제공 되는 렌더러 있습니다. 경우는 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) ios에서는 Xamarin.Forms 응용 프로그램에서 렌더링 되는 `MapRenderer` 네이티브 다시 인스턴스화하는 클래스를 인스턴스화할 `MKMapView` 제어 합니다. Android 플랫폼의 `MapRenderer` 클래스 인스턴스화합니다 네이티브 `MapView` 제어 합니다. 에 플랫폼 UWP (유니버설 Windows)는 `MapRenderer` 클래스 인스턴스화합니다 네이티브 `MapControl`합니다. 렌더러 및 Xamarin.Forms 컨트롤에 매핑되는 네이티브 컨트롤 클래스에 대 한 자세한 내용은 참조 [렌더러 기본 클래스와 기본 컨트롤](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)합니다.

다음 다이어그램에서는 간의 관계는 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) 및 구현 하는 해당 네이티브 컨트롤:

![](customized-pin-images/map-classes.png "지도 컨트롤 및 구현 네이티브 컨트롤 간의 관계")

에 대 한 사용자 지정 렌더러를 만들어 플랫폼 관련 사용자 지정을 구현 하는 렌더링 프로세스를 사용할 수 있습니다는 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) 각 플랫폼에 있습니다. 이 수행 하는 프로세스는 다음과 같습니다.

1. [만들](#Creating_the_Custom_Map) Xamarin.Forms 사용자 지정 지도입니다.
1. [사용할](#Consuming_the_Custom_Map) Xamarin.Forms에서 사용자 지정 맵.
1. [만들](#Creating_the_Custom_Renderer_on_each_Platform) 각 플랫폼에서 지도 대 한 사용자 지정 렌더러 합니다.

각 항목 이제 살펴봅니다 차례로 구현 하는 `CustomMap` 렌더러는 각 플랫폼에서 사용자 지정 된 pin과 pin 데이터의 사용자 지정된 보기를 사용 하 여 네이티브 map을 표시 하는 합니다.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) 초기화 하 고 사용 하기 전에 구성 해야 합니다. 자세한 내용은 참조 [ `Maps Control` ](~/xamarin-forms/user-interface/map.md)합니다.

<a name="Creating_the_Custom_Map" />

## <a name="creating-the-custom-map"></a>사용자 지정 맵 만들기

사용자 지정 지도 컨트롤 서브클래싱 여 만들 수 있습니다는 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) 다음 코드 예제와 같이 클래스:

```csharp
public class CustomMap : Map
{
  public List<CustomPin> CustomPins { get; set; }
}
```

`CustomMap` 제어 이식 가능한 클래스 라이브러리 (PCL) 프로젝트에서 생성 및 사용자 지정 지도 대 한 API를 정의 합니다. 사용자 지정 지도 표시의 `CustomPins` 속성의 컬렉션을 나타내는 `CustomPin` 각 플랫폼에 네이티브 지도 컨트롤에서 렌더링 되는 개체입니다. `CustomPin` 클래스는 다음 코드 예제에 표시 됩니다.

```csharp
public class CustomPin : Pin
{
  public string Url { get; set; }
}
```

이 클래스는 정의 `CustomPin` 원본의 속성을 상속으로 [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) 클래스 및 추가 `Url` 속성.

<a name="Consuming_the_Custom_Map" />

## <a name="consuming-the-custom-map"></a>사용자 지정 맵 사용

`CustomMap` 컨트롤 수 XAML에서 참조할 수는 PCL 프로젝트에 해당 위치에 대 한 네임 스페이스를 선언 하 고 사용자 지정 지도 컨트롤에서 네임 스페이스 접두사를 사용 하 여 합니다. 다음 코드 예제는 방법을 `CustomMap` 컨트롤 XAML 페이지 에서도 사용할 수 있습니다.

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer">
    <ContentPage.Content>
        <local:CustomMap x:Name="myMap" MapType="Street"
          WidthRequest="{x:Static local:App.ScreenWidth}"
          HeightRequest="{x:Static local:App.ScreenHeight}" />
    </ContentPage.Content>
</ContentPage>
```

`local` 원하는 네임 스페이스 접두사를 이름을 지정할 수 있습니다. 그러나는 `clr-namespace` 및 `assembly` 값에는 사용자 지정 지도 대 한 세부 정보과 일치 해야 합니다. 네임 스페이스 선언 되 면 사용자 지정 지도 참조 하는 접두사가 사용 됩니다.

다음 코드 예제는 방법을 `CustomMap` 컨트롤 C# 페이지 에서도 사용할 수 있습니다.

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

`CustomMap` 각 플랫폼에 네이티브 지도 표시 하려면 인스턴스가 사용 됩니다. 있기 [ `MapType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.MapType/) 의 표시 스타일을 설정 하는 속성은 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/)에 정의 되 고 가능한 값으로는 [ `MapType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapType/) 열거 합니다. IOS 및 Android에 대 한 너비와 높이 지도 설정의 속성을 통해는 `App` 플랫폼별 프로젝트에서 초기화 되는 클래스입니다.

지도와 핀의 위치를 포함, 다음 코드 예제와 같이 초기화 됩니다.

```csharp
public MapPage ()
{
  ...
  var pin = new CustomPin {
    Type = PinType.Place,
    Position = new Position (37.79752, -122.40183),
    Label = "Xamarin San Francisco Office",
    Address = "394 Pacific Ave, San Francisco CA",
    Id = "Xamarin",
    Url = "http://xamarin.com/about/"
  };

  customMap.CustomPins = new List<CustomPin> { pin };
  customMap.Pins.Add (pin);
  customMap.MoveToRegion (MapSpan.FromCenterAndRadius (
    new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
}
```

사용자 지정 핀을 추가 하 고 지도의 보기를 배치 하는이 초기화는 [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) 메서드를 만들어 위치 및 지도 확대/축소 수준을 변경 하는 [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) 는 에서[ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) 및 [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/)합니다.

사용자 지정 렌더러는 이제 기본 맵 컨트롤을 사용자 지정 하려면 각 응용 프로그램 프로젝트에 추가할 수 있습니다.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>각 플랫폼에서 사용자 지정 렌더러 만들기

사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. 서브 클래스를 만든는 `MapRenderer` 사용자 지정 지도 렌더링 하는 클래스입니다.
1. 재정의 `OnElementChanged` 렌더링 하는 사용자 지정 맵과 사용자 지정 논리를 작성할 메서드입니다. 이 메서드는 해당 Xamarin.Forms 사용자 지정 지도 만들 때 호출 됩니다.
1. 추가 `ExportRenderer` 특성을 사용자 지정 렌더러 클래스 렌더링 Xamarin.Forms 사용자 지정 맵 사용 수를 지정할 수 있습니다. 이 특성은 Xamarin.Forms를 사용한 사용자 지정 렌더러를 등록 하려면 사용 합니다.

> [!NOTE]
> 이 각 플랫폼 프로젝트에서 사용자 지정 렌더러를 제공 하는 선택 사항. 사용자 지정 렌더러 등록 되지 않은 경우 컨트롤의 기본 클래스에 대 한 기본 렌더러 사용 됩니다.

다음 다이어그램은 이들 간의 관계와 함께 샘플 응용 프로그램의 각 프로젝트의 책임을 보여줍니다.

![](customized-pin-images/solution-structure.png "CustomMap 사용자 지정 렌더러 프로젝트 책임")

`CustomMap` 컨트롤에서 파생 되는 플랫폼 특정 렌더러 클래스에서 렌더링 되는 `MapRenderer` 각 플랫폼에 대 한 클래스입니다. 이 인해 각 `CustomMap` 다음 스크린샷에서 같이 플랫폼 관련 컨트롤을 사용 하 여 렌더링 되 고 제어 합니다.

![](customized-pin-images/screenshots.png "각 플랫폼에서 CustomMap")

`MapRenderer` 클래스가 노출은 `OnElementChanged` 메서드를 해당 네이티브 컨트롤을 렌더링 하 Xamarin.Forms 사용자 지정 지도 만들 때 호출 됩니다. 이 메서드는 `ElementChangedEventArgs` 포함 하는 매개 변수 `OldElement` 및 `NewElement` 속성입니다. Xamarin.Forms 요소를 나타내야 하는 이러한 속성 하는 렌더러 *되었습니다* 에, 연결 된 및 Xamarin.Forms 요소는 렌더러 *은* 에, 각각 연결 된 합니다. 샘플 응용 프로그램에서의 `OldElement` 속성이 `null` 및 `NewElement` 속성에 대 한 참조에 포함 됩니다는 `CustomMap` 인스턴스.

재정의 된 버전의 `OnElementChanged` 각 플랫폼별 렌더러 클래스에서 메서드는 네이티브 컨트롤 사용자 지정을 수행할 위치 합니다. 플랫폼에서 사용 되는 네이티브 컨트롤에 대 한 형식화 된 참조를 통해 액세스할 수는 `Control` 속성입니다. 또한 통해 렌더링 되는 Xamarin.Forms 컨트롤에 대 한 참조를 가져올 수 있습니다는 `Element` 속성입니다.

이벤트 처리기를 구독할 때는 주의 기울여야 합니다는 `OnElementChanged` 메서드를 다음 코드 예제에서와 같이:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null) {
    // Unsubscribe from event handlers
  }

  if (e.NewElement != null) {
    // Configure the native control and subscribe to event handlers
  }
}
```

기본 컨트롤을 구성 해야 하 고 사용자 지정 렌더러 새 Xamarin.Forms 요소에 연결 된 경우에 이벤트 처리기를 구독 합니다. 마찬가지로, 구독 했던 모든 이벤트 처리기는 요소에 연결 된 렌더러는 변경 될 때만에서 구독 수 없습니다. 이 방법을 채택 메모리 누수에서 발생 하지 않는 사용자 지정 렌더러를 만드는 데 도움이 됩니다.

각 사용자 지정 렌더러 클래스도 데코레이팅되 어는 `ExportRenderer` xamarin.forms 렌더러를 등록 하는 특성입니다. 속성에는 두 개의 매개 변수-렌더링 되 고 Xamarin.Forms 사용자 지정 컨트롤의 형식 이름 및 사용자 지정 렌더러의 유형 이름을 사용 합니다. `assembly` 특성에 대 한 접두사 특성이 전체 어셈블리에 적용 되도록 지정 합니다.

다음 섹션에서는 각 사용자 지정 렌더러 플랫폼 특정 클래스의 구현에 설명 합니다.

### <a name="creating-the-custom-renderer-on-ios"></a>Ios 사용자 지정 렌더러 만들기

다음 스크린샷에서 전과 후 사용자 지정 지도 보여 줍니다.

![](customized-pin-images/map-layout-ios.png "지도 컨트롤 전과 후 사용자 지정")

IOS에서 pin 라고는 *주석*, 사용자 지정 이미지 또는 시스템에서 정의한 pin 다양 한 색 중 하나일 수 있습니다. 주석 필요에 따라 표시할 수 있습니다는 *설명선*, 표시할 주석을 선택 하면 사용자에 대 한 응답에서입니다. 설명선이 표시 됩니다는 `Label` 및 `Address` 의 속성은 `Pin` 선택적 왼쪽 및 오른쪽 액세서리 보기 인스턴스. 왼쪽된 액세서리 보기 되 고 오른쪽 액세서리 보기 원숭이, 이미지는 또한 위의 스크린 샷에서 *정보* 단추입니다.

다음 코드 예제에서는 iOS 플랫폼에 대 한 사용자 지정 렌더러를 보여 줍니다.

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        UIView customPinView;
        List<CustomPin> customPins;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveAnnotations(nativeMap.Annotations);
                    nativeMap.GetViewForAnnotation = null;
                    nativeMap.CalloutAccessoryControlTapped -= OnCalloutAccessoryControlTapped;
                    nativeMap.DidSelectAnnotationView -= OnDidSelectAnnotationView;
                    nativeMap.DidDeselectAnnotationView -= OnDidDeselectAnnotationView;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                customPins = formsMap.CustomPins;

                nativeMap.GetViewForAnnotation = GetViewForAnnotation;
                nativeMap.CalloutAccessoryControlTapped += OnCalloutAccessoryControlTapped;
                nativeMap.DidSelectAnnotationView += OnDidSelectAnnotationView;
                nativeMap.DidDeselectAnnotationView += OnDidDeselectAnnotationView;
            }
        }
        ...
    }
}
```

`OnElementChanged` 메서드를 다음 구성을 수행 된 [ `MKMapView` ](https://developer.xamarin.com/api/type/MapKit.MKMapView/) 인스턴스를 사용자 지정 렌더러 새 Xamarin.Forms 요소에 연결 되어 있는 경우:

- [ `GetViewForAnnotation` ](https://developer.xamarin.com/api/property/MapKit.MKMapView.GetViewForAnnotation/) 속성이로 설정 되 고 `GetViewForAnnotation` 메서드. 이 메서드를 호출한 경우는 [주석의 위치 지도에 표시 됩니다.](#Displaying_the_Annotation)를 표시 하려면 주석 전에 사용자 지정 하는 데 사용 합니다.
- 에 대 한 이벤트 처리기는 `CalloutAccessoryControlTapped`, `DidSelectAnnotationView`, 및 `DidDeselectAnnotationView` 이벤트를 등록 합니다. 이러한 이벤트를 발생 시키는 경우 사용자 [설명선에 오른쪽 액세서리 탭](#Tapping_on_the_Right_Callout_Accessory_View), 시기 및 사용자 [선택](#Selecting_the_Annotation) 및 [선택이 취소](#Deselecting_the_Annotation) 주석을 각각. 요소는 렌더러 변경 내용에 연결 된 경우에 이벤트에서 구독 되지 않습니다.

<a name="Displaying_the_Annotation" />

#### <a name="displaying-the-annotation"></a>주석 표시

`GetViewForAnnotation` 메서드는 주석의 위치는 지도에 표시 되 고 표시 하려면 주석 전에 사용자 지정 하는 데 사용 됩니다. 주석에 두 개의 구성 됩니다.

- `MkAnnotation` – 제목, 부제목 및 주석의 위치를 포함 합니다.
- `MkAnnotationView` – 주석 및 필요에 따라 사용자가 주석을 누를 때 표시 되는 설명선을 나타내는 이미지를 포함 합니다.

`GetViewForAnnotation` 메서드에 `IMKAnnotation` 주석 데이터를 포함 하 고 반환 하는 `MKAnnotationView` 지도에 표시 하기 위해 다음 코드 예제에 표시 됩니다:

```csharp
MKAnnotationView GetViewForAnnotation(MKMapView mapView, IMKAnnotation annotation)
{
    MKAnnotationView annotationView = null;

    if (annotation is MKUserLocation)
        return null;

    var customPin = GetCustomPin(annotation as MKPointAnnotation);
    if (customPin == null) {
        throw new Exception("Custom pin not found");
    }

    annotationView = mapView.DequeueReusableAnnotation(customPin.Id.ToString());
    if (annotationView == null) {
        annotationView = new CustomMKAnnotationView(annotation, customPin.Id.ToString());
        annotationView.Image = UIImage.FromFile("pin.png");
        annotationView.CalloutOffset = new CGPoint(0, 0);
        annotationView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile("monkey.png"));
        annotationView.RightCalloutAccessoryView = UIButton.FromType(UIButtonType.DetailDisclosure);
        ((CustomMKAnnotationView)annotationView).Id = customPin.Id.ToString();
        ((CustomMKAnnotationView)annotationView).Url = customPin.Url;
    }
    annotationView.CanShowCallout = true;

    return annotationView;
}
```

이미지를 사용자 지정으로 주석이 표시 됩니다 되지 않고을 시스템에서 정의한 핀으로 주석이 탭 설명선 표시 됩니다 왼쪽 및 오른쪽에 주석이 제목 및 주소 추가 콘텐츠를 포함 하는이 방법을 사용 하면 . 이 작업은 다음과 같이 수행 됩니다.

1. `GetCustomPin` 메서드는 주석에 대 한 사용자 지정 pin 데이터를 반환 합니다.
1. 메모리를 절약 하기 위해 주석 보기에 대 한 호출으로 다시 사용 하기 위해 풀링된 [ `DequeueReusableAnnotation` ](https://developer.xamarin.com/api/member/MapKit.MKMapView.DequeueReusableAnnotation/(System.String)/)합니다.
1. `CustomMKAnnotationView` 클래스 확장는 `MKAnnotationView` 클래스와 `Id` 및 `Url` 에서 동일한 속성에 해당 하는 속성의 `CustomPin` 인스턴스. 새 인스턴스는 `CustomMKAnnotationView` 주석이 만들어질 `null`:
  - `CustomMKAnnotationView.Image` 지도에 주석을 나타내는 이미지에 속성이 설정 되어 있습니다.
  - `CustomMKAnnotationView.CalloutOffset` 속성이로 설정 되는 `CGPoint` 설명선은 주석을 위에 가운데를 지정 하는 합니다.
  - `CustomMKAnnotationView.LeftCalloutAccessoryView` 주석 제목 및 주소의 왼쪽에 표시 될 원숭이의 이미지에 속성이 설정 되어 있습니다.
  - `CustomMKAnnotationView.RightCalloutAccessoryView` 속성이로 설정 되는 *정보* 주석 제목 및 주소의 오른쪽에 나타나는 단추입니다.
  - `CustomMKAnnotationView.Id` 속성이로 설정 되는 `CustomPin.Id` 속성에서 반환 되는 `GetCustomPin` 메서드. 이렇게 하면을 갖도록 식별할 수에 대 한 주석을 [설명선 추가로 사용자 지정할 수](#Selecting_the_Annotation)사용할 경우.
  - `CustomMKAnnotationView.Url` 속성이로 설정 되는 `CustomPin.Url` 속성에서 반환 되는 `GetCustomPin` 메서드. URL 이동 하 게 될 때 사용자 [가 오른쪽 설명선 액세서리 보기에 표시 단추를 누를](#Tapping_on_the_Right_Callout_Accessory_View)합니다.
1. [ `MKAnnotationView.CanShowCallout` ](https://developer.xamarin.com/api/property/MapKit.MKAnnotationView.CanShowCallout/) 속성이 `true` 설명선 주석이 탭이 수행 되는 경우 표시 되도록 합니다.
1. 지도에 표시 하기 위해 주석을 반환 됩니다.

<a name="Selecting_the_Annotation" />

#### <a name="selecting-the-annotation"></a>주석 선택

사용자가 주석을 누르면는 `DidSelectAnnotationView` 차례로 실행 되는 이벤트 발생은 `OnDidSelectAnnotationView` 메서드:

```csharp
void OnDidSelectAnnotationView (object sender, MKAnnotationViewEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  customPinView = new UIView ();

  if (customView.Id == "Xamarin") {
    customPinView.Frame = new CGRect (0, 0, 200, 84);
    var image = new UIImageView (new CGRect (0, 0, 200, 84));
    image.Image = UIImage.FromFile ("xamarin.png");
    customPinView.AddSubview (image);
    customPinView.Center = new CGPoint (0, -(e.View.Frame.Height + 75));
    e.View.AddSubview (customPinView);
  }
}
```

이 메서드를 추가 하 여 기존 설명선 (즉 왼쪽 및 오른쪽 액세서리 뷰 포함)을 확장 한 `UIView` 에 게 선택한 주석이 Xamarin 로고의 이미지를 포함 하는 인스턴스를 해당 `Id` 로설정하는속성`Xamarin`. 따라서 다른 설명선 다른 주석에 표시 될 수 있는 시나리오 수 있습니다. `UIView` 인스턴스 기존 설명선 위쪽 가운데에 표시 됩니다.

<a name="Tapping_on_the_Right_Callout_Accessory_View" />

#### <a name="tapping-on-the-right-callout-accessory-view"></a>오른쪽 설명선 액세서리 보기 탭

사용자가 누를 때는 *정보* 오른쪽 설명선 액세서리 보기에서 단추는 `CalloutAccessoryControlTapped` 차례로 실행 되는 이벤트 발생은 `OnCalloutAccessoryControlTapped` 메서드:

```csharp
void OnCalloutAccessoryControlTapped (object sender, MKMapViewAccessoryTappedEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  if (!string.IsNullOrWhiteSpace (customView.Url)) {
    UIApplication.SharedApplication.OpenUrl (new Foundation.NSUrl (customView.Url));
  }
}
```

이 메서드는 웹 브라우저를 열고에 저장 된 주소로 이동는 `CustomMKAnnotationView.Url` 속성입니다. 에 주소를 만들 때 정의 된는 `CustomPin` PCL 프로젝트에는 컬렉션입니다.

<a name="Deselecting_the_Annotation" />

#### <a name="deselecting-the-annotation"></a>주석을 선택 취소

주석이 표시 되 고 사용자가 지도 누를 때는 `DidDeselectAnnotationView` 차례로 실행 되는 이벤트 발생은 `OnDidDeselectAnnotationView` 메서드:

```csharp
void OnDidDeselectAnnotationView (object sender, MKAnnotationViewEventArgs e)
{
  if (!e.View.Selected) {
    customPinView.RemoveFromSuperview ();
    customPinView.Dispose ();
    customPinView = null;
  }
}
```

이 방법을 사용 하면 기존 설명선 선택 하지 않으면 설명선 (Xamarin 로고 이미지)의 확장 된 일부 표시 되 고 중지 됩니다 하 고 해당 리소스가 해제 됩니다.

사용자 지정 하는 방법에 대 한 자세한 내용은 `MKMapView` 인스턴스를 참조 [iOS 지도](~/ios/user-interface/controls/ios-maps/index.md)합니다.

### <a name="creating-the-custom-renderer-on-android"></a>Android 사용자 지정 렌더러 만들기

다음 스크린샷에서 전과 후 사용자 지정 지도 보여 줍니다.

![](customized-pin-images/map-layout-android.png "지도 컨트롤 전과 후 사용자 지정")

Android에서 pin 라고는 *표식*, 사용자 지정 이미지 또는 다양 한 색 중 시스템에서 정의한 표식 수 있습니다. 표식을 표시할 수는 *정보 창의*를 눌러 표식에 대 한 사용자에 대 한 응답에 표시 되는 합니다. 정보 창에 표시 됩니다는 `Label` 및 `Address` 의 속성은 `Pin` 인스턴스를 선택한 다른 콘텐츠를 포함 하도록 사용자 지정할 수 있습니다. 그러나 하나의 정보 창은 한 번에 표시할 수 있습니다.

다음 코드 예제에서는 Android 플랫폼에 대 한 사용자 지정 렌더러를 보여 줍니다.

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.Droid
{
    public class CustomMapRenderer : MapRenderer, GoogleMap.IInfoWindowAdapter
    {
        List<CustomPin> customPins;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                NativeMap.InfoWindowClick -= OnInfoWindowClick;
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                customPins = formsMap.CustomPins;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(GoogleMap map)
        {
            base.OnMapReady(map);

            NativeMap.InfoWindowClick += OnInfoWindowClick;
            NativeMap.SetInfoWindowAdapter(this);
        }
        ...
    }
}
```

사용자 지정 렌더러 새 Xamarin.Forms 요소에 연결 되어 있는 경우는 `OnElementChanged` 메서드 호출의 `MapView.GetMapAsync` 메서드를 기본 `GoogleMap` 보기에 연결 하는 합니다. 한 번의 `GoogleMap` 인스턴스를 사용할 수는 `OnMapReady` 재정의 호출 됩니다. 에 대 한 이벤트 처리기를 등록 하는이 메서드는 `InfoWindowClick` 때 발생 하는 이벤트는 [정보 창의 클릭할](#Clicking_on_the_Info_Window), 렌더러 요소 변경 내용에 연결 된 경우에에서 구독 취소 되 고 합니다. `OnMapReady` 호출 또한 재정의 `SetInfoWindowAdapter` 지정 하는 메서드는 `CustomMapRenderer` 클래스 인스턴스 정보 창의 사용자 지정 하는 메서드를 제공 합니다.

`CustomMapRenderer` 클래스가 구현 하는 `GoogleMap.IInfoWindowAdapter` 인터페이스 [정보 창의 사용자 지정](#Customizing_the_Info_Window)합니다. 이 인터페이스는 다음 메서드를 구현 되어야 합니다를 지정 합니다.

- `public Android.Views.View GetInfoWindow(Marker marker)` –이 메서드는 표식에 대 한 사용자 지정 정보 창을 반환할 호출 됩니다. 반환 하는 경우 `null`, 기본 창 렌더링이 사용 됩니다. 반환 하는 경우는 `View`, 하 한 다음 `View` 정보 창 프레임 내에 배치 되도록 합니다.
- `public Android.Views.View GetInfoContents(Marker marker)` –이 메서드를 호출 반환 하는 `View` 정보 창의 내용을 포함 하 고 경우에 호출 됩니다는 `GetInfoWindow` 메서드 반환 `null`합니다. 반환 하는 경우 `null`, 정보 창 콘텐츠의 기본 렌더링이 사용 됩니다.

샘플 응용 프로그램에서 정보 창의 콘텐츠를 사용자 지정한는 `GetInfoWindow` 메서드 반환 `null` 이 사용할 수 있도록 합니다.

#### <a name="customizing-the-marker"></a>마커를 사용자 지정

표식을 나타내는 데 사용 되는 아이콘을 호출 하 여 사용자 지정할 수 있습니다는 `MarkerOptions.SetIcon` 메서드. 재정의 하 여이 작업을 수행할 수 있습니다는 `CreateMarker` 메서드 각각에 대해 호출 되는 `Pin` 지도에 추가 된:

```csharp
protected override MarkerOptions CreateMarker(Pin pin)
{
    var marker = new MarkerOptions();
    marker.SetPosition(new LatLng(pin.Position.Latitude, pin.Position.Longitude));
    marker.SetTitle(pin.Label);
    marker.SetSnippet(pin.Address);
    marker.SetIcon(BitmapDescriptorFactory.FromResource(Resource.Drawable.pin));
    return marker;
}
```

이 메서드가 만드는 새 `MarkerOption` 인스턴스 각각에 대해 `Pin` 인스턴스. 위치, 레이블 및 표식의 주소를 설정한 후 해당 아이콘으로 설정 된는 `SetIcon` 메서드. 이 메서드는 `BitmapDescriptor` 아이콘을 사용 하 여 렌더링 하는 데 필요한 데이터가 포함 된 개체는 `BitmapDescriptorFactory` 만들기 과정을 간소화 하는 도우미 메서드를 제공 하는 클래스는 `BitmapDescriptor`합니다.

사용 하는 방법에 대 한 자세한 내용은 `BitmapDescriptorFactory` 마커를 사용자 지정 참조 하는 클래스 [표식을 사용자 지정](~/android/platform/maps-and-location/maps/maps-api.md)합니다.

<a name="Customizing_the_Info_Window" />

#### <a name="customizing-the-info-window"></a>사용자 정의 정보 창

사용자가 표식의 누를 때는 `GetInfoContents` 메서드를 실행 하는 `GetInfoWindow` 메서드 반환 `null`합니다. 다음 코드 예제는 `GetInfoContents` 메서드:

```csharp
public Android.Views.View GetInfoContents (Marker marker)
{
  var inflater = Android.App.Application.Context.GetSystemService (Context.LayoutInflaterService) as Android.Views.LayoutInflater;
  if (inflater != null) {
    Android.Views.View view;

    var customPin = GetCustomPin (marker);
    if (customPin == null) {
      throw new Exception ("Custom pin not found");
    }

    if (customPin.Id.ToString() == "Xamarin") {
      view = inflater.Inflate (Resource.Layout.XamarinMapInfoWindow, null);
    } else {
      view = inflater.Inflate (Resource.Layout.MapInfoWindow, null);
    }

    var infoTitle = view.FindViewById<TextView> (Resource.Id.InfoWindowTitle);
    var infoSubtitle = view.FindViewById<TextView> (Resource.Id.InfoWindowSubtitle);

    if (infoTitle != null) {
      infoTitle.Text = marker.Title;
    }
    if (infoSubtitle != null) {
      infoSubtitle.Text = marker.Snippet;
    }

    return view;
  }
  return null;
}
```

이 메서드는 반환 된 `View` 정보 창의 내용을 포함 합니다. 이 작업은 다음과 같이 수행 됩니다.

- A `LayoutInflater` 인스턴스를 검색 합니다. 이것은 인스턴스화하는 데 사용할 레이아웃 XML 파일을 그에 해당 `View`합니다.
- `GetCustomPin` 메서드는 정보 창에 대 한 사용자 지정 pin 데이터를 반환 합니다.
- `XamarinMapInfoWindow` 경우 레이아웃을 확장 된 `CustomPin.Id` 속성이 `Xamarin`합니다. 그렇지 않은 경우는 `MapInfoWindow` 레이아웃을 확장 합니다. 따라서 여러 가지 표식에 대 한 다른 정보 창 레이아웃 표시 될 수 있는 시나리오 수 있습니다.
- `InfoWindowTitle` 및 `InfoWindowSubtitle` 높여서 레이아웃에서 리소스를 검색 및 해당 `Text` 속성에서 해당 데이터에 설정 됩니다는 `Marker` 인스턴스를 해당 하는 리소스 하지 않는 `null`합니다.
- `View` 지도에 표시 하기 위해 인스턴스가 반환 됩니다.

> [!NOTE]
> 정보 창에는 라이브 아닙니다 `View`합니다. Android는 변환 하는 대신,는 `View` 정적 비트맵을 하는 이미지 형식으로 표시 합니다. 이 즉, 정보 창의 하는 동안 터치 이벤트 또는 제스처에 응답할 수 없습니다 및 정보 창에 있는 개별 컨트롤 자체에 응답할 수 없으므로 click 이벤트에 응답할 수 있는 이벤트를 클릭 합니다.

<a name="Clicking_on_the_Info_Window" />

#### <a name="clicking-on-the-info-window"></a>정보 창에서 클릭합니다.

정보 창에는 사용자가 클릭할 때는 `InfoWindowClick` 차례로 실행 되는 이벤트 발생은 `OnInfoWindowClick` 메서드:

```csharp
void OnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
  var customPin = GetCustomPin (e.Marker);
  if (customPin == null) {
    throw new Exception ("Custom pin not found");
  }

  if (!string.IsNullOrWhiteSpace (customPin.Url)) {
    var url = Android.Net.Uri.Parse (customPin.Url);
    var intent = new Intent (Intent.ActionView, url);
    intent.AddFlags (ActivityFlags.NewTask);
    Android.App.Application.Context.StartActivity (intent);
  }
}
```

이 메서드는 웹 브라우저를 열고에 저장 된 주소로 이동는 `Url` 검색 된 속성 `CustomPin` 에 대 한 인스턴스는 `Marker`합니다. 에 주소를 만들 때 정의 된는 `CustomPin` PCL 프로젝트에는 컬렉션입니다.

사용자 지정 하는 방법에 대 한 자세한 내용은 `MapView` 인스턴스를 참조 [지도 API](~/android/platform/maps-and-location/maps/maps-api.md)합니다.

### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>유니버설 Windows 플랫폼에 사용자 지정 렌더러 만들기

다음 스크린샷에서 전과 후 사용자 지정 지도 보여 줍니다.

![](customized-pin-images/map-layout-uwp.png "지도 컨트롤 전과 후 사용자 지정")

UWP에서 pin 라고는 *맵 아이콘*, 사용자 지정 이미지 또는 시스템 정의 기본 이미지 수입니다. 맵 아이콘을 표시할 수 있습니다는 `UserControl`, 지도 아이콘에서 누르기 사용자에 대 한 응답에 표시 되는 합니다. `UserControl` 모든 콘텐츠를 표시할 수 포함 하는 `Label` 및 `Address` 의 속성은 `Pin` 인스턴스.

다음 코드 예제에서는 UWP 사용자 지정 렌더러를 보여 줍니다.

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
        MapControl nativeMap;
        List<CustomPin> customPins;
        XamarinMapOverlay mapOverlay;
        bool xamarinOverlayShown = false;

        protected override void OnElementChanged(ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                nativeMap.MapElementClick -= OnMapElementClick;
                nativeMap.Children.Clear();
                mapOverlay = null;
                nativeMap = null;
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                nativeMap = Control as MapControl;
                customPins = formsMap.CustomPins;

                nativeMap.Children.Clear();
                nativeMap.MapElementClick += OnMapElementClick;

                foreach (var pin in customPins)
                {
                    var snPosition = new BasicGeoposition { Latitude = pin.Pin.Position.Latitude, Longitude = pin.Pin.Position.Longitude };
                    var snPoint = new Geopoint(snPosition);

                    var mapIcon = new MapIcon();
                    mapIcon.Image = RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///pin.png"));
                    mapIcon.CollisionBehaviorDesired = MapElementCollisionBehavior.RemainVisible;
                    mapIcon.Location = snPoint;
                    mapIcon.NormalizedAnchorPoint = new Windows.Foundation.Point(0.5, 1.0);

                    nativeMap.MapElements.Add(mapIcon);                    
                }
            }
        }
        ...
    }
}
```

`OnElementChanged` 메서드는 사용자 지정 렌더러 새 Xamarin.Forms 요소에 연결 되어 있는 경우 다음 작업을 수행 합니다.

- 지웁니다는 `MapControl.Children` 맵에서 대 한 이벤트 처리기를 등록 하기 전에 모든 기존 사용자 인터페이스 요소를 제거 하려면 컬렉션에서 `MapElementClick` 이벤트입니다. 이 이벤트는 사용자가 탭 하거나 클릭할 때 발생 한 `MapElement` 에 `MapControl`, 렌더러 요소 변경 내용에 연결 된 경우에에서 구독 취소 되 고 합니다.
- 각 핀의 `customPins` 컬렉션 올바른 지도에 지리적 위치에 다음과 같이 표시 됩니다.
  - 위치에 pin이 생성 한 `Geopoint` 인스턴스.
  - A `MapIcon` 인스턴스 pin을 나타내기 위해 생성 됩니다.
  - 나타내는 데 사용 되는 이미지는 `MapIcon` 설정 하 여 지정 된 `MapIcon.Image` 속성입니다. 그러나 지도 아이콘 이미지는 하지 항상 보장를 표시 해야 다른 맵의 요소에 의해 가려질 수 있습니다. 따라서 지도 아이콘의 `CollisionBehaviorDesired` 속성이 `MapElementCollisionBehavior.RemainVisible`, 계속 표시 되도록 합니다.
  - 위치는 `MapIcon` 설정 하 여 지정 된 `MapIcon.Location` 속성입니다.
  - `MapIcon.NormalizedAnchorPoint` 이미지에 대 한 포인터의 대략적인 위치에 속성이 설정 되어 있습니다. 이 속성의 값을 기본값 (0, 0) 나타내는 이미지의 왼쪽된 위 모퉁이 유지 하는 경우 지도 확대/축소 수준의 변경 내용을 다른 위치를 가리키는 이미지 될 수 있습니다.
  - `MapIcon` 인스턴스가에 추가 되는 `MapControl.MapElements` 컬렉션입니다. 이 인해 맵 아이콘에 표시 되는 `MapControl`합니다.

> [!NOTE]
> 여러 지도 아이콘에 대 한 동일한 이미지를 사용 하는 경우는 `RandomAccessStreamReference` 최상의 성능을 위해 인스턴스를 응용 프로그램 또는 페이지 수준에서 선언 해야 합니다.

#### <a name="displaying-the-usercontrol"></a>UserControl 표시

사용자가 지도 아이콘을 누르면는 `OnMapElementClick` 메서드를 실행 합니다. 다음 코드 예제에서는이 메서드를 보여 줍니다.

```csharp
private void OnMapElementClick(MapControl sender, MapElementClickEventArgs args)
{
    var mapIcon = args.MapElements.FirstOrDefault(x => x is MapIcon) as MapIcon;
    if (mapIcon != null)
    {
        if (!xamarinOverlayShown)
        {
            var customPin = GetCustomPin(mapIcon.Location.Position);
            if (customPin == null)
            {
                throw new Exception("Custom pin not found");
            }

            if (customPin.Id.ToString() == "Xamarin")
            {
                if (mapOverlay == null)
                {
                    mapOverlay = new XamarinMapOverlay(customPin);
                }

                var snPosition = new BasicGeoposition { Latitude = customPin.Pin.Position.Latitude, Longitude = customPin.Pin.Position.Longitude };
                var snPoint = new Geopoint(snPosition);

                nativeMap.Children.Add(mapOverlay);
                MapControl.SetLocation(mapOverlay, snPoint);
                MapControl.SetNormalizedAnchorPoint(mapOverlay, new Windows.Foundation.Point(0.5, 1.0));
                xamarinOverlayShown = true;
            }
        }
        else
        {
            nativeMap.Children.Remove(mapOverlay);
            xamarinOverlayShown = false;
        }
    }
}
```

이 메서드가 만드는 `UserControl` pin에 대 한 정보를 표시 하는 인스턴스. 이 작업은 다음과 같이 수행 됩니다.

- `MapIcon` 인스턴스를 검색 합니다.
- `GetCustomPin` 메서드는 표시 되는 사용자 지정 pin 데이터를 반환 합니다.
- A `XamarinMapOverlay` pin 사용자 지정 데이터를 표시 하려면 인스턴스가 만들어집니다. 이 클래스는 사용자 정의 컨트롤입니다.
- 지리적 위치를 표시할 위치의 `XamarinMapOverlay` 인스턴스의 `MapControl` 로 만들어집니다는 `Geopoint` 인스턴스.
- `XamarinMapOverlay` 인스턴스가에 추가 되는 `MapControl.Children` 컬렉션입니다. 이 컬렉션 지도에 표시 되는 XAML 사용자 인터페이스 요소를 포함 합니다.
- 지리적 위치는 `XamarinMapOverlay` 지도에 인스턴스를 호출 하 여 설정한는 `SetLocation` 메서드.
- 상대 위치는 `XamarinMapOverlay` 지정된 된 위치에 해당 하는 인스턴스를 호출 하 여 설정 됩니다는 `SetNormalizedAnchorPoint` 메서드. 이렇게 하면에서 지도 결과의 확대/축소 수준을 변경 하는 `XamarinMapOverlay` 항상 올바른 위치에 표시 되 고 인스턴스.

또는 pin에 대 한 정보는 이미 지도에 표시 되 고 지도에 누르기 제거는 `XamarinMapOverlay` 에서 인스턴스는 `MapControl.Children` 컬렉션입니다.

#### <a name="tapping-on-the-information-button"></a>정보 단추 누르기

사용자가 누를 때는 *정보* 단추는 `XamarinMapOverlay` 사용자 정의 컨트롤의 `Tapped` 차례로 실행 되는 이벤트 발생은 `OnInfoButtonTapped` 메서드:

```csharp
private async void OnInfoButtonTapped(object sender, TappedRoutedEventArgs e)
{
    await Launcher.LaunchUriAsync(new Uri(customPin.Url));
}
```

이 메서드는 웹 브라우저를 열고에 저장 된 주소로 이동는 `Url` 의 속성은 `CustomPin` 인스턴스. 에 주소를 만들 때 정의 된는 `CustomPin` PCL 프로젝트에는 컬렉션입니다.

사용자 지정 하는 방법에 대 한 자세한 내용은 `MapControl` 인스턴스를 참조 [지도 위치 개요](https://msdn.microsoft.com/library/windows/apps/mt219699.aspx) msdn 합니다.

## <a name="summary"></a>요약

이 문서에 대 한 사용자 지정 렌더러를 만드는 방법에 명시 된 `Map` 컨트롤 개발자가 자신의 플랫폼 관련 사용자 지정과 기본 네이티브 렌더링을 재정의할 수 있도록 합니다. Xamarin.Forms.Maps는 사용자에 대 한 네이티브 맵의 환경을 신속 하 고 친숙 한 지도 제공 하는 각 플랫폼에서 Api를 사용 하는 맵을 표시 하기 위한 플랫폼 독립적인 추상화를 제공 합니다.


## <a name="related-links"></a>관련 링크

- [지도 컨트롤](~/xamarin-forms/user-interface/map.md)
- [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md)
- [지도 API](~/android/platform/maps-and-location/maps/maps-api.md)
- [사용자 지정 된 Pin (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/pin/)
