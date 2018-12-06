---
title: 'Xamarin.Essentials: Share'
description: Xamarin.Essentials의 Share 클래스를 사용하면 애플리케이션이 디바이스에 있는 다른 애플리케이션의 텍스트 및 웹 링크와 같은 데이터를 공유할 수 있습니다.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 6fd3fd90d1e2ada225dafdd855f8903688677660
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52899376"
---
# <a name="xamarinessentials-share"></a>Xamarin.Essentials: Share

**Share** 클래스를 사용하면 Share이 디바이스에 있는 다른 Share의 텍스트 및 웹 링크와 같은 데이터를 공유할 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-share"></a>Share 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

Share 기능은 다른 애플리케이션에 공유할 정보를 포함하는 데이터 요청 페이로드로 `RequestAsync` 메서드를 호출하여 작동합니다. 텍스트 및 URI는 혼합될 수 있으며 각 플랫폼은 콘텐츠를 기반으로 필터링을 처리합니다.

```csharp

public class ShareTest
{
    public async Task ShareText(string text)
    {
        await Share.RequestAsync(new ShareTextRequest
            {
                Text = text,
                Title = "Share Text"
            });
    }

    public async Task ShareUri(string uri)
    {
        await Share.RequestAsync(new ShareTextRequest
            {
                Uri = uri,
                Title = "Share Web Link"
            });
    }
}
```

요청 시 표시되는 외부 응용 프로그램에 공유할 사용자 인터페이스:

![공유](share-images/share.png)

## <a name="platform-differences"></a>플랫폼의 차이점

# <a name="androidtabandroid"></a>[Android](#tab/android)

* `Subject` 속성은 메시지의 원하는 제목에 사용됩니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

* `Subject`이 사용되지 않습니다.
* `Title`이 사용되지 않습니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* 설정되지 않은 경우 `Title`은 기본적으로 응용 프로그램 이름으로 설정됩니다.
* `Subject`이 사용되지 않습니다.

-----

## <a name="api"></a>API

- [Share 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Share)
- [Share API 문서](xref:Xamarin.Essentials.Share)
