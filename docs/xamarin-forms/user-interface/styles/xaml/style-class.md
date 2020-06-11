---
제목: "스타일 Xamarin.Forms 클래스" 설명: "스타일 Xamarin.Forms 클래스를 사용 하면 스타일 상속을 사용 하지 않고 컨트롤에 여러 스타일을 적용할 수 있습니다."
assetid: 4762401E-2B48-48F1-B6E4-61F7AF8AA46F: xamarin-forms author: davidbritch: dabritch:: 01/30/2019-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xamarinforms-style-classes"></a>Xamarin.Forms 스타일 클래스

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_Xamarin.ios 스타일 클래스를 사용 하면 스타일 상속을 사용 하지 않고 컨트롤에 여러 스타일을 적용할 수 있습니다._

## <a name="create-style-classes"></a>스타일 클래스 만들기

스타일 클래스는 [`Class`](xref:Xamarin.Forms.Style.Class) [`Style`](xref:Xamarin.Forms.Style) 클래스 이름을 나타내는에 대 한 속성을 설정 하 여 만들 수 있습니다 `string` . 이에서 제공 하는 이점에는 특성을 사용 하 여 명시적 스타일을 정의 하는 `x:Key` 것 보다 여러 스타일 클래스를에 적용할 수 있습니다 [`VisualElement`](xref:Xamarin.Forms.VisualElement) .

> [!IMPORTANT]
> 여러 스타일이 서로 다른 형식을 대상으로 하는 경우 동일한 클래스 이름을 공유할 수 있습니다. 이렇게 하면 이름이 같은 여러 스타일 클래스를 사용 하 여 다른 형식을 대상으로 지정할 수 있습니다.

다음 예제에서는 세 가지 [`BoxView`](xref:Xamarin.Forms.BoxView) 스타일 클래스와 스타일 클래스를 보여 줍니다 [`VisualElement`](xref:Xamarin.Forms.VisualElement) .

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="BoxView"
               Class="Separator">
            <Setter Property="BackgroundColor"
                    Value="#CCCCCC" />
            <Setter Property="HeightRequest"
                    Value="1" />
        </Style>

        <Style TargetType="BoxView"
               Class="Rounded">
            <Setter Property="BackgroundColor"
                    Value="#1FAECE" />
            <Setter Property="HorizontalOptions"
                    Value="Start" />
            <Setter Property="CornerRadius"
                    Value="10" />
        </Style>    

        <Style TargetType="BoxView"
               Class="Circle">
            <Setter Property="BackgroundColor"
                    Value="#1FAECE" />
            <Setter Property="WidthRequest"
                    Value="100" />
            <Setter Property="HeightRequest"
                    Value="100" />
            <Setter Property="HorizontalOptions"
                    Value="Start" />
            <Setter Property="CornerRadius"
                    Value="50" />
        </Style>

        <Style TargetType="VisualElement"
               Class="Rotated"
               ApplyToDerivedTypes="true">
            <Setter Property="Rotation"
                    Value="45" />
        </Style>        
    </ContentPage.Resources>
