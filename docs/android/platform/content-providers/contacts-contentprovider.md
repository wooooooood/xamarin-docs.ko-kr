---
title: 연락처 ContentProvider 사용
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/22/2018
ms.openlocfilehash: fca57b7af34ae2b28dda9bf20a95183138cbc641
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020540"
---
# <a name="using-the-contacts-contentprovider"></a>연락처 ContentProvider 사용

`ContentProvider`에 의해 노출된 액세스 데이터를 사용하는 코드는 `ContentProvider` 클래스를 참조할 필요가 전혀 없습니다. 대신 `ContentProvider`에 의해 노출된 데이터에 대한 커서를 만들기 위해 URI가 사용됩니다. Android는 이 URI를 사용해서 해당 식별자로 `ContentProvider`를 노출하는 애플리케이션을 시스템에서 검색합니다. URI는 일반적으로 `com.android.contacts/data`와 같은 역방향 DNS 형식의 문자열입니다.

개발자가 이 문자열을 기억하도록 만드는 대신 Android *연락처* 공급자는 해당 메타데이터를 `android.provider.ContactsContract` 클래스로 노출합니다. 이 클래스는 쿼리할 수 있는 테이블 및 열의 이름 뿐만 아니라 `ContentProvider`의 URI를 확인하기 위해 사용됩니다.

일부 데이터 형식에는 특별한 액세스 권한도 필요합니다. 기본 제공되는 연락처 목록에는 **AndroidManifest.xml** 파일의 `android.permission.READ_CONTACTS` 권한이 필요합니다.

URI에서 커서를 만드는 방법은 세 가지입니다.

1. **ManagedQuery()** &ndash; Android 2.3(API 수준 10) 이하에서 선호되는 접근 방식이며, `ManagedQuery`가 커서를 반환하고 데이터 새로 고침 및 커서 닫기도 자동으로 관리합니다. 이 메서드는 Android 3.0(API 수준 11)에서 더 이상 사용되지 않습니다.

1. **ContentResolver.Query()** &ndash; 관리되지 않는 커서를 반환합니다. 즉, 코드에서 명시적으로 새로 고침 및 닫기를 수행해야 합니다.

1. **CursorLoader().LoadInBackground()** &ndash; Android 3.0(API 수준 11)에서 도입되었으며, `CursorLoader`가 이제 `ContentProvider` 사용에 선호되는 방법입니다. `CursorLoader`가 백그라운드 스레드에서 `ContentResolver`를 쿼리하기 때문에 UI가 차단되지 않습니다.
   이 클래스는 이전 버전의 Android에서 v4 호환성 라이브러리를 사용하여 액세스할 수 있습니다.

이러한 각 메서드는 기본 입력 집합이 동일합니다.

- **Uri** &ndash; `ContentProvider`의 정규화된 이름입니다.
- **Projection** &ndash; 커서에 대해 선택할 열을 지정합니다.
- **Selection** &ndash; SQL `WHERE` 절과 비슷합니다.
- **SelectionArgs** &ndash; 선택 항목에서 대체할 매개 변수입니다.
- **SortOrder** &ndash; 정렬할 열입니다.

## <a name="creating-inputs-for-a-query"></a>쿼리 입력 만들기

`ContactsProvider` 샘플 코드는 Android의 기본 제공 연락처 공급자에 대해 매우 간단한 쿼리를 수행합니다. 실제 URI 또는 열 이름을 알 필요가 없습니다. 연락처 `ContentProvider`를 쿼리하는 데 필요한 모든 정보는 `ContactsContract` 클래스로 노출되는 상수로 제공됩니다.

커서 검색에 사용되는 메서드에 관계없이 *ContactsProvider/ContactsAdapter.cs* 파일에 표시된 것처럼 이러한 동일한 개체가 매개변수로 사용됩니다.

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

이 예에서는 `selection`, `selectionArgs` 및 `sortOrder`를 `null`로 설정하여 무시합니다.

## <a name="creating-a-cursor-from-a-content-provider-uri"></a>콘텐츠 공급자 URI에서 커서 만들기

