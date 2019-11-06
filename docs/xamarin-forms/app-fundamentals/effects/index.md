---
title: Xamarin.Forms 효과
description: 효과를 사용하면 사용자 지정 렌더러 구현에 의존하지 않고도 각 플랫폼의 네이티브 컨트롤을 사용자 지정할 수 있습니다.
ms.prod: xamarin
ms.assetid: 8AF168A7-4CD9-4603-B961-15B8B1543784
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: 2de1d1dd065a01bb457ebf03acdc0c01529abf7b
ms.sourcegitcommit: 1242d32b7f072c837005cdee174abe6c0d1d0c68
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73083833"
---
# <a name="xamarinforms-effects"></a>Xamarin.Forms 효과

_Xamarin.Forms 사용자 인터페이스는 대상 플랫폼의 네이티브 컨트롤을 사용하여 렌더링되기 때문에 Xamarin.Forms 애플리케이션이 각 플랫폼에 적합한 모양과 느낌을 유지할 수 있습니다. 효과를 사용하면 사용자 지정 렌더러 구현에 의존하지 않고도 각 플랫폼의 네이티브 컨트롤을 사용자 지정할 수 있습니다._

## <a name="introduction-to-effectsintroductionmd"></a>[효과 소개](introduction.md)

효과를 사용하면 각 플랫폼의 네이티브 컨트롤을 사용자 지정할 수 있습니다. 효과는 일반적으로 작은 스타일 지정 변경에 사용됩니다. 이 문서에서는 효과에 대한 소개를 제공하고 효과와 사용자 지정 렌더러 사이의 경계를 간략히 설명하고 `PlatformEffect` 클래스를 설명합니다.

## <a name="creating-an-effectcreatingmd"></a>[효과 만들기](creating.md)

효과는 컨트롤 사용자 지정을 간소화합니다. 이 문서에서는 컨트롤에 포커스가 있을 때 [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤의 배경 색을 변경하는 효과를 만드는 방법을 보여줍니다.

## <a name="passing-parameters-to-an-effectpassing-parametersindexmd"></a>[효과에 매개 변수 전달](passing-parameters/index.md)

매개 변수를 통해 구성되는 효과를 만들면 효과를 다시 사용할 수 있습니다. 이 문서에서는 매개 변수를 사용하여 효과에 매개 변수를 전달하고 런타임에 매개 변수를 변경하는 방법을 보여줍니다.

## <a name="invoking-events-from-an-effecttouch-trackingmd"></a>[효과에서 이벤트 호출](touch-tracking.md)

효과는 이벤트를 호출할 수 있습니다. 이 문서에서는 하위 수준 멀티 터치 핑거 추적을 구현하고 애플리케이션에 터치 누름, 이동 및 릴리스 신호를 보내는 이벤트를 만드는 방법을 보여줍니다.

## <a name="reusable-roundeffectreusable-roundeffectmd"></a>[재사용 가능한 RoundEffect](reusable-roundeffect.md)

RoundEffect는 VisualElement에서 파생된 모든 컨트롤에 적용하여 컨트롤을 원으로 렌더링할 수 있는 재사용 가능한 효과입니다. 이 효과는 원형 이미지, 원형 단추 및 기타 원형 컨트롤을 만드는 데 사용할 수 있습니다.
