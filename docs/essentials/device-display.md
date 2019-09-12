---
title: 'Xamarin.Essentials: 디바이스 표시 정보'
description: 이 문서에서는 애플리케이션이 실행 중인 디바이스의 화면 메트릭을 제공하는 Xamarin.Essentials의 DeviceDisplay 클래스를 설명합니다.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 77af173bc3297ac9ccdef22dccbeab054895f772
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70756900"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials: 디바이스 표시 정보

**DeviceDisplay** 클래스는 애플리케이션이 실행되고 있는 디바이스의 화면 메트릭에 관한 정보를 제공하고, 애플리케이션이 실행 중일 때 화면이 절전 상태가 되지 않도록 요청할 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-devicedisplay"></a>DeviceDisplay 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

## <a name="main-display-info"></a>기본 디스플레이 정보

기본 디바이스 정보 외에도 **DeviceDisplay** 클래스에는 디바이스의 화면 및 방향에 대한 정보가 포함되어 있습니다.

```csharp
// Get Metrics
var mainDisplayInfo = DeviceDisplay.MainDisplayInfo;

// Orientation (Landscape, Portrait, Square, Unknown)
var orientation = mainDisplayInfo.Orientation;

// Rotation (0, 90, 180, 270)
var rotation = mainDisplayInfo.Rotation;

// Width (in pixels)
var width = mainDisplayInfo.Width;

// Height (in pixels)
var height = mainDisplayInfo.Height;

// Screen density
var density = mainDisplayInfo.Density;
```

또한 **DeviceDisplay** 클래스는 화면 메트릭이 변경될 때마다 트리거되는 구독할 수 있는 이벤트를 표시합니다.

```csharp
public class DisplayInfoTest
{
    public DisplayInfoTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.MainDisplayInfoChanged += OnMainDisplayInfoChanged;
    }

    void OnMainDisplayInfoChanged(object sender, DisplayInfoChangedEventArgs  e)
    {
        // Process changes
        var displayInfo = e.DisplayInfo;
    }
}
```

**DeviceDisplay** 클래스는 디바이스의 디스플레이가 꺼지거나 잠기지 않도록 설정할 수 있는 `KeepScreenOn`이라는 `bool` 속성을 노출합니다.

```csharp
public class KeepScreenOnTest
{
    public void ToggleScreenLock()
    {
        DeviceDisplay.KeepScreenOn = !DeviceDisplay.KeepScreenOn;
    }
}
```

## <a name="platform-differences"></a>플랫폼의 차이점

# <a name="androidtabandroid"></a>[Android](#tab/android)

차이점이 없습니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

- UI 스레드에서 `DeviceDisplay`에 액세스해야 합니다. 그렇지 않으면 예외가 발생합니다. [`MainThread.BeginInvokeOnMainThread`](~/essentials/main-thread.md) 메서드를 사용하여 UI 스레드에서 해당 코드를 실행할 수 있습니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

차이점이 없습니다.

--------------

## <a name="api"></a>API

- [DeviceDisplay 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [DeviceDisplay API 문서](xref:Xamarin.Essentials.DeviceDisplay)
