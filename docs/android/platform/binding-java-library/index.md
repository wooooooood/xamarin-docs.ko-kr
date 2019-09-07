---
title: Java 라이브러리 바인딩
description: Android 커뮤니티에는 앱에서 사용 하려는 여러 Java 라이브러리가 있습니다. 이 가이드에서는 바인딩 라이브러리를 만들어 Xamarin Android 응용 프로그램에 Java 라이브러리를 통합 하는 방법에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: B39FF1D5-69C3-8A76-D268-C227A23C9485
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/01/2017
ms.openlocfilehash: e829c953278d8edeb697d27da8e3707ee1c91784
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757594"
---
# <a name="binding-a-java-library"></a>Java 라이브러리 바인딩

_Android 커뮤니티에는 앱에서 사용 하려는 여러 Java 라이브러리가 있습니다. 이 가이드에서는 바인딩 라이브러리를 만들어 Xamarin Android 응용 프로그램에 Java 라이브러리를 통합 하는 방법에 대해 설명 합니다._

## <a name="overview"></a>개요

Android 용 타사 라이브러리 에코 시스템은 광범위 합니다. 따라서 기존 Android 라이브러리를 사용 하 여 새 Android 라이브러리를 만드는 것이 일반적입니다. Xamarin.ios는 이러한 라이브러리를 사용 하는 두 가지 방법을 제공 합니다.

- 호출을 통해 C# Java 코드를 호출할 수 있도록 래퍼 C# 를 사용 하 여 라이브러리를 자동으로 래핑하는 *바인딩 라이브러리* 를 만듭니다.

- *JNI*( *java Native Interface* )를 사용 하 여 java 라이브러리 코드에서 직접 호출을 호출 합니다. JNI는 Java 코드를 호출 하 고 네이티브 응용 프로그램 또는 라이브러리에서 호출할 수 있도록 하는 프로그래밍 프레임 워크입니다.

이 가이드에서는 첫 번째 옵션인, 하나 이상의 기존 Java 라이브러리를 응용 프로그램에서 연결할 수 있는 어셈블리로 래핑하는 *바인딩 라이브러리* 를 만드는 방법에 대해 설명 합니다. JNI 사용에 대 한 자세한 내용은 [JNI 작업](~/android/platform/java-integration/working-with-jni.md)을 참조 하세요.

Xamarin Android는*mcw*( *관리 되는 호출 가능 래퍼* )를 사용 하 여 바인딩을 구현 합니다. MCW는 관리 코드에서 Java 코드를 호출 해야 할 때 사용 되는 JNI 브리지입니다. 또한 관리 되는 호출 가능 래퍼는 java 형식의 하위 클래스를 지원 하 고 Java 형식의 가상 메서드를 재정의 하는 기능을 제공 합니다. 마찬가지로, Android runtime (ART) 코드에서 관리 코드를 호출 하려는 경우 ACW (Android 호출 가능 래퍼) 라고 하는 다른 JNI bridge를 통해 수행 합니다. 이 [아키텍처](~/android/internals/architecture.md) 는 다음 다이어그램에 설명 되어 있습니다.

[![Android JNI 브리지 아키텍처](images/architecture.png)](images/architecture.png#lightbox)

바인딩 라이브러리는 Java 형식에 대 한 관리 되는 호출 가능 래퍼를 포함 하는 어셈블리입니다. 예를 들어, 다음은 바인딩 라이브러리에서 `MyClass`래핑하는 Java 형식입니다.

```java
package com.xamarin.mycode;

public class MyClass
{
    public String myMethod (int i) { ... }
}
```

가 포함 `MyClass`된 **jar** 에 대 한 바인딩 라이브러리를 생성 한 후에는에서이를 인스턴스화하고에서 C#메서드를 호출할 수 있습니다.

```csharp
var instance = new MyClass ();

string result = instance.MyMethod (42);
```

이 바인딩 라이브러리를 만들려면 Xamarin.ios *Java 바인딩 라이브러리* 템플릿을 사용 합니다. 결과 바인딩 프로젝트는 MCW 클래스, **jar** 파일 및 리소스에 포함 된 Android 라이브러리 프로젝트용 리소스를 사용 하 여 .net 어셈블리를 만듭니다. Android 보관에 대 한 바인딩 라이브러리 ()를 만들 수도 있습니다. AAR) 파일 및 Eclipse Android 라이브러리 프로젝트 결과 바인딩 라이브러리 DLL 어셈블리를 참조 하 여 Xamarin Android 프로젝트에서 기존 Java 라이브러리를 다시 사용할 수 있습니다.

바인딩 라이브러리의 형식을 참조 하는 경우 바인딩 라이브러리의 네임 스페이스를 사용 해야 합니다. 일반적으로 .net 네임 스페이스 `using` 버전의 Java 패키지 이름인 지시문 C# 을 소스 파일의 맨 위에 추가 합니다. 예를 들어, 다음과 같은 경우에는 바인딩된 **jar** 의 Java 패키지 이름을 사용할 수 있습니다.

```csharp
com.company.package
```

그런 다음 C# 소스 파일의 맨 `using` 위에 다음 문을 추가 하 여 bound 파일의 형식에 액세스 **합니다** .

