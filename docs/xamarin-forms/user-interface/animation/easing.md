---
title: Xamarin.Forms의 감속/가속 함수
description: Xamarin.Forms는 애니메이션 속도 또는 실행 되는 속도가 저하 하는 방법을 제어 하는 전송 함수를 지정할 수 있도록 하는 감속/가속 클래스를 포함 합니다. 이 문서에는 미리 정의 된 감속/가속 함수를 사용 하는 방법 및 사용자 지정 감속/가속 함수를 만드는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 211f56e0d9f96383670be1d60421d3ac28eabe46
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53051998"
---
# <a name="easing-functions-in-xamarinforms"></a>Xamarin.Forms의 감속/가속 함수

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/easing/)

_Xamarin.Forms는 애니메이션 속도 또는 실행 되는 속도가 저하 하는 방법을 제어 하는 전송 함수를 지정할 수 있도록 하는 감속/가속 클래스를 포함 합니다. 이 문서에는 미리 정의 된 감속/가속 함수를 사용 하는 방법 및 사용자 지정 감속/가속 함수를 만드는 방법을 보여 줍니다._


합니다 [ `Easing` ](xref:Xamarin.Forms.Easing) 클래스는 다양 한 애니메이션에서 사용 될 수 있는 감속/가속 함수를 정의 합니다.

