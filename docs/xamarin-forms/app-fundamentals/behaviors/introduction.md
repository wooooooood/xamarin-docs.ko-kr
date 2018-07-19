---
title: 동작 소개
description: 동작을 통해 서브 클래스 하 하지 않고도 사용자 인터페이스 컨트롤에 기능을 추가할 수 있습니다. 대신 기능 동작 클래스에서 구현 되 고 컨트롤 자체의 일부 였던 것 처럼 컨트롤에 연결 합니다. 이 문서에서는 동작에 소개 합니다.
ms.prod: xamarin
ms.assetid: 0DF1EF8C-A212-4142-A3C6-DF760A82A757
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 176f41d4b7349af2cf7cc49de8ba0789ad2f8c11
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995816"
---
# <a name="introduction-to-behaviors"></a>동작 소개

_동작을 통해 서브 클래스 하 하지 않고도 사용자 인터페이스 컨트롤에 기능을 추가할 수 있습니다. 대신 기능 동작 클래스에서 구현 되 고 컨트롤 자체의 일부 였던 것 처럼 컨트롤에 연결 합니다. 이 문서에서는 동작에 소개 합니다._

동작을 사용 해야 하는 일반적으로 코드 숨김 파일로 작성 하 고 수 있습니다 간결 하 게 컨트롤에 연결 된 둘 이상의 응용 프로그램에서 다시 사용할 수 있도록 패키지 하는 방식으로 컨트롤의 API와 직접 상호 작용 하기 때문에 코드를 구현할 수 있습니다. 와 같은 광범위 한 컨트롤에 기능을 제공 하는 사용할 수 있습니다.

- 전자 메일 유효성 검사기 추가 [ `Entry` ](xref:Xamarin.Forms.Entry)합니다.
- 탭 제스처 인식기를 사용 하 여 등급 컨트롤을 만듭니다.
- 애니메이션을 제어 합니다.
- 컨트롤에 영향을 줄을 추가 합니다.

동작 보다 고급 시나리오에도 사용 하도록 설정 합니다. 컨텍스트에서 *명령*, 승인 된 명령에 컨트롤을 연결 하는 데 유용한 접근 방식입니다. 또한 명령을 사용 하 여 상호 작용 하도록 설계 되지 않았습니다 하는 컨트롤을 사용 하 여 명령을 연결 사용 수 수 있습니다. 예를 들어, 발생 하는 이벤트에 대 한 응답에서 명령을 호출 하 사용할 수 있습니다.

Xamarin.Forms 동작의 두 가지 다른 스타일을 지원합니다.

- **Xamarin.Forms 동작** –에서 파생 된 클래스를 [ `Behavior` ](xref:Xamarin.Forms.Behavior) 또는 [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) 클래스는 `T` 는 컨트롤의 형식인 동작 적용 해야 합니다. Xamarin.Forms 동작에 대 한 자세한 내용은 참조 하세요. [Xamarin.Forms 동작](~/xamarin-forms/app-fundamentals/behaviors/creating.md) 하 고 [재사용 가능한 동작](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md)합니다.
- **동작을 연결** – `static` 하나 이상의 연결 된 속성을 사용 하 여 클래스입니다. 연결 된 동작에 대 한 자세한 내용은 참조 하세요. [연결 된 동작](~/xamarin-forms/app-fundamentals/behaviors/attached.md)합니다.

이 가이드는 기본 동작이 생성 방법 이기 때문에 Xamarin.Forms 동작에 집중 합니다.



## <a name="related-links"></a>관련 링크

- [동작](xref:Xamarin.Forms.Behavior)
- [동작&lt;T&gt;](xref:Xamarin.Forms.Behavior`1)
