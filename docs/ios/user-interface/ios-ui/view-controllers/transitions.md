---
title: Xamarin.ios에서 컨트롤러 전환 보기
description: 이 문서에서는 Xamarin.ios 응용 프로그램의 뷰 컨트롤러 간에 애니메이션 전환을 사용자 지정 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: CB3AC8E2-8A47-4839-AFA5-AE33047BB26C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: f0886ac9d47e7ab08a6f74365bcc25163b803e11
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73002402"
---
# <a name="view-controller-transitions-in-xamarinios"></a>Xamarin.ios에서 컨트롤러 전환 보기

UIKit에는 뷰 컨트롤러를 발표할 때 발생 하는 애니메이션 전환을 사용자 지정 하는 기능이 추가 되었습니다. 이 지원은 기본 제공 컨트롤러 뿐만 아니라 `UIViewController`에서 직접 상속 하는 사용자 지정 컨트롤러에도 포함 되어 있습니다. 또한 `UICollectionViewController`는 컨트롤러 전환 사용자 지정을 활용 하 여 컬렉션 뷰 레이아웃에서 애니메이션 전환을 활용할 수 있습니다.

## <a name="custom-transitions"></a>사용자 지정 전환

IOS 7에서 보기 컨트롤러 간의 애니메이션 전환은 완전히 사용자 지정할 수 있습니다. 이제 `UIViewController`는 전환이 발생할 때 시스템에 사용자 지정 애니메이터 클래스를 제공 하는 `TransitioningDelegate` 속성을 포함 합니다.

`PresentViewController`에서 사용자 지정 전환을 사용 하려면 다음을 수행 합니다.

1. 표시 될 컨트롤러에서 `ModalPresentationStyle` `UIModalPresentationStyle.Custom`로 설정 합니다.
2. `UIViewControllerAnimatedTransitioning` 인스턴스인 애니메이터 클래스를 만들려면 `UIViewControllerTransitioningDelegate` 구현 합니다.
3. `TransitioningDelegate` 속성을 제공할 컨트롤러에도 있는 `UIViewControllerTransitioningDelegate` 인스턴스로 설정 합니다.
4. 뷰 컨트롤러를 표시 합니다.

예를 들어 다음 코드는 `ControllerTwo` `UIViewController` 하위 클래스 형식의 뷰 컨트롤러를 제공 합니다.

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo ();

    this.PresentViewController (controllerTwo, true, null);
};
```

앱을 실행 하 고 단추를 누르면 두 번째 컨트롤러 뷰의 기본 애니메이션이 아래와 같이 아래쪽에서에 애니메이션 효과를 줍니다.

 ![](transitions-images/no-custom-transition.png "Running the app and tapping the button causes the default animation of the second controllers view to animate in from the bottom")

그러나 `ModalPresentationStyle`를 설정 하 고 `TransitioningDelegate` 하면 전환에 대 한 사용자 지정 애니메이션이 생성 됩니다.

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo () {
        ModalPresentationStyle = UIModalPresentationStyle.Custom
        };

    transitioningDelegate = new TransitioningDelegate ();
    controllerTwo.TransitioningDelegate = transitioningDelegate;

    this.PresentViewController (controllerTwo, true, null);
};
```

`TransitioningDelegate`은 아래 예제에서 `UIViewControllerAnimatedTransitioning` 하위 클래스 호출 `CustomAnimator`의 인스턴스를 만듭니다.

```csharp
public class TransitioningDelegate : UIViewControllerTransitioningDelegate
{
    CustomTransitionAnimator animator;

    public override IUIViewControllerAnimatedTransitioning GetAnimationControllerForPresentedController (UIViewController presented, UIViewController presenting, UIViewController source)
    {
        animator = new CustomTransitionAnimator ();
        return animator;
    }
}
```

전환이 수행 되 면 시스템은 애니메이터의 메서드에 전달 된 `IUIViewControllerContextTransitioning`의 인스턴스를 만듭니다. `IUIViewControllerContextTransitioning`에는 애니메이션이 발생 하는 `ContainerView` 뿐만 아니라 전환을 시작 하는 뷰 컨트롤러와 전환 되는 뷰 컨트롤러를 포함 합니다.

`UIViewControllerAnimatedTransitioning` 클래스는 실제 애니메이션을 처리 합니다. 두 가지 메서드를 구현 해야 합니다.

1. `TransitionDuration` – 애니메이션의 기간 (초)을 반환 합니다.
1. `AnimateTransition` – 실제 애니메이션을 수행 합니다.

