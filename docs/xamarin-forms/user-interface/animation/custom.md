---
title: 사용자 지정 애니메이션
description: 애니메이션 클래스에는 하나 이상의 애니메이션 개체를 만드는 ViewExtensions 클래스의 확장 방법 사용 하 여 모든 Xamarin.Forms 애니메이션의 구성 요소입니다. 이 문서를 만들기 및 애니메이션을 취소 하 고, 여러 애니메이션 동기화 애니메이션 클래스를 사용 하 고 속성은 기존 애니메이션 메서드에서 애니메이션을 효과 적용 하는 사용자 지정 애니메이션을 만드는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 302aa784baad9afb703f88dcfba56b68fd3c9105
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="custom-animations"></a>사용자 지정 애니메이션

_애니메이션 클래스에는 하나 이상의 애니메이션 개체를 만드는 ViewExtensions 클래스의 확장 방법 사용 하 여 모든 Xamarin.Forms 애니메이션의 구성 요소입니다. 이 문서를 만들기 및 애니메이션을 취소 하 고, 여러 애니메이션 동기화 애니메이션 클래스를 사용 하 고 속성은 기존 애니메이션 메서드에서 애니메이션을 효과 적용 하는 사용자 지정 애니메이션을 만드는 방법을 보여 줍니다._


여러 매개 변수를 만들 때 지정 해야 합니다는 `Animation` 애니메이션 효과 줄 속성의 시작 및 끝 값을 포함 하 여 개체 및 속성의 값을 변경 하는 콜백을 합니다. `Animation` 개체를 실행 하 고 동기화 할 수 있는 자식 애니메이션의 컬렉션을 유지 관리할 수도 있습니다. 자세한 내용은 참조 [자식 애니메이션](#child)합니다.

사용 하 여 만든 애니메이션 실행는 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) 되었거나 자식 애니메이션 포함 되지 않을 수 있는 클래스를 호출 하 여 이루어집니다는 [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) 메서드. 이 메서드는 기간 애니메이션의 및 기타 항목을 적절 한 애니메이션을 반복 표시할지 여부를 제어 하는 콜백을 지정 합니다.

## <a name="creating-an-animation"></a>애니메이션 만들기

만들 때는 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) 개체, 일반적으로 최소 3 개의 매개 변수는 다음 코드 예제에서와 같이 필요:

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

이 코드의 애니메이션을 정의 하는 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) 속성은 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 인스턴스 값 1-2의 값입니다. Xamarin.Forms 하 여 파생 되는 애니메이션된 값을의 값을 변경 하려면 사용 된 첫 번째 인수로 지정 하는 콜백에 전달 되는 `Scale` 속성입니다.

애니메이션을 호출 하 여 시작 되는 [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) 메서드를 다음 코드 예제에서와 같이:

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

[ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) 메서드 반환 하지 않습니다는 `Task` 개체입니다. 대신, 알림 콜백 메서드를 통해 제공 됩니다.

다음 인수에 지정 된 된 `Commit` 메서드:

