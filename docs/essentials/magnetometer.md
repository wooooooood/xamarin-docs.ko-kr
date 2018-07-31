---
title: 'Xamarin.Essentials: 지자기 센터'
description: Xamarin.Essentials 지자기 센터 클래스 지구 자기장을 기준으로 장치의 방향을 나타내는 지자기 센터 센서, 장치를 모니터링할 수 있습니다.
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3827b9a57ec2667a8716f5b56bfb4631b979d43a
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353791"
---
# <a name="xamarinessentials-magnetometer"></a>Xamarin.Essentials: 지자기 센터

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **지자기 센터** 클래스를 사용 하면 지구 자기장을 기준으로 장치의 방향을 나타내는 장치의 지자기 센터 센서를 모니터링 합니다.

## <a name="using-magnetometer"></a>지자기 센터를 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

지자기 센터 기능을 호출 하 여 작동 합니다 `Start` 고 `Stop` 는 지자기 센터에 대 한 변경 내용을 수신 대기 하는 방법입니다. 변경 내용을 통해 다시 전송 되는 `ReadingChanged` 이벤트입니다. 사용법 예제는 다음과 같습니다.

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

Microteslas 모든 데이터가 반환 됩니다.

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [지자기 센터 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Magnetometer)
- [지자기 센터 API 설명서](xref:Xamarin.Essentials.Magnetometer)
