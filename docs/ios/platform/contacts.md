---
title: 연락처와 ContactsUI
description: 이 문서에서는 새 연락처 및 연락처 UI 프레임 워크 Xamarin.iOS 앱에서 작업에 대해 설명 합니다. 이러한 프레임 워크는 기존 주소록 및 iOS의 이전 버전에서 사용 하는 주소 주소록 UI를 대체 합니다.
ms.prod: xamarin
ms.assetid: 7b6fb66a-5e19-4a5a-9ed2-f6b02af099af
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 4d963bbefce2b4564c3f352be5768df77b45b34d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="contacts-and-contactsui"></a>연락처와 ContactsUI

_이 문서에서는 새 연락처 및 연락처 UI 프레임 워크 Xamarin.iOS 앱에서 작업에 대해 설명 합니다. 이러한 프레임 워크는 기존 주소록 및 iOS의 이전 버전에서 사용 하는 주소 주소록 UI를 대체 합니다._

Apple iOS 9의 도입으로 두 개의 새로운 프레임 워크를 출시 했습니다 `Contacts` 및 `ContactsUI`, 해당 바꾸기 기존 주소 예약 및 iOS 8 및 이전 버전에서 사용 하는 주소 주소록 UI 프레임 워크입니다.

두 개의 새로운 프레임 워크는 다음 기능을 포함 합니다.

