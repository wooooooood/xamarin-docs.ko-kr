---
title: Xamarin Android의 브로드캐스트 수신기
description: 이 섹션에서는 브로드캐스트 수신기를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/20/2018
ms.openlocfilehash: 9f490bec121481d9f3f661913d8aefcef999bb4a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016964"
---
# <a name="broadcast-receivers-in-xamarinandroid"></a>Xamarin Android의 브로드캐스트 수신기

_이 섹션에서는 브로드캐스트 수신기를 사용 하는 방법을 설명 합니다._

## <a name="broadcast-receiver-overview"></a>브로드캐스트 수신기 개요

_브로드캐스트 수신기_ 는 응용 프로그램이 android 운영 체제 또는 응용 프로그램에서 브로드캐스팅하는 메시지 (android [`Intent`](xref:Android.Content.Intent))에 응답할 수 있도록 하는 android 구성 요소입니다. 브로드캐스트는 _게시-구독_ 모델을 따르고 이벤트 &ndash; 이벤트에 관심이 있는 구성 요소에 의해 브로드캐스트가 게시 및 수신 됩니다.

Android는 다음과 같은 두 가지 유형의 브로드캐스트를 식별 합니다.

- **명시적 브로드캐스트** &ndash; 이러한 유형의 브로드캐스트는 특정 응용 프로그램을 대상으로 합니다. 명시적 브로드캐스트의 가장 일반적인 용도는 활동을 시작 하는 것입니다. 앱이 전화 번호로 전화를 걸 때의 명시적 브로드캐스트 예 Android에서 휴대폰 앱을 대상으로 하 고 전화 번호를 따라 전달할 의도를 디스패치합니다. 그런 다음 Android가 의도를 휴대폰 앱으로 라우팅합니다.
- 이러한 브로드캐스트 &ndash; **암시적 브로드캐스트** 는 장치의 모든 앱으로 발송 됩니다. 암시적 브로드캐스트의 예는 `ACTION_POWER_CONNECTED` 의도입니다. 이 의도는 장치에서 배터리가 충전 되 고 있음을 Android에서 검색할 때마다 게시 됩니다. Android는이 이벤트에 대해 등록 된 모든 앱에이 의도를 라우팅합니다.

브로드캐스트 수신기는 `BroadcastReceiver` 형식의 서브 클래스 이며 [`OnReceive`](xref:Android.Content.BroadcastReceiver.OnReceive*) 메서드를 재정의 해야 합니다. Android는 주 스레드에서 `OnReceive`를 실행 하므로이 메서드는 빠르게 실행 되도록 디자인 해야 합니다. `OnReceive`에서 스레드를 생성할 때는 메서드가 완료 될 때 Android에서 프로세스를 종료할 수 있으므로 주의를 기울여야 합니다. 브로드캐스트 수신자가 장기 실행 작업을 수행 해야 하는 경우 `JobScheduler` 또는 _Firebase 작업 디스패처_를 사용 하 여 _작업_ 을 예약 하는 것이 좋습니다. 작업을 예약 하는 작업은 별도의 가이드에서 설명 합니다.

_의도 필터_ 는 Android에서 메시지를 적절 하 게 라우팅할 수 있도록 브로드캐스트 수신기를 등록 하는 데 사용 됩니다. 의도 필터는 런타임에 지정 하거나 ( _컨텍스트 등록 수신기_ 또는 _동적 등록_으로 라고도 함) Android 매니페스트 ( _매니페스트 등록 수신자_)에서 정적으로 정의할 수 있습니다. Xamarin.ios는 의도 필터를 C# 정적으로 등록 하는`IntentFilterAttribute`특성을 제공 합니다 .이에 대해서는이 가이드의 뒷부분에서 자세히 설명 합니다. Android 8.0부터 응용 프로그램에서 암시적 브로드캐스트를 정적으로 등록 하는 것은 불가능 합니다.

매니페스트가 등록 된 수신기와 컨텍스트 등록 받는 사람 간의 주된 차이점은 컨텍스트가 등록 된 수신기는 응용 프로그램이 실행 되는 동안에만 브로드캐스트에 응답 하 고, 매니페스트는 등록 된 수신기는에 응답할 수 있다는 것입니다. 앱이 실행 되 고 있지 않은 경우에도 브로드캐스트합니다.  

브로드캐스트 수신기를 관리 하 고 브로드캐스트를 전송 하기 위한 두 가지 Api 집합이 있습니다.

1. &ndash; **`Context`** `Android.Content.Context` 클래스를 사용 하 여 시스템 차원의 이벤트에 응답 하는 브로드캐스트 수신기를 등록할 수 있습니다. `Context`는 시스템 차원 브로드캐스트를 게시 하는 데도 사용 됩니다.
2. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** &ndash; [Xamarin Support Library v4 NuGet 패키지](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)를 통해 사용할 수 있는 API입니다. 이 클래스는 브로드캐스트 및 브로드캐스트 수신기를 사용 하는 응용 프로그램의 컨텍스트에서 격리 된 상태를 유지 하는 데 사용 됩니다. 이 클래스는 다른 응용 프로그램이 응용 프로그램 전용 브로드캐스트에 응답 하거나 메시지를 개인 수신자에 게 보내지 못하도록 방지 하는 데 유용할 수 있습니다.

