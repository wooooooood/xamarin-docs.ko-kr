---
title: Android 음성
description: 이 문서에서는 매우 강력한 Android. Speech 네임 스페이스를 사용 하는 기본 사항을 설명 합니다. Android는 개시 된 후 음성을 인식 하 고 텍스트로 출력할 수 있었습니다. 비교적 간단한 프로세스입니다. 그러나 텍스트를 음성으로 변환 하는 경우에는 음성 엔진을 고려해 야 할 뿐만 아니라 TTS (Text To Speech) 시스템에서 사용 가능 하 고 설치 되는 언어를 고려해 야 하는 것 처럼 프로세스가 더 복잡 합니다.
ms.prod: xamarin
ms.assetid: FA3B8EC4-34D2-47E3-ACEA-BD34B28115B9
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/02/2018
ms.openlocfilehash: 693bca77fc22ac68c4a0480315363b241c3cf98b
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511220"
---
# <a name="android-speech"></a>Android 음성

_이 문서에서는 매우 강력한 Android. Speech 네임 스페이스를 사용 하는 기본 사항을 설명 합니다. Android는 개시 된 후 음성을 인식 하 고 텍스트로 출력할 수 있었습니다. 비교적 간단한 프로세스입니다. 그러나 텍스트를 음성으로 변환 하는 경우에는 음성 엔진을 고려해 야 할 뿐만 아니라 TTS (Text To Speech) 시스템에서 사용 가능 하 고 설치 되는 언어를 고려해 야 하는 것 처럼 프로세스가 더 복잡 합니다._

## <a name="speech-overview"></a>음성 개요

사용자 음성을 "이해" 하는 시스템 (음성에서 텍스트 및 Text to Speech)은 장치에 대 한 자연 통신용 요구가 증가 함에 따라 모바일 개발 내에서 증가 하는 영역입니다. 텍스트를 음성으로 변환 하거나 그 반대로 변환 하는 기능이 많은 인스턴스는 android 응용 프로그램에 통합 하는 데 매우 유용한 도구입니다.

예를 들어 휴대 하는 동안 휴대폰 사용에 대 한 클램프를 사용 하는 경우 사용자는 장치를 운영 하는 손을 무료로 사용할 수 있습니다. Android 마모와 같은 다양 한 Android 폼 팩터 및 Android 장치 (예: 태블릿 및 노트 패드)를 사용할 수 있는 기능을 확대 하는 것으로 인 한 다양 한는 뛰어난 TTS 응용 프로그램에 더 큰 초점을 만들었습니다.

Google은 개발자에 게 Android. Speech 네임 스페이스의 다양 한 Api 집합을 제공 하 여 장치를 "음성 인식" (예: 블라인드를 위해 설계 된 소프트웨어)을 만드는 대부분의 인스턴스를 포함 합니다.  네임 스페이스는 텍스트를 음성 `Android.Speech.Tts`으로 변환 하 고, 번역을 수행 하는 데 사용 되는 엔진을 제어 하 고, 음성을 텍스트로 변환할 수 있는 `RecognizerIntent`수를 포함 하는 기능을 포함 합니다.

음성 인식 기능에는 사용 되는 하드웨어에 따라 제한이 있습니다. 사용 가능한 모든 언어에서 장치에 대 한 모든 것을 성공적으로 해석할 가능성이 거의 없습니다.

## <a name="requirements"></a>요구 사항

이 가이드에는 마이크와 스피커가 있는 장치 외에 특별 한 요구 사항이 없습니다.

음성을 해석 하는 Android 장치의 핵심은 해당 `Intent` `OnActivityResult`하는와 함께를 사용 하는 것입니다.
그러나 음성이 인식 되지 않지만 텍스트로 해석 된다는 것을 인식 하는 것이 중요 합니다. 차이점은 중요 합니다.

### <a name="the-difference-between-understanding-and-interpreting"></a>이해 및 해석의 차이점

이해를 간단 하 게 이해 하는 것은 표시 되는 것에 대 한 실제 의미를 톤 및 컨텍스트로 결정할 수 있다는 것입니다. 단순히 단어를 사용 하 여 다른 형식으로 출력 하는 것으로 해석 합니다.

일상적인 대화에서 사용 되는 다음과 같은 간단한 예제를 살펴보겠습니다.

<kbd>안녕하세요, 어떻게 지내세요?</kbd>

변 곡점를 사용 하지 않는 경우 (특정 단어나 단어 부분에 대 한 강조) 간단한 질문입니다. 그러나 회선에 저속을 적용 하는 경우에는 asker이 너무 만족 하지 않으며 asker가 응원 해야 할 수 있습니다. 강조가 "is"에 배치 되는 경우 일반적으로 응답에 더 관심이 있습니다.

변 곡점를 사용 하 고 AI (인공 지능)를 사용 하 여 컨텍스트를 이해 하는 데 매우 강력한 오디오 처리가 필요 하지 않은 경우에도 소프트웨어는 음성을 텍스트로 변환 하는 것이 무엇 인지를 이해할 수 없습니다.

