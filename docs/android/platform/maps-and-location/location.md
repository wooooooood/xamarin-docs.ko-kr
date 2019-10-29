---
title: Android의 위치 서비스
description: 이 가이드에서는 Android 응용 프로그램의 위치 인식을 소개 하 고, Android Location Service API를 사용 하 여 사용자의 위치를 가져오는 방법 및 Google Location Services API에서 사용할 수 있는 퓨즈 위치 공급자를 보여 줍니다.
ms.prod: xamarin
ms.assetid: 0008682B-6CEF-0C1D-3200-56ECF58F5D3C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/22/2018
ms.openlocfilehash: e027d41e98c26ef1659c27ab05df3052e19cc670
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027140"
---
# <a name="location-services-on-android"></a>Android의 위치 서비스

_이 가이드에서는 Android 응용 프로그램의 위치 인식을 소개 하 고, Android Location Service API를 사용 하 여 사용자의 위치를 가져오는 방법 및 Google Location Services API에서 사용할 수 있는 퓨즈 위치 공급자를 보여 줍니다._

Android는 셀 타워 위치, Wi-fi 및 GPS와 같은 다양 한 위치 기술에 대 한 액세스를 제공 합니다. 각 위치 기술의 세부 정보는 *위치 공급자*를 통해 추상화 되므로 응용 프로그램에서 사용 하는 공급자에 관계 없이 동일한 방식으로 위치를 가져올 수 있습니다. 이 가이드에서는 사용 가능한 공급자와 장치 사용 방법에 따라 장치 위치를 얻는 가장 좋은 방법을 지능적으로 결정 하는 Google Play 서비스의 일부인 퓨즈 위치 공급자를 소개 합니다. Android Location Service API 및 `LocationManager`를 사용 하 여 시스템 위치 서비스와 통신 하는 방법을 보여 줍니다. 이 가이드의 두 번째 부분에서는 `LocationManager`를 사용 하 여 Android Location Services API에 대해 살펴봅니다.

일반적으로 응용 프로그램은 퓨저 위치 공급자를 사용 하 여 필요한 경우에만 이전 Android 위치 서비스 API를 다시 사용 하는 것이 좋습니다.

## <a name="location-fundamentals"></a>위치 기본 사항

Android에서는 위치 데이터를 사용 하기 위해 어떤 API를 선택 하 든, 여러 개념이 동일 하 게 유지 됩니다. 이 섹션에서는 위치 공급자 및 위치 관련 사용 권한을 소개 합니다.

### <a name="location-providers"></a>위치 공급자

사용자의 위치를 정확 하 게 파악 하기 위해 여러 기술이 내부적으로 사용 됩니다. 사용 되는 하드웨어는 데이터 수집 작업을 위해 선택한 *위치 공급자* 유형에 따라 달라 집니다. Android는 세 가지 위치 공급자를 사용 합니다.

