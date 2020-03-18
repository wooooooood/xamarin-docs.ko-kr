---
title: 28장의 요약 정보입니다. 위치 및 맵
description: 'Xamarin.Forms로 모바일 앱 만들기: 28장의 요약 정보입니다. 위치 및 맵'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 5dcd84536cc6d80deb753fc6fe57f9090f6b2dad
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "72697074"
---
# <a name="summary-of-chapter-28-location-and-maps"></a>28장의 요약 정보입니다. 위치 및 맵

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)

> [!NOTE]
> 이 페이지의 정보는 Xamarin.Forms가 책에 제공된 자료와 다른 영역을 표시합니다.

Xamarin.Forms는 `View`에서 파생되는 [`Map`](xref:Xamarin.Forms.Maps.Map) 요소를 지원합니다. 맵을 사용하려면 특수 플랫폼이 필요하기 때문에 별도의 어셈블리 **Xamarin.Forms.Maps**에서 구현되며, 다른 네임스페이스 `Xamarin.Forms.Maps`가 사용됩니다.

## <a name="the-geographic-coordinate-system"></a>지리 좌표계

지리 좌표계는 지구처럼 구형(또는 거의 구형) 개체의 위치를 식별합니다. 좌표는 *위도* 및 *경도*로 표현됩니다.

`equator`라고 하는 대원은 지구의 축이 개념적으로 확장하는 두 극 지방 사이에 있습니다.

### <a name="parallels-and-latitude"></a>위도선 및 위도

지구의 중심에서 적도의 북쪽 또는 남쪽을 측정한 각도를 동일한 위선으로 표시한 것을 *위도선*이라 합니다. 위도선의 범위는 적도에서는 0도이고 북극 및 남극에서는 90도입니다. 관례에 따라 적도 북쪽의 위도는 양수 값, 적도 남쪽의 위도는 음수 값으로 표시합니다.

### <a name="longitude-and-meridians"></a>경도 및 자오선

북극에서 남극까지 이어지는 대원의 절반은 동일한 경도 선이며 이를 *자오선*이라고 합니다. 이러한 자오선은 영국 그리니치의 본초 자오선과 비례합니다. 관례에 따라 본초 자오선의 동쪽 경도는 0~180도 사이의 양수 값으로, 본초 자오선의 서쪽 경도는 0~&ndash;180도 사이의 음수 값으로 표시합니다.

### <a name="the-equirectangular-projection"></a>등장방형 도법

지구를 표시하는 모든 평면 맵에는 왜곡이 있습니다. 모든 위도 및 경도 선이 직선이고 위도 및 경도 각도의 차이를 맵에서 동일한 거리로 표시하는 기법이 *등장방형 도법*입니다. 이 맵은 가로로 확장되기 때문에 극 지방과 가까운 영역이 왜곡됩니다.

### <a name="the-mercator-projection"></a>메르카토르 도법

널리 사용되는 *메르카토르 도법*은 가로 방향 확장을 보정하기 위해 이러한 영역을 세로 방향으로도 확장합니다. 그 결과로 극 지방 근처의 영역이 실제보다 훨씬 크게 표시되지만, 로컬 영역은 실제 영역과 거의 일치합니다.

### <a name="map-services-and-tiles"></a>맵 서비스 및 타일

맵 서비스에는 `Web Mercator`라고 하는 변형된 메르카토르 도법이 사용됩니다. 맵 서비스는 위치 및 확대/축소 수준에 따라 클라이언트에 비트맵 타일을 제공합니다.

## <a name="getting-the-users-location"></a>사용자 위치 가져오기

Xamarin.Forms `Map` 클래스에는 사용자의 지리적 위치를 가져오는 기능이 없지만 맵을 작업할 때 유용하게 사용되는 경우가 많기 때문에 종속성 서비스에서 이 기능을 처리해야 합니다.

> [!NOTE]
> Xamarin.Forms 애플리케이션에서 이 클래스 대신 Xamarin.Essentials에 포함된 [`Geolocation`](~/essentials/geolocation.md) 클래스를 사용할 수 있습니다.

