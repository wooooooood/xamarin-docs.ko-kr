---
title: Android의 위치 서비스
description: 이 가이드에서는 Android 애플리케이션의 위치 인식 기능을 소개하고, Android Location Service API를 사용하여 사용자의 위치를 가져오는 방법과 Google Location Services API에서 사용할 수 있는 융합 위치 공급자를 보여줍니다.
ms.prod: xamarin
ms.assetid: 0008682B-6CEF-0C1D-3200-56ECF58F5D3C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/22/2018
ms.openlocfilehash: 0fc74ae2307ffd14f8c52515c93993a51455997a
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "80805958"
---
# <a name="location-services-on-android"></a>Android의 위치 서비스

_이 가이드에서는 Android 애플리케이션의 위치 인식 기능을 소개하고, Android Location Service API를 사용하여 사용자의 위치를 가져오는 방법과 Google Location Services API에서 사용할 수 있는 융합 위치 공급자를 보여줍니다._

Android 공급자는 셀 타워 위치, Wi-Fi, GPS 등의 다양한 위치 기술을 사용합니다. 각 위치 기술에 대한 자세한 내용이 *위치 공급자*를 통해 추상화되므로 애플리케이션에서는 사용하는 공급자에 관계없이 동일한 방법으로 위치 정보를 가져올 수 있습니다. 이 가이드에서는 사용 가능한 공급자와 디바이스가 사용되는 방식에 따라 디바이스 위치 정보를 가져오는 가장 좋은 방법을 지능적으로 결정하는 Google Play 서비스의 일부인 융합 위치 공급자를 소개합니다. Android Location Service API는 `LocationManager`를 사용하여 시스템 위치 서비스와 통신하는 방법을 보여줍니다. 이 가이드의 두 번째 파트에서는 `LocationManager`를 사용하여 Android Location Services API를 살펴봅니다.

일반적으로 경험에 비추어볼 때, 애플리케이션에서 융합 위치 공급자를 사용하고, 꼭 필요한 경우에만 이전 Android Location Service API API를 사용하는 것이 좋습니다.

## <a name="location-fundamentals"></a>위치 기본 사항

Android에서는 위치 데이터에 사용할 API로 무엇을 선택하든, 여러 개념이 동일하게 유지됩니다. 이 섹션에서는 위치 공급자 및 위치 관련 권한을 소개합니다.

### <a name="location-providers"></a>위치 공급자

사용자의 위치를 정확하게 파악하기 위해 내부적으로 여러 기술이 사용됩니다. 사용되는 하드웨어는 데이터 수집 작업을 위해 선택하는 *위치 공급자* 유형에 따라 달라집니다. Android는 다음 세 가지 위치 공급자를 사용합니다.

