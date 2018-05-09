---
title: Xamarin.Essentials 데이터 전송
description: DataTransfer 클래스에는 응용을 프로그램을 장치에 다른 응용 프로그램에 대 한 텍스트와 웹 링크 등의 데이터를 공유할 수 있습니다.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6c6521c9bacd7b3e9da67165d2d271f5a40e4d7a
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-data-transfer"></a>Xamarin.Essentials 데이터 전송

![시험판 NuGet](~/media/shared/pre-release.png)

**DataTransfer** 클래스를 사용 하면 응용 프로그램을 장치에 다른 응용 프로그램에 대 한 텍스트와 웹 링크와 같은 데이터를 공유 합니다.

## <a name="using-data-transfer"></a>데이터 전송을 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

데이터 전송 기능을 호출 하 여 사용할는 `RequestAsync` 메서드를 다른 응용 프로그램에 공유 하는 정보가 포함 된 데이터 요청 페이로드입니다. 텍스트 및 Uri를 함께 사용할 수 있습니다 및 각 플랫폼 내용을 기반으로 필터링을 처리 합니다.

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

사용자 인터페이스를 요청이 있을 때 표시 되는 외부 응용 프로그램에 공유:

![데이터 전송](data-transfer-images/data-transfer.png)

## <a name="platform-differences"></a>플랫폼의 차이점

| 플랫폼 | 차이 |
| --- | --- |
| Android | Subject 속성은 메시지의 원하는 제목에 사용 됩니다. |
| iOS | 주체를 사용 되지 않습니다. |
| iOS | 제목 사용 되지 않습니다. |
| UWP | 제목 설정 하지 않으면 기본적으로 응용 프로그램 이름으로 됩니다. |
| UWP | 주체를 사용 되지 않습니다. |

## <a name="api"></a>API

- [데이터 전송 소스 코드](https://github.com/xamarin/Essentials/tree/master/Essentials/DataTransfer)
- [데이터 전송 API 설명서](xref:Xamarin.Essentials.DataTransfer)
