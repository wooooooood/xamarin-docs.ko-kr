---
title: “Xamarin.Essentials: SMS” description: “Xamarin.Essentials의 SMS 클래스를 사용하면 애플리케이션이 기본 SMS 애플리케이션에서 수신자에게 보내도록 지정된 메시지를 열 수 있습니다.”
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58 author: jamesmontemagno ms.custom: video ms.author: jamont ms.date: 11/04/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-sms"></a>Xamarin.Essentials: SMS

**Sms** 클래스를 사용하면 애플리케이션이 기본 SMS 애플리케이션에서 수신자에게 보내도록 지정된 메시지를 열 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

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
            var message = new SmsMessage(messageText, new []{ recipient });
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

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Shows/XamarinShow/SMS-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
