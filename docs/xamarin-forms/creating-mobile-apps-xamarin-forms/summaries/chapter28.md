---
title: 28 장 요약입니다. 위치 및 지도
description: 'Xamarin.ios를 사용 하 여 Mobile Apps 만들기: 28 장 요약입니다. 위치 및 지도'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 5dcd84536cc6d80deb753fc6fe57f9090f6b2dad
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697074"
---
# <a name="summary-of-chapter-28-location-and-maps"></a>28 장 요약입니다. 위치 및 지도

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)

> [!NOTE]
> 이 페이지의 정보는 Xamarin.ios가 책에 제공 된 자료에서 달라져서 있는 영역을 표시 합니다.

Xamarin.ios는 `View`에서 파생 되는 [`Map`](xref:Xamarin.Forms.Maps.Map) 요소를 지원 합니다. Maps를 사용 하는 특별 한 플랫폼 요구 사항 때문에이는 별도의 어셈블리인 **xamarin.ios**에서 구현 되 고 다른 네임 스페이스를 포함 하는 `Xamarin.Forms.Maps`합니다.

## <a name="the-geographic-coordinate-system"></a>지리적 좌표계

지리적 좌표계는 지구와 같이 구형 (또는 거의 구형) 개체의 위치를 식별 합니다. 좌표는 각도로 표현 되는 *위도* 및 *경도* 로 구성 됩니다.

`equator` 라는 큰 원은 지구 축에서 개념적으로 확장 하는 두 극 지방 사이에 있습니다.

### <a name="parallels-and-latitude"></a>위 도선 및 위도

같은 *위도를 중심*으로 하는 지구의 중심에서 적도의 북쪽 또는 남부로 측정 된 각도입니다. 이 범위는 적도의 0도에서 북부 및 남부 극 지방의 90도 까지입니다. 규칙에 따라 적도의 위도 북부는 양수 값이 고 적도의 남부는 음수 값입니다.

### <a name="longitude-and-meridians"></a>경도 및 자오선

고가 중 북부에서 남 극으로 큰 원의 절반은 동일한 경도 ( *자오선*라고도 함)의 선입니다. 이는 그리니치 표준시 (미국 자오선)에 상대적입니다. 규칙에 따라 프라임 자오선의 경도 동부는 0도에서 180 사이의 양수 값이 고, 소수 자오선의 경도 서쪽은 0도에서 &ndash;180도까지 음수 값입니다.

### <a name="the-equirectangular-projection"></a>등 장방형 프로젝션

지구에 대 한 모든 기본 지도에는 왜곡이 도입 되었습니다. 위도 및 경도의 모든 줄이 직선 이며 위도 및 경도 각도의 차이가 지도의 동일한 거리에 해당 하는 경우 결과는 *등 장방형 프로젝션*입니다. 이 맵은 가로로 확장 되기 때문에 극 지방에 가까운 영역을 왜곡 합니다.

### <a name="the-mercator-projection"></a>Mercator 프로젝션

인기 있는 *Mercator 프로젝션* 은 이러한 영역을 세로로 확장 하 여 수평 확장을 보정 하려고 시도 합니다. 이로 인해 극 지방 근처의 영역이 실제로 표시 되는 것 보다 훨씬 큰 것으로 나타나지만 로컬 영역은 실제 영역과 거의 일치 합니다.

### <a name="map-services-and-tiles"></a>서비스 및 타일 매핑

지도 서비스는 `Web Mercator`이라는 변형 Mercator 프로젝션을 사용 합니다. 지도 서비스는 위치 및 확대/축소 수준에 따라 클라이언트에 비트맵 타일을 제공 합니다.

## <a name="getting-the-users-location"></a>사용자의 위치 가져오기

Xamarin.ios `Map` 클래스에는 사용자의 지리적 위치를 가져오는 기능이 포함 되어 있지 않지만 map을 사용 하 여 작업 하는 경우에는 종속성 서비스에서 처리 해야 하는 경우가 많습니다.

