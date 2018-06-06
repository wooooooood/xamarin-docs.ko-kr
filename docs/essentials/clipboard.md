---
title: 'Xamarin.Essentials: 클립보드'
description: 이 문서에서 응용 프로그램 간 시스템 클립보드에 텍스트 복사 및 붙여넣기 수 있는 Xamarin.Essentials 클립보드 클래스를 설명 합니다.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 41b15b480fa23bd49667b68e904043e4f1a95732
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782361"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials: 클립보드

![시험판 NuGet](~/media/shared/pre-release.png)

**클립보드** 클래스를 사용 하면 응용 프로그램 간 시스템 클립보드에 텍스트 복사 및 붙여넣기 합니다.

## <a name="using-clipboard"></a>클립보드 사용

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

확인 하는 **클립보드** 텍스트가 현재 붙여 넣을 수 있습니다.

```csharp
var hasText = Clipboard.HasText;
```

텍스트 설정 하는 **클립보드**:

```csharp
Clipboard.SetText("Hello World");
```

텍스트를 읽을 수는 **클립보드**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>API

- [클립보드 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [클립보드 API 설명서](xref:Xamarin.Essentials.Clipboard)
