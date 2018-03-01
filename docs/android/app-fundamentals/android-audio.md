---
title: Android Audio
description: "Android OS 광범위 한 지원을 제공 멀티미디어, 오디오 및 비디오를 모두 포함 합니다. 이 가이드 Android에서 오디오를 중점적으로 다루며 재생 하 고 하위 수준 오디오 API 뿐 아니라 기본 제공 오디오 플레이어 레코더 클래스를 사용 하 여 오디오 녹음에 대해 설명 합니다. 또한 개발자는 올바르게 동작 응용 프로그램을 빌드할 수 있도록 다른 응용 프로그램에 의해 브로드캐스팅 오디오 이벤트 작업에 다룹니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 646ED563-C34E-256D-4B56-29EE99881C27
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: ea3fd7d73f104f7b9650431a5531fe4399a2630c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="android-audio"></a>Android Audio

_Android OS 광범위 한 지원을 제공 멀티미디어, 오디오 및 비디오를 모두 포함 합니다. 이 가이드 Android에서 오디오를 중점적으로 다루며 재생 하 고 하위 수준 오디오 API 뿐 아니라 기본 제공 오디오 플레이어 레코더 클래스를 사용 하 여 오디오 녹음에 대해 설명 합니다. 또한 개발자는 올바르게 동작 응용 프로그램을 빌드할 수 있도록 다른 응용 프로그램에 의해 브로드캐스팅 오디오 이벤트 작업에 다룹니다._

<a name="Overview" />

## <a name="overview"></a>개요

최신 모바일 장치 채택 하 고 이전이 요구 장비의 전용된 부분 기능 &ndash; 카메라, 음악 플레이어와 비디오 레코더입니다. 이 인해 멀티미디어 프레임 워크 되 고 모바일 Api의 첫 번째 클래스 기능 있습니다.

Android 멀티미디어 광범위 한 지원을 제공합니다. 이 문서 작업을 Android에서 오디오를 검사 하 고 다음 항목을 다룹니다.

1.  **MediaPlayer 사용 하 여 오디오 재생** &ndash; 기본 제공을 사용 하 여 `MediaPlayer` 로컬 오디오 파일 및 스트리밍된 오디오 파일을 포함 하 여 오디오를 재생 하는 클래스는 `AudioTrack` 클래스입니다.

2.  **오디오 녹음** &ndash; 기본 제공을 사용 하 여 `MediaRecorder` 오디오를 기록할 클래스입니다.

3.  **오디오 알림 작업** &ndash; 올바르게 이벤트 (예: 들어오는 전화 통화)에 응답 하는 올바르게 동작 응용 프로그램을 만드는 오디오 알림을 사용 하 여 일시 중단 또는 오디오 출력을 취소 하 여 합니다.

4.  **하위 수준 오디오 작업** &ndash; 오디오를 사용 하 여 재생는 `AudioTrack` 메모리 버퍼에 직접 작성 하 여 클래스입니다. 사용 하 여 오디오 녹음는 `AudioRecord` 메모리 버퍼에서 직접 읽고 클래스입니다.


## <a name="requirements"></a>요구 사항

이 가이드에 필요한 Android 2.0 (API 수준 5) 이상. 장치에서 Android에서 오디오 디버깅를 수행 해야 함을 note 하십시오.

요청 해야 하는 `RECORD_AUDIO` 에서 권한을 **AndroidManifest.XML**:

![Android 매니페스트 레코드와의 사용 권한 섹션을 필수\_오디오 사용](android-audio-images/image01.png)


<a name="Playing_Audio_with_the_MediaPlayer_Class" />

## <a name="playing-audio-with-the-mediaplayer-class"></a>MediaPlayer 클래스와 오디오를 재생

Android에서 오디오를 재생 하는 가장 간단한 방법은 기본 제공은 [MediaPlayer](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/) 클래스입니다.
`MediaPlayer` 파일 경로를 전달 하 여 로컬 또는 원격 파일을 재생할 수 있습니다. 그러나 `MediaPlayer` 매우 상태 구분 및 잘못 된 상태에서 해당 메서드 중 하나를 호출 하면 예외가 throw 됩니다. 와 상호 작용 해야 `MediaPlayer` 의 오류를 방지 하려면 아래에서 설명 하는 순서입니다.


