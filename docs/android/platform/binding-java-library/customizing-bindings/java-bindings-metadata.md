---
title: "Java 바인딩 메타 데이터"
description: "Xamarin.Android에서 C# 코드에서 Java 기본 인터페이스 (JNI)를 지정 된 하위 수준 세부 정보를 추상화 하는 메커니즘은 바인딩을 통해 Java 라이브러리를 호출 합니다. Xamarin.Android 이러한 바인딩을 생성 하는 도구를 제공 합니다. 이 도구를 사용 하면 개발자의 제어를 사용 하면 프로시저가 네임 스페이스를 수정 및 멤버 이름 바꾸기와 같은 메타 데이터를 사용 하 여 바인딩을 만들어지는 방법이 있습니다. 이 문서에서는 메타 데이터의 작동 원리, 특성 메타 데이터를 요약, 지원 및이 메타 데이터를 수정 하 여 바인딩 문제를 해결 하는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 27CB3C16-33F3-F580-E2C0-968005A7E02E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: edf25ebd089994c01b2fa45e77b35fad9a51e350
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="java-bindings-metadata"></a>Java 바인딩 메타 데이터

_Xamarin.Android에서 C# 코드에서 Java 기본 인터페이스 (JNI)를 지정 된 하위 수준 세부 정보를 추상화 하는 메커니즘은 바인딩을 통해 Java 라이브러리를 호출 합니다. Xamarin.Android 이러한 바인딩을 생성 하는 도구를 제공 합니다. 이 도구를 사용 하면 개발자의 제어를 사용 하면 프로시저가 네임 스페이스를 수정 및 멤버 이름 바꾸기와 같은 메타 데이터를 사용 하 여 바인딩을 만들어지는 방법이 있습니다. 이 문서에서는 메타 데이터의 작동 원리, 특성 메타 데이터를 요약, 지원 및이 메타 데이터를 수정 하 여 바인딩 문제를 해결 하는 방법을 설명 합니다._


## <a name="overview"></a>개요

Xamarin.Android **Java 바인딩 라이브러리** 라고도 하는 도구를 사용 하 여 기존 Android 라이브러리에 바인딩하는 데 필요한 작업의 대부분을 자동화 하려고는 _바인딩 생성기_합니다. Xamarin.Android Java 클래스를 검사 하 고 패키지, 형식 및 멤버의 목록을 생성 하는 Java 라이브러리를 바인딩하는 경우를 바인딩할 수 있습니다. 이 목록은 Api에서 찾을 수 있는 XML 파일에 저장 됩니다  **\{directory}\obj\Release\api.xml 프로젝트** 에 대 한는 **릴리스** 빌드 및  **\{프로젝트 directory}\obj\Debug\api.xml** 에 대 한는 **디버그** 작성 합니다.

![Obj/Debug 폴더에서 api.xml 파일의 위치](java-bindings-metadata-images/java-bindings-metadata-01.png)

바인딩 생성기 ´ ֲ는 **api.xml** 필요한 C# 래퍼 클래스를 생성 하기 위한 지침으로 파일입니다. 이 XML 파일의 내용이 Google의 변형 _Android 소스 프로젝트 열기_ 형식입니다.
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

이 예제에서는 **api.xml** 선언에서 클래스는 `android` 라는 패키지 `Manifest` 확장 하는 `java.lang.Object`합니다.

대부분의 경우 휴먼 지원이 더 많은 "와 같은.NET" 생각 Java API 또는 바인딩 어셈블리의 컴파일을 방지 하는 문제를 해결 해야 합니다. 예를 들어 Java 패키지 이름을.NET 네임 스페이스 변경, 클래스, 이름 바꾸기 또는 메서드의 반환 형식을 변경 해야 할 수도 있습니다.

이러한 변경 내용의 수정 하 여 높일 되지 **api.xml** 직접 합니다.
대신, 변경 내용에서 Java 바인딩 라이브러리 템플릿이 제공 되는 특수 한 XML 파일에 기록 됩니다. Xamarin.Android 바인딩 어셈블리를 컴파일할 때 바인딩 생성기 영향을 받을 수 이러한 매핑 파일 바인딩 어셈블리를 만들 때

이러한 XML 매핑 파일에서 찾을 수 있습니다는 **변환** 프로젝트의 폴더:

