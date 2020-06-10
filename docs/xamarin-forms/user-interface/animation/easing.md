---
제목: "설명:"의 감속/가속 함수는 애니메이션이 실행 되는 속도를 제어 하는 Xamarin.Forms Xamarin.Forms 방법을 제어 하는 전송 함수를 지정할 수 있도록 하는 감속/가속 클래스를 포함 합니다. 이 문서에서는 미리 정의 된 감속/가속 함수를 사용 하는 방법과 사용자 지정 감속/가속 함수를 만드는 방법에 대해 설명 합니다. "
assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE: xamarin-forms author: davidbritch: dabritch:: 07/14/2016-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="easing-functions-in-xamarinforms"></a>의 감속/가속 함수Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-easing)

_Xamarin.ios에는 애니메이션이 실행 되는 속도를 제어 하는 방법을 제어 하는 전송 함수를 지정할 수 있도록 하는 감속/가속 클래스가 포함 되어 있습니다. 이 문서에서는 미리 정의 된 감속/가속 함수를 사용 하는 방법과 사용자 지정 감속/가속 함수를 만드는 방법을 보여 줍니다._

[`Easing`](xref:Xamarin.Forms.Easing)클래스는 애니메이션에서 사용할 수 있는 여러 감속/가속 함수를 정의 합니다.

- [`BounceIn`](xref:Xamarin.Forms.Easing.BounceIn)감속/가속 함수는 시작 부분에서 애니메이션을 바운스 합니다.
- [`BounceOut`](xref:Xamarin.Forms.Easing.BounceOut)감속/가속 함수는 애니메이션을 끝에 바운스 합니다.
- [`CubicIn`](xref:Xamarin.Forms.Easing.CubicIn)감속/가속 함수는 애니메이션 속도를 느리게 합니다.
- [`CubicInOut`](xref:Xamarin.Forms.Easing.CubicInOut)감속/가속 함수는 시작 부분에서 애니메이션을 가속화 하 고 끝에서 애니메이션을 감속 합니다.
- [`CubicOut`](xref:Xamarin.Forms.Easing.CubicOut)감속/가속 함수는 애니메이션을 신속 하 게 감속 합니다.
- [`Linear`](xref:Xamarin.Forms.Easing.Linear)감속/가속 함수는 일정 한 속도를 사용 하며 기본 감속/가속 함수입니다.
- [`SinIn`](xref:Xamarin.Forms.Easing.SinIn)감속/가속 함수는 애니메이션을 부드럽게 가속화 합니다.
- [`SinInOut`](xref:Xamarin.Forms.Easing.SinInOut)감속/가속 함수는 시작 시 애니메이션을 부드럽게 가속화 하 고 끝에서 애니메이션을 부드럽게 감속 합니다.
- [`SinOut`](xref:Xamarin.Forms.Easing.SinOut)감속/가속 함수는 애니메이션을 부드럽게 감속 합니다.
- [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn)감속/가속 함수는 애니메이션이 매우 빠르게 종료 되도록 합니다.
- [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut)감속/가속 함수는 애니메이션이 끝에 빠르게 감속 되도록 합니다.

`In`및 `Out` 접미사는 감속/가속 함수에서 제공 하는 효과가 애니메이션의 시작, 끝 또는 양쪽 모두에서 눈에 띄게 표시 되는지 여부를 표시 합니다.

