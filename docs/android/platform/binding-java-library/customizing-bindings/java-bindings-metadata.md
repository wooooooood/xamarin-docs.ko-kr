---
title: Java 바인딩 메타 데이터
description: C#xamarin.android에서 코드에서 Java 기본 인터페이스 (JNI)를 지정 된 하위 수준 세부 정보를 추상화 하는 메커니즘에는 바인딩을 통해 Java 라이브러리를 호출 합니다. Xamarin.Android는 이러한 바인딩을 생성 하는 도구를 제공 합니다. 이 도구를 사용 하면 네임 스페이스를 수정 하 고 멤버 이름 바꾸기와 같은 절차를 허용 하는 메타 데이터를 사용 하 여 바인딩을 만들어지는 방법을 개발자 제어가 있습니다. 이 문서는 메타 데이터의 작동 원리에 대해 설명, 해당 메타 데이터 특성을 요약 한 것이 메타 데이터를 수정 하 여 바인딩 문제를 해결 하는 방법에 설명 하 고, 지원 합니다.
ms.prod: xamarin
ms.assetid: 27CB3C16-33F3-F580-E2C0-968005A7E02E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 858f1e5c0bd2af85b419bb9a1cffb7d484f3f7e4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113406"
---
# <a name="java-bindings-metadata"></a>Java 바인딩 메타 데이터

_C#xamarin.android에서 코드에서 Java 기본 인터페이스 (JNI)를 지정 된 하위 수준 세부 정보를 추상화 하는 메커니즘에는 바인딩을 통해 Java 라이브러리를 호출 합니다. Xamarin.Android는 이러한 바인딩을 생성 하는 도구를 제공 합니다. 이 도구를 사용 하면 네임 스페이스를 수정 하 고 멤버 이름 바꾸기와 같은 절차를 허용 하는 메타 데이터를 사용 하 여 바인딩을 만들어지는 방법을 개발자 제어가 있습니다. 이 문서는 메타 데이터의 작동 원리에 대해 설명, 해당 메타 데이터 특성을 요약 한 것이 메타 데이터를 수정 하 여 바인딩 문제를 해결 하는 방법에 설명 하 고, 지원 합니다._


## <a name="overview"></a>개요

Xamarin.Android **Java 바인딩 라이브러리** 라고도 하는 도구를 활용 하 여 기존 Android 라이브러리를 바인딩하는 데 필요한 작업의 대부분을 자동화 하려고 합니다 _바인딩 생성기_합니다. Xamarin.Android는 Java 클래스를 검사 하 고 모든 패키지, 형식 및 멤버의 목록을 생성할지 Java 라이브러리를 바인딩하는 경우를 바인딩할 수 있습니다. 이 목록이 Api에 있는 XML 파일에 저장 됩니다  **\{directory}\obj\Release\api.xml 프로젝트** 에 대 한를 **릴리스** 빌드 타임과  **\{프로젝트 directory}\obj\Debug\api.xml** 에 대 한는 **디버그** 작성 합니다.

![Obj/Debug 폴더를 api.xml 파일의 위치](java-bindings-metadata-images/java-bindings-metadata-01.png)

바인딩 생성기를 사용 합니다 **api.xml** 필요한을 생성 하기 위한 지침으로 파일 C# 래퍼 클래스입니다. 이 XML 파일의 내용이 Google의 변형을 _Android Open Source Project_ 형식입니다.
다음 코드 조각은의 내용에 대 한 예로 **api.xml**:

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

이 예에서 **api.xml** 에서 클래스를 선언 합니다 `android` 라는 패키지 `Manifest` 확장 하는 `java.lang.Object`합니다.

대부분의 경우에서 휴먼 지원은 자세한 "와 같은.NET" 생각 하는 Java API 하거나 바인딩 어셈블리의 컴파일을 방해 하는 문제를 해결 하는 데 필요 합니다. 예를 들어.NET 네임 스페이스에 Java 패키지 이름 변경, 클래스 이름 바꾸기 또는 메서드의 반환 형식을 변경 해야 할 수도 있습니다.

이러한 변경 내용을 수정 하 여 못한 됩니다 **api.xml** 직접.
대신, 변경 내용은 Java 바인딩 라이브러리 템플릿에서 제공 되는 특별 한 XML 파일에 기록 됩니다. Xamarin.Android 바인딩 어셈블리를 컴파일할 때 바인딩 생성기를 받을 수 이러한 매핑 파일에서 바인딩 어셈블리를 만들 때

