---
title: 'Xamarin.Essentials: 전자 메일'
description: Xamarin.Essentials의 Email 클래스를 사용하면 애플리케이션이 제목, 본문 및 받는 사람(받는 사람, 참조, 숨은 참조)을 포함한 지정된 정보를 사용하여 기본 메일 애플리케이션을 열 수 있습니다.
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.openlocfilehash: 06b4f4b612d0cb44e467a9da6dbee3194338027d
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58869964"
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials: 전자 메일

**Email** 클래스를 사용하면 애플리케이션이 제목, 본문 및 받는 사람(받는 사람, 참조, 숨은 참조)을 포함한 지정된 정보를 사용하여 기본 메일 애플리케이션을 열 수 있습니다.

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

모든 Android용 이메일 클라이언트가 `Html`을 지원하는 것은 아닙니다. 지원 여부를 확인할 방법은 없으므로 이메일을 보낼 때는 `PlainText`를 사용하는 것이 좋습니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

플랫폼의 차이점이 없습니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

`PlainText`를 `Html`을 보내려는 `BodyFormat`으로 지원하면 `FeatureNotSupportedException`을 throw합니다.

-----

## <a name="file-attachments"></a>첨부 파일

![미리 보기 기능](~/media/shared/preview.png)

이메일 파일은 Xamarin.Essentials 버전 1.1.0에서 실험적 미리 보기로 사용할 수 있습니다. 이 기능을 통해 앱이 디바이스의 이메일 클라이언트에 있는 파일을 이메일로 보낼 수 있습니다. 이 기능을 사용하려면 앱의 시작 코드에서 다음 속성을 설정합니다.

```csharp
ExperimentalFeatures.Enable(ExperimentalFeatures.EmailAttachments);
```

이 기능을 사용하도록 설정하면 모든 파일을 이메일로 보낼 수 있습니다. Xamarin.Essentials는 자동으로 파일 형식(MIME)을 검색하고 파일을 첨부 파일로 추가하도록 요청합니다. 모든 이메일 클라이언트는 서로 다르며 특정 파일 확장명만 지원하거나 전혀 지원하지 않을 수 있습니다.

다음은 디스크에 텍스트를 쓰고 이메일 첨부 파일로 추가하는 샘플입니다.

```csharp
var message = new EmailMessage
{
    Subject = "Hello",
    Body = "World",
};

var fn = "Attachment.txt";
var file = Path.Combine(FileSystem.CacheDirectory, fn);
File.WriteAllText(file, "Hello World");

message.Attachments.Add(new EmailAttachment(file));

await Email.ComposeAsync(message);
```

## <a name="api"></a>API

- [메일 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [메일 API 문서](xref:Xamarin.Essentials.Email)
