---
title: 원격 프로세스에서 Android 서비스 실행
description: 일반적으로 Android 응용 프로그램의 모든 구성 요소는 동일한 프로세스에서 실행 됩니다. Android 서비스는 다른 Android 개발자의 프로세스를 비롯 하 여 자체 프로세스에서 실행 되 고 다른 응용 프로그램과 공유 되도록 구성할 수 있다는 점에서 주목할 만한 예외입니다. 이 가이드에서는 Xamarin을 사용 하 여 Android 원격 서비스를 만들고 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 27A2E972-A690-480B-B31D-5EF1F74F673C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 2794a1d23cd7c1eab9cf4e94eaa805ad2b8bca61
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70119127"
---
# <a name="running-android-services-in-remote-processes"></a>원격 프로세스에서 Android 서비스 실행

_일반적으로 Android 응용 프로그램의 모든 구성 요소는 동일한 프로세스에서 실행 됩니다. Android 서비스는 다른 Android 개발자의 프로세스를 비롯 하 여 자체 프로세스에서 실행 되 고 다른 응용 프로그램과 공유 되도록 구성할 수 있다는 점에서 주목할 만한 예외입니다. 이 가이드에서는 Xamarin을 사용 하 여 Android 원격 서비스를 만들고 사용 하는 방법을 설명 합니다._

## <a name="out-of-process-services-overview"></a>Out-of-process 서비스 개요

응용 프로그램이 시작 되 면 Android는 응용 프로그램을 실행 하는 프로세스를 만듭니다. 일반적으로이 프로세스에서 응용 프로그램이 실행 되는 모든 구성 요소입니다. Android 서비스는 다른 Android 개발자의 프로세스를 비롯 하 여 자체 프로세스에서 실행 되 고 다른 응용 프로그램과 공유 되도록 구성할 수 있다는 점에서 주목할 만한 예외입니다. 이러한 유형의 서비스를 _원격 서비스_ 또는 _out-of-process 서비스_라고 합니다. 이러한 서비스에 대 한 코드는 주 응용 프로그램과 동일한 APK에 포함 됩니다. 그러나 서비스를 시작 하면 Android에서 해당 서비스에 대해서만 새 프로세스를 만듭니다. 이와 달리 응용 프로그램의 나머지 부분과 동일한 프로세스에서 실행 되는 서비스는 _로컬 서비스_라고도 합니다.

일반적으로 응용 프로그램에서 원격 서비스를 구현할 필요는 없습니다. 대부분의 경우 로컬 서비스는 앱의 요구 사항에 따라 충분 하 고 바람직한 방법입니다. Out-of-process에는 Android에서 관리 해야 하는 자체 메모리 공간이 있습니다. 이 경우 전체 응용 프로그램에 더 많은 오버 헤드가 발생 하지만 서비스를 자체 프로세스에서 실행 하는 것이 유용할 수 있는 몇 가지 시나리오가 있습니다.

1. **공유 기능** &ndash; 일부 응용 프로그램 개발자에 게는 모든 응용 프로그램 간에 공유 되는 여러 앱 및 기능이 있을 수 있습니다. 자체 프로세스에서 실행 되는 Android 서비스에서 해당 기능을 패키징하 면 응용 프로그램 유지 관리를 간소화할 수 있습니다. 또한 자체 독립 실행형 APK에서 서비스를 패키지 하 고 응용 프로그램의 나머지 부분과 별도로 배포할 수 있습니다.
2. **사용자 환경 개선** &ndash; Out-of-process 서비스가 응용 프로그램의 사용자 환경을 향상 시킬 수 있는 두 가지 가능한 방법이 있습니다. 첫 번째 방법은 메모리 관리를 처리 하는 것입니다. GC (가비지 수집) 순환이 발생 하면 Android는 GC가 완료 될 때까지 프로세스의 모든 작업을 일시 중지 합니다. 사용자가이 일시 중지를 "끊길" 또는 "jank"로 인지할 수 있습니다. 서비스가 자체 프로세스에서 실행 되는 경우 응용 프로그램 프로세스가 아닌 일시 중지 된 서비스 프로세스입니다. 이 일시 중지는 응용 프로그램 프로세스 (따라서 사용자 인터페이스)가 일시 중지 되지 않으므로 사용자에 게 훨씬 더 두드러집니다.

    둘째, 프로세스의 메모리 요구 사항이 너무 커지면 Android에서 해당 프로세스를 종료 하 여 장치에 대 한 리소스를 확보할 수 있습니다. 서비스의 메모리 공간이 크고 UI와 동일한 프로세스에서 실행 되는 경우 Android가 해당 리소스를 강제로 회수할 때 UI가 종료 되 고 사용자가 앱을 시작 하 게 됩니다. 그러나 자체 프로세스에서 실행 되는 서비스가 Android에서 종료 되는 경우 UI 프로세스는 영향을 받지 않습니다. UI는 사용자에 게 투명 하 게 서비스를 바인딩하고 다시 시작 하 고 정상적인 기능을 다시 시작할 수 있습니다.

3. **응용 프로그램 성능 향상** &ndash; 서비스 프로세스와 별개로 UI 프로세스가 종료 되거나 종료 될 수 있습니다. 시간이 오래 걸리는 시작 작업을 out-of-process 서비스로 이동 하면 UI 시작 시간이 향상 될 수 있습니다 (서비스 프로세스가 UI 시작 시간 사이에 활성 상태로 유지 되는 것으로 가정).

여러 가지 방법으로 다른 프로세스에서 실행 되는 서비스에 바인딩하는 것은 [로컬 서비스에 바인딩하](~/android/app-fundamentals/services/creating-a-service/bound-services.md)는 것과 같습니다. 클라이언트는를 호출 `BindService` 하 여 서비스를 바인딩하고 필요한 경우 시작 합니다. 클라이언트와 서비스 간의 연결을 관리 하기 위해 개체가생성됩니다.`Android.OS.IServiceConnection` 클라이언트에서 서비스에 성공적으로 바인딩한 경우 Android는 서비스에서 메서드를 호출 하는 `IServiceConnection` 데 사용할 수 있는을 통해 개체를 반환 합니다. 그러면 클라이언트는이 개체를 사용 하 여 서비스와 상호 작용 합니다. 검토 하려면 서비스에 바인딩하는 단계는 다음과 같습니다.

