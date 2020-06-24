---
title: 'Xamarin.Essentials: 지자기 센터'
description: Xamarin.Essentials의 Magnetometer 클래스를 사용하면 지구 자기장을 기준으로 디바이스 방향을 나타내는 디바이스의 지자기 센터 센서를 모니터링할 수 있습니다.
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b01bc1fd9c65186952635c5f472b1ac6beb0c9bd
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84802283"
---
# <a name="xamarinessentials-magnetometer"></a>Xamarin.Essentials: 지자기 센터

**Magnetometer** 클래스를 사용하면 지구 자기장을 기준으로 디바이스 방향을 나타내는 디바이스의 지자기 센터 센서를 모니터링할 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-magnetometer"></a>지자기 센터 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

지자기 센터 기능은 지자기 센터에 대한 변경 내용을 수신 대기하는 `Start` 및 `Stop` 메서드를 호출하여 작동합니다. `ReadingChanged` 이벤트를 통해 변경 내용을 다시 전송합니다. 샘플은 다음과 같이 사용합니다.

```csharp

public class MagnetometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public MagnetometerTest()
    {
        // Register for reading changes.
        Magnetometer.ReadingChanged += Magnetometer_ReadingChanged;
    }

    void Magnetometer_ReadingChanged(object sender, MagnetometerChangedEventArgs e)
    {
        var data = e.Reading;
        // Process MagneticField X, Y, and Z
        Console.WriteLine($"Reading: X: {data.MagneticField.X}, Y: {data.MagneticField.Y}, Z: {data.MagneticField.Z}");
    }

    public void ToggleMagnetometer()
    {
        try
        {
            if (Magnetometer.IsMonitoring)
              Magnetometer.Stop();
            else
              Magnetometer.Start(speed);
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

모든 데이터는 µT(microtesla) 단위로 반환됩니다.

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [지자기 센터 소스 코드](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Magnetometer)
- [지자기 센터 API 문서](xref:Xamarin.Essentials.Magnetometer)
