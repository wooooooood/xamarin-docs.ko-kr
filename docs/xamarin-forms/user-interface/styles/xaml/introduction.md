---
title: Xamarin.ios 스타일 소개
description: 스타일을 사용 하면 시각적 요소의 모양을 사용자 지정할 수 있습니다. 스타일은 특정 형식에 대해 정의 되며 해당 형식에 사용할 수 있는 속성에 대 한 값을 포함 합니다.
ms.prod: xamarin
ms.assetid: 3FF899C0-6CFB-4C1D-837D-9E9E10181967
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 35f8dad3590c07ceb3c93aa735b8c02d75098498
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "70228163"
---
# <a name="introduction-to-xamarinforms-styles"></a>Xamarin.ios 스타일 소개

_스타일을 사용 하면 시각적 요소의 모양을 사용자 지정할 수 있습니다. 스타일은 특정 형식에 대해 정의 되며 해당 형식에 사용할 수 있는 속성에 대 한 값을 포함 합니다._

Xamarin Forms 응용 프로그램에는 동일한 모양의 여러 컨트롤이 포함 되어 있는 경우가 많습니다. 예를 들어 다음 XAML 코드 예제에 표시 된 것 처럼 응용 프로그램에 동일한 글꼴 옵션 및 레이아웃 옵션을 포함 하는 여러 개의 [`Label`](xref:Xamarin.Forms.Label) 인스턴스가 있을 수 있습니다.

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

각 [`Label`](xref:Xamarin.Forms.Label) 인스턴스에는 `Label`표시 되는 텍스트의 모양을 제어 하는 동일한 속성 값이 있습니다. 이로 인해 결국 다음 스크린샷에 표시된 모양이 됩니다.

[스타일이 없는![레이블 모양](introduction-images/no-styles.png)](introduction-images/no-styles-large.png#lightbox)

각 개별 컨트롤의 모양을 반복적으로 설정 하면 오류가 발생할 수 있습니다. 대신 모양을 정의 하는 스타일을 만든 다음 필요한 컨트롤에 적용할 수 있습니다.

## <a name="create-a-style"></a>스타일 만들기

[`Style`](xref:Xamarin.Forms.Style) 클래스는 속성 값 컬렉션을 하나의 개체로 그룹화 한 후 여러 시각적 요소 인스턴스에 적용할 수 있습니다. 이렇게 하면 반복적인 태그를 줄이고 응용 프로그램의 모양을 보다 쉽게 변경할 수 있습니다.

스타일은 주로 XAML 기반 응용 프로그램용으로 설계 되었지만에서 C#만들 수도 있습니다.

- XAML에서 만든 [`Style`](xref:Xamarin.Forms.Style) 인스턴스는 일반적으로 컨트롤, 페이지의 [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) 컬렉션 또는 응용 프로그램의 [`Resources`](xref:Xamarin.Forms.Application.Resources) 컬렉션에 할당 되는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 에 정의 됩니다.
- [`Style`](xref:Xamarin.Forms.Style) 에서 C# 만든 인스턴스는 일반적으로 페이지의 클래스 또는 전역으로 액세스할 수 있는 클래스에서 정의 됩니다.

[`Style`](xref:Xamarin.Forms.Style)을 정의할 위치를 선택하면 사용할 수 있는 위치가 결정됩니다.

- 컨트롤 수준에서 정의 된 [`Style`](xref:Xamarin.Forms.Style) 인스턴스는 컨트롤과 해당 자식에만 적용할 수 있습니다.
- 페이지 수준에서 정의 된 [`Style`](xref:Xamarin.Forms.Style) 인스턴스는 페이지 및 해당 자식에만 적용할 수 있습니다.
- 응용 프로그램 수준에서 정의 된 [`Style`](xref:Xamarin.Forms.Style) 인스턴스는 응용 프로그램 전체에 적용할 수 있습니다.

각 [`Style`](xref:Xamarin.Forms.Style) 인스턴스에는 하나 이상의 [`Setter`](xref:Xamarin.Forms.Setter) 개체 컬렉션이 포함 되며 각 `Setter`에는 [`Property`](xref:Xamarin.Forms.Setter.Property) 및 [`Value`](xref:Xamarin.Forms.Setter.Value)있습니다. `Property`은 스타일이 적용 되는 요소의 바인딩 가능한 속성 이름 이며 `Value`는 속성에 적용 되는 값입니다.

각 [`Style`](xref:Xamarin.Forms.Style) 인스턴스는 *명시적*이거나 *암시적*일 수 있습니다.

- *명시적* [`Style`](xref:Xamarin.Forms.Style) 인스턴스는 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) 및 `x:Key` 값을 지정 하 고 대상 요소의 [`Style`](xref:Xamarin.Forms.NavigableElement.Style) 속성을 `x:Key` 참조로 설정 하 여 정의 됩니다. *명시적* 스타일에 대 한 자세한 내용은 [명시적 스타일](~/xamarin-forms/user-interface/styles/explicit.md)을 참조 하세요.
- *암시적* [`Style`](xref:Xamarin.Forms.Style) 인스턴스는 [`TargetType`](xref:Xamarin.Forms.Style.TargetType)만 지정 하 여 정의 됩니다. 그러면 `Style` 인스턴스가 해당 형식의 모든 요소에 자동으로 적용 됩니다. `TargetType`의 서브 클래스에는 자동으로 `Style` 적용 되지 않습니다. *암시적* 스타일에 대 한 자세한 내용은 [암시적 스타일](~/xamarin-forms/user-interface/styles/implicit.md)을 참조 하세요.

