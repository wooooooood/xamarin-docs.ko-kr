---
title: Xamarin.Forms에 명시적 스타일
description: 명시적 스타일의 스타일 속성을 설정 하 여 선택적으로 컨트롤에 적용 되는 경우 이 문서에서는 Xamarin.Forms 응용 프로그램에 명시적 스타일을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: C0DF9F8F-B431-4374-A574-325BC3C41A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: fcbdeac5ebceccddee68fcca635a3935944ecac8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61394965"
---
# <a name="explicit-styles-in-xamarinforms"></a>Xamarin.Forms에 명시적 스타일

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)

_명시적 스타일의 스타일 속성을 설정 하 여 선택적으로 컨트롤에 적용 되는 경우_

## <a name="create-an-explicit-style-in-xaml"></a>XAML에 명시적 스타일 만들기

선언 하는 [ `Style` ](xref:Xamarin.Forms.Style) 페이지 수준에 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 페이지 및 다음 하나 이상의 추가 해야 합니다 `Style` 선언에 포함 될 수는 `ResourceDictionary`. A `Style` 이루어집니다 *명시적* 선언을 제공 하 여는 `x:Key` 특성에 설명이 포함 된 키를 제공 하는 `ResourceDictionary`. *명시적* 스타일 다음에 적용 해야 특정 한 시각적 요소를 설정 하 여 해당 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) 속성입니다.

다음 코드 예와 *명시적* 페이지의 XAML에 선언 된 스타일 `ResourceDictionary` 페이지에 적용 하 고 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" Icon="xaml.png">
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

합니다 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 3 정의 *명시적* 페이지에 적용 되는 스타일 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스. 각 `Style` 도 글꼴 크기 및 가로 및 세로 레이아웃 옵션을 설정 하는 동안 다른 색으로 텍스트를 표시 하는 데 사용 됩니다. 각 `Style` 다른 적용 됩니다 `Label` 설정 하 여 해당 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) 사용 하 여 속성을 `StaticResource` 태그 확장 합니다. 이 인해 다음 스크린샷에 표시 된 모양:

[![](explicit-images/explicit-styles.png "명시적 스타일 예제")](explicit-images/explicit-styles-large.png#lightbox "명시적 스타일 예제")

또한 최종 [ `Label` ](xref:Xamarin.Forms.Label) 에 [ `Style` ](xref:Xamarin.Forms.Style) 적용 하지만 또한 재정의 [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) 속성을 다른 `Color`값입니다.

### <a name="create-an-explicit-style-at-the-control-level"></a>제어 수준에 명시적 스타일 만들기

외에도 *명시적* 페이지 수준에서 스타일도 만들 수 있습니다 제어 수준에서 다음 코드 예제 에서처럼:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" Icon="xaml.png">
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

이 예제에서는 *명시적* [ `Style` ](xref:Xamarin.Forms.Style) 에 할당 된 인스턴스를 [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) 의 컬렉션을 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 제어 합니다. 그런 다음 컨트롤과 해당 자식 컨트롤에 스타일을 적용할 수 있습니다.

응용 프로그램에서 스타일을 만드는 방법은 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)를 참조 하십시오 [글로벌 스타일](~/xamarin-forms/user-interface/styles/application.md).

## <a name="create-an-explicit-style-in-c35"></a>C에서 명시적 스타일 만들기&#35;

[`Style`](xref:Xamarin.Forms.Style) 인스턴스 페이지를 추가할 수 있습니다 [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) 새 C#에서 컬렉션 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), 추가 하 여 다음을 `Style` 인스턴스를 `ResourceDictionary`에서처럼는 다음 코드 예제:

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

생성자 정의 세 *명시적* 페이지에 적용 되는 스타일 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스. 각 *명시적* [ `Style` ](xref:Xamarin.Forms.Style) 에 추가 되는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 를 사용 하 여를 [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) 메서드를 지정 하는 `key` 를 참조 하는 문자열을 `Style` 인스턴스. 각 `Style` 다른 적용 됩니다 `Label` 설정 하 여 해당 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) 속성입니다.

그러나은 아무 이점이 없습니다 사용 하는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 여기 있습니다. 대신 [ `Style` ](xref:Xamarin.Forms.Style) 인스턴스를 직접 할당할 수 있습니다 합니다 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) 필요한 시각적 요소의 속성 및 `ResourceDictionary` 다음에 표시 된 대로 제거할 수 있습니다 코드 예제:

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

생성자 정의 세 *명시적* 페이지에 적용 되는 스타일 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스. 각 `Style` 도 글꼴 크기 및 가로 및 세로 레이아웃 옵션을 설정 하는 동안 다른 색으로 텍스트를 표시 하는 데 사용 됩니다. 각 `Style` 다른 적용 됩니다 `Label` 설정 하 여 해당 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) 속성입니다. 또한 최종 `Label` 에 `Style` 적용 하지만 또한 재정의 `TextColor` 속성을 다른 `Color` 값입니다.

## <a name="related-links"></a>관련 링크

- [XAML 마크업 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [기본 스타일 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [스타일 (샘플)를 사용 하 여 작업](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [스타일](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
