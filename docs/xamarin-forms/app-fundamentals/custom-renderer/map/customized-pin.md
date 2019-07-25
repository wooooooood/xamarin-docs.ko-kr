---
title: 지도 핀 사용자 지정
description: 이 문서에서는 각 플랫폼에서 사용자 지정된 핀과 사용자 지정된 핀 데이터 보기가 있는 네이티브 맵을 나타내는 맵 컨트롤에 대한 사용자 지정 렌더러를 만드는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: C5481D86-80E9-4E3D-9FB6-57B0F93711A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 8e80016e33e8bebba715be4f02060e76086884fc
ms.sourcegitcommit: 4b6e832d1db5616b657dc8540da67c509b28dc1d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68386201"
---
# <a name="customizing-a-map-pin"></a>지도 핀 사용자 지정

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/CustomRenderers/Map/Pin/)

_이 문서에서는 각 플랫폼에서 사용자 지정된 핀과 사용자 지정된 핀 데이터 보기가 있는 네이티브 맵을 나타내는 맵 컨트롤에 대한 사용자 지정 렌더러를 만드는 방법을 설명합니다._

모든 Xamarin.Forms 보기에는 네이티브 컨트롤의 인스턴스를 만드는 각 플랫폼에 대해 함께 제공되는 렌더러가 있습니다. iOS의 Xamarin.Forms 애플리케이션에서 [`Map`](xref:Xamarin.Forms.Maps.Map)를 렌더링하면 `MapRenderer` 클래스가 인스턴스화되며, 차례로 네이티브 `MKMapView` 컨트롤이 인스턴스화됩니다. Android 플랫폼에서 `MapRenderer` 클래스는 네이티브 `MapView` 컨트롤을 인스턴스화합니다. UWP(유니버설 Windows 플랫폼)에서 `MapRenderer` 클래스는 네이티브 `MapControl`을 인스턴스화합니다. Xamarin.Forms 컨트롤에 매핑되는 렌더러 및 네이티브 컨트롤 클래스에 대한 자세한 내용은 [렌더러 기본 클래스 및 네이티브 컨트롤](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)을 참조하세요.

다음 다이어그램은 [`Map`](xref:Xamarin.Forms.Maps.Map) 및 이를 구현하는 해당 네이티브 컨트롤 간의 관계를 보여줍니다.

![](customized-pin-images/map-classes.png "맵 컨트롤과 네이티브 컨트롤 구현 간의 관계")

렌더링 프로세스는 각 플랫폼에서 [`Map`](xref:Xamarin.Forms.Maps.Map)에 대한 사용자 지정 렌더러를 만들어 플랫폼별 사용자 지정을 구현하는 데 사용할 수 있습니다. 이 작업을 수행하는 프로세스는 다음과 같습니다.