또한 사용자 지정 감속/가속 함수를 만들 수 있습니다. 자세한 내용은 [사용자 지정 감속/가속 함수](#custom-easing-functions)를 참조 하세요.

## <a name="consuming-an-easing-function"></a>감속/가속 함수 사용

클래스의 애니메이션 확장 메서드는 [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 다음 코드 예제에서 보여 주는 것 처럼 감속/가속 함수를 최종 메서드 매개 변수로 지정할 수 있습니다.

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

애니메이션에 대 한 감속/가속 함수를 지정 하면 애니메이션 속도는 비선형이 되며 감속/가속 함수에서 제공 하는 효과를 생성 합니다. 애니메이션을 만들 때 감속/가속 함수를 생략 하면 애니메이션이 기본 [`Linear`](xref:Xamarin.Forms.Easing.Linear) 감속/가속 함수를 사용 하 여 선형 속도를 산출 합니다.

클래스에서 애니메이션 확장 메서드를 사용 하는 방법에 대 한 자세한 내용은 [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) [단순 애니메이션](~/xamarin-forms/user-interface/animation/simple.md)을 참조 하세요. 감속/가속 함수는 클래스에서 사용 될 수도 있습니다 [`Animation`](xref:Xamarin.Forms.Animation) . 자세한 내용은 [사용자 지정 애니메이션](~/xamarin-forms/user-interface/animation/custom.md)을 참조 하세요.

## <a name="custom-easing-functions"></a>사용자 지정 감속/가속 함수

사용자 지정 감속/가속 함수를 만드는 세 가지 주요 방법이 있습니다.

1. `double`인수를 사용 하 고 결과를 반환 하는 메서드를 만듭니다 `double` .
1. `Func<double, double>`를 만듭니다.
1. 감속/가속 함수를 생성자에 대 한 인수로 지정 합니다 [`Easing`](xref:Xamarin.Forms.Easing) .

세 경우 모두 사용자 지정 감속/가속 함수는 0의 인수에 대해 0을 반환 하 고 인수 1의 경우 1을 반환 해야 합니다. 그러나 0과 1의 인수 값 사이에는 모든 값이 반환 될 수 있습니다. 이제 각 접근 방식에 대해 설명 하겠습니다.

### <a name="custom-easing-method"></a>사용자 지정 감속/가속 메서드

사용자 지정 감속/가속 함수는 `double` `double` 다음 코드 예제에서 보여 주는 것 처럼 인수를 사용 하는 메서드로 정의 하 고 결과를 반환할 수 있습니다.

```csharp
double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}

await image.TranslateTo(0, 200, 2000, (Easing)CustomEase);
```

`CustomEase`메서드는 들어오는 값을 0, 0.2, 0.4, 0.6, 0.8 및 1 값으로 자릅니다. 따라서 인스턴스는 [`Image`](xref:Xamarin.Forms.Image) 순조롭게 진행 되는 것이 아니라 불연속 점프로 변환 됩니다.

### <a name="custom-easing-func"></a>사용자 지정 감속/가속 함수

`Func<double, double>`다음 코드 예제에서 보여 주는 것 처럼 사용자 지정 감속/가속 함수를로 정의할 수도 있습니다.

```csharp
Func<double, double> CustomEaseFunc = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEaseFunc);
```

는 `CustomEaseFunc` 신속 하 게 시작 하 고, 속도를 높이고, 역순으로 시작 하는 감속/가속 함수를 나타내며, 과정을 신속 하 게 가속화 하기 위해 과정을 다시 반대로 합니다. 따라서 인스턴스를 전반적으로 이동 하는 동안에는 애니메이션을 중간에 진행 하는 [`Image`](xref:Xamarin.Forms.Image) 과정을 일시적으로 취소 합니다.

### <a name="custom-easing-constructor"></a>사용자 지정 감속/가속 생성자

[`Easing`](xref:Xamarin.Forms.Easing)다음 코드 예제에서 보여 주는 것 처럼 사용자 지정 감속/가속 함수를 생성자에 대 한 인수로 정의할 수도 있습니다.

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

사용자 지정 감속/가속 함수는 생성자에 대 한 람다 함수 인수로 지정 되며, 메서드를 사용 하 여 [`Easing`](xref:Xamarin.Forms.Easing) `Math.Cos` 메서드에서 감소 되는지 하는 느리게 놓기 효과를 만듭니다 `Math.Exp` . 따라서 인스턴스는 [`Image`](xref:Xamarin.Forms.Image) 마지막 위치로 삭제 된 것으로 표시 되도록 변환 됩니다.

## <a name="summary"></a>요약

이 문서에서는 미리 정의 된 감속/가속 함수를 사용 하는 방법과 사용자 지정 감속/가속 함수를 만드는 방법을 살펴보았습니다. Xamarin.Forms에는 [`Easing`](xref:Xamarin.Forms.Easing) 애니메이션이 실행 되는 속도를 제어 하는 방법을 제어 하는 전송 함수를 지정 하는 데 사용할 수 있는 클래스가 포함 되어 있습니다.

## <a name="related-links"></a>관련 링크

- [비동기 지원 개요](~/cross-platform/platform/async.md)
- [감속/가속 함수 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-easing)
- [감속/가속](xref:Xamarin.Forms.Easing)
- [ViewExtensions](xref:Xamarin.Forms.ViewExtensions)
