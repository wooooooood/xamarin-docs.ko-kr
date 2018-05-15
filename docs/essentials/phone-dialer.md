---
title: Xamarin.Essentials 전화 걸기
description: PhoneDialer 클래스에 최적화 된 시스템 기본 브라우저 나 외부 브라우저에서 웹 링크를 열도록 응용을 프로그램을 수 있습니다.
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 112cc305457413ad057e390d46c5a765ea29514f
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials 전화 걸기

![시험판 NuGet](~/media/shared/pre-release.png)

**PhoneDialer** 클래스에 최적화 된 시스템 기본 브라우저 나 외부 브라우저에서 웹 링크를 열도록 응용 프로그램을 사용 하도록 설정 합니다.

## <a name="using-phone-dialer"></a>전화 걸기 사용

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

호출 하 여 전화 걸기 기능이 작동 하는 `Open` 와 걸기를 열려면 전화 번호를 사용 하 여 메서드. 때 `Open` API에서 자동으로 지정 된 경우에 국가 코드를 기반으로 숫자의 서식을 지정 하려면 시도 요청 합니다.

```csharp
public class PhoneDialerTest
{
    public async Task PlacePhoneCall(string number)
    {
        try
        {
            PhoneDialer.Open(number);
        }
        catch (ArgumentNullException anEx)
        {
            // Number was null or white space
        }
        catch (FeatureNotSupportedException ex)
        {
            // Phone Dialer is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="api"></a>API

- [전화 걸기 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/PhoneDialer)
- [전화 걸기 API 설명서](xref:Xamarin.Essentials.PhoneDialer)
