---
title: 가 속도계 Xamarin.Essentials
description: 가 속도계 클래스 가속 3 차원 공간에서 장치를 나타내는 장치의 속도계 센서를 모니터링할 수 있습니다.
ms.assetid: 97883573-F0D9-4854-AC7C-A654814401C5
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 33364b5df8edd3a5cc745d0131067bd9f3940d69
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-accelerometer"></a>가 속도계 Xamarin.Essentials

![시험판 NuGet](~/media/shared/pre-release.png)

**속도계** 클래스 사용 가속 3 차원 공간에서 장치를 나타내는 장치의 속도계 센서를 모니터링할 수 있습니다.

## <a name="using-accelerometer"></a>가 속도계를 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

가 속도계 기능이 작동 하는 호출 하 여는 `Start` 및 `Stop` 가속에 변경 내용을 수신 대기 하는 메서드. 모든 변경 내용이 통해 다시 전송 되는 `ReadingChanged` 이벤트입니다. 사용법 예제는 다음과 같습니다.

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

가 속도계 판독값 7.에 다시 보고 G gravitation 단위의 강제 같은 지구 gravitational 필드에 따라 미친 것입니다 (9.81 m/s ^2).

좌표 시스템의 기본 방향에서 휴대폰의 화면을 기준으로 정의 됩니다. 장치 화면 방향을 변경 될 때 축 교체 되지 않습니다.

X 축에는 가로 모양이 며 오른쪽에 Y 축에는 세로 및 위쪽 지점과 Z 축 화면 앞면의 외부를 가리킵니다. 이 시스템에서 좌표 화면 뒤 음수 Z 값을 갖습니다.

예를 들면 다음과 같습니다.

* 장치는 테이블에 평평 하 고 오른쪽 방향으로 왼쪽된 면에 푸시됩니다, x 가속 값이 양수입니다.

* 가속 값은 +1.00 G 장치 플랫 테이블에 있으면, 또는 (+ 9.81 m/s ^2), 장치 가속에 해당 하는 (0 m/s ^2) 중력의 빼기 (-9.81 m/s ^2) 및 7.와 같이 정규화 된

* 장치가 플랫 테이블에 있고 m/s는 가속화 된 sky에 대해 이루어지는 경우 ^ A + 9.81 장치의 가속에 해당 하는 2, 가속 값은 (+ m/s ^2) 중력의 빼기 (-9.81 m/s ^2) 및 7.에서 정규화 

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[센서 속도](xref:Xamarin.Essentials.SensorSpeed)

- **가장 빠른** – (UI 스레드에서 반환 하는 보장 되지)는 가능한 한 빠르게 센서 데이터를 가져옵니다.
- **게임** – (UI 스레드에서 반환 하는 보장 되지) 게임에 대 한 적합 한 비율입니다.
- **보통** – 급여 화면 방향 변경을에 적합 합니다.
- **Ui** – 일반 사용자 인터페이스에 대 한 적합 한 비율입니다.

## <a name="api"></a>API

- [가 속도계 소스 코드](https://github.com/xamarin/Essentials/tree/master/Essentials/Acceleromter)
- [가 속도계 API 설명서](xref:Xamarin.Essentials.Accelerometer)
