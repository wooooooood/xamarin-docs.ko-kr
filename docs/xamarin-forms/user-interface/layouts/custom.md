---
제목: "" 설명: "에서 사용자 지정 레이아웃을 만듭니다. Xamarin.Forms 이 문서에서는 사용자 지정 레이아웃 클래스를 작성 하는 방법을 설명 하 고 페이지 전체에서 자식을 가로로 정렬 한 다음 후속 자식의 표시를 추가 행으로 래핑하는 방향에 민감한 WrapLayout 클래스를 보여 줍니다.
assetid: B0CFDB59-14E5-49E9-965A-3DCCEDAC2E31: xamarin-forms author: davidbritch: dabritch:: 03/29/2017-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="create-a-custom-layout-in-xamarinforms"></a>에서 사용자 지정 레이아웃 만들기Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-customlayout-wraplayout)

_Xamarin은 5 가지 레이아웃 클래스 (StackLayout, AbsoluteLayout, RelativeLayout, Grid 및 Layout)를 정의 하 고 각각은 다른 방식으로 자식을 정렬 합니다. 그러나에서 제공 하지 않는 레이아웃을 사용 하 여 페이지 콘텐츠를 구성 해야 하는 경우도 있습니다 Xamarin.Forms . 이 문서에서는 사용자 지정 레이아웃 클래스를 작성 하는 방법을 설명 하 고 페이지 전체에서 자식을 가로로 정렬 한 다음 이후 자식의 표시를 추가 행으로 래핑하는 방향에 민감한 WrapLayout 클래스를 보여 줍니다._

에서 Xamarin.Forms 모든 레이아웃 클래스는 클래스에서 파생 [`Layout<T>`](xref:Xamarin.Forms.Layout`1) 되며 제네릭 형식과 [`View`](xref:Xamarin.Forms.View) 해당 파생 형식으로 제한 됩니다. 또한 클래스는 `Layout<T>` [`Layout`](xref:Xamarin.Forms.Layout) 자식 요소를 배치 하 고 크기를 조정 하는 메커니즘을 제공 하는 클래스에서 파생 됩니다.

모든 시각적 요소는 *요청* 된 크기 라고 하는 자체의 기본 크기를 결정 해야 합니다. [`Page`](xref:Xamarin.Forms.Page), [`Layout`](xref:Xamarin.Forms.Layout) 및 [`Layout<View>`](xref:Xamarin.Forms.Layout`1) 파생 형식은 자신을 기준으로 자식 또는 자식의 위치와 크기를 결정 하는 일을 담당 합니다. 따라서 레이아웃에는 부모-자식 관계가 포함 됩니다. 부모는 부모-자식 관계를 포함 합니다. 여기에서 부모는 자식 항목의 크기를 결정 하지만 요청 된 자식 크기를 수용 하려고 시도 합니다.

Xamarin.Forms사용자 지정 레이아웃을 만들려면 레이아웃 및 무효화 주기를 충분히 이해 해야 합니다. 이러한 주기는 이제 설명 되어 있습니다.

## <a name="layout"></a>Layout

레이아웃은 페이지를 사용 하 여 시각적 트리 위쪽에서 시작 하 고 시각적 트리의 모든 분기를 진행 하 여 페이지의 모든 시각적 요소를 포함 합니다. 다른 요소에 대 한 부모인 요소는 자신을 기준으로 자식 항목의 크기를 조정 하 고 위치를 지정 해야 합니다.

[`VisualElement`](xref:Xamarin.Forms.VisualElement)클래스는 [ `Measure` ] (f:)를 정의 합니다 Xamarin.Forms . VisualElement. Measure (system.string, System.web, Xamarin.Forms . MeasureFlags))을 (를) (레이아웃 작업의 경우) 및 [ `Layout` ] (f:) 메서드를 측정 Xamarin.Forms 합니다. VisualElement. Layout ( Xamarin.Forms . 사각형)). 요소가 렌더링 되는 사각형 영역을 지정 하는 메서드입니다. 응용 프로그램이 시작 되 고 첫 번째 페이지가 표시 되 면 첫 번째 호출로 구성 된 *레이아웃 사이클이* `Measure` `Layout` 개체에서 시작 하 고를 호출 합니다 [`Page`](xref:Xamarin.Forms.Page) .

