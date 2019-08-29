---
title: '연습: Xamarin.ios에서 Touch 사용'
description: 이 문서에서는 Xamarin.ios 응용 프로그램에서 터치를 처리 하는 방법, 샘플 터치 상호 작용, 제스처 인식기 및 사용자 지정 제스처 인식자에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 13F8289B-7A80-4959-AF3F-57874D866DCA
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 1e1bba499a14b92965a5fb544ccf321b0e453894
ms.sourcegitcommit: 3d21bb1a6d9b78b65aa49917b545c39d44aa3e3c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70065447"
---
# <a name="walkthrough-using-touch-in-xamarinios"></a>연습: Xamarin.ios에서 Touch 사용

이 연습에서는 여러 종류의 터치 이벤트에 응답 하는 코드를 작성 하는 방법을 보여 줍니다. 각 예제는 별도의 화면에 포함 되어 있습니다.

- [터치 샘플](#Touch_Samples) – 터치 이벤트에 응답 하는 방법입니다.
- [제스처 인식기 샘플](#Gesture_Recognizer_Samples) – 기본 제공 제스처 인식기를 사용 하는 방법입니다.
- [사용자 지정 제스처 인식기 샘플](#Custom_Gesture_Recognizer) – 사용자 지정 제스처 인식기를 빌드하는 방법입니다.

각 섹션에는 코드를 처음부터 작성 하는 지침이 포함 되어 있습니다.
[시작 샘플 코드](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-touch-start) 는 이미 전체 스토리 보드 및 메뉴 화면을 포함 합니다.

 [![](ios-touch-walkthrough-images/image3.png "이 샘플은 메뉴 화면을 포함 합니다.")](ios-touch-walkthrough-images/image3.png#lightbox)

아래 지침에 따라 스토리 보드에 코드를 추가 하 고 iOS에서 제공 되는 다양 한 유형의 터치 이벤트에 대해 알아보세요. 또는 [완성 된 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-touch-final) 을 열어 모든 것이 작동 하는지 확인 합니다.

<a name="Touch_Samples"/>

## <a name="touch-samples"></a>터치 샘플

이 샘플에서는 touch Api의 일부를 설명 합니다. 터치 이벤트를 구현 하는 데 필요한 코드를 추가 하려면 다음 단계를 수행 합니다.


1. **Touch_Start**프로젝트를 엽니다. 먼저 프로젝트를 실행 하 여 모든 것이 정상 인지 확인 하 고 **터치 샘플** 단추를 터치 합니다. 다음과 비슷한 화면이 표시 됩니다 (단추가 작동 하지 않음).

    [![](ios-touch-walkthrough-images/image4.png "작업 단추가 아닌 단추를 사용 하 여 샘플 앱 실행")](ios-touch-walkthrough-images/image4.png#lightbox)


1. **TouchViewController.cs** 파일을 편집 하 고 다음 두 인스턴스 변수를 클래스 `TouchViewController`에 추가 합니다.

    ```csharp 
    #region Private Variables
    private bool imageHighlighted = false;
    private bool touchStartedInside;
    #endregion
    ```


1. 아래 코드 `TouchesBegan` 에 나와 있는 것 처럼 메서드를 구현 합니다.

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
    
    이 메서드는 `UITouch` 개체를 확인 하 고 존재 하는 경우 터치가 발생 한 위치에 따라 동작을 수행 합니다.

    * _TouchImage 내부_ – 레이블에 텍스트 `Touches Began` 를 표시 하 고 이미지를 변경 합니다.
    * _DoubleTouchImage 내부_ – 제스처가 두 번 누르기로 면 표시 되는 이미지를 변경 합니다.
    * _DragImage 내부_ – 터치가 시작 되었음을 나타내는 플래그를 설정 합니다. 이 메서드 `TouchesMoved` 는 다음 단계에서 볼 수 있듯이 `DragImage` 이 플래그를 사용 하 여가 화면 주위에서 이동 해야 하는지 여부를 확인 합니다.

    위의 코드는 개별 접촉만 처리 하며 사용자가 화면에서 손가락을 이동 하는 경우에도 여전히 동작 하지 않습니다. 이동에 응답 하려면 아래 코드 `TouchesMoved` 와 같이을 구현 합니다.

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

    이 메서드는 `UITouch` 개체를 가져온 다음 터치가 발생 한 위치를 확인 합니다. 에서 `TouchImage`터치가 발생 한 경우 이동 된 텍스트가 화면에 표시 됩니다. 

    이 true 이면 사용자가 `DragImage` 손가락을 사용 하 여 이동 하 고 있음을 알 수 있습니다. `touchStartedInside` 사용자가 화면 주위 `DragImage` 에서 손가락을 움직이면 코드가 이동 합니다.

1. 사용자가 화면에서 손가락을 뗄 때 또는 iOS가 터치 이벤트를 취소 하는 경우를 처리 해야 합니다. 이를 위해 아래와 같이 `TouchesEnded` 및 `TouchesCancelled` 을 구현 합니다.

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

    이러한 두 메서드는 플래그를 `touchStartedInside` false로 다시 설정 합니다. `TouchesEnded`화면에도 `TouchesEnded` 표시 됩니다.

1. 이제 터치 샘플 화면이 종료 되었습니다. 다음 스크린샷에 표시 된 것 처럼 각 이미지와 상호 작용할 때 화면이 어떻게 달라 지는 지 확인 합니다.

    [![](ios-touch-walkthrough-images/image4.png "앱 시작 화면")](ios-touch-walkthrough-images/image4.png#lightbox)
    
    [![](ios-touch-walkthrough-images/image5.png "사용자가 단추를 끈 후 화면")](ios-touch-walkthrough-images/image5.png#lightbox)
 

<a name="Gesture_Recognizer_Samples" />

## <a name="gesture-recognizer-samples"></a>제스처 인식기 샘플

[이전 섹션](#Touch_Samples) 에서는 터치 이벤트를 사용 하 여 화면 주위에서 개체를 끄는 방법을 살펴보았습니다.
이 섹션에서는 터치 이벤트를 제거 하 고 다음 제스처 인식기를 사용 하는 방법을 보여 줍니다.

- 화면 주위에서 이미지를 끌기 위한입니다.`UIPanGestureRecognizer`
- 화면 `UITapGestureRecognizer` 에서 두 번 탭 할 때 응답 하는입니다.

[시작 샘플 코드](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-touch-start) 를 실행 하 고 **제스처 인식기 샘플** 단추를 클릭 하면 다음과 같은 화면이 표시 됩니다.

 [![](ios-touch-walkthrough-images/image6.png "제스처 인식기 샘플 단추를 클릭 하면이 화면이 표시 됩니다.")](ios-touch-walkthrough-images/image6.png#lightbox)

제스처 인식기를 구현 하려면 다음 단계를 수행 합니다.


1. **GestureViewController.cs** 파일을 편집 하 고 다음 인스턴스 변수를 추가 합니다.

    ```csharp
    #region Private Variables
    private bool imageHighlighted = false;
    private RectangleF originalImageFrame = RectangleF.Empty;
    #endregion
    ```

    이 인스턴스 변수는 이미지의 이전 위치를 추적 하는 데 필요 합니다.
팬 제스처 인식기는 화면에서 이미지 `originalImageFrame` 를 다시 그리는 데 필요한 오프셋을 계산 하는 데 값을 사용 합니다.

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

    이 코드는 `UIPanGestureRecognizer` 인스턴스를 인스턴스화하고 뷰에 추가 합니다.
메서드의 `HandleDrag` 형태로 제스처에 대상을 할당 합니다 .이 메서드는 다음 단계에서 제공 됩니다.

1. HandleDrag를 구현 하려면 다음 코드를 컨트롤러에 추가 합니다.

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

    위의 코드는 먼저 제스처 인식기의 상태를 확인 한 다음 이미지를 화면 주위로 이동 합니다. 이제이 코드를 사용 하면 컨트롤러에서 화면 주위에 있는 이미지 하나를 끌 수 있습니다.


1. DoubleTouchImage에 `UITapGestureRecognizer` 표시 되는 이미지를 변경 하는를 추가 합니다. `GestureViewController` 컨트롤러에 다음 메서드를 추가 합니다.

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

    이 코드는에 대 한 `UIPanGestureRecognizer` 코드와 매우 비슷하지만를 `Action`사용 하는 대상에 대해 대리자를 사용 하는 대신를 사용 합니다. 

1. 방금 추가한 메서드를 호출 하도록 수정 `ViewDidLoad` 해야 합니다. ViewDidLoad을 다음 코드와 같이 변경 합니다.

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

    의 `originalImageFrame`값을 초기화 하는 것도 좋습니다.


1. 응용 프로그램을 실행 하 고 두 이미지를 조작 합니다.
다음 스크린샷은 이러한 상호 작용의 한 가지 예입니다.
    
    [![](ios-touch-walkthrough-images/image7.png "이 스크린샷은 끌기 상호 작용을 보여 줍니다.")](ios-touch-walkthrough-images/image7.png#lightbox)



<a name="Custom_Gesture_Recognizer"/>

## <a name="custom-gesture-recognizer"></a>사용자 지정 제스처 인식기

이 섹션에서는 이전 섹션의 개념을 적용 하 여 사용자 지정 제스처 인식기를 작성 합니다. 사용자 지정 제스처 인식기는 하위 `UIGestureRecognizer`클래스를 사용 하 여 사용자가 화면에서 "V"를 그린 다음 비트맵을 전환 하는 경우를 인식 합니다. 다음 스크린샷에는이 화면의 예제가 나와 있습니다.

 [![](ios-touch-walkthrough-images/image8.png "사용자가 화면에서 ' V '를 그리면 앱이 인식 합니다.")](ios-touch-walkthrough-images/image8.png#lightbox)

사용자 지정 제스처 인식기를 만들려면 다음 단계를 따르세요.


1. 라는 `CheckmarkGestureRecognizer`프로젝트에 새 클래스를 추가 하 고 다음 코드와 같이 표시 되도록 합니다.

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

    Reset 메서드는 `State` 속성이 `Recognized` 또는 `Ended`로 변경 될 때 호출 됩니다. 사용자 지정 제스처 인식기에서 내부 상태 집합을 다시 설정 하는 시간입니다.
이제 클래스는 다음에 사용자가 응용 프로그램과 상호 작용 하 고 제스처를 다시 시도할 준비가 될 때 새로 시작할 수 있습니다.



1. 이제 사용자 지정 제스처 인식기 (`CheckmarkGestureRecognizer`)를 정의 했으므로 **CustomGestureViewController.cs** 파일을 편집 하 고 다음 두 인스턴스 변수를 추가 했습니다.

    ```csharp
    #region Private Variables
    private bool isChecked = false;
    private CheckmarkGestureRecognizer checkmarkGesture;
    #endregion
    ```

1. 제스처 인식기를 인스턴스화하고 구성 하려면 다음 메서드를 컨트롤러에 추가 합니다.

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

1. 다음 `ViewDidLoad` 코드 조각과 같이를 `WireUpCheckmarkGestureRecognizer`호출 하도록 편집 합니다.

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
    
        // Wire up the gesture recognizer
        WireUpCheckmarkGestureRecognizer();
    }
    ```

1. 응용 프로그램을 실행 하 고 화면에서 "V" 그리기를 시도 합니다. 다음 스크린샷에 표시 된 것 처럼 표시 되는 이미지가 변경 됩니다.
    
    [![](ios-touch-walkthrough-images/image9.png "선택 된 단추")](ios-touch-walkthrough-images/image9.png#lightbox)
    
    [![](ios-touch-walkthrough-images/image10.png "선택 취소 된 단추")](ios-touch-walkthrough-images/image10.png#lightbox)



위의 세 섹션에서는 터치 이벤트, 기본 제공 제스처 인식기 또는 사용자 지정 제스처 인식기를 사용 하 여 iOS의 터치 이벤트에 응답 하는 다양 한 방법을 보여 주었습니다.



## <a name="related-links"></a>관련 링크

- [iOS Touch 시작 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-touch-start)
- [iOS Touch Final (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-touch-final)
