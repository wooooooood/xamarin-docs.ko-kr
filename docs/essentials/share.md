---
title: 'Xamarin.Essentials: 공유'
description: Xamarin.Essentials의 Share 클래스를 사용하면 애플리케이션이 디바이스에 있는 다른 애플리케이션의 텍스트 및 웹 링크와 같은 데이터를 공유할 수 있습니다.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 08/20/2019
ms.custom: video
ms.openlocfilehash: b889573c0cefbd692b1e879c1612a143d4bb7c99
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70120756"
---
# <a name="xamarinessentials-share"></a>Xamarin.Essentials: 공유

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

요청 시 표시되는 외부 애플리케이션에 공유할 사용자 인터페이스:

![공유](images/share.png)

## <a name="files"></a>파일

이 기능을 통해 앱이 디바이스의 다른 애플리케이션과 파일을 공유할 수 있습니다. Xamarin.Essentials는 자동으로 파일 형식(MIME)을 검색하고 공유를 요청합니다. 각 플랫폼은 특정 파일 확장명만 지원할 수 있습니다.

다음은 디스크에 텍스트를 작성하고 다른 앱과 공유하는 샘플입니다.

```csharp
var fn =  "Attachment.txt";
var file = Path.Combine(FileSystem.CacheDirectory, fn);
File.WriteAllText(file, "Hello World");

await Share.RequestAsync(new ShareFileRequest
{
    Title = Title,
    File = new ShareFile(file)
});
```

## <a name="platform-differences"></a>플랫폼의 차이점

# <a name="androidtabandroid"></a>[Android](#tab/android)

- `Subject` 속성은 메시지의 원하는 제목에 사용됩니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

- `Subject`가 사용되지 않습니다.
- `Title`이 사용되지 않습니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- 설정되지 않은 경우 `Title`은 기본적으로 애플리케이션 이름으로 설정됩니다.
- `Subject`가 사용되지 않습니다.

-----


## <a name="api"></a>API

- [Share 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Share)
- [Share API 문서](xref:Xamarin.Essentials.Share)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Share-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