1. 레이아웃 주기 동안 모든 부모 요소는 `Measure` 해당 자식에 대해 메서드를 호출 해야 합니다.
1. 자식을 측정 한 후에는 모든 부모 요소가 `Layout` 자식에 대해 메서드를 호출 해야 합니다.

이 주기를 사용 하면 페이지의 모든 시각적 요소가 및 메서드에 대 한 호출을 받을 수 `Measure` `Layout` 있습니다. 프로세스는 다음 다이어그램에 표시 됩니다.

![](custom-images/layout-cycle.png "Xamarin.Forms Layout Cycle")

> [!NOTE]
> 레이아웃에 영향을 주는 변경 사항이 있을 경우 시각적 트리의 하위 집합 에서도 레이아웃 사이클이 발생할 수 있습니다. 여기에는의와 같은 컬렉션에서 추가 또는 제거 되는 항목 [`StackLayout`](xref:Xamarin.Forms.StackLayout) , [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) 요소의 속성 변경 또는 요소 크기 변경 내용이 포함 됩니다.

Xamarin.Forms또는 속성이 있는 모든 클래스 `Content` 에는 `Children` 재정의 가능한 [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) 메서드가 있습니다. 에서 파생 되는 사용자 지정 레이아웃 클래스는 [`Layout<View>`](xref:Xamarin.Forms.Layout`1) 이 메서드를 재정의 하 고 `Measure` [] (f:)를 확인 해야 합니다 Xamarin.Forms . VisualElement. Measure (system.string, System.web, Xamarin.Forms . MeasureFlags)) 및 [ `Layout` ] (f: Xamarin.Forms . VisualElement. Layout ( Xamarin.Forms . 사각형) 메서드를 호출 하 여 원하는 사용자 지정 레이아웃을 제공 합니다.

또한 또는에서 파생 되는 모든 클래스 [`Layout`](xref:Xamarin.Forms.Layout) 는 [`Layout<View>`](xref:Xamarin.Forms.Layout`1) 메서드를 재정의 해야 합니다 [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) .이 경우 레이아웃 클래스는 [ `Measure` ] (f:를 호출 하 여 필요한 크기를 결정 합니다 Xamarin.Forms . VisualElement. Measure (system.string, System.web, Xamarin.Forms . MeasureFlags))의 하위 메서드

> [!NOTE]
> 요소는 요소의 부모 내에서 요소에 사용할 수 있는 공간을 나타내는 *제약 조건을*기반으로 크기를 결정 합니다. []에 전달 된 제약 조건 `Measure` 입니다. (f: Xamarin.Forms . VisualElement. Measure (system.string, System.web, Xamarin.Forms . MeasureFlags)) 및 [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) 메서드는 0에서 사이를 사용할 수 있습니다 `Double.PositiveInfinity` . 요소는 해당 *constrained*[] (f:)에 대 한 호출을 받을 때 제한 되거나 *완전히 제한*됩니다. `Measure` Xamarin.Forms VisualElement. Measure (system.string, System.web, Xamarin.Forms . MeasureFlags))를 사용 합니다 .이 메서드는 요소가 특정 크기로 제한 됩니다. 요소는 하나 이상의 인수를 사용 하 여 해당 메서드에 대 한 호출을 받을 때 *제한*되지 않거나 *부분적으로 제한*됩니다 `Measure` `Double.PositiveInfinity` . 무한 제약 조건은 자동 크기 조정를 나타내는 것으로 간주할 수 있습니다.

## <a name="invalidation"></a>무효화

무효화는 페이지의 요소를 변경 하 여 새 레이아웃 주기를 트리거하는 프로세스입니다. 요소는 더 이상 올바른 크기나 위치가 없는 경우 유효 하지 않은 것으로 간주 됩니다. 예를 들어 [`FontSize`](xref:Xamarin.Forms.Button.FontSize) 의 속성이 변경 되는 경우은 [`Button`](xref:Xamarin.Forms.Button) 더 이상 `Button` 올바른 크기를 갖지 않기 때문에 유효 하지 않은 것으로 간주 됩니다. 그러면의 크기를 조정 하면 `Button` 페이지의 나머지 부분을 통해 레이아웃의 변경 내용이 잔물결 효과를 가질 수 있습니다.

