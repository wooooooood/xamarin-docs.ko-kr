---
title: 요약 22 장입니다. 애니메이션
description: 'Xamarin.ios를 사용 하 여 Mobile Apps 만들기: 요약 22 장입니다. 애니메이션'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 47C2B9AB-E688-4412-8AF5-9F633B3DA695
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 935be5bd6696600644463eb4ec26410b546f42a0
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70771003"
---
# <a name="summary-of-chapter-22-animation"></a>요약 22 장입니다. 애니메이션

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)

지금까지 살펴본 Xamarin.Forms 타이머를 사용 하 여 사용자 고유의 애니메이션을 만들 수 있습니다 또는 `Task.Delay`, Xamarin.Forms 제공 하는 애니메이션 기능을 사용 하 여 일반적으로 쉽게 이지만 합니다. 세 클래스는 이러한 애니메이션을 구현합니다.

- [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)고급 접근 방식
- [`Animation`](xref:Xamarin.Forms.Animation)를 꺼내는 융통성이 뛰어납니다. 또한
- [`AnimationExtension`](xref:Xamarin.Forms.AnimationExtensions)에서 가장 다양 한, 가장 낮은 수준의 방법

일반적으로 애니메이션 바인딩 가능한 속성에 의해 백업 되는 속성을 대상으로 합니다. 요구 사항은 아닙니다. 하지만 다음은 동적으로 변경에 반응 하는 유일한 속성입니다.

이러한 애니메이션에 대 한 XAML 인터페이스가 있지만 설명한 기법을 사용 하 여 XAML에 애니메이션을 통합할 수 있습니다 [ **23 장입니다. 트리거 및 동작**](chapter23.md)합니다.

## <a name="exploring-basic-animations"></a>탐색의 기본적인 애니메이션

기본 애니메이션 기능은 있는 확장 메서드는 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) 클래스입니다. 이러한 메서드에서 파생 된 모든 개체에 적용할 `VisualElement`합니다. 가장 간단한 애니메이션 대상 속성에 설명 된 변환을 [ `Chapter 21. Transforms` ](chapter21.md)합니다.

[ **AnimationTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/AnimationTryout) 방법을 보여 줍니다 하는 방법을 `Clicked` 에 대 한 이벤트 처리기를 `Button` 호출할 수는 [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 확장 메서드를 실행 합니다 원 안에 단추입니다.

`RotateTo` 메서드 변경 합니다 `Rotation` 의 속성을 `Button` 360 기본적으로 1-4 초 동안 0에서. 하지만 경우는 `Button` 를 탭 할 다시 아무것도 수행 하지 않는다면 때문에 `Rotation` 속성은 이미 360도 합니다.

### <a name="setting-the-animation-duration"></a>애니메이션 지속 시간 설정

두 번째 인수를 `RotateTo` 기간 (밀리초)입니다. 경우 큰 값으로 설정 탭을 `Button` 애니메이션 중 현재 각도에서 시작 하 여 새 애니메이션을 시작 합니다.

### <a name="relative-animations"></a>상대 애니메이션

`RelRotateTo` 메서드는 기존 값에 지정된 된 값을 추가 하 여 상대적인 회전을 수행 합니다. 이 방법을 사용 하면 합니다 `Button` 여러 시간 및 각 탭 할 시간 증가 `Rotation` 360도 속성.

### <a name="awaiting-animations"></a>애니메이션을 대기 중

모든 애니메이션 메서드 `ViewExtensions` 반환 `Task<bool>` 개체입니다. 즉, 일련의 순차적 애니메이션을 사용 하 여 정의할 수 있습니다 `ContinueWith` 또는 `await`합니다. `bool` 완료 반환 값은 `false` 중단 없이 애니메이션이 완료 되 면 또는 `true` 가 취소 된 경우는 [ `CancelAnimation` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) 에서 시작 하는 모든 애니메이션을 취소 하는 메서드를 다른 `ViewExtensions` 는 동일한 요소에 설정 됩니다.

### <a name="composite-animations"></a>복합 애니메이션

복합 애니메이션을 만드는 대기 및 비 대기 애니메이션을 혼합할 수 있습니다. 애니메이션에서 이들은 `ViewExtensions` 대상으로 하는 합니다 `TranslationX`, `TranslationY`, 및 `Scale` 변환 속성:

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))

