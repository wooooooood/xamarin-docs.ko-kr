---
title: 연락처 및 ContactsUI Xamarin.iOS에서
description: 이 문서에서는 새 연락처 및 연락처 UI 프레임 워크는 Xamarin.iOS 앱에 작업을 설명 합니다. 이러한 프레임 워크에는 기존 주소록 및 iOS의 이전 버전에서 사용 하는 주소 책 UI를 바꿉니다.
ms.prod: xamarin
ms.assetid: 7b6fb66a-5e19-4a5a-9ed2-f6b02af099af
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: e3f1533605d08df58d8d257714dd8135690c0e5d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61388979"
---
# <a name="contacts-and-contactsui-in-xamarinios"></a>연락처 및 ContactsUI Xamarin.iOS에서

_이 문서에서는 새 연락처 및 연락처 UI 프레임 워크는 Xamarin.iOS 앱에 작업을 설명 합니다. 이러한 프레임 워크에는 기존 주소록 및 iOS의 이전 버전에서 사용 하는 주소 책 UI를 바꿉니다._

Apple iOS 9의 도입으로 두 가지 새 프레임 워크를 출시 했습니다 `Contacts` 고 `ContactsUI`, 해당 replace 기존 주소록 및 iOS 8 및 이전 버전에서 사용 하는 주소 책 UI 프레임 워크입니다.

두 새 프레임 워크는 다음 기능을 포함 합니다.

