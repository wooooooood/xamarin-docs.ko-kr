---
title: 'Xamarin.Essentials: 데이터 전송'
description: Xamarin.Essentials의 DataTransfer 클래스를 사용하면 응용 프로그램이 장치에 있는 다른 응용 프로그램의 텍스트 및 웹 링크와 같은 데이터를 공유할 수 있습니다.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 179d4327aa768e7aa2c81dbbffd694d078327400
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674836"
---
# <a name="xamarinessentials-data-transfer"></a>Xamarin.Essentials: 데이터 전송

![시험판 NuGet](~/media/shared/pre-release.png)

**DataTransfer** 클래스를 사용하면 응용 프로그램이 장치에 있는 다른 응용 프로그램의 텍스트 및 웹 링크와 같은 데이터를 공유할 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-data-transfer"></a>데이터 전송 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

데이터 전송 기능은 다른 응용 프로그램에 공유할 정보를 포함하는 데이터 요청 페이로드로 `RequestAsync` 메서드를 호출하여 작동합니다. 텍스트 및 URI는 혼합될 수 있으며 각 플랫폼은 콘텐츠를 기반으로 필터링을 처리합니다.

```csharp

public class DataTransferTest
{
    public async Task ShareText(string text)
    {
        await DataTransfer.RequestAsync(new ShareTextRequest
            {
                Text = text,
                Title = "Share Text"
            });
    }

    public async Task ShareUri(string uri)
    {
        await DataTransfer.RequestAsync(new ShareTextRequest
            {
                Uri = uri,
                Title = "Share Web Link"
            });
    }
}
```

요청 시 표시되는 외부 응용 프로그램에 공유할 사용자 인터페이스:

![데이터 전송](data-transfer-images/data-transfer.png)

## <a name="platform-differences"></a>플랫폼의 차이점

# <a name="androidtabandroid"></a>[Android](#tab/android)

* `Subject` 속성은 메시지의 원하는 제목에 사용됩니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

* `Subject`가 사용되지 않습니다.
* `Title`이 사용되지 않습니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* 설정되지 않은 경우 `Title`은 기본적으로 응용 프로그램 이름으로 설정됩니다.
* `Subject`가 사용되지 않습니다.

-----

## <a name="api"></a>API

- [데이터 전송 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DataTransfer)
- [데이터 전송 API 문서](xref:Xamarin.Essentials.DataTransfer)
