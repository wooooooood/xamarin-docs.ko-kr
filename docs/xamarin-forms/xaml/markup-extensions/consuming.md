---
title: XAML 태그 확장 사용
description: 이 문서에서는 다양 한 소스에서 요소 특성을 설정할 수 있도록 하 여 Xamarin.ios XAML 태그 확장을 사용 하 여 XAML의 기능과 유연성을 향상 시키는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2019
ms.openlocfilehash: 03aaf471479a5113aade6bd3f34034afadfb538c
ms.sourcegitcommit: dad4dfcd194b63ec9e903363351b6d9e543d4888
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/18/2019
ms.locfileid: "69887897"
---
# <a name="consuming-xaml-markup-extensions"></a>XAML 태그 확장 사용

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

XAML 태그 확장은 다양 한 소스에서 요소 특성을 설정할 수 있도록 하 여 XAML의 기능과 유연성을 향상 시키는 데 도움이 됩니다. 몇 가지 XAML 태그 확장은 XAML 2009 사양의 일부입니다. 이러한 파일은 일반적인 `x` 네임 스페이스 접두사를 사용 하 여 XAML 파일에 표시 되며 일반적으로이 접두사를 사용 하 여 참조 됩니다. 이 문서에서는 다음과 같은 태그 확장에 대해 설명 합니다.

