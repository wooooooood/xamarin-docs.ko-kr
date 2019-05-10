---
title: Objective-c 바인딩 개요
description: 만드는 다양 한 방법의 개요를 제공 하는이 문서 C# 명령줄 바인딩, 바인딩 프로젝트 목표 Sharpie 등 Objective-c 코드에 대 한 바인딩. 또한 바인딩의 작동 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
author: asb3993
ms.author: amburns
ms.date: 11/25/2015
ms.openlocfilehash: ec173c0ed7881439ecbe2b5cf83c8f5484c7e5aa
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/01/2019
ms.locfileid: "64977622"
---
# <a name="overview-of-objective-c-bindings"></a>Objective-c 바인딩 개요

_바인딩 프로세스의 작동 원리의 세부 정보_

Xamarin 사용에 대 한 Objective-c 라이브러리 바인딩 세 단계로 수행 합니다.

1. 쓰기는 C# "API 정의" 설명 하는 네이티브 API 노출 되는 방식을.NET 및 기본 목표 C에 매핑하는 방법 이렇게 표준을 사용 하 여 C# 구문 등 `interface` 및 다양 한 바인딩 **특성** (이 참조 하십시오 [간단한 예제](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)).

2. "API 정의" 작성 한 C#를 "바인딩" 어셈블리를 생성 하기 위해 컴파일할 합니다. 이 작업을 수행할 수 있습니다는 [ **명령줄** ](#commandline) 사용 하 여 또는 [ **바인딩 프로젝트** ](#bindingproject) Mac 또는 Visual Studio 용 Visual Studio에서.

3. 해당 "바인딩" 어셈블리는 정의한 API를 사용 하는 기본 기능에 액세스할 수 있도록 다음 Xamarin 응용 프로그램 프로젝트에 추가 됩니다.
  바인딩 프로젝트는 응용 프로그램 프로젝트와 완전히 별개입니다.

