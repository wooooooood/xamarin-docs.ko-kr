---
title: 응답성이 뛰어난 응용 프로그램 작성
ms.prod: xamarin
ms.assetid: 452DF940-6331-55F0-D130-002822BBED55
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: b8c113b67b3fbfa57ca86c72e11ddeb0e4e1a9ab
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="writing-responsive-applications"></a>응답성이 뛰어난 응용 프로그램 작성

응답성이 뛰어난 GUI를 유지 하는 키 중 하나는 GUI 차단 하지 않는 장기 실행 작업 백그라운드 스레드 작업을 합니다. 하지만 해당 값은 5 초를 계산 하는 사용자에 게 표시 하려면 값을 계산 하려는 경우를 가정해 보십시오.

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        SlowMethod ();
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        textview.Text = "Method Complete";
    }
}
```

이 작동 하지만, 응용 프로그램은 "중단" 5 초 동안 값이 계산 됩니다. 이 시간 동안 응용 프로그램은 모든 사용자 상호 작용에 응답 하지 않습니다. 이 해결 하려면 백그라운드 스레드에서 계산을 수행 하려면:

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        ThreadPool.QueueUserWorkItem (o => SlowMethod ());
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        textview.Text = "Method Complete";
    }
}
```

이제 계산 하는 동안이 GUI 유지 응답 하도록 백그라운드 스레드에서 값을 계산 합니다. 그러나 계산 수행 되는 경우 앱 충돌, 로그에 유지 합니다.

```shell
E/mono    (11207): EXCEPTION handling: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):
E/mono    (11207): Unhandled Exception: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):   at Android.Runtime.JNIEnv.CallVoidMethod (IntPtr jobject, IntPtr jmethod, Android.Runtime.JValue[] parms)
E/mono    (11207):   at Android.Widget.TextView.set_Text (IEnumerable`1 value)
E/mono    (11207):   at MonoDroidDebugging.Activity1.SlowMethod ()
```

즉, GUI 스레드에서 GUI를 업데이트 해야 합니다. 코드는 응용 프로그램 충돌을 일으키는 스레드 풀 스레드에서 GUI를 업데이트 합니다. 백그라운드 스레드에서 여 값을 계산 하지만 다음 사용 하 여 처리 하는 GUI 스레드에서 업데이트를 수행 해야 [Activity.RunOnUIThread](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)):

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        ThreadPool.QueueUserWorkItem (o => SlowMethod ());
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        RunOnUiThread (() => textview.Text = "Method Complete");
    }
}
```

이 코드는 예상 대로 작동 합니다. 이 GUI 응답성 유지 되 고 계산이 복합 되 면 제대로 업데이트를 가져옵니다.

참고가이 기술을 비용이 많이 드는 값을 계산한 다음에 대 한 사용 되지 않습니다. 웹 서비스 호출, 인터넷 데이터를 다운로드 하 게 백그라운드에서 수행할 수 있는 장기 실행 작업에 사용할 수 있습니다.
