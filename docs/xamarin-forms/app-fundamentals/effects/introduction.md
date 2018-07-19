---
title: 효과 소개
description: 효과를 사용자 지정할 수는 각 플랫폼에서 네이티브 컨트롤을 허용 small 스타일 변경에 대 한 일반적으로 사용 됩니다. 이 문서에서는 효과에 대해 소개 효과 및 사용자 지정 렌더러 간의 경계를 간략하게 설명 하며 PlatformEffect 클래스를 설명 합니다.
ms.prod: xamarin
ms.assetid: 30CB8615-8F39-4762-BDB7-333D2B57D112
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: d3fa958e999a10832d5fa15e4190077955b0e6df
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997383"
---
# <a name="introduction-to-effects"></a>효과 소개

_효과를 사용자 지정할 수는 각 플랫폼에서 네이티브 컨트롤을 허용 small 스타일 변경에 대 한 일반적으로 사용 됩니다. 이 문서에서는 효과에 대해 소개 효과 및 사용자 지정 렌더러 간의 경계를 간략하게 설명 하며 PlatformEffect 클래스를 설명 합니다._

Xamarin.Forms [페이지, 레이아웃 및 컨트롤](~/xamarin-forms/user-interface/controls/index.md) 플랫폼 간 모바일 사용자 인터페이스를 설명 하는 공용 API를 제공 합니다. 각 페이지, 레이아웃 및 컨트롤 사용 하 여 각 플랫폼에서 다르게 렌더링 됩니다는 `Renderer` 클래스 (표현에 해당 하는 Xamarin.Forms), 네이티브 컨트롤을 다시 만드는 화면에서 정렬 및 공유에 지정 된 동작을 추가 코드입니다.

개발자는 컨트롤의 모양 및/또는 동작을 사용자 지정하기 위해 자신 만의 사용자 지정 `Renderer` 클래스를 구현할 수 있습니다. 그러나 간단한 컨트롤 사용자 지정 하는 데 사용자 지정 렌더러 클래스를 구현 경우가 중량 응답 합니다. 효과를 보다 쉽게 사용자 지정할 수는 각 플랫폼에서 네이티브 컨트롤을 허용 하는이 프로세스를 간소화 합니다.

효과 서브클래싱하 여 플랫폼별 프로젝트에 생성 됩니다는 `PlatformEffect` 제어 및 효과 Xamarin.Forms.NET Standard 라이브러리 또는 라이브러리 공유 프로젝트에 적절 한 컨트롤에 연결 하 여 사용 됩니다.

## <a name="why-use-an-effect-over-a-custom-renderer"></a>사용자 지정 렌더러를 통해 효과 사용 해야 하는 이유

효과는 컨트롤의 사용자 지정을 간소화, 다시 사용할 수는 및 재사용을 더욱 높이기 위해 매개 변수화 할 수 있습니다.

효과 얻을 수 있는 모든 사용자 지정 렌더러를 사용 하 여도 수행할 수 있습니다. 그러나 사용자 지정 렌더러는 더 많은 유연성과 효과 보다 사용자 지정을 제공합니다. 다음 지침을 사용자 지정 렌더러를 통해 효과 선택 하는 상황을 나열 합니다.

- 원하는 결과 달성 플랫폼 특정 컨트롤의 속성을 변경 하는 경우에 영향을 줄 것이 좋습니다.
- 사용자 지정 렌더러를 플랫폼 관련 컨트롤의 메서드를 재정의 해야 할 때 필요 합니다.
- 사용자 지정 렌더러를 Xamarin.Forms 컨트롤을 구현 하는 플랫폼 특정 컨트롤을 대체 해야 하는 경우 필요 합니다.

## <a name="subclassing-the-platformeffect-class"></a>PlatformEffect 클래스 서브클래싱

다음 표에서 대 한 네임 스페이스는 `PlatformEffect` 각 플랫폼에 해당 속성의 형식은 클래스:

|플랫폼|네임스페이스|컨테이너|Control|
|--- |--- |--- |--- |
|iOS|Xamarin.Forms.Platform.iOS|UIView|UIView|
|Android|Xamarin.Forms.Platform.Android|ViewGroup|보기|
|UWP(유니버설 Windows 플랫폼)|Xamarin.Forms.Platform.UWP|FrameworkElement|FrameworkElement|

각 플랫폼별 `PlatformEffect` 클래스는 다음 속성을 노출 합니다.

- `Container` -레이아웃을 구현 하는 데 사용 되는 플랫폼 특정 컨트롤을 참조 합니다.
- `Control` – Xamarin.Forms 컨트롤을 구현 하는 데 사용 되는 플랫폼 특정 컨트롤을 참조 합니다.
- `Element` – Xamarin.Forms 컨트롤을 렌더링 되는 참조 합니다.

효과 컨테이너, 컨트롤 또는 요소에 연결할 수 있으므로 연결 된 요소에 대 한 형식 정보를 사용할 필요가 없습니다. 따라서 효과 지원 하지 않는 요소에 연결 되 면 해야 정상적으로 성능 저하 또는 예외를 throw 합니다. 그러나 합니다 `Container`, `Control`, 및 `Element` 속성을 구현 하는 형식으로 캐스팅 될 수 있습니다. 이 대 한 자세한 내용은 참조 형식에 대 한 [렌더러 기본 클래스 및 네이티브 컨트롤](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)합니다.

각 플랫폼별 `PlatformEffect` 클래스 효과 구현 하려면 재정의 해야 하는 다음 메서드를 노출 합니다.

- [`OnAttached`](xref:Xamarin.Forms.Effect.OnAttached) – Xamarin.Forms 컨트롤에 영향을 줄 연결 될 때 호출 됩니다. 이 메서드의 각 플랫폼 특정 효과 클래스에서 재정의 된 버전을 사용 하면 지정된 된 Xamarin.Forms 컨트롤에 효과 적용할 수 없습니다 경우 예외 처리와 함께 컨트롤의 사용자 지정 하는 데 좋습니다.
- [`OnDetached`](xref:Xamarin.Forms.Effect.OnDetached) – Xamarin.Forms 컨트롤에서 영향을 줄 분리 되 면 호출 됩니다. 이 메서드의 각 플랫폼별 결과 클래스에서 재정의 된 버전은 이벤트 처리기를 등록 취소와 같은 효과 정리 작업을 수행 하는 위치입니다.

또한 합니다 `PlatformEffect` 노출 합니다 [ `OnElementPropertyChanged` ](xref:Xamarin.Forms.PlatformEffect`2.OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs)) 메서드를 재정의할 수도 있습니다. 이 메서드는 요소의 속성이 변경 되 면 호출 됩니다. 이 메서드의 각 플랫폼별 결과 클래스에서 재정의 된 버전은 Xamarin.Forms 컨트롤에 바인딩 가능한 속성 변경 내용에 응답할 수 있는 곳입니다. 변경 되는 속성에 대 한 검사를 항상 수 있어야를 따라이 재정의 여러 번 호출할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
