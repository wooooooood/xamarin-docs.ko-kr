---
title: Android 앱에서 데이터 사용
ms.prod: xamarin
ms.assetid: D5932AEB-0B6E-4F37-8B32-9BE4775AEE85
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: b79b2e44e79a6ff75b096c7443f6d46c20e27144
ms.sourcegitcommit: 797597d902330652195931dec9ac3e0cc00792c5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/20/2018
---
# <a name="using-data-in-an-app"></a>응용 프로그램의 데이터를 사용 하 여

**DataAccess_Adv** 샘플에서는 CRUD (만들기, 읽기, 업데이트 및 삭제) 데이터베이스 기능 및 사용자 입력을 허용 하는 작업 중인 응용 프로그램을 보여 줍니다. 응용 프로그램 두 화면으로 이루어져: 목록 및 데이터 입력 폼입니다. 모든 데이터 액세스 코드는 수정 하지 않고 iOS 및 Android에서 다시 사용할 수 있습니다.

일부 데이터를 추가한 후 응용 프로그램 화면은 Android에서 다음과 같이 표시.

![Android 샘플 목록](using-data-in-an-app-images/image11.png "Android 샘플 목록")

![Android 샘플 세부 정보](using-data-in-an-app-images/image12.png "Android 샘플 세부 정보")

Android 프로젝트 아래에 나와 &ndash; 이 섹션에 표시 된 코드에 포함 되어는 **Orm** 디렉터리:

![Android 프로젝트 트리](using-data-in-an-app-images/image14.png "Android 프로젝트 트리")

네이티브 UI 코드 활동 Android에 대 한이 문서에 대 한 범위를 벗어납니다. 참조는 [Android Listview 및 어댑터](~/android/user-interface/layouts/list-view/index.md) UI 컨트롤에 대 한 자세한 내용은 가이드입니다.

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

로 데이터를 렌더링 하는 android는 `ListView`합니다.

## <a name="create-and-update"></a>만들기 및 업데이트

응용 프로그램 코드를 간소화 하기 위해 save 메서드는 삽입을 수행 하는 제공 되거나 PrimaryKey 설정 되었는지 여부에 따라 업데이트 합니다. 때문에 `Id` 속성이으로 표시 되는 `[PrimaryKey]` 특성 하지 코드에 설정 해야 합니다. 이 메서드는 되었는지 여부를 확인 값 이전 (기본 키 속성을 확인 하는 중)에 의해 저장 삽입 하거나 개체를 적절 하 게 업데이트 합니다.

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

실제 응용 프로그램 (예: 필수 필드, 최소 길이 또는 기타 비즈니스 규칙) 일부 유효성 검사가 필요 합니다. 플랫폼 간 응용 프로그램 플랫폼의 기능에 따라 디스플레이 대 한 UI 백업 유효성 검사 오류를 전달 하는 공유 코드에서 최대한 논리 유효성 검사의 많은 구현 합니다.

## <a name="delete"></a>삭제

와 달리는 `Insert` 및 `Update` 메서드는 `Delete<T>` 만 기본 키 값 보다는 전체 메서드를 사용할 수 `Stock` 개체입니다. 이 예제는 `Stock` 개체를 메서드에 전달 되지만 Id 속성으로 전달 됩니다는 `Delete<T>` 메서드.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>미리 채워진된 SQLite 데이터베이스 파일 사용

일부 응용 프로그램에 미리 데이터가 채워진 데이터베이스와 함께 제공 됩니다. 쉽게이 수행할 수 있습니다 모바일 응용 프로그램에서 앱을 가진 기존 SQLite 데이터베이스 파일을 전달 하 고 쓰기 가능한 디렉터리에 액세스 하기 전에 복사 하 여 합니다. SQLite 많은 플랫폼에서 사용 되는 표준 파일 형식 이므로 SQLite 데이터베이스 파일을 만드는 데 사용할 수 있는 도구는 여러 가지가 있습니다.

-   **SQLite 관리자 Firefox 확장** &ndash; iOS 및 Android와 호환 되는 파일을 Mac 및 Windows 및 생성에 작동 합니다.

-   **명령줄** &ndash; 참조 [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) 합니다.

응용 프로그램과 함께 배포에 대 한 데이터베이스 파일을 만들 때는 테이블 및 열 이름이 일치 하는지 확인 하세요 코드의에서는, C# 클래스와 속성에 맞게 이름을 증명 있는 SQLite.NET 사용 중인 경우에 특히 to와 주의 (또는 연결 된 사용자 지정 특성)입니다.

일부 코드 Android 앱에서 다른 것 보다 먼저 실행 되도록 첫 번째 활동을 배치할 수 있습니다 또는 만들 수 있습니다는 `Application` 모든 작업 전에 로드 되어 있는 하위 클래스입니다. 다음 코드는 `Application` 기존 데이터베이스 파일을 복사 하는 하위 클래스 **data.sqlite** 부재 중는 **/Resources/Raw/** 디렉터리입니다.

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
- [DataAccess 고급 (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android 데이터 레시피](https://developer.xamarin.com/recipes/android/data/)
- [Xamarin.Forms 데이터 액세스](~/xamarin-forms/app-fundamentals/databases.md)
