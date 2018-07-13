---
title: 요약 28 장입니다. 위치 및 지도
description: 'Xamarin.Forms를 사용 하 여 모바일 앱 만들기: 28 장 요약 합니다. 위치 및 지도'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: a02239906f5a30c068cb7eebd31308ad188696b3
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998100"
---
# <a name="summary-of-chapter-28-location-and-maps"></a>요약 28 장입니다. 위치 및 지도

Xamarin.Forms 지원를 [ `Map` ](xref:Xamarin.Forms.Maps.Map) 에서 파생 된 요소 `View`합니다. 맵을 사용 하 여 관련 된 위에 표시 되는 특별 한 플랫폼 요구 사항으로 인해 별도 어셈블리에 구현 됩니다 **Xamarin.Forms.Maps**를 다른 네임 스페이스를 포함 하 고: `Xamarin.Forms.Maps`합니다.

## <a name="the-geographic-coordinate-system"></a>지리 좌표계

지리 좌표계 지구 처럼 구면 (또는 거의 구면) 개체의 위치를 식별합니다. 좌표 둘 다로 이루어져를 *위도* 하 고 *경도* 각도에 표시 합니다.

대권 호출을 `equator` 는 지구 축 개념적으로 확장 하는 두 극 지방의 중간입니다.

### <a name="parallels-and-latitude"></a>Parallels와 위도

각도로 측정 북쪽 또는 남쪽 지구의 눈금 표시의 가운데에서 적도 라고 같은 위도 줄 *parallels*합니다. 이러한 범위 중 북부 및 중남부 극 지방까지 90도에서 적도에서 0도입니다. 규칙에 따라 적도 북쪽의 위도 양수 값 및 음수 값은 적도 남쪽 해당 합니다.

### <a name="longitude-and-meridians"></a>경도 및 자오선

남부 극에 북극에서 훌륭한 원 중 절반은 같은 경도 줄 라고도 *자오선*합니다. 이들은 본초 자오선에서 영국 그리니치를 기준으로 합니다. 규칙에 따라 경도 본초 자오선의 동쪽 180도, 양수 값 0도에서 되며 본초 자오선 서쪽 경도 음수 값으로 0도에서 &ndash;180도 합니다.

### <a name="the-equirectangular-projection"></a>등 장방형 도법이

모든 플랫 맵 지구 왜곡을 소개합니다. 위도 및 경도의 모든 줄에는 직선, 및 지도에 같은 거리에 해당 하는 위도 및 경도 각도에서 같은 차이점을 하는 경우 결과 경우는 *등 장방형 도법이*합니다. 이 지도 수평으로 확장 하므로 극 지방까지 더 가까운 곳에 영역을 왜곡 합니다.

### <a name="the-mercator-projection"></a>메 르 카 토르 도법

널리 사용 되 *메 르 카 토르 도법* 또한 이러한 영역을 세로로 확장 하 여 수평 확장에 대 한 보정을 시도 합니다. 이 인해 영역 극 지방까지 거의 모든 지역 실제 영역을 사용 하 여 매우 밀접 하 게 준수 하지만, 실제로 보다 훨씬 더 큰 나타나는 맵.

### <a name="map-services-and-tiles"></a>지도 서비스 및 타일

지도 서비스 호출 메 르 카 토르 도법의 변형을 사용 하 여 `Web Mercator`입니다. 지도 서비스 위치 및 확대/축소 수준에 따라 클라이언트에 비트맵 타일을 제공 합니다.

## <a name="getting-the-users-location"></a>사용자의 위치를 가져오는 중

Xamarin.Forms `Map` 클래스에서 사용자의 지리적 위치를 가져올 수 있는 기능을 포함 하지는 않지만이 바람직한 경우가 많습니다 처리 해야 하므로 종속성 서비스 맵 작업 하는 경우.

### <a name="the-location-tracker-api"></a>위치 추적기 API

합니다 [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) 솔루션 위치 추적기 API에 대 한 코드를 포함 합니다. 합니다 [ `GeographicLocation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs) 구조 위도 및 경도 캡슐화 합니다. 합니다 [ `ILocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs) 인터페이스 시작 및 일시 중지 위치 추적기 및 이벤트를 새 위치로 사용할 수 있는 경우 두 개의 메서드를 정의 합니다.

