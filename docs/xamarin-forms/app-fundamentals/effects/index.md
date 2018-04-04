---
title: 효과
description: Xamarin.Forms 사용자 인터페이스는 각 플랫폼에 대 한 적절 한 모양 및 느낌을 유지 하려면 Xamarin.Forms 응용 프로그램의 대상 플랫폼에 네이티브 컨트롤을 사용 하 여 렌더링 됩니다. 효과를 사용자 지정 렌더러 구현에 의존 하지 않고도 사용자 지정할 수는 각 플랫폼에서 네이티브 컨트롤을 사용할 수 있습니다.
ms.prod: xamarin
ms.assetid: 8AF168A7-4CD9-4603-B961-15B8B1543784
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: 63d7750294321a28dbde833f72fe6b7277ada397
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="effects"></a>효과

_Xamarin.Forms 사용자 인터페이스는 각 플랫폼에 대 한 적절 한 모양 및 느낌을 유지 하려면 Xamarin.Forms 응용 프로그램의 대상 플랫폼에 네이티브 컨트롤을 사용 하 여 렌더링 됩니다. 효과를 사용자 지정 렌더러 구현에 의존 하지 않고도 사용자 지정할 수는 각 플랫폼에서 네이티브 컨트롤을 사용할 수 있습니다._

## <a name="introduction-to-effectsintroductionmd"></a>[효과 소개](introduction.md)

효과를 사용자 지정할 수는 각 플랫폼에서 네이티브 컨트롤을 사용할 수 있으며 작은 스타일 변경 내용에 일반적으로 사용 됩니다. 이 문서에서는 효과를 소개 효과 사용자 지정 렌더러 사이의 경계에 간략하게 설명 하 고 설명의 `PlatformEffect` 클래스입니다.

## <a name="creating-an-effectcreatingmd"></a>[효과 만들기](creating.md)

효과 컨트롤의 사용자 지정을 간소화합니다. 이 문서에서는의 배경색을 변경 하는 효과를 만드는 방법을 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 컨트롤이 포커스를 획득 하는 시기를 제어 합니다.

## <a name="passing-parameters-to-an-effectpassing-parametersindexmd"></a>[효과에 매개 변수 전달](passing-parameters/index.md)

매개 변수를 통해 구성 된 효과 만들면 효과를를 다시 사용할 수 있습니다. 이러한 문서 설명 속성을 사용 하 여 매개 변수는 효과를 전달 하 고 런타임에 매개 변수를 변경 합니다.

## <a name="invoking-events-from-an-effecttouch-trackingmd"></a>[효과의 이벤트를 호출합니다.](touch-tracking.md)

효과 이벤트를 호출할 수 있습니다. 이 문서에는 하위 수준 멀티 터치 손가락 추적을 구현 하는 신호를 누르면 터치, 이전, 및 버전에 대 한 응용 프로그램 이벤트를 만드는 방법을 보여 줍니다.