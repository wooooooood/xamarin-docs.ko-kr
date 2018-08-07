---
title: Xamarin.Essentials 시작 관리자
description: Xamarin.Essentials Launcher 클래스 응용 프로그램을을 시스템에서 URI를 열 수 있습니다.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: 252bb873c1494265aafb2285057490ca29ce7419
ms.sourcegitcommit: bf51592be39b2ae3d63d029be1d7745ee63b0ce1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39573635"
---
# <a name="xamarinessentials-launcher"></a>Xamarin.Essentials: 시작 관리자

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **Launcher** 클래스를 사용 하면 응용 프로그램을 시스템에서 URI를 엽니다. 이 심층 다른 응용 프로그램의 사용자 지정 URI 체계에 연결 하는 경우 자주 사용 됩니다. 웹 사이트에 브라우저를 열고 하려는 경우 참조 해야 합니다 **[브라우저](open-browser.md)** API.

## <a name="using-launcher"></a>시작 관리자를 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

Launcher 기능 호출을 사용 합니다 `OpenAsync` 메서드와 전달을 `string` 또는 `Uri` 를 엽니다. 필요에 따라는 `CanOpenAsync` 메서드를 사용 하 여를 URI 스키마를 장치에 응용 프로그램에서 처리할 수 하는 경우 확인할 수 있습니다.

```csharp
public class LauncherTest
{
    public async Task OpenRideShareAsync()
    {
        var supportsUri = await Launcher.CanOpenAsync("lyft://");
        if (supportsUri)
            await Launcher.OpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

## <a name="api"></a>API

- [실행 프로그램 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Launcher)
- [시작 관리자 API 설명서](xref:Xamarin.Essentials.Launcher)
