---
title: Cursoradapters 사용
ms.prod: xamarin
ms.assetid: 60DE467E-A5DA-4420-52E5-D86AD1678FE6
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 10/25/2017
ms.openlocfilehash: fbdd0f2ea000f0cf46178c615e7526bf7f210a41
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103040"
---
# <a name="using-cursoradapters"></a>Cursoradapters 사용


## <a name="overview"></a>개요

Android는 SQLite 데이터베이스 쿼리의 데이터를에서 표시 하도록 특별히 어댑터 클래스를 제공 합니다.

 **SimpleCursorAdapter** 유사한 –에 `ArrayAdapter` 서브클래싱 없이 사용할 수 있으므로 합니다. 생성자에 필요한 매개 변수 (예: 커서 및 레이아웃 정보)를 제공 하기만 하면 됩니다 하 고 다음에 할당 된 `ListView`합니다.

 **CursorAdapter** – 때 더 본격적인 제어가 필요한 데이터 바인딩을 통해에서 상속할 수 있는 기본 클래스를 레이아웃 컨트롤 (예: 숨기기/표시 컨트롤 또는 해당 속성을 변경) 값입니다.

커서 어댑터 SQLite에 저장 된 데이터의 긴 목록을 스크롤하여 방법은 고성능을 제공 합니다. 사용 하는 코드에서 SQL 쿼리를 정의 해야 합니다는 `Cursor` 개체 및 다음 만들고 각 행에 대 한 뷰를 채우는 방법에 설명 합니다.


## <a name="creating-an-sqlite-database"></a>SQLite 데이터베이스를 만드는 방법

커서 어댑터를 보여 주기 위해 간단한 SQLite 데이터베이스 구현이 필요 합니다. 코드 **SimpleCursorTableAdapter/VegetableDatabase.cs** SQL 테이블을 만들고 일부 데이터를 입력 하 고 코드를 포함 합니다.
전체 `VegetableDatabase` 클래스는 다음과 같습니다.

```csharp
class VegetableDatabase  : SQLiteOpenHelper {
   public static readonly string create_table_sql =
       "CREATE TABLE [vegetables] ([_id] INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL UNIQUE, [name] TEXT NOT NULL UNIQUE)";
   public static readonly string DatabaseName = "vegetables.db";
   public static readonly int DatabaseVersion = 1;
   public VegetableDatabase(Context context) : base(context, DatabaseName, null, DatabaseVersion) { }
   public override void OnCreate(SQLiteDatabase db)
   {
       db.ExecSQL(create_table_sql);
       // seed with data
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Vegetables')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Fruits')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Flower Buds')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Legumes')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Bulbs')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Tubers')");
   }
   public override void OnUpgrade(SQLiteDatabase db, int oldVersion, int newVersion)
   {   // not required until second version :)
       throw new NotImplementedException();
   }
}
```

`VegetableDatabase` 클래스에서 인스턴스화되는 `OnCreate` 메서드의 `HomeScreen` 활동입니다. 합니다 `SQLiteOpenHelper` 기본 클래스 데이터베이스 파일의 설치를 관리 하 고 SQL에서 되도록 해당 `OnCreate` 메서드는 한 번만 실행 합니다. 이 클래스에 대 한 다음 두 예제에서 사용 됩니다 `SimpleCursorAdapter` 고 `CursorAdapter`입니다.

커서 쿼리에 *해야 합니다* 정수 열이 있는 `_id` 에 대 한는 `CursorAdapter` 작동 하도록 합니다. 기본 테이블에 명명 된 정수 열이 없는 경우 `_id` 그런 다음 다른 고유한 정수에 대 한 열 별칭을 사용 하 여는 `RawQuery` 커서를 구성 합니다. 참조 된 [Android docs](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/) 대 한 자세한 내용은 합니다.


### <a name="creating-the-cursor"></a>커서 만들기

예제에서는 사용을 `RawQuery` 에 SQL 쿼리를 설정 하는 `Cursor` 개체입니다. 커서에서 반환 되는 열은 커서 어댑터에 표시 하기 위해 사용할 수 있는 데이터 열을 정의 합니다. 데이터베이스를 만드는 코드를 **SimpleCursorTableAdapter/HomeScreen.cs** `OnCreate` 메서드는 다음과 같습니다.

```csharp
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null); // cursor query
StartManagingCursor(cursor);
// use either SimpleCursorAdapter or CursorAdapter subclass here!
```

호출 하는 코드가 `StartManagingCursor` 도 호출 해야 `StopManagingCursor`합니다. 사용 예제 `OnCreate` 를 시작 하려면 및 `OnDestroy` 를 커서를 닫습니다. `OnDestroy` 메서드에이 코드를 포함 합니다.

```csharp
StopManagingCursor(cursor);
cursor.Close();
```

하거나 사용할 수 응용 프로그램 SQLite 데이터베이스가 제공 하 고 표시 된 것 처럼 커서 개체를 만들었습니다를 `SimpleCursorAdapter` 또는 하위 클래스 `CusorAdapter` 의 행을 표시 하는 `ListView`합니다.


## <a name="using-simplecursoradapter"></a>SimpleCursorAdapter를 사용 하 여

`SimpleCursorAdapter` 비슷합니다는 `ArrayAdapter`, SQLite 사용 하지만 특수 합니다. 서브클래싱 필요 – 개체를 만들 때 몇 가지 간단한 매개 변수를 설정 하기만 하 고 하지 않습니다 다음에 할당 된 `ListView`의 `Adapter` 속성입니다.

