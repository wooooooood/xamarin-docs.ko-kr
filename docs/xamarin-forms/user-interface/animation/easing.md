---
title: Xamarin.Forms의 감속/가속 함수
description: Xamarin.Forms는 애니메이션의 속도가 빨라지거나 느려지는 방식을 제어하는 전달 함수를 지정할 수 있는 Easing 클래스가 포함되어 있습니다. 이 문서는 미리 정의된 감속/가속 함수를 사용하는 방법 및 사용자 지정 감속/가속 함수를 만드는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 211f56e0d9f96383670be1d60421d3ac28eabe46
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61345576"
---
# <a name="easing-functions-in-xamarinforms"></a>Xamarin.Forms의 감속/가속 함수

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/easing/)

_Xamarin.Forms는 애니메이션의 속도가 빨라지거나 느려지는 방식을 제어하는 전달 함수를 지정할 수 있는 Easing 클래스가 포함되어 있습니다. 이 문서는 미리 정의된 감속/가속 함수를 사용하는 방법 및 사용자 지정 감속/가속 함수를 만드는 방법을 보여줍니다._


[`Easing`](xref:Xamarin.Forms.Easing) 클래스는 애니메이션에서 사용할 수 있는 다양한 감속/가속 함수를 제공합니다.

- [`BounceIn`](xref:Xamarin.Forms.Easing.BounceIn) 감속/가속 함수는 애니메이션 시작 부분에서 바운스합니다.
- [`BounceOut`](xref:Xamarin.Forms.Easing.BounceOut) 감속/가속 함수는 애니메이션 끝 부분에서 바운스합니다.
- [`CubicIn`](xref:Xamarin.Forms.Easing.CubicIn) 감속/가속 함수는 애니메이션을 천천히 가속합니다.
- [`CubicInOut`](xref:Xamarin.Forms.Easing.CubicInOut) 감속/가속 함수는 애니메이션 시작 부분에서는 가속하고 끝부분에서는 감속합니다.
- [`CubicOut`](xref:Xamarin.Forms.Easing.CubicOut) 감속/가속 함수는 빠르게 감속합니다.
- [`Linear`](xref:Xamarin.Forms.Easing.Linear) 감속/가속 함수는 일정한 속도로 애니메이션이 실행되는 기본 감속/가속 함수입니다.
- [`SinIn`](xref:Xamarin.Forms.Easing.SinIn) 감속/가속 함수는 애니메이션을 매끄럽게 가속합니다.
- [`SinInOut`](xref:Xamarin.Forms.Easing.SinInOut) 감속/가속 함수는 애니메이션이 시작할 때 매끄럽게 가속하고 끝날 때 매끄럽게 감속합니다.
- [`SinOut`](xref:Xamarin.Forms.Easing.SinOut) 감속/가속 함수는 애니메이션을 매끄럽게 감속합니다.
- [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) 감속/가속 함수는 끝 부분에서 매우 빠르게 가속합니다.
- [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut) 감속/가속 함수는 끝 부분에서 매우 빠르게 감속합니다.

`In`과 `Out` 접미사는 감속/가속 함수가 애니메이션의 시작 부분에서 효과가 있는지, 아니면 끝 부분이나 둘 다에서 효과가 있는지를 나타냅니다.

