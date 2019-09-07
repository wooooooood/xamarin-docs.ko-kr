---
title: 사용자 지정 ContentProvider 만들기
description: 이전 섹션에서는 기본 제공 ContentProvider 구현에서 데이터를 사용 하는 방법을 살펴보았습니다. 이 섹션에서는 사용자 지정 ContentProvider를 빌드한 다음 해당 데이터를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/07/2018
ms.openlocfilehash: f72baaa4c74eb4bf0bb5eec64211d6ea2b18076c
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70756672"
---
# <a name="creating-a-custom-contentprovider"></a>사용자 지정 ContentProvider 만들기

_이전 섹션에서는 기본 제공 ContentProvider 구현에서 데이터를 사용 하는 방법을 살펴보았습니다. 이 섹션에서는 사용자 지정 ContentProvider를 빌드한 다음 해당 데이터를 사용 하는 방법을 설명 합니다._

## <a name="about-contentproviders"></a>ContentProviders 정보

콘텐츠 공급자 클래스는에서 `ContentProvider`상속 해야 합니다. 쿼리에 응답 하는 데 사용 되는 내부 데이터 저장소로 구성 되며, 코드를 사용 하 여 데이터에 대 한 유효한 요청을 수행할 수 있도록 Uri 및 MIME 형식을 상수로 노출 해야 합니다.

### <a name="uri-authority"></a>URI (기관)

`ContentProviders`는 Uri를 사용 하 여 Android에서 액세스 됩니다. 를 `ContentProvider` 노출 하는 응용 프로그램은 해당 **androidmanifest .xml** 파일에서 응답 하는 uri를 설정 합니다. 응용 프로그램이 설치 되 면 다른 응용 프로그램에서 액세스할 수 있도록 이러한 Uri가 등록 됩니다.

Android 용 Mono에서 콘텐츠 공급자 클래스 `[ContentProvider]` 에는 **androidmanifest**에 추가 해야 하는 uri를 지정 하는 특성이 있어야 합니다.

### <a name="mime-type"></a>Mime 형식

MIME 형식에 대 한 일반적인 형식은 두 부분으로 구성 됩니다. Android `ContentProviders` 는 일반적으로 MIME 형식의 첫 번째 부분에 이러한 두 문자열을 사용 합니다.

1. `vnd.android.cursor.item`단일 행을 나타내려면 코드에서 상수를 `ContentResolver.CursorItemBaseType` 사용 합니다. &ndash;

1. `vnd.android.cursor.dir`여러 행의 경우 코드에서 `ContentResolver.CursorDirBaseType` 상수를 사용 합니다. &ndash;

MIME 형식의 두 번째 부분은 응용 프로그램에 고유 하며 `vnd.` 접두사와 함께 역방향 DNS 표준을 사용 해야 합니다. 샘플 코드에서는를 `vnd.com.xamarin.sample.Vegetables`사용 합니다.

### <a name="data-model-metadata"></a>데이터 모델 메타 데이터

응용 프로그램을 사용 하려면 다양 한 형식의 데이터에 액세스 하는 Uri 쿼리를 구성 해야 합니다. 기본 Uri는 특정 데이터 테이블을 참조 하도록 확장할 수 있으며 결과를 필터링 하는 매개 변수도 포함할 수 있습니다. 결과 커서와 함께 데이터를 표시 하는 데 사용 되는 열과 절도 선언 해야 합니다.

유효한 Uri 쿼리만 생성 되도록 하려면 유효한 문자열을 상수 값으로 제공 하는 것이 일반적인 방법입니다. 이렇게 하면 코드 완성을 통해 값 `ContentProvider` 을 검색 가능 하 게 만들고 문자열의 오타를 방지할 수 있으므로에 쉽게 액세스할 수 있습니다.

이전 예제에서 클래스는 `android.provider.ContactsContract` 연락처 데이터에 대 한 메타 데이터를 노출 했습니다. 사용자 지정 `ContentProvider` 의 경우 클래스 자체에 대 한 상수만 노출 합니다.

## <a name="implementation"></a>구현

사용자 지정 `ContentProvider`를 만들고 사용 하는 세 가지 단계가 있습니다.

1. **데이터베이스 클래스 만들기** 을 구현`SQLiteOpenHelper`합니다. &ndash;

2. **데이터베이스의 인스턴스, 상수 값으로 노출 되는 메타 데이터, 데이터에 액세스 하기 위한 메서드로 구현 `ContentProvider`**  &ndash; `ContentProvider` 되는 클래스를 만듭니다.

