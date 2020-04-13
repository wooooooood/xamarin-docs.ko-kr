---
title: Xamarin.Forms 리소스 사전
description: Xamarin.Forms XAML 리소스는 Xamarin.Forms 응용 프로그램 전체에서 공유하고 다시 사용할 수 있는 개체입니다.
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2020
ms.custom: video
ms.openlocfilehash: 8dd3c7f36ddd436a812927816a1326dbb7c48341
ms.sourcegitcommit: ee9e48e2ec643915f42a6c1641077970ae20cb17
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80523204"
---
# <a name="xamarinforms-resource-dictionaries"></a>Xamarin.Forms 리소스 사전

[![샘플](~/media/shared/download.png) 다운로드 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)

A는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) Xamarin.Forms 응용 프로그램에서 사용하는 리소스에 대한 리포지토리입니다. [스타일,](~/xamarin-forms/user-interface/styles/index.md) [제어 템플릿,](~/xamarin-forms/app-fundamentals/templates/control-template.md) [데이터 템플릿,](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)색상 및 변환기를 `ResourceDictionary` 포함합니다.

XAML에서 에 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 저장된 리소스는 `StaticResource` 또는 `DynamicResource` 태그 확장을 사용하여 요소에 참조하고 적용할 수 있습니다. C#에서 리소스는 문자열 기반 `ResourceDictionary` 인덱서를 사용하여 요소에 참조되고 적용될 수도 있습니다. 그러나 공유 개체를 필드 또는 `ResourceDictionary` 속성으로 저장하고 사전에서 먼저 검색할 필요 없이 직접 액세스할 수 있기 때문에 C#에서 를 사용하는 데는 거의 이점이 없습니다.

## <a name="create-resources-in-xaml"></a>XAML에서 리소스 만들기

모든 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 파생 개체에는 [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) 리소스를 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 포함할 수 있는 속성이 있습니다. 마찬가지로 [`Application`](xref:Xamarin.Forms.Application) 파생 개체에는 리소스를 [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) 포함할 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 수 있는 속성이 있습니다.

Xamarin.Forms 응용 프로그램에는 [`Application`](xref:Xamarin.Forms.Application)에서 파생되는 클래스만 포함하지만 페이지, [`VisualElement`](xref:Xamarin.Forms.VisualElement)레이아웃 및 컨트롤을 포함하여 파생되는 많은 클래스를 사용하는 경우가 많습니다. 이러한 개체는 해당 `Resources` 속성을 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 포함된 리소스로 설정할 수 있습니다. 리소스를 사용할 수 `ResourceDictionary` 있는 특정 영향을 넣을 위치를 선택합니다.

- 뷰에 `ResourceDictionary` [`Button`](xref:Xamarin.Forms.Button) 첨부되거나 [`Label`](xref:Xamarin.Forms.Label) 특정 개체에만 적용할 수 있는 리소스입니다.
- 레이아웃에 `ResourceDictionary` 첨부된 리소스 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 또는 [`Grid`](xref:Xamarin.Forms.Grid) 레이아웃및 해당 레이아웃의 모든 자식에 적용할 수 있습니다.
- 페이지 수준에서 `ResourceDictionary` 정의된 리소스는 페이지와 모든 자식에 적용할 수 있습니다.
- 응용 프로그램 `ResourceDictionary` 수준에서 정의된 리소스는 응용 프로그램 전체에 적용할 수 있습니다.

암시적 스타일을 제외하고 리소스 사전의 각 리소스에는 `x:Key` 특성으로 정의된 고유한 문자열 키가 있어야 합니다.

다음 XAML은 `ResourceDictionary` **App.xaml** 파일의 응용 프로그램 수준에 정의된 리소스를 보여 주며 다음과 같이 표시됩니다.

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ResourceDictionaryDemo.App">
    <Application.Resources>

        <Thickness x:Key="PageMargin">20</Thickness>

        <!-- Colors -->
        <Color x:Key="AppBackgroundColor">AliceBlue</Color>
        <Color x:Key="NavigationBarColor">#1976D2</Color>
        <Color x:Key="NavigationBarTextColor">White</Color>
        <Color x:Key="NormalTextColor">Black</Color>

        <!-- Implicit styles -->
        <Style TargetType="{x:Type NavigationPage}">
            <Setter Property="BarBackgroundColor"
                    Value="{StaticResource NavigationBarColor}" />
            <Setter Property="BarTextColor"
                    Value="{StaticResource NavigationBarTextColor}" />
        </Style>

        <Style TargetType="{x:Type ContentPage}"
               ApplyToDerivedTypes="True">
            <Setter Property="BackgroundColor"
                    Value="{StaticResource AppBackgroundColor}" />
        </Style>

    </Application.Resources>
