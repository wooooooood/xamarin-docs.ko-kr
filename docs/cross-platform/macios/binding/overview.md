---
title: 목적-C 바인딩 개요
description: 이 문서에서는 명령줄 바인딩, 바인딩 프로젝트 및 목표 C# Sharpie를 포함 하 여 목표 C 코드에 대 한 바인딩을 만드는 다양 한 방법에 대 한 개요를 제공 합니다. 또한 바인딩의 작동 방식을 설명 합니다.
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
author: davidortinau
ms.author: daortin
ms.date: 11/25/2015
ms.openlocfilehash: be2f7f555b76d472f7a66d95e661bb2f5884c58f
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725343"
---
# <a name="overview-of-objective-c-bindings"></a>목적-C 바인딩 개요

_바인딩 프로세스의 작동 방식에 대 한 세부 정보_

Xamarin과 함께 사용 하기 위해 목표-C 라이브러리를 바인딩하는 데는 세 가지 단계가 사용 됩니다.

1. 기본 API C# 가 .net에 노출 되는 방식과 기본 목표에 매핑되는 방법을 설명 하는 "api 정의"를 작성 합니다. `interface` 및 다양 한 바인딩 C# **특성과** 같은 표준 구문을 사용 하 여 수행 됩니다 (이 [간단한 예제](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)참조).

2. 에서 C#"API 정의"를 작성 한 후에는이를 컴파일하여 "바인딩" 어셈블리를 생성 합니다. 이 작업은 [**명령줄**](#commandline) 에서 또는 Mac용 Visual Studio 또는 Visual Studio에서 [**바인딩 프로젝트**](#bindingproject) 를 사용 하 여 수행할 수 있습니다.

3. 그러면 사용자가 정의한 API를 사용 하 여 네이티브 기능에 액세스할 수 있도록 해당 "바인딩" 어셈블리가 Xamarin 응용 프로그램 프로젝트에 추가 됩니다.
   바인딩 프로젝트는 응용 프로그램 프로젝트와 완전히 분리 됩니다.

   > [!NOTE]
   > 1 단계는 [**목표 Sharpie**](#objectivesharpie)지원 하 여 자동화할 수 있습니다. 이 클래스는 목표 C API를 검사 하 고 제안 C# 된 "api 정의"를 생성 합니다. Sharpie를 사용 하 여 만든 파일을 사용자 지정 하 고 바인딩 프로젝트 또는 명령줄에서 사용 하 여 바인딩 어셈블리를 만들 수 있습니다. 목표 Sharpie 자체적으로 바인딩을 만들지 않으며, 단순히 더 큰 프로세스의 선택적 부분입니다.

[작동 방식](#howitworks)에 대 한 자세한 기술 정보를 읽고 바인딩을 작성 하는 데 도움이 될 수도 있습니다.

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>명령줄 바인딩

Xamarin.ios (또는 Xamarin.ios를 사용 하는 경우 `bmac-native`)에 대 한 `btouch-native`를 사용 하 여 바인딩을 직접 빌드할 수 있습니다. 직접 만든 C# API 정의 (또는 목표 Sharpie 사용)를 명령줄 도구`btouch-native` (IOS 또는 Mac 용 `bmac-native`)에 전달 하는 방식으로 작동 합니다.

이러한 도구를 호출 하는 일반적인 구문은 다음과 같습니다.

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

위의 명령은 현재 디렉터리에 `cocos2d.dll` 파일을 생성 하 고 프로젝트에서 사용할 수 있는 완전히 바인딩된 라이브러리를 포함 합니다. 바인딩 프로젝트를 사용 하는 경우 바인딩을 만드는 데 사용 하 Mac용 Visual Studio는 도구입니다 ( [아래](#bindingproject)설명 참조).

<a name="bindingproject" />

## <a name="binding-project"></a>바인딩 프로젝트

바인딩 프로젝트는 Mac용 Visual Studio 또는 Visual Studio에서 만들 수 있습니다. Visual Studio는 iOS 바인딩만 지원 하며, 명령줄을 사용 하는 것과 비교 하 여 바인딩 용 API 정의를 더 쉽게 편집 및 빌드할 수 있게 해줍니다.

이 [시작 가이드](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started) 에 따라 바인딩 프로젝트를 만들고 사용 하 여 바인딩을 생성 하는 방법을 확인 합니다.

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>Objective Sharpie

목표 Sharpie는 바인딩을 만드는 초기 단계에 도움이 되는 또 다른 별도의 명령줄 도구입니다. 바인딩 자체를 만들지 않고 대상 네이티브 라이브러리에 대 한 API 정의를 생성 하는 초기 단계를 자동화 합니다.

기본 라이브러리, 네이티브 프레임 워크 및 CocoaPods를 바인딩에 기본 제공 되는 API 정의로 구문 분석 하는 방법에 대해 알아보려면 [목표 Sharpie 문서](~/cross-platform/macios/binding/objective-sharpie/index.md) 를 참조 하세요.

<a name="howitworks" />

## <a name="how-binding-works"></a>바인딩 작동 방법

[[Register]](xref:Foundation.RegisterAttribute) 특성, [[Export]](xref:Foundation.ExportAttribute) 특성 및 [수동 목표-c 선택기 호출](~/ios/internals/objective-c-selectors.md) 을 함께 사용 하 여 새 (이전에는 바인딩되지 않은) 목표-c 형식을 수동으로 바인딩할 수 있습니다.

먼저 바인딩하려는 형식을 찾습니다. 토론 목적 (및 단순성)을 위해 [NSEnumerator](https://developer.apple.com/documentation/foundation/nsenumerator) 형식 ( [NSEnumerator](xref:Foundation.NSEnumerator)에 이미 바인딩되어 있음)을 바인딩합니다. 아래 구현은 단지 예를 제공 하기 위한 것입니다.

두 번째로, C# 형식을 만들어야 합니다. 네임 스페이스에이를 추가할 수 있습니다. 목표-C는 네임 스페이스를 지원 하지 않으므로 `[Register]` 특성을 사용 하 여 Xamarin.ios에서 목표-C 런타임으로 등록 하는 형식 이름을 변경 해야 합니다. 또한 C# 이 형식은 [기본 nsobject](xref:Foundation.NSObject)에서 상속 해야 합니다.

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

셋째, 목표-C 설명서를 검토 하 고 사용 하려는 각 선택기에 대해 [Objcruntime 선택기](xref:ObjCRuntime.Selector) 인스턴스를 만듭니다. 이러한 클래스를 클래스 본문에 추가 합니다.

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

네, 형식은 생성자를 제공 해야 합니다. 생성자 호출을 기본 클래스 생성자에 연결 *해야 합니다* . `[Export]` 특성은 지정 된 선택기 이름을 사용 하 여 생성자를 호출 하는 것을 목표로 C 코드를 허용 합니다.

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

5\.3 단계에서 선언 된 각 선택기에 대해 메서드를 제공 합니다. 이렇게 하면 `objc_msgSend()`를 사용 하 여 네이티브 개체에 대 한 선택기를 호출 합니다. [런타임. GetNSObject ()](xref:ObjCRuntime.Runtime.GetNSObject*) 를 사용 하 여 `IntPtr`를 적절 하 게 형식화 된 `NSObject` (하위) 형식으로 변환 합니다. 메서드를 목표 C 코드에서 호출할 수 있도록 하려면 멤버가 **virtual** *이어야* 합니다.

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

모두 함께 배치:

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
