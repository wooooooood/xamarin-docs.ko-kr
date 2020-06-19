---
title: 명시적 스타일Xamarin.Forms
description: 명시적 스타일은 스타일 속성을 설정 하 여 컨트롤에 선택적으로 적용 되는 스타일입니다. 이 문서에서는 응용 프로그램에서 명시적 스타일을 사용 하는 방법을 설명 Xamarin.Forms 합니다.
ms.prod: xamarin
ms.assetid: C0DF9F8F-B431-4374-A574-325BC3C41A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 62b84a5028c17c28a69a887a832028c2064fa78d
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136268"
---
# <a name="explicit-styles-in-xamarinforms"></a>명시적 스타일Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_명시적 스타일은 스타일 속성을 설정 하 여 컨트롤에 선택적으로 적용 되는 스타일입니다._

## <a name="create-an-explicit-style-in-xaml"></a>XAML에서 명시적 스타일 만들기

페이지 수준에서을 선언 하려면를 [`Style`](xref:Xamarin.Forms.Style) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 페이지에 추가한 다음 하나 이상의 `Style` 선언을에 포함할 수 있습니다 `ResourceDictionary` . 는의 `Style` 선언에 특성을 제공 하 여 *명시적* 으로 지정 됩니다 .이 특성은 `x:Key` 에 대 한 설명 키를 제공 `ResourceDictionary` 합니다. 그런 다음 해당 속성을 설정 하 여 *명시적* 스타일을 특정 시각적 요소에 적용 해야 합니다 [`Style`](xref:Xamarin.Forms.NavigableElement.Style) .

다음 코드 예제에서는 페이지의에 XAML에 선언 된 *명시적* 스타일을 보여 주고 `ResourceDictionary` 페이지의 인스턴스에 적용 합니다 [`Label`](xref:Xamarin.Forms.Label) .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="labelRedStyle" TargetType="Label">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
            <Style x:Key="labelGreenStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Green" />
            </Style>
            <Style x:Key="labelBlueStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelRedStyle}" />
            <Label Text="are demonstrating"
                   Style="{StaticResource labelGreenStyle}" />
            <Label Text="explicit styles,"
                   Style="{StaticResource labelBlueStyle}" />
            <Label Text="and an explicit style override"
                   Style="{StaticResource labelBlueStyle}"
                   TextColor="Teal" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 페이지의 인스턴스에 적용 되는 세 가지 *명시적* 스타일을 정의 합니다 [`Label`](xref:Xamarin.Forms.Label) . 각 `Style` 는 텍스트를 다른 색으로 표시 하는 데 사용 되 고 글꼴 크기 및 가로 및 세로 레이아웃 옵션도 설정 합니다. 각 `Style` 은 `Label` [`Style`](xref:Xamarin.Forms.NavigableElement.Style) 태그 확장을 사용 하 여 해당 속성을 설정 하 여 서로 다른에 적용 됩니다 `StaticResource` . 이로 인해 결국 다음 스크린샷에 표시된 모양이 됩니다.