```csharp
using Com.Company.Package;
```

기존 Android 라이브러리를 바인딩할 때는 다음 사항을 염두에 두어야 합니다.

- **라이브러리에 대 한 외부 종속성이 있나요?** &ndash;Android 라이브러리에 필요한 모든 Java 종속성은 **ReferenceJar** 프로젝트에 포함 되거나 **EmbeddedReferenceJar**로 포함 되어야 합니다. 모든 네이티브 어셈블리를 바인딩 프로젝트에 **EmbeddedNativeLibrary**로 추가 해야 합니다.  

- **Android 라이브러리 대상의 Android API 버전은 무엇 인가요?** &ndash;Android API 수준을 "다운 그레이드" 할 수 없습니다. Xamarin Android 바인딩 프로젝트가 Android 라이브러리와 동일한 API 수준 (또는 그 이상)을 대상으로 하는지 확인 합니다.

- **라이브러리를 컴파일하는 데 사용 된 JDK 버전은 무엇 인가요?** &ndash;Android 라이브러리가 Xamarin.ios에서 사용 하는 것과 다른 버전의 JDK로 빌드된 경우 바인딩 오류가 발생할 수 있습니다. 가능 하면 Xamarin.ios 설치에 사용 되는 것과 동일한 버전의 JDK를 사용 하 여 Android 라이브러리를 다시 컴파일하십시오.

## <a name="build-actions"></a>빌드 작업

바인딩 라이브러리를 만들 때 **jar** 또는에 대 한 *빌드 작업* 을 설정 합니다. 바인딩 라이브러리 프로젝트 &ndash; 에 통합 하는 AAR 파일 각 빌드 작업은 **.jar** 또는의 방법을 결정 합니다. AAR 파일은 바인딩 라이브러리에 포함 되거나 바인딩 라이브러리에 의해 참조 됩니다. 다음 목록에서는 이러한 빌드 작업을 요약 합니다.

- `EmbeddedJar`결과 바인딩 라이브러리 DLL에 jar를 포함 리소스로 포함 합니다. &ndash; 가장 간단 하 고 가장 일반적으로 사용 되는 빌드 작업입니다. **Jar** 를 자동으로 바이트 코드로 컴파일하고 바인딩 라이브러리에 패키지 하려는 경우이 옵션을 사용 합니다.

- `InputJar`는 결과 바인딩 라이브러리에 jar을 포함 하지 않습니다. &ndash; GDIPLUS.DLL. 바인딩 라이브러리입니다. DLL은 런타임에이 **jar** 에 종속 됩니다. 바인딩 라이브러리에 **jar** 를 포함 하지 않으려는 경우 (예: 라이선스의 경우)이 옵션을 사용 합니다. 이 옵션을 사용 하는 경우 앱을 실행 하는 장치에서 입력 **jar** 를 사용할 수 있는지 확인 해야 합니다.

- `LibraryProjectZip`&ndash; 을 포함 합니다. AAR 파일을 결과 바인딩 라이브러리로 변환 합니다. GDIPLUS.DLL. 이는 바인딩된의 코드 및 리소스에 액세스할 수 있다는 점을 제외 하 고 EmbeddedJar와 비슷합니다. AAR 파일입니다. 을 포함 하려는 경우이 옵션을 사용 합니다. 바인딩 라이브러리로 AAR.

- `ReferenceJar`참조를 지정 합니다. jar: 참조 jar는 바인딩된 jar 또는입니다. &ndash; AAR 파일은에 따라 달라 집니다. 이 참조 **jar** 는 컴파일 타임 종속성을 충족 하는 데만 사용 됩니다. 이 빌드 작업을 사용 하는 C# 경우 참조 **jar** 에 대 한 바인딩이 생성 되지 않으며 결과 바인딩 라이브러리에 포함 되지 않습니다. GDIPLUS.DLL. 참조에 대 한 바인딩 라이브러리를 만들지만 아직 수행 하지 않은 경우이 옵션을 사용 **합니다.** 이 빌드 작업은 여러 **.jar**s (및/또는)를 패키징하는 데 유용 합니다. AARs)를 상호 의존적인 여러 바인딩 라이브러리로 변환 합니다.

- `EmbeddedReferenceJar`결과 바인딩 라이브러리에 참조 jar를 포함 합니다. &ndash; GDIPLUS.DLL. 입력 C# **jar** (또는)에 대 한 바인딩을 만들려는 경우이 빌드 작업을 사용 합니다. AAR) 및 모든 참조 **jar**를 바인딩 라이브러리에 있습니다.

- `EmbeddedNativeLibrary`Native를 바인딩에 포함 **합니다.** &ndash; 이 빌드 작업은에 사용 **되므로** **jar** 파일에 필요한 파일을 바인딩합니다. Java 라이브러리에서 코드를 실행 하기 전에 **라이브러리를** 수동으로 로드 해야 할 수도 있습니다. 이 내용은 아래에 설명 되어 있습니다.

이러한 빌드 작업은 다음 가이드에 자세히 설명 되어 있습니다.

