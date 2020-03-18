---
title: '연습: Android Kotlin 라이브러리 바인딩'
description: 이 문서에서는 기존 Kotlin 라이브러리인 BubblePicker에 대한 Xamarin.Android 바인딩을 만드는 실습 과정을 제공합니다. Kotlin 라이브러리의 컴파일 및 바인딩, Xamarin.Android 애플리케이션에서 바인딩 사용 등의 토픽을 다룹니다.
ms.prod: xamarin
ms.assetid: 5E45451F-514F-4F2B-A6E8-599ED2EFDA2C
ms.technology: xamarin-android
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: cbd7c796cd13aa45dc107bddf06ca44d6adbdf9d
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "77519680"
---
# <a name="walkthrough-bind-an-android-kotlin-library"></a>연습: Android Kotlin 라이브러리 바인딩

모바일 개발자는 Xamarin을 통해 Visual Studio 및 C#을 사용하여 플랫폼 간 네이티브 모바일 앱을 만들 수 있습니다. Android 플랫폼 SDK 구성 요소를 즉시 사용할 수 있지만 대부분의 개발자는 해당 플랫폼용으로 작성된 타사 SDK를 사용하며, Xamarin에서는 바인딩을 통해 타사 SDK를 사용할 수 있습니다. 타사 Android 프레임워크를 Xamarin.Android 애플리케이션에 통합하려면 먼저 타사 프레임워크에 대한 Xamarin.Android 바인딩을 만들어야 합니다. 그러면 애플리케이션에서 타사 프레임워크를 사용할 수 있습니다.

Android 플랫폼은 최근에 도입된 Kotlin 언어를 비롯한 기본 언어 및 도구와 함께 지속적으로 발전하고 있으며, Kotlin은 최종적으로 Java를 대체하도록 설정되었습니다. 이미 Java에서 Kotlin으로 마이그레이션된 다양한 타사 SDK가 있으며, 덕분에 새로운 과제가 생기고 있습니다. Kotlin 바인딩 프로세스는 Java와 비슷하지만, Xamarin.Android 애플리케이션의 일부로 빌드하고 실행하려면 추가 단계와 구성 설정이 필요합니다.  

이 문서의 목표는 이 시나리오를 해결하기 위한 개략적인 방법을 간략하게 설명하고 간단한 예제를 통해 자세한 단계별 가이드를 제공하는 것입니다.

## <a name="background"></a>배경

Kotlin은 2016년 2월에 출시되었으며 2017년까지 표준 Java 컴파일러의 대안으로 Android Studio에 포함되도록 포지셔닝되었습니다. 2019년 말에 Google은 Kotlin 프로그래밍 언어가 Android 앱 개발자들의 기본 언어가 될 것이라고 발표했습니다. 대략적인 바인딩 방법은 [일반적인 Java 라이브러리의 바인딩 프로세스](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/)와 비슷하지만, Kotlin에만 해당하는 몇 가지 중요한 단계가 있습니다.

## <a name="prerequisites"></a>사전 요구 사항

이 연습을 완료하려면 다음 사항이 필요합니다.

