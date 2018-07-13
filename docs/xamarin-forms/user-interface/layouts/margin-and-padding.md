---
title: 여백 및 안쪽 여백
description: 여백 및 안쪽 여백 속성 사용자 인터페이스에서 요소를 렌더링할 때 레이아웃 동작을 제어 합니다. 이 문서에서는 두 속성을 설정 하는 방법 간의 차이점을 보여 줍니다.
ms.prod: xamarin
ms.assetid: BEB096BB-51DF-410F-B0F1-D235287B0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 595e673c59d23a45cbaf923a0d58faff2000c296
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996618"
---
# <a name="margin-and-padding"></a>여백 및 안쪽 여백

_여백 및 안쪽 여백 속성 사용자 인터페이스에서 요소를 렌더링할 때 레이아웃 동작을 제어 합니다. 이 문서에서는 두 속성을 설정 하는 방법 간의 차이점을 보여 줍니다._

## <a name="overview"></a>개요

여백 및 안쪽 여백은 레이아웃 관련된 개념:

- 합니다 [ `Margin` ](xref:Xamarin.Forms.View.Margin) 요소와 해당 인접 한 요소 간의 거리를 나타내는 속성과 요소의 렌더링 위치 및 인접 요소의 렌더링 위치를 제어 하는 데 사용 됩니다. `Margin` 에 값을 지정할 수 있습니다 [레이아웃](~/xamarin-forms/user-interface/controls/layouts.md) 하 고 [보기](~/xamarin-forms/user-interface/controls/views.md) 클래스입니다.
- 합니다 [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) 요소 및 해당 자식 요소 사이의 거리를 나타내는 속성과 컨트롤 자체 콘텐츠 구분 하기 위해 사용 됩니다. `Padding` 에 값을 지정할 수 있습니다 [레이아웃](~/xamarin-forms/user-interface/controls/layouts.md) 클래스입니다.

다음 다이어그램은 두 가지 개념을 보여 줍니다.

[![](margin-and-padding-images/margins-and-padding-sml.png "여백 및 안쪽 여백 개념")](margin-and-padding-images/margins-and-padding.png#lightbox "여백 및 안쪽 여백 개념")

사실은 [ `Margin` ](xref:Xamarin.Forms.View.Margin) 값은 가산적입니다. 따라서 20 픽셀의 여백을 지정 하는 인접 요소 두 개, 요소 간 거리 40 픽셀 됩니다. 여백 및 안쪽 여백도 모두 적용 되 면 여백 및 안쪽 여백 요소와 콘텐츠 사이의 간격 수에 합산 합니다.

## <a name="specifying-a-thickness"></a>두께 지정합니다.

[ `Margin` ](xref:Xamarin.Forms.View.Margin) 하 고 [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) 속성을 사용 하면 형식의 둘 [ `Thickness` ](xref:Xamarin.Forms.Thickness)합니다. 세 가지 가능성이 만들면를 `Thickness` 구조:

- 만들기는 [ `Thickness` ](xref:Xamarin.Forms.Thickness) 균일 한 단일 값으로 정의 된 구조체입니다. 단일 값을 왼쪽, 위쪽, 오른쪽 및 아래쪽 면 요소에 적용 됩니다.
- 만들기는 [ `Thickness` ](xref:Xamarin.Forms.Thickness) 가로 및 세로 값으로 정의 된 구조체입니다. 가로 값 요소의 위쪽과 아래쪽에 대칭적으로 적용 되 고 세로 값을 사용 하 여 요소의 왼쪽과 오른쪽에 대칭적으로 적용 됩니다.
- 만들기는 [ `Thickness` ](xref:Xamarin.Forms.Thickness) 왼쪽, 위쪽, 오른쪽 및 아래쪽 면 요소에 적용 되는 4 개의 고유 값으로 정의 된 구조체입니다.

다음 XAML 코드 예제에서는 모든 세 가지 가능성을 보여 줍니다.

```xaml
<StackLayout Padding="0,20,0,0">
  <Label Text="Xamarin.Forms" Margin="20" />
  <Label Text="Xamarin.iOS" Margin="10, 15" />
  <Label Text="Xamarin.Android" Margin="0, 20, 15, 5" />
</StackLayout>
```

해당하는 C# 코드가 다음 코드 예제에 표시됩니다.

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
> `Thickness` 일반적으로 자르거나 콘텐츠 overdraws 값은 음수를 수 있습니다.

## <a name="summary"></a>요약

이 문서에서는 차이점을 설명 합니다 [ `Margin` ](xref:Xamarin.Forms.View.Margin) 및 [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) 속성 및 설정 하는 방법입니다. 속성은 사용자 인터페이스에서 요소를 렌더링할 때는 레이아웃 동작을 제어 합니다.


## <a name="related-links"></a>관련 링크

- [여백](xref:Xamarin.Forms.View.Margin)
- [안쪽 여백](xref:Xamarin.Forms.Layout.Padding)
- [두께](xref:Xamarin.Forms.Thickness)
