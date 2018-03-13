---
title: Android Speech
description: "이 문서에서는 매우 강력한 Android.Speech 네임 스페이스를 사용 하는 기본적인에 설명 합니다. 을 도입한 이후 Android 음성 인식 하 고 텍스트로 출력 수 있었습니다. 이 작업은 비교적 간단 합니다. 그러나 텍스트 음성 변환, 프로세스는 조금 더 복잡 뿐만 아니라 음성 엔진은 않으므로 계정으로 간주 되려면 하지만 사용 가능 하 고 개의 문자 음성 변환 TTS () 시스템에서 설치 된 언어도 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: FA3B8EC4-34D2-47E3-ACEA-BD34B28115B9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 7c38ebb6b482f4097a4977accecc4a230d3f3ed3
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="android-speech"></a>Android Speech

_이 문서에서는 매우 강력한 Android.Speech 네임 스페이스를 사용 하는 기본적인에 설명 합니다. 을 도입한 이후 Android 음성 인식 하 고 텍스트로 출력 수 있었습니다. 이 작업은 비교적 간단 합니다. 그러나 텍스트 음성 변환, 프로세스는 조금 더 복잡 뿐만 아니라 음성 엔진은 않으므로 계정으로 간주 되려면 하지만 사용 가능 하 고 개의 문자 음성 변환 TTS () 시스템에서 설치 된 언어도 합니다._

## <a name="speech-overview"></a>음성 개요

시스템을 실제 음성 "이해" 및 입력 내용을 enunciates 필요-텍스트 및 개의 문자 음성 변환 하는 음성-우리의 장치와 일반적인 통신에 대 한 요구 증가 등에 따라 대로 내 모바일 개발 끊임없이 성장 하 영역입니다. 인스턴스가 많을 여기서 텍스트 음성 변환를 변환 하거나 그 반대로 android 응용 프로그램에 통합 하는 매우 유용한 도구 기능을 사용 함으로써 합니다.

예를 들어 구동 하는 동안 사용 하 여 휴대폰에서 아래로 제한, 사용자가 자신의 장치를 운영 바늘 가능한 방식으로 원하는 합니다. 다양 한 Android 폼 팩터 너무 많은-Android 착용 같은-적이 확대를 포함 하는 것 (예: 태블릿 및 참고 패드) Android 장치를 사용할 수를 만들었는지 더 큰 포커스 뛰어난 TTS 응용 프로그램에 있습니다.

Google 개발자 Android.Speech 네임 스페이스에 풍부한 Api 사용 장치 "음성 인식" (예: 시각 장애인 들에 대 한 설계 된 소프트웨어)을 만드는 대부분의 경우에 제공 합니다.  네임 스페이스를 통해 음성으로 변환 하는 텍스트를 허용 하는 기능을 포함 `Android.Speech.Tts`, 변환, 뿐만 아니라 다양 한 수행 하는 데 사용 하는 엔진에 대 한 제어 `RecognizerIntent`의음성 텍스트로 변환할 수를 허용 합니다.

시설 이해할 수 있도록 음성에 대 한 사항이 동안 사용 되는 하드웨어에 따라 제한이 있습니다. 그럴 가능성은 장치를 사용할 수 있는 모든 언어에서 통용 모든 해석 성공적으로 합니다.

## <a name="requirements"></a>요구 사항

마이크 및 스피커 장치 이외의이 가이드에 대 한 특별 한 요구 사항이 없습니다 있습니다.

음성을 해석 하는 Android 장치의 핵심은 사용 하는 `Intent` 해당 `OnActivityResult`합니다.
반드시, 하지만 된 음성 인식 하지 못하면 인식할 수-하지만 텍스트를 해석 합니다. 차이가 중요 합니다.

### <a name="the-difference-between-understanding-and-interpreting"></a>이해 하 고 해석의 차이점

이해의 간단한 정의 씨 어떤 의미 톤 및 컨텍스트를 확인할 수 있다는 것입니다. 방금를 해석 하는 단어를 사용 하 고 다른 폼에 출력 하 여는 것입니다.

일상적인 대화에 사용 되는 다음의 간단한 예제를 고려 합니다. 

