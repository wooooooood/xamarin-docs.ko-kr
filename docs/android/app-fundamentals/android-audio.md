---
title: Android Audio
description: Android OS는 광범위 한 지원 멀티미디어, 오디오와 비디오 둘 다 포함 합니다. 이 가이드는 Android에서 오디오에 중점을 둡니다 하 고 재생 하 고 낮은 수준의 오디오 API 뿐만 아니라 기본 제공 오디오 플레이어 레코더 클래스를 사용 하 여 오디오를 녹음/녹화를 다룹니다. 또한 커지긴 응용 프로그램을 개발할 수 있도록 다른 응용 프로그램에 의해 브로드캐스팅 되는 오디오 이벤트를 사용 하 여 작업을 다룹니다.
ms.prod: xamarin
ms.assetid: 646ED563-C34E-256D-4B56-29EE99881C27
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/28/2018
ms.openlocfilehash: f34a76879c2a38ec8253d8f3dd3230f03d9af32e
ms.sourcegitcommit: 2eb8961dd7e2a3e06183923adab6e73ecb38a17f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66827312"
---
# <a name="android-audio"></a>Android Audio

_Android OS는 광범위 한 지원 멀티미디어, 오디오와 비디오 둘 다 포함 합니다. 이 가이드는 Android에서 오디오에 중점을 둡니다 하 고 재생 하 고 낮은 수준의 오디오 API 뿐만 아니라 기본 제공 오디오 플레이어 레코더 클래스를 사용 하 여 오디오를 녹음/녹화를 다룹니다. 또한 커지긴 응용 프로그램을 개발할 수 있도록 다른 응용 프로그램에 의해 브로드캐스팅 되는 오디오 이벤트를 사용 하 여 작업을 다룹니다._


## <a name="overview"></a>개요

최신 모바일 장치는 이전의 장비 부분이 전용된 기능을 채택 했습니다 &ndash; 카메라, 음악 플레이어와 비디오 레코더입니다. 이 인해 멀티미디어 프레임 워크에 모바일 Api의 첫 번째 클래스 기능 했으면 합니다.

Android 멀티미디어 광범위 한 지원을 제공합니다. 이 문서에서는 android에서 오디오를 사용 하 여 작업을 검사 하 고 다음 항목을 다룹니다.

1.  **MediaPlayer 사용 하 여 오디오를 재생** &ndash; 기본 제공을 사용 하 여 `MediaPlayer` 로컬 오디오 파일 및 사용 하 여 스트리밍된 오디오 파일을 포함 하 여 오디오를 재생 하는 클래스는 `AudioTrack` 클래스입니다.

2.  **오디오 녹음** &ndash; 기본 제공을 사용 하 여 `MediaRecorder` 오디오 녹음 하는 클래스입니다.

3.  **오디오 알림 작업** &ndash; 커지긴 응용 프로그램 이벤트 (예: 들어오는 전화 통화)에 제대로 응답을 만들려면 오디오 알림을 사용 하 여 일시 중단 또는 오디오 출력을 취소 합니다.

4.  **저수준 오디오 작업할** &ndash; 오디오를 사용 하 여 재생을 `AudioTrack` 메모리 버퍼에 직접 작성 하 여 클래스. 오디오를 사용 하 여 기록 된 `AudioRecord` 메모리 버퍼에서 직접 읽고 클래스입니다.


## <a name="requirements"></a>요구 사항

이 가이드에서는 Android 2.0 (API 레벨 5) 이상. 장치의 Android에서 오디오를 디버그를 수행 해야 함을 note 하십시오.

요청 하는 데 필요한 것은 `RECORD_AUDIO` 에서 사용 권한을 **AndroidManifest.XML**:

![필요한 사용 권한 레코드를 사용 하 여 Android 매니페스트 부분\_오디오 사용](android-audio-images/image01.png)



## <a name="playing-audio-with-the-mediaplayer-class"></a>MediaPlayer 클래스를 사용 하 여 오디오를 재생합니다.

