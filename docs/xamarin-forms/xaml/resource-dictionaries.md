---
title: "리소스 사전"
description: "XAML 리소스는 한 번만 사용할 수 있는 개체의 정의입니다. ResourceDictionary에 리소스를를 단일 위치에서 정의 하 고 Xamarin.Forms 응용 프로그램에 걸쳐 다시 사용할 수 있습니다. 이 문서를 만들고 ResourceDictionary를 사용 하는 방법 및 리소스 사전은 병합 하는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/17/2017
ms.openlocfilehash: 0c2765551c16be605bc78d9ef32a91fd2c4ead8c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="resource-dictionaries"></a>리소스 사전

_XAML 리소스는 한 번만 사용할 수 있는 개체의 정의입니다. ResourceDictionary에 리소스를를 단일 위치에서 정의 하 고 Xamarin.Forms 응용 프로그램에 걸쳐 다시 사용할 수 있습니다. 이 문서를 만들고 ResourceDictionary를 사용 하는 방법 및 리소스 사전은 병합 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

A [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 는 Xamarin.Forms 응용 프로그램에서 사용 되는 리소스에 대 한 리포지토리입니다. 에 저장 된 리소스에는 대개는 `ResourceDictionary` 포함 [스타일](~/xamarin-forms/user-interface/styles/index.md), [컨트롤 템플릿](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md), [데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), 색 및 변환기입니다.

XAML, 리소스에 정의 되어 한 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 다음 검색 하 고 사용 하 여 요소에 적용 된 `StaticResource` 태그 확장 합니다. C#에서 리소스에 정의 되어는 `ResourceDictionary` 다음 검색 하 고 문자열 기반 인덱서를 사용 하 여 요소에 적용 합니다. 그러나는 사용 하는 이점이 별로 `ResourceDictionary` C#에서 리소스 그룹으로 쉽게 직접에 지정할 수 시각적 요소의 속성에서 처음 검색 하지 않고도 `ResourceDictionary`합니다.

## <a name="creating-and-consuming-a-resourcedictionary"></a>만들기 및 ResourceDictionary 사용

리소스를 정의할 수는 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 에 연결 된는 [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) 이후 또는 페이지 또는 컨트롤의 컬렉션은 [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) 컬렉션 응용 프로그램입니다. 정의할 위치를 선택는 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 사용할 수 있는 위치에 영향을 미칩니다.

- 리소스는 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 정의 컨트롤에서 수준만 적용 될 수 및 해당 자식 컨트롤에 있습니다.
- 리소스는 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 정의 페이지에서 수준만 적용 될 수 및 해당 자식 페이지에 있습니다.
- 리소스는 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 정의 된 응용 프로그램에서 수준은 응용 프로그램에 걸쳐 적용 수 있습니다.

다음 XAML 코드 예제에서는 응용 프로그램 수준에 정의 된 리소스 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/):

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

이 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 세 정의 [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) 리소스 및 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 리소스입니다. XAML을 작성 하는 방법에 대 한 자세한 내용은 `App` 클래스를 참조 하십시오. [App 클래스](~/xamarin-forms/app-fundamentals/application-class.md)합니다.

각 리소스를 사용 하 여 지정 된 키가는 `x:Key` 특성에 설명이 포함 된 키를 제공 하는 `ResourceDictionary`합니다. 리소스를 검색 하는 키가 사용 되는 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 하 여는 `StaticResource` 수준 컨트롤에 정의 된 추가 리소스를 보여 주는 다음 XAML 코드 예제에서와 같이 태그 확장 `ResourceDictionary`:

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

첫 번째 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 인스턴스를 검색 하 고 사용 된 `LabelPageHeadingStyle` 응용 프로그램 수준에서 정의 된 리소스 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), 두 번째 `Label` 인스턴스 검색 하 고 사용 된 `LabelNormalStyle` 제어 수준에 정의 된 리소스 `ResourceDictionary`합니다. 마찬가지로,는 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) 인스턴스를 검색 하 고 사용 된 `NormalTextColor` 응용 프로그램 수준에서 정의 된 리소스 `ResourceDictionary`, 및 `MediumBoldText` 제어 수준에 정의 된 리소스 `ResourceDictionary`합니다. 다음 스크린샷에 표시 된 모양 결과이 됩니다.

[![](resource-dictionaries-images/screenshots-sml.png "ResourceDictionary 리소스를 소비")](resource-dictionaries-images/screenshots.png "ResourceDictionary 리소스 사용")

