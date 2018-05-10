---
title: Xamarin.Essentials 지자기 센터
description: 지자기 센터 클래스를 사용 하면 지표의 자기 필드를 기준으로 장치의 방향을 나타내는 장치의 지자기 센터 센서를 모니터링할 수 있습니다.
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: bb9bd656c809b05c49a27f7b3dab2a64ff7b7e94
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-magnetometer"></a>Xamarin.Essentials 지자기 센터

![시험판 NuGet](~/media/shared/pre-release.png)

**지자기 센터** 클래스 지표의 자기 필드를 기준으로 장치의 방향을 나타내는 장치의 지자기 센터 센서를 모니터링할 수 있습니다.

## <a name="using-magnetometer"></a>지자기 센터를 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

지자기 센터 기능이 작동 하는 호출 하 여는 `Start` 및 `Stop` 는 지자기 센터에 변경 내용을 수신 대기 하는 메서드. 모든 변경 내용이 통해 다시 전송 되는 `ReadingChanged` 이벤트입니다. 사용법 예제는 다음과 같습니다.

```csharp

public class MagnetometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public MagnetometerTest()
    {
        // Register for reading changes.
        Magnetometer.ReadingChanged += Magnetometer_ReadingChanged;
    }

    void Magnetometerr_ReadingChanged(MagnetometerChangedEventArgs e)
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

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[센서 속도](xref:Xamarin.Essentials.SensorSpeed)

- **가장 빠른** – (UI 스레드에서 반환 하는 보장 되지)는 가능한 한 빠르게 센서 데이터를 가져옵니다.
- **게임** – (UI 스레드에서 반환 하는 보장 되지) 게임에 대 한 적합 한 비율입니다.
- **보통** – 급여 화면 방향 변경을에 적합 합니다.
- **Ui** – 일반 사용자 인터페이스에 대 한 적합 한 비율입니다.

## <a name="api"></a>API

- [지자기 센터 소스 코드](https://github.com/xamarin/Essentials/tree/master/Essentials/Magnetometer)
- [지자기 센터 API 설명서](xref:Xamarin.Essentials.Magnetometer)
