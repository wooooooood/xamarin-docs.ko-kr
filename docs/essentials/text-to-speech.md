---
title: 'Xamarin.Essentials: 텍스트 음성 변환'
description: Xamarin.Essentials 사용 하면 장치에서 그리고 엔진을 지원할 수 있는 쿼리 사용 가능한 언어 백 텍스트를 말하는 텍스트 음성 변환 엔진에서 응용 프로그램을 기본 제공 활용 TextToSpeech 클래스입니다.
ms.assetid: AEEF03AE-A047-4DF0-B0E8-CC8D9A7B8351
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 9383411074bc43af1034138aadbb6ac5494c2c01
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815663"
---
# <a name="xamarinessentials-text-to-speech"></a>Xamarin.Essentials: 텍스트 음성 변환

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **TextToSpeech** 클래스를 사용 하면 장치에서 그리고 엔진을 지원할 수 있는 쿼리 사용 가능한 언어 백 텍스트를 말하는 텍스트 음성 변환 엔진에서 기본 제공을 활용 하는 응용 프로그램입니다.

## <a name="using-text-to-speech"></a>텍스트 음성 변환 사용

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

텍스트 음성 변환 기능을 호출 하 여 작동 합니다 `SpeakAsync` 메서드 텍스트 및 선택적 매개 변수를 utterance 완료 된 후 반환 합니다. 

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

이 메서드는 일단 시작 된 utterance를 중지 하는 선택적 CancellationToken에 사용 합니다. 
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

Text to speech는 동일한 스레드에서 음성 요청을 큐 자동으로 합니다. 

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

오디오 사용 되는 방식을 보다 자세히 제어를 사용 하 여 다시 `SpeakSettings` 볼륨, 피치 및 로캘 설정 수 있도록 합니다.

```csharp
public async Task SpeakNow()
{
    var settings = new SpeakSettings()
        {
            Volume = .75,
            Pitch = 1.0
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

다음은 이러한 매개 변수에 대해 지원 되는 값입니다.

| 매개 변수 | 최소 | Maximum |
| --- | :---: | :---: |
| 피치 | 0 | 2.0 |
| 볼륨 | 0 | 1.0 |

### <a name="speech-locales"></a>음성 로캘

각 플랫폼에는 여러 언어에서 악센트 백 텍스트 읽어주기 로캘을 제공 합니다. 각 플랫폼에 다른 코드 및 의미 하기 때문에이 지정 하는 방법으로 Essentials 제공 플랫폼 간 `Locale` 클래스와 사용 하 여 쿼리 하는 방법은 `GetLocalesAsync`합니다.

```csharp
public async Task SpeakNow()
{
    var locales = await TextToSpeech.GetLocalesAsync();

    // Grab the first locale
    var locale = locales.FirstOrDefault();

    var settings = new SpeakSettings()
        {
            Volume = .75,
            Pitch = 1.0,
            Locale = locale
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

## <a name="limitations"></a>제한 사항

- 여러 스레드에서 호출 되는 경우에 utterance 큐 보장 되지 않습니다.
- 백그라운드 오디오 재생 공식적으로 지원 되지 않습니다.

## <a name="api"></a>API

- [TextToSpeech 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/TextToSpeech)
- [TextToSpeech API 설명서](xref:Xamarin.Essentials.TextToSpeech)