#### <a name="the-ios-location-manager"></a>IOS 위치 관리자

IOS 구현의 `ILocationTracker` 되는 [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs) iOS 클래스는 사용 [ `CLLocationManager` ](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/)합니다.

#### <a name="the-android-location-manager"></a>Android 위치 관리자

Android 구현의 `ILocationTracker` 되는 [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs) Android 이용 하는 클래스 [ `LocationManager` ](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/) 클래스입니다.

#### <a name="the-windows-runtime-geo-locator"></a>Windows 런타임 지역 로케이터

Windows 런타임 구현의 `ILocationTracker` 은 [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs) UWP를 사용 하는 클래스 [ `Geolocator` ](https://msdn.microsoft.com/library/windows/apps/br225534)합니다.

### <a name="display-the-phones-location"></a>휴대폰의 위치를 표시 합니다.

합니다 [ **WhereAmI** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI) 샘플 위치 추적기를 사용 하 여 텍스트와 등 장방형 맵에서 휴대폰의 위치를 표시 합니다.

### <a name="the-required-overhead"></a>오버 헤드가 필요 합니다.

에 대 한 약간의 오버 헤드가 필요 **WhereAmI** 위치 추적기를 사용 하도록 합니다. 모든 프로젝트에 첫 번째는 **WhereAmI** 솔루션에서 해당 프로젝트에 대 한 참조가 있어야 합니다. **Xamarin.FormsBook.Platform**, 및 각 **WhereAmI** 프로젝트 호출 해야 합니다는 `Toolkit.Init` 메서드.

위치 권한이 형식의 몇 가지 추가 플랫폼별 오버 헤드가 필요합니다.

#### <a name="location-permission-for-ios"></a>IOS에 대 한 위치 사용 권한

Ios의 경우는 **info.plist** 파일에는 해당 사용자의 위치를 가져오는 허용 여부를 묻는 질문의 텍스트를 포함 하는 항목이 포함 되어야 합니다.

#### <a name="location-permissions-for-android"></a>Android에 대 한 위치 권한

사용자의 위치는 android 응용 프로그램에는 AndroidManifest.xml 파일에는 ACCESS_FILE_LOCATION 권한이 있어야 합니다.

#### <a name="location-permissions-for-the-windows-runtime"></a>Windows 런타임에 대 한 위치 권한

Windows 또는 Windows Phone 응용 프로그램에 있어야는 `location` 장치 기능이 Package.appxmanifest 파일을 표시 합니다.

## <a name="working-with-xamarinformsmaps"></a>Xamarin.Forms.Maps 사용

몇 가지 요구 사항을 사용에 관련 된 `Map` 클래스입니다.

### <a name="the-nuget-package"></a>NuGet 패키지

합니다 **Xamarin.Forms.Maps** NuGet 라이브러리 응용 프로그램 솔루션에 추가 해야 합니다. 버전 번호와 동일 해야 합니다 **Xamarin.Forms** 현재 설치 된 패키지입니다.

### <a name="initializing-the-maps-package"></a>맵 패키지를 초기화합니다.

응용 프로그램 프로젝트를 호출 해야 합니다 `Xamarin.FormsMaps.Init` 에 대 한 호출을 수행한 후 메서드 `Xamarin.Forms.Forms.Init`합니다.

### <a name="enabling-map-services"></a>지도 서비스를 사용 하도록 설정

때문에 `Map` 사용자의 위치를 가져올 수 있습니다, 응용 프로그램에는이 장 앞부분에서 설명한 방식으로 사용자에 대 한 권한을 얻어야 합니다.

#### <a name="enabling-ios-maps"></a>IOS를 사용 하도록 설정 하면 맵

사용 하 여 iOS 응용 프로그램 `Map` info.plist 파일에서 두 줄을 해야 합니다.

#### <a name="enabling-android-maps"></a>Android를 사용 하도록 설정 하면 맵

권한 부여 키는 Google 지도 서비스를 사용 하기 위해 필요 합니다. 이 키에 삽입 되는 **AndroidManifest.xml** 파일입니다. 또한 합니다 **AndroidManifest.xml** 파일에 필요한 `manifest` 사용자의 위치 가져오기에 관련 된 태그입니다.

#### <a name="enabling-windows-runtime-maps"></a>Windows 런타임 사용 하도록 설정 하면 맵

Windows 런타임 응용 프로그램을 Bing Maps를 사용 하 여 권한 부여 키가 필요 합니다. 이 키에 대 한 인수로 전달 되는 `Xamarin.FormsMaps.Init` 메서드. 위치 서비스에 대 한 응용 프로그램을 사용할 수도 있어야 합니다.

### <a name="the-unadorned-map"></a>표시 되지 않은 맵

[ **MapDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos) 이루어져 있습니다 샘플을 [MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml) 파일 및 [MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs) 코드 숨김 파일 다양 한 데모 프로그램을 이동할 수 있습니다.

합니다 [BasicMapPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml) 파일을 표시 하는 방법을 보여 줍니다 합니다 [ `Map` ](xref:Xamarin.Forms.Maps.Map) 보기. 마시는, 기본적으로 표시 되지만 사용자가 지도 조작할 수 있습니다.

가로 및 세로 스크롤을 사용 하지 않으려면 다음을 설정 합니다 [ `HasScrollEnabled` ](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled) 속성을 `false`입니다. 확대/축소를 사용 하지 않으려면 설정할 [ `HasZoomEnabled` ](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) 하려면 `false`합니다. 이러한 속성 모든 플랫폼에서 작동 하지 않을 수 있습니다.

### <a name="streets-and-terrain"></a>거리와 지형

설정 하 여 다양 한 유형의 지도 표시할 수 있습니다 합니다 `Map` 속성 [ `MapType` ](xref:Xamarin.Forms.Maps.Map.MapType) 형식의 [ `MapType` ](xref:Xamarin.Forms.Maps.MapType), 세 가지 멤버로 구성 된 열거형:

- [`Street`](xref:Xamarin.Forms.Maps.MapType.Street)기본값
- [`Satellite`](xref:Xamarin.Forms.Maps.MapType.Satellite)
- [`Hybrid`](xref:Xamarin.Forms.Maps.MapType.Hybrid)

합니다 [MapTypesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml) 파일에는 지도 유형을 선택 하려면 라디오 단추를 사용 하는 방법을 보여 줍니다. 것 활용 합니다 [ `RadioButtonManager` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs) 클래스를 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리 및 클래스를 기반으로 [MapTypeRadioButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml) 파일입니다.

### <a name="map-coordinates"></a>지도 좌표

프로그램을 가져오면 현재 영역은 합니다 `Map` 를 통해 표시 되는 [ `VisibleRegion` ](xref:Xamarin.Forms.Maps.Map.VisibleRegion) 속성입니다. 이 속성은 *되지* 바인딩 가능한 속성으로 지원 하도록 지정 하는 변경 된 경우, 속성을 모니터링 하고자 하는 프로그램은 해당 목적을 위해 타이머를 사용 아마도 알림 메커니즘이 없습니다.

`VisibleRegion` 유형의 [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan), 4 개의 읽기 전용 속성을 사용 하 여 클래스:

- [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center) 형식의 [`Position`](xref:Xamarin.Forms.Maps.Position)
- [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees) 형식의 `double`, 지도의 표시 영역 높이 나타내는
- [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees) 형식의 `double`, 지도의 표시 영역 너비를 나타내는
- [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius) 형식의 [ `Distance` ](xref:Xamarin.Forms.Maps.Distance), 지도 가장 큰 순환 영역 크기를 나타내는

`Position` 및 `Distance` 두 구조입니다. `Position` 통해 설정 하는 두 개의 읽기 전용 속성을 정의 합니다 [ `Position` 생성자](xref:Xamarin.Forms.Maps.Position.%23ctor(System.Double,System.Double)):

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)