<kbd>안녕하세요, 어떻게 지내세요?</kbd>

굴곡 (주안점 특정 단어나 단어의 일부)가 없으면 간단한 질문 합니다. 그러나 느린 속도로 줄에 적용 하는 수신 대기 하는 사용자는 자가 잘 아닌지 또는 자가 너무 만족도 매우 아니며 아마도 cheering 필요 검색 됩니다. 강조 되는 경우에 "are", 요청 하는 사람이 일반적으로 응답에 더 관심이 있습니다.

확인 하기 위해 처리 하는 비교적 강력한 오디오 없이 하는지 이해 하는 변 곡점 않았고 인공 인텔리전스 (AI)의 사용도 사용 하는 소프트웨어를 말한 이해 시작할 수 없습니다-간단한 전화 수행할 수 있는 가장 된 음성 텍스트로 변환 됩니다.

## <a name="setting-up"></a>설정

음성 시스템을 사용 하기 전에 항상 이므로 장치 마이크에 있는지 확인 하는 것이 좋습니다. 설치 마이크 없이 Kindle 또는 Google 메모장에서 앱을 실행 하려고 하는 이유가 거의 것입니다.

다음 코드 예제는 마이크 표시 되 고 그렇지 않은 경우 쿼리 하는 방법을 보여 줍니다 경고를 만들려면 합니다. 사용할 수 없는 마이크 경우이 시점에서 활동을 종료 하거나 된 음성 기록 하는 기능을 사용 하지 않도록 설정 합니다.

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

특정 유형의 의도 호출을 사용 하는 음성 시스템에 대 한 의도 `RecognizerIntent`합니다. 이 의도 제어 기록을 통해 인식 하 고 출력으로 모든 추가 언어 것으로 간주 됩니다 때까지 대기 순위로 대기 하 시간 포함 하 여 매개 변수 및에 포함할 텍스트를 다 수의 `Intent`의 명령의 수단으로 모달 대화 상자. 이 코드 조각 `VOICE` 는 `readonly int` 의 인식에 사용 되는 `OnActivityResult`합니다.

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

### <a name="conversion-of-the-speech"></a>변환 된 음성의

해석 된 음성의 텍스트 내에서 배달 됩니다는 `Intent`는 완료 된 활동과 통해 액세스 되는 경우 반환 되는 `GetStringArrayListExtra(RecognizerIntent.ExtraResults)`합니다. 이 반환 됩니다는 `IList<string>`인덱스 하 수 있는 사용 되는 호출자가 의도에서 요청 된 언어의 수에 따라 표시 되는 (에 지정 된 및의 `RecognizerIntent.ExtraMaxResults`). 와 마찬가지로 모든 목록 하지만 데이터를 표시할 수 있도록 하기 위해 확인해 합니다.

반환 값에 대 한 수신 대기 하는 경우는 `StartActivityForResult`, `OnActivityResult` 메서드에 제공 해야 합니다.

