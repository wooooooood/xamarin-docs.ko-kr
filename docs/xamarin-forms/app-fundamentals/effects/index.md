---
title: Xamarin.Forms 효과
description: 효과 사용자 지정 렌더러 구현에 의존 하지 않고 지정할 각 플랫폼의 네이티브 컨트롤을 사용 합니다.
ms.prod: xamarin
ms.assetid: 8AF168A7-4CD9-4603-B961-15B8B1543784
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: 9d859377c40c6fca07e140c50da46d8f30aaae04
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994460"
---
# <a name="xamarinforms-effects"></a>Xamarin.Forms 효과

_Xamarin.Forms 사용자 인터페이스는 Xamarin.Forms 응용 프로그램이 각 플랫폼에 대 한 적절 한 모양과 느낌을 유지할 수 있도록 하는 대상 플랫폼의 네이티브 컨트롤을 사용 하 여 렌더링 됩니다. 효과 사용자 지정 렌더러 구현에 의존 하지 않고 지정할 각 플랫폼의 네이티브 컨트롤을 사용 합니다._

## <a name="introduction-to-effectsintroductionmd"></a>[효과 소개](introduction.md)

효과를 사용자 지정할 수는 각 플랫폼에서 네이티브 컨트롤을 허용 small 스타일 변경에 대 한 일반적으로 사용 됩니다. 이 문서 효과에 대해 소개 효과 및 사용자 지정 렌더러 간의 경계를 간략하게 설명 하 고 설명 된 `PlatformEffect` 클래스입니다.

## <a name="creating-an-effectcreatingmd"></a>[효과 만들기](creating.md)

효과 컨트롤의 사용자 지정을 간소화합니다. 이 문서에서는의 배경색을 변경 하는 효과 만드는 방법을 보여 줍니다.는 [ `Entry` ](xref:Xamarin.Forms.Entry) 컨트롤이 포커스를 획득 하는 경우를 제어 합니다.

## <a name="passing-parameters-to-an-effectpassing-parametersindexmd"></a>[효과로 매개 변수 전달](passing-parameters/index.md)

매개 변수를 통해 구성 된 효과 만들면 결과를를 다시 사용할 수 있습니다. 이러한 문서 속성을 사용 하 여 매개 변수는 효과를 전달 하는 방법을 보여 줍니다 하 고 런타임에 매개 변수를 변경 합니다.

## <a name="invoking-events-from-an-effecttouch-trackingmd"></a>[효과의 이벤트를 호출합니다.](touch-tracking.md)

결과 이벤트를 호출할 수 있습니다. 이 문서에서는 다중 터치 손가락 하위 수준 추적을 구현 하는 터치 누름, 이동 및 버전에 대 한 응용 프로그램을 알리는 이벤트를 만드는 방법을 보여 줍니다.
