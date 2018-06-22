---
title: Android에서 데이터 저장소 소개
ms.prod: xamarin
ms.assetid: FDAC0771-4749-4758-865A-F1BD190CA54B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/28/2017
ms.openlocfilehash: 42af02d2ed2a679d89425257d92f03a3764675ce
ms.sourcegitcommit: 797597d902330652195931dec9ac3e0cc00792c5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/20/2018
ms.locfileid: "31646991"
---
# <a name="introduction"></a>소개

## <a name="when-to-use-a-database"></a>데이터베이스를 사용 하는 경우

모바일 장치의 저장소 및 처리 기능으로 증가 하는 동안 데스크톱 및 랩톱 대응 뒤 휴대폰 및 태블릿 여전히 지연 합니다. 이러한 이유로 알아두는 것 바로 데이터베이스는 항상 올바른 결과 가정 하는 것이 아니라 응용 프로그램에 대 한 데이터 저장소 아키텍처를 계획 하는 데 시간이 걸리고입니다. 와 같은 서로 다른 요구 사항에 따라 다양 한 옵션의 여러 가지:

-  **기본 설정** – Android에서는 단순한 키-값 쌍의 데이터를 저장 하기 위한 기본 제공 메커니즘을 제공 합니다. 간단한 사용자 설정 또는 소량의 데이터 (예: 개인 설정 정보)를 저장 하는 이러한 종류의 정보를 저장 하기 위한 플랫폼의 기본 기능을 사용 합니다.
-  **텍스트 파일** -사용자 입력 또는의 캐시 않음 (예: 콘텐츠 다운로드 파일 시스템에서 직접 HTML)를 저장할 수 있습니다. 적절 한 파일 명명 규칙을 사용 하 여 파일을 구성 하 고 데이터를 찾을 수 있도록 합니다.
-  **데이터 파일을 직렬화** – 파일 시스템에 XML 또는 JSON 형식으로 개체를 보관할 수 있습니다. .NET framework는 직렬화 및 역직렬화 할 개체를 쉽게 확인 하는 라이브러리를 포함 합니다. 적절 한 이름을 사용 하 여 데이터 파일을 구성 합니다.
-  **데이터베이스** – The SQLite 데이터베이스 엔진은 Android 플랫폼에서 사용할 수 있는 하 고 저장 하는 데 유용한 구조화 된 데이터를 쿼리, 정렬 하거나 조작 해야 하는 합니다. 데이터베이스 저장소는 많은 속성을 사용 하 여 데이터의 목록에 적합 합니다.
-  **이미지 파일** – 모바일 장치에서 데이터베이스에 이진 데이터를 저장할 수 있지만 것이 좋습니다 파일 시스템에 직접 저장 하 합니다. 필요한 경우 이미지를 다른 데이터와 연결 하기 위해 데이터베이스에 파일 이름을 저장할 수 있습니다. 큰 이미지 또는 다양 한 이미지를 처리할 때 것이 좋습니다 모든 사용자의 저장소 공간을 사용 하지 않도록 필요 없는 파일을 삭제 하는 캐싱 전략을 계획 합니다.

앱에 대 한 올바른 저장 메커니즘은 데이터베이스의 경우이 문서의 나머지 부분에서는 SQLite Xamarin 플랫폼에서 사용 하는 방법을 설명 합니다.

## <a name="advantages-of-using-a-database"></a>데이터베이스 사용의 장점

다양 한 모바일 앱에서 SQL 데이터베이스 사용의 이점에는

-  SQL 데이터베이스는 구조화 된 데이터를 효율적으로 저장할을 허용 합니다.
-  복잡 한 쿼리에서 특정 데이터를 추출할 수 있습니다.
-  쿼리 결과 정렬할 수 있습니다.
-  쿼리 결과 집계할 수 있습니다.
-  기존 데이터베이스 기술 개발자에 게 데이터베이스 및 데이터 액세스 코드 디자인에 대 한 지식을 활용할 수 있습니다.
-  연결 된 응용 프로그램의 서버 구성 요소에서 데이터 모델 다시 또는 사용할 수 있습니다 (전체에서 부분적) 모바일 응용 프로그램에서 합니다.


## <a name="sqlite-database-engine"></a>SQLite 데이터베이스 엔진

SQLite는 자신의 모바일 플랫폼에 대 한 Google에서 채택 하는 오픈 소스 데이터베이스 엔진입니다. SQLite 데이터베이스 엔진은 활용 하는 개발자를 위한 추가 작업 없이 이므로 두 운영 체제에 기본적으로 제공 합니다. SQLite는 때문에 플랫폼 간 모바일 개발에 매우 적합 합니다.

-  데이터베이스 엔진에는 작은 빠르고 쉽게 가져올은입니다.
-  데이터베이스는 모바일 장치에서 관리 하기 쉬운 단일 파일에 저장 됩니다.
-  파일 형식이 플랫폼에서 사용 하기 쉬운: 여부를 32 비트 또는 64 비트, 및 큰-또는 little endian 시스템.
-  대부분의 표준 SQL92 구현합니다.


SQLite는 하도록 설계 되었으므로 작고 빠르게, 몇 가지 주의할 사항이 용도에 있습니다.

-  일부 OUTER join 구문이 지원 되지 않습니다.
-  테이블 이름 바꾸기 및 ADDCOLUMN 지원 됩니다. 스키마에 기타 수정 작업을 수행할 수 없습니다.
-  뷰는 읽기 전용입니다.


-웹 사이트에서 SQLite에 대해 자세히 알아볼 수 있습니다 [SQLite.org](http://SQLite.org) SQLite Xamarin 함께 사용 하는 데 필요한 모든 정보는이 문서에 포함 된 샘플 관련 있지만-합니다. SQLite 데이터베이스 엔진 Android 2 이후 Android에서 지원 되는 합니다.
이 장에서 설명 하지 않지만 SQLite도 Windows Phone 및 Windows 응용 프로그램에서 사용 하기 위해 제공 됩니다.

## <a name="windows-and-windows-phone"></a>Windows 및 Windows Phone

SQLite 사용할 수도 있습니다 Windows 플랫폼에서 있지만 해당 플랫폼은이 문서에서 다루지 않습니다.
자세한 내용은 [Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) 및 [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) 사례 연구, 검토 및 [Tim Heuer 블로그](http://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx)합니다.


## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess 고급 (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android 데이터 레시피](https://developer.xamarin.com/recipes/android/data/)
- [Xamarin.Forms 데이터 액세스](~/xamarin-forms/app-fundamentals/databases.md)
