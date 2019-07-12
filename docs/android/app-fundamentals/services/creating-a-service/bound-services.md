---
title: Xamarin.Android에 바인딩된 서비스
description: '바인딩된 서비스는 클라이언트 (예: Android 활동)와 상호 작용할 수 있는 클라이언트-서버 인터페이스를 제공 하는 Android 서비스입니다. 이 가이드에서는 Xamarin.Android 응용 프로그램에서 사용 하는 방법과 바인딩된 서비스 만들기와 관련 된 주요 구성 요소를 설명 합니다.'
ms.prod: xamarin
ms.assetid: 809ECE88-EF08-4E9A-B389-A2DC08C51A6E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/04/2018
ms.openlocfilehash: 490331663d94a1e3130fc794a11a52acdacca014
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67829753"
---
# <a name="bound-services-in-xamarinandroid"></a>Xamarin.Android에 바인딩된 서비스

_바인딩된 서비스는 클라이언트 (예: Android 활동)와 상호 작용할 수 있는 클라이언트-서버 인터페이스를 제공 하는 Android 서비스입니다. 이 가이드에서는 Xamarin.Android 응용 프로그램에서 사용 하는 방법과 바인딩된 서비스 만들기와 관련 된 주요 구성 요소를 설명 합니다._

## <a name="bound-services-overview"></a>바인딩된 서비스 개요

