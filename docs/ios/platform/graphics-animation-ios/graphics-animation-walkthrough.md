---
title: 연습-CoreGraphics 및 CoreAnimation 사용 하 여
description: 이 문서에서는 과정을 단계별로 코어 그래픽 및 코어 애니메이션을 사용 하는 응용 프로그램을 만드는 방법을 설명 합니다. 이미지 경로 따라 이동 하는 애니메이션 효과 적용 하는 방법 뿐만 아니라 사용자 터치에 대 한 응답의 화면에 그리는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 4B96D5CD-1BF5-4520-AAA6-2B857C83815C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f857accfcdec4cb60e781936d1d0836dbf8d6ffb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="drawing-and-animating-along-a-path"></a>그리기 및 경로 따라 애니메이션 적용

이 연습을 하겠습니다 터치식 입력에 대 한 응답으로 코어 그래픽을 사용 하 여 패스를 그립니다. 그런 다음 추가 됩니다 한 `CALayer` 경로 따라 애니메이션는 이미지가 포함 된 합니다.

다음 스크린 샷에서 완성 된 응용 프로그램을 보여 줍니다.

![](graphics-animation-walkthrough-images/00-final-app.png "완성 된 응용 프로그램")

다운로드를 시작 하기 전에 *GraphicsDemo* 이 가이드를 함께 제공 되는 샘플입니다. 다운로드할 수 있습니다 [여기](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/) 묶여 및는 **GraphicsWalkthrough** 라는 프로젝트를 시작 하는 디렉터리 **GraphicsDemo_starter** , 두 번 클릭 및 열은 `DemoView` 클래스입니다.

## <a name="drawing-a-path"></a>패스 그리기


1. `DemoView` 추가 `CGPath` 변수 클래스를 생성자에서 인스턴스화합니다. 또한 두 개를 선언 `CGPoint` 변수 `initialPoint` 및 `latestPoint`, 경로 म 생성에 있는 연결점 캡처 데 됩니다:
    
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

3. 다음으로 재정의 `TouchesBegan` 및 `TouchesMoved,` 각각 초기 터치 포인트 및 각 후속 터치 포인트를 캡처하려면 다음 구현을 추가 하 고 있습니다.

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

    `SetNeedsDisplay` 작업 순서에서 이동 될 때마다 호출 됩니다 `Draw` 다음 실행된 루프 패스에서 호출할 수 있습니다.

4. 줄에 포함 된 경로를 추가 합니다는 `Draw` 메서드와 빨간색, 파선 줄을 사용 하 여 그리기에 사용 합니다. [구현 `Draw` ](~/ios/platform/graphics-animation-ios/core-graphics.md) 아래에 표시 된 코드와 함께:

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

지금 응용 프로그램을 실행 하는 경우 다음 스크린샷에 표시 된 것 처럼는 화면에 나타내려면 인접할 수 있습니다.

![](graphics-animation-walkthrough-images/01-path.png "화면 그리기")

## <a name="animating-along-a-path"></a>경로 따라 애니메이션 적용

사용자가 경로 그리는 수 있도록 코드를 구현한 했으므로 그려지는 경로 따라 레이어 애니메이션 효과를 줄 코드를 추가 해 보겠습니다.

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

2. 다음으로, 사용자가 화면에서 손가락 자신의를 뗄 때 보기의 계층의 하위 계층으로 레이어를 추가 합니다 했습니다. 그런 다음 만듭니다의 경로 사용 하 여 키 프레임 애니메이션을 애니메이션을 적용 하의 `Position`합니다.

    재정의 해야이를 위해는 `TouchesEnded` 다음 코드를 추가 합니다.

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

3. 이제 및 그리기를 마친 후 응용 프로그램을 실행, 이미지와 함께 계층 추가 되 고 이동 그려지는 경로 따라:

![](graphics-animation-walkthrough-images/00-final-app.png "이미지와 함께 계층 추가 되 고 이동 그려지는 경로 따라")

## <a name="summary"></a>요약

이 문서에서는 그래픽 및 애니메이션 개념을 함께 연결 하는 예제를 통해 실행 합니다. 핵심 그래픽에서 경로 그리는 데 사용 하는 방법을 먼저 설명한는 `UIView` 사용자 터치에 대 한 응답에서입니다. 그런 다음 해당 경로 따라 이동 이미지 만들기를 코어 애니메이션을 사용 하는 방법을 살펴보았습니다.


## <a name="related-links"></a>관련 링크

- [핵심 애니메이션](~/ios/platform/graphics-animation-ios/core-animation.md)
- [핵심 그래픽](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [코어 애니메이션 레시피](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
