---
title: iOS 7 소개
description: 이 문서에서는 iOS 뷰 컨트롤러 전환 애니메이션 UIView UIKit Dynamics 및 텍스트 키트의 향상 된 기능을 포함 하 여 7에에서 도입 된 주요 새로운 Api를 설명 합니다. 또한 일부 사용자 인터페이스 및 새로운 향상 된 멀티태스킹 기능 변경 내용을 다룹니다.
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: db2ce779962947e2121ff03280544a080e193e2e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61037420"
---
# <a name="introduction-to-ios-7"></a>iOS 7 소개

_이 문서에서는 iOS 뷰 컨트롤러 전환 애니메이션 UIView UIKit Dynamics 및 텍스트 키트의 향상 된 기능을 포함 하 여 7에에서 도입 된 주요 새로운 Api를 설명 합니다. 또한 일부 사용자 인터페이스 및 새로운 향상 된 멀티태스킹 기능 변경 내용을 다룹니다._

iOS 7는 iOS에 대 한 주요 업데이트입니다. 응용 프로그램 대신 콘텐츠 chrome에서 포커스를 배치 하는 완전히 새로운 사용자 인터페이스 디자인을 소개 합니다. IOS 7 시각적 개체가 바뀌면서를와 함께 다양 한 다양 한 상호 작용 및 환경을 만들려면 새 Api 추가 합니다. 이 문서 설문 조사 새로운 기술 ios 7에서 도입 된 및에 대 한 시작 점으로 사용 됩니다.

## <a name="uiview-animation-enhancements"></a>UIView 애니메이션 향상 된 기능

iOS 7 UIKit, Core Animation 프레임 워크를 직접 삭제 필요 했던 작업을 수행 하는 응용 프로그램의 애니메이션 지원을 보완 합니다. 예를 들어 `UIView` 스프링 애니메이션 뿐만 아니라 키 프레임 애니메이션을 수행할 수는 이전에 `CAKeyframeAnimation` 적용할는 `CALayer`합니다.

### <a name="spring-animations"></a>스프링 애니메이션

 `UIView` 이제 spring 효과 사용 하 여 속성 변경에 애니메이션을 지원합니다. 이 추가 하려면 하나를 호출 합니다 `AnimateNotify` 또는 `AnimateNotifyAsync` 메서드를 아래 설명 된 대로 spring 차단 (damping) 비율 및 초기 spring 속도 대 한 값의 전달:

-  `springWithDampingRatio` -0과 1을 더 작은 값에 대 한는 진동 길어지는 사이의 값입니다.
-  `initialSpringVelocity` -초당 총 애니메이션 거리에 대 한 백분율로 초기 spring 속도입니다.


다음 코드 이미지 보기의 중심 변경 되 면 spring 효과 생성 합니다.

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

이 spring 효과 사용 하면 아래 그림과 같이 새 가운데 위치로 애니메이션이 완료 되 면 반송에 게 표시 될 이미지 보기:

 ![](images/spring-animation.png "이 spring 효과 사용 하면 새 중앙 위치로 애니메이션이 완료 되 면 반송에 게 표시 될 이미지 보기")

### <a name="keyframe-animations"></a>키프레임 애니메이션

`UIView` 클래스에 포함 되어 이제는 `AnimateWithKeyframes` 에서 키 프레임 애니메이션을 만드는 메서드를 `UIView`입니다. 이 메서드는 다른 유사한 `UIView` 애니메이션 메서드를 제외한 추가 `NSAction` 키 프레임이 포함에 대 한 매개 변수로 전달 됩니다. 내 합니다 `NSAction`를 호출 하 여 키 프레임 추가 됩니다 `UIView.AddKeyframeWithRelativeStartTime`합니다.

예를 들어, 다음 코드 조각은 뷰 회전에 대 한 보기의 중심에도 애니메이션 효과를 주는 키 프레임 애니메이션을 만듭니다.

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

처음 두 매개 변수를는 `AddKeyframeWithRelativeStartTime` 메서드 시작 시간 및 기간 각각 지정, 전체 애니메이션 길이의 백분율로 표시 합니다. 다음 두 번째 통해 90도 회전 하 여 다음 결과 이미지 보기 애니메이션 효과 주는 새 중심에는 첫 번째 단위로 위의 예제입니다. 애니메이션 지정 하므로 `UIViewKeyframeAnimationOptions.Autoreverse` 옵션으로 역순으로 모두 키 프레임 애니메이션 합니다. 마지막으로 최종 값은 완료 처리기에서 초기 상태로 설정 됩니다.

아래 스크린샷에서 결합 된 키 프레임 애니메이션을 보여 줍니다.

 ![](images/keyframes.png "이 스크린샷은 결합 된 키 프레임 애니메이션을 보여 줍니다.")

## <a name="uikit-dynamics"></a>UIKit Dynamics

