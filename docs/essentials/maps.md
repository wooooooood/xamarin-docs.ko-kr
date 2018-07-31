---
title: Xamarin.Essentials 맵
description: Xamarin.Essentials에 있는 맵 클래스 placemark 또는 특정 위치에 설치 된 지도 응용 프로그램을 열려면 응용 프로그램을 수 있습니다.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: 445e2da84e9a9aaf1ce4d836af11cfba963b8cbb
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353943"
---
# <a name="xamarinessentials-maps"></a>Xamarin.Essentials: 맵

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **매핑합니다** 클래스를 사용 하면 응용 프로그램을 특정 위치 또는 placemark에 설치 된 지도 응용 프로그램을 엽니다.

## <a name="using-maps"></a>맵을 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

Maps 기능을 호출 하 여 작동 합니다 `OpenAsync` 메서드를 `Location` 또는 `Placemark` 선택적으로 열려는 `MapsLaunchOptions`합니다.

```csharp
public class MapsTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapsLaunchOptions { Name = "Microsoft Building 25" };

        await Maps.OpenAsync(location, options);
    }
}
```

사용 하 여 열면는 `Placemark` 다음 정보가 필요 합니다.

* `CountryName`
* `AdminArea`
* `Thoroughfare`
* `Locality`

```csharp
public class MapsTest
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
        var options =  new MapsLaunchOptions { Name = "Microsoft Building 25" };

        await Maps.OpenAsync(placemark, options);
    }
}
```

## <a name="extension-methods"></a>확장명 메서드

에 대 한 참조를 이미 있는 경우는 `Location` 나 `Placemark` 기본 제공 확장 메서드를 사용할 수 있습니다 `OpenMapsAsync` 선택적으로 `MapsLaunchOptions`:

```csharp
public class MapsTest
{
    public async Task OpenPlacemarkOnMaps(Placemark placemark)
    {
        await placemark.OpenMapsAsync();
    }
}
```

## <a name="platform-differences"></a>플랫폼의 차이점

# <a name="androidtabandroid"></a>[Android](#tab/android)

* `MapDirectionsMode` 지원 되지 않습니다 하 고 영향을 주지 않습니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

* `MapDirectionsMode` 지도 응용 프로그램이 열릴 때의 기본 방향 모드 설정에 지원 됩니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* `MapDirectionsMode` 지원 되지 않습니다 하 고 영향을 주지 않습니다.

--------------

## <a name="platform-implementation-specifics"></a>플랫폼 구현 세부 정보

# <a name="androidtabandroid"></a>[Android](#tab/android)

사용 하 여 android는 `geo:` Uri 체계 장치의 maps 응용 프로그램을 시작 합니다. 이 사용자가이 Uri 체계를 지 원하는 기존 앱에서 선택할 것인지 수 있습니다.  이 체계를 지 원하는 Google Maps Xamarin.Essentials 테스트 됩니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

플랫폼 특정 구현 세부 정보가 없습니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

플랫폼 특정 구현 세부 정보가 없습니다.

--------------

## <a name="api"></a>API

- [맵 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Maps)
- [Maps API 설명서](xref:Xamarin.Essentials.Maps)
