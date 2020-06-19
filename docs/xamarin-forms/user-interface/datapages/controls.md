---
title: DataPages 컨트롤 참조
description: 이 문서에서는 datapages NuGet 패키지에서 사용할 수 있는 컨트롤을 소개 합니다 Xamarin.Forms .
ms.prod: xamarin
ms.assetid: 891615D0-E8BD-4ACC-A7F0-4C3725FBCC31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 86b526fff305b195221aca3fb6a86ad0823cb145
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84569402"
---
# <a name="datapages-controls-reference"></a>DataPages 컨트롤 참조

![](~/media/shared/preview.png "This API is currently in preview")

> [!IMPORTANT]
> DataPages Xamarin.Forms 를 렌더링 하려면 테마 참조가 필요 합니다. 여기에는를 설치 하는 작업이 포함 됩니다 [ Xamarin.Forms . 테마. 기본](https://www.nuget.org/packages/Xamarin.Forms.Theme.Base/) NuGet 패키지를 프로젝트에 삽입 하 고 다음을 수행 [ Xamarin.Forms 합니다. 테마. Light](https://www.nuget.org/packages/Xamarin.Forms.Theme.Light/) 또는 [ Xamarin.Forms . 테마. 짙은](https://www.nuget.org/packages/Xamarin.Forms.Theme.Dark/) NuGet 패키지.

Xamarin.FormsDatapages NuGet에는 데이터 소스 바인딩을 활용할 수 있는 여러 컨트롤이 포함 되어 있습니다.

XAML에서 이러한 컨트롤을 사용 하려면 네임 스페이스가 포함 되어 있는지 확인 합니다 (예 `xmlns:pages` : 아래 선언 참조).

```xaml
<ContentPage
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:pages="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
    x:Class="DataPagesDemo.Detail">
```

아래 예제에는 `DynamicResource` 작업을 수행 하기 위해 프로젝트의 리소스 사전에 있어야 하는 참조가 포함 되어 있습니다. [사용자 지정 컨트롤](#custom-control-example)을 빌드하는 방법에 대 한 예제도 있습니다.

## <a name="built-in-controls"></a>기본 제공 컨트롤

* [HeroImage](#heroimage)
* [ListItem](#listitem)

### <a name="heroimage"></a>HeroImage

컨트롤에는 `HeroImage` 네 가지 속성이 있습니다.

* 텍스트
* 세부 정보
* ImageSource
* 양상

```xaml
<pages:HeroImage
    ImageSource="{ DynamicResource HeroImageImage }"
    Text="Keith Ballinger"
    Detail="Xamarin"
/>
```

**Android**

![](controls-images/heroimage-light-android.png "Android의 HeroImage 컨트롤") ![](controls-images/heroimage-dark-android.png "Android의 HeroImage 컨트롤")

**iOS**

![](controls-images/heroimage-light-ios.png "IOS의 HeroImage 컨트롤") ![](controls-images/heroimage-dark-ios.png "IOS의 HeroImage 컨트롤")

### <a name="listitem"></a>ListItem

`ListItem`컨트롤의 레이아웃은 기본 iOS 및 Android 목록 또는 테이블 행과 유사 하지만 일반 보기로 사용 될 수도 있습니다. 아래 예제 코드에서는 안에 호스팅된 것 `StackLayout` 으로 표시 되지만 데이터 바인딩된 scolling list 컨트롤에도 사용할 수 있습니다.

5 개의 속성이 있습니다.

* 제목
* 세부 정보
* ImageSource
* PlaceholdImageSource
* 양상

```xaml
<StackLayout Spacing="0">
    <pages:ListItemControl
        Detail="Xamarin"
        ImageSource="{ DynamicResource UserImage }"
        Title="Miguel de Icaza"
        PlaceholdImageSource="{ DynamicResource IconImage }"
    />
```

이러한 스크린샷은 `ListItem` 밝은 테마와 어두운 테마를 모두 사용 하 여 iOS 및 Android 플랫폼에 대 한을 보여 줍니다.

**Android**

![](controls-images/listitem-light-android.png "Android의 ListItem 컨트롤") ![](controls-images/listitem-dark-android.png "Android의 ListItem 컨트롤")

**iOS**

![](controls-images/listitem-light-ios.png "IOS의 ListItem 컨트롤") ![](controls-images/listitem-dark-ios.png "IOS의 ListItem 컨트롤")

## <a name="custom-control-example"></a>사용자 지정 컨트롤 예제

이 사용자 지정 컨트롤의 목표는 `CardView` 네이티브 Android CardView와 비슷합니다.

다음 세 가지 속성을 포함 합니다.

* 텍스트
* 세부 정보
* ImageSource

목표는 아래 코드와 같이 표시 되는 사용자 지정 컨트롤입니다 `xmlns:local` . 현재 어셈블리를 참조 하는 사용자 지정이 필요 합니다.

```xaml
<local:CardView
  ImageSource="{ DynamicResource CardViewImage }"
  Text="CardView Text"
  Detail="CardView Detail"
/>
```

기본 제공 되는 밝은 테마와 어두운 테마에 해당 하는 색을 사용 하 여 아래의 스크린샷 처럼 표시 됩니다.

**Android**

![](controls-images/cardview-light-android.png "Android의 CardView 사용자 지정 컨트롤") ![](controls-images/cardview-dark-android.png "Android의 CardView 사용자 지정 컨트롤")

**iOS**

![](controls-images/cardview-light-ios.png "IOS의 CardView 사용자 지정 컨트롤") ![](controls-images/cardview-dark-ios.png "IOS의 CardView 사용자 지정 컨트롤")

### <a name="building-the-custom-cardview"></a>사용자 지정 CardView 빌드

1. [DataView 하위 클래스](#1-dataview-subclass)
2. [글꼴, 레이아웃 및 여백 정의](#2-define-font-layout-and-margins)
3. [컨트롤의 자식에 대 한 스타일 만들기](#3-create-styles-for-the-controls-children)
4. [컨트롤 레이아웃 템플릿 만들기](#4-create-the-control-layout-template)
5. [테마별 리소스 추가](#5-add-the-theme-specific-resources)
6. [CardView 클래스에 대 한 ControlTemplate 설정](#6-set-the-controltemplate-for-the-cardview-class)
7. [페이지에 컨트롤 추가](#7-add-the-control-to-a-page)

#### <a name="1-dataview-subclass"></a>1. DataView 하위 클래스

의 c # 서브 클래스는 `DataView` 컨트롤에 대 한 바인딩 가능한 속성을 정의 합니다.

```csharp
public class CardView : DataView
{
    public static readonly BindableProperty TextProperty =
        BindableProperty.Create ("Text", typeof (string), typeof (CardView), null, BindingMode.TwoWay);

    public string Text
    {
        get { return (string)GetValue (TextProperty); }
        set { SetValue (TextProperty, value); }
    }

    public static readonly BindableProperty DetailProperty =
        BindableProperty.Create ("Detail", typeof (string), typeof (CardView), null, BindingMode.TwoWay);

    public string Detail
    {
        get { return (string)GetValue (DetailProperty); }
        set { SetValue (DetailProperty, value); }
    }

    public static readonly BindableProperty ImageSourceProperty =
        BindableProperty.Create ("ImageSource", typeof (ImageSource), typeof (CardView), null, BindingMode.TwoWay);

    public ImageSource ImageSource
    {
        get { return (ImageSource)GetValue (ImageSourceProperty); }
        set { SetValue (ImageSourceProperty, value); }
    }

    public CardView()
    {
    }
}
```

#### <a name="2-define-font-layout-and-margins"></a>2. 글꼴, 레이아웃 및 여백 정의

컨트롤 디자이너는 사용자 지정 컨트롤에 대 한 사용자 인터페이스 디자인의 일부로 이러한 값을 파악 합니다. 플랫폼별 사양이 필요한 경우 `OnPlatform` 요소가 사용 됩니다.

일부 값은 s를 참조 `StaticResource` 합니다. 이러한 값은 [5 단계](#5-add-the-theme-specific-resources)에서 정의 됩니다.

```xml
<!-- CARDVIEW FONT SIZES -->
<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewTextFontSize">
        <On Platform="iOS, Android" Value="15" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewDetailFontSize">
        <On Platform="iOS, Android" Value="13" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewTextTextColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewTextTextColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewTextTextColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="Thickness"    x:Key="CardViewTextlMargin">
        <On Platform="iOS" Value="12,10,12,4" />
        <On Platform="Android" Value="20,0,20,5" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewDetailTextColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewDetailTextColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewDetailTextColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="Thickness"    x:Key="CardViewDetailMargin">
        <On Platform="iOS" Value="12,0,10,12" />
        <On Platform="Android" Value="20,0,20,20" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewBackgroundColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewBackgroundColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewBackgroundColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewShadowSize">
        <On Platform="iOS" Value="2" />
        <On Platform="Android" Value="5" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewCornerRadius">
        <On Platform="iOS" Value="0" />
        <On Platform="Android" Value="4" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewShadowColor">
        <On Platform="iOS, Android" Value="#CDCDD1" />
</OnPlatform>
```

#### <a name="3-create-styles-for-the-controls-children"></a>3. 컨트롤의 자식에 대 한 스타일 만들기

에 대해 정의 된 모든 요소를 참조 하 여 사용자 지정 컨트롤에 사용 될 자식을 만듭니다.

```xml
<!-- EXPLICIT STYLES (will be Classes) -->
<Style TargetType="Label" x:Key="CardViewTextStyle">
    <Setter Property="FontSize" Value="{ StaticResource CardViewTextFontSize }" />
    <Setter Property="TextColor" Value="{ StaticResource CardViewTextTextColor }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="Margin" Value="{ StaticResource CardViewTextlMargin }" />
    <Setter Property="HorizontalTextAlignment" Value="Start" />
</Style>

<Style TargetType="Label" x:Key="CardViewDetailStyle">
    <Setter Property="HorizontalTextAlignment" Value="Start" />
    <Setter Property="TextColor" Value="{ StaticResource CardViewDetailTextColor }" />
    <Setter Property="FontSize" Value="{ StaticResource CardViewDetailFontSize }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="Margin" Value="{ StaticResource CardViewDetailMargin }" />
</Style>

<Style TargetType="Image" x:Key="CardViewImageImageStyle">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="Center" />
    <Setter Property="WidthRequest" Value="220"/>
    <Setter Property="HeightRequest" Value="165"/>
</Style>
```

#### <a name="4-create-the-control-layout-template"></a>4. 컨트롤 레이아웃 템플릿 만들기

사용자 지정 컨트롤의 시각적 디자인은 위에 정의 된 리소스를 사용 하 여 컨트롤 템플릿에서 명시적으로 선언 됩니다.

```xml
<!--- CARDVIEW -->
<ControlTemplate x:Key="CardViewControlControlTemplate">
  <StackLayout
    Spacing="0"
    BackgroundColor="{ TemplateBinding BackgroundColor }"
  >

    <!-- CARDVIEW IMAGE -->
    <Image
      Source="{ TemplateBinding ImageSource }"
      HorizontalOptions="FillAndExpand"
      VerticalOptions="StartAndExpand"
      Aspect="AspectFill"
      Style="{ StaticResource CardViewImageImageStyle }"
    />

    <!-- CARDVIEW TEXT -->
    <Label
      Text="{ TemplateBinding Text }"
      LineBreakMode="WordWrap"
      VerticalOptions="End"
      Style="{ StaticResource CardViewTextStyle }"
    />

    <!-- CARDVIEW DETAIL -->
    <Label
      Text="{ TemplateBinding Detail }"
      LineBreakMode="WordWrap"
      VerticalOptions="End"
      Style="{ StaticResource CardViewDetailStyle }" />

  </StackLayout>

</ControlTemplate>
```

#### <a name="5-add-the-theme-specific-resources"></a>5. 테마별 리소스 추가

사용자 지정 컨트롤 이므로 리소스 사전을 사용 하는 테마와 일치 하는 리소스를 추가 합니다.

##### <a name="light-theme-colors"></a>밝은 테마 색

```xaml
<Color x:Key="iOSCardViewBackgroundColor">#FFFFFF</Color>
<Color x:Key="AndroidCardViewBackgroundColor">#FFFFFF</Color>

<Color x:Key="AndroidCardViewTextTextColor">#030303</Color>
<Color x:Key="iOSCardViewTextTextColor">#030303</Color>

<Color x:Key="AndroidCardViewDetailTextColor">#8F8E94</Color>
<Color x:Key="iOSCardViewDetailTextColor">#8F8E94</Color>
```

##### <a name="dark-theme-colors"></a>어두운 테마 색

```xaml
<!-- CARD VIEW COLORS -->
            <Color x:Key="iOSCardViewBackgroundColor">#404040</Color>
            <Color x:Key="AndroidCardViewBackgroundColor">#404040</Color>

            <Color x:Key="AndroidCardViewTextTextColor">#FFFFFF</Color>
            <Color x:Key="iOSCardViewTextTextColor">#FFFFFF</Color>

            <Color x:Key="AndroidCardViewDetailTextColor">#B5B4B9</Color>
            <Color x:Key="iOSCardViewDetailTextColor">#B5B4B9</Color>
```

#### <a name="6-set-the-controltemplate-for-the-cardview-class"></a>6. CardView 클래스에 대 한 ControlTemplate 설정

마지막으로 [1 단계](#1-dataview-subclass) 에서 만든 c # 클래스가 요소를 사용 하 여 [4 단계](#4-create-the-control-layout-template) 에서 정의 된 컨트롤 템플릿을 사용 하는지 확인 합니다. `Style` `Setter`

```xml
<Style TargetType="local:CardView">
    <Setter Property="ControlTemplate" Value="{ StaticResource CardViewControlControlTemplate }" />
  ... some custom styling omitted
  <Setter Property="BackgroundColor" Value="{ StaticResource CardViewBackgroundColor }" />
</Style>
```

#### <a name="7-add-the-control-to-a-page"></a>7. 페이지에 컨트롤을 추가 합니다.

`CardView`이제 컨트롤을 페이지에 추가할 수 있습니다. 아래 예제에서는에서 호스팅되는 것을 보여 줍니다 `StackLayout` .

```xaml
<StackLayout Spacing="0">
  <local:CardView
    Margin="12,6"
    ImageSource="{ DynamicResource CardViewImage }"
    Text="CardView Text"
    Detail="CardView Detail"
  />
</StackLayout>

```
