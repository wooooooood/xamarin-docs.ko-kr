---
title: 'Xamarin.Essentials: 전화 걸기'
description: Xamarin.Essentials PhoneDialer 클래스는 최적화 된 시스템 기본 설정된 브라우저 나 외부 브라우저에서 웹 링크를 열도록 응용 프로그램을 수 있습니다.
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6733e43ed4174d1dd78b2e8f70268eb54adadb98
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831399"
---
# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials: 전화 걸기

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **PhoneDialer** 클래스를 사용 하면 최적화 된 시스템 기본 설정된 브라우저 나 외부 브라우저에서 웹 링크를 열도록 응용 프로그램입니다.

## <a name="using-phone-dialer"></a>전화 걸기 사용

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

전화 걸기 기능을 호출 하 여 작동 합니다 `Open` 전화 번호를 사용 하 여 걸기를 열려면 메서드. 때 `Open` API는 지정 된 경우에 국가 코드를 기반으로 숫자 형식을 지정 하려고 자동으로 요청 합니다.

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
