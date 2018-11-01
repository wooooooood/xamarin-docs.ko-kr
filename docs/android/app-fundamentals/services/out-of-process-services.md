---
title: 원격 프로세스에서 실행 중인 Android 서비스
description: 일반적으로 Android 응용 프로그램의 모든 구성 요소는 동일한 프로세스에서 실행 됩니다. Android 서비스는 자체 프로세스에서 실행 되도록 구성 및 다른 Android 개발자를 포함 하 여 다른 응용 프로그램과 공유 수에 주목할 만한 예외는 있습니다. 이 가이드에서는 만들고 Xamarin을 사용 하 여 Android 원격 서비스를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 27A2E972-A690-480B-B31D-5EF1F74F673C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 8514d3b2c423e524d03a800f5f56359f3aee4b75
ms.sourcegitcommit: 650fd5813e243d67eea13c4bc76683c0f8134123
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50737195"
---
# <a name="running-android-services-in-remote-processes"></a>원격 프로세스에서 실행 중인 Android 서비스

_일반적으로 Android 응용 프로그램의 모든 구성 요소는 동일한 프로세스에서 실행 됩니다. Android 서비스는 자체 프로세스에서 실행 되도록 구성 및 다른 Android 개발자를 포함 하 여 다른 응용 프로그램과 공유 수에 주목할 만한 예외는 있습니다. 이 가이드에서는 만들고 Xamarin을 사용 하 여 Android 원격 서비스를 사용 하는 방법을 설명 합니다._

## <a name="out-of-process-services-overview"></a>서비스 개요

응용 프로그램이 시작 되 면 Android 응용 프로그램을 실행 하는 프로세스를 만듭니다. 일반적으로 모든 구성 요소가 응용 프로그램 프로세스에서 실행 됩니다이 하나 있습니다. Android 서비스는 자체 프로세스에서 실행 되도록 구성 및 다른 Android 개발자를 포함 하 여 다른 응용 프로그램과 공유 수에 주목할 만한 예외는 있습니다. 이러한 유형의 서비스 라고 _원격 서비스_ 하거나 _out of process 서비스_합니다. 이러한 서비스에 대 한 코드를 주 응용 프로그램으로 동일한 APK에 포함 됩니다. 그러나 서비스를 시작할 때 Android만 해당 서비스에 대 한 새 프로세스를 만듭니다. 반면, 응용 프로그램의 나머지 부분과 동일한 프로세스에서 실행 되는 서비스는 때때로 이라고 하는 _로컬 서비스_합니다.

일반적으로 원격 서비스를 구현 하는 응용 프로그램에 대 한 필요는 없습니다. 충분 한 (및 필요) 로컬 서비스는 대부분의 응용 프로그램의 요구 사항에 대 한 합니다. out of process 자체 메모리 공간 Android에서 관리 해야 하는 있습니다. 전체 응용 프로그램에 더 많은 오버 헤드가 생깁니다, 있지만 가지 자체 프로세스에서 서비스를 실행 하면 도움이 될 수 있는 몇 가지 시나리오가 있습니다.

1. **공유 기능** &ndash; 일부 응용 프로그램 개발자는 여러 앱 및 모든 응용 프로그램 간에 공유 되는 기능에 있을 수 있습니다. Android 서비스는 자체 프로세스에서 실행 되는 응용 프로그램 유지 관리를 단순화할 수에 해당 기능을 패키지 합니다. 독립 실행형 자체 APK에서 서비스 패키지 및 응용 프로그램의 나머지 부분에서 별도로 배포할 수 이기도 합니다.
2. **사용자 환경 개선** &ndash; 두 가지 가능한 out of process 서비스를 응용 프로그램의 사용자 환경을 향상 시킬 수 있습니다. 첫 번째 방법은 메모리 관리를 처리합니다. 가비지 수집 (GC) 주기 발생 하면 GC 완료 될 때까지 Android 프로세스에서 모든 작업은 일시 중지 합니다. 사용자는 "끊김" 또는 "jank"이 일시이 중지를 인식할 수 있습니다. 서비스에서 실행 중인 경우에 자체 프로세스를 응용 프로그램 프로세스가 아니라 서비스 프로세스는 일시 중지 된 것입니다. 응용 프로그램 프로세스 (및 따라서 사용자 인터페이스)가 일시 중지 하는 대로 사용자에 게 훨씬 덜 눈에 띄는이 일시 중지가 됩니다.

    둘째, 프로세스의 메모리 요구 사항을 너무 커지면, Android 장치에 대 한 리소스를 확보 하는 프로세스를 제거할 수 있습니다. 서비스는 큰 메모리 사용 공간을 없는 UI와 같은 프로세스에서 실행 중인 경우 다음 Android 강제로 해당 리소스를 회수할 때 UI는 종료할 수 되어 사용자가 앱을 시작 합니다. 그러나 자체 프로세스에서 실행 되는 서비스를 Android에서 종료 된 경우 UI 프로세스 영향을 받지 않고 남아 있습니다. UI 수 바인딩 (및 다시 시작) 서비스, 사용자 및 resume 일반 기능에 대해 투명 합니다.

3. **응용 프로그램 성능 향상** &ndash; UI 프로세스 종료 되었거나 독립 서비스 프로세스를 종료 합니다. 긴 시작 작업 out of process 서비스를 이동 하 여 있기 UI의 시작 시간 사용 (가정 서비스 프로세스 UI가 시작 되는 시간 사이 활성 상태로 유지) 개선 있을 수 있습니다.

