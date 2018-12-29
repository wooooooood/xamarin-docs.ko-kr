---
title: Xamarin.iOS에서 뷰 컨트롤러 전환
description: 이 문서에서는 Xamarin.iOS 응용 프로그램에서 뷰 컨트롤러 간에 애니메이션 된 전환이 사용자 지정 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: CB3AC8E2-8A47-4839-AFA5-AE33047BB26C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: 6711d3af06619aa54a2d735cb83862ed64abacec
ms.sourcegitcommit: 06f88979db160fb8dd1c9ee0d5000d8749107489
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/28/2018
ms.locfileid: "53806901"
---
# <a name="view-controller-transitions-in-xamarinios"></a>Xamarin.iOS에서 뷰 컨트롤러 전환

UIKit 뷰 컨트롤러를 표시할 때 나타나는 애니메이션된 전환을 사용자 지정에 대 한 지원을 추가 합니다. 이 지원은에서 직접 상속 하는 모든 사용자 지정 컨트롤러 뿐만 아니라 기본 제공 컨트롤러를 사용 하 여 포함 `UIViewController`합니다. 또한 `UICollectionViewController` 컬렉션 보기 레이아웃의 애니메이션된 전환을 활용 하 여 컨트롤러 전환 사용자 지정 기능을 활용 합니다.

## <a name="custom-transitions"></a>사용자 지정 전환

IOS 7에서에서 뷰 컨트롤러 전환 애니메이션된은 완전히 사용자 지정할 수 있습니다. `UIViewController` 이제는 `TransitioningDelegate` 전환이 발생 하는 경우 시스템에 사용자 지정 애니메이터 클래스를 제공 하는 속성입니다.

사용 하 여 사용자 지정 전환을 사용 하 여 `PresentViewController`:

1.  설정 된 `ModalPresentationStyle` 에 `UIModalPresentationStyle.Custom` 표시할 컨트롤러에서.
2.  구현 `UIViewControllerTransitioningDelegate` 애니메이터 클래스를 만들려면의 인스턴스인 `UIViewControllerAnimatedTransitioning` 합니다.
3.  설정 된 `TransitioningDelegate` 의 인스턴스에 대 한 속성 `UIViewControllerTransitioningDelegate` , 표시 되는 컨트롤러에도 합니다.
4.  뷰 컨트롤러를 제공 합니다.


