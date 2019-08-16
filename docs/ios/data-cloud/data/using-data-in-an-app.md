---
title: IOS 앱에서 데이터 사용
description: 이 문서에서는 Xamarin.ios 앱에서 사용자 입력을 수집 하 고 CRUD (만들기, 읽기, 업데이트 및 삭제) 데이터베이스 작업을 수행 하는 방법을 보여 주는 DataAccess_Adv 샘플에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 2CB8150E-CD2C-4E97-8605-1EE8CBACFEEC
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/11/2016
ms.openlocfilehash: 9da0f48a8798a16ccd6410913d0b31c0b4444cb8
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69527186"
---
# <a name="using-data-in-an-ios-app"></a>IOS 앱에서 데이터 사용

**DataAccess_Adv** 샘플은 사용자 입력 및 *CRUD* (만들기, 읽기, 업데이트 및 삭제) 데이터베이스 기능을 허용 하는 작동 하는 응용 프로그램을 보여 줍니다. 응용 프로그램은 목록 및 데이터 입력 형식의 두 가지 화면으로 구성 됩니다. 모든 데이터 액세스 코드는 수정 없이 iOS 및 Android에서 다시 사용할 수 있습니다.

일부 데이터를 추가한 후 응용 프로그램 화면은 iOS에서 다음과 같이 표시 됩니다.

 ![](using-data-in-an-app-images/image9.png "iOS 샘플 목록")

 ![](using-data-in-an-app-images/image10.png "iOS 샘플 세부 정보")

IOS 프로젝트는 아래와 같습니다 .이 섹션에 표시 된 코드는 **Orm** 디렉터리에 포함 되어 있습니다.

 ![](using-data-in-an-app-images/image13.png "iOS 프로젝트 트리")

IOS에서 ViewControllers의 기본 UI 코드는이 문서의 범위를 벗어났습니다.
UI 컨트롤에 대 한 자세한 내용은 [IOS 테이블 및 셀 작업](~/ios/user-interface/controls/tables/index.md) 가이드를 참조 하세요.

## <a name="read"></a>Read

예제에는 몇 가지 읽기 작업이 있습니다.

- 목록 읽기
- 개별 레코드 읽기


`StockDatabase` 클래스의 두 메서드는 다음과 같습니다.

```csharp
public IEnumerable<Stock> GetStocks ()
{
    lock (locker) {
        return (from i in Table<Stock> () select i).ToList ();
    }
}
public Stock GetStock (int id)
{
    lock (locker) {
        return Table<Stock>().FirstOrDefault(x => x.Id == id);
    }
}
```

iOS는와 다른 방식으로 `UITableView`데이터를 렌더링 합니다.

## <a name="create-and-update"></a>만들기 및 업데이트

응용 프로그램 코드를 간소화 하기 위해 PrimaryKey의 설정 여부에 따라 삽입 또는 업데이트를 수행 하는 단일 save 메서드가 제공 됩니다. 속성은 `[PrimaryKey]` 특성으로 표시 되므로 코드에서 설정 하면 안 됩니다. `Id`
이 메서드는 기본 키 속성을 확인 하 여 값이 이전에 저장 되었는지 여부를 검색 하 고 그에 따라 개체를 삽입 또는 업데이트 합니다.

```csharp
public int SaveStock (Stock item)
{
    lock (locker) {
        if (item.Id != 0) {
            Update (item);
            return item.Id;
    } else {
            return Insert (item);
        }
    }
}
```



실제 응용 프로그램은 일반적으로 일부 유효성 검사 (예: 필수 필드, 최소 길이 또는 기타 비즈니스 규칙)를 요구 합니다.
적절 한 플랫폼 간 응용 프로그램은 공유 코드에서 가능한 한 많은 유효성 검사 논리를 구현 하 여 플랫폼의 기능에 따라 표시 하기 위해 유효성 검사 오류를 UI에 다시 전달 합니다.

## <a name="delete"></a>삭제

및 메서드와 달리 `Delete<T>` 메서드는 전체 `Stock` 개체가 아닌 기본 키 값만 수락할 수 있습니다. `Insert` `Update`
이 예제 `Stock` 에서 개체는 메서드에 전달 되지만 Id 속성만 `Delete<T>` 메서드에 전달 됩니다.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>미리 채워진 SQLite 데이터베이스 파일 사용

일부 응용 프로그램은 이미 데이터로 채워진 데이터베이스와 함께 제공 됩니다.
앱에 기존 SQLite 데이터베이스 파일을 전달 하 고 액세스 하기 전에 쓰기 가능한 디렉터리에 복사 하 여 모바일 응용 프로그램에서이를 쉽게 수행할 수 있습니다. SQLite는 대부분의 플랫폼에서 사용 되는 표준 파일 형식 이므로 SQLite 데이터베이스 파일을 만드는 데 사용할 수 있는 여러 도구가 있습니다.

- **SQLite 관리자 Firefox 확장** – Mac 및 Windows에서 작동 하며 IOS 및 Android와 호환 되는 파일을 생성 합니다.
- **명령줄** – [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) 를 참조 하세요.


앱과 함께 배포할 데이터베이스 파일을 만들 때 특히 이름이 C# 클래스 및 속성과 일치 해야 하는 SQLite.NET를 사용 하는 경우에는 테이블 및 열의 이름을 지정 하 여 코드에 필요한 내용과 일치 하는지 확인 합니다. 또는 연결 된 사용자 지정 특성을 지정 합니다.

IOS의 경우 응용 프로그램에 sqlite 파일을 포함 하 고 빌드 작업으로 **표시 되어 있는지 확인 합니다. 콘텐츠**. 데이터 메서드를 호출 `FinishedLaunching` *하기 전에* 에 코드를 저장 하 여 쓰기 가능한 디렉터리에 파일을 복사 합니다. 다음 코드는 이미 존재 하지 않는 경우에만 **data. sqlite**라는 기존 데이터베이스를 복사 합니다.

```csharp
// Copy the database across (if it doesn't exist)
var appdir = NSBundle.MainBundle.ResourcePath;
var seedFile = Path.Combine (appdir, "data.sqlite");
if (!File.Exists (Database.DatabaseFilePath))
{
  File.Copy (seedFile, Database.DatabaseFilePath);
}
```

이 작업이 완료 된 후 실행 되는 모든 데이터 액세스 코드 (ADO.NET 또는 using SQLite.NET)는 미리 채워진 데이터에 액세스할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Advanced (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS 데이터 조리법](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin 양식 데이터 액세스](~/xamarin-forms/data-cloud/data/databases.md)
