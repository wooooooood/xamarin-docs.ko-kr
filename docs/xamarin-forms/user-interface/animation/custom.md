---
title: Xamarin.Forms에서 사용자 지정 애니메이션
description: 이 문서에서는 Xamarin.FOrms 애니메이션 클래스를 사용 하 여 만드는 애니메이션을 취소 하 고 여러 애니메이션을 동기화 하 고 속성은 기존 애니메이션 방법을 사용 하 여 애니메이션을 애니메이션을 적용 하는 사용자 지정 애니메이션을 만드는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: b195e63bcc88c4d1c659216f99ab698773f73e9e
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656807"
---
# <a name="custom-animations-in-xamarinforms"></a>Xamarin.Forms에서 사용자 지정 애니메이션

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-custom)

_애니메이션 클래스는 하나 이상의 애니메이션 개체를 만드는 ViewExtensions 클래스의 확장 메서드를 사용 하 여 모든 Xamarin.Forms 애니메이션의 빌딩 블록입니다. 이 문서에서는 기존 애니메이션 방법으로 애니메이션 효과가 적용 되지 않습니다 하는 속성에 애니메이션을 적용 하는 사용자 지정 애니메이션을 만들고 애니메이션 클래스를 사용 하 여 만드는 애니메이션을 취소 하 고 여러 애니메이션을 동기화 하는 방법을 보여 줍니다._


여러 매개 변수를 만들 때 지정 해야 합니다는 `Animation` 애니메이션 효과 줄 속성의 시작 및 끝 값을 포함 하 여 개체 및 속성의 값을 변경 하는 콜백입니다. `Animation` 개체를 실행 하 고 동기화 될 수 있는 자식 애니메이션의 컬렉션인 유지할 수도 있습니다. 자세한 내용은 [자식 애니메이션](#child)합니다.

사용 하 여 만든 애니메이션을 실행 합니다 [ `Animation` ](xref:Xamarin.Forms.Animation) 수도 자식 애니메이션 포함 되지 않을 수 있는 클래스를 호출 하 여 이루어집니다 합니다 [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) 메서드. 이 메서드는 기간 애니메이션 및 기타 항목 간에 애니메이션을 반복할지 여부를 제어 하는 콜백을 지정 합니다.

## <a name="creating-an-animation"></a>애니메이션 만들기

만들 때를 [ `Animation` ](xref:Xamarin.Forms.Animation) 개체, 일반적으로 다음 코드 예제에 설명 된 대로 최소 세 개의 매개 변수는 필요 합니다.

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

이 코드의 애니메이션을 정의 하는 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) 의 속성을 [ `Image` ](xref:Xamarin.Forms.Image) 인스턴스 값에서 1의 값이 2입니다. Xamarin.Forms에서 파생 된 애니메이션된 된 값의 값을 변경 하려면 사용 된 첫 번째 인수로 지정 된 콜백에 전달 되는 `Scale` 속성입니다.

애니메이션에 대 한 호출을 사용 하 여 시작 합니다 [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) 메서드를 다음 코드 예제에서 설명한 것 처럼:

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

합니다 [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) 메서드에서 반환 하지 않습니다는 `Task` 개체입니다. 대신, 알림 콜백 메서드를 통해 제공 됩니다.

다음 인수에 지정 된 된 `Commit` 메서드:

- 첫 번째 인수 (*소유자*) 애니메이션의 소유자를 식별 합니다. 이 애니메이션이 적용 되는 시각적 요소 또는 페이지와 같은 다른 시각적 요소 수 있습니다.
- 두 번째 인수 (*이름을*) 이름 사용 하 여 애니메이션을 식별 합니다. 애니메이션을 고유 하 게 식별 하기 위해 소유자 이름을 결합 됩니다. 애니메이션이 실행 중인지 여부를 확인 하려면 고유한 식별이 사용할 수 있습니다 ([`AnimationIsRunning`](xref:Xamarin.Forms.AnimationExtensions.AnimationIsRunning(Xamarin.Forms.IAnimatable,System.String)))를 취소 하거나 ([`AbortAnimation`](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String))).
- 세 번째 인수 (*속도*)에 정의 된 콜백 메서드 호출 사이의 시간을 밀리초 단위로 나타냅니다 합니다 [ `Animation` ](xref:Xamarin.Forms.Animation) 생성자
- 네 번째 인수 (*길이*) 밀리초에서 애니메이션의 지속 시간을 나타냅니다.
- 다섯 번째 인수 (*감속/가속*) 애니메이션에 사용할 감속/가속 함수를 정의 합니다. 감속/가속 함수에 인수로 지정할 수 있습니다 또는 합니다 [ `Animation` ](xref:Xamarin.Forms.Animation) 생성자입니다. 감속/가속 함수에 대 한 자세한 내용은 참조 하십시오 [감속/가속 함수](~/xamarin-forms/user-interface/animation/easing.md)합니다.
- 여섯 번째 인수 (*완료*) 애니메이션이 완료 되 면 실행 될 콜백 합니다. 이 콜백에서 최종 값 및 되 고 있는 두 번째 인수를 나타내는 첫 번째 인수를 사용 하 여 두 개의 인수를 한 `bool` 로 설정 된 `true` 애니메이션 취소 된 경우. 또는 합니다 *완료* 콜백을 인수로 지정할 수 있습니다 합니다 [ `Animation` ](xref:Xamarin.Forms.Animation) 생성자입니다. 그러나 단일 애니메이션을 사용 하 여 경우 *완료* 콜백을 둘 다 지정 되는 `Animation` 생성자 및 `Commit` 에 지정 된 콜백만 메서드를 `Commit` 메서드가 실행 됩니다.
- 일곱 번째 인수 (*반복*)는 애니메이션 반복을 허용 하는 콜백입니다. 애니메이션의 끝에 호출 되 고 반환 `true` 애니메이션이 반복 됨을 나타냅니다.

이 따라 증가 하는 애니메이션을 만드는 것을 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) 의 속성을 [ `Image` ](xref:Xamarin.Forms.Image) 1에서 2, 2 초 이상 (2000 밀리초)을 사용 하 여를 [ `Linear` ](xref:Xamarin.Forms.Easing.Linear) 감속/가속 함수입니다. 애니메이션이 완료 되 면 때마다 해당 `Scale` 속성을 1로 다시 설정 되 고 애니메이션 반복 합니다.

> [!NOTE]
> 만들어 서로 독립적으로 실행 하는 동시 애니메이션을 생성할 수 있습니다는 `Animation` 각 애니메이션에 대 한 개체를 호출 합니다 `Commit` 각 애니메이션 메서드.

<a name="child" />

### <a name="child-animations"></a>자식 애니메이션

합니다 [ `Animation` ](xref:Xamarin.Forms.Animation) 클래스에서는 자식 애니메이션 만들기 포함 됩니다는 `Animation` 개체는 다른 `Animation` 개체가 추가 됩니다. 이 통해 일련의 애니메이션을 실행 하 고 동기화 합니다. 다음 코드 예제에서는 만들고 자식 애니메이션을 실행 하는 방법을 보여 줍니다.

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

두 코드 예제에서는 부모 [ `Animation` ](xref:Xamarin.Forms.Animation) 추가 하려는 개체를 만든 `Animation` 개체 추가 됩니다. 처음 두 인수를 합니다 [ `Add` ](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)) 메서드 시작 및 자식 애니메이션 완료 시기를 지정 합니다. 인수 값 0과 1 사이 여야 하 고 지정 된 자식 애니메이션 active 되도록 부모 애니메이션 내의 상대 기간을 나타냅니다. 따라서이 예제의 합니다 `scaleUpAnimation` 애니메이션의 처음 절반에 대해 활성화 됩니다 합니다 `scaleDownAnimation` 애니메이션의 두 번째 절반에 대해 활성화 됩니다 및 `rotateAnimation` 전체 기간에 대 한 활성화 됩니다.

