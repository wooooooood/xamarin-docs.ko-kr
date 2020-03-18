---
title: Jelly Bean 기능
description: 이 문서에서는 Android 4.1에 도입된 개발자를 위한 새로운 기능에 대한 개략적인 개요를 제공합니다. 이러한 기능으로는 향상된 알림, 대용량 파일을 공유하도록 Android Beam 업데이트, 멀티미디어 업데이트, 피어 투 피어 네트워크 검색, 애니메이션, 새 권한 등이 있습니다.
ms.prod: xamarin
ms.assetid: 23F57634-2EF9-5C15-C710-B3E19A5AF7E1
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: a638ccf7810c737faaeded7fcc98fcf657c85288
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027206"
---
# <a name="jelly-bean-features"></a>Jelly Bean 기능

_이 문서에서는 Android 4.1에 도입된 개발자를 위한 새로운 기능에 대한 개략적인 개요를 제공합니다. 이러한 기능으로는 향상된 알림, 대용량 파일을 공유하도록 Android Beam 업데이트, 멀티미디어 업데이트, 피어 투 피어 네트워크 검색, 애니메이션, 새 권한 등이 있습니다._

## <a name="overview"></a>개요

"Jelly Bean"이라고도 하는 Android 4.1(API 레벨 16)은 2012년 7월 9일에 릴리스되었습니다. 이 문서에서는 Xamarin.Android를 사용하는 개발자를 위한 Android 4.1의 새 기능 중 일부를 개략적으로 소개합니다. 새로 도입된 기능 중 일부는 작업을 시작하기 위한 향상된 애니메이션, 카메라의 새로운 사운드, 애플리케이션 스택 탐색을 위한 향상된 지원입니다. 이제 의도를 사용하여 자르고 붙여넣을 수 있습니다.

불안정한 콘텐츠 공급자에 대한 종속성을 격리하는 기능을 도입하여 Android 애플리케이션의 안정성이 향상되었습니다. 서비스를 시작한 작업에서만 서비스에 액세스할 수 있도록 서비스를 격리할 수도 있습니다.

Bonjour, UPnP 또는 멀티캐스트 DNS 기반 서비스를 사용하는 네트워크 서비스 검색 지원이 추가되었습니다. 이제 서식이 지정된 텍스트, 작업 단추 및 대형 이미지가 포함된 풍부한 알림이 가능합니다.

마지막으로 Android 4.1에는 몇 가지 새로운 권한이 추가되었습니다.

## <a name="requirements"></a>요구 사항

Jelly Bean을 사용하여 Xamarin.Android 애플리케이션을 개발하려면 다음 스크린샷과 같이 Android SDK 관리자를 통해 Xamarin.Android 4.2.6 이상 및 Android 4.1(API 레벨 16)을 설치해야 합니다.

