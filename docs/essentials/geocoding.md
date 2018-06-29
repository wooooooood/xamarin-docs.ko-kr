---
title: 'Xamarin.Essentials: 지 오 코딩'
description: Xamarin.Essentials에 지 오 코딩 클래스는 Api를 제공 두 geocode를 위치 좌표로 placemark geocode 좌표는 placemark을 반전 합니다.
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 063adba82d96e7fcc64d7ec49a0c0133e1cef8ef
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080328"
---
# <a name="xamarinessentials-geocoding"></a>Xamarin.Essentials: 지 오 코딩

![시험판 NuGet](~/media/shared/pre-release.png)

**지 오 코딩** 클래스는 Api를 제공 geocode를 위치 좌표로 placemark geocode coordincates는 placemark을 반전 합니다.

## <a name="getting-started"></a>시작

액세스는 **지 오 코딩** 는 다음과 같은 플랫폼 특정 설치 기능이 필요 합니다.

# <a name="androidtabandroid"></a>[Android](#tab/android)

추가 설치 하지 않아도 됩니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

추가 설치 하지 않아도 됩니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Bing maps API 키를 지 오 코딩 funcationality 사용 하려면 필요 합니다. 무료 등록 [Bing Maps](https://www.bingmapsportal.com/) 계정. 아래 **내 계정 > 내 키** 새 키를 만들고 응용 프로그램 종류에 따라 정보를 채울 (해야 **공용 Windows 앱 (UWP, 8.x, 및 이전 버전)** UWP 앱 용).

호출 하기 전에 응용 프로그램의 수명에 초기에 **지 오 코딩** 메서드 API 키를 설정 합니다.

```csharp
Geocoding.MapKey = "YOUR-KEY-HERE";
```

-----

## <a name="using-geocoding"></a>지 오 코딩을 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

가져오기 [위치](xref:Xamarin.Essentials.Location) 주소의 좌표:

```csharp
try
{
    var address =  "Microsoft Building 25 Redmond WA USA";
    var locations = await Geocoding.GetLocationsAsync(address);

    var location = locations?.FirstOrDefault();
    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Handle exception that may have occured in geocoding
}
```

고도 항상 사용할 수 없습니다. 를 사용할 수 없는 경우는 `Altitude` 속성 수 `null` 값 0이 될 수도 있습니다. 고도 사용할 수 있는 바다 수준 위의 위에 미터는 값은입니다. 

가져오기 [placemarks](xref:Xamarin.Essentials.Placemark) 기존의 좌표 집합에 대 한:

```csharp
try
{
    var lat = 47.673988;
    var lon = -122.121513;

    var placemarks = await Geocoding.GetPlacemarksAsync(lat, lon);

    var placemark = placemarks?.FirstOrDefault();
    if (placemark != null)
    {
        var geocodeAddress =
            $"AdminArea:       {placemark.AdminArea}\n" +
            $"CountryCode:     {placemark.CountryCode}\n" +
            $"CountryName:     {placemark.CountryName}\n" +
            $"FeatureName:     {placemark.FeatureName}\n" +
            $"Locality:        {placemark.Locality}\n" +
            $"PostalCode:      {placemark.PostalCode}\n" +
            $"SubAdminArea:    {placemark.SubAdminArea}\n" +
            $"SubLocality:     {placemark.SubLocality}\n" +
            $"SubThoroughfare: {placemark.SubThoroughfare}\n" +
            $"Thoroughfare:    {placemark.Thoroughfare}\n";

        Console.WriteLine(geocodeAddress);
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Handle exception that may have occurred in geocoding
}
```

## <a name="distance-between-two-locations"></a>두 위치 사이의 거리

[ `Location` ](xref:Xamarin.Essentials.Location) 및 [ `LocationExtensions` ](xref:Xamarin.Essentials.LocationExtensions) 클래스 두 위치 사이의 거리를 계산 하는 메서드를 정의 합니다. 문서 참조 [ **Xamarin.Essentials: 지리적 위치** ](geolocation.md#calculate-distance) 예제입니다.

## <a name="api"></a>API

- [지 오 코딩 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geocoding)
- [지 오 코딩 API 설명서](xref:Xamarin.Essentials.Geocoding)
