---
title: "연락처 ContentProvider를 사용 하 여"
ms.topic: article
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: 730cc1f815641d79350784790e3b33b743d1aebe
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="using-the-contacts-contentprovider"></a>연락처 ContentProvider를 사용 하 여

코드를 사용 하 여에 의해 노출 되는 데이터에 액세스 하는 `ContentProvider` 에 대 한 참조가 필요 하지 않습니다는 `ContentProvider` 전혀 클래스입니다. Uri가 제공 하는 데이터를 통해 커서를 만드는 데 사용 되는 대신,는 `ContentProvider`합니다. Android Uri를 사용 하 여 노출 하는 응용 프로그램에 대 한 시스템을 검색 하는 `ContentProvider` 해당 식별자를 사용 합니다. 와 같은 Uri는 역방향 DNS 형식에 일반적으로 문자열로, `com.android.contacts/data`합니다.

이 문자열은 Android 기억 하는 개발자가 연결 하는 대신 *연락처* 해당 메타 데이터를 노출 하는 공급자는 `android.provider.ContactsContract` 클래스입니다. Uri를 확인 하려면이 클래스는 사용 된 `ContentProvider` 쿼리할 수 있는 열과 테이블의 이름을 뿐만 아니라 합니다.

일부 데이터 형식에 액세스할 수 있는 특별 한 권한이 있어야 합니다. 제공 되는 연락처 목록 필요는 `android.permission.READ_CONTACTS` 에서 권한을 **AndroidManifest.xml** 파일입니다.

Uri에서 커서를 만들려고 하는 방법은 세 가지가 있습니다.

1. **ManagedQuery()** &ndash; 선호 되는 Android 2.3 (API 수준 10) 및 이전 버전에서는 방법은 `ManagedQuery` 는 커서를 반환 하 고 데이터를 새로 고쳐 하 고 커서를 닫고를 자동으로 관리 합니다. 이 메서드는 Android 3.0 (API 수준 11)에서 사용 되지 않습니다.

1. **ContentResolver.Query()** &ndash; 이므로 새로 고칠를 코드에서 명시적으로 종료 해야 합니다는 관리 되지 않는 커서를 반환 합니다.

1. **CursorLoader() 합니다. LoadInBackground()** &ndash; Android 3.0 (API 수준 11)에 도입 된 `CursorLoader` 는 이제 사용 하는 기본 방법은 `ContentProvider` 합니다. `CursorLoader` 쿼리는 `ContentResolver` 백그라운드 스레드 하므로 UI가 차단 되지 않습니다.
   이 클래스를 사용 하 여 Android v4 호환 라이브러리의 이전 버전에서는 액세스할 수 있습니다.


이러한 각 방법에는 동일한 기본 집합이 입력:

-  **Uri** &ndash; 의 정규화 된 이름을 `ContentProvider` 합니다.
-  **프로젝션** &ndash; 커서에 대해 선택 하는 열을 지정 합니다.
-  **선택 영역** &ndash; SQL 비슷하지만 `WHERE` 절.
-  **SelectionArgs** &ndash; 매개 변수는 선택 영역에서 대체 해야 합니다.
-  **SortOrder** &ndash; 열을 기준으로 정렬 합니다.



## <a name="creating-inputs-for-a-query"></a>쿼리에 대 한 입력 만들기

`ContactsProvider` 샘플 코드에서는 Android의 기본 제공 연락처 공급자에 대해 매우 간단한 쿼리를 수행 합니다. 실제 Uri 또는 열 이름-연락처를 쿼리 하는 데 필요한 모든 정보를 알 필요가 없습니다 `ContentProvider` 에 의해 노출 되는 상수로 사용할 수는 `ContactsContract` 클래스입니다.

커서를 검색 하는 방법이 사용에 관계 없이 이러한 동일한 개체를 사용 매개 변수로 표시 된 대로 *ContactsProvider/ContactsAdapter.cs* 파일:

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

이 예는 `selection`, `selectionArgs` 및 `sortOrder` 로 설정 하 여 무시 됩니다 `null`합니다.



## <a name="creating-a-cursor-from-a-content-provider-uri"></a>콘텐츠 공급자 Uri에서 커서를 만드는

매개 변수 개체를 만든 후 다음 세 가지 방법 중 하나에 사용할 수 있습니다.



### <a name="using-a-managed-query"></a>관리 되는 쿼리를 사용 하 여

Android 2.3 (API 수준 10)을 대상으로 응용 프로그램 또는 이전이 방법을 사용 해야 합니다.

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

이 커서 되므로 닫습니다 하지 않고도 Android에서 관리 됩니다.



### <a name="using-contentresolver"></a>ContentResolver를 사용 하 여

에 액세스 `ContentResolver` 직접에 대 한 커서를 가져오려는 `ContentProvider` 수 다음과 같은:

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

이 커서를 관리 하지 않으므로 더 이상 필요할 때 닫혀 있어야 합니다.
확인 코드는 열려 있는 커서를 닫고, 그렇지 않으면 오류가 발생 합니다.

```csharp
cursor.Close();
```

호출할 수 있습니다 `StartManagingCursor()` 및 `StopManagingCursor()` 커서 '관리' 하 합니다. 자동으로 관리 되는 커서 비활성화 하 고 작업 중지 및 다시 시작할 때 다시 쿼리 됩니다.



### <a name="using-cursorloader"></a>CursorLoader를 사용 하 여

Android 3.0 (API 수준 11)에 대 한 응용 프로그램 작성 하거나 최신는이 방법을 사용 해야 합니다.

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

