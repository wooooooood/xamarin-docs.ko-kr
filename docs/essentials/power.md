---
title: 'Xamarin.Essentials: 전원 에너지 보호기 상태'
description: 성능 등급 프로그램을 장치를 절전 모드에서 작동 하는지 확인 하려면 에너지 보호기 상태를 가져올 수 있습니다.
ms.assetid: C176D177-8B77-4A9C-9F3B-27852A8DCD5F
author: charlespetzold
ms.author: chape
ms.date: 06/27/2018
ms.openlocfilehash: 760a305280269734034a817182a8c2a07894ca2b
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353492"
---
# <a name="xamarinessentials-power-energy-saver-status"></a>Xamarin.Essentials: 전원 에너지 보호기 상태

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **전원** 클래스 장치가 절전 모드에서 실행 되 고 있는지 여부를 나타내는 장치 에너지 보호기 상태에 대 한 정보를 제공 합니다. 장치의 에너지 보호기 상태에 있으면 응용 프로그램에서 백그라운드 처리를 피해 야 합니다.

## <a name="background"></a>배경

배터리 전원으로 실행 하는 장치는 저전원 에너지 보호기 모드로 배치할 수 있습니다. 경우에 따라 장치로 전환 됩니다이 모드를 자동으로 예를 들어, 배터리 용량을 20% 아래로 떨어지는 경우. 운영 체제는 배터리를 상당 부분 사용 하는 작업을 줄여 에너지 절약 모드로에 응답 합니다. 응용 프로그램-절전 모드에 있으면 백그라운드 처리 또는 기타 소비가 작업 방지에 도움이 됩니다.

Android 장치의 경우는 **전원** Android 버전 5.0 (Lollipop)에 대해서만 이상 의미 있는 정보를 반환 하는 클래스입니다.

## <a name="using-the-power-class"></a>전원 클래스 사용

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

정적을 사용 하 여 장치의 현재 에너지 보호기 상태를 가져오는 `Power.EnergySaverStatus` 속성:

```csharp
// Get energy saver status
var status = Power.EnergySaverStatus;
```

이 속성의 멤버를 반환 합니다 `EnergySaverStatus` 열거형입니다 `On`, `Off`, 또는 `Unknown`합니다. 속성을 반환 하는 경우 `On`, 강력한 기능을 사용할 수는 다른 작업이 나 백그라운드 처리 응용 프로그램 피해 야 합니다.

응용 프로그램 이벤트 처리기를 설치 해야 합니다. **전원** 클래스 에너지 보호기 상태가 변경 될 때 트리거되는 이벤트를 노출 합니다.

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

에너지 보호기 상태가으로 변경 하는 경우 `On`, 백그라운드 처리를 수행할 응용 프로그램을 중지 합니다. 상태가으로 변경 하는 경우 `Unknown` 또는 `Off`, 응용 프로그램에서 백그라운드 처리를 다시 시작할 수 있습니다.

## <a name="api"></a>API

- [전원 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Power)
- [전원 API 설명서](xref:Xamarin.Essentials.Power)
