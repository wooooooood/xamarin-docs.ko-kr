---
title: 응답성이 뛰어난 응용 프로그램 작성
ms.prod: xamarin
ms.assetid: 452DF940-6331-55F0-D130-002822BBED55
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: a1642c4cbb790cf09d2a31e629408afc61d5b7ab
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121778"
---
# <a name="writing-responsive-applications"></a>응답성이 뛰어난 응용 프로그램 작성

GUI를 차단 하지 않습니다 이렇게 백그라운드 스레드에서 장기 실행 작업 방법은 응답성이 뛰어난 GUI를 유지 하기 위한 키 중 하나입니다. 값을 계산 하는 5 초를 사용 하지만 해당 사용자에 게 표시할 값을 계산 하려는 경우를 가정해 보십시오.

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

이 작동 하지만 응용 프로그램은 "중단" 5 초에 대 한 값은 계산 하는 동안. 이 시간 동안 앱을 모든 사용자 상호 작용에 응답 하지 않습니다. 이 문제를 해결 하는 백그라운드 스레드에서 계산을 수행 해야 합니다.

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

이제 계산 하는 동안이 GUI 계속 응답 하므로 백그라운드 스레드에서 값을 계산 했습니다. 그러나 계산 완료 되 면 앱 충돌 로그에 유지 합니다.

```shell
E/mono    (11207): EXCEPTION handling: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):
E/mono    (11207): Unhandled Exception: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):   at Android.Runtime.JNIEnv.CallVoidMethod (IntPtr jobject, IntPtr jmethod, Android.Runtime.JValue[] parms)
E/mono    (11207):   at Android.Widget.TextView.set_Text (IEnumerable`1 value)
E/mono    (11207):   at MonoDroidDebugging.Activity1.SlowMethod ()
```

즉, GUI 스레드에서 GUI를 업데이트 해야 합니다. 코드는 응용 프로그램에서 충돌을 일으키는 ThreadPool 스레드에서 GUI를 업데이트 합니다. 백그라운드 스레드에서 값을 계산 하지만 다음 GUI 스레드에서 사용 하 여 처리 되는 업데이트를 수행 해야 [Activity.RunOnUIThread](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)):

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

이 코드는 예상 대로 작동 합니다. 이 GUI 응답성을 유지 하 고 계산이 완료 대기 되 면 제대로 업데이트 됩니다.

참고가이 기술은 비용이 많이 드는 값을 계산 하는 데 사용 되지 않습니다. 웹 서비스 호출 또는 다운로드 인터넷 데이터와 같은 백그라운드에서 수행 될 수 있는 장기 실행 작업에 사용할 수 있습니다.
