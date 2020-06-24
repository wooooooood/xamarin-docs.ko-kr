---
title: Xamarin.Essentials 브라우저 열기
description: Xamarin.Essentials에서 Browser 클래스를 사용하면 애플리케이션이 최적화된 시스템 기본 브라우저 또는 외부 브라우저에서 웹 링크를 열 수 있습니다.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.custom: video
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 997c6b66b5dba43eb440130f3f58d31a5a274815
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84802242"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials: 브라우저

**Browser** 클래스를 사용하면 애플리케이션이 최적화된 시스템 기본 브라우저 또는 외부 브라우저에서 웹 링크를 열 수 있습니다.

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

이 메서드는 브라우저가 _시작_된 후 반환되며, 반드시 사용자가 _종료_하는 것은 아닙니다.  `bool` 결과는 시작의 성공 여부를 나타냅니다.

## <a name="customization"></a>사용자 지정

시스템 기본 브라우저를 사용할 때 iOS 및 Android에서 사용할 수 있는 몇 가지 사용자 지정 옵션이 있습니다. 여기에는 `TitleMode`(Android에만 해당), 표시되는 `Toolbar`(iOS 및 Android) 및 `Controls`(iOS만 해당)에 대한 기본 색상 옵션이 포함됩니다.

이러한 옵션은 `OpenAsync`를 호출할 때 `BrowserLaunchOptions`를 사용하여 지정됩니다.

```csharp
await Browser.OpenAsync(uri, new BrowserLaunchOptions
                {
                    LaunchMode = BrowserLaunchMode.SystemPreferred,
                    TitleMode = BrowserTitleMode.Show,
                    PreferredToolbarColor = Color.AliceBlue,
                    PreferredControlColor = Color.Violet
                });
```

![브라우저 옵션](images/browser-options.png)

## <a name="platform-implementation-specifics"></a>플랫폼 구현 관련 정보

# <a name="android"></a>[Android](#tab/android)

시작 모드는 브라우저 시작 방법을 결정합니다.

## <a name="system-preferred"></a>시스템 기본 설정

[사용자 지정 탭](https://developer.chrome.com/multidevice/android/customtabs)을 사용하여 URI를 로드하고 탐색 인식을 유지합니다.

## <a name="external"></a>외부

`Intent`를 사용하여 시스템 일반 브라우저에서 URI를 열도록 요청합니다.

# <a name="ios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>시스템 기본 설정

[SFSafariViewController](xref:SafariServices.SFSafariViewController)를 사용하여 URI를 로드하고 탐색 인식을 유지합니다.

## <a name="external"></a>외부

기본 애플리케이션의 표준 `OpenUrl`를 사용하여 애플리케이션 외부에서 기본 브라우저를 시작합니다.

# <a name="uwp"></a>[UWP](#tab/uwp)

`BrowserLaunchMode`와 관계없이 항상 사용자의 기본 브라우저가 시작됩니다.

--------------

## <a name="api"></a>API

- [Browser 소스 코드](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Browser)
- [Browser API 문서](xref:Xamarin.Essentials.Browser)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Open-Browser-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
