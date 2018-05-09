---
title: Xamarin.Essentials 자이로스코프가
description: 자이로스코프가 클래스를 사용 하면 해당 장치의 세 가지 기본 축 회전 각도 장치의 자이로스코프가 센서를 모니터링할 수 있습니다.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 586c8446df2f84070925faee2fc851657f32a2ab
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-gyroscope"></a>Xamarin.Essentials 자이로스코프가

![시험판 NuGet](~/media/shared/pre-release.png)

**자이로스코프가** 클래스 장치의 세 가지 기본 축 회전 각도 장치의 자이로스코프가 센서를 모니터링할 수 있습니다.

## <a name="using-gyroscope"></a>자이로스코프가 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

호출 하 여 자이로스코프 기능이 작동 하는 `Start` 및 `Stop` 는 자이로스코프가 변경 내용을 수신 하는 메서드를 합니다. 모든 변경 내용이 통해 다시 전송 되는 `ReadingChanged` 이벤트입니다. 사용법 예제는 다음과 같습니다.

```csharp

public class GyroscopeTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public GyroscopeTest()
    {
        // Register for reading changes.
        Gyroscope.ReadingChanged += Gyroscope_ReadingChanged;
    }

    void Gyroscope_ReadingChanged(GyroscopeChangedEventArgs e)
    {
        var data = e.Reading;
        // Process Angular Velocity X, Y, and Z
        Console.WriteLine($"Reading: X: {data.AngularVelocity.X}, Y: {data.AngularVelocity.Y}, Z: {data.AngularVelocity.Z}");
    }

    public void ToggleGyroscope()
    {
        try
        {
            if (Gyroscope.IsMonitoring)
              Gyroscope.Stop();
            else
              Gyroscope.Start(speed);
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

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[센서 속도](xref:Xamarin.Essentials.SensorSpeed)

- **가장 빠른** – (UI 스레드에서 반환 하는 보장 되지)는 가능한 한 빠르게 센서 데이터를 가져옵니다.
- **게임** – (UI 스레드에서 반환 하는 보장 되지) 게임에 대 한 적합 한 비율입니다.
- **보통** – 급여 화면 방향 변경을에 적합 합니다.
- **Ui** – 일반 사용자 인터페이스에 대 한 적합 한 비율입니다.

## <a name="api"></a>API

- [자이로스코프가 소스 코드](https://github.com/xamarin/Essentials/tree/master/Essentials/Gyroscope)
- [자이로스코프가 API 설명서](xref:Xamarin.Essentials.Gyroscope)