- **의도 만들기** &ndash; 서비스에 바인딩하려면 명시적 의도를 사용 해야 합니다.
- **`IServiceConnection`**  클라이언트와 서비스 간의 중개자 역할 `IServiceConnection` 을 하 &ndash; 는 개체를 구현 하 고 인스턴스화합니다.  클라이언트와 서버 간의 연결을 모니터링 하는 일을 담당 합니다.
- **`BindService`**  을 &ndash; 호출 하는메서드를호출하면이전단계에서만든의도및서비스연결이Android로발송되며,이는서비스를시작하고통신을설정하는데사용됩니다.`BindService` 클라이언트 및 서비스.

프로세스 경계를 교차 하지 않아도 되는 추가 복잡성이 도입 됩니다. 통신은 단방향 (클라이언트에서 서버로)이 고 클라이언트는 서비스 클래스에서 메서드를 직접 호출할 수 없습니다. 서비스가 클라이언트와 동일한 프로세스를 실행 하는 경우 Android는 양방향 통신을 허용할 수 `IBinder` 있는 개체를 제공 합니다. 이는 서비스가 자체 프로세스에서 실행 되는 경우에는 그렇지 않습니다. 클라이언트는 `Android.OS.Messenger` 클래스의 도움말을 사용 하 여 원격 서비스와 통신 합니다.

클라이언트가 원격 서비스에 바인딩하려를 요청 하면 Android에서 `Service.OnBind` 수명 주기 메서드를 호출 하며,이 메서드는로 `Messenger`캡슐화 된 `IBinder` 내부 개체를 반환 합니다. 는 `Messenger` Android SDK에서 제공 하는 특수 `IBinder` 한 구현에 대 한 씬 래퍼입니다. 는 `Messenger` 서로 다른 두 프로세스 간의 통신을 담당 합니다. 개발자는 메시지를 직렬화 하 고 프로세스 경계를 넘어 메시지를 마샬링 한 다음 클라이언트에서 deserialize 하는 방법에 대해 걱정할 필요가 없습니다. 이 작업은 개체에 `Messenger` 의해 처리 됩니다. 이 다이어그램에서는 클라이언트가 out-of-process 서비스에 대 한 바인딩을 시작할 때 수반 되는 클라이언트 쪽 Android 구성 요소를 보여 줍니다.

![클라이언트에서 서비스에 바인딩하는 단계 및 구성 요소를 보여 주는 다이어그램](out-of-process-services-images/ipc-01.png "서비스에 대 한 클라이언트 바인딩에 대 한 단계 및 구성 요소를 보여 주는 다이어그램입니다.")

원격 프로세스의 클래스는 로컬 프로세스의 바인딩된 서비스에서 통과 하는 것과 관련 된 많은 api가 동일한 수명 주기 콜백을 거칩니다. `Service` `Service.OnCreate`는를 `Handler` 초기화 하 고 개체에 `Messenger` 삽입 하는 데 사용 됩니다. 마찬가지로가 재정의 되지만 `IBinder` 개체를 반환 하는 대신 서비스 `Messenger`에서을 반환 합니다. `OnBind`  이 다이어그램은 클라이언트가 바인딩하는 경우 서비스에서 발생 하는 상황을 보여 줍니다.

![원격 클라이언트에서 바인딩할 때 서비스가 거치는 단계 및 구성 요소를 보여 주는 다이어그램](out-of-process-services-images/ipc-02.png "원격 클라이언트에서 바인딩할 때 서비스가 거치는 단계 및 구성 요소를 보여 주는 다이어그램입니다.")

는 `Message` 서비스에서 수신 될 때의 `Android.OS.Handler`인스턴스를 통해 처리 됩니다. 서비스는 메서드를 `Handler` `HandleMessage` 재정의 해야 하는 자체 서브 클래스를 구현 합니다. 이 메서드는에서 호출 `Messenger` 되며를 `Message` 매개 변수로 받습니다. 은 `Handler` 메타 데이터를 `Message` 검사 하 고 해당 정보를 사용 하 여 서비스에서 메서드를 호출 합니다.

단방향 통신은 클라이언트에서 개체를 `Message` 만들고 메서드를 `Messenger.Send` 사용 하 여 서비스에 디스패치할 때 발생 합니다. `Messenger.Send`는 직렬화 된 `Message` 데이터를 Android로 serialize 하 고, 프로세스 경계 및 서비스에서 메시지를 라우팅합니다.  서비스 `Messenger` 에서 호스트 되는는 들어오는 데이터에서 개체 `Message` 를 만듭니다. 이 `Message` 는 메시지가 한 번 `Handler`에 하나씩 전송 되는 큐에 배치 됩니다. 는에 포함 `Message` 된 메타 데이터를 검사 하 고에서 적절 한 메서드 `Service`를 호출 합니다. `Handler` 다음 다이어그램은 이러한 다양 한 개념의 동작을 보여 줍니다.

![메시지를 프로세스 간에 전달 하는 방법을 보여 주는 다이어그램](out-of-process-services-images/ipc-03.png "메시지를 프로세스 간에 전달 하는 방법을 보여 주는 다이어그램")

이 가이드에서는 out-of-process 서비스를 구현 하는 방법에 대해 자세히 설명 합니다. 자체 프로세스에서 실행 되는 서비스를 구현 하는 방법과 클라이언트에서 프레임 워크를 `Messenger` 사용 하 여 해당 서비스와 통신할 수 있는 방법을 설명 합니다. 또한 서비스에 메시지를 보내는 클라이언트와 메시지를 다시 클라이언트로 보내는 서비스에 메시지를 보내는 양방향 통신에 대해서도 간략하게 설명 합니다. 서비스는 서로 다른 응용 프로그램 간에 공유할 수 있기 때문에이 가이드에서는 Android 권한을 사용 하 여 서비스에 대 한 클라이언트 액세스를 제한 하는 한 가지 방법에 대해서도 설명 합니다.

