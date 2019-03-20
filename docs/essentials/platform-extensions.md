---
title: Xamarin.Essentials 플랫폼 확장
description: Xamarin.Essentials는 Rect, 크기 및 포인트와 같은 플랫폼 형식으로 작업해야 할 때 몇 가지 플랫폼 확장 메서드를 제공합니다.
ms.assetid: AB4D198A-4FD7-479E-8627-01F887A6D056
author: jamesmontemagno
ms.author: jamont
ms.date: 03/13/2019
ms.openlocfilehash: 806fcb3fa90a5ec9d39cecb743b72116b8388a03
ms.sourcegitcommit: 64d6da88bb6ba222ab2decd2fdc8e95d377438a6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58176041"
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
var platform = system.ToPlatformSize();

// Back to System.Drawing.Size
var system2 = platform.ToSystemSize();
```

### <a name="size"></a>크기

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
var platform = system.ToPlatformSize();

// Back to System.Drawing.Size
var system2 = platform.ToSystemSize();
```

## <a name="api"></a>API

- [변환기 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/PlatformExtensions)
- [포인트 변환기 API 설명서](xref:Xamarin.Essentials.PointConverters)
- [사각형 변환기 API 설명서](xref:Xamarin.Essentials.RectangleConverters)
- [크기 변환기 API 설명서](xref:Xamarin.Essentials.SizeConverters)
