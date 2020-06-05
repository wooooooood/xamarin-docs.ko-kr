---
title: “Xamarin.Essentials: 나침반” description: “이 문서에서는 디바이스의 자기 북쪽 방향을 모니터링할 수 있는 Xamarin.Essentials의 Compass 클래스를 설명합니다.”
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86 author: jamesmontemagno ms.custom: video ms.author: jamont ms.date: 11/04/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-compass"></a>Xamarin.Essentials: 나침반

**Compass** 클래스를 사용하면 디바이스의 자기 북쪽 방향을 모니터링할 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-compass"></a>나침반 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

나침반 기능은 나침반에 대한 변경 내용을 수신 대기하는 `Start` 및 `Stop` 메서드를 호출하여 작동합니다. `ReadingChanged` 이벤트를 통해 변경 내용을 다시 전송합니다. 예를 들면 다음과 같습니다.

```csharp
public class CompassTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public CompassTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Compass.ReadingChanged += Compass_ReadingChanged;
    }

    void Compass_ReadingChanged(object sender, CompassChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: {data.HeadingMagneticNorth} degrees");
        // Process Heading Magnetic North
    }

    public void ToggleCompass()
    {
        try
        {
            if (Compass.IsMonitoring)
              Compass.Stop();
            else
              Compass.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Some other exception has occurred
        }
    }
}
```

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="platform-implementation-specifics"></a>플랫폼 구현 관련 정보

# <a name="android"></a>[Android](#tab/android)

Android는 나침반 방향을 검색하기 위한 API를 제공하지 않습니다. 가속도계 및 지자기 센터를 활용하여 Google에서 추천하는 자기 북쪽 방향을 계산합니다.

드문 경우지만, 센서를 보정하려면 그림 8의 움직임으로 디바이스를 이동해야 하므로 일관되지 않은 결과가 표시될 수 있습니다. 이 작업을 수행하는 가장 좋은 방법은 Google Maps를 열고, 현재 위치의 점을 탭하고, **나침반 보정**을 선택하는 것입니다.

앱에서 동시에 여러 센서를 실행하면 센서 속도가 조정될 수 있습니다.

## <a name="low-pass-filter"></a>저역 필터

Android 나침반 값이 업데이트 및 계산되는 방법으로 인해 값을 평준화해야 할 수 있습니다. ‘저역 필터’는 각도의 사인 및 코사인 값 평균을 계산하기 위해 적용할 수 있고 `bool applyLowPassFilter` 매개 변수를 허용하는 `Start` 메서드 오버로드를 사용하여 켤 수 있습니다.

```csharp
Compass.Start(SensorSpeed.UI, applyLowPassFilter: true);
```

이는 Android 플랫폼에만 적용되며 iOS 및 UWP에서는 매개 변수가 무시됩니다.  자세한 내용은 [여기](https://github.com/xamarin/Essentials/pull/354#issuecomment-405316860)에서 참조할 수 있습니다.

--------------

## <a name="api"></a>API

- [나침반 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [나침반 API 문서](xref:Xamarin.Essentials.Compass)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Compass-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
