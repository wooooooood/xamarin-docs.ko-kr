---
title: 연락처 ContentProvider 사용
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/22/2018
ms.openlocfilehash: 0784fe5fe42fc82d7067c976bdda6f0436e140b5
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757573"
---
# <a name="using-the-contacts-contentprovider"></a>연락처 ContentProvider 사용

에서 `ContentProvider` 노출 하는 액세스 데이터를 사용 하는 코드에는 `ContentProvider` 클래스에 대 한 참조가 필요 하지 않습니다. 대신 Uri는에서 제공 `ContentProvider`하는 데이터에 대 한 커서를 만드는 데 사용 됩니다. Android는 Uri를 사용 하 여 시스템에서 해당 식별자로를 `ContentProvider` 노출 하는 응용 프로그램을 검색 합니다. Uri는 일반적으로와 `com.android.contacts/data`같은 역방향 DNS 형식의 문자열입니다.

개발자가이 문자열을 기억할 것이 아니라 Android *연락처* 공급자는 `android.provider.ContactsContract` 클래스에서 해당 메타 데이터를 노출 합니다. 이 클래스는의 `ContentProvider` Uri와 쿼리할 수 있는 테이블 및 열의 이름을 결정 하는 데 사용 됩니다.

일부 데이터 형식에는에 대 한 특별 한 액세스 권한도 필요 합니다. 기본 제공 연락처 목록에는 `android.permission.READ_CONTACTS` **androidmanifest .xml** 파일의 권한이 필요 합니다.

Uri에서 커서를 만드는 방법에는 다음 세 가지가 있습니다.

1. **Managedquery ()** Android 2.3 (API 수준 10)에서 선호 하는 접근 방식에서는 `ManagedQuery` 커서를 반환 하 고 데이터 새로 고침 및 커서 닫기를 자동으로 관리 합니다. &ndash; 이 메서드는 Android 3.0 (API 수준 11)에서 더 이상 사용 되지 않습니다.

1. **Contentresolver. 쿼리 ()** &ndash; 관리 되지 않는 커서를 반환 합니다. 즉, 코드에서 명시적으로 새로 고치거 나 닫아야 합니다.

1. **CursorLoader ().**  &ndash; Android 3.0 (API 수준 `CursorLoader` 11)에 도입 된 loadinbackground ()는 이제를 사용 `ContentProvider` 하는 데 선호 되는 방법입니다. `CursorLoader`UI가 `ContentResolver` 차단 되지 않도록 백그라운드 스레드에서을 쿼리 합니다.
   이 클래스는 v4 호환성 라이브러리를 사용 하 여 이전 버전의 Android에서 액세스할 수 있습니다.

이러한 각 메서드에는 동일한 기본 입력 집합이 있습니다.

- **Uri** 의 정규화 된 이름입니다. `ContentProvider` &ndash;
- **프로젝션** &ndash; 커서에 대해 선택할 열 사양입니다.
- **선택** &ndash; SQL`WHERE` 절과 유사 합니다.
- **Selectionargs** &ndash; 선택 영역에서 대체 되는 매개 변수입니다.
- **SortOrder** &ndash; 정렬할 열입니다.

## <a name="creating-inputs-for-a-query"></a>쿼리에 대 한 입력 만들기

샘플 `ContactsProvider` 코드는 Android의 기본 제공 연락처 공급자에 대해 매우 간단한 쿼리를 수행 합니다. 실제 Uri 또는 열 이름을 몰라도 됩니다. 연락처 `ContentProvider` 를 쿼리 하는 데 필요한 모든 정보는 `ContactsContract` 클래스에서 제공 하는 상수로 사용할 수 있습니다.

커서를 검색 하는 데 사용 되는 방법과 관계 없이 이러한 동일한 개체는 다음과 같이 지 수와 같은 개체를 매개 변수로 사용 합니다 *.*

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

이 예 `selection` `sortOrder` 에서는를 로`null`설정 하 여 및를무시합니다.`selectionArgs`

## <a name="creating-a-cursor-from-a-content-provider-uri"></a>콘텐츠 공급자 Uri에서 커서 만들기

