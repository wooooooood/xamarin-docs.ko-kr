---
title: XAML 태그 확장명 만들기
description: 이 문서에서는 사용자 고유의 사용자 지정 하는 Xamarin.Forms XAML 태그 확장을 정의 하는 방법을 설명 합니다. XAML 태그 확장은 IMarkupExtension IMarkupExtension 인터페이스를 구현 하는 클래스입니다.
ms.prod: xamarin
ms.assetid: 797C1EF9-1C8E-4208-8610-9B79CCF17D46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: b928c55f447d68b8adfedaa031fd85750ee71267
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563708"
---
# <a name="creating-xaml-markup-extensions"></a>XAML 태그 확장명 만들기

프로그래밍 방식으로 수준에서 XAML 태그 확장은 구현 하는 클래스를 [ `IMarkupExtension` ](xref:Xamarin.Forms.Xaml.IMarkupExtension) 하거나 [ `IMarkupExtension<T>` ](xref:Xamarin.Forms.Xaml.IMarkupExtension`1) 인터페이스입니다. 아래에 설명 된 표준 태그 확장의 소스 코드를 탐색할 수 있습니다 합니다 [ **MarkupExtensions** directory](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) Xamarin.Forms GitHub 리포지토리.

파생 하 여 사용자 고유의 사용자 지정 XAML 태그 확장을 정의할 수 이기도 `IMarkupExtension` 또는 `IMarkupExtension<T>`합니다. 태그 확장 특정 형식의 값을 가져오는 경우 일반 양식은 사용 합니다. 다양 한 Xamarin.Forms 태그 확장을 사용 하는 경우 다음과 같습니다.

- `TypeExtension` 파생 `IMarkupExtension<Type>`
- `ArrayExtension` 파생 `IMarkupExtension<Array>`
- `DynamicResourceExtension` 파생 `IMarkupExtension<DynamicResource>`
- `BindingExtension` 파생 `IMarkupExtension<BindingBase>`
- `ConstraintExpression` 파생 `IMarkupExtension<Constraint>`

두 개의 `IMarkupExtension` 각각 하나의 메서드를 정의 하는 인터페이스 이름이 `ProvideValue`:

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

하므로 `IMarkupExtension<T>` 에서 파생 되 `IMarkupExtension` 포함 합니다 `new` 키워드 `ProvideValue`를 모두 포함 되어 `ProvideValue` 메서드.

많은 경우 XAML 태그 확장 반환 값에 영향을 주는 속성을 정의 합니다. (은 예외임 `NullExtension`에는 `ProvideValue` 반환 하기만 `null`.) 합니다 `ProvideValue` 메서드는 형식의 단일 인수 `IServiceProvider` 는이 문서의 뒷부분에서 설명 됩니다.

## <a name="a-markup-extension-for-specifying-color"></a>색을 지정 하는 것에 대 한 태그 확장

다음 XAML 태그 확장을 사용 하면 생성 하는 `Color` 색상, 채도 및 명도 구성 요소를 사용 하 여 값입니다. 1로 초기화 되는 알파 구성 요소를 포함 하 여 색의 네 가지 구성 요소에 대 한 네 가지 속성을 정의 합니다. 클래스에서 파생 `IMarkupExtension<Color>` 나타내려면는 `Color` 값을 반환 합니다.

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

때문에 `IMarkupExtension<T>` 에서 파생 되 `IMarkupExtension`, 두 개의 클래스를 포함 해야 합니다 `ProvideValue` 메서드를 반환 하는 것 `Color` 반환 하 고 다른 `object`, 하지만 두 번째 방법은 첫 번째 메서드를 호출 하기만 하면 됩니다.

합니다 **HSL 색 데모** 페이지에 따르면 다양 한 방법으로 `HslColorExtension` 색을 지정 하는 XAML 파일에 나타날 수는 `BoxView`:

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

때 `HslColorExtension` XML 태그가, 네 가지 속성은 특성으로 설정 되지만 중괄호 사이 나타날 때 네 가지 속성은 쉼표로 구분 하 여 따옴표 없이 합니다. 기본값을 `H`, `S`, 및 `L` 는 0이 고 기본값은 `A` 1 이므로 기본값으로 설정 하려는 경우 해당 속성을 생략할 수 있습니다. 마지막 예제에서는 여기서 명도 검정색에서 결과 일반적으로 0 이지만 알파 채널 0.5 반 투명 하 고 표시 되는 예를 보여 줍니다. 페이지의 흰색 배경에 회색:

[![HSL 색 데모](creating-images/hslcolordemo-small.png "HSL 색 데모")](creating-images/hslcolordemo-large.png#lightbox "HSL 색 데모")

## <a name="a-markup-extension-for-accessing-bitmaps"></a>비트맵에 액세스 하기 위한 태그 확장

인수 `ProvideValue` 를 구현 하는 개체를 [ `IServiceProvider` ](xref:System.IServiceProvider) .NET에 정의 된 인터페이스 `System` 네임 스페이스입니다. 이 인터페이스에는 하나의 멤버, 명명 된 메서드 `GetService` 사용 하 여는 `Type` 인수입니다.

`ImageResourceExtension` 아래에 표시 된 클래스의 한 가지 가능한 용도 보여 줍니다. `IServiceProvider` 하 고 `GetService` 가져오려고는 `IXmlLineInfoProvider` 나타내는 특정 오류를 발견 된 줄과 문자 정보를 제공할 수 있는 개체입니다. 이 경우 예외가 발생 하는 경우는 `Source` 속성이 설정 되지 않았습니다:

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

`ImageResourceExtension` XAML 파일을.NET Standard 라이브러리 프로젝트에 포함 리소스로 저장 된 이미지 파일에 액세스 해야 하는 경우 유용 합니다. 사용 된 `Source` 정적 호출 하는 속성 `ImageSource.FromResource` 메서드. 이 메서드는 어셈블리 이름, 폴더, 이름과 마침표로 구분 된 파일 이름으로 구성 된 정규화 된 리소스 이름에 필요 합니다. 합니다 `ImageResourceExtension` 하지 필요한 어셈블리의 이름 부분 앞에 추가 하 고 리플렉션을 사용 하 여 어셈블리 이름을 가져옵니다 있기 때문에 `Source` 속성입니다. 그럼에도 불구 하 고 `ImageSource.FromResource` 해당 라이브러리에서 이미지도 하지 않는 한이 XAML 리소스 확장에서 외부 라이브러리 포함 될 수 없습니다는 즉 비트맵을 포함 하는 어셈블리에서 호출 되어야 합니다. (참조를 [ **포함 된 이미지가** ](~/xamarin-forms/user-interface/images.md#embedded-images) 비트맵을 포함 리소스로 저장 된 액세스에 대 한 자세한 내용은 문서입니다.)

하지만 `ImageResourceExtension` 필요 합니다 `Source` 속성을 설정할 수는 `Source` 속성은 특성에서 클래스의 content 속성으로 표시 됩니다. 즉는 `Source=` 중괄호 안에 있는 식의 부분을 생략할 수 있습니다. 에 **이미지 리소스 데모** 페이지를 `Image` 요소 인출 폴더 이름과 마침표로 구분 된 파일 이름을 사용 하 여 두 개의 이미지:

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

다음은 세 플랫폼 모두에서 실행 중인 프로그램이입니다.

[![이미지 리소스 데모](creating-images/imageresourcedemo-small.png "이미지 리소스 데모")](creating-images/imageresourcedemo-large.png#lightbox "리소스 데모 이미지")

## <a name="service-providers"></a>서비스 공급자

사용 하 여 합니다 `IServiceProvider` 인수를 `ProvideValue`, XAML 태그 확장 사용 되는 XAML 파일에 대 한 유용한 정보를 얻을 수 있습니다. 하지만 사용 하 여 `IServiceProvider` 인수 성공적으로 알아야 할 어떤 종류의 서비스는 특정 컨텍스트에서 사용할 수 있습니다. 기존 XAML 태그 확장의 소스 코드를 조사 하 여이 기능을 이해 하는 가장 좋은 방법은는 [ **MarkupExtensions** 폴더](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) Xamarin.Forms 리포지토리의 GitHub에서. 일부 종류의 서비스는 내부 Xamarin.Forms에 주의 합니다.

일부 XAML 태그 확장에서이 서비스는 유용할 수 있습니다.

```csharp
 IProvideValueTarget provideValueTarget = serviceProvider.GetService(typeof(IProvideValueTarget)) as IProvideValueTarget;
```

합니다 `IProvideValueTarget` 인터페이스는 두 개의 속성 정의 `TargetObject` 및 `TargetProperty`합니다. 이 정보를 가져옵니다 하는 경우는 `ImageResourceExtension` 클래스 `TargetObject` 는 `Image` 및 `TargetProperty` 는 `BindableProperty` 개체에 대 한 합니다 `Source` 속성 `Image`. XAML 태그 확장에 설정 된 속성입니다.

`GetService` 의 인수와 함께 호출 `typeof(IProvideValueTarget)` 실제로 형식의 개체를 반환 `SimpleValueTargetProvider`에 정의 되어 있는 `Xamarin.Forms.Xaml.Internals` 네임 스페이스입니다. 반환 값을 캐스팅 하는 경우 `GetService` 해당 형식에 액세스할 수도 있습니다는 `ParentObjects` 속성을 포함 하는 배열을 `Image` 요소를 `Grid` 부모 및 `ImageResourceDemoPage` 의 부모는 `Grid`합니다.

## <a name="conclusion"></a>결론

다양 한 원본에서에서 특성을 설정 하는 기능을 확장 하 여 XAML에서 중요 한 역할을 재생 하는 XAML 태그 확장 합니다. 또한 기존 XAML 태그 확장은 필요한 것만 제공 하지 않습니다, 경우 기록도 고유한 합니다.


## <a name="related-links"></a>관련 링크

- [태그 확장 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Xamarin.Forms 책에서 XAML 태그 확장 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