서비스와 직접 상호 작용 하는 클라이언트에 대 한 클라이언트-서버 인터페이스를 제공 하는 서비스 라고 _바인딩된 서비스_합니다.  여러 클라이언트가 동시에 서비스의 단일 인스턴스에 연결 되어 있을 수 있습니다. 바인딩된 서비스와 클라이언트는 서로 격리 됩니다. 대신, Android 일련의 둘 간의 연결의 상태를 관리 하는 중간 개체를 제공 합니다. 이 상태는 구현 하는 개체에서 유지 관리 합니다 [ `Android.Content.IServiceConnection` ](https://developer.xamarin.com/api/type/Android.Content.IServiceConnection/) 인터페이스입니다.  클라이언트에서이 개체를 만들어 매개 변수로 전달 된 [ `BindService` ](https://developer.xamarin.com/api/member/Android.Content.Context.BindService/) 메서드. 합니다 `BindService` 에서 사용할 수 [ `Android.Content.Context` ](https://developer.xamarin.com/api/type/Android.Content.Context) 개체 (예: 활동). 것이 서비스를 시작 하 여 클라이언트를 바인딩할 Android 운영 체제를 요청 합니다. 세 가지 방법으로 클라이언트를 사용 하 여 서비스에 바인딩할 수 있습니다는 `BindService` 메서드:

* **서비스 바인더** &ndash; 서비스 바인더를 구현 하는 클래스는 합니다 [ `Android.OS.IBinder` ](https://developer.xamarin.com/api/type/Android.OS.IBinder) 인터페이스입니다. 대부분의 응용 프로그램이이 인터페이스를 직접 구현 하지 것입니다는 확장 대신 합니다 [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder) 클래스입니다. 이 가장 일반적인 방법은 이며 서비스와 클라이언트는 동일한 프로세스에 존재 하는 경우에 적합 합니다.
* **Messenger를 사용 하 여** &ndash; 이 방법은 별도의 프로세스로 서비스 있을 때 적합 합니다. 클라이언트와 서비스를 통해 서비스 요청은 배열 하는 대신 한 [ `Android.OS.Messenger` ](https://developer.xamarin.com/api/type/Android.OS.Messenger)합니다. [ `Android.OS.Handler` ](https://developer.xamarin.com/api/type/Android.OS.Handler) 처리 하는 서비스에서 만들어진는 `Messenger` 요청 합니다. 이 다른 가이드에서 다루어야 합니다.
* **인터페이스 정의 언어 AIDL (Android)를 사용 하 여** &ndash; [AIDL](https://developer.android.com/guide/components/aidl) 은 고급 기술이이 가이드에서 다루지 것입니다.

둘 사이의 통신은 클라이언트가 서비스에 바인딩한 후을 통해 발생 `Android.OS.IBinder` 개체입니다.  이 개체는 클라이언트는 서비스와 상호 작용할 수 있도록 인터페이스를 담당 합니다. Android SDK를 제공 하지 않으면 각 Xamarin.Android 응용 프로그램부터에서이 인터페이스를 구현 하는 데 필요한 합니다 [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder) 간에 개체 마샬링 사용 하 여는 데 필요한 코드의 대부분을 처리 하는 클래스 클라이언트와 서비스를 제공 합니다.

클라이언트는 서비스를 사용 하 여 완료 되 면 해당 바인딩을 해제 해야 합니다를 호출 하 여는 `UnbindService` 메서드. 마지막으로 클라이언트는 서비스에서 바인딩되지 않은, 되 면 Android 중지 하 고 바인딩된 서비스 삭제 됩니다.

이 다이어그램은 서로 관련 작업, 서비스 연결, 바인더 및 서비스 모두 보여 줍니다.

![서비스 구성 요소 간의 관계를 보여 주는 다이어그램](bound-services-images/bound-services-02.png "서비스 구성 요소 간의 관계를 보여 주는 다이어그램입니다.")

이 가이드에서는 확장 하는 방법을 설명 합니다 `Service` 바인딩된 서비스를 구현 하는 클래스입니다. 구현에 적용 됩니다 `IServiceConnection` 확장 및 `Binder` 는 클라이언트가 서비스와 통신할 수 있도록 합니다. 라는 단일 Xamarin.Android 프로젝트를 사용 하 여 솔루션을 포함 하는 샘플 앱을 함께이 가이드에서는 **[BoundServiceDemo](https://github.com/xamarin/monodroid-samples/tree/master/ApplicationFundamentals/ServiceSamples/BoundServiceDemo)** 합니다. 이 서비스를 구현 하는 방법 및 작업에 바인딩하는 방법을 보여 주는 기본적인 응용 프로그램입니다. 바인딩된 서비스에 하나의 메서드를 사용 하 여 매우 간단한 API `GetFormattedTimestamp`, 서비스가 시작 되었을 때 사용자를 알려 주는 문자열 및 기간 실행 된 반환 합니다. 앱에는 사용자를를 수동으로 바인딩을 해제 하 고 서비스를 바인딩할 수 있습니다.

[![Android 휴대폰에서 실행 중인 응용 프로그램의 스크린샷](bound-services-images/bound-services-03-sml.png)](bound-services-images/bound-services-03.png#lightbox)

## <a name="implementing-and-consuming-a-bound-service"></a>구현 및 바인딩된 서비스를 사용 합니다.

바인딩된 서비스를 사용 하는 Android 응용 프로그램에서 구현 해야 하는 세 가지 구성 요소는

1. **확장 된 `Service` 클래스 및 수명 주기 콜백 메서드를 구현** &ndash; 이 클래스는 서비스의 요청 하는 작업을 수행 하는 코드가 포함 됩니다. 아래에서 자세히 설명 합니다.
2. **구현 하는 클래스를 만듭니다 `IServiceConnection`**  &ndash; 콜백 메서드는 호출을이 인터페이스는 제공 서비스에 대 한 연결이 변경 되 면 클라이언트에 알리기 위해 Android에서 즉, 클라이언트에 연결 또는 연결 끊김에 서비스입니다. 서비스 연결 클라이언트는 서비스와 직접 상호 작용 하는 데 사용할 수 있는 개체에 대 한 참조도 제공 합니다. 이 참조 라고 합니다 _바인더_합니다.
3. **구현 하는 클래스를 만듭니다 `IBinder`**  &ndash; A _바인더_ 구현은 클라이언트가 서비스와 통신 하는 데 사용 하는 API를 제공 합니다. 바인더를 제공 하거나 바인딩된 서비스에 대 한 참조를 직접 호출 될 메서드를 허용 하거나 바인더는 클라이언트 API를 캡슐화 하 고 응용 프로그램에서 바인딩된 서비스를 숨기는 제공할 수 있습니다. `IBinder` 원격 프로시저 호출에 필요한 코드를 제공 해야 합니다. 이 없거나 필요 (권장)를 구현 하는 `IBinder` 인터페이스를 직접. 대신 응용 프로그램을 확장 해야 합니다 `Binder` 형식에 필요한 기본 기능을 대부분 제공 하는 `IBinder`합니다.
4. **시작 하 고 서비스에 바인딩** &ndash; Android 응용 프로그램은 서비스를 시작 하 고 되도록 바인딩 서비스 연결, 바인더 및 서비스를 만들면 됩니다.

이러한 각 단계 자세히의 다음 섹션에서 설명 합니다.

### <a name="extend-the-service-class"></a>확장 된 `Service` 클래스

Xamarin.Android를 사용 하는 서비스를 만들려면 서브 클래스 하는 데 필요한 것 `Service` 를 사용 하 여 클래스를 표시 합니다 [ `ServiceAttribute` ](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute)합니다. 특성은 Xamarin.Android 빌드 도구에 의해 앱의 서비스를 제대로 등록 하는 데 사용 됩니다 **AndroidManifest.xml** 활동 비슷하게 파일에 바인딩된 서비스 자체 수명 주기 콜백 메서드와 연관 된 수명 주기에서 중요 한 이벤트입니다. 다음 목록은 서비스를 구현 하는 보다 일반적인 콜백 메서드의 일부를 보여 줍니다.

* `OnCreate` &ndash; 이 메서드는 서비스를 인스턴스화하는 것 처럼 Android에서 호출 됩니다. 모든 변수 또는 개체의 수명 동안 서비스에 필요한 초기화 하기 위해 사용 됩니다. 이 메서드는 선택 사항입니다.
* `OnBind` &ndash; 이 메서드는 바인딩된 모든 서비스에서 구현 되어야 합니다. 첫 번째 클라이언트가 서비스에 연결 하려고 할 때 호출 됩니다. 인스턴스를 반환 합니다 `IBinder` 클라이언트는 서비스와 상호 작용할 수 있도록 합니다. 서비스는 실행 되는 `IBinder` 개체를 바인딩할 모든 향후 클라이언트 요청을 수행 하 게 됩니다.
* `OnUnbind` &ndash; 이 메서드는 바인딩된 모든 클라이언트는 바인딩되지 않은 경우 호출 됩니다. 되돌려 `true` 서비스는 나중에이 메서드에서 호출 `OnRebind` 전달할 의도로 `OnUnbind` 새 클라이언트를 바인딩하는 경우. 이렇게 하려면 서비스를 계속 바인딩된 된 후 실행 하는 경우. 최근에 바인딩되지 않은 서비스 시작된 서비스에도 되었으면 상황이이 고 `StopService` 또는 `StopSelf` 호출 하지 않은 합니다. 이러한 시나리오에서는 `OnRebind` 의도를 검색할 수 있습니다. 기본값은 반환 `false` , 아무 작업도 수행 하지는 합니다. 선택 사항입니다.
* `OnDestroy` &ndash; 이 메서드는 Android는 서비스를 제거 하는 경우 호출 됩니다. 리소스 해제와 같이 필요한 정리,이 메서드에서 수행 되어야 합니다. 선택 사항입니다.

바인딩된 서비스의 키 수명 주기 이벤트는이 다이어그램에 나와 있습니다.

![수명 주기 메서드는 호출 되는 순서를 보여 주는 다이어그램](bound-services-images/bound-services-01.png "수명 주기 메서드는 호출 되는 순서를 보여 주는 다이어그램입니다.")

이 가이드와 함께 제공 되는 도우미 응용 프로그램에서 다음 코드 조각에서는 Xamarin.Android에서 바인딩된 서비스를 구현 하는 방법을 보여 줍니다.

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

이 샘플에서는 `OnCreate` 메서드를 검색 하 고 할 수 없게 클라이언트에서 요청 하는 타임 스탬프를 서식 지정에 대 한 논리를 포함 하는 개체를 초기화 합니다. 첫 번째 클라이언트가 서비스에 바인딩하려고 할 때, Android 호출을 `OnBind` 메서드. 이 서비스를 인스턴스화하는 `TimestampBinder` 하면 클라이언트가 실행 중인 서비스의이 인스턴스에 액세스 하는 개체입니다. `TimestampBinder` 클래스에 다음 섹션에 설명 합니다.

### <a name="implementing-ibinder"></a>IBinder 구현

언급 했 듯이 `IBinder` 개체는 클라이언트와 서비스 간의 통신 채널을 제공 합니다. Android 응용 프로그램을 구현 하지 않아야 합니다 `IBinder` 인터페이스를 [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder/) 확장 해야 합니다. `Binder` 클래스는 필요한 마샬링하는 데 필요한 인프라 많이 바인더 개체 (별도의 프로세스로 실행 될 수)는 서비스에서 클라이언트에 제공 합니다. 대부분의 경우에는 `Binder` 하위 클래스 코드 몇 줄만 되며 서비스에 대 한 참조를 래핑합니다. 이 예에서 `TimestampBinder` 노출 하는 속성에는 `TimestampService` 클라이언트:

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

이 `Binder` 서비스에서 공용 메서드를 호출할 수 있도록 예를 들어:

```csharp
string currentTimestamp = serviceConnection.Binder.Service.GetFormattedTimestamp()
```

한 번 `Binder` 되었으면 확장 해야 하는 구현 `IServiceConnection` 모두 함께 연결 합니다.

### <a name="creating-the-service-connection"></a>서비스 연결 만들기

`IServiceConnection` 나타납니다 | 소개 | 노출 | 연결을 `Binder` 클라이언트 개체입니다. 구현 하는 것 외에도 합니다 `IServiceConnection` 인터페이스 클래스를 확장 해야 `Java.Lang.Object`합니다. 서비스 연결에는 클라이언트에 액세스할 수 있는 몇 가지 방식으로 제공 해야 합니다 `Binder` (및 따라서 바인딩된 서비스와 통신할).

이 코드에서 함께 제공 되는 샘플 프로젝트는 한 가지 방법을 구현 하려면 `IServiceConnection`:

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

Android 바인딩 프로세스의 일부로 호출 합니다 `OnServiceConnected` 메서드를 제공 하는 `name` 바인딩되는 서비스의 및 `binder` 서비스 자체에 대 한 참조를 보유 하는 합니다. 이 예제에서는 서비스 연결에는 두 개의 속성인 여부 클라이언트 서비스에 연결 되어 있으면 바인더에 대 한 부울 플래그에 대 한 참조를 보유 하는 하나 있습니다.

`OnServiceDisconnected` 메서드는 클라이언트와 서비스 간의 연결이 예기치 않게 분실 또는 고장 난 경우에 호출 됩니다. 이 메서드는 클라이언트는 서비스 중단을 응답할 수가 있습니다.  

## <a name="starting-and-binding-to-a-service-with-an-explicit-intent"></a>시작 하 고 명시적 의도 사용 하 여 서비스에 바인딩

바인딩된 서비스를 사용 하는 클라이언트 (예: 활동) 구현 하는 개체 인스턴스화해야 `Android.Content.IServiceConnection` 호출을 `BindService` 메서드. `BindService` 돌아갑니다 `true` 서비스에 바인딩된 경우 `false` 없는 경우. `BindService` 메서드는 다음 세 개의 매개 변수를 사용합니다.

* **`Intent`**  &ndash; 의 의도에 연결 하는 서비스를 명시적으로 식별 해야 합니다.
* **`IServiceConnection` 개체** &ndash; 이 개체가 바인딩된 서비스 시작 및 중지 하는 경우 클라이언트에 알리기 위해 콜백 메서드를 제공 하는 중개자입니다.
* **[`Android.Content.Bind`](https://developer.xamarin.com/api/type/Android.Content.Bind/) 열거형** &ndash; 이 매개 변수는 플래그 집합이에 개체를 바인딩할 때 시스템에서 사용 됩니다. 가장 자주 사용 되는 값이 [ `Bind.AutoCreate` ](https://developer.xamarin.com/api/field/Android.Content.Bind.AutoCreate/), 자동 시작 서비스가 이미 실행 되지 않는 경우.

다음 코드 조각은 다음과 같습니다. 명시적 의도 사용 하 여 활동에 바인딩된 서비스를 시작 하는 방법의 예

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
> Android 5.0 (API 수준 21)를 사용 하 여 시작은 명시적 의도 사용 하 여 서비스에 바인딩할 수만 있습니다.

## <a name="architectural-notes-about-the-service-connection-and-the-binder"></a>서비스 연결 및 바인더에 대 한 아키텍처 정보입니다.

이전 구현의 일부 OOP 주의자 반대할 수는 `TimestampBinder` 위반 이므로 클래스는 [데메테르의 법칙](https://en.wikipedia.org/wiki/Law_of_Demeter) 는 가장 간단한 형태로 상태 "대화를 나눠 면허증; 만 친구에 게 "말입니다. 이 특정 구현에는 구체적인 노출 `TimestampService` 모든 클라이언트에는 클래스입니다.

클라이언트에 대해 알아야 할 필요 없는 엄격 하 게 말해는 `TimestampService` 하 고 클라이언트에는 구체적인 클래스를 노출 수 응용 프로그램 더 불안정 하 고 해당 수명 동안 유지 관리 하기 어렵습니다. 대안을 노출 하는 인터페이스를 사용 하는 것을 `GetFormattedTimestamp()` 메서드를 통해 서비스에 대 한 프록시 호출의 `Binder` (또는 가능한 서비스 연결 클래스).  

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

이 특정 예제에서는 서비스 자체에서 메서드를 호출 하기 위한 작업을 허용 합니다.

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
- [BoundServiceDemo (sample)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/BoundServiceDemo/)