## <a name="setting-up"></a>설정

음성 시스템을 사용 하기 전에 항상 장치에 마이크가 있는지 확인 하는 것이 좋습니다. 마이크가 설치 되지 않은 Kindle 또는 Google note pad에서 앱을 실행 하는 데는 작은 점이 있습니다.

아래 코드 샘플은 마이크를 사용할 수 있는지 여부를 쿼리 하는 방법과 그렇지 않은 경우 경고를 생성 하는 방법을 보여 줍니다. 이 시점에서 마이크를 사용할 수 없는 경우 작업을 종료 하거나 음성을 기록 하는 기능을 사용 하지 않도록 설정 합니다.

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

음성 시스템의 의도는 이라는 `RecognizerIntent`특정 유형의 의도를 사용 합니다. 이러한 의도는 기록을 사용할 때까지 대기 하는 시간을 포함 하는 많은 매개 변수를 제어 합니다 .이 경우에는 기록을 사용할 때까지 대기 하는 시간을 포함 하 고 `Intent`, 인식할 수 있는 추가 언어를 포함 하며,의 모달 대화 상자에 포함 시킬 텍스트를 명령 수단 이 코드 조각 `VOICE` 에서 `readonly int` 은에서 `OnActivityResult`인식 하는 데 사용 됩니다.

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

음성에서 해석 되는 텍스트는 활동 `Intent`을 완료 하 고를 통해 `GetStringArrayListExtra(RecognizerIntent.ExtraResults)`액세스 하는 경우에 반환 되는 내에 전달 됩니다. 호출자 의도에서 요청 `IList<string>`된 언어 수 (및 `RecognizerIntent.ExtraMaxResults`에 지정 됨)에 따라 인덱스를 사용 하 고 표시할 수 있는를 반환 합니다. 그러나 모든 목록과 마찬가지로 표시할 데이터가 있는지 확인 하는 것이 좋습니다.

`StartActivityForResult` 의`OnActivityResult` 반환 값을 수신 대기 하는 경우 메서드를 제공 해야 합니다.

아래 `textBox` 예제에서는 결정 된 내용을 `TextBox` 출력 하는 데 사용 됩니다. 텍스트를 인터프리터의 특정 형식으로 전달 하는 데에도 사용 될 수 있습니다. 응용 프로그램은 텍스트와 분기를 응용 프로그램의 다른 부분과 비교할 수 있습니다.

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

## <a name="text-to-speech"></a>Text to Speech

텍스트를 음성으로 변환 하는 것은 텍스트가 음성으로 전환 되는 것이 아니라 두 가지 주요 구성 요소에 의존 합니다. 장치에 설치 되는 텍스트 음성 변환 엔진과 설치 되는 언어입니다.

대부분의 Android 장치는 기본 Google TTS 서비스를 설치 하 고 하나 이상의 언어를 제공 합니다. 이는 장치를 처음 설정할 때 설정 되 고 장치의 위치에 따라 결정 됩니다. 예를 들어 독일에서 설정 된 전화는 독일 언어를 설치 하는 반면 아메리카는 미국 영어를 사용 합니다.

### <a name="step-1---instantiating-texttospeech"></a>1 단계-TextToSpeech 인스턴스화

`TextToSpeech`최대 3 개의 매개 변수를 사용할 수 있습니다. 첫 번째 매개 변수는 선택적 (`AppContext`, `IOnInitListener`, `engine`)이 필요 합니다. 수신기는 서비스에 바인딩하는 데 사용 되며, 엔진에서 사용 가능한 Android 텍스트를 음성 엔진에 연결 하는 데 사용할 수 있는 엔진의 실패를 테스트 합니다. 최소한이 장치에는 Google의 고유한 엔진이 있습니다.

### <a name="step-2---finding-the-languages-available"></a>2 단계-사용 가능한 언어 찾기

클래스 `Java.Util.Locale` 에는 라는 `GetAvailableLocales()`유용한 메서드가 포함 되어 있습니다. 그러면 음성 엔진에서 지원 되는이 언어 목록을 설치 된 언어에 대해 테스트할 수 있습니다.

"이해 된" 언어 목록을 생성 하는 것이 중요 합니다. 항상 기본 언어 (사용자가 장치를 먼저 설정 하는 경우 사용자가 설정 하는 언어)가 있습니다 `List<string>` `textToSpeech.IsLanguageAvailable(locale)`. 따라서이 예에서는 첫 번째 매개 변수로 "default"가 설정 되어 있으므로 목록의 나머지 부분은의 결과에 따라 채워집니다.

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

이 코드는 [Texttospeech. IsLanguageAvailable](xref:Android.Speech.Tts.TextToSpeech.IsLanguageAvailable*) 를 호출 하 여 지정 된 로캘의 언어 패키지가 장치에 이미 있는지 여부를 테스트 합니다.
이 메서드는 전달 된 로캘의 언어를 사용할 수 있는지 여부를 나타내는 [LanguageAvailableResult](xref:Android.Speech.Tts.LanguageAvailableResult)를 반환 합니다. 이 `LanguageAvailableResult` 언어가 인 `NotSupported`것으로 표시 되 면 해당 언어에 대해 사용할 수 있는 음성 패키지 (다운로드도 포함)가 없습니다. 가 `LanguageAvailableResult` 로`MissingData`설정 된 경우 4 단계에서 설명한 대로 새 언어 패키지를 다운로드할 수 있습니다.