있음을 `TranslateTo` 둘 다 영향을 미칠 합니다 `TranslationX` 및 `TranslationY` 속성입니다.

### <a name="taskwhenall-and-taskwhenany"></a>Task.WhenAll 및 Task.WhenAny

사용 하 여 동시 애니메이션을 관리할 수 이기도 [ `Task.WhenAll` ](xref:System.Threading.Tasks.Task.WhenAll*), 여러 작업 모두를 완료 하는 경우는 신호를 보냅니다 및 [ `Task.WhenAny` ](xref:System.Threading.Tasks.Task.WhenAny*)때 신호를 보냅니다는 여러 가지 방법 중 첫 번째 작업 종료 되었습니다.

### <a name="rotation-and-anchors"></a>회전 및 앵커

호출 하는 경우는 `ScaleTo`, `RelScaleTo`, `RotateTo`, 및 `RelRotateTo` 메서드를 설정할 수 있습니다는 [ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX) 및 [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) 속성 나타내기 위해는 크기 조정 및 회전의 중심입니다.

합니다 [ **CircleButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CircleButton) 회전 하 여이 기술을 보여 줍니다는 `Button` 페이지의 가운데를 기준으로 합니다.

### <a name="easing-functions"></a>감속/가속 함수

일반적으로 애니메이션은 선형 시작 값에서 끝 값입니다. 감속/가속 함수 애니메이션 속도를 저하는 과정을 통해 발생할 수 있습니다. 애니메이션 메서드에 마지막 선택적 인수는 형식의 [ `Easing` ](xref:Xamarin.Forms.Easing), 형식의 11 정적 읽기 전용 필드를 정의 하는 클래스 `Easing`:

- [`Linear`](xref:Xamarin.Forms.Easing.Linear)기본값
- [`SinIn`](xref:Xamarin.Forms.Easing.SinIn)하십시오 [ `SinOut` ](xref:Xamarin.Forms.Easing.SinOut), 및 [`SinInOut`](xref:Xamarin.Forms.Easing.SinInOut)
- [`CubicIn`](xref:Xamarin.Forms.Easing.CubicIn)하십시오 [ `CubicOut` ](xref:Xamarin.Forms.Easing.CubicOut), 및 [`CubicInOut`](xref:Xamarin.Forms.Easing.CubicInOut)
- [`BounceIn`](xref:Xamarin.Forms.Easing.BounceIn) 및 [`BounceOut`](xref:Xamarin.Forms.Easing.BounceOut)
- [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) 및 [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut)

`In` 효과 애니메이션의 시작 부분에는 접미사 나타냅니다 `Out` 끝에 의미 및 `InOut` 애니메이션의 시작과 끝에 있음을 의미 합니다.

합니다 [ **BounceButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BounceButton) 감속/가속 함수의 사용법을 보여 줍니다.

### <a name="your-own-easing-functions"></a>사용자 고유의 감속/가속 함수

