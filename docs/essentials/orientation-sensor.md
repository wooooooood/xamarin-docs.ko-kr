---
title: 'Xamarin.Essentials: OrientationSensor'
description: OrientationSensor 클래스를 사용하면 3차원 공간에서 디바이스의 방향을 모니터링할 수 있습니다.
ms.assetid: F3091D93-E779-41BA-8696-23D296F2F6F5
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e61a3730e80c8fb60aee076b028ee54bed6887b3
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84802224"
---
# <a name="xamarinessentials-orientationsensor"></a>Xamarin.Essentials: OrientationSensor

**OrientationSensor** 클래스를 사용하면 3차원 공간에서 디바이스의 방향을 모니터링할 수 있습니다.

> [!NOTE]
> 이 클래스는 3D 공간에서 디바이스의 방향을 확인하기 위한 것입니다. 디바이스의 비디오 디스플레이가 세로 또는 가로 모드인지 확인해야 하는 경우 [`DeviceDisplay`](device-display.md) 클래스에서 제공되는 `ScreenMetrics` 개체의 `Orientation` 속성을 사용합니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-orientationsensor"></a>OrientationSensor 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

`OrientationSensor`는 디바이스의 방향 변경 내용을 모니터링하기 위해 `Start` 메서드를 호출하여 사용되고, `Stop` 메서드를 호출하여 사용하지 않게 됩니다. `ReadingChanged` 이벤트를 통해 변경 내용을 다시 전송합니다. 다음은 샘플 사용법입니다.

```csharp

public class OrientationSensorTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public OrientationSensorTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        OrientationSensor.ReadingChanged += OrientationSensor_ReadingChanged;
    }

    void OrientationSensor_ReadingChanged(object sender, OrientationSensorChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: X: {data.Orientation.X}, Y: {data.Orientation.Y}, Z: {data.Orientation.Z}, W: {data.Orientation.W}");
        // Process Orientation quaternion (X, Y, Z, and W)
    }

    public void ToggleOrientationSensor()
    {
        try
        {
            if (OrientationSensor.IsMonitoring)
                OrientationSensor.Stop();
            else
                OrientationSensor.Start(speed);
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

`OrientationSensor` 판독값은 두 개의 3D 좌표계를 기준으로 디바이스의 방향을 설명하는 [`Quaternion`](xref:System.Numerics.Quaternion) 형식으로 다시 보고됩니다.

디바이스(일반적으로 휴대폰 또는 태블릿)에는 다음 축을 포함하는 3D 좌표계가 있습니다.

- 양수 X 축은 세로 모드에서 디스플레이 오른쪽을 가리킵니다.
- 양수 Y 축은 세로 모드에서 디바이스의 위쪽을 가리킵니다.
- 양수 Z 축은 화면 밖을 가리킵니다.

지구의 3D 좌표계에는 다음 축이 있습니다.

- 양수 X 축은 지구 표면에 접하고 동쪽을 가리킵니다.
- 양수 Y 축도 지구 표면에 접하고 북쪽을 가리킵니다.
- 양수 Z 축은 지구 표면에 수직이고 위쪽을 가리킵니다.

`Quaternion`은 지구 좌표계를 기준으로 디바이스 좌표계의 회전을 설명합니다.

`Quaternion` 값은 축을 중심으로 하는 회전과 밀접한 관련이 있습니다. 회전의 축이 정규화된 벡터(a<sub>x</sub>, a<sub>y</sub>, a<sub>z</sub>)이고 회전 각도가 Θ인 경우 쿼터니언의 (X, Y, Z, W) 구성 요소는 다음과 같습니다.

(a<sub>x</sub>·sin(Θ/2), a<sub>y</sub>·sin(Θ/2), a<sub>z</sub>·sin(Θ/2), cos(Θ/2))

이는 오른손 좌표계이므로, 오른손 엄지손가락이 회전 축의 양수 방향을 가리킬 때 손가락 곡선은 양수 각도에 대한 회전 방향을 나타냅니다.

예:

- 디바이스가 화면이 위를 향하도록 테이블에 놓여 있고 디바이스의 위쪽(세로 모드)이 북쪽을 가리키는 경우 두 좌표계가 정렬됩니다. `Quaternion` 값은 ID 쿼터니언(0, 0, 0, 1)을 나타냅니다. 이 위치를 기준으로 모든 회전을 분석할 수 있습니다.

- 디바이스가 화면이 위를 향하도록 테이블에 놓여 있고 디바이스의 위쪽(세로 모드)이 서쪽을 가리키는 경우 `Quaternion` 값은 (0, 0, 0.707, 0.707)입니다. 디바이스가 지구의 Z 축을 중심으로 90도 회전되었습니다.

- 위쪽(세로 모드)이 하늘을 가리키고 디바이스의 뒤쪽이 북쪽을 향하도록 디바이스를 똑바로 드는 경우 디바이스가 X 축을 중심으로 90도 회전되었습니다. `Quaternion` 값은 (0.707, 0, 0, 0.707)입니다.

- 왼쪽 가장자리가 테이블 위에 오고 위쪽이 북쪽을 가리키도록 디바이스를 놓는 경우 디바이스가 Y 축을 중심으로 &ndash;90도(또는 음수 Y 축을 중심으로 90도) 회전되었습니다. `Quaternion` 값은 (0, -0.707, 0, 0.707)입니다.

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [OrientationSensor 소스 코드](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/OrientationSensor)
- [OrientationSensor API 문서](xref:Xamarin.Essentials.OrientationSensor)
