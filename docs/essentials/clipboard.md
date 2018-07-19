---
title: 'Xamarin.Essentials: 클립보드'
description: 이 문서에서 복사 하 고 응용 프로그램 간 시스템 클립보드에 텍스트를 붙여넣을 수 있어 Xamarin.Essentials 클립보드 클래스를 설명 합니다.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 41b15b480fa23bd49667b68e904043e4f1a95732
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38842617"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials: 클립보드

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **클립보드** 클래스를 사용 하면 응용 프로그램 간 시스템 클립보드에 텍스트 복사 및 붙여넣기 합니다.

## <a name="using-clipboard"></a>클립보드를 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

확인 합니다 **클립보드** 텍스트가 현재 붙여 넣을 수:

```csharp
var hasText = Clipboard.HasText;
```

텍스트 설정 하는 **클립보드**:

```csharp
Clipboard.SetText("Hello World");
```

텍스트를 읽는 합니다 **클립보드**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>API

- [클립보드 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [클립보드 API 설명서](xref:Xamarin.Essentials.Clipboard)