다음 예제에서 `textBox` 는 `TextBox` 지시 내용을 출력 하는 중에 사용 합니다. 텍스트를 특정 형식 인터프리터의 고 여기에서 전달 하는 데 사용 될 동일 하 게 수, 응용 프로그램은 텍스트와 응용 프로그램의 다른 부분으로 분기 비교할 수 있습니다.

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
                  switch(matches[0].Substring(0,5).ToLower())
                  {
                     case "north":
                      MovePlayer(0);
                     break;
                   case "south":
                     MovePlayer(1);
                     break;
             }
             else
                  textBox.Text = "No speech was recognised";
        }
   }
    base.OnActivityResult(requestCode, resultVal, data);
}
```

## <a name="text-to-speech"></a>개의 문자 음성 변환

개의 문자 음성 변환은 하지 텍스트로 음성 역방향 매우 하며 두 가지 주요 구성 요소입니다. 텍스트 음성 변환 엔진 장치에 설치 되 고 및 언어를 설치 합니다.

Android 장치는 기본 함께 제공 되는 대부분, Google TTS 서비스를 설치 하 고 하나 이상의 언어입니다. 장치가 처음 설정 하 고 시간에 장치가 된 위치에 따라 달라 집니다이 설정 됩니다 (예를 들어 독일에 설정 하는 전화는 설치 독일어를 하지만 미국 내에서 한 영어 (미국)).

### <a name="step-1---instantiating-texttospeech"></a>1 단계-인스턴스화하는 동안 TextToSpeech

`TextToSpeech` 최대 3 개의 매개 변수를 걸릴 수 있으며, 세 번째 작업에서 사용 됩니다 처음 두 선택 사항 (`AppContext`, `IOnInitListener`, `engine`). 수신기는 바인딩하는 데 사용 서비스 및 실패에 대 한 테스트 되 여기에 최소한의 사용 가능한 Android 개의 문자 음성 변환 엔진을 개수에 관계 없이 엔진, 장치는 Google의 엔진입니다.

### <a name="step-2---finding-the-languages-available"></a>2 단계-사용할 수 있는 언어 찾기

`Java.Util.Locale` 라는 유용한 메서드를 포함 하는 네임 스페이스 `GetAvailableLocales()`합니다. 이 목록 음성 엔진에서 지 원하는 언어의 설치 된 언어에 대 한 다음 테스트할 수 있습니다.

"이해" 언어의 목록을 생성 하는 그다지 중요 하지 않은 경우 기본 언어 (처음 설정할 때 장치 사용자가 설정할 언어)은 항상에이 예제는 `List<string>` "Default" 첫 번째 매개 변수로, 결과에 따라 목록의 나머지를 채우는 `textToSpeech.IsLanguageAvailable(locale)`합니다.

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

### <a name="step-3---setting-the-speed-and-pitch"></a>3 단계-피치와 속도 설정 합니다.

Android 된 음성 소리가 변경 하 여 변경할 수 있습니다는 `SpeechRate` 및 `Pitch` (속도 된 음성의 모양을의 빈도). 이 0에서 1로 둘 다에 대해 1 되 고 "일반" 음성 인 합니다.

### <a name="step-4---testing-and-loading-new-languages"></a>4 단계-테스트 하 고 새 언어를 로드 합니다.

사용 하 여이 작업을 수행는 `Intent` 에서 해석 되 고 결과와 `OnActivityResult`합니다. 사용할 음성-텍스트 예제와 달리는 `RecognizerIntent` 로 `PutExtra` 매개 변수를는 `Intent`, 의도 사용 하 여 설치 프로그램 `Action`합니다.

다음 코드를 사용 하 여 Google의 새 언어를 설치 하는 것이 불가능 합니다. 결과 `Activity` 되려면 다운로드에 대 한 메시지가 표시 되 면 언어를 설치 하는 언어 필요 하 고 그럴 경우를 확인 합니다.

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

### <a name="step-5---the-ioninitlistener"></a>5-는 IOnInitListener 단계

텍스트를 음성 인터페이스 메서드를 변환할 수 있어야 하는 활동에 대 한 `OnInit` 을 만들어야 (의 인스턴스화에 대 한 지정 된 두 번째 매개 변수 인지는 `TextToSpeech` 클래스). 이 수신기를 초기화 하 고 결과 테스트 합니다.

수신기를 모두 테스트 해야 `OperationResult.Success` 및 `OperationResult.Failure` 최소한 합니다.
다음 예제에서는 보여 줍니다.

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

이 가이드에서 살펴본 개의 문자 음성 변환 및 음성을 텍스트와 앱 내에서를 포함 하는 방법의 가능한 방법으로 변환의 기본 사항입니다. 모든 특별 한 경우 적합 하지는 않습니다, 동안 이제 있어야 음성 해석 되는 방식을, 새 언어를 설치 하는 방법 및 응용 프로그램의 inclusivity 증가 하는 방법에 대 한 기본적인 이해 합니다.



## <a name="related-links"></a>관련 링크

- [Xamarin.Forms DependencyService](https://developer.xamarin.com/samples/UsingDependencyService/)
- [개의 문자 음성 변환 (샘플)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TextToSpeech)
- [음성 텍스트로 (샘플)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SpeechToText)
- [Android.Speech 네임 스페이스](https://developer.xamarin.com/api/namespace/Android.Speech/)
- [Android.Speech.Tts namespace](https://developer.xamarin.com/api/namespace/Android.Speech.Tts/)
