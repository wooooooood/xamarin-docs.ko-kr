---
title: Xamarin.forms에서 XAML 컴파일
description: 이 문서에서는 어떻게 XAML 컴파일될 수 있습니다 필요에 따라 직접 IL (중간 언어) Xamarin.Forms XAML 컴파일러 (XAMLC)를 사용 하 여 설명 합니다.
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 08/22/2018
ms.openlocfilehash: de1ac47a56bf8d75eefb2ed6c7237f2f63f56ecc
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105950"
---
# <a name="xaml-compilation-in-xamarinforms"></a>Xamarin.forms에서 XAML 컴파일

_XAML 컴파일러 (XAMLC)를 사용 하 여 중간 언어 (IL)에 직접 XAML은 선택적으로 컴파일할 수 있습니다._

XAML 컴파일은 다양 한 이점 제공합니다.

- 컴파일 시 XAML 검사를 수행하여 발견된 오류를 사용자에게 알립니다.
- XAML 요소의 일부 로드 및 인스턴스화 시간을 제거합니다.
- 더 이상 .xaml 파일을 포함하지 않아 최종 어셈블리의 파일 크기를 줄이는 데 도움이 됩니다.

XAML 컴파일 이전 버전과 호환성을 보장 하기 위해 기본적으로 비활성화 됩니다. 추가 하 여 어셈블리와 클래스 수준에서 사용할 수 있습니다 합니다 [ `XamlCompilation` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) 특성입니다.

다음 코드 예제는 어셈블리 수준에서 사용 하도록 설정 하면 XAML 컴파일 방법을 보여 줍니다.

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

이 예제에서는 컴파일 시간 어셈블리 내에 포함 된 모든 XAML 검사는 수행 될 런타임 대신 컴파일 타임에 보고할 XAML 오류를 사용 하 여. 따라서 합니다 `assembly` 접두사를 사용 하는 [ `XamlCompilation` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) 특성 특성이 전체 어셈블리에 적용 되도록 지정 합니다.

> [!NOTE]
> 합니다 [ `XamlCompilation` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) 특성 및 [ `XamlCompilationOptions` ](xref:Xamarin.Forms.Xaml.XamlCompilationOptions) 열거형에는 `Xamarin.Forms.Xaml` 네임 스페이스를 사용 하 여 가져올 수 있어야 합니다.

다음 코드 예제는 클래스 수준에서 사용 하도록 설정 하면 XAML 컴파일 방법을 보여 줍니다.

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

이 예제에서는 컴파일 시간에 대 한 XAML 검사는 `HomePage` 수행할 클래스 및 컴파일 프로세스의 일부로 오류 보고 합니다.

> [!NOTE]
> 컴파일된 바인딩은 Xamarin.Forms 응용 프로그램에서 데이터 바인딩 성능 향상을 위해 사용할 수 있습니다. 자세한 내용은 [컴파일된 바인딩](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)합니다.

## <a name="related-links"></a>관련 링크

- [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)
- [`XamlCompilationOptions`](xref:Xamarin.Forms.Xaml.XamlCompilationOptions)
