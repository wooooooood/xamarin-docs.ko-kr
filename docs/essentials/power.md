---
title: 'Xamarin.Essentials: Power Energy Saver Status'
description: Power 클래스를 사용하면 프로그램이 절전 상태를 통해 장치가 절전 모드로 작동하고 있는지 확인할 수 있습니다.
ms.assetid: C176D177-8B77-4A9C-9F3B-27852A8DCD5F
author: jamesmontemagno
ms.author: jamont
ms.date: 06/27/2018
ms.openlocfilehash: 5a89dba16a93b007c5d7312221d8d33e00c7404a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110006"
---
# <a name="xamarinessentials-power-energy-saver-status"></a>Xamarin.Essentials: Power Energy Saver Status

![시험판 NuGet](~/media/shared/pre-release.png)

**Power** 클래스는 장치가 절전 모드로 실행되고 있는지 여부를 나타내는, 장치의 절전 상태 정보를 제공합니다. 장치의 절전 상태가 켜짐이면 응용 프로그램에서 후순위 처리를 피해야 합니다.

## <a name="background"></a>배경

배터리로 실행되는 장치를 절전 모드로 전환할 수 있습니다. 배터리 용량이 20% 미만으로 떨어지는 경우와 같이 장치가 자동으로 이 모드로 전환되는 경우도 있습니다. 운영 체제는 배터리를 고갈시키는 경향이 있는 활동을 줄여 절전 모드에 응답합니다. 응용 프로그램은 절전 모드가 켜져 있을 때 후순위 처리나 다른 고전력 활동을 피하여 도울 수 있습니다.

Android 장치의 경우, **Power** 클래스는 Android 버전 5.0(Lollipop) 이상에 대해서만 의미 있는 정보를 반환합니다.

## <a name="using-the-power-class"></a>Power 클래스 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

static `Power.EnergySaverStatus` 속성을 사용하여 장치의 현재 절전 상태를 확인합니다.

```csharp
// Get energy saver status
var status = Power.EnergySaverStatus;
```

이 속성은 `On`, `Off` 또는 `Unknown`인 `EnergySaverStatus` 열거형의 멤버를 반환합니다. 속성이 `On`을 반환하는 경우 응용 프로그램에서 후순위 처리나 많은 전력을 소모할 수 있는 다른 활동을 피해야 합니다.

또한 응용 프로그램에서 이벤트 처리기를 설치해야 합니다. **Power** 클래스는 절전 상태가 변경될 때 트리거되는 이벤트를 표시합니다.

```csharp
public class EnergySaverTest
{
    public EnergySaverTest()
    {
        // Subscribe to changes of energy-saver status
        Power.EnergySaverStatusChanged += OnEnergySaverStatusChanged;
    }

    private void OnEnergySaverStatusChanged(EnergySaverStatusChangedEventArgs e)
    {
        // Process change
        var status = e.EnergySaverStatus;
    }
}
```

절전 상태가 `On`으로 변경되면 응용 프로그램에서 후순위 처리 수행을 중지해야 합니다. 상태가 `Unknown` 또는 `Off`로 변경되면 응용 프로그램에서 후순위 처리를 다시 시작할 수 있습니다.

## <a name="api"></a>API

- [Power 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Power)
- [Power API 문서](xref:Xamarin.Essentials.Power)