### <a name="step-3---setting-the-speed-and-pitch"></a>3 단계-속도 및 피치 설정

Android를 통해 사용자는 `SpeechRate` 및 `Pitch` (음성의 속도 및 음질 속도)를 변경 하 여 음성 소리를 변경할 수 있습니다. 이는 두 가지 모두에 대해 "normal" 음성이 1 인 0에서 1로 이동 합니다.

### <a name="step-4---testing-and-loading-new-languages"></a>4 단계-새 언어 테스트 및 로드

새 언어 다운로드는를 `Intent`사용 하 여 수행 됩니다. 이로 인해 [Onactivityresult](xref:Android.App.Activity.OnActivityResult*) 메서드가 호출 됩니다. [RecognizerIntent](xref:Android.Speech.RecognizerIntent) 를 `PutExtra` 에 대 `Intent`한 매개 변수로 사용 하는 음성 텍스트 예제와는 달리의 테스트 및 로드 `Intent` `Action`는 다음과 같습니다.

- [Texttospeech. actioncheckttsdata](xref:Android.Speech.Tts.TextToSpeech.Engine.ActionCheckTtsData) &ndash; 는 플랫폼 `TextToSpeech` 엔진에서 작업을 시작 하 여 장치에서 언어 리소스의 적절 한 설치 및 사용 가능 여부를 확인 합니다.

- [Texttospeech. actioninstallttsdata](xref:Android.Speech.Tts.TextToSpeech.Engine.ActionInstallTtsData) &ndash; 는 필요한 언어를 다운로드 하도록 사용자에 게 요청 하는 활동을 시작 합니다.

다음 코드 예제에서는 이러한 작업을 사용 하 여 언어 리소스를 테스트 하 고 새 언어를 다운로드 하는 방법을 보여 줍니다.

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

`TextToSpeech.Engine.ActionCheckTtsData`언어 리소스의 가용성을 테스트 합니다. `OnActivityResult`이 테스트가 완료 되 면이 호출 됩니다. 언어 리소스를 다운로드 해야 하는 경우 `OnActivityResult` 에서 `TextToSpeech.Engine.ActionInstallTtsData` 사용자가 필요한 언어를 다운로드할 수 있는 작업을 시작 하는 작업을 시작 합니다. 이 구현에서는 `Result` 코드를 확인 하지 않습니다 .이 간단한 예제에서는 언어 패키지를 다운로드 해야 하는 결정이 이미 수행 되었기 때문입니다. `OnActivityResult`

작업을 수행 하면 다운로드할 언어를 선택 하기 위해 **Google TTS 음성 데이터** 활동이 사용자에 게 표시 됩니다. `TextToSpeech.Engine.ActionInstallTtsData`

![Google TTS 음성 데이터 작업](speech-images/01-google-tts-voice-data.png)

예를 들어 사용자는 프랑스어를 선택 하 고 다운로드 아이콘을 클릭 하 여 프랑스어 음성 데이터를 다운로드할 수 있습니다.

![프랑스어 다운로드 예](speech-images/02-selecting-french.png)

다운로드를 완료 한 후이 데이터의 설치가 자동으로 수행 됩니다.


### <a name="step-5---the-ioninitlistener"></a>5 단계-이 기능 수신기

활동에서 텍스트를 음성으로 변환 하려면 인터페이스 메서드 `OnInit` 를 구현 해야 합니다 .이는 `TextToSpeech` 클래스의 인스턴스화에 대해 지정 된 두 번째 매개 변수입니다. 수신기를 초기화 하 고 결과를 테스트 합니다.

수신기는 최소한과 `OperationResult.Success` `OperationResult.Failure` 를 모두 테스트 해야 합니다.
다음 예제에서는만 보여 줍니다.

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

이 가이드에서는 텍스트를 음성으로 변환 하는 기본 사항과 음성으로 텍스트를 변환 하는 방법에 대 한 기본 사항을 살펴보고 자신의 앱 내에 이러한 항목을 포함 하는 방법에 대해 알아봅니다. 모든 특정 사례를 포함 하지는 않지만 이제 음성이 해석 되는 방법, 새 언어를 설치 하는 방법 및 앱 inclusivity를 늘리는 방법에 대 한 기본적인 내용을 이해 해야 합니다.



## <a name="related-links"></a>관련 링크

- [Xamarin.Forms DependencyService](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Text to Speech (샘플)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TextToSpeech)
- [음성 텍스트 (샘플)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SpeechToText)
- [Android. Speech 네임 스페이스](xref:Android.Speech)
- [Android. Tts 네임 스페이스](xref:Android.Speech.Tts)
