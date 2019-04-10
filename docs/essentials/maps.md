---
title: Xamarin.Essentials Map
description: Xamarin.Essentials의 Map 클래스를 사용하면 애플리케이션이 설치된 지도 애플리케이션에서 특정 위치나 placemark를 열 수 있습니다.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.custom: video
ms.openlocfilehash: c0875534d88ea5b66b3072c35b9d38894fe98934
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870029"
---
# <a name="xamarinessentials-map"></a>Xamarin.Essentials: 맵

**Map** 클래스를 사용하면 애플리케이션이 설치된 지도 애플리케이션에서 특정 위치나 placemark를 열 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-map"></a>Map 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

Map 기능은 선택적 `MapLaunchOptions`를 사용해서 열려는 `Location` 또는 `Placemark`로 `OpenAsync` 메서드를 호출하여 작동합니다.

```csharp
public class MapTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapLaunchOptions { Name = "Microsoft Building 25" };

        await Map.OpenAsync(location, options);
    }
}
```

`Placemark`로 여는 경우 다음 정보가 필요합니다.

- `CountryName`
- `AdminArea`
- `Thoroughfare`
- `Locality`

```csharp
public class MapTest
{
    public async Task NavigateToBuilding25()
    {
        var placemark = new Placemark
            {
                CountryName = "United States",
                AdminArea = "WA",
                Thoroughfare = "Microsoft Building 25",
                Locality = "Redmond"
            };
        var options =  new MapLaunchOptions { Name = "Microsoft Building 25" };

        await Map.OpenAsync(placemark, options);
    }
}
```

## <a name="extension-methods"></a>확장명 메서드

`Location` 또는 `Placemark`에 대한 참조가 이미 있는 경우 선택적 `MapLaunchOptions`와 함께 기본 제공 확장 메서드 `OpenMapAsync`를 사용할 수 있습니다.

```csharp
public class MapTest
{
    public async Task OpenPlacemarkOnMap(Placemark placemark)
    {
        await placemark.OpenMapAsync();
    }
}
```

## <a name="directions-mode"></a>길 찾기 모드

`MapLaunchOptions` 없이 `OpenMapAsync`를 호출하면 지도가 지정한 위치에서 시작됩니다. 필요에 따라, 탐색 경로가 디바이스의 현재 위치에서 계산되도록 할 수 있습니다. 이렇게 하려면 `MapLaunchOptions`에서 `NavigationMode`를 설정합니다.

```csharp
public class MapTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapLaunchOptions { NavigationMode = NavigationMode.Driving };

        await Map.OpenAsync(location, options);
    }
}
```

## <a name="platform-differences"></a>플랫폼의 차이점

# [<a name="android"></a>Android](#tab/android)

- `NavigationMode` 자전거 타기, 운전 및 걷기를 지원합니다.

# [<a name="ios"></a>iOS](#tab/ios)

- `NavigationMode` 운전, 대중교통 및 걷기를 지원합니다.

# [<a name="uwp"></a>UWP](#tab/uwp)

- `NavigationMode` 운전, 대중교통 및 걷기를 지원합니다.

--------------

## <a name="platform-implementation-specifics"></a>플랫폼 구현 관련 정보

# [<a name="android"></a>Android](#tab/android)

Android는 `geo:` URI 체계를 사용하여 장치에서 지도 애플리케이션을 시작합니다. 이 URI 체계를 지원하는 기존 앱에서 선택하라는 메시지가 사용자에게 표시될 수 있습니다.  Xamarin.Essentials는 이 체계를 지원하는 Google Maps로 테스트되었습니다.

# [<a name="ios"></a>iOS](#tab/ios)

플랫폼 특정 구현 세부 정보는 없습니다.

# [<a name="uwp"></a>UWP](#tab/uwp)

플랫폼 특정 구현 세부 정보는 없습니다.

--------------

## <a name="api"></a>API

- [맵 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Map)
- [맵 API 설명서](xref:Xamarin.Essentials.Map)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Maps-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
