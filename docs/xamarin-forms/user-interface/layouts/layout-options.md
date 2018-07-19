---
title: Xamarin.forms에서 레이아웃 옵션
description: 모든 Xamarin.Forms 보기 LayoutOptions 형식의 HorizontalOptions 및 VerticalOptions 속성에 있습니다. 이 문서에서는 각 LayoutOptions 값 맞춤 및 보기의 확장에는 영향을 설명 합니다.
ms.prod: xamarin
ms.assetid: 7CAB5631-5153-4DEF-8AD7-C6011CE44307
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2017
ms.openlocfilehash: 1ede5f75925a3dafa93062d147fa349ff91f07d2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995312"
---
# <a name="layout-options-in-xamarinforms"></a>Xamarin.forms에서 레이아웃 옵션

_모든 Xamarin.Forms 보기 LayoutOptions 형식의 HorizontalOptions 및 VerticalOptions 속성에 있습니다. 이 문서에서는 각 LayoutOptions 값 맞춤 및 보기의 확장에는 영향을 설명 합니다._

## <a name="overview"></a>개요

합니다 [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) 구조 두 레이아웃 기본 설정을 캡슐화 합니다.

- **맞춤** – 보기의 해당 위치와 해당 부모 레이아웃 내에서 크기를 결정 하는 맞춤, 기본 설정으로 표시 됩니다.
- **확장** 만 사용 되는 –를 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), 가능 하다 면 뷰 공간을 사용 해야 하는 경우를 나타냅니다.

이러한 레이아웃 기본 설정을 적용할 수 있습니다를 [ `View` ](xref:Xamarin.Forms.View)설정 하 여 부모에 상대적 합니다 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) 또는 [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) 의 속성을 `View` 에서 public 필드 중 하나에 [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) 구조입니다. Public 필드는 아래와 같습니다.

- [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

합니다 `Start`, `Center`를 `End`, 및 `Fill` 필드를 사용 하 여 부모 레이아웃 내에서 보기의 맞춤을 정의할 수:

- 가로 맞춤에 대 한 [ `Start` ](xref:Xamarin.Forms.LayoutOptions.Start) 위치를 [ `View` ](xref:Xamarin.Forms.View) 는 왼쪽 세로 맞춤 및 부모 레이아웃에 배치 된 `View` 맨 위에 있는 부모 레이아웃입니다.
- 가로 및 세로 맞춤에 대 한 [ `Center` ](xref:Xamarin.Forms.LayoutOptions.Center) 가로나 세로 방향으로 가운데 맞춤 합니다 [ `View` ](xref:Xamarin.Forms.View)합니다.
- 가로 맞춤에 대 한 [ `End` ](xref:Xamarin.Forms.LayoutOptions.End) 위치를 [ `View` ](xref:Xamarin.Forms.View) 부모 레이아웃 및 세로 맞춤에 대 한 오른쪽 측에서 배치를 `View` 맨 아래에 부모 레이아웃입니다.
- 가로 맞춤에 대 한 [ `Fill` ](xref:Xamarin.Forms.LayoutOptions.Fill) 되도록 합니다 [ `View` ](xref:Xamarin.Forms.View) 너비를 채웁니다 부모 레이아웃 및 세로 맞춤을 보장 합니다 `View` 채웁니다는 부모 레이아웃의 높이입니다.

합니다 `StartAndExpand`, `CenterAndExpand`를 `EndAndExpand`, 및 `FillAndExpand` 값 된 맞춤 기본 설정을 정의 하는 데 사용 됩니다 여부 뷰 공간을 차지 합니다 자세한 부모 내에서 사용 가능한 경우 및 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)합니다.

> [!NOTE]
> 보기의 기본값인 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) 하 고 [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) 속성은 [ `LayoutOptions.Fill` ](xref:Xamarin.Forms.LayoutOptions.Fill)합니다.

<a name="alignment" />

## <a name="alignment"></a>맞춤

맞춤 부모 레이아웃 공백이 사용 되지 않는 경우 보기를 부모 레이아웃 내에서 배치 하는 방식을 제어 (즉, 부모 레이아웃은 모든 해당 하위 항목의 조합된 된 크기 보다 큰).

[ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 만 고려 합니다 `Start`, `Center`를 `End`, 및 `Fill` [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) 반대 방향에 있는 자식 보기의 필드 에 `StackLayout` 방향입니다. 세로 방향의 내의 자식 뷰 따라서 `StackLayout` 설정할 수 있습니다 해당 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) 속성 중 하나에 `Start`를 `Center`를 `End`, 또는 `Fill` 필드입니다. 가로 방향의 내의 자식 뷰 마찬가지로 `StackLayout` 설정할 수 있습니다 해당 [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) 속성 중 하나에 `Start`를 `Center`를 `End`, 또는 `Fill` 필드입니다.

[ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 영향을 미치지 않습니다 합니다 `Start`, `Center`를 `End`, 및 `Fill` [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) 같은 방향으로에 있는 자식 보기의 필드 `StackLayout` 방향입니다. 따라서 세로 방향의 `StackLayout` 무시 합니다 `Start`, `Center`, `End`, 또는 `Fill` 필드에 설정 된 경우를 [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) 자식 뷰의 속성입니다. 마찬가지로, 가로 방향의 `StackLayout` 무시 합니다 `Start`, `Center`, `End`, 또는 `Fill` 필드에 설정 된 경우를 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) 자식 뷰의 속성.

