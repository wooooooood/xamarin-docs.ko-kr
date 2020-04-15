---
title: 사용자 지정 ContentProvider 만들기
description: 이전 섹션에서는 기본 제공 ContentProvider 구현에서 데이터를 사용하는 방법을 살펴보았습니다. 이 섹션에서는 사용자 지정 ContentProvider를 빌드한 다음, 해당 데이터를 사용하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/07/2018
ms.openlocfilehash: 3e57e0cd2fa87db8035fa68995b69f231151fa09
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73020533"
---
# <a name="creating-a-custom-contentprovider"></a>사용자 지정 ContentProvider 만들기

_이전 섹션에서는 기본 제공 ContentProvider 구현에서 데이터를 사용하는 방법을 살펴보았습니다. 이 섹션에서는 사용자 지정 ContentProvider를 빌드한 다음, 해당 데이터를 사용하는 방법을 설명합니다._

## <a name="about-contentproviders"></a>ContentProviders 정보

콘텐츠 공급자 클래스는 `ContentProvider`에서 상속되어야 합니다. 쿼리에 응답하는 데 사용되는 내부 데이터 저장소로 구성되며 코드를 사용하여 데이터에 대한 유효한 요청을 수행할 수 있도록 URI 및 MIME 형식을 상수로 노출해야 합니다.

### <a name="uri-authority"></a>URI(기관)

`ContentProviders`는 URI를 사용하여 Android에서 액세스됩니다. `ContentProvider`를 노출하는 애플리케이션은 **AndroidManifest.xml** 파일에서 응답할 URI를 설정합니다. 애플리케이션이 설치되면 다른 애플리케이션에 액세스할 수 있도록 이러한 URI가 등록됩니다.

Android용 Mono에서 콘텐츠 공급자 클래스에는 **AndroidManifest.xml**에 추가할 URI(또는 URIs)를 지정하는 `[ContentProvider]` 특성이 있어야 합니다.

### <a name="mime-type"></a>Mime 형식

MIME 형식의 일반적인 형식은 두 부분으로 구성됩니다. Android `ContentProviders`는 일반적으로 MIME 형식의 첫 번째 부분에 이러한 두 문자열을 사용합니다.

1. `vnd.android.cursor.item` &ndash; 단일 행을 나타내려면 코드에서 `ContentResolver.CursorItemBaseType` 상수를 사용합니다.

1. `vnd.android.cursor.dir` &ndash; 여러 행의 경우 코드에서 `ContentResolver.CursorDirBaseType` 상수를 사용합니다.

MIME 형식의 두 번째 부분은 애플리케이션에 따라 다르며, `vnd.` 접두사가 있는 역방향 DNS 표준을 사용해야 합니다. 샘플 코드는 `vnd.com.xamarin.sample.Vegetables`를 사용합니다.

### <a name="data-model-metadata"></a>데이터 모델 메타데이터

애플리케이션을 사용하려면 다양한 형식의 데이터에 액세스하는 URI 쿼리를 구성해야 합니다. 기본 URI는 특정 데이터 테이블을 참조하도록 확장할 수 있으며 결과를 필터링하는 매개 변수도 포함할 수 있습니다. 결과 커서와 함께 데이터를 표시하는 데 사용되는 열과 절도 선언해야 합니다.

유효한 URI 쿼리만 구성되도록 하려면 유효한 문자열을 상수 값으로 제공하는 것이 일반적인 방법입니다. 이렇게 하면 코드 완성을 통해 값을 검색할 수 있고 문자열의 오타를 방지할 수 있으므로 `ContentProvider`에 보다 쉽게 액세스할 수 있습니다.

이전 예제에서 `android.provider.ContactsContract` 클래스는 연락처 데이터에 대한 메타데이터를 노출했습니다. 사용자 지정 `ContentProvider`의 경우 클래스 자체에 대한 상수만 노출합니다.

## <a name="implementation"></a>구현

사용자 지정 `ContentProvider`를 만들고 사용하는 데는 세 가지 단계가 있습니다.

1. `SQLiteOpenHelper`를 구현하는 **데이터베이스 클래스를 만듭니다** &ndash;.

