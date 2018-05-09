---
title: 바인딩는 합니다. JAR
description: 이 연습에서는 Android에서 Xamarin.Android Java 바인딩 라이브러리를 만들기 위한 단계별 지침을 제공 합니다. JAR 파일입니다.
ms.prod: xamarin
ms.assetid: 93F1D5C5-E2AF-46EA-8460-485A0860C176
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/11/2018
ms.openlocfilehash: 2d9f2198dbb88e7614944ac73729a4e6eca42647
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
---
# <a name="binding-a-jar"></a>바인딩는 합니다. JAR

_이 연습에서는 Android에서 Xamarin.Android Java 바인딩 라이브러리를 만들기 위한 단계별 지침을 제공 합니다. JAR 파일입니다._

## <a name="overview"></a>개요

Android 커뮤니티 앱에서 사용 하려고 수 있는 많은 Java 라이브러리를 제공 합니다. 이러한 Java 라이브러리에 종종 패키지 됩니다. 하지만 JAR (Java 아카이브) 형식으로 패키지할 수는 있습니다. JAR는 *Java 바인딩 라이브러리* 의 기능은 Xamarin.Android 앱에 사용할 수 있도록 합니다. Java 바인딩 라이브러리의 목적은의 Api를 확인 하는 합니다. JAR 파일을 자동으로 생성 된 코드 래퍼를 통해 C# 코드에서 사용할 수 있습니다.

Xamarin 도구에서 하나 이상의 입력 바인딩 라이브러리를 생성할 수 있습니다. JAR 파일입니다. 바인딩 라이브러리 (합니다. DLL 어셈블리)에 다음이 포함 되어 있습니다. 

-   원래의 내용입니다. JAR 파일입니다.

-   관리 되는 호출 가능 래퍼 MCW (), C# 요소인 형식 내에서 해당 Java 유형을 래핑하는 합니다. JAR 파일입니다.

생성 된 MCW 코드를 내부 API 호출을 전달 하 JNI (Java 기본 인터페이스)를 사용 합니다. JAR 파일입니다. 에 대 한 바인딩 라이브러리를 만들 수 있습니다. Android (참고 Xamarin 도구가 현재 Android Java 비 라이브러리의 바인딩을 지원 하 고 하지 않는)에서 사용 되는 원래 대상 JAR 파일입니다. 내용을 포함 하지 않고 바인딩 라이브러리를 작성 하도록 선택할 수도 있습니다는 합니다. DLL에 종속 되도록 JAR 파일의 합니다. 런타임 시 JAR.

이 가이드에서는 단일에 대 한 바인딩 라이브러리를 만드는 기본적인 단계별로 합니다에서는 합니다. JAR 파일입니다. 모든 작업이 오른쪽 첫 번째 예를 설명 하겠습니다 &ndash; 즉, 사용자 지정 항목이 없습니다 또는 바인딩 디버깅가 필요한 곳입니다. 
[바인딩을 사용 하 여 메타 데이터 만들기](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) 여기서 바인딩 프로세스는 완전히 자동이 아니므로 하 고 어느 정도의 수동 개입이 필요한는 좀 더 고급 시나리오의 예를 제공 합니다. Java 라이브러리 바인딩으로 일반적 (기본 코드 예제를 보려면)의 개요를 참조 하십시오. [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)합니다. 

 
## <a name="walkthrough"></a>연습

