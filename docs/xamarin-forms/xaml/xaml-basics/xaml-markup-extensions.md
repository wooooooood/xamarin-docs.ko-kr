---
제목: "3 부. Xaml 태그 확장 "설명:" XAML 태그 확장은 다른 소스에서 간접적으로 참조 되는 개체 또는 값으로 속성을 설정할 수 있도록 하는 XAML의 중요 한 기능을 구성 합니다. "
ms. prod: xamarin ms. 기술: assetid: F4A37564-B18B-42FF-B841-9A1949895AB6 author: davidbritch: dabritch:: 03/27/2018:: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="part-3-xaml-markup-extensions"></a>3부. XAML 태그 확장

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_XAML 태그 확장은 다른 소스에서 간접적으로 참조 되는 개체 또는 값으로 속성을 설정할 수 있도록 하는 XAML의 중요 한 기능을 구성 합니다. XAML 태그 확장은 개체를 공유 하 고 응용 프로그램 전체에서 사용 되는 상수를 참조 하는 데 특히 중요 하지만 데이터 바인딩에서 가장 큰 유틸리티를 찾을 수 있습니다._

## <a name="xaml-markup-extensions"></a>XAML 태그 확장

일반적으로 XAML을 사용 하 여 개체의 속성을 문자열, 숫자, 열거형 멤버 또는 백그라운드에서 값으로 변환 되는 문자열과 같은 명시적 값으로 설정 합니다.

그러나 속성은 경우에 따라 다른 위치에 정의 된 값을 참조 해야 하거나 런타임에 코드에서 약간의 처리가 필요할 수 있습니다. 이러한 목적을 위해 XAML *태그 확장* 을 사용할 수 있습니다.

이러한 XAML 태그 확장은 XML의 확장이 아닙니다. XAML은 완전히 올바른 XML입니다. 는를 구현 하는 클래스의 코드로 지원 되기 때문에 "확장" 이라고 `IMarkupExtension` 합니다. 사용자 고유의 사용자 지정 태그 확장을 작성할 수 있습니다.

대부분의 경우 xaml 태그 확장은 중괄호 ({및})로 구분 되는 특성 설정으로 나타나므로 XAML 파일에서 바로 인식할 수 있지만 태그 확장이 기존 요소로 태그에 표시 되는 경우도 있습니다.

## <a name="shared-resources"></a>공유 리소스

일부 XAML 페이지에는 속성이 같은 값으로 설정 된 여러 뷰가 포함 되어 있습니다. 예를 들어 이러한 개체에 대 한 대부분의 속성 설정은 `Button` 동일 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <StackLayout>
        <Button Text="Do this!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

        <Button Text="Do that!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

        <Button Text="Do the other thing!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

    </StackLayout>
</ContentPage>
```

이러한 속성 중 하나를 변경 해야 하는 경우 세 번이 아니라 한 번만 변경 하는 것이 좋습니다. 이러한 값이 코드 인 경우 상수 및 정적 읽기 전용 개체를 사용 하 여 이러한 값을 일관성 있게 유지 하 고 쉽게 수정할 수 있습니다.

XAML에서 널리 사용 되는 솔루션 중 하나는 *리소스 사전*에 이러한 값 또는 개체를 저장 하는 것입니다. 클래스는 형식의 `VisualElement` `Resources` `ResourceDictionary` 키 및 형식의 값을 포함 하는 사전 인 형식의 속성을 정의 합니다 `string` `object` . 개체를이 사전에 배치 하 고 XAML의 모든 태그에서 참조할 수 있습니다.

페이지에서 리소스 사전을 사용 하려면 `Resources` 속성-요소 태그 쌍을 포함 합니다. 페이지 맨 위에 배치 하는 것이 가장 편리 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>

    </ContentPage.Resources>
    ...
</ContentPage>
```

태그를 명시적으로 포함 해야 할 수도 `ResourceDictionary` 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

이제 다양 한 형식의 개체 및 값을 리소스 사전에 추가할 수 있습니다. 이러한 형식은 인스턴스화할 수 있어야 합니다. 예를 들어 추상 클래스가 될 수 없습니다. 이러한 형식에는 매개 변수가 없는 public 생성자도 있어야 합니다. 각 항목에는 특성으로 지정 된 사전 키가 필요 `x:Key` 합니다. 예를 들면 다음과 같습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <LayoutOptions x:Key="horzOptions"
                           Alignment="Center" />

            <LayoutOptions x:Key="vertOptions"
                           Alignment="Center"
                           Expands="True" />
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

