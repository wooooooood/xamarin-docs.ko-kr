---
title: 바인딩 문제 해결
description: 이 문서에 바인딩 가능한 원인과 해결 방법이 함께 생성 하는 경우 발생할 수 있는 몇 가지 일반적인 오류 요약 되어 있습니다.
ms.prod: xamarin
ms.assetid: BB81FCCF-F7BF-4C78-884E-F02C49AA819A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: da6286eed091114c117c723f462bbb8cac77034b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting-bindings"></a>바인딩 문제 해결

_이 문서에 바인딩 가능한 원인과 해결 방법이 함께 생성 하는 경우 발생할 수 있는 몇 가지 일반적인 오류 요약 되어 있습니다._


## <a name="overview"></a>개요

Android 라이브러리 바인딩 (한 **.aar** 또는 **.jar**) 파일은 간단한 일 경우는 거의 없습니다; 일반적으로 Java 및.NET 간의 차이로 인해 발생 하는 문제를 완화 하기 위해 추가 작업이 필요 합니다.
이러한 문제는 바인딩 Android 라이브러리에서 Xamarin.Android를 작업이 방지 되 고 빌드 로그에 오류 메시지가 표시 됩니다. 이 가이드는 문제 해결을 위한 몇 가지 팁을 제공 하 고 몇 가지 일반적인 문제/시나리오를 나열 하 고를 성공적으로 Android 라이브러리를 바인딩 가능한 솔루션을 제공 됩니다.

기존 Android 라이브러리를 바인딩하는 경우에 다음 사항을 염두에 해야 합니다.

- **라이브러리에 대 한 외부 종속성** &ndash; Android 라이브러리에 필요한 모든 Java 종속성으로 Xamarin.Android 프로젝트에 포함 되어야 합니다는 **ReferenceJar** 로  **EmbeddedReferenceJar**합니다.

- **Android 라이브러리 인지 대상으로 하는 Android API 수준** &ndash; "다운 그레이드할" Android API 수준 수 없습니다; 확인 Xamarin.Android 바인딩 프로젝트의 수준 (또는 그 이상)와 동일한 API를 대상으로 Android 라이브러리입니다.

- **Android 라이브러리 패키지를 사용한 Android JDK 버전** &ndash; Android 라이브러리 Xamarin.Android에 의해 사용 중인 보다 JDK의 다른 버전으로 빌드된 경우 바인딩 오류 발생할 수 있습니다. 가능 하면 Xamarin.Android 설치에서 사용 되는 JDK의 동일한 버전을 사용 하 여 Android 라이브러리를 다시 컴파일하십시오.

바인딩 Xamarin.Android 라이브러리를 사용 하 여 문제를 해결 하는 첫 번째 단계는가 수 있도록 [진단 MSBuild 출력](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)합니다.
진단 출력을 활성화 한 다음 Xamarin.Android 바인딩 프로젝트를 다시 작성 하는 문제가 발생 하는 방법에 대 한 단서를 찾으려고 빌드 로그를 검사 하십시오.

또한 Android 라이브러리 디컴파일 하 형식과 Xamarin.Android 바인딩할 시도 하는 메서드를 검사할 유용할 수 있습니다. 이 내용은이 가이드에서 나중에 자세히 설명 합니다.


## <a name="decompiling-an-android-library"></a>Android 라이브러리 디컴파일하지

