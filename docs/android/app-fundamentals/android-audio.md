---
title: Android Audio
description: Android OS는 오디오 및 비디오를 모두 제공 하는 멀티미디어를 광범위 하 게 지원 합니다. 이 가이드는 Android의 오디오를 중심으로 하 고 기본 제공 오디오 플레이어와 레코더 클래스를 사용 하 여 오디오를 재생 하 고 기록 하는 방법 및 하위 수준 오디오 API에 대해 다룹니다. 또한 개발자가 잘 작동 하는 응용 프로그램을 빌드할 수 있도록 다른 응용 프로그램에서 브로드캐스팅하는 오디오 이벤트를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 646ED563-C34E-256D-4B56-29EE99881C27
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/28/2018
ms.openlocfilehash: 8719d521fe512f348aade0a3d43d2f0b0d3bf0ec
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70755622"
---
# <a name="android-audio"></a>Android Audio

_Android OS는 오디오 및 비디오를 모두 제공 하는 멀티미디어를 광범위 하 게 지원 합니다. 이 가이드는 Android의 오디오를 중심으로 하 고 기본 제공 오디오 플레이어와 레코더 클래스를 사용 하 여 오디오를 재생 하 고 기록 하는 방법 및 하위 수준 오디오 API에 대해 다룹니다. 또한 개발자가 잘 작동 하는 응용 프로그램을 빌드할 수 있도록 다른 응용 프로그램에서 브로드캐스팅하는 오디오 이벤트를 사용 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

최신 모바일 장치에는 이전에 장비 &ndash; 카메라, 음악 플레이어 및 비디오 레코더의 전용 부분이 필요한 기능을 채택 했습니다. 이로 인해 멀티미디어 프레임 워크는 모바일 Api의 최고 수준의 기능이 됩니다.

Android는 멀티미디어를 광범위 하 게 지원 합니다. 이 문서에서는 Android에서 오디오 작업을 검토 하 고 다음 항목에 대해 설명 합니다.

1. **MediaPlayer로 오디오 재생** &ndash; 기본 `AudioTrack` 제공 `MediaPlayer` 클래스를 사용 하 여 클래스를 사용 하 여 로컬 오디오 파일 및 스트리밍된 오디오 파일을 비롯 한 오디오 재생

2. **오디오 기록** &ndash; 기본 제공`MediaRecorder` 클래스를 사용 하 여 오디오 기록

3. **오디오 알림 사용** &ndash; 오디오 알림을 사용 하 여 오디오 출력을 일시 중단 하거나 취소 하 여 이벤트 (예: 수신 전화 통화)에 올바르게 응답 하는 잘 작동 하는 응용 프로그램을 만듭니다.

4. **하위 수준 오디오 작업** 메모리 버퍼에 직접 `AudioTrack` 기록 하 여 클래스를 사용 하 여 오디오 재생 &ndash; 클래스를 `AudioRecord` 사용 하 여 오디오를 기록 하 고 메모리 버퍼에서 직접 읽습니다.

## <a name="requirements"></a>요구 사항

이 가이드를 사용 하려면 Android 2.0 (API 레벨 5) 이상이 필요 합니다. Android에서 오디오 디버깅은 장치에서 수행 해야 합니다.

`RECORD_AUDIO` **Androidmanifest**에서 사용 권한을 요청 해야 합니다.

![녹화\_오디오가 활성화 된 Android 매니페스트의 필수 사용 권한 섹션](android-audio-images/image01.png)

## <a name="playing-audio-with-the-mediaplayer-class"></a>MediaPlayer 클래스를 사용 하 여 오디오 재생

Android에서 오디오를 재생 하는 가장 간단한 방법은 기본 제공 [MediaPlayer](xref:Android.Media.MediaPlayer) 클래스를 사용 하는 것입니다.
`MediaPlayer`는 파일 경로를 전달 하 여 로컬 또는 원격 파일을 재생할 수 있습니다. `MediaPlayer` 그러나는 상태를 구분 하 고 잘못 된 상태에서 해당 메서드 중 하나를 호출 하면 예외가 throw 됩니다. 오류를 방지 하려면 아래에 `MediaPlayer` 설명 된 순서 대로와 상호 작용 하는 것이 중요 합니다.

