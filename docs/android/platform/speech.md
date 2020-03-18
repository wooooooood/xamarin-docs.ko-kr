---
title: Android 음성
description: 이 문서에서는 매우 강력한 Android.Speech 네임스페이스를 사용하는 기본 사항을 설명합니다. Android는 처음부터 음성을 인식하여 텍스트로 출력할 수 있었습니다. 이는 비교적 간단한 프로세스입니다. 그러나 텍스트를 음성으로 변환하는 경우에는 음성 엔진을 고려해야 할 뿐만 아니라 TTS(텍스트 음성 변환) 시스템에서 사용 가능하고 설치되는 언어도 고려해야 하므로 프로세스가 복잡해집니다.
ms.prod: xamarin
ms.assetid: FA3B8EC4-34D2-47E3-ACEA-BD34B28115B9
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/02/2018
ms.openlocfilehash: e8c7d1a4fb3537644ed3b7737158a5e50abcdae5
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73019763"
---
# <a name="android-speech"></a>Android 음성

_이 문서에서는 매우 강력한 Android.Speech 네임스페이스를 사용하는 기본 사항을 설명합니다. Android는 처음부터 음성을 인식하여 텍스트로 출력할 수 있었습니다. 이는 비교적 간단한 프로세스입니다. 그러나 텍스트를 음성으로 변환하는 경우에는 음성 엔진을 고려해야 할 뿐만 아니라 TTS(텍스트 음성 변환) 시스템에서 사용 가능하고 설치되는 언어도 고려해야 하므로 프로세스가 복잡해집니다._

## <a name="speech-overview"></a>음성 개요

디바이스와의 자연어 통신에 대한 요구가 증가함에 따라 인간의 언어를 “이해”하고 입력된 내용을 발음하는 음성 텍스트 변환 및 텍스트 음성 변환 시스템 개발은 모바일 개발에서 지속적으로 확대되는 영역입니다. 텍스트를 음성으로 변환하거나 그 반대로 변환하는 기능은 Android 애플리케이션에 통합하면 매우 유용한 도구인 경우가 많습니다.

예를 들어 운전 중 휴대폰 사용이 금지되어 있으므로 사용자는 디바이스를 핸즈프리로 사용하기를 원합니다. Android Wear를 비롯해 Android 디바이스(예: 태블릿 및 노트 패드)를 사용할 수 있는 매우 다양한 Android 폼 팩터가 존재하고 계속 확대되고 있으므로 뛰어난 TTS 애플리케이션에 대한 관심은 커지고 있습니다.

Google은 Android.Speech 네임스페이스에서 개발자가 "음성 인식" 디바이스(예: 시각장애인용 소프트웨어)를 디자인하는 대부분의 경우를 커버하는 풍부한 API 세트를 제공합니다.  이 네임스페이스에는 `Android.Speech.Tts`를 통해 텍스트를 음성으로 변환하는 기능, 번역을 수행하는 데 사용되는 엔진에 대한 컨트롤, 음성을 텍스트로 변환할 수 있는 다수의 `RecognizerIntent`가 포함되어 있습니다.

음성을 이해하는 기능은 존재하지만 사용되는 하드웨어에 따라 제한이 있습니다. 디바이스가 사용 가능한 모든 언어로 말한 모든 내용을 성공적으로 해석할 가능성은 없습니다.

## <a name="requirements"></a>요구 사항

이 가이드에는 마이크와 스피커가 있는 디바이스 이외에 특별한 요구 사항이 없습니다.

음성을 해석하는 Android 디바이스의 핵심은 해당 `OnActivityResult`에서 `Intent`를 사용하는 것입니다.
그러나 음성이 이해되지 않지만 텍스트로 해석된다는 것을 인식하는 것이 중요합니다. 그 차이는 중요합니다.

### <a name="the-difference-between-understanding-and-interpreting"></a>이해와 해석의 차이

이해에 대한 간단한 정의는 어조와 맥락을 통해 말하는 내용의 실제 의미를 판단할 수 있는 것입니다. 해석이란 단순히 한 형식의 단어를 다른 형식으로 출력하는 것을 의미합니다.

다음과 같이 일상 대화에서 사용되는 간단한 예를 살펴보겠습니다.

<kbd>Hello, how are you?</kbd>

어조(특정 단어 또는 단어 부분에 대한 강세)가 없으면 이는 단순한 질문입니다. 그러나 이 질문을 느리게 말하면 듣는 사람은 질문하는 사람이 기분이 좋지 않거나 몸이 좋지 않다고 생각할 것입니다. 강세가 "are"에 있는 경우 질문하는 사람은 일반적으로 응답에 더 관심이 있습니다.

어조를 활용하는 상당히 강력한 오디오 처리 그리고 맥락을 이해하기 위한 일정 수준의 AI(인공 지능) 없이는 소프트웨어는 말하는 내용을 전혀 이해할 수 없습니다. 간단한 휴대폰에서 가능한 최선은 음성을 텍스트로 변환하는 것입니다.

