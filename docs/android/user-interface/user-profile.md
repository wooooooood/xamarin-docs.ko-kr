---
title: 사용자 프로필
ms.prod: xamarin
ms.assetid: 6BB01F75-5E98-49A1-BBA0-C2680905C59D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/22/2018
ms.openlocfilehash: 1eaae86ab9eacf007eca792d96e889db6f367922
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="user-profile"></a>사용자 프로필

Android 사용 하 여 열거 연락처에 지원 되는 [ContactsContract](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract/) API 수준 5 이후 공급자입니다. 예를 들어 연락처 나열 작업은을 사용 하 여는 [ContactContracts.Contacts](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract+Contacts/) 다음 코드 예제와 같이 클래스:

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

Android 4 (API 수준 14)로 시작는 [ContactsContact.Profile](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract+Profile/) 클래스를 통해 사용할 수는 `ContactsContract` 공급자입니다. `ContactsContact.Profile` 장치 소유자의 이름 및 전화 번호와 같은 연락처 데이터를 포함 하는 장치의 소유자에 대 한 개인 프로필에 대 한 액세스를 제공 합니다.


## <a name="required-permissions"></a>필요한 권한

읽기 및 쓰기 연락처 데이터를 응용 프로그램 요청 해야는 `READ_CONTACTS` 및 `WRITE_CONTACTS` 사용 권한, 각각.
또한 읽고 사용자 프로필을 편집 응용 프로그램 요청 하는 `READ_PROFILE` 및 `WRITE_PROFILE` 사용 권한.


## <a name="updating-profile-data"></a>프로필 데이터 업데이트

이러한 사용 권한을 설정한 후 응용 프로그램 정상 Android 기술을 사용해 수 사용자 프로필 데이터와 상호 작용 합니다. 예를 들어 프로필의 표시 이름을 업데이트 하려면 호출 [ContentResolver.Update](https://developer.xamarin.com/api/member/Android.Content.ContentResolver.Update) 와 `Uri` 통해 검색는 [ContactsContract.Profile.ContentRawContactsUri](https://developer.xamarin.com/api/property/Android.Provider.ContactsContract+Profile.ContentRawContactsUri/) 속성을 표시 된 것 처럼 아래:

```csharp
var values = new ContentValues ();
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName, "John Doe");

// Update the user profile with the name "John Doe":
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri, values, null, null);
```

## <a name="reading-profile-data"></a>프로필 데이터 읽기

쿼리를 발급 하는 [ContactsContact.Profile.ContentUri](https://developer.xamarin.com/api/property/Android.Provider.ContactsContract+Profile.ContentUri/) 읽기 프로필 데이터를 다시 합니다. 예를 들어 다음 코드는 사용자 프로필의 표시 이름을 다음과 같습니다.

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

## <a name="navigating-to-the-user-profile"></a>사용자 프로필 탐색

마지막으로, 사용자 프로필을 이동 하려면와 의도 만듭니다는 `ActionView` 동작 및 `ContactsContract.Profile.ContentUri` 다음에 전달 된 `StartActivity` 다음과 같이 메서드:

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);           
StartActivity (intent);
```

위의 코드를 실행할 때 다음 스크린샷에 표시 된 것 처럼 사용자 프로필이 표시 됩니다.

[![John Doe 사용자 프로필을 표시 하는 프로필의 스크린 샷](user-profile-images/01-profile-screen-sml.png)](user-profile-images/01-profile-screen.png#lightbox)

사용자 프로필을 가진 작업은 Android의 다른 데이터와 상호 작용와 장치 개인 설정의 추가 수준을 제공 합니다.



## <a name="related-links"></a>관련 링크

- [ContactsProviderDemo (샘플)](https://developer.xamarin.com/samples/monodroid/ContactsProviderDemo/)
- [아이스크림 샌드위치 소개](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 플랫폼](http://developer.android.com/sdk/android-4.0.html)
