---
title: 리소스 사전
description: XAML 리소스는 공유 하 고 Xamarin.Forms 응용 프로그램 전체에서 다시 사용할 수 있는 개체입니다.
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2019
ms.custom: video
ms.openlocfilehash: 49ea5c2a8332e625710af8a9947e1cb0e0338040
ms.sourcegitcommit: 211fed94fb96127a3e158ae1ff5d7eb831a203d8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75955823"
---
# <a name="resource-dictionaries"></a>리소스 사전

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)

_XAML 리소스는 Xamarin.ios 응용 프로그램 전체에서 공유 하 고 다시 사용할 수 있는 개체의 정의입니다. 이러한 리소스 개체는 리소스 사전에 저장 됩니다._

A [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 는 Xamarin.Forms 응용 프로그램에서 사용 되는 리소스에 대 한 리포지토리입니다. 에 저장 되는 일반적인 리소스를 `ResourceDictionary` 포함 [스타일](~/xamarin-forms/user-interface/styles/index.md)를 [컨트롤 템플릿](~/xamarin-forms/app-fundamentals/templates/control-template.md)를 [데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), 색 및 변환기입니다.

XAML을에 저장 되는 리소스에는 `ResourceDictionary` 다음 검색 및 수를 사용 하 여 요소에 적용 합니다 `StaticResource` 태그 확장 합니다. C#에서 리소스 또한에서 정의할 수 있습니다는 `ResourceDictionary` 검색 한 다음 및 문자열 기반으로 하는 인덱서를 사용 하 여 요소에 적용 합니다. 그런데 잇점이로 `ResourceDictionary` C#에서는 공유 개체 필드 또는 속성으로 저장 및 없이 직접 액세스할 수 있습니다 단순히 대로 첫 번째 필요가에서 검색 사전입니다.

## <a name="create-and-consume-a-resourcedictionary"></a>ResourceDictionary 만들기 및 사용

리소스에 정의 되어는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 즉 설정 중 하나를 `Resources` 속성:

- 합니다 [ `Resources` ](xref:Xamarin.Forms.Application.Resources) 에서 파생 되는 모든 클래스의 속성 [`Application`](xref:Xamarin.Forms.Application)
- 합니다 [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) 에서 파생 되는 모든 클래스의 속성 [`VisualElement`](xref:Xamarin.Forms.Application)

파생 되는 하나의 클래스를 포함 하는 Xamarin.Forms 프로그램 `Application` 에서 파생 되는 많은 클래스가 자주 사용 되지만 `VisualElement`, 페이지, 레이아웃 및 컨트롤 포함 합니다. 이러한 개체 중 하나가 있을 수 있습니다 해당 `Resources` 속성이로 설정 된 `ResourceDictionary`합니다. 특정을 넣을 위치를 선택 `ResourceDictionary` 리소스를 사용할 수 있는 영향을 줍니다.

- 리소스를 `ResourceDictionary` 와 같은 보기에 연결 된 `Button` 또는 `Label` 이 기능은 매우 유용 하므로 해당 특정 개체에 적용할만 있습니다.
- 리소스를 `ResourceDictionary` 와 같은 레이아웃에 연결 `StackLayout` 또는 `Grid` 레이아웃 및 레이아웃의 모든 자식에 적용할 수 있습니다.
- 리소스를 `ResourceDictionary` 정의 페이지 수준 및 적용할 수 있습니다 페이지에 모든 해당 하위 항목입니다.
- 리소스를 `ResourceDictionary` 정의 응용 프로그램 수준 응용 프로그램에 걸쳐 적용 수 있습니다.

다음 XAML 응용 프로그램 수준에 정의 된 리소스를 보여 줍니다 `ResourceDictionary` 에 **App.xaml** 표준 Xamarin.Forms 프로그램의 일부로 생성 된 파일:

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

이렇게 `ResourceDictionary` 세 가지 정의 [ `Color` ](xref:Xamarin.Forms.Color) 리소스 및 [ `Style` ](xref:Xamarin.Forms.Style) 리소스. 에 대 한 자세한 내용은 합니다 `App` 클래스를 참조 하십시오 [App 클래스](~/xamarin-forms/app-fundamentals/application-class.md)합니다.

명시적 Xamarin.Forms 3.0부터 `ResourceDictionary` 태그는 필요 하지 않습니다. 합니다 `ResourceDictionary` 개체는 자동으로 생성 되 고 리소스 사이 직접 삽입할 수 있습니다는 `Resources` 속성 요소 태그:

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

각 리소스를 사용 하 여 지정 된 키에는 `x:Key` 사전 키에 해당 되는 특성을 `ResourceDictionary`합니다. 리소스를 검색 하려면 키가 사용 합니다 `ResourceDictionary` 여는 [ `StaticResource` ](xref:Xamarin.Forms.Xaml.StaticResourceExtension) 내에 정의 된 추가 리소스를 보여 주는 다음 XAML 코드 예제 에서처럼 태그 확장은 `StackLayout`:

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

첫 번째 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스를 검색 하 고 사용 합니다 `LabelPageHeadingStyle` 응용 프로그램 수준에 정의 된 리소스 `ResourceDictionary`, 두 번째 `Label` 인스턴스 검색 및 사용 하 여 `LabelNormalStyle`컨트롤 수준에 정의 된 리소스 `ResourceDictionary`합니다. 마찬가지로,는 [ `Button` ](xref:Xamarin.Forms.Button) 인스턴스를 검색 하 고 사용 합니다 `NormalTextColor` 응용 프로그램 수준에 정의 된 리소스 `ResourceDictionary`, 및 `MediumBoldText` 컨트롤 수준에 정의 된 리소스 `ResourceDictionary`합니다. 이로 인해 결국 다음 스크린샷에 표시된 모양이 됩니다.

[ResourceDictionary 리소스를 소비 하는 ![](resource-dictionaries-images/screenshots-sml.png)](resource-dictionaries-images/screenshots.png#lightbox)

> [!NOTE]
> 단일 페이지와 관련 된 리소스를 포함 하는 응용 프로그램 수준 리소스 사전에 따라서 리소스를 구문 분석 대신 응용 프로그램 시작 시 페이지에서 필요한 경우 되어서는 안 됩니다. 자세한 내용은 [응용 프로그램 리소스 사전 크기 줄이기](~/xamarin-forms/deploy-test/performance.md)합니다.

## <a name="override-resources"></a>리소스 재정의

때 `ResourceDictionary` 리소스 공유 `x:Key` 특성 값을 낮은 뷰 계층 구조에서 정의 된 리소스 우선를 더 높은 정의 된 합니다. 예를 들어 설정 된 `PageBackgroundColor` 리소스를 `Blue` 응용 프로그램 수준 페이지 수준에 따라 재정의 됩니다 `PageBackgroundColor` 리소스 집합 `Yellow`합니다. 마찬가지로, 페이지 수준 `PageBackgroundColor` 리소스 제어 수준에 따라 재정의 됩니다 `PageBackgroundColor` 리소스입니다. 이 우선 순위는 다음 XAML 코드 예제에서 보여 주는 것:

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

원래 `PageBackgroundColor` 및 `NormalTextColor` 인스턴스를 응용 프로그램 수준에서 정의 의해 재정의 되는 `PageBackgroundColor` 및 `NormalTextColor` 페이지 수준에서 정의 된 인스턴스. 따라서 페이지 배경색 파란색 되며 다음 스크린샷과에서 같이 페이지의 텍스트 노랑 됩니다.

[ResourceDictionary 리소스를 재정의 ![](resource-dictionaries-images/overridding-screenshots-sml.png)](resource-dictionaries-images/overridding-screenshots.png#lightbox)

하지만의 막대는 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 는 여전히 노란색 때문에 [ `BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) 속성의 값으로 설정 됩니다는 `PageBackgroundColor` 응용 프로그램에 정의 된 리소스 수준 `ResourceDictionary`합니다.

에 대해 생각 하는 또 다른 방법은 다음과 같습니다 `ResourceDictionary` 우선 순위: 때의 XAML 파서는 `StaticResource`시각적 트리를 통해 위로 이동 하 여 일치 하는 키에 대 한 검색, 찾으면 첫 번째 일치 항목을 사용 하 여 합니다. 페이지에서이 검색이 종료 없고 키 여전히 않은 검색 하는 경우 XAML 파서를 검색 합니다 `ResourceDictionary` 연결할는 `App` 개체. 키를 아직 없는 경우 예외가 발생 합니다.

## <a name="stand-alone-resource-dictionaries"></a>독립 실행형 리소스 사전

파생 된 클래스 `ResourceDictionary` 별도 독립 실행형 파일에 있을 수도 있습니다. (보다 정확 하 게 `ResourceDictionary`에서 파생 된 클래스에는 리소스가 XAML 파일에 정의 되어 있지만 `InitializeComponent` 호출을 포함 하는 코드 숨김이 필요 하므로 일반적으로 파일 _쌍_ 이 필요 합니다.) 그러면 결과 파일을 응용 프로그램 간에 공유할 수 있습니다.

이러한 파일을 만들려면 새 추가 **콘텐츠 보기** 또는 **콘텐츠 페이지** 항목을 프로젝트 (아닌를 **콘텐츠 뷰** 하거나 **콘텐츠 페이지** 사용 하 여 C# 파일만)입니다. XAML 파일 및 C# 파일을 기본 클래스의 이름을 변경 `ContentView` 나 `ContentPage` 에 `ResourceDictionary`입니다. XAML 파일에서 기본 클래스의 이름에는 최상위 요소입니다.

다음 XAML 예제는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 라는 `MyResourceDictionary`:

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

이렇게 `ResourceDictionary` 형식의 개체는 단일 리소스를 포함 `DataTemplate`합니다.

인스턴스화할 수 있습니다 `MyResourceDictionary` 쌍 간 넣어 `Resources` 속성 요소 태그, 예를 들어 한 `ContentPage`:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <local:MyResourceDictionary />
    </ContentPage.Resources>
    ...
</ContentPage>
```

인스턴스의 `MyResourceDictionary` 로 설정 되어를 `Resources` 의 속성을 `ContentPage` 개체입니다.

그러나이 방법에 몇 가지 제한 사항이:는 `Resources` 의 속성을 `ContentPage` 이 하나만 참조 `ResourceDictionary`합니다. 대부분의 경우에서 다른 포함 옵션을 원하는 `ResourceDictionary` 인스턴스 및 아마도 다른 리소스도 있습니다.

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

> [!IMPORTANT]
> [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에는 `MergedDictionaries` 속성 요소 태그가 하나만 있을 수 있지만 원하는 만큼 `ResourceDictionary` 개체를 넣을 수 있습니다.

병합할 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 리소스는 동일한 공유 `x:Key` 특성 값, Xamarin.Forms 다음 리소스 우선 순위를 사용 하 여:

1. 리소스 사전에 로컬 리소스입니다.
1. 통해 병합 된 리소스 사전에 포함 된 리소스를 `MergedDictionaries` 컬렉션에 나열 된 순서와 반대로 `MergedDictionaries` 속성입니다.

> [!NOTE]
> 리소스가 검색 될 수 있습니다 계산이 많은 작업을 응용 프로그램에 여러 포함 된 경우 큰 리소스 사전입니다. 따라서 불필요 한 검색을 방지 하려면 응용 프로그램에서 각 페이지에만 페이지에 적절 한 리소스 사전 사용 확인 해야 합니다.

## <a name="related-links"></a>관련 링크

- [리소스 사전 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)
- [스타일](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Application-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