`CursorLoader` 모든 커서 작업이 백그라운드 스레드에서 수행 되 고 지능적으로 다시 사용할 수는 기존 커서 활동 인스턴스 간에 작업 다시 시작 되 면 (예: 구성 변경)으로 인해 대신 다시 데이터를 다시 로드 하는 합니다.

이전 Android 버전도 사용할 수는 `CursorLoader` 클래스를 사용 하 여는 [v4 지원 라이브러리](http://developer.android.com/tools/support-library/index.html)합니다.



## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>사용자 지정 어댑터를 사용 하 여 커서 데이터를 표시합니다.

연락처 이미지를 표시 하려면에서는 사용자 지정 어댑터를 수동으로 해결할 수 있도록는 `PhotoId` 이미지 파일 경로에 대 한 참조입니다.

이 예에서는 사용 데이터는 사용자 지정 어댑터를 표시 하려면는 `CursorLoader` 의 로컬 컬렉션에 있는 모든 연락처 데이터를 검색 하는 **FillContacts** 메서드에서 **ContactsProvider/ContactsAdapter.cs**:

```csharp
void FillContacts ()
{
   var uri = ContactsContract.Contacts.ContentUri;
   string[] projection = {
       ContactsContract.Contacts.InterfaceConsts.Id,
       ContactsContract.Contacts.InterfaceConsts.DisplayName,
       ContactsContract.Contacts.InterfaceConsts.PhotoId
  };
   // CursorLoader introduced in Honeycomb (3.0, API11)
   var loader = new CursorLoader(activity, uri, projection, null, null, null);
   var cursor = (ICursor)loader.LoadInBackground();
   contactList = new List<Contact> ();
   if (cursor.MoveToFirst ()) {
      do {
          contactList.Add (new Contact{
              Id = cursor.GetLong (cursor.GetColumnIndex (projection [0])),
              DisplayName = cursor.GetString (cursor.GetColumnIndex (projection [1])),
              PhotoId = cursor.GetString (cursor.GetColumnIndex (projection [2]))
          });
       } while (cursor.MoveToNext());
   }
}
```

그런 다음 BaseAdapter의 메서드를 사용 하 여 구현 된 `contactList` 컬렉션입니다. 어떤 컬렉션에도 다른 것 처럼는 어댑터 구현 &ndash; 데이터에서 소싱 된 때문에 특별히 처리 하지 않습니다는 `ContentProvider`:

```csharp
Activity activity;
public ContactsAdapter (Activity activity)
{
   this.activity = activity;
   FillContacts ();
}
public override int Count {
   get { return contactList.Count; }
}
public override Java.Lang.Object GetItem (int position)
{
  return null; // could wrap a Contact in a Java.Lang.Object to return it here if needed
}
public override long GetItemId (int position)
{
   return contactList [position].Id;
}
public override View GetView (int position, View convertView, ViewGroup parent)
{
   var view = convertView ?? activity.LayoutInflater.Inflate (Resource.Layout.ContactListItem, parent, false);
   var contactName = view.FindViewById<TextView> (Resource.Id.ContactName);
   var contactImage = view.FindViewById<ImageView> (Resource.Id.ContactImage);
   contactName.Text = contactList [position].DisplayName;
   if (contactList [position].PhotoId == null) {
       contactImage = view.FindViewById<ImageView> (Resource.Id.ContactImage);
       contactImage.SetImageResource (Resource.Drawable.ContactImage);
   } else {
       var contactUri = ContentUris.WithAppendedId (ContactsContract.Contacts.ContentUri, contactList [position].Id);
       var contactPhotoUri = Android.Net.Uri.WithAppendedPath (contactUri, Contacts.Photos.ContentDirectory);
       contactImage.SetImageURI (contactPhotoUri);
   }
   return view;
}
```

(있는 경우) 이미지가 표시 되는 Uri를 사용 하 여 장치에 이미지 파일에 있습니다. 응용 프로그램은 다음과 같습니다.

[![ListView;에 연락처를 표시 하는 응용 프로그램의 스크린 샷 이미지가 한 항목의 왼쪽에 표시 됩니다.](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png#lightbox)

응용 프로그램 유사한 코드 패턴을 사용 하 여 다양 한 사용자의 사진, 비디오 및 음악을 포함 하 여 시스템 데이터를 액세스할 수 있습니다.
일부 데이터 형식은 프로젝트의 요청에 특별 한 권한이 필요 **AndroidManifest.xml**합니다.



## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>SimpleCursorAdapter 사용 하 여 커서 데이터를 표시합니다.

커서와도 표시 될 수 있습니다는 `SimpleCursorAdapter` (이름만 표시 됩니다, 되지만 동적 디스크인 사진 없습니다). 이 코드에서는 사용 하는 방법을 보여 줍니다.는 `ContentProvider` 와 `SimpleCursorAdapter` (이 코드 샘플에는 표시 되지 않습니다).

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName
};
var loader = new CursorLoader (this, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
var fromColumns = new string[] {ContactsContract.Contacts.InterfaceConsts.DisplayName};
var toControlIds = new int[] {Android.Resource.Id.Text1};
adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlsIds);
listView.Adapter = adapter;
```

참조는 [Listview 및 어댑터](~/android/user-interface/layouts/list-view/index.md) 구현에 대 한 자세한 내용은 `SimpleCursorAdapter`합니다.


## <a name="related-links"></a>관련 링크

- [ContactsAdapter 데모 (샘플)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ContactsAdapterDemo/)
