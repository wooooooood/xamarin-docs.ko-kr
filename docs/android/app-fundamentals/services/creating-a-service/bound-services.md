---
title: Xamarin Android의 바인딩된 서비스
description: '바인딩된 서비스는 클라이언트 (예: Android 작업)가 상호 작용할 수 있는 클라이언트-서버 인터페이스를 제공 하는 Android 서비스입니다. 이 가이드에서는 바인딩된 서비스 만들기와 관련 된 주요 구성 요소 및 Xamarin Android 응용 프로그램에서이를 사용 하는 방법을 설명 합니다.'
ms.prod: xamarin
ms.assetid: 809ECE88-EF08-4E9A-B389-A2DC08C51A6E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/04/2018
ms.openlocfilehash: 584f523446584192cfa882697c0f76865ce78a10
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "70754920"
---
# <a name="bound-services-in-xamarinandroid"></a>Xamarin Android의 바인딩된 서비스

_바인딩된 서비스는 클라이언트 (예: Android 작업)가 상호 작용할 수 있는 클라이언트-서버 인터페이스를 제공 하는 Android 서비스입니다. 이 가이드에서는 바인딩된 서비스 만들기와 관련 된 주요 구성 요소 및 Xamarin Android 응용 프로그램에서이를 사용 하는 방법을 설명 합니다._

## <a name="bound-services-overview"></a>바인딩된 서비스 개요

클라이언트에서 서비스와 직접 상호 작용 하는 클라이언트 서버 인터페이스를 제공 하는 서비스를 _바인딩된 서비스_라고 합니다.  서비스의 단일 인스턴스에 여러 클라이언트를 동시에 연결할 수 있습니다. 바인딩된 서비스와 클라이언트는 서로 격리 됩니다. 대신, Android는 둘 간의 연결 상태를 관리 하는 일련의 중간 개체를 제공 합니다. 이 상태는 [`Android.Content.IServiceConnection`](xref:Android.Content.IServiceConnection) 인터페이스를 구현 하는 개체에 의해 유지 관리 됩니다.  이 개체는 클라이언트에서 만들고 [`BindService`](xref:Android.Content.Context.BindService*) 메서드에 매개 변수로 전달 합니다. @No__t_0는 모든 [`Android.Content.Context`](xref:Android.Content.Context) 개체 (예: 활동)에서 사용할 수 있습니다. Android 운영 체제에서 서비스를 시작 하 고이 서비스에 클라이언트를 바인딩하기 위한 요청입니다. 클라이언트는 다음과 같은 세 가지 방법으로 `BindService` 메서드를 사용 하 여 서비스에 바인딩할 수 있습니다.

