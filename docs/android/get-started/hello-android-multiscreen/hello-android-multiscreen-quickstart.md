---
title: 'Hello, Android 멀티스크린: 빠른 시작'
description: 두 부분으로 구성된 이 가이드는 Phoneword 응용 프로그램을 확장하여 두 번째 화면을 처리합니다. 이 과정에서 Android 아키텍처에 대해 자세히 알아보려면 기본 Android 응용 프로그램 구성 요소를 도입합니다.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: ED99584A-BA3B-429A-AEE5-CF3CB0116762
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/30/2018
ms.openlocfilehash: d8f909ab522b5bbf08a2b666fd4f64340e60b3e5
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
---
# <a name="hello-android-multiscreen-quickstart"></a>Hello, Android 멀티스크린: 빠른 시작

_두 부분으로 구성된 이 가이드는 Phoneword 응용 프로그램을 확장하여 두 번째 화면을 처리합니다. 이 과정에서 Android 아키텍처에 대해 자세히 알아보려면 기본 Android 응용 프로그램 구성 요소를 도입합니다._

## <a name="hello-android-multiscreen-quickstart"></a>Hello, Android 멀티스크린 빠른 시작

이 가이드의 연습 부분에서는 앱을 사용하여 변역된 숫자의 기록을 추적하기 위해 두 번째 화면을 [Phoneword](https://developer.xamarin.com/samples/monodroid/Phoneword/) 응용 프로그램에 추가합니다. 오른쪽 스크린샷에 표시된 것처럼 [최종 응용 프로그램](https://developer.xamarin.com/samples/monodroid/PhonewordMultiscreen/)에 "변환된" 수를 표시하는 두 번째 화면이 포함됩니다.

[![예제 앱 스크린샷](hello-android-multiscreen-quickstart-images/screenshot-sml.png)](hello-android-multiscreen-quickstart-images/screenshot.png#lightbox)

함께 제공된 [심층 분석](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-deepdive.md)은 빌드된 기능을 검토하고, 해당 과정에서 발생한 아키텍처, 탐색 및 기타 새로운 Android 개념에 대해 설명합니다.


## <a name="requirements"></a>요구 사항

이 가이드가 [Hello, Android](~/android/get-started/hello-android/index.md)를 사용하지 않는 위치를 선택하기 때문에 [Hello, Android 빠른 시작](~/android/get-started/hello-android/hello-android-quickstart.md)을 완료해야 합니다.
아래의 연습으로 직접 이동하려는 경우 전체 버전의 [Phoneword](https://developer.xamarin.com/samples/monodroid/Phoneword/)(Hello, Android 빠른 시작)를 다운로드하고 시작하여 연습을 시작할 수 있습니다.

## <a name="walkthrough"></a>연습

이 연습에서는 **Phoneword** 응용 프로그램에 **변환 기록** 화면을 추가합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio에서 **Phoneword** 응용 프로그램을 열고 **솔루션 탐색기**에서 **Main.axml** 파일을 편집하여 시작합니다.

### <a name="updating-the-layout"></a>레이아웃 업데이트

**도구 상자**에서 **단추**를 디자인 화면에 끌어와서 **TranslatedPhoneWord** TextView 아래에 배치합니다. **속성** 창에서 **ID** 단추를 `@+id/TranslationHistoryButton`로 변경합니다. 

[![새 단추 끌어오기](hello-android-multiscreen-quickstart-images/vs/02-new-button-sml.png)](hello-android-multiscreen-quickstart-images/vs/02-new-button.png#lightbox)

단추의 **Text** 속성을 `@string/translationHistory`로 설정합니다. Android Designer는 문자 그대로 이를 변환하지만 단추의 텍스트가 올바르게 표시되도록 몇 가지를 변경해야 합니다.

[![변환 기록 단추 텍스트 설정](hello-android-multiscreen-quickstart-images/vs/03-translation-history-string-sml.png)](hello-android-multiscreen-quickstart-images/vs/03-translation-history-string.png#lightbox)

**솔루션 탐색기**의 **리소스** 폴더에서 **값** 노드를 확장하고, 문자열 리소스 파일인 **Strings.xml**을 두 번 클릭합니다.

[![Strings.xml 열기](hello-android-multiscreen-quickstart-images/vs/04-strings-resources-file-sml.png)](hello-android-multiscreen-quickstart-images/vs/04-strings-resources-file.png#lightbox)

`translationHistory` 문자열 이름 및 값을 **Strings.xml** 파일에 추가하고 저장합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Translation History</string>
    <string name="ApplicationName">Phoneword</string>
</resources>
```

**변환 기록** 단추 텍스트는 새 문자열 값을 반영하도록 업데이트되어야 합니다.

[![새 문자열 값을 반영하는 단추](hello-android-multiscreen-quickstart-images/vs/05-new-string-value.png)](hello-android-multiscreen-quickstart-images/vs/05-new-string-value.png#lightbox)

디자인 화면에서 선택한 **변환 기록** 단추를 사용하여 **속성** 창에서 `enabled` 설정을 찾고 값을 `false`로 설정하여 단추를 비활성화합니다. 그러면 단추의 색깔이 디자인 화면에서 짙어집니다.

[![변환 기록 단추 사용 안 함](hello-android-multiscreen-quickstart-images/vs/06-enabled-false-sml.png)](hello-android-multiscreen-quickstart-images/vs/06-enabled-false.png#lightbox)

### <a name="creating-the-second-activity"></a>두 번째 작업 만들기

두 번째 화면에 전원을 공급하는 두 번째 작업을 만듭니다. **솔루션 탐색기**에서 **Phoneword** 프로젝트를 마우스 오른쪽 단추로 클릭하고, **추가 > 새 항목...** 을 선택합니다.

[![새 파일 추가](hello-android-multiscreen-quickstart-images/vs/07-add-new-file-sml.png)](hello-android-multiscreen-quickstart-images/vs/07-add-new-file.png#lightbox)

**새 항목 추가** 대화 상자에서 **Visual C# > 작업**을 선택하고 작업 파일의 이름을 **TranslationHistoryActivity.cs**로 지정합니다.

**TranslationHistoryActivity.cs**의 템플릿 코드를 다음으로 바꿉니다.

```csharp
using System;
using System.Collections.Generic;
using Android.App;
using Android.OS;
using Android.Widget;
namespace Phoneword
{
    [Activity(Label = "@string/translationHistory")]
    public class TranslationHistoryActivity : ListActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            // Create your application here
            var phoneNumbers = Intent.Extras.GetStringArrayList("phone_numbers") ?? new string[0];
            this.ListAdapter = new ArrayAdapter<string>(this, Android.Resource.Layout.SimpleListItem1, phoneNumbers);
        }
    }
}
```

이 작업에 새 레이아웃 파일을 만들 필요가 없도록 이 클래스에서 `ListActivity`을 만들고 프로그래밍 방식으로 채웁니다. [Hello, Android 멀티스크린 심층 분석](~/android/get-started/hello-android/hello-android-deepdive.md)에서 자세히 설명되어 있습니다.

### <a name="adding-translation-history-code"></a>변환 기록 코드 추가

이 앱은 전화 번호(사용자가 첫 번째 화면에서 변환함)를 수집하고 두 번째 화면으로 전달합니다. 전화 번호는 문자열 목록으로 저장됩니다. 목록(및 나중에 사용되는 의도)을 지원하려면 다음 `using` 지시문을 **MainActivity.cs** 맨 위에 추가합니다.

```csharp
using System.Collections.Generic;
using Android.Content;
```

다음으로 전화 번호로 채울 수 있는 빈 목록을 만듭니다.
`MainActivity` 클래스는 다음과 같습니다.

```csharp
[Activity(Label = "Phoneword", MainLauncher = true)]
public class MainActivity : Activity
{
    static readonly List<string> phoneNumbers = new List<string>();
    ...// OnCreate, etc.
}
```

`MainActivity` 클래스에서 다음 코드를 추가하여 **변환 기록** 단추를 등록합니다(이 줄을 `translateButton` 선언 뒤에 배치함).

```csharp
Button translationHistoryButton = FindViewById<Button> (Resource.Id.TranslationHistoryButton);
```

다음 코드를 `OnCreate` 메서드의 끝에 추가하여 **변환 기록** 단추를 연결합니다.

```csharp
translationHistoryButton.Click += (sender, e) =>
{
    var intent = new Intent(this, typeof(TranslationHistoryActivity));
    intent.PutStringArrayListExtra("phone_numbers", phoneNumbers);
    StartActivity(intent);
};
```

**변환** 단추를 업데이트하여 전화 번호를 `phoneNumbers` 목록에 추가합니다. `TranslateHistoryButton`에 대한 `Click` 처리기는 다음 코드와 유사합니다.

```csharp
// Add code to translate number
string translatedNumber = string.Empty;
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = "";
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
        phoneNumbers.Add(translatedNumber);
        translationHistoryButton.Enabled = true;
    }
};
```

응용 프로그램을 저장하고 빌드하여 오류가 없는지 확인합니다.

### <a name="running-the-app"></a>앱 실행

에뮬레이터 또는 장치에 응용 프로그램을 배포합니다. 다음 스크린샷에서는 실행 중인 **Phoneword** 응용 프로그램을 설명합니다.

[![예제 스크린샷](hello-android-multiscreen-quickstart-images/screenshot-sml.png)](hello-android-multiscreen-quickstart-images/screenshot.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac용 Visual Studio에서 **Phoneword** 프로젝트를 열고 **솔루션 패드**에서 **Main.axml** 파일을 편집하여 시작합니다.

### <a name="updating-the-layout"></a>레이아웃 업데이트

**도구 상자**에서 **단추**를 디자인 화면에 끌어와서 **TranslatedPhoneWord** TextView 아래에 배치합니다. **속성** 패드에서 **ID** 단추를 `@+id/TranslationHistoryButton`로 변경합니다. 

[![새 단추 끌어오기](hello-android-multiscreen-quickstart-images/xs/02-new-button-sml.png)](hello-android-multiscreen-quickstart-images/xs/02-new-button.png#lightbox)

단추의 **Text** 속성을 `@string/translationHistory`로 설정합니다. Android Designer는 문자 그대로 이를 변환하지만 단추의 텍스트가 올바르게 표시되도록 몇 가지를 변경해야 합니다.

[![변환 기록 단추 텍스트 설정](hello-android-multiscreen-quickstart-images/xs/03-call-history-string-sml.png)](hello-android-multiscreen-quickstart-images/xs/03-call-history-string.png#lightbox)


**솔루션 패드**의 **리소스** 폴더에서 **값** 노드를 확장하고, 문자열 리소스 파일인 **Strings.xml**을 두 번 클릭합니다.

[![문자열 열기](hello-android-multiscreen-quickstart-images/xs/04-strings-resources-file-sml.png)](hello-android-multiscreen-quickstart-images/xs/04-strings-resources-file.png#lightbox)


`translationHistory` 문자열 이름 및 값을 **Strings.xml** 파일에 추가하고 저장합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Translation History</string>
    <string name="ApplicationName">Phoneword</string>
</resources>
```

**변환 기록** 단추 텍스트는 새 문자열 값을 반영하도록 업데이트되어야 합니다.

[![새 문자열 값을 반영하는 단추](hello-android-multiscreen-quickstart-images/xs/05-new-string-value-sml.png)](hello-android-multiscreen-quickstart-images/xs/05-new-string-value.png#lightbox)


디자인 화면에서 선택한 **변환 기록** 단추를 사용하여 **속성 패드**에서 **동작** 탭을 열고, **사용함** 확인란을 두 번 클릭하여 단추를 비활성화합니다. 그러면 단추의 색깔이 디자인 화면에서 짙어집니다.

[![변환 기록 단추 사용 안 함](hello-android-multiscreen-quickstart-images/xs/06-enabled-false-sml.png)](hello-android-multiscreen-quickstart-images/xs/06-enabled-false.png#lightbox)

### <a name="creating-the-second-activity"></a>두 번째 작업 만들기

두 번째 화면에 전원을 공급하는 두 번째 작업을 만듭니다. **솔루션 패드**에서 **Phoneword** 프로젝트 옆에 있는 회색 기어 아이콘을 클릭하고 **추가 > 새 파일...** 을 선택합니다.

**새 파일** 대화 상자에서 **Android > 작업**을 선택하고 작업 이름을 `TranslationHistoryActivity`로 지정한 다음, **추가**를 클릭합니다.

`TranslationHistoryActivity`의 템플릿 코드를 다음으로 바꿉니다.

```csharp
using System;
using System.Collections.Generic;
using Android.App;
using Android.OS;
using Android.Widget;
namespace Phoneword
{
    [Activity(Label = "@string/translationHistory")]
    public class TranslationHistoryActivity : ListActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            // Create your application here
            var phoneNumbers = Intent.Extras.GetStringArrayList("phone_numbers") ?? new string[0];
            this.ListAdapter = new ArrayAdapter<string>(this, Android.Resource.Layout.SimpleListItem1, phoneNumbers);
        }
    }
}
```

이 작업에 새 레이아웃 파일을 만들 필요가 없도록 이 클래스에서 `ListActivity`을 만들고 프로그래밍 방식으로 채웁니다. [Hello, Android 멀티스크린 심층 분석](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-deepdive.md)에서 자세히 설명되어 있습니다.

### <a name="adding-translation-history-code"></a>변환 기록 코드 추가

이 앱은 전화 번호(사용자가 첫 번째 화면에서 변환함)를 수집하고 두 번째 화면으로 전달합니다. 전화 번호는 문자열 목록으로 저장됩니다. 목록(및 나중에 사용되는 의도)을 지원하려면 다음 `using` 지시문을 **MainActivity.cs** 맨 위에 추가합니다.

```csharp
using System.Collections.Generic;
using Android.Content;
```

다음으로 전화 번호로 채울 수 있는 빈 목록을 만듭니다. `MainActivity` 클래스는 다음과 같습니다.

```csharp
[Activity(Label = "Phoneword", MainLauncher = true)]
public class MainActivity : Activity
{
    static readonly List<string> phoneNumbers = new List<string>();
    ...// OnCreate, etc.
}
```

`MainActivity` 클래스에서 다음 코드를 추가하여 **TranslationHistory 기록** 단추를 등록합니다(이 줄을 `TranslationHistoryButton` 선언 뒤에 배치함).

```csharp
Button translationHistoryButton = FindViewById<Button> (Resource.Id.TranslationHistoryButton);
```

다음 코드를 `OnCreate` 메서드의 끝에 추가하여 **변환 기록** 단추를 연결합니다.

```csharp
translationHistoryButton.Click += (sender, e) =>
{
    var intent = new Intent(this, typeof(TranslationHistoryActivity));
    intent.PutStringArrayListExtra("phone_numbers", phoneNumbers);
    StartActivity(intent);
};
```

**변환** 단추를 업데이트하여 전화 번호를 `phoneNumbers` 목록에 추가합니다. `TranslateHistoryButton`에 대한 `Click` 처리기는 다음 코드와 유사합니다.

```csharp
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = "";
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
        phoneNumbers.Add(translatedNumber);
        translationHistoryButton.Enabled = true;
    }
};
```

### <a name="running-the-app"></a>앱 실행

에뮬레이터 또는 장치에 응용 프로그램을 배포합니다. 다음 스크린샷에서는 실행 중인 **Phoneword** 응용 프로그램을 설명합니다.

[![예제 스크린샷](hello-android-multiscreen-quickstart-images/screenshot.png)](hello-android-multiscreen-quickstart-images/screenshot.png#lightbox)

-----

첫 번째 멀티스크린 Xamarin.Android 응용 프로그램을 완성한 것을 축하합니다! 이제 사용할 &ndash;에 대해 알아본 도구 및 기술을 분석하겠습니다. 다음은 [Hello, Android 멀티스크린 심층 분석](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-deepdive.md)입니다.


## <a name="related-links"></a>관련 링크

- [Xamarin 앱 아이콘 및 시작 화면(ZIP)](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true)
- [Phoneword(샘플)](https://developer.xamarin.com/samples/monodroid/Phoneword)
- [PhonewordMultiscreen(샘플)](https://developer.xamarin.com/samples/monodroid/PhonewordMultiscreen)
