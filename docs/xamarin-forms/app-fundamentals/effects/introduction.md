---
title: "효과 소개"
description: "효과를 사용자 지정할 수는 각 플랫폼에서 네이티브 컨트롤을 사용할 수 있으며 작은 스타일 변경 내용에 일반적으로 사용 됩니다. 이 문서에서는 효과를 소개 하 고 효과 사용자 지정 렌더러 사이의 경계에 간략하게 설명 PlatformEffect 클래스를 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 30CB8615-8F39-4762-BDB7-333D2B57D112
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 037c82aa31c167e44a88619cba91a5be8035d0fa
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
---
# <a name="introduction-to-effects"></a>효과 소개

_효과를 사용자 지정할 수는 각 플랫폼에서 네이티브 컨트롤을 사용할 수 있으며 작은 스타일 변경 내용에 일반적으로 사용 됩니다. 이 문서에서는 효과를 소개 하 고 효과 사용자 지정 렌더러 사이의 경계에 간략하게 설명 PlatformEffect 클래스를 설명 합니다._

Xamarin.Forms [페이지, 레이아웃 및 컨트롤](~/xamarin-forms/user-interface/controls/index.md) 플랫폼 간 모바일 사용자 인터페이스를 설명 하기 위해 공용 API를 제공 합니다. 각 페이지, 레이아웃 및 컨트롤 사용 하 여 각 플랫폼에서 다르게 렌더링 되는 `Renderer` 차례로 (Xamarin.Forms 표현에 해당), 네이티브 컨트롤을 만드는 클래스를 화면에 정렬 하 고이 공유에 지정 된 동작을 추가 합니다. 코드입니다.

개발자는 컨트롤의 모양 및/또는 동작을 사용자 지정하기 위해 자신 만의 사용자 지정 `Renderer` 클래스를 구현할 수 있습니다. 그러나 단순 컨트롤 사용자 지정을 수행 하는 사용자 지정 렌더러 클래스를 구현 방식은 규모 응답 합니다. 효과를 보다 쉽게 사용자 지정할 수는 각 플랫폼에서 네이티브 컨트롤 수 있도록이 프로세스를 간소화 합니다.

효과 서브클래싱하 여 플랫폼별 프로젝트에 생성 됩니다는 `PlatformEffect` 제어 및 효과 적절 한 컨트롤 Xamarin.Forms PCL 이식 가능한 클래스 라이브러리 () 또는 라이브러리 공유 프로젝트에 연결 하 여 사용 합니다.

## <a name="why-use-an-effect-over-a-custom-renderer"></a>사용자 지정 렌더러를 통해 비슷한 효과 사용 해야 하는 이유

효과 컨트롤의 사용자 지정을 단순화 하 고 다시 사용할 수는 하십시오 재사용을 더욱 높일 수 매개 변수화 할 수 있습니다.

효과로 얻을 수 있는 모든 사용자 지정 렌더러로 구현할 수도 있습니다. 그러나 사용자 지정 렌더러 더 많은 유연성 및 사용자 지정 효과 보다를 제공 합니다. 다음 지침에는 사용자 지정 렌더러를 통해 비슷한 효과 선택할 수 있는 상황을 나열 합니다.

- 플랫폼 관련 컨트롤의 속성을 변경 하면 원하는 결과가 달성할 시기 효과 것이 좋습니다.
- 사용자 지정 렌더러는 플랫폼 특정 컨트롤의 메서드를 재정의 해야 할 필요 합니다.
- 사용자 지정 렌더러가 Xamarin.Forms 컨트롤을 구현 하는 플랫폼 특정 컨트롤을 대체 해야 하는 경우에 필요 합니다.

## <a name="subclassing-the-platformeffect-class"></a>PlatformEffect 클래스 서브클래싱

다음 표에서에 대 한 네임 스페이스는 `PlatformEffect` 각 플랫폼에서 해당 속성의 종류 등에 클래스:

|플랫폼|네임스페이스|컨테이너|Control|
|--- |--- |--- |--- |
|iOS|Xamarin.Forms.Platform.iOS|UIView|UIView|
|Android|Xamarin.Forms.Platform.Android|ViewGroup|보기|
|Windows Phone 8.1|Xamarin.Forms.Platform.WinRT|FrameworkElement|FrameworkElement|
|UWP(유니버설 Windows 플랫폼)|Xamarin.Forms.Platform.UWP|FrameworkElement|FrameworkElement|

각 플랫폼 특정 `PlatformEffect` 클래스는 다음과 같은 속성을 노출 합니다.

- `Container` – 레이아웃을 구현 하는 데 사용 되 고 플랫폼 특정 컨트롤을 참조 합니다.
- `Control` – Xamarin.Forms 컨트롤을 구현 하는 데 사용 되는 플랫폼 특정 컨트롤을 참조 합니다.
- `Element` – Xamarin.Forms 컨트롤을 렌더링 되는 참조 합니다.

효과 컨테이너, 제어 또는 모든 요소에 연결 될 수 있으므로에 연결 된 요소에 대 한 형식 정보를 사용할 필요가 없습니다. 따라서 비슷한 효과 지원 하지 않는 요소에 연결 해야 정상적 저하 또는 예외를 throw 합니다. 그러나는 `Container`, `Control`, 및 `Element` 속성을 구현 하는 형식으로 캐스팅 될 수 있습니다. 이 대 한 자세한 내용은 참조 형식에 대 한 [렌더러 기본 클래스와 기본 컨트롤](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)합니다.

각 플랫폼 특정 `PlatformEffect` 클래스 효과 구현 하는 재정의 해야 합니다는 다음 메서드를 노출 합니다.

- [`OnAttached`](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.OnAttached()/) – 영향을 줍니다. Xamarin.Forms 컨트롤에 연결 될 때 호출 합니다. 이 메서드의 각 플랫폼 관련 효과 클래스에서 재정의 된 버전에 지정된 된 Xamarin.Forms 컨트롤에 효과 적용할 수 없습니다 경우 예외 처리와 함께 컨트롤의 사용자 지정을 수행할 위치입니다.
- [`OnDetached`](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.OnDetached()/) – 효과 Xamarin.Forms 컨트롤에서 분리 되 면 호출 됩니다. 이 메서드의 각 플랫폼 특정 효과 클래스에서 재정의 된 버전은 이벤트 처리기를 등록 취소와 같은 효과 정리 작업을 수행 하기에 적합 합니다.

또한는 `PlatformEffect` 노출 된 [ `OnElementPropertyChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E.OnElementPropertyChanged/p/System.ComponentModel.PropertyChangedEventArgs/) 메서드를 재정의할 수도 있습니다. 이 메서드는 요소의 속성이 변경 되 면 호출 됩니다. 이 메서드의 각 플랫폼 특정 효과 클래스에서 재정의 된 버전은 Xamarin.Forms 컨트롤에 바인딩 가능한 속성 변경에 대응할 수 있는 곳입니다. 변경 된 속성에 대 한 확인을 항상을 설정 해야으로이 재정의 여러 번 호출할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
