---
title: "요약 장 28입니다. 위치 및 지도"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: d7a75ce0303030d53315b5e698fc604a910c969c
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-28-location-and-maps"></a>요약 장 28입니다. 위치 및 지도

Xamarin.Forms 지원는 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) 에서 파생 된 요소 `View`합니다. 맵을 사용 하는 데 참여할된 위에 표시 되는 특별 한 플랫폼 요구 사항 때문에 별도 어셈블리에 구현 된 **Xamarin.Forms.Maps**, 다른 네임 스페이스를 포함 하 고: `Xamarin.Forms.Maps`합니다.

## <a name="the-geographic-coordinate-system"></a>지리 좌표계

지리 좌표계 지구 처럼 구면 이동해 왔거나 거의 구면 개체 위치를 식별합니다. 좌표 둘 다로 이루어져는 *위도* 및 *경도* 각도으로 표현 됩니다.

좋은 원 호출는 `equator` 있는 지표의 축 개념적으로 확장에서 두 극 지방 중간입니다.

### <a name="parallels-and-latitude"></a>Parallels와 위도

각도 측정으로 북쪽 또는 남쪽 지구 부호를 사용 하 여 중심 적도 라고 같은 위도 줄 *parallels*합니다. 북부 및 남 극 지방에서 90도 적도에서 0도에서 이러한 다양 합니다. 규칙에 따라 적도 북쪽 latitude 양수 값 이며 적도 남쪽 하는 것은 음수 값입니다.

### <a name="longitude-and-meridians"></a>경도 및 자오선

남부 극을 북극에서 훌륭한 원 중 절반은 줄의 같은 경도 라고도 *자오선*합니다. 이들은 영국 그리니치를에서 자오선에 상대적입니다. 규칙에 따라 자오선의 동쪽 위 도와 경도 180도, 0도부터 양수 값 및 경도 자오선 서쪽은 음수 값으로 0도에서 &ndash;180도 합니다.

### <a name="the-equirectangular-projection"></a>등 장방형 도법이

모든 플랫 지도 지구의 왜곡을 소개합니다. 위도 및 경도의 모든 줄은 직선, 고 위도 및 경도 각도의 차이 같은 동일한 간격으로 지도에 대응 하는 경우 결과 경우는 *등 장방형 도법이*합니다. 이 맵에서 가로로 확대 하기 때문에 영역에서 극 지방까지 가깝게 왜곡 합니다.

### <a name="the-mercator-projection"></a>메 르 카 토르 도법

인기 있는 *메 르 카 토르 도법* 도 이러한 영역을 세로로 확장 하 여 수평 확장에 대 한 보정을 시도 합니다. 이 인해 지도 영역에서 극 지방 근처 표시 실제로 되지만 모든 로컬 영역 실제 영역와 매우 밀접 하 게 준수 하는 보다 훨씬 큰 것입니다.

### <a name="map-services-and-tiles"></a>서비스 맵 및 타일

서비스 맵 호출 메 르 카 토르 도법의 변형을 사용 하 여 `Web Mercator`합니다. 맵 서비스 위치 및 확대/축소 수준에 따라 클라이언트에 비트맵 타일을 제공 합니다.

## <a name="getting-the-users-location"></a>사용자의 위치를 가져오는 중

Xamarin.Forms는 `Map` 클래스는 사용자의 지리적 위치를 찾을 수 있는 기능을 포함 하지는 않지만 이것은 바람직한 경우가 많습니다 처리 해야 하므로 종속성 서비스 맵, 작업 하는 경우.

### <a name="the-location-tracker-api"></a>위치 추적기 API

[ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) 솔루션 위치 추적기 API에 대 한 코드를 포함 합니다. [ `GeographicLocation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs) 구조 위도 및 경도 캡슐화 합니다. [ `ILocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs) 인터페이스를 시작 및 일시 중지 위치 추적기 및 새 위치를 사용할 수 있는 경우의 이벤트 두 메서드를 정의 합니다.

#### <a name="the-ios-location-manager"></a>IOS 위치 관리자

IOS 구현의 `ILocationTracker` 는 [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs) ios 사용 하는 클래스 [ `CLLocationManager` ](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/)합니다.

#### <a name="the-android-location-manager"></a>Android 위치 관리자

Android 구현의 `ILocationTracker` 는 [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs) 에서 Android를 활용 하는 클래스 [ `LocationManager` ](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/) 클래스입니다.

#### <a name="the-windows-runtime-geo-locator"></a>Windows 런타임 지역 로케이터