> [!NOTE]
> [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) 일반적으로 사용 하 여 지정 된 요청 크기를 재정의 합니다 [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) 하 고 [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) 속성입니다.

다음 XAML 코드 예제에서는 세로 방향의 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 여기서 각 자식 [ `Label` ](xref:Xamarin.Forms.Label) 설정 하는 해당 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) 속성 4 개의 맞춤 필드 중 하나에 [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) 구조:

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <Label Text="Start" BackgroundColor="Gray" HorizontalOptions="Start" />
  <Label Text="Center" BackgroundColor="Gray" HorizontalOptions="Center" />
  <Label Text="End" BackgroundColor="Gray" HorizontalOptions="End" />
  <Label Text="Fill" BackgroundColor="Gray" HorizontalOptions="Fill" />
</StackLayout>
```

해당 하는 C# 코드는 다음과 같습니다.

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

코드를 다음 스크린샷에서 표시 된 레이아웃에 발생 합니다.

[![](layout-options-images/alignment.png "맞춤 레이아웃 옵션")](layout-options-images/alignment-large.png#lightbox "맞춤 레이아웃 옵션")

<a name="expansion" />

## <a name="expansion"></a>확장

확장 내에서 사용 가능한 경우 뷰를 더 많은 공간을 차지 합니다 있는지 여부를 제어를 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)합니다. 경우는 `StackLayout` 공백이 사용 되지 않는 (즉, 합니다 `StackLayout` 해당 자식을 모두의 결합 된 크기 보다 큽니다.), 하지 않은 공간을 설정 하 여 확장을 요청 하는 모든 자식 보기에서 동일 하 게 공유 됩니다 해당 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions)또는 [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) 속성을 [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) 사용 하는 필드는 `AndExpand` 접미사. 경우의 공간을 모두는 `StackLayout` 는 사용, 확장 옵션은 영향을 주지 않습니다.

A [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 자식 뷰 방향 해당 방향으로 확장할 수 있습니다. 따라서 세로 방향의 `StackLayout` 설정 하는 자식 뷰를 확장할 수 있습니다 해당 [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) 속성 중 하나에 `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, 또는 `FillAndExpand` 경우 필드를 `StackLayout` 사용 하지 않은 공간을 포함 합니다. 마찬가지로, 가로 방향의 `StackLayout` 설정 하는 자식 뷰를 확장할 수 해당 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) 속성 중 하나에 `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, 또는 `FillAndExpand` 필드, 합니다 `StackLayout` 사용 하지 않은 공간을 포함 합니다.

A [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 방향이 반대 방향으로 자식 뷰를 확장할 수 없습니다. 세로 방향의에 따라서 `StackLayout`으로 설정 합니다 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) 자식 뷰에 속성 [ `StartAndExpand` ](xref:Xamarin.Forms.LayoutOptions.StartAndExpand) 속성 설정 하는 것과 동일한 효과가 [ `Start`](xref:Xamarin.Forms.LayoutOptions.Start).

> [!NOTE]
> 확장을 사용 하도록 설정 하면 변경 되지 않는지 뷰의 크기를 사용 하지 않는 이상 참고 [ `LayoutOptions.FillAndExpand` ](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)합니다.

다음 XAML 코드 예제에서는 세로 방향의 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 여기서 각 자식 [ `Label` ](xref:Xamarin.Forms.Label) 설정 하는 해당 [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) 속성 네 개의 확장 필드 중 하나에 [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) 구조:

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

해당 하는 C# 코드는 다음과 같습니다.

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

코드를 다음 스크린샷에서 표시 된 레이아웃에 발생 합니다.

[![](layout-options-images/expansion.png "확장 레이아웃 옵션")](layout-options-images/expansion-large.png#lightbox "확장 레이아웃 옵션")

각 [ `Label` ](xref:Xamarin.Forms.Label) 동일한 양의 내의 공간을 차지 합니다 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)합니다. 그러나 최종만 `Label`, 집합 해당 [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) 속성을 [ `FillAndExpand` ](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) 크기가 다른 합니다. 또한 각 `Label` 작은 빨간색으로 구분 됩니다 [ `BoxView` ](xref:Xamarin.Forms.BoxView)에 공간을 사용 하도록 설정 하는 `Label` 쉽게 볼 수를 차지 합니다.

## <a name="summary"></a>요약

이 문서에서는 설명 효과 각 [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) 구조체 값 맞춤 및 부모를 기준으로 보기의 확장에 있습니다. `Start`, `Center`, `End`, 및 `Fill` 필드를 사용 하 여를 부모 레이아웃 내에서 보기의 맞춤을 정의 및 `StartAndExpand`를 `CenterAndExpand`를 `EndAndExpand`, 및 `FillAndExpand` 필드를 사용 하 여 정의할 수 맞춤 우선, 및 보기 내에서 사용 가능한 경우 더 많은 공간을 차지 합니다 있는지 여부를 결정 하는 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)합니다.



## <a name="related-links"></a>관련 링크

- [LayoutOptions (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutoptions/)
- [LayoutOptions](xref:Xamarin.Forms.LayoutOptions)
