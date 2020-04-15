---
title: 애플리케이션에서 Google Maps API 사용
description: Xamarin.Android 애플리케이션에서 Google Maps API v2 기능을 구현하는 방법입니다.
ms.prod: xamarin
ms.assetid: C0589878-2D04-180E-A5B9-BB41D5AF6E02
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/07/2018
ms.openlocfilehash: adcfb1457742d343f87a602885566107cf327e2d
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027147"
---
# <a name="using-the-google-maps-api-in-your-application"></a>애플리케이션에서 Google Maps API 사용

Maps 애플리케이션을 사용하는 것은 좋지만, 때로는 내 애플리케이션에 맵을 직접 포함하고 싶은 경우가 있습니다. Google은 기본 제공 맵 애플리케이션 외에도 [Android용 네이티브 매핑 API](https://developers.google.com/maps/documentation/android-sdk/intro)를 제공합니다.
Maps API는 매핑 환경을 보다 강력하게 제어하려는 경우에 적합합니다. Maps API로 가능한 기능은 다음과 같습니다.

- 프로그래밍 방식으로 맵의 시점을 변경합니다.
- 표식을 추가하고 사용자 지정합니다.
- 오버레이로 맵에 주석을 추가합니다.

이제는 더 이상 사용되지 않는 Google Maps Android API v1과 달리, Google Maps Android API v2는 [Google Play 서비스](https://developers.google.com/android/guides/overview)의 일부입니다.
Google Maps Android API를 사용하려면 그 전에 Xamarin.Android 앱이 몇 가지 필수 조건을 충족해야 합니다.

## <a name="google-maps-api-prerequisites"></a>Google Maps API 필수 조건

Maps API를 사용하려면 다음을 비롯한 몇 가지 단계를 수행해야 합니다.

- [Maps API 키 가져오기](#obtain-maps-key)
- [Google Play 서비스 SDK 설치](#install-gps-sdk)
- [NuGet에서 Xamarin.GooglePlayServices.Maps 패키지 설치](#install-gpsmaps-nuget)
- [필수 권한 지정](#declare-permissions)
- [필요한 경우, Google API를 사용하여 에뮬레이터 만들기](#create-emulator-with-google-api)

### <a name="obtain-a-google-maps-api-key"></a><a name="obtain-maps-key" />Google Maps API 키 가져오기

첫 번째 단계는 Google Maps API 키를 얻는 것입니다. (기존 Google Maps v1 API의 API 키를 재사용할 수는 없습니다.) Xamarin.Android를 사용하여 API 키를 얻고 사용하는 방법에 대한 자세한 내용은 [Google Maps API 키 가져오기](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)를 참조하세요.

### <a name="install-the-google-play-services-sdk"></a><a name="install-gps-sdk" /> Google Play 서비스 SDK 설치

Google Play 서비스는 Android 애플리케이션에서 Google+, 인앱 결제 및 Maps와 같은 다양한 Google 기능을 활용할 수 있도록 하는 Google의 기술입니다. 이러한 기능은 Android 디바이스에서 [Google Play 서비스 APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)에 포함된 백그라운드 서비스로 액세스할 수 있습니다.

Android 애플리케이션은 Google Play 서비스 클라이언트 라이브러리를 통해 Google Play 서비스와 상호 작용합니다. 이 라이브러리에는 Maps와 같은 개별 서비스에 대한 인터페이스와 클래스가 포함되어 있습니다. 다음 다이어그램은 Android 애플리케이션과 Google Play 서비스 간의 관계를 보여줍니다.

![Google Play 서비스 APK를 업데이트하는 Google Play 스토어를 보여주는 다이어그램](maps-api-images/play-services-diagram.png)

Android Maps API는 Google Play 서비스의 일부로 제공됩니다.
Xamarin.Android 애플리케이션에서 Maps API를 사용하려면 [Android SDK 관리자](~/android/get-started/installation/android-sdk.md)를 사용하여 Google Play 서비스 SDK를 설치해야 합니다. 다음 스크린샷은 Android SDK 관리자에서 Google Play 서비스 클라이언트를 찾을 수 있는 위치를 보여줍니다.

![Google Play 서비스는 Android SDK 관리자의 Extras 아래에 나타납니다.](maps-api-images/image01.png)

> [!NOTE]
> Google Play 서비스 APK는 라이선스 제품이기 때문에 일부 디바이스에는 없을 수도 있습니다. 설치되어 있지 않은 디바이스에서는 Google Maps가 작동하지 않습니다.

### <a name="install-the-xamaringoogleplayservicesmaps-package-from-nuget"></a><a name="install-gpsmaps-nuget" /> NuGet에서 Xamarin.GooglePlayServices.Maps 패키지 설치

[Xamarin.GooglePlayServices.Maps 패키지](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Maps)에는 Google Play 서비스 Maps API에 대한 Xamarin.Android 바인딩이 포함되어 있습니다.
Google Play 서비스 맵 패키지를 추가하려면 솔루션 탐색기에서 프로젝트의 **참조** 폴더를 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리...** 를 클릭합니다.

![참조 아래에 NuGet 패키지 관리라는 상황에 맞는 메뉴 항목이 표시된 솔루션 탐색기](maps-api-images/image02.png)

그러면 **NuGet 패키지 관리자**가 열립니다. **찾아보기**를 클릭하고 검색 필드에 **Xamarin Google Play 서비스 Maps**를 입력합니다. **Xamarin.GooglePlayServices.Maps**를 선택하고 **설치**를 클릭합니다. (이 패키지가 이전에 설치된 경우 **업데이트**를 클릭합니다.):

[![Xamarin.GooglePlayServices.Maps 패키지가 선택된 NuGet 패키지 관리자](maps-api-images/image03-sml.png)](maps-api-images/image03.png#lightbox)

다음과 같은 종속성 패키지도 설치됩니다.

- **Xamarin.GooglePlayServices.Base**
- **Xamarin.GooglePlayServices.Basement**
- **Xamarin.GooglePlayServices.Tasks**

### <a name="specify-the-required-permissions"></a><a name="declare-permissions" /> 필수 권한 지정

Google Maps API를 사용하려면 앱이 하드웨어 및 권한 요구 사항을 식별해야 합니다.  일부 권한은 Google Play 서비스 SDK에서 자동으로 부여되며 개발자가 **AndroidManfest.XML**에 명시적으로 추가할 필요가 없습니다.

- **네트워크 상태에 대한 액세스** &ndash; Maps API는 맵 타일을 다운로드할 수 있는지 확인할 수 있어야 합니다.

- **인터넷 액세스** &ndash; API 액세스를 위해 맵 타일을 다운로드하고 Google Play 서버와 통신하려면 인터넷 액세스가 필요합니다.

Google Maps Android API의 **AndroidManifest.XML**에 다음 권한 및 기능을 지정해야 합니다.

- **OpenGL ES v2** &ndash; 애플리케이션이 OpenGL ES v2에 대한 요구 사항을 선언해야 합니다.

- **Google Maps API 키** &ndash; API 키는 애플리케이션이 Google Play 서비스를 사용하도록 등록되고 권한이 부여되었는지 확인하는 데 사용됩니다. 이 키에 대한 자세한 내용은 [Google Maps API 키 가져오기](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)를 참조하세요.

- **레거시 Apache HTTP 클라이언트 요청** &ndash; Android 9.0(API 레벨 28) 이상을 대상으로 하는 앱의 경우 사용할 선택적 라이브러리가 레거시 Apache HTTP 클라이언트라고 지정해야 합니다.

- **Google 웹 기반 서비스에 대한 액세스** &ndash; Android Maps API를 지원하는 Google의 웹 서비스에 액세스할 수 있는 권한이 애플리케이션에 필요합니다.

- **Google Play 서비스 알림에 대한 권한** &ndash; Google Play 서비스의 원격 알림을 받을 수 있는 권한이 애플리케이션에 부여되어야 합니다.

- **위치 공급자에 대한 액세스** &ndash; 이것은 선택적 권한입니다.
   이 권한이 있으면 `GoogleMap` 클래스가 디바이스의 위치를 맵에 표시할 수 있습니다.

또한 Android 9는 bootclasspath에서 Apache HTTP 클라이언트 라이브러리를 제거했기 때문에 API 28 이상을 대상으로 하는 애플리케이션에서는 사용할 수 없습니다. API 28 이상을 대상으로 하는 애플리케이션에서 Apache HTTP 클라이언트를 계속 사용하려면 **AndroidManifest.xml** 파일의 `application` 노드에 다음 줄을 추가해야 합니다.

```xml
<application ...>
   ...
   <uses-library android:name="org.apache.http.legacy" android:required="false" />    
</application>
```

> [!NOTE]
> 아주 오래된 Google Play SDK 버전의 경우 앱이 `WRITE_EXTERNAL_STORAGE` 권한을 요청해야 했습니다. Google Play 서비스의 최근 Xamarin 바인딩에는 이러한 요구 사항이 더 이상 필요하지 않습니다.

다음 코드 조각은 **AndroidManifest.XML**에 추가해야 하는 설정의 예입니다.

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

**AndroidManifest.XML** 권한을 요청하는 것 외에, 앱은 `ACCESS_COARSE_LOCATION`과 `ACCESS_FINE_LOCATION` 권한에 대한 런타임 권한 검사도 수행해야 합니다. 런타임 권한 검사 수행에 대한 자세한 내용은 [Xamarin.Android 권한](~/android/app-fundamentals/permissions.md) 가이드를 참조하세요.

### <a name="create-an-emulator-with-google-apis"></a><a name="create-emulator-with-google-api" />Google API를 사용하여 에뮬레이터 만들기

Google Play 서비스를 사용하는 물리적 Android 디바이스가 설치되지 않은 경우 개발용 에뮬레이터 이미지를 만드는 것이 가능합니다. 자세한 내용은 [디바이스 관리자](~/android/get-started/installation/android-emulator/device-manager.md)를 참조하세요.

## <a name="the-googlemap-class"></a>GoogleMap 클래스

필수 조건이 충족되었으면, 애플리케이션 개발을 시작하고 Android Maps API를 사용할 차례입니다. [GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap) 클래스는 Xamarin.Android 애플리케이션이 Android용 Google Maps를 표시하고 상호 작용하는 데 사용하는 기본 API입니다. 이 클래스는 다음과 같은 기능을 담당합니다.

- Google 웹 서비스를 통해 애플리케이션을 승인하기 위해 Google Play 서비스와 상호 작용

- 맵 타일을 다운로드, 캐싱 및 표시

- 사용자에게 이동 및 확대/축소와 같은 UI 컨트롤을 표시

- 맵에 마커 및 도형 그리기

`GoogleMap`은 다음 두 가지 방법 중 하나로 활동에 추가됩니다.

- **MapFragment** - [MapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment)는 `GoogleMap` 개체의 호스트 역할을 하는 특수한 Fragment입니다. `MapFragment`에는 Android API 레벨 12 이상이 필요합니다.
   오래된 Android 버전은 [SupportMapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/SupportMapFragment)를 사용할 수 있습니다.  이 가이드는 `MapFragment` 클래스 사용에 중점을 두고 있습니다.

- **MapView** - [MapView](https://developers.google.com/android/reference/com/google/android/gms/maps/MapView)는 `GoogleMap` 개체의 호스트 역할을 할 수 있는 특수한 View 하위 클래스입니다. 이 클래스 사용자는 모든 활동 수명 주기 메서드를 `MapView` 클래스로 전달해야 합니다.

이러한 각각의 컨테이너는 `GoogleMap` 인스턴스를 반환하는 `Map` 속성을 노출합니다. [MapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment) 클래스는 개발자가 수동으로 구현해야 하는 상용구 코드의 양을 줄여주는 간단한 API이기 때문에 우선적으로 사용하는 것이 좋습니다

### <a name="adding-a-mapfragment-to-an-activity"></a>활동에 MapFragment 추가

다음 스크린샷은 간단한 `MapFragment`의 예입니다.

[![Google Map Fragment를 표시하는 디바이스 스크린샷](maps-api-images/image05-sml.png)](maps-api-images/image05.png#lightbox)

다른 Fragment 클래스와 마찬가지로 활동에 `MapFragment`를 추가하는 방법은 두 가지입니다.

- **선언적으로** - `MapFragment`는 활동에 대한 XML 레이아웃 파일을 통해 추가할 수 있습니다. 다음 XML 코드 조각은 `fragment` 요소를 사용하는 방법의 예를 보여줍니다.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <fragment xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/map"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              class="com.google.android.gms.maps.MapFragment" />
    ```

- **프로그래밍 방식으로** - `MapFragment`는 [`MapFragment.NewInstance`](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment.html#newInstance()) 메서드를 사용하여 프로그래밍 방식으로 인스턴스화한 다음, 활동에 추가할 수 있습니다. 이 코드 조각은 `MapFragment` 개체를 인스턴스화하고 활동에 추가하는 가장 간단한 방법을 보여줍니다.

    ```csharp
        var mapFrag = MapFragment.NewInstance();
        activity.FragmentManager.BeginTransaction()
                                .Add(Resource.Id.map_container, mapFrag, "map_fragment")
                                .Commit();

    ```

    [`GoogleMapOptions`](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) 개체를 `NewInstance`에 전달하여 `MapFragment` 개체를 구성하는 것이 가능합니다. 이 내용은 이 가이드의 뒷부분에 나오는 [GoogleMap 속성](#googlemap_object) 섹션에 언급되어 있습니다.

`MapFragment.GetMapAsync` 메서드는 fragment가 호스트하는 [`GoogleMap`](#googlemap_object)을 초기화하고 `MapFragment`가 호스트하는 맵 개체에 대한 참조를 확보하는 데 사용됩니다. 이 메서드는 `IOnMapReadyCallback` 인터페이스를 구현하는 개체를 사용합니다.

이 인터페이스에는 `IMapReadyCallback.OnMapReady(MapFragment map)`라는 단일 메서드가 있으며, 앱이 `GoogleMap` 개체와 상호 작용이 가능할 때 호출됩니다. 다음 코드 조각은 Android 활동이 `MapFragment`를 초기화하고 `IOnMapReadyCallback` 인터페이스를 구현하는 방법을 보여줍니다.

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

### <a name="map-types"></a>맵 유형

Google Maps API에서 사용할 수 있는 맵 유형은 다섯 가지입니다.

- **일반** - 기본 맵 유형입니다. 도로 및 중요한 자연 지형을 인공적인 관심 지점(예: 건물 및 교량)과 함께 보여줍니다.

- **위성** - 이 맵은 위성 사진을 보여줍니다.

- **하이브리드** - 이 맵은 위성 사진과 도로 지도를 보여줍니다.

- **지형** - 주로 일부 도로와 지형적인 특징을 보여줍니다.

- **없음** - 이 맵은 타일을 로드하지 않으며 빈 그리드로 렌더링됩니다.

아래 이미지는 왼쪽에서 오른쪽으로(일반, 하이브리드, 지형) 세 가지 유형의 맵을 보여줍니다.

[![세 가지 맵 예제 스크린샷: 일반, 하이브리드, 지형](maps-api-images/map-types-sml.png)](maps-api-images/map-types.png#lightbox)

`GoogleMap.MapType` 속성은 표시할 맵 유형을 설정하거나 변경하는 데 사용됩니다. 다음 코드 조각은 위성 맵을 표시하는 방법을 보여줍니다.

```csharp
public void OnMapReady(GoogleMap map)
{
    map.MapType = GoogleMap.MapTypeHybrid;
}
```

### <a name="googlemap-properties"></a><a name="googlemap_object" />GoogleMap 속성

`GoogleMap`은 맵의 기능과 모양을 제어할 수 있는 몇 가지 속성을 정의합니다. `GoogleMap`의 초기 상태를 구성하는 한 가지 방법은 `MapFragment`를 만들 때 [GoogleMapOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) 개체를 전달하는 것입니다. 다음 코드 조각은 `MapFragment`를 만들 때 `GoogleMapOptions` 개체를 사용하는 예입니다.

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

`GoogleMap`을 구성하는 다른 방법은 맵 개체의 [UiSettings](https://developers.google.com/android/reference/com/google/android/gms/maps/UiSettings)에서 속성을 조작하는 것입니다. 다음 코드 샘플은 확대/축소 컨트롤과 나침반을 표시하도록 `GoogleMap`을 구성하는 방법을 보여줍니다.

```csharp
public void OnMapReady(GoogleMap map)
{
    map.UiSettings.ZoomControlsEnabled = true;
    map.UiSettings.CompassEnabled = true;
}
```

## <a name="interacting-with-the-googlemap"></a>GoogleMap과 상호 작용

Android Maps API는 활동을 통해 시점을 변경하거나, 마커를 추가하거나, 사용자 지정 오버레이를 배치하거나, 도형을 그릴 수 있는 API를 제공합니다. 이 섹션에서는 Xamarin.Android에서 이러한 작업 중 일부를 수행하는 방법을 설명합니다.

### <a name="changing-the-viewpoint"></a>시점 변경

맵은 메르카토르 도법을 기반으로 화면에 평면으로 모델링됩니다. 맵 보기는 이 평면에서 똑바로 내려다 보는 *카메라* 보기입니다. 카메라의 위치는 위치, 확대/축소, 기울기 및 방향을 변경하여 제어할 수 있습니다. [CameraUpdate](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdate) 클래스는 카메라 위치를 이동하는 데 사용됩니다. `CameraUpdate` 개체는 직접 인스턴스화되지 않으며, 대신 Maps API가 [CameraUpdateFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdateFactory) 클래스를 제공합니다.

`CameraUpdate` 개체가 만들어지면 [GoogleMap.MoveCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#moveCamera(com.google.android.gms.maps.CameraUpdate)) 또는 [GoogleMap.AnimateCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#animateCamera(com.google.android.gms.maps.CameraUpdate)) 메서드에 매개 변수로 전달됩니다. `MoveCamera` 메서드는 맵을 즉시 업데이트하고 `AnimateCamera` 메서드는 부드러운 애니메이션 전환을 제공합니다.

이 코드 조각은 `CameraUpdateFactory`를 사용하여 맵의 확대/축소 수준을 한 수준 높이는 `CameraUpdate`를 만드는 간단한 예를 보여줍니다.

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...

public void OnMapReady(GoogleMap map)
{   
    map.MoveCamera(CameraUpdateFactory.ZoomIn());
}
```

Maps API는 카메라 위치에 가능한 모든 값을 집계하는 [CameraPosition](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.html)을 제공합니다. 이 클래스의 인스턴스는 `CameraUpdate` 개체를 반환하는 [CameraUpdateFactory.NewCameraPosition](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdateFactory#newCameraPosition%28com.google.android.gms.maps.model.CameraPosition%29) 메서드에 제공될 수 있습니다. 지도 api에는 `CameraPosition`개체를 만들기 위한 흐름 api를 제공하는 [CameraPosition.Builder](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.Builder.html) 클래스도 포함되어 있습니다.
다음 코드 조각은 `CameraPosition`에서 `CameraUpdate`를 만들고 이것을 사용하여 `GoogleMap`에서 카메라 위치를 변경하는 예를 보여줍니다.

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

이전 코드 조각에서 맵의 특정 위치는 [LatLng](https://developers.google.com/android/reference/com/google/android/gms/maps/model/LatLng) 클래스로 나타냅니다. 확대/축소 수준은 18로 설정되며, 이것은 Google Maps에 사용되는 임의의 확대/축소의 측정값입니다. 방향은 북쪽에서 시계 방향의 나침반 측정값입니다. 기울기 속성은 시야각을 제어하고 세로에서 25도 각도를 지정합니다. 다음 스크린샷은 위의 코드를 실행한 후 `GoogleMap`을 보여줍니다.

[![기울어진 시야각으로 지정된 위치를 보여주는 예제 Google Map](maps-api-images/image06-sml.png)](maps-api-images/image06.png#lightbox)

### <a name="drawing-on-the-map"></a>맵에 그리기

Android Maps API는 맵에 다음 항목을 그리는 API를 제공합니다.

- **마커** - 맵에서 단일 위치를 식별하는 데 사용되는 특수 아이콘입니다.

- **오버레이** - 맵에서 위치나 영역 컬렉션을 식별하는 데 사용할 수 있는 이미지입니다.

- **선, 다각형, 원** - 활동이 맵에 도형을 추가할 수 있도록 하는 API입니다.

#### <a name="markers"></a>Markers

Maps API는 맵의 단일 위치에 대한 모든 데이터를 캡슐화하는 [Marker](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) 클래스를 제공합니다. 기본적으로 Marker 클래스는 Google Maps가 제공하는 표준 아이콘을 사용합니다. 마커 모양을 사용자 지정하고 사용자 클릭에 응답할 수 있습니다.

##### <a name="adding-a-marker"></a>마커 추가

마커를 맵에 추가하려면 [MarkerOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions) 개체를 만든 다음, `GoogleMap` 인스턴스에서 [AddMarker](https://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addMarker%28com.google.android.gms.maps.model.MarkerOptions%29) 메서드를 호출해야 합니다. 이 메서드는 [Marker](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) 개체를 반환합니다.

```csharp
public void OnMapReady(GoogleMap map)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");

    map.AddMarker(markerOpt1);
}
```

사용자가 마커를 탭하면 *정보 창*에 마커 제목이 표시됩니다. 다음 스크린샷은 이 마커의 모양을 보여줍니다.

[![Vimy Ridge에 대한 마커 및 정보 창이 있는 Google Map 예](maps-api-images/image07-sml.png)](maps-api-images/image07.png#lightbox)

##### <a name="customizing-a-marker"></a>마커 사용자 지정

마커를 맵에 추가할 때 `MarkerOptions.InvokeIcon` 메서드를 호출하여 마커에 사용되는 아이콘을 사용자 지정할 수 있습니다.
이 메서드는 아이콘을 렌더링하는 데 필요한 데이터가 포함된 [BitmapDescriptor](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptor) 개체를 사용합니다. [BitmapDescriptorFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptorFactory) 클래스는 `BitmapDescriptor` 만들기를 간소화하는 몇 가지 도우미 메서드를 제공합니다. 이러한 메서드 중 일부를 다음 목록에서 소개합니다.

- `DefaultMarker(float colour)` &ndash; 기본 Google Maps 마커를 사용하지만 색을 변경합니다.

- `FromAsset(string assetName)` &ndash; 자산 폴더에서 지정된 파일의 사용자 지정 아이콘을 사용합니다.

- `FromBitmap(Bitmap image)` &ndash; 지정한 비트맵을 아이콘으로 사용합니다.

- `FromFile(string fileName)` &ndash; 지정된 경로에 있는 파일에서 사용자 지정 아이콘을 만듭니다.

- `FromResource(int resourceId)` &ndash; 지정된 리소스에서 사용자 지정 아이콘을 만듭니다.

다음 코드 조각은 녹청색 기본 마커를 만드는 예를 보여줍니다.

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

*정보 창*은 사용자가 특정 마커를 탭하면 정보를 표시하기 위해 팝업되는 특수 창입니다. 기본적으로 정보 창에는 마커 제목의 콘텐츠가 표시됩니다. 제목이 지정되지 않은 경우 정보 창이 표시되지 않습니다. 정보 창은 한 번에 하나만 표시될 수 있습니다.

[GoogleMap.IInfoWindowAdapter](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.InfoWindowAdapter) 인터페이스를 구현하여 정보 창을 사용자 지정할 수 있습니다. 이 인터페이스에는 두 가지 중요한 메서드가 있습니다.

- `public View GetInfoWindow(Marker marker)` &ndash; 이 메서드는 마커에 대한 사용자 지정 정보 창을 가져오기 위해 호출됩니다. `null`을 반환하면 기본 창 렌더링이 사용됩니다. 이 메서드가 View를 반환하면 View가 정보 창 프레임 내에 배치됩니다.

- `public View GetInfoContents(Marker marker)` &ndash; 이 메서드는 GetInfoWindow가 `null`을 반환하는 경우에만 호출됩니다. 이 메서드는 정보 창 콘텐츠의 기본 렌더링이 사용되는 경우 `null` 값을 반환합니다. 그렇지 않으면 이 메서드는 정보 창의 콘텐츠가 포함된 View를 반환합니다.

정보 창은 라이브 뷰가 아니라 Android가 뷰를 정적 비트 맵으로 변환하여 이미지에 표시하는 것입니다. 즉, 정보 창은 터치 이벤트나 제스처에 응답할 수 없고 자동으로 알아서 업데이트되지 않습니다. 정보 창을 업데이트하려면 [GoogleMap.ShowInfoWindow](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker.html#showInfoWindow()) 메서드를 호출해야 합니다.

다음 이미지는 일부 사용자 지정된 정보 창의 몇 가지 예를 보여줍니다. 왼쪽 이미지에는 콘텐츠가 사용자 지정되어 있고 오른쪽 이미지에는 모서리가 둥글게 사용자 지정된 창과 콘텐츠가 있습니다.

![아이콘과 인구가 포함된 멜버른의 마커 창 예제 오른쪽 창에는 둥근 모서리가 있습니다.](maps-api-images/marker-infowindows.png)

#### <a name="groundoverlays"></a>GroundOverlays

맵에서 특정 위치를 식별하는 마커와 달리 [GroundOverlay](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlay)는 맵의 영역 또는 위치 컬렉션을 식별하는 데 사용되는 이미지입니다.

##### <a name="adding-a-groundoverlay"></a>GroundOverlay 추가

맵에 지면 오버레이를 추가하는 것은 맵에 마커를 추가하는 것과 유사합니다. 먼저 [GroundOverlayOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlayOptions) 개체가 생성됩니다. 그런 다음, 이 개체가 [`GoogleMap.AddGroundOverlay`](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addGroundOverlay(com.google.android.gms.maps.model.GroundOverlayOptions)) 메서드에 매개 변수로 전달되면 `GroundOverlay` 개체가 반환됩니다. 이 코드 조각은 맵에 지면 오버레이를 추가하는 예제입니다.

```csharp
BitmapDescriptor image = BitmapDescriptorFactory.FromResource(Resource.Drawable.polarbear);
GroundOverlayOptions groundOverlayOptions = new GroundOverlayOptions()
    .Position(position, 150, 200)
    .InvokeImage(image);
GroundOverlay myOverlay = googleMap.AddGroundOverlay(groundOverlayOptions);
```

다음 스크린샷은 이 오버레이를 맵에 보여줍니다.

[![북극곰 이미지가 오버레이된 예제 맵](maps-api-images/image09-sml.png)](maps-api-images/image09.png#lightbox)

#### <a name="lines-circles-and-polygons"></a>선, 원, 다각형

맵에 추가할 수 있는 간단한 유형의 기하도형은 세 가지입니다.

- **폴리라인** - 일련의 연결된 선분입니다. 맵에 경로를 표시하거나 도형을 만들 수 있습니다.

- **원** - 맵에 원이 그려집니다.

- **다각형** - 맵에 영역을 표시하는 닫힌 도형입니다.

##### <a name="polylines"></a>폴리라인

[폴리라인](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Polyline)은 각 선분의 꼭짓점을 지정하는 연속 `LatLng` 개체 목록입니다. 폴리라인은 먼저 `PolylineOptions` 개체를 만들고 여기에 점을 추가하여 만들어집니다. 그러면 `PolylineOption` 개체가 `AddPolyline` 메서드를 호출하여 `GoogleMap` 개체로 전달됩니다.

```csharp
PolylineOption rectOptions = new PolylineOption();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
rectOptions.Add(new LatLng(37.35, -122.0)); // close the polyline - this makes a rectangle.

googleMap.AddPolyline(rectOptions);
```

##### <a name="circles"></a>원

원은 먼저 원의 중심과 반지름을 미터 단위로 지정하는 [CircleOption](https://developers.google.com/android/reference/com/google/android/gms/maps/model/CircleOptions) 개체를 인스턴스화하여 만들어집니다. 원은 [GoogleMap.AddCircle](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addCircle(com.google.android.gms.maps.model.CircleOptions))을 호출하여 맵에 그려집니다.
다음 코드 조각은 원을 그리는 방법을 보여줍니다.

```csharp
CircleOptions circleOptions = new CircleOptions ();
circleOptions.InvokeCenter (new LatLng(37.4, -122.1));
circleOptions.InvokeRadius (1000);

googleMap.AddCircle (circleOptions);
```

##### <a name="polygons"></a>다각형

`Polygon`은 `Polyline`과 유사하지만 열려있지 않습니다. `Polygon`은 닫힌 고리이며 내부가 채워져 있습니다.
`Polygon`은 호출된 [GoogleMap.AddPolygon](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addPolygon(com.google.android.gms.maps.model.PolygonOptions)) 메서드를 제외하고 `Polyline`과 똑같은 방식으로 만들어집니다.

`Polyline`과 달리 `Polygon`은 자체적으로 닫힙니다. 다각형은 첫 번째와 마지막 점을 연결하는 선을 그려서 `AddPolygon` 메서드로 닫힙니다. 다음 코드 조각은 `Polyline` 예의 이전 코드 조작과 동일한 영역에 사각형을 만듭니다.

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

사용자가 맵과 상호 작용할 수 있는 유형은 세 가지입니다.

- **마커 클릭** - 사용자가 마커를 클릭합니다.

- **마커 끌기** - 사용자가 mparger를 길게 클릭합니다.

- **정보 창 클릭** - 사용자가 정보 창을 클릭합니다.

이러한 각 이벤트는 아래에서 자세히 설명합니다.

### <a name="marker-click-events"></a>마커 클릭 이벤트

사용자가 마커를 탭하면 `MarkerClicked` 이벤트가 발생합니다. 이 이벤트는 `GoogleMap.MarkerClickEventArgs` 개체를 매개 변수로 받아들입니다. 이 클래스에는 두 가지 속성이 들어 있습니다.

- `GoogleMap.MarkerClickEventArgs.Handled` &ndash; 이벤트 처리기가 이벤트를 사용했음을 나타내려면 이 속성을 `true`로 설정해야 합니다. `false`로 설정되면 이벤트 처리기의 사용자 지정 동작 외에 기본 동작이 발생합니다.

- `Marker` &ndash; 이 속성은 `MarkerClick` 이벤트를 발생시킨 마커에 대한 참조입니다.

이 코드 조각은 맵에서 카메라 위치를 새 위치로 변경하는 `MarkerClick`의 예를 보여줍니다.

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

### <a name="marker-drag-events"></a>마커 끌기 이벤트

이 이벤트는 사용자가 마커를 끌려고 할 때 발생합니다. 기본적으로 마커는 끌기가 가능하지 않습니다. `Marker.Draggable` 속성을 `true`로 설정하거나 `true`를 매개 변수로 사용하여 `MarkerOptions.Draggable` 메서드를 호출하면 마커를 끌기가 가능하도록 설정할 수 있습니다.

마커를 끌려면 먼저 사용자가 마커를 길게 클릭한 다음, 손가락을 맵에 그대로 두어야 합니다. 화면에서 사용자가 손가락을 끌면 마커가 이동합니다. 사용자가 화면에서 손가락을 들어 올려도 마커는 그대로 유지됩니다.

다음 목록은 끌기 가능 마커에 대해 발생하는 다양한 이벤트를 설명합니다.

- `GoogleMap.MarkerDragStart(object sender, GoogleMap.MarkerDragStartEventArgs e)` &ndash;   이 이벤트는 사용자가 마커를 처음 끌 때 발생합니다.

- `GoogleMap.MarkerDrag(object sender, GoogleMap.MarkerDragEventArgs e)` &ndash;   이 이벤트는 마커를 끄는 동안 발생합니다.

- `GoogleMap.MarkerDragEnd(object sender, GoogleMap.MarkerDragEndEventArgs e)` &ndash;   이 이벤트는 사용자가 마커 끌기를 마치면 발생합니다.

각 `EventArgs` 끌고 있는 `Marker` 개체에 대한 참조인 `P0`이라는 단일 속성이 포함되어 있습니다.

### <a name="info-window-click-events"></a>정보 창 클릭 이벤트

정보 창은 한 번에 하나만 표시할 수 있습니다. 사용자가 맵에서 정보 창을 클릭하면 맵 개체가 `InfoWindowClick` 이벤트를 발생시킵니다. 다음 코드 조각은 처리기를 이벤트에 연결하는 방법을 보여줍니다.

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

다시 말하지만 정보 창은 맵에 이미지로 렌더링되는 정적 `View`입니다. 정보 창 내에 배치되는 단추, 확인란 또는 텍스트 뷰와 같은 위젯은 비활성이며 필수적인 사용자 이벤트에 응답할 수 없습니다.

## <a name="related-links"></a>관련 링크

- [SimpleMapDemo](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3/SimpleMapDemo)
- [Google Play 서비스](https://developers.google.com/android/guides/overview)
- [Google Maps Android API v2](https://developers.google.com/maps/documentation/android-sdk/intro)
- [Google Play 서비스 APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)
- [Google Maps API 키 가져오기](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [uses-library](https://developer.android.com/guide/topics/manifest/uses-library-element)
- [uses-feature](https://developer.android.com/guide/topics/manifest/uses-feature-element)
