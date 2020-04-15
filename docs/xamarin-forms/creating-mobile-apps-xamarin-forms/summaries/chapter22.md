---
title: 22장의 요약 정보입니다. 애니메이션
description: 'Xamarin.Forms로 모바일 앱 만들기: 22장의 요약 정보입니다. 애니메이션'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 47C2B9AB-E688-4412-8AF5-9F633B3DA695
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 935be5bd6696600644463eb4ec26410b546f42a0
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "70771003"
---
# <a name="summary-of-chapter-22-animation"></a>22장의 요약 정보입니다. 애니메이션

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)

Xamarin.Android 타이머 또는 `Task.Delay`를 사용하여 사용자 고유의 애니메이션을 만들 수 있지만 일반적으로 Xamarin.Forms에서 제공하는 애니메이션 기능을 사용하는 것이 더 쉽습니다. 세 클래스는 다음과 같은 애니메이션을 구현합니다.

- [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions), 상위 수준 접근 방식
- [`Animation`](xref:Xamarin.Forms.Animation), 다양한 기능을 제공하지만 더 어려움
- [`AnimationExtension`](xref:Xamarin.Forms.AnimationExtensions), 가장 다양한 기능을 제공하는 가장 낮은 수준의 접근 방식

일반적으로 애니메이션은 바인딩 가능한 속성을 통해 지원되는 속성을 대상으로 합니다. 이것은 요구 사항이 아니지만 동적으로 변경 사항에 대응하는 유일한 속성입니다.

해당 애니메이션에 대한 XAML 인터페이스는 없지만 [**23장 트리거 및 동작**](chapter23.md)에서 설명하는 기술을 사용하여 애니메이션을 XAML에 연결할 수 있습니다.

## <a name="exploring-basic-animations"></a>기본 애니메이션 살펴보기

기본 애니메이션 함수는 [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 클래스에 있는 확장 메서드입니다. 해당 메서드는 `VisualElement`에서 파생되는 모든 개체에 적용됩니다. 가장 간단한 애니메이션은 [`Chapter 21. Transforms`](chapter21.md)에 설명된 변환 속성을 대상으로 합니다.

[**AnimationTryout**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/AnimationTryout)은 `Button`에 대한 `Clicked` 이벤트 처리기가 [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 확장 메서드를 호출하여 원의 단추를 회전하는 방법을 보여 줍니다.

`RotateTo` 메서드는 1/4초 주기(기본값)로 `Button`의 `Rotation` 속성을 0에서 360까지 변경합니다. 그러나 `Button`을 다시 탭하면 `Rotation` 속성이 이미 360도이므로 아무 작업도 수행되지 않습니다.

### <a name="setting-the-animation-duration"></a>애니메이션 기간 설정

`RotateTo`에 대한 두 번째 인수는 시간(밀리초)입니다. 이 인수를 큰 값으로 설정하고 애니메이션 중에 `Button`을 누르면 현재 각도부터 새 애니메이션이 시작됩니다.

### <a name="relative-animations"></a>상대 애니메이션

`RelRotateTo` 메서드는 지정된 값을 기존 값에 추가하여 상대 회전을 수행합니다. 이 메서드를 사용하면 `Button`을 여러 번 탭할 수 있으며, 매번 `Rotation` 속성이 360도까지 늘어납니다.

### <a name="awaiting-animations"></a>대기 애니메이션

`ViewExtensions`의 모든 애니메이션 메서드는 `Task<bool>` 개체를 반환합니다. 즉, `ContinueWith` 또는 `await`를 사용하여 일련의 순차 애니메이션을 정의할 수 있습니다. `bool` 완료 반환 값은 애니메이션이 중단 없이 완료되면 `false`이고, [`CancelAnimation`](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) 메서드에 의해 취소되어 동일한 요소에 설정된 `ViewExtensions`의 다른 메서드에서 시작된 모든 애니메이션이 취소되면 `true`입니다.

### <a name="composite-animations"></a>복합 애니메이션

대기 및 비대기 애니메이션을 조합하여 복합 애니메이션을 만들 수 있습니다. `TranslationX`, `TranslationY`및 `Scale` 변환 속성을 대상으로 하는 `ViewExtensions`의 애니메이션이 여기에 해당합니다.

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))

