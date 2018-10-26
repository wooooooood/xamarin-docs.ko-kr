---
title: Android 음성
description: 이 문서에서는 매우 강력한 Android.Speech 네임 스페이스를 사용 하 여의 기본 사항을 다룹니다. 처음 등장할 Android는 음성 인식 및 텍스트로 출력 수 있게 되었습니다. 비교적 간단한 프로세스입니다. 그러나 텍스트 음성 변환에 대 한 프로세스 작업은 좀 더 뿐만 아니라는 음성 엔진의 계정으로 수행할 수 있지만 사용 가능 하 고 텍스트 음성 변환 (TTS) 시스템에서 설치 된 언어도 합니다.
ms.prod: xamarin
ms.assetid: FA3B8EC4-34D2-47E3-ACEA-BD34B28115B9
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/02/2018
ms.openlocfilehash: e88f6e24cbf4c8b2f0c0486c6408e234e87066cc
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104351"
---
# <a name="android-speech"></a>Android 음성

_이 문서에서는 매우 강력한 Android.Speech 네임 스페이스를 사용 하 여의 기본 사항을 다룹니다. 처음 등장할 Android는 음성 인식 및 텍스트로 출력 수 있게 되었습니다. 비교적 간단한 프로세스입니다. 그러나 텍스트 음성 변환에 대 한 프로세스 작업은 좀 더 뿐만 아니라는 음성 엔진의 계정으로 수행할 수 있지만 사용 가능 하 고 텍스트 음성 변환 (TTS) 시스템에서 설치 된 언어도 합니다._

## <a name="speech-overview"></a>음성 개요

"인식" 실제 음성 입력 내용을 enunciates 있으며 시스템에 필요-Speech to Text 및 텍스트 음성 변환-점점 영역 내의 모바일 개발은이 장치를 사용 하 여 자연 스러운 통신에 대 한 요구가 증가 합니다. 여러 인스턴스가 있는 텍스트를 음성으로 변환 하거나 그 반대의 경우는 android 응용 프로그램에 통합 하는 매우 유용한 도구는 기능을 필요 합니다.

예를 들어 사용자 구동 하는 동안 휴대폰 사용 시 제한 아래로 사용 하 여 해당 장치 운영의 손에 무료 방식 으로를 원합니다. 다양 한 Android 다양 한 폼 팩터로의-Android Wear 같은-계속 넓히고 포함 된 Android 장치 (예: 태블릿 및 참고 패드)를 사용 하 여 수를 만들었는지 큰 중점을 뛰어난 TTS 응용 프로그램에 있습니다.

Google 장치 "음성 인식" (예: for the blind 설계 된 소프트웨어)의 대부분을 포함 하는 풍부한 Api 집합이 Android.Speech 네임 스페이스에서를 사용 하 여 개발자를 제공 합니다.  네임 스페이스를 통해 음성으로 변환 해야 하는 텍스트를 허용 하는 기능을 포함 `Android.Speech.Tts`, 다양 한 뿐만 아니라 번역 하는 데 사용 되는 엔진에 대 한 제어 `RecognizerIntent`음성 텍스트 변환 될 수 있는 s입니다.

기능 이해할 수 있도록 음성에 대 한 사항이 하는 동안 사용 되는 하드웨어를 기반으로 하는 제한이 있습니다. 그럴 가능성은 장치에 사용 가능한 모든 언어에서 말하는 모든 해석 성공적으로 합니다.

## <a name="requirements"></a>요구 사항

이 가이드에서는 장치의 마이크와 스피커 이외의 특별 한 요구 사항이 없습니다 있습니다.

음성을 해석 하는 Android 장치의 핵심은 사용 하는 `Intent` 해당 `OnActivityResult`합니다.
하기가 중요 하지만 음성 인식 하지 못하면 인식-하지만 텍스트를 해석 합니다. 차이점은 중요 합니다.

### <a name="the-difference-between-understanding-and-interpreting"></a>이해 및 해석의 차이점

이해에 대 한 간단한 정의 언급 되는 내용을 의미 톤 및 컨텍스트를 확인할 수 있다는 것입니다. 방금 해석에 단어를 다른 형식으로 출력을 의미 합니다.

일상적인 대화에 사용 되는 다음 간단한 예제를 살펴보세요. 

<kbd>안녕하세요, 어떻게 지내세요?</kbd>

변 곡점 (초점을 맞춘 특정 단어나 단어의 일부)에 없는 것이 간단한 질문 합니다. 그러나 느린 속도로 줄에 적용 되는 질문자 너무 좋아하지 않습니다 하며 아마도 cheering 없거나는 질문자 잘 수신 대기 하는 사용자 감지 합니다. 중점을 두는 경우에 "are", 요청 하는 사용자는 일반적으로 응답 합니다.