다양 한 방법으로 다른 프로세스에서 실행 되는 서비스 바인딩 같습니다 [로컬 서비스에 바인딩](~/android/app-fundamentals/services/creating-a-service/bound-services.md)합니다. 클라이언트는 호출 `BindService` 바인딩 (및 필요한 경우 시작)를 서비스 합니다. `Android.OS.IServiceConnection` 클라이언트와 서비스 간의 연결을 관리 하는 개체를 만들 수는 있습니다. 클라이언트 서비스를 성공적으로 바인딩할 경우 Android를 통해 개체를 반환 합니다는 `IServiceConnection` 는 서비스에서 메서드를 호출할 수 있습니다. 클라이언트는 다음이 개체를 사용 하 여 서비스와 상호 작용 합니다. 를 검토 하려면 서비스에 바인딩할 단계는 다음과 같습니다.

* **의도 만듭니다** &ndash; 명시적 의도를 서비스에 바인딩을 사용 해야 합니다.
* **구현 및 인스턴스화를 `IServiceConnection` 개체** &ndash; 는 `IServiceConnection` 클라이언트와 서비스 간의 중간 단계로 사용 되는 개체입니다.  클라이언트와 서버 간의 연결 모니터링을 담당 합니다.
* **호출 된 `BindService` 메서드** &ndash; 호출 `BindService` 의도 및 서비스를 시작 및 통신을 설정 하며 Android에 대 한 이전 단계에서 만든 서비스 연결 발송 클라이언트와 서비스입니다.

프로세스 경계를 넘나들 필요가 추가 복잡성을 도입 합니다: 통신은 단방향 (클라이언트에서 서버로) 및 클라이언트 서비스 클래스의 메서드를 직접 호출할 수 없습니다. 서비스 클라이언트와 동일한 프로세스를 실행 중일 때 Android는 회수는 `IBinder` 개체 양방향 통신을 허용할 수 있습니다. 서비스가 자체 프로세스에서 실행 되 고 사용 하는 경우가 아닙니다. 클라이언트를 사용 하 여 원격 서비스와 통신 합니다 `Android.OS.Messenger` 클래스입니다.

Android 호출 클라이언트가 원격 서비스를 사용 하 여 바인딩할를 요청 하는 경우는 `Service.OnBind` 내부 반환 하는 수명 주기 메서드를 `IBinder` 캡슐화 된 개체는 `Messenger`합니다. 합니다 `Messenger` 특별 한 씬 래퍼를 스타일러스가 `IBinder` Android SDK에서 제공 하는 구현 합니다. `Messenger` 두 개의 서로 다른 프로세스 간에 통신을 처리 합니다. 개발자는 메시지를 직렬화 하는 작업, 프로세스 경계를 넘어 메시지 마샬링 및 클라이언트에서 역직렬화의 세부 정보를 사용 하 여 신경을 쓰지 않아도 됩니다. 이 작업에 의해 처리 됩니다는 `Messenger` 개체입니다. 이 다이어그램에서는 클라이언트 시작 out of process 서비스를 바인딩할 때 관련 되는 클라이언트 쪽 Android 구성 요소를 보여 줍니다.

![서비스에 바인딩 클라이언트에 대 한 단계 및 구성 요소를 보여 주는 다이어그램](out-of-process-services-images/ipc-01.png "서비스에 바인딩 클라이언트에 대 한 단계 및 구성 요소를 보여 주는 다이어그램입니다.")

`Service` 원격 프로세스의 클래스는 로컬 프로세스에 바인딩된 서비스가 경험한 동일한 수명 주기 콜백을 통해 이동 되 고 대부분의 관련 된 Api는 동일 합니다. `Service.OnCreate` 초기화 하는 데 사용 되는 `Handler` 에 삽입 및 `Messenger` 개체입니다. 마찬가지로 `OnBind` 은 재정의 반환 하는 대신는 `IBinder` 개체 서비스는 반환 된 `Messenger`.  클라이언트를 바인딩할 때 서비스에서 발생 하는 것이 다이어그램:

![원격 클라이언트에서 바인딩할 때 서비스 단계 및 구성 요소를 보여 주는 다이어그램 거칩니다](out-of-process-services-images/ipc-02.png "원격 클라이언트에서 바인딩할 때 서비스 단계 및 구성 요소를 보여 주는 다이어그램을 거칩니다.")

경우는 `Message` 수신 되는 서비스에서 처리 된 인스턴스의 `Android.OS.Handler`합니다. 서비스는 자체 구현 `Handler` 서브 클래스를 무시 해야 하는 `HandleMessage` 메서드. 이 메서드를 호출한 합니다 `Messenger` 받고는 `Message` 매개 변수로 합니다. `Handler` 는 조사를 `Message` 메타 데이터 서비스에서 메서드를 호출 하는 정보를 사용 하 여.

단방향 통신 클라이언트를 만들 때 발생 한 `Message` 개체를 사용 하 여 서비스에 디스패치를 `Messenger.Send` 메서드. `Messenger.Send` serialize 된 `Message` 및 프로세스 경계를 넘어 메시지가 라우팅되는 android 및 서비스 해제 데이터를 serialize 하는 직접.  `Messenger` 에서 호스팅되는 서비스 만들어집니다는 `Message` 들어오는 데이터의 개체입니다. 이렇게 `Message` 메시지를 한 번에 전송 된 하나씩 있는 큐에 배치 되는 `Handler`. `Handler` 에 포함 된 메타 데이터를 검사 하는 합니다 `Message` 에서 적절 한 메서드를 호출 하 고는 `Service`합니다. 다음 다이어그램은 작업에서 이러한 다양 한 개념을 보여 줍니다.

