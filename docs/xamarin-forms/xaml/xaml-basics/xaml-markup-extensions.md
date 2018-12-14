---
title: 3부. XAML 태그 확장
description: XAML 태그 확장 속성은 다른 소스에서 간접적으로 참조되는 개체 또는 값으로 속성을 설정할 수 있는 XAML에서 중요 한 기능을 구성 합니다.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F4A37564-B18B-42FF-B841-9A1949895AB6
author: davidbritch
ms.author: dabritch
ms.date: 3/27/2018
ms.openlocfilehash: a93503762528885dfc7d3b5400bf4ec716ea9fab
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53056217"
---
# <a name="part-3-xaml-markup-extensions"></a>3부. XAML 태그 확장

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)

_XAML 태그 확장 속성은 다른 소스에서 간접적으로 참조되는 개체 또는 값으로 속성을 설정할 수 있는 XAML에서 중요 한 기능을 구성 합니다. XAML 태그 확장은 개체를 공유하고 응용 프로그램 전체에서 사용되는 상수를 참조할 때 특히 중요하지만 가장 큰 장점은 데이터 바인딩에서 찾을 수 있습니다._

## <a name="xaml-markup-extensions"></a>XAML 태그 확장

일반적으로 XAML을 사용하여 문자열, 숫자, 열거형 멤버, 또는 배후에서 값으로 변환되는 문자열 등의 명시적 값으로 개체의 속성을 설정하기 위해 사용 합니다.

그러나 때로는 속성이 다른 곳에 정의 된 값을 대신 참조하거나 런타임 시 코드에 의해 약간의 처리가 필요할 수 있습니다. 이러한 목적에서 XAML *태그 확장*을 사용할 수 있습니다.

해당 XAML 태그 확장은 XML의 확장이 아닙니다. XAML은 완전히 올바른 XML입니다. `IMarkupExtension`을 구현하는 클래스의 코드에 의해 뒷받침되기 때문에 "extensions"이라 합니다. 자신의 사용자 지정 태그 확장을 작성할 수 있습니다.

대부분의 경우에서 XAML 태그 확장은 XAML 파일에서 즉시 인식할 수 있는 중괄호로 구분 된 특성 설정 { and }로 표시 되지만, 때로는 태그 확장이 전통적인 요소로 태그에 표시 됩니다.

## <a name="shared-resources"></a>공유 리소스

일부 XAML 페이지는 동일한 값으로 설정하는 속성을 사용하는 여러 뷰를 포함 합니다. 예를 들어, 다음과 같이 `Button` 개체에 대한 대부분의 속성 설정은 동일 합니다.

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

해당 속성 중 하나를 변경해야 하는 경우, 세 번 보다는 한 번만 내용을 변경하는 것이 좋습니다. 코드라면 상수 및 정적(static) 읽기 전용 개체를 사용하여 해당 값을 일관되고 유지하고 쉽게 수정할 수 있습니다.

XAML에서 한가지 인기있는 해결책은 해당 값이나 개체를 *리소스 사전*에 저장하는 것입니다. `VisualElement` 클래스는 `ResourceDictionary` 유형의 `Resources`라는 속성을 정의합니다. `string` 유형의 키와 `object` 유형의 값을 가진 사전입니다. 이 사전에 개체를 넣으면 모든 XAML 내에서 해당 개체를 태그로 참조할 수 있습니다.

한 페이지에서 리소스 사전을 사용하려면, 한 쌍의 `Resources` 속성 요소 태그를 포함 합니다. 다음과 같이 해당 페이지의 맨 위에 두는 것이 가장 유용 합니다.

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

다음과 같이 `ResourceDictionary` 태그를 명시적으로 포함시켜야 합니다.

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

이제 다양한 유형의 개체와 값을 리소스 사전에 추가할 수 있습니다. 해당 유형은 인스턴스화할 수 있어야 합니다. 예를 들어, 추상(abstract) 클래스는 추가 할 수 없습니다. 해당 유형에는 매개 변수가 없는 공용(public) 생성자가 있어야 합니다. 각 항목에는 `x:Key` 특성으로 지정된 사전 키가 필요합니다. 예를 들어 다음과 같습니다.

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

위 두 항목은 `LayoutOptions` 구조체 형식의 값이 있고, 각각은 고유 키 및 하나 또는 두 개의 속성 설정이 있습니다. 코드 및 태그에서 `LayoutOptions`의 정적(static) 필드를 사용하는 것이 일반적이지만 여기서는 속성을 설정하는 것이 더 편리 합니다.

