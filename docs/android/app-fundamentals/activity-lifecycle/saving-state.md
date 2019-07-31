---
title: 연습 - 작업 상태 저장
description: 활동 수명 주기 가이드의 저장 상태에 대 한 이론적 원리를 다루었습니다. 이제 예를 살펴보겠습니다.
ms.prod: xamarin
ms.assetid: A6090101-67C6-4BDD-9416-F2FB74805A87
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 9fafc6965c5d2dec79f440579a5cf3746a545bae
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68644390"
---
# <a name="walkthrough---saving-the-activity-state"></a>연습 - 작업 상태 저장

_활동 수명 주기 가이드의 저장 상태에 대 한 이론적 원리를 다루었습니다. 이제 예를 살펴보겠습니다._

## <a name="activity-state-walkthrough"></a>활동 상태 연습

[Activitylifecycle 주기](https://docs.microsoft.com/samples/xamarin/monodroid-samples/activitylifecycle) 샘플에서 **ActivityLifecycle_Start** 프로젝트를 열고 빌드하고 실행 해 보겠습니다. 이는 작업 수명 주기를 보여 주는 두 가지 작업 및 다양 한 수명 주기 방법이 호출 되는 방법을 보여 주는 매우 간단한 프로젝트입니다. 응용 프로그램을 시작 하면 다음과 같은 화면이 `MainActivity` 표시 됩니다.

[![작업 화면](saving-state-images/01-activity-a-sml.png)](saving-state-images/01-activity-a.png#lightbox)

### <a name="viewing-state-transitions"></a>상태 전환 보기

이 샘플의 각 메서드는 작업 상태를 나타내는 IDE 응용 프로그램 출력 창에 씁니다. (Visual Studio에서 출력 창을 열려면 **CTRL + ALT-O**;를 입력 하 여 Mac용 Visual Studio에서 출력 창을 열고 **보기 > 패드 > 응용 프로그램 출력**을 클릭 합니다.

앱이 처음 시작 되 면 출력 창에 *작업 A*의 상태 변경 내용이 표시 됩니다. 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

활동 b **시작** 단추를 클릭 하면 활동 *b* 가 상태 변경을 거치는 동안 *활동* 일시 중지 및 중지가 표시 됩니다. 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.SecondActivity] Activity B - OnCreate
[ActivityLifecycle.SecondActivity] Activity B - OnStart
[ActivityLifecycle.SecondActivity] Activity B - OnResume
[ActivityLifecycle.MainActivity] Activity A - OnStop
```

결과적으로 작업 *B* 가 시작 되 고 *작업 a*대신 표시 됩니다. 

[![작업 B 화면](saving-state-images/02-activity-b-sml.png)](saving-state-images/02-activity-b.png#lightbox)

**뒤로** 단추를 클릭 하면 *작업 B* 가 제거 되 고 *작업 A* 가 다시 시작 됩니다. 

```shell
[ActivityLifecycle.SecondActivity] Activity B - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnRestart
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
[ActivityLifecycle.SecondActivity] Activity B - OnStop
[ActivityLifecycle.SecondActivity] Activity B - OnDestroy
```
### <a name="adding-a-click-counter"></a>클릭 카운터 추가

다음으로 응용 프로그램을 변경 하 여 클릭 횟수를 계산 하 고 표시 하는 단추를 표시 합니다. 먼저 `_counter` `MainActivity`다음과 같이 인스턴스 변수를 추가 해 보겠습니다.

```csharp
int _counter = 0;
```

그런 다음 **리소스/레이아웃/주. axml** 레이아웃 파일을 편집 하 고 사용자가 단추를 `clickButton` 클릭 한 횟수를 표시 하는 새를 추가 합니다. 결과로 생성 되는 **기본. axml** 은 다음과 유사 합니다. 

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

[OnCreate](xref:Android.App.Activity.OnCreate*) 메서드의 `MainActivity` 끝에`clickButton`다음 코드를 추가 해 보겠습니다. 이코드에서는에서clickevents를처리합니다.&ndash;

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

앱을 빌드 및 실행 하는 경우 각 클릭 `_counter` 에서 값을 증가 시키고 표시 하는 새 단추가 표시 됩니다.

[![Touch count 추가](saving-state-images/03-touched-sml.png)](saving-state-images/03-touched.png#lightbox)

그러나 장치를 가로 모드로 회전 하면이 수가 손실 됩니다.

[![가로로 회전 하면 수가 0으로 다시 설정 됩니다.](saving-state-images/05-rotate-nosave-sml.png)](saving-state-images/05-rotate-nosave.png#lightbox)

응용 프로그램 출력을 검사 하면 *작업 A* 가 일시 중지 됨, 중지 됨, 제거 됨, 다시 시작 됨, 세로 모드에서 가로 모드로 회전 하는 동안 다시 시작 됨을 확인할 수 있습니다. 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

장치를 회전할 때 *작업 A* 가 제거 되 고 다시 생성 되므로 해당 인스턴스 상태가 손실 됩니다. 다음에는 인스턴스 상태를 저장 하 고 복원 하는 코드를 추가 합니다.

### <a name="adding-code-to-preserve-instance-state"></a>인스턴스 상태를 유지 하는 코드 추가

에 `MainActivity` 메서드를 추가 하 여 인스턴스 상태를 저장 해 보겠습니다. *작업 A* 가 제거 되기 전에 Android에서 자동으로 [OnSaveInstanceState](xref:Android.App.Activity.OnSaveInstanceState*) 를 호출 하 고 인스턴스 상태를 저장 하는 데 사용할 수 있는 [번들](xref:Android.OS.Bundle) 을 전달 합니다. 이를 사용 하 여 클릭 횟수를 정수 값으로 저장할 수 있습니다.

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
    outState.PutInt ("click_count", _counter);
    Log.Debug(GetType().FullName, "Activity A - Saving instance state");

    // always call the base implementation!
    base.OnSaveInstanceState (outState);    
}
```

*작업 A* 가 다시 만들어지고 다시 시작 되 면 Android는 `Bundle` 이를 `OnCreate` 메서드에 다시 전달 합니다. 에 전달 `OnCreate` 된에서`_counter` 값을 복원 하는 코드를 추가 해 보겠습니다. `Bundle` `clickbutton` 가 정의 된 줄 바로 앞에 다음 코드를 추가 합니다. 

```csharp
if (bundle != null)
{
    _counter = bundle.GetInt ("click_count", 0);
    Log.Debug(GetType().FullName, "Activity A - Recovered instance state");
}
```

앱을 다시 빌드하고 실행 한 다음 두 번째 단추를 몇 번 클릭 합니다. 장치를 가로 모드로 회전할 때 수가 유지 됩니다.

[![화면을 회전 하면 유지 되는 4 개 수가 표시 됩니다.](saving-state-images/06-rotate-save-sml.png)](saving-state-images/06-rotate-save.png#lightbox)

출력 창에서 발생 한 결과를 확인해 보겠습니다.

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

[OnStop](xref:Android.App.Activity.OnStop) 메서드를 호출 하기 전에 새 `OnSaveInstanceState` 메서드를 `_counter` 호출 하 여에 `Bundle`값을 저장 했습니다. Android `Bundle` 는메서드`OnCreate` 를 호출할 때이를 다시 microsoft에 전달 했으며,이 값을사용하여중단된위치에값을복원할수있었습니다.`_counter`

## <a name="summary"></a>요약

이 연습 작업 수명 주기에 대 한 정보를 사용 하 여 상태 데이터를 유지 했습니다.

## <a name="related-links"></a>관련 링크

- [ActivityLifecycle 주기 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/activitylifecycle)
- [작업 수명 주기](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Android 작업](xref:Android.App.Activity)