> [!IMPORTANT]
> [Bugzilla 51940/GitHub 1950-격리 된 프로세스 및 사용자 지정 응용 프로그램 클래스가 포함 된 서비스](https://github.com/xamarin/xamarin-android/issues/1950) 는 `IsolatedProcess` 가로 `true`설정 된 경우 xamarin.ios 서비스가 제대로 시작 되지 않도록 하는 오버 로드를 올바르게 보고 하지 않습니다. 이 가이드는 참조용으로 제공 됩니다. Xamarin Android 응용 프로그램은 여전히 Java로 작성 된 out-of-process 서비스와 통신할 수 있어야 합니다.

## <a name="requirements"></a>요구 사항

이 가이드에서는 서비스 만들기에 대해 잘 알고 있는 것으로 가정 합니다.

이전 Android Api를 대상으로 하는 앱에서 암시적 의도를 사용할 수 있지만이 가이드는 명시적 의도를 사용 하는 경우에만 중점적으로 설명 합니다. Android 5.0 (API 레벨 21) 이상을 대상으로 하는 앱은 명시적 의도를 사용 하 여 서비스와 바인딩해야 합니다. 이는이 가이드에서 설명 하는 기술입니다.

## <a name="create-a-service-that-runs-in-a-separate-process"></a>별도의 프로세스에서 실행 되는 서비스 만들기

위에서 설명한 것 처럼 서비스가 자체 프로세스에서 실행 되는 사실은 몇 가지 다른 Api가 포함 되어 있음을 의미 합니다. 간단히 설명 하면 원격 서비스를 사용 하 고 사용 하는 단계는 다음과 같습니다.  

- **하위 클래스 `Service`**  서브&ndash; 클래스`Service` 를 만들고 바인딩된 서비스의 수명 주기 메서드를 구현 합니다. 또한 Android에서 서비스가 자체 프로세스에서 실행 되 고 있음을 알리는 메타 데이터를 설정 해야 합니다.
- 을 구현 하 여`Handler`클라이언트 요청을 분석 하 고, 클라이언트에서 전달 된 매개 변수를 추출 하 고, 서비스에서 적절 한 메서드를 호출 합니다. **`Handler`** &ndash;
- 위에서 설명한 것 처럼 `Service` **를 인스턴스화하면 `Messenger`**  &ndash; `Handler`각는 클라이언트 요청을 이전 단계에서 만든로 라우팅하는 클래스의인스턴스를유지해야합니다.`Messenger`

자체 프로세스에서 실행 되는 서비스는 근본적으로 여전히 바인딩된 서비스입니다. Service 클래스는 기본 `Service` 클래스를 확장 하 고 android에서 android 매니페스트에 번들 해야 하는 메타 데이터를 `ServiceAttribute` 포함 하는로 데코 레이트 됩니다. 먼저 out-of-process 서비스에 중요 한의 `ServiceAttribute` 다음 속성을 사용 합니다.

1. `Exported`다른 응용 프로그램에서 서비스와 `true` 상호 작용할 수 있도록 하려면이 속성을로 설정 해야 합니다. &ndash; 이 속성의 기본값은 `false`입니다.
2. `Process`&ndash; 이 속성을 설정 해야 합니다. 서비스가 실행 될 프로세스의 이름을 지정 하는 데 사용 됩니다.
3. `IsolatedProcess`&ndash; 이 속성을 사용 하면 추가 보안이 가능 하며, Android에서 시스템의 나머지 부분과 상호 작용 하는 데 필요한 최소한의 권한을 가진 격리 된 샌드박스에서 서비스를 실행 하도록 지시 합니다. [Bugzilla 51940-격리 된 프로세스의 서비스 및 사용자 지정 응용 프로그램 클래스가 오버 로드를 제대로 해결 하지 못했습니다 .를](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)참조 하세요.
4. `Permission`&ndash; 클라이언트에서 요청 하 고 권한을 부여 해야 하는 사용 권한을 지정 하 여 서비스에 대 한 클라이언트 액세스를 제어할 수 있습니다.

서비스 자체 프로세스 `Process` 를 실행 하려면의 속성 `ServiceAttribute` 을 서비스의 이름으로 설정 해야 합니다. 외부 응용 프로그램과 `Exported` 상호 작용 하려면 속성을로 `true`설정 해야 합니다. `Exported` 가`false`이면 동일한 apk (즉, 동일한 응용 프로그램)의 클라이언트만 서비스와 상호 작용할 수 있습니다.

서비스가 실행 되는 프로세스의 종류는 `Process` 속성의 값에 따라 달라 집니다. Android는 다음과 같은 세 가지 유형의 프로세스를 식별 합니다.

- **개인 프로세스** &ndash; 개인 프로세스는 해당 프로세스를 시작한 응용 프로그램에만 사용할 수 있는 프로세스입니다. 프로세스를 개인으로 식별 하려면 해당 이름이 (세미콜론)로 시작 해야 합니다. 이전 코드 조각과 스크린샷에 표시 된 서비스는 개인 프로세스입니다. 다음 코드 조각은의 `ServiceAttribute`예제입니다.

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process=":timestampservice_process",
             Exported=true)]
    ```

- **글로벌 프로세스** &ndash; 전역 프로세스에서 실행 되는 서비스는 장치에서 실행 되는 모든 응용 프로그램에 액세스할 수 있습니다. 전역 프로세스는 소문자로 시작 하는 정규화 된 클래스 이름 이어야 합니다.
    서비스를 보호 하기 위해 단계를 수행 하지 않으면 다른 응용 프로그램에서 해당 서비스를 바인딩하고 상호 작용할 수 있습니다. 무단 사용 으로부터 서비스를 보호 하는 방법에 대해서는이 가이드의 뒷부분에서 설명 합니다.

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

- **격리 된 프로세스** &ndash; 격리 된 프로세스는 자체 샌드박스에서 실행 되는 프로세스로, 시스템의 나머지 부분에서 분리 되며 별도의 특별 한 사용 권한은 없습니다. 격리 된 프로세스 `IsolatedProcess` 에서 서비스를 실행 하기 위해 `ServiceAttribute` 의 속성은 다음 코드 조각에 `true` 표시 된 것 처럼로 설정 됩니다.
    
    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             IsolatedProcess= true,
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

> [!IMPORTANT]
> [Bugzilla 51940-격리 된 프로세스의 서비스 및 사용자 지정 응용 프로그램 클래스가 오버 로드를 제대로 해결 하지 못함](https://bugzilla.xamarin.com/show_bug.cgi?id=51940) 참조

격리 된 서비스는 신뢰할 수 없는 코드 로부터 응용 프로그램 및 장치를 보호 하는 간단한 방법입니다. 예를 들어 앱은 웹 사이트에서 스크립트를 다운로드 하 여 실행할 수 있습니다. 이 경우 격리 된 프로세스에서이 작업을 수행 하면 신뢰할 수 없는 코드에 대 한 추가 보안 계층을 제공 하 여 Android 장치를 손상 시킬 수 있습니다.

> [!IMPORTANT]
> 서비스를 내보낸 후에는 서비스의 이름을 변경 하면 안 됩니다. 서비스의 이름을 변경 하면 서비스를 사용 하는 다른 응용 프로그램이 중단 될 수 있습니다.

속성의 `Process` 영향을 확인 하기 위해 다음 스크린샷에서는 자체 개인 프로세스에서 실행 되는 서비스를 보여 줍니다.

![개인 프로세스에서 실행 되는 서비스를 보여 주는 스크린샷](out-of-process-services-images/ipc-04.png "개인 프로세스에서 실행 되는 서비스를 보여 주는 스크린샷")

다음 스크린샷에서는 글로벌 `Process="com.xamarin.xample.messengerservice.timestampservice_process"` 프로세스에서 실행 되는 및 서비스를 보여 줍니다.

![전역 프로세스에서 실행 되는 서비스의 스크린샷](out-of-process-services-images/ipc-05.png "전역 프로세스에서 실행 되는 서비스의 스크린샷")

가 설정 되 면 서비스에서를 `Handler`구현 해야 합니다. `ServiceAttribute`

### <a name="implementing-a-handler"></a>처리기 구현

클라이언트 요청을 처리 하려면 서비스에서를 `Handler` 구현 하 고 메서드를 `HandleMessage` 재정의 해야 합니다. 메서드는 클라이언트에서 메서드 호출을 캡슐화 하 고 해당 호출을 일부 작업 또는 작업으로 변환 하는 인스턴스를 `Message` 사용 합니다. 서비스가 수행 하는입니다. 개체 `Message` 는 정수 값인 라는 `What` 속성을 노출 하 고, 클라이언트와 서비스 간에 공유 되는의 의미를 제공 하며, 서비스에서 클라이언트에 대해 수행할 작업에 대 한 의미를 제공 합니다.

예제 응용 프로그램의 다음 코드 조각에서는의 `HandleMessage`한 예를 보여 줍니다. 이 예제에는 클라이언트가 서비스를 요청할 수 있는 두 가지 작업이 있습니다.

- 첫 번째 작업은 _Hello, 세계_ 메시지입니다. 클라이언트는 서비스에 간단한 메시지를 보냈습니다.
- 두 번째 작업은 서비스에서 메서드를 호출 하 고 문자열을 검색 합니다 .이 경우 문자열은 서비스가 시작 된 시간과 실행 시간을 반환 하는 메시지입니다.

```csharp
public class TimestampRequestHandler : Android.OS.Handler
{
    // other code omitted for clarity