`TranslateTo`는 `TranslationX` 및 `TranslationY` 속성에 잠재적으로 영향을 줄 수 있습니다.

### <a name="taskwhenall-and-taskwhenany"></a>Task.WhenAll 및 Task.WhenAny

또한 여러 작업을 모두 완료되었을 때 신호를 보내는 [`Task.WhenAll`](xref:System.Threading.Tasks.Task.WhenAll*)를 사용하거나 처음 몇 개 작업이 완료되었을 때 신호를 보내는 [`Task.WhenAny`](xref:System.Threading.Tasks.Task.WhenAny*)를 사용하여 동시 애니메이션을 관리할 수도 있습니다.

### <a name="rotation-and-anchors"></a>회전 및 앵커

`ScaleTo`, `RelScaleTo`, `RotateTo` 및 `RelRotateTo` 메서드를 호출하는 경우 [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) 및 [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) 속성을 설정하여 크기 조정 및 회전의 중심을 나타낼 수 있습니다.

[**CircleButton**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CircleButton)은 페이지의 중심을 기준으로 `Button`을 회전하여 이 기술을 보여 줍니다.

### <a name="easing-functions"></a>감속/가속 함수

일반적으로 애니메이션은 시작 값부터 끝 값까지 선형입니다. 감속/가속 함수를 통해 코스에서 애니메이션을 가속 또는 감속할 수 있습니다. 애니메이션 메서드에 대한 마지막 선택적 인수는 [`Easing`](xref:Xamarin.Forms.Easing) 형식으로, `Easing` 형식의 11가지 정적 읽기 전용 필드를 정의하는 클래스입니다.

- [`Linear`](xref:Xamarin.Forms.Easing.Linear), 기본값
- [`SinIn`](xref:Xamarin.Forms.Easing.SinIn), [`SinOut`](xref:Xamarin.Forms.Easing.SinOut) 및 [`SinInOut`](xref:Xamarin.Forms.Easing.SinInOut)
- [`CubicIn`](xref:Xamarin.Forms.Easing.CubicIn), [`CubicOut`](xref:Xamarin.Forms.Easing.CubicOut) 및 [`CubicInOut`](xref:Xamarin.Forms.Easing.CubicInOut)
- [`BounceIn`](xref:Xamarin.Forms.Easing.BounceIn) 및 [`BounceOut`](xref:Xamarin.Forms.Easing.BounceOut)
- [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) 및 [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut)

`In` 접미사는 해당 효과가 애니메이션의 시작 부분에 있음을 나타내고, `Out`은 끝 부분에 있음을 나타내고, `InOut`는 애니메이션의 시작과 끝 부분에 있음을 나타냅니다.

[**BounceButton**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BounceButton) 샘플에서는 감속/가속 함수를 사용하는 방법을 보여 줍니다.

### <a name="your-own-easing-functions"></a>사용자 고유의 감속/가속 함수

