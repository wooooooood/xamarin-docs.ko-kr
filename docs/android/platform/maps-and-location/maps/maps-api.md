---
title: 응용 프로그램에서 Google Maps API를 사용 하 여
description: Xamarin.Android 응용 프로그램에서 Google Maps API v2 기능을 구현 하는 방법입니다.
ms.prod: xamarin
ms.assetid: C0589878-2D04-180E-A5B9-BB41D5AF6E02
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/07/2018
ms.openlocfilehash: 12ff6f615b30e53704fee6368c9d7f171f881df0
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671067"
---
# <a name="using-the-google-maps-api-in-your-application"></a>Google Maps API를 사용 하 여 응용 프로그램에서

Maps 응용 프로그램을 사용 하는 것은 유용 하지만 응용 프로그램에서 직접 지도 포함 하려는 경우가 있습니다. 맵 응용 프로그램을 기본 제공 하는 것 외에도 Google도 제공 합니다.는 [Android 용 네이티브 매핑 API](https://developers.google.com/maps/documentation/android-sdk/intro)합니다.
Maps API를 통한 매핑 환경 더 많은 제어를 유지 관리 하려는 경우에 적합 합니다. Maps API를 사용할 수 있는 사항은 다음과 같습니다.

-  Map의 관점을 프로그래밍 방식으로 변경 합니다.
-  추가 및 표식을 사용자 지정 합니다.
-  오버레이 사용 하 여 맵의 주석을 추가 합니다.

이제 사용 되지 않는 Google Maps Android API v1 달리 Google Maps Android API v2의 일부인 [Google Play Services](https://developers.google.com/android/guides/overview)합니다.
Xamarin.Android 앱을 Google Maps Android API를 사용할 수 있기 전에 몇 가지 필수 조건 충족 해야 합니다.


## <a name="google-maps-api-prerequisites"></a>Google Maps API 필수 조건

몇 가지 단계를 Maps API를 사용 하기 전에 수행 해야 합니다. 포함 하 여:

-  [Maps API 키를 얻으려면](#obtain-maps-key)
-  [Google Play Services SDK 설치](#install-gps-sdk)
-  [NuGet에서 Xamarin.GooglePlayServices.Maps 패키지를 설치 합니다.](#install-gpsmaps-nuget)
-  [필요한 사용 권한을 지정 합니다.](#declare-permissions)
-  [필요에 따라 Google Api를 사용 하 여 에뮬레이터를 만들기](#create-emulator-with-google-api)


### <a name="a-nameobtain-maps-key-obtain-a-google-maps-api-key"></a><a name="obtain-maps-key" />Google Maps API 키를 가져오려면

첫 번째 단계 (참고 레거시 Google Maps v1 API에서 API 키를 다시 사용할 수 없습니다) Google Maps API 키를 가져오는 것입니다. Xamarin.Android를 사용 하 여 API 키를 사용 하는 방법에 대 한 정보를 참조 하세요 [는 Google Maps API 키 가져오기](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)합니다.
 

### <a name="a-nameinstall-gps-sdk--install-the-google-play-services-sdk"></a><a name="install-gps-sdk" /> Google Play Services SDK 설치

Google Play Services에 Google +, 앱 내 청구, 및 지도 같은 다양 한 Google 기능을 활용 하려면 Android 응용 프로그램을 허용 하는 Google에서 기술입니다. 이러한 기능에 포함 된 백그라운드 서비스로 Android 장치에서 액세스할 수 합니다 [Google Play Services APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)합니다.

Google Play Services 클라이언트 라이브러리를 통해 Google Play 서비스를 사용 하 여 android 응용 프로그램 상호 작용 합니다. 이 라이브러리는 인터페이스 및 맵과 같은 개별 서비스에 대 한 클래스를 포함합니다. 다음 다이어그램은 Android 응용 프로그램 및 Google Play 서비스 간의 관계를 보여줍니다.

![Google Play Services APK를 업데이트 하는 Google Play 스토어를 보여 주는 다이어그램](maps-api-images/play-services-diagram.png)

Android Maps API는 Google Play 서비스의 일부로 제공 됩니다.
사용 하 여 Google Play Services SDK을 설치 해야 하는지 Xamarin.Android 응용 프로그램 맵 API를 사용 하려면 먼저 합니다 [Android SDK Manager](~/android/get-started/installation/android-sdk.md)합니다. 다음 스크린샷에서 Google Play 서비스 클라이언트를 찾을 수 있습니다 Android SDK Manager의 위치를 보여줍니다.

![Google Play 서비스 추가 Android SDK Manager의 아래에 나타납니다.](maps-api-images/image01.png)

> [!NOTE]
> Google Play services APK는 모든 장치에 있을 수 있는 사용이 허가 된 제품. 이 설치 되어 있지 않으면 장치에서 Google Maps 작동 하지 않습니다.

### <a name="a-nameinstall-gpsmaps-nuget--install-the-xamaringoogleplayservicesmaps-package-from-nuget"></a><a name="install-gpsmaps-nuget" /> NuGet에서 Xamarin.GooglePlayServices.Maps 패키지를 설치 합니다.

합니다 [Xamarin.GooglePlayServices.Maps 패키지](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Maps) Google Play Services Maps API에 대 한 Xamarin.Android 바인딩을 포함 합니다.
Google Play 서비스 맵 패키지를 추가 하려면 마우스 오른쪽 단추로 클릭 합니다 **참조가** 고 솔루션 탐색기에서 프로젝트의 폴더 **NuGet 패키지 관리...** :

![참조에서 보여주는 솔루션 탐색기 NuGet 패키지 관리 상황에 맞는 메뉴 항목](maps-api-images/image02.png)

열립니다는 **NuGet 패키지 관리자**합니다. 클릭 **찾아보기** enter **Xamarin Google Play 서비스 맵** 검색 필드에 있습니다. 선택 **Xamarin.GooglePlayServices.Maps** 누릅니다 **설치**합니다. (이 패키지는 이전에 설치 되어 있던, 클릭 **업데이트**.):

[![선택한 Xamarin.GooglePlayServices.Maps 패키지를 사용 하 여 NuGet 패키지 관리자](maps-api-images/image03-sml.png)](maps-api-images/image03.png#lightbox)

다음 종속성 패키지도 설치 되어 있는지 확인 합니다.

-   **Xamarin.GooglePlayServices.Base**
-   **Xamarin.GooglePlayServices.Basement**
-   **Xamarin.GooglePlayServices.Tasks**

### <a name="a-namedeclare-permissions--specify-the-required-permissions"></a><a name="declare-permissions" /> 필요한 사용 권한을 지정 합니다.

앱에는 Google Maps API를 사용 하려면 하드웨어 및 사용 권한 요구 사항을 식별 해야 합니다.  Google Play Services SDK에서 일부 사용 권한을 자동으로 부여 됩니다 및 개발자를 명시적으로 추가할 필요는 없습니다 **AndroidManfest.XML**:

-  **네트워크 상태에 대 한 액세스** &ndash; Maps API 지도 타일을 다운로드할 수 하는 경우를 확인할 수 있어야 합니다.

-  **인터넷에 액세스할** &ndash; 인터넷 액세스는 맵 타일을 다운로드 하 고 API 액세스에 대 한 Google Play 서버와 통신 하는 데 필요한 합니다.

에 다음 사용 권한 및 기능을 지정 해야 합니다 **AndroidManifest.XML** Google Maps Android API에 대 한 합니다.

-  **OpenGL ES v2** &ndash; 응용 프로그램에는 OpenGL ES v2에 대 한 요구 사항을 선언 해야 합니다.

-  **Google Maps API 키** &ndash; 응용 프로그램이 등록 되 고 Google Play 서비스를 사용할 수 있는 권한이 있는지 확인 하려면 API 키가 사용 됩니다. 참조 [Google Maps API 키를 가져오는](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md) 이 키에 대 한 세부 정보에 대 한 합니다.
   
- **레거시 Apache HTTP 클라이언트 요청** &ndash; Android 9.0 (API 레벨 28)를 대상으로 하는 앱 또는 위에 임을 지정 해야 기존 Apache HTTP 클라이언트를 사용 하는 선택적 라이브러리입니다. 

-  **Google 웹 기반 서비스에 액세스할** &ndash; 응용 프로그램에는 Android Maps API를 지 Google의 웹 서비스에 액세스할 수 있는 권한이 필요 합니다.

-  **Google Play 서비스 알림에 대 한 사용 권한** &ndash; 응용 프로그램에 Google Play 서비스에서 원격 알림을 받을 수 있는 권한을 부여 해야 합니다.

-  **위치 공급자에 대 한 액세스** &ndash; 권한은 선택 사항입니다.
   이러한 자습서를 통해 여 `GoogleMap` 맵에 장치 위치를 표시 하는 클래스입니다.


> [!NOTE]
> Google Play SDK의 아주 오래 된 버전에서는 앱이 요청은 `WRITE_EXTERNAL_STORAGE` 권한. 이 요구 사항을 Google Play 서비스에 대 한 최근 Xamarin 바인딩을 사용 하 여 필요한 경우 더 이상

다음 코드 조각에 추가 되어야 하는 설정의 한 예로 **AndroidManifest.XML**:

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
    </application>
</manifest>
```

사용 권한을 요청 하는 것 외에도 **AndroidManifest.XML**를 앱에 대 한 런타임 권한을 검사도 수행 해야 합니다는 `ACCESS_COARSE_LOCATION` 및 `ACCESS_FINE_LOCATION` 권한. 참조를 [Xamarin.Android 권한을](~/android/app-fundamentals/permissions.md) 런타임 권한 검사를 수행 하는 방법에 대 한 자세한 내용은 가이드입니다.


### <a name="a-namecreate-emulator-with-google-api-create-an-emulator-with-google-apis"></a><a name="create-emulator-with-google-api" />Google Api를 사용 하 여 에뮬레이터 만들기

Google Play 서비스를 사용 하 여 물리적 Android 장치가 설치 되어 있지는 개발에 대 한 에뮬레이터 이미지를 만들 수 것입니다. 자세한 내용은 참조는 [장치 관리자](~/android/get-started/installation/android-emulator/device-manager.md)합니다.


## <a name="the-googlemap-class"></a>GoogleMap 클래스

필수 조건이 충족 되 면 응용 프로그램 개발을 시작 하 여 Android Maps API를 사용 하 여 차례입니다. 합니다 [GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap) 클래스는 Xamarin.Android 응용 프로그램을 표시 하 고 Android 용 Google 지도와 상호 작용에 사용할 기본 API입니다. 이 클래스에는 다음 책임이 있습니다.

-  Google 웹 서비스를 사용 하 여 응용 프로그램을 인증 하려면 Google Play 서비스와 상호 작용 합니다.

-  다운로드, 캐싱, 및 지도 타일을 표시 합니다.

-  와 같은 UI 컨트롤을 표시 하는 이동 및 사용자에 게 확대/축소 합니다.

-  지도에 표식 및 기 하 도형을 그리기입니다.

`GoogleMap` 활동에 두 가지 방법 중 하나에 추가 됩니다.

-  **MapFragment** - [MapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment) 역할을 호스트 하는 특수화 된 조각는 `GoogleMap` 개체입니다. `MapFragment` Android API 수준 12 이상이 필요 합니다.
   이전 버전의 Android에서 사용할 수는 [SupportMapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/SupportMapFragment)합니다.  이 가이드를 사용 하 여 중점을 `MapFragment` 클래스입니다.

-  **MapView** - [MapView](https://developers.google.com/android/reference/com/google/android/gms/maps/MapView) 특수화 된 보기 하위 클래스에 대 한 호스트 역할을 수행할 수 있는를 `GoogleMap` 개체입니다. 이 클래스의 사용자는 모든 활동 수명 주기 메서드를 전달 해야 합니다는 `MapView` 클래스입니다.

각이 컨테이너를 노출 하는 `Map` 의 인스턴스를 반환 하는 속성 `GoogleMap`합니다. 기본 설정에 부여 해야 합니다 [MapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment) 개발자 수동으로 구현 해야 하는 상용구 코드 양을 줄일 수 있는 간단한 API를 그대로 클래스입니다.

### <a name="adding-a-mapfragment-to-an-activity"></a>활동을 MapFragment 추가

다음 스크린샷에서 간단한 예가 `MapFragment`:

[![Google 지도 조각을 표시 하는 장치의 스크린샷](maps-api-images/image05-sml.png)](maps-api-images/image05.png#lightbox)

다른 조각 클래스와 마찬가지로 두 가지 추가 하는 `MapFragment` 활동:

-   **선언적** - `MapFragment` 활동에 대 한 XML 레이아웃 파일을 통해 추가할 수 있습니다. 다음 XML 코드 조각을 사용 하는 방법의 예를 보여 줍니다.는 `fragment` 요소:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <fragment xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/map"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              class="com.google.android.gms.maps.MapFragment" />
    ```

-   **프로그래밍 방식으로** - `MapFragment` 사용 하 여 프로그래밍 방식으로 인스턴스화할 수 있습니다 합니다 [ `MapFragment.NewInstance` ](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment.html#newInstance()) 메서드 후 활동에 추가 합니다. 이 조각을 인스턴스화하는 가장 간단한 방법은 사용 하는 `MapFragment` 활동에 추가 합니다.
    
    ```csharp
        var mapFrag = MapFragment.NewInstance();
        activity.FragmentManager.BeginTransaction()
                                .Add(Resource.Id.map_container, mapFrag, "map_fragment")
                                .Commit();

    ```

    구성 하는 것이 불가능 합니다 `MapFragment` 전달 하 여 개체를 [ `GoogleMapOptions` ](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) 개체를 `NewInstance`입니다. 이 섹션에 설명 되어 [GoogleMap 속성](#googlemap_object) 이 가이드에 나중에 표시 됩니다.

`MapFragment.GetMapAsync` 메서드 초기화를 사용 하는 [ `GoogleMap` ](#googlemap_object) 조각에서 호스트 되는 및에서 호스트 되는 map 개체에 대 한 참조는 `MapFragment`합니다. 이 메서드는 구현 하는 개체는 `IOnMapReadyCallback` 인터페이스입니다. 

이 인터페이스에는 단일 메서드 `IMapReadyCallback.OnMapReady(MapFragment map)` 상호 작용 하는 앱에 대 한 가능한 경우 호출 되는 여 `GoogleMap` 개체입니다. 다음 코드 조각은 Android 활동을 초기화할 수는 방법을 보여 줍니다.는 `MapFragment` 하 고 구현 된 `IOnMapReadyCallback` 인터페이스:
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

Google Maps API에서 사용할 수 있는 5 가지 유형의 지도:

-  **보통** -기본 지도 형식입니다. 도 및 중요 한 자연 스러운 기능 (예: 건물 및 브리지) 관심 임의의 점수와 함께 보여 줍니다.

-  **위성** -이 맵은 위성 사진을 보여 줍니다.

-  **하이브리드** -이 맵에 위성 사진 표시 하 고도 매핑합니다.

-  **지형** -주로 일부도 사용 하 여 지형적 특징을 보여 줍니다.

-  **None** -빈 그리드가으로 렌더링 하므로,이 맵은 모든 타일을 로드 하지 않습니다.


아래 이미지에서는 왼쪽에서 오른쪽 (보통, 하이브리드, 지형)에서 다양 한 유형의 지도 중 3 개를 보여 줍니다.

[![세 가지 지도 예제 스크린샷. 보통, 하이브리드 및 지형](maps-api-images/map-types-sml.png)](maps-api-images/map-types.png#lightbox)

`GoogleMap.MapType` 속성은 설정 하거나 표시 되는 지도 유형을 변경 하려면 사용 합니다. 다음 코드 조각에는 위성 지도 표시 하는 방법을 보여 줍니다.

```csharp
public void OnMapReady(GoogleMap map)
{
    map.MapType = GoogleMap.MapTypeHybrid;
}
```


### <a name="a-namegooglemapobject-googlemap-properties"></a><a name="googlemap_object" />GoogleMap 속성

`GoogleMap` map의 모양과 기능을 제어할 수 있는 몇 가지 속성을 정의 합니다. 초기 상태를 구성 하는 한 가지 방법은 `GoogleMap` 전달 하는 것을 [GoogleMapOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) 만들면 개체를 `MapFragment`입니다. 다음 코드 조각은 사용 하는 한 가지 예는 `GoogleMapOptions` 만들 때 개체를 `MapFragment`:

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

구성 하는 다른 방법은 `GoogleMap` 에서 속성을 조작 하는 것은 [UiSettings](https://developers.google.com/android/reference/com/google/android/gms/maps/UiSettings) 지도 개체의 합니다. 다음 코드 샘플을 구성 하는 방법을 보여 줍니다는 `GoogleMap` 컴퍼스 및 확대/축소 컨트롤 표시 하려면:

```csharp
public void OnMapReady(GoogleMap map)
{
    map.UiSettings.ZoomControlsEnabled = true;
    map.UiSettings.CompassEnabled = true;
}
```

## <a name="interacting-with-the-googlemap"></a>GoogleMap 상호 작용

Android Maps API 관점 변경, 표식, 사용자 지정 오버레이 배치 하거나 기 하 도형을 그리기 작업을 허용 하는 Api를 제공 합니다. 이 섹션에서는 Xamarin.Android에서 이러한 태스크 중 일부를 수행 하는 방법을 설명 합니다.

### <a name="changing-the-viewpoint"></a>관점 변경

맵 기반 메 르 카 토르 도법으로 화면의 평면으로 모델링 됩니다. 지도 보기에는 메시지를 표시 합니다는 *카메라* 바로 아래로이 평면에서 보기. 위치, 확대/축소, 기울기, 변경 하 고 포함 하 여 카메라의 위치를 제어할 수 있습니다. 합니다 [CameraUpdate](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdate) 클래스 카메라 위치를 이동 하는 데 사용 됩니다. `CameraUpdate` 개체는 직접 인스턴스화되지, Maps API를 제공 하는 대신 합니다 [CameraUpdateFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdateFactory) 클래스입니다.

한 번을 `CameraUpdate` 개체가 생성 되었음을을 매개 변수로 전달 합니다 [GoogleMap.MoveCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#moveCamera(com.google.android.gms.maps.CameraUpdate)) 또는 [GoogleMap.AnimateCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#animateCamera(com.google.android.gms.maps.CameraUpdate)) 메서드. 합니다 `MoveCamera` 메서드는 동안 즉시 맵 업데이트는 `AnimateCamera` 메서드 원활 하 고 애니메이션 전환을 제공 합니다.

이 코드 조각은 사용 하는 방법의 간단한 예는 `CameraUpdateFactory` 만들려면는 `CameraUpdate` 하나의 확대/축소 수준에 따라 지도의 확대/축소 수준을 증가 하는:

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...

public void OnMapReady(GoogleMap map)
{   
    map.MoveCamera(CameraUpdateFactory.ZoomIn());
}
```

Maps API를 제공 된 [CameraPosition](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.html) 카메라 위치에 대 한 가능한 값의 모든 집계는 합니다. 이 클래스의 인스턴스를 지정할 수는 [CameraUpdateFactory.NewCameraPosition](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdateFactory#newCameraPosition%28com.google.android.gms.maps.model.CameraPosition%29) 반환 하는 메서드를 `CameraUpdate` 개체입니다. Maps API도 포함 되어 있습니다 합니다 [CameraPosition.Builder](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.Builder.html) 클래스를 만들기 위한 fluent API를 제공 하는 `CameraPosition` 개체입니다.
다음 코드 조각을 만드는 예를 보여 줍니다.는 `CameraUpdate` 에서 `CameraPosition` 는에서 카메라 위치를 변경 하는 데 사용 된 `GoogleMap`:

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

맵에서 특정 위치를 나타내는 이전 코드 조각에는 [LatLng](https://developers.google.com/android/reference/com/google/android/gms/maps/model/LatLng) 클래스입니다. 확대/축소 수준은 측정 하는 임의의 Google Maps에서 사용 하는 확대/축소는 18로 설정 됩니다. 베어링 나침반 북쪽에서 시계 방향으로 측정 됩니다. 기울기 속성 각도 컨트롤과 세로에서 25 각도의 각도 지정 합니다. 다음 스크린 샷에 표시 된 `GoogleMap` 앞의 코드를 실행 한 후:

[![기운를 사용 하 여 지정된 된 위치를 표시 하는 예제 Google 맵입니다 보는 각도](maps-api-images/image06-sml.png)](maps-api-images/image06.png#lightbox)


### <a name="drawing-on-the-map"></a>지도 그리기

Android Maps API를 지도에 다음 항목을 그리기 위한 API를 제공 합니다.

-  **표식** -이들은 맵에서 단일 위치를 식별 하는 데 사용 되는 특수 아이콘이 있습니다.

-  **오버레이** -위치 또는 영역 맵의 컬렉션을 식별 하는 이미지입니다.

-  **선, 다각형 및 원** -도형 맵에 추가 하는 작업을 허용 하는 Api는 합니다.


#### <a name="markers"></a>Markers

Maps API를 제공 된 [표식](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) 맵에서 단일 위치에 대 한 데이터를 모두 캡슐화 하는 클래스입니다. 표식은 기본적으로 클래스는 Google Maps에서 제공 하는 표준 아이콘을 사용 합니다. 표식의 모양을 사용자 지정할 수 있으며 사용자가 클릭에 응답 하는 것입니다.


##### <a name="adding-a-marker"></a>마커 추가

표식에 지도를 추가 하려면 새 [MarkerOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions) 개체를 호출 합니다 [AddMarker](https://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addMarker%28com.google.android.gms.maps.model.MarkerOptions%29) 메서드를를 `GoogleMap` 인스턴스. 이 메서드는 반환 된 [표식](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) 개체입니다.

```csharp
public void OnMapReady(GoogleMap map)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");
    
    map.AddMarker(markerOpt1);
}
```

표식의 제목에 표시 됩니다는 *정보 창의* 사용자 표식에가 하는 경우. 다음 스크린샷은이 표식 모양을 보여 줍니다.

[![Vimy Ridge에 대 한 정보 창과 마커를 사용 하 여 Google 지도 예](maps-api-images/image07-sml.png)](maps-api-images/image07.png#lightbox)


##### <a name="customizing-a-marker"></a>표식을 사용자 지정

호출 하 여 마커로 사용 하는 아이콘을 사용자 지정할 수 있기를 `MarkerOptions.InvokeIcon` 메서드 표식 지도에 추가 하는 경우.
이 메서드는 [BitmapDescriptor](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptor) 아이콘을 렌더링 하는 데 필요한 데이터를 포함 하는 개체입니다. 합니다 [BitmapDescriptorFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptorFactory) 생성을 간소화 하기 위해 일부 도우미 메서드를 제공 하는 클래스는 `BitmapDescriptor`합니다. 다음은 이러한 메서드 중 일부를 제공합니다.

-   `DefaultMarker(float colour)` &ndash; 기본 Google 지도 표식에 사용 되지만 컬러를 변경 합니다.

-   `FromAsset(string assetName)` &ndash; Assets 폴더에 지정된 된 파일에서 사용자 지정 아이콘을 사용 합니다.

-   `FromBitmap(Bitmap image)` &ndash; 지정한 비트맵에서 아이콘으로 사용 합니다.

-   `FromFile(string fileName)` &ndash; 지정된 된 경로에 파일에서 사용자 지정 아이콘을 만듭니다.

-   `FromResource(int resourceId)` &ndash; 지정된 된 리소스에서 사용자 지정 아이콘을 만듭니다.

다음 코드 조각은 녹청 색이 지정 된 기본 표식을 만드는 예제를 보여 줍니다.

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

#### <a name="info-windows"></a>Windows 정보

*정보 windows* 특별 한 windows 특정 표식 탭 할 때 사용자에 게 정보를 표시 하는 팝업 됩니다. 기본적으로 표식의 제목 내용의 정보 창에 표시 됩니다. 제목을 할당 되지 않은 경우 정보 창이 표시 됩니다. 한 번에 하나만 정보 창의 표시 될 수 있습니다.

구현 하 여 정보 창을 사용자 지정할 수는 [GoogleMap.IInfoWindowAdapter](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.InfoWindowAdapter) 인터페이스입니다. 이 인터페이스에서 두 가지 중요 한 메서드는

-  `public View GetInfoWindow(Marker marker)` &ndash; 이 메서드는 사용자 지정 정보 창의 표식에 가져오려는 호출 됩니다. 반환 하는 경우 `null` , 기본 창 렌더링이 사용 됩니다. 이 메서드는 뷰를 반환 하는 경우 해당 보기 정보 창 프레임 내에서 배치 됩니다.

-  `public View GetInfoContents(Marker marker)` &ndash; 이 메서드 GetInfoWindow 반환 하는 경우에 호출할 수 `null` 입니다. 이 메서드가 반환할 수는 `null` 정보 창 내용의 기본 렌더링 하는 데 값입니다. 그렇지 않은 경우이 메서드 정보 창의 콘텐츠를 사용 하 여 뷰를 반환 해야 합니다.

정보 창에는 라이브 뷰입니다-Android는 뷰의 정적 비트맵으로 변환 하 고 이미지에 표시 하는 대신 합니다. 즉, 하는 정보 창의 제스처를 나 터치 이벤트에 응답할 수 없습니다 나이 자동으로 자동으로 업데이트 됩니다. 정보 창에는 업데이트를 호출 하는 데 필요한 것은 [GoogleMap.ShowInfoWindow](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker.html#showInfoWindow()) 메서드.

다음 이미지에는 몇 가지 예가 일부 사용자 지정된 정보 창 보여 줍니다. 왼쪽 이미지에는 오른쪽에 있는 이미지에 해당 창에 있는 동안 사용자 지정 내용 및 둥근된 모서리를 사용 하 여 사용자 지정 하는 내용:

![표식 windows 멜버른, 아이콘 등 모집단에 대 한 예제입니다. 오른쪽 창 모서리가 둥근 합니다.](maps-api-images/marker-infowindows.png)

#### <a name="groundoverlays"></a>GroundOverlays

맵에서 특정 위치를 식별 하는 마커 달리를 [GroundOverlay](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlay) 위치나 맵에서 영역 컬렉션을 식별 하는 데 사용 되는 이미지입니다.

##### <a name="adding-a-groundoverlay"></a>GroundOverlay 추가

지도에 처음부터 오버레이 추가 하는 것을 표식 지도에 추가 하는 것과 비슷합니다. 첫 번째는 [GroundOverlayOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlayOptions) 개체가 만들어집니다. 이 개체를 매개 변수로 전달 되는 [ `GoogleMap.AddGroundOverlay` ](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addGroundOverlay(com.google.android.gms.maps.model.GroundOverlayOptions)) 반환 하는 메서드를 `GroundOverlay` 개체입니다. 이 코드 조각은 다음과 같습니다. 지도를 처음부터 오버레이 추가 하는 예제

```csharp
BitmapDescriptor image = BitmapDescriptorFactory.FromResource(Resource.Drawable.polarbear);
GroundOverlayOptions groundOverlayOptions = new GroundOverlayOptions()
    .Position(position, 150, 200)
    .InvokeImage(image);
GroundOverlay myOverlay = googleMap.AddGroundOverlay(groundOverlayOptions);
```

다음 스크린 샷에서 맵에서이 오버레이 보여 줍니다.

[![예제는 북극곰의 오버레이할 이미지 맵으로](maps-api-images/image09-sml.png)](maps-api-images/image09.png#lightbox)


#### <a name="lines-circles-and-polygons"></a>선, 원 및 다각형

기하학적 도형 맵에 추가할 수 있는 간단한에 세 가지 종류가 있습니다.

-  **다중선** -일련의 연결 된 선 세그먼트입니다. 지도에 경로 표시 하거나는 기 하 도형을 만들 수 있습니다.

-  **원** -이 맵에 원 그리기 됩니다.

-  **다각형** -지도의 영역을 표시 하기 위한 닫힌된 도형입니다.


##### <a name="polylines"></a>다중선

A [다중선](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Polyline) 연속 목록이 `LatLng` 각 선 세그먼트의 꼭 짓 점을 지정 하는 개체입니다. 첫 번째 만들어 다중선 만들어집니다는 `PolylineOptions` 개체 및 요소를 추가 합니다. 합니다 `PolylineOption` 개체를 전달 합니다는 `GoogleMap` 호출 하 여 개체를 `AddPolyline` 메서드.

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

원 첫 번째 인스턴스화하여 생성 되는 [CircleOption](https://developers.google.com/android/reference/com/google/android/gms/maps/model/CircleOptions) metres의 가운데와 원의 반지름을 지정 하는 개체입니다. 원을 호출 하 여 지도에 그릴 [GoogleMap.AddCircle](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addCircle(com.google.android.gms.maps.model.CircleOptions))합니다.
다음 코드 조각은 원을 그리는 방법을 보여 줍니다.

```csharp
CircleOptions circleOptions = new CircleOptions ();
circleOptions.InvokeCenter (new LatLng(37.4, -122.1));
circleOptions.InvokeRadius (1000);

googleMap.AddCircle (circleOptions);
```


##### <a name="polygons"></a>다각형

`Polygon`하지만 동일 `Polyline`s 열려 있지 않은 종료 합니다. `Polygon`s 폐쇄형된 루프 되며 채워진 해당 내부를 갖습니다.
`Polygon`정확히 동일한 방식으로 만들어진를 `Polyline`를 제외 하 고는 [GoogleMap.AddPolygon](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addPolygon(com.google.android.gms.maps.model.PolygonOptions)) 메서드를 호출 합니다.

와 달리를 `Polyline`, `Polygon` 자체 닫는 중입니다. 다각형을 닫습니다 해제는 `AddPolygon` 의 첫 번째 및 마지막 지점을 연결 하는 선을 그려 메서드. 다음 코드 조각은 이전 코드 조각에서와 동일한 영역을 통해 실선의 사각형이 만들어집니다는 `Polyline` 예제입니다.

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

맵을 사용 하 여 사용자가 상호 작용 하는 방법은 세 종류가 있습니다.

-  **표식 클릭** -사용자가 표식을 클릭 합니다.

-  **표식 끌어서** -사용자가 장기-클릭을 mparger에서

-  **정보 창 클릭** -사용자가 정보 창에서를 클릭 합니다.

이러한 각 이벤트 아래에서 자세히 설명 합니다.


### <a name="marker-click-events"></a>표식 이벤트를 클릭 합니다.

`MarkerClicked` 표식에서 사용자가 누를 때 이벤트가 발생 합니다. 이 이벤트는는 `GoogleMap.MarkerClickEventArgs` 개체를 매개 변수로 합니다. 이 클래스에는 두 가지 속성이 포함 됩니다.

-  `GoogleMap.MarkerClickEventArgs.Handled` &ndash; 이 속성에 설정할 `true` 를 나타내는 이벤트 처리기가 이벤트를 사용 했습니다. 이 값을 설정 하는 경우 `false` 기본 동작 이벤트 처리기의 사용자 지정 동작 외에도 발생 합니다.

-  `Marker` &ndash; 이 속성이 발생 시킨 표식에 대 한 참조를 `MarkerClick` 이벤트입니다.


이 코드 예제는 `MarkerClick` 카메라 위치를 지도에 새 위치로 바뀝니다는:

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


### <a name="marker-drag-events"></a>끌기 이벤트 표식

이 이벤트는 사용자 표식을 끕니다 하고자 할 때 발생 합니다. 기본적으로 표식 draggable 되지 않습니다. 설정 하 여으로 draggable 표식의 설정할 수 있습니다는 `Marker.Draggable` 속성을 `true` 호출 하 여 합니다 `MarkerOptions.Draggable` 메서드를 `true` 매개 변수로 합니다.

마커를 끌기, 사용자 해야 먼저 장기-클릭 표식에 대 고 해당 손가락 지도 켜져 있어야 합니다. 주위 화면에서 사용자의 손가락을 끌 때 표식 이동 합니다. 사용자의 손가락을 화면 리프트, 표식 위치에 유지 됩니다.

다음 목록에는 draggable 표식에 발생할 수 있는 다양 한 이벤트를 설명 합니다.

-   `GoogleMap.MarkerDragStart(object sender, GoogleMap.MarkerDragStartEventArgs e)` &ndash; 이 이벤트는 처음으로 가져왔을 표식의 경우 발생 합니다.

-   `GoogleMap.MarkerDrag(object sender, GoogleMap.MarkerDragEventArgs e)` &ndash; 마커를 끄는 대로 이벤트가 발생 합니다.

-   `GoogleMap.MarkerDragEnd(object sender, GoogleMap.MarkerDragEndEventArgs e)` &ndash; 이 이벤트는 사용자가 완료 되 면 발생 합니다. 표식을 끌어 합니다.

각 합니다 `EventArgs` 라는 단일 속성을 포함 `P0` 에 대 한 참조는는 `Marker` 끄는 개체입니다.


### <a name="info-window-click-events"></a>정보 창에서 클릭 이벤트

한 번에 하나만 정보 창을 표시할 수 있습니다. Map 개체에서 발생 하는 지도에 정보 창에서 사용자가는 `InfoWindowClick` 이벤트입니다. 다음 코드 조각에는 이벤트 처리기를 연결 하는 방법을 보여 줍니다.

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

정보 창에는 정적은 `View` 맵에서 이미지로 렌더링 되는 합니다. 정보 창에 위치한 텍스트 뷰 단추, 확인란 등 위젯을 버려지는 단순한 되 고 해당 정수 계열 사용자 이벤트에 응답할 수 없습니다.


## <a name="related-links"></a>관련 링크

- [SimpleMapDemo](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3/SimpleMapDemo)
- [Google Play 서비스](https://developers.google.com/android/guides/overview)
- [Google Android API v2 맵](https://developers.google.com/maps/documentation/android-sdk/intro)
- [Google Play는 APK를 서비스](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)
- [Google Maps API 키 가져오기](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [uses-library](https://developer.android.com/guide/topics/manifest/uses-library-element)
- [uses-feature](https://developer.android.com/guide/topics/manifest/uses-feature-element)
