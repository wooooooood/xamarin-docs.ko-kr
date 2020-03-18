---
title: Objective-C 바인딩 개요
description: 이 문서에서는 명령줄 바인딩, 바인딩 프로젝트 및 Objective Sharpie를 포함하여 Objective-C 코드의 C# 바인딩을 만드는 다양한 방법에 대한 개요를 제공합니다. 또한 바인딩의 작동 방식을 설명합니다.
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
author: davidortinau
ms.author: daortin
ms.date: 11/25/2015
ms.openlocfilehash: be2f7f555b76d472f7a66d95e661bb2f5884c58f
ms.sourcegitcommit: 60d2243809d8e980fca90b9f771e72f8c0e64d71
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "76725343"
---
# <a name="overview-of-objective-c-bindings"></a>Objective-C 바인딩 개요

_바인딩 프로세스의 작동 방식에 대한 세부 정보_

Xamarin과 함께 사용할 Objective-C 라이브러리를 바인딩하려면 3단계를 수행해야 합니다.

1. 네이티브 API가 .NET에 노출되는 방식과 기본 Objective-C에 매핑되는 방식을 설명하는 C# "API 정의"를 작성합니다. 이 작업은 `interface` 같은 표준 C# 구문과 다양한 바인딩 **특성**을 사용하여 수행합니다(이 [간단한 예](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API) 참조).

2. C#에서 "API 정의"를 작성한 후에는 이를 컴파일하여 "바인딩" 어셈블리를 생성합니다. 이 작업은 Mac용 Visual Studio 또는 Visual Studio에서 [**명령줄**](#commandline) 또는 [**바인딩 프로젝트**](#bindingproject)를 사용하여 수행할 수 있습니다.

3. 그러면 사용자가 정의한 API를 사용하여 네이티브 기능에 액세스할 수 있도록 해당 "바인딩" 어셈블리가 Xamarin 애플리케이션 프로젝트에 추가됩니다.
   바인딩 프로젝트는 애플리케이션 프로젝트와 완전히 분리됩니다.

   > [!NOTE]
   > 1단계는 [**Objective Sharpie**](#objectivesharpie)의 지원을 받아 자동화할 수 있습니다. 이는 Objective-C API를 살펴보고 제안된 C# "API 정의"를 생성합니다. Objective Sharpie에서 만든 파일을 사용자 지정하고 바인딩 프로젝트(또는 명령줄)에서 이를 사용하여 바인딩 어셈블리를 만들 수 있습니다. Objective Sharpie는 자체적으로 바인딩을 만들지 않으며, 단순히 더 큰 프로세스의 선택적 부분입니다.

또한 바인딩을 작성하는 데 도움이 되는 [작동 방법](#howitworks)에 대한 자세한 기술 정보를 읽을 수 있습니다.

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>명령줄 바인딩

Xamarin.iOS(또는 Xamarin.Mac을 사용하는 경우 `bmac-native`)에 `btouch-native`를 사용하여 바인딩을 직접 빌드할 수 있습니다. 그러려면 직접(또는 Objective Sharpie를 사용하여) 만든 C# API 정의를 명령줄 도구(iOS의 경우`btouch-native`, Mac의 경우 `bmac-native`)에 전달하면 됩니다.

이러한 도구를 호출하는 일반적인 구문은 다음과 같습니다.

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

위의 명령은 현재 디렉터리에 `cocos2d.dll` 파일을 생성하며, 프로젝트에서 사용할 수 있는 완전히 바인딩된 라이브러리를 포함합니다. 바인딩 프로젝트([아래](#bindingproject) 설명 참조)를 사용하는 경우 Mac용 Visual Studio에서 이 도구를 사용하여 바인딩을 만듭니다.

<a name="bindingproject" />

## <a name="binding-project"></a>바인딩 프로젝트

바인딩 프로젝트는 Mac용 Visual Studio 또는 Visual Studio에서 만들 수 있으며(Visual Studio는 iOS 바인딩만 지원) 명령줄을 사용할 때보다 바인딩용 API 정의를 더 쉽게 편집하고 빌드할 수 있게 해줍니다.

바인딩 프로젝트를 만들고 사용하여 바인딩을 생성하는 방법을 보려면 이 [시작 가이드](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started)를 따르세요.

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>Objective Sharpie

Objective Sharpie는 바인딩 생성 초기 단계에 도움이 되는 또 다른 별도의 명령줄 도구입니다. 바인딩 자체를 만들지 않고 대상 네이티브 라이브러리에 대한 API 정의를 생성하는 초기 단계를 자동화합니다.

네이티브 라이브러리, 네이티브 프레임워크 및 CocoaPod를 바인딩으로 빌드할 수 있는 API 정의로 구문 분석하는 방법을 알아보려면 [Objective Sharpie 문서](~/cross-platform/macios/binding/objective-sharpie/index.md)를 읽어보세요.

<a name="howitworks" />

## <a name="how-binding-works"></a>바인딩 작동 방법

[[Register]](xref:Foundation.RegisterAttribute) 특성, [[Export]](xref:Foundation.ExportAttribute) 특성 및 [수동 Objective-C 선택기 호출](~/ios/internals/objective-c-selectors.md)을 함께 사용하여 새로운(이전에 바인딩되지 않은) Objective-C 유형을 수동으로 바인딩할 수 있습니다.

먼저 바인딩하려는 형식을 찾습니다. 설명(및 단순성)을 위해 [Foundation.NSEnumerator](xref:Foundation.NSEnumerator)에 이미 바인딩된 [NSEnumerator](https://developer.apple.com/documentation/foundation/nsenumerator) 형식을 바인딩합니다. 아래 구현은 예시일 뿐입니다.

둘째, C# 형식을 만들어야 합니다. 이를 네임스페이스에 배치하는 것이 좋습니다. Objective-C는 네임스페이스를 지원하지 않으므로 `[Register]` 특성을 사용하여 Xamarin.iOS가 Objective-C 런타임에 등록할 형식 이름을 변경해야 합니다. C# 형식은 또한 [Foundation.NSObject](xref:Foundation.NSObject)에서 상속되어야 합니다.

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

셋째, Objective-C 설명서를 검토하고 사용하려는 각 선택기에 대해 [ObjCRuntime.Selector](xref:ObjCRuntime.Selector) 인스턴스를 만듭니다. 이러한 클래스를 클래스 본문에 배치합니다.

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

넷째, 해당 형식이 생성자를 제공해야 합니다. 생성자 호출은 기본 클래스 생성자에 *연결해야 합니다*. `[Export]` 특성은 Objective-C 코드가 지정된 선택기 이름을 사용하여 생성자를 호출하도록 허용합니다.

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

다섯 째, 3단계에서 선언된 각 선택기에 대한 메서드를 제공합니다. 이는 `objc_msgSend()`를 사용하여 네이티브 개체에 대한 선택기를 호출합니다. [Runtime.GetNSObject()](xref:ObjCRuntime.Runtime.GetNSObject*)를 사용하여 `IntPtr`을 적절한 형식이 지정된 `NSObject`(하위) 형식으로 변환할 수 있습니다. Objective-C 코드에서 메서드를 호출할 수 있도록 하려면 멤버가 **가상***이어야 합니다*.

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

총정리:

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