이 따라 애니메이션이 4 초 (4000 밀리초) 이상 발생 하는 경우 `scaleUpAnimation` 애니메이션을 적용 합니다 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) 1에서 속성을 2, 2 초 이상. 합니다 `scaleDownAnimation` 애니메이션을 적용 한 다음는 `Scale` 2에서 속성을 1, 2 초 이상. 모두 확장 애니메이션 발생 하는 동안에 `rotateAnimation` 애니메이션 효과 줍니다 합니다 [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) 속성을 0부터 360 사이, 4 초 이상. 크기 조정 애니메이션 감속/가속 함수를 사용 하는 참고 합니다. [ `SpringIn` ](xref:Xamarin.Forms.Easing.SpringIn) 하면 감속/가속 함수를 [ `Image` ](xref:Xamarin.Forms.Image) 더 가져오기 전에 처음부터 축소 하 고 [ `SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut) 감속/가속 함수 하면는 `Image` 완료 애니메이션의 끝 쪽 실제 크기 보다 작게 되도록 합니다.

차이점 가지는 [ `Animation` ](xref:Xamarin.Forms.Animation) 없는 자식 애니메이션을 사용 하는 개체 및:

- 자식 애니메이션을 사용 하는 경우는 *완료* 자식 애니메이션에는 콜백을 나타냅니다 자식 완료 되 면, 및 *완료* 에 전달 된 콜백은 `Commit` 메서드 시기를 지정 합니다.는 전체 애니메이션이 완료 되었습니다.
- 자식 애니메이션을 사용 하는 경우 반환 `true` 에서 합니다 *반복* 에서 콜백을 `Commit` 메서드 애니메이션을 반복 하 고, 발생 하지 것입니다 하지만 애니메이션 계속 새 값 없이 실행 됩니다.
- 에 감속/가속 함수를 포함 하는 경우는 `Commit` 메서드 및 감속/가속 함수 반환 값을 1 보다 큰, 애니메이션이 종료 됩니다. 감속/가속 함수 반환 값이 0 보다 작은 경우 값은 0으로 고정 됩니다. 아닌 자식 애니메이션의 하나로 지정 해야 합니다 0 미만 이거나 1 보다 큰 값을 반환 하는 감속/가속 함수를 사용 하 여 `Commit` 메서드.

합니다 [ `Animation` ](xref:Xamarin.Forms.Animation) 클래스도 포함 되어 있습니다 [ `WithConcurrent` ](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double)) 부모에 자식 애니메이션을 추가할 수 있는 메서드 `Animation` 개체입니다. 그러나 해당 *시작* 하 고 *마침* 인수 값에는 0에서 1 사이의에 제한 되지 않습니다. 하지만 0 ~ 1 범위에 해당 하는 자식 애니메이션의 해당 부분에만 활성화 됩니다. 예를 들어 경우는 `WithConcurrent` 메서드 호출 대상으로 하는 자식 애니메이션을 정의 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) 6, 하지만 1에서 속성 *시작* 및 *마침* 값 -2와 3을를 *시작* 에 해당 하는 값-2 `Scale` 값이 1, 및 *마침* 에 해당 하는 값이 3 `Scale` 값 6. 0과 1의 범위를 벗어나는 값 애니메이션의 어떠한 부분도 재생 하기 때문에 `Scale` 속성만 애니메이트 될 3에서 6입니다.

## <a name="canceling-an-animation"></a>애니메이션을 취소합니다.

응용 프로그램에 대 한 호출을 사용 하 여 애니메이션을 취소할 수는 [ `AbortAnimation` ](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String)) 확장 메서드를 다음 코드 예제에서 설명한 것 처럼:

```csharp
this.AbortAnimation ("SimpleAnimation");
```

애니메이션 애니메이션 소유자 및 애니메이션 이름 조합으로 고유 하 게 식별 하는 참고 합니다. 따라서 이름과 소유자 때 애니메이션을 실행 하려면 반드시 지정 해야 애니메이션 취소 지정 합니다. 코드 예제에서는 명명 된 애니메이션을 즉시 취소 하는 따라서 `SimpleAnimation` 페이지가 소유 합니다.

## <a name="creating-a-custom-animation"></a>사용자 지정 애니메이션 만들기

지금 여기 나와 있는 예제에서 메서드를 사용 하 여 동일 하 게 얻을 수 있는 애니메이션 주었습니다 합니다 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) 클래스입니다. 그러나 장점이 합니다 [ `Animation` ](xref:Xamarin.Forms.Animation) 클래스는 애니메이션된 된 값이 변경 될 때 실행 되는 콜백 메서드에 대 한 액세스에 있습니다. 이렇게 하면 원하는 모든 애니메이션을 구현 하는 콜백에 수 있습니다. 예를 들어, 다음 코드 예제에서는 애니메이션을 적용 합니다 [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) 로 설정 하 여 페이지의 속성 [ `Color` ](xref:Xamarin.Forms.Color) 에서 생성 한 값을 [ `Color.FromHsla` ](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double))색상 값이 0에서 1 사이의 메서드:

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

결과 애니메이션 무지개 색을 통해 페이지 배경을 이동의 모양을 제공 합니다.

베 지 어 곡선 애니메이션을 포함 하 여 복잡 한 애니메이션을 만드는 방법의 자세한 예제를 참조 하세요. [22 장](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf) 의 [Creating Mobile Apps with Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)합니다.

## <a name="creating-a-custom-animation-extension-method"></a>사용자 지정 애니메이션 확장 메서드 만들기

확장 메서드는 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) 클래스를 지정 된 값의 현재 값에서 속성 애니메이션을 적용 합니다. 이렇게 되 면 어려워집니다를 만들려면 예를 들어를 `ColorTo` 때문에 다른 값에서 색에 애니메이션 효과를 사용할 수 있는 애니메이션 메서드:

- 유일한 [ `Color` ](xref:Xamarin.Forms.Color) 에 정의 된 속성을 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) 클래스는 [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor), 원하는 아닌 항상 `Color` 속성 에 애니메이션을 적용 합니다.
- 종종 현재 값을 [ `Color` ](xref:Xamarin.Forms.Color) 속성이 [ `Color.Default` ](xref:Xamarin.Forms.Color.Default), 실제 색을 아닌 및 보간 계산에 사용할 수 없는 합니다.

이 문제를 해결 하지는 합니다 `ColorTo` 메서드를 특정 대상 [ `Color` ](xref:Xamarin.Forms.Color) 속성입니다. 대신, 보간된를 전달 하는 콜백 메서드를 사용 하 여 쓸 수 `Color` 다시 호출자에 게는 값입니다. 메서드는 시작을 수행 하 고 종료 하는 또한 `Color` 인수입니다.

`ColorTo` 메서드를 사용 하는 확장 메서드로 구현할 수 있습니다는 [ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*) 에서 메서드를 [ `AnimationExtensions` ](xref:Xamarin.Forms.AnimationExtensions) 해당 기능을 제공 합니다. 왜냐하면 합니다 `Animate` 유형이 아닌 대상 속성에 메서드를 사용할 수 `double`다음 코드 예제에서 설명한 것 처럼:

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

합니다 [ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*) 메서드를 사용 하려면를 *변환* 콜백 메서드 인수입니다. 이 콜백은에 대 한 입력은 항상를 `double` 0에서 1 까지입니다. 따라서를 `ColorTo` 메서드 정의 자체 변환 `Func` 받아들이는 `double` 0에서 1이 고 반환 까지의 [ `Color` ](xref:Xamarin.Forms.Color) 해당 값에 해당 하는 값입니다. `Color` 값은 보간에 따라 계산 합니다 [ `R` ](xref:Xamarin.Forms.Color.R), [ `G` ](xref:Xamarin.Forms.Color.G)를 [ `B` ](xref:Xamarin.Forms.Color.B), 및 [ `A` ](xref:Xamarin.Forms.Color.A) 제공 된 두 값 `Color` 인수입니다. `Color` 값은 특정 속성에 대 한 응용 프로그램에 대 한 콜백 메서드로 전달 합니다.

이 방법을 사용 하면 합니다 `ColorTo` 하나에 애니메이션을 적용 하는 방법 [ `Color` ](xref:Xamarin.Forms.Color) 속성을 다음 코드 예제에서 설명한 것 처럼:

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

이 코드 예제에서는 `ColorTo` 메서드 애니메이션을 적용 합니다 [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) 및 [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) 의 속성을 [ `Label` ](xref:Xamarin.Forms.Label), 합니다 `BackgroundColor`페이지의 속성 및 [ `Color` ](xref:Xamarin.Forms.BoxView.Color) 속성을 [ `BoxView` ](xref:Xamarin.Forms.BoxView)합니다.

## <a name="summary"></a>요약

이 문서에 사용 하는 방법을 설명 합니다 [ `Animation` ](xref:Xamarin.Forms.Animation) 및 애니메이션을 취소, 여러 애니메이션 동기화 만들고 기존 애니메이션은 애니메이션 속성에 애니메이션을 적용 하는 사용자 지정 애니메이션 클래스 메서드입니다. `Animation` 클래스는 모든 Xamarin.Forms 애니메이션의 빌딩 블록입니다.


## <a name="related-links"></a>관련 링크

- [사용자 지정 애니메이션 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-custom)
- [애니메이션](xref:Xamarin.Forms.Animation)
- [AnimationExtensions](xref:Xamarin.Forms.AnimationExtensions)
