---
title: Xamarin.Forms에서 암시적 스타일
description: 암시적 스타일은 각 컨트롤에 스타일을 참조 하지 않고도 동일한 TargetType의 모든 컨트롤에서 사용 되는 하나입니다.
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
ms.openlocfilehash: d58ba81596cccf470b7246514d71f35968599880
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77131060"
---
# <a name="implicit-styles-in-xamarinforms"></a>Xamarin.Forms에서 암시적 스타일

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_암시적 스타일은 각 컨트롤이 스타일을 참조할 필요 없이 동일한 TargetType의 모든 컨트롤에서 사용 하는 스타일입니다._

## <a name="create-an-implicit-style-in-xaml"></a>XAML에서 암시적 스타일 만들기

페이지 수준에서 [`Style`](xref:Xamarin.Forms.Style) 를 선언 하려면 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 를 페이지에 추가한 다음 하나 이상의 `Style` 선언을 `ResourceDictionary`에 포함할 수 있습니다. `x:Key` 특성을 지정 하지 않으면 `Style` *암시적* 으로 수행 됩니다. 그러면 `TargetType`와 정확 하 게 일치 하는 시각적 요소에 스타일이 적용 되지만 `TargetType` 값에서 파생 된 요소에는 적용 되지 않습니다.

다음 코드 예제에서는 페이지의 `ResourceDictionary`에서 XAML로 선언 되 고 페이지의 [`Entry`](xref:Xamarin.Forms.Entry) 인스턴스에 적용 되는 *암시적* 스타일을 보여 줍니다.

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

[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 는 페이지의 [`Entry`](xref:Xamarin.Forms.Entry) 인스턴스에 적용 되는 단일 *암시적* 스타일을 정의 합니다. `Style`는 노란색 배경에 파란색 텍스트를 표시 하는 동시에 다른 모양 옵션도 설정 하는 데 사용 됩니다. `Style` `x:Key` 특성을 지정 하지 않고 페이지의 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 에 추가 됩니다. 따라서 `Style`는 `Style`의 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) 속성과 정확히 일치 하므로 모든 `Entry` 인스턴스에 암시적으로 적용 됩니다. 그러나 `Style`은 서브클래싱된 `Entry``CustomEntry` 인스턴스에 적용 되지 않습니다. 이로 인해 결국 다음 스크린샷에 표시된 모양이 됩니다.

[![암시적 스타일 예제](implicit-images/implicit-styles.png)](implicit-images/implicit-styles-large.png#lightbox)

또한 네 번째 [`Entry`](xref:Xamarin.Forms.Entry) 는 암시적 스타일의 [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) 및 [`TextColor`](xref:Xamarin.Forms.InputView.TextColor) 속성을 다른 `Color` 값으로 재정의 합니다.

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

이 예제에서는 *암시적* [`Style`](xref:Xamarin.Forms.Style) [`StackLayout`](xref:Xamarin.Forms.StackLayout) 컨트롤의 [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) 컬렉션에 할당 됩니다. 그런 다음 컨트롤 및 해당 자식에 *암시적* 스타일을 적용할 수 있습니다.

응용 프로그램의 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에서 스타일을 만드는 방법에 대 한 자세한 내용은 [전역 스타일](~/xamarin-forms/user-interface/styles/application.md)을 참조 하세요.

## <a name="create-an-implicit-style-in-c35"></a>C에서 암시적 스타일 만들기&#35;

다음 코드 예제와 같이 새 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)를 만든 다음 `Style` 인스턴스 C# 를 `ResourceDictionary`에 추가 하 여의 페이지 [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) 컬렉션에 [`Style`](xref:Xamarin.Forms.Style) 인스턴스를 추가할 수 있습니다.

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

생성자는 페이지의 [`Entry`](xref:Xamarin.Forms.Entry) 인스턴스에 적용 되는 단일 *암시적* 스타일을 정의 합니다. `Style`는 노란색 배경에 파란색 텍스트를 표시 하는 동시에 다른 모양 옵션도 설정 하는 데 사용 됩니다. `Style` `key` 문자열을 지정 하지 않고 페이지의 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 에 추가 됩니다. 따라서 `Style`는 `Style`의 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) 속성과 정확히 일치 하므로 모든 `Entry` 인스턴스에 암시적으로 적용 됩니다. 그러나 `Style`은 서브클래싱된 `Entry``CustomEntry` 인스턴스에 적용 되지 않습니다.

## <a name="apply-a-style-to-derived-types"></a>파생 형식에 스타일 적용

[`Style.ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) 속성을 사용 하면 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) 속성이 참조 하는 기본 형식에서 파생 된 컨트롤에 스타일을 적용할 수 있습니다. 따라서이 속성을 `true`로 설정 하면 형식이 `TargetType` 속성에 지정 된 기본 형식에서 파생 되는 경우 단일 스타일이 여러 형식을 대상으로 지정할 수 있습니다.

다음 예제에서는 [`Button`](xref:Xamarin.Forms.Button) 인스턴스의 배경색을 빨강으로 설정 하는 암시적 스타일을 보여 줍니다.

```xaml
<Style TargetType="Button"
       ApplyToDerivedTypes="True">
    <Setter Property="BackgroundColor"
            Value="Red" />
</Style>
```

이 스타일을 페이지 수준 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 에 배치 하면 페이지의 모든 [`Button`](xref:Xamarin.Forms.Button) 인스턴스에 적용 되 고 `Button`에서 파생 되는 모든 컨트롤에도 적용 됩니다. 그러나 [`ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) 속성이 설정 되지 않은 상태로 유지 되는 경우에는 `Button` 인스턴스에만 스타일이 적용 됩니다.

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
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
