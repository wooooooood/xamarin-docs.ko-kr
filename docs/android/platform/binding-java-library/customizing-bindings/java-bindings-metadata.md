---
title: Java 바인딩 메타데이터
description: Xamarin.Android의 C# 코드는 바인딩을 통해 JNI(Java Native Interface)에 지정된 하위 수준 세부 정보를 추상화하는 메커니즘인 Java 라이브러리를 호출합니다. Xamarin.Android는 이러한 바인딩을 생성하는 도구를 제공합니다. 개발자는 이 도구를 통해 메타데이터를 사용하여 바인딩을 만드는 방법을 제어할 수 있으며, 이를 통해 네임스페이스 수정이나 멤버 이름 바꾸기 같은 절차를 수행할 수 있습니다 이 문서에서는 메타데이터가 작동하는 방식을 설명하고, 메타데이터에서 지원하는 특성을 요약하고, 이 메타데이터를 수정하여 바인딩 문제를 해결하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 27CB3C16-33F3-F580-E2C0-968005A7E02E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 2a88888b2306589930ad6386fb69bbd3b48924b7
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571378"
---
# <a name="java-bindings-metadata"></a>Java 바인딩 메타데이터

_Xamarin.Android의 C# 코드는 바인딩을 통해 JNI(Java Native Interface)에 지정된 하위 수준 세부 정보를 추상화하는 메커니즘인 Java 라이브러리를 호출합니다. Xamarin.Android는 이러한 바인딩을 생성하는 도구를 제공합니다. 개발자는 이 도구를 통해 메타데이터를 사용하여 바인딩을 만드는 방법을 제어할 수 있으며, 이를 통해 네임스페이스 수정이나 멤버 이름 바꾸기 같은 절차를 수행할 수 있습니다 이 문서에서는 메타데이터가 작동하는 방식을 설명하고, 메타데이터에서 지원하는 특성을 요약하고, 이 메타데이터를 수정하여 바인딩 문제를 해결하는 방법을 설명합니다._

## <a name="overview"></a>개요

Xamarin.Android **Java 바인딩 라이브러리**는 _바인딩 생성기_라고도 하는 도구의 도움을 받아 기존 Android 라이브러리를 바인딩하는 데 필요한 대부분의 작업을 자동화하려고 시도합니다. Java 라이브러리를 바인딩할 때 Xamarin.Android는 Java 클래스를 검사하고 바인딩할 모든 패키지, 형식 및 멤버 목록을 생성합니다. 이 API 목록은 **릴리스** 빌드의 **\{project directory}\obj\Release\api.xml** 및 **디버그** 빌드의 **\{project directory}\obj\Debug\api.xml**에서 찾을 수 있는 XML 파일에 저장됩니다.

![Obj/Debug 폴더에 있는 api.xml 파일의 위치](java-bindings-metadata-images/java-bindings-metadata-01.png)

바인딩 생성기는 **api.xml** 파일을 지침으로 사용하여 필요한 C# 래퍼 클래스를 생성합니다. 이 XML 파일의 콘텐츠는 Google의 _Android 오픈 소스 프로젝트_ 형식의 변형입니다.
다음 코드 조각은 **api.xml** 콘텐츠의 예입니다.

```xml
<api>
    <package name="android">
        <class abstract="false" deprecated="not deprecated" extends="java.lang.Object"
            extends-generic-aware="java.lang.Object" 
            final="true" 
            name="Manifest" 
            static="false" 
            visibility="public">
            <constructor deprecated="not deprecated" final="false"
                name="Manifest" static="false" type="android.Manifest"
                visibility="public">
            </constructor>
        </class>
...
</api>
```

이 예제의 **api.xml**은 `android` 패키지에서 `java.lang.Object`를 확장하는 `Manifest`라는 클래스를 선언합니다.

대부분의 경우에는 Java API를 ".NET과 비슷한 느낌"으로 만들거나 바인딩 어셈블리를 컴파일할 수 없게 만드는 문제를 해결하려면 사람의 도움이 필요합니다. 예를 들어 Java 패키지 이름을 .NET 네임스페이스로 변경하거나, 클래스 이름을 바꾸거나, 메서드의 반환 형식을 변경해야 할 수 있습니다.

이러한 변경 작업은 **api.xml**을 직접 수정하는 방법으로 수행되지 않습니다.
그 대신, Java 바인딩 라이브러리 템플릿에서 제공하는 특수 XML 파일에 변경 내용이 기록됩니다. Xamarin.Android 바인딩 어셈블리를 컴파일할 때, 바인딩 생성기는 바인딩 어셈블리를 만들 때 이러한 매핑 파일의 영향을 받습니다.

