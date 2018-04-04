---
title: XAML 컴파일
description: 필요한 경우 XAML 컴파일러(XAMLC)를 사용하여 XAML을 중간 언어(IL)로 바로 컴파일할 수 있습니다.
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/21/2016
ms.openlocfilehash: fc4c7df6011fdf8d263b9fca88a5ffb551ec78e3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="xaml-compilation"></a>XAML 컴파일

_IL (중간 언어) XAML 컴파일러 (XAMLC)에 직접 XAML은 선택적으로 컴파일할 수 있습니다._

XAMLC는 여러 가지 이점을 제공합니다.

- 컴파일 시 XAML 검사를 수행하여 발견된 오류를 사용자에게 알립니다.
- XAML 요소의 일부 로드 및 인스턴스화 시간을 제거합니다.
- 더 이상 .xaml 파일을 포함하지 않아 최종 어셈블리의 파일 크기를 줄이는 데 도움이 됩니다.

XAMLC는 이전 버전과의 호환성을 위해 기본적으로 비활성화됩니다. 추가 하 여 어셈블리 및 클래스 수준에서 사용할 수 있습니다는 `XamlCompilation` 특성입니다.

다음 코드 예제에서는 XAMLC 어셈블리 수준에서 사용 하도록 설정 하는 방법을 보여 줍니다.

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

이 예제에서는 모든 XAML의 컴파일 타임 검사 내에 포함 된는 `PhotoApp` 네임 스페이스 수행 됩니다, 런타임 대신 컴파일 타임에서 보고 되 고 XAML 오류가 발생 합니다.
`assembly` 접두사에 `XamlCompilation` 특성 특성이 전체 어셈블리에 적용 되도록 지정 합니다.

다음 코드 예제에서는 XAMLC 클래스 수준에서 사용 하도록 설정 하는 방법을 보여 줍니다.

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

이 예제에서는 컴파일 타임에 대 한 XAML의 검사는 `HomePage` 컴파일 프로세스의 일부로 오류 보고 및 클래스 수행 됩니다.

> [!NOTE]
> `XamlCompilation` 특성 및 `XamlCompilationOptions` 열거형에 있는 `Xamarin.Forms.Xaml` 네임 스페이스를 사용 하 여 가져와야 합니다.


## <a name="related-links"></a>관련 링크

- [XamlCompilation](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/)
- [XamlCompilationOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationOptions/)
