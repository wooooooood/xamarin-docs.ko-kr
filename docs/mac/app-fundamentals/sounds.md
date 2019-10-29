---
title: Xamarin.ios에서 Av오디오 플레이어로 소리 재생
description: 이 문서에서는 Xamarin.ios 앱에서 Av오디오 플레이어로 소리를 재생 하는 방법을 설명 합니다. 높은 수준의 Avother 플레이어에 대해 설명 하 고 그에 대 한 링크를 제공 하는 다른 설명서에 대 한 링크를 제공 합니다.
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 10/19/2016
ms.openlocfilehash: 18043a88a129d48a1cad3b9ee15b6989d50ad126
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030051"
---
# <a name="playing-sound-with-avaudioplayer-in-xamarinmac"></a>Xamarin.ios에서 Av오디오 플레이어로 소리 재생

## <a name="about-the-avaudioplayer"></a>Av오디오 플레이어 정보

`AVAudioPlayer` 클래스는 메모리 또는 파일에서 오디오 데이터를 재생 하는 데 사용 됩니다. 네트워크 스트리밍을 수행 하거나 대기 시간이 짧은 오디오 i/o를 요구 하지 않는 한 Apple은이 클래스를 사용 하 여 앱에서 오디오를 재생 하는 것을 권장 합니다.

`AVAudioPlayer` 클래스를 사용 하 여 다음을 수행할 수 있습니다.

- 선택적인 반복이 있는 모든 기간의 소리를 재생 합니다.
- 선택적 동기화를 사용 하 여 여러 소리를 동시에 재생 합니다.
- 재생 하는 각 사운드의 볼륨, 재생 빈도 및 스테레오 위치를 제어 합니다.
- 빨리 감기 또는 되감기와 같은 기능을 지원 합니다.
- 재생 수준 계량 데이터를 가져옵니다.

`AVAudioPlayer`은 iOS, tvOS 및 macOS에서 제공 하는 오디오 형식 (예: aif, .wav 또는 mp3)의 소리를 지원 합니다.

## <a name="playing-sounds-in-macos"></a>MacOS에서 소리 재생

MacOS는 iOS와 같은 오디오 도구 상자 클래스를 지원 하기 때문에 Xamarin.ios 앱에서 오디오를 재생 하는 방법에 대 한 자세한 내용은 [Avaudio player를 사용 하 여 Ios 게임 재생](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer) 설명서를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [Av오디오 플레이어 참조](https://developer.apple.com/documentation/avfoundation/avaudioplayer)
