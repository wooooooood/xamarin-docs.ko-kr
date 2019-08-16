---
title: Android 터치
ms.prod: xamarin
ms.assetid: 405A1FA0-4EFA-4AEB-B672-F36307B9CF16
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 274c441e0507f100697fc153a9f748de1bce4cf3
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69526075"
---
# <a name="touch-in-android"></a>Android 터치

IOS와 마찬가지로 Android는 &ndash; `Android.View.MotionEvent` 개체를 화면에 대 한 사용자의 물리적 상호 작용에 대 한 데이터를 포함 하는 개체를 만듭니다. 이 개체에는 수행 되는 작업, 터치가 발생 한 위치, 적용 된 압력 등의 데이터가 포함 됩니다. 개체 `MotionEvent` 는 다음 값으로 이동 하는 작업을 중단 합니다.

- 초기 터치, 화면 전체 터치 이동 또는 터치 종료와 같은 동작의 유형을 설명 하는 작업 코드입니다.

- 터치가 발생 하는 위치, 터치가 수행 된 `MotionEvent` 시간 및 사용 된 압력 등의 기타 이동 속성의 위치를 설명 하는 축 값 집합입니다.
   축 값은 장치에 따라 다를 수 있으므로 이전 목록에서 모든 축 값을 설명 하지는 않습니다.


개체 `MotionEvent` 는 응용 프로그램의 적절 한 메서드에 전달 됩니다. Xamarin Android 응용 프로그램에서 터치 이벤트에 응답 하는 방법에는 다음 세 가지가 있습니다.

- *이벤트 처리기를에 `View.Touch`*  `Android.Views.View` 할당 합니다. 클래스는 응용 `EventHandler<View.TouchEventArgs>` 프로그램에서 처리기를 할당할 수 있는를 포함 합니다. 이는 일반적인 .NET 동작입니다.

- 이 인터페이스의 인스턴스를 *구현 `View.IOnTouchListener`*  하면 뷰를 사용 하 여 뷰 개체에 할당할 수 있습니다. `SetOnListener`방법이. 이벤트 처리기를 `View.Touch` 이벤트에 할당 하는 것과 기능적으로 동일 합니다. 작업을 수행할 때 여러 뷰가 필요할 수 있는 일반적인 또는 공유 논리가 있는 경우 클래스를 만들고이 메서드를 구현 하는 것이 더 효율적입니다.

- *Override`View.OnTouchEvent`* -Android 하위 클래스 `Android.Views.View`의 모든 뷰입니다. 뷰가 작업 될 때 Android는를 `OnTouchEvent` 호출 하 고 개체를 `MotionEvent` 매개 변수로 전달 합니다.


> [!NOTE]
> 모든 Android 장치에서 터치 스크린을 지원 하지는 않습니다. 

매니페스트 파일에 다음 태그를 추가 하면 터치를 사용 하는 장치에만 앱이 표시 Google Play.

```xml
<uses-configuration android:reqTouchScreen="finger" />
```

## <a name="gestures"></a>제스처

터치 스크린에서 제스처는 손으로 그린 셰이프입니다. 제스처는 하나 이상의 스트로크를 가질 수 있으며, 각 스트로크는 화면과 다른 접촉 지점에서 만든 점 시퀀스로 구성 됩니다. Android는 화면 전체에서 다양 한 유형의 제스처를 지원할 수 있습니다 .이를 통해 여러 터치가 관련 된 복잡 한 제스처로 볼 수 있습니다.

Android는 제스처 `Android.Gestures` 를 관리 하 고이에 대응 하는 네임 스페이스를 제공 합니다. 모든 제스처의 핵심은 라는 `Android.Gestures.GestureDetector`특수 클래스입니다. 이름에서 알 수 있듯이이 클래스는 운영 체제에서 제공 하는에 `MotionEvents` 따라 제스처와 이벤트를 수신 대기 합니다.

제스처 감지기를 구현 하려면 다음 코드 조각과 같이 활동에서 `GestureDetector` 클래스를 인스턴스화하고의 `IOnGestureListener`인스턴스를 제공 해야 합니다.

```csharp
GestureOverlayView.IOnGestureListener myListener = new MyGestureListener();
_gestureDetector = new GestureDetector(this, myListener);
```

또한 작업은 OnTouchEvent을 구현 하 고 MotionEvent를 제스처 탐지기에 전달 해야 합니다. 다음 코드 조각에서는이에 대 한 예를 보여 줍니다.

```csharp
public override bool OnTouchEvent(MotionEvent e)
{
    // This method is in an Activity
    return _gestureDetector.OnTouchEvent(e);
}
```

인스턴스가 `GestureDetector` 관심 있는 제스처를 식별 하면 이벤트를 발생 시키거나에서 `GestureDetector.IOnGestureListener`제공 하는 콜백을 통해 활동이 나 응용 프로그램에이를 알립니다.
이 인터페이스는 다양 한 제스처에 대해 6 개의 메서드를 제공 합니다.

- *Ondown* -탭이 발생 하지만 릴리스되지 않을 때 호출 됩니다.

- *Onfling* -이벤트를 트리거한 시작 및 종료에 대 한 데이터를 제공 하 고 fling이 발생 하는 경우 호출 됩니다.

- *Onlongpress* 는 긴 누름이 발생할 때 호출 됩니다.

- *Onscroll* -scroll 이벤트가 발생 하면 호출 됩니다.

- *Onshowpress* 는 ondown이 발생 하 고 move 또는 up 이벤트가 수행 되지 않은 후에 호출 됩니다.

- *OnSingleTapUp* -단일 탭이 발생 하면 호출 됩니다.


