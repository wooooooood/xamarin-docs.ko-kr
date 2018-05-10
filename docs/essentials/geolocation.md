---
title: Xamarin.Essentials 지리적 위치
description: 지리적 위치 클래스 장치의 현재 지리적 위치 좌표를 검색 하는 Api를 제공 합니다.
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 399dcd54d574875bcb5e491e87731b817e840e54
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-geocoding"></a>Xamarin.Essentials 지 오 코딩

![시험판 NuGet](~/media/shared/pre-release.png)

**지리적 위치** 클래스 장치의 현재 지리적 위치 좌표를 검색 하는 Api를 제공 합니다.

## <a name="getting-started"></a>시작

액세스는 **지리적 위치** 는 다음과 같은 플랫폼 특정 설치 기능이 필요 합니다.

# <a name="androidtabandroid"></a>[Android](#tab/android)

거칠게 및 미세 하 게 위치 필요한 사용 권한과 Android 프로젝트에서 구성 해야 합니다. 또한 앱이 Android 5.0 (API 수준 21) 대상으로 하거나 선언 해야 합니다는 이상 하는 경우 응용 프로그램 매니페스트 파일에는 하드웨어 기능을 사용 합니다. 다음과 같은 방법으로 추가할 수 있습니다.

열기는 **AssemblyInfo.cs** 에서 파일의 **속성** 폴더 추가:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessCoarseLocation)]
[assembly: UsesPermission(Android.Manifest.Permission.AccessFineLocation)]
[assembly: UsesFeature("android.hardware.location", Required = false)]
[assembly: UsesFeature("android.hardware.location.gps", Required = false)]
[assembly: UsesFeature("android.hardware.location.network", Required = false)]
```

또는 Android 매니페스트를 업데이트 합니다.

열기는 **AndroidManifest.xml** 아래 파일는 **속성** 폴더 내에 다음 코드를 추가 하 고는 **매니페스트** 노드.

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-feature android:name="android.hardware.location" android:required="false" />
<uses-feature android:name="android.hardware.location.gps" android:required="false" />
<uses-feature android:name="android.hardware.location.network" android:required="false" />
```

또는 Anroid 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 프로젝트의 속성을 엽니다. 아래 **Android 매니페스트** 찾을 **필요한 권한:** 영역 및 검사는 **ACCESS_COARSE_LOCATION** 및 **ACCESS_FINE_LOCATION**사용 권한. 이 자동으로 업데이트는 **AndroidManifest.xml** 파일입니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

앱 키를 포함 해야 합니다. 프로그램 **Info.plist** 장치의 위치에 액세스 하기 위해 NSLocationWhenInUseUsageDescription에 대 한 합니다.

Plist 편집기를 열고 추가 **개인정보 취급 방침-위치 때에 사용 하 여 사용 설명** 속성 및 사용자를 표시 하려면 값을 입력 합니다.

또는 수동으로 파일을 편집 하 고 다음을 추가 합니다.

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access location when open.</string>
```

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

설정 해야 합니다는 `Location` 응용 프로그램에 대 한 권한이 있습니다. Open으로이 작업을 수행할 수 있습니다는 **Package.appxmanifest** 를 선택 하 고는 **기능** 탭 및 검사 **위치**합니다.

-----

## <a name="using-geolocation"></a>지리적 위치를 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

Geoloation API 사용자에 게 필요한 경우 사용 권한 라는 메시지도 표시 됩니다.

최근에 얻을 수 [위치](xref:Xamarin.Essentials.Location) 호출 하 여 장치의 `GetLastKnownLocationAsync` 메서드. 이 다음 전체 쿼리를 수행 하 고 빠르게 주로 발생 하지만 덜 정확할 수 있습니다.

```csharp
try
{
    var location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
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

현재 장치를 쿼리하려면 [위치](xref:Xamarin.Essentials.Location) 좌표는 `GetLocationAsync` 사용할 수 있습니다. 전체를 전달 하는 것이 좋습니다. `GeolocationRequest` 및 `CancellationToken` 장치의 위치를 가져오는 데 약간의 시간이 걸릴 수 있기 때문입니다.

```csharp
try
{
    var request = new GeolocationRequest(GeolocationAccuracy.Medium);
    var location = await Geolocation.GetLocationAsync(request);

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
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

다음 표에서 플랫폼 마다 정확도 설명

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
| UWP | 30-500 |

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

## <a name="api"></a>API

- [지리적 위치 소스 코드](https://github.com/xamarin/Essentials/tree/master/Essentials/Geolocation)
- [지리적 위치 API 설명서](xref:Xamarin.Essentials.Geolocation)
