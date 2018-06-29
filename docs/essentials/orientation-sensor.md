---
title: 'Xamarin.Essentials: OrientationSensor'
description: OrientationSensor 클래스를 사용 하면 방향 3d 공간에서 장치를 모니터링할 수 있습니다.
ms.assetid: F3091D93-E779-41BA-8696-23D296F2F6F5
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: c7bbc849e5fa0b901f5b54e77d548b28bc2a72c6
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080502"
---
# <a name="xamarinessentials-orientationsensor"></a>Xamarin.Essentials: OrientationSensor

![시험판 NuGet](~/media/shared/pre-release.png)

**OrientationSensor** 클래스를 사용 하면 세 가지 차원 공간에서 장치의 방향을 모니터링 합니다.

> [!NOTE]
> 이 클래스는 3D 공간에서 장치의 방향을 결정 하는 합니다. 디스플레이 가로 또는 세로 모드에 있으면 사용 하 여 장치 비디오의 경우를 결정 하는 경우는 `Orientation` 속성은 `ScreenMetrics` 에서 사용할 수 있는 개체는 [ `DeviceDisplay` ](device-display.md) 클래스입니다.

## <a name="using-orientationsensor"></a>OrientationSensor를 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

`OrientationSensor` 호출 하 여 사용 하도록 설정 된 `Start` 메서드를 호출 하 여 장치의 방향 및 사용 안 함에 변경 내용을 모니터링는 `Stop` 메서드. 모든 변경 내용이 통해 다시 전송 되는 `ReadingChanged` 이벤트입니다. 사용법 예제는 다음과 같습니다.

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

`OrientationSensor` 판독값의 형태로 다시 보고 되는 [ `Quaternion` ](xref:System.Numerics.Quaternion) 두 3D 좌표계에 따라 장치 방향을 설명 하는:

장치 (일반적으로 휴대폰 또는 태블릿) 다음 축이 있는 3 차원 좌표계에 있습니다.

- X 축 요소가 오른쪽의 세로 모드에 표시 되는 양수입니다.
- 양의 Y 축이 세로 모드의 장치가 맨을 가리킵니다.
- 화면에서 양수 Z 축이 가리킵니다.

지구 좌표계 3D 다음 축에 있습니다.

- 양수 X 축에는 지구 표면의 탄젠트 이며 동쪽 가리킵니다.
- 양의 Y 축 지구와 북쪽 포인트의 화면으로 탄젠트 이기도합니다.
- 양의 Z 축이 수직를 점과 지구 표면으로.

`Quaternion` 지구 좌표계를 기준으로 장치의 좌표 시스템의 회전에 설명 합니다.

A `Quaternion` 값은 매우 밀접 하 축 중심으로 회전 합니다. 회전 축이 정규화 된 벡터 (한<sub>x</sub>,<sub>y</sub>,<sub>z</sub>), 회전 각도 Θ, 한 다음 (X, Y, Z, W) 및 쿼터 니 언의 구성 요소는:

(한<sub>x</sub>·sin(Θ/2)는<sub>y</sub>·sin(Θ/2)는<sub>z</sub>·sin(Θ/2), cos(Θ/2))

오른쪽 좌표계는 되므로 양의 방향으로 회전 축 가리키고 오른쪽의 엄지도 손가락 곡선의 회전 양의 각도 방향을 표시 합니다.

예를 들면 다음과 같습니다.

* 북쪽,을 가리키는 (세로 모드)에서 장치 맨 향하도록 화면으로 장치에 있는 플랫 테이블에 두 개의 좌표 시스템을 정렬 됩니다. `Quaternion` 값은 id 쿼터 니 언이 (0, 0, 0, 1)를 나타냅니다. 이 위치를 기준으로 모든 회전을 분석할 수 있습니다.

* 장치를 향하도록 화면 및 서쪽을 가리키는 (세로 모드)에서 장치 맨 플랫 테이블에 있으면는 `Quaternion` 값 (0, 0, 0.707, 0.707)이 있습니다. 장치는 지구 Z 축 중심 90도 회전 된 합니다.

* 장치 (세로 모드)에서 top sky, 가리킵니다 하 고 장치 후면 북쪽 향하도록 있도록 수직 커서가, 장치는 X 축을 90도 회전된 되었습니다. `Quaternion` (0.707, 0, 0, 0.707) 값이 있습니다.

* 왼쪽된 가장자리는 한 테이블에 있고 위쪽 북쪽, 가리키는 장치 회전 된 되므로 장치가 배치 되 면 &ndash;90도 Y 축을 중심 (또는 음수 Y 축 중심으로 90도). `Quaternion` 값 (0,-0.707, 0, 0.707)이 있습니다.

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[센서 속도](xref:Xamarin.Essentials.SensorSpeed)

- **가장 빠른** – (UI 스레드에서 반환 하는 보장 되지)는 가능한 한 빠르게 센서 데이터를 가져옵니다.
- **게임** – (UI 스레드에서 반환 하는 보장 되지) 게임에 대 한 적합 한 비율입니다.
- **보통** – 급여 화면 방향 변경을에 적합 합니다.
- **Ui** – 일반 사용자 인터페이스에 대 한 적합 한 비율입니다.

이벤트 처리기는 UI 스레드에서 실행 하 고 이벤트 처리기가 사용자 인터페이스 요소에 액세스 해야 하는 경우 사용할 보장 되지 않습니다는 [ `MainThread.BeginInvokeOnMainThread` ](main-thread.md) 메서드를 UI 스레드에서 해당 코드를 실행 합니다.

## <a name="api"></a>API

- [OrientationSensor 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/OrientationSensor)
- [OrientationSensor API 설명서](xref:Xamarin.Essentials.OrientationSensor)
