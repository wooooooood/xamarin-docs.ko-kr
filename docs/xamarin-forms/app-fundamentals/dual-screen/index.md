---
title: Xamarin.Forms 이중 화면
description: 이 가이드에서는 이중 화면 디바이스용 Xamarin.Forms 앱을 만드는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: 344b6293090ffa4281ea6351f7f176a5be37e5bd
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "78165562"
---
# <a name="xamarinforms-dual-screen"></a>Xamarin.Forms 이중 화면

![](~/media/shared/preview.png "This API is currently pre-release")

Surface Duo(Android) 및 Surface Neo(Windows 10X)에서는 터치 애플리케이션을 위해 새로운 패턴을 도입합니다. Xamarin.Forms에는 이들 디바이스용으로 앱을 개발할 수 있는 `TwoPaneView` 클래스와 `DualScreenInfo` 클래스가 있습니다.

## <a name="dual-screen-design-patterns"></a>[이중 화면 디자인 패턴](design-patterns.md)

이중 화면 디바이스에서 다중 화면을 최대한 활용하는 방법을 고려할 때는 패턴 지침을 참조하여 애플리케이션 인터페이스에 적합한 패턴을 찾으세요.

## <a name="dual-screen-layout"></a>[이중 화면 레이아웃](twopaneview.md)

같은 이름의 UWP 컨트롤에서 영감을 받아 만들어진 Xamarin.Forms `TwoPaneView` 클래스는 이중 화면 디바이스에 최적화된 교차 플랫폼 레이아웃입니다.

## <a name="dual-screen-device-capabilities"></a>[이중 화면 디바이스 기능](dual-screen-info.md)

`DualScreenInfo` 클래스를 사용하면 보기가 표시된 창과 그 크기, 디바이스의 방향, 힌지 각도 등을 확인할 수 있습니다.

## <a name="dual-screen-platform-helpers"></a>[이중 화면 플랫폼 도우미](dual-screen-helper.md)

`DualScreenHelper` 클래스를 사용하면 플랫폼에서 화면 속 화면으로 새 창 결기를 지원하는지 여부를 확인할 수 있습니다. Neo의 경우, 디바이스가 작성 모드일 때 원더바(Wonderbar)에서 표시되는 창을 열 수 있습니다.

## <a name="dual-screen-triggers"></a>[이중 화면 트리거](triggers.md)

[`Xamarin.Forms.DualScreen`](xref:Xamarin.Forms.DualScreen) 네임스페이스에는 연결된 레이아웃 또는 창의 보기 모드가 변경될 때 [`VisualState`](xref:Xamarin.Forms.VisualState) 변경을 트리거하는 두 개의 상태 트리거가 포함되어 있습니다.
