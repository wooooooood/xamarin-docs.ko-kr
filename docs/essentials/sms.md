---
title: 'Xamarin.Essentials: SMS'
description: Xamarin.Essentials Sms 클래스에는 지정 된 메시지를 받는 사람에 게 보낼 기본 SMS 응용 프로그램을 열려면 응용 프로그램 수 있습니다.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a93a67b83ea8f435a5e3ad5d26e1d6cbbb7092f7
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815599"
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials: SMS

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **Sms** 클래스를 사용 하면 지정 된 메시지를 받는 사람에 게 보낼 기본 SMS 응용 프로그램을 열려면 응용 프로그램입니다.

## <a name="using-sms"></a>Sms를 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

SMS 기능을 호출 하 여 작동 합니다 `ComposeAsync` 메서드는 `SmsMessage` 메시지의 받는 사람 및 사항이 모두 메시지의 본문을 포함 하 합니다.

```csharp
public class SmsTest
{
    public async Task SendSms(string messageText, string recipient)
    {
        try
        {
            var message = new SmsMessage(messageText, recipient);
            await Sms.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException ex)
        {
            // Sms is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="api"></a>API

- [Sms 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Sms)
- [Sms API 설명서](xref:Xamarin.Essentials.Sms)
