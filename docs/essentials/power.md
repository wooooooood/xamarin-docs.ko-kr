---
title: 'Xamarin.Essentials: 전원 에너지 보호기 상태'
description: 전원 클래스 프로그램을 장치가 절전 모드에서 작동 하는지 확인 하려면 에너지 보호기 상태를 가져올 수 있습니다.
ms.assetid: C176D177-8B77-4A9C-9F3B-27852A8DCD5F
author: charlespetzold
ms.author: chape
ms.date: 06/27/2018
ms.openlocfilehash: 6d8ccb5be69eb1ea7ea63d3f5c373d9284089679
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080533"
---
# <a name="xamarinessentials-power-energy-saver-status"></a>Xamarin.Essentials: 전원 에너지 보호기 상태

![시험판 NuGet](~/media/shared/pre-release.png)

**전원** 클래스 장치가 절전 모드에서 실행 되 고 있는지 여부를 나타내는 장치의 에너지 보호기 상태에 대 한 정보를 제공 합니다. 장치의 에너지 보호기 상태가 on 이면 응용 프로그램에서 백그라운드 처리를 피해 야 합니다.

## <a name="background"></a>배경

배터리 전원으로 실행 하는 장치를 절전-절전 모드를 넣을 수 있습니다. 경우에 따라 장치는 전환이 모드로 자동으로 예를 들어 배터리 용량 20% 아래로 떨어질 경우. 운영 체제 배터리를 상당 부분 사용 하는 활동을 감소 시켜 에너지 절약 모드에 응답 합니다. 응용 프로그램 사용 하지 않음으로써 백그라운드 처리 또는 다른 소비가 활동 에너지 절약 모드에 있으면 유용 합니다.

Android 장치에 대 한는 **전원** 클래스는 Android 버전 5.0 (롤리팝)에 대해서만 이상 의미 있는 정보를 반환 합니다.

## <a name="using-the-power-class"></a>성능 등급을 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

정적을 사용 하 여 장치의 현재 에너지 보호기 상태를 가져오는 `Power.EnergySaverStatus` 속성:

```csharp
// Get energy saver status
var status = Power.EnergySaverStatus;
```

이 속성의 멤버를 반환 합니다.는 `EnergySaverStatus` 이거나 열거형 `On`, `Off`, 또는 `Unknown`합니다. 속성이 반환 하는 경우 `On`, 강력한 기능을 사용할 수 있습니다는 다른 작업이 나 백그라운드 처리 응용 프로그램 하지 않아야 합니다.

응용 프로그램 이벤트 처리기도 설치 해야 합니다. **전원** 에너지 보호기 상태가 변경 될 때 트리거되는 이벤트를 노출 하는 클래스:

```csharp
public class EnergySaverTest
{
    public EnergySaverTest()
    {
        // Subscribe to changes of energy-saver status
        Power.EnergySaverStatusChanaged += OnEnergySaverStatusChanaged;
    }

    private void OnEnergySaverStatusChanaged(EnergySaverStatusChanagedEventArgs e)
    {
        // Process change
        var status = e.EnergySaverStatus;
    }
}
```

에너지 보호기 상태 변경 되 면 `On`, 백그라운드 처리 응용 프로그램 중지 해야 합니다. 상태가 변경 되 면 `Unknown` 또는 `Off`, 백그라운드 처리 응용 프로그램을 다시 시작할 수 있습니다.

## <a name="api"></a>API

- [전원 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Power)
- [전원 API 설명서](xref:Xamarin.Essentials.Power)