    public override void HandleMessage(Message msg)
    {
        int messageType = msg.What;
        Log.Debug(TAG, $"Message type: {messageType}.");

        switch (messageType)
        {
            case Constants.SAY_HELLO_TO_TIMESTAMP_SERVICE:
                // The client as sent a simple Hello, say in the Android Log.
                break;

            case Constants.GET_UTC_TIMESTAMP:
                // Call methods on the service to retrieve a timestamp message.
                break;
            default:
                Log.Warn(TAG, $"Unknown messageType, ignoring the value {messageType}.");
                base.HandleMessage(msg);
                break;
        }
    }
}
```

또한에서 `Message`서비스에 대 한 매개 변수를 패키지할 수 있습니다. 이에 대해서는이 가이드의 뒷부분에서 설명 합니다. 고려해 야 할 다음 항목에서는 들어오는 `Messenger` `Message`를 처리 하는 개체를 만듭니다.

### <a name="instantiating-the-messenger"></a>메신저 인스턴스화

앞에서 설명한 것 처럼 `Message` 개체를 deserialize 하 고를 호출 `Handler.HandleMessage` 하는 `Messenger` 것은 개체의 책임입니다. 또한 `Messenger` 클래스는 클라이언트에서 `IBinder` 서비스에 메시지를 보내는 데 사용 하는 개체를 제공 합니다.  

서비스가 시작 되 면를 인스턴스화하고 `Messenger` 를 `Handler`삽입 합니다. 이 초기화 `OnCreate` 를 수행 하는 좋은 장소는 서비스의 메서드입니다. 이 코드 조각은 자체 `Handler` 및를 `Messenger`초기화 하는 서비스의 한 예입니다.

```csharp
private Messenger messenger; // Instance variable for the Messenger