![프로세스 간 메시지 전달 하는 방법을 보여 주는 다이어그램](out-of-process-services-images/ipc-03.png "프로세스 간의 메시지 전달 하는 방법을 보여 주는 다이어그램입니다.")

이 가이드에서는 out-of-process-서비스를 구현 하는 세부 정보를 설명 합니다. 에서는 자체 프로세스에서 실행 해야 하는 서비스를 구현 하는 방법 및 클라이언트 사용 하 여 해당 서비스와 통신할 수 있습니다는 방법을 설명 합니다 `Messenger` 프레임 워크입니다. 양방향 통신 대해서도 간략하게 설명 합니다: 서비스 및 클라이언트에 다시 메시지를 보내는 서비스에 메시지를 보내는 클라이언트입니다. 다른 응용 프로그램 사이 서비스를 공유할 수 있으므로이 가이드에서는 Android 권한을 사용 하 여 서비스에 대 한 클라이언트 액세스를 제한 하기 위해 한 가지 방법은 설명 합니다.

> [!IMPORTANT]
> [오버 로드를 제대로 확인: Bugzilla 51940/GitHub 1950-격리 된 프로세스 및 사용자 지정 응용 프로그램 클래스를 사용 하 여 서비스 실패](https://github.com/xamarin/xamarin-android/issues/1950) Xamarin.Android 서비스가 제대로 시작 되지 않는 보고서 때 합니다 `IsolatedProcess` 로 설정 된 `true`합니다. 이 가이드는 참조를 위해 제공 됩니다. Xamarin.Android 응용 프로그램은 Java로 작성 하는 out of process 서비스와 통신할 수 해야 합니다.

## <a name="requirements"></a>요구 사항

이 가이드에서는 서비스를 만드는 데 익숙해야를 가정 합니다.

이전 대상으로 하는 앱을 사용 하 여 암시적 의도 사용할 수 있지만 Android Api,이 가이드는 명시적 의도 사용에만 집중 됩니다. Android 5.0 (API 수준 21)를 대상으로 하는 앱 또는 더 높은 서비스를 사용 하 여 바인딩할 명시적 의도 사용 해야 합니다 이 방법은이 가이드에서 소개 합니다...

## <a name="create-a-service-that-runs-in-a-separate-process"></a>별도의 프로세스로 실행 되는 서비스 만들기

위에서 설명한 대로 관련 된 일부 다른 Api는는 서비스가 자체 프로세스에서 실행 되 고 있다는 사실을 의미 합니다. 간략 한 개요를으로 다음과 같습니다. 바인딩할 원격 서비스를 사용 하는 단계  

* **만들기는 `Service` 하위 클래스입니다** &ndash; 하위 클래스 입니다는 `Service` 입력 하 고 바인딩된 서비스에 대 한 수명 주기 메서드를 구현 합니다. 서비스가 자체 프로세스에서 실행 되는 Android에 알려주는 메타 데이터를 설정 하는 데 필요한 이기도 합니다.
* **구현 된 `Handler`**  &ndash; 는 `Handler` 는 클라이언트 요청을 분석 하 고, 클라이언트에서 전달 된 매개 변수를 추출, 서비스에서 적절 한 메서드를 호출 하는 일을 담당 합니다.
* **인스턴스화하는 `Messenger`**  &ndash; 각 위에서 설명한 대로 `Service` 의 인스턴스를 유지 해야 합니다는 `Messenger` 클라이언트 요청을 라우팅하는 클래스는 `Handler` 이전 단계에서 만든.

자체 프로세스에서 실행 해야 하는 서비스는를 근본적으로 여전히 바인딩된 서비스. 서비스 클래스 기본 확장 `Service` 클래스 및으로 데코 레이트 된는 `ServiceAttribute` Android Android 매니페스트에서 번들 해야 하는 메타 데이터를 포함 합니다. 다음 속성과 함께 시작 하는 `ServiceAttribute` out of process 서비스에 중요 하지만:

1. `Exported` &ndash; 이 속성으로 설정 되어 있어야 `true` 다른 응용 프로그램의 서비스와 상호 작용할 수 있도록 합니다. 이 속성의 기본값은 `false`입니다.
2. `Process` &ndash; 이 속성을 설정 해야 합니다. 서비스가 실행 되는 프로세스의 이름을 지정 하는 것이 됩니다.
3. `IsolatedProcess` &ndash; 이 속성 Android에는 격리 된 샌드박스 iteract 시스템의 나머지 부분을 사용 하 여 최소한의 권한이 있는 서비스를 실행 하도록 지시 하는 추가 보안을 사용 하도록 설정 됩니다. 참조 [Bugzilla 51940-서비스 격리 된 프로세스 및 사용자 지정 응용 프로그램 클래스를 사용 하 여 오버 로드를 제대로 확인 되지 않습니다](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)합니다.
4. `Permission` &ndash; 요청 해야 합니다 (및 부여할) 클라이언트는 사용 권한을 지정 하 여 서비스에 대 한 클라이언트 액세스를 제어 하는 것이 가능 합니다.

서비스는 자체 프로세스를 실행 하는 `Process` 속성에는 `ServiceAttribute` 서비스의 이름으로 설정 해야 합니다. 외부 응용 프로그램과 상호 작용 하는 `Exported` 속성에 설정할 `true`합니다. 하는 경우 `Exported` 는 `false`, 동일한 APK (예: 동일한 응용 프로그램)에 클라이언트와 동일한 프로세스에서 실행 되는 서비스와 상호 작용할 수 됩니다.

값에 따라 어떤 유형의 프로세스에서 서비스가 실행 되는 `Process` 속성입니다. Android는 세 가지 유형의 프로세스를 식별합니다.

-   **개인 프로세스** &ndash; 개인 프로세스 한다는 것을 시작 하는 응용 프로그램에만 제공 됩니다. 개인 프로세스를 식별 하려면 해당 이름을로 시작 해야 합니다는 **:** (세미콜론). 스크린 샷 고 이전 코드 조각에 나와 있는 서비스는 개인 프로세스입니다. 다음 코드 조각은의 한 예로 `ServiceAttribute`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process=":timestampservice_process",
             Exported=true)]
    ```

-   **전역 프로세스** &ndash; 전역 프로세스에서 실행 되는 서비스는 장치에서 실행 하는 모든 응용 프로그램에 액세스할 수 있습니다. 전역 프로세스 소문자로 시작 하는 정규화 된 클래스 이름 이어야 합니다.
    (단계는 서비스를 보호 하지 않는 한 다른 응용 프로그램 수 바인딩 및 상호 작용 합니다. 무단된 사용 으로부터 서비스 보안 살펴봅니다이 가이드의 뒷부분에 나오는.)

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

-   **프로세스 격리** &ndash; 는 격리 된 프로세스는 자체의 특별 한 권한이 없어도 시스템의 나머지 부분에서 격리 된 고유한 샌드박스에서 실행 되는 프로세스입니다. 격리 된 프로세스에서 서비스를 실행 하는 `IsolatedProcess` 의 속성을 `ServiceAttribute` 로 설정 된 `true` 이 코드 조각과 같이:
    
    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             IsolatedProcess= true,
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

> [!IMPORTANT]
> 참조 [Bugzilla 51940-서비스 격리 된 프로세스 및 사용자 지정 응용 프로그램 클래스를 사용 하 여 오버 로드를 제대로 확인 되지 않습니다](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)

격리 된 서비스가 응용 프로그램 및 신뢰할 수 없는 코드에 대 한 장치를 보호 하는 간단한 방법을 보여 줍니다. 예를 들어, 앱을 다운로드 하 고 웹 사이트에서 스크립트를 실행할 수 있습니다. 이 경우에 격리 프로세스에서 수행 하는이 Android 장치를 손상 시 키 지 신뢰할 수 없는 코드에 대해 보안 강화를 제공 합니다.

> [!IMPORTANT]
> 서비스를 내보낸 후에 서비스의 이름을 변경 하지 마십시오. 서비스의 이름을 변경 하면 서비스를 사용 하는 다른 응용 프로그램 중단 될 수 있습니다.

결과 볼 수 있는 `Process` 속성에 다음 스크린샷에서 자체 개인 프로세스에서 실행 되는 서비스:

![개인 프로세스에서 실행 되는 서비스를 보여 주는 스크린 샷](out-of-process-services-images/ipc-04.png "개인 프로세스에서 실행 되는 서비스를 보여 주는 스크린샷.")

다음 스크린샷은이 `Process="com.xamarin.xample.messengerservice.timestampservice_process"` 전역 프로세스에서 실행 되는 서비스:

![전역 프로세스에서 실행 되는 서비스의 스크린샷](out-of-process-services-images/ipc-05.png "전역 프로세스에서 실행 되는 서비스의 스크린샷.")

한 번 합니다 `ServiceAttribute` 구현 하는 서비스 요구 사항 설정 되어를 `Handler`입니다.

### <a name="implementing-a-handler"></a>처리기를 구현합니다.

클라이언트 요청을 처리 하려면 서비스를 구현 해야 합니다는 `Handler` 시키고 합니다 `HandleMessage` methodThis는 메서드는 `Message` 인스턴스는 클라이언트에서 메서드 호출을 캡슐화 및 일부 작업을 호출 하는 변환 또는 서비스를 수행 하는 작업입니다. 합니다 `Message` 개체 라는 속성도 노출 `What` 는 정수 값의 의미 클라이언트와 서비스 간에 공유 되 고, 서비스가 클라이언트에 대해 수행 하는 일부 작업에 연결 합니다.

다음 코드 조각은 샘플 응용 프로그램에서 한 예를 보여 줍니다. `HandleMessage`합니다. 이 예제에서는 두 작업은 클라이언트가 요청할 수 있는 서비스

* 첫 번째 작업을 _Hello, World_ 메시지를 클라이언트에 간단한 메시지를 서비스로 전송 합니다.
* 두 번째 작업은 서비스에서 메서드를 호출 하 고 문자열을 검색,이 경우에 문자열은 실행 된 서비스 시작 및 기간 시간을 반환 하는 메시지:

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
                // Call methods on the service to retrive a timestamp message.
                break;
            default:
                Log.Warn(TAG, $"Unknown messageType, ignoring the value {messageType}.");
                base.HandleMessage(msg);
                break;
        }
    }
}
```

