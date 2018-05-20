---
title: 리소스 사전
description: XAML 리소스는 공유 하 고 Xamarin.Forms 응용 프로그램에 걸쳐 다시 사용할 수 있는 개체입니다.
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/07/2018
ms.openlocfilehash: 47cca2f726b0af396ea1eb287cfa4e1f1bf19724
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/19/2018
---
# <a name="resource-dictionaries"></a>리소스 사전

_XAML 리소스는 공유 하 고 Xamarin.Forms 응용 프로그램에 걸쳐 다시 사용할 수 있는 개체의 정의입니다._

이러한 리소스 개체 리소스 사전에 저장 됩니다. 이 문서를 만들고 리소스 사전을 사용 하는 방법과 리소스 사전을 병합 하는 방법을 설명 합니다.

## <a name="overview"></a>개요

A [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 는 Xamarin.Forms 응용 프로그램에서 사용 되는 리소스에 대 한 리포지토리입니다. 에 저장 된 리소스에는 대개는 `ResourceDictionary` 포함 [스타일](~/xamarin-forms/user-interface/styles/index.md), [컨트롤 템플릿](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md), [데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), 색 및 변환기입니다.

XAML에 저장 된 리소스에는 `ResourceDictionary` 다음 검색 및 수를 사용 하 여 요소에 적용 된 `StaticResource` 태그 확장 합니다. C#에서 리소스도 정의할 수에 `ResourceDictionary` 다음 검색 하 고 문자열 기반 인덱서를 사용 하 여 요소에 적용 합니다. 그러나는 사용 하는 이점이 별로 한 `ResourceDictionary` C#에서 공유 개체 간단히 필드 또는 속성으로 저장 하 고 액세스할 수 하지 않고 직접 대로 첫 번째 필요 검색할 사전에서 합니다.

## <a name="creating-and-consuming-a-resourcedictionary"></a>만들기 및 ResourceDictionary 사용

에 정의 된 리소스는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 다음 중 하나를 설정 합니다 즉 `Resources` 속성:

- [ `Resources` ](xref:Xamarin.Forms.Application.Resources) 에서 파생 된 클래스의 속성 [`Application`](xref:Xamarin.Forms.Application)
- [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) 속성에서 파생 된 클래스의 ['VisualElement'](xref:Xamarin.Forms.Application)

파생 되는 하나의 클래스를 포함 하는 Xamarin.Forms 프로그램 `Application` 에서는 종종에서 파생 되는 많은 클래스를 활용 하지만 `VisualElement`, 페이지, 레이아웃 및 컨트롤을 포함 합니다. 이러한 개체 중 하나를 가질 수 있습니다 해당 `Resources` 속성이로 설정 된 `ResourceDictionary`합니다. 특정 넣을 위치를 선택 `ResourceDictionary` 영향 리소스를 사용할 수 있습니다.

- 리소스는 `ResourceDictionary` 와 같은 보기에 연결 된 `Button` 또는 `Label` 이 기능은 매우 유용 하므로 해당 특정 개체에 적용할만 있습니다.
- 리소스는 `ResourceDictionary` 와 같은 레이아웃에 연결 된 `StackLayout` 또는 `Grid` 레이아웃 및 해당 레이아웃의 모든 자식에 적용할 수 있습니다.
- 리소스는 `ResourceDictionary` 정의 페이지에서 수준 수에 적용할 수 페이지에 있는 모든 자식을 합니다.
- 리소스는 `ResourceDictionary` 정의 된 응용 프로그램에서 수준은 응용 프로그램에 걸쳐 적용 수 있습니다.

다음 XAML 응용 프로그램 수준에 정의 된 리소스를 보여 줍니다. `ResourceDictionary` 에 **App.xaml** 표준 Xamarin.Forms 프로그램의 일부분으로 만든 파일:

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

이 `ResourceDictionary` 세 정의 [ `Color` ](xref:Xamarin.Forms.Color) 리소스 및 [ `Style` ](xref:Xamarin.Forms.Style) 리소스입니다. 에 대 한 자세한 내용은 `App` 클래스를 참조 하십시오. [App 클래스](~/xamarin-forms/app-fundamentals/application-class.md)합니다.

명시적 Xamarin.Forms 3.0부터 `ResourceDictionary` 태그가 필요 하지 않습니다. `ResourceDictionary` 개체는 자동으로 생성 되 고 리소스를 사이 직접 삽입할 수 있습니다는 `Resources` 속성 요소 태그:

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

각 리소스를 사용 하 여 지정 된 키에는 `x:Key` 에 사전 키가 있는 특성에는 `ResourceDictionary`합니다. 리소스를 검색 하는 키를 사용 하는 `ResourceDictionary` 여는 [ `StaticResource` ](xref:Xamarin.Forms.Xaml.StaticResourceExtension) 내에 정의 된 추가 리소스를 보여 주는 다음 XAML 코드 예제에서와 같이 태그 확장은 `StackLayout`:

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

첫 번째 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스를 검색 하 고 사용 된 `LabelPageHeadingStyle` 응용 프로그램 수준에서 정의 된 리소스 `ResourceDictionary`, 두 번째 `Label` 인스턴스 검색 및 사용 하 여 `LabelNormalStyle`제어 수준에 정의 된 리소스 `ResourceDictionary`합니다. 마찬가지로,는 [ `Button` ](xref:Xamarin.Forms.Button) 인스턴스를 검색 하 고 사용 된 `NormalTextColor` 응용 프로그램 수준에서 정의 된 리소스 `ResourceDictionary`, 및 `MediumBoldText` 제어 수준에 정의 된 리소스 `ResourceDictionary`합니다. 다음 스크린샷에 표시 된 모양 결과이 됩니다.

[![](resource-dictionaries-images/screenshots-sml.png "ResourceDictionary 리소스를 소비")](resource-dictionaries-images/screenshots.png#lightbox "ResourceDictionary 리소스 사용")

> [!NOTE]
> 응용 프로그램 수준 리소스 사전을, 따라서 리소스 다음 구문 분석 아닌 응용 프로그램 시작 시 페이지에서 필요한 경우에 한 페이지에 관련 된 리소스를 포함 해서는 안. 자세한 내용은 참조 [응용 프로그램 리소스 사전의 크기를 줄이거나](~/xamarin-forms/deploy-test/performance.md)합니다.

## <a name="overriding-resources"></a>리소스를 재정의합니다.

때 `ResourceDictionary` 리소스가 공유 `x:Key` 특성 값 뷰 계층 구조에서 더 아래로 정의 된 리소스 보다 우선 합니다 높을수록를 정의 하는 것입니다. 예를 들어 설정는 `PageBackgroundColor` 리소스를 `Blue` 응용 프로그램에서 수준 페이지 수준으로 재정의 됩니다 `PageBackgroundColor` 리소스 집합 `Yellow`합니다. 페이지 수준 마찬가지로, `PageBackgroundColor` 리소스 제어 수준으로 재정의 됩니다 `PageBackgroundColor` 리소스입니다. 이러한 우선 순위는 다음 XAML 코드 예제에서는 하 여 표시 됩니다.

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

원래 `PageBackgroundColor` 및 `NormalTextColor` 에 의해 재정의 되며 응용 프로그램 수준에서 정의 된 인스턴스는 `PageBackgroundColor` 및 `NormalTextColor` 페이지 수준에서 정의 된 인스턴스. 따라서 페이지 배경색 파란색 되며 다음 스크린샷에서 같이 페이지에서 텍스트, 노란색 됩니다.

[![](resource-dictionaries-images/overridding-screenshots-sml.png "ResourceDictionary 리소스 재정의")](resource-dictionaries-images/overridding-screenshots.png#lightbox "ResourceDictionary 리소스를 재정의 합니다.")

하지만의 막대는 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 은 여전히 노란색 때문에 [ `BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) 속성의 값으로 설정 됩니다는 `PageBackgroundColor` 응용 프로그램에 정의 된 리소스 수준 `ResourceDictionary`합니다.

다른 방법에 대해 생각 하 `ResourceDictionary` 우선 순위: 때 XAML 파서가 발견할는 `StaticResource`, 시각적 트리를 통해 위로 이동 하 여 일치 하는 키에 대 한 검색, 첫 번째 일치 항목을 사용 하 여 발견 합니다. 이 검색 페이지에서 끝나고 키 여전히 되었습니다 찾을 수 없을 경우 XAML 파서가 검색는 `ResourceDictionary` 에 연결 된는 `App` 개체입니다. 키 아직 없는 경우, 예외가 발생 합니다.

## <a name="stand-alone-resource-dictionaries"></a>독립 실행형 리소스 사전

클래스에서 파생 `ResourceDictionary` 별도 독립 실행형 파일에 있을 수도 있습니다. (보다 정확 하 게 파생 클래스를 `ResourceDictionary` 일반적으로 _쌍_ 파일의 리소스와 코드 숨김 파일을 제외 하 고 XAML 파일에 정의 되어 있으므로 `InitializeComponent` 호출은도 필요 합니다.) 그런 다음 응용 프로그램에서 결과 파일을 공유할 수 있습니다.

이러한 파일을 만들려면 새 추가 **콘텐츠 보기** 또는 **콘텐츠 페이지** 항목을 프로젝트 (하지 않고는 **콘텐츠 보기** 또는 **콘텐츠 페이지** 와 C# 파일만)입니다. XAML 파일와 C# 파일을 기본 클래스의 이름을 변경 `ContentView` 또는 `ContentPage` 를 `ResourceDictionary`합니다. XAML 파일에서 기본 클래스의 이름에는 최상위 요소입니다.

XAML은 다음 예제는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 라는 `MyResourceDictionary`:

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

이 `ResourceDictionary` 형식의 개체는 단일 리소스 포함 `DataTemplate`합니다.

인스턴스화할 수 `MyResourceDictionary` 사용 하 여 한 쌍의 `Resources` 속성 요소 태그, 예를 들어 한 `ContentPage`:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <local:MyResourceDictionary />
    </ContentPage.Resources>
    ...
</ContentPage>
```

인스턴스 `MyResourceDictionary` 로 설정 되는 `Resources` 속성은 `ContentPage` 개체입니다.

그러나이 방법에는 몇 가지 제한 사항이:는 `Resources` 의 속성은 `ContentPage` 이 하나만 참조 `ResourceDictionary`합니다. 대부분의 경우에서 다른 포함 하려는 `ResourceDictionary` 인스턴스 및 아마도 다른 뿐만 아니라 리소스입니다.

이 작업에는 병합 된 리소스 사전이 필요합니다.

## <a name="merged-resource-dictionaries"></a>병합된 리소스 사전

하나 이상의 병합 된 리소스 사전을 결합 `ResourceDictionary` 다른 파티션으로 인스턴스 `ResourceDictionary`합니다. 설정 하 여 XAML 파일에서 이렇게 하려면는 [ `MergedDictionaries` ](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) 속성을 응용 프로그램, 페이지 또는 제어 수준으로 병합 될 하나 이상의 리소스 사전 `ResourceDictionary`합니다.

> [!IMPORTANT]
> `ResourceDictionary` 또한 정의 [ `MergedWith` ](xref:Xamarin.Forms.ResourceDictionary.MergedWith) 속성입니다. 이 속성을 사용 하지 마십시오 이 사용 되지 Xamarin.Forms 3.0을 기준으로 합니다.

인스턴스 및 `MyResourceDictionary` 모든 응용 프로그램, 페이지 또는 제어 수준에 병합 `ResourceDictionary`합니다. 페이지 수준에 병합 하 고 다음 XAML 코드 예제에서는 표시 `ResourceDictionary` 를 사용 하 여 `MergedDictionaries` 속성:

```xaml
<ContentPage ...>
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

해당 태그의 인스턴스만 표시 `MyResourceDictionary` 에 추가 되는 `ResourceDictionary` 하지만 다른도 참조할 수 있습니다 `ResourceDictionary` 인스턴스에 `MergedDictionary` 속성 요소 태그 및 해당 태그 외에 다른 리소스:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>

            <!-- Add more resources here -->

            <ResourceDictionary.MergedDictionaries>

                <!-- Add more resource dictionaries here -->

                <local:MyResourceDictionary />

                <!-- Add more resource dictionaries here -->

            </ResourceDictionary.MergedDictionaries>

            <!-- Add more resources here -->

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

하나만 있을 수 있습니다 `MergedDictionaries` 섹션는 `ResourceDictionary`, 만큼 넣을 수 있습니다 하지만 `ResourceDictionary` 거기에 인스턴스를 원하는 만큼 합니다.

병합할 때 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 리소스가 동일한 공유 `x:Key` Xamarin.Forms 특성 값을 다음과 같은 리소스 우선 순위를 사용 하 여:

1. 리소스 사전에 대 한 로컬 리소스입니다.
1. 병합 된 리소스 사전에 포함 된 리소스 사용 되지 않는 통해 [ `MergedWith` ](xref:Xamarin.Forms.ResourceDictionary.MergedWith) 속성입니다.
1. 통해 병합 된 리소스 사전에 포함 된 리소스는 `MergedDictionaries` 컬렉션에 나열 된 순서로 `MergedDictionaries` 속성입니다.

> [!NOTE]
> 리소스 사전 검색 될 수 있습니다는 계산이 많은 작업 응용 프로그램에 여러 포함 된 경우 큰 리소스 사전입니다. 따라서 불필요 한 검색을 방지 하려면 응용 프로그램에서 각 페이지에만 페이지에 적절 한 리소스 사전을 사용 확인 해야 합니다.

## <a name="merging-dictionaries-in-xamarinforms-30"></a>Xamarin.forms 3.0 사전 병합

병합 프로세스 Xamarin.Forms 3.0 부터는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 다소 더 쉽고 유연한 인스턴스 커졌습니다. `MergedDictionaries` 속성 요소 태그는 더 이상 필요 합니다. 대신 추가 리소스 사전에 다른 `ResourceDictionary` 새 태그 [ `Source` ](xref:Xamarin.Forms.ResourceDictionary.Source) 속성이 리소스를 가진 XAML 파일의 파일 이름으로 설정 합니다.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>

            <!-- Add more resources here -->

            <ResourceDictionary Source="MyResourceDictionary.xaml" />

            <!-- Add more resources here -->

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Xamarin.Forms 3.0이 자동으로 인스턴스화합니다 때문에 `ResourceDictionary`, 그 두 외부 `ResourceDictionary` 태그는 더 이상 필요 합니다.

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

이 새로운 구문의 않습니다 _하지_ 인스턴스화하는 `MyResourceDictionary` 클래스입니다. 대신, XAML 파일을 참조합니다. 코드 숨김 파일을 이유로입니다 (**MyResourceDictionary.xaml.cs**)는 더 이상 필요 합니다. 제거할 수도 있습니다는 `x:Class` 의 루트 태그에서 특성은 **MyResourceDictionary.xaml** 파일입니다.

## <a name="summary"></a>요약

이 문서를 만들고 사용 하는 방법을 설명는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), 및 리소스 사전은 병합 하는 방법입니다. A `ResourceDictionary` 하면 리소스를 단일 위치에서 정의 하 고 Xamarin.Forms 응용 프로그램에 걸쳐 다시 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [리소스 사전 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/xaml/resourcedictionaries/)
- [스타일](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
