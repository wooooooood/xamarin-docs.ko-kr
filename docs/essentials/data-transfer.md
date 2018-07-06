---
title: 'Xamarin.Essentials: 데이터 전송'
description: Xamarin.Essentials DataTransfer 클래스 응용 프로그램을을 장치에서 다른 응용 프로그램에 대 한 텍스트 및 웹 링크와 같은 데이터를 공유할 수 있습니다.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: c1ed298e1317d0a3f78f4dbd9fc89a2b01c6958c
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855112"
---
# <a name="xamarinessentials-data-transfer"></a>Xamarin.Essentials: 데이터 전송

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **DataTransfer** 클래스를 사용 하면 응용 프로그램을 장치에서 다른 응용 프로그램에 대 한 텍스트 및 웹 링크와 같은 데이터를 공유 합니다.

## <a name="using-data-transfer"></a>데이터 전송을 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

데이터 전송 기능을 호출 하 여 작동 합니다 `RequestAsync` 다른 응용 프로그램에 공유 하는 정보가 포함 된 데이터 요청 페이로드를 사용 하 여 메서드. 텍스트 및 Uri를 혼합할 수 있습니다 및 플랫폼별 콘텐츠를 기반으로 필터링을 처리 합니다.

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

요청이 있을 때 표시 되는 외부 응용 프로그램에 공유 사용자 인터페이스:

![데이터 전송](data-transfer-images/data-transfer.png)

## <a name="platform-differences"></a>플랫폼의 차이점

# <a name="androidtabandroid"></a>[Android](#tab/android)

* `Subject` 속성은 메시지의 원하는 제목에 사용 됩니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

* `Subject` 사용 되지 않습니다.
* `Title` 사용 되지 않습니다. 

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* `Title` 기본적으로 응용 프로그램 이름으로 설정 되지 않은 경우.
* `Subject` 사용 되지 않습니다.

-----

## <a name="api"></a>API

- [데이터 전송 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DataTransfer)
- [데이터 전송 API 설명서](xref:Xamarin.Essentials.DataTransfer)