이러한 XML 매핑 파일에서 찾을 수 있습니다 합니다 **변환** 프로젝트의 폴더:

-   **MetaData.xml** &ndash; 최종 api에 생성 된 바인딩의 네임 스페이스를 변경 하는 등의 변경을 허용 합니다. 

-   **EnumFields.xml** &ndash; Java 간의 매핑을 포함 `int` 상수 및 C# `enums` 합니다. 

-   **EnumMethods.xml** &ndash; Java에서 메서드 매개 변수 및 반환 형식 변경 허용 `int` 상수를 C# `enums` 합니다. 

합니다 **MetaData.xml** 파일은 이러한 파일의 가져오기 가장 범용 같은 바인딩 변경이 허용:

-   .NET 규칙을 따르는 있도록 네임 스페이스, 클래스, 메서드 또는 필드 이름을 바꿉니다. 

-   네임 스페이스, 클래스, 메서드 또는 필요 하지 않은 필드를 제거 합니다. 

-   다른 네임 스페이스에 클래스를 이동 합니다. 

-   바인딩의 디자인을 추가 지원 클래스를 추가.NET framework 패턴을 따릅니다. 

설명으로 이동할 수 있습니다 **Metadata.xml** 자세히입니다.


## <a name="metadataxml-transform-file"></a>Metadata.xml 변환 파일

