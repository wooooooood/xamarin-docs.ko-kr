---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6ae4116be99f076a7afd5ed9c2823bc12f445e18
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137993"
---
# <a name="margin-and-padding"></a>여백 및 안쪽 여백

_여백 및 안쪽 여백 속성은 요소가 사용자 인터페이스에서 렌더링 될 때 레이아웃 동작을 제어 합니다. 이 문서에서는 두 속성의 차이점 및 설정 하는 방법에 대해 설명 합니다._

## <a name="overview"></a>개요

여백과 안쪽 여백은 관련 된 레이아웃 개념입니다.

- [`Margin`](xref:Xamarin.Forms.View.Margin)속성은 요소와 인접 요소 사이의 거리를 나타내며 요소의 렌더링 위치와 해당 인접 요소의 렌더링 위치를 제어 하는 데 사용 됩니다. `Margin`값은 [레이아웃](~/xamarin-forms/user-interface/controls/layouts.md) 및 [뷰](~/xamarin-forms/user-interface/controls/views.md) 클래스에 지정할 수 있습니다.
- [`Padding`](xref:Xamarin.Forms.Layout.Padding)속성은 요소와 자식 요소 사이의 거리를 나타내며 컨트롤을 자체 콘텐츠에서 분리 하는 데 사용 됩니다. `Padding`값은 [레이아웃](~/xamarin-forms/user-interface/controls/layouts.md) 클래스에 지정할 수 있습니다.

다음 다이어그램에서는 두 가지 개념을 보여 줍니다.

[![](margin-and-padding-images/margins-and-padding-sml.png "Margins and Padding Concepts")](margin-and-padding-images/margins-and-padding.png#lightbox "Margins and Padding Concepts")

[`Margin`](xref:Xamarin.Forms.View.Margin)값은 덧셈입니다. 따라서 인접 한 두 개의 요소가 20 픽셀의 여백을 지정 하는 경우 요소 간의 거리가 40 픽셀입니다. 또한 여백과 안쪽 여백이 모두 적용 되는 경우에는 요소와 모든 내용 간의 거리가 여백 뒤에 추가 됩니다.

## <a name="specifying-a-thickness"></a>두께 지정

[`Margin`](xref:Xamarin.Forms.View.Margin)및 [`Padding`](xref:Xamarin.Forms.Layout.Padding) 속성은 모두 형식입니다 [`Thickness`](xref:Xamarin.Forms.Thickness) . 구조를 만들 때 세 가지 가능성이 있습니다 `Thickness` .

- [`Thickness`](xref:Xamarin.Forms.Thickness)단일 값으로 정의 된 구조체를 만듭니다. 단일 값은 요소의 왼쪽, 위쪽, 오른쪽 및 아래쪽에 적용 됩니다.
- [`Thickness`](xref:Xamarin.Forms.Thickness)가로 및 세로 값으로 정의 된 구조체를 만듭니다. 가로 값은 요소의 왼쪽과 오른쪽에 대칭으로 적용 되며 세로 값은 요소의 위쪽 및 아래쪽에 대칭으로 적용 됩니다.
- [`Thickness`](xref:Xamarin.Forms.Thickness)요소의 왼쪽, 위쪽, 오른쪽 및 아래쪽에 적용 되는 네 개의 고유 값으로 정의 된 구조체를 만듭니다.

다음 XAML 코드 예제에서는 세 가지 가능성을 모두 보여 줍니다.

```xaml
<StackLayout Padding="0,20,0,0">
  <Label Text="Xamarin.Forms" Margin="20" />
  <Label Text="Xamarin.iOS" Margin="10, 15" />
  <Label Text="Xamarin.Android" Margin="0, 20, 15, 5" />
</StackLayout>
```

동등한 C# 코드는 다음 코드 예제와 같습니다.

```csharp
var stackLayout = new StackLayout {
  Padding = new Thickness(0,20,0,0),
  Children = {
    new Label { Text = "Xamarin.Forms", Margin = new Thickness (20) },
    new Label { Text = "Xamarin.iOS", Margin = new Thickness (10, 25) },
    new Label { Text = "Xamarin.Android", Margin = new Thickness (0, 20, 15, 5) }
  }
};
```

> [!NOTE]
> `Thickness`값은 음수일 수 있으며, 일반적으로 콘텐츠를 클리핑 또는 과도 하 게 그립니다.

## <a name="summary"></a>요약

이 문서에서는와 속성의 차이점 [`Margin`](xref:Xamarin.Forms.View.Margin) [`Padding`](xref:Xamarin.Forms.Layout.Padding) 및 속성을 설정 하는 방법을 보여 줍니다. 속성은 요소가 사용자 인터페이스에서 렌더링 될 때 레이아웃 동작을 제어 합니다.

## <a name="related-links"></a>관련 링크

- [Margin](xref:Xamarin.Forms.View.Margin)
- [여백](xref:Xamarin.Forms.Layout.Padding)
- [Thickness](xref:Xamarin.Forms.Thickness)
