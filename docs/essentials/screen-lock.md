---
title: 'Xamarin.Essentials: 화면 잠금'
description: 이 문서에 속하는 응용 프로그램이 실행 중일 때 절전 모드가에서 화면을 유지 하도록 요청할 수 있는 Xamarin.Essentials ScreenLock 클래스를 설명 합니다.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3c8110b7abc86fe1d12485579f134997718540e6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782910"
---
# <a name="xamarinessentials-screen-lock"></a>Xamarin.Essentials: 화면 잠금

![시험판 NuGet](~/media/shared/pre-release.png)

**ScreenLock** 클래스 화면 떨어지는 응용 프로그램이 실행 중일 때 절전 모드가 되지 않도록 요청할 수 있습니다.

## <a name="using-screenlock"></a>ScreenLock를 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

호출 하 여 화면 잠금 기능이 작동 하는 `RequestActive` 및 `RequestRelease` 해제에서 화면을 요청 하는 메서드.

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
- [화면 잠금 API 설명서](xref:Xamarin.Essentials.ScreenLock)
