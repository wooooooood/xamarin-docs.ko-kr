---
title: 'Xamarin.Essentials: 클립보드'
description: 이 문서에서는 응용 프로그램 간에 텍스트를 복사하여 시스템 클립보드에 붙여넣을 수 있는 Xamarin.Essentials의 Clipboard 클래스를 설명합니다.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 8dd238da678dfb5773801137d313b286590aa463
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675538"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials: 클립보드

![시험판 NuGet](~/media/shared/pre-release.png)

**Clipboard** 클래스를 사용하여 응용 프로그램 간에 텍스트를 복사하여 시스템 클립보드에 붙여넣을 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-clipboard"></a>클립보드 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

**클립보드**에 현재 붙여넣을 준비가 된 텍스트가 있는지 확인합니다.

```csharp
var hasText = Clipboard.HasText;
```

**클립보드**의 텍스트를 설정합니다.

```csharp
Clipboard.SetText("Hello World");
```

**클립보드**에서 텍스트를 읽습니다.

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>API

- [클립보드 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [클립보드 API 문서](xref:Xamarin.Essentials.Clipboard)
