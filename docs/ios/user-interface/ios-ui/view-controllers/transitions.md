---
title: Xamarin.iOS 컨트롤러 전환 보기
description: 이 문서에는 애니메이션된 전환을 Xamarin.iOS 응용 프로그램의 컨트롤러 보기를 사용자 지정 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: CB3AC8E2-8A47-4839-AFA5-AE33047BB26C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 35795002310cd79a1897061fe6e3e41b48b45b4d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790450"
---
# <a name="view-controller-transitions-in-xamarinios"></a>Xamarin.iOS 컨트롤러 전환 보기

UIKit은 컨트롤러 보기를 표시할 때 발생 하는 애니메이션된 변환 사용자 지정에 대 한 지원을 추가 합니다. 이 지원은으로 기본 제공 컨트롤러에서 직접 상속 하는 모든 사용자 지정 컨트롤러에 포함 된 `UIViewController`합니다. 또한 `UICollectionViewController` 컨트롤러 전환 애니메이션된 전환을 컬렉션 보기 레이아웃에서을 활용 하는 사용자 지정 기능을 활용 합니다.

## <a name="custom-transitions"></a>사용자 지정 전환

애니메이션 효과 준 간의 전환은 iOS 7에서에서 컨트롤러 보기가 완전히 사용자 지정할 수 있습니다. `UIViewController` 이제는 `TransitioningDelegate` 전환이 발생할 때 시스템에 대 한 사용자 지정 애니메이터 클래스를 제공 하는 속성입니다.

사용자 지정 전환을 사용 하 여 `PresentViewController`:

1.  설정의 `ModalPresentationStyle` 를 `UIModalPresentationStyle.Custom` 컨트롤러를 표시 해야 합니다.
2.  구현 `UIViewControllerTransitioningDelegate` 애니메이터 클래스를 만들려면의 인스턴스인 `UIViewControllerAnimatedTransitioning` 합니다.
3.  설정의 `TransitioningDelegate` 의 인스턴스에 대 한 속성 `UIViewControllerTransitioningDelegate` 를 제공할 때는 컨트롤러에도 합니다.
4.  뷰 컨트롤러를 제공 합니다.


다음 코드에서는 형식의 보기 컨트롤러를 제공 하는 예를 들어 `ControllerTwo` - `UIViewController` 서브 클래스:

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo ();

    this.PresentViewController (controllerTwo, true, null);
};
```

응용 프로그램을 실행 하 고 단추를 누르면 두 번째 컨트롤러의 보기에는 아래에서 애니메이션 효과를 줄의 기본 애니메이션 다음과 같이 발생 합니다.

 ![](transitions-images/no-custom-transition.png "응용 프로그램을 실행 하 고 단추를 누르면 애니메이션이 기본 두 번째 컨트롤러 보기 맨 아래에서에 애니메이션 효과를 줄의")

그러나 설정 된 `ModalPresentationStyle` 및 `TransitioningDelegate` 전환에 대 한 사용자 지정 애니메이션으로 인해:

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo () {
        ModalPresentationStyle = UIModalPresentationStyle.Custom;
        };

    transitioningDelegate = new TransitioningDelegate ();
    controllerTwo.TransitioningDelegate = transitioningDelegate;

    this.PresentViewController (controllerTwo, true, null);
};
```

`TransitioningDelegate` 의 인스턴스를 만드는 책임이 `UIViewControllerAnimatedTransitioning` 하위 클래스-호출 `CustomAnimator` 아래 예제에서:

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

전환을 실행 될 때 시스템의 인스턴스를 만듭니다 `IUIViewControllerContextTransitioning`, 애니메이터의 메서드에 전달 합니다. `IUIViewControllerContextTransitioning` 포함 된 `ContainerView` 전환을 시작 뷰 컨트롤러 및 전환 결과 뷰 컨트롤러 애니메이션 발생 합니다.

`UIViewControllerAnimatedTransitioning` 실제 애니메이션을 처리 하는 클래스입니다. 두 가지 방법으로 구현 되어야 합니다.

1.  `TransitionDuration` – 애니메이션의 지속 시간을 초 단위로 반환 합니다.
1.  `AnimateTransition` – 실제 애니메이션을 수행 합니다.


예를 들어 다음 클래스 구현 `UIViewControllerAnimatedTransitioning` 컨트롤러의 보기 프레임 애니메이션 효과를 줄:

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

애니메이션 구현 되 고 단추 탭이 수행 되는 경우 이제는 `UIViewControllerAnimatedTransitioning` 클래스가 사용 됩니다.

 ![](transitions-images/custom-transition.png "실제로 실행 하는 확대/축소의 예")

## <a name="collection-view-transitions"></a>컬렉션 뷰 전환

컬렉션 뷰 전환 애니메이션된 만들기에 대 한 기본 제공 지원이 있습니다.

-  **탐색 컨트롤러** – 두 전환에 애니메이션을 `UICollectionViewController` 인스턴스 수 필요에 따라 자동으로 처리 하면 한 `UINavigationController` 에서 자동으로 관리 합니다.
-  **레이아웃 전환** – 새 `UICollectionViewTransitionLayout` 클래스를 사용 하면 대화형 레이아웃 간을 전환 합니다.