예를 들어 다음 클래스는 컨트롤러 뷰의 프레임에 애니메이션 효과를 주기 위해 `UIViewControllerAnimatedTransitioning`를 구현 합니다.

```csharp
public class CustomTransitionAnimator : UIViewControllerAnimatedTransitioning
{
    public CustomTransitionAnimator ()
    {
    }

    public override double TransitionDuration (IUIViewControllerContextTransitioning transitionContext)
    {
        return 1.0;
    }

    public override void AnimateTransition (IUIViewControllerContextTransitioning transitionContext)
    {
        var inView = transitionContext.ContainerView;
        var toVC = transitionContext.GetViewControllerForKey (UITransitionContext.ToViewControllerKey);
        var toView = toVC.View;

        inView.AddSubview (toView);

        var frame = toView.Frame;
        toView.Frame = CGRect.Empty;

        UIView.Animate (TransitionDuration (transitionContext), () => {
            toView.Frame = new CGRect (20, 20, frame.Width - 40, frame.Height - 40);
        }, () => {
            transitionContext.CompleteTransition (true);
        });
    }
}
```

이제 단추를 탭 하면 `UIViewControllerAnimatedTransitioning` 클래스에서 구현 된 애니메이션이 사용 됩니다.

 ![](transitions-images/custom-transition.png "An example of the zoom in effect running")

## <a name="collection-view-transitions"></a>컬렉션 뷰 전환

컬렉션 뷰는 애니메이션 전환을 만들 수 있도록 기본적으로 지원 됩니다.

- **탐색 컨트롤러** – 두 `UICollectionViewController` 인스턴스 간의 애니메이션이 적용 되는 전환은 `UINavigationController` 관리 하는 경우 선택적으로 자동으로 처리할 수 있습니다.
- **전환 레이아웃** – 새 `UICollectionViewTransitionLayout` 클래스를 사용 하면 레이아웃 간의 대화형 전환을 수행할 수 있습니다.

### <a name="navigation-controller-transitions"></a>탐색 컨트롤러 전환

탐색 컨트롤러 내에서 사용 되는 `UICollectionViewController`에는 컨트롤러 간의 애니메이션 전환을 위한 지원이 포함 됩니다. 이 지원은 기본적으로 제공 되며 다음과 같은 몇 가지 간단한 단계를 구현 해야 합니다.

1. `UICollectionViewController`에서 `UseLayoutToLayoutNavigationTransitions`를 `false`로 설정 합니다.
1. `UICollectionViewController` 인스턴스를 탐색 컨트롤러 스택의 루트에 추가 합니다.
1. 두 번째 `UICollectionViewController`를 만들고 `UseLayoutToLayoutNavigtionTransitions` 속성을 `true`로 설정 합니다.
1. 두 번째 `UICollectionViewController`를 탐색 컨트롤러의 스택에 푸시합니다.

다음 코드는 `UseLayoutToLayoutNavigationTransitions` 속성이 `false`로 설정 된 `ImagesCollectionViewController` 이라는 `UICollectionViewController` 하위 클래스를 탐색 컨트롤러 스택의 루트에 추가 합니다.

```csharp
UIWindow window;
ImagesCollectionViewController viewController;
UICollectionViewFlowLayout layout;
UINavigationController navController;

public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
    window = new UIWindow (UIScreen.MainScreen.Bounds);

    // create and initialize a UICollectionViewFlowLayout
    layout = new UICollectionViewFlowLayout (){
        SectionInset = new UIEdgeInsets (10,5,10,5),
        MinimumInteritemSpacing = 5,
        MinimumLineSpacing = 5,
        ItemSize = new CGSize (100, 100)
    };

    viewController = new ImagesCollectionViewController (layout) {
            UseLayoutToLayoutNavigationTransitions = false
        };

    navController = new UINavigationController (viewController);

    window.RootViewController = navController;
    window.MakeKeyAndVisible ();

    return true;
}
```

항목을 선택 하면 `ImagesController`의 두 번째 인스턴스가 생성 되 고이 시간은 다른 레이아웃 클래스를 사용 하 여 생성 됩니다. 이 컨트롤러의 경우 아래와 같이 `UseLayoutToLayoutNavigtionTransitions` `true`로 설정 됩니다.

```csharp
CircleLayout circleLayout;
ImagesCollectionViewController controller2;

...

public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    // UseLayoutToLayoutNavigationTransitions when item is selected
    circleLayout = new CircleLayout (Monkeys.Instance.Count){
        ItemSize = new CGSize (100, 100)
    };

    controller2 = new ImagesCollectionViewController (circleLayout) {
        UseLayoutToLayoutNavigationTransitions = true
        };

    NavigationController.PushViewController (controller2, true);
}
```

