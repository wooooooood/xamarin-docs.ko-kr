---
title: Xamarin.Essentials 배터리
description: 배터리 클래스에는 장치의 배터리 정보 및 변경 내용에 대 한 모니터를 확인할 수 있습니다.
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 1420e95067c060991851aff9ef99a89ed89a8358
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials 배터리

![시험판 NuGet](~/media/shared/pre-release.png)

**배터리** 클래스를 사용 하면 장치의 배터리 정보 및 변경 내용에 대 한 모니터를 확인 합니다.

## <a name="getting-started"></a>시작

액세스는 **배터리** 는 다음과 같은 플랫폼 특정 설치 기능이 필요 합니다.

# <a name="androidtabandroid"></a>[Android](#tab/android)

`Battery` 권한이 필요 하 고 Android 프로젝트에서 구성 해야 합니다. 다음과 같은 방법으로 추가할 수 있습니다.

열기는 **AssemblyInfo.cs** 에서 파일의 **속성** 폴더 추가:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Battery)]
```

또는 Android 매니페스트를 업데이트 합니다.

열기는 **AndroidManifest.xml** 아래 파일는 **속성** 폴더 내에 다음 코드를 추가 하 고는 **매니페스트** 노드.

```xml
<uses-permission android:name="android.permission.BATTERY" />
```

또는 Anroid 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 프로젝트의 속성을 엽니다. 아래 **Android 매니페스트** 찾을 **필요한 권한:** 영역 및 검사는 **배터리** 권한. 이 자동으로 업데이트는 **AndroidManifest.xml** 파일입니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

추가 설치 하지 않아도 됩니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

추가 설치 하지 않아도 됩니다.

-----

## <a name="using-battery"></a>배터리를 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

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
    case BatteryPowerSource.Ac:
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

때마다 배터리의 속성 chagne 이벤트의 트리거됩니다.

```csharp
public class BatteryTest
{
    public BatteryTest()
    {
        // Register for battery changes, be sure to unsubscribe when needed
        Battery.BatteryChanged += Battery_BatteryChanged;
    }

    void Battery_BatteryChanged(BatteryChangedEventArgs   e)
    {
        var level = e.ChargeLevel;
        var state = e.State;
        var source = e.PowerSource;
        Console.WriteLine($"Reading: Level: {level}, State: {state}, Source: {source}");
    }
}
```

## <a name="platform-differences"></a>플랫폼의 차이점

| 플랫폼 | 차이 |
| --- | --- |
| iOS | 장치 테스트 Api 사용 되어야 합니다. |
| iOS | 만 또는 반환 합니다 Ac 배터리 전원 콘센트에 꽂습니다에 대 한 합니다. |
| UWP | 만 또는 반환 합니다 Ac 배터리 전원 콘센트에 꽂습니다에 대 한 합니다. |

## <a name="api"></a>API

- [배터리 소스 코드](https://github.com/xamarin/Essentials/tree/master/Essentials/Battery)
- [배터리 API 설명서](xref:Xamarin.Essentials.Battery)