- 첫 번째 인수 (*소유자*) 애니메이션의 소유자를 식별 합니다. 이 애니메이션을 적용 하는 시각적 요소 또는 페이지 등의 다른 시각적 요소 수 있습니다.
- 두 번째 인수 (*이름*) 이름으로 애니메이션을 식별 합니다. 이름은 고유 하 게 식별 애니메이션 소유자와 결합 됩니다. 이 고유한 식별 애니메이션이 실행 중인지 확인을 사용할 수 있습니다 ([`AnimationIsRunning`](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AnimationIsRunning/p/Xamarin.Forms.IAnimatable/System.String/)), 또는을 취소할 ([`AbortAnimation`](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AbortAnimation/p/Xamarin.Forms.IAnimatable/System.String/)).
- 세 번째 인수 (*속도*)에 정의 된 콜백 메서드를 호출할 때마다 사이의 밀리초 수를 나타내는 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) 생성자
- 네 번째 인수 (*길이*) 밀리초에서 애니메이션의 지속 시간을 나타냅니다.
- 다섯 번째 인수 (*감속/가속*) 애니메이션에 사용 될 감속/가속 함수를 정의 합니다. 감속/가속 함수를 인수로 지정할 수 있습니다 또는 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) 생성자입니다. 감속/가속 함수에 대 한 자세한 내용은 참조 [감속/가속 함수](~/xamarin-forms/user-interface/animation/easing.md)합니다.
- 여섯 번째 인수 (*완료*)는 애니메이션 완료 되었을 때 실행 되는 콜백입니다. 이 콜백은 최종 값을 나타내는 두 번째 인수 되는 첫 번째 인수와 두 개의 인수는 `bool` 로 설정 되어 있는 `true` 애니메이션 취소 된 경우. 또는 *완료* 콜백 인수를 지정할 수 있습니다는 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) 생성자입니다. 그러나 단일 애니메이션 된 경우 *완료* 콜백을 둘 다에 지정 된는 `Animation` 생성자 및 `Commit` 메서드에 지정 된 콜백만는 `Commit` 메서드가 실행 됩니다.
- 일곱 번째 인수 (*반복*)는 애니메이션 반복을 허용 하는 콜백입니다. 애니메이션의 끝에서 호출 되 고 반환 `true` 애니메이션 반복 됨을 나타냅니다.

이 따라 증가 하는 애니메이션을 생성 하는 것은 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) 속성의는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 1에서 2, 2 초 이상 (2000 밀리초)을 사용 하는 [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) 감속/가속 함수입니다. 애니메이션이 완료 될 때마다 해당 `Scale` 속성을 1로 다시 설정 되 고 애니메이션 반복 됩니다.

> [!NOTE]
> 만들어 서로 독립적으로 실행 하는 동시 애니메이션을 생성할 수는 `Animation` 각 애니메이션에 대 한 개체를 호출한 다음는 `Commit` 각 애니메이션 메서드.

<a name="child" />

### <a name="child-animations"></a>자식 애니메이션

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) 클래스도 지원 자식 애니메이션을 만드는 과정이 포함 되어 있는 한 `Animation` 개체를 다른 `Animation` 개체가 추가 됩니다. 이렇게 하면 일련의 애니메이션을 실행 하 고 동기화 할 수 있습니다. 다음 코드 예제에서는 만들고 자식 애니메이션을 실행 하는 방법을 보여 줍니다.

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

또는 코드 예제에서는 더 간결 하 게, 다음 코드 예제에서 설명한로 작성할 수 있습니다.

