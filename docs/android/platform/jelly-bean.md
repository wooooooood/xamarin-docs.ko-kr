---
title: Jelly Bean 기능
description: '이 문서는 Android 4.1에서 도입 된 개발자를 위한 새로운 기능의 간략 한 개요를 제공 합니다. 이러한 기능에 포함: 알림, 멀티미디어, 피어-투-피어 네트워크 검색, 애니메이션, 새 사용 권한을 업데이트 하는 큰 파일을 공유 하는 Android 무선 전송에 대 한 업데이트를 강화 합니다.'
ms.prod: xamarin
ms.assetid: 23F57634-2EF9-5C15-C710-B3E19A5AF7E1
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 24fc14b0342591c56f5bf91862b0d94759a42834
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670105"
---
# <a name="jelly-bean-features"></a>Jelly Bean 기능

_이 문서는 Android 4.1에서 도입 된 개발자를 위한 새로운 기능의 간략 한 개요를 제공 합니다. 이러한 기능에 포함: 알림, 멀티미디어, 피어-투-피어 네트워크 검색, 애니메이션, 새 사용 권한을 업데이트 하는 큰 파일을 공유 하는 Android 무선 전송에 대 한 업데이트를 강화 합니다._



## <a name="overview"></a>개요

Android 4.1 (API 레벨 16) 라고도 "Jelly Bean", 2012 년 7 월 9 일에 릴리스 되었습니다. 이 문서에서는 Xamarin.Android를 사용 하 여 개발자를 위한 Android 4.1의 새로운 기능 중 일부에 대 한 높은 수준의 소개를 제공 합니다. 이러한 새로운 기능을 일부는 시작 작업, 카메라에 대 한 새 사운드 및 응용 프로그램 스택 탐색에 대 한 향상 된 지원에 대 한 애니메이션의 향상 된 기능입니다. 이제를 잘라내고 붙여넣으려면 의도 사용 하 여는 것이 불가능 합니다.

불안정 한 콘텐츠 공급자에 대 한 종속성을 격리 하는 기능을 사용 하 여 Android 응용 프로그램의 안정성이 향상 됩니다. 서비스 시작 하는 작업에만 액세스할 수 있도록 격리 된 수도 있습니다.

지원은 사용 하 여 네트워크 서비스 검색에 대 한 추가 되었습니다 Bonjour, UPnP, 또는 멀티 캐스트 DNS 기반 서비스. 이제 텍스트, 실행 단추 및 큰 이미지 형식이 지정 된 다양 한 알림에 가능성이 있습니다.

마지막으로 여러 새 사용 권한 Android 4.1에 추가 되었습니다.



## <a name="requirements"></a>요구 사항

Xamarin.Android 응용 프로그램을 개발 하려면 Xamarin.Android 필요 Jelly Bean을 사용 하 여 4.2.6 또는 더 높은 및 Android 4.1 (API 레벨 16) 다음 화면에 표시 된 대로 Android SDK Manager를 통해를 설치할 수 있습니다.