2. 데이터베이스 인스턴스, 상수 값으로 노출되는 메타데이터, 데이터에 액세스하는 메서드로 노출되는 메타데이터를 사용하여 `ContentProvider`를 구현하는  **`ContentProvider` 클래스를 만듭니다** &ndash;.

3. **해당 URI를 통해 `ContentProvider`에 액세스** &ndash; 해당 URI를 통해 액세스하는 `ContentProvider`를 사용하여 `CursorAdapter`를 채웁니다.

앞에서 설명한 것처럼 `ContentProviders`는 정의된 위치 이외에 애플리케이션에서 사용할 수 있습니다. 이 예제에서 데이터는 동일한 애플리케이션에서 사용되지만 URI 및 스키마에 대한 정보(일반적으로 상수 값으로 노출됨)를 알고 있는 한 다른 애플리케이션에서도 액세스할 수 있습니다.

## <a name="create-a-database"></a>데이터베이스 만들기

대부분의 `ContentProvider` 구현은 `SQLite` 데이터베이스를 기반으로 합니다. **SimpleContentProvider/VegetableDatabase.cs**의 예제 데이터베이스 코드는 다음과 같이 매우 간단한 두 개의 열로된 데이터베이스를 만듭니다.

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

데이터베이스 구현 자체에는 `ContentProvider`와 함께 노출하기 위한 특별한 고려 사항이 필요하지 않지만, `ContentProvider's` 데이터를 `ListView` 컨트롤에 바인딩하려면 `_id`라는 고유한 정수 열이 결과 집합의 일부여야 합니다. `ListView` 컨트롤 사용에 대한 자세한 내용은 [ListViews 및 Adapters](~/android/user-interface/layouts/list-view/index.md) 문서를 참조하세요.

## <a name="create-the-contentprovider"></a>ContentProvider 만들기

이 섹션의 나머지 부분에서는 **SimpleContentProvider/VegetableProvider.cs** 예제 클래스를 빌드하는 방법에 대한 단계별 지침을 제공합니다.

### <a name="initialize-the-database"></a>데이터베이스 초기화

첫 번째 단계는 `ContentProvider`를 서브클래싱하고 사용할 데이터베이스를 추가하는 것입니다.

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

코드의 나머지 부분에서는 데이터를 검색하고 쿼리할 수 있는 실제 콘텐츠 공급자 구현을 구성합니다.

## <a name="add-metadata-for-consumers"></a>소비자를 위한 메타데이터 추가

`ContentProvider` 클래스에 노출할 네 가지 유형의 메타데이터가 있습니다. 기관만 필요하며 나머지는 규칙에 따라 수행됩니다.

- **기관** &ndash; 애플리케이션 설치 시 Android에 등록되도록 `ContentProvider` 특성을 클래스에 추가*해야* 합니다.

- **URI** &ndash; `CONTENT_URI`는 코드에서 쉽게 사용할 수 있도록 상수로 노출되어 있습니다. 기관과 일치해야 하지만 스키마 및 기본 경로를 포함해야 합니다.

- **MIME 형식** &ndash; 결과 목록과 단일 결과는 서로 다른 콘텐츠 형식으로 처리되므로 두 개의 MIME 형식을 정의하여 이를 나타냅니다.

- **InterfaceConsts** &ndash; 각 데이터 열 이름에 대해 상수 값을 제공하여 코드를 사용하는 것이 입력 오류를 방지하고 쉽게 검색하고 참조할 수 있습니다.

이 코드는 이전 단계에서 데이터베이스 정의에 추가되어 이러한 각 항목이 구현되는 방법을 보여줍니다.

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

코드를 사용하면 URI를 통해 `ContentProvider` 요청을 수행하므로 반환할 데이터를 결정하기 위해 해당 요청을 구문 분석할 수 있어야 합니다. `UriMatcher` 클래스는 `ContentProvider`에서 지원되는 URI 패턴을 사용하여 URI를 초기화한 후 URIs를 구문 분석하는 데 도움이 될 수 있습니다.

예제의 `UriMatcher`는 두 개의 URIs를 사용하여 초기화됩니다.

