---
title: 요약 장 22입니다. 애니메이션
description: 'Xamarin.Forms를 사용 하 여 모바일 응용 프로그램 만들기: 22 장 요약 합니다. 애니메이션'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 47C2B9AB-E688-4412-8AF5-9F633B3DA695
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b25fed9a86b82e56cb3b2bf5e3276c8ff63f4e35
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241971"
---
# <a name="summary-of-chapter-22-animation"></a>요약 장 22입니다. 애니메이션

지금까지 살펴본 Xamarin.Forms 타이머를 사용 하 여 사용자 고유의 애니메이션을 만들 수 있습니다 또는 `Task.Delay`, Xamarin.Forms에서 제공 하는 애니메이션 기능을 사용 하 여 어렵거나 하지만 합니다. 세 가지 클래스는 이러한 애니메이션을 구현합니다.

- [`ViewExtensions`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)를 높은 수준의 접근 방식
- [`Animation`](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)어렵다는 문제가 더 융통성
- [`AnimationExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/)를 가장 용도가 넓은 함수로, 낮은 수준의 접근 방식

일반적으로 애니메이션 바인딩 가능한 속성에 의해 지원 되는 속성을 대상으로 합니다. 요구 사항 하지만 동적으로 변경에 반응 하는 유일한 속성.

에 설명 된 기술을 사용 하 여 XAML에 애니메이션을 통합할 수 있습니다 하지만 이러한 애니메이션에 대 한 XAML 인터페이스가 [ **장 23입니다. 트리거 및 동작**](chapter23.md)합니다.

## <a name="exploring-basic-animations"></a>기본적인 애니메이션 탐색

기본 애니메이션 함수는에 확장 메서드는 [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) 클래스입니다. 이러한 메서드는에서 파생 된 모든 개체에 적용 `VisualElement`합니다. 가장 간단한 애니메이션에서 속성에 설명 된 변환 대상 [ `Chapter 21. Transforms` ](chapter21.md)합니다.

[ **AnimationTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/AnimationTryout) 보여 줍니다 방법을 `Clicked` 에 대 한 이벤트 처리기는 `Button` 호출할 수는 [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 회전 하는 데 확장 메서드는 원 안에 단추입니다.

`RotateTo` 방법 변경은 `Rotation` 의 속성은 `Button` 0 ~ 360 기본적으로 1 / 4 초의 과정 동안 합니다. 하지만 경우는 `Button` 는 탭 다시 않으면 때문에 `Rotation` 속성은 이미 360도 합니다.

### <a name="setting-the-animation-duration"></a>애니메이션 지속 시간 설정

두 번째 인수를 `RotateTo` 는 기간 (밀리초)입니다. 경우 큰 값으로 설정 탭에서 `Button` 애니메이션 중 현재 각도에서 시작 하 여 새 애니메이션을 시작 합니다.

### <a name="relative-animations"></a>상대 애니메이션

`RelRotateTo` 메서드 기존 값에 지정된 된 값을 추가 하 여 상대적인 회전을 수행 합니다. 이 방법을 사용 하면는 `Button` 여러 번 하 고 각 누를 수 시간 증가 `Rotation` 360도 속성입니다.

### <a name="awaiting-animations"></a>애니메이션을 대기 중

모든 애니메이션 메서드 `ViewExtensions` 반환 `Task<bool>` 개체입니다. 즉, 일련의 순차적 애니메이션을 사용 하 여 정의할 수 `ContinueWith` 또는 `await`합니다. `bool` 완료 반환 값은 `false` 애니메이션 중단 없이 완료 하는 경우 또는 `true` 으로 취소 되었으면는 [ `CancelAnimation` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) 를 취소 하 여 시작 하는 모든 애니메이션이 메서드는 다른 메서드 `ViewExtensions` 같은 요소에 설정 된입니다.

### <a name="composite-animations"></a>복합 애니메이션

복합 애니메이션을 만드는 대기 중이 던 및 비-대기 애니메이션을 혼합할 수 있습니다. 애니메이션 이들은 `ViewExtensions` 대상으로 하는 `TranslatonX`, `TranslationY`, 및 `Scale` 변환 속성:

- [`TranslateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/)
- [`ScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.ScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)
- [`RelScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)

다음에 유의 `TranslateTo` 둘 다 영향을 미칠는 `TranslationX` 및 `TranslationY` 속성입니다.

### <a name="taskwhenall-and-taskwhenany"></a>Task.WhenAll 및 Task.WhenAny

사용 하 여 동시 애니메이션을 관리할 수 이기도 [ `Task.WhenAll` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.WhenAll%7BTResult%7D/p/System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7BTResult%7D%7D/), 여러 작업 모두를 완료 하는 경우 신호를 및 [ `Task.WhenAny` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.WhenAny%7BTResult%7D/p/System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7BTResult%7D%7D/)때 신호를 보냅니다는 여러 가지 방법 중 첫 번째 가 작업을 완료 합니다.

