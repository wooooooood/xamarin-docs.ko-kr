---
title: Xamarin.Android 브로드캐스트 수신기
description: 이 섹션에서는 브로드캐스트 수신기를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 04/20/2018
ms.openlocfilehash: 9c17641312384634983c2cbb34fa923a9416c9f7
ms.sourcegitcommit: 797597d902330652195931dec9ac3e0cc00792c5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/20/2018
---
# <a name="broadcast-receivers-in-xamarinandroid"></a>Xamarin.Android 브로드캐스트 수신기

_이 섹션에서는 브로드캐스트 수신기를 사용 하는 방법을 설명 합니다._

## <a name="broadcast-receiver-overview"></a>브로드캐스트 수신기 개요

A _브로드캐스트 수신기_ 는 메시지에 응답 하도록 응용 프로그램을 허용 하는 Android 구성 요소 (Android [ `Intent` ](https://developer.xamarin.com/api/type/Android.Content.Intent/)) Android 운영 체제 또는 응용 프로그램에 의해 브로드캐스팅 되며입니다. 브로드캐스트에 따라는 _게시-구독_ 모델 &ndash; 이벤트를 게시 하 고 이벤트에 관심이 있는 구성 요소에서 받은 브로드캐스트 발생 합니다. 

Android에는 두 가지 유형의 브로드캐스트 식별합니다.

* **명시적 브로드캐스트** &ndash; 브로드캐스트 이러한 유형의 특정 응용 프로그램을 대상으로 합니다. 명시적 브로드캐스트의 가장 일반적인 용도 활동이 시작 되려고 합니다. 앱에서 전화 걸기; 해야 할 경우에 명시적 브로드캐스트의 예 Android 및 전화 번호와 함께 전달 전화를 걸어야 전화 앱을 대상으로 하는 의도 전달 하는 것입니다. Android는 휴대폰 앱에 의도 라우팅할 됩니다.
* **암시적 broadcase** &ndash; 이 브로드캐스트 이러한 장치에서 모든 앱으로 디스패치 됩니다. 암시적 브로드캐스트의 예로 `ACTION_POWER_CONNECTED` 의도 합니다. 이 의도 Android 장치에서 배터리를 충전 감지 될 때마다 게시 됩니다. Android은이 이벤트에 대해 등록 된 모든 앱에 이러한 의도를 라우팅합니다.

브로드캐스트 수신기가의 서브 클래스는 `BroadcastReceiver` 유형 이므로 재정의 해야 합니다는 [ `OnReceive` ](https://developer.xamarin.com/api/member/Android.Content.BroadcastReceiver.OnReceive/p/Android.Content.Context/Android.Content.Intent/) 메서드. Android에서 실행 될 `OnReceive` 주 스레드에서 하므로이 메서드를 디자인 해야 신속 하 게 실행 합니다. 스레드를 생성할 때 주의 해야 `OnReceive` 하므로 Android 메서드가 종료 되는 프로세스를 종료할 수 있습니다. 브로드캐스트 수신기 장기 실행 작업을 수행 해야 하는 경우 다음을 예약 하는 것이 좋습니다.는 _작업_ 를 사용 하는 `JobScheduler` 또는 _Firebase 작업 발송자_합니다. 별도 지침에는 작업과 작업 일정을 설명 합니다.

_의도 필터_ Android 메시지를 올바르게 라우팅할 수 있도록 브로드캐스트 수신기를 등록 하는 데 사용 됩니다. 런타임 시 의도 필터를 지정할 수 있습니다 (이 경우에 따라 라고는 _컨텍스트 등록 수신기_ 또는 as _동적 등록_) Android 매니페스트 (한 에정적으로정의할수있습니다_수신기 매니페스트에 등록_). Xamarin.Android 제공 C# 특성 `IntentFilterAttribute`, (이 살펴봅니다이 가이드의 뒷부분에 보다 자세히) 의도 필터 등록 하는 정적으로 합니다. Android 8.0부터 수 없으면 암시적 브로드캐스트에 정적으로 등록 하는 응용 프로그램에 대 한 합니다.

매니페스트에 등록 된 수신기 및 상황에 맞는 등록 수신기 간의 주요 차이점은 응용 프로그램 매니페스트 등록 수신기에 응답할 수 하는 동안 실행 되는 동안 컨텍스트 등록 수신기 브로드캐스트에 응답 에서만 합니다. 응용 프로그램 실행 되지 않을 경우에 전파 합니다.  

브로드캐스트 수신기를 관리 하 고 브로드캐스트를 보내기 위한 Api의 두 집합이 있습니다.

1. **`Context`** &ndash; `Android.Content.Context` 클래스를 사용 하 여는 시스템 전체 이벤트에 응답 하는 브로드캐스트 수신기를 등록할 수 있습니다. `Context` 시스템 차원의 브로드캐스트를 게시 하는 데도 사용 합니다.
2. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** &ndash; 이것은 API를 통해 제공 되는 [Xamarin 지원 라이브러리 v4 NuGet 패키지](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)합니다. 이 클래스는 브로드캐스트 및 브로드캐스트 수신기를 사용 하는 응용 프로그램의 컨텍스트에서 격리에 사용 됩니다. 이 클래스는 다른 응용 프로그램에서 응용 프로그램 전용 브로드캐스트에 응답 또는 개인 수신기에 메시지를 보내는 데 유용할 수 있습니다.

브로드캐스트 수신기 대화 상자, 표시 되지 않을 수 있으며 브로드캐스트 수신기에서 활동을 시작 하는 것이 좋습니다. 브로드캐스트 수신기 해야 사용자에 게 알리고, 하는 경우 알림의 게시 해야 합니다.

바인딩할 또는 브로드캐스트 수신기 내에서 서비스를 시작 하는 것이 불가능 합니다. 

이 가이드에서는 브로드캐스트 수신기를 만드는 방법과 브로드캐스트를 받을 수 있도록 등록 하는 방법을 설명 합니다.

## <a name="creating-a-broadcast-receiver"></a>브로드캐스트 수신기 만들기

Xamarin.Android에서 브로드캐스트 수신기를 만들려는 응용 프로그램의 서브 클래스 여야는 `BroadcastReceiver` 클래스를 사용 하 여 장식할는 `BroadcastReceiverAttribute`, 재정의 `OnReceive` 메서드:

```csharp
[BroadcastReceiver(Enabled = true, Exported = false)]
public class SampleReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here.

        String value = intent.GetStringExtra("key");
    }
}
```

Xamarin.Android 클래스 컴파일되면 수신기를 등록 하는 데 필요한 메타 데이터는 AndroidManifest도 업데이트 됩니다. 정적으로 등록 브로드캐스트 수신기에 대 한는 `Enabled` 제대로으로 설정 되어 있어야 `true`, 그렇지 않으면 Android 수신기의 인스턴스를 만들 할 수 있습니다.
 
`Exported` 속성 브로드캐스트 수신기 응용 프로그램 외부의 메시지를 받을 수 있는지 여부를 제어 합니다. 속성이 명시적으로 설정 하지 않으면 속성의 기본값에 따라 의도 필터가 브로드캐스트 수신기와 관련 된 모든 Android에서 결정 됩니다. 브로드캐스트 수신기에 대 한 의도 필터가 하나 이상 있으면 Android 가정 됩니다는 `Exported` 속성은 `true`합니다. 브로드캐스트 수신기와 관련 된 의도-필터가 없는 경우 Android 값 임을 가정 `false`합니다. 

`OnReceive` 메서드 수신에 대 한 참조는 `Intent` 브로드캐스트 수신자에 게 발송 되었습니다.입니다. 이 브로드캐스트 수신기에 값을 전달 하는 것의 보낸 사람에 대 한 불가능 합니다.

### <a name="statically-registering-a-broadcast-receiver-with-an-intent-filter"></a>필터를 의도 브로드캐스트 수신기를 정적으로 등록

때는 `BroadcastReceiver` 으로 데코레이팅되 어는 [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/), Xamarin.Android 필요한 추가 `<intent-filter>` 요소는 Android 컴파일 타임에 매니페스트 합니다. 다음 코드 조각은 장치 (Android 적절 한 권한이 사용자에 의해 부여 된) 경우 부트를 끝낼 때 실행 되는 브로드캐스트 수신기의 예입니다.

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { Android.Content.Intent.ActionBootCompleted })]
public class MyBootReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Work that should be done when the device boots.     
    }
}
```

사용자 지정 의도에 응답 하는 의도 필터를 만들 수 이기도 합니다. 다음 예제를 참조하세요. 

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { "com.xamarin.example.TEST" })]
public class MySampleBroadcastReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here
    }
}
```

