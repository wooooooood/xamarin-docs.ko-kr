---
title: 'Xamarin.Essentials: 나침반'
description: 이 문서에서 Xamarin.Essentials 장치의 자기 북부 머리글을 모니터링할 수 있는 나침반 클래스를 설명 합니다.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 63818014a9b3bdbef479055cbbcfbf8d348080fc
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080501"
---
# <a name="xamarinessentials-compass"></a>Xamarin.Essentials: 나침반

![시험판 NuGet](~/media/shared/pre-release.png)

**나침반** 클래스 장치의 자기 북부 머리글을 모니터링할 수 있습니다.

## <a name="using-compass"></a>나침반을 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

호출 하 여 나침반 기능이 작동 하는 `Start` 및 `Stop` 나침반에 변경 내용을 수신 대기 하는 메서드. 모든 변경 내용이 통해 다시 전송 되는 `ReadingChanged` 이벤트입니다. 예를 들면 다음과 같습니다.

```csharp
public class CompassTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public CompassTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Compass.ReadingChanged += Compass_ReadingChanged;
    }

    void Compass_ReadingChanged(CompassChangedEventArgs e)
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
            // Some other exception has occured
        }
    }
}
```

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[센서 속도](xref:Xamarin.Essentials.SensorSpeed)

- **가장 빠른** – (UI 스레드에서 반환 하는 보장 되지)는 가능한 한 빠르게 센서 데이터를 가져옵니다.
- **게임** – (UI 스레드에서 반환 하는 보장 되지) 게임에 대 한 적합 한 비율입니다.
- **보통** – 급여 화면 방향 변경을에 적합 합니다.
- **Ui** – 일반 사용자 인터페이스에 대 한 적합 한 비율입니다.

이벤트 처리기는 UI 스레드에서 실행 하 고 이벤트 처리기가 사용자 인터페이스 요소에 액세스 해야 하는 경우 사용할 보장 되지 않습니다는 [ `MainThread.BeginInvokeOnMainThread` ](main-thread.md) 메서드를 UI 스레드에서 해당 코드를 실행 합니다.

## <a name="platform-implementation-specifics"></a>플랫폼 구현 세부 사항

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android 나침반 머리글을 검색 하기 위한 API를 제공 하지 않습니다. 가 속도계 및 Google에서 권장 하는 자기 북부 머리글, 계산에 지자기 센터를 사용할 했습니다. 

경우에 따라에서 미정 일관성 없는 결과가 표시 보정 해야 센서 필요 하기 때문에 그림 8 동작에서 장치를 이동 하는 것입니다. 열고 Google 맵, 탭에 사용자의 위치에 있는 점을 선택 이것이 수행 하는 가장 좋은 방법은 **보정 나침반**합니다.

센서 속도 조정 될 수 있습니다 동시에 여러 센서 응용 프로그램에서 실행 된다는 점에 유의.

--------------

## <a name="api"></a>API

- [나침반 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [나침반 API 설명서](xref:Xamarin.Essentials.Compass)
