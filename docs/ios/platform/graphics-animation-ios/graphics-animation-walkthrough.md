---
title: Xamarin.ios에서 핵심 그래픽 및 핵심 애니메이션 사용
description: 이 문서에서는 핵심 그래픽 및 핵심 애니메이션을 사용 하는 응용 프로그램을 만드는 방법을 단계별로 설명 합니다. 사용자 터치에 대 한 응답으로 화면에 그리는 방법 뿐만 아니라 경로를 따라 이동 하도록 이미지에 애니메이션 효과를 주는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 4B96D5CD-1BF5-4520-AAA6-2B857C83815C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: badb65ace8d2ab68e102c9be127abe998a602091
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70225856"
---
# <a name="using-core-graphics-and-core-animation-in-xamarinios"></a>Xamarin.ios에서 핵심 그래픽 및 핵심 애니메이션 사용

이 연습에서는 터치 입력에 대 한 응답으로 핵심 그래픽을 사용 하 여 경로를 그리기로 전환 합니다. 그런 다음 경로를 따라 애니메이션 `CALayer` 효과를 주는 이미지를 포함 하는를 추가 합니다.

다음 스크린샷은 완성 된 응용 프로그램을 보여 줍니다.

![](graphics-animation-walkthrough-images/00-final-app.png "완료 된 응용 프로그램")

