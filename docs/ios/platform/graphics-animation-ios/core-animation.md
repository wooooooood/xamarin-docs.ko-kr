---
title: "코어 애니메이션"
description: "이 문서는 고성능 방법 UIKit에서 유동 애니메이션 하위 애니메이션 컨트롤에 직접 사용 하도록 사용 방법을 보여 주는 핵심 애니메이션 프레임 워크를 검사 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: D4744147-FACB-415B-8155-3A6B3C35E527
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f0cb4e00abffead854c2590bde6df45c200ff0bb
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="core-animation"></a>코어 애니메이션

_이 문서는 고성능 방법 UIKit에서 유동 애니메이션 하위 애니메이션 컨트롤에 직접 사용 하도록 사용 방법을 보여 주는 핵심 애니메이션 프레임 워크를 검사 합니다._

iOS 포함 [ *코어 애니메이션* ](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html) 지원을 제공 하기 위해 애니메이션 응용 프로그램의 뷰에 대 한 합니다.
테이블의 스크롤 및 다른 뷰 간에 넘기기가 같은 iOS에서 엄청나게 부드러운 애니메이션의 모든 핵심 애니메이션에 내부적으로 사용 하기 위한 것도 수행 합니다.

핵심 애니메이션 그래픽과 핵심 프레임 워크 beautiful, 만들려는 함께 작업할 수 2 차원 그래픽을 애니메이션 효과가 적용 합니다. 실제로 코어 애니메이션도 변형할 수 3D 공간에서 2 차원 그래픽 놀라운, 시네마 환경을 만들기. 그러나 true 3D 그래픽을 만들려는 해야 OpenGL ES 또는 게임 나아가서는 MonoGame, 등의 API에 대 한 형식을 사용 하려면 3D은이 문서의 범위를 벗어납니다.

<a name="Using_Core_Animation" />

## <a name="core-animation"></a>코어 애니메이션

iOS 코어 애니메이션 프레임 워크를 사용 하 여와 같은 보기 간에 전환 되 고, 메뉴 슬라이딩 및 스크롤 효과에 애니메이션 효과 만듭니다. 애니메이션을 사용 하는 방법은 두 가지가 있습니다.

