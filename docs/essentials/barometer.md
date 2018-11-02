---
title: 'Xamarin.Essentials: Barometer'
description: Xamarin.Essentials의 Barometer 클래스를 사용하면 압력을 측정하는 장치의 기압계 센서를 모니터링할 수 있습니다.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 08/16/2018
ms.openlocfilehash: 0217b89bc3f45347acecbdb3b8cdccfeed751744
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50130304"
---
# <a name="xamarinessentials-barometer"></a>Xamarin.Essentials: Barometer

![시험판 NuGet](~/media/shared/pre-release.png)

**Barometer** 클래스를 사용하면 압력을 측정하는 장치의 기압계 센서를 모니터링할 수 있습니다.

## <a name="using-barometer"></a>Barometer 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

Barometer 기능은 기압계의 압력 판독값(킬로파스칼 단위) 변경 내용을 수신 대기하기 위해 `Start` 및 `Stop` 메서드를 호출하여 작동합니다. `ReadingChanged` 이벤트를 통해 변경 내용을 다시 전송합니다. 샘플은 다음과 같이 사용합니다.

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
        Console.WriteLine($"Reading: Pressure: {data.Pressure} kilopascals");
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

# <a name="androidtabandroid"></a>[Android](#tab/android)

플랫폼 특정 구현 세부 정보는 없습니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

이 API는 [CMAltimeter](https://developer.apple.com/documentation/coremotion/cmaltimeter#//apple_ref/occ/cl/CMAltimeter)를 사용하여 압력 변경 내용을 모니터링합니다. 이 기능은 iPhone 6 이상 장치에 추가된 하드웨어 기능입니다. 고도계를 지원하지 않는 장치에서는 `FeatureNotSupportedException`이 throw됩니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

플랫폼 특정 구현 세부 정보는 없습니다.

-----

## <a name="api"></a>API

- [Barometer 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Barometer)
- [Barometer API 문서](xref:Xamarin.Essentials.Barometer)
