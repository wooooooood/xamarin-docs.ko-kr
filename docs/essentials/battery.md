---
title: 'Xamarin.Essentials: 배터리'
description: 이 문서에서는 장치의 배터리 정보를 확인하고 변경 내용을 모니터링할 수 있는 Xamarin.Essentials의 Battery 클래스를 설명합니다.
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6a14c939064538a405a1fe64061e0bb2e903fedd
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675434"
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials: 배터리

![시험판 NuGet](~/media/shared/pre-release.png)

**Battery** 클래스를 사용하여 장치의 배터리 정보를 확인하고 변경 내용을 모니터링할 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

**배터리** 기능에 액세스하려면 다음 플랫폼 관련 설정이 필요합니다.

# <a name="androidtabandroid"></a>[Android](#tab/android)

`Battery` 권한이 필요하며 Android 프로젝트에서 구성해야 합니다. 이 권한은 다음과 같은 방법으로 추가할 수 있습니다.

**속성** 폴더 아래의 **AssemblyInfo.cs** 파일을 열고 다음을 추가합니다.

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.BatteryStats)]
```

또는 Android 매니페스트를 업데이트합니다.

**속성** 폴더 아래의 **AndroidManifest.xml** 파일을 열고 **매니페스트** 노드 내부에 다음을 추가합니다.

```xml
<uses-permission android:name="android.permission.BATTERY_STATS" />
```

또는 Android 프로젝트를 마우스 오른쪽 단추로 클릭하고 프로젝트의 속성을 엽니다. **Android 매니페스트** 아래에서 **필요한 권한:** 영역을 찾아 **Battery** 권한을 확인합니다. 그러면 **AndroidManifest.xml** 파일이 자동으로 업데이트됩니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

추가 설정이 필요하지 않습니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

추가 설정이 필요하지 않습니다.

-----

## <a name="using-battery"></a>배터리 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

현재 배터리 정보를 확인합니다.

```csharp
var level = Battery.ChargeLevel; // returns 0.0 to 1.0 or -1.0 if unable to determine.

var state = Battery.State;

switch (state)
{
    case BatteryState.Charging:
        // Currently charging
        break;
    case BatteryState.Full:
        // Battery is full
        break;
    case BatteryState.Discharging:
    case BatteryState.NotCharging:
        // Currently discharging battery or not being charged
        break;
    case BatteryState.NotPresent:
        // Battery doesn't exist in device (desktop computer)
    case BatteryState.Unknown:
        // Unable to detect battery state
        break;
}

var source = Battery.PowerSource;

switch (source)
{
    case BatteryPowerSource.Battery:
        // Being powered by the battery
        break;
    case BatteryPowerSource.AC:
        // Being powered by A/C unit
        break;
    case BatteryPowerSource.Usb:
        // Being powered by USB cable
        break;
    case BatteryPowerSource.Wireless:
        // Powered via wireless charging
        break;
    case BatteryPowerSource.Unknown:
        // Unable to detect power source
        break;
}
```

배터리의 속성이 변경될 때마다 다음과 같이 이벤트가 트리거됩니다.

```csharp
public class BatteryTest
{
    public BatteryTest()
    {
        // Register for battery changes, be sure to unsubscribe when needed
        Battery.BatteryChanged += Battery_BatteryChanged;
    }

    void Battery_BatteryChanged(object sender, BatteryChangedEventArgs   e)
    {
        var level = e.ChargeLevel;
        var state = e.State;
        var source = e.PowerSource;
        Console.WriteLine($"Reading: Level: {level}, State: {state}, Source: {source}");
    }
}
```

## <a name="platform-differences"></a>플랫폼의 차이점

# <a name="androidtabandroid"></a>[Android](#tab/android)

플랫폼의 차이점이 없습니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

* API를 테스트하려면 장치를 사용해야 합니다. 
* `PowerSource`의 경우 `AC` 또는 `Battery`만 반환합니다.
* 진동을 취소할 수 없습니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* `PowerSource`의 경우 `AC` 또는 `Battery`만 반환합니다.

-----

## <a name="api"></a>API

- [배터리 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Battery)
- [배터리 API 문서](xref:Xamarin.Essentials.Battery)