3. **`ContentProvider`**  Uri를 통해 액세스 `ContentProvider`되는를 `CursorAdapter` 사용 하 여에 액세스 합니다 &ndash; .

앞에서 설명한 `ContentProviders` 것 처럼는 정의 된 위치 이외의 응용 프로그램에서 사용 될 수 있습니다. 이 예제에서 데이터는 동일한 응용 프로그램에서 사용 되지만 스키마 (일반적으로 상수 값으로 노출 됨)에 대 한 정보를 알고 있는 한 다른 응용 프로그램 에서도 액세스할 수 있다는 점을 명심 해야 합니다.

## <a name="create-a-database"></a>데이터베이스 만들기

대부분 `ContentProvider` 의 구현은 `SQLite` 데이터베이스를 기반으로 합니다. **SimpleContentProvider/VegetableDatabase** 의 예제 데이터베이스 코드는 다음과 같이 매우 간단한 두 개의 열로 된 데이터베이스를 만듭니다.

```csharp
class VegetableDatabase  : SQLiteOpenHelper {
  const string create_table_sql =
    "CREATE TABLE [vegetables] ([_id] INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL UNIQUE, [name] TEXT NOT NULL UNIQUE)";
  const string DatabaseName = "vegetables.db";
  const int DatabaseVersion = 1;

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
  {
    throw new NotImplementedException();
  }
}
```

데이터베이스 `ContentProvider`구현 자체에는를 사용 하 여 노출 해야 하는 특별 한 고려 사항이 필요 하지 않지만 `ContentProvider's` 데이터를 `ListView` 컨트롤에 바인딩하려면 라는 `_id` 고유한 정수 열이의 일부 여야 합니다. 결과 집합. `ListView` 컨트롤 사용에 대 한 자세한 내용은 [listview and 어댑터](~/android/user-interface/layouts/list-view/index.md) 문서를 참조 하세요.

## <a name="create-the-contentprovider"></a>ContentProvider 만들기

이 섹션의 나머지 부분에서는 **SimpleContentProvider/VegetableProvider** 예제 클래스를 빌드하는 방법에 대 한 단계별 지침을 제공 합니다.

### <a name="initialize-the-database"></a>데이터베이스 초기화

첫 번째 단계는 사용할 데이터베이스 `ContentProvider` 를 하위 클래스로 추가 하는 것입니다.

```csharp
public class VegetableProvider : ContentProvider 
{
    VegetableDatabase vegeDB;
    public override bool OnCreate()
    {
       vegeDB = new VegetableDatabase(Context);
        return true;
    }
}
```

코드의 나머지 부분에서는 데이터를 검색 하 고 쿼리할 수 있는 실제 콘텐츠 공급자 구현을 구성 합니다.

## <a name="add-metadata-for-consumers"></a>소비자에 대 한 메타 데이터 추가

`ContentProvider` 클래스에 노출할 네 가지 형식의 메타 데이터가 있습니다. Authority만 필요 하며 나머지는 규칙에 따라 수행 됩니다.

- **권한** 응용 프로그램이 설치 될 때 Android에 등록 되도록 특성을클래스에추가해야`ContentProvider`합니다. &ndash;

- **Uri** &ndash; 는`CONTENT_URI` 코드에서 쉽게 사용할 수 있도록 상수로 노출 됩니다. 이는 기관과 일치 해야 하지만 스키마 및 기본 경로를 포함 합니다.

- **MIME 형식** &ndash; 결과 목록과 단일 결과는 서로 다른 콘텐츠 형식으로 처리 되므로 두 개의 MIME 형식을 정의 하 여 해당 결과를 나타냅니다.

- **InterfaceConsts** &ndash; 각 데이터 열 이름에 대 한 상수 값을 제공 하 여 코드를 사용 하면 입력 오류를 발생 시 키 지 않고 코드를 쉽게 검색 하 고 참조할 수 있습니다.

이 코드는 이러한 각 항목을 구현 하는 방법을 보여 주며, 이전 단계에서 데이터베이스 정의를 추가 합니다.

