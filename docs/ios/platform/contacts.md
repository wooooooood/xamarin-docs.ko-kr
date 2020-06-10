---
title: Xamarin.ios의 연락처 및 연락처
description: 이 문서에서는 Xamarin.ios 앱에서 새 연락처 및 연락처 UI 프레임 워크를 사용 하는 방법을 설명 합니다. 이러한 프레임 워크는 이전 버전의 iOS에서 사용 되는 기존 주소록 및 주소록 UI를 대체 합니다.
ms.prod: xamarin
ms.assetid: 7b6fb66a-5e19-4a5a-9ed2-f6b02af099af
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: 918030120e6b7d0e22abdf5ea3e57f3849b86616
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84572574"
---
# <a name="contacts-and-contactsui-in-xamarinios"></a>Xamarin.ios의 연락처 및 연락처

_이 문서에서는 Xamarin.ios 앱에서 새 연락처 및 연락처 UI 프레임 워크를 사용 하는 방법을 설명 합니다. 이러한 프레임 워크는 이전 버전의 iOS에서 사용 되는 기존 주소록 및 주소록 UI를 대체 합니다._

IOS 9가 도입 되면서 Apple은 `Contacts` `ContactsUI` ios 8 및 이전 버전에서 사용 하는 기존 주소록 및 주소록 UI 프레임 워크를 대체 하는 두 가지 새로운 프레임 워크 및를 출시 했습니다.

새로운 두 프레임 워크에는 다음과 같은 기능이 포함 되어 있습니다.

