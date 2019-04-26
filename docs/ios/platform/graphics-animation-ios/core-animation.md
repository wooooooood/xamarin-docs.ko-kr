---
title: Xamarin.iOS에서 핵심 애니메이션
description: 이 아티클에서 하위 수준의 애니메이션 컨트롤에 대 한 직접 사용 하려면 고성능 UIKit, 방법과 부드러운 애니메이션을 사용 하는 방법을 보여 주는 핵심 애니메이션 프레임 워크를 살펴봅니다.
ms.prod: xamarin
ms.assetid: D4744147-FACB-415B-8155-3A6B3C35E527
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: a40d0911b7dabc900a4c6e50c692e4f091f22be9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61206364"
---
# <a name="core-animation-in-xamarinios"></a>Xamarin.iOS에서 핵심 애니메이션

_이 아티클에서 하위 수준의 애니메이션 컨트롤에 대 한 직접 사용 하려면 고성능 UIKit, 방법과 부드러운 애니메이션을 사용 하는 방법을 보여 주는 핵심 애니메이션 프레임 워크를 살펴봅니다._

iOS를 포함 [ *Core Animation* ](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html) 애니메이션 지원 하기 위해 응용 프로그램에서 뷰.
모든 테이블의 스크롤 및 다른 뷰 간에 살짝 같은 iOS에서 매우 부드러운 애니메이션 뿐만 아니라 핵심 애니메이션을 내부적으로 의존 하기 때문에 그럴 수행 합니다.

핵심 애니메이션 및 핵심 그래픽 프레임 워크를 함께 작동 하 여 사용 가능한 멋진 만들려면 2D 그래픽 애니메이션을 적용 합니다. 사실 핵심 애니메이션도 변형할 수 3D 공간에서 2D 그래픽 놀라운 시네마 환경을 만들기. 그러나 true 3D 그래픽을 만들려면 해야 사용할지 것 같은 OpenGL ES 게임 차례 MonoGame 등의 API에 대 한 3D은이 문서의 범위를 벗어납니다.

<a name="Using_Core_Animation" />

## <a name="core-animation"></a>핵심 애니메이션

iOS 핵심 애니메이션 프레임 워크를 사용 하 여 뷰 간을 전환, 메뉴 슬라이딩, 효과 등 스크롤 등 애니메이션 효과 만듭니다. 애니메이션을 사용 하는 방법은 두 가지 있습니다.

