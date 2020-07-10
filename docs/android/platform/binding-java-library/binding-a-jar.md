---
title: JAR 바인딩
description: 이 연습에서는 Android .JAR 파일에서 Xamarin.Android Java 바인딩 라이브러리를 만드는 방법에 대한 단계별 지침을 제공합니다.
ms.prod: xamarin
ms.assetid: 93F1D5C5-E2AF-46EA-8460-485A0860C176
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/11/2018
ms.openlocfilehash: 336462176813b9985e2c269273d08c4aacbea9b5
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853105"
---
# <a name="binding-a-jar"></a>JAR 바인딩

> [!IMPORTANT]
> 현재 Xamarin 플랫폼에서 사용자 지정 바인딩 사용을 조사하고 있습니다. [**설문 조사**](https://www.surveymonkey.com/r/KKBHNLT)에 참여하여 향후 개발 작업에 대해 알려 주시기 바랍니다.

_이 연습에서는 Android .JAR 파일에서 Xamarin.Android Java 바인딩 라이브러리를 만드는 방법에 대한 단계별 지침을 제공합니다._

## <a name="overview"></a>개요

Android 커뮤니티는 앱에서 사용하면 좋은 다양한 Java 라이브러리를 제공합니다. 해당 Java 라이브러리는 종종 .JAR(Java Archive) 형식으로 패키지되지만 해당 기능을 Xamarin.Android 앱에서 사용할 수 있도록 ‘Java 바인딩 라이브러리’에 .JAR를 패키지할 수 있습니다. Java 바인딩 라이브러리의 목적은 자동으로 생성된 코드 래퍼를 통해 .JAR 파일의 API를 C# 코드에서 사용할 수 있게 만드는 것입니다.

Xamarin 도구는 하나 이상의 입력 .JAR 파일에서 바인딩 라이브러리를 생성할 수 있습니다. 바인딩 라이브러리(.DLL 어셈블리)에는 다음이 포함됩니다. 

- 원본 .JAR 파일의 내용

- .JAR 파일 내에 해당 Java 형식을 래핑하는 C# 형식인 MCW(관리형 호출 가능 래퍼)

생성된 MCW 코드는 JNI(Java Native Interface)를 사용하여 API 호출을 기본 .JAR 파일에 전달합니다. 원래 Android에서 사용하도록 대상으로 지정된 모든 .JAR 파일에 대해 바인딩 라이브러리를 만들 수 있습니다(Xamarin 도구는 현재 Android 이외 Java 라이브러리의 바인딩을 지원하지 않음). .JAR 파일의 콘텐츠를 포함하지 않고 바인딩 라이브러리를 빌드하도록 선택할 수도 있습니다. 이렇게 하면 DLL이 런타임에 .JAR에 종속되게 됩니다.

이 가이드에서는 단일 .JAR 파일에 대한 바인딩 라이브러리 만들 때의 기본 사항을 단계별로 설명합니다. 모든 항목이 적절히 작용하므로 바인딩의 사용자 지정 또는 디버깅이 필요하지 않은 예제를 살펴보겠습니다. 
[메타데이터를 사용하여 바인딩을 만드는 경우](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)는 바인딩 프로세스가 완전히 자동으로 수행되지는 않으며 약간의 수동 작업이 필요한 고급 시나리오의 예제에 해당합니다. 일반적인 Java 라이브러리 바인딩에 대한 개요(기본 코드 예제 포함)는 [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)을 참조하세요. 

## <a name="walkthrough"></a>연습

다음 연습에서는 이미지 로드 및 캐싱 기능을 제공하는 인기 있는 Android .JAR인 [Picasso](https://square.github.io/picasso/)용 바인딩 라이브러리를 만듭니다. 다음 단계에 따라 **picasso-2.x.x.jar**을 바인딩하여 Xamarin.Android 프로젝트에서 사용할 수 있는 새 .NET 어셈블리를 만듭니다. 

1. 새 Java 바인딩 라이브러리 프로젝트를 만듭니다.

2. 프로젝트에 .JAR 파일을 추가합니다.

3. .JAR 파일에 대한 적절한 빌드 작업을 설정합니다.

4. .JAR이 지원하는 대상 프레임워크를 선택합니다.

5. 바인딩 라이브러리를 빌드합니다.

바인딩 라이브러리를 만들었으면 바인딩 라이브러리에서 API를 호출하는 기능을 보여 주는 소형 Android 앱을 개발합니다. 이 예제에서는 **picasso-2.x.x.jar**의 메서드에 액세스하려고 합니다.

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

**picasso-2.x.x.jar**에 대한 바인딩 라이브러리를 생성한 후에는 C#에서 해당 메서드를 호출할 수 있습니다. 예를 들어:

```csharp
using Com.Squareup.Picasso;
...
Picasso.With (this)
    .Load ("http://mydomain.myimage.jpg")
    .Into (imageView);

```

### <a name="creating-the-bindings-library"></a>바인딩 라이브러리 만들기

아래 단계를 시작하기 전에 [picasso-2.x.x.jar](http://repo1.maven.org/maven2/com/squareup/picasso/picasso/2.5.2/picasso-2.5.2.jar)을 다운로드하세요.

먼저 새 바인딩 라이브러리 프로젝트를 만듭니다. Mac용 Visual Studio 또는 Visual Studio에서 새 솔루션을 만들고 ‘Android 바인딩 라이브러리’ 템플릿을 선택합니다. 이 연습의 스크린샷은 Visual Studio를 사용하지만 Mac용 Visual Studio는 매우 유사합니다. 솔루션 이름을 **JarBinding**으 지정합니다. 

[![JarBinding 라이브러리 프로젝트 만들기](binding-a-jar-images/01-new-bindings-library-sml.w157.png)](binding-a-jar-images/01-new-bindings-library.w157.png#lightbox)

이 템플릿에는 바인딩 라이브러리 프로젝트에 .JAR을 추가하는 **Jar** 폴더가 포함되어 있습니다. **Jars** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 > 기존 항목**을 선택합니다. 

[![기존 항목 추가](binding-a-jar-images/02-add-existing-item-sml.png)](binding-a-jar-images/02-add-existing-item.png#lightbox)

이전에 다운로드한 **picasso-2.x.x.jar** 파일로 이동하여 선택한 후 **추가**를 클릭합니다. 

[![Jar 파일 선택 및 추가 클릭](binding-a-jar-images/03-select-jar-file-sml.png)](binding-a-jar-images/03-select-jar-file.png#lightbox)

**picasso-2.x.x.jar** 파일이 프로젝트에 성공적으로 추가되었는지 확인합니다. 

[![프로젝트에 추가된 Jar](binding-a-jar-images/04-jar-added-sml.png)](binding-a-jar-images/04-jar-added.png#lightbox)

Java 바인딩 라이브러리 프로젝트를 만드는 경우 .JAR을 바인딩 라이브러리에 포함할지 또는 별도로 패키지할지를 지정해야 합니다. 이렇게 하려면 다음 ‘빌드 작업’ 중 하나를 지정합니다. 

- **EmbeddedJar** &ndash; .JAR은 바인딩 라이브러리에 포함됩니다.

- **InputJar** &ndash; .JAR은 바인딩 라이브러리와 별도로 유지됩니다.

일반적으로 .JAR이 바인딩 라이브러리에 자동으로 패키지되도록 **EmbeddedJar** 빌드 작업을 사용합니다. 이것은 가장 간단한 옵션입니다. .JAR의 Java 바이트 코드는 Dex 바이트 코드로 변환된 후 APK에 포함됩니다(관리형 호출 가능 래퍼와 함께). .JAR을 바인딩 라이브러리와는 별도로 유지하려면 **InputJar** 옵션을 사용할 수 있습니다. 그러나 앱을 실행하는 디바이스에서 .JAR 파일을 사용할 수 있는지 확인해야 합니다. 

빌드 작업을 **EmbeddedJar**로 설정합니다. 

[![EmbeddedJar 빌드 작업 선택](binding-a-jar-images/05-embeddedjar-sml.png)](binding-a-jar-images/05-embeddedjar.png#lightbox)

그런 다음, 프로젝트 속성을 열어 ‘대상 프레임워크’를 구성합니다. .JAR은 Android API를 사용하는 경우 대상 프레임워크를 .JAR에 필요한 API 레벨로 설정합니다. 일반적으로 .JAR 파일의 개발자는 .JAR이 호환되는 API 레벨을 나타냅니다. (일반적으로 대상 프레임워크 설정 및 Android API 레벨에 대한 자세한 내용은 [Android API 레벨 이해](~/android/app-fundamentals/android-api-levels.md)를 참조하세요.)

바인딩 라이브러리의 대상 API 레벨을 설정합니다(이 예제에서는 API 레벨 19 사용). 

[![API 19로 설정되는 대상 API 레벨](binding-a-jar-images/06-set-target-framework-sml.png)](binding-a-jar-images/06-set-target-framework.png#lightbox)

마지막으로 바인딩 라이브러리를 빌드합니다. 일부 경고 메시지가 표시될 수 있지만 바인딩 라이브러리 프로젝트는 성공적으로 빌드된 후 다음 위치에 출력 .DLL을 생성해야 합니다. **JarBinding/bin/Debug/JarBinding.dll**

### <a name="using-the-bindings-library"></a>바인딩 라이브러리 사용

Xamarin.Android 앱에서 이 .DLL을 사용하려면 다음을 수행합니다.

1. 바인딩 라이브러리에 대한 참조를 추가합니다.

2. 관리형 호출 가능 래퍼를 통해 .JAR을 호출합니다. 

다음 단계에서는 바인딩 라이브러리를 사용하여 이미지를 다운로드한 후 `ImageView`에 표시하는 최소 앱을 만듭니다. "까다로운 작업"은 .JAR 파일에 있는 코드를 통해 수행됩니다. 

먼저 바인딩 라이브러리를 사용하는 새 Xamarin.Android 앱을 만듭니다. 솔루션을 마우스 오른쪽 단추로 클릭하고 **새 프로젝트 추가**를 선택하고 새 프로젝트의 이름을 **BindingTest**로 지정합니다. 이 연습을 간소화하기 위해 바인딩 라이브러리와 동일한 솔루션에서 이 앱을 만듭니다. 그러나 바인딩 라이브러리를 사용하는 앱은 다른 솔루션에 상주할 수 있습니다. 

[![새 BindingTest 프로젝트 추가](binding-a-jar-images/07-add-new-project-sml.w157.png)](binding-a-jar-images/07-add-new-project.w157.png#lightbox)

**BindingTest** 프로젝트의 **참조** 노드를 마우스 오른쪽 단추로 클릭하고 **참조 추가...** 를 선택합니다.

[![적절한 참조 추가](binding-a-jar-images/08-add-reference.png)](binding-a-jar-images/08-add-reference.png#lightbox)

이전에 만든 **JarBinding** 프로젝트를 선택하고 **확인**을 클릭합니다.

[![JarBinding 프로젝트 선택](binding-a-jar-images/09-choose-jar-binding-sml.png)](binding-a-jar-images/09-choose-jar-binding.png#lightbox)

**BindingTest** 프로젝트의 **참조** 노드를 열고 **JarBinding** 참조가 있는지 확인합니다. 

[![JarBinding이 참조 아래에 있음](binding-a-jar-images/10-references-shows-jarbinding-sml.png)](binding-a-jar-images/10-references-shows-jarbinding.png#lightbox)

단일 `ImageView`를 포함하도록 **BindingTest** 레이아웃(**Main.axml**)을 수정합니다.

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

다음 `using` 문을 **MainActivity.cs**에 추가합니다. 이렇게 하면 바인딩 라이브러리에 있는 Java 기반 `Picasso` 클래스의 메서드에 쉽게 액세스할 수 있습니다.

```csharp
using Com.Squareup.Picasso;
```

`Picasso` 클래스를 사용하여 URL에서 이미지를 로드한 후 `ImageView`에 표시하도록 `OnCreate` 메서드를 수정합니다. 

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

**BindingTest** 프로젝트를 컴파일하고 실행합니다. 앱이 시작되고, 잠시 후(네트워크 조건에 따라) 다음 스크린샷과 비슷한 이미지를 다운로드한 후 표시해야 합니다.

[![BindingTest 실행 스크린샷](binding-a-jar-images/11-result-sml.png)](binding-a-jar-images/11-result.png#lightbox)

지금까지 Java 라이브러리 .JAR을 성공적으로 바인딩한 후 Xamarin.Android 앱에서 사용했습니다.

## <a name="summary"></a>요약

이 연습에서는 타사 .JAR 파일에 대한 바인딩 라이브러리를 만들고, 최소 테스트 앱에 바인딩 라이브러리를 추가한 다음, 앱을 실행하여 C# 코드에서 .JAR 파일에 있는 Java 코드를 호출할 수 있는지 확인했습니다. 

## <a name="related-links"></a>관련 링크

- [Java 바인딩 라이브러리 빌드(비디오)](https://university.xamarin.com/classes#10090)
- [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)
