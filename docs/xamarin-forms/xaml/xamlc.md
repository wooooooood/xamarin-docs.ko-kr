---
title: XAML 컴파일Xamarin.Forms
description: 이 문서에서는 xaml을 Xamarin.Forms xaml 컴파일러 (XAMLC)를 사용 하 여 선택적으로 IL (중간 언어)로 직접 컴파일하는 방법을 설명 합니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: eebbb3040175118320639bcb4482ec77b5c16ac7
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137295"
---
# <a name="xaml-compilation-in-xamarinforms"></a>XAML 컴파일Xamarin.Forms

_필요한 경우 XAML 컴파일러(XAMLC)를 사용하여 XAML을 중간 언어(IL)로 바로 컴파일할 수 있습니다._

XAML 컴파일은 다음과 같은 다양 한 이점을 제공 합니다.

- 컴파일 시 XAML 검사를 수행하여 발견된 오류를 사용자에게 알립니다.
- XAML 요소의 일부 로드 및 인스턴스화 시간을 제거합니다.
- 더 이상 .xaml 파일을 포함하지 않아 최종 어셈블리의 파일 크기를 줄이는 데 도움이 됩니다.

XAML 컴파일은 이전 버전과의 호환성을 위해 기본적으로 사용 하지 않도록 설정 되어 있습니다. 특성을 추가 하 여 어셈블리 및 클래스 수준 모두에서 사용 하도록 설정할 수 있습니다 [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) .

다음 코드 예제에서는 어셈블리 수준에서 XAML 컴파일을 사용 하도록 설정 하는 방법을 보여 줍니다.

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

이 예제에서 어셈블리에 포함 된 모든 XAML의 컴파일 타임 검사가 수행 되며,이는 컴파일 시간에 런타임 대신 XAML 오류가 보고 됩니다. 따라서 `assembly` 특성에 대 한 접두사는 [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) 특성이 전체 어셈블리에 적용 되도록 지정 합니다.

> [!NOTE]
> [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)특성과 [`XamlCompilationOptions`](xref:Xamarin.Forms.Xaml.XamlCompilationOptions) 열거형은 네임 스페이스에 있으며 `Xamarin.Forms.Xaml` ,이를 사용 하려면 가져와야 합니다.

다음 코드 예제에서는 클래스 수준에서 XAML 컴파일을 사용 하도록 설정 하는 방법을 보여 줍니다.

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

이 예제에서는 클래스의 XAML에 대 한 컴파일 타임 검사가 `HomePage` 수행 되 고 컴파일 프로세스의 일부로 오류가 보고 됩니다.

> [!NOTE]
> 컴파일된 바인딩을 사용 하면 응용 프로그램에서 데이터 바인딩 성능을 향상 시킬 수 있습니다 Xamarin.Forms . 자세한 내용은 [컴파일된 바인딩](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)을 참조하세요.

## <a name="related-links"></a>관련 링크

- [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)
- [`XamlCompilationOptions`](xref:Xamarin.Forms.Xaml.XamlCompilationOptions)
