---
title: Xamarin.Android Data Access
description: 대부분의 응용 프로그램에는 로컬 장치에서 데이터를 저장 하려면 몇 가지 요구를 사항이 있습니다. 많으면 데이터의 양이 다르거나 일반적으로,이 일반적으로 필요 데이터베이스 및 데이터 계층 응용 프로그램에 데이터베이스 액세스를 관리 합니다.  Android에는 '기본' SQLite 데이터베이스 엔진에는 및 데이터 저장 및 검색에 대 한 액세스 Xamarin 플랫폼을 통해 간소화 되었습니다. 이 문서는 플랫폼 간 방식에서 SQLite 데이터베이스에 액세스 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 6B47E864-C6E7-4AA2-8DEF-2C8BF551D17C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 508a8f54723bfdd30b1c8ea8d4b6c1d422ae24e9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763252"
---
# <a name="xamarinandroid-data-access"></a>Xamarin.Android Data Access

_대부분의 응용 프로그램에는 로컬 장치에서 데이터를 저장 하려면 몇 가지 요구를 사항이 있습니다. 많으면 데이터의 양이 다르거나 일반적으로,이 일반적으로 필요 데이터베이스 및 데이터 계층 응용 프로그램에 데이터베이스 액세스를 관리 합니다.  Android에는 '기본' SQLite 데이터베이스 엔진에는 및 데이터 저장 및 검색에 대 한 액세스 Xamarin 플랫폼을 통해 간소화 되었습니다. 이 문서는 플랫폼 간 방식에서 SQLite 데이터베이스에 액세스 하는 방법을 보여 줍니다._

## <a name="data-access-overview"></a>데이터 액세스 개요

대부분의 응용 프로그램에는 로컬 장치에서 데이터를 저장 하려면 몇 가지 요구를 사항이 있습니다. 많으면 데이터의 양이 다르거나 일반적으로,이 일반적으로 필요 데이터베이스 및 데이터 계층 응용 프로그램에 데이터베이스 액세스를 관리 합니다. 두 android "내장" Sqlite 데이터베이스 엔진 있으며 데이터에 대 한 액세스 SQLite 데이터 공급자와 함께 제공 되는 Xamarin 플랫폼을 통해 간소화 되었습니다.

Xamarin.Android와 같은 데이터베이스 액세스 Api를 지원 합니다.

-  ADO.NET 프레임 워크입니다.
-  SQLite NET 타사 라이브러리.

대부분의이 섹션의 코드는 완전히 크로스 플랫폼 하 고 수정 하지 않고 iOS 또는 Android에서 실행 됩니다. 설명 하는 두 개의 샘플 앱 가지가 있습니다.

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) &ndash; 단순 데이터 작업; 컨트롤을 표시 하는 결과 텍스트를 씁니다.

-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) &ndash; 데이터 작업을 나열 하 고 간단한 데이터 구조를 편집 하는 작은 작업 응용 프로그램에 통합 합니다.

두 샘플 솔루션에는 iOS 및 Android 샘플 응용 프로그램 프로젝트를 포함 합니다.

Xamarin.Forms 응용 프로그램에 대 한 읽기 [데이터베이스 작업](~/xamarin-forms/app-fundamentals/databases.md) SQLite PCL 라이브러리에 xamarin.forms를 사용 하는 방법을 설명 하 합니다.

이 섹션의 항목 Xamarin.Android SQLite를 사용 하 여 데이터베이스 엔진으로의 데이터 액세스에 설명 합니다. ADO.NET 구문을 사용 하 여 "직접"는 데이터베이스에 액세스할 수 있고 SQLite.NET ORM 포함 C#에서 데이터 작업을 수행할 수 있습니다.

두 개의 샘플을 검토: 업데이트 및 삭제 기능을 출력 텍스트 필드를 포함 하는 간단한 응용 프로그램 만들기, 읽기는 매우 간단한 데이터 액세스 코드를 포함 합니다. 스레딩 및 응용 프로그램을 미리 채워진된 SQLite 데이터베이스를 시드하고 하는 방법을 설명 합니다.

플랫폼 간 데이터 액세스의 추가 예 참조에 대 한 우리의 [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) 사례 연구.


## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess 고급 (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android 데이터 레시피](https://developer.xamarin.com/recipes/android/data/)
- [Xamarin.Forms 데이터 액세스](~/xamarin-forms/app-fundamentals/databases.md)
