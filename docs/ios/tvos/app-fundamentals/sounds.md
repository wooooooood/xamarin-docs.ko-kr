---
title: Xamarin에 AVAudioPlayer와 tvOS에서 소리 재생
description: 이 문서에서는 Xamarin.iOS 응용 프로그램에는 AVAudioPlayer를 사용 하 여 소리 재생을 제어 하는 도우미 클래스를 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 7d95a8ea6c22c0d897d8ccfe0c2ca401f6523783
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788636"
---
# <a name="playing-sound-in-tvos-with-avaudioplayer-in-xamarin"></a>Xamarin에 AVAudioPlayer와 tvOS에서 소리 재생

## <a name="about-the-avaudioplayer"></a>AVAudioPlayer에 대 한

`AVAudioPlayer` 메모리 또는 파일에서 오디오 데이터를 재생 하는 데 사용 됩니다. Apple이이 클래스를 사용 하 여 네트워크 스트리밍 수행 하는 하거나 오디오 I/O 대기 시간이 필요 하지 않는 앱에서 오디오를 재생 하는 것이 좋습니다.

사용할 수는 `AVAudioPlayer` 다음을 수행 합니다.

- 선택적 반복 구조 있는 모든 기간의 소리를 재생 합니다.
- 선택적 동기화와 동시에 여러 소리를 재생 합니다.
- 볼륨, 재생 속도 및 각 소리 재생에 대 한 스테레오 배치를 제어 합니다.
- 빨리 감기 또는 되감기와 같은 기능을 지원 합니다.
- 재생 수준을 계량 데이터를 가져옵니다.

`AVAudioPlayer` 과 같은 iOS, tvOS 및 OS X에서 제공 하는 모든 오디오 형식에서에서 소리 `.aif`, `.wav` 또는 `.mp3`합니다.

## <a name="playing-sounds-in-tvos"></a>TvOS에서 소리 재생

TvOS iOS 하므로 동일한 오디오 도구 상자 클래스를 지원 하므로 iOS를 참조 하십시오 [AVAudioPlayer와 소리 재생](http://developer.xamarin.com/recipes/ios/media/sound/avaudioplayer/) Xamarin.tvOS 앱에서 오디오 재생의 전체 세부 정보에 대 한 설명서입니다.



## <a name="related-links"></a>관련 링크

- [AVAudioPlayer 참조](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVAudioPlayerClassReference/)