```csharp
[ContentProvider(new string[] { CursorTableAdapter.VegetableProvider.AUTHORITY })]
public class VegetableProvider : ContentProvider 
{
   public const string AUTHORITY = "com.xamarin.sample.VegetableProvider";
   static string BASE_PATH = "vegetables";
   public static readonly Android.Net.Uri CONTENT_URI = Android.Net.Uri.Parse("content://" + AUTHORITY + "/" + BASE_PATH);
   // MIME types used for getting a list, or a single vegetable
   public const string VEGETABLES_MIME_TYPE = ContentResolver.CursorDirBaseType + "/vnd.com.xamarin.sample.Vegetables";
   public const string VEGETABLE_MIME_TYPE = ContentResolver.CursorItemBaseType + "/vnd.com.xamarin.sample.Vegetables";
   // Column names
   public static class InterfaceConsts {
       public const string Id = "_id";
       public const string Name = "name";
   }
   VegetableDatabase vegeDB;
   public override bool OnCreate()
   {
       vegeDB = new VegetableDatabase(Context);
       return true;
   }
}
```

## <a name="implement-the-uri-parsing-helper"></a>URI 구문 분석 도우미 구현

코드를 사용 하면 uri를 사용 하 여에 `ContentProvider`대 한 요청을 수행 하므로 반환할 데이터를 결정 하기 위해 해당 요청을 구문 분석할 수 있어야 합니다. 클래스 `UriMatcher` 는에서 `ContentProvider` 지 원하는 uri 패턴으로 초기화 된 후 uri를 구문 분석 하는 데 도움이 될 수 있습니다.

예제의 `UriMatcher` 는 두 개의 uri를 사용 하 여 초기화 됩니다.

1. *"VegetableProvider/야채"* &ndash; 야채의 전체 목록 반환을 요청 합니다.

2. *"VegetableProvider\#/야채/"* &ndash; 입니다. 여기서는 \# 숫자 매개 변수의 자리 표시자 ( `_id` 데이터베이스의 행)입니다. 별표 자리 표시자 ("\*")를 사용 하 여 텍스트 매개 변수를 일치 시킬 수도 있습니다.

코드에서 상수를 사용 하 여 인증 기관 및 기본\_경로와 같은 메타 데이터 값을 참조 합니다. 반환 코드는 Uri 구문 분석을 수행 하는 메서드에서 반환할 데이터를 결정 하는 데 사용 됩니다.

```csharp
const int GET_ALL = 0; // return code when list of Vegetables requested
const int GET_ONE = 1; // return code when a single Vegetable is requested by ID
static UriMatcher uriMatcher = BuildUriMatcher();
static UriMatcher BuildUriMatcher()
{
  var matcher = new UriMatcher(UriMatcher.NoMatch);
  // Uris to match, and the code to return when matched
  matcher.AddURI(AUTHORITY, BASE_PATH, GET_ALL); // all vegetables
  matcher.AddURI(AUTHORITY, BASE_PATH + "/#", GET_ONE); // specific vegetable by numeric ID
  return matcher;
}
```

이 코드는 `ContentProvider` 클래스에 대 한 모든 private입니다. 자세한 내용은 [Google의 UriMatcher 설명서](xref:Android.Content.UriMatcher) 를 참조 하세요.

## <a name="implement-the-querymethod"></a>QueryMethod 구현

`ContentProvider` 구현`Query` 하는 가장 간단한 방법은 메서드입니다. 아래 구현에서는를 사용 `UriMatcher` 하 여 `uri` 매개 변수를 구문 분석 하 고 올바른 데이터베이스 메서드를 호출 합니다. 에 `uri` ID 매개 변수가 포함 된 경우 정수는 구문 분석 되 고 ( `LastPathSegment`사용) 데이터베이스 쿼리에 사용 됩니다.

```csharp
public override Android.Database.ICursor Query(Android.Net.Uri uri, string[] projection, string selection, string[] selectionArgs, string sortOrder)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return GetFromDatabase();
  case GET_ONE:
    var id = uri.LastPathSegment;
    return GetFromDatabase(id); // the ID is the last part of the Uri
  default:
    throw new Java.Lang.IllegalArgumentException("Unknown Uri: " + uri);
  }
}
Android.Database.ICursor GetFromDatabase()
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables", null);
}
Android.Database.ICursor GetFromDatabase(string id)
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables WHERE _id = " + id, null);
}
```

또한 `GetType` 메서드를 재정의 해야 합니다. 이 메서드를 호출 하 여 지정 된 Uri에 대해 반환 되는 콘텐츠 형식을 확인할 수 있습니다.
이렇게 하면 소비 응용 프로그램에서 해당 데이터를 처리 하는 방법을 알 수 있습니다.

