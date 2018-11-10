---
title: 'Xamarin.Essentials: 화면 잠금'
description: 이 문서에서는 응용 프로그램이 실행되는 동안 화면이 절전 모드로 전환되지 않도록 요청할 수 있는 Xamarin.Essentials의 ScreenLock 클래스를 설명합니다.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3bf8c949650cf9f039a5a516366a90e717dc944b
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675317"
---
# <a name="xamarinessentials-screen-lock"></a>Xamarin.Essentials: 화면 잠금

![시험판 NuGet](~/media/shared/pre-release.png)

**ScreenLock** 클래스는 응용 프로그램이 실행되는 동안 화면이 절전 모드로 전환되지 않도록 요청할 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-screenlock"></a>ScreenLock 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

화면 잠금 기능은 화면이 꺼지지 않도록 요청하기 위해 `RequestActive` 및 `RequestRelease` 메서드를 호출하여 작동합니다.

```csharp
public class ScreenLockTest
{
    public void ToggleScreenLock()
    {
        if (!ScreenLock.IsActive)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease();
    }
}
```

## <a name="api"></a>API

- [화면 잠금 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/ScreenLock)
- [화면 잠금 API 문서](xref:Xamarin.Essentials.ScreenLock)