[![Android SDK Manager에서 Android 4.1 선택](jelly-bean-images/image1.png)](jelly-bean-images/image1.png#lightbox)



## <a name="whats-new"></a>새로운 기능



### <a name="animations"></a>애니메이션

사용 하 여 확대/축소 애니메이션 또는 사용자 지정 애니메이션을 사용 하 여 작업을 실행할 수 있습니다는 `ActivityOptions` 클래스입니다. 다음의 새 메서드가 이러한 애니메이션을 지원 하기 위해 제공 됩니다.

-   `MakeScaleUpAnimation` -시작 위치와 크기를 화면에서에서 활동 창을 확장 하는 애니메이션을 만들어집니다.
-   `MakeThumbnailScaleUpAnimation` –이 화면에 지정 된 위치에서 썸네일 이미지에서 규모 확장 하는 애니메이션을 만들어집니다.
-   `MakeCustomAnimation` –이 응용 프로그램 리소스에서 애니메이션을 만듭니다. 활동 열리면에 대 한 애니메이션 및 다른 작업을 중지할 시기에 대 한가


새 `TimeAnimator` 인터페이스를 제공 하는 클래스 `TimeAnimator.ITimeListener` 프레임 애니메이션에 변경 될 때마다 응용 프로그램에 알릴 수입니다. 예를 들어 다음 구현의 `TimeAnimator.ITimeListener`:

```csharp
class MyTimeListener : Java.Lang.Object,  TimeAnimator.ITimeListener
{
    public void OnTimeUpdate(TimeAnimator animation, long totalTime, long deltaTime)
    {
        Log.Debug("Activity1", "totalTime={0}, deltaTime={1}", totalTime, deltaTime);
    }
}
```

그리고 클래스의 인스턴스를 사용 하는 이제 `TimeAnimator` 만들어지면 수신기 설정 됩니다.

```csharp
var animator = new TimeAnimator();
animator.SetTimeListener(new MyTimeListener());
animator.Start();
```

로 `TimeAnimator` 인스턴스가 실행 되 고, 호출 하는 `ITimeAnimator.ITimeListener`, 방법을 다음 로깅됩니다 애니메이터가 장기 실행 및 기간 해당 메서드를 마지막으로 이후 된 대로 호출 된 되었습니다.



### <a name="application-stack-navigation"></a>응용 프로그램 스택 탐색

Android 4.1 Android 3.0에 도입 된 응용 프로그램 스택 탐색을 개선 합니다. 지정 하 여는 `ParentName` 의 속성을 `ActivityAttribute`, Android를 누를 때 적절 한 부모 활동을 열 수를 [위로 단추를 클릭](https://developer.android.com/design/patterns/navigation.html#up-vs-back) 작업 모음의-Android 합니다 하여지정된활동인스턴스화`ParentName`속성입니다. 따라서 특정된 작업 하는 작업의 계층 구조를 유지 하기 위해 응용 프로그램.

대부분의 응용 프로그램 설정의 `ParentName` 활동에는 응용 프로그램 스택을 탐색 하는 것에 대 한 올바른 동작을 제공 하는 Android에 대 한 충분 한 정보 Android는 각 부모 활동에 대 한 일련의 의도 만들어 필요한 백 스택에 합성 됩니다. 그러나, 인위적인 응용 프로그램 스택을 이기 때문에 각 가상 작업 자연 스러운 활동을 포함 하는 저장된 된 상태가 제공 되지 않습니다. 가상 부모 활동에 저장 된 상태를 제공 하려면 작업 보다 우선 합니다 `OnPrepareNavigationUpTaskStack` 메서드. 이 메서드는 수신는 `TaskStackBuilder` 의도의 컬렉션을 갖게 되는 인스턴스는 Android 백 스택에 만드는 데 사용할 개체입니다. 가상 작업을 만들 때 적절 한 상태 정보를 받이 되도록 작업에서 이러한 의도 수정할 수 있습니다.

복잡 한 시나리오에 대 한 가지가 새 설치의 동작을 처리 하 고 백 스택을 생성 하는 데 사용할 수 있는 활동 클래스에:

-   `OnNavigateUp` –이 메서드를 재정의 하 여 사용자 지정 작업을 수행할 수 있기 때를 <span class="ui">등록</span> 단추가 눌러져 있습니다.
-   `NavigateUpTo` –이 메서드를 호출 하면 현재 활동의 지정된 여부는 지정 된 활동으로 이동 하려면 응용 프로그램.
-   `ParentActivityIntent` –이 현재 활동의 부모 활동을 시작 하는 의도 가져오려면 사용 됩니다.
-   `ShouldUpRecreateTask` –이 메서드는 부모 활동까지 이동할 가상 백 스택에 만들어야 할 쿼리 사용 됩니다. 반환 `true` 가상 스택 만들어야 하는 경우. 
-   `FinishAffinity` –이 메서드를 호출 합니다. 현재 활동 및 그 아래의 현재 작업의 하는 모든 작업이 동일한 작업 선호도 완료 됩니다.
-   `OnCreateNavigateUpTaskStack` –이 메서드는 가상 스택 만들어지는 방법을 완전히 제어 하는 데 필요한 경우 재정의 됩니다.




### <a name="camera"></a>카메라

새 인터페이스가 `Camera.IAutoFocusMoveCallback`, 자동 포커스를 시작 또는 이동을 중지 시기를 감지 하 사용할 수 있는 합니다. 다음 코드 조각과에서이 새 인터페이스의 예제를 확인할 수 있습니다.

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

새 클래스 `MediaActionSound` 다양 한 미디어 작업에 대 한 소리를 생성 하는 것에 대 한 API의 집합을 제공 합니다. 열거형에서 정의 되며, 카메라를 사용 하 여 발생할 수 있는 몇 가지 작업이 있습니다 `Android.Media.MediaActionSoundType`:

-   `MediaActionSoundType.FocusComplete` –이 소리를 집중 완료 하는 경우에 재생 됩니다.
-   `MediaActionSoundType.ShutterClick` –이 소리는 여전히 이미지 그림을 만들 때 재생 됩니다.
-   `MediaActionSoundType.StartVideoRecording` –이 소리는 비디오 녹화 시작을 표시 합니다.
-   `MediaActionSoundType.StopVideoRecording` – 비디오 기록의 끝을 나타내는이 소리를 재생 됩니다.


사용 하는 방법의 예로 `MediaActionSound` 클래스는 다음 코드 조각에서 볼 수 있습니다.

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



#### <a name="android-beam"></a>Android Beam

Android 보는 두 개의 Android 장치가 서로 통신할 수 있도록 NFC 기반 기술입니다. Android 4.1 큰 파일 전송에 대 한 더 나은 지원을 제공합니다. 새 메서드를 사용 하는 경우 `NfcAdapter.SetBeamPushUris()` Android 빠른 전송 속도 달성 하기 위해 대체 전송 메커니즘 (예: Bluetooth) 간을 전환 합니다.



#### <a name="network-services-discovery"></a>네트워크 서비스 검색

Android 4.1 새 API의 DNS 기반 멀티 캐스트 서비스 검색을 포함합니다.
이렇게 하면 응용 프로그램을을 검색 하 고 프린터, 카메라, 미디어 장치 등의 다른 장치에서 Wi-fi를 통해 연결할 수 있습니다. 이러한 새 API에는 `Android.Net.Nsd` 패키지 있습니다.

다른 서비스에 사용 될 수 있는 서비스를 만들려는 `NsdServiceInfo` 클래스는 서비스의 속성을 정의 하는 개체를 만드는 데 사용 됩니다. 이 개체에 제공 됩니다 `NsdManager.RegisterService()` 구현의 함께 `NsdManager.ResolveListener`입니다. 구현의 `NsdManager.ResolveListener` 알릴 성공적으로 등록 하 고 서비스 등록을 취소 하려면 사용 됩니다.

네트워크 및 구현에는 서비스를 검색할 `Nsd.DiscoveryListener` 전달할 `NsdManager.discoverServices()`합니다.



#### <a name="network-usage"></a>네트워크 사용량

새 메서드인 `ConnectivityManager.IsActiveNetworkMetered` 요금제 네트워크에 연결 되어 있는지 확인 하려면 장치를 허용 합니다. 정확 하 게 데이터 작업에 대 한 비용이 많이 드는 요금은 있을 수 있음을 사용자에 게 알리는에서 데이터 사용량을 관리할 수 있도록이 메서드를 사용할 수 있습니다.



#### <a name="wifi-direct-service-discovery"></a>WiFi 직접 서비스 검색

합니다 `WifiP2pManager` 클래스를 지원 하도록 Android 4.0에 도입 *zeroconf*합니다. Zeroconf (구성 네트워킹 0)는 특별 한 구성이 서버나 휴먼 네트워크 운영자의 개입을 사용 하 여 장치 (컴퓨터, 프린터, 휴대폰) 네트워크에 자동으로 연결할 수 있는 기술의 집합입니다.

Jelly Bean `WifiP2pManager` 주변 장치 중 하나를 사용 하 여 검색할 수 있습니다 *Bonjour* 하거나 *Upnp*합니다. Bonjour는 zeroconf의 Apple의 구현입니다. Upnp은 zeroconf 지 네트워킹 프로토콜 설정 됩니다. 에 추가 된 다음 메서드는 `WiFiP2pManager` Wi-fi 서비스 검색을 지 원하는:

-   `AddLocalService()` –이 메서드는 피어에서 검색에 대 한 Wi-fi 상에 서 서비스로 응용 프로그램을 발표 합니다.
-   `AddServiceRequest(` )-이 메서드는 프레임 워크 서비스 검색 요청을 보냅니다. Wi-fi 서비스 검색을 초기화 하는 것이 됩니다.
-   `SetDnsSdResponseListeners()` –이 메서드는 Bonjour에서 검색 요청에 대 한 응답을 받으면 호출할 콜백을 등록에 사용 됩니다.
-   `SetUpnpServiceResponseListener()` –이 메서드는 Upnp 검색 요청에 대 한 응답을 받으면 호출할 콜백을 등록에 사용 됩니다.




### <a name="content-providers"></a>콘텐츠 공급자

합니다 `ContentResolver` 클래스에는 새 메서드를 받았습니다 `AcquireUnstableContentProvider`합니다. 이 메서드를 사용 하면 응용 프로그램 "불안정" 콘텐츠 공급자를 가져오려고 합니다. 일반적으로 응용 프로그램 콘텐츠 공급자를 획득 하 고 해당 콘텐츠 공급자 작동이 중단 하는 경우 응용 프로그램 이므로 됩니다. 이 메서드 호출을 사용 하 여 콘텐츠 공급자 크래시 되는 경우 응용 프로그램 중단 되지 됩니다. 대신 `Android.OS.DeadObjectionException` 콘텐츠 공급자 사라진 하는 응용 프로그램에 알림을 보내야 콘텐츠 공급자에 대 한 호출에서 throw 됩니다. 다른 응용 프로그램에서 콘텐츠 공급자와 상호 작용할 때 "불안정" 콘텐츠 공급자로 유용 합니다.-다른 응용 프로그램에서 버그가 있는 코드는 다른 응용 프로그램에 영향을 발생할 가능성이 줄어듭니다.



### <a name="copy-and-paste-with-intents"></a>인 텐트도 복사 및 붙여넣기

`Intent` 클래스는 이제는 `ClipData` 통해 연관 된 개체는 `Intent.ClipData` 속성입니다. 이 메서드를 사용 하면 추가 클립보드의에서 데이터를 의도 사용 하 여 전송할 수입니다. 인스턴스의 `ClipData` 하나 이상 포함 될 수 있습니다 `ClipData.Item`합니다. `ClipData.Item`항목이 다음 형식 중:

-   **텍스트** –이 두 HTML 텍스트 문자열 또는 문자열 형식의 기본 제공 Android 스타일 지에 걸쳐 있습니다.
-  **의도** – 모든 `Intent` 개체입니다.
-   **Uri** –이 콘텐츠 공급자에 대 한 URI는 HTTP 책갈피 등 모든 URI를 수 있습니다.




### <a name="isolated-services"></a>격리 된 서비스

격리 된 서비스는 서비스 특별 한 자체 프로세스에서 실행 되는 자체 권한이 없습니다. 경우는 서비스와만 통신 서비스를 시작 하 고 서비스 API를 통해 바인딩할 합니다. 속성을 설정 하 여 격리와 서비스를 선언 하는 것이 불가능 `IsolatedProcess="true"` 에 `ServiceAttribute` 는 서비스 클래스를 보여 합니다.


### <a name="media"></a>미디어

새 `Android.Media.MediaCodec` 클래스는 하위 수준 미디어 코덱 하기 위한 API를 제공 합니다. 응용 프로그램을 장치에서 사용할 수 있는 낮은 수준의 코덱을 확인 하려면 시스템을 쿼리할 수 있습니다.

새 `Android.Media.Audiofx.AudioEffect` 서브 클래스 추가 오디오 미리 캡처된 오디오에 대 한 처리를 지원 하기 위해 추가 되었습니다.

-   `Android.Media.Audiofx.AcousticEchoCanceler` -이 클래스는 신호를 캡처된 오디오 신호에서 원격 상대방에서 제거할 오디오를 사전 처리 하는 것에 대 한 사용 됩니다. 예를 들어, 음성 통신 응용 프로그램에서 에코를 제거합니다.
-   `Android.Media.Audiofx.AutomaticGainControl` -이 클래스는 캡처된 신호 승격 하거나 출력 신호는 상수 입력된 신호를 낮추어 정규화 사용 됩니다.
-   `Android.Media.Audiofx.NoiseSuppressor` –이 클래스는 백그라운드 노이즈 캡처된 신호에서 제거 됩니다.


모든 장치는 이러한 효과 지원 합니다. 메서드가 `AudioEffect.IsAvailable` 응용 프로그램을 실행 하는 장치에서 해당 오디오 효과 지원 하는 경우 응용 프로그램에서 호출 해야 합니다.

합니다 `MediaPlayer` 클래스는 이제 사용 하 여 매끄러운 재생을 `SetNextMediaPlayer()` 메서드. 이 새 메서드는 현재 미디어 플레이어의 재생을 완료 하는 경우 시작 하려면 다음 MediaPlayer를 지정 합니다.

다음 새 클래스를 미디어를 재생 하는 위치를 선택 하기 위한 표준 메커니즘 및 UI를 제공 합니다.

-   `MediaRouter` -이 클래스는 외부 스피커로 장치나 다른 장치에서 미디어 채널의 라우팅을 제어 하는 응용 프로그램을 허용 합니다.
-   `MediaRouterActionProvider` 및 `MediaRouteButton` – 이러한 클래스를 선택 하 고 미디어 재생에 대 한 일관 된 UI를 제공할 수 있습니다.




### <a name="notifications"></a>알림

Android 4.1 컨트롤 알림을 표시 하 고 유연 하 게 응용 프로그램을 허용 합니다. 이제 응용 프로그램 사용자에 게 더 크고 향상 알림을 표시할 수 있습니다. 새 메서드인 `NotificationBuilder.SetStyle()` 새로운 3 개의 새 스타일 중 하나에 대 한 알림을 설정할 수 있습니다.

-   `Notification.BigPictureStyle` – 이미지에 갖게 되는 알림을 생성 하는 도우미 클래스입니다. 다음 이미지는 큰 이미지를 사용 하 여 알림의 예를 보여 줍니다.


 [![BigPictureStyle 알림의 예제 스크린 샷](jelly-bean-images/image2.png)](jelly-bean-images/image2.png#lightbox)

-   `Notification.BigTextStyle` -여러 줄의 텍스트, 전자 메일과 같은 갖게 되는 알림을 생성 하는 도우미 클래스입니다. 이 새 알림 스타일의 예는 다음 스크린샷에 확인할 수 있습니다.


 [![BigTextStyle 알림의 예제 스크린 샷](jelly-bean-images/image3.png)](jelly-bean-images/image3.png#lightbox)

-   `Notification.InboxStyle` –이 스크린샷에 표시 된 대로 전자 메일 메시지의 조각 같은 문자열의 목록을 포함 하는 알림을 생성 하는 도우미 클래스입니다.


 [![Notification.InboxStyle 알림의 예제 스크린 샷](jelly-bean-images/image4.png)](jelly-bean-images/image4.png#lightbox)

알림은 보통 또는 큰 스타일을 사용 하는 경우 알림 메시지의 맨 아래에 최대 두 개의 실행 단추를 추가 하는 것이 가능 합니다.
이 예제 실행 단추 알림의 맨 아래에 표시 되는 다음 스크린샷에 확인할 수 있습니다.

 [![알림 메시지를 아래에 표시 되는 작업 단추의 스크린샷 예제](jelly-bean-images/image5.png)](jelly-bean-images/image5.png#lightbox)

`Notification` 클래스를 사용 하면 개발자는 알림에 대 한 5 가지 우선 순위 수준 중 하나를 지정 하는 새 상수 받았습니다. 사용 하 여 알림을 설정할 수 있습니다 이러한 합니다 `Priority` 속성입니다.



### <a name="permissions"></a>사용 권한

다음 새 권한이 추가 되었습니다.

-   `READ_EXTERNAL_STORAGE` -응용 프로그램에는 외부 저장소에 읽기 전용 액세스가 필요합니다. 모든 응용 프로그램 현재 기본적으로 읽기 액세스 권한이 있지만 이후 릴리스의 Android 응용 프로그램 읽기 액세스를 명시적으로 요청 해야 합니다.
-   `READ_USER_DICTIONARY` -사용자의 word 사전에 읽기 액세스를 허용 합니다.
-   `READ_CALL_LOG` -응용 프로그램을을 호출 로그를 읽는 여 들어오고 나가는 호출에 대 한 정보를 얻을 수 있습니다.
-   `WRITE_CALL_LOG` -응용 프로그램을을 휴대폰에서 통화 로그를 쓸 수 있습니다.
-   `WRITE_USER_DICTIONARY` -응용 프로그램을 사용자의 word 사전에 쓸 수 있습니다.


확인에 대 한 중요 변경 `READ_EXTERNAL_STORAGE` – Android에서이 권한이 자동으로 부여 되는 현재 합니다. 이후 버전의 Android 권한을 부여 하기 전에이 권한을 요청 하는 응용 프로그램에 게 해야 합니다.



## <a name="summary"></a>요약

이 문서에서는 새 api의 Android 4.1 (API 레벨 16)에서 사용할 수 있는 소개 했습니다. 애니메이션 및 활동 시작 애니메이션에 대 한 변경 내용 중 일부를 강조 표시 하 고 새 API Bonjour 또는 UPnP와 같은 프로토콜을 사용 하 여 기타 장치의 네트워크 검색을 도입 합니다. 다른 API의 변경 내용 된 잘라내기 및 붙여넣기 의도 통해 데이터 격리 된 서비스 또는 "불안정" 콘텐츠 공급자를 사용 하는 기능 등도 강조 표시 됩니다.

이 문서에서는 다음 업데이트 알림을 소개 발생 하 고 Android 4.1을 사용 하 여 도입 된 새로운 권한 중 일부를 설명


## <a name="related-links"></a>관련 링크

- [시간에 애니메이션 예제 (샘플)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TimeAnimatorExample/)
- [Android 4.1 Api](https://developer.android.com/about/versions/android-4.1.html)
- [작업 및 백 스택](https://developer.android.com/guide/components/tasks-and-back-stack.html)
- [뒤로 / 위쪽으로 탐색](https://developer.android.com/design/patterns/navigation.html)
