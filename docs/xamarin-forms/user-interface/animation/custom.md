---
title: Xamarin.Forms에서 사용자 지정 애니메이션
description: 이 문서에서는 Xamarin.FOrms 애니메이션 클래스를 사용 하 여 만드는 애니메이션을 취소 하 고 여러 애니메이션을 동기화 하 고 속성은 기존 애니메이션 방법을 사용 하 여 애니메이션을 애니메이션을 적용 하는 사용자 지정 애니메이션을 만드는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2019
ms.openlocfilehash: 405d7990b622b890aa3d66bd632662f086441666
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635759"
---
# <a name="custom-animations-in-xamarinforms"></a>Xamarin.Forms에서 사용자 지정 애니메이션

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-custom)

_애니메이션 클래스는 하나 이상의 애니메이션 개체를 만드는 ViewExtensions 클래스의 확장 메서드를 사용 하 여 모든 Xamarin.ios 애니메이션의 빌딩 블록입니다. 이 문서에서는 Animation 클래스를 사용 하 여 애니메이션을 만들고 취소 하 고, 여러 애니메이션을 동기화 하 고, 기존 애니메이션 메서드에서 애니메이션을 적용 하지 않는 속성에 애니메이션을 적용 하는 사용자 지정 애니메이션을 만드는 방법을 보여 줍니다._

애니메이션 효과가 적용 되는 속성의 시작 값과 끝 값을 비롯 하 여 `Animation` 개체를 만들 때 매개 변수를 몇 가지 지정 해야 하며, 속성의 값을 변경 하는 콜백입니다. `Animation` 개체는 실행 및 동기화 할 수 있는 자식 애니메이션의 컬렉션을 유지할 수도 있습니다. 자세한 내용은 [자식 애니메이션](#child)(영문)을 참조 하세요.

자식 애니메이션이 있을 수도 있고 포함 하지 않을 수 있는 [`Animation`](xref:Xamarin.Forms.Animation) 클래스를 사용 하 여 만든 애니메이션을 실행 하는 것은 [`Commit`](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) 메서드를 호출 하 여 수행 됩니다. 이 메서드는 기간 애니메이션 및 기타 항목 간에 애니메이션을 반복할지 여부를 제어 하는 콜백을 지정 합니다.

또한 [`Animation`](xref:Xamarin.Forms.Animation) 클래스에는 절전 모드를 활성화 한 경우와 같이 운영 체제에서 애니메이션을 사용 하지 않도록 설정 했는지 여부를 확인 하기 위해 검사할 수 있는 `IsEnabled` 속성이 있습니다.

## <a name="create-an-animation"></a>애니메이션 만들기

[`Animation`](xref:Xamarin.Forms.Animation) 개체를 만들 때 일반적으로 다음 코드 예제에서 설명한 것 처럼 최소 세 개의 매개 변수가 필요 합니다.

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

이 코드는 값 1에서 값 2로 [`Image`](xref:Xamarin.Forms.Image) 인스턴스의 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성에 대 한 애니메이션을 정의 합니다. Xamarin.ios에서 파생 되는 애니메이션이 적용 된 값은 첫 번째 인수로 지정 된 콜백에 전달 되며,이 값은 `Scale` 속성의 값을 변경 하는 데 사용 됩니다.

애니메이션은 다음 코드 예제에서 보여 주는 것 처럼 [`Commit`](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) 메서드를 호출 하 여 시작 됩니다.

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

[`Commit`](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) 메서드는 `Task` 개체를 반환 하지 않습니다. 대신, 알림 콜백 메서드를 통해 제공 됩니다.

다음 인수는 `Commit` 메서드에서 지정 됩니다.

- 첫 번째 인수 (*owner*)는 애니메이션의 소유자를 식별 합니다. 이 애니메이션이 적용 되는 시각적 요소 또는 페이지와 같은 다른 시각적 요소 수 있습니다.
- 두 번째 인수 (*이름*)는 이름을 사용 하 여 애니메이션을 식별 합니다. 애니메이션을 고유 하 게 식별 하기 위해 소유자 이름을 결합 됩니다. 그런 다음이 고유 id를 사용 하 여 애니메이션이 실행 중인지 ([`AnimationIsRunning`](xref:Xamarin.Forms.AnimationExtensions.AnimationIsRunning(Xamarin.Forms.IAnimatable,System.String))) 아니면 취소 ([`AbortAnimation`](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String)))를 확인할 수 있습니다.
- 세 번째 인수 (*rate*)는 [`Animation`](xref:Xamarin.Forms.Animation) 생성자에 정의 된 콜백 메서드를 호출할 때마다 시간 (밀리초)을 나타냅니다.
- 네 번째 인수 (*길이*)는 애니메이션의 기간 (밀리초)을 나타냅니다.
- 다섯 번째 인수 (*감속/가속*)는 애니메이션에 사용할 감속/가속 함수를 정의 합니다. 또는 감속/가속 함수를 [`Animation`](xref:Xamarin.Forms.Animation) 생성자에 대 한 인수로 지정할 수 있습니다. 감속/가속 함수에 대 한 자세한 내용은 [감속/가속 함수](~/xamarin-forms/user-interface/animation/easing.md)를 참조 하세요.
- 여섯 번째 인수 (*마침*)는 애니메이션이 완료 될 때 실행 되는 콜백입니다. 이 콜백은 마지막 값을 나타내는 첫 번째 인수를 사용 하 여 두 개의 인수를 사용 하 고 두 번째 인수는 애니메이션이 취소 된 경우 `true`로 설정 된 `bool` 됩니다. 또는 *완성* 된 콜백을 [`Animation`](xref:Xamarin.Forms.Animation) 생성자에 대 한 인수로 지정할 수 있습니다. 그러나 단일 애니메이션을 사용 하면 `Animation` 생성자와 `Commit` 메서드에서 *완료* 된 콜백이 지정 된 경우 `Commit` 메서드에 지정 된 콜백만 실행 됩니다.
- 일곱 번째 인수 (*반복*)는 애니메이션을 반복할 수 있도록 하는 콜백입니다. 애니메이션의 끝에서 호출 되며 `true`를 반환 하면 애니메이션을 반복 해야 함을 나타냅니다.

