---
title: 동작 소개
description: 동작을 통해 하위 클래스에 하지 않고도 사용자 인터페이스 컨트롤에 기능을 추가할 수 있습니다. 대신 기능 동작 클래스에서 구현 되 고 컨트롤 자체의 일부 하는 경우 해당 컨트롤에 연결. 이 문서에서는 동작에 대 한 소개를 제공 합니다.
ms.prod: xamarin
ms.assetid: 0DF1EF8C-A212-4142-A3C6-DF760A82A757
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: b5aa0d3de7092ac87d511ab8d59c329471fa6a28
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-behaviors"></a>동작 소개

_동작을 통해 하위 클래스에 하지 않고도 사용자 인터페이스 컨트롤에 기능을 추가할 수 있습니다. 대신 기능 동작 클래스에서 구현 되 고 컨트롤 자체의 일부 하는 경우 해당 컨트롤에 연결. 이 문서에서는 동작에 대 한 소개를 제공 합니다._

동작을 사용 하는 간결 하 게 해당 컨트롤에 연결 하 고 수 둘 이상의 응용 프로그램 간에 다시 사용 하기 위해 패키지 하는 방식으로 컨트롤의 API와 직접 상호 작용 하기 때문에 코드 숨김 파일로 작성 해야 일반적으로 코드를 구현할 수 있습니다. 와 같은 다양 한 컨트롤에는 기능을 제공 데 사용할 수 있습니다.

- 전자 메일 유효성 검사기를 추가 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)합니다.
- Tap 제스처 인식기를 사용 하 여 등급 컨트롤을 만듭니다.
- 애니메이션을 제어 합니다.
- 컨트롤에 효과 추가 합니다.

동작 더 고급 시나리오에도 사용 하도록 설정 합니다. 컨텍스트에서 *명령 실행*, 승인 된 컨트롤에는 명령에 연결 하기 위한 유용한 접근 방법입니다. 또한 명령을와 상호 작용 하도록 설계 되지 않은 컨트롤 명령을 연관 시킬 사용 될 수 있습니다. 예를 들어 명령을 발생 시키는 이벤트에 대 한 응답으로 호출 하 사용할 수 있습니다.

Xamarin.Forms는 두 가지 다른 스타일의 동작을 지원합니다.

- **Xamarin.Forms 동작** –에서 파생 된 클래스는 [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) 또는 [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) 클래스, 여기서 `T` 는 컨트롤에 유형의 동작 적용 해야 합니다. Xamarin.Forms 동작에 대 한 자세한 내용은 참조 [Xamarin.Forms 동작](~/xamarin-forms/app-fundamentals/behaviors/creating.md) 및 [다시 사용할 수 있는 동작](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md)합니다.
- **동작의 첨부 된** – `static` 하나 이상의 연결 된 속성을 사용 하 여 클래스입니다. 연결 된 동작에 대 한 자세한 내용은 참조 [연결 된 동작](~/xamarin-forms/app-fundamentals/behaviors/attached.md)합니다.

이 가이드 동작 생성을 선호 되는 방법은 서로 하므로 Xamarin.Forms 동작을 설명 합니다.



## <a name="related-links"></a>관련 링크

- [동작](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [동작<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
