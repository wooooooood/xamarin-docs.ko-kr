---
title: "스타일 상속"
description: "스타일을 중복을 줄이고 다시 사용할 수 있도록 다른 스타일에서 상속할 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 67A3A39C-8CC0-446D-8162-FFA73582D3B8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: e57f19d1eb66e22badb418d4584f5654904c7ade
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="style-inheritance"></a>스타일 상속

_스타일을 중복을 줄이고 다시 사용할 수 있도록 다른 스타일에서 상속할 수 있습니다._

## <a name="style-inheritance-in-xaml"></a>XAML의 스타일 상속

스타일 상속을 설정 하 여 수행 된 [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) 속성을 기존 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)합니다. Xaml에서는 이렇게 설정 하 여는 `BasedOn` 속성을 한 `StaticResource` 이전에 만든 참조 하는 태그 확장 `Style`합니다. C#에서는 이렇게 설정 하 여는 `BasedOn` 속성을는 `Style` 인스턴스.

기본 스타일에서 상속 하는 스타일에는 수 [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) 새 속성에 대 한 인스턴스 또는 기본 스타일을 스타일을에서 재정의 하는 데 사용할 합니다. 또한 기본 스타일에서 상속 하는 스타일에는 동일한 형식 또는 기본 스타일의 대상 형식에서 파생 되는 형식을 대상 해야 합니다. 예를 들어 한 기본 스타일 대상 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) 스타일 기본 스타일을 기반으로 하는 대상 인스턴스, `View` 인스턴스 또는 형식에서 파생 되는 `View` 클래스 같은 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 및 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) 인스턴스.

다음 코드에서는 *명시적* XAML 페이지에 스타일을 상속 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
            </Style>
            <Style x:Key="labelStyle" TargetType="Label"
                   BasedOn="{StaticResource baseStyle}">
                ...
                <Setter Property="TextColor" Value="Teal" />
            </Style>
            <Style x:Key="buttonStyle" TargetType="Button"
                   BasedOn="{StaticResource baseStyle}">
                <Setter Property="BorderColor" Value="Lime" />
                ...
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelStyle}" />
            ...
            <Button Text="So is the button"
                    Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

`baseStyle` 대상 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) 인스턴스가 집합과 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) 및 [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) 속성입니다. `baseStyle` 모든 컨트롤에 직접 설정 되지 않았습니다. 대신, `labelStyle` 및 `buttonStyle` 바인딩할 수 있는 추가 속성 값을 설정, 여기에서 상속 합니다. `labelStyle` 및 `buttonStyle` 그런 다음에 적용 되는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 인스턴스 및 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) 인스턴스를 설정 하 여 자신의 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) 속성입니다. 다음 스크린샷에 표시 된 모양 결과이 됩니다.

[![](inheritance-images/style-inheritance.png)](inheritance-images/style-inheritance-large.png#lightbox)

> [!NOTE]
> 암시적 스타일 명시적 스타일에서 파생 될 수 있지만 명시적 스타일 암시적 스타일에서 파생 될 수 없습니다.

### <a name="respecting-the-inheritance-chain"></a>상속 체인을 유지합니다.

스타일을 스타일 같은 수준 이상에서 에서만 상속할 수 뷰 계층 구조에서. 이는 다음을 의미합니다.

- 다른 응용 프로그램 수준 리소스에서 응용 프로그램 수준의 리소스 에서만 상속할 수 있습니다.
- 페이지 수준 리소스는 응용 프로그램 수준 리소스 및 기타 페이지 수준 리소스에서 상속할 수 있습니다.
- 제어 수준 리소스는 응용 프로그램 수준 리소스, 페이지 수준 리소스 및 기타 컨트롤 수준 리소스에서 상속할 수 있습니다.

이 상속 체인에 대해서는 다음 코드 예제에서 설명 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelStyle" TargetType="Label" BasedOn="{StaticResource baseStyle}">
                      ...
                    </Style>
                    <Style x:Key="buttonStyle" TargetType="Button" BasedOn="{StaticResource baseStyle}">
                      ...
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

이 예제에서는 `labelStyle` 및 `buttonStyle` 제어 수준 리소스는 동안 `baseStyle` 은 페이지 수준 리소스입니다. 그러나 `labelStyle` 및 `buttonStyle` 에서 상속 `baseStyle`에 대 한 수는 없습니다 `baseStyle` 상속할 `labelStyle` 또는 `buttonStyle`뷰 계층 구조에서 해당 위치로 인해 합니다.

## <a name="style-inheritance-in-c35"></a>C에서 스타일 상속&#35;

해당 하는 C# 페이지, 여기서 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 인스턴스 게 직접 할당 된는 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) 필요한 컨트롤 속성의 다음 코드 예제에 표시 됩니다:

```csharp
public class StyleInheritancePageCS : ContentPage
{
    public StyleInheritancePageCS ()
    {
        var baseStyle = new Style (typeof(View)) {
            Setters = {
                new Setter {
                    Property = View.HorizontalOptionsProperty, Value = LayoutOptions.Center },
                ...
            }
        };

        var labelStyle = new Style (typeof(Label)) {
            BasedOn = baseStyle,
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Teal }
            }
        };

        var buttonStyle = new Style (typeof(Button)) {
            BasedOn = baseStyle,
            Setters = {
                new Setter { Property = Button.BorderColorProperty, Value = Color.Lime },
                ...
            }
        };
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelStyle },
                ...
                new Button { Text = "So is the button", Style = buttonStyle }
            }
        };
    }
}
```

`baseStyle` 대상 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) 인스턴스가 집합과 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) 및 [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) 속성입니다. `baseStyle` 모든 컨트롤에 직접 설정 되지 않았습니다. 대신, `labelStyle` 및 `buttonStyle` 바인딩할 수 있는 추가 속성 값을 설정, 여기에서 상속 합니다. `labelStyle` 및 `buttonStyle` 그런 다음에 적용 되는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 인스턴스 및 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) 인스턴스를 설정 하 여 자신의 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) 속성입니다.

## <a name="summary"></a>요약

스타일을 중복을 줄이고 다시 사용할 수 있도록 다른 스타일에서 상속할 수 있습니다. 스타일 상속을 설정 하 여 수행 된 [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) 속성을 기존 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)합니다.


## <a name="related-links"></a>관련 링크

- [XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [기본 스타일 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [스타일 (샘플) 작업을](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Style](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
