---
title: "맵"
description: "Xamarin.Forms는 각 플랫폼에서 네이티브 지도 Api를 사용합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 2f0ad800ed1ab3a336f10dd4431e234ac4ff9675
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="map"></a>맵

_Xamarin.Forms는 각 플랫폼에서 네이티브 지도 Api를 사용합니다._

Xamarin.Forms.Maps 네이티브 맵을 Api를 사용 하 여 각 플랫폼에 있습니다. 이 사용자에 대 한 지도 신속 하 고 친숙 한 환경을 제공 하지만 각 플랫폼 특정 API 요구 사항을 충족 하는 데 몇 가지 구성 단계가 필요 않음을 의미 합니다.
한 번 구성 하면는 `Map` 컨트롤의 일반적인 코드의 다른 Xamarin.Forms 요소와 마찬가지로 작동 합니다.

* [초기화 매핑합니다](#Maps_Initialization) 사용 하 여- `Map` 시작 시 추가 초기화 코드가 필요 합니다.
* [플랫폼 구성](#Platform_Configuration) -각 플랫폼에서 실행 되도록 하는 지도 대 한 몇 가지 구성이 필요 합니다.
* [지도 사용 하 여 C#에서](#Using_Maps) -매핑하고 고정 하는 C#을 사용 하 여 표시 합니다.
* [XAML에서 지도 사용 하 여](#Using_Xaml) -XAML 사용 하 여 map을 표시 합니다.

지도 컨트롤에서 사용 된는 [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/) 는 아래에 표시 됩니다.

 [![지도 MobileCRM 샘플에서](map-images/maps-zoom-sml.png "지도 컨트롤 예제")](map-images/maps-zoom.png#lightbox "지도 컨트롤 예제")

만들어 맵 기능을 더욱 향상 시킬 수 있습니다는 [사용자 지정 렌더러를 매핑할](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)합니다.

<a name="Maps_Initialization" />

## <a name="maps-initialization"></a>맵 초기화

Xamarin.Forms는 응용 프로그램에 지도 추가할 때 **Xamarin.Forms.Maps** 되는 솔루션의 모든 프로젝트에 추가 해야 하는 별도 NuGet 패키지 합니다.
Android에서는이 또한에 종속이 되어 GooglePlayServices (다른 NuGet) Xamarin.Forms.Maps를 추가할 때 자동으로 다운로드 됩니다.

NuGet 패키지를 설치한 후 각 응용 프로그램 프로젝트의 일부 초기화 코드는 필요 *후* 는 `Xamarin.Forms.Forms.Init` 메서드를 호출 합니다. IOS에 대 한 다음 코드를 사용 합니다.

```csharp
Xamarin.FormsMaps.Init();
```

Android에서와 동일한 매개 변수를 전달 해야 `Forms.Init`:

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

유니버설 Windows 플랫폼 (UWP) 및는 WinRT (Windows 런타임)에 대 한 다음 코드를 사용 합니다.

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

각 플랫폼에 대해 다음 파일에이 호출을 추가 합니다.

-  **iOS** -AppDelegate.cs 파일에 `FinishedLaunching` 메서드.
-  **Android** -MainActivity.cs 파일에 `OnCreate` 메서드.
-  **WinRT 및 UWP** -MainPage.xaml.cs 파일에 `MainPage` 생성자입니다.

NuGet 패키지 추가 되 고 각 응용 프로그램 내에서 초기화 메서드가 호출 되 면 `Xamarin.Forms.Maps` 일반적인 PCL 또는 공유 프로젝트 코드에서 Api를 사용할 수 있습니다.

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>플랫폼 구성

지도 ֳ µ ֻ 전에 일부 플랫폼 추가 구성 단계가 필요 합니다.

### <a name="ios"></a>iOS

IOS 7에서 지도 컨트롤의 "정당한 작동" 수준으로 `FormsMaps.Init()` 호출이 수행 되었습니다.

IOS 8 두 개의 키에 추가할을 위해는 **Info.plist** 파일: [ `NSLocationAlwaysUsageDescription` ](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) 및 [ `NSLocationWhenInUseUsageDescription` ](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26)합니다. XML 표현은 다음과 같습니다.-업데이트 해야는 `string` 응용 프로그램에서 위치 정보를 사용 하는 방식을 반영 하는 값:

```xml
<key>NSLocationAlwaysUsageDescription</key>
    <string>Can we use your location</string>
<key>NSLocationWhenInUseUsageDescription</key>
    <string>We are using your location</string>
```

**Info.plist** 항목에 추가할 수도 있습니다 **소스** 편집 하는 동안 보기는 **Info.plist** 파일:

![IOS 8에 대 한 Info.plist](map-images/ios8-map-permissions.png "iOS 8 필수 Info.plist 입력")


### <a name="android"></a>Android

사용 하 여 [Google 맵 API v2](https://developers.google.com/maps/documentation/android/) Android에서 API 키를 생성 한 Android 프로젝트에 추가 해야 합니다.
Xamarin 문서의 지침에 따라 [Google 맵 API v2 키를 얻을](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)합니다.
이러한 지침을 따랐다면에서 API 키를 붙여는 **Properties/AndroidManifest.xml** 파일 (소스 보기 및 찾기/업데이트는 다음 요소가):

```xml
<meta-data android:name="com.google.android.maps.v2.API_KEY"
            android:value="AbCdEfGhIjKlMnOpQrStUvWValueGoesHere" />
```

올바른 API 키가 없는 지도 컨트롤에는 Android에서 회색 상자로 표시 됩니다.

> [!NOTE]
> 업로드 되는 응용 프로그램의 릴리스 버전에 서명 하는 데 사용 되는 키 저장소 파일을 사용 하 여 다른 키를 생성 해야 합니다. Google Play 스토어에 있습니다. 개발에 대 한 생성 된 키 및 디버깅이 실행 되지 않습니다 및 Google Play에서 다운로드 하는 앱을 위반한 것은 지도 표시 합니다. 또한 응용 프로그램의 경우는 키를 다시 생성 해야 **패키지 이름** 변경 합니다.

Android 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 적절 한 사용 권한을 사용할 수 있도록 해야 **옵션 > 빌드 > Android 응용 프로그램** 다음 틱 및:

* `AccessCoarseLocation`
* `AccessFineLocation`
* `AccessLocationExtraCommands`
* `AccessMockLocation`
* `AccessNetworkState`
* `AccessWifiState`
* `Internet`

이 중 일부를 아래 스크린샷에 나와 있습니다.

![Android 용 필수 권한](map-images/android-map-permissions.png "Android에 필요한 권한")

마지막 두는 응용 프로그램 맵 데이터를 다운로드 하려면 네트워크 연결이 필요 하기 때문에 필요 합니다. Android에 대 한 읽기 [권한을](http://developer.android.com/reference/android/Manifest.permission.html) 자세한 합니다.

### <a name="windows-runtime-and-universal-windows-platform"></a>Windows 런타임 및 유니버설 Windows 플랫폼

Windows 런타임 및 유니버설 Windows 플랫폼에서 맵을 사용 하는 권한 부여 토큰을 생성 해야 합니다. 자세한 내용은 참조 [지도 인증 키를 요청](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx) msdn 합니다.

에 다음 지정 하는 인증 토큰의 `FormsMaps.Init("AUTHORIZATION_TOKEN")` 메서드 호출에서 Bing 지도와 앱을 인증 합니다.

<a name="Using_Maps" />

## <a name="using-maps"></a>지도 사용 하 여

참조는 [MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) MobileCRM 샘플 코드에서 지도 컨트롤을 사용할 수 있는 방법을의 예입니다. 간단한 `MapPage` 클래스-본이 통지 같을 수 있습니다 하는 새 `MapSpan` 지도의 보기를 배치 하기 위해 만들어지는:

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

지도 내용 설정 하 여 변경할 수도 있습니다는 `MapType` 속성 일반도 지도 (기본값), 위성 이미지 또는 둘의 조합을 표시 합니다.

```csharp
map.MapType == MapType.Street;
```

유효한 `MapType` 값은:

-  하이브리드
-  위성
-  동 (기본값)


### <a name="map-region-and-mapspan"></a>지도 영역 및 MapSpan

위의 코드 조각에서와 같이 제공는 `MapSpan` 초기 보기를 설정 하는 지도 생성자에는 인스턴스 (지점 가운데 맞춤 및 확대/축소 수준을) 로드 될 때 지도 합니다. `MoveToRegion` 지도의 위치 또는 확대/축소 수준을 변경 하려면 다음 map 클래스에서 메서드를 사용할 수 있습니다. 두 가지 방법으로 새 수 `MapSpan` 인스턴스:

-  **MapSpan.FromCenterAndRadius()** -의 범위를 만드는 정적 메서드를 한 `Position` 지정 하는 `Distance` 합니다.
-  **새 MapSpan ()** -생성자를 사용 하는 `Position` 및 위도 및 경도 표시할의 degress 합니다.


위치를 변경 하지 않고 지도 확대/축소 수준을 변경 하려면 새 만듭니다 `MapSpan` 에서 현재 위치를 사용 하는 `VisibleRegion.Center` 지도 컨트롤의 속성입니다. A `Slider` (지도 컨트롤에서 직접 확대/축소 슬라이더의 값을 현재 업데이트할 수 없습니다) 있지만 다음과 같은 지도 확대/축소 컨트롤 데 사용할 수 없습니다.

```csharp
var slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) => {
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

 [![지도 확대/축소는](map-images/maps-zoom-sml.png "지도 컨트롤 확대/축소")](map-images/maps-zoom.png#lightbox "지도 컨트롤 확대/축소")

### <a name="map-pins"></a>지도 핀

위치와 함께 맵에 표시 될 수 있습니다 `Pin` 개체입니다.

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

 `PinType` pin (플랫폼)에 따라 렌더링 방식을 영향을 줄 수 있는 다음과 같은 값 중 하나로 설정할 수 있습니다.

-  제네릭
-  현재 위치
-  SavedPin
-  SearchResult


<a name="Using_Xaml" />

## <a name="using-xaml"></a>Xaml을 사용 하 여

이 코드 조각에 표시 된 대로 지도 Xaml 레이아웃에 배치 수도 수 있습니다.

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
            MapType="Hybrid"
        />
    </StackLayout>
</ContentPage>
```

`MapRegion` 및 `Pins` 사용 하 여 코드에서 설정할 수 있습니다는 `MyMap` 참조 (또는 지도 라는 무엇이 든). 추가적인 `xmlns` 네임 스페이스 정의 Xamarin.Forms.Maps 컨트롤을 참조 해야 합니다.

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

<a name="Summary" />

## <a name="summary"></a>요약

Xamarin.Forms.Maps 별도 NuGet는 Xamarin.Forms 솔루션의 각 프로젝트에 추가 해야 하는입니다. 추가 초기화 코드는 일부 잘 구성 단계는 iOS, Android, WinRT, 및 UWP로 필요 합니다.

구성 하면 규 단 몇 줄의 코드로 pin 표식을 사용 하 여 지도 렌더링 하 매핑합니다 API를 사용할 수 있습니다. 지도와 더 향상 될 수 있습니다는 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)합니다.


## <a name="related-links"></a>관련 링크

- [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)
- [맵 사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)
