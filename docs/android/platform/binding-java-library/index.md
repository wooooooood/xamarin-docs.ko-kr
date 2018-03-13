---
title: "Java 라이브러리 바인딩"
description: "Android 커뮤니티는; 앱에서 사용 하려고 수 있는 많은 Java 라이브러리 이 가이드에서는 바인딩 라이브러리를 만들어 Java 라이브러리 Xamarin.Android 응용 프로그램에 통합 하는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: B39FF1D5-69C3-8A76-D268-C227A23C9485
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/01/2017
ms.openlocfilehash: f336767cb6aea8bd8c7ce44f6479850a63d473a6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="binding-a-java-library"></a>Java 라이브러리 바인딩

_Android 커뮤니티는; 앱에서 사용 하려고 수 있는 많은 Java 라이브러리 이 가이드에서는 바인딩 라이브러리를 만들어 Java 라이브러리 Xamarin.Android 응용 프로그램에 통합 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Android에 대 한 타사 라이브러리 생태계는 대규모입니다. 이 때문에 자주 의미가 만드는 새 보다 기존 Android 라이브러리를 사용 하도록 합니다. Xamarin.Android는 이러한 라이브러리를 사용 하는 두 가지를 제공 합니다.

-   만들기는 *바인딩 라이브러리* 를 자동으로 래핑하는 C# 래퍼를 사용 하 여 라이브러리 C# 호출을 통해 Java 코드를 호출할 수 있도록 합니다.

-   사용 하 여는 *Java 기본 인터페이스* (*JNI*) Java 라이브러리 코드에서 호출을 직접 호출 합니다. JNI는 Java 코드를 호출 하 고 네이티브 응용 프로그램 또는 라이브러리에 의해 호출 될 수 있도록 하는 프로그래밍 프레임 워크입니다.

이 가이드에서는 첫 번째 옵션을 설명:를 만드는 방법은 *바인딩 라이브러리* 을 응용 프로그램에 연결할 수 있는 어셈블리에 하나 이상의 기존 Java 라이브러리를 래핑합니다. JNI 사용에 대 한 자세한 내용은 참조 [JNI 작업](~/android/platform/java-integration/working-with-jni.md)합니다.

Xamarin.Android를 사용 하 여 바인딩을 구현 *호출 가능 래퍼 관리* (*MCW*). MCW 관리 코드에서 Java 코드를 호출 해야 할 때 사용 되는 JNI 브리지입니다. 관리 되는 호출 가능 래퍼 지원도 제공 서브클래싱 Java 형식에 대 한 Java 형식에 대 한 가상 메서드를 재정의 합니다. 마찬가지로, Android 런타임 (아트) 코드에서 하지 않고자 한다면 관리 코드를 호출할 때마다이 작업을 수행으로 Android 호출 가능 래퍼 (ACW) 알려진 다른 JNI 브리지를 통해 합니다. 이 [아키텍처](~/android/internals/architecture.md) 다음 다이어그램에 나와 있습니다.

