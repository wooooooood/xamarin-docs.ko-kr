---
title: Xamarin.Android에서 서비스 바인딩
description: '바인딩된 서비스는 클라이언트 (예: Android 활동) 상호 작용할 수 있는 클라이언트와 서버 인터페이스를 제공 하는 Android 서비스입니다. 이 가이드에서는 Xamarin.Android 응용 프로그램에서 사용 하는 방법과 바인딩된 서비스 만들기와 관련 된 주요 구성 요소를 설명 합니다.'
ms.prod: xamarin
ms.assetid: 809ECE88-EF08-4E9A-B389-A2DC08C51A6E
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: 18cfe6acae08efac85223c9c121a12f102f846cc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="bound-services-in-xamarinandroid"></a>Xamarin.Android에서 서비스 바인딩

_바인딩된 서비스는 클라이언트 (예: Android 활동) 상호 작용할 수 있는 클라이언트와 서버 인터페이스를 제공 하는 Android 서비스입니다. 이 가이드에서는 Xamarin.Android 응용 프로그램에서 사용 하는 방법과 바인딩된 서비스 만들기와 관련 된 주요 구성 요소를 설명 합니다._

## <a name="bound-services-overview"></a>바인딩된 서비스 개요

서비스와 직접 상호 작용 하는 클라이언트에 대 한 클라이언트-서버 인터페이스를 제공 하는 서비스 라고 _서비스 바인딩된_합니다.  여러 클라이언트가 동시에 서비스의 단일 인스턴스에 연결 되어 있을 수 있습니다. 바인딩된 서비스와 클라이언트가 서로 격리 됩니다. 대신, Android 일련의 둘 사이의 연결의 상태를 관리 하는 중간 개체를 제공 합니다. 이 상태는 구현 하는 개체에 의해 유지 관리는 [ `Android.Content.IServiceConnection` ](https://developer.xamarin.com/api/type/Android.Content.IServiceConnection/) 인터페이스입니다.  이 개체는 클라이언트에서 생성 하 고에 대 한 매개 변수로 전달 되는 [ `BindService` ](https://developer.xamarin.com/api/member/Android.Content.Context.BindService/) 메서드. `BindService` 는에서 사용 가능한 [ `Android.Content.Context` ](https://developer.xamarin.com/api/type/Android.Content.Context) 개체 (예: 활동). 이 Android 운영 체제 서비스를 시작 하는 클라이언트를 바인딩할를 요청 합니다. 세 가지 방법으로 클라이언트를 사용 하 여 서비스에 바인딩할 수 있습니다는 `BindService` 메서드:

* **서비스 바인더** &ndash; 서비스 바인더는를 구현 하는 클래스는 [ `Android.OS.IBinder` ](https://developer.xamarin.com/api/type/Android.OS.IBinder) 인터페이스입니다. 확장 되는 대신, 대부분의 응용 프로그램은 직접이 인터페이스를 구현 하지는 [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder) 클래스입니다. 가장 일반적인 방법은 이며 서비스와 클라이언트는 동일한 프로세스에 존재 하는 경우에 적합 합니다.
* **Messenger를 사용 하 여** &ndash; 이 방법은 서비스는 별도 프로세스에 있을 수는 경우에 적합 합니다. 서비스 요청을 통해 서비스와 클라이언트 간의 배열는 대신, 프로그램 [ `Android.OS.Messenger` ](https://developer.xamarin.com/api/type/Android.OS.Messenger)합니다. [ `Android.OS.Handler` ](https://developer.xamarin.com/api/type/Android.OS.Handler) 처리 하는 서비스에 생성 됩니다는 `Messenger` 요청 합니다. 다른 가이드에서 설명 합니다.
* **AIDL를 사용 하 여** &ndash; 이 테러 대부분 Android 개발자의 핵심에 발생 하며이 가이드에서 다루지 것입니다.

클라이언트가 서비스에 바인딩된 후에 둘 사이의 통신을 통해 발생 `Android.OS.IBinder` 개체입니다.  이 개체는 서비스와 상호 작용 하는 클라이언트 수 있는 인터페이스에 대 한. Android SDK 제공 처음부터이 인터페이스를 구현 하려면 각 Xamarin.Android 응용 프로그램에 필요한 경우가 아니라면는 [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder) 마샬링 간에 개체와 필요한 코드의 대부분을 담당 하는 클래스 클라이언트와 서비스를 제공 합니다.

클라이언트가 서비스와 함께 완료 되 면 해당 바인딩을 해제 해야 합니다를 호출 하 여는 `UnbindService` 메서드. 서비스에서 마지막 클라이언트에 언바운드 되 면 Android 중지 하 고 바인딩된 서비스를 삭제 합니다.

이 다이어그램 서로 관련 활동, 서비스 연결, 바인더 및 서비스 모두 보여 줍니다.

![서비스 구성 요소 간의 관계를 보여 주는 다이어그램](bound-services-images/bound-services-02.png "서비스 구성 요소 간의 관련 방식을 보여 주는 다이어그램입니다.")

이 가이드에는 확장 하는 방법을 설명 합니다는 `Service` 바운드 서비스를 구현 하는 클래스입니다. 구현에 적용 됩니다 `IServiceConnection` 및 확장 `Binder` 클라이언트가 서비스와 통신할 수 있도록 합니다. 샘플 응용 프로그램을 라는 단일 Xamarin.Android 프로젝트와 솔루션을 포함 하는이 가이드에서는 포함 **[BoundServiceDemo](https://github.com/xamarin/monodroid-samples/tree/master/ApplicationFundamentals/ServiceSamples/BoundServiceDemo)** 합니다. 이 서비스를 구현 하는 방법과 활동을 바인딩하는 방법을 설명 하는 매우 기본적인 응용 프로그램입니다. 바인딩된 서비스에 단 하나의 메서드를 사용 하 여 매우 간단한 API `GetFormattedTimestamp`, 서비스가 시작 될 때 사용자를 알려 주는 문자열 및 기간 실행 된 반환 하는 합니다. 응용 프로그램에는 사용자를가 직접 바인딩을 해제 하는 서비스에 바인딩할 수 있습니다.

[![Android 휴대폰에서 실행 되는 응용 프로그램의 스크린 샷](bound-services-images/bound-services-03-sml.png)](bound-services-images/bound-services-03.png#lightbox)

## <a name="implementing-and-consuming-a-bound-service"></a>구현 및 바운드 서비스 사용

바운드 서비스를 사용 하는 Android 응용 프로그램을 위해 구현 해야 하는 세 가지 구성 요소 가지가 있습니다.

1. **확장 된 `Service` 클래스 및 수명 주기 콜백 메서드를 구현** &ndash; 이 클래스는 서비스의 요청 하는 작업을 수행 하는 코드가 포함 됩니다. 아래에서 자세히 설명 합니다.
2. **구현 하는 클래스를 만듭니다 `IServiceConnection`**  &ndash; 이 개체에서 서비스에 연결 (하거나 때의 연결이 해제) 클라이언트에 알리는 콜백 메서드를 포함 합니다. 또한 서비스 연결 클라이언트는 서비스와 직접 상호 작용 하는 데 사용할 수 있는 개체에 대 한 참조를 제공 합니다. 이 참조 라고는 _바인더_합니다.
3. **구현 하는 클래스를 만듭니다 `IBinder`**  &ndash; A _바인더_ 구현에서는 서비스와 통신 하는 클라이언트에서 사용 하는 API를 제공 합니다. 바인더 입력 하거나 바인딩된 서비스에 대 한 참조를 직접 호출 될 메서드를 허용 하거나 바인더 클라이언트 API를 캡슐화 하 고 숨깁니다 바인딩된 응용 프로그램에서 서비스를 제공할 수 있습니다. `IBinder` 원격 프로시저 호출에 필요한 코드를 제공 해야 합니다. 필요한 (또는 권장)를 구현 하는 `IBinder` 인터페이스를 직접 합니다. `IBinder` Instead 응용 프로그램을 확장 해야는 `Binder` 대부분의에 필요한 기본 기능을 제공 하는 `IBinder`합니다.
4. **시작 및 서비스에 바인딩** &ndash; Android 응용 프로그램은 서비스를 시작 하 여 바인딩를 서비스 연결, 바인더 및 서비스 작성 되 면 합니다.

이러한 각 단계에서 자세히 다음 섹션에서 설명 합니다.

### <a name="extend-the-service-class"></a>확장 된 `Service` 클래스

Xamarin.Android를 사용 하 여 서비스를 만들려면 해야 하는 하위 클래스 `Service` 를 표시 하 고는 사용 하 여 클래스는 [ `ServiceAttribute` ](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute)합니다. Xamarin.Android 빌드 도구에 의해 제대로 응용 프로그램에 서비스를 등록 하는 특성은 사용 **AndroidManifest.xml** 것과 마찬가지로 활동 파일, 바인딩된 서비스는 자체 수명 주기 및 콜백와 연결 된 메서드는 수명 주기에서 중요 한 이벤트입니다. 다음 목록은 서비스를 구현 하는 보다 일반적인 콜백 메서드 중 일부를 보여 줍니다.

* `OnCreate` &ndash; 이 메서드는 서비스를 인스턴스화하는 것 처럼 Android에서 호출 됩니다. 모든 변수 또는 개체의 수명 동안 서비스에 필요한 초기화 사용 됩니다. 이 메서드는 선택 사항입니다.
* `OnBind` &ndash; 이 메서드는 바인딩된 모든 서비스에서 구현 해야 합니다. 첫 번째 클라이언트가 서비스에 연결 하려고 할 때 호출 됩니다. 인스턴스를 반환 합니다 `IBinder` 클라이언트가 서비스와 상호 작용할 수 있도록 합니다. 서비스가 실행 상태로 `IBinder` 개체는 서비스에 바인딩할 이후의 클라이언트 요청을 처리 하는 데 사용 됩니다.
* `OnUnbind` &ndash; 이 메서드는 바인딩된 모든 클라이언트에 바인딩되지 않은 있을 때 호출 됩니다. 반환 하 여 `true` 서비스는 나중에이 메서드에서 호출 `OnRebind` 에 전달 된 의도로 `OnUnbind` 새 클라이언트에 바인딩할 때. 서비스 계속 바인딩된 된 후 실행 하는 경우이 작업을 수행 합니다. 최근에 바인딩되지 않은 서비스는 시작된 서비스도 있었습니다. 이럴 및 `StopService` 또는 `StopSelf` 하지 않은 호출 되었습니다. 이러한 시나리오에서는 `OnRebind` 의도를 검색할 수 있습니다. 기본값은 반환 `false` , 하는 아무 작업도 수행 합니다. 선택 사항입니다.
* `OnDestroy` &ndash; 이 메서드는 Android는 서비스를 삭제 하는 경우 호출 됩니다. 이 방법에서는 리소스를 해제 하는 등 필요한 정리를 수행 되어야 합니다. 선택 사항입니다.

바인딩된 서비스의 키 수명 주기 이벤트는이 다이어그램에 표시 됩니다.

![수명 주기 메서드 호출 되는 순서를 보여 주는 다이어그램](bound-services-images/bound-services-01.png "수명 주기 메서드 호출 되는 순서를 보여 주는 다이어그램입니다.")

이 가이드를 함께 제공 되는 도우미 응용 프로그램에서 다음 코드 조각 Xamarin.Android에 바인딩된 서비스를 구현 하는 방법을 보여 줍니다.

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

이 샘플에서는 `OnCreate` 메서드를 검색 하 고 클라이언트에서 요청할 수는 타임 스탬프를 서식 지정에 대 한 논리를 포함 하는 개체를 초기화 합니다. 첫 번째 클라이언트가 서비스에 바인딩을 시도, Android 호출 하는 `OnBind` 메서드. 이 서비스는 인스턴스화하고 `TimestampServiceBinder` 개체 클라이언트가 실행 중인 서비스의이 인스턴스를 액세스할 수 있도록 합니다. `TimestampServiceBinder` 클래스에 다음 섹션에 설명 합니다.

### <a name="implementing-ibinder"></a>IBinder 구현

앞서 언급 했 듯이 `IBinder` 개체는 클라이언트와 서비스 간의 통신 채널을 제공 합니다. Android 응용 프로그램을 구현 하지 않아야는 `IBinder` 인터페이스는 [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder/) 확장 해야 합니다. `Binder` 클래스는 필요한 마샬링하는 데 필요한 인프라 많이 바인더 개체 (된 별도 프로세스에서 실행 될 수)에서 클라이언트에 제공 합니다. 대부분의 경우에는 `Binder` 하위 클래스는 단 몇 줄의 코드 및 서비스에 대 한 참조를 래핑합니다. 이 예제에서는 `TimestampBinder` 노출 하는 속성에는 `TimestampService` 클라이언트:

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

이 `Binder` ; 서비스의 공용 메서드를 호출할 수 있도록 예:

```csharp
string currentTimestamp = serviceConnection.Binder.Service.GetFormattedTimestamp()
```

한 번 `Binder` 되었습니다 확장 해야 하는 구현 `IServiceConnection` 모든 항목을 서로 연결 하 합니다.

### <a name="creating-the-service-connection"></a>서비스 연결 만들기

`IServiceConnection` 제시 합니다 | 소개 | 노출 | 연결의 `Binder` 클라이언트에는 개체입니다. 구현 외에도 `IServiceConnection` 인터페이스, 클래스를 확장 해야 `Java.Lang.Object`합니다. 서비스 연결에는 클라이언트에 액세스할 수 있는 몇 가지 방식으로 제공 해야는 `Binder` (및 따라서 바인딩된 서비스와 통신).

이 코드에서 함께 제공 되는 샘플 프로젝트에는 한 가지 방법을 구현 하는 `IServiceConnection`:

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

Android는 호출 하는 바인딩 프로세스의 일부로 `OnServiceConnected` 메서드를 제공 하는 `name` 바인딩되는 서비스의 및 `binder` 서비스 자체에 대 한 참조를 보유 하는 합니다. 이 예제에서는 서비스 연결에는 두 개의 속성, 클라이언트 또는 서비스에 연결 되어 있는 경우 바인더에 대 한 부울 플래그에 대 한 참조를 보유 하는 하나 있습니다.

`OnServiceDisconnected` 메서드는 클라이언트와 서비스 간의 연결이 예기치 않게 손실 되거나 손상에 호출 됩니다. 이 메서드는 클라이언트가 서비스의 중단에 응답할 수 있는 기회를 수 있습니다.  

## <a name="starting-and-binding-to-a-service-with-an-explicit-intent"></a>시작 및 명시적 의도 가진 서비스에 바인딩

바인딩된 서비스를 사용 하려면 클라이언트 (예: 활동) 구현 하는 개체를 인스턴스화해야 합니다. 그렇지 `Android.Content.IServiceConnection` 하 고 호출에서 `BindService` 메서드.` BindService` 반환 `true` 서비스 바인딩되어 있으면 `false` 없는 경우. `BindService` 메서드는 다음 세 개의 매개 변수를 사용합니다.

* **`Intent`**  &ndash; The 의도에 연결 하는 서비스를 명시적으로 식별 해야 합니다.
* **`IServiceConnection` 개체** &ndash; 이 개체는 바인딩된 서비스 시작 및 중지 하는 경우 클라이언트에 알리기 위해 콜백 메서드를 제공 하는 중간자 합니다.
* **[`Android.Content.Bind`](https://developer.xamarin.com/api/type/Android.Content.Bind/) enum** &ndash; 플래그 집합은이 매개 변수는 경우 개체를 바인딩할 시스템에서 사용 됩니다. 가장 일반적으로 사용 되는 값은 [ `Bind.AutoCreate` ](https://developer.xamarin.com/api/field/Android.Content.Bind.AutoCreate/), 실행 되지 않았으면 서비스를 자동으로 시작 합니다입니다.

다음 코드 조각은 명시적 의도 사용 하 여 활동에 바인딩된 서비스를 시작 하는 방법의 예입니다.

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
> Android 5.0 (API 수준 21)과 함께 시작은 명시적 의도 가진 서비스에 바인딩할 수만 있습니다.

## <a name="architectural-notes-about-the-service-connection-and-the-binder"></a>바인더와 서비스 연결에 대 한 아키텍처 정보입니다.

이전 구현의 일부 OOP 자라면 승인을 취소할 수 있습니다는 `TimestampBinder` 위반 이므로 클래스는 [데메테르 법률](https://en.wikipedia.org/wiki/Law_of_Demeter) 는 가장 간단한 형태에 따르면 "대화를 나눠 모르는; 만 통신할 친구 "입니다. 이 특정 구현에는 비추상 노출 `TimestampService` 모든 클라이언트에는 클래스입니다.

엄격히 말해서, 클라이언트에 대해 알고 필요 하지 않습니다는 `TimestampService` 하 고 클라이언트에 해당 구체적 클래스를 노출 만들 수는 응용 프로그램 보다 불안정 하 고의 수명 주기 동안 유지 관리 하기 쉽다는 점입니다. 또 다른 방법은 노출 하는 인터페이스를 사용 하는 것은 `GetFormattedTimestamp()` 메서드를 통해 서비스에 대 한 프록시 호출은 `Binder` (또는 가능한 서비스 연결 클래스).  

```csharp
public class TimestampBinder : Binder, IGetTimesamp
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

이 예는 서비스 자체에서 메서드를 호출 하기 위한 작업 허용:

```csharp
// In this example the Activity is only talking to a friend, i.e. the IGetTimestamp interface provided by the Binder.
string currentTimestamp = serviceConnection.Binder.GetFormattedTimestamp()
```


## <a name="related-links"></a>관련 링크

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.Content.Bind](https://developer.xamarin.com/api/type/Android.Content.Bind/)
- [Android.Content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [Android.Content.IServiceConnection](https://developer.xamarin.com/api/type/Android.Content.IServiceConnection/)
- [Android.OS.Binder](https://developer.xamarin.com/api/type/Android.OS.Binder/)
- [Android.OS.IBinder](https://developer.xamarin.com/api/type/Android.OS.IBinder)
- [BoundServiceDemo (샘플)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/BoundServiceDemo/)
