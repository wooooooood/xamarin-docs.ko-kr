---
title: Android 터치
ms.prod: xamarin
ms.assetid: 405A1FA0-4EFA-4AEB-B672-F36307B9CF16
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 84767975eece4f8f0efae1fe53463cbc053bd836
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61012919"
---
# <a name="touch-in-android"></a>Android 터치

훨씬 것 처럼 iOS, Android는 만듭니다 화면을 사용 하 여 사용자의 물리적 상호 작용에 대 한 데이터를 보유 하는 개체 &ndash; 는 `Android.View.MotionEvent` 개체입니다. 이 개체는 어떤 기능을 수행 하는 등의 데이터를 보유를 터치 수행한 경우, 얼마나 많은 압력 적용 된, 등. `MotionEvent` 개체 이동에 다음 값으로 나눕니다.

-  초기 터치 화면 또는 터치 끝 간에 이동 터치 같은 동작의 형식을 설명 하는 작업 코드입니다.

-  위치를 설명 하는 축 값 집합을 `MotionEvent` 및 터치 수행 되는, 터치 위치를 수행 하는 경우, 얼마나 많은 압력 사용한 등 기타 이동 속성입니다.
   축 값을 이전 목록의 모든 축 값을 설명 하지 않습니다 있도록 장치에 따라 달라질 수 있습니다.


`MotionEvent` 개체를 응용 프로그램에서 적절 한 메서드로 전달 됩니다. Xamarin.Android 응용 프로그램의 터치 이벤트에 응답 하는 방법은 세 가지가 있습니다.

-  *이벤트 처리기를 할당 `View.Touch`*  - `Android.Views.View` 클래스에는 `EventHandler<View.TouchEventArgs>` 응용 프로그램에 대 한 처리기를 할당할 수 있습니다. 이 일반적인.NET 동작입니다.

-  *구현 `View.IOnTouchListener`*  -이 인터페이스의 인스턴스 보기를 사용 하 여 뷰 개체를 할당할 수 있습니다. `SetOnListener` 메서드입니다. 이 기능적으로 할당 하는 이벤트 처리기는 `View.Touch` 이벤트입니다. 여러 뷰가 실행 될 때 필요할 수 있는 몇 가지 일반적인 또는 공유 논리의 경우 클래스를 만들고 할당 각 뷰에 고유한 이벤트 처리기 보다이 메서드를 구현 하는 것이 효율적 됩니다.

-  *재정의 `View.OnTouchEvent`*  -Android 서브 클래스의 모든 뷰에 `Android.Views.View`합니다. 뷰를 연결 하는 경우 Android를 호출 합니다는 `OnTouchEvent` 전달 하는 `MotionEvent` 개체를 매개 변수로 합니다.


> [!NOTE]
> 일부 Android 장치는 터치 스크린을 지원합니다. 

매니페스트 파일에 다음 태그를 추가 하면 Google Play만 표시할 터치 사용 하도록 설정 된 해당 장치에 앱:

```xml
<uses-configuration android:reqTouchScreen="finger" />
```

## <a name="gestures"></a>제스처

제스처에는 터치 스크린에서 손으로 그린 모양입니다. 제스처는 일련의 연결 화면을 사용 하 여 다른 지점에서 만든 점으로 구성 된 각 스트로크에 하나 이상의 스트로크를 가질 수 있습니다. Android는 화면에서 멀티 터치를 포함 하는 복잡 한 제스처를 플 하는 간단한 링에서 제스처를 다양 한 유형을 지원할 수 있습니다.

Android가 제공 된 `Android.Gestures` 제스처에 응답 하 고 관리에 맞게 네임 스페이스입니다. 모든 제스처의 핵심은 라는 특수 한 클래스 `Android.Gestures.GestureDetector`합니다. 이 클래스는 제스처와 기준으로 이벤트에 대 한 수신 이름에서 알 수 있듯이 `MotionEvents` 운영 체제에서 제공 합니다.

제스처 감지기를 구현 하려면 활동 인스턴스화해야를 `GestureDetector` 클래스의 인스턴스를 제공 하며 `IOnGestureListener`와 다음 코드 조각에 표시 된 것과 같이:

```csharp
GestureOverlayView.IOnGestureListener myListener = new MyGestureListener();
_gestureDetector = new GestureDetector(this, myListener);
```

또한 활동은 OnTouchEvent 구현 하 고 제스처 감지기는 MotionEvent 전달 해야 합니다. 다음 코드 조각은이 예를 보여 줍니다.

```csharp
public override bool OnTouchEvent(MotionEvent e)
{
    // This method is in an Activity
    return _gestureDetector.OnTouchEvent(e);
}
```

인스턴스가 `GestureDetector` 식별 하는 제스처 관심에 게 알려줍니다 작업 또는 응용 프로그램 이벤트를 발생 시키는 하거나에서 제공 하는 콜백을 통해 `GestureDetector.IOnGestureListener`합니다.
이 인터페이스는 다양 한 제스처에 대해 6 가지 메서드를 제공합니다.

-  *OnDown* -탭 발생 하지만 해제 되지 않습니다 때 호출 됩니다.

-  *OnFling* -는 플 링 발생 하 고 시작 및 종료 터치 이벤트를 트리거한에서 데이터를 제공 하는 경우 호출 됩니다.

-  *OnLongPress* -길게 누름 발생할 때 호출 됩니다.

-  *OnScroll* -스크롤 이벤트가 발생할 때 호출 됩니다.