- Gps **공급자** &ndash; gps은 가장 정확한 위치를 제공 하 고, 가장 강력한 기능을 사용 하 고, 최상의 작업을 합니다. 이 공급자는 휴대용 타워에서 수집 된 GPS 데이터를 반환 하는 GPS 및 지원 GPS ([Agps](https://en.wikipedia.org/wiki/Assisted_GPS))의 조합을 사용 합니다.

- **네트워크 공급자** &ndash;는 셀 타워에 의해 수집 된 agps 데이터를 포함 하 여 WiFi 및 셀룰러 데이터의 조합을 제공 합니다. GPS 공급자 보다 더 많은 전력을 사용 하지만, 다양 한 정확도의 위치 데이터를 반환 합니다.

- **수동 공급자** 는 다른 응용 프로그램 또는 서비스에서 요청 하는 공급자를 사용 하 여 응용 프로그램에서 위치 데이터를 생성 하는 "긍정" 옵션을 &ndash; 합니다. 이는 일정 한 위치 업데이트가 작동 하지 않아도 되는 응용 프로그램에 적합 하지만 전원 저장 옵션은 안정적이 지 않습니다.

위치 공급자는 항상 사용할 수 있는 것은 아닙니다. 예를 들어 응용 프로그램에 대해 GPS를 사용 하 고 있지만 설정에서 GPS를 끌 수도 있고, 장치에 GPS가 없을 수도 있습니다. 특정 공급자를 사용할 수 없는 경우 해당 공급자를 선택 하면 `null`반환 될 수 있습니다.

### <a name="location-permissions"></a>위치 권한

위치 인식 응용 프로그램은 GPS, Wi-fi 및 셀룰러 데이터를 수신 하기 위해 장치의 하드웨어 센서에 액세스 해야 합니다. 응용 프로그램의 Android 매니페스트에서 적절 한 사용 권한을 통해 액세스를 제어 합니다.
응용 프로그램의 요구 사항에 따라 사용할 수 &ndash; 있는 두 가지 사용 권한 및 사용자의 API를 선택할 수 있습니다. 다음을 허용 하는 것이 좋습니다.

- `ACCESS_FINE_LOCATION` &ndash;를 사용 하면 응용 프로그램에서 GPS에 액세스할 수 있습니다.
    *Gps 공급자* 및 *수동 공급자* 옵션 (*수동 공급자는 다른 응용 프로그램 또는 서비스에서 수집 된 GPS 데이터에 액세스할 수 있는 권한이 필요*함)에 필요 합니다. *네트워크 공급자*에 대 한 선택적 권한입니다.

- `ACCESS_COARSE_LOCATION` &ndash;를 사용 하면 응용 프로그램에서 셀룰러 및 Wi-fi 위치에 액세스할 수 있습니다. `ACCESS_FINE_LOCATION` 설정 되지 않은 경우 *네트워크 공급자* 에 필요 합니다.

API 버전 21 (Android 5.0 롤리팝) 이상을 대상으로 하는 앱의 경우 `ACCESS_FINE_LOCATION`를 사용 하도록 설정 하 고 GPS 하드웨어가 없는 장치에서 계속 실행할 수 있습니다. 앱에 GPS 하드웨어가 필요한 경우 Android 매니페스트에 `android.hardware.location.gps` `uses-feature` 요소를 명시적으로 추가 해야 합니다. 자세한 내용은 Android [사용-기능](https://developer.android.com/guide/topics/manifest/uses-feature-element.html) 요소 참조를 참조 하세요.

권한을 설정 하려면 **Solution Pad** 에서 **속성** 폴더를 확장 하 고 **androidmanifest .xml**을 두 번 클릭 합니다. 사용 권한은 **필요한 권한**아래에 나열 됩니다.

[Android 매니페스트 필요한 사용 권한 설정의![스크린샷](location-images/location-01-xs.png)](location-images/location-01-xs.png#lightbox)

이러한 사용 권한 중 하나를 설정 하면 응용 프로그램에 위치 공급자에 액세스할 수 있는 권한이 사용자에 대 한 권한이 필요 하다는 메시지가 나타납니다. API 수준 22 (Android 5.1) 이상을 실행 하는 장치는 앱이 설치 될 때마다 사용자에 게 이러한 권한을 부여 하도록 요청 합니다. API 레벨 23 (Android 6.0) 이상을 실행 하는 장치에서 앱은 위치 공급자를 요청 하기 전에 런타임 권한 검사를 수행 해야 합니다. 

> [!NOTE]
>참고: `ACCESS_FINE_LOCATION` 설정 하면 정교 하지 않은 위치 데이터에 대 한 액세스를 의미 합니다. 앱이 작동 하는 데 필요한 *최소* 권한만 두 권한을 모두 설정할 필요는 없습니다.

이 코드 조각은 앱에 `ACCESS_FINE_LOCATION` 권한이 있는지 확인 하는 방법의 예입니다.

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

앱은 사용자가 권한을 부여 하지 않거나 사용 권한을 취소 하 고 해당 상황을 적절 하 게 처리할 수 있는 시나리오를 허용 해야 합니다. Xamarin.ios에서 런타임 권한 검사를 구현 하는 방법에 대 한 자세한 내용은 [사용 권한 가이드](~/android/app-fundamentals/permissions.md) 를 참조 하세요.

## <a name="using-the-fused-location-provider"></a>퓨즈 위치 공급자 사용

퓨즈 위치 공급자는 Android 응용 프로그램이 장치에서 위치 업데이트를 수신 하는 기본 방법입니다 .이는 런타임 중에 위치 공급자를 효율적으로 선택 하 여 배터리 효율적인 방식으로 최상의 위치 정보를 제공 하기 때문입니다. 예를 들어, 전 세계적으로 이동 하는 사용자는 GPS를 사용 하 여 가장 적합 한 위치를 가져옵니다. 그런 다음 GPS가 제대로 작동 하지 않는 경우 (예를 들어), 퓨즈 위치 공급자가 자동으로 WiFi로 전환 될 수 있으며,이는 실내에서 잘 작동 합니다.

퓨즈 위치 공급자 API는 지 오 펜싱 및 작업 모니터링을 포함 하 여 위치 인식 응용 프로그램을 위한 다양 한 기타 도구를 제공 합니다. 이 섹션에서는 `LocationClient`설정, 공급자 설정 및 사용자의 위치 가져오기에 대 한 기본 사항에 집중 하겠습니다.

퓨즈 위치 공급자는 [Google Play 서비스](https://developer.android.com/google/play-services/index.html)의 일부입니다.
응용 프로그램에서 Google Play 서비스 패키지를 설치 하 고 제대로 구성 하 여 퓨즈 위치 공급자 API가 작동 하 고 장치에 Google Play 서비스 APK가 설치 되어 있어야 합니다.

Xamarin Android 응용 프로그램에서 퓨즈 위치 공급자를 사용 하려면 먼저 프로젝트에 **xamarin.googleplayservices.base** 패키지를 추가 해야 합니다. 또한 아래에 설명 된 클래스를 참조 하는 모든 소스 파일에 다음 `using` 문을 추가 해야 합니다.

```csharp
using Android.Gms.Common;
using Android.Gms.Location;
```

### <a name="checking-if-google-play-services-is-installed"></a>Google Play 서비스 설치 되어 있는지 확인 하는 중

Xamarin.ios는 Google Play 서비스 설치 되지 않았거나 만료 된 경우에도 퓨즈 위치 공급자를 사용 하려고 시도 하면 작동 중단 됩니다. 그러면 런타임 예외가 발생 합니다.  Google Play 서비스 설치 되지 않은 경우 응용 프로그램은 위에서 설명한 Android 위치 서비스로 대체 되어야 합니다. Google Play 서비스 만료 된 경우 응용 프로그램은 설치 된 Google Play 서비스 버전을 업데이트 하 라는 메시지를 사용자에 게 표시할 수 있습니다.

이 코드 조각은 Android 작업에서 Google Play 서비스 설치 되었는지 여부를 프로그래밍 방식으로 확인할 수 있는 방법의 예입니다.

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

퓨즈 위치 공급자와 상호 작용 하려면 Xamarin.ios 응용 프로그램에 `FusedLocationProviderClient`인스턴스가 있어야 합니다. 이 클래스는 위치 업데이트를 구독 하 고 장치의 마지막으로 알려진 위치를 검색 하는 데 필요한 메서드를 제공 합니다.

활동의 `OnCreate` 메서드는 다음 코드 조각과 같이 `FusedLocationProviderClient`에 대 한 참조를 가져올 수 있는 적절 한 위치입니다.

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

`FusedLocationProviderClient.GetLastLocationAsync()` 메서드는 Xamarin Android 응용 프로그램에서 최소한의 코딩 오버 헤드로 장치를 마지막으로 알려진 위치를 신속 하 게 가져올 수 있는 간단한 차단을 제공 합니다.

이 코드 조각에서는 `GetLastLocationAsync` 메서드를 사용 하 여 장치의 위치를 검색 하는 방법을 보여 줍니다.

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

Xamarin Android 응용 프로그램은 다음 코드 조각에 나와 있는 것 처럼 `FusedLocationProviderClient.RequestLocationUpdatesAsync` 메서드를 사용 하 여 퓨즈 위치 공급자의 위치 업데이트를 구독할 수도 있습니다.

```csharp
await fusedLocationProviderClient.RequestLocationUpdatesAsync(locationRequest, locationCallback);
```

이 메서드는 두 개의 매개 변수를 사용 합니다.

- `LocationRequest` 개체를 &ndash; **`Android.Gms.Location.LocationRequest`** 는 Xamarin Android 응용 프로그램이 퓨즈 위치 공급자가 작동 하는 방식에 대 한 매개 변수를 전달 하는 방법입니다. `LocationRequest`에는 자주 발생 하는 요청 또는 정확한 위치 업데이트의 중요도와 같은 정보가 포함 됩니다. 예를 들어 중요 한 위치 요청은 장치에서 GPS를 사용 하 고, 위치를 결정할 때 더 많은 기능을 사용 합니다. 이 코드 조각은 위치 업데이트에 대해 약 5 분 마다 확인 하 고 (요청 사이에 2 분 미만), 높은 정확도가 있는 위치에 대 한 `LocationRequest`를 만드는 방법을 보여 줍니다. 퓨즈 위치 공급자는 장치 위치를 확인 하려고 할 때 사용할 위치 공급자에 대 한 지침으로 `LocationRequest`를 사용 합니다.

    ```csharp
    LocationRequest locationRequest = new LocationRequest()
                                      .SetPriority(LocationRequest.PriorityHighAccuracy)
                                      .SetInterval(60 * 1000 * 5)
                                      .SetFastestInterval(60 * 1000 * 2);
    ```

- &ndash; **`Android.Gms.Location.LocationCallback`** 위치 업데이트를 수신 하기 위해 xamarin.ios 응용 프로그램은 `LocationProvider` 추상 클래스를 하위 클래스 해야 합니다. 이 클래스는 위치 정보를 사용 하 여 응용 프로그램을 업데이트 하기 위해 퓨즈 위치 공급자에 의해 호출 될 수 있는 두 개의 메서드를 노출 했습니다. 이에 대해서는 아래에서 자세히 설명 합니다.

Xamarin Android 응용 프로그램에 위치 업데이트를 알리려면 퓨즈 위치 공급자가 `LocationCallBack.OnLocationResult(LocationResult result)`를 호출 합니다. `Android.Gms.Location.LocationResult` 매개 변수는 업데이트 위치 정보를 포함 합니다.

퓨즈 위치 공급자가 위치 데이터의 가용성에 대 한 변경 내용을 감지 하면 `LocationProvider.OnLocationAvailability(LocationAvailability
locationAvailability)` 메서드를 호출 합니다. `LocationAvailability.IsLocationAvailable` 속성이 `true`를 반환 하는 경우 `OnLocationResult`에서 보고 하는 장치 위치 결과가 정확 하 고 `LocationRequest`에서 요구 하는 대로 최신으로 간주 될 수 있습니다. `IsLocationAvailable` false 이면 `OnLocationResult`에 의해 위치 결과가 반환 되지 않습니다.

이 코드 조각은 `LocationCallback` 개체의 샘플 구현입니다.

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

Android Location Service는 Android에서 위치 정보를 사용 하기 위한 이전 API입니다. 위치 데이터는 하드웨어 센서에 의해 수집 되 고 시스템 서비스에 의해 수집 되며, `LocationManager` 클래스 및 `ILocationListener`를 사용 하 여 응용 프로그램에서 액세스 됩니다.

Location Service는 Google Play 서비스 설치 되지 않은 장치에서 실행 해야 하는 응용 프로그램에 가장 적합 합니다.

Location Service는 시스템에서 관리 하는 특수 한 유형의 [서비스](https://developer.android.com/guide/components/services.html) 입니다. 시스템 서비스는 장치 하드웨어와 상호 작용 하며 항상 실행 되 고 있습니다. 응용 프로그램에서 위치 업데이트를 탭 하면 `LocationManager` 및 `RequestLocationUpdates` 호출을 사용 하 여 시스템 위치 서비스에서 위치 업데이트를 구독 합니다.

Android Location Service를 사용 하 여 사용자의 위치를 얻으려면 몇 가지 단계를 수행 해야 합니다.

1. `LocationManager` 서비스에 대 한 참조를 가져옵니다.
2. `ILocationListener` 인터페이스를 구현 하 고 위치가 변경 될 때 이벤트를 처리 합니다.
3. `LocationManager`를 사용 하 여 지정 된 공급자에 대 한 위치 업데이트를 요청 합니다. 이전 단계의 `ILocationListener` `LocationManager`에서 콜백을 수신 하는 데 사용 됩니다.
4. 응용 프로그램에서 업데이트를 수신 하는 데 더 이상 적절 하지 않은 경우 위치 업데이트를 중지 합니다.

### <a name="location-manager"></a>위치 관리자

`LocationManager` 클래스의 인스턴스를 사용 하 여 시스템 위치 서비스에 액세스할 수 있습니다. `LocationManager`는 시스템 위치 서비스와 상호 작용 하 고 메서드를 호출할 수 있는 특수 클래스입니다. 응용 프로그램은 아래와 같이 `GetSystemService`를 호출 하 고 서비스 유형으로 전달 하 여 `LocationManager`에 대 한 참조를 가져올 수 있습니다.

```csharp
LocationManager locationManager = (LocationManager) GetSystemService(Context.LocationService);
```

`OnCreate`는 `LocationManager`에 대 한 참조를 가져올 수 있는 좋은 장소입니다.
활동 수명 주기의 다양 한 지점에서 호출할 수 있도록 `LocationManager`을 클래스 변수로 유지 하는 것이 좋습니다.

### <a name="request-location-updates-from-the-locationmanager"></a>LocationManager에서 위치 업데이트 요청

응용 프로그램에 `LocationManager`에 대 한 참조가 있으면 필요한 위치 정보 유형과 해당 정보를 업데이트할 빈도를 `LocationManager`에 알려야 합니다. 이렇게 하려면 `LocationManager` 개체에 대 한 `RequestLocationUpdates`를 호출 하 고 업데이트에 대 한 조건 및 위치 업데이트를 수신 하는 콜백을 전달 합니다. 이 콜백은 `ILocationListener` 인터페이스를 구현 해야 하는 형식입니다 (이 가이드의 뒷부분에서 자세히 설명).

`RequestLocationUpdates` 메서드는 응용 프로그램이 위치 업데이트 수신을 시작 하도록 시스템 위치 서비스에 지시 합니다. 이 메서드를 사용 하면 공급자를 지정 하 고 시간 및 거리 임계값을 지정 하 여 업데이트 빈도를 제어할 수 있습니다. 예를 들어 아래 메서드는 2000 밀리초 마다 GPS 위치 공급자의 위치 업데이트를 요청 하며 위치가 미터 이상으로 변경 되는 경우에만 해당 합니다.

```csharp
// For this example, this method is part of a class that implements ILocationListener, described below
locationManager.RequestLocationUpdates(LocationManager.GpsProvider, 2000, 1, this);
```

응용 프로그램은 응용 프로그램을 원활 하 게 수행 하는 데 필요한 경우에만 위치 업데이트를 요청 해야 합니다. 이렇게 하면 배터리 수명이 유지 되 고 사용자에 게 더 나은 환경이 생성 됩니다.

### <a name="responding-to-updates-from-the-locationmanager"></a>LocationManager의 업데이트에 대 한 응답

응용 프로그램에서 `LocationManager`업데이트를 요청 하면 [`ILocationListener`](xref:Android.Locations.ILocationListener) 인터페이스를 구현 하 여 서비스에서 정보를 받을 수 있습니다. 이 인터페이스는 위치 서비스 및 위치 공급자를 수신 하는 네 가지 메서드를 제공 `OnLocationChanged`합니다. 위치 업데이트를 요청할 때 설정 된 조건에 따라 위치가 변경 될 때까지 사용자의 위치가 변경 될 때까지 시스템에서 `OnLocationChanged`를 호출 합니다. 

다음 코드는 `ILocationListener` 인터페이스의 메서드를 보여 줍니다.

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

시스템 리소스를 절약 하기 위해 응용 프로그램은 가능한 한 빨리 위치 업데이트를 구독 취소 해야 합니다. `RemoveUpdates` 메서드는 응용 프로그램에 대 한 업데이트 전송을 중지 하도록 `LocationManager`에 지시 합니다.  예를 들어 작업이 화면에 없는 상태에서 응용 프로그램에 위치 업데이트가 필요 하지 않은 경우에는 전원을 절약할 수 있도록 `OnPause` 메서드에서 `RemoveUpdates`를 호출할 수 있습니다.

```csharp
protected override void OnPause ()
{
    base.OnPause ();
    locationManager.RemoveUpdates (this);
}
```

백그라운드에서 응용 프로그램이 위치 업데이트를 가져와야 하는 경우 시스템 위치 서비스를 구독 하는 사용자 지정 서비스를 만들려고 합니다. 자세한 내용은 [Android 서비스를 사용](~/android/app-fundamentals/services/index.md) 하는 Backgrounding 가이드를 참조 하세요.

### <a name="determining-the-best-location-provider-for-the-locationmanager"></a>LocationManager의 가장 적합 한 위치 공급자 확인

위의 응용 프로그램은 위치 공급자로 GPS를 설정 합니다. 그러나 장치가 실내에 있거나 GPS 수신기가 없는 경우와 같이 일부 경우에는 GPS를 사용 하지 못할 수 있습니다. 이 경우 결과는 공급자에 대 한 `null` 반환 됩니다.

GPS를 사용할 수 없을 때 앱이 작동 하려면 응용 프로그램 시작 시 사용 가능한 최상의 (장치 지원 및 사용자 지원) 위치 공급자를 요청 하는 `GetBestProvider` 방법을 사용 합니다. 특정 공급자를 전달 하는 대신 공급자에 대 한 요구 사항 (예: [`Criteria` 개체](xref:Android.Locations.Criteria)를 사용 하 여 정확도 및 전원)을 `GetBestProvider` 확인할 수 있습니다. `GetBestProvider`는 지정 된 조건에 가장 적합 한 공급자를 반환 합니다.

다음 코드는 사용 가능한 가장 적합 한 공급자를 가져와서 위치 업데이트를 요청할 때 사용 하는 방법을 보여 줍니다.

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
> 사용자가 모든 위치 공급자를 사용 하지 않도록 설정 하면 `GetBestProvider` `null`반환 됩니다. 실제 장치에서이 코드가 작동 하는 방식을 확인 하려면 다음 스크린샷에 표시 된 것 처럼 **Google 설정 > 위치 > 모드** 에서 GPS, wi-fi 및 셀룰러 네트워크를 사용 하도록 설정 해야 합니다.
>
> [Android 휴대폰의![설정 위치 모드 화면](location-images/location-02.png)](location-images/location-02.png#lightbox)
>
> 아래 스크린샷은 `GetBestProvider`를 사용 하 여 실행 되는 위치 응용 프로그램을 보여 줍니다.
>
> [위도, 경도 및 공급자를 표시 하는![GetBestProvider 앱](location-images/location-03.png)](location-images/location-03.png#lightbox)
>
> `GetBestProvider`는 공급자를 동적으로 변경 하지 않습니다. 대신 작업 수명 주기 중에 사용 가능한 가장 적합 한 공급자를 결정 합니다. 공급자 상태가 설정 된 후에 변경 되는 경우 공급자 스위치와 관련 된 모든 가능성을 처리 하기 위해 응용 프로그램에 `OnProviderEnabled`, `OnProviderDisabled`및 `OnStatusChanged` &ndash; &ndash; `ILocationListener` 메서드에서 추가 코드가 필요 합니다.

## <a name="summary"></a>요약

이 가이드에서는 Google Location Services API에서 Android Location Service와 퓨즈 위치 공급자를 모두 사용 하 여 사용자의 위치를 파악 하는 방법을 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [Location (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/location)
- [FusedLocationProvider (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fusedlocationprovider)
- [Google Play 서비스](https://developer.android.com/google/play-services/index.html)
- [조건 클래스](xref:Android.Locations.Criteria)
- [LocationManager 클래스](xref:Android.Locations.LocationManager)
- [LocationListener 클래스](xref:Android.Locations.ILocationListener)
- [LocationClient API](https://developer.android.com/reference/com/google/android/gms/location/LocationClient.html)
- [LocationListener API](https://developer.android.com/reference/com/google/android/gms/location/LocationListener.html)
- [LocationRequest API](https://developer.android.com/reference/com/google/android/gms/location/LocationRequest.html)
