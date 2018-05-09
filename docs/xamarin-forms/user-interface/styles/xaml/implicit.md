---
title: 암시적 스타일
description: 암시적 스타일은 스타일을 참조 하려면 각 컨트롤을 요구 하지 않고 동일한 TargetType의 모든 컨트롤에서 사용 됩니다.
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: d9c9f8816ea45ac122829739ad5134cc740263df
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
---
# <a name="implicit-styles"></a>암시적 스타일

_암시적 스타일은 스타일을 참조 하려면 각 컨트롤을 요구 하지 않고 동일한 TargetType의 모든 컨트롤에서 사용 됩니다._

## <a name="creating-an-implicit-style-in-xaml"></a>Xaml에서 암시적 스타일 만들기

선언 하는 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 페이지 수준에서 한 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 페이지 다음 하나 이상의에 추가 해야 `Style` 선언에 포함 될 수는 `ResourceDictionary`합니다. A `Style` 이루어집니다 *암시적* 지정 하 여 프로그램 `x:Key` 특성입니다. 스타일과 일치 하는 시각적 요소에 적용 됩니다는 `TargetType` 정확 하 게 속하지만에서 파생 된 요소에는 `TargetType` 값입니다.

다음 코드 예제는 *암시적* 스타일 페이지의 XAML에 선언 된 `ResourceDictionary`는 페이지에 적용 하 고 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 인스턴스:

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

[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 단일 정의 *암시적* 스타일의 페이지에 적용 되는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 인스턴스. `Style` 배경이 노란색인에 다른 모양 옵션을 설정 하는 동안 파란색 텍스트를 표시 하는 데 사용 됩니다. `Style` 는 페이지에 추가 되 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 지정 하지 않고는 `x:Key` 특성입니다. 따라서는 `Style` 모두에 적용 되는 `Entry` 일치 하는 대로 암시적으로 인스턴스는 [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) 속성은 `Style` 정확 하 게 합니다. 그러나는 `Style` 에 적용 되지 않습니다는 `CustomEntry` 서브클래싱된는 인스턴스에 `Entry`합니다. 다음 스크린샷에 표시 된 모양 결과이 됩니다.

[![](implicit-images/implicit-styles.png "암시적 스타일 예제")](implicit-images/implicit-styles-large.png#lightbox "암시적 스타일 예제")

또한, 네 번째 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 재정의 [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) 및 [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) 다른 암시적스타일의속성`Color`값입니다.

### <a name="creating-an-implicit-style-at-the-control-level"></a>컨트롤에서 암시적 스타일 수준 만들기

생성 될 뿐만 아니라 *암시적* 페이지 수준에서 스타일을 만들 수 있습니다도 제어 수준에 다음 코드 예제에 나와 있는 것 처럼:

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

이 예제에서는 *암시적* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 에 할당 되는 [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) 의 컬렉션은 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)제어 합니다. *암시적* 스타일 컨트롤과 해당 자식 컨트롤에 적용할 수 있습니다.

응용 프로그램에서 스타일을 만드는 방법은 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), 참조 [글로벌 스타일](~/xamarin-forms/user-interface/styles/application.md)합니다.

## <a name="creating-an-implicit-style-in-c35"></a>C에서 암시적 스타일 만들기&#35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 인스턴스는 페이지에 추가할 수 있습니다 [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) 새 C#에서 컬렉션 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), 고 추가 하 여 다음의 `Style` 인스턴스는 `ResourceDictionary`에 나타난 것 처럼는 다음 코드 예제

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

생성자 정의 단일 *암시적* 스타일의 페이지에 적용 되는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 인스턴스. `Style` 배경이 노란색인에 다른 모양 옵션을 설정 하는 동안 파란색 텍스트를 표시 하는 데 사용 됩니다. `Style` 는 페이지에 추가 되 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 지정 하지 않고는 `key` 문자열입니다. 따라서는 `Style` 모두에 적용 되는 `Entry` 일치 하는 대로 암시적으로 인스턴스는 [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) 속성은 `Style` 정확 하 게 합니다. 그러나는 `Style` 에 적용 되지 않습니다는 `CustomEntry` 서브클래싱된는 인스턴스에 `Entry`합니다.

## <a name="summary"></a>요약

*암시적* 하나는 동일한 모든 시각적 요소에 의해 사용 되는 스타일은 [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/), 각 컨트롤에 스타일을 참조 하지 않고도 합니다. A `Style` 이루어집니다 *암시적* 지정 하 여 프로그램 `x:Key` 특성입니다. 대신,는 `x:Key` 특성의 값이 자동으로 됩니다는 [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) 속성입니다.



## <a name="related-links"></a>관련 링크

- [XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [기본 스타일 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [스타일 (샘플) 작업을](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [스타일](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