1. Xamarin.Forms 사용자 지정 맵을 [만듭니다](#Creating_the_Custom_Map).
1. Xamarin.Forms에서 사용자 지정 맵을 [사용합니다](#Consuming_the_Custom_Map).
1. 각 플랫폼의 맵에 대한 사용자 지정 렌더러를 [만듭니다](#Creating_the_Custom_Renderer_on_each_Platform).

이제 각 항목을 차례로 설명하여, 사용자 지정된 핀이 있는 네이티브 맵과 각 플랫폼에 있는 핀 데이터의 사용자 지정된 보기를 표시하는 `CustomMap` 랜더러를 구현합니다.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps)는 사용 전에 초기화되고 구성되어야 합니다. 자세한 내용은 [`Maps Control`](~/xamarin-forms/user-interface/map.md)를 참조하세요.

<a name="Creating_the_Custom_Map" />

## <a name="creating-the-custom-map"></a>사용자 지정 맵 만들기

다음 코드 예제와 같이 [`Map`](xref:Xamarin.Forms.Maps.Map) 클래스를 서브클래싱하여 사용자 지정 맵 컨트롤을 만들 수 있습니다.

```csharp
public class CustomMap : Map
{
  public List<CustomPin> CustomPins { get; set; }
}
```

`CustomMap` 컨트롤은 .NET Standard 라이브러리 프로젝트에서 생성되고 사용자 지정 맵에 대한 API를 정의합니다. 사용자 지정 맵은 각 플랫폼의 네이티브 맵 컨트롤을 통해 랜더링될 `CustomPin` 개체의 컬렉션을 나타내는 `CustomPins` 속성을 노출합니다. `CustomPin` 클래스는 다음 코드 예제에 나와 있습니다.

```csharp
public class CustomPin : Pin
{
  public string Url { get; set; }
}
```

이 클래스는 `CustomPin`을 [`Pin`](xref:Xamarin.Forms.Maps.Pin) 클래스의 속성으로 상속하고 `Url` 속성을 추가하는 것으로 정의합니다.

<a name="Consuming_the_Custom_Map" />

## <a name="consuming-the-custom-map"></a>사용자 지정 맵 사용

`CustomMap` 컨트롤은 해당 위치의 네임스페이스를 선언하고 사용자 지정 맵 컨트롤의 네임스페이스 접두사를 사용하여 .NET Standard 라이브러리 프로젝트의 XAML에서 참조할 수 있습니다. 다음 코드 예제에서는 `CustomMap` 컨트롤을 XAML 페이지에서 사용하는 방법을 보여줍니다.

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

`local` 네임스페이스 접두사는 원하는 것으로 이름을 지정할 수 있습니다. 그러나 `clr-namespace` 및 `assembly` 값은 사용자 지정 맵의 세부 정보와 일치해야 합니다. 네임스페이스가 선언되면 사용자 지정 맵을 참조하는 데 사용됩니다.

다음 코드 예제에서는 C# 페이지에서 `CustomMap` 컨트롤을 사용하는 방법을 보여줍니다.

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

`CustomMap` 인스턴스는 각 플랫폼에 네이티브 맵을 표시하는 데 사용됩니다. [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) 속성은 [`MapType`](xref:Xamarin.Forms.Maps.MapType) 열거형에 정의된 가능한 값을 사용하여 [`Map`](xref:Xamarin.Forms.Maps.Map)의 표시 스타일을 설정합니다. iOS 및 Android의 경우 맵의 너비와 높이는 플랫폼별 프로젝트에서 초기화되는 `App` 클래스의 속성을 통해 설정됩니다.

맵의 위치 및 맵에 포함된 핀은 다음 코드 예제와 같이 초기화됩니다.

```csharp
public MapPage ()
{
  ...
  var pin = new CustomPin {
    Type = PinType.Place,
    Position = new Position (37.79752, -122.40183),
    Label = "Xamarin San Francisco Office",
    Address = "394 Pacific Ave, San Francisco CA",
    MarkerId = "Xamarin",
    Url = "http://xamarin.com/about/"
  };

  customMap.CustomPins = new List<CustomPin> { pin };
  customMap.Pins.Add (pin);
  customMap.MoveToRegion (MapSpan.FromCenterAndRadius (
    new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
}
```

이 초기화에서는 사용자 지정 핀을 추가하고 [`Position`](xref:Xamarin.Forms.Maps.Position) 및 [`Distance`](xref:Xamarin.Forms.Maps.Distance)에서 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)을 생성하여 맵의 위치와 확대/축소 수준을 변경하는 [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) 메서드를 사용하여 맵 보기를 배치합니다.

이제 각 애플리케이션 프로젝트에 사용자 지정 렌더러를 추가하여 네이티브 맵 컨트롤을 사용자 지정할 수 있습니다.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>각 플랫폼에서 사용자 지정 렌더러 만들기

사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. 사용자 지정 맵을 렌더링하는 `MapRenderer` 클래스의 서브클래스를 만듭니다.
1. 사용자 지정 맵을 렌더링하고 이를 사용자 지정하기 위한 논리를 작성하는 `OnElementChanged` 메서드를 재정의합니다. 이 메서드는 해당 Xamarin.Forms 사용자 지정 맵이 생성될 때 호출됩니다.
1. 사용자 지정 렌더러 클래스에 `ExportRenderer` 특성을 추가하여 Xamarin.Forms 사용자 지정 맵을 렌더링하는 데 사용하도록 지정합니다. 이 특성은 사용자 지정 랜더러를 Xamarin.Forms에 등록하는 데 사용됩니다.

> [!NOTE]
> 각 플랫폼 프로젝트에서 사용자 지정 렌더러를 제공하는 것은 선택 사항입니다. 사용자 지정 렌더러가 등록되지 않은 경우 컨트롤의 기본 클래스에 대한 기본 렌더러가 사용됩니다.

다음 다이어그램은 샘플 애플리케이션에서 각 프로젝트의 책임과 이들 간의 관계를 보여줍니다.

![](customized-pin-images/solution-structure.png "CustomMap 사용자 지정 렌더러 프로젝트 책임")

`CustomMap` 컨트롤은 각 플랫폼의 `MapRenderer` 클래스에서 파생되는 플랫폼별 렌더러 클래스에 의해 렌더링됩니다. 그러면 다음 스크린샷과 같이 각 `CustomMap` 컨트롤이 플랫폼별 컨트롤로 렌더링됩니다.

![](customized-pin-images/screenshots.png "각 플랫폼의 CustomMap")

`MapRenderer` 클래스는 해당 네이티브 컨트롤을 렌더링하기 위해 Xamarin.Forms 사용자 지정 맵이 생성될 때 호출되는 `OnElementChanged` 메서드를 노출합니다. 이 메서드는 `OldElement` 및 `NewElement` 속성이 포함된 `ElementChangedEventArgs` 매개 변수를 가져옵니다. 이러한 속성은 랜더러가 연결*된* Xamarin.Forms 요소와 렌더러가 연결*되는* Xamarin.Forms 요소를 각각 나타냅니다. 샘플 애플리케이션에서 `OldElement` 속성은 `null`이고, `NewElement` 속성은 `CustomMap` 인스턴스에 대한 참조를 포함합니다.

각 플랫폼별 렌더러 클래스에서 `OnElementChanged` 메서드의 재정의된 버전은 네이티브 컨트롤 사용자 지정을 수행하는 위치입니다. 플랫폼에서 사용되는 네이티브 컨트롤에 대한 형식화된 참조는 `Control` 속성을 통해 액세스할 수 있습니다. 또한 랜더링되는 Xamarin.Forms 컨트롤에 대한 참조는 `Element` 속성을 통해 얻을 수 있습니다.

`OnElementChanged` 메서드에서 이벤트 처리기를 구독할 때는 다음 코드 예제와 같이 주의해야 합니다.

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.View> e)
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

