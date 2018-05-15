---
title: Xamarin.Essentials 전자 메일
description: 전자 메일 클래스에 기본 전자 메일 응용 프로그램 제목, 본문 및 recepients (TO, CC, BCC)를 포함 하 여 지정 된 정보를 열도록 응용을 프로그램을 수 있습니다.
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: dde3f7f160d796afe3184ef1d61d8b437ec09ea8
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials 전자 메일

![시험판 NuGet](~/media/shared/pre-release.png)

**전자 메일** 클래스를 사용 하면 기본 전자 메일 응용 프로그램 제목, 본문 및 recepients (TO, CC, BCC)를 포함 하 여 지정 된 정보를 열도록 응용 프로그램을 합니다.

## <a name="using-email"></a>전자 메일을 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

호출 하 여 전자 메일 기능이 작동 하는 `ComposeAsync` 메서드는 `EmailMessage` 전자 메일에 대 한 정보가 들어 있는:

```csharp
public class EmailTest
{
    public async Task SendEmail(string subject, string, body, List<string> recipients)
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
            }
            await Email.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException fbsEx)
        {
            // Sms is not supported on this device
        }
        catch (Exception ex)
        {
            // Some other exception occured
        }
    }
}
```

## <a name="api"></a>API

- [전자 메일 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [전자 메일 API 설명서](xref:Xamarin.Essentials.Email)
