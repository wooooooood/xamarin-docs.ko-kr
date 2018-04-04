---
title: 연습-iOS에서 사용 하 여 터치
ms.prod: xamarin
ms.assetid: 13F8289B-7A80-4959-AF3F-57874D866DCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 58066ef0071c8105658f0d766e8f038b2bd3ddf2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough--using-touch-in-ios"></a>연습-iOS에서 사용 하 여 터치

이 연습에는 여러 유형의 터치 이벤트에 응답 하는 코드를 작성 하는 방법을 보여 줍니다. 각 예에서는 별도 화면에 포함 되어 있습니다.

- [샘플을 터치](#Touch_Samples) -터치 이벤트에 응답 하는 방법입니다.
- [제스처 인식기 샘플](#Gesture_Recognizer_Samples) – 기본 제공 제스처 인식기를 사용 하는 방법입니다.
- [사용자 지정 제스처 인식기 샘플](#Custom_Gesture_Recognizer) -사용자 지정 제스처 인식기를 구축 하는 방법입니다.

각 섹션에는 처음부터 코드를 작성 하는 지침이 포함 되어 있습니다.
[샘플 코드를 시작](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start) 완료 스토리 보드 및 메뉴 화면에 이미 포함 되어 있습니다.

 [![](ios-touch-walkthrough-images/image3.png "이 샘플에 메뉴 화면을 포함 됩니다.")](ios-touch-walkthrough-images/image3.png#lightbox)

코드에 추가 하는 스토리 보드를 다양 한 유형의 iOS에서 사용할 수 있는 터치 이벤트에 대 한 자세한 내용은 아래 지침을 따릅니다. 또는 열에서 [완성 된 샘플](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final) 모든 작업을 수행 하십시오.

<a name="Touch_Samples"/>

## <a name="touch-samples"></a>터치 샘플

이 샘플에서는 일부의 Api 터치 보여 드리겠습니다. 터치 이벤트를 구현 하는 데 필요한 코드를 추가 하려면 다음이 단계를 수행 합니다.


1. 프로젝트를 열고 **Touch_Start**합니다. 이제 모든 것이 있는지 프로젝트를 실행 하는 첫 번째와 터치는 **터치 샘플** 단추입니다. (사용할 수도 있지만 면 단추가) 다음와 같은 화면이 나타나야 합니다.
    
    [![](ios-touch-walkthrough-images/image4.png "작동 하지 않는 단추를 사용 하 여 실행 하는 샘플 응용 프로그램")](ios-touch-walkthrough-images/image4.png#lightbox)


1. 파일을 편집 **TouchViewController.cs** 클래스에 다음 두 가지 인스턴스 변수를 추가 하 고 `TouchViewController`:

    ```csharp 
    #region Private Variables
    private bool imageHighlighted = false;
    private bool touchStartedInside;
    #endregion
    ```


1. 구현 된 `TouchesBegan` 메서드를 다음 코드에 나와 있는 것 처럼:

    ```csharp 
    public override void TouchesBegan(NSSet touches, UIEvent evt)
    {
        base.TouchesBegan(touches, evt);
    
        // If Multitouch is enabled, report the number of fingers down
        TouchStatus.Text = string.Format ("Number of fingers {0}", touches.Count);
    
        // Get the current touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            // Check to see if any of the images have been touched
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                // Fist image touched
                TouchImage.Image = UIImage.FromBundle("TouchMe_Touched.png");
                TouchStatus.Text = "Touches Began";
            } else if (touch.TapCount == 2 && DoubleTouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                // Second image double-tapped, toggle bitmap
                if (imageHighlighted)
                {
                    DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe.png");
                    TouchStatus.Text = "Double-Tapped Off";
                }
                else
                {
                    DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe_Highlighted.png");
                    TouchStatus.Text = "Double-Tapped On";
                }
                imageHighlighted = !imageHighlighted;
            } else if (DragImage.Frame.Contains(touch.LocationInView(View)))
            {
                // Third image touched, prepare to drag
                touchStartedInside = true;
            }
        }
    }
    ```
    
    에 대 한 확인 하 여 사용할 수 있는이 방법은 `UITouch` 개체를 특성이 있으면 터치 발생 한 위치에 따라 일부 작업을 수행 합니다.

    * _TouchImage 내부_ – 텍스트 표시 `Touches Began` 레이블 및 이미지를 변경 합니다.
    * _DoubleTouchImage 내부_ – 제스처가 두 번 누르면 표시 되는 이미지를 변경 합니다.
    * _DragImage 내부_ -터치가 시작 되었음을 나타내는 플래그를 설정 합니다. 메서드 `TouchesMoved` 이 플래그를 사용 하 여 여부를 확인 하는 `DragImage` 설정할지 화면, 다음 단계에서를 보게 될 것으로 합니다.

    위의 코드는 개별 터치를 하나만 처리, 여전히 동작이 없는 사용자가 자신의 손가락 화면에서 이동 하는 경우. 이동에 응답 하려면 구현 `TouchesMoved` 아래 코드에 나와 있는 것 처럼:

    ```csharp 
    public override void TouchesMoved(NSSet touches, UIEvent evt)
    {
        base.TouchesMoved(touches, evt);
        // get the touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            //==== IMAGE TOUCH
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                TouchStatus.Text = "Touches Moved";
            }

            //==== IMAGE DRAG
            // check to see if the touch started in the drag me image
            if (touchStartedInside)
            {
                // move the shape
                float offsetX = touch.PreviousLocationInView(View).X - touch.LocationInView(View).X;
                float offsetY = touch.PreviousLocationInView(View).Y - touch.LocationInView(View).Y;
                DragImage.Frame = new RectangleF(new PointF(DragImage.Frame.X - offsetX, DragImage.Frame.Y - offsetY), DragImage.Frame.Size);
            }
        }
    }
    ```

    이 메서드를 가져옵니다는 `UITouch` 개체 및 터치 발생 한 위치를 확인 합니다. 터치에서 발생 한 경우 `TouchImage`, 회색 터치 이동 화면에 표시 됩니다. 

    경우 `touchStartedInside` 가 true 이면 회원님의 사용자에 게에 자신의 손가락 `DragImage` 은 이동 하 고 있습니다. 코드에 들어왔다 `DragImage` 사용자가 자신의 손가락 화면 이동 합니다.

1. 사용자가 자신의 화면에서 손가락을 강화 하거나 iOS 터치 이벤트를 취소 한 경우를 처리 해야 합니다. 이 구현 해 서 `TouchesEnded` 및 `TouchesCancelled` 아래와 같이:

    ```csharp
    public override void TouchesCancelled(NSSet touches, UIEvent evt)
    {
        base.TouchesCancelled(touches, evt);
    
        // reset our tracking flags
        touchStartedInside = false;
        TouchImage.Image = UIImage.FromBundle("TouchMe.png");
        TouchStatus.Text = "";
    }
    
    public override void TouchesEnded(NSSet touches, UIEvent evt)
    {
        base.TouchesEnded(touches, evt);
        // get the touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            //==== IMAGE TOUCH
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                TouchImage.Image = UIImage.FromBundle("TouchMe.png");
                TouchStatus.Text = "Touches Ended";
            }
        }
        // reset our tracking flags
        touchStartedInside = false;
    }
    ```
    
    두이 방법 모두 다시 설정 됩니다는 `touchStartedInside` 플래그를 false입니다. `TouchesEnded` 이 디스플레이 `TouchesEnded` 화면에 있습니다.

1. 이 시점에서 샘플 터치 화면이 완료 됩니다. 화면에 따라 어떻게 변경의 이미지를 각 상호 작용 다음 스크린샷에 표시 된 대로 확인 합니다.
        
    [![](ios-touch-walkthrough-images/image4.png "시작 응용 프로그램 화면")](ios-touch-walkthrough-images/image4.png#lightbox)
    
    [![](ios-touch-walkthrough-images/image5.png "사용자가 단추를 끈 후 화면")](ios-touch-walkthrough-images/image5.png#lightbox)
 

<a name="Gesture_Recognizer_Samples" />

##  <a name="gesture-recognizer-samples"></a>제스처 인식기 샘플

[이전 섹션](#Touch_Samples) 터치 이벤트를 사용 하 여 화면 개체를 마우스로 끌어 하는 방법을 살펴보았습니다.
이 섹션에서는 터치 이벤트를 제거 합니다 하 고 다음 제스처 인식기를 사용 하는 방법을 보여 줍니다.

-  `UIPanGestureRecognizer` 끌어 화면 이미지에 대 한 합니다.
-  `UITapGestureRecognizer` 화면에 두 개의 탭에 응답할 수 있습니다.

실행 하는 경우는 [샘플 코드를 시작](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start) 에서 클릭 하 고는 **제스처 인식기 샘플** 단추를 다음 화면이 표시 됩니다.

 [![](ios-touch-walkthrough-images/image6.png "이 화면 제스처 인식기 샘플 단추를 클릭 하면 표시")](ios-touch-walkthrough-images/image6.png#lightbox)

제스처 인식기를 구현 하려면 다음이 단계를 수행 합니다.


1. 파일을 편집 **GestureViewController.cs** 다음 인스턴스 변수를 추가 합니다.

    ```csharp
    #region Private Variables
    private bool imageHighlighted = false;
    private RectangleF originalImageFrame = RectangleF.Empty;
    #endregion
    ```

    이 인스턴스 변수 이미지의 이전 위치를 추적 하기 위해 필요 합니다.
팬 제스처 인식기 ´ ֲ는 `originalImageFrame` 는 화면에 이미지를 그리는 데 필요한 오프셋을 계산 하는 값입니다.

1. 컨트롤러에 다음 메서드를 추가 합니다.

    ```csharp
    private void WireUpDragGestureRecognizer()
    {
        // Create a new tap gesture
        UIPanGestureRecognizer gesture = new UIPanGestureRecognizer();
    
        // Wire up the event handler (have to use a selector)
        gesture.AddTarget(() => HandleDrag(gesture));  // to be defined
    
        // Add the gesture recognizer to the view
        DragImage.AddGestureRecognizer(gesture);
    }
    ```

    이 코드를 인스턴스화하는 `UIPanGestureRecognizer` 인스턴스를 선택한 보기에 추가 합니다.
메서드의 형태로 제스처 대상을 할당 했습니다 확인 `HandleDrag` –이 메서드는 다음 단계에서 제공 됩니다.

1. HandleDrag를 구현 하려면 컨트롤러에 다음 코드를 추가 합니다.

    ```csharp
    private void HandleDrag(UIPanGestureRecognizer recognizer)
    {
        // If it's just began, cache the location of the image
        if (recognizer.State == UIGestureRecognizerState.Began)
        {
            originalImageFrame = DragImage.Frame;
        }
    
        // Move the image if the gesture is valid
        if (recognizer.State != (UIGestureRecognizerState.Cancelled | UIGestureRecognizerState.Failed
            | UIGestureRecognizerState.Possible))
        {
            // Move the image by adding the offset to the object's frame
            PointF offset = recognizer.TranslationInView(DragImage);
            RectangleF newFrame = originalImageFrame;
            newFrame.Offset(offset.X, offset.Y);
            DragImage.Frame = newFrame;
        }
    }
    ```

    위의 코드는 먼저 제스처 인식기의 상태를 확인 하 고 화면 이미지를 이동 합니다. 이 코드 위치에서 컨트롤러 수 이제 지원 화면 하나의 이미지를 끌기 합니다.


1. 추가 `UITapGestureRecognizer` DoubleTouchImage에 표시 되 고 이미지 변경 됩니다. 다음 메서드를 추가 `GestureViewController` 컨트롤러:

    ```csharp
    private void WireUpTapGestureRecognizer()
    {
        // Create a new tap gesture
        UITapGestureRecognizer tapGesture = null;
    
        // Report touch
        Action action = () => {
            TouchStatus.Text = string.Format("Image touched at: {0}",tapGesture.LocationOfTouch(0, DoubleTouchImage));
    
            // Toggle the image
            if (imageHighlighted)
            {
                DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe.png");
            }
            else
            {
                DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe_Highlighted.png");
            }
            imageHighlighted = !imageHighlighted;
        };
    
        tapGesture = new UITapGestureRecognizer(action);
    
        // Configure it
        tapGesture.NumberOfTapsRequired = 2;
    
        // Add the gesture recognizer to the view
        DoubleTouchImage.AddGestureRecognizer(tapGesture);
    }
    ```

    이 코드는 매우 유사에 대 한 코드는 `UIPanGestureRecognizer` 비슷하지만 사용 하 여 대상에 대 한 대리자를 사용 하는 `Action`합니다. 

1. 수정 해야 할 마지막 작업은 `ViewDidLoad` 방금 추가한 메서드를 호출 하는 것에 있도록 합니다. 다음 코드와 같이 표시 하므로 ViewDidLoad를 변경 합니다.

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
    
        Title = "Gesture Recognizers";
    
        // Save initial state
        originalImageFrame = DragImage.Frame;
    
        WireUpTapGestureRecognizer();
        WireUpDragGestureRecognizer();
    }
    ```

    값을 초기화 했습니다도 확인 `originalImageFrame`합니다.


1. 응용 프로그램을 실행 하 고 두 개의 이미지가 상호 작용 합니다.
다음 스크린 샷에서 이러한 상호 작용의 한 예:
    
    [![](ios-touch-walkthrough-images/image7.png "이 스크린 샷 끌어서 상호 작용을 보여 줍니다.")](ios-touch-walkthrough-images/image7.png#lightbox)



<a name="Custom_Gesture_Recognizer"/>

## <a name="custom-gesture-recognizer"></a>사용자 지정 제스처 인식기

이 섹션에서는 사용자 지정 제스처 인식기를 작성 하는 이전 섹션에서 개념을 적용 합니다. 사용자 지정 제스처 인식기는 서브 클래스 `UIGestureRecognizer`, 화면에 사용자를 "V" 그릴 때 인식 됩니다 힙이고 비트맵을 설정/해제 합니다. 다음 스크린 샷에서이 화면이의 예시:

 [![](ios-touch-walkthrough-images/image8.png "응용 프로그램에서는 사용자가 화면에 'V'를 그리면 인식")](ios-touch-walkthrough-images/image8.png#lightbox)

사용자 지정 제스처 인식기를 만들려면 다음이 단계를 수행 합니다.


1. 프로젝트에 새 클래스를 추가 `CheckmarkGestureRecognizer`, 다음 코드와 같은 모양 확인:

    ```csharp
    using System;
    using CoreGraphics;
    using Foundation;
    using UIKit;
    
    namespace Touch
    {
        public class CheckmarkGestureRecognizer : UIGestureRecognizer
        {
            #region Private Variables
            private CGPoint midpoint = CGPoint.Empty;
            private bool strokeUp = false;
            #endregion
    
            #region Override Methods
            /// <summary>
            ///   Called when the touches end or the recognizer state fails
            /// </summary>
            public override void Reset()
            {
                base.Reset();
    
                strokeUp = false;
                midpoint = CGPoint.Empty;
            }
    
            /// <summary>
            ///   Is called when the fingers touch the screen.
            /// </summary>
            public override void TouchesBegan(NSSet touches, UIEvent evt)
            {
                base.TouchesBegan(touches, evt);
    
                // we want one and only one finger
                if (touches.Count != 1)
                {
                    base.State = UIGestureRecognizerState.Failed;
                }
    
                Console.WriteLine(base.State.ToString());
            }
    
            /// <summary>
            ///   Called when the touches are cancelled due to a phone call, etc.
            /// </summary>
            public override void TouchesCancelled(NSSet touches, UIEvent evt)
            {
                base.TouchesCancelled(touches, evt);
                // we fail the recognizer so that there isn't unexpected behavior
                // if the application comes back into view
                base.State = UIGestureRecognizerState.Failed;
            }
    
            /// <summary>
            ///   Called when the fingers lift off the screen
            /// </summary>
            public override void TouchesEnded(NSSet touches, UIEvent evt)
            {
                base.TouchesEnded(touches, evt);
                //
                if (base.State == UIGestureRecognizerState.Possible && strokeUp)
                {
                    base.State = UIGestureRecognizerState.Recognized;
                }
    
                Console.WriteLine(base.State.ToString());
            }
    
            /// <summary>
            ///   Called when the fingers move
            /// </summary>
            public override void TouchesMoved(NSSet touches, UIEvent evt)
            {
                base.TouchesMoved(touches, evt);
    
                // if we haven't already failed
                if (base.State != UIGestureRecognizerState.Failed)
                {
                    // get the current and previous touch point
                    CGPoint newPoint = (touches.AnyObject as UITouch).LocationInView(View);
                    CGPoint previousPoint = (touches.AnyObject as UITouch).PreviousLocationInView(View);
    
                    // if we're not already on the upstroke
                    if (!strokeUp)
                    {
                        // if we're moving down, just continue to set the midpoint at
                        // whatever point we're at. when we start to stroke up, it'll stick
                        // as the last point before we upticked
                        if (newPoint.X >= previousPoint.X && newPoint.Y >= previousPoint.Y)
                        {
                            midpoint = newPoint;
                        }
                        // if we're stroking up (moving right x and up y [y axis is flipped])
                        else if (newPoint.X >= previousPoint.X && newPoint.Y <= previousPoint.Y)
                        {
                            strokeUp = true;
                        }
                        // otherwise, we fail the recognizer
                        else
                        {
                            base.State = UIGestureRecognizerState.Failed;
                        }
                    }
                }
    
                Console.WriteLine(base.State.ToString());
            }
            #endregion
        }
    }
    ```

    Reset 메서드를 호출한 경우는 `State` 속성 변경 하거나 `Recognized` 또는 `Ended`합니다. 사용자 지정 제스처 인식기에서 설정 하는 모든 내부 상태를 다시 설정 하는 시간입니다.
이제 클래스 다음에 사용자 응용 프로그램을 상호 작용할 때 새로 시작 하며 인식 하는 동작을 다시 시도 준비가 될 수 있습니다.



1. 이전 정의한 사용자 지정 제스처 인식기 했으므로 (`CheckmarkGestureRecognizer`) 편집는 **CustomGestureViewController.cs** 파일을 다음 두 가지 인스턴스 변수를 추가 합니다.

    ```csharp
    #region Private Variables
    private bool isChecked = false;
    private CheckmarkGestureRecognizer checkmarkGesture;
    #endregion
    ```

1. 인스턴스화하고 우리의 제스처 인식기를 구성 하려면 컨트롤러에 다음 메서드를 추가 합니다.

    ```csharp
    private void WireUpCheckmarkGestureRecognizer()
    {
        // Create the recognizer
        checkmarkGesture = new CheckmarkGestureRecognizer();
    
        // Wire up the event handler
        checkmarkGesture.AddTarget(() => {
            if (checkmarkGesture.State == (UIGestureRecognizerState.Recognized | UIGestureRecognizerState.Ended))
            {
                if (isChecked)
                {
                    CheckboxImage.Image = UIImage.FromBundle("CheckBox_Unchecked.png");
                }
                else
                {
                    CheckboxImage.Image = UIImage.FromBundle("CheckBox_Checked.png");
                }
                isChecked = !isChecked;
            }
        });
    
        // Add the gesture recognizer to the view
        View.AddGestureRecognizer(checkmarkGesture);
    }
    ```

1. 편집 `ViewDidLoad` 호출 되도록 `WireUpCheckmarkGestureRecognizer`다음 코드 조각에서와 같이:

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
    
        // Wire up the gesture recognizer
        WireUpCheckmarkGestureRecognizer();
    }
    ```

1. 응용 프로그램을 실행 하 고 화면에 "V" 그려 봅니다. 다음 스크린샷에 표시 된 것 처럼 있습니다을 변경, 표시 되는 이미지가 나타납니다.
    
    [![](ios-touch-walkthrough-images/image9.png "확인 단추")](ios-touch-walkthrough-images/image9.png#lightbox)
    
    [![](ios-touch-walkthrough-images/image10.png "검사 되지 않은 단추")](ios-touch-walkthrough-images/image10.png#lightbox)



터치 iOS에서 이벤트에 응답 하는 다른 방법을 설명 했습니다 섹션 3 위의: 터치 이벤트, 기본 제공 제스처 인식기를 사용 하 여 또는 사용자 지정 제스처 인식기로 합니다.



## <a name="related-links"></a>관련 링크

- [iOS (샘플)를 시작 터치](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS 최종 터치 (샘플)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
