---
title: 응용 프로그램에서 Google Maps API 사용
description: Xamarin Android 응용 프로그램에서 Google Maps API v2 기능을 구현 하는 방법입니다.
ms.prod: xamarin
ms.assetid: C0589878-2D04-180E-A5B9-BB41D5AF6E02
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/07/2018
ms.openlocfilehash: adcfb1457742d343f87a602885566107cf327e2d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027147"
---
# <a name="using-the-google-maps-api-in-your-application"></a>응용 프로그램에서 Google Maps API 사용

Maps 응용 프로그램을 사용 하는 것이 좋지만 응용 프로그램에 맵을 직접 포함 하려는 경우가 있습니다. 기본 제공 맵 응용 프로그램 외에도 Google은 [Android 용 네이티브 매핑 API](https://developers.google.com/maps/documentation/android-sdk/intro)를 제공 합니다.
Maps API는 매핑 환경에 대해 더 많은 제어를 유지 하려는 경우에 적합 합니다. Maps API에서 수행할 수 있는 작업은 다음과 같습니다.

- 프로그래밍 방식으로 지도의 관점 변경
- 표식을 추가 하 고 사용자 지정 합니다.
- 오버레이를 사용 하 여 지도에 주석을 추가 합니다.

현재 사용 되지 않는 Google Maps Android API v1과 달리 Google Maps Android API v2는 [Google Play 서비스](https://developers.google.com/android/guides/overview)의 일부입니다.
Google Maps Android API를 사용 하려면 먼저 Xamarin Android 앱이 몇 가지 필수 구성 요소를 충족 해야 합니다.

## <a name="google-maps-api-prerequisites"></a>Google Maps API 필수 조건

Maps API를 사용 하려면 다음을 포함 하 여 몇 가지 단계를 수행 해야 합니다.

- [맵 API 키 가져오기](#obtain-maps-key)
- [Google Play 서비스 SDK 설치](#install-gps-sdk)
- [NuGet에서 Xamarin.googleplayservices.base 패키지를 설치 합니다.](#install-gpsmaps-nuget)
- [필요한 권한 지정](#declare-permissions)
- [필요에 따라 Google Api를 사용 하 여 에뮬레이터를 만듭니다.](#create-emulator-with-google-api)

### <a name="a-nameobtain-maps-key-obtain-a-google-maps-api-key"></a>Google Maps API 키를 <a name="obtain-maps-key" />가져옵니다.

첫 번째 단계는 Google Maps API 키를 가져오는 것입니다 (레거시 Google Maps v1 API에서 API 키를 다시 사용할 수 없음). Xamarin. Android에서 API 키를 가져오고 사용 하는 방법에 대 한 자세한 내용은 [Google MAPS API 키 가져오기](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)를 참조 하세요.

### <a name="a-nameinstall-gps-sdk--install-the-google-play-services-sdk"></a>Google Play 서비스 SDK <a name="install-gps-sdk" /> 설치

Google Play 서비스는 Android 응용 프로그램에서 Google +, 인앱 결제, 지도 등의 다양 한 Google 기능을 활용할 수 있도록 지 원하는 Google의 기술입니다. 이러한 기능은 Android 장치에서 [Google Play 서비스 APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)에 포함 된 백그라운드 서비스로 액세스할 수 있습니다.

Android 응용 프로그램은 Google Play 서비스 클라이언트 라이브러리를 통해 Google Play 서비스와 상호 작용 합니다. 이 라이브러리에는 맵과 같은 개별 서비스에 대 한 인터페이스와 클래스가 포함 되어 있습니다. 다음 다이어그램에서는 Android 응용 프로그램과 Google Play 서비스 간의 관계를 보여 줍니다.

![Google Play 서비스 APK를 업데이트 Google Play 스토어를 보여 주는 다이어그램](maps-api-images/play-services-diagram.png)

Android Maps API는 Google Play 서비스 일부로 제공 됩니다.
Xamarin Android 응용 프로그램에서 Maps API를 사용 하려면 먼저 [Android SDK 관리자](~/android/get-started/installation/android-sdk.md)를 사용 하 여 Google Play 서비스 SDK를 설치 해야 합니다. 다음 스크린샷은 Android SDK Manager에서 Google Play services 클라이언트를 찾을 수 있는 위치를 보여 줍니다.

![Google Play 서비스은 Android SDK 관리자의 추가 기능 아래에 나타납니다.](maps-api-images/image01.png)

> [!NOTE]
> Google Play services APK는 모든 장치에 없을 수 있는 사용이 허가 된 제품입니다. 설치 되지 않은 경우에는 Google Maps가 장치에서 작동 하지 않습니다.

### <a name="a-nameinstall-gpsmaps-nuget--install-the-xamaringoogleplayservicesmaps-package-from-nuget"></a>NuGet에서 Xamarin.googleplayservices.base 패키지를 설치 <a name="install-gpsmaps-nuget" />

[Xamarin.googleplayservices.base 패키지](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Maps) 에는 GOOGLE PLAY 서비스 Maps API에 대 한 xamarin.ios 바인딩이 포함 되어 있습니다.
Google Play 서비스 맵 패키지를 추가 하려면 솔루션 탐색기에서 프로젝트의 **참조** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리 ...** 를 클릭 합니다.

![참조 아래의 NuGet 패키지 관리 상황에 맞는 메뉴 항목을 보여 주는 솔루션 탐색기](maps-api-images/image02.png)

그러면 **NuGet 패키지 관리자**가 열립니다. **찾아보기** 를 클릭 하 고 검색 필드에 **Xamarin Google Play 서비스 지도** 를 입력 합니다. **Xamarin.googleplayservices.base** 를 선택 하 고 **설치**를 클릭 합니다. 이 패키지가 이전에 설치 된 경우 **업데이트**를 클릭 합니다.

[Xamarin.googleplayservices.base 패키지가 선택 된 NuGet 패키지 관리자를![합니다.](maps-api-images/image03-sml.png)](maps-api-images/image03.png#lightbox)

다음 종속성 패키지도 설치 되어 있습니다.

- **Xamarin.GooglePlayServices.Base**
- **Xamarin.GooglePlayServices.Basement**
- **Xamarin.GooglePlayServices.Tasks**

### <a name="a-namedeclare-permissions--specify-the-required-permissions"></a>필요한 사용 권한을 지정 <a name="declare-permissions" />

앱은 Google Maps API를 사용 하기 위해 하드웨어 및 사용 권한 요구 사항을 확인 해야 합니다.  일부 권한은 Google Play 서비스 SDK에서 자동으로 부여 되며 개발자가 명시적으로 **Androidmanfest**에 추가 하는 데는 필요 하지 않습니다.

- Maps API &ndash; **네트워크 상태에 대 한 액세스** 는 맵 타일을 다운로드할 수 있는지 확인할 수 있어야 합니다.

- **인터넷 액세스 &ndash; 지도** 타일을 다운로드 하 고 API 액세스를 위해 Google Play 서버와 통신 해야 합니다.

Google Maps Android API에 대해 **Androidmanifest** 에 다음 사용 권한 및 기능을 지정 해야 합니다.

- 응용 프로그램 &ndash; **OPENGL es V2** 는 opengl es v 2에 대 한 요구 사항을 선언 해야 합니다.

- **Google MAPS Api 키** &ndash; api 키를 사용 하 여 응용 프로그램이 등록 되 고 Google Play 서비스 사용할 수 있는 권한이 있는지 확인 합니다. 이 키에 대 한 자세한 내용은 [Google MAPS API 키 가져오기를](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md) 참조 하세요.

- Android 9.0 (API 수준 28) 이상을 대상으로 하는 앱 &ndash; **레거시 APACHE http 클라이언트를 요청** 하 여 레거시 apache http 클라이언트를 사용할 선택적 라이브러리를 지정 해야 합니다.

- **Google 웹 기반 서비스에 액세스** 하려면 응용 프로그램 &ndash; ANDROID Maps API를 실행 하는 google 웹 서비스에 액세스할 수 있는 권한이 필요 합니다.

- 응용 프로그램 &ndash; **Google Play 서비스 알림에 대 한 권한은** Google Play 서비스에서 원격 알림을 받을 수 있는 권한을 부여 받아야 합니다.

- &ndash; **위치 공급자에 대 한 액세스** 는 선택적 권한입니다.
   이를 통해 `GoogleMap` 클래스에서 지도의 장치 위치를 표시할 수 있습니다.

또한 Android 9는 bootclasspath에서 Apache HTTP 클라이언트 라이브러리를 제거 했으므로 API 28 이상을 대상으로 하는 응용 프로그램에서는 사용할 수 없습니다. API 28 이상을 대상으로 하는 응용 프로그램에서 Apache HTTP 클라이언트를 계속 사용 하려면 다음 줄을 **Androidmanifest .xml** 파일의 `application` 노드에 추가 해야 합니다.

```xml
<application ...>
   ...
   <uses-library android:name="org.apache.http.legacy" android:required="false" />    
</application>
```

> [!NOTE]
> Google Play SDK의 이전 버전에서는 `WRITE_EXTERNAL_STORAGE` 권한을 요청 하는 앱이 필요 했습니다. Google Play 서비스에 대 한 최근 Xamarin 바인딩에는이 요구 사항이 더 이상 필요 하지 않습니다.

다음 코드 조각은 **Androidmanifest**에 추가 해야 하는 설정의 예입니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionName="4.5" package="com.xamarin.docs.android.mapsandlocationdemo2" android:versionCode="6">
    <uses-sdk android:minSdkVersion="23" android:targetSdkVersion="28" />

    <!-- Google Maps for Android v2 requires OpenGL ES v2 -->
    <uses-feature android:glEsVersion="0x00020000" android:required="true" />

    <!-- Necessary for apps that target Android 9.0 or higher -->
    <uses-library android:name="org.apache.http.legacy" android:required="false" />

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
        <!-- Necessary for apps that target Android 9.0 or higher -->
        <uses-library android:name="org.apache.http.legacy" android:required="false" />
    </application>
</manifest>
```

권한 **Androidmanifest**을 요청 하는 것 외에도 앱은 `ACCESS_COARSE_LOCATION` 및 `ACCESS_FINE_LOCATION` 사용 권한에 대 한 런타임 권한 검사를 수행 해야 합니다. 런타임 권한 검사를 수행 하는 방법에 대 한 자세한 내용은 [Xamarin Android 사용 권한](~/android/app-fundamentals/permissions.md) 가이드를 참조 하세요.

### <a name="a-namecreate-emulator-with-google-api-create-an-emulator-with-google-apis"></a>Google Api를 사용 하 여 에뮬레이터 <a name="create-emulator-with-google-api" />만들기

Google Play services를 사용 하는 물리적 Android 장치가 설치 되지 않은 경우 개발용 에뮬레이터 이미지를 만들 수 있습니다. 자세한 내용은 [Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)를 참조 하세요.

## <a name="the-googlemap-class"></a>GoogleMap 클래스

필수 구성 요소가 충족 되 면 응용 프로그램 개발을 시작 하 고 Android Maps API를 사용할 차례입니다. [GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap) 클래스는 Xamarin android 응용 프로그램이 Android 용 Google Maps를 표시 하 고 상호 작용 하는 데 사용 하는 기본 API입니다. 이 클래스에는 다음과 같은 책임이 있습니다.

- Google 웹 서비스를 사용 하 여 응용 프로그램에 권한을 부여 하기 위해 Google Play 서비스와 상호 작용 합니다.

- 지도 타일을 다운로드 하 고, 캐시 하 고, 표시 합니다.

- 사용자에 게 이동 및 확대와 같은 UI 컨트롤을 표시 합니다.

- 지도에서 표식 및 기하학적 모양을 그립니다.

`GoogleMap`은 다음 두 가지 방법 중 하나로 활동에 추가 됩니다.

- **Mapfragment** - [mapfragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment) 는 `GoogleMap` 개체에 대 한 호스트 역할을 하는 특수 한 조각입니다. `MapFragment`에는 Android API 레벨 12 이상이 필요 합니다.
   이전 버전의 Android에서는 [Supportmapfragment](https://developers.google.com/android/reference/com/google/android/gms/maps/SupportMapFragment)를 사용할 수 있습니다.  이 가이드에서는 `MapFragment` 클래스를 사용 하는 방법을 집중적으로 설명 합니다.

- **Mapview** - [mapview](https://developers.google.com/android/reference/com/google/android/gms/maps/MapView) 는 `GoogleMap` 개체에 대 한 호스트 역할을 할 수 있는 특수 뷰 하위 클래스입니다. 이 클래스의 사용자는 모든 작업 수명 주기 메서드를 `MapView` 클래스로 전달 해야 합니다.

이러한 각 컨테이너는 `GoogleMap`의 인스턴스를 반환 하는 `Map` 속성을 노출 합니다. 개발자가 수동으로 구현 해야 하는 상용구 코드의 크기를 줄이는 단순한 API 이므로 기본 설정은 [Mapfragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment) 클래스에 부여 해야 합니다.

### <a name="adding-a-mapfragment-to-an-activity"></a>작업에 MapFragment 추가

다음 스크린샷은 간단한 `MapFragment`의 예입니다.

[Google 지도 조각을 표시 하는 장치의![스크린샷](maps-api-images/image05-sml.png)](maps-api-images/image05.png#lightbox)

다른 조각 클래스와 마찬가지로 활동에 `MapFragment`를 추가 하는 방법에는 두 가지가 있습니다.

- **선언적** -작업에 대 한 XML 레이아웃 파일을 통해 `MapFragment`를 추가할 수 있습니다. 다음 XML 코드 조각에서는 `fragment` 요소를 사용 하는 방법의 예를 보여 줍니다.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <fragment xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/map"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              class="com.google.android.gms.maps.MapFragment" />
    ```

- **프로그래밍 방식** -`MapFragment` [`MapFragment.NewInstance`](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment.html#newInstance()) 메서드를 사용 하 여 프로그래밍 방식으로 인스턴스화한 다음 활동에 추가할 수 있습니다. 이 코드 조각은 `MapFragment` 개체를 인스턴스화하고 활동에 추가 하는 가장 간단한 방법을 보여 줍니다.

    ```csharp
        var mapFrag = MapFragment.NewInstance();
        activity.FragmentManager.BeginTransaction()
                                .Add(Resource.Id.map_container, mapFrag, "map_fragment")
                                .Commit();

    ```

    `NewInstance`에 [`GoogleMapOptions`](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) 개체를 전달 하 여 `MapFragment` 개체를 구성할 수 있습니다. 이에 대해서는이 가이드의 뒷부분에 나오는 [GoogleMap 속성](#googlemap_object) 섹션에 설명 되어 있습니다.

`MapFragment.GetMapAsync` 메서드는 조각에서 호스트 되는 [`GoogleMap`](#googlemap_object) 를 초기화 하 고 `MapFragment`에서 호스팅하는 map 개체에 대 한 참조를 가져오는 데 사용 됩니다. 이 메서드는 `IOnMapReadyCallback` 인터페이스를 구현 하는 개체를 사용 합니다.

이 인터페이스에는 응용 프로그램이 `GoogleMap` 개체와 상호 작용할 수 있을 때 호출 되는 단일 메서드인 `IMapReadyCallback.OnMapReady(MapFragment map)` 있습니다. 다음 코드 조각에서는 Android 작업에서 `MapFragment`를 초기화 하 고 `IOnMapReadyCallback` 인터페이스를 구현 하는 방법을 보여 줍니다.

```csharp
public class MapWithMarkersActivity : AppCompatActivity, IOnMapReadyCallback
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.MapLayout);

        var mapFragment = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.map);
        mapFragment.GetMapAsync(this);

        // remainder of code omitted
    }

    public void OnMapReady(GoogleMap map)
    {
        // Do something with the map, i.e. add markers, move to a specific location, etc.
    }
}
```

### <a name="map-types"></a>지도 유형

Google Maps API에서 사용할 수 있는 지도에는 5 가지 유형이 있습니다.

- **Normal** -기본 지도 유형입니다. 이 도구는 빌딩 및 브리지와 같은 몇 가지 인공 점과 함께도로와 중요 한 자연 기능을 보여 줍니다.

- **위성** -이 지도는 위성 사진을 보여 줍니다.

- **하이브리드** -이 지도는 위성 사진 및도로 지도를 표시 합니다.

- **지형** -주로 일부도로 topographical 기능을 보여 줍니다.

- **없음** -이 맵은 타일을 로드 하지 않으며 빈 그리드로 렌더링 됩니다.

아래 이미지에는 왼쪽에서 오른쪽 (보통, 하이브리드, 지형)의 다양 한 지도 유형이 나와 있습니다.

[![3 개의 지도 예제 스크린샷 (Normal, 하이브리드 및 지형)](maps-api-images/map-types-sml.png)](maps-api-images/map-types.png#lightbox)

`GoogleMap.MapType` 속성은 표시 되는 맵의 유형을 설정 하거나 변경 하는 데 사용 됩니다. 다음 코드 조각에서는 위성 지도를 표시 하는 방법을 보여 줍니다.

```csharp
public void OnMapReady(GoogleMap map)
{
    map.MapType = GoogleMap.MapTypeHybrid;
}
```

### <a name="a-namegooglemap_object-googlemap-properties"></a><a name="googlemap_object" />GoogleMap 속성

`GoogleMap`는 맵의 기능과 기능을 제어할 수 있는 여러 속성을 정의 합니다. `GoogleMap`의 초기 상태를 구성 하는 한 가지 방법은 `MapFragment`를 만들 때 [GoogleMapOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) 개체를 전달 하는 것입니다. 다음 코드 조각은 `MapFragment`를 만들 때 `GoogleMapOptions` 개체를 사용 하는 한 가지 예입니다.

```csharp
GoogleMapOptions mapOptions = new GoogleMapOptions()
    .InvokeMapType(GoogleMap.MapTypeSatellite)
    .InvokeZoomControlsEnabled(false)
    .InvokeCompassEnabled(true);

FragmentTransaction fragTx = FragmentManager.BeginTransaction();
mapFragment = MapFragment.NewInstance(mapOptions);
fragTx.Add(Resource.Id.map, mapFragment, "map");
fragTx.Commit();
```

`GoogleMap`를 구성 하는 다른 방법은 map 개체의 [Uisettings](https://developers.google.com/android/reference/com/google/android/gms/maps/UiSettings) 에서 속성을 조작 하는 것입니다. 다음 코드 샘플은 확대/축소 컨트롤 및 나침반을 표시 하도록 `GoogleMap`를 구성 하는 방법을 보여 줍니다.

```csharp
public void OnMapReady(GoogleMap map)
{
    map.UiSettings.ZoomControlsEnabled = true;
    map.UiSettings.CompassEnabled = true;
}
```

## <a name="interacting-with-the-googlemap"></a>GoogleMap와 상호 작용

Android Maps API는 활동에서 시점을 변경 하거나, 표식을 추가 하거나, 사용자 지정 오버레이를 추가 하거나, 도형을 그릴 수 있는 Api를 제공 합니다. 이 섹션에서는 Xamarin Android에서 이러한 작업 중 일부를 수행 하는 방법을 설명 합니다.

### <a name="changing-the-viewpoint"></a>관점 변경

지도는 Mercator 프로젝션을 기반으로 화면에서 평면 평면으로 모델링할 됩니다. 지도 보기는이 평면에서 바로 작동 하는 *카메라* 의 뷰입니다. 위치, 확대/축소, 기울기 및 베어링을 변경 하 여 카메라의 위치를 제어할 수 있습니다. [CameraUpdate](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdate) 클래스는 카메라 위치를 이동 하는 데 사용 됩니다. `CameraUpdate` 개체가 직접 인스턴스화되지 않고 맵 API가 [CameraUpdateFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdateFactory) 클래스를 제공 합니다.

`CameraUpdate` 개체를 만든 후에는 [MoveCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#moveCamera(com.google.android.gms.maps.CameraUpdate)) 또는 [GoogleMap AnimateCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#animateCamera(com.google.android.gms.maps.CameraUpdate)) 메서드에 매개 변수로 전달 됩니다. `MoveCamera` 메서드는 `AnimateCamera` 메서드가 부드러운 애니메이션 전환을 제공 하는 동안 즉시 맵을 업데이트 합니다.

이 코드 조각은 `CameraUpdateFactory`를 사용 하 여 지도의 확대/축소 수준을 1 확대/축소 수준으로 증가 시키는 `CameraUpdate`를 만드는 방법에 대 한 간단한 예입니다.

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...

public void OnMapReady(GoogleMap map)
{   
    map.MoveCamera(CameraUpdateFactory.ZoomIn());
}
```

Maps API는 카메라 위치에 대해 가능한 모든 값을 집계 하는 [CameraPosition](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.html) 을 제공 합니다. `CameraUpdate` 개체를 반환 하는 [NewCameraPosition](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdateFactory#newCameraPosition%28com.google.android.gms.maps.model.CameraPosition%29) 메서드에이 클래스의 인스턴스를 제공할 수 있습니다. 지도 api에는 `CameraPosition`개체를 만들기 위한 흐름 api를 제공하는 [CameraPosition.Builder](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.Builder.html) 클래스도 포함되어 있습니다.
다음 코드 조각에서는 `CameraPosition`에서 `CameraUpdate`를 만들고이를 사용 하 여 `GoogleMap`에서 카메라 위치를 변경 하는 예를 보여 줍니다.

```csharp
public void OnMapReady(GoogleMap map)
{
    LatLng location = new LatLng(50.897778, 3.013333);

    CameraPosition.Builder builder = CameraPosition.InvokeBuilder();
    builder.Target(location);
    builder.Zoom(18);
    builder.Bearing(155);
    builder.Tilt(65);

    CameraPosition cameraPosition = builder.Build();

    CameraUpdate cameraUpdate = CameraUpdateFactory.NewCameraPosition(cameraPosition);

    map.MoveCamera(cameraUpdate);
}
```

위의 코드 조각에서 map의 특정 위치는 [LatLng](https://developers.google.com/android/reference/com/google/android/gms/maps/model/LatLng) 클래스로 나타냅니다. 확대/축소 수준은 Google Maps에서 사용 하는 확대/축소의 임의 측정 인 18로 설정 됩니다. 베어링은 북쪽에서 시계 방향으로 나침반입니다. 기울기 속성은 보기 각도를 제어 하 고 세로에서 25도의 각도를 지정 합니다. 다음 스크린샷에서는 위의 코드를 실행 한 후 `GoogleMap`를 보여 줍니다.

[기울어진 보기 각도를 사용 하 여 지정 된 위치를 보여 주는 예제 Google 지도![예제](maps-api-images/image06-sml.png)](maps-api-images/image06.png#lightbox)

### <a name="drawing-on-the-map"></a>지도에 그리기

Android Maps API는 맵에 다음 항목을 그리기 위한 API를 제공 합니다.

- **표식** -지도에서 단일 위치를 식별 하는 데 사용 되는 특수 아이콘입니다.

- **오버레이** -맵의 위치나 영역 컬렉션을 식별 하는 데 사용할 수 있는 이미지입니다.

- **선, 다각형 및 원** -활동에서 지도에 셰이프를 추가할 수 있도록 하는 api입니다.

#### <a name="markers"></a>Markers

Maps API는 맵의 단일 위치에 대 한 모든 데이터를 캡슐화 하는 [표식](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) 클래스를 제공 합니다. 기본적으로 표식 클래스는 Google Maps에서 제공 하는 표준 아이콘을 사용 합니다. 표식의 모양을 사용자 지정 하 고 사용자 클릭에 응답할 수 있습니다.

##### <a name="adding-a-marker"></a>표식 추가

지도에 표식을 추가 하려면 새 [Markeroptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions) 개체를 만든 다음 `GoogleMap` 인스턴스에서 [addmarker](https://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addMarker%28com.google.android.gms.maps.model.MarkerOptions%29) 메서드를 호출 해야 합니다. 이 메서드는 [표식](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) 개체를 반환 합니다.

```csharp
public void OnMapReady(GoogleMap map)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");

    map.AddMarker(markerOpt1);
}
```

마커 제목은 사용자가 표식을 누를 때 *정보 창* 에 표시 됩니다. 다음 스크린샷은이 표식을 보여 줍니다.

[![예: 마커 및 Vimy 볼록에 대 한 정보 창을 포함 하는 Google Map](maps-api-images/image07-sml.png)](maps-api-images/image07.png#lightbox)

##### <a name="customizing-a-marker"></a>표식 사용자 지정

지도에 표식을 추가할 때 `MarkerOptions.InvokeIcon` 메서드를 호출 하 여 마커에 사용 되는 아이콘을 사용자 지정할 수 있습니다.
이 메서드는 아이콘을 렌더링 하는 데 필요한 데이터를 포함 하는 [BitmapDescriptor](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptor) 개체를 사용 합니다. [BitmapDescriptorFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptorFactory) 클래스는 `BitmapDescriptor`만들기를 간소화 하는 도우미 메서드를 제공 합니다. 다음 목록에서는 이러한 메서드 중 일부를 소개 합니다.

- 기본 Google 지도 표식을 사용 하 &ndash; `DefaultMarker(float colour)` 색을 변경 합니다.

- `FromAsset(string assetName)` 자산 폴더에서 지정 된 파일의 사용자 지정 아이콘을 사용 &ndash; 합니다.

- 지정 된 비트맵을 아이콘으로 사용 &ndash; `FromBitmap(Bitmap image)` 합니다.

- 지정 된 경로에 있는 파일에서 사용자 지정 아이콘을 &ndash; `FromFile(string fileName)` 만듭니다.

- 지정 된 리소스에서 사용자 지정 아이콘을 &ndash; `FromResource(int resourceId)` 만듭니다.

다음 코드 조각에서는 사이안 coloured default 표식을 만드는 예를 보여 줍니다.

```csharp
public void OnMapReady(GoogleMap map)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");

    var bmDescriptor = BitmapDescriptorFactory.DefaultMarker (BitmapDescriptorFactory.HueCyan);
    markerOpt1.InvokeIcon(bmDescriptor);

    map.AddMarker(markerOpt1);
}
```

#### <a name="info-windows"></a>정보 창

*정보 창은* 특정 마커를 누를 때 사용자에 게 정보를 표시 하는 특수 창입니다. 기본적으로 정보 창에 표식 제목의 내용이 표시 됩니다. 제목이 할당 되지 않은 경우에는 정보 창이 표시 되지 않습니다. 한 번에 하나의 정보 창만 표시할 수 있습니다.

[GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.InfoWindowAdapter) 을 구현 하 여 정보 창을 사용자 지정할 수 있습니다. 이 인터페이스에는 다음과 같은 두 가지 중요 한 메서드가 있습니다.

- `public View GetInfoWindow(Marker marker)` &ndash;이 메서드는 마커의 사용자 지정 정보 창을 가져오기 위해 호출 됩니다. `null` 반환 하는 경우 기본 창 렌더링이 사용 됩니다. 이 메서드가 뷰를 반환 하면 해당 뷰가 정보 창 프레임 내에 배치 됩니다.

- `public View GetInfoContents(Marker marker)` &ndash;이 메서드는 GetInfoWindow에서 `null`를 반환 하는 경우에만 호출 됩니다. 정보 창 내용의 기본 렌더링이 사용 되는 경우이 메서드는 `null` 값을 반환할 수 있습니다. 그렇지 않으면이 메서드는 정보 창의 내용이 포함 된 뷰를 반환 해야 합니다.

정보 창은 라이브 보기가 아닙니다. 대신 Android에서 뷰를 정적 비트맵으로 변환 하 고 이미지에 표시 합니다. 즉, 정보 창이 터치 이벤트 나 제스처에 응답할 수 없고 자동으로 업데이트 되지 않습니다. 정보 창을 업데이트 하려면 [GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker.html#showInfoWindow()) 메서드를 호출 해야 합니다.

다음 이미지는 일부 사용자 지정 된 정보 창의 몇 가지 예를 보여 줍니다. 왼쪽 이미지에는 사용자 지정 된 내용이 있고 오른쪽 이미지에는 모퉁이가 둥근 모퉁이가 지정 된 창 및 내용이 있습니다.

![아이콘 및 채우기를 포함 하는 멜버른의 예제 표식 창입니다. 오른쪽 창에 모퉁이가 둥근 경우](maps-api-images/marker-infowindows.png)

#### <a name="groundoverlays"></a>GroundOverlays

지도에서 특정 위치를 식별 하는 표식과 달리 [GroundOverlay](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlay) 은 위치의 컬렉션 또는 지도의 영역을 식별 하는 데 사용 되는 이미지입니다.

##### <a name="adding-a-groundoverlay"></a>GroundOverlay 추가

지도에 접지 오버레이를 추가 하는 것은 지도에 표식을 추가 하는 것과 비슷합니다. 먼저 [GroundOverlayOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlayOptions) 개체가 생성 됩니다. 그런 다음이 개체는 `GroundOverlay` 개체를 반환 하는 [`GoogleMap.AddGroundOverlay`](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addGroundOverlay(com.google.android.gms.maps.model.GroundOverlayOptions)) 메서드에 매개 변수로 전달 됩니다. 이 코드 조각은 지도에 접지 오버레이를 추가 하는 예제입니다.

```csharp
BitmapDescriptor image = BitmapDescriptorFactory.FromResource(Resource.Drawable.polarbear);
GroundOverlayOptions groundOverlayOptions = new GroundOverlayOptions()
    .Position(position, 150, 200)
    .InvokeImage(image);
GroundOverlay myOverlay = googleMap.AddGroundOverlay(groundOverlayOptions);
```

다음 스크린샷에는 맵에이 오버레이가 표시 됩니다.

[오버레이할 이미지를 포함 하는![예제 맵](maps-api-images/image09-sml.png)](maps-api-images/image09.png#lightbox)

#### <a name="lines-circles-and-polygons"></a>선, 원 및 다각형

지도에 추가할 수 있는 세 가지 간단한 형식의 기하학적 수치가 있습니다.

- **다중선** -일련의 연결 된 선 세그먼트입니다. 지도에서 경로를 표시 하거나 기 하 도형을 만들 수 있습니다.

- **원** -지도에 원을 그립니다.

- **Polygon** -지도에 영역을 표시 하기 위한 닫힌 셰이프입니다.

##### <a name="polylines"></a>폴리라인의 선을

[다중선](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Polyline) 은 각 선 세그먼트의 꼭 짓 점을 지정 하는 연속 `LatLng` 개체의 목록입니다. 먼저 `PolylineOptions` 개체를 만들고이 개체에 요소를 추가 하 여 다중선을 만듭니다. 그런 다음 `PolylineOption` 개체는 `AddPolyline` 메서드를 호출 하 여 `GoogleMap` 개체에 전달 됩니다.

```csharp
PolylineOption rectOptions = new PolylineOption();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
rectOptions.Add(new LatLng(37.35, -122.0)); // close the polyline - this makes a rectangle.

googleMap.AddPolyline(rectOptions);
```

##### <a name="circles"></a>원이

원은 metres에서 원의 중심과 반지름을 지정 하는 [CircleOption](https://developers.google.com/android/reference/com/google/android/gms/maps/model/CircleOptions) 개체를 먼저 인스턴스화하여 생성 됩니다. [GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addCircle(com.google.android.gms.maps.model.CircleOptions))를 호출 하 여 지도에 원이 그려집니다.
다음 코드 조각에서는 원을 그리는 방법을 보여 줍니다.

```csharp
CircleOptions circleOptions = new CircleOptions ();
circleOptions.InvokeCenter (new LatLng(37.4, -122.1));
circleOptions.InvokeRadius (1000);

googleMap.AddCircle (circleOptions);
```

##### <a name="polygons"></a>다각형

`Polygon`은 `Polyline`s와 유사 하지만 열려 있지 않습니다. `Polygon`s는 닫힌 루프 이며 내부를 채웁니다.
`Polygon`는 호출 된 [GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addPolygon(com.google.android.gms.maps.model.PolygonOptions)) 메서드를 제외 하 고 `Polyline`와 정확히 동일한 방식으로 만들어집니다.

`Polyline`와 달리 `Polygon`은 자체적으로 닫힙니다. 첫 번째 및 마지막 요소를 연결 하는 선을 그려 `AddPolygon` 메서드에서 다각형을 닫습니다. 다음 코드 조각에서는 `Polyline` 예제에서 이전 코드 조각과 동일한 영역에 대 한 solid 사각형을 만듭니다.

```csharp
PolygonOptions rectOptions = new PolygonOptions();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
// notice we don't need to close off the polygon

googleMap.AddPolygon(rectOptions);
```

## <a name="responding-to-user-events"></a>사용자 이벤트에 응답

사용자에 게는 다음과 같은 세 가지 유형의 상호 작용이 있습니다.

- **표식 클릭** -사용자가 표식을 클릭 합니다.

- **표식 드래그** -사용자가 mparger를 길게 클릭 했습니다.

- **정보 창 클릭** -사용자가 정보 창을 클릭 했습니다.

이러한 각 이벤트는 아래에서 자세히 설명 합니다.

### <a name="marker-click-events"></a>표식 클릭 이벤트

사용자가 표식을 누르면 `MarkerClicked` 이벤트가 발생 합니다. 이 이벤트는 `GoogleMap.MarkerClickEventArgs` 개체를 매개 변수로 받아들입니다. 이 클래스에는 두 가지 속성이 있습니다.

- &ndash; `GoogleMap.MarkerClickEventArgs.Handled` 이벤트 처리기가 이벤트를 사용 했음을 나타내려면이 속성을 `true`으로 설정 해야 합니다. 이를 `false`로 설정 하면 이벤트 처리기의 사용자 지정 동작 외에도 기본 동작이 수행 됩니다.

- `Marker` &ndash;이 속성은 `MarkerClick` 이벤트를 발생 시킨 표식에 대 한 참조입니다.

이 코드 조각은 지도의 새 위치로 카메라 위치를 변경 하는 `MarkerClick`의 예를 보여 줍니다.

```csharp
void MapOnMarkerClick(object sender, GoogleMap.MarkerClickEventArgs markerClickEventArgs)
{
    markerClickEventArgs.Handled = true;

    var marker = markerClickEventArgs.Marker;
    if (marker.Id.Equals(gotMauiMarkerId))
    {
        LatLng InMaui = new LatLng(20.72110, -156.44776);

        // Move the camera to look at Maui.
        PositionPolarBearGroundOverlay(InMaui);
        googleMap.AnimateCamera(CameraUpdateFactory.NewLatLngZoom(InMaui, 13));
        gotMauiMarkerId = null;
        polarBearMarker.Remove();
        polarBearMarker = null;
    }
    else
    {
        Toast.MakeText(this, $"You clicked on Marker ID {marker.Id}", ToastLength.Short).Show();
    }
}
```

### <a name="marker-drag-events"></a>표식 끌기 이벤트

이 이벤트는 사용자가 표식을 끌 때 발생 합니다. 기본적으로 표식을 끌 수 없습니다. `Marker.Draggable` 속성을 `true`로 설정 하거나 `true`를 매개 변수로 사용 하 여 `MarkerOptions.Draggable` 메서드를 호출 하 여 마커를 끌 수 있도록 설정할 수 있습니다.

표식을 끌어 오면 사용자는 먼저 마커를 길게 클릭 해야 합니다. 그런 다음 해당 손가락을 지도에 그대로 두어야 합니다. 화면 위에서 사용자의 손가락을 끌면 마커가 이동 합니다. 사용자의 손가락이 화면에서 리프트 될 때 마커는 그대로 유지 됩니다.

다음 목록에서는 끌기 가능한 마커에 대해 발생 하는 다양 한 이벤트에 대해 설명 합니다.

- `GoogleMap.MarkerDragStart(object sender, GoogleMap.MarkerDragStartEventArgs e)` &ndash;이 이벤트는 사용자가 표식을 처음 끌 때 발생 합니다.

- `GoogleMap.MarkerDrag(object sender, GoogleMap.MarkerDragEventArgs e)` &ndash; 표식을 끌 때 발생 합니다.

- `GoogleMap.MarkerDragEnd(object sender, GoogleMap.MarkerDragEndEventArgs e)` &ndash;이 이벤트는 사용자가 표식 끌기를 완료 했을 때 발생 합니다.

각 `EventArgs`는 끌고 있는 `Marker` 개체에 대 한 참조 인 `P0` 라는 단일 속성을 포함 합니다.

### <a name="info-window-click-events"></a>정보 창 클릭 이벤트

한 번에 하나의 정보 창만 표시할 수 있습니다. 사용자가 지도의 정보 창을 클릭 하면 map 개체가 `InfoWindowClick` 이벤트를 발생 시킵니다. 다음 코드 조각에서는 이벤트에 처리기를 연결 하는 방법을 보여 줍니다.

```csharp
public void OnMapReady(GoogleMap map)
{
    map.InfoWindowClick += MapOnInfoWindowClick;
}

private void MapOnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
    Marker myMarker = e.Marker;
    // Do something with marker.
}
```

정보 창은 지도의 이미지로 렌더링 되는 정적 `View`입니다. 정보 창 내에 배치 되는 단추, 확인란 또는 텍스트 뷰와 같은 모든 위젯을 버려지는 단순한 하 고 해당 정수 사용자 이벤트에 응답할 수 없습니다.

## <a name="related-links"></a>관련 링크

- [SimpleMapDemo](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3/SimpleMapDemo)
- [Google Play 서비스](https://developers.google.com/android/guides/overview)
- [Google Maps Android API v2](https://developers.google.com/maps/documentation/android-sdk/intro)
- [Google Play 서비스 APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)
- [Google Maps API 키 가져오기](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [uses-library](https://developer.android.com/guide/topics/manifest/uses-library-element)
- [uses-feature](https://developer.android.com/guide/topics/manifest/uses-feature-element)