<a name="Initializing_and_Playing" />

### <a name="initializing-and-playing"></a>초기화 하 고 재생

와 오디오 재생 `MediaPlayer` 다음 시퀀스를 필요로 합니다.

1. 새 [MediaPlayer](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/) 개체입니다.

1. 구성 파일을 통해 재생는 [SetDataSource](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.SetDataSource/p/Java.IO.FileDescriptor/) 메서드.

1. 호출 된 [준비](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Prepare/) 플레이어를 초기화 하는 메서드.

1. 호출 된 [시작](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Start/) 오디오 재생을 시작 하는 메서드.


다음 코드 예제는이 사용법을 보여 줍니다.

```csharp
protected MediaPlayer player;
public void StartPlayer(String  filePath)
{
  if (player == null) {
    player = new MediaPlayer();
  } else {
    player.Reset();
    player.SetDataSource(filePath);
    player.Prepare();
    player.Start();
  }
}
```

<a name="Suspending_and_Resuming_Playback" />

### <a name="suspending-and-resuming-playback"></a>일시 중단 하 고 재생을 재개

호출 하 여 재생을 일시 중단할 수는 [일시 중지](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Pause%28%29/) 메서드:

```csharp
player.Pause();
```

재생을 일시 중지, 계속 하려면는 [시작](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Start%28%29/) 메서드.
재생에 일시 중지 된 위치에서 계속 실행 합니다.

```csharp
player.Start();
```

호출의 [중지](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Stop%28%29/) 플레이어에서 메서드는 진행 중인 재생을 종료 합니다.

```csharp
player.Stop();
```

호출 하 여 리소스를 해제 합니다 플레이어가 필요 하지 않은 경우는 [릴리스](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Release%28%29/) 메서드:

```csharp
player.Release();
```

<a name="Using_the_MediaRecorder_Class_to_Record_Audio" />


## <a name="using-the-mediarecorder-class-to-record-audio"></a>오디오 녹음 MediaRecorder 클래스를 사용 하 여

단독 `MediaPlayer` Android에서 오디오를 녹음할은는 [MediaRecorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/) 클래스입니다. 마찬가지로 `MediaPlayer`, 상태/구분 하 고 기록을 시작할 수에 도달 하는 몇 가지 상태로 전환 합니다. 오디오를 기록 하려면 여 `RECORD_AUDIO` 권한이 설정 되어 있어야 합니다. 사용 권한 참조 응용 프로그램을 설정 하는 방법에 대 한 지침은 [AndroidManifest.xml 작업](~/android/platform/android-manifest.md)합니다.

<a name="Initializing_and_Recording" />

### <a name="initializing-and-recording"></a>초기화 및 기록

와 오디오 녹음는 `MediaRecorder` 다음 단계를 수행 해야 합니다.

1. 새 [MediaRecorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/) 개체입니다.

2. 오디오 입력을 통해 캡처를 사용 하는 하드웨어 장치를 지정 된 [SetAudioSource](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetAudioSource/p/Android.Media.AudioSource/) 메서드.

