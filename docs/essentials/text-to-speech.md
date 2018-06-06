---
title: 'Xamarin.Essentials: 텍스트 음성 변환'
description: Xamarin.Essentials 사용 하면 장치에서 및 쿼리 엔진을 지원할 수 있는 사용 가능한 언어를 뒤로 텍스트를 텍스트 음성 변환 엔진에 응용 프로그램을 기본 제공 사용 TextToSpeech 클래스입니다.
ms.assetid: AEEF03AE-A047-4DF0-B0E8-CC8D9A7B8351
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 9383411074bc43af1034138aadbb6ac5494c2c01
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782803"
---
# <a name="xamarinessentials-text-to-speech"></a>Xamarin.Essentials: 텍스트 음성 변환

![시험판 NuGet](~/media/shared/pre-release.png)

**TextToSpeech** 클래스를 사용 하면 장치에서 및 쿼리 엔진을 지원할 수 있는 사용 가능한 언어를 뒤로 텍스트를 텍스트 음성 변환 엔진에 기본 제공 활용 하 여 응용 프로그램입니다.

## <a name="using-text-to-speech"></a>텍스트 음성 변환 사용

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

호출 하 여 텍스트 음성 변환 기능이 작동 하는 `SpeakAsync` 텍스트 및 선택적 매개 변수 및 반환 된 utterance 완료 된 후 메서드. 

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

선택적 CancellationToken에 일단 시작 된 utterance를 중지 하려면이 메서드를 사용 합니다. 
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

텍스트 음성 변환 동일한 스레드에서 음성 요청을 대기 자동으로 됩니다. 

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

오디오 사용 되는 방식을 보다 자세히 제어 함께 다시 `SpeakSettings` 볼륨, 피치 및 로캘 설정 하도록 허용 하는 합니다.

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

각 플랫폼으로 언급 해야 할 여러 언어와 악센트에서 뒤로 텍스트 로캘을 제공 합니다. 각 플랫폼에는 다른 코드와 한 방법으로이 지정 하는 이유 Essentials 제공 플랫폼 간 `Locale` 클래스 및를 사용 하 여 쿼리할 수 있는 방법을 `GetLocalesAsync`합니다.

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

- 여러 스레드에서 호출 하면 utterance 큐 보장 되지 않습니다.
- 오디오 재생 배경 공식적으로 지원 되지 않습니다.

## <a name="api"></a>API

- [TextToSpeech 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/TextToSpeech)
- [TextToSpeech API 설명서](xref:Xamarin.Essentials.TextToSpeech)
