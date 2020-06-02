---
title: ''Xamarin.Essentials: Gyroscope'' description: ms.assetid: author: ms.author: ms.date: no-loc:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---

# <a name="xamarinessentials-gyroscope"></a>Xamarin.Essentials: 자이로스코프

**Gyroscope** 클래스를 사용하면 디바이스의 3개 기본 축을 중심으로 한 회전인 디바이스의 자이로스코프 센서를 모니터링할 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-gyroscope"></a>자이로스코프 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

자이로스코프 기능은 자이로스코프에 대한 변경 내용을 수신 대기하는 `Start` 및 `Stop` 메서드를 호출하여 작동합니다. `ReadingChanged` 이벤트를 통해 변경 내용을 다시 전송합니다(rad/s). 샘플은 다음과 같이 사용합니다.

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
        // Process Angular Velocity X, Y, and Z reported in rad/s
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

- [자이로스코프 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Gyroscope)
- [자이로스코프 API 문서](xref:Xamarin.Essentials.Gyroscope)
