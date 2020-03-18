---
title: .AAR 바인딩
description: 이 연습에서는 Android .AAR 파일에서 Xamarin.Android Java 바인딩 라이브러리를 만드는 방법에 대한 단계별 지침을 제공합니다.
ms.prod: xamarin
ms.assetid: 380413B8-6A99-4BB8-B64C-3EAF9F359C22
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/11/2018
ms.openlocfilehash: 103720c8cb47b1ac4cfe5cfadeb6b18828318ad3
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73618539"
---
# <a name="binding-an-aar"></a>.AAR 바인딩

_이 연습에서는 Android .AAR 파일에서 Xamarin.Android Java 바인딩 라이브러리를 만드는 방법에 대한 단계별 지침을 제공합니다._

## <a name="overview"></a>개요

*Android 보관(.AAR)* 파일은 Android 라이브러리용 파일 형식입니다.
.AAR 파일은 다음을 포함하는 .ZIP 보관 파일입니다.

- 컴파일된 Java 코드
- 리소스 ID
- 리소스
- 메타데이터(예: 작업 선언, 사용 권한)

이 가이드에서는 단일 .AAR 파일에 대한 바인딩 라이브러리 만들 때의 기본 사항을 단계별로 설명합니다. 일반적인 Java 라이브러리 바인딩에 대한 개요(기본 코드 예제 포함)는 [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)을 참조하세요.

> [!IMPORTANT]
> 바인딩 프로젝트에는 하나의 .AAR 파일만 포함할 수 있습니다. .AAR이 다른 .AAR에 종속되어 있는 경우 이러한 종속성은 자체 바인딩 프로젝트에 포함된 다음, 참조되어야 합니다. [버그 44573](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)를 참조하세요.

## <a name="walkthrough"></a>연습

Android Studio에서 만든 예제 Android 보관 파일([textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true))에 대한 바인딩 라이브러리를 만듭니다. 이. AAR에는 문자열의 모음 및 자음 수를 계산하는 정적 메서드를 사용하는 `TextCounter` 클래스가 포함됩니다. 또한 **textanalyzer.aar**에는 계산 결과를 표시하는 데 도움이 되는 이미지 리소스가 포함되어 있습니다.

다음 단계를 사용하여 .AAR 파일에서 바인딩 라이브러리를 만듭니다.

1. 새 Java 바인딩 라이브러리 프로젝트를 만듭니다.

2. 단일 .AAR 파일을 프로젝트에 추가합니다. 바인딩 프로젝트에는 단일 .AAR만 포함할 수 있습니다.

3. .AAR 파일에 적절한 빌드 작업을 설정합니다.

4. .AAR이 지원하는 대상 프레임워크를 선택합니다.

5. 바인딩 라이브러리를 빌드합니다.

바인딩 라이브러리를 만든 후에는 사용자에게 텍스트 문자열을 묻는 메시지를 표시하고, 텍스트를 분석할 .AAR 메서드를 호출하고, .AAR에서 이미지를 검색하고, 이미지와 함께 결과를 표시하는 작은 Android 앱을 개발합니다.

샘플 앱이 **textanalyzer.aar**의 `TextCounter` 클래스에 액세스합니다.

```java
package com.xamarin.textcounter;

public class TextCounter
{
    ...
    public static int numVowels (String text) { ... };
    ...
    public static int numConsonants (String text) { ... };
    ...
}
```

또한 이 샘플 앱은 **textanalyzer.aar**에 패키지된 이미지 리소스를 검색하여 표시합니다.

