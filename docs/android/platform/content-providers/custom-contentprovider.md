---
title: "사용자 지정 ContentProvider 만들기"
description: "이전 섹션에는 기본 제공 ContentProvider 구현에서 데이터를 사용 하는 방법을 보여 줍니다. 이 섹션에서는 사용자 지정 ContentProvider 빌드하고 다음 해당 데이터를 사용 하는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: 66b956eddc48699c6fd61e9cb52a7fbc3fa70a51
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-custom-contentprovider"></a>사용자 지정 ContentProvider 만들기

_이전 섹션에는 기본 제공 ContentProvider 구현에서 데이터를 사용 하는 방법을 보여 줍니다. 이 섹션에서는 사용자 지정 ContentProvider 빌드하고 다음 해당 데이터를 사용 하는 방법을 설명 합니다._

## <a name="about-contentproviders"></a>ContentProviders에 대 한

콘텐츠 공급자 클래스에서 상속 해야 `ContentProvider`합니다. 쿼리에 응답 하는 데 사용 되는 내부 데이터 저장소가으로 구성 되어야 하 고 상수를 사용 하는 코드가 확인 데이터에 대 한 올바른 요청 Uri 및 MIME 형식을 노출 해야 것입니다.

### <a name="uri-authority"></a>URI (기관)

`ContentProviders` Uri를 사용 하 여 Android에서 액세스 됩니다. 노출 하는 응용 프로그램은 `ContentProvider` 에 응답 하는 Uri 설정의 **AndroidManifest.xml** 파일입니다. 응용 프로그램을 설치할 때 이러한 Uri는 다른 응용 프로그램에 액세스할 수 있도록 등록 됩니다.

Android 용 모노, 콘텐츠 공급자 클래스 없어야는 `[ContentProvider]` Uri (또는 Uri)를 지정 하는 특성에 추가 해야 하는 **AndroidManifest.xml**합니다.

<a name="Mime_Type" />

### <a name="mime-type"></a>Mime 형식

MIME 형식에 대 한 일반적인 형식은 두 부분으로 구성 됩니다. Android `ContentProviders` 일반적으로 MIME 형식의 첫 번째 부분에 대 한 이러한 두 문자열을 사용 합니다.

1. `vnd.android.cursor.item` &ndash; 사용 하 여 한 행을 표시 하는 `ContentResolver.CursorItemBaseType` 코드에서 상수입니다.

1. `vnd.android.cursor.dir` &ndash; 사용 하 여 여러 행에는 `ContentResolver.CursorDirBaseType` 코드에서 상수입니다.

역방향 DNS 표준에 사용 해야 응용 프로그램에 특정 MIME 형식을 두 번째 부분 되며는 `vnd.` 접두사입니다. 예제 코드에서는 사용 `vnd.com.xamarin.sample.Vegetables`합니다.

<a name="Data_Model_Metadata" />

### <a name="data-model-metadata"></a>데이터 모델 메타 데이터

소비 응용 프로그램을 서로 다른 유형의 데이터를 액세스 하는 Uri 쿼리를 구성 해야 합니다. 기본 Uri는 데이터의 특정 테이블을 참조 하도록 확장할 수 있으며 결과 필터링 하려면 매개 변수를 포함할 수도 있습니다. 열과 데이터를 표시 하기 위해 결과 커서와 함께 사용 하는 절 선언 되어야 합니다.

유효한 Uri 쿼리 생성 된,이 상수 값으로 유효한 문자열을 제공할 일반적인 합니다. 이렇게 하면 쉽게 액세스 하는 `ContentProvider` 쉽게 값 코드 완성 기능을 통해 검색할 수 있으므로 입력 문자열에 오류를 방지 합니다.

이전 예에서 `android.provider.ContactsContract` 클래스 연락처 데이터에 대 한 메타 데이터를 노출 합니다. 사용자 지정에 대 한 `ContentProvider` 우리는 방금 클래스 자체에 상수를 제공 합니다.

<a name="Implementation" />

## <a name="implementation"></a>구현

세 가지 단계를 만들고 사용 하는 사용자 지정 `ContentProvider`:

1. **데이터베이스 클래스를 만들고** &ndash; 구현 `SQLiteOpenHelper`합니다.

2. **만들기는 `ContentProvider` 클래스** &ndash; 구현 `ContentProvider` 인스턴스로 데이터베이스의 메타 데이터 상수 값 및 데이터에 액세스 하는 방법으로 노출 합니다.

