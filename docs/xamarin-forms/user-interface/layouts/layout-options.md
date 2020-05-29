---
title: 의 레이아웃 옵션Xamarin.Forms
description: 모든 Xamarin.Forms 보기에는 LayoutOptions 형식의 HorizontalOptions 및 VerticalOptions 속성이 있습니다. 이 문서에서는 각 LayoutOptions 값이 뷰의 맞춤 및 확장에 미치는 영향을 설명 합니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 17f4e76f9bef71352cabddfba9397e95bcdd24d3
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138036"
---
# <a name="layout-options-in-xamarinforms"></a>의 레이아웃 옵션Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutoptions)

_모든 Xamarin.Forms 보기에는 LayoutOptions 형식의 HorizontalOptions 및 VerticalOptions 속성이 있습니다. 이 문서에서는 각 LayoutOptions 값이 뷰의 맞춤 및 확장에 미치는 영향을 설명 합니다._

## <a name="overview"></a>개요

[`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions)구조는 다음과 같은 두 가지 레이아웃 기본 설정을 캡슐화 합니다.

- **Alignment** – 부모 레이아웃 내에서 위치와 크기를 결정 하는 뷰의 기본 설정 맞춤입니다.
- **확장** – 에서만 사용 되며, [`StackLayout`](xref:Xamarin.Forms.StackLayout) 사용할 수 있는 경우 뷰에서 추가 공간을 사용 해야 하는지 여부를 나타냅니다.

이러한 레이아웃 기본 설정은 [`View`](xref:Xamarin.Forms.View) [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 의 또는 속성을 구조체의 `View` 공용 필드 중 하나로 설정 하 여 부모에 상대적인에 적용할 수 있습니다 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) . 공용 필드는 다음과 같습니다.

- [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

`Start`,, `Center` `End` 및 필드는 `Fill` 부모 레이아웃 내에서 뷰의 맞춤을 정의 하는 데 사용 됩니다.

- 가로 맞춤의 경우 [`Start`](xref:Xamarin.Forms.LayoutOptions.Start) [`View`](xref:Xamarin.Forms.View) 부모 레이아웃의 왼쪽에를 배치 하 고 세로 맞춤의 경우를 `View` 부모 레이아웃의 위쪽에 배치 합니다.
- 가로 및 세로 맞춤의 경우를 [`Center`](xref:Xamarin.Forms.LayoutOptions.Center) 가로 또는 세로로 가운데에 맞춥니다 [`View`](xref:Xamarin.Forms.View) .
- 가로 맞춤의 경우 [`End`](xref:Xamarin.Forms.LayoutOptions.End) [`View`](xref:Xamarin.Forms.View) 부모 레이아웃의 오른쪽에를 배치 하 고 세로 맞춤의 경우를 `View` 부모 레이아웃의 아래쪽에 배치 합니다.
- 가로 맞춤의 경우 [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) 는 [`View`](xref:Xamarin.Forms.View) 부모 레이아웃의 너비를 채우도록 하 고 세로 맞춤의 경우는 `View` 부모 레이아웃의 높이를 채우도록 합니다.

`StartAndExpand`,, `CenterAndExpand` `EndAndExpand` 및 값은 `FillAndExpand` 맞춤 기본 설정을 정의 하는 데 사용 되며, 부모 내에서 사용 가능한 경우 뷰가 더 많은 공간을 차지 하는지 여부를 정의 하는 데 사용 됩니다 [`StackLayout`](xref:Xamarin.Forms.StackLayout) .

> [!NOTE]
> 보기 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 및 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성의 기본값은 [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)입니다.

<a name="alignment" />

## <a name="alignment"></a>맞춤

맞춤은 부모 레이아웃에 사용 되지 않는 공간이 있는 경우 (즉, 부모 레이아웃이 모든 자식의 전체 크기 보다 큰 경우) 뷰가 부모 레이아웃 내에 배치 되는 방식을 제어 합니다.

는 방향에 [`StackLayout`](xref:Xamarin.Forms.StackLayout) `Start` 대 한 `Center` `End` `Fill` [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 반대 방향으로 자식 뷰의,, 및 필드 `StackLayout` 를 고려 합니다. 따라서 세로 방향으로 된 자식 뷰는 `StackLayout` [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 속성 `Start` 을,, `Center` `End` 또는 필드 중 하나로 설정할 수 있습니다 `Fill` . 마찬가지로, 가로 방향으로 된 자식 보기는 `StackLayout` [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성 `Start` 을,, `Center` `End` 또는 필드 중 하나로 설정할 수 있습니다 `Fill` .

는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) `Start` `Center` `End` `Fill` [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 방향과 같은 방향에 있는 자식 보기의,, 및 필드를 고려 하지 않습니다 `StackLayout` . 따라서 `StackLayout` `Start` `Center` `End` `Fill` 자식 뷰의 속성에 설정 된 경우 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 에는 세로 방향으로,, 또는 필드가 무시 됩니다. 마찬가지로,,, `StackLayout` `Start` `Center` `End` 또는 `Fill` 필드가 자식 뷰의 속성에 설정 된 경우 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 가로 방향으로 무시 됩니다.

> [!NOTE]
> [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)일반적으로는 및 속성을 사용 하 여 지정 된 크기 요청을 재정의 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 합니다.

다음 XAML 코드 예제에서는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 각 자식 [`Label`](xref:Xamarin.Forms.Label) [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 에서 해당 속성을 구조체의 네 가지 맞춤 필드 중 하나로 설정 하는 방향을 세로로 보여 줍니다 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) .

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <Label Text="Start" BackgroundColor="Gray" HorizontalOptions="Start" />
  <Label Text="Center" BackgroundColor="Gray" HorizontalOptions="Center" />
  <Label Text="End" BackgroundColor="Gray" HorizontalOptions="End" />
  <Label Text="Fill" BackgroundColor="Gray" HorizontalOptions="Fill" />
</StackLayout>
```

해당 c # 코드는 다음과 같습니다.

```csharp
Content = new StackLayout
{
  Margin = new Thickness(0, 20, 0, 0),
  Children = {
    ...
    new Label { Text = "Start", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Start },
    new Label { Text = "Center", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Center },
    new Label { Text = "End", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.End },
    new Label { Text = "Fill", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Fill }
  }
};
```

이 코드는 다음 스크린샷에 표시 된 레이아웃을 생성 합니다.

[![](layout-options-images/alignment.png "Alignment Layout Options")](layout-options-images/alignment-large.png#lightbox "Alignment Layout Options")

<a name="expansion" />

## <a name="expansion"></a>확장용

확장은 뷰가 사용 가능한 경우 내에서 더 많은 공간을 차지 하는지 여부를 제어 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 합니다. 에 `StackLayout` 사용 되지 않는 공간이 있는 경우 (즉, `StackLayout` 가 모든 자식의 전체 크기 보다 큰 경우) 해당 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 또는 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성을 접미사를 사용 하 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 는 필드로 설정 하 여 확장을 요청 하는 모든 자식 뷰에서 사용 하지 않은 공간이 동일 하 게 공유 됩니다 `AndExpand` . 의 모든 공간을 사용 하는 경우 `StackLayout` 확장 옵션은 적용 되지 않습니다.

[`StackLayout`](xref:Xamarin.Forms.StackLayout)은 해당 방향으로만 자식 뷰를 확장할 수 있습니다. 따라서가 `StackLayout` [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) `StartAndExpand` `CenterAndExpand` `EndAndExpand` `FillAndExpand` `StackLayout` 사용 하지 않는 공간을 포함 하는 경우에는 세로 방향으로,,, 또는 필드 중 하나로 속성을 설정 하는 자식 뷰를 확장할 수 있습니다. 마찬가지로,가 `StackLayout` [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) `StartAndExpand` `CenterAndExpand` `EndAndExpand` `FillAndExpand` `StackLayout` 사용 하지 않는 공간을 포함 하는 경우에는 가로 방향으로,,, 또는 필드 중 하나로 해당 속성을 설정 하는 자식 뷰를 확장할 수 있습니다.

는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 방향이 반대 방향으로 자식 보기를 확장할 수 없습니다. 따라서 세로 방향으로 `StackLayout` [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 자식 뷰의 속성을로 설정 하는 것은 속성을로 설정 하는 [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand) 것과 동일한 효과를 가집니다 [`Start`](xref:Xamarin.Forms.LayoutOptions.Start) .

> [!NOTE]
> 확장을 사용 하도록 설정 해도를 사용 하는 경우를 제외 하 고는 뷰 크기가 변경 되지 않습니다 [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) .

다음 XAML 코드 예제에서는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 각 자식 [`Label`](xref:Xamarin.Forms.Label) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 에서 해당 속성을 구조에서 4 개의 확장 필드 중 하나로 설정 하는 방향을 세로로 보여 줍니다 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) .

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Start" BackgroundColor="Gray" VerticalOptions="StartAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Center" BackgroundColor="Gray" VerticalOptions="CenterAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="End" BackgroundColor="Gray" VerticalOptions="EndAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Fill" BackgroundColor="Gray" VerticalOptions="FillAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
</StackLayout>
```

해당 c # 코드는 다음과 같습니다.

```csharp
Content = new StackLayout
{
  Margin = new Thickness(0, 20, 0, 0),
  Children = {
    ...
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "StartAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.StartAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "CenterAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.CenterAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "EndAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.EndAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "FillAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.FillAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 }
  }
};
```

이 코드는 다음 스크린샷에 표시 된 레이아웃을 생성 합니다.

[![](layout-options-images/expansion.png "Expansion Layout Options")](layout-options-images/expansion-large.png#lightbox "Expansion Layout Options")

각 [`Label`](xref:Xamarin.Forms.Label) 은 내에서 동일한 공간을 차지 합니다 [`StackLayout`](xref:Xamarin.Forms.StackLayout) . 그러나 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성을 [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)로 설정하는 최종 `Label`만 크기가 다릅니다. 또한 각는 `Label` 작은 빨강으로 구분 되며,이를 통해 [`BoxView`](xref:Xamarin.Forms.BoxView) 사용 되는 공간을 `Label` 쉽게 볼 수 있습니다.

## <a name="summary"></a>요약

이 문서에서는 각 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 구조 값이 부모에 상대적인 뷰의 맞춤 및 확장에 미치는 영향에 대해 설명 했습니다. ,, 및 `Start` `Center` 필드는 `End` `Fill` 부모 레이아웃 내에서 뷰의 맞춤을 정의 하는 데 사용 되 고,, `StartAndExpand` `CenterAndExpand` `EndAndExpand` 및 필드는 `FillAndExpand` 맞춤 기본 설정을 정의 하는 데 사용 되며,에서 사용할 수 있는 경우 뷰에서 더 많은 공간을 사용할 수 있는지 여부를 결정 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 하는 데 사용 됩니다.

## <a name="related-links"></a>관련 링크

- [LayoutOptions (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutoptions)
- [LayoutOptions](xref:Xamarin.Forms.LayoutOptions)
