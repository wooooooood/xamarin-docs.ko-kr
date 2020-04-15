---
title: Xamarin.Essentials 색 변환기
description: Xamarin.Essentials의 ColorConverters 클래스는 System.Drawing.Color와 함께 작동하는 몇 가지 도우미 메서드와 확장 메서드를 제공합니다.
ms.assetid: B10428D6-89E2-4714-A39F-7E6E626391B2
author: jamesmontemagno
ms.author: jamont
ms.date: 01/06/2020
ms.openlocfilehash: 5d64967dfaa6ce7ef746a97f739cac67f5102fc2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "77545169"
---
# <a name="xamarinessentials-color-converters"></a>Xamarin.Essentials: 색 변환기

Xamarin.Essentials의 **ColorConverters** 클래스는 System.Drawing.Color에 대한 몇 가지 도우미 메서드를 제공합니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-color-converters"></a>색 변환기 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

`System.Drawing.Color`로 작업할 때 기본 제공된 Xamarin.Forms의 변환기를 사용하여 Hsl, Hex 또는 UInt에서 색을 만들 수 있습니다.

```csharp
var blueHex = ColorConverters.FromHex("#3498db");
var blueHsl = ColorConverters.FromHsl(204, 70, 53);
var blueUInt = ColorConverters.FromUInt(3447003);
```

## <a name="using-color-extensions"></a>색 확장 사용

`System.Drawing.Color`의 확장 메서드를 사용하면 다양한 속성을 적용할 수 있습니다.

```csharp
var blue = ColorConverters.FromHex("#3498db");

// Multiplies the current alpha by 50%
var blueWithAlpha = blue.MultiplyAlpha(.5f);
```

다음과 같은 몇 가지 확장 메서드가 있습니다.

- GetComplementary
- MultiplyAlpha
- ToUInt
- WithAlpha
- WithHue
- WithLuminosity
- WithSaturation

## <a name="using-platform-extensions"></a>플랫폼 확장 사용

또한 System.Drawing.Color를 플랫폼별 색 구조로 변환할 수 있습니다. 이러한 메서드는 iOS, Android 및 UWP 프로젝트에서만 호출할 수 있습니다.

```csharp
var system = System.Drawing.Color.FromArgb(255, 52, 152, 219);

// Extension to convert to Android.Graphics.Color, UIKit.UIColor, or Windows.UI.Color
var platform = system.ToPlatformColor();
```

```csharp
var platform = new Android.Graphics.Color(52, 152, 219, 255);

// Back to System.Drawing.Color
var system = platform.ToSystemColor();
```

`ToSystemColor` 메서드는 Android.Graphics.Color, UIKit.UIColor 및 Windows.UI.Color에 적용됩니다.

## <a name="api"></a>API

- [색 변환기 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/ColorConverters.shared.cs)
- [색 변환기 API 설명서](xref:Xamarin.Essentials.ColorConverters)
- [색 확장 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/ColorConverters.shared.cs)
- [색 확장 API 설명서](xref:Xamarin.Essentials.ColorExtensions)
