---
title: Java 라이브러리 바인딩
description: Android 커뮤니티는 앱에서 사용 하려는 많은 Java 라이브러리 이 가이드에서는 Xamarin.Android 응용 프로그램에 바인딩 라이브러리를 만들어 Java 라이브러리를 통합 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: B39FF1D5-69C3-8A76-D268-C227A23C9485
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/01/2017
ms.openlocfilehash: c41aecf5f8c65ad5bfba5361b77d7c7fc047cda4
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171614"
---
# <a name="binding-a-java-library"></a>Java 라이브러리 바인딩

_Android 커뮤니티는 앱에서 사용 하려는 많은 Java 라이브러리 이 가이드에서는 Xamarin.Android 응용 프로그램에 바인딩 라이브러리를 만들어 Java 라이브러리를 통합 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Android 용 타사 라이브러리 에코 시스템은 대규모 합니다. 이 인해 자주 편이 보다 새로 만들려면 기존 Android 라이브러리를 사용 합니다. Xamarin.Android는 이러한 라이브러리를 사용 하는 두 가지 방법으로 제공 합니다.

-   만들기는 *바인딩 라이브러리* 자동으로 사용 하 여 라이브러리를 래핑하는 C# Java를 호출할 수 있도록 하는 래퍼를 통해 코드 C# 호출 합니다.

-   사용 된 *Java 기본 인터페이스* (*JNI*) Java 라이브러리 코드에서 직접 호출을 합니다. JNI는 Java 코드를 호출 하 여 네이티브 응용 프로그램 또는 라이브러리에서 호출할 수 있게 해 주는 프로그래밍 프레임 워크입니다.

이 가이드에서는 첫 번째 옵션을 설명 합니다: 만드는 방법을 *바인딩 라이브러리* 응용 프로그램에서 연결할 수 있는 어셈블리로 하나 이상의 기존 Java 라이브러리를 래핑하는 합니다. JNI 사용에 대 한 자세한 내용은 참조 하세요. [jni 작업](~/android/platform/java-integration/working-with-jni.md)합니다.

Xamarin.Android를 사용 하 여 바인딩을 구현 *관리 되는 호출 가능 래퍼* (*MCW*). MCW 관리 코드에서 Java 코드를 호출 해야 할 때 사용 되는 JNI 브리지입니다. 관리 되는 호출 가능 래퍼는 또한 Java 형식 서브클래싱 한 Java 형식에서 가상 메서드를 재정의 하는 것에 대 한 지원을 제공 합니다. 마찬가지로 Android 런타임 (최신) 코드는 관리 코드를 호출 하려는, 때마다으로 Android 호출 가능 래퍼 (ACW) 알려진 다른 JNI 브리지를 통해 수행 하도록 합니다. 이렇게 [아키텍처](~/android/internals/architecture.md) 다음 다이어그램에 나와 있습니다.