- [**연락처** ](#contacts) -사용자 대화 상대 목록 데이터에 대 한 액세스를 제공 합니다.
    대부분의 응용 프로그램에는 읽기 전용 액세스 해야 하기 때문에이 프레임 워크는 스레드로부터 안전 하 고 읽기 전용 액세스를 위해 최적화 되었습니다.

- [**ContactsUI** ](#contactsui) -제공 Xamarin.iOS UI 요소를 표시 하려면 편집을 선택 하 고 iOS 장치에서 연락처를 만들 합니다.

[![](contacts-images/add01.png "IOS 장치에서 연락처 시트 예")](contacts-images/add01.png#lightbox)

> [!IMPORTANT]
> 기존 `AddressBook` 및 `AddressBookUI` 프레임 워크 iOS 8에서 사용할 사전 iOS 9에서에서 사용 되지 않는 및 새로 대체 해야 `Contacts` 및 `ContactsUI` 모든 기존 Xamarin.iOS 앱에 대 한 가능한 한 빨리 프레임 워크입니다. 새 프레임 워크에 대해 새 응용 프로그램을 작성 되어야 합니다.




다음 섹션에서는 이러한 새 프레임 워크 및 Xamarin.iOS 앱에서이 구현 하는 방법을 살펴보겠습니다를 살펴보겠습니다.

<a name="contacts" />

## <a name="the-contacts-framework"></a>연락처 프레임 워크

연락처 프레임 워크 Xamarin.iOS 사용자의 연락처 정보에 대 한 액세스를 제공합니다. 대부분의 응용 프로그램에는 읽기 전용 액세스 해야 하기 때문에이 프레임 워크는 스레드로부터 안전 하 고 읽기 전용 액세스를 위해 최적화 되었습니다.

<a name="Contact_Objects" />

### <a name="contact-objects"></a>연락처 개체

`CNContact` 클래스 이름, 주소 또는 전화 번호와 같은 연락처의 속성에 스레드 안전 하 고 읽기 전용 액세스를 제공 합니다. `CNContact` 와 같은 함수는 `NSDictionary` 포함 속성 (주소 또는 전화 번호) 등의 여러, 읽기 전용 컬렉션:

[![](contacts-images/contactobjects.png "연락처 개체 개요")](contacts-images/contactobjects.png#lightbox)

배열으로 표현 되 있습니다 (예: 전자 메일 주소 또는 전화 번호) 여러 값을 가질 수 있는 모든 속성에 대 한 `NSLabeledValue` 개체입니다. `NSLabeledValue` 레이블 읽기 전용 집합으로 구성 된 스레드 안전 튜플입니다 및 레이블을 사용자 (예를 들어 홈 또는 회사 전자 메일)에 값을 정의 하는 위치 값입니다. 연락처 프레임 워크는 미리 정의 된 레이블의 선택 영역을 제공 (통해는 `CNLabelKey` 및 `CNLabelPhoneNumberKey` 정적 클래스)는 응용 프로그램에서 사용할 수 있거나 요구 사항에 대 한 사용자 지정 레이블이 정의 옵션이 있습니다.

기존 연락처의 값을 조정 합니다 (또는 새로 만들) 하는 모든 Xamarin.iOS 앱을 사용 하 여는 `NSMutableContact` 버전의 클래스 및 해당 하위 클래스 (같은 `CNMutablePostalAddress`).

예를 들어 다음 코드는 새 연락처를 만들 하 고 연락처의 사용자의 컬렉션에 추가 됩니다.

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

IOS 9 장치에서이 코드를 실행 하면 새 연락처를 사용자의 컬렉션에 추가 됩니다. 예를 들어:

[![](contacts-images/add01.png "사용자의 컬렉션에 추가 된 새 연락처")](contacts-images/add01.png#lightbox)

### <a name="contact-formatting-and-localization"></a>연락처 서식 지정 및 지역화

여러 개체를 포함 하는 연락처 프레임 워크 및 메서드 수 있는 서식을 지정 하 고 사용자에 게 표시에 대 한 콘텐츠 필드를 지역화 합니다. 예를 들어 다음 코드는 연락처 이름 및 우편 주소에 대 한 디스플레이 형식을 올바로:

```csharp
Console.WriteLine(CNContactFormatter.GetStringFrom(contact, CNContactFormatterStyle.FullName));
Console.WriteLine(CNPostalAddressFormatter.GetStringFrom(workAddress, CNPostalAddressFormatterStyle.MailingAddress));
```

응용 프로그램의 UI에 표시 될 경우 되는 속성 레이블에 연락처 프레임 워크에도 이러한 문자열 지역화를 위한 메서드가 있습니다. 다시,이 설정은 iOS 장치에서 앱이 실행의 현재 로캘에 기반 합니다. 예를 들어:

```csharp
// Localized properties
Console.WriteLine(CNContact.LocalizeProperty(CNContactOption.Nickname));
Console.WriteLine(CNLabeledValue.LocalizeLabel(CNLabelKey.Home));
```

### <a name="fetching-existing-contacts"></a>기존 연락처를 인출합니다.

인스턴스를 사용 하 여는 `CNContactStore` 클래스를 사용자의 연락처 데이터베이스에서 연락처 정보를 인출할 수 있습니다. `CNContactStore` 모든 인출 또는 연락처 및 그룹의 데이터베이스에서 업데이트를 위한 필요한 메서드를 포함 합니다. 이러한 메서드는 동기적 이므로 백그라운드 스레드에서 UI를 차단 하지 못하도록 하려면에서으로 실행 하는 것이 좋습니다.

조건자를 사용 하 여 (에서 빌드된는 `CNContact` 클래스)을 데이터베이스에서 연락처를 인출할 때 반환 된 결과 필터링 할 수 있습니다. 문자열이 포함 된 연락처에만 가져올 `Appleseed`, 다음 코드를 사용 합니다.

```csharp
// Create predicate to locate requested contact
var predicate = CNContact.GetPredicateForContacts("Appleseed");
```

> [!IMPORTANT]
> 제네릭 및 복합 조건자 연락처 프레임 워크에서 지원 되지 않습니다.

예를 들어, 인출만을 제한 하는 **GivenName** 및 **FamilyName** 다음 코드를 사용 하는 연락처의 속성:

```csharp
// Define fields to be searched
var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName};
```

마지막으로 데이터베이스를 검색 하 고 결과 반환 하려면 다음 코드를 사용 하 여:

```csharp
// Grab matching contacts
var store = new CNContactStore();
NSError error;
var contacts = store.GetUnifiedContacts(predicate, fetchKeys, out error);
```

이 코드에서 만든 샘플 실행 후 실행 된 경우는 **연락처 개체** 위의 섹션에서 방금 만든 "John Appleseed" 연락처가 반환 됩니다.

### <a name="contact-access-privacy"></a>연락처 액세스 개인 정보

최종 사용자가 권한을 부여 하거나 응용 프로그램 별로 그들의 연락처 정보에 대 한 액세스를 거부할 수, 때문에 처음으로 호출 하 여 `CNContactStore`, 앱에 대 한 액세스를 허용 하는 대화 상자가 표시 됩니다.

권한 요청은 한 번만 표시 됩니다, 앱은 후속 및 실행, 처음으로 실행 하거나에 대 한 호출이 `CNContactStore` 당시에는 사용자가 선택한 사용 권한이 사용 됩니다.

연락처의 데이터베이스에 대 한 액세스를 거부 하면 사용자를 적절히 처리 되도록 앱을 디자인 해야 합니다.

#### <a name="fetching-partial-contacts"></a>부분 연락처를 인출합니다.

A _부분 문의_ 은 연락처에 대 한 연락처 저장소에서 인출 된 사용 가능한 속성의 일부입니다. 이 이전에 인출 된 있는 속성에 액세스 하려고 하면 예외가 발생 합니다.

지정 된 연락처 중 하나를 사용 하 여 원하는 속성에 있는지를 쉽게 확인할 수 있습니다는 `IsKeyAvailable` 또는 `AreKeysAvailable` 의 메서드는 `CNContact` 인스턴스. 예를 들어:

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
> `GetUnifiedContact` 및 `GetUnifiedContacts` 의 메서드는 `CNContactStore` 클래스 _만_ 제공 하는 fetch 키에서 요청 된 속성으로 제한 된 일부 연락처를 반환 합니다.

### <a name="unified-contacts"></a>통합 된 연락처

사용자는 다양 한 소스 (예: Facebook 또는 Google 메일 iCloud) 연락처 데이터베이스에서 한 사람에 대 한 연락처 정보에 있을 수 있습니다. IOS 및 OS X 앱에서이 연락처 정보를 자동으로 함께 연결 되며는 단일 사용자에 게 표시 _문의 통합_:

[![](contacts-images/unified01.png "통합 된 연락처 개요")](contacts-images/unified01.png#lightbox)

이 통합 문의 (해야 하는 데 사용할 필요한 경우 연락처를 반드시 다시 반입) 자체 고유 식별자를 지정 하는 링크 연락처 정보의 임시 메모리 보기입니다. 기본적으로 연락처 프레임 워크는 가능한 경우 항상 통합 연락처를 반환 합니다.

### <a name="creating-and-updating-contacts"></a>만들기 및 연락처를 업데이트 합니다.

설명한 것 처럼는 [Contact 개체](#Contact_Objects) 사용 하면 위의 섹션에서는 `CNContactStore` 의 인스턴스는 `CNMutableContact` 만들려는 새 연락처 사용자의 작성 되는 문의 사용 하 여 데이터베이스를 `CNSaveRequest`:

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

A `CNSaveRequest` 한 번에 여러 연락처 및 그룹의 변경 내용을 캐시 하 고 이러한 수정 사항을에 일괄 처리를 사용할 수도 있습니다는 `CNContactStore`합니다.

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

연락처 저장소에 게시 연락처가 수정 될 때마다 한 `CNContactStoreDidChangeNotification` 기본 알림 센터에 있습니다. 연락처 저장소에서 해당 개체를 새로 고치려면 해야 캐시 되지 않은 현재 연락처 표시 하는 경우 (`CNContactStore`).

### <a name="containers-and-groups"></a>컨테이너 및 그룹

사용자의 연락처 또는 로컬 사용자의 장치에서 하나 이상의 서버 계정 (예: Facebook 또는 Google)에서 장치에 동기화 된 연락처 존재할 수 있습니다. 연락처의 각 풀에는 자체 _컨테이너_ 및 지정 된 연락처 하나의 컨테이너에만 존재할 수 있습니다.

[![](contacts-images/containers01.png "컨테이너 및 그룹 개요")](contacts-images/containers01.png#lightbox)

일부 컨테이너를 하나 이상에 정렬 연락처에 대 한 허용 _그룹_ 또는 _하위 그룹_합니다. 이 동작은 특정된 컨테이너에 대 한 백업 저장소에 따라 달라 집니다. 예를 들어 iCloud 하나의 컨테이너 갖지만 많은 그룹 (하지만 하위 그룹이 없는) 있을 수 있습니다. Microsoft Exchange는 반면에 그룹 지원 하지 않지만 (각 Exchange 폴더에 대해 하나씩)는 여러 컨테이너가 있을 수 있습니다.

[![](contacts-images/containers02.png "컨테이너와 그룹 내에서 겹쳐질 수")](contacts-images/containers02.png#lightbox)

<a name="contactsui" />

## <a name="the-contactsui-framework"></a>ContactsUI 프레임 워크

사용자 지정 UI를 표시 하도록 응용 프로그램이 필요 하지 않는 상황에 맞는 표시, 편집, 선택 하 고 Xamarin.iOS 앱에서 연락처를 만들어서 UI 요소를 표시 하는 ContactsUI 프레임 워크를 사용할 수 있습니다.

Apple의 기본 제공 컨트롤을 사용 하 여 뿐만 아니라 Xamarin.iOS 앱에서 연락처를 지원 하기 위해 만들어야 하는 코드의 양을 줄일 수 있지만 응용 프로그램의 사용자에 게 일관 된 인터페이스를 제공 합니다.

### <a name="the-contact-picker-view-controller"></a>연락처 선택 뷰 컨트롤러

연락처 선택 뷰 컨트롤러 (`CNContactPickerViewController`) 사용자의 연락처 데이터베이스의 연락처 연락처 속성 선택할 수 있는 표준 연락처 선택 보기를 관리 합니다. 하나 이상의 연락처 (사용법에 따라)를 선택할 수 있는 하 고 문의 선택기 뷰 컨트롤러 고 선택기를 표시 하기 전에 권한을 묻지 않습니다.

호출 하기 전에 `CNContactPickerViewController` 사용자를 선택 하 고 디스플레이 및 연락처 속성의 선택을 제어 하는 조건자를 정의할 수 있는 속성을 정의 클래스입니다.

상속 되는 클래스의 인스턴스를 사용 하 여 `CNContactPickerDelegate` 고 선택기와 사용자의 상호 작용에 응답할 수 있습니다. 예를 들어:

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

사용자 자신의 데이터베이스에 있는 연락처에서 전자 메일 주소를 선택할 수 있도록 다음 코드를 사용할 수 있습니다.

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

연락처 보기 컨트롤러 (`CNContactViewController`) 클래스는 최종 사용자에 게 표준 연락처 보기를 표시 하는 컨트롤러를 제공 합니다. 연락처 보기 새 신규, 알 수 없음 또는 기존 연락처를 표시할 수 있고 올바른 정적 생성자를 호출 하 여 보기 표시 되기 전에 형식을 지정 해야 합니다 (`FromNewContact`, `FromUnknownContact`, `FromContact`). 예를 들면 다음과 같습니다.

```csharp
// Create a new contact view
var view = CNContactViewController.FromContact(contact);

// Display the view
PresentViewController(view, true, null);
```

## <a name="summary"></a>요약

이 문서에 자세히 살펴보고 Xamarin.iOS 응용 프로그램에서 연락처 및 연락처 UI 프레임 워크와 함께 작업 수행 했습니다. 첫째, 다양 한 유형의 연락처 프레임 워크를 제공 하는 개체와 어떻게를 사용 하거나 새로 만들거나 기존 연락처에 액세스를 다룹니다. 또한 기존 연락처를 선택 하 고 연락처 정보를 표시할 문의 UI 프레임 워크를 검사 합니다.


## <a name="related-links"></a>관련 링크

- [QuickContacts 샘플](https://developer.xamarin.com/samples/monotouch/ios9/QuickContacts/)
- [IOS 9에](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html#//apple_ref/doc/uid/TP40016198-DontLinkElementID_14))
- [연락처 프레임 워크 참조](https://developer.apple.com/library/prerelease/ios/documentation/Contacts/Reference/Contacts_Framework/index.html#//apple_ref/doc/uid/TP40015328)
- [ContactsUI 프레임 워크 참조](https://developer.apple.com/library/prerelease/ios/documentation/ContactsUI/Reference/ContactsUI_Framework/index.html#//apple_ref/doc/uid/TP40016207)
