---
title: 'Xamarin.Essentials: 가속도계'
description: Xamarin.Essentials의 가속도계 클래스를 사용하면 3차원 공간에서 디바이스의 가속을 나타내는 디바이스의 가속도계 센서를 모니터링할 수 있습니다.
ms.assetid: 97883573-F0D9-4854-AC7C-A654814401C5
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: f443cc094c7d7be6ec782df27332bafe5d51dda3
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898799"
---
# <a name="xamarinessentials-accelerometer"></a>Xamarin.Essentials: 가속도계

**Accelerometer** 클래스를 사용하면 3차원 공간에서 장치의 가속을 나타내는 장치의 가속도계 센서를 모니터링할 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-accelerometer"></a>가속도계 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

가속도계 기능은 가속에 대한 변경 내용을 수신 대기하는 `Start` 및 `Stop` 메서드를 호출하여 작동합니다. `ReadingChanged` 이벤트를 통해 변경 내용을 다시 전송합니다. 샘플은 다음과 같이 사용합니다.

```csharp

public class AccelerometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public AccelerometerTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Accelerometer.ReadingChanged += Accelerometer_ReadingChanged;
    }

    void Accelerometer_ReadingChanged(object sender, AccelerometerChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: X: {data.Acceleration.X}, Y: {data.Acceleration.Y}, Z: {data.Acceleration.Z}");
        // Process Acceleration X, Y, and Z
    }

    public void ToggleAccelerometer()
    {
        try
        {
            if (Accelerometer.IsMonitoring)
              Accelerometer.Stop();
            else
              Accelerometer.Start(speed);
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

가속도계 판독은 G 단위로 다시 보고됩니다. G는 지구 중력 필드(9.81m/s^2)에서 영향을 주는 중력과 동일한 중력의 단위입니다.

좌표 시스템은 기본 방향에서 휴대폰의 화면을 기준으로 정의됩니다. 디바이스의 화면 방향이 변경되면 축이 바뀌지 않습니다.

X축은 가로이고 오른쪽을 가리키며, Y축은 세로이고 위를 가리키며, Z축은 화면 앞의 바깥쪽을 가리킵니다. 이 시스템에서 화면 뒤에 있는 좌표는 음수 Z 값입니다.

예를 들면 다음과 같습니다.

- 디바이스가 테이블 위에 눕혀 있고 왼쪽을 오른쪽 방향으로 푸시한 경우 x 가속 값은 양수입니다.

- 디바이스가 테이블 위에 눕혀 있으면 가속 값이 +1.00G 또는 (+9.81m/s^2)입니다. 이 값은 디바이스의 가속(0m/s^2) - 중력(-9.81m/s^2)에 해당하며 G 단위로 일반화됩니다.

- 디바이스가 테이블 위에 눕혀 있고 Am/s^2 가속으로 위쪽으로 푸시한 경우 가속 값은 A+9.81과 동일합니다. 이 값은 디바이스의 가속(+Am/s^2) - 중력(-9.81m/s^2)에 해당하며 G 단위로 일반화됩니다.

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [가속도계 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Accelerometer)
- [가속도계 API 설명서](xref:Xamarin.Essentials.Accelerometer)
