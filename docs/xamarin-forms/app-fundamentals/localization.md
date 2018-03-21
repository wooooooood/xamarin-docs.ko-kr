---
title: "지역화"
description: ".NET 리소스 파일을 사용 하 여 Xamarin.Forms 응용 프로그램을 지역화할 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 852B4ED3-2D2D-48A5-A759-A6591F6A1509
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/06/2016
ms.openlocfilehash: e04ea24883bdf1e29a538aaff92c555df8e1755f
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2018
---
# <a name="localization"></a>지역화

_.NET 리소스 파일을 사용 하 여 Xamarin.Forms 응용 프로그램을 지역화할 수 있습니다._

## <a name="overview"></a>개요

.NET 응용 프로그램 사용 하 여 지역화를 위한 기본 제공 메커니즘 [RESX 파일](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx) 와의 클래스는 `System.Resources` 및 `System.Globalization` 네임 스페이스입니다. RESX 파일 번역 된 문자열을 포함 하는 번역에 대 한 강력한 형식의 액세스를 제공 하는 컴파일러에서 생성 된 클래스와 함께 Xamarin.Forms 어셈블리에 포함 됩니다. 코드에서 번역 텍스트를 검색할 수 있습니다.

### <a name="sample-code"></a>샘플 코드

이 문서와 관련 된 두 개의 샘플 가지가 있습니다.

