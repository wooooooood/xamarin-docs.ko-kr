---
title: Xamarin.forms에서 XAML 컴파일
description: 이 문서에서는 XAML을 Xamarin.Forms XAML 컴파일러(XAMLC)를 사용하여 선택적으로 중간 언어(IL, Intermediate Language)로 직접 컴파일 할 수 있는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/22/2018
ms.openlocfilehash: 9567f3ad8d748a94a03cd1c86254072d4ba3bbdc
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563526"
---
# <a name="xaml-compilation-in-xamarinforms"></a>Xamarin.forms에서 XAML 컴파일

_XAML은 XAML 컴파일러(XAMLC)를 사용하여 선택적으로 중간 언어(IL, Intermediate Language)로 직접 컴파일할 수 있습니다._

XAML 컴파일은 다음과 같이 다양한 이점을 제공합니다.

- XAML 컴파일은 컴파일 시 XAML 검사를 수행하여 발견된 오류를 사용자에게 알립니다.
- XAML 컴파일은 XAML 요소에 대한 일부 로드 및 인스턴스화 시간을 제거합니다.
- XAML 컴파일은 더 이상 .xaml 파일을 포함하지 않으므로 최종 어셈블리의 파일 크기를 줄이는 데 도움이 됩니다.

XAML 컴파일은 기본적으로 이전 버전과의 호환성 보장을 위해 비활성화 됩니다. [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) 특성을 추가하여 어셈블리와 클래스 수준에서 활성화 할 수 있습니다.

다음 코드 예제는 어셈블리 수준에서 XAML 컴파일을 활성화하는 방법을 보여 줍니다.

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

해당 예제에서는 어셈블리 내에 포함 된 모든 XAML의 컴파일 시 검사가 수행되며 런타임 대신 컴파일 시에 XAML 오류가 보고 됩니다. 따라서 [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) 특성에 대한 접두어 `assembly`는 속성이 전체 어셈블리에 적용되도록 지정합니다.

> [!NOTE]
> [ `XamlCompilation` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) 특성 및 [ `XamlCompilationOptions` ](xref:Xamarin.Forms.Xaml.XamlCompilationOptions) 열거형은 사용하기 위해 포함되어야 하는 `Xamarin.Forms.Xaml` 네임 스페이스에 있습니다.

다음 코드 예제는 클래스 수준에서 XAML 컴파일 활성화 방법을 보여 줍니다.

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

이 예제에서는 `HomePage` 클래스에 대한 XAML의 컴파일 시 검사가 수행되고 컴파일 프로세스의 일부로 오류가 보고 됩니다.

> [!NOTE]
> 컴파일 된 바인딩을 사용하면 Xamarin.Forms 응용 프로그램에서 데이터 바인딩 성능을 향상시킬 수 있습니다. 자세한 내용은 [컴파일 된 바인딩](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)을 참조하십시오.

## <a name="related-links"></a>관련 링크

- [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)
- [`XamlCompilationOptions`](xref:Xamarin.Forms.Xaml.XamlCompilationOptions)
