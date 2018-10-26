---
title: Xamarin.Android에 브로드캐스트 수신기
description: 이 섹션에서는 브로드캐스트 수신기를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/20/2018
ms.openlocfilehash: 51bd3dd4c27dce19344f7660c31a0d4e741e1ad4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121141"
---
# <a name="broadcast-receivers-in-xamarinandroid"></a>Xamarin.Android에 브로드캐스트 수신기

_이 섹션에서는 브로드캐스트 수신기를 사용 하는 방법을 설명 합니다._

## <a name="broadcast-receiver-overview"></a>브로드캐스트 수신기 개요

A _브로드캐스트 수신기_ 는 응용 프로그램 메시지에 응답할 수 있도록 Android 구성 요소 (Android [ `Intent` ](https://developer.xamarin.com/api/type/Android.Content.Intent/)) Android 운영 체제 또는 응용 프로그램에 브로드캐스트 되는 합니다. 브로드캐스트를 수행을 _게시-구독_ 모델 &ndash; 이벤트를 게시 하 고 이벤트에 관심이 있는 이러한 구성 요소에서 받은 브로드캐스트를 사용 하면 됩니다.

Android는 두 가지 유형의 브로드캐스트를 식별합니다.

* **명시적 브로드캐스트** &ndash; 브로드캐스트 이러한 유형의 특정 응용 프로그램을 대상으로 합니다. 명시적 브로드캐스트는 가장 일반적인 사용 활동을 시작 하는 것입니다. 앱에서 전화 걸기; 해야 할 경우에 명시적 브로드캐스트의 예 Android 및 전화 번호를 따라 전달 전화 걸 전화 앱을 대상으로 하는 의도 발송 하 합니다. Android는 휴대폰 앱에 의도 라우팅할 됩니다.
* **암시적 브로드캐스트** &ndash; 이러한 브로드캐스트 장치의 모든 앱에 디스패치 됩니다. 암시적 브로드캐스트의 예로 `ACTION_POWER_CONNECTED` 의도 합니다. 이 Android 장치에서 배터리를 충전 감지 될 때마다 게시 됩니다. Android는이 이벤트에 대 한 등록 된 모든 앱에이 의도를 라우팅합니다.

브로드캐스트 수신기의 서브 클래스는를 `BroadcastReceiver` 형식 및이 재정의 해야 합니다 [ `OnReceive` ](https://developer.xamarin.com/api/member/Android.Content.BroadcastReceiver.OnReceive/p/Android.Content.Context/Android.Content.Intent/) 메서드. Android 실행될지 `OnReceive` 주 스레드에서 하므로이 메서드는 하도록 설계 되어야 신속 하 게 실행 합니다. 스레드를 생성 하는 경우 주의 해야 `OnReceive` 하므로 메서드가 완료 되 면 Android 프로세스를 종료할 수 있습니다. 브로드캐스트 수신기는 장기 실행 작업을 수행 해야 하는 경우 다음 예약 하는 것이 좋습니다.는 _작업_ 사용 하는 `JobScheduler` 또는 _Firebase 작업 디스패처_합니다. 작업과 작업 일정을 별도 가이드에서 설명 합니다.

_의도 필터_ Android 메시지를 올바르게 라우팅할 수 있도록 브로드캐스트 수신기를 등록 하는 데 사용 됩니다. 런타임 시 의도 필터를 지정할 수 있습니다 (이 경우에 따라 라고는 _받는 사람 컨텍스트 등록_ 되거나 _동적 등록_) Android 매니페스트 (한 에정적으로정의할수있습니다_수신기 매니페스트 등록_). Xamarin.Android 제공는 C# 특성인 `IntentFilterAttribute`를 정적으로 등록 하는 의도 필터 (이 다룰이 가이드의 뒷부분에서 자세히). Android 8.0부터 수 없는 응용 프로그램에 암시적 브로드캐스트에 대 한 정적으로 등록 합니다.

컨텍스트 등록 된 수신기만 응답할 브로드캐스트 응용 프로그램 매니페스트 등록 된 수신기에 응답할 수 있지만 실행 되는 동안 매니페스트 등록 받는 사람 및 받는 사람 컨텍스트 등록 간의 주요 차이점은 앱 실행 되지 않을 경우에 브로드캐스트합니다.  

브로드캐스트 수신기를 관리 하 고 브로드캐스트를 보내기 위한 두 가지 Api 집합을 가지 있습니다.

1. **`Context`** &ndash; `Android.Content.Context` 시스템 전체 이벤트에 응답 하는 브로드캐스트 수신기를 등록 하는 클래스를 사용할 수 있습니다. `Context` 시스템 차원의 브로드캐스트 게시할 에서도 사용 됩니다.
2. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** &ndash; 이 API를 통해 제공 되는 [Xamarin 지원 라이브러리 v4 NuGet 패키지](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)합니다. 이 클래스는 브로드캐스트 하 고 브로드캐스트 수신기를 사용 하는 응용 프로그램의 컨텍스트에서 격리 된 상태로 유지 됩니다. 이 클래스는 응용 프로그램 전용 브로드캐스트에 응답 하거나 개인 수신자에 게 메시지를 전송에서 다른 응용 프로그램을 방지 하는 데 유용할 수 있습니다.

브로드캐스트 수신기 대화 상자를 표시할 수 없습니다 며 브로드캐스트 수신기에서 활동을 시작 하는 것이 좋습니다. 브로드캐스트 수신기에는 사용자에 게 해야, 하는 경우 알림의 게시 해야 합니다.

바인딩할 또는 브로드캐스트 수신기 내에서 서비스를 시작 하는 것이 불가능 합니다. 

이 가이드에서는 브로드캐스트 수신기를 만드는 방법과 브로드캐스트를 수신할 수 있도록 등록 하는 방법을 설명 합니다.

## <a name="creating-a-broadcast-receiver"></a>브로드캐스트 수신기 만들기

Xamarin.Android에서 브로드캐스트 수신기를 만들려는 응용 프로그램의 서브 클래스 여야 합니다 `BroadcastReceiver` 클래스를 사용 하 여 표시할 합니다 `BroadcastReceiverAttribute`, 재정의 및를 `OnReceive` 메서드:

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

Xamarin.Android 클래스 컴파일되면 수신기를 등록 하는 데 필요한 메타 데이터를 사용 하 여는 AndroidManifest도 업데이트 됩니다. 정적으로 등록 브로드캐스트 수신기에 대 한 합니다 `Enabled` 올바르게 설정 해야 합니다 `true`, 그렇지 않으면 Android 수신기의 인스턴스를 만들 수 수 됩니다.
 
`Exported` 속성 브로드캐스트 수신기 응용 프로그램 외부에서 메시지를 받을 수 있는지 여부를 제어 합니다. 속성을 명시적으로 설정 하지 않으면 속성의 기본값은 Android 브로드캐스트 수신기와 연결 된 모든 의도 필터가 있을 경우에 따라 의해 결정 됩니다. 브로드캐스트 수신기에 대 한 의도 필터 중 적어도 하나의 경우 Android를 가정 합니다 `Exported` 속성은 `true`합니다. 브로드캐스트 수신기와 연결 된 의도 필터가 없는 경우 Android 값 임을 가정 `false`합니다. 

`OnReceive` 메서드 수신에 대 한 참조를 `Intent` 는 브로드캐스트 수신자에 게 발송 되었습니다. 이렇게이 하면 보낸 사람의 의도 브로드캐스트 수신기에 값을 전달할 가능성이 있습니다.

### <a name="statically-registering-a-broadcast-receiver-with-an-intent-filter"></a>정적으로 의도 필터를 사용 하 여 브로드캐스트 수신기를 등록

경우는 `BroadcastReceiver` 으로 데코 레이트 된를 [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/), Xamarin.Android 필요한 추가 `<intent-filter>` Android 요소 컴파일 타임에 매니페스트 합니다. 다음 코드 조각은 다음과 같습니다. (사용자가 Android 권한이 부여 된) 경우 부팅 장치를 마쳤을 때 실행 되는 브로드캐스트 수신기의 예

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

사용자 지정 의도 응답할 의도 필터를 만들 수 이기도 합니다. 다음 예제를 참조하세요. 

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

Android 8.0 (API 레벨 26)를 대상으로 하는 앱 이상 등록할 수 없습니다 정적 암시적는 브로드캐스트 또는 합니다. 앱을 명시적 브로드캐스트에 대 한 정적으로 여전히 등록이 가능 합니다. 이 제한에서 제외 되는 암시적 브로드캐스트의 작은 목록이 있습니다. 이러한 예외에 설명 되어는 [암시적 브로드캐스트 예외](https://developer.android.com/guide/components/broadcast-exceptions.html) Android 설명서 가이드입니다. 암시적 브로드캐스트에 관심이 있는 앱을 사용 하 여 동적으로 수행 해야 합니다는 `RegisterReceiver` 메서드. 이 작업은 다음 설명 되어 있습니다.

### <a name="context-registering-a-broadcast-receiver"></a>브로드캐스트 수신기를 컨텍스트 등록

컨텍스트 (동적 등록 라고도 함)의 등록 된 수신기를 호출 하 여 수행 됩니다 합니다 `RegisterReceiver` 메서드와 브로드캐스트 수신기 없어야 호출 하 여 등록을 `UnregisterReceiver` 메서드. 가 리소스 누수를 방지 하려면 컨텍스트 (작업 또는 서비스)에 대 한 더 이상 관련이 있는 경우 수신기를 등록 취소 해야 합니다. 예를 들어, 서비스 업데이트가 사용자에 게 표시할 수 있는 작업을 알리기 위해 의도 브로드캐스트 수 있습니다. 작업이 시작 되 면 이러한 의도 대 한 등록 합니다. 활동을 배경으로 이동 하 고 사용자에 게는 더 이상 볼 등록 취소 해야 수신기가 업데이트를 표시 하기 위한 UI를 볼 수 없게 되었기 때문에 경우. 다음 코드 조각은 다음과 같습니다. 등록 및 활동의 컨텍스트에서 브로드캐스트 수신기를 등록 취소 하는 방법의 예

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

이전 예제에서는 활동을 전경에 들어오면 등록할를 사용 하 여 사용자 지정 의도 대 한 수신 대기 하는 브로드캐스트 수신기는 `OnResume` 수명 주기 메서드. 작업을 백그라운드로 이동함에 따라는 `OnPause()` 메서드 수신기를 등록 취소 됩니다.

## <a name="publishing-a-broadcast"></a>브로드캐스트를 게시합니다.

브로드캐스트 의도 개체를 만들고 사용 하 여 디스패치 하는 장치에 설치 하는 모든 앱에 게시 될 수 있습니다 합니다 `SendBroadcast` 또는 `SendOrderedBroadcast` 메서드.  

1. **Context.SendBroadcast 메서드** &ndash; 이 방법의 몇 가지 구현이 있습니다.
   이러한 메서드는 전체 시스템에 의도 브로드캐스트 됩니다. 의도 결정 되지 않은 순서로 받을 브로드캐스트 수신기입니다. 이 상당한 유연성을 제공 하지만 다른 응용 프로그램에 등록 하 고 의도 받을 수 있다는 것을 의미 합니다. 이 잠재적인 보안 위험이 발생할 수 있습니다. 응용 프로그램에 대 한 무단된 액세스를 방지 하기 위해 추가 보안을 구현 해야 합니다. 한 가지 솔루션은 사용 하는 `LocalBroadcastManager` 앱의 개인 공간 내에서 메시지 발송만. 이 코드 조각 중 하나를 사용 하 여 의도 디스패치 하는 방법의 예로는 `SendBroadcast` 메서드:

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   message.PutExtra("key", "value");
   SendBroadcast(message);
   ```

    이 코드 조각은 브로드캐스트를 사용 하 여 전송 하는 또 다른 예는 `Intent.SetAction` 동작을 식별 하는 방법.

    ```csharp
    Intent intent = new Intent();
    intent.SetAction("com.xamarin.example.TEST");
    intent.PutExtra("key", "value");
    SendBroadcast(intent);
    ```

2. **Context.SendOrderedBroadcast** &ndash; 이 메서드는 매우 비슷합니다 `Context.SendBroadcast`, 의도 된 수신자 등록 된 순서 대로 수신자에 게 시간에 게시 한 된다는 것의 차이입니다.

### <a name="localbroadcastmanager"></a>LocalBroadcastManager

합니다 [Xamarin 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) 라는 도우미 클래스를 제공 [ `LocalBroadcastManager` ](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html)합니다. `LocalBroadcastManager` 장치의 다른 앱에서 브로드캐스트를 주고받을 원하지 않는 앱을 위한 것입니다. 합니다 `LocalBroadcastManager` 에 등록 된 해당 브로드캐스트 수신기를 응용 프로그램의 컨텍스트 내에서 메시지를 게시만 됩니다는 `LocalBroadcastManager`합니다. 이 코드 조각을 사용 하 여 브로드캐스트 수신기를 등록 하는 예로 `LocalBroadcastManager`:

```csharp
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this). RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
```

장치의 다른 앱으로 게시 된 메시지를 받을 수 없습니다는 `LocalBroadcastManager`합니다. 이 코드 조각을 사용 하는 의도 디스패치 하는 방법을 보여 줍니다는 `LocalBroadcastManager`:

```csharp
Intent message = new Intent("com.xamarin.example.TEST");
// If desired, pass some values to the broadcast receiver.
message.PutExtra("key", "value");
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
```

## <a name="related-links"></a>관련 링크

- [BroadcastReceiver API](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Context.RegisterReceiver API](https://developer.xamarin.com/api/member/Android.Content.Context.RegisterReceiver/p/Android.Content.BroadcastReceiver/Android.Content.IntentFilter/System.String/Android.OS.Handler/)
- [Context.SendBroadcast API](https://developer.xamarin.com/api/member/Android.Content.Context.SendBroadcast/p/Android.Content.Intent/)
- [Context.UnregisterReceiver API](https://developer.xamarin.com/api/member/Android.Content.Context.UnregisterReceiver/p/Android.Content.BroadcastReceiver/)
- [의도 API](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [IntentFilter API](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
- [LocalBroadcastManager (Android 문서)](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))
- [Android에서 로컬 알림](~/android/app-fundamentals/notifications/local-notifications.md) 가이드
- [Android 지원 v4 라이브러리 (NuGet)](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
