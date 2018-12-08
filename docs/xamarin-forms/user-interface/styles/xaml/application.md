---
title: Xamarin.Forms에서 글로벌 스타일
description: 스타일 사용할 수 있습니다 전체적으로 응용 프로그램의 리소스 사전에 추가 하 여 합니다. 이를 페이지 또는 컨트롤에서 스타일의 중복을 방지할 수 있습니다.
ms.prod: xamarin
ms.assetid: BDC65F82-65E0-4C8E-BB91-8E340EB2D15A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: b663a7f2a4a67a9c3e18ed474d9935227fe34294
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53059563"
---
# <a name="global-styles-in-xamarinforms"></a>Xamarin.Forms에서 글로벌 스타일

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)

_스타일 사용할 수 있습니다 전체적으로 응용 프로그램의 리소스 사전에 추가 하 여 합니다. 이를 페이지 또는 컨트롤에서 스타일의 중복을 방지할 수 있습니다._

## <a name="creating-a-global-style-in-xaml"></a>XAML에서 전역 스타일 만들기

기본적으로 템플릿에서 만든 모든 Xamarin.Forms 응용 프로그램 사용 합니다 **앱** 클래스를 구현 하는 [ `Application` ](xref:Xamarin.Forms.Application) 하위 클래스입니다. 선언 하는 [ `Style` ](xref:Xamarin.Forms.Style) 응용 프로그램의 응용 프로그램 수준 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) XAML에서 기본값을 사용 하 여 **앱** 클래스는 XAML을 사용 하 여 교체 해야 합니다 **앱** 클래스와 관련 된 코드 숨김입니다. 자세한 내용은 [앱 클래스를 사용 하 여 작업](~/xamarin-forms/app-fundamentals/application-class.md)합니다.

다음 코드 예제는 [ `Style` ](xref:Xamarin.Forms.Style) 응용 프로그램 수준에서 선언 합니다.

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

이렇게 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 단일 정의 *명시적* 스타일 `buttonStyle`의 모양을 설정 하는 데 사용할 됩니다 [ `Button` ](xref:Xamarin.Forms.Button) 인스턴스. 그러나 전역 스타일 될 수 있습니다 *명시적* 하거나 *암시적*합니다.

다음 코드 예제에서는 XAML 페이지 적용 된 `buttonStyle` 페이지의 [ `Button` ](xref:Xamarin.Forms.Button) 인스턴스:

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

이 인해 다음 스크린샷에 표시 된 모양:

[![](application-images/application-styles-1.png "글로벌 스타일 예제")](application-images/application-styles-1-large.png#lightbox "글로벌 스타일 예제")

페이지의 스타일을 만드는 방법은 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)를 참조 하십시오 [명시적 스타일](~/xamarin-forms/user-interface/styles/explicit.md) 및 [암시적 스타일](~/xamarin-forms/user-interface/styles/implicit.md)합니다.

### <a name="overriding-styles"></a>스타일을 재정의

스타일 뷰 계층 구조의 하위를 더 높은 정의 보다 우선 합니다. 예를 들어 설정를 [ `Style` ](xref:Xamarin.Forms.Style) 설정 하는 [ `Button.TextColor` ](xref:Xamarin.Forms.Button.TextColor) 하 `Red` 응용 프로그램 수준 설정 하는 페이지 수준 스타일에 의해 재정의 됩니다 `Button.TextColor` 를`Green`. 마찬가지로, 페이지 수준 스타일 컨트롤 수준 스타일에 의해 재정의 됩니다. 또한 경우 `Button.TextColor` 직접 컨트롤 속성에이 우선적으로 적용 됩니다 스타일이 모두 설정 합니다. 이 우선 순위는 다음 코드 예제에서 보여 줍니다.

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

원래 `buttonStyle`을 재정의 하 여, 응용 프로그램 수준에서 정의 된 `buttonStyle` 페이지 수준에서 정의 된 인스턴스. 페이지 수준 스타일 컨트롤 수준에서 재정의 되는 또한 `buttonStyle`합니다. 따라서 합니다 [ `Button` ](xref:Xamarin.Forms.Button) 인스턴스가 다음 스크린샷과에서 같이 파란색 텍스트로 표시 됩니다.

[![](application-images/application-styles-2.png "스타일 예제 재정의")](application-images/application-styles-2-large.png#lightbox "스타일 예제를 재정의 합니다.")

## <a name="creating-a-global-style-in-c35"></a>C에서 전역 스타일 만들기&#35;

[`Style`](xref:Xamarin.Forms.Style) 응용 프로그램의 인스턴스를 추가할 수 있습니다 [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) 새 C#에서 컬렉션 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)를 추가 하 여 다음을 `Style` 인스턴스를 `ResourceDictionary`,으로 다음 코드 예제에 표시 합니다.

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

생성자 정의 단일 *명시적* 적용에 대 한 스타일 [ `Button` ](xref:Xamarin.Forms.Button) 응용 프로그램에 걸쳐 인스턴스. *명시적* [ `Style` ](xref:Xamarin.Forms.Style) 인스턴스는에 추가 합니다 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 사용 하 여를 [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) 메서드를 지정 된 `key`를 참조 하는 문자열을 `Style` 인스턴스. `Style` 인스턴스 응용 프로그램에서 올바른 형식의 모든 컨트롤에 적용할 수 있습니다. 그러나 전역 스타일 될 수 있습니다 *명시적* 하거나 *암시적*합니다.

다음 코드 예제에서는 C# 페이지 적용을 보여 줍니다.는 `buttonStyle` 페이지의 [ `Button` ](xref:Xamarin.Forms.Button) 인스턴스:

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

`buttonStyle` 에 적용 되는 [ `Button` ](xref:Xamarin.Forms.Button) 설정 하 여 인스턴스 해당 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) 속성의 모양을 제어 하 고는 `Button` 인스턴스.

## <a name="summary"></a>요약

스타일 사용할 수 있습니다 전체적으로 응용 프로그램을 추가 하 여 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)합니다. 이를 페이지 또는 컨트롤에서 스타일의 중복을 방지할 수 있습니다.



## <a name="related-links"></a>관련 링크

- [XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [기본 스타일 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [스타일 (샘플)를 사용 하 여 작업](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [스타일](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
