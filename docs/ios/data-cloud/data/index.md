---
title: Xamarin.iOS 데이터 액세스
description: 이 문서는 Xamarin.iOS 응용 프로그램에서 로컬 데이터베이스를 사용 하는 방법을 설명 하는 가이드에 연결 합니다. 연결 된 콘텐츠 SQLite.NET, ADO.NET 등을 설명합니다.
ms.prod: xamarin
ms.assetid: 3AEDFD8D-FB10-4CEF-BE04-CCD14E95F02C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/11/2016
ms.openlocfilehash: 8d2513ba1c2ae2769e81659c98f3897f33d83fbf
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112821"
---
# <a name="xamarinios-data-access"></a>Xamarin.iOS 데이터 액세스

Xamarin.iOS와 같은 데이터베이스 액세스 Api를 지원합니다.

-  ADO.NET 프레임 워크입니다.
-  SQLite-NET 타사 라이브러리입니다.

이 가이드에서는 일반적 ADO.NET 및 SQLite.NET Xamarin.iOS 응용 프로그램에서 SQLite 데이터베이스에 액세스를 설정 하는 방법을 설명 하기 전에 데이터베이스의 대략적인 개요를 제공 합니다. 

이 문서의 코드의 대부분 완전히 플랫폼 이며 수정 하지 않고 iOS 또는 Android에서 실행 됩니다. 설명 하는 두 개의 샘플 앱 가지가 있습니다.

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) – 간단한 데이터 작업 기록 결과 텍스트를 표시 컨트롤
-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) – 데이터 작업을 나열 하 고 간단한 데이터 구조를 편집 하는 작은 작업 응용 프로그램을 통합 합니다.

샘플 솔루션을 모두 iOS 및 Android 샘플 응용 프로그램 프로젝트를 포함 합니다.

Xamarin.Forms 응용 프로그램에 대 한 읽을 [데이터베이스 작업](~/xamarin-forms/app-fundamentals/databases.md) Xamarin.Forms 사용 하 여 PCL 라이브러리에 SQLite를 사용 하는 방법을 설명 합니다.

## <a name="sections"></a>섹션

-  [소개](introduction.md)
-  [구성](configuration.md)
-  [SQLite.NET ORM 사용](using-sqlite-orm.md)
-  [ADO.NET 사용](using-adonet.md)
-  [앱에서 데이터 사용](using-data-in-an-app.md)

## <a name="summary"></a>요약

이 장에서 Xamarin.iOS SQLite를 사용 하 여 데이터베이스 엔진으로의 데이터 액세스를 설명 합니다. "직접" ADO.NET 구문을 사용 하 여 데이터베이스에 액세스할 수 있습니다 또는 SQLite.NET ORM을 포함 하 고 C#에서 데이터 작업을 수행할 수 있습니다.

두 개의 샘플을 살펴보았습니다: 출력 텍스트 필드를 포함 하는 간단한 응용 프로그램 만들기, 읽기는 매우 간단한 데이터 액세스 코드를 포함 하는 업데이트 및 삭제 기능입니다. 에서는 또한 스레딩 및 응용 프로그램을 미리 채워진된 SQLite 데이터베이스를 사용 하 여 시드 하는 방법을 설명 합니다.

플랫폼 간 데이터 액세스의 추가 예제를 보려면 우리의 [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) 사례 연구 합니다.

## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess 고급 (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS 데이터 레시피](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms 데이터 액세스](~/xamarin-forms/app-fundamentals/databases.md)
