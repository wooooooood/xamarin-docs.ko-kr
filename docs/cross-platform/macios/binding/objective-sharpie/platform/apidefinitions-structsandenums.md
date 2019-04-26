---
title: ApiDefinitions 및 StructsAndEnums 파일
description: 이 문서에서는 발생 하는 목표 Sharpie ApiDefinitions.cs 및 StructsAndEnums.cs 파일을 설명 합니다. 이러한 파일은 다음에서 Objective-c 코드에 액세스 하는 데 사용 됩니다 C#입니다.
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: df8d4508db14116a5b36e893f161ac891d58dc46
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61266365"
---
# <a name="apidefinitions--structsandenums-files"></a>ApiDefinitions 및 StructsAndEnums 파일

생성 목표 Sharpie 성공적으로 실행 되 면 `Binding/ApiDefinitions.cs` 고 `Binding/StructsAndEnums.cs` 파일입니다.
이러한 두 파일을 Mac 용 Visual Studio에서 바인딩 프로젝트에 추가 하거나에 직접 전달 된 `btouch` 또는 `bmac` 최종 바인딩을 생성 하는 도구입니다.

하지만 *일부* 경우 생성 된 파일 (예: 해당 플래그가 지정 된 도구를 통해 자동으로 처리할 수 없는 하는 모든 문제를 해결 하는 파일을 생성 더 자주 개발자 이러한를 수동으로 수정 해야 합니다. 필요한 모든 수 있습니다 사용 하 여는 [ `Verify` 특성](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)).

다음 단계 중 일부는 다음과 같습니다.

- **이름을 조정**: 메서드 및.NET Framework 디자인 지침을 일치 하는 클래스의 이름을 조정할 수도 있습니다.
- **메서드 또는 속성**: 때로는 목표 Sharpie에서 사용 되는 추론을 속성으로 설정 하는 메서드를 선택 합니다. 이 시점에서 의도 된 동작 인지 여부를 결정할 수 있습니다.
- **이벤트 후크**: 대리자 클래스를 사용 하 여 클래스에 연결 하 고 해당 이벤트를 자동으로 생성할 수 없습니다.
- **알림 후크**: 알림 API 계약 순수 헤더 파일에서 추출할 수 없는, API 설명서를 새로 구입 해야 합니다. 강력한 형식의 알림을 원하는 경우에 결과 업데이트 해야 합니다.
- **API 큐 레이 션**: 이 시점에서 추가 생성자를 제공 하 여 메서드를 추가를 선택할 수 있습니다 (허용 하기 위해 C# 생성에 초기화 구문을), 연산자 오버 로드 하 고 추가 정의 파일에 고유한 인터페이스를 구현 합니다.

참조를 [API를 바인딩](~/cross-platform/macios/binding/objective-c-libraries.md) 설명 아래 다이어그램에 표시 된 대로 이러한 파일에 바인딩 프로세스를 어떻게 확인 하려면:

![](apidefinitions-structsandenums-images/binding-flowchart.png "바인딩 프로세스는이 다이어그램에 표시 됩니다.")

참조 된 [바인딩 유형 참조](~/cross-platform/macios/binding/binding-types-reference.md) 이러한 파일의 내용에 대 한 자세한 내용은 합니다.

## <a name="related-links"></a>관련 링크

- [Xamarin University 과정: Objective-c 바인딩 라이브러리를 빌드](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University 과정: 목표 Sharpie로는 Objective-c 바인딩 라이브러리 빌드](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
