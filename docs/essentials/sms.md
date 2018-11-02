---
title: 'Xamarin.Essentials: SMS'
description: Xamarin.Essentials의 SMS 클래스를 사용하면 응용 프로그램이 기본 SMS 응용 프로그램에서 수신자에게 보내도록 지정된 메시지를 열 수 있습니다.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: fa55a17e99a11861b98c4d515df882ed3af58a0b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102739"
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials: SMS

![시험판 NuGet](~/media/shared/pre-release.png)

**Sms** 클래스를 사용하면 응용 프로그램이 기본 SMS 응용 프로그램에서 수신자에게 보내도록 지정된 메시지를 열 수 있습니다.

## <a name="using-sms"></a>Sms 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

SMS 기능은 메시지의 수신자와 메시지 본문이 포함된 `SmsMessage`에 대해 `ComposeAsync` 메서드를 호출하여 작동합니다.

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

또한 여러 명의 수신자를 `SmsMessage`에 전달할 수 있습니다.

```csharp
public class SmsTest
{
    public async Task SendSms(string messageText, string[] recipients)
    {
        try
        {
            var message = new SmsMessage(messageText, recipients);
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
- [Sms API 문서](xref:Xamarin.Essentials.Sms)