## <a name="setting-up"></a>설정

음성 시스템을 사용하기 전에 항상 디바이스에 마이크가 있는지 확인해야 합니다. 마이크가 설치되지 않은 Kindle 또는 Google 노트 패드에서 앱을 실행하는 것은 의미가 없을 테니까요.

아래 코드 샘플은 마이크를 사용할 수 있는지 여부를 쿼리하는 방법과 그렇지 않은 경우 경고를 생성하는 방법을 보여 줍니다. 이 시점에서 마이크를 사용할 수 없는 경우 작업을 종료하거나 음성 기록 기능을 사용하지 않도록 설정합니다.

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

음성 시스템을 위한 의도는 `RecognizerIntent`라고 하는 특정 유형의 의도를 사용합니다. 이러한 의도는 기록이 완료된 것으로 간주하기 위해 대기할 침묵 기간, 인식 및 출력할 추가 언어, `Intent`의 모달 대화 상자에 지시 형태로 포함시킬 모든 텍스트 등 많은 수의 매개 변수를 제어합니다. 이 코드 조각에서 `VOICE`는 `OnActivityResult`에서 인식에 사용되는 `readonly int`입니다.

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

### <a name="conversion-of-the-speech"></a>음성 변환

음성에서 해석된 텍스트는 `Intent`를 통해 전달되며, 이는 작업이 완료되고 `GetStringArrayListExtra(RecognizerIntent.ExtraResults)`를 통해 액세스될 때 반환됩니다. 그러면 `IList<string>`이 반환되어 호출자 의도에서 요청된(그리고 `RecognizerIntent.ExtraMaxResults`에서 지정된) 언어 수에 따라 해당 인덱스를 사용하고 표시할 수 있습니다. 그러나 모든 목록과 마찬가지로 표시할 데이터가 있는지 확인하는 것이 좋습니다.

`StartActivityForResult`의 반환 값을 수신 대기하는 경우 `OnActivityResult` 메서드를 제공해야 합니다.

아래 예제에서 `textBox`는 딕테이션한 내용을 출력하는 데 사용되는 `TextBox`입니다. 텍스트를 인터프리터의 특정 형식으로 전달하는 데에도 사용될 수 있습니다. 애플리케이션은 텍스트 및 분기를 애플리케이션의 다른 부분과 비교할 수 있습니다.

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

## <a name="text-to-speech"></a>텍스트에서 음성 변환

텍스트 음성 변환은 단지 음성 텍스트 변환의 반대가 아니며, 디바이스에 설치된 텍스트 음성 변환 엔진과 설치된 언어라는 두 가지 주요 구성 요소에 의존합니다.

대부분의 Android 디바이스는 기본 Google TTS 서비스가 설치되어 있고 하나 이상의 언어를 제공합니다. 이는 디바이스를 처음 설정할 때 그리고 당시 디바이스의 위치에 따라 결정됩니다. 예를 들어 독일에서 설정된 휴대폰은 독일어가 설치되고 미국에서 설정된 휴대폰은 미국식 영어가 설치됩니다.

### <a name="step-1---instantiating-texttospeech"></a>1단계 - TextToSpeech 인스턴스화

`TextToSpeech`는 최대 세 개의 매개 변수를 사용할 수 있습니다. 처음 두 개는 필수이고, 세 번째는 선택입니다(`AppContext`, `IOnInitListener`, `engine`). 수신기는 서비스에 바인딩하고 엔진(임의의 사용 가능한 Android 텍스트 음성 변환 엔진)의 실패를 테스트하는 데 사용됩니다. 최소한 이 디바이스에는 Google의 고유한 엔진이 있습니다.

### <a name="step-2---finding-the-languages-available"></a>2단계 - 사용 가능한 언어 찾기

`Java.Util.Locale` 클래스에는 `GetAvailableLocales()`라는 유용한 메서드가 포함되어 있습니다. 그러면 음성 엔진이 지원하는 이 언어 목록을 설치된 언어에 대해 테스트할 수 있습니다.

"이해된" 언어 목록을 생성하는 것이 중요하지 않습니다. 항상 기본 언어(사용자가 디바이스를 처음 설정할 때 설정하는 언어)가 있습니다. 따라서 이 예제에서는 `List<string>`가 첫 번째 매개 변수로 "Default"를 취하고, `textToSpeech.IsLanguageAvailable(locale)`의 결과에 따라 목록의 나머지가 채워집니다.

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

