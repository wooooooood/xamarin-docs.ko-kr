---
title: Xamarin Forms CarouselView 상호 작용
description: CarouselView에 현재 표시 된 항목은 CurrentItem 및 Position 속성을 통해 액세스할 수 있습니다.
ms.prod: xamarin
ms.assetid: 854D97E5-D119-4BE2-AE7C-BD428792C992
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/11/2020
ms.openlocfilehash: 150c358346f90a513e1558dc847ad7eb6dd6e6e2
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635801"
---
# <a name="xamarinforms-carouselview-interaction"></a>Xamarin Forms CarouselView 상호 작용

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView) 은 사용자 상호 작용을 제어 하는 다음 속성을 정의 합니다.

- 현재 표시 되는 항목 `object`형식의 `CurrentItem`입니다. 이 속성은 `TwoWay`의 기본 바인딩 모드 이며 표시할 데이터가 없는 경우 `null` 값을 갖습니다.
- 현재 항목이 변경 될 때 실행 되는 `ICommand`형식의 `CurrentItemChangedCommand`입니다.
- `CurrentItemChangedCommandParameter` 형식의 `object` - `CurrentItemChangedCommand`에 전달되는 매개 변수입니다.
- `IsBounceEnabled``CarouselView` 콘텐츠 경계에서 바운스 하는지 여부를 지정 하는 `bool`형식입니다. 기본값은 `true`입니다.
- `IsSwipeEnabled`는 살짝 밀기 제스처가 표시 된 항목을 변경 하는지 여부를 결정 하는 `bool`형식입니다. 기본값은 `true`입니다.
- `int`형식의 `Position`기본 컬렉션에 있는 현재 항목의 인덱스입니다. 이 속성은 `TwoWay`의 기본 바인딩 모드를 가지 며 표시할 데이터가 없는 경우 0 값을 갖습니다.
- `PositionChangedCommand`는 위치가 변경 될 때 실행 되는 `ICommand`형식입니다.
- `PositionChangedCommandParameter` 형식의 `object` - `PositionChangedCommand`에 전달되는 매개 변수입니다.
- 현재 표시 되는 항목에 대 한 개체를 포함 하는 읽기 전용 속성인 `ObservableCollection<View>`형식의 `VisibleViews`입니다.

이 모든 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원되며, 이는 속성이 데이터 바인딩의 대상이 될 수 있음을 의미합니다.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) 은 사용자 스크롤으로 인해 또는 응용 프로그램에서 속성을 설정 하는 경우 `CurrentItem` 속성이 변경 될 때 발생 하는 `CurrentItemChanged` 이벤트를 정의 합니다. `CurrentItemChanged` 이벤트와 함께 제공 되는 `CurrentItemChangedEventArgs` 개체에는 두 가지 속성인 `object`형식이 있습니다.

- `PreviousItem` – 속성이 변경 된 후의 이전 항목입니다.
- `CurrentItem` – 속성 변경 후 현재 항목입니다.

또한 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 는 사용자 스크롤으로 인해 또는 응용 프로그램에서 속성을 설정 하는 경우 `Position` 속성이 변경 될 때 발생 하는 `PositionChanged` 이벤트를 정의 합니다. `PositionChanged` 이벤트와 함께 제공 되는 `PositionChangedEventArgs` 개체에는 두 가지 속성인 `int`형식이 있습니다.

- `PreviousPosition` – 속성 변경 후의 이전 위치입니다.
- `CurrentPosition` – 속성 변경 후의 현재 위치입니다.

## <a name="respond-to-the-current-item-changing"></a>현재 항목 변경에 응답

현재 표시 된 항목이 변경 되 면 `CurrentItem` 속성이 항목의 값으로 설정 됩니다. 이 속성이 변경 되 면 `ICommand`전달 되는 `CurrentItemChangedCommandParameter` 값을 사용 하 여 `CurrentItemChangedCommand` 실행 됩니다. 그런 다음 `Position` 속성이 업데이트 되 고 `CurrentItemChanged` 이벤트가 발생 합니다.

> [!IMPORTANT]
> `Position` 속성은 `CurrentItem` 속성이 변경 될 때 변경 됩니다. 그러면 `PositionChangedCommand` 실행 되 고 `PositionChanged` 이벤트가 발생 합니다.

### <a name="event"></a>이벤트

다음 XAML 예제에서는 이벤트 처리기를 사용 하 여 현재 항목의 변경 내용에 응답 하는 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 를 보여 줍니다.

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              CurrentItemChanged="OnCurrentItemChanged">
    ...
