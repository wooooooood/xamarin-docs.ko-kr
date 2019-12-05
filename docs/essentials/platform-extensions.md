---
title: Xamarin.Essentials 플랫폼 확장
description: Xamarin.Essentials는 Rect, 크기 및 포인트와 같은 플랫폼 형식으로 작업해야 할 때 몇 가지 플랫폼 확장 메서드를 제공합니다.
ms.assetid: AB4D198A-4FD7-479E-8627-01F887A6D056
author: jamesmontemagno
ms.author: jamont
ms.date: 03/13/2019
ms.openlocfilehash: 4a80c004dd55486db18a3149dcd889a4673d7438
ms.sourcegitcommit: 1c87135a47780f34102952d4b140850b4f08b075
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74536495"
---
# <a name="xamarinessentials-platform-extensions"></a>Xamarin.Essentials: 플랫폼 확장

Xamarin.Essentials는 Rect, 크기 및 포인트와 같은 플랫폼 형식으로 작업해야 할 때 몇 가지 플랫폼 확장 메서드를 제공합니다. 즉, iOS, Android 및 UWP의 특정 유형에 대해 이 유형의 `System` 버전 사이를 변환할 수 있습니다. 

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-platform-extensions"></a>플랫폼 확장 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

모든 플랫폼 확장은 iOS, Android 또는 UWP 프로젝트에서만 호출할 수 있습니다.

### <a name="point"></a>요소

```csharp
var system = new System.Drawing.Point(x, y);

// Convert to CoreGraphics.CGPoint, Android.Graphics.Point, and Windows.Foundation.Point
var platform = system.ToPlatformPoint();

// Back to System.Drawing.Point
var system2 = platform.ToSystemPoint();
```

### <a name="size"></a>Size

```csharp
var system = new System.Drawing.Size(width, height);

// Convert to CoreGraphics.CGSize, Android.Util.Size, and Windows.Foundation.Size
var platform = system.ToPlatformSize();

// Back to System.Drawing.Size
var system2 = platform.ToSystemSize();
```

### <a name="rectangle"></a>사각형

```csharp
var system = new System.Drawing.Rectangle(x, y, width, height);

// Convert to CoreGraphics.CGRect, Android.Graphics.Rect, and Windows.Foundation.Rect
var platform = system.ToPlatformRectangle();

// Back to System.Drawing.Rectangle
var system2 = platform.ToSystemRectangle();
```

## <a name="api"></a>API

- [변환기 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/PlatformExtensions)
- [포인트 변환기 API 설명서](xref:Xamarin.Essentials.PointExtensions)
- [사각형 변환기 API 설명서](xref:Xamarin.Essentials.RectangleExtensions)
- [크기 변환기 API 설명서](xref:Xamarin.Essentials.SizeExtensions)
