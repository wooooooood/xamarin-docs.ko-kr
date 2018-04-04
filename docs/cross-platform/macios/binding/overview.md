---
title: 개요
description: 바인딩 프로세스의 작동 방식에 대 한 세부 정보
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: d1a90934cf7a9a832172f32ed95cf3e254e04385
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="overview"></a>개요

_바인딩 프로세스의 작동 방식에 대 한 세부 정보_

Xamarin 사용한 사용에 대 한 Objective C 라이브러리 바인딩 다음 세 단계로 수행 합니다.

1. C# "정의 API" 작성 설명 하기 위해 어떻게 네이티브 API에 노출 된.NET 및 기본 목표 C.에 매핑되는 방식 이 작업은 수행 같은 구문을 사용 하 여 표준 C# `interface` 및 다양 한 바인딩 **특성** (이 [간단한 예제](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)).

2. 한 번을 작성 했습니다 "API 정의" C#에서는 "바인딩" 어셈블리를 생성 하기 위해 컴파일합니다. 이 작업을 수행할 수 있습니다는 [ **명령줄** ](#commandline) 또는 사용 하 여 한 [ **바인딩 프로젝트** ](#bindingproject) Mac 또는 Visual Studio 용 Visual Studio에서.

3. 그러면 사용자가 정의한 API를 사용 하는 기본 기능에 액세스할 수 있도록 해당 "바인딩" 어셈블리 Xamarin 응용 프로그램 프로젝트에 추가 됩니다.
  바인딩 프로젝트가 응용 프로그램 프로젝트와에서 완전히 분리 됩니다.

**참고:** 사용 하 여 1 단계를 자동화할 수 있습니다 [ **목표 Sharpie**](#objectivesharpie)합니다. Objective-c API를 검사 하 고 생성 하는 제안 된 C# ""API 정의 합니다. 목표 Sharpie에서 생성 된 파일을 사용자 지정할 수 있으며 바인딩 프로젝트 (또는 명령줄에서) 사용할 수 바인딩 어셈블리를 만들려고 합니다. 목표 Sharpie 단독으로 바인딩을 만들지 않습니다를 단순히 더 큰 프로세스의 선택적 부분.

자세한 기술 세부 정보를 읽을 수 있습니다 [방식과](#howitworks), 도움이 되는 값은 바인딩이 작성할 수 있습니다.

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>명령줄 바인딩

사용할 수 있습니다는 `btouch-native` Xamarin.iOS에 대 한 (또는 `bmac-native` Xamarin.Mac를 사용 하는 경우) 바인딩을 직접 작성 합니다. 사용자가 직접 만든 C# API 정의 전달 하 여 작동 (또는 목표 Sharpie를 사용 하 여) 명령줄 도구에 (`btouch-native` iOS에 대 한 또는 `bmac-native` Mac 용).


이러한 도구를 호출 하기 위한 일반 구문은 다음과 같습니다.

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

위의 명령 파일을 생성 합니다 `cocos2d.dll` 현재 디렉터리에 프로젝트에서 사용할 수 있는 완전히 바인딩된 라이브러리를 포함 합니다. 이 도구는 Mac에 대 한 Visual Studio에서 바인딩 프로젝트를 사용 하는 경우 사용자 바인딩을 만드는 데 사용할 도구 (설명 [아래](#bindingproject)).


<a name="bindingproject" />

## <a name="binding-project"></a>바인딩 프로젝트

바인딩 프로젝트 Mac 또는 Visual Studio (Visual Studio에서는 iOS 바인딩을을 지원)에 대 한 Visual Studio에서 만들 수 있으며 쉽게를 편집 하 여 빌드 명령줄을 사용 하 여) (아닌 바인딩에 대 한 API 정의 합니다.

이 따라 [시작 가이드](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started) 만들고 바인딩을 생성 하는 바인딩 프로젝트를 사용 하는 방법을 보려면 합니다.

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>목표 Sharpie

목표 Sharpie는 바인딩을 만드는 초기 단계에 도움이 되는 다른, 별도 명령줄 도구입니다. 단독으로 바인딩의 생성 하지 않습니다, 그리고 대신 대상 네이티브 라이브러리에 대 한 API 정의 생성 하는 초기 단계를 자동화 합니다.

읽기는 [목표 Sharpie docs](~/cross-platform/macios/binding/objective-sharpie/index.md) 바인딩을에 빌드할 수 있는 API 정의 네이티브 라이브러리, 네이티브 프레임 워크 및 CocoaPods 구문 분석 하는 방법을 배울 수 있습니다.

<a name="howitworks" />

## <a name="how-binding-works"></a>바인딩의 작동 방식

사용할 수는 [[등록]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) 특성 [[내보내기]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) 특성 및 [수동 Objective-c 선택기 호출](~/ios/internals/objective-c-selectors.md) 함께 바인딩할 수동으로 new (이전에 Objective C 형식 바인딩 안 됨된)입니다.

먼저 바인딩할 하는 형식을 찾습니다. 토론 목적으로 (및 단순)에서는 바인딩합니다는 [NSEnumerator](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSEnumerator_Class/Reference/Reference.html) 유형 (에 바인딩된 이미 있는 [Foundation.NSEnumerator](https://developer.xamarin.com/api/type/Foundation.NSEnumerator/); 목적으로 예를 들어 바로 아래 구현을).

둘째, C# 형식을 만드는 데 필요 합니다. 이 네임 스페이스를 포함할 가능성이 하고자 사용 하도록 해야 Objective-c 네임 스페이스를 지원 하지 않으므로 `[Register]` Objective C 런타임 Xamarin.iOS 등록 하는 형식 이름을 변경할 특성입니다. C# 형식에서 상속 되어야 [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/):

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

셋째, Objective-c 설명서를 검토 하 고 만들 [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) 사용 하려는 각 선택기에 대 한 인스턴스. 클래스 본문 내에서이 배치 합니다.

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

넷째, 형식 생성자를 제공 해야 합니다. 하면 *해야* 기본 클래스 생성자에 프로그램 생성자 호출을 연결 합니다. `[Export]` 특성에 지정 된 선택기 이름으로 생성자를 호출 하는 Objective C 코드 허용:

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

다섯째, 3 단계에서에서 선언 된 각 선택기에 대 한 메서드를 제공 합니다. 이러한 ´ ֲ `objc_msgSend()` 네이티브 개체에 선택기를 호출 합니다. 사용 하 여 [Runtime.GetNSObject()](https://developer.xamarin.com/api/member/ObjCRuntime.Runtime.GetNSObject/(System.IntPtr)) 변환 하는 `IntPtr` 에 적절 하 게 형식화 된 `NSObject` (sub) 형식입니다. 멤버를 Objective C 코드에서 호출할 수 있으려면 메서드로 *해야* 수 **가상**합니다.

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

정리:

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

