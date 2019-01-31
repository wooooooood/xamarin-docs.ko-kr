---
title: Xamarin.Forms에서 암시적 스타일
description: 암시적 스타일은 각 컨트롤에 스타일을 참조 하지 않고도 동일한 TargetType의 모든 컨트롤에서 사용 되는 하나입니다.
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
ms.openlocfilehash: 67b8bac62cacb091323d084e1c7cec9accc30844
ms.sourcegitcommit: 817d26585093cd180a36b28179eb354b0eb900b3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55291975"
---
# <a name="implicit-styles-in-xamarinforms"></a>Xamarin.Forms에서 암시적 스타일

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)

_암시적 스타일은 각 컨트롤에 스타일을 참조 하지 않고도 동일한 TargetType의 모든 컨트롤에서 사용 되는 하나입니다._

## <a name="create-an-implicit-style-in-xaml"></a>XAML에서 암시적 스타일 만들기

선언 하는 [ `Style` ](xref:Xamarin.Forms.Style) 페이지 수준에 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 페이지 및 다음 하나 이상의 추가 해야 합니다 `Style` 선언에 포함 될 수는 `ResourceDictionary`. A `Style` 이루어집니다 *암시적* 지정 하지는 `x:Key` 특성입니다. 스타일 다음 일치 하는 시각적 요소에 적용할 합니다 `TargetType` 정확 하 게 아니라에서 파생 된 요소에는 `TargetType` 값입니다.

다음 코드 예제는 *암시적* 페이지의 XAML에 선언 된 스타일 `ResourceDictionary`, 페이지에 적용 하 고 [ `Entry` ](xref:Xamarin.Forms.Entry) 인스턴스:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" Icon="xaml.png">
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

합니다 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 단일 정의 *암시적* 페이지에 적용 되는 스타일 [ `Entry` ](xref:Xamarin.Forms.Entry) 인스턴스. `Style` 도 다른 모양 옵션을 설정 하는 동안 노란색 배경이에 파란색 텍스트를 표시 하는 데 사용 됩니다. 합니다 `Style` 페이지에 추가 됩니다 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 지정 하지 않고는 `x:Key` 특성입니다. 따라서 합니다 `Style` 모두에 적용 됩니다는 `Entry` 일치 하는 암시적으로 인스턴스를 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) 의 속성을 `Style` 정확 하 게 합니다. 그러나 합니다 `Style` 에 적용 되지 않습니다 합니다 `CustomEntry` 서브클래싱된는 인스턴스 `Entry`합니다. 이 인해 다음 스크린샷에 표시 된 모양:

[![](implicit-images/implicit-styles.png "암시적 스타일 예제")](implicit-images/implicit-styles-large.png#lightbox "암시적 스타일 예제")

또한, 네 번째 [ `Entry` ](xref:Xamarin.Forms.Entry) 재정의 [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) 고 [ `TextColor` ](xref:Xamarin.Forms.Entry.TextColor) 다른 암시적스타일의속성`Color`값입니다.

### <a name="create-an-implicit-style-at-the-control-level"></a>암시적 스타일 컨트롤 수준에서 만들기

외에도 *암시적* 페이지 수준에서 스타일도 만들 수 있습니다 제어 수준에서 다음 코드 예제 에서처럼:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" Icon="xaml.png">
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

이 예제에서는 *암시적* [ `Style` ](xref:Xamarin.Forms.Style) 에 할당 되는 [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) 의 컬렉션을 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)컨트롤입니다. 합니다 *암시적* 스타일 컨트롤과 해당 자식 컨트롤에 적용할 수 있습니다.

응용 프로그램에서 스타일을 만드는 방법은 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)를 참조 하십시오 [글로벌 스타일](~/xamarin-forms/user-interface/styles/application.md).

## <a name="create-an-implicit-style-in-c35"></a>C에서 암시적 스타일 만들기&#35;

[`Style`](xref:Xamarin.Forms.Style) 인스턴스 페이지를 추가할 수 있습니다 [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) 새 C#에서 컬렉션 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), 추가 하 여 다음을 `Style` 인스턴스를 `ResourceDictionary`에서처럼는 다음 코드 예제:

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

생성자 정의 단일 *암시적* 페이지에 적용 되는 스타일 [ `Entry` ](xref:Xamarin.Forms.Entry) 인스턴스. `Style` 도 다른 모양 옵션을 설정 하는 동안 노란색 배경이에 파란색 텍스트를 표시 하는 데 사용 됩니다. 합니다 `Style` 페이지에 추가 됩니다 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 지정 하지 않고는 `key` 문자열입니다. 따라서 합니다 `Style` 모두에 적용 됩니다는 `Entry` 일치 하는 암시적으로 인스턴스를 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) 의 속성을 `Style` 정확 하 게 합니다. 그러나 합니다 `Style` 에 적용 되지 않습니다 합니다 `CustomEntry` 서브클래싱된는 인스턴스 `Entry`합니다.

## <a name="apply-a-style-to-derived-types"></a>파생된 형식에 스타일 적용

합니다 [ `Style.ApplyToDerivedTypes` ](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) 속성을 참조 하는 기본 형식에서 파생 되는 컨트롤에 적용할 스타일을 사용 하면 합니다 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) 속성입니다. 따라서이 속성을 설정 `true` 형식에 지정 된 기본 형식에서 파생 된 여러 형식을 대상으로 단일 스타일을 사용 하도록 설정 된 `TargetType` 속성입니다.

다음 예제에서는의 배경색을 설정 하는 암시적 스타일을 보여 줍니다 [ `Button` ](xref:Xamarin.Forms.Button) 빨간색 인스턴스:

```xaml
<Style TargetType="Button"
       ApplyToDerivedTypes="True">
    <Setter Property="BackgroundColor"
            Value="Red" />
</Style>
```

이 스타일을 페이지 수준에 배치 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 모두에 적용 되 고 하면 [ `Button` ](xref:Xamarin.Forms.Button) 페이지 및 컨트롤에서 파생 되는 인스턴스 `Button`합니다. 그러나 경우 합니다 [ `ApplyToDerivedTypes` ](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) 속성으로 설정 되지 않은 남아, 스타일에만 적용 됩니다 `Button` 인스턴스.

해당 하는 C# 코드가입니다.

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

- [XAML 마크업 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [기본 스타일 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [스타일 (샘플)를 사용 하 여 작업](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [스타일](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