다음 코드 형식 뷰 컨트롤러를 제공 하는 예를 들어 `ControllerTwo` - `UIViewController` 하위 클래스입니다.

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo ();

    this.PresentViewController (controllerTwo, true, null);
};
```

앱을 실행 하 고 단추를 누르면 아래 표시 된 대로 애니메이션 효과 줄 맨 아래에서 두 번째 컨트롤러의 뷰의 기본 애니메이션을 발생 시킵니다.

 ![](transitions-images/no-custom-transition.png "앱을 실행 하 고 단추를 탭 하면 애니메이션 효과 줄 맨 아래에서 두 번째 컨트롤러 뷰의 기본 애니메이션")

그러나 설정 된 `ModalPresentationStyle` 및 `TransitioningDelegate` 전환에 대 한 사용자 지정 애니메이션에서 결과:

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

`TransitioningDelegate` 의 인스턴스를 만들기 위한 일을 담당 합니다 `UIViewControllerAnimatedTransitioning` 서브 클래스-호출 `CustomAnimator` 아래 예제에서:

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

전환이 완료 되 면 시스템의 인스턴스를 만듭니다 `IUIViewControllerContextTransitioning`, 애니메이터의 메서드에 전달 하는 것입니다. `IUIViewControllerContextTransitioning` 포함 된 `ContainerView` 전환을 시작 하 여 뷰 컨트롤러 및 뷰 컨트롤러 전환 되는 애니메이션 발생 합니다.

`UIViewControllerAnimatedTransitioning` 클래스는 실제 애니메이션을 처리 합니다. 두 메서드를 구현 해야 합니다.

1.  `TransitionDuration` – 애니메이션의 지속 시간을 초 단위로 반환 합니다.
1.  `AnimateTransition` – 실제 애니메이션을 수행 합니다.


예를 들어, 다음 클래스 구현 `UIViewControllerAnimatedTransitioning` 컨트롤러의 보기의 프레임 애니메이션 효과를 주는:

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

이제 단추는 탭 할 때 애니메이션에서 구현 된 `UIViewControllerAnimatedTransitioning` 클래스는:

 ![](transitions-images/custom-transition.png "확대/축소를 실제로 실행의 예")

## <a name="collection-view-transitions"></a>컬렉션 뷰 전환

컬렉션 뷰는 애니메이션 된 전환이 만들기 위한 기본 제공 지원 합니다.

-  **탐색 컨트롤러** – 두 전환 애니메이션 `UICollectionViewController` 인스턴스 필요에 따라 처리할 수 자동으로 경우는 `UINavigationController` 관리 합니다.
-  **레이아웃 전환** – 새 `UICollectionViewTransitionLayout` 클래스를 사용 하면 대화형 레이아웃 간을 전환 합니다.


### <a name="navigation-controller-transitions"></a>탐색 컨트롤러 전환

탐색 컨트롤러 내에서 사용 하는 경우는 `UICollectionViewController` 애니메이션된 전환 컨트롤러에 대 한 지원이 포함 되어 있습니다. 이 지원은 기본 제공 되며, 구현 하는 몇 가지 간단한 단계만 필요:

1.  설정 `UseLayoutToLayoutNavigationTransitions` 하 `false` 에 `UICollectionViewController` 합니다.
1.  인스턴스를 추가 합니다 `UICollectionViewController` 스택의 탐색 컨트롤러의 루트에 있습니다.
1.  하나 더 만듭니다 `UICollectionViewController` 설정 및 해당 `UseLayoutToLayoutNavigtionTransitions` 속성을 `true` 입니다.
1.  두 번째 푸시 `UICollectionViewController` 탐색 컨트롤러의 스택으로 합니다.


다음 코드를 추가 `UICollectionViewController` 라는 하위 클래스입니다. `ImagesCollectionViewController` 스택의 탐색 컨트롤러의 루트에 사용 하 여 합니다 `UseLayoutToLayoutNavigationTransitions` 속성이 설정 `false`:

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

항목을 선택 하는 경우, 두 번째 인스턴스는 `ImagesController` 만들어지면 다른 레이아웃 클래스를 사용 하 여이 시간에만 합니다. 이 컨트롤러에 대 한 `UseLayoutToLayoutNavigtionTransitions` 로 설정 된 `true`다음과 같이 합니다.

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

`UseLayoutToLayoutNavigationTransitions` 탐색 스택에 컨트롤러를 추가 하기 전에 속성을 설정 해야 합니다. 이 속성을 설정 아래 그림과 같이 일반 가로 슬라이딩 전환 애니메이션된을 전환 두 컨트롤러의 레이아웃을 사용 하 여 대체 됩니다.

![](transitions-images/nav2.png "두 컨트롤러의 레이아웃을 애니메이션된 전환")

### <a name="transition-layout"></a>레이아웃 전환

탐색 컨트롤러 내에서 레이아웃 전환 지원 외에도 새 레이아웃 호출 `UICollectionViewTransitionLayout` 출시 되었습니다. 이 레이아웃 클래스 레이아웃 전환 과정에서 허용 하 여 대화형 제어를 허용 합니다 `TransitionProgress` 코드에서 설정 해야 합니다. `UICollectionViewTransitionLayout` 다른-및 대신 하지-는 `SetCollectionViewLayout` 메서드에서 발생 하는 애니메이션된 레이아웃 전환 시킨 iOS 6입니다. 메서드는 애니메이션된 전환의 진행 상태를 제어 하기 위한 기본 제공 지원을 제공 하지 않았습니다.

 `UICollectionViewTransitionLayout` 예를 들어, 원하는 레이아웃으로 전환 뿐 아니라 원래 레이아웃을 관리 하 여 사용자 상호 작용에 대 한 응답으로 레이아웃 간을 전환 제어 하도록 제스처 인식기를 허용 합니다.

사용 하 여 제스처 인식기를 내는 대화형 전환을 구현 하는 단계 `UICollectionViewTransitionLayout` 는 다음과 같습니다.

1.  제스처 인식기를 만듭니다.
1.  호출을 `StartInteractiveTransition` 메서드는 `UICollectionView` , 대상 레이아웃과 완료 처리기에 전달 합니다.
1.  설정 합니다 `TransitionProgress` 의 속성을 `UICollectionViewTransitionLayout` 인스턴스에서 반환 된를 `StartInteractiveTransition` 메서드.
1.  레이아웃을 무효화 합니다.
1.  호출을 `FinishInteractiveTransition` 메서드를 `UICollectionView` 전환을 완료 하려면 또는 `CancelInteractiveTransition` 취소 하는 방법입니다.  `FinishInteractiveTransition` 반면 대상 레이아웃으로 전환을 완료 하려면 애니메이션이 `CancelInteractiveTransition` 원래 레이아웃으로 반환 하는 애니메이션의 결과입니다.
1.  완료 처리기에 전환 완료를 처리 합니다 `StartInteractiveTransition` 메서드.
1.  제스처 인식기 컬렉션 뷰에 추가 합니다.


다음 코드는 대화형 레이아웃 전환 축소 제스처 인식기 내에서 구현합니다.

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

사용자 컬렉션 뷰의 pinches 대로 `TransitionProgress` 는 축소의 눈금에 상대적으로 설정 됩니다. 사용자 전환 50% 완료 되기 전에 축소를 종료 하는 경우에이 구현에서 전환 취소 되었습니다. 그렇지 않은 경우 전환이 완료 되었습니다.




## <a name="related-links"></a>관련 링크

- [IOS 7 (샘플) 소개](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [iOS 7 사용자 인터페이스 개요](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)
