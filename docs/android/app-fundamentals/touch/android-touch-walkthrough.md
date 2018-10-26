---
title: 연습-Android 터치 사용
ms.prod: xamarin
ms.assetid: E281F89B-4142-4BD8-8882-FB65508BF69E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/09/2018
ms.openlocfilehash: c4192f22ebd0ad1cde27745f5439c2d18a268ed3
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111027"
---
# <a name="walkthrough---using-touch-in-android"></a>연습-Android 터치 사용

응용 프로그램의 이전 섹션에서 개념을 사용 하는 방법을 참조 알려 주세요. 4 가지 작업을 사용 하 여 응용 프로그램을 만들겠습니다. 첫 번째 활동은 메뉴 또는 다양 한 Api를 설명 하기 위해 다른 작업을 시작 하는 스위치 보드 됩니다. 다음 스크린샷은 기본 활동을 보여 줍니다.

[![Me 예 스크린샷을 사용 하 여 단추를 제공 하는 터치](android-touch-walkthrough-images/image14.png)](android-touch-walkthrough-images/image14.png#lightbox)

첫 번째 활동을 터치 샘플 보기를 변경 하는 것에 대 한 이벤트 처리기를 사용 하는 방법을 표시 됩니다. 제스처 인식기 활동 살펴보겠습니다 서브 클래스 하는 방법을 `Android.View.Views` 이벤트 처리 뿐만 아니라 축소 제스처를 처리 하는 방법을 보여 줍니다. 세 번째이자 마지막 활동 **사용자 지정 제스처**에 표시 하는 방법을 사용 하 여 사용자 지정 제스처입니다. 작업을 더 쉽게 따르고 absorb를 중단 하 게 됩니다이 연습에서는 작업 중 하나에 집중 하는 각 섹션을 사용 하 여 섹션으로 합니다.

## <a name="touch-sample-activity"></a>터치 샘플 활동

-   프로젝트를 엽니다 **TouchWalkthrough\_시작**합니다. **MainActivity** 이동할 모든 집합인 &ndash; 까지 작업의 터치 동작을 구현 하는 것입니다. 응용 프로그램을 실행 하 고 클릭 **Touch 샘플**, 다음 작업이 시작 됩니다.

    [![터치 표시 시작 된 작업의 스크린샷](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

-   작업 시작을 확인 했습니다 했으므로 파일을 엽니다 **TouchActivity.cs** 에 대 한 처리기를 추가 합니다 `Touch` 이벤트는 `ImageView`:

    ```csharp
    _touchMeImageView.Touch += TouchMeImageViewOnTouch;
    ```

-   다음으로 다음 메서드를 추가 합니다 **TouchActivity.cs**:

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

처리 하는 위의 코드에서 확인 합니다 `Move` 및 `Down` 과 같이 동작 합니다. 도 사용자 수를 들지는 손가락 되므로이 `ImageView`이동할 수 있습니다, 또는 사용자가 고 압력 변경 될 수 있습니다. 이러한 유형의 변경이 발생 한 `Move` 동작 합니다.

사용자 터치 때마다 합니다 `ImageView`, `Touch` 이벤트가 발생 하 고 처리기가 메시지를 표시 하는 **Touch 시작** 화면에서 다음 스크린샷에 표시 된 대로:

[![Touch 시작 된 작업의 스크린샷](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

사용자는 터치으로 `ImageView`, **Touch 시작** 에 표시 됩니다는 `TextView`합니다. 사용자는 더 이상으로 변경 합니다 `ImageView`, 메시지 **종료 Touch** 에 표시 됩니다는 `TextView`다음 스크린샷에 표시 된 것 처럼:

[![터치 종료 된 활동의 스크린샷](android-touch-walkthrough-images/image16.png)](android-touch-walkthrough-images/image16.png#lightbox)


## <a name="gesture-recognizer-activity"></a>제스처 인식기 활동

이제 제스처 인식기 작업을 구현할 수 있습니다. 이 작업에서는 화면에 대 한 뷰를 끌어서 축소-확대를 구현 하는 한 가지 방법은 설명 하는 방법을 보여 줍니다.

-   새 작업을 호출 응용 프로그램에 추가 `GestureRecognizer`합니다.
    다음 코드와 같이이 작업에 대 한 코드를 편집 합니다.

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

-   추가 하는 새 Android 프로젝트에 보고 하 고 이름을 `GestureRecognizerView`입니다. 이 클래스에 다음 변수를 추가 합니다.

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

-   에 다음 생성자를 추가 `GestureRecognizerView`합니다. 이 생성자는 추가 `ImageView` 작업을 합니다. 이 시점에서 코드를 아직 컴파일되지 &ndash; 클래스를 만들어야 `MyScaleListener` 사용 하 여 크기 조정에 도움이 되도록는 `ImageView` 사용자 pinches 경우:

    ```csharp
    public GestureRecognizerView(Context context): base(context, null, 0)
    {
        _icon = context.Resources.GetDrawable(Resource.Drawable.Icon);
        _icon.SetBounds(0, 0, _icon.IntrinsicWidth, _icon.IntrinsicHeight);
        _scaleDetector = new ScaleGestureDetector(context, new MyScaleListener(this));
    }
    ```

-   활동에 이미지를 그릴 재정의 해야 합니다 `OnDraw` 다음 코드 조각에 나와 있는 것 처럼 뷰 클래스의 메서드. 이 코드를 이동 합니다 `ImageView` 하 여 지정 된 위치 `_posX` 및 `_posY` 배율에 따라 이미지 크기 조정으로:

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

-   다음 인스턴스 변수를 업데이트 해야 `_scaleFactor` pinches 사용자로는 `ImageView`합니다. 클래스 추가 `MyScaleListener`합니다. 이 클래스는 사용자 pinches 하는 경우 Android에서 발생할 수 있는 확장 이벤트에 대 한 수신 대기를 `ImageView`입니다.
    다음 내부 클래스를 추가 `GestureRecognizerView`합니다. 이 클래스는를 `ScaleGesture.SimpleOnScaleGestureListener`입니다. 이 클래스는 편의 클래스는 수신기 제스처의 하위 집합에 관심이 있는 경우 서브클래싱 할 수 있습니다.

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

-   다음 메서드를 재정의 해야 `GestureRecognizerView` 는 `OnTouchEvent`합니다. 다음 코드는이 메서드 구현의 전체를 나열합니다. 많은 양의 코드를 여기에 있도록 잠시 및 여기서 진행 상황을 확인 합니다. 가장 먼저이 메서드는 필요한 경우 아이콘 크기를 조정 됩니다 &ndash; 이 호출 하 여 처리 됩니다 `_scaleDetector.OnTouchEvent`합니다. 그런 다음이 메서드를 호출 하는 작업을 파악 하려고 합니다.

    - 사용자 화면에 접촉 하는 경우 X 및 Y 위치 및 화면을 터치 하는 첫 번째 포인터의 ID 기록 했습니다.

    - 사용자가 터치 화면에서를 이동 하는 경우에서는 파악 합니다. 사용자는 포인터를 이동 하는 정도입니다.

    - 사용자가 손가락을 화면 리프트가 제스처 추적 중지 됩니다 했습니다.

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
    이 시작 될 때 화면 아래 스크린샷과 비슷하게 표시 됩니다.

    [![Android 아이콘을 사용 하 여 제스처 인식기 시작 화면](android-touch-walkthrough-images/image17.png)](android-touch-walkthrough-images/image17.png#lightbox)

-   이제 아이콘을 터치 하 고 화면으로 끕니다. 축소 확대/축소 제스처를 시도 합니다. 특정 시점 화면은 다음 스크린 샷과 같이 보일 수 있습니다.

    [![화면에서 이리저리 제스처 이동 아이콘](android-touch-walkthrough-images/image18.png)](android-touch-walkthrough-images/image18.png#lightbox)

이 시점에서 부여 해야 사용자가 직접 pat를 뒷면: Android 응용 프로그램에서 축소-확대만 구현 했습니다! 짧은 휴식을 가져와이 연습의 세 번째이자 마지막 활동으로 이동할 수 있습니다. &ndash; 사용자 지정 제스처를 사용 하 여 합니다.

## <a name="custom-gesture-activity"></a>사용자 지정 제스처 작업

이 연습에서는 최종 화면 사용자 지정 제스처를 사용 합니다.

이 연습에서는 제스처 라이브러리 이미 제스처 도구를 사용 하 여 생성 되어 프로젝트 파일에 추가할 **리소스/원시/제스처**합니다. 방해가 정리의이 비트를 사용 하 여 연습에서 마지막 활동을 사용 하 여 로그온 수 있습니다.

-   라는 레이아웃 파일을 추가 **사용자 지정\_제스처\_layout.axml** 다음 콘텐츠를 사용 하 여 프로젝트입니다. 프로젝트에 이미 모든 이미지는 **리소스** 폴더:

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

-   그런 다음 프로젝트에 새 활동을 추가 하 고 이름을 `CustomGestureRecognizerActivity.cs`입니다. 클래스에 다음 두 줄의 코드로 표시로 두 인스턴스 변수를 추가 합니다.

    ```csharp
    private GestureLibrary _gestureLibrary;
    private ImageView _imageView;
    ```

-   편집 된 `OnCreate` 이 메서드의 활동 한다는는 다음 코드와 유사 합니다. 이 코드 상황을 설명 하는 데 1 분 걸릴 수 있습니다. 가장 먼저 수행 됩니다 인스턴스화할를 `GestureOverlayView` 활동의 루트 뷰로 설정 합니다.
    이벤트 처리기를 할당 합니다 `GesturePerformed` 이벤트를 `GestureOverlayView`입니다. 그런 다음 이전에 생성 된 레이아웃 파일을 확장 하 고의 자식 뷰에 추가 된 `GestureOverlayView`합니다. 변수를 초기화 하는 최종 단계입니다 `_gestureLibrary` 응용 프로그램 리소스에서 제스처 파일을 로드 합니다. 어떤 이유로 제스처 파일을 로드할 수 없습니다, 경우 많지 않습니다이 작업으로 수행할 수 종료 되도록 합니다.

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

-   메서드를 구현 해야 할 사항이 `GestureOverlayViewOnGesturePerformed` 다음 코드 조각과에서 같이 합니다. 경우는 `GestureOverlayView` 제스처를 감지 합니다.이 메서드를 다시 호출 합니다. 가장 먼저 가져올는 `IList<Prediction>` 제스처를 호출 하 여 일치 하는 개체 `_gestureLibrary.Recognize()`합니다. Linq 약간 사용 하 여 가져오기는 `Prediction` 제스처에 대 한 가장 높은 점수를 포함 합니다.

    일치 하는 경우 충분 한 점수를 높은 제스처 다음 이벤트 처리기 아무 작업도 수행 하지 않고 종료 합니다. 그렇지 않은 경우 예측의 이름을 확인 하 고 제스처의 이름을 기반으로 표시 되 고 이미지를 변경 합니다.

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

-   응용 프로그램을 실행 하 고 사용자 지정 제스처 인식기 작업을 시작 합니다. 다음 스크린샷과 비슷하게 표시 됩니다.

    [![저와 스크린 샷 이미지를 제공 하는 확인](android-touch-walkthrough-images/image19.png)](android-touch-walkthrough-images/image19.png#lightbox)

    이제 화면에서 확인 표시를 그리는 및 비트맵 표시 되는 다음 스크린샷과에서 같이 같아야 합니다.

    [![확인 표시가 그려지는 확인을 인식할 수 있습니다.](android-touch-walkthrough-images/image20.png)](android-touch-walkthrough-images/image20.png#lightbox)

    마지막으로, 화면에 자유 곡선을 그립니다. 이 스크린 샷에 표시 된 것 처럼 확인란을 원래 이미지로 다시 변경 해야 합니다.

    [![Scribble 화면에서 원래 이미지 표시 됩니다.](android-touch-walkthrough-images/image21.png)](android-touch-walkthrough-images/image21.png#lightbox)

이제 터치 및 제스처를 Xamarin.Android를 사용 하 여 Android 응용 프로그램에 통합 하는 방법을 이해를 해야 합니다.


## <a name="related-links"></a>관련 링크

- [Android 터치 (샘플) 시작](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android 터치 최종 (샘플)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
