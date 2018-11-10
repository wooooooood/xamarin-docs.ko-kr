---
title: 'Xamarin.Essentials: 메일'
description: Xamarin.Essentials의 Email 클래스를 사용하면 응용 프로그램이 제목, 본문 및 받는 사람(받는 사람, 참조, 숨은 참조)을 포함한 지정된 정보를 사용하여 기본 메일 응용 프로그램을 열 수 있습니다.
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: c8d4a83caf6832f911193067324915fd6226b380
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674966"
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

## <a name="api"></a>API

- [메일 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [메일 API 문서](xref:Xamarin.Essentials.Email)
