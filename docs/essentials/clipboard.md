---
title: Xamarin.Essentials 클립보드
description: 클립보드 클래스를 사용 하면 응용 프로그램 간 시스템 클립보드에 텍스트 복사 및 붙여넣기 수 있습니다.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 218f90746f130f02c47040a9191b1001fa80c203
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials 클립보드

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
ClipBoard.SetText("Hello World");
```

텍스트를 읽을 수는 **클립보드**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>API

- [클립보드 소스 코드](https://github.com/xamarin/Essentials/tree/master/Essentials/Clipboard)
- [클립보드 API 설명서](xref:Xamarin.Essentials.Clipboard)
