---
title: 연습 - 작업 상태 저장
description: 작업 수명 주기 가이드;의 상태를 저장 이론 살펴 봤 이제 예제를 살펴보겠습니다.
ms.prod: xamarin
ms.assetid: A6090101-67C6-4BDD-9416-F2FB74805A87
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: c8f92e55648dff469227cc3bad981ad5f6e6d0ac
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61019156"
---
# <a name="walkthrough---saving-the-activity-state"></a>연습 - 작업 상태 저장

_작업 수명 주기 가이드;의 상태를 저장 이론 살펴 봤 이제 예제를 살펴보겠습니다._

## <a name="activity-state-walkthrough"></a>활동 상태 연습

열어 보겠습니다 합니다 **ActivityLifecycle_Start** 프로젝트 (에 [ActivityLifecycle](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle) 샘플), 빌드하고 실행 합니다. 작업 수명 주기 및 다양 한 수명 주기 메서드를 호출 하는 방법을 보여 주기 위해 두 활동이 있는 간단한 프로젝트입니다. 응용 프로그램의 화면을 시작할 때 `MainActivity` 표시 됩니다. 

[![작업 화면](saving-state-images/01-activity-a-sml.png)](saving-state-images/01-activity-a.png#lightbox)

### <a name="viewing-state-transitions"></a>보기 상태 전환

이 예제의 각 메서드는 활동 상태를 나타내기 위해 IDE 응용 프로그램 출력 창에 씁니다. (Visual Studio에서 출력 창을 열고를 입력 **CTRL-ALT-O**하려면 Mac 용 Visual Studio에서 출력 창을 열고 클릭 **보기 > 패드 > 응용 프로그램 출력**.)

출력 창 표시 상태 변경의 앱을 처음 시작 하는 경우 *활동 A*: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

클릭 하면 합니다 **시작 활동 B** 단추를 표시 *활동* 일시 중지 및 중지 하는 동안 *활동 B* 해당 상태 변경을 통해 이동: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.SecondActivity] Activity B - OnCreate
[ActivityLifecycle.SecondActivity] Activity B - OnStart
[ActivityLifecycle.SecondActivity] Activity B - OnResume
[ActivityLifecycle.MainActivity] Activity A - OnStop
```

따라서 *활동 B* 시작 되 고 대신 표시 *활동 A*: 

[![작업 B 화면](saving-state-images/02-activity-b-sml.png)](saving-state-images/02-activity-b.png#lightbox)

클릭 하면를 **다시** 단추를 *활동 B* 소멸 되기 및 *활동 A* 이 다시 시작: 

```shell
[ActivityLifecycle.SecondActivity] Activity B - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnRestart
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
[ActivityLifecycle.SecondActivity] Activity B - OnStop
[ActivityLifecycle.SecondActivity] Activity B - OnDestroy
```
### <a name="adding-a-click-counter"></a>클릭 카운터 추가

다음으로 계산 하 고 클릭 횟수를 표시 하는 단추 수 있도록 응용 프로그램을 변경 하려고 합니다. 먼저 추가 된 `_counter` 인스턴스 변수를 `MainActivity`:

```csharp
int _counter = 0;
```

다음으로, 편집할 합니다 **Resource/layout/Main.axml** 레이아웃 파일을 새 `clickButton` 사용자가 단추를 클릭 하는 횟수를 표시 하는 합니다. 결과 **Main.axml** 다음과 유사 해야 합니다. 

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

끝에 다음 코드를 추가 해 보겠습니다 합니다 [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) 에서 메서드 `MainActivity` &ndash; 이 코드 클릭 이벤트를 처리 합니다 `clickButton`:

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

증가 시킨 경우 구축 하 고 앱을 다시 실행 새 단추를 표시 하는 값을 표시 `_counter` 각 클릭 합니다.

[![터치 개수를 추가 합니다.](saving-state-images/03-touched-sml.png)](saving-state-images/03-touched.png#lightbox)

하지만 가로 모드로 장치를 회전할 것이 개수 손실 됩니다.

[![개수를 0으로 설정 가로 방향으로 회전](saving-state-images/05-rotate-nosave-sml.png)](saving-state-images/05-rotate-nosave.png#lightbox)

응용 프로그램 출력을 검사 했습니다 보면 *활동 A* 된 일시 중지, 중지, 제거, 다시 생성, 다시 시작 후에 세로에서 가로 모드로 회전 하는 동안 다시 시작 합니다. 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

때문에 *활동 A* 제거 되며 장치를 회전 하는 경우 다시 다시 해당 인스턴스 상태가 손실 됩니다. 다음으로 저장 하 고 인스턴스 상태를 복원 하는 코드를 추가 합니다.

### <a name="adding-code-to-preserve-instance-state"></a>인스턴스 상태를 유지 하는 코드를 추가

메서드를 추가 해 보겠습니다 `MainActivity` 인스턴스 상태를 저장 합니다. 하기 전에 *활동* 는 소멸 Android 자동으로 호출 [OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) 전달를 [번들](https://developer.xamarin.com/api/type/Android.OS.Bundle/) 우리는 인스턴스 상태를 저장 하는 데 사용할 수 있는 합니다. 정수 값으로는 클릭 수를 저장 하려면이 사용해 보겠습니다.

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
    outState.PutInt ("click_count", _counter);
    Log.Debug(GetType().FullName, "Activity A - Saving instance state");

    // always call the base implementation!
    base.OnSaveInstanceState (outState);    
}
```

때 *활동* 만들어지고 다시 시작 Android 전달 `Bundle` 로 다시 우리의 `OnCreate` 메서드. 코드를 추가 해 보겠습니다 `OnCreate` 복원 하는 `_counter` 전달 기능에서 값 `Bundle`합니다. 줄 바로 앞에 다음 코드를 추가 합니다. 여기서 `clickbutton` 정의 됩니다. 

```csharp
if (bundle != null)
{
    _counter = bundle.GetInt ("click_count", 0);
    Log.Debug(GetType().FullName, "Activity A - Recovered instance state");
}
```

빌드는 앱을 다시 실행 하 고 두 번째 단추를 몇 번 클릭 합니다. 가로 모드로 장치를 회전할 것 수 유지 됩니다.

[![유지 4의 수를 보여 주는 화면 회전](saving-state-images/06-rotate-save-sml.png)](saving-state-images/06-rotate-save.png#lightbox)


발생 한 내용을 확인할 출력 창에 살펴보겠습니다.
    
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

전에 [OnStop](https://developer.xamarin.com/api/member/Android.App.Activity.OnStop/) 메서드를 호출 새로운 `OnSaveInstanceState` 메서드를 호출한 저장 하는 `_counter` 값을 `Bundle`. 이 android 전달 `Bundle` 호출 때 우리에 게 다시 우리의 `OnCreate` 메서드를 복원 하려면 사용할 수 있었습니다를 `_counter` 부터 값.


## <a name="summary"></a>요약

이 연습에서는 상태 데이터를 유지 하기 위해 활동 수명 주기에 대 한 지식을 사용 했습니다. 



## <a name="related-links"></a>관련 링크

- [ActivityLifecycle (샘플)](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle)
- [작업 수명 주기](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Android 활동](https://developer.xamarin.com/api/type/Android.App.Activity/)