public override void OnCreate()
{
    base.OnCreate();
    messenger = new Messenger(new TimestampRequestHandler(this));
    Log.Info(TAG, $"TimestampService is running in process id {Android.OS.Process.MyPid()}.");
}
```

이 시점에서 마지막 단계 `Service` 는를 재정의 `OnBind`하는입니다.

### <a name="implementing-serviceonbind"></a>서비스. OnBind 구현

자체 프로세스에서 실행 되는지 여부에 관계 없이 모든 바인딩된 서비스는 메서드를 `OnBind` 구현 해야 합니다. 이 메서드의 반환 값은 클라이언트에서 서비스와 상호 작용 하는 데 사용할 수 있는 개체입니다. 서비스가 로컬 서비스 인지 또는 원격 서비스 인지에 관계 없이 해당 개체는 정확히 달라 집니다. 로컬 서비스는 사용자 지정 `IBinder` 구현을 반환 하지만, 원격 서비스는 캡슐화 되어 있지만 `Messenger` 이전 `IBinder` 섹션에서 만든를 반환 합니다.

```csharp
public override IBinder OnBind(Intent intent)
{
    Log.Debug(TAG, "OnBind");
    return messenger.Binder;
}
```

이러한 세 단계가 완료 되 면 원격 서비스가 완료 된 것으로 간주 될 수 있습니다.

## <a name="consuming-the-service"></a>서비스 사용

모든 클라이언트는 원격 서비스를 바인딩하고 사용할 수 있도록 일부 코드를 구현 해야 합니다. 개념적으로 클라이언트의 관점에서 로컬 서비스 또는 원격 서비스에 바인딩하는 것 사이에는 약간의 차이가 있습니다. 클라이언트는 메서드를 `BindService` 호출 하 여 서비스를 식별 하는 명시적 의도를 전달 `IServiceConnection` 하 고 클라이언트와 서비스 간의 연결을 관리 하는 데 도움이 되는를 전달 합니다.

이 코드 조각은 원격 서비스에 바인딩하기 위한 **명시적 의도** 를 만드는 방법의 예입니다. 의도는 서비스 및 서비스 이름을 포함 하는 패키지를 식별 해야 합니다. 이 정보를 설정 하는 한 가지 방법은 `Android.Content.ComponentName` 개체를 사용 하 고이를 의도에 제공 하는 것입니다. 이 코드 조각은 한 가지 예입니다.  

```csharp
// This is the package name of the APK, set in the Android manifest
const string REMOTE_SERVICE_COMPONENT_NAME = "com.xamarin.TimestampService";
// This is the name of the service, according the value of ServiceAttribute.Name
const string REMOTE_SERVICE_PACKAGE_NAME   = "com.xamarin.xample.messengerservice";

// Provide the package name and the name of the service with a ComponentName object.
ComponentName cn = new ComponentName(REMOTE_SERVICE_PACKAGE_NAME, REMOTE_SERVICE_COMPONENT_NAME);
Intent serviceToStart = new Intent();
serviceToStart.SetComponent(cn);
```

서비스가 바인딩되면 `IServiceConnection.OnServiceConnected` 메서드가 호출 되 고가 클라이언트 `IBinder` 에 제공 됩니다. 그러나 클라이언트는를 `IBinder`직접 사용 하지 않습니다. 대신 개체 `Messenger` `IBinder`를 인스턴스화합니다. 이는 클라이언트가 원격 서비스와 상호 작용 하는 데 사용 하는입니다.`Messenger`

다음은 클라이언트에서 서비스 연결 및 연결 해제 `IServiceConnection` 를 처리 하는 방법을 보여 주는 매우 기본적인 구현의 예입니다. 메서드는 `OnServiceConnected` 및 `IBinder`를 받고 `Messenger` ,`IBinder`클라이언트는 다음 위치에서를 만듭니다.

```csharp
public class TimestampServiceConnection : Java.Lang.Object, IServiceConnection
{
    static readonly string TAG = typeof(TimestampServiceConnection).FullName;

    MainActivity mainActivity;
    Messenger messenger;

    public TimestampServiceConnection(MainActivity activity)
    {
        IsConnected = false;
        mainActivity = activity;
    }

    public bool IsConnected { get; private set; }
    public Messenger Messenger { get; private set; }

    public void OnServiceConnected(ComponentName name, IBinder service)
    {
        Log.Debug(TAG, $"OnServiceConnected {name.ClassName}");

        IsConnected = service != null
        Messenger = new Messenger(service);

        if (IsConnected)
        {
            // things to do when the connection is successful. perhaps notify the client? enable UI features?
        }
        else
        {
            // things to do when the connection isn't successful.
        }
    }