또한 사용자 지정 감속/가속 함수도 만들 수 있습니다. 자세한 내용은 [사용자 지정 감속/가속 함수](#customeasing)를 참조합니다.

## <a name="consuming-an-easing-function"></a>감속/가속 함수 사용

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 클래스의 애니메이션 확장 메서드는 다음 코드 예제에서 설명한 것처럼 메서드의 마지막 매개 변수에서 감속/가속 함수를 지정할 수 있습니다.

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

애니메이션에 감속/가속 함수를 지정하면 애니메이션 속도가 비선형이 되면서 효과가 만들어집니다. 애니메이션을 생성할 때 감속/가속 함수를 생략하면 선형 속도로 애니메이션을 생성하는 기본 [`Linear`](xref:Xamarin.Forms.Easing.Linear) 감속/가속 함수를 사용합니다.

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 클래스의 확장 메서드를 사용하는 방법에 대한 더 자세한 정보는 [간단한 애니메이션](~/xamarin-forms/user-interface/animation/simple.md)을 참조합니다. 감속/가속 함수는 또한 [`Animation`](xref:Xamarin.Forms.Animation) 클래스에서도 사용할 수 있습니다. 자세한 내용은 [사용자 지정 애니메이션](~/xamarin-forms/user-interface/animation/custom.md)을 참조합니다.

<a name="customeasing" />

## <a name="custom-easing-functions"></a>사용자 지정 감속/가속 함수

사용자 지정 감속/가속 함수를 만들 때 주로 사용하는 세 가지 방법이 있습니다.

1. `double`을 인수로 사용하고 반환을 `double`로 하는 메서드를 만듭니다.
1. `Func<double, double>`를 만듭니다.
1. [`Easing`](xref:Xamarin.Forms.Easing) 생성자의 인수에 감속/가속 함수를 지정합니다.

이 세 가지 경우 모두 사용자 지정 감속/가속 함수는 인수가 0인 경우에는 0을, 인수가 1인 경우에는 1을 반환해야 합니다. 그렇지만 인수가 0과 1 사이이면 어떤 값도 반환할 수 있습니다. 각 방법에 대해 차례로 설명합니다.

### <a name="custom-easing-method"></a>사용자 지정 감속/가속 메서드

다음 코드 예제에서 설명하는 것처럼 사용자 지정 감속/가속 함수는 인수를 `double`로 받고 `double`로 반환하여 정의할 수 있습니다.

```csharp
await image.TranslateTo(0, 200, 2000, CustomEase);

double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}
```

`CustomEase` 메서드는 들어오는 값을 0, 0.2, 0.4, 0.6, 0.8 및 1로 자릅니다. 따라서 [`Image`](xref:Xamarin.Forms.Image) 인스턴스는 부드럽게 움직이지 않고 불연속적으로 움직입니다.

### <a name="custom-easing-func"></a>사용자 지정 가속/감속 Func

사용자 지정 감속/가속 함수는 다음 코드 예제에서 설명한 것처럼 `Func<double, double>`을 사용해 정의할 수 있습니다.

```csharp
Func<double, double> CustomEase = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEase));
```

`CustomEase` `Func`는 빠르게 시작하고 감속하면서 코스가 역전된 다음 다시 역전되어 끝까지 빠르게 향하는 감속/가속 함수를 나타냅니다. 따라서 [`Image`](xref:Xamarin.Forms.Image) 인스턴스의 전체적인 동작은 아래쪽을 향하며 애니메이션 도중 일시적으로 코스를 절반 정도 돌립니다.

### <a name="custom-easing-constructor"></a>사용자 지정 Easing 생성자

다음 코드 예제처럼 사용자 지정 감속/가속 함수는 [`Easing`](xref:Xamarin.Forms.Easing) 생성자를 인수로 전달하여 정의할 수 있습니다.

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

이 사용자 지정 감속/가속 함수는 [`Easing`](xref:Xamarin.Forms.Easing) 생성자에 람다 함수를 인수로 사용하여 정의했고, `Math.Cos` 메서드를 사용하여 `Math.Exp` 메서드에 의해 저감되는 드롭 효과를 만듭니다. 그리하여 [`Image`](xref:Xamarin.Forms.Image) 인스턴스는 최종 도착 위치로 차분히 떨어지면서 이동합니다.

## <a name="summary"></a>요약

이 문서에서는 미리 정의 된 감속/가속 함수를 사용하는 방법 및 사용자 지정 감속/가속 함수를 만드는 방법을 살펴보았습니다. Xamarin.Forms에는 애니메이션의 속도가 올라가거나 느려지는 방식을 제어하는 전달 함수를 지정할 수 있는 [`Easing`](xref:Xamarin.Forms.Easing) 클래스가 포함되어 있습니다.



## <a name="related-links"></a>관련 링크

- [비동기 지원 개요](~/cross-platform/platform/async.md)
- [감속/가속 함수 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/easing/)
- [감속/가속](xref:Xamarin.Forms.Easing)
- [ViewExtensions](xref:Xamarin.Forms.ViewExtensions)
