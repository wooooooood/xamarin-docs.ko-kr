---
title: Xamarin.ios의 음성 인식
description: 이 문서에서는 새로운 Speech API를 제공 하 고 높여줄 앱에서이를 구현 하 여 연속 음성 인식을 지원 하 고 라이브 또는 녹화 된 오디오 스트림에서 오디오를 텍스트로 변환 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 64FED50A-6A28-4833-BEAE-63CEC9A09010
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 40aa36fa8a89eacd8be7914020c06f3fec75baff
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70227330"
---
# <a name="speech-recognition-in-xamarinios"></a>Xamarin.ios의 음성 인식

_이 문서에서는 새로운 Speech API를 제공 하 고 높여줄 앱에서이를 구현 하 여 연속 음성 인식을 지원 하 고 라이브 또는 녹화 된 오디오 스트림에서 오디오를 텍스트로 변환 하는 방법을 보여 줍니다._

IOS 10의 새로운 기능으로, Apple은 iOS 앱이 지속적인 음성 인식과 높여줄 음성 (라이브 또는 기록 된 오디오 스트림)을 텍스트로 변환할 수 있도록 하는 음성 인식 API를 출시 했습니다.

Apple에 따르면 음성 인식 API는 다음과 같은 기능과 이점을 제공 합니다.

- 매우 정확
- 아트의 상태
- 사용 하기 쉬운
- Fast
- 여러 언어 지원
- 사용자 개인 정보 취급 방침

## <a name="how-speech-recognition-works"></a>음성 인식 작동 방법

음성 인식은 iOS 앱에서 구현 되며, 라이브 또는 미리 녹음 된 오디오 (API가 지 원하는 음성 언어 중 하나)를 획득 하 고 음성 인식기의 일반 텍스트 기록을 반환 하는 음성 인식기에 전달 합니다.

