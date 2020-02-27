---
title: 리소스 사전
description: XAML 리소스는 공유 하 고 Xamarin.Forms 응용 프로그램 전체에서 다시 사용할 수 있는 개체입니다.
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2020
ms.custom: video
ms.openlocfilehash: f5266049e1498d902864185a61cba6d3730f9594
ms.sourcegitcommit: 2836f2003a5b745b042ee6003a3d6a11b9139e44
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77618867"
---
# <a name="resource-dictionaries"></a>리소스 사전

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)

_XAML 리소스는 Xamarin.ios 응용 프로그램 전체에서 공유 하 고 다시 사용할 수 있는 개체의 정의입니다. 이러한 리소스 개체는 리소스 사전에 저장 됩니다._

[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 은 xamarin.ios 응용 프로그램에서 사용 되는 리소스에 대 한 리포지토리입니다. `ResourceDictionary`에 저장 되는 일반적인 리소스에는 [스타일](~/xamarin-forms/user-interface/styles/index.md), [컨트롤 템플릿](~/xamarin-forms/app-fundamentals/templates/control-template.md), [데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), 색 및 변환기가 포함 됩니다.

XAML에서는 `StaticResource` 태그 확장을 사용 하 여 `ResourceDictionary`에 저장 된 리소스를 검색 하 고 요소에 적용할 수 있습니다. 에서 C#리소스를 `ResourceDictionary` 정의한 다음 문자열 기반 인덱서를 사용 하 여 요소에 검색 하 고 적용할 수 있습니다. 그러나 공유 개체를 단순히 필드 또는 속성으로 저장 하 C#고 사전에서 먼저 검색 하지 않고도 직접 액세스할 수 있으므로에서 `ResourceDictionary`를 사용 하는 경우에는 약간의 이점이 있습니다.

## <a name="create-and-consume-a-resourcedictionary"></a>ResourceDictionary 만들기 및 사용

리소스는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 에서 정의 되 고 다음 `Resources` 속성 중 하나로 설정 됩니다.

- 에서 파생 되는 클래스의 [`Resources`](xref:Xamarin.Forms.Application.Resources) 속성 [`Application`](xref:Xamarin.Forms.Application)
- 에서 파생 되는 클래스의 [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) 속성 [`VisualElement`](xref:Xamarin.Forms.Application)

Xamarin.ios 프로그램은 `Application`에서 파생 되는 클래스를 하나만 포함 하지만 페이지, 레이아웃 및 컨트롤을 비롯 하 여 `VisualElement`에서 파생 되는 많은 클래스를 종종 사용 합니다. 이러한 개체는 `Resources` 속성을 `ResourceDictionary`로 설정할 수 있습니다. 특정 `ResourceDictionary`를 배치할 수 있는 위치를 선택 하 여 리소스를 사용할 수 있는 위치에 영향을 줍니다.

- `Button` 또는 `Label`와 같이 뷰에 연결 된 `ResourceDictionary`의 리소스는 특정 개체에만 적용 될 수 있으므로이는 그다지 유용 하지 않습니다.
- `StackLayout` 또는 `Grid`와 같은 레이아웃에 연결 된 `ResourceDictionary`의 리소스는 레이아웃 및 해당 레이아웃의 모든 자식에 적용 될 수 있습니다.
- 페이지 수준에서 정의 된 `ResourceDictionary`의 리소스를 페이지 및 모든 해당 자식에 적용할 수 있습니다.
- 응용 프로그램 수준에서 정의 된 `ResourceDictionary` 리소스는 응용 프로그램 전체에 적용 될 수 있습니다.

다음 XAML은 표준 Xamarin.ios 프로그램의 일부로 **생성 된 app.xaml 파일에** `ResourceDictionary` 응용 프로그램 수준에서 정의 된 리소스를 보여 줍니다.

```xaml
<Application ...>
    <Application.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Yellow</Color>
            <Color x:Key="HeadingTextColor">Black</Color>
            <Color x:Key="NormalTextColor">Blue</Color>
            <Style x:Key="LabelPageHeadingStyle" TargetType="Label">
                <Setter Property="FontAttributes" Value="Bold" />
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="TextColor" Value="{StaticResource HeadingTextColor}" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

이 `ResourceDictionary`은 3 개의 [`Color`](xref:Xamarin.Forms.Color) 리소스 및 [`Style`](xref:Xamarin.Forms.Style) 리소스를 정의 합니다. `App` 클래스에 대 한 자세한 내용은 [App 클래스](~/xamarin-forms/app-fundamentals/application-class.md)를 참조 하세요.

Xamarin. Forms 3.0부터 명시적 `ResourceDictionary` 태그가 필요 하지 않습니다. `ResourceDictionary` 개체는 자동으로 생성 되며 `Resources` 속성-요소 태그 사이에 직접 리소스를 삽입할 수 있습니다.

```xaml
<Application ...>
    <Application.Resources>
        <Color x:Key="PageBackgroundColor">Yellow</Color>
        <Color x:Key="HeadingTextColor">Black</Color>
        <Color x:Key="NormalTextColor">Blue</Color>
        <Style x:Key="LabelPageHeadingStyle" TargetType="Label">
            <Setter Property="FontAttributes" Value="Bold" />
            <Setter Property="HorizontalOptions" Value="Center" />
            <Setter Property="TextColor" Value="{StaticResource HeadingTextColor}" />
        </Style>
    </Application.Resources>
</Application>
```

각 리소스에는 `x:Key` 특성을 사용 하 여 지정 된 키가 있으며이는 `ResourceDictionary`에서이 키가 됩니다. 키는 `StackLayout`내에 정의 된 추가 리소스를 보여 주는 다음 XAML 코드 예제에 나와 있는 것 처럼 [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) 태그 확장을 통해 `ResourceDictionary`에서 리소스를 검색 하는 데 사용 됩니다.

```xaml
<StackLayout Margin="0,20,0,0">
  <StackLayout.Resources>
    <ResourceDictionary>
      <Style x:Key="LabelNormalStyle" TargetType="Label">
        <Setter Property="TextColor" Value="{StaticResource NormalTextColor}" />
      </Style>
      <Style x:Key="MediumBoldText" TargetType="Button">
        <Setter Property="FontSize" Value="Medium" />
        <Setter Property="FontAttributes" Value="Bold" />
      </Style>
    </ResourceDictionary>
  </StackLayout.Resources>
  <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
    <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
           Margin="10,20,10,0"
           Style="{StaticResource LabelNormalStyle}" />
    <Button Text="Navigate"
            Clicked="OnNavigateButtonClicked"
            TextColor="{StaticResource NormalTextColor}"
            Margin="0,20,0,0"
            HorizontalOptions="Center"
            Style="{StaticResource MediumBoldText}" />
</StackLayout>
```

첫 번째 [`Label`](xref:Xamarin.Forms.Label) 인스턴스는 응용 프로그램 `ResourceDictionary`수준에서 정의 된 `LabelPageHeadingStyle` 리소스를 검색 하 여 사용 하며, 두 번째 `Label` 인스턴스는 컨트롤 수준 `LabelNormalStyle`에 정의 된 `ResourceDictionary`리소스를 검색 하 고 사용 합니다. 마찬가지로 [`Button`](xref:Xamarin.Forms.Button) 인스턴스는 응용 프로그램 수준 `ResourceDictionary`에 정의 된 `NormalTextColor` 리소스를 검색 및 사용 하 고, 컨트롤 수준에서 정의 된 `MediumBoldText` 리소스를 `ResourceDictionary`합니다. 이로 인해 결국 다음 스크린샷에 표시된 모양이 됩니다.

[ResourceDictionary 리소스를 소비 하는 ![](resource-dictionaries-images/screenshots-sml.png)](resource-dictionaries-images/screenshots.png#lightbox)

> [!NOTE]
> 단일 페이지와 관련 된 리소스를 포함 하는 응용 프로그램 수준 리소스 사전에 따라서 리소스를 구문 분석 대신 응용 프로그램 시작 시 페이지에서 필요한 경우 되어서는 안 됩니다. 자세한 내용은 [응용 프로그램 리소스 사전 크기 줄이기](~/xamarin-forms/deploy-test/performance.md)를 참조 하세요.

## <a name="override-resources"></a>리소스 재정의

`ResourceDictionary` 리소스가 `x:Key` 특성 값을 공유 하는 경우 뷰 계층 구조에 정의 된 리소스가 상위 수준에서 정의 된 것 보다 우선 적용 됩니다. 예를 들어 응용 프로그램 수준에서 `Blue`로 `PageBackgroundColor` 리소스를 설정 하면 `Yellow`로 설정 된 페이지 수준 `PageBackgroundColor` 리소스에 의해 재정의 됩니다. 마찬가지로, 페이지 수준 `PageBackgroundColor` 리소스는 컨트롤 수준 `PageBackgroundColor` 리소스에 의해 재정의 됩니다. 이 우선 순위는 다음 XAML 코드 예제에서 보여 주는 것:

```xaml
<ContentPage ... BackgroundColor="{StaticResource PageBackgroundColor}">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Blue</Color>
            <Color x:Key="NormalTextColor">Yellow</Color>
        </ResourceDictionary>
    </ContentPage.Resources>
    <StackLayout Margin="0,20,0,0">
        ...
        <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
        <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
               Margin="10,20,10,0"
               Style="{StaticResource LabelNormalStyle}" />
        <Button Text="Navigate"
                Clicked="OnNavigateButtonClicked"
                TextColor="{StaticResource NormalTextColor}"
                Margin="0,20,0,0"
                HorizontalOptions="Center"
                Style="{StaticResource MediumBoldText}" />
    </StackLayout>
</ContentPage>
```

응용 프로그램 수준에서 정의 된 원래 `PageBackgroundColor` 및 `NormalTextColor` 인스턴스는 페이지 수준에서 정의 된 `PageBackgroundColor` 및 `NormalTextColor` 인스턴스에 의해 재정의 됩니다. 따라서 페이지 배경색 파란색 되며 다음 스크린샷과에서 같이 페이지의 텍스트 노랑 됩니다.

[ResourceDictionary 리소스를 재정의 ![](resource-dictionaries-images/overridding-screenshots-sml.png)](resource-dictionaries-images/overridding-screenshots.png#lightbox)

그러나 [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) 속성이 응용 프로그램 수준 `ResourceDictionary`에 정의 된 `PageBackgroundColor` 리소스의 값으로 설정 되어 있기 때문에 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 의 배경 막대가 여전히 노란색입니다.

`ResourceDictionary` 우선 순위를 고려 하는 또 다른 방법은 다음과 같습니다. XAML 파서가 `StaticResource`를 발견 하면 시각적 트리를 통해 발견 된 첫 번째 일치 항목을 사용 하 여 일치 하는 키를 검색 합니다. 이 검색이 페이지에서 종료 되 고 아직 키를 찾지 못한 경우 XAML 파서는 `App` 개체에 연결 된 `ResourceDictionary`를 검색 합니다. 키를 아직 없는 경우 예외가 발생 합니다.

## <a name="stand-alone-resource-dictionaries"></a>독립 실행형 리소스 사전

`ResourceDictionary`에서 파생 된 클래스는 별도의 독립 실행형 파일에 있을 수도 있습니다. (보다 정확 하 게 `ResourceDictionary`에서 파생 된 클래스에는 리소스가 XAML 파일에 정의 되어 있지만 `InitializeComponent` 호출을 포함 하는 코드 숨김이 필요 하므로 일반적으로 파일 _쌍_ 이 필요 합니다.) 그러면 결과 파일을 응용 프로그램 간에 공유할 수 있습니다.

이러한 파일을 만들려면 프로젝트에 새 **콘텐츠 뷰** 또는 **콘텐츠 페이지** 항목을 추가 합니다 ( C# 파일 하나만 있는 콘텐츠 **뷰** 또는 **콘텐츠 페이지** 는 아님). XAML 파일 및 C# 파일에서 기본 클래스의 이름을 `ContentView` 또는 `ContentPage`에서 `ResourceDictionary`으로 변경 합니다. XAML 파일에서 기본 클래스의 이름에는 최상위 요소입니다.

다음 XAML 예제에서는 `MyResourceDictionary`이라는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 를 보여 줍니다.

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ResourceDictionaryDemo.MyResourceDictionary">
    <DataTemplate x:Key="PersonDataTemplate">
        <ViewCell>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.5*" />
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.3*" />
                </Grid.ColumnDefinitions>
                <Label Text="{Binding Name}" TextColor="{StaticResource NormalTextColor}" FontAttributes="Bold" />
                <Label Grid.Column="1" Text="{Binding Age}" TextColor="{StaticResource NormalTextColor}" />
                <Label Grid.Column="2" Text="{Binding Location}" TextColor="{StaticResource NormalTextColor}" HorizontalTextAlignment="End" />
            </Grid>
        </ViewCell>
    </DataTemplate>
</ResourceDictionary>
```

이 `ResourceDictionary`는 `DataTemplate`형식의 개체인 단일 리소스를 포함 합니다.

예를 들어 `ContentPage`의 `Resources` 속성-요소 태그 쌍 사이에 배치 하 여 `MyResourceDictionary`를 인스턴스화할 수 있습니다.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <local:MyResourceDictionary />
    </ContentPage.Resources>
    ...
</ContentPage>
```

`MyResourceDictionary` 인스턴스는 `ContentPage` 개체의 `Resources` 속성으로 설정 됩니다.

그러나이 방법에는 몇 가지 제한 사항이 있습니다. `ContentPage`의 `Resources` 속성은이 `ResourceDictionary`만 참조 합니다. 대부분의 경우 다른 `ResourceDictionary` 인스턴스와 다른 리소스도 포함 하는 옵션을 원합니다.

이 작업에는 병합 된 리소스 사전에 필요합니다.

## <a name="merged-resource-dictionaries"></a>병합된 리소스 사전

병합 된 리소스 사전은 하나 이상의 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 개체를 다른 `ResourceDictionary`에 결합 합니다.

### <a name="merge-local-resource-dictionaries"></a>로컬 리소스 사전 병합

[`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) 속성을 리소스를 사용 하 여 XAML 파일의 파일 이름으로 설정 하 여 로컬 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 를 다른 `ResourceDictionary`에 병합할 수 있습니다.

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

