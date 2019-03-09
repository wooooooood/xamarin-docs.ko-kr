---
title: 바인딩 문제 해결
description: 이 문서는 가능한 원인 및 해결 하는 방법이 제안된 함께 바인딩을 생성할 때 발생할 수 있는 몇 가지 일반적인 오류를 요약 합니다.
ms.prod: xamarin
ms.assetid: BB81FCCF-F7BF-4C78-884E-F02C49AA819A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: b0bb7cbb6160865af5b1e40d40c7b999a8bd5ebc
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668519"
---
# <a name="troubleshooting-bindings"></a>바인딩 문제 해결

_이 문서는 가능한 원인 및 해결 하는 방법이 제안된 함께 바인딩을 생성할 때 발생할 수 있는 몇 가지 일반적인 오류를 요약 합니다._


## <a name="overview"></a>개요

Android 라이브러리 바인딩 (을 **.aar** 또는 **.jar**) 파일은 거의 간단 하 게 회의; 일반적으로 Java와.NET 간의 차이 통해 발생 하는 문제를 완화 하려면 추가적인 노력이 필요 합니다.
이러한 문제는 Xamarin.Android에서 Android 라이브러리 바인딩 방지를 빌드 로그에 오류 메시지 자체를 표시 합니다. 이 가이드는 문제 해결을 위한 몇 가지 팁을 제공, 몇 가지 일반적인 문제/시나리오 목록를 Android 라이브러리를 성공적으로 바인딩할 가능한 솔루션을 제공 합니다.

기존 Android 라이브러리를 바인딩하는 경우 다음 사항을 유의 해야 하는:

- **라이브러리에 대 한 외부 종속성** &ndash; Android 라이브러리에 필요한 모든 Java 종속성을 Xamarin.Android 프로젝트에 포함 되어야 합니다는 **ReferenceJar** 로  **EmbeddedReferenceJar**합니다.

- **Android 라이브러리를 대상으로 하는 Android API 레벨** &ndash; "다운 그레이드할" Android API 수준 수 없으며, Xamarin.Android 바인딩 프로젝트에는 수준 (이상) 동일한 API를 대상으로 하는지 확인 하는 Android 라이브러리와 합니다.

- **Android 라이브러리 패키지를 사용한 Android JDK 버전이** &ndash; Android 라이브러리 Xamarin.Android에서 사용 중인 것 보다 JDK의 다른 버전으로 작성 된 경우 바인딩 오류에 발생할 수 있습니다. 가능한 경우 동일한 버전의 Xamarin.Android 설치에서 사용 되는 JDK 사용 하 여 Android 라이브러리를 다시 컴파일하십시오.

Xamarin.Android 라이브러리 바인딩 문제를 해결 하는 첫 번째 단계를 사용 하도록 설정 하는 것 [진단 MSBuild 출력](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)합니다.
진단 출력을 사용 하도록 설정한 후 Xamarin.Android 바인딩 프로젝트를 다시 작성 하 고 문제의 원인을 사항에 대 한 단서를 찾을 빌드 로그를 검사 합니다.

Android 라이브러리 디컴파일 형식과 Xamarin.Android는 바인딩하려고 하는 메서드를 검사 하도 유용할 수 있습니다. 이 가이드에서 나중에 자세히 설명 합니다.


## <a name="decompiling-an-android-library"></a>Android 라이브러리 디컴파일

