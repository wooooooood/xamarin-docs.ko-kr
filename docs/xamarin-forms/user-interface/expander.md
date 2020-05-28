---
title: Xamarin.Forms기호
description: Xamarin.Forms확장기 컨트롤은 모든 콘텐츠를 호스트 하는 확장 가능한 컨테이너를 제공 합니다. 확장 헤더를 누르면 콘텐츠가 표시 되거나 숨겨집니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5e9afa0f6d27003891963af5715d5721e3129306
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84129573"
---
# <a name="xamarinforms-expander"></a>Xamarin.Forms기호

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-expanderdemos/)

Xamarin.Forms `Expander` 컨트롤은 모든 콘텐츠를 호스트 하는 확장 가능한 컨테이너를 제공 합니다. 컨트롤에는 헤더와 콘텐츠가 있으며 헤더를 눌러 콘텐츠가 표시 되거나 숨겨집니다 `Expander` . 머리글만 표시 되는 경우 `Expander` `Expander` 이 *축소*됩니다. `Expander`콘텐츠가 표시 되 면 `Expander` 이 *확장*됩니다.

다음 스크린샷은 `Expander` 축소 및 확장 된 상태의을 보여 주고 헤더와 콘텐츠를 나타내는 빨간색 상자를 표시 합니다.

![IOS 및 Android에서 축소 및 확장 된 상태의 확장기 스크린샷](expander-images/expander.png "IOS 및 Android의 확장기")

> [!IMPORTANT]
> `Expander`는 현재 실험적 이며 플래그를 설정 하는 방법 으로만 사용할 수 있습니다 `Expander_Experimental` . 자세한 내용은 [실험적 플래그](~/xamarin-forms/internals/experimental-flags.md)를 참조 하세요.
>
> 또한 `Expander` 컨트롤은 네임 스페이스에서 완전히 구현 됩니다 `Xamarin.Forms` . 따라서에서 지 원하는 모든 플랫폼에서 사용할 수 있습니다 Xamarin.Forms .

`Expander`컨트롤은 다음 속성을 정의 합니다.

- `CollapseAnimationEasing`는 [`Easing`](xref:Xamarin.Forms.Easing) `Expander` 축소 될 때 콘텐츠에 적용 되는 감속/가속 함수를 나타내는 형식의입니다.
- `CollapseAnimationLength``uint`는를 축소할 때 애니메이션의 기간을 정의 하는 형식의입니다 `Expander` . 이 속성의 기본값은 250ms입니다.
- `Command`는 헤더를 `ICommand` 누를 때 실행 되는 형식의입니다 `Expander` .
- `object` 형식의 `CommandParameter` - `Command`에 전달되는 매개 변수입니다.
- `Content`[`View`](xref:Xamarin.Forms.View)는가 확장 될 때 표시 되는 내용을 정의 하는 형식의입니다 `Expander` .
- `ContentTemplate`는의 콘텐츠를 동적으로 확장 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 하는 데 사용 되는 템플릿인 형식의입니다 `Expander` .
- `ExpandAnimationEasing`는 [`Easing`](xref:Xamarin.Forms.Easing) 확장 중에 콘텐츠에 적용 되는 감속/가속 함수를 나타내는 형식의입니다 `Expander` .
- `ExpandAnimationLength``uint`는가 확장 될 때 애니메이션의 기간을 정의 하는 형식의입니다 `Expander` . 이 속성의 기본값은 250ms입니다.
- `ForceUpdateSizeCommand``ICommand`는의 크기를 강제로 업데이트할 때 실행 되는 명령을 정의 하는 형식의입니다 `Expander` . 이 속성은 `OneWayToSource` 바인딩 모드를 사용 합니다.
- `Header`[`View`](xref:Xamarin.Forms.View)헤더 콘텐츠를 정의 하는 형식의입니다.
- `IsExpanded`가 `bool` 확장 되었는지 여부를 결정 하는 형식의입니다 `Expander` . 이 속성은 `TwoWay` 바인딩 모드를 사용 하며 기본값은 `false` 입니다.
- `Spacing``double`-헤더와 해당 내용 사이의 공백을 나타내는 형식의 이 속성의 기본값은 0입니다.
- `State`의 상태를 `ExpanderState` 나타내는 형식의입니다 `Expander` . 이 속성은 `OneWayToSource` 바인딩 모드를 사용 합니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

> [!NOTE]
> `Content`속성은 클래스의 콘텐츠 속성 이므로 `Expander` XAML에서 명시적으로 설정할 필요가 없습니다.

