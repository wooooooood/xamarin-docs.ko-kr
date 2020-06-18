---
title: Xamarin.Essentials 단위 변환기
description: Xamarin.Essentials의 UnitConverters 클래스는 Xamarin.Essentials를 사용할 때 개발자를 돕기 위한 여러 단위 변환기를 제공합니다.
ms.assetid: 35DE2704-E730-4337-9476-66CD53376943
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 01/06/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 332762a83796fef24278c6685a29f7d31a10dadc
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84801857"
---
# <a name="xamarinessentials-unit-converters"></a>Xamarin.Essentials: 단위 변환기

**UnitConverters** 클래스는 Xamarin.Essentials를 사용할 때 개발자를 돕기 위한 여러 단위 변환기를 제공합니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-unit-converters"></a>단위 변환기 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

모든 단위 변환기는 Xamarin.Essentials의 정적 `UnitConverters` 클래스를 통해 사용할 수 있습니다. 예를 들어 화씨온도를 쉽게 섭씨온도로 변환할 수 있습니다.

```csharp
var celsius = UnitConverters.FahrenheitToCelsius(32.0);
```

다음은 사용 가능한 변환 목록입니다.

- FahrenheitToCelsius
- CelsiusToFahrenheit
- CelsiusToKelvin
- KelvinToCelsius
- MilesToMeters
- MilesToKilometers
- KilometersToMiles
- MetersToInternationalFeet
- InternationalFeetToMeters
- DegreesToRadians
- RadiansToDegrees
- DegreesPerSecondToRadiansPerSecond
- RadiansPerSecondToDegreesPerSecond
- DegreesPerSecondToHertz
- RadiansPerSecondToHertz
- HertzToDegreesPerSecond
- HertzToRadiansPerSecond
- KilopascalsToHectopascals
- HectopascalsToKilopascals
- KilopascalsToPascals
- HectopascalsToPascals
- AtmospheresToPascals
- PascalsToAtmospheres
- CoordinatesToMiles
- CoordinatesToKilometers
- KilogramsToPounds
- PoundsToKilograms
- StonesToPounds
- PoundsToStones

## <a name="api"></a>API

- [단위 변환기 소스 코드](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Types/UnitConverters.shared.cs)
- [단위 변환기 API 설명서](xref:Xamarin.Essentials.UnitConverters)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Unit-Conversion-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
