---
title: 바인딩 문제 해결
description: 이 문서에서는 바인딩을 생성할 때 발생할 수 있는 여러 일반적인 오류를 요약하고 가능한 원인과 해결 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: BB81FCCF-F7BF-4C78-884E-F02C49AA819A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 0c273797d7512f062260e49e0f71fdd1132f037b
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "76723805"
---
# <a name="troubleshooting-bindings"></a>바인딩 문제 해결

_이 문서에서는 바인딩을 생성할 때 발생할 수 있는 여러 일반적인 오류를 요약하고 가능한 원인과 해결 방법을 설명합니다._

## <a name="overview"></a>개요

Android 라이브러리( **.aar** 또는 **.jar**) 파일을 바인딩하는 작업은 별로 간단하지 않습니다. 일반적으로 Java와 .NET 간의 차이로 인해 발생하는 문제를 완화하기 위한 추가적인 작업이 필요합니다.
해당 문제는 Xamarin.Android는 Android 라이브러리를 바인딩하지 못하게 하며 빌드 로그에 오류 메시지로 표시됩니다. 이 가이드에서는 문제 해결을 위한 몇 가지 팁을 제공하고, 몇 가지 일반적인 문제/시나리오를 나열하고, Android 라이브러리를 성공적으로 바인딩할 수 있는 가능한 솔루션을 제공합니다.

기존 Android 라이브러리를 바인딩할 때는 다음 사항에 유의해야 합니다.

- **Android 라이브러리에 대한 외부 종속성** &ndash; Android 라이브러리에 필요한 Java 종속성은 Xamarin.Android 프로젝트에 **ReferenceJar** 또는 **EmbeddedReferenceJar**로 포함되어야 합니다.

- **Android 라이브러리의 대상이 되는 Android API 레벨** &ndash; Android API 레벨을 "다운그레이드"할 수 없습니다. Xamarin.Android 바인딩 프로젝트가 Android 라이브러리와 동일한 API 레벨(또는 그 이상)을 대상으로 하는지 확인합니다.

- **Android 라이브러리를 패키지하는 데 사용된 Android JDK 버전** &ndash; 바인딩 오류는 Android 라이브러리가 Xamarin.Android에서 사용되는 것과는 다른 버전의 JDK로 빌드된 경우에 발생할 수 있습니다. 가능한 경우 설치된 Xamarin.Android에서 사용되는 것과 동일한 버전의 JDK를 사용하여 Android 라이브러리를 다시 컴파일하세요.

Xamarin.Android 라이브러리 바인딩과 관련된 문제를 해결하는 첫 번째 단계는[진단 MSBuild 출력](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)을 사용하도록 설정하는 것입니다.
진단 출력을 사용하도록 설정한 후 Xamarin.Android 바인딩 프로젝트를 다시 빌드하고 빌드 로그를 검토하여 문제의 원인에 대한 단서를 찾습니다.

Android 라이브러리를 디컴파일하고 Xamarin.Android에서 바인딩하려는 형식 및 메서드를 검사하는 데에도 유용할 수 있습니다. 이 내용은 이 가이드의 뒷부분에서 자세히 설명합니다.

## <a name="decompiling-an-android-library"></a>Android 라이브러리 디컴파일

