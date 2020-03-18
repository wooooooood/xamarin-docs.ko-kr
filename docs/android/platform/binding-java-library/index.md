---
title: Java 라이브러리 바인딩
description: Android 커뮤니티에는 앱에서 사용할 수 있는 여러 Java 라이브러리가 있습니다. 이 가이드에서는 바인딩 라이브러리를 만들어 Xamarin.Android 애플리케이션에 Java 라이브러리를 통합하는 방법에 대해 설명합니다.
ms.prod: xamarin
ms.assetid: B39FF1D5-69C3-8A76-D268-C227A23C9485
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/01/2017
ms.openlocfilehash: d40a23076ec8f405e57ec40de47ec9ad2261d85d
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027599"
---
# <a name="binding-a-java-library"></a>Java 라이브러리 바인딩

_Android 커뮤니티에는 앱에서 사용할 수 있는 여러 Java 라이브러리가 있습니다. 이 가이드에서는 바인딩 라이브러리를 만들어 Xamarin.Android 애플리케이션에 Java 라이브러리를 통합하는 방법에 대해 설명합니다._

## <a name="overview"></a>개요

Android용 타사 라이브러리 에코시스템은 방대한 규모를 갖습니다. 따라서 기존 Android 라이브러리를 사용하는 것이 새로 만드는 것보다 합리적인 경우가 많습니다. Xamarin.Android는 이러한 라이브러리를 사용하는 두 가지 방법을 제공합니다.

- C# 호출을 통해 Java 코드를 호출할 수 있도록 C# 래퍼를 사용하여 라이브러리를 자동으로 래핑하는 *바인딩 라이브러리*를 만듭니다.

- *Java 기본 인터페이스*(*JNI*)를 사용하여 Java 라이브러리 코드에서 직접 호출을 실행합니다. JNI는 Java 코드에서 기본 애플리케이션 또는 라이브러리를 호출하거나 기본 애플리케이션 또는 라이브러리에서 Java 코드를 호출할 수 있도록 하는 프로그래밍 프레임워크입니다.

이 가이드에서는 첫 번째 옵션인 하나 이상의 기존 Java 라이브러리를 애플리케이션에서 연결할 수 있는 어셈블리로 래핑하는 *바인딩 라이브러리*를 만드는 방법에 대해 설명합니다. JNI 사용에 대한 자세한 내용은 [JNI 작업](~/android/platform/java-integration/working-with-jni.md)을 참조하세요.

Xamarin.Android는 *관리형 호출 가능 래퍼*(*MCW*)를 사용하여 바인딩을 구현합니다. MCW는 관리 코드에서 Java 코드를 호출해야 할 때 사용되는 JNI 브리지입니다. 또한 관리형 호출 가능 래퍼는 Java 형식의 하위 클래스를 지원하고 Java 형식의 가상 메서드를 재정의하도록 지원합니다. 마찬가지로, Android 런타임(ART) 코드에서 관리 코드를 호출할 때는 Android 호출 가능 래퍼(ACW)로 알려진 다른 JNI 브리지를 통해 이 작업이 수행됩니다. 다음 다이어그램에 이 [아키텍처](~/android/internals/architecture.md)가 나와 있습니다.