이제 다음과 같이 버튼의 `HorizontalOptions` 및 `VerticalOptions` 속성을 해당 리소스에 설정하고, 해당 작업은 `StaticResource` XAML 태그 확장으로 완료됩니다.

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="3"
        Rotation="-15"
        TextColor="Red"
        FontSize="24" />
```

`StaticResource` 태그 확장은 항상 중괄호로 구분되며 사전 키를 포함 합니다.

`StaticResource`라는 이름은 Xamarin.Forms도 마찬가지로 지원하는 `DynamicResource`와 구별됩니다. `DynamicResource`는 런타임 중에 변경 될 수 있는 값과 연결 된 사전 키를 위한 것이고, `StaticResource`는 페이지 상의 요소가 생성될 때 한 번만 사전에서 요소를 접근 합니다.

`BorderWidth` 속성의 경우, 사전에 double 값을 저장 할 필요가 있습니다. 다음과 같이 XAML은 `x:Double` 및 `x:Int32`와 같은 공통 데이터 유형에 대한 태그를 편리하게 정의합니다.

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

위에서 처럼 세 줄에 추가할 필요는 없습니다. 다음과 같이 해당 회전 각도 대한 사전 항목은 한 줄만 차지합니다.

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

위의 두 리소스는 다음과 같이 `LayoutOptions` 값과 동일한 방식으로 참조할 수 있습니다.

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="Red"
        FontSize="24" />
```

`Color` 유형의 리소스의 경우, 해당 유형의 특성을 직접 할당할 때 사용하는 것과 동일한 문자열 표현을 사용할 수 있습니다. 유형 변환기는 리소스가 생성될 때 호출 됩니다. 다음은 `Color` 유형 리소스입니다.

```xaml
<Color x:Key="textColor">Red</Color>
```

종종 프로그램은 `FontSize` 속성을 `Large`와 같은 `NamedSize` 열거형 멤버로 설정합니다. `FontSizeConverter` 클래스는 배후에서 작동하여 `Device.GetNamedSized` 메서드를 사용하여 플랫폼 독립적인 값으로 변환합니다. 그러나 글꼴 크기 리소스를 정의할 때는 여기서 보는 바와 같이 `x:Double`로 숫자 값을 사용하는 것이 더 합리적입니다.

```xaml
<x:Double x:Key="fontSize">24</x:Double>
```

이제 `Text`를 제외한 모든 속성은 리소스 설정에 의해 정의 됩니다.

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="{StaticResource textColor}"
        FontSize="{StaticResource fontSize}" />
```

또한 리소스 사전에서 `OnPlatform`을 사용하여 플랫폼 별로 다른 값을 정의할 수도 있습니다. 다음은 `OnPlatform` 개체가 다른 텍스트 색상을 위한 리소스 사전의 일부가 되도록 하는 방법입니다.

```xaml
<OnPlatform x:Key="textColor"
            x:TypeArguments="Color">
    <On Platform="iOS" Value="Red" />
    <On Platform="Android" Value="Aqua" />
    <On Platform="UWP" Value="#80FF80" />