이 구문은 `MyResourceDictionary` 클래스를 인스턴스화하지 않습니다. 대신, XAML 파일을 참조합니다. 이러한 이유로 [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) 속성을 설정할 때 코드 숨김이 필요 하지 않으며 `x:Class` 특성을 **myresourcedictionary .xaml** 파일의 루트 태그에서 제거할 수 있습니다. 또한이 방법을 사용 하 여 리소스 사전을 병합할 때 Xamarin.ios는 자동으로 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)를 인스턴스화하면 외부 `ResourceDictionary` 태그가 필요 하지 않습니다.

> [!IMPORTANT]
> [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) 속성은 XAML 에서만 설정할 수 있습니다.

### <a name="merge-resource-dictionaries-from-other-assemblies"></a>다른 어셈블리의 리소스 사전 병합

[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 를 `ResourceDictionary`의 [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) 속성에 추가 하 여 다른 `ResourceDictionary`에 병합할 수도 있습니다. 이 기법을 사용 하면 리소스가 상주 하는 어셈블리에 관계 없이 리소스 사전을 병합할 수 있습니다.

> [!IMPORTANT]
> 또한 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 클래스는 [`MergedWith`](xref:Xamarin.Forms.ResourceDictionary.MergedWith) 속성을 정의 합니다. 그러나이 속성은 더 이상 사용 되지 않으므로 사용 하면 안 됩니다.

다음 코드 예제에서는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)페이지 수준 [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) 컬렉션에 추가 되는 `MyResourceDictionary` 보여 줍니다.

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:ResourceDictionaryDemo">
    <ContentPage.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <local:MyResourceDictionary />
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

