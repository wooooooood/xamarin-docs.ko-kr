---
title: 'Xamarin.Essentials: 지표'
description: Xamarin.Essentials에서 Barometer 클래스를 사용하면 압력을 측정하는 디바이스의 기압계 센서를 모니터링할 수 있습니다.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9a2742a5d515c864611361e85ea0678e9c5611ba
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84802496"
---
# <a name="xamarinessentials-barometer"></a>Xamarin.Essentials: 지표

**Barometer** 클래스를 사용하면 압력을 측정하는 디바이스의 기압계 센서를 모니터링할 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-barometer"></a>Barometer 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

Barometer 기능은 기압계의 압력 판독값(헥토파스칼 단위) 변경 내용을 수신 대기하기 위해 `Start` 및 `Stop` 메서드를 호출하여 작동합니다. `ReadingChanged` 이벤트를 통해 변경 내용을 다시 전송합니다. 샘플은 다음과 같이 사용합니다.

```csharp

public class BarometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public BarometerTest()
    {
        // Register for reading changes.
        Barometer.ReadingChanged += Barometer_ReadingChanged;
    }

    void Barometer_ReadingChanged(object sender, BarometerChangedEventArgs e)
    {
        var data = e.Reading;
        // Process Pressure
        Console.WriteLine($"Reading: Pressure: {data.PressureInHectopascals} hectopascals");
    }

    public void ToggleBarometer()
    {
        try
        {
            if (Barometer.IsMonitoring)
              Barometer.Stop();
            else
              Barometer.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="platform-implementation-specifics"></a>플랫폼 구현 관련 정보

# <a name="android"></a>[Android](#tab/android)

플랫폼 특정 구현 세부 정보는 없습니다.

# <a name="ios"></a>[iOS](#tab/ios)

이 API는 [CMAltimeter](https://developer.apple.com/documentation/coremotion/cmaltimeter#//apple_ref/occ/cl/CMAltimeter)를 사용하여 압력 변경 내용을 모니터링합니다. 이 기능은 iPhone 6 이상 디바이스에 추가된 하드웨어 기능입니다. 고도계를 지원하지 않는 디바이스에서는 `FeatureNotSupportedException`이 throw됩니다.

`SensorSpeed`는 iOS에서 지원되지 않으므로 사용되지 않습니다.

# <a name="uwp"></a>[UWP](#tab/uwp)

플랫폼 특정 구현 세부 정보는 없습니다.

-----

## <a name="api"></a>API

- [Barometer 소스 코드](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Barometer)
- [Barometer API 문서](xref:Xamarin.Essentials.Barometer)