`ExpanderState` 열거형은 다음 멤버를 정의합니다.

- `Expanding`가 확장 중임을 나타냅니다 `Expander` .
- `Expanded`가 확장 됨을 나타냅니다 `Expander` .
- `Collapsing`가 축소 됨을 나타냅니다 `Expander` .
- `Collapsed`이 축소 됨을 나타냅니다 `Expander` .

`Expander`또한이 컨트롤은 `Tapped` 헤더를 누를 때 발생 하는 이벤트를 정의 합니다 `Expander` . 또한에 `Expander` `ForceUpdateSize` 는 런타임에 프로그래밍 방식으로 크기를 조정 하기 위해 호출할 수 있는 메서드가 포함 되어 있습니다 `Expander` .

## <a name="create-an-expander"></a>확장기 만들기

다음 예제에서는 XAML로를 인스턴스화하는 방법을 보여 줍니다 `Expander` .

```xaml
<Expander>
    <Expander.Header>
        <Label Text="Baboon"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Grid Padding="10">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>
        <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
               Aspect="AspectFill"
               HeightRequest="120"
               WidthRequest="120" />
        <Label Grid.Column="1"
               Text="Baboons are African and Arabian Old World monkeys belonging to the genus Papio, part of the subfamily Cercopithecinae."
               FontAttributes="Italic" />
    </Grid>
</Expander>
```

이 예제에서는 `Expander` 가 기본적으로 축소 되 고를 헤더로 표시 합니다 [`Label`](xref:Xamarin.Forms.Label) . 헤더를 누르면 확장 중에 `Expander` 자식 컨트롤이 포함 된 콘텐츠가 표시 됩니다 [`Grid`](xref:Xamarin.Forms.Grid) . `Expander`가 확장 될 때 해당 헤더를 누르면이 축소 됩니다 `Expander` .

