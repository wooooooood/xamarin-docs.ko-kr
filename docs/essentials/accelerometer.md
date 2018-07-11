---
title: Xamarin.Essentials:가 속도계
description: Xamarin.Essentials의가 속도계 클래스를 사용 하면 가속 3 차원 공간에 장치를 나타내는 장치의 속도계 센서가 모니터링할 수 있습니다.
ms.assetid: 97883573-F0D9-4854-AC7C-A654814401C5
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 15e2cb69806f281e88e226b7bcd87a20e149d508
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947311"
---
# <a name="xamarinessentials-accelerometer"></a>Xamarin.Essentials:가 속도계

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **가 속도계** 클래스를 사용 하면 장치의 속도계 센서가 가속 3 차원 공간에 장치를 나타내는 모니터링 합니다.

## <a name="using-accelerometer"></a>가 속도계를 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

가 속도계 기능을 호출 하 여 작동 합니다 `Start` 고 `Stop` 가속에 대 한 변경 내용을 수신 대기 하는 방법입니다. 변경 내용을 통해 다시 전송 되는 `ReadingChanged` 이벤트입니다. 사용법 예제는 다음과 같습니다.

```csharp

public class AccelerometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public AccelerometerTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Accelerometer.ReadingChanged += Accelerometer_ReadingChanged;
    }

    void Accelerometer_ReadingChanged(AccelerometerChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: X: {data.Acceleration.X}, Y: {data.Acceleration.Y}, Z: {data.Acceleration.Z}");
        // Process Acceleration X, Y, and Z
    }

    public void ToggleAcceleromter()
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

가 속도계 판독값 G.에 다시 보고 G는 gravitation 단위의 강제로 지구 gravitational 필드로 절대적는 같음 (9.81 m/s ^2).

좌표 시스템의 기본 방향에서 휴대폰의 화면을 기준으로 정의 됩니다. 축에는 장치의 화면 방향이 변경 되 면 교환 되지 않습니다.

X 축이 가로 및 오른쪽에 Y 축이 세로 및 지점 지점과 Z 축 화면의 앞면의 바깥쪽을 가리킵니다. 이 시스템에서 좌표는 화면 뒤에 음수 Z 값입니다.

예를 들면 다음과 같습니다.

* 장치 테이블에서 하나의 왼쪽에 있는 오른쪽 방향 푸시됩니다를 x 가속 값이 양수입니다.

* 가속 가치가 +1.00 G 장치 플랫 테이블에 있으면, 또는 (+ 9.81 m/s ^2), 해당 장치의 가속도 하는 (0 m/s ^2) 중력의 빼기 (-9.81 m/s ^2) 및 7.와 같이 정규화 된

* 장치 플랫 테이블에서 및는 m/s는 가속을 사용 하 여 무한대 방향으로 푸시 될 때 ^ + 9.81 장치의 가속에 해당 하는 2, 가속 값은 (+ m 초당 ^2) 중력의 빼기 (-9.81 m/s ^2) 및 7.에서 정규화 

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [가 속도계 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Accelerometer)
- [가 속도계 API 설명서](xref:Xamarin.Essentials.Accelerometer)
