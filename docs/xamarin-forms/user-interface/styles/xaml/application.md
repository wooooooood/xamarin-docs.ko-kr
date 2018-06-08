---
title: 전역 스타일
description: 스타일 있습니다 사용할 수 전체적으로 응용 프로그램의 리소스 사전에 추가 하 여 합니다. 페이지나 컨트롤 스타일의 중복을 방지할 수 있습니다.
ms.prod: xamarin
ms.assetid: BDC65F82-65E0-4C8E-BB91-8E340EB2D15A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 219973e26c5ee25accec57f1bebbd1753391e6de
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847906"
---
# <a name="global-styles"></a>전역 스타일

_스타일 있습니다 사용할 수 전체적으로 응용 프로그램의 리소스 사전에 추가 하 여 합니다. 페이지나 컨트롤 스타일의 중복을 방지할 수 있습니다._

## <a name="creating-a-global-style-in-xaml"></a>XAML에서 전역 스타일 만들기

기본적으로 템플릿에서 만든 모든 Xamarin.Forms 응용 프로그램 사용는 **앱** 클래스를 구현 하는 [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) 하위 클래스입니다. 선언 하는 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 응용 프로그램의 응용 프로그램 수준 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) XAML에서는 기본값을 사용 하 여 **앱** 클래스는 XAML로 바꾸어야 합니다 **앱** 클래스와 관련 된 코드 숨김 합니다. 자세한 내용은 참조 [App 클래스 작업](~/xamarin-forms/app-fundamentals/application-class.md)합니다.

다음 코드 예제는 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 응용 프로그램 수준에서 선언 합니다.

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.App">
    <Application.Resources>
        <ResourceDictionary>
            <Style x:Key="buttonStyle" TargetType="Button">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                <Setter Property="BorderColor" Value="Lime" />
                <Setter Property="BorderRadius" Value="5" />
                <Setter Property="BorderWidth" Value="5" />
                <Setter Property="WidthRequest" Value="200" />
                <Setter Property="TextColor" Value="Teal" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

이 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 단일 정의 *명시적* 스타일 `buttonStyle`의 모양을 설정 하는 데 사용할 수는 있는 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) 인스턴스. 그러나 전역 스타일 수 *명시적* 또는 *암시적*합니다.

다음 코드 예제에서는 XAML 페이지 적용 된 `buttonStyle` 하 여 페이지의 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) 인스턴스:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

다음 스크린샷에 표시 된 모양 결과이 됩니다.

[![](application-images/application-styles-1.png "전역 스타일 예제")](application-images/application-styles-1-large.png#lightbox "글로벌 스타일 예제")

페이지의 스타일을 만드는 방법은 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), 참조 [명시적 스타일](~/xamarin-forms/user-interface/styles/explicit.md) 및 [암시적 스타일](~/xamarin-forms/user-interface/styles/implicit.md)합니다.

### <a name="overriding-styles"></a>스타일을 재정의

아래쪽 계층 구조 보기에 있는 스타일을 높을수록 정의 된 보다 우선 합니다. 예를 들어 설정는 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 설정 하는 [ `Button.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.TextColor/) 를 `Red` 응용 프로그램에서 수준으로 설정 하는 페이지 수준 스타일 재정의 됩니다 `Button.TextColor` 를`Green`. 마찬가지로, 페이지 수준 스타일 제어 수준 스타일에 의해 재정의 됩니다. 또한 경우 `Button.TextColor` 직접에서 컨트롤 속성을이 우선적으로 적용 됩니다 스타일이 모두 설정 되어 있습니다. 이러한 우선 순위는 다음 코드 예제에서 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="buttonStyle" TargetType="Button">
                ...
                <Setter Property="TextColor" Value="Red" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="buttonStyle" TargetType="Button">
                        ...
                        <Setter Property="TextColor" Value="Blue" />
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

원래 `buttonStyle`응용 프로그램 수준에서 정의 된,을 재정의 하 여는 `buttonStyle` 페이지 수준에서 정의 된 인스턴스. 페이지 수준 스타일을 재정의 하 여 제어 수준 또한 `buttonStyle`합니다. 따라서는 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) 다음 스크린샷에서 같이 인스턴스가 파란색 텍스트로 표시 됩니다.

[![](application-images/application-styles-2.png "스타일 예제 재정의")](application-images/application-styles-2-large.png#lightbox "스타일 예제를 재정의 합니다.")

## <a name="creating-a-global-style-in-c35"></a>C에서 전역 스타일 만들기&#35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 인스턴스는 응용 프로그램에 추가할 수 있습니다 [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) 새 C#에서 컬렉션 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), 고 추가 하 여 다음의 `Style` 인스턴스는 `ResourceDictionary`,으로 다음 코드 예제에 나와 있습니다.

```csharp
public class App : Application
{
    public App ()
    {
        var buttonStyle = new Style (typeof(Button)) {
            Setters = {
                ...
                new Setter { Property = Button.TextColorProperty,    Value = Color.Teal }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("buttonStyle", buttonStyle);
        ...
    }
    ...
}
```

생성자 정의 단일 *명시적* 스타일을 적용 하기 위한 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) 인스턴스 응용 프로그램에 걸쳐 있습니다. *명시적* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 인스턴스가 추가 되는 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 를 사용 하는 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) 는 를지정하는메서드를`key`을 가리키는 문자열은 `Style` 인스턴스. `Style` 인스턴스 응용 프로그램에서 올바른 형식의 모든 컨트롤에 적용할 수 있습니다. 그러나 전역 스타일 수 *명시적* 또는 *암시적*합니다.

다음 코드 예제는 C# 페이지 적용 표시는 `buttonStyle` 하 여 페이지의 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) 인스턴스:

```csharp
public class ApplicationStylesPageCS : ContentPage
{
    public ApplicationStylesPageCS ()
    {
        ...
        Content = new StackLayout {
            Children = {
                new Button { Text = "These buttons", Style = (Style)Application.Current.Resources ["buttonStyle"] },
                new Button { Text = "are demonstrating", Style = (Style)Application.Current.Resources ["buttonStyle"] },
                new Button { Text = "application styles", Style = (Style)Application.Current.Resources ["buttonStyle"]
                }
            }
        };
    }
}
```

`buttonStyle` 에 적용 되는 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) 인스턴스 설정 하 여 자신의 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) 속성의 모양을 제어 하 고는 `Button` 인스턴스.

## <a name="summary"></a>요약

스타일 사용할 수 있도록 설정 전체적으로 응용 프로그램을 추가 하 여 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)합니다. 페이지나 컨트롤 스타일의 중복을 방지할 수 있습니다.



## <a name="related-links"></a>관련 링크

- [XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [기본 스타일 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [스타일 (샘플) 작업을](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [스타일](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
