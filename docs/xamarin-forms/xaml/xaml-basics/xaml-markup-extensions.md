---
title: "3 부 합니다. XAML 태그 확장"
description: "XAML 태그 확장 개체 또는 다른 소스에서 직접 참조 되는 값을 설정 하는 속성을 허용 하는 XAML에 중요 한 기능을 구성 합니다. XAML 태그 확장은 개체를 공유 하 고 응용 프로그램 전체에서 사용 되는 상수를 참조 하기 위해 특히 중요 하지만 활용도 가장 큰 데이터 바인딩을 찾습니다."
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 7aea7b1536efc952378c6a1df63640af191f1ebe
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="part-3-xaml-markup-extensions"></a>3 부 합니다. XAML 태그 확장

_XAML 태그 확장 개체 또는 다른 소스에서 직접 참조 되는 값을 설정 하는 속성을 허용 하는 XAML에 중요 한 기능을 구성 합니다. XAML 태그 확장은 개체를 공유 하 고 응용 프로그램 전체에서 사용 되는 상수를 참조 하기 위해 특히 중요 하지만 활용도 가장 큰 데이터 바인딩을 찾습니다._

## <a name="xaml-markup-extensions"></a>XAML 태그 확장

일반적으로 XAML 속성을 설정할 수 있는 개체의 문자열, 숫자, 열거형 멤버 또는 원리를 자세히 파악할수록 값으로 변환 하는 문자열 등의 명시적 값을 사용 합니다.

그러나 경우에 따라 속성 곳에서 정의 된 다른 값을 대신 참조 해야 합니다 또는 런타임 시 코드에 의해 거의 처리 필요할 수 있는 합니다. 이러한 목적을 XAML에 대 한 *태그 확장* 사용할 수 있습니다.

이러한 XAML 태그 확장은 XML의 확장 되지 않습니다. XAML은 xml이 유효한 전체 XML입니다. 코드를 구현 하는 클래스에 의해 백업 하기 때문에 "extension" 라고 `IMarkupExtension`합니다. 사용자 고유의 사용자 지정 태그 확장을 작성할 수 있습니다.

대부분의 경우에서 XAML 태그 확장은 XAML 파일에 쉽게 알아볼 중괄호로 구분 된 특성 설정으로 나타나기 때문에: {및} 있지만 태그 확장 태그에 기존 요소로 표시 하는 경우도 있습니다.

## <a name="shared-resources"></a>공유 리소스

일부 XAML 페이지 속성이 동일한 값으로 설정 된 여러 가지 뷰를 포함 합니다. 예를 들어 대부분의 이러한 속성 설정 `Button` 개체는 동일 합니다.

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
                FontSize="Large" />

        <Button Text="Do that!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="Large" />

        <Button Text="Do the other thing!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="Large" />

    </StackLayout>
</ContentPage>
```

이러한 속성 중 하나를 변경 해야, 하는 경우 세 번 보다는 한 번만 변경 만들기 수도 있습니다. 코드 인 있습니다 가능성이 사용 될 수 상수 및 정적 읽기 전용 개체 일관적이 고 쉽게 수정할 수 이러한 값을 유지 합니다.

XAML에서 하나의 인기 있는 솔루션은 이러한 값을 저장 하 또는 개체에 *리소스 사전*합니다. `VisualElement` 라는 속성을 정의 하는 클래스 `Resources` 형식의 `ResourceDictionary`, 형식의 키를 사용 하 여 사전 변수인 `string` 형식의 값 및 `object`합니다. 이 사전에 개체를 입력 하 고 모든 XAML에서 태그에서 참조할 수 있습니다.

리소스 사전에서 페이지를 사용 하는 한 쌍의 포함 `Resources` 속성 요소 태그입니다. 페이지의 위쪽에 배치 하는 데 가장 편리한 됩니다.

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

명시적으로 포함 하는 데 필요한 이기도 `ResourceDictionary` 태그:

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

이제 리소스 사전에 개체 및 다양 한 형식의 값을 추가할 수 있습니다. 이러한 형식은 인스턴스화할 수 있는 이어야 합니다. 추상 클래스, 예를 들어 일 수 없습니다. 이러한 형식에 매개 변수가 없는 public 생성자가 있어야 합니다. 각 항목은 지정 된 사전 키 사용은 `x:Key` 특성입니다. 예:

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

이러한 두 항목은 구조체 형식의 값 `LayoutOptions`, 고유 키 마다 하나 또는 두 개의 속성을 설정 하 고 있습니다. 코드 및 태그에서 훨씬 더 일반적으로의 정적 필드를 사용 하는 `LayoutOptions`, 하는데이 매개 변수가 속성을 설정 하는 것이 편리 합니다.

설정 해야 하는 이제는 `HorizontalOptions` 및 `VerticalOptions` 이러한 리소스에 이러한 단추의 속성 작업을 완료 하 고는 `StaticResource` XAML 태그 확장:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="3"
        Rotation="-15"
        TextColor="Red"
        FontSize="Large" />
```