이 가이드와 함께 제공 되는 *GraphicsDemo* 샘플을 다운로드 하기 시작 합니다. [여기](https://docs.microsoft.com/samples/xamarin/ios-samples/graphicsandanimation) 에서 다운로드할 수 있으며 **GraphicsWalkthrough** 디렉터리 내에 있는 **GraphicsDemo_starter** 프로젝트를 두 번 클릭 하 여 시작 하 고 `DemoView` 클래스를 엽니다.

## <a name="drawing-a-path"></a>패스 그리기


1. 에서 `DemoView` 클래스에 `CGPath` 변수를 추가 하 고 생성자에서 인스턴스화합니다. 또한 경로를 `CGPoint` 생성 하 `initialPoint` 는 `latestPoint`터치 지점을 캡처하기 위해 사용할 두 변수 및를 선언 합니다.

    ```csharp
    public class DemoView : UIView
    {
        CGPath path;
        CGPoint initialPoint;
        CGPoint latestPoint;

        public DemoView ()
        {
            BackgroundColor = UIColor.White;

            path = new CGPath ();
        }
    }
    ```

2. 다음 using 지시문을 추가 합니다.

    ```csharp
    using CoreGraphics;
    using CoreAnimation;
    using Foundation;
    ```

3. 그런 다음 및 `TouchesBegan` `TouchesMoved,` 를 재정의 하 고 다음 구현을 추가 하 여 초기 터치 지점과 각각의 후속 터치 지점을 각각 캡처합니다.

    ```csharp
    public override void TouchesBegan (NSSet touches, UIEvent evt){

        base.TouchesBegan (touches, evt);

        UITouch touch = touches.AnyObject as UITouch;

        if (touch != null) {
            initialPoint = touch.LocationInView (this);
        }
    }

    public override void TouchesMoved (NSSet touches, UIEvent evt){

        base.TouchesMoved (touches, evt);

        UITouch touch = touches.AnyObject as UITouch;

        if (touch != null) {
            latestPoint = touch.LocationInView (this);
            SetNeedsDisplay ();
        }
    }
    ```

    `SetNeedsDisplay`는 다음 실행 루프 패스에서 호출 되기 위해 매번 `Draw` 접촉 될 때마다 호출 됩니다.

4. `Draw` 메서드의 경로에 줄을 추가 하 고 빨간색 파선을 사용 하 여 그립니다. 아래에 표시 된 코드를 사용 하 여를 [구현 `Draw` ](~/ios/platform/graphics-animation-ios/core-graphics.md) 합니다.

    ```csharp
    public override void Draw (CGRect rect){

        base.Draw (rect);

        if (!initialPoint.IsEmpty) {

            //get graphics context
            using(CGContext g = UIGraphics.GetCurrentContext ()){

                //set up drawing attributes
                g.SetLineWidth (2);
                UIColor.Red.SetStroke ();

                //add lines to the touch points
                if (path.IsEmpty) {
                    path.AddLines (new CGPoint[]{initialPoint, latestPoint});
                } else {
                    path.AddLineToPoint (latestPoint);
                }

                //use a dashed line
                g.SetLineDash (0, new nfloat[] { 5, 2 * (nfloat)Math.PI });

                //add geometry to graphics context and draw it
                g.AddPath (path);
                g.DrawPath (CGPathDrawingMode.Stroke);
            }
        }
    }
    ```

이제 응용 프로그램을 실행 하는 경우 다음 스크린샷에 표시 된 것 처럼 화면에 그릴 수 있습니다.

![](graphics-animation-walkthrough-images/01-path.png "화면에 그리기")

## <a name="animating-along-a-path"></a>경로를 따라 애니메이션 적용

이제 사용자가 경로를 그릴 수 있도록 코드를 구현 했으므로 그리기 경로를 따라 레이어에 애니메이션 효과를 주는 코드를 추가 해 보겠습니다.

1. 먼저 클래스에 [`CALayer`](~/ios/platform/graphics-animation-ios/core-animation.md) 변수를 추가 하 고 생성자에서 만듭니다.

    ```csharp
    public class DemoView : UIView
        {
            …

            CALayer layer;

            public DemoView (){
                …

                //create layer
                layer = new CALayer ();
                layer.Bounds = new CGRect (0, 0, 50, 50);
                layer.Position = new CGPoint (50, 50);
                layer.Contents = UIImage.FromFile ("monkey.png").CGImage;
                layer.ContentsGravity = CALayer.GravityResizeAspect;
                layer.BorderWidth = 1.5f;
                layer.CornerRadius = 5;
                layer.BorderColor = UIColor.Blue.CGColor;
                layer.BackgroundColor = UIColor.Purple.CGColor;
            }
    ```

2. 그런 다음 사용자가 화면에서 손가락을 뗄 때 계층을 뷰의 계층에 대 한 하위 계층으로 추가 합니다. 그런 다음 경로를 사용 하 여 키 프레임 애니메이션을 만들어 계층 `Position`에 애니메이션을 적용 합니다.

    이 작업을 수행 하려면를 재정의 하 `TouchesEnded` 고 다음 코드를 추가 해야 합니다.

    ```csharp
    public override void TouchesEnded (NSSet touches, UIEvent evt)
        {
            base.TouchesEnded (touches, evt);

            //add layer with image and animate along path

            if (layer.SuperLayer == null)
                Layer.AddSublayer (layer);

            // create a keyframe animation for the position using the path
            layer.Position = latestPoint;
            CAKeyFrameAnimation animPosition = (CAKeyFrameAnimation)CAKeyFrameAnimation.FromKeyPath ("position");
            animPosition.Path = path;
            animPosition.Duration = 3;
            layer.AddAnimation (animPosition, "position");
        }
    ```

3. 이제 응용 프로그램을 실행 하 고 그리기 후 이미지가 있는 레이어가 추가 되 고 그려진 경로를 따라 이동 합니다.

![](graphics-animation-walkthrough-images/00-final-app.png "이미지가 있는 레이어가 추가 되 고 그려진 패스를 따라 이동 합니다.")

## <a name="summary"></a>요약

이 문서에서는 그래픽 및 애니메이션 개념을 함께 연결 하는 예제를 단계별로 설명 했습니다. 먼저 핵심 그래픽을 사용 하 여 사용자 터치에 대 한 응답 `UIView` 으로에서 경로를 그리는 방법을 살펴보았습니다. 그런 다음 핵심 애니메이션을 사용 하 여 해당 경로를 따라 이미지를 이동 하는 방법을 살펴보았습니다.


## <a name="related-links"></a>관련 링크

- [핵심 애니메이션](~/ios/platform/graphics-animation-ios/core-animation.md)
- [핵심 그래픽](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [핵심 애니메이션 조리법](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
