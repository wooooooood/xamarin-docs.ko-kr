---
title: Xamarin.Forms에 레이아웃 옵션
description: 모든 Xamarin.Forms 보기 LayoutOptions 형식의 HorizontalOptions 및 VerticalOptions 속성에 있습니다. 이 문서에서는 각 LayoutOptions 값 맞춤 및 보기의 확장에 대 한 효과 설명 합니다.
ms.prod: xamarin
ms.assetid: 7CAB5631-5153-4DEF-8AD7-C6011CE44307
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2017
ms.openlocfilehash: dc15c05bf3633ef2ae5f71754290a7bd768dc836
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245692"
---
# <a name="layout-options-in-xamarinforms"></a>Xamarin.Forms에 레이아웃 옵션

_모든 Xamarin.Forms 보기 LayoutOptions 형식의 HorizontalOptions 및 VerticalOptions 속성에 있습니다. 이 문서에서는 각 LayoutOptions 값 맞춤 및 보기의 확장에 대 한 효과 설명 합니다._

## <a name="overview"></a>개요

[ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) 구조 두 레이아웃 기본 설정을 캡슐화 합니다.

- **맞춤** – 보기의 맞춤의 위치와 부모 레이아웃 내에서 크기를 결정 하는 기본 설정으로 표시 됩니다.
- **확장** 만 사용 되는 –는 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), 사용 가능한 경우 보기 추가 공간을 사용 해야 하는 경우를 나타냅니다.

이러한 레이아웃 기본 설정을 적용할 수 있습니다는 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)을 기준으로 설정 하 여 해당 부모는 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) 또는 [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) 의 속성은 `View` 에서 public 필드 중 하나에 [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) 구조입니다. Public 필드는 다음과 같습니다.

- [`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

`Start`, `Center`, `End`, 및 `Fill` 필드를 사용 하 여 부모 레이아웃 내에서 보기의 맞춤을 정의할 수 있습니다.

- 가로 맞춤에 대 한 [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/) 위치는 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) 배치 부모 레이아웃 및 세로 맞춤에 대 한 왼쪽 면에는 `View` 위쪽에는 부모 레이아웃입니다.
- 가로 및 세로 맞춤에 대 한 [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/) 가로 또는 세로로 가운데에 맞춥니다는 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)합니다.
- 가로 맞춤에 대 한 [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/) 위치는 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) 부모 레이아웃 및 세로 맞춤에 대 한 오른쪽 면에 배치 된 `View` 맨 아래에 부모 레이아웃입니다.
- 가로 맞춤에 대 한 [ `Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/) 되도록는 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) 너비를 채웁니다 부모 레이아웃의 시키고 세로 맞춤에 있는 되도록는 `View` 채웁니다는 부모 레이아웃의 높이입니다.

`StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, 및 `FillAndExpand` 값을 사용 하 여 정렬 된 기본 설정을 정의할 수 여부 뷰 공간을 차지 합니다 더 많은 부모 내에서 사용 가능한 경우 및 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)합니다.

> [!NOTE]
> 보기의 기본값인 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) 및 [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) 속성은 [ `LayoutOptions.Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)합니다.

<a name="alignment" />

## <a name="alignment"></a>맞춤

맞춤 부모 레이아웃에 사용 하지 않은 공간 부모 레이아웃 내에서 보기 배치 되는 방법을 제어 (즉, 부모 레이아웃은 모든 자식의 결합 된 크기 보다 큰).

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 만 존중는 `Start`, `Center`, `End`, 및 `Fill` [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) 반대 방향에 있는 자식 보기에는 필드 에 `StackLayout` 방향입니다. 세로 방향의 내에서 자식 뷰 따라서 `StackLayout` 설정할 수 자신의 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) 속성 중 하나에 `Start`, `Center`, `End`, 또는 `Fill` 필드입니다. 가로 방향의 내에서 자식 뷰 마찬가지로, `StackLayout` 설정할 수 자신의 [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) 속성 중 하나에 `Start`, `Center`, `End`, 또는 `Fill` 필드입니다.

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 지키지 않습니다는 `Start`, `Center`, `End`, 및 `Fill` [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) 와 동일한 방향으로에 있는 자식 보기에는 필드 `StackLayout` 방향입니다. 따라서 세로 방향 `StackLayout` 무시는 `Start`, `Center`, `End`, 또는 `Fill` 에 설정 된 경우 필드는 [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) 자식 뷰의 속성입니다. 마찬가지로, 한 가로 방향의 `StackLayout` 무시는 `Start`, `Center`, `End`, 또는 `Fill` 에 설정 된 경우 필드는 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) 자식 뷰의 속성입니다.

