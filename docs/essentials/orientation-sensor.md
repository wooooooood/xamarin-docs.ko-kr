---
title: 'Xamarin.Essentials: OrientationSensor'
description: OrientationSensor 클래스를 사용 하면 방향을 3 차원 공간에 장치를 모니터링할 수 있습니다.
ms.assetid: F3091D93-E779-41BA-8696-23D296F2F6F5
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: c01fa28e495eb3eceec62885060dce8f096c4086
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947392"
---
# <a name="xamarinessentials-orientationsensor"></a>Xamarin.Essentials: OrientationSensor

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **OrientationSensor** 클래스를 사용 하면 세 가지 차원 공간에서 장치의 방향을 모니터링 합니다.

> [!NOTE]
> 이 클래스는 3D 공간에서 장치의 방향을 결정 합니다. 세로 또는 가로 모드로 표시 되는 장치의 비디오를 확인 해야 할 경우 사용 합니다는 `Orientation` 의 속성을 `ScreenMetrics` 에서 사용 가능한 개체를 [ `DeviceDisplay` ](device-display.md) 클래스.

## <a name="using-orientationsensor"></a>OrientationSensor를 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

합니다 `OrientationSensor` 호출 하 여 사용 하도록 설정할지를 `Start` 메서드를 호출 하 여 장치 방향 및 사용 안 함으로 변경 내용을 모니터링 합니다 `Stop` 메서드. 변경 내용을 통해 다시 전송 되는 `ReadingChanged` 이벤트입니다. 사용 예는 다음과 같습니다.

```csharp

public class OrientationSensorTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public OrientationSensorTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        OrientationSensor.ReadingChanged += OrientationSensor_ReadingChanged;
    }

    void OrientationSensor_ReadingChanged(AccelerometerChangedEventArgs e)
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

`OrientationSensor` 판독값의 형태로 다시 보고 됩니다는 [ `Quaternion` ](xref:System.Numerics.Quaternion) 두 3D 좌표 시스템에 따라 장치 방향을 설명 합니다.

장치 (일반적으로 휴대폰 또는 태블릿)에 다음 축 사용 하 여 3D 좌표 시스템을 있습니다.

- X 축 요소 오른쪽의 세로 모드에 표시 되는 양수입니다.
- 양의 Y 축 위쪽 세로 모드에서 장치를 가리킵니다.
- 양의 Z 축 요소는 화면입니다.

지구 3D 좌표 시스템의 다음 축:

- 양수 X 축 지구 표면의 탄젠트 및 동부 가리킵니다.
- 양의 Y 축을 지점 북쪽 고 지구 표면의 탄젠트 이기도합니다.
- 양의 Z 축 등록 지점과 지구의 화면에 수직인 합니다.

`Quaternion` 지구 좌표계를 기준으로 장치의 좌표 시스템의 회전에 설명 합니다.

`Quaternion` 값은 매우 밀접 하는 축 기준으로 회전 합니다. 회전의 축을 정규화 된 벡터 이면 (을<sub>x</sub>,<sub>y</sub>,<sub>z</sub>), 회전 각도 Θ, 한 다음 (X, Y, Z, W) 이며 쿼터 니 언의 구성 요소는:

(한<sub>x</sub>·sin(Θ/2)는<sub>y</sub>·sin(Θ/2)는<sub>z</sub>·sin(Θ/2), cos(Θ/2))

이들은 오른쪽 좌표계를 양의 방향으로 회전 축 가리키고 오른쪽의 thumb을 사용 하 여 손가락의 곡선을 나타낼 회전 양의 각도 방향입니다.

예를 들면 다음과 같습니다.

* 북쪽을 가리키는 위쪽 (세로 모드)에서 장치를 연결 하는 화면을 사용 하 여 플랫 테이블에서 장치에 존재 하는 경우에 두 개의 좌표 시스템 정렬 됩니다. `Quaternion` 값 (0, 0, 0, 1) id 4 원수를 나타냅니다. 이 위치를 기준으로 모든 회전을 분석할 수 있습니다.

* 장치 연결, 화면 및 서쪽을 가리키는 위쪽 (세로 모드)에서 장치를 사용 하 여 플랫 테이블에 존재 하는 경우는 `Quaternion` 값 (0, 0, 0.707, 0.707)이 있습니다. 장치 지구 Z 축 중심으로 90도 회전 된 합니다.

* 장치가 적용 하는 동안 수직 위쪽 (세로 모드로), 하늘 가리킵니다 하 고 장치 뒷면 북쪽 직면 하는 경우 장치는 X 축 중심으로 90도 회전된 되었습니다. `Quaternion` (0.707, 0, 0, 0.707) 값이 있습니다.

* 테이블의 왼쪽된 가장자리 이며 맨 북쪽을 가리키는 장치 회전 된 하므로 장치가 있을 경우 &ndash;90도 Y 축을 중심으로 (또는 음의 Y 축 중심으로 90도). `Quaternion` 값 (0,-0.707, 0, 0.707)이 있습니다.

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [OrientationSensor 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/OrientationSensor)
- [OrientationSensor API 설명서](xref:Xamarin.Essentials.OrientationSensor)