컨트롤러를 탐색 스택에 추가 하기 전에 `UseLayoutToLayoutNavigationTransitions` 속성을 설정 해야 합니다. 이 속성을 설정 하면 일반 가로 슬라이딩 전환이 아래 그림과 같이 두 컨트롤러의 레이아웃 사이에서 애니메이션 전환으로 바뀝니다.

![](transitions-images/nav2.png "An animated transition between the layouts of the two controllers")

### <a name="transition-layout"></a>전환 레이아웃

탐색 컨트롤러 내의 레이아웃 전환 지원 외에도 `UICollectionViewTransitionLayout` 라는 새로운 레이아웃을 사용할 수 있습니다. 이 레이아웃 클래스는 코드에서 `TransitionProgress`를 설정할 수 있도록 하 여 레이아웃 전환 프로세스 중에 대화형 제어를 허용 합니다. `UICollectionViewTransitionLayout`은 (는)와 다르며,이는 애니메이션 레이아웃 전환을 발생 시킨 iOS 6의 `SetCollectionViewLayout` 메서드를 대체 하는 것이 아닙니다. 이 메서드는 애니메이션 전환의 진행률을 제어 하기 위한 기본 제공 지원을 제공 하지 않았습니다.

 예를 들어, `UICollectionViewTransitionLayout`를 사용 하 여 사용자 상호 작용에 대 한 응답으로 레이아웃 간의 전환을 제어 하도록 제스처 인식기를 구성 하 고, 원래 레이아웃과로 전환할 의도 한 레이아웃을 관리할 수 있습니다.

`UICollectionViewTransitionLayout`를 사용 하 여 제스처 인식기 내에서 대화형 전환을 구현 하는 단계는 다음과 같습니다.

1. 제스처 인식기를 만듭니다.
1. 대상 레이아웃 및 완료 처리기를 전달 하 여 `UICollectionView`의 `StartInteractiveTransition` 메서드를 호출 합니다.
1. `StartInteractiveTransition` 메서드에서 반환 된 `UICollectionViewTransitionLayout` 인스턴스의 `TransitionProgress` 속성을 설정 합니다.
1. 레이아웃을 무효화 합니다.
1. `UICollectionView`의 `FinishInteractiveTransition` 메서드를 호출 하 여 전환을 완료 하거나 `CancelInteractiveTransition` 메서드를 호출 하 여 취소 합니다.  `FinishInteractiveTransition`는 애니메이션이 대상 레이아웃으로의 전환을 완료 하는 반면 `CancelInteractiveTransition`는 애니메이션이 원래 레이아웃으로 반환 되도록 합니다.
1. `StartInteractiveTransition` 메서드의 완료 처리기에서 전환 완료를 처리 합니다.
1. 컬렉션 뷰에 제스처 인식기를 추가 합니다.

다음 코드는 손가락을 발생 하는 제스처 인식기 내에서 대화형 레이아웃 전환을 구현 합니다.

```csharp
imagesController = new ImagesCollectionViewController (flowLayout);

nfloat sf = 0.4f;
UICollectionViewTransitionLayout trLayout = null;
UICollectionViewLayout nextLayout;

pinch = new UIPinchGestureRecognizer (g => {

    var progress = Math.Abs(1.0f -  g.Scale)/sf;

    if(trLayout == null){
        if(imagesController.CollectionView.CollectionViewLayout is CircleLayout)
            nextLayout = flowLayout;
        else
            nextLayout = circleLayout;

        trLayout = imagesController.CollectionView.StartInteractiveTransition (nextLayout, (completed, finished) => {
            Console.WriteLine ("transition completed");
            trLayout = null;
        });
    }

    trLayout.TransitionProgress = (nfloat)progress;

    imagesController.CollectionView.CollectionViewLayout.InvalidateLayout ();

    if(g.State == UIGestureRecognizerState.Ended){
        if (trLayout.TransitionProgress > 0.5f)
            imagesController.CollectionView.FinishInteractiveTransition ();
        else
            imagesController.CollectionView.CancelInteractiveTransition ();
    }

});

imagesController.CollectionView.AddGestureRecognizer (pinch);

```

컬렉션 뷰의 사용자 인 경우에는 `TransitionProgress`의 눈금을 기준으로 설정 됩니다. 이 구현에서 전환이 50% 완료 되기 전에 사용자가를 종료 하면 전환이 취소 됩니다. 그렇지 않으면 전환이 완료 된 것입니다.

## <a name="related-links"></a>관련 링크

- [IOS 7 소개 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/introtoios7)
- [iOS 7 사용자 인터페이스 개요](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)
