---
title: Xamarin.FormsContentView
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
ms.openlocfilehash: 46d2abf895ffe31bd1dc1c22caf36440c54b331c
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84130119"
---
# <a name="xamarinforms-contentview"></a>Xamarin.FormsContentView

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-contentviewdemos/)

Xamarin.Forms [`ContentView`](xref:Xamarin.Forms.ContentView) 클래스는 `Layout` 단일 자식 요소를 포함 하 고 재사용 가능한 사용자 지정 컨트롤을 만드는 데 일반적으로 사용 되는의 형식입니다. `ContentView`클래스는에서 상속 [`TemplatedView`](xref:Xamarin.Forms.TemplatedView) 됩니다. 이 문서 및 관련 샘플에서는 클래스를 기반으로 사용자 지정 컨트롤을 만드는 방법을 설명 `CardView` `ContentView` 합니다.

다음 스크린샷은 `CardView` 클래스에서 파생 되는 컨트롤을 보여 줍니다 `ContentView` .

[![CardView 샘플 응용 프로그램 스크린샷](contentview-images/cardview-list-cropped.png)](contentview-images/cardview-list.png#lightbox)

`ContentView`클래스는 단일 속성을 정의 합니다.

* [`Content`](xref:Xamarin.Forms.ContentView.Content)는 `View` 개체입니다. 이 속성은 개체에 의해 지원 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 되므로 데이터 바인딩의 대상이 될 수 있습니다.

는 `ContentView` 또한 클래스에서 속성을 상속 합니다 `TemplatedView` .

* [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate)는 `ControlTemplate` 컨트롤의 모양을 정의 하거나 재정의할 수 있는입니다.

속성에 대 한 자세한 내용은 `ControlTemplate` ControlTemplate를 [사용 하 여 모양 사용자 지정](#customize-appearance-with-a-controltemplate)을 참조 하세요.

## <a name="create-a-custom-control"></a>사용자 지정 컨트롤 만들기

`ContentView`클래스는 자체적으로 거의 기능을 제공 하지 않지만 사용자 지정 컨트롤을 만드는 데 사용할 수 있습니다. 샘플 프로젝트는 `CardView` 이미지, 제목 및 설명을 카드와 같은 레이아웃으로 표시 하는 UI 요소 컨트롤을 정의 합니다.

사용자 지정 컨트롤을 만드는 프로세스는 다음과 같습니다.

1. Visual Studio 2019에서 템플릿을 사용 하 여 새 클래스를 만듭니다 `ContentView` .
1. 새 사용자 지정 컨트롤에 대 한 코드 숨겨진 파일의 고유한 속성 또는 이벤트를 정의 합니다.
1. 사용자 지정 컨트롤에 대 한 UI를 만듭니다.

> [!NOTE]
> 레이아웃이 XAML이 아닌 코드에 정의 되어 있는 사용자 지정 컨트롤을 만들 수 있습니다. 간단히 하기 위해 샘플 응용 프로그램은 `CardView` XAML 레이아웃을 사용 하는 단일 클래스만 정의 합니다. 그러나 샘플 응용 프로그램에는 코드에서 사용자 지정 컨트롤을 사용 하는 프로세스를 보여 주는, 코드에서 사용자 지정 **컨트롤을 사용** 하는 프로세스를 보여 주는

### <a name="create-code-behind-properties"></a>코드 숨겨진 속성 만들기

`CardView`사용자 지정 컨트롤은 다음 속성을 정의 합니다.

* `CardTitle`: `string` 카드에 표시 되는 제목을 나타내는 개체입니다.
* `CardDescription`: `string` 카드에 표시 된 설명을 나타내는 개체입니다.
* `IconImageSource`: `ImageSource` 카드에 표시 되는 이미지를 나타내는 개체입니다.
* `IconBackgroundColor`: `Color` 카드에 표시 된 이미지의 배경색을 나타내는 개체입니다.
* `BorderColor`: `Color` 카드 테두리, 이미지 테두리 및 구분선의 색을 나타내는 개체입니다.
* `CardColor`: `Color` 카드의 배경색을 나타내는 개체입니다.

> [!NOTE]
> 속성은 데모를 위해 `BorderColor` 여러 항목에 영향을 줍니다. 필요한 경우이 속성을 세 가지 속성으로 구분할 수 있습니다.

각 속성은 인스턴스에 의해 지원 됩니다 `BindableProperty` . 백업을 `BindableProperty` 사용 하면 MVVM 패턴을 사용 하 여 각 속성의 스타일을 지정 하 고 바인딩할 수 있습니다.

다음 예에서는 백업을 만드는 방법을 보여 줍니다 `BindableProperty` .

```csharp
public static readonly BindableProperty CardTitleProperty = BindableProperty.Create(
    "CardTitle",        // the name of the bindable property
    typeof(string),     // the bindable property type
    typeof(CardView),   // the parent object type
    string.Empty);      // the default value for the property
```

사용자 지정 속성은 및 메서드를 사용 하 여 `GetValue` `SetValue` 개체 값을 가져오고 설정 합니다 `BindableProperty` .

```csharp
public string CardTitle
{
    get => (string)GetValue(CardView.CardTitleProperty);
    set => SetValue(CardView.CardTitleProperty, value);
}
```

개체에 대 한 자세한 내용은 `BindableProperty` [바인딩 가능한 속성](~/xamarin-forms/xaml/bindable-properties.md)을 참조 하세요.

### <a name="define-ui"></a>UI 정의

사용자 지정 컨트롤 UI는를 `ContentView` 컨트롤의 루트 요소로 사용 합니다 `CardView` . 다음 예제에서는 XAML을 보여 줍니다 `CardView` .

```XAML
<ContentView ...
             x:Name="this"
             x:Class="CardViewDemo.Controls.CardView">
    <Frame BindingContext="{x:Reference this}"
            BackgroundColor="{Binding CardColor}"
            BorderColor="{Binding BorderColor}"
            ...>
        <Grid>
            ...
            <Frame BorderColor="{Binding BorderColor, FallbackValue='Black'}"
                   BackgroundColor="{Binding IconBackgroundColor, FallbackValue='Grey'}"
                   ...>
                <Image Source="{Binding IconImageSource}"
                       .. />
            </Frame>
            <Label Text="{Binding CardTitle, FallbackValue='Card Title'}"
                   ... />
            <BoxView BackgroundColor="{Binding BorderColor, FallbackValue='Black'}"
                     ... />
            <Label Text="{Binding CardDescription, FallbackValue='Card description text.'}"
                   ... />
        </Grid>
    </Frame>
</ContentView>
```

`ContentView`요소는 속성을로 설정 합니다 `x:Name` . **이**속성은 인스턴스에 바인딩된 개체에 액세스 하는 데 사용할 수 있습니다 `CardView` . 레이아웃의 요소는 해당 속성에 대 한 바인딩을 바인딩된 개체에 정의 된 값으로 설정 합니다.

데이터 바인딩에 대 한 자세한 내용은 [ Xamarin.Forms 데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)을 참조 하세요.

> [!NOTE]
> `FallbackValue`바인딩이 인 경우 속성은 기본값을 제공 합니다 `null` . 이를 통해 Visual Studio에서 [XAML 미리 보기](~/xamarin-forms/xaml/xaml-previewer/index.md) 를 사용 하 여 컨트롤을 렌더링할 수도 있습니다 `CardView` .

## <a name="instantiate-a-custom-control"></a>사용자 지정 컨트롤 인스턴스화

사용자 지정 컨트롤을 인스턴스화하는 페이지에 사용자 지정 컨트롤 네임 스페이스에 대 한 참조를 추가 해야 합니다. 다음 예제에서는 XAML의 인스턴스에 추가 된 **컨트롤** 이라는 네임 스페이스 참조를 보여 줍니다 `ContentPage` .

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CardViewDemo.Controls" >
```

참조가 추가 된 후에는 `CardView` XAML에서 인스턴스화할 수 있고 해당 속성은 다음과 같이 정의 됩니다.

```xaml
<controls:CardView BorderColor="DarkGray"
                   CardTitle="Slavko Vlasic"
                   CardDescription="Lorem ipsum dolor sit..."
                   IconBackgroundColor="SlateGray"
                   IconImageSource="user.png"/>
```

`CardView`코드에서를 인스턴스화할 수도 있습니다.

```csharp
CardView card = new CardView
{
    BorderColor = Color.DarkGray,
    CardTitle = "Slavko Vlasic",
    CardDescription = "Lorem ipsum dolor sit...",
    IconBackgroundColor = Color.SlateGray,
    IconImageSource = ImageSource.FromFile("user.png")
};
```

## <a name="customize-appearance-with-a-controltemplate"></a>ControlTemplate를 사용 하 여 모양 사용자 지정

클래스에서 파생 된 사용자 지정 컨트롤은 `ContentView` XAML, 코드를 사용 하 여 모양을 정의 하거나 모양을 정의 하지 않을 수 있습니다. 모양이 정의 된 방법에 관계 없이 `ControlTemplate` 개체는 사용자 지정 레이아웃을 사용 하 여 모양을 재정의할 수 있습니다.

`CardView`일부 사용 사례에서는 레이아웃이 너무 많은 공간을 차지할 수 있습니다. 에서는 `ControlTemplate` 레이아웃을 재정의 하 여 압축 된 `CardView` 목록에 적합 한 보다 간결한 뷰를 제공할 수 있습니다.

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <ControlTemplate x:Key="CardViewCompressed">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="100" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="100" />
                    <ColumnDefinition Width="100*" />
                </Grid.ColumnDefinitions>
                <Image Grid.Row="0"
                       Grid.Column="0"
                       Source="{TemplateBinding IconImageSource}"
                       BackgroundColor="{TemplateBinding IconBackgroundColor}"
                       WidthRequest="100"
                       HeightRequest="100"
                       Aspect="AspectFill"
                       HorizontalOptions="Center"
                       VerticalOptions="Center"/>
                <StackLayout Grid.Row="0"
                             Grid.Column="1">
                    <Label Text="{TemplateBinding CardTitle}"
                           FontAttributes="Bold" />
                    <Label Text="{TemplateBinding CardDescription}" />
                </StackLayout>
            </Grid>
        </ControlTemplate>
    </ResourceDictionary>
</ContentPage.Resources>
```

의 데이터 바인딩은 `ControlTemplate` 태그 확장을 사용 하 여 `TemplateBinding` 바인딩을 지정 합니다. `ControlTemplate`그런 다음 해당 값을 사용 하 여 정의 된 ControlTemplate 개체에 속성을 설정할 수 있습니다 `x:Key` . 다음 예에서는 인스턴스에 설정 된 속성을 보여 줍니다 `ControlTemplate` `CardView` .

```xaml
<controls:CardView ControlTemplate="{StaticResource CardViewCompressed}"/>
```

다음 스크린샷에는 `CardView` `CardView` 가 재정의 된 표준 인스턴스가 나와 `ControlTemplate` 있습니다.

[![CardView ControlTemplate 스크린 샷](contentview-images/cardview-controltemplates-cropped.png)](contentview-images/cardview-controltemplates.png#lightbox)

컨트롤 템플릿에 대 한 자세한 내용은 [ Xamarin.Forms 컨트롤 템플릿](~/xamarin-forms/app-fundamentals/templates/control-template.md)을 참조 하세요.

## <a name="related-links"></a>관련 링크

* [ContentView 샘플 응용 프로그램](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-contentviewdemos/)
* [Xamarin.Forms데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)
* [바인딩 가능한 속성](~/xamarin-forms/xaml/bindable-properties.md)입니다.
* [Xamarin.Forms컨트롤 템플릿](~/xamarin-forms/app-fundamentals/templates/control-template.md)
