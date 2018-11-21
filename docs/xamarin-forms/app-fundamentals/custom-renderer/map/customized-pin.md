---
title: 지도 핀 사용자 지정
description: 이 문서에는 각 플랫폼에서 사용자 지정 된 pin 및 pin 데이터의 사용자 지정된 보기를 사용 하 여 기본 지도 표시 하는 지도 컨트롤에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: C5481D86-80E9-4E3D-9FB6-57B0F93711A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: ab5315d169615430f5f5a733c0fa8c2ca9caa4b0
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52172303"
---
# <a name="customizing-a-map-pin"></a>지도 핀 사용자 지정

_이 문서에는 각 플랫폼에서 사용자 지정 된 pin 및 pin 데이터의 사용자 지정된 보기를 사용 하 여 기본 지도 표시 하는 지도 컨트롤에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다._

모든 Xamarin.Forms 보기에 네이티브 컨트롤의 인스턴스를 만드는 각 플랫폼에 대 한는 함께 제공 되는 렌더러. 경우는 [ `Map` ](xref:Xamarin.Forms.Maps.Map) ios에서는 Xamarin.Forms 응용 프로그램에서 렌더링 되는 `MapRenderer` 클래스가 인스턴스화되면 네이티브를 다시 인스턴스화하는 `MKMapView` 제어 합니다. Android 플랫폼에는 `MapRenderer` 클래스에는 네이티브 인스턴스화합니다 `MapView` 컨트롤입니다. Windows 플랫폼 (UWP (유니버설)을 합니다 `MapRenderer` 클래스에는 네이티브 인스턴스화합니다 `MapControl`합니다. 렌더러 및 Xamarin.Forms 컨트롤에 매핑되는 네이티브 컨트롤 클래스에 대 한 자세한 내용은 참조 하세요. [렌더러 기본 클래스 및 네이티브 컨트롤](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)합니다.

다음 다이어그램 간의 관계는 [ `Map` ](xref:Xamarin.Forms.Maps.Map) 및 구현 하는 해당 네이티브 컨트롤:

![](customized-pin-images/map-classes.png "맵 컨트롤 및 구현 네이티브 컨트롤 간의 관계")

플랫폼별 사용자 지정에 대 한 사용자 지정 렌더러를 만들어 구현 하려면 렌더링 프로세스를 사용할 수는 [ `Map` ](xref:Xamarin.Forms.Maps.Map) 각 플랫폼에서 합니다. 이 수행 하는 프로세스는 다음과 같습니다.

1. [만들](#Creating_the_Custom_Map) Xamarin.Forms 사용자 지정 지도입니다.
1. [사용할](#Consuming_the_Custom_Map) Xamarin.Forms에서 사용자 지정 맵.
1. [만들](#Creating_the_Custom_Renderer_on_each_Platform) 지도의 각 플랫폼에서 사용자 지정 렌더러.

각 항목 이제 살펴봅니다를 구현 하는 `CustomMap` 각 플랫폼에 사용자 지정 된 pin 및 pin 데이터의 사용자 지정된 보기를 사용 하 여 기본 지도 표시 하는 렌더러.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) 초기화 하 고 사용 하기 전에 구성 해야 합니다. 자세한 내용은 [`Maps Control`](~/xamarin-forms/user-interface/map.md)를 참조하세요.

<a name="Creating_the_Custom_Map" />

## <a name="creating-the-custom-map"></a>사용자 지정 맵 만들기

사용자 지정 맵 컨트롤을 서브클래싱하 여 만들 수 있습니다 합니다 [ `Map` ](xref:Xamarin.Forms.Maps.Map) 클래스에 다음 코드 예제 에서처럼:

```csharp
public class CustomMap : Map
{
  public List<CustomPin> CustomPins { get; set; }
}
```

`CustomMap` 컨트롤.NET Standard 라이브러리 프로젝트에서 생성 되 고 사용자 지정 지도 대 한 API를 정의 합니다. 사용자 지정 지도 노출 합니다 `CustomPins` 속성의 컬렉션을 나타내는 `CustomPin` 각 플랫폼의 네이티브 지도 컨트롤에서 렌더링 되는 개체입니다. `CustomPin` 클래스는 다음 코드 예제에 표시 됩니다.

```csharp
public class CustomPin : Pin
{
  public string Url { get; set; }
}
```

이 클래스 정의 `CustomPin` 의 속성을 상속 합니다 [ `Pin` ](xref:Xamarin.Forms.Maps.Pin) 클래스 및 추가 `Url` 속성.

<a name="Consuming_the_Custom_Map" />

## <a name="consuming-the-custom-map"></a>사용자 지정 맵 사용

`CustomMap` 컨트롤 참조할 수 있습니다 XAML에서.NET Standard 라이브러리 프로젝트에서 해당 위치에 대 한 네임 스페이스를 선언 하 고 사용자 지정 지도 컨트롤에서 네임 스페이스 접두사를 사용 하 여 합니다. 다음 코드 예제에서는 방법을 `CustomMap` XAML 페이지에서 컨트롤을 사용할 수 있습니다.

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

`local` 원하는 네임 스페이스 접두사를 이름을 지정할 수 있습니다. 그러나 합니다 `clr-namespace` 고 `assembly` 값 사용자 지정 지도의 세부 정보를 일치 해야 합니다. 네임 스페이스를 선언 하는 사용자 지정 지도 참조 하는 접두사가 사용 됩니다.

다음 코드 예제에서는 방법을 `CustomMap` C# 페이지에서 컨트롤을 사용할 수 있습니다.

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

`CustomMap` 인스턴스에서 각 플랫폼에 기본 지도 표시 하는 데 사용 됩니다. 있기 [ `MapType` ](xref:Xamarin.Forms.Maps.Map.MapType) 속성의 표시 스타일을 설정 합니다 [ `Map` ](xref:Xamarin.Forms.Maps.Map)에 정의 되 고 가능한 값으로는 [ `MapType` ](xref:Xamarin.Forms.Maps.MapType) 열거형. IOS 및 Android 용 지도의 높이 너비는 설정의 속성을 통해는 `App` 플랫폼별 프로젝트에서 초기화 되는 클래스입니다.

지도 및 핀의 위치가 들어 있습니다. 다음 코드 예제와 같이 초기화 됩니다.

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

Pin을 사용자 지정을 추가 하 고 사용 하 여 지도의 보기를 배치 하는이 초기화를 [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) 메서드를 만들어 위치 및 지도 확대/축소 수준을 변경 하는 [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) 에서 [ `Position` ](xref:Xamarin.Forms.Maps.Position) 와 [ `Distance` ](xref:Xamarin.Forms.Maps.Distance)합니다.

사용자 지정 렌더러는 이제 네이티브 맵 컨트롤을 사용자 지정 하려면 각 응용 프로그램 프로젝트에 추가할 수 있습니다.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>각 플랫폼에서 사용자 지정 렌더러 만들기

사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. 서브 클래스를 만든를 `MapRenderer` 사용자 지정 지도 렌더링 하는 클래스입니다.
1. 재정의 `OnElementChanged` 쓰기 논리 사용자 지정 하 고 사용자 지정 지도 렌더링 하는 메서드. 이 메서드는 해당 Xamarin.Forms 사용자 지정 지도 만들 때 호출 됩니다.
1. 추가 `ExportRenderer` 특성을 사용자 지정 렌더러 클래스 Xamarin.Forms 사용자 지정 맵 렌더링을 사용 수를 지정할 수 있습니다. 이 특성은 Xamarin.Forms를 사용 하 여 사용자 지정 렌더러를 등록 하는 데 사용 됩니다.

> [!NOTE]
> 각 플랫폼 프로젝트에서 사용자 지정 렌더러를 제공 하는 선택 사항입니다. 사용자 지정 렌더러를 등록 하지 않은 경우 컨트롤의 기본 클래스에 대 한 기본 렌더러 사용 됩니다.

다음 다이어그램은 이들 간의 관계와 함께 샘플 응용 프로그램에서 각 프로젝트의 책임을 보여 줍니다.

![](customized-pin-images/solution-structure.png "CustomMap 사용자 지정 렌더러 프로젝트 책임")

합니다 `CustomMap` 컨트롤에서 파생 되는 플랫폼별 렌더러 클래스에 의해 렌더링 되는 `MapRenderer` 각 플랫폼에 대 한 클래스입니다. 이 인해 각 `CustomMap` 다음 스크린샷과에서 같이 플랫폼 특정 컨트롤에 렌더링 되 고 제어 합니다.

![](customized-pin-images/screenshots.png "각 플랫폼에서 CustomMap")

`MapRenderer` 노출 클래스는 `OnElementChanged` 메서드를 해당 네이티브 컨트롤을 렌더링 하는 Xamarin.Forms 사용자 지정 지도 만들 때 호출 됩니다. 이 메서드는 `ElementChangedEventArgs` 포함 된 매개 변수 `OldElement` 및 `NewElement` 속성입니다. 이러한 속성은 Xamarin.Forms 요소를 나타냅니다는 렌더러 *되었습니다* 에 연결 및 Xamarin.Forms 요소는 렌더러 *는* 을 각각 연결 합니다. 샘플 응용 프로그램에서을 `OldElement` 속성이 `null` 와 `NewElement` 속성에 대 한 참조가 포함 됩니다는 `CustomMap` 인스턴스.

재정의 된 버전을 `OnElementChanged` 각 플랫폼별 렌더러 클래스에서 메서드는 네이티브 컨트롤 사용자 지정을 수행 하는 위치입니다. 플랫폼에서 사용 되는 네이티브 컨트롤에 대 한 형식화 된 참조를 통해 액세스할 수는 `Control` 속성입니다. 또한를 통해 렌더링 되는 Xamarin.Forms 컨트롤에 대 한 참조를 가져올 수 있습니다는 `Element` 속성입니다.

이벤트 처리기를 구독할 때는 주의 기울여야 합니다는 `OnElementChanged` 메서드를 다음 코드 예제에서 설명한 것 처럼:

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

네이티브 컨트롤을 구성 해야 하 고 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결 된 경우에 이벤트 처리기를 구독 합니다. 마찬가지로, 된 모든 이벤트 처리기에서 렌더러에 연결 된 요소가 변경 될 때만 구독 해야 수 있습니다. 이 접근 방식을 채택 메모리 누수의 영향을 받지 않으며는 사용자 지정 렌더러를 만드는 데 도움이 됩니다.

으로 데코 레이트 된 각 사용자 지정 렌더러 클래스는 `ExportRenderer` Xamarin.Forms를 사용 하 여 렌더러를 등록 하는 특성입니다. 특성 매개 변수 두 개 – 렌더링 되 고 Xamarin.Forms 사용자 지정 컨트롤의 형식 이름 및 사용자 지정 렌더러의 형식 이름입니다. `assembly` 접두사 특성에 특성을 전체 어셈블리에 적용 되도록 지정 합니다.

다음 섹션에서는 각 플랫폼별 사용자 지정 렌더러 클래스의 구현을 설명합니다.

### <a name="creating-the-custom-renderer-on-ios"></a>IOS에서 사용자 지정 렌더러 만들기

다음 스크린샷은 전후의 지도 보여 줍니다.

![](customized-pin-images/map-layout-ios.png "지도 컨트롤 사용자 지정 전후")

IOS pin 라고는 *주석*, 사용자 지정 이미지 또는 다양 한 색의 시스템에 정의 된 pin을 수 있습니다. 주석을 표시할 수는 *설명선*, 주석이 선택 하면 사용자에 대 한 응답에 표시 됩니다. 설명선이 표시 됩니다는 `Label` 및 `Address` 의 속성을 `Pin` 선택적 왼쪽 및 오른쪽 액세서리 뷰를 사용 하 여 인스턴스. 위의 스크린샷에서 왼쪽된 액세서리 보기의 monkey, 이미지 되 고 오른쪽 액세서리 보기는 *정보* 단추입니다.

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

`OnElementChanged` 메서드의 다음 구성 작업을 수행 합니다 [ `MKMapView` ](https://developer.xamarin.com/api/type/MapKit.MKMapView/) 는 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결 된 인스턴스:

- 합니다 [ `GetViewForAnnotation` ](https://developer.xamarin.com/api/property/MapKit.MKMapView.GetViewForAnnotation/) 속성을 `GetViewForAnnotation` 메서드. 이 메서드를 호출한 경우 합니다 [주석의 위치를 지도에 표시 됩니다.](#Displaying_the_Annotation), 표시할 주석을 이전 사용자 지정 하는 데 사용 됩니다.
- 에 대 한 이벤트 처리기를 `CalloutAccessoryControlTapped`, `DidSelectAnnotationView`, 및 `DidDeselectAnnotationView` 이벤트 등록 됩니다. 이러한 이벤트를 발생 시키는 경우 사용자 [설명선에 오른쪽 액세서리 탭](#Tapping_on_the_Right_Callout_Accessory_View), 및 시기 사용자 [선택](#Selecting_the_Annotation) 및 [취소](#Deselecting_the_Annotation) 주석 각각. 렌더러가 변경 내용에 연결 된 경우에 이벤트 수신을있지 않습니다.

<a name="Displaying_the_Annotation" />

#### <a name="displaying-the-annotation"></a>주석을 표시합니다.

`GetViewForAnnotation` 메서드 주석의 위치 지도에 표시 되 고 표시에 주석이 이전 사용자 지정 하는 경우 호출 됩니다. 주석의 두 부분이 있습니다.

- `MkAnnotation` – 제목, 부제목 및 주석의 위치를 포함 합니다.
- `MkAnnotationView` – 이미지 주석 및 필요에 따라 사용자가 주석을 누를 때 표시 되는 설명선 나타내는를 포함 합니다.

`GetViewForAnnotation` 메서드는 `IMKAnnotation` 주석 데이터를 포함 하 고 반환 하는 `MKAnnotationView` 지도에 표시 하기 위해 다음 코드 예제에 표시 됩니다:

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

이 메서드를 사용 하면 사용자 지정 이미지로 표시할 주석을 보다는 시스템에 정의 된 pin을 하며 주석이 탭 할 때 설명선 표시할 왼쪽 및 오른쪽에 주석이 제목 및 주소를 추가 콘텐츠를 포함 하는 . 이 작업은 다음과 같이 수행 됩니다.

1. `GetCustomPin` 메서드를 호출 하는 주석에 대 한 사용자 지정 고정 데이터를 반환 합니다.
1. 메모리를 절약 하는 주석의 보기에 대 한 호출을 사용 하 여 다시 사용할 수 있도록 풀링되 [ `DequeueReusableAnnotation` ](https://developer.xamarin.com/api/member/MapKit.MKMapView.DequeueReusableAnnotation/(System.String)/)합니다.
1. `CustomMKAnnotationView` 클래스를 확장 합니다 `MKAnnotationView` 클래스와 `Id` 및 `Url` 속성에서 동일한 속성에 해당 하는 `CustomPin` 인스턴스. 새 인스턴스를 `CustomMKAnnotationView` 주석이 것임 만들어집니다 `null`:
    - `CustomMKAnnotationView.Image` 맵에 주석을 나타내는 이미지 속성이 있습니다.
    - 합니다 `CustomMKAnnotationView.CalloutOffset` 속성을 `CGPoint` 설명선이 주석 위에 가운데 맞춤 될를 지정 하는 합니다.
    - `CustomMKAnnotationView.LeftCalloutAccessoryView` 주석 제목 및 주소를 왼쪽에 표시 되는 monkey 이미지 속성이 있습니다.
    - `CustomMKAnnotationView.RightCalloutAccessoryView` 속성을 *정보* 주석 제목과 주소의 오른쪽에 표시 되는 단추입니다.
    - 합니다 `CustomMKAnnotationView.Id` 속성을 `CustomPin.Id` 속성에서 반환 되는 `GetCustomPin` 메서드. 이렇게 하면 갖도록 식별할 수에 대 한 주석을 [설명선 추가로 사용자 지정할 수 있습니다](#Selecting_the_Annotation)필요한 경우.
    - 합니다 `CustomMKAnnotationView.Url` 속성을 `CustomPin.Url` 속성에서 반환 되는 `GetCustomPin` 메서드. 때 URL을 이동 하 게 될 사용자 [가 올바른 설명선 액세서리 뷰에 표시 단추를 누를](#Tapping_on_the_Right_Callout_Accessory_View)합니다.
1. 합니다 [ `MKAnnotationView.CanShowCallout` ](https://developer.xamarin.com/api/property/MapKit.MKAnnotationView.CanShowCallout/) 속성이 `true` 설명선 주석을 탭 할 때 표시 되도록 합니다.
1. 주석이 지도에 표시 하기 위해 반환 됩니다.

<a name="Selecting_the_Annotation" />

#### <a name="selecting-the-annotation"></a>주석 선택

사용자 주석을 누르면 합니다 `DidSelectAnnotationView` 을 실행 하는 이벤트 발생을 `OnDidSelectAnnotationView` 메서드:

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

이 메서드를 추가 하 여 기존 설명선 (보기가 있는 왼쪽 및 오른쪽 액세서리) 확장을 `UIView` 선택한 주석에 Xamarin 로고 이미지를 포함 하는 인스턴스를 해당 `Id` 로설정하는속성`Xamarin`. 따라서 다른 주석에 대 한 다양 한 설명선이 표시 될 수 있는 시나리오. `UIView` 인스턴스 기존 설명선 위쪽 가운데에 표시 됩니다.

<a name="Tapping_on_the_Right_Callout_Accessory_View" />

#### <a name="tapping-on-the-right-callout-accessory-view"></a>오른쪽 설명선 액세서리 보기 탭

사용자가 되는 경우는 *정보* 오른쪽 설명선 액세서리 보기에서 단추를 `CalloutAccessoryControlTapped` 을 실행 하는 이벤트가 발생 합니다 `OnCalloutAccessoryControlTapped` 메서드:

```csharp
void OnCalloutAccessoryControlTapped (object sender, MKMapViewAccessoryTappedEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  if (!string.IsNullOrWhiteSpace (customView.Url)) {
    UIApplication.SharedApplication.OpenUrl (new Foundation.NSUrl (customView.Url));
  }
}
```

이 메서드는 웹 브라우저를 열고에 저장 된 주소의으로 이동 합니다 `CustomMKAnnotationView.Url` 속성입니다. 주소를 만들 때 정의 된는 `CustomPin` .NET Standard 라이브러리 프로젝트에는 컬렉션입니다.

<a name="Deselecting_the_Annotation" />

#### <a name="deselecting-the-annotation"></a>주석 취소

주석이 표시 되 고 지도에 사용자가 누를 때 합니다 `DidDeselectAnnotationView` 을 실행 하는 이벤트 발생을 `OnDidDeselectAnnotationView` 메서드:

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

이 메서드가 사용 하면 기존 설명선 선택 하지 않으면 설명선 (Xamarin 로고 이미지)의 확장 된 부분을 표시 되 고 중지 됩니다 하 고 해당 리소스가 해제 됩니다.

사용자 지정 하는 방법에 대 한 자세한 내용은 `MKMapView` 인스턴스를 참조 하십시오 [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md)합니다.

### <a name="creating-the-custom-renderer-on-android"></a>Android에서 사용자 지정 렌더러 만들기

다음 스크린샷은 전후의 지도 보여 줍니다.

![](customized-pin-images/map-layout-android.png "지도 컨트롤 사용자 지정 전후")

Android에서 pin 라고는 *표식*, 및 사용자 지정 이미지 또는 다양 한 색의 시스템 정의 표식 일 수 있습니다. 표식을 표시할 수는 *정보 창의*, 표식 탭 하거나 사용자에 대 한 응답에 표시 됩니다. 정보 창에 표시 됩니다는 `Label` 및 `Address` 의 속성을 `Pin` 인스턴스 및 다른 콘텐츠를 포함 하도록 사용자 지정할 수 있습니다. 그러나 하나의 정보 창은 한 번에 표시할 수 있습니다.

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

사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결 되어 있는 `OnElementChanged` 메서드 호출을 `MapView.GetMapAsync` 메서드 내부를 가져옵니다 `GoogleMap` 는 뷰에 연결 됩니다. 한 번를 `GoogleMap` 인스턴스가 사용 가능한를 `OnMapReady` 재정의 호출 됩니다. 에 대 한 이벤트 처리기를 등록 하는이 메서드는 `InfoWindowClick` 될 때 발생 하는 이벤트를 [정보 창을 클릭할](#Clicking_on_the_Info_Window), 렌더러가 변경 내용에 연결 된 경우에에서 구독을 취소 되 고 합니다. `OnMapReady` 재정의 호출 합니다 `SetInfoWindowAdapter` 지정 하는 메서드를 `CustomMapRenderer` 클래스 인스턴스 정보 창의 사용자 지정 하는 방법을 제공 합니다.

합니다 `CustomMapRenderer` 클래스가 구현 하는 `GoogleMap.IInfoWindowAdapter` 인터페이스 [정보 창을 사용자 지정할](#Customizing_the_Info_Window)합니다. 이 인터페이스는 다음 메서드를 구현 해야 해야 지정 합니다.

- `public Android.Views.View GetInfoWindow(Marker marker)` –이 메서드는 표식에 대 한 사용자 지정 정보 창을 반환 합니다. 반환 하는 경우 `null`, 기본 창 렌더링이 사용 됩니다. 반환 하는 경우는 `View`에 다음 `View` 정보 창 프레임 내에 배치 되도록 합니다.
- `public Android.Views.View GetInfoContents(Marker marker)` –이 메서드는 반환 하기 위해 호출을 `View` 정보 창의 콘텐츠를 포함 하 고 경우에 호출 됩니다 합니다 `GetInfoWindow` 메서드가 반환 되는 `null`합니다. 반환 하는 경우 `null`, 정보 창 콘텐츠의 기본 렌더링은이 사용 됩니다.

샘플 응용 프로그램에서 정보 창의 콘텐츠를 사용자 지정한 하므로 `GetInfoWindow` 메서드가 반환 되는 `null` 이 사용 하도록 설정 합니다.

#### <a name="customizing-the-marker"></a>마커를 사용자 지정

표식을 나타내는 데 사용 되는 아이콘을 호출 하 여 사용자 지정할 수 있습니다는 `MarkerOptions.SetIcon` 메서드. 재정의 하 여이 작업을 수행할 수 있습니다 합니다 `CreateMarker` 각각에 대해 호출 되는 메서드를 `Pin` 지도에 추가 되는:

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

이 메서드를 만듭니다 `MarkerOption` 각각에 대 한 인스턴스 `Pin` 인스턴스. 해당 아이콘으로 설정 된 위치, 레이블 및 표식의 주소를 설정한 후의 `SetIcon` 메서드. 이 메서드를 `BitmapDescriptor` 아이콘을 사용 하 여 렌더링 하는 데 필요한 데이터가 포함 된 개체를 `BitmapDescriptorFactory` 생성을 간소화 하기 위한 도우미 메서드를 제공 하는 클래스를 `BitmapDescriptor`. 사용에 대 한 자세한 내용은 합니다 `BitmapDescriptorFactory` 마커를 사용자 지정할 내용은 클래스 [표식을 사용자 지정](~/android/platform/maps-and-location/maps/maps-api.md)합니다.

> [!NOTE]
> 필요한 경우는 `GetMarkerForPin` 를 검색 하 여 맵 렌더러에서 메서드를 호출할 수는 `Marker` 에서 `Pin`합니다.

<a name="Customizing_the_Info_Window" />

#### <a name="customizing-the-info-window"></a>정보 창 사용자 지정

사용자 마커를 누르면 합니다 `GetInfoContents` 메서드를 실행 하는 제공 합니다 `GetInfoWindow` 메서드가 반환 되는 `null`합니다. 다음 코드 예제는 `GetInfoContents` 메서드:

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

이 메서드가 반환을 `View` 정보 창의 내용을 포함 합니다. 이 작업은 다음과 같이 수행 됩니다.

- `LayoutInflater` 인스턴스를 검색 합니다. 레이아웃 XML 파일을 해당를 인스턴스화하는이 `View`합니다.
- `GetCustomPin` 메서드를 호출 하는 정보 창에 대 한 사용자 지정 고정 데이터를 반환 합니다.
- `XamarinMapInfoWindow` 경우 레이아웃을 확장 합니다 `CustomPin.Id` 속성과 같으면 `Xamarin`합니다. 그렇지 않은 경우는 `MapInfoWindow` 레이아웃을 확장 합니다. 따라서 여러 가지 표식에 대 한 다른 정보 창 레이아웃에 표시 될 수 있는 시나리오.
- 합니다 `InfoWindowTitle` 및 `InfoWindowSubtitle` 리소스가 배포용된 레이아웃에서 검색 됩니다 및 해당 `Text` 속성에서 해당 데이터에 설정 됩니다는 `Marker` 인스턴스를 제공 하는 리소스는 되지 않습니다 `null`합니다.
- `View` 지도에 표시 하기 위해 인스턴스가 반환 됩니다.

> [!NOTE]
> 정보 창에는 라이브 아닙니다 `View`합니다. Android에서는 변환 하는 대신는 `View` 정적 비트맵 및 이미지로 표시 합니다. 이 즉, 프로그램 정보 창의 하는 동안 터치 이벤트 나 제스처에 응답할 수 및 정보 창에서 개별 컨트롤 자체에 응답할 수 없으므로 클릭 이벤트에 응답할 수 있는 이벤트를 클릭 합니다.

<a name="Clicking_on_the_Info_Window" />

#### <a name="clicking-on-the-info-window"></a>정보 창에서 클릭합니다.

정보 창에서 사용자가 클릭 합니다 `InfoWindowClick` 을 실행 하는 이벤트 발생을 `OnInfoWindowClick` 메서드:

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

이 메서드는 웹 브라우저를 열고에 저장 된 주소의으로 이동 합니다 `Url` 검색 하 고 검색 속성 `CustomPin` 에 대 한 인스턴스는 `Marker`합니다. 주소를 만들 때 정의 된는 `CustomPin` .NET Standard 라이브러리 프로젝트에는 컬렉션입니다.

사용자 지정 하는 방법에 대 한 자세한 내용은 `MapView` 인스턴스를 참조 하십시오 [Maps API](~/android/platform/maps-and-location/maps/maps-api.md)합니다.

### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>유니버설 Windows 플랫폼에서 사용자 지정 렌더러 만들기

다음 스크린샷은 전후의 지도 보여 줍니다.

![](customized-pin-images/map-layout-uwp.png "지도 컨트롤 사용자 지정 전후")

UWP pin 라고는 *맵 아이콘*, 및 사용자 지정 이미지 또는 시스템 정의 기본 이미지 일 수 있습니다. 맵 아이콘을 표시할 수는 `UserControl`, 맵 아이콘을 탭 하거나 사용자에 대 한 응답에 표시 됩니다. `UserControl` 모든 콘텐츠를 표시할 수 포함 하 여는 `Label` 및 `Address` 의 속성을 `Pin` 인스턴스.

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

`OnElementChanged` 메서드는 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결할 때 다음과 같은 작업을 수행 합니다.

- 지웁니다 합니다 `MapControl.Children` 맵에서 대 한 이벤트 처리기를 등록 하기 전에 모든 기존 사용자 인터페이스 요소를 제거 하려면 컬렉션을 `MapElementClick` 이벤트입니다. 사용자 탭 하거나 클릭할 때이 이벤트가 발생을 `MapElement` 에 `MapControl`, 렌더러가 변경 내용에 연결 된 경우에에서 구독을 취소 되 고 합니다.
- 핀을 `customPins` 컬렉션 맵에서 올바른 지리적 위치에 다음과 같이 표시 됩니다.
  - Pin에 대 한 위치 만들어집니다는 `Geopoint` 인스턴스.
  - `MapIcon` 인스턴스 pin을 나타내기 위해 만든 것입니다.
  - 나타내는 데 사용 되는 이미지를 `MapIcon` 설정 하 여 지정 된 `MapIcon.Image` 속성입니다. 그러나 구조 아이콘의 이미지 보장 되지 않습니다 항상 표시 될으로 map의 요소에 의해 가려질 수 있습니다. 따라서 맵 아이콘의 `CollisionBehaviorDesired` 속성이 `MapElementCollisionBehavior.RemainVisible`계속 표시 되도록 합니다.
  - 위치를 `MapIcon` 설정 하 여 지정 된 `MapIcon.Location` 속성입니다.
  - `MapIcon.NormalizedAnchorPoint` 이미지에 대 한 포인터의 대략적인 위치를 설정 합니다. 이 속성의 값을 기본값 (0, 0), 이미지의 왼쪽된 위 모퉁이 나타내는 유지 하는 경우 다른 위치를 가리키는 이미지 지도 확대/축소 수준의 변경 될 수 있습니다.
  - 합니다 `MapIcon` 인스턴스를 추가할는 `MapControl.MapElements` 컬렉션입니다. 이 인해 맵 아이콘에 표시 되는 `MapControl`합니다.

> [!NOTE]
> 여러 맵 아이콘에 대 한 동일한 이미지를 사용 하는 경우는 `RandomAccessStreamReference` 최상의 성능을 위해 인스턴스를 페이지나 응용 프로그램 수준에서 선언 해야 합니다.

#### <a name="displaying-the-usercontrol"></a>UserControl 표시

사용자 맵 아이콘을 누르면는 `OnMapElementClick` 메서드를 실행 합니다. 다음 코드 예제에서는이 메서드를 보여 줍니다.

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
- `GetCustomPin` 메서드는 표시 되는 사용자 지정 고정 데이터를 반환 합니다.
- `XamarinMapOverlay` pin 사용자 지정 데이터를 표시 하려면 인스턴스가 만들어집니다. 이 클래스는 사용자 정의 컨트롤입니다.
- 지리적 위치를 표시할 합니다 `XamarinMapOverlay` 인스턴스를 `MapControl` 으로 만들어집니다는 `Geopoint` 인스턴스.
- 합니다 `XamarinMapOverlay` 인스턴스를 추가할는 `MapControl.Children` 컬렉션입니다. 이 컬렉션 지도에 표시 되는 XAML 사용자 인터페이스 요소를 포함 합니다.
- 지리적 위치를 `XamarinMapOverlay` 지도에 인스턴스를 호출 하 여 설정한는 `SetLocation` 메서드.
- 상대 위치를 `XamarinMapOverlay` 에 지정된 된 위치에 해당 하는 경우 호출 하 여 설정 됩니다는 `SetNormalizedAnchorPoint` 메서드. 이렇게 하면 변경에 따라 맵 결과가의 확대/축소 수준에는 `XamarinMapOverlay` 항상 올바른 위치에 표시 되 고 인스턴스.

또는 pin에 대 한 정보는 이미 지도에 표시 되는 경우 지도에 탭을 제거 합니다 `XamarinMapOverlay` 에서 인스턴스는 `MapControl.Children` 컬렉션입니다.

#### <a name="tapping-on-the-information-button"></a>정보 단추 탭

사용자가 누를 때를 *정보* 단추를 `XamarinMapOverlay` 사용자 정의 컨트롤을 `Tapped` 을 실행 하는 이벤트가 발생은 `OnInfoButtonTapped` 메서드:

```csharp
private async void OnInfoButtonTapped(object sender, TappedRoutedEventArgs e)
{
    await Launcher.LaunchUriAsync(new Uri(customPin.Url));
}
```

이 메서드는 웹 브라우저를 열고에 저장 된 주소의으로 이동 합니다 `Url` 의 속성을 `CustomPin` 인스턴스. 주소를 만들 때 정의 된는 `CustomPin` .NET Standard 라이브러리 프로젝트에는 컬렉션입니다.

사용자 지정 하는 방법에 대 한 자세한 내용은 `MapControl` 인스턴스를 참조 하십시오 [맵과 위치 개요](https://msdn.microsoft.com/library/windows/apps/mt219699.aspx) MSDN에서.

## <a name="summary"></a>요약

이 문서에 대 한 사용자 지정 렌더러를 만드는 방법을 설명 합니다 `Map` 컨트롤 개발자가 자신의 플랫폼 특정 사용자 지정을 사용 하 여 기본 네이티브 렌더링을 재정의할 수 있도록 합니다. Xamarin.Forms.Maps는 사용자를 위한 빠르고 친숙 한 지도 제공 하는 각 플랫폼에서 Api 환경을 기본 지도 사용 하는 지도 표시 하기 위한 플랫폼 간 추상화를 제공 합니다.


## <a name="related-links"></a>관련 링크

- [지도 컨트롤](~/xamarin-forms/user-interface/map.md)
- [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md)
- [지도 API](~/android/platform/maps-and-location/maps/maps-api.md)
- [사용자 지정 된 Pin (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/pin/)
