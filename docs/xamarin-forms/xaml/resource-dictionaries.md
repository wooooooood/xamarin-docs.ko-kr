---
title: Xamarin.Forms리소스 사전
description: Xamarin.FormsXAML 리소스는 응용 프로그램 전체에서 공유 하 고 다시 사용할 수 있는 개체입니다 Xamarin.Forms .
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.custom: video
ms.openlocfilehash: a1c7cfd4a0f3549b11ac51dc13b40da552f6b758
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139414"
---
# <a name="xamarinforms-resource-dictionaries"></a>Xamarin.Forms리소스 사전

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)

는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 응용 프로그램에서 사용 되는 리소스에 대 한 리포지토리입니다 Xamarin.Forms . 에 저장 되는 일반적인 리소스에 `ResourceDictionary` 는 [스타일](~/xamarin-forms/user-interface/styles/index.md), [컨트롤 템플릿](~/xamarin-forms/app-fundamentals/templates/control-template.md), [데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), 색 및 변환기가 포함 됩니다.

XAML에서 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) `StaticResource` 또는 태그 확장을 사용 하 여에 저장 된 리소스를 참조 하 고 요소에 적용할 수 있습니다 `DynamicResource` . C #에서 리소스는에서 정의 된 `ResourceDictionary` 다음 문자열 기반 인덱서를 사용 하 여 참조 되 고 요소에 적용 될 수도 있습니다. 그러나 `ResourceDictionary` 공유 개체를 필드 또는 속성으로 저장 하 고 사전에서 먼저 검색 하지 않고도 직접 액세스할 수 있으므로 c #에서를 사용 하는 경우에는 약간의 이점이 있습니다.

## <a name="create-resources-in-xaml"></a>XAML에서 리소스 만들기

모든 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 파생 개체에는 [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 리소스를 포함할 수 있는 인 속성이 있습니다. 마찬가지로, [`Application`](xref:Xamarin.Forms.Application) 파생 된 개체에는 [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) 리소스를 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 포함할 수 있는 인 속성이 있습니다.

Xamarin.Forms응용 프로그램에는에서 파생 된 클래스만 포함 [`Application`](xref:Xamarin.Forms.Application) 되지만 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 페이지, 레이아웃 및 컨트롤을 비롯 하 여에서 파생 되는 많은 클래스를 종종 사용 합니다. 이러한 개체는 해당 `Resources` 속성을 포함 하는 리소스로 설정할 수 있습니다 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . `ResourceDictionary`리소스를 사용할 수 있는 특정 영향을 줄 수 있는 위치 선택:

- `ResourceDictionary`또는와 같은 뷰에 연결 된의 리소스는 [`Button`](xref:Xamarin.Forms.Button) [`Label`](xref:Xamarin.Forms.Label) 해당 특정 개체에만 적용 될 수 있습니다.
- `ResourceDictionary`또는와 같은 레이아웃에 연결 된의 리소스 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 는 [`Grid`](xref:Xamarin.Forms.Grid) 레이아웃 및 해당 레이아웃의 모든 자식에 적용 될 수 있습니다.
- `ResourceDictionary`페이지 수준에서 정의 된의 리소스는 페이지와 모든 해당 자식에 적용 될 수 있습니다.
- 응용 프로그램 수준에서 정의 된의 리소스는 `ResourceDictionary` 응용 프로그램 전체에 적용 될 수 있습니다.

암시적 스타일을 제외 하 고 리소스 사전의 각 리소스에는 특성으로 정의 된 고유한 문자열 키가 있어야 합니다 `x:Key` .

다음 XAML은 app.xaml 파일의 응용 프로그램 수준에서 정의 된 리소스를 보여 줍니다 `ResourceDictionary` . **App.xaml**

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

이 예제에서 리소스 사전은 리소스 [`Thickness`](xref:Xamarin.Forms.Thickness) , 여러 [`Color`](xref:Xamarin.Forms.Color) 리소스 및 두 개의 암시적 리소스를 정의 합니다 [`Style`](xref:Xamarin.Forms.Style) . 클래스에 대 한 자세한 내용은 `App` [ Xamarin.Forms App 클래스](~/xamarin-forms/app-fundamentals/application-class.md)를 참조 하세요.

> [!NOTE]
> 명시적 태그 사이에 모든 리소스를 저장 하는 것도 유효 `ResourceDictionary` 합니다. 그러나 3.0 이므로 Xamarin.Forms `ResourceDictionary` 태그가 필요 하지 않습니다. 대신 `ResourceDictionary` 개체가 자동으로 만들어지고 `Resources` 속성-요소 태그 사이에 직접 리소스를 삽입할 수 있습니다.

## <a name="consume-resources-in-xaml"></a>XAML에서 리소스 사용

각 리소스에 `x:Key` 는의 사전 키가 되는 특성을 사용 하 여 지정 된 키가 있습니다 `ResourceDictionary` . 키는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) 또는 태그 확장을 사용 하 여에서 리소스를 참조 하는 데 사용 됩니다 [`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension) .

`StaticResource`태그 확장은 `DynamicResource` 둘 다 사전 키를 사용 하 여 리소스 사전의 값을 참조 한다는 점에서 태그 확장과 비슷합니다. 그러나 `StaticResource` 태그 확장은 단일 사전 조회를 수행 하지만 `DynamicResource` 태그 확장은 사전 키에 대 한 링크를 유지 관리 합니다. 따라서 키와 연결 된 사전 항목이 대체 되 면 변경 내용이 시각적 요소에 적용 됩니다. 이렇게 하면 응용 프로그램에서 런타임 리소스를 변경할 수 있습니다. 태그 확장에 대 한 자세한 내용은 [XAML 태그 확장](~/xamarin-forms/xaml/markup-extensions/index.md)을 참조 하세요.

다음 XAML 예제에서는 리소스를 사용 하는 방법과에서 추가 리소스를 정의 하는 방법을 보여 줍니다 [`StackLayout`](xref:Xamarin.Forms.StackLayout) .

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

이 예제에서 개체는 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 응용 프로그램 수준 리소스 사전에 정의 된 암시적 스타일을 사용 합니다. [`StackLayout`](xref:Xamarin.Forms.StackLayout)개체는 `PageMargin` 응용 프로그램 수준 리소스 사전에 정의 된 리소스를 사용 하는 반면 [`Button`](xref:Xamarin.Forms.Button) 개체는 리소스 사전에 정의 된 암시적 스타일을 사용 합니다 [`StackLayout`](xref:Xamarin.Forms.StackLayout) . 이로 인해 결국 다음 스크린샷에 표시된 모양이 됩니다.

[![ResourceDictionary 리소스 소비](resource-dictionaries-images/consuming.png "ResourceDictionary 리소스 소비")](resource-dictionaries-images/consuming-large.png#lightbox "ResourceDictionary 리소스 소비")

> [!IMPORTANT]
> 단일 페이지에 관련 된 리소스는 응용 프로그램 수준 리소스 사전에 포함 될 수 없습니다. 이러한 리소스는 페이지에 필요한 경우 대신 응용 프로그램 시작 시 구문 분석 됩니다. 자세한 내용은 [응용 프로그램 리소스 사전 크기 줄이기](~/xamarin-forms/deploy-test/performance.md)를 참조 하세요.

## <a name="resource-lookup-behavior"></a>리소스 조회 동작

다음 조회 프로세스는 [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) 또는 태그 확장을 사용 하 여 리소스를 참조할 때 발생 합니다 [`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension) .

