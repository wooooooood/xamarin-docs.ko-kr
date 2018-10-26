---
title: 위치 서비스
description: 이 가이드는 Android 응용 프로그램에서 인식 되는 위치를 소개 하 고 Google 위치 서비스 API를 사용 하 여 사용 가능한 퓨즈 위치 공급자 뿐만 아니라 Android 위치 서비스 API를 사용 하 여 사용자의 위치를 가져오는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 0008682B-6CEF-0C1D-3200-56ECF58F5D3C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/22/2018
ms.openlocfilehash: e3cfc9a345c8ab92b35ad428b550ec42de6312e5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120296"
---
# <a name="location-services"></a>위치 서비스

_이 가이드는 Android 응용 프로그램에서 인식 되는 위치를 소개 하 고 Google 위치 서비스 API를 사용 하 여 사용 가능한 퓨즈 위치 공급자 뿐만 아니라 Android 위치 서비스 API를 사용 하 여 사용자의 위치를 가져오는 방법을 보여 줍니다._

## <a name="location-services-overview"></a>위치 서비스 개요

Android 셀 타워 위치, Wi-fi 및 GPS와 같은 다양 한 위치 기술에 대 한 액세스를 제공합니다. 통해 각 위치 기술 세부 정보는 추상화 *위치 공급자*, 응용 프로그램에 사용 되는 공급자에 관계 없이 동일한 방식으로 위치를 가져올 수 있습니다. 이 가이드에서는 퓨즈 위치 공급자를 지능적으로 가장 좋은 방법은 어떤 공급자를 사용할 수 및 장치 사용 방법에 따라 장치의 위치를 결정 하는 Google Play Services의 일부를 소개 합니다. Android 위치 서비스 API 및 시스템의 위치를 사용 하 여 통신 하는 방법을 보여 줍니다. 사용 하 여 서비스를 `LocationManager`입니다. 이 가이드의 두 번째 부분에서는 사용 하 여 Android 위치 서비스 API는 `LocationManager`합니다.
 
일반으로 응용 프로그램 퓨즈 위치 공급자를 사용 하면 이전 Android 위치 서비스 API를 필요한 경우에 대체 하려고 합니다.

## <a name="location-fundamentals"></a>위치 기본 사항

Android에서 위치 데이터를 사용 하 여 작업에 대해 선택한 어떤 API 든 몇 가지 개념이 동일 합니다. 이 섹션에서는 위치 공급자 및 위치와 관련 된 사용 권한 소개 합니다.

### <a name="location-providers"></a>위치 공급자

사용자의 위치를 정확 하 게 여러 가지 기술은 내부적으로 사용 됩니다. 사용 되는 하드웨어의 유형에 따라 달라 집니다 *위치 공급자* 데이터를 수집 하는 작업에 대 한 선택입니다. Android에서는 세 가지 위치 공급자를 사용합니다.