SimpleCursorAdapter 생성자의 매개 변수는:

 **상황에 맞는** – 포함 활동에 대 한 참조입니다.

 **레이아웃** – 사용할 행 보기의 리소스 ID입니다.

 **ICursor** -SQLite 쿼리를 표시할 데이터를 포함 하는 커서입니다.

 **에서** 문자열 배열 – 커서의 열 이름에 해당 하는 문자열 배열입니다.

 **정수 배열하려면** – 행 레이아웃의 컨트롤에 해당 하는 레이아웃 Id 배열입니다. 에 지정 된 열의 값을 `from` 배열 동일한 인덱스에 있는이 배열에 지정 된 ControlID에 바인딩됩니다.

합니다 `from` 및 `to` 뷰에서 데이터 원본의 레이아웃 컨트롤에 대 한 매핑을 하므로 배열 항목 수가 있어야 합니다.

합니다 **SimpleCursorTableAdapter/HomeScreen.cs** 위쪽 와이어 코드 샘플을 `SimpleCursorAdapter` 같이:

```csharp
// which columns map to which layout controls
string[] fromColumns = new string[] {"name"};
int[] toControlIDs = new int[] {Android.Resource.Id.Text1};
// use a SimpleCursorAdapter
listView.Adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor,
       fromColumns,
       toControlIDs);
```

`SimpleCursorAdapter` SQLite 데이터를 표시 하려면 빠르고 간단한 방법이 `ListView`합니다. 주요 제한 사항은 컨트롤에 표시할 열 값만 바인딩할 수 있습니다 (예를 들어, 표시/숨기기 컨트롤 또는 속성을 변경) 행 레이아웃의 다른 측면을 변경 하려면 하면 메시지를 표시 하지 않습니다.


## <a name="subclassing-cursoradapter"></a>CursorAdapter 서브클래싱

A `CursorAdapter` 서브 클래스에 동일한 성능 혜택을 `SimpleCursorAdapter` 만들고 각 행 뷰의 레이아웃을 완전히 제어할에서는 SQLite에서 데이터 표시에 대 한 합니다. 합니다 `CursorAdapter` 구현은 서브클래싱 다르지 `BaseAdapter` 재정의 하지 않으므로 `GetView`를 `GetItemId`를 `Count` 또는 `this[]` 인덱서 합니다.

작동 하는 지정 된 SQLite 데이터베이스에 하면 만들려는 두 메서드를 재정의 하는 `CursorAdapter` 하위 클래스입니다.

- **BindView** – 제공 된 커서의 데이터를 표시 하도록 업데이트 뷰를 지정 합니다.

- **NewView** – 될 때 호출 된 `ListView` 표시할 새 보기를 필요 합니다. 합니다 `CursorAdapter` 뷰를 재생 처리 됩니다 (달리는 `GetView` 일반 어댑터 메서드).

이전 예제에서 어댑터 서브 클래스는 행의 수를 반환 하는 방법과 – 현재 항목을 검색 하는 `CursorAdapter` 커서 자체에서 해당 정보 정보를 수집할 수 있으므로 이러한 메서드를 필요 하지 않습니다. 만들고 이러한 두 개의 메서드로 각 뷰의 채우기 분할 하 여는 `CursorAdapter` 보기 사용 하 여 다시 적용 합니다. 이 반면 일반 어댑터에 무시할 수는 `convertView` 의 매개 변수는 `BaseAdapter.GetView` 메서드.


### <a name="implementing-the-cursoradapter"></a>CursorAdapter 구현

코드 **CursorTableAdapter/HomeScreenCursorAdapter.cs** 포함 된 `CursorAdapter` 하위 클래스입니다. 액세스할 수 있도록 생성자에 전달 된 컨텍스트 참조를 저장 한 `LayoutInflater` 에 `NewView` 메서드. 완전 한 클래스는 다음과 같습니다.

```csharp
public class HomeScreenCursorAdapter : CursorAdapter {
   Activity context;
   public HomeScreenCursorAdapter(Activity context, ICursor c)
       : base(context, c)
   {
       this.context = context;
   }
   public override void BindView(View view, Context context, ICursor cursor)
   {
       var textView = view.FindViewById<TextView>(Android.Resource.Id.Text1);
       textView.Text = cursor.GetString(1); // 'name' is column 1 in the cursor query
   }
   public override View NewView(Context context, ICursor cursor, ViewGroup parent)
   {
       return this.context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, parent, false);
   }
}
```


### <a name="assigning-the-cursoradapter"></a>CursorAdapter 할당

에 `Activity` 표시할를 `ListView`, 커서를 만들지 및 `CursorAdapter` 목록 보기에 할당할 수 있습니다.

이 작업을 수행 하는 코드를 **CursorTableAdapter/HomeScreen.cs** `OnCreate` 메서드는 다음과 같습니다.

```csharp
// create the cursor
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null);
StartManagingCursor(cursor);

// create the CursorAdapter
listView.Adapter = (IListAdapter)new HomeScreenCursorAdapter(this, cursor, false);
```

`OnDestroy` 메서드를 포함 합니다 `StopManagingCursor` 앞에서 설명한 메서드 호출 합니다.



## <a name="related-links"></a>관련 링크

- [SimpleCursorTableAdapter (샘플)](https://developer.xamarin.com/samples/SimpleCursorTableAdapter/)
- [CursorTableAdapter (샘플)](https://developer.xamarin.com/samples/CursorTableAdapter/)