매개 변수 개체를 만든 후에는 다음 세 가지 방법 중 하나로 사용할 수 있습니다.

### <a name="using-a-managed-query"></a>관리 되는 쿼리 사용

Android 2.3 (API 수준 10) 또는 이전 버전을 대상으로 하는 응용 프로그램은이 메서드를 사용 해야 합니다.

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

이 커서는 Android에서 관리 되므로 닫을 필요가 없습니다.

### <a name="using-contentresolver"></a>ContentResolver 사용

`ContentResolver` 에`ContentProvider` 대해 커서를 가져오기 위해 직접 액세스 하려면 다음과 같이 수행 하면 됩니다.

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

이 커서는 관리 되지 않으므로 더 이상 필요 하지 않은 경우 닫아야 합니다.
코드가 열려 있는 커서를 닫도록 해야 합니다. 그렇지 않으면 오류가 발생 합니다.

```csharp
cursor.Close();
```

또는를 호출 `StartManagingCursor()` `StopManagingCursor()` 하 여 커서를 ' 관리 ' 할 수도 있습니다. 작업이 중지 되었다가 다시 시작 되 면 관리 되는 커서가 자동으로 비활성화 되 고 다시 쿼리 됩니다.

### <a name="using-cursorloader"></a>CursorLoader 사용

Android 3.0 (API 수준 11) 용으로 빌드된 응용 프로그램은이 메서드를 사용 해야 합니다.

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

는 `CursorLoader` 백그라운드 스레드에서 모든 커서 작업이 수행 되도록 하 고, 데이터를 다시 로드 하는 대신 활동이 다시 시작 될 때 활동 인스턴스 간에 기존 커서를 지능적으로 다시 사용할 수 있도록 합니다.

이전 Android 버전은 `CursorLoader` [v4 지원 라이브러리](https://developer.android.com/tools/support-library/index.html)를 사용 하 여 클래스를 사용할 수도 있습니다.

## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>사용자 지정 어댑터를 사용 하 여 커서 데이터 표시

연락처 이미지를 표시 하려면 이미지 파일 경로에 대 한 `PhotoId` 참조를 수동으로 확인할 수 있도록 사용자 지정 어댑터를 사용 합니다.

사용자 지정 어댑터를 사용 하 여 데이터를 표시 하기 위해 `CursorLoader` 이 예제에서는를 사용 하 여 연락처 데이터를 모든 연락처 데이터를 담당자 **sprovider/동료 sadapter. cs**의 **fillcontacts** 메서드에 있는 로컬 컬렉션으로 검색 합니다.

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

그런 다음 컬렉션을 `contactList` 사용 하 여 baseadapter의 메서드를 구현 합니다. 어댑터는 다른 컬렉션 &ndash; 을 사용 하는 것과 마찬가지로 구현 됩니다 `ContentProvider`. 데이터는에서 원본으로 제공 되기 때문에 특별 한 처리는 없습니다.

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

[![ListView에서 연락처를 표시 하는 앱의 스크린샷 이미지는 한 항목의 왼쪽에 표시 됩니다.](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png#lightbox)

응용 프로그램은 비슷한 코드 패턴을 사용 하 여 사용자의 사진, 비디오 및 음악을 비롯 한 다양 한 시스템 데이터에 액세스할 수 있습니다.
일부 데이터 형식에는 프로젝트의 **Androidmanifest**에서 특정 권한을 요청 해야 합니다.

## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>SimpleCursorAdapter를 사용 하 여 커서 데이터 표시

커서는와 함께 `SimpleCursorAdapter` 표시 될 수도 있습니다 (사진만 아닌 이름만 표시 됨). 이 코드에서는와 함께 `ContentProvider` `SimpleCursorAdapter` 를 사용 하는 방법을 보여 줍니다 (이 코드는 샘플에 표시 되지 않음).

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

구현`SimpleCursorAdapter`에 대 한 자세한 내용은 [listview 및 어댑터](~/android/user-interface/layouts/list-view/index.md) 를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [연락처 Sadapter 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)
