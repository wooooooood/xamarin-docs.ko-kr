---
title: .AAR 바인딩
description: 이 연습에서는 Android에서 Xamarin.ios Java 바인딩 라이브러리를 만드는 방법에 대 한 단계별 지침을 제공 합니다. AAR 파일입니다.
ms.prod: xamarin
ms.assetid: 380413B8-6A99-4BB8-B64C-3EAF9F359C22
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/11/2018
ms.openlocfilehash: 291547f7d0fa77edf77b29762576494de0b1abb4
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757808"
---
# <a name="binding-an-aar"></a>.AAR 바인딩

_이 연습에서는 Android에서 Xamarin.ios Java 바인딩 라이브러리를 만드는 방법에 대 한 단계별 지침을 제공 합니다. AAR 파일입니다._

## <a name="overview"></a>개요

*Android 보관 파일 (. AAR)* 파일은 Android 라이브러리의 파일 형식입니다.
하면. AAR 파일은입니다. 다음을 포함 하는 ZIP 보관 파일:

- 컴파일된 Java 코드
- 리소스 Id
- 리소스
- 메타 데이터 (예: 활동 선언, 사용 권한)

이 가이드에서는 단일에 대 한 바인딩 라이브러리를 만드는 기본 사항을 단계별로 설명 합니다. AAR 파일입니다. 일반적인 Java 라이브러리 바인딩에 대 한 개요는 기본 코드 예제와 함께 [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)을 참조 하세요.

> [!IMPORTANT]
> 바인딩 프로젝트는 하나만 포함할 수 있습니다. AAR 파일입니다. 이면이 고, 그렇지 않으면입니다. 다른에 대 한 종속성을 AAR 합니다. AAR 이러한 종속성은 자체의 바인딩 프로젝트에 포함 된 다음 참조 되어야 합니다. [버그 44573](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)을 참조 하세요.

## <a name="walkthrough"></a>연습

Android Studio, [textanalyzer. aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true)에서 만든 예제 Android 보관 파일에 대 한 바인딩 라이브러리를 만듭니다. 이. AAR에는 `TextCounter` 문자열의 모음 및 자음 수를 계산 하는 정적 메서드를 포함 하는 클래스가 포함 되어 있습니다. 또한 **textanalyzer** 에는 개수 결과를 표시 하는 데 도움이 되는 이미지 리소스가 포함 되어 있습니다.

다음 단계를 사용 하 여에서 바인딩 라이브러리를 만듭니다. AAR 파일:

1. 새 Java 바인딩 라이브러리 프로젝트를 만듭니다.

2. 단일를 추가 합니다. AAR 파일을 프로젝트에 추가할 수 있습니다. 바인딩 프로젝트는 단일만 포함할 수 있습니다. AAR.

3. 에 대 한 적절 한 빌드 작업을 설정 합니다. AAR 파일입니다.

4. 이 있는 대상 프레임 워크를 선택 합니다. AAR는을 지원 합니다.

5. 바인딩 라이브러리를 빌드합니다.

바인딩 라이브러리를 만든 후에는 사용자에 게 텍스트 문자열을 묻는 메시지를 표시 하는 작은 Android 앱을 개발 합니다. 텍스트를 분석 하는 AAR 메서드는에서 이미지를 검색 합니다. AAR 및 이미지와 함께 결과를 표시 합니다.

샘플 앱은 `TextCounter` **textanalyzer aar**의 클래스에 액세스 합니다.

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

또한이 샘플 앱은 **textanalyzer. aar**에 패키지 된 이미지 리소스를 검색 하 고 표시 합니다.

