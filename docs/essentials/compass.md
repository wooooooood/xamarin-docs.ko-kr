---
title: 'Xamarin.Essentials: 나침반'
description: 이 문서에서 Xamarin.Essentials 장치의 자기장의 북쪽 머리글을 모니터링할 수 있는 나침반 클래스를 설명 합니다.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: cf41948c55c742140896bfb48d9bb4abf25c8d68
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947415"
---
# <a name="xamarinessentials-compass"></a>Xamarin.Essentials: 나침반

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **나침반** 클래스를 사용 하면 장치의 자기장의 북쪽 머리글을 모니터링 합니다.

## <a name="using-compass"></a>Compass를 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

나침반 기능을 호출 하 여 작동 합니다 `Start` 고 `Stop` 나침반에 변경 내용을 수신 대기 하는 방법입니다. 변경 내용을 통해 다시 전송 되는 `ReadingChanged` 이벤트입니다. 예를 들면 다음과 같습니다.

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

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="platform-implementation-specifics"></a>플랫폼 구현 세부 정보

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android는 나침반을 검색 하는 API를 제공 하지 않습니다. 가 속도계 및 Google에서 권장 되지 않는 자기장의 북쪽 머리글을 계산 하려면 지자기 센터를 활용 합니다. 

드물지만에서 있을 수 있습니다 표시 일관성 없는 결과가 센서 보정 해야 해야 하기 때문에 그림 8 동작에서 장치를 전환 포함 됩니다. 이렇게 하는 가장 좋은 방법은이 방법은 Google 맵을 열고, 사용자 위치에 대 한 점을 탭 및 선택 **보정 나침반**합니다.

앱에서 여러 센서를 실행 하는 동시에 센서 속도 조정할 수에 유의 합니다.

--------------

## <a name="api"></a>API

- [나침반 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [나침반 API 설명서](xref:Xamarin.Essentials.Compass)