확인을 처리 하는 매우 강력한 오디오 없이 억양과 AI (인공 지능) 수준의 컨텍스트를 이해를 사용 하 여, 소프트웨어 언급 된 내용 알아야도 시작할 수 없습니다-가장 간단한 휴대폰 작업을 수행할 수는 음성을 텍스트로 변환 됩니다.

## <a name="setting-up"></a>설정

음성 시스템을 사용 하기 전에 항상 것은 장치에서 마이크 있는지 확인 하는 것이 좋습니다. 설치 마이크가 없는 Kindle 또는 Google 메모장에서 앱을 실행 하려고 하는 이유가 거의 없습니다 것입니다.

다음 코드 예제를 보여 줍니다 마이크를 사용할 수 있는 경우와 그렇지 않은 경우 쿼리 경고를 만들려면. 없는 마이크를 사용할 수 있는 경우이 시점에서 활동을 종료 하거나 음성을 기록 하는 기능을 사용 하지 않도록 설정 합니다.

```csharp
string rec = Android.Content.PM.PackageManager.FeatureMicrophone;
if (rec != "android.hardware.microphone")
{
    var alert = new AlertDialog.Builder(recButton.Context);
    alert.SetTitle("You don't seem to have a microphone to record with");
    alert.SetPositiveButton("OK", (sender, e) =>
    {
        return;
    });
    alert.Show();
}
```

### <a name="creating-the-intent"></a>의도 만들기

음성 시스템에 대 한 의도 의도 호출의 특정 형식을 사용 하 여 `RecognizerIntent`입니다. 이 모든 추가 언어를 인식 하 고 출력을 통해 녹음/녹화 비율은 될 때까지 대기를 사용 하 여 대기 기간 등 매개 변수 및 모든 텍스트에 포함할 수가 많은 제어를 `Intent`의 명령의 수단으로 모달 대화 상자. 이 코드 조각 `VOICE` 은 `readonly int` 의 인식에 사용 되는 `OnActivityResult`합니다.

```csharp
var voiceIntent = new Intent(RecognizerIntent.ActionRecognizeSpeech);
voiceIntent.PutExtra(RecognizerIntent.ExtraLanguageModel, RecognizerIntent.LanguageModelFreeForm);
voiceIntent.PutExtra(RecognizerIntent.ExtraPrompt, Application.Context.GetString(Resource.String.messageSpeakNow));
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputCompleteSilenceLengthMillis, 1500);
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputPossiblyCompleteSilenceLengthMillis, 1500);
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputMinimumLengthMillis, 15000);
voiceIntent.PutExtra(RecognizerIntent.ExtraMaxResults, 1);
voiceIntent.PutExtra(RecognizerIntent.ExtraLanguage, Java.Util.Locale.Default);
StartActivityForResult(voiceIntent, VOICE);
```

### <a name="conversion-of-the-speech"></a>음성의 변환

음성에서 해석 된 텍스트 내에서 배달할 합니다 `Intent`, 작업이 완료 되 고를 통해 액세스 하는 경우 반환 되는 `GetStringArrayListExtra(RecognizerIntent.ExtraResults)`합니다. 이 반환 됩니다는 `IList<string>`는 인덱스 수 사용 되며 호출자 의도에서 요청 된 언어의 수에 따라 표시의 (지정 되어를 `RecognizerIntent.ExtraMaxResults`). 으로 목록에 그러나 가치가 데이터를 표시할 수 있는지 확인 하기 위해 검사 합니다.

반환 값에 대 한 수신 대기 하는 경우는 `StartActivityForResult`, `OnActivityResult` 메서드를 제공 해야 합니다.

아래 예에서 `textBox` 는 `TextBox` 무엇을 지정 된 출력에 사용 되는 합니다. 인터프리터 및 여기에서 일종의 텍스트를 전달할 동일 하 게 사용할 수 없습니다, 그리고 응용 프로그램은 텍스트와 응용 프로그램의 다른 부분으로 분기를 비교할 수 있습니다.

```csharp
protected override void OnActivityResult(int requestCode, Result resultVal, Intent data)
{
    if (requestCode == VOICE)
    {
        if (resultVal == Result.Ok)
        {
            var matches = data.GetStringArrayListExtra(RecognizerIntent.ExtraResults);
            if (matches.Count != 0)
            {
                string textInput = textBox.Text + matches[0];
                textBox.Text = textInput;
                switch (matches[0].Substring(0, 5).ToLower())
                {
                    case "north":
                        MovePlayer(0);
                        break;
                    case "south":
                        MovePlayer(1);
                        break;
                }
            }
            else
            {
                textBox.Text = "No speech was recognised";
            }
        }
        base.OnActivityResult(requestCode, resultVal, data);
    }
}
```