`StaticResource` 태그 확장 항상 중괄호로 구분 하 고 사전 키가 포함 됩니다.

이름을 `StaticResource` 구별할 `DynamicResource`, Xamarin.Forms도 지원입니다. `DynamicResource` 런타임 중에 변경 될 수 있는 값과 연결 된 사전 키에 대 한 동안 `StaticResource` 페이지에서 요소는 생성 된 경우 한 번만 사전에서 요소에 액세스 합니다.

에 대 한는 `BorderWidth` 속성을 해야 하는 double 사전에 저장 합니다. 와 같은 일반적인 데이터 형식에 대 한 태그를 편리 하 게 정의 하는 XAML `x:Double` 및 `x:Int32`:

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

세 줄에 넣이 필요가 없습니다. 이 회전 각도이 사전 항목을 한 줄 위로 사용 합니다.

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

이러한 두 리소스와 동일한 방식으로 참조할 수 있습니다는 `LayoutOptions` 값:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="Red"
        FontSize="Large" />
```

종류의 리소스에 대 한 `Color`, 직접 이러한 유형의 특성을 할당할 때 사용 하는 동일한 문자열 표현을 사용할 수 있습니다. 형식 변환기는 리소스를 만들 때 호출 됩니다. 다음은 형식의 리소스 `Color`:

```xaml
<Color x:Key="textColor">Red</Color>
```

`FontSize` 속성에서 작은 문제가 발생 합니다. 속성 형식으로 정의 된 `double`합니다. 멤버에 속성을 설정 하면는 `NamedSize` 열거와 같은 `Large`, `FontSizeConverter` 사용 하 여 플랫폼별 값으로 변환 하는 백그라운드로 작동 클래스는 `Device.GetNamedSized` 메서드.

그러나으로 글꼴 크기에 대 한 리소스를 정의할 수 없습니다는 `double` "large" 값을 설정 합니다. XAML 파서에서 리소스를 처리 하는 동시에 있는지 알 수 없으므로 글꼴 크기는 값이 사용 됩니다. 

솔루션으로 리소스를 정의 하는 것을 `string` 를 사용 하는 `x:String` 유형:

```xaml
<x:String x:Key="fontSize">Large</x:String>
```

이제 모든 속성을 제외한 `Text` 리소스 설정에 의해 정의 됩니다.

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="{StaticResource textColor}"
        FontSize="{StaticResource fontSize}" />
```

사용 하 여 이기도 `OnPlatform` 리소스 사전에는 플랫폼에 대해 다른 값을 정의 내에서. 다음은 어떻게는 `OnPlatform` 개체에는 다양 한 텍스트 색에 대 한 리소스 사전의 일부가 될 수 있습니다:

```xaml
<OnPlatform x:Key="textColor"
            x:TypeArguments="Color">
    <On Platform="iOS" Value="Red" />
    <On Platform="Android" Value="Aqua" />
    <On Platform="UWP" Value="#80FF80" />
</OnPlatform>
```

다음에 유의 `OnPlatform` 둘 다 가져옵니다는 `x:Key` 사전에 있는 개체 이기 때문에 특성 및 `x:TypeArguments` 특성은 제네릭 클래스입니다. `iOS`, `Android`, 및 `UWP` 특성으로 변환 됩니다 `Color` 개체가 인스턴스화될 때 값입니다.

다음은 공유 6 개 값에 액세스 하는 세 개의 단추가 포함 된 마지막 전체 XAML 파일이입니다.

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
                FontSize"{StaticResource fontSize}" />

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

스크린샷을 일관 된 스타일 및 플랫폼 종속 스타일 지정을 확인합니다.

[ ![](xaml-markup-extensions-images/sharedresources.png "스타일의 컨트롤")](xaml-markup-extensions-images/sharedresources-large.png "스타일의 컨트롤")