[![명시적 스타일 예제](explicit-images/explicit-styles.png)](explicit-images/explicit-styles-large.png#lightbox)

또한 최종에는가 [`Label`](xref:Xamarin.Forms.Label) [`Style`](xref:Xamarin.Forms.Style) 적용 되지만 [`TextColor`](xref:Xamarin.Forms.Label.TextColor) 속성은 다른 값으로 재정의 됩니다 `Color` .

### <a name="create-an-explicit-style-at-the-control-level"></a>컨트롤 수준에서 명시적 스타일 만들기

다음 코드 예제와 같이 페이지 수준에서 *명시적* 스타일을 만들 수 있을 뿐 아니라 컨트롤 수준 에서도 만들 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelRedStyle" TargetType="Label">
                      ...
                    </Style>
                    ...
                </ResourceDictionary>
            </StackLayout.Resources>
            <Label Text="These labels" Style="{StaticResource labelRedStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

이 예제에서 *명시적* [`Style`](xref:Xamarin.Forms.Style) 인스턴스는 [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) 컨트롤의 컬렉션에 할당 됩니다 [`StackLayout`](xref:Xamarin.Forms.StackLayout) . 그런 다음 컨트롤 및 해당 자식에 스타일을 적용할 수 있습니다.

응용 프로그램에서 스타일을 만드는 방법에 대 한 자세한 내용은 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [전역 스타일](~/xamarin-forms/user-interface/styles/application.md)을 참조 하세요.

## <a name="create-an-explicit-style-in-c35"></a>C&#35;에서 명시적 스타일 만들기

[`Style`](xref:Xamarin.Forms.Style)[`Resources`](xref:Xamarin.Forms.VisualElement.Resources) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) `Style` `ResourceDictionary` 다음 코드 예제와 같이 새를 만든 다음 인스턴스를에 추가 하 여 c #의 페이지 컬렉션에 인스턴스를 추가할 수 있습니다.

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Red    }
            }
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Green }
            }
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Blue }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("labelRedStyle", labelRedStyle);
        Resources.Add ("labelGreenStyle", labelGreenStyle);
        Resources.Add ("labelBlueStyle", labelBlueStyle);
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels",
                            Style = (Style)Resources ["labelRedStyle"] },
                new Label { Text = "are demonstrating",
                            Style = (Style)Resources ["labelGreenStyle"] },
                new Label { Text = "explicit styles,",
                            Style = (Style)Resources ["labelBlueStyle"] },
                new Label {    Text = "and an explicit style override",
                            Style = (Style)Resources ["labelBlueStyle"], TextColor = Color.Teal }
            }
        };
    }
}
```

생성자는 페이지의 인스턴스에 적용 되는 세 가지 *명시적* 스타일을 정의 합니다 [`Label`](xref:Xamarin.Forms.Label) . 각 *명시적* [`Style`](xref:Xamarin.Forms.Style) 은 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 메서드를 사용 하 여 [`Add`](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) 인스턴스를 참조 하는 문자열을 지정 하는에 추가 됩니다 `key` `Style` . 각 `Style` 은 해당 속성을 설정 하 여 서로 다른에 적용 됩니다 `Label` [`Style`](xref:Xamarin.Forms.NavigableElement.Style) .

그러나 여기서를 사용 하는 이점은 없습니다 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . 대신, [`Style`](xref:Xamarin.Forms.Style) [`Style`](xref:Xamarin.Forms.NavigableElement.Style) 다음 코드 예제와 같이 인스턴스를 필요한 시각적 요소의 속성에 직접 할당 하 고을 `ResourceDictionary` 제거할 수 있습니다.

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            ...
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            ...
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            ...
        };
        ...
        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelRedStyle },
                new Label { Text = "are demonstrating", Style = labelGreenStyle },
                new Label { Text = "explicit styles,", Style = labelBlueStyle },
                new Label { Text = "and an explicit style override", Style = labelBlueStyle,
                            TextColor = Color.Teal }
            }
        };
    }
}
```

생성자는 페이지의 인스턴스에 적용 되는 세 가지 *명시적* 스타일을 정의 합니다 [`Label`](xref:Xamarin.Forms.Label) . 각 `Style` 는 텍스트를 다른 색으로 표시 하는 데 사용 되 고 글꼴 크기 및 가로 및 세로 레이아웃 옵션도 설정 합니다. 각 `Style` 은 해당 속성을 설정 하 여 서로 다른에 적용 됩니다 `Label` [`Style`](xref:Xamarin.Forms.NavigableElement.Style) . 또한 최종에는가 `Label` `Style` 적용 되지만 `TextColor` 속성은 다른 값으로 재정의 됩니다 `Color` .

## <a name="related-links"></a>관련 링크

- [XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [기본 스타일 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [스타일 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
