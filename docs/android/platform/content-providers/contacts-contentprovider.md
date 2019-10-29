---
title: 연락처 ContentProvider 사용
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/22/2018
ms.openlocfilehash: fca57b7af34ae2b28dda9bf20a95183138cbc641
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020540"
---
# <a name="using-the-contacts-contentprovider"></a>연락처 ContentProvider 사용

`ContentProvider`에서 노출 하는 액세스 데이터를 사용 하는 코드에는 `ContentProvider` 클래스에 대 한 참조가 필요 하지 않습니다. 대신 Uri는 `ContentProvider`에서 제공 하는 데이터에 대 한 커서를 만드는 데 사용 됩니다. Android는 Uri를 사용 하 여 해당 식별자와 `ContentProvider`를 노출 하는 응용 프로그램에 대 한 시스템을 검색 합니다. Uri는 일반적으로 `com.android.contacts/data`와 같은 역방향 DNS 형식의 문자열입니다.

개발자가이 문자열을 기억할 것이 아니라 Android *연락처* 공급자는 `android.provider.ContactsContract` 클래스에서 해당 메타 데이터를 노출 합니다. 이 클래스는 쿼리할 수 있는 테이블 및 열의 이름 뿐만 아니라 `ContentProvider`의 Uri를 확인 하는 데 사용 됩니다.

일부 데이터 형식에는에 대 한 특별 한 액세스 권한도 필요 합니다. 기본 제공 연락처 목록에는 **Androidmanifest .xml** 파일의 `android.permission.READ_CONTACTS` 권한이 필요 합니다.

Uri에서 커서를 만드는 방법에는 다음 세 가지가 있습니다.

1. **Managedquery ()** &ndash; Android 2.3 (API 수준 10) 및 이전 버전에서 `ManagedQuery` 커서를 반환 하 고 데이터 새로 고침 및 커서 닫기를 자동으로 관리 합니다. 이 메서드는 Android 3.0 (API 수준 11)에서 더 이상 사용 되지 않습니다.

1. **Contentresolver. Query ()** &ndash;는 관리 되지 않는 커서를 반환 합니다. 즉, 코드에서 명시적으로 새로 고치거 나 닫아야 합니다.

1. **CursorLoader (). LoadInBackground ()** &ndash; Android 3.0 (API 수준 11)에 도입 된 `CursorLoader` 이제 `ContentProvider`를 사용 하는 데 선호 되는 방법입니다. `CursorLoader`는 UI가 차단 되지 않도록 백그라운드 스레드에서 `ContentResolver`를 쿼리 합니다.
   이 클래스는 v4 호환성 라이브러리를 사용 하 여 이전 버전의 Android에서 액세스할 수 있습니다.

이러한 각 메서드에는 동일한 기본 입력 집합이 있습니다.

- **Uri** &ndash; `ContentProvider`의 정규화 된 이름입니다.
- 커서에 대해 선택할 열의 **프로젝션** &ndash; 사양입니다.
- **선택** &ndash; SQL `WHERE` 절과 유사 합니다.
- **Selectionargs** &ndash; 매개 변수를 선택 영역에서 대체 합니다.
- 정렬할 정렬 **기준 &ndash; 열** 입니다.

## <a name="creating-inputs-for-a-query"></a>쿼리에 대 한 입력 만들기

`ContactsProvider` 샘플 코드는 Android의 기본 제공 연락처 공급자에 대해 매우 간단한 쿼리를 수행 합니다. 실제 Uri 또는 열 이름을 몰라도 됩니다. 연락처 `ContentProvider` 쿼리 하는 데 필요한 모든 정보는 `ContactsContract` 클래스에서 제공 하는 상수로 사용할 수 있습니다.

커서를 검색 하는 데 사용 되는 방법과 관계 없이 이러한 동일한 개체는 다음과 같이 지 수와 같은 개체를 매개 변수로 사용 합니다 *.*

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

이 예에서는 `selection``selectionArgs` 및 `sortOrder`를 `null`로 설정 하 여 무시 합니다.