전달 하 여 감속/가속 함수를 직접 있습니다 정의할 수도 있습니다는 [ `Func<double, double>` ](xref:System.Func`2) 에 [ `Easing` 생성자](xref:Xamarin.Forms.Easing.%23ctor(System.Func{System.Double,System.Double}))합니다. `Easing` 암시적 변환을 정의 `Func<double, double>` 에 `Easing`입니다. 감속/가속 함수에 인수는 애니메이션이 처음부터 끝까지 연속적으로 진행 됨에 따라 0 ~ 1 범위에 항상입니다. 함수 *일반적으로* 0 ~ 1 범위에 값을 반환 하지만 음수 이거나 1 보다 큰 간단 하 게 될 수 있습니다 (경우와 마찬가지로 합니다 `SpringIn` 및 `SpringOut` 함수) 또는 작업을 잘 알고 있는 경우 규칙을 손상 시킬 수 있습니다.

합니다 [ **UneasyScale** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/UneasyScale) 샘플에서는 사용자 지정 감속/가속 함수를 보여 줍니다. 및 [ **CustomCubicEase** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CustomCubicEase) 다른 방법을 보여 줍니다.

[ **SwingButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingButton) 도 보여 줍니다는 사용자 지정 감속/가속 함수 및 변경 하는 기법을 `AnchorX` 및 `AnchorY` 회전 애니메이션 시퀀스 내에서 속성.

합니다 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에는 [ `JiggleButton` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/JiggleButton.cs) 단추를 클릭할 때 jiggle 함수는 사용자 지정 감속/가속을 사용 하는 클래스입니다. 합니다 [ **JiggleButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/JiggleButtonDemo) 샘플 이것을 보여 줍니다.

### <a name="entrance-animations"></a>진입 애니메이션

인기 있는 한 가지 유형의 애니메이션에는 페이지가 처음 나타날 때 발생 합니다. 이러한 애니메이션을 시작할 수 있습니다 합니다 [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) 페이지의 재정의 합니다. 이러한 애니메이션을 표시 하도록 페이지를 원하는 방식에 대 한 XAML을 설정 하는 가장에 대 한 *후* 애니메이션 및 초기화 하 고 레이아웃 코드에서 애니메이션을 적용 합니다.

합니다 [ **FadingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingEntrance) 샘플에서는 합니다 [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 확장 메서드를 페이지의 내용에 페이드입니다.

합니다 [ **SlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SlidingEntrance) 샘플에서는 합니다 [ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) 확장 메서드 측면에서 페이지의 내용에 슬라이드를 합니다.

합니다 [ **SwingingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingingEntrance) 샘플에서는 합니다 [ `RotateYTo` ](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 애니메이션 효과를 주는 확장 메서드는 `RotationY` 속성입니다. A [ `RotateXTo` ](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드 제공 됩니다.

### <a name="forever-animations"></a>영구적으로 애니메이션

다른 한편으로 "forever" 애니메이션 때까지 실행 프로그램이 종료 됩니다. 그 이유는 일반적으로 데모용 위해서입니다.

합니다 [ **FadingTextAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingTextAnimation) 사용 하 여 샘플 [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 두 가지 텍스트 페이드 인 / 아웃 애니메이션 합니다.

[**PalindromeAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/PalindromeAnimation) 역재생, 표시 되 고 다음 순서 대로 개별 문자도 180도 회전 모든 거꾸로 있으므로. 그런 다음 전체 문자열을 원래 문자열과 같은 읽기 180도 반대로 수행 됩니다.

합니다 [ **CopterAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CopterAnimation) 샘플 회전 간단한 `BoxView` 헬리콥터 화면의 가운데를 기준으로 회전 하는 동안.

[**RotatingSpokes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotatingSpokes) 클라이언트나 `BoxView` spoke 화면의 가운데를 기준으로 하 고 다음 자체 주목할 만한 패턴을 만들려면 각 스포크를 회전 합니다.

[![Spoke 회전 하는 triple 스크린샷](images/ch22fg21-small.png "Spoke 회전")](images/ch22fg21-large.png#lightbox "Spoke 회전")

그러나 점진적으로 증가 합니다 `Rotation` 요소의 속성으로은 장기적인 관점에서 작동 하지 않을 [ **RotationBreakdown** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotationBreakdown) 샘플을 보여 줍니다.

합니다 [ **SpinningImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpinningImage) 샘플에서는 [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))하십시오 [ `RotateXTo` ](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), 및 [ `RotateYTo` ](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 3D 공간에서 비트맵 회전은 처럼 보일 수 있도록 하려면.

### <a name="animating-the-bounds-property"></a>Bounds 속성에 애니메이션 적용

에 확장 메서드가 하나만 `ViewExtensions` 는 아직 설명 [ `LayoutTo` ](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)), 효과적으로 애니메이션 효과 적용 읽기 전용 [ `Bounds` ](xref:Xamarin.Forms.VisualElement.Bounds) 호출 하 여 속성을 [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) 메서드. 이 메서드는 일반적으로 호출한 `Layout` 파생형으로 이어지는 [ **26 장입니다. CustomLayouts**](chapter26.md)합니다.

`LayoutTo` 메서드는 특수 목적으로 제한 해야 합니다. [ **BouncingBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BouncingBox) 압축 하 고 확장 프로그램이 사용 하는 `BoxView` 페이지 측면에 튕겨으로 합니다.

합니다 [ **XamagonXuzzle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle) 샘플에서는 `LayoutTo` 이동할 타일의 구현에서 번호가 매겨진된 타일 보다는 암호화 된 이미지를 표시 하는 15 ~ 16 퍼즐:

[![삼중 스크린샷 Xamarin Xuzzle](images/ch22fg26-small.png "Xuzzle 퍼즐 게임")](images/ch22fg26-large.png#lightbox "Xuzzle 퍼즐 게임")

### <a name="your-own-awaitable-animations"></a>사용자 고유의 awaitable 애니메이션

합니다 [ **TryAwaitableAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/TryAwaitableAnimation) 샘플 awaitable 애니메이션을 만듭니다. 반환할 수 있는 중요 한 클래스를 `Task` 메서드 및 신호 애니메이션이 완료 되 면 개체가 [ `TaskCompletionSource` ](xref:System.Threading.Tasks.TaskCompletionSource`1)합니다.