이러한 XML 매핑 파일은 다음과 같은 프로젝트의 **Transforms** 폴더에서 찾을 수 있습니다.

- **MetaData.xml** &ndash; 생성된 바인딩의 네임스페이스를 변경하는 등 최종 API를 변경할 수 있습니다. 

- **EnumFields.xml** &ndash; Java `int` 상수와 C# `enums` 간의 매핑을 포함합니다. 

- **EnumMethods.xml** &ndash; 메서드 매개 변수와 반환 형식을 Java `int` 상수에서 C# `enums`로 변경할 수 있습니다. 

**MetaData.xml** 파일은 다음과 같이 일반적인 이유로 바인딩을 변경할 수 있으므로 이러한 파일 중 가장 중요합니다.

- .NET 규칙을 따르도록 네임스페이스, 클래스, 메서드 또는 필드 이름을 변경합니다. 

- 필요 없는 네임스페이스, 클래스, 메서드 또는 필드를 제거합니다. 

- 클래스를 다른 네임스페이스로 이동합니다. 

- 바인딩 디자인이 .NET 프레임워크 패턴을 따르도록 지원 클래스를 추가합니다. 

지금부터 **Metadata.xml**을 자세히 살펴보겠습니다.

## <a name="metadataxml-transform-file"></a>Metadata.xml 변환 파일

