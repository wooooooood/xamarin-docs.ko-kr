---
title: Xamarin.Android Data Access
description: 대부분의 응용 프로그램에는 로컬 장치에서 데이터를 저장 하려면 몇 가지 요구 사항이. 데이터의 양이 일반적으로 작은 아닌 일반적으로 데이터베이스 및 데이터 계층 응용 프로그램 데이터베이스 액세스를 관리 하에 있어야 합니다.  Android에 '기본 제공' SQLite 데이터베이스 엔진 및 데이터 저장 및 검색에 대 한 액세스는 Xamarin의 플랫폼을 통해 간소화 되었습니다. 이 문서에는 플랫폼 간 방식으로 SQLite 데이터베이스에 액세스 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 6B47E864-C6E7-4AA2-8DEF-2C8BF551D17C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 6858e290d93007d6054ba0ef63dce86e6e2e53e3
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67649626"
---
# <a name="xamarinandroid-data-access"></a>Xamarin.Android Data Access

_대부분의 응용 프로그램에는 로컬 장치에서 데이터를 저장 하려면 몇 가지 요구 사항이. 데이터의 양이 일반적으로 작은 아닌 일반적으로 데이터베이스 및 데이터 계층 응용 프로그램 데이터베이스 액세스를 관리 하에 있어야 합니다.  Android에 '기본 제공' SQLite 데이터베이스 엔진 및 데이터 저장 및 검색에 대 한 액세스는 Xamarin의 플랫폼을 통해 간소화 되었습니다. 이 문서에는 플랫폼 간 방식으로 SQLite 데이터베이스에 액세스 하는 방법을 보여 줍니다._

## <a name="data-access-overview"></a>데이터 액세스 개요

대부분의 응용 프로그램에는 로컬 장치에서 데이터를 저장 하려면 몇 가지 요구 사항이. 데이터의 양이 일반적으로 작은 아닌 일반적으로 데이터베이스 및 데이터 계층 응용 프로그램 데이터베이스 액세스를 관리 하에 있어야 합니다. Android 모두에 "기본 제공" SQLite 데이터베이스 엔진 및 데이터에 대 한 액세스는 SQLite 데이터 공급자와 함께 제공 되는 Xamarin의 플랫폼을 통해 간소화 되었습니다.

Xamarin.Android는와 같은 데이터베이스 액세스 Api를 지원 합니다.

- ADO.NET 프레임 워크입니다.
- SQLite-NET 타사 라이브러리입니다.

이 섹션의 코드 대부분 완전히 플랫폼 이며 수정 하지 않고 iOS 또는 Android에서 실행 됩니다. 설명 하는 두 개의 샘플 앱 가지가 있습니다.

- [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) &ndash; 간단한 데이터 작업 기록 결과 텍스트를 표시 컨트롤

- [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) &ndash; 데이터 작업을 나열 하 고 간단한 데이터 구조를 편집 하는 작은 작업 응용 프로그램을 통합 합니다.

샘플 솔루션을 모두 iOS 및 Android 샘플 응용 프로그램 프로젝트를 포함 합니다.

Xamarin.Forms 응용 프로그램에 대 한 읽을 [데이터베이스 작업](~/xamarin-forms/data-cloud/data/databases.md) Xamarin.Forms 사용 하 여 PCL 라이브러리에 SQLite를 사용 하는 방법을 설명 합니다.

이 섹션의에서 항목에서는 SQLite를 사용 하 여 데이터베이스 엔진으로 Xamarin.Android에서 데이터 액세스에 설명 합니다. ADO.NET 구문을 사용 하 여 "직접" 데이터베이스에 액세스할 수 있습니다 또는 SQLite.NET ORM을 포함 하 고 C#에서 데이터 작업을 수행할 수 있습니다.

두 개의 샘플을 검토 합니다: 출력 텍스트 필드를 포함 하는 간단한 응용 프로그램 만들기, 읽기는 매우 간단한 데이터 액세스 코드를 포함 하는 업데이트 및 삭제 기능입니다. 스레딩 및 응용 프로그램을 미리 채워진된 SQLite 데이터베이스를 사용 하 여 시드 하는 방법을 설명 합니다.

플랫폼 간 데이터 액세스의 추가 예제를 보려면 우리의 [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) 사례 연구 합니다.


## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess 고급 (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android 데이터 레시피](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin.Forms 데이터 액세스](~/xamarin-forms/data-cloud/data/databases.md)
