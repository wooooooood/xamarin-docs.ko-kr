---
title: Xamarin.Essentials Launcher
description: Xamarin.Essentials의 Launcher 클래스를 사용하면 응용 프로그램이 시스템을 통해 URI를 열 수 있습니다.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: 8f5ef8ef97999e9e85944d9fa9d4e57660779a48
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115785"
---
# <a name="xamarinessentials-launcher"></a>Xamarin.Essentials: Launcher

![시험판 NuGet](~/media/shared/pre-release.png)

**Launcher** 클래스를 사용하면 응용 프로그램이 시스템을 통해 URI를 열 수 있습니다. 다른 응용 프로그램의 사용자 지정 URI 체계에 대한 딥 링크를 설정할 때 주로 사용됩니다. 브라우저에서 웹 사이트를 열려면 **[Browser](open-browser.md)** API를 참조해야 합니다.

## <a name="using-launcher"></a>Launcher 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

Launcher 기능을 사용하려면 `OpenAsync` 메서드를 호출하고 열려는 `string` 또는 `Uri`를 전달합니다. 필요에 따라 `CanOpenAsync` 메서드를 사용하여 장치의 응용 프로그램이 URI 스키마를 처리할 수 있는지 확인할 수 있습니다.

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

## <a name="platform-differences"></a>플랫폼의 차이점

# <a name="androidtabandroid"></a>[Android](#tab/android)

`CanOpenAsync`에서 반환된 작업이 즉시 완료됩니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

이 장치의 대상 응용 프로그램이 사용자 응용 프로그램의 `OpenAsync`에 의해 열린 적이 없는 경우 iOS에서 앱이 열 수 있도록 허용하라는 메시지를 사용자에게 한 번 표시합니다.

`CanOpenAsync`에서 반환된 작업이 즉시 완료됩니다.

[여기](https://developer.xamarin.com/api/member/UIKit.UIApplication.CanOpenUrl/p/Foundation.NSUrl/)에서 iOS 구현에 대한 자세한 정보를 확인할 수 있습니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

플랫폼의 차이점이 없습니다.

-----

## <a name="api"></a>API

- [Launcher 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Launcher)
- [Launcher API 문서](xref:Xamarin.Essentials.Launcher)
