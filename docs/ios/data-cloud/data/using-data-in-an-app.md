---
title: "응용 프로그램의 데이터를 사용 하 여"
ms.topic: article
ms.prod: xamarin
ms.assetid: D5932AEB-0B6E-4F37-8B32-9BE4775AEE85
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: b9892ce129beaf168d8a091ff9b894db8bf66cf5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="using-data-in-an-app"></a>응용 프로그램의 데이터를 사용 하 여

**DataAccess_Adv** 샘플에서는 사용자 입력을 허용 하는 작업 중인 응용 프로그램을 보여 줍니다 및 *CRUD* 데이터베이스 기능 (만들기, 읽기, 업데이트 및 삭제) 합니다. 응용 프로그램 두 화면으로 이루어져: 목록 및 데이터 입력 폼입니다. 모든 데이터 액세스 코드는 수정 하지 않고 iOS 및 Android에서 다시 사용할 수 있습니다.

일부 데이터를 추가한 후 iOS에서 응용 프로그램 화면 다음과 같이 표시.

 ![](using-data-in-an-app-images/image9.png "iOS 샘플 목록")

 ![](using-data-in-an-app-images/image10.png "iOS 샘플 세부 정보")

이 섹션에 표시 된 코드에 포함 된 – iOS 프로젝트 아래 나와 **Orm** 디렉터리:

 ![](using-data-in-an-app-images/image13.png "iOS 프로젝트 트리")

IOS에서 ViewControllers 용 네이티브 UI 코드는이 문서에 대 한 범위를 벗어납니다.
참조는 [iOS 테이블 및 셀 작업](~/ios/user-interface/controls/tables/index.md) UI 컨트롤에 대 한 자세한 내용은 가이드입니다.

## <a name="read"></a>읽기

이 샘플에서 읽기 작업의 두 가지가 있습니다.

-  목록을 읽는
-  개별 레코드 읽기


두 개의 메서드는 `StockDatabase` 클래스:

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

데이터를 렌더링 하는 iOS와 다르게는 `UITableView`합니다.

## <a name="create-and-update"></a>만들기 및 업데이트

응용 프로그램 코드를 간소화 하기 위해 save 메서드는 삽입을 수행 하는 제공 되거나 PrimaryKey 설정 되었는지 여부에 따라 업데이트 합니다. 때문에 `Id` 속성이으로 표시 되는 `[PrimaryKey]` 특성 하지 코드에 설정 해야 합니다.
이 메서드는 되었는지 여부를 확인 값 이전 (기본 키 속성을 확인 하는 중)에 의해 저장 삽입 하거나 개체를 적절 하 게 업데이트 합니다.

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



실제 응용 프로그램 (예: 필수 필드, 최소 길이 또는 기타 비즈니스 규칙) 일부 유효성 검사가 필요 합니다.
플랫폼 간 응용 프로그램 플랫폼의 기능에 따라 디스플레이 대 한 UI 백업 유효성 검사 오류를 전달 하는 공유 코드에서 최대한 논리 유효성 검사의 많은 구현 합니다.

## <a name="delete"></a>삭제

와 달리는 `Insert` 및 `Update` 메서드는 `Delete<T>` 만 기본 키 값 보다는 전체 메서드를 사용할 수 `Stock` 개체입니다.
이 예제는 `Stock` 개체를 메서드에 전달 되지만 Id 속성으로 전달 됩니다는 `Delete<T>` 메서드.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>미리 채워진된 SQLite 데이터베이스 파일 사용

일부 응용 프로그램에 미리 데이터가 채워진 데이터베이스와 함께 제공 됩니다.
쉽게이 수행할 수 있습니다 모바일 응용 프로그램에서 앱을 가진 기존 SQLite 데이터베이스 파일을 전달 하 고 쓰기 가능한 디렉터리에 액세스 하기 전에 복사 하 여 합니다. SQLite 많은 플랫폼에서 사용 되는 표준 파일 형식 이므로 SQLite 데이터베이스 파일을 만드는 데 사용할 수 있는 도구는 여러 가지가 있습니다.

-  **SQLite 관리자 Firefox 확장** – iOS 및 Android와 호환 되는 파일을 Mac 및 Windows 및 생성에 작동 합니다.
-  **명령줄** – 참조 [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) 합니다.


응용 프로그램과 함께 배포에 대 한 데이터베이스 파일을 만들 때는 테이블 및 열 이름이 일치 하는지 확인 하세요 코드의에서는, C# 클래스와 속성에 맞게 이름을 증명 있는 SQLite.NET 사용 중인 경우에 특히 to와 주의 (또는 연결 된 사용자 지정 특성)입니다.

Ios의 경우 응용 프로그램에 sqlite 파일을 포함 하 고 확인으로 표시 되어 **빌드 작업: 콘텐츠**합니다. 코드를 추가 하는 `FinishedLaunching` 쓰기 가능한 디렉터리에 파일을 복사 하려면 *전에* 데이터 메서드를 호출 하기. 다음 코드는 라는 기존 데이터베이스를 복사 **data.sqlite**존재 하지 않는 경우에 합니다.

```csharp
// Copy the database across (if it doesn't exist)
var appdir = NSBundle.MainBundle.ResourcePath;
var seedFile = Path.Combine (appdir, "data.sqlite");
if (!File.Exists (Database.DatabaseFilePath))
{
  File.Copy (seedFile, Database.DatabaseFilePath);
}
```

모든 데이터 액세스 코드 (여부 ADO.NET 또는 SQLite.NET를 사용 하 여)이 작업은 미리 채워진된 데이터에 액세스 하는 완료 된 됩니다 후에 실행 하는 합니다.


## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess 고급 (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS 데이터 레시피](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Xamarin.Forms 데이터 액세스](~/xamarin-forms/app-fundamentals/databases.md)