- 서비스 바인더 &ndash; **서비스 바인더** 는 [`Android.OS.IBinder`](xref:Android.OS.IBinder) 인터페이스를 구현 하는 클래스입니다. 대부분의 응용 프로그램은이 인터페이스를 직접 구현 하지 않고 [`Android.OS.Binder`](xref:Android.OS.Binder) 클래스를 확장 합니다. 이는 가장 일반적인 방법 이며 서비스와 클라이언트가 동일한 프로세스에 있는 경우에 적합 합니다.
- 이 방법은 **Messenger &ndash; 사용 하 여** 서비스가 별도의 프로세스에 있을 수 있는 경우에 적합 합니다. 대신, 서비스 요청은 [`Android.OS.Messenger`](xref:Android.OS.Messenger)를 통해 클라이언트와 서비스 간에 마샬링됩니다. [@No__t_1](xref:Android.OS.Handler) `Messenger` 요청을 처리할 서비스에 생성 됩니다. 이 내용은 다른 가이드에서 설명 합니다.
- **Aidl (Android Interface Definition Language)** &ndash; [aidl](https://developer.android.com/guide/components/aidl) 은이 가이드에서 다루지 않는 고급 기술입니다.

클라이언트를 서비스에 바인딩한 후에는 `Android.OS.IBinder` 개체를 통해 둘 사이의 통신이 발생 합니다.  이 개체는 클라이언트가 서비스와 상호 작용할 수 있게 해 주는 인터페이스를 담당 합니다. 각 Xamarin.ios 응용 프로그램에서이 인터페이스를 처음부터 구현 하는 것은 필요 하지 않습니다. Android SDK에서는 클라이언트와 서비스 간에 개체를 마샬링하는 데 필요한 대부분의 코드를 처리 하는 [`Android.OS.Binder`](xref:Android.OS.Binder) 클래스를 제공 합니다.

클라이언트는 서비스를 사용 하 여 작업을 수행 하는 경우 `UnbindService` 메서드를 호출 하 여 바인딩 해제 해야 합니다. 마지막 클라이언트가 서비스에서 바인딩 해제 되 면 Android가 바인딩된 서비스를 중지 하 고 삭제 합니다.

이 다이어그램에서는 활동, 서비스 연결, 바인더 및 서비스가 모두 서로 관련 된 방식을 보여 줍니다.

![서비스 구성 요소가 서로 관련 되는 방법을 보여 주는 다이어그램](bound-services-images/bound-services-02.png "서비스 구성 요소가 서로 관련 되는 방법을 보여 주는 다이어그램")

이 가이드에서는 `Service` 클래스를 확장 하 여 바인딩된 서비스를 구현 하는 방법을 설명 합니다. 또한 클라이언트에서 서비스와 통신할 수 있도록 `IServiceConnection`를 구현 하 고 `Binder`을 확장 하는 방법을 다룹니다. 샘플 앱은 **[BoundServiceDemo](https://github.com/xamarin/monodroid-samples/tree/master/ApplicationFundamentals/ServiceSamples/BoundServiceDemo)** 이라는 단일 xamarin.ios 프로젝트를 포함 하는 솔루션을 포함 하는이 가이드를 함께 제공 합니다. 이 응용 프로그램은 서비스를 구현 하는 방법과 작업을이 서비스에 바인딩하는 방법을 보여 주는 매우 기본적인 응용 프로그램입니다. 바인딩된 서비스에는 서비스가 시작 될 때 사용자에 게 지시 하는 문자열을 반환 하는 `GetFormattedTimestamp`와 서비스가 실행 된 기간을 나타내는 매우 간단한 API가 있습니다. 사용자는 앱을 사용 하 여 수동으로 바인딩 해제 하 고 서비스에 바인딩할 수도 있습니다.

[Android 휴대폰에서 실행 되는 응용 프로그램의 ![Screenshot](bound-services-images/bound-services-03-sml.png)](bound-services-images/bound-services-03.png#lightbox)

## <a name="implementing-and-consuming-a-bound-service"></a>바인딩된 서비스 구현 및 사용

Android 응용 프로그램에서 바인딩된 서비스를 사용 하기 위해 구현 해야 하는 세 가지 구성 요소가 있습니다.

1. **@No__t_1 클래스를 확장 하 고 &ndash; 수명 주기 콜백 메서드를 구현** 합니다 .이 클래스에는 서비스에서 요청 되는 작업을 수행 하는 코드가 포함 됩니다. 이 내용은 아래에서 자세히 설명 합니다.
2. @No__t_2 **`IServiceConnection`를 구현 하는 클래스를 만듭니다** .이 인터페이스는 서비스에 대 한 연결이 변경 될 때 (즉, 클라이언트가 서비스에 연결 되거나 연결이 끊어진 경우) Android에서 호출 하는 콜백 메서드를 제공 하 여 클라이언트에 게 알립니다. 서비스 연결에서는 클라이언트에서 서비스와 직접 상호 작용 하는 데 사용할 수 있는 개체에 대 한 참조도 제공 합니다. 이 참조를 _바인더_라고 합니다.
3. 클라이언트에서 서비스와 통신 하는 데 사용 하는 API를 제공 하는 _바인더_ 구현 &ndash; **`IBinder`을 구현 하는 클래스를 만듭니다** . 바인더는 바인딩된 서비스에 대 한 참조를 제공 하 여 메서드를 직접 호출할 수 있도록 하거나, 바인더가 응용 프로그램에서 바인딩된 서비스를 캡슐화 및 숨기는 클라이언트 API를 제공할 수 있습니다. @No__t_0는 원격 프로시저 호출에 필요한 코드를 제공 해야 합니다. @No__t_0 인터페이스를 직접 구현 하는 것은 필요 하지 않습니다. 대신 응용 프로그램은 `IBinder`에 필요한 대부분의 기본 기능을 제공 하는 `Binder` 유형을 확장 해야 합니다.
4. 서비스 연결, 바인더 및 서비스가 생성 되 면 서비스를 **시작 하 고 &ndash; 하** 는 것은 Android 응용 프로그램에서 서비스를 시작 하 고 해당 서비스에 바인딩하는 역할을 담당 합니다.

이러한 각 단계는 다음 섹션에서 자세히 설명 합니다.

### <a name="extend-the-service-class"></a>@No__t_0 클래스 확장

Xamarin.ios를 사용 하 여 서비스를 만들려면 `Service`를 서브클래싱하 고 [`ServiceAttribute`](xref:Android.App.ServiceAttribute)를 사용 하 여 클래스를 장식할 해야 합니다. 특성은 Xamarin.ios 빌드 도구에서 응용 프로그램의 **Androidmanifest** 파일에 서비스를 적절 하 게 등록 하는 데 사용 됩니다. 작업, 바인딩된 서비스에는의 중요 한 이벤트와 연결 된 자체 수명 주기 및 콜백 메서드가 있습니다. 수명 주기입니다. 다음 목록은 서비스에서 구현 하는 몇 가지 일반적인 콜백 메서드의 예입니다.

- `OnCreate` &ndash;이 메서드는 서비스를 인스턴스화하는 동안 Android에서 호출 됩니다. 수명이 지속 되는 동안 서비스에 필요한 변수나 개체를 초기화 하는 데 사용 됩니다. 이 메서드는 선택 사항입니다.
- `OnBind` &ndash;이 메서드는 모든 바인딩된 서비스에 의해 구현 되어야 합니다. 첫 번째 클라이언트가 서비스에 연결 하려고 할 때 호출 됩니다. 클라이언트가 서비스와 상호 작용할 수 있도록 `IBinder`의 인스턴스를 반환 합니다. 서비스가 실행 되는 동안에는 `IBinder` 개체를 사용 하 여 서비스에 바인딩하기 위해 이후의 모든 클라이언트 요청을 수행 합니다.
- `OnUnbind` &ndash;이 메서드는 바인딩된 모든 클라이언트의 바인딩이 해제 될 때 호출 됩니다. 이 메서드에서 `true` 반환 함으로써 서비스는 나중에 새 클라이언트가 바인딩될 때 `OnUnbind`에 전달 된 의도를 가진 `OnRebind`를 호출 합니다. 바인딩 해제 된 후 서비스를 계속 실행 하면이 작업을 수행할 수 있습니다. 이는 최근 바인딩되지 않은 서비스가 시작 된 서비스이 고 `StopService` 또는 `StopSelf`를 호출 하지 않은 경우에 발생 합니다. 이러한 시나리오에서는 의도를 검색할 수 있도록 `OnRebind` 합니다. 기본값은 아무 작업도 수행 하지 않는 `false`을 반환 합니다. (선택 사항)
- `OnDestroy` &ndash;이 메서드는 Android에서 서비스를 삭제 하는 경우 호출 됩니다. 리소스 해제와 같은 필요한 정리는이 방법에서 수행 해야 합니다. (선택 사항)

바인딩된 서비스의 키 수명 주기 이벤트는 다음 다이어그램에 표시 됩니다.

![수명 주기 메서드가 호출 되는 순서를 보여 주는 다이어그램](bound-services-images/bound-services-01.png "수명 주기 메서드가 호출 되는 순서를 보여 주는 다이어그램")

이 가이드와 함께 제공 되는 도우미 응용 프로그램의 다음 코드 조각은 Xamarin.ios에서 바인딩된 서비스를 구현 하는 방법을 보여 줍니다.

```csharp
using Android.App;
using Android.Util;
using Android.Content;
using Android.OS;

namespace BoundServiceDemo
{
    [Service(Name="com.xamarin.ServicesDemo1")]
    public class TimestampService : Service, IGetTimestamp
    {
        static readonly string TAG = typeof(TimestampService).FullName;
        IGetTimestamp timestamper;

        public IBinder Binder { get; private set; }

        public override void OnCreate()
        {
            // This method is optional to implement
            base.OnCreate();
            Log.Debug(TAG, "OnCreate");
            timestamper = new UtcTimestamper();
        }

        public override IBinder OnBind(Intent intent)
        {
            // This method must always be implemented
            Log.Debug(TAG, "OnBind");
            this.Binder = new TimestampBinder(this);
            return this.Binder;
        }

        public override bool OnUnbind(Intent intent)
        {
            // This method is optional to implement
            Log.Debug(TAG, "OnUnbind");
            return base.OnUnbind(intent);
        }

        public override void OnDestroy()
        {
            // This method is optional to implement
            Log.Debug(TAG, "OnDestroy");
            Binder = null;
            timestamper = null;
            base.OnDestroy();
        }

        /// <summary>
        /// This method will return a formatted timestamp to the client.
        /// </summary>
        /// <returns>A string that details what time the service started and how long it has been running.</returns>
        public string GetFormattedTimestamp()
        {
            return timestamper?.GetFormattedTimestamp();
        }
    }
}
```

이 샘플에서 `OnCreate` 메서드는 클라이언트에서 요청 하는 타임 스탬프를 검색 하 고 포맷 하는 논리를 포함 하는 개체를 초기화 합니다. 첫 번째 클라이언트가 서비스에 바인딩하려는 경우 Android는 `OnBind` 메서드를 호출 합니다. 이 서비스는 클라이언트가 실행 중인 서비스의이 인스턴스에 액세스할 수 있도록 하는 `TimestampBinder` 개체를 인스턴스화합니다. @No__t_0 클래스는 다음 섹션에 설명 되어 있습니다.

### <a name="implementing-ibinder"></a>IBinder 구현

앞서 언급 했 듯이 `IBinder` 개체는 클라이언트와 서비스 간에 통신 채널을 제공 합니다. Android 응용 프로그램은 `IBinder` 인터페이스를 구현 해서는 안 되며 [`Android.OS.Binder`](xref:Android.OS.Binder) 확장 해야 합니다. @No__t_0 클래스는 서비스 (별도의 프로세스에서 실행 될 수 있음)에서 클라이언트로 바인더 개체를 마샬링하는 데 필요한 많은 인프라를 제공 합니다. 대부분의 경우 `Binder` 하위 클래스는 몇 줄의 코드에 불과합니다. 서비스에 대 한 참조를 래핑합니다. 이 예제에서는 `TimestampBinder`에 `TimestampService`를 클라이언트에 노출 하는 속성이 있습니다.

```csharp
public class TimestampBinder : Binder
{
    public TimestampBinder(TimestampService service)
    {
        this.Service = service;
    }

    public TimestampService Service { get; private set; }
}
```

이 `Binder`를 사용 하면 서비스에서 public 메서드를 호출할 수 있습니다. 예를 들어:

```csharp
string currentTimestamp = serviceConnection.Binder.Service.GetFormattedTimestamp()
```

@No__t_0 확장 된 후에는 `IServiceConnection`를 구현 하 여 모든 항목을 함께 연결 해야 합니다.

### <a name="creating-the-service-connection"></a>서비스 연결 만들기

@No__t_0 표시 됩니다 | 소개 | 노출 | `Binder` 개체를 클라이언트에 연결 합니다. @No__t_0 인터페이스를 구현 하는 것 외에도 클래스는 `Java.Lang.Object`를 확장 해야 합니다. 또한 서비스 연결은 클라이언트가 `Binder`에 액세스할 수 있도록 하는 몇 가지 방법을 제공 해야 합니다. 따라서 바인딩된 서비스와 통신 합니다.

이 코드는 함께 제공 되는 샘플 프로젝트의 `IServiceConnection` 구현할 수 있는 방법 중 하나입니다.

```csharp
using Android.Util;
using Android.OS;
using Android.Content;

namespace BoundServiceDemo
{
    public class TimestampServiceConnection : Java.Lang.Object, IServiceConnection, IGetTimestamp
    {
        static readonly string TAG = typeof(TimestampServiceConnection).FullName;

        MainActivity mainActivity;
        public TimestampServiceConnection(MainActivity activity)
        {
            IsConnected = false;
            Binder = null;
            mainActivity = activity;
        }

        public bool IsConnected { get; private set; }
        public TimestampBinder Binder { get; private set; }

        public void OnServiceConnected(ComponentName name, IBinder service)
        {
            Binder = service as TimestampBinder;
            IsConnected = this.Binder != null;

            string message = "onServiceConnected - ";
            Log.Debug(TAG, $"OnServiceConnected {name.ClassName}");

            if (IsConnected)
            {
                message = message + " bound to service " + name.ClassName;
                mainActivity.UpdateUiForBoundService();
            }
            else
            {
                message = message + " not bound to service " + name.ClassName;
                mainActivity.UpdateUiForUnboundService();
            }

            Log.Info(TAG, message);
            mainActivity.timestampMessageTextView.Text = message;

        }

        public void OnServiceDisconnected(ComponentName name)
        {
            Log.Debug(TAG, $"OnServiceDisconnected {name.ClassName}");
            IsConnected = false;
            Binder = null;
            mainActivity.UpdateUiForUnboundService();
        }

        public string GetFormattedTimestamp()
        {
            if (!IsConnected)
            {
                return null;
            }

            return Binder?.GetFormattedTimestamp();
        }
    }
}

```

바인딩 프로세스의 일부로 Android는 `OnServiceConnected` 메서드를 호출 하 여 바인딩되는 서비스의 `name` 및 서비스 자체에 대 한 참조를 포함 하는 `binder`을 제공 합니다. 이 예제에서 서비스 연결에는 바인더에 대 한 참조를 포함 하는 속성과 클라이언트를 서비스에 연결 하는 경우에 대 한 부울 플래그를 포함 하는 두 개의 속성이 있습니다.

@No__t_0 메서드는 클라이언트와 서비스 간의 연결이 예기치 않게 손실 되거나 손상 된 경우에만 호출 됩니다. 이 메서드를 사용 하면 클라이언트가 서비스 중단에 응답할 수 있습니다.  

## <a name="starting-and-binding-to-a-service-with-an-explicit-intent"></a>명시적 의도를 사용 하 여 서비스 시작 및 바인딩

바인딩된 서비스를 사용 하려면 클라이언트 (예: 작업)가 `Android.Content.IServiceConnection`를 구현 하 고 `BindService` 메서드를 호출 하는 개체를 인스턴스화해야 합니다. `BindService`는 서비스가에 바인딩되는 경우 `true`을 반환 하 고, 그렇지 않은 경우 `false`을 반환 합니다. `BindService` 메서드는 다음 세 개의 매개 변수를 사용합니다.

- 의도 &ndash; **`Intent`** 은 연결할 서비스를 명시적으로 식별 해야 합니다.
- 이 개체 &ndash; 하는 **`IServiceConnection` 개체** 는 바인딩된 서비스가 시작 및 중지 될 때 클라이언트에 알리기 위한 콜백 메서드를 제공 하는 매개체입니다.
- **열거형 &ndash; [`Android.Content.Bind`](xref:Android.Content.Bind)**  이 매개 변수는 시스템에서 개체를 바인딩할 때 사용 하는 플래그 집합입니다. 가장 일반적으로 사용 되는 값은 [`Bind.AutoCreate`](xref:Android.Content.Bind.AutoCreate)입니다. 서비스가 아직 실행 되지 않은 경우 자동으로 시작 됩니다.

다음 코드 조각은 명시적 의도를 사용 하 여 활동에서 바인딩된 서비스를 시작 하는 방법의 예입니다.

```csharp
protected override void OnStart ()
{
    base.OnStart ();

    if (serviceConnection == null)
    {
        this.serviceConnection = new TimestampServiceConnection(this);
    }

    Intent serviceToStart = new Intent(this, typeof(TimestampService));
    BindService(serviceToStart, this.serviceConnection, Bind.AutoCreate);

}
```

> [!IMPORTANT]
> Android 5.0 (API 수준 21)부터 명시적 의도를 사용 하 여 서비스에 바인딩할 수 있습니다.

## <a name="architectural-notes-about-the-service-connection-and-the-binder"></a>서비스 연결 및 바인더에 대 한 아키텍처 정보입니다.

일부 OOP purists는 [Demeter 법률](https://en.wikipedia.org/wiki/Law_of_Demeter) 을 위반 하는 것으로, 클래스 `TimestampBinder`의 이전 구현을 제거할 수 있습니다 .이 법칙은 가장 간단한 형태의 상태에서 "strangers와 통신 하지 않습니다. 친구 에게만 이야기 하세요. " 이 특정 구현은 모든 클라이언트에 구체적 `TimestampService` 클래스를 노출 합니다.

엄격 하 게 말하면 클라이언트는 `TimestampService`에 대 한 정보를 알 필요가 없으며 해당 클래스를 클라이언트에 노출 하 여 응용 프로그램을 더 불안정 하 고 수명 주기 동안 유지 관리 하기가 더 어려워집니다. 또 다른 방법은 `GetFormattedTimestamp()` 메서드를 노출 하는 인터페이스를 사용 하 고 `Binder` 또는 서비스 연결 클래스를 통해 서비스에 대 한 프록시 호출을 사용 하는 것입니다.  

```csharp
public class TimestampBinder : Binder, IGetTimestamp
{
    TimestampService service;
    public TimestampBinder(TimestampService service)
    {
        this.service = service;
    }

    public string GetFormattedTimestamp()
    {
        return service?.GetFormattedTimestamp();
    }
}
```

이 특정 예제에서는 활동에서 서비스 자체에 대 한 메서드를 호출할 수 있습니다.

```csharp
// In this example the Activity is only talking to a friend, i.e. the IGetTimestamp interface provided by the Binder.
string currentTimestamp = serviceConnection.Binder.GetFormattedTimestamp()
```

## <a name="related-links"></a>관련 링크

- [Android. App. 서비스](xref:Android.App.Service)
- [Android. 바인딩](xref:Android.Content.Bind)
- [Android. 내용. 컨텍스트](xref:Android.Content.Context)
- [IServiceConnection](xref:Android.Content.IServiceConnection)
- [Android. 바인더](xref:Android.OS.Binder)
- [Android. IBinder](xref:Android.OS.IBinder)
- [BoundServiceDemo (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-boundservicedemo)