- 합니다 [ `BounceIn` ](xref:Xamarin.Forms.Easing.BounceIn) 애니메이션 시작 부분에서 바운스 감속/가속 함수입니다.
- 합니다 [ `BounceOut` ](xref:Xamarin.Forms.Easing.BounceOut) 끝 애니메이션 바운스 감속/가속 함수입니다.
- 합니다 [ `CubicIn` ](xref:Xamarin.Forms.Easing.CubicIn) 애니메이션 감속 감속/가속 함수를 느리게 합니다.
- 합니다 [ `CubicInOut` ](xref:Xamarin.Forms.Easing.CubicInOut) 시작 되 면 애니메이션을 가속화 하 고 끝에 있는 애니메이션 감속 감속/가속 함수입니다.
- 합니다 [ `CubicOut` ](xref:Xamarin.Forms.Easing.CubicOut) 애니메이션 감속 감속/가속 함수를 신속 하 게 합니다.
- 합니다 [ `Linear` ](xref:Xamarin.Forms.Easing.Linear) 상수 개발 속도 사용 하 여 감속/가속 함수 및는 기본 감속/가속 함수입니다.
- 합니다 [ `SinIn` ](xref:Xamarin.Forms.Easing.SinIn) 애니메이션 감속 감속/가속 함수를 원활 하 게 합니다.
- 합니다 [ `SinInOut` ](xref:Xamarin.Forms.Easing.SinInOut) 시작 되 면 애니메이션을 가속화 하 고 원활 하 게 끝 애니메이션 감속 감속/가속 함수를 원활 하 게 합니다.
- 합니다 [ `SinOut` ](xref:Xamarin.Forms.Easing.SinOut) 애니메이션 감속 감속/가속 함수를 원활 하 게 합니다.
- 합니다 [ `SpringIn` ](xref:Xamarin.Forms.Easing.SpringIn) 을 매우 신속 하 게 끝 가속화 애니메이션이 감속/가속 함수입니다.
- 합니다 [ `SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut) 끝 빠르게 감속을 애니메이션이 감속/가속 함수입니다.

합니다 `In` 고 `Out` 접미사 감속/가속 함수를 제공한 효과 끝 또는 둘 다에서 애니메이션의 시작 부분에 눈에 띄는 인지를 나타냅니다.

또한 사용자 지정 감속/가속 함수를 만들 수 있습니다. 자세한 내용은 [사용자 지정 감속/가속 함수](#customeasing)합니다.

## <a name="consuming-an-easing-function"></a>감속/가속 함수를 사용합니다.

애니메이션 확장 메서드는 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) 클래스는 다음 코드 예제에서 설명한 것 처럼 최종 메서드 매개 변수로 지정 감속/가속 함수를 허용 합니다.

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

애니메이션에 감속/가속 함수를 지정 하 여 애니메이션 속도 비선형 되며 감속/가속 함수를 제공한 효과 생성 합니다. 기본값을 사용 하려면 애니메이션이 애니메이션을 만들 때에 감속/가속 함수를 생략 [ `Linear` ](xref:Xamarin.Forms.Easing.Linear) 감속/가속 함수는 선형 속도 생성 합니다.

애니메이션 확장 메서드를 사용 하는 방법에 대 한 자세한 내용은 합니다 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) 클래스를 참조 하십시오 [간단한 애니메이션](~/xamarin-forms/user-interface/animation/simple.md)합니다. 감속/가속 함수 또한 에서도 사용할 수 있습니다 합니다 [ `Animation` ](xref:Xamarin.Forms.Animation) 클래스입니다. 자세한 내용은 [사용자 지정 애니메이션](~/xamarin-forms/user-interface/animation/custom.md)합니다.

<a name="customeasing" />

## <a name="custom-easing-functions"></a>사용자 지정 감속/가속 함수

가지 사용자 지정 감속/가속 함수를 만드는 세 가지 주요 방법이 있습니다.

1. 사용 하는 메서드 만들기는 `double` 인수 및 반환을 `double` 결과입니다.
1. `Func<double, double>`를 만듭니다.
1. 감속/가속 함수에 대 한 인수로 지정 된 [ `Easing` ](xref:Xamarin.Forms.Easing) 생성자입니다.

세 가지 경우 모두 사용자 지정 감속/가속 함수는 인수 1에 대 한 0과 1의 인수에 대해 0을 반환 해야 합니다. 그러나 인수 값은 0과 1 사이의 모든 값을 반환할 수 있습니다. 이제 각 방법은 차례로 설명 합니다.

### <a name="custom-easing-method"></a>사용자 지정 메서드를 감속/가속

사용자 지정 감속/가속 함수를 사용 하는 방법으로 정의할 수 있습니다는 `double` 인수 및 반환을 `double` 다음 코드 예제에서 설명한 것 처럼 결과:

```csharp
await image.TranslateTo(0, 200, 2000, CustomEase);

double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}
```

`CustomEase` 메서드 0, 0.2, 0.4, 0.6, 0.8을 및 1 값을 들어오는 값이 잘립니다. 따라서 합니다 [ `Image` ](xref:Xamarin.Forms.Image) 인스턴스는 개별 점프, 보다 원활 하 게 변환 됩니다.

### <a name="custom-easing-func"></a>사용자 지정 Func 감속/가속

사용자 지정 감속/가속 함수를 정의할 수도 있습니다는 `Func<double, double>`다음 코드 예제에서 설명한 것 처럼:

```csharp
Func<double, double> CustomEase = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEase));
```

합니다 `CustomEase` `Func` 끝 신속 하 게 가속화를 다시 코스는 속도가 빠르고 및 과정을 반대로 및 취소 한 다음 시작 되는 감속/가속 함수를 나타냅니다. 따라서 전체 이동 하는 동안 합니다 [ `Image` ](xref:Xamarin.Forms.Image) 인스턴스가 아래쪽으로, 애니메이션 중간 과정을 일시적으로 되돌립니다.

### <a name="custom-easing-constructor"></a>사용자 지정 생성자를 감속/가속

에 대 한 인수로 사용자 지정 감속/가속 함수를 정의할 수도 있습니다는 [ `Easing` ](xref:Xamarin.Forms.Easing) 다음 코드 예제 에서처럼 생성자:

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

사용자 지정 감속/가속 함수에 람다 함수 인수로 지정 되는 [ `Easing` ](xref:Xamarin.Forms.Easing) 생성자를 사용 하 여를 `Math.Cos` 하 여이 감소 되는지 저속 놓기 효과 만드는 방법의 `Math.Exp` 메서드. 따라서 합니다 [ `Image` ](xref:Xamarin.Forms.Image) 인스턴스는 해당 최종 도착 위치로 삭제 표시 되도록 변환 됩니다.

## <a name="summary"></a>요약

이 문서에는 미리 정의 된 감속/가속 함수를 사용 하는 방법 및 사용자 지정 감속/가속 함수를 만드는 방법을 보여 줍니다. Xamarin.Forms에는 [ `Easing` ](xref:Xamarin.Forms.Easing) 애니메이션 속도를 향상 하는 방법을 제어 하는 전송 함수를 지정 하거나 실행 중인으로 느려질 수 있는 클래스입니다.



## <a name="related-links"></a>관련 링크

- [비동기 지원 개요](~/cross-platform/platform/async.md)
- [감속/가속 함수 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/easing/)
- [감속/가속](xref:Xamarin.Forms.Easing)
- [ViewExtensions](xref:Xamarin.Forms.ViewExtensions)
