---
title: Xamarin.Mac 확장 지원
description: 이 문서에서는 찾기, 공유 및 오늘 확장에 대 한 Xamarin.ios의 지원에 대해 설명 합니다. 제한 사항 및 알려진 문제를 조사 하 고, 연습 및 샘플 앱에 대 한 링크를 제공 하 고, 확장 작업에 대 한 팁을 제공 합니다.
ms.prod: xamarin
ms.assetid: 4148F1BE-DFA0-46B6-9FCD-425A6541F510
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 2129281f389c440d9ae746c4b9b06c4ddb32d1dc
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770037"
---
# <a name="xamarinmac-extension-support"></a>Xamarin.Mac 확장 지원

2\.10 Xamarin.ios에서 여러 macOS 확장 점에 대 한 지원이 추가 되었습니다.

- Finder
- 공유
- 오늘

<a name="Limitations-and-Known-Issues" />

## <a name="limitations-and-known-issues"></a>제한 사항 및 알려진 문제

다음은 Xamarin.ios에서 확장을 개발할 때 발생할 수 있는 제한 사항 및 알려진 문제입니다.

- 현재 Mac용 Visual Studio에는 디버깅을 지원 하지 않습니다. 모든 디버깅은 **Nslog** 및 **콘솔**을 통해 수행 해야 합니다. 자세한 내용은 아래의 팁 섹션을 참조 하십시오.
- 확장은 호스트 응용 프로그램에 포함 되어야 하며,이 응용 프로그램은 시스템에 등록 하 여 한 번 실행 될 때 사용할 수 있습니다. 그런 다음 **시스템 기본 설정**의 **확장** 섹션에서 사용 하도록 설정 해야 합니다. 
- 일부 확장 작동이 중단 되 면 호스트 응용 프로그램이 불안정 해질 수 있으며 이상한 동작이 발생할 수 있습니다. 특히 **알림 센터** 의 **Finder** 및 **Today** 섹션은 "걸린" 상태가 되 고 응답 하지 않게 될 수 있습니다. 이는 Xcode의 확장 프로젝트 에서도 발생 했으며 현재 Xamarin.ios와 관련이 없는 것으로 나타납니다. 시스템 로그에 표시 되는 경우가 종종 있습니다. ( **콘솔**을 통해 자세한 내용을 보려면 팁 참조) 반복 된 오류 메시지를 인쇄 합니다. MacOS를 다시 시작 하면이 문제를 해결할 수 있습니다.

<a name="Tips" />

## <a name="tips"></a>팁

Xamarin.ios에서 확장을 사용 하는 경우 다음과 같은 팁을 유용 하 게 사용할 수 있습니다.

- Xamarin.ios는 현재 디버깅 확장을 지원 하지 않으므로 디버깅 환경은 주로 실행과 `printf` 같은 문에 따라 달라 집니다. 그러나 확장은 샌드박스 프로세스에서 실행 되므로 `Console.WriteLine` 다른 xamarin.ios 응용 프로그램에서 수행 되는 것 처럼 작동 하지 않습니다. 을 [ `NSLog` 직접](https://gist.github.com/chamons/e2e409013a449cfbe1f2fbe5547f6554) 호출 하면 디버깅 메시지가 시스템 로그에 출력 됩니다.
- Catch 되지 않은 예외는 확장 프로세스를 중단 하 여 **시스템 로그**에 적은 양의 유용한 정보만 제공 합니다. 다시 throw 하기 전의 `try/catch` (예외) 블록에 문제가 있는 코드를 래핑하는 것이 `NSLog`유용할 수 있습니다.
- **시스템 로그** 는 **응용 프로그램** > **유틸리티**아래의 **콘솔** 앱에서 액세스할 수 있습니다.

    [![](extensions-images/extension02.png "시스템 로그")](extensions-images/extension02.png#lightbox)
- 위에서 설명한 것 처럼 확장 호스트 응용 프로그램을 실행 하면 시스템에 등록 됩니다. 응용 프로그램 번들을 삭제 하 고 등록을 취소 합니다. 
- 앱 확장의 "흩어진" 버전을 등록 하는 경우 다음 명령을 사용 하 여 해당 버전을 찾아서 삭제할 수 있습니다.`plugin kit -mv`

<a name="Walkthrough-and-Sample-App" />

## <a name="walkthrough-and-sample-app"></a>연습 및 샘플 앱

개발자는 xamarin.ios 확장과 동일한 방식으로 Xamarin.ios 확장을 만들고 작업 하므로 자세한 내용은 [확장 소개](~/ios/platform/extensions.md) 설명서를 참조 하세요.

각 확장 형식의 작은 작업 샘플을 포함 하는 Xamarin.ios 프로젝트 예제는 [여기](https://docs.microsoft.com/samples/xamarin/mac-samples/extensionsamples)에서 찾을 수 있습니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 버전 2.10 이상의 앱에서 확장을 사용 하는 방법을 간략히 살펴보겠습니다.

## <a name="related-links"></a>관련 링크

- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [ExtensionSamples](https://docs.microsoft.com/samples/xamarin/mac-samples/extensionsamples)
- [macOS 휴먼 인터페이스 지침](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)
