---
title: iOS 7 소개
description: 이 문서에서는 보기 컨트롤러 전환을 비롯 하 여 iOS 7에 도입 된 주요 새로운 Api 인 UIView 애니메이션, Uiview Dynamics 및 텍스트 키트의 향상 된 기능에 대해 설명 합니다. 또한 사용자 인터페이스에 대 한 몇 가지 변경 내용 및 새로운 향상 된 멀티태스킹 기능도 다룹니다.
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 885cf4b77d4eac0668a2e70c57187e9b23a91dd1
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69527562"
---
# <a name="introduction-to-ios-7"></a>iOS 7 소개

_이 문서에서는 보기 컨트롤러 전환을 비롯 하 여 iOS 7에 도입 된 주요 새로운 Api 인 UIView 애니메이션, Uiview Dynamics 및 텍스트 키트의 향상 된 기능에 대해 설명 합니다. 또한 사용자 인터페이스에 대 한 몇 가지 변경 내용 및 새로운 향상 된 멀티태스킹 기능도 다룹니다._

ios 7은 iOS에 대 한 주요 업데이트입니다. 응용 프로그램 chrome이 아닌 콘텐츠에 초점을 맞춘 완전히 새로운 사용자 인터페이스 디자인을 도입 합니다. 시각적 변경 내용과 함께 iOS 7은 다양 한 상호 작용 및 환경을 만들기 위해 다양 한 새 Api를 추가 합니다. 이 문서에서는 iOS 7에 도입 된 새로운 기술을 설문 조사 하 고, 추가 탐색을 위한 시작 지점으로 사용 합니다.

## <a name="uiview-animation-enhancements"></a>UIView 애니메이션 기능 향상

iOS 7은 UIKit에서 애니메이션 지원을 보강 하 여 응용 프로그램이 이전에 핵심 애니메이션 프레임 워크에 직접 삭제 해야 했던 작업을 수행할 수 있도록 합니다. 예 `UIView` 를 들어 이제는 `CALayer`스프링 애니메이션 뿐만 아니라 `CAKeyframeAnimation` 이전에에 적용 된 키 프레임 애니메이션을 수행할 수 있습니다.

### <a name="spring-animations"></a>스프링 애니메이션

 `UIView`에서는 이제 스프링 효과를 사용 하 여 속성 변경에 애니메이션 효과를 지원 합니다. 이를 추가 하려면 아래에서 설명 `AnimateNotify` 하 `AnimateNotifyAsync` 는 것 처럼 또는 메서드를 호출 하 여 스프링 댐핑 비율 및 초기 스프링 속도에 대 한 값을 전달 합니다.

- `springWithDampingRatio`– 진동 범위가가 더 작은 값에 대해 늘리는 0에서 1 사이의 값입니다.
- `initialSpringVelocity`– 초기 스프링 속도는 초당 총 애니메이션 거리의 백분율로 나타낸 것입니다.


다음 코드는 이미지 뷰의 중심이 변경 될 때 스프링 효과를 생성 합니다.

```csharp
void AnimateWithSpring ()
{ 
    float springDampingRatio = 0.25f;
    float initialSpringVelocity = 1.0f;
    
    UIView.AnimateNotify (3.0, 0.0, springDampingRatio, initialSpringVelocity, 0, () => {
    
        imageView.Center = new CGPoint (imageView.Center.X, 400);   
            
    }, null);
}
```

이 스프링 효과는 아래 그림과 같이 새 중심 위치에 대 한 애니메이션을 완료할 때 이미지 뷰가 바운스에 표시 되도록 합니다.

 ![](images/spring-animation.png "이 스프링 효과는 새 중심 위치에 대 한 애니메이션을 완료할 때 이미지 뷰가 바운스에 표시 되도록 합니다.")

### <a name="keyframe-animations"></a>키 프레임 애니메이션

이제 `UIView` 클래스는에 키 `AnimateWithKeyframes` 프레임 애니메이션을 `UIView`만드는 메서드를 포함 합니다. 이 메서드는 추가 `UIView` `NSAction` 가 키 프레임을 포함 하는 매개 변수로 전달 된다는 점을 제외 하 고 다른 애니메이션 메서드와 비슷합니다. 내에서 `UIView.AddKeyframeWithRelativeStartTime`를 호출 하 여 키 프레임을 추가 합니다. `NSAction`

