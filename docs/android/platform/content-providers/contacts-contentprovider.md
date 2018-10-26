---
title: 연락처 ContentProvider 사용
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/22/2018
ms.openlocfilehash: 95d11ef692ec8b43c128cb55a21d0973151cd24a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120439"
---
# <a name="using-the-contacts-contentprovider"></a>연락처 ContentProvider 사용

사용 하 여 액세스에 의해 노출 되는 데이터는 코드를 `ContentProvider` 에 대 한 참조가 필요 하지 않습니다는 `ContentProvider` 전혀 클래스입니다. 대신, Uri는에서 제공 하는 데이터를 통해 커서를 만드는 데는 `ContentProvider`합니다. Android Uri를 사용 하 여 노출 하는 응용 프로그램에 대 한 시스템을 검색 하는 `ContentProvider` 해당 식별자를 사용 합니다. Uri는 역방향 DNS 형식으로 일반적으로 문자열 같은 `com.android.contacts/data`합니다.

개발자가 Android 해당이 문자열을 기억 하는 것 보다 *연락처* 공급자의 메타 데이터를 노출 합니다 `android.provider.ContactsContract` 클래스입니다. Uri를 확인 하려면이 클래스는 사용 된 `ContentProvider` 테이블 및 쿼리할 수 있는 열 이름을 뿐만 아니라 합니다.

일부 데이터 형식에 액세스 하는 데 특별 한 권한이 필요 합니다. 제공 되는 연락처 목록에 필요 합니다 `android.permission.READ_CONTACTS` 권한 합니다 **AndroidManifest.xml** 파일.

Uri에서 커서를 만들려고 하는 방법은 세 가지가 있습니다.

1. **ManagedQuery()** &ndash; 선호 되는 Android 2.3 (API 수준 10) 및 이전 버전에서는 방법은 `ManagedQuery` 커서를 반환 하 고 자동으로 커서 닫기 및 데이터 새로 고침을 관리 합니다. 이 메서드는 Android 3.0 (API 수준 11)에서 사용 되지 않습니다.

1. **ContentResolver.Query()** &ndash; 새로 고칠 여야 하며 코드에서 명시적으로 닫지는 관리 되지 않는 커서를 반환 합니다.

1. **CursorLoader() 합니다. LoadInBackground()** &ndash; (API 수준 11), Android 3.0에서 도입 되었습니다 `CursorLoader` 사용 하는 기본 방법은 되었습니다는 `ContentProvider` 합니다. `CursorLoader` 쿼리는 `ContentResolver` 백그라운드 스레드 않으므로 UI 차단 되지 않습니다.
   이 클래스는 이전 버전의 v4 호환 라이브러리를 사용 하 여 Android에서 액세스할 수 있습니다.


이러한 메서드의 각 입력의 동일한 기본 집합을 있습니다.

-  **Uri** &ndash; 정규화 된 이름의 `ContentProvider` 합니다.
-  **프로젝션** &ndash; 사양의 커서에 대해 선택할 열입니다.
-  **선택 영역** &ndash; SQL 유사 `WHERE` 절.
-  **SelectionArgs** &ndash; 매개 변수 선택 영역에서 대체 해야 합니다.
-  **SortOrder** &ndash; 열을 기준으로 정렬 합니다.



## <a name="creating-inputs-for-a-query"></a>쿼리에 대 한 입력 만들기

`ContactsProvider` 샘플 코드에서는 Android의 기본 제공 연락처 공급자에 대해 간단한 쿼리를 수행 합니다. 실제 Uri 또는 열 이름을-연락처를 쿼리 하는 데 필요한 모든 정보를 알 필요가 없습니다 `ContentProvider` 에 의해 노출 되는 상수로 사용할 수는 `ContactsContract` 클래스입니다.

커서를 검색에 사용 되는 방법을 관계 없이 이러한 동일한 개체를 사용 매개 변수로 에서처럼 합니다 *ContactsProvider/ContactsAdapter.cs* 파일:

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

예를 들어 합니다 `selection`, `selectionArgs` 하 고 `sortOrder` 로 설정 하 여 무시 됩니다 `null`합니다.



## <a name="creating-a-cursor-from-a-content-provider-uri"></a>콘텐츠 공급자 Uri에서 커서를 만들지