이러한 두 항목은 구조체 형식의 값 이며, 각 항목에는 `LayoutOptions` 고유한 키와 하나 또는 두 개의 속성이 설정 되어 있습니다. 코드 및 태그에서의 정적 필드를 사용 하는 것이 훨씬 더 일반적 `LayoutOptions` 이지만 여기서는 속성을 설정 하는 것이 더 편리 합니다.

이제 이러한 `HorizontalOptions` `VerticalOptions` 단추의 및 속성을 이러한 리소스로 설정 해야 하며 XAML 태그 확장을 사용 하 여 수행 됩니다 `StaticResource` .

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="3"
        Rotation="-15"
        TextColor="Red"
        FontSize="24" />
```

`StaticResource`태그 확장은 항상 중괄호로 구분 되며 사전 키를 포함 합니다.

이름은 `StaticResource` `DynamicResource` 를 Xamarin.Forms 지 원하는와 구별 됩니다. `DynamicResource`는 런타임에 변경 될 수 있는 값과 연결 된 사전 키 용 이며,는 `StaticResource` 페이지의 요소가 생성 될 때 사전에 있는 요소에 한 번만 액세스 합니다.

속성의 경우 `BorderWidth` 사전에 double을 저장 해야 합니다. XAML은 및와 같은 일반적인 데이터 형식에 대 한 태그를 편리 하 게 정의 합니다 `x:Double` `x:Int32` .

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />

        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center"
                       Expands="True" />

        <x:Double x:Key="borderWidth">
            3
        </x:Double>
    </ResourceDictionary>
</ContentPage.Resources>
```

세 줄에는 넣을 필요가 없습니다. 이 회전 각도에 대 한이 사전 항목은 한 줄만 사용 됩니다.

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />

        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center"
                       Expands="True" />

         <x:Double x:Key="borderWidth">
            3
         </x:Double>

        <x:Double x:Key="rotationAngle">-15</x:Double>
    </ResourceDictionary>
</ContentPage.Resources>
```

이러한 두 리소스는 값과 동일한 방식으로 참조할 수 있습니다 `LayoutOptions` .

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="Red"
        FontSize="24" />
```

형식의 리소스의 경우 `Color` 이러한 형식의 특성을 직접 할당할 때 사용 하는 것과 동일한 문자열 표현을 사용할 수 있습니다. 리소스를 만들 때 형식 변환기가 호출 됩니다. 유형의 리소스는 `Color` 다음과 같습니다.

```xaml
<Color x:Key="textColor">Red</Color>
```

프로그램에서 `FontSize` 와 같은 열거형의 멤버로 속성을 설정 하는 경우가 많습니다 `NamedSize` `Large` . `FontSizeConverter`클래스는 메서드를 사용 하 여 플랫폼 종속 값으로 변환 하기 위해 내부적으로 작동 합니다 `Device.GetNamedSized` . 그러나 font-size 리소스를 정의할 때 여기에는 형식으로 표시 된 숫자 값을 사용 하는 것이 더 적합할 수 있습니다 `x:Double` .

```xaml
<x:Double x:Key="fontSize">24</x:Double>
```

이제를 제외한 모든 속성 `Text` 은 리소스 설정으로 정의 됩니다.

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="{StaticResource textColor}"
        FontSize="{StaticResource fontSize}" />
```

`OnPlatform`리소스 사전 내에서를 사용 하 여 플랫폼에 대해 다른 값을 정의할 수도 있습니다. 다음은 `OnPlatform` 개체가 다른 텍스트 색에 대 한 리소스 사전의 일부가 될 수 있는 방법입니다.

```xaml
<OnPlatform x:Key="textColor"
            x:TypeArguments="Color">
    <On Platform="iOS" Value="Red" />
    <On Platform="Android" Value="Aqua" />
    <On Platform="UWP" Value="#80FF80" />