-   **MetaData.xml** &ndash; 변경 내용을 생성 된 바인딩 네임 스페이스를 변경 하는 등 최종 API를 만들 수 있습니다. 

-   **EnumFields.xml** &ndash; Java 간의 매핑을 포함 `int` 상수 및 C# `enums` 합니다. 

-   **EnumMethods.xml** &ndash; Java에서 메서드 매개 변수 및 반환 형식을 변경 하는 허용 `int` C# 상수 `enums` 합니다. 

**MetaData.xml** 파일은 이러한 파일의 가져오기를 가장과 같은 바인딩 범용 변경이 허용:

-   네임 스페이스, 클래스, 메서드 또는 필드 이름이 변경 되므로.NET 규칙을 따릅니다. 

-   네임 스페이스, 클래스, 메서드 또는 필요 하지 않는 필드를 제거 합니다. 

-   서로 다른 네임 스페이스에 클래스를 이동합니다. 

-   바인딩의 디자인 해야 추가 지원 클래스는.NET framework 패턴을 따릅니다. 

논의로 이동할 수 있습니다. **Metadata.xml** 자세히 합니다.


## <a name="metadataxml-transform-file"></a>Metadata.xml 변환 파일

이미 학습 한, 파일 처럼 **Metadata.xml** 바인딩 어셈블리의 생성에 영향을 줍니다 바인딩 생성기에서 사용 됩니다.
메타 데이터 형식은 사용 하 여 [XPath](https://www.w3.org/TR/xpath/) 구문와 거의 동일 하 고는 *GAPI 메타 데이터* 에 설명 된 [GAPI 메타 데이터](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata) 가이드입니다. 이 구현은 XPath 1.0 거의 완전히 구현 하며 따라서 표준 1.0에서 항목을 지원 합니다. 이 파일은 변경, 추가, 숨기기 또는 API 파일에서 임의의 요소 또는 특성을 이동할 수 있는 강력한 XPath 기반 메커니즘. 모든 메타 데이터 사양에서 규칙 요소 규칙을 적용할 노드를 식별 하는 경로 특성이 포함 합니다. 규칙은 다음과 같은 순서로 적용 됩니다.

* **노드 추가** &ndash; 경로 특성에 지정 된 자식 노드를 추가 합니다.
* **attr** &ndash; 경로 특성에 지정 된 요소의 특성의 값을 설정 합니다.
* **노드 제거** &ndash; 지정한 XPath는 노드를 제거 합니다.

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

다음과 같은 몇 가지 자주 사용 되는 XPath 요소 Java API에 대 한:

-   `interface` &ndash; Java 인터페이스를 찾는 데 사용 합니다. 예를 들어 `/interface[@name='AuthListener']`합니다.

-   `class` &ndash; 사용 하는 클래스를 찾습니다. 예를 들어 `/class[@name='MapView']`합니다.

-   `method` &ndash; Java 클래스 또는 인터페이스에 대 한 메서드를 찾는 데 사용 합니다. 예를 들어 `/class[@name='MapView']/method[@name='setTitleSource']`합니다.

-   `parameter` &ndash; 메서드에 대 한 매개 변수를 식별 합니다. 예: `/parameter[@name='p0']`



### <a name="adding-types"></a>형식 추가

`add-node` 요소 쿼리 하는 새 래퍼 클래스를 추가 하기 위해 Xamarin.Android 바인딩 프로젝트를 확인할 **api.xml**합니다. 예를 들어 다음 코드 조각은 바인딩 생성기는 생성자와 단일 필드는 클래스를 만드는 데 직접 됩니다.

```xml
<add-node path="/api/package[@name='org.alljoyn.bus']">
    <class abstract="false" deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="true" visibility="public" extends="java.lang.Object">
        <constructor deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="false" type="org.alljoyn.bus.AuthListener.AuthRequest" visibility="public" />
        <field name="p0" type="org.alljoyn.bus.AuthListener.Credentials" />
    </class>
</add-node>
```


### <a name="removing-types"></a>형식 제거

Xamarin.Android 바인딩 생성기 Java 유형을 무시 하 고 바인딩할 없습니다를 지시 하는 것이 불가능 합니다. 추가 하 여 이렇게는 `remove-node` XML 요소는 **metadata.xml** 파일:

```xml
<remove-node path="/api/package[@name='{package_name}']/class[@name='{name}']" />
```

### <a name="renaming-members"></a>멤버 이름 바꾸기

멤버 이름 바꾸기을 직접 편집 하 여 수행할 수 없습니다는 **api.xml** Xamarin.Android 원래 Java 기본 인터페이스 (JNI) 이름이 필요 하기 때문에 파일입니다. 따라서는 `//class/@name` 특성을 변경할 수 없습니다; 바인딩이 인 경우 작동 하지 것입니다.

유형의 이름을 변경 하려는 경우를 살펴봅시다 `android.Manifest`합니다.
이를 위해 직접 편집 하려고 수 **api.xml** 같이 클래스 이름 바꾸기 및:

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="name">NewName</attr>
```

이 래퍼 클래스에 대 한 다음 C# 코드를 작성 하는 바인딩 생성기 발생 합니다.

```csharp
[Register ("android/NewName")]
public class NewName : Java.Lang.Object { ... }
```

래퍼 클래스를 바꾼 공지 `NewName`원래 Java 형식은 여전히 `Manifest`합니다. 더 이상 Xamarin.Android 바인딩 클래스에는 메서드를 액세스 하려면 `android.Manifest`; 하면 래퍼 클래스가 존재 하지 않는 Java 형식에 바인딩되어 없습니다.

래핑된 형식 (또는 메서드)의 관리 되는 이름을 올바르게 변경을 설정 하는 데 필요한는 `managedName` 이 예제에 표시 된 대로 특성:

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="managedName">NewName</attr>
```

<a name="Renaming_EventArg_Wrapper_Classes" />

#### <a name="renaming-eventarg-wrapper-classes"></a>이름 바꾸기 `EventArg` 래퍼 클래스

Xamarin.Android 바인딩 생성기 식별 하는 경우는 `onXXX` 에 대 한 setter 메서드는 _수신기 형식_, C# 이벤트 및 `EventArgs` 하위 클래스를 생성 됩니다는.NET 지원 하기 위해 Java 기반 수신기에 대 한 API flavoured 패턴입니다. 예를 들어 다음 Java 클래스와 메서드가 고려 합니다.

```xml
com.someapp.android.mpa.guidance.NavigationManager.on2DSignNextManuever(NextManueverListener listener);
```

Xamarin.Android 접두사 짧아집니다 `on` setter 메서드에서 대신 사용 `2DSignNextManuever` 의 이름에 대 한 기준으로는 `EventArgs` 하위 클래스입니다. 하위 클래스 이름 유사한 항목:

```csharp
NavigationManager.2DSignNextManueverEventArgs
```

이것이 올바른 C# 클래스 이름입니다. 이 문제를 해결 하려면 바인딩 작성자가 사용 해야 합니다는 `argsType` 특성에 대 한 올바른 C# 이름을 지정 하 고는 `EventArgs` 서브 클래스:
 
```xml
<attr path="/api/package[@name='com.someapp.android.mpa.guidance']/
    interface[@name='NavigationManager.Listener']/
    method[@name='on2DSignNextManeuver']" 
    name="argsType">NavigationManager.TwoDSignNextManueverEventArgs</attr>
```

 

## <a name="supported-attributes"></a>지원 되는 특성

다음 섹션에서는 Java Api를 변환 하기 위한 특성 중 일부에 대해 설명 합니다.

### <a name="argstype"></a>argsType

이 특성이 이름을 지정 하는 setter 메서드에 배치 되는 `EventArg` Java 수신기를 지원 하기 위해 생성 되는 하위 클래스입니다. 이 섹션에서 아래 자세히 설명 되어 [EventArg 래퍼 클래스 이름 바꾸기](#Renaming_EventArg_Wrapper_Classes) 이 가이드에서 나중에 있습니다.

### <a name="eventname"></a>eventName

이벤트에 대 한 이름을 지정합니다. 비어 있는 경우 이벤트 생성을 금지 합니다.
섹션 제목에서 자세히 설명 되어이 [EventArg 래퍼 클래스 이름 바꾸기](#Renaming_EventArg_Wrapper_Classes)합니다.

### <a name="managedname"></a>managedName

이 패키지, 클래스, 메서드, 또는 매개 변수 이름을 변경 하려면 사용 됩니다. 예를 들어 Java 클래스의 이름을 변경 하려면 `MyClass` 를 `NewClassName`:

```xml
<attr path="/api/package[@name='com.my.application']/class[@name='MyClass']" 
    name="managedName">NewClassName</attr>
```

다음 예제에서는 메서드의 이름을 대 한 XPath 식을 `java.lang.object.toString` 를 `Java.Lang.Object.NewManagedName`:

```xml
<attr path="/api/package[@name='java.lang']/class[@name='Object']/method[@name='toString']" 
    name="managedName">NewMethodName</attr>
```

### <a name="managedtype"></a>managedType

`managedType` 메서드의 반환 형식을 변경 하는 데 사용 됩니다. 상황에 따라 바인딩을 생성기 올바르게 유추 합니다 하지는 컴파일 타임 오류를 초래할 Java 메서드의 반환 형식입니다. 이 상황에서 가능한 한 가지 솔루션은 반환 형식의 메서드를 변경 하는 것입니다.

예를 들어 바인딩 생성기가 있다고 인식 하는 Java 메서드 `de.neom.neoreadersdk.resolution.compareTo()` 반환할지 여부는 `int`, 오류 메시지에 줄어들고 결과적 **오류 CS0535: ' DE 합니다. Neom.Neoreadersdk.Resolution' 'Java.Lang.IComparable.CompareTo(Java.Lang.Object)' 인터페이스 멤버를 구현 하지 않는**합니다. 다음 코드 조각에서는 생성 된 C#에서 메서드의 반환 형식을 변경 하는 방법을 보여 줍니다.는 `int` 에 `Java.Lang.Object`: 

```xml
<attr path="/api/package[@name='de.neom.neoreadersdk']/
    class[@name='Resolution']/
    method[@name='compareTo' and count(parameter)=1 and
    parameter[1][@type='de.neom.neoreadersdk.Resolution']]/
    parameter[1]"name="managedType">Java.Lang.Object</attr> 
```

### <a name="managedreturn"></a>managedReturn

메서드의 반환 형식을 변경합니다. (특성을 JNI 서명과 호환 되지 않는 변경 될 수 있습니다 반환 변경 됨)으로 반환 특성을 변경 되지 않습니다. 다음 예제에서는 반환 형식이 고 `append` 메서드가에서 변경 될 `SpannableStringBuilder` 를 `IAppendable` (회수는 C# 지원 하지 않습니다 공변 반환 형식):

```xml
<attr path="/api/package[@name='android.text']/
    class[@name='SpannableStringBuilder']/
    method[@name='append']" 
    name="managedReturn">Java.Lang.IAppendable</attr>
```

### <a name="obfuscated"></a>난독 처리

Java 라이브러리를 난독 처리 하는 도구는 Xamarin.Android 바인딩 생성기 및 C# 래퍼 클래스를 생성 하는 기능과 방해할 수 있습니다. 난독 처리 된 클래스의 특성 포함: * 클래스 이름에는  **$** , 즉, **$.class** * 클래스 이름은 소문자, 즉, 손상 완전히  **a.class**

이 조각은 "난독 처리 되지 않은" C# 형식을 생성 하는 방법의 예입니다.

```xml
<attr path="/api/package[@name='{package_name}']/class[@name='{name}']" 
    name="obfuscated">false</attr>
```

### <a name="propertyname"></a>propertyName

관리 되는 속성의 이름을 변경 하려면이 특성을 사용할 수 있습니다.

사용 하 여의 특수 한 경우 `propertyName` Java 클래스의 필드에 대 한 getter 메서드만 있는 상황을 포함 합니다. 이 경우 바인딩 생성기.net에서이 금지 하는 쓰기 전용 속성을 만드는 할 수 있습니다. 다음 코드 예제는.NET 속성을 설정 하 여 "제거" 하는 방법의 `propertyName` 빈 문자열로:

```xml
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='setResourceDescriptor' 
    and count(parameter)=1 
    and parameter[1][@type='java.lang.String']]" 
    name="propertyName"></attr>
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='getResourceDescriptor' 
    and count(parameter)=0]" 
    name="propertyName"></attr>
```

참고 setter 및 getter 메서드 여전히 바인딩 생성기에서 만들 수 있습니다.

### <a name="sender"></a>sender

메서드의 매개 변수 지정는 `sender` 이벤트에 매핑되는 메서드를 될 때 매개 변수입니다. 값일 수 `true` 또는 `false`합니다. 예:

```xml
<attr path="/api/package[@name='android.app']/
    interface[@name='TimePickerDialog.OnTimeSetListener']/
    method[@name='onTimeSet']/
    parameter[@name='view']" 
    name="sender">true</ attr>
```

### <a name="visibility"></a>표시 여부

이 특성은 클래스, 메서드 또는 속성의 표시 유형을 변경 하려면 사용 합니다. 예를 들어 승격 해야 할 수 있습니다는 `protected` Java 메서드 C# 래퍼를 해당 하는 `public`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method --> 
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

## <a name="enumfieldsxml-and-enummethodsxml"></a>EnumFields.xml 및 EnumMethods.xml

Android 라이브러리의 라이브러리 메서드나 속성에 전달 되는 상태를 나타내는 정수 상수를 사용 하는 경우가 있습니다. 대부분의 경우에서 이러한 정수 상수를 C#에서 열거형 바인딩할 유용 합니다. 이 매핑은 위해 사용 하 여는 **EnumFields.xml** 및 **EnumMethods.xml** 바인딩 프로젝트의 파일입니다. 

### <a name="defining-an-enum-using-enumfieldsxml"></a>EnumFields.xml를 사용 하 여 열거형을 정의 합니다.

**EnumFields.xml** 파일 Java 사이의 매핑이 포함 `int` 상수 및 C# `enums`합니다. 집합에 대해 생성 되 고 C# 열거형의 다음 예제에서는 보겠습니다 `int` 상수: 

```xml 
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit">
    <field jni-name="UNIT_SECOND" clr-name="Second" value="0" />
    <field jni-name="UNIT_METER" clr-name="Meter" value="1" />
    <field jni-name="UNIT_MILIWATT_HOURS" clr-name="MilliwattHour" value="2" />
</mapping>
```

Java 클래스 이동 여기 `SKRealReachSettings` 라는 C# 열거형 정의 및 `SKRealReachSettings` 네임 스페이스에 `Skobbler.Ngx.Map.RealReach`합니다. `field` Java 상수 이름을 정의 하는 항목 (예제 `UNIT\_SECOND`), 열거형 항목의 이름 (예 `Second`), 및 두 엔터티를 나타내는 정수 값 (예 `0`). 

### <a name="defining-gettersetter-methods-using-enummethodsxml"></a>EnumMethods.xml를 사용 하 여 Getter/Setter 메서드를 정의 합니다.

**EnumMethods.xml** 파일을 통해 Java에서 메서드 매개 변수 및 반환 형식을 변경 `int` C# 상수 `enums`합니다. 즉, 읽기 및 쓰기의 C# 열거형 매핑합니다 (에 정의 된는 **EnumFields.xml** 파일) java `int` 상수 `get` 및 `set` 메서드.

지정 된는 `SKRealReachSettings` 다음 위에 정의 된 열거형 **EnumMethods.xml** 파일에서이 열거형에 대 한 getter/setter를 정의 합니다.

```xml
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings">
    <method jni-name="getMeasurementUnit" parameter="return" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
    <method jni-name="setMeasurementUnit" parameter="measurementUnit" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
</mapping>
```

첫 번째 `method` 줄 Java의 반환 값을 매핑합니다 `getMeasurementUnit` 메서드는 `SKRealReachSettings` 열거형입니다. 두 번째 `method` 줄의 첫 번째 매개 변수를 매핑하는 `setMeasurementUnit` 동일한 열거형에 있습니다.

모든 위치에서 이러한 변경 내용을 사용 하 여 사용할 수 있습니다 다음 코드 Xamarin.Android에서 설정 하 여 `MeasurementUnit`: 

```csharp
realReachSettings.MeasurementUnit = SKMeasurementUnit.Second;
```


## <a name="summary"></a>요약

이 문서 Xamarin.Android API 정의에서 변환할 메타 데이터를 사용 하는 방법을 설명는 *Google* *AOSP 형식*합니다. 사용 하 여 사용할 수 있는 변경 내용을 포함 한 후 *Metadata.xml*멤버의 이름을 바꿀 때 발생 하는 같은 제한 사항이 검사 하 고 그 보내서 각 특성을 사용 해야 하는 시기를 설명 하는 지원 되는 XML 특성의 목록입니다.



## <a name="related-links"></a>관련 링크

- [JNI 작업](~/android/platform/java-integration/working-with-jni.md)
- [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)
- [GAPI 메타 데이터](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
