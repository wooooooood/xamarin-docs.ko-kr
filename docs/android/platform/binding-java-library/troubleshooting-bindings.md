---
title: 바인딩 문제 해결
description: 이 문서에서는 바인딩을 생성할 때 발생할 수 있는 여러 일반적인 오류를 요약 하 고 가능한 원인과 해결 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: BB81FCCF-F7BF-4C78-884E-F02C49AA819A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 0c273797d7512f062260e49e0f71fdd1132f037b
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723805"
---
# <a name="troubleshooting-bindings"></a>바인딩 문제 해결

_이 문서에서는 바인딩을 생성할 때 발생할 수 있는 여러 일반적인 오류를 요약 하 고 가능한 원인과 해결 방법을 설명 합니다._

## <a name="overview"></a>概述

Android 라이브러리 ( **aar** 또는 **.jar**) 파일을 바인딩하는 것은 거의 번거롭고 되지 않습니다. Java와 .NET 간의 차이로 인해 발생 하는 문제를 완화 하기 위해 일반적으로 추가 작업이 필요 합니다.
이러한 문제로 인해 Xamarin.ios는 Android 라이브러리를 바인딩하지 않으며 빌드 로그에 오류 메시지로 표시 됩니다. 이 가이드에서는 문제 해결을 위한 몇 가지 팁을 제공 하 고, 몇 가지 일반적인 문제/시나리오를 나열 하 고, Android 라이브러리를 성공적으로 바인딩할 수 있는 가능한 솔루션을 제공 합니다.

기존 Android 라이브러리를 바인딩할 때는 다음 사항을 염두에 두어야 합니다.

- Android 라이브러리에 필요한 모든 Java 종속성 &ndash; **라이브러리의 외부 종속성은** **ReferenceJar** 프로젝트에 **EmbeddedReferenceJar**로 포함 되어야 합니다.

- Android **라이브러리에서 대상으로 하는 ANDROID api 수준** &ndash; android api 수준을 "다운 그레이드" 할 수 없습니다. Xamarin Android 바인딩 프로젝트가 Android 라이브러리와 동일한 API 수준 (또는 그 이상)을 대상으로 하는지 확인 합니다.

- Android 라이브러리를 **패키지 하는 데 사용 된 ANDROID JDK 버전이** xamarin.ios에서 사용 중인 것과 다른 버전의 JDK로 빌드된 경우에 발생할 수 있습니다 &ndash; 바인딩 오류는 android 라이브러리입니다. 가능 하면 Xamarin.ios 설치에 사용 되는 것과 동일한 버전의 JDK를 사용 하 여 Android 라이브러리를 다시 컴파일하십시오.

Xamarin Android 라이브러리 바인딩과 관련 된 문제를 해결 하는 첫 번째 단계는 [진단 MSBuild 출력](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)을 사용 하는 것입니다.
진단 출력을 사용 하도록 설정한 후 Xamarin. Android 바인딩 프로젝트를 다시 빌드하고 빌드 로그를 검토 하 여 문제의 원인에 대 한 단서를 찾습니다.

Android 라이브러리를 디컴파일 하 고 Xamarin.ios에서 바인딩하려는 형식 및 메서드를 검사 하는 데에도 도움이 될 수 있습니다. 이 내용은이 가이드의 뒷부분에서 자세히 설명 합니다.

## <a name="decompiling-an-android-library"></a>Android 라이브러리 디컴파일