이미 배웠습니다, 파일 처럼 **Metadata.xml** 바인딩 생성기에서 바인딩 어셈블리 생성에 영향을 하는 데 사용 됩니다.
메타 데이터 형식은 사용 하 여 [XPath](https://www.w3.org/TR/xpath/) 구문와 거의 동일한 합니다 *GAPI 메타 데이터* 에 설명 된 [GAPI 메타 데이터](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata) 가이드입니다. 이 구현 거의 완전히 구현 하는 XPath 1.0 이며 따라서 1.0 표준에 항목을 지원 합니다. 이 파일은 변경, 추가, 숨기기 또는 API 파일에서 임의의 요소 또는 특성을 이동 하는 강력한 XPath 기반 메커니즘. 모든 메타 데이터 사양에 규칙 요소는 규칙을 적용 하는 노드를 식별 경로 특성을 포함 합니다. 규칙은 다음 순서 대로 적용 됩니다.

* **노드 추가** &ndash; 경로 특성에 지정 된 자식 노드를 추가 합니다.
* **attr** &ndash; 경로 특성에 지정 된 요소의 특성의 값을 설정 합니다.
* **노드 제거** &ndash; XPath를 지정된 하는 노드를 제거 합니다.

다음은의 예는 **Metadata.xml** 파일:

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

다음 목록은 몇 가지 자주 사용 되는 XPath 요소는 Java API에 대 한.

-   `interface` &ndash; Java 인터페이스를 찾는 데 사용. 예를 들어 `/interface[@name='AuthListener']`합니다.

-   `class` &ndash; 클래스를 찾는 데 사용 합니다. 예를 들어 `/class[@name='MapView']`합니다.

-   `method` &ndash; Java 클래스 또는 인터페이스에서 메서드를 찾는 데 사용 합니다. 예를 들어 `/class[@name='MapView']/method[@name='setTitleSource']`합니다.

-   `parameter` &ndash; 메서드에 대 한 매개 변수를 식별 합니다. 예: `/parameter[@name='p0']`



### <a name="adding-types"></a>추가 형식

합니다 `add-node` 요소를 새 래퍼 클래스를 추가 하려면 Xamarin.Android 바인딩 프로젝트 알려 **api.xml**합니다. 예를 들어, 다음 코드 조각은 클래스 생성자 및 단일 필드를 사용 하 여 만들려는 바인딩 생성기 안내 합니다.

```xml
<add-node path="/api/package[@name='org.alljoyn.bus']">
    <class abstract="false" deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="true" visibility="public" extends="java.lang.Object">
        <constructor deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="false" type="org.alljoyn.bus.AuthListener.AuthRequest" visibility="public" />
        <field name="p0" type="org.alljoyn.bus.AuthListener.Credentials" />
    </class>
</add-node>
```


### <a name="removing-types"></a>형식 제거

Xamarin.Android 바인딩 생성기는 Java 형식을 무시 하 고 해당 바인딩하지를 지시 하는 것이 가능 합니다. 추가 하 여 이렇게를 `remove-node` XML 요소를 **metadata.xml** 파일:

```xml
<remove-node path="/api/package[@name='{package_name}']/class[@name='{name}']" />
```

### <a name="renaming-members"></a>멤버 이름 바꾸기

멤버 이름 바꾸기 직접 편집 하 여 수행할 수 없습니다는 **api.xml** Xamarin.Android에는 원래 Java 기본 인터페이스 (JNI) 이름이 필요로 하기 때문에 파일입니다. 따라서는 `//class/@name` 특성을 변경할 수 없습니다; 인 경우 바인딩이 작동 하지 것입니다.

형식, 이름 바꾸기 하려는 경우를 생각해 보겠습니다 `android.Manifest`합니다.
이를 위해 수 하려고 노력을 직접 편집할 **api.xml** 같이 클래스 이름 바꾸기 및:

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="name">NewName</attr>
```

그러면 다음 만들 바인딩 생성기 C# 래퍼 클래스에 대 한 코드:

```csharp
[Register ("android/NewName")]
public class NewName : Java.Lang.Object { ... }
```

래퍼 클래스를 바뀌었습니다 되었다는 `NewName`원래 Java 형식이 계속 되는 반면 `Manifest`합니다. Xamarin.Android 바인딩 클래스에 메서드를 액세스 하려면 더 이상 불가능 `android.Manifest`; 래퍼 클래스는 존재 하지 않는 Java 형식에 바인딩되어 없습니다.

래핑된 형식 (또는 메서드)의 관리 되는 이름을 올바르게 변경 하려면 설정 하는 데 필요한 것은 `managedName` 이 예와 같이 특성:

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="managedName">NewName</attr>
```

<a name="Renaming_EventArg_Wrapper_Classes" />

#### <a name="renaming-eventarg-wrapper-classes"></a>이름 바꾸기 `EventArg` 래퍼 클래스

Xamarin.Android 바인딩 생성기를 식별 하는 경우는 `onXXX` 에 대 한 setter 메서드를 _수신기 형식_, C# 이벤트 및 `EventArgs` 서브 클래스를 생성할.NET 지원 하기 위해 Java 기반 API를 flavoured 수신기 패턴입니다. 예를 들어 다음 Java 클래스 및 메서드를 고려 합니다.

```xml
com.someapp.android.mpa.guidance.NavigationManager.on2DSignNextManuever(NextManueverListener listener);
```

Xamarin.Android는 접두사를 삭제 `on` setter 메서드에서 대신 사용 하 여 `2DSignNextManuever` 의 이름으로는 `EventArgs` 하위 클래스입니다. 서브 클래스 이름은 비슷합니다.

```csharp
NavigationManager.2DSignNextManueverEventArgs
```

이 올바른 C# 클래스 이름입니다. 이 문제를 해결 하려면 바인딩 작성자 사용 해야 합니다는 `argsType` 특성 및 유효한 제공 C# 에 대 한 이름를 `EventArgs` 서브 클래스:
 
```xml
<attr path="/api/package[@name='com.someapp.android.mpa.guidance']/
    interface[@name='NavigationManager.Listener']/
    method[@name='on2DSignNextManeuver']" 
    name="argsType">NavigationManager.TwoDSignNextManueverEventArgs</attr>
```

 

## <a name="supported-attributes"></a>지원 되는 특성

다음 섹션에서는 Java Api를 변환 하는 것에 대 한 특성 중 일부를 설명 합니다.

### <a name="argstype"></a>argsType

이 특성이 setter 메서드 이름을 배치 되는 `EventArg` Java 수신기를 지원 하기 위해 생성 되는 하위 클래스입니다. 이 섹션에서 아래 자세히 설명 되어 [EventArg 래퍼 클래스 이름 바꾸기](#Renaming_EventArg_Wrapper_Classes) 나중에이 가이드에서.

### <a name="eventname"></a>eventName

이벤트에 대 한 이름을 지정합니다. 비어 있는 경우 이벤트 생성을 금지 합니다.
이 섹션 제목에서 자세히 설명 되어 [EventArg 래퍼 클래스 이름 바꾸기](#Renaming_EventArg_Wrapper_Classes)합니다.

### <a name="managedname"></a>managedName

패키지, 클래스, 메서드 또는 매개 변수의 이름을 변경 하려면 사용 됩니다. 예를 들어 Java 클래스의 이름을 바꾸려면 `MyClass` 에 `NewClassName`:

```xml
<attr path="/api/package[@name='com.my.application']/class[@name='MyClass']" 
    name="managedName">NewClassName</attr>
```

다음 예제에서는 메서드 이름 바꾸기에 대 한 XPath 식을 보여 줍니다 `java.lang.object.toString` 에 `Java.Lang.Object.NewManagedName`:

```xml
<attr path="/api/package[@name='java.lang']/class[@name='Object']/method[@name='toString']" 
    name="managedName">NewMethodName</attr>
```

### <a name="managedtype"></a>managedType

`managedType` 메서드의 반환 형식을 변경 하는 데 사용 됩니다. 일부 상황에서 바인딩 생성기 올바르게 유추 하지는 컴파일 시간 오류가 발생에서 하면 Java 메서드의 반환 형식. 이 경우 한 가지 해결책은 반환 형식의 메서드를 변경 하기 위해서입니다.

바인딩 생성기는가 예를 들어 다음 Java 메서드의 경우 `de.neom.neoreadersdk.resolution.compareTo()` 반환 해야 합니다는 `int`, 오류 메시지의 결과 **오류 CS0535: ' DE 합니다. Neom.Neoreadersdk.Resolution' 인터페이스 멤버 'Java.Lang.IComparable.CompareTo(Java.Lang.Object)' 구현 하지 않습니다**합니다. 다음 코드 조각에는 생성 된 반환 형식을 변경 하는 방법을 보여 줍니다. C# 메서드에서 `int` 에 `Java.Lang.Object`: 

```xml
<attr path="/api/package[@name='de.neom.neoreadersdk']/
    class[@name='Resolution']/
    method[@name='compareTo' and count(parameter)=1 and
    parameter[1][@type='de.neom.neoreadersdk.Resolution']]/
    parameter[1]"name="managedType">Java.Lang.Object</attr> 
```

### <a name="managedreturn"></a>managedReturn

메서드의 반환 형식을 변경합니다. 반환 특성 (특성을 JNI 서명과 호환 되지 않는 변경 될 수 있습니다 반환할 변경)에 따라 변경 되지 않습니다. 다음 예에서 반환 형식이 합니다 `append` 메서드가에서 변경 될 `SpannableStringBuilder` 하 `IAppendable` (이전에 설명한 대로 C# 공변 (covariant) 반환 형식을 지원 하지 않습니다):

```xml
<attr path="/api/package[@name='android.text']/
    class[@name='SpannableStringBuilder']/
    method[@name='append']" 
    name="managedReturn">Java.Lang.IAppendable</attr>
```

### <a name="obfuscated"></a>난독 처리

Java 라이브러리를 난독 처리 하는 도구 Xamarin.Android 바인딩 생성기를 생성 하는 기능을 방해할 수 C# 래퍼 클래스입니다. 난독 처리 된 클래스의 특성 포함: * 클래스 이름에는 **$**, 즉 **$.class** * 클래스 이름을 완전히 손상 된 소문자 문자 즉,  **a.class**

이 코드 조각을 생성 하는 방법의 예로 "난독 처리 되지 않은"는 C# 형식:

```xml
<attr path="/api/package[@name='{package_name}']/class[@name='{name}']" 
    name="obfuscated">false</attr>
```

### <a name="propertyname"></a>propertyName

관리 되는 속성의 이름을 변경 하려면이 특성을 사용할 수 있습니다.

사용 하는 특수 한 경우 `propertyName` Java 클래스의 필드에 대 한 getter 메서드만 있는 상황을 포함 합니다. 이 경우 바인딩 생성기는.NET에서 권장 되지 않는 쓰기 전용 속성을 만들 하려고 합니다. 다음 코드 조각은.NET 속성을 설정 하 여 "제거" 하는 방법을 보여 줍니다는 `propertyName` 빈 문자열:

```xml
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='setResourceDescriptor' 
    and count(parameter)=1 
    and parameter[1][@type='java.lang.String']]" 
    name="propertyName"></attr>
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='getResourceDescriptor' 
    and count(parameter)=0]" 
    name="propertyName"></attr>
```

참고 setter 및 getter 메서드를 여전히 바인딩 생성기에서 만들 수 있습니다.

### <a name="sender"></a>sender

메서드의 매개 변수 지정은 `sender` 메서드 이벤트에 매핑된 경우 매개 변수입니다. 값은 `true` 또는 `false`합니다. 예를 들어:

```xml
<attr path="/api/package[@name='android.app']/
    interface[@name='TimePickerDialog.OnTimeSetListener']/
    method[@name='onTimeSet']/
    parameter[@name='view']" 
    name="sender">true</ attr>
```

### <a name="visibility"></a>표시 여부

이 특성은 클래스, 메서드 또는 속성의 표시 유형을 변경 하려면 사용 합니다. 예를 들어, 승격 하는 일을 할 수 있습니다는 `protected` Java 메서드는 해당 하는 C# 래퍼가 `public`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method --> 
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

## <a name="enumfieldsxml-and-enummethodsxml"></a>EnumFields.xml 및 EnumMethods.xml

Android 라이브러리를 라이브러리의 메서드나 속성에 전달 되는 상태를 나타내는 정수 상수를 사용 하는 경우가 있습니다. 대부분의 경우에 유용에서 열거형이 정수 상수를 바인딩할 C#입니다. 이 매핑은 용이 하 게 사용 합니다 **EnumFields.xml** 하 고 **EnumMethods.xml** 바인딩 프로젝트의 파일. 

### <a name="defining-an-enum-using-enumfieldsxml"></a>EnumFields.xml를 사용 하 여 열거형 정의

합니다 **EnumFields.xml** Java 사이의 매핑을 포함 하는 파일 `int` 상수 및 C# `enums`합니다. 다음 예제를 살펴보겠습니다.는 C# 열거형 집합에 대해 만들어지는 `int` 상수: 

```xml 
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit">
    <field jni-name="UNIT_SECOND" clr-name="Second" value="0" />
    <field jni-name="UNIT_METER" clr-name="Meter" value="1" />
    <field jni-name="UNIT_MILIWATT_HOURS" clr-name="MilliwattHour" value="2" />
</mapping>
```

Java 클래스 이동 여기 `SKRealReachSettings` 정의 C# 라는 열거형 `SKMeasurementUnit` 네임 스페이스에서 `Skobbler.Ngx.Map.RealReach`. `field` 항목의 Java 상수 이름을 정의 (예제 `UNIT_SECOND`), 열거형 항목의 이름 (예제 `Second`), 두 엔터티를 나타내는 정수 값 (예제 `0`). 

### <a name="defining-gettersetter-methods-using-enummethodsxml"></a>EnumMethods.xml를 사용 하 여 Getter/Setter 메서드를 정의 합니다.

합니다 **EnumMethods.xml** 파일을 통해 Java에서 메서드 매개 변수 및 반환 형식 변경 `int` 상수를 C# `enums`합니다. 즉, 읽기 및 쓰기를 매핑합니다 C# 열거형 (에 정의 된는 **EnumFields.xml** 파일)에서 Java `int` 상수 `get` 하 고 `set` 메서드.

지정 된 합니다 `SKRealReachSettings` 다음 위에 정의 된 열거형 **EnumMethods.xml** 파일에서이 열거형에 대 한 getter/setter를 정의 합니다.

```xml
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings">
    <method jni-name="getMeasurementUnit" parameter="return" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
    <method jni-name="setMeasurementUnit" parameter="measurementUnit" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
</mapping>
```

첫 번째 `method` 줄 Java의 반환 값을 매핑합니다 `getMeasurementUnit` 방법의 `SKMeasurementUnit` 열거형입니다. 두 번째 `method` 줄의 첫 번째 매개 변수를 매핑하는 `setMeasurementUnit` 동일한 열거형에 있습니다.

모든 위치에서 이러한 변경 내용을 사용 하 여 사용할 수에 따라 코드 Xamarin.Android에서 설정 하 여 `MeasurementUnit`: 

```csharp
realReachSettings.MeasurementUnit = SKMeasurementUnit.Second;
```


## <a name="summary"></a>요약

이 문서에서는 Xamarin.Android에서 API 정의 변환 하려면 메타 데이터를 사용 하는 방법을 설명 합니다 *Google* *AOSP 형식*합니다. 사용 하 여 사용할 수 있는 변경 내용을 살펴보았으므로 *Metadata.xml*, 조사 하 고 멤버의 이름을 바꿀 때 발생 한 제한 사항 및 각 특성을 사용 해야 하는 경우를 설명 하는 지원 되는 XML 특성 목록을 제공 합니다.



## <a name="related-links"></a>관련 링크

- [Jni 작업](~/android/platform/java-integration/working-with-jni.md)
- [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)
- [GAPI 메타 데이터](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
