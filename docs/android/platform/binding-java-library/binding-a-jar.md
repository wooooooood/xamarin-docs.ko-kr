---
title: JAR 바인딩
description: 이 연습에서는 Android에서 Xamarin.Android Java 바인딩 라이브러리를 만들기 위한 단계별 지침을 제공 합니다. JAR 파일입니다.
ms.prod: xamarin
ms.assetid: 93F1D5C5-E2AF-46EA-8460-485A0860C176
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/11/2018
ms.openlocfilehash: 3c84b29807fd4a181ed867095645005bf9ba4df0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60957333"
---
# <a name="binding-a-jar"></a>JAR 바인딩

_이 연습에서는 Android에서 Xamarin.Android Java 바인딩 라이브러리를 만들기 위한 단계별 지침을 제공 합니다. JAR 파일입니다._

## <a name="overview"></a>개요

Android 커뮤니티는 앱에서 사용 하려는 많은 Java 라이브러리를 제공 합니다. 이러한 Java 라이브러리에 자주 패키지 됩니다. 하지만 JAR (Java Archive) 형식으로 패키지할 수는 있습니다. JAR를 *Java 바인딩 라이브러리* 해당 기능을 Xamarin.Android 앱에 사용할 수 있도록 합니다. Java 바인딩 라이브러리의 목적은의 Api를 확인 합니다. JAR 파일을 사용할 수 있는 C# 코드 자동 생성 래퍼를 통해 코드입니다.

Xamarin 도구에서 하나 이상의 입력 바인딩 라이브러리를 생성할 수 있습니다. JAR 파일입니다. 바인딩 라이브러리 (합니다. DLL 어셈블리)에 다음이 포함 됩니다. 

-   콘텐츠 원본입니다. JAR 파일입니다.

-   관리 되는 호출 가능 래퍼 (MCW)에 C# 내에서 해당 Java 형식을 래핑하는 형식 합니다. JAR 파일입니다.

생성된 된 MCW 코드는 기본 API 호출에 전달할 JNI (Java 기본 인터페이스)를 사용 합니다. JAR 파일입니다. 에 대 한 바인딩 라이브러리를 만들 수 있습니다. Android (참고 Xamarin 도구 현재 Android Java가 아닌 라이브러리의 바인딩을 지원 하 고 하지 않습니다)와 함께 사용 될 원래 대상 JAR 파일입니다. 바인딩 라이브러리의 콘텐츠를 포함 하지 않고 빌드를 선택할 수도 있습니다는. JAR 파일을 DLL에 종속 되어 있도록 합니다. 런타임 시 JAR.

이 가이드에서는 단일 바인딩 라이브러리 만들기의 기본 사항을 단계별로 실행 했습니다. JAR 파일입니다. 예를 들어 모든 항목은 오른쪽에 대해 설명 하겠습니다 &ndash; 즉, 여기서 사용자 지정 없이 또는 바인딩의 디버깅가 필요 합니다. 
[바인딩을 사용 하 여 메타 데이터 만들기](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) 바인딩 프로세스는 완전 자동 되지 않았고 어느 정도의 수동 개입이 필요 고급 시나리오의 예를 제공 합니다. Java 라이브러리 바인딩 일반적 (기본 코드 예제)의 개요를 보려면 [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)합니다. 

 
## <a name="walkthrough"></a>연습

