---
title: '연습: Android Kotlin 라이브러리 바인딩'
description: 이 문서에서는 기존 Kotlin 라이브러리, BubblePicker에 대 한 Xamarin Android 바인딩을 만드는 실습 연습을 제공 합니다. Kotlin 라이브러리 컴파일, 바인딩, Xamarin Android 응용 프로그램에서 바인딩을 사용 하는 등의 항목을 다룹니다.
ms.prod: xamarin
ms.assetid: 5E45451F-514F-4F2B-A6E8-599ED2EFDA2C
ms.technology: xamarin-android
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: cbd7c796cd13aa45dc107bddf06ca44d6adbdf9d
ms.sourcegitcommit: fec87846fcb262fc8b79774a395908c8c8fc8f5b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77519680"
---
# <a name="walkthrough-bind-an-android-kotlin-library"></a>연습: Android Kotlin 라이브러리 바인딩

Xamarin을 사용 하면 모바일 개발자가 Visual Studio 및 C#를 사용 하 여 플랫폼 간 네이티브 모바일 앱을 만들 수 있습니다. Android platform SDK 구성 요소를 즉시 사용할 수 있지만 대부분의 경우 해당 플랫폼에 대해 작성 된 타사 Sdk를 사용 하 고 Xamarin을 통해 바인딩을 통해 수행할 수 있습니다. Xamarin android 응용 프로그램에 타사 Android 프레임 워크를 통합 하려면 응용 프로그램에서 사용 하기 전에 Xamarin android 바인딩을 만들어야 합니다.

Android 플랫폼은 기본 언어 및 도구와 함께 Kotlin 언어의 최근 소개를 비롯 하 여 지속적으로 진화 하 고 있으며 최종적으로 Java를 대체 하도록 설정 됩니다. Java에서 Kotlin로 이미 마이그레이션된 많은 3d 파티 Sdk가 있으며 새로운 과제가 제공 됩니다. Kotlin 바인딩 프로세스가 Java와 유사 하더라도 Xamarin Android 응용 프로그램의 일부로 성공적으로 빌드하고 실행 하려면 추가 단계 및 구성 설정이 필요 합니다.  

이 문서의 목표는이 시나리오를 해결 하기 위한 개략적인 접근 방식을 간략하게 설명 하 고 간단한 예제를 사용 하 여 자세한 단계별 가이드를 제공 하는 것입니다.

## <a name="background"></a>배경

Kotlin는 2016 년 2 월에 출시 되었으며 표준 Java 컴파일러에 대 한 대 안으로 2017에서 Android Studio로 배치 되었습니다. 이후 2019에서 Google은 Kotlin 프로그래밍 언어가 Android 앱 개발자에 게 선호 되는 언어 임을 발표 했습니다. 높은 수준의 바인딩 방식은 몇 가지 중요 한 Kotlin 특정 단계를 포함 하는 [일반적인 Java 라이브러리의 바인딩 프로세스](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/) 와 비슷합니다.

## <a name="prerequisites"></a>사전 요구 사항

이 연습을 완료하려면 다음이 필요합니다.

