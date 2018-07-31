---
title: 'Xamarin.Essentials: 자이로스코프가'
description: Xamarin.Essentials 자이로스코프가 클래스를 사용 하면 장치의 세 가지 기본 축 회전을 측정 하는 장치의 자이로스코프가 센서에서 모니터링할 수 있습니다.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: f1e1199ae32158889ec569eb5f7e9742f37d45d4
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353628"
---
# <a name="xamarinessentials-gyroscope"></a>Xamarin.Essentials: 자이로스코프가

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **자이로스코프가** 클래스를 사용 하면 장치의 세 가지 기본 축 회전 각도 장치의 자이로스코프가 센서를 모니터링 합니다.

## <a name="using-gyroscope"></a>자이로스코프가 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

자이로스코프가 기능을 호출 하 여 작동 합니다 `Start` 및 `Stop` 는 자이로스코프가 변경 내용을 수신 대기 하는 방법입니다. 변경 내용을 통해 다시 전송 되는 `ReadingChanged` 이벤트입니다. 사용법 예제는 다음과 같습니다.

```csharp

public class GyroscopeTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public GyroscopeTest()
    {
        // Register for reading changes.
        Gyroscope.ReadingChanged += Gyroscope_ReadingChanged;
    }

    void Gyroscope_ReadingChanged(object sender, GyroscopeChangedEventArgs e)
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

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [자이로스코프가 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Gyroscope)
- [자이로스코프가 API 설명서](xref:Xamarin.Essentials.Gyroscope)
