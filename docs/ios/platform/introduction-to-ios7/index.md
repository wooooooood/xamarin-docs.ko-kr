---
title: "IOS 7 소개"
description: "이 문서에서는 iOS 뷰-컨트롤러 전환, UIKit Dynamics 및 텍스트 키트 UIView 애니메이션의 향상 된 기능을 포함 하 여 7에에서 도입 된 주요 새로운 Api에 설명 합니다. 또한 변경 내용을 사용자 인터페이스 및 새로운 향상 된 멀티태스킹 기능 중 일부를 다룹니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: a7bebc2b73ecb564028a92340c726bd5c1f1c54b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-ios-7"></a>IOS 7 소개

_이 문서에서는 iOS 뷰-컨트롤러 전환, UIKit Dynamics 및 텍스트 키트 UIView 애니메이션의 향상 된 기능을 포함 하 여 7에에서 도입 된 주요 새로운 Api에 설명 합니다. 또한 변경 내용을 사용자 인터페이스 및 새로운 향상 된 멀티태스킹 기능 중 일부를 다룹니다._

iOS 7은 iOS에 대 한 주요 업데이트 합니다. 콘텐츠 보다는 응용 프로그램의 크롬에 중점을 두는 완전히 새로운 사용자 인터페이스 디자인을 소개 합니다. IOS 7 시각적 개체가 바뀌면서 함께 다양 한 보다 다양 한 상호 작용 및 환경을 만드는 새로운 Api 추가 합니다. 이 문서 설문 조사 새로운 기술 iOS 7에 도입 된 및 따르는 시작 지점으로 사용 합니다.

## <a name="uiview-animation-enhancements"></a>UIView 애니메이션 기능

iOS 7 UIKit, 핵심 애니메이션 프레임 워크에 직접 삭제 해야 했던 작업을 수행 하는 응용 프로그램의 애니메이션 지원을 강화 되었습니다. 예를 들어 `UIView` 으로 스프링 애니메이션 키 프레임 애니메이션을 수행할 수 있는 이전에 `CAKeyframeAnimation` 에 적용 한 `CALayer`합니다.

### <a name="spring-animations"></a>스프링 애니메이션

 `UIView` 이제 스프링 효과 애니메이션 적용 속성 변경 내용을 지원합니다. 이 열을 추가 하려면 하나를 호출는 `AnimateNotify` 또는 `AnimateNotifyAsync` 아래 설명 된 대로 스프링 차단 (damping) 비율과 스프링 초기 속도 대 한 값에서 전달 하는 메서드:

-  `springWithDampingRatio` -0과 1 더 작은 값에 대 한는 진동 길어지는 사이의 값입니다.
-  `initialSpringVelocity` -초당 총 애니메이션 거리의 백분율 초기 스프링 속도입니다.


다음 코드 이미지 보기의 중심 변경 될 때 스프링 효과를 생성 합니다.

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

이 스프링 효과 사용 하면 이미지 보기 아래 그림과 같이 새 가운데 위치로 해당 애니메이션을 완료 bounce에 게 표시 합니다.

 ![](images/spring-animation.png "이 스프링 효과 사용 하면 이미지 보기를 표시할 bounce 새 센터 위치에 애니메이션을 완료 하려면")

### <a name="keyframe-animations"></a>키 프레임 애니메이션

`UIView` 클래스에 포함 되어 이제는 `AnimateWithKeyframes` 에 키 프레임 애니메이션을 만드는 방법을 `UIView`합니다. 이 메서드는 다른 곳으로 `UIView` 애니메이션 메서드 점을 제외 하면 추가 `NSAction` 포함 된 키 프레임에 대 한 매개 변수로 전달 됩니다. 내에서 `NSAction`를 호출 하 여 키 프레임 추가 `UIView.AddKeyframeWithRelativeStartTime`합니다.

예를 들어 다음 코드 조각도 보기의 중심 보기 회전에 대 한 애니메이션 효과를 주는 키 프레임 애니메이션을 만듭니다.

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

처음 두 매개 변수는 `AddKeyframeWithRelativeStartTime` 메서드 시작 시간 및 기간 각각 지정, 전체 애니메이션 길이의 백분율입니다. 위의 결과에 이미지 보기 애니메이션을 적용할 새 중심을 관통 하 통해 첫 번째 두 번째 예에서는 다음 초를 통해 90도 회전 하 여 뒤에 있습니다. 애니메이션 지정 하므로 `UIViewKeyframeAnimationOptions.Autoreverse` 옵션으로 역순으로 모두 키 프레임 애니메이션 합니다. 마지막으로, 최종 값은 완료 처리기에서 초기 상태로 설정 됩니다.

아래 스크린샷과 결합 된 애니메이션을 키프레임을 통해 보여 줍니다.

 ![](images/keyframes.png "이 스크린 샷을 결합 된 애니메이션을 키프레임을 통해를 보여 줍니다.")

