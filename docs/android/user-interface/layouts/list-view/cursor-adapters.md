---
title: CursorAdapters 사용
ms.prod: xamarin
ms.assetid: 60DE467E-A5DA-4420-52E5-D86AD1678FE6
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 10/25/2017
ms.openlocfilehash: 084e1e9a4af8d7e27bee955ff4f27af28f9db08a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758681"
---
# <a name="using-cursoradapters-with-xamarinandroid"></a>Xamarin Android에서 CursorAdapters 사용

Android는 SQLite 데이터베이스 쿼리의 데이터를 표시 하는 어댑터 클래스를 제공 합니다.

 **SimpleCursorAdapter** – 서브클래싱 없이 사용할 `ArrayAdapter` 수 있기 때문에와 비슷합니다. 생성자에 필요한 매개 변수 (예: 커서 및 레이아웃 정보)를 제공한 다음에 할당 `ListView`하기만 하면 됩니다.

 **CursorAdapter** – 레이아웃 컨트롤에 대 한 데이터 값의 바인딩 (예: 컨트롤 숨기기/표시 또는 속성 변경)에 대 한 더 많은 제어가 필요한 경우 상속할 수 있는 기본 클래스입니다.

커서 어댑터는 SQLite에 저장 된 긴 데이터 목록을 스크롤 하는 고성능 방법을 제공 합니다. 소비 하는 코드는 `Cursor` 개체에서 SQL 쿼리를 정의한 다음 각 행에 대 한 뷰를 만들고 채우는 방법을 설명 해야 합니다.

## <a name="creating-an-sqlite-database"></a>SQLite 데이터베이스 만들기

커서 어댑터를 시연 하려면 간단한 SQLite 데이터베이스 구현이 필요 합니다. **SimpleCursorTableAdapter/VegetableDatabase** 의 코드에는 테이블을 만들고 일부 데이터로 채우는 코드와 SQL이 포함 되어 있습니다.
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

이 `VegetableDatabase` 클래스는 `HomeScreen` 활동의 `OnCreate` 메서드에서 인스턴스화됩니다. 기본 `SQLiteOpenHelper` 클래스는 데이터베이스 파일의 설치를 관리 하 고 해당 `OnCreate` 메서드의 SQL이 한 번만 실행 되도록 합니다. 이 클래스는 및 `SimpleCursorAdapter` `CursorAdapter`에 대 한 다음 두 가지 예제에서 사용 됩니다.

커서 쿼리에는 `_id` `CursorAdapter` 가 작동 하려면 정수 열 *이 있어야 합니다* . 기본 테이블에 라는 `_id` 정수 열이 없으면 커서를 구성 `RawQuery` 하는의 다른 고유한 정수에 대해 열 별칭을 사용 합니다. 자세한 내용은 [Android 문서](xref:Android.Widget.CursorAdapter) 를 참조 하세요.

### <a name="creating-the-cursor"></a>커서 만들기

이 예에서는를 `RawQuery` 사용 하 여 SQL 쿼리 `Cursor` 를 개체로 변환 합니다. 커서에서 반환 되는 열 목록은 커서 어댑터에서 표시할 수 있는 데이터 열을 정의 합니다. **SimpleCursorTableAdapter/HomeScreen** `OnCreate` 메서드에 데이터베이스를 만드는 코드는 다음과 같습니다.

```csharp
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null); // cursor query
StartManagingCursor(cursor);
// use either SimpleCursorAdapter or CursorAdapter subclass here!
```

을 호출 `StartManagingCursor` 하는 모든 코드는 `StopManagingCursor`도 호출 해야 합니다. 이 예에서는 `OnCreate` 를 사용 하 여 `OnDestroy` 를 시작 하 고 커서를 닫습니다. 메서드에 `OnDestroy` 는 다음 코드가 포함 되어 있습니다.

```csharp
StopManagingCursor(cursor);
cursor.Close();
```

응용 프로그램이 사용할 수 있는 SQLite 데이터베이스를 사용 하 고 표시 된 것 처럼 커서 개체를 만든 후에는 `SimpleCursorAdapter` 또는의 `CusorAdapter` 서브 클래스를 사용 `ListView`하 여에 행을 표시할 수 있습니다.

## <a name="using-simplecursoradapter"></a>SimpleCursorAdapter 사용

`SimpleCursorAdapter`는 `ArrayAdapter`와 유사 하지만 SQLite와 함께 사용 하도록 특수화 되었습니다. 서브 클래스는 필요 하지 않습니다. 개체를 만든 다음이를 `ListView`의 `Adapter` 속성에 할당 하는 경우 간단한 매개 변수를 설정 하기만 하면 됩니다.

