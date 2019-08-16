---
title: Java 바인딩 메타데이터
description: C#Xamarin.ios의 코드는 JNI (Java Native Interface)에 지정 된 하위 수준 세부 정보를 추상화 하는 메커니즘인 바인딩을 통해 Java 라이브러리를 호출 합니다. Xamarin.ios는 이러한 바인딩을 생성 하는 도구를 제공 합니다. 개발자는이 도구를 사용 하 여 메타 데이터를 사용 하 여 바인딩을 만드는 방법을 제어할 수 있습니다 .이를 통해 네임 스페이스 수정과 멤버 이름 바꾸기와 같은 절차를 수행할 수 있습니다 이 문서에서는 메타 데이터가 작동 하는 방법을 설명 하 고, 메타 데이터에서 지 원하는 특성을 요약 하 고,이 메타 데이터를 수정 하 여 바인딩 문제를 해결 하는
ms.prod: xamarin
ms.assetid: 27CB3C16-33F3-F580-E2C0-968005A7E02E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 05b8be21373930ae2b501c84757b7be11f794aa9
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524613"
---
# <a name="java-bindings-metadata"></a>Java 바인딩 메타데이터

_C#Xamarin.ios의 코드는 JNI (Java Native Interface)에 지정 된 하위 수준 세부 정보를 추상화 하는 메커니즘인 바인딩을 통해 Java 라이브러리를 호출 합니다. Xamarin.ios는 이러한 바인딩을 생성 하는 도구를 제공 합니다. 개발자는이 도구를 사용 하 여 메타 데이터를 사용 하 여 바인딩을 만드는 방법을 제어할 수 있습니다 .이를 통해 네임 스페이스 수정과 멤버 이름 바꾸기와 같은 절차를 수행할 수 있습니다 이 문서에서는 메타 데이터가 작동 하는 방법을 설명 하 고, 메타 데이터에서 지 원하는 특성을 요약 하 고,이 메타 데이터를 수정 하 여 바인딩 문제를 해결 하는_


## <a name="overview"></a>개요

