---
title: Xamarin.Forms 맵
description: 이 문서에서는 Xamarin.Forms 맵 클래스를 사용 하 여 각 플랫폼에서 기본 지도 Api를 사용 하 여 친숙 한 맵 사용자를 위한 환경을 제공 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/27/2018
ms.openlocfilehash: ddc9d18b57eac099331f0814b5963fb207840380
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61169593"
---
# <a name="xamarinforms-map"></a>Xamarin.Forms 맵

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/WorkingWithMaps/)

_Xamarin.Forms는 각 플랫폼에서 기본 지도 Api를 사용합니다._

Xamarin.Forms.Maps 각 플랫폼에 기본 지도 Api를 사용합니다. 이 사용자에 대 한 지도 신속 하 고 친숙 한 환경을 제공 하지만 각 플랫폼 API 요구 사항을 준수 하려면 몇 가지 구성 단계가 필요 함을 의미 합니다.
한 번 구성 하면는 `Map` 공용 코드의 다른 모든 Xamarin.Forms 요소 처럼 작동을 제어 합니다.

지도 컨트롤에서 사용 된 합니다 [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/) 아래 나와 있는 샘플입니다.

 [![MobileCRM 예제의 maps](map-images/maps-zoom-sml.png "지도 컨트롤 예제")](map-images/maps-zoom.png#lightbox "지도 컨트롤 예제")

맵 기능을 만들어 더 향상 시킬 수 있습니다는 [사용자 지정 렌더러를 매핑할](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)합니다.

<a name="Maps_Initialization" />

## <a name="maps-initialization"></a>맵 초기화

Xamarin.Forms 응용 프로그램에 맵을 추가 하는 경우 **Xamarin.Forms.Maps** 솔루션의 모든 프로젝트에 추가 해야 하는 별도 NuGet 패키지입니다.
Android에서이 또한에 종속이 되어 GooglePlayServices (다른 NuGet) Xamarin.Forms.Maps를 추가 하면 자동으로 다운로드 됩니다.

NuGet 패키지를 설치한 후 초기화 코드를 각 응용 프로그램 프로젝트에서 필요 *한 후* 는 `Xamarin.Forms.Forms.Init` 메서드 호출 합니다. IOS에 대 한 다음 코드를 사용 합니다.

```csharp
Xamarin.FormsMaps.Init();
```

Android에서 동일한 매개 변수를 전달 해야 `Forms.Init`:

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

다음 코드를 사용 하는 유니버설 Windows 플랫폼 (UWP)에 대 한 합니다.

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

각 플랫폼에 대해 다음 파일에이 호출을 추가 합니다.

-  **iOS** -AppDelegate.cs 파일을 `FinishedLaunching` 메서드.
-  **Android** -MainActivity.cs 파일은 `OnCreate` 메서드.
-  **UWP** -MainPage.xaml.cs 파일을 `MainPage` 생성자입니다.

NuGet 패키지를 추가 하 고 각 응용 프로그램 내에서 초기화 메서드 호출 되 면 `Xamarin.Forms.Maps` 일반적인.NET Standard 라이브러리 프로젝트 또는 공유 프로젝트 코드에서 Api를 사용할 수 있습니다.

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>플랫폼 구성

지도 표시 되기 전에 일부 플랫폼에서 추가 구성 단계가 필요 합니다.

### <a name="ios"></a>iOS

에 다음 키를 설정 해야 하는 iOS에서 위치 서비스에 액세스 하려면 **Info.plist**:

- iOS 11
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – 앱 사용 중인 경우 위치 서비스를 사용 하기 위한
    - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization?language=objc) -위치 서비스를 사용 하 여 모든 시간에 대 한
- iOS 10 및 이전 버전
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – 앱 사용 중인 경우 위치 서비스를 사용 하기 위한
    - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) -위치 서비스를 사용 하 여 모든 시간에 대 한    

IOS 11 및 이전 버전을 지원 하려면 모든 세 개의 키를 포함할 수 있습니다: `NSLocationWhenInUseUsageDescription`, `NSLocationAlwaysAndWhenInUseUsageDescription`, 및 `NSLocationAlwaysUsageDescription`합니다.

이러한 키에 대 한 XML 표현을 **Info.plist** 아래에 표시 됩니다. 업데이트 해야 합니다 `string` 응용 프로그램 위치 정보를 사용 하는 방식을 반영 하도록 값:

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Can we use your location at all times?</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Can we use your location when your app is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

합니다 **Info.plist** 항목에 추가할 수도 있습니다 **원본** 편집 하는 동안 보기의 **Info.plist** 파일:

![Ios 8 Info.plist](map-images/ios8-map-permissions.png "iOS 8 필요한 Info.plist 항목")

### <a name="android"></a>Android