매개 변수 개체를 만든 다음 세 가지 방법 중 하나에서 사용할 수 있습니다.



### <a name="using-a-managed-query"></a>관리 되는 쿼리를 사용 하 여

Android 2.3 (API 수준 10)을 대상으로 하는 응용 프로그램 이전이 메서드를 사용 해야 합니다.

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

이 커서를 닫을 필요가 Android에서 관리 됩니다.



### <a name="using-contentresolver"></a>ContentResolver를 사용 하 여

에 액세스 `ContentResolver` 에 대해 커서를 가져오려는 직접는 `ContentProvider` 수 다음과 같은:

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

이 커서 관리 하지 않으므로 더 이상 필요한 경우 닫혀 있어야 합니다.
코드는 열려 있는 커서를 닫습니다. 그렇지 않으면 오류가 발생 되도록 합니다.

```csharp
cursor.Close();
```

호출할 수 있습니다 `StartManagingCursor()` 고 `StopManagingCursor()` 커서 '관리'로 합니다. 자동으로 관리 되는 커서 비활성화 되어 다시 작업을 중지 했다가 다시 시작 하는 경우를 쿼리 합니다.



### <a name="using-cursorloader"></a>CursorLoader를 사용 하 여

Android 3.0 (API 수준 11) 용으로 빌드된 응용 프로그램 또는 최신이 메서드를 사용 해야 합니다.

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

`CursorLoader` 하면 모든 커서 작업이 백그라운드 스레드에서 완료 되 고 지능적으로 다시 사용할 수는 기존 커서 활동 인스턴스 간에 작업 다시 시작 되 면 (예: 구성 변경)로 인해 대신 다시 데이터를 다시 로드 하는 합니다.

이전 Android 버전을 사용할 수도 있습니다는 `CursorLoader` 를 사용 하 여 클래스를 [v4 지원 라이브러리](http://developer.android.com/tools/support-library/index.html)합니다.



## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>사용자 지정 어댑터를 사용 하 여 커서 데이터를 표시합니다.

연락처 이미지를 표시 하려면에서는 사용자 지정 어댑터를 수동으로 해결할 수 있도록 합니다 `PhotoId` 는 이미지 파일 경로에 대 한 참조입니다.

예제에서는 사용자 지정 어댑터를 사용 하 여 데이터를 표시 하려면를 `CursorLoader` 의 로컬 컬렉션에 모든 연락처 데이터를 검색 하는 **FillContacts** 메서드에서 **ContactsProvider/ContactsAdapter.cs**:

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

그런 다음 사용 하 여 BaseAdapter의 메서드를 구현 합니다 `contactList` 컬렉션입니다. 다른 컬렉션을 사용 하 여는 것 처럼는 어댑터 구현 &ndash; 데이터에서 소싱 된 있으므로 특별히 처리 하지 않습니다는 `ContentProvider`:

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

이미지 (있는 경우)에 표시 되는 Uri를 사용 하 여 장치에서 이미지 파일입니다. 응용 프로그램은 다음과 같습니다.

[![ListView;에 연락처를 표시 하는 앱의 스크린 샷 이미지는 하나의 항목의 왼쪽에 표시 됩니다.](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png#lightbox)

응용 프로그램 코드에서 비슷한 패턴을 사용 하 여, 다양 한 사용자의 사진, 비디오 및 음악을 포함 하 여 시스템 데이터를 액세스할 수 있습니다.
일부 데이터 형식에는 프로젝트의 요청에 특별 한 권한이 있어야 **AndroidManifest.xml**합니다.



## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>SimpleCursorAdapter 사용 하 여 커서 데이터를 표시합니다.

커서를 사용 하 여 표시 될 수도 `SimpleCursorAdapter` (이름만 표시 됩니다 있지만 사진이 없습니다). 이 코드를 사용 하는 방법에 설명 된 `ContentProvider` 사용 하 여 `SimpleCursorAdapter` (이 코드 샘플에는 표시 되지 않습니다).

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

참조 된 [Listview 및 어댑터](~/android/user-interface/layouts/list-view/index.md) 구현에 대 한 자세한 내용은 `SimpleCursorAdapter`합니다.


## <a name="related-links"></a>관련 링크

- [ContactsAdapter 데모 (샘플)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ContactsAdapterDemo/)