예를 들어 다음 코드 조각에서는 보기의 중심에 애니메이션 효과를 주는 키 프레임 애니메이션을 만들고 뷰를 회전 합니다.

```csharp
void AnimateViewWithKeyframes ()
{
    var initialTransform = imageView.Transform;
    var initialCeneter = imageView.Center;

    // can now use keyframes directly on UIView without needing to drop directly into Core Animation

    UIView.AnimateKeyframes (2.0, 0, UIViewKeyframeAnimationOptions.Autoreverse, () => {
        UIView.AddKeyframeWithRelativeStartTime (0.0, 0.5, () => { 
            imageView.Center = new CGPoint (200, 200);
        });

        UIView.AddKeyframeWithRelativeStartTime (0.5, 0.5, () => { 
            imageView.Transform = CGAffineTransform.MakeRotation ((float)Math.PI / 2);
        });
    }, (finished) => {
        imageView.Center = initialCeneter;
        imageView.Transform = initialTransform;

        AnimateWithSpring ();
    });
}
```

`AddKeyframeWithRelativeStartTime` 메서드에 대 한 처음 두 매개 변수는 각각 전체 애니메이션 길이에 대 한 백분율로 키 프레임의 시작 시간과 기간을 지정 합니다. 위의 예제에서는 첫 번째 초 동안 이미지 뷰가 새 중심에 애니메이션 효과를 적용 한 다음, 다음 초 동안 90도를 회전 합니다. 애니메이션은 옵션으로 `UIViewKeyframeAnimationOptions.Autoreverse` 를 지정 하므로 두 키프레임이 모두 반대 방향으로도 애니메이션 효과를 적용 합니다. 마지막으로 최종 값은 완료 처리기의 초기 상태로 설정 됩니다.

아래 스크린샷은 키프레임을 통해 결합 된 애니메이션을 보여 줍니다.

 ![](images/keyframes.png "이 스크린샷은 키프레임을 통해 결합 된 애니메이션을 보여 줍니다.")

## <a name="uikit-dynamics"></a>UIKit Dynamics

UIKit Dynamics는 응용 프로그램이 물리학에 따라 애니메이션 된 상호 작용을 만들 수 있도록 하는 UIKit의 새로운 Api 집합입니다. UIKit Dynamics는 이러한 작업을 가능 하 게 하는 2D 물리학 엔진을 캡슐화 합니다.

API는 본질적으로 선언적입니다. 무게, 충돌, 스프링 등의 물리학 개념을 표현 하 는 개체를 만들어 물리 상호 작용의 동작 방식을 선언 합니다. 그런 다음 뷰를 캡슐화 하는 *동적 애니메이터*라는 다른 개체에 동작을 연결 합니다. 동적 애니메이터는 선언 된 물리 동작을 *동적 항목* 에 적용 하는 데 관심이 `IUIDynamicItem` `UIView`있습니다 .와 같이를 구현 하는 항목입니다.

다음을 포함 하 여 복잡 한 상호 작용을 트리거하는 데 사용할 수 있는 여러 가지 기본 동작이 있습니다.

- `UIAttachmentBehavior`– 함께 이동 하거나 동적 항목을 첨부 지점에 연결 하는 두 개의 동적 항목을 연결 합니다.
- `UICollisionBehavior`– 동적 항목이 충돌에 참여할 수 있도록 허용 합니다.
- `UIDynamicItemBehavior`– 탄력성, 밀도, 마찰 등의 동적 항목에 적용할 일반적인 속성 집합을 지정 합니다.
- `UIGravityBehavior`-동적 항목에 무게를 적용 하 여 gravitational 방향으로 항목을 가속 시킵니다.
- `UIPushBehavior`– Force를 동적 항목에 적용 합니다.
- `UISnapBehavior`– 스프링 효과를 사용 하 여 동적 항목을 위치에 맞출 수 있습니다.


여러 가지 기본 형식이 있지만 UIKit Dynamics를 사용 하 여 보기에 물리학 기반 상호 작용을 추가 하는 일반적인 프로세스는 동작 간에 일관 됩니다.

