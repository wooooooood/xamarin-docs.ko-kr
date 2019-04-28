---
title: Xamarin.Forms의 애니메이션
description: Xamarin.Forms는 복잡한 애니메이션을 만들 수 있을 정도로 다양하면서도, 간단한 애니메이션을 직관적으로 만들 수 있는 자체 애니메이션 체계를 가지고 있습니다.
ms.prod: xamarin
ms.assetid: AC0B4127-ECA3-44DA-8A24-A2B10A275083
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: bebb3e32f298a2079069787dba3453e1817cf64f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61158604"
---
# <a name="animation-in-xamarinforms"></a>Xamarin.Forms의 애니메이션

_Xamarin.Forms는 복잡한 애니메이션을 만들 수 있을 정도로 다양하면서도, 간단한 애니메이션을 직관적으로 만들 수 있는 자체 애니메이션 체계를 가지고 있습니다._

Xamarin.Forms 애니메이션 클래스는 시각 요소들(Visual Elments)의 여러 속성들을 대상으로 하며, 일반적인 애니메이션은 일정 기간 동안 한 속성의 값을 다른 값으로 점진적으로 변경합니다. Xamarin.Forms 애니메이션 클래스에는 XAML 인터페이스가 없다는 점은 참고해야 합니다. 그렇지만 애니메이션은 [동작](~/xamarin-forms/app-fundamentals/behaviors/index.md)을 통해 캡슐화한 후 XAML에서 참조할 수 있습니다.

## <a name="simple-animationssimplemd"></a>[간단한 애니메이션](simple.md)

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 클래스를 사용해 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 인스턴스를 회전, 크기 조정, 변환 및 페이드 인하는 간단한 애니메이션을 만들 수 있습니다. 이 문서는 `ViewExtensions` 클래스를 이용해 애니메이션을 만들고 취소하는 방법을 보여줍니다.

## <a name="easing-functionseasingmd"></a>[감속/가속 함수](easing.md)

Xamarin.Forms는 애니메이션이 동작하고 있을 때 [`Easing`](xref:Xamarin.Forms.Easing) 클래스를 통해 어떻게 애니메이션 속도를 높이고 낮추는 지를 제어하는 변환 함수를 정할 수 있습니다. 이 문서에서는 미리 정의된 감속/가속 함수를 사용하는 방법과 사용자 지정 감속/가속 함수를 만드는 방법을 알아봅니다.

## <a name="custom-animationscustommd"></a>[사용자 지정 애니메이션](custom.md)

[`Animation`](xref:Xamarin.Forms.Animation) 클래스로 하나 혹은 그 이상의 애니메이션을 만드는 확장메서드를 사용할 수 있는 [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)를 통해 Xamarin.Forms의 모든 애니메이션들의 블록을 만들 수 있습니다. 이 문서는 `Animation` 클래스를 사용해 어떻게 애니메이션을 만들고 취소하는지와 여러 애니메이션을 동기화하는 방법 그리고 기존 애니메이션 메서드로 애니메이션 적용할 수 없는 속성을 애니메이션 적용하는 사용자 지정 애니메이션을 만드는 방법에 대해 살펴봅니다.
