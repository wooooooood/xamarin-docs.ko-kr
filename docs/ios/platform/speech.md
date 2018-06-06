---
title: Xamarin.iOS의 음성 인식
description: 이 문서는 새로운 음성 API를 표시 하 고 연속 음성 인식을 지원 및 음성 (라이브 또는 기록 된 오디오 스트림)에서 텍스트로 변환 하도록 Xamarin.iOS 응용 프로그램에서 구현 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 64FED50A-6A28-4833-BEAE-63CEC9A09010
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 00841a73f9da3c4c434419cdb37726b17c08cf31
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788363"
---
# <a name="speech-recognition-in-xamarinios"></a>Xamarin.iOS의 음성 인식

_이 문서는 새로운 음성 API를 표시 하 고 연속 음성 인식을 지원 및 음성 (라이브 또는 기록 된 오디오 스트림)에서 텍스트로 변환 하도록 Xamarin.iOS 응용 프로그램에서 구현 하는 방법을 보여 줍니다._

새 Apple iOS 10에 텍스트로 연속 음성 인식을 지원 및 음성 (라이브 또는 기록 된 오디오 스트림)에서 변환 하도록 iOS 앱을 허용 하는 음성 인식을 API 릴리스 했습니다.

Apple에 따라 음성 인식 API에는 다음 기능과 이점:

- 정확도 높은
- 최신
- 간편한 사용
- Fast
- 여러 언어 지원
- 측면 사용자 개인 정보 보호

## <a name="how-speech-recognition-works"></a>음성 인식 작동 방식

음성 인식 (에 지 원하는 API 통용된 언어 중 하나로) 라이브 또는 미리 녹음 된 오디오를 획득 하는 단어는 일반 텍스트로 기록 반환 하는 음성 인식기에 전달 하 여 iOS 앱에 구현 됩니다.