Android (API 수준 26) 8.0을 대상으로 하는 응용 프로그램 또는 이상이 정적으로 등록 되지 암시적 브로드캐스트에 대 한 합니다. 앱 명시적 브로드캐스트에 정적으로 여전히 등록할 수 있습니다. 다음은 이러한 제한에서 제외 되는 암시적 브로드캐스트를 사용 하는 작은 목록입니다. 이러한 예외에 설명 합니다.는 [암시적 브로드캐스트 예외](https://developer.android.com/guide/components/broadcast-exceptions.html) Android 설명서에 대 한 가이드입니다. 암시적 브로드캐스트에 관심이 있는 앱을 사용 하 여 동적으로 수행 해야 합니다는 `RegisterReceiver` 메서드. 이 작업은 다음 설명 되어 있습니다.

### <a name="context-registering-a-broadcast-receiver"></a>상황에 맞는 등록 브로드캐스트 수신기

컨텍스트 (동적 등록 라고도 함)의 등록 수신기를 호출 하 여 수행 되는 `RegisterReceiver` 메서드와 브로드캐스트 수신기 해야 등록을 취소할을 호출 하 여는 `UnregisterReceiver` 메서드. 리소스 누수를 방지 하려면 컨텍스트 (활동 또는 서비스)에 대 한 더 이상 관련이 있는 경우에 수신기를 등록 취소 해야 합니다. 예를 들어 서비스 업데이트를 사용할 수 있는 사용자에 게 표시 되는 활동을 알리기 위해 의도 브로드캐스트 수 있습니다. 활동 시작 하는 경우 이러한 의도 대 한 등록 합니다. 활동은를 배경으로 이동 하 고 사용자에 게는 더 이상 볼 것 등록을 취소 해야 수신기에서 업데이트를 표시 하기 위한 UI 볼 수 없게 되었기 때문에 때. 다음 코드 조각은 등록 및 활동의 컨텍스트에서 브로드캐스트 수신기를 등록 취소 하는 방법의 예입니다.

```csharp
[Activity(Label = "MainActivity", MainLauncher = true, Icon = "@mipmap/icon")]
public class MainActivity: Activity 
{
    MySampleBroadcastReceiver receiver;

    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        receiver = new MySampleBroadcastReceiver()

        // Code omitted for clarity
    }

    protected override OnResume() 
    {
        base.OnResume();
        RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
        // Code omitted for clarity
    }

    protected override OnPause() 
    {
        UnregisterReceiver(receiver);
        // Code omitted for clarity
        base.OnPause();
    }
}
```

이전 예제에서는 활동, 전경으로 제공 되는 경우에 등록 합니다를 사용 하 여 사용자 지정 의도 대기할 브로드캐스트 수신기는 `OnResume` 수명 주기 메서드. 활동의 배경으로 전환 하기 때문는 `OnPause()` 메서드 수신기를 등록 취소 됩니다.

## <a name="publishing-a-broadcast"></a>브로드캐스트를 게시합니다.

브로드캐스트 의도 개체를 만들고 사용 하 여 디스패치 장치에 설치 된 모든 앱에 게시 될 수는 `SendBroadcast` 또는 `SendOrderedBroadcast` 메서드.  

1. **Context.SendBroadcast 메서드** &ndash; 이 방법의 몇 가지 구현이 있습니다.
   이러한 메서드는 전체 시스템에 의도 브로드캐스트 됩니다. 브로드캐스트 수신기 thatwill 결정 되지 않은 순서로 의도 수신 합니다. 이 상당한 수준의 유연성을 제공 하지만 다른 응용 프로그램에 등록 한 의도 받을 수 있다는 것을 의미 합니다. 이 잠재적 보안 위험이 발생할 수 있습니다. 응용 프로그램 무단된 액세스를 방지 하기 위해 추가 보안을 구현 해야 합니다. 한 가지 가능한 해결 방법은 사용 하 여 `LocalBroadcastManager` 만 응용 프로그램의 개인 공간 내에서 메시지를 발송 하는 합니다. 이 코드 조각 중 하나를 사용 하 여 의도 디스패치 하는 방법의 한 가지 예는는 `SendBroadcast` 메서드:

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   intent.PutExtra("key", "value");
   SendBroadcast(intent);
   ```

    이 조각은 브로드캐스트를 사용 하 여 보내는의 또 다른 예는 `Intent.SetAction` 작업을 식별 하는 메서드:

    ```csharp 
    Intent intent = new Intent();
    intent.SetAction("com.xamarin.example.TEST");
    intent.PutExtra("key", "value");
    SendBroadcast(intent);
    ```

2. **Context.SendOrderedBroadcast** &ndash; 이 메서드는 매우 비슷하지만 `Context.SendBroadcast`, 의도 recievers 등록 된 순서로 수신자에 대 한 번에 게시 된 하나씩 된다는 차이입니다.

### <a name="localbroadcastmanager"></a>LocalBroadcastManager

[Xamarin 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) 이라는 도우미 클래스를 제공 [ `LocalBroadcastManager` ](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html)합니다. `LocalBroadcastManager` 앱에서 장치에 다른 앱의에서 브로드캐스트를 받거나 보낼를 원하지 않는 위한 기능입니다. `LocalBroadcastManager` 만 하 고 등록 된 이러한 브로드캐스트 수신기에만 응용 프로그램의 컨텍스트 내에서 메시지를 게시 합니다는 `LocalBroadcastManager`합니다. 이 코드 조각은와 브로드캐스트 수신기를 등록 한 예로 `LocalBroadcastManager`:

```csharp
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this). RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
```

장치에서 앱이 다른 앱으로 게시 된 메시지를 받을 수는 `LocalBroadcastManager`합니다. 이 코드 조각을 사용 하는 의도 디스패치 하는 방법을 보여 줍니다는 `LocalBroadcastManager`:

```csharp
Intent message = new Intent("com.xamarin.example.TEST");
// If desired, pass some values to the broadcast receiver.
intent.PutExtra("key", "value");
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
```

## <a name="related-links"></a>관련 링크

- [BroadcastReceiver](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Context.RegisterReceiver](https://developer.xamarin.com/api/member/Android.Content.Context.RegisterReceiver/p/Android.Content.BroadcastReceiver/Android.Content.IntentFilter/System.String/Android.OS.Handler/)
- [Context.SendBroadcast](https://developer.xamarin.com/api/member/Android.Content.Context.SendBroadcast/p/Android.Content.Intent/)
- [Context.UnregisterReceiver](https://developer.xamarin.com/api/member/Android.Content.Context.UnregisterReceiver/p/Android.Content.BroadcastReceiver/)
- [의도](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [IntentFilter](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
- [LocalBroadcastManager](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))
- [Android의 로컬 알림](~/android/app-fundamentals/notifications/local-notifications.md)
- [Android 라이브러리 v4 지원](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