이 코드는 [TextToSpeech.IsLanguageAvailable](xref:Android.Speech.Tts.TextToSpeech.IsLanguageAvailable*)을 호출하여 지정된 로캘의 언어 패키지가 디바이스에 이미 있는지 테스트합니다.
이 메서드는 전달된 로캘의 언어를 사용할 수 있는지 여부를 나타내는 [LanguageAvailableResult](xref:Android.Speech.Tts.LanguageAvailableResult)를 반환합니다. `LanguageAvailableResult` 언어가 `NotSupported`임을 나타내는 경우 해당 언어에 사용할 수 있는 음성 패키지가 없는 것입니다(다운로드조차 없음). `LanguageAvailableResult`를 `MissingData`로 설정하면 4단계에서 설명한 대로 새 언어 패키지를 다운로드할 수 있습니다.

### <a name="step-3---setting-the-speed-and-pitch"></a>3단계 - 속도 및 피치 설정

Android에서는 사용자가 `SpeechRate` 및 `Pitch`(음성의 속도 및 어조)를 바꿔 음성의 소리를 변경할 수 있습니다. 둘 모두 범위는 0~1이고, 1은 "정상" 음성입니다.

### <a name="step-4---testing-and-loading-new-languages"></a>4단계 - 새 언어 테스트 및 로드

새 언어 다운로드는 `Intent`를 사용하여 수행됩니다. 이 의도의 결과로 [OnActivityResult](xref:Android.App.Activity.OnActivityResult*) 메서드가 호출됩니다. [RecognizerIntent](xref:Android.Speech.RecognizerIntent)를 `Intent`에 대한 `PutExtra` 매개 변수로 사용하는 음성 텍스트 변환 예제와 달리, `Intent`에 대한 테스트 및 로드는 `Action`을 기반으로 합니다.

- [TextToSpeech. ActionCheckTtsData](xref:Android.Speech.Tts.TextToSpeech.Engine.ActionCheckTtsData) &ndash; 플랫폼 `TextToSpeech` 엔진에서 작업을 시작하여 디바이스에서 언어 리소스의 적절한 설치 및 가능 여부를 확인합니다.

- [TextToSpeech.Engine.ActionInstallTtsData](xref:Android.Speech.Tts.TextToSpeech.Engine.ActionInstallTtsData) &ndash; 필요한 언어를 다운로드하라는 메시지를 표시하는 작업을 시작합니다.

다음 코드 예제에서는 이러한 작업을 사용하여 언어 리소스를 테스트하고 새 언어를 다운로드하는 방법을 보여 줍니다.

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

`TextToSpeech.Engine.ActionCheckTtsData`는 언어 리소스의 가용성을 테스트합니다. 이 테스트가 완료되면 `OnActivityResult`가 호출됩니다. 언어 리소스를 다운로드해야 하는 경우 `OnActivityResult`가 `TextToSpeech.Engine.ActionInstallTtsData` 작업을 발생시켜 사용자가 필요한 언어를 다운로드하도록 허용하는 작업을 시작합니다. 이 `OnActivityResult` 구현에서는 `Result` 코드를 확인하지 않습니다. 이 간단한 예제에서는 언어 패키지를 다운로드해야 한다는 결정이 이미 성립되었기 때문입니다.

`TextToSpeech.Engine.ActionInstallTtsData` 작업은 사용자가 다운로드할 언어를 선택할 수 있도록 **Google TTS 음성 데이터** 작업을 사용자에게 제공합니다.

![Google TTS 음성 데이터 작업](speech-images/01-google-tts-voice-data.png)

예를 들어 사용자는 프랑스어를 선택하고 다운로드 아이콘을 클릭하여 프랑스어 음성 데이터를 다운로드할 수 있습니다.

![프랑스어 다운로드 예](speech-images/02-selecting-french.png)

다운로드 완료 후 이 데이터가 자동으로 설치됩니다.

### <a name="step-5---the-ioninitlistener"></a>5단계 - IOnInitListener

작업이 텍스트를 음성으로 변환하려면 인터페이스 메서드 `OnInit`를 구현해야 합니다. 이는 `TextToSpeech` 클래스의 인스턴스화에서 지정된 두 번째 매개 변수입니다. 이 메서드는 수신기를 초기화하고 결과를 테스트합니다.

수신기는 최소한 `OperationResult.Success` 및 `OperationResult.Failure`에 대해 테스트해야 합니다.
다음 예제에서 이를 보여 줍니다.

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

이 가이드에서는 텍스트 음성 변환 및 음성 텍스트 변환의 기본 사항과 앱에 이러한 기능을 포함할 수 있는 방법에 대해 알아보았습니다. 모든 사례가 포함된 것은 아니지만 이제 음성이 해석되는 방식, 새 언어를 설치하는 방법, 앱 포용성을 확대하는 방법을 기본적으로 이해했을 것입니다.

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms DependencyService](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice//)
- [텍스트 음성 변환(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-texttospeech)
- [음성 텍스트 변환(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-speechtotext)
- [Android.Speech 네임스페이스](xref:Android.Speech)
- [Android.Speech.Tts 네임스페이스](xref:Android.Speech.Tts)
