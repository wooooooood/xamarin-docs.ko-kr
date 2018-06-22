---
title: 지도 API
ms.prod: xamarin
ms.assetid: C0589878-2D04-180E-A5B9-BB41D5AF6E02
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: fc16178a4068b2dcf22fc19047e0ef403e83633f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30773526"
---
# <a name="maps-api"></a>지도 API

지도 응용 프로그램을 사용 하는 뛰어나지만 응용 프로그램에 직접 지도 포함 하려는 경우가 있습니다. 맵 응용 프로그램을 기본 제공 외에도 Google 또한 제공 하는 [Android 용 네이티브 매핑 API](https://developers.google.com/maps/documentation/android/)합니다.
지도 API는 더 많이 제어할 매핑 환경 유지 하려는 경우에 적합 합니다. 지도 API와 함께 사용할 수 있는 사항은 다음과 같습니다.

-  Map의 관점을 프로그래밍 방식으로 변경 합니다.
-  추가 하 고 표식 사용자 지정 합니다.
-  오버레이 사용 하 여 map에 주석 지정 합니다.

지금은 사용 되지 않는 Google 맵 Android API v1 달리 Google 맵 Android API v 2의 일부인 [Google Play 서비스](http://developer.android.com/google/play-services/index.html)합니다.
따라서은 Google 맵 Android API Xamarin.Android 응용 프로그램의 사용 가능 상태가 되기 전에 일부 필수 필수 구성 요소를 충족 하기 위해 필요 합니다.


## <a name="google-maps-api-prerequisites"></a>Google는 API 필수 구성 요소를 맵

여러 항목이 지도 API를 사용 하려면 먼저 구성 해야 합니다. 포함 하 여:

-  Google Play 서비스 SDK 설치
-  Google Api를 사용 하 여 에뮬레이터 만들기
-  지도 API 키를 가져오려면
-  필요한 사용 권한을 지정 합니다.



### <a name="install-the-google-play-services-sdk"></a>Google Play 서비스 SDK 설치

Google Play 서비스는 Google +,-App 대금 청구, 및 지도 등의 다양 한 Google 기능을 활용 하려면 Android 응용 프로그램을 허용 하는 Google에서 기술입니다. 이러한 기능은 백그라운드 서비스에 포함 되어 있는 Android 장치에 액세스할 수 있는 [Google 재생 서비스 APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)합니다.

Android 응용 프로그램 Google Play 서비스 클라이언트 라이브러리를 통해 Google Play 서비스 상호 작용 합니다. 이 라이브러리는 인터페이스 및 맵과 같은 개별 서비스에 대 한 클래스를 포함합니다. 다음 다이어그램에서는 Android 응용 프로그램 및 Google Play 서비스 간의 관계를 보여 줍니다.

![Google 재생 서비스 APK 업데이트 Google Play 스토어를 보여 주는 다이어그램](maps-api-images/play-services-diagram.png)

Android 지도 API는 Google Play 서비스의 일부로 제공 됩니다.
Xamarin.Android 응용 프로그램에서 지도 API를 사용 하려면 먼저 Google 재생 서비스 SDK은 설치 하 고 연결 해야 합니다. Google 재생 서비스 SDK는 Android SDK Manager는 통해 설치 됩니다. 다음 스크린 샷에서 Google Play 서비스 클라이언트를 찾을 수 있습니다 Android SDK Manager의 위치를 보여 줍니다.

![Google Play 서비스 Extras Android SDK Manager 아래에 나타납니다.](maps-api-images/image01.png)

> [!NOTE]
> Google Play 서비스 APK는 모든 장치에 있을 수 있는 사용이 허가 된 제품입니다. 이 설치 되어 있지 않으면 장치에서 Google 맵 작동 하지 않습니다.


#### <a name="binding-google-play-services"></a>바인딩 Google Play 서비스

Google Play 서비스 클라이언트 라이브러리를 설치한 후 Xamarin.Android Java 바인딩 라이브러리에서 연결 해야 합니다. 두 가지 방법으로이를 위해.

-  **Google 재생 서비스 맵 NuGet 패키지를 사용 하 여** -이 가장 간단한 방법 (Xamarin.Android 4.8 에서만 사용할 수 또는 이상)입니다.
   설치는 [Xamarin Google 재생 서비스 맵 NuGet](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Maps); Google Play 서비스 종속성 패키지도 설치 합니다.
   이 가이드의 나머지 부분에서는이 접근 방법을 중점을 둡니다.

-  **Google Play 서비스 클라이언트 라이브러리를 수동으로 바인딩할** -은 더 복잡 한 접근 방식 및 Google 재생 서비스 SDK를 바인딩할 Xamarin.Android 4.4 또는 Xamarin.Android 4.6에 대 한 유일한 방법은이 있습니다.
   이 문서의 범위를 벗어납니다 Google Play 서비스 클라이언트 라이브러리를 수동으로 바인딩 하지만에서 수행 하는 방법의 예를 확인할 수 있습니다는 [지도 위치 데모 v3 샘플](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3) Github에서 합니다.


#### <a name="adding-the-google-play-services-map-package"></a>Google Play 서비스 맵 패키지 추가

Google 재생 서비스 맵 패키지를 추가 하려면 마우스 오른쪽 단추로 클릭는 **참조** 클릭 고 솔루션 탐색기에서 프로젝트의 폴더 **NuGet 패키지 관리...** :

![참조에서 NuGet 패키지 관리 상황에 맞는 메뉴 항목 표시 된 솔루션 탐색기](maps-api-images/image02.png)

열립니다는 **NuGet 패키지 관리자**합니다. 클릭 **찾아보기** 입력 **Xamarin Google 재생 맵** 검색 필드에 있습니다. 선택 **Xamarin.GooglePlayServices.Maps** 클릭 **설치**합니다. (이 패키지는 이전에 설치 되어 있던, 클릭 **업데이트**.):

[![선택한 Xamarin.GooglePlayServices.Maps 패키지와 함께 NuGet 패키지 관리자](maps-api-images/image03-sml.png)](maps-api-images/image03.png#lightbox)

다음과 같은 종속성 패키지도 설치 되어 있는지 확인 합니다.

-   **Xamarin.GooglePlayServices.Base**
-   **Xamarin.GooglePlayServices.Basement**
-   **Xamarin.GooglePlayServices.Tasks**



### <a name="create-an-emulator-with-google-apis"></a>Google Api를 사용 하 여 에뮬레이터 만들기

권장 되지는 않지만 Android 지도 API를 지원 하기 위해 에뮬레이터를 설정 하는 것이 같습니다. 에뮬레이터가 Google API 수준 17 (Android 4.2.2) 대상으로 하도록 구성 해야 이상. 다음 스크린 샷에 에뮬레이터 이미지 API 수준 19 구성 됩니다. 

![Android 에뮬레이터 관리자는 AVD API 수준 19에 대해 구성 된](maps-api-images/image04.png)



### <a name="obtain-a-google-maps-api-key"></a>Google 맵 API 키를 가져오려면

마지막 단계 (참고 레거시 Google 맵 v 1에서 API 키를 다시 사용할 수 없습니다) Google 맵 API 키를 가져오는 것입니다. API 키 Xamarin.Android를 사용 하는 방법에 대 한 정보를 참조 하십시오. [A Google 지도 API 키를 가져오는](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)합니다.
 


### <a name="specify-the-required-permissions"></a>필요한 사용 권한을 지정 합니다.

다음 사용 권한을 지정 해야 합니다는 **AndroidManifest.XML** Google 맵 Android API에 대 한:

-  **네트워크 상태에 대 한 액세스** &ndash; 지도 API 확인 하는 경우 지도 타일을 다운로드할 수 있어야 합니다.

-  **인터넷 액세스** &ndash; 지도 타일을 다운로드 하 고 API 액세스에 대 한 Google 재생 서버와 통신 하는 데 필요한는 인터넷에 연결 합니다.

-  **OpenGL ES v2** &ndash; 응용 프로그램 OpenGL ES v 2에 대 한 요구 사항을 선언 해야 합니다.

-  **Google 맵 API 키** &ndash; API 키를 사용 하 여 응용 프로그램은 등록 되 고 Google Play 서비스를 사용할 수 있는 권한이 있는지 확인 합니다. 참조 [Google 맵 API 키를 얻을](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md) 이 키에 대 한 세부 정보에 대 한 합니다.

-  **외부 저장소에 쓸** &ndash; Google 맵 Android API가 외부 저장소에 다운로드 한 타일을 캐시 합니다.

-  **Google 웹 기반 서비스에 대 한 액세스** &ndash; 응용 프로그램 다시 Android 지도 API는 Google의 웹 서비스에 액세스할 수 있는 권한이 필요 합니다.

-  **서비스 알림 재생 Google에 대 한 권한을** &ndash; 응용 프로그램에 Google Play 서비스에서 원격 알림을 받을 수 있는 권한을 부여 해야 합니다.

-  **위치 공급자에 대 한 액세스** &ndash; 하는 선택적 권한.
   허용 됩니다는 `GoogleMap` 지도에서 장치의 위치를 표시 하는 클래스입니다.


다음 코드 조각에 추가 해야 하는 설정의 한 예로 **AndroidManifest.XML**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionName="4.5" package="com.xamarin.docs.android.mapsandlocationdemo2" android:versionCode="6">
    <uses-sdk android:minSdkVersion="14" android:targetSdkVersion="17" />

    <!-- Google Maps for Android v2 requires OpenGL ES v2 -->
    <uses-feature android:glEsVersion="0x00020000" android:required="true" />

    <!-- We need to be able to download map tiles and access Google Play Services-->
    <uses-permission android:name="android.permission.INTERNET" />

    <!-- Allow the application to access Google web-based services. -->
    <uses-permission android:name="com.google.android.providers.gsf.permission.READ_GSERVICES" />

    <!-- Google Maps for Android v2 will cache map tiles on external storage -->
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

    <!-- Google Maps for Android v2 needs this permission so that it may check the connection state as it must download data -->
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <!-- Permission to receive remote notifications from Google Play Services -->
    <!-- Notice here that we have the package name of our application as a prefix on the permissions. -->
    <uses-permission android:name="<PACKAGE NAME>.permission.MAPS_RECEIVE" />
    <permission android:name="<PACKAGE NAME>.permission.MAPS_RECEIVE" android:protectionLevel="signature" />

    <!-- These are optional, but recommended. They will allow Maps to use the My Location provider. -->
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />


    <application android:label="@string/app_name">
        <!-- Put your Google Maps V2 API Key here. -->
        <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
        <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
    </application>
</manifest>
```


## <a name="the-googlemap-class"></a>GoogleMap 클래스

필수 구성 요소 처리, 응용 프로그램 개발을 시작 하 고 Android 지도 API를 사용 하는 데 시간이 있습니다. [GoogleMap](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap) 클래스는 표시 하 고 Android 용 Google 지도와 상호 작용 Xamarin.Android 응용 프로그램에서 사용할 기본 API입니다. 이 클래스에는 다음 책임이 있습니다.

-  Google 웹 서비스와 응용 프로그램을 인증 하려면 Google Play 서비스와 상호 작용 합니다.

-  다운로드 하 고, 캐싱 및 지도 타일을 표시 합니다.

-  와 같은 UI 컨트롤을 표시 하는 이동 및 확대/축소를 사용자에 게입니다.

-  지도에 표식 및 기 하 도형 그리기입니다.

`GoogleMap` 두 가지 방법 중 하나에 활동에 추가 됩니다.

-  **MapFragment** - [MapFragment](http://developer.android.com/reference/com/google/android/gms/maps/MapFragment.html) 호스트에 대 한 역할을 하는 특수 한 조각이 `GoogleMap` 개체입니다. `MapFragment` Android API 수준 12 이상이 필요 합니다.
   이전 버전의 Android צ ְ ײ는 [SupportMapFragment](http://developer.android.com/reference/com/google/android/gms/maps/SupportMapFragment.html)합니다.

-  **MapView** - [MapView](https://developer.xamarin.com/api/type/Android.GoogleMaps.MapView/) 에 대 한 호스트 역할을 수행할 수 있는 특수 한 보기 서브 클래스는 `GoogleMap` 개체입니다. 이 클래스의 사용자가을 활동 수명 주기 메서드 중 일부를 전달 해야는 `MapView` 클래스입니다.

각이 컨테이너를 노출 한 `Map` 속성의 인스턴스를 반환 하는 `GoogleMap`합니다. 기본 설정에 부여 해야는 [MapFragment](http://developer.android.com/reference/com/google/android/gms/maps/MapFragment.html) 개발자 수동으로 구현 해야 하는 양 상용구 코드는 감소 하는 간단한 API 이므로 클래스입니다.


### <a name="adding-a-mapfragment-to-an-activity"></a>활동에는 MapFragment 추가

다음 스크린 샷에서 매우 간단한의 예시 `MapFragment`:

[![지도 조각 표시 하는 장치의 스크린 샷](maps-api-images/image05-sml.png)](maps-api-images/image05.png#lightbox)

마찬가지로 다른 조각 클래스, 두 가지 추가 하시 `MapFragment` 활동에:

-   **선언적으로** - `MapFragment` 활동에 대 한 XML 레이아웃 파일을 통해 추가할 수 있습니다. 다음 XML 조각을 사용 하는 방법의 예가 나와 `fragment` 요소:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <fragment xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/map"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              class="com.google.android.gms.maps.MapFragment" />
    ```

-   **프로그래밍 방식으로** - `MapFragment` 다음에 설명한 대로 프로그래밍 방식으로 추가할 수 있습니다.

프로그래밍 방식으로 추가 하는 `MapFragment`, 활동 구현 해야 합니다는 `IOnMapReadyCallback` 인터페이스입니다. 때문에의 초기화는 `GoogleMap` 개체에 따라 Google Play와 통신 하는 API를 완료 하려면 다소 시간이 걸릴 수 있습니다, 응용 프로그램에 알립니다. 콜백을 제공 해야 때는 `GoogleMap` 준비 합니다.

첫째, 추가 `IOnMapReadyCallback` 에 `Activity` 클래스 선언 합니다.
예를 들어:

```csharp
public class MapWithMarkersActivity : Activity, IOnMapReadyCallback
```

다음의 `OnCreate` 메서드를 추가 `MapFragment` 다음 코드 예제와 같이 (의 `GoogleMapOptions` 클래스가이 가이드 뒷부분에서 설명):

```csharp
_mapFragment = FragmentManager.FindFragmentByTag("map") as MapFragment;
if (_mapFragment == null)
{
    GoogleMapOptions mapOptions = new GoogleMapOptions()
        .InvokeMapType(GoogleMap.MapTypeSatellite)
        .InvokeZoomControlsEnabled(false)
        .InvokeCompassEnabled(true);

    FragmentTransaction fragTx = FragmentManager.BeginTransaction();
    _mapFragment = MapFragment.NewInstance(mapOptions);
    fragTx.Add(Resource.Id.map, _mapFragment, "map");
    fragTx.Commit();
}
_mapFragment.GetMapAsync(this);
```

A `GoogleMap` 를 사용 하 여 획득 해야 `GetMapAsync`끝 이전 코드 예제에 표시 된 것 처럼 &ndash; 지도 시스템과 뷰에 자동으로 초기화 합니다. (참고가이 메서드를 사용 하지 않는 `await` / `async` 의미 체계 &ndash; 는 `Async` Android에서 동작을 구현 합니다.) 경우는 `GoogleMap` 개체 준비 되 면 Android 앱의 호출 `OnMapReady` 메서드 (의 일부로 구현 해야 하는 `IOnMapReadyCallback` 인터페이스). 예를 들어:

```csharp
public void OnMapReady (GoogleMap map)
{
    _map = map;
}
```

위의 코드 예제는 `OnMapReady` 콜백 초기화는 `_map` 는에 만든 변수 `GoogleMap` 개체입니다.

이 결과 사용 하는 방법의 예를 들어 때 `OnResume` 가 있는지 확인할 수 호출 `_map` null이 아닌 합니다. 경우 `_map` 로 설정 되는 `GoogleMap` 개체 `OnResume` 마커를 추가 하 고 지정 된 경도 위도를 해당 카메라를 이동 하는 데 메서드를 호출할 수 있습니다. 전체 코드 예제를 보려면 [SimpleMapDemo](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3/SimpleMapDemo)합니다.



### <a name="map-types"></a>맵 유형

Google 맵 API에서 사용할 수 있는 5 개의 서로 다른 유형의 지도 됩니다.

-  **보통** -이 기본 지도 형식입니다. 도 중요 한 자연 기능 (예: 건물과 브리지) 관련 옮겨 일부 포인트에 따라 표시합니다.

-  **위성** -이 맵에서 위성 사진 보여 줍니다.

-  **하이브리드** -이 맵은 위성 사진 보여주며,도 매핑합니다.

-  **지형** -일부도 지형적 특징을 주로 보여 줍니다.

-  **None** -이 맵은 모든 타일을 로드 하지 않습니다, 빈 그리드가로 렌더링 됩니다.


아래 이미지에서는 다양 한 유형의 지도의 왼쪽에서 오른쪽 (보통, 하이브리드, 지형)에서 중 3 개를 보여 줍니다.

[![스크린 샷입니다 매핑할 3: 보통, 하이브리드 및 지형](maps-api-images/map-types-sml.png)](maps-api-images/map-types.png#lightbox)

`GoogleMap.MapType` 속성 설정 하거나 표시 하는 지도 유형을 변경 하는 데 사용 됩니다. 다음 코드 조각에는 위성 지도 표시 하는 방법을 보여 줍니다.

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MapType = GoogleMap.MapTypeSatellite;
}
```


### <a name="googlemap-properties"></a>GoogleMap 속성

`GoogleMap` 기능 및 지도의 모양을 제어할 수 있는 몇 가지 속성을 정의 합니다. 초기 상태를 구성 하는 한 가지 방법은 `GoogleMap` 전달 하는 한 [GoogleMapOptions](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMapOptions.html) 만들 때 개체는 `MapFragment`합니다. 다음 코드 조각을 사용 하 여 한 가지 예는 한 `GoogleMapOptions` 만들 때 개체는 `MapFragment`:

```csharp
GoogleMapOptions mapOptions = new GoogleMapOptions()
    .InvokeMapType(GoogleMap.MapTypeSatellite)
    .InvokeZoomControlsEnabled(false)
    .InvokeCompassEnabled(true);

FragmentTransaction fragTx = FragmentManager.BeginTransaction();
_mapFragment = MapFragment.NewInstance(mapOptions);
fragTx.Add(Resource.Id.map, _mapFragment, "map");
fragTx.Commit();
```

구성 하는 다른 방법은 `GoogleMap` 개체는 값을 설정 하는 [UiSettings](http://developer.android.com/reference/com/google/android/gms/maps/UiSettings.html) 지도 개체의 속성입니다. 구성 하는 방법을 보여 주는 하는 다음 코드 예제는 `GoogleMap` 컴퍼스 및 확대/축소 컨트롤 표시 하려면:

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.UiSettings.ZoomControlsEnabled = true;
    _map.UiSettings.CompassEnabled = true;
}
```


## <a name="interacting-with-the-map"></a>지도와 상호 작용

Android 지도 API API의 관점을 변경 하거나, 표식을 추가 하거나, 사용자 지정 오버레이 배치 기 하 도형 그리기 작업을 허용 하는 제공 합니다. 이 섹션에는 일부의 Xamarin.Android 이러한 작업을 수행 하는 방법을 설명 합니다.

### <a name="changing-the-viewpoint"></a>관점 변경

지도 메 르 카 토르 도법에 따라 화면에 플랫 평면으로 모델링 됩니다. 지도 보기는의 *카메라* 이 평면에 수직으로 찾고 있습니다. 카메라의 위치를 변경 하 여 위치, 확대/축소, 기울기, 영향을 주지 제어할 수 있습니다. [CameraUpdate](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdate) 클래스 카메라 위치를 이동 하는 데 사용 됩니다. `CameraUpdate` 개체가 직접 인스턴스화될, 지도 API 대신 제공 하는 [CameraUpdateFactory](http://developer.android.com/reference/com/google/android/gms/maps/CameraUpdateFactory.html) 클래스입니다.

한 번는 `CameraUpdate` 개체가 생성 되었음을를 매개 변수로 전달 된 [GoogleMap.MoveCamera](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.html#moveCamera(com.google.maps.CameraUpdate)) 또는 [GoogleMap.AnimateCamera](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.html#animateCamera(com.google.maps.CameraUpdate)) 메서드. `MoveCamera` 메서드 동안 즉시 맵 업데이트는 `AnimateCamera` 메서드 부드러운, 애니메이션 전환을 제공 합니다.

이 코드 조각은 사용 하는 방법의 간단한 예제는 `CameraUpdateFactory` 만들려는 `CameraUpdate` 하는 map의 확대/축소 수준을 1 씩:

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MoveCamera(CameraUpdateFactory.ZoomIn());
}
```

지도 API를 제공는 [CameraPosition](http://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.html) 카메라 위치에 대 한 가능한 값의 모든 집계입니다. 이 클래스의 인스턴스를 지정할 수는 [CameraUpdateFactory.NewCameraPosition](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdateFactory#newCameraPosition(com.google.android.gms.maps.model.CameraPosition)) 을 반환 하는 메서드는 `CameraUpdate` 개체입니다. 지도 API도 포함 되어는 [CameraPosition.Builder](http://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.Builder.html) 작성용 fluent API를 제공 하는 클래스 `CameraPosition` 개체입니다.
다음 코드 조각을 만드는 예를 보여 줍니다.는 `CameraUpdate` 에서 `CameraPosition` 하는 카메라 위치에서 변경 하는 데 사용 된 `GoogleMap`:

```csharp
LatLng location = new LatLng(50.897778, 3.013333);
CameraPosition.Builder builder = CameraPosition.InvokeBuilder();
builder.Target(location);
builder.Zoom(18);
builder.Bearing(155);
builder.Tilt(65);
CameraPosition cameraPosition = builder.Build();
CameraUpdate cameraUpdate = CameraUpdateFactory.NewCameraPosition(cameraPosition);

MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MoveCamera(cameraUpdate);
}
```

이전 코드 조각에서 지도에서 특정 위치도 표시 됩니다는 [LatLng](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/LatLng) 클래스입니다. 확대/축소 수준은 18로 설정 됩니다. 영향을 주지 북부에서 시계 방향으로 나침반 측정입니다. 속성 보기 각도 제어 되며 기울기 세로에서 25도 각도 지정 합니다. 다음 스크린 샷에 표시 된 `GoogleMap` 앞의 코드를 실행 한 후:

[![기운는와 지정된 된 위치를 보여 주는 예제 Google 맵 시야각](maps-api-images/image06-sml.png)](maps-api-images/image06.png#lightbox)


### <a name="drawing-on-the-map"></a>지도에 그리기

Android 지도 API를 지도에 다음 항목을 그리기 위한 API를 제공 합니다.

-  **표식** -이 지도에 단일 위치를 식별 하는 데 사용 되는 특수 아이콘이 있습니다.

-  **오버레이** -위치 또는 지도에서 영역의 컬렉션을 식별 하는 데 사용할 수 있는 이미지입니다.

-  **선, 다각형 및 원** -이 활동 지도에 셰이프를 추가할 수 있는 Api입니다.


#### <a name="markers"></a>Markers

지도 API를 제공는 [표식](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Marker) 지도에 단일 위치에 대 한 데이터를 모두 캡슐화 하는 클래스입니다. 기본적으로 Google 맵에서 제공 되는 표준 아이콘을 사용 합니다. 되기 표식 모양을 사용자 지정 하 고 사용자가 클릭할 때 응답할 수 있습니다.


##### <a name="adding-a-marker"></a>표식을 추가

표시자 지도 추가 하려면 새 [MarkerOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions) 개체와 호출 후의 [AddMarker](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addMarker(com.google.android.gms.maps.model.MarkerOptions)) 메서드를는 `GoogleMap` 인스턴스. 이 메서드는 반환 된 [표식](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Marker) 개체입니다.

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");
    _map.AddMarker(marker1);
}
```

표식의 제목에 표시 됩니다는 *정보 창의* 사용자 표식에 대가 하는 경우. 다음 스크린샷에이 표식 모양을 보여 줍니다.

[![예제 Google 맵 마커 및 Vimy 홈 음각 대 한 정보 창](maps-api-images/image07-sml.png)](maps-api-images/image07.png#lightbox)


##### <a name="customizing-a-marker"></a>표식을 사용자 지정

호출 하 여는 표식으로 사용 되는 아이콘을 사용자 지정할 수는 `MarkerOptions.InvokeIcon` 메서드 표식 지도에 추가 하는 경우.
이 메서드는 [BitmapDescriptor](http://developer.android.com/reference/com/google/android/gms/maps/model/BitmapDescriptor.html) 아이콘을 렌더링 하는 데 필요한 데이터를 포함 하는 개체입니다. [BitmapDescriptorFactory](https://developer.android.com/reference/com/google/android/gms/maps/model/BitmapDescriptorFactory.html) 만들기를 간소화 하기 위해 몇 가지 도우미 메서드를 제공 하는 클래스는 `BitmapDescriptor`합니다. 다음 목록에는 이러한 메서드의 일부 소개:

-   `DefaultMarker(float colour)` &ndash; 기본 Google 맵 마커를 사용 하 여 하지만 컬러 변경 합니다.

-   `FromAsset(string assetName)` &ndash; 자산 폴더에서 지정된 된 파일에서 사용자 지정 아이콘을 사용 합니다.

-   `FromBitmap(Bitmap image)` &ndash; 지정한 비트맵에서 아이콘으로 사용 합니다.

-   `FromFile(string fileName` &ndash; 지정된 된 경로에 파일에서 사용자 지정 아이콘을 만듭니다.

-   `FromResource(int resourceId)` &ndash; 지정된 된 리소스에서 사용자 지정 아이콘을 만듭니다.

다음 코드 조각 녹청 coloured 기본 마커를 만드는 예를 보여 줍니다.

```csharp
mapFrag.GetMapAsync(this);
...
if (_map != null)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");
    markerOpt1.InvokeIcon(BitmapDescriptorFactory.DefaultMarker (BitmapDescriptorFactory.HueCyan));
    _map.AddMarker(marker1);
}
```


#### <a name="info-windows"></a>Windows 정보

*정보 windows* 특정 마커를 탭 하는 경우 사용자에 게 정보를 표시 하려면 해당 팝업에는 특별 한 창이 있습니다. 기본적으로 표식의 제목의 내용을 정보 창에 표시 됩니다. 제목 할당 되지 않은 경우 정보 창이 표시 됩니다. 한 번에 하나만 정보 창 표시 될 수 있습니다.

구현 하 여 정보 창의 사용자 지정할 수는 [GoogleMap.IInfoWindowAdapter](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.InfoWindowAdapter) 인터페이스입니다. 이 인터페이스에서 두 가지 중요 한 메서드는

-  `public View GetInfoWindow(Marker marker)` &ndash; 이 메서드는 표식에 대 한 사용자 지정 정보 창의 가져오려는 합니다. 반환 하는 경우 `null` , 기본 창 렌더링이 사용 됩니다. 이 메서드가 뷰를 반환 하는 경우 해당 보기 정보 창 프레임 안에 배치 됩니다.

-  `public View GetInfoContents(Marker marker)` &ndash; 이 메서드 GetInfoWindow 반환 하는 경우에 호출할 수 `null` 합니다. 이 메서드가 반환할 수는 `null` 정보 창의 내용에 대 한 기본 렌더링 사용할 수 있으면 값입니다. 그렇지 않으면이 메서드 정보 창의 내용 보기를 반환 해야 합니다.

정보 창 라이브 뷰입니다-대신 Android 보기 정적 비트맵으로 변환 되며 하 고 이미지에 표시 합니다. 즉, 정보 창의 터치 이벤트 또는 제스처에 응답할 수 없습니다 및 그 자동으로 자동으로 업데이트 됩니다. 정보 창을 업데이트 하려면를 호출할 필요는 [GoogleMap.ShowInfoWindow](http://developer.android.com/reference/com/google/android/gms/maps/model/Marker.html#showInfoWindow()) 메서드.

다음 그림에서는 일부 사용자 지정 된 정보 창의 몇 가지 예를 보여 줍니다. 왼쪽 이미지에 내용을 오른쪽에 있는 이미지의 창 및 사용자 지정 된 내용에 있을 때 사용자 지정:

![멜버른, 아이콘 및 인구를 포함 하 여에 대 한 예제 표식 기간입니다. 오른쪽 창에 모서리가 둥글게 변합니다.](maps-api-images/marker-infowindows.png)


#### <a name="ground-overlays"></a>명확 하 게 오버레이

지도에 특정 위치를 식별 하는 마커 달리는 [GroundOverlay](http://developer.android.com/reference/com/google/android/gms/maps/model/GroundOverlay.html) 위치나 영역 지도에 있는 컬렉션을 식별 하는 데 사용 하는 이미지입니다.


##### <a name="adding-a-groundoverlay"></a>GroundOverlay 추가

지도에 명확 하 게 오버레이 추가 매우 비슷합니다는 표식 지도에 추가 합니다. 첫째, 한 [GroundOverlayOptions](http://developer.android.com/reference/com/google/android/gms/maps/model/GroundOverlayOptions.html) 개체가 만들어집니다. 이 개체에 대 한 매개 변수로 전달 되는 `GoogleMap.AddGroundOverlay` 을 반환 하는 메서드는 `GroundOverlay` 개체입니다. 이 코드 조각은 명확 하 게 오버레이 지도에 추가 하는 예입니다.

```csharp
BitmapDescriptor image = BitmapDescriptorFactory.FromResource(Resource.Drawable.polarbear);
GroundOverlayOptions groundOverlayOptions = new GroundOverlayOptions()
    .Position(position, 150, 200)
    .InvokeImage(image);
GroundOverlay myOverlay = _map.AddGroundOverlay(groundOverlayOptions);
```

다음 스크린 샷에서 지도에이 오버레이 보여 줍니다.

[![북극곰 오버레이할 이미지 예제 맵](maps-api-images/image09-sml.png)](maps-api-images/image09.png#lightbox)


#### <a name="lines-circles-and-polygons"></a>선, 원 및 다각형

지도에 추가할 수 있는 기하학적 수치 단순에 세 가지 종류가 있습니다.

-  **폴리라인** -는 일련의 연결 된 선 세그먼트입니다. 지도에서 경로 표시 하거나 필요한 모든 셰이프를 형성 수 있습니다.

-  **다각형** -지도에서 영역의 표시에 대 한 닫힌된 셰이프입니다.

-  **원** -이 지도에 있는 원형 그리기 됩니다.



##### <a name="polylines"></a>폴리라인

A [폴리라인](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Polyline) 연속 목록이 `LatLng` 각 선 세그먼트의 꼭지점을 지정 하는 개체입니다. 먼저 만들어 다중선 만들어집니다는 `PolylineOptions` 요소를 추가 하는 개체입니다. `PolylineOption` 에 다음 전달 된 개체는 `GoogleMap` 호출 하 여 개체는 `AddPolyline` 메서드.

```csharp
PolylineOption rectOptions = new PolylineOption();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
rectOptions.Add(new LatLng(37.35, -122.0)); // close the polyline - this makes a rectangle.
myMap.AddPolyline(rectOptions);
```


##### <a name="polygons"></a>다각형

`Polygon`하지만 s는 매우 유사 `Polyline`s, 열려 있지 않은 종료 되었습니다. `Polygon`s 닫힌된 루프는 하며 해당 내부에 입력 합니다.
`Polygon`s는 정확히 같은 방식으로 생성 됩니다는 `Polyline`를 제외 하 고는 [GoogleMap.AddPolygon](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addPolygon(com.google.android.gms.maps.model.PolygonOptions)) 메서드를 호출 합니다.

와 달리는 `Polyline`, `Polygon` 자체 닫기 합니다. 때 `AddPolygon` 를 호출 하면 메서드를 자동으로 끊고 첫 번째 및 마지막 요소를 연결 하는 선을 그려 다각형 해제 합니다. 다음 코드 조각 이전 코드 조각에서와 동일한 영역을 통해 견고한 사각형 만들어집니다는 `Polyline` 예제입니다.

```csharp
PolygonOptions rectOptions = new PolygonOptions();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
// notice we don't need to close off the polygon
myMap.AddPolygon(rectOptions);
```


##### <a name="circles"></a>원

첫 번째 인스턴스화하여 원 만들어집니다는 [CircleOption](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/CircleOptions) metres의 가운데와 원의 반지름 지정 하는 개체입니다. 호출 하 여 지도에 그릴 원 [GoogleMap.AddCircle](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap#addCircle(com.google.android.gms.maps.model.CircleOptions))합니다.
다음 코드 조각 원을 그리려면 하는 방법을 보여 줍니다.

```csharp
CircleOptions circleOptions = new CircleOptions ();
circleOptions.InvokeCenter (new LatLng(37.4, -122.1));
circleOptions.InvokeRadius (1000);
_map.AddCircle (CircleOptions);
```


## <a name="responding-to-events"></a>이벤트에 응답

세 가지 방법으로 상호 작용 사용자 지도가 있을 수 있습니다.

-  **표식 클릭** -사용자가 마커를 클릭 합니다.

-  **표식 끌어서** -사용자가 장기-를 클릭 한 mparger

-  **정보 창 클릭** -사용자가 정보 창에서를 클릭 합니다.

이러한 각 이벤트는 아래에서 자세히 설명 합니다.


### <a name="marker-click-events"></a>표식 이벤트를 클릭 합니다.

사용자 표식의 클릭할 때는 `MarkerClick` 이벤트 발생 및 `GoogleMap.MarkerClickEventArgs` 전달 합니다. 이 클래스에는 두 개의 속성이 포함 됩니다.

-  `GoogleMap.MarkerClickEventArgs.Handled` &ndash; 이 속성에 설정할 `true` 를 나타내는 이벤트 처리기가 이벤트를 사용 했습니다. 로 설정 되 면 `false` 의 기본 동작의 이벤트 처리기의 사용자 지정 동작 외에도 발생 합니다.

-  `P0` &ndash; 이 잘못 name 매개 변수는 발생 하는 표식에 대 한 참조는 `MarkerClick` 이벤트입니다.


이 코드 조각은의 예가 나와 `MarkerClick` 카메라 위치 지도에 새 위치로 변경 됩니다.

```csharp
private void MapOnMarkerClick(object sender, GoogleMap.MarkerClickEventArgs markerClickEventArgs)
{
    markerClickEventArgs.Handled = true;
    Marker marker = markerClickEventArgs.P0;
    if (marker.Id.Equals(MyMarkerId)) // The ID of a specific marker the user clicked on.
    {
        _map.AnimateCamera(CameraUpdateFactory.NewLatLngZoom(new LatLng(20.72110, -156.44776), 13));
    }
    else
    {
        Toast.MakeText(this, String.Format("You clicked on Marker ID {0}", marker.Id), ToastLength.Short).Show();
    }
}
```


### <a name="marker-drag-events"></a>표식 끌기 이벤트

이 이벤트는 사용자 표식을 끕니다 하고자 할 때 발생 합니다. 기본적으로 표식 draggable 되지 않습니다. 설정 하 여 같이 draggable 표식의 설정할 수 있습니다는 `Marker.Draggable` 속성을 `true` 호출 하 여는 `MarkerOptions.Draggable` 메서드 `true` 매개 변수로 합니다.

표식을 끌어 먼저, 사용자 해야 장기-를 클릭 하 고 지도에 자신의 손가락을 유지. 화면 자신의 손가락을 끌 때 마커 이동 합니다. 가 화면의 리프트 때 마커 위치에 유지 됩니다.

다음 목록에서는 draggable 표식에 대해 발생할 수 있는 다양 한 이벤트를 설명 합니다.

-   `GoogleMap.MarkerDragStart(object sender, GoogleMap.MarkerDragStartEventArgs e)` &ndash; 이 이벤트는 처음으로 가져왔을 표식의 때 발생 합니다.

-   `GoogleMap.MarkerDrag(object sender, GoogleMap.MarkerDragEventArgs e)` &ndash; 이 이벤트는 표식의 끌 때 발생 합니다.

-   `GoogleMap.MarkerDragEnd(object sender, GoogleMap.MarkerDragEndEventArgs e)` &ndash; 이 이벤트는 발생 때 경우에는 사용자가 완료 표식을 끌어 합니다.

각각의 `EventArgs` 라는 단일 속성 포함 `P0` 즉에 대 한 참조는 `Marker` 끌고 개체입니다.


### <a name="info-window-click-events"></a>정보 창 Click 이벤트

한 번에 하나만 정보 창의 표시할 수 있습니다. Map 개체에서 발생 하는 지도에 정보 창에서 사용자가 프로그램 `InfoWindowClick` 이벤트입니다. 다음 코드 조각에는 이벤트에 처리기를 연결 하는 방법을 보여 줍니다.

```csharp
private bool SetupMapIfNeeded()
{
    if (_map == null)
    {
        _map = _mapFragment.Map;
        if (_map != null)
        {
            _map.InfoWindowClick += MapOnInfoWindowClick;
            return true;
        }
        return false;
    }
    return true;
}

private void MapOnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
    Marker myMarker = e.P0;
    // Do something with marker.
}
```

정보 창에는 정적은 `View` 지도에 이미지 형식으로 렌더링 되는 합니다. 단추, 확인란, 또는 정보 창 내에 배치 하는 텍스트 뷰 같은 모든 위젯을 버려지는 되 고 해당 정수 계열 사용자 이벤트에 응답할 수 없습니다.



## <a name="related-links"></a>관련 링크

- [Google Play 서비스](http://developer.android.com/google/play-services/index.html)
- [Google Android API v2 맵](https://developers.google.com/maps/documentation/android/)
- [Google Play 서비스 APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)
- [Google 맵 API 키 가져오기](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [하지 업데이트 AVD 되지만 문제 57880: Google Play 서비스](https://code.google.com/p/android/issues/detail?id=57880)