### <a name="the-location-tracker-api"></a>위치 추적기 API

[**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) 솔루션에는 위치 추적기 API에 대한 코드가 포함되어 있습니다. [`GeographicLocation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs) 구조체는 위도 및 경도를 캡슐화합니다. [`ILocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs) 인터페이스는 위치 추적기를 시작하고 일시 중지하는 두 가지 메서드, 그리고 새 위치를 사용할 수 있을 때의 이벤트를 정의합니다.

#### <a name="the-ios-location-manager"></a>iOS 위치 관리자

iOS에서 구현되는 `ILocationTracker`는 iOS [`CLLocationManager`](xref:CoreLocation.CLLocationManager)를 사용하는 [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs) 클래스입니다.

#### <a name="the-android-location-manager"></a>Android 위치 관리자

Android에서 구현되는 `ILocationTracker`는 Android [`LocationManager`](xref:Android.Locations.LocationManager) 클래스를 사용하는 [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs) 클래스입니다.

#### <a name="the-uwp-geo-locator"></a>UWP 지역 로케이터

유니버설 Windows 플랫폼에서 구현되는 `ILocationTracker`는 UWP [`Geolocator`](/uwp/api/Windows.Devices.Geolocation.Geolocator)를 사용하는 [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs) 클래스입니다.

### <a name="display-the-phones-location"></a>휴대폰의 위치 표시

[**WhereAmI**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI) 샘플은 위치 추적기를 사용하여 휴대폰의 위치를 텍스트와 등장방형 맵에 표시합니다.

### <a name="the-required-overhead"></a>필요한 오버헤드

**WhereAmI**에서 위치 추적기를 사용하려면 약간의 오버헤드가 필요합니다. 우선, **WhereAmI** 솔루션의 모든 프로젝트에는 **Xamarin.FormsBook.Platform**의 해당 프로젝트에 대한 참조가 있어야 하고, 각 **WhereAmI** 프로젝트는 `Toolkit.Init` 메서드를 호출해야 합니다.

위치 권한 형식으로 약간의 플랫폼별 추가 오버헤드가 필요합니다.

#### <a name="location-permission-for-ios"></a>iOS의 위치 권한

iOS의 경우 **info.plist** 파일에는 사용자의 위치를 가져올 수 있도록 허용해 달라고 사용자에게 요청하는 질문 텍스트가 포함된 항목이 들어 있어야 합니다.

#### <a name="location-permissions-for-android"></a>Android의 위치 권한

사용자 위치를 가져오는 Android 애플리케이션의 AndroidManifest.xml 파일에는 ACCESS_FILE_LOCATION 권한이 있어야 합니다.

#### <a name="location-permissions-for-the-uwp"></a>UWP의 위치 권한

유니버설 Windows 플랫폼 애플리케이션은 Package.appxmanifest 파일에 표시된 `location` 디바이스 기능이 필요합니다.

## <a name="working-with-xamarinformsmaps"></a>Xamarin.Forms.Maps 사용

`Map` 클래스를 사용하려면 몇 가지 요구 사항을 충족해야 합니다.

### <a name="the-nuget-package"></a>NuGet 패키지

**Xamarin.Forms.Maps** NuGet 라이브러리를 애플리케이션 솔루션에 추가해야 합니다. 버전 번호는 현재 설치된 **Xamarin.Forms** 패키지와 동일해야 합니다.

### <a name="initializing-the-maps-package"></a>Maps 패키지 초기화

애플리케이션 프로젝트는 `Xamarin.Forms.Forms.Init`를 호출한 후 `Xamarin.FormsMaps.Init` 메서드를 호출해야 합니다.

### <a name="enabling-map-services"></a>맵 서비스 사용

`Map`이 사용자의 위치를 가져올 수 있으므로, 애플리케이션은 이 챕터의 앞부분에서 설명한 방식으로 사용자에 대한 권한을 획득해야 합니다.

