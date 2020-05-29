---
title: 스타일 소개 Xamarin.Forms
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
ms.openlocfilehash: 5766af7da3a0cf550a2ccb3a926dad25fd7962eb
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138825"
---
# <a name="introduction-to-xamarinforms-styles"></a>스타일 소개 Xamarin.Forms

_스타일을 사용 하면 시각적 요소의 모양을 사용자 지정할 수 있습니다. 스타일은 특정 형식에 대해 정의 되며 해당 형식에 사용할 수 있는 속성에 대 한 값을 포함 합니다._

Xamarin.Forms응용 프로그램에는 동일한 모양의 여러 컨트롤이 포함 되어 있는 경우가 많습니다. 예를 들어 [`Label`](xref:Xamarin.Forms.Label) 다음 XAML 코드 예제에 표시 된 것 처럼 응용 프로그램에 동일한 글꼴 옵션 및 레이아웃 옵션을 가진 인스턴스가 여러 개 있을 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="Styles.NoStylesPage"
    Title="No Styles"
    IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="are not"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="using styles"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

다음 코드 예제에서는 C#에서 만든 해당 페이지를 보여 줍니다.

```csharp
public class NoStylesPageCS : ContentPage
{
    public NoStylesPageCS ()
    {
        Title = "No Styles";
        IconImageSource = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label {
                    Text = "These labels",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "are not",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "using styles",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                }
            }
        };
    }
}
```

각 [`Label`](xref:Xamarin.Forms.Label) 인스턴스에는에 표시 되는 텍스트의 모양을 제어 하는 동일한 속성 값이 있습니다 `Label` . 이로 인해 결국 다음 스크린샷에 표시된 모양이 됩니다.

[![스타일이 없는 레이블 모양](introduction-images/no-styles.png)](introduction-images/no-styles-large.png#lightbox)

각 개별 컨트롤의 모양을 반복적으로 설정 하면 오류가 발생할 수 있습니다. 대신 모양을 정의 하는 스타일을 만든 다음 필요한 컨트롤에 적용할 수 있습니다.

## <a name="create-a-style"></a>스타일 만들기

[`Style`](xref:Xamarin.Forms.Style)클래스는 속성 값 컬렉션을 하나의 개체로 그룹화 한 다음 여러 시각적 요소 인스턴스에 적용할 수 있습니다. 이렇게 하면 반복적인 태그를 줄이고 응용 프로그램의 모양을 보다 쉽게 변경할 수 있습니다.

스타일은 주로 XAML 기반 응용 프로그램용으로 설계 되었지만 c #에서 만들 수도 있습니다.

- [`Style`](xref:Xamarin.Forms.Style)XAML로 생성 된 인스턴스는 일반적으로 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) 컨트롤, 페이지 또는 [`Resources`](xref:Xamarin.Forms.Application.Resources) 응용 프로그램의 컬렉션에 할당 된에 정의 됩니다.
- [`Style`](xref:Xamarin.Forms.Style)c #에서 만든 인스턴스는 일반적으로 페이지의 클래스 또는 전역으로 액세스할 수 있는 클래스에서 정의 됩니다.

사용할 수 있는 경우 영향을 정의 하는 위치 선택 [`Style`](xref:Xamarin.Forms.Style) :

- [`Style`](xref:Xamarin.Forms.Style)컨트롤 수준에서 정의 된 인스턴스는 컨트롤과 해당 자식에만 적용할 수 있습니다.
- [`Style`](xref:Xamarin.Forms.Style)페이지 수준에서 정의 된 인스턴스는 페이지 및 해당 자식에만 적용할 수 있습니다.
- [`Style`](xref:Xamarin.Forms.Style)응용 프로그램 수준에서 정의 된 인스턴스는 응용 프로그램 전체에 적용 될 수 있습니다.

