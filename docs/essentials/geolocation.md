---
title: 'Xamarin.Essentials: 지리적 위치'
description: 이 문서에서는 Xamarin.Essentials 장치의 현재 지리적 위치 좌표를 검색 하는 Api를 제공 하는 지리적 위치 클래스를 설명 합니다.
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 11749107403fc99e1d49b63ee3b50ff105abaa57
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848753"
---
# <a name="xamarinessentials-geolocation"></a>Xamarin.Essentials: 지리적 위치

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **지리적 위치** 클래스는 장치의 현재 지리적 위치 좌표를 검색 하는 Api를 제공 합니다.

## <a name="getting-started"></a>시작

액세스 하는 **지리적 위치** , 다음 플랫폼별 설정을 기능은 필요:

# <a name="androidtabandroid"></a>[Android](#tab/android)

대략 및 위치를 세밀 하 게 사용 권한 필요 및 Android 프로젝트에 구성 되어야 합니다. 또한 앱이 Android 5.0 (API 수준 21) 대상으로 선언 해야 합니다는 이상 하는 경우 앱 매니페스트 파일의 하드웨어 기능을 사용 합니다. 이 다음과 같은 방법으로 추가할 수 있습니다.

엽니다는 **AssemblyInfo.cs** 파일을 **속성** 폴더 추가:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessCoarseLocation)]
[assembly: UsesPermission(Android.Manifest.Permission.AccessFineLocation)]
[assembly: UsesFeature("android.hardware.location", Required = false)]
[assembly: UsesFeature("android.hardware.location.gps", Required = false)]
[assembly: UsesFeature("android.hardware.location.network", Required = false)]
```

또는 Android 매니페스트를 업데이트 합니다.

열기는 **AndroidManifest.xml** 파일을 **속성** 폴더 내에 다음 줄을 추가 합니다 **매니페스트** 노드:

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-feature android:name="android.hardware.location" android:required="false" />
<uses-feature android:name="android.hardware.location.gps" android:required="false" />
<uses-feature android:name="android.hardware.location.network" android:required="false" />
```

또는 Android 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 프로젝트의 속성을 엽니다. 아래 **Android 매니페스트** 찾을 합니다 **필요한 권한:** 영역 및 확인 합니다 **ACCESS_COARSE_LOCATION** 및 **ACCESS_FINE_LOCATION**사용 권한. 이 자동으로 업데이트 합니다 **AndroidManifest.xml** 파일입니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

앱의 **Info.plist** 포함 해야 합니다는 `NSLocationWhenInUseUsageDescription` 장치의 위치에 액세스 하기 위해 키입니다.

Plist 편집기를 열고 추가 된 **개인 정보-위치는 경우에 사용 하 여 사용 설명** 속성 및 사용자를 표시 하려면 값을 입력 합니다.

또는 수동으로 파일을 편집 하 고 다음을 추가 합니다.

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access location when open.</string>
```

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

설정 해야 합니다는 `Location` 응용 프로그램에 대 한 권한이 있습니다. 열어 이렇게 합니다 **Package.appxmanifest** 를 선택 하 고는 **기능** 탭 및 검사 **위치**합니다.

-----

## <a name="using-geolocation"></a>지리적 위치를 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

Geoloation API를 필요한 경우 사용 권한에 대 한 사용자 라는 메시지도 표시 됩니다.

마지막으로 알려진을 가져올 수 있습니다 [위치](xref:Xamarin.Essentials.Location) 를 호출 하 여 장치를 `GetLastKnownLocationAsync` 메서드. 이 종종 다음 전체 쿼리를 수행 하 고 빠르게 이지만 덜 정확할 수 있습니다.

```csharp
try
{
    var location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to get location
}
```

고도 항상 사용할 수 없습니다. 를 사용할 수 없는 경우는 `Altitude` 속성 수 `null` 또는 0 값일 수 있습니다. 고도 사용 가능한 경우 바다 수준 위의 위에 미터는 값은입니다. 

현재 장치를 쿼리하려면 [위치](xref:Xamarin.Essentials.Location) 좌표를 `GetLocationAsync` 사용할 수 있습니다. 전체에 전달 하는 것이 좋습니다 `GeolocationRequest` 고 `CancellationToken` 장치의 위치를 가져오려면 약간의 시간이 걸릴 수 있기 때문입니다.

```csharp
try
{
    var request = new GeolocationRequest(GeolocationAccuracy.Medium);
    var location = await Geolocation.GetLocationAsync(request);

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to get location
}
```

## <a name="geolocation-accuracy"></a>지리적 위치 정확도

다음 표에서 플랫폼별 정확도를 설명합니다.

### <a name="lowest"></a>가장 낮음

| 플랫폼 | 거리 (미터) |
| --- | --- |
| Android | 500 |
| iOS | 3000 |
| UWP | 1000에서 5000 |

### <a name="low"></a>낮음

| 플랫폼 | 거리 (미터) |
| --- | --- |
| Android | 500 |
| iOS | 1000 |
| UWP | 300-3000 |

### <a name="medium-default"></a>중간 (기본값)

| 플랫폼 | 거리 (미터) |
| --- | --- |
| Android | 100-500 |
| iOS | 100 |
| UWP | 30 ~ 500 |

### <a name="high"></a>높음

| 플랫폼 | 거리 (미터) |
| --- | --- |
| Android | 0-100 |
| iOS | 10 |
| UWP | < = 10 |

### <a name="best"></a>가장

| 플랫폼 | 거리 (미터) |
| --- | --- |
| Android | 0-100 |
| iOS | ~0 |
| UWP | < = 10 |

<a name="calculate-distance" />

## <a name="distance-between-two-locations"></a>두 위치 사이의 거리

[ `Location` ](xref:Xamarin.Essentials.Location) 하 고 [ `LocationExtensions` ](xref:Xamarin.Essentials.LocationExtensions) 클래스 정의 `CalculateDistance` 두 곳의 지리적 위치 사이의 거리를 계산할 수 있도록 하는 방법입니다. 이 계산 거리를,도 또는 다른 경로 고려 하지 않습니다 및이를 화면에 따라 두 점 간 최단 단순히 라고도 합니다 _대권 거리_ 또는 흔히,는 지정 된 거리 "crow 덮어쓰는."

예를 들면 다음과 같습니다.

```csharp
Location boston = new Location(42.358056, -71.063611);
Location sanFrancisco = new Location(37.783333, -122.416667);
double miles = Location.CalculateDistance(boston, sanFrancisco, DistanceUnits.Miles);
```

`Location` 생성자에 지정 된 순서로 위도 및 경도 인수가 있습니다. 위도 값은 적도 북쪽의 및 양수 경도 값은 본초 자오선의 동쪽 양수입니다. 마지막 인수를 사용 하 여 `CalculateDistance` 마일 또는 킬로미터를 지정 합니다. 합니다 `Location` 클래스도 정의 `KilometersToMiles` 및 `MilesToKilometers` 단위 두 개 사이의 변환 메서드를 합니다.

## <a name="api"></a>API

- [지리적 위치 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geolocation)
- [지리적 위치 API 설명서](xref:Xamarin.Essentials.Geolocation)