[`Style`](xref:Xamarin.Forms.Style)를 만들 때 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) 속성은 항상 필요 합니다. 다음 코드 예제에서는 XAML에서 만든 *명시적* 스타일 (`x:Key`참고)을 보여 줍니다.

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

`Style`를 적용 하려면 다음 XAML 코드 예제에 표시 된 것 처럼 대상 개체가 `Style`의 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) 속성 값과 일치 하는 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 이어야 합니다.

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

뷰 계층 구조의 하위 스타일은 상위 수준에서 정의 된 스타일 보다 우선 적용 됩니다. 예를 들어 응용 프로그램 수준에서 `Red` [`Label.TextColor`](xref:Xamarin.Forms.Label.TextColor) 를 설정 하는 [`Style`](xref:Xamarin.Forms.Style) 설정은 `Label.TextColor`를 `Green`로 설정 하는 페이지 수준 스타일에 의해 재정의 됩니다. 마찬가지로 페이지 수준 스타일은 컨트롤 수준 스타일에 의해 재정의 됩니다. 또한 컨트롤 속성에 대 한 `Label.TextColor`를 직접 설정 하는 경우 스타일 보다 우선 적용 됩니다.

이 단원의 문서에서는 *명시적* 및 *암시적* 스타일을 만들고 적용 하는 방법, 전역 스타일을 만드는 방법, 스타일 상속, 런타임에 스타일 변경에 응답 하는 방법 및에 포함 된 작성 된 스타일을 사용 하는 방법에 대해 설명 하 고 설명 합니다. Xamarin.ios.

> [!NOTE]
> **StyleId 란?**
>
> Xamarin.ios 2.2 이전에는 [`StyleId`](xref:Xamarin.Forms.Element.StyleId) 속성을 사용 하 여 UI 테스트에서 식별할 수 있도록 응용 프로그램에서 개별 요소를 식별 하 고 Pixate와 같은 테마 엔진을 사용 했습니다. 그러나 Xamarin.ios 2.2에는 [`StyleId`](xref:Xamarin.Forms.Element.StyleId) 속성을 대체 한 [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) 속성이 도입 되었습니다.

## <a name="related-links"></a>관련 링크

- [XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [스타일](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
