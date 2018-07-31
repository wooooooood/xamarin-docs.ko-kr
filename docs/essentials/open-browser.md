---
title: Xamarin.Essentials 오픈 브라우저
description: 브라우저 클래스 Xamarin.Essentials에 최적화 된 시스템 기본 설정된 브라우저 나 외부 브라우저에서 웹 링크를 열도록 응용 프로그램을 수 있습니다.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7e58d439f5a6eaafe9b1b5e7ca874a986e468cb9
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353284"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials: 브라우저

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **브라우저** 클래스를 사용 하면 최적화 된 시스템 기본 설정된 브라우저 나 외부 브라우저에서 웹 링크를 열도록 응용 프로그램입니다.

## <a name="using-browser"></a>브라우저를 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

브라우저 기능을 호출 하 여 작동 합니다 `OpenAsync` 메서드를 `Uri` 및 `BrowserLaunchMode`합니다.

```csharp

public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.OpenAsync(uri, BrowserLaunchMode.SystemPreferred);
    }
}
```

## <a name="platform-implementation-specifics"></a>플랫폼 구현 세부 정보

# <a name="androidtabandroid"></a>[Android](#tab/android)

시작 모드는 브라우저가 시작 되는 방식을 결정 합니다.

## <a name="system-preferred"></a>시스템 기본 설정

[사용자 지정 탭 chrome](https://developer.chrome.com/multidevice/android/customtabs) 사용할 시도 Uri를 로드 하 고 탐색 인식 유지 합니다.

## <a name="external"></a>외부

`Intent` Uri 시스템 기본 브라우저를 통해 열 수를 요청 하 게 됩니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>시스템 기본 설정

[필요한 SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) 에 Uri를 로드 하 고 탐색 인식 유지 하는 데 사용 됩니다.

## <a name="external"></a>외부

표준 `OpenUrl` 주 응용 프로그램에서 응용 프로그램 외부의 기본 브라우저가 시작을 사용 합니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

사용자의 기본 브라우저에 관계 없이 항상 실행을 `BrowserLaunchMode`입니다.

--------------

## <a name="api"></a>API

- [브라우저 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [API 브라우저 설명서](xref:Xamarin.Essentials.Browser)
