---
title: Android 앱에서 데이터 사용
ms.prod: xamarin
ms.assetid: D5932AEB-0B6E-4F37-8B32-9BE4775AEE85
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/08/2018
ms.openlocfilehash: 922b1fa411a176df580050384e7555120fd68137
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "70754455"
---
# <a name="using-data-in-an-app"></a>앱에서 데이터 사용

**DataAccess_Adv** 샘플은 사용자 입력 및 CRUD (만들기, 읽기, 업데이트 및 삭제) 데이터베이스 기능을 허용 하는 작동 하는 응용 프로그램을 보여 줍니다. 응용 프로그램은 목록 및 데이터 입력 형식의 두 가지 화면으로 구성 됩니다. 모든 데이터 액세스 코드는 수정 없이 iOS 및 Android에서 다시 사용할 수 있습니다.

일부 데이터를 추가한 후에는 Android에서 응용 프로그램 화면이 다음과 같이 표시 됩니다.

![Android 샘플 목록](using-data-in-an-app-images/image11.png "Android 샘플 목록")

![Android 샘플 세부 정보](using-data-in-an-app-images/image12.png "Android 샘플 세부 정보")

Android 프로젝트는 &ndash; 아래에 표시 되어 있습니다 .이 섹션에 표시 된 코드는 **Orm** 디렉터리에 포함 되어 있습니다.

![Android 프로젝트 트리](using-data-in-an-app-images/image14.png "Android 프로젝트 트리")

Android의 활동에 대 한 네이티브 UI 코드는이 문서의 범위를 벗어났습니다. UI 컨트롤에 대 한 자세한 내용은 [Android listview And 어댑터](~/android/user-interface/layouts/list-view/index.md) 가이드를 참조 하세요.

## <a name="read"></a>Read

예제에는 몇 가지 읽기 작업이 있습니다.

- 목록 읽기
- 개별 레코드 읽기

@No__t_0 클래스의 두 메서드는 다음과 같습니다.

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

Android는 데이터를 `ListView`으로 렌더링 합니다.

## <a name="create-and-update"></a>만들기 및 업데이트

응용 프로그램 코드를 간소화 하기 위해 PrimaryKey의 설정 여부에 따라 삽입 또는 업데이트를 수행 하는 단일 save 메서드가 제공 됩니다. @No__t_0 속성은 `[PrimaryKey]` 특성으로 표시 되므로 코드에서 설정 하면 안 됩니다. 이 메서드는 기본 키 속성을 확인 하 여 값이 이전에 저장 되었는지 여부를 검색 하 고 그에 따라 개체를 삽입 또는 업데이트 합니다.

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

실제 응용 프로그램은 일반적으로 일부 유효성 검사 (예: 필수 필드, 최소 길이 또는 기타 비즈니스 규칙)를 요구 합니다. 적절 한 플랫폼 간 응용 프로그램은 공유 코드에서 가능한 한 많은 유효성 검사 논리를 구현 하 여 플랫폼의 기능에 따라 표시 하기 위해 유효성 검사 오류를 UI에 다시 전달 합니다.

## <a name="delete"></a>삭제

@No__t_0 및 `Update` 메서드와 달리 `Delete<T>` 메서드는 전체 `Stock` 개체가 아닌 기본 키 값만 사용할 수 있습니다. 이 예제에서는 `Stock` 개체가 메서드에 전달 되지만 Id 속성만 `Delete<T>` 메서드에 전달 됩니다.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>미리 채워진 SQLite 데이터베이스 파일 사용

일부 응용 프로그램은 이미 데이터로 채워진 데이터베이스와 함께 제공 됩니다. 앱에 기존 SQLite 데이터베이스 파일을 전달 하 고 액세스 하기 전에 쓰기 가능한 디렉터리에 복사 하 여 모바일 응용 프로그램에서이를 쉽게 수행할 수 있습니다. SQLite는 대부분의 플랫폼에서 사용 되는 표준 파일 형식 이므로 SQLite 데이터베이스 파일을 만드는 데 사용할 수 있는 여러 도구가 있습니다.

- **SQLite 관리자 Firefox 확장** &ndash; Mac 및 Windows에서 작동 하며 IOS 및 Android와 호환 되는 파일을 생성 합니다.

- **명령줄** &ndash;[Www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html)를 참조하세요.

앱과 함께 배포할 데이터베이스 파일을 만들 때 특히 이름이 C# 클래스 및 속성과 일치 해야 하는 SQLite.NET를 사용 하는 경우에는 테이블 및 열의 이름을 지정 하 여 코드에 필요한 내용과 일치 하는지 확인 합니다. 또는 연결 된 사용자 지정 특성을 지정 합니다.

일부 코드가 Android 앱에서 다른 작업 보다 먼저 실행 되도록 하기 위해 첫 번째 작업에 추가 하 여 로드 하거나 작업 이전에 로드 되는 `Application` 하위 클래스를 만들 수 있습니다. 아래 코드는 기존 데이터베이스 파일 데이터를 복사 하는 `Application` 하위 클래스를 보여 줍니다 **. sqlite** 는 **/Resources/Raw/** 디렉터리를 제외 합니다.

```csharp
[Application]
public class YourAndroidApp : Application {
    public override void OnCreate ()
    {
        base.OnCreate ();
        var docFolder = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
        Console.WriteLine ("Data path:" + Database.DatabaseFilePath);
        var dbFile = Path.Combine(docFolder, "data.sqlite"); // FILE NAME TO USE WHEN COPIED
        if (!System.IO.File.Exists(dbFile)) {
            var s = Resources.OpenRawResource(Resource.Raw.data);  // DATA FILE RESOURCE ID
            FileStream writeStream = new FileStream(dbFile, FileMode.OpenOrCreate, FileAccess.Write);
            ReadWriteStream(s, writeStream);
        }
    }
    // readStream is the stream you need to read
    // writeStream is the stream you want to write to
    private void ReadWriteStream(Stream readStream, Stream writeStream)
    {
        int Length = 256;
        Byte[] buffer = new Byte[Length];
        int bytesRead = readStream.Read(buffer, 0, Length);
        // write the required bytes
        while (bytesRead > 0)
        {
            writeStream.Write(buffer, 0, bytesRead);
            bytesRead = readStream.Read(buffer, 0, Length);
        }
        readStream.Close();
        writeStream.Close();
    }
}
```

## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Advanced (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android 데이터 조리법](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin 양식 데이터 액세스](~/xamarin-forms/data-cloud/data/databases.md)
