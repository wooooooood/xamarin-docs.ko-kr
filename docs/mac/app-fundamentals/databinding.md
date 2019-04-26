---
title: 데이터 바인딩 및 Xamarin.Mac 키-값 코딩
description: 이 문서에서는 키-값 코딩 및 Xcode의 Interface Builder에서 UI 요소에 데이터 바인딩할 수 있도록 관찰 키-값을 사용 하 여 설명 합니다.
ms.prod: xamarin
ms.assetid: 72594395-0737-4894-8819-3E1802864BE7
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 4a391160f2102fd1f069a45eb7c16aec91dfd7e0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61428281"
---
# <a name="data-binding-and-key-value-coding-in-xamarinmac"></a>데이터 바인딩 및 Xamarin.Mac 키-값 코딩

_이 문서에서는 키-값 코딩 및 Xcode의 Interface Builder에서 UI 요소에 데이터 바인딩할 수 있도록 관찰 키-값을 사용 하 여 설명 합니다._

## <a name="overview"></a>개요

Xamarin.Mac 응용 프로그램에서 C# 및.NET을 사용 하 여 작업을 하는 경우 동일한 키-값 코딩 및 데이터 바인딩 기술에 액세스할 수 있습니다 하에서 작업을 수행할 *Objective-c* 및 *Xcode* 않습니다. Xcode의 Xamarin.Mac이 Xcode와 직접 통합 되므로 사용할 수 있습니다 _Interface Builder_ 코드를 작성 하는 대신 UI 요소를 사용 하 여 데이터를 바인딩합니다.

키-값 코딩 및 바인딩 기술 Xamarin.Mac 응용 프로그램에서 데이터를 사용 하 여 작성 하 고 채우는 UI 요소를 사용 하 여 작업을 유지 해야 하는 코드의 양을 크게 줄일 수 있습니다. 추가 백업 데이터를 분리의 장점은 수도 있습니다 (_데이터 모델_)에 처음부터 사용자 인터페이스를 종료 합니다 (_모델-뷰-컨트롤러_)를 더 쉬워진 유지 관리를 보다 융통성 있는 응용 프로그램 디자인 합니다.