[![Android SDK 관리자에서 Android 4.1 선택](jelly-bean-images/image1.png)](jelly-bean-images/image1.png#lightbox)

## <a name="whats-new"></a>새로운 기능

### <a name="animations"></a>애니메이션

`ActivityOptions` 클래스를 통해 확대/축소 애니메이션 또는 사용자 지정 애니메이션을 사용하여 작업을 시작할 수 있습니다. 이러한 애니메이션을 지원하기 위해 다음과 같은 새로운 메서드가 제공됩니다.

- `MakeScaleUpAnimation` – 화면의 시작 위치와 크기에서 작업 창을 확장하는 애니메이션을 만듭니다.
- `MakeThumbnailScaleUpAnimation` – 화면의 지정된 위치에서 축소판 이미지를 확대하는 애니메이션을 만듭니다.
- `MakeCustomAnimation` – 애플리케이션의 리소스에서 애니메이션을 만듭니다. 작업이 열리는 때와 관련된 애니메이션과 작업이 중지되는 때와 관련된 애니메이션이 각각 하나씩 있습니다.

새 `TimeAnimator` 클래스는 애니메이션에서 프레임이 변경될 때마다 애플리케이션에 알릴 수 있는 `TimeAnimator.ITimeListener` 인터페이스를 제공합니다. 예를 들어 다음 `TimeAnimator.ITimeListener` 구현을 생각해 봅시다.

```csharp
class MyTimeListener : Java.Lang.Object,  TimeAnimator.ITimeListener
{
    public void OnTimeUpdate(TimeAnimator animation, long totalTime, long deltaTime)
    {
        Log.Debug("Activity1", "totalTime={0}, deltaTime={1}", totalTime, deltaTime);
    }
}
```

이제 클래스를 사용하기 위해 다음과 같이 `TimeAnimator` 인스턴스가 만들어지고, 수신기가 설정됩니다.

```csharp
var animator = new TimeAnimator();
animator.SetTimeListener(new MyTimeListener());
animator.Start();
```

`TimeAnimator` 인스턴스는 실행되는 동안 `ITimeAnimator.ITimeListener`를 호출합니다. 그러면 애니메이터가 실행된 시간 및 메서드가 마지막으로 호출된 이후 경과한 시간이 기록됩니다.

### <a name="application-stack-navigation"></a>애플리케이션 스택 탐색

Android 4.1은 Android 3.0에 도입된 애플리케이션 스택 탐색을 개선했습니다. `ActivityAttribute`의 `ParentName` 속성을 지정하면 사용자가 작업 모음에서 [위로 단추](https://developer.android.com/design/patterns/navigation.html#up-vs-back)를 누를 때 Android에서 적절한 부모 작업을 열 수 있습니다. Android는 `ParentName` 속성에 지정된 작업을 인스턴스화합니다. 이렇게 하면 애플리케이션에 지정된 작업을 만드는 작업 계층 구조를 유지할 수 있습니다.

대부분의 애플리케이션은 작업에서 `ParentName`을 설정하는 것만으로도 Android가 애플리케이션 스택을 탐색하기 위한 올바른 동작을 제공하기에 충분한 정보가 됩니다. Android는 각 부모 작업에 대한 일련의 의도를 만들어 필요한 백 스택을 합성합니다. 그러나 이것은 인공적인 애플리케이션 스택이므로, 자연적인 작업에 있는 저장된 상태가 각 가상 작업에는 없습니다. 가상 부모 작업에 저장된 상태를 제공하기 위해, 작업에서 `OnPrepareNavigationUpTaskStack` 메서드를 재정의할 수 있습니다. 이 메서드는 Android에서 백 스택을 만드는 데 사용할 의도 개체 컬렉션을 포함하는 `TaskStackBuilder` 인스턴스를 수신합니다. 가상 작업이 만들어질 때 적절한 상태 정보를 받을 수 있도록 작업에서 이러한 의도를 수정할 수 있습니다.

좀 더 복잡한 시나리오의 경우 위로 탐색 동작을 처리하고 백 스택을 생성하는 데 사용할 수 있는 작업 클래스에 대한 새 메서드가 있습니다.

- `OnNavigateUp` – 이 메서드를 재정의하면 **위로** 단추가 눌릴 때 사용자 지정 작업을 수행할 수 있습니다.
- `NavigateUpTo` – 이 메서드를 호출하면 애플리케이션이 현재 작업을 탐색하여 주어진 의도에 지정된 작업으로 이동합니다.
- `ParentActivityIntent` – 현재 작업의 부모 작업을 시작할 의도를 가져오는 데 사용됩니다.
- `ShouldUpRecreateTask` – 이 메서드는 부모 작업까지 탐색하려면 가상 백 스택을 만들어야 하는지 여부를 쿼리하는 데 사용됩니다. 가상 스택을 만들어야 하는 경우 `true`를 반환합니다. 
- `FinishAffinity` – 이 메서드를 호출하면 현재 작업, 그리고 현재 태스크에서 해당 작업 아래에 있으면서 작업 선호도가 같은 모든 작업이 완료됩니다.
- `OnCreateNavigateUpTaskStack` – 이 메서드는 가상 스택을 만드는 방법을 완전히 제어해야 하는 경우에 재정의됩니다.

### <a name="camera"></a>카메라

자동 포커스가 이동을 시작 또는 중지하는 것을 감지하는 데 사용할 수 있는 새로운 `Camera.IAutoFocusMoveCallback` 인터페이스가 있습니다. 이 새 인터페이스의 예제는 다음 코드 조각에서 볼 수 있습니다.

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

새 클래스 `MediaActionSound`는 다양한 미디어 작업의 소리를 생성할 수 있는 API 세트를 제공합니다. 카메라에서 발생할 수 있는 여러 가지 작업이 있는데, 이러한 작업은 열거형 `Android.Media.MediaActionSoundType`에 의해 정의됩니다.

- `MediaActionSoundType.FocusComplete` – 포커싱이 완료될 때 재생되는 소리입니다.
- `MediaActionSoundType.ShutterClick` – 정지 이미지 사진을 찍을 때 이 소리가 재생됩니다.
- `MediaActionSoundType.StartVideoRecording` – 이 소리는 비디오 녹화의 시작을 나타내는 데 사용됩니다.
- `MediaActionSoundType.StopVideoRecording` – 비디오 녹화의 끝을 알리기 위해 재생되는 소리입니다.

`MediaActionSound` 클래스를 사용하는 방법에 대한 예제는 다음 코드에서 볼 수 있습니다.

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

Android Beam은 두 대의 Android 디바이스가 서로 통신할 수 있게 해주는 NFC 기반 기술입니다. Android 4.1은 대형 파일 전송을 보다 잘 지원합니다. 새 메서드 `NfcAdapter.SetBeamPushUris()`를 사용하면 Android는 대체 전송 메커니즘(예: Bluetooth) 간에 전환하여 전송 속도를 높입니다.

#### <a name="network-services-discovery"></a>Network Services 검색

Android 4.1에는 멀티캐스트 DNS 기반 서비스 검색을 위한 새 API가 포함되어 있습니다.
따라서 애플리케이션에서 프린터, 카메라, 미디어 디바이스 등의 다른 디바이스를 검색하여 Wi-Fi를 통해 연결할 수 있습니다. 이러한 새 API는 `Android.Net.Nsd` 패키지에 있습니다.

다른 서비스에서 사용할 수 있는 서비스를 만들려면 `NsdServiceInfo` 클래스를 사용하여 서비스의 속성을 정의하는 개체를 만듭니다. 그러면 이 개체는 `NsdManager.ResolveListener` 구현과 함께 `NsdManager.RegisterService()`에 제공됩니다. `NsdManager.ResolveListener` 구현은 등록 성공을 알리고 서비스 등록을 취소하는 데 사용됩니다.

네트워크에서 서비스를 검색하고, `NsdManager.discoverServices()`에 전달된 `Nsd.DiscoveryListener`의 구현을 검색합니다.

#### <a name="network-usage"></a>네트워크 사용량

새 메서드 `ConnectivityManager.IsActiveNetworkMetered`를 사용하여 디바이스가 요금제 네트워크에 연결되어 있는지 확인할 수 있습니다. 이 메서드를 사용하면 데이터 작업 비용이 많이 발생할 수 있다고 사용자에게 정확하게 알려 데이터 사용량을 관리할 수 있습니다.

#### <a name="wifi-direct-service-discovery"></a>WiFi 다이렉트 서비스 검색

`WifiP2pManager` 클래스는 *zeroconf*를 지원하기 위해 Android 4.0에서 도입되었습니다. zeroconf(제로 구성 네트워킹)는 디바이스(컴퓨터, 프린터, 휴대폰)에서 네트워크에 자동으로 연결할 수 있게 해주는 일련의 기술이며, 사람 네트워크 운영자 또는 특수 구성 서버가 개입됩니다.

Jelly Bean에서 `WifiP2pManager`는 *Bonjour* 또는 *Upnp*를 사용하여 주변에 있는 디바이스를 검색할 수 있습니다. Bonjour는 Apple의 zeroconf 구현입니다. Upnp는 zeroconf도 지원하는 네트워킹 프로토콜 세트입니다. Wi-Fi 서비스 검색을 지원하기 위해 다음 메서드가 `WiFiP2pManager`에 추가되었습니다.

- `AddLocalService()` – 이 메서드는 피어가 검색할 수 있도록 Wi-Fi를 통해 서비스로 제공되는 애플리케이션을 알리기 위한 용도로 사용됩니다.
- `AddServiceRequest(`) – 이 메서드는 프레임워크에 서비스 검색 요청을 보냅니다. Wi-Fi 서비스 검색을 초기화하는 데 사용됩니다.
- `SetDnsSdResponseListeners()` – 이 메서드는 Bonjour로부터 검색 요청에 대한 응답을 받을 때 호출되는 콜백을 등록하는 데 사용됩니다.
- `SetUpnpServiceResponseListener()` – 이 메서드는 Upnp로부터 검색 요청에 대한 응답을 받을 때 호출되는 콜백을 등록하는 데 사용됩니다.

### <a name="content-providers"></a>콘텐츠 공급자

`ContentResolver` 클래스가 새 메서드 `AcquireUnstableContentProvider`를 받았습니다. 이 메서드를 사용하면 애플리케이션에서 "불안정한" 콘텐츠 공급자를 획득할 수 있습니다. 일반적으로 애플리케이션에서 콘텐츠 공급자를 획득하고 해당 콘텐츠 공급자가 충돌하면 애플리케이션도 충돌합니다. 이 메서드를 호출하면 콘텐츠 공급자가 충돌해도 애플리케이션이 충돌하지 않습니다. 그 대신, 콘텐츠 공급자를 호출하면 콘텐츠 공급자가 사라졌다고 애플리케이션에 알리기 위해 `Android.OS.DeadObjectionException`이 throw됩니다. "불안정한" 콘텐츠 공급자는 다른 애플리케이션의 콘텐츠 공급자와 상호 작용할 때 유용합니다. 다른 애플리케이션의 버그가 있는 코드가 다른 애플리케이션에 영향을 줄 가능성이 낮기 때문입니다.

### <a name="copy-and-paste-with-intents"></a>의도를 사용하여 복사 및 붙여넣기

이제 `Intent` 클래스는 `Intent.ClipData` 속성을 사용하여 `ClipData` 개체를 연결할 수 있습니다. 이 메서드를 사용하면 의도를 사용하여 클립보드의 추가 데이터를 전송할 수 있습니다. `ClipData`의 인스턴스는 하나 이상의 `ClipData.Item`을 포함할 수 있습니다. `ClipData.Item`은 다음 형식의 항목입니다.

- **텍스트** – 모든 텍스트 문자열로, HTML이거나 기본 제공 Android 스타일 범위에서 지원되는 형식의 문자열입니다.
- **의도** – 모든 `Intent` 개체입니다.
- **Uri** – HTTP 책갈피 또는 콘텐츠 공급자의 URI와 같은 URI일 수 있습니다.

### <a name="isolated-services"></a>격리된 서비스

격리된 서비스는 자체적인 특수 프로세스에서 실행되고 자체 권한은 없는 서비스입니다. 서비스와의 통신은 서비스를 시작하고 서비스 API를 통해 서비스에 바인딩하는 경우에만 수행됩니다. `ServiceAttribute`에서 서비스 클래스를 표시하는 `IsolatedProcess="true"` 속성을 설정하여 서비스를 격리된 상태로 선언할 수 있습니다.

### <a name="media"></a>미디어

새 `Android.Media.MediaCodec` 클래스는 하위 수준 미디어 코덱에 대한 API를 제공합니다. 애플리케이션에서는 시스템을 쿼리하여 디바이스에서 사용 가능한 하위 수준의 코덱을 확인할 수 있습니다.

캡처된 오디오에서 추가적인 오디오 전처리를 지원하기 위해 새 `Android.Media.Audiofx.AudioEffect` 서브클래스가 추가되었습니다.

- `Android.Media.Audiofx.AcousticEchoCanceler` – 이 클래스는 오디오를 전처리하여 캡처된 오디오 신호의 원격 파티에서 신호를 제거하는 데 사용됩니다. 예를 들어 음성 통신 애플리케이션에서 에코를 제거하는 데 사용됩니다.
- `Android.Media.Audiofx.AutomaticGainControl` – 이 클래스는 출력 신호가 일정하도록 입력 신호를 늘리거나 줄여 캡처된 신호를 정규화하는 데 사용됩니다.
- `Android.Media.Audiofx.NoiseSuppressor` – 이 클래스는 캡처된 신호에서 배경 노이즈를 제거합니다.

모든 디바이스에서 이러한 효과를 지원하는 것은 아닙니다. 애플리케이션을 실행하는 디바이스에서 문제의 오디오 효과가 지원되는지 확인하려면 애플리케이션에서 `AudioEffect.IsAvailable` 메서드를 호출해야 합니다.

이제 `MediaPlayer` 클래스는 `SetNextMediaPlayer()` 메서드를 사용하여 끊김 없는 재생을 지원합니다. 이 새 메서드는 현재 미디어 플레이어가 재생을 마칠 때 시작할 다음 MediaPlayer를 지정합니다.

다음 새 클래스는 미디어를 재생할 위치를 선택하는 표준 메커니즘과 UI를 제공합니다.

- `MediaRouter` – 애플리케이션에서 이 클래스를 사용하면 디바이스에서 외부 스피커나 기타 디바이스로의 미디어 채널 라우팅을 제어할 수 있습니다.
- `MediaRouterActionProvider` 및 `MediaRouteButton` – 두 클래스는 미디어를 선택하고 재생할 수 있는 일관적인 UI를 제공합니다.

### <a name="notifications"></a>알림

Android 4.1을 사용하면 애플리케이션에서 보다 유연하고 알림을 표시하고 보다 철저하게 알림을 제어할 수 있습니다. 이제 애플리케이션에서 사용자에게 보다 크고 우수한 알림을 표시할 수 있습니다. 새 메서드 `NotificationBuilder.SetStyle()`를 사용하면 알림에 대한 다음 세 가지 새 스타일 중 하나를 설정할 수 있습니다.

- `Notification.BigPictureStyle` – 내부에 이미지가 들어 있는 알림을 생성하는 도우미 클래스입니다. 다음 이미지는 큰 이미지가 포함된 알림의 예를 보여줍니다.

 [![BigPictureStyle 알림의 예제 스크린샷](jelly-bean-images/image2.png)](jelly-bean-images/image2.png#lightbox)

- `Notification.BigTextStyle` – 이메일처럼 여러 텍스트 줄이 포함될 알림을 생성하는 도우미 클래스입니다. 다음은 이 새로운 알림 스타일의 예를 보여주는 스크린샷입니다.

 [![BigTextStyle 알림의 예제 스크린샷](jelly-bean-images/image3.png)](jelly-bean-images/image3.png#lightbox)

- `Notification.InboxStyle` – 이 스크린샷에 표시된 것처럼 이메일 메시지의 조각과 같은 문자열 목록을 포함하는 알림을 생성하는 도우미 클래스입니다.

 [![Notification.InboxStyle 알림의 예제 스크린샷](jelly-bean-images/image4.png)](jelly-bean-images/image4.png#lightbox)

알림에 보통 스타일 또는 큰 스타일을 사용하는 경우 알림 메시지의 아래쪽에 최대 2개의 작업 단추를 추가할 수 있습니다.
이에 대한 예제는 다음 스크린샷에서 볼 수 있으며, 작업 단추는 알림 아래쪽에 표시됩니다.

 [![알림 메시지 아래에 표시되는 작업 단추의 예제 스크린샷](jelly-bean-images/image5.png)](jelly-bean-images/image5.png#lightbox)

`Notification` 클래스는 개발자가 알림의 5가지 우선 순위 수준 중 하나를 지정할 수 있는 새 상수를 받았습니다. 이러한 우선 순위는 `Priority` 속성을 사용하여 알림에서 설정할 수 있습니다.

### <a name="permissions"></a>사용 권한

다음과 같은 새 권한이 추가되었습니다.

- `READ_EXTERNAL_STORAGE` - 애플리케이션에는 외부 스토리지에 대한 읽기 전용 액세스 권한이 필요합니다. 현재 모든 애플리케이션에는 기본적으로 읽기 권한이 있지만, Android 이후 릴리스에서는 애플리케이션에서 명시적으로 읽기 액세스를 요청해야 합니다.
- `READ_USER_DICTIONARY` - 사용자의 단어 사전에 대한 읽기 액세스를 허용합니다.
- `READ_CALL_LOG` - 애플리케이션이 호출 로그를 읽어서 들어오고 나가는 호출에 대한 정보를 얻을 수 있습니다.
- `WRITE_CALL_LOG` - 애플리케이션이 휴대폰에 호출 로그를 작성할 수 있도록 허용합니다.
- `WRITE_USER_DICTIONARY` - 애플리케이션이 사용자의 단어 사전에 쓸 수 있도록 허용합니다.

`READ_EXTERNAL_STORAGE` 메모의 중요한 변경 사항 – 현재 이 권한은 Android에서 자동으로 부여됩니다. Android 이후 버전에서는 애플리케이션에서 이 권한을 요청해야만 권한이 부여됩니다.

## <a name="summary"></a>요약

이 문서에서는 Android 4.1(API 레벨 16)에서 사용할 수 있는 몇 가지 새 API를 소개했습니다. 애니메이션 및 작업 시작에 애니메이션을 적용하는 기능의 변경 사항을 집중적으로 살펴보고, Bonjour 또는 UPnP 같은 프로토콜을 사용하여 다른 디바이스의 네트워크를 검색하기 위한 새 API를 소개했습니다. 의도를 통해 데이터를 자르고 붙여넣는 기능, 격리된 서비스 또는 "불안정한" 콘텐츠 공급자를 사용하는 기능 등 API의 기타 변경 사항도 자세히 살펴보았습니다.

이어서 이 문서에서는 알림 업데이트를 소개하고, Android 4.1에 도입된 새로운 권한 중 일부를 설명했습니다.

## <a name="related-links"></a>관련 링크

- [시간 애니메이션 예제(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-timeanimatorexample)
- [Android 4.1 API](https://developer.android.com/about/versions/android-4.1.html)
- [작업 및 백 스택](https://developer.android.com/guide/components/tasks-and-back-stack.html)
- [뒤로 및 위로 탐색](https://developer.android.com/design/patterns/navigation.html)