요소는 [`InvalidateMeasure`](xref:Xamarin.Forms.VisualElement.InvalidateMeasure) 일반적으로 요소의 속성을 변경 하 여 요소의 새 크기를 초래할 수 있는 경우 메서드를 호출 하 여 자신을 무효화 합니다. 이 메서드는 [`MeasureInvalidated`](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) 이벤트를 발생 시킵니다 .이 이벤트는 요소의 부모가 새 레이아웃 주기를 트리거하기 위해 처리 합니다.

[`Layout`](xref:Xamarin.Forms.Layout)클래스는 [`MeasureInvalidated`](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) 해당 속성 또는 컬렉션에 추가 된 모든 자식에 대 한 이벤트 처리기를 설정 하 `Content` `Children` 고 자식이 제거 될 때 처리기를 분리 합니다. 따라서 자식 트리의 모든 요소는 자식 항목 중 하나가 크기를 변경할 때마다 경고를 표시 합니다. 다음 다이어그램에서는 시각적 트리의 요소 크기를 변경 하면 트리가 변경 될 수 있는 경우를 보여 줍니다.

![](custom-images/invalidation.png "Invalidation in the Visual Tree")

그러나 클래스는 `Layout` 페이지 레이아웃에 변경 내용의 영향을 제한 하려고 시도 합니다. 레이아웃의 크기가 제한 된 경우에는 자식 크기 변경이 시각적 트리의 부모 레이아웃 보다 높은 항목에는 영향을 주지 않습니다. 그러나 일반적으로 레이아웃 크기를 변경 하면 레이아웃이 자식을 정렬 하는 방식에 영향을 줍니다. 따라서 레이아웃 크기를 변경 하면 레이아웃에 대 한 레이아웃 주기가 시작 되 고 레이아웃에서 해당 및 메서드에 대 한 호출을 받게 됩니다 [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) .