## <a name="deeper-into-animations"></a>애니메이션에 대 한 자세한

Xamarin.Forms 애니메이션 시스템을 약간 혼동 될 수 있습니다. 외에 `Easing` 클래스 애니메이션 시스템 구성 합니다 `ViewExtensions`, `Animation`, 및 `AnimationExtension` 클래스.

### <a name="viewextensions-class"></a>ViewExtensions 클래스

이미 살펴본 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions)합니다. 반환 하는 9 개의 메서드를 정의 하는 것 `Task<bool>` 하 고 [ `CancelAnimations` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement))합니다. 7 9 메서드 대상의 속성을 변환합니다. 다른 두 가지 [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), 대상 합니다 [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity) 속성인 및 [ `LayoutTo` ](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing))를 호출 하는 [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) 메서드.

### <a name="animation-class"></a>애니메이션 클래스

합니다 [ `Animation` ](xref:Xamarin.Forms.AnimationExtensions) 클래스에는 [생성자](xref:Xamarin.Forms.Animation.%23ctor(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Action)) 콜백, 완료 된 메서드 및 애니메이션의 매개 변수를 정의 하는 다섯 개의 인수를 사용 하 여 합니다.

자식 애니메이션으로 추가할 수 있습니다 [ `Add` ](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation))합니다 [ `Insert` ](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation))를 [ `WithConcurrent` ](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double)), 오버 로드 하 고 [ `WithConcurrent` ](xref:Xamarin.Forms.Animation.WithConcurrent(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Double,System.Double)).

다음 애니메이션 개체에 대 한 호출을 사용 하 여 시작 합니다 [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) 메서드.

### <a name="animationextensions-class"></a>AnimationExtensions 클래스

합니다 [ `AnimationExtensions` ](xref:Xamarin.Forms.AnimationExtensions) 클래스 대부분 확장 메서드를 포함 합니다. 여러 버전의 `Animate` 메서드와 제네릭 [ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*) 방법은 하다는 필요한 유일한 애니메이션 함수가 다양 합니다.

## <a name="working-with-the-animation-class"></a>애니메이션 클래스를 사용 하 여 작업

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) 샘플과 합니다 [ `Animation` ](xref:Xamarin.Forms.Animation) 여러 다양 한 애니메이션을 사용 하 여 클래스입니다.

