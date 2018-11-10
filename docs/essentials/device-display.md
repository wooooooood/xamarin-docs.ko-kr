---
title: 'Xamarin.Essentials: 장치 디스플레이 정보'
description: 이 문서에서는 응용 프로그램이 실행 중인 장치의 화면 메트릭을 제공하는 Xamarin.Essentials의 DeviceDisplay 클래스를 설명합니다.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: ebe97cf7fbb78bff17196110e835bd35ff76b826
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674888"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials: 장치 디스플레이 정보

![시험판 NuGet](~/media/shared/pre-release.png)

**DeviceDisplay** 클래스는 응용 프로그램이 실행 중인 장치의 화면 메트릭 정보를 제공합니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-devicedisplay"></a>DeviceDisplay 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

## <a name="screen-metrics"></a>화면 메트릭

기본 장치 정보 외에도 **DeviceDisplay** 클래스에는 장치의 화면 및 방향에 대한 정보가 포함되어 있습니다.

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

또한 **DeviceDisplay** 클래스는 화면 메트릭이 변경될 때마다 트리거되는 구독할 수 있는 이벤트를 표시합니다.

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanged += OnScreenMetricsChanged;
    }

    void OnScreenMetricsChanged(ScreenMetricsChangedEventArgs  e)
    {
        // Process changes
        var metrics = e.Metrics;
    }
}
```

## <a name="platform-differences"></a>플랫폼의 차이점

# <a name="androidtabandroid"></a>[Android](#tab/android)

차이점이 없습니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

* UI 스레드에서 `ScreenMetrics`에 액세스해야 합니다. 그렇지 않으면 예외가 발생합니다. [`MainThread.BeginInvokeOnMainThread`](~/essentials/main-thread.md) 메서드를 사용하여 UI 스레드에서 해당 코드를 실행할 수 있습니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

차이점이 없습니다.

--------------


## <a name="api"></a>API

- [DeviceDisplay 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [DeviceDisplay API 문서](xref:Xamarin.Essentials.DeviceDisplay)