- [**연락처**](#contacts) -사용자의 연락처 목록 데이터에 대 한 액세스를 제공 합니다.
  대부분의 앱은 읽기 전용 액세스만 필요 하므로이 프레임 워크는 스레드로부터 안전한 읽기 전용 액세스를 위해 최적화 되었습니다.

- [**연락처-ios**](#contactsui) 장치에서 연락처를 표시, 편집, 선택 및 만들 수 있는 xamarin.ios UI 요소를 제공 합니다.

[![](contacts-images/add01.png "An example Contact Sheet on an iOS device")](contacts-images/add01.png#lightbox)

> [!IMPORTANT]
> `AddressBook` `AddressBookUI` Ios 8 (및 이전 버전)에서 사용 하는 기존 및 프레임 워크는 ios 9에서 더 이상 사용 되지 않으며, `Contacts` `ContactsUI` 기존 xamarin.ios 앱의 경우 가능한 한 빨리 새 및 프레임 워크로 바꾸어야 합니다. 새 프레임 워크에 대해 새 앱을 작성 해야 합니다.

다음 섹션에서는 이러한 새 프레임 워크와 Xamarin.ios 앱에서 이러한 프레임 워크를 구현 하는 방법을 살펴보겠습니다.

<a name="contacts"></a>

## <a name="the-contacts-framework"></a>연락처 프레임 워크

연락처 프레임 워크는 사용자의 연락처 정보에 대 한 Xamarin.ios 액세스를 제공 합니다. 대부분의 앱은 읽기 전용 액세스만 필요 하므로이 프레임 워크는 스레드로부터 안전한 읽기 전용 액세스를 위해 최적화 되었습니다.

<a name="Contact_Objects"></a>

### <a name="contact-objects"></a>연락처 개체

`CNContact`클래스는 이름, 주소 또는 전화 번호와 같은 연락처의 속성에 대 한 읽기 전용 액세스를 스레드로부터 안전 하 게 제공 합니다. `CNContact`및와 같은 함수는 `NSDictionary` 여러 개의 읽기 전용 속성 컬렉션 (예: 주소 또는 전화 번호)을 포함 합니다.

[![](contacts-images/contactobjects.png "Contact Object overview")](contacts-images/contactobjects.png#lightbox)

전자 메일 주소 또는 전화 번호와 같이 여러 값을 가질 수 있는 속성의 경우에는 개체 배열로 표시 됩니다 `NSLabeledValue` . `NSLabeledValue`레이블이 사용자에 게 값을 정의 하는 읽기 전용 레이블 및 값 (예: Home 또는 Work email)으로 구성 된 스레드로부터 안전한 튜플입니다. 연락처 프레임 워크는 `CNLabelKey` 응용 프로그램에서 사용할 수 있는 미리 정의 된 레이블 (및 정적 클래스를 통해)을 선택 `CNLabelPhoneNumberKey` 하거나 사용자 요구에 맞게 사용자 지정 레이블을 정의할 수 있습니다.

기존 연락처의 값을 조정 해야 하는 Xamarin.ios 앱 (또는 새로 만들기)의 경우 `NSMutableContact` 클래스 및 해당 하위 클래스의 버전 (예:)을 사용 `CNMutablePostalAddress` 합니다.

예를 들어 다음 코드는 새 연락처를 만들어 사용자의 연락처 컬렉션에 추가 합니다.

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

이 코드가 iOS 9 장치에서 실행 되는 경우 새 연락처가 사용자의 컬렉션에 추가 됩니다. 예를 들면 다음과 같습니다.

[![](contacts-images/add01.png "A new contact added to the user's collection")](contacts-images/add01.png#lightbox)

### <a name="contact-formatting-and-localization"></a>연락처 서식 지정 및 지역화

연락처 프레임 워크에는 사용자에 게 표시할 콘텐츠를 서식 지정 하 고 지역화 하는 데 도움이 되는 여러 개체와 메서드가 포함 되어 있습니다. 예를 들어 다음 코드는 표시를 위해 연락처 이름과 우편 주소를 올바르게 지정 합니다.

```csharp
Console.WriteLine(CNContactFormatter.GetStringFrom(contact, CNContactFormatterStyle.FullName));
Console.WriteLine(CNPostalAddressFormatter.GetStringFrom(workAddress, CNPostalAddressFormatterStyle.MailingAddress));
```

응용 프로그램의 UI에 표시 되는 속성 레이블의 경우 Contact framework에도 이러한 문자열을 지역화 하는 메서드가 있습니다. 이는 앱이 실행 되는 iOS 장치의 현재 로캘을 기반으로 합니다. 예를 들면 다음과 같습니다.

```csharp
// Localized properties
Console.WriteLine(CNContact.LocalizeProperty(CNContactOptions.Nickname));
Console.WriteLine(CNLabeledValue<NSString>.LocalizeLabel(CNLabelKey.Home));
```

### <a name="fetching-existing-contacts"></a>기존 연락처를 페치하는 중

클래스의 인스턴스를 사용 하 여 `CNContactStore` 사용자의 연락처 데이터베이스에서 연락처 정보를 가져올 수 있습니다. 에는 `CNContactStore` 데이터베이스에서 연락처와 그룹을 페치 하거나 업데이트 하는 데 필요한 모든 메서드가 포함 되어 있습니다. 이러한 메서드는 동기적 이므로 UI를 차단 하지 않도록 백그라운드 스레드에서 실행 하는 것이 좋습니다.

클래스에서 빌드된 조건자를 사용 하 여 `CNContact` 데이터베이스에서 연락처를 가져올 때 반환 되는 결과를 필터링 할 수 있습니다. 문자열을 포함 하는 연락처만 페치 하려면 `Appleseed` 다음 코드를 사용 합니다.

```csharp
// Create predicate to locate requested contact
var predicate = CNContact.GetPredicateForContacts("Appleseed");
```

> [!IMPORTANT]
> Contact 프레임 워크에서는 제네릭 및 복합 조건자가 지원 되지 않습니다.

예를 들어, **GivenName** 및 **FamilyName** 속성 으로만 fetch를 제한 하려면 다음 코드를 사용 합니다.

```csharp
// Define fields to be searched
var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName};
```

마지막으로 데이터베이스를 검색 하 고 결과를 반환 하려면 다음 코드를 사용 합니다.

```csharp
// Grab matching contacts
var store = new CNContactStore();
NSError error;
var contacts = store.GetUnifiedContacts(predicate, fetchKeys, out error);
```

위의 contact **개체** 섹션에서 만든 샘플 다음에이 코드를 실행 한 경우 방금 만든 "John 사과 eseed" 연락처를 반환 합니다.

### <a name="contact-access-privacy"></a>액세스 개인 정보

최종 사용자는 응용 프로그램 별로 연락처 정보에 대 한 액세스 권한을 부여 하거나 거부할 수 있으므로를 처음으로 호출 하면 `CNContactStore` 앱에 대 한 액세스를 허용 하도록 요청 하는 대화 상자가 표시 됩니다.

권한 요청은 앱을 처음 실행할 때 한 번만 표시 되 고 후속 실행 또는에 대 한 호출은 `CNContactStore` 사용자가 해당 시간에 선택한 사용 권한을 사용 하 게 됩니다.

사용자의 연락처 데이터베이스 액세스를 거부 하는 사용자를 정상적으로 처리 하도록 앱을 디자인 해야 합니다.

#### <a name="fetching-partial-contacts"></a>부분 연락처를 페치하는 중

일부 _연락처_ 는의 연락처 저장소에서 사용 가능한 속성 중 일부만 인출 된 연락처입니다. 이전에 인출 되지 않은 속성에 액세스 하려고 하면 예외가 발생 합니다.

인스턴스의 또는 메서드 중 하나를 사용 하 여 지정 된 연락처에 desired 속성이 있는지 쉽게 확인할 수 있습니다 `IsKeyAvailable` `AreKeysAvailable` `CNContact` . 예를 들면 다음과 같습니다.

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
> `GetUnifiedContact` `GetUnifiedContacts` 클래스의 및 메서드는 `CNContactStore` 제공 된 fetch 키에서 요청 된 속성으로 제한 된 부분 _연결만_ 반환 합니다.

### <a name="unified-contacts"></a>통합 연락처

사용자에 게 연락처 데이터베이스의 한 사용자에 대 한 연락처 정보 소스가 있을 수 있습니다 (예: iCloud, Facebook, Google 메일). IOS 및 OS X 앱에서이 연락처 정보는 자동으로 함께 연결 되 고 사용자에 게 단일 _통합 연락처로_표시 됩니다.

[![](contacts-images/unified01.png "Unified Contacts overview")](contacts-images/unified01.png#lightbox)

이 통합 된 연락처는 고유한 식별자 (필요한 경우 연락처를 다시 페치 하는 데 사용 해야 함)가 제공 되는 링크 연락처 정보의 임시 메모리 내 뷰입니다. 기본적으로 연락처 프레임 워크는 가능 하면 항상 통합 된 연락처를 반환 합니다.

### <a name="creating-and-updating-contacts"></a>연락처 만들기 및 업데이트

위의 [Contact Objects](#Contact_Objects) 섹션에서 살펴본 것 처럼, 및 인스턴스를 사용 하 여 `CNContactStore` `CNMutableContact` 새 연락처를 만든 다음을 사용 하 여 사용자의 연락처 데이터베이스에 기록 합니다 `CNSaveRequest` .

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

를 `CNSaveRequest` 사용 하 여 여러 연락처 및 그룹 변경 내용을 단일 작업으로 캐시 하 고 이러한 수정 내용을에 일괄 처리할 수도 있습니다 `CNContactStore` .

Fetch 작업에서 가져온 변경할 수 없는 연락처를 업데이트 하려면 먼저 변경 가능한 복사본을 요청한 다음 수정 하 고 연락처 저장소에 다시 저장 해야 합니다. 예를 들면 다음과 같습니다.

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

연락처가 수정 될 때마다 연락처 저장소는를 `CNContactStoreDidChangeNotification` 기본 알림 센터에 게시 합니다. 캐시 되었거나 현재 연락처를 표시 하 고 있는 경우 연락처 저장소 ()에서 해당 개체를 새로 고쳐야 `CNContactStore` 합니다.

### <a name="containers-and-groups"></a>컨테이너 및 그룹

사용자의 연락처는 사용자의 장치에 로컬로 있거나 하나 이상의 서버 계정 (예: Facebook 또는 Google)에서 장치와 동기화 된 연락처로 있을 수 있습니다. 각 연락처 풀에는 자체 _컨테이너가_ 있고 지정 된 연락처는 한 컨테이너에만 존재할 수 있습니다.

[![](contacts-images/containers01.png "Containers and Groups overview")](contacts-images/containers01.png#lightbox)

일부 컨테이너는 연락처를 하나 이상의 _그룹_ 또는 _하위 그룹_으로 정렬할 수 있습니다. 이 동작은 지정 된 컨테이너에 대 한 백업 저장소에 종속 됩니다. 예를 들어 iCloud에는 하나의 컨테이너만 있지만 그룹이 여러 개 있을 수 있지만 하위 그룹은 없습니다. 반면에 Microsoft Exchange는 그룹을 지원 하지 않지만 각 Exchange 폴더에 대해 하나씩 여러 컨테이너를 포함할 수 있습니다.

[![](contacts-images/containers02.png "Overlap within Containers and Groups")](contacts-images/containers02.png#lightbox)

<a name="contactsui"></a>

## <a name="the-contactsui-framework"></a>연락처

응용 프로그램에서 사용자 지정 UI를 제공 하지 않아도 되는 경우에는 ContactsUI 프레임 워크를 사용 하 여 Xamarin.ios 앱에서 연락처를 표시, 편집, 선택 및 만들 수 있는 UI 요소를 제공할 수 있습니다.

Apple의 기본 제공 컨트롤을 사용 하면 Xamarin.ios 앱에서 연락처를 지원 하기 위해 만들어야 하는 코드의 양을 줄일 수 있을 뿐만 아니라 앱의 사용자에 게 일관 된 인터페이스를 제공 합니다.

### <a name="the-contact-picker-view-controller"></a>연락처 선택기 보기 컨트롤러

연락처 선택기 보기 컨트롤러 ()는 사용자가 `CNContactPickerViewController` 사용자의 연락처 데이터베이스에서 연락처 또는 연락처 속성을 선택할 수 있는 표준 연락처 선택기 보기를 관리 합니다. 사용자는 해당 사용량에 따라 하나 이상의 연락처를 선택할 수 있으며, 연락처 선택기 보기 컨트롤러는 선택 항목을 표시 하기 전에 사용 권한을 묻는 메시지를 표시 하지 않습니다.

클래스를 호출 하기 전에 `CNContactPickerViewController` 사용자가 선택할 수 있는 속성을 정의 하 고 연락처 속성의 표시 및 선택을 제어 하는 조건자를 정의 합니다.

에서 상속 되는 클래스의 인스턴스를 사용 `CNContactPickerDelegate` 하 여 선택기와의 사용자 상호 작용에 응답 합니다. 예를 들면 다음과 같습니다.

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

사용자가 데이터베이스의 연락처에서 전자 메일 주소를 선택할 수 있게 하려면 다음 코드를 사용할 수 있습니다.

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

### <a name="the-contact-view-controller"></a>연락처 보기 컨트롤러

Contact View Controller ( `CNContactViewController` ) 클래스는 최종 사용자에 게 표준 연락처 보기를 제공 하는 컨트롤러를 제공 합니다. 연락처 보기는 새로운 새, 알 수 없음 또는 기존 연락처를 표시할 수 있으며, 올바른 정적 생성자 ( `FromNewContact` ,,)를 호출 하 여 뷰가 표시 되기 전에 형식을 지정 해야 합니다 `FromUnknownContact` `FromContact` . 예를 들면 다음과 같습니다.

```csharp
// Create a new contact view
var view = CNContactViewController.FromContact(contact);

// Display the view
PresentViewController(view, true, null);
```

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 응용 프로그램에서 연락처 및 연락처 UI 프레임 워크를 사용 하는 방법에 대해 자세히 살펴봅니다. 먼저, 연락처 프레임 워크에서 제공 하는 다양 한 유형의 개체와 이러한 개체를 사용 하 여 새 사용자를 만들거나 기존 연락처에 액세스 하는 방법을 설명 했습니다. 또한 연락처 UI 프레임 워크를 검토 하 여 기존 연락처를 선택 하 고 연락처 정보를 표시 합니다.

## <a name="related-links"></a>관련 링크

- [연락처 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/contacts/)
- [IOS 9의 새로운 기능](https://developer.apple.com/library/content/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [연락처 프레임 워크 참조](https://developer.apple.com/documentation/contacts?language=objc)
- [연락처 Sui 프레임 워크 참조](https://developer.apple.com/documentation/contactsui?language=objc)