[`Easing` 생성자](xref:Xamarin.Forms.Easing.%23ctor(System.Func{System.Double,System.Double}))에 [`Func<double, double>`](xref:System.Func`2)을 전달하여 사용자 고유의 감속/가속 함수를 정의할 수도 있습니다. 또한 `Easing`은 `Func<double, double>`에서 `Easing`으로의 묵시적 변환을 정의합니다. 감속/가속 함수에 대한 인수는 애니메이션이 시작부터 끝까지 선형으로 진행되므로 항상 0에서 1 사이입니다. 이 함수는 *일반적으로* 0에서 1 사이의 값을 반환하지만, 음수이거나 1보다 클 수도 있고(`SpringIn` 및 `SpringOut` 함수를 사용하는 경우) 수행 중인 작업을 잘 알고 있다면 규칙과 다를 수도 있습니다.

[**UneasyScale**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/UneasyScale) 샘플에서는 사용자 지정 감속/가속 함수를 보여 주고 [**CustomCubicEase**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CustomCubicEase)는 다른 함수를 보여 줍니다.

[**SwingButton**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingButton) 샘플은 사용자 지정 감속/가속 함수를 보여 주고 회전 애니메이션 시퀀스 내에서 `AnchorX` 및 `AnchorY` 속성을 변경하는 기술도 보여 줍니다.

[**Xamarin.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에는 사용자 지정 감속/가속 함수를 사용하여 단추를 클릭할 때 위아래로 빠르게 움직이는 [`JiggleButton`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/JiggleButton.cs) 클래스가 있습니다. [**JiggleButtonDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/JiggleButtonDemo) 샘플은 이 작업을 보여 줍니다.

### <a name="entrance-animations"></a>나타내기 애니메이션

한 가지 인기 있는 애니메이션 유형은 페이지가 처음 표시될 때 나타납니다. 해당 애니메이션은 페이지의 [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) 재정의 시 시작될 수 있습니다. 해당 애니메이션의 경우 해당 페이지를 애니메이션을 *뒤에* 표시하는 방법을 XAML로 설정한 다음, 코드에서 레이아웃을 초기화하고 애니메이션 효과를 주는 것이 가장 좋습니다.

[**FadingEntrance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingEntrance) 샘플은 [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 확장 메서드를 사용하여 페이지 내용을 페이드 인합니다.

[**SlidingEntrance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SlidingEntrance) 샘플은 [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) 확장 메서드를 사용하여 슬라이드에서 페이지 내용을 슬라이드 인합니다.

[**SwingingEntrance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingingEntrance) 샘플은 [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 확장 메서드를 사용하여 `RotationY` 속성에 애니메이션 효과를 적용합니다. [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드를 사용할 수도 있습니다.

### <a name="forever-animations"></a>영구 애니메이션

다른 극단적인 경우에 "영구" 애니메이션은 프로그램이 종료될 때까지 실행됩니다. 해당 기능은 일반적으로 데모용입니다.

[**FadingTextAnimation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingTextAnimation) 샘플은 [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 애니메이션을 사용하여 텍스트의 두 부분을 페이드 인 및 페이드 아웃합니다.

[**PalindromeAnimation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/PalindromeAnimation)은 회문을 표시한 다음, 개별 문자를 180도씩 순서대로 회전하여 모두 거꾸로 표시합니다. 그런 다음, 전체 문자열을 원래 문자열과 동일하게 180도로 대칭 이동합니다.

[**CopterAnimation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CopterAnimation) 샘플은 화면을 중심으로 회전하면서 간단한 `BoxView` 헬리콥터를 회전합니다.

[**RotatingSpokes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotatingSpokes)는 화면을 중심으로 `BoxView` 스포크를 회전한 다음, 각 스포크 자체를 회전하여 흥미로운 패턴을 만듭니다.

[![회전하는 스포크의 3중 스크린샷](images/ch22fg21-small.png "스포크 회전")](images/ch22fg21-large.png#lightbox "스포크 회전")

그러나 [**RotationBreakdown**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotationBreakdown) 샘플이 나타내는 것처럼, 요소의 `Rotation` 속성을 점진적으로 늘려도 장기간 작동하지 않을 수 있습니다.

[**SpinningImage**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpinningImage) 샘플은 [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 및 [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))을 사용하여 3D 공간에서 비트맵을 회전하는 것처럼 보이게 합니다.

### <a name="animating-the-bounds-property"></a>범위 속성에 애니메이션 적용

아직 설명되지 않은 `ViewExtensions`의 유일한 확장 메서드는 [`Layout`](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) 메서드를 호출하여 읽기 전용 [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds) 속성에 효과적으로 애니메이션 효과를 주는 [`LayoutTo`](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing))입니다. 이 메서드는 일반적으로 `Layout` 파생물에 의해 호출됩니다. 이 내용은 [**26장. CustomLayouts**](chapter26.md)에서 설명합니다.

`LayoutTo` 메서드는 특별한 용도로만 제한해야 합니다. [**BouncingBox**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BouncingBox) 프로그램은 이 메서드를 사용하여 페이지의 측면에 닿을 대 `BoxView`를 압축했다가 확장합니다.

[**XamagonXuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle) 샘플은 `LayoutTo`를 사용하여 클래식 15-16 퍼즐 구현에서 타일을 이동하여 번호가 매겨진 타일이 아닌 스크램블된 이미지를 표시합니다.

[![Xamarin Xuzzle의 3중 스크린샷](images/ch22fg26-small.png "Xuzzle 퍼즐 게임")](images/ch22fg26-large.png#lightbox "Xuzzle 퍼즐 게임")

### <a name="your-own-awaitable-animations"></a>사용자 고유의 대기 가능 애니메이션

[**TryAwaitableAnimation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/TryAwaitableAnimation) 샘플은 대기 가능 애니메이션을 만듭니다. 메서드에서 `Task` 개체를 반환하고 애니메이션이 완료될 때 신호할 수 있는 중요한 클래스는 [`TaskCompletionSource`](xref:System.Threading.Tasks.TaskCompletionSource`1)입니다.