</CarouselView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.CurrentItemChanged += OnCurrentItemChanged;
```

이 예제에서는 `CurrentItemChanged` 이벤트가 발생 하면 `OnCurrentItemChanged` 이벤트 처리기가 실행 됩니다.

```csharp
void OnCurrentItemChanged(object sender, CurrentItemChangedEventArgs e)
{
    Monkey previousItem = e.PreviousItem as Monkey;
    Monkey currentItem = e.CurrentItem as Monkey;
}
```

이 예제에서 `OnCurrentItemChanged` 이벤트 처리기는 이전 및 현재 항목을 노출 합니다.

[![IOS 및 Android의 이전 및 현재 항목을 포함 하는 CarouselView의 스크린샷](interaction-images/current-item-events.png "현재 및 이전 항목으로 CarouselView")](interaction-images/current-item-events-large.png#lightbox "현재 및 이전 항목으로 CarouselView")

### <a name="command"></a>명령

다음 XAML 예제에서는 명령을 사용 하 여 현재 항목의 변경 내용에 응답 하는 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 를 보여 줍니다.

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              CurrentItemChangedCommand="{Binding ItemChangedCommand}"
              CurrentItemChangedCommandParameter="{Binding Source={RelativeSource Self}, Path=CurrentItem}">
    ...
</CarouselView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.SetBinding(CarouselView.CurrentItemChangedCommandProperty, "ItemChangedCommand");
carouselView.SetBinding(CarouselView.CurrentItemChangedCommandParameterProperty, new Binding("CurrentItem", source: RelativeBindingSource.Self));
```

이 예제에서 `CurrentItemChangedCommand` 속성은 `ItemChangedCommand` 속성에 바인딩되고 `CurrentItem` 속성 값을 인수로 전달 합니다. 그런 다음 필요에 따라 현재 항목을 변경 하는 `ItemChangedCommand`에 응답할 수 있습니다.

```csharp
public ICommand ItemChangedCommand => new Command<Monkey>((item) =>
{
    PreviousMonkey = CurrentMonkey;
    CurrentMonkey = item;
});
```

이 예제에서 `ItemChangedCommand`은 이전 및 현재 항목을 저장 하는 개체를 업데이트 합니다.

## <a name="respond-to-the-position-changing"></a>변경 된 위치에 응답

현재 표시 된 항목이 변경 되 면 `Position` 속성이 기본 컬렉션의 현재 항목 인덱스에 설정 됩니다. 이 속성이 변경 되 면 `ICommand`전달 되는 `PositionChangedCommandParameter` 값을 사용 하 여 `PositionChangedCommand` 실행 됩니다. 그러면 `PositionChanged` 이벤트가 발생 합니다. `Position` 속성이 프로그래밍 방식으로 변경 되 면 [`CarouselView`](xref:Xamarin.Forms.CarouselView) `Position` 값에 해당 하는 항목으로 스크롤됩니다.

> [!NOTE]
> `Position` 속성을 0으로 설정 하면 기본 컬렉션의 첫 번째 항목이 표시 됩니다.

### <a name="event"></a>이벤트

다음 XAML 예제에서는 이벤트 처리기를 사용 하 여 `Position` 속성의 변경에 응답 하는 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 을 보여 줍니다.

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"              
              PositionChanged="OnPositionChanged">
    ...
</CarouselView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.PositionChanged += OnPositionChanged;
```

이 예제에서는 `PositionChanged` 이벤트가 발생 하면 `OnPositionChanged` 이벤트 처리기가 실행 됩니다.

```csharp
void OnPositionChanged(object sender, PositionChangedEventArgs e)
{
    int previousItemPosition = e.PreviousPosition;
    int currentItemPosition = e.CurrentPosition;
}
```

이 예제에서 `OnCurrentItemChanged` 이벤트 처리기는 이전 및 현재 위치를 노출 합니다.

[![IOS 및 Android의 이전 및 현재 위치를 사용 하는 CarouselView의 스크린샷](interaction-images/current-position-events.png "현재 위치와 이전 위치를 사용 하는 CarouselView")](interaction-images/current-position-events-large.png#lightbox "현재 위치와 이전 위치를 사용 하는 CarouselView")

### <a name="command"></a>명령

다음 XAML 예제에서는 명령을 사용 하 여 `Position` 속성의 변경에 응답 하는 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 보여 줍니다.

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PositionChangedCommand="{Binding PositionChangedCommand}"
              PositionChangedCommandParameter="{Binding Source={RelativeSource Self}, Path=Position}">
    ...
</CarouselView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.SetBinding(CarouselView.PositionChangedCommandProperty, "PositionChangedCommand");
carouselView.SetBinding(CarouselView.PositionChangedCommandParameterProperty, new Binding("Position", source: RelativeBindingSource.Self));
```