다음 연습에서는 만들어에 대 한 바인딩 라이브러리 [피카소](http://square.github.io/picasso/), 인기 있는 Android 합니다. 이미지 로드 및 캐시 기능을 제공 하는 JAR. 다음 단계를 사용 하 여 바인딩할 됩니다 우리 **피카소 2.x.x.jar** Xamarin.Android 프로젝트에서 사용할 수 있는 새.NET 어셈블리를 만들려면: 

1. 새 Java 바인딩 라이브러리 프로젝트를 만듭니다.

2. 추가 합니다. JAR 파일을 프로젝트입니다.

3. 적절 한 빌드 작업에 대 한 설정에서 합니다. JAR 파일입니다.

4. 대상 프레임 워크를 선택는 합니다. JAR 지원합니다.

5. 바인딩 라이브러리를 빌드하십시오.

바인딩 라이브러리를 만들고 나면에서는 바인딩 라이브러리의 Api를 호출 하는 능력을 보여 주는 작은 Android 앱을 개발 합니다. 이 예제에서는의 메서드에 액세스 하려는 **피카소 2.x.x.jar**:

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

```csharp
After we generate a Bindings Library for **picasso-2.x.x.jar**,
we can call these methods from C#. For example:

```csharp
using Com.Squareup.Picasso;
...
Picasso.With (this)
    .Load ("http://mydomain.myimage.jpg")
    .Into (imageView);

```


### <a name="creating-the-bindings-library"></a>바인딩 라이브러리 만들기

다음 단계와 작업을 시작 하기 전에 다운로드 하세요 [피카소 2.x.x.jar](http://repo1.maven.org/maven2/com/squareup/picasso/picasso/2.5.2/picasso-2.5.2.jar)합니다.

먼저 새 바인딩 라이브러리 프로젝트를 만듭니다. Mac 또는 Visual Studio 용 Visual Studio에서 새 솔루션을 만들고 선택 된 *Android 바인딩 라이브러리* 템플릿. (Visual Studio를 사용 하는이 연습에서 스크린 샷 있지만 Mac 용 Visual Studio는 매우 유사). 솔루션 이름을 **JarBinding**: 

[![JarBinding 라이브러리 프로젝트 만들기](binding-a-jar-images/01-new-bindings-library-sml.w157.png)](binding-a-jar-images/01-new-bindings-library.w157.png#lightbox)

템플릿에 포함 되어는 **단지** 폴더를 추가 하면 합니다. 바인딩 라이브러리 프로젝트에 JAR(s) 합니다. 마우스 오른쪽 단추로 클릭는 **단지** 폴더와 선택 **추가 > 기존 항목**: 

[![기존 항목 추가](binding-a-jar-images/02-add-existing-item-sml.png)](binding-a-jar-images/02-add-existing-item.png#lightbox)

탐색 하 고 **피카소 2.x.x.jar** 이전에 다운로드 한 파일을 선택 하 고 클릭 **추가**: 

[![Jar 파일을 선택 하 고 추가 클릭 합니다.](binding-a-jar-images/03-select-jar-file-sml.png)](binding-a-jar-images/03-select-jar-file.png#lightbox)

확인 하는 **피카소 2.x.x.jar** 파일이 프로젝트에 추가 되었습니다. 

[![프로젝트에 추가 하는 jar](binding-a-jar-images/04-jar-added-sml.png)](binding-a-jar-images/04-jar-added.png#lightbox)

Java 바인딩 라이브러리 프로젝트를 만들 때 지정 해야 하는지 여부를 합니다. JAR는 바인딩 라이브러리에 포함 하거나 별도로 패키지 하는 것입니다. 이렇게 하려면 중 하나를 지정 다음 *빌드 작업*: 

-   **EmbeddedJar** &ndash; 는 합니다. JAR는 바인딩 라이브러리에 포함 됩니다.

-   **InputJar** &ndash; 는 합니다. JAR는 바인딩 라이브러리와에서 별도로 유지 됩니다.

일반적으로 사용 된 **EmbeddedJar** 빌드 작업 하는 합니다. JAR는 바인딩 라이브러리에 자동으로 패키지 됩니다. 이 옵션은 가장 간단한 옵션 &ndash; 에서 바이트 Java 코드는 합니다. JAR는 Dex 바이트 코드로 변환 되 고 프로그램 APK에 관리 되는 호출 가능 래퍼) (함께 포함 됩니다. 계속 하려면는 합니다. 하지만 바인딩 라이브러리에서 별도 JAR를 사용할 수 있습니다는 **InputJar** 옵션; 확인 해야는 합니다. JAR 파일은 응용 프로그램을 실행 하는 장치에서 사용할 수 있습니다. 

빌드 작업을 설정 **EmbeddedJar**: 

[![EmbeddedJar 빌드 작업을 선택 합니다.](binding-a-jar-images/05-embeddedjar-sml.png)](binding-a-jar-images/05-embeddedjar.png#lightbox)

프로젝트 속성을 구성을 열고는 *대상 프레임 워크*합니다. 경우는 합니다. 모든 Android Api를 사용 하는 JAR, API 수준으로 대상 프레임 워크 설정에서 합니다. JAR 필요합니다. 일반적으로 개발자는 합니다. JAR 파일에는 API 수준 (또는 수준) 표시 됩니다 하 고 있습니다. JAR 호환 됩니다. (대상 프레임 워크 설정 및 일반적으로 Android API 수준에 대 한 자세한 내용은 참조 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md).)

바인딩 라이브러리에 대 한 대상 API 수준 설정 (이 예제에서 사용 하 여 API 수준 19): 

[![API 19로 설정 하는 대상 API 레벨](binding-a-jar-images/06-set-target-framework-sml.png)](binding-a-jar-images/06-set-target-framework.png#lightbox)


마지막으로, 바인딩 라이브러리를 빌드하십시오. 일부 경고 메시지가 나타날 수 있지만 바인딩 라이브러리 프로젝트에는 성공적으로 빌드를 업데이트 하 고 출력을 생성 해야 합니다. 다음 위치에서 DLL: **JarBinding/bin/Debug/JarBinding.dll**
    


### <a name="using-the-bindings-library"></a>바인딩 라이브러리를 사용 하 여

이 사용 합니다. DLL이 Xamarin.Android 응용 프로그램에 다음을 수행 합니다.

1.  바인딩 라이브러리에 대 한 참조를 추가 합니다.

2.  호출의 합니다. 관리 되는 호출 가능 래퍼를 통해 JAR. 

다음 단계에서는 만들어 바인딩 라이브러리를 사용 하 여 다운로드 하 고 이미지를 표시 하는 최소 앱은 `ImageView`;에 상주 하는 코드에서 "작업을 대신" 이루어집니다는 합니다. JAR 파일입니다. 

먼저 바인딩 라이브러리를 사용 하는 새 Xamarin.Android 앱을 만듭니다. 솔루션을 마우스 오른쪽 단추로 클릭 하 고 선택 **새 프로젝트 추가**; 새 프로젝트의 이름을 **BindingTest**합니다. 이 연습에서는; 간소화 하기 위해 바인딩을 라이브러리와 동일한 솔루션에이 응용 프로그램을 만들으십시오 그러나 바인딩 라이브러리를 사용 하는 앱 다른 솔루션에 있는 대신 수 있습니다.: 

[![새 BindingTest 프로젝트 추가](binding-a-jar-images/07-add-new-project-sml.w157.png)](binding-a-jar-images/07-add-new-project.w157.png#lightbox)

마우스 오른쪽 단추로 클릭는 **참조** 의 노드는 **BindingTest** 프로젝트를 마우스 선택 **참조 추가...** :

[![오른쪽 참조를 추가 합니다.](binding-a-jar-images/08-add-reference.png)](binding-a-jar-images/08-add-reference.png#lightbox)

확인 된 **JarBinding** 클릭 하 고 앞에서 만든 프로젝트 **확인**:

[![JarBinding 프로젝트 선택](binding-a-jar-images/09-choose-jar-binding-sml.png)](binding-a-jar-images/09-choose-jar-binding.png#lightbox)

열기는 **참조** 의 노드는 **BindingTest** 되어 있는지 확인 하 고 프로젝트는 **JarBinding** 참조가 있습니다. 

[![JarBinding 참조 아래에 나타납니다.](binding-a-jar-images/10-references-shows-jarbinding-sml.png)](binding-a-jar-images/10-references-shows-jarbinding.png#lightbox)

수정 된 **BindingTest** 레이아웃 (**Main.axml**)에 단일 있도록 `ImageView`:

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

다음 추가 `using` 문을 **MainActivity.cs** &ndash; 의 Java 기반 메서드를 쉽게 액세스할 수 있도록이 `Picasso` 바인딩 라이브러리에 있는 클래스:

```csharp
using Com.Squareup.Picasso;
```

수정 된 `OnCreate` 메서드를 사용 하 여는 `Picasso` 클래스에 표시 하는 URL에서 이미지를 로드 하는 `ImageView`: 

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

컴파일 및 실행 된 **BindingTest** 프로젝트. 앱의 시작 및 (네트워크 상태)에 따라 짧은 지연 후 다운로드 하 고 다음 스크린샷과 유사 하 게 이미지를 표시 합니다.

[![BindingTest 스크린샷 실행](binding-a-jar-images/11-result-sml.png)](binding-a-jar-images/11-result.png#lightbox)

지금까지 Java 라이브러리에 성공적으로 연결 했습니다. Xamarin.Android 앱에서 사용 하는 JAR.
 
 
## <a name="summary"></a>요약

이 연습에서는 다른 공급 업체에 대 한 바인딩 라이브러리를 만들었습니다. JAR 파일을 바인딩 라이브러리는 최소한의 테스트 앱을 추가 하 고 다음 C# 코드에 있는 Java 코드를 호출할 수 있는지 확인 하려면 앱을 실행 된 합니다. JAR 파일입니다. 



## <a name="related-links"></a>관련 링크

- [작성 Java 바인딩 라이브러리 (비디오)](https://university.xamarin.com/classes#10090)
- [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)