네이티브 컨트롤을 구성하고 사용자 지정 렌터러가 새 Xamarin.Forms 요소에 연결된 경우에만 이벤트 처리기를 구독해야 합니다. 마찬가지로, 구독된 모든 이벤트 처리기는 렌더러가 연결된 요소가 변경될 때만 구독을 취소해야 합니다. 이러한 방식을 채택하면 메모리 누수가 없는 사용자 지정 렌더러를 만들 수 있습니다.

각 사용자 지정 렌더러 클래스는 랜더러를 Xamarin.Forms에 등록하는 `ExportRenderer` 속성으로 데코레이트됩니다. 이 특성은 렌더링되는 Xamarin.Forms 사용자 지정 컨트롤의 형식 이름과 지정 렌더러의 형식 이름이라는 두 가지 매개 변수를 사용합니다. 특성의 `assembly` 접두사는 특성이 전체 어셈블리에 적용되도록 지정합니다.

다음 섹션에서는 각 플랫폼별 사용자 지정 렌더러 클래스의 구현을 설명합니다.

### <a name="creating-the-custom-renderer-on-ios"></a>iOS에서 사용자 지정 렌더러 만들기

다음 스크린샷은 사용자 지정 전후의 맵을 보여줍니다.

![](customized-pin-images/map-layout-ios.png "사용자 지정 전후의 맵 컨트롤")

iOS에서 핀을 *주석*이라고 하며, 사용자 지정 이미지 또는 다양한 색의 시스템 정의 핀일 수 있습니다. 주석은 사용자가 주석을 선택하는 것에 응답하여 표시되는 *설명선*을 선택적으로 표시할 수 있습니다. 설명선에는 `Pin` 인스턴스의 `Label` 및 `Address` 속성이 표시되며 선택적으로 왼쪽 및 오른쪽 액세서리 보기가 표시됩니다. 위의 스크린샷에서 왼쪽 액세서리 보기는 monkey의 이미지이며 오른쪽 액세서리 보기는 *정보* 단추입니다.

다음 코드 예제에서는 iOS 플랫폼용 사용자 지정 렌더러를 보여줍니다.

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

사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결되어 있는 경우 `OnElementChanged` 메서드는 [`MKMapView`](xref:MapKit.MKMapView) 인스턴스의 다음 구성을 수행합니다.