</OnPlatform>
```

`OnPlatform`은 해당 개체가 사전에 있기 때문에 `x:Key` 특성과 제네릭 클래스이기 때문에 `x:TypeArguments` 특성 둘 모두를 얻습니다. `iOS`, `Android` 및 `UWP` 특성은 개체가 초기화 될 때 `Color` 값으로 변환됩니다.

다음은 여섯 개의 공유 값에 접근하는 세 개의 버튼이 있는 완전한 최종 XAML 파일 입니다.

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

            <x:String x:Key="fontSize">Large</x:String>
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

스크린샷은 일관 된 스타일 및 플랫폼 별 스타일 지정을 다음과 같이 보여줍니다.

[![](xaml-markup-extensions-images/sharedresources.png "스타일의 컨트롤")](xaml-markup-extensions-images/sharedresources-large.png#lightbox "스타일의 컨트롤")

페이지의 상단에 `Resources` 컬렉션을 정의하는 것이 가장 일반적이지만, `Resources` 속성은 `VisualElement`에 의해 정의되고, 페이지의 다른 요소에서 `Resources` 컬렉션을 사용할 수도 있습니다. 예를 들어, 다음과 같이 `StackLayout`에 하나를 추가해 봅니다.

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

버튼의 텍스트 색상이 파란색임을 이제 알 수 있습니다. 기본적으로 XAML 파서가 `StaticResource` 태그 확장을 발견하면 시각적 트리를 검색하여 해당 키가 포함된 첫 번째 `ResourceDictionary`를 사용합니다.

리소스 사전에 저장 된 가장 일반적인 유형 중 하나는 속성 설정 컬렉션을 정의하는 Xamarin.Forms `Style` 입니다. 스타일은 [스타일](~/xamarin-forms/user-interface/styles/index.md) 문서에 설명되어 있습니다.

가끔 XAML 처음 접하는 개발자는 `ResourceDictionary`에 `Label` 또는 `Button`과 같은 시각적 요소를 넣을 수 있는지 궁금해 합니다. 물론 가능은 하지만 별로 의미가 없습니다. `ResourceDictionary`의 목적은 개체를 공유하는 것입니다. 시각적 요소는 공유할 수 없습니다. 동일한 인스턴스가 한 페이지에 두 번 나타날 수 없습니다.

## <a name="the-xstatic-markup-extension"></a>x:Static 태그 확장

해당 이름의 유사성에도 불구하고 `x:Static`과 `StaticResource`는 매우 다릅니다. `StaticResource`는 리소스 사전에서 개체를 반환하는 반면 `x:Static`은 다음 중 하나에 액세스 합니다.

- 공용(public) 정적(static) 필드
- 공용 정적 속성
- 공용 상수(constant) 필드
- 열거형 멤버 입니다.

`StaticResource` 태그 확장은 리소스 사전을 정의하는 XAML 구현에 의해 지원되는 반면, `x:Static`은 `x` 접두사가 드러내는 것처럼 XAML의 본질적인 부분입니다.

다음은 `x:Static`이 정적 필드 및 열거형 멤버를 명시적으로 참조할 수 있는 방법을 보여주는 몇 가지 예제입니다.

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="{x:Static LayoutOptions.Start}"
       HorizontalTextAlignment="{x:Static TextAlignment.Center}"
       TextColor="{x:Static Color.Aqua}" />
```

지금까지는 별로 인상적이지 않습니다. 하지만 `x:Static` 태그 확장은 정적 필드나 속성을 자신의 코드에서 참조할 수도 있습니다. 예를 들어, 다음은 응용 프로그램 전체에서 여러 페이지에 사용할 수 있는 정적 필드가 포함된 `AppConstants` 클래스 입니다.

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

XAML 파일에서 해당 클래스의 정적 필드를 참조 하려면 XAML 파일에서 해당 파일의 위치를 나타내는 몇 가지 방법이 필요합니다. XML 네임 스페이스 선언을 사용하면 됩니다.

표준 Xamarin.Forms XAML 템플릿의 일부로 생성 된 XAML 파일에는 다음과 같이 Xamarin.Forms 클래스에 접근하기 위한 XML 선언과 XAML에 고유한 태그 및 특성을 참조하는 두 개의 XML 네임 스페이스 선언이 포함되어 있습니다.

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

다른 클래스에 접근하려면 XML 네임 스페이스 선언이 필요 합니다. 각 추가 XML 네임 스페이스 선언은 새 접두사를 정의합니다. `AppConstants`와 같은 공유 응용 프로그램 .NET Standard 라이브러리의 로컬 클래스에 접근하려면, XAML 프로그래머는 종종 `local` 접두사를 사용합니다. 네임 스페이스 선언은 CLR (Common Language Runtime, 공용 언어 런타임) 네임 스페이스 이름(.NET 네임 스페이스 이름이라고도 함)을 나타내야 합니다. 해당 네임 스페이스 이름은 C# `namespace` 정의 또는 `using` 지시문에 나타나는 이름 입니다.

```csharp
xmlns:local="clr-namespace:XamlSamples"
```

.NET Standard 라이브러리를 참조하는 모든 어셈블리에서 .NET 네임 스페이스에 대한 XML 네임 스페이스 선언을 정의할 수도 있습니다. 예를 들어, 다음은 표준 .NET `System` 네임 스페이스에 대한 `sys` 접두사 입니다. 해당 네임 스페이스는 다음과 같이 **mscorlib** 어셈블리에 있으며, 한때 "Microsoft 공용 개체 런타임 라이브러리(Microsoft Common Object Runtime Library)"를 나타냈지만 이제 "다국어 표준 개체 런타임 라이브러리(Multilanguage Standard Common Object Runtime Library)"를 의미합니다.

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

키워드 `clr-namespace` 뒤에 콜론이 오고, 그 다음 .NET 네임 스페이스 이름, 세미콜론, 키워드 `assembly`, 등호 및 어셈블리 이름이 옵니다.

