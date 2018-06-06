---
title: Xamarin.Essentials 열기 브라우저
description: 브라우저 클래스 Xamarin.Essentials에 최적화 된 시스템 기본 브라우저 나 외부 브라우저에서 웹 링크를 열도록 응용을 프로그램을 수 있습니다.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 563d3899cffb80c0215d90e8e4392046c4635256
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783137"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials: 브라우저

![시험판 NuGet](~/media/shared/pre-release.png)

**브라우저** 클래스에 최적화 된 시스템 기본 브라우저 나 외부 브라우저에서 웹 링크를 열도록 응용 프로그램을 사용 하도록 설정 합니다.

## <a name="using-browser"></a>브라우저를 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

호출 하 여 브라우저 기능이 작동 하는 `OpenAsync` 메서드는 `Uri` 및 `BrowserLaunchType`합니다.

```csharp

public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.OpenAsync(uri, BrowserLaunchType.SystemPreferred);
    }
}
```

## <a name="platform-implementation-specifics"></a>플랫폼 구현 세부 사항

# <a name="androidtabandroid"></a>[Android](#tab/android)

시작 유형은 브라우저가 시작 되는 방식을 결정 합니다.

## <a name="system-preferred"></a>시스템 기본 설정

[사용자 지정 탭 크롬](https://developer.chrome.com/multidevice/android/customtabs) 됩니다 사용 하려고 했습니다. Uri를 로드 하 고 탐색 인식 유지 합니다.

## <a name="external"></a>외부

`Intent` 요청 Uri는 시스템 기본 브라우저를 통해 열 수를 사용할 수 있습니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>시스템 기본 설정

[SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) 에 Uri를 로드 하 고 탐색 인식 유지 하는 데 사용 됩니다.

## <a name="external"></a>외부

표준 `OpenUrl` 은 주 응용 프로그램에는 응용 프로그램 외부의 기본 브라우저를 시작 합니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

사용자의 기본 브라우저에 관계 없이 항상 실행은 `BrowserLaunchType`합니다.

--------------

## <a name="api"></a>API

- [브라우저 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [브라우저 API 설명서](xref:Xamarin.Essentials.Browser)
