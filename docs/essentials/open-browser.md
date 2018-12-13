---
title: Xamarin.Essentials 브라우저 열기
description: Xamarin.Essentials의 Browser 클래스를 사용하면 응용 프로그램이 최적화된 시스템 기본 브라우저 또는 외부 브라우저에서 웹 링크를 열 수 있습니다.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: ea2a10c11a77fcb2b3ce142d176522ebf0310725
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898903"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials: 브라우저

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
    public async Task<bool> OpenBrowser(Uri uri)
    {
        return await Browser.OpenAsync(uri, BrowserLaunchMode.SystemPreferred);
    }
}
```

이 메서드는 브라우저가 _시작_된 후 반환되며, 반드시 사용자가 _종료_하는 것은 아닙니다.  `bool` 결과는 시작의 성공 여부를 나타냅니다.

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