Java 클래스의 클래스와 메서드를 검사하면 라이브러리를 바인딩하는 데 도움이 되는 유용한 정보를 얻을 수 있습니다.
[JD-GUI](http://jd.benow.ca/)는 JAR에 포함된 **CLASS** 파일의 Java 소스 코드를 표시할 수 있는 그래픽 유틸리티입니다. 독립 실행형 애플리케이션으로 실행되거나 IntelliJ 또는 Eclipse용 플러그 인으로 실행될 수 있습니다.

Android 라이브러리를 디컴파일하려면 Java 디컴파일러를 사용하여 **.JAR** 파일을 엽니다. 라이브러리가 **.AAR** 파일이면 보관 파일에서 파일 **classes.jar**을 추출해야 합니다. 다음은 JD-GUI를 사용하여 [Picasso](https://square.github.io/picasso/) JAR을 분석하는 모습을 보여 주는 샘플 스크린샷입니다.

![Java 디컴파일러를 사용하여 picasso-2.5.2.jar 분석](troubleshooting-bindings-images/troubleshoot-bindings-01.png)

Android 라이브러리를 디컴파일한 경우 소스 코드를 검사합니다. 일반적으로 다음을 찾습니다.

- **난독 처리 특성이 있는 클래스** &ndash; 난독 처리 클래스 특성에는 다음이 포함됩니다.

  - 클래스 이름에는 **$** (즉, **a$.class**)가 포함됩니다.
  - 클래스 이름은 완전히 소문자(즉, **a.class**)로 구성됩니다.      

- **참조되지 않은 라이브러리에 대한 `import` 문** &ndash; 참조되지 않은 라이브러리를 식별하고 **빌드 작업**이 **ReferenceJar** 또는 **EmbedddedReferenceJar**인 Xamarin.Android 바인딩 프로젝트에 해당 종속성을 추가합니다.

> [!NOTE]
> Java 라이브러리 디컴파일은 금지되거나 Java 라이브러리가 게시될 때 적용된 현지 법률 또는 라이선스에 따라 법적 제한을 받을 수 있습니다. 필요한 경우 Java 라이브러리를 디컴파일하고 소스 코드를 검사하기 전에 법률 전문가의 서비스를 신청하세요.

## <a name="inspect-apixml"></a>API.XML 검사

바인딩 프로젝트를 빌드하는 과정에서 Xamarin.Android는 XML 파일 이름 **obj/Debug/api.xml**을 생성합니다.

![obj/Debug 아래에 생성된 api.xml](troubleshooting-bindings-images/troubleshoot-bindings-02.png)

이 파일은 Xamarin.Android에서 바인딩을 시도하는 모든 Java API 목록을 제공합니다. 이 파일의 내용은 누락된 형식 또는 메서드, 중복된 바인딩을 식별하는 데 도움이 될 수 있습니다. 이 파일 검사는 지루하고 시간이 오래 걸리는 작업일 수 있지만 바인딩 문제의 원인을 파악하는 에 유용할 수 있습니다. 예를 들어 **api.xml**은 속성이 부적절한 형식을 반환하거나 동일한 관리형 이름을 공유하는 두 가지 형식이 있음을 나타낼 수 있습니다.

## <a name="known-issues"></a>알려진 문제

이 섹션에서는 Android 라이브러리를 바인딩할 때 발생하는 몇 가지 일반적인 오류 메시지 또는 증상을 나열합니다.

### <a name="problem-java-version-mismatch"></a>문제: Java 버전 불일치

라이브러리 컴파일에 사용한 Java 버전보다 더 최신이거나 오래된 버전을 사용하고 있으므로 형식이 생성되지 않거나 예기치 않은 크래시가 발생할 수 있습니다. Xamarin.Android 프로젝트에서 사용하는 것과 동일한 버전의 JDK를 사용하여 Android 라이브러리를 다시 컴파일합니다.

### <a name="problem-at-least-one-java-library-is-required"></a>문제: 하나 이상의 Java 라이브러리가 필요합니다.

그러나 .JAR이 추가되었더라도 "하나 이상의 Java 라이브러리가 필요합니다." 오류가 표시됩니다.

#### <a name="possible-causes"></a>가능한 원인:

빌드 작업이 `EmbeddedJar`로 설정되었는지 확인합니다. .JAR 파일에 대한 빌드 작업이 여러 개 있으므로(예: `InputJar`, `EmbeddedJar`, `ReferenceJar` 및 `EmbeddedReferenceJar`) 바인딩 생성기에서 기본적으로 사용할 빌드 작업을 자동으로 추측할 수 없습니다. 빌드 작업에 대한 자세한 내용은 [빌드 작업](~/android/platform/binding-java-library/index.md)을 참조하세요.

### <a name="problem-binding-tools-cannot-load-the-jar-library"></a>문제: 바인딩 도구는 .JAR 라이브러리를 로드할 수 없습니다.

바인딩 라이브러리 생성기는 .JAR 라이브러리를 로드하지 못합니다.

#### <a name="possible-causes"></a>가능한 원인

코드 난독 처리(Proguard와 같은 도구를 통해)를 사용하는 .JAR 라이브러리는 Java 도구를 사용하여 로드할 수 없습니다. Microsoft 도구는 Java 리플렉션 및 ASM 바이트 코드 엔지니어링 라이브러리를 사용하므로 Android 런타임 도구는 난독 처리된 라이브러리를 허용할 수 있지만 해당 종속 도구는 거부할 수 있습니다. 이에 대한 해결 방법은 바인딩 생성기를 사용하는 대신 해당 라이브러리를 직접 바인딩하는 것입니다.

### <a name="problem-missing-c-types-in-generated-output"></a>문제: 생성된 출력에서 C# 형식이 누락되었습니다.

바인딩 **.dll**이 빌드되지만 일부 Java 형식이 누락되었거나 생성된 C# 소스가 누락된 형식이 있음을 나타내는 오류로 인해 빌드되지 않습니다.

#### <a name="possible-causes"></a>가능한 원인:

이 오류는 다음과 같은 몇 가지 이유로 인해 발생할 수 있습니다.

- 바인딩되는 라이브러리는 두 번째 Java 라이브러리를 참조할 수 있습니다. 바인딩된 라이브러리의 퍼블릭 API가 두 번째 라이브러리의 형식을 사용하는 경우 두 번째 라이브러리에 대한 관리형 바인딩도 참조해야 합니다.

- 위의 라이브러리 로드 오류가 발생하는 이유와 유사하게 Java 리플렉션으로 인해 라이브러리가 주입되었을 수 있으며, 이로 인해 예기치 않게 메타데이터가 로드될 수 있습니다. Xamarin.Android의 도구로는 현재 이 상황을 해결할 수 없습니다. 이 경우 라이브러리는 수동으로 바인딩되어야 합니다.

- .NET 4.0 런타임에 필요한 어셈블리를 로드하지 못하는 버그가 있습니다. 이 문제는 .NET 4.5 런타임에서 해결되었습니다.

- Java는 public이 아닌 클래스에서 public 클래스에서 파생할 수 있지만 .NET에서는 해당 파생이 지원되지 않습니다. 바인딩 생성기는 public이 아닌 클래스에 대한 바인딩을 생성하지 않으므로 해당 파생 클래스를 올바르게 생성할 수 없습니다. 이 문제를 해결하려면 **Metadata.xml**의 remove-node를 사용하여 파생 클래스에 대한 메타데이터 항목을 제거하거나 public이 아닌 클래스를 public으로 설정하도록 메타데이터를 수정합니다. 후자 솔루션은 C# 소스가 빌드하도록 바인딩을 만들기 때문에 public이 아닌 클래스를 사용하면 안 됩니다.

  예를 들어:

  ```xml
  <attr path="/api/package[@name='com.some.package']/class[@name='SomeClass']"
      name="visibility">public</attr>
  ```

- Java 라이브러리를 난독 처리하는 도구는 Xamarin.Android 바인딩 생성기 및 C# 래퍼 클래스 생성 기능을 방해할 수 있습니다. 다음 코드 조각에서는 **Metadata.xml**을 업데이트하여 클래스 이름의 난독 처리를 해제하는 방법을 보여 줍니다.

  ```xml
  <attr path="/api/package[@name='{package_name}']/class[@name='{name}']"
      name="obfuscated">false</attr>
  ```

### <a name="problem-generated-c-source-does-not-build-due-to-parameter-type-mismatch"></a>문제: 매개 변수 형식이 일치하지 않아 생성된 C# 소스가 빌드되지 않습니다.

생성된 C# 소스가 빌드되지 않습니다. 재정의된 메서드의 매개 변수 형식이 일치하지 않습니다.

#### <a name="possible-causes"></a>가능한 원인:

Xamarin.Android에는 C# 바인딩의 열거형에 매핑되는 다양한 Java 필드가 포함되어 있습니다. 이로 인해 생성된 바인딩에서 형식이 호환되지 않을 수 있습니다. 이 문제를 해결하려면 바인딩 생성기에서 만든 메서드 시그니처를 열거형을 사용하도록 수정해야 합니다. 자세한 내용은 [열거형 수정](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)을 참조하세요.

### <a name="problem-noclassdeffounderror-in-packaging"></a>문제: 패키징의 NoClassDefFoundError

패키징 단계에서 `java.lang.NoClassDefFoundError`가 throw됩니다.

#### <a name="possible-causes"></a>가능한 원인:

이 오류의 가장 가능성 높은 이유는 필수 Java 라이브러리를 애플리케이션 프로젝트( **.csproj**)에 추가해야 한다는 것입니다. .JAR 파일은 자동으로 확인되지 않습니다. Java 라이브러리 바인딩이 대상 디바이스 또는 에뮬레이터에 없는 사용자 어셈블리에 대해 항상 생성되는 것은 아닙니다(예: Google 지도 **maps.jar**). 라이브러리 .JAR이 라이브러리 dll에 포함되어 있으므로 Android 라이브러리 프로젝트 지원은 여기에 해당되지 않습니다. 예를 들어: [버그 4288](https://bugzilla.xamarin.com/show_bug.cgi?id=4288)

### <a name="problem-duplicate-custom-eventargs-types"></a>문제: 중복된 사용자 지정 EventArgs 형식

중복된 사용자 지정 EventArgs 형식으로 인해 빌드가 실패합니다. 다음과 같은 오류가 발생합니다.

```shell
error CS0102: The type `Com.Google.Ads.Mediation.DismissScreenEventArgs' already contains a definition for `p0'
```

#### <a name="possible-causes"></a>가능한 원인:

이는 동일한 이름을 가진 메서드를 공유하는 둘 이상의 인터페이스 "listener" 형식에서 제공하는 이벤트 형식 간에 충돌이 발생하기 때문입니다. 예를 들어, 아래 예제에서 볼 수 있는 것처럼 두 개의 Java 인터페이스가 있는 경우 생성기는 `MediationBannerListener` 및 `MediationInterstitialListener`에 대해 `DismissScreenEventArgs`를 생성하므로 오류가 발생합니다.

```java
// Java:
public interface MediationBannerListener {
    void onDismissScreen(MediationBannerAdapter p0);
}
public interface MediationInterstitialListener {
    void onDismissScreen(MediationInterstitialAdapter p0);
}
```

이 오류는 이벤트 인수 형식에 대해 긴 이름을 피하도록 의도적으로 설계된 것입니다. 해당 충돌을 방지하기 위해 메타데이터 변환이 필요합니다. **Transforms\Metadata.xml**을 편집하고 인터페이스 중 하나(또는 인터페이스 메서드)에 `argsType` 특성을 추가합니다.

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

### <a name="problem-class-does-not-implement-interface-method"></a>문제: 클래스가 인터페이스 멤버를 구현하지 않습니다.

생성된 클래스가 생성된 클래스에서 구현하는 인터페이스에 필요한 메서드를 구현하지 않음을 나타내는 오류 메시지가 생성됩니다. 그러나 생성된 코드를 살펴보면 메서드가 구현된 것을 볼 수 있습니다.

다음은 오류 예입니다.

```shell
obj\Debug\generated\src\Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.cs(8,23):
error CS0738: 'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter' does not
implement interface member 'Oauth.Signpost.Http.IHttpRequest.Unwrap()'.
'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.Unwrap()' cannot implement
'Oauth.Signpost.Http.IHttpRequest.Unwrap()' because it does not have the matching
return type of 'Java.Lang.Object'
```

#### <a name="possible-causes"></a>가능한 원인:

Java 메서드를 공변 반환 형식에 바인딩할 때 발생하는 문제입니다. 이 예제에서 메서드 `Oauth.Signpost.Http.IHttpRequest.UnWrap()`은 `Java.Lang.Object`를 반환해야 합니다. 그러나 메서드 `Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.UnWrap()`의 반환 형식은 `HttpURLConnection`입니다. 이를 해결하는 방법에는 두 가지가 있습니다.

- `HttpURLConnectionRequestAdapter`에 대한 partial 클래스 선언을 추가하고 `IHttpRequest.Unwrap()`을 명시적으로 구현합니다.

  ```csharp
  namespace Oauth.Signpost.Basic {
      partial class HttpURLConnectionRequestAdapter {
          Java.Lang.Object OauthSignpost.Http.IHttpRequest.Unwrap() {
              return Unwrap();
          }
      }
  }
  ```

- 생성된 C# 코드에서 공변성(Covariance)을 제거합니다. 이 과정에서 **Transforms\Metadata.xml**에 다음 변환이 추가되어 생성된 C# 코드에 `Java.Lang.Object` 반환 형식이 지정됩니다.

  ```xml
  <attr
      path="/api/package[@name='oauth.signpost.basic']/class[@name='HttpURLConnectionRequestAdapter']/method[@name='unwrap']"
      name="managedReturn">Java.Lang.Object
  </attr>
  ```

### <a name="problem-name-collisions-on-inner-classes--properties"></a>문제: 내부 클래스/속성에서 이름 충돌 발생

상속된 개체에 대한 가시성이 충돌합니다.

Java에서 파생 클래스의 가시성이 부모와 같을 필요는 없습니다. Java에서 자동으로 수정됩니다. C#에서는 명시적이어야 하므로 계층 구조의 모든 클래스에 적절한 가시성이 유지되는지 확인해야 합니다. 다음 예제에서는 Java 패키지 이름을 `com.evernote.android.job`에서 `Evernote.AndroidJob`으로 변경하는 방법을 보여 줍니다.

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

### <a name="problem-a-so-library-required-by-the-binding-is-not-loading"></a>문제: 바인딩에 필요한 **.so** 라이브러리가 로드되지 않습니다.

일부 바인딩 프로젝트는 **.so** 라이브러리의 기능에 의존할 수도 있습니다. Xamarin.Android가 **.so** 라이브러리를 자동으로 로드하지 않을 수 있습니다. 래핑된 Java 코드가 실행될 때 Xamarin.Android는 JNI 호출을 수행하지 못하고 _java.lang.UnsatisfiedLinkError: 네이티브 메서드를 찾을 수 없음:_ 오류 메시지가 애플리케이션에 대한 로그(logcat)에 표시됩니다.

이 문제의 해결 방법은 `Java.Lang.JavaSystem.LoadLibrary`를 호출하여 **.so** 라이브러리를 수동으로 로드하는 것입니다. 예를 들어, Xamarin.Android 프로젝트에 **EmbeddedNativeLibrary** 빌드 작업에 대한 바인딩 프로젝트에 공유 라이브러리 **libpocketsphinx_jni.so**가 있다고 가정할 경우 다음 코드 조각(공유 라이브러리를 사용하기 전에 실행)은 **.so** 라이브러리를 로드합니다.

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="summary"></a>요약

이 문서에서는 Java 바인딩과 관련된 일반적인 문제 해결 및 해결 방법에 대해 설명했습니다.

## <a name="related-links"></a>관련 링크

- [라이브러리 프로젝트](https://developer.android.com/tools/projects/index.html#LibraryProjects)
- [JNI 사용](~/android/platform/java-integration/working-with-jni.md)
- [진단 출력 사용](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)
- [Android 개발자용 Xamarin](~/android/get-started/java-developers.md)
- [JD-GUI](http://jd.benow.ca/)
