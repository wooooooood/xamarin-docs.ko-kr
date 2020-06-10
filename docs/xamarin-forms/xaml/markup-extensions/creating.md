---
제목: "XAML 태그 확장 만들기" 설명: "이 문서에서는 사용자 지정 xaml 태그 확장을 정의 하는 방법을 설명 Xamarin.Forms 합니다. XAML 태그 확장은 IMarkupExtension 또는 IMarkupExtension 인터페이스를 구현 하는 클래스입니다 <T> . "
assetid: 797C1EF9-1C8E-4208-8610-9B79CCF17D46: xamarin-forms author: davidbritch: dabritch:: 01/05/2018-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="creating-xaml-markup-extensions"></a>XAML 태그 확장 만들기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

프로그래밍 수준에서 XAML 태그 확장은 또는 인터페이스를 구현 하는 클래스입니다 [`IMarkupExtension`](xref:Xamarin.Forms.Xaml.IMarkupExtension) [`IMarkupExtension<T>`](xref:Xamarin.Forms.Xaml.IMarkupExtension`1) . GitHub 리포지토리의 [ **MarkupExtensions** 디렉터리](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) 에서 아래에 설명 된 표준 태그 확장의 소스 코드를 탐색할 수 있습니다 Xamarin.Forms .

또는에서 파생 하 여 사용자 고유의 사용자 지정 XAML 태그 확장을 정의할 수도 `IMarkupExtension` 있습니다 `IMarkupExtension<T>` . 태그 확장이 특정 형식의 값을 가져오는 경우 일반 폼을 사용 합니다. 다음은 몇 가지 태그 확장을 사용 하는 경우입니다 Xamarin.Forms .

- `TypeExtension`는 `IMarkupExtension<Type>`에서 파생됩니다.
- `ArrayExtension`는 `IMarkupExtension<Array>`에서 파생됩니다.
- `DynamicResourceExtension`는 `IMarkupExtension<DynamicResource>`에서 파생됩니다.
- `BindingExtension`는 `IMarkupExtension<BindingBase>`에서 파생됩니다.
- `ConstraintExpression`는 `IMarkupExtension<Constraint>`에서 파생됩니다.

두 `IMarkupExtension` 인터페이스는 각각 하나의 메서드만 정의 합니다 `ProvideValue` .

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

`IMarkupExtension<T>`는에서 파생 되 `IMarkupExtension` 고에 키워드를 포함 하므로 `new` `ProvideValue` 두 메서드를 모두 포함 `ProvideValue` 합니다.

XAML 태그 확장은 반환 값에 영향을 주는 속성을 정의 하는 경우가 많습니다. 분명 한 예외는 이며 `NullExtension` , 여기서는를 `ProvideValue` 반환 `null` 합니다. `ProvideValue`메서드에는 `IServiceProvider` 이 문서의 뒷부분에서 설명 하는 형식의 단일 인수가 있습니다.

## <a name="a-markup-extension-for-specifying-color"></a>색을 지정 하기 위한 태그 확장

다음 XAML 태그 확장을 사용 하면 `Color` 색상, 채도 및 광도 구성 요소를 사용 하 여 값을 생성할 수 있습니다. 1로 초기화 된 알파 구성 요소를 포함 하 여 네 가지 색 구성 요소에 대 한 네 가지 속성을 정의 합니다. 클래스는 `IMarkupExtension<Color>` 반환 값을 나타내기 위해에서 파생 됩니다 `Color` .

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

는 `IMarkupExtension<T>` 에서 파생 되기 때문에 `IMarkupExtension` 클래스에는 두 개의 메서드가 포함 되어야 합니다 `ProvideValue` . 하나는을 반환 하 고 다른 하나는 `Color` `object` 를 반환 하지만 두 번째 메서드는 단순히 첫 번째 메서드를 호출할 수 있습니다.

**HSL 색 데모** 페이지에 `HslColorExtension` 는 XAML 파일에 표시 될 수 있는 다양 한 방법이 표시 되어의 색을 지정할 수 있습니다 `BoxView` .

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

`HslColorExtension`이 XML 태그 이면 4 개의 속성이 특성으로 설정 되지만 중괄호 사이에 표시 되는 경우 네 개의 속성이 따옴표 없이 쉼표로 구분 됩니다. , 및의 기본값 `H` 은 `S` 0이 `L` 고,의 기본값은 `A` 1 이므로 이러한 속성을 기본값으로 설정 하려는 경우 이러한 속성을 생략할 수 있습니다. 마지막 예제에서는 명도가 0이 고 일반적으로 검정색으로 표시 되는 예제를 보여 주지만 알파 채널은 0.5 이므로 절반 투명 하 고 페이지의 흰색 배경에 회색으로 표시 됩니다.

[![HSL 색 데모](creating-images/hslcolordemo-small.png "HSL 색 데모")](creating-images/hslcolordemo-large.png#lightbox "HSL 색 데모")

## <a name="a-markup-extension-for-accessing-bitmaps"></a>비트맵에 액세스 하기 위한 태그 확장

에 대 한 인수는 `ProvideValue` [`IServiceProvider`](xref:System.IServiceProvider) .net 네임 스페이스에 정의 된 인터페이스를 구현 하는 개체입니다 `System` . 이 인터페이스에는 인수를 사용 하 여 라는 메서드의 멤버가 하나 있습니다 `GetService` `Type` .

`ImageResourceExtension`아래에 표시 된 클래스는 및를 사용 `IServiceProvider` 하 여 `GetService` `IXmlLineInfoProvider` 특정 오류가 검색 된 위치를 나타내는 줄 및 문자 정보를 제공할 수 있는 개체를 가져오는 방법을 보여 줍니다. 이 경우에는 `Source` 속성이 설정 되지 않은 경우 예외가 발생 합니다.

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

`ImageResourceExtension`는 XAML 파일이 .NET Standard 라이브러리 프로젝트에 포함 리소스로 저장 된 이미지 파일에 액세스 해야 하는 경우에 유용 합니다. 속성을 사용 하 여 `Source` 정적 메서드를 호출 `ImageSource.FromResource` 합니다. 이 메서드에는 어셈블리 이름, 폴더 이름 및 마침표로 구분 된 파일 이름으로 구성 되는 정규화 된 리소스 이름이 필요 합니다. 메서드에 대 한 두 번째 인수는 `ImageSource.FromResource` 어셈블리 이름을 제공 하며 UWP의 릴리스 빌드에만 필요 합니다. 와 관계 없이 `ImageSource.FromResource` 은 비트맵이 포함 된 어셈블리에서 호출 되어야 합니다. 즉, 이미지가 해당 라이브러리에도 포함 되어 있지 않으면이 XAML 리소스 확장이 외부 라이브러리의 일부가 될 수 없습니다. 포함 리소스로 저장 된 비트맵에 액세스 하는 방법에 대 한 자세한 내용은 [**포함 된 이미지**](~/xamarin-forms/user-interface/images.md#embedded-images) 문서를 참조 하세요.

속성을 설정 해야 하지만 `ImageResourceExtension` `Source` 속성은 `Source` 특성에서 클래스의 콘텐츠 속성으로 표시 됩니다. 이는 `Source=` 중괄호 안에 있는 식의 일부를 생략할 수 있음을 의미 합니다. **이미지 리소스 데모** 페이지에서 `Image` 요소는 폴더 이름 및 마침표로 구분 된 파일 이름을 사용 하 여 두 개의 이미지를 인출 합니다.

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

[![이미지 리소스 데모](creating-images/imageresourcedemo-small.png "이미지 리소스 데모")](creating-images/imageresourcedemo-large.png#lightbox "이미지 리소스 데모")

## <a name="service-providers"></a>서비스 공급자

`IServiceProvider`Xaml 태그 확장은에 대 한 인수를 사용 하 여 `ProvideValue` 사용 중인 xaml 파일에 대 한 유용한 정보에 액세스할 수 있습니다. 하지만 인수를 성공적으로 사용 하려면 `IServiceProvider` 특정 컨텍스트에서 사용할 수 있는 서비스 종류를 알고 있어야 합니다. 이 기능을 이해 하는 가장 좋은 방법은 GitHub에서 리포지토리의 [ **MarkupExtensions** 폴더](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) 에 있는 기존 XAML 태그 확장의 소스 코드를 조사 하는 것입니다 Xamarin.Forms . 일부 서비스 유형은 내부에 Xamarin.Forms 있습니다.

일부 XAML 태그 확장에서는이 서비스가 유용할 수 있습니다.

```csharp
 IProvideValueTarget provideValueTarget = serviceProvider.GetService(typeof(IProvideValueTarget)) as IProvideValueTarget;
```

`IProvideValueTarget`인터페이스는 및의 두 속성을 정의 합니다 `TargetObject` `TargetProperty` . 클래스에서이 정보를 가져오는 경우는이 `ImageResourceExtension` `TargetObject` 고는 `Image` `TargetProperty` `BindableProperty` 의 속성에 대 한 개체입니다 `Source` `Image` . XAML 태그 확장이 설정 된 속성입니다.

`GetService`의 인수를 사용 하는 `typeof(IProvideValueTarget)` 호출은 실제로 `SimpleValueTargetProvider` 네임 스페이스에 정의 된 형식의 개체를 반환 합니다 `Xamarin.Forms.Xaml.Internals` . 의 반환 값을 해당 형식으로 캐스팅 하는 경우 `GetService` `ParentObjects` `Image` 요소, `Grid` 부모 및 `ImageResourceDemoPage` 의 부모를 포함 하는 배열인 속성에 액세스할 수도 있습니다 `Grid` .

## <a name="conclusion"></a>결론

XAML 태그 확장은 다양 한 소스에서 특성을 설정 하는 기능을 확장 하 여 XAML에서 중요 한 역할을 수행 합니다. 또한 기존 XAML 태그 확장이 필요한 항목을 정확 하 게 제공 하지 않을 경우 직접 작성할 수도 있습니다.

## <a name="related-links"></a>관련 링크

- [태그 확장 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [XAML 태그 확장 책의 장 Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
