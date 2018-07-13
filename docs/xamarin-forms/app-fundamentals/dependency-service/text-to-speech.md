---
title: 텍스트 음성 변환 구현
description: 이 문서에서는 Xamarin.Forms DependencyService 클래스를 사용 하 여 각 플랫폼의 기본 텍스트 음성 변환 API를 호출 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2017
ms.openlocfilehash: d39902f2269d3eb241b48831b8eb1b181ff11e7a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997545"
---
# <a name="implementing-text-to-speech"></a>텍스트 음성 변환 구현

이 문서에서는 안내를 사용 하는 플랫폼 간 앱을 만들면 [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) 네이티브 텍스트 음성 변환 Api에 액세스 하려면:

- **[인터페이스를 만드는](#Creating_the_Interface)**  &ndash; 공유 코드에서 인터페이스를 만드는 방법을 이해 합니다.
- **[iOS 구현](#iOS_Implementation)**  &ndash; iOS 용 네이티브 코드에서 인터페이스를 구현 하는 방법을 알아봅니다.
- **[Android 구현을](#Android_Implementation)**  &ndash; Android 용 네이티브 코드에서 인터페이스를 구현 하는 방법을 알아봅니다.
- **[UWP 구현을](#WindowsImplementation)**  &ndash; 유니버설 Windows 플랫폼 (UWP)에 대 한 네이티브 코드에서 인터페이스를 구현 하는 방법을 알아봅니다.
- **[공유 코드에서 구현](#Implementing_in_Shared_Code)**  &ndash; 사용 하는 방법을 알아봅니다 `DependencyService` 공유 코드에서 기본 구현을 호출 합니다.

사용 하 여 응용 프로그램 `DependencyService` 같은 구조를 가집니다.

![](text-to-speech-images/tts-diagram.png "DependencyService 응용 프로그램 구조")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>인터페이스 만들기

먼저 구현 하려는 기능을 표현 하는 공유 코드에서 인터페이스를 만듭니다. 예를 들어 인터페이스는 단일 메서드를 포함 `Speak`:

```csharp
public interface ITextToSpeech
{
    void Speak (string text);
}
```

공유 코드에서이 인터페이스에 대 한 코딩 하면 Xamarin.Forms 앱 각 플랫폼에서 speech Api에 액세스할 수 있습니다.

> [!NOTE]
> 인터페이스를 구현 하는 클래스를 사용 하려면 매개 변수가 없는 생성자가 있어야 합니다 `DependencyService`합니다.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS 구현

각 플랫폼 관련 응용 프로그램 프로젝트에는 인터페이스를 구현 해야 합니다. 클래스는 매개 변수가 없는 생성자에 있도록는 `DependencyService` 새 인스턴스를 만들 수 있습니다.

```csharp
[assembly: Dependency(typeof(TextToSpeechImplementation))]
namespace DependencyServiceSample.iOS
{

    public class TextToSpeechImplementation : ITextToSpeech
    {
        public TextToSpeechImplementation() { }

        public void Speak(string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer();
            var speechUtterance = new AVSpeechUtterance(text)
            {
                Rate = AVSpeechUtterance.MaximumSpeechRate / 4,
                Voice = AVSpeechSynthesisVoice.FromLanguage("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance(speechUtterance);
        }
    }
}
```

`[assembly]` 특성의 구현으로 클래스를 등록 합니다 `ITextToSpeech` 인터페이스, 즉 `DependencyService.Get<ITextToSpeech>()` 의 인스턴스를 만드는 공유 코드에 사용할 수 있습니다.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android 구현

Android 코드는 iOS 버전 보다 더 복잡: Android 관련에서 상속 하도록 구현 하는 클래스 필요 `Java.Lang.Object` 및 구현 하는 `IOnInitListener` 인터페이스도 합니다. 에 의해 노출 되는 현재 Android 컨텍스트에 대 한 액세스도 필요 합니다 `MainActivity.Instance` 속성입니다.

```csharp
[assembly: Dependency(typeof(TextToSpeechImplementation))]
namespace DependencyServiceSample.Droid
{
    public class TextToSpeechImplementation : Java.Lang.Object, ITextToSpeech, TextToSpeech.IOnInitListener
    {
        TextToSpeech speaker;
        string toSpeak;

        public void Speak(string text)
        {
            toSpeak = text;
            if (speaker == null)
            {
                speaker = new TextToSpeech(MainActivity.Instance, this);
            }
            else
            {
                speaker.Speak(toSpeak, QueueMode.Flush, null, null);
            }
        }

        public void OnInit(OperationResult status)
        {
            if (status.Equals(OperationResult.Success))
            {
                speaker.Speak(toSpeak, QueueMode.Flush, null, null);
            }
        }
    }
}
```

`[assembly]` 특성의 구현으로 클래스를 등록 합니다 `ITextToSpeech` 인터페이스, 즉 `DependencyService.Get<ITextToSpeech>()` 의 인스턴스를 만드는 공유 코드에 사용할 수 있습니다.

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>유니버설 Windows 플랫폼 구현

유니버설 Windows 플랫폼에서 speech API에는 `Windows.Media.SpeechSynthesis` 네임 스페이스입니다. 눈금에 한 가지 주의할은 **마이크** 기능 매니페스트에서 그렇지 않은 경우에 대 한 액세스 speech Api에서 차단 됩니다.

```csharp
[assembly:Dependency(typeof(TextToSpeechImplementation))]
public class TextToSpeechImplementation : ITextToSpeech
{
    public async void Speak(string text)
    {
        var mediaElement = new MediaElement();
        var synth = new Windows.Media.SpeechSynthesis.SpeechSynthesizer();
        var stream = await synth.SynthesizeTextToStreamAsync(text);

        mediaElement.SetSource(stream, stream.ContentType);
        mediaElement.Play();
    }
}
```

`[assembly]` 특성의 구현으로 클래스를 등록 합니다 `ITextToSpeech` 인터페이스, 즉 `DependencyService.Get<ITextToSpeech>()` 의 인스턴스를 만드는 공유 코드에 사용할 수 있습니다.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>공유 코드에서 구현

이제 작성 하 고 텍스트 음성 변환 인터페이스에 액세스 하는 공유 코드를 테스트할 수 했습니다. 이 간단한 페이지에는 음성 기능을 트리거하는 단추가 포함 됩니다. 사용 하 여는 `DependencyService` 의 인스턴스를 가져오려면 합니다 `ITextToSpeech` 인터페이스 &ndash; 런타임 시이 인스턴스 네이티브 SDK에 대 한 전체 액세스 권한이 있는 플랫폼 전용 구현 됩니다.

```csharp
public MainPage ()
{
    var speak = new Button {
        Text = "Hello, Forms !",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    speak.Clicked += (sender, e) => {
        DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
    };
    Content = speak;
}
```

IOS, Android 또는 UWP에서이 응용 프로그램을 실행 하 고 단추를 누르면 말하기 네이티브 speech SDK를 사용 하 여 각 플랫폼에서 응용 프로그램에서 발생 합니다.

 ![iOS 및 Android 텍스트 음성 변환 단추](text-to-speech-images/running.png "텍스트 음성 변환 샘플")


## <a name="related-links"></a>관련 링크

- [DependencyService (샘플)를 사용 하 여](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample/)
- [텍스트를 음성으로 통합 문서](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/text-to-speech/text-to-speech.workbook)
