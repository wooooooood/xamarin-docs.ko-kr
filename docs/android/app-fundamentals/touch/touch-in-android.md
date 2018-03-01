---
title: "Android에서 터치"
ms.topic: article
ms.prod: xamarin
ms.assetid: 405A1FA0-4EFA-4AEB-B672-F36307B9CF16
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 83997018f4e08567a9150bf1e21374a98c8ddb4e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="touch-in-android"></a>Android에서 터치

훨씬 것 처럼 iOS, Android 만듭니다 사용자의 물리적 상호 화면에 대 한 데이터를 보유 하는 개체 &ndash; 는 `Android.View.MotionEvent` 개체입니다. 이 개체는 어떤 기능을 수행 하는 등의 데이터를 보유, 배치 터치 걸린 경우 얼마나 많은 압력 적용, 등입니다. A `MotionEvent` 개체로의 이동은 다음 값을 나눕니다.

-  초기 터치 화면 또는 터치 종료 간에 이동 터치 같은 동작의 형식을 설명 하는 작업 코드입니다.

-  위치를 설명 하는 축 값의 집합은 `MotionEvent` 및 여기서 터치 수행 되 고 때 터치 발생 했는지, 그리고 사용 된 양을 압력와 같은 기타 이동 속성입니다.
   축 값 위 목록의 모든 축 값을 설명 하지 않으면 하므로 장치에 따라 달라질 수 있습니다.


`MotionEvent` 개체 응용 프로그램에서 적절 한 메서드에 전달 됩니다. Xamarin.Android 응용 프로그램의 터치 이벤트에 대응 하는 방법은 세 가지가 있습니다.

-  *이벤트 처리기를 할당 `View.Touch`*  - `Android.Views.View` 클래스에는 `EventHandler<View.TouchEventArgs>` 응용 프로그램에 대 한 처리기를 할당할 수 있습니다. 이 일반적인.NET 동작 합니다.

-  *구현 `View.IOnTouchListener`*  -이 인터페이스의 인스턴스 뷰를 사용 하는 보기 개체에 할당 될 수 있습니다. `SetOnListener` 메서드입니다. 이벤트 처리기를 할당 하는 기능적는 `View.Touch` 이벤트입니다. 여러 뷰가 변경 될 때 필요할 수 있는 몇 가지 일반적인 또는 공유 논리 이면을 보다 효과적으로 클래스를 만들고이 메서드를 각 보기 고유한 이벤트 처리기를 할당 하는 보다 구현 됩니다.

-  *재정의 `View.OnTouchEvent`*  -Android 하위 클래스의 모든 뷰에 `Android.Views.View`합니다. 보기에 접촉 될 경우 Android를 호출 합니다는 `OnTouchEvent` 전달는 `MotionEvent` 개체를 매개 변수로 합니다.


> [!NOTE]
> **참고:** 모든 Android 장치의 터치 스크린을 지원 합니다. 

매니페스트 파일에 다음 태그를 추가 하면 Google Play를 디스플레이만 터치 사용에 이러한 장치에 앱:

```xml
<uses-configuration android:reqTouchScreen="finger" />
```

## <a name="gestures"></a>제스처

제스처는 터치 스크린에서 필기 셰이프. 제스처는 일련의 화면에 대 한 연결이의 다른 지점에서 만든 점으로 구성 된 각 스트로크에 하나 이상의 스트로크를 가질 수 있습니다. Android에는 다양 한 유형의 제스처를 화면 멀티 터치를 포함 하는 복잡 한 제스처에 플 하는 간단한 링에서 지원할 수 있습니다.

Android에서 제공 된 `Android.Gestures` 관리 및 제스처에 응답 하기 위한 구체적으로 네임 스페이스입니다. 에 모든 제스처의 핵심은 라는 특수 클래스 `Android.Gestures.GestureDetector`합니다. 이 클래스에 제스처 및 기준으로 이벤트를 수신 이름에서 알 수 있듯이 `MotionEvents` 운영 체제에서 제공 합니다.