`Distance` 미터법과 영국식 단위 간의 변환 하 여 거리 단위에 관계 없이 제공 됩니다. `Distance` 여러 가지 방법으로 값을 만들 수 있습니다.

- [`Distance` 생성자](xref:Xamarin.Forms.Maps.Distance.%23ctor(System.Double)) 미터에서 거리를 사용 하 여
- [`Distance.FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters(System.Double)) 정적 메서드
- [`Distance.FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers(System.Double)) 정적 메서드
- [`Distance.FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles(System.Double)) 정적 메서드

값은 세 가지 속성에서 사용할 수 있습니다.

- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters) 형식의 `double`
- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers) 형식의 `double`
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles) 형식의 `double`

합니다 [MapCoordinatesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml) 파일에는 몇 `Label` 표시에 대 한 요소는 `MapSpan` 정보입니다. 합니다 [MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs) 코드 숨김 파일 타이머를 사용 하 여 지도 조작 하는 사용자에 따라 업데이트 정보를 유지 합니다.

### <a name="position-extensions"></a>위치 확장

명명 된이 책에 대 한 새 라이브러리 [ **Xamarin.FormsBook.Toolkit.Maps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps) 맵 관련 있지만 플랫폼 독립적인 형식을 포함 합니다. [ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) 클래스에는 `ToString` 에 대 한 메서드 `Position`, 및 간 거리를 계산 하는 방법을 `Position` 값입니다.

### <a name="setting-an-initial-location"></a>초기 위치를 설정합니다.

호출할 수 있습니다 합니다 [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)) 메서드의 `Map` 프로그래밍 방식으로 지도에 위치 및 확대/축소 수준을 설정 하려면. 형식 인수가 `MapSpan`합니다. 만들 수는 `MapSpan` 다음 중 하나를 사용 하 여 개체:

- [`MapSpan` 생성자](xref:Xamarin.Forms.Maps.MapSpan.%23ctor(Xamarin.Forms.Maps.Position,System.Double,System.Double)) 사용 하 여는 `Position`, 및 위 도와 경도 범위
- [`MapSpan.FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius(Xamarin.Forms.Maps.Position,Xamarin.Forms.Maps.Distance)) 사용 하 여를 `Position` 및 radius