3. 사용 하 여 출력 파일 오디오 형식이 설정 된 [SetOutputFormat](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetOutputFormat/p/Android.Media.OutputFormat/) 메서드. 지원 되는 오디오 유형의 목록에 대 한 참조 [Android 지원 미디어 형식](http://developer.android.com/guide/appendix/media-formats.html)합니다.

4. 호출 된 [SetAudioEncoder](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetAudioEncoder/p/Android.Media.AudioEncoder/) 오디오 인코딩 형식을 설정 하는 방법은 합니다.

5. 호출 된 [SetOutputFile](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetOutputFile/p/System.String/) 오디오 데이터에 기록 된 출력 파일의 이름을 지정 하는 메서드.

6. 호출 된 [준비](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Prepare%28%29/) 레코더를 초기화 하는 메서드.

7. 호출 된 [시작](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Start%28%29/) 기록을 시작 하려면 메서드.


다음 코드 예제에서는이 시퀀스를 보여 줍니다.

```csharp
protected MediaRecorder recorder;
void RecordAudio (String filePath)
{
  try {
    if (File.Exists (filePath)) {
      File.Delete (filePath);
    }
    if (recorder == null) {
      recorder = new MediaRecorder (); // Initial state.
    } else {
      recorder.Reset ();
      recorder.SetAudioSource (AudioSource.Mic);
      recorder.SetOutputFormat (OutputFormat.ThreeGpp);
      recorder.SetAudioEncoder (AudioEncoder.AmrNb);
      // Initialized state.
      recorder.SetOutputFile (filePath);
      // DataSourceConfigured state.
      recorder.Prepare (); // Prepared state
      recorder.Start (); // Recording state.
    }
  } catch (Exception ex) {
    Console.Out.WriteLine( ex.StackTrace);
  }
}
```

<a name="Stopping_recording" />

### <a name="stopping-recording"></a>기록을 중지 합니다.

기록을 중지 하려면 호출는 `Stop` 에서 메서드는 `MediaRecorder`:

```csharp
recorder.Stop();
```

<a name="Cleaning_up" />


### <a name="cleaning-up"></a>정리

한 번의 `MediaRecorder` 되었습니다 호출 중지는 [재설정](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Reset%28%29/) 유휴 상태로에 다시 배치 하는 메서드:

```csharp
recorder.Reset();
```

경우는 `MediaRecorder` 가 필요 없는 리소스를 해제 해야 합니다를 호출 하 여는 [릴리스](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Release%28%29/) 메서드:

```csharp
recorder.Release();
```

<a name="Managing_Audio_Notifications" />

## <a name="managing-audio-notifications"></a>오디오 알림 관리

<a name="The_AudioManager_Class" />


### <a name="the-audiomanager-class"></a>AudioManager 클래스

[AudioManager](https://developer.xamarin.com/api/type/Android.Media.AudioManager/) 클래스는 응용 프로그램이 오디오 이벤트가 발생할 때 알 수 있도록 알림 오디오에 대 한 액세스를 제공 합니다. 이 서비스가 다른 오디오 볼륨 및 신호음 모드 컨트롤 같은 기능에 액세스할 수도 있습니다. `AudioManager` 오디오 재생을 제어 하는 오디오 알림을 처리 하는 응용 프로그램 수 있습니다.

<a name="Managing_Audio_Focus" />


### <a name="managing-audio-focus"></a>오디오 포커스 관리

장치 (기본 제공 플레이어 및 레코더)의 오디오 리소스는 실행 중인 모든 응용 프로그램에서 공유 됩니다.

개념적으로이 하나의 응용 프로그램에 키보드 포커스가 데스크톱 컴퓨터에서 응용 프로그램과 유사한: 실행 중인 응용 프로그램 중 하나를 마우스 클릭 하 여 선택한 후 해당 응용 프로그램에만 키보드 입력 이동 합니다.

오디오 포커스는 비슷한 좋습니다 및 둘 이상의 응용 프로그램에서 재생 하거나 동시에 오디오를 기록 하지 않습니다. 자발적 이기 때문에 키보드 포커스가 보다 복잡 한 않은 &ndash; 응용 프로그램에는 현재 오디오 포커스를 둔 하 든 상관 없이 재생 그 사실을 무시할 수 &ndash; 및 다양 한 유형의 될 수 있는 오디오 포커스 있기 때문에 가 요청 했습니다. 예를 들어 요청자는 매우 짧은 시간에 대 한 오디오를 재생 해야만 일시적인 포커스를 요청할 수 것입니다.

오디오 포커스 및 될 수 즉시 권한이 부여 또는 처음 거부 나중에 부여 합니다. 예를 들어는 응용 프로그램 요청 오디오 포커스 전화 통화를 하는 경우 액세스가 거부 됩니다, 있지만 전화 통화 완료 되 면 포커스 부여도 될 수 있습니다. 이 경우 수신기 오디오 포커스 취소할 경우 그에 따라 응답 하기 위해 등록 됩니다. 오디오 포커스 요청 여부 좋다고 확인 재생 또는 오디오 녹음 결정 하는 데 사용 됩니다.

오디오 포커스에 대 한 자세한 내용은 참조 [오디오 포커스 관리](http://developer.android.com/training/managing-audio/audio-focus.html)합니다.


<a name="Registering_the_Callback_for_Audio_Focus" />

#### <a name="registering-the-callback-for-audio-focus"></a>오디오 포커스에 대 한 콜백 등록

등록 된 `FocusChangeListener` 콜백을 `IOnAudioChangeListener` 구하고 오디오 포커스 해제의 중요 한 부분입니다. 즉, 나중에 오디오 포커스를 부여 하는 지연 될 수 있습니다. 예를 들어 응용 프로그램은 진행 중인 전화 통화 동안 음악 재생을 요청할 수 있습니다. 전화 통화 완료 될 때까지 오디오 포커스를 부여 되지 않습니다.

이러한 이유로 콜백 개체에는 매개 변수로 전달 되는 `GetAudioFocus` 의 메서드는 `AudioManager`, 있고 그것이 콜백을 등록 하는이 호출 합니다. 오디오 포커스는 처음 거부 했는데 나중에 허용 하는 경우 응용 프로그램은 호출 하 여 내용 알림이 표시 `OnAudioFocusChange` 은 콜백 시. 오디오 포커스 자리를 비울 수행 되는 응용 프로그램을 구별 하는 동일한 방법을 사용 됩니다.

호출 응용 프로그램 오디오 리소스를 사용 하 여 완료 되 면는 `AbandonFocus` 의 메서드는 `AudioManager`, 다시 콜백에 전달 합니다. 이 알림의 콜백 등록을 취소 하 고 다른 응용 프로그램에서 오디오 포커스를 얻을 수 있도록 오디오 리소스를 해제 합니다.


<a name="Requesting_Audio_Focus" />

#### <a name="requesting-audio-focus"></a>오디오 포커스를 요청합니다.

장치의 오디오 리소스를 요청 하는 데 필요한 단계는 다음과 같습니다.

1.  에 대 한 핸들을 가져올는 `AudioManager` 시스템 서비스입니다.

2.  콜백 클래스의 인스턴스를 만듭니다.

3.  장치의 오디오 리소스를 호출 하 여 요청는 `RequestAudioFocus` 에서 메서드는 `AudioManager` 합니다. 매개 변수는 콜백 개체, 스트림 유형 (음악, 음성 통화, 링 등) 및 오른쪽 요청 된 액세스 유형 (오디오 리소스를 요청할 수 있습니다 일시적으로 또는 기간에 관계 없이 대 한 예를 들어).

4.  요청을 허용 하는 경우는 `playMusic` 메서드를 즉시 호출 되 고 오디오 재생을 시작 합니다.

5.  요청이 거부 되는 경우 추가 작업이 없으므로 수행 됩니다. 이 경우 요청은 나중에 부여 된 경우에 오디오 재생 됩니다.


다음 코드 예제에서는 다음이 단계를 보여 줍니다.

```csharp
Boolean RequestAudioResources(INotificationReceiver parent)
{
  AudioManager audioMan = (AudioManager) GetSystemService(Context.AudioService);
  AudioManager.IOnAudioFocusChangeListener listener  = new MyAudioListener(this);
  var ret = audioMan.RequestAudioFocus (listener, Stream.Music, AudioFocus.Gain );
  if (ret == AudioFocusRequest.Granted) {
    playMusic();
    return (true);
  } else if (ret == AudioFocusRequest.Failed) {
    return (false);
  }
  return (false);
}
```

<a name="Releasing_Audio_Focus" />

#### <a name="releasing-audio-focus"></a>오디오 포커스를 해제합니다.

트랙 재생이 완료 되 면는 `AbandonFocus` 메서드를 `AudioManager` 가 호출 됩니다. 따라서 다른 응용 프로그램이 장치의 오디오 리소스를 얻을 수 있습니다. 다른 응용 프로그램을 고유한 수신기를 등록 한 경우이 오디오 포커스 변경에 대 한 알림을 받습니다.

<a name="Low_Level_Audio_API" />

## <a name="low-level-audio-api"></a>낮은 수준의 오디오 API

하위 수준 오디오 Api를 보다 효과적으로 제어할 오디오 재생 하 고 메모리 버퍼 파일 Uri를 사용 하는 대신 직접 상호 작용 하기 때문에 기록 합니다. 이 방법은 것이 좋습니다 몇 가지 시나리오가 있습니다. 이러한 시나리오는 다음과 같습니다.

1.  암호화 된 오디오 파일에서 재생 합니다.

2.  짧은 클립의 연속을 재생 합니다.

3.  오디오 스트리밍입니다.


 <a name="AudioTrack_Class" />


### <a name="audiotrack-class"></a>AudioTrack 클래스

[AudioTrack](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/) 클래스 기록에 대 한 하위 수준 오디오 Api를 사용 하 고 하위 수준에 해당는 `MediaPlayer` 클래스입니다.

<a name="Initializing_and_Playing" />

#### <a name="initializing-and-playing"></a>초기화 하 고 재생

오디오의 새 인스턴스를 재생 하려면 `AudioTrack` 인스턴스화되어야 합니다. 인수 목록에 전달 되는 [생성자](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/#memberlist) 버퍼에 포함 된 오디오 샘플을 재생 하는 방법을 지정 합니다. 인수는:

1.  스트림 형식 &ndash; 음성, 벨 소리, 음악, 시스템 또는 경보 합니다.

2.  빈도 &ndash; 샘플링 속도 Hz로 표현 됩니다.

3.  구성 채널 &ndash; 모노 또는 스테레오 합니다.

4.  오디오 형식 &ndash; 8 비트 또는 16 비트 인코딩입니다.

5.  버퍼 크기 &ndash; (바이트)에서입니다.

6.  버퍼 모드 &ndash; 스트리밍 또는 정적입니다.


구성한 후의 [재생](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Play%28%29/) 방식의 `AudioTrack` 재생 시작 하도록 설정 하기 위해 호출 된 합니다. 오디오 버퍼에 쓰기는 `AudioTrack` 재생을 시작 합니다.

```csharp
void PlayAudioTrack(byte[] audioBuffer)
{
  AudioTrack audioTrack = new AudioTrack(
    // Stream type
    Stream.Music,
    // Frequency
    11025,
    // Mono or stereo
    ChannelOut.Mono,
    // Audio encoding
    Android.Media.Encoding.Pcm16bit,
    // Length of the audio clip.
    audioBuffer.Length,
    // Mode. Stream or static.
    AudioTrackMode.Stream);

    audioTrack.Play();
    audioTrack.Write(audioBuffer, 0, audioBuffer.Length);
}
```

<a name="Pausing_and_Stopping_the_Playback" />

#### <a name="pausing-and-stopping-the-playback"></a>일시 중지 하 고 재생이 중지

호출의 [일시 중지](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Pause%28%29/) 메서드는 재생을 일시 중지 합니다.

```csharp
audioTrack.Pause();
```

호출 된 [중지](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Stop%28%29/) 메서드 재생을 영구적으로 종료 됩니다.

```csharp
audioTrack.Stop();
```

<a name="Cleaning_up" />

#### <a name="cleanup"></a>정리

경우는 `AudioTrack` 가 필요 없는 리소스를 해제 해야 합니다를 호출 하 여 [릴리스](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Release%28%29/):

```csharp
audioTrack.Release();
```

<a name="The_AudioRecord_Class" />

### <a name="the-audiorecord-class"></a>AudioRecord 클래스

[AudioRecord](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/) 클래스는 해당 하는 `AudioTrack` 기록 쪽에 있습니다. 마찬가지로 `AudioTrack`, 파일 및 Uri 대신 메모리 버퍼를 직접 사용 합니다. `RECORD_AUDIO` 매니페스트에 권한이 설정 되어 있습니다.

<a name="Initializing_and_Recording" />

#### <a name="initializing-and-recording"></a>초기화 및 기록

첫 번째 단계는 새 [AudioRecord](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/) 개체입니다. 인수 목록에 전달 되는 [생성자](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/#memberlist) 기록 하는 데 필요한 모든 정보를 제공 합니다. 와 달리에 `AudioTrack`인수는 크게 열거형에 상응 하는 인수 여기서 `AudioRecord` 는 정수입니다. 여기에는 다음이 포함됩니다.

1.  하드웨어 마이크 등 오디오 입력된 원본입니다.

2.  스트림 형식 &ndash; 음성, 벨 소리, 음악, 시스템 또는 경보 합니다.

3.  빈도 &ndash; 샘플링 속도 Hz로 표현 됩니다.

4.  구성 채널 &ndash; 모노 또는 스테레오 합니다.

5.  오디오 형식 &ndash; 8 비트 또는 16 비트 인코딩입니다.

6.  버퍼의 크기 (바이트)


한 번는 `AudioRecord` 생성 된 해당 [StartRecording](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.StartRecording%28%29/) 메서드가 호출 됩니다. 기록을 시작할 준비가 되었습니다. `AudioRecord` 지속적으로 입력에 대 한 오디오 버퍼를 읽고 쓰는이 오디오 파일을 입력 해야 합니다.

```csharp
void RecordAudio()
{
  byte[] audioBuffer = new byte[100000];
  var audRecorder = new AudioRecord(
    // Hardware source of recording.
    AudioSource.Mic,
    // Frequency
    11025,
    // Mono or stereo
    ChannelIn.Mono,
    // Audio encoding
    Android.Media.Encoding.Pcm16bit,
    // Length of the audio clip.
    audioBuffer.Length
  );
  audRecorder.StartRecording();
  while (true) {
    try
    {
      // Keep reading the buffer while there is audio input.
      audRecorder.Read(audioBuffer, 0, audioBuffer.Length);
      // Write out the audio file.
    } catch (Exception ex) {
      Console.Out.WriteLine(ex.Message);
      break;
    }
  }
}
```

<a name="Stopping_the_Recording" />

#### <a name="stopping-the-recording"></a>기록을 중지합니다.

호출 된 [중지](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.Stop%28%29/) 메서드 종료 기록:

```csharp
audRecorder.Stop();
```

<a name="Clean_Up" />

#### <a name="cleanup"></a>정리

경우는 `AudioRecord` 개체가 더 이상 필요 호출 해당 [릴리스](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.Release%28%29/) 연결 된 모든 리소스를 해제 하는 메서드:

```csharp
audRecorder.Release();
```

<a name="Summary" />

## <a name="summary"></a>요약

Android OS는 재생, 기록 및 오디오를 관리 하기 위한 강력한 기능을 제공 합니다. 이 문서에서는 재생 하는 방법 및 사용 하 여 상위 수준 오디오 녹음 `MediaPlayer` 및 `MediaRecorder` 클래스입니다. 다음으로, 다른 응용 프로그램 간에 장치의 오디오 리소스를 공유할 오디오 알림을 사용 하는 방법을 탐색 합니다. 마지막으로, 어떻게 재생 하는 하위 수준 Api를 사용 하 여 오디오 녹음는와 상호 메모리 버퍼를 처리 합니다.


## <a name="related-links"></a>관련 링크

- [오디오와 작업 (샘플)](https://developer.xamarin.com/samples/Example_WorkingWithAudio/)
- [미디어 플레이어](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/)
- [미디어 레코더](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/)
- [오디오 관리자](https://developer.xamarin.com/api/type/Android.Media.AudioManager/)
- [오디오 트랙](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/)
- [오디오 레코더](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/)