Java 클래스의 클래스와 메서드를 검사 하면 라이브러리를 바인딩하는 데 도움이 되는 유용한 정보를 제공할 수 있습니다.
[Jd-GUI](http://jd.benow.ca/) 는 JAR에 포함 된 **클래스** 파일의 Java 소스 코드를 표시할 수 있는 그래픽 유틸리티입니다. 독립 실행형 응용 프로그램으로 실행 되거나 IntelliJ 또는 Eclipse의 플러그 인으로 실행 될 수 있습니다.

Android 라이브러리를 디컴파일 하려면를 엽니다 **.** Java 디컴파일러를 사용 하는 JAR 파일입니다. 라이브러리가 이면이 **고, AAR** 파일을 보관 하려면 보관 파일에서 파일 **클래스 .jar** 를 추출 해야 합니다. 다음은 JD-GUI를 사용 하 여 [Picasso](https://square.github.io/picasso/) JAR을 분석 하는 샘플 스크린샷입니다.

![Java 디컴파일러를 사용 하 여 picasso-2.5.2를 분석 합니다.](troubleshooting-bindings-images/troubleshoot-bindings-01.png)

Android 라이브러리를 디컴파일된 소스 코드를 검사 합니다. 일반적으로 다음을 찾습니다.

- 난독 처리 된 클래스의 특성에 **난독 처리** &ndash; 특성이 있는 클래스는 다음과 같습니다.

  - Class 名称中包含 **$** ，即 **$.class**
  - 클래스 이름은 소문자 (예: **클래스** )로 완전히 손상 됩니다.      

- 참조 되지 않은 라이브러리 **`import` 문은** 참조 되지 않은 라이브러리 &ndash; 식별 하 고 **ReferenceJar** 또는 **EmbedddedReferenceJar**의 **빌드 작업** 을 사용 하 여 이러한 종속성을 Xamarin. Android 바인딩 프로젝트에 추가 합니다.

> [!NOTE]
> 디컴파일 Java 라이브러리를 사용 하지 않도록 설정 하거나 로컬 법률 또는 Java 라이브러리가 게시 된 라이선스에 따라 법적 제한을 받을 수 있습니다. 필요한 경우 Java 라이브러리를 디컴파일 하 고 소스 코드를 검사 하기 전에 법적 전문가의 서비스를 등록 합니다.

## <a name="inspect-apixml"></a>API를 검사 합니다. XML

바인딩 프로젝트를 빌드하는 과정에서 Xamarin.ios는 XML 파일 이름 **obj/Debug/api-version**을 생성 합니다.

![Obj/Debug 아래에 api .xml을 생성 했습니다.](troubleshooting-bindings-images/troubleshoot-bindings-02.png)

이 파일은 Xamarin에서 바인딩을 시도 하는 모든 Java Api 목록을 제공 합니다. 이 파일의 내용은 누락 된 형식 또는 메서드, 중복 된 바인딩을 식별 하는 데 도움이 될 수 있습니다. 이 파일을 검사 하는 데 오랜 시간이 걸리고 있지만 바인딩 문제를 일으킬 수 있는 원인을 파악할 수 있습니다. 예를 들어, **api .xml** 은 속성이 부적절 한 형식을 반환 하거나 동일한 관리 되는 이름을 공유 하는 두 가지 형식이 있음을 나타낼 수 있습니다.

## <a name="known-issues"></a>알려진 문제점

이 섹션에서는 Android 라이브러리를 바인딩할 때 발생 하는 몇 가지 일반적인 오류 메시지 또는 증상을 나열 합니다.

### <a name="problem-java-version-mismatch"></a>문제: Java 버전이 일치 하지 않습니다.

라이브러리가 컴파일되는 것과 비교 하 여 최신 또는 이전 버전의 Java를 사용 하는 경우에도 형식이 생성 되지 않거나 예기치 않은 충돌이 발생할 수 있습니다. Xamarin Android 프로젝트에서 사용 하는 것과 동일한 버전의 JDK를 사용 하 여 Android 라이브러리를 다시 컴파일합니다.

### <a name="problem-at-least-one-java-library-is-required"></a>문제: 하나 이상의 Java 라이브러리가 필요 합니다.

그러나 "하나 이상의 Java 라이브러리가 필요 합니다." 라는 오류가 표시 됩니다. JAR을 추가 했습니다.

#### <a name="possible-causes"></a>가능한 원인:

빌드 작업이 `EmbeddedJar`로 설정 되었는지 확인 합니다. 에 대 한 여러 빌드 작업이 있기 때문입니다. JAR 파일 (예: `InputJar`, `EmbeddedJar`, `ReferenceJar` 및 `EmbeddedReferenceJar`)은 바인딩 생성기에서 기본적으로 사용할 항목을 자동으로 추측할 수 없습니다. 빌드 작업에 대 한 자세한 내용은 [빌드 작업](~/android/platform/binding-java-library/index.md)을 참조 하세요.

### <a name="problem-binding-tools-cannot-load-the-jar-library"></a>문제: 바인딩 도구는를 로드할 수 없습니다. JAR 라이브러리

바인딩 라이브러리 생성기는를 로드 하지 못합니다. JAR 라이브러리.

#### <a name="possible-causes"></a>가능한 원인

개. 코드 난독 처리 (Proguard와 같은 도구를 통해)를 사용 하는 JAR 라이브러리는 Java 도구를 사용 하 여 로드할 수 없습니다. 이 도구는 Java 리플렉션 및 ASM 바이트 코드 엔지니어링 라이브러리를 사용 하므로 Android 런타임 도구가 통과할 수 있는 동안 이러한 종속 도구는 난독 처리 된 라이브러리를 거부할 수 있습니다. 이에 대 한 해결 방법은 바인딩 생성기를 사용 하는 대신 이러한 라이브러리를 직접 바인딩하는 것입니다.

### <a name="problem-missing-c-types-in-generated-output"></a>문제: 생성 C# 된 출력에 누락 된 형식이 있습니다.

Binding **.dll** 은 일부 Java 형식을 작성 하지만 누락 된 형식이 있다는 오류 때문 C# 에 생성 된 소스가 빌드되지 않습니다.

#### <a name="possible-causes"></a>가능한 원인:

이 오류는 다음과 같은 몇 가지 이유로 인해 발생할 수 있습니다.

- 바인딩되는 라이브러리는 두 번째 Java 라이브러리를 참조할 수 있습니다. 바인딩된 라이브러리의 공용 API가 두 번째 라이브러리의 형식을 사용 하는 경우 두 번째 라이브러리에 대 한 관리 되는 바인딩만 참조 해야 합니다.

- 위의 라이브러리 로드 오류가 발생 하는 이유와 유사 하 게 Java 리플렉션으로 인해 라이브러리가 삽입 되었을 수 있으며,이로 인해 예기치 않은 메타 데이터를 로드할 수 있습니다. Xamarin Android의 도구는 현재이 상황을 해결할 수 없습니다. 이러한 경우 라이브러리는 수동으로 바인딩되어야 합니다.

- .NET 4.0 런타임에 어셈블리를 로드 하지 못한 버그가 있습니다. 이 문제는 .NET 4.5 런타임에서 해결 되었습니다.

- Java는 public 클래스가 아닌 클래스에서 파생 될 수 있지만 .NET에서는 지원 되지 않습니다. 바인딩 생성기는 public이 아닌 클래스에 대 한 바인딩을 생성 하지 않으므로 이러한 파생 클래스를 올바르게 생성할 수 없습니다. 이 문제를 해결 하려면 **메타 데이터 .xml**의 제거 노드를 사용 하 여 파생 된 클래스에 대 한 메타 데이터 항목을 제거 하거나 public이 아닌 클래스를 public으로 설정 하는 메타 데이터를 수정 합니다. 후자 솔루션은 C# 소스가 빌드하도록 바인딩을 만들기 때문에 public이 아닌 클래스를 사용 하면 안 됩니다.

  예를 들면 다음과 같습니다.:

  ```xml
  <attr path="/api/package[@name='com.some.package']/class[@name='SomeClass']"
      name="visibility">public</attr>
  ```

- Java 라이브러리를 난독 처리 하는 도구는 Xamarin Android 바인딩 생성기 및 래퍼 클래스 생성 C# 기능을 방해할 수 있습니다. 다음 코드 조각은 클래스 이름을 unobfuscate 하는 **메타 데이터** 를 업데이트 하는 방법을 보여 줍니다.

  ```xml
  <attr path="/api/package[@name='{package_name}']/class[@name='{name}']"
      name="obfuscated">false</attr>
  ```

### <a name="problem-generated-c-source-does-not-build-due-to-parameter-type-mismatch"></a>문제: 매개 C# 변수 형식이 일치 하지 않아 생성 된 소스가 빌드되지 않습니다.

생성 C# 된 소스가 빌드되지 않습니다. 재정의 된 메서드의 매개 변수 형식이 일치 하지 않습니다.

#### <a name="possible-causes"></a>가능한 원인:

Xamarin.ios에는 C# 바인딩의 열거형에 매핑되는 다양 한 Java 필드가 포함 되어 있습니다. 이로 인해 생성 된 바인딩에서 형식이 호환 되지 않을 수 있습니다. 이 문제를 해결 하려면 열거형을 사용 하도록 바인딩 생성기에서 만든 메서드 시그니처를 수정 해야 합니다. 자세한 내용은 [열거형 수정](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)을 참조 하세요.

### <a name="problem-noclassdeffounderror-in-packaging"></a>문제: 포장 NoClassDefFoundError

`java.lang.NoClassDefFoundError`는 패키징 단계에서 throw 됩니다.

#### <a name="possible-causes"></a>가능한 원인:

이 오류가 발생 하는 가장 큰 이유는 필수 Java 라이브러리를 응용 프로그램 프로젝트 ( **.csproj**)에 추가 해야 하기 때문입니다. . JAR 파일은 자동으로 확인 되지 않습니다. 대상 장치 또는 에뮬레이터 (예: Google Maps **맵**)에 없는 사용자 어셈블리에 대해 Java 라이브러리 바인딩이 항상 생성 되는 것은 아닙니다. 이는 라이브러리로 서 Android 라이브러리 프로젝트를 지원 하지 않는 경우입니다. JAR은 라이브러리 dll에 포함 되어 있습니다. 예: [버그 4288](https://bugzilla.xamarin.com/show_bug.cgi?id=4288)

### <a name="problem-duplicate-custom-eventargs-types"></a>문제: 사용자 지정 EventArgs 형식이 중복 되었습니다.

사용자 지정 EventArgs 형식이 중복 되어 빌드가 실패 합니다. 다음과 같은 오류가 발생 합니다.

```shell
error CS0102: The type `Com.Google.Ads.Mediation.DismissScreenEventArgs' already contains a definition for `p0'
```

#### <a name="possible-causes"></a>가능한 원인:

이는 동일한 이름을 가진 메서드를 공유 하는 둘 이상의 인터페이스 "listener" 형식에서 제공 하는 이벤트 형식 간에 충돌이 발생 하기 때문입니다. 예를 들어 아래 예제에서 볼 수 있는 것 처럼 두 개의 Java 인터페이스가 있는 경우 생성기는 `MediationBannerListener` 및 `MediationInterstitialListener`에 대 한 `DismissScreenEventArgs`를 생성 하므로 오류가 발생 합니다.

```java
// Java:
public interface MediationBannerListener {
    void onDismissScreen(MediationBannerAdapter p0);
}
public interface MediationInterstitialListener {
    void onDismissScreen(MediationInterstitialAdapter p0);
}
```

이는 이벤트 인수 형식에 대 한 긴 이름이 회피 되도록 의도적으로 설계 된 것입니다. 이러한 충돌을 방지 하기 위해 일부 메타 데이터 변환이 필요 합니다. **Transforms\Metadata.xml** 를 편집 하 고 인터페이스 (또는 인터페이스 메서드에서) 중 하나에 `argsType` 특성을 추가 합니다.

```xml
<attr path="/api/package[@name='com.google.ads.mediation']/
        interface[@name='MediationBannerListener']/method[@name='onDismissScreen']"
        name="argsType">BannerDismissScreenEventArgs</attr>

<attr path="/api/package[@name='com.google.ads.mediation']/
        interface[@name='MediationInterstitialListener']/method[@name='onDismissScreen']"
        name="argsType">IntersitionalDismissScreenEventArgs</attr>

<attr path="/api/package[@name='android.content']/
        interface[@name='DialogInterface.OnClickListener']"
        name="argsType">DialogClickEventArgs</attr>
```

### <a name="problem-class-does-not-implement-interface-method"></a>문제: 클래스에서 인터페이스 메서드를 구현 하지 않습니다.

생성 된 클래스가 구현 된 클래스에서 구현 하는 인터페이스에 필요한 메서드를 구현 하지 않음을 나타내는 오류 메시지가 생성 됩니다. 그러나 생성 된 코드를 살펴보면 메서드가 구현 된 것을 볼 수 있습니다.

오류의 예는 다음과 같습니다.

```shell
obj\Debug\generated\src\Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.cs(8,23):
error CS0738: 'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter' does not
implement interface member 'Oauth.Signpost.Http.IHttpRequest.Unwrap()'.
'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.Unwrap()' cannot implement
'Oauth.Signpost.Http.IHttpRequest.Unwrap()' because it does not have the matching
return type of 'Java.Lang.Object'
```

#### <a name="possible-causes"></a>가능한 원인:

이는 공변 (covariant) 반환 형식을 사용 하 여 Java 메서드를 바인딩하는 경우 발생 하는 문제입니다. 이 예제에서 메서드 `Oauth.Signpost.Http.IHttpRequest.UnWrap()` `Java.Lang.Object`를 반환 해야 합니다. 그러나 메서드 `Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.UnWrap()` `HttpURLConnection`의 반환 형식이 있습니다. 이 문제를 해결 하는 방법에는 다음 두 가지가 있습니다.

- `HttpURLConnectionRequestAdapter`에 대 한 partial 클래스 선언을 추가 하 고 `IHttpRequest.Unwrap()`을 명시적으로 구현 합니다.

  ```csharp
  namespace Oauth.Signpost.Basic {
      partial class HttpURLConnectionRequestAdapter {
          Java.Lang.Object OauthSignpost.Http.IHttpRequest.Unwrap() {
              return Unwrap();
          }
      }
  }
  ```

- 생성 C# 된 코드에서 공 분산을 제거 합니다. 여기에는 생성 C# 된 코드에 `Java.Lang.Object`반환 형식이 포함 되도록 다음 변환을 **Transforms\Metadata.xml** 에 추가 하는 작업이 포함 됩니다.

  ```xml
  <attr
      path="/api/package[@name='oauth.signpost.basic']/class[@name='HttpURLConnectionRequestAdapter']/method[@name='unwrap']"
      name="managedReturn">Java.Lang.Object
  </attr>
  ```

### <a name="problem-name-collisions-on-inner-classes--properties"></a>문제: 내부 클래스/속성에 대 한 이름 충돌

상속 된 개체에 대 한 표시 유형이 충돌 합니다.

Java에서 파생 된 클래스의 표시 유형이 부모와 같을 필요는 없습니다. Java는 사용자를 대신 하 여 수정 합니다. C#에서는 명시적 이어야 하므로 계층 구조의 모든 클래스에 적절 한 표시 유형이 있는지 확인 해야 합니다. 다음 예제에서는 Java 패키지 이름을 `com.evernote.android.job`에서 `Evernote.AndroidJob`로 변경 하는 방법을 보여 줍니다.

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

### <a name="problem-a-so-library-required-by-the-binding-is-not-loading"></a>문제: A **. 따라서** 바인딩에 필요한 라이브러리가 로드 되 고 있지 않습니다.

일부 바인딩 프로젝트는의 기능에 종속 될 수도 **있습니다.** Xamarin.ios가 자동으로 라이브러리를 로드 하지 않을 **수 있습니다.** 래핑된 Java 코드가 실행 되 면 Xamarin.ios는 JNI 호출을 수행 하지 않고 오류 메시지 _UnsatisfiedLinkError: Native method를 찾을 수 없습니다._ 는 응용 프로그램에 대해 logcat out에 표시 됩니다.

이 문제를 해결 하려면 `Java.Lang.JavaSystem.LoadLibrary`를 호출 하 여. 라이브러리를 수동으로 로드 해야 합니다 **.** 예를 들어 Xamarin.ios 프로젝트에 공유 된 라이브러리 libpocketsphinx_jni 있는 것으로 가정 **합니다. 따라서** **EmbeddedNativeLibrary**의 빌드 작업을 사용 하 여 바인딩 프로젝트에 포함 되어 있으므로, 공유 라이브러리를 사용 하기 전에 실행 되는 다음 코드 조각이를 로드 **합니다.**

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="summary"></a>요약

이 문서에서는 Java 바인딩과 관련 된 일반적인 문제 해결 및 해결 방법에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [라이브러리 프로젝트](https://developer.android.com/tools/projects/index.html#LibraryProjects)
- [JNI 사용](~/android/platform/java-integration/working-with-jni.md)
- [진단 출력 사용](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)
- [Android 개발자 용 Xamarin](~/android/get-started/java-developers.md)
- [JD-GUI](http://jd.benow.ca/)