> [!NOTE]
> Xamarin Forms 응용 프로그램은 Xamarin.ios에 포함 된 [`Geolocation`](~/essentials/geolocation.md) 클래스를 대신 사용할 수 있습니다.

### <a name="the-location-tracker-api"></a>Location tracker API

[**Xamarin.ios 설명서**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) 에는 LOCATION tracker API에 대 한 코드가 포함 되어 있습니다. [`GeographicLocation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs) 구조는 위도 및 경도를 캡슐화 합니다. [`ILocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs) 인터페이스는 위치 추적기를 시작 하 고 일시 중지 하는 두 가지 방법 및 새 위치를 사용할 수 있을 때의 이벤트를 정의 합니다.

#### <a name="the-ios-location-manager"></a>IOS 위치 관리자

`ILocationTracker`의 iOS 구현은 iOS [`CLLocationManager`](xref:CoreLocation.CLLocationManager)를 사용 하는 [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs) 클래스입니다.

#### <a name="the-android-location-manager"></a>Android 위치 관리자

`ILocationTracker`의 Android 구현은 Android [`LocationManager`](xref:Android.Locations.LocationManager) 클래스를 사용 하는 [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs) 클래스입니다.

#### <a name="the-uwp-geo-locator"></a>UWP 지역 로케이터

`ILocationTracker`의 유니버설 Windows 플랫폼 구현은 UWP [`Geolocator`](/uwp/api/Windows.Devices.Geolocation.Geolocator)를 사용 하는 [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs) 클래스입니다.

### <a name="display-the-phones-location"></a>휴대폰의 위치를 표시 합니다.

[**WhereAmI**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI) 샘플은 location tracker를 사용 하 여 텍스트와 등 장방형 맵에 휴대폰의 위치를 표시 합니다.

### <a name="the-required-overhead"></a>필요한 오버 헤드

**WhereAmI** 가 location tracker를 사용 하려면 약간의 오버 헤드가 필요 합니다. 먼저 **WhereAmI** 솔루션의 모든 프로젝트에 **xamarin.ios**의 해당 프로젝트에 대 한 참조가 있어야 하 고 각 **WhereAmI** 프로젝트가 `Toolkit.Init` 메서드를 호출 해야 합니다.

위치 권한 형식의 일부 추가 플랫폼별 오버 헤드가 필요 합니다.

#### <a name="location-permission-for-ios"></a>IOS에 대 한 위치 권한

IOS의 경우 **info.plist** 파일은 사용자의 위치를 가져올 수 있도록 사용자에 게 요청 하는 질문의 텍스트가 포함 된 항목을 포함 해야 합니다.

#### <a name="location-permissions-for-android"></a>Android에 대 한 위치 권한

사용자 위치를 가져오는 Android 응용 프로그램은 AndroidManifest .xml 파일에 ACCESS_FILE_LOCATION 권한이 있어야 합니다.

#### <a name="location-permissions-for-the-uwp"></a>UWP에 대 한 위치 권한

유니버설 Windows 플랫폼 응용 프로그램은 appxmanifest.xml 파일에 표시 된 `location` 장치 기능을 포함 해야 합니다.

## <a name="working-with-xamarinformsmaps"></a>Xamarin.ios 사용

`Map` 클래스를 사용 하는 데는 몇 가지 요구 사항이 있습니다.

### <a name="the-nuget-package"></a>NuGet 패키지

응용 프로그램 솔루션에 **Xamarin.ios** NuGet 라이브러리를 추가 해야 합니다. 버전 번호는 현재 설치 되어 있는 **Xamarin. Forms** 패키지와 동일 해야 합니다.

### <a name="initializing-the-maps-package"></a>맵 패키지 초기화

응용 프로그램 프로젝트는 `Xamarin.Forms.Forms.Init`를 호출한 후 `Xamarin.FormsMaps.Init` 메서드를 호출 해야 합니다.

### <a name="enabling-map-services"></a>Map services 사용