Android에서 오디오를 재생 하는 가장 간단한 방법은 기본 제공 된 [MediaPlayer](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/) 클래스입니다.
`MediaPlayer` 파일 경로 전달 하 여 로컬 또는 원격 파일을 재생할 수 있습니다. 그러나 `MediaPlayer` 상태를 매우 중요 한 잘못 된 상태에서 해당 메서드 중 하나를 호출 하면 예외가 throw 됩니다. 상호 작용 하는 것이 반드시 `MediaPlayer` 오류를 방지 하려면 아래 설명 된 순서로 합니다.



### <a name="initializing-and-playing"></a>초기화 및 재생

사용 하 여 오디오 재생 `MediaPlayer` 다음 시퀀스가 필요 합니다.

1. 새 인스턴스화할 [MediaPlayer](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/) 개체입니다.

1. 구성 파일을 통해 재생 합니다 [SetDataSource](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.SetDataSource/p/Java.IO.FileDescriptor/) 메서드.

1. 호출 된 [준비](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Prepare/) 플레이어 초기화 하는 방법입니다.

1. 호출 된 [시작](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Start/) 오디오 재생을 시작 하는 방법입니다.


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


### <a name="suspending-and-resuming-playback"></a>일시 중단 하 고 재생을 다시 시작

호출 하 여 재생을 일시 중단할 수는 [일시 중지](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Pause%28%29/) 메서드:

```csharp
player.Pause();
```

일시 중지 된 재생을 재개 하려면 호출을 [시작](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Start%28%29/) 메서드.
이 재생 일시 중지 된 위치에서 다시 시작 됩니다.

```csharp
player.Start();
```

호출 된 [중지](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Stop%28%29/) player 메서드는 진행 중인 재생 종료:

```csharp
player.Stop();
```

플레이어가 더 이상 필요 하면 리소스를 호출 하 여 해제 되어야 합니다는 [릴리스](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Release%28%29/) 메서드:

```csharp
player.Release();
```



## <a name="using-the-mediarecorder-class-to-record-audio"></a>오디오 녹음 MediaRecorder 클래스를 사용 하 여

필연적인 `MediaPlayer` Android의 오디오를 녹음/녹화에는 [MediaRecorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/) 클래스. 같은 `MediaPlayer`, 상태를 구분 하 고 기록 시작 지점으로 가져오려면 몇 가지 상태로 전환 합니다. 오디오를 기록 하려면 여 `RECORD_AUDIO` 권한이 설정 되어 있어야 합니다. 사용 권한 참조 응용 프로그램을 설정 하는 방법에 대 한 지침은 [AndroidManifest.xml 작업할](~/android/platform/android-manifest.md)합니다.


### <a name="initializing-and-recording"></a>초기화 및 기록

사용 하 여 오디오를 녹음/녹화를 `MediaRecorder` 다음 단계를 수행 합니다.

1. 새 인스턴스화할 [MediaRecorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/) 개체입니다.

2. 오디오 입력을 통해 캡처를 사용 하는 하드웨어 장치를 지정 합니다 [SetAudioSource](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetAudioSource/p/Android.Media.AudioSource/) 메서드.