#### <a name="enabling-ios-maps"></a>iOS 맵 사용

`Map`을 사용하는 iOS 애플리케이션의 info.plist 파일에 두 줄이 필요합니다.

#### <a name="enabling-android-maps"></a>Android 맵 사용

Google Map 서비스를 사용하려면 권한 부여 키가 필요합니다. 이 키는 **AndroidManifest.xml** 파일에 삽입됩니다. 또한 **AndroidManifest.xml** 파일은 사용자의 위치를 가져올 때 `manifest` 태그가 필요합니다.

#### <a name="enabling-uwp-maps"></a>UWP 맵 사용

유니버설 Windows 플랫폼 애플리케이션에서 Bing Maps를 사용하려면 권한 부여 키가 필요합니다. 이 키는 `Xamarin.FormsMaps.Init` 메서드에 인수로 전달됩니다. 또한 애플리케이션에서 위치 서비스를 사용하도록 설정해야 합니다.

### <a name="the-unadorned-map"></a>아무 것도 없는 맵

[**MapDemos**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos) 샘플은 다양한 데모 프로그램으로 이동할 수 있는 [MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml) 파일 및 [MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs) 코드 숨김 파일로 구성되어 있습니다.

[BasicMapPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml) 파일은 [`Map`](xref:Xamarin.Forms.Maps.Map) 보기를 표시하는 방법을 보여줍니다. 이 파일은 기본적으로 로마를 표시하지만, 사용자가 맵을 조작할 수 있습니다.

가로 및 세로 스크롤을 사용하지 않도록 설정하려면 [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled) 속성을 `false`로 설정합니다. 확대/축소를 사용하지 않도록 설정하려면 [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) 속성을 `false`로 설정합니다. 이러한 속성은 일부 플랫폼에서 작동하지 않을 수 있습니다.

### <a name="streets-and-terrain"></a>거리 및 지형

[`MapType`](xref:Xamarin.Forms.Maps.MapType) 형식의 `Map` 속성 [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)을 설정하여 다양한 형식의 맵을 표시할 수 있으며, 이 속성은 다음 세 가지 멤버가 있는 열거형입니다.

- [`Street`](xref:Xamarin.Forms.Maps.MapType.Street). 기본값입니다.
- [`Satellite`](xref:Xamarin.Forms.Maps.MapType.Satellite)
- [`Hybrid`](xref:Xamarin.Forms.Maps.MapType.Hybrid)

[MapTypesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml) 파일은 라디오 단추를 사용하여 맵 형식을 선택하는 방법을 보여줍니다. 이 파일은 [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 [`RadioButtonManager`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs) 클래스와 [MapTypeRadioButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml) 파일 기반의 클래스를 사용합니다.

### <a name="map-coordinates"></a>맵 좌표

프로그램은 `Map`에서 [`VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion) 속성을 통해 표시하는 현재 영역을 가져올 수 있습니다. 이 속성은 바인딩 가능한 속성에서 지원되지 *않으며*, 변경되었을 때 그 사실을 알리는 알림 메커니즘이 없기 때문에 속성을 모니터링하려는 프로그램은 알림 목적으로 타이머를 사용해야 합니다.

`VisibleRegion`은 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) 형식이며, 다음 네 가지 읽기 전용 속성이 있는 클래스입니다.

- [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center)([`Position`](xref:Xamarin.Forms.Maps.Position) 형식)
- 표시된 맵 영역의 높이를 나타내는 `double` 형식의 [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees)
- 표시된 맵 영역의 너비를 나타내는 `double` 형식의 [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees)
- 맵에 표시되는 가장 큰 원형 영역을 나타내는 [`Distance`](xref:Xamarin.Forms.Maps.Distance) 형식의 [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius)

`Position` 및 `Distance` 모두 구조체입니다. `Position`은 [`Position` 생성자](xref:Xamarin.Forms.Maps.Position.%23ctor(System.Double,System.Double))를 통해 설정되는 다음 두 가지 읽기 전용 속성을 정의합니다.

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)

`Distance`는 미터법과 영국식 단위 사이에서 변환하여 단위 독립적 거리를 제공하는 데 사용됩니다. `Distance` 값은 다음과 같은 여러 가지 방법으로 만들 수 있습니다.

- 미터법 거리를 사용하는 [`Distance` 생성자](xref:Xamarin.Forms.Maps.Distance.%23ctor(System.Double))
- [`Distance.FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters(System.Double)) 정적 메서드
- [`Distance.FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers(System.Double)) 정적 메서드
- [`Distance.FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles(System.Double)) 정적 메서드

값은 다음 세 가지 속성에서 얻을 수 있습니다.

- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters)(`double` 형식)
- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers)(`double` 형식)
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles)(`double` 형식)

