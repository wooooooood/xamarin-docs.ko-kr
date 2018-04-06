---
title: 계측을 사용하여 Xamarin.iOS 응용 프로그램 프로파일링
description: 장치 또는 시뮬레이터에서 Xamarin.iOS 응용 프로그램에 대해 계측을 사용하는 방법
ms.prod: xamarin
ms.assetid: 70A8CAC8-20C2-655B-37C3-ACF9EA7874D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: a5c44531a8f05529b169390f25b2946133411098
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="profiling-xamarinios-applications-with-instruments"></a>계측을 사용하여 Xamarin.iOS 응용 프로그램 프로파일링

_장치 또는 시뮬레이터에서 Xamarin.iOS 응용 프로그램에 대해 계측을 사용하는 방법_

Xcode **계측**은 장치 또는 시뮬레이터에서 Xamarin.iOS 앱을 프로파일링하는 데 사용할 수 있는 도구입니다. Mono는 JIT(Just-in-Time) 모델을 사용하여 코드를 컴파일하고 계측은 이러한 종류의 데이터를 잘 해석하지 않으므로, 계측을 사용하는 시뮬레이터 기반 응용 프로그램의 출력 작업이 어려울 수 있습니다.
이 문제로 인해 이 가이드에서는 개발자 앱을 사용하여 이 문서의 계측 출력을 해석하는 방법에 집중합니다.

## <a name="requirements"></a>요구 사항

Xcode 계측은 Mac에서만 실행됩니다.

## <a name="opening-the-instruments-app"></a>계측 앱 열기

장치를 선택하고 계측 앱을 실행합니다.

1.  Mac용 Visual Studio에서 Xamarin.iOS 프로젝트를 엽니다.
2.  **디버그 | iPhone** 구성을 선택합니다.
3.  컴퓨터에 iOS 장치를 연결합니다.
4.  **실행** 메뉴에서 **장치에 업로드**를 선택합니다. 이제 응용 프로그램이 빌드되어 장치에 업로드됩니다.
5.  **도구** 메뉴에서 **계측 시작**을 선택합니다.


이제 계측이 열리고 다음 대화 상자가 표시됩니다.

 [![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments1.png "프로파일링 템플릿 선택")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments1.png#lightbox)

클릭하여 **할당** 템플릿을 선택합니다. 다른 템플릿도 유효하지만, 이 문서에서는 **할당** 프로필 템플릿만 설명합니다.

다음으로, 창의 위쪽에 있는 메뉴를 사용하여 장치와 응용 프로그램을 선택합니다.

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments2.png "장치 및 응용 프로그램 선택")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments2.png#lightbox)

iOS 장치는 창 위쪽의 메뉴에서 선택해야 하며, 프로비전할 응용 프로그램은 해당 장치(위 스크린샷의 **MemoryDemo**) 옆에서 선택해야 합니다.

장치가 메뉴에 나열되지 않으면 Mac용 Visual Studio의 **콘솔**에서 앱을 장치에 배포할 때 표시될 수 있는 오류 메시지를 확인합니다. 또한 장치가 개발을 위해 Xcode 구성 도우미를 통해 프로비전되었는지 확인합니다.

**선택** 단추를 클릭하면 다음 화면이 표시됩니다.

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments3.png "프로파일링 인터페이스")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments3.png#lightbox)

레코드 단추(왼쪽 위의 빨간색 원)를 클릭하여 프로파일링을 시작합니다.

다음 스크린샷에서는 **계측**을 사용한 프로파일링의 예를 보여 줍니다.

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments4.png "계측을 사용한 프로파일링의 예제")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments4.png#lightbox)

## <a name="summary"></a>요약

이 가이드에서는 Mac용 Visual Studio 내에서 Xcode 계측을 시작하여 iOS 앱을 모니터링하는 방법을 보여 주었습니다. 계측을 사용하여 메모리 문제를 진단하는 방법의 예는 [계측 연습](~/ios/deploy-test/walkthrough-apples-instrument.md)으로 계속 진행하세요.

## <a name="related-links"></a>관련 링크

- [계측 연습](~/ios/deploy-test/walkthrough-apples-instrument.md)
- [Xamarin.iOS 가비지 수집](https://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/)