## <a name="deeper-into-animations"></a>애니메이션에 대해 자세히 알아보기

Xamarin.Forms 애니메이션 시스템은 약간 혼동을 줄 수 있습니다. 애니메이션 시스템은 `Easing` 클래스 외에, `ViewExtensions`, `Animation`및 `AnimationExtension` 클래스를 구성합니다.

### <a name="viewextensions-class"></a>ViewExtensions 클래스

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)를 이미 살펴보았을 것입니다. 이 클래스는 `Task<bool>` 및 [`CancelAnimations`](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement))를 반환하는 9개의 메서드를 정의합니다. 9개의 메서드 중 7개는 변환 속성을 대상으로 합니다. 다른 두 메서드는 [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) 속성을 대상으로 하는 [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))와, [`Layout`](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) 메서드를 호출하는 [`LayoutTo`](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing))입니다.

### <a name="animation-class"></a>Animation 클래스

[`Animation`](xref:Xamarin.Forms.AnimationExtensions) 클래스에는 콜백 및 완료된 메서드와 애니메이션의 매개 변수를 정의하기 위해 5개의 인수를 사용하는 [생성자](xref:Xamarin.Forms.Animation.%23ctor(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Action))가 있습니다.

자식 애니메이션은 [`Add`](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)), [`Insert`](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation)), [`WithConcurrent`](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double))와 [`WithConcurrent`](xref:Xamarin.Forms.Animation.WithConcurrent(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Double,System.Double))의 오버로드를 사용하여 추가할 수 있습니다.

그런 다음, [`Commit`](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) 메서드를 호출하여 애니메이션 개체를 시작합니다.

### <a name="animationextensions-class"></a>AnimationExtensions 클래스

[`AnimationExtensions`](xref:Xamarin.Forms.AnimationExtensions) 클래스에는 주로 확장 메서드가 포함되어 있습니다. `Animate` 메서드의 버전은 여러 가지이며 제네릭 [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) 메서드가 다양한 기능을 제공하므로 실제로는 이 애니메이션 함수만 있으면 됩니다.

## <a name="working-with-the-animation-class"></a>Animation 클래스 작업

[**ConcurrentAnimations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) 샘플은 여러 가지 애니메이션을 사용하여 [`Animation`](xref:Xamarin.Forms.Animation) 클래스를 보여 줍니다.

### <a name="child-animations"></a>자식 애니메이션

[**ConcurrentAnimations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) 샘플도(매우 유사한) [`Add`](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)) 및 [`Insert`](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation)) 메서드를 사용하는 자식 애니메이션을 보여 줍니다.