### <a name="child-animations"></a>자식 애니메이션

합니다 [ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) 자식 하 게 하는 애니메이션을 사용 합니다 (매우 유사함)도 보여 [ `Add` ](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)) 고 [ `Insert` ](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation)) 메서드.

### <a name="beyond-the-high-level-animation-methods"></a>고급 애니메이션 방법 외에도

합니다 [ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) 샘플을 대상으로 지정 된 속성을 넘어서는 애니메이션을 수행 하는 방법을 보여 줍니다는 `ViewExtensions` 메서드. 예를 들어, 일련의 기간이 더 길어집니다; 또 다른 예로 `BackgroundColor` 속성에 애니메이션 효과가 적용 됩니다.

### <a name="more-of-your-own-awaitable-methods"></a>더 많은 사용자 고유의 awaitable 메서드

합니다 [ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드의 `ViewExtensions` 작동 하지 않습니다는 [ `Easing.SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut) 함수입니다. 1 위의 감속/가속 출력을 가져올 때 중지 됩니다.

합니다 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에는 [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) 클래스 [ `TranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L12) 및 [ `TranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L49) 이 문제를 없는 확장 메서드 뿐만 [ `CancelTranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L44) 하 고 [ `CancelTranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L71) 이러한 취소 방법 애니메이션 합니다.

합니다 [ **SpringSlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpringSlidingEntrance) 보여 줍니다는 `TranslateXTo` 메서드.

합니다 `MoreExtensions` 클래스도 포함 되어 있습니다를 [ `TranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L76) X 및 Y 번역을 결합 하는 확장 메서드 및 [ `CancelTranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L113) 메서드.

### <a name="implementing-a-bezier-animation"></a>베 지 어 애니메이션을 구현합니다.

요소를 베 지 어 스플라인의 경로 따라 이동 하는 애니메이션을 개발할 수 이기도 합니다. [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리 포함는 [ `BezierSpline` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierSpline.cs) 베 지 어 스플라인을 캡슐화 하는 구조와 [ `BezierTangent` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierTangent.cs) 컨트롤 방향으로 열거형입니다.

합니다 [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) 클래스를 포함 한 [ `BezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L118) 확장 메서드 및 [ `CancelBezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L161) 메서드.

합니다 [ **BezierLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BezierLoop) 샘플 Beizer 경로의 요소에 애니메이션을 적용 하는 방법을 보여 줍니다.

## <a name="working-with-animationextensions"></a>AnimationExtensions 사용

한 가지 유형의 애니메이션 표준 컬렉션에서 누락 된 경우 색 애니메이션 문제는 없는 올바른 방법으로 두 가지 사이 보간 하는 `Color` 값입니다. 개별 RGB 값을 보간 하는 것이 가능 하지만 유효한 것으로 효과 주는 HSL 값입니다.

이러한 이유로 합니다 [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) 클래스를 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에는 두 개의 `Color` 애니메이션 메서드: [ `RgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L166)하 고 [ `HslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L188)합니다. (또한 두 가지가 취소: [ `CancelRgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L183) 하 고 [ `CancelHslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L206)).

두 메서드를 사용 [ `ColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L211), 광범위 한 제네릭을 호출 하 여 애니메이션을 수행 하는 [ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*) 에서 메서드 [ `AnimationExtensions` ](xref:Xamarin.Forms.AnimationExtensions)합니다.

합니다 [ **ColorAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ColorAnimations) 샘플에서는 이러한 두 가지 유형의 색 애니메이션을 사용 하 여 보여 줍니다.

## <a name="structuring-your-animations"></a>애니메이션을 구성

경우에 따라 XAML의 애니메이션 express MVVM과 함께에서 사용 하는 것이 유용 합니다. 다음 장에서 설명 [ **23 장입니다. 트리거 및 동작**](chapter23.md)합니다.

## <a name="related-links"></a>관련 링크

- [22 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)
- [22 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)
- [애니메이션](~/xamarin-forms/user-interface/animation/index.md)
