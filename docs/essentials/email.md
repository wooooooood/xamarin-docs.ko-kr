---
title: 'Xamarin.Essentials: 전자 메일'
description: Xamarin.Essentials의 전자 메일 클래스에는 제목, 본문 및 받는 사람 (TO, CC, 숨은 참조)를 포함 하 여 지정 된 정보를 사용 하 여 기본 메일 응용 프로그램을 열려면 응용 프로그램 수 있습니다.
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: aea2f429126180ae3d98bc665bed5574f416ea53
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848548"
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials: 전자 메일

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **전자 메일** 클래스를 사용 하면 응용 프로그램 제목, 본문 및 recepients (TO, CC, 숨은 참조)를 포함 하 여 지정 된 정보를 사용 하 여 기본 메일 응용 프로그램을 엽니다.

## <a name="using-email"></a>전자 메일을 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

전자 메일 기능을 호출 하 여 작동 합니다 `ComposeAsync` 메서드는 `EmailMessage` 전자 메일에 대 한 정보를 포함 하는:

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
            // Some other exception occured
        }
    }
}
```

## <a name="api"></a>API

- [전자 메일 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [전자 메일 API 설명서](xref:Xamarin.Essentials.Email)
