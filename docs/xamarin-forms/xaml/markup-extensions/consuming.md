---
title: "XAML 태그 확장 사용"
description: "Xamarin.Forms에 사용할 수 있는 XAML 태그 확장 사용"
ms.topic: article
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: cc4407be9dee7e19dbf1f3cc03b3b88191717e6f
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="consuming-xaml-markup-extensions"></a>XAML 태그 확장 사용

XAML 태그 확장을 강화 강력 함 및 유연성 XAML의 다양 한 소스에서 요소 특성을 허용 하 여 합니다. 몇 가지 XAML 태그 확장은 XAML 2009 사양의 일부입니다. 이러한 일반적인를 사용 하 여 XAML 파일에 나타납니다 `x` 네임 스페이스 접두사 및가 흔히이 접두사로 합니다. 아래 섹션에 설명 되어 있습니다.

- [`x:Static`](#static) &ndash; 정적 속성, 필드 또는 열거형 멤버를 참조 합니다.
- [`x:Reference`](#reference) &ndash; 명명 된 페이지에서 요소를 참조 합니다.
- [`x:Type`](#type) &ndash; 특성으로 설정 된 `System.Type` 개체입니다.
- [`x:Array`](#array) &ndash; 특정 유형의 개체의 배열을 생성 합니다.
- [`x:Null`](#null) &ndash; 특성으로 설정 된 `null` 값입니다.

다른 세 XAML 태그 확장명 다른 XAML 구현에서 지금까지 지원 및 Xamarin.Forms 에서도 지원 됩니다. 보다 완전 하 게 다른 문서에 설명 되어 있습니다.

- `StaticResource` &ndash; 문서에 설명 된 대로 리소스 사전에서 개체를 참조 [ **리소스 사전을**](~/xamarin-forms/xaml/resource-dictionaries.md)합니다.
- `DynamicResource` &ndash; 문서에 설명 된 대로 리소스 사전에 있는 개체에 대 한 변경 내용에 응답 [ **동적 스타일**](~/xamarin-forms/user-interface/styles/dynamic.md)합니다.
- `Binding` &ndash; 문서에 설명 된 대로 두 개체의 속성 사이의 연결을 설정할 [ **데이터 바인딩**](~/xamarin-forms/app-fundamentals/data-binding/index.md)합니다.
- `TemplateBinding` &ndash; 문서에 설명 된 대로 컨트롤 서식 파일에서 데이터 바인딩을 수행 [**컨트롤 서식 파일에서 바인딩**] / 안내선/xamarin forms/응용 프로그램-기본 사항/템플릿/컨트롤-템플릿/템플릿-바인딩 /)

[ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) 레이아웃 사용자 지정 태그 확장 사용 [ `ConstraintExpression` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ConstraintExpression/)합니다. 이 태그 확장의 문서에 설명 되어 [ **RelativeLayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md)합니다.

<a name="static" />

## <a name="xstatic-markup-extension"></a>x:Static 태그 확장명

`x:Static` 태그 확장으로 사용할 수는 [ `StaticExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) 클래스입니다. 클래스에는 단일 속성인 [ `Member` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.StaticExtension.Member/) 형식의 `string` 공용 상수, 정적 속성, 정적 필드 또는 열거형 멤버의 이름으로 설정 하는 합니다.

사용 하는 하나의 일반적인 방법은 `x:Static` 먼저와 같은 일부 상수 또는 정적 변수를 사용 하는 클래스를 정의 하는 것이 아주 작은 `AppConstants` 클래스에 [ **MarkupExtensions** ](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/) 프로그램:

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

**X:static 데모** 페이지 사용 하는 방법을 보여 줍니다는 `x:Static` 태그 확장 합니다. 가장 자세 접근 방식에서 인스턴스화하는 `StaticExtension` 간의 클래스 `Label.FontSize` 속성 요소 태그:

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

XAML 파서에서 허용는 `StaticExtension` 클래스로 축약할 수를 `x:Static`:

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

더욱 간단 해질 수 있습니다 하지만 변경에 몇 가지 새 구문을 소개: 배치 이루어져는 `StaticExtension` 클래스와 멤버 중괄호로 설정이 합니다. 결과 식에 직접 설정 되 고 `FontSize` 특성:

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

있으며 *없는* 중괄호 내에 따옴표입니다. `Member` 속성 `StaticExtension` 는 더 이상 XML 특성입니다. 대신의 일부인 태그 확장에 대 한 식입니다.

축약할 수와 마찬가지로 `x:StaticExtension` 를 `x:Static` 개체 요소를 사용 하면 축약할 수 있습니다 또한 중괄호 안에 있는 식의:

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

`StaticExtension` 클래스에는 `ContentProperty` 속성을 참조 하는 특성 `Member`,이 속성은 클래스의 기본 콘텐츠 속성으로 표시 하는입니다. 중괄호로 표현 된 XAML 태그 확장을 제거할 수 있습니다는 `Member=` 식의 일부:

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

이것은 가장 일반적인 형태의 `x:Static` 태그 확장 합니다.

**정적 데모** 페이지에 다른 두 가지 예입니다. XAML 파일의 루트 태그.NET에 대 한 XML 네임 스페이스 선언을 포함 `System` 네임 스페이스:

```xaml
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

이 통해는 `Label` 정적 필드에 설정할 글꼴 크기 `Math.PI`합니다. 대신 작은 텍스트 인해 하므로 `Scale` 속성이 `Math.E`:

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

마지막 예에서는 표시 된 `Device.RuntimePlatform` 값입니다. `Environment.NewLine` 정적 속성을 사용 하 고 둘 사이의 줄 바꿈 문자를 삽입할 `Span` 개체:

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

다음은 세 플랫폼 모두에서 실행 되는 샘플입니다.

[![X:static 데모](consuming-images/staticdemo-small.png "X:static 데모")](consuming-images/staticdemo-large.png "X:static 데모")

<a name="reference" />

## <a name="xreference-markup-extension"></a>x:Reference 태그 확장명

`x:Reference` 태그 확장으로 사용할 수는 [ `ReferenceExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) 클래스입니다. 클래스에는 단일 속성인 [ `Name` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.ReferenceExtension.Name/) 형식의 `string` 사용 하 여 이름을 지정 하는 페이지에 있는 요소의 이름으로 설정 하는 `x:Name`합니다. 이 `Name` 속성의 콘텐츠 속성은 `ReferenceExtension`하므로 `Name=` 필요 없는 경우 `x:Reference` 중괄호에 나타납니다.

`x:Reference` 태그 확장을 사용 하는 문서에 자세히 설명 하는 데이터 바인딩을 사용 하는 단독으로 [ **데이터 바인딩**](~/xamarin-forms/app-fundamentals/data-binding/index.md)합니다.

**X:reference 데모** 페이지의 두 가지 용도 보여 줍니다. `x:Reference` 데이터 바인딩을 사용 하는 설정에 사용 된 첫 번째는 `Source` 의 속성은 `Binding` 개체 및 설정에 사용 된 두 번째는 `BindingContext` 두 데이터 바인딩에 대 한 속성:

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

둘 다 `x:Reference` 의 축약된 버전을 사용 하 여 식에서 `ReferenceExtension` 클래스 이름 및 제거는 `Name=` 식의 부분입니다. 첫 번째 예제에서는 `x:Reference` 태그 확장에 포함 되어는 `Binding` 태그 확장 합니다. 에 `Source` 및 `StringFormat` 설정을 쉼표로 구분 됩니다. 다음은 세 플랫폼 모두에서 실행 중인 프로그램입니다.

[![X:reference 데모](consuming-images/referencedemo-small.png "X:reference 데모")](consuming-images/referencedemo-large.png "X:reference 데모")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type 태그 확장명

`x:Type` 태그 확장은 C#의 해당 하는 XAML [ `typeof` ](/dotnet/csharp/language-reference/keywords/typeof/) 키워드입니다. 지원 되는 [ `TypeExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/) 라는 하나의 속성을 정의 하는 클래스 [ `TypeName` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.TypeExtension.TypeName/) 형식의 `string` 클래스 또는 구조체 이름으로 설정 되어 있는 합니다. `x:Type` 태그 확장 반환 된 [ `System.Type` ](https://developer.xamarin.com/api/type/System.Type/) 해당 클래스 또는 구조체의 개체입니다. `TypeName` 콘텐츠 속성인 `TypeExtension`하므로 `TypeName=` 필요 없는 경우 `x:Type` 중괄호로 표시 됩니다.

Xamarin.Forms에 내에서 여러 가지 속성 형식의 인수가 있는 `Type`합니다. 예로 [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) 속성 `Style`, 및 [X:typearguments](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments) 제네릭 클래스의 인수를 지정 하는 데 사용 되는 특성입니다. 하지만 XAML 파서가 수행는 `typeof` 작업이 자동으로 및 `x:Type` 이러한 경우에는 태그 확장 사용 되지 않습니다.

한 곳 여기서 `x:Type` *은* 와 필요는 `x:Array` 에 설명 된 태그 확장의 [다음 섹션](#array)합니다.

`x:Type` 메뉴가 특정 유형의 개체에 각 메뉴 항목을 해당 하는 위치를 구성할 때 태그 확장 도움이 됩니다. 연결할 수는 `Type` 각 메뉴 항목 개체를 메뉴 항목을 선택할 때 다음 개체를 인스턴스화합니다.

이 어떻게 탐색 메뉴에서 `MainPage` 에 **태그 확장** 프로그램 작동 합니다. **MainPage.xaml** 파일에 포함 되어는 `TableView` 각 `TextCell` 에 해당 하는 프로그램의 특정 페이지:

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

여기는 첫 주 페이지 **태그 확장**:

[![주 페이지](consuming-images/mainpage-small.png "Main 페이지")](consuming-images/mainpage-large.png "주 페이지")

각 `CommandParameter` 속성이로 설정 되는 `x:Type` 다른 페이지 중 하나를 참조 하는 태그 확장입니다. `Command` 속성이 라는 속성이에 바인딩될 `NavigateCommand`합니다. 이 속성에 정의 된는 `MainPage` 코드 숨김 파일:

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

`NavigateCommand` 속성은 한 `Command` execute 명령이 형식의 인수를 구현 하는 개체 `Type` &mdash; 값 `CommandParameter`합니다. 메서드에서 사용 `Activator.CreateInstance` 를 인스턴스화하는 페이지를 이동 합니다. 설정 하 여 결론을 내립니다 생성자는 `BindingContext` 페이지 자신에 게이 통해는 `Binding` 에 `Command` 에서 실행 되도록 합니다. 참조는 [ **데이터 바인딩** ](~/xamarin-forms/app-fundamentals/data-binding/index.md) 문서와 특히 [ **직속 상관** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) 이러한 종류의 코드에 대 한 자세한 내용은 문서.

**X:type 데모** 페이지 Xamarin.Forms 요소 인스턴스화할를를 추가 하는 비슷한 기술을 사용 하 여 한 `StackLayout`합니다. XAML 파일의 경우 세 처음 구성 됩니다 `Button` 요소를 자신의 `Command` 속성으로 설정 된 `Binding` 및 `CommandParameter` 속성이 세 Xamarin.Forms 뷰 유형으로 설정:

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

코드 숨김 파일을 정의 하 고 초기화는 `CreateCommand` 속성:

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

메서드는 될 때 실행 되는 `Button` 눌려질 인수의 새 인스턴스를 만들고, 설정 해당 `VerticalOptions` 속성에 추가 `StackLayout`합니다. 세 개의 `Button` 요소가 동적으로 생성된 된 뷰가 포함 된 다음 페이지를 공유 합니다.

[![X:type 데모](consuming-images/typedemo-small.png "X:type 데모")](consuming-images/typedemo-large.png "X:type 데모")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array 태그 확장명

`x:Array` 태그 확장을 사용 하면 태그에서 배열을 정의할 수 있습니다. 지원 되는 [ `ArrayExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/) 두 속성을 정의 하는 클래스:

- `Type` 형식의 `Type`, 배열에 있는 요소의 형식을 나타냅니다.
- `Items` 형식의 `IList`를 자체의 컬렉션입니다. 콘텐츠 속성인 `ArrayExtension`합니다.

`x:Array` 자체 태그 확장 중괄호로 표시 되지 않습니다. 대신, `x:Array` 시작 및 끝 태그 항목 목록을 구분 합니다. 설정의 `Type` 속성을는 `x:Type` 태그 확장 합니다.

**X:array 데모** 페이지를 사용 하는 방법을 보여 줍니다 `x:Array` 항목을 추가 하는 `ListView` 설정 하 여는 `ItemsSource` 배열에는 속성:

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

`ViewCell` 간단한 만듭니다 `BoxView` 각 색 항목에 대해:

[![X:array 데모](consuming-images/arraydemo-small.png "X:array 데모")](consuming-images/arraydemo-large.png "X:array 데모")

개별을 지정 하는 방법은 여러 가지가 `Color` 이 배열의 항목입니다. 사용할 수는 `x:Static` 태그 확장:

```xaml
<x:Static Member="Color.Blue" />
```

사용할 수 있습니다 또는 `StaticResource` 리소스 사전에서 색을 검색 하려면:

```xaml
<StaticResource Key="myColor" />
```

이 문서 끝 방향으로 새 색 값을 만드는 사용자 지정 XAML 태그 확장에 표시 됩니다.

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

문자열 또는 숫자와 같은 일반적인 형식 배열을 정의할 때는 사용에 나열 된 태그는 [ **생성자 인수를 전달** ](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) 값을 구분 하는 문서입니다.

<a name="null" />

## <a name="xnull-markup-extension"></a>x:Null 태그 확장명

`x:Null` 태그 확장으로 사용할 수는 [ `NullExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/) 클래스입니다. 속성이 없습니다.이 있으며 단순히 XAML에 해당 하는 C#의 [ `null` ](/dotnet/csharp/language-reference/keywords/null/) 키워드입니다.

`x:Null` 필요를 찾을 두어야 존재 하는 있지만 태그 확장 거의 필요 하 고 거의 사용 합니다.

**X:null 데모** 페이지 한 시나리오를 보여 줍니다. 때 `x:Null` 편리할 수 있습니다. 암시적을 정의할 수 `Style` 에 대 한 `Label` 포함 하는 `Setter` 로 설정 하는 `FontFamily` 속성을 플랫폼별 제품군 이름:

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

다음 중 하나에 대 한 사실을 발견는 `Label` 요소를 암시적에 있는 모든 속성 설정 하려는 `Style` 을 제외 하 고는 `FontFamily`, 기본값이 될 하고자 하 합니다. 다른 정의할 수 있습니다 `Style` 간단 하지만 해당 용도 대 한 간단히 설정 하는 것의 `FontFamily` 특정 속성 `Label` 를 `x:Null`가운데에 표시 된 대로, `Label`합니다.

다음은 세 가지 플랫폼에서 실행 중인 프로그램입니다.

[![X:null 데모](consuming-images/nulldemo-small.png "X:null 데모")](consuming-images/nulldemo-large.png "X:null 데모")

해당 4 통지는 `Label` 요소 중심 하지만 serif 글꼴에는 `Label` 기본 굴림 글꼴에 합니다.

## <a name="define-your-own-markup-extensions"></a>고유한 태그 확장을 정의 합니다.

Xamarin.Forms에서 사용할 수 없는 XAML 태그 확장에 대 한 요구가 발생 한 경우 다음을 할 수 있습니다 [나만의 사이트 생성](creating.md)합니다.


## <a name="related-links"></a>관련 링크

- [태그 확장 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Xamarin.Forms 책에서 XAML 태그 확장 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)
- [동적 스타일](~/xamarin-forms/user-interface/styles/dynamic.md)
- [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)