정의 하는 가장 일반적으로 하지만 `Resources` 컬렉션 페이지 맨 위에 있는 염두에서에 둬야 하는 `Resources` 속성으로 정의 된 `VisualElement`, 있으며 있을 수 있습니다 `Resources` 컬렉션 페이지에서 다른 요소에 합니다. 예를 들어를에 추가 시도 `StackLayout` 이 예에서:

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

단추의 텍스트 색은 파란색 이제 알 수 있습니다. XAML 파서가 발견할 때마다 기본적으로, 한 `StaticResource` 태그 확장 시각적 트리를 검색 하 고 첫 번째를 사용 하 여 `ResourceDictionary` 해당 키가 포함 된 발생 합니다.

Xamarin.Forms는 가장 일반적인 유형의 리소스 사전에 저장 된 개체 중 하나는 `Style`, 속성 설정의 컬렉션을 정의 하는 합니다. 스타일 문서에 설명 된 [스타일](~/xamarin-forms/user-interface/styles/index.md)합니다.

개발자가 XAML 새 궁금해 경우 시각적 요소를 같은 넣을 수 있습니다 하는 경우에 따라 `Label` 또는 `Button` 에 `ResourceDictionary`합니다. 분명히 가능 하지만 것으로 것은 적합을 확인 하지 않습니다. 용도 `ResourceDictionary` 개체를 공유 하는 것입니다. 시각적 요소를 공유할 수 없습니다. 동일한 인스턴스는 단일 페이지에 두 번 나타날 수 없습니다.

## <a name="the-xstatic-markup-extension"></a>X:static 태그 확장

해당 이름의 유사성 불구 하 고 `x:Static` 및 `StaticResource` 은 매우 다릅니다. `StaticResource` 하는 동안 리소스 사전에서 개체를 반환 합니다. `x:Static` 다음 중 하나에 액세스 합니다.

- 공용 정적 필드
- 공용 정적 속성
- 공용 상수 필드 
- 열거형 멤버입니다. 

`StaticResource` 리소스 사전 정의 하는 XAML 구현에서 태그 확장은 지원 동안 `x:Static` 은 XAML의 내장 부분으로는 `x` 접두사를 보여 줍니다.

다음은 몇 가지 예를 보여 주는 방법을 `x:Static` 정적 필드 및 열거형 멤버를 명시적으로 참조할 수 있습니다.

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="{x:Static LayoutOptions.Start}"
       HorizontalTextAlignment="{x:Static TextAlignment.Center}"
       TextColor="{x:Static Color.Aqua}" />