제스처 탐지기를 구현 하려면 활동 인스턴스화해야는 `GestureDetector` 클래스의 인스턴스를 제공 하 고 `IOnGestureListener`와 다음 코드 조각에서와 같이:

```csharp
GestureOverlayView.IOnGestureListener myListener = new MyGestureListener();
_gestureDetector = new GestureDetector(this, myListener);
```

또한 활동은 OnTouchEvent를 구현 하 고는 MotionEvent 제스처 탐지기에 전달 해야 합니다. 다음 코드 조각에서는 이러한 예제를 보여 줍니다.

```csharp
public override bool OnTouchEvent(MotionEvent e)
{
    // This method is in an Activity
    return _gestureDetector.OnTouchEvent(e);
}
```

인스턴스가 `GestureDetector` 제스처 식별 관심 있는 알려줍니다 활동 또는 응용 프로그램 이벤트를 발생 시키는 또는에서 제공 하는 콜백을 통해 `GestureDetector.IOnGestureListener`합니다.
이 인터페이스 다양 한 제스처에 대 한 6 개의 메서드를 제공합니다.

-  *OnDown* -호출 탭 발생 하지만 해제 되지 않습니다.

-  *OnFling* -는 플 링 발생 하 고 이벤트를 트리거한 시작 및 끝 터치에 데이터를 제공 하는 경우 호출 합니다.

-  *OnLongPress* -긴 키를 눌러 발생할 때 호출 합니다.

-  *OnScroll* -스크롤 이벤트가 발생할 때 호출 합니다.

-  *OnShowPress* -는 OnDown 발생 한 후 및 이동 호출 또는 이벤트를 수행 하지 않았습니다.

-  *OnSingleTapUp* -단일 tap 발생할 때 호출 합니다.


대부분의 경우에서 응용 프로그램만 관심 제스처의 하위 집합입니다. 응용 프로그램 해야 GestureDetector.SimpleOnGestureListener 클래스를 확장 하 고 이벤트에 해당 하는 메서드를 재정의 하는 경우에에 관심이 있습니다.

## <a name="custom-gestures"></a>사용자 지정 제스처

제스처는 사용자가 응용 프로그램 사용에 대 한 훌륭한 방법입니다. 지금까지 살펴본 것 Api 간단한 제스처에 대 한 충분 하지만 약간 더 복잡 한 제스처 성가실 맺을 수 있습니다. 더 복잡 한 제스처에 도움이 되도록 Android API의 일부 사용자 지정 제스처에 대 한 부담을 간편 하 게 Android.Gestures 네임 스페이스의 다른 집합을 제공 합니다.

### <a name="creating-custom-gestures"></a>사용자 지정 제스처 만들기

Android SDK는 Android 1.6 이후 제스처 작성기 라는 에뮬레이터에 미리 설치 된 응용 프로그램 함께 제공 됩니다. 이 응용 프로그램 개발자를 응용 프로그램에 포함할 수 있는 미리 정의 된 제스처를 만들 수 있습니다. 다음 스크린 샷에서 제스처 작성기의 예를 보여 줍니다.

[![예제 동작과 상호 스크린샷 제스처 작성기의](touch-in-android-images/image11.png)](touch-in-android-images/image11.png)

Google Play 제스처 도구 라는이 응용 프로그램의 향상된 된 버전을 찾을 수 있습니다. 제스처 도구는 제스처 작성기와 매우 비슷하며 점을 제외 하 고 생성 된 후 제스처를 테스트할 수 있습니다. 다음 스크린샷은이 제스처 작성기:

[![예제 동작과 상호 스크린 샷의 제스처 도구](touch-in-android-images/image12.png)](touch-in-android-images/image12.png)

제스처 도구 생성 될 때 테스트할 제스처 수 있으므로 사용자 지정 제스처를 만들기 위한 좀 더 유용 하 고 Google Play를 통해 쉽게 사용할 수 있습니다.

