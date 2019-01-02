---
title: 텍스트 음성 변환 구현
description: 이 문서에서는 Xamarin.Forms DependencyService 클래스를 사용하여 각 플랫폼의 네이티브 텍스트 음성 변환 API로 호출하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2017
ms.openlocfilehash: 0351436259bb782e4f8e3a3405b9620c4e8b20bb
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53050753"
---
# <a name="implementing-text-to-speech"></a>텍스트 음성 변환 구현

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)

이 문서에서는 [`DependencyService`](xref:Xamarin.Forms.DependencyService)를 사용하여 네이티브 텍스트 음성 변환 API에 액세스하는 플랫폼 간 앱을 만드는 방법을 안내합니다.

- **[인터페이스 만들기](#Creating_the_Interface)** &ndash; 공유 코드에서 인터페이스를 만드는 방법을 이해합니다.
- **[iOS 구현](#iOS_Implementation)** &ndash; iOS용 네이티브 코드에서 인터페이스를 구현하는 방법을 알아봅니다.
- **[Android 구현](#Android_Implementation)** &ndash; Android용 네이티브 코드에서 인터페이스를 구현하는 방법을 알아봅니다.
- **[UWP 구현](#WindowsImplementation)** &ndash; UWP(유니버설 Windows 플랫폼)용 네이티브 코드에서 인터페이스를 구현하는 방법을 알아봅니다.
- **[공유 코드에서 구현](#Implementing_in_Shared_Code)** &ndash; 공유 코드에서 기본 구현을 호출하기 위해 `DependencyService`를 사용하는 방법을 알아봅니다.

`DependencyService`를 사용한 애플리케이션에는 다음과 같은 구조가 있습니다.

![](text-to-speech-images/tts-diagram.png "DependencyService 애플리케이션 구조")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>인터페이스 만들기

먼저 구현하려는 기능이 표시되는 공유 코드에서 인터페이스를 만듭니다. 예를 들어 인터페이스에는 단일 메서드가 포함됩니다. `Speak`

```csharp
public interface ITextToSpeech
{
    void Speak (string text);
}
```

공유 코드에서 이 인터페이스에 대한 코딩을 하면 Xamarin.Forms 앱에서 각 플랫폼의 speech API에 액세스할 수 있습니다.

> [!NOTE]
> 인터페이스를 구현하는 클래스에서 `DependencyService`를 사용하려면 매개 변수가 없는 생성자가 있어야 합니다.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS 구현

각 플랫폼별 애플리케이션 프로젝트에서 인터페이스를 구현해야 합니다. 클래스에는 매개 변수가 없는 생성자가 있으므로 `DependencyService`가 새 인스턴스를 만들 수 있습니다.

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

`[assembly]` 특성은 해당 클래스를 `ITextToSpeech` 인터페이스의 구현으로 등록합니다. 즉, 공유 코드에서 `DependencyService.Get<ITextToSpeech>()`을 사용하여 해당 인스턴스를 만들 수 있습니다.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android 구현

Android 코드는 iOS 버전보다 더 복잡합니다. 구현 클래스가 Android별 `Java.Lang.Object`에서 상속받고 `IOnInitListener` 인터페이스도 구현해야 합니다. `MainActivity.Instance` 속성에 의해 노출되는 현재 Android 컨텍스트에 대한 액세스도 필요합니다.

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

`[assembly]` 특성은 해당 클래스를 `ITextToSpeech` 인터페이스의 구현으로 등록합니다. 즉, 공유 코드에서 `DependencyService.Get<ITextToSpeech>()`을 사용하여 해당 인스턴스를 만들 수 있습니다.

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>유니버설 Windows 플랫폼 구현

유니버설 Windows 플랫폼에는 `Windows.Media.SpeechSynthesis` 네임스페이스에 speech API가 있습니다. 한 가지 주의사항은 매니페스트에서 **마이크** 기능을 틱해야 하는 것입니다. 그렇지 않은 경우 speech API에 대한 액세스가 차단됩니다.

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

`[assembly]` 특성은 해당 클래스를 `ITextToSpeech` 인터페이스의 구현으로 등록합니다. 즉, 공유 코드에서 `DependencyService.Get<ITextToSpeech>()`을 사용하여 해당 인스턴스를 만들 수 있습니다.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>공유 코드에서의 구현

이제 텍스트 음성 변환 인터페이스에 액세스하는 공유 코드를 작성하고 테스트할 수 있습니다. 이 간단한 페이지에는 음성 기능을 트리거하는 단추가 포함됩니다. 이 페이지는 `DependencyService`를 사용하여 `ITextToSpeech` 인터페이스의 인스턴스를 가져옵니다 &ndash; 런타임 시 이 인스턴스는 기본 SDK에 대한 모든 액세스 권한이 있는 플랫폼별 구현이 됩니다.

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

iOS, Android 또는 UWP에서 이 애플리케이션을 실행하고 단추를 누르면 애플리케이션에서 각 플랫폼의 네이티브 speech SDK를 사용하여 사용자에게 말을 겁니다.

 ![iOS 및 Android 텍스트 음성 변환 단추](text-to-speech-images/running.png "텍스트 음성 변환 샘플")


## <a name="related-links"></a>관련 링크

- [DependencyService 사용(샘플)](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample/)