### <a name="navigation-controller-transitions"></a>탐색 컨트롤러 전환

탐색 컨트롤러 내에서 사용 하는 경우는 `UICollectionViewController` 컨트롤러 간의 애니메이션된 전환에 대 한 지원이 포함 됩니다. 이러한 지원이 기본 제공 이며 구현 하려면 몇 가지 간단한 단계만 필요 합니다.

1.  설정 `UseLayoutToLayoutNavigationTransitions` 를 `false` 에 `UICollectionViewController` 합니다.
1.  인스턴스를 추가 `UICollectionViewController` 탐색 컨트롤러의 스택의 루트에 있습니다.
1.  두 번째 만들기 `UICollectionViewController` 설정 하 고 해당 `UseLayoutToLayoutNavigtionTransitions` 속성을 `true` 합니다.
1.  두 번째 푸시 `UICollectionViewController` 탐색 컨트롤러의 스택으로 합니다.


다음 코드를 추가 `UICollectionViewController` 명명 된 하위 클래스 `ImagesCollectionViewController` 탐색 컨트롤러의 스택의 루트에 있는 `UseLayoutToLayoutNavigationTransitions` 속성이로 설정 `false`:

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
            UseLayoutToLayoutNavigationTransitions = false;
        }

    navController = new UINavigationController (viewController);

    window.RootViewController = navController;
    window.MakeKeyAndVisible ();
    
    return true;
}
```

항목을 선택 하는 경우, 두 번째 인스턴스는 `ImagesController` 만들어지면 다른 레이아웃 클래스를 사용 하 여이 경우에만 합니다. 이 컨트롤러에 대 한 `UseLayoutToLayoutNavigtionTransitions` 로 설정 된 `true`다음과 같이 합니다.

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
        UseLayoutToLayoutNavigationTransitions = true;
        }

    NavigationController.PushViewController (controller2, true);
}
```

`UseLayoutToLayoutNavigationTransitions` 탐색 스택의에 컨트롤러를 추가 하기 전에 속성을 설정 해야 합니다. 이 속성이 설정 된 일반 가로 슬라이딩 전환 아래 템플릿으로 바뀝니다 두 컨트롤러 중 레이아웃 간에 애니메이션된 전환 아래 그림과 같이:

![](transitions-images/nav2.png "두 컨트롤러 중 레이아웃 간에 애니메이션된 전환")

### <a name="transition-layout"></a>레이아웃 전환

새 레이아웃 탐색 컨트롤러 내에서 레이아웃 전환 지원 외에도 호출 `UICollectionViewTransitionLayout` ´ ù. 이 레이아웃 클래스를 허용 하 여 대화형 컨트롤 레이아웃 전환 과정에서 사용 하면는 `TransitionProgress` 를 코드에서 설정할 수 있습니다. `UICollectionViewTransitionLayout` 와 다른-및에 대 한 대체 되지-는 `SetCollectionViewLayout` 메서드에서 발생 하는 애니메이션된 레이아웃 전환을 일으킨 iOS 6입니다. 해당 메서드에 애니메이션된 전환의 진행 상태를 제어 하기 위한 기본 제공 지원을 제공 하지 않았습니다.

 `UICollectionViewTransitionLayout` 예를 들어의 전환에 대 한 응답으로 원래 레이아웃으로 전환 의도 한 레이아웃을 관리 하 여 사용자 상호 작용에 대 한 레이아웃을 제어 하도록 구성 되어야 제스처 인식기를 허용 합니다.

사용 하 여 제스처 인식기 내에서 대화형 전환을 구현 하는 단계 `UICollectionViewTransitionLayout` 은 다음과 같습니다.

1.  제스처 인식기를 만듭니다.
1.  호출 된 `StartInteractiveTransition` 의 메서드는 `UICollectionView` , 대상 레이아웃과 완료 처리기에 전달 합니다.
1.  설정의 `TransitionProgress` 의 속성에서 `UICollectionViewTransitionLayout` 에서 반환 된 인스턴스는 `StartInteractiveTransition` 메서드.
1.  레이아웃을 무효화 합니다.
1.  호출 된 `FinishInteractiveTransition` 의 메서드는 `UICollectionView` 의 전환을 완료 하려면 또는 `CancelInteractiveTransition` 취소 하는 메서드.  `FinishInteractiveTransition` 반면 대상 레이아웃으로의 전환을 완료 하려면 애니메이션이 `CancelInteractiveTransition` 를 원래 레이아웃으로 반환 하는 애니메이션으로 인해 합니다.
1.  완료 처리기에서 전환을 완료를 처리는 `StartInteractiveTransition` 메서드.
1.  제스처 인식기 컬렉션 뷰에 추가 합니다.


다음 코드는 대화형 레이아웃 전환 내 축소 제스처 인식기를 구현합니다.

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

사용자 컬렉션 뷰를 pinches 대로 `TransitionProgress` 는 약간의 눈금에 상대적으로 설정 됩니다. 이 구현에서 사용자 전환 50% 완료 되기 전에 축소 종료 되 면 전환이 취소 됩니다. 그렇지 않은 경우의 전환을 완료 됩니다.




## <a name="related-links"></a>관련 링크

- [IOS 7 (샘플) 소개](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [iOS 7 사용자 인터페이스 개요](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)