`Map` 사용자의 위치를 가져올 수 있으므로 응용 프로그램은이 챕터의 앞부분에서 설명한 방식으로 사용자에 대 한 권한을 받아야 합니다.

#### <a name="enabling-ios-maps"></a>IOS 맵 사용

`Map`를 사용 하는 iOS 응용 프로그램은 info.plist 파일에 두 줄이 필요 합니다.

#### <a name="enabling-android-maps"></a>Android 맵 사용

Google Map services를 사용 하려면 인증 키가 필요 합니다. 이 키는 **Androidmanifest .xml** 파일에 삽입 됩니다. 또한 **Androidmanifest .xml** 파일에는 사용자의 위치를 가져오는 데 필요한 `manifest` 태그가 필요 합니다.

#### <a name="enabling-uwp-maps"></a>UWP 맵 사용

유니버설 Windows 플랫폼 응용 프로그램에는 Bing Maps를 사용 하기 위한 인증 키가 필요 합니다. 이 키는 `Xamarin.FormsMaps.Init` 메서드에 인수로 전달 됩니다. 응용 프로그램 에서도 위치 서비스를 사용 하도록 설정 해야 합니다.

### <a name="the-unadorned-map"></a>되지 않은 맵

[**Mapdemos**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos) 샘플은 다양한 데모 프로그램 탐색을 허용하는 [MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml) 파일 및 [MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs) 코드 숨김으로 구성되어 있습니다.

[Basicmappage](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml) 파일은 [`Map`](xref:Xamarin.Forms.Maps.Map) 보기를 표시 하는 방법을 보여 줍니다. 기본적으로 로마 도시는 표시 되지만 사용자가 지도를 조작할 수 있습니다.

가로 및 세로 스크롤을 사용 하지 않도록 설정 하려면 [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled) 속성을 `false`로 설정 합니다. 확대/축소를 사용 하지 않도록 설정 하려면 [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) 를 `false`로 설정 합니다. 이러한 속성은 일부 플랫폼에서 작동 하지 않을 수 있습니다.

### <a name="streets-and-terrain"></a>거리 및 지형

형식이 [`MapType`](xref:Xamarin.Forms.Maps.MapType) [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) `Map` 속성을 설정 하 여 다양 한 형식의 맵을 표시할 수 있습니다 .이 열거형의 멤버는

- [`Street`](xref:Xamarin.Forms.Maps.MapType.Street)기본값
- [`Satellite`](xref:Xamarin.Forms.Maps.MapType.Satellite)
- [`Hybrid`](xref:Xamarin.Forms.Maps.MapType.Hybrid)

[Maptype 페이지 .xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml) 파일은 라디오 단추를 사용 하 여 지도 유형을 선택 하는 방법을 보여 줍니다. [MapTypeRadioButton](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml) 파일을 기반으로 하는 클래스 및 [**xamarin.ios**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 [`RadioButtonManager`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs) 클래스를 사용 합니다.

### <a name="map-coordinates"></a>지도 좌표

프로그램은 `Map` [`VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion) 속성을 통해 표시 되는 현재 영역을 가져올 수 있습니다. 이 속성은 바인딩 가능한 속성에 의해 지원 *되지* 않으며 변경 된 시간을 나타내는 알림 메커니즘이 없기 때문에 속성을 모니터링 하려는 프로그램은 해당 목적으로 타이머를 사용 해야 합니다.

`VisibleRegion`는 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)형식 이며, 4 개의 읽기 전용 속성이 있는 클래스입니다.

- [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center)([`Position`](xref:Xamarin.Forms.Maps.Position) 형식)
- 맵의 표시 된 영역 높이를 나타내는 `double`형식의 [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees)
- 맵의 표시 된 영역 너비를 나타내는 `double`형식의 [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees) 입니다.
- 지도에 표시 되는 가장 큰 원형 영역의 크기를 나타내는 [`Distance`](xref:Xamarin.Forms.Maps.Distance)형식의 [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius)

`Position`와 `Distance`는 모두 구조체입니다. `Position` [`Position` 생성자](xref:Xamarin.Forms.Maps.Position.%23ctor(System.Double,System.Double))를 통해 설정 된 두 개의 읽기 전용 속성을 정의 합니다.

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)

`Distance`는 메트릭과 영어 단위를 변환 하 여 단위 독립적 거리를 제공 하기 위한 것입니다. `Distance` 값은 여러 가지 방법으로 만들 수 있습니다.

- 거리가 미터 인 [`Distance` 생성자](xref:Xamarin.Forms.Maps.Distance.%23ctor(System.Double))
- [`Distance.FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters(System.Double)) 정적 메서드
- [`Distance.FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers(System.Double)) 정적 메서드
- [`Distance.FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles(System.Double)) 정적 메서드

값은 다음 세 가지 속성에서 사용할 수 있습니다.

- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters)(`double` 형식)
- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers)(`double` 형식)
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles)(`double` 형식)