3. 사용 하 여 출력 파일 오디오 형식을 설정 합니다 [SetOutputFormat](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetOutputFormat/p/Android.Media.OutputFormat/) 메서드. 지원 되는 오디오 형식 목록을 참조 하세요 [Android 지원 되는 미디어 형식](https://developer.android.com/guide/appendix/media-formats.html)합니다.

4. 호출 된 [SetAudioEncoder](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetAudioEncoder/p/Android.Media.AudioEncoder/) 오디오 인코딩 형식을 설정 하는 방법입니다.

5. 호출 된 [SetOutputFile](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetOutputFile/p/System.String/) 오디오 데이터를 쓸 출력 파일의 이름을 지정 하는 방법입니다.

6. 호출 된 [준비](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Prepare%28%29/) 레코더를 초기화 하는 방법입니다.

7. 호출 된 [시작](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Start%28%29/) 기록을 시작 하는 방법입니다.


다음 코드 샘플은이 순서를 보여 줍니다.

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


### <a name="stopping-recording"></a>기록을 중지 합니다.

녹음/녹화를 중지 하려면 호출을 `Stop` 메서드는 `MediaRecorder`:

```csharp
recorder.Stop();
```



### <a name="cleaning-up"></a>정리

한 번 합니다 `MediaRecorder` 되었습니다 중지를 호출 합니다 [재설정](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Reset%28%29/) 유휴 상태로 다시 배치 하는 방법:

```csharp
recorder.Reset();
```

경우는 `MediaRecorder` 는 더 이상 필요 해당 리소스가 해제 되어야 합니다를 호출 하 여 합니다 [릴리스](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Release%28%29/) 메서드:

```csharp
recorder.Release();
```


## <a name="managing-audio-notifications"></a>오디오 알림 관리



### <a name="the-audiomanager-class"></a>AudioManager 클래스

합니다 [AudioManager](https://developer.xamarin.com/api/type/Android.Media.AudioManager/) 클래스는 응용 프로그램이 오디오 이벤트가 발생할 때 알 수 있는 오디오 알림에 대 한 액세스를 제공 합니다. 이 서비스는 또한 볼륨과 신호음 모드 컨트롤 등 다른 오디오 기능에 대 한 액세스를 제공합니다. `AudioManager` 오디오 재생을 제어 하려면 오디오 알림을 처리 하는 응용 프로그램을 허용 합니다.



### <a name="managing-audio-focus"></a>오디오 포커스를 관리합니다.

(기본 제공 player 및 레코더) 장치의 오디오 리소스는 모든 실행 중인 응용 프로그램에서 공유 됩니다.

개념적으로이 하나의 응용 프로그램에 키보드 포커스가 있는 데스크톱 컴퓨터에서 응용 프로그램과 유사한: 실행 중인 응용 프로그램 중 하나를 마우스 클릭 하 여 선택한 후 바로 입력 해당 응용 프로그램에만 이동 합니다.

오디오 포커스 비슷한 개념 이며 둘 이상의 응용 프로그램에서 재생 하거나 동시 오디오를 기록 하지 않습니다. 자발적 이기 때문에 키보드 포커스가 보다 더 복잡 한지 &ndash; 응용 프로그램은 현재 오디오 포커스가와 상관 없이 재생 사실을 무시 해도 &ndash; 이므로 가지 다른 유형의 수 있는 오디오 포커스 요청 합니다. 예를 들어,만 요청자에 게 매우 짧은 시간에 오디오만 재생을 예상 하지만, 일시적 포커스를 요청할 수 있습니다 것입니다.

오디오 포커스 있습니다 즉시 권한이 부여 또는 처음 거부 되며 나중에 부여 합니다. 예를 들어 경우는 응용 프로그램 요청 오디오 포커스 전화 통화를 거부, 됩니다 있지만 포커스 전화 통화 완료 되 면 부여 되어야 잘 수 있습니다. 이 경우 수신기 오디오 포커스 번 수행 되 면 그에 따라 응답 하기 위해 등록 됩니다. 오디오 포커스 요청 여부 확인 재생 또는 오디오 녹음을 결정 하는 데 사용 됩니다.

오디오 포커스에 대 한 자세한 내용은 참조 하세요. [오디오 포커스 관리](https://developer.android.com/training/managing-audio/audio-focus.html)합니다.



#### <a name="registering-the-callback-for-audio-focus"></a>오디오 포커스에 대 한 콜백 등록

등록 합니다 `FocusChangeListener` 콜백에서 `IOnAudioChangeListener` 오디오 포커스 해제를 얻고 중요 한 부분입니다. 즉, 나중에 오디오 포커스를 부여 하는 지연 될 수 있습니다. 예를 들어, 응용 프로그램이 전화 통화를 진행에서 하는 동안 음악을 재생을 요청할 수 있습니다. 전화 통화 완료 될 때까지 오디오 포커스 부여 되지 않습니다.

이 따라서 콜백 개체를 매개 변수로 전달 됩니다는 `GetAudioFocus` 메서드를 `AudioManager`, 것이 콜백을 등록 하는이 호출 합니다. 호출 하 여 응용 프로그램 합리적인 오디오 포커스 처음 거부 하지만 나중에 부여 하는 경우 `OnAudioFocusChange` 콜백 시. 동일한 방법은 오디오 포커스 번 수행 되는 응용 프로그램에 지시 됩니다.

호출 응용 프로그램이 오디오 리소스를 사용 하 여 완료 되 면를 `AbandonFocus` 메서드는 `AudioManager`, 콜백에서 다시 전달 합니다. 이 콜백을 등록 취소 하 고 다른 응용 프로그램에서 오디오 포커스를 얻을 수 있도록 오디오 리소스를 해제 합니다.



#### <a name="requesting-audio-focus"></a>오디오 포커스를 요청합니다.

장치의 오디오 리소스를 요청 하는 데 필요한 단계는 다음과 같습니다.

1.  에 대 한 핸들을 가져올는 `AudioManager` 시스템 서비스입니다.

2.  콜백 클래스의 인스턴스를 만듭니다.

3.  호출 하 여 장치의 오디오 리소스를 요청 합니다 `RequestAudioFocus` 메서드는 `AudioManager` . 매개 변수는 콜백 개체, 스트림 형식 (음악, 음성 통화, 링 등) 및 오른쪽 요청 된 액세스 유형 (오디오 리소스를 요청할 수 있습니다을 무기한 동안 일시적으로 또는 예를 들어).

4.  요청을 허용 하는 경우는 `playMusic` 메서드를 즉시 호출 되 고 오디오 재생을 시작 합니다.

5.  요청이 거부 되 면 추가 작업이 없으므로 만들어질 수 있습니다. 이 경우 요청은 나중에 부여 된 경우에 오디오 재생 됩니다.


다음 코드 예제는 이러한 단계를 보여줍니다.

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


#### <a name="releasing-audio-focus"></a>오디오 포커스를 해제합니다.

트랙의 재생이 완료 되 면 합니다 `AbandonFocus` 메서드를 `AudioManager` 가 호출 됩니다. 이렇게 하면 장치의 오디오 리소스를 다른 응용 프로그램. 다른 응용 프로그램을 고유한 수신기를 등록 한 경우이 오디오 포커스 변경에 대 한 알림을 받습니다.


## <a name="low-level-audio-api"></a>낮은 수준의 오디오 API

오디오 하위 수준 Api를 보다 효과적으로 제어할 오디오 재생 하 고 메모리 버퍼 파일 Uri를 사용 하는 대신 직접 상호 작용 하므로 기록 합니다. 일부의 시나리오가이 방법은 것이 좋습니다. 이러한 시나리오에는 다음이 포함 됩니다.

1.  암호화 된 오디오 파일에서 재생 합니다.

2.  경우 짧은 클립의 연속을 재생 합니다.

3.  오디오 스트리밍입니다.


### <a name="audiotrack-class"></a>AudioTrack 클래스

합니다 [AudioTrack](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/) 클래스는 기록에 대 한 오디오 하위 수준 Api를 사용 하 여 이며 하위 수준에 해당 하는는 `MediaPlayer` 클래스입니다.


#### <a name="initializing-and-playing"></a>초기화 및 재생

오디오의 새 인스턴스를 재생 하려면 `AudioTrack` 인스턴스화되어야 합니다. 인수 목록에 전달 된 [생성자](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/#memberlist) 버퍼에 포함 된 샘플 오디오를 재생 하는 방법을 지정 합니다. 인수는:

1.  Stream 형식 &ndash; 음성, 벨소리, 음악, 시스템 또는 경보입니다.

2.  빈도 &ndash; Hz 단위로 샘플링 비율입니다.

3.  구성 채널 &ndash; Mono 또는 스테레오 합니다.

4.  오디오 형식 &ndash; 8 비트 또는 16 비트 인코딩입니다.

5.  버퍼 크기 &ndash; (바이트)에서입니다.

6.  버퍼 모드 &ndash; 스트리밍 또는 정적입니다.


생성 후에 [재생](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Play%28%29/) 메서드의 `AudioTrack` 재생 시작 하도록 설정 하는 호출. 오디오 버퍼 작성을 `AudioTrack` 재생을 시작 합니다.

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


#### <a name="pausing-and-stopping-the-playback"></a>일시 중지 하 고 재생을 중지 합니다.

호출 된 [일시 중지](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Pause%28%29/) 재생을 일시 중지 하는 방법.

```csharp
audioTrack.Pause();
```

호출 된 [중지](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Stop%28%29/) 메서드 재생을 영구적으로 종료 됩니다.

```csharp
audioTrack.Stop();
```


#### <a name="cleanup"></a>정리

경우는 `AudioTrack` 는 더 이상 필요 해당 리소스가 해제 되어야 합니다를 호출 하 여 [릴리스](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Release%28%29/):

```csharp
audioTrack.Release();
```


### <a name="the-audiorecord-class"></a>AudioRecord 클래스

합니다 [AudioRecord](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/) 클래스는 해당 `AudioTrack` 기록 우변입니다. 같은 `AudioTrack`, 파일 및 Uri 대신 메모리 버퍼를 직접 사용 합니다. 필요는 `RECORD_AUDIO` 매니페스트에서 권한이 설정 되어 있습니다.


#### <a name="initializing-and-recording"></a>초기화 및 기록

첫 번째 단계는 새 [AudioRecord](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/) 개체입니다. 인수 목록에 전달 된 [생성자](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/#memberlist) 기록 하는 데 필요한 모든 정보를 제공 합니다. 와 달리에 `AudioTrack`인수는 주로 열거형에 상응 하는 인수 여기서 `AudioRecord` 은 정수입니다. 여기에는 다음이 포함됩니다.

1.  하드웨어 마이크와 같은 오디오 입력된 소스입니다.

2.  Stream 형식 &ndash; 음성, 벨소리, 음악, 시스템 또는 경보입니다.

3.  빈도 &ndash; Hz 단위로 샘플링 비율입니다.

4.  구성 채널 &ndash; Mono 또는 스테레오 합니다.

5.  오디오 형식 &ndash; 8 비트 또는 16 비트 인코딩입니다.

6.  버퍼의 크기 (바이트)


한 번 합니다 `AudioRecord` 생성 되 면 해당 [StartRecording](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.StartRecording%28%29/) 메서드가 실행 됩니다. 녹음/녹화를 시작할 준비가 되었습니다. `AudioRecord` 지속적으로 오디오 입력 버퍼를 읽고 쓰는이 오디오 파일을 입력 해야 합니다.

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


#### <a name="stopping-the-recording"></a>녹음/녹화 중지

호출 된 [중지](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.Stop%28%29/) 메서드 기록 종료:

```csharp
audRecorder.Stop();
```


#### <a name="cleanup"></a>정리

경우는 `AudioRecord` 개체가 더 이상 필요 호출 해당 [릴리스](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.Release%28%29/) 메서드가 연결 된 모든 리소스를 해제 합니다.

```csharp
audRecorder.Release();
```


## <a name="summary"></a>요약

Android OS는 재생, 기록 및 오디오를 관리 하는 강력한 프레임 워크를 제공 합니다. 이 문서에서는 설명 재생 하는 방법 및 상위 수준를 사용 하 여 오디오를 녹음 `MediaPlayer` 및 `MediaRecorder` 클래스입니다. 다음으로 오디오 알림을 사용 하 여 다른 응용 프로그램 간에 장치의 오디오 리소스를 공유 하는 방법을 알아보았습니다. 마지막으로, 방법을 하위 수준 Api를 사용 하 여 오디오를 녹음을 재생 하는 인터페이스 메모리 버퍼를 사용 하 여 직접 사용 하 여 처리 합니다.


## <a name="related-links"></a>관련 링크

- [오디오 사용 하 여 작업 (샘플)](https://developer.xamarin.com/samples/monodroid/Example_WorkingWithAudio/)
- [Media Player](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/)
- [미디어 레코더](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/)
- [Audio Manager](https://developer.xamarin.com/api/type/Android.Media.AudioManager/)
- [오디오 트랙](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/)
- [오디오 레코더](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/)