에 대 한 바인딩 라이브러리 다음 연습에서는 만들겠습니다 [피카소](http://square.github.io/picasso/), 널리 사용 되는 Android. 이미지를 로드 하 고 캐싱 기능을 제공 하는 JAR. 다음 단계를 사용 하 여 바인딩할는 우리가 **피카소 2.x.x.jar** Xamarin.Android 프로젝트에서 사용할 수 있는 새.NET 어셈블리를 만들려면: 

1. 새 Java 바인딩 라이브러리 프로젝트를 만듭니다.

2. 추가 합니다. 프로젝트에 JAR 파일입니다.

3. 에 대 한 적절 한 빌드 동작을 설정 합니다. JAR 파일입니다.

4. 대상 프레임 워크를 선택 합니다. JAR을 지원합니다.

5. 바인딩 라이브러리를 빌드하십시오.

바인딩 라이브러리를 만든 후 바인딩 라이브러리에서 Api를 호출 하는 능력을 보여 주는 작은 Android 앱을 개발 했습니다. 이 예의 메서드에 액세스 하려고 **피카소 2.x.x.jar**:

```java
package com.squareup.picasso

public class Picasso
{ 
    ...
    public static Picasso with (Context context) { ... };
    ...
    public RequestCreator load (String path) { ... };
    ...
}
```

에 대 한 바인딩 라이브러리를 생성 한 후 **피카소 2.x.x.jar**에서 이러한 메서드를 호출할 수 있습니다 C#합니다. 예를 들어:

```csharp
using Com.Squareup.Picasso;
...
Picasso.With (this)
    .Load ("http://mydomain.myimage.jpg")
    .Into (imageView);

```


### <a name="creating-the-bindings-library"></a>바인딩 라이브러리 만들기

다음 단계를 사용 하 여을 시작 하기 전에 다운로드 하세요 [피카소 2.x.x.jar](http://repo1.maven.org/maven2/com/squareup/picasso/picasso/2.5.2/picasso-2.5.2.jar)합니다.

먼저 새 바인딩 라이브러리 프로젝트를 만듭니다. Mac 또는 Visual Studio에 대 한 Visual Studio에서 새 솔루션을 만들고 선택 합니다 *Android 바인딩 라이브러리* 템플릿. (이 연습의 스크린샷은 Visual Studio를 사용 하지만 Mac 용 Visual Studio 매우 비슷합니다.) 솔루션의 이름을 **JarBinding**: 

[![JarBinding 라이브러리 프로젝트 만들기](binding-a-jar-images/01-new-bindings-library-sml.w157.png)](binding-a-jar-images/01-new-bindings-library.w157.png#lightbox)

템플릿에 포함 되어는 **Jars** 폴더를 추가 하는 프로그램입니다. 바인딩 라이브러리 프로젝트에 JAR(s)입니다. 마우스 오른쪽 단추로 클릭 합니다 **Jar** 선택한 폴더 **추가 > 기존 항목**: 

[![기존 항목 추가](binding-a-jar-images/02-add-existing-item-sml.png)](binding-a-jar-images/02-add-existing-item.png#lightbox)

로 이동 합니다 **피카소 2.x.x.jar** 이전에 다운로드 한 파일을 선택 하 고 클릭 **추가**: 

[![Jar 파일을 선택 하 고 추가 클릭 합니다.](binding-a-jar-images/03-select-jar-file-sml.png)](binding-a-jar-images/03-select-jar-file.png#lightbox)

있는지 확인 합니다 **피카소 2.x.x.jar** 파일이 프로젝트에 추가 되었습니다. 

[![프로젝트에 추가 하는 jar](binding-a-jar-images/04-jar-added-sml.png)](binding-a-jar-images/04-jar-added.png#lightbox)

Java 바인딩 라이브러리 프로젝트를 만들 때 지정 해야 하는지 여부를 합니다. JAR는 바인딩 라이브러리에 포함 하거나 별도로 패키지 하는 것입니다. 이렇게 하려면 중 하나를 지정한 다음 *빌드 작업*: 

-   **EmbeddedJar** &ndash; 합니다. JAR는 바인딩 라이브러리에 포함 됩니다.

-   **InputJar** &ndash; 합니다. JAR는 바인딩 라이브러리를 별도로 유지 됩니다.

일반적으로 사용 합니다 **EmbeddedJar** 빌드 작업 되도록 합니다. JAR는 바인딩 라이브러리에 자동으로 패키지 됩니다. 이 가장 간단한 옵션 &ndash; Java 바이트 코드에서 합니다. JAR는 Dex 바이트 코드를 변환 되 고 관리 되는 호출 가능 래퍼) (함께 APK에 포함 됩니다. 유지 하려는 경우 합니다. 바인딩 라이브러리에서 별도 JAR를 사용할 수 있습니다 합니다 **InputJar** 옵션; 그러나 확인 해야 합니다. JAR 파일은 응용 프로그램을 실행 하는 장치에서 사용할 수 있습니다. 

빌드 작업을 설정 **EmbeddedJar**: 

[![EmbeddedJar 빌드 작업 선택](binding-a-jar-images/05-embeddedjar-sml.png)](binding-a-jar-images/05-embeddedjar.png#lightbox)

프로젝트 구성 속성을 엽니다는 *대상 프레임 워크*합니다. 경우는 합니다. 모든 Android Api를 사용 하는 JAR, API 수준으로 대상 프레임 워크를 설정 합니다. JAR이 필요합니다. 일반적으로 개발자는 합니다. JAR 파일은 어떤 API 수준 (또는 수준)을 나타냅니다는 합니다. JAR는 호환 됩니다. (대상 프레임 워크 설정과 일반에서 Android API 수준에 대 한 자세한 내용은 참조 하세요. [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md).)

바인딩 라이브러리에 대 한 대상 API 수준 설정 (이 예에서는 사용 하는 API 수준 19): 

[![대상 API 수준 19 API 설정](binding-a-jar-images/06-set-target-framework-sml.png)](binding-a-jar-images/06-set-target-framework.png#lightbox)


마지막으로 바인딩 라이브러리를 빌드하십시오. 일부 경고 메시지가 표시 될 수 있습니다, 있지만 바인딩 라이브러리 프로젝트를 성공적으로 빌드를 업데이트 하 고 출력을 생성 해야 합니다. 다음 위치에서 DLL: **JarBinding/bin/Debug/JarBinding.dll**
    


### <a name="using-the-bindings-library"></a>바인딩 라이브러리를 사용 하 여

이 사용 하 합니다. Xamarin android 앱에서 DLL 다음을 수행 합니다.

1.  바인딩 라이브러리에 대 한 참조를 추가 합니다.

2.  호출 합니다. 관리 되는 호출 가능 래퍼를 통해 JAR. 

다음 단계에서는 바인딩 라이브러리를 사용 하 여 다운로드 하 고 이미지를 표시 하는 최소한의 앱을 만듭니다는 `ImageView`; "어려운"에 있는 코드에서 수행 되는 합니다. JAR 파일입니다. 

바인딩 라이브러리를 사용 하는 새 Xamarin.Android 앱을 먼저 만듭니다. 솔루션을 마우스 오른쪽 단추로 클릭 **새 프로젝트 추가**; 새 프로젝트의 이름을 **BindingTest**합니다. 이 연습에서는; 간소화 하기 위해 바인딩 라이브러리와 동일한 솔루션에서이 앱을 만들 때 했습니다. 그러나 바인딩 라이브러리를 사용 하는 앱 다른 솔루션에 위치할 대신 수 있습니다. 

[![새 BindingTest 프로젝트 추가](binding-a-jar-images/07-add-new-project-sml.w157.png)](binding-a-jar-images/07-add-new-project.w157.png#lightbox)

마우스 오른쪽 단추로 클릭 합니다 **참조** 노드의 합니다 **BindingTest** 프로젝트를 마우스 **참조 추가...** :

[![오른쪽 참조 추가](binding-a-jar-images/08-add-reference.png)](binding-a-jar-images/08-add-reference.png#lightbox)

확인 합니다 **JarBinding** 앞에서 만든 프로젝트 및 클릭 **확인**:

[![JarBinding 프로젝트 선택](binding-a-jar-images/09-choose-jar-binding-sml.png)](binding-a-jar-images/09-choose-jar-binding.png#lightbox)

열기는 **참조** 노드의 **BindingTest** 프로젝트를 확인 합니다 **JarBinding** 참조가 있습니다: 

[![JarBinding 참조 아래에 나타납니다.](binding-a-jar-images/10-references-shows-jarbinding-sml.png)](binding-a-jar-images/10-references-shows-jarbinding.png#lightbox)

수정 된 **BindingTest** 레이아웃 (**Main.axml**)에 단일 `ImageView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <ImageView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/imageView" />
</LinearLayout>
```

다음을 추가 합니다 `using` 문을 **MainActivity.cs** &ndash; 따라서 Java 기반 메서드에 쉽게 액세스할 수 `Picasso` 바인딩 라이브러리에 있는 클래스:

```csharp
using Com.Squareup.Picasso;
```

수정 합니다 `OnCreate` 메서드를 사용 하 여 합니다 `Picasso` 에 표시 하는 URL에서 이미지를 로드 하는 클래스는 `ImageView`: 

```csharp
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);
        ImageView imageView = FindViewById<ImageView>(Resource.Id.imageView);

        // Use the Picasso jar library to load and display this image:
        Picasso.With (this)
            .Load ("http://i.imgur.com/DvpvklR.jpg")
            .Into (imageView);
    }
}
```

컴파일 및 실행 합니다 **BindingTest** 프로젝트입니다. 앱 시작 되며 (네트워크 조건)에 따라 짧은 지연 후 다운로드 하 고 다음 스크린샷과 유사 하 게 이미지를 표시 합니다.

[![BindingTest의 스크린 샷 실행](binding-a-jar-images/11-result-sml.png)](binding-a-jar-images/11-result.png#lightbox)

축하합니다. Java 라이브러리를 성공적으로 연결 했습니다. Xamarin android 앱에서 사용 하는 JAR.
 
 
## <a name="summary"></a>요약

이 연습에서는 타사에 대 한 바인딩 라이브러리를 만들었습니다. JAR 파일에 바인딩 라이브러리 최소한의 테스트 앱을 추가 하 고 다음 확인 하려면 앱을 실행는 C# 코드에 있는 Java 코드를 호출할 수 있습니다 합니다. JAR 파일입니다. 



## <a name="related-links"></a>관련 링크

- [Java 바인딩 라이브러리 (비디오) 빌드](https://university.xamarin.com/classes#10090)
- [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)