```csharp
new Animation {
    { 0, 0.5, new Animation (v => image.Scale = v, 1, 2) },
    { 0, 1, new Animation (v => image.Rotation = v, 0, 360) },
    { 0.5, 1, new Animation (v => image.Scale = v, 2, 1) }
    }.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

두 코드 예제에서는 부모 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) 추가 하려는 개체가 만들어진 `Animation` 개체를 추가 합니다. 처음 두 개의 인수는 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/) 메서드 시작 및 자식 애니메이션을 종료 하는 시기를 지정 합니다. 인수 값 0과 1 사이 여야 하며 지정 된 자식 애니메이션 활성화 됩니다 부모 애니메이션 내의 상대적 기간을 나타냅니다. 따라서이 예제에에서는 `scaleUpAnimation` 애니메이션의 첫 번째 부분에 대해 활성화 됩니다는 `scaleDownAnimation` 애니메이션의 두 번째 절반에 대해 활성화 됩니다 및 `rotateAnimation` 전체 기간에 대 한 활성화 됩니다.

이 따라는 애니메이션 4 초 (4, 000 밀리초) 이상 발생 하 고 있습니다. `scaleUpAnimation` 애니메이션 효과 적용 된 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) 속성이 1에서 2, 2 초 이상으로 합니다. `scaleDownAnimation` 애니메이션을 적용 한 다음는 `Scale` 속성이 2에서 1, 2 초 이상으로 합니다. 두 배율 애니메이션 발생 하는 동안는 `rotateAnimation` 애니메이션 효과 적용는 [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) 속성을 0부터 360 사이, 4 초 이상. 크기 조정 애니메이션 감속/가속 함수를 사용 하는 참고 합니다. [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) 하면 감속/가속 함수는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 큰, 가져오기 전에 처음 축소 하 고 [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) 감속/가속 함수 로 인해는 `Image` 전체 애니메이션의 끝까지 실제 크기 보다 작게 되도록 합니다.

차이점의 여러 가지는 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) 하지 않은 개와 자식 애니메이션을 사용 하는 개체:

- 자식 애니메이션을 사용 하는 경우는 *완료* 자식 애니메이션에서 콜백 자식 완료 된 시점을 나타냅니다 및 *완료* 에 전달 된 콜백은 `Commit` 메서드 시기를 지정 합니다.는 전체 애니메이션이 완료 되었습니다.
- 자식 애니메이션을 사용 하는 경우 반환 `true` 에서 *반복* 에서 콜백은 `Commit` 메서드 애니메이션을 반복 하 고, 발생 하지 것입니다 되지만 애니메이션 계속 새 값 없이 실행 됩니다.
- 감속/가속 함수에 포함 되어 있는 경우는 `Commit` 메서드와 감속/가속 함수 반환 값을 1 보다 큰, 애니메이션 종료 됩니다. 감속/가속 함수 반환 값이 0 보다 작은 값을 0으로 제한 됩니다. 0 보다 작거나 1 보다 큰 값을 반환 하는 감속/가속 함수를 사용 하려면 해야에 지정 된 아닌 자식 애니메이션의 하나는 `Commit` 메서드.

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) 클래스도 포함 되어 [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/Xamarin.Forms.Animation/System.Double/System.Double/) 부모에 자식 애니메이션을 추가 하는 데 사용할 수 있는 메서드 `Animation` 개체입니다. 그러나 해당 *시작* 및 *마침* 인수 값을 1로 0으로 제한 된 아니지만 0 ~ 1 범위에 해당 하는 자식 애니메이션의 해당 부분에만 활성화 됩니다. 예를 들어 경우는 `WithConcurrent` 메서드 호출을 대상으로 하는 자식 애니메이션을 정의 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) 속성 1에서 6, 하지만 *시작* 및 *마침* 의 값 -2와 3을는 *시작* -2의 값에 해당 하는 `Scale` 값이 1 및 *마침* 값 3에 해당 하는 `Scale` 값 6입니다. 값 0과 1의 범위 밖에 있는 부분이 없으면 애니메이션에서 하므로 `Scale` 속성만 애니메이션이 적용 될 3 자에서 6입니다.

## <a name="canceling-an-animation"></a>애니메이션을 취소합니다.

응용 프로그램을 호출 하 여 애니메이션을 취소할 수는 [ `AbortAnimation` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AbortAnimation/p/Xamarin.Forms.IAnimatable/System.String/) 확장 메서드를 다음 코드 예제에서와 같이:

```csharp
this.AbortAnimation ("SimpleAnimation");
```

참고 애니메이션 애니메이션 소유자 및 애니메이션 이름 조합으로 고유 하 게 식별 합니다. 따라서 소유자와 이름 때 애니메이션을 실행 하려면 반드시 지정 해야 애니메이션 취소 지정 합니다. 코드 예제에서는 명명 된 애니메이션을 바로 취소 하는 따라서 `SimpleAnimation` 페이지가 소유 하는 합니다.

## <a name="creating-a-custom-animation"></a>사용자 지정 애니메이션 만들기

지금까지 여기 표시 된 예의 메서드와 동일 하 게 얻을 수 있는 애니메이션 확인 한 것은 [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) 클래스입니다. 그러나의 장점은 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) 은 애니메이션된 값이 변경 될 때 실행 되는 콜백 메서드에 대 한 액세스를 갖는다는 점 클래스입니다. 따라서 원하는 애니메이션을 구현 하는 콜백에 수 있습니다. 예를 들어 다음 코드 예제에서는 애니메이션 효과 적용 된 [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) 속성을 설정 하 여 페이지의 [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) 에서 생성 한 값은 [ `Color.FromHsla` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/)색상 값이 0에서 1 사이의 메서드:

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

애니메이션은 무지개 색의 색상을 통해 페이지 배경을 당기지의 모양을 제공 합니다.

베 지 어 곡선 애니메이션을 포함 하 여 복잡 한 애니메이션을 만드는 예는 참고 [22 장](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf) 의 [Xamarin.Forms 사용 하 여 모바일 앱 만들기](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)합니다.

## <a name="creating-a-custom-animation-extension-method"></a>사용자 지정 애니메이션 확장 메서드 만들기

확장 메서드는 [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) 클래스 속성 현재 값에서 지정된 된 값을 애니메이션 합니다. 이렇게 어려워집니다을 만들려면 예를 들어 한 `ColorTo` 때문에 다른 값이 두 개에서 색 애니메이션 효과를 사용할 수 있는 애니메이션 방법:

- 유일한 [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) 에 정의 된 속성의 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) 클래스는 [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/), 원하는 아닌 항상 `Color` 속성 에 애니메이션을 적용 합니다.
- 현재 값이 자주는 [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) 속성은 [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/), 실제 색 아닌 및 보간 계산에 사용할 수 없습니다.

이 문제를 해결할 수 없는 `ColorTo` 메서드는 특정 대상 [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) 속성입니다. 대신,는 보간된 전달 하는 콜백 메서드를 작성할 수 있습니다 `Color` 다시 호출자에 게는 값입니다. 또한 메서드는 사용 시작 및 종료 `Color` 인수입니다.

`ColorTo` 메서드를 사용 하는 확장 방법으로 구현할 수 있습니다는 [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate{T}/p/Xamarin.Forms.IAnimatable/System.String/System.Func{System.Double,T}/System.Action{T}/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{T,System.Boolean}/System.Func{System.Boolean}/) 에서 메서드는 [ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) 해당 기능을 제공 하는 클래스입니다. 때문에 이것이 `Animate` 속성을 대상 형식의 없는 메서드를 사용할 수 `double`다음 코드 예제에서와 같이,:

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

[ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate{T}/p/Xamarin.Forms.IAnimatable/System.String/System.Func{System.Double,T}/System.Action{T}/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{T,System.Boolean}/System.Func{System.Boolean}/) 메서드를 사용 하려면 한 *변환* 콜백 메서드로 인수입니다. 이 콜백은에 대 한 입력은 항상 한 `double` 0에서 1 까지입니다. 따라서는 `ColorTo` 메서드 정의 자체 변환 `Func` 수락 하는 `double` 0에서 1, 및 해당 반환 까지의 [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) 값에 해당 하는 값입니다. `Color` 보간 하 여 계산 된 [ `R` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.R/), [ `G` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.G/), [ `B` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.B/), 및 [ `A` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.A/) 제공 된 두 값 `Color` 인수입니다. `Color` 값은 특정 속성에 대 한 응용 프로그램에 대 한 콜백 메서드로 전달 합니다.

이 방법을 사용 하면는 `ColorTo` 메서드 하나를 애니메이션 효과를 주는 [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) 속성을 다음 코드 예제에서와 같이:

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

이 코드 예제는 `ColorTo` 메서드 애니메이션 효과 적용는 [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) 및 [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) 속성의는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)는 `BackgroundColor`는 페이지의 속성 및 [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) 속성은 [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)합니다.

## <a name="summary"></a>요약

이 문서를 사용 하는 방법에 명시 된 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) 을 생성 및 애니메이션 취소, 여러 애니메이션 동기화 속성은 기존 애니메이션에 애니메이션을 효과 적용 하는 사용자 지정 애니메이션을 만들 클래스 메서드를 합니다. `Animation` 클래스는 모든 Xamarin.Forms 애니메이션의 빌딩 블록입니다.


## <a name="related-links"></a>관련 링크

- [사용자 지정 애니메이션 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/custom/)
- [애니메이션](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)
- [AnimationExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/)
