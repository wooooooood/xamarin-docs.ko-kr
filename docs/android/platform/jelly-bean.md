---
title: 젤리 Bean 기능
description: '이 문서는 Android 4.1에서 도입 된 개발자를 위한 새로운 기능의 높은 수준의 개요를 제공 합니다. 이러한 기능에 포함: 알림, 멀티미디어, 피어-투-피어 네트워크 검색, 애니메이션, 새 사용 권한에 대 한 업데이트, 큰 파일을 공유할 Android 빔에 대 한 업데이트를 강화 합니다.'
ms.prod: xamarin
ms.assetid: 23F57634-2EF9-5C15-C710-B3E19A5AF7E1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 1d8068ccfc8d0f159a88704370261ec5f20d8b7c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="jelly-bean-features"></a>젤리 Bean 기능

_이 문서는 Android 4.1에서 도입 된 개발자를 위한 새로운 기능의 높은 수준의 개요를 제공 합니다. 이러한 기능에 포함: 알림, 멀티미디어, 피어-투-피어 네트워크 검색, 애니메이션, 새 사용 권한에 대 한 업데이트, 큰 파일을 공유할 Android 빔에 대 한 업데이트를 강화 합니다._



## <a name="overview"></a>개요

Android 4.1 (API 수준 16), 라고도 "젤리 Bean", 2012 년 7 월 9 일에 출시 되었습니다. 이 문서에서는 Xamarin.Android를 사용 하 여 개발자를 위한 Android 4.1의 새로운 기능 중 일부를 간략하게 소개를 제공 합니다. 이러한 새로운 기능을 일부는 시작 작업, 카메라에 대 한 새 사운드 및 응용 프로그램 스택 탐색에 대 한 지원 향상된에 대 한 애니메이션의 향상 된 기능입니다. 잘라내기 및 붙여넣기 의도 수 되었습니다.

Android 응용 프로그램의 안정성 불안정 한 콘텐츠 공급자에 대 한 종속성을 격리 하는 기능 향상 됩니다. 시작 하는 작업에만 액세스할 수 있도록 서비스 격리 될 수도 있습니다.

지원이 사용 하 여 네트워크 서비스 검색에 대 한 추가 되었습니다 Bonjour, UPnP, 또는 멀티 캐스트 DNS 기반 서비스입니다. 텍스트, 실행 단추 및 큰 이미지 형식이 지정 된 다양 한 알림에 대 한 가능한 되었습니다.

마지막으로 여러 새 사용 권한 Android 4.1에 추가 되었습니다.



## <a name="requirements"></a>요구 사항

Xamarin.Android 응용 프로그램을 개발 하려면 Xamarin.Android 필요 젤리 Bean을 사용 하 여 4.2.6 또는 상위 수준과 Android 4.1 (API 수준 16) 다음 화면에 표시 된 대로 Android SDK Manager를 통해를 설치할 수 있습니다.