Xamarin Android **Java 바인딩 라이브러리** 는 _바인딩 생성기_라고도 하는 도구를 사용 하 여 기존 Android 라이브러리를 바인딩하는 데 필요한 대부분의 작업을 자동화 하려고 시도 합니다. Java 라이브러리를 바인딩할 때 Xamarin.ios는 Java 클래스를 검사 하 고 바인딩할 모든 패키지, 형식 및 멤버의 목록을 생성 합니다. 이 api 목록은 **디버그** 빌드에 대 한  **\{프로젝트 디렉터리** 에서 찾을 수 있는 xml 파일 ( **릴리스** 빌드 및  **\{프로젝트 디렉터리} \obj\Debug\api.xml** 에 있는 xml 파일에 저장 됩니다.

![Obj/Debug 폴더에 있는 api .xml 파일의 위치](java-bindings-metadata-images/java-bindings-metadata-01.png)

바인딩 생성기는 **api .xml** 파일을 필요한 C# 래퍼 클래스를 생성 하는 지침으로 사용 합니다. 이 XML 파일의 내용은 Google의 _Android 오픈 소스 프로젝트_ 형식에 대 한 변형입니다.
다음 코드 조각은 **api .xml**의 내용에 대 한 예입니다.

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

이 예제에서 **api .xml** 은를 `android` `java.lang.Object`확장 하는 이라는 `Manifest` 패키지의 클래스를 선언 합니다.

대부분의 경우에는 Java API가 더 많은 ".NET like"를 사용 하거나 바인딩 어셈블리를 컴파일할 수 없도록 하는 문제를 해결 하기 위해 사용자 지원이 필요 합니다. 예를 들어 Java 패키지 이름을 .NET 네임 스페이스로 변경 하거나, 클래스 이름을 바꾸거나, 메서드의 반환 형식을 변경 해야 할 수 있습니다.

이러한 변경 내용은 **api .xml** 을 직접 수정 하 여 수행 되지 않습니다.
대신, Java 바인딩 라이브러리 템플릿에서 제공 하는 특수 XML 파일에 변경 내용이 기록 됩니다. Xamarin Android 바인딩 어셈블리를 컴파일할 때 바인딩 생성기는 바인딩 어셈블리를 만들 때 이러한 매핑 파일의 영향을 받습니다.

이러한 XML 매핑 파일은 프로젝트의 **변환** 폴더에서 찾을 수 있습니다.

- **메타 데이터** &ndash; 를 사용 하면 생성 된 바인딩의 네임 스페이스를 변경 하는 등 최종 API에 대 한 변경 내용을 적용할 수 있습니다. 

- **Enumfields .xml** &ndash; 에는 Java `int` 상수와 C# `enums` 간의 매핑이 포함 되어 있습니다. 

- **Enummethods .xml** &ndash; 을 사용 하면 메서드 매개 변수를 변경 하 고 `int` 형식을 Java C# `enums` 상수에서로 반환할 수 있습니다. 

**메타 데이터 .xml** 파일은 다음과 같이 바인딩에 대 한 일반적인 용도의 변경을 허용 하므로 이러한 파일을 가장 많이 가져오는 것입니다.

- 네임 스페이스, 클래스, 메서드 또는 필드의 이름을 변경 하 여 .NET 규칙을 따르도록 합니다. 

- 필요 하지 않은 네임 스페이스, 클래스, 메서드 또는 필드를 제거 합니다. 

- 클래스를 다른 네임 스페이스로 이동 합니다. 

- 바인딩 디자인이 .NET framework 패턴을 따르도록 지원 클래스를 추가 합니다. 

**메타 데이터** 에 대 한 자세한 내용을 설명 하기 위해 이동 합니다.


## <a name="metadataxml-transform-file"></a>Metadata .xml 변환 파일

이미 학습 한 대로 바인딩 생성기에서 파일 **메타 데이터** 를 사용 하 여 바인딩 어셈블리 생성에 영향을 줍니다.
메타 데이터 형식은 [XPath](https://www.w3.org/TR/xpath/) 구문을 사용 하며, [gapi 메타 데이터](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata) 가이드에 설명 된 *gapi 메타 데이터* 와 거의 동일 합니다. 이 구현은 거의 완전 한 XPath 1.0 구현 이며, 따라서 1.0 표준의 항목을 지원 합니다. 이 파일은 API 파일의 요소나 특성을 변경, 추가, 숨기기 또는 이동 하는 강력한 XPath 기반 메커니즘입니다. 메타 데이터 사양의 모든 rule 요소에는 규칙이 적용 되는 노드를 식별 하는 경로 특성이 포함 됩니다. 규칙은 다음 순서로 적용 됩니다.

* **노드 추가** &ndash; Path 특성으로 지정 된 노드에 자식 노드를 추가 합니다.
* **attr** &ndash; Path 특성에 지정 된 요소의 특성 값을 설정 합니다.
* **노드 제거** &ndash; 지정 된 XPath와 일치 하는 노드를 제거 합니다.

다음은 **메타 데이터 .xml** 파일의 예입니다.

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

다음 목록에는 Java API의 가장 일반적으로 사용 되는 XPath 요소가 나와 있습니다.

- `interface`&ndash; Java 인터페이스를 찾는 데 사용 됩니다. `/interface[@name='AuthListener']`예:.

- `class`&ndash; 클래스를 찾는 데 사용 됩니다. `/class[@name='MapView']`예:.

- `method`&ndash; Java 클래스 또는 인터페이스에서 메서드를 찾는 데 사용 됩니다. `/class[@name='MapView']/method[@name='setTitleSource']`예:.

- `parameter`&ndash; 메서드에 대 한 매개 변수를 식별 합니다. 예: `/parameter[@name='p0']`



### <a name="adding-types"></a>형식 추가

요소 `add-node` 는 xamarin.ios 바인딩 프로젝트에 새 래퍼 클래스를 **api .xml**에 추가 하도록 지시 합니다. 예를 들어 다음 코드 조각에서는 생성자와 단일 필드를 사용 하 여 클래스를 만들도록 바인딩 생성기에 지시 합니다.

```xml
<add-node path="/api/package[@name='org.alljoyn.bus']">
    <class abstract="false" deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="true" visibility="public" extends="java.lang.Object">
        <constructor deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="false" type="org.alljoyn.bus.AuthListener.AuthRequest" visibility="public" />
        <field name="p0" type="org.alljoyn.bus.AuthListener.Credentials" />
    </class>
</add-node>
```


### <a name="removing-types"></a>형식 제거

Java 형식을 무시 하 고 바인딩하지 않도록 Xamarin Android 바인딩 생성기에 지시할 수 있습니다. 이 작업은 `remove-node` xml 요소를 **메타 데이터 .xml** 파일에 추가 하 여 수행 됩니다.

```xml
<remove-node path="/api/package[@name='{package_name}']/class[@name='{name}']" />
```

### <a name="renaming-members"></a>멤버 이름 바꾸기

Xamarin.ios는 원래 Java Native Interface (JNI) 이름이 필요 하므로 **api .xml** 파일을 직접 편집 하 여 멤버 이름을 바꿀 수 없습니다. `//class/@name` 따라서 특성을 변경할 수 없습니다 .이 경우 바인딩이 작동 하지 않습니다.

형식의 이름을 바꾸려는 경우를 `android.Manifest`고려 합니다.
이를 위해 **api .xml** 을 직접 편집 하 고 다음과 같이 클래스의 이름을 바꿀 수 있습니다.

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="name">NewName</attr>
```

그러면 바인딩 생성기가 래퍼 클래스에 대해 다음 C# 코드를 만듭니다.

```csharp
[Register ("android/NewName")]
public class NewName : Java.Lang.Object { ... }
```

래퍼 클래스의 이름이로 `NewName`바뀌고 원래 Java 형식은 여전히 `Manifest`입니다. 더 이상 Xamarin. Android 바인딩 클래스에서의 `android.Manifest`메서드에 액세스할 수 없습니다. 래퍼 클래스는 존재 하지 않는 Java 형식에 바인딩됩니다.

래핑된 형식 (또는 메서드)의 관리 되는 이름을 적절 하 게 변경 하려면 다음 예제와 같이 특성 `managedName` 을 설정 해야 합니다.

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="managedName">NewName</attr>
```

<a name="Renaming_EventArg_Wrapper_Classes" />

#### <a name="renaming-eventarg-wrapper-classes"></a>래퍼 `EventArg` 클래스 이름 바꾸기

Xamarin Android 바인딩 생성기가 _수신기 형식_에 대 `onXXX` 한 setter 메서드를 식별 하면 Java 기반 C# 수신기 패턴 `EventArgs` 에 대 한 .net flavoured API를 지원 하기 위해 이벤트와 하위 클래스가 생성 됩니다. 예를 들어 다음 Java 클래스와 메서드를 살펴보겠습니다.

```xml
com.someapp.android.mpa.guidance.NavigationManager.on2DSignNextManuever(NextManueverListener listener);
```

Xamarin.ios는 setter 메서드에서 접두사 `on` 를 삭제 하 고 대신 `EventArgs` 서브 클래스 이름에 `2DSignNextManuever` 대 한 기준으로 사용 합니다. 하위 클래스의 이름은 다음과 유사 합니다.

```csharp
NavigationManager.2DSignNextManueverEventArgs
```

올바른 C# 클래스 이름이 아닙니다. 이 문제를 해결 하려면 바인딩 작성자는 `argsType` 특성을 사용 하 고 `EventArgs` 하위 클래스에 C# 대 한 올바른 이름을 제공 해야 합니다.
 
```xml
<attr path="/api/package[@name='com.someapp.android.mpa.guidance']/
    interface[@name='NavigationManager.Listener']/
    method[@name='on2DSignNextManeuver']" 
    name="argsType">NavigationManager.TwoDSignNextManueverEventArgs</attr>
```

 

## <a name="supported-attributes"></a>지원 되는 특성

다음 섹션에서는 Java Api를 변환 하기 위한 몇 가지 특성에 대해 설명 합니다.

### <a name="argstype"></a>argsType

이 특성은 Java 수신기를 지원 하기 위해 생성 `EventArg` 되는 하위 클래스의 이름을 지정할 수 있도록 setter 메서드에 배치 됩니다. 이 내용은이 가이드의 뒷부분에 나오는 [EventArg 래퍼 클래스 이름 바꾸기](#Renaming_EventArg_Wrapper_Classes) 섹션에서 자세히 설명 합니다.

### <a name="eventname"></a>eventName

이벤트의 이름을 지정 합니다. 비어 있는 경우 이벤트 생성을 금지 합니다.
이에 대 한 자세한 내용은 [EventArg 래퍼 클래스 이름 바꾸기](#Renaming_EventArg_Wrapper_Classes)섹션을 참조 하세요.

### <a name="managedname"></a>managedName

패키지, 클래스, 메서드 또는 매개 변수의 이름을 변경 하는 데 사용 됩니다. 예를 들어 Java 클래스 `MyClass` 의 이름을로 `NewClassName`변경 합니다.

```xml
<attr path="/api/package[@name='com.my.application']/class[@name='MyClass']" 
    name="managedName">NewClassName</attr>
```

다음 예제에서는 메서드의 `java.lang.object.toString` 이름을로 `Java.Lang.Object.NewManagedName`바꾸기 위한 XPath 식을 보여 줍니다.

```xml
<attr path="/api/package[@name='java.lang']/class[@name='Object']/method[@name='toString']" 
    name="managedName">NewMethodName</attr>
```

### <a name="managedtype"></a>managedType

`managedType`는 메서드의 반환 형식을 변경 하는 데 사용 됩니다. 경우에 따라 바인딩 생성기는 Java 메서드의 반환 형식을 잘못 유추 하므로 컴파일 시간 오류가 발생 합니다. 이 경우 한 가지 가능한 해결 방법은 메서드의 반환 형식을 변경 하는 것입니다.

예를 들어 바인딩 생성기는 Java 메서드가 `de.neom.neoreadersdk.resolution.compareTo()` `int`을 반환 해야 하는 것으로 간주 하며,이로 인해 **오류 메시지 오류 CS0535이 발생 합니다. 취소. Neoreadersdk '는 인터페이스 멤버 ' (Java. a s t e m. a s t. t o p. a s t.** t e m) '를 구현 하지 않습니다. 다음 코드 조각에서는 생성 C# 된 메서드의 매개 변수 형식을에서 `DE.Neom.Neoreadersdk.Resolution` 로 `Java.Lang.Object`변경 하는 방법을 보여 줍니다. 

```xml
<attr path="/api/package[@name='de.neom.neoreadersdk']/
    class[@name='Resolution']/
    method[@name='compareTo' and count(parameter)=1 and
    parameter[1][@type='de.neom.neoreadersdk.Resolution']]/
    parameter[1]" name="managedType">Java.Lang.Object</attr> 
```

### <a name="managedreturn"></a>managedReturn

메서드의 반환 형식을 변경 합니다. 반환 특성을 변경 하지 않습니다. 반환 특성을 변경 하면 JNI 서명에 대해 호환 되지 않는 변경이 발생할 수 있습니다. 다음 예제에서는 `append` 메서드의 반환 형식이에서 `SpannableStringBuilder` 로 `IAppendable` 변경 됩니다 (공변 (covariant) 반환 형식을 지원 C# 하지 않는 회수).

```xml
<attr path="/api/package[@name='android.text']/
    class[@name='SpannableStringBuilder']/
    method[@name='append']" 
    name="managedReturn">Java.Lang.IAppendable</attr>
```

### <a name="obfuscated"></a>처리

Java 라이브러리를 난독 처리 하는 도구는 Xamarin Android 바인딩 생성기 및 래퍼 클래스 생성 C# 기능을 방해할 수 있습니다. 난독 처리 된 클래스의 특징은 다음과 같습니다. 

* 클래스 이름에는 **$** 가 포함 됩니다 (예: **$. class** ).
* 클래스 이름은 소문자 (예: **클래스** )로 완전히 손상 됩니다.

이 코드 조각은 "난독 처리 되지 않은" C# 형식을 생성 하는 방법의 예입니다.

```xml
<attr path="/api/package[@name='{package_name}']/class[@name='{name}']" 
    name="obfuscated">false</attr>
```

### <a name="propertyname"></a>propertyName

이 특성은 관리 되는 속성의 이름을 변경 하는 데 사용할 수 있습니다.

를 사용 하 `propertyName` 는 특수 한 사례에는 Java 클래스에 필드에 대 한 getter 메서드만 있는 상황이 포함 됩니다. 이 경우 바인딩 생성기는 .NET에서 권장 되지 않는 쓰기 전용 속성을 만들려고 합니다. 다음 코드 조각에서는를 `propertyName` 빈 문자열로 설정 하 여 .net 속성을 "제거" 하는 방법을 보여 줍니다.

```xml
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='setResourceDescriptor' 
    and count(parameter)=1 
    and parameter[1][@type='java.lang.String']]" 
    name="propertyName"></attr>
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='getResourceDescriptor' 
    and count(parameter)=0]" 
    name="propertyName"></attr>
```

Setter 및 getter 메서드는 바인딩 생성기에 의해 여전히 생성 됩니다.

### <a name="sender"></a>으로부터

메서드가 이벤트에 매핑될 때 `sender` 매개 변수로 사용할 메서드의 매개 변수를 지정 합니다. 값은 또는 `false`일 `true` 수 있습니다. 예를 들어:

```xml
<attr path="/api/package[@name='android.app']/
    interface[@name='TimePickerDialog.OnTimeSetListener']/
    method[@name='onTimeSet']/
    parameter[@name='view']" 
    name="sender">true</ attr>
```

### <a name="visibility"></a>visibility

이 특성은 클래스, 메서드 또는 속성의 표시 여부를 변경 하는 데 사용 됩니다. 예를 들어 해당 `protected` C# 하는 래퍼가 `public`되도록 Java 메서드를 승격 해야 할 수 있습니다.

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method --> 
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

## <a name="enumfieldsxml-and-enummethodsxml"></a>EnumFields .xml 및 Enumfields

Android 라이브러리가 정수 상수를 사용 하 여 라이브러리의 속성 또는 메서드에 전달 되는 상태를 나타내는 경우가 있습니다. 대부분의 경우 이러한 정수 상수를의 C#열거형에 바인딩하는 것이 유용 합니다. 이 매핑을 용이 하 게 하려면 바인딩 프로젝트에서 **Enumfields** 및 **enumfields .xml** 파일을 사용 합니다. 

### <a name="defining-an-enum-using-enumfieldsxml"></a>EnumFields .xml을 사용 하 여 열거형 정의

**Enumfields .xml** 파일에는 Java `int` 상수와 C# `enums`간의 매핑이 포함 되어 있습니다. `int` 상수 집합에 대해 생성 되는 C# 열거형의 다음 예를 살펴보겠습니다. 

```xml 
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit">
    <field jni-name="UNIT_SECOND" clr-name="Second" value="0" />
    <field jni-name="UNIT_METER" clr-name="Meter" value="1" />
    <field jni-name="UNIT_MILIWATT_HOURS" clr-name="MilliwattHour" value="2" />
</mapping>
```

`SKRealReachSettings` 여기서는 Java 클래스를 사용 하 고 네임 스페이스 C# `SKMeasurementUnit` `Skobbler.Ngx.Map.RealReach`에 라는 열거형을 정의 했습니다. 항목 `field` 은 Java 상수 이름 (예: `UNIT_SECOND`), 열거형 항목의 이름 (예 `Second`) 및 두 엔터티가 나타내는 정수 값 (예: `0`)을 정의 합니다. 

### <a name="defining-gettersetter-methods-using-enummethodsxml"></a>EnumMethods .xml을 사용 하 여 Getter/Setter 메서드 정의

**Enummethods .xml** 파일을 사용 하면 메서드 매개 변수를 변경 하 고 Java `int` 상수에서 C# `enums`로 형식을 반환할 수 있습니다. 즉, 열거형 ( **enumfields .xml** 파일에 정의 됨 C# )의 읽기 및 쓰기를 Java `int` 상수 `get` 및 `set` 메서드에 매핑합니다.

위에서 정의한 열거형을 지정 하는 경우 다음 **enummethods .xml** 파일은이 열거형에 대 한 getter/setter를 정의 합니다. `SKRealReachSettings`

```xml
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings">
    <method jni-name="getMeasurementUnit" parameter="return" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
    <method jni-name="setMeasurementUnit" parameter="measurementUnit" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
</mapping>
```

첫 번째 `method` 줄은 Java `getMeasurementUnit` 메서드의 반환 값을 `SKMeasurementUnit` 열거형에 매핑합니다. 두 번째 `method` 줄은 `setMeasurementUnit` 의 첫 번째 매개 변수를 동일한 열거형에 매핑합니다.

이러한 모든 변경 내용을 적용 하면 Xamarin.ios에서 다음 코드를 사용 하 여를 설정할 `MeasurementUnit`수 있습니다. 

```csharp
realReachSettings.MeasurementUnit = SKMeasurementUnit.Second;
```


## <a name="summary"></a>요약

이 문서에서는 Xamarin Android에서 메타 데이터를 사용 하 여 *Google* *AOSP 형식의*API 정의를 변환 하는 방법을 설명 했습니다. *Metadata .xml*을 사용 하 여 가능한 변경 내용을 적용 한 후에는 멤버 이름을 바꿀 때 발생 하는 제한 사항을 검사 하 고 각 특성을 사용 해야 하는 경우를 설명 하는 지원 되는 xml 특성 목록을 제공 합니다.



## <a name="related-links"></a>관련 링크

- [JNI 사용](~/android/platform/java-integration/working-with-jni.md)
- [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)
- [GAPI 메타 데이터](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
