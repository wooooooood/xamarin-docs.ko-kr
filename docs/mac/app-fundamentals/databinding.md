---
title: 데이터 바인딩 및 키-값 코딩
description: 이 문서에서는 키-값 코딩 및 관찰 Xcode의 인터페이스 작성기에서 UI 요소에 대 한 데이터 바인딩을 허용 하는 키-값을 사용 하 여 설명 합니다.
ms.prod: xamarin
ms.assetid: 72594395-0737-4894-8819-3E1802864BE7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 48ee5d4e4a0a53de49fbba46d79424e03af6fe5c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="data-binding-and-key-value-coding"></a>데이터 바인딩 및 키-값 코딩

_이 문서에서는 키-값 코딩 및 관찰 Xcode의 인터페이스 작성기에서 UI 요소에 대 한 데이터 바인딩을 허용 하는 키-값을 사용 하 여 설명 합니다._

## <a name="overview"></a>개요

같은 키-값 코딩 및 데이터 바인딩 기술에 대 한 액세스를 사용할 때 C# 및.NET Xamarin.Mac 응용 프로그램에 있는 하에서 작업을 수행할 *Objective-c* 및 *Xcode* 않습니다. Xamarin.Mac Xcode에 직접 통합을 사용 하면 Xcode의 _인터페이스 작성기_ 바인딩할 데이터 코드를 작성 하는 대신 UI 요소입니다.

키-값 코딩 및 바인딩 기술 Xamarin.Mac 응용 프로그램에서 데이터를 사용 하 여 작성 및 유지를 채우고 UI 요소를 사용 해야 하는 코드의 양을 크게 줄일 수 있습니다. 추가 백업 데이터를 분리 하는 이점이 있습니다 (_데이터 모델_) 사용자 인터페이스를 종료 하면 앞에서 (_모델-뷰-컨트롤러_)를 보다 융통성 있는 응용 프로그램 유지 관리 디자인 합니다.