- [UIKit 통해](#Using_UIKit_Animation), 애니메이션 뷰 기반 뿐만 아니라 컨트롤러 간의 애니메이션된 전환을 포함 합니다.
- [코어 애니메이션을 통해](#Using_Core_Animation)에 대 한 세부적으로 제어할 수 있도록 레이어를 직접 합니다.

<a name="Using_UIKit_Animation" />

## <a name="using-uikit-animation"></a>UIKit 애니메이션을 사용 하 여

UIKit 쉽게 애니메이션 응용 프로그램을 추가할 수는 몇 가지 기능을 제공 합니다. 코어 애니메이션을 내부적으로 사용 되지만 것에서 벗어난 것 때문에 뷰 및 컨트롤러 에서만 작동 합니다.

이 섹션에서는 UIKit 애니메이션 기능을 포함 하 여 설명 합니다.

-  컨트롤러 간의 전환
-  보기 간 전환
-  보기 속성이 애니메이션


### <a name="view-controller-transitions"></a>보기 컨트롤러 전환

 `UIViewController` 기본적으로 제공을 통해 컨트롤러 보기 간 전환을 위한는 `PresentViewController` 메서드. 사용 하는 경우 `PresentViewController`, 두 번째 컨트롤러로의 전환을 선택적으로 애니메이션을 적용할 수 있습니다.

예를 들어 첫 번째 컨트롤러에서 단추를 호출 하는 두 명의 컨트롤러와 응용 프로그램 `PresentViewController` 두 번째 컨트롤러를 표시 합니다. 어떤 전환 애니메이션은 두 번째 컨트롤러를 표시 하는 데를 제어 하려면 설정 하면 해당 [ `ModalTransitionStyle` ](https://developer.xamarin.com/api/type/UIKit.UIModalTransitionStyle/) 아래와 같이 속성:

```csharp
SecondViewController vc2 = new SecondViewController {
    ModalTransitionStyle = UIModalTransitionStyle.PartialCurl
};
```

이 경우에 `PartialCurl` 사용할 수 있는 포함 하는 여러 다른 애니메이션 사용 됩니다.

-  `CoverVertical` – 슬라이드를 화면 아래쪽에서
-  `CrossDissolve` – 이전 뷰 페이드 아웃 & 페이드 인 새 보기
-  `FlipHorizontal` -가로 오른쪽에서 왼쪽으로 대칭 이동 합니다. 기에서 전환에는 왼쪽에서 오른쪽 대칭 이동합니다.


전환에 애니메이션 효과 적용 하려면 전달 `true` 에 대 한 두 번째 인수로 `PresentViewController`:

```csharp
PresentViewController (vc2, true, null);
```

다음 스크린 샷에서 대 한 전환 모양을 보여 줍니다.는 `PartialCurl` 사례:

 ![](core-animation-images/06-view-transitions.png "이 스크린샷은 PartialCurl 전환")

### <a name="view-transitions"></a>뷰 전환

UIKit은 컨트롤러 간의 전환 뿐 아니라 다른 보기를 교체 하는 뷰 간의 애니메이션 효과 주기 전환을 지원 합니다.

예를 들어 컨트롤러를 갖고 있다고 `UIImageView`, 이미지에는 누르기 초를 표시 해야 `UIImageView`합니다. 보기의 슈퍼 뷰의 두 번째 이미지 보기로 전환 하는 호출 만큼 간단 이미지 애니메이션 효과를 주는 `UIView.Transition`, 전달 된 `toView` 및 `fromView` 아래와 같이:

```csharp
UIView.Transition (
    fromView: view1,
    toView: view2,
    duration: 2,
    options: UIViewAnimationOptions.TransitionFlipFromTop |
        UIViewAnimationOptions.CurveEaseInOut,
    completion: () => { Console.WriteLine ("transition complete"); });
```

`UIView.Transition` 또한를 사용 하며 한 `duration` 애니메이션 실행 시간을 제어 하는 매개 변수 및 [ `options` ](https://developer.xamarin.com/api/type/UIKit.UIViewAnimationOptions/) 감속/가속 함수를 사용 하 여 애니메이션 등을 지정할 합니다. 또한 애니메이션 완료 될 때 호출 되는 완료 처리기를 지정할 수 있습니다.

경우 표시 아래 스크린샷에서 애니메이션된 전환 이미지 뷰 `TransitionFlipFromTop` 사용 됩니다.

 ![](core-animation-images/07-animated-transition.png "이 스크린샷은 애니메이션된 전환 TransitionFlipFromTop 사용 되는 경우 이미지 뷰를 보여 줍니다.")

### <a name="view-property-animations"></a>애니메이션 속성 보기

다양 한 속성에 애니메이션 적용 UIKit 지원는 `UIView` 포함 하 여 무료로 클래스:

-  프레임
-  범위
-  가운데 맞춤
-  알파
-  변형
-  색


이 애니메이션에서 속성 변경 내용을 지정 하 여 암시적으로 발생 한 `NSAction` 정적 전달 된 대리자 `UIView.Animate` 메서드. 예를 들어 다음 코드 애니메이션 효과 적용의 중심점은 `UIImageView`:

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

그 결과 이미지 애니메이션 간에 화면 맨 아래와 같이 같습니다.

 ![](core-animation-images/08-animate-center.png "이미지 애니메이션 간에 위쪽 화면에 출력으로")

과 마찬가지로 `Transition` 메서드를 `Animate` 감속/가속 함수를 함께 설정 하는 기간을 허용 합니다. 이 예제에서는 또한 사용는 `UIViewAnimationOptions.Autoreverse` 옵션을 애니메이션 효과를 줄 값 초기 책갈피로 애니메이션 합니다. 그러나 코드 설정도 `Center` 완료 처리기에서 초기 값으로 다시 합니다. 애니메이션은 시간이 지남에 따라 속성 값을 보간, 하는 동안 속성의 실제 모델 값은 항상 설정 된 마지막 값입니다. 이 예제에서는 값 위치가 오른쪽 근처의 슈퍼 뷰의 합니다. 설정 하지 않고는 `Center` 애니메이션으로 인해 완료 되는 위치는 시작 점으로 `Autoreverse` 설정 되는 이미지 넣었습니다 다시 왼쪽에서 오른쪽으로 애니메이션이 완료 된 후 아래와 같이:

 ![](core-animation-images/09-animation-complete.png "가운데 초기 지점을 설정 하지 않을 이미지가 넣었습니다 다시 왼쪽에서 오른쪽으로 애니메이션이 완료 된 후")

## <a name="using-core-animation"></a>코어 애니메이션을 사용 하 여

 `UIView` 애니메이션 기능을 많이 허용 및 간편한 구현으로 인해 가능한 경우 사용 해야 합니다. 앞서 언급 했 듯이 UIView 애니메이션 코어 애니메이션 프레임 워크를 사용 합니다. 그러나로 몇 가지를 수행할 수 없습니다 `UIView` 보기를 애니메이션을 적용할 수 있는 추가 속성에 애니메이션 적용 또는 비선형 경로 따라 보간 같은 애니메이션 합니다. 이러한 경우 세밀 하 게 제어 해야 하는 코어 애니메이션도 직접 사용할 수 있습니다.

### <a name="layers"></a>계층

애니메이션을 통해 발생 하는 핵심 애니메이션에서 작업할 때는 *레이어*, 유형을 `CALayer`합니다. 레이어가 비슷합니다 개념적으로 보기 한다는 점에서 레이어 계층 훨씬 뷰 계층 구조는 없습니다. 실제로, 레이어 사용자 상호 작용에 대 한 지원을 추가 하기 위한 보기와 보기를 백업 합니다. 모든 보기는 보기를 통해 계층에 액세스할 수 있습니다 `Layer` 속성입니다. 컨텍스트가에 사용 되는 사실, `Draw` 메서드 `UIView` 레이어에서 실제로 만들어집니다. 백업 레이어 내부적으로 `UIView` 의 뷰 자체는 어떤 호출로 설정 하는 대리자가 `Draw`합니다. 따라서를 그릴 때는 `UIView`, 해당 계층에 그릴 실제로 합니다.

암시적 또는 명시적 계층 애니메이션 될 수 있습니다. 암시적 애니메이션은 선언적 합니다. 변경할지 레이어 속성 단순히 선언 하 고 애니메이션 그대로 작동 합니다. 반면에 애니메이션 명시적 계층에 추가 되는 애니메이션 클래스를 통해 생성 됩니다. 명시적 애니메이션 애니메이션을 만드는 방법에 대 한 추가 제어를 허용 합니다. 다음 섹션에서는 자세히 암시적 및 명시적 애니메이션에 자세히 설명 합니다.

### <a name="implicit-animations"></a>암시적 애니메이션

계층의 속성에 애니메이션 효과를 주는 가지 방법은 암시적 애니메이션을 통해입니다. `UIView` 애니메이션 암시적 애니메이션 만들기 그러나도 계층에 대해 직접 암시적 애니메이션을 만들 수 있습니다.

다음 코드는 계층을 설정 하는 예를 들어 `Contents` 이미지에서 색과 테두리 두께 설정 하 고 보기의 계층의 하위 계층 레이어를 추가 하는 중:

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

계층에 대 한 암시적 애니메이션을 추가 하려면 단순히에서 속성 변경 내용을 래핑하는 `CATransaction`합니다. 이렇게 되지 않은 보기 애니메이션을 애니메이션 가능한와 같은 속성에 애니메이션을 적용 하면는 `BorderWidth` 및 `BorderColor` 아래와 같이:

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

이 코드 또한 애니메이션 효과 적용 레이어의 `Position`, 변수인 superlayer의 좌표의 왼쪽 위에서 측정 레이어의 앵커 지점의 위치입니다. 계층의 앵커 지점을 좌표계는 계층 내에서 정규화 된 지점입니다.

다음 그림은 위치 및 앵커 지점:

 ![](core-animation-images/10-postion-anchorpt.png "이 그림은 위치 및 앵커 지점을 보여 줍니다.")

예제를 실행 하는 경우는 `Position`, `BorderWidth` 및 `BorderColor` 다음 스크린샷에 표시 된 대로 애니메이션 효과 적용 합니다.

 ![](core-animation-images/11-implicit-animation.png "위치, BorderWidth 및 BorderColor이 표시 된 것 처럼 예제가 실행 될 때 애니메이션")

### <a name="explicit-animations"></a>명시적 애니메이션

암시적 애니메이션 외에도 코어 애니메이션에 다양 한 클래스에서 상속 하는 `CAAnimation` 는 데 사용 되는 계층에 명시적으로 추가 됩니다 애니메이션을 캡슐화 합니다. 애니메이션을 애니메이션의 시작 값을 수정, 애니메이션 그룹화 키프레임을 비선형 경로를 허용 하도록 지정 등을 통해 세부적으로 제어할을 수 있습니다.

다음 코드 예제를 사용 하 여 명시적 애니메이션은 `CAKeyframeAnimation` 에서 설명한 것 (암시적 애니메이션 섹션) 계층에 대 한:

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

이 코드가 변경 되는 `Position` 다음 키 프레임 애니메이션을 정의 하는 데 사용 되는 경로 만들어서 계층의 합니다. 다음에 유의 레이어의 `Position` 의 최종 값으로 설정 되는 `Position` 애니메이션에서 합니다. 이렇게 하지 않으면 레이어 갑자기 되돌아가 해당 `Position` 애니메이션 전에 애니메이션 프레젠테이션 값 및 실제 모델 값이 아니라만 변경 되므로 합니다. 레이어 모델 값은 최종 값에 애니메이션에서을 설정 하 여 애니메이션의 끝에는 그대로 유지 됩니다.

다음 스크린샷에서 지정 된 경로 통해 이미지 애니메이션 포함 된 레이어를 보여 줍니다.

 ![](core-animation-images/12-explicit-animation.png "이 스크린샷은 지정된 된 경로 통해 이미지 애니메이션 포함 된 레이어를 보여 줍니다.")
 
## <a name="summary"></a>요약

이 문서에서 보이는 것 처럼 애니메이션 기능을 통해 제공 되는 *코어 애니메이션* 프레임 워크입니다. 어떻게 UIKit에서 애니메이션을 구동 하 고 어떻게 하위 애니메이션 컨트롤에 대 한 직접 사용할 수를 보여 주는 핵심 애니메이션을 검사 했습니다.

## <a name="related-links"></a>관련 링크

- [코어 애니메이션 샘플](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [핵심 그래픽](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [그래픽 및 애니메이션 연습](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [핵심 애니메이션](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