새 수 이기도 `MapSpan` 메서드를 사용 하 여 기존에서 [ `ClampLatitude` ](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude(System.Double,System.Double)) 하거나 [ `WithZoom` ](xref:Xamarin.Forms.Maps.MapSpan.WithZoom(System.Double))합니다.

[WyomingPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml) 파일 및 [WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs) 코드 숨김 파일을 사용 하는 방법에 설명 합니다 `MoveToRegion` 와이오밍 상태를 표시 하는 방법입니다.

사용할 수 있습니다 합니다 [ `Map` 생성자](xref:Xamarin.Forms.Maps.Map.%23ctor(Xamarin.Forms.Maps.MapSpan)) 사용 하 여를 `MapSpan` map의 위치를 초기화할 개체입니다. 합니다 [XamarinHQPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml) 파일 샌프란시스코에 Xamarin의 본사를 표시 하는 XAML에서 전체적으로이 작업을 수행 하는 방법을 보여 줍니다.

### <a name="dynamic-zooming"></a>동적 확대/축소

사용할 수는 `Slider` 동적 맵을 확대/축소 합니다. 합니다 [RadiusZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml) 파일 및 [RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs) 코드 숨김 파일에 따라 지도의 반지름을 변경 하는 방법을 표시 합니다 `Slider` 값입니다.

합니다 [LongitudeZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml) 파일 및 [LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs) 코드 숨김 파일에는 Android에서 더 잘 작동 하는 또 다른 방법은 표시 하지만 두 가지 방식에서 Windows 플랫폼입니다.

### <a name="the-phones-location"></a>휴대폰의 위치

[ `IsShowingUser` ](xref:Xamarin.Forms.Maps.Map.IsShowingUser) 의 속성 `Map` 약간으로 세 가지 플랫폼에서 다르게 작동 합니다 [ShowLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml) 파일을 보여 줍니다.