[![Android JNI 브리지 아키텍처](images/architecture.png)](images/architecture.png#lightbox)

바인딩 라이브러리는 Java 형식에 대 한 관리 되는 호출 가능 래퍼를 포함 하는 어셈블리. 예를 들어, 다음 형식인 Java `MyClass`, 바인딩 라이브러리에 배치 하려는:

```java
package com.xamarin.mycode;

public class MyClass
{
    public String myMethod (int i) { ... }
}
```

에 대 한 바인딩 라이브러리를 생성 한 후 합니다 **.jar** 포함 하는 `MyClass`를 인스턴스화해야 하에서 메서드를 호출 했습니다 C#:

```csharp
var instance = new MyClass ();

string result = instance.MyMethod (42);
```

Xamarin.Android를 사용 하면이 바인딩 라이브러리를 만들려면 *Java 바인딩 라이브러리* 템플릿. 결과 바인딩 프로젝트 MCW 클래스를 사용 하 여.NET 어셈블리를 만듭니다 **.jar** 파일 및 리소스에 포함 된 Android 라이브러리 프로젝트용입니다. Android 보관에 대 한 바인딩 라이브러리를 만들 수도 있습니다 (합니다. AAR) 파일 및 Eclipse Android 라이브러리 프로젝트. 결과 바인딩 라이브러리 DLL 어셈블리를 참조 하 여 Xamarin.Android 프로젝트에서 기존 Java 라이브러리를 재사용할 수 있습니다.

바인딩 라이브러리의 형식을 참조 하는 경우 바인딩 라이브러리의 네임 스페이스를 사용 해야 합니다. 일반적으로 `using` 맨 위에 있는 지시문에 C# 원본 파일에.NET 네임 스페이스 버전의 Java 패키지 이름입니다. 예를 들어 Java 패키지 이름에 바인딩된 **.jar** 다음과 같습니다.

```csharp
com.company.package
```

다음을 입력 하는 `using` 맨 위에 있는 문을 C# 소스 파일 형식에 바인딩된 **.jar** 파일:

```csharp
using Com.Company.Package;
```


기존 Android 라이브러리를 바인딩하는 경우 다음 사항을 유념 해야 하는:

* **라이브러리에 대 한 모든 외부 종속성 있습니까?** &ndash; Android 라이브러리에 필요한 Java 종속성을 Xamarin.Android 프로젝트에 포함 되어야 합니다는 **ReferenceJar** 또는 **EmbeddedReferenceJar**합니다. 네이티브 어셈블리 바인딩 프로젝트에 추가 되어야 합니다는 **EmbeddedNativeLibrary**합니다.  

* **Android API의 버전은 Android 라이브러리 대상 하나요?** &ndash; "다운 그레이드" Android API 레벨; 불가능 Xamarin.Android 바인딩 프로젝트에는 수준 (이상) 동일한 API를 대상으로 하는지 확인 하는 Android 라이브러리와 합니다.

* **어떤 버전 JDK의 라이브러리를 컴파일하는 데 사용 되었습니다.** &ndash; 바인딩 오류 Android 라이브러리는 Xamarin.Android에서 사용에서 보다 JDK의 다른 버전으로 작성 된 경우 발생할 수 있습니다. 가능한 경우 동일한 버전의 Xamarin.Android 설치에서 사용 되는 JDK 사용 하 여 Android 라이브러리를 다시 컴파일하십시오.


## <a name="build-actions"></a>빌드 작업

바인딩 라이브러리를 만들 때 설정한 *빌드 작업* 에 **.jar** 또는 합니다. 바인딩 라이브러리 프로젝트에 통합 하는 AAR 파일 &ndash; 각 빌드 작업을 결정 하는 방법을 **.jar** 또는 합니다. AAR 파일은 수에 포함 (또는 참조) 바인딩 라이브러리입니다. 다음 목록은 요약 이러한 빌드 작업:

* `EmbeddedJar` &ndash; 포함 된 **.jar** 포함 리소스로 결과 바인딩 라이브러리 DLL로 합니다. 가장 간단한와 대부분의 자주 사용 하는 빌드 작업입니다. 원하는 경우이 옵션을 사용 합니다 **.jar** 자동으로 바이트 코드를 컴파일 및 바인딩 라이브러리에 패키지 합니다.

* `InputJar` &ndash; 포함 하지 않습니다 합니다 **.jar** 결과 바인딩 라이브러리에 있습니다. DLL입니다. 바인딩 라이브러리입니다. DLL이 종속성을 갖습니다 **.jar** 런타임 시. 포함 하지 않을 경우이 옵션을 사용 합니다 **.jar** 바인딩 라이브러리 (예: 라이선싱의 이유로)에 있습니다. 이 옵션을 사용 해야 하는 입력 **.jar** 앱을 실행 하는 장치에서 사용할 수 있습니다.

* `LibraryProjectZip` &ndash; 포함을 합니다. 결과 바인딩에 라이브러리로 AAR 파일입니다. DLL입니다. 이 비슷합니다 EmbeddedJar를 제외 하 고 바인딩된 리소스 (뿐만 아니라 코드) 액세스할 수 있습니다. AAR 파일입니다. 포함 하려는 경우이 옵션을 사용 하는 합니다. 바인딩 라이브러리로 AAR 합니다.

* `ReferenceJar` &ndash; 참조를 지정 **.jar**: 참조일 **.jar** 은 **.jar** 바인딩된 중 하나는 **.jar** 또는 합니다. AAR 파일에 따라 달라 집니다. 이 참조가 **.jar** 컴파일 시간 종속성을 충족만 사용 됩니다. 이 빌드 작업을 사용 하는 경우 C# 바인딩을 참조 만들어지지 **.jar** 및 결과 바인딩 라이브러리에 포함 되지 않습니다. DLL입니다. 바인딩 라이브러리를 참조 하는 경우이 옵션을 사용 하 여 **.jar** 아직 수행 하지 않은 하지만 합니다. 이 빌드 동작은 여러 패키지에 대 한 유용한 **.jar**s (및/또는 합니다. AARs) 여러 개의 상호 종속적 바인딩 라이브러리에 있습니다.

* `EmbeddedReferenceJar` &ndash; 참조를 포함 **.jar** 결과 바인딩 라이브러리에 있습니다. DLL입니다. 만들려는 경우이 빌드 작업을 사용 하 여 C# 두 입력에 대 한 바인딩을 **.jar** (또는 합니다. AAR) 및 해당 참조의 모든 **.jar**(s) 바인딩 라이브러리에 있습니다.

* `EmbeddedNativeLibrary` &ndash; 네이티브 포함 **.so** 바인딩을에 있습니다. 이 빌드 작업에 사용 됩니다 **.so** 에 필요한 파일을 **.jar** 바인딩되는 파일입니다. 수동으로 로드 해야 합니다 **.so** Java 라이브러리에서 코드를 실행 하기 전에 라이브러리입니다. 이 아래 설명 되어 있습니다.

이러한 빌드 작업은 다음 가이드에서 자세히 설명 합니다.

또한 다음 빌드 작업을은 Java API 설명서를 가져오는 데 도움이 되로 변환 하는 데 사용 됩니다 C# XML 문서:

* `JavaDocJar` Maven 패키지 스타일을 따르는 Java 라이브러리에 대 한 Javadoc 아카이브 Jar 가리키는 데 사용 됩니다 (일반적으로 `FOOBAR-javadoc**.jar**`).
* `JavaDocIndex` 가리키는 데 사용 됩니다 `index.html` API 참조 설명서 HTML 파일입니다.
* `JavaSourceJar` 보완 하는 데 사용 됩니다 `JavaDocJar`먼저 원본의 JavaDoc을 생성 하 고 다음 결과 처리로 `JavaDocIndex`, Java 라이브러리는 Maven을 따르는 패키지 스타일 (일반적으로 `FOOBAR-sources**.jar**`).

API 설명서 Java8, Java7 또는 Java6 SDK (다른 모든 형식으로) 기본 doclet 해야 합니다. 또는 DroidDoc 스타일입니다.

## <a name="including-a-native-library-in-a-binding"></a>네이티브 라이브러리를 포함 하 여 바인딩에

포함 하는 데 필요한 것을 **.so** Java 라이브러리 바인딩의 일부로 Xamarin.Android 바인딩 프로젝트의 라이브러리입니다. JNI 호출 및 오류 메시지에 실패 하기 래핑된 Java 코드를 실행 하는 경우 Xamarin.Android _java.lang.UnsatisfiedLinkError: 네이티브 메서드를 찾을 수 없음:_ 응용 프로그램에 대 한 아웃 logcat에 나타납니다.

이 해결 수동으로 로드 하는 것은 **.so** 라이브러리를 호출 하 여 `Java.Lang.JavaSystem.LoadLibrary`입니다. 예를 들어 Xamarin.Android 프로젝트는 공유 라이브러리를 가정 **libpocketsphinx_jni.so** 빌드 작업을 사용 하 여 바인딩 프로젝트에 포함 된 **EmbeddedNativeLibrary**, 다음 코드 조각 (공유 라이브러리를 사용 하기 전에 실행)가 로드 된 **.so** 라이브러리:

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="adapting-java-apis-to-ceparsl"></a>C Java Api를 조정합니다.&eparsl;

Xamarin.Android 바인딩 생성기는 일부 관용구 Java 및.NET 패턴에 해당 하는 패턴 변경 됩니다. 다음은 Java에 매핑되는 방법을 설명 합니다. C#/.NET:

-   _Setter/Getter 메서드_ Java에는 _속성_ .net에서.

-   _필드_ Java에는 _속성_ .net에서.

-   _수신기/수신기 인터페이스_ Java에는 _이벤트_ .net에서. 콜백 인터페이스의 메서드 매개 변수를 나타내는 수를 `EventArgs` 하위 클래스입니다.

-   A _정적 중첩 클래스_ Java의는 _중첩 된 클래스_ .net에서.

-   _내부 클래스_ Java의 한 _중첩 된 클래스_ 의 인스턴스 생성자를 사용 하 여 C#.



## <a name="binding-scenarios"></a>바인딩 시나리오

다음 바인딩 시나리오 가이드는 앱을 통합에 대 한 Java 라이브러리 (또는 라이브러리)을 바인딩할 수 있습니다.

-   [바인딩를 합니다. JAR](~/android/platform/binding-java-library/binding-a-jar.md) 에 대 한 바인딩 라이브러리 만들기에 대 한 연습은 **.jar** 파일입니다.

-   [바인딩는 합니다. AAR](~/android/platform/binding-java-library/binding-an-aar.md) 에 대 한 바인딩 라이브러리 만들기에 대 한 연습입니다. AAR 파일입니다. Android Studio 라이브러리를 바인딩하는 방법에 알아보려면이 연습을 읽습니다.

-   [Eclipse 라이브러리 프로젝트 바인딩](~/android/platform/binding-java-library/binding-a-library-project.md) Android 라이브러리 프로젝트에서 바인딩 라이브러리를 만들기 위한 연습입니다. 이 연습에서는 Android 라이브러리 프로젝트를 Eclipse에 바인딩하는 방법을 알아보려면를 읽습니다.

-   [바인딩 사용자 지정](~/android/platform/binding-java-library/customizing-bindings/index.md) 빌드 오류를 해결 하 고 자세한 되도록 결과 API 모양을 바인딩의을 수동으로 변경 하는 방법에 설명 "C#-같은"입니다.

-   [바인딩 문제 해결](~/android/platform/binding-java-library/troubleshooting-bindings.md) 일반적인 바인딩 오류 시나리오를 나열, 가능한 원인에 설명 하 고 이러한 오류를 해결 하기 위한 제안 사항을 제공 합니다.


## <a name="related-links"></a>관련 링크

- [Jni 작업](~/android/platform/java-integration/working-with-jni.md)
- [GAPI 메타 데이터](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
- [네이티브 라이브러리 사용](~/android/platform/native-libraries.md)
