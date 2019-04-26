---
title: Xamarin.mac에서 AVAudioPlayer로 소리 재생
description: 이 문서에서는 Xamarin.Mac 앱에서 AVAudioPlayer로 소리를 재생 하는 방법을 설명 합니다. AVAudioPlayer 자세히에 탐색 하는 기타 문서에 대 한 링크를 높은 수준에서 설명 합니다.
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 10/19/2016
ms.openlocfilehash: 9aeb7bbfc2fddef1f690b5299ec060c475ea1ce7
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61234868"
---
# <a name="playing-sound-with-avaudioplayer-in-xamarinmac"></a>Xamarin.mac에서 AVAudioPlayer로 소리 재생

## <a name="about-the-avaudioplayer"></a>AVAudioPlayer에 대 한

`AVAudioPlayer` 클래스는 메모리 또는 파일에서 오디오 데이터를 재생 하는 데 사용 됩니다. Apple이이 클래스를 사용 하 여 네트워크 스트리밍을 수행 하는 작업 또는 대기 시간이 짧은 오디오 I/O 필요 하지 않는 한 앱에서 오디오를 재생 하는 것이 좋습니다.

사용할 수는 `AVAudioPlayer` 다음을 수행 하는 클래스:

- 선택적 반복 재생 시간이의 소리를 재생 합니다.
- 선택적 동기화를 사용 하 여 동시에 여러 소리를 재생 합니다.
- 볼륨, 재생 속도 및 각 사운드 재생에 대 한 스테레오 배치를 제어 합니다.
- 빨리 감기 또는 rewind 같은 기능을 지원 합니다.
- 재생 수준을 계량 데이터를 가져옵니다.

`AVAudioPlayer` iOS, tvOS 및 macOS.aif,.wav 또는. mp3 등에서 제공 하는 모든 오디오 형식에서 소리를 지원 합니다.

## <a name="playing-sounds-in-macos"></a>MacOS에서 소리 재생

MacOS iOS와 같은 오디오 도구 상자 클래스를 지원 하므로 iOS를 참조 하세요 [AVAudioPlayer로 소리 재생](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer) Xamarin.Mac 앱에서 오디오 재생의 전체 세부 정보에 대 한 설명서입니다.

## <a name="related-links"></a>관련 링크

- [AVAudioPlayer 참조](https://developer.apple.com/documentation/avfoundation/avaudioplayer)
