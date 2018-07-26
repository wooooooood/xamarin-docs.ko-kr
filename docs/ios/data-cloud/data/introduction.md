---
title: Xamarin.iOS 앱의 데이터 저장소 소개
description: 이 문서는 Xamarin.iOS 응용 프로그램의 데이터 저장소를 위한 다양 한 방법에 설명 및 SQLite의 이점에 대 한 특정 정보를 제공 합니다.
ms.prod: xamarin
ms.assetid: B1994468-FD06-4FD9-96B3-FCEBB13A972A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: c5324d7e7daa8fbefde1776743dbdc595463fe33
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241151"
---
# <a name="introduction-to-data-storage-in-xamarinios-apps"></a>Xamarin.iOS 앱의 데이터 저장소 소개

## <a name="when-to-use-a-database"></a>데이터베이스를 사용 하는 경우

모바일 장치의 저장소 및 처리 기능을 증가 하는 동안 휴대폰 및 태블릿 여전히 보다 뒤 처지 데스크톱 &amp; 랩톱 대응 합니다. 이러한 이유로 가치가 데이터베이스는 항상 정확한 답을 가정 하는 것이 아니라 앱에 대 한 데이터 저장소 아키텍처를 계획 하는 시간이 좀 걸리고 있습니다. 와 같은 다양 한 요구 사항에 맞는 다양 한 옵션의 사항이 있습니다.

-  **기본 설정** – iOS에서는 간단한 키-값 쌍 데이터를 저장 하기 위한 기본 제공 메커니즘을 제공 합니다. 간단한 사용자 설정 또는 소량의 데이터 (예: 개인 설정 정보)를 저장 하는 이러한 종류의 정보를 저장 하기 위한 플랫폼의 기본 기능을 사용 합니다. IOS 용도 백업 하 고 여러 장치를 사용 하 여 사용자에 대 한 동기화 둘 다에서이 데이터를 icloud와 동기화 활용을 걸릴 수 있습니다.
-  **텍스트 파일** – 사용자 입력 또는의 캐시 다운로드 된 콘텐츠 (예: 파일 시스템에서 직접 HTML)을 저장할 수 있습니다. 적절 한 파일 명명 규칙을 사용 하 여 파일을 구성 하 고 데이터를 찾을 수 있도록 합니다.
-  **데이터 파일을 직렬화** – 파일 시스템에 XML 또는 JSON으로 개체를 유지할 수 있습니다. .NET framework는 직렬화 및 역직렬화 할 개체를 쉽게 확인 하는 라이브러리가 포함 되어 있습니다. 적절 한 이름을 사용 하 여 데이터 파일을 구성 합니다.
-  **데이터베이스** –는 SQLite 데이터베이스 엔진이 사용 가능한 iOS 및 쿼리, 정렬 또는 조작을 해야 하는 구조화 된 데이터를 저장 하는 데 유용 합니다. 데이터베이스 저장소는 대부분의 속성을 사용 하 여 데이터의 목록에 적합 합니다.
-  **이미지 파일** – 모바일 장치에서 데이터베이스에 이진 데이터를 저장할 수는 아니지만 것이 좋습니다 파일 시스템에 직접 저장 하는 합니다. 필요한 경우 다른 데이터를 사용 하 여 이미지를 연결할 데이터베이스의 파일 이름을 저장할 수 있습니다. 큰 이미지 또는 많은 이미지를 처리할 때 더 이상 모든 사용자의 저장소 공간을 사용 하지 않도록 하는 데 필요한 파일을 삭제 하는 캐싱 전략을 계획 하는 것이 좋습니다.


데이터베이스를 앱에 대 한 올바른 저장소 메커니즘 인 경우이 문서의 나머지 부분 Xamarin 플랫폼에서 SQLite를 사용 하는 방법을 설명 합니다.

## <a name="advantages-of-using-a-database"></a>데이터베이스를 사용할 때의 장점

모바일 앱에서 SQL database를 사용 하 여 장점이 많습니다.

-  SQL 데이터베이스는 구조화 된 데이터의 효율적인 저장소를 허용합니다.
-  복잡 한 쿼리를 사용 하 여 특정 데이터를 추출할 수 있습니다.
-  쿼리 결과 정렬할 수 있습니다.
-  쿼리 결과 집계할 수 있습니다.
-  기존 데이터베이스 기술을 통해 개발자는 데이터베이스 및 데이터 액세스 코드를 설계 하도록 지식을 활용할 수 있습니다.
-  연결된 된 응용 프로그램 서버 구성 요소에서 데이터 모델 다시에서 사용할 수 있습니다 (전체 또는 일부) 모바일 응용 프로그램입니다.


## <a name="sqlite-database-engine"></a>SQLite 데이터베이스 엔진

SQLite는 자신의 모바일 플랫폼에 대 한 Apple에서 채택 하는 오픈 소스 데이터베이스 엔진입니다. SQLite 데이터베이스 엔진이 iOS에 기본 제공 되므로 활용 하기 위한 추가 작업이 없습니다. SQLite는 때문에 플랫폼 간 모바일 개발에 적합 합니다.

-  데이터베이스 엔진 빠르고 쉽게 이식 가능한 작은 경우
-  데이터베이스를 간단히 모바일 장치에서 관리할 수 있는 단일 파일에 저장 됩니다.
-  파일 형식은 사용 하기 쉬운 플랫폼: 있는지 여부를 32 비트 또는 64 비트, 및 큰-또는 little endian 시스템입니다.
-  대부분의 표준 SQL92 구현합니다.


SQLite는 작고 빠른을 디자인 되었으므로 몇 가지 주의 사항이 용도에서:

-  일부 외부 조인 구문은 지원 되지 않습니다.
-  테이블 이름 바꾸기 및 ADDCOLUMN 지원 됩니다. 스키마에 다른 수정 작업을 수행할 수 없습니다.
-  뷰는 읽기 전용입니다.


웹 사이트-SQLite 자세히 알아보십시오 [SQLite.org](http://SQLite.org) -있지만 Xamarin을 사용 하 여 SQLite를 사용 하는 데 필요한 모든 정보는이 문서에 포함 된 샘플에 연결 합니다. SQLite 데이터베이스 엔진이 iOS의 모든 버전에 기본 제공 됩니다.
이 챕터에 포함 되지 않지만 SQLite Windows Phone 및 Windows 응용 프로그램에서 사용 하기 위해 사용할 수도 있습니다.

## <a name="windows-and-windows-phone"></a>Windows 및 Windows Phone

SQLite 사용할 수도 있습니다 Windows 플랫폼에서 있지만 이러한 플랫폼은이 문서에서 다루지 않습니다.
자세한 내용은 합니다 [Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) 하 고 [Tasky Pro](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/building_cross_platform_applications/case_study%3A_tasky) 사례 연구 및 검토 [Tim Heuer의 블로그](http://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx).



## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess 고급 (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS 데이터 레시피](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms 데이터 액세스](~/xamarin-forms/app-fundamentals/databases.md)