전반적인 효과는 [`Linear`](xref:Xamarin.Forms.Easing.Linear) 감속/가속 함수를 사용 하 여 [`Image`](xref:Xamarin.Forms.Image) 의 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성을 2 초 (2000 밀리초) 이상으로 늘리는 애니메이션을 만드는 것입니다. 애니메이션이 완료 될 때마다 해당 `Scale` 속성이 1로 다시 설정 되 고 애니메이션이 반복 됩니다.

> [!NOTE]
> 서로 독립적으로 실행 되는 동시 애니메이션은 각 애니메이션에 대 한 `Animation` 개체를 만든 다음 각 애니메이션에서 `Commit` 메서드를 호출 하 여 생성할 수 있습니다.

<a name="child" />

### <a name="child-animations"></a>자식 애니메이션

[`Animation`](xref:Xamarin.Forms.Animation) 클래스는 다른 `Animation` 개체가 추가 되는 `Animation` 개체를 만드는 작업을 포함 하는 자식 애니메이션도 지원 합니다. 이 통해 일련의 애니메이션을 실행 하 고 동기화 합니다. 다음 코드 예제에서는 만들고 자식 애니메이션을 실행 하는 방법을 보여 줍니다.

```csharp
var parentAnimation = new Animation ();
var scaleUpAnimation = new Animation (v => image.Scale = v, 1, 2, Easing.SpringIn);
var rotateAnimation = new Animation (v => image.Rotation = v, 0, 360);
var scaleDownAnimation = new Animation (v => image.Scale = v, 2, 1, Easing.SpringOut);

parentAnimation.Add (0, 0.5, scaleUpAnimation);
parentAnimation.Add (0, 1, rotateAnimation);
parentAnimation.Add (0.5, 1, scaleDownAnimation);

parentAnimation.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

또는 코드 예제는 다음 코드 예제에서 설명한 것 처럼 더 간결 작성할 수 있습니다.

```csharp
new Animation {
    { 0, 0.5, new Animation (v => image.Scale = v, 1, 2) },
    { 0, 1, new Animation (v => image.Rotation = v, 0, 360) },
    { 0.5, 1, new Animation (v => image.Scale = v, 2, 1) }
    }.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