서비스에 대 한 패키지 매개 변수 수 이기도 합니다 `Message`합니다. 이 가이드의 뒷부분에서 설명 합니다. 고려해 야 할 다음 항목을 만드는 것은 `Messenger` 들어오는 처리할 개체 `Message`s입니다.

### <a name="instantiating-the-messenger"></a>Messenger 인스턴스화

앞에서 설명한 대로 역직렬화 합니다 `Message` 개체와 호출 `Handler.HandleMessage` 의 responsibilty는는 `Messenger` 개체. 합니다 `Messenger` 클래스에서는 `IBinder` 서비스에 메시지를 보내도록 클라이언트를 사용 하 여 개체입니다.  

인스턴스화하고 서비스를 시작 합니다 `Messenger` 삽입 및를 `Handler`. 이 초기화를 수행 하기에 좋은 위치는는 `OnCreate` 서비스의 메서드. 이 코드는 자체를 초기화 하는 서비스의 예로 `Handler` 고 `Messenger`:

```csharp
private Messenger messenger; // Instance variable for the Messenger

public override void OnCreate()
{
    base.OnCreate();
    messenger = new Messenger(new TimestampRequestHandler(this));
    Log.Info(TAG, $"TimestampService is running in process id {Android.OS.Process.MyPid()}.");
}
```

