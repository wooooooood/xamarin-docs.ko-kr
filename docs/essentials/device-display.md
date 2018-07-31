---
title: 'Xamarin.Essentials: 장치 정보 표시'
description: 이 문서에서 Xamarin.Essentials 응용 프로그램이 실행 되는 장치에 대 한 화면 메트릭을 제공 DeviceDisplay 클래스를 설명 합니다.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: cb42da4c8c2d0e381a5b00f7e60da6f427d19c66
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353830"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials: 장치 정보 표시

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **DeviceDisplay** 클래스 응용 프로그램에서 실행 되는 장치의 화면 메트릭에 대 한 정보를 제공 합니다.

## <a name="using-devicedisplay"></a>DeviceDisplay를 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

## <a name="screen-metrics"></a>화면 메트릭

기본 장치 정보 외에도 합니다 **DeviceDisplay** 장치의 화면 및 방향에 대 한 정보를 포함 하는 클래스입니다.

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

합니다 **DeviceDisplay** 클래스도 모든 화면 메트릭이 변경 될 때마다 트리거되는 구독할 수 있는 이벤트를 제공 합니다.

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

## <a name="api"></a>API

- [DeviceDisplay 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [DeviceDisplay API 설명서](xref:Xamarin.Essentials.DeviceDisplay)