1. 동적 애니메이터를 만듭니다.
1. 동작을 만듭니다.
1. 동적 애니메이터에 동작을 추가 합니다.


### <a name="dynamics-example"></a>Dynamics 예제

에 중력 및 충돌 경계 `UIView`를 추가 하는 예제를 살펴보겠습니다.

#### <a name="uigravitybehavior"></a>UIGravityBehavior

이미지 뷰에 중력을 추가 하는 것은 위에 설명 된 3 단계를 따릅니다.

이 예제에서는 `ViewDidLoad` 메서드를 사용 합니다. 먼저 다음과 같이 인스턴스 `UIImageView` 를 추가 합니다.

```csharp
image = UIImage.FromFile ("monkeys.jpg");

imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
                    Image =  image
                }

View.AddSubview (imageView);
```

그러면 화면 위쪽 가장자리를 중심으로 하는 이미지 뷰가 만들어집니다. 이미지를 중력으로 "포함" 하려면의 `UIDynamicAnimator`인스턴스를 만듭니다.

```csharp
dynAnimator = new UIDynamicAnimator (this.View);
```

는 `UIDynamicAnimator` 연결 된 동작에 따라 애니메이션이 `UIView` 적용 될 `UICollectionViewLayout`항목을 포함 하는 또는 참조의 인스턴스를 사용 합니다.

그런 다음 `UIGravityBehavior` 인스턴스를 만듭니다. 와 같이를 구현 하는 `IUIDynamicItem`하나 이상의 개체를 전달할 수 있습니다. `UIView`

```csharp
var gravity = new UIGravityBehavior (dynItems);
```

동작에는의 `IUIDynamicItem`배열이 전달 됩니다 .이 경우에는 애니메이션이 적용 되는 단일 `UIImageView` 인스턴스가 포함 됩니다.

마지막으로 동적 애니메이터에 동작을 추가 합니다.

```csharp
dynAnimator.AddBehavior (gravity);
```

이로 인해 아래 그림과 같이 이미지가 중력에 애니메이션 효과를 줍니다.

![](images/gravity2.png "시작 이미지 위치") 
![](images/gravity3.png "끝 이미지 위치")

화면 경계를 제한 하는 것이 없으므로 이미지 뷰는 아래쪽을 벗어납니다. 이미지가 화면 가장자리와 충돌 하도록 보기를 제한 하려면를 `UICollisionBehavior`추가할 수 있습니다. 이에 대해서는 다음 섹션에서 설명 합니다.

#### <a name="uicollisionbehavior"></a>UICollisionBehavior

`UICollisionBehavior` 에서`UIGravityBehavior`수행한 것 처럼를 만들고 동적 애니메이터에 추가 하 여 시작 합니다.

을 포함 `UICollisionBehavior`하도록 코드를 수정 합니다.

```csharp
using (image = UIImage.FromFile ("monkeys.jpg")) {

    imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
        Image =  image
    };

    View.AddSubview (imageView);

    // 1. create the dynamic animator
    dynAnimator = new UIDynamicAnimator (this.View);

    // 2. create behavior(s)
    var gravity = new UIGravityBehavior (imageView);
    var collision = new UICollisionBehavior (imageView) {
        TranslatesReferenceBoundsIntoBoundary = true
    };

    // 3. add behaviors(s) to the dynamic animator
    dynAnimator.AddBehaviors (gravity, collision);
}
```

에 `UICollisionBehavior` 는 라는 `TranslatesReferenceBoundsIntoBoundry`속성이 있습니다. 로 `true` 설정 하면 참조 뷰의 범위가 충돌 경계로 사용 됩니다.

이제 이미지가 중력을 사용 하 여 아래쪽으로 애니메이션 효과를 정착 전에 화면 아래쪽을 약간 벗어나 바운스 합니다.

<!--, as shown below:

 ![](images/bounce.png "Now, when the image animates downward with gravity, it bounces slightly off the bottom of the screen before settling to rest there")-->

#### <a name="uidynamicitembehavior"></a>UIDynamicItemBehavior

추가 동작을 사용 하 여 대체 이미지 뷰의 동작을 추가로 제어할 수 있습니다. 예를 들어를 추가 `UIDynamicItemBehavior` 하 여 탄력성를 늘릴 수 있습니다. 그러면 이미지 뷰가 화면 아래쪽과 충돌 하는 경우 더 많이 바운스 됩니다.