- 요청 된 키는 속성을 설정 하는 요소에 대 한 리소스 사전 (있는 경우)에서 확인 됩니다. 요청 된 키가 발견 되 면 해당 값이 반환 되 고 조회 프로세스가 종료 됩니다.
- 일치 항목을 찾을 수 없는 경우 조회 프로세스는 시각적 트리를 위쪽으로 검색 하 여 각 부모 요소의 리소스 사전을 검사 합니다. 요청 된 키가 발견 되 면 해당 값이 반환 되 고 조회 프로세스가 종료 됩니다. 그렇지 않으면 루트 요소에 도달할 때까지 프로세스가 계속 됩니다.
- 루트 요소에 일치 하는 항목이 없으면 응용 프로그램 수준 리소스 사전을 검사 합니다.
- 일치 하는 항목을 찾을 수 없으면 `XamlParseException` 이 throw 됩니다.

따라서 XAML 파서는 `StaticResource` 또는 태그 확장을 발견할 경우 `DynamicResource` 발견 된 첫 번째 일치 항목을 사용 하 여 시각적 트리를 통해 이동 하 여 일치 하는 키를 검색 합니다. 이 검색이 페이지에서 종료 되 고 키를 찾지 못한 경우 XAML 파서는 개체에 연결 된을 검색 합니다 `ResourceDictionary` `App` . 키가 아직 없으면 예외가 throw 됩니다.

## <a name="override-resources"></a>리소스 재정의

리소스에서 키를 공유 하는 경우 시각적 트리에 정의 된 리소스가 상위 수준에서 정의 된 것 보다 우선적으로 적용 됩니다. 예를 들어 `AppBackgroundColor` 응용 프로그램 수준에서로 리소스를 설정 하는 것 `AliceBlue` 은로 설정 된 페이지 수준 리소스에 의해 재정의 됩니다 `AppBackgroundColor` `Teal` . 마찬가지로 페이지 수준 리소스는 `AppBackgroundColor` 컨트롤 수준 리소스에 의해 재정의 됩니다 `AppBackgroundColor` .

## <a name="stand-alone-resource-dictionaries"></a>독립 실행형 리소스 사전