**참고:** 1 단계를 사용 하 여 자동화할 수 있습니다 [ **목표 Sharpie**](#objectivesharpie)합니다. Objective C API를 검사 하 고 생성 된 제안 된 C# "API 정의입니다." 목표 Sharpie에서 만든 파일을 사용자 지정 하 고 바인딩 프로젝트 (또는 명령줄)에서 사용할 수 있습니다에 바인딩 어셈블리를 만들 수 있습니다. 목표 Sharpie 자체적으로 바인딩을 만들지 않으므로 단순히 대규모 프로세스의 선택적 부분입니다.

자세한 기술 정보를 참조할 수 있습니다 [작동 방식](#howitworks), 바인딩을 작성 하는 데 도움이 됩니다입니다.

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>명령줄 바인딩

사용할 수는 `btouch-native` Xamarin.iOS 용 (또는 `bmac-native` Xamarin.Mac을 사용 하는 경우) 바인딩을 직접 빌드할 수 있습니다. 전달 하 여 작동 합니다 C# API 정의 직접 만든 (또는 목표 Sharpie를 사용 하 여) 명령줄 도구 (`btouch-native` iOS 용 또는 `bmac-native` Mac 용).


이러한 도구를 호출 하는 것에 대 한 일반 구문은 다음과 같습니다.

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

위의 명령은 파일을 생성 `cocos2d.dll` 현재 디렉터리의 프로젝트에서 사용할 수 있는 완전히 바인딩된 라이브러리 포함 됩니다. 바인딩 프로젝트를 사용 하는 경우 바인딩을 만들려면 Mac 용 Visual Studio를 사용 하는 도구입니다 (설명 [아래](#bindingproject)).


<a name="bindingproject" />

## <a name="binding-project"></a>프로젝트 바인딩

바인딩 프로젝트 Mac 또는 Visual Studio (Visual Studio에서는 iOS 바인딩을을 지원)에 대 한 Visual Studio에서 만들 수 있으며 쉽게 편집 및 빌드 (명령줄을 사용 하 여) 및 바인딩에 대 한 API 정의 합니다.

이 따라 [시작 가이드](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started) 만들고 바인딩을 생성할 바인딩 프로젝트를 사용 하는 방법을 확인 합니다.

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>Objective Sharpie

목표 Sharpie는 바인딩을 만드는 방법의 초기 단계를 사용 하 여는 데 도움이 되는 다른, 별도 명령줄 도구입니다. 자체적으로 바인딩을 만들지 않습니다, 그리고 대신 대상 네이티브 라이브러리에 대 한 API 정의 생성의 초기 단계를 자동화 합니다.

읽기를 [목표 Sharpie docs](~/cross-platform/macios/binding/objective-sharpie/index.md) 바인딩에 빌드할 수 있는 API 정의를 네이티브 라이브러리, 네이티브 프레임 워크 및 CocoaPods를 구문 분석 하는 방법을 알아보려면.

<a name="howitworks" />

## <a name="how-binding-works"></a>바인딩의 작동 방법

사용 하는 것이 가능 합니다 [[등록]](xref:Foundation.RegisterAttribute) 특성인 [[내보내기]](xref:Foundation.ExportAttribute) 특성인 및 [수동 Objective-c 선택기 호출](~/ios/internals/objective-c-selectors.md) 함께 바인딩할 수동으로 새 (이전에 바인딩되지 않은) Objective-c 형식입니다.

먼저, 바인딩할 하려는 형식을 찾습니다. 바인딩한에서는 설명을 위해 (및 단순성)는 [NSEnumerator](https://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSEnumerator_Class/Reference/Reference.html) 형식 (에서 이미 바인딩 되었으면 하는 [Foundation.NSEnumerator](xref:Foundation.NSEnumerator); 아래 구현은 예를 들어 방금 목적).

둘째, 생성 해야 합니다 C# 형식입니다. 네임 스페이스에 배치 해야 할 수 있습니다. 사용 해야 Objective-c 네임 스페이스를 지원 하지 않으므로 `[Register]` 특성 Xamarin.iOS Objective-c 런타임에 등록 하는 형식 이름을 변경할 수 있습니다. C# 형식에서 상속 되어야 합니다 [Foundation.NSObject](xref:Foundation.NSObject):

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

셋째, Objective-c로 설명서를 검토 및 만들기 [ObjCRuntime.Selector](xref:ObjCRuntime.Selector) 사용 하려는 각 선택기에 대 한 인스턴스. 클래스 본문 내에서이 배치 합니다.

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

넷째, 형식 생성자를 제공 해야 합니다. 있습니다 *해야* 기본 클래스 생성자에 생성자 호출을 연결 합니다. `[Export]` 특성 이름 지정 된 선택기를 사용 하 여 생성자를 호출 하는 Objective-c 코드를 허용 합니다.

```csharp
[Export("init")]
public NSEnumerator()
    : base(NSObjectFlag.Empty)
{
    Handle = Messaging.IntPtr_objc_msgSend(this.Handle, selInit.Handle);
}
```

```csharp
// This constructor must be present so that Xamarin.iOS
// can create instances of your type from Objective-C code.
public NSEnumerator(IntPtr handle)
    : base(handle)
{
}
```

다섯째, 3 단계에서에서 선언 된 각 선택기에 대 한 메서드를 제공 합니다. 이러한 사용할지 `objc_msgSend()` 네이티브 개체에서 선택기를 호출 합니다. 사용 하 여 [Runtime.GetNSObject()](xref:ObjCRuntime.Runtime.GetNSObject*) 변환 하는 `IntPtr` 에 적절 하 게 형식화 된 `NSObject` (sub) 형식입니다. Objective C 코드에서 멤버를 호출할 수 있으려면 메서드로 *해야 합니다* 될 **가상**합니다.

```csharp
[Export("nextObject")]
public virtual NSObject NextObject()
{
    return Runtime.GetNSObject(
        Messaging.IntPtr_objc_msgSend(this.Handle, selNextObject.Handle));
}
```

```csharp
// Note that for properties, [Export] goes on the get/set method:
public virtual NSArray AllObjects {
    [Export("allObjects")]
    get {
        return (NSArray) Runtime.GetNSObject(
            Messaging.IntPtr_objc_msgSend(this.Handle, selAllObjects.Handle));
    }
}
```

전체 과정:

```csharp
using System;
using Foundation;
using ObjCRuntime;

namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        static Selector selInit       = new Selector("init");
        static Selector selAllObjects = new Selector("allObjects");
        static Selector selNextObject = new Selector("nextObject");

        [Export("init")]
        public NSEnumerator()
            : base(NSObjectFlag.Empty)
        {
            Handle = Messaging.IntPtr_objc_msgSend(this.Handle,
                selInit.Handle);
        }

        public NSEnumerator(IntPtr handle)
            : base(handle)
        {
        }

        [Export("nextObject")]
        public virtual NSObject NextObject()
        {
            return Runtime.GetNSObject(
                Messaging.IntPtr_objc_msgSend(this.Handle,
                    selNextObject.Handle));
        }

        // Note that for properties, [Export] goes on the get/set method:
        public virtual NSArray AllObjects {
            [Export("allObjects")]
            get {
                return (NSArray) Runtime.GetNSObject(
                    Messaging.IntPtr_objc_msgSend(this.Handle,
                        selAllObjects.Handle));
            }
        }
    }
}
```