    public void OnServiceDisconnected(ComponentName name)
    {
        Log.Debug(TAG, $"OnServiceDisconnected {name.ClassName}");
        IsConnected = false;
        Messenger = null;

        // Things to do when the service disconnects. perhaps notify the client? disable UI features?

    }
}
```

서비스 연결 및 의도가 만들어지면 클라이언트는를 호출 `BindService` 하 고 바인딩 프로세스를 시작할 수 있습니다.

```csharp
IServiceConnection serviceConnection = new TimestampServiceConnection(this);
BindActivity(serviceToStart, serviceConnection, Bind.AutoCreate);
```

클라이언트를 서비스 `Messenger` 에 성공적으로 바인딩한 후를 사용할 수 있게 되 면 클라이언트에서 서비스로 보낼 `Messages` 수 있습니다.

## <a name="sending-messages-to-the-service"></a>서비스에 메시지 보내기

클라이언트가 연결 되어 있고 `Messenger` 개체가 있는 경우을 `Messenger`통해 개체를 디스패치 `Message` 하 여 서비스와 통신할 수 있습니다. 이 통신은 단방향 이며 클라이언트는 메시지를 보내지만 서비스에서 클라이언트로의 반환 메시지는 없습니다. 이와 관련 하 여은 `Message` 화재 및 잊은 메커니즘입니다.

개체를 `Message` 만드는 기본 방법은 [`Message.Obtain`](xref:Android.OS.Message) 팩터리 메서드를 사용 하는 것입니다. 이 메서드는 Android에서 `Message` 유지 관리 되는 전역 풀에서 개체를 가져옵니다. `Message.Obtain`에는 서비스에 필요한 값 및 매개 `Message` 변수를 사용 하 여 개체를 초기화할 수 있도록 하는 오버 로드 된 메서드가 포함 되어 있습니다.  이 인스턴스화된 `Message` 후에는을 호출 `Messenger.Send`하 여 서비스로 발송 됩니다. 이 코드 조각은를 `Message` 만들어 서비스 프로세스에 디스패치 하는 한 가지 예입니다.

```csharp
Message msg = Message.Obtain(null, Constants.SAY_HELLO_TO_TIMESTAMP_SERVICE);
try
{
    serviceConnection.Messenger.Send(msg);
}
catch (RemoteException ex)
{
    Log.Error(TAG, ex, "There was a error trying to send the message.");
}
```

`Message.Obtain` 메서드에는 여러 가지 형식이 있습니다. 이전 예제에서는를 [`Message.Obtain(Handler h, Int32 what)`](xref:Android.OS.Message.Obtain)사용 합니다. 이는 out-of-process 서비스에 대 한 비동기 요청 이기 때문입니다. 서비스 `Handler` 에서 응답 하지 않으므로가로 `null`설정 됩니다. 두 번째 매개 변수 `Int32 what`는 `Message` 개체의 `.What` 속성에 저장 됩니다. `.What` 속성은 서비스 프로세스의 코드에서 서비스의 메서드를 호출 하는 데 사용 됩니다.

또한 `Message` 클래스는 `Arg1` 받는 사람에 사용 될 수 있는 두 가지 추가 속성인 및 `Arg2`를 노출 합니다. 이러한 두 속성은 클라이언트와 서비스 간에 의미가 있는 값에 대 한 몇 가지 특별 한 합의를 가질 수 있는 정수 값입니다. 예 `Arg1` 를 들어는 고객 ID를 보유할 수 `Arg2` 있으며 해당 고객의 구매 주문 번호를 보유할 수 있습니다. 는를 만들 `Message` 때 두 속성을 설정 하는 데 사용할 [수있습니다.`Method.Obtain(Handler h, Int32 what, Int32 arg1, Int32 arg2)`](xref:Android.OS.Message.Obtain) 이러한 두 값을 채우는 또 다른 방법은 `.Arg` `Message` 개체를 만든 후 `.Arg2` 개체에서 직접 및 속성을 설정 하는 것입니다.

### <a name="passing-additional-values-to-the-service"></a>서비스에 추가 값 전달

을 `Bundle`사용 하 여 더 복잡 한 데이터를 서비스에 전달할 수 있습니다. 이 경우에는를 `Bundle` 보내기 전에 [ `.Data` 속성](xref:Android.OS.Message.Data) 속성을 설정 `Message` 하 여 추가 값을에 배치 하 고와 함께 보낼 수 있습니다.

```csharp
Bundle serviceParameters = new Bundle();
serviceParameters.

var msg = Message.Obtain(null, Constants.SERVICE_TASK_TO_PERFORM);
msg.Data = serviceParameters;

messenger.Send(msg);
```


> [!NOTE]
> 일반적으로에는 `Message` 1mb 보다 큰 페이로드가 없어야 합니다. 크기 제한은 Android 버전에 따라 다르며, 공급 업체에서 장치와 함께 제공 되는 AOSP (Android 오픈 소스 프로젝트)의 구현에 대해 수행할 수 있는 모든 소유 변경 내용에 따라 달라질 수 있습니다.

## <a name="returning-values-from-the-service"></a>서비스에서 값 반환

이 시점에 설명 된 메시징 아키텍처는 단방향 이며, 클라이언트는 서비스에 메시지를 보냅니다. 서비스가 클라이언트에 값을 반환 해야 하는 경우이 지점에 대해 설명한 모든 것이 반대입니다. 서비스는을 `Message`(를) 만들고, 반환 값을 패키지 하 고 `Message` ,를 `Messenger` 통해를 클라이언트에 디스패치합니다. 그러나 서비스는 자체 `Messenger`를 만들지는 않으며, 대신 초기 요청의 일부로를 `Messenger` 인스턴스화하고 패키지 하는 클라이언트에 의존 합니다. 서비스 `Send` 는이 클라이언트를 사용 하 여 메시지를 `Messenger`제공 합니다.  

양방향 통신에 대 한 이벤트 시퀀스는 다음과 같습니다.

1. 클라이언트는 서비스에 바인딩됩니다. 서비스와 클라이언트가 연결 되 면 클라이언트에 의해 `IServiceConnection` 유지 관리 되는에는를 서비스에 전송 `Message`하는 `Messenger` 데 사용 되는 개체에 대 한 참조가 포함 됩니다. 혼동을 피하기 위해이를 _서비스 Messenger_라고 합니다.
2. 클라이언트는 `Handler` ( _클라이언트 처리기_라고 함)를 인스턴스화하고이를 사용 하 여 자체 `Messenger` ( _클라이언트 Messenger_)를 초기화 합니다. 서비스 Messenger와 클라이언트 Messenger는 서로 다른 두 방향으로 트래픽을 처리 하는 두 가지 다른 개체입니다. 서비스 Messenger는 클라이언트에서 서비스로 메시지를 처리 하는 반면 클라이언트 Messenger는 서비스에서 클라이언트로 메시지를 처리 합니다.
3. 클라이언트에서 `Message` 개체를 만들고 클라이언트 Messenger를 사용 `ReplyTo` 하 여 속성을 설정 합니다. 그런 다음 서비스 Messenger를 사용 하 여 서비스에 메시지를 보냅니다.
4. 서비스는 클라이언트에서 메시지를 수신 하 고 요청 된 작업을 수행 합니다.
5. 서비스가 클라이언트에 응답을 보낼 때 시간을 사용 `Message.Obtain` 하 여 새 `Message` 개체를 만듭니다.
6. 클라이언트에이 메시지를 보내기 위해 서비스는 클라이언트 메시지의 `.ReplyTo` 속성에서 클라이언트 Messenger를 추출 하 고이를 클라이언트에 게 `Message` 다시 `.Send` 사용 합니다.
7. 클라이언트에서 응답을 수신 하 `Handler` 는 경우 `.What` 속성을 검사 하 여를 `Message` 처리 하는 자체를 가집니다 (필요한 경우에 `Message`포함 된 매개 변수를 추출).

이 샘플 코드는 클라이언트가를 `Message` 인스턴스화하고 서비스에서 응답에 사용 해야 하는을 `Messenger` 패키지 하는 방법을 보여 줍니다.

```csharp
Handler clientHandler = new ActivityHandler();
Messenger clientMessenger = new Messenger(activityHandler);

