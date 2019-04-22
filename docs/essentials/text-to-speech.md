---
title: 'Xamarin.Essentials: 텍스트 음성 변환'
description: Xamarin.Essentials의 TextToSpeech 클래스를 사용하면 애플리케이션이 기본 제공 텍스트 음성 변환 엔진을 이용하여 장치에서 텍스트를 말하고, 엔진이 지원할 수 있는 사용 가능한 언어를 쿼리할 수도 있습니다.
ms.assetid: AEEF03AE-A047-4DF0-B0E8-CC8D9A7B8351
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 35ef922553cb91aa915c08df03414d1e5f3034cb
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59239929"
---
# <a name="xamarinessentials-text-to-speech"></a>Xamarin.Essentials: 텍스트 음성 변환

**TextToSpeech** 클래스를 사용하면 애플리케이션이 기본 제공 텍스트 음성 변환 엔진을 이용하여 장치에서 텍스트를 말하고, 엔진이 지원할 수 있는 사용 가능한 언어를 쿼리할 수도 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-text-to-speech"></a>텍스트 음성 변환 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

텍스트 음성 변환은 텍스트 및 선택적 매개 변수로 `SpeakAsync` 메서드를 호출하여 작동하며, 발화가 완료된 후 반환됩니다.

```csharp
public async Task SpeakNowDefaultSettings()
{
    await TextToSpeech.SpeakAsync("Hello World");

    // This method will block until utterance finishes.
}

public void SpeakNowDefaultSettings2()
{
    TextToSpeech.SpeakAsync("Hello World").ContinueWith((t) =>
    {
        // Logic that will run after utterance finishes.

    }, TaskScheduler.FromCurrentSynchronizationContext());
}
```

이 메서드는 선택적 `CancellationToken`을 사용하여 시작된 발화를 중지합니다.

```csharp
CancellationTokenSource cts;
public async Task SpeakNowDefaultSettings()
{
    cts = new CancellationTokenSource();
    await TextToSpeech.SpeakAsync("Hello World", cancelToken: cts.Token);

    // This method will block until utterance finishes.
}

public void CancelSpeech()
{
    if (cts?.IsCancellationRequested ?? false)
        return;

    cts.Cancel();
}
```

텍스트 음성 변환은 동일한 스레드의 음성 요청을 자동으로 큐에 넣습니다.

```csharp
bool isBusy = false;
public void SpeakMultiple()
{
    isBusy = true;
    Task.Run(async () =>
    {
        await TextToSpeech.SpeakAsync("Hello World 1");
        await TextToSpeech.SpeakAsync("Hello World 2");
        await TextToSpeech.SpeakAsync("Hello World 3");
        isBusy = false;
    });

    // or you can query multiple without a Task:
    Task.WhenAll(
        TextToSpeech.SpeakAsync("Hello World 1"),
        TextToSpeech.SpeakAsync("Hello World 2"),
        TextToSpeech.SpeakAsync("Hello World 3"))
        .ContinueWith((t) => { isBusy = false; }, TaskScheduler.FromCurrentSynchronizationContext());
}
```

### <a name="speech-settings"></a>음성 설정

볼륨, 피치 및 로캘을 설정할 수 있는 `SpeechOptions`를 사용하여 오디오를 말하는 방법을 세부적으로 제어할 수 있습니다.

```csharp
public async Task SpeakNow()
{
    var settings = new SpeechOptions()
        {
            Volume = .75f,
            Pitch = 1.0f
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

다음은 이러한 매개 변수에 대해 지원되는 값입니다.

| 매개 변수 | 최소 | Maximum |
| --- | :---: | :---: |
| 피치 | 0 | 2.0 |
| 볼륨 | 0 | 1.0 |

### <a name="speech-locales"></a>음성 로캘

각 플랫폼은 서로 다른 언어와 악센트로 텍스트를 말하기 위해 다양한 로캘을 지원합니다. 플랫폼마다 각기 다른 코드와 방법으로 로캘을 지정하며, 이 때문에 Xamarin.Essentials는 플랫폼 간 `Locale` 클래스와 `GetLocalesAsync`로 쿼리하는 방법을 제공합니다.

```csharp
public async Task SpeakNow()
{
    var locales = await TextToSpeech.GetLocalesAsync();

    // Grab the first locale
    var locale = locales.FirstOrDefault();

    var settings = new SpeechOptions()
        {
            Volume = .75f,
            Pitch = 1.0f,
            Locale = locale
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

## <a name="limitations"></a>제한 사항

- 여러 스레드에서 호출하는 경우 발화 큐는 보장되지 않습니다.
- 백그라운드 오디오 재생은 공식적으로 지원되지 않습니다.

## <a name="api"></a>API

- [TextToSpeech 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/TextToSpeech)
- [TextToSpeech API 문서](xref:Xamarin.Essentials.TextToSpeech)
