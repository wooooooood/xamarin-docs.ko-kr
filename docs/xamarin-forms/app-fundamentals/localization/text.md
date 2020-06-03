---
title: Xamarin.Forms의 문자열 및 이미지 지역화
description: .NET 리소스 파일을 사용하여 Xamarin.Forms 앱을 지역화할 수 있습니다.
zone_pivot_groups: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: af15dc5a23404a11be6207bef7b4fc3e4bf9fad7
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137607"
---
# <a name="xamarinforms-string-and-image-localization"></a>Xamarin.Forms 문자열 및 이미지 지역화

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/usingresxlocalization)

지역화는 대상 시장의 특정 언어 또는 문화적 요구 사항을 충족하도록 애플리케이션을 조정하는 프로세스입니다. 지역화를 수행하려면 애플리케이션의 텍스트와 이미지를 여러 언어로 번역해야 할 수 있습니다. 지역화된 애플리케이션은 모바일 디바이스의 문화권 설정에 따라 번역된 텍스트를 자동으로 표시합니다.

![iOS 및 Android의 지역화 애플리케이션 스크린샷](text-images/localizationdemo-screenshots.png)

.NET Framework에는 [Resx 리소스 파일](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps)을 사용하여 애플리케이션을 지역화하는 기본 제공 메커니즘이 포함되어 있습니다. 리소스 파일은 애플리케이션에서 제공된 키에 대한 콘텐츠를 검색할 수 있게 해주는 이름/값 쌍으로 텍스트 및 기타 콘텐츠를 저장합니다. 리소스 파일을 사용하면 지역화된 콘텐츠를 애플리케이션 코드에서 분리할 수 있습니다.

리소스 파일을 사용하여 Xamarin.Forms 애플리케이션을 지역화하려면 다음 단계를 수행해야 합니다.

