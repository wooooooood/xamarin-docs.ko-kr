---
title: ApiDefinitions 파일 &
description: 이 문서에서는 Sharpie가 생성 하는 ApiDefinitions.cs 및 StructsAndEnums.cs 파일에 대해 설명 합니다. 그런 다음 이러한 파일을 사용 하 여에서 C#목표-C 코드에 액세스 합니다.
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 950f9149744cb8aa2abaed60ccefb416405ab110
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290787"
---
# <a name="apidefinitions--structsandenums-files"></a>ApiDefinitions 파일 &

목표 Sharpie 성공적으로 실행 되 면 및 `Binding/ApiDefinitions.cs` `Binding/StructsAndEnums.cs` 파일을 생성 합니다.
이러한 두 파일은 Mac용 Visual Studio의 바인딩 프로젝트에 추가 되거나 `btouch` 또는 `bmac` 도구에 직접 전달 되어 최종 바인딩을 생성 합니다.

경우 *에 따라 이러한* 생성 된 파일이 필요할 수도 있지만 개발자가 도구에서 자동으로 처리할 수 없는 문제를 해결 하기 위해 이러한 생성 된 파일을 수동으로 수정 해야 하는 경우가 있습니다 (예: [ `Verify`특성](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)).

다음 단계 중 일부는 다음과 같습니다.

- **이름 조정**: 경우에 따라 .NET Framework 디자인 지침과 일치 하도록 메서드 및 클래스의 이름을 조정 해야 합니다.
- **메서드 또는 속성**: 목표 Sharpie에서 사용 하는 추론은 종종 속성으로 설정 될 메서드를 선택 합니다. 이 시점에서 의도 된 동작 인지 여부를 결정할 수 있습니다.
- **후크 이벤트**: 클래스를 대리자 클래스와 연결 하 고 해당 이벤트에 대 한 이벤트를 자동으로 생성할 수 있습니다.
- **알림 후크**: 순수 헤더 파일에서 알림의 API 계약을 추출할 수는 없으므로 API 설명서로 이동 해야 합니다. 강력한 형식의 알림이 필요한 경우 결과를 업데이트 해야 합니다.
- **API Curation**: 이 시점에서 추가 생성자를 제공 하 고, 메서드를 추가 하 고 (초기화 생성 C# 구문에 대해 허용 하기 위해), 연산자 오버 로드를 선택 하 고, 추가 정의 파일에서 고유한 인터페이스를 구현할 수 있습니다.

아래 다이어그램에 표시 된 것 처럼 이러한 파일이 바인딩 프로세스에 얼마나 적합 한지 확인 하려면 [API 바인딩](~/cross-platform/macios/binding/objective-c-libraries.md) 설명을 참조 하세요.

![](apidefinitions-structsandenums-images/binding-flowchart.png "바인딩 프로세스가이 다이어그램에 표시 됩니다.")

이러한 파일의 내용에 대 한 자세한 내용은 [바인딩 형식 참조](~/cross-platform/macios/binding/binding-types-reference.md) 를 참조 하세요.