- IOS에서 파랑 점 휴대폰의 위치를 나타내는 있지만 있습니다에 수동으로 이동 해야 합니다.
- Android에서는 아이콘이 표시 됩니다 때 푸시 이동 휴대폰의 위치에 매핑
- UWP는 iOS와 유사 하지만 경우에 따라 자동으로 위치로 이동

합니다 **MapDemos** 프로젝트는 Android 접근 방식을 기반으로 하는 아이콘 기반 단추를 먼저 정의 함으로써 모방 하기 위해 시도 합니다 [MyLocationButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml) 파일 및 [MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs) 코드 숨김 파일입니다.

합니다 [GoToLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml) 파일 및 [GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs) 휴대폰의 위치로 이동 하려면이 단추를 사용 하는 코드 숨김 파일입니다.

### <a name="pins-and-science-museums"></a>Pin 및 과학 박물관

마지막으로, 합니다 `Map` 클래스 정의 [ `Pins` ](xref:Xamarin.Forms.Maps.Map.Pins) 형식의 속성 `IList<Pin>`합니다. 합니다 [ `Pin` ](xref:Xamarin.Forms.Maps.Pin) 클래스 네 가지 속성을 정의 합니다.

- [`Label`](xref:Xamarin.Forms.Maps.Pin.Label) 형식의 `string`, 필수 속성
- [`Address`](xref:Xamarin.Forms.Maps.Pin.Address) 형식의 `string`, 사람이 읽을 수는 선택적 주소
- [`Position`](xref:Xamarin.Forms.Maps.Pin.Position) 형식의 `Position`, pin 지도에 표시 되는 위치를 나타내는
- [`Type`](xref:Xamarin.Forms.Maps.Pin.Type) 형식의 [ `PinType` ](xref:Xamarin.Forms.Maps.PinType), 사용 되지 않는 열거형

합니다 **MapDemos** 프로젝트 파일이 [ScienceMuseums.xml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml), 미국에 과학 박물관을 나열 하는 및 [ `Locations` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs) 및 [ `Site` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs) 이 데이터를 역직렬화 하는 동안에 클래스입니다.

합니다 [ScienceMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml) 파일 및 [ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs) 맵에서 이러한 과학 박물관에 대 한 코드 숨김 파일 표시 pin입니다. 사용자가 pin을 누르면 고 박물관에 대 한 웹 사이트와 주소를 표시 합니다.

### <a name="the-distance-between-two-points"></a>두 점 사이의 거리

[ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) 클래스를 포함 한 [ `DistanceTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88) 간소화 된 두 지리적 위치 사이의 거리를 계산을 사용 하 여 메서드.

이 [LocalMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml) 파일 및 [LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs) 표시할 또한 거리는 박물관에 사용자의 위치에서 코드 숨김 파일:

[![삼중 로컬 박물관 페이지 스크린샷](images/ch28fg28-small.png "위치로 거리")](images/ch28fg28-large.png#lightbox "위치로 거리")

프로그램에는 동적으로 지도 위치에 따라 pin 수를 제한 하는 방법을 보여 줍니다.

## <a name="geocoding-and-back-again"></a>지 오 코딩 한 후 다시 돌아오기

[ **Xamarin.Forms.Maps** ](xref:Xamarin.Forms.Maps) 어셈블리 포함는 [ `Geocoder` ](xref:Xamarin.Forms.Maps.Geocoder) 클래스를 [ `GetPositionsForAddressAsync` ](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync(System.String)) 변환 하는 메서드 0 또는 보다 가능한 지리적 위치 및 다른 메서드는 텍스트 주소 [ `GetAddressesForPositionAsync` ](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync(Xamarin.Forms.Maps.Position)) 반대 방향에서으로 변환 하는 합니다.

합니다 [GeocoderRoundTrip.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml) 파일 및 [GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs) 코드 숨김 파일에서이 기능을 보여 줍니다.



## <a name="related-links"></a>관련 링크

- [28 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [28 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [지도 컨트롤](~/xamarin-forms/user-interface/map.md)