각 [`Style`](xref:Xamarin.Forms.Style) 인스턴스는 하나 이상의 개체 컬렉션을 포함 하며 [`Setter`](xref:Xamarin.Forms.Setter) 각 개체에 `Setter` 는 [`Property`](xref:Xamarin.Forms.Setter.Property) 및가 있습니다 [`Value`](xref:Xamarin.Forms.Setter.Value) . `Property`는 스타일이 적용된 요소의 바인딩 가능한 속성 이름이며 `Value`는 속성에 적용되는 값입니다.

각 [`Style`](xref:Xamarin.Forms.Style) 인스턴스는 *명시적*이거나 *암시적*일 수 있습니다.

- *명시적* [`Style`](xref:Xamarin.Forms.Style) 인스턴스는 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) 및 `x:Key` 값을 지정 하 고 대상 요소의 [`Style`](xref:Xamarin.Forms.NavigableElement.Style) 속성을 참조로 설정 하 여 정의 됩니다 `x:Key` . *명시적* 스타일에 대 한 자세한 내용은 [명시적 스타일](~/xamarin-forms/user-interface/styles/explicit.md)을 참조 하세요.
- *암시적* [`Style`](xref:Xamarin.Forms.Style) 인스턴스는만 지정 하 여 정의 됩니다 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) . `Style`그러면 인스턴스가 해당 형식의 모든 요소에 자동으로 적용 됩니다. 의 서브 클래스에 `TargetType` 는가 자동으로 적용 되지 않습니다 `Style` . *암시적* 스타일에 대 한 자세한 내용은 [암시적 스타일](~/xamarin-forms/user-interface/styles/implicit.md)을 참조 하세요.

를 만들 때 [`Style`](xref:Xamarin.Forms.Style) 속성은 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) 항상 필요 합니다. 다음 코드 예제에서는 XAML로 생성 된 *명시적* 스타일 (참고)을 보여 줍니다 `x:Key` .

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

를 적용 하려면 `Style` [`VisualElement`](xref:Xamarin.Forms.VisualElement) [`TargetType`](xref:Xamarin.Forms.Style.TargetType) `Style` 다음 XAML 코드 예제에 표시 된 것 처럼 대상 개체가의 속성 값과 일치 하는 이어야 합니다.

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

뷰 계층 구조의 하위 스타일은 상위 수준에서 정의 된 스타일 보다 우선 적용 됩니다. 예를 들어 [`Style`](xref:Xamarin.Forms.Style) 응용 프로그램 수준에서로 설정 되는를로 설정 하면가 [`Label.TextColor`](xref:Xamarin.Forms.Label.TextColor) `Red` 로 설정 되는 페이지 수준 스타일에 의해 재정의 됩니다 `Label.TextColor` `Green` . 마찬가지로 페이지 수준 스타일은 컨트롤 수준 스타일에 의해 재정의 됩니다. 또한 `Label.TextColor` 가 컨트롤 속성에 직접 설정 된 경우이는 모든 스타일 보다 우선적으로 적용 됩니다.

이 단원의 문서에서는 *명시적* 및 *암시적* 스타일을 만들고 적용 하는 방법, 전역 스타일을 만드는 방법, 스타일 상속, 런타임에 스타일 변경에 응답 하는 방법 및에 포함 된 기본 제공 스타일을 사용 하는 방법에 대해 설명 하 고 설명 합니다 Xamarin.Forms .

> [!NOTE]
> **StyleId 란?**
>
> 2.2 이전에는 Xamarin.Forms [`StyleId`](xref:Xamarin.Forms.Element.StyleId) 속성을 사용 하 여 UI 테스트에서 식별할 수 있도록 응용 프로그램의 개별 요소를 식별 하 고 Pixate와 같은 테마 엔진에서 속성을 사용 했습니다. 그러나 속성을 Xamarin.Forms [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) 대체 한 속성이 2.2에 도입 되었습니다 [`StyleId`](xref:Xamarin.Forms.Element.StyleId) .

## <a name="related-links"></a>관련 링크

- [XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [스타일](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