사용 하는 [Google Maps API v2](https://developers.google.com/maps/documentation/android/) Android에서 API 키를 생성 한 Android 프로젝트에 추가 해야 합니다.
Xamarin 문서에서 지침에 따라 [v2 Google Maps API 키를 가져오는](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)합니다.
이러한 지침에서는 뒤에 있는 API 키를 붙여 넣습니다. 합니다 **Properties/AndroidManifest.xml** 파일 (소스 보기 및 다음 요소 찾기/업데이트):

```xml
<application ...>
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
</application>
```

올바른 API 키가 없는 지도 컨트롤은 Android에서 회색 상자로 표시 됩니다.

> [!NOTE]
> Google Maps에 액세스할 APK에 대 한 순서로 sha-1 지문 포함 하며 APK에 서명 하는 데 사용 하는 모든 keystore (디버그 및 릴리스)에 대 한 이름을 패키지 note 합니다. 예를 들어, 디버그 및 릴리스 APK를 생성 하기 위한 다른 컴퓨터에 대 한 컴퓨터를 사용 하는 경우 포함 해야 첫 번째 컴퓨터의 디버그 키 저장소에서 sha-1 인증서 지문 및의 릴리스 키 저장소에서 sha-1 인증서 지문 두 번째 컴퓨터입니다. 또한 경우 키 자격 증명을 편집 해야 앱의 **패키지 이름을** 변경 합니다. 참조 [v2 Google Maps API 키를 가져오는](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)합니다.

Android 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 적절 한 권한을 사용 하도록 설정 해야 **옵션 > 빌드 > Android 응용 프로그램** 다음 타이머에서 틱 및:

* `AccessCoarseLocation`
* `AccessFineLocation`
* `AccessLocationExtraCommands`
* `AccessMockLocation`
* `AccessNetworkState`
* `AccessWifiState`
* `Internet`

그 중 일부는 아래 스크린샷에 표시 됩니다.

![Android에 필요한 권한](map-images/android-map-permissions.png "Android에 대 한 필요한 사용 권한")

마지막 두는 응용 프로그램 맵 데이터를 다운로드 하려면 네트워크 연결이 필요 하기 때문에 필요 합니다. Android에 대해 알아보세요 [권한을](https://developer.android.com/reference/android/Manifest.permission.html) 에 대해 자세히 알아보세요.

또한 Android 9가 bootclasspath에서 Apache HTTP 클라이언트 라이브러리를 제거 및 이므로 이상의 API 28를 대상으로 하는 응용 프로그램에 사용할 수 없습니다. 에 다음 줄을 추가 해야 합니다 `application` 노드의 하 **AndroidManifest.xml** 28 이상 API를 대상으로 하는 응용 프로그램에서 Apache HTTP 클라이언트를 사용 하 여 계속 하려면 파일:

```xml
<application ...>
    ...
    <uses-library android:name="org.apache.http.legacy" android:required="false" />    
</application>
```

### <a name="universal-windows-platform"></a>UWP

유니버설 Windows 플랫폼에서 지도 사용 하 여 권한 부여 토큰을 생성 해야 합니다. 자세한 내용은 [지도 인증 키를 요청](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx) MSDN에서.

인증 토큰을 다음으로 지정 해야 합니다 `FormsMaps.Init("AUTHORIZATION_TOKEN")` 메서드 호출에서 Bing Maps를 사용 하 여 앱을 인증 합니다.

<a name="Using_Maps" />

## <a name="using-maps"></a>맵을 사용 하 여

참조 된 [MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) MobileCRM 샘플 코드의 맵 컨트롤을 사용할 수 있는 방법을의 예입니다. 간단한 `MapPage` 클래스-이 알림은 다음과 같을 수 있습니다 하는 새 `MapSpan` 지도의 보기를 배치 하기 위해 만들어집니다.

```csharp
public class MapPage : ContentPage {
    public MapPage() {
        var map = new Map(
            MapSpan.FromCenterAndRadius(
                    new Position(37,-122), Distance.FromMiles(0.3))) {
                IsShowingUser = true,
                HeightRequest = 100,
                WidthRequest = 960,
                VerticalOptions = LayoutOptions.FillAndExpand
            };
        var stack = new StackLayout { Spacing = 0 };
        stack.Children.Add(map);
        Content = stack;
    }
}
```

### <a name="map-type"></a>맵 유형

맵 내용을 설정 하 여 변경할 수도 있습니다는 `MapType` 일반 거리 맵 (기본값), 위성 이미지 또는 둘의 조합을 표시할 속성입니다.

```csharp
map.MapType == MapType.Street;
```

유효한 `MapType` 값은:

-  하이브리드
-  위성
-  주소 (기본값)

### <a name="map-region-and-mapspan"></a>지도 영역 및 MapSpan

위의 코드 조각에서와 같이 제공을 `MapSpan` 초기 보기를 설정 하는 맵 생성자에는 인스턴스 (중심점 및 확대/축소 수준) 로드 되는 경우 맵의 합니다. `MoveToRegion` 지도의 위치 또는 확대/축소 수준을 변경 하려면 다음 map 클래스에서 메서드를 사용할 수 있습니다. 두 가지 방법으로 새 `MapSpan` 인스턴스:

-  **MapSpan.FromCenterAndRadius()** -의 범위를 만드는 정적 메서드를를 `Position` 지정 하는 `Distance` 합니다.
-  **새 MapSpan ()** -사용 하는 생성자를 `Position` 위도 및 경도 표시할 각도입니다.


위치를 변경 하지 않고 지도의 확대/축소 수준을 변경 하려면 새로 만듭니다 `MapSpan` 에서 현재 위치를 사용 하는 `VisibleRegion.Center` 지도 컨트롤의 속성입니다. 하지만 `Slider` (지도 컨트롤에서 직접 확대/축소 슬라이더의 값을 현재 업데이트할 수 없습니다) 다음과 같은 지도 확대/축소를 제어 하는데 사용할 수 없습니다.

```csharp
var slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) => {
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

 [![확대/축소를 사용 하 여 maps](map-images/maps-zoom-sml.png "지도 컨트롤 확대/축소")](map-images/maps-zoom.png#lightbox "지도 컨트롤 확대/축소")

### <a name="map-pins"></a>지도 핀

위치를 사용 하 여 지도에 표시할 수 있습니다 `Pin` 개체입니다.

```csharp
var position = new Position(37,-122); // Latitude, Longitude
var pin = new Pin {
            Type = PinType.Place,
            Position = position,
            Label = "custom pin",
            Address = "custom detail info"
        };
map.Pins.Add(pin);
```

 `PinType` pin (플랫폼)에 따라 렌더링 되는 방식을 영향을 줄 수 있는 다음 값 중 하나로 설정할 수 있습니다.

-  제네릭
-  현재 위치
-  SavedPin
-  SearchResult

<a name="Using_Xaml" />

## <a name="using-xaml"></a>XAML을 사용 하 여

맵이 코드이 조각에서와 같이 XAML 레이아웃에 배치 될 수도 합니다.

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="MapDemo.MapPage">
    <StackLayout VerticalOptions="StartAndExpand" Padding="30">
        <maps:Map WidthRequest="320" HeightRequest="200"
                  x:Name="MyMap"
                  IsShowingUser="true"
                  MapType="Hybrid" />
    </StackLayout>
</ContentPage>
```

> [!NOTE]
> 추가 `xmlns` Xamarin.Forms.Maps 컨트롤 참조를 네임 스페이스 정의가 필요 합니다.

합니다 `MapRegion` 하 고 `Pins` 사용 하 여 코드에서 설정할 수 있습니다는 `MyMap` 참조 (또는 맵의 이름이 무엇이 든).

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

## <a name="populating-a-map-with-data-using-data-binding"></a>지도 데이터 바인딩을 사용 하 여 데이터로 채우기

합니다 [ `Map` ](xref:Xamarin.Forms.Maps.Map) 클래스에는 다음 속성을 노출 합니다.

- `ItemsSource` -컬렉션을 지정 `IEnumerable` 항목을 표시할 수 있습니다.
- `ItemTemplate` – 지정 된 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 표시 되는 항목의 컬렉션에 있는 각 항목에 적용 하 합니다.

따라서 한 [ `Map` ](xref:Xamarin.Forms.Maps.Map) 바인딩할 데이터 바인딩을 사용 하 여 데이터로 채울 수 있습니다 해당 `ItemsSource` 속성을는 `IEnumerable` 컬렉션:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="WorkingWithMaps.PinItemsSourcePage">
    <Grid>
        ...
        <maps:Map x:Name="map"
                  ItemsSource="{Binding Locations}">
            <maps:Map.ItemTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </maps:Map.ItemTemplate>
        </maps:Map>
        ...
    </Grid>
</ContentPage>
```

`ItemsSource` 속성 데이터를 바인딩합니다를 `Locations` 반환 하는 연결 된 뷰 모델의 속성을 `ObservableCollection` 의 `Location` 사용자 지정 형식인 개체입니다. 각 `Location` 개체를 정의 `Address` 하 고 `Description` 형식의 속성을 `string`, 및 `Position` 형식의 속성 [ `Position` ](xref:Xamarin.Forms.Maps.Position)합니다.

각 항목의 모양을 합니다 `IEnumerable` 컬렉션이 설정 하 여 정의 된를 `ItemTemplate` 속성을을 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 포함 하는 [ `Pin` ](xref:Xamarin.Forms.Maps.Pin) 데이터 바인딩할 개체 적절 한 속성입니다.

다음 스크린샷에서 표시 된 [ `Map` ](xref:Xamarin.Forms.Maps.Map) 표시를 [ `Pin` ](xref:Xamarin.Forms.Maps.Pin) 데이터 바인딩을 사용 하 여 컬렉션:

[![스크린샷 지도에 데이터 바인딩된 iOS 및 Android에서 핀](map-images/pins-itemssource.png "바인딩된 pin 데이터로 매핑합니다")](map-images/pins-itemssource-large.png#lightbox "핀이 바인딩된 데이터를 사용 하 여 매핑")

## <a name="related-links"></a>관련 링크

- [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)
- [맵 사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)
