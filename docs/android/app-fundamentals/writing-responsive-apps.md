---
title: 응답성이 뛰어난 응용 프로그램 작성
ms.prod: xamarin
ms.assetid: 452DF940-6331-55F0-D130-002822BBED55
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: 1900a4fc42778db07e78c41bbc0acfafbd594cdf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024191"
---
# <a name="writing-responsive-applications"></a>응답성이 뛰어난 응용 프로그램 작성

응답성이 뛰어난 GUI를 유지 관리 하기 위한 키 중 하나는 백그라운드 스레드에서 장기 실행 작업을 수행 하는 것입니다. 따라서 GUI는 차단 되지 않습니다. 사용자에 게 표시할 값을 계산 하 고이 값을 계산 하는 데 5 초가 소요 된다고 가정해 보겠습니다.

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

이는 작동 하지만 응용 프로그램은 값이 계산 되는 동안 5 초 동안 "중단" 됩니다. 이 시간 동안 앱은 사용자 상호 작용에 응답 하지 않습니다. 이 문제를 해결 하기 위해 백그라운드 스레드에 대 한 계산을 수행 하려고 합니다.

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

이제 백그라운드 스레드에서 값을 계산 하 여 GUI가 계산 중에 응답성을 유지 합니다. 그러나 계산이 완료 되 면 앱이 충돌 하 여 로그에 그대로 남아 있습니다.

```shell
E/mono    (11207): EXCEPTION handling: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):
E/mono    (11207): Unhandled Exception: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):   at Android.Runtime.JNIEnv.CallVoidMethod (IntPtr jobject, IntPtr jmethod, Android.Runtime.JValue[] parms)
E/mono    (11207):   at Android.Widget.TextView.set_Text (IEnumerable`1 value)
E/mono    (11207):   at MonoDroidDebugging.Activity1.SlowMethod ()
```

Gui를 GUI 스레드에서 업데이트 해야 하기 때문입니다. 코드는 스레드 풀 스레드에서 GUI를 업데이트 하 여 앱이 충돌 합니다. 백그라운드 스레드에서 값을 계산 해야 하지만 GUI 스레드에서 업데이트를 수행 해야 합니다 .이 [작업은 작업. RunOnUIThread](xref:Android.App.Activity.RunOnUiThread*)를 사용 하 여 처리 됩니다.

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

이 코드는 정상적으로 작동 합니다. 이 GUI는 응답을 유지 하며 계산이 복합 되 면 제대로 업데이트 됩니다.

참고이 기술은 비용이 많이 드는 값을 계산 하는 데만 사용 되는 것은 아닙니다. 웹 서비스 호출 또는 인터넷 데이터 다운로드와 같이 백그라운드에서 수행할 수 있는 장기 실행 작업에 사용할 수 있습니다.
