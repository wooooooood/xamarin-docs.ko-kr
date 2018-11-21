---
title: XAML 태그 확장 사용
description: 이 문서에서는 다양 한 원본에서에서 설정할 요소 특성을 허용 하 여 성능과 XAML의 유연성을 강화 하기 위해 Xamarin.Forms XAML 태그 확장을 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/01/2018
ms.openlocfilehash: 2ab7381baefc6ca013b6c8a5c9f7bf7b5cae8b10
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171718"
---
# <a name="consuming-xaml-markup-extensions"></a>XAML 태그 확장 사용

XAML 태그 확장 다양 한 원본에서에서 설정할 요소 특성을 허용 하 여 성능과 XAML의 유연성을 향상 하도록 도와줍니다. 몇 가지 XAML 태그 확장은 XAML 2009 사양의 일부입니다. 이러한 일반적인 XAML 파일에서 표시 `x` 되며 네임 스페이스 접두사를 흔히이 접두사를 사용 하 여 합니다. 다음 태그 확장에 설명 합니다.

- [`x:Static`](#static) – 정적 속성, 필드 또는 열거형 멤버 참조 합니다.
- [`x:Reference`](#reference) – 명명 된 페이지의 요소 참조 합니다.
- [`x:Type`](#type) – 특성으로는 `System.Type` 개체입니다.
- [`x:Array`](#array) – 특정 형식의 개체 배열을 생성 합니다.
- [`x:Null`](#null) – 특성으로는 `null` 값입니다.
- [`OnPlatform`](#onplatform) – 플랫폼별 기준 UI 모양을 사용자 지정 합니다.
- [`OnIdiom`](#onidiom) – 응용 프로그램에서 실행 중인 장치의 관용구를 기반으로 하는 UI 모양을 사용자 지정 합니다.

추가 XAML 태그 확장 지금까지 다른 XAML 구현에서 지 원하는 및 Xamarin.Forms 에서도 지원 됩니다. 이러한 다른 문서에 자세히 설명 된:

- `StaticResource` &ndash; 이 문서에 설명 된 대로 리소스 사전에서 개체를 참조 [ **리소스가**](~/xamarin-forms/xaml/resource-dictionaries.md)합니다.
- `DynamicResource` &ndash; 이 문서에 설명 된 대로 리소스 사전에 있는 개체에 대 한 변경 내용에 응답할 [ **동적 스타일**](~/xamarin-forms/user-interface/styles/dynamic.md)합니다.
- `Binding` &ndash; 이 문서에 설명 된 대로 두 개체의 속성 사이의 연결을 설정할 [ **데이터 바인딩**](~/xamarin-forms/app-fundamentals/data-binding/index.md)합니다.
- `TemplateBinding` &ndash; 문서에 설명 된 대로 데이터 바인딩 컨트롤 템플릿에서 수행 [ **바인딩 컨트롤 템플릿에서**](/guides/xamarin-forms/application-fundamentals/templates/control-templates/template-binding/)합니다.

합니다 [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout) 레이아웃 사용자 지정 태그 확장 사용 [ `ConstraintExpression` ](xref:Xamarin.Forms.ConstraintExpression)합니다. 이 태그 확장은 문서에서 설명한 [ **RelativeLayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md)합니다.

<a name="static" />

## <a name="xstatic-markup-extension"></a>x:Static 태그 확장명

합니다 `x:Static` 태그 확장 되는 [ `StaticExtension` ](xref:Xamarin.Forms.Xaml.StaticExtension) 클래스입니다. 클래스에 라는 단일 속성만 [ `Member` ](xref:Xamarin.Forms.Xaml.StaticExtension.Member) 형식의 `string` 공용 상수, 정적 속성, 정적 필드 또는 열거형 멤버의 이름으로 설정 하는 합니다.

사용 하 여 한 가지 일반적인 방법 `x:Static` 와 같은 일부 상수 또는 정적 변수를 사용 하 여 클래스를 먼저 정의 하는 것이 작은 `AppConstants` 클래스는 [ **MarkupExtensions** ](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/) 프로그램:

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

합니다 **X:static 데모** 페이지를 사용 하는 여러 방법을 보여 줍니다는 `x:Static` 태그 확장 합니다. 가장 자세한 정보 표시 방법 인스턴스화하는 `StaticExtension` 간의 클래스 `Label.FontSize` 속성 요소 태그:

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

XAML 파서에서 허용 합니다 `StaticExtension` 로 축약할 수 클래스 `x:Static`:

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

더 나아가 단순화할 수 있습니다. 하지만 변경에 몇 가지 새 구문을 소개: 배치로 구성 됩니다는 `StaticExtension` 클래스와 중괄호에서 설정 멤버입니다. 결과 식에 직접 설정 되는 `FontSize` 특성:

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

있으며 *없습니다* 중괄호 안의 따옴표입니다. 합니다 `Member` 속성의 `StaticExtension` 은 더 이상 XML 특성입니다. 대신의 일부인 태그 확장에 대 한 식입니다.

축약할 수와 마찬가지로 `x:StaticExtension` 에 `x:Static` 개체 요소를 사용 하면 축약할 수 있습니다도 중괄호 내에서 식에서:

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

합니다 `StaticExtension` 클래스에는 `ContentProperty` 속성을 참조 하는 특성 `Member`, 클래스의 기본 콘텐츠 속성으로이 속성을 표시 하는 합니다. 중괄호를 사용 하 여 표현 XAML 태그 확장을 제거할 수 있습니다는 `Member=` 식의 일부:

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

이것은 가장 일반적인 형태의 `x:Static` 태그 확장 합니다.

합니다 **정적 데모** 페이지에 두 가지 예제가 포함 되어 있습니다. XAML 파일의 루트 태그는.NET에 대 한 XML 네임 스페이스 선언을 포함 `System` 네임 스페이스:

```xaml
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

따라서 합니다 `Label` 정적 필드에 설정할 글꼴 크기 `Math.PI`합니다. 결과는 작은 텍스트 이므로 `Scale` 속성이 `Math.E`:

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

마지막 예에서는 표시 된 `Device.RuntimePlatform` 값입니다. 합니다 `Environment.NewLine` 정적 속성은 둘 사이의 줄 바꿈 문자를 삽입 데 `Span` 개체:

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

샘플이 실행 되는 다음과 같습니다.

[![X:static 데모](consuming-images/staticdemo-small.png "X:static 데모")](consuming-images/staticdemo-large.png#lightbox "X:static 데모")

<a name="reference" />

## <a name="xreference-markup-extension"></a>x:Reference 태그 확장명

합니다 `x:Reference` 태그 확장 되는 [ `ReferenceExtension` ](xref:Xamarin.Forms.Xaml.ReferenceExtension) 클래스입니다. 클래스에 라는 단일 속성만 [ `Name` ](xref:Xamarin.Forms.Xaml.ReferenceExtension.Name) 형식의 `string` 이름을 있는 페이지의 요소 이름으로 설정 하는 `x:Name`합니다. 이렇게 `Name` 속성의 콘텐츠 속성은 `ReferenceExtension`이므로 `Name=` 필요 없는 경우 `x:Reference` 중괄호로 표시 됩니다.

합니다 `x:Reference` 태그 확장을 사용 하 여 문서에서 자세히 설명 하는 데이터 바인딩을 사용 하 여 단독으로 [ **데이터 바인딩**](~/xamarin-forms/app-fundamentals/data-binding/index.md)합니다.

**X:reference 데모** 페이지의 두 가지 용도 보여 줍니다. `x:Reference` 데이터 바인딩을 사용 하 여 설정에 사용 된 첫 번째는 `Source` 속성을 `Binding` 개체 및 설정에 사용 된 두 번째는 `BindingContext` 두 데이터 바인딩에 대 한 속성:

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

둘 다 `x:Reference` 식의 약식된 버전을 사용 합니다 `ReferenceExtension` 클래스 이름 및 제거는 `Name=` 식의 일부입니다. 첫 번째 예에서는 합니다 `x:Reference` 태그 확장에 포함 되어는 `Binding` 태그 확장 합니다. 에 `Source` 및 `StringFormat` 설정 쉼표로 구분 됩니다. 실행 중인 프로그램이 다음과 같습니다.

[![X:reference 데모](consuming-images/referencedemo-small.png "X:reference 데모")](consuming-images/referencedemo-large.png#lightbox "X:reference 데모")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type 태그 확장명

합니다 `x:Type` 태그 확장은 XAML에 해당 하는 C# [ `typeof` ](/dotnet/csharp/language-reference/keywords/typeof/) 키워드입니다. 지 원하는 합니다 [ `TypeExtension` ](xref:Xamarin.Forms.Xaml.TypeExtension) 라는 속성이 하나 정의 하는 클래스 [ `TypeName` ](xref:Xamarin.Forms.Xaml.TypeExtension.TypeName) 형식의 `string` 클래스 또는 구조체 이름으로 설정 된 합니다. 합니다 `x:Type` 태그 확장 반환 합니다 [ `System.Type` ](xref:System.Type) 해당 클래스 또는 구조체의 개체입니다. `TypeName` 콘텐츠 속성인 `TypeExtension`이므로 `TypeName=` 필요 없는 경우 `x:Type` 중괄호를 사용 하 여 표시 됩니다.

에서는 Xamarin.Forms 내의 여러 가지 속성 형식의 인수가 있는 `Type`합니다. 예로 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) 속성을 `Style`, 및 [X:typearguments](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments) 제네릭 클래스에서 인수를 지정 하는 데 사용 되는 특성입니다. 그러나 XAML 파서를 수행 합니다 `typeof` 작업 자동으로 및 `x:Type` 이러한 경우 태그 확장 사용 되지 않습니다.

한 곳 위치 `x:Type` *은* 된 필요한를 `x:Array` 에 설명 된 태그 확장을 [다음 섹션](#array)합니다.

`x:Type` 메뉴 특정 형식의 개체에 각 메뉴 항목을 해당 하는 위치를 생성할 때 태그 확장 유용한 이기도 합니다. 연결할 수는 `Type` 각 메뉴 항목을 사용 하 여 개체 및 메뉴 항목이 선택 될 때 개체를 인스턴스화해야 합니다.

이것이 어떻게 탐색 메뉴에서 `MainPage` 에 **태그 확장** 프로그램 작동 합니다. **MainPage.xaml** 파일에는 `TableView` 각 `TextCell` 프로그램의 특정 페이지에 해당:

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

[![기본 페이지](consuming-images/mainpage-small.png "기본 페이지")](consuming-images/mainpage-large.png#lightbox "주 페이지")

각 `CommandParameter` 속성을 `x:Type` 다른 페이지 중 하나를 참조 하는 태그 확장 합니다. 합니다 `Command` 속성은 명명 된 속성에 바인딩할 `NavigateCommand`합니다. 이 속성에 정의 된는 `MainPage` 코드 숨김 파일:

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

`NavigateCommand` 속성은는 `Command` 형식의 인수를 사용 하 여 execute 명령이 구현 하는 개체 `Type` &mdash; 변수의 `CommandParameter`합니다. 메서드를 사용 하 여 `Activator.CreateInstance` 페이지를 인스턴스화하기 위해 다음 여기로 이동 하 고 있습니다. 생성자를 설정 하 여 완료 합니다 `BindingContext` 그러면 자신에 게 페이지의를 `Binding` 에서 `Command` 작동 하려면. 참조 된 [ **데이터 바인딩** ](~/xamarin-forms/app-fundamentals/data-binding/index.md) 문서 및 특히 [ **직속 상관** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) 이러한 종류의 코드에 대 한 자세한 문서.

합니다 **X:type 데모** 페이지를 사용 하 여 유사한 기술을 Xamarin.Forms 요소를 인스턴스화하고을 추가 하는 `StackLayout`합니다. XAML 파일을 처음에 세 개인으로 구성 됩니다 `Button` 요소 해당 `Command` 속성으로 설정 된 `Binding` 및 `CommandParameter` 속성이 세 Xamarin.Forms 뷰 형식으로 설정:

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

코드 숨김 파일을 정의 하 고 초기화 된 `CreateCommand` 속성:

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

메서드는 될 때 실행 되는 `Button` 눌러져 인수의 새 인스턴스를 만들고, 설정 해당 `VerticalOptions` 속성에 추가 `StackLayout`합니다. 세 가지 `Button` 요소가 동적으로 생성된 된 뷰를 사용 하 여 다음 페이지를 공유 합니다.

[![X:type 데모](consuming-images/typedemo-small.png "X:type 데모")](consuming-images/typedemo-large.png#lightbox "X:type 데모")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array 태그 확장명

`x:Array` 태그 확장을 사용 하면 태그 배열을 정의할 수 있습니다. 지원 되는 [ `ArrayExtension` ](xref:Xamarin.Forms.Xaml.ArrayExtension) 두 속성을 정의 하는 클래스:

- `Type` 형식의 `Type`, 배열에 있는 요소의 형식을 나타냅니다.
- `Items` 형식의 `IList`을 항목 자체의 컬렉션인 합니다. 콘텐츠 속성인 `ArrayExtension`합니다.

`x:Array` 중괄호로 나타나지 않으며 자체 태그 확장 합니다. 대신 `x:Array` 시작 및 끝 태그 항목 목록을 구분 합니다. 설정 된 `Type` 속성을는 `x:Type` 태그 확장 합니다.

합니다 **X:array 데모** 페이지를 사용 하는 방법을 보여 줍니다 `x:Array` 항목을 추가 하는 `ListView` 설정 하 여는 `ItemsSource` 배열에 대 한 속성:

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

합니다 `ViewCell` 간단한 만듭니다 `BoxView` 각 색 항목에 대 한 합니다.

[![X:array 데모](consuming-images/arraydemo-small.png "X:array 데모")](consuming-images/arraydemo-large.png#lightbox "X:array 데모")

개별을 지정 하는 방법은 여러 가지 `Color` 이 배열의 항목입니다. 사용할 수는 `x:Static` 태그 확장:

```xaml
<x:Static Member="Color.Blue" />
```

사용할 수 있습니다 또는 `StaticResource` 리소스 사전에서 색을 검색 하려면:

```xaml
<StaticResource Key="myColor" />
```

이 문서의 끝도 새 색 값을 만들고 사용자 지정 XAML 태그 확장을 볼 수 있습니다.

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

문자열이 나 숫자와 같은 일반적인 형식 배열을 정의할 때에 나열 된 태그를 사용 합니다 [ **생성자 인수 전달** ](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) 값을 구분 하는 문서입니다.

<a name="null" />

## <a name="xnull-markup-extension"></a>x:Null 태그 확장명

합니다 `x:Null` 태그 확장 되는 [ `NullExtension` ](xref:Xamarin.Forms.Xaml.NullExtension) 클래스입니다. 속성이 있으며 단순히 XAML에 해당 하는 C# [ `null` ](/dotnet/csharp/language-reference/keywords/null/) 키워드입니다.

`x:Null` 필요를 찾을 경우 반갑게도 존재 하는 태그 확장은 거의 필요 하 고 거의 사용 되지만 합니다.

합니다 **X:null 데모** 페이지 하나 시나리오를 보여 줍니다. 때 `x:Null` 편리할 수 있습니다. 암시적 정의 한다고 가정 `Style` 에 대 한 `Label` 포함 하는 `Setter` 설정 하는 `FontFamily` 속성을 플랫폼에 종속 된 제품군 이름:

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

중 하나에를 검색 한 다음 합니다 `Label` 요소를 암시적의 모든 속성 설정을 `Style` 제외 하 고는 `FontFamily`, 기본값을 합니다. 다른 정의할 수 있습니다 `Style` 간단 하지만 해당 용도 설정 합니다 `FontFamily` 특정 속성 `Label` 에 `x:Null`센터에서 설명한 것 처럼 `Label`합니다.

실행 중인 프로그램이 다음과 같습니다.

[![X:null 데모](consuming-images/nulldemo-small.png "X:null 데모")](consuming-images/nulldemo-large.png#lightbox "X:null 데모")

에 대 한 공지는 4 합니다 `Label` 요소를 가질 가운데 있지만 serif 글꼴을 `Label` 기본 굴림 글꼴에.

<a name="onplatform" />

## <a name="onplatform-markup-extension"></a>OnPlatform 태그 확장

`OnPlatform` 태그 확장을 사용 하면 플랫폼별 기준 UI 모양을 사용자 지정할 수 있습니다. 동일한 기능을 제공 합니다 [ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) 및 [ `On` ](xref:Xamarin.Forms.On) 클래스 하지만 보다 간결 하 게 표현 합니다.

합니다 `OnPlatform` 태그 확장 되는 [ `OnPlatformExtension` ](xref:Xamarin.Forms.Xaml.OnPlatformExtension) 다음 속성을 정의 하는 클래스:

- `Default` 형식의 `object`, 플랫폼을 나타내는 속성에 적용할 기본 값으로 설정 합니다.
- `Android` 형식의 `object`, Android에 적용할 값으로 설정 합니다.
- `GTK` 형식의 `object`, GTK 플랫폼에 적용할 값으로 설정 합니다.
- `iOS` 형식의 `object`, iOS에서 적용할 값으로 설정 합니다.
- `macOS` 형식의 `object`를 macOS에서 적용할 값으로 설정 합니다.
- `Tizen` 형식의 `object`, Tizen 플랫폼에 적용할 값으로 설정 합니다.
- `UWP` 형식의 `object`, 유니버설 Windows 플랫폼에 적용할 값으로 설정 합니다.
- `WPF` 형식의 `object`, Windows Presentation Foundation 플랫폼에 적용할 값으로 설정 합니다.
- `Converter` 형식의 `IValueConverter`, 설정 하는 `IValueConverter` 구현 합니다.
- `ConverterParameter` 형식의 `object`에 전달할 값으로 설정 하는 `IValueConverter` 구현 합니다.

> [!NOTE]
> XAML 파서에서 허용 합니다 [ `OnPlatformExtension` ](xref:Xamarin.Forms.Xaml.OnPlatformExtension) 로 축약할 수 클래스 `OnPlatform`합니다.

합니다 `Default` 속성의 콘텐츠 속성은 `OnPlatformExtension`합니다. 따라서 XAML 태그 식 중괄호를 사용 하 여 표현에서 제거할 수 있습니다는 `Default=` 첫 번째 인수는 제공 된 식의 일부입니다.

> [!IMPORTANT]
> XAML 파서를 사용 하는 속성에 올바른 형식의 값을 제공할 것는 예상 된 `OnPlatform` 태그 확장 합니다. 형식 변환이 필요한 경우는 `OnPlatform` Xamarin.Forms 제공한 기본 변환기를 사용 하 여 수행 하는 데 태그 확장 하려고 합니다. 그러나 이러한 경우 기본 변환기에서 수행할 수 없는 일부 형식 변환이 있습니다.는 `Converter` 속성에 설정할는 `IValueConverter` 구현 합니다.

합니다 **OnPlatform 데모** 페이지를 사용 하는 방법을 보여 줍니다는 `OnPlatform` 태그 확장:

```xaml
<BoxView Color="{OnPlatform Yellow, iOS=Red, Android=Green, UWP=Blue}"
         WidthRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"  
         HeightRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"
         HorizontalOptions="Center" />
```

이 예제에서는 모든 세 `OnPlatform` 식의 약식된 버전을 사용 합니다 `OnPlatformExtension` 클래스 이름입니다. 세 가지 `OnPlatform` 태그 확장 집합을 [ `Color` ](xref:Xamarin.Forms.BoxView.Color), [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest), 및 [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) 의 속성을 [ `BoxView` ](xref:Xamarin.Forms.BoxView) iOS, Android 및 UWP에서 서로 다른 값으로. 또한 태그 확장을 제거 하는 동안 지정 되지 않은 플랫폼에서 이러한 속성에 대 한 기본값을 제공 합니다 `Default=` 식의 일부입니다. 알림 설정 되는 태그 확장 속성은 쉼표로 구분 합니다.

실행 중인 프로그램이 다음과 같습니다.

[![OnPlatform 데모](consuming-images/onplatformdemo-small.png "OnPlatform 데모")](consuming-images/onplatformdemo-large.png#lightbox "OnPlatform 데모")

<a name="onidiom" />

## <a name="onidiom-markup-extension"></a>OnIdiom 태그 확장

`OnIdiom` 태그 확장을 사용 하면 응용 프로그램에서 실행 중인 장치의 관용구를 기반으로 하는 UI 모양을 사용자 지정할 수 있습니다. 지원 되는 [ `OnIdiomExtension` ](xref:Xamarin.Forms.Xaml.OnIdiomExtension) 다음 속성을 정의 하는 클래스:

- `Default` 형식의 `object`, 장치 관용구를 나타내는 속성에 적용할 기본 값으로 설정 합니다.
- `Phone` 형식의 `object`, 휴대폰에 적용할 값으로 설정 합니다.
- `Tablet` 형식의 `object`, 태블릿에 적용할 값으로 설정 합니다.
- `Desktop` 형식의 `object`, 데스크톱 플랫폼에 적용할 값으로 설정 합니다.
- `TV` 형식의 `object`, TV 플랫폼에 적용할 값으로 설정 합니다.
- `Watch` 형식의 `object`, 조사식 플랫폼에 적용할 값으로 설정 합니다.
- `Converter` 형식의 `IValueConverter`, 설정 하는 `IValueConverter` 구현 합니다.
- `ConverterParameter` 형식의 `object`에 전달할 값으로 설정 하는 `IValueConverter` 구현 합니다.

> [!NOTE]
> XAML 파서에서 허용 합니다 [ `OnIdiomExtension` ](xref:Xamarin.Forms.Xaml.OnIdiomExtension) 로 축약할 수 클래스 `OnIdiom`합니다.

합니다 `Default` 속성의 콘텐츠 속성은 `OnIdiomExtension`합니다. 따라서 XAML 태그 식 중괄호를 사용 하 여 표현에서 제거할 수 있습니다는 `Default=` 첫 번째 인수는 제공 된 식의 일부입니다.

> [!IMPORTANT]
> XAML 파서를 사용 하는 속성에 올바른 형식의 값을 제공할 것는 예상 된 `OnIdiom` 태그 확장 합니다. 형식 변환이 필요한 경우는 `OnIdiom` Xamarin.Forms 제공한 기본 변환기를 사용 하 여 수행 하는 데 태그 확장 하려고 합니다. 그러나 이러한 경우 기본 변환기에서 수행할 수 없는 일부 형식 변환이 있습니다.는 `Converter` 속성에 설정할는 `IValueConverter` 구현 합니다.

합니다 **OnIdiom 데모** 페이지를 사용 하는 방법을 보여 줍니다는 `OnIdiom` 태그 확장:

```xaml
<BoxView Color="{OnIdiom Yellow, Phone=Red, Tablet=Green, Desktop=Blue}"
         WidthRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HeightRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HorizontalOptions="Center" />
```

이 예제에서는 모든 세 `OnIdiom` 식의 약식된 버전을 사용 합니다 `OnIdiomExtension` 클래스 이름입니다. 세 가지 `OnIdiom` 태그 확장 집합을 [ `Color` ](xref:Xamarin.Forms.BoxView.Color), [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest), 및 [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) 의 속성을 [ `BoxView` ](xref:Xamarin.Forms.BoxView) 전화, 태블릿 및 데스크톱 관용구의 다른 값입니다. 또한 태그 확장을 제거 하는 동안 지정 되지 않은 코드에서 이러한 속성에 대 한 기본값을 제공 합니다 `Default=` 식의 일부입니다. 알림 설정 되는 태그 확장 속성은 쉼표로 구분 합니다.

실행 중인 프로그램이 다음과 같습니다.

[![OnIdiom 데모](consuming-images/onidiomdemo-small.png "OnIdiom 데모")](consuming-images/onidiomdemo-large.png#lightbox "OnIdiom 데모")

## <a name="define-your-own-markup-extensions"></a>사용자 고유의 태그 확장을 정의 합니다.

Xamarin.Forms에서 사용할 수 없는 XAML 태그 확장에 대 한 필요를 알리는 경우 할 수 있습니다 [직접 만들어보십시오](creating.md)합니다.

## <a name="related-links"></a>관련 링크

- [태그 확장 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Xamarin.Forms 책에서 XAML 태그 확장 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)
- [동적 스타일](~/xamarin-forms/user-interface/styles/dynamic.md)
- [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)
