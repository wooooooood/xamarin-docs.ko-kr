---
title: "ApiDefinitions & StructsAndEnums 파일"
ms.topic: article
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 0dedc6f574fbe2f2bf4ffaf4e70fb972670e9a8c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="apidefinitions--structsandenums-files"></a>ApiDefinitions & StructsAndEnums 파일

목표 Sharpie에 성공적으로 실행 하는 경우에 생성 `Binding/ApiDefinitions.cs` 및 `Binding/StructsAndEnums.cs` 파일입니다.
이러한 두 파일을 Mac 용 Visual Studio에서 바인딩 프로젝트에 추가 하거나으로 직접 전달는 `btouch` 또는 `bmac` 최종 바인딩을 생성 하는 도구입니다.

*일부* 경우 생성 된 파일 생성 된 더 자주 개발자 이러한 항목을 수동으로 수정 해야 합니다 (예: 해당 플래그가 지정 된 도구에 의해 자동으로 처리할 수 없는 모든 문제를 해결 하는 파일 있지만 필요한 것 수 있습니다 와 [ `Verify` 특성](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)).

다음 단계는 다음과 같습니다.

- **이름은 조정**: 메서드 및.NET Framework 디자인 지침과 일치 하는 클래스의 이름을 조정 해야 할 경우도 있습니다.
- **메서드 또는 속성**: 목표 Sharpie 여 경우에 따라 사용 되는 추론 속성으로 설정 하는 방법을 선택 합니다. 이 시점에서 의도 된 동작 인지 여부를 결정할 수 있습니다.
- **이벤트 후크**: 대리자 클래스와 클래스에 연결 하 고 자동으로 하는 것에 대 한 이벤트를 생성할 수 없습니다.
- **알림 후크**: 알림 API 계약 순수 헤더 파일에서 추출할 수 없으면,이 API 설명서에 이동 해야 합니다. 강력한 형식의 알림을 하려는 경우에 결과 업데이트 해야 합니다.
- **API 큐 레이 션**: 추가 생성자를 제공, (C# 초기화에 생성 구문에 대 한 허용) 하는 메서드, 연산자 오버 로드 및 구현 추가 하도록 선택할 수 있습니다이 시점에서 추가 정의 파일에 직접 인터페이스입니다.

참조는 [API 바인딩](~/cross-platform/macios/binding/objective-c-libraries.md) 설명 및 아래 다이어그램에 나와 있는 것 처럼 바인딩 프로세스에 이러한 파일을 배치 하는 방법을 참조 합니다.

![](apidefinitions-structsandenums-images/binding-flowchart.png "바인딩 프로세스는이 다이어그램에 나와")

참조는 [바인딩 형식 참조](~/cross-platform/macios/binding/binding-types-reference.md) 이러한 파일의 내용에 대 한 자세한 내용은 합니다.