[![Xamarin 원숭이 이미지](binding-an-aar-images/00-monkey-sml.png)](binding-an-aar-images/00-monkey.png#lightbox)

이 이미지 리소스는 **textanalyzer. aar**의 **res/그릴 때/원숭이 .png** 에 있습니다.

### <a name="creating-the-bindings-library"></a>바인딩 라이브러리 만들기

아래 단계를 시작 하기 전에 [aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true) Android 보관 파일 예제를 다운로드 하세요.

1. Android 바인딩 라이브러리 템플릿으로 시작 하는 새 바인딩 라이브러리 프로젝트를 만듭니다. Mac용 Visual Studio 또는 Visual Studio 중 하나를 사용할 수 있습니다. 아래 스크린샷은 Visual Studio를 표시 하지만 Mac용 Visual Studio는 매우 비슷합니다. 솔루션 이름을 **AarBinding**로 합니다.

    [![AarBindings 프로젝트 만들기](binding-an-aar-images/01-new-bindings-library-vs-sml.w160.png)](binding-an-aar-images/01-new-bindings-library-vs.w160.png#lightbox)

2. 템플릿에는를 추가 하는 **jar** 폴더가 포함 되어 있습니다. 바인딩 라이브러리 프로젝트에 대 한 AAR입니다. **Jar** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **> 기존 항목 추가**를 선택 합니다.

    [![기존 항목 추가](binding-an-aar-images/02-add-existing-item-vs-sml.png)](binding-an-aar-images/02-add-existing-item-vs.png#lightbox)

3. 이전에 다운로드 한 **textanalyzer. aar** 파일로 이동 하 여 선택 하 고 **추가**를 클릭 합니다.

    [![Textanalayzer. aar를 추가 합니다.](binding-an-aar-images/03-select-aar-file-vs-sml.png)](binding-an-aar-images/03-select-aar-file-vs.png#lightbox)

4. **Textanalyzer** 파일이 프로젝트에 성공적으로 추가 되었는지 확인 합니다.

    [![Textanalyzer 파일이 추가 되었습니다.](binding-an-aar-images/04-aar-added-vs-sml.png)](binding-an-aar-images/04-aar-added-vs.png#lightbox)

5. **Textanalyzer. aar** 에 대 한 빌드 작업을 `LibraryProjectZip`로 설정 합니다. Mac용 Visual Studio에서 **textanalyzer. aar** 를 마우스 오른쪽 단추로 클릭 하 여 빌드 작업을 설정 합니다. Visual Studio에서 빌드 작업은 **속성** 창에서 설정할 수 있습니다.

    [![Textanalyzer. aar 빌드 작업을 라이브러리 편집기로 설정](binding-an-aar-images/05-embedded-aar-vs-sml.png)](binding-an-aar-images/05-embedded-aar-vs.png#lightbox)

6. 프로젝트 속성을 열어 *대상 프레임 워크*를 구성 합니다. 이면이 고, 그렇지 않으면입니다. AAR는 Android Api를 사용 하 여 대상 프레임 워크를의 API 수준으로 설정 합니다. AAR에는가 필요 합니다. (일반적으로 대상 프레임 워크 설정 및 Android API 수준에 대 한 자세한 내용은 [ANDROID Api 수준 이해](~/android/app-fundamentals/android-api-levels.md)를 참조 하세요.)

    바인딩 라이브러리의 대상 API 수준을 설정 합니다. 이 예제에서 **textanalyzer** 는 Android api에 대 한 종속성이 없기 때문에 최신 플랫폼 api 수준 (api 수준 23)을 무료로 사용할 수 있습니다.

    [![대상 수준을 API 23으로 설정](binding-an-aar-images/06-set-target-framework-vs-sml.png)](binding-an-aar-images/06-set-target-framework-vs.png#lightbox)

7. 바인딩 라이브러리를 빌드합니다. 바인딩 라이브러리 프로젝트가 성공적으로 작성 되 고 출력을 생성 해야 합니다. 다음 위치의 DLL: **AarBinding/bin/Debug/AarBinding.dll**

### <a name="using-the-bindings-library"></a>바인딩 라이브러리 사용

이를 사용 하려면입니다. DLL을 다운로드 하려면 먼저 바인딩 라이브러리에 대 한 참조를 추가 해야 합니다. 다음 단계를 사용합니다.

1. 이 연습을 간소화 하기 위해 바인딩 라이브러리와 동일한 솔루션에이 앱을 만듭니다. 바인딩 라이브러리를 사용 하는 앱은 다른 솔루션에도 있을 수 있습니다. 새 Xamarin Android 앱 만들기: 솔루션을 마우스 오른쪽 단추로 클릭 하 고 **새 프로젝트 추가**를 선택 합니다. 새 프로젝트의 이름을 **Bindingtest**로 합니다.

    [![새 BindingTest 프로젝트 만들기](binding-an-aar-images/07-add-new-project-vs-sml.w157.png)](binding-an-aar-images/07-add-new-project-vs.w157.png#lightbox)

2. **Bindingtest** 프로젝트의 **참조** 노드를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가**...를 선택 합니다.

    [![참조 추가를 클릭 합니다.](binding-an-aar-images/08-add-reference-vs-sml.png)](binding-an-aar-images/08-add-reference-vs.png#lightbox)

3. 이전에 만든 **AarBinding** 프로젝트를 선택 하 고 **확인을**클릭 합니다.

    [![AAR 바인딩 프로젝트를 확인 합니다.](binding-an-aar-images/09-choose-aar-binding-vs-sml.png)](binding-an-aar-images/09-choose-aar-binding-vs.png#lightbox)

4. **Bindingtest** 프로젝트의 **참조** 노드를 열어 **AarBinding** 참조가 있는지 확인 합니다.

    [![AarBinding은 참조 아래에 나열 됩니다.](binding-an-aar-images/10-references-shows-aarbinding-vs-sml.png)](binding-an-aar-images/10-references-shows-aarbinding-vs.png#lightbox)

바인딩 라이브러리 프로젝트의 콘텐츠를 보려면 참조를 두 번 클릭 하 여 **개체 브라우저**에서 열 수 있습니다. `Com.Xamarin.Textcounter` 네임 스페이스의 매핑된 콘텐츠 (Java `com.xamarin.textanalyzezr` 패키지에서 매핑됨)를 확인 하 고 `TextCounter` 클래스의 멤버를 볼 수 있습니다.

[![개체 브라우저 보기](binding-an-aar-images/11-object-browser-vs-sml.png)](binding-an-aar-images/11-object-browser-vs.png#lightbox)

위의 스크린샷은 예제 `TextAnalyzer` 앱이 `NumConsonants` 호출 하는 두 가지 메서드 (기본 java `numConsonants` 메서드를 래핑하는) 및 `NumVowels` (기본 java `numVowels` 메서드를 래핑하는)를 강조 표시 합니다.

### <a name="accessing-aar-types"></a>하. AAR 형식

바인딩 라이브러리를 가리키는 앱에 대 한 참조를 추가한 후에는에서 Java 형식에 액세스할 수 있습니다. 형식에 액세스 C# 하는 것과 같은 방식으로 C# AAR 합니다 (래퍼 덕분). C#응용 프로그램 코드는 `TextAnalyzer` 다음 예제에 나와 있는 것 처럼 메서드를 호출할 수 있습니다.

```csharp
using Com.Xamarin.Textcounter;
...
int numVowels = TextCounter.NumVowels (myText);
int numConsonants = TextCounter.NumConsonants (myText);
```

위의 예제에서는 `TextCounter` 클래스에서 정적 메서드를 호출 합니다. 그러나 클래스를 인스턴스화하고 인스턴스 메서드를 호출할 수도 있습니다. 예를 들면입니다. AAR는 인스턴스 메서드 `Employee` `buildFullName`를 포함 하는 라는 클래스를 래핑하여 여기에 `MyClass` 표시 된 대로 인스턴스화하고 사용할 수 있습니다.

```csharp
var employee = new Com.MyCompany.MyProject.Employee();
var name = employee.BuildFullName ();
```

다음 단계에서는 사용자에 게 텍스트를 묻는 메시지를 표시 하 고를 사용 `TextCounter` 하 여 텍스트를 분석 한 다음 결과를 표시 하도록 응용 프로그램에 코드를 추가 합니다.

**Bindingtest** 레이아웃 (**Main. axml**)을 다음 XML로 바꿉니다. 이 레이아웃에는 `EditText` 텍스트 입력에 대 한와 모음 및 자음 개수를 시작 하기 위한 두 개의 단추가 있습니다.

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

**MainActivity.cs** 의 내용을 다음 코드로 바꿉니다. 이 예제에서 볼 때와 같이 단추 이벤트 처리기는에 `TextCounter` 상주 하는 래핑된 메서드를 호출 합니다. AAR 및 알림을를 사용 하 여 결과를 표시 합니다. 바인딩된 라이브러리 `using` (이 `Com.Xamarin.Textcounter`경우)의 네임 스페이스에 대 한 문을 확인 합니다.

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

**Bindingtest** 프로젝트를 컴파일하고 실행 합니다. 앱이 시작 되 고 왼쪽에 스크린샷을 표시 합니다 .은 `EditText` 일부 텍스트를 사용 하 여 초기화 되지만이를 탭 하 여 변경할 수 있습니다. **개수 모음**을 탭 하면 오른쪽에 표시 된 것 처럼 알림 메시지에 모음 수가 표시 됩니다.

[![BindingTest 실행의 스크린샷](binding-an-aar-images/12-count-vowels.png)](binding-an-aar-images/12-count-vowels.png#lightbox)

**개수 자음** 단추를 누르면 됩니다. 또한 텍스트 줄을 수정 하 고이 단추를 다시 탭 하 여 여러 모음 및 자음 수를 테스트할 수 있습니다.

### <a name="accessing-aar-resources"></a>하. AAR 리소스

Xamarin 도구는에서 **R** 데이터를 병합 합니다. 앱의 **리소스** 클래스에 AAR. 따라서에 액세스할 수 있습니다. 프로젝트의 **리소스** 경로에 있는 리소스에 액세스 하는 것과 동일한 방식으로 레이아웃 (및 코드 숨김으로)에서 리소스를 AAR 합니다.

이미지 리소스에 액세스 하려면 내에 압축 된 이미지에 대해 **그릴** 수 있는 이름을 사용 합니다. AAR. 예를 들어에서 **이미지 .png** 를 참조할 수 있습니다. 다음을 사용 하 `@drawable/image`여 파일 AAR:

```xml
<ImageView android:src="@drawable/image" ... />
```

에 있는 리소스 레이아웃에도 액세스할 수 있습니다. AAR. 이렇게 하려면 내에 패키지 된 레이아웃에 대 한 **리소스. 레이아웃** 이름을 사용 합니다. AAR. 예를 들어:

```csharp
var a = new ArrayAdapter<string>(this, Resource.Layout.row_layout, ...);
```

**Aar** 예제에는 **res/그릴 때/원숭이 .png**에 있는 이미지 파일이 포함 되어 있습니다. 이 이미지 리소스에 액세스 하 고 예제 앱에서 사용 하겠습니다.

**Bindingtest** 레이아웃 (**Main. axml**)을 편집 하 `ImageView` 고 `LinearLayout` 컨테이너의 끝에를 추가 합니다. `ImageView` **에서@drawable/monkey** 찾은 이미지가 표시 됩니다 .이 이미지는 **textanalyzer. aar**의 리소스 섹션에서 로드 됩니다.

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

**Bindingtest** 프로젝트를 컴파일하고 실행 합니다. 앱이 시작 되 고 왼쪽 &ndash; 의 스크린샷에 표시 됩니다. **자음**을 누르면 오른쪽에 표시 된 대로 결과가 표시 됩니다.

[![자음 수를 표시 하는 BindingTest](binding-an-aar-images/13-count-consonants.png)](binding-an-aar-images/13-count-consonants.png#lightbox)

축하합니다. Java 라이브러리를 성공적으로 바인딩 했습니다. AAR!

## <a name="summary"></a>요약

이 연습에서는에 대 한 바인딩 라이브러리를 만들었습니다. AAR 파일을 열고, 최소 테스트 앱에 바인딩 라이브러리를 추가 하 고, 앱을 실행 하 여 C# 코드에서에 상주 하는 Java 코드를 호출할 수 있는지 확인 합니다. AAR 파일입니다.
또한 응용 프로그램을 확장 하 여에 있는 이미지 리소스에 액세스 하 고이를 표시 합니다. AAR 파일입니다.

## <a name="related-links"></a>관련 링크

- [Java 바인딩 라이브러리 빌드 (비디오)](https://university.xamarin.com/classes#10090)
- [JAR 바인딩](~/android/platform/binding-java-library/binding-a-jar.md)
- [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)
- [AarBinding (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/javaintegration-aarbinding)
- [버그 44573-한 프로젝트에서 여러 aar 파일을 바인딩할 수 없습니다.](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)
