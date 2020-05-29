---
title: 암시적 스타일Xamarin.Forms
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3fb6ea40ced93103ec9cc92fa707f68c674d7826
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139011"
---
# <a name="implicit-styles-in-xamarinforms"></a>암시적 스타일Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_암시적 스타일은 각 컨트롤이 스타일을 참조할 필요 없이 동일한 TargetType의 모든 컨트롤에서 사용 하는 스타일입니다._

## <a name="create-an-implicit-style-in-xaml"></a>XAML에서 암시적 스타일 만들기

페이지 수준에서을 선언 하려면를 [`Style`](xref:Xamarin.Forms.Style) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 페이지에 추가한 다음 하나 이상의 `Style` 선언을에 포함할 수 있습니다 `ResourceDictionary` . 는 `Style` 특성을 지정 하지 않고 *암시적* 으로 수행 됩니다 `x:Key` . 그런 다음 `TargetType` 값에서 파생 된 요소가 아니라 정확 하 게 일치 하는 시각적 요소에 스타일이 적용 됩니다 `TargetType` .

다음 코드 예제에서는 페이지의에 XAML에 선언 된 *암시적* 스타일을 보여 주고 `ResourceDictionary` 페이지의 인스턴스에 적용 합니다 [`Entry`](xref:Xamarin.Forms.Entry) .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Entry">
                <Setter Property="HorizontalOptions" Value="Fill" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                <Setter Property="BackgroundColor" Value="Yellow" />
                <Setter Property="FontAttributes" Value="Italic" />
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Entry Text="These entries" />
            <Entry Text="are demonstrating" />
            <Entry Text="implicit styles," />
            <Entry Text="and an implicit style override" BackgroundColor="Lime" TextColor="Red" />
            <local:CustomEntry Text="Subclassed Entry is not receiving the style" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 페이지의 인스턴스에 적용 되는 단일 *암시적* 스타일을 정의 합니다 [`Entry`](xref:Xamarin.Forms.Entry) . 는 `Style` 노란색 배경에 파란색 텍스트를 표시 하는 동시에 다른 모양 옵션도 설정 하는 데 사용 됩니다. 는 `Style` [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 특성을 지정 하지 않고 페이지에 추가 됩니다 `x:Key` . 따라서은 `Style` `Entry` 의 속성과 정확히 일치 하므로 모든 인스턴스에 암시적으로 적용 됩니다 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) `Style` . 그러나는 `Style` 서브클래싱된 인 인스턴스에 적용 되지 않습니다 `CustomEntry` `Entry` . 이로 인해 결국 다음 스크린샷에 표시된 모양이 됩니다.

[![암시적 스타일 예제](implicit-images/implicit-styles.png)](implicit-images/implicit-styles-large.png#lightbox)

또한 네 번째는 [`Entry`](xref:Xamarin.Forms.Entry) [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) [`TextColor`](xref:Xamarin.Forms.InputView.TextColor) 암시적 스타일의 및 속성을 서로 다른 값으로 재정의 합니다 `Color` .

### <a name="create-an-implicit-style-at-the-control-level"></a>컨트롤 수준에서 암시적 스타일 만들기

다음 코드 예제와 같이 페이지 수준에서 *암시적* 스타일을 만들 수 있을 뿐 아니라 컨트롤 수준 에서도 만들 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style TargetType="Entry">
                        <Setter Property="HorizontalOptions" Value="Fill" />
                        ...
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            <Entry Text="These entries" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

이 예제에서는 *암시적* [`Style`](xref:Xamarin.Forms.Style) 이 [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) 컨트롤의 컬렉션에 할당 됩니다 [`StackLayout`](xref:Xamarin.Forms.StackLayout) . 그런 다음 컨트롤 및 해당 자식에 *암시적* 스타일을 적용할 수 있습니다.

응용 프로그램에서 스타일을 만드는 방법에 대 한 자세한 내용은 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [전역 스타일](~/xamarin-forms/user-interface/styles/application.md)을 참조 하세요.

## <a name="create-an-implicit-style-in-c35"></a>C&#35;에서 암시적 스타일 만들기

[`Style`](xref:Xamarin.Forms.Style)[`Resources`](xref:Xamarin.Forms.VisualElement.Resources) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) `Style` `ResourceDictionary` 다음 코드 예제와 같이 새를 만든 다음 인스턴스를에 추가 하 여 c #의 페이지 컬렉션에 인스턴스를 추가할 수 있습니다.

```csharp
public class ImplicitStylesPageCS : ContentPage
{
    public ImplicitStylesPageCS ()
    {
        var entryStyle = new Style (typeof(Entry)) {
            Setters = {
                ...
                new Setter { Property = Entry.TextColorProperty, Value = Color.Blue }
            }
        };

        ...
        Resources = new ResourceDictionary ();
        Resources.Add (entryStyle);

        Content = new StackLayout {
            Children = {
                new Entry { Text = "These entries" },
                new Entry { Text = "are demonstrating" },
                new Entry { Text = "implicit styles," },
                new Entry { Text = "and an implicit style override", BackgroundColor = Color.Lime, TextColor = Color.Red },
                new CustomEntry  { Text = "Subclassed Entry is not receiving the style" }
            }
        };
    }
}
```

생성자는 페이지의 인스턴스에 적용 되는 단일 *암시적* 스타일을 정의 합니다 [`Entry`](xref:Xamarin.Forms.Entry) . 는 `Style` 노란색 배경에 파란색 텍스트를 표시 하는 동시에 다른 모양 옵션도 설정 하는 데 사용 됩니다. 는 `Style` [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 문자열을 지정 하지 않고 페이지에 추가 됩니다 `key` . 따라서은 `Style` `Entry` 의 속성과 정확히 일치 하므로 모든 인스턴스에 암시적으로 적용 됩니다 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) `Style` . 그러나는 `Style` 서브클래싱된 인 인스턴스에 적용 되지 않습니다 `CustomEntry` `Entry` .

## <a name="apply-a-style-to-derived-types"></a>파생 형식에 스타일 적용

[`Style.ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes)속성을 사용 하면 속성이 참조 하는 기본 형식에서 파생 된 컨트롤에 스타일을 적용할 수 있습니다 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) . 따라서 `true` 형식이 속성에 지정 된 기본 형식에서 파생 되는 경우이 속성을로 설정 하면 여러 형식을 대상으로 하는 단일 스타일을 사용할 수 있습니다 `TargetType` .

다음 예제에서는 인스턴스의 배경색을 빨간색으로 설정 하는 암시적 스타일 [`Button`](xref:Xamarin.Forms.Button) 을 보여 줍니다.

```xaml
<Style TargetType="Button"
       ApplyToDerivedTypes="True">
    <Setter Property="BackgroundColor"
            Value="Red" />
</Style>
```

이 스타일을 페이지 수준에 배치 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 하면 페이지의 모든 [`Button`](xref:Xamarin.Forms.Button) 인스턴스와에서 파생 되는 모든 컨트롤에 적용 됩니다 `Button` . 그러나 속성이 설정 되지 않은 상태로 유지 되는 경우 [`ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) 에는 해당 스타일만 `Button` 인스턴스에 적용 됩니다.

해당하는 C# 코드는 다음과 같습니다.

```csharp
var buttonStyle = new Style(typeof(Button))
{
    ApplyToDerivedTypes = true,
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.Red
        }
    }
};

Resources = new ResourceDictionary { buttonStyle };
```

## <a name="related-links"></a>관련 링크

- [XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [기본 스타일 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [스타일 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [스타일](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