> [!NOTE]
> **참고**: 따라서 리소스 다음 구문 분석 대신 응용 프로그램 시작 시 페이지에서 필요한 경우, 한 페이지에 관련 리소스를 응용 프로그램 수준 리소스 사전에 포함 해서는 안 됩니다. 자세한 내용은 참조 [응용 프로그램 리소스 사전의 크기를 줄이거나](~/xamarin-forms/deploy-test/performance.md)합니다.

## <a name="overriding-resources"></a>리소스를 재정의합니다.

때 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 리소스가 공유 `x:Key` 특성 값 뷰 계층 구조에서 더 아래로 정의 된 리소스 보다 우선 합니다 높을수록를 정의 하는 것입니다. 예를 들어 설정는 `PageBackgroundColor` 리소스를 `Blue` 응용 프로그램에서 수준 페이지 수준으로 재정의 됩니다 `PageBackgroundColor` 리소스 집합 `Yellow`합니다. 페이지 수준 마찬가지로, `PageBackgroundColor` 리소스 제어 수준으로 재정의 됩니다 `PageBackgroundColor` 리소스입니다. 이러한 우선 순위는 다음 XAML 코드 예제에서는 하 여 표시 됩니다.

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

[![](resource-dictionaries-images/overridding-screenshots-sml.png "ResourceDictionary 리소스 재정의")](resource-dictionaries-images/overridding-screenshots.png "ResourceDictionary 리소스를 재정의 합니다.")

하지만의 막대는 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 은 여전히 노란색 때문에 [ `BarBackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarBackgroundColor/) 속성의 값으로 설정 됩니다는 `PageBackgroundColor` 응용 프로그램에 정의 된 리소스 수준 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)합니다.

## <a name="merged-resource-dictionaries"></a>병합된 리소스 사전

하나 이상의 병합 된 리소스 사전을 결합 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 다른 파티션으로 인스턴스. 설정 하 여 이렇게는 `ResourceDictionary.MergedDictionaries` 속성을 응용 프로그램, 페이지 또는 제어 수준으로 병합 될 하나 이상의 리소스 사전 `ResourceDictionary`합니다.

> [!IMPORTANT]
> [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 형식 역시는 [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/) 단일 병합에 사용할 수 있는 속성 `ResourceDictionary` ,와 `ResourceDictionary` 는 값으로지정`MergedWith`현재 병합 하는 속성 `ResourceDictionary` 인스턴스. 통해 병합할 때는 `MergedWith` 속성, 현재에서 리소스 `ResourceDictionary` 공유 하는 `x:Key` 특성 값에 있는 리소스와의 `ResourceDictionary` 병합 될 대체 됩니다. 그러나는 `MergedWith` 속성 Xamarin.Forms의 이후 릴리스에서 중단 될 예정입니다. 따라서는 것이 좋습니다 사용 하는 `MergedDictionaries` 속성 병합을 `ResourceDictionary` 인스턴스.

다음 XAML 코드 예제는 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 라는 `MyResourceDictionary` 단일 리소스가 포함 된:

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

`MyResourceDictionary` 모든 응용 프로그램, 페이지 또는 제어 수준에 병합 `ResourceDictionary`합니다. 페이지 수준에 병합 하 고 다음 XAML 코드 예제에서는 표시 `ResourceDictionary` 를 사용 하 여 `MergedDictionaries` 속성:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <local:MyResourceDictionary />
                <!-- Add more resource dictionaries here -->
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

병합할 때 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 리소스가 동일한 공유 `x:Key` Xamarin.Forms 특성 값을 다음과 같은 리소스 우선 순위를 사용 하 여:

1. 리소스 사전에 대 한 로컬 리소스입니다.
1. 이전에 병합을 통해 리소스 사전에 포함 된 리소스는 [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/) 속성입니다.
1. 통해 병합 된 리소스 사전에 포함 된 리소스는 `MergedDictionaries` 컬렉션에 나열 된 순서로 `MergedDictionaries` 속성입니다.

> [!NOTE]
> **참고**: 리소스 사전 검색 될 수 있습니다는 계산이 많은 작업 응용 프로그램에 여러 포함 된 경우 큰 리소스 사전입니다. 따라서, 응용 프로그램에서 각 페이지에만 불필요 한 검색을 방지 하기 위해 페이지에 적절 한 리소스 사전을 사용 확인 해야 합니다.

## <a name="summary"></a>요약

이 문서를 만들고 사용 하는 방법을 설명는 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), 및 리소스 사전은 병합 하는 방법입니다. A `ResourceDictionary` 하면 리소스를 단일 위치에서 정의 하 고 Xamarin.Forms 응용 프로그램에 걸쳐 다시 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [리소스 사전 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/xaml/resourcedictionaries/)
- [스타일](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