## <a name="text-to-speech"></a>텍스트 음성 변환

텍스트 음성 변환 텍스트를 음성 역방향 상당히 아니며 두 가지 주요 구성 요소 사용 텍스트 음성 변환 엔진을 장치에 설치 되 고 언어를 설치 합니다.

Android 장치 기본 제공 되는 대부분을 설치 하는 Google TTS 서비스 및 하나 이상의 언어입니다. 장치는 먼저 설정 하 고 시 장치를 되는 위치에 따라 달라 집니다이 설정 됩니다 (예를 들어 독일에서 휴대폰 설치 독일어를 하나 미국에서 영어 (미국) 해야 하는 반면).

### <a name="step-1---instantiating-texttospeech"></a>1 단계-인스턴스화하 TextToSpeech

`TextToSpeech` 최대 3 개의 매개 변수를 사용할 수 있습니다, 첫째와 둘째가 셋째 필요 선택 사항 (`AppContext`하십시오 `IOnInitListener`, `engine`). 수신기는 서비스 및 실패에 대 한 테스트는 엔진이 사용 가능한 Android 텍스트 음성 변환 엔진을 개수에 관계 없이 바인딩할 사용 됩니다. 최소 장치는 Google의 엔진을 해야 합니다.

### <a name="step-2---finding-the-languages-available"></a>2 단계-사용 가능한 언어 찾기

합니다 `Java.Util.Locale` 라는 유용한 메서드를 포함 하는 클래스 `GetAvailableLocales()`합니다. 이 목록은 음성 엔진에서 지 원하는 언어의 설치 된 언어에 대 한 다음 테스트할 수 있습니다.

그리 "인식" 언어 목록을 생성 하는 것입니다. 기본 언어 (언어는 먼저 해당 장치를 설정 하는 경우 사용자 설정)은 항상 따라서이 예제의 합니다 `List<string>` "Default"는 첫 번째 매개 변수로 목록의 나머지를 결과에 따라 채워집니다는 `textToSpeech.IsLanguageAvailable(locale)`합니다.

```csharp
var langAvailable = new List<string>{ "Default" };
var localesAvailable = Java.Util.Locale.GetAvailableLocales().ToList();
foreach (var locale in localesAvailable)
{
    var res = textToSpeech.IsLanguageAvailable(locale);
    switch (res)
    {
        case LanguageAvailableResult.Available:
          langAvailable.Add(locale.DisplayLanguage);
          break;
        case LanguageAvailableResult.CountryAvailable:
          langAvailable.Add(locale.DisplayLanguage);
          break;
        case LanguageAvailableResult.CountryVarAvailable:
          langAvailable.Add(locale.DisplayLanguage);
          break;
    }
}
langAvailable = langAvailable.OrderBy(t => t).Distinct().ToList();
```