두 코드 예제에서는 부모 [`Animation`](xref:Xamarin.Forms.Animation) 개체가 생성 된 후 추가 `Animation` 개체가 추가 됩니다. [`Add`](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)) 메서드에 대 한 처음 두 인수는 자식 애니메이션을 시작 하 고 완료 하는 시기를 지정 합니다. 인수 값 0과 1 사이 여야 하 고 지정 된 자식 애니메이션 active 되도록 부모 애니메이션 내의 상대 기간을 나타냅니다. 따라서이 예제에서 `scaleUpAnimation`는 애니메이션의 처음 절반에 대해 활성화 되 고 `scaleDownAnimation`는 애니메이션의 두 번째 절반에 대해 활성화 되 고 `rotateAnimation`는 전체 기간 동안 활성화 됩니다.

이 따라 애니메이션이 4 초 (4000 밀리초) 이상 발생 하는 경우 `scaleUpAnimation`은 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성에 1 ~ 2 (2 초) 동안 애니메이션 효과를 적용 합니다. 그런 다음 `scaleDownAnimation`는 2 초를 초과 하 여 2에서 1로 `Scale` 속성에 애니메이션을 적용 합니다. 두 크기 조정 애니메이션이 모두 발생 하는 동안 `rotateAnimation`는 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 속성을 4 초 이상 0에서 360으로 애니메이션 효과를 적용 합니다. 크기 조정 애니메이션 감속/가속 함수를 사용 하는 참고 합니다. [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) 감속/가속 함수를 통해 [`Image`](xref:Xamarin.Forms.Image) 를 축소 하기 전에 먼저 축소 하 고, [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut) 감속/가속 함수를 통해 `Image` 실제 크기 보다 작아 전체 애니메이션의 끝 부분을 향해 축소 됩니다.

자식 애니메이션을 사용 하는 [`Animation`](xref:Xamarin.Forms.Animation) 개체와 다음을 사용 하지 않는 개체 간에는 몇 가지 차이점이 있습니다.

- 자식 애니메이션을 사용할 때 자식 애니메이션에 대 한 *완료* 된 콜백은 자식이 완료 된 시기를 나타내고, `Commit` 메서드에 전달 된 *완료* 된 콜백은 전체 애니메이션이 완료 된 시간을 나타냅니다.
- 자식 애니메이션을 사용 하는 경우 `Commit` 메서드의 *반복* 콜백에서 `true`을 반환 해도 애니메이션이 반복 되지 않지만 애니메이션은 새 값 없이 계속 실행 됩니다.
- `Commit` 메서드에 감속/가속 함수를 포함 하는 경우 감속/가속 함수는 1 보다 큰 값을 반환 합니다. 애니메이션이 종료 됩니다. 감속/가속 함수 반환 값이 0 보다 작은 경우 값은 0으로 고정 됩니다. 0 보다 작거나 1 보다 큰 값을 반환 하는 감속/가속 함수를 사용 하려면 `Commit` 메서드가 아니라 자식 애니메이션 중 하나에 값을 지정 해야 합니다.

