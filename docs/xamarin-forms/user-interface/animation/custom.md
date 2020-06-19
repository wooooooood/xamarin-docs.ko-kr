---
title: 사용자 지정 애니메이션Xamarin.Forms
description: 이 문서에서는 Xamarin.ios 애니메이션 클래스를 사용 하 여 애니메이션을 만들고 취소 하 고, 여러 애니메이션을 동기화 하 고, 기존 애니메이션 메서드에서 애니메이션을 적용 하지 않는 속성에 애니메이션을 적용 하는 사용자 지정 애니메이션을 만드는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 573f18de0d7593d832505eb6bb2b492caea024a1
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84946106"
---
# <a name="custom-animations-in-xamarinforms"></a>사용자 지정 애니메이션Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-custom)

_애니메이션 클래스는 Xamarin.Forms ViewExtensions 클래스의 확장 메서드를 사용 하 여 하나 이상의 애니메이션 개체를 만드는 모든 애니메이션의 빌딩 블록입니다. 이 문서에서는 Animation 클래스를 사용 하 여 애니메이션을 만들고 취소 하 고, 여러 애니메이션을 동기화 하 고, 기존 애니메이션 메서드에서 애니메이션을 적용 하지 않는 속성에 애니메이션을 적용 하는 사용자 지정 애니메이션을 만드는 방법을 보여 줍니다._

`Animation`애니메이션 효과가 적용 되는 속성의 시작 값과 끝 값을 비롯 하 여 개체를 만들 때 여러 매개 변수를 지정 해야 하며, 속성의 값을 변경 하는 콜백입니다. `Animation`개체는 실행 및 동기화 할 수 있는 자식 애니메이션의 컬렉션을 유지할 수도 있습니다. 자세한 내용은 [자식 애니메이션](#child-animations)(영문)을 참조 하세요.

자식 애니메이션이 있을 수도 있고 포함 하지 않을 수 있는 클래스를 사용 하 여 만든 애니메이션을 실행 하는 [`Animation`](xref:Xamarin.Forms.Animation) 것은 [ `Commit` ] (f: Xamarin.Forms . animation. 커밋 ()을 호출 하 여 수행 됩니다 Xamarin.Forms . System.windows.media.animation.ianimatable>, System.string, 시스템. uint32, Xamarin.Forms . 감속/가속, system.string, system.string}, system.xml {system.string}) 메서드를 실행 합니다. 이 메서드는 애니메이션의 기간 및 다른 항목 중에서 애니메이션을 반복할지 여부를 제어 하는 콜백을 지정 합니다.

또한 클래스에는 [`Animation`](xref:Xamarin.Forms.Animation) `IsEnabled` 운영 체제에서 애니메이션을 사용 하지 않도록 설정 했는지 여부를 확인 하기 위해 검사할 수 있는 속성 (예: 전원 절약 모드가 활성화 된 경우)이 있습니다.

## <a name="create-an-animation"></a>애니메이션 만들기

개체를 만들 때 [`Animation`](xref:Xamarin.Forms.Animation) 일반적으로 다음 코드 예제에서 보여 주는 것 처럼 최소 세 개의 매개 변수가 필요 합니다.

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

이 코드는 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) [`Image`](xref:Xamarin.Forms.Image) 값 1에서 값 2로 인스턴스의 속성에 대 한 애니메이션을 정의 합니다. 에서 파생 되는 애니메이션이 적용 된 값은 Xamarin.Forms 첫 번째 인수로 지정 된 콜백에 전달 되며이 값은 속성의 값을 변경 하는 데 사용 됩니다 `Scale` .

애니메이션은 [ `Commit` ] (f Xamarin.Forms ( Xamarin.Forms )를 호출 하 여 시작 됩니다. System.windows.media.animation.ianimatable>, System.string, 시스템. uint32, Xamarin.Forms . 다음 코드 예제에 나와 있는 것 처럼 감속/가속, 시스템 작업 {system.string, system.string}, system.xml {system.string}) 메서드를 실행 합니다.

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

[ `Commit` ] (F: Xamarin.Forms . Animation. 커밋 ( Xamarin.Forms . System.windows.media.animation.ianimatable>, System.string, 시스템. uint32, Xamarin.Forms . 감속/가속, System.web, system.string}, system.xml {system.string}) 메서드는 개체를 반환 하지 않습니다. `Task` 대신 콜백 메서드를 통해 알림이 제공 됩니다.

메서드에 지정 된 인수는 다음과 `Commit` 같습니다.