[MapCoordinatesPage](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml) 파일에는 `MapSpan` 정보를 표시 하기 위한 여러 `Label` 요소가 포함 되어 있습니다. [MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs) 코드를 사용 하는 파일은 사용자가 맵을 조작할 때 정보를 업데이트 하는 데 타이머를 사용 합니다.

### <a name="position-extensions"></a>위치 확장

이름이 [**xamarin.ios**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps) 인이 책의 새 라이브러리에는 맵 전용 및 플랫폼 독립적인 형식이 포함 되어 있습니다. [`PositionExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) 클래스에는 `Position`에 대 한 `ToString` 메서드와 두 `Position` 값 사이의 거리를 계산 하는 메서드가 있습니다.

### <a name="setting-an-initial-location"></a>초기 위치 설정

`Map`의 [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)) 메서드를 호출 하 여 지도에 위치 및 확대/축소 수준을 프로그래밍 방식으로 설정할 수 있습니다. 인수의 형식은 `MapSpan`입니다. 다음 중 하나를 사용 하 여 `MapSpan` 개체를 만들 수 있습니다.

- `Position`, 위도 및 경도 범위가 있는 [`MapSpan` 생성자](xref:Xamarin.Forms.Maps.MapSpan.%23ctor(Xamarin.Forms.Maps.Position,System.Double,System.Double))
- `Position` 및 radius를 사용 하 여 [`MapSpan.FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius(Xamarin.Forms.Maps.Position,Xamarin.Forms.Maps.Distance))