> [!NOTE]
> [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/) 일반적으로 사용 하 여 지정 된 요청 크기 조정 재정의 [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) 및 [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) 속성입니다.

다음 XAML 코드 예제에서는 세로 방향 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 여기서 각 자식 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 설정 해당 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) 속성 4 개의 맞춤 필드 중 하나에 [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) 구조:

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

다음 스크린샷에 표시 된 레이아웃은 코드 발생 합니다.

[![](layout-options-images/alignment.png "맞춤 레이아웃 옵션")](layout-options-images/alignment-large.png#lightbox "맞춤 레이아웃 옵션")

<a name="expansion" />

## <a name="expansion"></a>확장

확장 보기 내에서 사용 가능한 경우 더 많은 공간을 차지 합니다 있는지 여부를 제어는 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)합니다. 경우는 `StackLayout` 사용 되지 않는 공간이 (즉,는 `StackLayout` 해당 자식을 모두의 결합 된 크기 보다 크면)를 사용 하지 않은 공간을 설정 하 여 확장을 요청 하는 모든 자식 보기에서 동일 하 게 공유 자신의 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)또는 [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) 속성을 한 [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) 사용 하는 필드는 `AndExpand` 접미사입니다. 경우의 공간을 모두는 `StackLayout` 는 사용 하는 확장 옵션은 영향을 주지 않습니다.

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 해당 방향에 자식 뷰를 확장할 수만 있습니다. 따라서 세로 방향 `StackLayout` 설정 하는 자식 뷰를 확장할 수 자신의 [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) 속성 중 하나에 `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, 또는 `FillAndExpand` 하는 경우이 필드는 `StackLayout` 사용 하지 않는 공간을 포함 합니다. 마찬가지로, 한 가로 방향의 `StackLayout` 설정 하는 자식 뷰를 확장할 수 자신의 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) 속성 중 하나에 `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, 또는 `FillAndExpand` 하는 경우이 필드는 `StackLayout` 사용 하지 않는 공간을 포함 합니다.

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 방향이 반대 방향으로 자식 뷰를 확장할 수 없습니다. 세로 방향에 따라서 `StackLayout`설정는 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) 속성에 자식 뷰에 [ `StartAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/) 에 속성을 설정 하는 것과 같습니다 [ `Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/).

> [!NOTE]
> 확장을 사용 하도록 설정 하지 않는 변경 뷰의 크기를 사용 하지 않는 한 [ `LayoutOptions.FillAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)합니다.

다음 XAML 코드 예제에서는 세로 방향 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 여기서 각 자식 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 설정 해당 [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) 속성 네 개의 확장 필드 중 하나에 [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) 구조:

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

다음 스크린샷에 표시 된 레이아웃은 코드 발생 합니다.

[![](layout-options-images/expansion.png "확장 레이아웃 옵션")](layout-options-images/expansion-large.png#lightbox "확장 레이아웃 옵션")

각 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 동일한 양의 내에서 공간을 차지 하는 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)합니다. 그러나 최종만 `Label`, 입력 집합의 [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) 속성을 [ `FillAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/) 크기가 다른 합니다. 또한 각 `Label` 작은 빨간색으로 구분 됩니다 [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/),이 통해 공간은 `Label` 차지를 쉽게 볼 수 있습니다.

## <a name="summary"></a>요약

이 문서에서는 효과 각 [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) 구조 값 맞춤 및 부모에 상대적인 보기의 확장에 대 한 합니다. `Start`, `Center`, `End`, 및 `Fill` 필드를 사용 하 여 부모 레이아웃 내에서 보기의 맞춤을 정의 하 고 `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, 및 `FillAndExpand` 필드를 사용 하 여 정의할 수 맞춤 기본 설정 및 보기 내에서 사용 가능한 경우 더 많은 공간을 차지 합니다 있는지 여부를 결정 하는 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)합니다.



## <a name="related-links"></a>관련 링크

- [LayoutOptions (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutoptions/)
- [LayoutOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/)
