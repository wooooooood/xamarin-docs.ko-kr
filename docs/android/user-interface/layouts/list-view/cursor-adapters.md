---
title: "CursorAdapters를 사용 하 여"
ms.topic: article
ms.prod: xamarin
ms.assetid: 60DE467E-A5DA-4420-52E5-D86AD1678FE6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/25/2017
ms.openlocfilehash: 43d1ef53933ca7867b834dbf118ec730ccbf71ac
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="using-cursoradapters"></a>CursorAdapters를 사용 하 여

<a name="overview" />

## <a name="overview"></a>개요

Android는 데이터 SQLite 데이터베이스 쿼리를 표시 하도록 특별히 어댑터 클래스를 제공 합니다.

 **SimpleCursorAdapter** -유사 하는 `ArrayAdapter` 서브클래싱 없이 사용할 수 없기 때문에 있습니다. 생성자에 필요한 매개 변수 (예: 커서 및 레이아웃 정보)를 제공 하기만 하면 한 다음에 할당 한 `ListView`합니다.

 **CursorAdapter** – 때 필요한 세부적으로 제어할 수 데이터 바인딩을 통해에서 상속할 수 있는 기본 클래스를 레이아웃 컨트롤 (예를 들어 컨트롤 숨기기/표시 또는 해당 속성을 변경) 값입니다.

커서 어댑터 SQLite에 저장 된 데이터의 긴 목록을 통해 스크롤할 수는 성능이 뛰어난 방법을 제공 합니다. 사용 하는 코드에서 SQL 쿼리를 정의 해야 합니다는 `Cursor` 개체를 다음 만들고 각 행에 대 한 보기를 입력 하는 방법에 설명 합니다.

<a name="Creating_an_SQLite_Database" />

## <a name="creating-an-sqlite-database"></a>SQLite 데이터베이스 만들기

커서 어댑터를 보여 주기 위해 간단한 SQLite 데이터베이스 구현이 필요 합니다. 코드 **SimpleCursorTableAdapter/VegetableDatabase.cs** 코드와 SQL 테이블을 만들고 일부 데이터를 채웁니다을 포함 합니다.
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

`VegetableDatabase` 클래스에서 인스턴스화되는 `OnCreate` 의 메서드는 `HomeScreen` 활동입니다. `SQLiteOpenHelper` 기본 클래스는 데이터베이스 파일의 설치를 관리 하 고에서 SQL을 사용 하면 해당 `OnCreate` 메서드를 한 번만 실행 합니다. 에 대 한 다음 두 예제에서이 클래스는 사용 `SimpleCursorAdapter` 및 `CursorAdapter`합니다.

커서 쿼리에 *해야* 는 정수 열이 있어야 `_id` 에 대 한는 `CursorAdapter` 에서 실행 되도록 합니다. 기본 테이블에 명명 된 정수 열이 없는 경우 `_id` 다음에 다른 고유한 정수에 대 한 열 별칭을 사용 하 여는 `RawQuery` 커서를 구성 합니다. 참조는 [Android docs](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/) 자세한 정보.

<a name="Creating_the_Cursor" />

### <a name="creating-the-cursor"></a>커서 만들기

예제에서는 사용는 `RawQuery` 에 SQL 쿼리를 설정 하는 `Cursor` 개체입니다. 커서에서 반환 되는 열 목록 커서 어댑터에 표시 하기 위해 사용할 수 있는 데이터 열을 정의 합니다. 데이터베이스를 만드는 코드는 **SimpleCursorTableAdapter/HomeScreen.cs** `OnCreate` 메서드는 다음과 같습니다.

```csharp
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null); // cursor query
StartManagingCursor(cursor);
// use either SimpleCursorAdapter or CursorAdapter subclass here!
```

호출 하는 코드가 `StartManagingCursor` 호출 또한 해야 `StopManagingCursor`합니다. 예제에서는 사용 `OnCreate` 를 시작 하려면 및 `OnDestroy` 를 커서를 닫습니다. `OnDestroy` 메서드에이 코드를 포함 합니다.

```csharp
StopManagingCursor(cursor);
cursor.Close();
```

중 하나 사용 되 면 응용 프로그램이 사용할 수 있는 SQLite 데이터베이스 하 나와 있는 것 처럼 커서 개체를 만들었습니다는 `SimpleCursorAdapter` 또는 하위 클래스 `CusorAdapter` 에서 행을 표시 하는 `ListView`합니다.

<a name="Using_SimpleCursorAdapter" />

## <a name="using-simplecursoradapter"></a>SimpleCursorAdapter를 사용 하 여

`SimpleCursorAdapter` 비슷합니다는 `ArrayAdapter`, SQLite와 함께 사용할 하지만 특수 합니다. 않습니다 하지 서브클래싱 필요 – 개체를 만들 때 몇 가지 간단한 매개 변수를 설정 하기만 하 고 다음에 할당 한 `ListView`의 `Adapter` 속성입니다.