[![](speech-images/speech01.png "음성 인식 작동 방식")](speech-images/speech01.png#lightbox)

### <a name="keyboard-dictation"></a>키보드 받아쓰기

대부분의 사용자가 iOS 장치에서 음성 인식을 생각 하면, 설치 된 iOS 5 iPhone 4S에에서 키보드 받아쓰기와 함께 출시 된 기본 제공 Siri 음성 지원을 생각 합니다.

키보드 받아쓰기 TextKit를 지 원하는 모든 인터페이스 요소에서 지원 됩니다 (같은 `UITextField` 또는 `UITextArea`) iOS 가상 키보드 (스페이스바 왼쪽)에 직접 받아쓰기 단추를 클릭 하는 사용자가 활성화 됩니다.

Apple (2011 이후 수집 됨) 다음과 같은 키보드 받아쓰기 통계를 출시 했습니다.

- IOS 5에에서 출시 된 이후에 키보드 받아쓰기 광범위 하 게 사용 되었습니다.
- 대략 65, 000 앱 하루 사용 합니다.
- 모든 iOS의 3에 대 한 받아쓰기 3 번째 파티 앱에서 수행 됩니다.

키보드 받아쓰기는 작업 없이 TextKit 인터페이스 요소를 사용 하 여 응용 프로그램의 UI 디자인에 이외의 개발자의 부분에 있어야 하므로 사용 방법이 매우 간단 합니다. 키보드 받아쓰기는 또한 사용 하려면 먼저 앱에서 모든 특별 한 권한 요청을 필요로 하지 않는다는 이점이 있습니다.

새 음성 인식 Api를 사용 하는 앱에는 전송 및 Apple 서버에서 데이터의 임시 저장소 음성 인식 필요 하므로 사용자가 부여할 특별 한 권한이 필요 합니다. 참조 하십시오 우리의 [보안 및 개인 정보 보호 향상](~/ios/app-fundamentals/security-privacy.md) 세부 정보에 대 한 설명서입니다.

키보드 받아쓰기는 구현 하기가, 몇 가지 제한 사항 및 단점 가져옵니까:

- 텍스트 입력 필드의 용도 및 키보드 표시 해야합니다.
- 실시간 오디오만 입력와 함께 작동 하 고 응용 프로그램에 오디오 녹음 프로세스 제어할 수 없습니다.
- 제어할 수 없거나 사용자의 음성을 해석 하는 데 사용 되는 언어를 제공 합니다.
- 받아쓰기 단추는 사용자에 게도 사용할 수에 대해 알아야 앱에 대 한 방법이 없습니다.
- 응용 프로그램에는 오디오 녹음 프로세스 사용자 지정할 수 없습니다.
- 타이밍 및 정확도 같은 정보가 부족 하는 매우 단순 하는 결과 집합을 제공 합니다.

### <a name="speech-recognition-api"></a>음성 인식 API

새 Apple iOS 10에 음성 인식 기능을 구현 하는 iOS 앱에 대 한 더 강력한 방법을 제공 하는 음성 인식을 API를 출시 했습니다. 이 API는 Apple Siri와 키보드 받아쓰기 전원을 사용 하는 것과 동일한 지 며 최신 판단할 고속 기록을 제공할 수 있습니다.

음성 인식 API에서 제공 하는 결과 앱 수집 하거나 모든 사용자 개인 데이터에 액세스할 필요 없이 개별 사용자에 게 사용자 지정 투명 하 게 됩니다.

음성 인식 API에서 호출 응용 프로그램에 다시 결과 거의 실시간 텍스트만 보다 번역의 결과 대 한 자세한 정보를 제공 하 고 사용자를 말할을 제공 합니다. 여기에는 다음이 포함됩니다.

- 어떤 사용자의 여러 가지로 해석 되어야합니다.
- 개별 번역에 대 한 신뢰도 수준입니다.
- 타이밍 정보입니다.

위에서 설명 했 듯이 중 하나는 라이브 피드 또는 미리 녹음 된 소스에서 옵션과에서 50 개 이상의 언어와 iOS 10에서 지 원하는 언어 중 번역에 대 한 오디오를 제공할 수 있습니다.

음성 인식 API 10의 iOS를 실행 하는 모든 iOS 장치에서 사용할 수 있으며 번역 대량의 Apple 서버에서 발생 하므로 대부분의 경우에서 라이브 인터넷 연결을 요구 합니다. 즉, 몇 가지 최신 iOS 장치에는 항상 지원, 특정 언어의 장치에 변환 합니다.

Apple가 지정된 된 언어는 현재 시점에서 변환에 사용할 수 있는지 확인 하는 가용성 API를 포함 합니다. 응용 프로그램 자체는 인터넷 연결을 직접 테스트 하는 대신이 API를 사용 해야 합니다.

키보드 받아쓰기 섹션에 위에서 언급 한 대로 음성 인식 등 응용 프로그램으로 및 인터넷을 통해 전송 및 Apple 서버에서 데이터를 임시로 저장을 필요 _해야_ 수행 하는 사용자의 권한 요청 포함 하 여 인식은 `NSSpeechRecognitionUsageDescription` 키에 해당 `Info.plist` 파일과 호출은 `SFSpeechRecognizer.RequestAuthorization` 메서드. 

음성 인식 기능에 사용 되는 오디오의 소스에 따라, 변경 된 기타 응용 프로그램의 `Info.plist` 파일이 필요할 수 있습니다. 참조 하십시오 우리의 [보안 및 개인 정보 보호 향상](~/ios/app-fundamentals/security-privacy.md) 세부 정보에 대 한 설명서입니다.

## <a name="adopting-speech-recognition-in-an-app"></a>음성 인식 응용 프로그램의 채택

가지 개발자 음성 인식 iOS 앱의 채택 하기 위해 수행 해야 하는 4 개의 주요 단계가 있습니다.

- 에 응용 프로그램의 사용량 설명을 제공 `Info.plist` 사용 하 여 파일의 `NSSpeechRecognitionUsageDescription` 키입니다. 카메라 앱 다음 설명을 같습니다. 예를 들어 _"이 수 있도록 'cheese' 라는 단어를 말하는 것 만으로도 사진을 찍어."_
- 호출 하 여 권한 부여를 요청는 `SFSpeechRecognizer.RequestAuthorization` 설명을 표시 하는 메서드 (에서 제공 되는 `NSSpeechRecognitionUsageDescription` 위의 키) 이유 앱이 음성의 인식 대화 상자에서 사용자에 대 한 액세스 및 수락 또는 거부 하도록 허용 합니다.
- 음성 인식 요청을 만듭니다.
    * 디스크에 미리 녹음 된 오디오 사용은 `SFSpeechURLRecognitionRequest` 클래스입니다.
    * 실시간 오디오 (또는 메모리에서 오디오)에 대 한 사용은 `SFSPeechAudioBufferRecognitionRequest` 클래스입니다.
- 음성 인식기에 음성 인식 요청을 전달 (`SFSpeechRecognizer`) 인식을 시작 합니다. 응용 프로그램에 반환 된 선택적으로 보유할 수 `SFSpeechRecognitionTask` 를 모니터링 하 고 인식 결과 추적 합니다.

이러한 단계는 아래에서 자세히 설명 합니다.

### <a name="providing-a-usage-description"></a>사용 설명을 제공합니다.

필요한 제공 하기 `NSSpeechRecognitionUsageDescription` 키에 `Info.plist` 파일에서 다음을 수행 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 두 번 클릭 하 여 `Info.plist` 편집을 위해 열 파일입니다.
2. 전환 하는 **소스** 보기: 

    [![](speech-images/speech02.png "소스 뷰")](speech-images/speech02.png#lightbox)
3. 클릭 **새 항목 추가**, 입력 `NSSpeechRecognitionUsageDescription` 에 대 한는 **속성**, `String` 에 대 한는 **형식** 및 **사용 설명을** 로 **값**합니다. 예를 들어: 

    [![](speech-images/speech03.png "NSSpeechRecognitionUsageDescription 추가")](speech-images/speech03.png#lightbox)
4. 응용 프로그램은 실시간 오디오 기록을 처리 하는 경우 마이크 사용 설명도 필요 합니다. 클릭 **새 항목 추가**, 입력 `NSMicrophoneUsageDescription` 에 대 한는 **속성**, `String` 에 대 한는 **형식** 및 **사용 설명을** 로 **값**합니다. 예를 들어: 

    [![](speech-images/speech04.png "NSMicrophoneUsageDescription 추가")](speech-images/speech04.png#lightbox)
4. 파일의 변경 내용을 저장합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 두 번 클릭 하 여 `Info.plist` 편집을 위해 열 파일입니다.
3. 클릭 **새 항목 추가**, 입력 `NSSpeechRecognitionUsageDescription` 에 대 한는 **속성**, `String` 에 대 한는 **형식** 및 **사용 설명을** 로 **값**합니다. 예를 들어: 

    [![](speech-images/speech03w.png "NSSpeechRecognitionUsageDescription 추가")](speech-images/speech03w.png#lightbox)
4. 응용 프로그램은 실시간 오디오 기록을 처리 하는 경우 마이크 사용 설명도 필요 합니다. 클릭 **새 항목 추가**, 입력 `NSMicrophoneUsageDescription` 에 대 한는 **속성**, `String` 에 대 한는 **형식** 및 **사용 설명을** 로 **값**합니다. 예를 들어: 

    [![](speech-images/speech04w.png "NSMicrophoneUsageDescription 추가")](speech-images/speech04w.png#lightbox)
4. 파일의 변경 내용을 저장합니다.

-----

> [!IMPORTANT]
> 위의 항목 중 하나를 제공 하지 못하면 `Info.plist` 키 (`NSSpeechRecognitionUsageDescription` 또는 `NSMicrophoneUsageDescription`) 음성 인식 또는 실시간 오디오 마이크 중 하나에 액세스 하려고 할 때 경고 없이 실패 한 응용 프로그램에서 발생할 수 있습니다.




### <a name="requesting-authorization"></a>권한 부여를 요청합니다.

음성 인식 기능에 액세스할 수 있도록 허용 하는 데 필요한 사용자 권한 부여를 요청 하려면 주 뷰-컨트롤러 클래스를 편집 하 고 다음 코드를 추가 합니다.

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

`RequestAuthorization` 의 메서드는 `SFSpeechRecognizer` 클래스에서 제공 하는 개발자는 이유를 사용 하 여 액세스 음성 인식 하는 사용자 로부터 권한을 요청 합니다는 `NSSpeechRecognitionUsageDescription` 의 키는 `Info.plist` 파일입니다.

A `SFSpeechRecognizerAuthorizationStatus` 결과에 반환 되는 `RequestAuthorization` 사용자의 허가에 따라 조치를 사용할 수 있는 방법의 콜백 루틴입니다. 

> [!IMPORTANT]
> Apple는 사용자 동작이 사용이 권한을 요청 하기 전에 음성 인식을 요구 하는 앱에서 시작할 때까지 대기 하는 것을 제안 합니다.

### <a name="recognizing-pre-recorded-speech"></a>미리 녹음 된 음성 인식

응용 프로그램을 미리 녹음된 WAV 또는 MP3 파일에서 음성 인식 하려는 경우 다음 코드를 사용할 수 있습니다.

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

이 코드를 자세히 보기를 먼저 생성을 시도할 Speech Recognizer (`SFSpeechRecognizer`). 음성 인식 기능에 대 한 기본 언어는 지원 되지 않으면 `null` 반환 되 고 함수 종료 됩니다.

응용 프로그램에 현재 사용 하 여 인식에 사용할 수 있는지 확인 음성 인식기에서 기본 언어에 사용할 수 있는 경우는 `Available` 속성입니다. 예를 들어 인식 하지 못할 수도 있습니다는 장치는 활성 인터넷 연결 없는 경우.

A `SFSpeechUrlRecognitionRequest` 에서 만든는 `NSUrl` iOS 장치에서 미리 녹음 된 파일의 위치는 콜백 루틴 처리할 수 음성 인식기에 전달 됩니다.

하는 경우, 콜백이 호출 되는 `NSError` 없는 `null` 처리 해야 하는 동안 오류가 발생 했습니다. 음성 인식으로 증분 방식으로 수행 때문에 콜백 루틴을 호출할 수 있으므로 한 번 이상 `SFSpeechRecognitionResult.Final` 변환이 완료 되 고 작성 된 번역의 적합 한 버전을 확인 하려면 테스트 속성 (`BestTranscription`).

### <a name="recognizing-live-speech"></a>라이브 음성 인식

응용 프로그램에서 라이브 음성 인식 하도록 하려는 경우 과정이 미리 녹음 된 음성 인식와 매우 비슷합니다. 예를 들어:

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

이 코드를 자세히 살펴보면, 여러 개인 변수 인식 프로세스 처리를 생성 됩니다.

```csharp
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
```

사용 하 여 AV Foundation에 전달 되는 오디오 녹음는 `SFSpeechAudioBufferRecognitionRequest` 인식 요청을 처리할:

```csharp
var node = AudioEngine.InputNode;
var recordingFormat = node.GetBusOutputFormat (0);
node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
    // Append buffer to recognition request
    LiveSpeechRequest.Append (buffer);
});
```

응용 프로그램 기록을 시작 하 고 기록을 시작할 수 없는 경우 모든 오류가 처리 됩니다.

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

인식 작업을 시작 하 고 인식 작업에 대 한 핸들 보관 됩니다 (`SFSpeechRecognitionTask`):

```csharp
RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
    ...
});
```

콜백은 미리 녹음 된 음성에 위의 사용 하는 유사한 방식으로 사용 됩니다.

녹음/녹화는 사용자가 중지, 오디오 엔진 및 음성 인식 요청 정보:

```csharp
AudioEngine.Stop ();
LiveSpeechRequest.EndAudio ();
```

사용자가 인식을 취소 하는 경우 다음 오디오 엔진과 인식 작업 내용 알림이 표시 됩니다.

```csharp
AudioEngine.Stop ();
RecognitionTask.Cancel ();
```

호출 해야 `RecognitionTask.Cancel` 사용자가 장치의 프로세서와 메모리를 확보 하려면 번역을 취소 합니다.

> [!IMPORTANT]
> 제공 하지 못하면는 `NSSpeechRecognitionUsageDescription` 또는 `NSMicrophoneUsageDescription` `Info.plist` 키 음성 인식 또는 실시간 오디오 마이크 중 하나에 액세스 하려고 할 때 경고 없이 실패 한 응용 프로그램에서 발생할 수 있습니다 (`var node = AudioEngine.InputNode;`). 참조 하십시오는 **사용 설명을 제공 하** 자세한 내용은 섹션.

## <a name="speech-recognition-limits"></a>음성 인식 제한

Apple는 iOS 앱에서 음성 인식 하 여 작업할 때 다음과 같은 제한 사항이 있습니다.

- 음성 인식을 모든 앱에 있지만 해당 사용법은 무제한:
    - 개별 iOS 장치는 하루에 수행할 수 있는 인증 내역 수가 제한 합니다.
    - 앱 매일 요청 별로 전체적으로 제한 됩니다.
- 음성 인식 네트워크 연결 및 사용 현황 속도 제한 오류를 처리 하는 앱을 준비 되어야 합니다.
- 음성 인식이 때문에 사용자의 iOS 장치에 배터리 소모와 높은 네트워크 트래픽 비용을 높이 미칠 수 있습니다, 그리고 Apple 문장 최대 약 1 분에서 엄격한 오디오 지속 시간 제한을 설정 합니다.

앱 속도 제한을 제한에 도달 정기적으로, 사과 개발자 접속 하 묻습니다.

## <a name="privacy-and-usability-considerations"></a>개인 정보 보호 및 사용 가능성 고려 사항

Apple에 투명 하 게 되 고 음성 인식 iOS 앱에 포함 되어 있는 경우 사용자의 개인 정보를 존중 하는 데 다음 적절 합니다.

- 사용자의 음성 녹음을 명확 하 게를 녹음/녹화 수행 되 고 응용 프로그램의 사용자 인터페이스에서 표시 해야 합니다. 예를 들어 응용 프로그램 기록을 재생 하는 "" 사운드 및 녹음/녹화 표시기가 표시 될 수 있습니다.
- 암호, 상태 데이터 또는 재무 정보와 같은 중요 사용자 정보에 대 한 음성 인식 기능을 사용 하지 마십시오.
- 인식 결과 표시 _전에_ 에 대해 작동 합니다. 이 뿐만 아니라 응용 프로그램을 수행 중인 하지만 내용이 인식 오류를 처리할 수 있도록 대 한 피드백을 제공 합니다.

## <a name="summary"></a>요약

이 문서에 새 음성 API를 표시 하 고 연속 음성 인식을 지원 및 음성 (라이브 또는 기록 된 오디오 스트림)에서 텍스트로 변환 하도록 Xamarin.iOS 앱에서 구현 하는 방법을 배웠습니다. 



## <a name="related-links"></a>관련 링크

- [SpeakToMe (샘플)](https://developer.xamarin.com/samples/monotouch/ios10/SpeakToMe/)