3. **액세스는 `ContentProvider` 해당 Uri를 통해** &ndash; 채우기는 `CursorAdapter` 를 사용 하는 `ContentProvider`, 해당 Uri를 통해 액세스 합니다.

앞에서 설명한 대로 `ContentProviders` 정의 되어 있는 이외의 응용 프로그램에서 사용 될 수 있습니다. 이 예에서 동일한 응용 프로그램에서 데이터를 사용 하지만 다른 응용 프로그램 에서도 액세스할 수 Uri 및 스키마 (일반적으로 노출 되는 상수 값)에 대 한 정보를 모르는 상태로 염두에서에 둬야 합니다.

<a name="Create_a_database" />

## <a name="create-a-database"></a>데이터베이스 만들기

대부분 `ContentProvider` 구현에 따라 달라 집니다는 `SQLite` 데이터베이스입니다. 예제 데이터베이스 코드 **SimpleContentProvider/VegetableDatabase.cs** 같이 매우 간단한 열이 두 데이터베이스를 만듭니다.

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

자체 데이터베이스 구현으로 노출 되도록 특별 한 고려 하지 않아도 `ContentProvider`그러나 바인딩을 의도 하는 경우는 `ContentProvider's` 데이터를는 `ListView` 라는 고유한 정수 열을 다음 제어 `_id` 의 일부가 되어야 합니다는 결과 집합입니다. 참조는 [Listview 및 어댑터](~/android/user-interface/layouts/list-view/index.md) 문서에 대 한 자세한 내용은 사용 하는 `ListView` 제어 합니다.

<a name="Create_the_ContentProvider" />

## <a name="create-the-contentprovider"></a>ContentProvider 만들기

이 섹션의 나머지 부분 방법에 대 한 단계별 지침인 **SimpleContentProvider/VegetableProvider.cs** 클래스 예제 빌드 되었습니다.

<a name="Initialize_the_Database" />

### <a name="initialize-the-database"></a>데이터베이스를 초기화 합니다.

첫 번째 단계는 하위 클래스 `ContentProvider` 사용할 데이터베이스를 추가 합니다.

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

코드의 나머지 부분 하면 데이터를 검색 하 고 쿼리 하는 실제 콘텐츠 공급자 구현을 형성 됩니다.

<a name="Add_Metadata_for_Consumers" />


## <a name="add-metadata-for-consumers"></a>소비자에 대 한 메타 데이터 추가

네 가지 종류의 메타 데이터에 노출 하려는 없는 `ContentProvider` 클래스입니다. 만 기관 필요할 나머지 규칙으로 수행 됩니다.

- **기관** &ndash; 는 `ContentProvider` 특성 *해야* 것에 등록 된 Android 응용 프로그램을 설치 하는 클래스에 추가 합니다.

- **Uri** &ndash; 는 `CONTENT_URI` 코드에서 쉽게 사용할 수 있도록 상수로 노출 됩니다. 인증 기관을 일치 해야 하는데 스키마 및 기본 경로 포함 합니다.

- **MIME 형식을** &ndash; 그 인수를 나타내는 두 개의 MIME 형식을 정의 하므로 결과 및 단일 결과 목록이 서로 다른 내용 유형을으로 처리 됩니다.

- **InterfaceConsts** &ndash; 사용 하는 코드가 쉽게 검색 하 고 입력 오류의 위험 없이 참조할 수 있도록 각 데이터 열 이름에 대 한 상수 값을 제공 합니다.

이 코드에서는 이러한 각 항목 구현 하는 방법을, 보여 줍니다. 이전 단계에서 데이터베이스 정의에 추가 합니다.

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

<a name="Implement_the_URI_Parsing_Helper" />

## <a name="implement-the-uri-parsing-helper"></a>도우미를 구문 분석 하는 URI를 구현 합니다.

Uri를 사용 하 여 요청을 사용 하는 코드가 있으므로 `ContentProvider`, 반환 될 데이터를 확인 하기 위해 해당 요청을 구문 분석 해야 합니다. `UriMatcher` 클래스 Uri 구문 분석 하는 데 도움이 될 수, 하는 패턴 Uri로 초기화 된 후의 `ContentProvider` 지원 합니다.

`UriMatcher` 예제에서으로 초기화 됩니다 두 Uri:

1. *"com.xamarin.sample.VegetableProvider/vegetables"* &ndash; 야채의 전체 목록을 반환 하는 요청입니다.