에서 파생 된 클래스는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 별도의 독립 실행형 파일에 있을 수도 있습니다. 그러면 결과 파일을 응용 프로그램 간에 공유할 수 있습니다.

이러한 파일을 만들려면 프로젝트에 새 **콘텐츠 뷰** 또는 **콘텐츠 페이지** 항목을 추가 합니다 (c # 파일만 있는 **콘텐츠 뷰** 또는 **콘텐츠 페이지** 는 아님). 코드 숨겨진 파일을 삭제 하 고 XAML 파일에서 기본 클래스의 이름을 또는에서로 변경 합니다 [`ContentView`](xref:Xamarin.Forms.ContentView) [`ContentPage`](xref:Xamarin.Forms.ContentPage) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . 또한 `x:Class` 파일의 루트 태그에서 특성을 제거 합니다.

다음 XAML 예제에서는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 이름이 **myresourcedictionary .xaml**인를 보여 줍니다.

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

이 예제에서에는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 형식의 개체인 단일 리소스가 포함 되어 있습니다 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . **Myresourcedictionary. xaml** 을 다른 리소스 사전에 병합 하 여 사용할 수 있습니다.

## <a name="merged-resource-dictionaries"></a>병합된 리소스 사전

병합 된 리소스 사전은 하나 이상의 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 개체를 다른 개체에 결합 `ResourceDictionary` 합니다.

### <a name="merge-local-resource-dictionaries"></a>로컬 리소스 사전 병합

[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)해당 속성을 리소스를 사용 하는 `ResourceDictionary` `ResourceDictionary` [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) XAML 파일의 파일 이름으로 설정 하는 개체를 만들어 로컬 파일을 다른 파일에 병합할 수 있습니다.

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

이 구문은 클래스를 인스턴스화하지 않습니다 `MyResourceDictionary` . 대신 XAML 파일을 참조 합니다. 이러한 이유로 속성을 설정할 때 [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) 코드 숨김이 필요 하지 않으며, `x:Class` **myresourcedictionary .xaml** 파일의 루트 태그에서 특성을 제거할 수 있습니다.

> [!IMPORTANT]
> [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source)속성은 XAML 에서만 설정할 수 있습니다.

### <a name="merge-resource-dictionaries-from-other-assemblies"></a>다른 어셈블리의 리소스 사전 병합

는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) `ResourceDictionary` 의 속성에 추가 하 여 다른로 병합할 수도 있습니다 [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) `ResourceDictionary` . 이 기법을 사용 하면 리소스가 상주 하는 어셈블리에 관계 없이 리소스 사전을 병합할 수 있습니다. 외부 어셈블리에서 리소스 사전을 병합 하려면에 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 빌드 작업을 **EmbeddedResource**로 설정 하 고, 코드를 사용 하 여 파일을 포함 하 고, `x:Class` 파일의 루트 태그에 특성을 정의 해야 합니다.

> [!WARNING]
> [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)또한 클래스는 속성을 정의 [`MergedWith`](xref:Xamarin.Forms.ResourceDictionary.MergedWith) 합니다. 그러나이 속성은 더 이상 사용 되지 않으므로 사용 하면 안 됩니다.

다음 코드 예제에서는 두 개의 리소스 사전을 페이지 수준의 컬렉션에 추가 하는 방법을 보여 줍니다 [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) .

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

이 예제에서는 동일한 어셈블리의 리소스 사전과 외부 어셈블리의 리소스 사전이 페이지 수준 리소스 사전으로 병합 됩니다. 또한 `ResourceDictionary` [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) 속성-요소 태그 내의 다른 개체와 이러한 태그 외부의 기타 리소스를 추가할 수도 있습니다.

> [!IMPORTANT]
> `MergedDictionaries`에는 속성 요소 태그가 하나만 있을 수 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 있지만 필요한 만큼 개체를 포함할 수 있습니다 `ResourceDictionary` .

병합 된 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 리소스가 동일한 특성 값을 공유 하는 경우는 `x:Key` Xamarin.Forms 다음과 같은 리소스 우선 순위를 사용 합니다.

1. 리소스 사전에 대 한 로컬 리소스입니다.
1. 컬렉션을 통해 병합 된 리소스 사전에 포함 된 리소스는 `MergedDictionaries` 역순으로 속성에 나열 됩니다 `MergedDictionaries` .

> [!NOTE]
> 응용 프로그램에 여러 개의 많은 리소스 사전이 포함 된 경우 리소스 사전을 검색 하는 작업은 계산 집약적인 작업이 될 수 있습니다. 따라서 불필요 한 검색을 방지 하려면 응용 프로그램의 각 페이지에서 해당 페이지에 적절 한 리소스 사전만 사용 하도록 해야 합니다.

## <a name="related-links"></a>관련 링크

- [리소스 사전 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)
- [XAML 태그 확장](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Xamarin.Forms 스타일](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Application-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
