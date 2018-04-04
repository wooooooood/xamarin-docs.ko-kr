---
title: 애니메이션
description: Xamarin.Forms에는 간단 하면서도 용도가 넓은 함수로 충분히 복잡 한 애니메이션을 만드는 간단한 애니메이션을 만들기 위한 자체 애니메이션 인프라에 포함 됩니다.
ms.prod: xamarin
ms.assetid: AC0B4127-ECA3-44DA-8A24-A2B10A275083
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 7cff122e7ecc24f5ad93bd863ee422981871f857
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="animation"></a>애니메이션

_Xamarin.Forms에는 간단 하면서도 용도가 넓은 함수로 충분히 복잡 한 애니메이션을 만드는 간단한 애니메이션을 만들기 위한 자체 애니메이션 인프라에 포함 됩니다._

Xamarin.Forms 애니메이션 클래스 점진적으로 속성을 변경 하면 한 값에서 다른 시간 동안 일반적인 애니메이션으로 시각적 요소의 다른 속성을 대상입니다. Xamarin.Forms 애니메이션 클래스에 대 한 XAML 인터페이스가 없는 있다는 것을 참고 합니다. 하지만 애니메이션에 캡슐화 할 수 있습니다 [동작](~/xamarin-forms/app-fundamentals/behaviors/index.md) 와 후 XAML에서 참조 합니다.

## <a name="simple-animationssimplemd"></a>[단순 애니메이션](simple.md)

[ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) 간단한 애니메이션 회전, 크기 조정, 변환 및 페이드을 생성 하는 데 사용할 수 있는 확장 메서드를 제공 하는 클래스 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) 인스턴스. 이 문서에서는 만들기 및 사용 하 여 애니메이션 취소는 `ViewExtensions` 클래스입니다.

## <a name="easing-functionseasingmd"></a>[감속/가속 함수](easing.md)

Xamarin.Forms에 포함 되어는 [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) 클래스 애니메이션 속도를 향상 하는 방법을 제어 하는 전송 함수를 지정 하거나 실행 하는 대로 속도가 느려질 수 있습니다. 이 문서를 사용자 지정 감속/가속 함수를 만드는 방법과 미리 정의 된 감속/가속 함수를 사용 하는 방법을 보여줍니다.

## <a name="custom-animationscustommd"></a>[사용자 지정 애니메이션](custom.md)

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) 클래스는의 확장 방법 사용 하 여 모든 Xamarin.Forms 애니메이션의 빌딩 블록의 [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) 하나 이상의 클래스 `Animation` 개체입니다. 이 문서에서는 방법을 사용 하는 `Animation` 을 생성 및 애니메이션 취소, 여러 애니메이션 동기화 속성은 기존 애니메이션 메서드에서 애니메이션을 효과 적용 하는 사용자 지정 애니메이션을 만드는 클래스입니다.