- 첫 번째 인수 (*owner*)는 애니메이션의 소유자를 식별 합니다. 애니메이션이 적용 되는 시각적 요소 또는 페이지와 같은 다른 시각적 요소가 될 수 있습니다.
- 두 번째 인수 (*이름*)는 이름을 사용 하 여 애니메이션을 식별 합니다. 이름은 소유자와 결합 되어 애니메이션을 고유 하 게 식별 합니다. 그러면이 고유 id를 사용 하 여 애니메이션이 실행 중인지 여부를 확인할 수 있습니다 ([ `AnimationIsRunning` ] (f: Xamarin.Forms ). 애니메이션 확장. 애니메이션 실행 ( Xamarin.Forms . System.windows.media.animation.ianimatable>))을 (를) 만들거나 취소 하려면 ([ `AbortAnimation` ] (f: Xamarin.Forms . AbortAnimation ( Xamarin.Forms . System.windows.media.animation.ianimatable>)).
- 세 번째 인수 (*rate*)는 생성자에 정의 된 콜백 메서드를 호출할 때마다 시간 (밀리초)을 나타냅니다 [`Animation`](xref:Xamarin.Forms.Animation) .
- 네 번째 인수 (*길이*)는 애니메이션의 기간 (밀리초)을 나타냅니다.
- 다섯 번째 인수 (*감속/가속*)는 애니메이션에 사용할 감속/가속 함수를 정의 합니다. 또는 감속/가속 함수를 생성자에 대 한 인수로 지정할 수 있습니다 [`Animation`](xref:Xamarin.Forms.Animation) . 감속/가속 함수에 대 한 자세한 내용은 [감속/가속 함수](~/xamarin-forms/user-interface/animation/easing.md)를 참조 하세요.
- 여섯 번째 인수 (*마침*)는 애니메이션이 완료 될 때 실행 되는 콜백입니다. 이 콜백은 마지막 값을 나타내는 첫 번째 인수를 사용 하 여 두 개의 인수를 사용 하 고 두 번째 인수는 `bool` 애니메이션이 취소 된 경우로 설정 된입니다 `true` . 또는 *완성* 된 콜백을 생성자에 대 한 인수로 지정할 수 있습니다 [`Animation`](xref:Xamarin.Forms.Animation) . 그러나 단일 애니메이션을 사용 하면 생성자와 메서드에서 *완료* 된 콜백이 지정 된 경우 `Animation` `Commit` 메서드에 지정 된 콜백만 실행 됩니다 `Commit` .
- 일곱 번째 인수 (*반복*)는 애니메이션을 반복할 수 있도록 하는 콜백입니다. 애니메이션의 끝에서 호출 되 고를 반환 `true` 하면 애니메이션을 반복 해야 함을 나타냅니다.

전반적인 효과는 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) [`Image`](xref:Xamarin.Forms.Image) [`Linear`](xref:Xamarin.Forms.Easing.Linear) 감속/가속 함수를 사용 하 여 2 초 (2000 밀리초) 이상으로의 속성을 1에서 2로 늘리는 애니메이션을 만드는 것입니다. 애니메이션이 완료 될 때마다 `Scale` 속성이 1로 다시 설정 되 고 애니메이션이 반복 됩니다.

> [!NOTE]
> 서로 독립적으로 실행 되는 동시 애니메이션은 각 `Animation` 애니메이션에 대 한 개체를 만든 다음 `Commit` 각 애니메이션에서 메서드를 호출 하 여 생성할 수 있습니다.

### <a name="child-animations"></a>자식 애니메이션

[`Animation`](xref:Xamarin.Forms.Animation)클래스는 `Animation` 다른 개체가 추가 되는 개체를 만드는 작업을 포함 하는 자식 애니메이션도 지원 `Animation` 합니다. 이렇게 하면 일련의 애니메이션을 실행 하 고 동기화 할 수 있습니다. 다음 코드 예제에서는 자식 애니메이션을 만들고 실행 하는 방법을 보여 줍니다.

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

또는 다음 코드 예제에서 보여 주는 것 처럼 코드 예제를 더 간결 하 게 작성할 수 있습니다.

