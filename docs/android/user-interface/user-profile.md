---
title: "사용자 프로필"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/21/2017
ms.openlocfilehash: 53ac30abea05095583fcac5ddc315f93ce7024f2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="user-profile"></a>사용자 프로필

Android 사용 하 여 열거 연락처에 지원 되는 `ContactsContract` API 수준 5 이후 공급자입니다. 예를 들어 목록에 연락처가 사용 하 여 단순하게는 `ContactContracts.Contacts` 다음 코드에 나와 있는 것 처럼 클래스:

```csharp
var uri = ContactsContract.Contacts.ContentUri;
           
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.Id,
    ContactsContract.Contacts.InterfaceConsts.DisplayName };
           
var cursor = ManagedQuery (uri, projection, null, null, null);
           
if (cursor.MoveToFirst ()) {
    do {
        Console.WriteLine ("Contact ID: {0}, Contact Name: {1}",
            cursor.GetString (cursor.GetColumnIndex (projection [0])),
            cursor.GetString (cursor.GetColumnIndex (projection [1])));
                   
    } while (cursor.MoveToNext());
}
```

Android 4 (API 수준 14) 새 `ContactsContact.Profile` 클래스는 ContactsContract 공급자를 통해 사용할 수 있습니다. `ContactsContact.Profile` 장치 소유자의 이름 및 전화 번호와 같은 연락처 데이터를 포함 하는 장치의 소유자에 대 한 개인 프로필에 대 한 액세스를 제공 합니다.

<a name="Required_Permissions" />

## <a name="required-permissions"></a>필요한 권한

읽기 및 쓰기 연락처 데이터를 응용 프로그램 요청 해야는 `Read_Contacts` 및 `Write_Contacts` 사용 권한, 각각. 또한 읽고 사용자 프로필을 편집 응용 프로그램 요청 하는 `Read_Profile` 및 `Write_Profile` 사용 권한.

<a name="Updating_Profile_Data" />

## <a name="updating-profile-data"></a>프로필 데이터 업데이트

이러한 사용 권한을 설정한 후 응용 프로그램 정상 Android 기술을 사용해 수 사용자 프로필 데이터와 상호 작용 합니다. 예를 들어, 것 이라고 하는 프로필의 표시 이름을 업데이트 하려면 `ContentResolver.Update` 와 `Uri` 통해 검색는 `ContactsContract.Profile.ContentRawContactsUri` 속성을 아래와 같이:

```csharp
var values = new ContentValues ();
          
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName,
    "John Doe");
           
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri,
    values, null, null);
```

<a name="Reading_Profile_Data" />

## <a name="reading-profile-data"></a>프로필 데이터 읽기

쿼리를 발급 하는 `ContactsContact.Profile.ContentUri` 읽기 프로필 데이터를 다시 합니다. 예를 들어 다음 코드는 사용자 프로필의 표시 이름을 다음과 같습니다.

```csharp
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.DisplayName };
           
var cursor = ManagedQuery (uri, projection, null, null, null);

if (cursor.MoveToFirst ()) {
    Console.WriteLine(
        cursor.GetString (cursor.GetColumnIndex (projection [0])));
}
```

<a name="Navigating_to_the_People_App" />

## <a name="navigating-to-the-people-app"></a>사용자 응용 프로그램으로 이동

마지막으로, Android 4와 함께 제공 되는 새 사용자 앱에서 사용자 프로필을 이동 하려면 단순히 만듭니다와 의도 `ActionView` 동작 및 `ContactsContract.Profile.ContentUri`에 전달 된 `StartActivity` 다음과 같이 메서드:

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);           
StartActivity (intent);
```

위의 코드를 실행할 때 다음 스크린샷에 표시 된 것 처럼 People 앱 사용자 프로필에 로드 됩니다.

[![John Doe 사용자 프로필을 표시 하는 사용자 스크린샷 응용 프로그램](user-profile-images/15-people-app.png)](user-profile-images/15-people-app.png)

사용자 프로필을 가진 작업 이제 Android의 다른 데이터와 상호 작용 유사 하며 장치 개인 설정 된 추가 수준을 제공 합니다.



## <a name="related-links"></a>관련 링크

- [ContactsProviderDemo (샘플)](https://developer.xamarin.com/samples/monodroid/ContactsProviderDemo/)
- [아이스크림 샌드위치 소개](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 플랫폼](http://developer.android.com/sdk/android-4.0.html)
