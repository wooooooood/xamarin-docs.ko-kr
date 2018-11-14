---
title: 'Xamarin.Essentials: 지오코딩'
description: Xamarin.Essentials의 Geocoding 클래스는 위치 좌표에 장소 표시를 지오코딩하고 좌표를 장소 표시로 역 지오코딩하는 API를 제공합니다.
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3202d4ace85c68042425ca5f23641ca7a76a6c7e
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528392"
---
# <a name="xamarinessentials-geocoding"></a>Xamarin.Essentials: 지오코딩

![시험판 NuGet](~/media/shared/pre-release.png)

**Geocoding** 클래스는 위치 좌표에 장소 표시를 지오코딩하고 좌표를 장소 표시로 역 지오코딩하는 API를 제공합니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

**지오코딩** 기능에 액세스하려면 다음 플랫폼 관련 설정이 필요합니다.

# <a name="androidtabandroid"></a>[Android](#tab/android)

추가 설정이 필요하지 않습니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

추가 설정이 필요하지 않습니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

지오코딩 기능을 사용하려면 Bing Maps API 키가 필요합니다. [Bing Maps](https://www.bingmapsportal.com/) 체험 계정을 등록합니다. **내 계정 > 내 키**에서 새 키를 만들고 응용 프로그램 유형(UWP 앱의 경우 **공용 Windows 앱(UWP, 8.x 이하)** 이어야 함)에 따라 정보를 입력합니다.

**지오코딩** 메서드를 호출하기 전에 응용 프로그램 수명 초기에 API 키를 설정합니다.

```csharp
Geocoding.MapKey = "YOUR-KEY-HERE";
```

-----

## <a name="using-geocoding"></a>지오코딩 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

주소의 [위치](xref:Xamarin.Essentials.Location) 좌표를 가져옵니다.

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
    // Handle exception that may have occurred in geocoding
}
```

고도를 항상 사용할 수는 없습니다. 사용할 수 없는 경우 `Altitude` 속성은 `null`이거나 값이 0일 수 있습니다. 고도를 사용할 수 있는 경우 값은 해발 미터 단위입니다.

기존 좌표 집합의 [장소 표시](xref:Xamarin.Essentials.Placemark)를 가져옵니다.

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

## <a name="distance-between-two-locations"></a>두 위치 간 거리

[`Location`](xref:Xamarin.Essentials.Location) 및 [`LocationExtensions`](xref:Xamarin.Essentials.LocationExtensions) 클래스는 두 위치 간 거리를 계산하는 메서드를 정의합니다. 예제를 보려면 [**Xamarin.Essentials: 지리적 위치**](geolocation.md#calculate-distance) 문서를 참조하세요.

## <a name="api"></a>API

- [지오코딩 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geocoding)
- [지오코딩 API 문서](xref:Xamarin.Essentials.Geocoding)
