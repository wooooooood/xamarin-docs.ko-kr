---
title: XAML 태그 확장 만들기
description: 이 문서에서는 사용자 고유의 사용자 지정 Xamarin.Forms XAML 태그 확장을 정의하는 방법을 설명합니다. XAML 태그 확장은 IMarkupExtension 인터페이스를 구현하는 클래스입니다.
ms.prod: xamarin
ms.assetid: 797C1EF9-1C8E-4208-8610-9B79CCF17D46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 531fb9500bdbf9d07ac3f781113768395465bd50
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53050558"
---
# <a name="creating-xaml-markup-extensions"></a>XAML 태그 확장 만들기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)

프로그래밍 방식 수준에서 XAML 태그 확장은 [ `IMarkupExtension` ](xref:Xamarin.Forms.Xaml.IMarkupExtension) 또는 [ `IMarkupExtension<T>` ](xref:Xamarin.Forms.Xaml.IMarkupExtension`1) 인터페이스를 구현하는 클래스입니다. 아래에 설명 된 Xamarin.Forms GitHub 리포지토리의 [ **MarkupExtensions** 디렉토리](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions)에서 표준 태그 확장의 소스 코드를 살펴볼 수 있습니다.

또한 `IMarkupExtension` 또는 `IMarkupExtension<T>`를 파생하여 사용자 고유의 사용자 지정 XAML 태그 확장을 정의할 수 있습니다. 태그 확장이 특정 유형의 값을 얻는 경우 제네릭 형식을 사용 하십시오. Xamarin.Forms 태그 확장을 사용하는 몇 가지 경우는 다음과 같습니다.

- `IMarkupExtension<Type>`에서 파생하는 `TypeExtension`
- `IMarkupExtension<Array>`에서 파생하는 `ArrayExtension`
- `IMarkupExtension<DynamicResource>`에서 파생하는 `DynamicResourceExtension`
- `IMarkupExtension<BindingBase>`에서 파생하는 `BindingExtension`
- `IMarkupExtension<Constraint>`에서 파생하는 `ConstraintExpression`

두 개의 `IMarkupExtension` 인터페이스는 다음과 같이 각각 `ProvideValue`라는 하나의 메서드만 정의합니다.

```csharp
public interface IMarkupExtension
{
    object ProvideValue(IServiceProvider serviceProvider);
}

public interface IMarkupExtension<out T> : IMarkupExtension
{
    new T ProvideValue(IServiceProvider serviceProvider);
}
```

`IMarkupExtension<T>`는 `IMarkupExtension`에서 파생되고 `ProvideValue`에 `new` 키워드를 포함하고 있으며, 모두 `ProvideValue` 메서드를 포함합니다.

많은 경우 XAML 태그 확장은 반환 값에 영향을 주는 속성을 정의합니다. (명백한 예외는 `ProvideValue`가 단순히 `null`을 반환하는 `NullExtension` 입니다.) `ProvideValue` 메서드는 이 문서의 뒷부분에서 논의 될 `IServiceProvider` 유형의 단일 인수를 가집니다.

## <a name="a-markup-extension-for-specifying-color"></a>색상을 지정하는 태그 확장

다음 XAML 태그 확장은 색상, 채도 및 명도 구성 요소를 사용하여 `Color` 값을 구성할 수 있습니다. 해당 클래스는 1로 초기화 되는 알파 구성 요소를 포함하여 색상의 네 가지 구성 요소에 대한 네 가지 속성을 정의합니다. 해당 클래스는 `Color` 반환 값을 나타내기 위해 `IMarkupExtension<Color>`에서 파생됩니다.

```csharp
public class HslColorExtension : IMarkupExtension<Color>
{
    public double H { set; get; }

    public double S { set; get; }

    public double L { set; get; }

    public double A { set; get; } = 1.0;

    public Color ProvideValue(IServiceProvider serviceProvider)
    {
        return Color.FromHsla(H, S, L, A);
    }

    object IMarkupExtension.ProvideValue(IServiceProvider serviceProvider)
    {
        return (this as IMarkupExtension<Color>).ProvideValue(serviceProvider);
    }
}
```

`IMarkupExtension<T>`는 `IMarkupExtension`에서 파생 되므로, 클래스에는 두 개의 `ProvideValue` 메서드를 포함해야 하고, 하나는 `Color`를 반환하고 다른 하나는 `object`를 반환 하지만, 두 번째 메서드는 첫 번째 메서드를 단순히 호출만 하면 됩니다.

다음의 **HSL Color Demo** 페이지는 XAML 파일에서 `BoxView`의 색상을 지정하여 표시할 수 있는 `HslColorExtension`의 다양한 방법을 보여줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.HslColorDemoPage"
             Title="HSL Color Demo">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="BoxView">
                <Setter Property="WidthRequest" Value="80" />
                <Setter Property="HeightRequest" Value="80" />
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <BoxView>
            <BoxView.Color>
                <local:HslColorExtension H="0" S="1" L="0.5" A="1" />
            </BoxView.Color>
        </BoxView>

        <BoxView>
            <BoxView.Color>
                <local:HslColor H="0.33" S="1" L="0.5" />
            </BoxView.Color>
        </BoxView>

        <BoxView Color="{local:HslColorExtension H=0.67, S=1, L=0.5}" />

        <BoxView Color="{local:HslColor H=0, S=0, L=0.5}" />

        <BoxView Color="{local:HslColor A=0.5}" />
    </StackLayout>
</ContentPage>
```

`HslColorExtension`이 XML 태그일 때 네 가지 속성은 특성으로 설정 되지만, 중괄호 사이에 나타날 때는 네 가지 속성은 따옴표 없이 쉼표로 구분합니다. `H`, `S` 및 `L`에 대한 기본값은 0 이고, `A`에 대한 기본값은 1 이므로, 기본값으로 설정 하려는 경우 해당 속성을 생략할 수 있습니다. 마지막 예제에서는 일반적으로 검정색 결과인 명도가 0 이지만, 알파 채널 0.5 이므로 반 투명하고 페이지가 흰색 배경인 데 비해 회색으로 표시되는 예를 보여줍니다.

[![HSL Color Demo](creating-images/hslcolordemo-small.png "HSL Color Demo")](creating-images/hslcolordemo-large.png#lightbox "HSL Color Demo")

## <a name="a-markup-extension-for-accessing-bitmaps"></a>비트맵에 접근하는 태그 확장

`ProvideValue`에 대한 인수는 .NET `System` 네임 스페이스에 정의된 [ `IServiceProvider` ](xref:System.IServiceProvider) 인터페이스를 구현하는 개체입니다. 해당 인터페이스에는 `Type` 인수를 가진 `GetService`라는 메소드가 있습니다.

아래 보이는 `ImageResourceExtension` 클래스는 `IServiceProvider`와 `GetService`를 사용하여 특정 오류가 발견된 곳을 가리키는 줄과 문자 정보를 제공할 수 있는 `IXmlLineInfoProvider` 개체를 얻을 수 있습니다. 이 경우 `Source` 속성이 설정되지 않으면 예외가 발생합니다.

```csharp
[ContentProperty("Source")]
class ImageResourceExtension : IMarkupExtension<ImageSource>
{
    public string Source { set; get; }

    public ImageSource ProvideValue(IServiceProvider serviceProvider)
    {
        if (String.IsNullOrEmpty(Source))
        {
            IXmlLineInfoProvider lineInfoProvider = serviceProvider.GetService(typeof(IXmlLineInfoProvider)) as IXmlLineInfoProvider;
            IXmlLineInfo lineInfo = (lineInfoProvider != null) ? lineInfoProvider.XmlLineInfo : new XmlLineInfo();
            throw new XamlParseException("ImageResourceExtension requires Source property to be set", lineInfo);
        }

        string assemblyName = GetType().GetTypeInfo().Assembly.GetName().Name;
        return ImageSource.FromResource(assemblyName + "." + Source, typeof(ImageResourceExtension).GetTypeInfo().Assembly);
    }

    object IMarkupExtension.ProvideValue(IServiceProvider serviceProvider)
    {
        return (this as IMarkupExtension<ImageSource>).ProvideValue(serviceProvider);
    }
}
```

`ImageResourceExtension`은 XAML 파일이 .NET Standard 라이브러리 프로젝트의 포함 리소스로 저장된 이미지 파일에 접근해야 할 때 유용합니다. 해당 클래스는 `Source` 속성을 사용하여 정적 `ImageSource.FromResource` 메서드를 호출합니다. 해당 메서드는 어셈블리 이름, 폴더 이름 및 마침표로 구분된 파일 이름으로 구성된 정규화된 리소스 이름이 필요합니다. `ImageSource.FromResource` 메서드에 대한 두 번째 인수는 어셈블리 이름을 제공하며 UWP의 릴리스 빌드에만 필요합니다. 그럼에도 불구하고 비트맵을 포함하는 어셈블리에서 `ImageSource.FromResource`를 호출해야 합니다. 즉, XAML 리소스 확장은 이미지가 해당 라이브러리에도 없는 경우 외부 라이브러리의 일부로 될 수 없습니다. (포함 리소스로 저장된 비트맵에 접근하는 좀 더 자세한 정보는 [ **포함 된 이미지가** ](~/xamarin-forms/user-interface/images.md#embedded-images) 문서를 참고하십시오.)

`ImageResourceExtension`은 `Source` 속성 설정이 필요하지만, `Source` 속성은 특성에서 클래스의 컨텐츠 속성으로 표시됩니다. 즉, 중괄호 안에 있는 표현식의 `Source=` 부분을 생략할 수 있습니다. 다음의 **Image Resource Demo** 페이지에서 `Image` 요소는 폴더 이름을 마침표로 구분하여 두 개의 이미지를 가져옵니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.ImageResourceDemoPage"
             Title="Image Resource Demo">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Image Source="{local:ImageResource Images.SeatedMonkey.jpg}"
               Grid.Row="0" />

        <Image Source="{local:ImageResource Images.FacePalm.jpg}"
               Grid.Row="1" />

    </Grid>
</ContentPage>
```

실행 중인 프로그램은 다음과 같습니다.

[![Image Resource Demo](creating-images/imageresourcedemo-small.png "Image Resource Demo")](creating-images/imageresourcedemo-large.png#lightbox "Image Resource Demo")

## <a name="service-providers"></a>서비스 공급자

`ProvideValue`에 대한 `IServiceProvider` 인수를 사용하면, XAML 태그 확장에서 해당 태그가 사용되는 XAML 파일에 대한 유용한 정보에 접근할 수 있습니다. 하지만 `IServiceProvider` 인수를 성공적으로 사용하려면 특정 상황에서 어떤 종류의 서비스가 사용 가능한지 알아야 합니다. 해당 기능을 이해하는 가장 좋은 방법은 GitHub의 Xamarin.Forms 리포지토리에서 [ **MarkupExtensions** 폴더](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions)에 있는 기존 XAML 태그 확장의 소스 코드를 공부하는 것입니다. 몇 가지 종류의 서비스는 Xamarin.Forms 내부에 있다는 것에 주의 합니다.

일부 XAML 태그 확장에서 해당 서비스는 유용할 수 있습니다.

```csharp
 IProvideValueTarget provideValueTarget = serviceProvider.GetService(typeof(IProvideValueTarget)) as IProvideValueTarget;
```

`IProvideValueTarget` 인터페이스는 두 개의 속성인 `TargetObject`와 `TargetProperty`를 정의합니다. 해당 정보를 `ImageResourceExtension` 클래스에서 가져오면, `TargetObject`는 `Image`이고 `TargetProperty`는 `Image`의 `Source` 속성을 위한 `BindableProperty` 개체입니다. 해당 개체는 XAML 태그 확장이 설정된 속성입니다.

`typeof(IProvideValueTarget)` 인수를 갖는 `GetService` 호출은 `Xamarin.Forms.Xaml.Internals` 네임 스페이스에 정의된 `SimpleValueTargetProvider` 유형의 개체를 실제로 반환합니다. `GetService`의 반환 값을 해당 유형으로 변환하면 `Image` 요소, `Grid` 부모 및 `Grid`의 `ImageResourceDemoPage` 부모를 포함하는 배열인 `ParentObjects` 속성에 접근할 수 있습니다.

## <a name="conclusion"></a>결론

XAML 태그 확장은 다양한 원본의 특성을 설정할 수 있는 기능을 확장하여 XAML에서 중요한 역할을 합니다. 또한 기존 XAML 태그 확장이 필요한 것을 정확하게 제공하지 못하는 경우 사용자가 직접 작성할 수도 경우 기록도 고유한 합니다.

## <a name="related-links"></a>관련 링크

- [태그 확장 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Xamarin.Forms 책에서 XAML 태그 확장 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