UIKit Dynamics는 UIKit의 응용 프로그램 물리학 기반 애니메이션 상호 작용을 허용 하는 Api의 새 집합입니다. UIKit Dynamics 주역은 (2d 물리학에 엔진을 캡슐화 합니다.

API를 기본적으로 선언적 방법입니다. 호출 개체를 만들어 물리학 상호 작용의 동작 방식을 선언 *동작* -중력, 충돌, 스프링 등 express 물리 개념입니다. 다른 개체를 호출 하는 동작이 있습니다를 연결한 다음을 *동적 애니메이터*, 뷰를 캡슐화 합니다. 동적 애니메이터는 선언 된 물리학 동작을 적용 하는 조치를 취합니다 *동적 항목* -구현 하는 항목 `IUIDynamicItem`, 등을 `UIView`입니다.

다양 한 기본 동작이 복잡 한 상호 작용을 포함 하 여 트리거를 사용할 수 있습니다.

-  `UIAttachmentBehavior` – 동적 두 항목이 함께 이동 되도록 연결 하거나 동적 항목 첨부 파일 지점에 연결 합니다.
-  `UICollisionBehavior` – 동적 항목을 충돌에 참여할 수 있습니다.
-  `UIDynamicItemBehavior` -일반 탄력성, 밀도 마찰 등 동적 항목에 적용 하는 속성 집합을 지정 합니다.
-  `UIGravityBehavior` -중력 gravitational 방향으로 가속화 하는 항목을 일으키는 동적 항목에 적용 합니다.
-  `UIPushBehavior` – Force 동적 항목에 적용합니다.
-  `UISnapBehavior` – Spring 효과가 적용 된 위치에 동적 항목을 수 있습니다.


많은 기본 형식에는 없지만 동작에서 물리학 기반 상호 작용 UIKit Dynamics를 사용 하 여 뷰를 추가 하기 위한 일반적인 프로세스는 일치:

1.  동적 애니메이터를 만듭니다.
1.  동작이 있습니다를 만듭니다.
1.  동적 애니메이터에 동작을 추가 합니다.


### <a name="dynamics-example"></a>Dynamics 예제

무게 및 충돌 경계를 추가 하는 예제를 살펴보겠습니다는 `UIView`합니다.

#### <a name="uigravitybehavior"></a>UIGravityBehavior

앞에서 설명한 3 단계를 따릅니다 중력 이미지 뷰를 추가 합니다.

처리 하겠습니다는 `ViewDidLoad` 메서드 예입니다. 첫째, 추가 `UIImageView` 같이 인스턴스:

```csharp
image = UIImage.FromFile ("monkeys.jpg");

imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
                    Image =  image
                }

View.AddSubview (imageView);
```

이 화면의 위쪽 가장자리에서 가운데에 이미지 뷰를 만듭니다. 무게를 사용 하 여 이미지 "년"을 하려면 인스턴스를 만들고를 `UIDynamicAnimator`:

```csharp
dynAnimator = new UIDynamicAnimator (this.View);
```

합니다 `UIDynamicAnimator` 참조의 인스턴스를 사용 하 `UIView` 또는 `UICollectionViewLayout`, 연결 된 동작이 있습니다 당 애니메이션이 적용 될 항목을 포함 하는 합니다.

그런 다음 만들기를 `UIGravityBehavior` 인스턴스. 구현 하는 하나 이상의 개체를 전달할 수 있습니다 합니다 `IUIDynamicItem`같은 `UIView`:

```csharp
var gravity = new UIGravityBehavior (dynItems);
```

동작의 배열을 전달 `IUIDynamicItem`,이 경우 포함 하는 단일 `UIImageView` 인스턴스 애니메이션 적용 된 것입니다.

마지막으로 동작을 동적 애니메이터를 추가 합니다.

```csharp
dynAnimator.AddBehavior (gravity);
```

그러면 아래 설명한 것 처럼 무게를 사용 하 여 하향 애니메이션 이미지:

![](images/gravity2.png "시작 이미지의 위치") 
![](images/gravity3.png "끝 이미지 위치")

항목이 없으므로 화면의 경계 제한, 이미지 보기는 아래쪽 단순히 대체 됩니다. 이미지 화면 가장자리와 충돌 하는 보기를 제한할 추가 수를 `UICollisionBehavior`입니다. 이 다음 섹션에서 설명 합니다.

#### <a name="uicollisionbehavior"></a>UICollisionBehavior

만들기부터 시작 합니다는 `UICollisionBehavior` 에 대해 수행한 것 처럼 동적 애니메이터가에 추가 하 고는 `UIGravityBehavior`합니다.

포함 하는 코드를 수정 합니다 `UICollisionBehavior`:

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

합니다 `UICollisionBehavior` 라는 속성이 `TranslatesReferenceBoundsIntoBoundry`합니다. 이 값을 설정 `true` 충돌 경계로 사용할 보기의 범위 참조를 사용 하면 됩니다.

이제 이미지 무게를 사용 하 여 하향 애니메이션을 하는 경우 해당 바운스 약간 화면의 아래쪽 있습니다 rest를 해결 하기 전에 합니다.

<!--, as shown below:

 ![](images/bounce.png "Now, when the image animates downward with gravity, it bounces slightly off the bottom of the screen before settling to rest there")-->

#### <a name="uidynamicitembehavior"></a>UIDynamicItemBehavior

추가 동작을 사용 하 여 떨어지는 이미지 뷰의 동작을 제어할 추가 했습니다. 예를 들어 추가 수를 `UIDynamicItemBehavior` 이미지 뷰 화면 아래쪽 충돌할 때 자세히 반송를 일으키는 탄력성을 높이기 위해.

추가 된 `UIDynamicItemBehavior` 다른 동작에서와 마찬가지로 동일한 단계를 따릅니다. 먼저 동작을 만듭니다.

```csharp
var dynBehavior = new UIDynamicItemBehavior (dynItems) {
    Elasticity = 0.7f
};
```

그런 다음 동적 애니메이터에 동작을 추가 합니다.

 `dynAnimator.AddBehavior (dynBehavior);`

이 동작을 사용 하 여 이미지 보기 바운스 자세히 경계와 충돌 하는 경우.

## <a name="general-user-interface-changes"></a>일반 사용자 인터페이스 변경

UIKit Dynamics, 컨트롤러 전환 및 위에서 설명한 향상 된 UIView 애니메이션 같은 새 UIKit Api 외에도 iOS 7 다양 한 시각적 개체가 바뀌면서 UI 및 다양 한 보기와 컨트롤에 대 한 관련된 API 변경 내용을 소개 합니다. 자세한 내용은 참조는 [iOS 7 사용자 인터페이스 개요](~/ios/platform/introduction-to-ios7/ios7-ui.md)합니다.

## <a name="text-kit"></a>텍스트 키트

텍스트 키트는 강력한 텍스트 레이아웃 및 렌더링 기능을 제공 하는 새 API. 이 하위 수준 Core 텍스트 프레임 워크를 기반으로 하지만 핵심 텍스트에 보다 사용 하기가 훨씬 쉽습니다.

자세한 내용은 참조 하십시오이 [TextKit](~/ios/platform/textkit.md)

## <a name="multitasking"></a>멀티태스킹

iOS 7 시기 및 백그라운드 작업 수행 되는 방식을 변경 합니다. 작업 완료 iOS 7에서에서 더 이상 작업이 백그라운드에서 실행 되 고 및 연속 되지 않은 방식에서으로 처리 하는 백그라운드에 대 한 응용 프로그램은 해제 하는 경우 절전 모드 해제 응용 프로그램을 유지 합니다. 또한 iOS 7 백그라운드에서 새 콘텐츠를 사용 하 여 응용 프로그램 업데이트에 대 한 세 개의 새로운 Api를 추가 합니다.

-  백그라운드 페치-정기적으로 백그라운드에서 콘텐츠를 업데이트할 수 있도록 응용 프로그램입니다.
-  원격 알림-응용 프로그램이 푸시 알림의 받을 때 콘텐츠를 업데이트할 수 있습니다. 알림을 수 자동 잠금 화면에 배너를 표시할 수 있습니다.
-  백그라운드 전송 서비스 – 업로드 및 고정된 시간 제한 없이 대용량 파일 등의 데이터를 다운로드할 수 있습니다.


새 멀티태스킹 기능에 대 한 자세한 내용은의 Xamarin iOS 섹션을 참조 하세요 [Backgrounding 가이드](~/ios/app-fundamentals/backgrounding/index.md)합니다.

## <a name="summary"></a>요약

이 문서에서는 iOS에 몇 가지 주요 새 추가 기능을 다룹니다. 첫째, 사용자 지정 전환을 보기 컨트롤러에 추가 하는 방법을 보여 줍니다. 그런 다음 전환 컬렉션 뷰 사이 대화형으로 탐색 컨트롤러 내의에서 모두 컬렉션 뷰를 사용 하는 방법을 보여 줍니다. 다음으로, 응용 프로그램 코어 애니메이션에 대해 직접 프로그래밍 필요 했던 항목에 대 한 UIKit를 사용 하는 방법을 보여 주는 UIView 애니메이션에 대 한 여러 개선 된 기능을 소개 합니다. 마지막으로, 물리 엔진 UIKit 적용할 경우에 새 UIKit Dynamics API는 이제 텍스트 키트 프레임 워크에서 사용할 수 있는 서식 있는 텍스트 지원와 함께 도입 되었습니다.

## <a name="related-links"></a>관련 링크

- [IOS 7 (샘플) 소개](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [iOS 7 사용자 인터페이스 개요](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)
