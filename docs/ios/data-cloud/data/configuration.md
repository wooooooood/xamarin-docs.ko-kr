---
title: Xamarin.iOS에서 SQLite를 구성합니다.
description: 이 문서에서는 Xamarin.iOS 응용 프로그램에서 SQLite 데이터베이스 파일의 위치를 확인 하는 방법을 설명 합니다. 이러한 개념은 선택한 데이터 액세스 메커니즘에 관계 없이 관련 됩니다.
ms.prod: xamarin
ms.assetid: E5582F4B-AD74-420F-9E6D-B07CFB420B3A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/11/2016
ms.openlocfilehash: ce9822b9c6c6a360f8c289f46d4d777d80b898e1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61217430"
---
# <a name="configuring-sqlite-in-xamarinios"></a>Xamarin.iOS에서 SQLite를 구성합니다.

Xamarin.iOS 응용 프로그램에서 SQLite를 사용 하려면 데이터베이스 파일에 대 한 올바른 파일 위치를 확인 해야 합니다.

## <a name="database-file-path"></a>데이터베이스 파일 경로

데이터 액세스 방법을 사용 하 여 만들어야 데이터베이스 파일 전에 SQLite를 사용 하 여 데이터를 저장할 수 있습니다. 대상으로 하는 플랫폼에 따라 파일 위치는 이와 다릅니다. IOS에 대 한 다음 코드 조각에 표시 된 대로 올바른 경로 생성 하려면으로 Environment 클래스를 사용할 수 있습니다.

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

가지 다른 데이터베이스 파일을 저장할 위치를 결정할 때 고려해 야 합니다. IOS에서 데이터베이스 백업 자동으로 (또는 비공유)를 확인할 수 있습니다.

플랫폼 간 응용 프로그램에서 각 플랫폼에서 다른 위치를 사용 하려는 경우 사용할 수 컴파일러 지시문 표시 된 것 처럼 각 플랫폼에 대해 다른 경로를 생성 합니다.

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

참조 된 [파일 시스템 작업](~/ios/app-fundamentals/file-system.md) iOS에서 사용 하는 파일 위치에 대 한 자세한 내용은 문서. 참조 된 [크로스 플랫폼 응용 프로그램 빌드](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) 컴파일러 지시문을 사용 하 여 각 플랫폼 특정 코드를 쓸 대 한 자세한 내용은 문서입니다.

## <a name="threading"></a>스레딩

여러 스레드에서 동일한 SQLite 데이터베이스 연결을 사용 하지 마십시오. 사용 하 여 열고 다음 동일한 스레드에서 만든 모든 연결을 닫습니다 주의 해야 합니다.

코드에 여러 스레드에서 동시에 SQLite 데이터베이스에 액세스 하려고 하지 되도록 수동으로 잠금을 같이 데이터베이스에 액세스 하려는 때마다:

```csharp
object locker = new object(); // class level private field
// rest of class code
lock (locker){
    // Do your query or insert here
}
```

모든 데이터베이스 액세스 (읽기, 쓰기, 업데이트 등)는 동일한 잠금을 사용 하 여 래핑되어야 합니다. 잠금 절 내에서 작업을 단순하게 유지 되 고 잠금을 촬영할 수도 있는 다른 방법 호출 하지 않습니다 교착 상태 상황이 발생 하지 않도록 주의 해야 합니다.


## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess 고급 (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS 데이터 레시피](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms 데이터 액세스](~/xamarin-forms/app-fundamentals/databases.md)