[![Android SDK Manager에서 Android 4.1를 선택합니다.](jelly-bean-images/image1.png)](jelly-bean-images/image1.png#lightbox)



## <a name="whats-new"></a>새로운 기능



### <a name="animations"></a>애니메이션

사용 하 여 확대/축소 애니메이션 또는 사용자 지정 애니메이션 중 하나를 사용 하 여 활동을 실행할 수 있습니다는 `ActivityOptions` 클래스입니다. 다음 새 메서드가 이러한 애니메이션을 지원 하기 위해 제공 됩니다.

-   `MakeScaleUpAnimation` –이 시작 위치 및 길이로 화면에 활동 창은 크기를 조정 하는 애니메이션을 만들어집니다.
-   `MakeThumbnailScaleUpAnimation` -이렇게 하면 확장 하는 애니메이션 화면에서 지정 된 위치에서 축소판 이미지에서 만들어집니다.
-   `MakeCustomAnimation` –이 응용 프로그램의 리소스에서 애니메이션을 만듭니다. 활동을 열면에 대 한 애니메이션 용이고 다른 하나는 활동을 중지할 시기 있습니다.


새 `TimeAnimator` 클래스 인터페이스를 제공 `TimeAnimator.ITimeListener` 애니메이션에서 프레임이 변경 될 때마다 응용 프로그램에 게 알릴 수입니다. 예를 들어 다음 구현의 `TimeAnimator.ITimeListener`:

```csharp
class MyTimeListener : Java.Lang.Object,  TimeAnimator.ITimeListener
{
    public void OnTimeUpdate(TimeAnimator animation, long totalTime, long deltaTime)
    {
        Log.Debug("Activity1", "totalTime={0}, deltaTime={1}", totalTime, deltaTime);
    }
}
```

클래스의 인스턴스를 사용 하는 지금은 `TimeAnimator` 만들어지면 수신기 설정 및:

```csharp
var animator = new TimeAnimator();
animator.SetTimeListener(new MyTimeListener());
animator.Start();
```

로 `TimeAnimator` 인스턴스가 실행 되 고, 호출 `ITimeAnimator.ITimeListener`, 방법을 다음 기록 합니다는 긴 애니메이터가 경과 했는데도 실행 기간 것 메서드가 마지막으로 이후에 된 대로 호출 했습니다.



### <a name="application-stack-navigation"></a>응용 프로그램 스택 탐색

Android 4.1 Android 3.0에 도입 된 응용 프로그램 스택 탐색 향상 합니다. 지정 하 여는 `ParentName` 속성은 `ActivityAttribute`, Android를 누를 때 적절 한 부모 활동을 열 수는 [위로 단추](http://developer.android.com/design/patterns/navigation.html#up-vs-back) 작업 모음-에서 Android는 가지정한인스턴스화`ParentName`속성입니다. 지정된 된 태스크를 구성 하는 작업의 계층 구조를 유지 하지 않도록 할 수 있습니다.

대부분의 응용 프로그램 설정에 대 한는 `ParentName` 활동에는 응용 프로그램 스택의;를 탐색 하기 위한 올바른 동작을 제공 하는 Android에 대 한 충분 한 정보 Android 각 부모 활동에 대 한 일련의 의도 만들어서 필요한 백 스택에 이해할수록 됩니다. 그러나, 인위적인 응용 프로그램 스택 이기 때문에 각 가상 활동 자연 스러운 활동에 저장된 된 상태가 제공 되지 않습니다. 가상 부모 활동에 저장 된 상태를 제공 하려면 활동 재정의할 수 있습니다는 `OnPrepareNavigationUpTaskStack` 메서드. 이 메서드는 `TaskStackBuilder` 의도의 컬렉션에 있는 인스턴스 개체를 Android 백 스택을 만들려면 사용 합니다. 가상 작업을 만들 때 적절 한 상태 정보를 받게 됩니다 되도록 활동에서 이러한 의도 수정할 수 있습니다.

보다 복잡 한 시나리오에 대 한 설치의 동작을 처리 하 고에서 백 스택을 생성 하는 데 사용할 수 있는 활동 클래스에 새로운 메서드에:

-   `OnNavigateUp` –이 메서드를 재정의 하 여 사용자 지정 작업을 수행할 수는 때는 <span class="ui">를</span> 추가 합니다.
-   `NavigateUpTo` –이 메서드를 호출 하면 현재 활동에서 지정된 된 의미에서 지정 된 활동으로 이동 하려면 응용 프로그램.
-   `ParentActivityIntent` –이 현재 활동의 부모 활동을 시작 하는 프로그램 의도 얻으려고 사용 됩니다.
-   `ShouldUpRecreateTask` –이 메서드는 부모 활동까지 탐색 하는 가상 백 스택에 만들어야 할 쿼리할 사용 됩니다. 반환 `true` 가상 스택 만들어야 합니다. 
-   `FinishAffinity` –이 메서드를 호출 하면 현재 활동 및 그 아래에 현재 작업 하는 모든 활동 동일한 작업 선호도 완료 됩니다.
-   `OnCreateNavigateUpTaskStack` –이 메서드는 가상 스택 만들어지는 방법을 완전히 제어 해야 하는 경우 재정의 됩니다.




### <a name="camera"></a>카메라

새 인터페이스는 `Camera.IAutoFocusMoveCallback`, 자동 포커스에 시작 또는 이동을 중지 될 때 검색에 사용할 수 있는 합니다. 이 새 인터페이스의 예는 다음 코드 조각에서 볼 수 있습니다.

```csharp
public class AutoFocusCallbackActivity : Activity, Camera.IAutoFocusCallback
{
    public void OnAutoFocus(bool success, Camera camera)
    {
        // camera is an instance of the camera service object.

        if (success)
        {
            // Auto focus was successful - do something here.
        }
        else
        {
            // Auto focus didn't happen for some reason - react to that here.
        }
    }
}
```

새 클래스 `MediaActionSound` 다양 한 미디어 작업에 대 한 소리를 생성 하기 위한 API의 집합을 제공 합니다. 열거형에서 정의 되며 카메라, 발생할 수 있는 몇 가지 작업이 있습니다 `Android.Media.MediaActionSoundType`:

-   `MediaActionSoundType.FocusComplete` –이 소리를 중 점으로 완료 되 면 재생 됩니다.
-   `MediaActionSoundType.ShutterClick` –이 소리 정지 이미지 사진을 라인 상태가 될 때 재생 됩니다.
-   `MediaActionSoundType.StartVideoRecording` –이 소리가 사용 되는 비디오 기록 시작을 표시 합니다.
-   `MediaActionSoundType.StopVideoRecording` –이 비디오 기록이의 끝을 나타내는이 소리 재생 됩니다.


사용 하는 방법의 예는 `MediaActionSound` 클래스는 다음 코드 조각에서 볼 수 있습니다.

```csharp
var mediaActionPlayer = new MediaActionSound();

// Preload the sound for a shutter click.
mediaActionPlayer.Load(MediaActionSoundType.ShutterClick);
var button = FindViewById<Button>(Resource.Id.MyButton);

// Play the sound on a button click.
button.Click += (sender, args) => mediaActionPlayer.Play(MediaActionSoundType.ShutterClick);

// This releases the preloaded resources. Don’t make any calls on
// mediaActionPlayer after this.
mediaActionPlayer.Release();
```



### <a name="connectivity"></a>연결



#### <a name="android-beam"></a>Android 빔

Android 빔 두 Android 장치가 서로 통신할 수 있도록 하는 NFC 기반 기술입니다. Android 4.1 큰 파일 전송에 대 한 향상 된 지원을 제공합니다. 새 메서드를 사용 하는 경우 `NfcAdapter.SetBeamPushUris()` Android는 빠른 전송 속도 달성 하기 위해 (예: Bluetooth) 대체 전송 메커니즘 간에 전환 합니다.



#### <a name="network-services-discovery"></a>네트워크 서비스 검색

Android 4.1 새로운 API의 멀티 캐스트 DNS 기반 서비스 검색에 포함 되어 있습니다.
이렇게 하면 응용 프로그램을 검색 하 고 프린터, 카메라 및 미디어 장치 같은 다른 장치를 Wi-fi에 연결 합니다. 이러한 새 API가 있는 `Android.Net.Nsd` 패키지 합니다.

다른 서비스에 사용 될 수 있는 서비스를 만들려면는 `NsdServiceInfo` 클래스는 서비스의 속성을 정의 하는 개체를 만드는 데 사용 됩니다. 이 개체는 다음을 제공 `NsdManager.RegisterService()` 구현의 함께 `NsdManager.ResolveListener`합니다. 구현 `NsdManager.ResolveListener` 성공적으로 등록 하는 알림을 보낼 하 고 서비스 등록을 취소 하는 데 사용 됩니다.

네트워크 및의 구현에서 서비스를 검색 하려면 `Nsd.DiscoveryListener` 에 전달 된 `NsdManager.discoverServices()`합니다.



#### <a name="network-usage"></a>네트워크 사용량

새로운 메서드를 `ConnectivityManager.IsActiveNetworkMetered` 요금제 네트워크에 연결 되어 있는지 확인 하는 장치를 허용 합니다. 이 메서드는 정확 하 게 데이터 작업에 대 한 비용이 많이 드는 비용은 있을 수 있음을 사용자에 게 알리는 하 여 데이터 사용을 관리 하는 데 도움이 데 사용할 수 있습니다.



#### <a name="wifi-direct-service-discovery"></a>WiFi 직접 서비스 검색

`WifiP2pManager` 클래스 지원 하기 위해 Android 4.0에서 도입 된 *zeroconf*합니다. Zeroconf (네트워킹 구성 0)는 인간 네트워크 운영자 또는 특별 한 구성이 서버 개입으로 장치 (컴퓨터, 프린터, 휴대폰) 네트워크에 자동으로 연결할 수 있도록 허용 하는 기술 집합.

젤리 Bean의 `WifiP2pManager` 근처의 하나를 사용 하 여 장치를 검색할 수 *Bonjour* 또는 *Upnp*합니다. Bonjour는 zeroconf의 Apple의 구현입니다. Upnp 지원 zeroconf 네트워킹 프로토콜 설정 됩니다. 에 추가 된 다음 메서드는 `WiFiP2pManager` Wi-fi 서비스 검색을 지원 하려면:

-   `AddLocalService()` –이 메서드는 피어가 검색에 대 한 wi-fi 서비스로 응용 프로그램을 알립니다.
-   `AddServiceRequest(` )-이 방법은 framework에 대 한 서비스 검색 요청을 보내려고 합니다. Wi-fi 서비스 검색을 초기화 하지 사용 됩니다.
-   `SetDnsSdResponseListeners()` –이 메서드는 Bonjour의 검색 요청에 응답을 수신한 경우에 호출 될 콜백을 등록에 사용 됩니다.
-   `SetUpnpServiceResponseListener()` –이 메서드는 Upnp 검색 요청에 대 한 응답을 수신한 경우에 호출 될 콜백을 등록에 사용 됩니다.




### <a name="content-providers"></a>콘텐츠 공급자

`ContentResolver` 클래스에는 새로운 메서드를 받았음을 `AcquireUnstableContentProvider`합니다. 이 메서드를 사용 하면 응용 프로그램에서 "불안정" 콘텐츠 공급자를 얻으려고 합니다. 일반적으로 응용 프로그램 콘텐츠 공급자를 획득 하 고이 콘텐츠 공급자가 충돌을 응용 프로그램이 됩니다. 이 메서드 호출으로 콘텐츠 공급자가 충돌 하면 응용 프로그램 충돌 하지 않습니다. 대신, `Android.OS.DeadObjectionException` 콘텐츠 공급자가 지나갈 하는 응용 프로그램에 알리기 위해 콘텐츠 공급자에 대 한 호출에서 throw 됩니다. "불안정" 콘텐츠 공급자가 다른 응용 프로그램에서 콘텐츠 공급자와 상호 작용할 때 유용 – 확률이 다른 응용 프로그램에서 오류가 있는 코드가 다른 응용 프로그램에 영향을 줍니다.



### <a name="copy-and-paste-with-intents"></a>복사 및 붙여넣기 의도

`Intent` 클래스 이제 사용할 수 있습니다는 `ClipData` 통해 연관 된 개체는 `Intent.ClipData` 속성입니다. 목적으로 전송 될 클립보드의 추가 데이터에 대 한이 방법을 사용 하면 됩니다. 인스턴스 `ClipData` 하나 이상 포함 될 수 `ClipData.Item`합니다. `ClipData.Item`다음과 같은 형식의 항목이:

-   **텍스트** –이 모든 문자열의 텍스트를 HTML 중 하나 또는 기본 제공 Android 스타일에서 해당 형식을 사용할 모든 문자열에 걸쳐 있습니다.
-  **의도** -모두 `Intent` 개체입니다.
-   **Uri** – HTTP 책갈피 콘텐츠 공급자에 대 한 URI 등 모든 URI 수입니다.




### <a name="isolated-services"></a>격리 서비스

격리 된 서비스는 서비스 특수 자체 프로세스에서 실행 하는 자체의 권한이 없습니다. 서비스와만 통신 인 경우 서비스 API를 통해 바인딩 및 서비스를 시작 합니다. isolated 서비스 속성을 설정 하 여 선언할 수는 `IsolatedProcess="true"` 에 `ServiceAttribute` 하는 서비스 클래스를 보여 합니다.


### <a name="media"></a>미디어

새 `Android.Media.MediaCodec` 클래스는 낮은 수준의 미디어 코덱 하기 위한 API를 제공 합니다. 응용 프로그램 시스템 낮은 수준의 코덱 장치에서 사용할 수 있는 확인을 쿼리할 수 있습니다.

새 `Android.Media.Audiofx.AudioEffect` 하위 클래스 추가 오디오 미리 캡처된 오디오에 대 한 처리를 지원 하기 위해 추가 되었습니다.

-   `Android.Media.Audiofx.AcousticEchoCanceler` –이 클래스는 캡처된 오디오 신호에서 원격측에서 신호를 제거 하려면 오디오 전처리에 사용 됩니다. 예를 들어, 음성 통신 응용 프로그램에서 에코를 제거합니다.
-   `Android.Media.Audiofx.AutomaticGainControl` –이 클래스는을 승격 하거나 출력 신호는 상수 수 있도록 입력된 신호를 낮추는 하 여 캡처된 신호 정규화 사용 됩니다.
-   `Android.Media.Audiofx.NoiseSuppressor` –이 클래스는 백그라운드 노이즈 캡처된 신호에서 제거 됩니다.


모든 장치는 이러한 효과 지원 합니다. 메서드가 `AudioEffect.IsAvailable` 문제의 오디오 효과 응용 프로그램을 실행 하는 장치에서 지원 하는지는 응용 프로그램에서 호출 해야 합니다.

`MediaPlayer` 이제 클래스를 사용 하 여 겹치지 재생 지원는 `SetNextMediaPlayer()` 메서드. 이 새 메서드는 현재 미디어 플레이어에는 해당 재생이 완료 되 면 시작 하려면 다음 MediaPlayer를 지정 합니다.

다음 새 클래스 미디어를 재생 하는 위치를 선택 하기 위한 표준 메커니즘 및 UI를 제공 합니다.

-   `MediaRouter` –이 클래스를 외부 스피커로 장치 또는 다른 장치에서 미디어 채널 라우팅을 제어할 수 있습니다.
-   `MediaRouterActionProvider` 및 `MediaRouteButton` -이러한 클래스를 선택 하 고 미디어 재생 일관 된 UI를 제공할 수 있습니다.




### <a name="notifications"></a>알림

Android 4.1 더 많은 유연성 및 알림을 표시 하는 컨트롤과 응용 프로그램이 있습니다. 이제 응용 프로그램 사용자에 게 더 나은 및 큰 알림을 표시할 수 있습니다. 새로운 메서드를 `NotificationBuilder.SetStyle()` 새로운 세 개의 새 스타일 중 하나에 대 한 알림을에 설정 될 수 있습니다.

-   `Notification.BigPictureStyle` – 이미지 개는 알림을 생성 하는 도우미 클래스입니다. 다음 이미지는 큰 이미지를 사용 하는 알림 예를 보여 줍니다.


 [![BigPictureStyle 알림 예제 스크린 샷](jelly-bean-images/image2.png)](jelly-bean-images/image2.png#lightbox)

-   `Notification.BigTextStyle` -전자 메일 등의 텍스트의 여러 줄에 있는 알림을 생성 하는 도우미 클래스입니다. 다음 스크린샷에이 새 알림 스타일의 예를 볼 수 있습니다.


 [![BigTextStyle 알림 예제 스크린 샷](jelly-bean-images/image3.png)](jelly-bean-images/image3.png#lightbox)

-   `Notification.InboxStyle` –이 클래스는이 스크린샷에 표시 된 대로 전자 메일 메시지에서 조각 등의 문자열의 목록을 포함 하는 알림을 생성 하는 도우미 클래스:


 [![Notification.InboxStyle 알림 예제 스크린 샷](jelly-bean-images/image4.png)](jelly-bean-images/image4.png#lightbox)

것 알림을 normal 또는 더 큰 스타일을 사용 하는 경우 알림 메시지의 맨 아래에 최대 두 개의 실행 단추를 추가 하는 것과 같습니다.
실행 단추는 알림 메시지의 아래쪽에 다음 스크린샷에서 이러한 예제를 볼 수 있습니다.

 [![아래 알림 메시지가 표시 되는 작업 단추의 예제 스크린 샷](jelly-bean-images/image5.png)](jelly-bean-images/image5.png#lightbox)

`Notification` 클래스는 개발자가 알림에 대 한 우선 순위 수준 5 개 중 하나를 지정할 수 있는 새로운 상수를 수신 했습니다. 사용 하 여 알림을 설정할 수 있습니다는 `Priority` 속성입니다.



### <a name="permissions"></a>사용 권한

다음 새 권한이 추가 되었습니다.

-   `READ_EXTERNAL_STORAGE` -응용 프로그램에는 외부 저장소에 읽기 전용 액세스가 필요합니다. 모든 응용 프로그램 현재 기본적으로 읽기 액세스 권한이 있지만 이후 릴리스의 Android 응용 프로그램에 명시적으로 읽기 액세스 권한을 요청 해야 합니다.
-   `READ_USER_DICTIONARY` -사용자의 word 사전에 읽기 액세스를 허용 합니다.
-   `READ_CALL_LOG` -응용을 프로그램을 호출 로그를 읽는 여 들어오고 나가는 호출에 대 한 정보를 얻을 수 있습니다.
-   `WRITE_CALL_LOG` -응용을 프로그램의 전화 통화 로그에 기록할 수 있습니다.
-   `WRITE_USER_DICTIONARY` -응용을 프로그램 사용자의 단어 사전에 쓸 수 있습니다.


중요 한 변경 사항에 유의 하 `READ_EXTERNAL_STORAGE` – Android에서이 권한이 자동으로 부여 되는 현재 합니다. Android의 이후 버전에는 사용 권한을 부여 하기 전에이 권한을 요청 하는 응용 프로그램 필요 합니다.



## <a name="summary"></a>요약

이 문서에서는 몇 가지 새로운 API의 Android 4.1 (API 수준 16)에서 사용할 수 있는 도입 되었습니다. 애니메이션 및 시작 활동을 애니메이션에 대 한 변경 내용 중 일부를 강조 표시 하 고 새로운 API Bonjour 또는 UPnP 같은 프로토콜을 사용 하 여 기타 장치의 네트워크 검색을 도입 합니다. API의 기타 변경 내용 된 격리 된 서비스 또는 "불안정" 콘텐츠 공급자를 사용 하는 기능은 의도 통해 데이터를 잘라내어 하는 등도 강조 표시 됩니다.

이 문서에서 다음 오류가 발생 했습니다.는 업데이트 알림, 소개 하 고 Android 4.1으로 도입 된 새 권한 중 일부를 설명


## <a name="related-links"></a>관련 링크

- [시간 애니메이션 예제 (샘플)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TimeAnimatorExample/)
- [Android 4.1 Api](http://developer.android.com/about/versions/android-4.1.html)
- [작업 및 뒤로 스택](http://developer.android.com/guide/components/tasks-and-back-stack.html)
- [뒤로 / 위쪽으로 탐색](http://developer.android.com/design/patterns/navigation.html)