## <a name="creating-a-cursor-from-a-content-provider-uri"></a>콘텐츠 공급자 Uri에서 커서 만들기

매개 변수 개체를 만든 후에는 다음 세 가지 방법 중 하나로 사용할 수 있습니다.

### <a name="using-a-managed-query"></a>관리 되는 쿼리 사용

Android 2.3 (API 수준 10) 또는 이전 버전을 대상으로 하는 응용 프로그램은이 메서드를 사용 해야 합니다.

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

이 커서는 Android에서 관리 되므로 닫을 필요가 없습니다.

### <a name="using-contentresolver"></a>ContentResolver 사용

`ContentProvider`에 대해 커서를 가져오기 위해 `ContentResolver`에 직접 액세스 하는 것은 다음과 같이 수행할 수 있습니다.

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

이 커서는 관리 되지 않으므로 더 이상 필요 하지 않은 경우 닫아야 합니다.
코드가 열려 있는 커서를 닫도록 해야 합니다. 그렇지 않으면 오류가 발생 합니다.

```csharp
cursor.Close();
```

또는 `StartManagingCursor()`를 호출 하 고 커서를 ' 관리 ' 하도록 `StopManagingCursor()` 수 있습니다. 작업이 중지 되었다가 다시 시작 되 면 관리 되는 커서가 자동으로 비활성화 되 고 다시 쿼리 됩니다.

### <a name="using-cursorloader"></a>CursorLoader 사용

Android 3.0 (API 수준 11) 용으로 빌드된 응용 프로그램은이 메서드를 사용 해야 합니다.

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

`CursorLoader`는 모든 커서 작업이 백그라운드 스레드에서 수행 되도록 하 고, 작업을 다시 시작할 때 (예: 구성 변경으로 인해) 작업 인스턴스 간에 기존 커서를 지능적으로 다시 사용할 수 있도록 합니다.

이전 Android 버전은 [v4 지원 라이브러리](https://developer.android.com/tools/support-library/index.html)를 사용 하 여 `CursorLoader` 클래스를 사용할 수도 있습니다.

## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>사용자 지정 어댑터를 사용 하 여 커서 데이터 표시

연락처 이미지를 표시 하려면 이미지 파일 경로에 대 한 `PhotoId` 참조를 수동으로 확인할 수 있도록 사용자 지정 어댑터를 사용 합니다.

사용자 지정 어댑터를 사용 하 여 데이터를 표시 하기 위해이 예제에서는 `CursorLoader`를 사용 하 여 모든 연락처 데이터 **를 동료** **sprovider/동료 sadapter.**

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

그런 다음 `contactList` 컬렉션을 사용 하 여 BaseAdapter의 메서드를 구현 합니다. 어댑터는 다른 모든 컬렉션과 마찬가지로 구현 되며, 데이터는 `ContentProvider`원본 이기 때문에 특별 한 처리는 없습니다 &ndash;.

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

장치의 이미지 파일에 대 한 Uri를 사용 하 여 이미지가 표시 됩니다 (있는 경우). 응용 프로그램은 다음과 같습니다.

[ListView에서 연락처를 표시 하는 앱의![스크린샷 이미지는 한 항목의 왼쪽에 표시 됩니다.](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png#lightbox)

응용 프로그램은 비슷한 코드 패턴을 사용 하 여 사용자의 사진, 비디오 및 음악을 비롯 한 다양 한 시스템 데이터에 액세스할 수 있습니다.
일부 데이터 형식에는 프로젝트의 **Androidmanifest**에서 특정 권한을 요청 해야 합니다.

## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>SimpleCursorAdapter를 사용 하 여 커서 데이터 표시

커서는 `SimpleCursorAdapter` 표시 될 수도 있습니다 (사진만 아닌 이름만 표시 됨). 이 코드는 `SimpleCursorAdapter`에서 `ContentProvider`를 사용 하는 방법을 보여 줍니다 (이 코드는 샘플에 표시 되지 않음).

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

`SimpleCursorAdapter`구현에 대 한 자세한 내용은 [listview 및 어댑터](~/android/user-interface/layouts/list-view/index.md) 를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [연락처 Sadapter 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)
