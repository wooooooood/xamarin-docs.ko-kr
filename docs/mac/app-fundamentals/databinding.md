---
title: Xamarin.ios의 데이터 바인딩 및 키-값 코딩
description: 이 문서에서는 Xcode의 Interface Builder에서 UI 요소에 대 한 데이터 바인딩을 허용 하기 위해 키-값 코딩 및 키-값 관찰을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 72594395-0737-4894-8819-3E1802864BE7
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 81a1f63078a5f7a2a70f731d1790f85f4283d22f
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78918943"
---
# <a name="data-binding-and-key-value-coding-in-xamarinmac"></a>Xamarin.ios의 데이터 바인딩 및 키-값 코딩

_이 문서에서는 Xcode의 Interface Builder에서 UI 요소에 대 한 데이터 바인딩을 허용 하기 위해 키-값 코딩 및 키-값 관찰을 사용 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Xamarin.ios 응용 프로그램 C# 에서 및 .net을 사용 하는 경우 *목표-C* 및 *Xcode* 에서 작업 하는 개발자가 동일한 키-값 코딩 및 데이터 바인딩 기술에 액세스할 수 있습니다. Xamarin.ios는 Xcode와 직접 통합 되므로 Xcode의 _Interface Builder_ 를 사용 하 여 코드를 작성 하는 대신 UI 요소를 사용 하 여 데이터 바인딩할 수 있습니다.

Xamarin.ios 응용 프로그램에서 키-값 코딩 및 데이터 바인딩 기술을 사용 하 여 UI 요소를 채우고 사용 하기 위해 작성 하 고 유지 관리 해야 하는 코드의 양을 크게 줄일 수 있습니다. 또한 프런트 엔드 사용자 인터페이스 (_모델-뷰-컨트롤러_)에서 지원 데이터 (_데이터 모델_)를 추가로 분리 하 여 더 쉽게 유지 관리 하 고 더욱 유연한 응용 프로그램을 디자인할 수 있는 이점을 누릴 수 있습니다.