클래스 및 Java 클래스의 메서드를 검사 라이브러리에 바인딩하는 데 도움이 되는 중요 한 정보를 제공할 수 있습니다.
[JD GUI](http://jd.benow.ca/) 는에서 Java 소스 코드를 표시할 수 있는 그래픽 유틸리티는 **클래스** JAR에 포함 된 파일입니다. 그 또는 실행할 수 있는 독립 실행형 응용 프로그램으로 플러그 인으로 IntelliJ 또는 Eclipse에 대 한 합니다.

Android 라이브러리 열기 디컴파일할는 **합니다. JAR** Java 디컴파일러 인 파일입니다. 라이브러리 이면는 **합니다. AAR** 은 파일을 추출 하는 데 필요한 파일을 **classes.jar** 보관 파일에서 합니다. 다음은 샘플 스크린 샷 JD GUI를 사용 하 여 분석 하는 [피카소](http://square.github.io/picasso/) JAR:

![Java 디컴파일러 피카소 2.5.2.jar 분석을 사용 하 여](troubleshooting-bindings-images/troubleshoot-bindings-01.png)

Android 라이브러리 decompiled 있어야, 되 면 소스 코드를 검사 합니다. 일반적으로 찾습니다.

- **난독 처리 하는 특성을 갖는 클래스** &ndash; 난독 처리 된 클래스의 특성을 포함 합니다.

    - 클래스 이름에는 **$**, 즉, **$.class**
    - 클래스 이름은 소문자, 즉, 손상 완전히 **a.class**      

- **`import` 참조 되지 않는 라이브러리에 대 한 문을** &ndash; 참조 되지 않는 라이브러리를 식별 하 고 해당 종속성을 사용 하 여 바인딩 Xamarin.Android 프로젝트 추가 **빌드 작업** 의 **ReferenceJar**  또는 **EmbedddedReferenceJar**합니다.

> [!NOTE]
> Java 라이브러리 디컴파일하지 금지 되었거나 현지법 또는 Java 라이브러리를 게시 라이선스에 따라 법적 제한 사항이 적용 됩니다. 필요한 경우를 디컴파일할 Java 라이브러리 및 소스 코드를 검사 하기 전에 법적 professional의 서비스를 등록 합니다.


## <a name="inspect-apixml"></a>API를 검사 합니다. XML

Xamarin.Android은 XML 파일 이름의 생성 하는 데 바인딩 프로젝트 작성의 일부로 **obj/Debug/api.xml**:

![Obj/Debug에서 생성 된 api.xml](troubleshooting-bindings-images/troubleshoot-bindings-02.png)

이 파일 Xamarin.Android 바인딩은 중임 모든 Java Api의 목록을 제공 합니다. 이 파일의 내용에는 누락 된 형식 또는 메서드를 식별, 바인딩이 중복 되었습니다 데 도움이 됩니다. 이 파일의 검사 지루하고 시간이 오래 걸리는 경우에 원인이 무엇 바인딩 문제에 대 한 단서를 제공할 수 있습니다. 예를 들어 **api.xml** 속성은 부적절 한 형식을 반환 하거나 관리 되는 동일한 이름을 공유 하를 입력 하는 두 드러날 수 있습니다.


## <a name="known-issues"></a>알려진 문제

이 섹션은 일반적인 오류 메시지 또는 증상 중 일부를 나열 하는 내 Android 라이브러리 바인딩할 하려고 할 때 발생 합니다.


### <a name="problem-java-version-mismatch"></a>문제: Java 버전이 일치 하지 않습니다

형식이 생성 되지 않을 경우에 따라 또는 예기치 않은 작동 중단의 Java 라이브러리도 컴파일 되었습니다에 비해 최신 또는 이전 버전 중 하나를 사용 하는 때문에 발생할 수 있습니다. Xamarin.Android 프로젝트를 사용 하는 JDK의 동일한 버전을 사용 하 여 Android 라이브러리를 다시 컴파일하십시오.


### <a name="problem-at-least-one-java-library-is-required"></a>문제: Java 라이브러리를 하나 이상 있어야 있습니다.

오류가 나타나면 "하나 이상의 Java 라이브러리는 필요한 경우" 경우에 한 합니다. JAR 추가 되었습니다.

#### <a name="possible-causes"></a>가능한 원인:

빌드 작업 설정 되어 있는지 확인 `EmbeddedJar`합니다. 에 대 한 여러 빌드 동작 때문입니다. JAR 파일 (예: `InputJar`, `EmbeddedJar`, `ReferenceJar` 및 `EmbeddedReferenceJar`), 바인딩 생성기 기본적으로 사용할을 자동으로 추측할 수 없습니다. 빌드 작업에 대 한 자세한 내용은 참조 [빌드 동작](~/android/platform/binding-java-library/index.md)합니다.


### <a name="problem-binding-tools-cannot-load-the-jar-library"></a>문제: 로드할 수 없습니다 바인딩 도구는 합니다. JAR 라이브러리

로드에 실패 하는 바인딩 라이브러리 생성기는 합니다. JAR 라이브러리입니다.

#### <a name="possible-causes"></a>가능한 원인

일부입니다. Java 도구 (Proguard 등의 도구)를 통해 코드 난독 처리를 사용 하는 JAR 라이브러리를 로드할 수 없습니다. 자사 도구로 Java 리플렉션 사용 하 여와 라이브러리 엔지니어링 ASM 바이트 코드를, 이후 Android 런타임 도구에 전달할 수 있지만 이러한 종속 도구 난독 처리 된 라이브러리를 거부할 수 있습니다. 이 대 한 해결 방법은 직접 바인딩 바인딩 생성기를 사용 하는 대신 이러한 라이브러리입니다.



### <a name="problem-missing-c-types-in-generated-output"></a>문제: C# 형식 생성 된 출력에 없습니다.

바인딩 **.dll** 빌드는 하지만 일부 Java 형식 누락 또는 생성 된 C# 소스 종류가 누락 되었다는 오류로 인해 빌드하지 않습니다.

#### <a name="possible-causes"></a>가능한 원인:

이 오류는 아래와 같은 몇 가지 이유로 인해 발생할 수 있습니다.

-   바인딩 중인 라이브러리는 두 번째 Java 라이브러리를 참조할 수 있습니다. 바인딩된 라이브러리에 대 한 공용 API 형식에서 두 번째 라이브러리를 사용 하면 두 번째 라이브러리에 대 한 관리 되는 바인딩을 참조 해야 합니다.

-   라이브러리 Java 반사, 유사한 메타 데이터를 로드 하는 예기치 않은 일으키는 위에 라이브러리 로드 오류에 대 한 이유 때문에 삽입 된 것 같습니다. Xamarin.Android의 도구는이 상황을 해결할 현재 수 없습니다. 이 경우 라이브러리 수동으로 연결 해야 합니다.

-   버그에 있어야 하는 경우 어셈블리를 로드 하지 못한.NET 4.0 런타임 했습니다. 이 문제는.NET 4.5 런타임은에서 해결 되었습니다.

-   Java public이 아닌 클래스에서 공용 클래스를 파생 가능 하지만, 이것은.NET에서 지원 되지 않습니다. 바인딩 생성기에서 public이 아닌 클래스에 대 한 바인딩을 생성 하므로 파생 클래스가 이러한 올바르게 생성 될 수 없습니다. 노드 제거를 사용 하는 파생된 클래스에 대 한 메타 데이터 항목을 제거 하거나이 해결 하려면 **Metadata.xml**, 하거나 수행 하는 public이 아닌 클래스 공용 메타 데이터를 수정 합니다. 후자의 솔루션을 만들 것 바인딩 C# 소스 빌드됩니다 되도록 하지만 public이 아닌 클래스를 사용할 수 없습니다.

    예를 들어:

    ```xml
    <attr path="/api/package[@name='com.some.package']/class[@name='SomeClass']"
        name="visibility">public</attr>
    ```

-   Java 라이브러리를 난독 처리 하는 도구는 Xamarin.Android 바인딩 생성기 및 C# 래퍼 클래스를 생성 하는 기능과 방해할 수 있습니다. 다음 코드 조각에서는 업데이트 하는 방법을 보여 줍니다. **Metadata.xml** unobfuscate 클래스 이름에:

    ```xml
    <attr path="/api/package[@name='{package_name}']/class[@name='{name}']"
        name="obfuscated">false</attr>
    ```

### <a name="problem-generated-c-source-does-not-build-due-to-parameter-type-mismatch"></a>문제: 생성 된 C# 소스 매개 변수 유형이 일치 하지 않아 구축 하지 않습니다.

생성 된 C# 소스 작성 하지 않습니다. 형식이 일치 하지 않는 메서드의 매개 변수를 재정의 합니다.

#### <a name="possible-causes"></a>가능한 원인:

Xamarin.Android 다양 한 C# 바인딩에서 열거형에 매핑되는 Java 필드가 포함 되어 있습니다. 생성 된 바인딩에서 형식 비 호환성 문제가 발생할 수 있습니다 이러한. 이 해결 하려면 바인딩 생성기에서 만든 메서드 시그니처는 열거형을 사용 하도록 수정 해야 합니다. 자세한 정보를 참조 하십시오 [열거형 수정](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)합니다.

### <a name="problem-noclassdeffounderror-in-packaging"></a>문제: NoClassDefFoundError 패키징의

`java.lang.NoClassDefFoundError` 패키징 단계에서 throw 됩니다.

#### <a name="possible-causes"></a>가능한 원인:

이 오류에 대 한 가장 가능성이 높은 이유는 필수 Java 라이브러리 응용 프로그램 프로젝트에 추가 해야 한다는 (**.csproj**). . JAR 파일을 자동으로 확인 되지 않습니다. Java 라이브러리 바인딩 대상 장치 또는 에뮬레이터에 존재 하지 않는 사용자 어셈블리에 대해 생성 되지 않습니다 (Google 맵과 같은 **maps.jar**). 이 경우가 아니라면 Android 라이브러리 프로젝트 지원에 대 한 라이브러리와 합니다. JAR는 라이브러리 dll에 포함 됩니다. 예를 들어: [버그 4288](https://bugzilla.xamarin.com/show_bug.cgi?id=4288)

### <a name="problem-duplicate-custom-eventargs-types"></a>문제: 사용자 지정 EventArgs 형식 복제

빌드는 사용자 지정 중복 EventArgs 형식에 실패 했습니다. 다음과 같은 오류가 발생합니다.

```shell
error CS0102: The type `Com.Google.Ads.Mediation.DismissScreenEventArgs' already contains a definition for `p0'
```

#### <a name="possible-causes"></a>가능한 원인:

메서드 동일한 이름을 공유 하는 둘 이상의 인터페이스 "수신기" 형식에서 제공 하는 이벤트 형식 간의 몇 가지 충돌 하는 때문입니다. 예를 들어 있는 경우 두 개의 Java 인터페이스 아래 예에서 같이, 생성기를 만듭니다 `DismissScreenEventArgs` 둘 다에 대해 `MediationBannerListener` 및 `MediationInterstitialListener`, 오류가 발생 합니다.

```java
// Java:
public interface MediationBannerListener {
    void onDismissScreen(MediationBannerAdapter p0);
}
public interface MediationInterstitialListener {
    void onDismissScreen(MediationInterstitialAdapter p0);
}
```

이것은 의도적인 이벤트 인수 형식에 있는 긴 이름은 방지 되도록 합니다. 이러한 충돌을 방지 하려면 일부 메타 데이터 변환 해야 합니다. 편집 [ **Transforms\Metadata.xml** ](https://github.com/xamarin/monodroid-samples/blob/master/AdMob/AdMob/Transforms/Metadata.xml) 추가 `argsType` 특성 인터페이스 중 하나에 (또는 인터페이스 메서드의):

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

### <a name="problem-class-does-not-implement-interface-method"></a>문제: 클래스가 인터페이스 메서드를 구현 하지 않습니다.

생성된 된 클래스는 생성 된 클래스를 구현 하는 인터페이스에 필요한 메서드를 구현 하지 않습니다 나타내는 오류 메시지가 생성 됩니다. 그러나 생성된 된 코드를 살펴보면 메서드를 구현 하는 알 수 있습니다.

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

공변 반환 형식으로 Java 메서드 바인딩 사용 하 여 발생 하는 문제입니다. 이 예제에서는 메서드 `Oauth.Signpost.Http.IHttpRequest.UnWrap()` 를 반환 해야 `Java.Lang.Object`합니다. 그러나 메서드가 `Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.UnWrap()` 의 반환 형식이 `HttpURLConnection`합니다. 이 문제를 해결 하는 방법은 두 가지가 있습니다.

-   에 대 한 partial 클래스 선언 추가 `HttpURLConnectionRequestAdapter` 를 명시적으로 구현 하 고 `IHttpRequest.Unwrap()`:

    ```csharp
    namespace Oauth.Signpost.Basic {
        partial class HttpURLConnectionRequestAdapter {
            Java.Lang.Object OauthSignpost.Http.IHttpRequest.Unwrap() {
                return Unwrap();
            }
        }
    }
    ```

-   생성된 된 C# 코드에서 공 분산을 제거 합니다. 여기에 다음과 같은 변환을 추가 하 여 **Transforms\Metadata.xml** 의 반환 형식이로 생성 된 C# 코드 발생 `Java.Lang.Object`:

    ```xml
    <attr
        path="/api/package[@name='oauth.signpost.basic']/class[@name='HttpURLConnectionRequestAdapter']/method[@name='unwrap']"
        name="managedReturn">Java.Lang.Object
    </attr>
    ```

### <a name="problem-name-collisions-on-inner-classes--properties"></a>문제: 이름 내부 클래스에는 충돌 / 속성

상속 된 개체에 충돌 하는 표시 유형입니다.

Java에서 필요 하지는 파생된 클래스는 부모와 동일한 표시 유형입니다. Java만을 통해 해결할입니다. 명시적으로 지정 해야 하는 C#에서 되도록 해야 하므로 계층의 모든 클래스에는 적절 한 표시 유형을 갖습니다. 다음 예제에서 Java 패키지 이름을 변경 하는 방법을 보여 줍니다 `com.evernote.android.job` 를 `Evernote.AndroidJob`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

### <a name="problem-a-so-library-required-by-the-binding-is-not-loading"></a>문제: A **.so** 바인딩에서 필요한 라이브러리는 로드 되지 않고

일부 프로젝트의 바인딩 기능에 따라 달라질 수도 있습니다는 **.so** 라이브러리입니다. Xamarin.Android 자동으로 로드 되지 것입니다 수는 **.so** 라이브러리입니다. Xamarin.Android JNI 통화 및 오류 메시지에 실패 하기 래핑된 Java 코드를 실행 하는 경우 _java.lang.UnsatisfiedLinkError: 네이티브 메서드를 찾을 수 없음:_ 응용 프로그램에 대 한 아웃 logcat에 표시 됩니다.

이 대 한 해결 방법은 수동으로 로드는 **.so** 라이브러리를 호출 하 여 `Java.Lang.JavaSystem.LoadLibrary`합니다. 예를 들어 Xamarin.Android 프로젝트 공유 라이브러리를 가정 **libpocketsphinx_jni.so** 의 빌드 작업을 사용 하 여 바인딩 프로젝트에 포함 된 **EmbeddedNativeLibrary**, 다음 코드 조각 (공유 라이브러리를 사용 하기 전에 실행)가 로드는 **.so** 라이브러리:

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="summary"></a>요약

이 문서에서는 Java 바인딩와 관련 된 일반적인 문제 해결 관련 나열 하 고이 문제를 해결 하는 방법을 설명 합니다.


## <a name="related-links"></a>관련 링크

- [라이브러리 프로젝트](http://developer.android.com/tools/projects/index.html#LibraryProjects)
- [JNI 작업](~/android/platform/java-integration/working-with-jni.md)
- [진단 출력을 사용 하도록 설정](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)
- [Xamarin Android 개발자를 위한](~/android/get-started/java-developers.md)
- [JD-GUI](http://jd.benow.ca/)
