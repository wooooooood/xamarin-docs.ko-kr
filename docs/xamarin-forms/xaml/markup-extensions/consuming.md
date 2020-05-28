---
title: ''
description: 이 문서에서는 Xamarin.Forms 다양 한 소스에서 요소 특성을 설정할 수 있도록 하 여 xaml 태그 확장을 사용 하 여 xaml의 기능과 유연성을 향상 시키는 방법을 설명 합니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7cd3a9d275919240fcaa7315d5043854df5529bb
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84127324"
---
# <a name="consuming-xaml-markup-extensions"></a>XAML 태그 확장 사용

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

XAML 태그 확장은 다양 한 소스에서 요소 특성을 설정할 수 있도록 하 여 XAML의 기능과 유연성을 향상 시키는 데 도움이 됩니다. 몇 가지 XAML 태그 확장은 XAML 2009 사양의 일부입니다. 이러한 파일은 일반적인 네임 스페이스 접두사를 사용 하 여 XAML 파일에 표시 `x` 되며 일반적으로이 접두사를 사용 하 여 참조 됩니다. 이 문서에서는 다음과 같은 태그 확장에 대해 설명 합니다.

- [`x:Static`](#static)– 정적 속성, 필드 또는 열거형 멤버를 참조 합니다.
- [`x:Reference`](#reference)– 페이지의 명명 된 요소를 참조 합니다.
- [`x:Type`](#type)– 특성을 `System.Type` 개체로 설정 합니다.
- [`x:Array`](#array)– 특정 형식의 개체 배열을 생성 합니다.
- [`x:Null`](#null)– 특성을 값으로 설정 `null` 합니다.
- [`OnPlatform`](#onplatform)– 플랫폼 별로 UI 모양을 사용자 지정 합니다.
- [`OnIdiom`](#onidiom)– 응용 프로그램이 실행 되 고 있는 장치를 기준으로 UI 모양을 사용자 지정 합니다.
- [`DataTemplate`](#datatemplate-markup-extension)– 형식을로 변환 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 합니다.
- [`FontImage`](#fontimage-markup-extension)–를 표시할 수 있는 모든 보기에 글꼴 아이콘을 표시 `ImageSource` 합니다.
- [`OnAppTheme`](#onapptheme-markup-extension)– 현재 시스템 테마를 기반으로 리소스를 사용 합니다.

추가 XAML 태그 확장은 이전에 다른 XAML 구현에서 지원 되었으며 에서도 지원 됩니다 Xamarin.Forms . 이에 대해서는 다른 문서에 자세히 설명 되어 있습니다.

- `StaticResource`- [**리소스 사전 문서에**](~/xamarin-forms/xaml/resource-dictionaries.md)설명 된 대로 리소스 사전에서 개체를 참조 합니다.
- `DynamicResource`- [**동적 스타일**](~/xamarin-forms/user-interface/styles/dynamic.md)문서에 설명 된 대로 리소스 사전에 있는 개체의 변경 내용에 응답 합니다.
- `Binding`- [**데이터 바인딩**](~/xamarin-forms/app-fundamentals/data-binding/index.md)문서에 설명 된 대로 두 개체의 속성 간에 링크를 설정 합니다.
- `TemplateBinding`-컨트롤 [** Xamarin.Forms 템플릿**](~/xamarin-forms/app-fundamentals/templates/control-template.md)문서에 설명 된 대로 컨트롤 템플릿에서 데이터 바인딩을 수행 합니다.
- `RelativeSource`- [상대 바인딩](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md)문서에 설명 된 대로 바인딩 대상의 위치를 기준으로 바인딩 소스를 설정 합니다.

[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)레이아웃은 사용자 지정 태그 확장을 사용 합니다 [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression) . 이 태그 확장은 [**RelativeLayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md)문서에 설명 되어 있습니다.

<a name="static" />

## <a name="xstatic-markup-extension"></a>x:Static 태그 확장

`x:Static`태그 확장은 클래스에서 지원 됩니다 [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension) . 클래스에는 [`Member`](xref:Xamarin.Forms.Xaml.StaticExtension.Member) `string` public 상수, 정적 속성, 정적 필드 또는 열거형 멤버의 이름으로 설정 하는 형식 이라는 단일 속성이 있습니다.

를 사용 하는 일반적인 방법 중 하나 `x:Static` 는 먼저 `AppConstants` [**MarkupExtensions**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions) 프로그램에서이 작은 클래스와 같은 일부 상수 또는 정적 변수를 사용 하 여 클래스를 정의 하는 것입니다.

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

**X:Static 데모** 페이지에서는 태그 확장을 사용 하는 여러 가지 방법을 보여 줍니다 `x:Static` . 가장 자세한 방법은 `StaticExtension` `Label.FontSize` 속성-요소 태그 사이에 클래스를 인스턴스화하는 것입니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=netstandard"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.StaticDemoPage"
             Title="x:Static Demo">
    <StackLayout Margin="10, 0">
        <Label Text="Label No. 1">
            <Label.FontSize>
                <x:StaticExtension Member="local:AppConstants.NormalFontSize" />
            </Label.FontSize>
        </Label>

        ···

    </StackLayout>
</ContentPage>
```

또한 XAML 파서는 `StaticExtension` 클래스를 다음과 같이 약식으로 지정할 수 있습니다 `x:Static` .

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

이 작업은 더 간단 하 게 수행할 수 있지만 변경 내용에는 `StaticExtension` 클래스 및 멤버 설정을 중괄호 안에 배치 하는 기능이 포함 됩니다. 결과 식은 특성에 직접 설정 됩니다 `FontSize` .

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

중괄호 안에는 따옴표가 *없습니다* . `Member`의 속성은 `StaticExtension` 더 이상 XML 특성이 아닙니다. 대신 태그 확장에 대 한 식의 일부입니다.

`x:StaticExtension`개체 요소로 사용 하는 경우 약어를 사용 하는 것과 마찬가지로 `x:Static` 중괄호 안에 있는 식에서 약어를 약어로 지정할 수도 있습니다.

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

클래스에는 `StaticExtension` `ContentProperty` `Member` 이 속성을 클래스의 기본 콘텐츠 속성으로 표시 하는 속성을 참조 하는 특성이 있습니다. 중괄호로 표시 된 XAML 태그 확장의 경우 식의 일부를 제거할 수 있습니다 `Member=` .

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

태그 확장의 가장 일반적인 형식입니다 `x:Static` .

**정적 데모** 페이지에는 다른 두 가지 예가 포함 되어 있습니다. XAML 파일의 루트 태그에는 .NET 네임 스페이스에 대 한 XML 네임 스페이스 선언이 포함 되어 있습니다 `System` .

```xaml
xmlns:sys="clr-namespace:System;assembly=netstandard"
```

이렇게 하면 `Label` 글꼴 크기를 정적 필드로 설정할 수 있습니다 `Math.PI` . 이로 인해 작은 텍스트가 되므로 `Scale` 속성은로 설정 됩니다 `Math.E` .

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

마지막 예제에서는 값을 표시 합니다 `Device.RuntimePlatform` . `Environment.NewLine`Static 속성은 두 개체 사이에 줄 바꿈 문자를 삽입 하는 데 사용 됩니다 `Span` .

```xaml
<Label HorizontalTextAlignment="Center"
       FontSize="{x:Static local:AppConstants.NormalFontSize}">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Runtime Platform: " />
            <Span Text="{x:Static sys:Environment.NewLine}" />
            <Span Text="{x:Static Device.RuntimePlatform}" />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

실행 되는 샘플은 다음과 같습니다.

[![x:Static Demo](consuming-images/staticdemo-small.png "x:Static Demo")](consuming-images/staticdemo-large.png#lightbox "x:Static Demo")

<a name="reference" />

## <a name="xreference-markup-extension"></a>x:Reference 태그 확장

`x:Reference`태그 확장은 클래스에서 지원 됩니다 [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension) . 클래스에는 [`Name`](xref:Xamarin.Forms.Xaml.ReferenceExtension.Name) `string` 이름이 지정 된 페이지의 요소 이름으로 설정 하는 형식 이라는 단일 속성이 있습니다 `x:Name` . 이 `Name` 속성은의 content 속성 이므로가 중괄호 `ReferenceExtension` 안에 표시 되는 `Name=` 경우에는 필요 하지 않습니다 `x:Reference` .

`x:Reference`태그 확장은 데이터 바인딩에 독점적으로 사용 되며이에 대해서는 문서 [**데이터 바인딩에**](~/xamarin-forms/app-fundamentals/data-binding/index.md)자세히 설명 되어 있습니다.

**X:reference Demo** 페이지는 데이터 바인딩과 함께의 두 가지 사용 `x:Reference` , 즉 개체의 속성을 설정 하는 데 사용 되는 첫 번째 `Source` `Binding` 및 두 번째 데이터 바인딩의 속성을 설정 하는 데 사용 되는 두 번째를 보여 줍니다 `BindingContext` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.ReferenceDemoPage"
             x:Name="page"
             Title="x:Reference Demo">

    <StackLayout Margin="10, 0">

        <Label Text="{Binding Source={x:Reference page},
                              StringFormat='The type of this page is {0}'}"
               FontSize="18"
               VerticalOptions="CenterAndExpand"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="Center" />

        <Label BindingContext="{x:Reference slider}"
               Text="{Binding Value, StringFormat='{0:F0}&#x00B0; rotation'}"
               Rotation="{Binding Value}"
               FontSize="24"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

    </StackLayout>
</ContentPage>
```

두 `x:Reference` 식 모두 약식 버전의 `ReferenceExtension` 클래스 이름을 사용 하 여 `Name=` 식의 일부를 제거 합니다. 첫 번째 예제에서 `x:Reference` 태그 확장은 태그 확장에 포함 되어 `Binding` 있습니다. `Source`및 `StringFormat` 설정은 쉼표로 구분 됩니다. 실행 중인 프로그램은 다음과 같습니다.

[![x:Reference 데모](consuming-images/referencedemo-small.png "x:Reference 데모")](consuming-images/referencedemo-large.png#lightbox "x:Reference 데모")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type 태그 확장

`x:Type`태그 확장은 c # 키워드와 동일한 XAML입니다 [`typeof`](/dotnet/csharp/language-reference/keywords/typeof/) . 클래스 [`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension) [`TypeName`](xref:Xamarin.Forms.Xaml.TypeExtension.TypeName) `string` 또는 구조체 이름으로 설정 된 형식의 속성 하나를 정의 하는 클래스에서 지원 됩니다. `x:Type`태그 확장은 [`System.Type`](xref:System.Type) 해당 클래스 또는 구조체의 개체를 반환 합니다. `TypeName`는의 content 속성 `TypeExtension` 이므로 `TypeName=` `x:Type` 중괄호와 함께 표시 되는 경우에는 필요 하지 않습니다.

에는 Xamarin.Forms 형식의 인수를 포함 하는 몇 가지 속성이 있습니다 `Type` . 예제에는 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) 의 속성과 `Style` 제네릭 클래스에서 인수를 지정 하는 데 사용 되는 [x:TypeArguments](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments) 특성이 포함 됩니다. 그러나 XAML 파서는 `typeof` 작업을 자동으로 수행 하며 `x:Type` 태그 확장은 이러한 경우에 사용 되지 않습니다.

`x:Type` *이* 필요한 한 곳은 태그 확장을 사용 하는 것입니다 .이에 `x:Array` 대해서는 [다음 섹션](#array)에서 설명 합니다.

`x:Type`태그 확장은 각 메뉴 항목이 특정 형식의 개체에 해당 하는 메뉴를 생성 하는 경우에도 유용 합니다. `Type`개체를 각 메뉴 항목과 연결한 다음 메뉴 항목을 선택할 때 개체를 인스턴스화할 수 있습니다.

`MainPage` **태그 확장** 프로그램의 탐색 메뉴가 작동 하는 방식입니다. **Mainpage** 파일에는 `TableView` `TextCell` 프로그램의 특정 페이지에 해당 하는가 포함 되어 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.MainPage"
             Title="Markup Extensions"
             Padding="10">
    <TableView Intent="Menu">
        <TableRoot>
            <TableSection>
                <TextCell Text="x:Static Demo"
                          Detail="Access constants or statics"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:StaticDemoPage}" />

                <TextCell Text="x:Reference Demo"
                          Detail="Reference named elements on the page"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:ReferenceDemoPage}" />

                <TextCell Text="x:Type Demo"
                          Detail="Associate a Button with a Type"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:TypeDemoPage}" />

                <TextCell Text="x:Array Demo"
                          Detail="Use an array to fill a ListView"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:ArrayDemoPage}" />

                ···                          

        </TableRoot>
    </TableView>
</ContentPage>
```

**태그 확장**에서 열리는 기본 페이지는 다음과 같습니다.

[![기본 페이지](consuming-images/mainpage-small.png "기본 페이지")](consuming-images/mainpage-large.png#lightbox "기본 페이지")

각 `CommandParameter` 속성은 `x:Type` 다른 페이지 중 하나를 참조 하는 태그 확장으로 설정 됩니다. `Command`속성은 라는 속성에 바인딩됩니다 `NavigateCommand` . 이 속성은 코드 파일에 정의 되어 `MainPage` 있습니다.

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(async (Type pageType) =>
        {
            Page page = (Page)Activator.CreateInstance(pageType);
            await Navigation.PushAsync(page);
        });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

`NavigateCommand`속성은 `Command` 값 형식의 인수를 사용 하 여 execute 명령을 구현 하는 개체입니다 `Type` &mdash; `CommandParameter` . 메서드는를 사용 하 여 `Activator.CreateInstance` 페이지를 인스턴스화한 다음 탐색 합니다. 생성자는 `BindingContext` 페이지의를 자신으로 설정 하 여이 완료 될 수 있도록 `Binding` 합니다 `Command` . 이러한 코드 형식에 대 한 자세한 내용은 [**데이터 바인딩**](~/xamarin-forms/app-fundamentals/data-binding/index.md) 문서 및 특히 [**명령**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) 문서를 참조 하세요.

**X:Type Demo** 페이지는 유사한 기법을 사용 하 여 요소를 인스턴스화하고에 Xamarin.Forms 추가 `StackLayout` 합니다. XAML 파일은 처음에 속성이로 `Button` `Command` 설정 되 `Binding` 고 `CommandParameter` 속성이 다음 세 가지 뷰 형식으로 설정 된 세 개의 요소로 구성 됩니다 Xamarin.Forms .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.TypeDemoPage"
             Title="x:Type Demo">

    <StackLayout x:Name="stackLayout"
                 Padding="10, 0">

        <Button Text="Create a Slider"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Slider}" />

        <Button Text="Create a Stepper"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Stepper}" />

        <Button Text="Create a Switch"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Switch}" />
    </StackLayout>
</ContentPage>
```

코드 숨김이 파일은 속성을 정의 하 고 초기화 합니다 `CreateCommand` .

```csharp
public partial class TypeDemoPage : ContentPage
{
    public TypeDemoPage()
    {
        InitializeComponent();

        CreateCommand = new Command<Type>((Type viewType) =>
        {
            View view = (View)Activator.CreateInstance(viewType);
            view.VerticalOptions = LayoutOptions.CenterAndExpand;
            stackLayout.Children.Add(view);
        });

        BindingContext = this;
    }

    public ICommand CreateCommand { private set; get; }
}
```

을 누를 때 실행 되는 메서드는 `Button` 인수의 새 인스턴스를 만들고, 해당 속성을 설정 하 `VerticalOptions` 고,이를에 추가 `StackLayout` 합니다. 그러면 세 개의 `Button` 요소가 동적으로 생성 된 뷰로 페이지를 공유 합니다.

[![x:Type 데모](consuming-images/typedemo-small.png "x:Type 데모")](consuming-images/typedemo-large.png#lightbox "x:Type 데모")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array 태그 확장

`x:Array`태그 확장을 사용 하면 태그에서 배열을 정의할 수 있습니다. 이 [`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension) 클래스는 두 가지 속성을 정의 하는 클래스에서 지원 됩니다.

- `Type``Type`배열의 요소 형식을 나타내는 형식의입니다.
- `Items``IList`항목 자체의 컬렉션인 형식의입니다. 의 콘텐츠 속성입니다 `ArrayExtension` .

`x:Array`태그 확장 자체는 중괄호로 표시 되지 않습니다. 대신 `x:Array` 시작 및 끝 태그는 항목 목록을 구분 합니다. 속성을 `Type` 태그 확장으로 설정 합니다 `x:Type` .

**X:Array Demo** 페이지는를 사용 하 여에 항목을 추가 하는 방법을 보여 줍니다 `x:Array` `ListView` `ItemsSource` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.ArrayDemoPage"
             Title="x:Array Demo Page">
    <ListView Margin="10">
        <ListView.ItemsSource>
            <x:Array Type="{x:Type Color}">
                <Color>Aqua</Color>
                <Color>Black</Color>
                <Color>Blue</Color>
                <Color>Fuchsia</Color>
                <Color>Gray</Color>
                <Color>Green</Color>
                <Color>Lime</Color>
                <Color>Maroon</Color>
                <Color>Navy</Color>
                <Color>Olive</Color>
                <Color>Pink</Color>
                <Color>Purple</Color>
                <Color>Red</Color>
                <Color>Silver</Color>
                <Color>Teal</Color>
                <Color>White</Color>
                <Color>Yellow</Color>
            </x:Array>
        </ListView.ItemsSource>

        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <BoxView Color="{Binding}"
                             Margin="3" />    
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>        
```

는 `ViewCell` `BoxView` 각 색 항목에 대해 간단한를 만듭니다.

[![x:Array Demo](consuming-images/arraydemo-small.png "x:Array Demo")](consuming-images/arraydemo-large.png#lightbox "x:Array Demo")

이 배열에서 개별 항목을 지정 하는 방법에는 여러 가지가 있습니다 `Color` . 태그 확장을 사용할 수 있습니다 `x:Static` .

```xaml
<x:Static Member="Color.Blue" />
```

또는를 사용 하 여 `StaticResource` 리소스 사전에서 색을 검색할 수 있습니다.

```xaml
<StaticResource Key="myColor" />
```

이 문서의 끝 부분을 향해 새 색 값을 만드는 사용자 지정 XAML 태그 확장이 표시 됩니다.

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

문자열이 나 숫자와 같은 공용 형식의 배열을 정의할 때는 [**생성자 인수 전달**](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) 문서에 나열 된 태그를 사용 하 여 값을 구분 합니다.

<a name="null" />

## <a name="xnull-markup-extension"></a>x:Null 태그 확장명

`x:Null`태그 확장은 클래스에서 지원 됩니다 [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension) . 이 클래스에는 속성이 없으며 단순히 c # 키워드와 동일한 XAML이 [`null`](/dotnet/csharp/language-reference/keywords/null/) 있습니다.

`x:Null`태그 확장은 거의 필요 하지 않지만 거의 사용 되지 않지만 필요한 경우에는 해당 항목이 존재 하는 것이 좋습니다.

**X:Null Demo** 페이지는에서 편리 하 게 사용할 수 있는 한 가지 시나리오를 보여 줍니다 `x:Null` . `Style` `Label` `Setter` 속성을 `FontFamily` 플랫폼 종속 패밀리 이름으로 설정 하는를 포함 하는에 대 한 암시적를 정의 한다고 가정 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.NullDemoPage"
             Title="x:Null Demo">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="48" />
                <Setter Property="FontFamily">
                    <Setter.Value>
                        <OnPlatform x:TypeArguments="x:String">
                            <On Platform="iOS" Value="Times New Roman" />
                            <On Platform="Android" Value="serif" />
                            <On Platform="UWP" Value="Times New Roman" />
                        </OnPlatform>
                    </Setter.Value>
                </Setter>
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <ContentPage.Content>
        <StackLayout Padding="10, 0">
            <Label Text="Text 1" />
            <Label Text="Text 2" />

            <Label Text="Text 3"
                   FontFamily="{x:Null}" />

            <Label Text="Text 4" />
            <Label Text="Text 5" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>   
```

그런 다음 요소 중 하나에 대해를 `Label` 제외 하 고 모든 속성 설정을 암시적으로 지정할 수 있습니다 .이 경우 기본값을 사용할 수 있습니다 `Style` `FontFamily` . 이 목적을 위해 다른을 정의할 수 `Style` 있지만 `FontFamily` `Label` , 가운데에 설명 된 대로 단순히 특정의 속성을로 설정 하는 것이 더 간단 합니다 `x:Null` `Label` .

실행 중인 프로그램은 다음과 같습니다.

[![x:Null 데모](consuming-images/nulldemo-small.png "x:Null 데모")](consuming-images/nulldemo-large.png#lightbox "x:Null 데모")

4 개 요소에는 `Label` 세리프 글꼴이 있지만 가운데에는 `Label` 기본 sans-serif 글꼴이 있습니다.

<a name="onplatform" />

## <a name="onplatform-markup-extension"></a>OnPlatform 태그 확장

`OnPlatform` 태그 확장을 사용하면 플랫폼별 기준으로 UI 모양을 사용자 지정할 수 있습니다. 이 클래스는 및 클래스와 같은 기능을 제공 [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) [`On`](xref:Xamarin.Forms.On) 하지만 좀 더 간결 하 게 표현 합니다.

`OnPlatform`태그 확장은 다음 속성을 정의 하는 클래스에서 지원 됩니다 [`OnPlatformExtension`](xref:Xamarin.Forms.Xaml.OnPlatformExtension) .

- `Default`형식으로 `object` , 플랫폼을 나타내는 속성에 적용 되는 기본값으로 설정 됩니다.
- `Android``object`Android에 적용 되는 값으로 설정 하는 형식입니다.
- `GTK`유형으로 `object` , GTK 플랫폼에 적용 되는 값으로 설정 합니다.
- `iOS``object`iOS에 적용 되는 값으로 설정 하는 형식의입니다.
- `macOS`사용자가 `object` macOS에 적용 되는 값으로 설정 하는 형식입니다.
- `Tizen``object`Tizen 플랫폼에 적용 되는 값으로 설정 하는 형식의입니다.
- `UWP``object`유니버설 Windows 플랫폼에 적용할 값으로 설정 하는 형식의입니다.
- `WPF``object`Windows Presentation Foundation 플랫폼에 적용 되는 값으로 설정 하는 형식입니다.
- `Converter``IValueConverter`구현으로 설정할 수 있는 형식의입니다 `IValueConverter` .
- `ConverterParameter`형식으로 `object` , 구현에 전달할 값으로 설정할 수 있습니다 `IValueConverter` .

> [!NOTE]
> XAML 파서는 [`OnPlatformExtension`](xref:Xamarin.Forms.Xaml.OnPlatformExtension) 클래스를 약식으로 지정할 수 있습니다 `OnPlatform` .

`Default`속성은의 content 속성입니다 `OnPlatformExtension` . 따라서 중괄호로 표시 되는 XAML 태그 식의 경우 `Default=` 첫 번째 인수인 경우 식의 일부를 제거할 수 있습니다. `Default`속성이 설정 되지 않은 경우 [`BindableProperty.DefaultValue`](xref:Xamarin.Forms.BindableProperty.DefaultValue) 태그 확장이를 대상으로 하는 경우 속성 값으로 기본 설정 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) .

> [!IMPORTANT]
> XAML 파서는 태그 확장을 사용 하는 속성에 올바른 형식의 값이 제공 될 것으로 예상 합니다 `OnPlatform` . 형식 변환이 필요한 경우 `OnPlatform` 태그 확장은에서 제공 하는 기본 변환기를 사용 하 여이를 수행 하려고 시도 합니다 Xamarin.Forms . 그러나 기본 변환기에서 수행할 수 없는 일부 형식 변환이 있습니다. 이러한 경우에는 `Converter` 속성을 구현으로 설정 해야 합니다 `IValueConverter` .

**Onplatform Demo** 페이지는 태그 확장을 사용 하는 방법을 보여 줍니다 `OnPlatform` .

```xaml
<BoxView Color="{OnPlatform Yellow, iOS=Red, Android=Green, UWP=Blue}"
         WidthRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"  
         HeightRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"
         HorizontalOptions="Center" />
```

이 예제에서 세 개의 `OnPlatform` 식은 모두 약식 버전의 `OnPlatformExtension` 클래스 이름을 사용 합니다. 세 가지 `OnPlatform` 태그 확장은 [`Color`](xref:Xamarin.Forms.BoxView.Color) 의, [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 및 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 속성을 [`BoxView`](xref:Xamarin.Forms.BoxView) iOS, Android 및 UWP의 다른 값으로 설정 합니다. 태그 확장은 또한 지정 되지 않은 플랫폼에서 이러한 속성에 대 한 기본값을 제공 하는 한편 `Default=` 식의 일부를 제거 합니다. 설정 된 태그 확장 속성은 쉼표로 구분 됩니다.

실행 중인 프로그램은 다음과 같습니다.

[![OnPlatform Demo](consuming-images/onplatformdemo-small.png "OnPlatform Demo")](consuming-images/onplatformdemo-large.png#lightbox "OnPlatform Demo")

<a name="onidiom" />

## <a name="onidiom-markup-extension"></a>OnIdiom 태그 확장

`OnIdiom`태그 확장을 사용 하면 응용 프로그램이 실행 되는 장치를 기반으로 UI 모양을 사용자 지정할 수 있습니다. [`OnIdiomExtension`](xref:Xamarin.Forms.Xaml.OnIdiomExtension)다음 속성을 정의 하는 클래스에서 지원 됩니다.

- `Default`형식으로 `object` , 장치 관용구을 나타내는 속성에 적용 되는 기본값으로 설정 됩니다.
- `Phone``object`휴대폰에 적용 되는 값으로 설정 하는 형식의입니다.
- `Tablet``object`태블릿에 적용할 값으로 설정 하는 형식의입니다.
- `Desktop`형식으로 `object` , 데스크톱 플랫폼에 적용 되는 값으로 설정 합니다.
- `TV``object`TV 플랫폼에 적용 되는 값으로 설정 하는 형식입니다.
- `Watch`형식으로 `object` , 조사식 플랫폼에 적용 되는 값으로 설정 합니다.
- `Converter``IValueConverter`구현으로 설정할 수 있는 형식의입니다 `IValueConverter` .
- `ConverterParameter`형식으로 `object` , 구현에 전달할 값으로 설정할 수 있습니다 `IValueConverter` .

> [!NOTE]
> XAML 파서는 [`OnIdiomExtension`](xref:Xamarin.Forms.Xaml.OnIdiomExtension) 클래스를 약식으로 지정할 수 있습니다 `OnIdiom` .

`Default`속성은의 content 속성입니다 `OnIdiomExtension` . 따라서 중괄호로 표시 되는 XAML 태그 식의 경우 `Default=` 첫 번째 인수인 경우 식의 일부를 제거할 수 있습니다.

> [!IMPORTANT]
> XAML 파서는 태그 확장을 사용 하는 속성에 올바른 형식의 값이 제공 될 것으로 예상 합니다 `OnIdiom` . 형식 변환이 필요한 경우 `OnIdiom` 태그 확장은에서 제공 하는 기본 변환기를 사용 하 여이를 수행 하려고 시도 합니다 Xamarin.Forms . 그러나 기본 변환기에서 수행할 수 없는 일부 형식 변환이 있습니다. 이러한 경우에는 `Converter` 속성을 구현으로 설정 해야 합니다 `IValueConverter` .

**Onidiom Demo** 페이지에서는 태그 확장을 사용 하는 방법을 보여 줍니다 `OnIdiom` .

```xaml
<BoxView Color="{OnIdiom Yellow, Phone=Red, Tablet=Green, Desktop=Blue}"
         WidthRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HeightRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HorizontalOptions="Center" />
```

이 예제에서 세 개의 `OnIdiom` 식은 모두 약식 버전의 `OnIdiomExtension` 클래스 이름을 사용 합니다. 세 가지 `OnIdiom` 태그 확장은 [`Color`](xref:Xamarin.Forms.BoxView.Color) 의, [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 및 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 속성을 [`BoxView`](xref:Xamarin.Forms.BoxView) phone, 태블릿 및 데스크톱 관용구의 다른 값으로 설정 합니다. 태그 확장은 또한 식의 일부를 제거 하는 동시에 지정 되지 않은 관용구에서 이러한 속성에 대 한 기본값을 제공 `Default=` 합니다. 설정 된 태그 확장 속성은 쉼표로 구분 됩니다.

실행 중인 프로그램은 다음과 같습니다.

[![Ononedemo](consuming-images/onidiomdemo-small.png "Ononedemo")](consuming-images/onidiomdemo-large.png#lightbox "Ononedemo")

## <a name="datatemplate-markup-extension"></a>DataTemplate 태그 확장

`DataTemplate`태그 확장을 사용 하 여 형식을으로 변환할 수 있습니다 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . 로 `DataTemplateExtension` `TypeName` `string` 변환할 형식의 이름으로 설정 된 형식의 속성을 정의 하는 클래스에서 지원 됩니다 `DataTemplate` . `TypeName`속성은의 content 속성입니다 `DataTemplateExtension` . 따라서 중괄호가 사용된 XAML 태그 식의 경우 식의 `TypeName=` 부분을 제거할 수 있습니다.

> [!NOTE]
> XAML 파서를 사용하면 `DataTemplateExtension` 클래스를 `DataTemplate`로 축약할 수 있습니다.

이 태그 확장은 다음 예제와 같이 셸 응용 프로그램에서 일반적으로 사용 됩니다.

```xaml
<ShellContent Title="Monkeys"
              Icon="monkey.png"
              ContentTemplate="{DataTemplate views:MonkeysPage}" />
```

이 예제에서는 `MonkeysPage` [`ContentPage`](xref:Xamarin.Forms.ContentPage) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 속성의 값으로 설정 된에서로 변환 됩니다 `ShellContent.ContentTemplate` . 이렇게 하면 `MonkeysPage` 응용 프로그램 시작 시가 아니라 페이지 탐색이 발생 하는 경우에만이 생성 됩니다.

셸 응용 프로그램에 대 한 자세한 내용은 [ Xamarin.Forms shell](~/xamarin-forms/app-fundamentals/shell/index.md)을 참조 하세요.

## <a name="fontimage-markup-extension"></a>FontImage 태그 확장

`FontImage`태그 확장을 사용 하면를 표시할 수 있는 모든 보기에 글꼴 아이콘을 표시할 수 있습니다 `ImageSource` . 클래스와 동일한 기능을 제공 `FontImageSource` 하지만 더 간결 하 게 표현 합니다.

`FontImage` 태그 확장은 다음 속성을 정의하는 `FontImageExtension` 클래스에서 지원됩니다.

- `FontFamily``string`글꼴 아이콘이 속한 글꼴 패밀리 형식의입니다.
- `Glyph``string`글꼴 아이콘의 유니코드 문자 값인 형식의입니다.
- `Color`형식의 [`Color`](xref:Xamarin.Forms.Color) 글꼴 아이콘을 표시할 때 사용 되는 색입니다.
- `Size`형식 `double` , 렌더링 된 글꼴 아이콘의 크기 (장치 독립적 단위)입니다. 기본값은 30입니다. 또한이 속성은 명명 된 글꼴 크기로 설정할 수 있습니다.

> [!NOTE]
> XAML 파서를 사용하면 `FontImageExtension` 클래스를 `FontImage`로 축약할 수 있습니다.

`Glyph`속성은의 content 속성입니다 `FontImageExtension` . 따라서 중괄호로 표시 되는 XAML 태그 식의 경우 `Glyph=` 첫 번째 인수인 경우 식의 일부를 제거할 수 있습니다.

**FontImage Demo** 페이지에서는 태그 확장을 사용 하는 방법을 보여 줍니다 `FontImage` .

```xaml
<Image BackgroundColor="#D1D1D1"
       Source="{FontImage &#xf30c;, FontFamily={OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=44}" />
```

이 예제에서는 클래스 이름의 축약 된 버전을 `FontImageExtension` 사용 하 여의 아이콘 글꼴 패밀리에서 XBox 아이콘을 표시 [`Image`](xref:Xamarin.Forms.Image) 합니다. 또한이 식은 태그 확장을 사용 하 여 `OnPlatform` `FontFamily` IOS 및 Android에서 다른 속성 값을 지정 합니다. 또한 `Glyph=` 식의 일부는 제거 되며 설정 된 태그 확장 속성은 쉼표로 구분 됩니다. 아이콘의 유니코드 문자는 `\uf30c` 이지만 XAML에서 이스케이프 되어야 하므로가 됩니다 `&#xf30c;` .

실행 중인 프로그램은 다음과 같습니다.

[![FontImage 태그 확장의 스크린샷](consuming-images/fontimagedemo.png "FontImage 데모")](consuming-images/fontimagedemo-large.png#lightbox "FontImage 데모")

개체에서 글꼴 아이콘 데이터를 지정 하 여 글꼴 아이콘을 표시 하는 방법에 대 한 자세한 내용은 `FontImageSource` [글꼴 아이콘 표시](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons)를 참조 하세요.

## <a name="onapptheme-markup-extension"></a>OnAppTheme 태그 확장

`OnAppTheme`태그 확장을 사용 하면 현재 시스템 테마에 따라 이미지나 색과 같은 소비할 리소스를 지정할 수 있습니다. 클래스와 동일한 기능을 제공 `OnAppTheme<T>` 하지만 더 간결 하 게 표현 합니다.

> [!IMPORTANT]
> `OnAppTheme`태그 확장에는 최소 운영 체제 요구 사항이 있습니다. 자세한 내용은 [ Xamarin.Forms 응용 프로그램에서 시스템 테마 변경 내용에 응답](~/xamarin-forms/user-interface/theming/system-theme-changes.md)을 참조 하세요.

`OnAppTheme` 태그 확장은 다음 속성을 정의하는 `OnAppThemeExtension` 클래스에서 지원됩니다.

- `Default``object`기본적으로 사용 되는 리소스로 설정 하는 형식의입니다.
- `Light``object`장치에서 밝은 테마를 사용할 때 사용할 리소스로 설정 하는 형식의입니다.
- `Dark``object`장치에서 짙은 테마를 사용할 때 사용할 리소스로 설정 하는 형식의입니다.
- `Value``object`태그 확장에서 현재 사용 중인 리소스를 반환 하는 형식의입니다.
- `Converter``IValueConverter`구현으로 설정할 수 있는 형식의입니다 `IValueConverter` .
- `ConverterParameter`형식으로 `object` , 구현에 전달할 값으로 설정할 수 있습니다 `IValueConverter` .

> [!NOTE]
> XAML 파서를 사용하면 `OnAppThemeExtension` 클래스를 `OnAppTheme`로 축약할 수 있습니다.

`Default`속성은의 content 속성입니다 `OnAppThemeExtension` . 따라서 중괄호로 표시 되는 XAML 태그 식의 경우 `Default=` 첫 번째 인수인 경우 식의 일부를 제거할 수 있습니다.

**Onapptheme 데모** 페이지에는 태그 확장을 사용 하는 방법이 나와 `OnAppTheme` 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.OnAppThemeDemoPage"
             Title="OnAppTheme Demo">
    <ContentPage.Resources>

        <Style x:Key="labelStyle"
               TargetType="Label">
            <Setter Property="TextColor"
                    Value="{OnAppTheme Black, Light=Blue, Dark=Teal}" />
        </Style>

    </ContentPage.Resources>
    <StackLayout Margin="20">
        <Label Text="This text is green in light mode, and red in dark mode."
               TextColor="{OnAppTheme Light=Green, Dark=Red}" />
        <Label Text="This text is black by default, blue in light mode, and teal in dark mode."
               Style="{DynamicResource labelStyle}" />
    </StackLayout>
</ContentPage>
```

이 예제에서는 [`Label`](xref:Xamarin.Forms.Label) 장치에서 밝은 테마를 사용 하 고 장치에서 짙은 테마를 사용 하는 경우 빨간색으로 설정 된 첫 번째의 텍스트 색을 녹색으로 설정 합니다. 두 번째에 `Label` [`TextColor`](xref:Xamarin.Forms.Label.TextColor) 는를 통해 설정 된 속성이 있습니다 [`Style`](xref:Xamarin.Forms.Style) . 이는 `Style` 기본적으로의 텍스트 색을 검정으로 설정 하 `Label` 고, 장치에서 밝은 테마를 사용 하는 경우 blue로, 장치에서 짙은 테마를 사용 하는 경우 청록으로 설정 합니다.

> [!NOTE]
> 태그 확장을 [`Style`](xref:Xamarin.Forms.Style) 사용 하는는 `OnAppTheme` 태그 확장을 사용 하는 컨트롤에 적용 해야 `DynamicResource` 시스템 테마가 변경 될 때 앱의 UI가 업데이트 됩니다.

실행 중인 프로그램은 다음과 같습니다.

![OnAppTheme 데모](consuming-images/onappthemedemo.png "OnAppTheme 데모")

## <a name="define-markup-extensions"></a>태그 확장 정의

에서 사용할 수 없는 XAML 태그 확장이 필요한 경우 Xamarin.Forms [직접 만들](creating.md)수 있습니다.

## <a name="related-links"></a>관련 링크

- [태그 확장 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [XAML 태그 확장 책의 장 Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)
- [동적 스타일](~/xamarin-forms/user-interface/styles/dynamic.md)
- [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin.Forms셸로](~/xamarin-forms/app-fundamentals/shell/index.md)
- [응용 프로그램의 시스템 테마 변경 내용에 응답 Xamarin.Forms](~/xamarin-forms/user-interface/theming/system-theme-changes.md)
