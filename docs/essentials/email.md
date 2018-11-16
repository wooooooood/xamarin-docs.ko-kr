---
title: 'Xamarin.Essentials: 메일'
description: Xamarin.Essentials의 Email 클래스를 사용하면 응용 프로그램이 제목, 본문 및 받는 사람(받는 사람, 참조, 숨은 참조)을 포함한 지정된 정보를 사용하여 기본 메일 응용 프로그램을 열 수 있습니다.
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3c2958cc4572c2f87c46c9edc5fc194284658f24
ms.sourcegitcommit: 704d4cfd418c17b0e85a20c33a16d2419db0be71
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51691752"
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials: 메일

![시험판 NuGet](~/media/shared/pre-release.png)

**Email** 클래스를 사용하면 응용 프로그램이 제목, 본문 및 받는 사람(받는 사람, 참조, 숨은 참조)을 포함한 지정된 정보를 사용하여 기본 메일 응용 프로그램을 열 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-email"></a>메일 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

메일 기능은 메일에 대한 정보를 포함하는 `ComposeAsync` 메서드 `EmailMessage`를 호출하여 작동합니다.

```csharp
public class EmailTest
{
    public async Task SendEmail(string subject, string body, List<string> recipients)
    {
        try
        {
            var message = new EmailMessage
            {
                Subject = subject,
                Body = body,
                To = recipients,
                //Cc = ccRecipients,
                //Bcc = bccRecipients
            };
            await Email.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException fbsEx)
        {
            // Email is not supported on this device
        }
        catch (Exception ex)
        {
            // Some other exception occurred
        }
    }
}
```


## <a name="platform-differences"></a>플랫폼의 차이점

# <a name="androidtabandroid"></a>[Android](#tab/android)

플랫폼의 차이점이 없습니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

플랫폼의 차이점이 없습니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

`PlainText`를 `Html`을 보내려는 `BodyFormat`으로 지원하면 `FeatureNotSupportedException`을 throw합니다.

-----

## <a name="api"></a>API

- [메일 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [메일 API 문서](xref:Xamarin.Essentials.Email)