[![Android JNI 브리지 아키텍처](images/architecture.png)](images/architecture.png#lightbox)

바인딩 라이브러리는 Java 형식에 대 한 관리 되는 호출 가능 래퍼를 포함 하는 어셈블리. Java 형식 예를 들어, 다음은 `MyClass`, 바인딩 라이브러리에 배치 하려는:

```java
package com.xamarin.mycode;

public class MyClass
{
    public String myMethod (int i) { ... }
}
```

에 대 한 바인딩 라이브러리를 생성 한 후의 **.jar** 포함 된 `MyClass`, 인스턴스화할를 C#에서 메서드를 호출할 수 있습니다:

```csharp
var instance = new MyClass ();

string result = instance.MyMethod (42);
```

Xamarin.Android를 사용 하면이 바인딩 라이브러리를 만들려면 *Java 바인딩 라이브러리* 템플릿. 결과 바인딩 프로젝트 MCW 클래스와 함께.NET 어셈블리를 만들고 **.jar** 파일 및 항목에 포함 된 Android 라이브러리 프로젝트에 대 한 리소스입니다. Android 보관을 위한 바인딩 라이브러리를 만들 수도 있습니다 (합니다. AAR) 파일 및 Eclipse Android 라이브러리 프로젝트. 결과 바인딩 라이브러리 DLL 어셈블리를 참조 하 여 기존 Java 라이브러리 Xamarin.Android 프로젝트에서 다시 사용할 수 있습니다.

바인딩 라이브러리에서 형식을 참조 하는 경우 바인딩 라이브러리의 네임 스페이스를 사용 해야 합니다. 일반적으로 추가한는 `using` 지시문 C# 소스 맨 위에 있는 파일 즉 Java 패키지 이름의.NET 네임 스페이스 버전입니다. 예를 들어 Java 패키지 이름이 바인딩된 **.jar** 다음과 같습니다.

```csharp
com.company.package
```

다음 추가 하면 다음 `using` 범위에 액세스 하 여 C# 소스 파일 맨 위에 있는 문을 입력 **.jar** 파일:

```csharp
using Com.Company.Package;
```


기존 Android 라이브러리를 바인딩하는 경우에 다음 사항을 염두에 해야 합니다.

* **라이브러리에 대 한 외부 종속성 있습니까?** &ndash; Android 라이브러리에 필요한 Java 종속성으로 Xamarin.Android 프로젝트에 포함 되어야 합니다는 **ReferenceJar** 로 **EmbeddedReferenceJar**합니다. 네이티브 어셈블리 바인딩 프로젝트에 추가 해야는 **EmbeddedNativeLibrary**합니다.  

* **Android API의 버전은 Android 라이브러리 대상 합니까?** &ndash; Android API 수준; "다운 그레이드" 불가능 확인 Xamarin.Android 바인딩 프로젝트의 수준 (또는 그 이상)와 동일한 API를 대상으로 Android 라이브러리입니다.

* **JDK의 어떤 버전의 라이브러리를 컴파일하는 데 사용 되었습니다.** &ndash; 바인딩 오류 Android 라이브러리 Xamarin.Android에 의해 사용 중인 보다 JDK의 다른 버전으로 빌드된 경우 발생할 수 있습니다. 가능 하면 Xamarin.Android 설치에서 사용 되는 JDK의 동일한 버전을 사용 하 여 Android 라이브러리를 다시 컴파일하십시오.


## <a name="build-actions"></a>빌드 작업

바인딩 라이브러리를 만들 때 설정한 *빌드 작업* 에 **.jar** 또는 합니다. 바인딩 라이브러리 프로젝트에 통합할 AAR 파일 &ndash; 결정 하는 각 빌드 작업 방법을 **.jar** 또는 합니다. AAR 파일 됩니다 수에 포함 (또는 참조) 바인딩 라이브러리입니다. 다음 목록은 요약 이러한 빌드 작업:

* `EmbeddedJar` &ndash; 포함 된 **.jar** 포함 리소스로 결과 바인딩 라이브러리 DLL로 합니다. 가장 간단한와 대부분 일반적으로 사용 되는 빌드 작업입니다. 원하는 경우이 옵션을 사용 하 여는 **.jar** 자동으로 바이트 코드로 컴파일되고 패키지 바인딩 라이브러리에 포함 합니다.

* `InputJar` &ndash; 포함 되어 있지는 **.jar** 결과 바인딩 라이브러리에 있습니다. DLL입니다. 바인딩 라이브러리입니다. DLL이에 대 한 종속성을 갖습니다 **.jar** 런타임에 합니다. 포함 하지 않을 때이 옵션을 사용 하 여는 **.jar** 바인딩 라이브러리 (예: 이유로 라이선스)에 있습니다. 이 옵션을 사용 하는 경우 확인 해야 하는 입력 **.jar** 응용 프로그램을 실행 하는 장치에서 사용할 수 있습니다.

* `LibraryProjectZip` &ndash; 포함 된 합니다. 결과 바인딩에 라이브러리로 AAR 파일입니다. DLL입니다. 이 비슷합니다 EmbeddedJar를 제외 하 고 리소스 (뿐만 아니라 코드)의 경계에 액세스할 수 있습니다. AAR 파일입니다. 포함 하려면이 옵션을 사용 하 여 프로그램. 바인딩 라이브러리로 AAR 합니다.

* `ReferenceJar` &ndash; 참조를 지정 **.jar**: 참조 **.jar** 는 **.jar** 프로그램 바인딩 중 하나를 **.jar** 또는 합니다. AAR 파일에 따라 달라 집니다. 이 참조 **.jar** 컴파일 타임 종속성을 충족 하는 데에 사용 됩니다. C# 바인딩을이 빌드 작업을 사용 하면 참조에 대해 만들어지지 않습니다 **.jar** 및 결과 바인딩 라이브러리에 포함 되지 않습니다. DLL입니다. 참조에 대 한 바인딩 라이브러리 할 때이 옵션을 사용 하 여 **.jar** 아직 수행 하지 않은 있지만 합니다. 이 빌드 작업은 여러 패키징하는 데 유용 **.jar**s (및/또는 합니다. AARs) 여러 개의 상호 종속적 바인딩 라이브러리에 있습니다.

* `EmbeddedReferenceJar` &ndash; 대 한 참조 포함 **.jar** 결과 바인딩 라이브러리에 있습니다. DLL입니다. C# 입력에 대 한 바인딩을 만들 때이 빌드 작업을 사용 하 여 **.jar** (또는 합니다. AAR)와 해당 참조의 모든 **.jar**(s) 바인딩 라이브러리에 있습니다.

* `EmbeddedNativeLibrary` &ndash; 네이티브 포함 **.so** 바인딩을에 있습니다. 이 빌드 작업에 사용 되 **.so** 에 필요한 파일은 **.jar** 바인딩되 파일입니다. 수동으로 로드 해야는 **.so** Java 라이브러리에서 코드를 실행 하기 전에 라이브러리입니다. 이 아래 설명 되어 있습니다.

이 빌드 작업에서 다음 가이드에 자세히 설명 됩니다.

또한, 다음 빌드 작업 가져오기 Java API 설명서를 사용 하 고 C# XML 문서를 변환 하 합니다.

* `JavaDocJar` Maven 패키지 스타일에 맞는 Java 라이브러리용 Javadoc 보관 Jar 가리키는 데 사용 됩니다 (일반적으로 `FOOBAR-javadoc**.jar**`).
* `JavaDocIndex` 가리키는 데 `index.html` API 참조 설명서 HTML 내의 파일입니다.
* `JavaSourceJar` 실행에 사용할 `JavaDocJar`먼저 JavaDoc 원본에서 생성 하 고 다음으로 결과 처리 `JavaDocIndex`, 준수 하는 Maven Java 라이브러리용 스타일 패키지 (일반적으로 `FOOBAR-sources**.jar**`).

API 설명서 Java8, Java7 또는 Java6 SDK (서로 다른 모든 형식), 기본 doclet 있어야 합니다. 또는 DroidDoc 스타일입니다.

## <a name="including-a-native-library-in-a-binding"></a>네이티브 라이브러리를 포함 하 여 바인딩에

포함 하도록 할 수도 있습니다는 **.so** 바인딩 Java 라이브러리의 일부로 바인딩 Xamarin.Android 프로젝트에서 라이브러리입니다. Xamarin.Android JNI 통화 및 오류 메시지에 실패 하기 래핑된 Java 코드를 실행 하는 경우 _java.lang.UnsatisfiedLinkError: 네이티브 메서드를 찾을 수 없음:_ 응용 프로그램에 대 한 아웃 logcat에 표시 됩니다.

이 대 한 해결 방법은 수동으로 로드는 **.so** 라이브러리를 호출 하 여 `Java.Lang.JavaSystem.LoadLibrary`합니다. 예를 들어 Xamarin.Android 프로젝트 공유 라이브러리를 가정 **libpocketsphinx_jni.so** 의 빌드 작업을 사용 하 여 바인딩 프로젝트에 포함 된 **EmbeddedNativeLibrary**, 다음 코드 조각 (공유 라이브러리를 사용 하기 전에 실행)가 로드는 **.so** 라이브러리:

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="adapting-java-apis-to-ceparsl"></a>Java Api C 적용합니다.&eparsl;

일부 Java 관용구와.NET 패턴에 해당 하는 데 패턴 Xamarin.Android 바인딩 생성기 변경 됩니다. 다음 목록에는 Java C#에 매핑하는 방법을 설명 /.NET:

-   _Setter/Getter 메서드_ Java는 _속성_ .net에서 합니다.

-   _필드_ Java는 _속성_ .net에서 합니다.

-   _수신기/Listener 인터페이스_ Java는 _이벤트_ .net에서 합니다. 콜백 인터페이스의 메서드 매개 변수도 표현 됩니다는 `EventArgs` 하위 클래스입니다.

-   A _정적 중첩 클래스_ Java는는 _중첩 된 클래스_ .net에서 합니다.

-   _내부 클래스_ Java는는 _중첩 된 클래스_ C#에 인스턴스 생성자를 사용 합니다.



## <a name="binding-scenarios"></a>바인딩 시나리오

다음과 같은 바인딩 시나리오 가이드 앱에 통합에 대 한 Java 라이브러리 (또는 라이브러리)을 바인딩할 수 있습니다.

-   [바인딩는 합니다. JAR](~/android/platform/binding-java-library/binding-a-jar.md) 은 연습에 대 한 바인딩 라이브러리를 만들기 위한 **.jar** 파일입니다.

-   [바인딩는 합니다. AAR](~/android/platform/binding-java-library/binding-an-aar.md) 에 대 한 바인딩 라이브러리를 만들기 위한 연습입니다. AAR 파일입니다. Android Studio 라이브러리를 바인딩하는 방법에 알아보려면이 연습을 읽습니다.

-   [Eclipse 라이브러리 프로젝트를 바인딩](~/android/platform/binding-java-library/binding-a-library-project.md) Android 라이브러리 프로젝트에서 바인딩 라이브러리를 만들기 위한 연습입니다. 이 연습에서는 Eclipse Android 라이브러리 프로젝트를 바인딩하는 방법을 알아보려면를 읽습니다.

-   [바인딩 사용자 지정](~/android/platform/binding-java-library/customizing-bindings/index.md) 를 빌드 오류를 해결 하 고 결과 API 자세한 셰이프에 바인딩을 수동으로 변경 하는 방법을 설명 "C#-같은"입니다.

-   [바인딩 문제 해결](~/android/platform/binding-java-library/troubleshooting-bindings.md) 일반적인 바인딩 오류 시나리오를 나열, 가능한 원인에 설명 하 고 이러한 오류를 해결 하기 위한 제안 사항을 제공 합니다.


## <a name="related-links"></a>관련 링크

- [JNI 작업](~/android/platform/java-integration/working-with-jni.md)
- [GAPI 메타 데이터](http://www.mono-project.com/GAPI#Metadata)
- [네이티브 라이브러리 사용](~/android/platform/native-libraries.md)
