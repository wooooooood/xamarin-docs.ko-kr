---
title: Android의 연결
ms.prod: xamarin
ms.assetid: 3528E195-AA74-90AF-B5F3-3B65FB4F0BB8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/30/2018
ms.openlocfilehash: bcc9617553be425ab17050a1a6fb034f6d7f596d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30767585"
---
# <a name="linking-on-android"></a>Android의 연결

Xamarin.Android 응용 프로그램은 *링커*를 사용하여 응용 프로그램의 크기를 줄일 수 있습니다. 링커는 응용 프로그램의 정적 분석을 통해 실제로 사용되는 어셈블리, 형식 및 멤버를 확인합니다. 그런 다음, *가비지 수집기*로 작동하여 참조되는 어셈블리, 형식 및 멤버의 의 완전한 클로저가 발견될 때까지, 참조되는 어셈블리, 형식 및 멤버를 계속 찾습니다. 그런 다음, 이 클로저 외부의 모든 것은 *삭제*됩니다.

예를 들어 [Hello, Android](https://developer.xamarin.com/samples/HelloM4A/) 샘플은 다음과 같습니다.

|구성|1.2.0 크기|4.0.1 크기|
|---|---|---|
|연결 없이 릴리스:|14.0MB|16.0MB|
|연결 없이 릴리스:|4.2MB|2.9MB|

연결 결과 1.2.0에서 원래(연결되지 않은) 패키지 크기의 30%, 4.0.1에서 연결되지 않은 패키지 크기의 18%에 해당하는 패키지가 생성됩니다.



## <a name="control"></a>Control

연결은 *정적 분석*을 기반으로 합니다. 따라서 런타임 환경에 종속된 모든 것은 검색되지 않습니다.

```csharp
// To play along at home, Example must be in a different assembly from MyActivity.
public class Example {
    // Compiler provides default constructor...
}

[Activity (Label="Linker Example", MainLauncher=true)]
public class MyActivity {
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Will this work?
        var o = Activator.CreateInstance (typeof (ExampleLibrary.Example));
    }
}
```


### <a name="linker-behavior"></a>링커 동작

링커 제어의 기본 메커니즘은 **프로젝트 옵션** 대화 상자 내 **링커 동작**(Visual Studio의 *연결*) 드롭다운입니다. 여기에는 세 가지 옵션이 있습니다.

1.  **연결하지 않음**(Visual Studio의 *없음*)
1.  **SDK 어셈블리 연결**(*SDK 어셈블리만*)
1.  **모든 어셈블리 연결**(*SDK 및 사용자 어셈블리*)


**연결하지 않음** 옵션은 링커를 해제합니다. 위의 "연결 없이 릴리스" 응용 프로그램 크기 예제에서는 이 동작을 사용했습니다. 이는 런타임 오류를 해결할 때 링커가 원인인지 파악하는 데 유용합니다. 이 설정은 일반적으로 프로덕션 빌드에 권장되지 않습니다.

**SDK 어셈블리 연결** 옵션은 [Xamarin.Android와 함께 제공되는 어셈블리](~/cross-platform/internals/available-assemblies.md)만 연결합니다.
다른 모든 어셈블리(예: 코드)는 연결되지 않습니다.

**모든 어셈블리 연결** 옵션은 모든 어셈블리를 연결하므로, 정적 참조가 없을 경우 코드도 제거될 수 있습니다.

위의 예제에서는 *연결하지 않음* 및 *SDK 어셈블리 연결* 옵션을 사용할 수 있으며, *모든 어셈블리 연결* 동작을 사용할 경우 다음 오류가 생성됩니다.

```shell
E/mono    (17755): [0xafd4d440:] EXCEPTION handling: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
I/MonoDroid(17755): UNHANDLED EXCEPTION: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
I/MonoDroid(17755): at System.Activator.CreateInstance (System.Type,bool) <0x00180>
I/MonoDroid(17755): at System.Activator.CreateInstance (System.Type) <0x00017>
I/MonoDroid(17755): at LinkerScratch2.Activity1.OnCreate (Android.OS.Bundle) <0x00027>
I/MonoDroid(17755): at Android.App.Activity.n_OnCreate_Landroid_os_Bundle_ (intptr,intptr,intptr) <0x00057>
I/MonoDroid(17755): at (wrapper dynamic-method) object.95bb4fbe-bef8-4e5b-8e99-ca83a5d7a124 (intptr,intptr,intptr) <0x00033>
E/mono    (17755): [0xafd4d440:] EXCEPTION handling: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
E/mono    (17755):
E/mono    (17755): Unhandled Exception: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
E/mono    (17755):   at System.Activator.CreateInstance (System.Type type, Boolean nonPublic) [0x00000] in <filename unknown>:0
E/mono    (17755):   at System.Activator.CreateInstance (System.Type type) [0x00000] in <filename unknown>:0
E/mono    (17755):   at LinkerScratch2.Activity1.OnCreate (Android.OS.Bundle bundle) [0x00000] in <filename unknown>:0
E/mono    (17755):   at Android.App.Activity.n_OnCreate_Landroid_os_Bundle_ (IntPtr jnienv, IntPtr native__this, IntPtr native_savedInstanceState) [0x00000] in <filename unknown>:0
E/mono    (17755):   at (wrapper dynamic-method) object:95bb4fbe-bef8-4e5b-8e99-ca83a5d7a124 (intptr,intptr,intptr)
```


### <a name="preserving-code"></a>코드 유지

링커는 사용자가 유지하려는 코드를 제거하는 경우가 있습니다. 예:

-   `System.Reflection.MemberInfo.Invoke`를 통해 동적으로 호출하는 코드가 있을 수 있습니다.

-   형식을 동적으로 인스턴스화하는 경우 형식의 기본 생성자를 유지하는 것이 좋습니다.

-   XML 직렬화를 사용하는 경우에 형식의 속성을 유지하는 것이 좋습니다.

이러한 경우 [Android.Runtime.Preserve](https://developer.xamarin.com/api/type/Android.Runtime.PreserveAttribute/) 특성을 사용할 수 있습니다. 응용 프로그램에 의해 정적으로 연결되지 않은 모든 멤버는 제거될 수 있으므로, 이 특성을 사용하여 정적으로 참조되지 않았지만 여전히 응용 프로그램에 필요한 멤버를 표시할 수 있습니다. 형식의 모든 멤버 또는 형식 자체에 이 특성을 적용할 수 있습니다.

다음 예에서는 이 특성을 사용하여 `Example` 클래스의 생성자를 유지합니다.

```csharp
public class Example
{
    [Android.Runtime.Preserve]
    public Example ()
    {
    }
}
```

전체 형식을 유지하려는 경우 다음 특성 구문을 사용할 수 있습니다.

```csharp
[Android.Runtime.Preserve (AllMembers = true)]
```

예를 들어 다음 코드 조각에서는 XML 직렬화를 위해 전체 `Example` 클래스가 유지됩니다.

```csharp
[Android.Runtime.Preserve (AllMembers = true)]
class Example
{
    // Compiler provides default constructor...
}
```

특정 멤버를 유지하려는 경우가 있습니다(포함 형식이 유지된 경우에만 해당). 이러한 경우에는 다음 특성 구문을 사용합니다.

```csharp
[Android.Runtime.Preserve (Conditional = true)]
```

Xamarin 라이브러리에 대한 종속성을 유지하지 않으려는 경우(예를 들어 플랫폼 간에 이식 가능한 클래스 라이브러리(PCL)를 빌드하는 경우)에도 `Android.Runtime.Preserve` 특성을 사용할 수 있습니다. 이렇게 하려면 다음과 같이 `Android.Runtime` 내에 `PreserveAttribute` 클래스를 선언합니다.

```csharp
namespace Android.Runtime
{
    public sealed class PreserveAttribute : System.Attribute
    {
        public bool AllMembers;
        public bool Conditional;
    }
}
```

위의 예에서 `Preserve` 특성은 `Android.Runtime` 네임스페이스에 선언되어 있습니다. 그러나 링커가 형식 이름별로 이 특성을 조회하기 때문에 임의의 네임스페이스에서 `Preserve` 특성을 사용할 수 있습니다.



### <a name="falseflag"></a>falseflag

[Preserve] 특성을 사용할 수 없는 경우에는 링커가 해당 형식이 사용된다고 믿도록 코드 블록을 제공하는 한편 이 코드 블록이 런타임에 실행되지 않도록 하면 유용합니다. 이 기법을 활용하려면 다음과 같이 할 수 있습니다.

```csharp
[Activity (Label="Linker Example", MainLauncher=true)]
class MyActivity {

#pragma warning disable 0219, 0649
    static bool falseflag = false;
    static MyActivity ()
    {
        if (falseflag) {
            var ignore = new Example ();
        }
    }
#pragma warning restore 0219, 0649

    // ...
}
```



### <a name="linkskip"></a>linkskip

사용자가 제공한 어셈블리 집합을 절대 연결해서는 안 된다고 지정하고, [AndroidLinkSkip MSBuild 속성](~/android/deploy-test/building-apps/build-process.md)을 사용하여 *SDK 어셈블리 연결* 동작으로 다른 사용자 어셈블리를 건너뛸 수 있게 할 수 있습니다.

```xml
<PropertyGroup>
    <AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
</PropertyGroup>
```


### <a name="linkdescription"></a>LinkDescription

[사용자 지정 링커 구성 파일](~/cross-platform/deploy-test/linker.md)을 포함한 파일에서 [`@(LinkDescription)`](~/android/deploy-test/building-apps/build-process.md)
**빌드 동작**을 사용할 수 있습니다.
추가합니다. 사용자 지정 링커 구성 파일은 유지해야 하는 `internal` 또는 `private` 멤버를 유지하는 데 필요할 수 있습니다.



### <a name="custom-attributes"></a>사용자 지정 특성

어셈블리가 연결되면 모든 멤버에서 다음 사용자 지정 특성 형식이 제거됩니다.

-  System.ObsoleteAttribute
-  System.MonoDocumentationNoteAttribute
-  System.MonoExtensionAttribute
-  System.MonoInternalNoteAttribute
-  System.MonoLimitationAttribute
-  System.MonoNotSupportedAttribute
-  System.MonoTODOAttribute
-  System.Xml.MonoFIXAttribute


어셈블리가 연결되면 릴리스 빌드의 모든 멤버에서 다음 사용자 지정 특성 형식이 제거됩니다.

-  System.Diagnostics.DebuggableAttribute
-  System.Diagnostics.DebuggerBrowsableAttribute
-  System.Diagnostics.DebuggerDisplayAttribute
-  System.Diagnostics.DebuggerHiddenAttribute
-  System.Diagnostics.DebuggerNonUserCodeAttribute
-  System.Diagnostics.DebuggerStepperBoundaryAttribute
-  System.Diagnostics.DebuggerStepThroughAttribute
-  System.Diagnostics.DebuggerTypeProxyAttribute
-  System.Diagnostics.DebuggerVisualizerAttribute


## <a name="related-links"></a>관련 링크

- [사용자 지정 링커 구성](~/cross-platform/deploy-test/linker.md)
- [iOS에서 연결](~/ios/deploy-test/linker.md)