대부분의 경우 응용 프로그램은 제스처의 하위 집합에만 관심이 있을 수 있습니다. 이 경우 응용 프로그램은 GestureDetector 클래스를 확장 하 고 관심이 있는 이벤트에 해당 하는 메서드를 재정의 해야 합니다.

## <a name="custom-gestures"></a>사용자 지정 제스처

제스처는 사용자가 응용 프로그램과 상호 작용할 수 있는 좋은 방법입니다. 지금까지 살펴본 Api는 간단한 제스처로 충분 하지만 보다 복잡 한 제스처를 위해 비트 번거로운을 입증할 수 있습니다. 더 복잡 한 제스처를 지원 하기 위해 Android는 사용자 지정 제스처와 관련 된 몇 가지 부담을 줄일 수 있는 다른 API 집합을 Android. 제스처 네임 스페이스에 제공 합니다.

### <a name="creating-custom-gestures"></a>사용자 지정 제스처 만들기

Android 1.6 이후 Android SDK는 제스처 작성기 라는 에뮬레이터에 미리 설치 된 응용 프로그램과 함께 제공 됩니다. 개발자는이 응용 프로그램을 사용 하 여 응용 프로그램에 포함 될 수 있는 미리 정의 된 제스처를 만들 수 있습니다. 다음 스크린샷은 제스처 작성기의 예를 보여 줍니다.

[![예제 제스처를 사용 하는 제스처 작성기 스크린샷](touch-in-android-images/image11.png)](touch-in-android-images/image11.png#lightbox)

제스처 도구 라는이 응용 프로그램의 향상 된 버전을 Google Play 찾을 수 있습니다. 제스처 도구는 제스처를 만든 후에 테스트할 수 있다는 점을 제외 하면 제스처 작성기와 매우 비슷합니다. 다음 스크린샷은 제스처 작성기를 보여줍니다.

[![예제 제스처를 사용 하는 제스처 도구의 스크린샷](touch-in-android-images/image12.png)](touch-in-android-images/image12.png#lightbox)

제스처 도구는 제스처를 만들 때 사용할 수 있으며 Google Play를 통해 쉽게 사용할 수 있으므로 사용자 지정 제스처를 만드는 데 더 유용 합니다.

제스처 도구를 사용 하면 화면에서 그리고 이름을 할당 하 여 제스처를 만들 수 있습니다. 만든 제스처는 장치의 SD 카드에 있는 이진 파일에 저장 됩니다. 장치에서이 파일을 검색 한 다음/Resources/raw. 폴더에 응용 프로그램과 함께 패키지 해야 합니다. 이 파일은 Android Debug Bridge를 사용 하 여 에뮬레이터에서 검색할 수 있습니다. 다음 예제에서는 Galaxy Nexus에서 응용 프로그램의 리소스 디렉터리로 파일을 복사 하는 방법을 보여 줍니다.

```shell
$ adb pull /storage/sdcard0/gestures <projectdirectory>/Resources/raw
```

파일을 검색 한 후에는/Resources/raw. 디렉터리 내에서 응용 프로그램과 함께 패키지 해야 합니다. 이 제스처 파일을 사용 하는 가장 쉬운 방법은 다음 코드 조각과 같이 GestureLibrary에 파일을 로드 하는 것입니다.

```csharp
GestureLibrary myGestures = GestureLibraries.FromRawResources(this, Resource.Raw.gestures);
if (!myGestures.Load())
{
    // The library didn't load, so close the activity.
    Finish();
}
```

### <a name="using-custom-gestures"></a>사용자 지정 제스처 사용

활동에서 사용자 지정 제스처를 인식 하려면 GestureOverlay 개체가 해당 레이아웃에 추가 되어 있어야 합니다. 다음 코드 조각에서는 작업에 GestureOverlayView를 프로그래밍 방식으로 추가 하는 방법을 보여 줍니다.

```csharp
GestureOverlayView gestureOverlayView = new GestureOverlayView(this);
gestureOverlayView.AddOnGesturePerformedListener(this);
SetContentView(gestureOverlayView);
```

다음 XML 코드 조각은 선언적으로 GestureOverlayView를 추가 하는 방법을 보여 줍니다.

```xml
<android.gesture.GestureOverlayView
    android:id="@+id/gestures"
    android:layout_width="match_parent "
    android:layout_height="match_parent" />
```

에 `GestureOverlayView` 는 제스처를 그리는 과정에서 발생 하는 몇 가지 이벤트가 있습니다. 가장 흥미로운 이벤트 `GesturePerformed`는입니다. 이 이벤트는 사용자가 제스처 그리기를 완료 했을 때 발생 합니다.

이 이벤트가 발생 하면 활동은를 `GestureLibrary` 시도 하 고 사용자가 제스처 도구에서 만든 제스처 중 하 나와 일치 하는 제스처를 일치 시킵니다. `GestureLibrary`는 예측 개체 목록을 반환 합니다.

각 예측 개체는 `GestureLibrary`에 있는 제스처 중 하나의 점수와 이름을 보유 합니다. 점수가 높을수록 예측에서 이름이 지정 된 제스처가 사용자가 그린 제스처와 일치할 가능성이 높습니다.
일반적으로 1.0 보다 낮은 점수가 일치 하지 않는 것으로 간주 됩니다.

다음 코드는 제스처와 일치 하는 예를 보여 줍니다.

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

이 작업이 완료 되 면 Xamarin Android 응용 프로그램에서 터치 및 제스처를 사용 하는 방법을 이해 해야 합니다. 이제 연습으로 이동 하 여 작업 중인 샘플 응용 프로그램의 모든 개념을 확인할 수 있습니다.



## <a name="related-links"></a>관련 링크

- [Android Touch 시작 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-touch-start)
- [Android Touch Final (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-touch-final)