### <a name="rotation-and-anchors"></a>회전과 앵커

호출할 때는 `ScaleTo`, `RelScaleTo`, `RotateTo`, 및 `RelRotateTo` 메서드를 설정할 수 있습니다는 [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) 및 [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) 를 나타내는 속성의 크기 조정 및 회전의 중심입니다.

[ **CircleButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CircleButton) 회전 하 여이 기술을 보여 줍니다.는 `Button` 페이지의 중심 합니다.

### <a name="easing-functions"></a>감속/가속 함수

일반적으로 애니메이션은 선형 시작 값과에서 끝 값입니다. 감속/가속 함수 애니메이션 속도를 해당 과정 속도가 저하 될 수 있습니다. 애니메이션 메서드에 마지막 선택적 인수는 형식의 [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/), 형식의 11 정적 읽기 전용 필드를 정의 하는 클래스 `Easing`:

- [`Linear`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/)기본값
- [`SinIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinIn/)[ `SinOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinOut/), 및 [`SinInOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinInOut/)
- [`CubicIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicIn/)[ `CubicOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicOut/), 및 [`CubicInOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicInOut/)
- [`BounceIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceIn/) 및 [`BounceOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceOut/)
- [`SpringIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) 및 [`SpringOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/)

`In` 접미사 효과 애니메이션의 시작 부분에 임을 나타냅니다. `Out` 끝나도 의미 및 `InOut` 시작과 애니메이션의 끝에는 있다는 것을 의미 합니다.

[ **BounceButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BounceButton) 샘플 감속/가속 함수의 사용법을 보여줍니다.

### <a name="your-own-easing-functions"></a>감속/가속 함수

