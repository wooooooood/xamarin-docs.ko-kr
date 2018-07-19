---
title: 연습-작업 상태를 저장 합니다.
description: 활동 수명 주기 가이드;의 상태를 저장 이론 설명 했습니다. 이제 예제를 살펴보겠습니다.
ms.prod: xamarin
ms.assetid: A6090101-67C6-4BDD-9416-F2FB74805A87
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: e282eeb8732bd5294da4ec4e3fe337e81107c8f3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30767429"
---
# <a name="walkthrough---saving-the-activity-state"></a>연습-작업 상태를 저장 합니다.

_활동 수명 주기 가이드;의 상태를 저장 이론 설명 했습니다. 이제 예제를 살펴보겠습니다._

## <a name="activity-state-walkthrough"></a>활동 상태 연습

열어 보겠습니다는 **ActivityLifecycle_Start** 프로젝트 (에 [ActivityLifecycle](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle) 샘플)를 빌드하고 실행 합니다. 이 매우 간단한 프로젝트는 활동 수명 주기 및 다양 한 수명 주기 메서드를 호출 하는 방법을 보여 주기 위해 두 개의 활동입니다. 응용 프로그램의 화면을 시작할 때 `MainActivity` 표시 됩니다. 

[![활동 A 화면](saving-state-images/01-activity-a-sml.png)](saving-state-images/01-activity-a.png#lightbox)

### <a name="viewing-state-transitions"></a>보기 상태 전환

이 샘플의 각 메서드는 활동 상태를 표시 하는 IDE 응용 프로그램 출력 창에 씁니다. (Visual Studio에서 출력 창을 열려면 입력 **CTRL ALT-O**;를 Mac 용 Visual Studio에서 출력 창을 열려면 클릭 합니다. **보기 > 패드 > 응용 프로그램 출력**.)

출력 창 표시 상태 변경의 응용 프로그램을 처음 시작 하는 경우 *활동 A*: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

클릭는 **활동 B 시작** 단추를 보면 *활동 A* 일시 중지 및 중지 하는 동안 *활동 B* 해당 상태의 변경 내용을 통해 이동: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.SecondActivity] Activity B - OnCreate
[ActivityLifecycle.SecondActivity] Activity B - OnStart
[ActivityLifecycle.SecondActivity] Activity B - OnResume
[ActivityLifecycle.MainActivity] Activity A - OnStop
```

결과적으로, *활동 B* 시작 되 고 대신 표시할 *활동 A*: 

[![활동 B 화면](saving-state-images/02-activity-b-sml.png)](saving-state-images/02-activity-b.png#lightbox)

클릭는 **다시** 단추를 *활동 B* 소멸 되 고 *활동 A* 이 다시 시작: 

```shell
[ActivityLifecycle.SecondActivity] Activity B - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnRestart
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
[ActivityLifecycle.SecondActivity] Activity B - OnStop
[ActivityLifecycle.SecondActivity] Activity B - OnDestroy
```
### <a name="adding-a-click-counter"></a>클릭 하 여 카운터를 추가합니다.

다음으로 계산 하 고 클릭 횟수를 표시 하는 단추 할 수 있도록 응용 프로그램을 변경 하려고 합니다. 첫째, 추가 하겠습니다.는 `_counter` 인스턴스 변수를 `MainActivity`:

```csharp
int _counter = 0;
```

다음으로 편집는 **Resource/layout/Main.axml** 레이아웃 파일을 새 추가 `clickButton` 사용자가 단추를 클릭 하는 횟수를 표시 하는 합니다. 그 결과 **Main.axml** 는 다음과 같습니다. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <Button
        android:id="@+id/myButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/mybutton_text" />
    <Button
        android:id="@+id/clickButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/counterbutton_text" />
</LinearLayout>
```

끝에 다음 코드를 추가 해 보겠습니다는 [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) 에서 메서드 `MainActivity` &ndash; 이 코드에서 이벤트를 클릭 하는 처리는 `clickButton`:

```csharp
var clickbutton = FindViewById<Button> (Resource.Id.clickButton);
clickbutton.Text = Resources.GetString (
    Resource.String.counterbutton_text, _counter);
clickbutton.Click += (object sender, System.EventArgs e) =>
{
    _counter++;
    clickbutton.Text = Resources.GetString (
        Resource.String.counterbutton_text, _counter);
} ;
```

값을 표시를 증가 시킨 경우 구축 하 고 앱을 다시 실행 새 하는 단추가 표시 `_counter` 클릭할 때마다에:

[![터치 개수를 추가 합니다.](saving-state-images/03-touched-sml.png)](saving-state-images/03-touched.png#lightbox)

하지만 가로 모드로 장치를 회전할 우리 경우이 카운트는 손실 됩니다.

[![카운트를 0으로 설정 가로 방향으로 회전](saving-state-images/05-rotate-nosave-sml.png)](saving-state-images/05-rotate-nosave.png#lightbox)

응용 프로그램 출력을 검사 하 것을 확인할 *활동 A* 가 일시 중지, 중지, 삭제, 다시, 다시 시작 세로에서 가로 모드를 회전 하는 동안 다시 시작 합니다. 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

때문에 *활동 A* 소멸 되 고 장치를 회전 하는 경우는 다시에 다시 만들어집니다는 인스턴스 상태를 잃게 됩니다. 다음으로 저장 하 고 인스턴스 상태를 복원 하는 코드를 추가 합니다.

### <a name="adding-code-to-preserve-instance-state"></a>Preserve 인스턴스 상태에 코드 추가

메서드를 추가 해 보겠습니다 `MainActivity` 인스턴스 상태를 저장 합니다. 하기 전에 *활동 A* 가 소멸 Android 자동으로 호출 [OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) 하 고 전달는 [번들](https://developer.xamarin.com/api/type/Android.OS.Bundle/) 우리의 인스턴스 상태를 저장 하는 데 사용할 수 있는 것입니다. 정수 값으로 취급 클릭 횟수를 저장 하려면 사용 하겠습니다.

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
    outState.PutInt ("click_count", _counter);
    Log.Debug(GetType().FullName, "Activity A - Saving instance state");

    // always call the base implementation!
    base.OnSaveInstanceState (outState);    
}
```

때 *활동 A* 다시 생성 하 고 다시 시작 되 면 Android 전달이 `Bundle` 에 다시 우리의 `OnCreate` 메서드. 코드를 추가 해 보겠습니다 `OnCreate` 복원 하는 `_counter` 값 전달 기능에서 `Bundle`합니다. 줄 바로 앞에 다음 코드를 추가 합니다. 여기서 `clickbutton` 정의 됩니다. 

```csharp
if (bundle != null)
{
    _counter = bundle.GetInt ("click_count", 0);
    Log.Debug(GetType().FullName, "Activity A - Recovered instance state");
}
```

작성 하 고 응용 프로그램을 다시 실행 한 후 두 번째 단추를 몇 번 클릭 합니다. 가로 모드로 장치를 회전할 म 개수가 유지 됩니다!

[![보존 4의 수를 보여 주는 화면을 회전](saving-state-images/06-rotate-save-sml.png)](saving-state-images/06-rotate-save.png#lightbox)


발생 한 내용을 확인할 수 있는 출력 창에 살펴보겠습니다.
    
```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - Saving instance state
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - Recovered instance state
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
``` 

하기 전에 [OnStop](https://developer.xamarin.com/api/member/Android.App.Activity.OnStop/) 메서드를 호출 했지만 새 우리의 `OnSaveInstanceState` 메서드가 저장 하는 `_counter` 값에 `Bundle`합니다. Android 전달이 `Bundle` 호출 때 의견 보내기 다시 우리의 `OnCreate` 메서드와 우리 사용를 복원할 수 있었습니다.는 `_counter` 값부터입니다.


## <a name="summary"></a>요약

이 연습에서 상태 데이터를 유지 하기 위해 활동 수명 주기에 대 한 지식을 사용 했습니다. 



## <a name="related-links"></a>관련 링크

- [ActivityLifecycle (샘플)](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle)
- [작업 수명 주기](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Android 활동](https://developer.xamarin.com/api/type/Android.App.Activity/)