이 예제에서 `PositionChangedCommand` 속성은 `PositionChangedCommand` 속성에 바인딩되고 `Position` 속성 값을 인수로 전달 합니다. 그런 다음 필요에 따라 변경 된 위치에 응답할 수 `PositionChangedCommand`.

```csharp
public ICommand PositionChangedCommand => new Command<int>((position) =>
{
    PreviousPosition = CurrentPosition;
    CurrentPosition = position;
});
```

이 예제에서 `PositionChangedCommand`은 이전 및 현재 위치를 저장 하는 개체를 업데이트 합니다.

## <a name="preset-the-current-item"></a>현재 항목 미리 설정

[`CarouselView`](xref:Xamarin.Forms.CarouselView) 의 현재 항목은 `CurrentItem` 속성을 항목으로 설정 하 여 프로그래밍 방식으로 설정할 수 있습니다. 다음 XAML 예제에서는 현재 항목을 미리 선택 하는 `CarouselView` 보여 줍니다.

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              CurrentItem="{Binding CurrentItem}">
    ...
</CarouselView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.SetBinding(CarouselView.CurrentItemProperty, "CurrentItem");
```

> [!NOTE]
> `CurrentItem` 속성은 `TwoWay`의 기본 바인딩 모드를 갖습니다.

`CarouselView.CurrentItem` 속성 데이터는 `Monkey`유형인 연결 된 뷰 모델의 `CurrentItem` 속성에 바인딩됩니다. 기본적으로 사용자가 현재 항목을 변경 하는 경우 `CurrentItem` 속성의 값이 현재 `Monkey` 개체로 설정 되도록 `TwoWay` 바인딩이 사용 됩니다. `CurrentItem` 속성은 `MonkeysViewModel` 클래스에서 정의 됩니다.

```csharp
public class MonkeysViewModel : INotifyPropertyChanged
{
    // ...
    public ObservableCollection<Monkey> Monkeys { get; private set; }

    public Monkey CurrentItem { get; set; }

    public MonkeysViewModel()
    {
        // ...
        CurrentItem = Monkeys.Skip(3).FirstOrDefault();
        OnPropertyChanged("CurrentItem");
    }
}
```

이 예제에서 `CurrentItem` 속성은 `Monkeys` 컬렉션의 네 번째 항목으로 설정 됩니다.

[![IOS 및 Android에서 사전 설정 항목을 포함 하는 CarouselView의 스크린샷](interaction-images/preset-item.png "사전 설정 항목을 포함 하는 CarouselView")](interaction-images/preset-item-large.png#lightbox "사전 설정 항목을 포함 하는 CarouselView")

## <a name="preset-the-position"></a>위치를 미리 설정 합니다.

표시 된 항목 [`CarouselView`](xref:Xamarin.Forms.CarouselView) `Position` 속성을 기본 컬렉션의 항목 인덱스로 설정 하 여 프로그래밍 방식으로 설정할 수 있습니다. 다음 XAML 예제에서는 표시 된 항목을 설정 하는 `CarouselView` 보여 줍니다.

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              Position="{Binding Position}">
    ...
</CarouselView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.SetBinding(CarouselView.PositionProperty, "Position");
```

> [!NOTE]
> `Position` 속성은 `TwoWay`의 기본 바인딩 모드를 갖습니다.

`CarouselView.Position` 속성 데이터는 `int`유형인 연결 된 뷰 모델의 `Position` 속성에 바인딩됩니다. 기본적으로 사용자가 [`CarouselView`](xref:Xamarin.Forms.CarouselView)을 스크롤하면 `Position` 속성의 값이 표시 된 항목의 인덱스로 설정 되도록 `TwoWay` 바인딩이 사용 됩니다. `Position` 속성은 `MonkeysViewModel` 클래스에서 정의 됩니다.

```csharp
public class MonkeysViewModel : INotifyPropertyChanged
{
    // ...
    public int Position { get; set; }

    public MonkeysViewModel()
    {
        // ...
        Position = 3;
        OnPropertyChanged("Position");
    }
}
```