브로드캐스트 수신기는 대화를 표시 하지 않을 수 있으며 브로드캐스트 수신기 내에서 작업을 시작 하지 않는 것이 좋습니다. 브로드캐스트 수신자가 사용자에 게 알려야 할 경우 알림을 게시 해야 합니다.

브로드캐스트 수신기 내에서 서비스에 바인딩하거나 시작할 수는 없습니다. 

이 가이드에서는 브로드캐스트 수신기를 만드는 방법 및 브로드캐스트를 받을 수 있도록 등록 하는 방법에 대해 설명 합니다.

## <a name="creating-a-broadcast-receiver"></a>브로드캐스트 수신기 만들기

Xamarin.ios에서 브로드캐스트 수신기를 만들려면 응용 프로그램이 `BroadcastReceiver` 클래스의 서브 클래스를 장식할 하 고 `BroadcastReceiverAttribute`와 함께 사용 하 고 `OnReceive` 메서드를 재정의 해야 합니다.

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

Xamarin Android가 클래스를 컴파일하는 경우에는 필요한 메타 데이터로 AndroidManifest도 업데이트 하 여 수신기를 등록 합니다. 정적으로 등록 된 브로드캐스트 수신기의 경우 `Enabled`를 `true`으로 올바르게 설정 해야 합니다. 그렇지 않으면 Android에서 수신기의 인스턴스를 만들 수 없습니다.

`Exported` 속성은 브로드캐스트 수신기가 응용 프로그램 외부에서 메시지를 수신할 수 있는지 여부를 제어 합니다. 속성이 명시적으로 설정 되지 않은 경우 속성의 기본값은 브로드캐스트 수신기에 연결 된 의도 필터가 있는지 여부를 기반으로 Android에 의해 결정 됩니다. 브로드캐스트 수신기에 대해 하나 이상의 의도 필터가 있는 경우 Android는 `Exported` 속성이 `true`되는 것으로 가정 합니다. 브로드캐스트 수신기와 연결 된 의도 필터가 없는 경우 Android에서는 값이 `false`되는 것으로 가정 합니다. 

`OnReceive` 메서드는 브로드캐스트 수신기에 디스패치 된 `Intent`에 대 한 참조를 받습니다. 이렇게 하면 발신자가 브로드캐스트 수신기에 값을 전달할 수 있습니다.

### <a name="statically-registering-a-broadcast-receiver-with-an-intent-filter"></a>의도 필터를 사용 하 여 브로드캐스트 수신기를 정적으로 등록

`BroadcastReceiver` [`IntentFilterAttribute`](xref:Android.App.IntentFilterAttribute)으로 데코레이팅 되 면 xamarin.ios는 컴파일 타임에 필요한 `<intent-filter>` 요소를 android 매니페스트에 추가 합니다. 다음 코드 조각은 장치가 부팅을 마쳤을 때 (해당 사용자가 적절 한 Android 사용 권한을 부여 받은 경우) 실행 되는 브로드캐스트 수신기의 예입니다.

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

또한 사용자 지정 의도에 응답 하는 의도 필터를 만들 수 있습니다. 다음 예제를 참조하세요. 

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