Windows 런타임 구현 `ILocationTracker` 는 [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs) 에서 UWP 활용 하는 클래스 [ `Geolocator` ](https://msdn.microsoft.com/library/windows/apps/br225534)합니다.

### <a name="display-the-phones-location"></a>휴대폰의 위치를 표시 합니다.

[ **WhereAmI** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI) 샘플 위치 추적기를 사용 하 여 텍스트와 등 장방형 지도에 휴대폰의 위치를 표시 합니다.

### <a name="the-required-overhead"></a>필요한 오버 헤드

약간의 오버 헤드는 데 필요한 **WhereAmI** 위치 추적기를 사용 하도록 합니다. 모든 프로젝트에 첫 번째는 **WhereAmI** 솔루션에서 해당 프로젝트에 대 한 참조가 있어야 합니다. **Xamarin.FormsBook.Platform**, 및 각 **WhereAmI** 프로젝트 호출 해야 합니다는 `Toolkit.Init` 메서드.

위치 권한이 형식의 몇 가지 추가 플랫폼별 오버 헤드가 필요 합니다.

#### <a name="location-permission-for-ios"></a>IOS에 대 한 위치 사용 권한

Ios의 경우는 **info.plist** 파일에 해당 사용자의 위치를 가져오는 중 허용 여부를 묻는 질문의 텍스트를 포함 하는 항목에 포함 되어야 합니다.

#### <a name="location-permissions-for-android"></a>Android에 대 한 권한을 위치

Android 응용 프로그램 사용자의 위치를 가져오는 AndroidManifest.xml 파일에 ACCESS_FILE_LOCATION 권한이 있어야 합니다.

#### <a name="location-permissions-for-the-windows-runtime"></a>Windows 런타임에 대 한 사용 권한을 위치

Windows 또는 Windows Phone 응용 프로그램에 있어야는 `location` Package.appxmanifest 파일에서 장치 기능이 표시 됩니다.

## <a name="working-with-xamarinformsmaps"></a>Xamarin.Forms.Maps 작업

몇 가지 요구 사항이 사용에 관련 된 `Map` 클래스입니다.

### <a name="the-nuget-package"></a>NuGet 패키지

**Xamarin.Forms.Maps** NuGet 라이브러리 응용 프로그램 솔루션에 추가 해야 합니다. 버전 번호와 동일 해야는 **Xamarin.Forms** 현재 설치 된 패키지입니다.

### <a name="initializing-the-maps-package"></a>지도 패키지를 초기화합니다.

응용 프로그램 프로젝트를 호출 해야 합니다는 `Xamarin.FormsMaps.Init` 메서드 호출을 수행한 후 `Xamarin.Forms.Forms.Init`합니다.

### <a name="enabling-map-services"></a>서비스 맵 사용

때문에 `Map` 사용자의 위치를 가져올 수 있습니다, 응용 프로그램에는이 앞에서 설명한 방식으로 사용자에 대 한 권한을 얻어야 합니다.

#### <a name="enabling-ios-maps"></a>맵 iOS를 사용 하도록 설정

사용 하 여 iOS 응용 프로그램 `Map` info.plist 파일에서 두 줄 필요 합니다.

#### <a name="enabling-android-maps"></a>맵 Android를 사용 하도록 설정

인증 키가 Google 맵 서비스를 사용 하 여 필요 합니다. 이 키에 삽입 되는 **AndroidManifest.xml** 파일입니다. 또한는 **AndroidManifest.xml** 파일에 필요한 `manifest` 사용자의 위치를 가져오는에 관련 된 태그입니다.

#### <a name="enabling-windows-runtime-maps"></a>Windows Runtime을 사용 하면 맵

Windows 런타임 응용 프로그램 Bing 지도 사용 하기 위한 인증 키를 필요 합니다. 이 키에 대 한 인수로 전달 되는 `Xamarin.FormsMaps.Init` 메서드. 위치 서비스 응용 프로그램 사용 합니다.

### <a name="the-unadorned-map"></a>단순한 맵

[ **MapDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos) 샘플은 구성 되어는 [MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml) 파일 및 [MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs) 코드 숨김 파일을 다양 한 데모 프로그램으로의 이동을 허용 합니다.

[BasicMapPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml) 파일에서는 표시 하는 방법을 보여 줍니다.는 [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) 보기. 기본적으로 로마 도시를 표시 하지만 사용자가 지도 조작할 수 있습니다.

가로 및 세로 스크롤을 해제 하려면 설정는 [ `HasScrollEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.HasScrollEnabled/) 속성을 `false`합니다. 확대/축소 사용 하지 않으려면 설정 [ `HasZoomEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.HasZoomEnabled/) 를 `false`합니다. 이러한 속성은 모든 플랫폼에서 작동 하지 않을 수 있습니다.

### <a name="streets-and-terrain"></a>거리와 지형

설정 하 여 다른 유형의 지도 표시할 수 있습니다는 `Map` 속성 [ `MapType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.MapType/) 형식의 [ `MapType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapType/), 세 멤버가 포함 된 열거:

- [`Street`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Street/)기본값
- [`Satellite`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Satellite/)
- [`Hybrid`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Hybrid/)

[MapTypesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml) 파일 지도 유형을 선택 하려면 라디오 단추를 사용 하는 방법을 보여 줍니다. 이 사용 하는 [ `RadioButtonManager` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs) 클래스에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리와 클래스에 따라는 [MapTypeRadioButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml) 파일입니다.

### <a name="map-coordinates"></a>지도 좌표

프로그램을 가져올 수는 현재 영역 하는 `Map` 를 통해 표시 되는 [ `VisibleRegion` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.VisibleRegion/) 속성. 이 속성은 *하지* 바인딩 가능한 속성에 의해 지원 되므로 해당 용도 대 한 속성을 모니터링 하는 프로그램 타이머를 것 사용 해야이 변경 시기를 알리는 알림 메커니즘이 없습니다.

`VisibleRegion` 형식의 [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/), 4 개의 읽기 전용 속성을 사용 하 여 클래스:

- [`Center`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.Center/) 형식의 [`Position`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/)
- [`LatitudeDegrees`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.LatitudeDegrees/) 형식의 `double`, 지도의 표시 영역 너비를 나타내는
- [`LongitudeDegrees`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.LongitudeDegrees/) 형식의 `double`, 지도의 표시 영역 너비를 나타내는
- [`Radius`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.Radius/) 형식의 [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/), 지도 가장 큰 원형 영역의 크기를 나타내는

`Position` 및 `Distance` 구조를 모두 됩니다. `Position` 통해 설정 하는 두 개의 읽기 전용 속성을 정의 [ `Position` 생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Position.Position/p/System.Double/System.Double/):

- [`Latitude`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Position.Latitude/)
- [`Longitude`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Position.Longitude/)

`Distance` 메트릭 및 인치 단위로 간에 변환 하 여 장치에 관계 없이 거리를 제공 하는 데 사용 됩니다. A `Distance` 값을 여러 가지 방법으로 만들 수 있습니다.

- [`Distance` 생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Distance.Distance/p/System.Double/) 미터에서 거리로
- [`Distance.FromMeters`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromMeters/p/System.Double/) 정적 메서드
- [`Distance.FromKilometers`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromKilometers/p/System.Double/) 정적 메서드
- [`Distance.FromMiles`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromMiles/p/System.Double/) 정적 메서드

값이 세 가지 속성에서 사용할 수 있습니다.

- [`Meters`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Meters/) 형식의 `double`
- [`Kilometers`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Kilometers/) 형식의 `double`
- [`Miles`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Miles/) 형식의 `double`

[MapCoordinatesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml) 몇 가지 파일 들어 `Label` 표시 하기 위한 요소는 `MapSpan` 정보입니다. [MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs) 코드 숨김 파일 타이머를 사용 하 여 지도 조작 하는 사용자에 따라 업데이트 정보를 유지 합니다.

### <a name="position-extensions"></a>위치가 확장

명명 된이 책에 대 한 새 라이브러리 [ **Xamarin.FormsBook.Toolkit.Maps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps) 지도 관련 하지만 플랫폼 독립적인 형식을 포함 합니다. [ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) 클래스에는 `ToString` 방법을 `Position`, 두 사이의 거리를 계산 하는 방법 및 `Position` 값입니다.

### <a name="setting-an-initial-location"></a>초기 위치 설정

호출할 수 있습니다는 [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion/p/Xamarin.Forms.Maps.MapSpan/) 방식의 `Map` 프로그래밍 방식으로 지도에 위치 및 확대/축소 수준을 설정 하려면. 형식의 인수가 `MapSpan`합니다. 만들 수는 `MapSpan` 개체는 다음 중 하나를 사용 하 여:

- [`MapSpan` 생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.MapSpan.MapSpan/p/Xamarin.Forms.Maps.Position/System.Double/System.Double/) 와 `Position`, 고 위도 및 경도 범위
- [`MapSpan.FromCenterAndRadius`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius/p/Xamarin.Forms.Maps.Position/Xamarin.Forms.Maps.Distance/) 와 `Position` 및 radius

새로 만들 수 수 이기도 `MapSpan` 기존 메서드를 사용 하 여 프레젠테이션에서 [ `ClampLatitude` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.ClampLatitude/p/System.Double/System.Double/) 또는 [ `WithZoom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.WithZoom/p/System.Double/)합니다.

[WyomingPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml) 파일 및 [WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs) 코드 숨김 파일을 사용 하는 방법을 보여 줍니다는 `MoveToRegion` 와이오밍 상태를 표시 하는 메서드.

사용할 수 있습니다는 [ `Map` 생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Map.Map/p/Xamarin.Forms.Maps.MapSpan/) 와 `MapSpan` map의 위치를 초기화할 개체입니다. [XamarinHQPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml) 파일에서는 Xamarin의 본사 샌프란시스코에 표시 하는 XAML을이 작업을 수행 하는 방법을 보여 줍니다.

### <a name="dynamic-zooming"></a>동적 확대/축소

사용할 수는 `Slider` 동적으로 지도 확대/축소 합니다. [RadiusZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml) 파일 및 [RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs) 코드 숨김 파일에 따라 지도의 반지름을 변경 하는 방법을 보여는 `Slider` 값입니다.

[LongitudeZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml) 파일 및 [LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs) 코드 숨김 파일에는 Android에서 더 잘 작동 하는 다른 방법은 표시 되지만 Windows에서 잘 작동 하는 상호 배타적이 지 않습니다 플랫폼입니다.

### <a name="the-phones-location"></a>휴대폰의 위치

[ `IsShowingUser` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.IsShowingUser/) 속성 `Map` 약간으로 세 가지 플랫폼에서 다르게 작동는 [ShowLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml) 파일을 보여 줍니다.

- Ios에서 파란색 점 휴대폰의 위치를 나타냅니다. 하지만 있습니다에 수동으로 이동 해야 합니다.
- Android에서는 아이콘이 표시 됩니다는 푸시된 이동 휴대폰의 위치에 매핑
- UWP에는 iOS와 유사 하지만 경우에 따라 자동으로 위치로 이동

**MapDemos** Android 접근 방식에 따라 아이콘 기반 단추를 먼저 정의 모방 하기 위해 프로젝트 시도 [MyLocationButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml) 파일 및 [MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs) 코드 숨김 파일입니다.

[GoToLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml) 파일 및 [GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs) 휴대폰의 위치로 이동 하려면이 단추를 사용 하는 코드 숨김 파일입니다.

### <a name="pins-and-science-museums"></a>Pin과 과학 박물관

마지막으로 `Map` 클래스 정의 [ `Pins` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.Pins/) 형식의 속성이 `IList<Pin>`합니다. [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) 클래스 네 가지 속성을 정의 합니다.

- [`Label`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Label/) 형식의 `string`, 필수 속성
- [`Address`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Address/) 형식의 `string`, 선택적 읽을 주소
- [`Position`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Position/) 형식의 `Position`, pin 지도에 표시 되는 위치를 나타내는
- [`Type`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Type/) 형식의 [ `PinType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.PinType/), 사용 되지 않는 열거형

**MapDemos** 프로젝트 파일을 포함 [ScienceMuseums.xml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml), United States에 과학 박물관을 나열 하 고 [ `Locations` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs) 및 [ `Site` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs) 이 데이터를 역직렬화 하는 동안에 대 한 클래스입니다.

[ScienceMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml) 파일 및 [ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs) 맵에서 이러한 과학 박물관에 대 한 코드 숨김 파일 디스플레이 pin입니다. 사용자가 pin을 누르면 고 박물관에 대 한 웹 사이트의 주소와 표시 됩니다.

### <a name="the-distance-between-two-points"></a>두 점 사이의 거리

[ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) 클래스를 포함 한 [ `DistanceTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88) 메서드를 두 개의 지리적 위치 사이의 거리의 단순화 된 계산 합니다.

에 사용 됩니다는 [LocalMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml) 파일 및 [LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs) 코드 숨김 파일을 사용자의 위치는 박물관 거리도 표시 합니다.

[![로컬 박물관 페이지의 삼중 스크린샷](images/ch28fg28-small.png "위치로 거리")](images/ch28fg28-large.png#lightbox "위치로 거리")

프로그램에는 동적으로 지도 위치에 따라 pin 수를 제한 하는 방법을 보여 줍니다.

## <a name="geocoding-and-back-again"></a>지 오 코딩 하 고 다시 백

[ **Xamarin.Forms.Maps** ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) 도 포함 되어는 [ `Geocoder` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Geocoder/) 클래스와 [ `GetPositionsForAddressAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync/p/System.String/) 변환 하는 메서드 0 또는 그 밖의 지리적 위치 및 다른 방법으로 텍스트 주소 [ `GetAddressesForPositionAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync/p/Xamarin.Forms.Maps.Position/) 는 다른 곳에서 변환 하 합니다.

[GeocoderRoundTrip.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml) 파일 및 [GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs) 코드 숨김 파일에서이 기능을 보여 줍니다.



## <a name="related-links"></a>관련 링크

- [장 28 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [장 28 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [지도 컨트롤](~/xamarin-forms/user-interface/map.md)
