---
title: JAR 바인딩
description: 이 연습에서는 Android에서 Xamarin.ios Java 바인딩 라이브러리를 만드는 방법에 대 한 단계별 지침을 제공 합니다. JAR 파일.
ms.prod: xamarin
ms.assetid: 93F1D5C5-E2AF-46EA-8460-485A0860C176
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/11/2018
ms.openlocfilehash: 9a9e7e9c5d189527d4fbdcc2001d6f003fa63dd7
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757872"
---
# <a name="binding-a-jar"></a>JAR 바인딩

_이 연습에서는 Android에서 Xamarin.ios Java 바인딩 라이브러리를 만드는 방법에 대 한 단계별 지침을 제공 합니다. JAR 파일._

## <a name="overview"></a>개요

Android 커뮤니티는 앱에서 사용할 수 있는 많은 Java 라이브러리를 제공 합니다. 이러한 Java 라이브러리는 종종에 패키지 됩니다. JAR (Java Archive) 형식 이지만를 패키지할 수 있습니다. Xamarin Android 앱에서 해당 기능을 사용할 수 있도록 *Java 바인딩 라이브러리* 에서 JAR 합니다. Java 바인딩 라이브러리의 목적은에서 Api를 만드는 것입니다. 자동으로 생성 된 C# 코드 래퍼를 통해 코드에서 사용할 수 있는 JAR 파일입니다.

Xamarin 도구는 하나 이상의 입력에서 바인딩 라이브러리를 생성할 수 있습니다. JAR 파일. 바인딩 라이브러리 (. DLL 어셈블리)에는 다음이 포함 됩니다. 

- 원래의 내용입니다. JAR 파일.

- 에서 해당 하는 Java 형식을 래핑하는 C# 형식인 mcw (관리 되는 호출 가능 래퍼). JAR 파일.

생성 된 MCW 코드는 JNI (Java Native Interface)를 사용 하 여 API 호출을 기본에 전달 합니다. JAR 파일. 모든에 대해 바인딩 라이브러리를 만들 수 있습니다. 원래 Android에서 사용 되는 것으로 대상으로 지정 된 JAR 파일 (Xamarin 도구는 현재 비 Android Java 라이브러리의 바인딩을 지원 하지 않음) 콘텐츠를 포함 하지 않고 바인딩 라이브러리를 빌드하도록 선택할 수도 있습니다. DLL이에 종속 되도록 JAR 파일입니다. 런타임에 JAR.

이 가이드에서는 단일에 대 한 바인딩 라이브러리를 만드는 기본 사항을 단계별로 설명 합니다. JAR 파일. 모든 항목이 오른쪽 &ndash; 으로 이동 하는 예를 살펴보겠습니다. 여기에서 바인딩 사용자 지정 이나 디버깅이 필요 하지 않습니다. 
[메타 데이터를 사용 하 여 바인딩을 만들면](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) 바인딩 프로세스가 완전히 자동으로 수행 되지 않고 약간의 수동 작업이 필요한 고급 시나리오의 예가 제공 됩니다. 일반적인 Java 라이브러리 바인딩에 대 한 개요는 기본 코드 예제와 함께 [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)을 참조 하세요. 

## <a name="walkthrough"></a>연습

