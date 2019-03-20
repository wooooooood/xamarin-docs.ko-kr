---
title: 'Xamarin.Essentials: 흔들림 탐지'
description: Xamarin.Essentials의 Accelerometer 클래스를 사용하면 디바이스의 흔들림을 탐지할 수 있습니다.
ms.assetid: 07513D32-120F-4F12-8757-A47802A8027B
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: f1482de3fd1c3e550ac9739d0f815092f7fe753d
ms.sourcegitcommit: 64d6da88bb6ba222ab2decd2fdc8e95d377438a6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58176042"
---
# <a name="xamarinessentials-detect-shake"></a>Xamarin.Essentials: 흔들림 탐지

**[Accelerometer](accelerometer.md)** 클래스를 사용하면 3차원 공간에서 디바이스의 가속을 나타내는 디바이스의 가속도계 센서를 모니터링할 수 있습니다. 또한 사용자가 디바이스를 흔들면 이벤트를 등록할 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-detect-shake"></a>흔들림 탐지 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

디바이스의 흔들림을 탐지하려면 `Start` 및 `Stop` 메서드를 호출하여 가속도 변경 내용을 수신하고 흔들림을 탐지하는 가속도계 기능을 사용해야 합니다. 흔들림이 탐지될 때마다 `ShakeDetected ` 이벤트가 발생합니다. 샘플은 다음과 같이 사용합니다.

```csharp

public class DetectShakeTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public DetectShakeTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Accelerometer.ShakeDetected  += Accelerometer_ShakeDetected ;
    }

    void Accelerometer_ShakeDetected (object sender, EventArgs e)
    {
        // Process shake event
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

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="implementation-details"></a>구현 세부 정보

흔들림 탐지 API는 가속도계의 원시 판독값을 사용하여 가속도를 계산합니다. 최근 가속도계 이벤트의 3/4이 마지막 0.5초 동안에 발생했는지를 탐지하기 위해 간단한 큐 메커니즘을 사용합니다. 가속도는 가속도계의 X, Y 및 Z 판독값의 제곱을 추가하고 이를 특정 임계값과 비교하여 계산됩니다.

## <a name="api"></a>API

- [가속도계 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Accelerometer)
- [가속도계 API 설명서](xref:Xamarin.Essentials.Accelerometer)
