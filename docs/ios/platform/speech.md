---
title: Xamarin.iOS에서 음성 인식
description: 이 문서는 새 Speech API를 제공 하 고 연속 음성 인식을 지원 및 음성 (라이브 또는 기록 된 오디오 스트림)에서 텍스트를 기록 하는 Xamarin.iOS 앱에서 구현 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 64FED50A-6A28-4833-BEAE-63CEC9A09010
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: c1f488213f9b3be945fd98e09f630c243d0b0d62
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61382840"
---
# <a name="speech-recognition-in-xamarinios"></a>Xamarin.iOS에서 음성 인식

_이 문서는 새 Speech API를 제공 하 고 연속 음성 인식을 지원 및 음성 (라이브 또는 기록 된 오디오 스트림)에서 텍스트를 기록 하는 Xamarin.iOS 앱에서 구현 하는 방법을 보여 줍니다._

새 Apple ios 10, 텍스트로 연속 음성 인식을 지원 (라이브 또는 기록 된 오디오 스트림)에서 음성 기록 하는 iOS 앱을 허용 하는 음성 인식 API를 릴리스 했습니다.

Apple에 따라 음성 인식 API에는 다음 기능 및 이점에 있습니다.

- 매우 정확 하 게
- 최신
- 사용 하기 쉬운
- Fast
- 여러 언어 지원
- 사용자 개인 정보

## <a name="how-speech-recognition-works"></a>음성 인식의 작동 원리