1. *"com.xamarin.sample.VegetableProvider/vegetables"* &ndash; 전체 야채 목록을 반환하는 요청입니다.

2. *"com.xamarin.sample.VegetableProvider/vegetables/\#"* &ndash; 여기서 \#는 숫자 매개 변수(데이터베이스에 있는 행의 `_id`)의 자리 표시자입니다. 별표 자리 표시자("\*")를 사용하여 텍스트 매개 변수를 일치시킬 수도 있습니다.

코드에서 상수를 사용하여 AUTHORITY 및 BASE\_PATH와 같은 메타데이터 값을 참조합니다. 반환 코드는 URI 구문 분석을 수행하는 메서드에서 반환할 데이터를 결정하는 데 사용됩니다.

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

이 코드는 모두 `ContentProvider` 클래스 전용입니다. 자세한 내용은 [Google의 UriMatcher 설명서](xref:Android.Content.UriMatcher)를 참조하세요.

## <a name="implement-the-querymethod"></a>QueryMethod 구현

구현하는 가장 간단한 `ContentProvider` 메서드는 `Query` 메서드입니다. 아래 구현에서는 `UriMatcher`를 사용하여 `uri` 매개 변수를 구문 분석하고 올바른 데이터베이스 메서드를 호출합니다. `uri`에 ID 매개 변수가 포함된 경우 정수가 구문 분석(`LastPathSegment` 사용)되고 데이터베이스 쿼리에 사용됩니다.

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

`GetType` 메서드도 재정의해야 합니다. 이 메서드를 호출하여 지정된 URI에 대해 반환되는 콘텐츠 형식을 확인할 수 있습니다.
이렇게 하면 사용 애플리케이션에 해당 데이터를 처리하는 방법을 알려줄 수 있습니다.

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

## <a name="implement-the-other-overrides"></a>기타 재정의 구현

간단한 예제에서는 데이터를 편집하거나 삭제할 수 없지만 구현 없이 추가하려면 삽입, 업데이트 및 삭제 메서드를 구현해야 합니다.

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

그러면 기본 `ContentProvider` 구현이 완료됩니다. 애플리케이션이 설치되면 노출된 데이터는 애플리케이션 내부뿐만 아니라 URI를 참조하는 다른 애플리케이션에서도 사용할 수 있습니다.

## <a name="access-the-contentprovider"></a>ContentProvider 액세스

`VegetableProvider`가 구현되면 이 문서의 시작 부분에 있는 연락처 공급자와 동일한 방식으로 액세스합니다. 지정된 URI를 사용하여 커서를 가져온 다음, 어댑터를 사용하여 데이터에 액세스합니다.

## <a name="bind-a-listview-to-a-contentprovider"></a>ContentProvider에 ListView 바인딩

`ListView`에 데이터를 채우려면 필터링되지 않은 야채 목록에 해당하는 URI를 사용합니다. 코드에서 `com.xamarin.sample.vegetableprovider/vegetables`로 확인되는 상수 값 `VegetableProvider.CONTENT_URI`를 사용합니다. `VegetableProvider.Query` 구현은 `ListView`에 바인딩될 수 있는 커서를 반환합니다.

`SimpleContentProvider/HomeScreen.cs` 코드는 `ContentProvider`의 데이터를 간단히 표시하는 방법을 보여줍니다.

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

결과 애플리케이션은 다음과 같습니다.

[![야채, 과일, 꽃다발, Legumes, 전구, Tubers를 나열한 앱의 스크린샷](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png#lightbox)

## <a name="retrieve-a-single-item-from-a-contentprovider"></a>ContentProvider에서 단일 항목 검색

사용 애플리케이션은 단일 데이터 행에도 액세스할 수 있습니다. 이 작업은 특정 행(예:)을 참조하는 다른 URI를 구성하여 수행할 수 있습니다.

필요한 `Id`를 통해 URI를 빌드하여 단일 항목에 액세스하려면 `ContentResolver`를 직접 사용합니다.

```csharp
Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
```

complete 메서드는 다음과 같습니다.

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

- [SimpleContentProvider(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-simplecontentprovider)
