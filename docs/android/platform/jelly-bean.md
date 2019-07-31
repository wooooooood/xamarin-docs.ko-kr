---
title: Jelly Bean 기능
description: 이 문서에서는 Android 4.1에 도입 된 개발자를 위한 새로운 기능에 대 한 개략적인 개요를 제공 합니다. 이러한 기능으로는 향상 된 알림, Android 무선 업데이트를 사용 하 여 대량 파일 공유, 멀티미디어 업데이트, 피어 투 피어 네트워크 검색, 애니메이션, 새 권한 등이 있습니다.
ms.prod: xamarin
ms.assetid: 23F57634-2EF9-5C15-C710-B3E19A5AF7E1
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: e54f499316d2b99d87d05fbd202308eecaaed220
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68643366"
---
# <a name="jelly-bean-features"></a>Jelly Bean 기능

_이 문서에서는 Android 4.1에 도입 된 개발자를 위한 새로운 기능에 대 한 개략적인 개요를 제공 합니다. 이러한 기능으로는 향상 된 알림, Android 무선 업데이트를 사용 하 여 대량 파일 공유, 멀티미디어 업데이트, 피어 투 피어 네트워크 검색, 애니메이션, 새 권한 등이 있습니다._



## <a name="overview"></a>개요

Android 4.1 (API 수준 16) ("Jelly Bean"이 라고도 함)은 2012 년 7 월 9 일에 릴리스 되었습니다. 이 문서에서는 Xamarin. Android를 사용 하는 개발자를 위한 Android 4.1의 새로운 기능 중 일부에 대 한 개략적인 소개를 제공 합니다. 새로 도입 된 이러한 기능 중 일부는 작업을 시작 하기 위한 애니메이션, 카메라에 대 한 새 소리, 응용 프로그램 스택 탐색을 위한 향상 된 지원에 대 한 향상 된 기능입니다. 이제를 사용 하 여 잘라내어 붙여 넣을 수 있습니다.

안정적인 콘텐츠 공급자에 대 한 종속성을 격리 하는 기능으로 Android 응용 프로그램의 안정성이 향상 되었습니다. 서비스를 시작한 활동 에서만 액세스할 수 있도록 서비스를 격리할 수도 있습니다.

Bonjour, UPnP 또는 멀티 캐스트 DNS 기반 서비스를 사용 하는 네트워크 서비스 검색에 대 한 지원이 추가 되었습니다. 이제 서식이 지정 된 텍스트, 작업 단추 및 커다란 이미지를 포함 하는 다양 한 알림이 가능 합니다.

마지막으로 Android 4.1에 몇 가지 새로운 권한이 추가 되었습니다.



## <a name="requirements"></a>요구 사항

Jelly Bean을 사용 하 여 Xamarin Android 응용 프로그램을 개발 하려면 다음 스크린샷에 표시 된 것 처럼 Xamarin. Android 4.2.6 이상 및 Android 4.1 (API 수준 16)을 Android SDK Manager를 통해 설치 해야 합니다.