이 예제에서 `Position` 속성은 `Monkeys` 컬렉션의 네 번째 항목으로 설정 됩니다.

[![IOS 및 Android에서 사전 설정 된 위치가 있는 CarouselView의 스크린샷](interaction-images/preset-position.png "미리 설정 된 위치가 있는 CarouselView")](interaction-images/preset-position-large.png#lightbox "미리 설정 된 위치가 있는 CarouselView")

## <a name="define-visual-states"></a>시각적 상태 정의

[`CarouselView`](xref:Xamarin.Forms.CarouselView) 는 네 가지 시각적 상태를 정의 합니다.

- `CurrentItem` 현재 표시 된 항목의 시각적 상태를 나타냅니다.
- `PreviousItem`은 이전에 표시 된 항목의 시각적 상태를 나타냅니다.
- `NextItem`는 다음 항목에 대 한 시각적 상태를 나타냅니다.
- 항목의 나머지 부분에 대 한 시각적 상태를 나타내는 `DefaultItem`입니다.

이러한 시각적 상태를 사용 하 여 [`CarouselView`](xref:Xamarin.Forms.CarouselView)표시 되는 항목에 대 한 시각적 변경을 시작할 수 있습니다.

다음 XAML 예제에서는 `CurrentItem`, `PreviousItem`, `NextItem`및 `DefaultItem` 시각적 상태를 정의 하는 방법을 보여 줍니다.

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <VisualStateManager.VisualStateGroups>
                    <VisualStateGroup x:Name="CommonStates">
                        <VisualState x:Name="CurrentItem">
                            <VisualState.Setters>
                                <Setter Property="Scale"
                                        Value="1.1" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="PreviousItem">
                            <VisualState.Setters>
                                <Setter Property="Opacity"
                                        Value="0.5" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="NextItem">
                            <VisualState.Setters>
                                <Setter Property="Opacity"
                                        Value="0.5" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="DefaultItem">
                            <VisualState.Setters>
                                <Setter Property="Opacity"
                                        Value="0.25" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateManager.VisualStateGroups>

                <!-- Item template content -->
                <Frame HasShadow="true">
                    ...
                </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

이 예제에서 `CurrentItem` 시각적 상태는 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 표시 되는 현재 항목이 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성이 기본값 1에서 1.1로 변경 되도록 지정 합니다. `PreviousItem` 및 `NextItem` 시각적 상태는 현재 항목을 둘러싼 항목이 [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) 값 0.5와 함께 표시 되도록 지정 합니다. `DefaultItem` 시각적 상태는 `CarouselView` 표시 되는 항목의 나머지 부분이 `Opacity` 값 0.25와 함께 표시 되도록 지정 합니다.

> [!NOTE]
> 또는 `ItemTemplate` 속성 값으로 설정 된 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)의 루트 요소 형식인 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) 속성 값이 있는 [`Style`](xref:Xamarin.Forms.Style) 에서 시각적 상태를 정의할 수 있습니다.

다음 스크린샷은 `CurrentItem`, `PreviousItem`및 `NextItem` 시각적 상태를 보여 줍니다.

[![IOS 및 Android의 시각적 상태를 사용 하는 CarouselView의 스크린샷](interaction-images/visual-states.png "CarouselView 시각적 상태")](interaction-images/visual-states-large.png#lightbox "CarouselView 시각적 상태")

시각적 상태에 대 한 자세한 내용은 [Xamarin. Forms 시각적 상태 관리자](~/xamarin-forms/user-interface/visual-state-manager.md)를 참조 하세요.

## <a name="clear-the-current-item"></a>현재 항목 지우기

`CurrentItem` 속성은 해당 속성을 설정 하거나 `null`에 바인딩하는 개체를 설정 하 여 지울 수 있습니다.

## <a name="disable-bounce"></a>바운스 사용 안 함

기본적으로 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 콘텐츠 경계에서 항목을 바운스 합니다. `IsBounceEnabled` 속성을 `false`로 설정 하 여이를 비활성화할 수 있습니다.

## <a name="disable-swipe-interaction"></a>살짝 밀기 상호 작용 사용 안 함

기본적으로 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 는 사용자가 살짝 밀기 제스처를 사용 하 여 항목 간을 이동할 수 있도록 허용 합니다. `IsSwipeEnabled` 속성을 `false`로 설정 하면이 살짝의 상호 작용을 비활성화할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [CarouselView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [Xamarin.ios 시각적 상태 관리자](~/xamarin-forms/user-interface/visual-state-manager.md)