음성 인식 API를 지 원하는 음성된 언어 중 하나) (에서 라이브 또는 미리 녹음 된 오디오를 가져오고 음성 단어를 일반 텍스트로 기록 반환 하는 음성 인식기에 전달 하 여 iOS 앱에서 구현 됩니다.

[![](speech-images/speech01.png "음성 인식의 작동 원리")](speech-images/speech01.png#lightbox)

### <a name="keyboard-dictation"></a>키보드 받아쓰기

대부분의 사용자가 iOS 장치에서 음성 인식의 생각을 하는 경우 iPhone 4 초를 사용 하 여 iOS 5에서에서 키보드 받아쓰기와 함께 출시 된 기본 제공 Siri 음성 지원을 있다고 생각 합니다.

키보드 받아쓰기 TextKit를 지 원하는 모든 인터페이스 요소에서 지원 됩니다 (같은 `UITextField` 또는 `UITextArea`) (스페이스바 왼쪽)에 직접 iOS 가상 키보드에서 받아쓰기 단추를 클릭 하 여 활성화 됩니다.

Apple (2011 년부터 수집) 다음과 같은 키보드 받아쓰기 통계를 출시 했습니다.

- IOS 5에서에서 릴리스된 이후 키보드 받아쓰기 널리 사용 되었습니다.
- 약 65,000 앱 하루 사용 합니다.
- 모든 iOS의 세 번째 방법에 대 한 받아쓰기 타사 앱에서 수행 됩니다.

키보드 받아쓰기는 TextKit 인터페이스 요소를 사용 하 여 앱의 UI 디자인에서 이외의 개발자의 파트에서 작업 없이 필요 하므로 사용 방법이 매우 간단 합니다. 키보드 받아쓰기 사용 하기 전에 앱에서 모든 특수 권한 요청을 요구 하지 않는 장점이 있습니다.

새 음성 인식 Api를 사용 하는 앱에 음성 인식 전송 및 Apple 서버에서 데이터의 임시 저장소 필요 하므로 사용자가 부여할 특별 한 권한이 해야 합니다. 참조 하십시오 우리의 [보안 및 개인 정보 보호 향상](~/ios/app-fundamentals/security-privacy.md) 세부 정보에 대 한 설명서입니다.

키보드 받아쓰기를 쉽게 구현할 수 있지만 몇 가지 제한 사항 및 단점 가져옵니까 수 있습니다.

- 텍스트 입력 필드를 사용 하 고 키보드를 표시 해야합니다.
- 입력에 해당 하는 실시간 오디오를 사용 하 여 작동 하 고 앱에 오디오 기록 프로세스 제어 하지 않습니다.
- 사용자의 음성을 해석 하는 데 사용 되는 언어를 제어할 수 없거나 제공 합니다.
- 받아쓰기 단추는 사용자에 게 제공 하는 경우 알아야 앱에 대 한 방법이 없습니다.
- 앱 오디오 녹음 프로세스를 사용자 지정할 수 없습니다.
- 타이밍 및 신뢰도 같은 정보에는 매우 단순 결과 집합을 제공 합니다.

### <a name="speech-recognition-api"></a>음성 인식 API

새 Apple ios 10, 음성 인식을 구현 하는 iOS 앱에 대 한 더 강력한 방법을 제공 하는 음성 인식 API를 릴리스 했습니다. 이 API는 Apple Siri와 키보드 받아쓰기 power를 사용 하는 것과 동일한 이므로 최신 정확도 사용 하 여 빠른 기록을 제공할 수 있습니다.

음성 인식 API에서 제공 하는 결과 수집 하거나 모든 개인 사용자 데이터에 액세스 하지 않아도 앱 없이 개별 사용자에 게 사용자 지정 투명 하 게 됩니다.

음성 인식 API를 사용자가 말하고만 텍스트 보다 변환의 결과 대 한 자세한 정보를 제공에서 호출 앱으로 다시 결과 거의 실시간으로 제공 합니다. 여기에는 다음이 포함됩니다.

- 여러 사용자가 말한 해석 합니다.
- 개별 번역에 대 한 신뢰도 수준입니다.
- 타이밍 정보입니다.

위에서 설명한 대로 하거나 라이브 피드를 또는 미리 녹음 된 원본의 50 개 이상의 언어 및 iOS 10에서 지 원하는 언어의 번역에 대 한 오디오를 제공할 수 있습니다.

음성 인식 API를 iOS 10을 실행 하는 모든 iOS 장치에서 사용할 수 있습니다 및 위치 번역 대량의 Apple 서버에서 수행 되므로 대부분의 경우, 실시간 인터넷 연결이 필요 합니다. 즉, 몇 가지 최신 iOS 장치에서 항상 지원, 특정 언어의 장치에서 변환 합니다.

Apple가 지정된 된 언어는 현재 시점에서 번역에 사용할 수 있는지 확인 하기 위한 가용성 API를 포함 합니다. 앱 자체는 인터넷 연결에 대 한 직접 테스트 하는 대신이 API를 사용 해야 합니다.

키보드 받아쓰기 섹션에는 위에서 언급 했 듯이 음성 인식은 전송 및 Apple 서버에서 데이터의 임시 저장소를 인터넷에서 같은 앱 _해야_ 수행 하려면 사용자의 권한을 요청 합니다. 포함 하 여 인식 합니다 `NSSpeechRecognitionUsageDescription` 키 해당 `Info.plist` 파일과 호출은 `SFSpeechRecognizer.RequestAuthorization` 메서드. 

음성 인식에 사용 되는 오디오의 원본에 따라 다른 앱의 변경 `Info.plist` 파일이 필요할 수 있습니다. 참조 하십시오 우리의 [보안 및 개인 정보 보호 향상](~/ios/app-fundamentals/security-privacy.md) 세부 정보에 대 한 설명서입니다.

## <a name="adopting-speech-recognition-in-an-app"></a>앱에서 음성 인식 채택

가지 개발자는 iOS 앱에 음성 인식 기능을 채택 하기 위해 수행 해야 하는 네 가지 주요 단계가 있습니다.

- 앱의 사용량 설명을 `Info.plist` 를 사용 하 여 파일을 `NSSpeechRecognitionUsageDescription` 키입니다. 카메라 앱을 다음 설명에 포함 될 수 있습니다 예를 들어 _"따라서 '치즈'는 마디 하 여 사진을 촬영할 수 있습니다."_
- 호출 하 여 권한 부여를 요청 합니다 `SFSpeechRecognizer.RequestAuthorization` 설명을 제공 하는 방법 (에 제공 된는 `NSSpeechRecognitionUsageDescription` 위의 키) 이유 앱이 음성의 인식 대화 상자에서 사용자에 대 한 액세스 및 사용을 허용 하거나 거부할 수입니다.
- 음성 인식 요청을 만듭니다.
    * 디스크에 미리 녹음 된 오디오에 대 한 사용을 `SFSpeechURLRecognitionRequest` 클래스입니다.
    * 실시간 오디오 (또는 메모리에서 오디오)를 사용 하 여는 `SFSPeechAudioBufferRecognitionRequest` 클래스입니다.
- 음성 인식기에 음성 인식 요청을 전달 (`SFSpeechRecognizer`) 인식 하려면. 앱에 반환 된 선택적으로 보유할 수 `SFSpeechRecognitionTask` 를 모니터링 하 고 인식 결과 추적 합니다.

이러한 단계는 아래에서 자세히 설명 합니다.

### <a name="providing-a-usage-description"></a>사용 설명 제공

필요한 수 있도록 `NSSpeechRecognitionUsageDescription` 키를 `Info.plist` 파일에서 다음을 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 두 번 클릭 합니다 `Info.plist` 파일을 편집용으로 엽니다.
2. 으로 전환 합니다 **원본** 보기: 

    [![](speech-images/speech02.png "소스 보기")](speech-images/speech02.png#lightbox)
3. 클릭할 **새 항목 추가**, 입력 `NSSpeechRecognitionUsageDescription` 에 대 한 합니다 **속성**, `String` 에 대 한 합니다 **형식** 및 **사용 설명** 로 **값**합니다. 예를 들어: 

    [![](speech-images/speech03.png "NSSpeechRecognitionUsageDescription 추가")](speech-images/speech03.png#lightbox)
4. 앱은 실시간 오디오 전사를 처리 하는 경우 마이크 사용 설명도 필요 합니다. 클릭할 **새 항목 추가**, 입력 `NSMicrophoneUsageDescription` 에 대 한 합니다 **속성**, `String` 에 대 한 합니다 **형식** 및 **사용 설명** 로 **값**합니다. 예를 들어: 

    [![](speech-images/speech04.png "NSMicrophoneUsageDescription 추가")](speech-images/speech04.png#lightbox)
4. 파일의 변경 내용을 저장합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 두 번 클릭 합니다 `Info.plist` 파일을 편집용으로 엽니다.
3. 클릭할 **새 항목 추가**, 입력 `NSSpeechRecognitionUsageDescription` 에 대 한 합니다 **속성**, `String` 에 대 한 합니다 **형식** 및 **사용 설명** 로 **값**합니다. 예를 들어: 

    [![](speech-images/speech03w.png "NSSpeechRecognitionUsageDescription 추가")](speech-images/speech03w.png#lightbox)
4. 앱은 실시간 오디오 전사를 처리 하는 경우 마이크 사용 설명도 필요 합니다. 클릭할 **새 항목 추가**, 입력 `NSMicrophoneUsageDescription` 에 대 한 합니다 **속성**, `String` 에 대 한 합니다 **형식** 및 **사용 설명** 로 **값**합니다. 예를 들어: 

    [![](speech-images/speech04w.png "NSMicrophoneUsageDescription 추가")](speech-images/speech04w.png#lightbox)
4. 파일의 변경 내용을 저장합니다.

-----

> [!IMPORTANT]
> 위의 중 하나를 제공 하지 못함 `Info.plist` 키 (`NSSpeechRecognitionUsageDescription` 또는 `NSMicrophoneUsageDescription`) 음성 인식 또는 실시간 오디오에 대 한 마이크에 액세스 하려고 할 때 경고 없이 실패 한 앱에서 발생할 수 있습니다.




### <a name="requesting-authorization"></a>권한 부여를 요청합니다.

응용 프로그램에서 음성 인식 액세스를 허용 하는 데 필요한 사용자 권한 부여를 요청 하려면 기본 뷰 컨트롤러 클래스 편집한 다음 코드를 추가 합니다.

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

`RequestAuthorization` 메서드를 `SFSpeechRecognizer` 클래스에서 제공 하는 개발자 이유를 사용 하 여 액세스 음성 인식에 사용자의 권한을 요청 합니다를 `NSSpeechRecognitionUsageDescription` 의 키를 `Info.plist` 파일.

A `SFSpeechRecognizerAuthorizationStatus` 결과에 반환 됩니다는 `RequestAuthorization` 메서드의 콜백 루틴 작업을 수행 하는 사용자의 사용 권한을 기반으로 합니다. 

> [!IMPORTANT]
> Apple에서 제안 사용자 음성 인식이 사용이 권한을 요청 하기 전에 필요한 앱에서 작업을 시작할 때까지 대기 하 게 합니다.

### <a name="recognizing-pre-recorded-speech"></a>미리 녹음 된 음성 인식

앱에서 미리 녹음 된 WAV 또는 MP3 파일에서 음성 인식 하려는 경우 다음 코드를 따르면 됩니다.

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

세부 정보에서이 코드를 살펴보면 먼저 생성을 시도할 음성 인식기 (`SFSpeechRecognizer`). 음성 인식에 대 한 기본 언어 지원 되지 않는 경우 `null` 반환 되 고 함수 종료 됩니다.

음성 인식기를 기본 언어로 사용할 수 있는 경우 앱 확인 하는 현재 사용 하 여 인식을 사용할 수 있는지 확인 하는 `Available` 속성입니다. 예를 들어 인식 장치 활성 인터넷 연결이 없는 경우 사용할 수 있습니다.

A `SFSpeechUrlRecognitionRequest` 에서 만든는 `NSUrl` iOS 장치에서 미리 기록 된 파일의 위치를 처리할 콜백 루틴을 사용 하 여 음성 인식기에 전달 됩니다.

하는 경우, 콜백이 호출 되는 `NSError` 되지 `null` 처리 해야 하는 동안 오류가 발생 했습니다. 음성 인식, 증분 방식으로 수행 되므로 콜백 루틴을 호출할 수 있으므로 한 번 이상 `SFSpeechRecognitionResult.Final` 속성은 변환을 완료 되 고 가장 적합 한 번역 버전 기록 됩니다 확인 하려면 테스트 (`BestTranscription`).

### <a name="recognizing-live-speech"></a>실시간 음성 인식

앱에서 실시간 음성 인식 하려는 경우 프로세스 미리 녹음 된 음성 인식에 매우 비슷합니다. 예를 들어:

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

세부 정보에서이 코드를 살펴보면 인식 프로세스를 처리 하는 여러 private 변수 생성 됩니다.

```csharp
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
```

AV Foundation에 전달 되는 레코드 오디오를 사용 하 여는 `SFSpeechAudioBufferRecognitionRequest` 인식 요청을 처리 합니다.

```csharp
var node = AudioEngine.InputNode;
var recordingFormat = node.GetBusOutputFormat (0);
node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
    // Append buffer to recognition request
    LiveSpeechRequest.Append (buffer);
});
```

기록 시작 하려고 시도 하는 앱 및 녹음/녹화를 시작할 수 없는 경우 모든 오류를 처리 합니다.

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

인식 작업을 시작 하 고 인식 작업에 대 한 핸들은 유지 (`SFSpeechRecognitionTask`):

```csharp
RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
    ...
});
```

콜백 미리 녹음 된 음성의 위에서 사용 하는 비슷한 방식으로 사용 됩니다.

사용자가 기록을 stoped 됩니다, 경우 오디오 엔진 및 음성 인식 요청 내용 알림이 표시 됩니다.

```csharp
AudioEngine.Stop ();
LiveSpeechRequest.EndAudio ();
```

사용자 취소 인식, 오디오 엔진 및 인식 작업 정보는:

```csharp
AudioEngine.Stop ();
RecognitionTask.Cancel ();
```

호출 하는 것이 반드시 `RecognitionTask.Cancel` 사용자가 장치의 프로세서와 메모리를 확보 하려면 번역을 취소 합니다.

> [!IMPORTANT]
> 제공 하는 데 실패 합니다 `NSSpeechRecognitionUsageDescription` 또는 `NSMicrophoneUsageDescription` `Info.plist` 키 음성 인식 또는 실시간 오디오에 대 한 마이크에 액세스 하려고 할 때 경고 없이 실패 한 앱에서 발생할 수 있습니다 (`var node = AudioEngine.InputNode;`). 참조 하십시오 합니다 **사용량 설명을 제공 하** 자세한 내용은 위의 섹션.

## <a name="speech-recognition-limits"></a>음성 인식 제한

Apple는 iOS 앱에서 음성 인식 기능을 사용 하 여 작업할 때 다음과 같은 제한 사항이 적용:

- 음성 인식 모든 앱에 무료 이지만 용도 무제한 아닙니다.
    - 개별 iOS 장치 제한 많은 하루 수행할 수 있는 개 인식 합니다.
    - 앱을 매일 요청 단위로 전역적으로 제한 됩니다.
- 음성 인식 네트워크 연결 및 사용 현황 속도 제한 오류를 처리 하는 앱을 준비 되어야 합니다.
- Apple 음성 최대 약 1 분의 엄격한 오디오 기간 제한을 음성 인식을 사용자의 iOS 장치에서이 때문에 배터리가와 높은 네트워크 트래픽 비용을 높이 가질 수 있습니다.

앱의 속도 제한 한도 도달한 정기적으로, Apple 개발자 연락 여 묻습니다.

## <a name="privacy-and-usability-considerations"></a>개인 정보 보호 및 사용 편의성 고려 사항

Apple에 투명 하 게 되 고 iOS 앱에 음성 인식 기능을 포함 하는 경우 사용자의 개인 정보를 존중 하는 것에 대 한 다음 제안에 있습니다.

- 사용자의 음성을 녹음 하는 경우에 기록 수행 되는 앱의 사용자 인터페이스를 명확히 표시 해야 합니다. 예를 들어, 앱 기록을 재생 하는 "" 사운드 하 고 기록 표시기를 표시할 수 있습니다.
- 재무 정보, 암호 및 의료 데이터 등 중요 한 사용자 정보에 대 한 음성 인식 기능을 사용 하지 마세요.
- 인식 결과 보여 줍니다 _하기 전에_ 에 대해 작동 합니다. 이 앱은 작업을 수행 되지만 이루어지기 인식 오류를 처리할 수 있도록 하는 것에 대 한 피드백 뿐만 아니라 제공 합니다.

## <a name="summary"></a>요약

이 문서에 새 Speech API를 표시 하 고 연속 음성 인식을 지원 및 음성 (라이브 또는 기록 된 오디오 스트림)에서 텍스트를 기록 하는 Xamarin.iOS 앱을 구현 하는 방법을 살펴보았습니다. 



## <a name="related-links"></a>관련 링크

- [SpeakToMe (샘플)](https://developer.xamarin.com/samples/monotouch/ios10/SpeakToMe/)