[![실행 중인 앱의 예](databinding-images/intro01.png "실행 중인 앱의 예")](databinding-images/intro01-large.png#lightbox)

이 문서에서는 Xamarin.ios 응용 프로그램에서 키-값 코딩 및 데이터 바인딩 작업의 기본 사항을 다룹니다. 이 문서에서 사용할 주요 개념 및 기술에 대해 설명 하는 대로 [Hello, Mac](~/mac/get-started/hello-mac.md) 문서를 먼저 소개 하 고 특히 [Xcode 및 Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 및 [콘센트 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션을 소개 하는 것이 좋습니다.

[Xamarin.ios 내부](~/mac/internals/how-it-works.md) 문서의 [목적에 따라 클래스/메서드 노출 C# ](~/mac/internals/how-it-works.md) 섹션을 살펴볼 수 있습니다. C# 클래스를 목표-c 개체 및 UI 요소에 연결 하는 데 사용 되는 `Register` 및 `Export` 특성에 대해서도 설명 합니다.

<a name="What_is_Key-Value_Coding" />

## <a name="what-is-key-value-coding"></a>키-값 코딩 이란?

KVC (키-값 코딩)는 키 (특별히 서식이 지정 된 문자열)를 사용 하 여 개체의 속성에 간접적으로 액세스 하 고 인스턴스 변수 또는 접근자 메서드 (`get/set`)를 통해 액세스 하는 대신 속성을 식별 하는 메커니즘입니다. Xamarin.ios 응용 프로그램에서 키-값 코딩 규격 접근자를 구현 하 여 키-값 관찰 (KVO), 데이터 바인딩, 코어 데이터, Cocoa 바인딩 및 scriptability 같은 다른 macOS (이전의 OS X)에 액세스할 수 있습니다.

Xamarin.ios 응용 프로그램에서 키-값 코딩 및 데이터 바인딩 기술을 사용 하 여 UI 요소를 채우고 사용 하기 위해 작성 하 고 유지 관리 해야 하는 코드의 양을 크게 줄일 수 있습니다. 또한 프런트 엔드 사용자 인터페이스 (_모델-뷰-컨트롤러_)에서 지원 데이터 (_데이터 모델_)를 추가로 분리 하 여 더 쉽게 유지 관리 하 고 더욱 유연한 응용 프로그램을 디자인할 수 있는 이점을 누릴 수 있습니다.

예를 들어 KVC 규격 개체의 다음 클래스 정의를 살펴보겠습니다.

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

먼저 `[Register("PersonModel")]` 특성은 클래스를 등록 하 고 목표-C에 노출 합니다. 그런 다음 클래스는 `NSObject` 또는 `NSObject`에서 상속 되는 서브 클래스에서 상속 해야 합니다. 그러면 클래스가 KVC 규격이 될 수 있도록 하는 몇 가지 기본 메서드가 추가 됩니다. 그런 다음 `[Export("Name")]` 특성은 `Name` 속성을 노출 하 고 나중에 KVC 및 KVO 기술을 통해 속성에 액세스 하는 데 사용 되는 키 값을 정의 합니다.

마지막으로, 속성의 값에 대 한 키 값이 변경 될 수 있도록 접근자는 `WillChangeValue` 및 `DidChangeValue` 메서드 호출 (`Export` 특성과 동일한 키 지정)에서 해당 값에 대 한 변경 내용을 래핑해야 합니다.  다음은 그 예입니다.

```csharp
set {
    WillChangeValue ("Name");
    _name = value;
    DidChangeValue ("Name");
}
```

이 단계는 Xcode의 Interface Builder에서 데이터 바인딩에 _매우_ 중요 합니다 (이 문서의 뒷부분에서 볼 수 있는 것 처럼).

자세한 내용은 Apple의 [키-값 코딩 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)를 참조 하세요.

### <a name="keys-and-key-paths"></a>키 및 키 경로

_키는_ 개체의 특정 속성을 식별 하는 문자열입니다. 일반적으로 키는 키-값 코딩 규격 개체의 접근자 메서드 이름에 해당 합니다. 키는 ASCII 인코딩을 사용 해야 하며, 일반적으로 소문자로 시작 하 고 공백을 포함할 수 없습니다. 따라서 위의 예제에서 `Name`는 `PersonModel` 클래스의 `Name` 속성의 키 값입니다. 노출 되는 속성의 키와 이름은 같을 필요가 없지만 대부분의 경우에는 이러한 키와 키가 동일 합니다.

_키 경로_ 는 이동할 개체 속성의 계층을 지정 하는 데 사용 되는 점으로 구분 된 키의 문자열입니다. 시퀀스에서 첫 번째 키의 속성은 수신자를 기준으로 하며, 각 후속 키는 이전 속성의 값을 기준으로 평가 됩니다. 동일한 방식으로 C# 클래스에서 점 표기법을 사용 하 여 개체와 해당 속성을 트래버스 합니다.

예를 들어 `PersonModel` 클래스를 확장 하 고 `Child` 속성을 추가한 경우 다음을 수행 합니다.

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

자식 이름의 키 경로는 `self.Child.Name` 되거나 키 값이 사용 되는 방식에 따라 `Child.Name` 됩니다.

### <a name="getting-values-using-key-value-coding"></a>키-값 코딩을 사용 하 여 값 가져오기

`ValueForKey` 메서드는 요청을 받는 KVC 클래스의 인스턴스를 기준으로 지정 된 키 (`NSString`)의 값을 반환 합니다. 예를 들어 `Person`은 위에 정의 된 `PersonModel` 클래스의 인스턴스입니다.

```csharp
// Read value
var name = Person.ValueForKey (new NSString("Name"));
```

그러면 `PersonModel`의 해당 인스턴스에 대 한 `Name` 속성 값이 반환 됩니다.

### <a name="setting-values-using-key-value-coding"></a>키-값 코딩을 사용 하 여 값 설정

마찬가지로 `SetValueForKey`은 요청을 받는 KVC 클래스의 인스턴스를 기준으로 지정 된 키에 대 한 값을 `NSString`로 설정 합니다. 따라서 아래와 같이 `PersonModel` 클래스의 인스턴스를 사용 합니다.

```csharp
// Write value
Person.SetValueForKey(new NSString("Jane Doe"), new NSString("Name"));
```

`Name` 속성 값을 `Jane Doe`로 변경 합니다.

<a name="Observing_Value_Changes" />

### <a name="observing-value-changes"></a>값 변경 관찰

키-값 관찰 (KVO)을 사용 하 여, 관찰자를 KVC 규격 클래스의 특정 키에 연결 하 고, 해당 키에 대 한 값이 수정 될 때마다 (KVC 기술을 사용 하거나 코드에서 C# 지정 된 속성에 직접 액세스 하는 경우) 알리도록 할 수 있습니다. 다음은 그 예입니다.

```csharp
// Watch for the name value changing
Person.AddObserver ("Name", NSKeyValueObservingOptions.New, (sender) => {
    // Inform caller of selection change
    Console.WriteLine("New Name: {0}", Person.Name)
});
```

이제 `PersonModel` 클래스의 `Person` 인스턴스의 `Name` 속성이 수정 될 때마다 새 값이 콘솔에 기록 됩니다.

자세한 내용은 Apple의 [키-값 관찰 프로그래밍 가이드를](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i)참조 하세요.

## <a name="data-binding"></a>데이터 바인딩

다음 섹션에서는 코드를 사용 하 여 C# 값을 읽고 쓰는 대신 키-값 코딩 및 키-값을 관찰 하는 규격 클래스를 사용 하 여 Xcode의 Interface Builder에 있는 UI 요소에 데이터를 바인딩하는 방법을 보여 줍니다. 이러한 방식으로 데이터 모델을 표시 하는 데 사용 되는 보기에서 _데이터 모델_ 을 분리 하 여 xamarin.ios 응용 프로그램을 보다 유연 하 고 쉽게 유지 관리할 수 있도록 합니다. 또한 작성 해야 하는 코드의 양을 크게 줄일 수 있습니다.

<a name="Defining_your_Data_Model" />

### <a name="defining-your-data-model"></a>데이터 모델 정의

Interface Builder에서 UI 요소를 데이터 바인딩하려면 먼저 Xamarin.ios 응용 프로그램에 정의 된 KVC/KVO 규격 클래스가 있어야 바인딩에 대 한 _데이터 모델_ 역할을 할 수 있습니다. 데이터 모델은 사용자 인터페이스에 표시 되는 모든 데이터를 제공 하 고, 응용 프로그램을 실행 하는 동안 사용자가 UI에서 수행 하는 데이터에 대 한 모든 수정 사항을 받습니다.

예를 들어 직원 그룹을 관리 하는 응용 프로그램을 작성 하는 경우 다음 클래스를 사용 하 여 데이터 모델을 정의할 수 있습니다.

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

이 클래스의 기능 중 대부분은 위의 [키-값 코딩 이란?](#What_is_Key-Value_Coding) 섹션에서 설명 했습니다. 그러나이 클래스에서 **배열 컨트롤러** 및 **트리 컨트롤러** 에 대 한 데이터 모델 역할을 할 수 있도록 하는 몇 가지 특정 요소와 몇 가지 추가 사항을 살펴보겠습니다 (나중에 데이터 바인딩 **트리 보기**, **개요 뷰** 및 **컬렉션 보기**를 사용 하 게 됨).

첫째, 직원이 관리자 일 수 있기 때문에 관리 되는 직원 들이 연결 될 수 있도록 `NSArray` (특히 `NSMutableArray` 값을 수정할 수 있음)을 사용 했습니다.

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
```

다음 두 가지 사항을 참고 하세요.

1. 이는 **테이블 뷰**, **개요 뷰** 및 C# **컬렉션과**같은 appkit 컨트롤에 데이터를 바인딩하기 위한 요구 사항 이므로 표준 배열 또는 컬렉션 대신 `NSMutableArray`를 사용 했습니다.
2. 데이터 바인딩을 위해 `NSArray`로 캐스팅 하 고 C# 형식이 지정 된 이름 `People`를 데이터 바인딩에서 **class_name** `personModelArray` 필요로 하는 이름으로 변경 하 여 직원의 배열을 표시 합니다. (첫 번째 문자는 소문자가 됨)

다음으로 일부 특수 이름 public 메서드를 추가 하 여 **배열 컨트롤러** 및 **트리 컨트롤러**를 지원 해야 합니다.

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

그러면 컨트롤러에서 표시 되는 데이터를 요청 하 고 수정할 수 있습니다. 위의 `NSArray` 표시 되는 것과 마찬가지로, 이러한 명명 규칙은 일반적인 C# 명명 규칙에 따라 다릅니다.

- `addObject:`-배열에 개체를 추가 합니다.
- `insertObject:in{class_name}ArrayAtIndex:`-`{class_name}`은 클래스의 이름입니다. 이 메서드는 지정 된 인덱스에서 개체를 배열에 삽입 합니다.
- `removeObjectFrom{class_name}ArrayAtIndex:`-`{class_name}`은 클래스의 이름입니다. 이 메서드는 지정 된 인덱스의 배열에서 개체를 제거 합니다.
- `set{class_name}Array:`-`{class_name}`은 클래스의 이름입니다. 이 메서드를 사용 하면 기존 전달을 새 항목으로 바꿀 수 있습니다.

이러한 메서드 내에서 KVO 준수를 위해 `WillChangeValue` 및 `DidChangeValue` 메시지에서 배열의 변경 내용을 래핑 했습니다.

마지막으로 `Icon` 속성이 `isManager` 속성의 값을 사용 하기 때문에 `isManager` 속성의 변경 내용이 데이터 바인딩된 UI 요소에 대 한 `Icon` (KVO 중)에 반영 되지 않을 수 있습니다.

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

이를 해결 하기 위해 다음 코드를 사용 합니다.

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

자체 키 외에도 `isManager` 접근자는 `Icon` 키에 대 한 `WillChangeValue` 및 `DidChangeValue` 메시지를 전송 하 여 변경 내용이 표시 되도록 합니다.

이 문서의 나머지 부분에서 `PersonModel` 데이터 모델을 사용 합니다.

<a name="Simple_Data_Binding" />

### <a name="simple-data-binding"></a>단순 데이터 바인딩

데이터 모델을 정의 하면 Xcode의 Interface Builder에 있는 데이터 바인딩의 간단한 예를 살펴보겠습니다. 예를 들어 위에서 정의한 `PersonModel`을 편집 하는 데 사용할 수 있는 양식을 Xamarin.ios 응용 프로그램에 추가 하겠습니다. 몇 가지 텍스트 필드와 확인란을 추가 하 여 모델의 속성을 표시 하 고 편집할 수 있습니다.

먼저 Interface Builder의 **기본 storyboard** 파일에 새 **뷰 컨트롤러** 를 추가 하 고 해당 클래스의 이름을 `SimpleViewController`으로 지정 하겠습니다.

[![새 뷰 컨트롤러 추가](databinding-images/simple01.png "새 뷰 컨트롤러 추가")](databinding-images/simple01-large.png#lightbox)

그런 다음 Mac용 Visual Studio로 돌아가서 프로젝트에 자동으로 추가 된 **SimpleViewController.cs** 파일을 편집 하 고 폼에 데이터를 바인딩할 `PersonModel`의 인스턴스를 노출 합니다. 다음 코드를 추가합니다.

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

그런 다음 뷰가 로드 될 때 `PersonModel`의 인스턴스를 만들고이 코드로 채웁니다.

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

이제 양식을 만들고, **주. 스토리 보드** 파일을 두 번 클릭 하 여 Interface Builder에서 편집할 수 있도록 엽니다. 폼을 다음과 같이 레이아웃 합니다.

[![Xcode에서 storyboard 편집](databinding-images/simple02.png "Xcode에서 storyboard 편집")](databinding-images/simple02-large.png#lightbox)

폼을 `Person` 키를 통해 노출 된 `PersonModel`에 데이터 바인딩하려면 다음을 수행 합니다.

1. **직원 이름** 텍스트 필드를 선택 하 고 **바인딩 검사자**로 전환 합니다.
2. **바인딩 대상** 상자를 선택 하 고 드롭다운에서 **단순 뷰 컨트롤러** 를 선택 합니다. 다음 **키 경로**에 대 한 `self.Person.Name`를 입력 합니다.

    [![키 경로 입력](databinding-images/simple03.png "키 경로 입력")](databinding-images/simple03-large.png#lightbox)
3. **직업** 텍스트 필드를 선택 하 고 **바인딩 대상** 상자를 선택 하 고 드롭다운에서 **단순 보기 컨트롤러** 를 선택 합니다. 다음 **키 경로**에 대 한 `self.Person.Occupation`를 입력 합니다.

    [![키 경로 입력](databinding-images/simple04.png "키 경로 입력")](databinding-images/simple04-large.png#lightbox)
4. **Employee is a Manager** 확인란을 선택 하 고 **바인딩 대상** 상자를 선택 하 고 드롭다운에서 **단순 보기 컨트롤러** 를 선택 합니다. 다음 **키 경로**에 대 한 `self.Person.isManager`를 입력 합니다.

    [![키 경로 입력](databinding-images/simple05.png "키 경로 입력")](databinding-images/simple05-large.png#lightbox)
5. **직원의 관리 되** 는 텍스트 필드를 선택 하 고 **바인딩 대상** 상자를 선택 하 고 드롭다운에서 **단순 보기 컨트롤러** 를 선택 합니다. 다음 **키 경로**에 대 한 `self.Person.NumberOfEmployees`를 입력 합니다.

    [![키 경로 입력](databinding-images/simple06.png "키 경로 입력")](databinding-images/simple06-large.png#lightbox)
6. 직원이 관리자가 아니면 직원의 관리 되는 레이블과 텍스트 필드의 수를 숨기려는 것입니다.
7. **직원의 관리 되** 는 레이블 수를 선택 하 **고 Hidden** 접혀를 확장 한 다음 **바인딩 대상** 상자를 선택 하 고 드롭다운에서 **단순 보기 컨트롤러** 를 선택 합니다. 다음 **키 경로**에 대 한 `self.Person.isManager`를 입력 합니다.

    [![키 경로 입력](databinding-images/simple07.png "키 경로 입력")](databinding-images/simple07-large.png#lightbox)
8. **값 변환기** 드롭다운에서 `NSNegateBoolean`를 선택 합니다.

    ![NSNegateBoolean 키 변환 선택](databinding-images/simple08.png "NSNegateBoolean 키 변환 선택")
9. 이렇게 하면 `isManager` 속성 값이 `false`되는 경우 레이블이 숨겨지도록 데이터 바인딩에 지시 하 게 됩니다.
10. **직원 관리** 텍스트 필드에 대해 7 단계와 8 단계를 반복 합니다.
11. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

응용 프로그램을 실행 하는 경우 `Person` 속성의 값이 다음 형식으로 자동으로 채워집니다.

[![자동으로 채워진 폼 표시](databinding-images/simple09.png "자동으로 채워진 폼 표시")](databinding-images/simple09-large.png#lightbox)

사용자가 폼에 적용 하는 모든 변경 내용은 뷰 컨트롤러의 `Person` 속성에 다시 기록 됩니다. 예를 들어 직원 선택을 취소 하면 **관리자가** `PersonModel`의 `Person` 인스턴스를 업데이트 하 고 **직원의 관리** 되는 레이블 수와 텍스트 필드가 자동으로 숨겨집니다 (데이터 바인딩을 통해).

[![비관리자의 직원 수 숨기기](databinding-images/simple10.png "비관리자의 직원 수 숨기기")](databinding-images/simple10-large.png#lightbox)

<a name="Table_View_Data_Binding" />

### <a name="table-view-data-binding"></a>테이블 뷰 데이터 바인딩

이제 데이터 바인딩의 기본 사항을 제공 했으므로 _배열 컨트롤러_ 를 사용 하 고 테이블 뷰에 데이터 바인딩을 사용 하 여 좀 더 복잡 한 데이터 바인딩 태스크를 살펴보겠습니다. 테이블 뷰 작업에 대 한 자세한 내용은 [테이블 뷰](~/mac/user-interface/table-view.md) 설명서를 참조 하세요.

먼저 Interface Builder의 **기본 storyboard** 파일에 새 **뷰 컨트롤러** 를 추가 하 고 해당 클래스의 이름을 `TableViewController`으로 지정 하겠습니다.

[![새 뷰 컨트롤러 추가](databinding-images/table01.png "새 뷰 컨트롤러 추가")](databinding-images/table01-large.png#lightbox)

다음으로 **TableViewController.cs** 파일 (프로젝트에 자동으로 추가 됨)을 편집 하 고 폼에 데이터를 바인딩할 `PersonModel` 클래스의 배열 (`NSArray`)을 노출 합니다. 다음 코드를 추가합니다.

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

위의 `PersonModel` 클래스를 [데이터 모델 정의](#Defining_your_Data_Model) 섹션에서 살펴본 것 처럼 배열 컨트롤러에서 `PersonModels`컬렉션의 데이터를 읽고 쓰기 위해 특별히 명명 된 네 public 메서드를 노출 했습니다.

그런 다음 뷰가 로드 될 때 배열을 다음 코드로 채워야 합니다.

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

이제 테이블 뷰를 만들고, **주. 스토리 보드** 파일을 두 번 클릭 하 여 Interface Builder에서 편집할 수 있도록 엽니다. 테이블을 다음과 같이 레이아웃 합니다.

[![새 테이블 뷰 레이아웃](databinding-images/table02.png "새 테이블 뷰 레이아웃")](databinding-images/table02-large.png#lightbox)

테이블에 바인딩된 데이터를 제공 하기 위해 **배열 컨트롤러** 를 추가 하 고 다음을 수행 해야 합니다.

1. **라이브러리 검사기** 의 **배열 컨트롤러** 를 **인터페이스 편집기**로 끌어 옵니다.

    ![라이브러리에서 배열 컨트롤러 선택](databinding-images/table03.png "라이브러리에서 배열 컨트롤러 선택")
2. **인터페이스 계층 구조** 에서 **배열 컨트롤러** 를 선택 하 고 **특성 검사자**로 전환 합니다.

    [![특성 검사자 선택](databinding-images/table04.png "특성 검사자 선택")](databinding-images/table04-large.png#lightbox)
3. **클래스 이름**에 대 한 `PersonModel`를 입력 하 고 **더하기** 단추를 클릭 한 다음 세 개의 키를 추가 합니다. `Name`, `Occupation` 및 `isManager`에 이름을로 합니다.

    ![필수 키 경로 추가](databinding-images/table05.png "필수 키 경로 추가")
4. 이렇게 하면 배열 컨트롤러에서 배열을 관리 하는 대상과 키를 통해 노출 해야 하는 속성을 알 수 있습니다.
5. **바인딩 검사자** 로 전환 하 고 **콘텐츠 배열** 에서 **바인딩을** 선택 하 고 **테이블 뷰 컨트롤러**를 선택 합니다. `self.personModelArray`의 **모델 키 경로** 를 입력 합니다.

    ![키 경로 입력](databinding-images/table06.png "키 경로 입력")
6. 이렇게 하면 배열 컨트롤러가 보기 컨트롤러에 노출 된 `PersonModels` 배열에 연결 됩니다.

이제 테이블 뷰를 배열 컨트롤러에 바인딩해야 하며, 다음을 수행 해야 합니다.

1. 테이블 뷰 및 **바인딩 검사기**를 선택 합니다.

    [![바인딩 검사자 선택](databinding-images/table07.png "바인딩 검사자 선택")](databinding-images/table07-large.png#lightbox)
2. **표 내용** 접혀에서 바인딩 및 **배열 컨트롤러** **를** 선택 합니다. **컨트롤러 키** 필드에 `arrangedObjects`을 입력 합니다.

    ![컨트롤러 키 정의](databinding-images/table08.png "컨트롤러 키 정의")
3. **Employee** 열 아래에서 **테이블 뷰 셀** 을 선택 합니다. **바인딩 검사자** 의 **값** 접혀에서 **바인딩 및** **테이블 셀 뷰**를 선택 합니다. **모델 키 경로**에 대 한 `objectValue.Name`를 입력 합니다.

    [![모델 키 경로 설정](databinding-images/table09.png "모델 키 경로 설정")](databinding-images/table09-large.png#lightbox)
4. `objectValue`은 배열 컨트롤러에서 관리 하는 배열의 현재 `PersonModel`입니다.
5. **직업** 열 아래에서 **테이블 뷰 셀** 을 선택 합니다. **바인딩 검사자** 의 **값** 접혀에서 **바인딩 및** **테이블 셀 뷰**를 선택 합니다. **모델 키 경로**에 대 한 `objectValue.Occupation`를 입력 합니다.

    [![모델 키 경로 설정](databinding-images/table10.png "모델 키 경로 설정")](databinding-images/table10-large.png#lightbox)
6. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

응용 프로그램을 실행 하는 경우 테이블은 `PersonModels`배열로 채워집니다.

[![응용 프로그램 실행](databinding-images/table11.png "애플리케이션 실행")](databinding-images/table11-large.png#lightbox)

<a name="Outline_View_Data_Binding" />

### <a name="outline-view-data-binding"></a>개요 보기 데이터 바인딩

개요 뷰에 대 한 데이터 바인딩은 테이블 뷰에 대 한 바인딩과 매우 유사 합니다. 주요 차이점은 **배열 컨트롤러** 대신 **트리 컨트롤러** 를 사용 하 여 바인딩된 데이터를 개요 뷰에 제공 한다는 것입니다. 개요 보기를 사용 하는 방법에 대 한 자세한 내용은 [개요 보기](~/mac/user-interface/outline-view.md) 설명서를 참조 하세요.

먼저 Interface Builder의 **기본 storyboard** 파일에 새 **뷰 컨트롤러** 를 추가 하 고 해당 클래스의 이름을 `OutlineViewController`으로 지정 하겠습니다.

[![새 뷰 컨트롤러 추가](databinding-images/outline01.png "새 뷰 컨트롤러 추가")](databinding-images/outline01-large.png#lightbox)

다음으로 **OutlineViewController.cs** 파일 (프로젝트에 자동으로 추가 됨)을 편집 하 고 폼에 데이터를 바인딩할 `PersonModel` 클래스의 배열 (`NSArray`)을 노출 합니다. 다음 코드를 추가합니다.

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

위의 `PersonModel` 클래스를 [데이터 모델 정의](#Defining_your_Data_Model) 섹션에서 살펴본 것 처럼 트리 컨트롤러에서 `PersonModels`컬렉션의 데이터를 읽고 쓰기 위해 특별히 명명 된 네 public 메서드를 노출 했습니다.

그런 다음 뷰가 로드 될 때 배열을 다음 코드로 채워야 합니다.

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

이제 개요 보기를 만들고, **주. 스토리 보드** 파일을 두 번 클릭 하 여 Interface Builder에서 편집할 수 있도록 엽니다. 테이블을 다음과 같이 레이아웃 합니다.

[![개요 보기 만들기](databinding-images/outline02.png "개요 보기 만들기")](databinding-images/outline02-large.png#lightbox)

개요에 바인딩된 데이터를 제공 하는 **트리 컨트롤러** 를 추가 하 고 다음을 수행 해야 합니다.

1. **라이브러리 검사기** 에서 **트리 컨트롤러** 를 **인터페이스 편집기**로 끌어 옵니다.

    ![라이브러리에서 트리 컨트롤러 선택](databinding-images/outline03.png "라이브러리에서 트리 컨트롤러 선택")
2. **인터페이스 계층 구조** 에서 **트리 컨트롤러** 를 선택 하 고 **특성 검사자**로 전환 합니다.

    [![특성 검사자 선택](databinding-images/outline04.png "특성 검사자 선택")](databinding-images/outline04-large.png#lightbox)
3. **클래스 이름**에 대 한 `PersonModel`를 입력 하 고 **더하기** 단추를 클릭 한 다음 세 개의 키를 추가 합니다. `Name`, `Occupation` 및 `isManager`에 이름을로 합니다.

    ![필수 키 경로 추가](databinding-images/outline05.png "필수 키 경로 추가")
4. 이를 통해 트리 컨트롤러에서 배열을 관리 하는 대상 및 키를 통해 노출 해야 하는 속성을 알 수 있습니다.
5. **트리 컨트롤러** 섹션에서 **자식**에 대 한 `personModelArray`를 입력 하 고, **개수** 아래에 `NumberOfEmployees`를 입력 하 고 **리프**아래에 `isEmployee`을 입력 합니다.

    ![트리 컨트롤러 키 경로 설정](databinding-images/outline05.png "트리 컨트롤러 키 경로 설정")
6. 그러면 트리 컨트롤러에 자식 노드를 찾을 수 있는 위치, 자식 노드 수와 현재 노드에 자식 노드가 있는지 여부를 알 수 있습니다.
7. **바인딩 검사자** 로 전환 하 고 **콘텐츠 배열** 에서 **바인딩을** 선택 하 고 **파일의 소유자**를 선택 합니다. `self.personModelArray`의 **모델 키 경로** 를 입력 합니다.

    ![키 경로 편집](databinding-images/outline06.png "키 경로 편집")
8. 이는 트리 컨트롤러를 뷰 컨트롤러에 노출 된 `PersonModels`의 배열에 연결 합니다.

이제 개요 뷰를 트리 컨트롤러에 바인딩해야 하며, 다음을 수행 해야 합니다.

1. 개요 보기를 선택 하 고 **바인딩 검사기** 에서 다음을 선택 합니다.

    [![바인딩 검사자 선택](databinding-images/outline07.png "바인딩 검사자 선택")](databinding-images/outline07-large.png#lightbox)
2. **개요 보기 내용** 접혀에서 바인딩 및 **트리 컨트롤러** **를** 선택 합니다. **컨트롤러 키** 필드에 `arrangedObjects`을 입력 합니다.

    ![컨트롤러 키 설정](databinding-images/outline08.png "컨트롤러 키 설정")
3. **Employee** 열 아래에서 **테이블 뷰 셀** 을 선택 합니다. **바인딩 검사자** 의 **값** 접혀에서 **바인딩 및** **테이블 셀 뷰**를 선택 합니다. **모델 키 경로**에 대 한 `objectValue.Name`를 입력 합니다.

    [![모델 키 경로 입력](databinding-images/outline09.png "모델 키 경로 입력")](databinding-images/outline09-large.png#lightbox)
4. `objectValue`는 트리 컨트롤러에서 관리 하는 배열의 현재 `PersonModel`입니다.
5. **직업** 열 아래에서 **테이블 뷰 셀** 을 선택 합니다. **바인딩 검사자** 의 **값** 접혀에서 **바인딩 및** **테이블 셀 뷰**를 선택 합니다. **모델 키 경로**에 대 한 `objectValue.Occupation`를 입력 합니다.

    [![모델 키 경로 입력](databinding-images/outline10.png "모델 키 경로 입력")](databinding-images/outline10-large.png#lightbox)
6. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

응용 프로그램을 실행 하는 경우 개요는 `PersonModels`배열로 채워집니다.

[![응용 프로그램 실행](databinding-images/outline11.png "애플리케이션 실행")](databinding-images/outline11-large.png#lightbox)

### <a name="collection-view-data-binding"></a>컬렉션 뷰 데이터 바인딩

컬렉션 뷰를 사용 하는 데이터 바인딩은 배열 컨트롤러를 사용 하 여 컬렉션에 대 한 데이터를 제공 하므로 테이블 뷰로 바인딩하는 것과 매우 비슷합니다. 컬렉션 뷰에는 미리 설정 된 표시 형식이 없으므로 사용자 상호 작용 피드백을 제공 하 고 사용자 선택을 추적 하려면 더 많은 작업이 필요 합니다.

> [!IMPORTANT]
> Xcode 7 및 macOS 10.11 이상의 문제로 인해 컬렉션 뷰를 Storyboard 파일 (storyboard) 내에서 사용할 수 없습니다. 따라서 xib 파일을 계속 사용 하 여 Xamarin.ios 앱에 대 한 컬렉션 뷰를 정의 해야 합니다. 자세한 내용은 [컬렉션 뷰](~/mac/user-interface/collection-view.md) 설명서를 참조 하세요.

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

1. **Collection View Item** -  That manages a single instance of an item in the collection.
2. **View** - A custom view that provides the visual size and appearance of each item in the collection. This view is tied to and managed by the **Collection View Item**.

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

## <a name="debugging-native-crashes"></a>네이티브 충돌 디버깅

데이터 바인딩에서 실수를 하면 비관리 코드에서 _네이티브 충돌이_ 발생 하 여 xamarin.ios 응용 프로그램이 완전히 실패 하 고 `SIGABRT` 오류가 발생 합니다.

[![네이티브 크래시 대화 상자의 예](databinding-images/debug01.png "네이티브 크래시 대화 상자의 예")](databinding-images/debug01-large.png#lightbox)

일반적으로 데이터 바인딩 중에 기본 충돌의 네 가지 주요 원인은 다음과 같습니다.

1. 데이터 모델은 `NSObject` 또는 `NSObject`의 서브 클래스에서 상속 되지 않습니다.
2. `[Export("key-name")]` 특성을 사용 하 여 속성을 객관적인 C에 노출 하지 않았습니다.
3. `WillChangeValue` 및 `DidChangeValue` 메서드 호출에서 접근자의 값에 대 한 변경 내용을 줄 바꿈하지 않습니다 (`Export` 특성과 동일한 키 지정).
4. Interface Builder의 **바인딩 검사기** 에 잘못 된 키 또는 잘못 된 키가 있습니다.

### <a name="decoding-a-crash"></a>크래시 디코딩

데이터 바인딩에서 네이티브 크래시를 발생 하 여이를 찾고 수정 하는 방법을 보여 줄 수 있습니다. Interface Builder 컬렉션 뷰 예제에서 첫 번째 레이블의 바인딩을 `Name`에서 `Title`으로 변경 하겠습니다.

[![바인딩 키 편집](databinding-images/debug02.png "바인딩 키 편집")](databinding-images/debug02-large.png#lightbox)

변경 내용을 저장 하 고, Mac용 Visual Studio으로 다시 전환 하 여 Xcode와 동기화 하 고, 응용 프로그램을 실행 해 보겠습니다. 컬렉션 뷰가 표시 되 면 `PersonModel`는 키 `Title`를 사용 하 여 속성을 노출 하지 않으므로 응용 프로그램이 Mac용 Visual Studio의 **응용 프로그램 출력** 에 표시 된 대로 `SIGABRT` 오류와 함께 일시적으로 중단 됩니다.

[![바인딩 오류의 예](databinding-images/debug03.png "바인딩 오류의 예")](databinding-images/debug03-large.png#lightbox)

**응용 프로그램 출력** 에서 오류의 맨 위로 스크롤하면 문제 해결을 위한 키를 볼 수 있습니다.

[![오류 로그에서 문제 찾기](databinding-images/debug04.png "오류 로그에서 문제 찾기")](databinding-images/debug04-large.png#lightbox)

이 줄은 바인딩할 개체에 키 `Title` 존재 하지 않음을 알려 주는 것입니다. 바인딩을 Interface Builder, 저장, 동기화, 다시 작성 및 실행 `Name`로 다시 변경 하는 경우 응용 프로그램은 문제가 발생 하지 않고 정상적으로 실행 됩니다.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 응용 프로그램에서 데이터 바인딩 및 키-값 코딩을 사용 하는 방법에 대해 자세히 살펴봅니다. 먼저 KVC (키-값 C# 코딩) 및 키-값 관찰 (KVO)을 사용 하 여 클래스를 목표 C로 노출 하는 방법을 살펴보았습니다. 다음으로 KVO 규격 클래스를 사용 하 고 데이터를 Xcode의 Interface Builder에 있는 UI 요소에 바인딩하는 방법을 보여 주었습니다. 마지막으로, **배열 컨트롤러** 와 **트리 컨트롤러**를 사용 하 여 복잡 한 데이터 바인딩을 보여 주었습니다.

## <a name="related-links"></a>관련 링크

- [MacDatabinding 스토리 보드 (샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/macdatabinding-storyboard)
- [MacDatabinding Xib (샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/macdatabinding-xibs)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [표준 컨트롤](~/mac/user-interface/standard-controls.md)
- [테이블 보기](~/mac/user-interface/table-view.md)
- [개요 보기](~/mac/user-interface/outline-view.md)
- [컬렉션 뷰](~/mac/user-interface/collection-view.md)
- [키-값 코딩 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [키-값을 관찰 하는 프로그래밍 가이드 소개](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html)
- [Cocoa 바인딩 프로그래밍 항목 소개](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Cocoa 바인딩 참조 소개](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [macOS 휴먼 인터페이스 지침](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
