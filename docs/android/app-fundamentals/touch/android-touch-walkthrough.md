---
title: 연습-터치를 사용 하 여 Android에서
ms.prod: xamarin
ms.assetid: E281F89B-4142-4BD8-8882-FB65508BF69E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/09/2018
ms.openlocfilehash: 625ba800ce498f80c0344c67e26bd79360de4002
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/10/2018
ms.locfileid: "34050561"
---
# <a name="walkthrough---using-touch-in-android"></a>연습-터치를 사용 하 여 Android에서

응용 프로그램의 이전 섹션에서 개념을 사용 하는 방법을 참조 알려 주세요. 응용 프로그램를 4 개 작업을 만듭니다. 첫 번째 활동 메뉴 또는 다양 한 Api를 설명 하기 위해 다른 활동을 시작 하는 스위치 보드 됩니다. 다음 스크린 샷에서 기본 활동을 보여 줍니다.

[![단추 Me 있는 예제 스크린 샷 터치](android-touch-walkthrough-images/image14.png)](android-touch-walkthrough-images/image14.png#lightbox)

첫 번째 활동 터치 샘플 보기를 변경 하는 것에 대 한 이벤트 처리기를 사용 하는 방법을 표시 됩니다. 제스처 인식기 활동 보여주는 서브 클래스 하는 방법을 `Android.View.Views` 이벤트를 처리할 뿐만 아니라 축소 제스처를 처리 하는 방법을 보여 줍니다. 세 번째이자 마지막 활동 **사용자 지정 제스처**, 됩니다 표시 방법을 사용 하 여 사용자 지정 제스처입니다. 작업을 진행할 흡수를 간단 하 게 하려면 활동 중 하나에 중점을 두기 각 섹션의 섹션으로이 연습에서는 분석할 수 있습니다.

## <a name="touch-sample-activity"></a>터치 샘플 활동

-   프로젝트를 열고 **TouchWalkthrough\_시작**합니다. **MainActivity** 모든 설정의 이동이 스타일은 &ndash; 는 활동에 터치 동작을 구현 하도록 합니다. 응용 프로그램을 실행 하 고 클릭 **터치 샘플**, 다음 작업 시작 해야 합니다.

    [![표시 시작 터치 있는 활동의 스크린 샷](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

-   활동 시동 확인 우리는 이제 파일을 열 **TouchActivity.cs** 에 대 한 처리기를 추가 하 고는 `Touch` 의 이벤트는 `ImageView`:

    ```csharp
    _touchMeImageView.Touch += TouchMeImageViewOnTouch;
    ```

-   다음 메서드를 다음으로 추가 **TouchActivity.cs**:

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

취급 하에 위의 코드에서 확인할 수는 `Move` 및 `Down` 와 동일한 작업을 수행 합니다. 경우에 사용자 수를 들어올리지 자신의 손가락 때문에 이것이 `ImageView`이동할 수 있습니다, 또는 사용자가 운영 되 압력 변경 될 수 있습니다. 이러한 종류의 변경은 생성 됩니다는 `Move` 동작 합니다.

사용자 작업에서 사용자가 `ImageView`, `Touch` 이벤트가 발생 하 고 처리기에 메시지가 표시 됩니다 **시작 터치** 다음 스크린샷에 표시 된 것 처럼 화면에:

[![시작 터치 있는 활동의 스크린 샷](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

사용자가 터치으로 `ImageView`, **시작 터치** 에 표시 됩니다는 `TextView`합니다. 사용자는 더 이상으로 변경 된 `ImageView`, 메시지 **종료 터치** 에 표시 됩니다는 `TextView`다음 스크린샷에 표시 된 것 처럼:

[![터치 종료 된 활동의 스크린샷](android-touch-walkthrough-images/image16.png)](android-touch-walkthrough-images/image16.png#lightbox)


## <a name="gesture-recognizer-activity"></a>제스처 인식기 활동

이제 제스처 인식기 활동 구현 있습니다. 이 활동에서는 화면 중심 보기를 끌어서 핀치 확대/축소를 구현 하는 방법을 설명 하는 방법을 보여 줍니다.

-   호출 응용 프로그램에 새 활동을 추가 `GestureRecognizer`합니다.
    다음 코드와 같이이 활동에 대 한 코드를 편집 합니다.

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

-   추가할 새 Android 프로젝트에 보고 하 고 이름을 `GestureRecognizerView`합니다. 이 클래스에 다음 변수를 추가 합니다.

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

-   에 다음 생성자를 추가 `GestureRecognizerView`합니다. 이 생성자를 추가 합니다는 `ImageView` 활동을 합니다. 이 시점에서 코드는 아직 컴파일되지 것입니다 &ndash; 클래스를 만들어야 한다는 `MyScaleListener` 크기 조정에 도움이 될 것입니다는 `ImageView` 사용자 pinches 때:

    ```csharp
    public GestureRecognizerView(Context context): base(context, null, 0)
    {
        _icon = context.Resources.GetDrawable(Resource.Drawable.Icon);
        _icon.SetBounds(0, 0, _icon.IntrinsicWidth, _icon.IntrinsicHeight);
        _scaleDetector = new ScaleGestureDetector(context, new MyScaleListener(this));
    }
    ```

-   활동에 이미지를 나타내려면 재정의 해야는 `OnDraw` 다음 코드 조각에 나와 있는 것 처럼 뷰 클래스의 메서드. 이 코드에 들어왔다는 `ImageView` 로 지정 된 위치에 `_posX` 및 `_posY` 배율 인수에 따라 이미지 크기 조정으로:

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

-   그런 다음 인스턴스 변수를 업데이트 해야 `_scaleFactor` pinches 사용자로는 `ImageView`합니다. 라는 클래스를 추가 합니다 `MyScaleListener`합니다. 이 클래스는 사용자 pinches 때 Android에서 발생할 수 있는 크기 조정 이벤트를 수신 대기는 `ImageView`합니다.
    에 내부 클래스를 다음과 같은 추가 `GestureRecognizerView`합니다. 이 클래스는는 `ScaleGesture.SimpleOnScaleGestureListener`합니다. 이 클래스는 수신기 제스처의 하위 집합에 관심이 있는 경우 하위 클래스 수 있는 편리한 클래스:

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

            _iconview.Invalidate();
            return true;
        }
    }
    ```

-   다음 방법의 재정의 해야 `GestureRecognizerView` 은 `OnTouchEvent`합니다. 다음 코드는이 방법의 전체 구현을 나열 합니다. 많은 코드는 여기에서 1 분 소요 및 여기의 진행 상황을 확인 하므로 있습니다. 가장 먼저이 메서드는 필요에 따라 아이콘 크기는 &ndash; 이 호출 하 여 처리 됩니다 `_scaleDetector.OnTouchEvent`합니다. 그런 다음이 메서드를 호출 하는 어떤 작업을 파악 하려고 합니다.

    - 사용자는 화면에 접촉에서는 X 및 Y 위치와 화면을 처리 하는 첫 번째 포인터의 ID 기록 합니다.

    - 사용자 화면에 자신의 터치 이동 된 경우 다음 म 파악 얼마나 사용자 포인터를 이동 합니다.

    - 사용자가 화면 밖 자신의 손가락 리프트 제스처는 추적 중지 됩니다 했습니다.

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
    이 시작 될 때 화면 아래 스크린샷과 같이 같아야 합니다.

    [![Android 아이콘으로 제스처 인식기 시작 화면](android-touch-walkthrough-images/image17.png)](android-touch-walkthrough-images/image17.png#lightbox)

-   이제 아이콘을 터치 하 고 화면으로 끕니다. 핀치 확대/축소 제스처를 시도 하십시오. 특정 시점에 사용자의 화면에서는 다음 스크린 샷과 같이 나타낼 수 있습니다.

    [![화면 제스처 이동 아이콘](android-touch-walkthrough-images/image18.png)](android-touch-walkthrough-images/image18.png#lightbox)

이 시점에서 부여 해야 사용자가 직접 pat 뒷면: Android 응용 프로그램에서 핀치 확대/축소를 방금 구현 했습니다! 빠른 중단 하 고이 연습에서 세 번째이자 마지막 활동으로 이동할 수 있습니다. &ndash; 사용자 지정 제스처를 사용 하 여 합니다.

## <a name="custom-gesture-activity"></a>사용자 지정 제스처 활동

이 연습에서는 최종 화면 사용자 지정 제스처를 사용 합니다.

이 연습을 위해 제스처 라이브러리 이미 제스처 도구를 사용 하 여 생성 되어 프로젝트 파일에 추가 **리소스/원시/제스처**합니다. 이 약간 방해가 à의 어 서 연습의 최종 작업이 있습니다.

-   라는 레이아웃 파일 **사용자 지정\_제스처\_layout.axml** 다음 내용 사용 하 여 프로젝트에 있습니다. 프로젝트에 이미 모든 이미지는 **리소스** 폴더:

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

-   그런 다음 프로젝트에 새 활동을 추가 하 고 이름을 `CustomGestureRecognizerActivity.cs`합니다. 다음 두 줄의 코드에서 보여 주는 요소로 클래스에 두 개의 인스턴스 변수를 추가 합니다.

    ```csharp
    private GestureLibrary _gestureLibrary;
    private ImageView _imageView;
    ```

-   편집 된 `OnCreate` 메서드는이 활동에 다음 코드와 유사 합니다. 이 코드의 진행 상황을 설명 하는 데 1 분 걸릴 수 있습니다. 가장 먼저 수행 하는 것은 인스턴스화하는 `GestureOverlayView` 활동의 루트 보기로 설정 합니다.
    이벤트 처리기를 할당 된 `GesturePerformed` 이벤트의 `GestureOverlayView`합니다. 다음에서는 이전에 생성 하는 레이아웃 파일을 확장할를 더하고의 자식 뷰는 `GestureOverlayView`합니다. 변수 초기화는 마지막 단계로 `_gestureLibrary` 응용 프로그램 리소스에서 제스처 파일을 로드 합니다. 어떤 이유로 제스처 파일을 로드할 수, 하는 경우 많지 않습니다이 작업으로 수행할 수 종료 되므로:

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

-   메서드를 구현 해야 할 사항이 `GestureOverlayViewOnGesturePerformed` 다음 코드 조각에 나와 있는 것 처럼 합니다. 경우는 `GestureOverlayView` 가 제스처를 발견이 메서드를 다시 호출 합니다. 가장 먼저 가져올는 `IList<Prediction>` 제스처를 호출 하 여 일치 하는 개체 `_gestureLibrary.Recognize()`합니다. 약간 LINQ의 사용 하 여 가져오기는 `Prediction` 제스처 점수가 가장 높은 포함 합니다.

    일치 하는 경우 충분 한 점수와 높은 제스처 다음 이벤트 처리기 아무 작업도 수행 하지 않고 종료 합니다. 그렇지 않으면 예측의 이름을 확인 하 고 제스처의 이름에 따라 표시 되 고 이미지를 변경 합니다.

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

-   응용 프로그램을 실행 하 고 사용자 지정 제스처 인식기 활동을 시작 합니다. 다음 스크린 샷에서 같은 같아야 합니다.

    [![Me 있는 스크린 샷 이미지를 제공 하는 확인](android-touch-walkthrough-images/image19.png)](android-touch-walkthrough-images/image19.png#lightbox)

    이제 화면에서 확인 표시를 그립니다 하 고 스크린샷에 표시와 같은 화면이 표시 되는 비트맵은 표시 됩니다.

    [![확인 표시가 그려지는 확인 표시는 인식](android-touch-walkthrough-images/image20.png)](android-touch-walkthrough-images/image20.png#lightbox)

    마지막으로, 화면에 자유 곡선을 그립니다. 이 스크린 샷에 표시 된 것 처럼를 원래 이미지로 다시 확인란을 변경 해야 합니다.

    [![Scribble 원본 이미지 화면에 표시 됩니다.](android-touch-walkthrough-images/image21.png)](android-touch-walkthrough-images/image21.png#lightbox)

터치 및 제스처 Xamarin.Android를 사용 하 여 Android 응용 프로그램에 통합 하는 방법 이해를 해야 합니다.


## <a name="related-links"></a>관련 링크

- [Android 터치 (샘플)를 시작](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android 터치 최종 (샘플)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
