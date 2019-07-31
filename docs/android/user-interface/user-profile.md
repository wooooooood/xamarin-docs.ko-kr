---
title: 사용자 프로필
ms.prod: xamarin
ms.assetid: 6BB01F75-5E98-49A1-BBA0-C2680905C59D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/22/2018
ms.openlocfilehash: ef8f6aeb3c6577825210c0bce594ea0eb4af4e2f
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648256"
---
# <a name="user-profile"></a>사용자 프로필

Android는 API 수준 5 이후 동료 [S계약](xref:Android.Provider.ContactsContract) 공급자와 연락처를 열거 하는 것을 지원 합니다. 예를 들어 연락처를 나열 하는 것은 다음 코드 예제와 같이 [연락처](xref:Android.Provider.ContactsContract.Contacts) 클래스를 사용 하는 것 처럼 간단 합니다.

```csharp
// Get the URI for the user's contacts:
var uri = ContactsContract.Contacts.ContentUri;

// Setup the "projection" (columns we want) for only the ID and display name:
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.Id, 
    ContactsContract.Contacts.InterfaceConsts.DisplayName };

// Use a CursorLoader to retrieve the user's contacts data:
CursorLoader loader = new CursorLoader(this, uri, projection, null, null, null);
ICursor cursor = (ICursor)loader.LoadInBackground();

// Print the contact data to the console if reading back succeeds:
if (cursor != null)
{
    if (cursor.MoveToFirst())
    {
        do
        {
            Console.WriteLine("Contact ID: {0}, Contact Name: {1}",
                               cursor.GetString(cursor.GetColumnIndex(projection[0])),
                               cursor.GetString(cursor.GetColumnIndex(projection[1])));
        } while (cursor.MoveToNext());
    }
}
```

Android 4 (API 수준 14)부터 [연락처](xref:Android.Provider.ContactsContract.Profile) 클래스는 `ContactsContract` 공급자를 통해 사용할 수 있습니다. 는 `ContactsContact.Profile` 장치 소유자의 개인 프로필에 대 한 액세스를 제공 합니다. 여기에는 장치 소유자의 이름 및 전화 번호와 같은 연락처 데이터가 포함 됩니다.

## <a name="required-permissions"></a>필요한 권한

연락처 데이터를 읽고 쓰려면 응용 프로그램은 각각 `READ_CONTACTS` 및 `WRITE_CONTACTS` 권한을 요청 해야 합니다.
또한 사용자 프로필을 읽고 편집 하려면 응용 프로그램에서 `READ_PROFILE` 및 `WRITE_PROFILE` 권한을 요청 해야 합니다.

## <a name="updating-profile-data"></a>프로필 데이터 업데이트

이러한 권한이 설정 되 면 응용 프로그램은 일반 Android 기술을 사용 하 여 사용자 프로필의 데이터와 상호 작용할 수 있습니다. 예를 들어 프로필의 표시 이름을 업데이트 하려면 아래와 같이 [ContentRawContactsUri](xref:Android.Provider.ContactsContract.Profile.ContentRawContactsUri) 속성을 통해 `Uri` 검색 된로 [contentresolver. update를 호출 합니다.](xref:Android.Content.ContentResolver.Update*)

```csharp
var values = new ContentValues ();
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName, "John Doe");

// Update the user profile with the name "John Doe":
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri, values, null, null);
```

## <a name="reading-profile-data"></a>프로필 데이터 읽기

연락처에 대 한 쿼리를 실행 하면 프로필 데이터를 [다시 읽습니다.](xref:Android.Provider.ContactsContract.Profile.ContentUri) 예를 들어, 다음 코드는 사용자 프로필의 표시 이름을 읽습니다.

```csharp
// Read the profile
var uri = ContactsContract.Profile.ContentUri;

// Setup the "projection" (column we want) for only the display name:
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.DisplayName };

// Use a CursorLoader to retrieve the data:
CursorLoader loader = new CursorLoader(this, uri, projection, null, null, null);
ICursor cursor = (ICursor)loader.LoadInBackground();
if (cursor != null)
{
    if (cursor.MoveToFirst ())
    {
        Console.WriteLine(cursor.GetString (cursor.GetColumnIndex (projection [0])));
    }
}
```

## <a name="navigating-to-the-user-profile"></a>사용자 프로필로 이동

마지막으로, 사용자 프로필로 이동 하려면 `ActionView` 작업 `ContactsContract.Profile.ContentUri` 을 사용 하 여 의도를 만든 `StartActivity` 후 다음과 같이 메서드로 전달 합니다.

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);
StartActivity (intent);
```

위의 코드를 실행 하는 경우 사용자 프로필은 다음 스크린샷에 표시 된 대로 표시 됩니다.

[![John Doe 사용자 프로필을 표시 하는 프로필의 스크린샷](user-profile-images/01-profile-screen-sml.png)](user-profile-images/01-profile-screen.png#lightbox)

사용자 프로필 작업은 Android에서 다른 데이터와 상호 작용 하는 것과 유사 하며 추가 수준의 장치 개인 설정을 제공 합니다.

## <a name="related-links"></a>관련 링크

- [연락처 Sproviderdemo (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/contactsproviderdemo)
- [아이스크림 및 사우스 샌드위치](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 플랫폼](https://developer.android.com/sdk/android-4.0.html)
