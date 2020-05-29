---
title: Xamarin.Forms프레임씩
description: Xamarin.FormsFrame 클래스는 색, 그림자 및 기타 옵션으로 구성할 수 있는 테두리를 사용 하 여 보기 또는 레이아웃을 래핑하는 데 사용 되는 레이아웃입니다.
ms.prod: ''
ms.assetId: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 42192111befbefda7e0f62b7691a8392c2828818
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137191"
---
# <a name="xamarinforms-frame"></a>Xamarin.Forms프레임씩

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-frame/)

Xamarin.Forms [`Frame`](xref:Xamarin.Forms.Frame) 클래스는 색, 그림자 및 기타 옵션을 사용 하 여 구성할 수 있는 테두리를 사용 하 여 뷰를 래핑하는 데 사용 되는 레이아웃입니다. 프레임은 일반적으로 컨트롤 주위에 테두리를 만드는 데 사용 되지만 보다 복잡 한 UI를 만드는 데 사용할 수 있습니다. 자세한 내용은 [고급 프레임 사용](#advanced-frame-usage)을 참조 하세요.

다음 스크린샷에서는 `Frame` iOS 및 Android의 컨트롤을 보여 줍니다.

[!["IOS 및 Android의 프레임 예제"](frame-images/frame-cropped.png)](frame-images/frame-full.png#lightbox "IOS 및 Android의 프레임 예제")

`Frame`클래스는 다음 속성을 정의 합니다.

* [`BorderColor`](xref:Xamarin.Forms.Frame.BorderColor)`Color`테두리의 색을 결정 하는 값입니다 `Frame` .
* [`CornerRadius`](xref:Xamarin.Forms.Frame.CornerRadius)`float`모퉁이의 둥근 반경을 결정 하는 값입니다.
* [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow)`bool`프레임에 그림자가 있는지 여부를 결정 하는 값입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉,이 `Frame` 데이터 바인딩의 대상이 될 수 있습니다.

> [!NOTE]
> `HasShadow`속성 동작은 플랫폼에 따라 다릅니다. 기본값은 `true` 모든 플랫폼에 있습니다. 그러나 UWP 드롭 그림자는 렌더링 되지 않습니다. 그림자는 Android 및 iOS에서 렌더링 되지만 iOS의 그림자는 더 어둡고 더 많은 공간을 차지 합니다.

## <a name="create-a-frame"></a>프레임 만들기

는 `Frame` XAML에서 인스턴스화될 수 있습니다. 기본 개체에는 `Frame` 흰색 배경, 그림자 및 테두리 없음이 있습니다. `Frame`일반적으로 개체는 다른 컨트롤을 래핑합니다. 다음 예제에서는 개체를 기본적으로 래핑하는 방법을 보여 줍니다 `Frame` `Label` .

```xaml
<Frame>
  <Label Text="Example" />
</Frame>
```

`Frame`코드에서를 만들 수도 있습니다.

```csharp
Frame defaultFrame = new Frame
{
    Content = new Label { Text = "Example" }
};
```

`Frame`XAML에서 속성을 설정 하 여 모퉁이가 둥근 모퉁이, 색이 지정 된 테두리 및 그림자를 사용 하 여 개체를 사용자 지정할 수 있습니다. 다음 예제에서는 사용자 지정 된 개체를 보여 줍니다 `Frame` .

```xaml
<Frame BorderColor="Orange"
       CornerRadius="10"
       HasShadow="True">
  <Label Text="Example" />
</Frame>
```

이러한 인스턴스 속성은 코드에서 설정할 수도 있습니다.

```csharp
Frame frame = new Frame
{
    BorderColor = Color.Orange,
    CornerRadius = 10,
    HasShadow = true,
    Content = new Label { Text = "Example" }
};
```

## <a name="advanced-frame-usage"></a>고급 프레임 사용

`Frame`클래스는에서 상속 `ContentView` 합니다. 즉, 개체를 비롯 한 모든 형식의 개체를 포함할 수 있습니다 `View` `Layout` . 이 기능을 사용 하면를 사용 하 여 `Frame` 카드와 같은 복잡 한 UI 개체를 만들 수 있습니다.

### <a name="create-a-card-with-a-frame"></a>프레임을 사용 하 여 카드 만들기

개체를 `Frame` `Layout` 개체와 같은 개체와 결합 하면 `StackLayout` 더 복잡 한 UI를 만들 수 있습니다. 다음 스크린샷은 개체를 사용 하 여 만든 예제 카드를 보여줍니다 `Frame` .

[!["프레임을 사용 하 여 만든 카드의 스크린샷"](frame-images/frame-card-cropped.png)](frame-images/frame-full.png#lightbox "프레임을 사용 하 여 만든 카드의 스크린샷")

다음 XAML은 클래스를 사용 하 여 카드를 만드는 방법을 보여 줍니다 `Frame` .

```xaml
<Frame BorderColor="Gray"
       CornerRadius="5"
       Padding="8">
  <StackLayout>
    <Label Text="Card Example"
           FontSize="Medium"
           FontAttributes="Bold" />
    <BoxView Color="Gray"
             HeightRequest="2"
             HorizontalOptions="Fill" />
    <Label Text="Frames can wrap more complex layouts to create more complex UI components, such as this card!"/>
  </StackLayout>
</Frame>
```

코드에서 카드를 만들 수도 있습니다.

```csharp
Frame cardFrame = new Frame
{
    BorderColor = Color.Gray,
    CornerRadius = 5,
    Padding = 8,
    Content = new StackLayout
    {
        Children =
        {
            new Label
            {
                Text = "Card Example",
                FontSize = Device.GetNamedSize(NamedSize.Medium, typeof(Label)),
                FontAttributes = FontAttributes.Bold
            },
            new BoxView
            {
                Color = Color.Gray,
                HeightRequest = 2,
                HorizontalOptions = LayoutOptions.Fill
            },
            new Label
            {
                Text = "Frames can wrap more complex layouts to create more complex UI components, such as this card!"
            }
        }
    }
};
```

### <a name="round-elements"></a>Round 요소

`CornerRadius`컨트롤의 속성을 `Frame` 사용 하 여 원 이미지를 만들 수 있습니다. 다음 스크린샷은 개체를 사용 하 여 만든 라운드 이미지의 예를 보여 줍니다 `Frame` .

[!["프레임을 사용 하 여 만든 원 이미지의 스크린샷"](frame-images/circle-image-cropped.png)](frame-images/frame-full.png#lightbox "프레임을 사용 하 여 만든 원 이미지의 스크린샷")

다음 XAML은 XAML로 원 이미지를 만드는 방법을 보여 줍니다.

```xaml
<Frame Margin="10"
       BorderColor="Black"
       CornerRadius="50"
       HeightRequest="60"
       WidthRequest="60"
       IsClippedToBounds="True"
       HorizontalOptions="Center"
       VerticalOptions="Center">
  <Image Source="outdoors.jpg"
         Aspect="AspectFill"
         Margin="-20"
         HeightRequest="100"
         WidthRequest="100" />
</Frame>
```

원 이미지는 코드에서 만들 수도 있습니다.

```csharp
Frame circleImageFrame = new Frame
{
    Margin = 10,
    BorderColor = Color.Black,
    CornerRadius = 50,
    HeightRequest = 60,
    WidthRequest = 60,
    IsClippedToBounds = true,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center,
    Content = new Image
    {
        Source = ImageSource.FromFile("outdoors.jpg"),
        Aspect = Aspect.AspectFill,
        Margin = -20,
        HeightRequest = 100,
        WidthRequest = 100
    }
};
```

각 플랫폼 프로젝트에는 **실외 .jpg** 이미지가 추가 되어야 하며,이는 플랫폼에 따라 달라 집니다. 자세한 내용은 [의 Xamarin.Forms 이미지 ](~/xamarin-forms/user-interface/images.md)를 참조 하세요.

> [!NOTE]
> 모퉁이가 둥근 모퉁이는 플랫폼에서 약간 다르게 동작 합니다. `Image`개체의 너비는 `Margin` 이미지 너비와 부모 프레임 너비의 1/2 사이 여야 하며 개체 내에서 이미지를 균등 하 게 맞추려면 음수 여야 합니다 `Frame` . 그러나 요청 된 너비와 높이는 보장 되지 않으므로 `Margin` `HeightRequest` 사용자의 `WidthRequest` 이미지 크기와 기타 레이아웃 선택 항목에 따라 및 속성을 변경 해야 할 수 있습니다.

## <a name="related-links"></a>관련 링크

* [프레임 데모](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-frame/)
* [이미지Xamarin.Forms](~/xamarin-forms/user-interface/images.md)