Message msg = Message.Obtain(null, Constants.GET_UTC_TIMESTAMP);
msg.ReplyTo = clientMessenger;

try
{
    serviceConnection.Messenger.Send(msg);
}
catch (RemoteException ex)
{
    Log.Error(TAG, ex, "There was a problem sending the message.");
}
```

서비스는를 `Messenger` 추출 하 여를 추출 하 `Handler` 고이를 사용 하 여 클라이언트에 회신을 보내는 작업을 수행 해야 합니다. 이 코드 조각은 서비스의 `Handler` 에서를 `Message` 만들고 클라이언트에 다시 보내는 방법의 예입니다.  

```csharp
// This is the message that the service will send to the client.
Message responseMessage = Message.Obtain(null, Constants.RESPONSE_TO_SERVICE);
Bundle dataToReturn = new Bundle();
dataToReturn.PutString(Constants.RESPONSE_MESSAGE_KEY, "This is the result from the service.");
responseMessage.Data = dataToReturn;

// The msg object here is the message that was received by the service. The service will not instantiate a client,
// It will use the client that is encapsulated by the message from the client.
Messenger clientMessenger = msg.ReplyTo;
if (clientMessenger!= null)
{
    try
    {
        clientMessenger.Send(responseMessage);
    }
    catch (Exception ex)
    {
        Log.Error(TAG, ex, "There was a problem sending the message.");
    }
}
```

위의 `Messenger` 코드 샘플에서 클라이언트에 의해 생성 된 인스턴스는 서비스에서 받는 것과 동일한 개체가 *아닙니다* . 이러한 두 가지 `Messenger` 개체는 통신 채널을 나타내는 두 개의 개별 프로세스로 실행 됩니다.

## <a name="securing-the-service-with-android-permissions"></a>Android 권한으로 서비스 보안 설정

전역 프로세스에서 실행 되는 서비스는 해당 Android 장치에서 실행 되는 모든 응용 프로그램에서 액세스할 수 있습니다. 경우에 따라이 개방성 및 가용성은 바람직하지 않으며 권한이 없는 클라이언트의 액세스 로부터 서비스의 보안을 유지 해야 합니다. 원격 서비스에 대 한 액세스를 제한 하는 한 가지 방법은 Android 권한을 사용 하는 것입니다.

하위 클래스를 `Permission` `ServiceAttribute` 데코레이팅하는의속성을사용하여권한을`Service` 식별할 수 있습니다. 이렇게 하면 서비스에 바인딩할 때 클라이언트에 부여 되어야 하는 사용 권한의 이름을 지정할 수 있습니다. 클라이언트에 적절 한 권한이 없으면 클라이언트에서 서비스에 바인딩하려 할 때 Android에서 `Java.Lang.SecurityException` 을 throw 합니다.

Android에서 제공 하는 네 가지 권한 수준은 다음과 같습니다.

- **보통** &ndash; 이는 기본 권한 수준입니다. Android에서 요청 하는 클라이언트에 자동으로 부여할 수 있는 낮은 위험 권한을 식별 하는 데 사용 됩니다. 사용자는 이러한 권한을 명시적으로 부여 하지 않아도 되지만 앱 설정에서 사용 권한을 볼 수 있습니다.
- **서명** &ndash; 이는 Android에서 동일한 인증서로 서명 된 응용 프로그램에 자동으로 부여 되는 특별 한 범주의 권한입니다. 이 사용 권한은 응용 프로그램 개발자가 사용자를 일정 하 게 승인 하기 위해 신경 하지 않고도 앱 간에 구성 요소나 데이터를 쉽게 공유할 수 있도록 설계 되었습니다.
- **서명** 이는 위에서 설명한 서명 권한과 매우 유사 합니다. &ndash; 동일한 인증서로 서명 된 앱에 자동으로 부여 되는 것 외에도 Android 시스템 이미지를 사용 하 여 설치 된 앱에 서명 하는 데 사용 된 것과 동일한 인증서에 서명 된 앱에도이 권한이 부여 됩니다. 이 권한은 일반적으로 Android ROM 개발자가 응용 프로그램이 타사 앱에서 작동 하도록 허용 하는 데만 사용 됩니다. 일반적으로는 공용에 대 한 일반 배포를 위한 앱에서 일반적으로 사용 되지 않습니다.
- **위험** &ndash; 위험한 권한은 사용자에 게 문제가 발생할 수 있는 권한입니다. 따라서 **위험한** 사용 권한은 사용자가 명시적으로 승인 해야 합니다.

Android `signature` 에서는 `normal` 설치 된 시간에 및 사용 권한이 자동으로 부여 되므로 클라이언트를 포함 하는 apk **이전** 에 서비스를 호스트 하는 apk가 설치 되어 있어야 합니다. 클라이언트를 먼저 설치한 경우 Android에서 사용 권한을 부여 하지 않습니다. 이 경우 클라이언트 APK를 제거 하 고, 서비스 APK를 설치 하 고, 클라이언트 APK를 다시 설치 해야 합니다.

Android 권한으로 서비스를 보호 하는 일반적인 방법에는 다음 두 가지가 있습니다.

1. **서명 수준 보안 구현** &ndash; 서명 수준 보안은 서비스를 보유 하 고 있는 apk에 서명 하는 데 사용 된 것과 동일한 키로 서명 된 응용 프로그램에 권한이 자동으로 부여 됨을 의미 합니다. 이 방법은 개발자가 자신의 응용 프로그램에서 액세스할 수 있는 서비스를 보호 하는 간단한 방법입니다. 서명 수준 권한은 `Permission` `ServiceAttribute` 의 속성을로 `signature`설정 하 여 선언 됩니다.

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.TimestampService.timestampservice_process",
             Permission="signature")]
    public class TimestampService : Service
    {
    }
    ```