최종 단계는이 시점에서 `Service` 재정의할 `OnBind`합니다.

### <a name="implementing-serviceonbind"></a>Service.OnBind 구현

모든 바인딩된 서비스를 자체 프로세스에서 실행 하는지 여부를 구현 해야 합니다는 `OnBind` 메서드. 이 메서드의 반환 값은 클라이언트가 서비스와 상호 작용 하는 데 사용할 수 있는 일부 개체입니다. 해당 개체는 서비스는 로컬 서비스 또는 원격 서비스 인지 따라 달라 집니다. 로컬 서비스는 사용자 지정을 반환 하는 동안 `IBinder` 구현에서는 원격 서비스는 반환 합니다 `IBinder` 캡슐화 되는 하지만 `Messenger` 이전 섹션에서 만든:

```csharp
public override IBinder OnBind(Intent intent)
{
    Log.Debug(TAG, "OnBind");
    return messenger.Binder;
}
```

이러한 세 가지 단계를 완료 원격 서비스 전체 것으로 간주 됩니다.

## <a name="consuming-the-service"></a>서비스를 사용합니다.

모든 클라이언트 바인딩 및 원격 서비스를 사용 하는 일을 할 수 있는 일부 코드를 구현 해야 합니다. 개념적으로 클라이언트의 관점에서 차이가 거의 로컬 서비스 또는 원격 서비스 바인딩입니다. 클라이언트 호출을 `BindService` 서비스를 식별 하려는 명시적 의도 전달 하는 메서드, 및 `IServiceConnection` 클라이언트와 서비스 간의 연결을 관리할 수 있습니다는.

이 코드 조각은 만드는 방법의 예는 **내려졌습니다** 원격 서비스 바인딩에 대 한 합니다. 의도 서비스 및 서비스의 이름을 포함 하는 패키지를 식별 해야 합니다. 이 정보를 설정 하는 한 가지 방법은 사용 하는 것을 `Android.Content.ComponentName` 개체 및 제공 하는 의도 합니다. 이 코드 조각은 다음과 같습니다. 한 가지 예  

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

서비스 바인딩되면 합니다 `IServiceConnection.OnServiceConnected` 메서드가 호출 되 고 제공는 `IBinder` 클라이언트. 그러나 클라이언트는 직접 사용 하지는 `IBinder`합니다. 인스턴스화하는 대신 한 `Messenger` 개체는 `IBinder`합니다. 이 `Messenger` 클라이언트는 원격 서비스와 상호 작용을 사용 하는 합니다.

다음은 매우 기본적인 예가 `IServiceConnection` 클라이언트에 연결 하 고 서비스에서 연결을 끊은 처리 하는 방법을 보여 주는 구현 합니다. 에 `OnServiceConnected` 메서드는 수신 및 `IBinder`, 클라이언트를 만들고를 `Messenger` 는 `IBinder`:

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

호출 클라이언트에 대 한 가능성이 서비스 연결과 의도 만든 후 `BindService` 바인딩 프로세스를 시작 합니다.

```csharp
IServiceConnection serviceConnection = new TimestampServiceConnection(this);
BindActivity(serviceToStart, serviceConnection, Bind.AutoCreate);
```

클라이언트 서비스에 성공적으로 바인딩된 후 및 `Messenger` 는 사용할 수 있기 보내도록 클라이언트에 대 한 `Messages` 서비스.

## <a name="sending-messages-to-the-service"></a>서비스에 메시지 보내기

되 면 클라이언트 연결 및를 `Messenger` 개체를 디스패치 하 여 서비스와 통신할 수 있기 `Message` 를 통해 개체를 `Messenger`입니다. 이 통신은 단방향, 클라이언트는 메시지를 보냅니다 하지만 클라이언트에 서비스에서 반환 메시지가 없습니다. 이와 관련 하 여 `Message` 실행 후 제거 메커니즘입니다.