### <a name="initializing-and-playing"></a>초기화 및 재생

로 오디오를 `MediaPlayer` 재생 하려면 다음 시퀀스가 필요 합니다.

1. 새 [MediaPlayer](xref:Android.Media.MediaPlayer) 개체를 인스턴스화합니다.

1. [Setdatasource](xref:Android.Media.MediaPlayer.SetDataSource*) 메서드를 통해 재생할 파일을 구성 합니다.

1. [Prepare](xref:Android.Media.MediaPlayer.Prepare) 메서드를 호출 하 여 플레이어를 초기화 합니다.

1. [시작](xref:Android.Media.MediaPlayer.Start) 메서드를 호출 하 여 오디오 재생을 시작 합니다.

아래 코드 샘플에서는이 사용법을 보여 줍니다.

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

### <a name="suspending-and-resuming-playback"></a>재생 일시 중단 및 다시 시작

[Pause](xref:Android.Media.MediaPlayer.Pause) 메서드를 호출 하 여 재생을 일시 중단할 수 있습니다.

```csharp
player.Pause();
```

일시 중지 된 재생을 다시 시작 하려면 [Start](xref:Android.Media.MediaPlayer.Start) 메서드를 호출 합니다.
재생 시 일시 중지 된 위치에서 다시 시작 됩니다.

```csharp
player.Start();
```

플레이어의 [Stop](xref:Android.Media.MediaPlayer.Stop) 메서드를 호출 하면 진행 중인 재생이 종료 됩니다.

```csharp
player.Stop();
```

플레이어가 더 이상 필요 하지 않은 경우 [릴리스](xref:Android.Media.MediaPlayer.Release) 메서드를 호출 하 여 리소스를 해제 해야 합니다.

```csharp
player.Release();
```

## <a name="using-the-mediarecorder-class-to-record-audio"></a>MediaRecorder 클래스를 사용 하 여 오디오 기록

Android에서 오디오 `MediaPlayer` 를 기록 하는 필연적인 결과로는 [mediarecorder](xref:Android.Media.MediaRecorder) 클래스입니다. `MediaPlayer`와 마찬가지로 상태를 구분 하 고 여러 상태를 전환 하 여 기록을 시작할 수 있는 지점으로 이동 합니다. 오디오를 `RECORD_AUDIO` 기록 하려면 사용 권한을 설정 해야 합니다. 응용 프로그램 사용 권한을 설정 하는 방법에 대 한 지침은 [AndroidManifest 작업](~/android/platform/android-manifest.md)을 참조 하세요.

### <a name="initializing-and-recording"></a>초기화 및 기록

을 사용 하 여 `MediaRecorder` 오디오를 기록 하려면 다음 단계를 수행 해야 합니다.

1. 새 [Mediarecorder](xref:Android.Media.MediaRecorder) 개체를 인스턴스화합니다.

2. [Setpc 원본](xref:Android.Media.MediaRecorder.SetAudioSource*) 메서드를 통해 오디오 입력을 캡처하기 위해 사용할 하드웨어 장치를 지정 합니다.