-  *OnShowPress* -는 OnDown 발생 한 후 및 이동 호출 또는 이벤트를 수행 하지 않았습니다.

-  *OnSingleTapUp* -한 번의 터치로 발생할 때 호출 됩니다.


대부분의 응용 프로그램 제스처의 하위 집합에 관심이 될 수 있습니다. 이 경우 응용 프로그램 GestureDetector.SimpleOnGestureListener 클래스를 확장 하 고 관심 있는 이벤트에 해당 하는 메서드를 재정의 해야 합니다.

## <a name="custom-gestures"></a>사용자 지정 제스처

제스처는 사용자가 응용 프로그램 사용에 대 한 효과적인 방법입니다. 지금까지 살펴본 Api 간단한 제스처에 대 한 스캔이 있지만 더 복잡 한 제스처에 대해 약간 다루고자 증명할 수 있습니다. 더 복잡 한 제스처를 돕기 위해 Android는 사용자 지정 제스처와 관련 된 부담을 용이 하 게 하는 Android.Gestures 네임 스페이스에 다른 API의 집합을 제공 합니다.

### <a name="creating-custom-gestures"></a>사용자 지정 제스처 만들기

Android SDK Android 1.6 이후 응용 프로그램 제스처 Builder 라는 에뮬레이터에 미리 설치 되어 제공 됩니다. 이 응용 프로그램 개발자를를 응용 프로그램에 포함 될 수 있는 미리 정의 된 제스처를 만들 수 있습니다. 다음 스크린 샷은 제스처 작성기의 예제를 보여 줍니다.

[![예제 제스처를 사용 하 여 스크린 샷의 제스처 작성기](touch-in-android-images/image11.png)](touch-in-android-images/image11.png#lightbox)

제스처 도구 라는이 응용 프로그램의 향상 된 버전에는 Google Play 찾을 수 있습니다. 제스처 도구는 제스처 작성기와 매우 비슷한 점을 제외 하 고 생성 된 후 제스처를 테스트할 수 있습니다. 다음 스크린샷은이 제스처 작성기:

[![예제 제스처를 사용 하 여 스크린 샷의 제스처 도구](touch-in-android-images/image12.png)](touch-in-android-images/image12.png#lightbox)

제스처 도구는 생성 되는 테스트할 제스처를 수 있으므로 사용자 지정 제스처를 만드는 데 조금 더 유용 이며 Google Play를 통해 쉽게 사용할 수 있습니다.

제스처 도구는 화면에서 그리기 및 이름을 할당 하 여 제스처를 만들 수 있습니다. 제스처는 만들어진 후 장치의 SD 카드에 이진 파일에 저장 됩니다. 이 파일은 장치에서 검색 되어 패키징되 어 폴더 /Resources/raw에서 응용 프로그램을 사용 하 여 해야 합니다. Android 디버그 브리지를 사용 하 여 에뮬레이터에서이 파일을 검색할 수 있습니다. 다음 예제에서는 Galaxy Nexus에서 응용 프로그램의 리소스 디렉터리에 파일을 복사 합니다.

```shell
$ adb pull /storage/sdcard0/gestures <projectdirectory>/Resources/raw
```

파일을 검색 한 후 디렉터리 /Resources 내 응용 프로그램과 함께 패키지/원시 이어야 합니다. 이 제스처 파일을 사용 하는 가장 쉬운 방법은 다음 코드 조각에 표시 된 것과 같이 GestureLibrary에 파일을 로드할 수입니다.

```csharp
GestureLibrary myGestures = GestureLibraries.FromRawResources(this, Resource.Raw.gestures);
if (!myGestures.Load())
{
    // The library didn't load, so close the activity.
    Finish();
}
```

### <a name="using-custom-gestures"></a>사용자 지정 제스처를 사용 하 여

활동에서 사용자 지정 제스처를 인식 하도록 해당 레이아웃에 추가 된 Android.Gesture.GestureOverlay 개체가 있어야 합니다. 다음 코드 조각은 작업을 GestureOverlayView 프로그래밍 방식으로 추가 하는 방법을 보여 줍니다.

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

`GestureOverlayView` 에 제스처를 그리기 프로세스 중 발생할 수 있는 몇 가지 이벤트가 있습니다. 가장 흥미로운 이벤트 `GesturePerformed`합니다. 이 이벤트는 사용자가 제스처 그리기 완료 될 때 발생 합니다.

이 이벤트가 발생 하면 작업 요청을 `GestureLibrary` 해 제스처 도구로 제스처 중 하나를 사용 하 여 사용자가 만든 제스처와 일치 합니다. `GestureLibrary` 예측 개체의 목록을 반환 합니다.

각 예측 개체에서 제스처 중 하나의 이름과 점수를 보유 합니다 `GestureLibrary`합니다. 높을수록 점수, 가능성이 예측에 명명 된 제스처에 사용자가 그린 제스처와 일치 합니다.
일반적으로 1.0 보다 낮은 점수는 잘못 된 일치 항목으로 간주 됩니다.

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

이 작업을 사용 하 여 Xamarin.Android 응용 프로그램에서 터치 및 제스처를 사용 하는 방법 이해를 해야 합니다. 이제 연습은 이동할를 하겠습니다 모든 작업 샘플 응용 프로그램의 개념을 참조 하세요.



## <a name="related-links"></a>관련 링크

- [Android 터치 (샘플) 시작](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android 터치 최종 (샘플)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