Android 8.0 (API 레벨 26) 이상을 대상으로 하는 앱은 암시적 브로드캐스트에 대해 정적으로 등록 되지 않을 수 있습니다. 앱은 명시적 브로드캐스트에 대 한 정적 등록을 계속 할 수 있습니다. 이 제한에서 제외 되는 암시적 브로드캐스트에는 작은 목록이 있습니다. 이러한 예외는 Android 설명서의 [암시적 브로드캐스트 예외](https://developer.android.com/guide/components/broadcast-exceptions.html) 가이드에 설명 되어 있습니다. 암시적 브로드캐스트에 관심이 있는 앱은 `RegisterReceiver` 메서드를 사용 하 여 동적으로이 작업을 수행 해야 합니다. 이에 대해서는 다음에 설명 되어 있습니다.

### <a name="context-registering-a-broadcast-receiver"></a>컨텍스트-브로드캐스트 수신기 등록

수신기의 컨텍스트 등록 (동적 등록이 라고도 함)은 `RegisterReceiver` 메서드를 호출 하 여 수행 되며, `UnregisterReceiver` 메서드를 호출 하 여 브로드캐스트 수신자를 등록 취소 해야 합니다. 리소스 누수를 방지 하려면 더 이상 컨텍스트 (활동 또는 서비스)와 관련이 없는 수신자를 등록 취소 하는 것이 중요 합니다. 예를 들어 서비스는 업데이트를 사용자에 게 표시할 수 있음을 활동에 알리기 위한 의도를 브로드캐스트할 수 있습니다. 활동이 시작 되 면 해당 의도에 대해 등록 됩니다. 활동이 배경으로 이동 되 고 더 이상 사용자에 게 표시 되지 않는 경우 업데이트를 표시 하는 UI가 더 이상 표시 되지 않으므로 수신자를 등록 취소 해야 합니다. 다음 코드 조각은 활동의 컨텍스트에서 브로드캐스트 수신기를 등록 및 등록 취소 하는 방법의 예입니다.

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

이전 예제에서 작업이 포그라운드로 들어오면 `OnResume` 수명 주기 메서드를 사용 하 여 사용자 지정 의도를 수신 하는 브로드캐스트 수신기를 등록 합니다. 활동이 백그라운드로 이동 하면 `OnPause()` 메서드가 수신자의 등록을 취소 합니다.

## <a name="publishing-a-broadcast"></a>브로드캐스트 게시

브로드캐스트는 의도 개체를 만드는 장치에 설치 된 모든 앱에 게시 될 수 있으며 `SendBroadcast` 또는 `SendOrderedBroadcast` 메서드를 사용 하 여 디스패치할 수 있습니다.  

1. &ndash; **SendBroadcast 메서드** 는이 메서드의 여러 구현이 있습니다.
   이러한 메서드는 전체 시스템에 의도를 브로드캐스트합니다. 결정 되지 않은 순서로 의도를 받는 브로드캐스트 수신기입니다. 이는 상당한 유연성을 제공 하지만 다른 응용 프로그램이 해당 의도를 등록 하 고 받을 수 있음을 의미 합니다. 이로 인해 잠재적인 보안 위험이 발생할 수 있습니다. 응용 프로그램에서 추가 보안을 구현 하 여 무단 액세스를 방지 해야 할 수도 있습니다. 응용 프로그램의 개인 공간 내 에서만 메시지를 디스패치 하는 `LocalBroadcastManager`를 사용 하는 것이 가능 합니다. 이 코드 조각은 `SendBroadcast` 메서드 중 하나를 사용 하 여 의도를 디스패치 하는 방법의 한 가지 예입니다.

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   message.PutExtra("key", "value");
   SendBroadcast(message);
   ```

    이 코드 조각은 `Intent.SetAction` 메서드를 사용 하 여 작업을 식별 하 여 브로드캐스트를 보내는 또 다른 예입니다.

    ```csharp
    Intent intent = new Intent();
    intent.SetAction("com.xamarin.example.TEST");
    intent.PutExtra("key", "value");
    SendBroadcast(intent);
    ```

2. **SendOrderedBroadcast** &ndash;이 방법은 `Context.SendBroadcast`와 매우 유사 합니다 .이는 수신자가 등록 된 순서 대로 수신자에 게 의도를 게시할 때 차이가 있다는 것입니다.

### <a name="localbroadcastmanager"></a>LocalBroadcastManager

[Xamarin Support Library v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) 는 [`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html)라는 도우미 클래스를 제공 합니다. `LocalBroadcastManager`는 장치의 다른 앱에서 브로드캐스트를 보내거나 받지 않으려는 앱을 위한 것입니다. `LocalBroadcastManager`는 응용 프로그램의 컨텍스트 내 에서만 메시지를 게시 하 고 `LocalBroadcastManager`에 등록 된 브로드캐스트 수신기에만 게시 합니다. 이 코드 조각은 `LocalBroadcastManager`로 브로드캐스트 수신기를 등록 하는 예제입니다.

```csharp
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this). RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
```

장치에 있는 다른 앱은 `LocalBroadcastManager`게시 된 메시지를 받을 수 없습니다. 이 코드 조각에서는 `LocalBroadcastManager`를 사용 하 여 의도를 디스패치 하는 방법을 보여 줍니다.

```csharp
Intent message = new Intent("com.xamarin.example.TEST");
// If desired, pass some values to the broadcast receiver.
message.PutExtra("key", "value");
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
```

## <a name="related-links"></a>관련 링크

- [BroadcastReceiver API](xref:Android.Content.BroadcastReceiver)
- [컨텍스트별 수신기 API](xref:Android.Content.Context.RegisterReceiver*)
- [컨텍스트. SendBroadcast API](xref:Android.Content.Context.SendBroadcast*)
- [컨텍스트. UnregisterReceiver API](xref:Android.Content.Context.UnregisterReceiver*)
- [의도 API](xref:Android.Content.Intent)
- [IntentFilter API](xref:Android.App.IntentFilterAttribute)
- [LocalBroadcastManager (Android 문서)](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))
- [Android 가이드의 로컬 알림](~/android/app-fundamentals/notifications/local-notifications.md)
- [Android 지원 라이브러리 v4 (NuGet)](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
