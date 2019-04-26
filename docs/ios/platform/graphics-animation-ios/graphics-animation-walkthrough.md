---
title: Xamarin.iOS에서 핵심 그래픽 및 애니메이션 코어를 사용 하 여
description: 이 문서에서는 단계별 핵심 그래픽 및 핵심 애니메이션을 사용 하는 응용 프로그램을 만드는 방법. 이미지 경로 따라 이동 애니메이션을 적용 하는 방법 뿐만 아니라 사용자 터치에 대 한 응답으로 화면에 그리는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 4B96D5CD-1BF5-4520-AAA6-2B857C83815C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 2690b60abe963cf7b02ca282b32091098a224ccf
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61091365"
---
# <a name="using-core-graphics-and-core-animation-in-xamarinios"></a>Xamarin.iOS에서 핵심 그래픽 및 애니메이션 코어를 사용 하 여

이 연습에 대 한 응답의 핵심 그래픽을 사용 하 여 터치식 입력 경로 그릴 하려고 합니다. 그런 다음 추가 `CALayer` 경로 애니메이션 효과 주기는 우리는 이미지가 포함 된 합니다.

다음 스크린샷은 완료 된 응용 프로그램을 보여 줍니다.

![](graphics-animation-walkthrough-images/00-final-app.png "완성된 된 응용 프로그램")

다운로드를 시작 하기 전에 합니다 *GraphicsDemo* 이 가이드와 함께 제공 되는 샘플입니다. 다운로드할 수 있습니다 [여기](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/) 내에서 볼 수 있으며 합니다 **GraphicsWalkthrough** directory 시작 이라는 프로젝트 **GraphicsDemo_starter** 를 두 번 클릭 및 엽니다는 `DemoView` 클래스입니다.

## <a name="drawing-a-path"></a>패스 그리기


1. `DemoView` 추가 `CGPath` 변수 클래스를 생성자에서 인스턴스화합니다. 또한 두 개를 선언 `CGPoint` 변수인 `initialPoint` 및 `latestPoint`, 경로를 생성 한 터치 지점에 캡처를 사용 하:
    
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

2. 다음 추가 지시문을 사용 하 여:

    ```csharp
    using CoreGraphics;
    using CoreAnimation;
    using Foundation;
    ```

3. 다음으로 재정의 `TouchesBegan` 및 `TouchesMoved,` 각각 초기 터치 포인트 및 각 후속 터치 포인트를 캡처하려면 다음 구현을 추가 합니다.

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

    `SetNeedsDisplay` 터치에 대 한 순서로 이동 될 때마다 호출 됩니다 `Draw` 다음 실행된 루프 패스에서 호출할 수 있습니다.

4. 줄에 포함 된 경로에 추가할 것을 `Draw` 메서드 및 사용법 사용 하 여 그리는 파선된 하면 빨간색 줄. [구현 `Draw` ](~/ios/platform/graphics-animation-ios/core-graphics.md) 아래 표시 된 코드를 사용 하 여:

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

이제 응용 프로그램을 실행 하는 경우 다음 스크린샷에 표시 된 것 처럼 화면에 그릴 터치 수 것:

![](graphics-animation-walkthrough-images/01-path.png "화면에서 그리기")

## <a name="animating-along-a-path"></a>경로 따라 애니메이션 적용

경로 그릴 수 있도록 하는 코드에 구현한 했으므로 그린된 패스를 따라 계층에 애니메이션을 적용 하는 코드를 추가 해 보겠습니다.

1. 먼저 추가 [ `CALayer` ](~/ios/platform/graphics-animation-ios/core-animation.md) 변수 클래스를 생성자에 만듭니다.

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

2. 다음으로, 추가 계층으로 뷰의 계층의 하위 사용자가 화면에서 해당 손가락을 뗄 때. 그런 다음 만들겠습니다 경로 사용 하 여 키 프레임 애니메이션을 애니메이션 레이어의 `Position`합니다.

    재정의 해야 하는 것이 `TouchesEnded` 다음 코드를 추가 합니다.

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

3. 이제 및 그리기를 마친 후 응용 프로그램을 실행, 이미지를 사용 하 여 계층을 추가 되 고 이동 그린된 패스를 따라:

![](graphics-animation-walkthrough-images/00-final-app.png "이미지를 사용 하 여 계층을 추가 되 고 이동 그린된 패스를 따라")

## <a name="summary"></a>요약

이 문서에서는 그래픽 및 애니메이션 개념을 함께 연결 하는 예제를 진행 하면서 했습니다. 경로 그릴 핵심 그래픽을 사용 하는 방법을 살펴보았습니다 먼저는 `UIView` 사용자 터치에 응답에서 합니다. 그런 다음 핵심 애니메이션을 사용 하 여 해당 경로 따라 이동 하는 이미지를 확인 하는 방법을 살펴보았습니다.


## <a name="related-links"></a>관련 링크

- [핵심 애니메이션](~/ios/platform/graphics-animation-ios/core-animation.md)
- [핵심 그래픽](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [코어 애니메이션 레시피](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
