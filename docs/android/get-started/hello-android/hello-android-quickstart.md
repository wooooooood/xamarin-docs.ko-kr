---
title: 'Hello, Android: 빠른 시작'
description: 두 부분으로 구성된 이 가이드에서는 Mac용 Visual Studio 또는 Visual Studio를 사용하여 첫 번째 Xamarin.Android 응용 프로그램을 빌드하고, Xamarin을 사용하여 Android 응용 프로그램 개발에 대한 기본 사항을 이해하기 시작합니다. 이 과정에서 Xamarin.Android 응용 프로그램을 빌드하고 배포하는 데 필요한 도구, 개념 및 단계를 소개합니다.
ms.topic: article
ms.prod: xamarin
ms.assetid: 44007FA1-3ABC-4935-BF52-4613AF0553A6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: bbe243b108be6e0060ba9067db58a9875c7b5153
ms.sourcegitcommit: cc38757f56aab53bce200e40f873eb8d0e5393c3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2018
---
# <a name="hello-android-quickstart"></a>Hello, Android: 빠른 시작

_두 부분으로 구성된 이 가이드에서는 Mac용 Visual Studio 또는 Visual Studio를 사용하여 첫 번째 Xamarin.Android 응용 프로그램을 빌드하고, Xamarin을 사용하여 Android 응용 프로그램 개발에 대한 기본 사항을 이해하기 시작합니다. 이 과정에서 Xamarin.Android 응용 프로그램을 빌드하고 배포하는 데 필요한 도구, 개념 및 단계를 소개합니다._

## <a name="hello-android-quickstart"></a>Hello, Android 빠른 시작

이 연습에서는 사용자가 입력한 영숫자 전화 번호를 숫자 전화 번호로 변환하고 해당 숫자 전화 번호를 사용자에게 표시하는 응용 프로그램을 만듭니다. 최종 응용 프로그램은 다음과 같습니다.