-   **GPS 공급자** &ndash; GPS 가장 정확한 위치를 제공, 가장 power를 사용 하 여 및 옥외 가장 잘 작동 합니다. 이 공급자에 GPS와 보조 GPS 조합을 사용 하 여 ([aGPS](http://en.wikipedia.org/wiki/Assisted_GPS)), 셀룰러 towers에 의해 수집 된 GPS 데이터를 반환 하는 합니다.

-   **네트워크 공급자** &ndash; 셀 towers에서 수집한 aGPS 데이터를 포함 하 여 Wi-fi 및 셀룰러 데이터의 조합을 제공 합니다. GPS 공급자 보다 전원 사용 하지만 다양 한 정확도 위치 데이터를 반환 합니다.

-   **수동 공급자** &ndash; 다른 응용 프로그램 및 서비스에서 요청 하는 공급자를 사용 하 여 응용 프로그램에서 위치 데이터를 생성 하는 "추가 긍정" 옵션입니다. 이 작업에 대 한 상수 위치 업데이트를 필요로 하지 않는 응용 프로그램에 전원 절약 옵션 이상적 이지만 안정성이 저하 됩니다.

위치 공급자 항상 사용 가능한 것은 아닙니다. 예를 들어, GPS를 사용 하 여 응용 프로그램에서 하고자 할 수 있습니다 하지만 GPS 설정에서 해제 될 수 있습니다 또는 장치가 없을 GPS 전혀 합니다. 특정 공급자를 사용할 수 없는 경우 해당 공급자를 반환할 수 있습니다 선택 `null`합니다.

### <a name="location-permissions"></a>위치 권한이

위치 인식 응용 프로그램을 GPS, Wi-fi 및 셀룰러 데이터를 수신 하는 장치의 하드웨어 센서에 액세스 해야 합니다. Android 매니페스트는 응용 프로그램에서 적절 한 권한을 통해 액세스가 제어 됩니다.
두 사용 권한을 가지 &ndash; 하나를 허용 하려고 응용 프로그램의 요구 사항 및 API 선택에 따라:

-   `ACCESS_FINE_LOCATION` &ndash; GPS에 대 한 응용 프로그램 액세스를 허용 합니다.
    에 필요한 합니다 *GPS 공급자* 하 고 *수동 공급자* 옵션 (*수동 공급자는 다른 응용 프로그램 또는 서비스에 의해 수집 된 GPS 데이터를 액세스할 수 있는 권한이 있어야*). 에 대 한 선택적 권한을 합니다 *네트워크 공급자*합니다.

-   `ACCESS_COARSE_LOCATION` &ndash; 셀룰러 및 Wi-fi 위치에 대 한 응용 프로그램 액세스를 허용 합니다. 에 필요한 *네트워크 공급자* 경우 `ACCESS_FINE_LOCATION` 설정 되지 않았습니다.

API 21 (Android 5.0 Lollipop) 버전을 대상으로 하는 앱에 대 한 이상 설정할 수 있습니다. 또는 `ACCESS_FINE_LOCATION` 및 GPS 하드웨어에 있지 않은 장치에서 계속 실행 합니다. 하는 경우 앱에 필요한 GPS 하드웨어를 명시적으로 추가 해야는 `android.hardware.location.gps` `uses-feature` Android 매니페스트 요소입니다. 자세한 내용은 참조는 Android [사용 하 여 기능](https://developer.android.com/guide/topics/manifest/uses-feature-element.html) 요소 참조 합니다.

사용 권한을 설정 하려면 확장 합니다 **속성** 폴더에는 **Solution Pad** 두 번 클릭 하 고 **AndroidManifest.xml**합니다. 사용 권한을 나열 됩니다 **필요한 권한**:

[![Android 매니페스트 필요한 사용 권한 설정의 스크린샷](location-images/location-01-xs.png)](location-images/location-01-xs.png#lightbox)

이러한 사용 권한 중 하나가 설정에 응용 프로그램 위치 공급자에 액세스 하기 위해 사용자의 권한이 필요는 Android가 알립니다. 장치는 API 수준 22 (Android 5.1)를 실행 또는 낮은 앱이 설치 될 때마다 이러한 권한을 부여 하려면 사용자는 요청입니다. API를 실행 하는 장치에서 레벨 23 (Android 6.0) 또는 이상 앱 위치 공급자의 요청을 하기 전에 런타임 권한 검사를 수행 해야 합니다. 

> [!NOTE]
>참고: 설정 `ACCESS_FINE_LOCATION` 모두 거친 및 세밀 하 게 위치 데이터에 대 한 액세스를 의미 합니다. 만 모두 권한을 설정할 필요가 해야 합니다 *최소* 앱 작동 하는 데 필요한 사용 권한.

이 코드 조각은 앱에 대 한 권한이 있는지 확인 하는 방법의 예는 `ACCESS_FINE_LOCATION` 권한:

```csharp
 if (ContextCompat.CheckSelfPermission(this, Manifest.Permission.AccessFineLocation) == Permission.Granted)
{
    StartRequestingLocationUpdates();
    isRequestingLocationUpdates = true;
}
else
{
    // The app does not have permission ACCESS_FINE_LOCATION 
}
```

앱 수 있는 사용자 권한을 부여 하지 것입니다 (또는 사용 권한 해지) 시나리오를 허용 하 고 해당 상황을 정상적으로 처리 하는 방법이 해야 합니다. 참조 하세요 합니다 [사용 권한 가이드](~/android/app-fundamentals/permissions.md) 런타임 권한 구현에 대 한 자세한 내용은 Xamarin.Android에서 검사 합니다.


## <a name="using-the-fused-location-provider"></a>퓨즈 위치 공급자를 사용 하 여

위치 공급자 런타임 배터리 효율적인 방식으로 최상의 위치 정보를 제공 하는 동안 효율적으로 선택 하므로 장치에서 위치 업데이트를 수신 하는 Android 응용 프로그램에 대 한 기본 방법은 퓨즈 위치 공급자입니다. 예를 들어 야외를 따라 이동 하는 사용자는 GPS를 사용 하 여 읽기 최상의 위치를 가져옵니다. 사용자는 다음 실내 안내, 여기서 GPS 작동 잘못 (있는 경우), 퓨즈 위치 공급자 WiFi를 작동 실내 향상으로 자동으로 전환할 수 있습니다.
 
퓨즈 위치 공급자 API는 다양 한 지 오 펜싱 및 활동 모니터링을 포함 하 여 위치 인식 응용 프로그램에 권한을 부여 하는 다른 도구를 제공 합니다. 이 섹션에서는 하겠습니다 포커스를 설정 하는 기본 사항에는 `LocationClient`, 공급자, 설정 및 사용자의 위치를 가져오는 중입니다.

퓨즈 위치 공급자의 일부인 [Google Play Services](http://developer.android.com/google/play-services/index.html)합니다.
Google Play 서비스 패키지를 설치 하 고 퓨즈 위치 공급자 API에 대 한 응용 프로그램 작동에 올바르게 구성 해야 하며 장치는 Google Play Services APK 설치 되어 있어야 합니다.

Xamarin.Android 하기 전에 응용 프로그램 수 퓨즈 위치 공급자를 사용 하면 추가 해야 합니다 **Xamarin.GooglePlayServices.Maps** 프로젝트에 패키지 합니다. 또한 다음 `using` 문 아래에 설명 된 클래스를 참조 하는 모든 원본 파일에 추가 해야 합니다.

```csharp
using Android.Gms.Common;
using Android.Gms.Location;
```

### <a name="checking-if-google-play-services-is-installed"></a>Google Play Services가 설치 되어 있는지 확인 합니다.

Google Play Services 설치 되어 있지 않으면 퓨즈 위치 공급자를 사용 하려는 경우 크래시 (또는 만료)는 Xamarin.Android는 다음 런타임 예외가 발생 합니다.  Google Play 서비스를 설치 하지 않은 경우 응용 프로그램은 대체 항목 위에서 설명한 Android 위치 서비스입니다. Google Play 서비스를 오래 된 앱을 Google Play Services의 설치 된 버전을 업데이트 하 라는 사용자에 게 메시지를 표시할 수 있습니다.

이 코드 조각은 다음과 같습니다. 어떻게 Android 활동 수 프로그래밍 방식으로 Google Play Services 설치 되어 있는지 확인의 예

```csharp
bool IsGooglePlayServicesInstalled()
{
    var queryResult = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable(this);
    if (queryResult == ConnectionResult.Success)
    {
        Log.Info("MainActivity", "Google Play Services is installed on this device.");
        return true;
    }

    if (GoogleApiAvailability.Instance.IsUserResolvableError(queryResult))
    {
        // Check if there is a way the user can resolve the issue
        var errorString = GoogleApiAvailability.Instance.GetErrorString(queryResult);
        Log.Error("MainActivity", "There is a problem with Google Play Services on this device: {0} - {1}",
                  queryResult, errorString);
                  
        // Alternately, display the error to the user.
    }

    return false;
}
```

### <a name="fusedlocationproviderclient"></a>FusedLocationProviderClient

퓨즈 위치 공급자와 상호 작용을 하는 Xamarin.Android 응용 프로그램의 인스턴스가 있어야 합니다 `FusedLocationProviderClient`합니다. 이 클래스는 위치 업데이트를 구독 하는 데 장치의 마지막 알려진된 위치를 검색 하 고 필요한 메서드를 노출 합니다.

`OnCreate` 활동의 메서드는에 대 한 참조를 볼 수 있는 적합 한 곳의 `FusedLocationProviderClient`다음 코드 조각에서 설명한 것 처럼:

```csharp
public class MainActivity: AppCompatActivity
{
    FusedLocationProviderClient fusedLocationProviderClient;

    protected override void OnCreate(Bundle bundle) 
    {
        fusedLocationProviderClient = LocationServices.GetFusedLocationProviderClient(this);
    }
}
```

### <a name="getting-the-last-known-location"></a>마지막으로 알려진된 위치를 가져오는 중

`FusedLocationProviderClient.GetLastLocationAsync()` 메서드를 사용 하 여 최소한의 코딩 오버 헤드 장치의 마지막 알려진된 위치를 빠르게 가져올 Xamarin.Android 응용 프로그램에 대 한 간단 하 고 비차단 방식으로 제공 합니다.

이 코드 조각을 사용 하는 방법을 보여 줍니다는 `GetLastLocationAsync` 장치의 위치를 검색 하는 방법.

```csharp
async Task GetLastLocationFromDevice()
{
    // This method assumes that the necessary run-time permission checks have succeeded.
    getLastLocationButton.SetText(Resource.String.getting_last_location);
    Android.Locations.Location location = await fusedLocationProviderClient.GetLastLocationAsync();

    if (location == null)
    {
        // Seldom happens, but should code that handles this scenario
    }
    else
    {
        // Do something with the location 
        Log.Debug("Sample", "The latitude is " + location.Latitude);
    }
}
```

### <a name="subscribing-to-location-updates"></a>위치에 구독 업데이트

Xamarin.Android 응용 프로그램을 사용 하 여 퓨즈 위치 공급자 로부터 위치 업데이트를 구독할 수도 `FusedLocationProviderClient.RequestLocationUpdatesAsync` 메서드를이 코드 조각과 같이:

```csharp
await fusedLocationProviderClient.RequestLocationUpdatesAsync(locationRequest, locationCallback);
```
이 메서드는 두 개의 매개 변수를 사용합니다.

-   **`Android.Gms.Location.LocationRequest`** &ndash; `LocationRequest` 개체는 Xamarin.Android 응용 프로그램을 퓨즈 위치 공급자 해야 작동 하는 방식에 매개 변수를 전달 하는 방법입니다. `LocationRequest` 정보가 이러한 방법을 자주 요청 해야 또는 얼마나 중요 한 정확한 위치 업데이트 해야 합니다. 예를 들어, 중요 한 위치를 요청 하는 장치에서 위치를 결정할 때, GPS 및 결과적으로 더 많은 전원을 사용할 하면 합니다. 이 코드 조각을 만드는 방법을 보여 줍니다는 `LocationRequest` 높은 정확도 사용 하 여 위치에 대해 위치 업데이트 (하지만 요청 간에 2 분 보다 일찍 없습니다)에 대 한 분 마다 5 개 정도 확인 합니다. 퓨즈 위치 공급자가 사용 된 `LocationRequest` 장치 위치를 결정 하는 동안 사용 하는 위치 공급자에 대 한 지침:

    ```csharp
    LocationRequest locationRequest = new LocationRequest()
                                      .SetPriority(LocationRequest.PriorityHighAccuracy)
                                      .SetInterval(60 * 1000 * 5)
                                      .SetFastestInterval(60 * 1000 * 2);
    ```
                                          

-   **`Android.Gms.Location.LocationCallback`** &ndash; 위치 업데이트를 수신 하려면 Xamarin.Android 응용 프로그램 해야 서브 클래스는 `LocationProvider` 추상 클래스입니다. 이 클래스가 노출 퓨즈 위치 공급자의 위치 정보를 사용 하 여 앱을 업데이트 하 여 두 개의 메서드 호출 수 있습니다. 아래에서 자세히 설명 합니다.

퓨즈 위치 공급자 위치 업데이트의 Xamarin.Android 응용 프로그램을 알림 호출을 `LocationCallBack.OnLocationResult(LocationResult result)`입니다. `Android.Gms.Location.LocationResult` 매개 변수 업데이트 위치 정보가 포함 됩니다.

퓨즈 위치 공급자 변경 위치 데이터의 가용성에서을 감지 하면 호출 된 `LocationProvider.OnLocationAvaibility(LocationAvailability
locationAvailability)` 메서드. 경우는 `LocationAvailability.IsLocationAvailable` 속성에서 반환 `true`, 다음에서 장치 위치 결과 보고 있는지 간주 될 수 있습니다 `OnLocationResult` 으로 정확 하 고 최신으로 필요에 따라를 `LocationRequest`합니다. 하는 경우 `IsLocationAvailable` 가 false 이면 아무 위치에서 반환 될 `OnLocationResult`합니다.

이 코드의 예제 구현은 `LocationCallback` 개체:

```csharp
public class FusedLocationProviderCallback : LocationCallback
{
    readonly MainActivity activity;

    public FusedLocationProviderCallback(MainActivity activity)
    {
        this.activity = activity;
    }

    public override void OnLocationAvailability(LocationAvailability locationAvailability)
    {
        Log.Debug("FusedLocationProviderSample", "IsLocationAvailable: {0}",locationAvailability.IsLocationAvailable);
    }

    public override void OnLocationResult(LocationResult result)
    {
        if (result.Locations.Any())
        {
            var location = result.Locations.First();
            Log.Debug("Sample", "The latitude is :" + location.Latitude);
        }
        else
        {
            // No locations to work with.
        }
    }
}
```

## <a name="using-the-android-location-service-api"></a>Android 위치 서비스 API를 사용 하 여

Android 위치 서비스 위치 정보를 사용 하 여 android에 대 한 이전 API입니다. 위치 데이터는 하드웨어 센서에서 수집 되 고 응용 프로그램에 액세스할 수 있는 시스템 서비스에서 수집 되는 `LocationManager` 클래스 및 `ILocationListener`합니다.

위치 서비스 Google Play Services 설치 되지 않은 장치에서 실행 해야 하는 응용 프로그램에 가장 적합 합니다.

위치 서비스는 특수 유형의 [서비스](http://developer.android.com/guide/components/services.html) 시스템에 의해 관리 됩니다. 시스템 서비스는 장치 하드웨어와 상호 작용 하 고 항상 실행 됩니다. 응용 프로그램에서 위치 업데이트를 활용 하려면 가입할 위치 업데이트를 사용 하 여 시스템 위치 서비스를 `LocationManager` 및 `RequestLocationUpdates` 호출 합니다.

Android 위치 서비스를 사용 하 여 사용자의 위치를 가져오려면 여러 단계가 포함 됩니다.

1.  에 대 한 참조를 `LocationManager` 서비스입니다.
2.  구현 된 `ILocationListener` 인터페이스 및 핸들 이벤트는 위치가 변경 되 면 합니다.
3.  사용 된 `LocationManager` 지정된 된 공급자에 대 한 요청 위치 업데이트입니다. 합니다 `ILocationListener` 이전 단계에서 사용할에서 콜백을 받으려면는 `LocationManager`합니다.
4.  응용 프로그램이 더 이상 업데이트를 수신 하는 적절 한 위치 업데이트를 중지 합니다.

### <a name="location-manager"></a>위치 관리자

인스턴스를 사용 하 여 시스템 위치 서비스에 액세스할 수 있습니다는 `LocationManager` 클래스입니다. `LocationManager` 시스템 위치 서비스와 상호 작용 하 고 메서드를 호출할 수 있는 특수 클래스입니다. 응용 프로그램에 대 한 참조를 가져올 수는 `LocationManager` 를 호출 하 여 `GetSystemService` 아래와 같이 서비스 형식의 전달:

```csharp
LocationManager locationManager = (LocationManager) GetSystemService(Context.LocationService);
```

`OnCreate` 참조 하는 것이 좋습니다는 `LocationManager`합니다.
유지 하는 것이 좋습니다는 `LocationManager` 클래스 변수로에서는 작업 수명 주기에서 다양 한 지점에서 호출할 수 있도록 합니다.

### <a name="request-location-updates-from-the-locationmanager"></a>LocationManager에서 위치 업데이트를 요청 합니다.

응용 프로그램에 대 한 참조에는 `LocationManager`, 지시 해야를 `LocationManager` 어떤 유형의 위치 정보는 필수 이며 해당 정보를 업데이트 하는 빈도입니다. 호출 하 여이 작업을 수행할 `RequestionLocationUpdates` 에 `LocationManager` 개체 및 위치 업데이트를 받을 콜백 및 업데이트에 대 한 몇 가지 조건에 전달 합니다. 구현 해야 하는 형식이이 콜백에서 `ILocationListener` 인터페이스 (자세한 내용은이 가이드 뒷부분에서에서 설명).

`RequestionLocationUpdates` 메서드의 지시에 따라 시스템 위치 서비스 응용 프로그램 위치 업데이트 수신을 시작 하려고 합니다. 이 메서드를 사용 하는 공급자 뿐만 아니라 시간 및 거리 임계값 업데이트 빈도 제어할 수를 지정할 수 있습니다. 예를 들어, 요청 위치 아래의 아래 메서드 업데이트 GPS 위치 공급자 로부터 2000 밀리초 마다 위치 1 미터 두 개를 변경 하는 경우에:

```csharp
// For this example, this method is part of a class that implements ILocationListener, described below
locationManager.RequestLocationUpdates(LocationManager.GpsProvider, 2000, 1, this);
```

응용 프로그램이 종종 성능이 우수한 응용 프로그램에 필요한 대로 위치 업데이트를 요청 해야 합니다. 이 배터리를 유지 하 고 사용자에 대 한 더 나은 환경을 만듭니다.

### <a name="responding-to-updates-from-the-locationmanager"></a>LocationManager에서 업데이트에 응답

응용 프로그램에서 업데이트를 요청한 후 합니다 `LocationManager`를 구현 하 여 서비스에서 정보를 받을 수는 [ `ILocationListener` ](https://developer.xamarin.com/api/type/Android.Locations.ILocationListener/) 인터페이스. 이 인터페이스는 서비스 위치 및 위치 공급자를 수신 대기 하는 네 가지가 제공 `OnLocationChanged`합니다. 시스템은 호출 `OnLocationChanged` 경우 사용자의 위치가 변경 위치 업데이트를 요청할 때 설정 된 조건에 따라 위치 변경 내용으로 한 정하는 데 충분 합니다. 

다음 코드에서 메서드를 보여 줍니다.는 `ILocationListener` 인터페이스:

```csharp
public class MainActivity : AppCompatActivity, ILocationListener
{
    TextView latitude;
    TextView longitude;
    
    public void OnLocationChanged (Location location)
    {
        // called when the location has been updated.
    }
    
    public OnProviderDisabled(string locationProvider)
    {
        // called when the user disables the provider
    }
    
    public OnProviderEnabled(string locationProvider)
    {
        // called when the user enables the provider
    }
    
    public OnStatusChanged(string locationProvider, Availability status, Bundle extras)
    {
        // called when the status of the provider changes (there are a variety of reasons for this)
    }
}
```

### <a name="unsubscribing-to-locationmanager-updates"></a>LocationManager 업데이트를 구독 취소

시스템 리소스를 절약 하기 위해 응용 프로그램 구독을 취소 해야 위치 업데이트를 가능한 한 빨리 합니다. `RemoveUpdates` 메서드의 지시에 따라는 `LocationManager` 업데이트를 보내기 위해 응용 프로그램을 중지 합니다.  예를 들어 활동 호출할 수 있습니다 `RemoveUpdates` 에 `OnPause` 메서드 전원을 절약 하기 위해 응용 프로그램 수 있도록 필요가 위치 업데이트 화면에는 해당 작업:

```csharp
protected override void OnPause ()
{
    base.OnPause ();
    locationManager.RemoveUpdates (this);
}
```

응용 프로그램을 백그라운드에서 위치 업데이트 해야 하는 경우 시스템 위치 서비스를 구독 하는 사용자 지정 서비스를 만들려는 것이 좋습니다. 참조를 [Android 서비스를 사용 하 여 Backgrounding](~/android/app-fundamentals/services/index.md) 자세한 가이드입니다.

### <a name="determining-the-best-location-provider-for-the-locationmanager"></a>LocationManager에 대 한 최상의 위치 공급자를 확인합니다.

위의 응용 프로그램 GPS 위치 공급자를 설정합니다. 그러나 GPS 하지 못할 모든 경우에서와 같은 장치 실내는 또는 GPS 수신기는 없는 경우. 이 경우 결과는 `null` 공급자에 대해 반환 합니다.

GPS를 사용할 수 없는 경우 작동 하도록 앱을 가져오려면, 사용 된 `GetBestProvider` 응용 프로그램 시작 시 최상의 (장치 지원 및 사용자 지원)에 대 한 사용 가능한 위치로 공급자에 게 요청 하는 방법입니다. 특정 공급자를 전달 하는 대신 알 수 있습니다 `GetBestProvider` 사용 하 여 정확도 및 전원-와 같은 공급자의 요구 사항을 [ `Criteria` 개체](https://developer.xamarin.com/api/type/Android.Locations.Criteria/)합니다. `GetBestProvider` 지정된 된 조건에 대 한 최상의 공급자를 반환합니다.

다음 코드에 가장 사용 가능한 공급자를 가져오고 위치 업데이트를 요청할 때 사용 하는 방법을 보여 줍니다.

```csharp
Criteria locationCriteria = new Criteria();   
locationCriteria.Accuracy = Accuracy.Coarse;
locationCriteria.PowerRequirement = Power.Medium;

locationProvider = locationManager.GetBestProvider(locationCriteria, true);

if(locationProvider != null)
{
    locationManager.RequestLocationUpdates (locationProvider, 2000, 1, this);
}
else
{
    Log.Info(tag, "No location providers available");
}
```

> [!NOTE]
>  사용자가 모든 위치 공급자를 비활성화 하는 경우 `GetBestProvider` 돌아갑니다 `null`합니다. 이 코드는 실제 장치에서 작동 하는 방법을 보려면, GPS, Wi-fi 및 셀룰러 네트워크에서 사용 하도록 설정 될 **Google 설정 > 위치 > 모드** 이 스크린샷에 표시 된 대로:

[![Android 휴대폰에서 설정 위치 모드 화면](location-images/location-02.png)](location-images/location-02.png#lightbox)

아래 스크린샷에서 응용 프로그램 실행 하 여 위치 하는 방법을 보여 줍니다 `GetBestProvider`:

[![GetBestProvider 앱 위도, 경도 및 공급자를 표시 합니다.](location-images/location-03.png)](location-images/location-03.png#lightbox)

에 유의 `GetBestProvider` 공급자를 동적으로 변경 되지 않습니다. 대신, 작업 수명 주기 동안 한 번 가장 사용 가능한 공급자를 결정합니다. 응용 프로그램에서 코드를 추가 해야이 설정 된 후 공급자 상태를 변경 하는 경우는 `ILocationListener` 메서드 &ndash; `OnProviderEnabled`를 `OnProviderDisabled`, 및 `OnStatusChanged` &ndash; 와 관련 된 모든 가능성을 처리 하는 공급자 스위치입니다.

## <a name="summary"></a>요약

이 가이드는 Android 위치 서비스와 Google 위치 서비스 API에서 퓨즈 위치 공급자를 사용 하 여 사용자의 위치 가져오기 설명 합니다.


## <a name="related-links"></a>관련 링크

- [위치 (샘플)](https://developer.xamarin.com/samples/Location/)
- [FusedLocationProvider (샘플)](https://developer.xamarin.com/samples/FusedLocationProvider/)
- [Google Play 서비스](http://developer.android.com/google/play-services/index.html)
- [기준 클래스](https://developer.xamarin.com/api/type/Android.Locations.Criteria/)
- [LocationManager 클래스](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/)
- [LocationListener 클래스](https://developer.xamarin.com/api/type/Android.Locations.ILocationListener/)
- [LocationClient API](http://developer.android.com/reference/com/google/android/gms/location/LocationClient.html)
- [LocationListener API](http://developer.android.com/reference/com/google/android/gms/location/LocationListener.html)
- [LocationRequest API](https://developer.android.com/reference/com/google/android/gms/location/LocationRequest.html)
