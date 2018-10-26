---
title: 문자열 및 이미지 지역화
description: .NET 리소스 파일을 사용 하 여 Xamarin.Forms 앱을 지역화할 수 있습니다.
ms.prod: xamarin
ms.assetid: 852B4ED3-2D2D-48A5-A759-A6591F6A1509
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/06/2016
ms.openlocfilehash: 09fe3587e4e435383822e50bd12616747b807f82
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108459"
---
# <a name="localization"></a>지역화

_.NET 리소스 파일을 사용 하 여 Xamarin.Forms 앱을 지역화할 수 있습니다._

## <a name="overview"></a>개요

.NET 응용 프로그램 사용의 지역화를 위한 기본 제공 메커니즘이 [RESX 파일](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx) 및의 클래스는 `System.Resources` 및 `System.Globalization` 네임 스페이스입니다. RESX 파일 번역 된 문자열을 포함 하는 번역에 대 한 강력한 형식의 액세스를 제공 하는 컴파일러에서 생성 된 클래스와 함께 Xamarin.Forms 어셈블리에 포함 됩니다. 코드에서 번역 된 텍스트를 검색할 수 있습니다.

### <a name="sample-code"></a>샘플 코드

이 문서와 연결 된 두 개의 샘플이 있습니다.

* [UsingResxLocalization](https://github.com/xamarin/xamarin-forms-samples/tree/master/UsingResxLocalization) 설명 하는 개념의 매우 간단한을 보여 줍니다. 아래 코드 조각은이 샘플에서 모든입니다.
* [TodoLocalized](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized) 이러한 지역화 기법을 사용 하는 기본 작업 앱.

#### <a name="shared-projects-are-not-recommended"></a>공유 프로젝트는 권장 되지 않습니다.

TodoLocalized 샘플을 [공유 프로젝트 데모](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized/SharedProject/) 빌드 시스템의 제한으로 인해 리소스 파일을 얻을 수 없는 있지만 **. designer.cs** 액세스할 수 없게 하는 생성 된 파일 코드에서 강력한 형식의 번역 된 문자열입니다.

이 문서의 나머지 부분에서는 Xamarin.Forms.NET Standard 라이브러리 템플릿을 사용 하 여 프로젝트와 관련이 있습니다.

## <a name="globalizing-xamarinforms-code"></a>Xamarin.Forms 코드 세계화

**전역화** 응용 프로그램은 "세계 대응성."를 만드는 프로세스 즉, 다양 한 언어로 표시할 수 있는 코드를 작성 합니다.

있도록 사용자 인터페이스를 빌드하는 응용 프로그램 세계화의 주요 부분 중 하나 없습니다 *하드 코드 된* 텍스트입니다. 대신 사용자에 게 표시 항목을 검색할가 선택한 언어로 번역 된 문자열 집합이에서입니다.

이 문서에는 RESX 파일 문자열을 저장 하 고 사용자의 기본 설정에 따라 표시에 대 한 검색을 사용 하는 방법을 살펴봅니다.

샘플은 영어, 프랑스어, 스페인어, 독일어, 중국어, 일본어, 러시아어 및 포르투갈어 (브라질) 언어를 대상입니다. 응용 프로그램 필요에 따라으로 적거나 많은 언어로 번역할 수 있습니다.

> [!NOTE]
> 유니버설 Windows 플랫폼에서 RESW 파일에 사용할 RESX 파일 보다는 푸시 알림 지역화 합니다. 자세한 내용은 [UWP 지역화](/windows/uwp/design/globalizing/globalizing-portal/)합니다.

### <a name="adding-resources"></a>리소스 추가

Xamarin.Forms.NET Standard 라이브러리 응용 프로그램을 전역화 하는 첫 번째 단계는 앱에서 사용 되는 모든 텍스트를 저장 하는 데 사용할 수 있는 RESX 리소스 파일을 추가 합니다. 기본 텍스트를 포함 하는 RESX 파일을 추가 하 고 다음을 지원 하려는 각 언어에 대 한 추가 RESX 파일을 추가 해야 합니다.

#### <a name="base-language-resource"></a>기본 언어 리소스

기본 리소스 (RESX) 파일 (샘플 영어가 기본 언어가 가정) 기본 언어 문자열을 포함 됩니다. 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 Xamarin.Forms 일반적인 코드 프로젝트에 파일 추가 **추가 > 새 파일...** .

와 같은 의미 있는 이름을 선택 **AppResources** 누릅니다 **확인**합니다.

[![리소스 파일 추가](text-images/resx-new-file-sml.png "새 파일 대화 상자")](text-images/resx-new-file.png#lightbox "새 파일 대화 상자")

두 파일을 프로젝트에 추가 됩니다.

* **AppResources.resx** XML 형식으로 변환할 수 있는 문자열의 저장 된 파일입니다.
* **AppResources.designer.cs** RESX XML 파일에서 생성 된 모든 요소에 대 한 참조를 포함 하는 partial 클래스를 선언 하는 파일입니다.

솔루션 트리의 관련 파일이 표시 됩니다. RESX 파일 *해야* 새 번역 가능한 문자열을 추가 하도록 편집할 수는 **. designer.cs** 파일은 *하지* 편집할 수 있습니다.

![](text-images/appresources-tree.png "AppResources.resx 파일")

##### <a name="string-visibility"></a>문자열 표시

기본적으로 문자열에 대 한 강력한 참조를 생성할 때이 될 `internal` 어셈블리에 있습니다. RESX 파일에 대 한 기본 빌드 도구에서 생성 되므로이 **. designer.cs** 사용 하 여 파일 `internal` 속성입니다.

선택 합니다 **AppResources.resx** 파일을 표시 합니다 **속성** 패드 여기서이 빌드 도구는 참조를 구성 합니다. 아래 스크린샷에서 **사용자 지정 도구: ResXFileCodeGenerator**합니다.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](text-images/vs-resx-internal-sml.png "AppResources.Resx에 대 한 속성 창")](text-images/vs-resx-internal.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](text-images/xs-resx-internal-sml.png "Properties Pad AppResources.Resx에 대 한")](text-images/xs-resx-internal.png#lightbox)

-----

강력한 형식의 문자열 속성을 확인 하려면 `public`, 구성을 수동으로 변경 해야 **사용자 지정 도구: PublicResXFileCodeGenerator**아래 스크린샷에 표시 된 것 처럼:


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](text-images/vs-resx-public-sml.png "AppResources.Resx에 대 한 속성 창")](text-images/vs-resx-public.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](text-images/xs-resx-internal-sml.png "Properties Pad AppResources.Resx에 대 한")](text-images/xs-resx-internal.png#lightbox)


[![](text-images/xs-resx-public-sml.png "Properties Pad AppResources.Resx에 대 한")](text-images/xs-resx-public.png#lightbox)

-----

이 변경은 선택 사항이 며만 (예를 들어 코드에 다른 어셈블리에 RESX 파일을 저장) 지역화 된 문자열을 다른 어셈블리에서 참조 하려는 경우 필요 합니다. 이 항목에 대 한 샘플 문자열을 벗어날 `internal` 사용 되는 동일한 Xamarin.Forms.NET Standard 라이브러리 어셈블리에 정의 되기 때문입니다.

위에 표시 된 것과 같이 기본 RESX 파일에서 사용자 지정 도구를 설정 하기만 하면 설정할 필요가 없습니다 *모든* 다음 섹션에서 설명 하는 언어별 RESX 파일에서 빌드 도구입니다.

##### <a name="editing-the-resx-file"></a>RESX 파일 편집

그러나 없는 기본 제공 RESX 편집기가 mac 용 Visual Studio에서 새 XML 추가 해야 변환할 수 있는 새 문자열 추가 `data` 각 문자열에 대 한 요소입니다. 각 `data` 요소는 다음을 포함할 수 있습니다.

* `name` (필수) 특성에는이 변환할 수 있는 문자열에 대 한 키입니다. 올바른 이어야 합니다 C# 속성 이름-공백이 나 특수 문자 없이 사용할 수 있도록 합니다.
* `value` 요소 (필수), 응용 프로그램에 표시 되는 실제 문자열입니다.
* `comment` 요소 (선택 사항)이이 문자열은 사용 하는 방법을 설명 하는 변환기에 대 한 지침을 포함할 수 있습니다.
* `xml:space` 문자열의 공백은 유지 하는 방법을 제어 하려면 (선택 사항) 사용 되는 특성입니다.

몇 가지 예제 `data` 요소 여기에 표시 됩니다.

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

응용 프로그램은 작성 된 대로 사용자에 게 표시 되는 텍스트의 모든 부분에 추가할 기본 RESX 리소스 파일을 새 `data` 요소입니다. 포함 하는 것이 좋습니다. `comment`고품질 번역 하도록 최대한 s입니다.

> [!NOTE]
> 무료 Community edition 등 visual Studio 기본 RESX 편집기를 포함 합니다. 경우는 편리한 방법을 추가 하 고 RESX 파일에서 문자열을 편집할 수 있는 Windows 컴퓨터에 액세스할 수 있습니다.

#### <a name="language-specific-resources"></a>언어 관련 리소스

일반적으로 기본 텍스트 문자열을 실제 변환 (이 경우 기본 RESX 파일은 많이 포함 문자열) 대규모 응용 프로그램의 기록 된 때까지 일어나지 않습니다. 여전히 지역화 코드를 테스트 하기 위해 기계 번역 된 텍스트를 사용 하 여 필요에 따라 채우는 개발 주기의 초반에 언어별 리소스를 추가 하는 것이 좋습니다.

지원 하려는 각 언어에 대 한 추가 RESX 파일 추가 됩니다.
언어별 리소스 파일에는 특정 명명 규칙을 따라야 합니다: 파일 이름이 같은 기본 리소스 (예: 파일 사용 **AppResources**) 뒤에 마침표 (.) 고 언어 코드입니다. 간단한 예입니다.

* **AppResources.fr.resx** -프랑스어 언어 번역 합니다.
* **AppResources.es.resx** -스페인어 번역 합니다.
* **AppResources.de.resx** -독일어 언어 번역 합니다.
* **AppResources.ja.resx** -한국어 언어 번역 합니다.
* **AppResources.zh Hans.resx** -중국어 (간체) 언어 번역 합니다.
* **AppResources.zh Hant.resx** -중국어 (번체) 언어 번역 합니다.
* **AppResources.pt.resx** -포르투갈어 언어 번역 합니다.
* **AppResources.pt BR.resx** -브라질 포르투갈어 언어 번역 합니다.

일반적인 패턴 2 자 언어 코드를 사용 하는 것도 있습니다 (예: 포르투갈어 (브라질)) 다른 내용과 다른 형식으로 사용 된 위치 (예: 중국어) 몇 가지 예는 4 가지 문자 로캘 식별자가 필요.

이러한 언어별 리소스 파일 *하지 않습니다* 필요는 **. designer.cs** 사용 하 여 일반 XML 파일로 추가할 수 있도록 partial 클래스는 **빌드 작업: EmbeddedResource**설정 합니다. 이 스크린샷은 언어별 리소스 파일을 포함 하는 솔루션을 보여 줍니다.

![](text-images/appresources-langs.png "언어별 리소스 파일")

응용 프로그램 개발 하 고 기본 RESX 파일에 추가 된 텍스트, 있습니다 해야 보낼 각를 번역 하는 변환기를 `data` 요소 및 반환 (표시 된 명명 규칙을 사용 하 여) 언어별 리소스 파일을 앱에 포함 되도록 합니다. 일부 컴퓨터 변환 예제는 다음과 같습니다.

**AppResources.es.resx (스페인어)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>Escribir un artículo</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

**AppResources.ja.resx (일본어)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>新しい項目を追加</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

**AppResources.pt-BR.resx (포르투갈어 (브라질))**

```xml
<data name="AddButton" xml:space="preserve">
    <value>adicionar novo item</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

만 `value` 요소 변환기-가 업데이트 해야 합니다 `comment` 변환할 수 없습니다. 기억: 이스케이프 XML 파일을 편집 하는 경우 예약 된 문자 등 `<`, `>`, `&` 사용 하 여 `&lt;`, `&gt;`, 및 `&amp;` 에 나타난 경우에 `value` 또는 `comment`합니다.

<a name="incode" />

### <a name="using-resources-in-code"></a>코드에서 리소스 사용

RESX 리소스 파일에서 문자열 사용자 인터페이스 사용 하 여 코드에서 사용할 수는 `AppResources` 클래스입니다. `name` RESX에서 각 문자열에 할당 된 파일은 아래와 같이 Xamarin.Forms 코드에서 참조할 수 있는 해당 클래스의 속성:

```csharp
var myLabel = new Label ();
var myEntry = new Entry ();
var myButton = new Button ();
// populate UI with translated text values from resources
myLabel.Text = AppResources.NotesLabel;
myEntry.Placeholder = AppResources.NotesPlaceholder;
myButton.Text = AppResources.AddButton;
```

IOS, Android 및 유니버설 Windows 플랫폼 (UWP) 렌더링 때 사용자 인터페이스가 기대 있기 텍스트 리소스에서 로드 되 고 있으므로 앱을 여러 언어로 번역을 제외 하 고 대신 하드 코딩 합니다. 변환 하기 전에 각 플랫폼에서 UI를 보여 주는 스크린샷은 다음과 같습니다.

![](text-images/simple-example-english.png "변환 하기 전에 플랫폼 간 Ui")

### <a name="troubleshooting"></a>문제 해결

#### <a name="testing-a-specific-language"></a>특정 언어를 테스트합니다.

다른 문화권을 신속 하 게 테스트 하려는 경우 개발 중 특히 다른 언어로 시뮬레이터 또는 장치를 전환 하기 어려울 수 있습니다.

특정 언어를 설정 하 여 로드 할 수 있습니다는 `Culture` 이 코드 조각과 같이:

```csharp
// force a specific culture, useful for quick testing
AppResources.Culture =  new CultureInfo("fr-FR");
```

문화권에서 직접 설정-이 방법은 `AppResources` 클래스-내부 앱 대신 장치의 로캘을 사용 하 여 언어 선택기를 구현 하려면 사용할 수도 있습니다.

#### <a name="loading-embedded-resources"></a>포함 리소스 로드

다음 코드 조각 포함 된 리소스 (예: RESX 파일)를 사용 하 여 문제를 디버깅 하려고 할 때 유용 합니다. (초기 앱 수명 주기)에서 앱에이 코드를 추가 하 고 전체 리소스 식별자를 나타내는 어셈블리에 포함 된 모든 리소스가 나열 됩니다.

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

에 **AppResources.Designer.cs** 파일 (주위 *33 번 줄*), 전체 *리소스 관리자 이름* 지정 (예: `"UsingResxLocalization.Resx.AppResources"`) 아래 코드와 비슷합니다.

```csharp
System.Resources.ResourceManager temp =
        new System.Resources.ResourceManager(
                "UsingResxLocalization.Resx.AppResources",
                typeof(AppResources).GetTypeInfo().Assembly);
```

확인 합니다 **응용 프로그램 출력** 위에 표시 된 디버그 코드의 결과 확인 하 고 올바른 리소스 나열 됩니다 (즉 `"UsingResxLocalization.Resx.AppResources"`).

그렇지 않은 경우, `AppResources` 클래스는 해당 리소스를 로드할 수 없습니다.
리소스를 찾을 수 없는 문제를 해결 하려면 다음을 확인 합니다.

* 프로젝트에 대 한 기본 네임 스페이스의 루트 네임 스페이스와 일치 하는 **AppResources.Designer.cs** 파일입니다.
* 경우는 **AppResources.resx** 파일이 하위 디렉터리에서에 하위 디렉터리 이름에는 네임 스페이스의 일부 여야 합니다 *및* 리소스 식별자의 일부입니다.
* 합니다 **AppResources.resx** 파일이 **빌드 작업: EmbeddedResource**합니다.
* 합니다 **프로젝트 옵션 > 소스 >.NET 명명 정책 > 사용 하 여 Visual Studio 스타일 리소스 이름** 선택 됩니다. 그러나 원하는 경우이 untick 수, 앱 전체에서 업데이트 RESX 리소스를 참조할 때 사용 된 네임 스페이스를 해야 합니다.

#### <a name="doesnt-work-in-debug-mode-android-only"></a>디버그 모드 (Android에만 해당)에서 작동 하지 않습니다.

디버깅 하지 않음 Android 릴리스 빌드에 번역 된 문자열 작업, 마우스 오른쪽 단추로 클릭 합니다 **Android 프로젝트** 선택한 **옵션 > 빌드 > Android 빌드** 있는지 확인 합니다 **빠른 어셈블리 배포** 가 선택 되지 않습니다. 이 리소스 로드 중에 문제가 생길 옵션과 지역화 된 응용 프로그램을 테스트 하는 경우 사용할 수 해야 합니다.

### <a name="displaying-the-correct-language"></a>올바른 언어를 표시합니다.

번역을 제공할 수 있습니다, 있도록 코드를 작성 하는 방법을 살펴보았습니다 지금 실제로 표시 하기 위해 방법이 아님. Xamarin.Forms 코드 활용을 걸릴 수 있습니다. NET의 리소스를 올바른 언어로 번역 로드할 하지만 사용자가 선택한 언어를 결정 하는 각 플랫폼에서 운영 체제를 쿼리할 해야 합니다.

플랫폼별 코드도 가져올 사용자의 언어 기본 설정 하는 데 필요 하므로 사용을 [종속성 서비스](~/xamarin-forms/app-fundamentals/dependency-service/index.md) Xamarin.Forms 앱에서 해당 정보를 노출 하 여 각 플랫폼에 대 한 구현 합니다.

먼저 아래 코드와 비슷한 사용자의 기본 설정된 문화권을 노출 하기 위한 인터페이스를 정의 합니다.

```csharp
public interface ILocalize
{
    CultureInfo GetCurrentCultureInfo ();
    void SetLocale (CultureInfo ci);
}
```

둘째, 사용 합니다 [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) Xamarin.Forms에서 `App` 인터페이스를 호출 하 여 올바른 값으로 RESX 리소스 문화를 설정 하는 클래스입니다. 이러한 플랫폼에서 선택한 언어를 인식 하는 알림이 값을 수동으로 설정할이 유니버설 Windows 플랫폼의 경우 리소스 framework 이후 자동으로 필요가 없습니다.

```csharp
if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
{
    var ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
    Resx.AppResources.Culture = ci; // set the RESX for resource localization
    DependencyService.Get<ILocalize>().SetLocale(ci); // set the Thread for locale-aware methods
}
```

리소스 `Culture` 응용 프로그램이 올바른 언어 문자열 사용 되도록 처음 로드 될 때 설정 해야 합니다. 필요에 따라 iOS 또는 Android에서 앱 실행 되는 동안 사용자에 해당 언어 기본 설정을 업데이트 하는 경우 발생할 수 있는 플랫폼 특정 이벤트에 따라이 값을 업데이트할 수 있습니다.

에 대 한 구현을 합니다 `ILocalize` 인터페이스에 표시 됩니다는 [플랫폼별 코드](#Platform-specific_Code) 섹션 아래. 이러한 구현은이 활용 `PlatformCulture` 도우미 클래스:

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

### <a name="platform-specific-code"></a>플랫폼 특정 코드

IOS, Android 및 UWP 모두 약간 다른 방법으로이 정보를 노출 하기 때문에 표시할 언어를 검색 하는 코드에서 플랫폼별 이어야 합니다. 에 대 한 코드는 `ILocalize` 종속성 서비스가 아래 각 플랫폼에 대해 추가 플랫폼 특정 요구 사항이 되도록 함께 지역화 된 텍스트가 올바르게 렌더링 제공 합니다.

플랫폼 특정 코드는 운영 체제를 사용 하면에서 지원 되지 않는 로캘 식별자를 구성 하는 경우 처리도 해야 합니다. NET의 `CultureInfo` 클래스입니다. 이러한 경우를 지원 되지 않는 로캘을 검색 하 여 최상의 대체 사용자 지정 코드를 작성 합니다. NET-호환 되는 로캘.

#### <a name="ios-application-project"></a>iOS 응용 프로그램 프로젝트

iOS 사용자 자신의 기본 언어로 날짜 및 시간 형식 문화권에서 개별적으로 선택 합니다. 쿼리 하기만 Xamarin.Forms 앱을 지역화 하려면 올바른 리소스를 로드 하는 `NSLocale.PreferredLanguages` 첫 번째 요소에 대 한 배열입니다.

다음 구현과 `ILocalize` iOS 응용 프로그램 프로젝트에서 종속성 서비스를 배치 해야 합니다. 코드를 인스턴스화하기 전에 밑줄 대체 iOS 대시 (즉,.NET 표준 표현) 대신 밑줄을 사용 하기 때문에 `CultureInfo` 클래스:

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
            var netLanguage = iOSLanguage;
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
> 합니다 `try/catch` 블록을 `GetCurrentCultureInfo` 없으면 정확히 일치를 확인 근접 언어 (로캘에서 문자의 첫 번째 블록)만 기준에 대 한 로캘 지정자 – 일반적으로 사용 하는 대체 (fallback) 동작을 모방 하는 메서드.
>
> Xamarin.Forms의 경우 일부 로캘에서 iOS에서 유효 하지만 유효한에 맞지 않는 `CultureInfo` .NET 및이 처리 하려고 위의 코드에서.
>
> 예를 들어, iOS **설정 > 일반 언어 &amp; 지역** 화면을 사용 하면 휴대폰을 설정할 수 있습니다 **언어** 하 **영어** 되지만  **지역** 하 **스페인**, 로캘 문자열의 결과 `"en-ES"`합니다. 경우는 `CultureInfo` 생성 실패 코드를 사용 하도록 대체 처음 두 문자 바로 표시 언어를 선택 합니다.
>
> 개발자가 수정 해야 합니다 `iOSToDotnetLanguage` 및 `ToDotnetFallbackLanguage` 해당 지원 되는 언어에 필요한 특정 사례를 처리 하는 방법입니다.


같은 변환 iOS에 의해 자동으로 된 일부 시스템에 정의 된 사용자 인터페이스 요소가 합니다 **수행** 단추를 `Picker` 제어 합니다. IOS에서 지원 되는 언어를 지정 해야 하는 이러한 요소를 변환 하도록 합니다 **Info.plist** 파일입니다. 통해 이러한 값을 추가할 수 있습니다 **Info.plist > 소스** 다음과 같이 합니다.

![Info.plist 키 지역화](text-images/info-plist.png "지역화 Info.plist 키")

또는 열을 **Info.plist** XML 편집기에서 파일 및 값을 직접 편집 합니다.

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

종속성 서비스를 구현 하 고 업데이트 했으면 **Info.plist**, iOS 앱이 지역화 된 텍스트를 표시할 수 있게 됩니다.

> [!NOTE]
> Apple에서는 포르투갈어 보다 약간 다르게 기대 하는 참고 합니다.
> [해당 docs](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW2): _"(태평양 표준시)로 사용할 언어 ID 포르투갈어 사용 되므로 브라질 및 PT-PT 언어 ID로 포르투갈어 포르투갈에서 사용 됩니다"_ 합니다.
> 즉 포르투갈어 언어에서에서 선택 하는 비표준 로캘, 대체 언어 됩니다 포르투갈어 (브라질), iOS에서이 동작을 변경 하려면 코드를 작성 하지 않는 한 (같은 `ToDotnetFallbackLanguage` 위에).

IOS 지역화에 대 한 자세한 내용은 참조 하세요. [지역화 iOS](~/ios/app-fundamentals/localization/index.md)합니다.

#### <a name="android-application-project"></a>Android 응용 프로그램 프로젝트

Android를 통해 현재 선택된 된 로캘 노출 `Java.Util.Locale.Default`, 대시 (하는 다음 코드에 의해 대체 됩니다) 대신 밑줄 구분 기호를 사용 합니다. Android 응용 프로그램 프로젝트에이 종속성 서비스 구현을 추가 합니다.

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
> 합니다 `try/catch` 블록을 `GetCurrentCultureInfo` 없으면 정확히 일치를 확인 근접 언어 (로캘에서 문자의 첫 번째 블록)만 기준에 대 한 로캘 지정자 – 일반적으로 사용 하는 대체 (fallback) 동작을 모방 하는 메서드.
>
> Xamarin.Forms의 경우 일부 로캘에서 Android에서 유효 하지만 유효한에 맞지 않는 `CultureInfo` .NET 및이 처리 하려고 위의 코드에서.
>
> 개발자가 수정 해야 합니다 `iOSToDotnetLanguage` 및 `ToDotnetFallbackLanguage` 해당 지원 되는 언어에 필요한 특정 사례를 처리 하는 방법입니다.

이 코드는 Android 응용 프로그램 프로젝트에 추가 된 후 번역 된 문자열을 자동으로 표시 됩니다.

> [!NOTE]
>️ **경고:** 디버깅 하지 않음 Android 릴리스 빌드에 번역 된 문자열 작업, 마우스 오른쪽 단추로 클릭 합니다 **Android 프로젝트** 선택한 **옵션 > 빌드 > Android 빌드** 있는지 확인 합니다 **빠른 어셈블리 배포** 가 선택 되지 않습니다. 이 리소스 로드 중에 문제가 생길 옵션과 지역화 된 응용 프로그램을 테스트 하는 경우 사용할 수 해야 합니다.

Android 지역화에 대 한 자세한 내용은 참조 하세요. [Android 지역화](~/android/app-fundamentals/localization.md)합니다.

#### <a name="universal-windows-platform"></a>유니버설 Windows 플랫폼

UWP (유니버설 Windows 플랫폼) 프로젝트에 종속 서비스가 필요 하지 않습니다. 대신,이 플랫폼 자동으로 리소스의 문화권을 설정 올바르게 합니다.

##### <a name="assemblyinfocs"></a>AssemblyInfo.cs

.NET Standard 라이브러리 프로젝트에서 속성 노드를 확장 하 고 두 번 클릭 합니다 **AssemblyInfo.cs** 파일입니다. 중립 리소스 어셈블리 언어를 영어로 설정 하려면 파일에 다음 줄을 추가 합니다.

```csharp
[assembly: NeutralResourcesLanguage("en")]
```

따라서 되도록 언어 중립 RESX 파일에 정의 된 문자열을 앱의 기본 문화권의 리소스 관리자에 알립니다 (**AppResources.resx**) 로캘이 영어인 하나에서 앱을 실행 하는 경우에 표시 됩니다.

### <a name="example"></a>예제

플랫폼별 프로젝트와 같이 위의 업데이트 하 고 번역 된 RESX 파일을 사용 하 여 앱을 다시 컴파일하지 후 업데이트 된 번역 각 앱에서 사용할 수 있습니다. 중국어 (간체)로 변환 하는 샘플 코드의 스크린샷은 다음과 같습니다.

![](text-images/simple-example-hans.png "중국어 (간체)로 변환 하는 플랫폼 간 Ui")

UWP 지역화에 대 한 자세한 내용은 참조 하세요. [UWP 지역화](/windows/uwp/design/globalizing/globalizing-portal/)합니다.

## <a name="localizing-xaml"></a>XAML 지역화

XML에서 직접 포함 된 경우 XAML에서 Xamarin.Forms 사용자 인터페이스 태그 제작 유사이 문자열을 사용 하 여:

```xaml
<Label Text="Notes:" />
<Entry Placeholder="eg. buy milk" />
<Button Text="Add to list" />
```

이상적으로 사용자 인터페이스 컨트롤을 만들어 수행할 수 있습니다는 XAML 직접 변환할 수 것을 *태그 확장*합니다. XAML에 RESX 리소스를 노출 하는 태그 확장에 대 한 코드는 다음과 같습니다. 이 클래스는 Xamarin.Forms 공통 코드 (함께 XAML 페이지)에 추가할:

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

다음 글머리 기호 목록 위의 코드에서 중요 한 요소에 설명 합니다.

* 클래스 이름은 `TranslateExtension`은 규칙에 따라를 참조할 수 있지만 **변환** 는 태그입니다.
* 클래스를 구현 하는 `IMarkupExtension`, Xamarin.Forms에 대 한 작업 필요로 합니다.
* `"UsingResxLocalization.Resx.AppResources"` RESX 리소스에 대 한 리소스 식별자가입니다. 기본 네임 스페이스, 리소스 파일이 있는 폴더와 기본 RESX 파일의 구성 됩니다.
* 합니다 `ResourceManager` 클래스를 사용 하 여 만들어집니다 `IntrospectionExtensions.GetTypeInfo(typeof(TranslateExtension)).Assembly)` 에서 리소스를 로드 현재 어셈블리를 확인할 수는 정적 캐시 및 `ResMgr` 필드입니다. 으로 생성 한 `Lazy` 을 입력 하 여 처음에 사용 될 때까지 생성이 지연는 `ProvideValue` 메서드.
* `ci` 종속성 서비스를 사용 하 여 네이티브 운영 체제에서 사용자가 선택한 언어를 가져옵니다.
* `GetString` 리소스 파일에서 실제 번역 된 문자열을 검색 하는 방법이입니다. 유니버설 Windows 플랫폼의 경우 `ci` null이 됩니다 때문에 `ILocalize` 인터페이스는 이러한 플랫폼에서 구현 되지 않습니다. 이 호출에 해당 하는 `GetString` 첫 번째 매개 변수를 사용 하 여 메서드. 대신 리소스 프레임 워크는 로캘을 자동으로 인식 됩니다 하 고 적절 한 RESX 파일에서 번역 된 문자열을 검색 합니다.
* 오류 처리는 예외를 throw 하 여 누락 된 리소스를 디버깅 하는 데 포함 되어 있습니다 (에서 `DEBUG` 모드인 경우에).

다음 XAML 코드 조각에는 태그 확장을 사용 하는 방법을 보여 줍니다. 작동 하도록 하는 방법은 다음 두 단계가 있습니다.

1. 사용자 지정 선언 `xmlns:i18n` 루트 노드에 네임 스페이스입니다. 합니다 `namespace` 고 `assembly` 정확히-이 예제는 동일 하지만 프로젝트에서 다를 수 프로젝트 설정과 일치 해야 합니다.
2. 사용 하 여 `{Binding}` 구문을 포함 하는 일반적으로 호출 하는 텍스트 특성에는 `Translate` 태그 확장 합니다. 리소스 키는 유일한 필수 매개 변수를 설정 합니다.

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

다음 보다 세부적인 구문을 태그 확장에 대해서도 유효 합니다.

```xaml
<Button Text="{i18n:TranslateExtension Text=AddButton}" />
```

## <a name="localizing-platform-specific-elements"></a>플랫폼 특정 요소를 지역화합니다.

Xamarin.Forms 코드의 사용자 인터페이스를 사용 하는 변환에서 처리할 수 있는 것 이지만 각 플랫폼별 프로젝트에서 수행 해야 하는 몇 가지 요소가 있습니다. 이 섹션에서는 지역화 하는 방법을 살펴봅니다.

* Application Name
* 이미지

호출 하는 지역화 된 이미지를 포함 하는 샘플 프로젝트 **flag.png**, 참조 되는 C# 다음과 같습니다.

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

플래그 이미지 같이 XAML 에서도 참조 됩니다.

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

자동으로 모든 플랫폼은 아래에 설명 된 프로젝트 구조는 구현으로 이미지의 지역화 된 버전에 다음과 같은 이미지 참조를 확인 해야 합니다.

### <a name="ios-application-project"></a>iOS 응용 프로그램 프로젝트

iOS는 지역화 프로젝트 라는 이름의 명명 표준을 사용 하거나 **.lproj** 이미지 및 문자열 리소스가 포함 된 디렉터리입니다. 이러한 디렉터리는 앱에서 사용 되는 이미지의 지역화 된 버전을 포함할 수 있습니다 및 합니다 **InfoPlist.strings** 앱 이름을 지역화 하는 파일입니다. IOS 지역화에 대 한 자세한 내용은 참조 하세요. [지역화 iOS](~/ios/app-fundamentals/localization/index.md)합니다.

#### <a name="images"></a>이미지

이 스크린샷은 iOS 샘플 앱에 언어별 **.lproj** 디렉터리입니다. 스페인어 디렉터리 호출 **es.lproj**, 기본 이미지의 지역화 된 버전이 포함 된 뿐만 **flag.png**:

![](text-images/ios-resources.png "iOS 지역화 프로젝트 디렉터리")

복사본을 포함 하는 각 언어 디렉터리 **flag.png**, 해당 언어에 맞게 지역화 합니다. 이미지가 없는 경우 운영 체제 이미지의 기본 언어 디렉터리의 기본값은입니다. 제공 해야 전체 레 티 나 지원용 **@2x** 하 고 **@3x** 각 이미지의 복사본입니다.

#### <a name="app-name"></a>앱 이름

콘텐츠를 **InfoPlist.strings** 는 방금 단일 키-값 앱 이름을 구성 하려면:

```csharp
"CFBundleDisplayName" = "ResxEspañol";
```

응용 프로그램을 실행 하는 경우 앱 이름 및 이미지 모두 지역화 됩니다.

![](text-images/ios-imageicon.png "iOS 샘플 앱 텍스트 및 이미지 지역화")

### <a name="android-application-project"></a>Android 응용 프로그램 프로젝트

Android 다른을 사용 하 여 지역화 된 이미지를 저장 하기 위한 다른 체계를 따릅니다 **drawable** 하 고 **문자열** 언어 코드 접미사를 사용 하 여 디렉터리입니다. 4 자로 로캘 코드 (예: ZH-TW 또는 PT-BR) 필요한 경우 Android 추가 해야 한다는 점에 유의 **r** dash/앞 다음 로캘 코드 (예: zh rTW 또는 pt rBR). Android 지역화에 대 한 자세한 내용은 참조 하세요. [Android 지역화](~/android/app-fundamentals/localization.md)합니다.

#### <a name="images"></a>이미지

이 스크린샷은 Android는 일부 샘플 드로어 블 및 문자열을 지역화 합니다.

![](text-images/android-resources.png "Android 로컬 드로어 블 디렉터리 문자열")

Android에서 Zh-hans를 사용 하지 않음을 참고 및 간체 및 중국어 번체; Zh-hant 코드 대신, 해당 국가 특정 코드 ZH-CN 및 zh-tw로 제공에만 지원합니다.

고밀도 화면의 다른 고해상도 이미지를 지원 하기 위해 사용 하 여 추가 언어 폴더를 만듭니다 `-*dpi` 접미사와 같은 **드로어 블-es-mdpi**를 **드로어 블-es-xdpi**합니다 **드로어 블-es-xxdpi**등입니다. 참조 [대신 Android 리소스 제공](http://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources) 자세한 내용은 합니다.

#### <a name="app-name"></a>앱 이름

콘텐츠를 **strings.xml** 는 방금 단일 키-값 앱 이름을 구성 하려면:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">ResxEspañol</string>
</resources>
```

업데이트를 **MainActivity.cs** Android 앱 프로젝트에서 있도록는 `Label` XML 문자열을 참조 합니다.

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

이제 앱에는 이미지와 앱 이름을 문제의 지역화 합니다. 스크린샷 (스페인어)의 결과 다음과 같습니다.

![](text-images/android-imageicon.png "Android 샘플 앱 텍스트 및 이미지 지역화")

### <a name="universal-windows-platform-application-projects"></a>유니버설 Windows 플랫폼 응용 프로그램 프로젝트

유니버설 Windows 플랫폼 이미지 및 앱 이름 지역화를 간소화 하는 리소스 인프라를 소유 합니다. UWP 지역화에 대 한 자세한 내용은 참조 하세요. [UWP 지역화](/windows/uwp/design/globalizing/globalizing-portal/)합니다.

#### <a name="images"></a>이미지

다음 스크린샷에 표시 된 것과 같이 리소스 특정 폴더에 배치 하 여 이미지를 지역화할 수 있습니다.

![](text-images/uwp-image-folder-structure.png "UWP 이미지 지역화 폴더 구조")

런타임에 Windows 리소스 인프라는 사용자의 로캘을 기반으로 적절 한 이미지를 선택 합니다.

## <a name="summary"></a>요약

RESX 파일 및.NET 세계화 클래스를 사용 하 여 Xamarin.Forms 응용 프로그램을 지역화할 수 있습니다. 약간의 사용자가 선호 언어를 검색 하기 위해 플랫폼별 코드를 외에도 지역화 작업의 대부분은 공용 코드의 중앙 배포 됩니다.

이미지는 일반적으로 iOS 및 Android에서 제공 하는 다중 해상도 지원 활용 하기 위해 플랫폼 특정 방식으로 처리 됩니다.

## <a name="related-links"></a>관련 링크

- [RESX 지역화 샘플](https://developer.xamarin.com/samples/xamarin-forms/UsingResxLocalization/)
- [TodoLocalized 샘플 앱](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalized/)
- [플랫폼 간 지역화](~/cross-platform/app-fundamentals/localization.md)
- [iOS 지역화](~/ios/app-fundamentals/localization/index.md)
- [Android 지역화](~/android/app-fundamentals/localization.md)
- [UWP 지역화](/windows/uwp/design/globalizing/globalizing-portal/)
- [CultureInfo 클래스 (MSDN)를 사용 하 여](http://msdn.microsoft.com/library/87k6sx8t%28v=vs.90%29.aspx)
- [찾기 및 리소스를 사용 하 여 특정 문화권 (MSDN)](http://msdn.microsoft.com/library/s9ckwb4b%28v=vs.90%29.aspx)