[`Animation`](xref:Xamarin.Forms.Animation) 클래스에는 부모 `Animation` 개체에 자식 애니메이션을 추가 하는 데 사용할 수 있는 [`WithConcurrent`](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double)) 메서드도 포함 되어 있습니다. 그러나 *begin* 및 *finish* 인수 값은 0에서 1로 제한 되지 않지만 자식 애니메이션 중 범위는 0에서 1 사이에 해당 하는 부분만 활성화 됩니다. 예를 들어 `WithConcurrent` 메서드 호출에서 1 ~ 6 사이의 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성을 대상으로 하지만 *begin* 및 *finish* 값이-2 및 3 인 자식 애니메이션을 정의 하는 경우 *시작* 값-2는 `Scale` 값 1에 해당 하 고 *, 끝 값 3은 `Scale`* 값 6에 해당 합니다. 0에서 1 사이의 값 범위 밖의 값이 애니메이션의 일부를 재생 하지 않으므로 `Scale` 속성은 3에서 6 까지만 애니메이션을 적용 합니다.

## <a name="cancel-an-animation"></a>애니메이션 취소

응용 프로그램은 다음 코드 예제에서 보여 주는 것 처럼 [`AbortAnimation`](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String)) 확장 메서드를 호출 하 여 애니메이션을 취소할 수 있습니다.

```csharp
this.AbortAnimation ("SimpleAnimation");
```

애니메이션 애니메이션 소유자 및 애니메이션 이름 조합으로 고유 하 게 식별 하는 참고 합니다. 따라서 이름과 소유자 때 애니메이션을 실행 하려면 반드시 지정 해야 애니메이션 취소 지정 합니다. 따라서 코드 예제는 페이지에서 소유 하는 `SimpleAnimation` 라는 애니메이션을 즉시 취소 합니다.

## <a name="create-a-custom-animation"></a>사용자 지정 애니메이션 만들기