</Application>
```

이 예제에서 리소스 사전은 [`Thickness`](xref:Xamarin.Forms.Thickness) 리소스, [`Color`](xref:Xamarin.Forms.Color) 여러 리소스 및 [`Style`](xref:Xamarin.Forms.Style) 두 개의 암시적 리소스를 정의합니다. 클래스에 `App` 대한 자세한 내용은 [Xamarin.Forms 앱 클래스를](~/xamarin-forms/app-fundamentals/application-class.md)참조하십시오.

> [!NOTE]
> 명시적 `ResourceDictionary` 태그 사이에 모든 리소스를 배치하는 것도 유효합니다. 그러나 Xamarin.Forms 3.0 `ResourceDictionary` 이후 태그는 필요하지 않습니다. 대신 개체가 `ResourceDictionary` 자동으로 만들어지며 속성 요소 태그 사이에 `Resources` 리소스를 직접 삽입할 수 있습니다.

## <a name="consume-resources-in-xaml"></a>XAML의 리소스 소비

각 리소스에는 `x:Key` 에서 사전 키가 되는 특성을 사용하여 지정된 `ResourceDictionary`키가 있습니다. 키는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) 또는 [`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension) 태그 확장에서 리소스를 참조 하는 데 사용 됩니다.

`StaticResource` 태그 확장은 둘 다 `DynamicResource` 사전 키를 사용하여 리소스 사전의 값을 참조한다는 점에서 태그 확장과 유사합니다. 그러나 태그 `StaticResource` 확장단일 사전 조회를 수행 하지만 `DynamicResource` 태그 확장은 사전 키에 대 한 링크를 유지 합니다. 따라서 키와 연결된 사전 항목이 대체되면 변경 이 시각적 요소에 적용됩니다. 이렇게 하면 응용 프로그램에서 런타임 리소스를 변경할 수 있습니다. 마크업 확장에 대한 자세한 내용은 [XAML 마크업 확장 을](~/xamarin-forms/xaml/markup-extensions/index.md)참조하십시오.

다음 XAML 예제에서는 리소스를 사용하는 방법을 보여 주며 [`StackLayout`](xref:Xamarin.Forms.StackLayout)다음 의 추가 리소스를 정의합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ResourceDictionaryDemo.HomePage"
             Title="Home Page">
    <StackLayout Margin="{StaticResource PageMargin}">
        <StackLayout.Resources>
            <!-- Implicit style -->
            <Style TargetType="Button">
                <Setter Property="FontSize" Value="Medium" />
                <Setter Property="BackgroundColor" Value="#1976D2" />
                <Setter Property="TextColor" Value="White" />
                <Setter Property="CornerRadius" Value="5" />
            </Style>
        </StackLayout.Resources>

        <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries." />
        <Button Text="Navigate"
                Clicked="OnNavigateButtonClicked" />
    </StackLayout>
</ContentPage>
```

이 예제에서 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체는 응용 프로그램 수준 리소스 사전에 정의된 암시적 스타일을 사용합니다. 개체는 응용 [`StackLayout`](xref:Xamarin.Forms.StackLayout) `PageMargin` 프로그램 수준 리소스 사전에 정의된 리소스를 사용하고 [`Button`](xref:Xamarin.Forms.Button) 개체는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 리소스 사전에 정의된 암시적 스타일을 사용합니다. 이로 인해 결국 다음 스크린샷에 표시된 모양이 됩니다.

[![리소스사전 리소스 사용](resource-dictionaries-images/consuming.png "리소스사전 리소스 사용")](resource-dictionaries-images/consuming-large.png#lightbox "리소스사전 리소스 사용")

> [!IMPORTANT]
> 단일 페이지와 관련된 리소스는 응용 프로그램 수준 리소스 사전에 포함되지 않아야 합니다. 자세한 내용은 [응용 프로그램 리소스 사전 크기 축소를](~/xamarin-forms/deploy-test/performance.md)참조하십시오.

## <a name="resource-lookup-behavior"></a>리소스 조회 동작

다음 조회 프로세스는 리소스또는 [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) [`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension) 태그 확장과 함께 참조할 때 발생합니다.

