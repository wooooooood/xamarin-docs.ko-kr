---
title: XAML 태그 확장명 만들기
description: 사용자 고유의 사용자 지정 XAML 태그 확장을 정의 합니다.
ms.prod: xamarin
ms.assetid: 797C1EF9-1C8E-4208-8610-9B79CCF17D46
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: d4ae3b42c5c926749310da6e36b6f4e9754d398c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="creating-xaml-markup-extensions"></a>XAML 태그 확장명 만들기

프로그래밍 방식으로 수준에서 XAML 태그 확장은 구현 하는 클래스는 [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/) 또는 [ `IMarkupExtension<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension%3CT%3E/) 인터페이스입니다. 아래에 설명 된 표준 태그 확장의 소스 코드를 탐색할 수 있습니다는 [ **MarkupExtensions** 디렉터리](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) Xamarin.Forms GitHub 리포지토리의 합니다. 

파생 하 여 사용자 고유의 사용자 지정 XAML 태그 확장을 정의할 수 이기도 `IMarkupExtension` 또는 `IMarkupExtension<T>`합니다. 일반 양식을 사용 하면 특정 형식의 값을 가져오면 태그 확장 사용 합니다. 이 여러 가지 Xamarin.Forms 태그 확장의 경우:

- `TypeExtension` 파생 `IMarkupExtension<Type>`
- `ArrayExtension` 파생 `IMarkupExtension<Array>`
- `DynamicResourceExtension` 파생 `IMarkupExtension<DynamicResource>`
- `BindingExtension` 파생 `IMarkupExtension<BindingBase>`
- `ConstraintExpression` 파생 `IMarkupExtension<Constraint>`

두 `IMarkupExtension` 각각 하나의 메서드를 정의 하는 인터페이스 이름이 `ProvideValue`: 

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

이후 `IMarkupExtension<T>` 에서 파생 `IMarkupExtension` 작성과 `new` 키워드 `ProvideValue`, 둘 다 포함 되어 `ProvideValue` 메서드.

대개 XAML 태그 확장은 반환 값에 영향을 주는 속성을 정의 합니다. (확실 한 예외는 `NullExtension`는 `ProvideValue` 단순히 반환 `null`.) `ProvideValue` 메서드는 형식의 단일 인수 `IServiceProvider` 이 문서의 뒷부분에 나오는 설명입니다.

## <a name="a-markup-extension-for-specifying-color"></a>색을 지정 하는 것에 대 한 태그 확장

다음 XAML 태그 확장을 사용 하면 생성 하는 `Color` 색상, 채도 및 명도 구성 요소를 사용 하 여 값입니다. 1로 초기화 되는 알파 구성 요소를 포함 하 여 색의 네 가지 구성 요소에 대 한 네 가지 속성을 정의 합니다. 클래스에서 파생 `IMarkupExtension<Color>` 나타내기 위해는 `Color` 값을 반환 합니다.

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

때문에 `IMarkupExtension<T>` 에서 파생 `IMarkupExtension`, 클래스 두 개를 포함 해야 합니다 `ProvideValue` 메서드, 반환 된 `Color` 반환 하는 쿼리와 `object`만 두 번째 방법은 첫 번째 메서드를 호출 하기만 하면 수 있습니다.

**HSL 색 데모** 페이지 표시 다양 한 방법으로 `HslColorExtension` 에 대 한 색을 지정 하려면 XAML 파일에 나타날 수는 `BoxView`:

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

때 `HslColorExtension` XML 태그 인지, 특성으로, 다음 네 가지 속성은 설정 되지만 네 가지 속성은 따옴표 없이 쉼표로 구분 중괄호 사이의 있으면 오류가 발생 합니다. 기본값을 `H`, `S`, 및 `L` 0이 고의 기본값은 `A` 1 이므로 기본값으로 설정 하려는 경우 해당 속성을 생략할 수 있습니다. 마지막 예제에서는 명도 검은색으로 인해 일반적으로 0 이지만 알파 채널은 0.5, 절반 투명 한 표시 되므로 예를 보여 줍니다. 페이지의 흰색 배경 회색:

[![HSL 색 데모](creating-images/hslcolordemo-small.png "HSL 색 데모")](creating-images/hslcolordemo-large.png#lightbox "HSL 색 데모")

## <a name="a-markup-extension-for-accessing-bitmaps"></a>비트맵에 액세스 하기 위한 태그 확장

에 대 한 인수 `ProvideValue` 구현 하는 개체는 [ `IServiceProvider` ](https://developer.xamarin.com/api/type/System.IServiceProvider/) .NET에 정의 된 인터페이스 `System` 네임 스페이스입니다. 이 인터페이스에 멤버를 라는 메서드 하나 `GetService` 와 `Type` 인수입니다. 

`ImageResourceExtension` 아래에 표시 된 클래스의 가능한 용도 중 하나를 보여 줍니다. `IServiceProvider` 및 `GetService` 얻으려고는 `IXmlLineInfoProvider` 특정 오류 검색 되었습니다 나타내는 줄과 문자 정보를 제공할 수 있는 개체입니다. 이 경우 예외가 발생 하는 경우는 `Source` 속성이 설정 되지 않았습니다:

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

        return ImageSource.FromResource(assemblyName + "." + Source);
    }

    object IMarkupExtension.ProvideValue(IServiceProvider serviceProvider)
    {
        return (this as IMarkupExtension<ImageSource>).ProvideValue(serviceProvider);
    }
}
```

`ImageResourceExtension` XAML 파일에 이식 가능한 클래스 라이브러리 프로젝트에 포함 리소스로 저장 된 이미지 파일을 액세스 해야 할 때 유용 합니다. 사용 하 여는 `Source` 정적을 호출 하는 속성 `ImageSource.FromResource` 메서드. 이 메서드는 어셈블리 이름, 폴더 이름 및 마침표로 구분 된 파일 이름으로 구성 되는 정규화 된 리소스 이름이 필요 합니다. `ImageResourceExtension` 하지 않는 필요한 어셈블리의 이름 부분 앞에 추가 하 고 리플렉션을 사용 하 여 어셈블리 이름을 가져옵니다 있기 때문에 `Source` 속성입니다. 그럼에도 불구 하 고 `ImageSource.FromResource` 즉,이 XAML 리소스 확장 없습니다 외부 라이브러리의 일부 이미지가 아닌 해당 라이브러리에도 이상 비트맵을 포함 하는 어셈블리에서 호출 되어야 합니다. (참조는 [ **포함 이미지** ](~/xamarin-forms/user-interface/images.md#embedded_images) 비트맵 포함 리소스로 저장 된 액세스에 대 한 자세한 내용은 문서입니다.) 

하지만 `ImageResourceExtension` 필요는 `Source` 속성을 설정할 수는 `Source` 클래스의 콘텐츠 속성으로 속성 특성에 표시 됩니다. 즉는 `Source=` 중괄호 안에 있는 식의 부분을 생략할 수 있습니다. 에 **이미지 리소스 데모** 페이지는 `Image` 요소 인출 폴더 이름과 마침표로 구분 된 파일 이름을 사용 하 여 두 가지 이미지:

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

다음은 세 플랫폼 모두에서 실행 중인 프로그램입니다.

[![이미지 리소스 데모](creating-images/imageresourcedemo-small.png "이미지 리소스 데모")](creating-images/imageresourcedemo-large.png#lightbox "리소스 데모 이미지")

## <a name="service-providers"></a>서비스 공급자

사용 하 여는 `IServiceProvider` 인수 `ProvideValue`, XAML 태그 확장 사용 되는 XAML 파일에 대 한 유용한 정보에 대 한 액세스를 얻을 수 있습니다. 하지만 사용 하는 `IServiceProvider` 인수 성공적으로 필요한 서비스의 종류를 특정 컨텍스트에서 사용할 수 있습니다. 기존 XAML 태그 확장의 소스 코드를 조사 하 여이 기능을 이해 하는 가장 좋은 방법은는 [ **MarkupExtensions** 폴더](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) github Xamarin.Forms 리포지토리에 합니다. 일부 종류의 서비스 내부 Xamarin.Forms에는 수입니다.

일부 XAML 태그 확장에서이 서비스는 유용할 수 있습니다.

```csharp
 IProvideValueTarget provideValueTarget = serviceProvider.GetService(typeof(IProvideValueTarget)) as IProvideValueTarget;
```

`IProvideValueTarget` 두 속성을 정의 하는 인터페이스 `TargetObject` 및 `TargetProperty`합니다. 이 정보는에서 얻은 `ImageResourceExtension` 클래스 `TargetObject` 는 `Image` 및 `TargetProperty` 는 `BindableProperty` 개체에 대 한는 `Source` 속성 `Image`합니다. XAML 태그 확장을 설정한 속성입니다.

`GetService` 호출의 인수와 함께 `typeof(IProvideValueTarget)` 실제로 형식의 개체를 반환 `SimpleValueTargetProvider`에 정의 된는 `Xamarin.Forms.Xaml.Internals` 네임 스페이스입니다. 반환 값을 캐스팅 하는 경우 `GetService` 해당 형식에 액세스할 수도 있습니다는 `ParentObjects` 속성을 포함 하는 배열을 `Image` 요소는 `Grid` 부모 및 `ImageResourceDemoPage` 의 부모는 `Grid`합니다.

## <a name="conclusion"></a>결론

XAML 태그 확장 다양 한 소스에서에서 특성을 설정 하는 기능을 확장 하 여 XAML에서 중요 한 역할을 재생 합니다. 또한 기존 XAML 태그 확장은 필요한 정보가 제공 하지를 작성할 수도 있습니다 직접 합니다. 


## <a name="related-links"></a>관련 링크

- [태그 확장 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Xamarin.Forms 책에서 XAML 태그 확장 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