매개 변수 개체는 생성된 후, 다음 세 가지 방법 중 하나로 사용될 수 있습니다.

### <a name="using-a-managed-query"></a>관리되는 쿼리 사용

Android 2.3(API 수준 10) 이하를 대상으로 하는 애플리케이션은 다음 메서드를 사용해야 합니다.

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

이 커서는 Android에서 관리되므로 사용자가 이를 닫을 필요가 없습니다.

### <a name="using-contentresolver"></a>ContentResolver 사용

`ContentProvider`에 대해 커서를 가져오기 위해 `ContentResolver`에 직접 액세스하는 작업은 다음과 같이 수행될 수 있습니다.

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

이 커서는 관리되지 않으므로, 더 이상 필요하지 않으면 닫아야 합니다.
코드가 열려 있는 커서를 닫는지 확인하십시오. 그렇지 않으면 오류가 발생합니다.

```csharp
cursor.Close();
```

또는 커서 '관리'를 위해 `StartManagingCursor()` 및 `StopManagingCursor()`를 호출할 수 있습니다. 관리되는 커서는 작업이 중지되고 다시 시작될 때 자동으로 비활성화되고 다시 쿼리됩니다.

### <a name="using-cursorloader"></a>CursorLoader 사용

Android 3.0(API 수준 11) 이상을 위해 빌드된 애플리케이션은 다음 메서드를 사용해야 합니다.

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

`CursorLoader`는 모든 커서 작업이 백그라운드 스레드에서 실행되도록 보장하며, 데이터를 다시 로드하는 대신 작업이 재시작될 때(예: 구성 변경으로 인해) 작업 인스턴스 간에 기존 커서를 지능적으로 다시 사용합니다.

이전 Android 버전은 또한 [v4 지원 라이브러리](https://developer.android.com/tools/support-library/index.html)를 사용해서 `CursorLoader` 클래스를 사용할 수 있습니다.

## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>커스텀 어댑터로 커서 데이터 표시

연락처 이미지를 표시하기 위해 여기에서는 사용자 지정 어댑터가 사용됩니다. 따라서 이미지 파일 경로에 대한 `PhotoId` 참조를 수동으로 확인할 수 있습니다.

사용자 지정 어댑터로 데이터를 표시하기 위해 예제에서는 `CursorLoader`를 사용하여 모든 연락처 데이터를 **ContactsProvider/ContactsAdapter.cs**에서 **FillContacts** 메서드에 있는 로컬 컬렉션으로 검색합니다.

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

그런 후 `contactList` 컬렉션을 사용하여 BaseAdapter의 메서드를 구현합니다. 이 어댑터는 다른 컬렉션과 동일하게 구현됩니다. 데이터가 `ContentProvider`에서 제공되기 때문에 여기에서는 특별한 처리가 없습니다.

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

이미지는 디바이스의 이미지 파일에 대한 URI를 사용하여 표시됩니다(있는 경우). 애플리케이션은 다음과 같이 표시됩니다.

[![ListView에서 연락처를 표시하는 앱의 스크린샷, 이미지가 한 항목 왼쪽에 표시됨](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png#lightbox)

비슷한 코드 패턴을 사용해서 애플리케이션이 사용자 사진, 동영상 및 음악을 포함한 다양한 시스템 데이터에 액세스할 수 있습니다.
일부 데이터 형식은 프로젝트의 **AndroidManifest.xml**에서 특별한 권한을 요청해야 합니다.

## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>SimpleCursorAdapter로 커서 데이터 표시

`SimpleCursorAdapter`로 커서를 표시할 수도 있습니다(사진이 아닌 이름만 표시됨). 이 코드는 `SimpleCursorAdapter`로 `ContentProvider`를 사용하는 방법을 보여줍니다(이 코드는 샘플에 표시되지 않음).

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

`SimpleCursorAdapter` 구현에 대한 자세한 내용은 [ListViews 및 어댑터](~/android/user-interface/layouts/list-view/index.md)를 참조하세요.

## <a name="related-links"></a>관련 링크

- [ContactsAdapter 데모(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)