이미 배운 것처럼, **Metadata.xml** 파일은 바인딩 생성기에서 바인딩 어셈블리 생성에 영향을 주는 데 사용됩니다.
메타데이터 형식은 [XPath](https://www.w3.org/TR/xpath/) 구문을 사용하며 [GAPI 메타데이터](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata) 가이드에서 설명한 *GAPI 메타데이터*와 거의 동일합니다. 이 구현은 거의 완전한 XPath 1.0 구현이며, 따라서 1.0 표준의 항목을 지원합니다. 이 파일은 API 파일의 요소나 특성을 변경, 추가, 숨김 또는 이동하는 강력한 XPath 기반 메커니즘입니다. 메타데이터 사양의 모든 규칙 요소에는 규칙을 적용할 노드를 식별하는 경로 특성이 포함됩니다. 규칙은 다음 순서대로 적용됩니다.

- **add-node** &ndash; 경로 특성에 지정된 노드에 자식 노드를 추가합니다.
- **attr** &ndash; 경로 특성에 지정된 요소의 특성 값을 설정합니다.
- **remove-node** &ndash; 지정된 XPath와 일치하는 노드를 제거합니다.

다음은 **Metadata.xml** 파일의 예입니다.

```xml
<metadata>
    <!-- Normalize the namespace for .NET -->
    <attr path="/api/package[@name='com.evernote.android.job']" 
        name="managedName">Evernote.AndroidJob</attr>

    <!-- Don't  need these packages for the Xamarin binding/public API --> 
    <remove-node path="/api/package[@name='com.evernote.android.job.v14']" />
    <remove-node path="/api/package[@name='com.evernote.android.job.v21']" />

    <!-- Change a parameter name from the generic p0 to a more meaningful one. -->
    <attr path="/api/package[@name='com.evernote.android.job']/class[@name='JobManager']/method[@name='forceApi']/parameter[@name='p0']" 
        name="name">api</attr>
</metadata>
```

다음은 Java API에 가장 일반적으로 사용되는 XPath 요소 목록입니다.

- `interface` &ndash; Java 인터페이스를 찾는 데 사용됩니다. 예: `/interface[@name='AuthListener']`.

- `class` &ndash; 클래스를 찾는 데 사용됩니다. 예: `/class[@name='MapView']`.

- `method` &ndash; Java 클래스 또는 인터페이스에서 메서드를 찾는 데 사용됩니다. 예: `/class[@name='MapView']/method[@name='setTitleSource']`.

- `parameter` &ndash; 메서드의 매개 변수를 식별합니다. 예: `/parameter[@name='p0']`

### <a name="adding-types"></a>형식 추가

`add-node` 요소는 **api.xml**에 새 래퍼 클래스를 추가하도록 Xamarin.Android 바인딩 프로젝트에 지시합니다. 예를 들어 다음 코드 조각은 생성자와 단일 필드를 사용하여 클래스를 만들도록 바인딩 생성기에 지시합니다.

```xml
<add-node path="/api/package[@name='org.alljoyn.bus']">
    <class abstract="false" deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="true" visibility="public" extends="java.lang.Object">
        <constructor deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="false" type="org.alljoyn.bus.AuthListener.AuthRequest" visibility="public" />
        <field name="p0" type="org.alljoyn.bus.AuthListener.Credentials" />
    </class>
</add-node>
```

### <a name="removing-types"></a>형식 제거

Java 형식을 무시하고 바인딩하지 않도록 Xamarin.Android 바인딩 생성기에 지시할 수 있습니다. 이 작업은 `remove-node` XML 요소를 **metadata.xml** 파일에 추가하여 수행됩니다.

```xml
<remove-node path="/api/package[@name='{package_name}']/class[@name='{name}']" />
```

### <a name="renaming-members"></a>멤버 이름 바꾸기

Xamarin.Android는 원래 JNI(Java Native Interface) 이름이 필요하기 때문에 **api.xml** 파일을 직접 편집하여 멤버 이름을 바꿀 수 없습니다. 따라서 `//class/@name` 특성을 변경하면 안 됩니다. 변경할 경우 바인딩이 작동하지 않습니다.

`android.Manifest` 형식의 이름을 바꾸려 한다고 가정해 봅시다.
이 작업을 수행하기 위해, 다음과 같이 **api.xml** 파일을 직접 편집하여 클래스 이름을 바꾸려고 합니다.

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="name">NewName</attr>
```

그러면 바인딩 생성기가 래퍼 클래스에 대한 다음 C# 코드를 만듭니다.

```csharp
[Register ("android/NewName")]
public class NewName : Java.Lang.Object { ... }
```

래퍼 클래스의 이름이 `NewName`으로 바뀌었지만 원래 Java 형식은 여전히 `Manifest`입니다. 이제 Xamarin.Android 바인딩 클래스가 더 이상 `android.Manifest`의 메서드에 액세스할 수 없습니다. 래퍼 클래스는 존재하지 않는 Java 형식에 바인딩됩니다.

래핑된 형식(또는 메서드)의 관리되는 이름을 적절하게 변경하려면 다음 예제와 같이 `managedName` 특성을 설정해야 합니다.

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="managedName">NewName</attr>
```

<a name="Renaming_EventArg_Wrapper_Classes"></a>

#### <a name="renaming-eventarg-wrapper-classes"></a>`EventArg` 래퍼 클래스 이름 바꾸기

Xamarin.Android 바인딩 생성기가 _수신기 형식_의 `onXXX` setter 메서드를 발견하면 Java 기반 수신기 패턴에 .NET 종류의 API를 지원하기 위해 C# 이벤트 및 `EventArgs` 서브클래스가 생성됩니다. 예를 들어 다음과 같은 Java 클래스 및 메서드를 생각해 볼 수 있습니다.

```xml
com.someapp.android.mpa.guidance.NavigationManager.on2DSignNextManuever(NextManueverListener listener);
```

Xamarin.Android는 setter 메서드에서 `on` 접두사를 삭제하고, 그 대신 `2DSignNextManuever`를 `EventArgs` 서브클래스 이름의 기준으로 사용합니다. 서브클래스의 이름은 다음과 비슷합니다.

```csharp
NavigationManager.2DSignNextManueverEventArgs
```

이것은 올바른 C# 클래스 이름이 아닙니다. 이 문제를 해결하려면 바인딩 작성자가 다음과 같이 `argsType` 특성을 사용하여 `EventArgs` 서브클래스에 올바른 C# 이름을 제공해야 합니다.

```xml
<attr path="/api/package[@name='com.someapp.android.mpa.guidance']/
    interface[@name='NavigationManager.Listener']/
    method[@name='on2DSignNextManeuver']" 
    name="argsType">NavigationManager.TwoDSignNextManueverEventArgs</attr>
```

## <a name="supported-attributes"></a>지원되는 특성

다음 섹션에서는 Java API를 변환하는 데 사용되는 몇 가지 특성을 설명합니다.

### <a name="argstype"></a>argsType

이 특성은 Java 수신기를 지원하기 위해 생성되는 `EventArg` 서브클래스의 이름을 지정할 수 있도록 setter 메서드에 배치됩니다. 자세한 내용은 이 가이드의 뒷부분에 나오는 [EventArg 래퍼 클래스 이름 바꾸기](#Renaming_EventArg_Wrapper_Classes) 섹션에서 설명하겠습니다.

### <a name="eventname"></a>eventName

이벤트의 이름을 지정합니다. 비워 두면 이벤트를 생성할 수 없습니다.
자세한 내용은 [EventArg 래퍼 클래스 이름 바꾸기](#Renaming_EventArg_Wrapper_Classes) 섹션에서 설명하겠습니다.

### <a name="managedname"></a>managedName

패키지, 클래스, 메서드 또는 매개 변수의 이름을 변경하는 데 사용됩니다. 예를 들어 Java 클래스 이름 `MyClass`를 `NewClassName`으로 변경하는 방법은 다음과 같습니다.

```xml
<attr path="/api/package[@name='com.my.application']/class[@name='MyClass']" 
    name="managedName">NewClassName</attr>
```

다음 예제에서는 `java.lang.object.toString` 메서드를 `Java.Lang.Object.NewManagedName`으로 바꾸는 XPath 식을 보여줍니다.

```xml
<attr path="/api/package[@name='java.lang']/class[@name='Object']/method[@name='toString']" 
    name="managedName">NewMethodName</attr>
```

### <a name="managedtype"></a>managedType

`managedType`은 메서드의 반환 형식을 변경하는 데 사용됩니다. 바인딩 생성기가 Java 메서드의 반환 형식을 잘못 유추하여 컴파일 시간 오류가 발생하는 경우가 있습니다. 이 상황을 해결하는 한 가지 방법은 메서드의 반환 형식을 변경하는 것입니다.

예를 들어 바인딩 생성기는 Java 메서드 `de.neom.neoreadersdk.resolution.compareTo()`가 `int`를 반환하고 `Object`를 매개 변수로 사용해야 한다고 판단합니다. 그러면 **오류 CS0535: 'DE.Neom.Neoreadersdk.Resolution'이 인터페이스 멤버 'Java.Lang.IComparable.CompareTo(Java.Lang.Object)'를 구현하지 않습니다**라는 오류 메시지가 표시됩니다. 다음 코드 조각은 생성된 C# 메서드의 첫 번째 매개 변수 형식을 `DE.Neom.Neoreadersdk.Resolution`에서 `Java.Lang.Object`로 변경하는 방법을 보여줍니다. 

```xml
<attr path="/api/package[@name='de.neom.neoreadersdk']/
    class[@name='Resolution']/
    method[@name='compareTo' and count(parameter)=1 and
    parameter[1][@type='de.neom.neoreadersdk.Resolution']]/
    parameter[1]" name="managedType">Java.Lang.Object</attr> 
```

### <a name="managedreturn"></a>managedReturn

메서드의 반환 형식을 변경합니다. 반환 형식을 변경해도 반환 특성은 변경되지 않습니다(반환 특성을 변경하면 JNI 서명에 대해 호환되지 않는 변경이 발생할 수 있음). 다음 예제에서는 `append` 메서드의 반환 형식이 `SpannableStringBuilder`에서 `IAppendable`로 변경됩니다(C#은 공변 반환 형식을 지원하지 않음).

```xml
<attr path="/api/package[@name='android.text']/
    class[@name='SpannableStringBuilder']/
    method[@name='append']" 
    name="managedReturn">Java.Lang.IAppendable</attr>
```

### <a name="obfuscated"></a>obfuscated

Java 라이브러리를 난독 처리하는 도구는 Xamarin.Android 바인딩 생성기 및 C# 래퍼 클래스 생성 기능을 방해할 수 있습니다. obfuscated 클래스의 특징은 다음과 같습니다. 

- 클래스 이름에 **$** (예: **a$.class**)가 포함됩니다.
- 모든 클래스 이름이 소문자로 조정됩니다(예: **a.class**).

이 코드 조각은 "난독 처리되지 않은" C# 형식을 생성하는 방법의 예입니다.

```xml
<attr path="/api/package[@name='{package_name}']/class[@name='{name}']" 
    name="obfuscated">false</attr>
```

### <a name="propertyname"></a>propertyName

이 특성은 관리되는 속성의 이름을 변경하는 데 사용할 수 있습니다.

`propertyName`을 사용하는 특별한 사례로 Java 클래스에 필드의 getter 메서드만 있는 경우가 있습니다. 이 경우 바인딩 생성기는 .NET에서 권장하지 않는 쓰기 전용 속성을 만들려고 합니다. 다음 코드 조각은 `propertyName`을 빈 문자열로 설정하여 .NET 속성을 "제거"하는 방법을 보여줍니다.

```xml
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='setResourceDescriptor' 
    and count(parameter)=1 
    and parameter[1][@type='java.lang.String']]" 
    name="propertyName"></attr>
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='getResourceDescriptor' 
    and count(parameter)=0]" 
    name="propertyName"></attr>
```

setter 및 getter 메서드는 여전히 바인딩 생성기에 의해 생성됩니다.

### <a name="sender"></a>보낸 사람

메서드가 이벤트에 매핑될 때 `sender` 매개 변수로 사용할 메서드의 매개 변수를 지정합니다. 값은 `true`, `false`입니다. 예를 들어:

```xml
<attr path="/api/package[@name='android.app']/
    interface[@name='TimePickerDialog.OnTimeSetListener']/
    method[@name='onTimeSet']/
    parameter[@name='view']" 
    name="sender">true</ attr>
```

### <a name="visibility"></a>표시 여부

이 특성은 클래스, 메서드 또는 속성의 표시 여부를 변경하는 데 사용됩니다. 예를 들어 다음과 같이 해당 C# 래퍼가 `public`이 되도록 `protected` Java 메서드를 승격해야 하는 경우가 있습니다.

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method --> 
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

## <a name="enumfieldsxml-and-enummethodsxml"></a>EnumFields.xml 및 EnumMethods.xml

Android 라이브러리가 정수 상수를 사용하여 상태를 나타내고, 이 상태가 라이브러리의 속성 또는 메서드에 전달되는 경우가 있습니다. 대부분의 경우 C#에서 이러한 정수 상수를 열거형에 바인딩하는 것이 좋습니다. 이 매핑을 쉽게 하려면 바인딩 프로젝트에서 **EnumFields.xml** 및 **EnumMethods.xml** 파일을 사용합니다. 

### <a name="defining-an-enum-using-enumfieldsxml"></a>EnumFields.xml을 사용하여 열거형 정의

**EnumFields.xml** 파일에는 Java `int` 상수와 C# `enums` 간의 매핑이 포함되어 있습니다. `int` 상수 세트에 대해 생성되는 다음 C# 열거형 예제를 살펴보겠습니다. 

```xml 
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit">
    <field jni-name="UNIT_SECOND" clr-name="Second" value="0" />
    <field jni-name="UNIT_METER" clr-name="Meter" value="1" />
    <field jni-name="UNIT_MILIWATT_HOURS" clr-name="MilliwattHour" value="2" />
</mapping>
```

여기서는 Java 클래스 `SKRealReachSettings`를 사용하고 `Skobbler.Ngx.Map.RealReach` 네임스페이스에서 `SKMeasurementUnit`이라는 C# 열거형을 정의했습니다. `field` 항목은 Java 상수의 이름(예: `UNIT_SECOND`), 열거형 항목의 이름(예: `Second`) 및 두 엔터티로 나타내는 정수 값(예: `0`)을 정의합니다. 

### <a name="defining-gettersetter-methods-using-enummethodsxml"></a>EnumMethods.xml을 사용하여 Getter/Setter 메서드 정의

**EnumMethods.xml** 파일은 메서드 매개 변수와 반환 형식을 Java `int` 상수에서 C# `enums`로 변경할 수 있습니다. 즉, C# 열거형(**EnumFields.xml** 파일에 정의됨)의 읽기 및 쓰기를 Java `int` 상수 `get` 및 `set` 메서드에 매핑합니다.

위에서 정의한 `SKRealReachSettings` 열거형을 고려할 때, 다음 **EnumMethods.xml** 파일은 이 열거형에 대한 getter/setter 메서드를 정의합니다.

```xml
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings">
    <method jni-name="getMeasurementUnit" parameter="return" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
    <method jni-name="setMeasurementUnit" parameter="measurementUnit" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
</mapping>
```

첫 번째 `method` 줄은 Java `getMeasurementUnit` 메서드의 반환 값을 `SKMeasurementUnit` 열거형에 매핑합니다. 두 번째 `method` 줄은 `setMeasurementUnit`의 첫 번째 매개 변수를 동일한 열거형에 매핑합니다.

이러한 변경 내용을 모두 적용하면 Xamarin.Android에서 다음 코드를 사용하여 `MeasurementUnit`를 설정할 수 있습니다. 

```csharp
realReachSettings.MeasurementUnit = SKMeasurementUnit.Second;
```

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Android에서 메타데이터를 사용하여 *Google* *AOSP 형식*의 API 정의를 변환하는 방법을 설명했습니다. *Metadata.xml*을 사용하여 수행 가능한 변경 내용을 살펴본 후, 멤버 이름을 바꿀 때 발생하는 제한 사항을 알아보고 각 특성을 언제 사용해야 하는지 설명하는 지원되는 XML 특성 목록을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [JNI 작업](~/android/platform/java-integration/working-with-jni.md)
- [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)
- [GAPI 메타데이터](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