- 요청된 키는 속성을 설정하는 요소에 대해 리소스 사전(있는 경우)에서 확인됩니다. 요청된 키가 발견되면 해당 값이 반환되고 조회 프로세스가 종료됩니다.
- 일치하는 개체수를 찾을 수 없는 경우 조회 프로세스는 시각적 트리를 위쪽으로 검색하여 각 상위 요소의 리소스 사전을 확인합니다. 요청된 키가 발견되면 해당 값이 반환되고 조회 프로세스가 종료됩니다. 그렇지 않으면 루트 요소에 도달할 때까지 프로세스가 위쪽으로 계속됩니다.
- 루트 요소에서 일치하는 일치를 찾을 수 없는 경우 응용 프로그램 수준 리소스 사전이 검사됩니다.
- 그래도 매치를 찾지 못하면 A가 `XamlParseException` throw됩니다.

따라서 XAML 파서가 또는 `StaticResource` `DynamicResource` 마크업 확장이 발생하면 시각적 트리를 통해 이동하여 일치하는 키를 검색하고 찾은 첫 번째 일치 를 사용합니다. 이 검색이 페이지에서 끝나고 키가 아직 발견되지 않은 경우 XAML 파서는 `ResourceDictionary` `App` 개체에 첨부된 파서를 검색합니다. 키가 아직 발견되지 않으면 예외가 throw됩니다.

## <a name="override-resources"></a>리소스 재정의

리소스가 키를 공유하는 경우 시각적 트리에서 아래쪽에 정의된 리소스가 더 높은 순위로 정의된 리소스보다 우선합니다. 예를 들어 응용 `AppBackgroundColor` 프로그램 `AliceBlue` 수준에서 리소스를 설정하는 것은 로 설정된 `AppBackgroundColor` 페이지 `Teal`수준 리소스에 의해 재정의됩니다. 마찬가지로 페이지 수준 `AppBackgroundColor` 리소스는 제어 수준 `AppBackgroundColor` 리소스에 의해 재정의됩니다.

## <a name="stand-alone-resource-dictionaries"></a>독립 실행형 리소스 사전

파생된 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 클래스는 별도의 독립 실행형 파일에 있을 수도 있습니다. 그런 다음 결과 파일을 응용 프로그램 간에 공유할 수 있습니다.

이러한 파일을 만들려면 프로젝트에 새 **콘텐츠 보기** 또는 **콘텐츠 페이지** 항목을 추가합니다(C# 파일만 있는 콘텐츠 **보기** 또는 **콘텐츠 페이지는** 아님). 코드 숨는 파일을 삭제하고 XAML 파일에서 기본 클래스의 [`ContentView`](xref:Xamarin.Forms.ContentView) 이름을 [`ContentPage`](xref:Xamarin.Forms.ContentPage) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에서 또는 로 변경합니다. 또한 파일의 `x:Class` 루트 태그에서 특성을 제거합니다.

다음 XAML 예제에서는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) **MyResourceDictionary.xaml이라는**이름을 보여 주며 다음과 같은 이름을 보여 주며 다음과 같은 이름을 보여 주며 다음과 같은 이름을 보여 주며 다음과 같은 이름을 보여 주며 다음과 같은 이름을 보여 주며 다음과 같은 이름을 보여 주

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml">
    <DataTemplate x:Key="PersonDataTemplate">
        <ViewCell>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.5*" />
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.3*" />
                </Grid.ColumnDefinitions>
                <Label Text="{Binding Name}"
                       TextColor="{StaticResource NormalTextColor}"
                       FontAttributes="Bold" />
                <Label Grid.Column="1"
                       Text="{Binding Age}"
                       TextColor="{StaticResource NormalTextColor}" />
                <Label Grid.Column="2"
                       Text="{Binding Location}"
                       TextColor="{StaticResource NormalTextColor}"
                       HorizontalTextAlignment="End" />
            </Grid>
        </ViewCell>
    </DataTemplate>