다음 연습에서는 인기 있는 Android 인 [Picasso](http://square.github.io/picasso/)에 대 한 바인딩 라이브러리를 만듭니다. 이미지 로드 및 캐싱 기능을 제공 하는 JAR입니다. **Picasso-2** 를 바인딩하려면 다음 단계를 사용 하 여 xamarin.ios 프로젝트에서 사용할 수 있는 새 .net 어셈블리를 만듭니다. 

1. 새 Java 바인딩 라이브러리 프로젝트를 만듭니다.

2. 를 추가 합니다. 프로젝트에 JAR 파일을 추가할 수 있습니다.

3. 에 대 한 적절 한 빌드 작업을 설정 합니다. JAR 파일.

4. 이 있는 대상 프레임 워크를 선택 합니다. JAR은를 지원 합니다.

5. 바인딩 라이브러리를 빌드합니다.

바인딩 라이브러리를 만들었으면 바인딩 라이브러리에서 Api를 호출 하는 기능을 보여 주는 작은 Android 앱을 개발 합니다. 이 예제에서는 **picasso-2**의 메서드에 액세스 하려고 합니다.

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

**Picasso-2**에 대 한 바인딩 라이브러리를 생성 한 후에서 C#이 메서드를 호출할 수 있습니다. 예를 들어:

```csharp
using Com.Squareup.Picasso;
...
Picasso.With (this)
    .Load ("http://mydomain.myimage.jpg")
    .Into (imageView);

```

### <a name="creating-the-bindings-library"></a>바인딩 라이브러리 만들기

아래 단계를 시작 하기 전에 [picasso-2](http://repo1.maven.org/maven2/com/squareup/picasso/picasso/2.5.2/picasso-2.5.2.jar). x. x. x. x. x. x를 다운로드 하세요.

먼저 새 바인딩 라이브러리 프로젝트를 만듭니다. Mac용 Visual Studio 또는 Visual Studio에서 새 솔루션을 만들고 *Android 바인딩 라이브러리* 템플릿을 선택 합니다. 이 연습의 스크린샷은 Visual Studio를 사용 하지만 Mac용 Visual Studio 매우 유사 합니다. 솔루션 이름을 **JarBinding**로 합니다. 

[![JarBinding library 프로젝트 만들기](binding-a-jar-images/01-new-bindings-library-sml.w157.png)](binding-a-jar-images/01-new-bindings-library.w157.png#lightbox)

템플릿에는를 추가 하는 **jar** 폴더가 포함 되어 있습니다. 이는 바인딩 라이브러리 프로젝트에 대 한 JAR입니다. **Jar** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **> 기존 항목 추가**를 선택 합니다. 

[![기존 항목 추가](binding-a-jar-images/02-add-existing-item-sml.png)](binding-a-jar-images/02-add-existing-item.png#lightbox)

이전에 다운로드 한 **picasso-2** 파일을 찾아 선택 하 고 **추가**를 클릭 합니다. 

[![Jar 파일을 선택 하 고 추가를 클릭 합니다.](binding-a-jar-images/03-select-jar-file-sml.png)](binding-a-jar-images/03-select-jar-file.png#lightbox)

**Picasso-2** 파일이 프로젝트에 성공적으로 추가 되었는지 확인 합니다. 

[![프로젝트에 추가 된 Jar](binding-a-jar-images/04-jar-added-sml.png)](binding-a-jar-images/04-jar-added.png#lightbox)

Java 바인딩 라이브러리 프로젝트를 만드는 경우에는를 지정 해야 합니다. JAR은 바인딩 라이브러리에 포함 되거나 별도로 패키지할 수 있습니다. 이렇게 하려면 다음 *빌드 작업*중 하나를 지정 합니다. 

- **EmbeddedJar** &ndash; 입니다. JAR은 바인딩 라이브러리에 포함 됩니다.

- **Inputjar** &ndash; 입니다. JAR은 바인딩 라이브러리와 별도로 유지 됩니다.

일반적으로 **EmbeddedJar** 빌드 작업을 사용 하 여를 만듭니다. JAR은 자동으로 바인딩 라이브러리에 패키지 됩니다. 에서 가장 간단한 Java 바이트 &ndash; 코드 옵션입니다. JAR은 Dex 바이트 코드로 변환 되며 관리 되는 호출 가능 래퍼와 함께 APK에 포함 됩니다. 을 유지 하려는 경우 JAR-바인딩 라이브러리와는 별도로 **inputjar** 옵션을 사용할 수 있습니다. 그러나를 확인 해야 합니다. 앱을 실행 하는 장치에서 JAR 파일을 사용할 수 있습니다. 

빌드 작업을 **EmbeddedJar**로 설정 합니다. 

[![EmbeddedJar 빌드 작업 선택](binding-a-jar-images/05-embeddedjar-sml.png)](binding-a-jar-images/05-embeddedjar.png#lightbox)

그런 다음 프로젝트 속성을 열어 *대상 프레임 워크*를 구성 합니다. 이면이 고, 그렇지 않으면입니다. JAR은 Android Api를 사용 하 고, 대상 프레임 워크를에 대 한 API 수준으로 설정 합니다. JAR가 필요 합니다. 일반적으로의 개발자입니다. JAR 파일은에 해당 하는 API 수준 (또는 수준)을 표시 합니다. JAR은와 호환 됩니다. (일반적으로 대상 프레임 워크 설정 및 Android API 수준에 대 한 자세한 내용은 [ANDROID Api 수준 이해](~/android/app-fundamentals/android-api-levels.md)를 참조 하세요.)

바인딩 라이브러리의 대상 API 수준을 설정 합니다 (이 예제에서는 API 레벨 19를 사용 합니다). 

[![API 19로 설정 되는 대상 API 수준](binding-a-jar-images/06-set-target-framework-sml.png)](binding-a-jar-images/06-set-target-framework.png#lightbox)

마지막으로 바인딩 라이브러리를 빌드합니다. 일부 경고 메시지가 표시 될 수 있지만 바인딩 라이브러리 프로젝트를 성공적으로 빌드하고 출력을 생성 해야 합니다. 다음 위치의 DLL: **JarBinding/bin/Debug/JarBinding.dll**

### <a name="using-the-bindings-library"></a>바인딩 라이브러리 사용

이를 사용 하려면입니다. DLL을 Xamarin Android 앱에서 다음을 수행 합니다.

1. 바인딩 라이브러리에 참조를 추가 합니다.

2. 을 호출 합니다. 관리 되는 호출 가능 래퍼를 통해 JAR. 

다음 단계에서는 바인딩 라이브러리를 사용 하 여에서 `ImageView`이미지를 다운로드 하 고 표시 하는 최소 앱을 만듭니다. "과도 한"는에 있는 코드에 의해 수행 됩니다. JAR 파일. 

먼저 바인딩 라이브러리를 사용 하는 새 Xamarin Android 앱을 만듭니다. 솔루션을 마우스 오른쪽 단추로 클릭 하 고 **추가 새 프로젝트**를 선택 합니다. 새 프로젝트의 이름을 **Bindingtest**로 합니다. 이 연습을 간소화 하기 위해 바인딩 라이브러리와 동일한 솔루션에서이 앱을 만드는 중입니다. 그러나 바인딩 라이브러리를 사용 하는 앱은 다른 솔루션에 상주할 수 있습니다. 

[![새 BindingTest 프로젝트 추가](binding-a-jar-images/07-add-new-project-sml.w157.png)](binding-a-jar-images/07-add-new-project.w157.png#lightbox)

**Bindingtest** 프로젝트의 **참조** 노드를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가**...를 선택 합니다.

[![참조 추가 권한](binding-a-jar-images/08-add-reference.png)](binding-a-jar-images/08-add-reference.png#lightbox)

이전에 만든 **JarBinding** 프로젝트를 확인 하 고 **확인을**클릭 합니다.

[![JarBinding 프로젝트 선택](binding-a-jar-images/09-choose-jar-binding-sml.png)](binding-a-jar-images/09-choose-jar-binding.png#lightbox)

**Bindingtest** 프로젝트의 **참조** 노드를 열고 **JarBinding** 참조가 있는지 확인 합니다. 

[![JarBinding이 참조 아래에 나타납니다.](binding-a-jar-images/10-references-shows-jarbinding-sml.png)](binding-a-jar-images/10-references-shows-jarbinding.png#lightbox)

단일`ImageView`를 갖도록 **Bindingtest** 레이아웃 (**Main. axml**)을 수정 합니다.

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

`using` `Picasso` MainActivity.cs 에다음문을추가하여바인딩라이브러리에있는Java기반클래스의메서드에쉽게액세스할수있도록합니다.&ndash;

```csharp
using Com.Squareup.Picasso;
```

클래스를 `OnCreate` `Picasso` 사용 하 여 URL에서 이미지를 로드 하 고에 표시 하도록 메서드를 수정 합니다 `ImageView`. 

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

**Bindingtest** 프로젝트를 컴파일하고 실행 합니다. 앱이 시작 되 고, 잠시 후 (네트워크 조건에 따라), 다음 스크린샷 처럼 이미지를 다운로드 하 여 표시 해야 합니다.

[![BindingTest를 실행 하는 스크린샷](binding-a-jar-images/11-result-sml.png)](binding-a-jar-images/11-result.png#lightbox)

축하합니다. Java 라이브러리를 성공적으로 바인딩 했습니다. JAR 및 Xamarin Android 앱에서 사용 합니다.

## <a name="summary"></a>요약

이 연습에서는 타사에 대 한 바인딩 라이브러리를 만들었습니다. JAR 파일을 열고, 최소 테스트 앱에 바인딩 라이브러리를 추가한 다음, 앱을 실행 하 여 C# 코드에서에 상주 하는 Java 코드를 호출할 수 있는지 확인 합니다. JAR 파일. 

## <a name="related-links"></a>관련 링크

- [Java 바인딩 라이브러리 빌드 (비디오)](https://university.xamarin.com/classes#10090)
- [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)
