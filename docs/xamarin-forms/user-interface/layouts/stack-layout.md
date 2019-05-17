---
title: Xamarin.Forms StackLayout
description: 이 문서에서는 Xamarin.Forms StackLayout 클래스를 사용 하 여 한 차원에 대해 뷰 컬렉션을 제공 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 4e462346c2c0130c972d098aa2c988bdd61fc360
ms.sourcegitcommit: 0c823f5439f4279a35af23dd466e7a0483e65d50
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65804903"
---
# <a name="xamarinforms-stacklayout"></a>Xamarin.Forms StackLayout

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)

[StackLayout](xref:Xamarin.Forms.StackLayout) 가로 또는 세로로 1 차원 줄 ("스택")에 뷰를 구성 합니다. 뷰는 `StackLayout` 레이아웃 옵션을 사용 하 여 레이아웃에서 공간을 기준으로 크기 조정 될 수 있습니다. 위치 보기는 레이아웃과 뷰의 레이아웃 옵션에 추가 된 순서에 따라 결정 됩니다.

[![](stack-layout-images/layouts-sml.png "Xamarin.Forms 레이아웃")](stack-layout-images/layouts.png#lightbox "Xamarin.Forms 레이아웃")

## <a name="purpose"></a>용도

`StackLayout` 다른 보기 보다 덜 복잡 합니다. 뷰를 추가 하 여 간단한 선형 인터페이스를 만들 수 있습니다는 `StackLayout`, 및 중첩 하 여 만든 더 복잡 한 인터페이스입니다.

## <a name="usage--behavior"></a>사용량 및 동작

### <a name="spacing"></a>간격

기본적으로 `StackLayout` 뷰 사이의 6px 여백을 추가 합니다. 제어 또는 없습니다 여백을 설정 하 여 사용 하도록 설정 될 수 있습니다는 `Spacing` StackLayout 속성입니다. 다음 간격 및 다른 간격 옵션의 효과 설정 하는 방법에 설명 합니다.

XAML:

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

![](stack-layout-images/spacing-zero.png "간격을 사용 하 여 StackLayout = 0")

10의 간격:

![](stack-layout-images/spacing-ten.png "간격을 사용 하 여 StackLayout = 10")

### <a name="sizing"></a>크기 조정

StackLayout의 뷰 크기 높이 너비를 요청 및 레이아웃 옵션에 따라 달라 집니다. `StackLayout` 안쪽 여백을 적용 됩니다. 다음 `LayoutOption`의뷰 레이아웃에서 사용할 수 있는 만큼의 공간을 차지 하도록 하면:

- **CenterAndExpand** &ndash; 보기 레이아웃 내에서 가운데에 및 레이아웃으로 지정 하는 만큼의 공간을 차지 하도록 확장 합니다.
- **EndAndExpand** &ndash; 보기 레이아웃 (아래쪽 또는 오른쪽 경계)의 끝에 배치 하 고 레이아웃으로 지정 하는 만큼의 공간을 차지 하도록 확장 합니다.
- **FillAndExpand** &ndash; 채우지는 수 있고 레이아웃으로 지정 하는 만큼의 공간을 차지 되도록 보기를 배치 합니다.
- **StartAndExpand** &ndash; 레이아웃의 시작 부분에 보기를 배치 하 고 부모 나옴 만큼의 공간을 차지 합니다.

자세한 내용은 [확장](~/xamarin-forms/user-interface/layouts/layout-options.md#expansion)합니다.

### <a name="positioning"></a>위치 지정

뷰는 StackLayout에 배치 될 수 있습니다 및 크기를 사용 하 여 `LayoutOptions`입니다. 각 뷰를 지정할 수 있습니다 `VerticalOptions` 및 `HorizontalOptions`를 정의 하는 방법 보기는 배치 자체 레이아웃을 기준으로 합니다. 미리 정의 된 다음 `LayoutOptions` 사용할 수 있습니다.

- **Center** &ndash; 보기 레이아웃 내에서 가운데에 맞춥니다.
- **최종** &ndash; 보기 레이아웃 (아래쪽 또는 오른쪽 경계)의 끝에 배치 합니다.
- **채울** &ndash; 채우지는 포함 되도록 뷰를 배치 합니다.
- **시작** &ndash; 뷰 레이아웃의 시작 부분에 배치 합니다.

다음 코드는 레이아웃 옵션 설정 방법을 보여 줍니다.

XAML:

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

자세한 내용은 [맞춤](~/xamarin-forms/user-interface/layouts/layout-options.md#alignment)합니다.

## <a name="exploring-a-complex-layout"></a>복잡 한 레이아웃을 탐색합니다.

각 레이아웃의 강점 및 약점 특정 레이아웃을 만들에 있습니다. 이 문서 시리즈에서는 레이아웃을 전체 세 가지 다른 레이아웃을 사용 하 여 구현 동일한 페이지 레이아웃을 사용 하 여 샘플 앱을 만들었습니다.

다음 XAML을 고려해 야 합니다.

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

위 코드 다음의 레이아웃 발생합니다.

![](stack-layout-images/stack.png "복잡 한 StackLayout")

`StackLayouts`s 중첩 된 경우에 따라 레이아웃 중첩 수 있으므로 동일한 레이아웃 내에서 모든 요소를 제공 하는 것 보다 쉽습니다. 또한 알 수 있으므로 `StackLayout` 페이지 하지 있는 레이아웃 기능이의 일부 페이지에 있는 다른 레이아웃에 겹치는 항목을 지원 하지 않습니다.



## <a name="related-links"></a>관련 링크

- [LayoutOptions](~/xamarin-forms/user-interface/layouts/layout-options.md)
- [레이아웃 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble 예제 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