</ResourceDictionary>
```

이 예제에서는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 형식의 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)개체인 단일 리소스가 포함되어 있습니다. **MyResourceDictionary.xaml은** 다른 리소스 사전에 병합하여 사용할 수 있습니다.

## <a name="merged-resource-dictionaries"></a>병합된 리소스 사전

병합된 리소스 사전은 하나 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 이상의 개체를 다른 `ResourceDictionary`개체로 결합합니다.

### <a name="merge-local-resource-dictionaries"></a>로컬 리소스 사전 병합

속성이 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 리소스가 있는 [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) XAML `ResourceDictionary` 파일의 파일 이름으로 설정된 개체를 만들어 로컬 파일을 다른 `ResourceDictionary` 파일로 병합할 수 있습니다.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <!-- Add more resources here -->
        <ResourceDictionary Source="MyResourceDictionary.xaml" />
        <!-- Add more resources here -->
    </ContentPage.Resources>
    ...
</ContentPage>
```

이 구문은 클래스를 `MyResourceDictionary` 인스턴스화하지 않습니다. 대신 XAML 파일을 참조합니다. 따라서 [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) 속성을 설정할 때 코드 숨김 파일이 필요하지 않으며 `x:Class` **MyResourceDictionary.xaml** 파일의 루트 태그에서 특성을 제거할 수 있습니다.

> [!IMPORTANT]
> 숙소는 [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) XAML에서만 설정할 수 있습니다.

### <a name="merge-resource-dictionaries-from-other-assemblies"></a>다른 어셈블리에서 리소스 사전 병합

A를 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 의 `ResourceDictionary` [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) 속성에 추가하여 다른 로 병합할 `ResourceDictionary`수도 있습니다. 이 기술을 사용하면 리소스 사전이 있는 어셈블리에 관계없이 병합할 수 있습니다. 외부 어셈블리에서 리소스 사전을 병합하려면 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 빌드 작업을 **EmbeddedResource로**설정하고 코드 숨김 파일을 갖고 파일의 루트 태그에서 `x:Class` 특성을 정의해야 합니다.

> [!WARNING]
> 클래스는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [`MergedWith`](xref:Xamarin.Forms.ResourceDictionary.MergedWith) 속성도 정의합니다. 그러나 이 속성은 더 이상 사용되지 않으며 더 이상 사용해서는 안 됩니다.

다음 코드 예제에서는 페이지 수준 [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)컬렉션에 추가되는 두 가지 리소스 사전을 보여 주십니다.

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:ResourceDictionaryDemo"
             xmlns:theme="clr-namespace:MyThemes;assembly=MyThemes">
    <ContentPage.Resources>
        <ResourceDictionary>
            <!-- Add more resources here -->
            <ResourceDictionary.MergedDictionaries>
                <!-- Add more resource dictionaries here -->
                <local:MyResourceDictionary />
                <theme:LightTheme />
                <!-- Add more resource dictionaries here -->
            </ResourceDictionary.MergedDictionaries>
            <!-- Add more resources here -->
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

이 예제에서는 동일한 어셈블리의 리소스 사전과 외부 어셈블리의 리소스 사전이 페이지 수준 리소스 사전에 병합됩니다. 또한 [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) 속성 요소 태그 `ResourceDictionary` 내에 다른 개체와 해당 태그 외부의 다른 리소스를 추가할 수도 있습니다.

> [!IMPORTANT]
> 에 속성 요소 `MergedDictionaries` 태그가 하나만 있을 수 있지만 `ResourceDictionary` 필요에 따라 많은 개체를 넣을 수 있습니다. [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)

병합된 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 리소스가 `x:Key` 동일한 특성 값을 공유하는 경우 Xamarin.Forms는 다음 리소스 우선 순위를 사용합니다.

1. 리소스 사전에 로컬리소스입니다.
1. `MergedDictionaries` 컬렉션을 통해 병합된 리소스 사전에 포함된 리소스는 `MergedDictionaries` 속성에 나열된 역순으로 표시됩니다.

> [!NOTE]
> 응용 프로그램에 여러 개의 큰 리소스 사전이 포함된 경우 리소스 사전을 검색하는 것은 계산집약적인 작업일 수 있습니다. 따라서 불필요한 검색을 방지하려면 응용 프로그램의 각 페이지가 페이지에 적합한 리소스 사전만 사용해야 합니다.

## <a name="related-links"></a>관련 링크

- [리소스 사전(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)
- [XAML 마크업 확장](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Xamarin.Forms 스타일](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Application-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