</OnPlatform>
```

는 `OnPlatform` `x:Key` `x:TypeArguments` 제네릭 클래스 이기 때문에 특성이 사전에 있는 개체이 고 특성을 모두 가져옵니다. `iOS`, `Android` 및 특성은 `UWP` `Color` 개체가 초기화 될 때 값으로 변환 됩니다.

다음은 6 개의 공유 값에 액세스 하는 세 개의 단추가 있는 최종 전체 XAML 파일입니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <LayoutOptions x:Key="horzOptions"
                           Alignment="Center" />

            <LayoutOptions x:Key="vertOptions"
                           Alignment="Center"
                           Expands="True" />

            <x:Double x:Key="borderWidth">3</x:Double>

            <x:Double x:Key="rotationAngle">-15</x:Double>

            <OnPlatform x:Key="textColor"
                        x:TypeArguments="Color">
                <On Platform="iOS" Value="Red" />
                <On Platform="Android" Value="Aqua" />
                <On Platform="UWP" Value="#80FF80" />
            </OnPlatform>

            <x:Double x:Key="fontSize">24</x:Double>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <Button Text="Do this!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

        <Button Text="Do that!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

        <Button Text="Do the other thing!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

    </StackLayout>
</ContentPage>
```

스크린 샷은 일관 된 스타일과 플랫폼 종속 스타일을 확인 합니다.

