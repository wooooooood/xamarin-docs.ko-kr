---
title: "Xamarin.Mac 확장 프로그램별 지원 기능"
description: "이 문서에서는 Xamarin.Mac 버전 2.10 (이상)의 확장 프로그램별 지원 기능에 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4148F1BE-DFA0-46B6-9FCD-425A6541F510
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 5ce20322b576b12ff9dfe56ef0bc9d2e1ca27792
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinmac-extension-support"></a>Xamarin.Mac 확장 프로그램별 지원 기능

Xamarin.Mac 2.10 미리 보기에서 여러 macOS 확장 지점에 대 한 지원이 추가 되었습니다.

- 찾기
- 공유
- 오늘

<a name="Limitations-and-Known-Issues" />

## <a name="limitations-and-known-issues"></a>제한 사항 및 알려진된 문제

제한 사항은 आ स ा Xamarin.Mac에서 확장을 개발 하는 경우 발생할 수 있는 문제를 알고 있습니다.

* 현재 Mac.에 대 한 Visual Studio에서 디버깅 지원 되지 않습니다. 통해 수행 해야 할는 모든 디버깅 **NSLog** 및 **콘솔**합니다. 자세한 내용은 아래의 팁 섹션을 참조 하십시오.
* 확장에 포함 되어야 합니다 되는 레지스터 시스템으로 사용 하 여 한 번 실행 호스트 응용 프로그램의 경우. 사용할 수 있어야 다음는 **확장** 섹션 **시스템 환경 설정**합니다. 
* 일부 확장 충돌 호스트 응용 프로그램이 불안정 하 고 이상한 동작이 발생 수 있습니다. 특히, **Finder** 및는 **오늘** 의 섹션은 **알림 센터** "걸린" 되 고 응답 하지 않을 수 있습니다. 이 확장 프로젝트에서 Xcode에서도 숙련 된 되었으며 현재 Xamarin.Mac 관련이 나타납니다. 시스템 로그에서이 수 표시 하는 경우가 많습니다 (통해 **콘솔**, 세부 정보에 대 한 팁을 참조 하십시오.) 반복 된 오류 메시지를 인쇄 합니다. 이 문제를 해결 하려면 표시 macOS를 다시 시작 됩니다.

<a name="Tips" />

## <a name="tips"></a>팁

다음 팁 Xamarin.Mac에서 확장을 작업할 때 도움이 될 수 있습니다.

- 디버깅 환경을 실행에 주로 다릅니다으로 현재 Xamarin.Mac는 디버깅 확장을 지원 하지 않으므로, 및 `printf` 문을 선택 합니다. 그러나 확장에서에서 실행 하는 샌드박스 프로세스를 따라서 `Console.WriteLine` 와 다른 Xamarin.Mac 응용 프로그램에서 동작 하지 것입니다. 호출 [ `NSLog` 직접](https://gist.github.com/chamons/e2e409013a449cfbe1f2fbe5547f6554) 시스템 로그에 디버깅 메시지를 출력 합니다.
- 확인할 수 없는 모든 예외에는 적은 양의에서 유용한 정보를 제공 하는 확장 프로세스 작동이 중단 됩니다는 **시스템 로그**합니다. 문제가 있는 코드를 래핑하는 `try/catch` (예외) 블록을 `NSLog`의 전에 다시 throw 할 유용할 수 있습니다.
- **시스템 로그** 에서 액세스할 수는 **콘솔** 모드로 응용 프로그램 **응용 프로그램** > **유틸리티**:

    [ ![](extensions-images/extension02.png "시스템 로그")](extensions-images/extension02.png)
- 위에서 언급 한 대로 확장 호스트 응용 프로그램 실행은 등록 하 여 시스템. 삭제 된 응용 프로그램 번들 등록을 취소 합니다. 
- "흩어진" 버전의 응용 프로그램 확장 등록 된 다음 명령을 사용 하 여 (하므로 삭제할 수 있습니다) 찾기: `plugin kit -mv`


<a name="Walkthrough-and-Sample-App" />

## <a name="walkthrough-and-sample-app"></a>연습 및 샘플 앱

개발자는 만들고 Xamarin.Mac 확장과 함께 Xamarin.iOS 확장과 동일한 방식으로 작동 하므로를 참조 하십시오 우리의 [확장명 소개](~/ios/platform/extensions.md) 자세한 세부 정보에 대 한 설명서입니다.

작은 포함 하는 예제 Xamarin.Mac 프로젝트에서 각 확장 프로그램 종류의 작업 예제 있습니다 [여기](https://developer.xamarin.com/samples/mac/ExtensionSamples/)합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서 개요 Xamarin.Max 버전 2.10 (및 큰) 응용 프로그램에서 확장 작업을 수행 했습니다.

## <a name="related-links"></a>관련 링크

- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [ExtensionSamples](https://developer.xamarin.com/samples/mac/ExtensionSamples/)
- [OS X 사용자 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