[![Android SDK Manager에서 Android 4.1 선택](jelly-bean-images/image1.png)](jelly-bean-images/image1.png#lightbox)



## <a name="whats-new"></a>새로운 기능



### <a name="animations"></a>애니메이션

작업은 클래스를 `ActivityOptions` 사용 하 여 확대/축소 애니메이션이 나 사용자 지정 애니메이션을 사용 하 여 시작할 수 있습니다. 이러한 애니메이션을 지원 하기 위해 다음과 같은 새로운 메서드가 제공 됩니다.

-   `MakeScaleUpAnimation`– 작업 창을 화면에서 시작 위치와 크기에서 확장 하는 애니메이션을 만듭니다.
-   `MakeThumbnailScaleUpAnimation`– 화면에서 지정 된 위치에서 축소판 이미지를 확대 하는 애니메이션을 만듭니다.
-   `MakeCustomAnimation`– 응용 프로그램의 리소스에서 애니메이션을 만듭니다. 활동을 열 때와 활동이 중지 될 때에 대해 다른 애니메이션이 하나씩 있습니다.


새 `TimeAnimator` 클래스는 애니메이션에서 프레임이 `TimeAnimator.ITimeListener` 변경 될 때마다 응용 프로그램에 알릴 수 있는 인터페이스를 제공 합니다. 예를 들어 다음 구현을 `TimeAnimator.ITimeListener`고려 하십시오.

```csharp
class MyTimeListener : Java.Lang.Object,  TimeAnimator.ITimeListener
{
    public void OnTimeUpdate(TimeAnimator animation, long totalTime, long deltaTime)
    {
        Log.Debug("Activity1", "totalTime={0}, deltaTime={1}", totalTime, deltaTime);
    }
}
```

이제 클래스를 사용 하기 위해 인스턴스가 `TimeAnimator` 만들어지고 수신기가 설정 됩니다.

```csharp
var animator = new TimeAnimator();
animator.SetTimeListener(new MyTimeListener());
animator.Start();
```

인스턴스가 실행 되는 동안는를 호출 `ITimeAnimator.ITimeListener`하며,이는 애니메이터가 실행 된 시간 및 마지막으로 메서드가 호출 된 이후 경과한 시간을 기록 합니다. `TimeAnimator`



### <a name="application-stack-navigation"></a>응용 프로그램 스택 탐색

Android 4.1는 Android 3.0에 도입 된 응용 프로그램 스택 탐색을 개선 합니다. Android는 `ParentName` `ParentName` 의 속성을 지정 하 여 사용자가 작업 모음에서 [위로 단추](https://developer.android.com/design/patterns/navigation.html#up-vs-back) 를 누를 때 적절 한 부모 작업을 열 수 있습니다.-android는 속성에 지정 된 작업을 인스턴스화합니다. `ActivityAttribute` 이렇게 하면 응용 프로그램에서 지정 된 작업을 수행 하는 작업 계층 구조를 유지할 수 있습니다.

대부분의 응용 프로그램에서 `ParentName` 작업에 대 한를 설정 하면 Android에서 응용 프로그램 스택을 탐색 하는 데 올바른 동작을 제공 하는 데 충분 한 정보가 제공 됩니다. Android는 각 부모 작업에 대해 일련의 의도를 만들어 필요한 백 스택을 합성 합니다. 그러나이는 인공 응용 프로그램 스택 이므로 각 가상 활동에는 자연 활동의 저장 된 상태가 포함 되지 않습니다. 가상 부모 작업에 저장 된 상태를 제공 하기 위해 작업은 메서드를 `OnPrepareNavigationUpTaskStack` 재정의할 수 있습니다. 이 메서드는 Android `TaskStackBuilder` 에서 백 스택을 만드는 데 사용 하는 의도 개체의 컬렉션을 포함 하는 인스턴스를 수신 합니다. 활동은 가상 활동이 만들어질 때 적절 한 상태 정보를 받을 수 있도록 이러한 의도를 수정할 수 있습니다.

더 복잡 한 시나리오의 경우 활동 클래스에 탐색의 동작을 처리 하 고 백 스택을 생성 하는 데 사용할 수 있는 새로운 메서드가 있습니다.

-   `OnNavigateUp`–이 메서드를 재정의 하면 <span class="ui">위로</span> 단추를 누를 때 사용자 지정 작업을 수행할 수 있습니다.
-   `NavigateUpTo`–이 메서드를 호출 하면 응용 프로그램이 현재 활동에서 지정 된 의도에 지정 된 활동으로 이동 합니다.
-   `ParentActivityIntent`– 현재 활동의 부모 활동을 시작할 의도를 가져오는 데 사용 됩니다.
-   `ShouldUpRecreateTask`–이 메서드는 부모 작업까지 탐색 하기 위해 가상 뒤로 스택을 만들어야 하는지 여부를 쿼리 하는 데 사용 됩니다. 가상 `true` 스택을 만들어야 하는 경우를 반환 합니다. 
-   `FinishAffinity`–이 메서드를 호출 하면 현재 작업과 작업 선호도가 같은 현재 작업에서 해당 작업 아래에 있는 모든 작업이 완료 됩니다.
-   `OnCreateNavigateUpTaskStack`–이 메서드는 가상 스택을 만드는 방법을 완전히 제어 해야 하는 경우에 재정의 됩니다.




### <a name="camera"></a>카메라

자동 포커스가 시작 되거나 이동이 `Camera.IAutoFocusMoveCallback`중지 된 시기를 검색 하는 데 사용할 수 있는 새 인터페이스가 있습니다. 다음 코드 조각에서이 새 인터페이스의 예제를 볼 수 있습니다.

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

새 클래스 `MediaActionSound` 는 다양 한 미디어 동작에 대 한 소리를 생성 하기 위한 API 집합을 제공 합니다. 카메라에서 발생할 수 있는 몇 가지 작업은 열거형 `Android.Media.MediaActionSoundType`으로 정의 됩니다.

-   `MediaActionSoundType.FocusComplete`– 포커스를 완료할 때 재생 되는 소리입니다.
-   `MediaActionSoundType.ShutterClick`–이 소리는 이미지 그림을 만들 때 재생 됩니다.
-   `MediaActionSoundType.StartVideoRecording`–이 소리는 비디오 녹음의 시작을 나타내는 데 사용 됩니다.
-   `MediaActionSoundType.StopVideoRecording`–이 소리는 비디오 녹음의 끝을 나타내기 위해 재생 됩니다.


클래스를 `MediaActionSound` 사용 하는 방법에 대 한 예제는 다음 코드 조각에서 확인할 수 있습니다.

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

Android 무선은 두 Android 장치가 서로 통신할 수 있도록 하는 NFC 기반 기술입니다. Android 4.1은 많은 파일의 전송에 대 한 향상 된 지원을 제공 합니다. 새 메서드 `NfcAdapter.SetBeamPushUris()` 를 사용 하는 경우 Android는 빠른 전송 속도를 얻기 위해 대체 전송 메커니즘 (예: Bluetooth) 간을 전환 합니다.



#### <a name="network-services-discovery"></a>Network Services 검색

Android 4.1에는 멀티 캐스트 DNS 기반 서비스 검색용 새 API가 포함 되어 있습니다.
이를 통해 응용 프로그램은 Wi-fi를 통해 프린터, 카메라 및 미디어 장치와 같은 다른 장치에 대 한 Wi-fi를 검색 하 고 연결할 수 있습니다. 이러한 새 API는 `Android.Net.Nsd` 패키지에 있습니다.

다른 서비스 `NsdServiceInfo` 에서 사용 될 수 있는 서비스를 만들기 위해 클래스를 사용 하 여 서비스의 속성을 정의 하는 개체를 만듭니다. 그런 다음이 개체는의 `NsdManager.RegisterService()` `NsdManager.ResolveListener`구현과 함께에 제공 됩니다. 의 `NsdManager.ResolveListener` 구현은 성공적인 등록을 알리고 서비스 등록을 취소 하는 데 사용 됩니다.

네트워크에서 서비스를 검색 하 고에 `Nsd.DiscoveryListener` `NsdManager.discoverServices()`전달 되는 구현입니다.



#### <a name="network-usage"></a>네트워크 사용량

새 방법으로 `ConnectivityManager.IsActiveNetworkMetered` 장치에서 요금제 네트워크에 연결 되어 있는지 확인할 수 있습니다. 이 메서드를 사용 하 여 데이터 작업에 비용이 많이 들 수 있음을 사용자에 게 정확 하 게 알려 데이터 사용을 쉽게 관리할 수 있습니다.



#### <a name="wifi-direct-service-discovery"></a>WiFi Direct 서비스 검색

클래스 `WifiP2pManager` 는 *zeroconf*을 지원 하기 위해 Android 4.0에서 도입 되었습니다. Zeroconf (제로 구성 네트워킹)은 장치 (컴퓨터, 프린터, 휴대폰)가 네트워크에 자동으로 연결할 수 있도록 하는 일련의 기술입니다 .이는 휴먼 네트워크 운영자 또는 특수 구성 서버를 사용 하는 것입니다.

Jelly Bean에서는 `WifiP2pManager` *Bonjour* 또는 *Upnp*를 사용 하 여 주변 장치를 검색할 수 있습니다. Bonjour는 Apple의 zeroconf 구현입니다. Upnp는 zeroconf도 지 원하는 네트워킹 프로토콜의 집합입니다. Wi-fi 서비스 검색 `WiFiP2pManager` 을 지원 하기 위해에 추가 된 메서드는 다음과 같습니다.

-   `AddLocalService()`–이 메서드는 피어에서 검색을 위해 Wi-fi를 통해 응용 프로그램을 서비스로 알리기 위해 사용 됩니다.
-   `AddServiceRequest(`) –이 메서드는 서비스 검색 요청을 프레임 워크에 보내는 것입니다. Wi-fi 서비스 검색을 초기화 하는 데 사용 됩니다.
-   `SetDnsSdResponseListeners()`–이 메서드는 Bonjour의 검색 요청에 대 한 응답을 받을 때 호출 되는 콜백을 등록 하는 데 사용 됩니다.
-   `SetUpnpServiceResponseListener()`–이 메서드는 Upnp를 검색 요청에 대 한 응답을 받을 때 호출 되는 콜백을 등록 하는 데 사용 됩니다.




### <a name="content-providers"></a>콘텐츠 공급자

클래스가 새 메서드인를 `AcquireUnstableContentProvider`받았습니다. `ContentResolver` 이 방법을 사용 하면 응용 프로그램에서 "불안정 한" 콘텐츠 공급자를 가져올 수 있습니다. 일반적으로 응용 프로그램에서 콘텐츠 공급자를 획득 하 고 해당 콘텐츠 공급자가 충돌 하면 응용 프로그램이 됩니다. 이 메서드를 호출 하면 콘텐츠 공급자가 충돌 하는 경우 응용 프로그램이 작동 하지 않습니다. 대신 콘텐츠 공급자에 대 한 호출에서 콘텐츠 공급자가 사라졌습니다 .를 응용 프로그램에 알리기 위해가 throw 됩니다.`Android.OS.DeadObjectionException` "불안정 한" 콘텐츠 공급자는 다른 응용 프로그램의 콘텐츠 공급자와 상호 작용할 때 유용 합니다. 다른 응용 프로그램의 버그가 있는 코드가 다른 응용 프로그램에 영향을 줄 가능성이 낮습니다.



### <a name="copy-and-paste-with-intents"></a>의도를 사용 하 여 복사 및 붙여넣기

이제 `Intent` 클래스는 `Intent.ClipData` 속성을 통해 `ClipData` 개체와 연결 된 개체를 가질 수 있습니다. 이 메서드를 사용 하면 클립보드의 추가 데이터를 의도를 사용 하 여 전송할 수 있습니다. 인스턴스에 `ClipData` 는를 하나 `ClipData.Item`이상 포함할 수 있습니다. `ClipData.Item`는 다음 형식의 항목입니다.

-   **Text** – 기본 제공 Android 스타일 범위에서 지원 되는 형식의 텍스트 또는 HTML 또는 문자열입니다.
-  **의도** -모든 `Intent` 개체입니다.
-   **Uri** – HTTP 책갈피 또는 콘텐츠 공급자에 대 한 uri와 같은 모든 uri가 될 수 있습니다.




### <a name="isolated-services"></a>Isolated 서비스

Isolated 서비스는 자체의 특별 한 프로세스에서 실행 되 고 자체의 권한은 없는 서비스입니다. 서비스와의 통신은 서비스를 시작 하 고 서비스 API를 통해 해당 서비스에 바인딩하는 경우에만 사용할 수 있습니다. 서비스 클래스를 원으로 `IsolatedProcess="true"` `ServiceAttribute` 하는의 속성을 설정 하 여 서비스를 격리 된 것으로 선언할 수 있습니다.


### <a name="media"></a>미디어

새 `Android.Media.MediaCodec` 클래스는 하위 수준 미디어 코덱에 API를 제공 합니다. 응용 프로그램은 시스템을 쿼리하여 장치에서 사용 가능한 낮은 수준의 코덱을 확인할 수 있습니다.

캡처된 오디오 `Android.Media.Audiofx.AudioEffect` 에서 추가 오디오 전처리를 지원 하기 위해 새 서브 클래스가 추가 되었습니다.

-   `Android.Media.Audiofx.AcousticEchoCanceler`–이 클래스는 캡처된 오디오 신호에서 원격 파티의 신호를 제거 하는 오디오를 전처리 하는 데 사용 됩니다. 예를 들어 음성 통신 응용 프로그램에서 에코를 제거 합니다.
-   `Android.Media.Audiofx.AutomaticGainControl`–이 클래스는 출력 신호가 일정 하도록 입력 신호를 늘리거나 줄여 캡처된 신호를 정규화 하는 데 사용 됩니다.
-   `Android.Media.Audiofx.NoiseSuppressor`–이 클래스는 캡처된 신호에서 배경 노이즈를 제거 합니다.


모든 장치가 이러한 효과를 지 원하는 것은 아닙니다. 응용 프로그램 `AudioEffect.IsAvailable` 을 실행 하는 장치에서 문제의 오디오 효과가 지원 되는지 확인 하기 위해 응용 프로그램에서 메서드를 호출 해야 합니다.

이제 `MediaPlayer` 클래스는 `SetNextMediaPlayer()` 메서드를 사용 하 여 gapless 재생을 지원 합니다. 이 새 메서드는 현재 미디어 플레이어가 재생을 완료할 때 시작할 다음 MediaPlayer를 지정 합니다.

다음 새 클래스는 미디어를 재생할 위치를 선택 하기 위한 표준 메커니즘과 UI를 제공 합니다.

-   `MediaRouter`–이 클래스를 통해 응용 프로그램은 장치에서 외부 스피커나 기타 장치로의 미디어 채널 라우팅을 제어할 수 있습니다.
-   `MediaRouterActionProvider`및 `MediaRouteButton` – 이러한 클래스를 통해 미디어를 선택 하 고 재생 하는 일관 된 UI를 제공할 수 있습니다.




### <a name="notifications"></a>알림

Android 4.1을 사용 하면 응용 프로그램에서 보다 유연 하 고 알림을 표시 하 여 제어할 수 있습니다. 이제 응용 프로그램은 더 크고 더 나은 알림을 사용자에 게 표시할 수 있습니다. 새 메서드로, `NotificationBuilder.SetStyle()` 알림에 대해 새로운 세 가지 새 스타일 중 하나를 설정할 수 있습니다.

-   `Notification.BigPictureStyle`– 이미지를 포함 하는 알림을 생성 하는 도우미 클래스입니다. 다음 이미지는 큰 이미지를 사용 하는 알림의 예를 보여 줍니다.


 [![이상 및 스타일 알림의 예제 스크린샷](jelly-bean-images/image2.png)](jelly-bean-images/image2.png#lightbox)

-   `Notification.BigTextStyle`– 전자 메일과 같이 여러 줄의 텍스트를 포함 하는 알림을 생성 하는 도우미 클래스입니다. 다음 스크린샷에서는이 새 알림 스타일의 예를 볼 수 있습니다.


 [![이상 텍스트 알림 예제 스크린샷](jelly-bean-images/image3.png)](jelly-bean-images/image3.png#lightbox)

-   `Notification.InboxStyle`–이 스크린샷에 표시 된 것 처럼 전자 메일 메시지의 조각과 같은 문자열 목록을 포함 하는 알림을 생성 하는 도우미 클래스입니다.


 [![InboxStyle 알림의 예제 스크린샷](jelly-bean-images/image4.png)](jelly-bean-images/image4.png#lightbox)

알림이 정상 또는 더 큰 스타일을 사용 하는 경우 알림 메시지의 아래쪽에 최대 2 개의 작업 단추를 추가할 수 있습니다.
이에 대 한 예는 다음 스크린샷에서 볼 수 있습니다. 여기서 작업 단추는 알림 아래쪽에 표시 됩니다.

 [![알림 메시지 아래에 표시 되는 작업 단추의 예 스크린샷](jelly-bean-images/image5.png)](jelly-bean-images/image5.png#lightbox)

클래스 `Notification` 는 개발자가 알림의 5 가지 우선 순위 수준 중 하나를 지정할 수 있도록 하는 새 상수를 받았습니다. 이러한 속성은 속성을 `Priority` 사용 하 여 알림에 설정할 수 있습니다.



### <a name="permissions"></a>사용 권한

다음과 같은 새 사용 권한이 추가 되었습니다.

-   `READ_EXTERNAL_STORAGE`-응용 프로그램에는 외부 저장소에 대 한 읽기 전용 액세스 권한이 필요 합니다. 현재 모든 응용 프로그램에는 기본적으로 읽기 권한이 있지만 이후 Android 릴리스에서는 응용 프로그램이 명시적으로 읽기 액세스를 요청 해야 합니다.
-   `READ_USER_DICTIONARY`-사용자의 단어 사전에 대 한 읽기 액세스를 허용 합니다.
-   `READ_CALL_LOG`-호출 로그를 읽어 응용 프로그램에서 들어오고 나가는 호출에 대 한 정보를 가져올 수 있습니다.
-   `WRITE_CALL_LOG`-응용 프로그램이 휴대폰의 통화 로그에 쓸 수 있도록 허용 합니다.
-   `WRITE_USER_DICTIONARY`-응용 프로그램이 사용자의 word 사전에 쓸 수 있도록 허용 합니다.


중요 한 변경 `READ_EXTERNAL_STORAGE` 사항-현재이 권한은 Android에서 자동으로 부여 됩니다. 이후 버전의 Android에서는 사용 권한을 부여 하기 전에 응용 프로그램에서이 권한을 요청 해야 합니다.



## <a name="summary"></a>요약

이 문서에서는 Android 4.1 (API 수준 16)에서 사용할 수 있는 몇 가지 새로운 API를 소개 했습니다. 애니메이션에 대 한 일부 변경 내용을 강조 표시 하 고 작업 시작에 애니메이션을 적용 하 고, Bonjour 또는 UPnP와 같은 프로토콜을 사용 하 여 다른 장치의 네트워크 검색을 위한 새 API를 도입 했습니다. 의도를 통해 데이터를 잘라내거나 붙여 넣는 기능, 격리 된 서비스 또는 "불안정 한" 콘텐츠 공급자를 사용 하는 기능 등 API에 대 한 기타 변경 내용도 강조 되었습니다.

이 문서에서는 알림에 대 한 업데이트를 소개 하 고 Android 4.1에 도입 된 새로운 사용 권한 중 일부에 대해 설명 했습니다.


## <a name="related-links"></a>관련 링크

- [시간 애니메이션 예제 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-timeanimatorexample)
- [Android 4.1 Api](https://developer.android.com/about/versions/android-4.1.html)
- [작업 및 뒤로 스택](https://developer.android.com/guide/components/tasks-and-back-stack.html)
- [뒤로 및 위로 탐색](https://developer.android.com/design/patterns/navigation.html)