[![](speech-images/speech01.png "음성 인식 작동 방법")](speech-images/speech01.png#lightbox)

### <a name="keyboard-dictation"></a>키보드 받아쓰기

대부분의 사용자가 iOS 장치에서 음성 인식을 생각해 보면 iPhone 4S을 사용 하 여 iOS 5에서 키보드 받아쓰기와 함께 출시 된 기본 제공 Siri 음성 길잡이가 있다고 생각 합니다.

키보드 받아쓰기는 또는 `UITextField` `UITextArea`와 같은 textkit를 지 원하는 모든 인터페이스 요소에서 지원 되며 iOS 가상 키보드의 받아쓰기 단추 (스페이스바 바로 왼쪽)를 클릭 하 여 사용자가 활성화 합니다.

Apple에서 다음 키보드 받아쓰기 통계 (2011 이후 수집 됨)를 해제 했습니다.

- 키보드 받아쓰기는 iOS 5에서 릴리스된 이후 널리 사용 되었습니다.
- 약 65000 앱은 하루에 사용 합니다.
- 모든 iOS 받아쓰기의 세 번째는 타사 앱에서 수행 됩니다.

키보드 받아쓰기는 응용 프로그램의 UI 디자인에서 TextKit 인터페이스 요소를 사용 하는 것 외에 개발자 파트에 대 한 노력이 필요 하지 않기 때문에 매우 간편 하 게 사용할 수 있습니다. 키보드 받아쓰기는 앱을 사용 하기 전에 앱의 특별 한 권한 요청을 요구 하지 않는 이점도 있습니다.

음성 인식을 사용 하려면 Apple 서버에서 데이터의 전송 및 임시 저장소가 필요 하므로 새 음성 인식 Api를 사용 하는 앱에는 사용자가 부여 하는 특별 한 권한이 필요 합니다. 자세한 내용은 [보안 및 개인 정보 보호 기능](~/ios/app-fundamentals/security-privacy.md) 설명서를 참조 하세요.

키보드 받아쓰기는 구현 하기 쉽지만 다음과 같은 몇 가지 제한 사항이 있습니다.

- 텍스트 입력 필드를 사용 하 고 키보드를 표시 해야 합니다.
- 라이브 오디오 입력 전용으로 작동 하며 앱이 오디오 녹음 프로세스를 제어 하지 않습니다.
- 사용자의 음성을 해석 하는 데 사용 되는 언어에 대 한 제어를 제공 하지 않습니다.
- 앱에서 받아쓰기 단추를 사용자가 사용할 수 있는지 여부를 알 수 있는 방법은 없습니다.
- 앱에서 오디오 녹음 프로세스를 사용자 지정할 수 없습니다.
- 타이밍 및 신뢰도와 같은 정보가 부족 한 매우 얕은 결과 집합을 제공 합니다.

### <a name="speech-recognition-api"></a>음성 인식 API

IOS 10을 처음 접하는 Apple은 iOS 앱에서 음성 인식을 구현할 수 있는 보다 강력한 방법을 제공 하는 음성 인식 API를 출시 했습니다. 이 API는 Apple에서 Siri와 키보드 받아쓰기를 모두 사용 하는 데 사용 하는 것과 동일 하며, 미술 정확성 상태를 사용 하 여 빠른 기록을 제공할 수 있습니다.

음성 인식 API에서 제공 하는 결과는 앱이 개인 사용자 데이터를 수집 하거나 액세스 하지 않고도 개별 사용자에 게 투명 하 게 사용자 지정 됩니다.

음성 인식 API는 사용자가 말하는 동안 거의 실시간으로 호출 앱에 결과를 다시 제공 하며, 단순히 텍스트 보다 번역 결과에 대 한 자세한 정보를 제공 합니다. 이러한 개체는 다음과 같습니다.

- 사용자가 말한 작업을 여러 번 해석 합니다.
- 개별 번역을 위한 신뢰 수준입니다.
- 타이밍 정보

위에서 설명한 것 처럼 번역을 위한 오디오는 라이브 피드 또는 미리 기록 된 소스와 iOS 10에서 지 원하는 모든 50 언어 및 언어 중 하나에서 제공 될 수 있습니다.

음성 인식 API는 iOS 10을 실행 하는 모든 iOS 장치에서 사용할 수 있으며 대부분의 경우 대량 번역은 Apple의 서버에서 발생 하므로 라이브 인터넷에 연결 해야 합니다. 즉, 일부 최신 iOS 장치는 특정 언어의 항상 켜 지는 장치에서 변환을 지원 합니다.

Apple에는 지정 된 언어를 현재 시점에서 변환할 수 있는지 여부를 확인 하는 가용성 API가 포함 되어 있습니다. 앱은 직접 인터넷 연결을 테스트 하는 대신이 API를 사용 해야 합니다.

위에서 설명한 것 처럼 음성 인식은 인터넷을 통해 Apple 서버에서 데이터의 전송 및 임시 저장소가 필요 하므로 앱에서 사용자의 권한을 요청 _해야 합니다_ . 파일의 키를 입력 하 고 메서드 `SFSpeechRecognizer.RequestAuthorization` 를 호출 합니다. `NSSpeechRecognitionUsageDescription` `Info.plist` 

음성 인식에 사용 되는 오디오의 원본에 따라 앱의 `Info.plist` 파일을 다른 방법으로 변경 해야 할 수 있습니다. 자세한 내용은 [보안 및 개인 정보 보호 기능](~/ios/app-fundamentals/security-privacy.md) 설명서를 참조 하세요.

## <a name="adopting-speech-recognition-in-an-app"></a>앱에서 음성 인식 채택

개발자가 iOS 앱에서 음성 인식을 도입 하기 위해 수행 해야 하는 네 가지 주요 단계가 있습니다.

- 키를 `Info.plist` `NSSpeechRecognitionUsageDescription` 사용 하 여 앱의 파일에 사용 설명을 제공 합니다. 예를 들어 카메라 앱에는 다음 설명이 포함 될 수 있습니다 _. "이를 통해 ' 치즈 ' 라는 단어를 말하여 사진을 가져올 수 있습니다."_
- 응용 프로그램에서 대화 상자 `SFSpeechRecognizer.RequestAuthorization` 에 있는 사용자에 대 한 음성 인식을 인식 `NSSpeechRecognitionUsageDescription` 하 고 허용 또는 거부 하는 이유에 대 한 설명을 제공 하기 위해 메서드를 호출 하 여 권한 부여를 요청 합니다.
- 음성 인식 요청을 만듭니다.
  - 디스크에 미리 기록 된 `SFSpeechURLRecognitionRequest` 오디오의 경우 클래스를 사용 합니다.
  - 라이브 오디오 (또는 메모리의 오디오)의 경우 `SFSPeechAudioBufferRecognitionRequest` 클래스를 사용 합니다.
- 음성 인식 요청을 음성 인식기 (`SFSpeechRecognizer`)에 전달 하 여 인식을 시작 합니다. 앱은 선택적으로 반환 `SFSpeechRecognitionTask` 된을 포함 하 여 인식 결과를 모니터링 하 고 추적할 수 있습니다.

이러한 단계는 아래에 자세히 설명 되어 있습니다.

### <a name="providing-a-usage-description"></a>사용 설명 제공

파일에 필요한 `NSSpeechRecognitionUsageDescription` 키를 제공 하려면 다음을 수행 합니다. `Info.plist`

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 파일을 `Info.plist` 두 번 클릭 하 여 편집용으로 엽니다.
2. **원본** 뷰로 전환 합니다. 

    [![](speech-images/speech02.png "원본 뷰")](speech-images/speech02.png#lightbox)
3. **새 항목 추가**를 클릭 하 고 `NSSpeechRecognitionUsageDescription` **형식** 및 **사용 설명** 에 대 한 **속성** `String` 에 **값**을 입력 합니다. 예를 들어: 

    [![](speech-images/speech03.png "NSSpeechRecognitionUsageDescription 추가")](speech-images/speech03.png#lightbox)
4. 앱에서 라이브 오디오 기록을 처리 하는 경우에는 마이크 사용 설명도 필요 합니다. **새 항목 추가**를 클릭 하 고 `NSMicrophoneUsageDescription` **형식** 및 **사용 설명** 에 대 한 **속성** `String` 에 **값**을 입력 합니다. 예: 

    [![](speech-images/speech04.png "NSMicrophoneUsageDescription 추가")](speech-images/speech04.png#lightbox)
5. 파일의 변경 내용을 저장합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 파일을 `Info.plist` 두 번 클릭 하 여 편집용으로 엽니다.
2. **새 항목 추가**를 클릭 하 고 `NSSpeechRecognitionUsageDescription` **형식** 및 **사용 설명** 에 대 한 **속성** `String` 에 **값**을 입력 합니다. 예: 

    [![](speech-images/speech03w.png "NSSpeechRecognitionUsageDescription 추가")](speech-images/speech03w.png#lightbox)
3. 앱에서 라이브 오디오 기록을 처리 하는 경우에는 마이크 사용 설명도 필요 합니다. **새 항목 추가**를 클릭 하 고 `NSMicrophoneUsageDescription` **형식** 및 **사용 설명** 에 대 한 **속성** `String` 에 **값**을 입력 합니다. 예를 들어: 

    [![](speech-images/speech04w.png "NSMicrophoneUsageDescription 추가")](speech-images/speech04w.png#lightbox)
4. 파일의 변경 내용을 저장합니다.

-----

> [!IMPORTANT]
> 위의 `Info.plist` 키 (`NSSpeechRecognitionUsageDescription` 또는 `NSMicrophoneUsageDescription`) 중 하나를 제공 하지 못하면 음성 인식 또는 라이브 오디오의 마이크에 액세스 하려고 할 때 경고 없이 앱에 오류가 발생할 수 있습니다.




### <a name="requesting-authorization"></a>권한 부여 요청

앱에서 음성 인식에 액세스할 수 있도록 하는 필수 사용자 권한 부여를 요청 하려면 기본 보기 컨트롤러 클래스를 편집 하 고 다음 코드를 추가 합니다.

```csharp
using System;
using UIKit;
using Speech;

namespace MonkeyTalk
{
    public partial class ViewController : UIViewController
    {
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Request user authorization
            SFSpeechRecognizer.RequestAuthorization ((SFSpeechRecognizerAuthorizationStatus status) => {
                // Take action based on status
                switch (status) {
                case SFSpeechRecognizerAuthorizationStatus.Authorized:
                    // User has approved speech recognition
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.Denied:
                    // User has declined speech recognition
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.NotDetermined:
                    // Waiting on approval
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.Restricted:
                    // The device is not permitted
                    ...
                    break;
                }
            });
        }
    }
}
```

클래스의 메서드는 `RequestAuthorization` 개발자가 `Info.plist` 파일의 `NSSpeechRecognitionUsageDescription` 키에 제공 된 이유를 사용 하 여 사용자의 음성 인식 액세스 권한을 요청 합니다. `SFSpeechRecognizer`

사용자의 권한에 따라 작업을 `RequestAuthorization` 수행 하는 데 사용할 수 있는 메서드의 콜백 루틴에 결과가반환됩니다.`SFSpeechRecognizerAuthorizationStatus` 

> [!IMPORTANT]
> Apple은 사용자가 앱에서이 권한을 요청 하기 전에 음성 인식이 필요한 작업을 시작할 때까지 대기를 제안 합니다.

### <a name="recognizing-pre-recorded-speech"></a>미리 기록 된 음성 인식

앱이 미리 기록한 WAV 또는 MP3 파일의 음성을 인식 하려는 경우 다음 코드를 사용할 수 있습니다.

```csharp
using System;
using UIKit;
using Speech;
using Foundation;
...

public void RecognizeFile (NSUrl url)
{
    // Access new recognizer
    var recognizer = new SFSpeechRecognizer ();

    // Is the default language supported?
    if (recognizer == null) {
        // No, return to caller
        return;
    }

    // Is recognition available?
    if (!recognizer.Available) {
        // No, return to caller
        return;
    }

    // Create recognition task and start recognition
    var request = new SFSpeechUrlRecognitionRequest (url);
    recognizer.GetRecognitionTask (request, (SFSpeechRecognitionResult result, NSError err) => {
        // Was there an error?
        if (err != null) {
            // Handle error
            ...
        } else {
            // Is this the final translation?
            if (result.Final) {
                Console.WriteLine ("You said, \"{0}\".", result.BestTranscription.FormattedString);
            }
        }
    });
}
```

이 코드를 자세히 살펴보면 먼저 음성 인식기 (`SFSpeechRecognizer`)를 만들려고 시도 합니다. 기본 언어가 음성 인식을 `null` 지원 하지 않는 경우이 반환 되 고 함수가 종료 됩니다.

음성 인식기를 기본 언어로 사용할 수 있는 경우 앱은 `Available` 속성을 사용 하 여 현재 인식에 사용할 수 있는지 여부를 확인 합니다. 예를 들어 장치에 활성 인터넷 연결이 없는 경우 인식 기능을 사용 하지 못할 수 있습니다.

는 `SFSpeechUrlRecognitionRequest` iOS 장치에서 미리 `NSUrl` 기록 된 파일의 위치에서 만들어지며, 콜백 루틴을 사용 하 여 처리 하기 위해 음성 인식기로 전달 됩니다.

콜백이 호출 `NSError` `null` 되 면가 처리 해야 하는 오류가 없는 것입니다. 음성 인식이 증분 방식으로 수행 되기 때문에 콜백 루틴은 두 번 이상 호출 될 수 `SFSpeechRecognitionResult.Final` 있습니다. 이렇게 하면 변환이 완료 되 고 최상의 번역 버전 (`BestTranscription`)이 기록 되는지 여부를 확인 하는 속성이 테스트 됩니다.

### <a name="recognizing-live-speech"></a>라이브 음성 인식

앱에서 라이브 음성을 인식 하려는 경우 프로세스는 미리 기록 된 음성을 인식 하는 것과 매우 비슷합니다. 예:

```csharp
using System;
using UIKit;
using Speech;
using Foundation;
using AVFoundation;
...

#region Private Variables
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
#endregion
...

public void StartRecording ()
{
    // Setup audio session
    var node = AudioEngine.InputNode;
    var recordingFormat = node.GetBusOutputFormat (0);
    node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
        // Append buffer to recognition request
        LiveSpeechRequest.Append (buffer);
    });

    // Start recording
    AudioEngine.Prepare ();
    NSError error;
    AudioEngine.StartAndReturnError (out error);

    // Did recording start?
    if (error != null) {
        // Handle error and return
        ...
        return;
    }

    // Start recognition
    RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
        // Was there an error?
        if (err != null) {
            // Handle error
            ...
        } else {
            // Is this the final translation?
            if (result.Final) {
                Console.WriteLine ("You said \"{0}\".", result.BestTranscription.FormattedString);
            }
        }
    });
}

public void StopRecording ()
{
    AudioEngine.Stop ();
    LiveSpeechRequest.EndAudio ();
}

public void CancelRecording ()
{
    AudioEngine.Stop ();
    RecognitionTask.Cancel ();
}
```

이 코드를 자세히 살펴보면 인식 프로세스를 처리 하는 몇 가지 전용 변수를 만듭니다.

```csharp
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
```

이는 `SFSpeechAudioBufferRecognitionRequest` AV 기반을 사용 하 여에 전달 되는 오디오를 기록 하 여 인식 요청을 처리 합니다.

```csharp
var node = AudioEngine.InputNode;
var recordingFormat = node.GetBusOutputFormat (0);
node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
    // Append buffer to recognition request
    LiveSpeechRequest.Append (buffer);
});
```

앱에서 기록을 시작 하려고 시도 하 고 기록을 시작할 수 없는 경우 오류가 처리 됩니다.

```csharp
AudioEngine.Prepare ();
NSError error;
AudioEngine.StartAndReturnError (out error);

// Did recording start?
if (error != null) {
    // Handle error and return
    ...
    return;
}
```

인식 태스크가 시작 되 고 핸들이 인식 작업 (`SFSpeechRecognitionTask`)으로 유지 됩니다.

```csharp
RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
    ...
});
```

콜백은 미리 기록 된 음성에서 위에 사용 된 것과 비슷한 방식으로 사용 됩니다.

사용자가 기록을 stoped 하는 경우 오디오 엔진과 음성 인식 요청에 모두 알림이 전달 됩니다.

```csharp
AudioEngine.Stop ();
LiveSpeechRequest.EndAudio ();
```

사용자가 인식을 취소 하면 오디오 엔진 및 인식 작업에 대 한 알림이 나타납니다.

```csharp
AudioEngine.Stop ();
RecognitionTask.Cancel ();
```

사용자가 변환을 취소 하 `RecognitionTask.Cancel` 여 메모리와 장치의 프로세서를 모두 확보 하는 경우를 호출 하는 것이 중요 합니다.

> [!IMPORTANT]
> `NSSpeechRecognitionUsageDescription` 또는`var node = AudioEngine.InputNode;`키를 제공 하지 못하면 음성 인식 또는 라이브 오디오의 마이크 ()에 액세스 하려고 할 때 경고 없이 앱에 오류가 발생할 수 있습니다. `NSMicrophoneUsageDescription` `Info.plist` 자세한 내용은 위의 **사용 설명 제공** 섹션을 참조 하세요.

## <a name="speech-recognition-limits"></a>음성 인식 제한

Apple은 iOS 앱에서 음성 인식을 사용할 때 다음과 같은 제한 사항을 적용 합니다.

- 음성 인식은 모든 앱에서 무료로 사용할 수 있지만 사용에는 제한이 없습니다.
  - 개별 iOS 장치에는 하루에 수행할 수 있는 제한 된 수의 인식가 있습니다.
  - 앱은 매일 요청을 통해 전역적으로 제한 됩니다.
- 음성 인식 네트워크 연결 및 사용 빈도 제한 오류를 처리할 수 있도록 앱을 준비 해야 합니다.
- 음성 인식은 사용자의 iOS 장치에서 배터리 드레이닝 및 높은 네트워크 트래픽 모두에 대해 높은 비용을 발생 시킬 수 있습니다 .이 때문에 Apple은 엄격한 오디오 기간 제한을 약 1 분으로 설정 합니다.

앱이 요금 제한 제한에 자주 도달 하는 경우 Apple은 개발자에 게 연락 하도록 요청 합니다.

## <a name="privacy-and-usability-considerations"></a>개인 정보 및 유용성 고려 사항

Apple에는 iOS 앱에서 음성 인식을 포함 하 여 사용자의 개인 정보를 투명 하 고 준수 하기 위한 다음과 같은 제안이 있습니다.

- 사용자의 음성을 기록 하는 경우 앱의 사용자 인터페이스에서 기록이 발생 한다는 것을 명확 하 게 지정 해야 합니다. 예를 들어 앱에서 "기록" 소리를 재생 하 고 기록 표시기를 표시할 수 있습니다.
- 암호, 상태 데이터 또는 재무 정보와 같은 중요 한 사용자 정보에는 음성 인식을 사용 하지 마세요.
- 인식 결과를 작동 _하기 전에_ 표시 합니다. 앱에서 수행 하는 작업에 대 한 피드백을 제공할 뿐만 아니라 사용자가 인식 오류를 처리할 수 있습니다.

## <a name="summary"></a>요약

이 문서에서는 새로운 Speech API를 소개 하 고 Xamarin.ios 앱에서이를 구현 하 여 연속 음성 인식과 높여줄 음성 (라이브 또는 기록 오디오 스트림)을 텍스트로 변환 하는 방법을 살펴보았습니다. 



## <a name="related-links"></a>관련 링크

- [SpeakToMe (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-speaktome)