```csharp
public override String GetType(Android.Net.Uri uri)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return VEGETABLES_MIME_TYPE; // list
  case GET_ONE:
    return VEGETABLE_MIME_TYPE; // single item
  default:
    throw new Java.Lang.IllegalArgumentExceptoin ("Unknown Uri: " + uri);
   }
}
```

## <a name="implement-the-other-overrides"></a>다른 재정의 구현

간단한 예제에서는 데이터를 편집 하거나 삭제할 수 없지만 삽입, 업데이트 및 삭제 메서드를 구현 하 여 구현 없이 추가 해야 합니다.

```csharp
public override int Delete(Android.Net.Uri uri, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override Android.Net.Uri Insert(Android.Net.Uri uri, ContentValues values)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override int Update(Android.Net.Uri uri, ContentValues values, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
```

그러면 기본 `ContentProvider` 구현이 완료 됩니다. 응용 프로그램을 설치한 후에는 응용 프로그램 내에서 제공 하는 데이터를 응용 프로그램 내에서 사용할 수 있으며이를 참조 하는 Uri를 알고 있는 다른 응용 프로그램 에서도 사용할 수 있습니다.

## <a name="access-the-contentprovider"></a>ContentProvider 액세스

`VegetableProvider` 가 구현 된 후에는이 문서의 시작 부분에 있는 연락처 공급자와 동일한 방식으로 액세스 합니다. 지정 된 Uri를 사용 하 여 커서를 가져온 다음 어댑터를 사용 하 여 데이터에 액세스 합니다.

## <a name="bind-a-listview-to-a-contentprovider"></a>콘텐츠 공급자에 ListView 바인딩

데이터로를 `ListView` 채우려면 야채의 필터링 되지 않은 목록에 해당 하는 Uri를 사용 합니다. 코드에서는이 (가) 확인 하 `VegetableProvider.CONTENT_URI` `com.xamarin.sample.vegetableprovider/vegetables`는 상수 값을 사용 합니다. 구현 시에 바인딩할 `ListView`수 있는 커서가 반환 됩니다. `VegetableProvider.Query`

의 `SimpleContentProvider/HomeScreen.cs` 코드는에서 데이터 `ContentProvider`를 표시 하는 방법을 보여 줍니다.

```csharp
listView = FindViewById<ListView>(Resource.Id.List);
string[] projection = new string[] { VegetableProvider.InterfaceConsts.Id, VegetableProvider.InterfaceConsts.Name} ;
string[] fromColumns = new string[] { VegetableProvider.InterfaceConsts.Name };
int[] toControlIds = new int[] { Android.Resource.Id.Text1 };

// CursorLoader introduced in Honeycomb (3.0, API_11)
var loader = new CursorLoader(this,
   VegetableProvider.CONTENT_URI, projection, null, null, null);
cursor = (ICursor)loader.LoadInBackground();

// Create a SimpleCursorAdapter
adapter = new SimpleCursorAdapter(this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlIds);
listView.Adapter = adapter;
```

결과 응용 프로그램은 다음과 같습니다.

[![앱 목록 야채, 과일, 꽃 Buds, Legumes, 전구, Tubers의 스크린샷](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png#lightbox)

## <a name="retrieve-a-single-item-from-a-contentprovider"></a>ContentProvider에서 단일 항목을 검색 합니다.

소비 하는 응용 프로그램은 단일 데이터 행에도 액세스할 수 있습니다 .이 작업은 특정 행 (예:)을 참조 하는 다른 Uri를 생성 하 여 수행할 수 있습니다.

`ContentResolver` 필수`Id`를 사용 하 여 Uri를 빌드하여 단일 항목에 액세스 하려면 직접를 사용 합니다.

```csharp
Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
```

Complete 메서드는 다음과 같습니다.

```csharp
protected void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
  var id = e.Id;
  string[] projection = new string[] { "name" };
  var uri = Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
  ICursor vegeCursor = ContentResolver.Query(uri, projection, null, new string[] { id.ToString() }, null);
  string text = "";
  if (vegeCursor.MoveToFirst()) {
    text = vegeCursor.GetInt(0) + " " + vegeCursor.GetString(1);
    Android.Widget.Toast.MakeText(this, text, Android.Widget.ToastLength.Short).Show();
  }
  vegeCursor.Close();
}
```

## <a name="related-links"></a>관련 링크

- [SimpleContentProvider (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-simplecontentprovider)
