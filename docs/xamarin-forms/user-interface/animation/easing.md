---
title: "감속/가속 함수"
description: "Xamarin.Forms는 애니메이션의 속도 또는 실행 하는 대로 느려질 방법을 제어 하는 전송 함수를 지정할 수 있도록 하는 감속/가속 클래스를 포함 합니다. 이 문서를 사용자 지정 감속/가속 함수를 만드는 방법과 미리 정의 된 감속/가속 함수를 사용 하는 방법을 보여줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: a57fd6e45d744d0e527c811649ce5299ebcd34d5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="easing-functions"></a>감속/가속 함수

_Xamarin.Forms는 애니메이션의 속도 또는 실행 하는 대로 느려질 방법을 제어 하는 전송 함수를 지정할 수 있도록 하는 감속/가속 클래스를 포함 합니다. 이 문서를 사용자 지정 감속/가속 함수를 만드는 방법과 미리 정의 된 감속/가속 함수를 사용 하는 방법을 보여줍니다._


[ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) 애니메이션으로 사용할 수 있는 감속/가속 함수의 수를 정의 하는 클래스:

- [ `BounceIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceIn/) 애니메이션 시작 부분에서 반송 감속/가속 함수입니다.
- [ `BounceOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceOut/) 끝에 애니메이션 반송 감속/가속 함수입니다.
- [ `CubicIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicIn/) 애니메이션의 속도 높여 감속/가속 함수를 느리게 합니다.
- [ `CubicInOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicInOut/) 시작 되 면 애니메이션의 속도 높여 및 끝에 애니메이션 감속 감속/가속 함수입니다.
- [ `CubicOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicOut/) 애니메이션 감속 감속/가속 함수를 신속 하 게 합니다.
- [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) 사용 하 여 일정 한 속도 감속/가속 함수 및은 기본 감속/가속 함수입니다.
- [ `SinIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinIn/) 애니메이션의 속도 높여 감속/가속 함수를 원활 하 게 합니다.
- [ `SinInOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinInOut/) 시작 되 면 애니메이션을 가속화 하 고 끝에 애니메이션을 원활 하 게 감속 감속/가속 함수를 원활 하 게 합니다.
- [ `SinOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinOut/) 애니메이션 감속 감속/가속 함수를 원활 하 게 합니다.
- [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) 애니메이션이 끝쪽에 매우 신속 하 게 신속 하 게 감속/가속 함수입니다.
- [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) 애니메이션이 끝쪽에 감속 신속 하 게 감속/가속 함수입니다.

`In` 및 `Out` 접미사 감속/가속 함수에서 제공한 효과 끝 또는 둘 다에서 애니메이션의 시작 부분에 띄는 인지 나타냅니다.

또한 사용자 지정 감속/가속 함수를 만들 수 있습니다. 자세한 내용은 참조 [사용자 지정 감속/가속 함수](#customeasing)합니다.

## <a name="consuming-an-easing-function"></a>감속/가속 함수를 사용합니다.

애니메이션 확장 메서드는 [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) 다음 코드 예제에서와 같이 클래스 감속/가속 함수를 마지막 메서드 매개 변수로 지정을 허용 합니다.

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

애니메이션에 대 한 감속/가속 함수를 지정 하 여 애니메이션 속도 비선형 되며 감속/가속 함수에서 제공한 효과 생성 합니다. 기본값을 사용 하려면 애니메이션이 애니메이션을 만들 때 감속/가속 함수를 생략 하면 [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) 감속/가속 함수를 선형 속도 생성 합니다.

에 대 한 애니메이션 확장 메서드를 사용 하는 방법에 대 한 자세한 내용은 [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) 클래스를 참조 하십시오. [간단한 애니메이션](~/xamarin-forms/user-interface/animation/simple.md)합니다. 감속/가속 함수도 사용할 수는 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) 클래스입니다. 자세한 내용은 참조 [사용자 지정 애니메이션](~/xamarin-forms/user-interface/animation/custom.md)합니다.

<a name="customeasing" />

## <a name="custom-easing-functions"></a>사용자 지정 감속/가속 함수

사용자 지정 감속/가속 함수를 만드는 데 세 가지 주요 방법이 있습니다.

1. 사용 하는 메서드 만들기는 `double` 인수 및 반환 된 `double` 결과입니다.
1. `Func<double, double>`를 만듭니다.
1. 감속/가속 함수에 대 한 인수로 지정 된 [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) 생성자입니다.

세 가지 경우 모두 사용자 지정 감속/가속 함수는 0과 1의 1의 인수에 대 한 인수에 대 한 0을 반환 해야 합니다. 그러나 인수 값 0과 1 사이의 모든 값을 반환할 수 있습니다. 이제 각 방법은 차례로 설명 합니다.

### <a name="custom-easing-method"></a>사용자 지정 메서드를 감속/가속

사용자 지정 감속/가속 함수를 사용 하는 방법으로 정의할 수 있습니다는 `double` 인수 및 반환 된 `double` 다음 코드 예제에서와 같이 결과:

```csharp
await image.TranslateTo(0, 200, 2000, CustomEase);

double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}
```

`CustomEase` 메서드는 값 0, 0.2, 0.4, 0.6, 0.8, 1을 들어오는 값이 잘립니다. 따라서는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 인스턴스는 개별 점프에 보다는 원활 하 게 변환 됩니다.

### <a name="custom-easing-func"></a>Func 감속/가속 사용자 지정

사용자 지정 감속/가속 함수를 정의할 수도 있습니다는 `Func<double, double>`다음 코드 예제에서와 같이,:

```csharp
Func<double, double> CustomEase = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEase));
```

`CustomEase` `Func` 끝쪽에 신속 하 게을 가속화 하 다시 과정는 속도 느려집니다 및를 반대로 바꿉니다 및 다음 취소로 시작 하는 감속/가속 함수를 나타냅니다. 따라서 전체 이동 하는 동안는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 인스턴스는 아래쪽으로, 애니메이션 중간 부분 과정 또한 일시적으로 되돌립니다.

### <a name="custom-easing-constructor"></a>사용자 지정 생성자를 감속/가속

에 대 한 인수로 사용자 지정 감속/가속 함수를 정의할 수도 있습니다는 [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) 생성자에 다음 코드 예제에서와 같이:

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

사용자 지정 감속/가속 함수에 대 한 람다 함수 인수로 지정 된 [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) 생성자를 사용 하 여는 `Math.Cos` 방법을 사용 하 여이 감소 되는지 저속 놓기 효과 만들 수는 `Math.Exp` 메서드. 따라서는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 인스턴스 최종 도착 위치로 삭제 표시 되도록 변환 됩니다.

## <a name="summary"></a>요약

이 문서를 사용자 지정 감속/가속 함수를 만드는 방법과 미리 정의 된 감속/가속 함수를 사용 하는 방법을 보여 줍니다. Xamarin.Forms에 포함 되어는 [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) 클래스 애니메이션 속도를 향상 하는 방법을 제어 하는 전송 함수를 지정 하거나 실행 하는 대로 속도가 느려질 수 있습니다.



## <a name="related-links"></a>관련 링크

- [비동기 지원 개요](~/cross-platform/platform/async.md)
- [감속/가속 함수 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/easing/)
- [감속/가속](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/)
- [ViewExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)