지금까지 여기에 표시 된 예제에서는 [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 클래스의 메서드를 사용 하 여 동일 하 게 달성할 수 있는 애니메이션을 보여 주었습니다. 그러나 [`Animation`](xref:Xamarin.Forms.Animation) 클래스의 장점은 애니메이션 된 값이 변경 될 때 실행 되는 콜백 메서드에 액세스할 수 있다는 것입니다. 이렇게 하면 원하는 모든 애니메이션을 구현 하는 콜백에 수 있습니다. 예를 들어 다음 코드 예제에서는 0에서 1 사이의 색상 값을 사용 하 여 [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) 메서드에서 만든 [`Color`](xref:Xamarin.Forms.Color) 값으로 설정 하 여 페이지의 [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) 속성에 애니메이션 효과를 적용 합니다.

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

결과 애니메이션 무지개 색을 통해 페이지 배경을 이동의 모양을 제공 합니다.

베 지 어 곡선 애니메이션을 포함 하 여 복잡 한 애니메이션을 만드는 방법에 대 한 자세한 예제는 [xamarin.ios를 사용 하 여 Mobile Apps 만들기](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)의 [22 장](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf) 을 참조 하세요.

## <a name="create-a-custom-animation-extension-method"></a>사용자 지정 애니메이션 확장 메서드 만들기

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 클래스의 확장 메서드는 현재 값에서 지정 된 값으로 속성에 애니메이션을 적용 합니다. 이렇게 하면 예를 들어 다음과 같은 이유로 한 값에서 다른 값으로 색에 애니메이션 효과를 주는 데 사용할 수 있는 `ColorTo` 애니메이션 메서드를 만드는 것이 어렵습니다.

- [`VisualElement`](xref:Xamarin.Forms.VisualElement) 클래스에서 정의 되는 유일한 [`Color`](xref:Xamarin.Forms.Color) 속성은 [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)이며이는 항상 원하는 `Color` 속성이 적용 되지 않습니다.
- [`Color`](xref:Xamarin.Forms.Color) 속성의 현재 값이 실제 색이 아니며 보간 계산에 사용할 수 없는 [`Color.Default`](xref:Xamarin.Forms.Color.Default)되는 경우가 많습니다.

이 문제에 대 한 해결 방법은 `ColorTo` 메서드가 특정 [`Color`](xref:Xamarin.Forms.Color) 속성을 대상으로 하지 않도록 하는 것입니다. 대신 보간된 `Color` 값을 다시 호출자에 게 전달 하는 콜백 메서드를 사용 하 여 작성할 수 있습니다. 또한 메서드는 시작 및 끝 `Color` 인수를 사용 합니다.

`ColorTo` 메서드는 [`AnimationExtensions`](xref:Xamarin.Forms.AnimationExtensions) 클래스의 [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) 메서드를 사용 하 여 해당 기능을 제공 하는 확장 메서드로 구현 될 수 있습니다. 이는 다음 코드 예제에서 보여 주는 것 처럼 `Animate` 메서드를 사용 하 여 `double`형식이 아닌 속성을 대상으로 지정할 수 있기 때문입니다.

```csharp
public static class ViewExtensions
{
  public static Task<bool> ColorTo(this VisualElement self, Color fromColor, Color toColor, Action<Color> callback, uint length = 250, Easing easing = null)
  {
    Func<double, Color> transform = (t) =>
      Color.FromRgba(fromColor.R + t * (toColor.R - fromColor.R),
                     fromColor.G + t * (toColor.G - fromColor.G),
                     fromColor.B + t * (toColor.B - fromColor.B),
                     fromColor.A + t * (toColor.A - fromColor.A));
    return ColorAnimation(self, "ColorTo", transform, callback, length, easing);
  }

  public static void CancelAnimation(this VisualElement self)
  {
    self.AbortAnimation("ColorTo");
  }

  static Task<bool> ColorAnimation(VisualElement element, string name, Func<double, Color> transform, Action<Color> callback, uint length, Easing easing)
  {
    easing = easing ?? Easing.Linear;
    var taskCompletionSource = new TaskCompletionSource<bool>();

    element.Animate<Color>(name, transform, callback, 16, length, easing, (v, c) => taskCompletionSource.SetResult(c));
    return taskCompletionSource.Task;
  }
}
```

[`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) 메서드에는 콜백 메서드인 *transform* 인수가 필요 합니다. 이 콜백에 대 한 입력은 항상 0에서 1 사이의 `double`입니다. 따라서 `ColorTo` 메서드는 0에서 1 사이의 `double`을 허용 하 고 해당 값에 해당 하는 [`Color`](xref:Xamarin.Forms.Color) 값을 반환 하는 자체 변환 `Func`을 정의 합니다. `Color` 값은 제공 된 두`A`인수의 [`R`](xref:Xamarin.Forms.Color.R), [`G`](xref:Xamarin.Forms.Color.G), [`B`](xref:Xamarin.Forms.Color.B)및 [`Color`](xref:Xamarin.Forms.Color.A) 값을 보간 하 여 계산 됩니다. 그런 다음 `Color` 값은 응용 프로그램의 콜백 메서드에 특정 속성으로 전달 됩니다.

이 방법을 사용 하면 다음 코드 예제에 나와 있는 것 처럼 `ColorTo` 메서드에서 [`Color`](xref:Xamarin.Forms.Color) 속성에 애니메이션 효과를 줄 수 있습니다.

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

이 코드 예제에서 `ColorTo` 메서드는 [`Label`](xref:Xamarin.Forms.Label)의 [`TextColor`](xref:Xamarin.Forms.Label.TextColor) 및 [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) 속성, 페이지의 `BackgroundColor` 속성 및`Color`의 [`BoxView`](xref:Xamarin.Forms.BoxView.Color) 속성에 애니메이션을 적용 [합니다. ](xref:Xamarin.Forms.BoxView)

## <a name="related-links"></a>관련 링크

- [사용자 지정 애니메이션 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-custom)
- [애니메이션 API](xref:Xamarin.Forms.Animation)
- [애니메이션 확장 API](xref:Xamarin.Forms.AnimationExtensions)