- **GPS 공급자** &ndash; GPS는 가장 정확한 위치를 제공하고, 가장 많은 전력을 사용하고, 야외에서 가장 잘 작동합니다. 이 공급자는 GPS과 보조 GPS([aGPS](https://en.wikipedia.org/wiki/Assisted_GPS))의 조합을 사용하며, 셀룰러 타워에서 수집한 GPS 데이터를 반환합니다.

- **네트워크 공급자** &ndash; 셀 타워에서 수집한 aGPS 데이터를 포함하여 Wi-Fi와 셀룰러 데이터의 조합을 제공합니다. GPS 공급자보다 전력 소모가 적지만, 반환하는 위치 데이터의 정확도가 일정하지 않습니다.

- **수동 공급자** &ndash; 다른 애플리케이션 또는 서비스에서 요청한 공급자를 사용하여 애플리케이션에서 위치 데이터를 생성하는 "피기백" 옵션입니다. 안정적이지는 않지만 전력 소모가 적어 위치 업데이트를 지속적으로 할 필요가 없는 애플리케이션에 이상적입니다.

위치 공급자를 항상 사용할 수 있는 것은 아닙니다. 예를 들어 애플리케이션에 GPS를 사용하고 싶지만, 설정에서 GPS가 꺼져 있거나 디바이스에 GPS가 아예 없는 경우도 있습니다. 특정 공급자를 사용할 수 없는 경우 해당 공급자를 선택하면 `null`이 반환될 수 있습니다.

### <a name="location-permissions"></a>위치 권한

위치 인식 애플리케이션은 GPS, Wi-Fi 및 셀룰러 데이터를 수신하기 위해 디바이스의 하드웨어 센서에 액세스해야 합니다. 액세스는 애플리케이션의 Android 매니페스트에서 적절한 권한을 통해 제어됩니다.
애플리케이션의 요구 사항과 선택하는 API에 따라 두 가지 권한을 사용할 수 있으며, 둘 중 하나를 허용해야 합니다.

- `ACCESS_FINE_LOCATION` &ndash; 애플리케이션에서 GPS에 액세스하도록 허용합니다.
    *GPS 공급자* 및 *수동 공급자* 옵션에 필요합니다(*수동 공급자는 다른 애플리케이션 또는 서비스에서 수집한 GPS 데이터에 액세스할 수 있는 권한이 필요함*). *네트워크 공급자*의 선택적 권한입니다.

- `ACCESS_COARSE_LOCATION` &ndash; 애플리케이션에서 셀룰러 및 Wi-Fi 위치에 액세스하도록 허용합니다. `ACCESS_FINE_LOCATION`를 설정하지 않은 경우 *네트워크 공급자*에 필요합니다.

API 버전 21(Android 5.0 롤리팝) 이상을 대상으로 하는 앱의 경우 `ACCESS_FINE_LOCATION`을 사용하도록 설정하고 GPS 하드웨어가 없는 디바이스에서 계속 실행할 수 있습니다. 앱에 GPS 하드웨어가 필요한 경우 Android 매니페스트에 `android.hardware.location.gps` `uses-feature` 요소를 명시적으로 추가해야 합니다. 자세한 내용은 Android [uses-feature](https://developer.android.com/guide/topics/manifest/uses-feature-element.html) 요소 참조를 확인하세요.

권한을 설정하려면 **Solution Pad**에서 **Properties** 폴더를 확장하고 **AndroidManifest.xml** 파일을 두 번 클릭합니다. **필요한 권한**에 권한이 나열됩니다.

[![Android 매니페스트 필요한 권한 설정의 스크린샷](location-images/location-01-xs.png)](location-images/location-01-xs.png#lightbox)

이러한 두 권한 중 하나를 설정하면 애플리케이션에서 위치 공급자에 액세스하려면 사용자의 권한이 필요하다는 것을 Android에 알리게 됩니다. API 레벨 22(Android 5.1) 이하를 실행하는 디바이스는 앱이 설치될 때마다 사용자에게 이러한 권한을 부여해 달라고 요청합니다. API 레벨 23(Android 6.0) 이상을 실행하는 디바이스에서는 앱이 위치 공급자를 요청하기 전에 런타임 권한 검사를 수행해야 합니다. 

> [!NOTE]
>참고: `ACCESS_FINE_LOCATION`을 설정하는 것은 간략한 위치 데이터와 세밀한 위치 데이터에 모두 액세스한다는 의미입니다. 두 권한을 모두 설정하면 안 되고, 앱이 작동하는 데 필요한 *최소* 권한만 설정해야 합니다.

이 코드 조각은 앱에 `ACCESS_FINE_LOCATION` 권한이 있는지 확인하는 방법의 예입니다.

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

앱은 사용자가 권한을 부여하지 않아도(또는 권한을 취소해도) 상황을 정상적으로 처리할 수 있는 시나리오를 허용해야 합니다. Xamarin.Android에서 런타임 권한 검사를 구현하는 방법에 대한 자세한 내용은 [권한 가이드](~/android/app-fundamentals/permissions.md)를 참조하세요.

## <a name="using-the-fused-location-provider"></a>융합 위치 공급자 사용

융합 위치 공급자는 Android 애플리케이션에서 디바이스의 위치 업데이트를 수신할 때 선호하는 방법입니다. 런타임 중에 위치 공급자를 효율적으로 선택하여 배터리 효율적인 방식으로 최적의 위치 정보를 제공하기 때문입니다. 예를 들어 야외에서 활동하는 사용자는 GPS를 사용하여 가장 정확한 위치 정보를 얻습니다. 이 사용자가 실내로 들어와서 GPS가 제대로(또는 전혀) 작동하지 않는 경우 융합 위치 공급자는 자동으로 실내에서 잘 작동하는 WiFi로 전환할 수 있습니다.

융합 위치 공급자 API는 지오펜싱 및 작업 모니터링을 포함하여 위치 인식 애플리케이션의 성능을 강화하는 다양한 기타 도구를 제공합니다. 이 섹션에서는 `LocationClient`를 설정하고, 공급자를 설정하고, 사용자의 위치를 가져오기 위한 기본 사항을 집중적으로 살펴보겠습니다.

융합 위치 공급자는 [Google Play 서비스](https://developer.android.com/google/play-services/index.html)의 일부입니다.
융합 위치 공급자 API가 작동하려면 애플리케이션에 Google Play 서비스 패키지를 설치하고 올바르게 구성해야 하며, 디바이스에 Google Play 서비스 APK가 설치되어 있어야 합니다.

Xamarin.Android 애플리케이션에서 융합 위치 공급자를 사용하려면 먼저 **Xamarin.GooglePlayServices.Location** 패키지를 프로젝트에 추가해야 합니다. 또한 아래에 설명된 클래스를 참조하는 원본 파일에 다음 `using` 문을 추가해야 합니다.

```csharp
using Android.Gms.Common;
using Android.Gms.Location;
```

### <a name="checking-if-google-play-services-is-installed"></a>Google Play 서비스가 설치되었는지 확인

Google Play 서비스가 설치되지 않았거나 만료된 상태에서 Xamarin.Android가 융합 위치 공급자를 사용하려고 시도하면 충돌이 발생한 후 런타임 예외가 발생합니다.  Google Play 서비스가 설치되지 않은 경우 애플리케이션에서는 위에서 설명한 Android 위치 서비스를 대신 사용해야 합니다. Google Play 서비스가 만료된 경우 앱에서 사용자에게 설치된 Google Play 서비스 버전을 업데이트하라는 메시지를 표시할 수 있습니다.

이 코드 조각은 Android 작업에서 프로그래밍 방식으로 Google Play 서비스의 설치 여부를 확인 수 있는 방법의 예입니다.

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

융합 위치 공급자와 상호 작용하려면 Xamarin.Android 애플리케이션에 `FusedLocationProviderClient` 인스턴스가 있어야 합니다. 이 클래스는 위치 업데이트를 구독하고 디바이스의 마지막으로 알려진 위치를 검색하는 데 필요한 메서드를 노출합니다.

작업의 `OnCreate` 메서드는 다음 코드 조각에서 보여드리는 것처럼 `FusedLocationProviderClient`에 대한 참조를 가져오기에 적합한 위치입니다.

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

### <a name="getting-the-last-known-location"></a>마지막으로 알려진 위치 가져오기

`FusedLocationProviderClient.GetLastLocationAsync()` 메서드는 Xamarin.Android 애플리케이션에서 최소한의 코딩 오버헤드로 디바이스의 마지막으로 알려진 위치를 신속하게 가져올 수 있는 간단한 비차단식 방법을 제공합니다.

이 코드 조각에서는 `GetLastLocationAsync` 메서드를 사용하여 디바이스의 위치를 검색하는 방법을 보여줍니다.

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

### <a name="subscribing-to-location-updates"></a>위치 업데이트 구독

또한 Xamarin.Android 애플리케이션은 다음 코드 조각에서 보여드리는 것처럼 `FusedLocationProviderClient.RequestLocationUpdatesAsync` 메서드를 사용하여 융합 위치 공급자의 위치 업데이트를 구독할 수 있습니다.

```csharp
await fusedLocationProviderClient.RequestLocationUpdatesAsync(locationRequest, locationCallback);
```

이 메서드는 다음 두 매개 변수를 사용합니다.

- **`Android.Gms.Location.LocationRequest`** &ndash; Xamarin.Android 애플리케이션은 `LocationRequest` 개체를 사용하여 융합 위치 공급자의 작동 방식을 결정하는 매개 변수를 전달합니다. `LocationRequest`는 요청 빈도 또는 정확한 위치 업데이트의 중요도와 같은 정보를 포함합니다. 예를 들어 중요한 위치 요청은 디바이스에서 GPS를 사용하게 만들고, 따라서 위치를 결정할 때 더 많은 전력을 사용하게 됩니다. 이 코드 조각은 정확도가 높은 위치의 `LocationRequest`를 만들고, 약 5분마다(하지만 요청 사이의 시간은 2분 이상) 위치 업데이트를 확인하는 방법을 보여줍니다. 융합 위치 공급자는 디바이스 위치를 확인하려고 할 때 사용할 위치 공급자에 대한 지침으로 `LocationRequest`를 사용합니다.

    ```csharp
    LocationRequest locationRequest = new LocationRequest()
                                      .SetPriority(LocationRequest.PriorityHighAccuracy)
                                      .SetInterval(60 * 1000 * 5)
                                      .SetFastestInterval(60 * 1000 * 2);
    ```

- **`Android.Gms.Location.LocationCallback`** &ndash; 위치 업데이트를 수신하려면 Xamarin.Android 애플리케이션은 `LocationProvider` 추상 클래스를 서브클래스로 만들어야 합니다. 이 클래스는 융합 위치 공급자가 위치 정보를 사용하여 앱을 업데이트하기 위해 호출할 수 있는 두 개의 메서드를 노출했습니다. 이 내용은 아래에서 자세히 설명합니다.

Xamarin.Android 애플리케이션에 위치 업데이트를 알리기 위해 융합 위치 공급자는 `LocationCallBack.OnLocationResult(LocationResult result)`를 호출합니다. `Android.Gms.Location.LocationResult` 매개 변수에는 업데이트 위치 정보가 포함됩니다.

융합 위치 공급자는 위치 데이터의 가용성 변화를 감지하면 `LocationProvider.OnLocationAvailability(LocationAvailability
locationAvailability)` 메서드를 호출합니다. `LocationAvailability.IsLocationAvailable` 속성이 `true`를 반환하면 `OnLocationResult`에서 보고한 디바이스 위치 결과가 정확하고 `LocationRequest`에서 요구하는 최신 상태인 것으로 간주할 수 있습니다. `IsLocationAvailable`이 false이면 `OnLocationResult`에서 위치 결과를 반환하지 않습니다.

이 코드 조각은 `LocationCallback` 개체의 구현 샘플입니다.

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

## <a name="using-the-android-location-service-api"></a>Android Location Service API 사용

Android Location Service는 Android에서 위치 정보를 사용하기 위한 용도의 이전 API입니다. 위치 데이터는 하드웨어 센서와 시스템 서비스를 통해 수집되고, 이후에 애플리케이션에서`LocationManager` 클래스 및 `ILocationListener`에 의해 액세스됩니다.

위치 서비스는 Google Play 서비스가 설치되지 않은 디바이스에서 실행해야 하는 애플리케이션에 가장 적합합니다.

위치 서비스는 시스템에서 관리하는 특별한 유형의 [서비스](https://developer.android.com/guide/components/services.html)입니다. 시스템 서비스는 디바이스 하드웨어와 상호 작용하며 항상 실행됩니다. 애플리케이션에서 위치 업데이트를 사용하기 위해 `LocationManager` 및 `RequestLocationUpdates` 호출을 사용하여 시스템 위치 서비스에서 위치 업데이트를 구독하겠습니다.

Android Location Service를 사용하여 사용자의 위치를 얻으려면 다음과 같은 여러 단계를 수행해야 합니다.

1. `LocationManager` 서비스의 참조를 가져옵니다.
2. `ILocationListener` 인터페이스를 구현하고 위치가 변경될 때 이벤트를 처리합니다.
3. `LocationManager`를 사용하여 지정된 공급자의 위치 업데이트를 요청합니다. 이전 단계의 `ILocationListener`는 `LocationManager`에서 콜백을 수신하는 데 사용됩니다.
4. 애플리케이션이 업데이트를 수신하는 데 더 이상 적절하지 않은 경우 위치 업데이트를 중지합니다.

### <a name="location-manager"></a>위치 관리자

`LocationManager` 클래스의 인스턴스를 사용하여 시스템 위치 서비스에 액세스할 수 있습니다. `LocationManager`는 시스템 위치 서비스와 상호 작용하고 시스템 위치 서비스에서 메서드를 호출할 수 있는 특수 클래스입니다. 애플리케이션에서 아래와 같이 `GetSystemService`를 호출하고 서비스 유형을 전달하여 `LocationManager`의 참조를 가져올 수 있습니다.

```csharp
LocationManager locationManager = (LocationManager) GetSystemService(Context.LocationService);
```

`OnCreate`는 `LocationManager`의 참조를 가져올 수 있는 좋은 장소입니다.
작업 수명 주기의 다양한 지점에서 호출할 수 있도록 `LocationManager`를 클래스 변수로 유지하는 것이 좋습니다.

### <a name="request-location-updates-from-the-locationmanager"></a>LocationManager에 위치 업데이트 요청

애플리케이션에서 `LocationManager`의 참조를 가져온 후에는 필요한 위치 정보 유형과 해당 정보의 업데이트 빈도를 `LocationManager`에 알려야 합니다. 이렇게 하려면 `LocationManager` 개체에서 `RequestLocationUpdates`를 호출하고, 업데이트의 몇 가지 조건과 위치 업데이트를 수신할 콜백을 전달합니다. 이 콜백은 `ILocationListener` 인터페이스를 구현해야 하는 형식입니다(이 가이드의 뒷부분에서 자세히 설명).

`RequestLocationUpdates` 메서드는 애플리케이션에서 위치 업데이트 수신을 시작하려 한다는 것을 시스템 위치 서비스에 알립니다. 이 메서드를 사용하면 공급자를 지정하고, 시간 및 거리 임계값을 지정하여 업데이트 빈도를 제어할 수 있습니다. 예를 들어 아래 메서드는 2000밀리초마다(단, 위치가 1미터 이상으로 변하는 경우에만) GPS 위치 공급자에게 위치 업데이트를 요청합니다.

```csharp
// For this example, this method is part of a class that implements ILocationListener, described below
locationManager.RequestLocationUpdates(LocationManager.GpsProvider, 2000, 1, this);
```

애플리케이션의 원활한 작동에 필요한 만큼만 위치 업데이트를 요청해야 합니다. 이렇게 하면 배터리 수명이 유지되고 사용자에게 보다 나은 환경이 제공됩니다.

### <a name="responding-to-updates-from-the-locationmanager"></a>LocationManager의 위치 업데이트에 응답

애플리케이션에서 `LocationManager`의 업데이트를 요청한 후에는 [`ILocationListener`](xref:Android.Locations.ILocationListener) 인터페이스를 구현하여 서비스로부터 정보를 받을 수 있습니다. 이 인터페이스는 위치 서비스 및 위치 공급자를 수신 대기하는 네 가지 메서드 `OnLocationChanged`를 제공합니다. 위치 업데이트를 요청할 때 설정된 조건에 따라 위치 변경으로 간주하기에 충분할 정도로 사용자의 위치가 변경되면 시스템에서 `OnLocationChanged`를 호출합니다. 

다음 코드는 `ILocationListener` 인터페이스의 메서드를 보여줍니다.

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

### <a name="unsubscribing-to-locationmanager-updates"></a>LocationManager 업데이트 구독 취소

시스템 리소스를 절약하려면 애플리케이션에서 가능하면 빨리 위치 업데이트를 구독 취소해야 합니다. `RemoveUpdates` 메서드는 애플리케이션에 업데이트 전송을 중지하라고 `LocationManager`에 알립니다.  예를 들어 작업이 화면에 없는 동안 애플리케이션에서 위치 업데이트가 필요 없는 경우에는 전원을 절약할 수 있도록 `OnPause` 메서드에서 `RemoveUpdates`를 호출할 수 있습니다.

```csharp
protected override void OnPause ()
{
    base.OnPause ();
    locationManager.RemoveUpdates (this);
}
```

애플리케이션이 백그라운드에 있는 동안 위치 업데이트를 가져와야 하는 경우 시스템 위치 서비스를 구독하는 사용자 지정 서비스를 만들면 됩니다. 자세한 내용은 [Android 서비스에서 백그라운딩](~/android/app-fundamentals/services/index.md) 가이드를 참조하세요.

### <a name="determining-the-best-location-provider-for-the-locationmanager"></a>LocationManager에 가장 적합한 위치 공급자 결정

위의 애플리케이션은 GPS를 위치 공급자로 설정합니다. 그러나 디바이스가 실내에 있거나 GPS 수신기가 없는 경우처럼 GPS를 사용할 수 없는 경우가 있습니다. 이 경우 공급자가 `null`을 반환합니다.

GPS를 사용할 수 없을 때 앱이 작동할 수 있도록, 애플리케이션을 시작할 때 `GetBestProvider` 메서드를 사용하여 최적의(디바이스 지원 및 사용자 지원) 위치 공급자를 요청합니다. 특정 공급자를 전달하는 대신, [`Criteria` 개체](xref:Android.Locations.Criteria)를 사용하여 정확도, 전력 등의 공급자 요구 사항을 `GetBestProvider`에게 알려줄 수 있습니다. `GetBestProvider`는 지정된 조건에 가장 적합한 공급자를 반환합니다.

다음 코드는 최적의 공급자를 가져와서 위치 업데이트를 요청할 때 사용하는 방법을 보여줍니다.

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
> 사용자가 모든 위치 공급자를 사용하지 않도록 설정하면 `GetBestProvider`가 `null`을 반환합니다. 실제 디바이스에서 이 코드가 어떻게 작동하는지 보려면 다음 스크린샷처럼 **Google Settings > Location > Mode**에서 GPS, Wi-Fi 및 셀룰러 네트워크를 사용하도록 설정해야 합니다.
>
> [![Android 휴대폰의 위치 모드 설정 화면](location-images/location-02.png)](location-images/location-02.png#lightbox)
>
> 아래 스크린샷은 `GetBestProvider`를 사용하여 실행되는 위치 애플리케이션을 보여줍니다.
>
> [![위도, 경도 및 공급자를 표시하는 GetBestProvider 앱](location-images/location-03.png)](location-images/location-03.png#lightbox)
>
> `GetBestProvider`는 공급자를 동적으로 변경하지 않습니다. 그 대신 작업 수명 주기 중에 사용 가능한 최적의 공급자를 결정합니다. 공급자가 설정된 후 공급자 상태가 변경되면 애플리케이션에서는 공급자 스위치와 관련된 모든 가능성을 처리하기 위해 `ILocationListener` 메서드 `OnProviderEnabled`, `OnProviderDisabled` 및 `OnStatusChanged` &ndash; &ndash;에 추가 코드가 필요합니다.

## <a name="summary"></a>요약

이 가이드에서는 Google Location Services API의 Android 위치 서비스와 융합 위치 공급자를 모두 사용하여 사용자의 위치를 가져오는 방법을 설명했습니다.

## <a name="related-links"></a>관련 링크

- [위치(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/location)
- [FusedLocationProvider(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fusedlocationprovider)
- [Google Play 서비스](https://developer.android.com/google/play-services/index.html)
- [조건 클래스](xref:Android.Locations.Criteria)
- [LocationManager 클래스](xref:Android.Locations.LocationManager)
- [LocationListener 클래스](xref:Android.Locations.ILocationListener)
- [LocationClient API](https://developer.android.com/reference/com/google/android/gms/location/LocationClient.html)
- [LocationListener API](https://developer.android.com/reference/com/google/android/gms/location/LocationListener.html)
- [LocationRequest API](https://developer.android.com/reference/com/google/android/gms/location/LocationRequest.html)
