---
title: 'Xamarin.Essentials: 장치 디스플레이 정보'
description: 이 문서에서 응용 프로그램이 실행 되는 장치에 대 한 화면 메트릭을 제공 하는 Xamarin.Essentials DeviceDisplay 클래스를 설명 합니다.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3060d56e14fb0d3801a96ec0fe6e24c9efda4dac
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080315"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials: 장치 디스플레이 정보

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

**DeviceDisplay** 클래스도 모든 화면 메트릭이 변경 될 때마다 트리거되는 구독할 수 있는 이벤트를 노출 합니다.

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanaged += OnScreenMetricsChanged;
    }

    void OnScreenMetricsChanged(ScreenMetricsChangedEventArgs  e)
    {
        // Process changes
        var metrics = e.Metrics;
    }
}
```

## <a name="api"></a>API

- [DeviceDisplay 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [DeviceDisplay API 설명서](xref:Xamarin.Essentials.DeviceDisplay)