맞습니다, 콜론은 `clr-namespace` 뒤에 오지만 등호는 `assembly` 다음에 옵니다. 구문은 해당 방식으로 의도적으로 정의 되었습니다. 대부분의 XML 네임 스페이스 선언은 `http`와 같이 URI 스키마 이름을 시작하는 URI를 참조하며 항상 뒤에 콜론이 옵니다. 해당 문자열의 `clr-namespace` 부분은 URI 규칙을 모방하기 위함입니다.

해당 네임 스페이스 선언은 모두 **StaticConstantsPage** 샘플에 포함되어 입니다. `BoxView` 차원은 `Math.PI` 및 `Math.E`로 설정되어 있지만 100의 비율로 배율이 조정됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
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

다음과 같이 `BoxView` 결과의 크기는 화면에 상대적인 플랫폼에 따라 다릅니다.

 [![](xaml-markup-extensions-images/staticconstants.png "X:static 태그 확장을 사용하는 컨트롤")](xaml-markup-extensions-images/staticconstants-large.png#lightbox "X:static 태그 확장을 사용하는 컨트롤")

## <a name="other-standard-markup-extensions"></a>기타 표준 태그 확장

여러 가지 태그 확장은 XAML에 고유하고 Xamarin.Forms XAML 파일에서 지원합니다. 이 중 몇 가지는 자주 사용되지 않지만 필요할 때 기본적 입니다.

-  속성은 기본적으로 `null`이 아닌 값을 갖지만, `null`로 설정하고자 한다면 `{x:Null}` 태그 확장으로 설정합니다.
-  속성이 `Type` 형식인 경우, 그것을 `{x:Type someClass}`를 사용하여 `Type` 개체에 할당할 수 있습니다.
-  XAML의 배열은 `x:Array` 태그 확장을 사용하여 정의할 수 있습니다. 이 태그 확장에는 배열에 있는 요소 유형을 나타내는 `Type`이라는 필수 특성이 있습니다.
- `Binding` 태그 확장에 대해서는 설명 [4부. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)에서 논의 합니다.

## <a name="the-constraintexpression-markup-extension"></a>ConstraintExpression 태그 확장

태그 확장은 속성을 가질 수 있지만, XML 특성처럼 설정되지는 않습니다. 태그 확장에서 속성 설정은 쉼표로 구분하며 중괄호 내에서 따옴표는 표시되지 않습니다.

이것은 `RelativeLayout` 클래스와 함께 사용되는 `ConstraintExpression`이라는 Xamarin.Forms 태그 확장을 사용하여 설명할 수 있습니다. 자식 뷰의 위치나 크기를 상수로 지정하거나 부모 또는 다른 명명 된 뷰와 상대적으로 지정할 수 있습니다. `ConstraintExpression`의 구문은 `Factor`와 다른 뷰의 속성에 `Constant`를 곱한 값을 사용하여 뷰의 위치나 크기를 설정할 수 있습니다. 그것보다 더 복잡한 것은 코드가 필요합니다.

예를 들면 다음과 같습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.RelativeLayoutPage"
             Title="RelativeLayout Page">

    <RelativeLayout>

        <!-- 왼쪽 위 -->
        <BoxView Color="Red"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}" />
        <!-- 오른쪽 위 -->
        <BoxView Color="Green"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=1,
                                            Constant=-40}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}" />
        <!-- 왼쪽 아래 -->
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=1,
                                            Constant=-40}" />
        <!-- 오른쪽 아래 -->
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

        <!-- 중앙 및 부모의 넓이와 높이의 1/3 -->
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

        <!-- 이전 넓이와 높이의 1/3 -->
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

아마도이 샘플에서 얻어야 할 가장 중요한 배움은 태그 확장의 구문입니다. 따옴표는 태그 확장의 중괄호 내에 올 수 없습니다. XAML 파일에 태그 확장을 입력할 때 속성의 값을 자연스럽게 따옴표로 묶고 싶을 것입니다. 유혹을 이겨내십시오!

실행 중인 프로그램이 다음과 같습니다.

[![](xaml-markup-extensions-images/relativelayout.png "제약 조건을 사용하는 상대 레이아웃")](xaml-markup-extensions-images/relativelayout-large.png#lightbox "제약 조건을 사용하는 상대 레이아웃")

## <a name="summary"></a>요약

문서에 표시된 XAML 태그 확장은 XAML 파일에 대한 중요한 지원을 제공합니다. 하지만 가장 가치있는 XAML 태그 확장은 아마도 해당 시리즈의 다음 단원인 [4부. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)에서 다루는 `Binding` 입니다.



## <a name="related-links"></a>관련 링크

- [Xaml 샘플](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [1부. XAML 시작](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [2부. 필수 XAML 구문](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [4부. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [5부. 데이터 바인딩부터 MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