이 코드는 호출 [TextToSpeech.IsLanguageAvailable](https://developer.xamarin.com/api/member/Android.Speech.Tts.TextToSpeech.IsLanguageAvailable/p/Java.Util.Locale/) 로캘의 언어 패키지를 장치에 이미 있으면 테스트 합니다. 이 메서드가 반환 된 [LanguageAvailableResult](https://developer.xamarin.com/api/type/Android.Speech.Tts.LanguageAvailableResult/), 전달 된 로캘에 대 한 언어를 사용할 수 있는지 여부를 나타냅니다. 하는 경우 `LanguageAvailableResult` 언어 임을 나타냅니다 `NotSupported`, 음성 패키지가 없는 다운로드할 수 있습니다 (도)은 해당 언어에 대 한 합니다. 하는 경우 `LanguageAvailableResult` 로 설정 되어 `MissingData`, 4 단계에서에서 아래 설명 된 대로 새 언어 패키지를 다운로드 하는 것이 가능 합니다.

### <a name="step-3---setting-the-speed-and-pitch"></a>3 단계-빠르고 피치를 설정 합니다.

Android를 변경 하 여 음성의 소리를 변경할 수 있도록 합니다 `SpeechRate` 및 `Pitch` (속도 및 음성의 목소리 속도). 이 이동 0에서 1, "normal" 음성 둘 다에 대해 1을 사용 하 여 합니다.

### <a name="step-4---testing-and-loading-new-languages"></a>4 단계-테스트 및 새로운 언어를 로드 합니다.

사용 하 여 수행 됩니다 새로운 언어 다운로드는 `Intent`합니다. 이 의도 한 결과인 하면 합니다 [OnActivityResult](https://developer.xamarin.com/api/member/Android.App.Activity.OnActivityResult/) 메서드를 호출할 수 있습니다. 음성-텍스트 예와 달리 (사용 하는 [RecognizerIntent](https://developer.xamarin.com/api/type/Android.Speech.RecognizerIntent/) 으로 `PutExtra` 매개 변수를 `Intent`), 테스트 및 로드 `Intent`가 `Action`-기반:

-   [TextToSpeech.Engine.ActionCheckTtsData](https://developer.xamarin.com/api/field/Android.Speech.Tts.TextToSpeech+Engine.ActionCheckTtsData/) &ndash; 플랫폼에서 작업을 시작 `TextToSpeech` 적절 한 설치 및 장치에서 언어 리소스의 가용성을 확인 하는 엔진입니다.

-   [TextToSpeech.Engine.ActionInstallTtsData](https://developer.xamarin.com/api/field/Android.Speech.Tts.TextToSpeech+Engine.ActionInstallTtsData/) &ndash; 필요한 언어를 다운로드 하 라는 메시지는 작업을 시작 합니다.

다음 코드 예제에는 언어 리소스에 대 한 테스트 및 새로운 언어를 다운로드 하려면 이러한 작업을 사용 하는 방법을 보여 줍니다.

```csharp
var checkTTSIntent = new Intent();
checkTTSIntent.SetAction(TextToSpeech.Engine.ActionCheckTtsData);
StartActivityForResult(checkTTSIntent, NeedLang);
//
protected override void OnActivityResult(int req, Result res, Intent data)
{
    if (req == NeedLang)
    {
        var installTTS = new Intent();
        installTTS.SetAction(TextToSpeech.Engine.ActionInstallTtsData);
        StartActivity(installTTS);
    }
}
```

`TextToSpeech.Engine.ActionCheckTtsData` 언어 리소스의 가용성을 테스트 합니다. `OnActivityResult` 이 테스트가 완료 되 면 호출 됩니다. 언어 리소스를 다운로드 해야 하는 경우 `OnActivityResult` 시작을 `TextToSpeech.Engine.ActionInstallTtsData` 필요한 언어를 다운로드할 수 있도록 하는 활동을 시작 하는 작업입니다. 참고가 `OnActivityResult` 구현을 확인 하지 않습니다는 `Result` 되었기 때문에,이 간단한 예제에서는 이러한 결정에 이미 언어 패키지를 다운로드 해야 하는 코드입니다.

`TextToSpeech.Engine.ActionInstallTtsData` 동작 원인 합니다 **TTS Google 음성 데이터** 다운로드할 언어를 선택 하는 것에 대 한 사용자에 게 표시 되는 활동:

![Google TTS 음성 데이터 작업](speech-images/01-google-tts-voice-data.png)

예를 들어 사용자는 프랑스어를 선택 하 고 프랑스어 음성 데이터를 다운로드 하려면 다운로드 아이콘을 클릭 수 있습니다.

![예제에서는 프랑스어 다운로드](speech-images/02-selecting-french.png)

이 데이터의 설치 다운로드가 완료 된 후 자동으로 발생 합니다.


### <a name="step-5---the-ioninitlistener"></a>5 단계-는 IOnInitListener

텍스트를 음성 인터페이스 메서드를 변환할 수 활동이 `OnInit` 구현 해야 하는 (의 인스턴스화에 대 한 지정 된 두 번째 매개 변수는 `TextToSpeech` 클래스). 이 수신기를 초기화 하 고 결과 테스트 합니다.

수신기를 모두 테스트 해야 `OperationResult.Success` 고 `OperationResult.Failure` 최소한 합니다.
다음 예제를 보여줍니다.

```csharp
void TextToSpeech.IOnInitListener.OnInit(OperationResult status)
{
    // if we get an error, default to the default language
    if (status == OperationResult.Error)
        textToSpeech.SetLanguage(Java.Util.Locale.Default);
    // if the listener is ok, set the lang
    if (status == OperationResult.Success)
        textToSpeech.SetLanguage(lang);
}
```

## <a name="summary"></a>요약

이 가이드에서 텍스트 및 앱 내에서 포함 하는 방법의 가능한 메서드 텍스트 음성 변환, 음성으로 변환의 기본 사항을 살펴봤습니다. 모든 특별 한 경우 적합 하지는 않습니다, 하지만 이제는 음성 해석 되는, 새 언어를 설치 하는 방법 및 앱의 inclusivity 증가 하는 방법을 이해를 해야 합니다.



## <a name="related-links"></a>관련 링크

- [Xamarin.Forms DependencyService](https://developer.xamarin.com/samples/UsingDependencyService/)
- [텍스트를 음성 (샘플)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TextToSpeech)
- [음성을 텍스트로 (샘플)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SpeechToText)
- [Android.Speech 네임 스페이스](https://developer.xamarin.com/api/namespace/Android.Speech/)
- [Android.Speech.Tts 네임 스페이스](https://developer.xamarin.com/api/namespace/Android.Speech.Tts/)