[![스타일이 지정 된 컨트롤](xaml-markup-extensions-images/sharedresources.png)](xaml-markup-extensions-images/sharedresources-large.png#lightbox)

페이지 맨 위에 컬렉션을 정의 하는 것이 가장 일반적 이지만 속성은에 `Resources` `Resources` 의해 정의 되 `VisualElement` 고 `Resources` 페이지의 다른 요소에 대 한 컬렉션을 가질 수 있다는 점에 유의 하세요. 예를 들어 다음 예제에서에 하나를 추가 해 봅니다 `StackLayout` .

```xaml
<StackLayout>
    <StackLayout.Resources>
        <ResourceDictionary>
            <Color x:Key="textColor">Blue</Color>
        </ResourceDictionary>
    </StackLayout.Resources>
    ...
</StackLayout>
```

단추의 텍스트 색이 이제 파란색 임을 알 수 있습니다. 기본적으로 XAML 파서는 태그 확장을 발견할 때마다 `StaticResource` 시각적 트리를 검색 하 고 해당 키를 포함 하는 첫 번째 트리를 사용 합니다 `ResourceDictionary` .

리소스 사전에 저장 되는 가장 일반적인 개체 형식 중 하나는 Xamarin.Forms `Style` 속성 설정의 컬렉션을 정의 하는입니다. 스타일은 문서 [스타일](~/xamarin-forms/user-interface/styles/index.md)에 설명 되어 있습니다.

때로는 XAML을 처음 접하는 개발자가와 같은 시각적 요소를에 배치할 수 있는지 여부를 궁금해 `Label` `Button` `ResourceDictionary` 합니다. 이것은 가능 하지만 그다지 적합 하지 않습니다. 의 목적은 `ResourceDictionary` 개체를 공유 하는 것입니다. 시각적 요소는 공유할 수 없습니다. 동일한 인스턴스는 단일 페이지에 두 번 표시 될 수 없습니다.

## <a name="the-xstatic-markup-extension"></a>X:Static 태그 확장

이름의 유사성에도 불구 `x:Static` 하 고 `StaticResource` 는 매우 다릅니다. `StaticResource`는 다음 중 하나에 액세스 하는 동안 리소스 사전에서 개체를 반환 합니다 `x:Static` .

- 공용 정적 필드
- 공용 정적 속성
- 공용 상수 필드
- 열거형 멤버입니다.

`StaticResource`태그 확장은 리소스 사전을 정의 하는 xaml 구현에서 지원 되는 반면 `x:Static` 접두사는 xaml의 내장 부분입니다 `x` .

에서 `x:Static` 정적 필드 및 열거형 멤버를 명시적으로 참조할 수 있는 방법을 보여 주는 몇 가지 예제는 다음과 같습니다.

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="{x:Static LayoutOptions.Start}"
       HorizontalTextAlignment="{x:Static TextAlignment.Center}"
       TextColor="{x:Static Color.Aqua}" />
```

지금 까지는 매우 인상적이 지 않습니다. 그러나 `x:Static` 태그 확장은 사용자의 코드에서 정적 필드 또는 속성을 참조할 수도 있습니다. 예를 들어, 다음은 `AppConstants` 응용 프로그램 전체의 여러 페이지에서 사용할 수 있는 정적 필드를 포함 하는 클래스입니다.

```csharp
using System;
using Xamarin.Forms;

namespace XamlSamples
{
    static class AppConstants
    {
        public static readonly Thickness PagePadding;

        public static readonly Font TitleFont;

        public static readonly Color BackgroundColor = Color.Aqua;

        public static readonly Color ForegroundColor = Color.Brown;

        static AppConstants()
        {
            switch (Device.RuntimePlatform)
            {
                case Device.iOS:
                    PagePadding = new Thickness(5, 20, 5, 0);
                    TitleFont = Font.SystemFontOfSize(35, FontAttributes.Bold);
                    break;

                case Device.Android:
                    PagePadding = new Thickness(5, 0, 5, 0);
                    TitleFont = Font.SystemFontOfSize(40, FontAttributes.Bold);
                    break;

                case Device.UWP:
                    PagePadding = new Thickness(5, 0, 5, 0);
                    TitleFont = Font.SystemFontOfSize(50, FontAttributes.Bold);
                    break;
            }
        }
    }
}
```

XAML 파일에서이 클래스의 정적 필드를 참조 하려면이 파일이 있는 XAML 파일 내에를 표시 하는 방법이 필요 합니다. XML 네임 스페이스 선언을 사용 하 여이 작업을 수행 합니다.

표준 xaml 템플릿의 일부로 생성 된 XAML 파일에는 Xamarin.Forms 두 개의 XML 네임 스페이스 선언이 포함 됩니다. 하나는 클래스 액세스를 위한 것이 Xamarin.Forms 고 다른 하나는 xaml에 내장 된 태그 및 특성을 참조 하기 위한 것입니다.

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

다른 클래스에 액세스 하려면 추가 XML 네임 스페이스 선언이 필요 합니다. 각 추가 XML 네임 스페이스 선언은 새 접두사를 정의 합니다. 공유 응용 프로그램 .NET Standard 라이브러리에 대 한 로컬 클래스 (예:)에 액세스 하려면 `AppConstants` XAML 프로그래머는 종종 접두사를 사용 `local` 합니다. 네임 스페이스 선언은 .NET 네임 스페이스 이름이 라고도 하는 CLR (공용 언어 런타임) 네임 스페이스 이름 (c # 정의 또는 지시문에 표시 되는 이름)을 나타내야 합니다 `namespace` `using` .

```csharp
xmlns:local="clr-namespace:XamlSamples"
```

.NET Standard 라이브러리에서 참조 하는 모든 어셈블리에서 .NET 네임 스페이스에 대 한 XML 네임 스페이스 선언을 정의할 수도 있습니다. 예를 들어, 다음 `sys` `System` 은 **netstandard.library** 어셈블리에 있는 표준 .net 네임 스페이스에 대 한 접두사입니다. 이는 다른 어셈블리 이므로 어셈블리 이름 (이 경우 **netstandard.library**)도 지정 해야 합니다.

```csharp
xmlns:sys="clr-namespace:System;assembly=netstandard"
```

키워드 뒤에는 `clr-namespace` 콜론, .net 네임 스페이스 이름, 세미콜론, 키워드 `assembly` , 등호 및 어셈블리 이름이 차례로 나옵니다.

예, 콜론은 뒤에 `clr-namespace` 등호가 나옵니다 `assembly` . 구문은 이러한 방식으로 의도적으로 정의 되었습니다. 대부분의 XML 네임 스페이스 선언은와 같은 URI 체계 이름을 시작 하는 URI를 참조 합니다. 여기에는 `http` 항상 콜론이 붙습니다. `clr-namespace`이 문자열의 부분은 해당 규칙을 모방 하기 위한 것입니다.

이러한 네임 스페이스 선언은 모두 **StaticConstantsPage** 샘플에 포함 되어 있습니다. `BoxView`차원이 and로 설정 되어 `Math.PI` `Math.E` 있지만 100의 비율로 크기가 조정 되었습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             xmlns:sys="clr-namespace:System;assembly=netstandard"
             x:Class="XamlSamples.StaticConstantsPage"
             Title="Static Constants Page"
             Padding="{x:Static local:AppConstants.PagePadding}">

    <StackLayout>
       <Label Text="Hello, XAML!"
              TextColor="{x:Static local:AppConstants.BackgroundColor}"
              BackgroundColor="{x:Static local:AppConstants.ForegroundColor}"
              Font="{x:Static local:AppConstants.TitleFont}"
              HorizontalOptions="Center" />

      <BoxView WidthRequest="{x:Static sys:Math.PI}"
               HeightRequest="{x:Static sys:Math.E}"
               Color="{x:Static local:AppConstants.ForegroundColor}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               Scale="100" />
    </StackLayout>
</ContentPage>
```

`BoxView`화면을 기준으로 하는 결과의 크기는 플랫폼에 따라 다릅니다.

[![X:Static 태그 확장을 사용 하는 컨트롤](xaml-markup-extensions-images/staticconstants.png)](xaml-markup-extensions-images/staticconstants-large.png#lightbox)

## <a name="other-standard-markup-extensions"></a>기타 표준 태그 확장

몇 가지 태그 확장은 XAML에 내장 되며 xaml 파일에서 지원 됩니다 Xamarin.Forms . 이러한 항목 중 일부는 자주 사용 되지 않지만 필요할 때 반드시 필요 합니다.

- 속성에 `null` 기본값이 아닌 값이 있지만이 속성을로 설정 하려면 `null` 태그 확장으로 설정 합니다 `{x:Null}` .
- 속성이 형식인 경우 `Type` `Type` 태그 확장을 사용 하 여 개체에 할당할 수 있습니다 `{x:Type someClass}` .
- 태그 확장을 사용 하 여 XAML에서 배열을 정의할 수 있습니다 `x:Array` . 이 태그 확장에는 `Type` 배열의 요소 형식을 나타내는 라는 필수 특성이 있습니다.
- `Binding`태그 확장은 4 부에서 설명 합니다 [. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)입니다.
- `RelativeSource`태그 확장은 [상대 바인딩에서](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md)설명 합니다.

## <a name="the-constraintexpression-markup-extension"></a>ConstraintExpression 태그 확장

태그 확장에는 속성이 포함 될 수 있지만 XML 특성과 같이 설정 되지 않습니다. 태그 확장에서 속성 설정은 쉼표로 구분 되며 중괄호 안에는 따옴표가 표시 되지 않습니다.

이는 Xamarin.Forms `ConstraintExpression` 클래스와 함께 사용 되는 이라는 태그 확장을 사용 하 여 설명할 수 있습니다 `RelativeLayout` . 자식 뷰의 위치나 크기를 상수로 지정 하거나 부모 또는 다른 명명 된 뷰를 기준으로 지정할 수 있습니다. 의 구문을 `ConstraintExpression` 사용 하 여 `Factor` 다른 뷰의 속성을 더한 값을 사용 하 여 뷰의 위치나 크기를 설정할 수 있습니다 `Constant` . 코드가 필요한 것 보다 더 복잡 합니다.

예를 들면 다음과 같습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.RelativeLayoutPage"
             Title="RelativeLayout Page">

    <RelativeLayout>

        <!-- Upper left -->
        <BoxView Color="Red"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}" />
        <!-- Upper right -->
        <BoxView Color="Green"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=1,
                                            Constant=-40}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}" />
        <!-- Lower left -->
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=1,
                                            Constant=-40}" />
        <!-- Lower right -->
        <BoxView Color="Yellow"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=1,
                                            Constant=-40}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=1,
                                            Constant=-40}" />

        <!-- Centered and 1/3 width and height of parent -->
        <BoxView x:Name="oneThird"
                 Color="Red"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=0.33}"
                 RelativeLayout.WidthConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.HeightConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=0.33}"  />

        <!-- 1/3 width and height of previous -->
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=X}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Y}"
                 RelativeLayout.WidthConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.HeightConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Height,
                                            Factor=0.33}"  />
    </RelativeLayout>
</ContentPage>
```

이 샘플에서 수행 해야 하는 가장 중요 한 단원은 태그 확장의 구문입니다. 인용 부호는 태그 확장의 중괄호 안에 표시 되지 않아야 합니다. XAML 파일에 태그 확장을 입력할 때 속성의 값을 큰따옴표로 묶어야 합니다. 하려는 유혹!

실행 되는 프로그램은 다음과 같습니다.

[![제약 조건을 사용 하는 상대 레이아웃](xaml-markup-extensions-images/relativelayout.png)](xaml-markup-extensions-images/relativelayout-large.png#lightbox)

## <a name="summary"></a>요약

여기에 표시 된 XAML 태그 확장은 XAML 파일에 대 한 중요 한 지원을 제공 합니다. 그러나 가장 중요 한 XAML 태그 확장은 `Binding` 이 시리즈의 다음 부분인 4 부에서 설명 하는입니다 [. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)입니다.

## <a name="related-links"></a>관련 링크

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [1 부. XAML 시작](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [2 부. 필수 XAML 구문](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [4 부. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [5 부. MVVM에 데이터 바인딩](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