또한 다음과 같은 빌드 작업을 사용 하 여 Java API 설명서를 가져오고 C# XML 문서로 변환할 수 있습니다.

- `JavaDocJar`는 Maven 패키지 스타일 (일반적으로 `FOOBAR-javadoc**.jar**`)을 준수 하는 Java 라이브러리의 Javadoc archive Jar를 가리키는 데 사용 됩니다.
- `JavaDocIndex`는 API 참조 문서 HTML `index.html` 내에서 파일을 가리키는 데 사용 됩니다.
- `JavaSourceJar`는를 보완 `JavaDocJar`하 여 먼저 원본에서 JavaDoc를 생성 한 다음 Maven 패키지 스타일을 `JavaDocIndex`준수 하는 Java 라이브러리에 대해 결과를로 처리 합니다 (일반적 `FOOBAR-sources**.jar**`으로).

API 설명서는 Java8, Java7 또는 Java6 SDK (모두 다른 형식) 또는 DroidDoc 스타일의 기본 doclet 합니다.

## <a name="including-a-native-library-in-a-binding"></a>바인딩에 네이티브 라이브러리 포함

Java 라이브러리 바인딩의 일부로 Xamarin.ios 바인딩 프로젝트에 **.** 의 라이브러리를 포함 해야 할 수도 있습니다. 래핑된 Java 코드가 실행 될 때 xamarin.ios는 JNI을 호출 하 고 오류 메시지 _UnsatisfiedLinkError를 만듭니다. 네이티브 메서드를 찾을 수_ 없음: 응용 프로그램에 대해 logcat에서 표시 됩니다.

이 문제를 해결 하려면를 `Java.Lang.JavaSystem.LoadLibrary`호출 하 여 **.** 라이브러리를 수동으로 로드 해야 합니다. 예를 들어 Xamarin.ios 프로젝트에 shared library **libpocketsphinx_jni** 가 있다고 가정 하면 **EmbeddedNativeLibrary**의 빌드 작업을 사용 하 여 바인딩 프로젝트에 포함 된 다음 코드 조각 (공유 라이브러리를 사용 하기 전에 실행 됨) 는를 로드 **합니다.** 라이브러리:

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="adapting-java-apis-to-ceparsl"></a>C에 Java Api 적용&eparsl;

Xamarin Android 바인딩 생성기는 .NET 패턴에 해당 하는 일부 Java 관용구 및 패턴을 변경 합니다. 다음 목록에서는 Java가/.Net에 C#매핑되는 방법을 설명 합니다.

- Java의 _Setter/Getter 메서드_ 는 .Net의 _속성_ 입니다.

- Java의 _필드_ 는 .Net의 _속성_ 입니다.

- Java의 _수신기/수신기 인터페이스_ 는 .Net의 _이벤트_ 입니다. 콜백 인터페이스에 있는 메서드의 매개 변수는 `EventArgs` 서브 클래스로 표현 됩니다.

- Java의 _정적 중첩 클래스_ 는 .Net의 _중첩 된 클래스_ 입니다.

- Java의 _내부 클래스_ 는의 C#인스턴스 생성자를 사용 하는 _중첩 클래스_ 입니다.

## <a name="binding-scenarios"></a>바인딩 시나리오

다음 바인딩 시나리오 가이드를 참조 하 여 앱에 통합 하기 위해 Java 라이브러리 (또는 라이브러리)를 바인딩할 수 있습니다.

- [바인딩. JAR](~/android/platform/binding-java-library/binding-a-jar.md) 는 **jar** 파일에 대 한 바인딩 라이브러리를 만들기 위한 연습입니다.

- [바인딩. AAR](~/android/platform/binding-java-library/binding-an-aar.md) 은에 대 한 바인딩 라이브러리를 만들기 위한 연습입니다. AAR 파일. Android Studio 라이브러리를 바인딩하는 방법에 대 한 자세한 내용은이 연습을 참조 하세요.

- [Eclipse 라이브러리 프로젝트를 바인딩하](~/android/platform/binding-java-library/binding-a-library-project.md) 는 연습은 Android 라이브러리 프로젝트에서 바인딩 라이브러리를 만드는 데 사용할 수 있습니다. Eclipse Android 라이브러리 프로젝트를 바인딩하는 방법을 알아보려면이 연습을 참조 하세요.

- [바인딩을 사용자 지정](~/android/platform/binding-java-library/customizing-bindings/index.md) 하는 방법에 대 한 자세한C#내용은 바인딩을 수동으로 수정 하 여 빌드 오류를 해결 하 고 결과 API를 모양을 지정 하는 방법을 설명 합니다.

- [바인딩 문제 해결](~/android/platform/binding-java-library/troubleshooting-bindings.md) 에서는 일반적인 바인딩 오류 시나리오를 나열 하 고, 가능한 원인을 설명 하 고, 이러한 오류를 해결 하기 위한 제안 사항을 제공 합니다.

## <a name="related-links"></a>관련 링크

- [JNI 사용](~/android/platform/java-integration/working-with-jni.md)
- [GAPI 메타 데이터](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
- [네이티브 라이브러리 사용](~/android/platform/native-libraries.md)