[![Xamarin 원숭이 이미지](binding-an-aar-images/00-monkey-sml.png)](binding-an-aar-images/00-monkey.png#lightbox)

이 이미지 리소스는 **textanalyzer.aar**의 **res/drawable/monkey.png**에 있습니다.

### <a name="creating-the-bindings-library"></a>바인딩 라이브러리 만들기

아래 단계를 시작하기 전에 [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true) Android 보관 파일 예제를 다운로드하세요.

1. Android 바인딩 라이브러리 템플릿으로 시작하는 새 바인딩 라이브러리 프로젝트를 만듭니다. Mac용 Visual Studio 또는 Visual Studio 중 하나를 사용할 수 있습니다. 아래 스크린샷에는 Visual Studio가 나와 있지만 Mac용 Visual Studio도 매우 비슷합니다. 솔루션 이름을 **AarBinding**으로 지정합니다.

    [![AarBindings 프로젝트 만들기](binding-an-aar-images/01-new-bindings-library-vs-sml.w160.png)](binding-an-aar-images/01-new-bindings-library-vs.w160.png#lightbox)

2. 이 템플릿에는 .AAR을 바인딩 라이브러리 프로젝트에 추가할 수 있는 **Jars** 폴더가 포함되어 있습니다. **Jars** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 > 기존 항목**을 선택합니다.

    [![기존 항목 추가](binding-an-aar-images/02-add-existing-item-vs-sml.png)](binding-an-aar-images/02-add-existing-item-vs.png#lightbox)

3. 이전에 다운로드한 **textanalyzer.aar** 파일로 이동하여 선택한 후 **추가**를 클릭합니다.

    [![textanalayzer.aar 추가](binding-an-aar-images/03-select-aar-file-vs-sml.png)](binding-an-aar-images/03-select-aar-file-vs.png#lightbox)

4. **textanalyzer.aar** 파일이 프로젝트에 성공적으로 추가되었는지 확인합니다.

    [![textanalyzer.aar 파일이 추가됨](binding-an-aar-images/04-aar-added-vs-sml.png)](binding-an-aar-images/04-aar-added-vs.png#lightbox)

5. **textanalyzer.aar**에 대한 빌드 작업을 `LibraryProjectZip`으로 설정합니다. Mac용 Visual Studio에서 **textanalyzer.aar**을 마우스 오른쪽 단추로 클릭하여 빌드 작업을 설정합니다. Visual Studio에서는 빌드 작업을 **속성** 창에서 설정할 수 있습니다.

    [![textanalyzer.aar 빌드 작업을 LibraryProjectZip으로 설정](binding-an-aar-images/05-embedded-aar-vs-sml.png)](binding-an-aar-images/05-embedded-aar-vs.png#lightbox)

6. 프로젝트 속성을 열어 *대상 프레임워크*를 구성합니다. .AAR이 Android API를 사용하는 경우 대상 프레임워크를 .AAR에 필요한 API 수준으로 설정합니다. (일반적으로 대상 프레임워크 설정 및 Android API 수준에 대한 자세한 내용은 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)를 참조하세요.)

    바인딩 라이브러리의 대상 API 수준을 설정합니다. 이 예제에서는 **textanalyzer**가 Android API에 대한 종속성이 없기 때문에 최신 플랫폼 API 수준(API 수준 23)을 자유롭게 사용할 수 있습니다.

    [![대상 수준을 API 23으로 설정](binding-an-aar-images/06-set-target-framework-vs-sml.png)](binding-an-aar-images/06-set-target-framework-vs.png#lightbox)

7. 바인딩 라이브러리를 빌드합니다. 바인딩 라이브러리 프로젝트는 성공적으로 빌드된 후 다음 위치에 출력 .DLL을 생성해야 합니다. **AarBinding/bin/Debug/AarBinding.dll**

### <a name="using-the-bindings-library"></a>바인딩 라이브러리 사용

이 .DLL을 Xamarin.Android 앱에서 사용하려면 먼저 바인딩 라이브러리에 참조를 추가해야 합니다. 다음 단계를 사용합니다.

1. 이 연습을 간소화하기 위해 바인딩 라이브러리와 동일한 솔루션에 이 앱을 만듭니다. (바인딩 라이브러리를 사용하는 앱이 다른 솔루션에도 있을 수 있습니다.) 새 Xamarin.Android 앱을 만들기: 솔루션을 마우스 오른쪽 단추로 클릭하고 **새 프로젝트 추가**를 선택합니다. 새 프로젝트의 이름을 **BindingTest**로 지정합니다.

    [![새 BindingTest 프로젝트 만들기](binding-an-aar-images/07-add-new-project-vs-sml.w157.png)](binding-an-aar-images/07-add-new-project-vs.w157.png#lightbox)

2. **BindingTest** 프로젝트의 **참조** 노드를 마우스 오른쪽 단추로 클릭하고 **참조 추가...** 를 선택합니다.

    [![참조 추가 클릭](binding-an-aar-images/08-add-reference-vs-sml.png)](binding-an-aar-images/08-add-reference-vs.png#lightbox)

3. 이전에 만든 **AarBinding** 프로젝트를 선택하고 **확인**을 클릭합니다.

    [![AAR 바인딩 프로젝트 확인](binding-an-aar-images/09-choose-aar-binding-vs-sml.png)](binding-an-aar-images/09-choose-aar-binding-vs.png#lightbox)

4. **BindingTest** 프로젝트의 **참조** 노드를 열고 **AarBinding** 참조가 있는지 확인합니다.

    [![AarBinding이 참조 아래에 나열됨](binding-an-aar-images/10-references-shows-aarbinding-vs-sml.png)](binding-an-aar-images/10-references-shows-aarbinding-vs.png#lightbox)

바인딩 라이브러리 프로젝트의 콘텐츠를 보려면 참조를 두 번 클릭하여 **개체 브라우저**에서 열 수 있습니다. `Com.Xamarin.Textcounter`네임스페이스의 매핑된 콘텐츠(Java `com.xamarin.textanalyzezr` 패키지에서 매핑됨)를 볼 수 있으며, `TextCounter` 클래스의 멤버도 볼 수 있습니다.

[![개체 브라우저 보기](binding-an-aar-images/11-object-browser-vs-sml.png)](binding-an-aar-images/11-object-browser-vs.png#lightbox)

위의 스크린샷에는 예제 앱에서 호출하는 두 가지 `TextAnalyzer` 메서드가 강조 표시되어 있습니다(기본 Java `numConsonants` 메서드를 래핑하는 `NumConsonants` 및 기본 Java `numVowels` 메서드를 래핑하는 `NumVowels`).

### <a name="accessing-aar-types"></a>.AAR 형식 액세스

바인딩 라이브러리를 가리키는 앱에 대한 참조를 추가한 후에는 C# 형식에 액세스하는 것과 같은 방식으로 .AAR의 Java 형식에 액세스할 수 있습니다(C# 래퍼 덕분). C# 앱 코드는 이 예제에 나온 것처럼 `TextAnalyzer` 메서드를 호출할 수 있습니다.

```csharp
using Com.Xamarin.Textcounter;
...
int numVowels = TextCounter.NumVowels (myText);
int numConsonants = TextCounter.NumConsonants (myText);
```

위의 예제에서는 `TextCounter` 클래스의 정적 메서드를 호출합니다. 그러나 클래스를 인스턴스화한 후 인스턴스 메서드를 호출할 수도 있습니다. 예를 들어 .AAR이 인스턴스 메서드 `buildFullName`이 있는 `Employee`라는 클래스를 래핑하는 경우 `MyClass`를 인스턴스화한 후 여기에 나온 대로 사용할 수 있습니다.

```csharp
var employee = new Com.MyCompany.MyProject.Employee();
var name = employee.BuildFullName ();
```

다음 단계에서는 사용자에게 텍스트를 묻는 메시지를 표시하고, `TextCounter`를 사용하여 텍스트를 분석한 다음, 결과를 표시할 수 있도록 앱에 코드를 추가합니다.

**BindingTest** 레이아웃(**Main.axml**)을 다음 XML로 바꿉니다. 이 레이아웃에는 텍스트 입력에 대한 `EditText`와 모음 및 자음 계산을 시작하기 위한 두 개의 단추가 있습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation             ="vertical"
    android:layout_width            ="fill_parent"
    android:layout_height           ="fill_parent" >
    <TextView
        android:text                ="Text to analyze:"
        android:textSize            ="24dp"
        android:layout_marginTop    ="30dp"
        android:layout_gravity      ="center"
        android:layout_width        ="wrap_content"
        android:layout_height       ="wrap_content" />
    <EditText
        android:id                  ="@+id/input"
        android:text                ="I can use my .AAR file from C#!"
        android:layout_marginTop    ="10dp"
        android:layout_gravity      ="center"
        android:layout_width        ="300dp"
        android:layout_height       ="wrap_content"/>
    <Button
        android:id                  ="@+id/vowels"
        android:layout_marginTop    ="30dp"
        android:layout_width        ="240dp"
        android:layout_height       ="wrap_content"
        android:layout_gravity      ="center"
        android:text                ="Count Vowels" />
    <Button
        android:id                  ="@+id/consonants"
        android:layout_width        ="240dp"
        android:layout_height       ="wrap_content"
        android:layout_gravity      ="center"
        android:text                ="Count Consonants" />
</LinearLayout>
```

**MainActivity.cs**의 내용을 다음 코드로 바꿉니다. 이 예제와 같이 단추 이벤트 처리기는 .AAR에 상주하고 알림을 사용하여 결과를 표시하는 래핑된 `TextCounter` 메서드를 호출합니다. 바인딩된 라이브러리(이 경우 `Com.Xamarin.Textcounter`)의 네임스페이스에 대한 `using` 문을 확인합니다.

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using Android.Views.InputMethods;
using Com.Xamarin.Textcounter;

namespace BindingTest
{
    [Activity(Label = "BindingTest", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        InputMethodManager imm;

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            SetContentView(Resource.Layout.Main);

            imm = (InputMethodManager)GetSystemService(Context.InputMethodService);

            var vowelsBtn = FindViewById<Button>(Resource.Id.vowels);
            var consonBtn = FindViewById<Button>(Resource.Id.consonants);
            var edittext = FindViewById<EditText>(Resource.Id.input);
            edittext.InputType = Android.Text.InputTypes.TextVariationPassword;

            edittext.KeyPress += (sender, e) =>
            {
                imm.HideSoftInputFromWindow(edittext.WindowToken, HideSoftInputFlags.NotAlways);
                e.Handled = true;
            };

            vowelsBtn.Click += (sender, e) =>
            {
                int count = TextCounter.NumVowels(edittext.Text);
                string msg = count + " vowels found.";
                Toast.MakeText (this, msg, ToastLength.Short).Show ();
            };

            consonBtn.Click += (sender, e) =>
            {
                int count = TextCounter.NumConsonants(edittext.Text);
                string msg = count + " consonants found.";
                Toast.MakeText (this, msg, ToastLength.Short).Show ();
            };

        }
    }
}
```

**BindingTest** 프로젝트를 컴파일하고 실행합니다. 앱이 시작되고 왼쪽에 스크린샷이 표시됩니다(`EditText`가 일부 텍스트로 초기화되지만 탭하여 변경할 수 있음). **COUNT VOWELS**를 탭하면 오른쪽에 표시된 대로 알림 메시지에 모음 수가 표시됩니다.

[![BindingTest 실행 스크린샷](binding-an-aar-images/12-count-vowels.png)](binding-an-aar-images/12-count-vowels.png#lightbox)

**COUNT CONSONANTS** 단추를 탭합니다. 또한 텍스트 줄을 수정하고 이들 단추를 다시 탭하여 서로 다른 모음 및 자음 개수를 테스트할 수 있습니다.

### <a name="accessing-aar-resources"></a>.AAR 리소스 액세스

Xamarin 도구는 .AAR의 **R** 데이터를 앱의 **리소스** 클래스에 병합합니다. 따라서 프로젝트의 **리소스** 경로에 있는 리소스에 액세스하는 것과 동일한 방식으로 레이아웃(및 코드 숨김)의 .AAR 리소스에 액세스할 수 있습니다.

이미지 리소스에 액세스하려면 .AAR 내에 압축된 이미지의 **Resource.Drawable** 이름을 사용합니다. 예를 들어 `@drawable/image`를 사용하여 .AAR 파일의 **image.png**를 참조할 수 있습니다.

```xml
<ImageView android:src="@drawable/image" ... />
```

.AAR에 있는 리소스 레이아웃도 액세스할 수 있습니다. 이렇게 하려면 .AAR 내에 패키지된 레이아웃의 **Resource.Layout** 이름을 사용합니다. 예를 들어:

```csharp
var a = new ArrayAdapter<string>(this, Resource.Layout.row_layout, ...);
```

**textanalyzer.aar** 예제에는 **res/drawable/monkey.png**에 있는 이미지 파일이 포함되어 있습니다. 이 이미지 리소스에 액세스하여 예제 앱에서 사용해 보겠습니다.

**BindingTest** 레이아웃(**Main.axml**)을 편집하고 `LinearLayout` 컨테이너의 끝에 `ImageView`를 추가합니다. 이 `ImageView`는 **\@drawable/monkey**에 있는 이미지를 표시하며, 이 이미지는 **textanalyzer.aar**의 리소스 섹션에서 로드됩니다.

```xml
    ...
    <ImageView
        android:src                 ="@drawable/monkey"
        android:layout_marginTop    ="40dp"
        android:layout_width        ="200dp"
        android:layout_height       ="200dp"
        android:layout_gravity      ="center" />

</LinearLayout>
```

**BindingTest** 프로젝트를 컴파일하고 실행합니다. 앱이 시작되고 왼쪽에 스크린샷을 표시합니다. **COUNT CONSONANTS**를 탭하면 오른쪽에 표시된 대로 결과가 표시됩니다.

[![자음 개수를 표시하는 BindingTest](binding-an-aar-images/13-count-consonants.png)](binding-an-aar-images/13-count-consonants.png#lightbox)

지금까지 Java 라이브러리 .AAR을 성공적으로 바인딩했습니다.

## <a name="summary"></a>요약

이 연습에서는 .AAR 파일에 대한 바인딩 라이브러리를 만들고, 최소 테스트 앱에 바인딩 라이브러리를 추가한 다음, 앱을 실행하여 C# 코드에서 .AAR 파일에 있는 Java 코드를 호출할 수 있는지 확인했습니다.
또한 앱을 확장하여 .AAR 파일에 있는 이미지 리소스에 액세스하고 표시했습니다.

## <a name="related-links"></a>관련 링크

- [Java 바인딩 라이브러리 빌드(비디오)](https://university.xamarin.com/classes#10090)
- [JAR 바인딩](~/android/platform/binding-java-library/binding-a-jar.md)
- [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)
- [AarBinding(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/javaintegration-aarbinding)
- [버그 44573 - 한 프로젝트에서 여러 .aar 파일을 바인딩할 수 없습니다.](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)