### <a name="beyond-the-high-level-animation-methods"></a>상위 수준 애니메이션 메서드 이외의 메서드

또한 [**ConcurrentAnimations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) 샘플은 `ViewExtensions` 메서드의 대상이 되는 속성을 벗어나는 애니메이션 수행 방법을 보여 줍니다. 한 예에서는 일련의 기간이 더 길어지지만 다른 예에서는 `BackgroundColor` 속성에 애니메이션을 적용합니다.

### <a name="more-of-your-own-awaitable-methods"></a>사용자 고유의 대기 가능 메서드에 대한 추가 정보

`ViewExtensions`의 [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드는 [`Easing.SpringOut`](xref:Xamarin.Forms.Easing.SpringOut) 함수에서 작동하지 않습니다. 감속/가속 출력이 1을 초과하면 중지됩니다.

[**Xamarin.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에는 이 문제가 없는 [`TranslateXTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L12) and [`TranslateYTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L49) 확장 메서드와 해당 애니메이션을 취소하기 위한 [`CancelTranslateXTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L44) and [`CancelTranslateYTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L71) 메서드를 포함하는 [`MoreViewExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) 클래스가 있습니다.

[**SpringSlidingEntrance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpringSlidingEntrance)는 `TranslateXTo` 메서드를 보여 줍니다.

`MoreExtensions` 클래스에는 X 및 Y 변환을 결합하는 [`TranslateXYTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L76) 확장 메서드와 [`CancelTranslateXYTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L113) 메서드도 있습니다.

### <a name="implementing-a-bezier-animation"></a>베지어 애니메이션 구현

베지어 스플라인의 경로를 따라 요소를 이동하는 애니메이션을 개발할 수도 있습니다. [**Xamarin.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에는 베지어 스플라인 및 [`BezierTangent`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierTangent.cs) 열거형을 캡슐화하여 방향을 제어하는 [`BezierSpline`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierSpline.cs) 구조가 포함되어 있습니다.

[`MoreViewExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) 클래스에는 [`BezierPathTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L118) 확장 메서드와 [`CancelBezierPathTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L161) 메서드가 있습니다.

[**BezierLoop**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BezierLoop) 샘플은 베지어 경로를 따라 요소에 애니메이션 효과를 주는 방법을 보여 줍니다.

## <a name="working-with-animationextensions"></a>AnimationExtensions 작업

표준 컬렉션에서 누락된 애니메이션의 한 가지 유형은 색 애니메이션입니다. 문제는 두 `Color` 값 사이를 보간하는 적절한 방법이 없다는 것입니다. 개별 RGB 값을 보간하는 것이 가능하지만 HSL 값을 보간하는 것도 가능합니다.

해당 이유로, [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 [`MoreViewExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) 클래스에는 2가지 애니메이션 메서드인 [`RgbColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L166) 및 [`HslColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L188)가 포함되어 있습니다. [`CancelRgbColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L183) 및 [`CancelHslColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L206)의 2가지 취소 메서드도 있습니다.

두 메서드는 [`AnimationExtensions`](xref:Xamarin.Forms.AnimationExtensions)에서 광범위한 제네릭 [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) 메서드를 호출하여 애니메이션을 수행하는 [`ColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L211)를 사용합니다.

[**ColorAnimations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ColorAnimations) 샘플은 해당 두 가지 유형의 색 애니메이션을 사용하는 방법을 보여 줍니다.

## <a name="structuring-your-animations"></a>애니메이션 구조화

XAML에서 애니메이션을 표현하고 MVVM과 함께 사용하는 것이 유용한 경우도 있습니다. 이 내용은 다음 장, [**23장. 트리거 및 동작**](chapter23.md)에서 설명합니다.

## <a name="related-links"></a>관련 링크

- [22장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)
- [22장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)
- [애니메이션](~/xamarin-forms/user-interface/animation/index.md)