SimpleCursorAdapter 생성자에 대 한 매개 변수는.

 **상황에 맞는** – 포함 활동에 대 한 참조입니다.

 **레이아웃** – 사용할 행 보기의 리소스 ID입니다.

 **ICursor** – 표시할 데이터에 대 한 SQLite 쿼리를 포함 하는 커서입니다.

 **에서** 문자열 배열 – 커서의 열 이름에 해당 하는 문자열 배열입니다.

 **정수 배열하려면** – 행 레이아웃의 컨트롤에 해당 하는 레이아웃 Id 배열입니다. 에 지정 된 열의 값은 `from` 배열 같은 인덱스에이 배열에 지정 된 ControlID에 바인딩됩니다.

`from` 및 `to` 뷰의 레이아웃 컨트롤에 데이터 소스에서 매핑을 형성 하므로 배열은 동일한 항목 수를 가져야 합니다.

**SimpleCursorTableAdapter/HomeScreen.cs** 코드 와이어를 샘플링 한 `SimpleCursorAdapter` 다음과 같이:

```csharp
// which columns map to which layout controls
string[] fromColumns = new string[] {"name"};
int[] toControlIDs = new int[] {Android.Resource.Id.Text1};
// use a SimpleCursorAdapter
listView.Adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor,
       fromColumns,
       toControlIDs);
```

`SimpleCursorAdapter` SQLite 데이터를 표시할 빠르고 간편한 방법이 `ListView`합니다. 기본 제한은 컨트롤을 표시 하는 열 값만 바인딩할 수 있습니다, 행 레이아웃 (예를 들어 표시/숨기기 컨트롤 속성 또는 변경)의 다른 부분을 변경할 수 있도록 메시지를 표시 하지 않습니다.

<a name="Subclassing_CursorAdapter" />

## <a name="subclassing-cursoradapter"></a>CursorAdapter 서브클래싱

A `CursorAdapter` 하위 클래스는 이점이 동일한 성능으로는 `SimpleCursorAdapter` 에 완벽 하 게 생성 및 각 행 뷰의 레이아웃 제어에서는 SQLite 하지만의 데이터를 표시 합니다. `CursorAdapter` 구현에서에서 동작은 전혀 다릅니다 서브클래싱 `BaseAdapter` 재정의 하지 않으므로 `GetView`, `GetItemId`, `Count` 또는 `this[]` 인덱서 합니다.

작동 하는 지정 된 SQLite 데이터베이스에 필요를 만드는 두 가지 메서드를 재정의 하는 `CursorAdapter` 서브 클래스:

- **BindView** – 제공 된 커서에 데이터를 표시 하도록 업데이트 뷰를 지정 합니다.

- **NewView** – 때 호출 된 `ListView` 표시할 새 보기를 필요 합니다. `CursorAdapter` 하므로 뷰 재활용 (달리는 `GetView` 일반 어댑터에서 메서드).

이전 예제에서 어댑터 하위 클래스는 행의 수를 반환 하 고 현재 항목-을 검색 하는 메서드가 `CursorAdapter` 커서 자체에서 해당 정보를 수집할 수 있으므로 이러한 메서드를 필요 하지 않습니다. 만들고 채우는 각 뷰의 이러한 두 개의 메서드로 분할 하 여는 `CursorAdapter` 보기 사용 하 여 다시 적용 합니다. 이 통해 반대로 일반 어댑터를 무시 하려면 수는 `convertView` 의 매개 변수는 `BaseAdapter.GetView` 메서드.

<a name="Implementing_the_CursorAdapter" />

### <a name="implementing-the-cursoradapter"></a>CursorAdapter 구현

코드 **CursorTableAdapter/HomeScreenCursorAdapter.cs** 포함 한 `CursorAdapter` 하위 클래스입니다. 액세스할 수 있도록 생성자에 전달 된 컨텍스트 참조를 저장 한 `LayoutInflater` 에 `NewView` 메서드. 완전 한 클래스는 다음과 같습니다.

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

<a name="Assigning_the_CursorAdapter" />

### <a name="assigning-the-cursoradapter"></a>CursorAdapter 할당

에 `Activity` 을 표시할는 `ListView`를 커서가 생성 및 `CursorAdapter` 목록 보기에 할당 합니다.

이 작업을 수행 하는 코드는 **CursorTableAdapter/HomeScreen.cs** `OnCreate` 메서드는 다음과 같습니다.

```csharp
// create the cursor
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null);
StartManagingCursor(cursor);

// create the CursorAdapter
listView.Adapter = (IListAdapter)new HomeScreenCursorAdapter(this, cursor, false);
```

`OnDestroy` 메서드에 포함 되어는 `StopManagingCursor` 앞에서 설명한 메서드를 호출 합니다.



## <a name="related-links"></a>관련 링크

- [SimpleCursorTableAdapter (샘플)](https://developer.xamarin.com/samples/SimpleCursorTableAdapter/)
- [CursorTableAdapter (샘플)](https://developer.xamarin.com/samples/CursorTableAdapter/)