```

지금까지 매우 효과적인 아닙니다. 하지만 `x:Static` 태그 확장을 참조할 수도 정적 필드 또는 속성에서 사용자 고유의 코드입니다. 예를 들어, 다음은 프로그램 `AppConstants` 응용 프로그램을 통해 여러 페이지에는 데 사용할 수 있는 몇 가지 정적 필드를 포함 하는 클래스:

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

XAML 파일에서이 클래스의 정적 필드를 참조 하려면이 파일이 있는 XAML 파일 내에서 표시 하는 방법이 필요 합니다. XML 네임 스페이스 선언으로이 작업을 수행 합니다.

표준 Xamarin.Forms XAML 서식 파일의 일부로 생성 된 XAML 파일에 두 개의 XML 네임 스페이스 선언을 포함 있는지 회수: Xamarin.Forms 클래스와 다른 태그 및 특성을 XAML 내장 함수를 참조 하는 것에 대 한 액세스를 위한 하나:

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

다른 클래스에 액세스 하려면 추가 XML 네임 스페이스 선언이 필요 합니다. 각 추가 XML 네임 스페이스 선언을 새 접두사를 정의합니다. 클래스에 액세스 하려면 로컬 공유 응용 프로그램 PCL에 같은 `AppConstants`, XAML 프로그래머 접두사 자주 사용 하 여 `local`합니다. 네임 스페이스 선언 하기 위해 CLR (공용 언어 런타임) 네임 스페이스 이름 라고도.NET 네임 스페이스 이름,이 C#에 표시 되는 이름을 지정 해야 `namespace` 정의 또는 `using` 지시문:

```csharp
xmlns:local="clr-namespace:XamlSamples"
```

PCL 참조 하는 모든 어셈블리에서.NET 네임 스페이스에 대 한 XML 네임 스페이스 선언을 정의할 수도 있습니다. 예를 들어, 다음은 한 `sys` 표준.NET에 대 한 접두사 `System` 에 있는 네임 스페이스는 **mscorlib** "Microsoft 일반적인 개체 런타임 라이브러리에 대 한" 한 번 서 하지만 이제 "다국어 표준를 의미 하는 어셈블리를 공통 개체 런타임 Library "입니다. 다른 어셈블리 이기 때문에 지정 해야 하는 어셈블리 이름이 경우 **mscorlib**:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

키워드 `clr-namespace` 뒤에 콜론와 뒤에 오는 세미콜론 키워드.NET 네임 스페이스 이름을 `assembly`에 등호와 어셈블리 이름입니다.

예, 뒤에 콜론 `clr-namespace` 등호 기호 뒤에 오는 하지만 `assembly`합니다. 구문은이 방식으로 의도적으로 정의: 가장 XML 네임 스페이스 선언을 URI 체계 이름 같은 시작 하는 URI 참조 `http`는 뒤에 항상 콜론입니다. `clr-namespace` 이 문자열의 일부는 규칙을 모방 하기 위함입니다.

이 네임 스페이스 선언을 모두에 포함 된는 **StaticConstantsPage** 샘플. 에 `BoxView` 차원으로 설정 되어 `Math.PI` 및 `Math.E`, 100 배 하지만 크기 조정 된:

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

결과의 크기 `BoxView` 화면을 기준으로 플랫폼에 따라 다릅니다.

 [ ![](xaml-markup-extensions-images/staticconstants.png "X:static 태그 확장을 사용 하 여 컨트롤")](xaml-markup-extensions-images/staticconstants-large.png "X:static 태그 확장을 사용 하 여 컨트롤")

## <a name="other-standard-markup-extensions"></a>다른 표준 태그 확장

여러 가지 태그 확장은 XAML에 내장 및 Xamarin.Forms XAML 파일에서 지원 합니다. 이 중 일부를 매우 자주 사용 되지 않지만 필요할 때 필수:

-  가 속성이 있는 비-경우 `null` 값 수 있지만 기본적으로이 속성을 설정 하려면 `null`로 설정 된 `{x:Null}` 태그 확장 합니다.
-  속성 형식이 면 `Type`에 할당할 수 있습니다는 `Type` 개체 태그 확장을 사용 하 여 `{x:Type someClass}`합니다.
-  XAML에 사용 하 여 배열을 정의할 수 있습니다는 `x:Array` 태그 확장 합니다. 이 태그 확장은 라는 필수 특성이 `Type` 배열에 있는 요소의 형식을 나타내는입니다.
- `Binding` 태그 확장에 대해서는 설명 [4 부입니다. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)합니다.

## <a name="the-constraintexpression-markup-extension"></a>ConstraintExpression 태그 확장

태그 확장 속성을 가질 수 있지만 같은 XML 특성이 설정 되지 않습니다. 태그 확장에서 속성 설정을 쉼표로 구분 하 여에 나타나고 인용 부호 제외 중괄호 안에 있습니다.

명명 된 Xamarin.Forms 태그 확장을 통해 설명할 수이 `ConstraintExpression`,으로 사용 되는 `RelativeLayout` 클래스입니다. 상수 또는 부모 또는 다른 명명 된 보기에 상대적인 위치 또는 자식 뷰의 크기를 지정할 수 있습니다. 구문은 `ConstraintExpression` 위치 또는 사용 하 여 보기의 크기를 설정할 수 있습니다는 `Factor` 다른 뷰의 속성 및 한 번 `Constant`합니다. 모든 항목 보다 더 복잡 한 코드가 필요합니다.

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

아마도이 샘플에서 수행 해야 하는 가장 중요 한과 태그 확장의 구문: 인용 부호 제외 태그 확장의 중괄호 안에 나타나야 합니다. XAML 파일에서 태그 확장을 입력할 때 속성의 값을 따옴표로 묶어야 하는 합니다. 난관!

다음은 실행 중인 프로그램입니다.

[ ![](xaml-markup-extensions-images/relativelayout.png "제약 조건을 사용 하는 상대 레이아웃")](xaml-markup-extensions-images/relativelayout-large.png "제약 조건을 사용 하는 상대 레이아웃")

## <a name="summary"></a>요약

XAML 파일에 대 한 중요 한 지원을 제공 하는 여기에 표시 된 XAML 태그 확장 합니다. 아마도 가장 중요 한 XAML 태그 확장은 하지만 `Binding`,이 시리즈의 다음 부분에 나와 있는 [4 부입니다. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)합니다.



## <a name="related-links"></a>관련 링크

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [1 부입니다. XAML 시작](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [2 부 합니다. 필수 XAML 구문](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [4 부입니다. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [5 단계입니다. MVVM에 데이터 바인딩](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