을 `UIDynamicItemBehavior` 추가 하면 다른 동작과 동일한 단계를 따릅니다. 먼저 동작을 만듭니다.

```csharp
var dynBehavior = new UIDynamicItemBehavior (dynItems) {
    Elasticity = 0.7f
};
```

그런 다음 동적 애니메이터에 동작을 추가 합니다.

 `dynAnimator.AddBehavior (dynBehavior);`

이 동작을 수행 하면 이미지 뷰가 경계와 충돌 하는 경우 더 많이 바운스 됩니다.

## <a name="general-user-interface-changes"></a>일반 사용자 인터페이스 변경 내용

위에 설명 된 UIKit Dynamics, 컨트롤러 전환 및 향상 된 Uikit 애니메이션과 같은 새로운 UIKit Api 외에도 iOS 7에는 다양 한 UI 변경 내용과 다양 한 뷰 및 컨트롤에 대 한 관련 API 변경 내용이 도입 되었습니다. 자세한 내용은 [iOS 7 사용자 인터페이스 개요](~/ios/platform/introduction-to-ios7/ios7-ui.md)를 참조 하세요.

## <a name="text-kit"></a>텍스트 키트

텍스트 키트는 강력한 텍스트 레이아웃 및 렌더링 기능을 제공 하는 새로운 API입니다. 낮은 수준의 핵심 텍스트 프레임 워크를 기반으로 하지만 핵심 텍스트 보다 훨씬 더 쉽게 사용할 수 있습니다.

자세한 내용은 [Textkit](~/ios/platform/textkit.md) 를 참조 하세요.

## <a name="multitasking"></a>멀티태스킹

iOS 7은 백그라운드 작업 수행 시와 방법을 변경 합니다. 작업을 백그라운드에서 실행 하는 경우 iOS 7에서 작업을 완료 하면 더 이상 응용 프로그램을 계속 사용할 수 없으며, 응용 프로그램은 연속 하지 않는 방식으로 백그라운드 처리에 해제 됩니다. 또한 iOS 7에서는 응용 프로그램을 새 콘텐츠로 업데이트 하는 세 가지 새로운 Api를 백그라운드에서 추가 합니다.

- 백그라운드 페치 – 응용 프로그램에서 정기적인 간격으로 백그라운드에서 콘텐츠를 업데이트할 수 있습니다.
- 원격 알림-푸시 알림을 받을 때 응용 프로그램에서 콘텐츠를 업데이트할 수 있습니다. 알림은 자동 이거나 잠금 화면에 배너를 표시할 수 있습니다.
- 백그라운드 전송 서비스 – 고정 된 시간 제한이 없는 대량 파일 등의 데이터를 업로드 및 다운로드할 수 있습니다.


새 멀티태스킹 기능에 대 한 자세한 내용은 Xamarin [Backgrounding guide](~/ios/app-fundamentals/backgrounding/index.md)의 iOS 섹션을 참조 하세요.

## <a name="summary"></a>요약

이 문서에서는 iOS에 새로 추가 된 몇 가지 주요 사항을 설명 합니다. 먼저 뷰 컨트롤러에 사용자 지정 전환을 추가 하는 방법을 보여 줍니다. 그런 다음 탐색 컨트롤러 내에서 컬렉션 뷰의 전환을 사용 하는 방법과 컬렉션 뷰 사이에 대화형으로 사용 하는 방법을 보여 줍니다. 다음으로, 응용 프로그램에서 핵심 애니메이션에 대해 이전에 프로그래밍에 필요한 항목에 대해 Uiview를 사용 하는 방법을 보여 주는 UIView 애니메이션에 대 한 몇 가지 향상 된 기능이 도입 되었습니다. 마지막으로, UIKit로 물리학 엔진을 가져오는 새로운 UIKit Dynamics API는 이제 텍스트 키트 프레임 워크에서 사용할 수 있는 서식 있는 텍스트 지원과 함께 도입 되었습니다.

## <a name="related-links"></a>관련 링크

- [IOS 7 소개 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/introtoios7)
- [iOS 7 사용자 인터페이스 개요](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)
