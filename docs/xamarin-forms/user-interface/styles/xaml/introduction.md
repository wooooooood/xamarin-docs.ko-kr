---
title: Xamarin.Forms 스타일 소개
description: 스타일은 사용자 지정 시각적 요소의 모양을 허용 합니다. 스타일 특정 형식에 대 한 정의 되 고 해당 형식에 사용할 수 있는 속성에 대 한 값을 포함 합니다.
ms.prod: xamarin
ms.assetid: 3FF899C0-6CFB-4C1D-837D-9E9E10181967
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 4048ec78d48b810b39d46fbcb7708860c478cce3
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61023436"
---
# <a name="introduction-to-xamarinforms-styles"></a>Xamarin.Forms 스타일 소개

_스타일은 사용자 지정 시각적 요소의 모양을 허용 합니다. 스타일 특정 형식에 대 한 정의 되 고 해당 형식에 사용할 수 있는 속성에 대 한 값을 포함 합니다._

Xamarin.Forms 응용 프로그램은 종종 동일한 모양이 있는 여러 컨트롤을 포함 합니다. 예를 들어 응용 프로그램이 여러 있을 [ `Label` ](xref:Xamarin.Forms.Label) 동일한 글꼴 옵션 및 있는 레이아웃 옵션을 다음 XAML 코드 예제에 표시 된 대로 인스턴스:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="Styles.NoStylesPage"
    Title="No Styles"
    Icon="xaml.png">
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
        Icon = "csharp.png";
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

각 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스마다에서 표시 되는 텍스트의 모양을 제어 하기 위한 동일한 속성 값을 `Label`입니다. 이로 인해 결국 다음 스크린샷에 표시된 모양이 됩니다.

[![](introduction-images/no-styles.png "레이블 스타일 없이 모양을")](introduction-images/no-styles-large.png#lightbox "스타일 없이 모양을 레이블")

반복 될 수 있습니다 각 개별 컨트롤의 모양을 설정 하 고 오류가 발생 하기 쉽습니다. 대신, 스타일을 만들 수 있습니다 모양을 정의 하며 그런 다음 필요한 컨트롤에 적용 하는 합니다.

## <a name="create-a-style"></a>스타일 만들기

합니다 [ `Style` ](xref:Xamarin.Forms.Style) 다음 여러 시각적 요소 인스턴스에 적용할 수 있는 하나의 개체에 속성 값의 컬렉션을 그룹화 하는 클래스입니다. 이 반복 태그를 줄일 수 있습니다 및 응용 프로그램 모양을 보다 쉽게 변경할 수 있습니다.

하지만 주로 XAML 기반 응용 프로그램에 대 한 스타일 디자인에 만들 수 있습니다 C#에서:

- [`Style`](xref:Xamarin.Forms.Style) XAML에서 만든 인스턴스는 일반적으로 정의 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 에 할당 된 합니다 [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) 컨트롤의 컬렉션 페이지에서 또는 [ `Resources` ](xref:Xamarin.Forms.Application.Resources) 응용 프로그램의 컬렉션입니다.
- [`Style`](xref:Xamarin.Forms.Style) C#에서 만든 인스턴스는 일반적으로 페이지의 클래스 또는 전체적으로 액세스할 수 있는 클래스에서 정의 됩니다.

[`Style`](xref:Xamarin.Forms.Style)을 정의할 위치를 선택하면 사용할 수 있는 위치가 결정됩니다.

- [`Style`](xref:Xamarin.Forms.Style) 제어 수준에 정의 된 인스턴스 및 해당 자식 컨트롤에 적용할 수 있습니다.
- [`Style`](xref:Xamarin.Forms.Style) 페이지 수준에서 정의 된 인스턴스 및 해당 자식 페이지에 적용할 수 있습니다.
- [`Style`](xref:Xamarin.Forms.Style) 응용 프로그램 수준에서 정의 된 인스턴스는 응용 프로그램에 걸쳐 적용할 수 있습니다.

각 [ `Style` ](xref:Xamarin.Forms.Style) 하나 이상의 컬렉션을 포함 하는 인스턴스 [ `Setter` ](xref:Xamarin.Forms.Setter) 개체와 각 `Setter` 필요는 [ `Property` ](xref:Xamarin.Forms.Setter.Property) 및 [`Value`](xref:Xamarin.Forms.Setter.Value). 합니다 `Property` 스타일에 적용 되는 요소의 바인딩 가능한 속성 이름 및 `Value` 속성에 적용 되는 값입니다.

각 [ `Style` ](xref:Xamarin.Forms.Style) 인스턴스 수 *명시적*, 또는 *암시적*:

- *명시적* [ `Style` ](xref:Xamarin.Forms.Style) 인스턴스를 지정 하 여 정의 되는 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) 및 `x:Key` 값을 설정 하 여 대상 요소의 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) 속성을는 `x:Key` 참조 합니다. 에 대 한 자세한 내용은 *명시적* 스타일을 참조 하십시오 [명시적 스타일](~/xamarin-forms/user-interface/styles/explicit.md)합니다.
- *암시적* [ `Style` ](xref:Xamarin.Forms.Style) 인스턴스만 지정 하 여 정의 되는 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType)합니다. `Style` 인스턴스 자동으로 해당 형식의 모든 요소에 적용 한 다음 됩니다. 참고의 해당 서브 클래스는 `TargetType` 자동으로 부여 되지 않습니다는 `Style` 적용 합니다. 에 대 한 자세한 내용은 *암시적* 스타일을 참조 하십시오 [암시적 스타일](~/xamarin-forms/user-interface/styles/implicit.md)합니다.

만들 때를 [ `Style` ](xref:Xamarin.Forms.Style)서 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) 속성은 항상 필요 합니다. 다음 코드 예제는 *명시적* 스타일 (참고는 `x:Key`) XAML에서 만든:

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

적용할를 `Style`, 대상 개체 여야 합니다는 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) 일치 하는 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) 속성 값을 `Style`다음 XAML 코드 예제에 나와 있는 것 처럼:

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

스타일 뷰 계층 구조의 하위를 더 높은 정의 보다 우선 합니다. 예를 들어 설정를 [ `Style` ](xref:Xamarin.Forms.Style) 설정 하는 [ `Label.TextColor` ](xref:Xamarin.Forms.Label.TextColor) 하 `Red` 응용 프로그램 수준 설정 하는 페이지 수준 스타일에 의해 재정의 됩니다 `Label.TextColor` 를`Green`. 마찬가지로, 페이지 수준 스타일 컨트롤 수준 스타일에 의해 재정의 됩니다. 또한 경우 `Label.TextColor` 직접 컨트롤 속성에서이 우선 스타일 설정 합니다.

이 섹션의에서 문서에서는 설명 및 만들기 및 적용 하는 방법을 설명 *명시적* 하 고 *암시적* 스타일, 상속, 스타일, 전역 스타일을 만드는 방법 런타임에 스타일 변경 내용에 응답 하는 방법 및 Xamarin.Forms에 포함 된 기본 제공 스타일을 사용 하는 방법입니다.

> [!NOTE]
> **StyleId 란?**
>
> Xamarin.Forms 2.2 이전 합니다 [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) 속성은 Pixate 같은 테마 엔진 및 UI 테스트에서 id에 대 한 응용 프로그램의 개별 요소를 식별 하는 데 사용 되었습니다. 하지만 Xamarin.Forms 2.2 도입 합니다 [ `AutomationId` ](xref:Xamarin.Forms.Element.AutomationId) 속성에는 [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) 속성입니다.

## <a name="related-links"></a>관련 링크

- [XAML 마크업 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [스타일](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