2. **사용자 지정 권한 만들기** &ndash; 서비스 개발자가 서비스에 대 한 사용자 지정 권한을 만들 수 있습니다. 개발자가 다른 개발자의 응용 프로그램과 서비스를 공유 하려는 경우에 가장 적합 합니다. 사용자 지정 권한을 구현 하려면 약간의 노력이 필요 하며 아래에서 설명 합니다.

사용자 지정 `normal` 권한 만들기에 대 한 간단한 예는 다음 섹션에서 설명 합니다. Android 사용 권한에 대 한 자세한 내용은 Google 설명서를 참조 하 여 [보안 & 모범 사례](https://developer.android.com/training/articles/security-tips.html)를 참조 하세요. Android 사용 권한에 대 한 자세한 내용은 android 사용 권한에 대 한 자세한 내용은 응용 프로그램 매니페스트에 대 한 Android 설명서의 [사용 권한 섹션](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms) 을 참조 하세요.

> [!NOTE]
> 일반적으로 Google은 사용자의 혼동을 입증할 수 있으므로 [사용자 지정 권한을 사용](https://developer.android.com/training/articles/security-tips.html#RequestingPermissions) 하지 않습니다.

### <a name="creating-a-custom-permission"></a>사용자 지정 권한 만들기

사용자 지정 권한을 사용 하기 위해 클라이언트는 해당 권한을 명시적으로 요청 하는 동안 서비스에 의해 선언 됩니다.

서비스 apk `permission` 에서 사용 권한을 만들려면 요소를 **androidmanifest .xml**의 `manifest` 요소에 추가 합니다. 이 사용 권한에는 `name`, `protectionLevel`및 `label` 특성이 설정 되어 있어야 합니다. 특성 `name` 은 사용 권한을 고유 하 게 식별 하는 문자열로 설정 해야 합니다. 이름은 다음 섹션과 같이 **Android 설정** 의 **앱 정보** 보기에 표시 됩니다.

특성 `protectionLevel` 은 위에서 설명한 4 개의 문자열 값 중 하나로 설정 해야 합니다.  `label` 및`description` 는 문자열 리소스를 참조 해야 하며 사용자에 게 친숙 한 이름 및 설명을 제공 하는 데 사용 됩니다.

이 코드 조각은 서비스를 포함 하는 apk의 **xml** 에서 사용자 지정 `permission` 특성을 선언 하는 예제입니다.

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          android:versionCode="1"
          android:versionName="1.0"
          package="com.xamarin.xample.messengerservice">

    <uses-sdk android:minSdkVersion="21" />

    <permission android:name="com.xamarin.xample.messengerservice.REQUEST_TIMESTAMP"
                android:protectionLevel="signature"
                android:label="@string/permission_label"
                android:description="@string/permission_description"
                />

    <application android:allowBackup="true"
            android:icon="@mipmap/icon"
            android:label="@string/app_name"
            android:theme="@style/AppTheme">

    </application>
</manifest>
```

그런 다음 클라이언트 APK의 **Androidmanifest** 은이 새 사용 권한을 명시적으로 요청 해야 합니다. 이렇게 하려면 `users-permission` **androidmanifest**에 특성을 추가 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          android:versionCode="1"
          android:versionName="1.0"
          package="com.xamarin.xample.messengerclient">

    <uses-sdk android:minSdkVersion="21" />

    <uses-permission android:name="com.xamarin.xample.messengerservice.REQUEST_TIMESTAMP" />

    <application
            android:allowBackup="true"
            android:icon="@mipmap/icon"
            android:label="@string/app_name"
            android:theme="@style/AppTheme">
    </application>
    </manifest>
```

### <a name="view-the-permissions-granted-to-an-app"></a>앱에 부여 된 사용 권한 보기

응용 프로그램에 부여 된 사용 권한을 보려면 Android 설정 앱을 열고 **앱**을 선택 합니다. 목록에서 응용 프로그램을 찾아 선택 합니다. **앱 정보** 화면에서 앱에 부여 된 모든 사용 권한을 표시 하는 보기를 표시 하는 **권한** 을 탭 합니다.

[![응용 프로그램에 부여 된 사용 권한을 찾는 방법을 보여주는 Android 장치의 스크린샷](out-of-process-services-images/ipc-06-sml.png)](out-of-process-services-images/ipc-06.png#lightbox)

## <a name="summary"></a>요약

이 가이드는 원격 프로세스에서 Android 서비스를 실행 하는 방법에 대 한 고급 설명입니다. 로컬 서비스와 원격 서비스 간의 차이점은 원격 서비스가 Android 앱의 안정성과 성능에 도움이 될 수 있는 몇 가지 이유를 설명 했습니다. 원격 서비스를 구현 하는 방법 및 클라이언트가 서비스와 통신 하는 방법에 대해 설명 하 고 나면 인증 된 클라이언트만 서비스에 대 한 액세스를 제한 하는 한 가지 방법을 제공 합니다.


## <a name="related-links"></a>관련 링크

- [Handler](xref:Android.OS.Handler)
- [메시지](xref:Android.OS.Message)
- [Messenger](xref:Android.OS.Messenger)
- [ServiceAttribute](xref:Android.App.ServiceAttribute)
- [내보낸 특성](https://developer.android.com/guide/topics/manifest/service-element.html#exported)
- [격리 된 프로세스 및 사용자 지정 응용 프로그램 클래스가 있는 서비스에서 오버 로드를 제대로 해결 하지 못함](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)
- [프로세스 및 스레드](https://developer.android.com/guide/components/processes-and-threads.html)
- [Android 매니페스트-사용 권한](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms)
- [보안 팁](https://developer.android.com/training/articles/security-tips.html)
- [MessengerServiceDemo (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-messengerservicedemo)
