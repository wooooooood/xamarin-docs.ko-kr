---
title: Xamarin.Forms 성능
description: Xamarin.Forms 애플리케이션의 성능을 높이기 위한 많은 기술이 있습니다. 이러한 기술은 전체적으로 CPU에서 수행하는 작업의 양과 애플리케이션에서 소비하는 메모리의 양을 크게 줄일 수 있습니다. 이 문서에서는 이러한 기술에 대해 설명합니다.
ms.prod: xamarin
ms.assetid: 0be84c56-6698-448d-be5a-b4205f1caa9f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: b445c1f8d3d440ecf609d5f3c1b7cc7147343fe0
ms.sourcegitcommit: a153623a69b5cb125f672df8007838afa32e9edf
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67268612"
---
# <a name="xamarinforms-performance"></a>Xamarin.Forms 성능

_Xamarin.Forms 애플리케이션의 성능을 높이기 위한 많은 기술이 있습니다. 이러한 기술은 전체적으로 CPU에서 수행하는 작업의 양과 애플리케이션에서 소비하는 메모리의 양을 크게 줄일 수 있습니다. 이 문서에서는 이러한 기술에 대해 설명합니다._

> [!VIDEO https://youtube.com/embed/RZvdql3Ev0E]

**Evolve 2016: Xamarin.Forms를 사용하여 앱 성능 최적화**

## <a name="overview"></a>개요

낮은 애플리케이션 성능은 여러가지 방법으로 나타납니다. 이 경우에 애플리케이션이 응답하지 않는 것처럼 보이고, 스크롤 속도가 느려지고, 배터리 수명이 줄어들 수 있습니다. 그러나 성능을 최적화하려면 효율적인 코드를 구현하는 것 이상이 필요합니다. 애플리케이션 성능에 대한 사용자 환경도 고려해야 합니다. 예를 들어 사용자가 다른 활동을 수행하지 못하도록 차단하지 않고 작업을 실행하면 사용자 환경을 향상시키는 데 도움이 될 수 있습니다.

Xamarin.Forms 애플리케이션의 성능과 인식 성능을 높이는 여러 가지 기술이 있습니다. 다음과 같은 변경 내용이 해당됩니다.

- [XAML 컴파일러 활성화](#xamlc)
- [올바른 레이아웃 선택](#correctlayout)
- [레이아웃 압축 활성화](#layoutcompression)
- [빠른 렌더러 사용](#fastrenderers)
- [불필요한 바인딩 줄이기](#databinding)
- [레이아웃 성능 최적화](#optimizelayout)
- [ListView 성능 최적화](#optimizelistview)
- [이미지 리소스 최적화](#optimizeimages)
- [시각적 트리 크기 줄이기](#visualtree)
- [애플리케이션 리소스 사전 크기 줄이기](#resourcedictionary)
- [사용자 지정 렌더러 패턴 사용](#rendererpattern)

> [!NOTE]
>  이 아티클을 읽기 전에 먼저 Xamarin 플랫폼을 사용하여 빌드된 애플리케이션의 메모리 사용 및 성능을 향상시키기 위한 비플랫폼 특정 기술에 대해 설명하는 [플랫폼 간 성능](~/cross-platform/deploy-test/memory-perf-best-practices.md)을 참조해야 합니다.

<a name="xamlc" />

## <a name="enable-the-xaml-compiler"></a>XAML 컴파일러 활성화

필요한 경우 XAML 컴파일러(XAMLC)를 사용하여 XAML을 중간 언어(IL)로 바로 컴파일할 수 있습니다. XAMLC는 여러 가지 이점을 제공합니다.

- 컴파일 시 XAML 검사를 수행하여 발견된 오류를 사용자에게 알립니다.
- XAML 요소의 일부 로드 및 인스턴스화 시간을 제거합니다.
- 더 이상 .xaml 파일을 포함하지 않아 최종 어셈블리의 파일 크기를 줄이는 데 도움이 됩니다.

XAMLC는 이전 버전과의 호환성을 위해 기본적으로 비활성화됩니다. 그러나 어셈블리와 클래스 수준에서 활성화할 수 있습니다. 자세한 내용은 [XAML 컴파일](~/xamarin-forms/xaml/xamlc.md)을 참조하세요.

<a name="correctlayout" />

## <a name="choose-the-correct-layout"></a>올바른 레이아웃 선택

여러 자식 요소를 표시할 수 있지만 단일 자식만 있어 불필요한 레이아웃입니다. 예를 들어 다음 코드 예제는 단일 자식이 있는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)을 보여줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <ContentPage.Content>
        <StackLayout>
            <Image Source="waterfront.jpg" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

이는 불필요하며, 다음 코드 예제에 나와 있는 것처럼 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 요소를 제거해야 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <ContentPage.Content>
        <Image Source="waterfront.jpg" />
    </ContentPage.Content>
</ContentPage>
```

또한 다른 레이아웃의 조합을 사용하여 특정 레이아웃의 모양을 재현하려고 하지 마세요. 불필요한 레이아웃 계산이 수행됩니다. 예를 들어 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 인스턴스의 조합을 사용하여 [`Grid`](xref:Xamarin.Forms.Grid) 레이아웃을 재현하려고 하지 마세요. 다음 코드 예제는 이러한 나쁜 사례의 예를 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" />
                <Entry Placeholder="Enter your name" />
            </StackLayout>
            <StackLayout Orientation="Horizontal">
                <Label Text="Age:" />
                <Entry Placeholder="Enter your age" />
            </StackLayout>
            <StackLayout Orientation="Horizontal">
                <Label Text="Occupation:" />
                <Entry Placeholder="Enter your occupation" />
            </StackLayout>
            <StackLayout Orientation="Horizontal">
                <Label Text="Address:" />
                <Entry Placeholder="Enter your address" />
            </StackLayout>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

불필요한 레이아웃 계산이 수행되기 때문에 이는 불필요합니다. 대신에 다음 코드 예제에 나와 있는 것처럼 [`Grid`](xref:Xamarin.Forms.Grid)를 사용하면 원하는 레이아웃을 더 잘 구현할 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <ContentPage.Content>
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="100" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="30" />
                <RowDefinition Height="30" />
                <RowDefinition Height="30" />
                <RowDefinition Height="30" />
            </Grid.RowDefinitions>
            <Label Text="Name:" />
            <Entry Grid.Column="1" Placeholder="Enter your name" />
            <Label Grid.Row="1" Text="Age:" />
            <Entry Grid.Row="1" Grid.Column="1" Placeholder="Enter your age" />
            <Label Grid.Row="2" Text="Occupation:" />
            <Entry Grid.Row="2" Grid.Column="1" Placeholder="Enter your occupation" />
            <Label Grid.Row="3" Text="Address:" />
            <Entry Grid.Row="3" Grid.Column="1" Placeholder="Enter your address" />
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

<a name="layoutcompression" />

## <a name="enable-layout-compression"></a>레이아웃 압축 활성화

레이아웃 압축은 페이지 렌더링 성능을 개선하기 위해 시각적 트리에서 지정된 레이아웃을 제거합니다. 이로 인해 얻을 수 있는 성능 이점은 페이지의 복잡성, 사용 중인 운영 체제의 버전, 그리고 응용 프로그램이 실행 중인 디바이스에 따라 다릅니다. 하지만 가장 큰 성능 이점은 이전 버전의 디바이스에서 볼 수 있습니다. 자세한 내용은 [레이아웃 압축](~/xamarin-forms/user-interface/layouts/layout-compression.md)을 참조하세요.

<a name="fastrenderers" />

## <a name="use-fast-renderers"></a>빠른 렌더러 사용

빠른 렌더러는 결과 네이티브 컨트롤 계층을 평면화하여 Android에서 Xamarin.Forms 컨트롤의 인플레이션 및 렌더링 비용을 줄여줍니다. 이는 더 적은 개체를 만들어 시각적 트리의 복잡성을 낮추고 메모리 사용량을 줄여 성능을 개선합니다. 자세한 내용은 [빠른 렌더러](~/xamarin-forms/internals/fast-renderers.md)를 참조하세요.

<a name="databinding" />

## <a name="reduce-unnecessary-bindings"></a>불필요한 바인딩 줄이기

정적으로 쉽게 설정할 수 있는 콘텐츠에 바인딩을 사용하지 마세요. 바인딩할 필요가 없는 데이터를 바인딩할 경우 어떠한 이점도 없습니다. 바인딩은 비용 효율적이지 않기 때문입니다. 예를 들어 `Button.Text = "Accept"`를 설정하는 것은 "Accept" 값을 사용하여 ViewModel `string` 속성에 [`Button.Text`](xref:Xamarin.Forms.Button.Text)를 바인딩하는 것보다 오버헤드가 적습니다.

<a name="optimizelayout" />

## <a name="optimize-layout-performance"></a>레이아웃 성능 최적화

Xamarin.Forms 2에는 레이아웃 업데이트에 영향을 주는 최적화된 레이아웃 엔진이 도입되었습니다. 최적의 레이아웃 성능을 얻으려면 다음 지침을 따르세요.

- 더 적은 래핑 뷰를 사용하여 레이아웃을 만들 수 있는 [`Margin`](xref:Xamarin.Forms.View.Margin) 속성 값을 지정하여 레이아웃 계층의 깊이를 줄입니다. 자세한 내용은 [여백 및 패딩](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)을 참조하세요.
- [`Grid`](xref:Xamarin.Forms.Grid)를 사용할 경우 가능한 한 적은 행과 열을 [`Auto`](xref:Xamarin.Forms.GridLength.Auto) 크기로 설정하도록 하세요. 자동으로 크기가 조정된 행이나 열은 레이아웃 엔진이 추가적인 레이아웃 계산을 수행하도록 합니다. 가능한 경우 고정된 크기의 행과 열을 대신 사용하세요. 또는 부모 트리가 이러한 레이아웃 지침을 따르는 경우 [`GridUnitType.Star`](xref:Xamarin.Forms.GridUnitType.Star) 열거 값을 사용하여 행과 열이 일정한 비율의 공간을 차지하도록 하세요.
- 필요하지 않은 경우 레이아웃의 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 및 [`HorizontalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성을 설정하지 마세요. [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) 및 [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)의 기본값은 최고의 레이아웃 최적화를 지원합니다. 이러한 속성을 변경하는 경우 기본값으로 설정하더라도 비용이 들고 메모리가 소모됩니다.
- 가능한 경우 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)을 사용하지 마세요. 그러면 CPU가 훨씬 더 많은 작업을 수행해야 합니다.
- [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)을 사용할 경우 가능하다면 [`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize) 속성을 사용하지 마세요.
- [`StackLayout`](xref:Xamarin.Forms.StackLayout)을 사용할 경우 하나의 자식만 [`LayoutOptions.Expands`](xref:Xamarin.Forms.LayoutOptions.Expands)로 설정되도록 합니다. 이 속성은 지정된 자식 요소가 `StackLayout`에서 허용하는 최대의 공간을 차지하도록 하며, 이는 이러한 계산이 두 번 이상 수행되도록 하여 불필요합니다.
- [`Layout`](xref:Xamarin.Forms.Layout) 클래스의 메서드를 호출하지 마세요. 비용이 많이 드는 레이아웃 계산이 수행됩니다. 대신에 [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) 및 [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) 속성을 설정하면 원하는 레이아웃 동작을 구현할 가능성이 높습니다. 또는 [`Layout<View>`](xref:Xamarin.Forms.Layout`1) 클래스를 서브클래스로 지정하여 원하는 레이아웃 동작을 구현할 수 있습니다.
- [`Label`](xref:Xamarin.Forms.Label) 인스턴스를 필요 이상으로 자주 업데이트하지 마세요. 레이블의 크기가 변경되면 전체 화면 레이아웃이 다시 계산될 수 있습니다.
- 필요하지 않은 경우 [`Label.VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) 속성을 설정하지 마세요.
- 가능할 때마다 [`Label`](xref:Xamarin.Forms.Label) 인스턴스의 [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)를 [`NoWrap`](xref:Xamarin.Forms.LineBreakMode.NoWrap)으로 설정합니다.

<a name="optimizelistview" />

## <a name="optimize-listview-performance"></a>ListView 성능 최적화

[`ListView`](xref:Xamarin.Forms.ListView) 컨트롤을 사용할 때 최적화해야 하는 여러 가지 사용자 환경이 있습니다.

- **초기화** - 컨트롤을 만들 때부터 항목이 화면에 표시될 때까지의 시간 간격입니다.
- **스크롤** - 목록을 스크롤하는 기능이며, UI가 터치 제스처보다 지연되지 않도록 합니다.
- **상호 작용** - 항목을 추가, 삭제 및 선택합니다.

[`ListView`](xref:Xamarin.Forms.ListView) 컨트롤을 사용하려면 애플리케이션이 데이터 및 셀 템플릿을 제공해야 합니다. 이것이 구현되는 방식은 컨트롤의 성능에 큰 영향을 줍니다. 자세한 내용은 [ListView 성능](~/xamarin-forms/user-interface/listview/performance.md)을 참조하세요.

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>이미지 리소스 최적화

이미지 리소스를 표시하면 앱의 메모리 사용 공간이 크게 증가할 수 있습니다. 따라서 필요한 경우에만 만들어야 하며, 애플리케이션에 더 이상 필요하지 않을 경우 즉시 해제되어야 합니다. 예를 들어 애플리케이션이 스트림에서 데이터를 읽어 이미지를 표시하는 경우, 필요할 경우에만 스트림이 생성되도록 하고, 더 이상 필요 없는 경우에는 스트림이 해제되도록 합니다. 이는 페이지가 생성될 때나 [`Page.Appearing`](xref:Xamarin.Forms.Page.Appearing) 이벤트가 실행될 때 스트림을 만든 후 [`Page.Disappearing`](xref:Xamarin.Forms.Page.Disappearing) 이벤트가 실행될 때 스트림을 폐기하는 방식으로 구현할 수 있습니다.

[`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) 메서드를 사용하여 표시할 이미지를 다운로드할 때는 [`UriImageSource.CachingEnabled`](xref:Xamarin.Forms.UriImageSource.CachingEnabled) 속성을 `true`로 설정하여 다운로드된 이미지를 캐시에 저장합니다. 자세한 내용은 [이미지 작업](~/xamarin-forms/user-interface/images.md)을 참조하세요.

자세한 내용은 [이미지 리소스 최적화](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages)를 참조하세요.

<a name="visualtree" />

## <a name="reduce-the-visual-tree-size"></a>시각적 트리 크기 줄이기

페이지의 요소 수를 줄이면 페이지 렌더러의 속도가 더 빨라집니다. 이는 크게 두 가지 방법으로 구현할 수 있습니다. 첫 번째는 표시되지 않는 요소를 숨기는 것입니다. 각 요소의 [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) 속성은 해당 요소가 시각적 트리의 일부가 되어야 하는지 여부를 결정합니다. 따라서 요소가 다른 요소 뒤에 숨겨져 있어 표시되지 않을 경우 해당 요소를 제거하거나 `IsVisible` 속성을 `false`로 설정합니다.

두 번째 방법은 불필요한 요소를 제거하는 것입니다. 예를 들어 다음 코드 예제는 일련의 [`Label`](xref:Xamarin.Forms.Label) 요소를 표시하는 페이지 레이아웃을 보여줍니다.

```xaml
<ContentPage.Content>
    <StackLayout>
        <StackLayout Padding="20,20,0,0">
            <Label Text="Hello" />
        </StackLayout>
        <StackLayout Padding="20,20,0,0">
            <Label Text="Welcome to the App!" />
        </StackLayout>
        <StackLayout Padding="20,20,0,0">
            <Label Text="Downloading Data..." />
        </StackLayout>
    </StackLayout>
</ContentPage.Content>
```

다음 코드 예제와 같이 더 적은 요소 수로 동일한 페이지 레이아웃을 유지 관리할 수 있습니다.

```xaml
<ContentPage.Content>
  <StackLayout Padding="20,20,0,0" Spacing="25">
    <Label Text="Hello" />
    <Label Text="Welcome to the App!" />
    <Label Text="Downloading Data..." />
  </StackLayout>
</ContentPage.Content>
```

<a name="resourcedictionary" />

## <a name="reduce-the-application-resource-dictionary-size"></a>애플리케이션 리소스 사전 크기 줄이기

애플리케이션에서 사용되는 모든 리소스는 중복을 피하기 위해 애플리케이션의 리소스 사전에 저장되어야 합니다. 이는 애플리케이션에서 구문 분석되어야 하는 XAML의 양을 줄이는 데 도움이 됩니다. 다음 코드 예제는 애플리케이션 전체에서 사용되므로 애플리케이션의 리소스 사전에 정의된 `HeadingLabelStyle` 리소스를 보여 줍니다.

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Resources.App">
     <Application.Resources>
         <ResourceDictionary>
            <Style x:Key="HeadingLabelStyle" TargetType="Label">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
         </ResourceDictionary>
     </Application.Resources>
</Application>
```

그러나 페이지에만 적용되는 XAML은 앱의 리소스 사전에 포함되어서는 안 됩니다. 그러면 애플리케이션 시작 시 페이지에서 요구할 경우 리소스가 대신 구문 분석되기 때문입니다. 시작 페이지가 아닌 페이지에서 사용하는 리소스는 애플리케이션이 시작될 때 구문 분석되는 XAML을 줄이기 위해 해당 페이지의 리소스 사전에 저장해야 합니다. 다음 코드 예제는 단일 페이지에만 있으므로 페이지의 리소스 사전에 정의된 `HeadingLabelStyle` 리소스를 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Test.HomePage"
             Padding="0,20,0,0">
     <ContentPage.Resources>
          <ResourceDictionary>
            <Style x:Key="HeadingLabelStyle" TargetType="Label">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
        </ResourceDictionary>
     </ContentPage.Resources>
    <ContentPage.Content>
      ...
    </ContentPage.Content>
</ContentPage>

```

애플리케이션 리소스에 대한 자세한 내용은 [`Working with Styles`](~/xamarin-forms/user-interface/styles/index.md)을 참조하세요.

<a name="rendererpattern" />

## <a name="use-the-custom-renderer-pattern"></a>사용자 지정 렌더러 패턴 사용

대부분의 렌더러 클래스는 해당하는 네이티브 컨트롤을 렌더링하기 위해 Xamarin.Forms 사용자 지정 컨트롤이 생성될 때 호출되는 `OnElementChanged` 메서드를 노출합니다. 그러면 각 플랫폼별 렌더러 클래스에서 사용자 정의 렌더러 클래스가 이 메서드를 재정의하여 네이티브 컨트롤을 인스턴스화하고 사용자 지정합니다. `SetNativeControl` 메서드가 네이티브 컨트롤을 인스턴스화하는 데 사용되며, 이 메서드는 또한 `Control` 속성에 컨트롤 참조를 할당합니다.

그러나 일부 경우에는 `OnElementChanged` 메서드가 여러 번 호출될 수 있습니다. 따라서 성능에 영향을 줄 수 있는 메모리 누수를 방지하려면 새로운 네이티브 컨트롤을 인스턴스화할 때 주의를 기울여야 합니다. 사용자 지정 렌더러에서 새 네이티브 컨트롤을 인스턴스화할 때 사용하는 방법은 다음 코드 예제에 표시되어 있습니다.

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    if (Control == null) {
      // Instantiate the native control
    }
    // Configure the control and subscribe to event handlers
  }
}
```

`Control` 속성이 `null`일 경우 새 네이티브 컨트롤은 한 번만 인스턴스화되어야 합니다. 또한 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결된 경우에만 컨트롤을 만들어서 구성하고 이벤트 처리기를 구독해야 합니다. 마찬가지로, 요소 렌더러가 변경된 항목에 연결된 경우 구독 대상 이벤트 처리기의 구독이 취소되어야만 합니다. 이러한 방식을 도입하면 메모리 누수의 영향을 받지 않으며 효율적으로 작동하는 사용자 지정 렌더러를 만들 수 있습니다.

> [!IMPORTANT]
> `e.NewElement`가 `null`이 아닌 경우에만 `SetNativeControl` 메서드를 호출해야 합니다.

사용자 지정 렌더러에 대한 자세한 내용은 [각 플랫폼에서 컨트롤 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)을 참조하세요.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Forms 애플리케이션의 성능을 높이는 기술에 대해 설명했습니다. 이러한 기술은 전체적으로 CPU에서 수행하는 작업의 양과 애플리케이션에서 소비하는 메모리의 양을 크게 줄일 수 있습니다.


## <a name="related-links"></a>관련 링크

- [플랫폼 간 성능](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [ListView 성능](~/xamarin-forms/user-interface/listview/performance.md)
- [빠른 렌더러](~/xamarin-forms/internals/fast-renderers.md)
- [레이아웃 압축](~/xamarin-forms/user-interface/layouts/layout-compression.md)
- [XamlCompilation](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)
- [XamlCompilationOptions](xref:Xamarin.Forms.Xaml.XamlCompilationOptions)