> [!IMPORTANT]
> `Expander.Content`속성을 암시적 또는 명시적으로 설정 하는 경우 `Expander` 가 축소 된 경우에도이를 포함 하는 페이지를 탐색 하면 콘텐츠가 만들어집니다 `Expander` . 그러나이 `Expander.ContentTemplate` 처음으로 확장 될 때만 팽창 가져오는 콘텐츠로 속성을 설정할 수 있습니다 `Expander` . 자세한 내용은 [주문형 확장기 콘텐츠 만들기](#create-expander-content-on-demand)를 참조 하세요.

또는 `Expander` 코드에서를 만들 수 있습니다.

```csharp
Expander expander = new Expander
{
    Header = new Label
    {
        Text = "Baboon",
        FontAttributes = FontAttributes.Bold,
        FontSize = Device.GetNamedSize(NamedSize.Medium, typeof(Label))
    }
};

Grid grid = new Grid
{
    Padding = new Thickness(10),
    ColumnDefinitions =
    {
        new ColumnDefinition { Width = GridLength.Auto },
        new ColumnDefinition { Width = GridLength.Auto }
    }
};

grid.Children.Add(new Image
{
    Source = "http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg",
    Aspect = Aspect.AspectFill,
    HeightRequest = 120,
    WidthRequest = 120
});

grid.Children.Add(new Label
{
    Text = "Baboons are African and Arabian Old World monkeys belonging to the genus Papio, part of the subfamily Cercopithecinae.",
    FontAttributes = FontAttributes.Italic
}, 1, 0);

expander.Content = grid;
```

## <a name="create-expander-content-on-demand"></a>주문형 확장기 콘텐츠 만들기

`Expander`확장에 대 한 응답으로 요청 시 콘텐츠를 만들 수 있습니다 `Expander` . 이 `Expander.ContentTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 작업은 콘텐츠를 포함 하는로 속성을 설정 하 여 수행할 수 있습니다.

```xaml
<Expander>
    <Expander.Header>
        <Label Text="{Binding Name}"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Expander.ContentTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="120"
                       WidthRequest="120" />
                <Label Grid.Column="1"
                       Text="{Binding Details}"
                       FontAttributes="Italic" />
            </Grid>
        </DataTemplate>
    </Expander.ContentTemplate>
</Expander>
```

이 예제에서는가 `Expander` 처음으로 확장 될 때만 콘텐츠가 팽창 됩니다 `Expander` .

이 방법의 장점은 페이지에 여러 개체가 포함 되어 있는 경우 `Expander` 사용자가 처음으로 확장 하는 경우에만 해당 콘텐츠를 생성 한다는 것입니다 `Expander` .

## <a name="add-an-expansion-indicator"></a>확장 표시기 추가

를 [`Image`](xref:Xamarin.Forms.Image) 헤더에 추가 하 여 `Expander` 확장 상태를 시각적으로 나타낼 수 있습니다. 는 [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) `Image` `Source` 속성의 값에 따라 속성을 변경 하는에 연결할 수 있습니다 `Expander.IsExpanded` .

```xaml
<Expander>
    <Expander.Header>
        <Grid>
            <Label Text="{Binding Name}"
                   FontAttributes="Bold"
                   FontSize="Medium" />
            <Image Source="expand.png"
                   HorizontalOptions="End"
                   VerticalOptions="Start">
                <Image.Triggers>
                    <DataTrigger TargetType="Image"
                                 Binding="{Binding Source={RelativeSource AncestorType={x:Type Expander}}, Path=IsExpanded}"
                                 Value="True">
                        <Setter Property="Source"
                                Value="collapse.png" />
                    </DataTrigger>
                </Image.Triggers>
            </Image>
        </Grid>
    </Expander.Header>
    <Expander.ContentTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="120"
                       WidthRequest="120" />
                <Label Grid.Column="1"
                       Text="{Binding Details}"
                       FontAttributes="Italic" />
            </Grid>
        </DataTemplate>
    </Expander.ContentTemplate>
</Expander>
```

이 예제에서은 [`Image`](xref:Xamarin.Forms.Image) `expand` 기본적으로 아이콘을 표시 합니다.

![IOS 및 Android의 축소 된 상태에 있는 확장기 아이콘의 스크린샷](expander-images/icon-expand.png "IOS 및 Android의 Expandd 아이콘")

`IsExpanded`이 속성은 `true` 헤더를 `Expander` 탭 하면 `collapse` 아이콘이 표시 됩니다.

![IOS 및 Android에서 확장 상태의 확장 아이콘 스크린샷](expander-images/icon-collapse.png "IOS 및 Android의 Expandd 아이콘")

트리거에 대 한 자세한 내용은 [ Xamarin.Forms 트리거](~/xamarin-forms/app-fundamentals/triggers.md)를 참조 하세요.

## <a name="define-the-space-between-header-and-content"></a>헤더와 내용 사이의 간격을 정의 합니다.

기본적으로의 콘텐츠는 `Expander` 머리글 바로 아래에 나타납니다. 그러나 `Spacing` 속성을 `double` 콘텐츠와 헤더 사이의 빈 공간을 나타내는 값으로 설정 하 여이 동작을 변경할 수 있습니다.

```xaml
<Expander Spacing="50"
          IsExpanded="true">
    <Expander.Header>
        <Label Text="Baboon"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Grid Padding="10">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>
        <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
               Aspect="AspectFill"
               HeightRequest="120"
               WidthRequest="120" />
        <Label Grid.Column="1"
               Text="Baboons are African and Arabian Old World monkeys belonging to the genus Papio, part of the subfamily Cercopithecinae."
               FontAttributes="Italic" />
    </Grid>
</Expander>
```

이 예제에서 콘텐츠는 `Expander` 헤더 아래에 50 장치 독립적 단위로 표시 됩니다.

![IOS 및 Android에서 간격이 설정 된 확장기의 스크린샷](expander-images/expander-spacing.png "IOS 및 Android에서 간격이 설정 된 확장기")

## <a name="embed-an-expander-in-an-expander"></a>확장기에 확장 포함

의 콘텐츠를 `Expander` 다른 컨트롤로 설정 하 여 `Expander` 여러 수준의 확장을 사용할 수 있습니다. 다음 XAML은 `Expander` 다른 개체인 콘텐츠가 있는를 보여 줍니다 `Expander` .

```xaml
<Expander Spacing="10">
    <Expander.Header>
        <Label Text="{Binding Name}"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Expander Padding="10"
              Spacing="10">
        <Expander.Header>
            <Label Text="{Binding Location}"
                   FontSize="Medium" />
        </Expander.Header>
            <Expander.ContentTemplate>
                <DataTemplate>
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto" />
                            <ColumnDefinition Width="Auto" />
                        </Grid.ColumnDefinitions>
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="120"
                               WidthRequest="120" />
                        <Label Grid.Column="1"
                               Text="{Binding Details}"
                               FontAttributes="Italic" />
                    </Grid>
                </DataTemplate>
            </Expander.ContentTemplate>
    </Expander>
</Expander>
```

이 예제에서 루트 헤더를 누르면 `Expander` 자식에 대 한 헤더가 표시 됩니다 `Expander` .

![IOS 및 Android에서 포함 된 확장기의 스크린샷](expander-images/embedded-expander1.png "IOS 및 Android에 포함 된 확장기")

자식 헤더를 누르면 `Expander` 콘텐츠가 팽창 표시 되 고 표시 됩니다.

![IOS 및 Android에서 포함 된 확장기의 스크린샷](expander-images/embedded-expander2.png "IOS 및 Android에 포함 된 확장기")

## <a name="define-the-expand-and-collapse-animation"></a>확장 및 축소 애니메이션 정의

확장 또는 축소를 사용할 때 발생 하는 애니메이션은 `Expander` `ExpandAnimationEasing` 및 `CollapseAnimationEasing` 속성을에 포함 된 감속/가속 함수 Xamarin.Forms 또는 사용자 지정 감속/가속 함수 중 하나로 설정 하 여 정의할 수 있습니다. 기본적으로 확장 및 축소 애니메이션은 250ms에 대해 발생 합니다. 그러나 이러한 기간은 `ExpandAnimationLength` 및 `CollapseAnimationLength` 속성을 값으로 설정 하 여 변경할 수 있습니다 `uint` .

다음 XAML에서는 사용자가를 확장 하거나 축소할 때 발생 하는 애니메이션을 정의 하는 예제를 보여 줍니다 `Expander` .

```xaml
<Expander ExpandAnimationEasing="{x:Static Easing.CubicIn}"
          ExpandAnimationLength="500"
          CollapseAnimationEasing="{x:Static Easing.CubicOut}"
          CollapseAnimationLength="500">
    <Expander.Header>
        <Label Text="{Binding Name}"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Expander.ContentTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="120"
                       WidthRequest="120" />
                <Label Grid.Column="1"
                       Text="{Binding Details}"
                       FontAttributes="Italic" />
            </Grid>
        </DataTemplate>
    </Expander.ContentTemplate>
</Expander>
```

이 예에서 `CubicIn` 감속/가속 함수는 500ms에 대 한 확장 애니메이션을 천천히 가속화 하 고, `CubicOut` 감속/가속 함수는 500ms를 통해 축소 애니메이션을 신속 하 게 감속 합니다.

감속/가속 함수에 대 한 자세한 내용은 [ Xamarin.Forms 감속/가속 함수](~/xamarin-forms/user-interface/animation/easing.md)를 참조 하세요.

## <a name="resize-an-expander-at-runtime"></a>런타임에 확장기 크기 조정

는 `Expander` 런타임에 메서드를 사용 하 여 프로그래밍 방식으로 크기를 조정할 수 있습니다 `ForceUpdateSize` .

에 연결 된가 있는가 포함 된 이라는가 지정 된 경우 `Expander` `expander` [`Label`](xref:Xamarin.Forms.Label) `TapGestureRecognizer` 다음 코드 예제에서는 메서드를 호출 하는 방법을 보여 줍니다 `ForceUpdateSize` .

```csharp
void OnLabelTapped(object sender, EventArgs e)
{
    Label label = sender as Label;
    Expander expander = label.Parent.Parent.Parent as Expander;

    if (label.FontSize == Device.GetNamedSize(NamedSize.Default, label))
    {
        label.FontSize = Device.GetNamedSize(NamedSize.Large, label);
    }
    else
    {
        label.FontSize = Device.GetNamedSize(NamedSize.Default, label);
    }
    expander.ForceUpdateSize();
}
```

이 예제에서의는를 `FontSize` [`Label`](xref:Xamarin.Forms.Label) 누를 때 변경 됩니다 `Label` . 글꼴의 크기 때문에의 메서드를 호출 하 여의 크기를 업데이트 해야 합니다 `Expander` `ForceUpdateSize` .

## <a name="disable-an-expander"></a>확장기 사용 안 함

응용 프로그램은를 확장 하는 `Expander` 작업이 유효한 작업이 아닌 상태를 입력할 수 있습니다. 이러한 경우에는 `Expander` 속성을 false로 설정 하 여를 비활성화할 수 있습니다 `IsEnabled` . 이렇게 하면 사용자가를 확장 하거나 축소할 수 `Expander` 없습니다.

## <a name="related-links"></a>관련 링크

- [확장기 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-expanderdemos/)
- [Xamarin.Forms감속/가속 함수](~/xamarin-forms/user-interface/animation/easing.md)
- [Xamarin.FormsInstead](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms바인딩 가능한 레이아웃](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