</ContentPage>
```

`Separator`, `Rounded` 및 `Circle` 스타일 클래스는 각각 [`BoxView`](xref:Xamarin.Forms.BoxView) 속성을 특정 값으로 설정 합니다.

`Rotated`Style 클래스에는의가 있습니다 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) [`VisualElement`](xref:Xamarin.Forms.VisualElement) . 즉, 인스턴스에만 적용할 수 있습니다 `VisualElement` . 그러나 해당 [`ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) 속성은로 설정 됩니다 `true` . 그러면와 같이에서 파생 된 모든 컨트롤에 적용할 수 있습니다 `VisualElement` [`BoxView`](xref:Xamarin.Forms.BoxView) . 파생 형식에 스타일을 적용 하는 방법에 대 한 자세한 내용은 [파생 형식에 스타일 적용](implicit.md#apply-a-style-to-derived-types)을 참조 하세요.

해당하는 C# 코드는 다음과 같습니다.

```csharp
var separatorBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Separator",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#CCCCCC")
        },
        new Setter
        {
            Property = VisualElement.HeightRequestProperty,
            Value = 1
        }
    }
};

var roundedBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Rounded",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#1FAECE")
        },
        new Setter
        {
            Property = View.HorizontalOptionsProperty,
            Value = LayoutOptions.Start
        },
        new Setter
        {
            Property = BoxView.CornerRadiusProperty,
            Value = 10
        }
    }
};

var circleBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Circle",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#1FAECE")
        },
        new Setter
        {
            Property = VisualElement.WidthRequestProperty,
            Value = 100
        },
        new Setter
        {
            Property = VisualElement.HeightRequestProperty,
            Value = 100
        },
        new Setter
        {
            Property = View.HorizontalOptionsProperty,
            Value = LayoutOptions.Start
        },
        new Setter
        {
            Property = BoxView.CornerRadiusProperty,
            Value = 50
        }
    }
};

var rotatedVisualElementStyle = new Style(typeof(VisualElement))
{
    Class = "Rotated",
    ApplyToDerivedTypes = true,
    Setters =
    {
        new Setter
        {
            Property = VisualElement.RotationProperty,
            Value = 45
        }
    }
};

Resources = new ResourceDictionary
{
    separatorBoxViewStyle,
    roundedBoxViewStyle,
    circleBoxViewStyle,
    rotatedVisualElementStyle
};
```

## <a name="consume-style-classes"></a>스타일 클래스 사용

스타일 클래스는 [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass) 형식의 컨트롤 속성을 `IList<string>` 스타일 클래스 이름 목록으로 설정 하 여 사용할 수 있습니다. 컨트롤의 형식이 스타일 클래스의와 일치 하는 경우 스타일 클래스가 적용 됩니다 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) .

다음 예제에서는 [`BoxView`](xref:Xamarin.Forms.BoxView) 각각 다른 스타일 클래스로 설정 된 세 가지 인스턴스를 보여 줍니다.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        ...
    </ContentPage.Resources>
    <StackLayout Margin="20">
        <BoxView StyleClass="Separator" />       
        <BoxView WidthRequest="100"
                 HeightRequest="100"
                 HorizontalOptions="Center"
                 StyleClass="Rounded, Rotated" />
        <BoxView HorizontalOptions="Center"
                 StyleClass="Circle" />
    </StackLayout>
</ContentPage>    
```

이 예제에서 첫 번째는 [`BoxView`](xref:Xamarin.Forms.BoxView) 줄 분리자로 스타일이 지정 되 고 세 번째는 `BoxView` 원형입니다. 두 번째는 `BoxView` 두 개의 스타일 클래스를 적용 하며,이 클래스는 둥근 모퉁이를 제공 하 고 45도 회전 합니다.

![스타일 클래스를 사용 하 여 스타일 지정 된 BoxViews](style-class-images/boxviews.png)

> [!IMPORTANT]
> 속성이 형식이 기 때문에 여러 스타일 클래스를 컨트롤에 적용할 수 있습니다 [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass) `IList<string>` . 이 경우 스타일 클래스는 오름차순 목록 순서로 적용 됩니다. 따라서 여러 스타일 클래스가 동일한 속성을 설정 하는 경우 가장 높은 목록 위치에 있는 스타일 클래스의 속성이 우선적으로 적용 됩니다.

해당하는 C# 코드는 다음과 같습니다.

```csharp
...
Content = new StackLayout
{
    Children =
    {
        new BoxView { StyleClass = new [] { "Separator" } },
        new BoxView { WidthRequest = 100, HeightRequest = 100, HorizontalOptions = LayoutOptions.Center, StyleClass = new [] { "Rounded", "Rotated" } },
        new BoxView { HorizontalOptions = LayoutOptions.Center, StyleClass = new [] { "Circle" } }
    }
};
```

## <a name="related-links"></a>관련 링크

- [기본 스타일 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
