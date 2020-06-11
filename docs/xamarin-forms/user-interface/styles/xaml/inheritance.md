---
제목: "설명:" 스타일 상속 Xamarin.Forms 은 다른 스타일에서 상속 하 여 중복을 줄이고 재사용을 가능 하 게 합니다. 이 문서에서는 응용 프로그램에서 스타일 상속을 수행 하는 방법을 설명 Xamarin.Forms 합니다. "
assetid: 67A3A39C-8CC0-446D-8162-FFA73582D3B8 ms. 기술: xamarin-forms author: davidbritch: dabritch: ms. date: 02/17/2016 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="style-inheritance-in-xamarinforms"></a>의 스타일 상속Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_스타일은 다른 스타일에서 상속 하 여 중복을 줄이고 재사용을 가능 하 게 합니다._

## <a name="style-inheritance-in-xaml"></a>XAML의 스타일 상속

스타일 상속은 속성을 기존로 설정 하 여 수행 됩니다 [`Style.BasedOn`](xref:Xamarin.Forms.Style.BasedOn) [`Style`](xref:Xamarin.Forms.Style) . XAML에서는 `BasedOn` 속성을 `StaticResource` 이전에 만든를 참조 하는 태그 확장으로 설정 하 여이를 구현 `Style` 합니다. C #에서는 속성을 인스턴스로 설정 하 여이를 구현 `BasedOn` `Style` 합니다.

기본 스타일에서 상속 하는 스타일에는 [`Setter`](xref:Xamarin.Forms.Setter) 새 속성의 인스턴스가 포함 될 수 있으며, 기본 스타일에서 스타일을 재정의 하는 데 사용할 수 있습니다. 또한 기본 스타일에서 상속 되는 스타일은 동일한 형식을 대상으로 하거나 기본 스타일을 대상으로 하는 형식에서 파생 되는 형식 이어야 합니다. 예를 들어 기본 스타일이 인스턴스를 대상으로 하는 경우 [`View`](xref:Xamarin.Forms.View) 기본 스타일을 기반으로 하는 스타일은 `View` `View` 및 인스턴스와 같이 클래스에서 파생 된 인스턴스 또는 형식을 대상으로 지정할 수 있습니다 [`Label`](xref:Xamarin.Forms.Label) [`Button`](xref:Xamarin.Forms.Button) .

다음 코드에서는 XAML 페이지의 *명시적* 스타일 상속을 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
            </Style>
            <Style x:Key="labelStyle" TargetType="Label"
                   BasedOn="{StaticResource baseStyle}">
                ...
                <Setter Property="TextColor" Value="Teal" />
            </Style>
            <Style x:Key="buttonStyle" TargetType="Button"
                   BasedOn="{StaticResource baseStyle}">
                <Setter Property="BorderColor" Value="Lime" />
                ...
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelStyle}" />
            ...
            <Button Text="So is the button"
                    Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

는 `baseStyle` 대상 [`View`](xref:Xamarin.Forms.View) 인스턴스를 지정 하 고 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 및 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성을 설정 합니다. 는 `baseStyle` 컨트롤에서 직접 설정 되지 않습니다. 대신, `labelStyle` 및 `buttonStyle` 에서 상속 되며 추가 바인딩 가능한 속성 값을 설정 합니다. `labelStyle` `buttonStyle` 그런 다음 및는 [`Label`](xref:Xamarin.Forms.Label) [`Button`](xref:Xamarin.Forms.Button) 해당 속성을 설정 하 여 인스턴스 및 인스턴스에 적용 됩니다 [`Style`](xref:Xamarin.Forms.NavigableElement.Style) . 이로 인해 결국 다음 스크린샷에 표시된 모양이 됩니다.

[![](inheritance-images/style-inheritance.png)](inheritance-images/style-inheritance-large.png#lightbox)

> [!NOTE]
> 암시적 스타일은 명시적 스타일에서 파생 될 수 있지만 명시적 스타일은 암시적 스타일에서 파생 될 수 없습니다.

### <a name="respecting-the-inheritance-chain"></a>상속 체인 준수

스타일은 뷰 계층 구조에서 동일한 수준 이상의 스타일 에서만 상속할 수 있습니다. 이는 다음을 의미합니다.

- 응용 프로그램 수준 리소스는 다른 응용 프로그램 수준 리소스 에서만 상속할 수 있습니다.
- 페이지 수준 리소스는 응용 프로그램 수준 리소스와 기타 페이지 수준 리소스에서 상속할 수 있습니다.
- 컨트롤 수준 리소스는 응용 프로그램 수준 리소스, 페이지 수준 리소스 및 기타 제어 수준 리소스에서 상속할 수 있습니다.

다음 코드 예제에서는이 상속 체인을 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelStyle" TargetType="Label" BasedOn="{StaticResource baseStyle}">
                      ...
                    </Style>
                    <Style x:Key="buttonStyle" TargetType="Button" BasedOn="{StaticResource baseStyle}">
                      ...
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

이 예제에서 `labelStyle` 및는 `buttonStyle` 컨트롤 수준 리소스이 고는 `baseStyle` 페이지 수준 리소스입니다. 그러나 및가 `labelStyle` 에서 상속 되는 경우 `buttonStyle` `baseStyle` `baseStyle` `labelStyle` `buttonStyle` 뷰 계층의 각 위치 때문에 또는에서 상속할 수 없습니다.

## <a name="style-inheritance-in-c35"></a>C&#35;의 스타일 상속

[`Style`](xref:Xamarin.Forms.Style)인스턴스가 필요한 컨트롤의 속성에 직접 할당 되는 해당 c # 페이지는 [`Style`](xref:Xamarin.Forms.NavigableElement.Style) 다음 코드 예제에 나와 있습니다.

```csharp
public class StyleInheritancePageCS : ContentPage
{
    public StyleInheritancePageCS ()
    {
        var baseStyle = new Style (typeof(View)) {
            Setters = {
                new Setter {
                    Property = View.HorizontalOptionsProperty, Value = LayoutOptions.Center    },
                ...
            }
        };

        var labelStyle = new Style (typeof(Label)) {
            BasedOn = baseStyle,
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Teal    }
            }
        };

        var buttonStyle = new Style (typeof(Button)) {
            BasedOn = baseStyle,
            Setters = {
                new Setter { Property = Button.BorderColorProperty, Value =    Color.Lime },
                ...
            }
        };
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelStyle },
                ...
                new Button { Text = "So is the button", Style = buttonStyle }
            }
        };
    }
}
```

는 `baseStyle` 대상 [`View`](xref:Xamarin.Forms.View) 인스턴스를 지정 하 고 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 및 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성을 설정 합니다. 는 `baseStyle` 컨트롤에서 직접 설정 되지 않습니다. 대신, `labelStyle` 및 `buttonStyle` 에서 상속 되며 추가 바인딩 가능한 속성 값을 설정 합니다. `labelStyle` `buttonStyle` 그런 다음 및는 [`Label`](xref:Xamarin.Forms.Label) [`Button`](xref:Xamarin.Forms.Button) 해당 속성을 설정 하 여 인스턴스 및 인스턴스에 적용 됩니다 [`Style`](xref:Xamarin.Forms.NavigableElement.Style) .

## <a name="related-links"></a>관련 링크

- [XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [기본 스타일 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [스타일 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