이 예제에서는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에 추가 되는 동일한 어셈블리에 상주 하는 `MyResourceDictionary`의 인스턴스를 보여 줍니다. 또한 다른 어셈블리, [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) 속성-요소 태그 내의 다른 `ResourceDictionary` 개체 및 해당 태그 외부의 기타 리소스에서 리소스 사전을 추가할 수도 있습니다.

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

외부 어셈블리에 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 을 배치할 때 빌드 작업이 **EmbeddedResource**로 설정 되었는지 확인 합니다. 또한 코드 숨김이 파일에 있는지 확인 합니다.

> [!IMPORTANT]
> [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에는 `MergedDictionaries` 속성 요소 태그가 하나만 있을 수 있지만 원하는 만큼 `ResourceDictionary` 개체를 넣을 수 있습니다.

병합 된 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 리소스가 동일한 `x:Key` 특성 값을 공유 하는 경우 xamarin.ios는 다음과 같은 리소스 우선 순위를 사용 합니다.

1. 리소스 사전에 로컬 리소스입니다.
1. `MergedDictionaries` 컬렉션을 통해 병합 된 리소스 사전에 포함 된 리소스는 역순으로 `MergedDictionaries` 속성에 나열 됩니다.

> [!NOTE]
> 리소스가 검색 될 수 있습니다 계산이 많은 작업을 응용 프로그램에 여러 포함 된 경우 큰 리소스 사전입니다. 따라서 불필요 한 검색을 방지 하려면 응용 프로그램에서 각 페이지에만 페이지에 적절 한 리소스 사전 사용 확인 해야 합니다.

## <a name="related-links"></a>관련 링크

- [리소스 사전 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)
- [스타일](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Application-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