[![실행 중인 앱의 예가](databinding-images/intro01.png "실행 중인 앱의 예")](databinding-images/intro01-large.png#lightbox)

이 문서에서는 Xamarin.Mac 응용 프로그램의 데이터 바인딩 및 키-값 코딩 작업의 기본 사항을 설명 합니다. 것이 가장 좋습니다를 통해 작업 하는 합니다 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서 합니다 [Xcode 및 Interface Builder 소개](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 하 고 [출 선 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션으로 주요 개념 및이 문서를 사용 하는 기술을 설명 합니다.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명도 문서는 `Register` 및 `Export` 특성 요소 Objective-C 개체 및 UI에 C# 클래스를 연결 하는 데 사용 합니다.

<a name="What_is_Key-Value_Coding" />

## <a name="what-is-key-value-coding"></a>키-값 코딩 하는 것이 무엇입니까

개체의 속성을 직접 액세스, 인스턴스 변수를 통해 액세스 하는 대신 속성을 식별 (특별 한 형식의 문자열) 키 또는 접근자 메서드를 사용 하는 메커니즘은 키-값 코딩 (KVC) (`get/set`). Xamarin.Mac 응용 프로그램에서 키-값 코딩 규격 접근자를 구현, 키-값 관찰 (KVO), 데이터 바인딩, 핵심 데이터, Cocoa 바인딩 및 스크립팅 등과 같은 다른 macOS (OS X 이전의 함) 기능에 대 한 액세스를 얻을 수 있습니다.

키-값 코딩 및 바인딩 기술 Xamarin.Mac 응용 프로그램에서 데이터를 사용 하 여 작성 하 고 채우는 UI 요소를 사용 하 여 작업을 유지 해야 하는 코드의 양을 크게 줄일 수 있습니다. 추가 백업 데이터를 분리의 장점은 수도 있습니다 (_데이터 모델_)에 처음부터 사용자 인터페이스를 종료 합니다 (_모델-뷰-컨트롤러_)를 더 쉬워진 유지 관리를 보다 융통성 있는 응용 프로그램 디자인 합니다. 

예를 들어 KVC 호환 개체의 다음 클래스 정의 살펴보겠습니다.

```csharp
using System;
using Foundation;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        private string _name = "";
        
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }
        
        public PersonModel ()
        {
        }
    }
}
```

첫 번째는 `[Register("PersonModel")]` 특성 클래스를 등록 하 고 목표 C에 노출 그런 다음 클래스에서 상속 해야 `NSObject` (또는 하위 클래스에서 상속 되는 `NSObject`), 여러 기본 클래스에 KVC 호환 되도록 허용 하는 메서드 추가 합니다. 다음으로 `[Export("Name")]` 노출 특성을 `Name` 속성 KVC 및 KVO 기술을 통해 속성에 액세스 하려면 나중에 사용할 키 값을 정의 합니다. 

마지막으로 관찰 된 키-값 속성의 값으로 변경 수 있으려면, 접근자 래핑해야 해당 값의 변경 `WillChangeValue` 하 고 `DidChangeValue` 메서드 호출 (동일한 키로 지정 하는 `Export` 특성).  예를 들어:

```csharp
set {
    WillChangeValue ("Name");
    _name = value;
    DidChangeValue ("Name");
}
```

이 단계는 _매우_ Xcode에서 데이터 바인딩에 대 한 중요 한의 Interface Builder (보듯이이 문서의 뒷부분에서).

자세한 내용은 Apple의를 참조 하세요 [키-값 코딩 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)합니다.

### <a name="keys-and-key-paths"></a>키 및 키 경로

A _키_ 는 개체의 특정 속성을 식별 하는 문자열입니다. 일반적으로 키를 키-값 코딩 준수 개체에에서 접근자 메서드의 이름에 해당 합니다. 키 ASCII 인코딩을 사용 해야 일반적으로 소문자로 시작 하 고 공백을 포함할 수 없습니다. 따라서 위 예제에서 지정 된 `Name` 의 키 값이 일 `Name` 의 속성을 `PersonModel` 클래스입니다. 노출 된 속성의 이름과 키 필요가 같을 수 있는 대부분의 경우.

A _키 경로_ 점 문자열로 구분 이동할 개체 속성의 계층을 지정 하는 데 사용 되는 키입니다. 받는 사람을 기준으로 시퀀스의 첫 번째 키의 속성 이며 각 후속 키 이전 속성 값을 기준으로 평가 됩니다. 동일한 방식으로 개체 및 C# 클래스에서 해당 속성을 이동할 점 표기법을 사용 합니다.

예를 들어 확장 합니다 `PersonModel` 클래스 및 추가 `Child` 속성:

```csharp
using System;
using Foundation;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        private string _name = "";
        private PersonModel _child = new PersonModel();
        
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }
        
        [Export("Child")]
        public PersonModel Child {
            get { return _child; }
            set {
                WillChangeValue ("Child");
                _child = value;
                DidChangeValue ("Child");
            }
        }
        
        public PersonModel ()
        {
        }
    }
}
```

자식의 이름을 키 경로 `self.Child.Name` 또는 간단히 `Child.Name` (키 값을가 사용 되는 방식을 기반 함).

### <a name="getting-values-using-key-value-coding"></a>키-값 코딩으로 값 가져오기

합니다 `ValueForKey` 메서드는 지정된 된 키의 값을 반환 합니다. (으로 `NSString`)을 기준으로 요청을 받는 KVC 클래스의 인스턴스. 예를 들어 경우 `Person` 의 인스턴스는 `PersonModel` 위에 정의 된 클래스:

```csharp
// Read value 
var name = Person.ValueForKey (new NSString("Name"));
```

값이 반환 된 `Name` 의 해당 인스턴스에 대 한 속성 `PersonModel`. 

### <a name="setting-values-using-key-value-coding"></a>키-값 코딩을 사용 하 여 값 설정

마찬가지로, 합니다 `SetValueForKey` 지정된 된 키의 값을 설정 (으로 `NSString`)을 기준으로 요청을 받는 KVC 클래스의 인스턴스. 이번에 인스턴스를 사용 하는 `PersonModel` 아래와 같이 클래스:

```csharp
// Write value
Person.SetValueForKey(new NSString("Jane Doe"), new NSString("Name"));
```

값을 변경 합니다 `Name` 속성을 `Jane Doe`입니다.

<a name="Observing_Value_Changes" />

### <a name="observing-value-changes"></a>관찰 값이 변경

키-값 관찰 (KVO)를 사용 하 관찰자 KVC 호환 클래스의 특정 키를 연결할 수 있는 하 고 해당 키에 대 한 값 (KVC 기술을 사용 하 여 또는 C# 코드에서 지정된 된 속성에 직접 액세스) 만들어질 때마다 알림을 받을 합니다. 예를 들어:

```csharp
// Watch for the name value changing
Person.AddObserver ("Name", NSKeyValueObservingOptions.New, (sender) => {
    // Inform caller of selection change
    Console.WriteLine("New Name: {0}", Person.Name)
});
```

이제 언제 든 지는 `Name` 의 속성을 `Person` 인스턴스의 `PersonModel` 클래스 수정 되 면 새 값에 작성 되어 콘솔. 

자세한 내용은 Apple의를 참조 하세요 [키 값을 관찰 프로그래밍 가이드 소개](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i)합니다.

## <a name="data-binding"></a>데이터 바인딩

다음 섹션에서는 데이터를 읽고 C# 코드를 사용 하 여 값을 작성 하는 대신 Xcode의 Interface Builder에서 UI 요소에 바인딩하는 키-값 코딩 및 키-값 관찰 호환 클래스를 사용 하는 방법을 알아보겠습니다. 이러한 방식으로 별도 사용자 _데이터 모델_ 표시 하는 데 사용 되는 뷰에서 Xamarin.Mac 응용 프로그램 더 유연 하 고 쉽게 유지 관리할 수 있도록 합니다. 작성 하는 코드의 양을 크게 줄일 있습니다.

<a name="Defining_your_Data_Model" />

### <a name="defining-your-data-model"></a>데이터 모델 정의

Xamarin.Mac 응용 프로그램 역할을 하도록 정의 된 KVC/KVO 호환 클래스 수행 해야 하기 전에 데이터를 바인딩할 Interface Builder에서 UI 요소에는 _데이터 모델_ 바인딩에 대 한 합니다. 데이터 모델의 모든 사용자 인터페이스에 표시 됩니다 하 고 응용 프로그램을 실행 하는 동안 UI에서 사용자가 데이터에 대 한 수정은 받는 데이터를 제공 합니다.

예를 들어 직원의 그룹을 관리 하는 응용 프로그램을 작성 하는 경우 데이터 모델을 정의 하는 다음 클래스를 사용할 수 있습니다.

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        #endregion

        #region Computed Properties
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon {
            get {
                if (isManager) {
                    return NSImage.ImageNamed ("group.png");
                } else {
                    return NSImage.ImageNamed ("user.png");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion
    }
}
```

이 클래스의 기능 중 대부분은 검사 된 합니다 [란, 키-값 코딩](#What_is_Key-Value_Coding) 위의 섹션입니다. 그러나 살펴보겠습니다 몇 가지 특정 요소 및이 클래스에 대 한 데이터 모델로 작동 하도록 허용에 대 한 몇 가지 추가 **배열 컨트롤러** 하 고 **트리 컨트롤러** (하는 데이터를 나중에 사용 바인딩할 **트리 뷰**를 **개요 보기** 하 고 **컬렉션 뷰**).

먼저 직원 관리자 일 수 있으므로 사용한를 `NSArray` (특히는 `NSMutableArray` 값을 수정할 수 있도록)에 연결할 관리 되는 직원 수 있도록:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
```

여기에 두 가지가 있습니다.

1. 사용 하는 `NSMutableArray` 표준 C# 배열 또는 컬렉션 이므로 AppKit 컨트롤에 데이터 바인딩할 요구 사항 등을 대신 **테이블 뷰**, **개요 보기** 및 **컬렉션** .
2. 직원의 배열에 캐스팅 하 여 노출 했습니다를 `NSArray` 데이터에 대 한 바인딩 목적 및 변경 해당 C# 이름, 형식 `People`, 데이터 바인딩에서 예상 하는 하나에 `personModelArray` 형태로 **{0} class_name} 배열**(첫 번째 문자가 소문자로 변환 수행 되었음을 참고).

다음으로, 일부 특수 이름 공용 지 원하는 메서드를 추가 해야 **배열 컨트롤러** 하 고 **트리 컨트롤러**:

```csharp
[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    isManager = true;
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

컨트롤러를 요청 하 고 표시 하는 데이터를 수정할 수 있습니다. 노출 된 같은 `NSArray` (일반적인 C# 명명 규칙에서 서로 다름)는 매우 특정 명명 규칙을 위와 있습니다.

- `addObject:` -배열에 개체를 추가합니다.
- `insertObject:in{class_name}ArrayAtIndex:` -여기서 `{class_name}` 클래스의 이름입니다. 이 메서드는 개체 배열의 지정된 된 인덱스에 삽입합니다.
- `removeObjectFrom{class_name}ArrayAtIndex:` -여기서 `{class_name}` 클래스의 이름입니다. 이 메서드는 지정된 된 인덱스 배열의 개체를 제거합니다.
- `set{class_name}Array:` -여기서 `{class_name}` 클래스의 이름입니다. 이 메서드를 사용 하면 기존 전달 새 항목으로 교체할 수 있습니다.

이러한 메서드 내에서 배열에 대 한 변경 내용 래핑 했습니다 했습니다 `WillChangeValue` 고 `DidChangeValue` KVO 규정 준수에 대 한 메시지입니다.

이후 마지막으로 `Icon` 속성의 값에 의존 합니다 `isManager` 속성을 변경를 `isManager` 속성에 반영 되지 않을 수 있습니다는 `Icon` (KVO) 중 데이터 바인딩된 UI 요소에 대 한:

```csharp
[Export("Icon")]
public NSImage Icon {
    get {
        if (isManager) {
            return NSImage.ImageNamed ("group.png");
        } else {
            return NSImage.ImageNamed ("user.png");
        }
    }
}
``` 

문제를 해결 하려면 다음 코드를 사용 했습니다.

```csharp
[Export("isManager")]
public bool isManager {
    get { return _isManager; }
    set {
        WillChangeValue ("isManager");
        WillChangeValue ("Icon");
        _isManager = value;
        DidChangeValue ("isManager");
        DidChangeValue ("Icon");
    }
}
```

자체 키 외에도 유의 합니다 `isManager` 접근자를 보내고 합니다 `WillChangeValue` 및 `DidChangeValue` 에 대 한 메시지는 `Icon` 도 변경 내용이 반영 하므로 키.

사용 된 `PersonModel` 이 문서의 나머지 부분 전체에서 데이터 모델입니다.

<a name="Simple_Data_Binding" />

### <a name="simple-data-binding"></a>단순 데이터 바인딩

정의 된 데이터 모델을 사용 하 여 살펴보겠습니다 Xcode의 Interface Builder의 데이터 바인딩에 대 한 간단한 예제입니다. 폼 편집에 사용할 수 있는 Xamarin.Mac 응용 프로그램을 추가 해 보겠습니다에 예를 들어를 `PersonModel` 위에 정의 된 것입니다. 몇 가지 텍스트 필드와 확인란을 표시 하 고 모델의 속성을 편집 추가할 것입니다.

먼저 새를 추가 해 보겠습니다 **뷰 컨트롤러** 에 우리의 **Main.storyboard** Interface Builder에서 파일 및 해당 클래스 이름을 `SimpleViewController`: 

[![새 뷰 컨트롤러를 추가](databinding-images/simple01.png "새 뷰 컨트롤러를 추가 합니다.")](databinding-images/simple01-large.png#lightbox)

Mac 용 Visual Studio로 돌아가서 다음으로, 편집 합니다 **SimpleViewController.cs** 파일 (프로젝트에 자동으로 추가 된)의 인스턴스를 노출 하 고는 `PersonModel` 데이터를 양식에 바인딩 드리겠습니다. 다음 코드를 추가합니다.

```csharp
private PersonModel _person = new PersonModel();
...

[Export("Person")]
public PersonModel Person {
    get {return _person; }
    set {
        WillChangeValue ("Person");
        _person = value;
        DidChangeValue ("Person");
    }
}
```

다음 뷰가 로드 되 면 만들겠습니다 인스턴스의 우리의 `PersonModel` 이 코드를 입력 합니다.

```csharp
public override void ViewDidLoad ()
{
    base.AwakeFromNib ();

    // Set a default person
    var Craig = new PersonModel ("Craig Dunn", "Documentation Manager");
    Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    Person = Craig;

}
```

이제 이로써 폼이 해야를 두 번 클릭 합니다 **Main.storyboard** Interface Builder에서 편집용으로 열 파일입니다. 레이아웃 폼을 다음과 같이 표시 합니다.

[![Xcode에서 스토리 보드를 편집](databinding-images/simple02.png "Xcode에서 스토리 보드를 편집 합니다.")](databinding-images/simple02-large.png#lightbox)

데이터 바인딩할 폼에는 `PersonModel` 를 통해 노출 된 것입니다는 `Person` 다음을 수행 하는 키:

1. 선택 합니다 **Employee Name** 텍스트 필드 및 스위치를 **바인딩 검사기**.
2. 확인 합니다 **바인딩할** 선택한 상자 **단순 뷰 컨트롤러** 드롭다운 목록에서. 그런 다음 입력 `self.Person.Name` 에 대 한 합니다 **키 경로**: 

    [![키 경로 입력](databinding-images/simple03.png "키 경로 입력 합니다.")](databinding-images/simple03-large.png#lightbox)
3. 선택 합니다 **Occupation** 텍스트 필드 및 확인 합니다 **바인딩할** 상자를 선택 **간단한 뷰 컨트롤러** 드롭다운 목록에서. 그런 다음 입력 `self.Person.Occupation` 에 대 한 합니다 **키 경로**:  

    [![키 경로 입력](databinding-images/simple04.png "키 경로 입력 합니다.")](databinding-images/simple04-large.png#lightbox)
4. 선택 합니다 **직원은 관리자** 확인란을 확인 하 고는 **바인딩할** 상자를 선택 **간단한 뷰 컨트롤러** 드롭다운 목록에서. 그런 다음 입력 `self.Person.isManager` 에 대 한 합니다 **키 경로**:  

    [![키 경로 입력](databinding-images/simple05.png "키 경로 입력 합니다.")](databinding-images/simple05-large.png#lightbox)
5. 선택 합니다 **수의 직원 관리** 텍스트 필드 및 확인 합니다 **바인딩할** 상자를 선택 **간단한 뷰 컨트롤러** 드롭다운 목록에서. 그런 다음 입력 `self.Person.NumberOfEmployees` 에 대 한 합니다 **키 경로**:  

    [![키 경로 입력](databinding-images/simple06.png "키 경로 입력 합니다.")](databinding-images/simple06-large.png#lightbox)
6. 직원 관리자가 수의 직원 관리 되는 레이블 및 텍스트 필드를 숨기려면 하고자 합니다.
7. 선택는 **수의 직원 관리** 레이블을 확장를 **Hidden** 접혀 있는 부분을 확인 합니다 **바인딩할** 상자를 선택 **간단한 뷰 컨트롤러** 드롭다운 목록에서. 그런 다음 입력 `self.Person.isManager` 에 대 한 합니다 **키 경로**:  

    [![키 경로 입력](databinding-images/simple07.png "키 경로 입력 합니다.")](databinding-images/simple07-large.png#lightbox)
8. 선택 `NSNegateBoolean` 에서 합니다 **값 변환기** 드롭다운:  

    ![NSNegateBoolean 키 변환을 선택](databinding-images/simple08.png "NSNegateBoolean 키 변환을 선택")
9. 데이터 바인딩을 설정 하는 경우 레이블을 숨길 수는 그러면 값을 `isManager` 속성은 `false`합니다.
10. 7 및 8 단계를 반복 합니다 **수의 직원 관리** 텍스트 필드입니다.
11. 변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

응용 프로그램에서 값을 실행 하는 경우는 `Person` 속성 폼을 자동으로 입력 됩니다.

[![자동으로 채워진 폼을 보여 주는](databinding-images/simple09.png "를 자동으로 채워진 형식 표시")](databinding-images/simple09-large.png#lightbox)

폼의 사용자가 모든 변경 내용에 다시 기록 됩니다는 `Person` 뷰 컨트롤러의 속성입니다. 예를 들어, 선택 취소 **직원은 관리자** 업데이트를 `Person` 인스턴스의 우리의 `PersonModel` 및 **수의 직원 관리** 레이블 및 텍스트 필드 (을 통해 자동으로 숨겨진 데이터 바인딩):

[![비 관리자에 대 한 직원 수를 숨기기](databinding-images/simple10.png "아닌 관리자에 대 한 직원 수를 숨기기")](databinding-images/simple10-large.png#lightbox)

<a name="Table_View_Data_Binding" />

### <a name="table-view-data-binding"></a>테이블 뷰 데이터 바인딩

이제 데이터를 바인딩하는 기본적인 했으므로 살펴보겠습니다 더 복잡 한 데이터 바인딩 작업을 사용 하 여는 _배열 컨트롤러_ 및 표 보기로 데이터 바인딩. 테이블 뷰를 사용 하 여 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [테이블 뷰](~/mac/user-interface/table-view.md) 설명서.

먼저 새를 추가 해 보겠습니다 **뷰 컨트롤러** 에 우리의 **Main.storyboard** Interface Builder에서 파일 및 해당 클래스 이름을 `TableViewController`:

[![새 뷰 컨트롤러를 추가](databinding-images/table01.png "새 뷰 컨트롤러를 추가 합니다.")](databinding-images/table01-large.png#lightbox)

다음으로, 편집할를 **TableViewController.cs** 파일 (프로젝트에 자동으로 추가 된) 및 배열 노출 (`NSArray`)의 `PersonModel` 클래스는 데이터를 양식에 바인딩 수입니다. 다음 코드를 추가합니다.

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

수행한 것 처럼를 `PersonModel` 위의 클래스를 [데이터 모델을 정의](#Defining_your_Data_Model) 섹션인 4 특별 하 게 명명 된 공용 메서드를 공개 했습니다 있도록 배열 컨트롤러 및 읽기 및 쓰기 데이터의 컬렉션에서 `PersonModels`.

그런 다음 뷰가 로드 되 면이 코드를 사용 하 여이 배열을 채우는를 해야 합니다.

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    AddPerson (new PersonModel ("Craig Dunn", "Documentation Manager", true));
    AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (new PersonModel ("Larry O'Brien", "API Documentation Manager", true));
    AddPerson (new PersonModel ("Mike Norman", "API Documenter"));

}
```

이제이 테이블 뷰를 작성 해야를 두 번 클릭 합니다 **Main.storyboard** Interface Builder에서 편집용으로 열 파일입니다. 레이아웃은 다음과 같이 표시 합니다.

[![새 테이블 뷰를 레이아웃할](databinding-images/table02.png "새 테이블 뷰 레이아웃")](databinding-images/table02-large.png#lightbox)

추가 해야는 **배열 컨트롤러** 테이블에 바인딩된 데이터를 제공 하려면 다음을 수행 합니다.

1. 끌어서를 **컨트롤러 배열** 에서 합니다 **라이브러리 검사기** 에 **인터페이스 편집기**:  

    ![라이브러리에서 배열 컨트롤러를 선택 하면](databinding-images/table03.png "라이브러리에서 배열 컨트롤러 선택")
2. 선택 **배열 컨트롤러** 에 **인터페이스 계층 구조** 전환 하는 **특성 검사기**:  

    [![특성 검사기 선택](databinding-images/table04.png "특성 검사기를 선택 합니다.")](databinding-images/table04-large.png#lightbox)
3. 입력 `PersonModel` 에 대 한는 **클래스 이름**를 클릭 합니다 **Plus** 단추를 세 개의 키를 추가 합니다. 이름을 지정 하 여 `Name`하십시오 `Occupation` 고 `isManager`:  

    ![필요한 키 경로 추가](databinding-images/table05.png "필요한 키 경로 추가 합니다.")
4. 이렇게 하면 모든 속성 배열 컨트롤러의 배열을 관리 중인 새로운 속성 (키)를 통해 노출 해야 하 고 있습니다.
5. 으로 전환 합니다 **바인딩 검사기** 아래에서 **콘텐츠 배열** 선택 **바인딩할** 및 **테이블 뷰 컨트롤러**합니다. 입력 한 **키 경로 모델** 의 `self.personModelArray`:  

    ![키 경로 입력](databinding-images/table06.png "키 경로 입력 합니다.")
6. 이 연결의 배열에 배열 컨트롤러 `PersonModels` 보기 컨트롤러에 노출 하는 것입니다.

이제 테이블 뷰에 배열 컨트롤러에 바인딩할 해야, 다음을 수행 합니다.

1. 테이블 보기를 선택 하며 **바인딩 검사기**:  

    [![바인딩 검사기 선택](databinding-images/table07.png "바인딩 검사기를 선택 합니다.")](databinding-images/table07-large.png#lightbox)
2. 아래는 **목차** 접혀 있는 부분을 선택 **바인딩할** 하 고 **배열 컨트롤러**합니다. 입력 `arrangedObjects` 에 대 한 합니다 **컨트롤러 키** 필드:  

    ![컨트롤러 키를 정의](databinding-images/table08.png "컨트롤러 키를 정의 합니다.")
3. 선택 합니다 **테이블 뷰 셀** 아래 합니다 **직원** 열입니다. 에 **바인딩 검사기** 아래에서 **값** 접혀 있는 부분을 선택 **바인딩할** 및 **테이블 셀 뷰**합니다. 입력 `objectValue.Name` 에 대 한 합니다 **키 경로 모델**:  

    [![모델 키 경로 설정](databinding-images/table09.png "모델 키 경로 설정 합니다.")](databinding-images/table09-large.png#lightbox)
4. `objectValue` 현재 `PersonModel` 배열 컨트롤러에서 관리 되는 배열입니다.
5. 선택 합니다 **테이블 뷰 셀** 아래 합니다 **직업** 열. 에 **바인딩 검사기** 아래에서 **값** 접혀 있는 부분을 선택 **바인딩할** 및 **테이블 셀 뷰**합니다. 입력 `objectValue.Occupation` 에 대 한 합니다 **키 경로 모델**:  

    [![모델 키 경로 설정](databinding-images/table10.png "모델 키 경로 설정 합니다.")](databinding-images/table10-large.png#lightbox)
6. 변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

테이블 배열을 사용 하 여 채워집니다 응용 프로그램을 실행 하는 경우 `PersonModels`:

[![응용 프로그램을 실행](databinding-images/table11.png "응용 프로그램 실행")](databinding-images/table11-large.png#lightbox)

<a name="Outline_View_Data_Binding" />

### <a name="outline-view-data-binding"></a>개요 뷰 데이터 바인딩

한 개요 보기에 대해 데이터 바인딩을 바인딩 표 보기에 대해 매우 비슷합니다. 주요 차이점은 사용 하는 **트리 컨트롤러** 대신는 **배열 컨트롤러** 개요 보기에 바인딩된 데이터를 제공 합니다. 작업 개요 보기에 대 한 자세한 내용은 참조 하십시오 우리의 [개요 보기](~/mac/user-interface/outline-view.md) 설명서.

먼저 새를 추가 해 보겠습니다 **뷰 컨트롤러** 에 우리의 **Main.storyboard** Interface Builder에서 파일 및 해당 클래스 이름을 `OutlineViewController`: 

[![새 뷰 컨트롤러를 추가](databinding-images/outline01.png "새 뷰 컨트롤러를 추가 합니다.")](databinding-images/outline01-large.png#lightbox)

다음으로, 편집할를 **OutlineViewController.cs** 파일 (프로젝트에 자동으로 추가 된) 및 배열 노출 (`NSArray`)의 `PersonModel` 클래스는 데이터를 양식에 바인딩 수입니다. 다음 코드를 추가합니다.

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

수행한 것 처럼를 `PersonModel` 위의 클래스를 [데이터 모델을 정의](#Defining_your_Data_Model) 섹션인 4 특별 하 게 명명 된 공용 메서드를 공개 했습니다 있도록 트리 컨트롤러 및 읽기 및 쓰기 데이터의 컬렉션에서 `PersonModels`.

그런 다음 뷰가 로드 되 면이 코드를 사용 하 여이 배열을 채우는를 해야 합니다.

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    var Craig = new PersonModel ("Craig Dunn", "Documentation Manager");
    Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (Craig);

    var Larry = new PersonModel ("Larry O'Brien", "API Documentation Manager");
    Larry.AddPerson (new PersonModel ("Mike Norman", "API Documenter"));
    AddPerson (Larry);

}
```

개요 보기를 만들려면 먼저 이제 두 번 클릭 합니다 **Main.storyboard** Interface Builder에서 편집용으로 열 파일입니다. 레이아웃은 다음과 같이 표시 합니다.

[![개요 뷰를 만드는](databinding-images/outline02.png "개요 보기 만들기")](databinding-images/outline02-large.png#lightbox)

추가 해야는 **트리 컨트롤러** 바인딩된 데이터는 개요를 제공 하려면 다음을 수행 합니다.

1. 끌어서를 **컨트롤러 트리** 에서 합니다 **라이브러리 검사기** 에 **인터페이스 편집기**:  

    ![라이브러리에서 트리 컨트롤러를 선택할](databinding-images/outline03.png "라이브러리에서 트리 컨트롤러를 선택 하면")
2. 선택 **트리 컨트롤러** 에 **인터페이스 계층 구조** 전환 하는 **특성 검사기**:  

    [![특성 검사기 선택](databinding-images/outline04.png "특성 검사기를 선택 합니다.")](databinding-images/outline04-large.png#lightbox)
3. 입력 `PersonModel` 에 대 한는 **클래스 이름**를 클릭 합니다 **Plus** 단추를 세 개의 키를 추가 합니다. 이름을 지정 하 여 `Name`하십시오 `Occupation` 고 `isManager`:  

    ![필요한 키 경로 추가](databinding-images/outline05.png "필요한 키 경로 추가 합니다.")
4. 배열, 관리 중인 새로운 트리 컨트롤러를 이렇게 하면 속성 (키)를 통해 노출 해야 하 고 있습니다.
5. 아래는 **트리 컨트롤러** 섹션에서 입력 `personModelArray` 에 대 한 **자식**, 입력 `NumberOfEmployees` 아래를 **개수** enter `isEmployee` 에서 **리프**:  

    ![트리 컨트롤러 키 경로 설정](databinding-images/outline05.png "트리 컨트롤러 키 경로 설정 합니다.")
6. 이렇게 하면 트리 컨트롤러 노드, 자식 노드 수 있습니다 및 현재 노드에 자식 노드가 있으면 모든 자식을 찾을 수 있는 위치입니다.
7. 으로 전환 합니다 **바인딩 검사기** 아래에서 **콘텐츠 배열** 선택 **바인딩할** 및 **파일의 소유자**합니다. 입력 한 **키 경로 모델** 의 `self.personModelArray`:  

    ![키 경로 편집](databinding-images/outline06.png "키 경로 편집 합니다.")
8. 이 연결의 배열에는 트리 컨트롤러 `PersonModels` 보기 컨트롤러에 노출 하는 것입니다.

이제 트리 컨트롤러 개요 뷰에 바인딩할 해야, 다음을 수행 합니다.

1. 개요 뷰에서 선택 및 합니다 **바인딩 검사기** 선택:  

    [![바인딩 검사기 선택](databinding-images/outline07.png "바인딩 검사기를 선택 합니다.")](databinding-images/outline07-large.png#lightbox)
2. 아래는 **내용 보기 개요** 접혀 있는 부분을 선택 **바인딩할** 하 고 **트리 컨트롤러**. 입력 `arrangedObjects` 에 대 한 합니다 **컨트롤러 키** 필드:  

    ![키를 설정 하면 컨트롤러](databinding-images/outline08.png "컨트롤러 키 설정")
3. 선택 합니다 **테이블 뷰 셀** 아래 합니다 **직원** 열입니다. 에 **바인딩 검사기** 아래에서 **값** 접혀 있는 부분을 선택 **바인딩할** 및 **테이블 셀 뷰**합니다. 입력 `objectValue.Name` 에 대 한 합니다 **키 경로 모델**:  

    [![모델 키 경로 입력](databinding-images/outline09.png "모델 키 경로 입력 합니다.")](databinding-images/outline09-large.png#lightbox)
4. `objectValue` 현재 `PersonModel` 트리 컨트롤러에서 관리 되는 배열입니다.
5. 선택 합니다 **테이블 뷰 셀** 아래 합니다 **직업** 열. 에 **바인딩 검사기** 아래에서 **값** 접혀 있는 부분을 선택 **바인딩할** 및 **테이블 셀 뷰**합니다. 입력 `objectValue.Occupation` 에 대 한 합니다 **키 경로 모델**:  

    [![모델 키 경로 입력](databinding-images/outline10.png "모델 키 경로 입력 합니다.")](databinding-images/outline10-large.png#lightbox)
6. 변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

윤곽선의 배열으로 채워집니다 응용 프로그램을 실행 하는 경우 `PersonModels`:

[![응용 프로그램을 실행](databinding-images/outline11.png "응용 프로그램 실행")](databinding-images/outline11-large.png#lightbox)

### <a name="collection-view-data-binding"></a>컬렉션 뷰 데이터 바인딩

컬렉션에 대 한 데이터를 제공 하는 배열 컨트롤러 사용 되는 컬렉션 뷰를 사용 하 여 데이터 바인딩은 표 보기를 사용 하 여 바인딩과 매우 유사 합니다. 컬렉션 뷰에서 미리 설정 된 표시 형식 없으므로 더 많은 작업이 필요 사용자 상호 작용 피드백을 제공 하 고 사용자가 선택한 추적 합니다.

> [!IMPORTANT]
> Xcode 7 및 macOS 10.11 (이상)는 문제로 인해 컬렉션 뷰 스토리 보드 (.storyboard) 파일 내에서 사용할 수 없는 합니다. 결과적으로, 계속 하려면.xib 파일을 사용 하 여 Xamarin.Mac 앱에 대 한 컬렉션 보기를 정의 해야 합니다. 참조 하십시오 우리의 [컬렉션 뷰](~/mac/user-interface/collection-view.md) 자세한 정보에 대 한 설명서입니다.

<!--KKM 012/16/2015 - Once Apple fixes the issue with Xcode and Collection Views in Storyboards, we can uncomment this section.

First, let's add a new **View Controller** to our **Main.storyboard** file in Interface Builder and name its class `CollectionViewController`: 

![](databinding-images/collection01.png)

Next, let's edit the **CollectionViewController.cs** file (that was automatically added to our project) and expose an array (`NSArray`) of `PersonModel` classes that we will be data binding our form to. Add the following code:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Just like we did on the `PersonModel` class above in the [Defining your Data Model](#Defining_your_Data_Model) section, we've exposed four specially named public methods so that the Array Controller and read and write data from our collection of `PersonModels`.

Next when the View is loaded, we need to populate our array with this code:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    AddPerson (new PersonModel ("Craig Dunn", "Documentation Manager", true));
    AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (new PersonModel ("Larry O'Brien", "API Documentation Manager", true));
    AddPerson (new PersonModel ("Mike Norman", "API Documenter"));

}
```

Now we need to create our Collection View, double-click the **Main.storyboard** file to open it for editing in Interface Builder. Layout the Collection View to look something like the following:

![](databinding-images/collection02.png)

When you add a Collection View to a User Interface design, two extra elements are also added:

1.  **Collection View Item** -  That manages a single instance of an item in the collection.
2.  **View** - A custom view that provides the visual size and appearance of each item in the collection. This view is tied to and managed by the **Collection View Item**.

Select the view and make it look like the following using an Image View and two Text Fields:

![](databinding-images/collection03.png)

One thing to note here, a `NSBox` was added behind everything in the view with the following attributes:

![](databinding-images/collection04.png)

We'll be using this box to provide feedback to the user when an item is selected in the Collection View.

We need to add an **Array Controller** to provide bound data to our collection, do the following:

1. Drag an **Array Controller** from the **Library Inspector** onto the **Interface Editor**: <br/>![](databinding-images/table03.png)
2. Select **Array Controller** in the **Interface Hierarchy** and switch to the **Attribute Inspector**: <br/>![](databinding-images/collection05.png)
3. Enter `PersonModel` for the **Class Name**, click the **Plus** button and add four Keys. Name them `Icon`, `Name`, `Occupation` and `isManager`: <br/>![](databinding-images/table05.png)
4. This tells the Array Controller what it is managing an array of, and which properties it should expose (via Keys).
5. Switch to the **Bindings Inspector** and under **Content Array** select **Bind to** and **File's Owner**. Enter a **Model Key Path** of `self.personModelArray`: <br/>![](databinding-images/table06.png)
6. This ties the Array Controller to the array of `PersonModels` that we exposed on our View Controller.

Now we need to bind our Collection View to the Array Controller, do the following:

1. Select the Collection View and the **Binding Inspector**: <br/>![](databinding-images/collection06.png)
2. Under the **Contents** turndown, select **Bind to** and **Array Controller**. Enter `arrangedObjects` for the **Controller Key** field: <br/>![](databinding-images/collection07.png)
3. Under the **Selection Indexes** turndown, select **Bind to** and **Array Controller**. Enter `selectionIndexes` for the **Controller Key** field: <br/>![](databinding-images/collection08.png)
4. Select the **Image View**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Person** (the name of our Collection View Item). Enter `representedObject.Icon` for the **Model Key Path**: <br/>![](databinding-images/collection09.png)
5. `representedObject` is the current `PersonModel` in the array being managed by the Array Controller.
6. Select the first **Label**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Collection View Item**. Enter `representedObject.Name` for the **Model Key Path**: <br/>![](databinding-images/collection10.png)
7. Select the second **Label**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Collection View Item**. Enter `representedObject.Occupation` for the **Model Key Path**: <br/>![](databinding-images/collection11.png)
8. Select the `NSBox`. In the **Bindings Inspector** under the **Hidden** turndown, select **Bind to** and **Collection View Item**. Enter `selected` for the **Model Key Path** and `NSNegateBoolean` for the **Value Transformer**: <br/>![](databinding-images/collection12.png)
9. Save your changes and return to Visual Studio for Mac to sync with Xcode.

If we run the application, the table will be populated with our array of `PersonModels`:

![](databinding-images/collection13.png)

For more information on working with Collection Views, please see our [Collection Views](~/mac/user-interface/collection-view.md) documentation.-->

## <a name="debugging-native-crashes"></a>네이티브 크래시 디버깅

실수 데이터 바인딩에서 발생 수를 _네이티브 크래시_ 비관리 코드와 완전히 실패 하 게 사용 하 여 Xamarin.Mac 응용 프로그램을 `SIGABRT` 오류:

[![네이티브 크래시 대화 상자의 예](databinding-images/debug01.png "네이티브 크래시 대화 상자의 예")](databinding-images/debug01-large.png#lightbox)

데이터 바인딩 중 네 가지 주요 원인은 네이티브 크래시는 일반적으로:

1. 데이터 모델에서 상속 되지 않습니다 `NSObject` 또는 하위 클래스 `NSObject`합니다.
2. Objective-c로 사용 하 여 속성을 노출 하지 않은 `[Export("key-name")]` 특성입니다.
3. 접근자의 값의 변경에 배치 되지 않았습니다 `WillChangeValue` 하 고 `DidChangeValue` 메서드 호출 (으로 동일한 키를 지정 하는 `Export` 특성).
4. 에 잘못 되었거나 잘못 입력 된 키가 있습니다 합니다 **바인딩 검사기** Interface Builder에서.

### <a name="decoding-a-crash"></a>충돌을 디코딩

보겠습니다 네이티브 크래시에에서 발생할 데이터 바인딩의 있으므로 찾기 및 해결 방법에 살펴봅니다. Interface Builder에서 컬렉션 보기 예제에서 첫 번째 레이블의 바인딩의 변경해 보겠습니다 `Name` 에 `Title`:

[![바인딩 키를 편집](databinding-images/debug02.png "바인딩 키를 편집 합니다.")](databinding-images/debug02-large.png#lightbox)

변경 내용을 저장 하겠습니다 Xcode와 동기화 하 여 응용 프로그램을 실행 하는 Mac 용 Visual Studio로 다시 전환 합니다. 컬렉션 뷰에서 표시 되 면 응용 프로그램 사용 하 여 일시적으로 중단 됩니다는 `SIGABRT` 오류 (에 표시 된 대로 합니다 **응용 프로그램 출력** Mac 용 Visual Studio에서) 하므로 `PersonModel` 키를 사용 하 여 속성을 노출 하지 않습니다 `Title`:

[![바인딩 오류 예가](databinding-images/debug03.png "바인딩 오류의 예")](databinding-images/debug03-large.png#lightbox)

맨 위의에서 오류의으로 스크롤하면 합니다 **응용 프로그램 출력** 문제를 해결 하기 위한 키를 볼 수 있습니다.

[![오류 로그에서 문제를 찾는](databinding-images/debug04.png "오류 로그에서 문제 찾기")](databinding-images/debug04-large.png#lightbox)

이 줄은 표시 되는데 키 `Title` 에 바인딩하므로 개체에 존재 하지 않습니다. 바인딩을 다시 변경 하면 `Name` Interface Builder를 저장, 동기화, 다시 작성 및 실행, 응용 프로그램이 예상 대로 실행 됩니다 문제 없이 합니다.

## <a name="summary"></a>요약

이 문서에서는 자세히 살펴보고 데이터 바인딩 및 Xamarin.Mac 응용 프로그램에서 키-값 코딩 작업을 수행 했습니다. 첫째,이 키-값 관찰 (KVO) 및 키-값 코딩 (KVC)를 사용 하 여 Objective-c를 C# 클래스를 노출 살펴보았습니다. 다음으로 KVO 호환 클래스를 사용 하는 방법에 알아보았습니다 하 고 데이터 Xcode의 Interface Builder에서 UI 요소에 바인딩합니다. 마지막으로 설명 했습니다 복잡 한 데이터 바인딩을 사용 하 여 **배열 컨트롤러** 하 고 **트리 컨트롤러**합니다.


## <a name="related-links"></a>관련 링크

- [MacDatabinding 스토리 보드 (샘플)](https://developer.xamarin.com/samples/mac/MacDatabinding-Storyboard/)
- [MacDatabinding Xib (샘플)](https://developer.xamarin.com/samples/mac/MacDatabinding-XIBs/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [표준 컨트롤](~/mac/user-interface/standard-controls.md)
- [테이블 뷰](~/mac/user-interface/table-view.md)
- [개요 보기](~/mac/user-interface/outline-view.md)
- [컬렉션 뷰](~/mac/user-interface/collection-view.md)
- [키-값 코딩 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [키-값 관찰 프로그래밍 가이드 소개](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html)
- [Cocoa 바인딩 프로그래밍 항목 소개](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Cocoa 바인딩 참조 소개](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [macOS 휴먼 인터페이스 지침](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
