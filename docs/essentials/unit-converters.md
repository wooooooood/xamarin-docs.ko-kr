---
title: Xamarin.Essentials 단위 변환기
description: Xamarin.Essentials의 UnitConverters 클래스는 Xamarin.Essentials를 사용할 때 개발자를 돕기 위한 여러 단위 변환기를 제공합니다.
ms.assetid: 35DE2704-E730-4337-9476-66CD53376943
author: jamesmontemagno
ms.author: jamont
ms.date: 01/06/2020
ms.openlocfilehash: c07e0c7d9645c22f0d70c75fd7d8dffdec8cde04
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "77545036"
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

- [단위 변환기 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/UnitConverters.shared.cs)
- [단위 변환기 API 설명서](xref:Xamarin.Essentials.UnitConverters)