[![실행 중인 응용 프로그램의 예로](databinding-images/intro01.png "실행 중인 응용 프로그램의 예")](databinding-images/intro01-large.png#lightbox)

이 문서에서 키-값 코딩 및 Xamarin.Mac 응용 프로그램에서 데이터 바인딩을 사용 하 여 작업의 기본 사항을 다룰 것입니다. 것이 가장 좋습니다를 통해 협력 하는 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서는 [Xcode 및 인터페이스 작성기 소개](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) 및 [콘센트 및 동작](~/mac/get-started/hello-mac.md#Outlets_and_Actions) 섹션으로이 문서에서 사용할 수 있는 주요 개념 및 기술을 설명 합니다.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명도 문서는 `Register` 및 `Export` 특성 요소 Objective-C 개체 및 UI에 C# 클래스를 연결 하는 데 사용 합니다.

<a name="What_is_Key-Value_Coding" />

## <a name="what-is-key-value-coding"></a>키-값 코딩 하는 것이 무엇입니까

개체의 속성을 직접 액세스, 인스턴스 변수를 통해 액세스 하는 대신 속성을 식별 하 키 (특별 한 형식의 문자열) 또는 접근자 메서드를 사용 하는 메커니즘은 키-값 (KVC) 코딩 (`get/set`). Xamarin.Mac 응용 프로그램에서 키-값 코딩 규격 접근자를 구현 하 여 키-값 관찰 (KVO), 데이터 바인딩, 핵심 데이터, Cocoa 바인딩 및 스크립팅 등의 기타 macOS (이전의 OS X) 기능에 액세스할을 수 있습니다.

키-값 코딩 및 바인딩 기술 Xamarin.Mac 응용 프로그램에서 데이터를 사용 하 여 작성 및 유지를 채우고 UI 요소를 사용 해야 하는 코드의 양을 크게 줄일 수 있습니다. 추가 백업 데이터를 분리 하는 이점이 있습니다 (_데이터 모델_) 사용자 인터페이스를 종료 하면 앞에서 (_모델-뷰-컨트롤러_)를 보다 융통성 있는 응용 프로그램 유지 관리 디자인 합니다. 

예를 들어 살펴보겠습니다 KVC 규격 개체의 다음 클래스 정의.

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

첫째, 고 `[Register("PersonModel")]` 특성 클래스를 등록 하 고 목표 없습니다.에 노출 클래스에서 상속 해야 그런 다음 `NSObject` (또는 해당 서브 클래스에서 상속 되는 `NSObject`), 여러 기본 클래스에 KVC 호환 되도록 허용 하는 메서드 추가 합니다. 다음으로 `[Export("Name")]` 노출 특성는 `Name` 속성 KVC 및 KVO 기술을 통해 속성에 액세스 하 여 나중에 사용할 키 값을 정의 합니다. 

마지막으로 관찰 된 키-값 속성의 값으로 변경 될 수 있으려면 접근자 래핑해야 해당 값의 변경 `WillChangeValue` 및 `DidChangeValue` 메서드 호출 (같은 키로 지정 하는 `Export` 특성).  예를 들어:

```csharp
set {
    WillChangeValue ("Name");
    _name = value;
    DidChangeValue ("Name");
}
```

이 단계는 _매우_ Xcode의 데이터 바인딩에 대 한 중요 한의 인터페이스 작성기 (이 문서의 뒷부분에 나오는으로).

자세한 내용은 Apple의를 참조 하십시오 [키-값 코딩 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)합니다.

### <a name="keys-and-key-paths"></a>키 및 키 경로

A _키_ 개체의 특정 속성을 식별 하는 문자열입니다. 일반적으로 키 호환 개체를 코딩 하는 키-값에는 접근자 메서드 이름에 해당 합니다. ASCII 인코딩을 사용 해야 소문자로, 일반적으로 시작 하 고 공백을 포함할 수 없습니다. 따라서 위 예에서 지정 된 `Name` 의 키 값이 `Name` 의 속성은 `PersonModel` 클래스입니다. 키 및 노출 하는 속성의 이름이 필요가 없습니다 동일 이지만 대부분의 경우.

A _키 경로_ 점 문자열로 구분를 트래버스해야 하는 개체 속성의 계층을 지정 하는 데 사용 되는 키입니다. 시퀀스의 첫 번째 키의 속성 수신기에 상대적 이며 각 후속 키 이전 속성의 값을 기준으로 평가 됩니다. 동일한 방식으로 C# 클래스에서 해당 속성 및는 개체를 순회 하도록 점 표기법을 사용 합니다.

예를 들어를 확장 한 경우는 `PersonModel` 클래스 및 추가 `Child` 속성:

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

자식의 이름에 키 경로가 `self.Child.Name` 또는 단순히 `Child.Name` (키 값이 사용 되는 방식에 따라).

### <a name="getting-values-using-key-value-coding"></a>키-값 코딩을 사용 하 여 값 가져오기

`ValueForKey` 지정된 된 키에 대 한 값을 반환 하는 메서드 (으로 `NSString`)을 기준으로 요청을 받는 KVC 클래스의 인스턴스. 예를 들어 경우 `Person` 의 인스턴스가 `PersonModel` 위에 정의 된 클래스:

```csharp
// Read value 
var name = Person.ValueForKey (new NSString("Name"));
```

값 반환이 `Name` 속성의 해당 인스턴스에 대 한 `PersonModel`합니다. 

### <a name="setting-values-using-key-value-coding"></a>키-값 코딩을 사용 하 여 값을 설정 합니다.

마찬가지로,는 `SetValueForKey` 지정된 된 키에 대 한 값을 설정 (으로 `NSString`)을 기준으로 요청을 받는 KVC 클래스의 인스턴스. 다시, 인스턴스를 사용 하 여 `PersonModel` 아래와 같이 클래스:

```csharp
// Write value
Person.SetValueForKey(new NSString("Jane Doe"), new NSString("Name"));
```

값은 변경 된 `Name` 속성을 `Jane Doe`합니다.

<a name="Observing_Value_Changes" />

### <a name="observing-value-changes"></a>관찰 값이 변경

KVC 규격 클래스의 특정 키에 관찰자를 연결할 수 있으며 (KVC 기술을 사용 하 여 또는 C# 코드에서 지정된 된 속성에 직접 액세스) 그 키에 대 한 값을 수정 하 든 지 알림을 받을 (KVO)을 관찰 하는 키-값을 사용 하 여, 있습니다. 예를 들어:

```csharp
// Watch for the name value changing
Person.AddObserver ("Name", NSKeyValueObservingOptions.New, (sender) => {
    // Inform caller of selection change
    Console.WriteLine("New Name: {0}", Person.Name)
});
```

이제 언제 든 지는 `Name` 의 속성은 `Person` 의 인스턴스는 `PersonModel` 클래스 수정 되 면 새 값은 콘솔에 기록 합니다. 

자세한 내용은 Apple의를 참조 하십시오 [키-값을 관찰 프로그래밍 가이드 소개](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i)합니다.

## <a name="data-binding"></a>데이터 바인딩

다음 섹션에서는 됩니다 데이터 읽기 및 쓰기 C# 코드를 사용 하 여 값 대신 Xcode의 인터페이스 작성기에서 UI 요소를 바인딩하는 키-값 코딩 및 관찰 규격 클래스는 키-값을 사용 하는 방법을 보여 줍니다. 이러한 방식으로 구분 하면 _데이터 모델_ 보다 유연 하 고 쉽게 유지 관리를 표시 하는 데 사용 되는 뷰를 통해 Xamarin.Mac 응용 프로그램을 만드는 합니다. 크게를 작성 해야 하는 코드의 양을 줄입니다.

<a name="Defining_your_Data_Model" />

### <a name="defining-your-data-model"></a>데이터 모델 정의

역할을 하도록 Xamarin.Mac 응용 프로그램에 정의 된 KVC/KVO 호환 클래스가 해야 하기 전에 데이터 바인딩 인터페이스 작성기에 있는 UI 요소를는 _데이터 모델_ 바인딩에 대 한 합니다. 데이터 모델의 모든 사용자 인터페이스에 표시 되 고 수신는 응용 프로그램을 실행 하는 동안 UI에 사용자가 데이터를 수정 하지 하는 데이터를 제공 합니다.

예를 들어 직원 그룹을 관리 하는 응용 프로그램을 작성 하는 경우 데이터 모델을 정의 하는 다음 클래스를 사용할 수 있습니다.

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

이 클래스의 기능은 대부분에서 검사 된는 [코딩 하는 키-값 이란](#What_is_Key-Value_Coding) 위의 섹션. 그러나 살펴보겠습니다 몇 가지 특정 요소와 데이터 모델에 대 한 역할을 하도록이 클래스를 허용 하도록 적용 된 몇 가지 추가 **배열 컨트롤러** 및 **트리 컨트롤러** (데이터를 나중에 사용할 예정 있음 바인딩 **트리 보기**, **개요 뷰** 및 **컬렉션 뷰**).

첫째, 직원은 관리자 일 수 있으므로 사용 했습니다는 `NSArray` (구체적으로 `NSMutableArray` 값을 수정할 수 있도록)에 연결할 자격 증명은 관리 하는 직원 수 있도록:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
```

여기에 다음 두 가지.

1. 사용 하는 `NSMutableArray` C# 배열 또는 컬렉션 이므로 AppKit 컨트롤에 데이터 바인딩하 요구와 같은 표준 아니라 **테이블 뷰**, **개요 뷰** 및 **컬렉션** .
2. 직원의 배열에 캐스팅 하 여 노출 했습니다는 `NSArray` 이름, 데이터 바인딩 용 및 변경의 C# 서식 지정 된 경우 `People`, 데이터 바인딩에서는 하나로 `personModelArray` 형태로 **{class_name} 배열** (참고 첫 번째 문자 만들었다고 소문자).

다음으로, 몇 가지 특수 이름 공용 지 원하는 메서드를 추가 해야 **배열 컨트롤러** 및 **트리 컨트롤러**:

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

컨트롤러를 요청 하 고 표시 되는 데이터를 수정할 수 있습니다. 마찬가지로 노출 된 `NSArray` (것과 다르면 일반적인 C# 명명 규칙) 매우 특별 한 명명 규칙은 위의:

- `addObject:` -배열에 있는 개체를 추가합니다.
- `insertObject:in{class_name}ArrayAtIndex:` -여기서 `{class_name}` 클래스의 이름입니다. 이 메서드는 개체 배열의 지정된 된 인덱스에 삽입합니다.
- `removeObjectFrom{class_name}ArrayAtIndex:` -여기서 `{class_name}` 클래스의 이름입니다. 이 메서드는 배열의 지정된 된 인덱스에 있는 개체를 제거합니다.
- `set{class_name}Array:` -여기서 `{class_name}` 클래스의 이름입니다. 이 방법을 사용 하면 기존 carry를 새 노드로 바꿀 수 있습니다.

이러한 메서드 내에서 배열에 대 한 변경 래핑된 했습니다 우리 `WillChangeValue` 및 `DidChangeValue` KVO 규정 준수에 대 한 메시지입니다.

마지막으로, 이후에 `Icon` 속성의 값에 의존는 `isManager` 속성을 변경는 `isManager` 속성에 반영 될 수는 `Icon` (KVO) 하는 동안 데이터 바인딩된 UI 요소에 대 한:

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

문제를 해결 하려면 다음 코드를 사용 합니다.

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

자체 키 외에도 유의 `isManager` 접근자 보내는 `WillChangeValue` 및 `DidChangeValue` 에 대 한 메시지는 `Icon` 도 변경 내용을 반영 하도록 키입니다.

사용할 예정 된 `PersonModel` 이 문서의 나머지 부분 전체에서 데이터 모델입니다.

<a name="Simple_Data_Binding" />

### <a name="simple-data-binding"></a>단순 데이터 바인딩

정의 데이터 모델을 살펴 보겠습니다 Xcode의 인터페이스 작성기에서 데이터 바인딩의 간단한 예입니다. 예를 들어 폼에 추가 편집 하는 데 사용할 수 있는 Xamarin.Mac 응용 프로그램의 `PersonModel` 위에 정의 된 것입니다. 몇 가지 텍스트 필드와 확인란을 표시 하 고 모델의 속성을 편집 추가 합니다.

첫째, 새 추가 **뷰-컨트롤러** 에 우리의 **Main.storyboard** 인터페이스 작성기에서 파일을 해당 클래스 이름을 `SimpleViewController`: 

[![새 보기 컨트롤러 추가](databinding-images/simple01.png "새 보기 컨트롤러 추가")](databinding-images/simple01-large.png#lightbox)

Mac 용 Visual Studio로 돌아가서 다음으로, 편집는 **SimpleViewController.cs** (즉이 프로젝트에 자동으로 추가 된) 파일의 인스턴스를 노출 하 고는 `PersonModel` म 되도록 데이터를 폼 바인딩. 다음 코드를 추가합니다.

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

다음 뷰를 로드할 때 보겠습니다의 인스턴스를 만들고 우리의 `PersonModel` 이 코드를 채웁니다.

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

폼을 만들려면 필요한 이제 두 번 클릭 하 여 **Main.storyboard** 인터페이스 작성기에서 편집을 위해 열 파일입니다. 레이아웃 폼을 다음과 같이 표시 됩니다.

[![Xcode에서 스토리 보드 편집](databinding-images/simple02.png "Xcode에서 스토리 보드를 편집 합니다.")](databinding-images/simple02-large.png#lightbox)

데이터에 양식을 바인딩합니다는 `PersonModel` 을 통해 표시 우리는 `Person` 에서 다음을 수행 하는 키를:

1. 선택 된 **직원 이름** 텍스트 필드 및 스위치를는 **바인딩 검사기**합니다.
2. 확인은 **바인딩할** 상자 고 선택 **간단한 뷰-컨트롤러** 드롭다운 목록에서 합니다. 그 다음 입력 `self.Person.Name` 에 대 한는 **키 경로**: 

    [![키 경로 입력](databinding-images/simple03.png "키 경로 입력 합니다.")](databinding-images/simple03-large.png#lightbox)
3. 선택의 **직업** 텍스트 필드 및 검사는 **바인딩할** 상자 고 선택 **간단한 뷰-컨트롤러** 드롭다운 목록에서 합니다. 그 다음 입력 `self.Person.Occupation` 에 대 한는 **키 경로**:  

    [![키 경로 입력](databinding-images/simple04.png "키 경로 입력 합니다.")](databinding-images/simple04-large.png#lightbox)
4. 선택의 **직원은 관리자** 확인란을 선택 하 고 확인는 **바인딩할** 상자 고 선택 **간단한 뷰-컨트롤러** 드롭다운 목록에서 합니다. 그 다음 입력 `self.Person.isManager` 에 대 한는 **키 경로**:  

    [![키 경로 입력](databinding-images/simple05.png "키 경로 입력 합니다.")](databinding-images/simple05-large.png#lightbox)
5. 선택는 **수의 직원 관리** 텍스트 필드 및 검사는 **바인딩할** 상자 고 선택 **간단한 뷰-컨트롤러** 드롭다운 목록에서 합니다. 그 다음 입력 `self.Person.NumberOfEmployees` 에 대 한는 **키 경로**:  

    [![키 경로 입력](databinding-images/simple06.png "키 경로 입력 합니다.")](databinding-images/simple06-large.png#lightbox)
6. 직원 관리자가 수의 직원 관리 되는 레이블 및 텍스트 필드를 숨길 하고자 합니다.
7. 선택는 **수의 직원 관리** 레이블 확장는 **Hidden** turndown 및 검사는 **바인딩할** 상자 고 선택 **간단한 뷰-컨트롤러** 드롭다운 목록에서 합니다. 그 다음 입력 `self.Person.isManager` 에 대 한는 **키 경로**:  

    [![키 경로 입력](databinding-images/simple07.png "키 경로 입력 합니다.")](databinding-images/simple07-large.png#lightbox)
8. 선택 `NSNegateBoolean` 에서 **값 변환기** 드롭다운:  

    ![NSNegateBoolean 키 변환을 선택 하면](databinding-images/simple08.png "NSNegateBoolean 키 변환을 선택")
9. 데이터 바인딩 알 경우 레이블이 표시 되지 것입니다의 값은 `isManager` 속성은 `false`합니다.
10. 7과 8에 대 한 단계를 반복 하는 **수의 직원 관리** 텍스트 필드입니다.
11. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

응용 프로그램에서 값을 실행 하는 경우는 `Person` 속성 폼을 자동으로 입력 됩니다.

[![자동으로 채워진 폼을 보여 주는](databinding-images/simple09.png "자동으로 채워진 폼을 표시 합니다.")](databinding-images/simple09-large.png#lightbox)

에 사용자가 폼에 수행 하는 변경 내용을 다시 기록 됩니다는 `Person` 뷰 컨트롤러에는 속성입니다. 예를 들어의 선택을 취소 **직원은 관리자** 업데이트는 `Person` 인스턴스의 우리의 `PersonModel` 및 **수의 직원 관리** 레이블 및 텍스트 필드 (을 통해 자동으로 숨겨진 데이터 바인딩):

[![비 관리자에 대 한 직원 수를 숨기 거](databinding-images/simple10.png "아닌 관리자에 대 한 직원 수 숨기기")](databinding-images/simple10-large.png#lightbox)

<a name="Table_View_Data_Binding" />

### <a name="table-view-data-binding"></a>표 보기 데이터 바인딩

이제 데이터를 바인딩하는 기본적인 했으므로 살펴보겠습니다 더 복잡 한 데이터 바인딩 작업을 사용 하 여 프로그램 _배열 컨트롤러_ 표 보기에 및 데이터 바인딩. 표 보기를 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [테이블 뷰](~/mac/user-interface/table-view.md) 설명서입니다.

첫째, 새 추가 **뷰-컨트롤러** 에 우리의 **Main.storyboard** 인터페이스 작성기에서 파일을 해당 클래스 이름을 `TableViewController`:

[![새 보기 컨트롤러 추가](databinding-images/table01.png "새 보기 컨트롤러 추가")](databinding-images/table01-large.png#lightbox)

다음으로 편집는 **TableViewController.cs** 파일 (즉이 프로젝트에 자동으로 추가 된) 및 배열 노출 (`NSArray`)의 `PersonModel` म 되도록 데이터를 폼 바인딩 클래스입니다. 다음 코드를 추가합니다.

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

에 했던 것 처럼는 `PersonModel` 위에 클래스에 [데이터 모델 정의](#Defining_your_Data_Model) 섹션 4 개의 명명 된 특수 공용 메서드를 노출 했습니다 म 있도록 업체 컬렉션을 배열 컨트롤러와 읽기 및 쓰기 데이터로 `PersonModels`합니다.

다음 보기를 로드할 때이 코드로 배열을 채우는 데 필요 합니다.

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

이제 우리 테이블 뷰를 만들어야 한다는 두 번 클릭은 **Main.storyboard** 인터페이스 작성기에서 편집을 위해 열 파일입니다. 레이아웃 테이블을 다음과 같이 표시 됩니다.

[![새 테이블 뷰를 레이아웃할](databinding-images/table02.png "를 레이아웃 하는 새 테이블 뷰")](databinding-images/table02-large.png#lightbox)

추가 해야 한다고 한 **배열 컨트롤러** 테이블에 바인딩된 데이터를 제공 하려면 다음을 수행 합니다.

1. 끌어서는 **컨트롤러 배열** 에서 **라이브러리 검사기** 에 **인터페이스 편집기**:  

    ![라이브러리에서 배열 컨트롤러를 선택 하면](databinding-images/table03.png "라이브러리에서 배열 컨트롤러 선택")
2. 선택 **배열 컨트롤러** 에 **인터페이스 계층 구조** 로 전환 하 고는 **특성 검사기**:  

    [![속성 검사자를 선택 하면](databinding-images/table04.png "특성 관리자를 선택 합니다.")](databinding-images/table04-large.png#lightbox)
3. 입력 `PersonModel` 에 대 한는 **클래스 이름**, 클릭는 **플러스** 단추를 세 개의 키를 추가 합니다. 이름을 지정 하 여 `Name`, `Occupation` 및 `isManager`:  

    ![필요한 키 경로 추가](databinding-images/table05.png "필요한 키 경로 추가 합니다.")
4. 이 속성을 확인할 배열 컨트롤러 작업 관리의 배열 속성 (키)를 통해 노출 해야 하 고 있습니다.
5. 전환 하는 **바인딩 검사기** 아래에서 **콘텐츠 배열** 선택 **바인딩할** 및 **테이블 뷰-컨트롤러**합니다. 입력 한 **키 경로 모델** 의 `self.personModelArray`:  

    ![키 경로 입력](databinding-images/table06.png "키 경로 입력 합니다.")
6. 이 연결의 배열에 배열 컨트롤러 `PersonModels` 보기 Controller에 노출 했습니다.

테이블 뷰 우리의 배열 컨트롤러에 바인딩할 필요, 이제는 다음을 수행 합니다.

1. 테이블 보기를 선택 및 **검사기 바인딩**:  

    [![바인딩 관리자를 선택 하면](databinding-images/table07.png "바인딩 관리자를 선택 합니다.")](databinding-images/table07-large.png#lightbox)
2. 아래는 **테이블 내용을** turndown, **바인딩할** 및 **배열 컨트롤러**합니다. 입력 `arrangedObjects` 에 대 한는 **컨트롤러 키** 필드:  

    ![컨트롤러 키 정의](databinding-images/table08.png "컨트롤러 키 정의")
3. 선택 된 **테이블 보기 셀** 아래는 **직원** 열. 에 **바인딩 검사기** 아래는 **값** turndown, **바인딩할** 및 **테이블 셀 뷰**합니다. 입력 `objectValue.Name` 에 대 한는 **키 경로 모델**:  

    [![모델의 키 경로 설정 중](databinding-images/table09.png "모델 키 경로 설정 합니다.")](databinding-images/table09-large.png#lightbox)
4. `objectValue` 현재 `PersonModel` 배열 컨트롤러에서 관리 되는 배열에 있습니다.
5. 선택은 **테이블 보기 셀** 아래는 **직업** 열입니다. 에 **바인딩 검사기** 아래는 **값** turndown, **바인딩할** 및 **테이블 셀 뷰**합니다. 입력 `objectValue.Occupation` 에 대 한는 **키 경로 모델**:  

    [![모델의 키 경로 설정 중](databinding-images/table10.png "모델 키 경로 설정 합니다.")](databinding-images/table10-large.png#lightbox)
6. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

응용 프로그램을 실행 하는 경우 테이블은 채워집니다 배열을 `PersonModels`:

[![응용 프로그램을 실행](databinding-images/table11.png "응용 프로그램 실행")](databinding-images/table11-large.png#lightbox)

<a name="Outline_View_Data_Binding" />

### <a name="outline-view-data-binding"></a>개요 보기 데이터 바인딩

데이터 바인딩 개요 보기에 대 한 테이블 보기에 대해 바인딩을 매우 비슷합니다. 중요 한 차이점은을 사용할 예정는 **트리 컨트롤러** 대신는 **배열 컨트롤러** 바인딩된 데이터 개요 보기를 제공 하도록 합니다. 개요 보기를 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [개요 뷰](~/mac/user-interface/outline-view.md) 설명서입니다.

첫째, 새 추가 **뷰-컨트롤러** 에 우리의 **Main.storyboard** 인터페이스 작성기에서 파일을 해당 클래스 이름을 `OutlineViewController`: 

[![새 보기 컨트롤러 추가](databinding-images/outline01.png "새 보기 컨트롤러 추가")](databinding-images/outline01-large.png#lightbox)

다음으로 편집는 **OutlineViewController.cs** 파일 (즉이 프로젝트에 자동으로 추가 된) 및 배열 노출 (`NSArray`)의 `PersonModel` म 되도록 데이터를 폼 바인딩 클래스입니다. 다음 코드를 추가합니다.

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

에 했던 것 처럼는 `PersonModel` 위에 클래스에 [데이터 모델 정의](#Defining_your_Data_Model) 섹션 4 개의 명명 된 특수 공용 메서드를 노출 했습니다 म 있도록 우리의 컬렉션에서 데이터 트리 컨트롤러와 읽기 및 쓰기 `PersonModels`합니다.

다음 보기를 로드할 때이 코드로 배열을 채우는 데 필요 합니다.

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

이제 우리 개요 보기를 만들어야 한다는 두 번 클릭은 **Main.storyboard** 인터페이스 작성기에서 편집을 위해 열 파일입니다. 레이아웃 테이블을 다음과 같이 표시 됩니다.

[![개요 보기를 만드는](databinding-images/outline02.png "개요 보기를 만드는 방법")](databinding-images/outline02-large.png#lightbox)

추가 해야는 **트리 컨트롤러** 바인딩된 데이터의 개요를 제공 하려면 다음을 수행 합니다.

1. 끌어서는 **컨트롤러 트리** 에서 **라이브러리 검사기** 에 **인터페이스 편집기**:  

    ![라이브러리에서 트리 컨트롤러를 선택 하면](databinding-images/outline03.png "라이브러리에서 트리 컨트롤러를 선택 하면")
2. 선택 **트리 컨트롤러** 에 **인터페이스 계층 구조** 로 전환 하 고는 **특성 검사기**:  

    [![특성 관리자를 선택 하면](databinding-images/outline04.png "특성 관리자를 선택 합니다.")](databinding-images/outline04-large.png#lightbox)
3. 입력 `PersonModel` 에 대 한는 **클래스 이름**, 클릭는 **플러스** 단추를 세 개의 키를 추가 합니다. 이름을 지정 하 여 `Name`, `Occupation` 및 `isManager`:  

    ![필요한 키 경로 추가](databinding-images/outline05.png "필요한 키 경로 추가 합니다.")
4. 내용 관리를 트리 컨트롤러 나타냅니다 (예: 키)를 통해 노출 해야 있으므로 속성 및 합니다.
5. 아래는 **트리 컨트롤러** 섹션을 입력 `personModelArray` 에 대 한 **자식**, 입력 `NumberOfEmployees` 아래는 **Count** 입력 `isEmployee` 아래 **리프**:  

    ![트리 컨트롤러 키 경로 설정](databinding-images/outline05.png "트리 컨트롤러 키 경로 설정 합니다.")
6. 트리 컨트롤러 노드, 자식 노드 수 있습니다 및 현재 노드에 자식 노드가 경우 모든 자식 찾을 위치를 나타냅니다.
7. 로 전환는 **바인딩 검사기** 고 **콘텐츠 배열** 선택 **바인딩할** 및 **파일의 소유자만**합니다. 입력 한 **키 경로 모델** 의 `self.personModelArray`:  

    ![키 경로 편집](databinding-images/outline06.png "키 경로 편집 합니다.")
8. 이 연결의 배열에는 트리 컨트롤러 `PersonModels` 보기 Controller에 노출 했습니다.

우리의 개요 보기 트리 컨트롤러에 바인딩할 필요, 이제는 다음을 수행 합니다.

1. 개요 보기를 선택 및는 **바인딩 검사기** 선택:  

    [![바인딩 관리자를 선택 하면](databinding-images/outline07.png "바인딩 관리자를 선택 합니다.")](databinding-images/outline07-large.png#lightbox)
2. 아래는 **내용 개요 보기** turndown, **바인딩할** 및 **트리 컨트롤러**합니다. 입력 `arrangedObjects` 에 대 한는 **컨트롤러 키** 필드:  

    ![키를 설정 하면 컨트롤러](databinding-images/outline08.png "컨트롤러 키 설정")
3. 선택 된 **테이블 보기 셀** 아래는 **직원** 열. 에 **바인딩 검사기** 아래는 **값** turndown, **바인딩할** 및 **테이블 셀 뷰**합니다. 입력 `objectValue.Name` 에 대 한는 **키 경로 모델**:  

    [![모델의 키 경로 입력](databinding-images/outline09.png "모델 키 경로 입력 합니다.")](databinding-images/outline09-large.png#lightbox)
4. `objectValue` 현재 `PersonModel` 트리 컨트롤러에서 관리 되는 배열에 있습니다.
5. 선택은 **테이블 보기 셀** 아래는 **직업** 열입니다. 에 **바인딩 검사기** 아래는 **값** turndown, **바인딩할** 및 **테이블 셀 뷰**합니다. 입력 `objectValue.Occupation` 에 대 한는 **키 경로 모델**:  

    [![모델의 키 경로 입력](databinding-images/outline10.png "모델 키 경로 입력 합니다.")](databinding-images/outline10-large.png#lightbox)
6. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

배열 개요는 채워지므로 응용 프로그램을 실행 하는 경우 `PersonModels`:

[![응용 프로그램을 실행](databinding-images/outline11.png "응용 프로그램 실행")](databinding-images/outline11-large.png#lightbox)

### <a name="collection-view-data-binding"></a>컬렉션 뷰 데이터 바인딩

컬렉션 뷰로 데이터 바인딩은 것 처럼 테이블 보기를 바인딩 배열 컨트롤러 컬렉션에 대 한 데이터를 제공 하는 데 사용 합니다. 컬렉션 뷰에서 미리 설정 된 표시 형식에 한 더 많은 작업은 사용자 상호 작용 피드백을 제공 하 고 사용자 선택 항목을 추적 합니다.

> [!IMPORTANT]
> Xcode 7 및 macOS 10.11 (이상)에서 한 문제일 컬렉션 뷰가 (.storyboard) 스토리 보드 파일 내에서 사용할 수 없습니다. 결과적으로, 계속 Xamarin.Mac 앱에 대 한 사용자 컬렉션 뷰를 정의.xib 파일을 사용 해야 합니다. 참조 하십시오 우리의 [컬렉션 뷰](~/mac/user-interface/collection-view.md) 자세한 정보에 대 한 설명서입니다.

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

## <a name="debugging-native-crashes"></a>기본 충돌 디버깅

실수 데이터 바인딩이 발생할 수 있습니다는 _네이티브 크래시_ 비관리 코드와 완전히 실패 하 고 Xamarin.Mac 응용 프로그램의 한 `SIGABRT` 오류:

[![네이티브 충돌 대화 상자의 예](databinding-images/debug01.png "네이티브 충돌 대화 상자의 예")](databinding-images/debug01-large.png#lightbox)

원인은 일반적으로 4 개의 주요 네이티브 충돌에 대해 데이터 바인딩 중:

1. 데이터 모델에서 상속 되지 않는 `NSObject` 또는 하위 클래스 `NSObject`합니다.
2. 속성을 사용 하 여 Objective C를 표시 하지 않습니다는 `[Export("key-name")]` 특성입니다.
3. 접근자의 값의 변경에 배치 되지 않은 `WillChangeValue` 및 `DidChangeValue` 메서드 호출 (같은 키로 지정 하는 `Export` 특성).
4. 잘못 된 또는 잘못 입력 된 키가 있는 **검사기 바인딩** 인터페이스 작성기에서입니다.

### <a name="decoding-a-crash"></a>충돌 디코딩

보겠습니다으로 인해 네이티브 충돌 데이터 바인딩에에서 찾고 해결 하는 방법을 알아보겠습니다 수 있도록 합니다. 인터페이스 작성기에서 바꿔보겠습니다 컬렉션 뷰 예제에서 첫 번째 레이블의 바인딩에 `Name` 를 `Title`:

[![바인딩 키를 편집](databinding-images/debug02.png "바인딩 키를 편집 합니다.")](databinding-images/debug02-large.png#lightbox)

변경 내용을 저장, xcode를 동기화 하 고 응용 프로그램을 실행 하는 Mac에 대 한 Visual Studio로 다시 전환 해 보겠습니다. 응용 프로그램으로 작동이 일시적으로 중단 됩니다 컬렉션 뷰에 표시 되 면는 `SIGABRT` 오류 (에 표시 된 대로 **응용 프로그램 출력** Mac 용 Visual Studio에서) 이후는 `PersonModel` 키와 속성을 노출 하지 않습니다 `Title`:

[![바인딩 오류의 예](databinding-images/debug03.png "바인딩 오류의 예")](databinding-images/debug03-large.png#lightbox)

오류 메시지의 최상위에 스크롤는 **응용 프로그램 출력** 문제를 해결 하기 위한 키 볼 수 있습니다.

[![오류 로그에서 문제를 찾는](databinding-images/debug04.png "오류 로그에서 문제 찾기")](databinding-images/debug04-large.png#lightbox)

이 줄은 이야기 하는 키 `Title` 개체를 바인딩하는 것에 대해 존재 하지 않습니다. 바인딩을 다시 변경 하면 `Name` 인터페이스 작성기에서 저장, 동기화를 다시 작성 및 실행, 응용 프로그램은 예상 대로 실행 문제 없이 합니다.

## <a name="summary"></a>요약

이 문서에 데이터 바인딩 및 키-값 코딩 Xamarin.Mac 응용 프로그램에서 작업에 대해 자세히를 수행 했습니다. 첫째, 것 (KVO)을 관찰 하는 키-값 및 키-값 코딩 (KVC)를 사용 하 여 Objective C는 C# 클래스를 노출 살펴보았습니다. 다음으로, KVO 규격 클래스를 사용 하는 방법에 알아보았습니다를 데이터 Xcode의 인터페이스 작성기에서 UI 요소에 바인딩합니다. 마지막으로 사용 하 여 복잡 한 데이터 바인딩 표시 될 **배열 컨트롤러** 및 **트리 컨트롤러**합니다.


## <a name="related-links"></a>관련 링크

- [MacDatabinding 스토리 보드 (샘플)](https://developer.xamarin.com/samples/mac/MacDatabinding-Storyboard/)
- [MacDatabinding XIBs (샘플)](https://developer.xamarin.com/samples/mac/MacDatabinding-XIBs/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [표준 컨트롤](~/mac/user-interface/standard-controls.md)
- [테이블 뷰](~/mac/user-interface/table-view.md)
- [개요 보기](~/mac/user-interface/outline-view.md)
- [컬렉션 뷰](~/mac/user-interface/collection-view.md)
- [키-값 코딩 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [키-값을 관찰 프로그래밍 가이드 소개](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html)
- [Cocoa 바인딩 프로그래밍 항목 소개](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Cocoa 바인딩 참조 소개](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [Human Interface Guidelines macOS](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