- [`x:Static`](#static) – 정적 속성, 필드 또는 열거형 멤버를 참조 합니다.
- [`x:Reference`](#reference) – 페이지에서 명명 된 요소를 참조 합니다.
- [`x:Type`](#type) – 특성을 `System.Type` 개체로 설정 합니다.
- [`x:Array`](#array) – 특정 형식의 개체 배열을 생성 합니다.
- [`x:Null`](#null) – 특성을 `null` 값으로 설정 합니다.
- [`OnPlatform`](#onplatform) – 플랫폼 별로 UI 모양을 사용자 지정 합니다.
- [`OnIdiom`](#onidiom) – 응용 프로그램이 실행 되 고 있는 장치를 기준으로 UI 모양을 사용자 지정 합니다.
- [`DataTemplate`](#datatemplate-markup-extension) -형식을 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)변환 합니다.
- [`FontImage`](#fontimage-markup-extension) -`ImageSource`를 표시할 수 있는 보기에 글꼴 아이콘을 표시 합니다.

추가 XAML 태그 확장은 이전에 다른 XAML 구현에서 지원 되었으며 Xamarin.ios 에서도 지원 됩니다. 이에 대해서는 다른 문서에 자세히 설명 되어 있습니다.

- [**리소스 사전 문서에**](~/xamarin-forms/xaml/resource-dictionaries.md)설명 된 대로 리소스 사전에서 참조 개체를 &ndash; `StaticResource` 합니다.
- `DynamicResource`는 [**동적 스타일**](~/xamarin-forms/user-interface/styles/dynamic.md)문서에 설명 된 대로 리소스 사전에 있는 개체의 변경 내용에 응답 &ndash;.
- `Binding` [**데이터 바인딩**](~/xamarin-forms/app-fundamentals/data-binding/index.md)문서에 설명 된 대로 두 개체의 속성 간에 링크를 설정 &ndash; 합니다.
- `TemplateBinding` &ndash; [**는 컨트롤 템플릿에서 바인딩**](~/xamarin-forms/app-fundamentals/templates/control-templates/template-binding.md)문서에 설명 된 대로 컨트롤 템플릿에서 데이터 바인딩을 수행 합니다.

[@No__t_1](xref:Xamarin.Forms.RelativeLayout) 레이아웃을 사용 하면 [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression)사용자 지정 태그 확장을 사용할 수 있습니다. 이 태그 확장은 [**RelativeLayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md)문서에 설명 되어 있습니다.

<a name="static" />

## <a name="xstatic-markup-extension"></a>x:Static 태그 확장

@No__t_0 태그 확장은 [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension) 클래스에서 지원 됩니다. 클래스에는 public 상수, 정적 속성, 정적 필드 또는 열거형 멤버의 이름으로 설정 하는 `string` 형식의 [`Member`](xref:Xamarin.Forms.Xaml.StaticExtension.Member) 라는 단일 속성이 있습니다.

@No__t_0를 사용 하는 일반적인 방법 중 하나는 먼저 [**MarkupExtensions**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions) 프로그램에서이 작은 `AppConstants` 클래스와 같은 일부 상수 또는 정적 변수를 사용 하 여 클래스를 정의 하는 것입니다.

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

**X:Static Demo** 페이지는 `x:Static` 태그 확장을 사용 하는 여러 가지 방법을 보여 줍니다. 가장 자세한 방법은 `Label.FontSize` 속성-요소 태그 사이에 `StaticExtension` 클래스를 인스턴스화합니다.

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

또한 XAML 파서는 `StaticExtension` 클래스를 `x:Static`으로 약식으로 지정할 수 있습니다.

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

이 작업은 더 간단 하 게 수행할 수 있지만, 변경 내용에는 `StaticExtension` 클래스 및 멤버 설정을 중괄호 안에 배치 하는 기능이 포함 되어 있습니다. 결과 식은 `FontSize` 특성에 직접 설정 됩니다.

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

중괄호 안에는 따옴표가 *없습니다* . @No__t_1의 `Member` 속성은 더 이상 XML 특성이 아닙니다. 대신 태그 확장에 대 한 식의 일부입니다.

@No__t_0 약어를 개체 요소로 사용 하는 경우 `x:Static` 하는 것과 마찬가지로 중괄호 안의 식에서 약어를 약어로 지정할 수도 있습니다.

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

@No__t_0 클래스에는이 속성을 클래스의 기본 콘텐츠 속성으로 표시 하는 `Member` 속성을 참조 하는 `ContentProperty` 특성이 있습니다. 중괄호로 표시 된 XAML 태그 확장의 경우 식의 `Member=` 부분을 제거할 수 있습니다.

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

이는 `x:Static` 태그 확장의 가장 일반적인 형식입니다.

**정적 데모** 페이지에는 다른 두 가지 예가 포함 되어 있습니다. XAML 파일의 루트 태그에는 .NET `System` 네임 스페이스에 대 한 XML 네임 스페이스 선언이 포함 되어 있습니다.

```xaml
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

이렇게 하면 `Label` 글꼴 크기를 정적 필드 `Math.PI` 설정할 수 있습니다. 이로 인해 크기가 작은 텍스트가 되므로 `Scale` 속성이 `Math.E`로 설정 됩니다.

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

마지막 예제는 `Device.RuntimePlatform` 값을 표시 합니다. @No__t_0 static 속성은 두 `Span` 개체 사이에 줄 바꿈 문자를 삽입 하는 데 사용 됩니다.

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

@No__t_0 태그 확장은 [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension) 클래스에서 지원 됩니다. 클래스에는 `x:Name` 이름이 지정 된 페이지의 요소 이름으로 설정 된 `string` 형식의 [`Name`](xref:Xamarin.Forms.Xaml.ReferenceExtension.Name) 이라는 단일 속성이 있습니다. 이 `Name` 속성은 `ReferenceExtension`의 content 속성 이므로 `x:Reference` 중괄호로 표시 되는 경우 `Name=` 필요 하지 않습니다.

@No__t_0 태그 확장은 데이터 바인딩에만 사용 되며 문서 [**데이터 바인딩에**](~/xamarin-forms/app-fundamentals/data-binding/index.md)자세히 설명 되어 있습니다.

**X:reference Demo** 페이지는 데이터 바인딩과 `x:Reference`의 두 가지 사용, 즉 `Binding` 개체의 `Source` 속성을 설정 하는 데 사용 되는 첫 번째 및 두 번째 데이터 바인딩에 대 한 `BindingContext` 속성을 설정 하는 데 사용 되는 두 번째를 보여 줍니다. :

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

두 `x:Reference` 식 모두 `ReferenceExtension` 클래스 이름의 약식 버전을 사용 하 고 식의 `Name=` 부분을 제거 합니다. 첫 번째 예제에서는 `x:Reference` 태그 확장이 `Binding` 태그 확장에 포함 되어 있습니다. @No__t_0 및 `StringFormat` 설정은 쉼표로 구분 됩니다. 실행 중인 프로그램은 다음과 같습니다.

[![x:Reference 데모](consuming-images/referencedemo-small.png "x:Reference 데모")](consuming-images/referencedemo-large.png#lightbox "x:Reference 데모")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type 태그 확장

@No__t_0 태그 확장은 C# [`typeof`](/dotnet/csharp/language-reference/keywords/typeof/) 키워드와 동일한 XAML입니다. 클래스 또는 구조체 이름으로 설정 된 `string` 형식의 [`TypeName`](xref:Xamarin.Forms.Xaml.TypeExtension.TypeName) 라는 속성 하나를 정의 하는 [`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension) 클래스에서 지원 됩니다. @No__t_0 태그 확장은 해당 클래스 또는 구조체의 [`System.Type`](xref:System.Type) 개체를 반환 합니다. `TypeName`은 `TypeExtension`의 content 속성 이므로 `x:Type` 중괄호와 함께 표시 되는 경우 `TypeName=` 필요 하지 않습니다.

Xamarin.ios 내에는 `Type` 형식의 인수가 있는 여러 속성이 있습니다. 예를 들면 `Style`의 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) 속성 및 제네릭 클래스에서 인수를 지정 하는 데 사용 되는 [x:TypeArguments](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments) 특성이 있습니다. 그러나 XAML 파서는 `typeof` 작업을 자동으로 수행 하 고 `x:Type` 태그 확장은 이러한 경우에 사용 되지 않습니다.

@No__t_0 필요한 한 위치 *는 `x:Array`* 태그 확장을 사용 하는 것입니다 .이에 대해서는 [다음 섹션](#array)에서 설명 합니다.

@No__t_0 태그 확장은 각 메뉴 항목이 특정 형식의 개체에 해당 하는 메뉴를 생성할 때에도 유용 합니다. @No__t_0 개체를 각 메뉴 항목과 연결한 다음 메뉴 항목을 선택할 때 개체를 인스턴스화할 수 있습니다.

**태그 확장** 프로그램의 `MainPage` 탐색 메뉴가 작동 하는 방법입니다. **Mainpage** 파일에는 프로그램의 특정 페이지에 해당 하는 각 `TextCell`의 `TableView` 포함 되어 있습니다.

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

각 `CommandParameter` 속성은 다른 페이지 중 하나를 참조 하는 `x:Type` 태그 확장으로 설정 됩니다. @No__t_0 속성은 `NavigateCommand` 라는 속성에 바인딩됩니다. 이 속성은 코드 `MainPage` 파일에 정의 되어 있습니다.

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

@No__t_0 속성은 `CommandParameter` 값 &mdash; `Type` 형식의 인수를 사용 하 여 execute 명령을 구현 하는 `Command` 개체입니다. 메서드는 `Activator.CreateInstance`를 사용 하 여 페이지를 인스턴스화한 다음 탐색 합니다. 생성자는 페이지의 `BindingContext`를 자신으로 설정 하 여 종료 됩니다 .이를 통해 `Command`의 `Binding`를 사용할 수 있습니다. 이러한 코드 형식에 대 한 자세한 내용은 [**데이터 바인딩**](~/xamarin-forms/app-fundamentals/data-binding/index.md) 문서 및 특히 [**명령**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) 문서를 참조 하세요.

**X:Type Demo** 페이지는 유사한 기법을 사용 하 여 xamarin.ios 요소를 인스턴스화하고 `StackLayout`에 추가 합니다. XAML 파일은 처음에 `Binding`로 설정 된 `Command` 속성이 있는 세 개의 `Button` 요소와 `CommandParameter` 속성을 세 가지 Xamarin 양식 보기 형식으로 설정 합니다.

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

코드 숨김이 파일은 `CreateCommand` 속성을 정의 하 고 초기화 합니다.

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

@No__t_0를 누를 때 실행 되는 메서드는 인수의 새 인스턴스를 만들고 해당 `VerticalOptions` 속성을 설정한 다음 `StackLayout`에 추가 합니다. 그런 다음 세 개의 `Button` 요소가 동적으로 생성 된 뷰와 페이지를 공유 합니다.

[![x:Type 데모](consuming-images/typedemo-small.png "x:Type 데모")](consuming-images/typedemo-large.png#lightbox "x:Type 데모")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array 태그 확장

@No__t_0 태그 확장을 사용 하면 태그에서 배열을 정의할 수 있습니다. 이 클래스는 두 가지 속성을 정의 하는 [`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension) 클래스에서 지원 됩니다.

- 배열에 있는 요소의 형식을 나타내는 `Type` 형식의 `Type`입니다.
- 항목 자체의 컬렉션인 `IList` 형식의 `Items`입니다. @No__t_0의 콘텐츠 속성입니다.

@No__t_0 태그 확장 자체는 중괄호로 표시 되지 않습니다. 대신 시작 및 끝 태그 `x:Array` 항목 목록을 구분 합니다. @No__t_0 속성을 `x:Type` 태그 확장으로 설정 합니다.

**X:Array Demo** 페이지에서는 `ItemsSource` 속성을 배열로 설정 하 여 `x:Array`를 사용 하 여 `ListView`에 항목을 추가 하는 방법을 보여 줍니다.

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

@No__t_0는 각 색 항목에 대해 간단한 `BoxView`를 만듭니다.

[![x:Array Demo](consuming-images/arraydemo-small.png "x:Array Demo")](consuming-images/arraydemo-large.png#lightbox "x:Array Demo")

이 배열에서 개별 `Color` 항목을 지정 하는 방법에는 여러 가지가 있습니다. @No__t_0 태그 확장을 사용할 수 있습니다.

```xaml
<x:Static Member="Color.Blue" />
```

또는 `StaticResource`를 사용 하 여 리소스 사전에서 색을 검색할 수 있습니다.

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

@No__t_0 태그 확장은 [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension) 클래스에서 지원 됩니다. 이 클래스에는 속성이 없으며 단순히 C# [`null`](/dotnet/csharp/language-reference/keywords/null/) 키워드에 해당 하는 XAML이 있습니다.

@No__t_0 태그 확장은 거의 필요 하지 않지만 거의 사용 되지 않지만, 필요한 경우에는 존재 하는 것이 좋습니다.

**X:Null Demo** 페이지는 `x:Null` 편리 하 게 사용할 수 있는 한 가지 시나리오를 보여 줍니다. @No__t_3 속성을 플랫폼에 종속 된 패밀리 이름으로 설정 하는 `Setter`를 포함 하는 `Label`에 대해 암시적 `Style`를 정의 한다고 가정 합니다.

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

그런 다음 `Label` 요소 중 하나에 대해 `FontFamily`를 제외 하 고 암시적 `Style`의 모든 속성 설정을 선택 하 여 기본값을 사용할 수 있습니다. 이 목적을 위해 다른 `Style`를 정의할 수 있지만, 가운데 `Label`에 설명 된 것 처럼 간단한 방법은 단순히 특정 `Label`의 `FontFamily` 속성을 `x:Null`로 설정 하는 것입니다.

실행 중인 프로그램은 다음과 같습니다.

[![x:Null 데모](consuming-images/nulldemo-small.png "x:Null 데모")](consuming-images/nulldemo-large.png#lightbox "x:Null 데모")

@No__t_0 요소 중 4 개에는 세리프 글꼴이 있지만 가운데 `Label`에는 기본 sans-serif 글꼴이 있습니다.

<a name="onplatform" />

## <a name="onplatform-markup-extension"></a>OnPlatform 태그 확장

@No__t_0 태그 확장을 사용 하 여 플랫폼 별로 UI 모양을 사용자 지정할 수 있습니다. [@No__t_1](xref:Xamarin.Forms.OnPlatform`1) 및 [`On`](xref:Xamarin.Forms.On) 클래스와 동일한 기능을 제공 하지만 보다 간결한 표현을 제공 합니다.

@No__t_0 태그 확장은 다음 속성을 정의 하는 [`OnPlatformExtension`](xref:Xamarin.Forms.Xaml.OnPlatformExtension) 클래스에서 지원 됩니다.

- `object` 형식의 `Default` 이며, 플랫폼을 나타내는 속성에 적용 되는 기본값으로 설정 됩니다.
- Android에 적용할 값으로 설정 하는 `object` 유형의 `Android`입니다.
- GTK 플랫폼에 적용 되는 값으로 설정 하는 `object` 형식의 `GTK`입니다.
- iOS에 적용할 값으로 설정 하는 `object` 형식의 `iOS`입니다.
- macOS에 적용 되는 값으로 설정 하는 `object` 유형의 `macOS`입니다.
- Tizen 플랫폼에 적용 되는 값으로 설정 하는 `object` 형식의 `Tizen`입니다.
- 유니버설 Windows 플랫폼에 적용할 값으로 설정 하는 `object` 형식의 `UWP`입니다.
- Windows Presentation Foundation 플랫폼에 적용 되는 값으로 설정 하는 `object` 형식의 `WPF`입니다.
- `IValueConverter` 구현으로 설정 된 `IValueConverter` 형식의 `Converter`입니다.
- `IValueConverter` 구현에 전달할 값으로 설정 하는 `object` 형식의 `ConverterParameter`입니다.

> [!NOTE]
> XAML 파서는 [`OnPlatformExtension`](xref:Xamarin.Forms.Xaml.OnPlatformExtension) 클래스를 `OnPlatform`으로 약식으로 지정할 수 있습니다.

@No__t_0 속성은 `OnPlatformExtension`의 content 속성입니다. 따라서 중괄호로 표시 되는 XAML 태그 식의 경우 첫 번째 인수인 경우 식의 `Default=` 부분을 제거할 수 있습니다.

> [!IMPORTANT]
> XAML 파서는 `OnPlatform` 태그 확장을 사용 하는 속성에 올바른 형식의 값이 제공 될 것으로 예상 합니다. 형식 변환이 필요한 경우 `OnPlatform` 태그 확장에서 Xamarin.ios에서 제공 하는 기본 변환기를 사용 하 여이를 수행 하려고 시도 합니다. 그러나 기본 변환기에서 수행할 수 없는 일부 형식 변환이 있습니다. 이러한 경우에는 `Converter` 속성이 `IValueConverter` 구현으로 설정 되어야 합니다.

**Onplatform Demo** 페이지는 `OnPlatform` 태그 확장을 사용 하는 방법을 보여 줍니다.

```xaml
<BoxView Color="{OnPlatform Yellow, iOS=Red, Android=Green, UWP=Blue}"
         WidthRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"  
         HeightRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"
         HorizontalOptions="Center" />
```

이 예제에서는 세 개의 `OnPlatform` 식이 `OnPlatformExtension` 클래스 이름의 약식 버전을 사용 합니다. 3 개의 `OnPlatform` 태그 확장은 [`BoxView`](xref:Xamarin.Forms.BoxView) 의 [`Color`](xref:Xamarin.Forms.BoxView.Color), [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)및 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 속성을 iOS, Android 및 UWP의 다른 값으로 설정 합니다. 태그 확장은 또한 지정 되지 않은 플랫폼에서 이러한 속성에 대 한 기본값을 제공 하지만 식의 `Default=` 부분을 제거 합니다. 설정 된 태그 확장 속성은 쉼표로 구분 됩니다.

실행 중인 프로그램은 다음과 같습니다.

[![OnPlatform Demo](consuming-images/onplatformdemo-small.png "OnPlatform Demo")](consuming-images/onplatformdemo-large.png#lightbox "OnPlatform Demo")

<a name="onidiom" />

## <a name="onidiom-markup-extension"></a>OnIdiom 태그 확장

@No__t_0 태그 확장을 사용 하면 응용 프로그램이 실행 되 고 있는 장치를 기준으로 UI 모양을 사용자 지정할 수 있습니다. 다음 속성을 정의 하는 [`OnIdiomExtension`](xref:Xamarin.Forms.Xaml.OnIdiomExtension) 클래스에서 지원 됩니다.

- `object` 형식의 `Default` 장치 관용구을 나타내는 속성에 적용 되는 기본값으로 설정 됩니다.
- 휴대폰에 적용 되는 값으로 설정 하는 `object` 형식의 `Phone`입니다.
- 태블릿에 적용할 값으로 설정 하는 `object` 형식의 `Tablet`입니다.
- 데스크톱 플랫폼에 적용 되는 값으로 설정 하는 `object` 형식의 `Desktop`입니다.
- TV 플랫폼에 적용 되는 값으로 설정 하는 `object` 유형의 `TV`입니다.
- 조사식 플랫폼에 적용 되는 값으로 설정 하는 `object` 형식의 `Watch`입니다.
- `IValueConverter` 구현으로 설정 된 `IValueConverter` 형식의 `Converter`입니다.
- `IValueConverter` 구현에 전달할 값으로 설정 하는 `object` 형식의 `ConverterParameter`입니다.

> [!NOTE]
> XAML 파서는 [`OnIdiomExtension`](xref:Xamarin.Forms.Xaml.OnIdiomExtension) 클래스를 `OnIdiom`으로 약식으로 지정할 수 있습니다.

@No__t_0 속성은 `OnIdiomExtension`의 content 속성입니다. 따라서 중괄호로 표시 되는 XAML 태그 식의 경우 첫 번째 인수인 경우 식의 `Default=` 부분을 제거할 수 있습니다.

> [!IMPORTANT]
> XAML 파서는 `OnIdiom` 태그 확장을 사용 하는 속성에 올바른 형식의 값이 제공 될 것으로 예상 합니다. 형식 변환이 필요한 경우 `OnIdiom` 태그 확장에서 Xamarin.ios에서 제공 하는 기본 변환기를 사용 하 여이를 수행 하려고 시도 합니다. 그러나 기본 변환기에서 수행할 수 없는 일부 형식 변환이 있습니다. 이러한 경우에는 `Converter` 속성이 `IValueConverter` 구현으로 설정 되어야 합니다.

**Onidiom Demo** 페이지에서는 `OnIdiom` 태그 확장을 사용 하는 방법을 보여 줍니다.

```xaml
<BoxView Color="{OnIdiom Yellow, Phone=Red, Tablet=Green, Desktop=Blue}"
         WidthRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HeightRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HorizontalOptions="Center" />
```

이 예제에서는 세 개의 `OnIdiom` 식이 `OnIdiomExtension` 클래스 이름의 약식 버전을 사용 합니다. 3 개의 `OnIdiom` 태그 확장은 [`BoxView`](xref:Xamarin.Forms.BoxView) 의 [`Color`](xref:Xamarin.Forms.BoxView.Color), [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)및 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 속성을 phone, 태블릿 및 데스크톱 관용구의 다른 값으로 설정 합니다. 또한 태그 확장은 지정 되지 않은 관용구의 이러한 속성에 대 한 기본값을 제공 하는 한편 식의 `Default=` 부분을 제거 합니다. 설정 된 태그 확장 속성은 쉼표로 구분 됩니다.

실행 중인 프로그램은 다음과 같습니다.

[![Ononedemo](consuming-images/onidiomdemo-small.png "Ononedemo")](consuming-images/onidiomdemo-large.png#lightbox "Ononedemo")

## <a name="datatemplate-markup-extension"></a>DataTemplate 태그 확장

@No__t_0 태그 확장을 사용 하 여 형식을 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)변환할 수 있습니다. @No__t_3로 변환할 형식의 이름으로 설정 된 `string` 형식의 `TypeName` 속성을 정의 하는 `DataTemplateExtension` 클래스에서 지원 됩니다. @No__t_0 속성은 `DataTemplateExtension`의 content 속성입니다. 따라서 중괄호로 표현 된 XAML 태그 식의 경우 식의 `TypeName=` 부분을 제거할 수 있습니다.

> [!NOTE]
> XAML 파서는 `DataTemplateExtension` 클래스를 `DataTemplate`으로 약식으로 지정할 수 있습니다.

이 태그 확장은 다음 예제와 같이 셸 응용 프로그램에서 일반적으로 사용 됩니다.

```xaml
<ShellContent Title="Monkeys"
              Icon="monkey.png"
              ContentTemplate="{DataTemplate views:MonkeysPage}" />
```

이 예제에서 `MonkeysPage`은 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 에서 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)로 변환 되며, `ShellContent.ContentTemplate` 속성 값으로 설정 됩니다. 이렇게 하면 `MonkeysPage`는 응용 프로그램 시작이 아닌 페이지 탐색이 발생 하는 경우에만 만들어집니다.

셸 응용 프로그램에 대 한 자세한 내용은 [Xamarin.ios shell](~/xamarin-forms/app-fundamentals/shell/index.md)(영문)을 참조 하세요.

## <a name="fontimage-markup-extension"></a>FontImage 태그 확장

@No__t_0 태그 확장을 사용 하면 `ImageSource`를 표시할 수 있는 모든 보기에 글꼴 아이콘을 표시할 수 있습니다. @No__t_0 클래스와 동일한 기능을 제공 하지만 보다 간결한 표현을 제공 합니다.

@No__t_0 태그 확장은 다음 속성을 정의 하는 `FontImageExtension` 클래스에서 지원 됩니다.

- `string` 형식의 `FontFamily` 글꼴 아이콘이 속한 글꼴 패밀리입니다.
- `string` 형식의 `Glyph` 글꼴 아이콘의 유니코드 문자 값입니다.
- `Color` 형식의 `Color` 글꼴 아이콘을 표시할 때 사용 되는 색입니다.
- 렌더링 된 글꼴 아이콘의 크기 (장치 독립적 단위) 인 `double` 형식의 `Size`입니다.

> [!NOTE]
> XAML 파서는 `FontImageExtension` 클래스를 `FontImage`으로 약식으로 지정할 수 있습니다.

@No__t_0 속성은 `FontImageExtension`의 content 속성입니다. 따라서 중괄호로 표시 되는 XAML 태그 식의 경우 첫 번째 인수인 경우 식의 `Glyph=` 부분을 제거할 수 있습니다.

**FontImage Demo** 페이지에서는 `FontImage` 태그 확장을 사용 하는 방법을 보여 줍니다.

```xaml
<Image BackgroundColor="#D1D1D1"
       Source="{FontImage &#xf30c;, FontFamily={OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=44}" />
```

이 예제에서는 `FontImageExtension` 클래스 이름의 축약 된 버전을 사용 하 여 [`Image`](xref:Xamarin.Forms.Image)에 있는 아이콘 글꼴 제품군에서 XBox 아이콘을 표시 합니다. 또한이 식은 `OnPlatform` 태그 확장을 사용 하 여 iOS 및 Android에서 다른 `FontFamily` 속성 값을 지정 합니다. 또한 식의 `Glyph=` 부분이 제거 되며 설정 된 태그 확장 속성은 쉼표로 구분 됩니다. 아이콘의 유니코드 문자는 `\uf30c` 있지만 XAML로 이스케이프 되어야 하므로 `&#xf30c;` 됩니다.

실행 중인 프로그램은 다음과 같습니다.

[![FontImage 태그 확장의 스크린샷](consuming-images/fontimagedemo.png "FontImage 데모")](consuming-images/fontimagedemo-large.png#lightbox "FontImage 데모")

@No__t_0 개체에서 글꼴 아이콘 데이터를 지정 하 여 글꼴 아이콘을 표시 하는 방법에 대 한 자세한 내용은 [글꼴 아이콘 표시](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons)를 참조 하세요.

## <a name="define-your-own-markup-extensions"></a>사용자 고유의 태그 확장 정의

Xamarin.ios에서 사용할 수 없는 XAML 태그 확장이 필요한 경우 [직접 만들](creating.md)수 있습니다.

## <a name="related-links"></a>관련 링크

- [태그 확장 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [Xamarin.ios의 XAML 태그 확장 장 서적](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)
- [동적 스타일](~/xamarin-forms/user-interface/styles/dynamic.md)
- [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin.ios Shell](~/xamarin-forms/app-fundamentals/shell/index.md).
