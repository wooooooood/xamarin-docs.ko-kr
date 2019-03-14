---
title: 문자열 및 이미지 지역화
description: .NET 리소스 파일을 사용하여 Xamarin.Forms 앱을 지역화할 수 있습니다.
ms.prod: xamarin
ms.assetid: 852B4ED3-2D2D-48A5-A759-A6591F6A1509
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/06/2016
ms.openlocfilehash: 31992c7d9219289847ebc3e9c8af755d54dc18ab
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57672718"
---
# <a name="localization"></a>지역화

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UsingResxLocalization/)

_.NET 리소스 파일을 사용하여 Xamarin.Forms 앱을 지역화할 수 있습니다._

## <a name="overview"></a>개요

.NET 애플리케이션 지역화를 위해 기본 제공되는 메커니즘에서는 [RESX 파일](https://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx)과 `System.Resources` 및 `System.Globalization` 네임스페이스의 클래스를 사용합니다. 변환된 문자열이 포함된 RESX 파일이 변환에 대한 강력한 형식의 액세스 권한을 제공하는 컴파일러 생성 클래스와 함께 Xamarin.Forms 어셈블리에 포함됩니다. 그러면 변환된 텍스트를 코드에서 검색할 수 있습니다.

### <a name="sample-code"></a>샘플 코드

이 문서와 연결된 두 개의 샘플이 있습니다.

* [UsingResxLocalization](https://github.com/xamarin/xamarin-forms-samples/tree/master/UsingResxLocalization)은 설명된 개념의 매우 간단한 데모입니다. 아래에 표시된 코드 조각은 이 샘플의 모든 것입니다.
* [TodoLocalized](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized)은 이러한 지역화 기술을 사용하는 기본 작업 앱입니다.

#### <a name="shared-projects-are-not-recommended"></a>공유 프로젝트는 권장되지 않습니다.

빌드 시스템의 제한으로 인해 리소스 파일이 코드에서 강력한 형식의 변환된 문자열에 액세스할 수 없게 만드는 **.designer.cs** 파일을 생성하지 못한다 해도 TodoLocalized 샘플에는 [공유 프로젝트 데모](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized/SharedProject/)가 포함됩니다.

이 문서의 나머지 부분은 Xamarin.Forms .NET Standard 라이브러리 템플릿을 사용하는 프로젝트와 관련이 있습니다.

## <a name="globalizing-xamarinforms-code"></a>Xamarin.Forms 코드 글로벌화

애플리케이션의 **글로벌화**는 "세계를 준비"시키는 프로세스로서 다양한 언어를 표시할 수 있는 코드를 작성합니다.

애플리케이션 글로벌화의 주요 부분 중 하나는 *하드 코딩된* 텍스트가 없도록 사용자 인터페이스를 빌드하는 것입니다. 대신 사용자에게 표시된 모든 항목을 선택한 언어로 변환된 문자열 세트에서 검색해야 합니다.

이 문서에서는 RESX 파일을 사용하여 해당 문자열을 저장하고 사용자의 기본 설정에 따라 표시용 문자열을 검색하는 방법을 살펴봅니다.

샘플의 대상은 영어, 프랑스어, 스페인어, 독일어, 중국어, 일본어, 러시아어 및 브라질 포르투갈어입니다. 애플리케이션은 필요한 경우 적거나 많은 언어로 변환될 수 있습니다.

> [!NOTE]
> 유니버설 Windows 플랫폼에서 RESX 파일보다는 RESW 파일이 푸시 알림 지역화에 사용되어야 합니다. 자세한 내용은 [UWP 지역화](/windows/uwp/design/globalizing/globalizing-portal/)를 참조하세요.

### <a name="adding-resources"></a>리소스 추가

Xamarin.Forms. NET Standard 라이브러리 애플리케이션 글로벌화의 첫 번째 단계는 앱에서 사용하는 모든 텍스트를 저장하는 데 사용할 RESX 리소스 파일을 추가하는 것입니다. 기본 텍스트를 포함하는 RESX 파일을 추가한 다음, 지원하려는 각 언어에 대한 추가적인 RESX 파일을 추가해야 합니다.

#### <a name="base-language-resource"></a>기본 언어 리소스

기본 리소스(RESX) 파일에는 기본 언어 문자열이 포함됩니다(샘플에서는 영어를 기본 언어로 가정합니다). 프로젝트를 마우스 오른쪽 단추로 클릭하고 **추가 > 새 파일...** 을 선택하여 Xamarin.Forms 공용 코드 프로젝트에 파일을 추가합니다.

**AppResources**와 같은 의미 있는 이름을 선택하고 **확인**을 누릅니다.

[![리소스 파일 추가](text-images/resx-new-file-sml.png "새 파일 대화 상자")](text-images/resx-new-file.png#lightbox "새 파일 대화 상자")

프로젝트에 두 파일을 추가합니다.

* 변환 가능한 문자열이 XML 형식으로 저장되는 **AppResources.resx** 파일입니다.
* partial 클래스를 선언하여 RESX XML 파일에서 생성된 모든 요소에 대한 참조를 포함하는 **AppResources.designer.cs** 파일입니다.

솔루션 트리에서는 관련 파일을 보여줍니다. 새 변환 가능한 문자열을 추가하려면 RESX 파일을 편집*해야* 하며, **.designer.cs** 파일은 편집하지 *말아야* 합니다.

![](text-images/appresources-tree.png "AppResources.resx 파일")

##### <a name="string-visibility"></a>문자열 표시 유형

기본적으로 문자열에 대한 강력한 형식의 참조를 생성할 경우 어셈블리에 대한 `internal`이 됩니다. 이는 RESX 파일에 대한 기본 빌드 도구에서 `internal` 속성을 사용하여 **.designer.cs** 파일을 생성하기 때문입니다.

이 빌드 도구가 구성되는 위치를 확인하려면 **AppResources.resx** 파일을 선택하고 **속성** 패드를 표시합니다. 아래 스크린샷에서는 **사용자 지정 도구: ResXFileCodeGenerator**를 보여 줍니다.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](text-images/vs-resx-internal-sml.png "AppResources.Resx의 속성 창")](text-images/vs-resx-internal.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](text-images/xs-resx-internal-sml.png "AppResources.Resx의 속성 패드")](text-images/xs-resx-internal.png#lightbox)

-----

강력한 형식의 문자열 속성을 `public`으로 만들려면 아래 스크린샷에 표시된 것처럼 수동으로 구성을 **사용자 지정 도구: PublicResXFileCodeGenerator**로 변경해야 합니다.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](text-images/vs-resx-public-sml.png "AppResources.Resx의 속성 창")](text-images/vs-resx-public.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](text-images/xs-resx-internal-sml.png "AppResources.Resx의 속성 패드")](text-images/xs-resx-internal.png#lightbox)


[![](text-images/xs-resx-public-sml.png "AppResources.Resx의 속성 패드")](text-images/xs-resx-public.png#lightbox)

-----

이 변경은 선택 사항이며, 다양한 어셈블리에서 지역화된 문자열을 참조하려는 경우(예: 다양한 어셈블리의 RESX 파일을 코드에 저장하는 경우)에만 필요합니다. 이 항목에 대한 샘플은 `internal` 문자열이 사용되는 경우 동일한 Xamarin.Forms. NET Standard 라이브러리 어셈블리에서 정의되므로 해당 문자열을 유지합니다.

위에 표시된 것과 같이 기본 RESX 파일에서 사용자 지정 도구만 설정해야 합니다. 다음 섹션에 설명된 언어별 RESX 파일에서 *모든* 빌드 도구를 설정할 필요는 없습니다.

##### <a name="editing-the-resx-file"></a>RESX 파일 편집

안타깝게도 Mac용 Visual Studio에는 기본 제공 RESX 편집기가 없습니다. 변환 가능한 문자열을 추가하면 각 문자열에 대한 새 XML `data` 요소의 추가가 필요합니다. 각 `data` 요소에는 다음이 포함될 수 있습니다.

* `name` 특성(필수)는 이 변환 가능한 문자열의 핵심입니다. 공백이나 특수 문자가 허용되지 않도록 올바른 C# 속성 이름이어야 합니다.
* 실제 문자열인 `value` 요소(필수)는 애플리케이션에 표시됩니다.
* `comment` 요소(선택 사항)에는 문자열을 사용하는 방법을 설명하는 변환기에 대한 지침이 포함될 수 있습니다.
* 문자열의 공백을 유지하는 방법을 제어하기 위한 `xml:space` 특성(선택 사항)입니다.

몇 가지 예제 `data` 요소가 여기에 표시됩니다.

```xml
<data name="NotesLabel" xml:space="preserve">
    <value>Notes:</value>
    <comment>label for input field</comment>
</data>
<data name="NotesPlaceholder" xml:space="preserve">
    <value>eg. buy milk</value>
    <comment>example input for notes field</comment>
</data>
<data name="AddButton" xml:space="preserve">
    <value>Add new item</value>
</data>
```

애플리케이션이 작성되므로 사용자에 게 표시되는 텍스트의 모든 부분을 새 `data` 요소의 기본 RESX 리소스 파일에 추가해야 합니다. 변환의 고품질을 확보하려면 가급적 `comment`가 포함되는 것이 좋습니다.

> [!NOTE]
> Visual Studio(평가판 Community 버전을 포함하여)에는 기본 RESX 편집기가 포함됩니다. Windows 컴퓨터에 대한 액세스 권한이 있는 경우 RESX 파일에 문자열을 추가 및 편집하는 것이 간편한 방법일 수 있습니다.

#### <a name="language-specific-resources"></a>언어별 리소스

일반적으로 기본 텍스트 문자열은 애플리케이션의 대규모 청크가 작성되기까지(기본 RESX 파일에 많은 문자열이 포함된 경우) 실제로 변환되지 않습니다. 지역화 코드를 테스트할 수 있으려면 기계로 변환된 텍스트로 채우면서 개발 주기 초반에 언어별 리소스를 추가하는 것이 좋습니다.

지원하려는 각 언어에 대한 추가적인 RESX 파일 하나를 추가합니다.
언어별 리소스 파일은 특정 명명 규칙을 따라야 합니다. 즉, 기본 리소스 파일과 동일한 파일 이름을 사용하고(예: **AppResources**). 간단한 예제는 다음과 같습니다.

* **AppResources.fr.resx** - 프랑스어 번역입니다.
* **AppResources.es.resx** - 스페인어 번역입니다.
* **AppResources.de.resx** - 독일어 번역입니다.
* **AppResources.ja.resx** - 일본어 번역입니다.
* **AppResources.zh-Hans.resx** - 중국어(간체) 번역입니다.
* **AppResources.zh-Hant.resx** - 중국어(번체) 번역입니다.
* **AppResources.pt.resx** - 포르투갈어 번역입니다.
* **AppResources.pt-BR.resx** - 브라질 포르투갈어 번역입니다.

일반적인 패턴은 2글자 언어 코드를 사용하는 것이지만 다른 형식을 사용하는 일부 예제(예: 중국어) 및 4글자 로캘 식별자가 필요한 다른 예제(예: 브라질 포르투갈어)도 있습니다.

이러한 언어별 리소스 파일은 **.designer.cs** partial 클래스가 필요’하지 않기’ 때문에 해당 리소스 파일을 **빌드 작업: EmbeddedResource**이 설정된 일반 XML 파일로 추가할 수 있습니다. 이 스크린샷은 언어별 리소스 파일이 포함된 솔루션을 보여줍니다.

![](text-images/appresources-langs.png "언어별 리소스 파일")

애플리케이션을 개발하고 기본 RESX 파일이 텍스트를 추가했으므로 각 `data` 요소를 변환하고 앱에 포함되도록 언어별 리소스 파일을 반환할(표시된 명명 규칙을 사용하여) 변환기에 텍스트를 전송해야 합니다. 일부 '기계 번역' 예제는 다음과 같습니다.

**AppResources.es.resx(스페인어)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>Escribir un artículo</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

**AppResources.ja.resx(일본어)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>新しい項目を追加</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

**AppResources.pt-BR.resx(브라질 포르투갈어)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>adicionar novo item</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

`value` 요소만 변환기에서 업데이트되어야 합니다. `comment`는 변환되지 않습니다. 유의: `&lt;`, `&gt;` 및 `&amp;`가 포함된 `<`, `>`, `&`과 같이 예약된 문자를 이스케이프하기 위해 XML 파일을 편집하는 경우 및 `value` 또는 해당 문자가 `comment`에 표시되는 경우입니다.

<a name="incode" />

### <a name="using-resources-in-code"></a>코드의 리소스 사용

RESX 리소스 파일의 문자열은 `AppResources` 클래스를 사용하여 사용자 인터페이스 코드에서 사용할 수 있습니다. RESX 파일에서 각 문자열에 할당된 `name`은 아래에 표시된 대로 Xamarin.Forms 코드에서 참조할 수 있는 해당 클래스의 속성이 됩니다.

```csharp
var myLabel = new Label ();
var myEntry = new Entry ();
var myButton = new Button ();
// populate UI with translated text values from resources
myLabel.Text = AppResources.NotesLabel;
myEntry.Placeholder = AppResources.NotesPlaceholder;
myButton.Text = AppResources.AddButton;
```

텍스트가 하드 코딩되기보다는 리소스에서 로드되기 때문에 앱을 여러 언어로 변환할 수 있는 것을 제외하고 iOS, Android 및 UWP(유니버설 Windows 플랫폼)의 사용자 인터페이스가 예상대로 렌더링됩니다. 변환에 앞서 각 플랫폼에서 UI를 보여주는 스크린샷은 다음과 같습니다.

![](text-images/simple-example-english.png "변환 전의 플랫폼 간 UI")

### <a name="troubleshooting"></a>문제 해결

#### <a name="testing-a-specific-language"></a>특정 언어 테스트

다른 문화권을 신속하게 테스트하려는 경우 특히 개발 중에 다른 언어로 시뮬레이터 또는 디바이스를 전환하는 것은 까다로울 수 있습니다.

이 코드 조각에 표시된 것처럼 `Culture`를 설정하여 특정 언어를 강제로 로드할 수 있습니다.

```csharp
// force a specific culture, useful for quick testing
AppResources.Culture =  new CultureInfo("fr-FR");
```

이 방법(`AppResources` 클래스에서 직접 문화권 설정)은 앱 내부에서(디바이스의 로캘을 사용하지 않고) 언어 선택기를 구현하는 데 사용될 수 있습니다.

#### <a name="loading-embedded-resources"></a>포함 리소스 로드

다음 코드 조각은 포함된 리소스(예: RESX 파일)를 사용하여 문제를 디버깅 하려 할 때 유용합니다. 앱(앱 수명 주기 초반)에 이 코드를 추가하면 전체 리소스 식별자가 표시되며 어셈블리에 포함된 모든 리소스가 나열됩니다.

```csharp
using System.Reflection;
// ...
// NOTE: use for debugging, not in released app code!
var assembly = typeof(EmbeddedImages).GetTypeInfo().Assembly; // "EmbeddedImages" should be a class in your app
foreach (var res in assembly.GetManifestResourceNames())
{
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

**AppResources.Designer.cs** 파일(약 *33번 줄*)에서 전체 *리소스 관리자 이름*이 아래 코드와 비슷하게 지정됩니다(예: `"UsingResxLocalization.Resx.AppResources"`).

```csharp
System.Resources.ResourceManager temp =
        new System.Resources.ResourceManager(
                "UsingResxLocalization.Resx.AppResources",
                typeof(AppResources).GetTypeInfo().Assembly);
```

위에 표시된 디버그 코드의 결과에 대한 **애플리케이션 출력**을 검사하여 리소스가 올바로 나열됐는지 확인합니다(예: `"UsingResxLocalization.Resx.AppResources"`).

그렇지 않은 경우, `AppResources` 클래스는 해당 리소스를 로드할 수 없습니다.
리소스를 찾을 수 없는 문제를 해결하려면 다음을 확인합니다.

* 프로젝트에 대한 기본 네임스페이스는 **AppResources.Designer.cs** 파일의 루트 네임스페이스와 일치합니다.
* **AppResources.resx** 파일이 하위 디렉터리에 있는 경우 하위 디렉터리 이름은 네임스페이스 *및* 리소스 식별자에 속해야 합니다.
* **AppResources.resx** 파일에는 **빌드 작업: EmbeddedResource**가 있습니다.
* **프로젝트 옵션 > 소스 코드 > .NET 명명 정책 > Visual Studio 스타일 리소스 이름 사용**을 선택합니다. 그러나 앱 전체에서 업데이트되어야 할 RESX 리소스를 참조할 때 사용된 네임스페이스를 선호하는 경우 선택을 취소할 수 있습니다.

#### <a name="doesnt-work-in-debug-mode-android-only"></a>DEBUG 모드(Android 전용)에서 작동하지 않습니다.

변환된 문자열이 Android가 빌드한 RELEASE에서는 작동하지만 디버깅 중에는 작동하지 않는 경우 **Android 프로젝트**를 마우스 오른쪽 단추로 클릭하고 **옵션 > 빌드 > Android 빌드**를 선택하고 **빠른 어셈블리 배포**가 선택되지 않도록 해야 합니다. 이 옵션은 리소스를 로드할 때 문제가 발생하므로 지역화된 애플리케이션을 테스트하는 경우 사용해서는 안 됩니다.

### <a name="displaying-the-correct-language"></a>올바른 언어 표시

지금까지 실제로 번역이 나타나게 하는 방법이 아닌, 번역을 제공할 수 있도록 코드를 작성하는 방법을 살펴봤습니다. Xamarin.Forms 코드는 .NET의 리소스를 활용하여 올바른 언어 번역을 로드할 수 있습니다. 그러나 사용자가 선택한 언어를 확인하려면 각 플랫폼에서 운영 체제를 쿼리해야 합니다.

일부 플랫폼별 코드는 사용자의 언어 기본 설정을 가져와야 하므로 [종속성 서비스](~/xamarin-forms/app-fundamentals/dependency-service/index.md)를 사용하여 해당 정보를 Xamarin.Forms 앱에서 노출하고 각 플랫폼에서 구현합니다.

먼저 아래 코드와 비슷한 사용자의 기본 설정된 문화권을 노출하려면 인터페이스를 정의합니다.

```csharp
public interface ILocalize
{
    CultureInfo GetCurrentCultureInfo ();
    void SetLocale (CultureInfo ci);
}
```

다음으로 Xamarin.Forms `App` 클래스에서 [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)를 사용하여 인터페이스를 호출하고 RESX 리소스 문화권을 올바른 값으로 설정합니다. 리소스 프레임워크가 해당 플랫폼에서 선택한 언어를 자동으로 인식하므로 유니버설 Windows 플랫폼의 값을 수동으로 설정할 필요가 없습니다.

```csharp
if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
{
    var ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
    Resx.AppResources.Culture = ci; // set the RESX for resource localization
    DependencyService.Get<ILocalize>().SetLocale(ci); // set the Thread for locale-aware methods
}
```

`Culture` 리소스는 올바른 언어 문자열을 사용하도록 애플리케이션이 처음 로드될 때 설정해야 합니다. 사용자가 앱이 실행되는 동안 해당 언어 기본 설정을 업데이트하는 경우 iOS 또는 Android에서 발생하는 플랫폼별 이벤트에 따라 이 값을 선택적으로 업데이트할 수 있습니다.

`ILocalize` 인터페이스에 대한 구현이 아래의 [플랫폼별 코드](#Platform-specific_Code) 섹션에 표시됩니다. 이러한 구현은 이 `PlatformCulture` 도우미 클래스를 활용합니다.

```csharp
public class PlatformCulture
{
    public PlatformCulture (string platformCultureString)
    {
        if (String.IsNullOrEmpty(platformCultureString))
        {
            throw new ArgumentException("Expected culture identifier", "platformCultureString"); // in C# 6 use nameof(platformCultureString)
        }
        PlatformString = platformCultureString.Replace("_", "-"); // .NET expects dash, not underscore
        var dashIndex = PlatformString.IndexOf("-", StringComparison.Ordinal);
        if (dashIndex > 0)
        {
            var parts = PlatformString.Split('-');
            LanguageCode = parts[0];
            LocaleCode = parts[1];
        }
        else
        {
            LanguageCode = PlatformString;
            LocaleCode = "";
        }
    }
    public string PlatformString { get; private set; }
    public string LanguageCode { get; private set; }
    public string LocaleCode { get; private set; }
    public override string ToString()
    {
        return PlatformString;
    }
}
```

<a name="Platform-specific_Code" />

### <a name="platform-specific-code"></a>플랫폼별 코드

iOS, Android 및 UWP 모두가 조금 다르게 이 정보를 노출하므로 표시할 언어를 검색하는 코드는 플랫폼 특정적이어야 합니다. `ILocalize` 종속성 서비스에 대한 코드는 추가 플랫폼별 요구 사항과 함께 플랫폼에 대해 다음과 같이 제공되어 지역화된 텍스트가 올바로 렌더링되었는지 확인합니다.

플랫폼별 코드는 사용자가 .NET의 `CultureInfo` 클래스에서 지원되지 않는 로캘 식별자를 구성하도록 운영 체제에서 허용하는 사례도 처리해야 합니다. 이러한 경우 사용자 지정 코드가 작성되어 지원되지 않는 로캘을 검색하고 최상의 .NET 호환 가능한 로캘을 대체해야 합니다.

#### <a name="ios-application-project"></a>iOS 애플리케이션 프로젝트

iOS 사용자는 날짜 및 시간 형식의 문화권에서 개별적으로 자신의 기본 설정 언어를 선택합니다. Xamarin.Forms 앱을 지역화하는 올바른 리소스를 로드하려면 첫 번째 요소에 대한 `NSLocale.PreferredLanguages` 배열을 쿼리해야 합니다.

`ILocalize` 종속성 서비스의 다음 구현을 iOS 애플리케이션 프로젝트에 배치해야 합니다. iOS는 대시(.NET 표준 표현) 대신 밑줄을 사용하므로 코드는 `CultureInfo` 클래스를 인스턴스화하기 전에 해당 밑줄을 교체합니다.

```csharp
[assembly:Dependency(typeof(UsingResxLocalization.iOS.Localize))]

namespace UsingResxLocalization.iOS
{
    public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale (CultureInfo ci)
        {
            Thread.CurrentThread.CurrentCulture = ci;
            Thread.CurrentThread.CurrentUICulture = ci;
        }

        public CultureInfo GetCurrentCultureInfo ()
        {
            var netLanguage = "en";
            if (NSLocale.PreferredLanguages.Length > 0)
            {
                var pref = NSLocale.PreferredLanguages [0];
                netLanguage = iOSToDotnetLanguage(pref);
            }
            // this gets called a lot - try/catch can be expensive so consider caching or something
            System.Globalization.CultureInfo ci = null;
            try
            {
                ci = new System.Globalization.CultureInfo(netLanguage);
            }
            catch (CultureNotFoundException e1)
            {
                // iOS locale not valid .NET culture (eg. "en-ES" : English in Spain)
                // fallback to first characters, in this case "en"
                try
                {
                    var fallback = ToDotnetFallbackLanguage(new PlatformCulture(netLanguage));
                    ci = new System.Globalization.CultureInfo(fallback);
                }
                catch (CultureNotFoundException e2)
                {
                    // iOS language not valid .NET culture, falling back to English
                    ci = new System.Globalization.CultureInfo("en");
                }
            }
            return ci;
        }

        string iOSToDotnetLanguage(string iOSLanguage)
        {
            // .NET cultures don't support underscores
            string netLanguage = iOSLanguage.Replace("_", "-");

            //certain languages need to be converted to CultureInfo equivalent
            switch (iOSLanguage)
            {
                case "ms-MY":   // "Malaysian (Malaysia)" not supported .NET culture
                case "ms-SG":    // "Malaysian (Singapore)" not supported .NET culture
                    netLanguage = "ms"; // closest supported
                    break;
                case "gsw-CH":  // "Schwiizertüütsch (Swiss German)" not supported .NET culture
                    netLanguage = "de-CH"; // closest supported
                    break;
                // add more application-specific cases here (if required)
                // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }

        string ToDotnetFallbackLanguage (PlatformCulture platCulture)
        {
            var netLanguage = platCulture.LanguageCode; // use the first part of the identifier (two chars, usually);
            switch (platCulture.LanguageCode)
            {
                case "pt":
                    netLanguage = "pt-PT"; // fallback to Portuguese (Portugal)
                    break;
                case "gsw":
                    netLanguage = "de-CH"; // equivalent to German (Switzerland) for this app
                    break;
                // add more application-specific cases here (if required)
                // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
    }
}
```

> [!NOTE]
> `GetCurrentCultureInfo` 메서드의 `try/catch` 블록은 일반적으로 로캘 식별자에서 사용된 대체(fallback) 동작을 모방하며(정확한 일치가 발견되지 않는 경우), 언어를 기반으로 근접한 일치(로캘에서 첫 번째 블록의 문자)를 찾습니다.
>
> Xamarin.Forms의 경우 일부 로캘은 iOS에서 유효하지만 .NET의 유효한 `CultureInfo` 값과 일치하지 않으며 위의 코드는 이 값을 처리하려고 합니다.
>
> 예를 들어 iOS **설정 > 일반 언어 &amp; 지역** 화면을 선택하면 사용자는 휴대폰 **언어**는 **영어**로, **지역**은 **스페인**으로 설정할 수 있습니다. 그 결과로 `"en-ES"`의 로캘 문자열이 됩니다. `CultureInfo` 생성이 실패하는 경우 표시 언어를 선택하려면 코드는 처음 두 글자만 사용하는 것으로 대체할 수 있습니다.
>
> 개발자는 지원되는 해당 언어에 필요한 특정 사례를 처리하려면 `iOSToDotnetLanguage` 및 `ToDotnetFallbackLanguage` 메서드를 수정해야 합니다.

`Picker` 컨트롤의 **완료** 단추와 같이 iOS에서 자동으로 변환되는 일부 시스템에서 정의된 사용자 인터페이스 요소가 있습니다. iOS에서 이러한 요소를 강제로 변환하려면 **Info.plist** 파일에서 지원하는 언어를 표시해야 합니다. 여기에 표시된 것처럼 **Info.plist > 소스**를 통해 이러한 값을 추가할 수 있습니다.

![Info.plist의 지역화 키](text-images/info-plist.png "Info.plist의 지역화 키")

또는 XML 편집기에서 **Info.plist** 파일을 열고 값을 직접 편집합니다.

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>de</string>
    <string>es</string>
    <string>fr</string>
    <string>ja</string>
    <string>pt</string> <!-- Brazil -->
    <string>pt-PT</string> <!-- Portugal -->
    <string>ru</string>
    <string>zh-Hans</string>
    <string>zh-Hant</string>
</array>
<key>CFBundleDevelopmentRegion</key>
<string>en</string>
```

종속성 서비스를 구현하고 **Info.plist**를 업데이트하면 iOS 앱은 지역화된 텍스트를 표시할 수 있습니다.

> [!NOTE]
> Apple에서는 기대했던 것과 약간 다르게 포르투갈어를 처리합니다.
> [해당 docs](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW2): _"브라질에서 사용되는 포르투갈어에 대한 언어 ID로 pt를 사용하고 포르투갈에서 사용되는 포르투갈어에 대한 언어 ID로 pt-PT를 사용합니다"_.
> 즉 포르투갈어가 비표준 로캘에서 선택되는 경우 iOS에서 대체 언어는 코드가 이 동작(예: 위의 `ToDotnetFallbackLanguage`)을 변경하도록 작성되지 않는 경우 브라질 포르투갈어입니다.

iOS 지역화에 대한 자세한 내용은 [iOS 지역화](~/ios/app-fundamentals/localization/index.md)를 참조하세요.

#### <a name="android-application-project"></a>Android 애플리케이션 프로젝트

Android는 `Java.Util.Locale.Default`를 통해 현재 선택된 로캘을 노출하고 다음 코드로 교체되는 대시 대신 밑줄 구분 기호를 사용합니다. Android 애플리케이션 프로젝트에 이 종속성 서비스 구현을 추가합니다.

```csharp
[assembly:Dependency(typeof(UsingResxLocalization.Android.Localize))]

namespace UsingResxLocalization.Android
{
    public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale(CultureInfo ci)
        {
            Thread.CurrentThread.CurrentCulture = ci;
            Thread.CurrentThread.CurrentUICulture = ci;
        }
        public CultureInfo GetCurrentCultureInfo()
        {
            var netLanguage = "en";
            var androidLocale = Java.Util.Locale.Default;
            netLanguage = AndroidToDotnetLanguage(androidLocale.ToString().Replace("_", "-"));
            // this gets called a lot - try/catch can be expensive so consider caching or something
            System.Globalization.CultureInfo ci = null;
            try
            {
                ci = new System.Globalization.CultureInfo(netLanguage);
            }
            catch (CultureNotFoundException e1)
            {
                // iOS locale not valid .NET culture (eg. "en-ES" : English in Spain)
                // fallback to first characters, in this case "en"
                try
                {
                    var fallback = ToDotnetFallbackLanguage(new PlatformCulture(netLanguage));
                    ci = new System.Globalization.CultureInfo(fallback);
                }
                catch (CultureNotFoundException e2)
                {
                    // iOS language not valid .NET culture, falling back to English
                    ci = new System.Globalization.CultureInfo("en");
                }
            }
            return ci;
        }
        string AndroidToDotnetLanguage(string androidLanguage)
        {
            var netLanguage = androidLanguage;
            //certain languages need to be converted to CultureInfo equivalent
            switch (androidLanguage)
            {
                case "ms-BN":   // "Malaysian (Brunei)" not supported .NET culture
                case "ms-MY":   // "Malaysian (Malaysia)" not supported .NET culture
                case "ms-SG":   // "Malaysian (Singapore)" not supported .NET culture
                    netLanguage = "ms"; // closest supported
                    break;
                case "in-ID":  // "Indonesian (Indonesia)" has different code in  .NET
                    netLanguage = "id-ID"; // correct code for .NET
                    break;
                case "gsw-CH":  // "Schwiizertüütsch (Swiss German)" not supported .NET culture
                    netLanguage = "de-CH"; // closest supported
                    break;
                    // add more application-specific cases here (if required)
                    // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
        string ToDotnetFallbackLanguage(PlatformCulture platCulture)
        {
            var netLanguage = platCulture.LanguageCode; // use the first part of the identifier (two chars, usually);
            switch (platCulture.LanguageCode)
            {
                case "gsw":
                    netLanguage = "de-CH"; // equivalent to German (Switzerland) for this app
                    break;
                    // add more application-specific cases here (if required)
                    // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
    }
}
```

> [!NOTE]
> `GetCurrentCultureInfo` 메서드의 `try/catch` 블록은 일반적으로 로캘 식별자에서 사용된 대체(fallback) 동작을 모방하며(정확한 일치가 발견되지 않는 경우), 언어를 기반으로 근접한 일치(로캘에서 첫 번째 블록의 문자)를 찾습니다.
>
> Xamarin.Forms의 경우 일부 로캘은 Android에서 유효하지만 .NET의 유효한 `CultureInfo` 값과 일치하지 않으며 위의 코드는 이 값을 처리하려고 합니다.
>
> 개발자는 지원되는 해당 언어에 필요한 특정 사례를 처리하려면 `iOSToDotnetLanguage` 및 `ToDotnetFallbackLanguage` 메서드를 수정해야 합니다.

이 코드가 Android 애플리케이션 프로젝트에 추가되면 변환된 문자열이 자동으로 표시됩니다.

> [!NOTE]
>️**경고:** 변환된 문자열이 Android가 빌드한 RELEASE에서는 작동하지만 디버깅 중에는 작동하지 않는 경우 **Android 프로젝트**를 마우스 오른쪽 단추로 클릭하고 **옵션 > 빌드 > Android 빌드**를 선택하고 **빠른 어셈블리 배포**가 선택되지 않도록 해야 합니다. 이 옵션은 리소스를 로드할 때 문제가 발생하므로 지역화된 애플리케이션을 테스트하는 경우 사용해서는 안 됩니다.

Android 지역화에 대한 자세한 내용은 [Android 지역화](~/android/app-fundamentals/localization.md)를 참조하세요.

#### <a name="universal-windows-platform"></a>UWP

UWP(유니버설 Windows 플랫폼) 프로젝트에는 종속 서비스가 필요하지 않습니다. 대신 이 플랫폼에서 자동으로 리소스의 문화권을 올바르게 설정합니다.

##### <a name="assemblyinfocs"></a>AssemblyInfo.cs

.NET Standard 라이브러리 프로젝트에서 속성 노드를 확장하고 **AssemblyInfo.cs** 파일을 두 번 클릭합니다. 중립 리소스 어셈블리 언어를 영어로 설정하려면 파일에 다음 줄을 추가합니다.

```csharp
[assembly: NeutralResourcesLanguage("en")]
```

이러면 앱의 기본 문화권에 대해 리소스 관리자에게 알리므로, 하나의 영어 로캘에서 앱을 실행하는 경우 언어 중립 RESX 파일(**AppResources.resx**)에서 정의된 문자열이 표시되는지 확인합니다.

### <a name="example"></a>예제

위에 표시된 대로 플랫폼별 프로젝트를 업데이트하고 변환된 RESX 파일을 사용하여 앱을 다시 컴파일한 후 각 앱에서 업데이트된 변환을 사용할 수 있습니다. 중국어(간체)로 번역된 샘플 코드의 스크린샷은 다음과 같습니다.

![](text-images/simple-example-hans.png "중국어(간체)로 번역된 플랫폼 간 UI")

UWP 지역화에 대한 자세한 내용은 [UWP 지역화](/windows/uwp/design/globalizing/globalizing-portal/)를 참조하세요.

## <a name="localizing-xaml"></a>XAML 지역화

XAML에서 Xamarin.Forms 사용자 인터페이스를 빌드하는 경우 태그는 XML에 문자열이 직접 포함된 이 인터페이스와 비슷해 보입니다.

```xaml
<Label Text="Notes:" />
<Entry Placeholder="eg. buy milk" />
<Button Text="Add to list" />
```

이상적으로 XAML에서 직접 사용자 인터페이스 컨트롤을 변환할 수 있으며, *태그 확장*을 만들어 수행할 수 있습니다. XAML에 RESX 리소스를 노출하는 태그 확장에 대한 코드는 다음과 같습니다. 이 클래스는 Xamarin.Forms 공용 코드 (XAML 페이지와 함께)에 추가해야 합니다.

```csharp
using System;
using System.Globalization;
using System.Reflection;
using System.Resources;
using Xamarin.Forms;
using Xamarin.Forms.Xaml;

namespace UsingResxLocalization
{
    // You exclude the 'Extension' suffix when using in XAML
    [ContentProperty("Text")]
    public class TranslateExtension : IMarkupExtension
    {
        readonly CultureInfo ci = null;
        const string ResourceId = "UsingResxLocalization.Resx.AppResources";

        static readonly Lazy<ResourceManager> ResMgr = new Lazy<ResourceManager>(
            () => new ResourceManager(ResourceId, IntrospectionExtensions.GetTypeInfo(typeof(TranslateExtension)).Assembly));

        public string Text { get; set; }

        public TranslateExtension()
        {
            if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
            {
                ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
            }
        }

        public object ProvideValue(IServiceProvider serviceProvider)
        {
            if (Text == null)
                return string.Empty;

            var translation = ResMgr.Value.GetString(Text, ci);
            if (translation == null)
            {
#if DEBUG
                throw new ArgumentException(
                    string.Format("Key '{0}' was not found in resources '{1}' for culture '{2}'.", Text, ResourceId, ci.Name),
                    "Text");
#else
                translation = Text; // HACK: returns the key, which GETS DISPLAYED TO THE USER
#endif
            }
            return translation;
        }
    }
}
```

다음 글머리 기호는 위의 코드의 중요한 요소를 설명합니다.

* 클래스 이름은 `TranslateExtension`이라고 할 수 있지만 참조할 수 있는 규칙에 따라 태그의 **변환**으로 명명됩니다.
* 클래스는 작동하려면 Xamarin.Forms에서 필요한 `IMarkupExtension`을 구현합니다.
* `"UsingResxLocalization.Resx.AppResources"`는 RESX 리소스에 대한 리소스 식별자입니다. 이 식별자는 기본 네임스페이스, 리소스 파일이 있는 폴더 및 기본 RESX 파일로 구성되어 있습니다.
* 리소스를 로드하고 정적 `ResMgr` 필드에서 캐시될 현재 어셈블리를 확인하려면 `IntrospectionExtensions.GetTypeInfo(typeof(TranslateExtension)).Assembly)`를 사용하여 `ResourceManager` 클래스를 만듭니다. `ProvideValue` 메서드에서 처음 사용될 때까지 해당 생성을 지연하도록 해당 클래스를 `Lazy` 형식으로 만듭니다.
* `ci`는 종속성 서비스를 사용하여 기본 운영 체제에서 사용자가 선택한 언어를 가져옵니다.
* `GetString`은 리소스 파일에서 실제 변환된 문자열을 검색하는 메서드입니다. `ILocalize` 인터페이스가 해당 플랫폼에서 구현되지 않으므로 유니버설 Windows 플랫폼에서 `ci`는 null입니다. 이 방법은 첫 번째 매개 변수만 사용하여 `GetString` 메서드를 호출하는 것과 동일합니다. 대신 리소스 프레임 워크는 로캘을 자동으로 인식하고 적절한 RESX 파일에서 변환된 문자열을 검색합니다.
* 예외(`DEBUG` 모드 전용에서)를 throw하여 누락된 리소스를 디버깅하도록 오류 처리가 포함되었습니다.

다음 XAML 코드 조각에서는 태그 확장을 사용하는 방법을 보여줍니다. 작동하게 하려면 두 단계가 있습니다.

1. 루트 노드에서 사용자 지정 `xmlns:i18n` 네임스페이스를 선언합니다. `namespace` 및 `assembly`는 동일하지만 사용자 프로젝트에서 다를 수 있는 예제의 경우 프로젝트 설정과 정확히 일치해야 합니다.
2. 일반적으로 `Translate` 태그 확장을 호출하는 텍스트가 포함되는 특성에서 `{Binding}` 구문을 사용합니다. 리소스 키는 유일한 필수 매개 변수입니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        x:Class="UsingResxLocalization.FirstPageXaml"
        xmlns:i18n="clr-namespace:UsingResxLocalization;assembly=UsingResxLocalization">
    <StackLayout Padding="0, 20, 0, 0">
        <Label Text="{i18n:Translate NotesLabel}" />
        <Entry Placeholder="{i18n:Translate NotesPlaceholder}" />
        <Button Text="{i18n:Translate AddButton}" />
    </StackLayout>
</ContentPage>
```

다음과 같은 보다 자세한 정보 표시 구문은 태그 확장에도 유효합니다.

```xaml
<Button Text="{i18n:TranslateExtension Text=AddButton}" />
```

## <a name="localizing-platform-specific-elements"></a>플랫폼별 요소 지역화

Xamarin.Forms 코드로 사용자 인터페이스의 변환을 처리할 수 있지만 각 플랫폼별 프로젝트에서 수행해야 하는 몇 가지 요소가 있습니다. 이 섹션에서는 지역화 방법을 살펴봅니다.

* Application Name
* 이미지

샘플 프로젝트에는 다음과 같이 C#에서 참조하는 **flag.png**라는 지역화된 이미지가 포함됩니다.

```csharp
var flag = new Image();
switch (Device.RuntimePlatform)
{
    case Device.iOS:
    case Device.Android:
        flag.Source = ImageSource.FromFile("flag.png");
        break;
    case Device.UWP:
        flag.Source = ImageSource.FromFile("Assets/Images/flag.png");
        break;
}
```

플래그 이미지도 다음과 같이 XAML에서 참조됩니다.

```xaml
<Image>
  <Image.Source>
    <OnPlatform x:TypeArguments="ImageSource">
        <On Platform="iOS, Android" Value="flag.png" />
        <On Platform="UWP" Value="Assets/Images/flag.png" />
    </OnPlatform>
  </Image.Source>
</Image>
```

아래에 설명된 프로젝트 구조가 구현되는 한 모든 플랫폼은 지역화된 버전의 이미지에 다음과 같은 이미지 참조를 자동으로 확인합니다.

### <a name="ios-application-project"></a>iOS 애플리케이션 프로젝트

iOS는 지역화 프로젝트 또는 **.lproj** 디렉터리라는 명명 표준을 사용하여 이미지 및 문자열 리소스를 포함합니다. 이러한 디렉터리에는 앱에서 사용되는 지역화된 버전의 이미지 및 앱 이름을 지역화하는 데 사용할 수 있는 **InfoPlist.strings** 파일도 포함될 수 있습니다. iOS 지역화에 대한 자세한 내용은 [iOS 지역화](~/ios/app-fundamentals/localization/index.md)를 참조하세요.

#### <a name="images"></a>이미지

이 스크린샷에서는 언어별 **.lproj** 디렉터리가 포함된 iOS 샘플 앱을 보여줍니다. **es.lproj**라는 스페인어 디렉터리에는 **flag.png**뿐만 아니라 지역화된 버전의 기본 이미지도 포함됩니다.

![](text-images/ios-resources.png "iOS 지역화 프로젝트 디렉터리")

각 언어 디렉터리에는 해당 언어에 맞게 지역화된 **flag.png**의 복사본이 포함됩니다. 이미지가 제공되지 않으면 운영 체제의 기본값은 기본 언어 디렉터리의 이미지가 됩니다. Retina 지원의 경우 각 이미지의 **@2x** 및 **@3x** 복사본을 제공해야 합니다.

#### <a name="app-name"></a>앱 이름

**InfoPlist.strings**의 내용은 단일 키 값으로서 앱 이름을 구성합니다.

```csharp
"CFBundleDisplayName" = "ResxEspañol";
```

애플리케이션을 실행하는 경우 앱 이름 및 이미지 모두 지역화됩니다.

![](text-images/ios-imageicon.png "iOS 샘플 앱 텍스트 및 이미지 지역화")

### <a name="android-application-project"></a>Android 애플리케이션 프로젝트

Android는 언어 코드 접미사가 있는 다양한 **드로어블** 및 **문자열** 디렉터리를 사용하여 지역화된 이미지를 저장하기 위한 다른 체계를 따릅니다. 4글자 로캘 코드가 필요한 경우(예: zh-TW 또는 pt-BR) Android에서는 대시 뒤에/로캘 코드 앞에 **r**을 추가해야 합니다(예: zh-rTW 또는 pt-rBR). Android 지역화에 대한 자세한 내용은 [Android 지역화](~/android/app-fundamentals/localization.md)를 참조하세요.

#### <a name="images"></a>이미지

이 스크린샷은 일부 지역화된 드로어블 및 문자열을 통해 Android 샘플을 보여줍니다.

![](text-images/android-resources.png "Android 지역화된 드로어블 및 문자열 디렉터리")

Android에서는 중국어 간체 및 번체에 대해 zh-Hans 및 zh-Hant를 사용하지 않고 대신 zh-CN 및 zh-TW와 같은 국가별 코드를 지원합니다.

고밀도 화면에 대한 다른 해상도 이미지를 지원하려면 **drawables-es-mdpi**, **drawables-es-xdpi**, **drawables-es-xxdpi** 등의 `-*dpi` 접미사가 있는 언어 폴더를 추가로 만듭니다. 자세한 내용은 [대체 Android 리소스 제공](https://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources)을 참조하세요.

#### <a name="app-name"></a>앱 이름

**strings.xml**의 내용은 단일 키 값으로서 앱 이름을 구성합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">ResxEspañol</string>
</resources>
```

`Label`이 XML 문자열을 참조하도록 Android 앱 프로젝트에서 **MainActivity.cs**를 업데이트합니다.

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

이제 앱은 앱 이름 및 이미지를 지역화합니다. 결과(스페인어)의 스크린샷은 다음과 같습니다.

![](text-images/android-imageicon.png "Android 샘플 앱 텍스트 및 이미지 지역화")

### <a name="universal-windows-platform-application-projects"></a>유니버설 Windows 플랫폼 애플리케이션 프로젝트

유니버설 Windows 플랫폼은 이미지 및 앱 이름의 지역화를 간소화하는 리소스 인프라를 소유합니다. UWP 지역화에 대한 자세한 내용은 [UWP 지역화](/windows/uwp/design/globalizing/globalizing-portal/)를 참조하세요.

#### <a name="images"></a>이미지

다음 스크린샷에 표시된 것과 같이 리소스별 폴더에 배치하여 이미지를 지역화할 수 있습니다.

![](text-images/uwp-image-folder-structure.png "UWP 이미지 지역화 폴더 구조")

런타임 시 Windows 리소스 인프라는 사용자의 로캘에 따라 적절한 이미지를 선택합니다.

## <a name="summary"></a>요약

RESX 파일 및 .NET 글로벌화 클래스를 사용하여 Xamarin.Forms 애플리케이션을 지역화할 수 있습니다. 사용자가 선호하는 언어를 검색하기 위한 약간의 플랫폼별 코드를 제외하고는 지역화 작업의 대부분은 공용 코드로 중앙 집중화되어 있습니다.

이미지는 일반적으로 iOS 및 Android 모두에서 제공하는 다중 해상도 지원을 활용하기 위해 플랫폼 특정 방식으로 처리됩니다.

## <a name="related-links"></a>관련 링크

- [RESX 지역화 샘플](https://developer.xamarin.com/samples/xamarin-forms/UsingResxLocalization/)
- [TodoLocalized 샘플 앱](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalized/)
- [플랫폼 간 지역화](~/cross-platform/app-fundamentals/localization.md)
- [iOS 지역화](~/ios/app-fundamentals/localization/index.md)
- [Android 지역화](~/android/app-fundamentals/localization.md)
- [UWP 지역화](/windows/uwp/design/globalizing/globalizing-portal/)
- [CultureInfo 클래스 사용(MSDN)](https://msdn.microsoft.com/library/87k6sx8t%28v=vs.90%29.aspx)
- [특정 문화권에 대한 리소스 찾기 및 사용(MSDN)](https://msdn.microsoft.com/library/s9ckwb4b%28v=vs.90%29.aspx)
