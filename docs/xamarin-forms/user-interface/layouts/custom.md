---
title: 사용자 지정 레이아웃 만들기
description: 이 문서는 사용자 지정 레이아웃 클래스를 작성 하는 방법에 설명 하 고 페이지에 걸쳐 가로로 자식을 정렬 하 고 그런 다음 후속 자식이 추가 행을 표시 하는 방향에 민감한 WrapLayout 클래스를 보여 줍니다.
ms.prod: xamarin
ms.assetid: B0CFDB59-14E5-49E9-965A-3DCCEDAC2E31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/29/2017
ms.openlocfilehash: 0c16fd3930926a05ed7796391962d0fc8996dc96
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995377"
---
# <a name="creating-a-custom-layout"></a>사용자 지정 레이아웃 만들기

_Xamarin.Forms 4 레이아웃 클래스 – StackLayout, AbsoluteLayout, RelativeLayout, 및 그리드를 정의 하 고 자식을 다른 방식으로 정렬 하는 각 키를 누릅니다. 그러나 경우에 Xamarin.Forms에서 제공 하지 않는 레이아웃을 사용 하 여 페이지 콘텐츠를 구성 해야 합니다. 이 문서는 사용자 지정 레이아웃 클래스를 작성 하는 방법에 설명 하 고 페이지에 걸쳐 가로로 자식을 정렬 하 고 그런 다음 후속 자식이 추가 행을 표시 하는 방향에 민감한 WrapLayout 클래스를 보여 줍니다._

## <a name="overview"></a>개요

Xamarin.forms에 모든 레이아웃 클래스에서 파생 된 [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) 클래스 및 제네릭 형식 제약 [ `View` ](xref:Xamarin.Forms.View) 및 파생된 형식입니다. 차례로 합니다 `Layout<T>` 클래스에서 파생 되는 [ `Layout` ](xref:Xamarin.Forms.Layout) 요소 배치 및 크기 조정 자식 메커니즘을 제공 하는 클래스입니다.

모든 시각적 요소는 이라고 하는 자체 기본 크기를 결정 하는 일을 담당 합니다 *요청한* 크기. [`Page`](xref:Xamarin.Forms.Page)하십시오 [ `Layout` ](xref:Xamarin.Forms.Layout), 및 [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) 파생 형식은 위치와 해당 자식 또는 하위 항목 자체를 기준으로 크기를 결정 합니다. 레이아웃 위치 부모 결정 해야 자식의 크기를 부모-자식 관계를 포함 하는 따라서 하지만 자식 요청된 된 크기에 맞게 시도 됩니다.

Xamarin.Forms 레이아웃 및 무효화 순환의 철저 한 이해를 사용자 지정 레이아웃을 만들 필요 합니다. 이제 이러한 주기를 설명 합니다.

## <a name="layout"></a>레이아웃

레이아웃 페이지를 사용 하 여 시각적 트리의 맨 위에 있는 시작 및 페이지의 모든 시각적 요소를 포함 하도록 시각적 트리의 모든 분기를 진행 합니다. 다른 요소에는 부모 요소는 자체를 기준으로 자식을 배치 및 크기 조정 하는 일을 담당 합니다.

합니다 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) 클래스 정의 [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) 레이아웃 작업에 대 한 요소를 측정 하는 방법으로 [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) 지정 하는 방법 요소는 사각형 영역 내에서 렌더링 됩니다. 응용 프로그램이 시작 될 때 첫 번째 페이지가 표시 됩니다는 *레이아웃 주기가* 중 구성 된 첫 번째 `Measure` 호출 차례로 `Layout` 호출을 시작에는 [ `Page` ](xref:Xamarin.Forms.Page) 개체:

1. 레이아웃 주기 동안 모든 부모 요소는 호출을 담당 합니다 `Measure` 자식 요소에는 메서드.
1. 모든 부모 요소는 호출에 대 한 자식 항목을 측정 한 후의 `Layout` 자식 요소에는 메서드.

이 주기 페이지의 모든 시각적 요소에 대 한 호출 수신 되는지 확인 합니다 `Measure` 및 `Layout` 메서드. 프로세스는 다음 다이어그램에 표시 됩니다.

![](custom-images/layout-cycle.png "Xamarin.Forms 레이아웃 주기")

