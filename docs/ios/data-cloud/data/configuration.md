---
title: Xamarin.ios에서 SQLite 구성
description: 이 문서에서는 Xamarin.ios 응용 프로그램에서 SQLite 데이터베이스 파일의 위치를 확인 하는 방법을 설명 합니다. 이러한 개념은 선택한 데이터 액세스 메커니즘에 관계 없이 관련이 있습니다.
ms.prod: xamarin
ms.assetid: E5582F4B-AD74-420F-9E6D-B07CFB420B3A
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 10/11/2016
ms.openlocfilehash: 7b60c8f306ad815cd3292cc94bd80f87b49df547
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70766990"
---
# <a name="configuring-sqlite-in-xamarinios"></a>Xamarin.ios에서 SQLite 구성

Xamarin.ios 응용 프로그램에서 SQLite를 사용 하려면 데이터베이스 파일에 대 한 올바른 파일 위치를 확인 해야 합니다.

## <a name="database-file-path"></a>데이터베이스 파일 경로

사용 하는 데이터 액세스 방법에 관계 없이 데이터베이스 파일을 만든 후에 SQLite를 사용 하 여 데이터를 저장 해야 합니다. 파일 위치를 대상으로 하는 플랫폼에 따라 달라 집니다. IOS의 경우 다음 코드 조각과 같이 환경 클래스를 사용 하 여 유효한 경로를 생성할 수 있습니다.

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

데이터베이스 파일을 저장할 위치를 결정할 때 고려해 야 할 다른 사항이 있습니다. IOS에서 데이터베이스를 자동으로 백업 (또는 그렇지 않음) 할 수 있습니다.

플랫폼 간 응용 프로그램의 각 플랫폼에서 다른 위치를 사용 하려는 경우 표시 된 것 처럼 컴파일러 지시문을 사용 하 여 각 플랫폼에 대해 다른 경로를 생성할 수 있습니다.

```csharp
var sqliteFilename = "MyDatabase.db3";
#if __ANDROID__
// Just use whatever directory SpecialFolder.Personal returns
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
// we need to put in /Library/ on iOS5.1+ to meet Apple's iCloud terms
// (they don't want non-user-generated data in Documents)
string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder instead
#endif
var path = Path.Combine (libraryPath, sqliteFilename);
```

IOS에서 사용할 파일 위치에 대 한 자세한 내용은 [파일 시스템 작업](~/ios/app-fundamentals/file-system.md) 문서를 참조 하세요. 컴파일러 지시문을 사용 하 여 각 플랫폼과 관련 된 코드를 작성 하는 방법에 대 한 자세한 내용은 [플랫폼 간 응용 프로그램 빌드](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) 문서를 참조 하세요.

## <a name="threading"></a>스레딩

여러 스레드에서 동일한 SQLite 데이터베이스 연결을 사용 하면 안 됩니다. 를 열고 사용한 다음 동일한 스레드에서 만든 모든 연결을 닫아야 합니다.

코드가 여러 스레드에서 동시에 SQLite 데이터베이스에 액세스 하려고 하지 않도록 하려면 다음과 같이 데이터베이스에 액세스할 때마다 수동으로 잠금을 수행 합니다.

```csharp
object locker = new object(); // class level private field
// rest of class code
lock (locker){
    // Do your query or insert here
}
```

모든 데이터베이스 액세스 (읽기, 쓰기, 업데이트 등)는 동일한 잠금으로 래핑해야 합니다. Lock 절 내의 작업을 간단 하 게 유지 하 고 잠금을 사용할 수 있는 다른 메서드를 호출 하지 않도록 하 여 교착 상태 상황을 방지 하려면 주의를 기울여야 합니다.

## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Advanced (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS 데이터 조리법](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin 양식 데이터 액세스](~/xamarin-forms/data-cloud/data/databases.md)