정의할 수도 있습니다 있습니다 감속/가속 함수를 직접 전달 하 여 한 [ `Func<double, double>` ](https://developer.xamarin.com/api/type/System.Func%3CT,TResult%3E/) 에 [ `Easing` 생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Easing.Easing/p/System.Func%7BSystem.Double,System.Double%7D/)합니다. `Easing` 암시적 변환이 정의 `Func<double, double>` 를 `Easing`합니다. 감속/가속 함수 인수는 애니메이션 처음부터 끝까지 선형 진행 됨에 따라 0 ~ 1 범위에 항상입니다. 함수 *일반적으로* 0 ~ 1 범위에 값을 반환 하지만 음수 또는 1 보다 큰 간단 하 게 될 수 없습니다 (경우와 마찬가지로 `SpringIn` 및 `SpringOut` 함수) 또는 무엇을 알고 있는 경우 규칙을 손상 시킬 수 있습니다.

[ **UneasyScale** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/UneasyScale) 샘플 사용자 지정 감속/가속 함수를 보여 줍니다. 및 [ **CustomCubicEase** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CustomCubicEase) 다른 방법을 보여 줍니다.

[ **SwingButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingButton) 도 보여 사용자 지정 감속/가속 함수 및 변경의 기술을 `AnchorX` 및 `AnchorY` 회전 애니메이션 시퀀스 내에서 속성입니다.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에는 [ `JiggleButton` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/JiggleButton.cs) 클래스는 사용자 지정 감속/가속을 사용 하는 단추를 클릭할 때 jiggle를 작동 합니다. [ **JiggleButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/JiggleButtonDemo) 샘플 것을 보여 줍니다.

### <a name="entrance-animations"></a>진입 애니메이션

애니메이션의 한 인기 있는 형식에는 페이지가 처음 나타날 때 발생 합니다. 이러한 애니메이션 시작 될 수 있습니다는 [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) 페이지의 재정의 합니다. 이러한 애니메이션을 표시 되도록 페이지를 원하는 방식에 대 한 XAML를 설정 하려면 최적화에 대 한 *후* 애니메이션, 다음을 초기화 하 고 코드에서 레이아웃에 애니메이션을 적용 합니다.

[ **FadingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingEntrance) 샘플에서는 [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 확장 메서드는 페이지의 내용에 페이드 인 됩니다.

[ **SlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SlidingEntrance) 샘플에서는 [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) 확장 메서드를 측면에서 페이지의 내용에 밉니다.

[ **SwingingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingingEntrance) 샘플에서는 [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 애니메이션 효과를 주는 확장 메서드는 `RotationY` 속성입니다. A [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 메서드는 또한 사용할 수 있습니다.

### <a name="forever-animations"></a>영원히 애니메이션

다른 한편 "forever" 애니메이션 실행할 프로그램이 종료 되 고 있습니다. 이러한 일반적으로 오버 로드는 데모 목적으로 합니다.

[ **FadingTextAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingTextAnimation) 샘플에서는 [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 라는 두 가지 텍스트 페이드 인 / 아웃 애니메이션 합니다.

[**PalindromeAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/PalindromeAnimation) 역재생을 표시 하 고 다음 순서 대로 개별 문자도 180도 회전 하므로 모든 거꾸로 하기가 합니다. 그런 다음 전체 문자열 대칭 이동 180도 원래 문자열과 같은 읽을 수 있습니다.

[ **CopterAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CopterAnimation) 샘플 회전 하는 간단한 `BoxView` 화면 중심 회전 하는 동안 헬리콥터 합니다.

[**RotatingSpokes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotatingSpokes) 회전 `BoxView` 화면 중심 spoke 하 고 다음 각 스포크 자체 흥미로운 패턴을 만들 수 있습니다.

[![Spoke 회전의 삼중 스크린 샷](images/ch22fg21-small.png "Spoke 회전")](images/ch22fg21-large.png#lightbox "Spoke 회전")

그러나 점진적으로 증가 `Rotation` 요소의 속성으로는 장기적으로 작동 하지 않을 수는 [ **RotationBreakdown** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotationBreakdown) 샘플을 보여 줍니다.

[ **SpinningImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpinningImage) 샘플에서는 [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), 및 [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 3D 공간에서 비트맵 회전 처럼 보일 되도록 합니다.

### <a name="animating-the-bounds-property"></a>범위 속성에 애니메이션

유일한 확장 메서드에 `ViewExtensions` 는 설명 하지 않은 [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/)를 효과적으로 애니메이션 효과 적용 읽기 전용 [ `Bounds` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Bounds/) 호출 하 여는 [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) 메서드. 이 메서드는 일반적으로 하 여 호출 `Layout` 파생 항목에서 논의 것 처럼 [ **장 26입니다. CustomLayouts**](chapter26.md)합니다.

`LayoutTo` 메서드는 특수 한 용도로 수 있도록 제한 해야 합니다. [ **BouncingBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BouncingBox) 압축 하 여 확장 프로그램 사용 하는 `BoxView` 페이지의 양쪽 측면 오프 반송으로 합니다.

[ **XamagonXuzzle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle) 샘플에서는 `LayoutTo` 이동 하려면 타일의 기본 구현에서 번호가 매겨진된 타일 대신 암호화 된 이미지를 표시 하는 15-16 퍼즐:

[![Xamarin Xuzzle의 삼중 스크린 샷](images/ch22fg26-small.png "Xuzzle 퍼즐 게임")](images/ch22fg26-large.png#lightbox "Xuzzle 퍼즐 게임")

### <a name="your-own-awaitable-animations"></a>고유한 awaitable 애니메이션

[ **TryAwaitableAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/TryAwaitableAnimation) 샘플은 awaitable 애니메이션을 만듭니다. 반환할 수 있는 중요 한 클래스는 `Task` 메서드 및 신호 애니메이션이 완료 되 면 개체는 [ `TaskCompletionSource` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.TaskCompletionSource%3CTResult%3E/)합니다.

## <a name="deeper-into-animations"></a>애니메이션을 자세하게

Xamarin.Forms 애니메이션 시스템 약간 복잡할 수 있습니다. 외에 `Easing` 클래스 애니메이션 시스템 구성의 `ViewExtensions`, `Animation`, 및 `AnimationExtension` classses 합니다.

### <a name="viewextensions-class"></a>ViewExtensions 클래스

이미 본 [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)합니다. 반환 하는 9 개의 메서드를 정의 `Task<bool>` 및 [ `CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/)합니다. 9 개의 메서드 대상 중 7 변환 속성입니다. 다른 두 [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), 대상은 [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) 속성 및 [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/)를 호출 하는 [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) 메서드.

### <a name="animation-class"></a>애니메이션 클래스

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) 클래스에는 [생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Animation.Animation/p/System.Action%7BSystem.Double%7D/System.Double/System.Double/Xamarin.Forms.Easing/System.Action/) 콜백 및 완료 된 메서드와 애니메이션의 매개 변수를 정의 하려면 5 개 인수를 사용 합니다.

자식 애니메이션으로 추가할 수 있습니다 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/), [ `Insert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Insert/p/System.Double/System.Double/Xamarin.Forms.Animation/), [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/Xamarin.Forms.Animation/System.Double/System.Double/), 및 오버 로드 및 [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/System.Action%7BSystem.Double%7D/System.Double/System.Double/Xamarin.Forms.Easing/System.Double/System.Double/).

애니메이션 개체를 호출 하 여 시작 됩니다는 [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BSystem.Double,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) 메서드.

### <a name="animationextensions-class"></a>AnimationExtensions 클래스

[ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) 클래스 대부분 확장 메서드를 포함 합니다. 여러 버전의 `Animate` 메서드와 제네릭 [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate%7BT%7D/p/Xamarin.Forms.IAnimatable/System.String/System.Func%7BSystem.Double,T%7D/System.Action%7BT%7D/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BT,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) 방법은 인지 정말 필요한 유일한 애니메이션 함수 다양 합니다.

## <a name="working-with-the-animation-class"></a>애니메이션 클래스 사용

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) 샘플는 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) 몇 개의 서로 다른 애니메이션을 사용 하 여 클래스입니다.

### <a name="child-animations"></a>자식 애니메이션

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) 자식 하도록 하는 애니메이션 (매우 유사한)의 사용도 보여 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/) 및 [ `Insert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Insert/p/System.Double/System.Double/Xamarin.Forms.Animation/) 메서드.

### <a name="beyond-the-high-level-animation-methods"></a>높은 수준의 애니메이션 메서드 외

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) 또한 샘플에는 속성 대상이 이외에 애니메이션을 수행 하는 방법을 보여 줍니다는 `ViewExtensions` 메서드. 한 가지 예는 일련의 기간 전달 되는 더 깁니다. 또 다른 예로 `BackgroundColor` 속성에 애니메이션 효과가 적용 됩니다.

### <a name="more-of-your-own-awaitable-methods"></a>이외의 고유한 awaitable 메서드

[ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) 방식의 `ViewExtensions` 작동 하지는 [ `Easing.SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) 함수입니다. 보다 작거나 1 보다 감속/가속 출력을 가져올 때 중지 합니다.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에 포함 되어는 [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) 클래스와 [ `TranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L12) 및 [ `TranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L49) 이 문제를 갖지 않는 확장 메서드 및 [ `CancelTranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L44) 및 [ `CancelTranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L71) 취소 하는 것에 대 한 메서드 애니메이션 합니다.

[ **SpringSlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpringSlidingEntrance) 보여 줍니다는 `TranslateXTo` 메서드.

`MoreExtensions` 클래스도 포함 되어는 [ `TranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L76) X 및 Y 번역을 결합 하는 확장 메서드 및 [ `CancelTranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L113) 메서드.

### <a name="implementing-a-bezier-animation"></a>베 지 어 애니메이션을 구현합니다.

요소는 베 지 어 스플라인의 경로 따라 이동 하는 애니메이션을 개발 하는 것도 가능 합니다. [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에 포함 되어는 [ `BezierSpline` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierSpline.cs) 되는 베 지 어 스플라인을 캡슐화 하 고 [ `BezierTangent` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierTangent.cs) 방향 제어 하는 열거형입니다.

[ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) 클래스를 포함 한 [ `BezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L118) 확장 메서드 및 [ `CancelBezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L161) 메서드.

[ **BezierLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BezierLoop) 샘플 Beizer 경로 따라 요소에 애니메이션을 적용 하는 방법을 보여 줍니다.

## <a name="working-with-animationextensions"></a>AnimationExtensions 작업

한 가지 유형의 표준 컬렉션에서 누락 된 애니메이션은 색 애니메이션 합니다. 문제는 오른쪽 두 보간할 수 없지만 한지 `Color` 값입니다. 개별 RGB 값 보간할 수 있지만 HSL 값을 보간는 방금 유효 합니다.

이러한 이유로 [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) 클래스에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에는 두 개의 `Color` 애니메이션 메서드: [ `RgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L166)및 [ `HslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L188)합니다. (또한 두 가지가 취소: [ `CancelRgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L183) 및 [ `CancelHslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L206)).

두 방법 모두 확인 사용 [ `ColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L211), 광범위 한 제네릭을 호출 하 여 애니메이션을 수행 하는 [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate%7BT%7D/p/Xamarin.Forms.IAnimatable/System.String/System.Func%7BSystem.Double,T%7D/System.Action%7BT%7D/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BT,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) 에서 메서드 [ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/)합니다.

[ **ColorAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ColorAnimations) 샘플에서는 이러한 두 가지 유형의 색 애니메이션을 사용 하 여 보여 줍니다.

## <a name="structuring-your-animations"></a>애니메이션의 구조를 지정

경우에 따라 XAML의 애니메이션을 표현 하 MVVM과 함께에서 사용 하 여는 것이 유용 합니다. 이 내용은 다음 장에서 설명 [ **장 23입니다. 트리거 및 동작**](chapter23.md)합니다.



## <a name="related-links"></a>관련 링크

- [22 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)
- [22 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)
- [애니메이션](~/xamarin-forms/user-interface/animation/index.md)