## <a name="uikit-dynamics"></a>UIKit Dynamics

UIKit Dynamics는 새 집합 UIKit에서 응용 프로그램 물리학 기반 애니메이션된 상호 작용을 허용 하는 Api입니다. UIKit Dynamics 가능 하도록 하는 2D 물리 엔진을 캡슐화 합니다.

API는 본질적으로 선언적 방법입니다. 라고 하는 개체-만들어 물리 상호 작용의 동작 방식을 선언 *동작* -: 중력, 충돌, springs 등과 같은 빠른 물리 개념입니다. 다음 호출 하는 다른 개체에는 behavior(s) 연결는 *동적 애니메이터*, 보기를 캡슐화 하는 합니다. 동적 애니메이터는 선언 된 물리 동작을 적용 한 관심이 있는 *동적 항목* -구현 하는 항목 `IUIDynamicItem`와 같은 `UIView`합니다.

몇 가지 서로 다른 기본 동작을 포함 하 여 복잡 한 상호 작용을 트리거하는 사용할 수 있습니다.

-  `UIAttachmentBehavior` – 함께 이동 되도록 두 동적 항목을 연결 하거나 동적 항목 첨부 파일 지점에 연결 합니다.
-  `UICollisionBehavior` – 동적 충돌에 포함할 항목 수 있습니다.
-  `UIDynamicItemBehavior` -일반 집합이 탄력성, 밀도 및 마찰 같은 동적 항목에 적용할 속성을 지정 합니다.
-  `UIGravityBehavior` -중력 gravitational 방향의 가속화 하는 항목을 동적 항목에 적용 합니다.
-  `UIPushBehavior` – Force 동적 항목에 적용합니다.
-  `UISnapBehavior` – 스프링 영향을 주지 않고 위치에 맞춰서 동적 항목 수 있습니다.


여러 기본 형식에는 없지만 동작에서 물리 기반 상호 작용 UIKit Dynamics를 사용 하 여 뷰를 추가 하기 위한 일반적인 프로세스는 일치.

1.  동적 애니메이터를 만듭니다.
1.  Behavior(s)를 만듭니다.
1.  동적 애니메이터에 동작을 추가 합니다.


### <a name="dynamics-example"></a>Dynamics 예제

중력 및 충돌 경계를 추가 하는 예제를 살펴 보겠습니다는 `UIView`합니다.

#### <a name="uigravitybehavior"></a>UIGravityBehavior

중력 보기에 추가 하는 이미지 위에 설명 된 3 단계를 따릅니다.

사용할는 `ViewDidLoad` 메서드이 예제에 대 한 합니다. 먼저 추가 하는 `UIImageView` 다음과 같이 인스턴스:

```csharp
image = UIImage.FromFile ("monkeys.jpg");

imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
                    Image =  image
                }

View.AddSubview (imageView);
```

이 화면 위쪽 가장자리에서 가운데에 이미지 뷰를 만듭니다. 중력와 이미지 "년"을 하려면의 인스턴스를 만들고는 `UIDynamicAnimator`:

```csharp
dynAnimator = new UIDynamicAnimator (this.View);
```

`UIDynamicAnimator` 대 한 참조의 인스턴스 `UIView` 또는 `UICollectionViewLayout`, 연결 된 behavior(s) 당 애니메이션이 적용 될 항목을 포함 하 합니다.

다음으로 만듭니다는 `UIGravityBehavior` 인스턴스. 구현 하는 하나 이상의 개체를 전달할 수는 `IUIDynamicItem`처럼는 `UIView`:

```csharp
var gravity = new UIGravityBehavior (dynItems);
```

동작의 배열을 전달 `IUIDynamicItem`,이 경우 단일을 포함 하 `UIImageView` म 애니메이션 효과 적용할 인스턴스.

마지막으로, 동적 애니메이터에 동작을 추가 합니다.

```csharp
dynAnimator.AddBehavior (gravity);
```

이 인해 아래 설명한 것 처럼 중력 사용한 하향 애니메이션 이미지:

![](images/gravity2.png "시작 되는 이미지 위치") 
![](images/gravity3.png "끝 이미지 위치")

화면의 경계를 제한 하는 아무 것도가 이미지 보기 아래에서 단순히 해당 합니다. 추가할 수 있는 이미지 화면의 가장자리와 충돌 하는 보기를 제한할는 `UICollisionBehavior`합니다. 이 다음 섹션에서 설명 합니다.

#### <a name="uicollisionbehavior"></a>UICollisionBehavior

먼저 만들어는 `UICollisionBehavior` 에 대해 했던 것 처럼 동적 애니메이터에 추가 하 고는 `UIGravityBehavior`합니다.

코드를 포함 하도록 수정 하는 `UICollisionBehavior`:

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

`UICollisionBehavior` 라는 속성이 `TranslatesReferenceBoundsIntoBoundry`합니다. 이 값을 설정 `true` 충돌 경계로 사용할 보기의 범위 참조가 발생 합니다.