2. *"com.xamarin.sample.VegetableProvider/vegetables/\#"* &ndash; 여기서는 \# 는 숫자 매개 변수 자리 표시자 (의 `_id` 데이터베이스에서 행의). 별표 자리 표시자 ("\*") 텍스트 매개 변수와 일치를 사용할 수도 있습니다.

코드를 사용 하 여 상수 기관 및 자료와 같은 메타 데이터 값을 참조할\_경로입니다. 반환 코드 Uri 반환 될 데이터를 확인 하려면 구문 분석을 수행 하는 메서드에서 사용 됩니다.

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

이 코드는 모든 개인는 `ContentProvider` 클래스입니다. 참조 [Google UriMatcher 설명서](https://developer.xamarin.com/api/type/Android.Content.UriMatcher/) 자세한 정보.

<a name="Implement_the_QueryMethod" />

## <a name="implement-the-querymethod"></a>QueryMethod 구현

가장 간단한 `ContentProvider` 메서드를 구현 하는 `Query` 메서드. 구현을 사용 하 여 아래에서 `UriMatcher` 구문 분석 하는 `uri` 매개 변수 및 올바른 데이터베이스 메서드를 호출 합니다. 경우는 `uri` 정수 아웃 구문 분석 되는 ID 매개 변수를 포함 (사용 하 여 `LastPathSegment`) 데이터베이스 쿼리에 사용 되 고 있습니다.

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

`GetType` 메서드 재정의 해야 합니다. 지정 된 Uri에 대해 반환 되는 콘텐츠 형식을 확인 하려면이 메서드를 호출할 수 있습니다.
해당 데이터를 처리 하는 방법을 소비 응용 프로그램을 알려줄 수 있습니다이 있습니다.

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

<a name="Implement_the_Other_Overrides" />

## <a name="implement-the-other-overrides"></a>다른 재정의 구현 합니다.

간단한 예제는 편집 하거나 데이터를 삭제 하기 위해 허용 하지 않지만 Insert, Update 및 Delete 메서드를 구현 해야 하는 구현 없이 추가:

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

기본 완료 `ContentProvider` 구현 합니다. 응용 프로그램 설치가 완료 된 후 되 면 노출 된 데이터에서 제공 됩니다. 둘 다 응용 프로그램 내부 될 뿐만 아니라 다른 응용 프로그램을 참조 하는 Uri를 알고 있습니다.

<a name="Access_the_ContentProvider" />

## <a name="access-the-contentprovider"></a>액세스는 ContentProvider

한 번의 `VegetableProvider` 되었습니다, 구현에 액세스 이루어진다는 동일한 방식으로이 문서의 시작 부분에 연락처 공급자: 지정된 된 Uri를 사용 하 여 커서를 가져오고 다음 어댑터를 사용 하 여 데이터에 액세스 합니다.

<a name="Bind_a_ListView_to_a_ContentProvider" />

## <a name="bind-a-listview-to-a-contentprovider"></a>ListView를 ContentProvider에 바인딩

채우는 한 `ListView` 데이터로 야채 필터링 되지 않은 목록에 해당 하는 Uri를 사용 합니다. 코드 상수 값 사용 `VegetableProvider.CONTENT_URI`, 확인 알게 되 `com.xamarin.sample.vegetableprovider/vegetables`합니다. 우리의 `VegetableProvider.Query` 구현에 바인딩할 수 있는 커서를 반환 합니다는 `ListView`합니다.

코드 `SimpleContentProvider/HomeScreen.cs` 데이터를 표시 하는 간단한 방법을 보여 줍니다.는 `ContentProvider`:

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

완성 된 응용 프로그램은 다음과 같습니다.

[![야채, 과일, 꽃 bud과, Legumes, 전구, Tubers 나열 하는 앱의 스크린 샷](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png)


<a name="Retrieve_a_Single_Item_from_a_ContentProvider" />

## <a name="retrieve-a-single-item-from-a-contentprovider"></a>ContentProvider에서 단일 항목을 검색 합니다.

소비 응용 프로그램 예를 들어 특정 행으로 참조 하는 다른 Uri를 구성 하 여 변환을 수행할 수 있는 데이터의 단일 행에 액세스 해야 할 경우가 있습니다.

사용 하 여 `ContentResolver` 필요한와 Uri를 작성 하 여 단일 항목에 액세스 하는 직접 `Id`합니다.

```csharp
Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
```

전체 메서드는 다음과 같습니다.

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

- [SimpleContentProvider (샘플)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SimpleContentProvider)
