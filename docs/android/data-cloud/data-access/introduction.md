---
title: Android의 데이터 저장소 소개
ms.prod: xamarin
ms.assetid: FDAC0771-4749-4758-865A-F1BD190CA54B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 7e3f871d9d5992429c6f6619b2f5ff7059558045
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69525953"
---
# <a name="introduction"></a>소개

## <a name="when-to-use-a-database"></a>데이터베이스를 사용 하는 경우

모바일 장치의 저장소 및 처리 기능이 늘어나면 휴대폰 및 태블릿이 데스크톱 및 랩톱의 해당 항목 뒤에서 계속 지연 됩니다. 이러한 이유로 항상 데이터베이스가 올바른 답 인 것으로 가정 하는 것이 아니라 앱에 대 한 데이터 저장소 아키텍처를 계획 하는 데 다소 시간이 걸릴 수 있습니다. 다음과 같은 다양 한 요구 사항을 충족 하는 다양 한 옵션이 있습니다.

- **기본 설정** – Android는 간단한 키-값 쌍의 데이터를 저장 하는 기본 메커니즘을 제공 합니다. 간단한 사용자 설정 또는 작은 데이터 (예: 개인 설정 정보)를 저장 하는 경우이 유형의 정보를 저장 하는 데 플랫폼의 기본 기능을 사용 합니다.
- **텍스트 파일** – 다운로드 한 콘텐츠의 사용자 입력 또는 캐시 (예: HTML)를 파일 시스템에 직접 저장할 수 있습니다. 적절 한 파일 명명 규칙을 사용 하 여 파일을 구성 하 고 데이터를 찾을 수 있습니다.
- **직렬화 된 데이터 파일** – 파일 시스템에서 개체를 XML 또는 JSON으로 유지할 수 있습니다. .NET framework에는 개체를 쉽게 직렬화 및 역직렬화 하는 라이브러리가 포함 되어 있습니다. 적절 한 이름을 사용 하 여 데이터 파일을 구성 합니다.
- **데이터베이스** – SQLite 데이터베이스 엔진은 Android 플랫폼에서 사용할 수 있으며, 쿼리, 정렬 또는 달리 조작 해야 하는 구조화 된 데이터를 저장 하는 데 유용 합니다. 데이터베이스 저장소는 많은 속성이 있는 데이터 목록에 적합 합니다.
- **이미지 파일** – 모바일 장치에서 데이터베이스에 이진 데이터를 저장할 수 있지만 파일 시스템에 직접 저장 하는 것이 좋습니다. 필요한 경우 이미지를 다른 데이터와 연결 하기 위해 데이터베이스에 파일 이름을 저장할 수 있습니다. 큰 이미지 또는 많은 이미지를 처리 하는 경우 파일을 삭제 하는 캐싱 전략을 계획 하는 것이 좋습니다 .이는 더 이상 사용자의 저장소 공간을 더 이상 사용 하지 않아도 됩니다.

데이터베이스가 앱에 대 한 올바른 저장소 메커니즘인 경우이 문서의 나머지 부분에서는 Xamarin 플랫폼에서 SQLite를 사용 하는 방법을 설명 합니다.

## <a name="advantages-of-using-a-database"></a>데이터베이스 사용의 이점

모바일 앱에서 SQL database를 사용 하는 경우 다음과 같은 여러 가지 이점이 있습니다.

- SQL database를 사용 하면 구조화 된 데이터를 효율적으로 저장할 수 있습니다.
- 복잡 한 쿼리를 사용 하 여 특정 데이터를 추출할 수 있습니다.
- 쿼리 결과를 정렬할 수 있습니다.
- 쿼리 결과를 집계할 수 있습니다.
- 기존 데이터베이스 기술을 사용 하는 개발자는 자신의 지식을 활용 하 여 데이터베이스 및 데이터 액세스 코드를 디자인할 수 있습니다.
- 연결 된 응용 프로그램의 서버 구성 요소에 있는 데이터 모델은 모바일 응용 프로그램에서 전체적으로 나 부분적으로 다시 사용 될 수 있습니다.


## <a name="sqlite-database-engine"></a>SQLite 데이터베이스 엔진

SQLite는 모바일 플랫폼용 Google에서 채택 된 오픈 소스 데이터베이스 엔진입니다. SQLite 데이터베이스 엔진은 두 운영 체제에 기본 제공 되므로 개발자가 추가 작업을 사용 하지 않아도 됩니다. SQLite는 다음과 같은 이유 때문에 플랫폼 간 모바일 개발에 적합 합니다.

- 데이터베이스 엔진은 작고 빠르고 쉽게 이식할 수 있습니다.
- 데이터베이스는 모바일 장치에서 쉽게 관리할 수 있는 단일 파일에 저장 됩니다.
- 파일 형식은 32 또는 64 비트, 빅 또는 작은 endian 시스템의 플랫폼에서 쉽게 사용할 수 있습니다.
- SQL92 표준의 대부분을 구현 합니다.


SQLite는 작고 빠르게 설계 되었으므로 사용에 대 한 몇 가지 주의 사항이 있습니다.

- 일부 외부 조인 구문은 지원 되지 않습니다.
- 테이블 이름 바꾸기 및 ADDCOLUMN만 지원 됩니다. 스키마에 대해 다른 수정 작업을 수행할 수 없습니다.
- 뷰는 읽기 전용입니다.


[SQLite.org](http://SQLite.org) 에서 sqlite에 대해 자세히 알아볼 수 있습니다. 그러나 Xamarin을 사용 하는 데 필요한 모든 정보는이 문서 및 관련 샘플에 포함 되어 있습니다. SQLite 데이터베이스 엔진은 android 2 이후 Android에서 지원 됩니다.
이 장에서 다루지 않지만 SQLite는 Windows Phone 및 Windows 응용 프로그램 에서도 사용할 수 있습니다.

## <a name="windows-and-windows-phone"></a>Windows 및 Windows Phone

이러한 플랫폼은이 문서에서 다루지 않지만 SQLite는 Windows 플랫폼 에서도 사용할 수 있습니다.
[Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) 및 [tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) 사례 연구에서 추가 정보를 읽고 [Tim heuer cda (의 블로그](http://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx)를 검토 하세요.


## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Advanced (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android 데이터 조리법](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin 양식 데이터 액세스](~/xamarin-forms/data-cloud/data/databases.md)
