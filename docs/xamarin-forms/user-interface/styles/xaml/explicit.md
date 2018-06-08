---
title: 명시적 스타일
description: 명시적 스타일은 해당 스타일 속성을 설정 하 여 선택적으로 컨트롤에 적용 됩니다.
ms.prod: xamarin
ms.assetid: C0DF9F8F-B431-4374-A574-325BC3C41A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 9f9e87ae0fd9d609cef56123e9052d85941bda51
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848189"
---
# <a name="explicit-styles"></a>명시적 스타일

_명시적 스타일은 해당 스타일 속성을 설정 하 여 선택적으로 컨트롤에 적용 됩니다._

## <a name="creating-an-explicit-style-in-xaml"></a>XAML에 명시적 스타일 만들기

선언 하는 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 페이지 수준에서 한 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 페이지 다음 하나 이상의에 추가 해야 `Style` 선언에 포함 될 수는 `ResourceDictionary`합니다. A `Style` 이루어집니다 *명시적* 선언 제공 하 여는 `x:Key` 특성에 설명이 포함 된 키를 제공 하는 `ResourceDictionary`합니다. *명시적* 스타일 다음에 적용 해야 특정 시각적 요소를 설정 하 여 자신의 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) 속성입니다.

다음 코드 예제와 *명시적* 스타일 페이지의 XAML에 선언 된 `ResourceDictionary` 는 페이지에 적용 하 고 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 인스턴스:

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

[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 세 정의 *명시적* 스타일의 페이지에 적용 되는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 인스턴스. 각 `Style` 도 글꼴 크기 및 가로 및 세로 레이아웃 옵션을 설정 하는 동안 다른 색으로 텍스트를 표시 하는 데 사용 됩니다. 각 `Style` 다른 적용 `Label` 설정 하 여 해당 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) 사용 하 여 속성의 `StaticResource` 태그 확장 합니다. 다음 스크린샷에 표시 된 모양 결과이 됩니다.

[![](explicit-images/explicit-styles.png "명시적 스타일 예제")](explicit-images/explicit-styles-large.png#lightbox "명시적 스타일 예제")

또한 최종 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 에 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 적용 하지만 재정의 [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) 속성을 다른 `Color`값입니다.

### <a name="creating-an-explicit-style-at-the-control-level"></a>컨트롤에 명시적 스타일 수준 만들기

생성 될 뿐만 아니라 *명시적* 페이지 수준에서 스타일을 만들 수 있습니다도 제어 수준에 다음 코드 예제에 나와 있는 것 처럼:

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

이 예제에서는 *명시적* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 에 할당 된 인스턴스는 [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) 의 컬렉션은 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 제어 합니다. 그런 다음 컨트롤과 해당 자식 컨트롤에 스타일을 적용할 수 있습니다.

응용 프로그램에서 스타일을 만드는 방법은 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), 참조 [글로벌 스타일](~/xamarin-forms/user-interface/styles/application.md)합니다.

## <a name="creating-an-explicit-style-in-c35"></a>C에서 명시적 스타일 만들기&#35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 인스턴스는 페이지에 추가할 수 있습니다 [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) 새 C#에서 컬렉션 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), 고 추가 하 여 다음의 `Style` 인스턴스는 `ResourceDictionary`에 나타난 것 처럼는 다음 코드 예제

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

세 개의 생성자 정의 *명시적* 스타일의 페이지에 적용 되는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 인스턴스. 각 *명시적* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 에 추가 되는 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 를 사용 하는 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) 메서드를 지정 하는 `key` 을 가리키는 문자열은 `Style` 인스턴스. 각 `Style` 다른 적용 `Label` 설정 하 여 자신의 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) 속성입니다.

그러나 사용 하는 장점이 있습니다. 한 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 여기 합니다. 대신, [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 인스턴스에 직접 할당할 수는 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) 필요한 시각적 요소의 속성 및 `ResourceDictionary` 다음에 표시 된 대로 제거할 수 있습니다 코드 예:

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

세 개의 생성자 정의 *명시적* 스타일의 페이지에 적용 되는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 인스턴스. 각 `Style` 도 글꼴 크기 및 가로 및 세로 레이아웃 옵션을 설정 하는 동안 다른 색으로 텍스트를 표시 하는 데 사용 됩니다. 각 `Style` 다른 적용 `Label` 설정 하 여 해당 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) 속성입니다. 또한 최종 `Label` 에 `Style` 적용 하지만 재정의 `TextColor` 속성을 다른 `Color` 값입니다.

## <a name="summary"></a>요약

A [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 이루어집니다 *명시적* 선언 제공 하 여 프로그램 `x:Key` 특성을 선택한 다음 선택적으로 컨트롤에 적용을 설정 하 여 자신의 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) 속성입니다.



## <a name="related-links"></a>관련 링크

- [XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [기본 스타일 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [스타일 (샘플) 작업을](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [스타일](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