만들기 위한 기본 방법을 `Message` 개체가 사용 하는 [ `Message.Obtain` ](https://developer.xamarin.com/api/type/Android.OS.Message/#Public%20Methods) 팩터리 메서드입니다. 이 메서드는 끌어오기는 `Message` Android에서 유지 관리 되는 전역 풀에서 개체입니다. `Message.Obtain` 또한에 일부 오버 로드 된 메서드 수 있도록는 `Message` 값 및 서비스에 필요한 매개 변수를 사용 하 여 초기화할 개체입니다.  한 번 합니다 `Message` 가 인스턴스화된 것 발송 서비스를 호출 하 여 `Messenger.Send`입니다. 이 코드 조각을 만들고 디스패치의 예로는 `Message` 서비스 프로세스에:

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

여러 가지 형식의 가지는 `Message.Obtain` 메서드. 앞의 예제에서는 합니다 [ `Message.Obtain(Handler h, Int32 what)` ](https://developer.xamarin.com/api/member/Android.OS.Message.Obtain/p/Android.OS.Handler/System.Int32/)합니다. Out of process 서비스로; 비동기 요청 이기 때문에 서비스에서 응답이 있을 것 이므로 `Handler` 로 설정 된 `null`합니다. 두 번째 매개 변수를 `Int32 what`에 저장 됩니다 합니다 `.What` 의 속성을 `Message` 개체. `.What` 속성은 서비스에서 메서드를 호출할 코드 서비스 프로세스에서에서 사용 합니다.

`Message` 클래스는를 받는 하는 데 사용 될 수 있는 두 개의 추가 속성을 노출 합니다. `Arg1` 및 `Arg2`합니다. 이러한 두 속성은 있을 수 있는 몇 가지 특별 한 합의 된 클라이언트와 서비스 간의 의미가 있는 값 정수 값입니다. 예를 들어 `Arg1` 고객 ID를 보유할 수 있습니다 및 `Arg2` 해당 고객의 구매 주문 번호를 보유할 수 있습니다. 합니다 [ `Method.Obtain(Handler h, Int32 what, Int32 arg1, Int32 arg2)` ](https://developer.xamarin.com/api/member/Android.OS.Message.Obtain/p/Android.OS.Handler/System.Int32/System.Int32/System.Int32/) 두 속성을 설정 하려면 사용할 수 있습니다 때는 `Message` 만들어집니다. 이러한 두 값을 채우는 다른 방식으로 설정 하는 것을 `.Arg` 하 고 `.Arg2` 속성에 직접는 `Message` 개체를 만든 후.

### <a name="passing-additional-values-to-the-service"></a>서비스에 추가 값 전달

더 복잡 한 데이터를 사용 하 여 서비스에 전달할 수 있기를 `Bundle`입니다. 추가 값에 배치할 수 있습니다이 경우에 `Bundle` 와 함께 전송 및는 `Message` 설정 하 여를 [ `.Data` 속성](https://developer.xamarin.com/api/property/Android.OS.Message.Data/) 보내기 전에 속성입니다.

```csharp
Bundle serviceParameters = new Bundle();
serviceParameters.

var msg = Message.Obtain(null, Constants.SERVICE_TASK_TO_PERFORM);
msg.Data = serviceParameters;

messenger.Send(msg);
```


> [!NOTE]
> 일반적으로 `Message` 1MB 보다 큰 페이로드 없어야 합니다. 크기 제한에 Android의 버전에 따라 다를 수 있습니다 및 전용 변경 내용에 공급 업체 수에 대 한 구현과의 Android 오픈 소스 프로젝트 (AOSP) 장치와 함께 제공 되는 합니다.

## <a name="returning-values-from-the-service"></a>서비스에서 값을 반환합니다.

이 시점에 대해 이야기 하는 메시징 아키텍처는 단방향, 클라이언트는 서비스에 메시지를 보냅니다. 서비스가 클라이언트에 값을 반환 하는 데 필요한 것이 시점에 대해 이야기 하는 모든 항목 반전 됩니다. 서비스를 만들어야 합니다는 `Message`패키지, 모든 반환 값 및 디스패치를 `Message` 를 통해를 `Messenger` 클라이언트로. 그러나 서비스를 만들지 않습니다 자체 `Messenger`대신 클라이언트 인스턴스화 및 패키지에 의존을 `Messenger` 초기 요청의 일부로 합니다. 이 서비스는 `Send` 이 클라이언트 제공를 사용 하는 메시지 `Messenger`합니다.  

양방향 통신에 대 한 이벤트의 순서는이.

1. 클라이언트는 서비스에 바인딩합니다. 서비스 및 클라이언트에 연결.는 `IServiceConnection` 하 여 유지 관리 되는 클라이언트에 대 한 참조 해야 합니다.는 `Messenger` 전송 하는 데 사용 되는 개체 `Message`의서비스를 합니다. 혼동을 피하기 위해이로 간주 합니다 _서비스 Messenger_합니다.
2. 클라이언트를 인스턴스화하는 `Handler` (라고를 _클라이언트 처리기_)는를 사용 하 여 자체 초기화 및 `Messenger` (합니다 _클라이언트 Messenger_). Messenger 서비스와 클라이언트 Messenger 다른 양방향에서 트래픽을 처리 하는 두 개의 다른 개체는 참고 합니다. 서비스 Messenger 클라이언트 Messenger 서비스에서 클라이언트로 메시지를 처리할 동안 서비스에 클라이언트에서 메시지를 처리 합니다.
3. 클라이언트를 만듭니다는 `Message` 집합과 개체를 `ReplyTo` 클라이언트 Messenger를 사용 하 여 속성입니다. 메시지는 다음 서비스 Messenger를 사용 하 여 서비스에 전송 됩니다.
4. 서비스는 클라이언트에서 메시지를 받고 요청 된 작업을 수행 합니다.
5. 시간 클라이언트에 대 한 응답을 전송 하도록 서비스에 대 한 것을 사용할지 `Message.Obtain` 새 `Message` 개체입니다.
6. 클라이언트에이 메시지를 보내기 위해 서비스에서 클라이언트 Messenger 추출 합니다 `.ReplyTo` 클라이언트의 속성 메시지를 사용 하 `.Send` 는 `Message` 클라이언트로 다시 합니다.
7. 클라이언트에서 응답을 받으면 있기 자체 `Handler` 를 처리할 합니다 `Message` 검사 하 여를 `.What` 속성 (필요한 경우에 포함 된 모든 매개 변수를 추출 하 고는 `Message`).

이 샘플 코드 클라이언트 인스턴스화하는 방법을 보여 줍니다.는 `Message` 패키지는 `Messenger` 해당 응답에 대 한 서비스를 사용 해야 하는:

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

일부 변경 해야 하는 자체 `Handler` 추출 하는 `Messenger` 클라이언트로 응답을 보내도록이 사용 합니다. 이 코드 조각은 방법의 예제는 서비스의 `Handler` 만들어집니다는 `Message` 클라이언트로 다시 보냅니다.  

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

위의 코드 샘플에서 합니다 `Messenger` 클라이언트에서 생성 되는 인스턴스는 *하지* 서비스에서 수신 되는 동일한 개체입니다. 이 서로 다른 두 `Messenger` 통신 채널을 나타내는 두 개의 별도 프로세스에서 실행 되는 개체입니다.

## <a name="securing-the-service-with-android-permissions"></a>Android 권한 사용 하 여 서비스를 보호합니다.

전역 프로세스에서 실행 되는 서비스는 Android 장치에서 실행 되는 모든 응용 프로그램에서 액세스할 수 있습니다. 상황에 따라이 개방성 및 가용성은 바람직하지 및 권한이 없는 클라이언트에서 액세스 로부터 서비스를 보호 해야 합니다. 원격 서비스에 대 한 액세스를 제한 하는 한 가지 방법은 Android 권한을 사용 하는 것입니다.

권한으로 식별할 수 있습니다는 `Permission` 의 속성을 `ServiceAttribute` 데코 레이트 하는 `Service` 하위 클래스입니다. 이 클라이언트가 서비스에 바인딩할 때 부여 되어야 하는 사용 권한을 이름을 지정 합니다. 클라이언트에 적절 한 권한이 없는 경우 Android 시킵니다는 `Java.Lang.SecurityException` 클라이언트 서비스를 바인딩할 하려고 할 때입니다.

Android에서 제공 하는 네 개의 다른 권한 수준이 있습니다.

* **정상적인** &ndash; 기본 사용 권한 수준입니다. 클라이언트 요청 하는 Android에서 자동으로 부여할 수 있는 위험이 낮은 권한을 식별 하는 것이 됩니다. 사용자는 이러한 권한을 명시적으로 부여 하지 않아도 되지만 앱 설정에서 사용 권한을 볼 수 있습니다.
* **서명을** &ndash; 동일한 인증서로 서명 된 응용 프로그램을 Android에서 자동으로 부여 되는 권한의 특수 범주입니다. 이 사용 권한은 쉽게 구성 요소 또는 상수 승인에 대 한 사용자의 도움 없이 자신의 앱 간의 데이터 공유 응용 프로그램 개발자에 대 한 것을 확인 하도록 설계 되었습니다.
* **signatureOrSystem** &ndash; 이것은 매우 비슷합니다 합니다 **서명** 위에 설명 된 사용 권한. 될 뿐 아니라 자동으로 부여 된 동일한 인증서로 서명 된 앱에,이 사용 권한은도 부여 됩니다 서명 된 앱을 Android 시스템 이미지를 사용 하 여 앱에 서명 하는 데 사용 된 것과 동일한 인증서를 설치 합니다. 이 권한은 일반적으로 사용 됩니다 Android ROM 개발자가 타사 앱을 사용 하도록 응용 프로그램을 허용 하려면. 일반적으로 된에 대 한 일반 배포는 앱에서 사용 되지 됩니다.
* **위험한** &ndash; 위험한 권한은 사용자에 대 한 문제를 일으킬 수 있는 것입니다. 이러한 이유로 **위험한** 권한을 명시적으로 사용자가 승인 해야 합니다.

때문에 `signature` 하 고 `normal` 사용 권한을 자동으로 부여 됩니다 설치 시 Android에서 서비스를 호스트 하는 APK를 설치 하는 것이 중요 한 것 **하기 전에** 클라이언트가 포함 된 APK 합니다. 클라이언트는 먼저 설치 하는 경우 Android 권한을 부여 하지 됩니다. 이 경우 APK 클라이언트를 제거, APK 서비스를 설치 및 APK 클라이언트 다시 설치 해야 합니다.

두 가지 일반적인 Android 권한 사용 하 여 서비스를 보호 합니다.

1.  **서명을 수준 보안 구현** &ndash; 서명을 수준 보안 권한 인지 자동으로 부여는 서비스를 보유 하는 APK에 서명 하는 데 사용 된 동일한 키로 서명 된 해당 응용 프로그램을 의미 합니다. 해당 서비스를 보호 하면서 자신의 응용 프로그램에서 액세스할 수 있도록 개발자를 위한 간단한 방법입니다. 서명을 수준 사용 권한을 설정 하 여 선언 된를 `Permission` 의 속성을 `ServiceAttribute` 에 `signature`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.TimestampService.timestampservice_process",
             Permission="signature")]
    public class TimestampService : Service
    {
    }
    ```

2.  **사용자 지정 권한을 만드는** &ndash; 서비스의 개발자는 서비스에 대 한 사용자 지정 권한을 만드는 것입니다. 이 개발자가 다른 개발자가 응용 프로그램과 해당 서비스를 공유 하려고 할 때 적합 합니다. 사용자 지정 권한을 구현 하는 약간 더 많은 노력이 필요 하 고 아래 설명 합니다.

사용자 지정 만들기 간소화 된 예가 `normal` 권한이 다음 섹션에서 설명 합니다. Android 사용 권한에 대 한 자세한 내용은 Google의 설명서를 참조 하세요 [모범 사례 및 보안](https://developer.android.com/training/articles/security-tips.html)합니다. Android 사용 권한에 대 한 자세한 내용은 참조는 [사용 권한 섹션](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms) Android 권한에 대 한 자세한 내용은 응용 프로그램 매니페스트에 대 한 Android 설명서의 합니다.

> [!NOTE]
> 일반적으로 [사용자 지정 권한 사용을 방지 하는 Google](https://developer.android.com/training/articles/security-tips.html#RequestingPermissions) 사용자에 게 혼란 지식의 대로 합니다.

### <a name="creating-a-custom-permission"></a>사용자 지정 권한 만들기

사용자 지정 권한을 사용 하려면에 클라이언트에는 명시적으로 해당 권한을 요청 하는 동안 서비스에서 선언 됩니다.

APK 서비스에서 권한을 만드는 `permission` 요소에 추가 됩니다 합니다 `manifest` 요소에 **AndroidManifest.xml**합니다. 이 권한이 있어야 합니다 `name`, `protectionLevel`, 및 `label` 특성 집합입니다. `name` 특성을 고유 하 게 사용 권한을 식별 하는 문자열로 설정 해야 합니다. 이름에 표시 됩니다는 **앱 정보** 보기를 **Android 설정** (처럼 다음 섹션에서).

`protectionLevel` 특성 위에 설명 된 4 개의 문자열 값 중 하나로 설정 해야 합니다.  합니다 `label` 고 `description` 문자열 리소스를 참조 해야 하 고 사용자에 게 친숙 한 이름 및 설명을 제공 하는 데 사용 됩니다.

이 코드 조각은 사용자 지정을 선언 하는 한 예로 `permission` 특성 **AndroidManifest.xml** 서비스가 포함 된 APK의:

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

그런 다음, **AndroidManifest.xml** 클라이언트의 APK이 새 사용 권한을 요청 명시적으로 해야 합니다. 추가 하 여 이렇게 합니다 `users-permission` 특성을 합니다 **AndroidManifest.xml**:

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

### <a name="view-the-permissions-granted-to-an-app"></a>앱에 부여 된 권한 보기

응용 프로그램에 부여 된 사용 권한을 보려는 Android 설정 앱을 열고 선택 **앱**합니다. 찾아 응용 프로그램 목록에서 선택 합니다. **앱 정보** 화면에서 누릅니다 **권한을** 는 앱에 부여 된 모든 사용 권한을 표시 하는 보기를 표시 합니다.

[![응용 프로그램에 부여 된 권한을 찾는 방법을 보여 주는 Android 장치에서 스크린 샷](out-of-process-services-images/ipc-06-sml.png)](out-of-process-services-images/ipc-06.png#lightbox)

## <a name="summary"></a>요약

이 가이드에는 원격 프로세스에서 Android 서비스를 실행 하는 방법에 대 한 고급 토론을 했습니다. 원격 서비스를 Android 앱의 성능과 안정성에 유용할 수 있습니다 이유 몇 가지 이유 함께 로컬 및 원격 서비스 간의 차이점 설명 했습니다. 원격 서비스를 구현 하는 방법과 클라이언트 서비스가 통신할 수 있는 방법을 설명 하는, 후 가이드 발생 되에서 권한이 있는 클라이언트만 서비스에 액세스를 제한 하는 한 가지 방법은 제공 했습니다.


## <a name="related-links"></a>관련 링크

- [Handler](https://developer.xamarin.com/api/type/Android.OS.Handler/)
- [메시지](https://developer.xamarin.com/api/type/Android.OS.Message/)
- [Messenger](https://developer.xamarin.com/api/type/Android.OS.Messenger/)
- [ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute)
- [내보낸 특성](https://developer.android.com/guide/topics/manifest/service-element.html#exported)
- [오버 로드를 제대로 해결 하려면 격리 된 프로세스 및 사용자 지정 응용 프로그램 클래스를 사용 하 여 서비스 실패](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)
- [프로세스 및 스레드](https://developer.android.com/guide/components/processes-and-threads.html)
- [Android 매니페스트-사용 권한](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms)
- [보안 팁](https://developer.android.com/training/articles/security-tips.html)
- [MessengerServiceDemo (샘플)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/MessengerServiceDemo/)