1. 번역된 텍스트가 포함된 [Resx 파일을 만듭니다](#create-resx-files).
1. 공유 프로젝트에 [기본 문화권을 지정](#specify-the-default-culture)합니다.
1. [Xamarin.Forms의 텍스트를 지역화](#localize-text-in-xamarinforms)합니다.
1. 각 플랫폼의 문화권 설정에 따라 [이미지를 지역화](#localize-images)합니다.
1. 각 플랫폼에서 [애플리케이션 이름을 지역화](#localize-the-application-name)합니다.
1. 각 플랫폼에서 [지역화를 테스트](#test-localization)합니다.

## <a name="create-resx-files"></a>Resx 파일 만들기

리소스 파일은 빌드 프로세스 중에 이진 리소스(.resources) 파일로 컴파일되는 **.resx** 확장명을 가진 XML 파일입니다. Visual Studio 2019는 리소스를 검색하는 데 사용되는 API를 제공하는 클래스를 생성합니다. 지역화된 애플리케이션은 일반적으로 애플리케이션에서 사용되는 모든 문자열이 들어 있는 기본 리소스 파일과 지원되는 각 언어의 리소스 파일을 포함합니다. 샘플 애플리케이션은 공유 프로젝트에 리소스 파일과 **AppResources.resx**라는 기본 리소스 파일을 포함하는 **Resx** 폴더가 있습니다.

리소스 파일은 각 항목의 다음 정보를 포함합니다.

- **이름**은 코드의 텍스트에 액세스하는 데 사용되는 키를 지정합니다.
- **값**은 번역된 텍스트를 지정합니다.
- **주석**은 추가 정보를 포함하는 선택적 필드입니다.

::: zone pivot="windows"

리소스 파일은 Visual Studio 2019의 **새 항목 추가** 대화 상자에서 추가합니다.

![Visual Studio 2019에서 새 리소스 추가](text-images/pc-add-resource-file.png)

파일이 추가된 후 각 텍스트 리소스의 행을 추가할 수 있습니다.

![.resx 파일에 기본 텍스트 리소스 지정](text-images/pc-default-strings.png)

**액세스 한정자** 드롭다운 설정은 Visual Studio에서 리소스에 액세스하는 데 사용되는 클래스를 생성하는 방법을 결정합니다. 액세스 한정자를 **Public** 또는 **Internal**로 설정하면 지정된 접근성 수준으로 클래스가 생성됩니다. 액세스 한정자를 **코드 생성 안 됨**으로 설정하면 클래스 파일이 생성되지 않습니다. 기본 리소스 파일은 클래스 파일을 생성하도록 구성해야 합니다. 그러면 확장명이 **.designer.cs**인 파일이 프로젝트에 추가됩니다.

기본 리소스 파일을 만든 후에는 애플리케이션에서 지원하는 각 문화권에 대해 추가 파일을 만들 수 있습니다. 각 추가 리소스 파일은 파일 이름에 번역 문화권을 포함해야 하고 **액세스 한정자**가 **코드 생성 안 됨**으로 설정되어야 합니다. 

런타임에 애플리케이션은 특정성 순서대로 리소스 요청을 확인하려고 시도합니다. 예를 들어 디바이스 문화권이 **en-US**인 경우 애플리케이션은 다음 순서로 리소스 파일을 찾습니다.

1. AppResources.en-US.resx
1. AppResources.en.resx
1. AppResources.resx(기본값)

다음 스크린샷은 **AppResources.es.cs**라는 스페인어 번역 파일을 보여 줍니다.

![.resx 파일에 기본 텍스트 리소스 지정](text-images/pc-spanish-strings.png)

번역 파일은 기본 파일에 지정된 **이름** 값과 같은 값을 사용하지만, **값** 열에 스페인어 문자열이 포함되어 있습니다. 또한 **액세스 한정자**는 **코드 생성 안 됨**으로 설정됩니다.

::: zone-end
::: zone pivot="macos"

리소스 파일은 Mac용 Visual Studio 2019의 **새 파일 추가** 대화 상자에서 추가합니다.

![Mac용 Visual Studio 2019에서 새 리소스 추가](text-images/mac-add-resource-file.png)

기본 리소스 파일을 만든 후에는 리소스 파일의 `root` 요소 내에서 `data` 요소를 만들어 텍스트를 추가할 수 있습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
    ...
    <data name="AddButton" xml:space="preserve">
        <value>Add Note</value>
    </data>
    <data name="NotesLabel" xml:space="preserve">
        <value>Notes:</value>
    </data>
    <data name="NotesPlaceholder" xml:space="preserve">
        <value>e.g. Get Milk</value>
    </data>
</root>
```

리소스 파일 옵션에서 **사용자 지정 도구** 속성을 설정하여 **.designer.cs** 클래스 파일을 만들 수 있습니다.

![리소스 파일의 속성에 지정된 사용자 지정 도구](text-images/mac-resx-properties.png)

**사용자 지정 도구**를 **PublicResXFileCodeGenerator**로 설정하면 액세스 권한이 `public`인 클래스가 생성됩니다. **사용자 지정 도구**를 **InternalResXFileCodeGenerator**로 설정하면 액세스 권한이 `internal`인 클래스가 생성됩니다. 빈 **사용자 지정 도구** 값은 클래스를 생성하지 않습니다. 생성된 클래스 이름은 리소스 파일 이름과 일치합니다. 예를 들어 **AppResources.resx** 파일은 **AppResources.designer.cs**라는 파일에 `AppResources` 클래스를 만듭니다.

지원되는 각 문화권에 대해 추가 리소스 파일을 만들 수 있습니다. 각 언어 파일은 파일 이름에 번역 문화권을 포함해야 하므로 **es-MX**를 대상으로 하는 파일의 이름은 **AppResources.es-MX.resx**로 지정해야 합니다.

런타임에 애플리케이션은 특정성 순서대로 리소스 요청을 확인하려고 시도합니다. 예를 들어 디바이스 문화권이 **en-US**인 경우 애플리케이션은 다음 순서로 리소스 파일을 찾습니다.

1. AppResources.en-US.resx
1. AppResources.en.resx
1. AppResources.resx(기본값)

언어 번역 파일은 기본 파일로 지정된 **이름** 값과 같은 값을 가져야 합니다. 다음 XML은 **AppResources.es.resx**라는 스페인어 번역 파일을 보여 줍니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
    ...
    <data name="NotesLabel" xml:space="preserve">
        <value>Notas:</value>
    </data>
    <data name="NotesPlaceholder" xml:space="preserve">
        <value>por ejemplo . comprar leche</value>
    </data>
    <data name="AddButton" xml:space="preserve">
        <value>Agregar nuevo elemento</value>
    </data>
</root>
```

::: zone-end

## <a name="specify-the-default-culture"></a>기본 문화권 지정

리소스 파일이 제대로 작동하려면 애플리케이션에 `NeutralResourcesLanguage`가 지정되어 있어야 합니다. 공유 프로젝트에서 **AssemblyInfo.cs** 파일을 사용자 지정하여 기본 문화권을 지정해야 합니다. 다음 코드는 **AssemblyInfo.cs**파일에서 `NeutralResourcesLanguage`를 **en-US**로 설정하는 방법을 보여 줍니다.

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en-US")]
```

> [!WARNING]
> `NeutralResourcesLanguage` 특성을 지정하지 않으면 `ResourceManager` 클래스는 특정 리소스 파일이 없는 모든 문화권에 대해 `null` 값을 반환합니다. 기본 문화권이 지정된 경우 `ResourceManager`는 지원되지 않는 문화권에 대해 기본 Resx 파일의 결과를 반환합니다. 따라서 지원되지 않는 문화권에 대해 텍스트가 표시되도록 항상 `NeutralResourcesLanguage`를 지정하는 것이 좋습니다.

기본 리소스 파일이 만들어지고 **AssemblyInfo.cs** 파일에 기본 문화권이 지정된 후에는 애플리케이션이 런타임에 지역화된 문자열을 검색할 수 있습니다.

리소스 파일에 대한 자세한 내용은 [.NET 앱의 리소스 파일 만들기](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps)를 참조하세요.

## <a name="localize-text-in-xamarinforms"></a>Xamarin.Forms의 텍스트 지역화

텍스트는 생성된 `AppResources` 클래스를 사용하여 Xamarin.Forms에서 지역화됩니다. 이 클래스는 기본 리소스 파일 이름을 기반으로 이름이 지정됩니다. 샘플 프로젝트 리소스 파일의 이름이 **AppResources.cs**이므로 Visual Studio는 `AppResources`라는 일치하는 클래스를 생성합니다. 정적 속성은 리소스 파일의 각 행에 대해 `AppResources` 클래스에서 생성됩니다. 다음 정적 속성은 샘플 애플리케이션의 `AppResources` 클래스에서 생성됩니다.

- AddButton
- NotesLabel
- NotesPlaceholder

이 값에 [x:Static](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md#the-xstatic-markup-extension) 속성으로 액세스하면 지역화된 텍스트를 XAML로 표시할 수 있습니다.

```xaml
<ContentPage ...
             xmlns:resources="clr-namespace:LocalizationDemo.Resx">
    <Label Text="{x:Static resources:AppResources.NotesLabel}" />
    <Entry Placeholder="{x:Static resources:AppResources.NotesPlaceholder}" />
    <Button Text="{x:Static resources:AppResources.AddButton}" />
</ContentPage>
```

코드에서 지역화된 텍스트를 검색할 수도 있습니다.

```csharp
public LocalizedCodePage()
{
    Label notesLabel = new Label
    {
        Text = AppResources.NotesLabel,
        // ...
    };
    
    Entry notesEntry = new Entry
    {
        Placeholder = AppResources.NotesPlaceholder,
        //...
    };
    
    Button addButton = new Button
    {
        Text = AppResources.AddButton,
        // ...
    };
    
    Content = new StackLayout
    {
        Children = {
            notesLabel,
            notesEntry,
            addButton
        }
    };
}
```

`AppResources` 클래스의 속성은 `System.Globalization.CultureInfo.CurrentUICulture`의 현재 값을 사용하여 값을 검색할 문화권 리소스 파일을 결정합니다.

## <a name="localize-images"></a>이미지 지역화

Resx 파일은 텍스트를 저장할 수 있을 뿐만 아니라 이미지와 이진 데이터도 저장할 수 있습니다. 그러나 모바일 디바이스는 화면 크기와 밀도가 다양하며 각 모바일 플랫폼에는 밀도 종속 이미지 표시 기능이 있습니다. 따라서 리소스 파일에 이미지를 저장하는 대신 플랫폼 이미지 지역화 기능을 사용해야 합니다.

### <a name="localize-images-on-android"></a>Android에서 이미지 지역화

Android에서 지역화된 드로어블(이미지)은 **Resources** 디렉터리의 폴더에 대한 명명 규칙을 사용하여 저장됩니다. 폴더의 이름은 대상 언어의 접미사를 사용한 **drawable**로 지정됩니다. 예를 들어 스페인어 폴더의 이름은 **drawable-es**로 지정됩니다.

4자로 된 로캘 코드가 필요한 경우 Android에서는 대시 다음에 추가 **r**이 필요합니다. 예를 들어 멕시코 로캘(es-mx) 폴더의 이름은 **drawable-es-rMX**로 지정되어야 합니다. 각 로캘 폴더의 이미지 파일 이름은 같아야 합니다.

![Android 프로젝트의 지역화된 이미지](text-images/pc-android-images.png)

자세한 내용은 [Android 지역화](~/android/app-fundamentals/localization.md)를 참조하세요.

### <a name="localize-images-on-ios"></a>iOS에서 이미지 지역화

iOS에서 지역화된 이미지는 **Resources** 디렉터리의 폴더에 대한 명명 규칙을 사용하여 저장됩니다. 기본 폴더의 이름은 **Base.lproj**로 지정됩니다. 언어별 폴더 이름은 언어 또는 로캘 이름으로 지정되고 그 뒤에 **.lproj**가 붙습니다. 예를 들어 스페인어 폴더의 이름은 **es.lproj**로 지정됩니다.

4자로 된 로캘 코드는 2자로 된 언어 코드처럼 작동합니다. 예를 들어 멕시코 로캘(es-MX) 폴더의 이름은 **es-MX.lproj**로 지정되어야 합니다. 각 로캘 폴더의 이미지 파일 이름은 같아야 합니다.

![iOS 프로젝트의 지역화된 이미지](text-images/pc-ios-images.png)

> [!NOTE]
> iOS에서는 .lproj 폴더 구조를 사용하는 대신 지역화된 자산 카탈로그를 만들 수 있습니다. 그러나 지역화된 자산 카탈로그는 Xcode에서 만들고 관리해야 합니다.

자세한 내용은 [iOS 지역화](~/ios/app-fundamentals/localization/index.md)를 참조하세요.

### <a name="localize-images-on-uwp"></a>UWP에서 이미지 지역화

UWP에서 지역화된 이미지는 **Assets/Images** 디렉터리의 폴더에 대한 명명 규칙을 사용하여 저장됩니다. 폴더의 이름은 언어 또는 로캘을 사용하여 지정됩니다. 예를 들어 스페인어 폴더의 이름은 **es**로 지정되며 멕시코 로캘 폴더의 이름은 **es-MX**로 지정되어야 합니다. 각 로캘 폴더의 이미지 파일 이름은 같아야 합니다.

![UWP 프로젝트의 지역화된 이미지](text-images/pc-uwp-images.png)

자세한 내용은 [UWP 지역화](/windows/uwp/design/globalizing/globalizing-portal/)를 참조하세요.

### <a name="consume-localized-images"></a>지역화된 이미지 사용

각 플랫폼은 고유한 파일 구조를 사용하여 이미지를 저장하므로 XAML에서는 `OnPlatform` 클래스를 사용하여 현재 플랫폼에 따라 `ImageSource` 속성을 설정합니다.

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

> [!NOTE]
> `OnPlatform` 태그 확장은 플랫폼별 값을 지정하는 것보다 간결한 방법을 제공합니다. 자세한 내용은 [OnPlatform 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension)을 참조하세요.

이미지 소스는 코드에서 `Device.RuntimePlatform` 속성에 따라 설정할 수 있습니다.

```csharp
string imgSrc = Device.RuntimePlatform == Device.UWP ? "Assets/Images/flag.png" : "flag.png";
Image flag = new Image
{
    Source = ImageSource.FromFile(imgSrc),
    WidthRequest = 100
};
```

## <a name="localize-the-application-name"></a>애플리케이션 이름 지역화

애플리케이션 이름은 플랫폼별로 지정되며 Resx 리소스 파일을 사용하지 않습니다. Android에서 애플리케이션 이름을 지역화하려면 [Android에서 앱 이름 지역화](~/android/app-fundamentals/localization.md#stringsxml-file-format)를 참조하세요. iOS에서 애플리케이션 이름을 지역화하려면 [iOS에서 앱 이름 지역화](~/ios/app-fundamentals/localization/index.md#app-name)를 참조하세요. UWP에서 애플리케이션 이름을 지역화하려면 [UWP 패키지 매니페스트의 문자열 지역화](https://docs.microsoft.com/windows/uwp/app-resources/localize-strings-ui-manifest)를 참조하세요.

## <a name="test-localization"></a>지역화 테스트

지역화 테스트는 디바이스 언어를 변경하여 수행하는 것이 가장 좋습니다. 코드에서 `System.Globalization.CultureInfo.CurrentUICulture` 값을 설정할 수 있지만, 플랫폼 간에 동작이 일관되지 않으므로 테스트에는 권장되지 않습니다.

iOS의 경우 특별히 디바이스 언어를 변경하지 않고 설정 앱에서 각 앱의 언어를 설정할 수 있습니다.

Android의 경우 애플리케이션이 시작될 때 언어 설정이 감지되고 캐시됩니다. 언어를 변경하는 경우 변경 내용이 적용된 모습을 보려면 애플리케이션을 종료한 후 다시 시작해야 할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [지역화 샘플 프로젝트](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/usingresxlocalization)
- [.NET 앱의 리소스 파일 만들기](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps)
- [플랫폼 간 지역화](~/cross-platform/app-fundamentals/localization.md)
- [CultureInfo 클래스 사용(MSDN)](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo)
- [Android 지역화](~/android/app-fundamentals/localization.md)
- [iOS 지역화](~/ios/app-fundamentals/localization/index.md)
- [UWP 지역화](/windows/uwp/design/globalizing/globalizing-portal/)
- [특정 문화권에 대한 리소스 찾기 및 사용(MSDN)](https://msdn.microsoft.com/library/s9ckwb4b%28v=vs.90%29.aspx)
