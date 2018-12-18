---
title: XAML 태그 확장 사용
description: 이 문서에서는 다양 한 원본에서 요소 특성을 설정할 수 있도록 함으로써 XAML의 성능과 유연성을 향상시키는 Xamarin.Forms XAML 태그 확장 사용 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/01/2018
ms.openlocfilehash: 53c5f17672cc46ef097e979154a8911f8cdaef63
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53054128"
---
# <a name="consuming-xaml-markup-extensions"></a>XAML 태그 확장 사용

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)

XAML 태그 확장은 다양 한 원본에서 요소 특성을 설정할 수 있도록 함으로써 XAML의 성능과 유연성 향상을 도와줍니다. 몇 가지 XAML 태그 확장은 XAML 2009 사양의 일부입니다. XAML 태그 확장은 일반적으로 `x` 네임 스페이스 접두사를 사용하여 XAML 파일에 표시되고, 흔히 해당 접두사로 참조됩니다. 이 문서에서는 다음 태그 확장에 대해 설명합니다.

- [`x:Static`](#static) – 정적 속성, 필드 또는 열거형 멤버를 참조합니다.
- [`x:Reference`](#reference) – 페이지의 명명 된 요소를 참조합니다.
- [`x:Type`](#type) – `System.Type` 개체로 특성을 설정합니다.
- [`x:Array`](#array) – 특정 유형의 개체 배열을 생성합니다.
- [`x:Null`](#null) – `null` 값으로 특성을 설정합니다.
- [`OnPlatform`](#onplatform) – 플랫폼별 기준에서 UI 모양을 사용자 지정 합니다.
- [`OnIdiom`](#onidiom) – 응용 프로그램이 실행 중인 장치의 관용구를 기반으로 UI 모양을 사용자 지정 합니다.

추가적인 XAML 태그 확장은 역사적으로 다른 XAML 구현에서 지원되었으며, Xamarin.Forms에서도 지원됩니다. 해당 내용은 다음과 같이 다른 글에서 더 자세히 설명합니다.

- `StaticResource` &ndash; [ **리소스 사전**](~/xamarin-forms/xaml/resource-dictionaries.md) 문서에 설명 된 것처럼 리소스 사전에서 개체를 참조 합니다.
- `DynamicResource` &ndash; [ **동적 스타일**](~/xamarin-forms/user-interface/styles/dynamic.md) 문서에 설명 된 것처럼 리소스 사전에 있는 개체 변경에 응답합니다.
- `Binding` &ndash; [ **데이터 바인딩**](~/xamarin-forms/app-fundamentals/data-binding/index.md) 문서에 설명 된 것처럼 두 개체의 속성 간의 연결을 설정합니다.
- `TemplateBinding` &ndash; [ **컨트롤 템플릿에서 바인딩**](/guides/xamarin-forms/application-fundamentals/templates/control-templates/template-binding/) 문서에 설명 된 것처럼 컨트롤 템플릿에서 데이터 바인딩을 수행합니다.

[ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout) 레이아웃은 사용자 지정 태그 확장 [ `ConstraintExpression` ](xref:Xamarin.Forms.ConstraintExpression)을 사용합니다. 해당 태그 확장은 [ **RelativeLayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md) 문서에서 설명합니다.

<a name="static" />

## <a name="xstatic-markup-extension"></a>x:Static 태그 확장

`x:Static` 태그 확장은 [ `StaticExtension` ](xref:Xamarin.Forms.Xaml.StaticExtension) 클래스에서 지원됩니다. 클래스에는 공용 상수, 정적 속성, 정적 필드 또는 열거형 멤버의 이름으로 설정되는 `string` 유형의 [ `Member` ](xref:Xamarin.Forms.Xaml.StaticExtension.Member)라는 단일 속성이 있습니다.

`x:Static`을 사용하는 일반적인 방법 중 하나는 [ **MarkupExtensions** ](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/) 프로그램에 있는 다음과 같은 작은 `AppConstants` 클래스처럼 몇 가지 상수 또는 정적 변수를 가진 클래스를 먼저 정의하는 것입니다.

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

**x:Static Demo** 페이지는 `x:Static` 태그 확장을 사용하는 몇 가지 방법을 보여줍니다. 가장 장황한 방법은 다음과 같이 `Label.FontSize` 속성 요소 태그 사이에 `StaticExtension` 클래스를 인스턴스화하는 것입니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
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

XAML 파서는 또한 다음과 같이 `StaticExtension` 클래스를 `x:Static`으로 축약하도록 허용합니다.

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

이것은 더 단순화 할 수 있지만, 변경에 몇 가지 새로운 구문을 도입합니다. `StaticExtension` 클래스와 멤버 설정을 중괄호로 묶어서 구성합니다. 결과 표현식은 다음과 같이 직접 `FontSize` 특성으로 설정됩니다.

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

중괄호 안의 따옴표가 *없다*는 것에 주의하십시오. 합니다 `StaticExtension`의 `Member` 속성은 더 이상 XML 특성이 아닙니다. 대신 태그 확장에 대한 표현식의 일부입니다.

개체 요소로 사용할 때 `x:StaticExtension`을 `x:Static`으로 축약할 수 있는 것처럼, 다음과 같이 중괄호 내의 표현식에서 이를 줄여 쓸 수도 있습니다.

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

`StaticExtension` 클래스는 `Member` 속성을 참조하는 `ContentProperty` 특성을 가지고 있습니다. 이 특성은 해당 속성을 클래스의 기본 콘텐츠 속성으로 표시합니다. 중괄호를 사용하여 표시된 XAML 태그 확장의 경우 다음과 같이 표현식의 `Member=` 부분을 제거할 수 있습니다.

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

이것은 `x:Static` 태그 확장의 가장 일반적인 형태입니다.

**Static Demo** 페이지에는 두 가지 다른 예가 포함되어 있습니다. XAML 파일의 루트 태그에는 다음과 같이 .NET `System` 네임 스페이스에 대한 XML 네임 스페이스 선언이 포함되어 있습니다.

```xaml
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

이로 인해 `Label`의 글꼴 크기를 정적 필드 `Math.PI`로 설정 가능합니다. 해당 결과는 텍스트가 다소 작으며, `Scale` 속성은 `Math.E`로 설정됩니다.

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

마지막 예는 `Device.RuntimePlatform` 값을 표시합니다. `Environment.NewLine` 정적 속성은 다음과 같이 두 개의 `Span` 개체 사이에 개행 문자를 삽입하는 데 사용됩니다.

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

샘플이 실행은 다음과 같습니다.

[![x:Static Demo](consuming-images/staticdemo-small.png "x:Static Demo")](consuming-images/staticdemo-large.png#lightbox "x:Static Demo")

<a name="reference" />

## <a name="xreference-markup-extension"></a>x:Reference 태그 확장

`x:Reference` 태그 확장은 [ `ReferenceExtension` ](xref:Xamarin.Forms.Xaml.ReferenceExtension) 클래스에서 지원됩니다. 클래스에는 `x:Name`으로 이름이 붙여진 페이지 상의 요소 이름을 설정하는 `string` 유형의 [ `Name` ](xref:Xamarin.Forms.Xaml.ReferenceExtension.Name)이라는 단일 속성을 가지고 있습니다. `Name` 속성은 `ReferenceExtension`의 콘텐츠 속성이므로 `x:Reference`가 중괄호 안에 나타날 때 `Name=`은 필요 없습니다.

`x:Reference` 태그 확장은 [ **데이터 바인딩**](~/xamarin-forms/app-fundamentals/data-binding/index.md) 글에서 자세히 설명하는 데이터 바인딩과 함께 독점적으로 사용됩니다.

다음의 **x:Reference Demo** 페이지는 데이터 바인딩을 사용하는 `x:Reference`의 두 가지 사용법을 보여줍니다. 첫 번째는 `Binding` 개체의 `Source` 속성을 설정하는 데 사용되고, 두 번째는 두 개의 데이터 바인딩을 위한 `BindingContext` 속성을 설정하는 데 사용되었습니다.

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

두 `x:Reference` 표현식은 모두 `ReferenceExtension` 클래스 이름의 축약된 버전을 사용하고 표현식의 `Name=` 부분을 제거하였습니다. 첫 번째 예에서 `x:Reference` 태그 확장은 `Binding` 태그 확장에 내장되어 있습니다. `Source`와 `StringFormat` 설정은 쉼표로 구분됩니다. 실행 중인 프로그램은 다음과 같습니다.

[![x:Reference Demo](consuming-images/referencedemo-small.png "x:Reference Demo")](consuming-images/referencedemo-large.png#lightbox "x:Reference Demo")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type 태그 확장

`x:Type` 태그 확장은 C# [ `typeof` ](/dotnet/csharp/language-reference/keywords/typeof/) 키워드와 동일한 XAML 입니다. 클래스 또는 구조체 이름으로 설정 된 `string` 유형의 [ `TypeName` ](xref:Xamarin.Forms.Xaml.TypeExtension.TypeName)이라는 하나의 속성을 정의하는 [ `TypeExtension` ](xref:Xamarin.Forms.Xaml.TypeExtension) 클래스에서 지원됩니다. `x:Type` 태그 확장은 해당 클래스 또는 구조체의 [ `System.Type` ](xref:System.Type) 개체를 반환 합니다. `TypeName`은 `TypeExtension`의 콘텐츠 속성이므로 중괄호로 `x:Type`을 표시할 때 `TypeName=`이 필요하지 않습니다.

Xamarin.Forms에는 `Type` 유형의 인수를 갖는 여러 속성이 있습니다. 예제는 `Style`의 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) 속성과 [x:Typearguments](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments) 특성은 제네릭 클래스에서 인수를 지정하는 데 사용됩니다. 그러나 XAML 파서는 자동으로 `typeof` 연산을 수행하며, 이 경우에는 `x:Type` 태그 확장이 사용되지 않습니다.

`x:Type`이 *필수* 인 한 곳은 [다음 절](#array)에서 설명하는 `x:Array` 태그 확장 입니다.

`x:Type` 태그 확장은 각 메뉴 항목이 특정 유형의 개체에 해당하는 메뉴를 구성할 때도 유용합니다. `Type` 개체를 각 메뉴 항목과 연결한 다음 메뉴 항목이 선택 될 때 개체를 인스턴스화 할 수 있습니다.

이것이 **Markup Extensions** 프로그램의 `MainPage`에 있는 탐색 메뉴가 작동하는 방식입니다. 다음과 같이 **MainPage.xaml** 파일에는 프로그램의 특정 페이지에 해당하는 각 `TextCell`이 있는 `TableView`가 들어 있습니다.

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

**Markup Extensions**의 열린 기본 페이지는 다음과 같습니다.

[![기본 페이지](consuming-images/mainpage-small.png "기본 페이지")](consuming-images/mainpage-large.png#lightbox "기본 페이지")

각 `CommandParameter` 속성은 다른 페이지 중 하나를 참조하는 `x:Type` 태그 확장으로 설정됩니다. `Command` 속성은 `NavigateCommand`라는 속성에 바인딩 됩니다. 해당 속성은 `MainPage` 코드 비하인드 파일에 다음과 같이 정의되어 있습니다.

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

`NavigateCommand` 속성은 `CommandParameter`의 값인 `Type` 유형의 인수를 사용하여 execute 명령이 구현하는 `Command` 개체입니다. 메서드는 페이지 인스턴스화를 위해 `Activator.CreateInstance`를 사용한 다음 해당 페이지로 이동합니다. 생성자는 `Command`의 `Binding`이 동작 가능하도록 페이지의 `BindingContext`를 자신으로 설정하여 완료됩니다. 해당 종류의 코드에 대한 더 자세한 내용은 [ **데이터 바인딩** ](~/xamarin-forms/app-fundamentals/data-binding/index.md) 문서와 특히 [ **명령** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) 문서를 참조하십시오.

**x:Type Demo** 페이지는 유사한 기술을 사용하여 Xamarin.Forms 요소를 인스턴스화하고 해당 인스턴스를 `StackLayout`에 추가합니다. 다음 XAML 파일은 먼저 `Command` 속성이 `Binding`으로 설정되고 `CommandParameter` 속성이 세 개의 Xamarin.Forms 뷰 유형으로 설정된 `Button` 요소 세 개로 구성됩니다.

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

코드 비하인드 파일은 다음과 같이 `CreateCommand` 속성을 정의하고 초기화 합니다.

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

`Button`을 누를 때 실행되는 메서드는 인수의 새로운 인스턴스를 만들고, 버튼의 `VerticalOptions` 속성을 설정하고 `StackLayout`에 추가합니다. 세 개의 `Button` 요소는 동적으로 생성된 된 뷰를 사용하여 다음과 같이 페이지를 공유합니다.

[![x:Type Demo](consuming-images/typedemo-small.png "x:Type Demo")](consuming-images/typedemo-large.png#lightbox "x:Type Demo")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array 태그 확장

`x:Array` 태그 확장은 태그에서 배열을 정의할 수 있도록 해줍니다. 이는 다음의 두 속성을 정의하는 [ `ArrayExtension` ](xref:Xamarin.Forms.Xaml.ArrayExtension) 클래스에서 지원됩니다.

- 배열에 있는 요소의 유형을 가리키는 `Type` 유형의 `Type`.
- 항목 자체의 컬렉션인 `IList` 유형의 `Items`. 이는 `ArrayExtension`의 컨텐츠 속성입니다.

`x:Array` 태그 확장 자체는 절대 중괄호 안에 나타나지 않습니다. 대신, `x:Array` 시작과 끝 태그는 항목의 목록을 구분합니다. `Type` 속성을 `x:Type` 태그 확장으로 설정하십시오.

다음 **x:Array Demo** 페이지는 `ItemsSource` 속성을 배열로 설정하여 `ListView`에 항목을 추가하는 `x:Array`를 사용하는 방법을 보여줍니다.

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

`ViewCell`은 다음과 같이 각 색상 항목에 대한 단순 `BoxView`를 생성합니다.

[![x:Array Demo](consuming-images/arraydemo-small.png "x:Array Demo")](consuming-images/arraydemo-large.png#lightbox "x:Array Demo")

해당 배열에 개별적인 `Color` 항목을 지정하는 여러가지 방법이 있습니다. 다음과 같이 `x:Static` 태그 확장을 사용할 수 있습니다.

```xaml
<x:Static Member="Color.Blue" />
```

또는 다음과 같이 리소스 사전에서 색상을 검색하는 `StaticResource`을 사용할 수 있습니다.

```xaml
<StaticResource Key="myColor" />
```

이 문서의 끝 부분으로 가면서 다음과 같이 새로운 색상 값을 만드는 사용자 지정 XAML 태그 확장 또한 보게 될 것입니다.

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

문자열이나 숫자와 같은 일반적인 유형의 배열을 정의할 때는 [ **생성자 인수 전달** ](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) 문서에 나열된 태그를 사용하여 값을 구분하십시오.

<a name="null" />

## <a name="xnull-markup-extension"></a>x:Null 태그 확장

`x:Null` 태그 확장은 [ `NullExtension` ](xref:Xamarin.Forms.Xaml.NullExtension) 클래스에서 지원됩니다. 속성이 없고 단순하게 C# [ `null` ](/dotnet/csharp/language-reference/keywords/null/) 키워드와 동일한 XAML 입니다.

`x:Null` 태그 확장은 거의 필요하지 않으며 드물게 사용되지만, 필요성을 느낀다면 그것이 존재한다는 것에 기뻐할 것입니다.

**x:Null Demo** 페이지는 `x:Null`이 편리하게 사용되는 시나리오를 보여줍니다. 다음과 같이 `FontFamily` 속성을 플랫폼 의존적인 글꼴 이름으로 설정하는 `Setter`를 포함하는 `Label`에 대한 암시적 `Style`을 정의한다고 가정해 보겠습니다.

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

그러면 `Label` 요소 중 하나에 `FontFamily`를 제외하고 모든 암시적 `Style`의 속성 설정을 기본 값이 되도록 하고 싶어 한다는 것을 알게 됩니다. 해당 목적을 위해 또 하나의 `Style`을 정의할 수 있지만, 좀 더 간단한 접근법은 가운데 `Label`에서 보이는 것처럼 특정 `Label`의 `FontFamily` 속성을 `x:Null`로 설정하는 것입니다.

실행 중인 프로그램은 다음과 같습니다.

[![x:Null Demo](consuming-images/nulldemo-small.png "x:Null Demo")](consuming-images/nulldemo-large.png#lightbox "x:Null Demo")

`Label` 요소 중 네 개는 serif 글꼴을 갖고 있지만, 가운데 `Label`은 기본 sans-serif 글꼴을 갖습니다.

<a name="onplatform" />

## <a name="onplatform-markup-extension"></a>OnPlatform 태그 확장

`OnPlatform` 태그 확장은 플랫폼별 기준 UI 모양을 사용자 지정할 수 있게 해줍니다. 해당 태그 확장은 [ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) 및 [ `On` ](xref:Xamarin.Forms.On) 클래스와 동일한 기능을 제공하지만 보다 간결한 표현을 제공합니다.

`OnPlatform` 태그 확장은 다음 속성을 정의하는 [ `OnPlatformExtension` ](xref:Xamarin.Forms.Xaml.OnPlatformExtension) 클래스에서 지원됩니다.

- 플랫폼을 나타내는 속성에 적용할 기본 값으로 설정하는 `object` 유형의 `Default`.
- Android에 적용할 값으로 설정하는 `object` 유형의 `Android`.
- GTK 플랫폼에 적용할 값으로 설정하는 `object` 유형의 `GTK`.
- iOS에서 적용할 값으로 설정하는 `object` 유형의 `iOS`.
- macOS에서 적용할 값으로 설정하는 `object` 유형의 `macOS`.
- Tizen 플랫폼에 적용할 값으로 설정하는 `object` 유형의 `Tizen`. 
- UWP(Universal Windows Platform) 플랫폼에 적용할 값으로 설정하는 `object` 유형의 `UWP`.
- WPF(Windows Presentation Foundation) 플랫폼에 적용할 값으로 설정하는 `object` 유형의 `WPF`.
- `IValueConverter` 구현에 설정하는 `IValueConverter` 유형의 `Converter`.
- `IValueConverter` 구현에 전달할 값으로 설정하는 `object` 유형의 `ConverterParameter`.

> [!NOTE]
> XAML 파서는 [ `OnPlatformExtension` ](xref:Xamarin.Forms.Xaml.OnPlatformExtension) 클래스를 `OnPlatform`으로 축약을 허용합니다.

`Default` 속성은 `OnPlatformExtension`의 콘텐츠 속성입니다. 따라서 XAML 태그 표현식은 중괄호를 사용하여 표현되며, 첫 번째 인수로 제공된 표현식의 `Default=` 부분은 제거할 수 있습니다.

> [!IMPORTANT]
> XAML 파서는 올바른 유형의 값이 `OnPlatform` 태그 확장을 사용하는 속성에 제공될 것으로 예상 합니다. 유형 변환이 필요한 경우, `OnPlatform` 태그 확장은 Xamarin.Forms에서 제공하는 기본 변환기를 사용하여 변환을 수행하려고 합니다. 그러나 기본 변환기로는 수행할 수 없는 일부 유형 변환이 있으며 이러한 경우 `Converter` 속성은 `IValueConverter` 구현으로 설정되어야 합니다.

**OnPlatform Demo** 페이지는 다음과 같이 `OnPlatform` 태그 확장을 사용하는 방법을 보여줍니다.

```xaml
<BoxView Color="{OnPlatform Yellow, iOS=Red, Android=Green, UWP=Blue}"
         WidthRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"  
         HeightRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"
         HorizontalOptions="Center" />
```

이 예제에서 세 개의 `OnPlatform` 표현식 모두가 `OnPlatformExtension` 클래스 이름의 축약된 버전을 사용합니다. 세 가지 `OnPlatform` 태그 확장은 [ `BoxView` ](xref:Xamarin.Forms.BoxView)의 [ `Color` ](xref:Xamarin.Forms.BoxView.Color), [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) 및 [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) 속성을 iOS, Android 및 UWP에서 달라지는 값으로 설정합니다. 또한 태그 확장은 지정 되지 않은 플랫폼에서 해당 속성들에 대한 기본값을 제공하지만, 표현식의 `Default=` 부분은 제거하였습니다. 설정되는 태그 확장 속성은 쉼표로 구분 합니다.

실행 중인 프로그램은 다음과 같습니다.

[![OnPlatform Demo](consuming-images/onplatformdemo-small.png "OnPlatform Demo")](consuming-images/onplatformdemo-large.png#lightbox "OnPlatform Demo")

<a name="onidiom" />

## <a name="onidiom-markup-extension"></a>OnIdiom 태그 확장

`OnIdiom` 태그 확장을 사용하면 응용 프로그램이 실행 중인 장치의 관용구를 기반으로 UI 모양을 사용자 지정할 수 있습니다. 다음 속성을 정의하는 [ `OnIdiomExtension` ](xref:Xamarin.Forms.Xaml.OnIdiomExtension) 클래스에서 지원됩니다.

- 장치 관용구를 나타내는 속성에 적용할 기본 값으로 설정하는 `object` 유형의 `Default`.
- 폰에 적용할 값으로 설정하는 `object` 유형의 `Phone`.
- 태블릿에 적용할 값으로 설정하는 `object` 유형의 `Tablet`.
- 데스크톱 플랫폼에 적용할 값으로 설정하는 `object` 유형의 `Desktop`.
- TV 플랫폼에 적용할 값으로 설정하는 `object` 유형의 `TV`.
- 조사식 플랫폼에 적용할 값으로 설정하는 `object` 유형의 `Watch`.
- `IValueConverter` 구현에 설정하는 `IValueConverter` 유형의 `Converter`.
- `IValueConverter` 구현에 전달할 값으로 설정하는 `object` 유형의 `ConverterParameter`.

> [!NOTE]
> XAML 파서는 [ `OnIdiomExtension` ](xref:Xamarin.Forms.Xaml.OnIdiomExtension) 클래스를 `OnIdiom`으로 축약을 허용합니다.

`Default` 속성은 `OnIdiomExtension`의 콘텐츠 속성 입니다. 따라서 중괄호로 표시되는 XAML 태그 표현식의 경우, 첫 번째 인수로 제공 된 표현식의 `Default=` 부분을 제거 할 수 있습니다.

> [!IMPORTANT]
> XAML 파서는 올바른 유형의 값이 `OnIdiom` 태그 확장을 사용하는 속성에 제공될 것으로 예상합니다. 유형 변환이 필요한 경우 `OnIdiom` 태그 확장은 Xamarin.Forms에서 제공하는 기본 변환기를 사용하여 변환을 수행하려고 합니다. 그러나 기본 변환기에서 수행할 수 없는 일부 유형 변환이 있으며 이러한 경우 `Converter` 속성은 `IValueConverter` 구현으로 설정되어야 합니다.

**OnIdiom Demo** 페이지는 다음과 같이 `OnIdiom` 태그 확장을 사용하는 방법을 보여 줍니다.

```xaml
<BoxView Color="{OnIdiom Yellow, Phone=Red, Tablet=Green, Desktop=Blue}"
         WidthRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HeightRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HorizontalOptions="Center" />
```

이 예제에서 세 개의 `OnIdiom` 표현식은 모두 `OnIdiomExtension` 클래스 이름의 축약된 버전을 사용합니다. 세 개의 `OnIdiom` 태그 확장은 [ `BoxView` ](xref:Xamarin.Forms.BoxView)의 [ `Color` ](xref:Xamarin.Forms.BoxView.Color), [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) 및 [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) 속성을 휴대폰, 태블릿 및 데스크톱 관용구에서 달라지는 값으로 설정합니다. 또한 태그 확장은 지정되지 않은 관용구의 해당 속성에 대한 기본값을 제공하지만, 표현식의 `Default=` 부분은 제거합니다. 설정되는 태그 확장 속성은 쉼표로 구분 합니다.

실행 중인 프로그램은 다음과 같습니다.

[![OnIdiom Demo](consuming-images/onidiomdemo-small.png "OnIdiom Demo")](consuming-images/onidiomdemo-large.png#lightbox "OnIdiom Demo")

## <a name="define-your-own-markup-extensions"></a>사용자 고유의 태그 확장 정의

Xamarin.Forms에서 사용할 수 없는 XAML 태그 확장이 필요하다면 [직접 만들기](creating.md)를 참조하십시오.

## <a name="related-links"></a>관련 링크

- [태그 확장 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Xamarin.Forms 책에서 XAML 태그 확장 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)
- [동적 스타일](~/xamarin-forms/user-interface/styles/dynamic.md)
- [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)