[`ClampLatitude`](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude(System.Double,System.Double)) 또는 [`WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom(System.Double))메서드를 사용 하 여 기존 항목에서 새 `MapSpan`를 만들 수도 있습니다.

[WyomingPage](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml) 파일 및 [WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs) 코드를 사용 하는 파일은 `MoveToRegion` 메서드를 사용 하 여 와이오밍의 상태를 표시 하는 방법을 보여 줍니다.

또는 [`Map` 생성자](xref:Xamarin.Forms.Maps.Map.%23ctor(Xamarin.Forms.Maps.MapSpan)) 를 `MapSpan` 개체와 함께 사용 하 여 맵의 위치를 초기화할 수 있습니다. [XamarinHQPage](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml) 파일은 완전히 xaml로이 작업을 수행 하 여 샌프란시스코에서 Xamarin의 사령부를 표시 하는 방법을 보여 줍니다.

### <a name="dynamic-zooming"></a>동적 확대/축소

`Slider`를 사용 하 여 지도를 동적으로 확대/축소할 수 있습니다. [RadiusZoomPage](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml) 파일 및 [RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs) 코드 숨김 파일에서는 `Slider` 값을 기준으로 지도의 반경을 변경 하는 방법을 보여 줍니다.

[LongitudeZoomPage](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml) 파일 및 [LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs) 코드 숨김 파일에는 Android에서 더 잘 작동 하는 다른 방법이 표시 되지만 두 방법 모두 Windows 플랫폼에서 제대로 작동 하지 않습니다.

### <a name="the-phones-location"></a>전화의 위치

`Map`의 [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser) 속성은 각 플랫폼에서 Showlocationpage로 약간 다르게 작동 [합니다. xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml) 파일은 다음을 보여 줍니다.

- IOS에서 파란색 점은 휴대폰의 위치를 나타내지만 수동으로 이동 해야 합니다.
- Android에서 푸시 될 때 지도를 전화의 위치로 이동할 때 아이콘이 표시 됩니다.
- UWP는 iOS와 유사 하지만 때때로 자동으로 이동 하는 위치로 이동 합니다.

**Mapdemos** 프로젝트는 먼저 [mylocationbutton .xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml) 파일 및 [MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs) 파일을 기반으로 아이콘 기반 단추를 정의 하 여 Android 접근 방식을 모방 하려고 합니다.

[GoToLocationPage](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml) 파일 및 [GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs) 코드 숨김이이 단추를 사용 하 여 휴대폰의 위치로 이동 합니다.

### <a name="pins-and-science-museums"></a>핀 및 과학 museums

마지막으로 `Map` 클래스는 `IList<Pin>`형식의 [`Pins`](xref:Xamarin.Forms.Maps.Map.Pins) 속성을 정의 합니다. [`Pin`](xref:Xamarin.Forms.Maps.Pin) 클래스는 네 가지 속성을 정의 합니다.

- 필수 속성인 `string`형식의 [`Label`](xref:Xamarin.Forms.Maps.Pin.Label)
- 사람이 읽을 수 있는 선택적 주소인 `string`형식의 [`Address`](xref:Xamarin.Forms.Maps.Pin.Address)
- 맵에 핀이 표시 되는 위치를 나타내는 `Position`형식의 [`Position`](xref:Xamarin.Forms.Maps.Pin.Position)
- 사용 되지 않는 열거형 [`PinType`](xref:Xamarin.Forms.Maps.PinType)형식의 [`Type`](xref:Xamarin.Forms.Maps.Pin.Type)

**Mapdemos** 프로젝트에는 [ScienceMuseums](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml)파일이 포함 되어 있으며,이 파일에는 미국에서 과학 museums을 나열 하 고,이 데이터를 deserialize 하기 위한 [`Locations`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs) 및 [`Site`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs) 클래스가 있습니다.

[ScienceMuseumsPage](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml) 파일과 [ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs) 파일은 지도에 이러한 과학 museums에 대 한 핀을 표시 합니다. 사용자는 pin을 탭 할 때 박물관의 주소와 웹 사이트를 표시 합니다.

### <a name="the-distance-between-two-points"></a>두 요소 사이의 거리입니다.

[`PositionExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) 클래스에는 두 지리적 위치 간의 거리를 단순화 하는 [`DistanceTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88) 메서드가 포함 되어 있습니다.

[LocalMuseumsPage](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml) 파일 및 [LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs) 코드를 사용 하 여 사용자의 위치에서 박물관 까지의 거리를 표시 합니다.

[![로컬 Museums 페이지의 삼중 스크린샷](images/ch28fg28-small.png "위치에 대 한 거리")](images/ch28fg28-large.png#lightbox "위치에 대 한 거리")

또한 프로그램은 지도의 위치를 기준으로 핀의 수를 동적으로 제한 하는 방법을 보여 줍니다.

## <a name="geocoding-and-back-again"></a>다시 지 오 코딩

또한 [**xamarin.ios**](xref:Xamarin.Forms.Maps) 어셈블리에는 텍스트 주소를 0 개 이상의 가능한 지리적 위치로 변환 하는 [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync(System.String)) 메서드를 포함 하는 [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) 클래스와 다른 메서드 [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync(Xamarin.Forms.Maps.Position)) 를 변환 하는 다른 메서드가 포함 되어 있습니다. 방향도.

[GeocoderRoundTrip](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml) 파일 및 [GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs) 코드 숨김이이 기능을 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [28 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [28 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [Xamarin.ios 맵](~/xamarin-forms/user-interface/map/index.md)