[![완료 시 앱 스크린샷](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)


## <a name="requirements"></a>요구 사항

이 연습을 수행하려면 다음이 필요합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   Windows 7 이상

-   Visual Studio 2015 Professional 이상

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   최신 버전의 Mac용 Visual Studio

-   OS X Yosemite 이상

-----

이 연습에서는 최신 버전의 Xamarin.Android를 설치하고 선택한 플랫폼에서 실행하고 있다고 가정합니다. Xamarin.Android를 설치하는 가이드는 [Xamarin.Android 설치](~/android/get-started/installation/index.md) 가이드를 참조하세요.
시작하기 전에 [Xamarin 앱 아이콘 및 시작 화면](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true) 설정을 다운로드하고 압축을 푸세요.

## <a name="configuring-emulators"></a>에뮬레이터 구성

Google Android SDK 에뮬레이터를 사용하는 경우 하드웨어 가속을 사용하도록 에뮬레이터를 구성하는 것이 좋습니다. 하드웨어 가속을 구성하는 지침은 [하드웨어 가속](~/android/get-started/installation/android-emulator/hardware-acceleration.md)에서 제공됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio Android 에뮬레이터를 사용하는 경우 컴퓨터에서 Hyper-V를 사용할 수 있어야 합니다. Visual Studio Android 에뮬레이터를 구성하는 방법에 대한 자세한 내용은 [Android용 Visual Studio 에뮬레이터에 대한 시스템 요구 사항](https://msdn.microsoft.com/en-us/library/mt228280.aspx)을 참조하세요.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-----

## <a name="walkthrough"></a>연습

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio를 시작합니다.  **파일 > 새로 만들기 > 프로젝트**를 클릭하여 새 프로젝트를 만듭니다.

**새 프로젝트** 대화 상자에서 **비어 있는 앱(Android)** 템플릿을 클릭합니다.
새 프로젝트의 이름을 `Phoneword`로 지정합니다. **확인**을 클릭하여 새 프로젝트를 만듭니다.

[![새 프로젝트는 Phoneword입니다.](hello-android-quickstart-images/vs/02-new-project-name-sml.png)](hello-android-quickstart-images/vs/02-new-project-name.png#lightbox)

### <a name="creating-the-layout"></a>레이아웃 만들기

새 프로젝트를 만든 후에 **솔루션 탐색기**에서 **리소스** 폴더 및 **레이아웃** 폴더를 차례로 확장합니다.
**Main.axml**을 두 번 클릭하여 Android Designer에서 엽니다. 앱의 화면에 대한 레이아웃 파일입니다.

[![Main.axml 열기](hello-android-quickstart-images/vs/04-open-layout-sml.png)](hello-android-quickstart-images/vs/04-open-layout.png#lightbox)

**도구 상자**(왼쪽 영역)에서 검색 표시줄에 `text`을 입력하고,**큰 텍스트** 위젯을 디자인 화면(가운데 영역)으로 끌어옵니다.

[![큰 텍스트 위젯 추가](hello-android-quickstart-images/vs/04-large-text-sml.png)](hello-android-quickstart-images/vs/04-large-text.png#lightbox)

디자인 화면에서 선택한 **큰 텍스트** 컨트롤에서 **속성** 창을 사용하여 다음과 같이 **큰 텍스트** 위젯의 `text` 속성을 `Enter a Phoneword:`로 변경합니다.

[![큰 텍스트 속성 설정](hello-android-quickstart-images/vs/05-enter-a-phoneword-sml.png)](hello-android-quickstart-images/vs/05-enter-a-phoneword.png#lightbox)

**도구 상자**에서 **일반 텍스트** 위젯을 디자인 화면으로 끌어와서 **큰 텍스트** 위젯 아래에 놓습니다.

[![일반 텍스트 위젯 추가](hello-android-quickstart-images/vs/06-plain-text-sml.png)](hello-android-quickstart-images/vs/06-plain-text.png#lightbox)

디자인 화면에서 선택한 **일반 텍스트** 위젯에서 **속성** 창을 사용하여 **일반 텍스트** 위젯의 `id` 속성을 `@+id/PhoneNumberText`로 변경하고 `text` 속성을 `1-855-XAMARIN`로 변경합니다.

[![일반 텍스트 속성 설정](hello-android-quickstart-images/vs/07-add-properties-sml.png)](hello-android-quickstart-images/vs/07-add-properties.png#lightbox)

**도구 상자**에서 **단추**를 디자인 화면으로 끌어와서 **일반 텍스트** 위젯 아래에 놓습니다.

[![디자인에 변환 단추 끌어오기](hello-android-quickstart-images/vs/08-drag-button-sml.png)](hello-android-quickstart-images/vs/08-drag-button.png#lightbox)

디자인 화면에서 선택한 **단추**에서 **속성** 창을 사용하여 **단추**의 `id` 속성을 `@+id/TranslateButton`로 변경하고 `text` 속성을 `Translate`로 변경합니다.

[![변환 단추 속성 설정](hello-android-quickstart-images/vs/09-translate-button-sml.png)](hello-android-quickstart-images/vs/09-translate-button.png#lightbox)

**도구 상자**에서 **TextView**를 디자인 화면으로 끌어와서 **단추** 위젯 아래에 놓습니다. **TextView**의 `id` 속성을 `@+id/TranslatedPhoneWord`로 설정하고 `text`를 빈 문자열로 변경합니다.

[![텍스트 보기에서 속성 설정](hello-android-quickstart-images/vs/10-textview-properties-sml.png)](hello-android-quickstart-images/vs/10-textview-properties.png#lightbox)    

**CTRL+S** 키를 눌러 작업을 저장합니다.

### <a name="writing-translation-code"></a>변환 코드 작성

다음 단계는 전화 번호를 영숫자에서 숫자로 변환하는 코드를 추가하는 것입니다. **솔루션 탐색기**에서 **Phoneword** 프로젝트를 마우스 오른쪽 단추로 클릭하고, 아래에 표시된 대로 **추가 > 새 항목...**을 선택하여 프로젝트에 새 파일을 추가합니다.

[![새 항목 추가](hello-android-quickstart-images/vs/12-add-new-item-sml.png)](hello-android-quickstart-images/vs/12-add-new-item.png#lightbox)

**새 항목 추가** 대화 상자에서 **Visual C# > 코드**를 선택하고, 새 코드 파일에 **PhoneTranslator.cs**라는 이름을 지정합니다.

[![PhoneTranslator.cs 추가](hello-android-quickstart-images/vs/14-add-class-sml.png)](hello-android-quickstart-images/vs/14-add-class.png#lightbox)

그러면 비어 있는 새 C# 클래스가 만들어집니다. 다음 코드를 파일에 삽입합니다.

```csharp
using System.Text;
using System;
namespace Core
{
    public static class PhonewordTranslator
    {
        public static string ToNumber(string raw)
        {
            if (string.IsNullOrWhiteSpace(raw))
                return "";
            else
                raw = raw.ToUpperInvariant();

            var newNumber = new StringBuilder();
            foreach (var c in raw)
            {
                if (" -0123456789".Contains(c))
                {
                    newNumber.Append(c);
                }
                else
                {
                    var result = TranslateToNumber(c);
                    if (result != null)
                        newNumber.Append(result);
                }
                // otherwise we've skipped a non-numeric char
            }
            return newNumber.ToString();
        }
        static bool Contains (this string keyString, char c)
        {
            return keyString.IndexOf(c) >= 0;
        }
        static int? TranslateToNumber(char c)
        {
            if ("ABC".Contains(c))
                return 2;
            else if ("DEF".Contains(c))
                return 3;
            else if ("GHI".Contains(c))
                return 4;
            else if ("JKL".Contains(c))
                return 5;
            else if ("MNO".Contains(c))
                return 6;
            else if ("PQRS".Contains(c))
                return 7;
            else if ("TUV".Contains(c))
                return 8;
            else if ("WXYZ".Contains(c))
                return 9;
            return null;
        }
    }
}
```

**파일 > 저장**을 클릭(하거나 **CTRL+S** 키를 눌러서)하여 **PhoneTranslator.cs** 파일에 변경 내용을 저장한 다음, 파일을 닫습니다.

### <a name="wiring-up-the-interface"></a>인터페이스 연결

다음 단계는 `MainActivity` 클래스에 백업 코드를 삽입하여 사용자 인터페이스를 연결하는 코드를 추가하는 것입니다. **변환** 단추를 마무리합니다. `MainActivity` 클래스에서 `OnCreate` 메서드를 찾습니다. 다음 단계는 `base.OnCreate(bundle)` 및 `SetContentView
(Resource.Layout.Main)` 호출 아래의 `OnCreate` 내부에 단추 코드를 추가하는 것입니다. 먼저 `OnCreate` 메서드가 다음과 유사하도록 템플릿 코드를 수정합니다.

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Widget;
using Android.OS;

namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Set our view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // New code will go here
        }
    }
}
```

Android Designer를 통해 레이아웃 파일에서 생성된 컨트롤에 대한 참조를 가져옵니다. `SetContentView` 호출 후에 `OnCreate` 메서드 내부에 다음 코드를 추가합니다.

```csharp
// Get our UI controls from the loaded layout
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
```

사용자가 **변환** 단추를 누르면 응답하는 코드를 추가합니다.
(이전 단계에서 추가한 줄 뒤에)`OnCreate` 메서드에 다음 코드를 추가합니다.

```csharp
// Add code to translate number
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    string translatedNumber = Core.PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = string.Empty;
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
    }
};
```

**파일 > 모두 저장**을 선택(하거나 **CTRL-SHIFT-S** 키를 눌러서)하여 작업을 저장하고, **빌드 > 솔루션 다시 빌드**를 선택(하거나 **CTRL-SHIFT-B** 키를 눌러서)하여 응용 프로그램을 빌드합니다. 

오류가 있는 경우 이전 단계를 진행하고, 응용 프로그램이 성공적으로 빌드될 때까지 모든 오류를 수정합니다. _현재 컨텍스트에서 리소스가 존재하지 않습니다._와 같은 빌드 오류가 발생할 경우 **MainActivity.cs**의 네임스페이스 이름이 프로젝트 이름(`Phoneword`)과 일치하는지 확인한 다음, 솔루션을 완전히 다시 빌드합니다. 빌드 오류가 여전히 발생하는 경우 최신 Xamarin.Android 업데이트를 설치했는지 확인합니다.

### <a name="setting-the-label-and-app-icon"></a>레이블 및 앱 아이콘 설정

이제 응용 프로그램을 만들었습니다. &ndash; 마무리 작업을 추가하겠습니다. **MainActivity.cs**에서 `MainActivity`의 `Label`을 편집합니다. `Label`은 사용자가 응용 프로그램에서 현재 위치를 알 수 있도록 Android가 화면 맨 위에 표시하는 항목입니다.
`MainActivity` 클래스의 맨 위에서 다음과 같이 `Label`을 `Phone Word`로 변경합니다.

```csharp
namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        ...
    }
}
```

이제 응용 프로그램 아이콘을 설정하겠습니다. 기본적으로 Visual Studio는 프로젝트에 기본 아이콘을 제공합니다. 솔루션에서 이러한 파일을 삭제하고 다른 아이콘으로 바꿔보겠습니다. **Solution Pad**에서 **리소스** 폴더를 확장합니다. **mipmap-**을 접두사로 지정한 5개의 폴더가 있고 해당 폴더에는 각각 단일 **Icon.png** 파일이 포함됩니다.

[![mipmap- 폴더 및 Icon.png 파일](hello-android-quickstart-images/vs/21-mipmap-folders-sml.png)](hello-android-quickstart-images/vs/21-mipmap-folders.png#lightbox)

이러한 아이콘 파일을 각 프로젝트에서 삭제해야 합니다. 각 **Icon.png** 파일을 마우스 오른쪽 단추로 클릭하고 상황에 맞는 메뉴에서 **삭제**를 선택합니다.
   
[![기본 Icon.png 삭제](hello-android-quickstart-images/vs/21-delete-icon-sml.png)](hello-android-quickstart-images/vs/21-delete-icon.png#lightbox)
   
대화 상자에서 **삭제** 단추를 클릭합니다.

다음으로 [Xamarin 앱 아이콘 설정](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true)을 다운로드하고 압축을 풉니다. 이 zip 파일은 응용 프로그램에 대한 아이콘을 보관합니다. 각 아이콘은 시각적으로 거의 동일하지만 다양한 화면 밀도를 가진 다양한 장치에서 다른 해상도로 올바르게 렌더링합니다.  파일 집합을 Xamarin.Android 프로젝트에 복사해야 합니다. Visual Studio의 **솔루션 탐색기**에서 **mipmap-hdpi** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 > 기존 항목**을 선택합니다.

[![파일 추가](hello-android-quickstart-images/vs/22-add-files-sml.png)](hello-android-quickstart-images/vs/22-add-files.png#lightbox)

선택 영역 대화 상자에서 압축을 푼 Xamarin AdApp 아이콘 디렉터리로 이동하고 **mipmap-hdpi** 폴더를 엽니다. **Icon.png**를 선택하고 **추가**를 클릭합니다.

**Phoneword** 프로젝트에서 **mipmap-** Xamarin 앱 아이콘 폴더의 콘텐츠를 해당하는 **mipmap-** 폴더에 복사할 때까지 **mipmap-** 폴더의 각각에 대해 이러한 단계를 반복합니다.

모든 아이콘을 Xamarin.Android 프로젝트에 복사한 후에 **Solution Pad**에서 프로젝트를 마우스 오른쪽 단추로 클릭하여 **프로젝트 옵션** 대화 상자를 엽니다. **빌드 > Android 응용 프로그램**을 선택하고 **응용 프로그램 아이콘** 콤보 상자에서 **@mipmap/icon**을 선택합니다.

[![프로젝트 아이콘 설정](hello-android-quickstart-images/vs/25-set-project-icon-sml.png)](hello-android-quickstart-images/vs/25-set-project-icon.png#lightbox)

### <a name="running-the-app"></a>앱 실행

마지막으로 Android 장치 또는 에뮬레이터에서 응용 프로그램을 실행하고 Phoneword를 변환하여 테스트합니다.

[![완료 시 앱 스크린샷](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**응용 프로그램** 폴더 또는 **스포트라이트**에서 Mac용 Visual Studio를 시작합니다. 

**새 솔루션...**을 클릭하여 새 프로젝트를 만듭니다.

**새 프로젝트의 템플릿 선택** 대화 상자에서 **Android > 앱**을 클릭하고 **Android 앱** 템플릿을 선택합니다. **다음**을 클릭합니다.

[![Android 앱 템플릿 선택](hello-android-quickstart-images/xs/03-choose-template-sml.png)](hello-android-quickstart-images/xs/03-choose-template.png#lightbox)

**Android 앱 구성** 대화 상자에서 새 앱의 이름을 `Phoneword`으로 지정하고 **다음**을 클릭합니다.

[![Android 앱 구성](hello-android-quickstart-images/xs/04-configure-android-app-sml.png)](hello-android-quickstart-images/xs/04-configure-android-app.png#lightbox)

**새 Android 앱 구성** 대화 상자에서 솔루션 및 프로젝트 이름을 `Phoneword`로 설정해 두고 **만들기**를 클릭하여 프로젝트를 만듭니다.

### <a name="creating-the-layout"></a>레이아웃 만들기

새 프로젝트를 만든 후에 **솔루션** 패드에서 **리소스** 폴더 및 **레이아웃** 폴더를 차례로 확장합니다.
**Main.axml**을 두 번 클릭하여 Android Designer에서 엽니다. Android Designer에서 볼 때 화면에 대한 레이아웃 파일입니다.

[![Main.axml 열기](hello-android-quickstart-images/xs/05-open-layout-sml.png)](hello-android-quickstart-images/xs/05-open-layout.png#lightbox)

**Hello World, 클릭하세요.**를 선택합니다. 디자인 화면의 **단추** 및 **삭제** 키를 눌러서 제거합니다. 

**도구 상자**(오른쪽 영역)에서 검색 표시줄에 `text`을 입력하고,**큰 텍스트** 위젯을 디자인 화면(가운데 영역)으로 끌어옵니다.

[![큰 텍스트 위젯 추가](hello-android-quickstart-images/xs/06-large-text-sml.png)](hello-android-quickstart-images/xs/06-large-text.png#lightbox)

디자인 화면에서 선택한 **큰 텍스트** 위젯에서 **속성** 패드를 사용하여 다음과 같이 **큰 텍스트** 위젯의 `Text` 속성을 `Enter a Phoneword:`로 변경할 수 있습니다.

[![큰 텍스트 위젯 속성 설정](hello-android-quickstart-images/xs/07-enter-a-phoneword-sml.png)](hello-android-quickstart-images/xs/07-enter-a-phoneword.png#lightbox)

다음으로 **도구 상자**에서 **일반 텍스트** 위젯을 디자인 화면으로 끌어와서 **큰 텍스트** 위젯 아래에 놓습니다. 필드를 사용하여 위젯을 이름으로 찾을 수 있습니다.

[![일반 텍스트 위젯 추가](hello-android-quickstart-images/xs/08-plain-text-sml.png)](hello-android-quickstart-images/xs/08-plain-text.png#lightbox)

디자인 화면에서 선택한 **일반 텍스트** 위젯에서 **속성** 패드를 사용하여 **일반 텍스트** 위젯의 `Id` 속성을 `@+id/PhoneNumberText`로 변경하고 `Text` 속성을 `1-855-XAMARIN`로 변경할 수 있습니다.

[![일반 텍스트 위젯 속성 설정](hello-android-quickstart-images/xs/09-add-properties-sml.png)](hello-android-quickstart-images/xs/09-add-properties.png#lightbox)

**도구 상자**에서 **단추**를 디자인 화면으로 끌어와서 **일반 텍스트** 위젯 아래에 놓습니다.

[![단추 추가](hello-android-quickstart-images/xs/10-drag-button-sml.png)](hello-android-quickstart-images/xs/10-drag-button.png#lightbox)

디자인 화면에서 선택한 **단추**에서 **속성** 패드를 사용하여 **단추**의 `Id` 속성을 `@+id/TranslateButton`로 변경하고 `Text` 속성을 `Translate`로 변경할 수 있습니다.

[![변환 단추로 구성](hello-android-quickstart-images/xs/11-translate-button-sml.png)](hello-android-quickstart-images/xs/11-translate-button.png#lightbox)

**도구 상자**에서 **TextView**를 디자인 화면으로 끌어와서 **단추** 위젯 아래에 놓습니다. **TextView**를 선택하여 **TextView**의 `id` 속성을 `@+id/TranslatedPhoneWord`로 설정하고 `text`를 빈 문자열로 변경합니다.

[![텍스트 보기에서 속성 설정](hello-android-quickstart-images/xs/12-textview-properties-sml.png)](hello-android-quickstart-images/xs/12-textview-properties.png#lightbox)    

**&#8984; + S** 키를 눌러 작업을 저장합니다.

### <a name="writing-translation-code"></a>변환 코드 작성

이제 전화 번호를 영숫자에서 숫자로 변환하는 코드 추가합니다. **솔루션** 패드에서 **Phoneword** 프로젝트의 옆에 있는 기어 아이콘을 클릭하고 **추가 > 새 파일...**을 선택하하여 프로젝트에 새 파일을 추가합니다.

[![프로젝트에 새 파일 추가](hello-android-quickstart-images/xs/14-add-new-file-sml.png)](hello-android-quickstart-images/xs/14-add-new-file.png#lightbox)

**새 파일** 대화 상자에서 **General > Empty Class**를 선택하고, 새 파일에 **PhoneTranslator**라는 이름을 지정하고 **새로 만들기**를 클릭합니다. 이렇게 하면 비어 있는 새 C# 클래스가 만들어집니다.

새 클래스에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

```csharp
using System.Text;
using System;
namespace Core
{
    public static class PhonewordTranslator
    {
        public static string ToNumber(string raw)
        {
            if (string.IsNullOrWhiteSpace(raw))
                return "";
            else
                raw = raw.ToUpperInvariant();

            var newNumber = new StringBuilder();
            foreach (var c in raw)
            {
                if (" -0123456789".Contains(c))
                {
                    newNumber.Append(c);
                }
                else
                {
                    var result = TranslateToNumber(c);
                    if (result != null)
                        newNumber.Append(result);
                }
                // otherwise we've skipped a non-numeric char
            }
            return newNumber.ToString();
        }
        static bool Contains (this string keyString, char c)
        {
            return keyString.IndexOf(c) >= 0;
        }
        static int? TranslateToNumber(char c)
        {
            if ("ABC".Contains(c))
                return 2;
            else if ("DEF".Contains(c))
                return 3;
            else if ("GHI".Contains(c))
                return 4;
            else if ("JKL".Contains(c))
                return 5;
            else if ("MNO".Contains(c))
                return 6;
            else if ("PQRS".Contains(c))
                return 7;
            else if ("TUV".Contains(c))
                return 8;
            else if ("WXYZ".Contains(c))
                return 9;
            return null;
        }
    }
}
```

**파일 > 저장**을 선택(하거나 **&#8984; + S** 키를 눌러서)하여 **PhoneTranslator.cs** 파일에 변경 내용을 저장한 다음, 파일을 닫습니다. 솔루션을 다시 빌드하여 컴파일 시간 오류가 없는지 확인합니다.

### <a name="wiring-up-the-interface"></a>인터페이스 연결

다음 단계는 `MainActivity` 클래스에 백업 코드를 추가하여 사용자 인터페이스를 연결하는 코드를 추가하는 것입니다.
**Solution Pad**에서 **MainActivity.cs**를 두 번 클릭하여 엽니다.

이벤트 처리기를 **변환** 단추에 추가하여 시작합니다. `MainActivity` 클래스에서 `OnCreate` 메서드를 찾습니다. `base.OnCreate(bundle)` 및 `SetContentView (Resource.Layout.Main)` 호출 아래의 `OnCreate` 내부에 단추 코드를 추가합니다. `OnCreate` 메서드가 다음과 유사하도록 템플릿 단추 처리 코드를 제거합니다.

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;

namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Set our view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Our code will go here
        }
    }
}
```

다음으로 Android Designer를 사용하여 레이아웃 파일에서 생성된 컨트롤에 참조가 필요합니다. (`SetContentView` 호출 후에)`OnCreate` 메서드 내부에 다음 코드를 추가합니다.

```csharp
// Get our UI controls from the loaded layout
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
```

(마지막 단계에서 추가한 줄 뒤에) 다음 코드를 `OnCreate` 메서드에 추가하여 사용자가 **변환** 단추를 누르면 응답하는 코드를 추가합니다.

```csharp
// Add code to translate number
string translatedNumber = string.Empty;

translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = string.Empty;
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
    }
};
```

**빌드 > 모두 빌드**를 선택(하거나 **&#8984; + B** 키를 눌러)하여 작업 내용을 저장하고 응용 프로그램을 빌드합니다. 응용 프로그램을 컴파일하는 경우 Mac용 Visual Studio 맨 위에서 성공 메시지가 표시됩니다.

오류가 있는 경우 이전 단계를 진행하고, 응용 프로그램이 성공적으로 빌드될 때까지 모든 오류를 수정합니다. _현재 컨텍스트에서 리소스가 존재하지 않습니다._와 같은 빌드 오류가 발생할 경우 **MainActivity.cs**의 네임스페이스 이름이 프로젝트 이름(`Phoneword`)과 일치하는지 확인한 다음, 솔루션을 완전히 다시 빌드합니다. 빌드 오류가 여전히 발생하는 경우 최신 Xamarin.Android 및 Mac용 Visual Studio 업데이트를 설치했는지 확인합니다.

### <a name="setting-the-label-and-app-icon"></a>레이블 및 앱 아이콘 설정

이제 응용 프로그램을 만들었습니다. 마무리 작업을 추가하겠습니다. `MainActivity`에 대한 `Label`을 편집하여 시작합니다.
`Label`은 사용자가 응용 프로그램에서 현재 위치를 알 수 있도록 Android가 화면 맨 위에 표시하는 항목입니다. `MainActivity` 클래스의 맨 위에서 다음과 같이 `Label`을 `Phone Word`로 변경합니다.

```csharp
namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        ...
    }
}
```

이제 응용 프로그램 아이콘을 설정하겠습니다. 기본적으로 Mac용 Visual Studio는 프로젝트에 기본 아이콘을 제공합니다. 솔루션에서 이러한 파일을 삭제하고 다른 아이콘으로 바꿔보겠습니다. **Solution Pad**에서 **리소스** 폴더를 확장합니다. **mipmap-**을 접두사로 지정한 5개의 폴더가 있고 해당 폴더에는 각각 단일 **Icon.png** 파일이 포함됩니다.

[![mipmap- 폴더 및 Icon.png 파일](hello-android-quickstart-images/xs/23-mipmap-folders-sml.png)](hello-android-quickstart-images/xs/23-mipmap-folders.png#lightbox)

이러한 아이콘 파일을 각 프로젝트에서 삭제해야 합니다. 각 **Icon.png** 파일을 마우스 오른쪽 단추로 클릭하고 상황에 맞는 메뉴에서 **제거**를 선택합니다.

[![기본 Icon.png 삭제](hello-android-quickstart-images/xs/23-delete-icon-sml.png)](hello-android-quickstart-images/xs/23-delete-icon.png#lightbox)

대화 상자에서 **삭제** 단추를 클릭합니다.

다음으로 [Xamarin 앱 아이콘 설정](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true)을 다운로드하고 압축을 풉니다. 이 zip 파일은 응용 프로그램에 대한 아이콘을 보관합니다. 각 아이콘은 시각적으로 거의 동일하지만 다양한 화면 밀도를 가진 다양한 장치에서 다른 해상도로 올바르게 렌더링합니다.  파일 집합을 Xamarin.Android 프로젝트에 복사해야 합니다. Mac용 Visual Studio의 **Solution Pad**에서 **mipmap-hdpi** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 > 파일 추가**를 선택합니다.

[![파일 추가](hello-android-quickstart-images/xs/24-add-files-sml.png)](hello-android-quickstart-images/xs/24-add-files.png#lightbox)

선택 영역 대화 상자에서 압축을 푼 Xamarin AdApp 아이콘 디렉터리로 이동하고 **mipmap-hdpi** 폴더를 엽니다. **Icon.png**를 선택하고 **열기**를 클릭합니다.

**폴더에 파일 추가** 대화 상자에서 **디렉터리에 파일 복사**를 선택하고 **확인**을 클릭합니다.

[![디렉터리 대화 상자에 파일 복사](hello-android-quickstart-images/xs/26-copy-to-directory-sml.png)](hello-android-quickstart-images/xs/26-copy-to-directory.png#lightbox)

**Phoneword** 프로젝트에서 **mipmap-** Xamarin 앱 아이콘 폴더의 콘텐츠를 해당하는 **mipmap-** 폴더에 복사할 때까지 **mipmap-** 폴더의 각각에 대해 이러한 단계를 반복합니다.

모든 아이콘을 Xamarin.Android 프로젝트에 복사한 후에 **Solution Pad**에서 프로젝트를 마우스 오른쪽 단추로 클릭하여 **프로젝트 옵션** 대화 상자를 엽니다. **빌드 > Android 응용 프로그램**을 선택하고 **응용 프로그램 아이콘** 콤보 상자에서 **@mipmap/icon**을 선택합니다.

[![프로젝트 아이콘 설정](hello-android-quickstart-images/xs/28-set-project-icon-sml.png)](hello-android-quickstart-images/xs/28-set-project-icon.png#lightbox)

### <a name="running-the-app"></a>앱 실행

마지막으로 Android 장치 또는 에뮬레이터에서 응용 프로그램을 실행하고 Phoneword를 변환하여 테스트합니다.

[![완료 시 앱 스크린샷](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)

-----

첫 번째 Xamarin.Android 응용 프로그램을 완성한 것을 축하합니다!
이제 방금 알아본 도구와 기술을 분석하겠습니다. 다음 단계는 [Hello, Android 심층 분석](~/android/get-started/hello-android/hello-android-deepdive.md)입니다.


## <a name="related-links"></a>관련 링크

- [Xamarin Android 앱 아이콘(ZIP)](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true)
- [Phoneword(샘플)](https://developer.xamarin.com/samples/monodroid/Phoneword)
