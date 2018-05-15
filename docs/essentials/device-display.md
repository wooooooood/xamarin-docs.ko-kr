---
title: Xamarin.Essentials 장치 디스플레이 정보
description: DeviceDisplay 클래스에서 실행 되는 장치 화면 메트릭 응용 프로그램에 대 한 정보를 제공 합니다.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 05701ff2bc9fceac8a0a490989e52d0327079d46
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials 장치 디스플레이 정보

![시험판 NuGet](~/media/shared/pre-release.png)

**DeviceDisplay** 클래스 응용 프로그램에서 실행 되는 장치의 화면 메트릭에 대 한 정보를 제공 합니다.

## <a name="using-devicedisplay"></a>DeviceDisplay를 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

## <a name="screen-metrics"></a>화면 메트릭

기본 장치 정보 외에 **DeviceDisplay** 클래스 장치의 화면와 방향에 대 한 정보가 포함 되어 있습니다.

```csharp
// Get Metrics
var metrics = DeviceDisplay.ScreenMetrics;

// Orientation (Landscape, Portrait, Square, Unknown)
var orientation = metrics.Orientation;

// Rotation (0, 90, 180, 270)
var rotation = metrics.Rotation;

// Width (in pixels)
var width = metrics.Width;

// Height (in pixels)
var height = metrics.Height;

// Screen density
var density = metrics.Density;
```

**DeviceDisplay** 클래스도 노출 및 이벤트를 구독할 수 있는 화면 메트릭이 변경 될 때마다 이벤트를 트리거하는:

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanaged += OnScreenMetricsChanged;
    }

    void OnScreenMetricsChanged(ScreenMetricsChanagedEventArgs  e)
    {
        // Process changes
        var metrics = e.Metrics;
    }
}
```

## <a name="api"></a>API

- [DeviceDisplay 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [DeviceDisplay API 설명서](xref:Xamarin.Essentials.DeviceDisplay)
