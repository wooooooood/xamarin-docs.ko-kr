---
title: 효과 소개
description: 효과를 사용하면 각 플랫폼의 네이티브 컨트롤을 사용자 지정할 수 있습니다. 효과는 일반적으로 작은 스타일 지정 변경에 사용됩니다. 이 문서에서는 효과에 대한 소개를 제공하고, 효과와 사용자 지정 렌더러 사이의 경계를 간략히 설명하고 PlatformEffect 클래스를 설명합니다.
ms.prod: xamarin
ms.assetid: 30CB8615-8F39-4762-BDB7-333D2B57D112
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: d3fa958e999a10832d5fa15e4190077955b0e6df
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997383"
---
# <a name="introduction-to-effects"></a>효과 소개

_효과를 사용하면 각 플랫폼의 네이티브 컨트롤을 사용자 지정할 수 있습니다. 효과는 일반적으로 작은 스타일 지정 변경에 사용됩니다. 이 문서에서는 효과에 대한 소개를 제공하고, 효과와 사용자 지정 렌더러 사이의 경계를 간략히 설명하고 PlatformEffect 클래스를 설명합니다._

Xamarin.Forms [페이지, 레이아웃 및 컨트롤](~/xamarin-forms/user-interface/controls/index.md)은 플랫폼 간 모바일 사용자 인터페이스를 설명하는 공용 API를 제공합니다. 각 페이지, 레이아웃 및 컨트롤은 차례로 네이티브 컨트롤을 만들고(Xamarin.Forms 표현에 해당), 화면에 정렬하고, 공유 코드에 지정한 동작을 추가하는 `Renderer` 클래스를 사용하여 각 플랫폼에서 다르게 렌더링됩니다.

개발자는 컨트롤의 모양 및/또는 동작을 사용자 지정하기 위해 자신 만의 사용자 지정 `Renderer` 클래스를 구현할 수 있습니다. 그러나 간단한 컨트롤 사용자 지정을 수행하는 사용자 지정 렌더러 클래스를 구현하는 것은 종종 중량 응답입니다. 효과는 각 플랫폼에서 네이티브 컨트롤을 보다 쉽게 사용자 지정할 수 있도록 하여 이 프로세스를 간소화합니다.

효과가 플랫폼별 프로젝트에 `PlatformEffect` 컨트롤을 서브클래싱하여 만들어진 다음, Xamarin.Forms .NET 표준 라이브러리 또는 공유 라이브러리 프로젝트의 적절한 컨트롤에 연결하여 사용됩니다.

## <a name="why-use-an-effect-over-a-custom-renderer"></a>사용자 지정 렌더러를 통해 효과를 사용하는 이유는 무엇인가요?

효과는 컨트롤의 사용자 지정을 간소화하고, 다시 사용할 수 있으며, 재사용을 더욱 높이기 위해 매개 변수화될 수 있습니다.

효과를 사용하여 얻을 수 있는 모든 것은 사용자 지정 렌더러를 사용하여도 얻을 수 있습니다. 그러나 사용자 지정 렌더러는 효과보다 더 많은 유연성과 사용자 지정을 제공합니다. 다음 지침은 사용자 지정 렌더러를 통해 효과를 선택하는 상황을 나열합니다.

- 플랫폼별 컨트롤의 속성을 변경하여 원하는 결과를 달성하는 경우 효과를 사용하는 것이 좋습니다.
- 사용자 지정 렌더러는 플랫폼별 컨트롤의 메서드를 재정의해야 하는 경우에 필요합니다.
- 사용자 지정 렌더러는 Xamarin.Forms 컨트롤을 구현하는 플랫폼별 컨트롤을 대체해야 하는 경우에 필요합니다.

## <a name="subclassing-the-platformeffect-class"></a>PlatformEffect 클래스 서브클래싱

다음 표는 각 플랫폼의 `PlatformEffect` 클래스에 대한 네임스페이스 및 해당 속성의 형식을 나열합니다.

|플랫폼|네임스페이스|컨테이너|Control|
|--- |--- |--- |--- |
|iOS|Xamarin.Forms.Platform.iOS|UIView|UIView|
|Android|Xamarin.Forms.Platform.Android|ViewGroup|보기|
|UWP(유니버설 Windows 플랫폼)|Xamarin.Forms.Platform.UWP|FrameworkElement|FrameworkElement|

각 플랫폼별 `PlatformEffect` 클래스는 다음 속성을 노출합니다.

- `Container` - 레이아웃을 구현하는 데 사용되는 플랫폼별 컨트롤을 참조합니다.
- `Control` - Xamarin.Forms 컨트롤을 구현하는 데 사용되는 플랫폼별 컨트롤을 참조합니다.
- `Element` – 렌더링되는 Xamarin.Forms 컨트롤을 참조합니다.

효과는 모든 요소에 연결할 수 있으므로 연결된 컨테이너, 컨트롤 또는 요소에 대한 형식 정보가 없습니다. 따라서 효과가 지원하지 않는 요소에 연결된 경우 정상적으로 성능을 저하하거나 예외를 throw해야 합니다. 그러나 `Container`, `Control` 및 `Element` 속성을 해당 구현 형식으로 캐스팅할 수 있습니다. 이러한 형식에 대한 자세한 내용은 [렌더러 기본 클래스 및 네이티브 컨트롤](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)을 참조하세요.

각 플랫폼별 `PlatformEffect` 클래스는 효과를 구현하도록 재정의되어야 하는 다음 메서드를 노출합니다.

- [`OnAttached`](xref:Xamarin.Forms.Effect.OnAttached) – 효과가 Xamarin.Forms 컨트롤에 연결될 때 호출됩니다. 각 플랫폼별 효과 클래스에서 이 메서드의 재정의된 버전은 지정된 Xamarin.Forms 컨트롤에 효과를 적용할 수 없는 경우의 예외 처리와 함께 컨트롤의 사용자 지정을 수행하는 위치입니다.
- [`OnDetached`](xref:Xamarin.Forms.Effect.OnDetached) – 효과가 Xamarin.Forms 컨트롤에서 분리될 때 호출됩니다. 각 플랫폼별 효과 클래스에서 이 메서드의 재정의된 버전은 이벤트 처리기 등록 취소와 같은 효과 정리를 수행하는 위치입니다.

또한 `PlatformEffect`는 재정의될 수 있는 [`OnElementPropertyChanged`](xref:Xamarin.Forms.PlatformEffect`2.OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs)) 메서드를 노출합니다. 요소의 속성이 변경될 때 이 메서드가 호출됩니다. 각 플랫폼별 효과 클래스에서 이 메서드의 재정의된 버전은 Xamarin.Forms 컨트롤의 바인딩 가능한 속성 변경 내용에 응답하는 위치입니다. 이 재정의는 여러 번 호출될 수 있으므로 변경되는 속성에 대한 검사는 항상 수행되어야 합니다.


## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