- [Android Studio](https://developer.android.com/studio)
- [Mac용 Visual Studio](https://visualstudio.microsoft.com/downloads)
- [Java 디컴파일러](http://java-decompiler.github.io/)

## <a name="build-a-native-library"></a>네이티브 라이브러리 빌드

첫 번째 단계는 Android Studio를 사용 하 여 네이티브 Kotlin 라이브러리를 빌드하는 것입니다. 라이브러리는 일반적으로 타사 개발자가 제공 하거나 [Google의 Maven 리포지토리](https://maven.google.com/web/index.html) 및 기타 원격 리포지토리에서 사용할 수 있습니다. 예를 들어이 자습서에서는 거품형 선택기 Kotlin 라이브러리에 대 한 바인딩을 만듭니다.

![GitHub BubblePicker 데모](walkthrough-images/github-bubblepicker-demo.png)

1. 라이브러리에 대 한 GitHub에서 [소스 코드를](https://github.com/igalata/Bubble-Picker/archive/develop.zip) 다운로드 하 고 로컬 폴더에 압축을 풉니다. **선택**합니다.

1. Android Studio를 시작 하 고, 거품형 선택기 로컬 폴더를 선택 하 여 **기존 Android Studio 프로젝트 열기** 메뉴 옵션을 선택 합니다.

    ![프로젝트 열기 Android Studio](walkthrough-images/android-studio-open-project.png)

1. Gradle를 포함 하 여 Android Studio 최신 인지 확인 합니다. 소스 코드는 Android Studio v 3.5.3, Gradle v 5.4.1에서 성공적으로 빌드할 수 있습니다. Gradle를 최신 Gradle 버전으로 업데이트 하는 방법에 대 한 지침은 [여기에서 찾을](https://gradle.org/install/)수 있습니다.

1. 필수 Android SDK 설치 되어 있는지 확인 합니다. 소스 코드에는 Android SDK v25 필요 합니다. Sdk 구성 요소를 설치 하는 **도구 > Sdk Manager** 메뉴 옵션을 엽니다.

1. 프로젝트 폴더의 루트에 있는 주 **gradle** 구성 파일을 업데이트 하 고 동기화 합니다.

    - Kotlin 버전을 1.3.10로 설정 합니다.

        ```gradle
        buildscript {
            ext.kotlin_version = '1.3.10'
        }
        ```

    - 지원 라이브러리 종속성을 확인할 수 있도록 기본 Google의 Maven 리포지토리를 등록 합니다.

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

    - 구성 파일이 업데이트 되 면 동기화 되지 않으며 Gradle에서 **지금 동기화** 단추를 표시 하 고 키를 눌러 동기화 프로세스가 완료 될 때까지 기다립니다.

        ![지금 Gradle Sync Android Studio](walkthrough-images/android-studio-gradle-syncnow.png)

        > [!TIP]
        > Gradle의 종속성 캐시가 손상 되었을 수 있습니다 .이는 네트워크 연결 시간 제한 이후에 발생 하기도 합니다. 종속성을 Redownload 프로젝트를 동기화 합니다 (네트워크 필요).

        > [!TIP]
        > 디먼 (Gradle build process)의 상태가 손상 되었을 수 있습니다. Gradle 디먼를 모두 중지 하면이 문제를 해결할 수 있습니다. Gradle 빌드 프로세스를 중지 합니다 (다시 시작 해야 함). 손상 된 Gradle 프로세스의 경우 IDE를 닫은 다음 모든 Java 프로세스를 종료할 수도 있습니다.

        > [!TIP]
        > 프로젝트에서 타사 플러그 인을 사용할 수 있습니다 .이 플러그 인은 프로젝트의 다른 플러그 인 또는 프로젝트에서 요청 된 Gradle 버전과 호환 되지 않습니다.

1. 오른쪽의 Gradle 메뉴를 열고 **bubblepicker > 작업** 메뉴로 이동 하 여 **빌드** 작업을 두 번 탭 하 여 실행 하 고 빌드 프로세스가 완료 될 때까지 기다립니다.

    ![Android Studio Gradle 실행 작업](walkthrough-images/android-studio-gradle-execute-task.png)

1. 루트 폴더 파일 브라우저를 열고 빌드 폴더로 이동 합니다. **bubblepicker >-> 빌드-> 출력-> aar**, **bubblepicker-release** 파일을 bubblepicker-v로 저장 aar **aar.** ,이 파일은 나중에 바인딩 프로세스에서 사용 됩니다.

    ![Android Studio AAR 출력](walkthrough-images/android-studio-aar-output.png)

AAR 파일은 android에서이 SDK를 사용 하 여 응용 프로그램을 실행 하는 데 필요한 컴파일된 Kotlin 소스 코드 및 자산을 포함 하는 Android 보관 파일입니다.  

## <a name="prepare-metadata"></a>메타 데이터 준비

두 번째 단계는 Xamarin.ios에서 각 C# 클래스를 생성 하는 데 사용 하는 메타 데이터 변환 파일을 준비 하는 것입니다. Xamarin Android 바인딩 프로젝트는 지정 된 Android 보관 파일에서 모든 네이티브 클래스 및 멤버를 검색 한 다음 적절 한 메타 데이터를 사용 하 여 XML 파일을 생성 합니다. 그러면 수동으로 만든 메타 데이터 변환 파일이 이전에 생성 된 기준선에 적용 되어 C# 코드를 생성 하는 데 사용 되는 최종 XML 정의 파일을 만듭니다.

메타 데이터는 [XPath](https://www.w3.org/TR/xpath/) 구문을 사용 하며 바인딩 생성기에서 바인딩 어셈블리 생성에 영향을 주는 데 사용 됩니다. [Java 바인딩 메타 데이터](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata) 문서는 적용 될 수 있는 변환에 대 한 자세한 정보를 제공 합니다.

1. 빈 **메타 데이터 .xml** 파일을 만듭니다.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <metadata>
    </metadata>
    ```

1. Xml 변환 정의:

- Native Kotlin 라이브러리에는 두 가지 종속성이 있으며,이 두 가지 종속성 C# 은 세계에 노출 하지 않고 완전히 무시 하기 위해 두 변환을 정의 합니다. 중요 한 것은 네이티브 멤버가 결과 이진에서 제거 되지 않고 C# 클래스만 생성 되지 않는다는 것입니다. [Java 디컴파일러](http://java-decompiler.github.io/) 는 종속성을 식별 하는 데 사용할 수 있습니다. 도구를 실행 하 고 앞에서 만든 AAR 파일을 엽니다. 결과적으로, 모든 종속성, 값, 리소스, 매니페스트 및 클래스를 반영 하 여 Android 보관의 구조가 표시 됩니다.  

    ![Java 디컴파일러 종속성](walkthrough-images/java-decompiler-dependencies.png)

    이러한 패키지 처리를 건너뛰는 변환은 XPath 지침을 사용 하 여 정의 됩니다.

    ```xml
    <remove-node path="/api/package[starts-with(@name,'org.jbox2d')]" />
    <remove-node path="/api/package[starts-with(@name,'org.slf4j')]" />
    ```

- 네이티브 `BubblePicker` 클래스에는 `getBackgroundColor` 및 `setBackgroundColor` 라는 두 개의 메서드가 있으며 다음 변환으로 C# `BackgroundColor` 속성으로 변경 됩니다.

    ```xml
    <attr path="/api/package[@name='com.igalata.bubblepicker.rendering']/class[@name='BubblePicker']/method[@name='getBackground' and count(parameter)=0]" name="propertyName">BackgroundColor</attr>
    <attr path="/api/package[@name='com.igalata.bubblepicker.rendering']/class[@name='BubblePicker']/method[@name='setBackground' and count(parameter)=1 and parameter[1][@type='int']]" name="propertyName">BackgroundColor</attr>
    ```

- 서명 되지 않은 형식 `UInt, UShort, ULong, UByte` 특수 처리가 필요 합니다. 이러한 형식에 대해 Kotlin는 메서드 이름 및 매개 변수 형식을 자동으로 변경 합니다 .이 형식은 생성 된 코드에 반영 됩니다.

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

    또한 `UIntArray, UShortArray, ULongArray, UByteArray`와 같은 관련 유형도 Kotlin의 영향을 받습니다. 추가 접미사를 포함 하도록 메서드 이름이 변경 되 고, 매개 변수가 동일한 형식의 서명 된 버전 요소 배열에 변경 됩니다. 아래 예제에서 `UIntArray` 형식의 매개 변수는 자동으로 `int[]`으로 변환 되 고 메서드 이름이 `fooUIntArrayMethod`에서 `fooUIntArrayMethod--ajY-9A`로 변경 됩니다. 후자는 Xamarin.ios 도구에서 검색 되 고 유효한 메서드 이름으로 생성 됩니다.

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

    의미 있는 이름을 지정 하기 위해 다음 메타 데이터를 **메타 데이터**에 추가할 수 있습니다 .이 메타 데이터는 Kotlin 코드에서 원래 정의 된 이름으로 다시 업데이트 됩니다.

    ```xml
    <attr path="/api/package[@name='com.microsoft.simplekotlinlib']/class[@name='FooClass']/method[@name='fooUIntArrayMethod--ajY-9A']" name="managedName">fooUIntArrayMethod</attr>
    ```

    BubblePicker 샘플에는 서명 되지 않은 형식을 사용 하는 멤버가 없으므로 추가 변경이 필요 하지 않습니다.

- 제네릭 매개 변수를 사용 하는 Kotlin 멤버는 기본적으로 Java의 매개 변수로 변환 됩니다.`Lang.Object` 입력할. 예를 들어, Kotlin 메서드에는 T > \<제네릭 매개 변수가 있습니다.

    ```Kotlin
    public open fun <T>fooGenericMethod(value: T) : String {
    return "fooGenericMethod${value}"
    }
    ```

    Xamarin Android 바인딩이 생성 되 면 아래와 같이 메서드가에 C# 노출 됩니다.

    ```csharp
    [Register ("fooGenericMethod", "(Ljava/lang/Object;)Ljava/lang/String;", "GetFooGenericMethod_Ljava_lang_Object_Handler")]
    [JavaTypeParameters (new string[] {
        "T"
    })]

    public virtual string FooGenericMethod (Java.Lang.Object value);
    ```

    Java 및 Kotlin 제네릭은 Xamarin. Android 바인딩에서 지원 되지 않으므로 일반 API에 액세스 하 C# 는 일반화 된 메서드가 만들어집니다. 해결 방법으로 래퍼 Kotlin 라이브러리를 만들고 제네릭을 사용 하지 않고 강력한 형식의 형식으로 필요한 Api를 노출할 수 있습니다. 또는 강력한 형식의 Api를 통해 동일한 C# 방법으로 문제를 해결 하기 위해 도우미를 함께 만들 수 있습니다.

    > [!TIP]
    > 메타 데이터를 변환 하 여 생성 된 바인딩에 모든 변경 내용을 적용할 수 있습니다. [Java 라이브러리 바인딩](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/) 문서에서는 메타 데이터를 생성 하 고 처리 하는 방법에 대해 자세히 설명 합니다.

## <a name="build-a-binding-library"></a>바인딩 라이브러리 빌드

다음 단계는 Visual Studio 바인딩 템플릿을 사용 하 여 Xamarin. Android 바인딩 프로젝트를 만들고, 필요한 메타 데이터와 네이티브 참조를 추가한 다음, 프로젝트를 빌드하여 사용할 수 있는 라이브러리를 생성 하는 것입니다.

1. Mac용 Visual Studio를 열고 새 Xamarin. Android 바인딩 라이브러리 프로젝트를 만들고 이름을 지정 합니다 .이 경우 **testBubblePicker** 를 입력 하 고 마법사를 완료 합니다. Xamarin Android 바인딩 템플릿은 다음 경로에서 찾을 수 있습니다. **android > library > 바인딩 라이브러리**:

    ![Visual Studio 바인딩 만들기](walkthrough-images/visual-studio-create-binding.png)

    변환 폴더에는 다음과 같은 세 가지 주요 변환 파일이 있습니다.

    - **Metadata** – 생성 된 바인딩의 네임 스페이스를 변경 하는 등 최종 API에 대 한 변경 내용을 허용 합니다.
    - **Enumfields** – Java int 상수와 C# 열거형 간의 매핑을 포함 합니다.
    - **Enummethods** – 메서드 매개 변수 및 반환 형식을 Java int 상수에서 열거형으로 C# 변경할 수 있습니다.

    **Enumfields .xml** 및 **enumfields .xml** 파일의 빈 상태를 유지 하 고 **메타 데이터** 를 업데이트 하 여 변환을 정의 합니다.

1. 기존 **변환/메타 데이터 .xml** 파일을 이전 단계에서 만든 **metadata .xml** 파일로 바꿉니다. 속성 창에서 파일 **빌드 작업이** **TransformationFile**로 설정 되어 있는지 확인 합니다.

    ![Visual Studio 메타 데이터](walkthrough-images/visual-studio-metadata.png)

1. 1 단계에서 빌드한 **bubblepicker-v 1.0. aar** 파일을 네이티브 참조로 바인딩 프로젝트에 추가 합니다. 네이티브 라이브러리 참조를 추가 하려면 finder를 열고 Android 보관 파일이 있는 폴더로 이동 합니다. 보관 파일을 솔루션 탐색기의 Jar 폴더로 끌어서 놓습니다. 또는 Jar 폴더의 상황에 맞는 메뉴 **추가** 옵션을 사용 하 고 **기존 파일**...을 선택할 수도 있습니다. 이 연습을 위해 파일을 디렉터리에 복사 하도록 선택 합니다. **빌드 작업이** **라이브러리 라이브러리**로 설정 되었는지 확인 해야 합니다.

    ![Visual Studio 네이티브 참조](walkthrough-images/visual-studio-native-reference.png)

1. Stdlib.h NuGet 패키지에 대 한 참조를 추가 합니다. [Kotlin.](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/) 이 패키지는 Kotlin 표준 라이브러리에 대 한 바인딩입니다. 이 패키지를 사용 하지 않으면 Kotlin 라이브러리가 Kotlin 특정 형식을 사용 하지 않는 경우에만 바인딩이 작동 합니다. 그렇지 않으면 이러한 모든 멤버가에 C# 노출 되지 않고 바인딩을 사용 하려고 하는 모든 앱은 런타임에 충돌 합니다.

    > [!TIP]
    > Xamarin Android의 제한 사항으로 인해 바인딩 프로젝트별 AAR (단일 Android 보관 파일)만 바인딩 프로젝트별로 추가할 수 있습니다. 여러 AAR 파일을 포함 해야 하는 경우 각 AAR 마다 하나씩 여러 Xamarin Android 프로젝트가 필요 합니다. 이 연습의 경우이 단계의 이전 4 가지 작업을 각 보관에 대해 반복 해야 합니다. 또 다른 옵션으로 여러 Android 보관 파일을 단일 아카이브로 수동으로 병합할 수 있으며, 따라서 단일 Xamarin.ios 바인딩 프로젝트를 사용할 수 있습니다.

1. 최종 작업은 라이브러리를 빌드하고 컴파일 오류가 없는지 확인 하는 것입니다. 컴파일 오류가 발생 하는 경우 이전에 xml 변환 메타 데이터를 추가 하 여 만든 Metadata .xml 파일을 사용 하 여 주소를 지정 하 고 처리할 수 있습니다. 그러면 라이브러리 멤버를 추가, 제거 또는 이름을 변경할 수 있습니다.

## <a name="consume-the-binding-library"></a>바인딩 라이브러리 사용

마지막 단계는 Xamarin.ios 응용 프로그램에서 Xamarin. Android 바인딩 라이브러리를 사용 하는 것입니다. 새 Xamarin Android 프로젝트를 만들고, 바인딩 라이브러리에 참조를 추가 하 고, 거품형 선택 UI를 렌더링 합니다.

1. Xamarin Android 프로젝트를 만듭니다. Android **> App > Android 앱** 을 시작 지점으로 사용 **하 고 호환성** 문제를 방지 하기 위해 대상 플랫폼 옵션을 선택 합니다. 이 프로젝트를 대상으로 하는 모든 단계는 다음과 같습니다.

    ![Visual Studio 앱 만들기](walkthrough-images/visual-studio-create-app.png)

1. 바인딩 프로젝트에 프로젝트 참조를 추가 하거나 이전에 만든 DLL을 참조를 추가 합니다.

    ![Visual Studio 추가 바인딩 참조 .png](walkthrough-images/visual-studio-add-binding-reference.png)

1. 이전에 Xamarin. Android 바인딩 프로젝트에 추가한 [Stdlib.h NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/) 패키지에 대 한 참조를 추가 합니다. 런타임에 처리 해야 하는 Kotlin 특정 형식에 대 한 지원을 추가 합니다.  이 패키지가 없으면 앱은 컴파일될 수 있지만 런타임에 작동이 중단 됩니다.

    ![Visual Studio Stdlib.h NuGet 추가](walkthrough-images/visual-studio-add-stdlib-nuget.png)

1. `MainActivity`에 대 한 Android 레이아웃에 `BubblePicker` 컨트롤을 추가 합니다. **TestBubblePicker/Resources/layout/content_main** 파일을 열고 BubblePicker 컨트롤 노드를 루트 RelativeLayout 컨트롤의 마지막 요소로 추가 합니다.

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

1. 앱의 소스 코드를 업데이트 하 고 `MainActivity`에 초기화 논리를 추가 하 여 거품형 선택기 SDK를 활성화 합니다.

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

    `BubblePickerAdapter` 및 `BubblePickerListener`는 두 클래스를 처음부터 새로 만들 수 있습니다 .이 클래스는 거품형 데이터 및 컨트롤 상호 작용을 처리 합니다.

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

1. 응용 프로그램을 실행 합니다. 그러면 거품형 선택 UI가 렌더링 됩니다.

    ![BubblePicker 데모](walkthrough-images/bubble-picker-demo.png)

    이 샘플에는 요소 스타일을 렌더링 하 고 상호 작용을 처리 하는 추가 코드가 필요 하지만 `BubblePicker` 컨트롤이 성공적으로 만들어지고 활성화 되었습니다.

축하합니다! Kotlin 라이브러리를 사용 하는 Xamarin Android 앱 및 바인딩 라이브러리를 만들었습니다.  

이제 Xamarin.ios 바인딩 라이브러리를 통해 네이티브 Kotlin 라이브러리를 사용 하는 기본 Xamarin Android 응용 프로그램이 있습니다. 이 연습에서는 의도적으로 기본 예제를 사용 하 여 도입 되는 주요 개념을 보다 잘 강조 합니다. 실제 시나리오에서 더 많은 수의 Api를 노출 하 고 메타 데이터 변환을 적용 해야 하는 경우가 많습니다.

## <a name="related-links"></a>관련 링크

- [Android Studio](https://developer.android.com/studio)
- [Gradle 설치](https://gradle.org/install/)
- [Mac용 Visual Studio](https://visualstudio.microsoft.com/downloads)
- [Java 디컴파일러](http://java-decompiler.github.io/)
- [BubblePicker Kotlin 라이브러리](https://github.com/igalata/Bubble-Picker)
- [Java 라이브러리 바인딩](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/)
- [XPath](https://www.w3.org/TR/xpath/)
- [Java 바인딩 메타 데이터](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata)
- [Kotlin. Stdlib.h NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/)
- [샘플 프로젝트 리포지토리](https://github.com/alexeystrakh/xamarin-binding-kotlin-framework)
