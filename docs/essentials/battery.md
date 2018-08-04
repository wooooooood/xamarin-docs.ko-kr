---
title: 'Xamarin.Essentials: 배터리'
description: 이 문서에서 장치의 배터리 정보 및 변경 내용에 대 한 모니터를 확인할 수 있어 Xamarin.Essentials 배터리 클래스를 설명 합니다.
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6b87625b3305d0a9ec40593d8b3fe29eb551bbf4
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514312"
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials: 배터리

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **배터리** 클래스를 사용 하면 장치 배터리 정보 및 변경 내용에 대 한 모니터를 확인 합니다.

## <a name="getting-started"></a>시작

액세스 하는 **배터리** 플랫폼 특정 설정 기능은 필요 합니다.

# <a name="androidtabandroid"></a>[Android](#tab/android)

`Battery` 권한이 필요 하며 Android 프로젝트에 구성 되어야 합니다. 이 다음과 같은 방법으로 추가할 수 있습니다.

엽니다는 **AssemblyInfo.cs** 파일을 **속성** 폴더 추가:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.BatteryStats)]
```

또는 Android 매니페스트를 업데이트 합니다.

열기를 **AndroidManifest.xml** 파일을 **속성** 폴더 내부에 다음 줄을 추가 합니다 **매니페스트** 노드.

```xml
<uses-permission android:name="android.permission.BATTERY_STATS" />
```

또는 Android 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 프로젝트의 속성을 엽니다. 아래 **Android 매니페스트** 찾을 합니다 **필요한 권한:** 영역과 확인을 **배터리** 권한. 이 자동으로 업데이트 합니다 **AndroidManifest.xml** 파일입니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

추가 설정이 필요 없습니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

추가 설정이 필요 없습니다.

-----

## <a name="using-battery"></a>배터리를 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

현재 배터리 정보를 확인 합니다.

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

배터리의 속성이 변경 될 때마다 이벤트가 트리거될 수 있습니다.

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

플랫폼 차이점이 없습니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

* 장치를 사용 하 여 Api를 테스트할 되어야 합니다. 
* 만 반환 됩니다 `AC` 나 `Battery` 에 대 한 `PowerSource`합니다.
* 진동을 취소 하는 것이 불가능 합니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* 만 반환 됩니다 `AC` 나 `Battery` 에 대 한 `PowerSource`합니다.

-----

## <a name="api"></a>API

- [배터리 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Battery)
- [배터리 API 설명서](xref:Xamarin.Essentials.Battery)