[`Layout`](xref:Xamarin.Forms.Layout)또한 클래스는 [`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout) 메서드와 비슷한 용도의 메서드를 정의 합니다 [`InvalidateMeasure`](xref:Xamarin.Forms.VisualElement.InvalidateMeasure) . `InvalidateLayout`레이아웃의 위치 및 크기를 조정 하는 방법에 영향을 주는 변경 내용이 있을 때마다 메서드를 호출 해야 합니다. 예를 들어, `Layout` 클래스는 `InvalidateLayout` 자식이 레이아웃에서 추가 되거나 제거 될 때마다 메서드를 호출 합니다.

[`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout)[ `Measure` ] (F:)의 반복적인 호출을 최소화 하기 위해 캐시를 구현 하도록를 재정의할 수 있습니다 Xamarin.Forms . VisualElement. Measure (system.string, System.web, Xamarin.Forms . MeasureFlags))을 (를)에 배치 합니다. 메서드를 재정의 하면 `InvalidateLayout` 자식 항목이 레이아웃에 추가 되거나 레이아웃에서 제거 되는 경우에 대 한 알림을 제공 합니다. 마찬가지로 [`OnChildMeasureInvalidated`](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) 레이아웃의 자식 중 하나가 크기를 변경 하는 경우 알림을 제공 하도록 메서드를 재정의할 수 있습니다. 두 메서드를 재정의 하는 경우 캐시를 지워 사용자 지정 레이아웃에 응답 해야 합니다. 자세한 내용은 [레이아웃 데이터 계산 및 캐시](#calculate-and-cache-layout-data)를 참조 하세요.

## <a name="create-a-custom-layout"></a>사용자 지정 레이아웃 만들기

사용자 지정 레이아웃을 만드는 프로세스는 다음과 같습니다.

1. `Layout<View>` 클래스에서 파생되는 클래스를 만듭니다. 자세한 내용은 [Create a WrapLayout](#create-a-wraplayout)을 참조 하세요.
1. [*선택 사항*] Layout 클래스에 설정 해야 하는 매개 변수에 대해 바인딩 가능한 속성에 의해 지원 되는 속성을 추가 합니다. 자세한 내용은 [바인딩 가능한 속성에 의해 지원 되는 속성 추가](#add-properties-backed-by-bindable-properties)를 참조 하세요.
1. 메서드를 재정의 [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) 하 여 [ `Measure` ] (f:)를 호출 합니다 Xamarin.Forms . VisualElement. Measure (system.string, System.web, Xamarin.Forms . MeasureFlags))를 반환 하 고 레이아웃에 대해 요청 된 크기를 반환 합니다. 자세한 내용은 [OnMeasure 메서드 재정의](#override-the-onmeasure-method)를 참조 하세요.
1. 메서드를 재정의 [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) 하 여 [ `Layout` ] (f:)를 호출 합니다 Xamarin.Forms . VisualElement. Layout ( Xamarin.Forms . Rectangle)의 모든 자식에 대 한 메서드를 설정 합니다. [ `Layout` ] (F:)를 호출 하지 못했습니다 Xamarin.Forms . VisualElement. Layout ( Xamarin.Forms . Rectangle)의 경우 레이아웃의 각 자식에 대해 자식이 올바른 크기나 위치를 수신 하지 않기 때문에 자식이 페이지에 표시 되지 않습니다. 자세한 내용은 [LayoutChildren 메서드 재정의](#override-the-layoutchildren-method)를 참조 하세요.

    > [!NOTE]
    > 및 재정의에서 자식을 열거 하는 경우 [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) 속성이로 설정 된 모든 자식을 건너뜁니다 `false` . 이렇게 하면 사용자 지정 레이아웃에서 보이지 않는 자식을 위한 공간이 확보 되지 않습니다.

1. [*선택 사항*] [`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout)자식이 레이아웃에서 추가 되거나 제거 될 때 알리도록 메서드를 재정의 합니다. 자세한 내용은 [Override The InvalidateLayout Method](#override-the-invalidatelayout-method)을 참조 하세요.
1. [*선택 사항*] [`OnChildMeasureInvalidated`](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated)레이아웃의 자식 중 하나가 크기를 변경할 때 알림이 표시 되도록 메서드를 재정의 합니다. 자세한 내용은 [Override The OnChildMeasureInvalidated Method](#override-the-onchildmeasureinvalidated-method)을 참조 하세요.

> [!NOTE]
> [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double))레이아웃의 크기가 자식 노드가 아닌 부모에 의해 관리 되는 경우 재정의는 호출 되지 않습니다. 그러나 제약 조건 중 하나 또는 둘 모두가 무한 이거나 layout 클래스에 기본값이 아닌 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 값 또는 속성 값이 있는 경우 재정의가 호출 됩니다 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) . 이러한 이유로 [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) 재정의는 메서드를 호출 하는 동안 가져온 자식 크기를 사용할 수 없습니다 [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) . 대신 `LayoutChildren` 는 [ `Measure` ] (f:)를 호출 해야 합니다 Xamarin.Forms . VisualElement. Measure (system.string, System.web, Xamarin.Forms . MeasureFlags))를 호출 하기 전에 레이아웃의 자식에 대 한 메서드를 호출 합니다 `Layout` Xamarin.Forms . VisualElement. Layout ( Xamarin.Forms . 사각형) 메서드. 또는 재정의에서 얻은 자식의 크기를 `OnMeasure` 캐시 하 여 나중에 재정의에 대 한 호출을 방지할 수 있습니다 `Measure` `LayoutChildren` . 하지만 레이아웃 클래스는 크기를 다시 가져와야 하는 경우에도 알고 있어야 합니다. 자세한 내용은 [레이아웃 데이터 계산 및 캐시](#calculate-and-cache-layout-data)를 참조 하세요.

그러면 레이아웃 클래스를에 추가 하 [`Page`](xref:Xamarin.Forms.Page) 고 레이아웃에 자식을 추가 하 여 사용할 수 있습니다. 자세한 내용은 [WrapLayout 사용](#consume-the-wraplayout)을 참조 하세요.

### <a name="create-a-wraplayout"></a>WrapLayout 만들기

이 샘플 응용 프로그램에서는 `WrapLayout` 페이지 전체에서 자식을 가로로 정렬 한 다음 후속 자식의 표시를 추가 행으로 래핑하는 방향에 민감한 클래스를 보여 줍니다.

`WrapLayout`클래스는 자식의 최대 크기를 기준으로 *셀 크기*라고 하는 각 자식에 대해 동일한 공간을 할당 합니다. 셀 크기 보다 작은 자식은 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 및 속성 값에 따라 셀 내에 배치 될 수 있습니다 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) .

`WrapLayout`클래스 정의는 다음 코드 예제에 나와 있습니다.

```csharp
public class WrapLayout : Layout<View>
{
  Dictionary<Size, LayoutData> layoutDataCache = new Dictionary<Size, LayoutData>();
  ...
}
```

#### <a name="calculate-and-cache-layout-data"></a>레이아웃 데이터 계산 및 캐시

`LayoutData`구조체는 자식 컬렉션에 대 한 데이터를 다양 한 속성에 저장 합니다.

- `VisibleChildCount`– 레이아웃에 표시 되는 자식의 수입니다.
- `CellSize`– 레이아웃 크기에 맞게 조정 된 모든 자식의 최대 크기입니다.
- `Rows`– 행의 수입니다.
- `Columns`– 열 수입니다.

`layoutDataCache`필드는 여러 값을 저장 하는 데 사용 됩니다 `LayoutData` . 응용 프로그램이 시작 되 면 두 `LayoutData` 개체가 `layoutDataCache` 현재 방향에 대 한 사전에 캐시 됩니다. 하나는 재정의에 대 한 제약 조건 인수이 `OnMeasure` 고 하나는 재정의 `width` `height` 에 대 한 인수입니다 `LayoutChildren` . 장치를 가로 방향으로 회전할 때 `OnMeasure` 재정의 및 `LayoutChildren` 재정의가 다시 호출 되 고,이로 인해 다른 두 `LayoutData` 개체가 사전에 캐시 됩니다. 그러나 장치를 세로 방향으로 반환할 때에는 이미 필요한 데이터가 있으므로 추가 계산은 필요 하지 않습니다 `layoutDataCache` .

다음 코드 예제에서는 `GetLayoutData` `LayoutData` 특정 크기에 따라 구조화 된의 속성을 계산 하는 메서드를 보여 줍니다.

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

`GetLayoutData`메서드는 다음 작업을 수행 합니다.

- 계산 된 값이 캐시에 이미 있는지 여부를 확인 `LayoutData` 하 고 사용할 수 있는 경우 반환 합니다.
- 그렇지 않으면 모든 자식을 열거 하 고, `Measure` 무한 너비와 높이를 사용 하 여 각 자식에서 메서드를 호출 하 고, 최대 자식 크기를 결정 합니다.
- 표시 되는 자식이 하나 이상 있는 경우 필요한 행과 열의 수를 계산 하 고의 크기를 기준으로 자식의 셀 크기를 계산 합니다 `WrapLayout` . 셀 크기는 일반적으로 최대 자식 크기 보다 약간 더 크기는 반면,가 가장 넓은 자식에 비해 너비가 가장 넓은 자식 항목에 비해 충분 하지 않은 경우에는 크기가 작을 수도 있습니다 `WrapLayout` .
- `LayoutData`캐시에 새 값을 저장 합니다.

#### <a name="add-properties-backed-by-bindable-properties"></a>바인딩 가능한 속성에 의해 지원 되는 속성 추가

`WrapLayout`클래스는 `ColumnSpacing` 및 속성을 정의 합니다 `RowSpacing` . 값은 레이아웃의 행과 열을 구분 하는 데 사용 되 고, 바인딩 가능한 속성으로 지원 됩니다. 바인딩 가능한 속성은 다음 코드 예제에 나와 있습니다.

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

각 바인딩 가능 속성의 속성 변경 처리기는 `InvalidateLayout` 메서드 재정의를 호출 하 여에서 새 레이아웃 패스를 트리거합니다 `WrapLayout` . 자세한 내용은 [InvalidateLayout 메서드 재정의](#override-the-invalidatelayout-method) 및 [OnChildMeasureInvalidated 메서드 재정의](#override-the-onchildmeasureinvalidated-method)를 참조 하세요.

#### <a name="override-the-onmeasure-method"></a>OnMeasure 메서드 재정의

`OnMeasure`재정의는 다음 코드 예제에 나와 있습니다.

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

재정의는 메서드를 호출 하 `GetLayoutData` 고 `SizeRequest` , `RowSpacing` 및 속성 값을 고려 하 여 반환 된 데이터에서 개체를 생성 합니다 `ColumnSpacing` . 메서드에 대 한 자세한 내용은 `GetLayoutData` [레이아웃 데이터 계산 및 캐시](#calculate-and-cache-layout-data)를 참조 하세요.

> [!IMPORTANT]
> [ `Measure` ] (F: Xamarin.Forms 입니다. VisualElement. Measure (system.string, System.web, Xamarin.Forms . MeasureFlags)) 및 [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) 메서드는 [`SizeRequest`](xref:Xamarin.Forms.SizeRequest) 속성이로 설정 된 값을 반환 하 여 무한 차원을 요청 해서는 안 됩니다 `Double.PositiveInfinity` . 그러나에 대 한 제약 조건 인수 중 적어도 하나는 `OnMeasure` 일 수 있습니다 `Double.PositiveInfinity` .

#### <a name="override-the-layoutchildren-method"></a>LayoutChildren 메서드 재정의

`LayoutChildren`재정의는 다음 코드 예제에 나와 있습니다.

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

재정의는 메서드를 호출 하 여 시작한 `GetLayoutData` 다음 크기에 대 한 모든 자식을 열거 하 고 각 자식의 셀 내에 배치 합니다. 이렇게 하려면 [ `LayoutChildIntoBoundingRegion` ] (f:)를 호출 Xamarin.Forms 합니다. LayoutChildIntoBoundingRegion ( Xamarin.Forms . VisualElement, Xamarin.Forms . 사각형) 메서드를 사용 합니다 .이 메서드는 및 속성 값을 기반으로 하 여 사각형 안에 자식을 배치 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 합니다. 이는 자식의 [ `Layout` ] (f:)를 호출 하는 것과 같습니다 Xamarin.Forms . VisualElement. Layout ( Xamarin.Forms . 사각형) 메서드.

> [!NOTE]
> 메서드에 전달 되는 사각형에는 `LayoutChildIntoBoundingRegion` 자식이 상주할 수 있는 전체 영역이 포함 됩니다.

메서드에 대 한 자세한 내용은 `GetLayoutData` [레이아웃 데이터 계산 및 캐시](#calculate-and-cache-layout-data)를 참조 하세요.

#### <a name="override-the-invalidatelayout-method"></a>InvalidateLayout 메서드 재정의

[`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout)재정의는 `WrapLayout` 다음 코드 예제와 같이 자식 항목이 레이아웃에 추가 되거나 레이아웃에서 제거 되거나 속성 중 하나가 값을 변경 하는 경우 호출 됩니다.

```csharp
protected override void InvalidateLayout()
{
  base.InvalidateLayout();
  layoutInfoCache.Clear();
}
```

재정의는 레이아웃을 무효화 하 고 캐시 된 레이아웃 정보를 모두 삭제 합니다.

> [!NOTE]
> [`Layout`](xref:Xamarin.Forms.Layout) [`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout) 자식이 레이아웃에 추가 되거나 레이아웃에서 제거 될 때마다 메서드를 호출 하는 클래스를 중지 하려면 [ `ShouldInvalidateOnChildAdded` ] (f:를 재정의 합니다 Xamarin.Forms . 레이아웃. ShouldInvalidateOnChildAdded ( Xamarin.Forms . View)) 및 [ `ShouldInvalidateOnChildRemoved` ] (f: Xamarin.Forms . 레이아웃. ShouldInvalidateOnChildRemoved ( Xamarin.Forms . ) 메서드를 반환 `false` 합니다. 그러면 레이아웃 클래스는 자식이 추가 되거나 제거 될 때 사용자 지정 프로세스를 구현할 수 있습니다.

#### <a name="override-the-onchildmeasureinvalidated-method"></a>OnChildMeasureInvalidated 메서드 재정의

[`OnChildMeasureInvalidated`](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated)재정의는 레이아웃의 자식 중 하나가 크기를 변경 하 고 다음 코드 예제에 표시 되는 경우 호출 됩니다.

```csharp
protected override void OnChildMeasureInvalidated()
{
  base.OnChildMeasureInvalidated();
  layoutInfoCache.Clear();
}
```

재정의는 자식 레이아웃을 무효화 하 고 캐시 된 레이아웃 정보를 모두 삭제 합니다.

### <a name="consume-the-wraplayout"></a>WrapLayout 사용

`WrapLayout` [`Page`](xref:Xamarin.Forms.Page) 다음 XAML 코드 예제에서 보여 주는 것 처럼 클래스를 파생 형식에 배치 하 여 사용할 수 있습니다.

```xaml
<ContentPage ... xmlns:local="clr-namespace:ImageWrapLayout">
    <ScrollView Margin="0,20,0,20">
        <local:WrapLayout x:Name="wrapLayout" />
    </ScrollView>
</ContentPage>
```

해당 c # 코드는 다음과 같습니다.

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

그런 다음 필요에 따라 자식을에 추가할 수 있습니다 `WrapLayout` . 다음 코드 예제에서는에 [`Image`](xref:Xamarin.Forms.Image) 추가 되는 요소를 보여 줍니다 `WrapLayout` .

```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    var images = await GetImageListAsync();
    if (images != null)
    {
        foreach (var photo in images.Photos)
        {
            var image = new Image
            {
                Source = ImageSource.FromUri(new Uri(photo))
            };
            wrapLayout.Children.Add(image);
        }
    }
}

async Task<ImageList> GetImageListAsync()
{
    try
    {
        string requestUri = "https://raw.githubusercontent.com/xamarin/docs-archive/master/Images/stock/small/stock.json";
        string result = await _client.GetStringAsync(requestUri);
        return JsonConvert.DeserializeObject<ImageList>(result);
    }
    catch (Exception ex)
    {
        Debug.WriteLine($"\tERROR: {ex.Message}");
    }

    return null;
}
```

가 포함 된 페이지가 표시 되 면 `WrapLayout` 샘플 응용 프로그램은 사진 목록이 포함 된 원격 JSON 파일에 비동기적으로 액세스 하 여 [`Image`](xref:Xamarin.Forms.Image) 각 사진에 대 한 요소를 만든 다음에 추가 합니다 `WrapLayout` . 이로 인해 결국 다음 스크린샷에 표시된 모양이 됩니다.

![](custom-images/portait-screenshots.png "Sample Application Portrait Screenshots")

다음 스크린샷에는 `WrapLayout` 가로 방향으로 회전 한 후이 표시 됩니다.

![](custom-images/landscape-ios.png "Sample iOS Application Landscape Screenshot")
![](custom-images/landscape-android.png "Sample Android Application Landscape Screenshot")
![](custom-images/landscape-uwp.png "Sample UWP Application Landscape Screenshot")

각 행의 열 수는 사진 크기, 화면 너비 및 장치 독립적 단위 당 픽셀 수에 따라 달라 집니다. [`Image`](xref:Xamarin.Forms.Image)요소는 사진을 비동기적으로 로드 하므로 `WrapLayout` [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) 각 `Image` 요소가 로드 된 사진을 기반으로 새 크기를 받을 때 클래스는 해당 메서드에 대 한 자주 호출을 받습니다.

## <a name="related-links"></a>관련 링크

- [WrapLayout (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-customlayout-wraplayout)
- [사용자 지정 레이아웃](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter26.md)
- [에서 사용자 지정 레이아웃 만들기 Xamarin.Forms (비디오)](https://www.youtube.com/watch?v=sxjOqNZFhKU)
- [Layout\<T>](xref:Xamarin.Forms.Layout`1)
- [레이아웃](xref:Xamarin.Forms.Layout)
- [VisualElement](xref:Xamarin.Forms.VisualElement)