```csharp
new Animation {
    { 0, 0.5, new Animation (v => image.Scale = v, 1, 2) },
    { 0, 1, new Animation (v => image.Rotation = v, 0, 360) },
    { 0.5, 1, new Animation (v => image.Scale = v, 2, 1) }
    }.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

두 코드 예제 모두에서 부모 [`Animation`](xref:Xamarin.Forms.Animation) 개체를 만들어 추가 `Animation` 개체를 추가 합니다. []에 대 한 처음 두 개의 인수 `Add` (f: Xamarin.Forms . Animation. Add (system.string, System.web, Xamarin.Forms . Animation)) 메서드는 자식 애니메이션을 시작 하 고 완료 하는 시기를 지정 합니다. 인수 값은 0에서 1 사이 여야 하 고 부모 애니메이션 내에서 지정 된 자식 애니메이션이 활성화 되는 상대 기간을 나타냅니다. 따라서이 예제에서는 `scaleUpAnimation` 애니메이션의 처음 절반에 대해 활성화 되 고,는 `scaleDownAnimation` 애니메이션의 두 번째 절반에 대해 활성화 되 고는 `rotateAnimation` 전체 기간 동안 활성화 됩니다.

전반적인 효과는 애니메이션이 4 초 (4000 밀리초) 이상 발생 한다는 것입니다. 는 `scaleUpAnimation` [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 2 초를 초과 하 여 속성에 1에서 2까지 애니메이션 효과를 적용 합니다. `scaleDownAnimation`그런 다음는 2 `Scale` 초를 초과 하 여 2에서 1로 속성에 애니메이션을 적용 합니다. 두 크기 조정 애니메이션이 모두 발생 하지만는 `rotateAnimation` 속성을 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 0에서 360 (4 초) 이상으로 애니메이션 효과를 적용 합니다. 크기 조정 애니메이션도 감속/가속 함수를 사용 합니다. [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn)감속/가속 함수는를 [`Image`](xref:Xamarin.Forms.Image) 더 크게 하기 전에를 축소 하 고, [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut) 감속/가속 함수를 통해가 `Image` 전체 애니메이션의 끝 부분을 향해 실제 크기 보다 작아집니다.

[`Animation`](xref:Xamarin.Forms.Animation)자식 애니메이션을 사용 하는 개체와 다음을 사용 하지 않는 개체 간에는 몇 가지 차이점이 있습니다.

- 자식 애니메이션을 사용할 때 자식 애니메이션에 대 한 *완료* 된 콜백은 자식이 완료 된 시간을 나타내고 메서드에 전달 된 *완료* 된 콜백은 `Commit` 전체 애니메이션이 완료 된 시간을 나타냅니다.
- 자식 애니메이션을 사용할 때 `true` 메서드의 *반복* 콜백에서 반환 해도 `Commit` 애니메이션이 반복 되지 않지만 애니메이션은 새 값 없이 계속 실행 됩니다.
- 메서드에 감속/가속 함수를 포함 하 `Commit` 는 경우 감속/가속 함수는 1 보다 큰 값을 반환 합니다. 애니메이션이 종료 됩니다. 감속/가속 함수에서 0 보다 작은 값을 반환 하는 경우에는 값이 0 고정 됩니다. 0 보다 작거나 1 보다 큰 값을 반환 하는 감속/가속 함수를 사용 하려면 메서드가 아니라 자식 애니메이션 중 하나에 값을 지정 해야 합니다 `Commit` .

클래스에는 [`Animation`](xref:Xamarin.Forms.Animation) [ `WithConcurrent` ] (f:)도 포함 Xamarin.Forms 됩니다. 애니메이션. WithConcurrent ( Xamarin.Forms . Animation, system.string, system.string) 메서드는 부모 개체에 자식 애니메이션을 추가 하는 데 사용할 수 있습니다 `Animation` . 그러나 *begin* 및 *finish* 인수 값은 0에서 1로 제한 되지 않지만 자식 애니메이션 중 범위는 0에서 1 사이에 해당 하는 부분만 활성화 됩니다. 예를 들어, `WithConcurrent` 메서드 호출에서 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 1 ~ 6의 속성을 대상으로 하지만 *begin* 및 *finish* 값이-2 및 3 인 자식 애니메이션을 정의 하는 경우 *시작* 값-2는 값 1에 해당 하 `Scale` 고 *마침* 값 3은 값 6에 해당 합니다 `Scale` . 0에서 1 사이의 값은 애니메이션의 일부를 재생 하지 않으므로 속성에는 `Scale` 3 ~ 6 개 까지만 애니메이션 효과가 적용 됩니다.

## <a name="cancel-an-animation"></a>애니메이션 취소

응용 프로그램은 [ `AbortAnimation` ] (f:)를 호출 하 여 애니메이션을 취소할 수 Xamarin.Forms 있습니다. AbortAnimation ( Xamarin.Forms . System.windows.media.animation.ianimatable>, System.string)) 확장 메서드를 보여 줍니다.

```csharp
this.AbortAnimation ("SimpleAnimation");
```

애니메이션은 애니메이션 소유자와 애니메이션 이름을 조합 하 여 고유 하 게 식별 됩니다. 따라서 애니메이션을 취소 하려면 애니메이션을 실행 하는 데 지정 된 소유자 및 이름을 지정 해야 합니다. 따라서 코드 예제는 `SimpleAnimation` 페이지에서 소유 하는 이라는 애니메이션을 즉시 취소 합니다.

## <a name="create-a-custom-animation"></a>사용자 지정 애니메이션 만들기

지금까지 여기에 표시 된 예제에서는 클래스의 메서드를 사용 하 여 동일한 효과를 얻을 수 있는 애니메이션을 보여 주었습니다 [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) . 그러나 클래스의 장점은 애니메이션 된 [`Animation`](xref:Xamarin.Forms.Animation) 값이 변경 될 때 실행 되는 콜백 메서드에 액세스할 수 있다는 것입니다. 이렇게 하면 콜백에서 원하는 애니메이션을 구현할 수 있습니다. 예를 들어 다음 코드 예제에서는 값을 [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) [`Color`](xref:Xamarin.Forms.Color) [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) 0에서 1 사이의 색상 값으로 설정 하 여 페이지의 속성에 애니메이션 효과를 적용 합니다.

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

결과 애니메이션은 무지개 색을 통해 페이지 배경을 이동 하는 모양을 제공 합니다.

베 지 어 곡선 애니메이션을 포함 하 여 복잡 한 애니메이션을 만드는 방법에 대 한 자세한 예제는를 [사용 하 여 Xamarin.Forms Mobile Apps 만들기 ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)의 [22 장](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)

## <a name="create-a-custom-animation-extension-method"></a>사용자 지정 애니메이션 확장 메서드 만들기

클래스의 확장 메서드는 [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 현재 값에서 지정 된 값으로 속성에 애니메이션을 적용 합니다. 이렇게 하면 예를 들어, `ColorTo` 다음과 같은 이유로 한 값에서 다른 값으로 색에 애니메이션 효과를 주는 데 사용할 수 있는 애니메이션 메서드를 만드는 것이 어렵습니다.

- 클래스에서 정의 하는 유일한 속성은 이며 [`Color`](xref:Xamarin.Forms.Color) [`VisualElement`](xref:Xamarin.Forms.VisualElement) ,이 속성은 [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) 항상 `Color` 애니메이션 효과를 주는 데 필요한 속성이 아닙니다.
- 속성의 현재 값은 일반적으로 [`Color`](xref:Xamarin.Forms.Color) [`Color.Default`](xref:Xamarin.Forms.Color.Default) 실제 색이 아니며 보간 계산에 사용할 수 없는입니다.

이 문제에 대 한 해결 방법은 `ColorTo` 메서드가 특정 속성을 대상으로 하지 않도록 하는 것입니다 [`Color`](xref:Xamarin.Forms.Color) . 대신, 보간된 `Color` 값을 다시 호출자에 게 전달 하는 콜백 메서드를 사용 하 여 작성할 수 있습니다. 또한이 메서드는 시작 및 끝 인수를 사용 `Color` 합니다.

`ColorTo`메서드는 클래스의 메서드를 사용 하 여 [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) 해당 기능을 제공 하는 확장 메서드로 구현 될 수 있습니다 [`AnimationExtensions`](xref:Xamarin.Forms.AnimationExtensions) . 이는 `Animate` `double` 다음 코드 예제에서 보여 주는 것 처럼 메서드를 사용 하 여 형식이 아닌 속성을 대상으로 지정할 수 있기 때문입니다.

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

[`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*)메서드에는 콜백 메서드인 *transform* 인수가 필요 합니다. 이 콜백에 대 한 입력의 범위는 항상 `double` 0에서 1 사이입니다. 따라서 메서드는 `ColorTo` `Func` `double` 0에서 1 사이의 값을 허용 하 고 해당 값에 해당 하는 값을 반환 하는 자체 변환을 정의 [`Color`](xref:Xamarin.Forms.Color) 합니다. `Color`값은 [`R`](xref:Xamarin.Forms.Color.R) 제공 된 [`G`](xref:Xamarin.Forms.Color.G) [`B`](xref:Xamarin.Forms.Color.B) 두 인수의,, 및 [`A`](xref:Xamarin.Forms.Color.A) 값 `Color` 을 보간 하 여 계산 됩니다. `Color`그런 다음이 값은 응용 프로그램의 콜백 메서드에 특정 속성으로 전달 됩니다.

이 접근 방식을 통해 `ColorTo` 메서드는 [`Color`](xref:Xamarin.Forms.Color) 다음 코드 예제에서 보여 주는 것 처럼 모든 속성에 애니메이션 효과를 줄 수 있습니다.

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

이 코드 예제에서 메서드는의 `ColorTo` [`TextColor`](xref:Xamarin.Forms.Label.TextColor) 및 [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) 속성 [`Label`](xref:Xamarin.Forms.Label) , `BackgroundColor` 페이지의 속성 및 [`Color`](xref:Xamarin.Forms.BoxView.Color) 의 속성 [`BoxView`](xref:Xamarin.Forms.BoxView) 에 애니메이션을 적용 합니다.

## <a name="related-links"></a>관련 링크

- [사용자 지정 애니메이션 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-custom)
- [애니메이션 API](xref:Xamarin.Forms.Animation)
- [애니메이션 확장 API](xref:Xamarin.Forms.AnimationExtensions)
