---
title: Xamarin.iOS 데이터 액세스
description: Xamarin.iOS 응용 프로그램에서 로컬 데이터베이스와 함께 작업 하는 방법에 설명 하는 지침은이 문서 연결 되어 있습니다. 연결 된 내용을 SQLite.NET, ADO.NET 등을 설명합니다.
ms.prod: xamarin
ms.assetid: 3AEDFD8D-FB10-4CEF-BE04-CCD14E95F02C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: a986ea9931f62497e5a6863c84bd4041983d66d9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784578"
---
# <a name="xamarinios-data-access"></a>Xamarin.iOS 데이터 액세스

Xamarin.iOS와 같은 데이터베이스 액세스 Api를 지원합니다.

-  ADO.NET 프레임 워크입니다.
-  SQLite NET 타사 라이브러리.

이 가이드에서는 일반적 Xamarin.iOS 응용 프로그램에서 SQLite 데이터베이스에 액세스할 수 ADO.NET 및 SQLite.NET를 설정 하는 방법을 설명 하기 전에 데이터베이스의 높은 수준의 개요를 제공 합니다. 

이 문서에 있는 코드의 대부분 완전히 크로스 플랫폼 하 고 수정 하지 않고 iOS 또는 Android에서 실행 됩니다. 설명 하는 두 개의 샘플 앱 가지가 있습니다.

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) – 간단한 데이터 작업; 컨트롤을 표시 하는 결과 텍스트를 씁니다.
-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) – 데이터 작업을 나열 하 고 간단한 데이터 구조를 편집 하는 작은 작업 응용 프로그램에 통합 합니다.

두 샘플 솔루션에는 iOS 및 Android 샘플 응용 프로그램 프로젝트를 포함 합니다.

Xamarin.Forms 응용 프로그램에 대 한 읽기 [데이터베이스 작업](~/xamarin-forms/app-fundamentals/databases.md) SQLite PCL 라이브러리에 xamarin.forms를 사용 하는 방법을 설명 하 합니다.

## <a name="sections"></a>섹션

-  [소개](introduction.md)
-  [구성](configuration.md)
-  [SQLite.NET ORM 사용](using-sqlite-orm.md)
-  [ADO.NET 사용](using-adonet.md)
-  [앱에서 데이터 사용](using-data-in-an-app.md)

## <a name="summary"></a>요약

이 장에서 Xamarin.iOS SQLite를 사용 하 여 데이터베이스 엔진으로의 데이터 액세스를 설명 합니다. "직접" ADO.NET 구문을 사용 하 여 데이터베이스에 액세스할 수 있고 SQLite.NET ORM 포함 C#에서 데이터 작업을 수행할 수 있습니다.

두 개의 샘플 방법을 살펴보았습니다: 업데이트 및 삭제 기능을 출력 텍스트 필드를 포함 하는 간단한 응용 프로그램 만들기, 읽기는 매우 간단한 데이터 액세스 코드를 포함 합니다. 우리는 에서도 스레딩 및 응용 프로그램을 미리 채워진된 SQLite 데이터베이스를 시드하고 하는 방법을 설명 합니다.

플랫폼 간 데이터 액세스의 추가 예 참조에 대 한 우리의 [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) 사례 연구.

## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess 고급 (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS 데이터 레시피](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Xamarin.Forms 데이터 액세스](~/xamarin-forms/app-fundamentals/databases.md)
