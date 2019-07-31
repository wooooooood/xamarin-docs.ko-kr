---
title: 연습-Android에서 Touch 사용
ms.prod: xamarin
ms.assetid: E281F89B-4142-4BD8-8882-FB65508BF69E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/09/2018
ms.openlocfilehash: 78878e62a36e3f6dd6ca3c7fcfb6413da4f0e0f9
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68644119"
---
# <a name="walkthrough---using-touch-in-android"></a>연습-Android에서 Touch 사용

작업 중인 응용 프로그램에서 이전 섹션의 개념을 사용 하는 방법을 알아보겠습니다. 네 가지 작업으로 응용 프로그램을 만듭니다. 첫 번째 작업은 다양 한 Api를 보여 주기 위해 다른 작업을 시작 하는 메뉴 또는 스위치 보드입니다. 다음 스크린샷은 주 활동을 보여 줍니다.

[![터치 Me 단추가 있는 예제 스크린샷](android-touch-walkthrough-images/image14.png)](android-touch-walkthrough-images/image14.png#lightbox)

첫 번째 작업 인 Touch 샘플은 뷰를 터치 하기 위해 이벤트 처리기를 사용 하는 방법을 보여 줍니다. 제스처 인식기 작업에서는 이벤트를 서브클래싱하 `Android.View.Views` 고 처리 하는 방법 뿐만 아니라 손가락으로 발생 하는 제스처를 처리 하는 방법을 보여 줍니다. 세 번째 및 최종 작업 인 **사용자 지정 제스처**는가 사용자 지정 제스처를 사용 하는 방법을 보여 줍니다. 더 쉽게 팔 로우 하 고 사용할 수 있도록이 연습을 섹션으로 나누고 각 섹션에서 작업 중 하나를 중점적으로 살펴보겠습니다.

## <a name="touch-sample-activity"></a>Touch 샘플 활동

-   Project **TouchWalkthrough\_Start**를 엽니다. **Mainactivity** 는 모두 go &ndash; 로 설정 되어 활동에서 터치 동작을 구현 합니다. 응용 프로그램을 실행 하 고 **터치 샘플**을 클릭 하면 다음 작업이 시작 됩니다.

    [![터치 시작이 표시 된 활동의 스크린샷](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

-   이제 활동이 시작 되는 것을 확인 했으므로 **TouchActivity.cs** 파일을 열고의 `Touch` `ImageView`이벤트에 대 한 처리기를 추가 합니다.

    ```csharp
    _touchMeImageView.Touch += TouchMeImageViewOnTouch;
    ```

-   그런 다음 **TouchActivity.cs**에 다음 메서드를 추가 합니다.

    ```csharp
    private void TouchMeImageViewOnTouch(object sender, View.TouchEventArgs touchEventArgs)
    {
        string message;
        switch (touchEventArgs.Event.Action & MotionEventActions.Mask)
        {
            case MotionEventActions.Down:
            case MotionEventActions.Move:
            message = "Touch Begins";
            break;

            case MotionEventActions.Up:
            message = "Touch Ends";
            break;

            default:
            message = string.Empty;
            break;
        }

        _touchInfoTextView.Text = message;
    }
    ```

위의 코드에서는 `Move` 및 `Down` 작업을 동일 하 게 처리 합니다. 사용자가에서 `ImageView`손가락을 리프트 하지 않을 수 있지만, 이동 하거나 사용자가 해지 하는 압력을 변경할 수 있기 때문입니다. 이러한 변경 내용으로 인해 `Move` 작업이 생성 됩니다.

사용자가에 `ImageView` `Touch` 연결할 때마다 이벤트가 발생 하 고 다음 스크린샷에 표시 된 것 처럼 처리기에서 메시지 터치가 화면에서 **시작** 됩니다.

[![터치 시작이 있는 활동의 스크린샷](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

사용자가에 `ImageView`접촉 하는 동안 **터치 시작** 이에 표시 `TextView`됩니다. 사용자가 더 이상에 `ImageView`접촉 하지 않으면 다음 스크린샷에 표시 된 것 처럼 메시지 **터치 끝** 이 `TextView`에 표시 됩니다.

[![터치 엔드가 있는 활동의 스크린샷](android-touch-walkthrough-images/image16.png)](android-touch-walkthrough-images/image16.png#lightbox)


## <a name="gesture-recognizer-activity"></a>제스처 인식기 작업

이제에서 제스처 인식기 작업을 구현할 수 있습니다. 이 활동에서는 화면을 중심으로 뷰를 끄는 방법 및 확대/축소를 구현 하는 한 가지 방법을 보여 줍니다.

-   이라는 `GestureRecognizer`응용 프로그램에 새 작업을 추가 합니다.
    이 활동에 대 한 코드를 다음 코드와 유사 하 게 편집 합니다.

    ```csharp
    public class GestureRecognizerActivity : Activity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            View v = new GestureRecognizerView(this);
            SetContentView(v);
        }
    }
    ```

-   새 Android 보기를 프로젝트에 추가 하 고 이름을 `GestureRecognizerView`로 합니다. 이 클래스에 다음 변수를 추가 합니다.

    ```csharp
    private static readonly int InvalidPointerId = -1;

    private readonly Drawable _icon;
    private readonly ScaleGestureDetector _scaleDetector;

    private int _activePointerId = InvalidPointerId;
    private float _lastTouchX;
    private float _lastTouchY;
    private float _posX;
    private float _posY;
    private float _scaleFactor = 1.0f;
    ```

-   에 `GestureRecognizerView`다음 생성자를 추가 합니다. 이 생성자는 활동 `ImageView` 에를 추가 합니다. 이 시점에서 코드는 여전히 컴파일되지 &ndash; 않습니다. 사용자가 pinches 일 때의 크기를 `ImageView` 조정 하는 데 도움이 되는 클래스 `MyScaleListener` 를 만들어야 합니다.

    ```csharp
    public GestureRecognizerView(Context context): base(context, null, 0)
    {
        _icon = context.Resources.GetDrawable(Resource.Drawable.Icon);
        _icon.SetBounds(0, 0, _icon.IntrinsicWidth, _icon.IntrinsicHeight);
        _scaleDetector = new ScaleGestureDetector(context, new MyScaleListener(this));
    }
    ```

-   활동에 이미지를 그리려면 다음 코드 조각과 같이 뷰 클래스의 `OnDraw` 메서드를 재정의 해야 합니다. 이 코드는를 `ImageView` 에 `_posX` 지정 된 위치로 이동 하 고 `_posY` 배율 인수에 따라 이미지 크기를 조정 합니다.

    ```csharp
    protected override void OnDraw(Canvas canvas)
    {
        base.OnDraw(canvas);
        canvas.Save();
        canvas.Translate(_posX, _posY);
        canvas.Scale(_scaleFactor, _scaleFactor);
        _icon.Draw(canvas);
        canvas.Restore();
    }
    ```

-   다음으로 인스턴스 변수 `_scaleFactor` 를 사용자 `ImageView`pinches 업데이트 해야 합니다. 이라는 `MyScaleListener`클래스를 추가 합니다. 이 클래스는 사용자가 pinches `ImageView`인 경우 Android에서 발생 하는 크기 조정 이벤트를 수신 대기 합니다.
    에 `GestureRecognizerView`다음 내부 클래스를 추가 합니다. 이 클래스는 `ScaleGesture.SimpleOnScaleGestureListener`입니다. 이 클래스는 제스처의 하위 집합에 관심이 있을 때 수신기에서 서브 클래스로 사용할 수 있는 편리한 클래스입니다.

    ```csharp
    private class MyScaleListener : ScaleGestureDetector.SimpleOnScaleGestureListener
    {
        private readonly GestureRecognizerView _view;

        public MyScaleListener(GestureRecognizerView view)
        {
            _view = view;
        }

        public override bool OnScale(ScaleGestureDetector detector)
        {
            _view._scaleFactor *= detector.ScaleFactor;

            // put a limit on how small or big the image can get.
            if (_view._scaleFactor > 5.0f)
            {
                _view._scaleFactor = 5.0f;
            }
            if (_view._scaleFactor < 0.1f)
            {
                _view._scaleFactor = 0.1f;
            }

            _view.Invalidate();
            return true;
        }
    }
    ```

-   `GestureRecognizerView` 에서`OnTouchEvent`재정의 해야 하는 다음 메서드는입니다. 다음 코드에서는이 메서드의 전체 구현을 보여 줍니다. 여기에는 많은 코드가 있으므로 몇 분 정도 걸리며 여기서 살펴보겠습니다. 이 메서드의 첫 번째 작업은 필요한 &ndash; 경우를 호출 `_scaleDetector.OnTouchEvent`하 여 해당 아이콘의 크기를 조정 하는 것입니다. 다음으로이 메서드를 호출 하는 작업을 파악 합니다.

    - 사용자가를 사용 하 여 화면을 작업 한 경우 X 및 Y 위치와 화면에 대 한 첫 번째 포인터의 ID를 기록 합니다.

    - 사용자가 화면에서 터치를 이동 하는 경우 사용자가 포인터를 이동한 거리를 확인 합니다.

    - 사용자가 화면에서 손가락을 리프트 하면 제스처 추적이 중지 됩니다.

    ```csharp
    public override bool OnTouchEvent(MotionEvent ev)
    {
        _scaleDetector.OnTouchEvent(ev);

        MotionEventActions action = ev.Action & MotionEventActions.Mask;
        int pointerIndex;

        switch (action)
        {
            case MotionEventActions.Down:
            _lastTouchX = ev.GetX();
            _lastTouchY = ev.GetY();
            _activePointerId = ev.GetPointerId(0);
            break;

            case MotionEventActions.Move:
            pointerIndex = ev.FindPointerIndex(_activePointerId);
            float x = ev.GetX(pointerIndex);
            float y = ev.GetY(pointerIndex);
            if (!_scaleDetector.IsInProgress)
            {
                // Only move the ScaleGestureDetector isn't already processing a gesture.
                float deltaX = x - _lastTouchX;
                float deltaY = y - _lastTouchY;
                _posX += deltaX;
                _posY += deltaY;
                Invalidate();
            }

            _lastTouchX = x;
            _lastTouchY = y;
            break;

            case MotionEventActions.Up:
            case MotionEventActions.Cancel:
            // We no longer need to keep track of the active pointer.
            _activePointerId = InvalidPointerId;
            break;

            case MotionEventActions.PointerUp:
            // check to make sure that the pointer that went up is for the gesture we're tracking.
            pointerIndex = (int) (ev.Action & MotionEventActions.PointerIndexMask) >> (int) MotionEventActions.PointerIndexShift;
            int pointerId = ev.GetPointerId(pointerIndex);
            if (pointerId == _activePointerId)
            {
                // This was our active pointer going up. Choose a new
                // action pointer and adjust accordingly
                int newPointerIndex = pointerIndex == 0 ? 1 : 0;
                _lastTouchX = ev.GetX(newPointerIndex);
                _lastTouchY = ev.GetY(newPointerIndex);
                _activePointerId = ev.GetPointerId(newPointerIndex);
            }
            break;

        }
        return true;
    }
    ```

-   이제 응용 프로그램을 실행 하 고 제스처 인식기 작업을 시작 합니다.
    시작 하는 경우 화면은 아래 스크린샷 처럼 표시 됩니다.

    [![Android 아이콘이 있는 제스처 인식기 시작 화면](android-touch-walkthrough-images/image17.png)](android-touch-walkthrough-images/image17.png#lightbox)

-   이제 아이콘을 터치 하 여 화면 주위로 끌어 옵니다. 확대/축소 제스처를 사용해 보세요. 어느 시점에서 화면이 다음 스크린샷 처럼 보일 수 있습니다.

    [![화면 주위에서 제스처 이동 아이콘](android-touch-walkthrough-images/image18.png)](android-touch-walkthrough-images/image18.png#lightbox)

이 시점에서는 Android 응용 프로그램에서 확대/축소를 구현 하는 것이 좋습니다. 빠른 중단을 수행 하 고 사용자 지정 제스처를 사용 하 여이 연습의 &ndash; 세 번째 및 최종 작업으로 이동할 수 있습니다.

## <a name="custom-gesture-activity"></a>사용자 지정 제스처 작업

이 연습의 마지막 화면에서는 사용자 지정 제스처를 사용 합니다.

이 연습을 위해 제스처 라이브러리는 이미 제스처 도구를 사용 하 여 만들어져 파일 **리소스/원시/제스처**의 프로젝트에 추가 되었습니다. 이러한 약간의 정리 작업을 통해 연습에서 최종 활동을 사용할 수 있습니다.

-   **사용자 지정\_제스처\_레이아웃. axml** 이라는 레이아웃 파일을 다음 내용이 포함 된 프로젝트에 추가 합니다. 프로젝트의 **Resources** 폴더에 모든 이미지가 이미 있습니다.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1" />
        <ImageView
            android:src="@drawable/check_me"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="3"
            android:id="@+id/imageView1"
            android:layout_gravity="center_vertical" />
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1" />
    </LinearLayout>
    ```

-   그런 다음 프로젝트에 새 활동을 추가 하 고 이름을 `CustomGestureRecognizerActivity.cs`로 만듭니다. 다음 두 줄의 코드에 표시 된 것 처럼 두 개의 인스턴스 변수를 클래스에 추가 합니다.

    ```csharp
    private GestureLibrary _gestureLibrary;
    private ImageView _imageView;
    ```

-   이 작업의 메서드를 다음 코드와 유사 하 게 편집 합니다. `OnCreate` 이 코드에서 진행 되는 사항을 설명 하는 데 몇 분 정도 걸릴 수 있습니다. 가장 먼저 할 일은를 `GestureOverlayView` 인스턴스화하고 활동의 루트 뷰로 설정 하는 것입니다.
    또한 이벤트 처리기를의 `GesturePerformed` `GestureOverlayView`이벤트에 할당 합니다. 그런 다음 앞에서 만든 레이아웃 파일을 확장 하 고이를의 `GestureOverlayView`자식 뷰로 추가 합니다. 마지막 단계는 변수 `_gestureLibrary` 를 초기화 하 고 응용 프로그램 리소스에서 제스처 파일을 로드 하는 것입니다. 어떤 이유로 든 제스처 파일을 로드할 수 없는 경우이 작업을 수행할 수 있는 작업은 많지 않으므로 종료 됩니다.

    ```csharp
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);

        GestureOverlayView gestureOverlayView = new GestureOverlayView(this);
        SetContentView(gestureOverlayView);
        gestureOverlayView.GesturePerformed += GestureOverlayViewOnGesturePerformed;

        View view = LayoutInflater.Inflate(Resource.Layout.custom_gesture_layout, null);
        _imageView = view.FindViewById<ImageView>(Resource.Id.imageView1);
        gestureOverlayView.AddView(view);

        _gestureLibrary = GestureLibraries.FromRawResource(this, Resource.Raw.gestures);
        if (!_gestureLibrary.Load())
        {
            Log.Wtf(GetType().FullName, "There was a problem loading the gesture library.");
            Finish();
        }
    }
    ```

-   다음 코드 조각과 같이 메서드 `GestureOverlayViewOnGesturePerformed` 를 구현 하는 데 필요한 최종 작업은 다음과 같습니다. 에서 `GestureOverlayView` 제스처를 검색 하면이 메서드를 다시 호출 합니다. 첫 번째는를 호출 `IList<Prediction>` `_gestureLibrary.Recognize()`하 여 제스처와 일치 하는 개체를 가져오려고 시도 하는 것입니다. 약간의 LINQ를 사용 하 여 제스처 점수가 `Prediction` 가장 높은를 가져옵니다.

    충분 한 점수가 있는 일치 하는 제스처가 없으면 이벤트 처리기는 아무 작업도 수행 하지 않고 종료 됩니다. 그렇지 않으면 예측의 이름을 확인 하 고 제스처 이름에 따라 표시 되는 이미지를 변경 합니다.

    ```csharp
    private void GestureOverlayViewOnGesturePerformed(object sender, GestureOverlayView.GesturePerformedEventArgs gesturePerformedEventArgs)
    {
        IEnumerable<Prediction> predictions = from p in _gestureLibrary.Recognize(gesturePerformedEventArgs.Gesture)
        orderby p.Score descending
        where p.Score > 1.0
        select p;
        Prediction prediction = predictions.FirstOrDefault();

        if (prediction == null)
        {
            Log.Debug(GetType().FullName, "Nothing seemed to match the user's gesture, so don't do anything.");
            return;
        }

        Log.Debug(GetType().FullName, "Using the prediction named {0} with a score of {1}.", prediction.Name, prediction.Score);

        if (prediction.Name.StartsWith("checkmark"))
        {
            _imageView.SetImageResource(Resource.Drawable.checked_me);
        }
        else if (prediction.Name.StartsWith("erase", StringComparison.OrdinalIgnoreCase))
        {
            // Match one of our "erase" gestures
            _imageView.SetImageResource(Resource.Drawable.check_me);
        }
    }
    ```

-   응용 프로그램을 실행 하 고 사용자 지정 제스처 인식기 작업을 시작 합니다. 다음 스크린샷 처럼 표시 됩니다.

    [![Check Me 이미지를 사용 하는 스크린샷](android-touch-walkthrough-images/image19.png)](android-touch-walkthrough-images/image19.png#lightbox)

    이제 화면에서 확인 표시를 그리면 다음 스크린샷에 표시 된 것 처럼 표시 되는 비트맵이 표시 됩니다.

    [![표시 된 확인 표시, 확인 표시 인식](android-touch-walkthrough-images/image20.png)](android-touch-walkthrough-images/image20.png#lightbox)

    마지막으로 화면에 자유롭게 그리기를 그립니다. 다음 스크린샷에 표시 된 것 처럼 확인란은 원래 이미지로 다시 변경 되어야 합니다.

    [![화면에서 자유롭게 그리고 원래 이미지가 표시 됩니다.](android-touch-walkthrough-images/image21.png)](android-touch-walkthrough-images/image21.png#lightbox)

이제 Xamarin.ios를 사용 하 여 Android 응용 프로그램에서 터치 및 제스처를 통합 하는 방법을 이해 했습니다.


## <a name="related-links"></a>관련 링크

- [Android Touch 시작 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-touch-start)
- [Android Touch Final (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-touch-final)