[![Android JNI 브리지 아키텍처](images/architecture.png)](images/architecture.png#lightbox)

바인딩 라이브러리는 Java 형식에 대한 관리형 호출 가능 래퍼를 포함하는 어셈블리입니다. 예를 들어 다음은 바인딩 라이브러리에 래핑할 Java 형식 `MyClass`입니다.

```java
package com.xamarin.mycode;

public class MyClass
{
    public String myMethod (int i) { ... }
}
```

`MyClass`를 포함하는 **.jar**에 대한 바인딩 라이브러리를 생성한 후 이를 인스턴스화하고 C#에서 이에 대한 메서드를 호출할 수 있습니다.

```csharp
var instance = new MyClass ();

string result = instance.MyMethod (42);
```

이 바인딩 라이브러리를 만들려면 Xamarin.Android *Java 바인딩 라이브러리* 템플릿을 사용합니다. 결과 바인딩 프로젝트는 MCW 클래스, **.jar** 파일 및 여기에 포함된 Android 라이브러리 프로젝트용 리소스를 사용하여 .NET 어셈블리를 만듭니다. Android 보관(.AAR) 파일 및 Eclipse Android 라이브러리 프로젝트에 대한 바인딩 라이브러리를 만들 수도 있습니다. 결과 바인딩 라이브러리 DLL 어셈블리를 참조하면 Xamarin.Android 프로젝트에서 기존 Java 라이브러리를 다시 사용할 수 있습니다.

바인딩 라이브러리의 형식을 참조하는 경우 바인딩 라이브러리의 네임스페이스를 사용해야 합니다. 일반적으로 Java 패키지 이름의 .NET 네임스페이스 버전인 C# 소스 파일 맨 위에 `using` 지시문을 추가합니다. 예를 들어, 바인딩된 **.jar**에 대한 Java 패키지 이름이 다음과 같은 경우:

```csharp
com.company.package
```

C# 소스 파일 맨 위에 다음 `using` 문을 배치하여 바인딩된 **.jar** 파일의 형식에 액세스합니다.

```csharp
using Com.Company.Package;
```

기존 Android 라이브러리를 바인딩할 때는 다음 사항에 유의해야 합니다.

- **라이브러리에 대한 외부 종속성이 있나요?** &ndash;Android 라이브러리에 필요한 Java 종속성은 Xamarin.Android 프로젝트에 **ReferenceJar** 또는 **EmbeddedReferenceJar**로 포함되어야 합니다. 모든 네이티브 어셈블리를 **EmbeddedNativeLibrary**로 바인딩 프로젝트에 추가해야 합니다.  

- **Android 라이브러리의 대상 Android API 버전이 무엇인가요?** &ndash;Android API 수준을 "다운그레이드"할 수 없습니다. Xamarin.Android 바인딩 프로젝트가 Android 라이브러리와 동일한 API 수준(또는 그 이상)을 대상으로 하는지 확인합니다.

- **라이브러리를 컴파일하는 데 사용된 JDK 버전은 무엇인가요?** &ndash; Android 라이브러리를 Xamarin.Android에서 사용하는 것과 다른 JDK 버전으로 빌드한 경우 바인딩 오류가 발생할 수 있습니다. 가능한 경우 설치된 Xamarin.Android에서 사용되는 것과 동일한 버전의 JDK를 사용하여 Android 라이브러리를 다시 컴파일하세요.

## <a name="build-actions"></a>빌드 작업

바인딩 라이브러리를 만들 때는 바인딩 라이브러리 프로젝트에 통합하는 **.jar** 또는 .AAR 파일에 *빌드 작업* 을 설정합니다. 각 빌드 작업에 따라 **.jar** 또는 .AAR 파일이 바인딩 라이브러리에 포함되거나 바인딩 라이브러리에 의해 참조되는 방법이 결정됩니다. 다음 목록에서는 이들 빌드 작업을 요약하여 보여 줍니다.

- `EmbeddedJar` &ndash; **.jar**을 결과 바인딩 라이브러리 DLL에 포함 리소스로 포함합니다. 가장 간단하고 가장 일반적으로 사용되는 빌드 작업입니다. **.jar**이 자동으로 바이트 코드로 컴파일되어 바인딩 라이브러리에 패키지되도록 하려면 이 옵션을 사용합니다.

- `InputJar` &ndash; **.jar**을 결과 바인딩 라이브러리 .DLL에 포함하지 않습니다. 바인딩 라이브러리 .DLL은 런타임 시 이 **.jar**에 대한 종속성을 갖습니다. 바인딩 라이브러리에 **.jar**을 포함하지 않으려는 경우 이 옵션을 사용합니다(예: 라이선스 관련 문제가 있는 경우). 이 옵션을 사용할 경우에는 앱을 실행하는 디바이스에서 입력 **.jar**을 사용할 수 있는지 확인해야 합니다.

- `LibraryProjectZip` &ndash; .AAR 파일을 결과 바인딩 라이브러리 .DLL에 포함합니다. 이는 바인딩된 .AAR 파일의 리소스에 액세스할 수 있다는 점을 제외하면 EmbeddedJar과 비슷합니다. .AAR을 바인딩 라이브러리에 포함하려는 경우 이 옵션을 사용합니다.

- `ReferenceJar` &ndash; 참조 **.jar**을 지정합니다. 참조 **.jar**은 바인딩된 **.jar** 또는 .AAR 파일 중 하나가 종속된 **.jar**입니다. 이 참조 **.jar**은 컴파일 타임 종속성을 충족시키는 데만 사용됩니다. 이 빌드 작업을 사용하는 경우 C# 바인딩이 참조 **.jar**에 대해 생성되지 않으며 결과 바인딩 라이브러리 .DLL에 포함되지 않습니다. 참조 **.jar**에 대한 바인딩 라이브러리를 만들 때 이 옵션을 사용하지만 아직 수행되지 않았습니다. 이 빌드 작업은 여러 **.jar**(및/또는 .AAR)을 여러 개의 상호 종속적 바인딩 라이브러리에 패키지하는 데 유용합니다.

- `EmbeddedReferenceJar` &ndash; 참조 **.jar**을 결과 바인딩 라이브러리 .DLL에 포함합니다. 입력 **.jar**(또는 .AAR) 및 바인딩 라이브러리의 모든 참조 **.jar**에 대해 C# 바인딩을 만들려는 경우 이 빌드 작업을 사용합니다.

- `EmbeddedNativeLibrary` &ndash; 네이티브 **.so**를 바인딩에 포함합니다. 이 빌드 작업은 **.jar** 파일을 바인딩할 때 필요한 **.so** 파일에 사용됩니다. Java 라이브러리에서 코드를 실행하기 전에 **.so** 라이브러리를 수동으로 로드해야 할 수 있습니다. 이 작업이 아래에 설명되어 있습니다.

이러한 빌드 작업은 다음 가이드에서 더 자세히 설명합니다.

또한 다음 빌드 작업을 사용하면 Java API 문서를 가져와 C# XML 문서로 변환할 수 있습니다.

- `JavaDocJar`은 Maven 패키지 스타일(일반적으로 `FOOBAR-javadoc**.jar**`)을 준수하는 Java 라이브러리의 Javadoc 보관 Jar을 가리키는 데 사용됩니다.
- `JavaDocIndex`는 API 참조 문서 HTML 내의 `index.html` 파일을 가리키는 데 사용됩니다.
- `JavaSourceJar`은 `JavaDocJar`을 보완하는 데 사용되며, 먼저 원본에서 JavaDoc를 생성한 다음, Maven 패키지 스타일(일반적으로 `FOOBAR-sources**.jar**`)을 준수하는 Java 라이브러리에 대해 결과를 `JavaDocIndex`로 처리합니다.

API 설명서는 Java8, Java7, Java6 SDK(모두 다른 형식임) 또는 DroidDoc 스타일의 기본 doclet이어야 합니다.

## <a name="including-a-native-library-in-a-binding"></a>바인딩에 네이티브 라이브러리 포함

**.so** 라이브러리를 Xamarin.Android 바인딩 프로젝트에 Java 라이브러리 바인딩의 일부로 포함해야 할 수 있습니다. 래핑된 Java 코드가 실행될 때 Xamarin.Android는 JNI 호출을 수행하지 못하고 _java.lang.UnsatisfiedLinkError: 네이티브 메서드를 찾을 수 없음:_ 오류 메시지가 애플리케이션에 대한 로그(logcat)에 표시됩니다.

이 문제의 해결 방법은 `Java.Lang.JavaSystem.LoadLibrary`를 호출하여 **.so** 라이브러리를 수동으로 로드하는 것입니다. 예를 들어, Xamarin.Android 프로젝트에 **EmbeddedNativeLibrary** 빌드 작업에 대한 바인딩 프로젝트에 공유 라이브러리 **libpocketsphinx_jni.so**가 있다고 가정할 경우 다음 코드 조각(공유 라이브러리를 사용하기 전에 실행)은 **.so** 라이브러리를 로드합니다.

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="adapting-java-apis-to-ceparsl"></a>C&eparsl;에 Java API 적용

Xamarin.Android 바인딩 생성기는 .NET 패턴에 대응하도록 일부 Java 관용구 및 패턴을 변경합니다. 다음 목록에서는 Java가 C#/.NET에 매핑되는 방법을 설명합니다.

- Java의 _Setter/Getter 메서드_는 .NET에서 _속성_입니다.

- Java의 _필드_는 .NET에서 _속성_입니다.

- Java의 _Listeners/Listener 인터페이스_는 .NET에서 _이벤트_입니다. 콜백 인터페이스의 메서드 매개 변수는 `EventArgs` 하위 클래스로 표시됩니다.

- Java의 _정적 중첩 클래스_는 .NET에서 _중첩 클래스_입니다.

- Java의 _내부 클래스_는 C#의 인스턴스 생성자를 포함하는 _중첩 클래스_입니다.

## <a name="binding-scenarios"></a>바인딩 시나리오

다음 바인딩 시나리오 가이드를 참조하여 앱에 통합하기 위해 Java 라이브러리(또는 복수의 라이브러리)를 바인딩할 수 있습니다.

- [.JAR 바인딩](~/android/platform/binding-java-library/binding-a-jar.md)은 **.jar** 파일에 대한 바인딩 라이브러리를 만들기 위한 연습입니다.

- [.AAR 바인딩](~/android/platform/binding-java-library/binding-an-aar.md)은 .AAR 파일에 대한 바인딩 라이브러리를 만들기 위한 연습입니다. Android Studio 라이브러리를 바인딩하는 방법에 대한 자세한 내용은 이 연습을 참조하세요.

- [Eclipse 라이브러리 프로젝트 바인딩](~/android/platform/binding-java-library/binding-a-library-project.md)은 Android 라이브러리 프로젝트에서 바인딩 라이브러리를 만들기 위한 연습입니다. Eclipse Android 라이브러리 프로젝트를 바인딩하는 방법에 대한 자세한 내용은 이 연습을 참조하세요.

- [바인딩 사용자 지정](~/android/platform/binding-java-library/customizing-bindings/index.md)에서는 빌드 오류를 해결하기 위해 바인딩을 수동으로 수정하고 결과 API를 좀 더 "C#과 유사"하도록 만드는 방법을 설명합니다.

- [바인딩 문제 해결](~/android/platform/binding-java-library/troubleshooting-bindings.md)에서는 일반적인 바인딩 오류 시나리오를 나열하고, 가능한 원인을 설명하고, 이러한 오류를 해결하기 위한 방안을 제안합니다.

## <a name="related-links"></a>관련 링크

- [JNI 작업](~/android/platform/java-integration/working-with-jni.md)
- [GAPI 메타데이터](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
- [네이티브 라이브러리 사용](~/android/platform/native-libraries.md)