- [Android Studio](https://developer.android.com/studio)
- [Mac용 Visual Studio](https://visualstudio.microsoft.com/downloads)
- [Java 디컴파일러](http://java-decompiler.github.io/)

## <a name="build-a-native-library"></a>네이티브 라이브러리 빌드

첫 번째 단계는 Android Studio를 사용하여 네이티브 Kotlin 라이브러리를 빌드하는 것입니다. 이 라이브러리는 일반적으로 타사 개발자가 제공하거나 [Google의 Maven 리포지토리](https://maven.google.com/web/index.html) 및 기타 원격 리포지토리에서 얻을 수 있습니다. 예를 들어 이 자습서에서는 Bubble Picker Kotlin 라이브러리에 대한 바인딩을 만듭니다.

![GitHub BubblePicker 데모](walkthrough-images/github-bubblepicker-demo.png)

1. GitHub에서 라이브러리의 [소스 코드](https://github.com/igalata/Bubble-Picker/archive/develop.zip)를 다운로드하고 로컬 폴더 **Bubble-Picker**에 압축을 풉니다.

1. 다음과 같이 Android Studio를 시작하고, **기존 Android Studio 프로젝트 열기** 메뉴 옵션을 선택하여 Bubble-Picker 로컬 폴더를 선택합니다.

    ![Android Studio 프로젝트 열기](walkthrough-images/android-studio-open-project.png)

1. Gradle을 포함하여 Android Studio가 최신 버전인지 확인합니다. 소스 코드는 Android Studio v3.5.3, Gradle v5.4.1에서 성공적으로 빌드할 수 있습니다. Gradle을 최신 Gradle 버전으로 업데이트하는 방법에 대한 지침은 [여기서 찾을 수 있습니다](https://gradle.org/install/).

1. 필요한 Android SDK가 설치되어 있는지 확인합니다. 소스 코드에는 Android SDK v25가 필요합니다. **도구 > SDK 관리자** 메뉴 옵션을 열고 SDK 구성 요소를 설치합니다.

1. 다음과 같이 프로젝트 폴더의 루트에 있는 기본 **build.gradle** 구성 파일을 업데이트하고 동기화합니다.

    - Kotlin 버전을 1.3.10으로 설정

        ```gradle
        buildscript {
            ext.kotlin_version = '1.3.10'
        }
        ```

    - 지원 라이브러리 종속성을 확인할 수 있도록 다음과 같이 Google의 기본 Maven 리포지토리를 등록합니다.

        ```gradle
        allprojects {
            repositories {
                jcenter()
                maven {
                    url "https://maven.google.com"
                }
            }
        }
        ```

    - 구성 파일이 업데이트되면 비동기 상태이며, Gradle에서 **지금 동기화** 단추를 표시합니다. 이 단추를 누르고 동기화 프로세스가 완료될 때까지 기다립니다.

        ![Android Studio Gradle 지금 동기화](walkthrough-images/android-studio-gradle-syncnow.png)

        > [!TIP]
        > Gradle의 종속성 캐시가 손상될 수 있습니다. 이 현상은 네트워크 연결 시간 제한 이후에 가끔 발생합니다. 종속성을 다시 다운로드하고 프로젝트를 동기화합니다(네트워크 필요).

        > [!TIP]
        > Gradle 빌드 프로세스(디먼)의 상태가 손상될 수 있습니다. 모든 Gradle 디먼을 중지하면 이 문제가 해결될 수도 있습니다. Gradle 빌드 프로세스를 중지합니다(다시 시작해야 함). Gradle 프로세스가 손상된 경우 IDE를 닫고 모든 Java 프로세스를 종료하는 방법을 시도해 볼 수도 있습니다.

        > [!TIP]
        > 프로젝트에서 사용하는 타사 플러그 인이 프로젝트의 다른 플러그 인 또는 프로젝트에서 요청한 Gradle 버전과 호환되지 않을 수 있습니다.

1. 오른쪽에서 Gradle 메뉴를 열고, **bubblepicker > 작업** 메뉴로 이동하고, **빌드** 작업을 두 번 탭하여 실행하 고, 빌드 프로세스가 완료될 때까지 기다립니다.

    ![Android Studio Gradle 작업 실행](walkthrough-images/android-studio-gradle-execute-task.png)

1. 다음과 같이 루트 폴더 파일 브라우저를 열고 빌드 폴더로 이동합니다. **Bubble-Picker > 빌드 > 출력 -> aar**에서 **bubblepicker-release.aar** 파일을 **bubblepicker-v1.0.aar**로 저장합니다. 이 파일은 나중에 바인딩 프로세스에서 사용됩니다.

    ![Android Studio AAR 출력](walkthrough-images/android-studio-aar-output.png)

AAR 파일은 컴파일된 Kotlin 소스 코드 및 자산을 포함하는 Android 보관 스토리지 계층이며, Android에서 이 SDK를 사용하여 애플리케이션을 실행할 때 필요합니다.  

## <a name="prepare-metadata"></a>메타데이터 준비

두 번째 단계는 Xamarin.Android에서 해당 C# 클래스를 생성하는 데 사용할 메타데이터 변환 파일을 준비하는 것입니다. Xamarin.Android 바인딩 프로젝트는 지정된 Android 보관 스토리지 계층의 모든 네이티브 클래스 및 멤버를 검색한 다음, 적절한 메타데이터를 사용하여 XML 파일을 생성합니다. 그러면 수동으로 만든 메타데이터 변환 파일이 이전에 생성된 기준에 적용되어 C# 코드를 생성하는 데 사용되는 최종 XML 정의 파일이 만들어집니다.

메타데이터는  [XPath](https://www.w3.org/TR/xpath/)  구문을 사용하며, 바인딩 생성기에서 바인딩 어셈블리 생성에 영향을 주는 데 사용됩니다. [Java 바인딩 메타데이터](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata) 문서에서는 다음과 같이 적용할 수 있는 변환에 대한 자세한 정보를 제공합니다.

1. 빈 **Metadata.xml** 파일 만들기:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <metadata>
    </metadata>
    ```

1. xml 변환 정의:

- 네이티브 Kotlin 라이브러리에는 C#에 노출하지 않는 것이 좋은 두 가지 종속성이 있습니다. 이러한 두 가지 종속성을 완전히 무시하는 두 가지 변환을 정의합니다. 중요한 것은, 네이티브 멤버는 결과 이진에서 제거되지 않으며, C# 클래스만 생성되지 않는다는 점입니다. [Java 디컴파일러](http://java-decompiler.github.io/)는 종속성을 식별하는 데 사용할 수 있습니다. 도구를 실행하고 앞에서 만든 AAR 파일을 엽니다. 그 결과로 Android 보관 스토리지 계층의 구조가 표시되고 모든 종속성, 값, 리소스, 매니페스트 및 클래스가 반영됩니다.  

    ![Java 디컴파일러 종속성](walkthrough-images/java-decompiler-dependencies.png)

    이러한 패키지 처리를 건너뛰는 변환은 XPath 지침을 사용하여 다음과 같이 정의됩니다.

    ```xml
    <remove-node path="/api/package[starts-with(@name,'org.jbox2d')]" />
    <remove-node path="/api/package[starts-with(@name,'org.slf4j')]" />
    ```

- 네이티브 `BubblePicker` 클래스에는 `getBackgroundColor` 및 `setBackgroundColor` 메서드가 있으며, 다음 변환은 두 메서드를 C# `BackgroundColor` 속성으로 변경합니다.

    ```xml
    <attr path="/api/package[@name='com.igalata.bubblepicker.rendering']/class[@name='BubblePicker']/method[@name='getBackground' and count(parameter)=0]" name="propertyName">BackgroundColor</attr>
    <attr path="/api/package[@name='com.igalata.bubblepicker.rendering']/class[@name='BubblePicker']/method[@name='setBackground' and count(parameter)=1 and parameter[1][@type='int']]" name="propertyName">BackgroundColor</attr>
    ```

- 서명되지 않은 `UInt, UShort, ULong, UByte` 형식은 특수하게 처리해야 합니다. 이러한 형식의 경우 Kotlin은 메서드 이름과 매개 변수 형식을 자동으로 변경하며, 변경 내용은 생성된 코드에 반영됩니다.

    ```Kotlin
    public open fun fooUIntMethod(value: UInt) : String {
        return "fooUIntMethod${value}"
    }
    ```

    이 코드는 다음 Java 바이트 코드로 컴파일됩니다.

    ```Java byte code
    @NotNull
    public String fooUIntMethod-WZ4Q5Ns(int value) {
    return "fooUIntMethod" + UInt.toString-impl(value);
    }
    ```

    `UIntArray, UShortArray, ULongArray, UByteArray` 같은 관련 형식도 Kotlin의 영향을 받습니다. 메서드 이름은 추가 접미사를 포함하도록 변경되고, 매개 변수는 형식이 같은 서명된 버전의 요소 배열로 변경됩니다. 아래 예제에서 `UIntArray` 형식의 매개 변수는 자동으로 `int[]`로 변환되고, 메서드 이름은 `fooUIntArrayMethod`에서 `fooUIntArrayMethod--ajY-9A`로 변경됩니다. 후자는 다음과 같이 Xamarin.Android 도구에서 검색되어 유효한 메서드 이름으로 생성됩니다.

    ```Kotlin
    public open fun fooUIntArrayMethod(value: UIntArray) : String {
        return "fooUIntArrayMethod${value.size}"
    }
    ```

    이 코드는 다음 Java 바이트 코드로 컴파일됩니다.

    ```Java byte code
    @NotNull
    public String fooUIntArrayMethod--ajY-9A(@NotNull int[] value) {
        Intrinsics.checkParameterIsNotNull(value, "value");
        return "fooUIntArrayMethod" + UIntArray.getSize-impl(value);
    }
    ```

    의미 있는 이름을 지정하기 위해 다음 메타데이터를 **Metadata.xml**에 추가할 수 있습니다. 그러면 이름이 Kotlin 코드에 원래 정의된 대로 업데이트됩니다.

    ```xml
    <attr path="/api/package[@name='com.microsoft.simplekotlinlib']/class[@name='FooClass']/method[@name='fooUIntArrayMethod--ajY-9A']" name="managedName">fooUIntArrayMethod</attr>
    ```

    BubblePicker 샘플에는 서명되지 않은 형식을 사용하는 멤버가 없으므로 추가 변경이 필요하지 않습니다.

- 제네릭 매개 변수를 사용하는 Kotlin 멤버는 기본적으로 Java.`Lang.Object` 형식의 매개 변수로 변환됩니다. 예를 들어 Kotlin 메서드에는 다음과 같은 \<T> 제네릭 매개 변수가 있습니다.

    ```Kotlin
    public open fun <T>fooGenericMethod(value: T) : String {
    return "fooGenericMethod${value}"
    }
    ```

    Xamarin.Android 바인딩이 생성되면 아래와 같이 메서드가 C#에 노출됩니다.

    ```csharp
    [Register ("fooGenericMethod", "(Ljava/lang/Object;)Ljava/lang/String;", "GetFooGenericMethod_Ljava_lang_Object_Handler")]
    [JavaTypeParameters (new string[] {
        "T"
    })]

    public virtual string FooGenericMethod (Java.Lang.Object value);
    ```

    Java 및 Kotlin 제네릭은 Xamarin.Android 바인딩에서 지원되지 않으므로 제네릭 API에 액세스하는 일반화된 C# 메서드가 만들어집니다. 해결 방법으로 래퍼 Kotlin 라이브러리를 만들고, 제네릭 없이 강력한 형식 방법으로 필요한 API를 노출할 수 있습니다. 또는 강력한 형식 API를 통해 동일한 방법으로 이슈를 해결하는 도우미를 C# 쪽에서 만들 수 있습니다.

    > [!TIP]
    > 메타데이터를 변환하면 모든 변경 내용을 생성된 바인딩에 적용할 수 있습니다. [바인딩 Java 라이브러리](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/) 문서에서는 메타데이터를 생성하고 처리하는 방법을 자세히 설명합니다.

## <a name="build-a-binding-library"></a>바인딩 라이브러리 빌드

다음 단계는 Visual Studio 바인딩 템플릿을 사용하여 Xamarin.Android 바인딩 프로젝트를 만들고, 필요한 메타데이터와 네이티브 참조를 추가한 다음, 프로젝트를 빌드하여 사용 가능한 라이브러리를 만드는 것입니다.

1. Mac용 Visual Studio를 열고, 새 Xamarin.Android 바인딩 라이브러리 프로젝트를 만들고 이름을 지정합니다. 여기서는 **testBubblePicker.Binding**이라고 지정하고 마법사를 완료합니다. Xamarin.Android 바인딩 템플릿은 다음 경로에 있습니다. **Android > 라이브러리 > 바인딩 라이브러리**:

    ![Visual Studio 바인딩 만들기](walkthrough-images/visual-studio-create-binding.png)

    변환 폴더에는 다음과 같은 세 가지 주요 변환 파일이 있습니다.

    - **Metadata.xml** - 생성된 바인딩의 네임스페이스를 변경하는 등 최종 API를 변경할 수 있습니다.
    - **EnumFields.xml** - Java int 상수와 C# 열거형 간의 매핑을 포함하고 있습니다.
    - **EnumMethods.xml** - 메서드 매개 변수와 반환 형식을 Java int 상수에서 C# 열거형으로 변경할 수 있습니다.

    **EnumFields.xml** 및 **EnumMethods.xml** 파일을 계속 비워 두고 **Metadata.xml**을 업데이트하여 변환을 정의합니다.

1. 기존 **Transformations/Metadata.xml** 파일을 이전 단계에서 만든 **Metadata.xml** 파일로 바꿉니다. 속성 창에서 **빌드 작업** 파일이 **TransformationFile**로 설정되었는지 확인합니다.

    ![Visual Studio 메타데이터](walkthrough-images/visual-studio-metadata.png)

1. 1단계에서 빌드한 **bubblepicker-v1.0.aar** 파일을 바인딩 프로젝트에 네이티브 참조로 추가합니다. 네이티브 라이브러리 참조를 추가하기 위해, Finder를 열고 Android 보관 스토리지 계층이 있는 폴더로 이동합니다. 솔루션 탐색기에서 보관 액세스 계층을 Jars 폴더로 끌어서 놓습니다. 또는 Jars 폴더의 **추가** 바로 가기 메뉴 옵션을 사용하여 **기존 파일...** 을 선택할 수도 있습니다. 이 연습의 목적을 위해 파일을 디렉터리에 복사하도록 선택합니다. **빌드 작업**이 **LibraryProjectZip**으로 설정되었는지 확인합니다.

    ![Visual Studio 네이티브 참조](walkthrough-images/visual-studio-native-reference.png)

1. [Xamarin.Kotlin.StdLib NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/) 패키지에 대한 참조를 추가합니다. 이 패키지는 Kotlin 표준 라이브러리의 바인딩입니다. 이 패키지가 없으면 Kotlin 라이브러리에서 Kotlin 관련 형식을 사용하지 않는 경우에만 바인딩이 작동합니다. 사용할 경우 모든 멤버가 C#에 노출되지 않으며, 바인딩을 사용하려고 시도하는 모든 앱이 런타임에 충돌합니다.

    > [!TIP]
    > Xamarin.Android의 제한 사항으로 인해 바인딩 도구에서 바인딩 프로젝트마다 AAR(Android 보관 스토리지 계층)을 하나만 추가할 수 있습니다. 여러 AAR 파일을 포함해야 하는 경우 AAR마다 하나씩 여러 개의 Xamarin.Android 프로젝트가 필요합니다. 이 연습에서 이렇게 하려면 이 단계의 이전 4개 작업을 보관 스토리지 계층마다 반복해야 합니다. 또 다른 옵션으로 여러 Android 보관 액세스 계층을 수동으로 단일 보관 스토리지 계층으로 병합할 수 있으며, 이렇게 하면 단일 Xamarin.Android 바인딩 프로젝트를 사용할 수 있습니다.

1. 마지막 작업은 라이브러리를 빌드하고 컴파일 오류가 없는지 확인하는 것입니다. 컴파일 오류가 발생하는 경우 앞에서 xml 변환 메타데이터를 추가하여 만든 Metadata.xml 파일(라이브러리 멤버를 추가, 제거 또는 이름을 변경하는 파일)을 사용하여 오류를 확인하고 처리할 수 있습니다.

## <a name="consume-the-binding-library"></a>바인딩 라이브러리 사용

마지막 단계는 Xamarin.Android 애플리케이션에서 Xamarin.Android 바인딩 라이브러리를 사용하는 것입니다. 새 Xamarin.Android 프로젝트를 만들고, 바인딩 라이브러리에 대한 참조를 추가하고, Bubble Picker UI를 렌더링합니다.

1. Xamarin.Android 프로젝트를 만듭니다. **Android > 앱 > Android 앱**을 시작점으로 사용하고, 호환성 이슈를 방지하기 위한 대상 플랫폼 옵션으로 **가장 유용한 최신**을 선택합니다. 다음 단계는 모두 이 프로젝트를 대상으로 합니다.

    ![Visual Studio 앱 만들기](walkthrough-images/visual-studio-create-app.png)

1. 다음과 같이 바인딩 프로젝트에 대한 프로젝트 참조를 추가하거나 DLL에서 이전에 만든 참조를 추가합니다.

    ![Visual Studio 바인딩 참조 추가.png](walkthrough-images/visual-studio-add-binding-reference.png)

1. 앞에서 Xamarin.Android 바인딩 프로젝트에 추가한 [Xamarin.Kotlin.StdLib NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/) 패키지에 대한 참조를 추가합니다. 이 패키지는 런타임에 처리해야 하는 Kotlin 관련 형식에 대한 지원을 추가합니다.  이 패키지가 없으면 앱을 컴파일할 수 있지만 런타임에 앱이 충돌합니다.

    ![Visual Studio StdLib NuGet 추가](walkthrough-images/visual-studio-add-stdlib-nuget.png)

1. `MainActivity`에 대한 Android 레이아웃에 `BubblePicker` 컨트롤을 추가합니다. 다음과 같이 **testBubblePicker/Resources/layout/content_main.xml** 파일을 열고, BubblePicker 컨트롤 노드를 루트 RelativeLayout 컨트롤의 마지막 요소로 추가합니다.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout …>
        …
        <com.igalata.bubblepicker.rendering.BubblePicker
            android:id="@+id/picker"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:backgroundColor="@android:color/white" />
    </RelativeLayout>
    ```

1. 다음과 같이 앱의 소스 코드를 업데이트하고, Bubble Picker SDK를 활성화하는 초기화 논리를 `MainActivity`에 추가합니다.

    ```csharp
    protected override void OnCreate(Bundle savedInstanceState)
    {
        ...
        var picker = FindViewById<BubblePicker>(Resource.Id.picker);
        picker.BubbleSize = 20;
        picker.Adapter = new BubblePickerAdapter();
        picker.Listener = new BubblePickerListener(picker);
        ...
    }
    ```

    `BubblePickerAdapter` 및 `BubblePickerListener`는 다음과 같이 처음부터 새로 만들어야 하는 클래스이며, 거품형 데이터 및 컨트롤 상호 작용을 처리합니다.

    ```csharp
    public class BubblePickerAdapter : Java.Lang.Object, IBubblePickerAdapter
    {
        private List<string> _bubbles = new List<string>();
        public int TotalCount => _bubbles.Count;
        public BubblePickerAdapter()
        {
            for (int i = 0; i < 10; i++)
            {
                _bubbles.Add($"Item {i}");
            }
        }

        public PickerItem GetItem(int itemIndex)
        {
            if (itemIndex < 0 || itemIndex >= _bubbles.Count)
                return null;

            var result = _bubbles[itemIndex];
            var item = new PickerItem(result);
            return item;
        }
    }

    public class BubblePickerListener : Java.Lang.Object, IBubblePickerListener
    {
        public View Picker { get; }
        public BubblePickerListener(View picker)
        {
            Picker = picker;
        }

        public void OnBubbleDeselected(PickerItem item)
        {
            Snackbar.Make(Picker, $"Deselected: {item.Title}", Snackbar.LengthLong)
                .SetAction("Action", (Android.Views.View.IOnClickListener)null)
                .Show();
        }

        public void OnBubbleSelected(PickerItem item)
        {
            Snackbar.Make(Picker, $"Selected: {item.Title}", Snackbar.LengthLong)
            .SetAction("Action", (Android.Views.View.IOnClickListener)null)
            .Show();
        }
    }
    ```

1. 앱을 실행합니다. 그러면 다음과 같이 Bubble Picker UI가 렌더링됩니다.

    ![BubblePicker 데모](walkthrough-images/bubble-picker-demo.png)

    이 샘플에는 요소 스타일을 렌더링하고 상호 작용을 처리하는 추가 코드가 필요하지만 `BubblePicker` 컨트롤이 성공적으로 생성되어 활성화되었습니다.

지금까지 Kotlin 라이브러리를 사용하는 Xamarin.Android 앱과 바인딩 라이브러리를 성공적으로 만들었습니다.  

이제 Xamarin.Android 바인딩 라이브러리를 통해 네이티브 Kotlin 라이브러리를 사용하는 기본적인 Xamarin.Android 애플리케이션이 생겼습니다. 이 연습에서는 새로 도입된 핵심 개념을 강조하기 위해 의도적으로 기본 예제를 사용합니다. 실제 시나리오에서는 훨씬 많은 API를 노출하고 메타데이터 변환을 적용해야 할 가능성이 높습니다.

## <a name="related-links"></a>관련 링크

- [Android Studio](https://developer.android.com/studio)
- [Gradle 설치](https://gradle.org/install/)
- [Mac용 Visual Studio](https://visualstudio.microsoft.com/downloads)
- [Java 디컴파일러](http://java-decompiler.github.io/)
- [BubblePicker Kotlin 라이브러리](https://github.com/igalata/Bubble-Picker)
- [Java 라이브러리 바인딩](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/)
- [XPath](https://www.w3.org/TR/xpath/)
- [Java 바인딩 메타데이터](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata)
- [Xamarin.Kotlin.StdLib NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/)
- [샘플 프로젝트 리포지토리](https://github.com/alexeystrakh/xamarin-binding-kotlin-framework)
