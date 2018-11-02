---
title: Xamarin.Essentials 브라우저 열기
description: Xamarin.Essentials의 Browser 클래스를 사용하면 응용 프로그램이 최적화된 시스템 기본 브라우저 또는 외부 브라우저에서 웹 링크를 열 수 있습니다.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a68837ac4447dabcf52a1d1b27913adf80b4cbd7
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675395"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials: Browser

![시험판 NuGet](~/media/shared/pre-release.png)

**Browser** 클래스를 사용하면 응용 프로그램이 최적화된 시스템 기본 브라우저 또는 외부 브라우저에서 웹 링크를 열 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-browser"></a>Browser 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

Browser 기능은 `Uri` 및 `BrowserLaunchMode`로 `OpenAsync` 메서드를 호출하여 작동합니다.

```csharp

public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.OpenAsync(uri, BrowserLaunchMode.SystemPreferred);
    }
}
```

## <a name="platform-implementation-specifics"></a>플랫폼 구현 관련 정보

# <a name="androidtabandroid"></a>[Android](#tab/android)

시작 모드는 브라우저 시작 방법을 결정합니다.

## <a name="system-preferred"></a>시스템 기본 설정

[Chrome 사용자 지정 탭](https://developer.chrome.com/multidevice/android/customtabs)을 사용하여 URI를 로드하고 탐색 인식을 유지합니다.

## <a name="external"></a>외부

`Intent`를 사용하여 시스템 일반 브라우저에서 URI를 열도록 요청합니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>시스템 기본 설정

[SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/)를 사용하여 URI를 로드하고 탐색 인식을 유지합니다.

## <a name="external"></a>외부

기본 응용 프로그램의 표준 `OpenUrl`를 사용하여 응용 프로그램 외부에서 기본 브라우저를 시작합니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

`BrowserLaunchMode`와 관계없이 항상 사용자의 기본 브라우저가 시작됩니다.

--------------

## <a name="api"></a>API

- [Browser 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [Browser API 문서](xref:Xamarin.Essentials.Browser)