> [!NOTE]
> 레이아웃에 영향을 변경 하는 경우 레이아웃 주기 시각적 트리의 하위 집합에서 발생할 수 있습니다 note 합니다. 여기에 추가 되거나 에서처럼 이러한 컬렉션에서 제거할 항목을 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), 변경 합니다 [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible) 요소 또는 요소의 크기를 변경 하는 속성입니다.

에 있는 모든 Xamarin.Forms 클래스는 `Content` 또는 `Children` 속성에는 재정의 가능한 [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) 메서드. 파생 된 사용자 지정 레이아웃 클래스 [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) 이 메서드를 재정의 하 고 있는지 확인 해야 합니다 [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) 하 고 [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) 메서드는 원하는 사용자 지정 레이아웃을 제공 하려면 요소의 모든 자식에서 호출 됩니다.

또한 모든 클래스에서 파생 되는 [ `Layout` ](xref:Xamarin.Forms.Layout) 또는 [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) 재정의 해야 합니다 [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) 되는 경우 레이아웃 클래스는 메서드를 호출 하 여 하는 데 필요한 크기를 결정 합니다 [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) 자식의 메서드.

> [!NOTE]
> 요소에 따라 해당 크기를 결정 *제약 조건*, 공간 요소의 부모 내에서 요소 수를 나타내는입니다. 제약 조건에 전달 합니다 [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) 하 고 [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) 메서드 범위는 0에서 `Double.PositiveInfinity`. 요소가 *제한*, 또는 *완벽 하 게 제한*에 대 한 호출을 받으면, 해당 [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) 메서드 유한 인수-를 사용 하 여 요소가 제한 됩니다 특정 크기입니다. 요소가 *비제한*, 또는 *부분적으로 제한*에 대 한 호출을 받으면, 해당 `Measure` 같음 하나 이상의 인수를 사용 하 여 메서드 `Double.PositiveInfinity` – 무한 제약 조건 수 나타내는 크기 자동 조정으로 간주 합니다.

## <a name="invalidation"></a>무효화

무효화는 변경 페이지에서 요소에는 새 레이아웃 주기를 트리거하는 프로세스입니다. 요소는 올바른 크기 또는 위치를 더 이상 없을 때에 잘못 된 간주 됩니다. 예를 들어 경우는 [ `FontSize` ](xref:Xamarin.Forms.Button.FontSize) 의 속성을 [ `Button` ](xref:Xamarin.Forms.Button) 변경은 `Button` 이 더 이상 올바른 크기 때문에 유효 하지 않은 것 이라고 합니다. 크기 조정 된 `Button` 변경의 영향을 미치기 레이아웃 페이지의 나머지 부분에 있을 수 있습니다.

요소는 자체를 호출 하 여 무효화 합니다 [ `InvalidateMeasure` ](xref:Xamarin.Forms.VisualElement.InvalidateMeasure) 메서드를 일반적으로 변경 될 때 요소의 속성 있는 요소의 새 크기입니다. 이 메서드가 발생 합니다 [ `MeasureInvalidated` ](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) 새 레이아웃 주기를 트리거하는 요소의 부모를 처리 하는 이벤트입니다.

[ `Layout` ](xref:Xamarin.Forms.Layout) 클래스에 대 한 처리기를 설정 합니다.는 [ `MeasureInvalidated` ](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) 이벤트에 추가 하는 모든 자식에 해당 `Content` 속성 또는 `Children` 컬렉션 처리기를 분리 및 경우를 자식 제거 됩니다. 따라서 자식이 있는 시각적 트리의 모든 요소 크기 자식 중 하나가 변경 될 때마다 경고가 표시 됩니다. 다음 다이어그램에서는 시각적 트리에 있는 요소의 크기 변경 트리 ripple 변경 발생할 수 있습니다 하는 방법을 보여 줍니다.

![](custom-images/invalidation.png "시각적 트리에서 무효화")

그러나는 `Layout` 클래스는 페이지의 레이아웃에서 자식의 크기 변경의 영향을 제한 하려고 합니다. 레이아웃 크기 제한 된 경우 다음 자식 크기 변경 영향을 주지 않습니다 시각적 트리의 상위 레이아웃 보다 높습니다. 그러나 일반적으로 레이아웃의 크기 변경에 영향을 줍니다 레이아웃 자식을 정렬 하는 방법. 따라서 레이아웃의 크기 변경은 레이아웃에 대 한 레이아웃 주기를 시작 하 고 레이아웃에 대 한 호출을 받을 해당 [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) 하 고 [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) 메서드.

