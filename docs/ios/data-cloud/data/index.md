---
title: Xamarin.ios 데이터 액세스
description: 이 문서는 Xamarin.ios 응용 프로그램에서 로컬 데이터베이스로 작업 하는 방법을 설명 하는 가이드로 연결 됩니다. 연결 된 콘텐츠 SQLite.NET, ADO.NET 등에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 3AEDFD8D-FB10-4CEF-BE04-CCD14E95F02C
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 10/11/2016
ms.openlocfilehash: 6050320f781ea89c5455e10a8754a89d7086e92a
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70281738"
---
# <a name="xamarinios-data-access"></a>Xamarin.ios 데이터 액세스

Xamarin.ios는 다음과 같은 데이터베이스 액세스 Api를 지원 합니다.

- ADO.NET 프레임 워크.
- SQLite-NET 타사 라이브러리.

이 가이드에서는 ADO.NET 및 SQLite.NET를 설정 하 여 Xamarin.ios 응용 프로그램의 SQLite 데이터베이스에 액세스 하는 방법을 설명 하기 전에 일반적으로 데이터베이스에 대 한 개략적인 개요를 제공 합니다. 

이 문서에 포함 된 대부분의 코드는 완전히 크로스 플랫폼 이며 수정 없이 iOS 또는 Android에서 실행 됩니다. 다음 두 가지 샘플 앱이 설명 되어 있습니다.

- [**DataAccess_Basic**](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) – 간단한 데이터 작업은 텍스트 표시 컨트롤에 결과를 기록 합니다.
- [**DataAccess_Advanced**](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) – 데이터 작업을 간단한 데이터 구조를 나열 하 고 편집 하는 작은 작업 응용 프로그램에 통합 합니다.

두 샘플 솔루션에는 iOS 및 Android 샘플 응용 프로그램 프로젝트가 포함 되어 있습니다.

Xamarin.ios 응용 프로그램의 경우 Xamarin.ios를 사용 하 여 PCL로 작업 하는 방법을 설명 하는 [데이터베이스 작업](~/xamarin-forms/data-cloud/data/databases.md) 을 참조 하세요.

## <a name="sections"></a>섹션

- [소개](introduction.md)
- [구성](configuration.md)
- [SQLite.NET ORM 사용](using-sqlite-orm.md)
- [ADO.NET 사용](using-adonet.md)
- [앱에서 데이터 사용](using-data-in-an-app.md)

## <a name="summary"></a>요약

이 장에서는 데이터베이스 엔진으로 SQLite를 사용 하 여 Xamarin.ios의 데이터 액세스에 대해 설명 했습니다. ADO.NET 구문을 사용 하 여 데이터베이스에 "직접" 액세스할 수 있으며, SQLite.NET ORM을 포함 하 고에서 C#데이터 작업을 수행할 수 있습니다.

텍스트 필드에 출력 하는 매우 간단한 데이터 액세스 코드를 포함 하는 샘플 및 만들기, 읽기, 업데이트 및 삭제 기능을 포함 하는 간단한 응용 프로그램의 두 가지 샘플을 검토 했습니다. 또한 스레딩 및 미리 채워진 SQLite 데이터베이스로 응용 프로그램의 초기값을 설명 합니다.

플랫폼 간 데이터 액세스의 추가 예는 [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) 사례 연구를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Advanced (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS 데이터 조리법](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin 양식 데이터 액세스](~/xamarin-forms/data-cloud/data/databases.md)
