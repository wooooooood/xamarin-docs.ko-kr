---
title: Xamarin.Forms StackLayout
description: 이 문서는 뷰 컬렉션을 하나의 차원에 대해 제공 하는 Xamarin.Forms StackLayout 클래스를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 6e278c466c352ad19575cd3a84d6e38e14ec2587
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244599"
---
# <a name="xamarinforms-stacklayout"></a>Xamarin.Forms StackLayout

`StackLayout` 가로 또는 세로로 1 차원 줄 ("스택")에 뷰를 구성합니다. 뷰는 `StackLayout` 크기를 조정할 수 레이아웃 옵션을 사용 하 여 레이아웃에서 공간을 기초로 합니다. 위치 지정 보기는 레이아웃과 뷰의 레이아웃 옵션에 추가 된 순서에 따라 결정 됩니다.

[![](stack-layout-images/layouts-sml.png "Xamarin.Forms 레이아웃")](stack-layout-images/layouts.png#lightbox "Xamarin.Forms 레이아웃")

## <a name="purpose"></a>용도

`StackLayout` 다른 보기 보다 덜 복잡 합니다. 방금 뷰를 추가 하 여 간단한 선형 인터페이스를 만들 수는 `StackLayout`, 및 중첩 하 여 만든 복잡 한 인터페이스입니다.

## <a name="usage--behavior"></a>사용 현황 및 동작

### <a name="spacing"></a>간격

기본적으로 `StackLayout` 뷰 사이의 6px 여백을 추가 합니다. 이 제어 하거나 수 없는 여백을 설정 하 여 사용 하도록 설정 된 `Spacing` StackLayout 속성입니다. 다음에서는 간격 및 다른 간격이 옵션의 효과 설정 하는 방법을 보여 줍니다.

Xaml의 경우:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.StackLayoutDemo"
Title="StackLayout Demo">
  <ContentPage.Content>
    <StackLayout Spacing="10" x:Name="layout">
      <Button Text="StackLayout" VerticalOptions="Start"
        HorizontalOptions="FillAndExpand" />
      <BoxView Color="Yellow" VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView Color="Green" VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView HeightRequest="75" Color="Blue" VerticalOptions="End"
        HorizontalOptions="FillAndExpand" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

C#:

```csharp
public class StackLayoutCode : ContentPage
{
  public StackLayoutCode ()
  {
    var layout = new StackLayout ();
    var button = new Button { Text = "StackLayout", VerticalOptions = LayoutOptions.Start,
     HorizontalOptions = LayoutOptions.FillAndExpand };
    var yellowBox = new BoxView { Color = Color.Yellow, VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var greenBox = new BoxView { Color = Color.Green, VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var blueBox = new BoxView { Color = Color.Blue, VerticalOptions = LayoutOptions.FillAndExpand,
      HorizontalOptions = LayoutOptions.FillAndExpand, HeightRequest = 75 };

    layout.Children.Add(button);
    layout.Children.Add(yellowBox);
    layout.Children.Add(greenBox);
    layout.Children.Add(blueBox);
    layout.Spacing = 10;
    Content = layout;
  }
}
```

간격 = 0.

![](stack-layout-images/spacing-zero.png "간격이 StackLayout = 0")

10의 간격:

![](stack-layout-images/spacing-ten.png "간격이 StackLayout = 10")

### <a name="sizing"></a>크기 조정

StackLayout에서 보기의 크기 높이 너비 요청와 레이아웃 옵션에 따라 달라 집니다. `StackLayout` 안쪽 여백을 적용 합니다. 다음 `LayoutOption`뷰 레이아웃에서 사용할 수 있는 만큼의 공간을 차지 하도록 하면 됩니다.

- **CenterAndExpand** &ndash; 레이아웃 내에서 보기 및 레이아웃으로 지정 하는 만큼의 공간을 차지 하도록 확장 됩니다.
- **EndAndExpand** &ndash; 레이아웃 (아래쪽 또는 오른쪽 경계)의 끝에 배치 됩니다. 보기 및 레이아웃으로 지정 하는 만큼의 공간을 차지 하도록 확장 됩니다.
- **FillAndExpand** &ndash; 채우지는 않으며 레이아웃으로 지정 하는 만큼의 공간을 차지 되도록 보기를 배치 합니다.
- **StartAndExpand** &ndash; 레이아웃의 시작 부분에 보기를 배치 하 고 부모 생깁니다 만큼의 공간을 차지 합니다.

자세한 내용은 참조 [확장](~/xamarin-forms/user-interface/layouts/layout-options.md#expansion)합니다.

### <a name="positioning"></a>위치 지정

뷰는 StackLayout에 배치 될 수 있습니다 및 크기를 사용 하 여 `LayoutOptions`합니다. 각 보기에 제공 될 수 있습니다 `VerticalOptions` 및 `HorizontalOptions`, 어떻게 뷰는 배치 레이아웃을 기준으로 정의 합니다. 다음 미리 정의 된 `LayoutOptions` 사용할 수 있습니다.

- **Center** &ndash; 레이아웃 내에서 보기.
- **최종** &ndash; 보기 레이아웃 (아래쪽 또는 오른쪽 경계)의 끝에 배치 합니다.
- **채우기** &ndash; 채우지는 되도록 보기를 배치 합니다.
- **시작** &ndash; 레이아웃의 시작 부분에 보기를 배치 합니다.

다음 코드에서는 레이아웃 옵션을 설정 합니다.

Xaml의 경우:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.StackLayoutDemo"
Title="StackLayout Demo">
  <ContentPage.Content>
    <StackLayout x:Name="layout">
      <Button VerticalOptions="Start"
        HorizontalOptions="FillAndExpand" />
      <BoxView VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView HeightRequest="75" VerticalOptions="End"
        HorizontalOptions="FillAndExpand" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

C#:

```csharp
public class StackLayoutCode : ContentPage
{
  public StackLayoutCode ()
  {
    var layout = new StackLayout ();
    var button = new Button { VerticalOptions = LayoutOptions.Start,
     HorizontalOptions = LayoutOptions.FillAndExpand };
    var oneBox = new BoxView { VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var twoBox = new BoxView {  VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var threeBox = new BoxView {  VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };

    layout.Children.Add(button);
    layout.Children.Add(oneBox);
    layout.Children.Add(twoBox);
    layout.Children.Add(threeBox);
    Content = layout;
  }
}
```

자세한 내용은 참조 [맞춤](~/xamarin-forms/user-interface/layouts/layout-options.md#alignment)합니다.

## <a name="exploring-a-complex-layout"></a>복잡 한 레이아웃을 탐색합니다.

사용할 수 있는 레이아웃의 각 특정 레이아웃을 만들 장단점을 포함. 이러한 일련의 레이아웃 문서 전체에서 샘플 응용 프로그램을 세 개의 다른 레이아웃을 사용 하 여 구현 동일한 페이지 레이아웃 작성 되었습니다.

다음 XAML을 고려 합니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.StackLayoutPage"
BackgroundColor="Maroon"
Title="StackLayouts">
    <ContentPage.Content>
    <ScrollView>
      <StackLayout Spacing="0" Padding="0" BackgroundColor="Maroon">
        <BoxView HorizontalOptions="FillAndExpand" HeightRequest="100"
          VerticalOptions="Start" Color="Gray" />
        <Button BorderRadius="30" HeightRequest="60" WidthRequest="60"
          BackgroundColor="Red" HorizontalOptions="Center" VerticalOptions="Start" />
        <StackLayout HeightRequest="100" VerticalOptions="Start" HorizontalOptions="FillAndExpand" Spacing="20" BackgroundColor="Maroon">
          <Label Text="User Name" FontSize="28" HorizontalOptions="Center"
            VerticalOptions="Center" FontAttributes="Bold" />
          <Entry Text="Bio + Hashtags" TextColor="White"
            BackgroundColor="Maroon" HorizontalOptions="FillAndExpand" VerticalOptions="CenterAndExpand" />
        </StackLayout>
        <StackLayout Orientation="Horizontal" HeightRequest="50" BackgroundColor="White" Padding="5">
          <StackLayout Spacing="0" BackgroundColor="White" Orientation="Horizontal" HorizontalOptions="Start">
            <BoxView BackgroundColor="Black" WidthRequest="40" HeightRequest="40"  HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
            <Label FontSize="14" TextColor="Black" Text="Accent Color" HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
          </StackLayout>
          <StackLayout Spacing="0" BackgroundColor="White" Orientation="Horizontal" HorizontalOptions="EndAndExpand">
            <BoxView BackgroundColor="Maroon" WidthRequest="40" HeightRequest="40" HorizontalOptions="Start" VerticalOptions="Center" />
            <Label FontSize="14" TextColor="Black" Text="Primary Color" HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
          </StackLayout>
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Age:" TextColor="White" HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100" />
          <Entry HorizontalOptions="FillAndExpand" Text="35" TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Interests:" TextColor="White"
          HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100" />
          <Entry HorizontalOptions="FillAndExpand" Text="Xamarin.Forms" TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Ask me about:" TextColor="White"
          HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100"/>
          <Entry HorizontalOptions="FillAndExpand" Text="Xamarin, C#, .NET, Mono..." TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
      </StackLayout>
    </ScrollView>
    </ContentPage.Content>
</ContentPage>

```

위의 코드는 다음과 같은 레이아웃 발생합니다.

![](stack-layout-images/stack.png "복잡 한 StackLayout")

다음에 유의 `StackLayouts`경우에 따라 레이아웃에 중첩 될 수 있으므로 동일한 레이아웃 내에서 모든 요소를 제공 하는 보다 쉽게 s 중첩 됩니다. 또한 알 수 있으므로 `StackLayout` 페이지 하지 않는 일부 레이아웃 유용한 찾았을 다른 레이아웃에 대 한 페이지에 겹치는 항목을 지원 하지 않습니다.



## <a name="related-links"></a>관련 링크

- [LayoutOptions](~/xamarin-forms/user-interface/layouts/layout-options.md)
- [레이아웃 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble 예제 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