합니다 [ `Layout` ](xref:Xamarin.Forms.Layout) 클래스도 정의 [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) 와 유사한 있는 메서드를 [ `InvalidateMeasure` ](xref:Xamarin.Forms.VisualElement.InvalidateMeasure) 메서드. `InvalidateLayout` 레이아웃을 배치 하 고 자식의 크기를 조정 하는 방법에 영향을 주는 변경 내용이 때마다 메서드를 호출 해야 합니다. 예를 들어 합니다 `Layout` 클래스를 호출 하는 `InvalidateLayout` 자식을 레이아웃에서 제거 또는 추가할 때마다 메서드.

합니다 [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) 반복 해 서 호출을 최소화 하기 위해 캐시를 구현 하도록 재정의할 수 있습니다 합니다 [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) 레이아웃의 자식의 메서드. 재정의 `InvalidateLayout` 메서드 자식이 추가 되거나 레이아웃에서 제거 시기에 대 한 알림을 제공 합니다. 마찬가지로, 합니다 [ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) 메서드를 재정의 하 여 레이아웃의 자식 중 하나가 크기를 변경 하는 경우 알림을 제공할 수 있습니다. 두 메서드 재정의 대 한 사용자 지정 레이아웃 캐시를 지우면 응답 해야 합니다. 자세한 내용은 [계산 및 데이터 캐싱](#caching)합니다.

## <a name="creating-a-custom-layout"></a>사용자 지정 레이아웃 만들기

사용자 지정 레이아웃을 만드는 프로세스는 다음과 같습니다.

1. `Layout<View>` 클래스에서 파생되는 클래스를 만듭니다. 자세한 내용은 [는 WrapLayout 만들기](#creating)합니다.
1. [*선택적*] 레이아웃 클래스에 설정 해야 하는 모든 매개 변수에 대 한 바인딩 가능한 속성에서 지 원하는 속성을 추가 합니다. 자세한 내용은 [추가 바인딩 가능한 속성에서 지 원하는 속성](#adding_properties)합니다.
1. 재정의 [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) 메서드를 호출 하는 [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) 레이아웃에 대 한 메서드는 레이아웃의 모든 자식에 요청 된 크기를 반환 합니다. 자세한 내용은 [OnMeasure 메서드 재정의](#onmeasure)합니다.
1. 재정의 된 [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) 메서드를 호출 하는 [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) 레이아웃의 모든 자식에서 메서드. 호출에 실패 합니다 [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) 메서드는 레이아웃에 있는 각 자식에 올바른 크기 또는 위치를 수신 하지 자식 하면 이며 따라서 자식 페이지에 표시 있게 됩니다. 자세한 내용은 [LayoutChildren 메서드 재정의](#layoutchildren)합니다.

  > [!NOTE]
>  자식을 열거 하는 경우는 [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) 하 고 [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) 재정의 건너뛸 모든 자식입니다 [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible) 속성`false`. 이렇게 하면 사용자 지정 레이아웃 보이지 않는 자식에 대 한 공간을 유지 하지 않습니다는 있습니다.

1. [*선택적*] 재정의 [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) 메서드 자식이 추가 되거나 레이아웃에서 제거 하는 경우 알림을 받을 수 있습니다. 자세한 내용은 [InvalidateLayout 메서드 재정의](#invalidatelayout)합니다.
1. [*선택적*] 재정의 [ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) 메서드 레이아웃의 자식 중 하나가 크기를 변경 하는 경우 알림을 받을 수 있습니다. 자세한 내용은 [OnChildMeasureInvalidated 메서드 재정의](#onchildmeasureinvalidated)합니다.

> [!NOTE]
> 이때 합니다 [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) 레이아웃의 크기는 자식 보다는 해당 부모에 의해 제어 됩니다 하는 경우 재정의 호출할 수 없습니다. 레이아웃 클래스에 기본이 아닌 경우 또는 제약 조건 중 하나 이상이 한정 된 경우 재정의 호출할 수 있지만 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) 하거나 [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) 속성 값입니다. 이러한 이유로 합니다 [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) 재정의 하는 동안 얻은 자식 크기를 사용할 수 없게 합니다 [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) 메서드 호출 합니다. 대신 `LayoutChildren` 를 호출 해야 합니다는 [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) 메서드를 호출 하기 전에 레이아웃의 자식에는 [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) 메서드. 자식의 크기를 얻은 또는 합니다 `OnMeasure` 나중 방지 하려면 재정의 캐시할 수 있습니다 `Measure` 호출을 `LayoutChildren` 재정의 하지만 레이아웃 클래스 크기를 다시 얻을 수 해야 하는 시기를 알고 있어야 합니다. 자세한 내용은 [계산 및 레이아웃 데이터 캐싱](#caching)합니다.

레이아웃 클래스를 추가 하 여 사용할 수 있습니다는 [ `Page` ](xref:Xamarin.Forms.Page), 자식을 레이아웃에 추가 하 여 합니다. 자세한 내용은 [사용 하 여 WrapLayout](#consuming)합니다.

<a name="creating" />

### <a name="creating-a-wraplayout"></a>WrapLayout 만들기

샘플 응용 프로그램을 보여 줍니다는 방향에 민감한 `WrapLayout` 페이지에 걸쳐 가로로 자식을 정렬 하 고 그런 다음 후속 자식이 추가 행을 표시 하는 클래스입니다.

`WrapLayout` 클래스 라고 하는 각 자식에 대 한 같은 크기의 공간을 할당 합니다 *셀 크기*자식 컨트롤의 최대 크기에 따라 합니다. 자식 셀 크기를 셀 안에 배치 될 수 있습니다 보다 작은 기준으로 해당 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) 하 고 [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) 속성 값입니다.

`WrapLayout` 클래스 정의 다음 코드 예제에 표시 됩니다.

```csharp
public class WrapLayout : Layout<View>
{
  Dictionary<Size, LayoutData> layoutDataCache = new Dictionary<Size, LayoutData>();
  ...
}
```

<a name="caching" />

#### <a name="calculating-and-caching-layout-data"></a>계산 및 레이아웃 데이터 캐싱

`LayoutData` 구조는 다양 한 속성에서에서 자식 컬렉션에 대 한 데이터를 저장 합니다.

- `VisibleChildCount` – 레이아웃에 표시 되는 자식의 수입니다.
- `CellSize` – 모든 자식이 있는 레이아웃의 크기 조정의 최대 크기입니다.
- `Rows` – 행의 수입니다.
- `Columns` -열 수 있습니다.

합니다 `layoutDataCache` 필드는 여러 `LayoutData` 값입니다. 응용 프로그램이 시작 되 면 두 `LayoutData` 개체에 캐시 됩니다 합니다 `layoutDataCache` 사전에 대 한 제약 조건 인수에 대 한 현재 방향 –를 `OnMeasure` 재정의 및에 대 한를 `width` 및 `height` 인수 에 `LayoutChildren` 재정의 합니다. 장치 가로 방향으로 회전 하는 경우는 `OnMeasure` 재정의 하며 `LayoutChildren` 재정의 다시 호출할 다른 두 결과 `LayoutData` 사전에 캐시 되는 개체입니다. 그러나 세로 방향으로 장치를 반환할 때 없는 추가 계산 되므로 `layoutDataCache` 이미는 필수 데이터를 포함 합니다.

다음 코드 예제는 `GetLayoutData` 의 속성을 계산 하는 메서드는 `LayoutData` 구조화 된 특정 크기에 따라:

```csharp
LayoutData GetLayoutData(double width, double height)
{
  Size size = new Size(width, height);

  // Check if cached information is available.
  if (layoutDataCache.ContainsKey(size))
  {
    return layoutDataCache[size];
  }

  int visibleChildCount = 0;
  Size maxChildSize = new Size();
  int rows = 0;
  int columns = 0;
  LayoutData layoutData = new LayoutData();

  // Enumerate through all the children.
  foreach (View child in Children)
  {
    // Skip invisible children.
    if (!child.IsVisible)
      continue;

    // Count the visible children.
    visibleChildCount++;

    // Get the child's requested size.
    SizeRequest childSizeRequest = child.Measure(Double.PositiveInfinity, Double.PositiveInfinity);

    // Accumulate the maximum child size.
    maxChildSize.Width = Math.Max(maxChildSize.Width, childSizeRequest.Request.Width);
    maxChildSize.Height = Math.Max(maxChildSize.Height, childSizeRequest.Request.Height);
  }

  if (visibleChildCount != 0)
  {
    // Calculate the number of rows and columns.
    if (Double.IsPositiveInfinity(width))
    {
      columns = visibleChildCount;
      rows = 1;
    }
    else
    {
      columns = (int)((width + ColumnSpacing) / (maxChildSize.Width + ColumnSpacing));
      columns = Math.Max(1, columns);
      rows = (visibleChildCount + columns - 1) / columns;
    }

    // Now maximize the cell size based on the layout size.
    Size cellSize = new Size();

    if (Double.IsPositiveInfinity(width))
      cellSize.Width = maxChildSize.Width;
    else
      cellSize.Width = (width - ColumnSpacing * (columns - 1)) / columns;

    if (Double.IsPositiveInfinity(height))
      cellSize.Height = maxChildSize.Height;
    else
      cellSize.Height = (height - RowSpacing * (rows - 1)) / rows;

    layoutData = new LayoutData(visibleChildCount, cellSize, rows, columns);
  }

  layoutDataCache.Add(size, layoutData);
  return layoutData;
}
```

`GetLayoutData` 메서드는 다음 작업을 수행 합니다.

- 계산 여부를 결정 `LayoutData` 값 캐시에 이미 있고 사용 가능한 경우 사용자에 게 돌려보냅니다.
- 호출 모든 자식을 열거이 고, 그렇지는 `Measure` 무한대의 너비 및 높이 사용 하 여 각 자식 메서드 고 자식 최대 크기를 결정 합니다.
- 표시 되는 하나 이상의 자식이 제공한 행과 열 필요한 개수를 계산 하 고 다음의 크기에 따라 자식에 대 한 셀 크기를 계산 합니다 `WrapLayout`합니다. 일반적으로 셀 크기는 최대 자식 크기 보다 약간 더 있지만 작은 일 수도 있습니다 경우는 `WrapLayout` 넓게 가장 넓은 자식 또는 충분히 설치에 대 한 가장 높은 자식 되지 않습니다.
- 새 저장 `LayoutData` 캐시에는 값입니다.

<a name="adding_properties" />

#### <a name="adding-properties-backed-by-bindable-properties"></a>바인딩 가능한 속성에서 지 원하는 속성 추가

`WrapLayout` 클래스 정의 `ColumnSpacing` 및 `RowSpacing` 속성 값이 포함 된 행과 열 레이아웃에서 구분 하는 데와 바인딩 가능한 속성에 의해 지원 됩니다. 바인딩 가능한 속성은 다음 코드 예제에 나와 있습니다.

```csharp
public static readonly BindableProperty ColumnSpacingProperty = BindableProperty.Create(
  "ColumnSpacing",
  typeof(double),
  typeof(WrapLayout),
  5.0,
  propertyChanged: (bindable, oldvalue, newvalue) =>
  {
    ((WrapLayout)bindable).InvalidateLayout();
  });

public static readonly BindableProperty RowSpacingProperty = BindableProperty.Create(
  "RowSpacing",
  typeof(double),
  typeof(WrapLayout),
  5.0,
  propertyChanged: (bindable, oldvalue, newvalue) =>
  {
    ((WrapLayout)bindable).InvalidateLayout();
  });
```

각 바인딩 가능한 속성의 속성 변경 처리기를 호출 하는 `InvalidateLayout` 메서드 재정의를 트리거하는 새 레이아웃 전달 된 `WrapLayout`. 자세한 내용은 [InvalidateLayout 메서드를 재정의](#invalidatelayout) 하 고 [OnChildMeasureInvalidated 메서드를 재정의](#onchildmeasureinvalidated)합니다.

<a name="onmeasure" />

#### <a name="overriding-the-onmeasure-method"></a>OnMeasure 메서드 재정의

`OnMeasure` 재정의 다음 코드 예제에 표시 됩니다.

```csharp
protected override SizeRequest OnMeasure(double widthConstraint, double heightConstraint)
{
  LayoutData layoutData = GetLayoutData(widthConstraint, heightConstraint);
  if (layoutData.VisibleChildCount == 0)
  {
    return new SizeRequest();
  }

  Size totalSize = new Size(layoutData.CellSize.Width * layoutData.Columns + ColumnSpacing * (layoutData.Columns - 1),
                layoutData.CellSize.Height * layoutData.Rows + RowSpacing * (layoutData.Rows - 1));
  return new SizeRequest(totalSize);
}
```

재정의 호출 합니다 `GetLayoutData` 메서드 및 생성자는 `SizeRequest` 도 고려 하는 동안 반환된 된 데이터에서 개체를 `RowSpacing` 및 `ColumnSpacing` 속성 값입니다. 에 대 한 자세한 내용은 합니다 `GetLayoutData` 메서드를 참조 하세요 [계산 및 데이터 캐싱](#caching)합니다.

> [!IMPORTANT]
> 합니다 [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) 하 고 [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) 메서드를 반환 하 여 무한 차원에 있는 요청 되지 해야를 [ `SizeRequest` ](xref:Xamarin.Forms.SizeRequest) 값으로 설정 된 속성을 사용 하 여 `Double.PositiveInfinity`. 그러나 하나 이상의 제약 조건 인수 `OnMeasure` 수 `Double.PositiveInfinity`입니다.

<a name="layoutchildren" />

#### <a name="overriding-the-layoutchildren-method"></a>LayoutChildren 메서드 재정의

`LayoutChildren` 재정의 다음 코드 예제에 표시 됩니다.

```csharp
protected override void LayoutChildren(double x, double y, double width, double height)
{
  LayoutData layoutData = GetLayoutData(width, height);

  if (layoutData.VisibleChildCount == 0)
  {
    return;
  }

  double xChild = x;
  double yChild = y;
  int row = 0;
  int column = 0;

  foreach (View child in Children)
  {
    if (!child.IsVisible)
    {
      continue;
    }

    LayoutChildIntoBoundingRegion(child, new Rectangle(new Point(xChild, yChild), layoutData.CellSize));
    if (++column == layoutData.Columns)
    {
      column = 0;
      row++;
      xChild = x;
      yChild += RowSpacing + layoutData.CellSize.Height;
    }
    else
    {
      xChild += ColumnSpacing + layoutData.CellSize.Width;
    }
  }
}
```

재정의 호출 하 여 시작 된 `GetLayoutData` 메서드를 모든 자식의 크기와 각 자식 셀 내에서 해당 위치를 열거 합니다. 호출 하 여 이렇게 합니다 [ `LayoutChildIntoBoundingRegion` ](xref:Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle)) 메서드를 기반으로 하는 사각형 내에서 자식 위치에 사용 되는 해당 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) 및 [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions)속성 값입니다. 자식의 호출 같습니다 [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) 메서드.

> [!NOTE]
> 에 전달 된 사각형을 `LayoutChildIntoBoundingRegion` 메서드 자식 있을 수 있는 전체 영역을 포함 합니다.

에 대 한 자세한 내용은 합니다 `GetLayoutData` 메서드를 참조 하세요 [계산 및 데이터 캐싱](#caching)합니다.

<a name="invalidatelayout" />

#### <a name="overriding-the-invalidatelayout-method"></a>InvalidateLayout 메서드 재정의

합니다 [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) 재정의 자식이 추가 되거나 하나 또는 레이아웃에서 제거 될 때 호출 되의 `WrapLayout` 속성 값을 변경, 다음 코드 예제 에서처럼:

```csharp
protected override void InvalidateLayout()
{
  base.InvalidateLayout();
  layoutInfoCache.Clear();
}
```

재정의 레이아웃을 무효화 하 고 모든 캐시 된 레이아웃 정보를 삭제 합니다.

> [!NOTE]
> 중지 하는 [ `Layout` ](xref:Xamarin.Forms.Layout) 클래스를 호출 하는 [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) 자식을를 레이아웃에서 제거 또는 추가할 때마다 메서드 재정의 [ `ShouldInvalidateOnChildAdded` ](xref:Xamarin.Forms.Layout.ShouldInvalidateOnChildAdded(Xamarin.Forms.View)) 및 [ `ShouldInvalidateOnChildRemoved` ](xref:Xamarin.Forms.Layout.ShouldInvalidateOnChildRemoved(Xamarin.Forms.View)) 메서드와 반환 `false`합니다. 자식이 추가 되거나 제거 되 면 레이아웃 클래스 사용자 지정 프로세스를 구현할 수 있습니다.

<a name="onchildmeasureinvalidated" />

#### <a name="overriding-the-onchildmeasureinvalidated-method"></a>OnChildMeasureInvalidated 메서드 재정의

합니다 [ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) 재정의 레이아웃의 자식 중 크기를 변경 하 고 다음 코드 예제에 표시 되 면 호출 됩니다.

```csharp
protected override void OnChildMeasureInvalidated()
{
  base.OnChildMeasureInvalidated();
  layoutInfoCache.Clear();
}
```

재정의 자식 레이아웃이 무효화 하 고 모든 캐시 된 레이아웃 정보를 삭제 합니다.

<a name="consuming" />

### <a name="consuming-the-wraplayout"></a>WrapLayout 사용

합니다 `WrapLayout` 클래스에 배치 하 여 사용할 수는 [ `Page` ](xref:Xamarin.Forms.Page) 파생 형식에 다음 XAML 코드 예제에서 설명한 것 처럼:

```xaml
<ContentPage ... xmlns:local="clr-namespace:ImageWrapLayout">
    <ScrollView Margin="0,20,0,20">
        <local:WrapLayout x:Name="wrapLayout" />
    </ScrollView>
</ContentPage>
```

해당 하는 C# 코드는 다음과 같습니다.

```csharp
public class ImageWrapLayoutPageCS : ContentPage
{
  WrapLayout wrapLayout;

  public ImageWrapLayoutPageCS()
  {
    wrapLayout = new WrapLayout();

    Content = new ScrollView
    {
      Margin = new Thickness(0, 20, 0, 20),
      Content = wrapLayout
    };
  }
  ...
}
```

자식에 추가할 수 있습니다는 `WrapLayout` 필요에 따라 합니다. 다음 코드 예와 [ `Image` ](xref:Xamarin.Forms.Image) 요소에 추가 되는 `WrapLayout`:

```csharp
protected override async void OnAppearing()
{
  base.OnAppearing();

  var images = await GetImageListAsync();
  foreach (var photo in images.Photos)
  {
    var image = new Image
    {
      Source = ImageSource.FromUri(new Uri(photo + string.Format("?width={0}&height={0}&mode=max", Device.RuntimePlatform == Device.UWP ? 120 : 240)))
    };
    wrapLayout.Children.Add(image);
  }
}

async Task<ImageList> GetImageListAsync()
{
  var requestUri = "https://docs.xamarin.com/demo/stock.json";
  using (var client = new HttpClient())
  {
    var result = await client.GetStringAsync(requestUri);
    return JsonConvert.DeserializeObject<ImageList>(result);
  }
}
```

때 포함 하는 페이지의 `WrapLayout` 비동기적으로 원격 사진 목록이 포함 된 JSON 파일에 액세스를 만들고 샘플 응용 프로그램 표시 되는 [ `Image` ](xref:Xamarin.Forms.Image) 합니다 에추가하는요소각각에대한사진,`WrapLayout`. 이 인해 다음 스크린샷에 표시 된 모양:

![](custom-images/portait-screenshots.png "샘플 응용 프로그램에 대 한 세로 스크린 샷")

다음 스크린샷에서 표시 된 `WrapLayout` 가로 방향으로 회전 된 후:

![](custom-images/landscape-ios.png "샘플 iOS 응용 프로그램 가로 스크린 샷")
![](custom-images/landscape-android.png "샘플 Android 응용 프로그램 가로 스크린 샷") 
 ![ ] (custom-images/landscape-uwp.png " 샘플 UWP 응용 프로그램 가로 스크린 샷")

각 행의 열 수가 사진 크기, 화면 너비를 픽셀 장치 독립적 단위 수에 따라 달라 집니다. [ `Image` ](xref:Xamarin.Forms.Image) 사진, 비동기적으로 로드 하는 요소 및 합니다 `WrapLayout` 클래스를 자주 호출할 받을 해당 [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) 각 메서드 `Image` 요소에는 로드 된 사진에 따라 새 크기를 받습니다.

## <a name="summary"></a>요약

이 문서에서는 사용자 지정 레이아웃 클래스를 작성 하는 방법을 설명 하 고 알아봅니다는 방향에 민감한 `WrapLayout` 페이지에 걸쳐 가로로 자식을 정렬 하 고 그런 다음 후속 자식이 추가 행을 표시 하는 클래스입니다.


## <a name="related-links"></a>관련 링크

- [WrapLayout (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/CustomLayout/WrapLayout/)
- [사용자 지정 레이아웃](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter26.md)
- [Xamarin.Forms에서 사용자 지정 레이아웃 (비디오) 만들기](https://evolve.xamarin.com/session/56e20f83bad314273ca4d81c)
- [레이아웃<T>](xref:Xamarin.Forms.Layout`1)
- [레이아웃](xref:Xamarin.Forms.Layout)
- [VisualElement](xref:Xamarin.Forms.VisualElement)