제스처 도구 화면에 그리기 및 이름을 할당 하 여 제스처를 만들 수 있습니다. 제스처는 만들어진 후 장치의 SD 카드에 이진 파일에 저장 됩니다. 이 파일에서 장치를 검색 하 고 폴더 /Resources/raw에서 응용 프로그램과 함께 패키징되 어 해야 합니다. Android 디버그 브리지를 사용 하 여 에뮬레이터에서이 파일을 검색할 수 있습니다. 다음 예제에서는 Galaxy Nexus에서 파일을 응용 프로그램의 리소스 디렉터리에 복사 합니다.

```shell
$ adb pull /storage/sdcard0/gestures <projectdirectory>/Resources/raw
```

파일을 검색 한 후 디렉터리 /Resources 내 응용 프로그램과 함께 패키지에 포함 된/원시 이어야 합니다. 다음 코드 조각에 표시 된 것과 같이이 제스처 파일을 사용 하는 가장 쉬운 방법은 GestureLibrary에 파일을 로드할 수입니다.

```csharp
GestureLibary myGestures = GestureLibraries.FromRawResources(this, Resource.Raw.gestures);
if (!myGestures.Load())
{
    // The library didn't load, so close the activity.
    Finish();
}
```

### <a name="using-custom-gestures"></a>사용자 지정 제스처를 사용 하 여

활동에서 사용자 지정 제스처를 인식 하도록 해당 레이아웃에 추가 된 Android.Gesture.GestureOverlay 개체가 있어야 합니다. 다음 코드 조각에 프로그래밍 방식으로 한 GestureOverlayView 활동에 추가 하는 방법을 보여 줍니다.

```csharp
GestureOverlayView gestureOverlayView = new GestureOverlayView(this);
gestureOverlayView.AddOnGesturePerformedListener(this);
SetContentView(gestureOverlayView);
```

다음 XML 조각에는 GestureOverlayView를 선언적으로 추가 하는 방법을 보여 줍니다.

```xml
<android.gesture.GestureOverlayView
    android:id="@+id/gestures"
    android:layout_width="match_parent "
    android:layout_height="match_parent" />
```

`GestureOverlayView` 에 제스처를 그리기의 프로세스 중 발생할 수 있는 몇 가지 이벤트가 있습니다. 가장 관심 있는 이벤트는 `GesturePeformed`합니다. 이 이벤트는 사용자가 자신의 제스처 드로잉을 완료 하는 경우에 발생 합니다.

이 이벤트는 발생 하는 때에 작업 요청을 `GestureLibrary` 시도 하 고 제스처 도구에 의해 사용자 제스처 중 하나를 만들고 있는 제스처와 일치 합니다. `GestureLibrary` 예측 개체 목록을 반환 합니다.

제스처 중 하나의 이름과 점수를 보유 하는 각 예측 개체는 `GestureLibrary`합니다. 높을수록 점수, 가능성이 높습니다 예측에 명명 된 제스처에는 사용자가 그린 제스처와 일치 합니다.
일반적으로 1.0 보다 낮은 점수는 낮은 일치 항목으로 간주 됩니다.

다음 코드는 제스처를 일치 하는 예를 보여 줍니다.

```csharp
private void GestureOverlayViewOnGesturePerformed(object sender, GestureOverlayView.GesturePerformedEventArgs gesturePerformedEventArgs)
{
    // In this example _gestureLibrary was instantiated in OnCreate
    IEnumerable<Prediction> predictions = from p in _gestureLibrary.Recognize(gesturePerformedEventArgs.Gesture)
    orderby p.Score descending
    where p.Score > 1.0
    select p;
    Prediction prediction = predictions.FirstOrDefault();

    if (prediction == null)
    {
        Log.Debug(GetType().FullName, "Nothing matched the user's gesture.");
        return;
    }

    Toast.MakeText(this, prediction.Name, ToastLength.Short).Show();
}
```

이렇게 Xamarin.Android 응용 프로그램에서 터치 및 제스처를 사용 하는 방법 이해를 해야 합니다. 주세요 이제 연습은 단계로 넘어갑니다와 모든 샘플 응용 프로그램의 개념을 표시 합니다.



## <a name="related-links"></a>관련 링크

- [Android 터치 (샘플)를 시작](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android 터치 최종 (샘플)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