클래스와 Java 클래스의 메서드를 검사 라이브러리를 바인딩하는 데 도움이 되는 중요 한 정보를 제공할 수 있습니다.
[JD GUI](http://jd.benow.ca/) 는 Java 소스 코드에서 표시할 수 있는 그래픽 유틸리티는 **클래스** JAR에 포함 된 파일입니다. 실행할 수 있습니다 독립 실행형 응용 프로그램 또는 플러그 인으로 IntelliJ 또는 Eclipse에 대 한 합니다.

Android 라이브러리 열린 디컴파일에 **합니다. JAR** Java 디컴파일러를 사용 하 여 파일입니다. 이면는 **합니다. AAR** 파일을 추출 하는 데 필요한 것이 파일인 **classes.jar** 보관 파일에서 합니다. 다음은 샘플 스크린샷 JD GUI를 사용 하 여 분석 합니다 [피카소](http://square.github.io/picasso/) JAR:

![Java 디컴파일러를 사용 하 여 피카소 2.5.2.jar를 분석 합니다.](troubleshooting-bindings-images/troubleshoot-bindings-01.png)

Android 라이브러리 디컴파일된가, 되 면 소스 코드를 검사 합니다. 일반적으로 찾습니다.

- **난독 처리의 특성이 있는 클래스** &ndash; 난독 처리 된 클래스의 특성을 포함 합니다.

    - 클래스 이름에는 **$**, 즉 **$.class**
    - 클래스 이름을 완전히 손상 된 소문자 문자 즉, **a.class**      

- **`import` 참조 되지 않은 라이브러리에 대 한 문을** &ndash; 참조 되지 않은 라이브러리를 식별 하 고 해당 종속성을 사용 하 여 Xamarin.Android 바인딩 프로젝트에 추가 된 **빌드 작업** 의 **ReferenceJar**  나 **EmbedddedReferenceJar**합니다.

> [!NOTE]
> Java 라이브러리 디컴파일 금지 되었거나 현지 법률 또는 Java 라이브러리를 게시 라이선스에 따라 법적 제한 될 수 있습니다. 필요한 경우 디컴파일 Java 라이브러리의 소스 코드 검사를 시도 하기 전에 법률 전문가의 서비스를 등록 합니다.


## <a name="inspect-apixml"></a>API를 검사 합니다. XML

Xamarin.Android 바인딩 프로젝트 빌드의 일부로, XML 파일 이름이 생성 됩니다 **obj/Debug/api.xml**:

![Obj/Debug에서 생성 된 api.xml](troubleshooting-bindings-images/troubleshoot-bindings-02.png)

이 파일에 Xamarin.Android의 바인딩을 시도 하는 모든 Java Api의 목록을 제공 합니다. 이 파일의 내용을 누락 된 형식 또는 메서드를 식별, 중복 된 바인딩 도움이 됩니다. 이 파일의 검사가 지루하고 시간이 많이 걸릴 경우에 원인이 무엇 바인딩 문제에 대 한 단서를 제공할 수 있습니다. 예를 들어 **api.xml** 는 속성을 부적절 한 형식을 반환 하거나 관리 되는 동일한 이름을 공유 하는 형식을 두 개의 드러날 수 있습니다.


## <a name="known-issues"></a>알려진 문제

이 섹션에서는 일반적인 오류 메시지 또는 증상의 일부 표시 하는 내 Android 라이브러리를 바인딩하는 동안 발생 합니다.


### <a name="problem-java-version-mismatch"></a>문제점: Java 버전이 일치 하지 않습니다.

형식이 생성 되지 않을 경우에 따라 또는 예기치 않은 작동 중단 중 어떤 라이브러리를 사용 하 여 컴파일된 비교할 Java의 최신 또는 이전 버전을 사용 하 고 있으므로 발생할 수 있습니다. 동일한 버전의 Xamarin.Android 프로젝트를 사용 하는 JDK 사용 하 여 Android 라이브러리를 다시 컴파일하십시오.


### <a name="problem-at-least-one-java-library-is-required"></a>문제점: Java 라이브러리를 하나 이상 필요합니다.

오류가 나타나면 "Java 라이브러리를 하나 이상 필요 하다"도 합니다. JAR 추가 되었습니다.

#### <a name="possible-causes"></a>가능한 원인:

빌드 작업 설정 되어 있는지 확인 `EmbeddedJar`합니다. 때문에 여러 빌드 작업이 있습니다. JAR 파일 (같은 `InputJar`, `EmbeddedJar`, `ReferenceJar` 고 `EmbeddedReferenceJar`), 바인딩 생성기 기본적으로 사용 하는 것을 자동으로 추측할 수 없습니다. 빌드 작업에 대 한 자세한 내용은 참조 하세요. [빌드 작업](~/android/platform/binding-java-library/index.md)합니다.


### <a name="problem-binding-tools-cannot-load-the-jar-library"></a>문제점: 바인딩 도구를 로드할 수 없습니다 합니다. JAR 라이브러리

바인딩 라이브러리 생성기 로드에 실패 합니다. JAR 라이브러리입니다.

#### <a name="possible-causes"></a>가능한 원인

일부입니다. Java 도구에서 (Proguard 같은 도구)를 통해 코드 난독 처리를 사용 하는 JAR 라이브러리를 로드할 수 없습니다. 도구인 Java 리플렉션 사용 하 고 라이브러리를 엔지니어링 ASM 바이트 코드가 있으므로 Android 런타임 도구에 전달할 수 있지만 이러한 종속 도구 난독 처리 된 라이브러리를 거부할 수 있습니다. 이 해결 방법은 직접-바인딩 바인딩 생성기를 사용 하는 대신 이러한 라이브러리입니다.



### <a name="problem-missing-c-types-in-generated-output"></a>문제점: 누락 된 C# 에서 생성 된 출력 형식입니다.

바인딩을 **.dll** 작성 하지만 일부 Java 형식 또는 생성 된 누락 C# 원본 형식은 누락 되었다는 오류로 인해 빌드하지 않습니다.

#### <a name="possible-causes"></a>가능한 원인:

이 오류는 아래와 같은 몇 가지 이유로 인해 발생할 수 있습니다.

-   바인딩되는 라이브러리는 두 번째 Java 라이브러리를 참조할 수 있습니다. 바인딩된 라이브러리에 대 한 공용 API에서는 형식에서 두 번째 라이브러리도 두 번째 라이브러리에 대 한 관리 되는 바인딩을 참조 해야 합니다.

-   라이브러리를 메타 데이터를 로드 하는 예기치 않은 발생 위에 라이브러리 로드 오류의 원인을 비슷합니다 Java 리플렉션을 인해 삽입 된는 가능성이 있습니다. Xamarin.Android의 도구는이 문제를 해결 현재 없습니다. 이러한 경우 라이브러리를 수동으로 바인딩해야 합니다.

-   .NET 4.0 런타임이 있어야 하는 경우 어셈블리를 로드 하지 못한에 버그가 있었습니다. 이 문제는.NET 4.5 런타임은에서 수정 되었습니다.

-   Java public이 아닌 클래스에서 공용 클래스를 파생할 수 있습니다. 하지만이.NET에서 지원 되지 않습니다. Public이 아닌 클래스에 대 한 바인딩을 바인딩 생성기를 생성 하지 않습니다, 올바르게 생성할 수 없습니다. 이러한 같은 클래스 파생 합니다. 노드 제거를 사용 하 여 해당 파생된 클래스에 대 한 메타 데이터 항목을 제거 하거나이 해결 하려면 **Metadata.xml**, 또는 public이 아닌 클래스를 공개 하는 메타 데이터를 수정 합니다. 후자의 경우 바인딩을 만들려면는 있지만 있도록는 C# 소스는 빌드, public이 아닌 클래스를 사용할 수 없습니다.

    예를 들어:

    ```xml
    <attr path="/api/package[@name='com.some.package']/class[@name='SomeClass']"
        name="visibility">public</attr>
    ```

-   Java 라이브러리를 난독 처리 하는 도구 Xamarin.Android 바인딩 생성기를 생성 하는 기능을 방해할 수 C# 래퍼 클래스입니다. 다음 코드 조각은 업데이트 하는 방법을 보여 줍니다 **Metadata.xml** unobfuscate 클래스 이름에:

    ```xml
    <attr path="/api/package[@name='{package_name}']/class[@name='{name}']"
        name="obfuscated">false</attr>
    ```

### <a name="problem-generated-c-source-does-not-build-due-to-parameter-type-mismatch"></a>문제점: 생성 된 C# 매개 변수 유형이 일치 하지 않아 소스를 빌드하지 않습니다

생성 된 C# 소스를 빌드하지 않습니다. 형식이 일치 하지 않는 메서드의 매개 변수를 재정의 합니다.

#### <a name="possible-causes"></a>가능한 원인:

Xamarin.Android는 다양 한 Java 필드에서 열거형에 매핑되는 C# 바인딩. 생성 된 바인딩에서 형식 비 호환성 문제가 발생할 수 있습니다 이러한. 이 해결 하려면 바인딩 생성기에서 생성 하는 메서드 시그니처는 열거형을 사용 하도록 수정 해야 합니다. 자세한 정보를 참조 하세요 [수정 열거형](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)합니다.

### <a name="problem-noclassdeffounderror-in-packaging"></a>문제점: 패키지에 NoClassDefFoundError

`java.lang.NoClassDefFoundError` 패키징 단계에서 throw 됩니다.

#### <a name="possible-causes"></a>가능한 원인:

가장 가능성이 높은이 오류의 원인을 필수 Java 라이브러리 응용 프로그램 프로젝트에 추가 해야 한다는 것 (**.csproj**). . JAR 파일을 자동으로 확인 되지 않습니다. Java 라이브러리 바인딩 대상 장치 또는 에뮬레이터에서 존재 하지 않는 사용자 어셈블리에 대해 생성 되지 않습니다 (Google Maps 등 **maps.jar**). 이 아닌 경우 Android 라이브러리 프로젝트 지원에 대 한 사례를 라이브러리로 JAR는 라이브러리 dll에 포함 됩니다. 예를 들어: [Bug 4288](https://bugzilla.xamarin.com/show_bug.cgi?id=4288)

### <a name="problem-duplicate-custom-eventargs-types"></a>문제점: 중복 된 사용자 지정 EventArgs 형식

빌드는 사용자 지정 EventArgs 형식을 중복으로 인해 실패합니다. 다음과 같은 오류가 발생합니다.

```shell
error CS0102: The type `Com.Google.Ads.Mediation.DismissScreenEventArgs' already contains a definition for `p0'
```

#### <a name="possible-causes"></a>가능한 원인:

동일한 이름의 메서드를 공유 하는 둘 이상의 인터페이스 "수신기" 형식에서 제공 되는 이벤트 형식 간의 몇 가지 충돌이 발생 때문입니다. 예를 들어 있는 경우 두 개의 Java 인터페이스 아래 예에서 볼 수 있듯이, 생성기를 만듭니다 `DismissScreenEventArgs` 둘 다에 대해 `MediationBannerListener` 및 `MediationInterstitialListener`, 오류가 발생 합니다.

```java
// Java:
public interface MediationBannerListener {
    void onDismissScreen(MediationBannerAdapter p0);
}
public interface MediationInterstitialListener {
    void onDismissScreen(MediationInterstitialAdapter p0);
}
```

이것은 의도적인 긴 이름 이벤트 인수 형식에서 발생 하지 않으며 되도록 합니다. 이러한 충돌을 방지 하려면 일부 메타 데이터 변환 해야 합니다. 편집할 [ **Transforms\Metadata.xml** ](https://github.com/xamarin/monodroid-samples/blob/master/AdMob/AdMob/Transforms/Metadata.xml) 추가 `argsType` 특성 인터페이스 중 하나 (또는 인터페이스 메서드):

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

### <a name="problem-class-does-not-implement-interface-method"></a>문제점: 클래스는 인터페이스 메서드를 구현 하지 않습니다.

오류 메시지를 나타내는 생성된 된 클래스 생성된 된 클래스를 구현 하는 인터페이스에 필요한 메서드를 구현 하지 않습니다 생성 됩니다. 그러나 생성된 된 코드를 살펴보면 메서드를 구현 하는 볼 수 있습니다.

오류의 예로 다음과 같습니다.

```shell
obj\Debug\generated\src\Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.cs(8,23):
error CS0738: 'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter' does not
implement interface member 'Oauth.Signpost.Http.IHttpRequest.Unwrap()'.
'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.Unwrap()' cannot implement
'Oauth.Signpost.Http.IHttpRequest.Unwrap()' because it does not have the matching
return type of 'Java.Lang.Object'
```

#### <a name="possible-causes"></a>가능한 원인:

이것이 공변 (covariant) 반환 형식으로 Java 메서드 바인딩을 사용 하 여 발생 하는 문제입니다. 이 예제에서는 메서드 `Oauth.Signpost.Http.IHttpRequest.UnWrap()` 를 반환 해야 `Java.Lang.Object`합니다. 그러나 메서드 `Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.UnWrap()` 의 반환 형식이 `HttpURLConnection`합니다. 두 가지 방법으로이 문제를 해결 하려면:

-   추가 대 한 partial 클래스 선언을 `HttpURLConnectionRequestAdapter` 명시적으로 구현 하 고 `IHttpRequest.Unwrap()`:

    ```csharp
    namespace Oauth.Signpost.Basic {
        partial class HttpURLConnectionRequestAdapter {
            Java.Lang.Object OauthSignpost.Http.IHttpRequest.Unwrap() {
                return Unwrap();
            }
        }
    }
    ```

-   생성 된 공 분산을 제거 C# 코드입니다. 다음 변환을 추가 하는 것이 **Transforms\Metadata.xml** 생성 된 그러면 C# 코드의 반환 형식을 갖도록 `Java.Lang.Object`:

    ```xml
    <attr
        path="/api/package[@name='oauth.signpost.basic']/class[@name='HttpURLConnectionRequestAdapter']/method[@name='unwrap']"
        name="managedReturn">Java.Lang.Object
    </attr>
    ```

### <a name="problem-name-collisions-on-inner-classes--properties"></a>문제점: 이름 충돌 내부 클래스 / 속성

상속 된 개체에 충돌 하는 표시 유형입니다.

Java이 든, 필요 하지는 파생된 클래스는 부모 표시 유형이 같이 있는 합니다. Java를는 수정 하면 됩니다. C#명확히 지정할 수 있는, 계층 구조의 모든 클래스는 적절 한 볼 수 있는지 확인 해야 합니다. 다음 예제에서 Java 패키지 이름을 변경 하는 방법을 보여 줍니다 `com.evernote.android.job` 에 `Evernote.AndroidJob`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

### <a name="problem-a-so-library-required-by-the-binding-is-not-loading"></a>문제점: A **.so** 바인딩에서 필요한 라이브러리는 로드 되지 않고

일부 바인딩 프로젝트의 기능에 따라 달라질 수도 있습니다는 **.so** 라이브러리입니다. Xamarin.Android 자동으로 로드 되지 것입니다 불가능 합니다 **.so** 라이브러리입니다. Xamarin.Android JNI 호출 및 오류 메시지에 실패 하기 래핑된 Java 코드를 실행 하는 경우 _java.lang.UnsatisfiedLinkError: 네이티브 메서드를 찾을 수 없음:_ 응용 프로그램에 대 한 아웃 logcat에 나타납니다.

이 해결 수동으로 로드 하는 것은 **.so** 라이브러리를 호출 하 여 `Java.Lang.JavaSystem.LoadLibrary`입니다. 예를 들어 Xamarin.Android 프로젝트는 공유 라이브러리를 가정 **libpocketsphinx_jni.so** 빌드 작업을 사용 하 여 바인딩 프로젝트에 포함 된 **EmbeddedNativeLibrary**, 다음 코드 조각 (공유 라이브러리를 사용 하기 전에 실행)가 로드 된 **.so** 라이브러리:

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="summary"></a>요약

이 문서에서는 Java Bindings와 관련 된 일반적인 문제를 나열 하 고 해결 하는 방법을 설명 합니다.


## <a name="related-links"></a>관련 링크

- [라이브러리 프로젝트](https://developer.android.com/tools/projects/index.html#LibraryProjects)
- [Jni 작업](~/android/platform/java-integration/working-with-jni.md)
- [진단 출력을 사용 하도록 설정](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)
- [Android 개발자를 위한 Xamarin](~/android/get-started/java-developers.md)
- [JD-GUI](http://jd.benow.ca/)