[MapCoordinatesPage](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml) 파일에는 `MapSpan` 정보를 표시하는 데 사용되는 여러 `Label` 요소가 포함되어 있습니다. [MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs) 코드 숨김 파일은 사용자가 맵을 조작할 때 타이머를 사용하여 정보를 최신 상태로 유지합니다.

### <a name="position-extensions"></a>위치 확장

이 책의 새 라이브러리인 [**Xamarin.FormsBook.Toolkit.Maps**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps)에는 맵과 관련되어 있지만 플랫폼과는 독립적인 형식이 포함되어 있습니다. [`PositionExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) 클래스에는 `Position`에 대한 `ToString` 메서드와 두 `Position` 값 사이의 거리를 계산하는 메서드가 포함되어 있습니다.

### <a name="setting-an-initial-location"></a>초기 위치 설정

`Map`의 [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)) 메서드를 호출하여 맵의 위치 및 확대/축소 수준을 프로그래밍 방식으로 설정할 수 있습니다. 이 인수는 `MapSpan` 형식입니다. 다음 중 하나를 사용하여 `MapSpan` 개체를 만들 수 있습니다.

- `Position`, 위도 및 경도 범위를 사용하는 [`MapSpan` 생성자](xref:Xamarin.Forms.Maps.MapSpan.%23ctor(Xamarin.Forms.Maps.Position,System.Double,System.Double))
- `Position` 및 반지름을 사용하는 [`MapSpan.FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius(Xamarin.Forms.Maps.Position,Xamarin.Forms.Maps.Distance))

