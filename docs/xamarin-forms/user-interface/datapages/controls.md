---
title: DataPages 컨트롤 참조
description: 이 문서에서는 Xamarin.Forms DataPages NuGet 패키지에서 사용할 수 있는 컨트롤을 소개 합니다.
ms.prod: xamarin
ms.assetid: 891615D0-E8BD-4ACC-A7F0-4C3725FBCC31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: c907d55f09d334e167c831a19f9d0edc4c97732f
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243131"
---
# <a name="datapages-controls-reference"></a>DataPages 컨트롤 참조

![](~/media/shared/preview.png "이 API는 현재 미리 보기")

> [!IMPORTANT]
> DataPages 필요는 [Xamarin.Forms 테마](~/xamarin-forms/user-interface/themes/index.md) 렌더링에 대 한 참조입니다.


Xamarin.Forms DataPages Nuget 다양 한 데이터 원본 바인딩에 사용할 수 있는 컨트롤을 포함 합니다.

XAML에서 이러한 컨트롤을 사용 하려면 네임 스페이스 포함 되었습니다. 확인, 예를 들어 참조는 `xmlns:pages` 아래의 선언 합니다.

```xaml
<ContentPage
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:pages="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
    x:Class="DataPagesDemo.Detail">
```

아래 예에는 포함 `DynamicResource` 작동 하려면 프로젝트의 리소스 사전에 존재 해야 하는 참조 합니다. 또한 작성 하는 방법의 예는 [사용자 지정 컨트롤](#custom)

## <a name="built-in-controls"></a>기본 제공 컨트롤

* [HeroImage](#heroimage)
* [ListItem](#listitem)

<a name="heroimage" />

### <a name="heroimage"></a>HeroImage

`HeroImage` 컨트롤에 4 개의 속성이 있습니다.

* 텍스트
* 자세히
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

![](controls-images/heroimage-light-android.png "Android에서 HeroImage 컨트롤") ![ ] (controls-images/heroimage-dark-android.png "Android에서 HeroImage 컨트롤")

**iOS**

![](controls-images/heroimage-light-ios.png "IOS에서 HeroImage 컨트롤") ![ ] (controls-images/heroimage-dark-ios.png "iOS에서 HeroImage 컨트롤")


<a name="listitem" />

### <a name="listitem"></a>ListItem

그러나 `ListItem` 컨트롤의 레이아웃은 네이티브 iOS와 Android 목록 또는 테이블 행,도 사용할 수 있습니다를 일반 보기로 합니다. 예제 아래에 있는 코드는 표시 내에서 호스팅되는 `StackLayout`, 하지만 scolling 데이터 바인딩된 목록 컨트롤에도 사용 될 수 있습니다.

5 개의 속성이 있습니다.

* 제목
* 자세히
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

이러한 스크린샷 표시는 `ListItem` iOS 및 Android 플랫폼 둘 다는 밝은 테마와 어두운 테마를 사용 하 여:

**Android**

![](controls-images/listitem-light-android.png "Android에서 ListItem 컨트롤") ![ ] (controls-images/listitem-dark-android.png "Android에서 ListItem 컨트롤")

**iOS**

![](controls-images/listitem-light-ios.png "IOS에서 ListItem 컨트롤") ![ ] (controls-images/listitem-dark-ios.png "iOS에서 ListItem 컨트롤")


## <a name="custom-control-example"></a>사용자 지정 컨트롤 예제

이 사용자 지정의 목표 `CardView` 컨트롤은 네이티브 Android CardView 유사 하 게 합니다.

세 가지 속성이 포함 됩니다.

* 텍스트
* 자세히
* ImageSource

목표는 사용자 지정 컨트롤 아래 코드 버리면입니다 (을 사용자 지정 `xmlns:local` 필요는 현재 어셈블리를 참조 하는):

```xaml
<local:CardView
  ImageSource="{ DynamicResource CardViewImage }"
  Text="CardView Text"
  Detail="CardView Detail"
/>
```

에 해당 하는 기본 제공 밝은 테마와 어두운 테마 색을 사용 하 여 아래 스크린샷 처럼 같아야 합니다.

**Android**

![](controls-images/cardview-light-android.png "Android에서 CardView 사용자 지정 컨트롤") ![ ] (controls-images/cardview-dark-android.png "Android에서 CardView 사용자 지정 컨트롤")

**iOS**

![](controls-images/cardview-light-ios.png "Ios 사용자 지정 컨트롤 CardView") ![ ] (controls-images/cardview-dark-ios.png "iOS에서 CardView 사용자 지정 컨트롤")

<a name="custom" />

### <a name="building-the-custom-cardview"></a>사용자 지정 CardView 작성

1. [DataView 하위 클래스](#1)
2. [글꼴, 레이아웃 및 여백 정의](#2)
3. [컨트롤의 자식에 대 한 스타일 만들기](#3)
4. [컨트롤 레이아웃 템플릿 만들기](#4)
5. [테마별 리소스 추가](#5)
6. [ControlTemplate CardView 클래스에 대 한 설정](#6)
7. [페이지에 컨트롤 추가](#7)

<a name="1" />

#### <a name="1-dataview-subclass"></a>1. DataView 하위 클래스

C# 하위 클래스의 `DataView` 컨트롤에 대 한 바인딩 가능 속성을 정의 합니다.

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

<a name="2" />

#### <a name="2-define-font-layout-and-margins"></a>2. 글꼴, 레이아웃 및 여백 정의

컨트롤 디자이너에서 사용자 지정 컨트롤에 대 한 사용자 인터페이스 디자인의 일부로 이러한 값을 파악 합니다. 플랫폼별 사양 필요한 경우를 들 여 `OnPlatform` 요소를 사용 합니다.

일부 값을 참조 하는 참고 `StaticResource`s-여기에 정의 될 [5 단계](#5)합니다.

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

<a name="3" />

#### <a name="3-create-styles-for-the-controls-children"></a>3. 컨트롤의 자식에 대 한 스타일 만들기

에 대 한 정보를 사용자 지정 컨트롤에서 사용 되는 자식을 만들기 위해 정의 된 모든 요소를 참조:

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

<a name="4" />

#### <a name="4-create-the-control-layout-template"></a>4. 컨트롤 레이아웃 템플릿 만들기

사용자 지정 컨트롤의 시각적 디자인 위에 정의 된 리소스를 사용 하 여 컨트롤 서식 파일에서 명시적으로 선언 됩니다.

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

<a name="5" />

#### <a name="5-add-the-theme-specific-resources"></a>5. 테마별 리소스 추가

사용자 지정 컨트롤 이므로 리소스 사전을 사용 하는 테마를 일치 하는 리소스를 추가 합니다.

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

<a name="6" />

#### <a name="6-set-the-controltemplate-for-the-cardview-class"></a>6. ControlTemplate CardView 클래스에 대 한 설정

끝으로,에서 만든 C# 클래스를 확인 [1 단계](#1) 에 정의 된 컨트롤 템플릿을 사용 하 여 [4 단계](#4) 를 사용 하는 `Style` `Setter` 요소

```xml
<Style TargetType="local:CardView">
    <Setter Property="ControlTemplate" Value="{ StaticResource CardViewControlControlTemplate }" />
  ... some custom styling omitted
  <Setter Property="BackgroundColor" Value="{ StaticResource CardViewBackgroundColor }" />
</Style>
```

<a name="7" />

#### <a name="7-add-the-control-to-a-page"></a>7. 페이지에 컨트롤 추가

`CardView` 컨트롤을 페이지에 추가할 수 있습니다. 다음 예제에서는 표시에서 호스팅되는 `StackLayout`:

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