SimpleCursorAdapter 생성자에 대 한 매개 변수는 다음과 같습니다.

 **Context** – 포함 하는 활동에 대 한 참조입니다.

 **Layout** – 사용할 행 뷰의 리소스 ID입니다.

 **ICursor** – 표시 되는 데이터에 대 한 SQLite 쿼리를 포함 하는 커서입니다.

 **에서** 문자열 배열 – 커서의 열 이름에 해당 하는 문자열 배열입니다.

 **정수 배열하려면** – 행 레이아웃의 컨트롤에 해당 하는 레이아웃 Id 배열입니다. `from` 배열에 지정 된 열의 값은 동일한 인덱스에서이 배열에 지정 된 ControlID에 바인딩됩니다.

`from` 및`to` 배열의 항목 수는 데이터 소스에서 뷰의 레이아웃 컨트롤에 대 한 매핑을 형성 하기 때문에 동일 해야 합니다.

**SimpleCursorTableAdapter/HomeScreen** 샘플 코드는 다음과 `SimpleCursorAdapter` 같이 구성 됩니다.

```csharp
// which columns map to which layout controls
string[] fromColumns = new string[] {"name"};
int[] toControlIDs = new int[] {Android.Resource.Id.Text1};
// use a SimpleCursorAdapter
listView.Adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor,
       fromColumns,
       toControlIDs);
```

`SimpleCursorAdapter`는 SQLite 데이터 `ListView`를에 표시 하는 빠르고 간단한 방법입니다. 주요 제한 사항은 열 값을 표시 하는 컨트롤에만 바인딩할 수 있으며, 컨트롤을 표시 하거나 숨기 거 나 속성을 변경 하는 등의 방법으로 행 레이아웃의 다른 측면을 변경할 수 없다는 것입니다.

## <a name="subclassing-cursoradapter"></a>CursorAdapter 하위 클래스

하위 클래스는 SQLite의 데이터를 표시 하 `SimpleCursorAdapter` 는 데 사용 하는와 동일한 성능상의 이점을 제공 하지만 각 행 뷰의 생성 및 레이아웃을 완벽 하 게 제어할 수 있습니다. `CursorAdapter` `GetView` `BaseAdapter` 구현은,`this[]` 또는 인덱서 를`GetItemId`재정의하지않으므로 서브클래싱과 매우 다릅니다. `Count` `CursorAdapter`

작동 하는 SQLite 데이터베이스의 경우 `CursorAdapter` 하위 클래스를 만들기 위해 두 가지 메서드만 재정의 하면 됩니다.

- **Bindview** – 뷰가 지정 된 경우 제공 된 커서의 데이터를 표시 하도록 업데이트 합니다.

- **Newview** –가 `ListView` 새 보기를 표시 해야 할 때 호출 됩니다. 는 `CursorAdapter` 일반 어댑터의 `GetView` 메서드와 달리 뷰를 재활용 합니다.

이전 예제의 어댑터 서브 클래스에는 행의 수를 반환 하 고 현재 항목 `CursorAdapter` 을 검색 하는 메서드가 있습니다 .이 정보는 커서 자체에서 통해 수집할 수 있으므로 이러한 메서드가 필요 하지 않습니다. 각 뷰의 생성 및 채우기를 이러한 두 메서드로 `CursorAdapter` 분할 하면 뷰가 다시 사용 됩니다. 이는 `convertView` `BaseAdapter.GetView` 메서드의 매개 변수를 무시할 수 있는 일반 어댑터와는 대조적입니다.

### <a name="implementing-the-cursoradapter"></a>CursorAdapter 구현

**CursorTableAdapter/HomeScreenCursorAdapter** 의 코드는 하위 클래스를 `CursorAdapter` 포함 합니다. `NewView` 메서드에서에 액세스할 `LayoutInflater` 수 있도록 생성자에 전달 된 컨텍스트 참조를 저장 합니다. 전체 클래스는 다음과 같습니다.

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

가 표시 되는에서 커서 `CursorAdapter` 를 만든 다음 목록 뷰에 할당 합니다. `ListView` `Activity`

**CursorTableAdapter/HomeScreen** `OnCreate` 메서드에서이 작업을 수행 하는 코드는 다음과 같습니다.

```csharp
// create the cursor
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null);
StartManagingCursor(cursor);

// create the CursorAdapter
listView.Adapter = (IListAdapter)new HomeScreenCursorAdapter(this, cursor, false);
```

메서드에 `OnDestroy` 는 앞에서 `StopManagingCursor` 설명한 메서드 호출이 포함 되어 있습니다.

## <a name="related-links"></a>관련 링크

- [SimpleCursorTableAdapter (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/simplecursortableadapter)
- [CursorTableAdapter (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/cursortableadapter)