[`ClampLatitude`](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude(System.Double,System.Double)) 또는 [`WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom(System.Double)) 메서드를 사용하여 기존 항목에서 새 `MapSpan`을 만들 수도 있습니다.

[WyomingPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml) 파일 및 [WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs) 코드 숨김 파일은 `MoveToRegion` 메서드를 사용하여 와이오밍 상태를 표시하는 방법을 보여줍니다.

또는 [`Map` 생성자](xref:Xamarin.Forms.Maps.Map.%23ctor(Xamarin.Forms.Maps.MapSpan))와 `MapSpan` 개체를 사용하여 맵의 위치를 초기화할 수도 있습니다. [XamarinHQPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml) 파일은 이 작업을 전적으로 XAML로 수행하여 샌프란시스코에 Xamarin 본사를 표시하는 방법을 보여줍니다.

### <a name="dynamic-zooming"></a>동적 확대/축소

`Slider`를 사용하여 맵을 동적으로 확대/축소할 수 있습니다. [RadiusZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml) 파일 및 [RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs) 코드 숨김 파일은 `Slider` 값에 따라 맵의 반경을 변경하는 방법을 보여줍니다.

[LongitudeZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml) 파일 및 [LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs) 코드 숨김 파일은 Android에서 더 잘 작동하는 대안을 보여주지만, 두 방법 모두 Windows 플랫폼에서 제대로 작동하지 않습니다.

### <a name="the-phones-location"></a>휴대폰의 위치

`Map`의 [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser) 속성은 다음과 같이 [ShowLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml) 파일이 보여주는 것처럼 각 플랫폼에서 약간 다르게 작동합니다.

- iOS에서 파란색 점은 휴대폰의 위치를 나타내지만 수동으로 이동해야 합니다.
- Android에서는 푸시하면 맵을 휴대폰 위치로 이동하는 아이콘이 표시됩니다.
- UWP는 iOS와 유사하지만 종종 자동으로 휴대폰 위치로 이동합니다.

**MapDemos** 프로젝트는 먼저 [MyLocationButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml) 파일 및 [MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs) 코드 숨김 파일을 기반으로 아이콘 기반 단추를 정의하여 Android 방식을 모방하려고 시도합니다.

[GoToLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml) 파일 및 [GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs) 코드 숨김 파일은 이 단추를 사용하여 휴대폰의 위치로 이동합니다.

### <a name="pins-and-science-museums"></a>핀 및 과학 박물관

마지막으로 `Map` 클래스는 `IList<Pin>` 형식의 [`Pins`](xref:Xamarin.Forms.Maps.Map.Pins) 속성을 정의합니다. [`Pin`](xref:Xamarin.Forms.Maps.Pin) 클래스는 다음 네 가지 속성을 정의합니다.

- `string` 형식의 [`Label`](xref:Xamarin.Forms.Maps.Pin.Label). 필수 속성입니다.
- `string` 형식의 [`Address`](xref:Xamarin.Forms.Maps.Pin.Address). 사람이 읽을 수 있는 주소이며 선택 사항입니다.
- `Position` 형식의 [`Position`](xref:Xamarin.Forms.Maps.Pin.Position). 맵에서 핀이 표시되는 위치를 나타냅니다.
- [`PinType`](xref:Xamarin.Forms.Maps.PinType) 형식의 [`Type`](xref:Xamarin.Forms.Maps.Pin.Type). 사용되지 않는 열거형입니다.

**MapDemos** 프로젝트에는 미국의 과학 박물관을 나열하는 [ScienceMuseums.xml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml) 파일과 이 데이터를 역직렬화하기 위한 [`Locations`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs) 및 [`Site`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs) 클래스가 포함되어 있습니다.

[ScienceMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml) 파일 및 [ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs) 코드 숨김 파일은 이러한 과학 박물관을 나타내는 핀을 맵에 표시합니다. 사용자가 핀을 탭하면 해당 박물관의 주소와 웹 사이트가 표시됩니다.

### <a name="the-distance-between-two-points"></a>두 지점 사이의 거리

[`PositionExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) 클래스에는 두 지리적 위치 간의 거리를 단순화하는 [`DistanceTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88) 메서드가 포함되어 있습니다.

[LocalMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml) 파일 및 [LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs) 코드 숨김 파일에도 사용되어 사용자의 위치에서 박물관까지의 거리를 표시합니다.

[![현지 박물관 페이지의 세 가지 스크린샷](images/ch28fg28-small.png "한 위치까지의 거리")](images/ch28fg28-large.png#lightbox "한 위치까지의 거리")

이 프로그램은 맵의 위치를 기반으로 핀의 수를 동적으로 제한하는 방법도 보여줍니다.

## <a name="geocoding-and-back-again"></a>지오코딩 및 돌아가기

[**Xamarin.Forms.Maps**](xref:Xamarin.Forms.Maps) 어셈블리 역시 텍스트 주소를 0개 이상의 가능한 지리적 위치로 변환하는 [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync(System.String)) 메서드와 다른 방향으로 변환하는 또 다른 [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync(Xamarin.Forms.Maps.Position)) 메서드가 들어 있는 [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) 클래스를 포함하고 있습니다.

[GeocoderRoundTrip.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml) 파일 및 [GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs) 코드 숨김 파일은 이 기능을 보여줍니다.

## <a name="related-links"></a>관련 링크

- [28장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [28장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [Xamarin.Forms 맵](~/xamarin-forms/user-interface/map/index.md)