3. [SetOutputFormat](xref:Android.Media.MediaRecorder.SetOutputFormat*) 메서드를 사용 하 여 출력 파일 오디오 형식을 설정 합니다. 지원 되는 오디오 유형 목록은 [Android 지원 미디어 형식](https://developer.android.com/guide/appendix/media-formats.html)을 참조 하세요.

4. [Setaudio encoder](xref:Android.Media.MediaRecorder.SetAudioEncoder*) 메서드를 호출 하 여 오디오 인코딩 유형을 설정 합니다.

5. [SetOutputFile](xref:Android.Media.MediaRecorder.SetOutputFile*) 메서드를 호출 하 여 오디오 데이터가 기록 되는 출력 파일의 이름을 지정 합니다.

6. [Prepare](xref:Android.Media.MediaRecorder.Prepare) 메서드를 호출 하 여 레코더를 초기화 합니다.

7. [Start](xref:Android.Media.MediaRecorder.Start) 메서드를 호출 하 여 기록을 시작 합니다.

다음 코드 샘플에서는이 시퀀스를 보여 줍니다.

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

### <a name="stopping-recording"></a>기록 중지

기록을 중지 하려면에서 `Stop` 메서드를 호출 합니다. `MediaRecorder`

```csharp
recorder.Stop();
```

### <a name="cleaning-up"></a>정리 중

가 중지 `MediaRecorder` 된 후 [Reset](xref:Android.Media.MediaRecorder.Reset) 메서드를 호출 하 여 다시 해당 유휴 상태로 전환 합니다.

```csharp
recorder.Reset();
```

`MediaRecorder`가 더 이상 필요 하지 않은 경우 [릴리스](xref:Android.Media.MediaRecorder.Release) 메서드를 호출 하 여 해당 리소스를 해제 해야 합니다.

```csharp
recorder.Release();
```

## <a name="managing-audio-notifications"></a>오디오 알림 관리

### <a name="the-audiomanager-class"></a>오디오 관리자 클래스

오디오 [관리자](xref:Android.Media.AudioManager) 클래스는 오디오 이벤트가 발생 하는 경우 응용 프로그램에서 알 수 있도록 오디오 알림에 대 한 액세스를 제공 합니다. 이 서비스는 볼륨 및 벨소리 모드 제어와 같은 다른 오디오 기능에도 액세스할 수 있도록 합니다. 를 `AudioManager` 사용 하면 응용 프로그램에서 오디오 재생을 제어 하는 오디오 알림을 처리할 수 있습니다.

### <a name="managing-audio-focus"></a>오디오 포커스 관리

장치의 오디오 리소스 (기본 제공 플레이어 및 레코더)는 실행 중인 모든 응용 프로그램에서 공유 됩니다.

개념적으로이는 한 응용 프로그램만 키보드 포커스를가지고 있는 데스크톱 컴퓨터의 응용 프로그램과 유사 합니다. 마우스로 클릭 하 여 실행 중인 응용 프로그램 중 하나를 선택한 후에는 키보드 입력이 해당 응용 프로그램 으로만 이동 합니다.

오디오 포커스는 비슷한 개념으로, 둘 이상의 응용 프로그램이 동시에 오디오를 재생 하거나 기록 하는 것을 방지 합니다. 이는 자발적 &ndash; 이기 때문에 키보드 포커스 보다 더 복잡 합니다 .이는 응용 프로그램에서 현재 오디오 포커스가 없고 &ndash; 재생에는 영향을 주지 않고 다른 형식의 오디오 포커스가 있을 수 있습니다. 하였습니다. 예를 들어 요청자는 매우 짧은 시간 동안만 오디오를 재생 하는 것으로 예상 되는 경우 일시적인 포커스를 요청할 수 있습니다.

오디오 포커스는 즉시 부여 되거나 나중에 처음으로 거부 되 고 부여 될 수 있습니다. 예를 들어 전화 통화 중에 응용 프로그램이 오디오 포커스를 요청 하는 경우에는 거부 되지만 전화 통화가 완료 된 후에는 포커스를 받을 수 있습니다. 이 경우 오디오 포커스가 사라질 때 적절 하 게 응답 하기 위해 수신기가 등록 됩니다. 오디오 포커스를 요청 하면 오디오를 재생 하거나 녹음할 수 있는지 여부를 확인 하는 데 사용 됩니다.

오디오 포커스에 대 한 자세한 내용은 [오디오 포커스 관리](https://developer.android.com/training/managing-audio/audio-focus.html)를 참조 하세요.

#### <a name="registering-the-callback-for-audio-focus"></a>음성 포커스에 대 한 콜백 등록

에서 콜백을 등록 하면 오디오 포커스를 가져오고 해제 하는 것 이중요한부분입니다.`IOnAudioChangeListener` `FocusChangeListener` 그 이유는 오디오 포커스가 이후 시간까지 지연 될 수 있기 때문입니다. 예를 들어, 응용 프로그램에서 전화 통화를 진행 하는 동안 음악 재생을 요청할 수 있습니다. 전화 호출이 완료 될 때까지 오디오 포커스가 부여 되지 않습니다.

이러한 이유로 콜백 개체는 `GetAudioFocus` `AudioManager`의 메서드에 매개 변수로 전달 되며이 호출은 콜백을 등록 합니다. 처음에 오디오 포커스가 거부 되었지만 나중에 부여 된 경우에는 콜백에서를 호출 `OnAudioFocusChange` 하 여 응용 프로그램에 알립니다. 응용 프로그램에 오디오 포커스를 사용 하 고 있음을 알리기 위해 동일한 메서드가 사용 됩니다.

응용 프로그램이 오디오 리소스 사용을 마치면 `AbandonFocus` `AudioManager`의 메서드를 호출 하 고 콜백을 다시 전달 합니다. 이렇게 하면 콜백이 등록 취소 오디오 리소스가 해제 되어 다른 응용 프로그램이 오디오 포커스를 받을 수 있습니다.

#### <a name="requesting-audio-focus"></a>오디오 포커스 요청

장치의 오디오 리소스를 요청 하는 데 필요한 단계는 다음과 같습니다.

1. `AudioManager` 시스템 서비스에 대 한 핸들을 가져옵니다.

2. 콜백 클래스의 인스턴스를 만듭니다.

3. 에서 메서드를 `RequestAudioFocus` 호출 하 여 장치의 오디오 리소스를 요청 합니다.`AudioManager` 매개 변수는 콜백 개체, 스트림 형식 (음악, 음성 통화, 링 등) 및 요청 된 액세스 권한 유형 (예: 일시적으로 또는 무기한으로 오디오 리소스를 요청할 수 있음)입니다.

4. 요청이 부여 `playMusic` 되 면 메서드가 즉시 호출 되 고 오디오가 재생 되기 시작 합니다.

5. 요청이 거부 되 면 추가 작업을 수행 하지 않습니다. 이 경우에는 요청이 나중에 부여 된 경우에만 오디오가 재생 됩니다.

아래 코드 샘플에서는 이러한 단계를 보여 줍니다.

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

#### <a name="releasing-audio-focus"></a>오디오 포커스 해제

트랙 재생이 완료 `AbandonFocus` 되 면의 `AudioManager` 메서드가 호출 됩니다. 이렇게 하면 다른 응용 프로그램에서 장치의 오디오 리소스를 얻을 수 있습니다. 다른 응용 프로그램은 자체 수신기를 등록 한 경우이 오디오 포커스 변경에 대 한 알림을 받게 됩니다.

## <a name="low-level-audio-api"></a>낮은 수준 오디오 API

하위 수준 오디오 Api는 파일 Uri를 사용 하는 대신 메모리 버퍼와 직접 상호 작용 하기 때문에 오디오 재생 및 기록에 대 한 더 많은 제어를 제공 합니다. 이 방법을 사용 하는 것이 가장 좋은 몇 가지 시나리오가 있습니다. 이러한 시나리오는 다음과 같습니다.

1. 암호화 된 오디오 파일에서 재생 하는 경우

2. 연속 하는 짧은 클립을 재생 하는 경우

3. 오디오 스트리밍.

### <a name="audiotrack-class"></a>오디오 트랙 클래스

오디오 [트랙](xref:Android.Media.AudioTrack) 클래스는 기록에 하위 수준 오디오 api를 사용 하 고,는 `MediaPlayer` 클래스에 해당 하는 하위 수준의 오디오 api를 사용 합니다.

#### <a name="initializing-and-playing"></a>초기화 및 재생

오디오를 재생 하려면의 `AudioTrack` 새 인스턴스를 인스턴스화해야 합니다. [생성자](xref:Android.Media.AudioTrack) 에 전달 된 인수 목록은 버퍼에 포함 된 오디오 샘플을 재생 하는 방법을 지정 합니다. 인수는 다음과 같습니다.

1. Stream 형식 &ndash; 음성, 벨 소리, 음악, 시스템 또는 경보.

2. 빈도 &ndash; 는 Hz로 표시 되는 샘플링 률입니다.

3. 채널 구성 &ndash; Mono 또는 스테레오.

4. 오디오 형식 &ndash; 8 비트 또는 16 비트 인코딩입니다.

5. 버퍼 크기 &ndash; (바이트)입니다.

6. 버퍼 모드 &ndash; 스트리밍 또는 정적입니다.

생성 후의 `AudioTrack` [재생](xref:Android.Media.AudioTrack.Play) 메서드를 호출 하 여 재생을 시작 하도록 설정 합니다. 오디오 버퍼 `AudioTrack` 를에 쓰면 재생이 시작 됩니다.

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

#### <a name="pausing-and-stopping-the-playback"></a>재생 일시 중지 및 중지

[Pause](xref:Android.Media.AudioTrack.Pause) 메서드를 호출 하 여 재생을 일시 중지 합니다.

```csharp
audioTrack.Pause();
```

[Stop](xref:Android.Media.AudioTrack.Stop) 메서드를 호출 하면 재생이 영구적으로 종료 됩니다.

```csharp
audioTrack.Stop();
```

#### <a name="cleanup"></a>정리

`AudioTrack`가 더 이상 필요 하지 않은 경우 [릴리스](xref:Android.Media.AudioTrack.Release)를 호출 하 여 해당 리소스를 해제 해야 합니다.

```csharp
audioTrack.Release();
```

### <a name="the-audiorecord-class"></a>AudioRecord 클래스

[AudioRecord](xref:Android.Media.AudioRecord) 클래스 `AudioTrack` 는 기록 측의에 해당 합니다. 와 `AudioTrack`마찬가지로, 파일 및 uri 대신 메모리 버퍼를 직접 사용 합니다. `RECORD_AUDIO` 매니페스트에 사용 권한을 설정 해야 합니다.

#### <a name="initializing-and-recording"></a>초기화 및 기록

첫 번째 단계는 새 [AudioRecord](xref:Android.Media.AudioRecord) 개체를 생성 하는 것입니다. [생성자](xref:Android.Media.AudioRecord) 에 전달 되는 인수 목록은 기록에 필요한 모든 정보를 제공 합니다. 에서 `AudioTrack`인수가 주로 열거 되는와 달리의 `AudioRecord` 해당 인수는 정수입니다. 이러한 개체는 다음과 같습니다.

1. 마이크와 같은 하드웨어 오디오 입력 원본입니다.

2. Stream 형식 &ndash; 음성, 벨 소리, 음악, 시스템 또는 경보.

3. 빈도 &ndash; 는 Hz로 표시 되는 샘플링 률입니다.

4. 채널 구성 &ndash; Mono 또는 스테레오.

5. 오디오 형식 &ndash; 8 비트 또는 16 비트 인코딩입니다.

6. 버퍼 크기 (바이트)

`AudioRecord`가 생성 되 면 해당 [startrecording](xref:Android.Media.AudioRecord.StartRecording) 메서드가 호출 됩니다. 이제 기록을 시작할 준비가 되었습니다. 는 `AudioRecord` 입력에 대 한 오디오 버퍼를 지속적으로 읽고 오디오 파일에이 입력을 씁니다.

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

#### <a name="stopping-the-recording"></a>기록을 중지 하는 중

[Stop](xref:Android.Media.AudioRecord.Stop) 메서드를 호출 하면 기록이 종료 됩니다.

```csharp
audRecorder.Stop();
```

#### <a name="cleanup"></a>정리

`AudioRecord`개체가 더 이상 필요 하지 않은 경우 해당 [Release](xref:Android.Media.AudioRecord.Release) 메서드를 호출 하면 연결 된 모든 리소스가 해제 됩니다.

```csharp
audRecorder.Release();
```

## <a name="summary"></a>요약

Android OS는 오디오를 재생, 기록 및 관리 하기 위한 강력한 프레임 워크를 제공 합니다. 이 문서에서는 상위 수준 `MediaPlayer` 및 `MediaRecorder` 클래스를 사용 하 여 오디오를 재생 하 고 기록 하는 방법을 설명 했습니다. 다음으로 오디오 알림을 사용 하 여 서로 다른 응용 프로그램 간에 장치의 오디오 리소스를 공유 하는 방법을 살펴보았습니다. 마지막으로 메모리 버퍼와 직접 상호 작용 하는 하위 수준 Api를 사용 하 여 오디오를 재생 하 고 기록 하는 방법을 처리 했습니다.

## <a name="related-links"></a>관련 링크

- [오디오 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/example-workingwithaudio)
- [Media Player](xref:Android.Media.MediaPlayer)
- [미디어 레코더](xref:Android.Media.MediaRecorder)
- [오디오 관리자](xref:Android.Media.AudioManager)
- [오디오 트랙](xref:Android.Media.AudioTrack)
- [오디오 레코더](xref:Android.Media.AudioRecord)
