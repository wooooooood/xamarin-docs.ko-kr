---
title: Xamarin.Forms의 애니메이션
description: Xamarin.Forms를 쉬우면서도 컬 복잡 한 애니메이션을 만드는 간단한 애니메이션을 만들기 위한 간단한 애니메이션 인프라 자체를 포함 합니다.
ms.prod: xamarin
ms.assetid: AC0B4127-ECA3-44DA-8A24-A2B10A275083
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: bebb3e32f298a2079069787dba3453e1817cf64f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998307"
---
# <a name="animation-in-xamarinforms"></a>Xamarin.Forms의 애니메이션

_Xamarin.Forms를 쉬우면서도 컬 복잡 한 애니메이션을 만드는 간단한 애니메이션을 만들기 위한 간단한 애니메이션 인프라 자체를 포함 합니다._

Xamarin.Forms 애니메이션 클래스를 점진적으로 속성을 변경 하면 한 값에서 다른 시간 동안의 일반적인 애니메이션을 사용 하 여 시각적 요소의 다른 속성을 대상으로 합니다. Xamarin.Forms 애니메이션 클래스에 대 한 XAML 인터페이스가 없는 참고 합니다. 그러나 애니메이션 캡슐화 될 수 있습니다 [동작](~/xamarin-forms/app-fundamentals/behaviors/index.md) 다음 XAML에서 참조 합니다.

## <a name="simple-animationssimplemd"></a>[단순 애니메이션](simple.md)

합니다 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) 회전, 크기 조정, 변환 및 페이드 인 되는 간단한 애니메이션을 만드는 데 사용할 수 있는 확장 메서드를 제공 하는 클래스 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) 인스턴스. 이 문서에서는 만들고 사용 하 여 애니메이션을 취소 합니다 `ViewExtensions` 클래스입니다.

## <a name="easing-functionseasingmd"></a>[감속/가속 함수](easing.md)

Xamarin.Forms에는 [ `Easing` ](xref:Xamarin.Forms.Easing) 애니메이션 속도를 향상 하는 방법을 제어 하는 전송 함수를 지정 하거나 실행 중인으로 느려질 수 있는 클래스입니다. 이 문서에는 미리 정의 된 감속/가속 함수를 사용 하는 방법 및 사용자 지정 감속/가속 함수를 만드는 방법을 보여 줍니다.

## <a name="custom-animationscustommd"></a>[사용자 지정 애니메이션](custom.md)

[ `Animation` ](xref:Xamarin.Forms.Animation) 클래스의 확장 메서드를 사용 하 여 모든 Xamarin.Forms 애니메이션의 기본 구성 요소는는 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) 클래스를 하나 이상 만드는 `Animation` 개체입니다. 이 문서를 사용 하는 방법에 설명 합니다 `Animation` 및 애니메이션을 취소, 여러 애니메이션 동기화 만들고 속성은 기존 애니메이션 방법을 사용 하 여 애니메이션을 애니메이션을 적용 하는 사용자 지정 애니메이션 클래스입니다.