이제 이미지 무게와 아래쪽으로 애니메이션을 적용 때 것 반송 약간의 화면 아래쪽에서 있습니다 놓여야 하 고 결정 하기 전까지 합니다.

<!--, as shown below:

 ![](images/bounce.png "Now, when the image animates downward with gravity, it bounces slightly off the bottom of the screen before settling to rest there")-->

#### <a name="uidynamicitembehavior"></a>UIDynamicItemBehavior

추가 동작을 가진 하강 이미지 보기의 동작을 제어할 추가 했습니다. 예를 들어, म 추가할 수는 `UIDynamicItemBehavior` 이미지 보기 화면 아래쪽 충돌할 때 더 bounce를 일으키는 탄력성 증가 합니다.

추가 `UIDynamicItemBehavior` 다른 동작에서와 마찬가지로 동일한 단계를 따릅니다. 첫 번째 동작을 만듭니다.

```csharp
var dynBehavior = new UIDynamicItemBehavior (dynItems) {
    Elasticity = 0.7f
};
```

동적 애니메이터에 동작을 추가 합니다.

 `dynAnimator.AddBehavior (dynBehavior);`

위치에이 동작으로 이미지 보기 반송 더 경계 충돌할 때.

## <a name="general-user-interface-changes"></a>일반 사용자 인터페이스 변경 내용

위에서 설명한 향상 된 UIView 애니메이션 UIKit Dynamics, 컨트롤러 전환 등 새로운 UIKit Api 뿐 아니라 iOS 7 다양 한 시각적 개체가 바뀌면서 UI 및 다양 한 보기 및 컨트롤에 대 한 관련된 API 변경 내용을 소개 합니다. 자세한 내용은 참조는 [iOS 7 사용자 인터페이스 개요](~/ios/platform/introduction-to-ios7/ios7-ui.md)합니다.

## <a name="text-kit"></a>텍스트 키트

텍스트 키트는 강력한 텍스트 레이아웃 및 렌더링 기능을 제공 하는 새로운 API입니다. 낮은 수준 코어 텍스트 프레임 워크 위에 빌드됩니다. 따라서 하 있지만 핵심 텍스트 보다 사용 하기가 훨씬 쉽습니다.

자세한 내용은 참조 하십시오 우리의 [TextKit](~/ios/platform/textkit.md)

## <a name="multitasking"></a>멀티태스킹

iOS 7 시기 및를 백그라운드 작업을 수행 하는 방법을 변경 합니다. 작업 완료 iOS 7에서에서 더 이상 작업이 백그라운드에서 실행 되 고 하 고 응용 프로그램은 백그라운드 연속 되지 않은 방식으로 처리에 대 한 절전 모드 해제 절전 모드 해제 응용 프로그램을 유지 합니다. iOS 7를 백그라운드에서 새 내용으로 업데이트 하는 응용 프로그램에 대 한 세 개의 새로운 Api 추가 됩니다.

-  백그라운드 가져오기를 – 정기적으로 백그라운드에서 콘텐츠를 업데이트할 수 있도록 응용 프로그램입니다.
-  원격 알림-응용 프로그램이 푸시 알림의 받을 때 콘텐츠를 업데이트할 수 있습니다. 알림을 일 수 있습니다 자동 잠금 화면에서 배너를 표시할 수 있습니다.
-  백그라운드 전송 서비스 – 업로드 및 고정된 시간 제한 없이 큰 파일을와 같은 데이터를 다운로드 하도록 허용 합니다.


새 멀티태스킹 기능에 대 한 자세한 내용은 Xamarin의 iOS 섹션을 참조 [Backgrounding 가이드](~/ios/app-fundamentals/backgrounding/index.md)합니다.

## <a name="summary"></a>요약

이 문서에서는 iOS에 몇 가지 주요 새로운 기능이 추가 설명 합니다. 첫째, 컨트롤러 보기를 사용자 지정 전환을 추가 하는 방법을 보여 줍니다. 그런 다음 컬렉션 뷰 컬렉션 뷰 사이 대화형으로 탐색 컨트롤러 내에서에서 모두에서 전환을 사용 하는 방법을 보여 줍니다. 다음으로 응용 프로그램 해야 했던 코어 애니메이션에 대해 직접 프로그래밍 하는 것의 UIKit를 사용 하는 방법을 보여 주는 UIView 애니메이션에 대 한 여러 개선 된 기능이 도입 되었습니다. 마지막으로, UIKit 물리 엔진은 경우, 하는 새 UIKit Dynamics API에는 이제 텍스트 키트 프레임 워크에서 사용할 수 있는 서식 있는 텍스트 지원와 함께 도입 되었습니다.

## <a name="related-links"></a>관련 링크

- [IOS 7 (샘플) 소개](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [iOS 7 사용자 인터페이스 개요](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)
