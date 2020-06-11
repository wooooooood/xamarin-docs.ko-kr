---
제목: " Xamarin.Forms 설명:" 스타일을 응용 프로그램의 리소스 사전에 추가 하 여 전역 스타일을 전역으로 사용할 수 있습니다. 이렇게 하면 페이지 또는 컨트롤에서 스타일의 중복을 방지할 수 있습니다. "
assetid: BDC65F82-65E0-4C8E-BB91-8E340EB2D15A: xamarin-forms author: davidbritch: dabritch:: 02/17/2016-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="global-styles-in-xamarinforms"></a>전역 스타일Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_스타일은 응용 프로그램의 리소스 사전에 추가 하 여 전역적으로 사용할 수 있습니다. 이렇게 하면 여러 페이지나 컨트롤에서 스타일의 중복을 방지할 수 있습니다._

## <a name="create-a-global-style-in-xaml"></a>XAML에서 전역 스타일 만들기

기본적으로 Xamarin.Forms 템플릿에서 만든 모든 응용 프로그램은 **App** 클래스를 사용 하 여 하위 클래스를 구현 [`Application`](xref:Xamarin.Forms.Application) 합니다. 응용 프로그램 수준에서를 선언 하려면 [`Style`](xref:Xamarin.Forms.Style) xaml을 사용 하 여 응용 프로그램의 기본 응용 프로그램 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 클래스를 xaml **앱** 클래스 및 관련 코드 숨김으로 바꾸어야 합니다. **App** 자세한 내용은 [App 클래스 작업](~/xamarin-forms/app-fundamentals/application-class.md)을 참조 하세요.

다음 코드 예제에서는 [`Style`](xref:Xamarin.Forms.Style) 응용 프로그램 수준에서 선언 된을 보여 줍니다.

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

이는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) *explicit* `buttonStyle` 인스턴스의 모양을 설정 하는 데 사용 되는 단일 명시적 스타일를 정의 [`Button`](xref:Xamarin.Forms.Button) 합니다. 그러나 전역 스타일은 *명시적* 이거나 *암시적*일 수 있습니다.

다음 코드 예제에서는를 페이지의 인스턴스에 적용 하는 XAML 페이지를 보여 줍니다 `buttonStyle` [`Button`](xref:Xamarin.Forms.Button) .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

이로 인해 결국 다음 스크린샷에 표시된 모양이 됩니다.

[![](application-images/application-styles-1.png "Global Styles Example")](application-images/application-styles-1-large.png#lightbox "Global Styles Example")

페이지의 스타일을 만드는 방법에 대 한 자세한 내용은 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [명시적 스타일](~/xamarin-forms/user-interface/styles/explicit.md) 및 [암시적 스타일](~/xamarin-forms/user-interface/styles/implicit.md)을 참조 하세요.

### <a name="override-styles"></a>재정의 스타일

뷰 계층 구조의 하위 스타일은 상위 수준에서 정의 된 스타일 보다 우선 적용 됩니다. 예를 들어 [`Style`](xref:Xamarin.Forms.Style) 응용 프로그램 수준에서로 설정 되는를로 설정 하면가 [`Button.TextColor`](xref:Xamarin.Forms.Button.TextColor) `Red` 로 설정 되는 페이지 수준 스타일에 의해 재정의 됩니다 `Button.TextColor` `Green` . 마찬가지로 페이지 수준 스타일은 컨트롤 수준 스타일에 의해 재정의 됩니다. 또한 `Button.TextColor` 가 컨트롤 속성에 직접 설정 된 경우 스타일 보다 우선 적용 됩니다. 이 우선 순위는 다음 코드 예제에서 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" IconImageSource="xaml.png">
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

`buttonStyle`응용 프로그램 수준에서 정의 된 원래는 `buttonStyle` 페이지 수준에서 정의 된 인스턴스에 의해 재정의 됩니다. 또한 페이지 수준 스타일이 컨트롤 수준에 의해 재정의 됩니다 `buttonStyle` . 따라서 [`Button`](xref:Xamarin.Forms.Button) 다음 스크린샷에 표시 된 것 처럼 인스턴스는 파란색 텍스트로 표시 됩니다.

[![](application-images/application-styles-2.png "Overriding Styles Example")](application-images/application-styles-2-large.png#lightbox "Overriding Styles Example")

## <a name="create-a-global-style-in-c35"></a>C&#35;에서 전역 스타일 만들기

[`Style`](xref:Xamarin.Forms.Style)[`Resources`](xref:Xamarin.Forms.VisualElement.Resources) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) `Style` `ResourceDictionary` 다음 코드 예제와 같이 새를 만든 다음 인스턴스를에 추가 하 여 c #의 응용 프로그램 컬렉션에 인스턴스를 추가할 수 있습니다.

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

생성자는 *explicit* [`Button`](xref:Xamarin.Forms.Button) 응용 프로그램 전체에서 인스턴스에 적용 하기 위한 단일 명시적 스타일을 정의 합니다. *명시적* [`Style`](xref:Xamarin.Forms.Style) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [`Add`](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) `key` 인스턴스는 인스턴스를 참조 하는 문자열을 지정 하 여 메서드를 사용 하 `Style` 여에 추가 됩니다. `Style`그런 다음 응용 프로그램에서 올바른 형식의 모든 컨트롤에 인스턴스를 적용할 수 있습니다. 그러나 전역 스타일은 *명시적* 이거나 *암시적*일 수 있습니다.

다음 코드 예제에서는를 페이지의 인스턴스에 적용 하는 c # 페이지를 보여 줍니다 `buttonStyle` [`Button`](xref:Xamarin.Forms.Button) .

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

는 `buttonStyle` [`Button`](xref:Xamarin.Forms.Button) 해당 속성을 설정 하 여 인스턴스에 적용 되 [`Style`](xref:Xamarin.Forms.NavigableElement.Style) 고 인스턴스의 모양을 제어 합니다 `Button` .

## <a name="related-links"></a>관련 링크

- [XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [기본 스타일 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [스타일 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