- [UIKit 통해](#Using_UIKit_Animation)를 보기 기반 애니메이션 뿐만 아니라 컨트롤러 간의 애니메이션 된 전환이 포함 합니다.
- [코어 애니메이션을 통해](#Using_Core_Animation)를 세부적으로 제어할 수 있습니다는 계층을 직접적으로 합니다.

<a name="Using_UIKit_Animation" />

## <a name="using-uikit-animation"></a>UIKit 애니메이션을 사용 하 여

UIKit 쉽게 애니메이션 응용 프로그램을 추가 하는 몇 가지 기능을 제공 합니다. 코어 애니메이션을 내부적으로 사용 하지만 추상화 것 뷰 및 컨트롤러에만 작동 합니다.

이 섹션에서는 UIKit 애니메이션 기능을 포함 하 여 설명 합니다.

-  컨트롤러 전환
-  뷰 전환
-  보기 속성 애니메이션


### <a name="view-controller-transitions"></a>뷰 컨트롤러 전환

 `UIViewController` 기본 제공 지원을 통해 뷰 컨트롤러 간의 전환에 대 한 제공 된 `PresentViewController` 메서드. 사용 하는 경우 `PresentViewController`, 두 번째 컨트롤러 전환 필요에 따라 애니메이션을 적용할 수 있습니다.

예를 들어 첫 번째 컨트롤러에서 단추를 호출 하는 경우 두 컨트롤러를 사용 하 여 응용 프로그램 `PresentViewController` 두 번째 컨트롤러를 표시 합니다. 어떤 전환 애니메이션은 두 번째 컨트롤러를 표시 하는 데을 제어 하려면 설정 하기만 하면 해당 [ `ModalTransitionStyle` ](xref:UIKit.UIModalTransitionStyle) 아래와 같이 속성:

```csharp
SecondViewController vc2 = new SecondViewController {
    ModalTransitionStyle = UIModalTransitionStyle.PartialCurl
};
```

이 경우에 `PartialCurl` 비롯 하 여 여러 다른 이지만 애니메이션 사용 됩니다.

-  `CoverVertical` – 슬라이드를 화면 맨 아래에서
-  `CrossDissolve` – 이전 보기에서 페이드 아웃 및 페이드 인 새 보기
-  `FlipHorizontal` 오른쪽에서 왼쪽-가로 대칭 이동 합니다. 사건의에서 전환에는 왼쪽에서 오른쪽 대칭 이동합니다.


전환에 애니메이션 효과를 주는 전달 `true` 두 번째 인수로 `PresentViewController`:

```csharp
PresentViewController (vc2, true, null);
```

다음 스크린샷은 대 한 전환 모양은 `PartialCurl` 사례:

 ![](core-animation-images/06-view-transitions.png "이 스크린샷은 PartialCurl 전환")

### <a name="view-transitions"></a>보기 전환

UIKit은 컨트롤러 간에 전환 하는 것 외에도 다른 한 보기를 교환 하는 뷰 간의 애니메이션 전환을 지원 합니다.

예를 들어 컨트롤러를 갖고 가정해 `UIImageView`이미지 탭 잠시 표시 되는, `UIImageView`합니다. 이미지에 애니메이션 효과를 보기 뷰의 두 번째 이미지 보기로 전환 하는 것 만큼 간단 호출 `UIView.Transition`를 전달 합니다 `toView` 고 `fromView` 아래와 같이:

```csharp
UIView.Transition (
    fromView: view1,
    toView: view2,
    duration: 2,
    options: UIViewAnimationOptions.TransitionFlipFromTop |
        UIViewAnimationOptions.CurveEaseInOut,
    completion: () => { Console.WriteLine ("transition complete"); });
```

`UIView.Transition` 도 사용을 `duration` 애니메이션 실행 기간을 제어 하는 매개 변수 뿐만 [ `options` ](xref:UIKit.UIViewAnimationOptions) 감속/가속 함수를 사용 하 여 애니메이션 등을 지정할 수 있습니다. 또한 애니메이션이 완료 되 면 호출 되는 완료 처리기를 지정할 수 있습니다.

경우 표시 아래 스크린샷에서 애니메이션된 전환 이미지 뷰 `TransitionFlipFromTop` 사용 됩니다.

 ![](core-animation-images/07-animated-transition.png "TransitionFlipFromTop 사용 되는 경우 이미지 뷰 간을 전환 애니메이션된을 보여 주는이 스크린샷")

### <a name="view-property-animations"></a>보기 속성 애니메이션

UIKit 다양 한 속성에 애니메이션 적용을 지원 합니다 `UIView` 포함 하 여 무료로 클래스:

-  프레임
-  범위
-  가운데 맞춤
-  알파
-  변환
-  색


이러한 애니메이션의 속성 변경 내용을 지정 하 여 암시적으로 발생을 `NSAction` 대리자가 정적 전달할 `UIView.Animate` 메서드. 예를 들어, 다음 코드는 애니메이션의 중심점을 `UIImageView`:

```csharp
pt = imgView.Center;

UIView.Animate (
    duration: 2, 
    delay: 0, 
    options: UIViewAnimationOptions.CurveEaseInOut | 
        UIViewAnimationOptions.Autoreverse,
    animation: () => {
        imgView.Center = new CGPoint (View.Bounds.GetMaxX () 
            - imgView.Frame.Width / 2, pt.Y);},
    completion: () => {
        imgView.Center = pt; }
);
```

그 결과 이미지에 애니메이션 적용 앞뒤로 화면 맨 아래와 같이.

 ![](core-animation-images/08-animate-center.png "이미지에 애니메이션 적용 앞뒤로 화면의 위쪽 출력으로")

와 마찬가지로 합니다 `Transition` 메서드를 `Animate` 기간 감속/가속 함수를 함께 설정할 수 있습니다. 사용이 예제를 `UIViewAnimationOptions.Autoreverse` 애니메이션이 처음으로 값에서 애니메이션 효과를 주는 하는 옵션입니다. 그러나 코드를 설정 합니다 `Center` 완료 처리기에서 초기 값으로 다시 합니다. 애니메이션 효과 시간에 따라 속성 값은, 하는 동안 속성의 실제 모델 값은 항상 설정 된 마지막 값입니다. 이 예에서 값은 슈퍼 뷰의 오른쪽 근처에서 지점. 설정 하지 않고 합니다 `Center` 애니메이션으로 인해 완료 되는 위치는 초기 지점에 `Autoreverse` 설정 되는 이미지를 넣었습니다 다시 왼쪽에서 오른쪽으로 애니메이션이 완료 되 면 아래와 같이:

 ![](core-animation-images/09-animation-complete.png "초기 지점을 중심으로 설정 하지 않고 이미지를 넣었습니다 다시 왼쪽에서 오른쪽으로 애니메이션이 완료 된 후")

## <a name="using-core-animation"></a>코어 애니메이션을 사용 하 여

 `UIView` 애니메이션 기능을 많이 허용 및 구현의 용이성으로 인해 가능한 경우 사용 해야 합니다. 앞에서 설명한 대로 UIView 애니메이션 핵심 애니메이션 프레임 워크를 사용 합니다. 그러나 사용 하 여 몇 가지 작업을 수행할 수 없습니다 `UIView` 뷰를 사용 하 여 애니메이션을 적용할 수 있는 추가 속성에 애니메이션 효과 주는 비선형 경로 따라 등의 애니메이션. 이러한 경우 보다 세부적으로 제어 해야 하는 핵심 애니메이션도 직접 사용할 수 있습니다.

### <a name="layers"></a>계층

애니메이션을 통해 발생 하는 핵심 애니메이션에서 작업할 때는 *레이어*, 유형을 `CALayer`합니다. 계층 계층 훨씬 뷰 계층 구조를 제공 되는 계층은 뷰를 개념적으로 비슷합니다입니다. 실제로 계층에는 사용자 상호 작용에 대 한 지원을 추가 하는 뷰를 사용 하 여 뷰를 백업 합니다. 뷰를 통해 뷰의 계층에 액세스할 수 있습니다 `Layer` 속성입니다. 컨텍스트가에서 사용 되는 사실 `Draw` 메서드의 `UIView` 계층에서 실제로 만들어집니다. 내부적으로 지 원하는 계층을 `UIView` 의 뷰 자체에, 새로운 호출 되는 설정 하는 대리자가 `Draw`합니다. 그릴 때를 `UIView`, 해당 계층에 그리고 실제로 있는 합니다.

암시적 또는 명시적 계층 애니메이션 수 있습니다. 암시적 애니메이션은 선언적 합니다. 변경할지 계층 속성 선언 및 애니메이션 작동 합니다. 반면에 애니메이션 명시적 계층에 추가 되는 애니메이션 클래스를 통해 생성 됩니다. 명시적 애니메이션 애니메이션을 만드는 방법에 대 한 추가 제어를 허용 합니다. 다음 섹션에서는 암시적 및 명시적 애니메이션을 더 자세히 살펴보고 있습니다.

### <a name="implicit-animations"></a>암시적 애니메이션

계층의 속성에 애니메이션을 적용 하는 한 방법은 암시적 애니메이션을 통해 것입니다. `UIView` 애니메이션 암시적 애니메이션을 만듭니다. 그러나 암시적 애니메이션도 계층에 대해 직접 만들 수 있습니다.

다음 코드 계층을 설정 하는 예를 들어 `Contents` 이미지에서 색과 테두리 두께 설정 하 고 뷰의 계층의 하위 계층을 추가 합니다.

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    layer = new CALayer ();
    layer.Bounds = new CGRect (0, 0, 50, 50);
    layer.Position = new CGPoint (50, 50);
    layer.Contents = UIImage.FromFile ("monkey2.png").CGImage;
    layer.ContentsGravity = CALayer.GravityResize;
    layer.BorderWidth = 1.5f;
    layer.BorderColor = UIColor.Green.CGColor;

    View.Layer.AddSublayer (layer);
}
```

계층에 대 한 암시적 애니메이션을 추가 하려면 단순히 속성 변경 내용을 래핑하는 `CATransaction`합니다. 이렇게 하면 수 없는 보기 애니메이션을 사용 하 여 애니메이션 효과 줄과 같은 속성에 애니메이션을 적용 합니다 `BorderWidth` 및 `BorderColor` 아래와 같이:

```csharp
public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);

    CATransaction.Begin ();
    CATransaction.AnimationDuration = 10;
    layer.Position = new CGPoint (50, 400);
    layer.BorderWidth = 5.0f;
    layer.BorderColor = UIColor.Red.CGColor;
    CATransaction.Commit ();
}
```

이 코드는 또한 레이어의 애니메이션 `Position`, superlayer의 좌표의 왼쪽 위에서 측정 레이어의 앵커 지점의 위치입니다. 계층의 앵커 지점에는 계층의 좌표 시스템 내에서 정규화 된 지점이입니다.

다음 그림에는 위치 및 앵커 지점을 보여 줍니다.

 ![](core-animation-images/10-postion-anchorpt.png "그림: 위치 및 앵커 지점")

예제를 실행 합니다 `Position`, `BorderWidth` 및 `BorderColor` 다음 스크린샷과에서 같이 애니메이션 효과 주기:

 ![](core-animation-images/11-implicit-animation.png "표시 된 것 처럼 위치, BorderWidth 및 BorderColor 애니메이션 예제를 실행 하는 경우")

### <a name="explicit-animations"></a>명시적 애니메이션

핵심 애니메이션에 다양 한 클래스에서 상속 되는 암시적 애니메이션 하는 것 외에도 `CAAnimation` 데 사용 되는 계층에 명시적으로 추가 됩니다는 애니메이션을 캡슐화 합니다. 애니메이션의 시작 값을 수정, 애니메이션 그룹화, 키 프레임 비선형 경로를 허용 하도록 지정 등의 애니메이션을 통해 세부적으로 제어할을 수 있습니다.

다음 코드를 사용 하 여 명시적 애니메이션의 예를 보여 줍니다.는 `CAKeyframeAnimation` 앞부분에 표시 된 (암시적 애니메이션 섹션 참조) 계층에 대 한 합니다.

```csharp
public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);
    
    // get the initial value to start the animation from
    CGPoint fromPt = layer.Position;
    
    /* set the position to coincide with the final animation value
    to prevent it from snapping back to the starting position
    after the animation completes*/
    layer.Position = new CGPoint (200, 300);
    
    // create a path for the animation to follow
    CGPath path = new CGPath ();
    path.AddLines (new CGPoint[] { fromPt, new CGPoint (50, 300), new CGPoint (200, 50), new CGPoint (200, 300) });
    
    // create a keyframe animation for the position using the path
    CAKeyFrameAnimation animPosition = (CAKeyFrameAnimation)CAKeyFrameAnimation.FromKeyPath ("position");
    animPosition.Path = path;
    animPosition.Duration = 2;
    
    // add the animation to the layer.
    /* the "position" key is used to overwrite the implicit animation created
    when the layer positino is set above*/
    layer.AddAnimation (animPosition, "position");
}
```

이 코드를 변경 합니다 `Position` 키 프레임 애니메이션을 정의 하는을 사용 하는 경로 만들어 계층입니다. 레이어의 `Position` 의 최종 값으로 설정 됩니다는 `Position` 애니메이션에서 합니다. 이렇게 하지 않으면 계층 갑자기 반환 하려면 해당 `Position` 애니메이션 하기 전에 애니메이션 프레젠테이션 값 및 실제 모델 값이 아닌만 변경 되기 때문에 있습니다. 계층 모델 값은 최종 값을 애니메이션에서로 설정 하면 애니메이션의 끝에는 그대로 유지 됩니다.

다음 스크린샷에서 이미지에 애니메이션 적용 된 지정 된 경로 통해 포함 된 레이어를 보여 줍니다.

 ![](core-animation-images/12-explicit-animation.png "지정 된 경로 통해 이미지에 애니메이션 적용을 포함 하는 계층을 보여 주는이 스크린샷")
 
## <a name="summary"></a>요약

이 문서에 대해 살펴보았습니다 애니메이션 기능을 통해 제공 합니다 *Core Animation* 프레임 워크입니다. 코어 애니메이션 UIKit, 애니메이션을 구동 하는 하는 방법 및 방법을 하위 수준 애니메이션 컨트롤에 대 한 직접 사용할 수를 보여 주는 검사 했습니다.

## <a name="related-links"></a>관련 링크

- [코어 애니메이션 샘플](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [핵심 그래픽](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [그래픽 및 애니메이션 연습](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [핵심 애니메이션](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
