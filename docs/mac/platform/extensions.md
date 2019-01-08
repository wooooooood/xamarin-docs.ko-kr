---
title: Xamarin.Mac 확장 지원
description: 이 문서에는 찾기, 공유 및 오늘 확장에 대 한 Xamarin.Mac의 지원을 설명합니다. 제한 사항 및 알려진된 문제, 연습 및 샘플 앱에 대 한 링크를 검사 하 고 확장 작업에 대 한 팁을 제공 합니다.
ms.prod: xamarin
ms.assetid: 4148F1BE-DFA0-46B6-9FCD-425A6541F510
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 0f4d6bb042f8bc8d48b45d7148984a53e3ce3437
ms.sourcegitcommit: e3e851080e6ea0b77e355487a61348d8e0419b09
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/07/2019
ms.locfileid: "54060072"
---
# <a name="xamarinmac-extension-support"></a>Xamarin.Mac 확장 지원

Xamarin.Mac 2.10에서 여러 macOS 확장 지점에 대 한 지원이 추가 되었습니다.

- 찾기
- 공유
- 오늘

<a name="Limitations-and-Known-Issues" />

## <a name="limitations-and-known-issues"></a>제한 사항 및 알려진된 문제

다음 제한 사항은 및 Xamarin.Mac 확장을 개발 하는 경우 발생할 수 있는 문제 인지 알고 있어야 합니다.

* 현재 mac 용 Visual Studio에서 디버깅 지원 되지 않습니다. 통해 수행할 수 해야 모든 디버깅 **NSLog** 하며 **콘솔**합니다. 자세한 내용은 아래 팁 섹션을 참조 하세요.
* 확장에 포함 되어야 합니다 하나씩 시스템을 사용 하 여 등록을 사용 하 여 경우에는 호스트 응용 프로그램에서 실행 합니다. 다음에서 사용할 수 있어야 합니다 **확장** 부분 **시스템 기본 설정**. 
* 일부 확장 충돌 호스트 응용 프로그램을 불안정 하 고 이상한 동작이 발생 수 있습니다. 특히 **Finder** 및 **오늘** 섹션의 **알림 센터** "걸린" 되 고 응답 하지 않을 수 있습니다. 이 확장 프로젝트에서 Xcode에서도 숙련 된 되었고 현재 Xamarin.Mac 관련이 표시 합니다. 시스템 로그에서이 수 표시 하는 경우가 많습니다 (통해 **콘솔**, 세부 정보에 대 한 팁을 참조 하세요.) 반복된 오류 메시지를 인쇄 합니다. MacOS를 다시 시작 하는 작업은이 문제를 해결 하려면 표시 됩니다.

<a name="Tips" />

## <a name="tips"></a>팁

다음 팁을 Xamarin.Mac에 확장을 사용 하 여 작업할 때 유용할 수 있습니다.

- 현재 Xamarin.Mac는 디버깅 확장을 지원 하지 않으므로, 디버깅 환경이 주로 달라 집니다 실행 및 `printf` 문 같은 합니다. 그러나 확장 프로세스에서 실행 샌드박스, 따라서 `Console.WriteLine` 다른 Xamarin.Mac 응용 프로그램 처럼 동작 하지 것입니다. 호출 [ `NSLog` 직접](https://gist.github.com/chamons/e2e409013a449cfbe1f2fbe5547f6554) 시스템 로그에 디버깅 메시지를 출력 합니다.
- 모든 예외로 소량의에서 유용한 정보를 제공 하는 확장 프로세스 충돌 합니다 **시스템 로그**합니다. 래핑에 문제가 있는 코드를 `try/catch` (Exception) 블록을 `NSLog`의 유용할 수 있습니다 다시 throw 하기 전에 합니다.
- 합니다 **시스템 로그** 에서 액세스할 수 있습니다 합니다 **콘솔** 아래에 있는 앱 **응용 프로그램** > **유틸리티**:

    [![](extensions-images/extension02.png "시스템 로그")](extensions-images/extension02.png#lightbox)
- 위에서 언급 했 듯이 확장 호스트 응용 프로그램을 실행는 시스템에 등록 합니다. 사용 하 여 응용 프로그램 번들을 삭제 하면 등록을 취소 합니다. 
- "이탈" 버전의 응용 프로그램의 확장을 등록 하는 경우 다음 명령을 사용 하 여 (삭제) 따라서 찾을: `plugin kit -mv`


<a name="Walkthrough-and-Sample-App" />

## <a name="walkthrough-and-sample-app"></a>연습 및 샘플 앱

개발자가 만들고 Xamarin.Mac 확장을 사용 하 여 Xamarin.iOS 확장으로 동일한 방식으로 작동 하므로 하세요 우리의 [확장 프로그램 소개](~/ios/platform/extensions.md) 자세한 설명서.

작은 포함 하는 예제 Xamarin.Mac 프로젝트에서 각 확장 형식의 작업 예제를 찾을 수 있습니다 [여기](https://developer.xamarin.com/samples/mac/ExtensionSamples/)합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Mac 버전 2.10 (이상) 앱에서 확장을 사용 하 여 작업을 빠르게 확인을 수행 했습니다.

## <a name="related-links"></a>관련 링크

- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [ExtensionSamples](https://developer.xamarin.com/samples/mac/ExtensionSamples/)
- [macOS 휴먼 인터페이스 지침](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)
