---
title: .AAR 바인딩
description: 이 연습에서는 Android에서 Xamarin.Android Java 바인딩 라이브러리를 만들기 위한 단계별 지침을 제공 합니다. AAR 파일입니다.
ms.prod: xamarin
ms.assetid: 380413B8-6A99-4BB8-B64C-3EAF9F359C22
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/11/2018
ms.openlocfilehash: 7f71ccf4ff61c176e73be6d3855136697a5c2130
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60958175"
---
# <a name="binding-an-aar"></a>.AAR 바인딩

_이 연습에서는 Android에서 Xamarin.Android Java 바인딩 라이브러리를 만들기 위한 단계별 지침을 제공 합니다. AAR 파일입니다._


## <a name="overview"></a>개요

*Android 보관 (합니다. AAR)* 파일은 Android 라이브러리에 대 한 파일 형식입니다.
합니다. AAR 파일을 합니다. 다음을 포함 하는 ZIP 보관 파일:

-   컴파일된 Java 코드
-   리소스 Id
-   자료
-   메타 데이터 (예를 들어, 활동 선언, 사용 권한)

이 가이드에서는 단일 바인딩 라이브러리 만들기의 기본 사항을 단계별로 실행 했습니다. AAR 파일입니다. Java 라이브러리 바인딩 일반적 (기본 코드 예제)의 개요를 보려면 [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)합니다.


> [!IMPORTANT]
> 바인딩 프로젝트를 하나씩만 포함할 수 있습니다. AAR 파일입니다. 경우는 합니다. 다른 AAR 종속성입니다. AAR, 다음 해당 종속성 자신의 바인딩 프로젝트에 포함 되어 있고 후 참조 합니다. 참조 [44573 버그](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)합니다.


## <a name="walkthrough"></a>연습

Android Studio에서 만든 Android 보관 파일을 예제에 대 한 바인딩 라이브러리를 만들겠습니다 [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true)합니다. 이 있습니다. AAR 포함을 `TextCounter` 모음 및 문자열의 자음의 수를 계산 하는 정적 메서드를 사용 하 여 클래스입니다. 또한 **textanalyzer.aar** 계산 결과 표시 하는 데 이미지 리소스를 포함 합니다.

바인딩 라이브러리를 만들려면 다음 단계에서는 합니다. AAR 파일:

1. 새 Java 바인딩 라이브러리 프로젝트를 만듭니다.

2. 단일을 추가 합니다. AAR 파일 프로젝트입니다. 단일 바인딩 프로젝트를 포함할 수 있습니다. AAR 합니다.

3. 에 대 한 적절 한 빌드 동작을 설정 합니다. AAR 파일입니다.

4. 대상 프레임 워크를 선택 합니다. AAR 지원합니다.

5. 바인딩 라이브러리를 빌드하십시오.

바인딩 라이브러리를 만들고 나면 텍스트 문자열을 호출 하 라는 작은 Android 앱을 개발 했습니다. AAR 텍스트 분석 방법에서 이미지를 검색 합니다. AAR, 및 이미지와 함께 결과 표시 합니다.

샘플 앱 액세스를 `TextCounter` 클래스의 **textanalyzer.aar**:

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

이 샘플 앱 검색 하 고에 패키지 된 이미지 리소스를 표시 합니다. 또한 **textanalyzer.aar**:

[![Xamarin monkey 이미지](binding-an-aar-images/00-monkey-sml.png)](binding-an-aar-images/00-monkey.png#lightbox)

이 이미지 리소스에 상주 **res/drawable/monkey.png** 에 **textanalyzer.aar**합니다.



### <a name="creating-the-bindings-library"></a>바인딩 라이브러리 만들기

다음 단계를 사용 하 여을 시작 하기 전에 예제를 다운로드 하세요 [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true) Android 보관 파일:

1.  Android 바인딩 라이브러리 템플릿을 사용 하 여 시작 되는 새 바인딩 라이브러리 프로젝트를 만듭니다. Mac 용 Visual Studio 또는 Visual Studio (아래 스크린샷은 Visual Studio를 표시 하지만 Mac 용 Visual Studio는 매우 유사)을 사용할 수 있습니다. 솔루션의 이름을 **AarBinding**:

    [![AarBindings 프로젝트 만들기](binding-an-aar-images/01-new-bindings-library-vs-sml.w157.png)](binding-an-aar-images/01-new-bindings-library-vs.w157.png#lightbox)

2.  템플릿에 포함 되어는 **Jars** 폴더를 추가 하는 프로그램입니다. 바인딩 라이브러리 프로젝트에 AAR(s)입니다. 마우스 오른쪽 단추로 클릭 합니다 **Jar** 선택한 폴더 **추가 > 기존 항목**:

    [![기존 항목 추가](binding-an-aar-images/02-add-existing-item-vs-sml.png)](binding-an-aar-images/02-add-existing-item-vs.png#lightbox)


3.  로 이동 합니다 **textanalyzer.aar** 이전에 다운로드 한 파일을 선택 하 고 클릭 **추가**:

    [![Textanalayzer.aar 추가](binding-an-aar-images/03-select-aar-file-vs-sml.png)](binding-an-aar-images/03-select-aar-file-vs.png#lightbox)


4.  있는지 확인 합니다 **textanalyzer.aar** 파일이 프로젝트에 추가 되었습니다.

    [![Textanalyzer.aar 파일이 추가 된](binding-an-aar-images/04-aar-added-vs-sml.png)](binding-an-aar-images/04-aar-added-vs.png#lightbox)

5.  빌드 작업을 설정 **textanalyzer.aar** 에 `LibraryProjectZip`입니다. Mac 용 Visual Studio에서 마우스 오른쪽 단추로 클릭 **textanalyzer.aar** 빌드 작업을 설정 합니다. Visual Studio에서 빌드 작업에서 설정할 수 있습니다 합니다 **속성** 창).

    [![Textanalyzer.aar 빌드 작업 LibraryProjectZip을로 설정](binding-an-aar-images/05-embedded-aar-vs-sml.png)](binding-an-aar-images/05-embedded-aar-vs.png#lightbox)

6.  프로젝트 속성을 구성 합니다 *대상 프레임 워크*합니다. 경우는 합니다. AAR Android Api를 사용 하 여, API 수준으로 대상 프레임 워크를 설정 합니다. AAR 필요합니다. (대상 프레임 워크 설정과 일반에서 Android API 수준에 대 한 자세한 내용은 참조 하세요. [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md).)

    바인딩 라이브러리에 대 한 대상 API 수준을 설정 합니다. 이 예제에서는 무료 때문에 최신 플랫폼 API 수준 (API 레벨 23)를 사용 하는 우리의 **textanalyzer** Android Api에 대 한 종속성이 없습니다.

    [![API 23을 대상 수준 설정](binding-an-aar-images/06-set-target-framework-vs-sml.png)](binding-an-aar-images/06-set-target-framework-vs.png#lightbox)

7.  바인딩 라이브러리를 빌드하십시오. 바인딩 라이브러리 프로젝트를 성공적으로 작성 하 고 출력을 생성 해야 합니다. 다음 위치에서 DLL: **AarBinding/bin/Debug/AarBinding.dll**



### <a name="using-the-bindings-library"></a>바인딩 라이브러리를 사용 하 여

이 사용 하 합니다. Xamarin android 앱에서 DLL을 먼저 바인딩 라이브러리에 대 한 참조를 추가 해야 합니다. 다음 단계를 사용 합니다.

1.  이 연습을 단순화 하기 위해 바인딩 라이브러리와 동일한 솔루션에서이 앱을 만들겠습니다. (바인딩 라이브러리를 사용 하는 앱 다른 솔루션에는 있을 수도 있습니다.) 새 Xamarin.Android 앱을 만듭니다: 솔루션을 마우스 오른쪽 단추로 클릭 **새 프로젝트 추가**합니다. 새 프로젝트의 이름을 **BindingTest**:

    [![새 BindingTest 프로젝트 만들기](binding-an-aar-images/07-add-new-project-vs-sml.w157.png)](binding-an-aar-images/07-add-new-project-vs.w157.png#lightbox)

2.  마우스 오른쪽 단추로 클릭 합니다 **참조** 노드의 합니다 **BindingTest** 프로젝트를 마우스 **참조 추가...** :

    [![참조 추가 클릭](binding-an-aar-images/08-add-reference-vs-sml.png)](binding-an-aar-images/08-add-reference-vs.png#lightbox)

3.  선택 된 **AarBinding** 앞에서 만든 프로젝트 및 클릭 **확인**:

    [![AAR 바인딩 프로젝트 체크 인](binding-an-aar-images/09-choose-aar-binding-vs-sml.png)](binding-an-aar-images/09-choose-aar-binding-vs.png#lightbox)

4.  열기는 **참조** 노드의 합니다 **BindingTest** 되었는지 확인 하는 프로젝트를 **AarBinding** 참조가 있습니다:

    [![AarBinding 참조 아래에 나열 됩니다.](binding-an-aar-images/10-references-shows-aarbinding-vs-sml.png)](binding-an-aar-images/10-references-shows-aarbinding-vs.png#lightbox)


바인딩 라이브러리 프로젝트의 내용을 보려면 원하는 두 번에서 열에 대 한 참조를 **개체 브라우저**합니다. 매핑된 내용의 표시 합니다 `Com.Xamarin.Textcounter` 네임 스페이스 (Java에서 매핑된 `com.xamarin.textanalyzezr` 패키지)의 멤버를 볼 수 있습니다는 `TextCounter` 클래스:

[![개체 브라우저 보기](binding-an-aar-images/11-object-browser-vs-sml.png)](binding-an-aar-images/11-object-browser-vs.png#lightbox)

위의 스크린샷에서 두 강조 표시 `TextAnalyzer` 메서드를 호출 하는 예제 앱: `NumConsonants` (기본 Java를 래핑하는 `numConsonants` 메서드), 및 `NumVowels` (기본 Java를 래핑하는 `numVowels` 메서드).



### <a name="accessing-aar-types"></a>에 액세스합니다. AAR 형식

바인딩 라이브러리에는 앱에 대 한 참조를 추가한 후에 Java 형식에 액세스할 수 있습니다 합니다. AAR 때 액세스 C# 형식 (덕분에 C# 래퍼). C#앱 코드를 호출할 수 `TextAnalyzer` 메서드가 예에서 확인할 수 있습니다.

```csharp
using Com.Xamarin.Textcounter;
...
int numVowels = TextCounter.NumVowels (myText);
int numConsonants = TextCounter.NumConsonants (myText);
```

위의 예에서 정적 메서드를 호출 하는 것은 `TextCounter` 클래스입니다. 그러나 또한 클래스를 인스턴스화하고 인스턴스 메서드를 호출 합니다. 예를 들어, 경우에 합니다. AAR 라는 클래스를 래핑하고 `Employee` 인스턴스 메서드가 `buildFullName`를 인스턴스화할 수 있습니다 `MyClass` 사용 볼 수 있듯이 여기:

```csharp
var employee = new Com.MyCompany.MyProject.Employee();
var name = employee.BuildFullName ();
```

다음 단계에서는 사용자에 게 텍스트를 사용 하 여 표시 되도록 앱에 코드 추가 `TextCounter` 분석할 텍스트 및 다음 결과 표시 합니다.

대체는 **BindingTest** 레이아웃 (**Main.axml**) 다음 XML을 사용 하 여 합니다. 이 레이아웃에는 `EditText` 모음과 자음 개수를 시작 하기 위한 두 가지 단추 및 텍스트 입력에 대해:

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

내용을 바꿉니다 **MainActivity.cs** 다음 코드를 사용 합니다. 이 예제에서 볼 수 있듯이 단추 이벤트 처리기 호출 래핑 `TextCounter` 에 상주 하는 메서드를 합니다. 결과 표시 하려면 알림 AAR 및 사용 합니다. 알림 합니다 `using` 바인딩된 라이브러리의 네임 스페이스에 대 한 문 (이 예제의 경우 `Com.Xamarin.Textcounter`):

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

컴파일 및 실행 합니다 **BindingTest** 프로젝트입니다. 앱 시작 되며 왼쪽에서 스크린샷 제공 (의 `EditText` 일부 텍스트를 사용 하 여 초기화 됩니다 변경 되도록 탭 수 있지만). 탭 **개수 모음**, 알림 오른쪽에 표시 된 것과 같이 모음 수가 표시 됩니다.

[![BindingTest 실행의 스크린샷](binding-an-aar-images/12-count-vowels.png)](binding-an-aar-images/12-count-vowels.png#lightbox)

탭을 시도 합니다 **개수 자음** 단추입니다. 또한 텍스트 줄을 수정 하 고 이러한 단추 다른 모음에 대 한 테스트를 다시 탭 수 및 자음을 계산 합니다.



### <a name="accessing-aar-resources"></a>에 액세스합니다. AAR 리소스

Xamarin 병합 도구를 **R** 데이터로 합니다. 앱에 AAR **리소스** 클래스입니다. 결과적으로 액세스할 수 있습니다. AAR 리소스 레이아웃에서 (및 코드 숨김 파일에서)에 있는 리소스를 액세스 하는 방식으로 동일한 합니다 **리소스** 프로젝트의 경로입니다.

이미지 리소스에 액세스 하려면 사용 합니다 **Resource.Drawable** 내에서 압축 된 이미지에 대 한 이름을 합니다. AAR 합니다. 예를 들어 참조할 수 있습니다 **image.png** 에서 합니다. 사용 하 여 AAR 파일 `@drawable/image`:

```xml
<ImageView android:src="@drawable/image" ... />
```

에 있는 리소스 레이아웃에 액세스할 수도 있습니다는. AAR 합니다. 이 작업을 수행 하려면 사용 합니다 **Resource.Layout** 레이아웃 내에서 패키지에 대 한 이름 합니다. AAR 합니다. 예를 들어:

```csharp
var a = new ArrayAdapter<string>(this, Resource.Layout.row_layout, ...);
```

합니다 **textanalyzer.aar** 에 있는 이미지 파일을 포함 하는 예제 **res/drawable/monkey.png**합니다. 이 이미지 리소스에 액세스 해 예제에서는 앱에서 사용할 보겠습니다.

편집 합니다 **BindingTest** 레이아웃 (**Main.axml**) 추가 하 고는 `ImageView` 의 끝에는 `LinearLayout` 컨테이너. 이렇게 `ImageView` 에 있는 이미지를 표시 **@drawable/monkey**;이 이미지의 리소스 섹션에서 로드 됩니다 **textanalyzer.aar**:

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

컴파일 및 실행 합니다 **BindingTest** 프로젝트입니다. 앱 시작 되며 스크린샷에서 왼쪽에 있는 &ndash; 탭 하면 **개수 자음**, 결과 오른쪽에 표시 된 것과 같이 표시 됩니다.

[![BindingTest 자음 수를 표시 합니다.](binding-an-aar-images/13-count-consonants.png)](binding-an-aar-images/13-count-consonants.png#lightbox)


축하합니다. Java 라이브러리를 성공적으로 연결 했습니다. AAR!



## <a name="summary"></a>요약

이 연습에 대 한 바인딩 라이브러리 만들었습니다를 합니다. AAR 파일을 최소한의 테스트 앱에 바인딩 라이브러리를 추가 하 고 실행 중인지 확인 하려면 앱이 C# 코드에 있는 Java 코드를 호출할 수 있습니다 합니다. AAR 파일입니다.
또한에서는 확장 앱에 액세스 하에 있는 이미지 리소스를 표시 합니다. AAR 파일입니다.


## <a name="related-links"></a>관련 링크

- [Java 바인딩 라이브러리 (비디오) 빌드](https://university.xamarin.com/classes#10090)
- [JAR 바인딩](~/android/platform/binding-java-library/binding-a-jar.md)
- [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)
- [AarBinding (샘플)](https://developer.xamarin.com/samples/monodroid/JavaIntegration/AarBinding)
- [버그 44573 하나 프로젝트 여러.aar 파일을 바인딩할 수 없습니다.](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)
