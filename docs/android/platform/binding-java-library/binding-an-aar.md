---
title: 바인딩는 합니다. AAR
description: 이 연습에서는 Android에서 Xamarin.Android Java 바인딩 라이브러리를 만들기 위한 단계별 지침을 제공 합니다. AAR 파일입니다.
ms.prod: xamarin
ms.assetid: 380413B8-6A99-4BB8-B64C-3EAF9F359C22
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 101fb28add97749549de9c44292a1ef99a717dde
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="binding-an-aar"></a>바인딩는 합니다. AAR

_이 연습에서는 Android에서 Xamarin.Android Java 바인딩 라이브러리를 만들기 위한 단계별 지침을 제공 합니다. AAR 파일입니다._


## <a name="overview"></a>개요

*Android 보관 (합니다. AAR)* 파일은 Android 라이브러리에 대 한 파일 형식입니다.
합니다. AAR 파일은 한 합니다. 다음을 포함 하는 ZIP 보관 파일:

-   컴파일된 Java 코드
-   리소스 Id
-   자료
-   메타 데이터 (예를 들어 활동 선언, 사용 권한)

이 가이드에서는 단일에 대 한 바인딩 라이브러리를 만드는 기본적인 단계별로 합니다에서는 합니다. AAR 파일입니다. Java 라이브러리 바인딩으로 일반적 (기본 코드 예제를 보려면)의 개요를 참조 하십시오. [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)합니다.


> [!IMPORTANT]
> 바인딩 프로젝트 하나 포함할 수 있습니다. AAR 파일입니다. 경우는 합니다. 다른 AAR 종속성입니다. AAR, 다음 이러한 종속성이 자신의 바인딩 프로젝트에 포함 된 고 그런 다음 참조 해야 합니다. 참조 [44573 버그](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)합니다.


## <a name="walkthrough"></a>연습

Android Studio에서 만든 Android 보관 파일을 하는 예제에 대 한 바인딩 라이브러리를 만들겠습니다 [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true)합니다. 이 있습니다. AAR 포함 한 `TextCounter` 모음과 문자열의 자음의 수를 계산 하는 정적 메서드를 사용 하 여 클래스입니다. 또한 **textanalyzer.aar** 계산 결과 표시 하려면 이미지 리소스를 포함 합니다.

에서는 다음 단계에서 바인딩 라이브러리를 만드는 합니다. AAR 파일:

1. 새 Java 바인딩 라이브러리 프로젝트를 만듭니다.

2. Single을 추가 합니다. AAR 파일을 프로젝트입니다. 바인딩 프로젝트는 단일을 포함할 수 있습니다. AAR 합니다.

3. 적절 한 빌드 작업에 대 한 설정에서 합니다. AAR 파일입니다.

4. 대상 프레임 워크를 선택는 합니다. AAR 지원합니다.

5. 바인딩 라이브러리를 빌드하십시오.

바인딩 라이브러리를 만들고 나면에서는 호출 하 여 텍스트 문자열에 대 한 사용자 요청 하는 작은 Android 앱을 개발 합니다. 이미지를 검색 하는 텍스트를 분석 하는 메서드를 AAR는 합니다. AAR, 및 이미지와 함께 결과 표시 합니다.

샘플 응용 프로그램에 액세스는 `TextCounter` 의 클래스 **textanalyzer.aar**:

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

이 샘플 응용 프로그램 검색 하 고에 패키지 된 이미지 리소스를 표시 합니다. 또한 **textanalyzer.aar**:

[![Xamarin 원숭이 이미지](binding-an-aar-images/00-monkey-sml.png)](binding-an-aar-images/00-monkey.png#lightbox)

이 이미지 리소스에 있는 **res/drawable/monkey.png** 에 **textanalyzer.aar**합니다.



### <a name="creating-the-bindings-library"></a>바인딩 라이브러리 만들기

다음 단계와 작업을 시작 하기 전에 예제를 다운로드 하세요 [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true) Android 보관 파일을 사용 합니다.

1.  Android 바인딩 라이브러리 템플릿을 사용 하 여 시작 하는 새 바인딩을 라이브러리 프로젝트를 만듭니다. Mac 용 Visual Studio 또는 Visual Studio (아래 스크린샷과 Visual Studio를 표시 하지만 Mac 용 Visual Studio는 매우 유사)을 사용할 수 있습니다. 솔루션 이름을 **AarBinding**:

    [![AarBindings 프로젝트 만들기](binding-an-aar-images/01-new-bindings-library-vs-sml.png)](binding-an-aar-images/01-new-bindings-library-vs.png#lightbox)

2.  템플릿에 포함 되어는 **단지** 폴더를 추가 하면 합니다. 바인딩 라이브러리 프로젝트에 AAR(s) 합니다. 마우스 오른쪽 단추로 클릭는 **단지** 폴더와 선택 **추가 > 기존 항목**:

    [![기존 항목 추가](binding-an-aar-images/02-add-existing-item-vs-sml.png)](binding-an-aar-images/02-add-existing-item-vs.png#lightbox)


3.  탐색 하 고 **textanalyzer.aar** 이전에 다운로드 한 파일을 선택 하 고 클릭 **추가**:

    [![Textanalayzer.aar 추가](binding-an-aar-images/03-select-aar-file-vs-sml.png)](binding-an-aar-images/03-select-aar-file-vs.png#lightbox)


4.  확인은 **textanalyzer.aar** 파일이 프로젝트에 추가 되었습니다.

    [![Textanalyzer.aar 파일이 추가 된](binding-an-aar-images/04-aar-added-vs-sml.png)](binding-an-aar-images/04-aar-added-vs.png#lightbox)

5.  빌드 작업에 대 한 설정 **textanalyzer.aar** 를 `LibraryProjectZip`합니다. Mac 용 Visual Studio에서 마우스 오른쪽 단추로 클릭 **textanalyzer.aar** 빌드 작업을 설정할 수 있습니다. Visual Studio 빌드 작업에서 설정할 수 있습니다는 **속성** 창):

    [![Textanalyzer.aar 빌드 작업 LibraryProjectZip을로 설정](binding-an-aar-images/05-embedded-aar-vs-sml.png)](binding-an-aar-images/05-embedded-aar-vs.png#lightbox)

6.  프로젝트 속성을 구성을 열고는 *대상 프레임 워크*합니다. 경우는 합니다. API 수준으로 대상 프레임 워크를 설정, AAR Android Api를 사용 하 여 여 합니다. AAR 필요합니다. (대상 프레임 워크 설정 및 일반적으로 Android API 수준에 대 한 자세한 내용은 참조 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md).)

    바인딩 라이브러리에 대 한 대상 API 레벨을 설정 합니다. 이 예제에서는 때문에 최신 플랫폼 API 수준 (API 수준 23)을 사용 하는 우리의 **textanalyzer** Android Api에 종속 되어 있지 않습니다.

    [![대상 수준을 API 23으로 설정](binding-an-aar-images/06-set-target-framework-vs-sml.png)](binding-an-aar-images/06-set-target-framework-vs.png#lightbox)

7.  바인딩 라이브러리를 빌드하십시오. 바인딩 라이브러리 프로젝트는 빌드만 성공할 하 고 출력을 생성 해야 합니다. 다음 위치에서 DLL: **AarBinding/bin/Debug/AarBinding.dll**



### <a name="using-the-bindings-library"></a>바인딩 라이브러리를 사용 하 여

이 사용 합니다. Xamarin.Android 앱에서 DLL을 먼저 바인딩 라이브러리에 대 한 참조를 추가 해야 합니다. 다음 단계를 따르십시오.

1.  이 연습을 단순화 하기 위해 바인딩을 라이브러리와 동일한 솔루션에이 응용 프로그램을 만들고 있습니다. (바인딩 라이브러리를 사용 하는 응용 프로그램은 다른 솔루션에는 있을 수도 없습니다.) 새 Xamarin.Android 앱 만들기: 솔루션을 마우스 오른쪽 단추로 클릭 하 고 선택 **새 프로젝트 추가**합니다. 새 프로젝트의 이름을 **BindingTest**:

    [![새 BindingTest 프로젝트 만들기](binding-an-aar-images/07-add-new-project-vs-sml.png)](binding-an-aar-images/07-add-new-project-vs.png#lightbox)

2.  마우스 오른쪽 단추로 클릭는 **참조** 의 노드는 **BindingTest** 프로젝트를 마우스 선택 **참조 추가...** :

    [![참조 추가 클릭](binding-an-aar-images/08-add-reference-vs-sml.png)](binding-an-aar-images/08-add-reference-vs.png#lightbox)

3.  선택 된 **AarBinding** 클릭 하 고 앞에서 만든 프로젝트 **확인**:

    [![AAR 바인딩 프로젝트를 확인 합니다.](binding-an-aar-images/09-choose-aar-binding-vs-sml.png)](binding-an-aar-images/09-choose-aar-binding-vs.png#lightbox)

4.  열기는 **참조** 의 노드는 **BindingTest** 되었는지 확인 하는 프로젝트는 **AarBinding** 참조가 있습니다.

    [![AarBinding 참조 아래에 나열 됩니다.](binding-an-aar-images/10-references-shows-aarbinding-vs-sml.png)](binding-an-aar-images/10-references-shows-aarbinding-vs.png#lightbox)


원하는 바인딩 라이브러리 프로젝트의 콘텐츠를 보려면를 두 번 눌러에서 열에 대 한 참조는 **개체 브라우저**합니다. 매핑된 콘텐츠를 볼 수는 `Com.Xamarin.Textcounter` 네임 스페이스 (Java에서 매핑된 `com.xamarin.textanalyzezr` 패키지)의 멤버를 볼 수 있습니다는 `TextCounter` 클래스:

[![개체 브라우저 보기](binding-an-aar-images/11-object-browser-vs-sml.png)](binding-an-aar-images/11-object-browser-vs.png#lightbox)

위의 스크린 샷에서 두 강조 표시 `TextAnalyzer` 예제 응용 프로그램에서 호출할 메서드를: `NumConsonants` (기본 Java를 래핑하는 `numConsonants` 메서드), 및 `NumVowels` (기본 Java를 래핑하는 `numVowels` 메서드).



### <a name="accessing-aar-types"></a>에 액세스합니다. AAR 형식

바인딩 라이브러리를 가리키는 앱에 대 한 참조를 추가한 후에 Java 형식에 액세스할 수 있습니다는 합니다. AAR 때 C# 형식 (C# 래퍼) 덕분에 액세스 하는 것입니다. C# 응용 프로그램 코드를 호출할 수 `TextAnalyzer` 메서드를이 예제에 나와 있습니다.

```csharp
using Com.Xamarin.Textcounter;
...
int numVowels = TextCounter.NumVowels (myText);
int numConsonants = TextCounter.NumConsonants (myText);
```

위의 예제 정적 메서드 호출 중인는 `TextCounter` 클래스입니다. 그러나 수 클래스를 인스턴스화할 수 있고 인스턴스 메서드를 호출 합니다. 예를 들어, 경우에 합니다. AAR 라는 클래스를 래핑합니다 `Employee` 인스턴스 메서드가 `buildFullName`를 인스턴스화할 수 `MyClass` 표시 된 대로 여기서 사용 및:

```csharp
var employee = new Com.MyCompany.MyProject.Employee();
var name = employee.BuildFullName ();
```

다음 단계에서는 사용자에 게 텍스트를 사용 하 여 표시 되도록 응용 프로그램에 코드 추가 `TextCounter` 분석할 텍스트 및 다음 결과 표시 합니다.

대체는 **BindingTest** 레이아웃 (**Main.axml**) 다음 XML로 합니다. 이 레이아웃에는 `EditText` 모음 및 자음 수를 시작 하기 위한 두 개의 단추 및 텍스트 입력:

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

내용을 대체 **MainActivity.cs** 다음 코드를 사용 합니다. 이 예에서 같이 단추 이벤트 처리기 호출 래핑된 `TextCounter` 에 상주 하는 메서드는 합니다. 결과 표시 하 고 알림을 AAR 및 사용 합니다. 공지는 `using` 바인딩된 라이브러리의 네임 스페이스에 대 한 문 (이 경우 `Com.Xamarin.Textcounter`):

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

컴파일 및 실행 된 **BindingTest** 프로젝트. 응용 프로그램 시작 되며 스크린샷에서 왼쪽에 있는 (의 `EditText` 일부 텍스트를 사용 하 여 초기화 되 변경 하 여 얻을 수 있지만). 탭 하면 **COUNT 모음**, 오른쪽에 표시 된 것 처럼 알림 메시지 모음 수가 표시 됩니다.

[![BindingTest 실행의 스크린 샷](binding-an-aar-images/12-count-vowels.png)](binding-an-aar-images/12-count-vowels.png#lightbox)

눌러 보십시오는 **COUNT 자음** 단추입니다. 또한 텍스트의 줄을 수정 하 고 다시 다른 모음을 테스트 하려면이 단추를 탭 하 고 자음 셉니다.



### <a name="accessing-aar-resources"></a>에 액세스합니다. AAR 리소스

병합 도구 Xamarin는 **R** 의 데이터는 합니다. 응용 프로그램에 AAR **리소스** 클래스입니다. 결과적으로, 액세스할 수 있습니다. AAR 리소스 레이아웃에서 (및 코드 숨김 파일에서)에서 같은 방식으로에서 있는 리소스 액세스 하는 것은 **리소스** 프로젝트의 경로입니다.

이미지 리소스에 액세스 하려면 사용 된 **Resource.Drawable** 내 압축 된 이미지에 대 한 이름을 합니다. AAR 합니다. 참조할 수는 예를 들어 **image.png** 에 합니다. 사용 하 여 AAR 파일 `@drawable/image`:

```xml
<ImageView android:src="@drawable/image" ... />
```

에 있는 리소스 레이아웃에 액세스할 수도 있습니다는 합니다. AAR 합니다. 이 위해 사용 하는 **Resource.Layout** 패키지 된 레이아웃에 대 한 이름에서. AAR 합니다. 예를 들어:

```csharp
var a = new ArrayAdapter<string>(this, Resource.Layout.row_layout, ...);
```

**textanalyzer.aar** 예제에 있는 이미지 파일에 포함 되어 **res/drawable/monkey.png**합니다. 이 이미지 리소스에 액세스 하 고 예제 앱에서 사용 보겠습니다.

편집 된 **BindingTest** 레이아웃 (**Main.axml**) 추가 하 고는 `ImageView` 의 끝에는 `LinearLayout` 컨테이너. 이 `ImageView` 에 이미지 표시 **@drawable/monkey**;이 이미지의 리소스 섹션에서 로드 됩니다. **textanalyzer.aar**:

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

컴파일 및 실행 된 **BindingTest** 프로젝트. 응용 프로그램 시작 되며 스크린샷에서 왼쪽에 있는 &ndash; 탭 하면 **COUNT 자음**, 오른쪽에 표시 된 것 처럼 결과가 표시 됩니다.

[![BindingTest 자음 개수를 표시 합니다.](binding-an-aar-images/13-count-consonants.png)](binding-an-aar-images/13-count-consonants.png#lightbox)


지금까지 Java 라이브러리에 성공적으로 연결 했습니다. AAR!



## <a name="summary"></a>요약

이 연습에서 만든에 대 한 바인딩 라이브러리는 합니다. AAR 파일을 최소한의 테스트 응용 프로그램에 바인딩 라이브러리를 추가 하 고 C# 코드에 있는 Java 코드를 호출할 수 있는지 확인 하려면 앱으로 실행 된 합니다. AAR 파일입니다.
또한 응용 프로그램에서 액세스 하 고 표시에 있는 이미지 리소스 확장의 합니다. AAR 파일입니다.


## <a name="related-links"></a>관련 링크

- [작성 Java 바인딩 라이브러리 (비디오)](https://university.xamarin.com/classes#10090)
- [JAR 바인딩](~/android/platform/binding-java-library/binding-a-jar.md)
- [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)
- [AarBinding (샘플)](https://developer.xamarin.com/samples/monodroid/JavaIntegration/AarBinding)
- [버그 44573 하나 프로젝트 여러.aar 파일을 바인딩할 수 없습니다.](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)