- [`GetViewForAnnotation`](xref:MapKit.MKMapView.GetViewForAnnotation*) 속성은 `GetViewForAnnotation` 메서드로 설정됩니다. 이 메서드는 [주석의 위치가 맵에 표시](#Displaying_the_Annotation)될 때 호출되고 표시 전에 주석을 사용자 지정하는 데 사용됩니다.
- `CalloutAccessoryControlTapped`, `DidSelectAnnotationView` 및 `DidDeselectAnnotationView` 이벤트에 대한 이벤트 처리기가 등록됩니다. 이러한 이벤트는 사용자가 [설명선에 오른쪽 액세서리를 태핑](#Tapping_on_the_Right_Callout_Accessory_View)하고 사용자가 주석을 각각 [선택](#Selecting_the_Annotation) 및 [선택 취소](#Deselecting_the_Annotation)할 때 해제됩니다. 이벤트는 렌더러가 변경 내용에 연결된 요소에서만 등록이 취소됩니다.

<a name="Displaying_the_Annotation" />

#### <a name="displaying-the-annotation"></a>주석 표시

`GetViewForAnnotation` 메서드는 주석의 위치가 맵에 표시될 때 호출되고 표시 전에 주석을 사용자 지정하는 데 사용됩니다. 주석에는 두 부분이 있습니다.

- `MkAnnotation` – 주석의 제목, 부제목 및 위치를 포함합니다.
- `MkAnnotationView` – 주석을 나타내는 이미지 및 사용자가 주석을 태핑할 때 표시되는 설명선(필요에 따라)을 포함합니다.

`GetViewForAnnotation` 메서드는 주석 데이터를 포함하는 `IMKAnnotation`을 허용하고 맵에 표시할 `MKAnnotationView`를 반환하며 맵에 표시하기 위해 다음 코드 예제에 표시됩니다.

```csharp
protected override MKAnnotationView GetViewForAnnotation(MKMapView mapView, IMKAnnotation annotation)
{
    MKAnnotationView annotationView = null;

    if (annotation is MKUserLocation)
        return null;

    var customPin = GetCustomPin(annotation as MKPointAnnotation);
    if (customPin == null) {
        throw new Exception("Custom pin not found");
    }

    annotationView = mapView.DequeueReusableAnnotation(customPin.MarkerId.ToString());
    if (annotationView == null) {
        annotationView = new CustomMKAnnotationView(annotation, customPin.MarkerId.ToString());
        annotationView.Image = UIImage.FromFile("pin.png");
        annotationView.CalloutOffset = new CGPoint(0, 0);
        annotationView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile("monkey.png"));
        annotationView.RightCalloutAccessoryView = UIButton.FromType(UIButtonType.DetailDisclosure);
        ((CustomMKAnnotationView)annotationView).MarkerId = customPin.MarkerId.ToString();
        ((CustomMKAnnotationView)annotationView).Url = customPin.Url;
    }
    annotationView.CanShowCallout = true;

    return annotationView;
}
```

이 메서드를 사용하면 주석이 시스템 정의 핀이 아닌 사용자 지정이미지로 표시되도록 하고, 주석을 누르면 주석 제목과 주소의 왼쪽과 오른쪽에 추가 내용이 포함된 설명선이 표시됩니다. 이는 다음과 같이 수행됩니다.

1. 주석에 대한 사용자 지정 핀 데이터를 반환하기 위해 `GetCustomPin` 메서드가 호출됩니다.
1. 메모리를 절약하기 위해 주석의 보기는 [`DequeueReusableAnnotation`](xref:MapKit.MKMapView.DequeueReusableAnnotation*)을 호출하여 다시 사용할 수 있도록 풀링됩니다.
1. `CustomMKAnnotationView` 클래스는 `CustomPin` 인스턴스의 동일한 속성에 해당하는 `MarkerId` 및 `Url` 속성으로 `MKAnnotationView` 클래스를 확장합니다. 주석이 `null`인 경우 `CustomMKAnnotationView`의 새 인스턴스가 생성됩니다.
    - `CustomMKAnnotationView.Image` 속성이 맵에서 주석을 나타내는 이미지로 설정됩니다.
    - `CustomMKAnnotationView.CalloutOffset` 속성은 설명선이 주석 위 중앙에 위치하도록 지정하는 `CGPoint`로 설정됩니다.
    - `CustomMKAnnotationView.LeftCalloutAccessoryView` 속성은 주석 제목 및 주소 왼쪽에 표시되는 monkey 이미지로 설정됩니다.
    - `CustomMKAnnotationView.RightCalloutAccessoryView` 속성은 주석 제목 및 주소 오른쪽에 표시되는 *정보* 단추로 설정됩니다.
    - `CustomMKAnnotationView.MarkerId` 속성은 `GetCustomPin` 메서드에서 반환된 `CustomPin.MarkerId` 속성으로 설정됩니다. 이렇게 하면 주석을 식별할 수 있으므로, 원하는 경우 [설명선을 추가로 사용자 지정할 수 있습니다](#Selecting_the_Annotation).
    - `CustomMKAnnotationView.Url` 속성은 `GetCustomPin` 메서드에서 반환된 `CustomPin.Url` 속성으로 설정됩니다. 사용자가 [오른쪽 설명선 액세서리 보기에 표시된 단추를 누를](#Tapping_on_the_Right_Callout_Accessory_View) 때 URL이 탐색됩니다.
1. 주석을 누를 때 설명선이 표시되도록 [`MKAnnotationView.CanShowCallout`](xref:MapKit.MKAnnotationView.CanShowCallout*) 속성이 `true`로 설정됩니다.
1. 주석이 맵에 표시하기 위해 반환됩니다.

<a name="Selecting_the_Annotation" />

#### <a name="selecting-the-annotation"></a>주석 선택

사용자가 주석을 누르면 `DidSelectAnnotationView` 이벤트가 발생하고, 차례로 `OnDidSelectAnnotationView` 메서드가 실행됩니다.

```csharp
void OnDidSelectAnnotationView (object sender, MKAnnotationViewEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  customPinView = new UIView ();

  if (customView.MarkerId == "Xamarin") {
    customPinView.Frame = new CGRect (0, 0, 200, 84);
    var image = new UIImageView (new CGRect (0, 0, 200, 84));
    image.Image = UIImage.FromFile ("xamarin.png");
    customPinView.AddSubview (image);
    customPinView.Center = new CGPoint (0, -(e.View.Frame.Height + 75));
    e.View.AddSubview (customPinView);
  }
}
```

이 메서드는 선택한 주석의 `MarkerId` 속성이 `Xamarin`으로 설정된 경우 Xamarin 로고 이미지를 포함하는 `UIView` 인스턴스를 추가하여 기존 설명선(왼쪽 및 오른쪽 액세서리 보기 포함)을 확장합니다. 이렇게 하면 여러 주석에 대해 다양한 설명선을 표시할 수 있는 시나리오를 허용합니다. `UIView` 인스턴스는 기존 설명선 위 가운데에 표시됩니다.

<a name="Tapping_on_the_Right_Callout_Accessory_View" />

#### <a name="tapping-on-the-right-callout-accessory-view"></a>오른쪽 설명선 액세서리 보기 태핑

사용자가 오른쪽 설명선 액세서리 보기의 *정보* 단추를 누르면 `CalloutAccessoryControlTapped` 이벤트가 발생하며, 차례로 `OnCalloutAccessoryControlTapped` 메서드가 실행됩니다.

```csharp
void OnCalloutAccessoryControlTapped (object sender, MKMapViewAccessoryTappedEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  if (!string.IsNullOrWhiteSpace (customView.Url)) {
    UIApplication.SharedApplication.OpenUrl (new Foundation.NSUrl (customView.Url));
  }
}
```

이 메서드는 웹 브라우저를 열고 `CustomMKAnnotationView.Url` 속성에 저장된 주소로 이동합니다. .NET Standard 라이브러리 프로젝트에서 `CustomPin` 컬렉션을 만들 때 주소가 정의되었습니다.

<a name="Deselecting_the_Annotation" />

#### <a name="deselecting-the-annotation"></a>주석 취소

주석이 표시되고 사용자가 맵을 누르면 `DidDeselectAnnotationView` 이벤트가 발생하고, 차례로 `OnDidDeselectAnnotationView` 메서드가 실행됩니다.

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

이 메서드를 사용하면 기존 설명선을 선택하지 않은 경우 설명선의 확장된 부분(Xamarin 로고 이미지)도 표시되는 것을 중지하고 해당 리소스가 해제됩니다.

`MKMapView` 인스턴스 사용자 지정에 대한 자세한 내용은 [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md)를 참조하세요.

### <a name="creating-the-custom-renderer-on-android"></a>Android에서 사용자 지정 렌더러 만들기

다음 스크린샷은 사용자 지정 전후의 맵을 보여줍니다.

![](customized-pin-images/map-layout-android.png "사용자 지정 전후의 맵 컨트롤")

Android에서 핀을 *표식*이라고 하며, 사용자 지정 이미지 또는 다양한 색의 시스템 정의 표식일 수 있습니다. 표식은 사용자가 표식을 탭핑하는 것에 응답하여 나타내는 *정보 창*을 표시할 수 있습니다. 정보 창에는 `Pin` 인스턴스의 `Label` 및 `Address` 속성이 표시되고 다른 콘텐츠를 포함하도록 지정할 수 있습니다. 그러나 한 번에 하나의 정보 창만 표시할 수 있습니다.

다음 코드 예제에서는 Android 플랫폼용 사용자 지정 렌더러를 보여줍니다.

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

사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결되어 있는 경우 `OnElementChanged` 메서드는 `MapView.GetMapAsync` 메서드를 호출하여 보기에 연결된 내부 `GoogleMap`을 가져옵니다. `GoogleMap` 인스턴스를 사용할 수 있게 되면 `OnMapReady` 재정의가 호출됩니다. 이 메서드는 [정보 창을 클릭할](#Clicking_on_the_Info_Window) 때 발생하는 `InfoWindowClick` 이벤트에 대한 이벤트 처리기를 등록하고, 렌더러가 연결된 요소가 변경될 때만 구독 취소됩니다. `OnMapReady` 재정의는 `SetInfoWindowAdapter` 메서드를 호출하여 `CustomMapRenderer` 클래스 인스턴스가 정보 창을 사용자 지정하는 방법을 제공하도록 지정합니다.

`CustomMapRenderer` 클래스가 `GoogleMap.IInfoWindowAdapter` 인터페이스를 구현하여 [정보 창을 사용자 지정](#Customizing_the_Info_Window)합니다. 이 인터페이스는 다음 메서드를 구현해야 한다고 지정합니다.

- `public Android.Views.View GetInfoWindow(Marker marker)` – 이 메서드는 표식에 대한 사용자 지정 정보 창을 반환하기 위해 호출됩니다. `null`을 반환하면 기본 창 렌더링이 사용됩니다. `View`를 반환하면 `View`가 정보 창 프레임 내에 배치됩니다.
- `public Android.Views.View GetInfoContents(Marker marker)` – 이 메서드는 정보 창의 콘텐츠를 포함하는 `View`를 반환하기 위해 호출되며 `GetInfoWindow` 메서드가 `null`을 반환하는 경우에만 호출됩니다. `null`을 반환하면 정보 창 콘텐츠의 기본 렌더링이 사용됩니다.

샘플 애플리케이션에서는 정보 창 콘텐츠만 사용자 지정되므로 `GetInfoWindow` 메서드가 `null`을 반환하여 이를 사용하도록 설정합니다.

#### <a name="customizing-the-marker"></a>표식 사용자 지정

표식을 나타내는 데 사용되는 아이콘은 `MarkerOptions.SetIcon` 메서드를 호출하여 사용자 지정할 수 있습니다. 이는 맵에 추가된 각 `CreateMarker`에 대해 호출되는 `Pin` 메서드를 재정의하여 수행할 수 있습니다.

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

이 메서드는 각 `Pin` 인스턴스에 대해 새 `MarkerOption` 인스턴스를 만듭니다. 표식의 위치, 레이블 및 주소를 설정한 후 해당 아이콘이 `SetIcon` 메서드로 설정됩니다. 이 메서드는 아이콘을 렌더링하는 데 필요한 데이터가 포함된 `BitmapDescriptor`개체를 사용하고 `BitmapDescriptorFactory` 클래스는 `BitmapDescriptor`의 생성을 간소화하는 도우미 메서드를 제공합니다. `BitmapDescriptorFactory` 클래스를 사용하여 표식을 사용자 지정하는 방법에 대한 자세한 내용은 [표식 사용자 지정](~/android/platform/maps-and-location/maps/maps-api.md)을 참조하세요.

> [!NOTE]
> 필요한 경우 맵 렌더러에서 `GetMarkerForPin` 메서드를 호출하여 `Pin`에서 `Marker`를 검색할 수 있습니다.

<a name="Customizing_the_Info_Window" />

#### <a name="customizing-the-info-window"></a>정보 창 사용자 지정

사용자가 표식을 누를 때 `GetInfoWindow` 메서드가 `null`을 반환하면 `GetInfoContents` 메서드가 실행됩니다. 다음 코드 예제는 `GetInfoContents` 메서드를 보여줍니다.

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

    if (customPin.MarkerId.ToString() == "Xamarin") {
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

이 메서드는 정보 창의 메서드를 포함하는 `View`를 반환합니다. 이는 다음과 같이 수행됩니다.

- `LayoutInflater` 인스턴스가 검색됩니다. 이는 레이아웃 XML 파일을 해당 `View`로 인스턴스화하는 데 사용됩니다.
- 정보 창에 대한 사용자 지정 핀 데이터를 반환하기 위해 `GetCustomPin` 메서드가 호출됩니다.
- `CustomPin.MarkerId` 속성이 `Xamarin`와 같은 경우 `XamarinMapInfoWindow` 레이아웃이 확장됩니다. 그렇지 않으면 `MapInfoWindow` 레이아웃이 확장됩니다. 이렇게 하면 여러 표식에 대해 다양한 정보 창 레이아웃을 표시할 수 있는 시나리오를 허용합니다.
- `InfoWindowTitle` 및 `InfoWindowSubtitle` 리소스는 확장된 레이아웃에서 검색되고, 리소스가 `null`이 아닌 경우 `Text` 속성이 `Marker` 인스턴스에서 해당 데이터로 설정됩니다.
- `View` 인스턴스가 맵에 표시하기 위해 반환됩니다.

> [!NOTE]
> 정보 창은 라이브 `View`가 아닙니다. 대신 Android는 `View`를 정적 비트맵으로 변환하고 이를 이미지로 표시합니다. 즉, 정보 창은 클릭 이벤트에 응답할 수 있지만, 터치 이벤트나 제스처에 응답할 수 없으며 정보 창의 개별 컨트롤은 자체 클릭 이벤트에 응답할 수 없습니다.

<a name="Clicking_on_the_Info_Window" />

#### <a name="clicking-on-the-info-window"></a>정보 창 클릭

사용자가 정보 창을 클릭하면 `InfoWindowClick` 이벤트가 발생하고, 차례로 `OnInfoWindowClick` 메서드가 실행됩니다.

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

이 메서드는 웹 브라우저를 열고 `Marker`에 대해 검색된 `CustomPin` 인스턴스의 `Url` 속성에 저장된 주소로 이동합니다. .NET Standard 라이브러리 프로젝트에서 `CustomPin` 컬렉션을 만들 때 주소가 정의되었습니다.

`MapView` 인스턴스 사용자 지정에 대한 자세한 내용은 [Maps API](~/android/platform/maps-and-location/maps/maps-api.md)를 참조하세요.

### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>유니버설 Windows 플랫폼에서 사용자 지정 렌더러 만들기

다음 스크린샷은 사용자 지정 전후의 맵을 보여줍니다.

![](customized-pin-images/map-layout-uwp.png "사용자 지정 전후의 맵 컨트롤")

UWP에서 핀은 *맵 아이콘*이라고 하며, 사용자 지정 이미지 또는 시스템 정의 기본 이미지일 수 있습니다. 맵 아이콘은 사용자가 맵 아이콘을 탭핑하는 것에 응답하여 나타내는 `UserControl`을 표시할 수 있습니다. `UserControl`은 `Pin` 인스턴스의 `Label` 및 `Address` 속성을 포함한 모든 내용을 표시할 수 있습니다.

다음 코드 예제는 UWP 사용자 지정 렌더러를 보여줍니다.

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

사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결되어 있는 경우 `OnElementChanged` 메서드가 다음 작업을 수행합니다.

- `MapElementClick` 이벤트에 대한 이벤트 처리기를 등록하기 전에 맵에서 기존 사용자 인터페이스 요소를 제거하기 위해 `MapControl.Children` 컬렉션을 지웁니다. 이 이벤트는 사용자가 `MapControl`에서 `MapElement`를 탭하거나 클릭할 때 발생하며, 렌더러가 변경 내용에 연결된 경우에만 구독 취소됩니다.
- `customPins` 컬렉션의 각 핀은 다음과 같이 맵의 올바른 지리적 위치에 표시됩니다.
  - 핀의 위치는 `Geopoint` 인스턴스로 생성됩니다.
  - 핀을 나타내는 `MapIcon` 인스턴스가 생성됩니다.
  - `MapIcon`을 나타내는 데 사용되는 이미지는 `MapIcon.Image` 속성을 설정하여 지정됩니다. 그러나 맵 아이콘의 이미지가 맵의 다른 요소에 의해 가려질 수 있기 때문에 항상 표시되지는 않습니다. 따라서 맵 아이콘의 `CollisionBehaviorDesired` 속성이 계속 표시되도록 `MapElementCollisionBehavior.RemainVisible`로 설정됩니다.
  - `MapIcon`의 위치는 `MapIcon.Location` 속성을 설정하여 지정됩니다.
  - `MapIcon.NormalizedAnchorPoint` 속성은 이미지에서 포인터의 대략적인 위치로 설정됩니다. 이 속성이 이미지의 왼쪽 위 모서리를 나타내는 기본값인 (0,0)을 유지하는 경우, 맵의 확대/축소 수준을 변경하면 이미지가 다른 위치를 가리킬 수 있습니다.
  - `MapIcon` 인스턴스가 `MapControl.MapElements` 컬렉션에 추가되었습니다. 이로 인해 맵 아이콘이 `MapControl`에 표시됩니다.

> [!NOTE]
> 여러 맵 아이콘에 동일한 이미지를 사용하는 경우 최상의 성능을 위해 페이지 또는 애플리케이션 수준에서 `RandomAccessStreamReference` 인스턴스를 선언해야 합니다.

#### <a name="displaying-the-usercontrol"></a>UserControl 표시

사용자가 맵 아이콘을 누르면 `OnMapElementClick` 메서드가 실행됩니다. 다음 코드 예제에서는 이 메서드를 보여줍니다.

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

            if (customPin.MarkerId.ToString() == "Xamarin")
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

이 메서드는 핀에 대한 정보를 표시하는 `UserControl` 인스턴스를 만듭니다. 이는 다음과 같이 수행됩니다.

- `MapIcon` 인스턴스가 검색됩니다.
- 표시할 사용자 지정 핀 데이터를 반환하기 위해 `GetCustomPin` 메서드가 호출됩니다.
- 사용자 지정 핀 데이터를 표시하기 위해 `XamarinMapOverlay` 인스턴스가 생성됩니다. 이 클래스는 사용자 정의 컨트롤입니다.
- `MapControl`에서 `XamarinMapOverlay` 인스턴스를 표시할 지리적 위치는 `Geopoint` 인스턴스로 생성됩니다.
- `XamarinMapOverlay` 인스턴스가 `MapControl.Children` 컬렉션에 추가되었습니다. 이 컬렉션에는 맵에 표시될 XAML 사용자 인터페이스 요소가 포함되어 있습니다.
- 맵에서 `XamarinMapOverlay` 인스턴스의 지리적 위치는 `SetLocation` 메서드를 호출하여 설정됩니다.
- 지정된 위치에 해당하는 `XamarinMapOverlay` 인스턴스의 상대 위치는 `SetNormalizedAnchorPoint` 메서드를 호출하여 설정됩니다. 이렇게 하면 `XamarinMapOverlay` 인스턴스가 항상 올바른 위치에 표시되는 맵 결과의 확대/축소 수준 변경이 보장됩니다.

또는 핀에 대한 정보가 이미 맵에 표시되어 있는 경우 맵을 누르면 `MapControl.Children` 컬렉션에서 `XamarinMapOverlay` 인스턴스가 제거됩니다.

#### <a name="tapping-on-the-information-button"></a>정보 단추 누르기

사용자가 `XamarinMapOverlay` 사용자 정의 컨트롤에서 *정보* 단추를 누르면 `Tapped` 이벤트가 발생하며, 차례로 `OnInfoButtonTapped` 메서드가 실행됩니다.

```csharp
private async void OnInfoButtonTapped(object sender, TappedRoutedEventArgs e)
{
    await Launcher.LaunchUriAsync(new Uri(customPin.Url));
}
```

이 메서드는 웹 브라우저를 열고 `CustomPin` 인스턴스의 `Url` 속성에 저장된 주소로 이동합니다. .NET Standard 라이브러리 프로젝트에서 `CustomPin` 컬렉션을 만들 때 주소가 정의되었습니다.

`MapControl` 인스턴스 사용자 지정에 대한 자세한 내용은 MSDN의 [맵 및 위치 개요](https://msdn.microsoft.com/library/windows/apps/mt219699.aspx)를 참조하세요.

## <a name="related-links"></a>관련 링크

- [맵 컨트롤](~/xamarin-forms/user-interface/map.md)
- [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md)
- [지도 API](~/android/platform/maps-and-location/maps/maps-api.md)
- [사용자 지정된 Pin(샘플)](https://developer.xamarin.com/samples/xamarin-forms/CustomRenderers/Map/Pin/)
