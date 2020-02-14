---
title: Xamarin.Forms 이중 화면 기능
description: 이 가이드에서는 Xamarin.Forms에서 이중 화면 디바이스용 앱을 쉽게 지원하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: 1f648297a608c592d90f2c70494ae4496fe73c0f
ms.sourcegitcommit: 07941cf9704ff88cf4087de5ebdea623ff54edb1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77145517"
---
# <a name="xamarinforms-dual-screen"></a>Xamarin.Forms 이중 화면

![](~/media/shared/preview.png "This API is currently pre-release")

Surface Duo(Android) 및 Surface Neo(Windows 10X)에서는 터치 애플리케이션을 위해 새로운 패턴을 도입합니다. 이러한 새로운 환경을 최대한 활용할 수 있도록 하기 위해 Xamarin.Forms에서는 `TwoPaneView` 및 `DualScreenInfo`를 도입합니다.

## <a name="dual-screen-patternsdesign-patternsmd"></a>[이중 화면 패턴](design-patterns.md)

이중 화면 디바이스에서 다중 화면을 최대한 활용하는 방법을 고려할 때는 패턴 지침을 참조하여 애플리케이션 인터페이스에 적합한 패턴을 찾으세요.

## <a name="twopaneviewtwopaneviewmd"></a>[TwoPaneView](twopaneview.md)

같은 이름의 UWP 컨트롤에서 영감을 얻은 Xamarin.Forms `TwoPaneView` 클래스는 플랫폼 간 방식으로 이중 화면 디바이스에 최적화된 레이아웃을 제공합니다.

## <a name="dualscreeninfodual-screen-infomd"></a>[DualScreenInfo](dual-screen-info.md)

이중 화면 디바이스를 최대한 활용하려면 보기가 있는 창, 보기의 크기, 디바이스의 방향, 힌지 각도 등을 알아야 할 수 있습니다. 이런 정보는 `DualScreenInfo`에서 제공합니다.

## <a name="dualscreenhelperdual-screen-helpermd"></a>[DualScreenHelper](dual-screen-helper.md)
`DualScreenHelper`를 사용하면 플랫폼에서 화면 속 화면으로 새 창 열기를 지원하는지 있습니다. Neo의 경우 디바이스가 작성 모드일 때 원더바(Wonderbar)에 표시되는 창을 열 수 있습니다.