* [UsingResxLocalization](https://github.com/xamarin/xamarin-forms-samples/tree/master/UsingResxLocalization) 설명 하는 개념의 매우 간단한을 보여 줍니다. 아래에 표시 된 코드 조각은이 샘플에서 모두 있습니다.
* [TodoLocalized](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized) 는 이러한 지역화 기술을 사용 하는 기본 작업 앱.

#### <a name="shared-projects-are-not-recommended"></a>공유 프로젝트는 권장 되지 않습니다.

TodoLocalized 샘플에 포함 되어는 [공유 프로젝트 데모](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized/SharedProject/) 빌드 시스템의 제한으로 인해 리소스 파일을 얻지 못한 있지만 **. designer.cs** 액세스 하는 기능을 중단 시키는 생성 된 파일 문자열 변환 [코드에서 강력한 형식의](~/xamarin-forms/app-fundamentals/localization.md)합니다.

이 문서의 나머지 부분에서는 PCL Xamarin.Forms 템플릿을 사용 하 여 프로젝트와 관련이 있습니다.

## <a name="globalizing-xamarinforms-code"></a>Xamarin.Forms 코드 전역화

**전역화** 응용 프로그램은 "세계에 대응 합니다."를 만드는 프로세스 즉, 서로 다른 언어를 표시할 수 있는 코드를 작성 합니다.

두는 사용자 인터페이스를 만들고 응용 프로그램 전역화의 주요 부분 중 하나 없습니다 *하드 코드 된* 텍스트입니다. 대신, 선택한 언어에 번역 된 문자열 집합이에서 사용자에 게 표시 하는 아무 것도 검색 하도록 합니다.

이 문서에 이러한 문자열을 저장 하 고이 검색할 사용자의 기본 설정에 따라 화면에 RESX 파일을 사용 하는 방법을 검토 합니다.

영어, 프랑스어, 스페인어, 독일어, 중국어, 일본어, 러시아어, 포르투갈어 (브라질) 언어를 대상으로 샘플. 응용 프로그램 필요에 따라 적은 또는 만큼 언어로 번역할 수 있습니다.

### <a name="adding-resources"></a>리소스 추가

Xamarin.Forms PCL 응용 프로그램을 전역화 하는 첫 번째 단계는 응용 프로그램에 사용 되는 모든 텍스트를 저장 하는 데 사용 될 RESX 리소스 파일 추가 하는 것입니다. 기본 텍스트를 포함 하는 RESX 파일을 추가 하 고 다음을 지 원하는 각 언어에 대 한 추가 RESX 파일을 추가 해야 합니다.

#### <a name="base-language-resource"></a>기본 언어 리소스

기본 리소스 (RESX) 파일에는 기본 언어 문자열 (샘플 영어는 기본 언어 라고 가정) 포함 됩니다. 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 Xamarin.Forms 공통 코드 프로젝트에 파일 추가 **추가 > 새 파일...** .

와 같은 의미 있는 이름을 선택 **AppResources** 누릅니다 **확인**합니다.

[![리소스 파일 추가](localization-images/resx-new-file-sml.png "새 파일 대화 상자")](localization-images/resx-new-file.png#lightbox "새 파일 대화 상자")

두 개의 파일을 프로젝트에 추가 됩니다.

* **AppResources.resx** XML 형식으로 변환할 수 있는 문자열이 저장 되는 위치는 파일입니다.
* **AppResources.designer.cs** RESX XML 파일에서 생성 된 모든 요소에 대 한 참조를 포함 하는 partial 클래스를 선언 하는 파일입니다.

솔루션 트리는 관련 파일을 표시 합니다. RESX 파일 *해야* 변환할 수 있는 새 문자열을 추가 하려면 편집할 수는 **. designer.cs** 파일 해야 *하지* 편집할 수 있습니다.

![](localization-images/appresources-tree.png "AppResources.resx 파일")

##### <a name="string-visibility"></a>문자열 표시 유형

기본적으로 강력한 형식의 참조 문자열에 생성 되 면 됩니다 `internal` 어셈블리에 있습니다. RESX 파일에 대 한 기본 빌드 도구 생성 하기 때문에 이것이 **. designer.cs** 파일 `internal` 속성입니다.

선택 된 **AppResources.resx** 파일을 표시는 **속성** 패드 여기서는이 빌드 도구를 구성 합니다. 다음 스크린샷에서 **사용자 지정 도구: ResXFileCodeGenerator**합니다.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](localization-images/vs-resx-internal-sml.png "AppResources.Resx에 대 한 속성 창")](localization-images/vs-resx-internal.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](localization-images/xs-resx-internal-sml.png "AppResources.Resx 패드 속성")](localization-images/xs-resx-internal.png#lightbox)

-----

강력한 형식의 문자열 속성을 확인 하려면 `public`, 구성을 사용 하 여 수동으로 변경 해야 **사용자 지정 도구: PublicResXFileCodeGenerator**아래 스크린샷에 표시 된 것 처럼:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](localization-images/vs-resx-public-sml.png "AppResources.Resx에 대 한 속성 창")](localization-images/vs-resx-public.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](localization-images/xs-resx-internal-sml.png "AppResources.Resx 패드 속성")](localization-images/xs-resx-internal.png#lightbox)


[![](localization-images/xs-resx-public-sml.png "AppResources.Resx 패드 속성")](localization-images/xs-resx-public.png#lightbox)

-----

이러한 변경에는 선택적 이며만 (예를 들어 경우 코드를 다른 어셈블리에 RESX 파일을 저장할) 지역화 된 문자열을 다른 어셈블리에서 참조 하고자 하는 경우 필요 합니다. 이 항목에 대 한 샘플 유지 문자열 `internal` 사용 된 동일한 Xamarin.Forms PCL 어셈블리에 정의 되기 때문입니다.

위에 표시 된 대로 기본 RESX 파일에서 사용자 지정 도구를 설정 하기만 하면 설정할 필요가 없습니다 *모든* 다음 섹션에 설명 된 특정 언어 관련 RESX 파일에서 빌드 도구입니다.

##### <a name="editing-the-resx-file"></a>RESX 파일 편집

그러나 편집기가 없습니다 기본 제공 RESX Mac.에 대 한 Visual Studio에서 새 XML의 추가 변환할 수 있는 새 문자열을 추가 하려면 `data` 각 문자열에 대 한 요소입니다. 각 `data` 요소는 다음을 포함할 수 있습니다.

* `name` (필수) 특성은 키가 변환할 수 있는 문자열입니다. 공백이 나 특수 문자 없이 사용할 수 있도록 올바른 C# 속성 이름-이어야 합니다.
* `value` 요소 (필수), 응용 프로그램에 표시 되는 실제 문자열입니다.
* `comment` 요소 (선택 사항)이이 문자열은 사용 하는 방법을 설명 하는 변환기에 대 한 지침을 포함할 수 있습니다.
* `xml:space` 특성 간격 문자열에 유지 하는 방법을 제어 하려면 (선택 사항).

몇 가지 예 `data` 요소는 다음과 같습니다.

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

사용자에 게 표시 되는 텍스트의 모든 부분을 새 기본 RESX 리소스 파일에 추가 해야 응용 프로그램 기록 될 때 `data` 요소입니다. 포함 하는 것이 좋습니다. `comment`의고품질 번역 수 있도록 가능한 한 합니다.

> [!NOTE]
> 무료 Community edition 등 visual Studio의 기본 RESX 편집기를 포함 합니다. Windows 컴퓨터에 대 한 액세스를 사용 하는 경우 추가 하 고 RESX 파일에서 문자열을 편집 하는 편리한 방법은 일 수 있는 합니다.

#### <a name="language-specific-resources"></a>언어 관련 리소스

일반적으로 기본 텍스트 문자열의 실제 번역 대규모 응용 프로그램의 작성 된 (이 경우 기본 RESX 파일 포함 됩니다 문자열의 많은) 때까지 실행 되지 않습니다. 여전히 지역화 코드를 테스트할 수 있도록 기계 번역 된 텍스트와 선택적으로 채우기 개발 주기의 초기 단계에서 특정 언어 관련 리소스를 추가 하는 것이 좋습니다.

하나의 추가 RESX 파일을 지 원하는 각 언어에 대 한 추가 됩니다.
언어 관련 리소스 파일 특별 한 명명 규칙을 따라야 합니다: 기본 리소스 파일 (예: 파일 이름이 같은 사용. **AppResources**) 뒤에 마침표 (.)와 언어 코드입니다. 간단한 예입니다.

* **AppResources.fr.resx** -프랑스어 언어 번역입니다.
* **AppResources.es.resx** -스페인어 언어 번역입니다.
* **AppResources.de.resx** -독일어 언어 번역입니다.
* **AppResources.ja.resx** -일본어 언어 번역입니다.
* **AppResources.zh Hans.resx** -중국어 (간체) 언어 번역입니다.
* **AppResources.zh Hant.resx** -중국어 (번체) 언어 번역입니다.
* **AppResources.pt.resx** -포르투갈어 언어 번역입니다.
* **AppResources.pt BR.resx** -브라질 포르투갈어 언어 번역입니다.

두 문자 언어 코드를 사용 하는 일반적인 패턴은 있지만 다른 형식 사용 되는 위치, 몇 가지 예 (예: 중국어) 및 (포르투갈어 (브라질)) 등의 다른 예는 4 가지 문자 로캘 식별자가 필요 합니다.

이러한 언어 관련 리소스 파일 *없는* 필요는 **. designer.cs** 부분 클래스와 일반 XML 파일로 추가 **빌드 작업: 포함 리소스**설정 합니다. 이 스크린샷에서 언어 관련 리소스 파일을 포함 하는 솔루션을 보여 줍니다.

![](localization-images/appresources-langs.png "언어 관련 리소스 파일")

응용 프로그램을 개발 하 고 기본 RESX 파일에 추가 된 텍스트, 해야로 보내기 각를 번역 하는 변환기가 `data` 요소 및 반환 (표시 된 명명 규칙을 사용 하 여) 언어 관련 리소스 파일을 응용 프로그램에 포함 되도록 합니다. 몇 가지 컴퓨터 변환 예는 다음과 같습니다.

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

만 `value` -변환기에 의해 업데이트 해야 하는 요소는 `comment` 변환할 수 없습니다. 기억: 이스케이프을 XML 파일을 편집 하는 경우 예약 된 등의 문자 `<`, `>`, `&` 와 `&lt;`, `&gt;`, 및 `&amp;` 에 나타난 경우에 `value` 또는 `comment`합니다.

<a name="incode" />

### <a name="using-resources-in-code"></a>코드에서 리소스 사용

RESX 리소스 파일에서 문자열 사용자 인터페이스 사용 하 여 코드에서 사용할 수 있게 됩니다는 `AppResources` 클래스입니다. `name` 는 RESX의 각 문자열에 할당 된 파일은 아래와 같이 Xamarin.Forms 코드에서 참조할 수 있는 해당 클래스의 속성:

```csharp
var myLabel = new Label ();
var myEntry = new Entry ();
var myButton = new Button ();
// populate UI with translated text values from resources
myLabel.Text = AppResources.NotesLabel;
myEntry.Placeholder = AppResources.NotesPlaceholder;
myButton.Text = AppResources.AddButton;
```

IOS, Android 및 Windows 플랫폼 렌더링 하면에서 사용자 인터페이스는 예상 텍스트 리소스에서 로드 되 고 때문에 응용 프로그램을 여러 언어로 번역할 수는 제외 이라기 보다는 하드 코딩 합니다. 다음은 UI 변환 하기 전에 각 플랫폼에서 보여 주는 스크린 샷입니다.

![](localization-images/simple-example-english.png "플랫폼 간 Ui 변환 하기 전에")

### <a name="troubleshooting"></a>문제 해결

#### <a name="testing-a-specific-language"></a>특정 언어를 테스트합니다.

시뮬레이터 또는 장치에 서로 다른 문화권을 신속 하 게 테스트 하려면 개발 하는 동안 particulary 다른 언어로 전환 까다로울 수 있습니다.

특정 언어를 설정 하 여 로드를 강제로 실행할 수는 `Culture` 이 코드 조각에 나와 있는 것 처럼:

```csharp
// force a specific culture, useful for quick testing
AppResources.Culture =  new CultureInfo("fr-FR");
```

직접 culture를 설정 합니다.이 방법을 `AppResources` 클래스 – 내 응용 프로그램 (사용 하지 않고 장치의 로캘) 언어 선택기를 구현 하는 데 사용할 수 있습니다.

#### <a name="loading-embedded-resources"></a>포함 리소스 로드

다음 코드 조각 문제 (예: RESX 파일) 포함 된 리소스를 디버그 하려고 할 때 유용 합니다. (초기 응용 프로그램 수명 주기)에 응용 프로그램에이 코드를 추가 하 고 완전 한 리소스 식별자를 나타내는 어셈블리에 포함 된 모든 리소스를 나열 하는 키를 누릅니다.

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

에 **AppResources.Designer.cs** 파일 (주위 *33 줄*), 전체 *리소스 관리자 이름* 않음 (예: 지정 된 `"UsingResxLocalization.Resx.AppResources"`) 아래 코드와 비슷합니다.

```csharp
System.Resources.ResourceManager temp =
        new System.Resources.ResourceManager(
                "UsingResxLocalization.Resx.AppResources",
                typeof(AppResources).GetTypeInfo().Assembly);
```

확인 된 **응용 프로그램 출력** 위에 나와 있는 디버그 코드의 결과 확인 하려면 올바른 리소스 나열 됩니다 (즉, `"UsingResxLocalization.Resx.AppResources"`).

그렇지 않은 경우, `AppResources` 클래스 해당 리소스를 로드할 수 없습니다.
리소스를 찾을 수 없는 문제를 해결 하려면 다음을 확인 합니다.

* 프로젝트에 대 한 기본 네임 스페이스의 루트 네임 스페이스와 일치는 **AppResources.Designer.cs** 파일입니다.
* 경우는 **AppResources.resx** 파일은 하위 디렉터리에 있으며, 하위 디렉터리 이름에는 네임 스페이스의 일부가 되어야 합니다. *및* 리소스 식별자의 일부입니다.
* **AppResources.resx** 파일에 **빌드 작업: 포함 리소스**합니다.
* **프로젝트 옵션 > 소스 코드 >.NET Naming p > 사용 하 여 Visual Studio 스타일 리소스 이름** 선택 되어 있습니다. 그러나 원하는 경우이 untick 수, 응용 프로그램 전체에서 업데이트 RESX 리소스를 참조할 때 사용 되는 네임 스페이스 해야 합니다.

#### <a name="doesnt-work-in-debug-mode-android-only"></a>디버그 모드 (Android에만 해당)에서 작동 하지 않습니다.

디버깅 하지 않음 Android 릴리스 빌드에 번역 된 문자열 작업, 마우스 오른쪽 단추로 클릭는 **Android 프로젝트** 선택 **옵션 > 빌드 > Android 빌드** 되어 있는지 확인 하 고는 **어셈블리 배포 빠른** 하지 선택 되어 있습니다. 이 옵션은 리소스를 로드 하는 문제가 시키고 지역화 된 응용 프로그램을 테스트 하는 경우 사용 하지 않아야 합니다.

### <a name="displaying-the-correct-language"></a>올바른 언어를 표시합니다.

번역을 제공할 수 있도록 코드를 작성 하는 방법을 검사에서는 지금까지 실제로 표시 되 게 할 방법이 아님. Xamarin.Forms 코드 활용할 수 있습니다. NET의 리소스를 올바른 언어로 번역 로드할 하지만 쿼리는 운영 체제에서 각 플랫폼 사용자가 선택한 언어를 결정 해야 합니다.

일부 플랫폼 특정 코드를 가져올 사용자의 언어 기본 설정 하는 데 필요 하기 때문에 사용 하 여 한 [종속성 서비스](~/xamarin-forms/app-fundamentals/dependency-service/index.md) Xamarin.Forms 응용 프로그램에서 해당 정보를 노출 하 고 각 플랫폼에 대해 구현 합니다.

먼저, 아래 코드와 유사한 사용자의 기본 문화권을 노출 하기 위한 인터페이스를 정의 합니다.

```csharp
public interface ILocalize
{
    CultureInfo GetCurrentCultureInfo ();
    void SetLocale (CultureInfo ci);
}
```

둘째, 사용 된 [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) xamarin.forms는 `App` 클래스를 호출 하는 인터페이스 및 올바른 값으로 취급 RESX 리소스 culture를 설정 합니다. 공지 수동으로이 값을 설정할 Windows Phone 및 유니버설 Windows 플랫폼에 대 한 리소스 프레임 워크 이후 자동으로 필요가 없습니다 해당 플랫폼에서 선택한 언어를 인식 합니다.

```csharp
if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
{
    var ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
    Resx.AppResources.Culture = ci; // set the RESX for resource localization
    DependencyService.Get<ILocalize>().SetLocale(ci); // set the Thread for locale-aware methods
}
```

리소스 `Culture` 응용 프로그램이 올바른 언어 문자열 사용 되도록 처음 로드 될 때 설정 해야 합니다. 필요에 따라 사용자는 응용 프로그램을 실행 하는 동안의 언어 기본 설정을 업데이트 하는 경우 iOS 또는 Android에 발생할 수 있는 플랫폼 관련 이벤트에 따라이 값을 업데이트할 수 있습니다.

에 대 한 구현을 `ILocalize` 인터페이스에 표시 되는 [플랫폼별 코드](#Platform-specific_Code) 아래 섹션. 이러한 구현은이 활용 하기 위해 `PlatformCulture` 도우미 클래스:

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

IOS, Android 및 Windows 플랫폼은 모두 약간 다른 방법으로이 정보를 노출 하기 때문에 표시 하는 언어를 검색 하기 위해 코드에서 플랫폼별 이어야 합니다. 에 대 한 코드는 `ILocalize` 종속성 서비스는 아래 각 플랫폼에 대 한 추가 플랫폼 관련 요구 사항이 되도록 함께 지역화 된 텍스트를 렌더링 올바르게 제공 합니다.

플랫폼별 코드 경우 운영 체제에서 지원 되지 않는 로캘 식별자를 구성 하는 사용자를 허용 하는 여기서도 처리 해야 합니다. NET의 `CultureInfo` 클래스입니다. 이러한 경우에 지원 되지 않는 로캘을 감지 하 여 가장 적합 한 대체 사용자 지정 코드를 작성 합니다. NET 호환 로캘입니다.

#### <a name="ios-application-project"></a>iOS 응용 프로그램 프로젝트

iOS 사용자는 기본 설정된 언어 날짜 및 시간 형식 문화권에서 개별적으로 선택 합니다. त ु म 쿼리 Xamarin.Forms 응용 프로그램을 지역화 하려면 올바른 리소스를 로드 하는 `NSLocale.PreferredLanguages` 첫 번째 요소에 대 한 배열입니다.

다음 구현과 `ILocalize` iOS 응용 프로그램 프로젝트에 종속성 서비스를 배치 해야 합니다. 코드를 인스턴스화하기 전에 밑줄 대체 iOS 대시 (즉,.NET 표준 표현) 대신 밑줄을 사용 하기 때문에 `CultureInfo` 클래스:

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
                case "ms-SG":   // "Malaysian (Singapore)" not supported .NET culture
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
> `try/catch` 블록에 `GetCurrentCultureInfo` 메서드 일반적으로 정확 하 게 일치 항목이 없을 봐 언어 (로캘에 있는 문자의 첫 번째 블록)에 기반으로 하는 비슷한 언어가 대 한 경우 로캘 지정자-와 함께 사용 되는 대체 (fallback) 동작을 모방 합니다.
>
> Xamarin.Forms에 일부 로캘에서 iOS에서는 사용할 수 있지만 유효한에 맞지 않는 `CultureInfo` .NET 및이 처리 하려고 위의 코드에서.
>
> 예를 들어 iOS **설정 > 일반 언어 &amp; 지역** 화면을 사용 하면 사용자의 휴대폰을 설정할 수 있습니다 **언어** 를 **영어** 되지만  **지역** 를 **스페인**, 로캘 문자열의 줄어들고 결과적 `"en-ES"`합니다. 경우는 `CultureInfo` 만들기 실패 코드가 다시 사용으로 처음 두 글자 방금 표시 언어를 선택 합니다.
>
> 개발자가 수정 해야는 `iOSToDotnetLanguage` 및 `ToDotnetFallbackLanguage` 메서드는 지원 되는 언어에 필요한 특정 경우를 처리 합니다.


와 같은 변환 iOS에 의해 자동으로 된 일부 시스템 정의 사용자 인터페이스 요소는 **수행** 단추는 `Picker` 제어 합니다. 지원 되는 언어를 나타내는 데 필요한 이러한 요소를 변환 하는 iOS 강제로 **Info.plist** 파일입니다. 이러한 값을 통해 추가할 수 있습니다 **Info.plist > 소스** 다음과 같이 합니다.

![Info.plist에서 지역화 키](localization-images/info-plist.png "Info.plist에서 지역화 키")

또는 열에서 **Info.plist** XML 편집기에서 파일을 직접 값을 편집 합니다.

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

종속성 서비스를 구현 하 고 업데이트 되 면 **Info.plist**, iOS 앱 지역화 된 텍스트를 표시 하는 일을 할 수 있습니다.

> [!NOTE]
> Apple에서는 포르투갈어 보다 약간 다르게 기대 되 참고 합니다.
> [자신의 docs](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW2): _"사용 pt 언어 ID로 포르투갈어 사용 중 이므로 브라질과 PT-PT에서 언어 ID로 포르투갈어 포르투갈에서 사용 중 이므로"_합니다.
> 즉 포르투갈어 언어를 선택한 비표준 로캘의 경우이 동작을 변경 하려면 코드를 작성 하지 않으면 대체 언어 ios, 포르투갈어 (브라질) 있게 됩니다 (같은 `ToDotnetFallbackLanguage` 위에).

#### <a name="android-application-project"></a>Android 응용 프로그램 프로젝트

Android를 통해 현재 선택 된 로캘 노출 `Java.Util.Locale.Default`, 밑줄 구분 기호는 대시 (다음 코드에 의해 대체) 되는 대신 사용 합니다. 이 종속성 서비스 구현을 Android 응용 프로그램 프로젝트에 추가 합니다.

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
> `try/catch` 블록에 `GetCurrentCultureInfo` 메서드 일반적으로 정확 하 게 일치 항목이 없을 봐 언어 (로캘에 있는 문자의 첫 번째 블록)에 기반으로 하는 비슷한 언어가 대 한 경우 로캘 지정자-와 함께 사용 되는 대체 (fallback) 동작을 모방 합니다.
>
> Xamarin.Forms에의 경우 일부 로캘에서 Android에서 유효 하지만 유효한에 맞지 않는 `CultureInfo` .NET 및이 처리 하려고 위의 코드에서.
>
> 개발자가 수정 해야는 `iOSToDotnetLanguage` 및 `ToDotnetFallbackLanguage` 메서드는 지원 되는 언어에 필요한 특정 경우를 처리 합니다.


이 코드는 Android 응용 프로그램 프로젝트에 추가 되 면 번역 된 문자열을 자동으로 표시 됩니다.

> [!NOTE]
>️ **경고:** 디버깅 하지 않음 Android 릴리스 빌드에 번역 된 문자열 작업, 마우스 오른쪽 단추로 클릭는 **Android 프로젝트** 선택 **옵션 > 빌드 > Android 빌드** 있는지 확인는 **어셈블리 배포 빠른** 하지 선택 되어 있습니다. 이 옵션은 리소스를 로드 하는 문제가 시키고 지역화 된 응용 프로그램을 테스트 하는 경우 사용 하지 않아야 합니다.

#### <a name="windows-application-projects"></a>Windows 응용 프로그램 프로젝트

Windows 8.1 및 유니버설 Windows 플랫폼 (UWP) 프로젝트 종속성 서비스가 필요 하지 않습니다-이러한 플랫폼 자동으로 리소스의 문화권 올바르게 설정 합니다.

이 문서의 뒷부분에 설명 된 XAML 태그 확장을 구현 해야 할 수 있습니다는 `ILocalize` Windows Phone 대 한 아래 표시 된 구현 합니다.

##### <a name="windows-phone-80"></a>Windows Phone 8.0

에 사용 되지 않지만 `App` 클래스, 여기는 대 한 Windows Phone 구현은 `ILocalize` 종속성 서비스입니다. Windows Phone 앱 프로젝트;에이 클래스를 추가 합니다. 나중에 설명 된 XAML 태그 확장을 구현 하는 경우 필요한 됩니다.

```csharp
[assembly: Dependency(typeof(UsingResxLocalization.WinPhone.Localize))]

namespace UsingResxLocalization.WinPhone
{
    public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale (CultureInfo ci) { }
        public System.Globalization.CultureInfo GetCurrentCultureInfo ()
        {
            return System.Threading.Thread.CurrentThread.CurrentUICulture;
        }
    }
}

```

Windows Phone 8.0 프로젝트에 대 한 지역화 된 표시 될 텍스트를 제대로 구성 되어야 합니다.
프로젝트 옵션에서 지원 되는 언어를 선택 해야 *및* 는 **WMAppManifest.xml** 파일입니다.
이러한 설정을 업데이트 하지 않으면 지역화 된 RESX 리소스 로드 되지 것입니다.

##### <a name="project-options"></a>프로젝트 옵션

Windows Phone 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **속성**합니다. 에 **응용 프로그램** 눈금 탭는 **지원 문화권** 지 원하는 응용 프로그램:

[![](localization-images/winphone-projectproperties-sml.png "프로젝트 속성-지원 되는 Culture")](localization-images/winphone-projectproperties.png#lightbox "프로젝트 속성-지원 되는 Culture")

##### <a name="wmappmanifestxml"></a>WMAppManifest.xml

Windows Phone 프로젝트의 속성 노드를 확장 하 고 두 번 클릭 하 고 **WMAppManifest.xml** 파일입니다. 클릭는 **패키징** 탭 하 고 응용 프로그램에서 지 원하는 모든 언어로 눈금.

[![](localization-images/winphone-wmappmanifest-sml.png "WMAppManifest.xml-지원 되는 언어")](localization-images/winphone-wmappmanifest.png#lightbox "WMAppManifest.xml-지원 되는 언어")

##### <a name="assemblyinfocs"></a>AssemblyInfo.cs

클래스 라이브러리 PCL (이식 가능한) 프로젝트의 속성 노드를 확장 하 고 두 번 클릭 하 고 **AssemblyInfo.cs** 파일입니다. 중립 리소스 어셈블리 언어를 영어로 설정 하려면 파일에 다음 줄을 추가 합니다.

```csharp
[assembly: NeutralResourcesLanguage("en")]
```

이 언어 중립 RESX 파일에 정의 된 문자열 확인 하므로 응용 프로그램의 기본 문화권의 리소스 관리자를 통해 알립니다 (**AppResources.resx**) 영어 로캘에 응용 프로그램 하나에서 실행 중인 경우에 표시 됩니다.

### <a name="example"></a>예제

플랫폼별 프로젝트에 표시 된 대로 위의 업데이트 하 고 번역 된 RESX 파일 사용 응용 프로그램을 다시 컴파일하지 후 업데이트 된 번역을 각 응용 프로그램에서는 사용할 수 있습니다. 중국어 (간체)로 변환 하는 샘플 코드의 스크린 샷을 다음과 같습니다.

![](localization-images/simple-example-hans.png "중국어 (간체)로 변환 하는 플랫폼 간 Ui")

## <a name="localizing-xaml"></a>XAML 지역화

경우 XAML에서 Xamarin.Forms 사용자 인터페이스는 태그를 작성와 비슷하게 표시이 문자열이 포함 된 XML에 직접 포함:

```xaml
<Label Text="Notes:" />
<Entry Placeholder="eg. buy milk" />
<Button Text="Add to list" />
```

이상적으로 만들어 실행할 수 있습니다는 XAML에서 직접 사용자 인터페이스 컨트롤 변환 못했습니다 우리는 *태그 확장*합니다. XAML에 RESX 리소스를 노출 하는 태그 확장에 대 한 코드는 다음과 같습니다. 이 클래스 (함께 XAML 페이지) Xamarin.Forms 공통 코드에 추가 해야 합니다.

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

위의 코드에서 중요 한 요소를 설명 하는 다음 글머리 기호.

* 클래스 이름은 `TranslateExtension`, 규칙을 함께 언급할 수에 의해 사용 하는 **번역** 우리의 태그에 있습니다.
* 클래스 구현 `IMarkupExtension`, 작업에 대 한 Xamarin.Forms 필요로 합니다.
* `"UsingResxLocalization.Resx.AppResources"` 우리의 RESX 리소스에 대 한 리소스 식별자가입니다. 이 기본 네임 스페이스, 리소스 파일이 있는 폴더 및 기본 RESX 파일 이름이 구성 됩니다.
* `ResourceManager` 클래스를 사용 하 여 만들어집니다 `IntrospectionExtensions.GetTypeInfo(typeof(TranslateExtension)).Assembly)` 리소스를 로드 하려면 현재 어셈블리를 확인할 수 있고 정적에 캐시 된 `ResMgr` 필드입니다. 으로 생성 됩니다는 `Lazy` 을 입력 하 여 만든 처음에 사용 될 때까지 지연 됩니다는 `ProvideValue` 메서드.
* `ci` 종속성 서비스를 사용 하 여 네이티브 운영 체제에서 사용자가 선택한 언어를 가져올 수 있습니다.
* `GetString` 리소스 파일에서 실제 번역 된 문자열을 검색 하는 메서드입니다. Windows Phone 8.1 및 유니버설 Windows 플랫폼 `ci` null 때문에 `ILocalize` 인터페이스는 해당 플랫폼에서 구현 되지 않습니다. 이 호출에 해당 하는 `GetString` 메서드 첫 번째 매개 변수입니다. 대신 리소스 프레임 워크에서는 자동으로 로캘을 인식 하 고 적절 한 RESX 파일에서 번역된 된 문자열을 검색 합니다.
* 오류 처리는 예외를 throw 하 여 누락 된 리소스 디버깅을 돕기 위해 포함 되었습니다 (에서 `DEBUG` 모드에만 해당).

다음 XAML 조각에는 태그 확장을 사용 하는 방법을 보여 줍니다. 작동 하려면 두 단계가 있습니다.

1. 사용자 지정 선언 `xmlns:i18n` 네임 스페이스는 루트 노드에 있습니다. `namespace` 및 `assembly` 정확히-이 예에서 동일 하지만 프로젝트에서 다 수의 프로젝트 설정과 일치 해야 합니다.
2. 사용 하 여 `{Binding}` 구문을 포함 하는 정상적으로 호출 하는 텍스트 특성에는 `Translate` 태그 확장 합니다. 리소스 키는 필요한 유일한 매개 변수를 설정 합니다.

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

다음 보다 세부적인 구문을 태그 확장에 대해 유효 합니다.

```xaml
<Button Text="{i18n:TranslateExtension Text=AddButton}" />
```

## <a name="localizing-platform-specific-elements"></a>플랫폼별 요소 지역화

Xamarin.Forms 코드에서 사용자 인터페이스의 변환에서 처리할 수 있는 म 있지만 각 플랫폼별 프로젝트에 수행 해야 하는 일부 요소가 있습니다. 이 섹션에서는 지역화 하는 방법을 설명 합니다.

* Application Name
* 이미지

호출 하는 지역화 된 이미지를 포함 하는 샘플 프로젝트 **flag.png**, 다음과 같이 C#에서 참조 되는:

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

또한 다음과 같이 XAML 플래그 이미지 참조 됩니다.

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

모든 플랫폼 아래에서 설명 하는 프로젝트 구조는 구현으로 이미지의 지역화 된 버전을 다음과 같이 이미지 참조를 자동으로 해결 됩니다.

### <a name="ios-application-project"></a>iOS 응용 프로그램 프로젝트

지역화 프로젝트 라는 이름의 명명 규칙을 사용 하 여 iOS 또는 **.lproj** 이미지와 문자열 리소스를 포함 하는 디렉터리입니다. 이 디렉터리는 응용 프로그램에서 사용 되는 이미지의 지역화 된 버전을 포함할 수 있습니다 및는 **InfoPlist.strings** 응용 프로그램 이름 필드를 지역화 하는 데 사용할 수 있는 파일입니다.

#### <a name="images"></a>이미지

이 스크린샷은 iOS 샘플 응용 프로그램을 특정 언어 관련 **.lproj** 디렉터리입니다. 스페인어 디렉터리 라는 **es.lproj**, 기본 이미지의 지역화 된 버전을 포함으로 **flag.png**:

![](localization-images/ios-resources.png "iOS 지역화 프로젝트 디렉터리")

각 언어 디렉터리의 복사본이 들어 **flag.png**, 해당 언어에 맞게 지역화 합니다. 이미지가 제공 되는 경우 운영 체제 이미지의 기본 언어 디렉터리의 기본값은입니다. 제공 해야 전체 레 티 나 지원에 대 한  **@2x**  및  **@3x**  각 이미지의 복사본입니다.

#### <a name="app-name"></a>앱 이름

콘텐츠는 **InfoPlist.strings** 은 단일 키-값만 응용 프로그램 이름을 구성 하려면:

```csharp
"CFBundleDisplayName" = "ResxEspañol";
```

응용 프로그램을 실행 하는 경우 응용 프로그램 이름 및 이미지는 둘 다 번역 됩니다.

![](localization-images/ios-imageicon.png "iOS 응용 프로그램의 예제 텍스트 및 이미지 지역화")

### <a name="android-application-project"></a>Android 응용 프로그램 프로젝트

Android 다른를 사용 하 여 지역화 된 이미지를 저장 하기 위한 다른 체계를 따릅니다 **그릴** 및 **문자열** 언어 코드 접미사를 사용 하 여 디렉터리입니다. Android에서는 추가 참고 4 문자로 로캘 코드 (예: ZH-TW 또는 PT-BR) 필요한 경우 **r** 대시/앞 다음 로캘 코드 (예:. 글꼴 rTW 또는 pt rBR).

#### <a name="images"></a>이미지

이 스크린샷은 Android 예제에는 일부 지역화 된 drawables 및 문자열:

![](localization-images/android-resources.png "지역화 된 android Drawables 및 문자열 디렉터리")

Android에서는 Zh-hans 사용 하지 및 간체 및 중국어 번체;에 대 한 Zh-hant 코드 대신,만 지원 하므로 국가별 코드 ZH-CN 및 zh-tw로 제공 합니다.

고밀도 화면에 대 한 다른 고해상도 이미지를 지원 하기 위해 사용 하 여 추가 언어 폴더를 만들 `-*dpi` 접미사와 같은 **drawables-es-mdpi**, **drawables-es-xdpi**, **drawables-es-xxdpi**등입니다. 참조 [제공 하는 대체 항목 Android 리소스](http://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources) 자세한 정보에 대 한 합니다.

#### <a name="app-name"></a>앱 이름

콘텐츠는 **strings.xml** 은 단일 키-값만 응용 프로그램 이름을 구성 하려면:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">ResxEspañol</string>
</resources>
```

업데이트는 **MainActivity.cs** Android 응용 프로그램 프로젝트에 있도록는 `Label` XML 문자열을 참조 합니다.

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

응용 프로그램에는 이제 응용 프로그램 이름 및 이미지 보여지는 합니다. 다음은 결과 (스페인어)의 스크린 샷이입니다.

![](localization-images/android-imageicon.png "Android 샘플 응용 프로그램 텍스트 및 이미지 지역화")

### <a name="windows-phone-80-application-project"></a>Windows Phone 8.0 응용 프로그램 프로젝트

Windows Phone 응용 프로그램 이름 지역화 나이 특정 지역화 된 이미지를 선택 하는 간단한 기본 제공 방법이 없는 것입니다.

#### <a name="images"></a>이미지

이 제한을 해결 하기 위한 샘플 한 제안 사항을 제공 이미지 로드를 사용 하 여 지역화 구현 하는 방법에 대 한는 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) 에 대 한는 `Image` 제어 합니다.

-아래에 사용자 지정 렌더러 코드가 나와 원본이 `FileImageSource` 파일 이름을 추출 하 고 사용 하 여 지역화 된 이미지에 대 한 경로 `CurrentUICulture`합니다. 오류율; 예상 대로 작동 되도록 일부 언어에 특별 한 처리가 필요 예제에서 몇 가지 특별 한 경우에만 제외 하 고 두 문자 언어 코드를 사용 하도록 기본값은입니다.

```csharp
using System.IO;
using Xamarin.Forms;
using Xamarin.Forms.Platform.WinPhone;

[assembly: ExportRenderer(typeof(Image), typeof(UsingResxLocalization.WinPhone.LocalizedImageRenderer))]
namespace UsingResxLocalization.WinPhone
{
    public class LocalizedImageRenderer : ImageRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Image> e)
        {
            base.OnElementChanged(e);

            if (e.NewElement != null)
            {
                var s = e.NewElement.Source as FileImageSource;
                if (s != null)
                {
                    var fileName = s.File;
                    string ci = System.Threading.Thread.CurrentThread.CurrentUICulture.ToString();
                    // you might need some custom logic here to support particular cultures and fallbacks
                    if (ci == "pt-BR") {
                        // use the complete string 'as is'
                    } else if (ci == "zh-CN") {
                         // we could have named the image directories differently,
                         // but this keeps them consisent with RESX file naming
                        ci = "zh-Hans";
                    } else if (ci == "zh-TW" || ci == "zh-HK") {
                        ci = "zh-Hant";
                    } else {
                        // for all others, just use the two-character language code
                        ci = System.Threading.Thread.CurrentThread.CurrentUICulture.TwoLetterISOLanguageName;
                    }
                    e.NewElement.Source = Path.Combine("Assets/" + ci + "/" + fileName);
                }
            }
        }
    }
}
```

이 코드는 아래 표시 된 디렉터리 구조에서 지역화 된 이미지와 함께 작동 합니다. 했으면 (예: 보다 구체적인 로캘에서 처리 하 고 이미지를 사용할 수 없는 경우에 다시 떨어지는) 특정 지역화 요구 사항을 충족 하도록 코드를 수정 하는 것이 좋습니다.

![](localization-images/winphone-resources.png "WinPhone 지역화 이미지 디렉터리 구조")

이제 Windows Phone 이미지를 보여지는 합니다. 스페인어 및 중국어 (간체)) (에서 결과의 스크린 샷을 다음과 같습니다.

![](localization-images/winphone-image-sml.png "WinPhone 샘플 응용 프로그램 텍스트 및 이미지 지역화")

#### <a name="app-name"></a>앱 이름

에 대 한 Microsoft의 설명서를 참조 [Windows Phone 8.0 앱 타이틀 지역화](http://msdn.microsoft.com/library/windows/apps/ff967550(v=vs.105).aspx)합니다.

### <a name="windows-phone-81-and-universal-windows-platform-application-projects"></a>Windows Phone 8.1 및 유니버설 Windows 플랫폼 응용 프로그램 프로젝트

Windows Phone 8.1 및 유니버설 Windows 플랫폼 둘 다의 이미지 및 응용 프로그램 이름 지역화를 간소화 하는 리소스 인프라를 소유 해야 합니다.

#### <a name="images"></a>이미지

다음 스크린샷에 표시 된 대로 이미지 리소스 관련 폴더에 배치 하 여 지역화할 수 있습니다.

![](localization-images/uwp-image-folder-structure.png "WinPhone 8.1 및 UWP 이미지 지역화 폴더 구조")

런타임 시 Windows 리소스 인프라는 사용자의 로캘에 따라 적절 한 이미지를 선택 합니다.

#### <a name="app-name"></a>앱 이름

에 대 한 Microsoft의 설명서를 참조 [Windows 8.1 스토어 앱: 사용자에 게 앱을 설명 하는 정보 지역화](https://msdn.microsoft.com/library/windows/apps/hh454044.aspx) 및 [앱 매니페스트에서 문자열 로드](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323.aspx#loading_strings_from_the_app_manifest.)합니다.

## <a name="summary"></a>요약

RESX 파일 및.NET 세계화 클래스를 사용 하 여 Xamarin.Forms 응용 프로그램을 지역화할 수 있습니다. 적은 양의 사용자가 선호 하는 언어를 검색 하도록 플랫폼별 코드를 별도로 대부분의 지역화 작업은 공통 코드에서 중앙 집중식으로 합니다.

이미지는 일반적으로 iOS 및 Android에서 제공 하는 다중 해상도 지원 활용 하기 위해 플랫폼 특정 방식으로 처리 됩니다. Windows Phone 크로스 플랫폼-친화적인 방식 이미지 필드를 지역화 하는 사용자 지정 코드를 필요 이 기능을 추가 하는 샘플 코드 제공 되었습니다.


## <a name="related-links"></a>관련 링크

- [RESX 지역화 샘플](https://developer.xamarin.com/samples/xamarin-forms/UsingResxLocalization/)
- [TodoLocalized 샘플 응용 프로그램](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalized/)
- [플랫폼 간 지역화](~/cross-platform/app-fundamentals/localization.md)
- [iOS 지역화](~/ios/app-fundamentals/localization/index.md)
- [Android 지역화](~/android/app-fundamentals/localization.md)
- [CultureInfo class (MSDN)을 사용 하 여](http://msdn.microsoft.com/library/87k6sx8t%28v=vs.90%29.aspx)
- [찾기 및 리소스를 사용 하 여 특정 문화권 (MSDN)](http://msdn.microsoft.com/library/s9ckwb4b%28v=vs.90%29.aspx)
