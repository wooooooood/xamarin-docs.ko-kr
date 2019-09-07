---
title: Xamarin.Android Data Access
description: 대부분의 응용 프로그램에는 장치에 로컬로 데이터를 저장 하기 위한 몇 가지 요구 사항이 있습니다. 데이터 양이 일반적으로 작은 경우에는 데이터베이스 액세스를 관리 하기 위해 응용 프로그램에서 데이터베이스와 데이터 계층이 필요 합니다.  Android에는 SQLite 데이터베이스 엔진 ' 기본 제공 '이 있으며, Xamarin의 플랫폼에서 데이터를 저장 하 고 검색 하는 액세스를 간소화 합니다. 이 문서에서는 플랫폼 간 방식으로 SQLite 데이터베이스에 액세스 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 6B47E864-C6E7-4AA2-8DEF-2C8BF551D17C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 2343603199661ea39b1f0af172ce0ccf48a2cd66
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70754580"
---
# <a name="xamarinandroid-data-access"></a>Xamarin.Android Data Access

_대부분의 응용 프로그램에는 장치에 로컬로 데이터를 저장 하기 위한 몇 가지 요구 사항이 있습니다. 데이터 양이 일반적으로 작은 경우에는 데이터베이스 액세스를 관리 하기 위해 응용 프로그램에서 데이터베이스와 데이터 계층이 필요 합니다.  Android에는 SQLite 데이터베이스 엔진 ' 기본 제공 '이 있으며, Xamarin의 플랫폼에서 데이터를 저장 하 고 검색 하는 액세스를 간소화 합니다. 이 문서에서는 플랫폼 간 방식으로 SQLite 데이터베이스에 액세스 하는 방법을 보여 줍니다._

## <a name="data-access-overview"></a>데이터 액세스 개요

대부분의 응용 프로그램에는 장치에 로컬로 데이터를 저장 하기 위한 몇 가지 요구 사항이 있습니다. 데이터 양이 일반적으로 작은 경우에는 데이터베이스 액세스를 관리 하기 위해 응용 프로그램에서 데이터베이스와 데이터 계층이 필요 합니다. Android에는 sqlite 데이터베이스 엔진 "기본 제공"이 있으며, SQLite Data Provider와 함께 제공 되는 Xamarin 플랫폼에 의해 데이터 액세스가 간소화 됩니다.

Xamarin Android는 다음과 같은 데이터베이스 액세스 Api를 지원 합니다.

- ADO.NET 프레임 워크.
- SQLite-NET 타사 라이브러리.

이 섹션에 포함 된 대부분의 코드는 완전히 플랫폼 간 이며 수정 없이 iOS 또는 Android에서 실행 됩니다. 다음 두 가지 샘플 앱이 설명 되어 있습니다.

- [**DataAccess_Basic**](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) &ndash; 간단한 데이터 작업은 텍스트 표시 컨트롤에 결과를 기록 합니다.

- [**DataAccess_Advanced**](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) &ndash; 데이터 작업을 간단한 데이터 구조를 나열 하 고 편집 하는 작은 작업 응용 프로그램에 통합 합니다.

두 샘플 솔루션에는 iOS 및 Android 샘플 응용 프로그램 프로젝트가 포함 되어 있습니다.

Xamarin.ios 응용 프로그램의 경우 Xamarin.ios를 사용 하 여 PCL로 작업 하는 방법을 설명 하는 [데이터베이스 작업](~/xamarin-forms/data-cloud/data/databases.md) 을 참조 하세요.

이 섹션의 항목에서는 데이터베이스 엔진으로 SQLite를 사용 하 여 Xamarin.ios의 데이터 액세스에 대해 설명 합니다. ADO.NET 구문을 사용 하 여 데이터베이스에 "직접" 액세스 하거나 SQLite.NET ORM을 포함 하 고에서 C#데이터 작업을 수행할 수 있습니다.

두 샘플이 검토 됩니다. 하나는 텍스트 필드에 출력 하는 매우 간단한 데이터 액세스 코드를 포함 하는 것이 고, 만들기, 읽기, 업데이트 및 삭제 기능을 포함 하는 간단한 응용 프로그램입니다. 스레딩 및 미리 채워진 SQLite 데이터베이스를 사용 하 여 응용 프로그램을 시드 하는 방법에 대해서도 설명 합니다.

플랫폼 간 데이터 액세스의 추가 예는 [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) 사례 연구를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Advanced (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android 데이터 조리법](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin 양식 데이터 액세스](~/xamarin-forms/data-cloud/data/databases.md)