- [**연락처** ](#contacts) -사용자의 연락처 목록 데이터에 대 한 액세스를 제공 합니다.
    대부분의 앱만 읽기 전용 액세스를 되어야 하므로이 프레임 워크는 스레드 안전 하 고 읽기 전용 액세스에 최적화 되었습니다.

- [**ContactsUI** ](#contactsui) -Xamarin.iOS UI 제공 요소를 표시 하려면 편집 선택 하 고 iOS 장치에서 연락처를 만듭니다.

[![](contacts-images/add01.png "IOS 장치에서 연락처 시트 예제")](contacts-images/add01.png#lightbox)

> [!IMPORTANT]
> 기존 `AddressBook` 하 고 `AddressBookUI` 프레임 워크 (및 이전 버전 iOS 8에서 사용 하 여) iOS 9에서에서 사용 되지 및 새 바꿔야 `Contacts` 및 `ContactsUI` 기존 Xamarin.iOS 앱에 대 한 가능한 한 빨리 프레임 워크입니다. 새 프레임 워크에 대 한 새 앱을 작성 되어야 합니다.




다음 섹션에서는 이러한 새 프레임 워크 및 Xamarin.iOS 앱에 구현 하는 방법을 알아보겠습니다.

<a name="contacts" />

## <a name="the-contacts-framework"></a>연락처 프레임 워크

연락처 프레임 워크는 사용자의 연락처 정보를 Xamarin.iOS 액세스를 제공합니다. 대부분의 앱만 읽기 전용 액세스를 되어야 하므로이 프레임 워크는 스레드 안전 하 고 읽기 전용 액세스에 최적화 되었습니다.

<a name="Contact_Objects" />

### <a name="contact-objects"></a>연락처 개체

`CNContact` 클래스 이름, 주소 또는 전화 번호와 같은 연락처의 속성에 스레드 안전 하 고 읽기 전용 액세스를 제공 합니다. `CNContact` 과 같은 함수를 `NSDictionary` 포함 여러 읽기 전용 컬렉션 속성 (예: 주소 또는 전화 번호)입니다.

[![](contacts-images/contactobjects.png "연락처 개체 개요")](contacts-images/contactobjects.png#lightbox)

여러 값 (예: 전자 메일 주소 또는 전화 번호)을 가질 수 있는 모든 속성에 대 한 표시지 것입니다 배열로 `NSLabeledValue` 개체입니다. `NSLabeledValue` 집합이 읽기 전용으로 레이블 구성 하는 스레드 안전 튜플로 이며 레이블 (예를 들어 홈 또는 회사 전자 메일)를 사용자에 게 값을 정의 하는 위치 값입니다. 연락처 프레임 워크는 다양 한 미리 정의 된 레이블 제공 (통해 합니다 `CNLabelKey` 및 `CNLabelPhoneNumberKey` 정적 클래스)는 앱에서 사용 하거나 요구 사항에 맞게 사용자 지정 레이블을 정의 하는 옵션이 있습니다.

기존 연락처의 값을 조정 합니다 (또는 새로 만들기) 하는 Xamarin.iOS 앱을 사용 합니다 `NSMutableContact` 버전의 클래스 및 해당 하위 클래스 (같은 `CNMutablePostalAddress`).

예를 들어, 다음 코드를 새 연락처를 만들고 연락처의 사용자 컬렉션에 추가 합니다.

```csharp
// Create a new Mutable Contact (read/write)
var contact = new CNMutableContact();

// Set standard properties
contact.GivenName = "John";
contact.FamilyName = "Appleseed";

// Add email addresses
var homeEmail = new CNLabeledValue<NSString>(CNLabelKey.Home, new NSString("john.appleseed@mac.com"));
var workEmail = new CNLabeledValue<NSString>(CNLabelKey.Work, new NSString("john.appleseed@apple.com"));
contact.EmailAddresses = new CNLabeledValue<NSString>[] { homeEmail, workEmail };

// Add phone numbers
var cellPhone = new CNLabeledValue<CNPhoneNumber>(CNLabelPhoneNumberKey.iPhone, new CNPhoneNumber("713-555-1212"));
var workPhone = new CNLabeledValue<CNPhoneNumber>("Work", new CNPhoneNumber("408-555-1212"));
contact.PhoneNumbers = new CNLabeledValue<CNPhoneNumber>[] { cellPhone, workPhone };

// Add work address
var workAddress = new CNMutablePostalAddress()
{
    Street = "1 Infinite Loop",
    City = "Cupertino",
    State = "CA",
    PostalCode = "95014"
};
contact.PostalAddresses = new CNLabeledValue<CNPostalAddress>[] { new CNLabeledValue<CNPostalAddress>(CNLabelKey.Work, workAddress) };

// Add birthday
var birthday = new NSDateComponents()
{
    Day = 1,
    Month = 4,
    Year = 1984
};
contact.Birthday = birthday;

// Save new contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.AddContact(contact, store.DefaultContainerIdentifier);

// Attempt to save changes
NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error))
{
    Console.WriteLine("New contact saved");
}
else
{
    Console.WriteLine("Save error: {0}", error);
}
```

IOS 9 장치에서이 코드가 실행 되는 경우 새 연락처를 사용자의 컬렉션에 추가 됩니다. 예를 들어:

[![](contacts-images/add01.png "사용자의 컬렉션에 추가 하는 새 연락처")](contacts-images/add01.png#lightbox)

### <a name="contact-formatting-and-localization"></a>연락처 서식 지정 및 지역화

여러 개체를 포함 하는 연락처 프레임 워크 및 도움이 되는 메서드 형식 및 사용자에 게 표시에 대 한 콘텐츠를 지역화 합니다. 예를 들어, 다음 코드는 연락처 이름 및 우편 주소 표시에 대 한 형식을 올바로:

```csharp
Console.WriteLine(CNContactFormatter.GetStringFrom(contact, CNContactFormatterStyle.FullName));
Console.WriteLine(CNPostalAddressFormatter.GetStringFrom(workAddress, CNPostalAddressFormatterStyle.MailingAddress));
```

앱의 UI에 표시 됩니다 하는 경우는 레이블의 경우 속성, 연락처 프레임 워크에도 이러한 문자열 지역화를 위한 메서드가 있습니다. 마찬가지로이 앱에서 실행 되는 iOS 장치의 현재 로캘을 기반 됩니다. 예를 들어:

```csharp
// Localized properties
Console.WriteLine(CNContact.LocalizeProperty(CNContactOptions.Nickname));
Console.WriteLine(CNLabeledValue<NSString>.LocalizeLabel(CNLabelKey.Home));
```

### <a name="fetching-existing-contacts"></a>기존 연락처를 가져오는 중

인스턴스를 사용 하 여는 `CNContactStore` 클래스 사용자의 연락처 데이터베이스에서 연락처 정보를 가져올 수 있습니다. `CNContactStore` 모든 인출 또는 연락처 및 그룹에서 데이터베이스를 업데이트 하는 데 필요한 메서드를 포함 합니다. 이러한 메서드는 동기 이기 때문에 UI 차단을 방지 백그라운드 스레드에서 실행 하는 것이 좋습니다.

조건자를 사용 하 여 (에서 작성 된 `CNContact` 클래스)을 연락처 데이터베이스에서 인출할 때 반환 된 결과 필터링 할 수 있습니다. 문자열이 포함 된 연락처만 가져올 `Appleseed`, 다음 코드를 사용 합니다.

```csharp
// Create predicate to locate requested contact
var predicate = CNContact.GetPredicateForContacts("Appleseed");
```

> [!IMPORTANT]
> 제네릭 및 복합 조건자 연락처 프레임 워크에서 지원 되지 않습니다.

예를 들어만 페치를 제한 하는 **GivenName** 및 **FamilyName** 연락처의 속성에 다음 코드를 사용 합니다.

```csharp
// Define fields to be searched
var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName};
```

마지막으로 데이터베이스를 검색 하 고 결과 반환할를 하려면 다음 코드를 사용 합니다.

```csharp
// Grab matching contacts
var store = new CNContactStore();
NSError error;
var contacts = store.GetUnifiedContacts(predicate, fetchKeys, out error);
```

이 코드에서 만든 샘플 실행 후 실행 된 경우는 **연락처 개체** 위의 섹션 우리가 방금 만들었던 "John Appleseed" 연락처를 반환 합니다.

### <a name="contact-access-privacy"></a>개인 정보 보호 연락처 액세스

최종 사용자에 게 권한을 부여 하거나 응용 프로그램 별로 그들의 연락처 정보에 대 한 액세스를 거부 하기, 때문에 처음으로 호출 하 여 `CNContactStore`, 앱에 대 한 액세스를 허용 하 라는 대화 상자가 표시 됩니다.

권한 요청은 한 번만 표시 됩니다, 앱이 실행 되며 이후 처음으로 실행 하거나 호출 된 `CNContactStore` 당시에 사용자가 선택한 사용 권한을 사용 합니다.

해당 연락처 데이터베이스에 대 한 액세스를 거부 하면 정상적으로 처리 되도록 앱을 디자인 해야 합니다.

#### <a name="fetching-partial-contacts"></a>일부 연락처를 가져오는 중

A _부분에 게 문의_ 에 대 한 연락처 저장소에서 인출 된 사용 가능한 속성 중 일부만 게 문의 합니다. 하지 이전에 가져온 속성에 액세스 하려고 하면 예외가 발생 합니다.

지정 된 연락처를 사용 하 여 desired 속성에 있는지를 쉽게 확인할 수 있습니다 합니다 `IsKeyAvailable` 또는 `AreKeysAvailable` 의 메서드는 `CNContact` 인스턴스. 예를 들어:

```csharp
// Does the contact contain the requested key?
if (!contact.IsKeyAvailable(CNContactOption.PostalAddresses)) {
    // No, re-request to pull required info
    var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName, CNContactKey.PostalAddresses};
    var store = new CNContactStore();
    NSError error;
    contact = store.GetUnifiedContact(contact.Identifier, fetchKeys, out error);
}
```

> [!IMPORTANT]
> `GetUnifiedContact` 하 고 `GetUnifiedContacts` 의 메서드는 `CNContactStore` 클래스 _만_ 속성에서 제공 하는 fetch 키 요청 제한 부분 연락처를 반환 합니다.

### <a name="unified-contacts"></a>통합 된 연락처

사용자는 다른 원본 (예: iCloud, Facebook 또는 Google 메일)가 연락처 데이터베이스에서 단일 사용자에 대 한 연락처 정보에 있을 수 있습니다. IOS 및 OS X 앱에서이 연락처 정보를 자동으로 연결 되며 단일 사용자에 게 표시 _에 문의 통합_:

[![](contacts-images/unified01.png "통합 된 연락처 개요")](contacts-images/unified01.png#lightbox)

이 통합에 게 문의 링크 연락처 정보 (사용 해야 하는 필요한 경우 연락처를 반드시 다시 반입 하)는 자체 고유 식별자를 지정 하는 임시 메모리 내 보기입니다. 기본적으로 연락처 프레임 워크는 가능 하면 통합 된 연락처를 반환 합니다.

### <a name="creating-and-updating-contacts"></a>만들기 및 연락처를 업데이트 하는 중

보았듯이 [Contact 개체](#Contact_Objects) 사용 하면 위의 섹션을 `CNContactStore` 의 인스턴스를 `CNMutableContact` 사용자의 작성 되는 새 연락처 만들기를 사용 하 여 데이터베이스에 문의 `CNSaveRequest`:

```csharp
// Create a new Mutable Contact (read/write)
var contact = new CNMutableContact();

// Set standard properties
contact.GivenName = "John";
contact.FamilyName = "Appleseed";

// Save new contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.AddContact(contact, store.DefaultContainerIdentifier);

NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error)) {
    Console.WriteLine("New contact saved");
} else {
    Console.WriteLine("Save error: {0}", error);
}
```

A `CNSaveRequest` 단일 작업으로 여러 연락처 및 그룹의 변경 내용을 캐시에 이러한 수정 사항은 batch를 사용할 수도 있습니다는 `CNContactStore`합니다.

인출 작업에서 가져온 변경할 수 없는 연락처를 업데이트 하려면 먼저 다음 수정 하 고 연락처 저장소에 다시 저장 하는 변경할 수 있는 복사본을 요청 해야 합니다. 예를 들어:

```csharp
// Get mutable copy of contact
var mutable = contact.MutableCopy() as CNMutableContact;
var newEmail = new CNLabeledValue<NSString>(CNLabelKey.Home, new NSString("john.appleseed@xamarin.com"));

// Append new email
var emails = new NSObject[mutable.EmailAddresses.Length+1];
mutable.EmailAddresses.CopyTo(emails,0);
emails[mutable.EmailAddresses.Length+1] = newEmail;
mutable.EmailAddresses = emails;

// Update contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.UpdateContact(mutable);

NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error)) {
    Console.WriteLine("Contact updated.");
} else {
    Console.WriteLine("Update error: {0}", error);
}
```

### <a name="contact-change-notifications"></a>연락처 변경 알림

연락처가 수정 될 때마다 담당자 저장소 게시물을 `CNContactStoreDidChangeNotification` 기본 알림 센터에 있습니다. 연락처 저장소에서 해당 개체를 새로 고치려면 해야 캐시 되지 않은 연락처를 현재 표시 된 경우 (`CNContactStore`).

### <a name="containers-and-groups"></a>컨테이너 및 그룹

사용자의 연락처 또는 로컬 사용자의 장치에서 하나 이상의 서버 계정 (예: Facebook 또는 Google)에서 장치에 동기화 된 연락처 존재할 수 있습니다. 연락처의 각 풀에는 자체 _컨테이너_ 및 지정 된 연락처를 한 컨테이너에만 있을 수 있습니다.

[![](contacts-images/containers01.png "컨테이너 및 그룹 개요")](contacts-images/containers01.png#lightbox)

일부 컨테이너를 하나 이상에 정렬 연락처에 대 한 허용 _그룹_ 하거나 _하위 그룹_합니다. 이 동작은 지정된 된 컨테이너에 대 한 백업 저장소에 따라 달라 집니다. 예를 들어 iCloud에 하나의 컨테이너가 있지만 많은 그룹 (하지만 하위 그룹 없음)을 가질 수 있습니다. Microsoft Exchange는 반면에 그룹을 지원 하지 않지만 (각 Exchange 폴더에 대해 하나) 여러 컨테이너가 있을 수 있습니다.

[![](contacts-images/containers02.png "컨테이너 및 그룹 내에서 겹칠")](contacts-images/containers02.png#lightbox)

<a name="contactsui" />

## <a name="the-contactsui-framework"></a>ContactsUI 프레임 워크

응용 프로그램 사용자 지정 UI를 표시할 필요가 없습니다 경우에 대 한 표시, 편집, 선택 및 Xamarin.iOS 앱에서 연락처를 만드는 UI 요소를 제공 ContactsUI 프레임 워크를 사용할 수 있습니다.

Apple의 기본 제공 컨트롤을 사용 하 여 있습니다 뿐만 아니라 Xamarin.iOS 앱에서 연락처를 지원 하기 위해 만들어야 하는 코드의 양이 줄어들지만 앱의 사용자에 게 일관 된 인터페이스를 제공 합니다.

### <a name="the-contact-picker-view-controller"></a>연락처 선택 뷰 컨트롤러

연락처 선택 뷰 컨트롤러 (`CNContactPickerViewController`) 사용자가 사용자의 연락처 데이터베이스에서 연락처 또는 연락처 속성을 선택할 수 있도록 표준 연락처 선택 뷰를 관리 합니다. 사용자 (사용량에 따라) 하나 이상의 연락처를 선택할 수 및 선택기를 표시 하기 전에 권한을 선택기 문의 뷰 컨트롤러 표시 하지 않습니다.

호출 하기 전에 `CNContactPickerViewController` 사용자를 선택 하 고 표시 및 다양 한 연락처 속성을 제어 하는 조건자를 정의할 수 있는 속성을 정의 클래스입니다.

상속 되는 클래스의 인스턴스를 사용 하 여 `CNContactPickerDelegate` 선택기를 사용 하 여 사용자의 상호 작용에 응답할 수 있습니다. 예를 들어:

```csharp
using System;
using System.Linq;
using UIKit;
using Foundation;
using Contacts;
using ContactsUI;

namespace iOS9Contacts
{
    public class ContactPickerDelegate: CNContactPickerDelegate
    {
        #region Constructors
        public ContactPickerDelegate ()
        {
        }

        public ContactPickerDelegate (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ContactPickerDidCancel (CNContactPickerViewController picker)
        {
            Console.WriteLine ("User canceled picker");

        }

        public override void DidSelectContact (CNContactPickerViewController picker, CNContact contact)
        {
            Console.WriteLine ("Selected: {0}", contact);
        }

        public override void DidSelectContactProperty (CNContactPickerViewController picker, CNContactProperty contactProperty)
        {
            Console.WriteLine ("Selected Property: {0}", contactProperty);
        }
        #endregion
    }
}
```

사용자가 데이터베이스에 있는 연락처에서 전자 메일 주소를 선택할 수 있도록 다음 코드를 사용할 수 있습니다.

```csharp
// Create a new picker
var picker = new CNContactPickerViewController();

// Select property to pick
picker.DisplayedPropertyKeys = new NSString[] {CNContactKey.EmailAddresses};
picker.PredicateForEnablingContact = NSPredicate.FromFormat("emailAddresses.@count > 0");
picker.PredicateForSelectionOfContact = NSPredicate.FromFormat("emailAddresses.@count == 1");

// Respond to selection
picker.Delegate = new ContactPickerDelegate();

// Display picker
PresentViewController(picker,true,null);
```

### <a name="the-contact-view-controller"></a>연락처 뷰 컨트롤러

연락처 보기 컨트롤러 (`CNContactViewController`) 클래스는 최종 사용자에 게 표준 연락처 보기를 제공 하는 컨트롤러를 제공 합니다. 연락처 보기 새 알 수 없는 기존 또는 새 연락처를 표시할 수 있으며 올바른 정적 생성자를 호출 하 여 보기를 표시할 형식을 지정 해야 (`FromNewContact`하십시오 `FromUnknownContact`, `FromContact`). 예를 들면 다음과 같습니다.

```csharp
// Create a new contact view
var view = CNContactViewController.FromContact(contact);

// Display the view
PresentViewController(view, true, null);
```

## <a name="summary"></a>요약

이 문서에서는 Xamarin.iOS 응용 프로그램에서 연락처 및 연락처 UI 프레임 워크를 사용 하 여 작업을 자세히 살펴보려면을 걸렸습니다. 첫째, 다양 한 유형의 연락처 프레임 워크를 제공 하는 개체 및 사용법을 새로 만들거나 기존 연락처에 액세스를 설명 합니다. 또한 기존 연락처를 선택 하 고 연락처 정보를 표시 된 문의 UI 프레임 워크를 검사 합니다.


## <a name="related-links"></a>관련 링크

- [QuickContacts 샘플](https://developer.xamarin.com/samples/monotouch/ios9/QuickContacts/)
- [새로운 iOS 9의 기능](https://developer.apple.com/library/content/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [연결 프레임 워크 참조](https://developer.apple.com/documentation/contacts?language=objc)
- [ContactsUI 프레임 워크 참조](https://developer.apple.com/documentation/contactsui?language=objc)
