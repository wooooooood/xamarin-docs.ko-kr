---
title: "사용자 지정 레이아웃 만들기"
description: "Xamarin.Forms 4 개의 레이아웃 클래스 – StackLayout, AbsoluteLayout, RelativeLayout, 및 눈금을 정의 하 고 자식을 다른 방식으로 정렬 하는 각 키를 누릅니다. 그러나 경우에 레이아웃 Xamarin.Forms에서 제공 하지 않는 사용 하 여 페이지 콘텐츠를 구성 하기 위해 합니다. 이 문서는 사용자 지정 레이아웃 클래스를 작성 하는 방법을 설명 하 고 자식을 페이지에 걸쳐 가로로 정렬 하 고 그런 다음 후속 자식이 추가 행을 표시 하는 방향/반자 WrapLayout 클래스를 보여 줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: B0CFDB59-14E5-49E9-965A-3DCCEDAC2E31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/29/2017
ms.openlocfilehash: 4c7bf5f2c867faef7d9baf8d511393dbe2d129a4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-custom-layout"></a>사용자 지정 레이아웃 만들기

_Xamarin.Forms 4 개의 레이아웃 클래스 – StackLayout, AbsoluteLayout, RelativeLayout, 및 눈금을 정의 하 고 자식을 다른 방식으로 정렬 하는 각 키를 누릅니다. 그러나 경우에 레이아웃 Xamarin.Forms에서 제공 하지 않는 사용 하 여 페이지 콘텐츠를 구성 하기 위해 합니다. 이 문서는 사용자 지정 레이아웃 클래스를 작성 하는 방법을 설명 하 고 자식을 페이지에 걸쳐 가로로 정렬 하 고 그런 다음 후속 자식이 추가 행을 표시 하는 방향/반자 WrapLayout 클래스를 보여 줍니다._

## <a name="overview"></a>개요

Xamarin.forms에 모든 레이아웃 클래스에서 파생 된 [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) 클래스 및 제네릭 형식이 제한 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) 및 파생된 형식입니다. 차례로 `Layout<T>` 클래스에서 파생 되는 [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) 요소 배치 및 크기 조정 자식 메커니즘을 제공 하는 클래스입니다.

모든 시각적 요소는를 라고 하는 자체 기본 크기를 결정 하는 *요청한* 크기입니다. [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/), 및 [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) 파생 유형은 자신의 자식 또는 자체를 기준으로 자식 항목의 크기와 위치를 결정 합니다. 따라서 레이아웃 여기서 부모 해야 자식의 크기를 결정 하는 부모-자식 관계를 포함 하지만 자식의 요청된 된 크기에 맞게 시도 합니다.

Xamarin.Forms 레이아웃 및 무효화 주기 철저 하 게 파악 하는 사용자 지정 레이아웃을 만들 필요 합니다. 이제 이러한 주기를 설명 합니다.

## <a name="layout"></a>레이아웃

레이아웃 페이지를 사용 하 여 시각적 트리 맨 위에 있는 차고 페이지에서 모든 시각적 요소를 포함 하도록 시각적 트리의 모든 분기를 진행 합니다. 다른 요소에 대 한 부모 요소는 자체를 기준으로 해당 자식 항목을 배치 및 크기 조정 하는 일을 담당 합니다.

[ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) 클래스 정의 [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) 레이아웃 작업에 대 한 요소를 측정 하는 방법 및 [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) 지정 하는 메서드 요소는 사각형 영역 내에서 렌더링 됩니다. 응용 프로그램이 시작 되 고 첫 번째 페이지가 표시 되는 *레이아웃 주기가* 중 구성 된 첫 번째 `Measure` 호출, 차례로 `Layout` 호출 하 고, 시작에는 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 개체:

1. 모든 부모 요소 레이아웃 주기 동안 호출 됩니다는 `Measure` 자식에서 메서드가 있습니다.
1. 모든 부모 요소를 호출 하는 데 자식을, 측정 한 후의 `Layout` 자식에서 메서드.

이 주기를 사용 하면 페이지에서 모든 시각적 요소에 대 한 호출을 수신 했음을 `Measure` 및 `Layout` 메서드. 프로세스는 다음 다이어그램에 표시 됩니다.

![](custom-images/layout-cycle.png "Xamarin.Forms 레이아웃 주기")

> [!NOTE]
> Note 레이아웃에 영향을 변경 하면 레이아웃 주기 시각적 트리의 하위 집합에서 발생할 수 있습니다. 항목이 추가 되거나에서 같이 이러한 컬렉션에서 제거 되 고 여기에 한 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), 변경에는 [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) 요소나 요소의 크기가 변경의 속성입니다.

에 모든 Xamarin.Forms 클래스는 `Content` 또는 `Children` 속성에는 재정의 가능한 [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) 메서드. 파생 된 사용자 지정 레이아웃 클래스 [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) 이 메서드를 재정의 하 고 있는지 확인 해야는 [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) 및 [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) 메서드 요소의 모든 자식에 원하는 사용자 지정 레이아웃을 제공 하기에 호출 됩니다.

또한 모든 클래스에서 파생 된 [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) 또는 [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) 재정의 해야 합니다는 [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) 도 레이아웃 클래스 메서드 호출 하 여는 데 필요한 크기를 결정은 [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) 자식 메서드.

> [!NOTE]
> 요소에 따라 크기가 결정 *제약 조건을*, 할 공간을 상위 요소의 내에서 요소에 사용할 수 있습니다. 제약 조건에 전달 되는 [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) 및 [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) 메서드 범위는 0에서 `Double.PositiveInfinity`합니다. 요소가 *제한*, 또는 *완전히 구속*에 대 한 호출을 받을 때, 해당 [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) 메서드-유한 인수를 갖는 요소가 제한 됩니다 특정 크기입니다. 요소가 *무제한*, 또는 *부분적으로 제한*에 대 한 호출을 받을 때, 해당 `Measure` 같은 하나 이상의 인수를 사용 하 여 메서드 `Double.PositiveInfinity` – 무한 제약 조건 수 크기 자동 조정 알리는 것으로 간주 합니다.

## <a name="invalidation"></a>무효화

무효화는 페이지의 요소에 변경 되는 기준인 새 레이아웃 주기가 트리거됩니다 프로세스입니다. 요소는 올바른 크기 또는 위치 더 이상 없을 때에 잘못 된 간주 됩니다. 예를 들어 경우는 [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontSize/) 속성은 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) 변경의 `Button` 올바른 크기가 더 더 이상 유효 하지 않은 것으로 간주 됩니다. 크기 조정 된 `Button` 변경의 영향을 미치기 페이지의 나머지 부분을 레이아웃에 있을 수 있습니다.

요소를 호출 하 여 자신을 무효화는 [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/) 메서드를 일반적으로 변경 될 때 요소의 속성이 있는 요소의 새 크기 발생할 수 있습니다. 이 메서드를 발생는 [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) 새 레이아웃 주기가 트리거하는 요소의 부모를 처리 하는 이벤트입니다.

[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) 에 대 한 처리기를 설정 하는 클래스는 [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) 이벤트에 추가 된 모든 자식 해당 `Content` 속성 또는 `Children` 컬렉션 하 고 처리기를 분리 때는 자식도 제거 됩니다. 따라서 자식이 있는 시각적 트리의 모든 요소 크기 중 하나가 변경 될 때마다 경고가 표시 됩니다. 다음 다이어그램에서는 시각적 트리에 있는 요소의 크기가 변경 트리를 ripple 변경 내용이 발생할 수 있습니다 방법을 보여 줍니다.

![](custom-images/invalidation.png "시각적 트리에서 무효화")

그러나는 `Layout` 클래스는 페이지의 레이아웃에 자녀의 크기가 변경의 영향을 제한 하려고 합니다. 제한 크기인 레이아웃을 사용 하는 경우 다음 자식 크기 변경 없이 시각적 트리의 부모 레이아웃 보다 높은 값 있습니다. 그러나 일반적으로 레이아웃의 크기 변경에 영향을 줍니다 레이아웃 자식을 정렬 하는 방법을 합니다. 따라서 모든 변경 레이아웃의 크기에는 레이아웃에 대 한 레이아웃 주기를 시작 하 고 레이아웃에 대 한 호출을 받게 됩니다 해당 [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) 및 [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) 메서드.

[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) 클래스도 정의 [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) 와 유사한 변수가 있는 메서드는 [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/) 메서드. `InvalidateLayout` 레이아웃 배치 하 고 자식의 크기를 조정 하는 방법에 영향을 주는 변경이 수행 될 때마다 메서드를 호출 해야 합니다. 예를 들어는 `Layout` 클래스 호출는 `InvalidateLayout` 자식에 추가 하거나 레이아웃에서 제거 될 때마다 메서드.

[ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) 반복 해 서 호출을 최소화 하기 위해 캐시를 구현 하는 재정의 될 수 있습니다는 [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) 메서드는 레이아웃의 자식입니다. 재정의 `InvalidateLayout` 메서드는 자식 항목은에 추가 또는 레이아웃에서 제거 하는 경우에 대 한 알림을 제공 합니다. 마찬가지로,는 [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) 레이아웃의 자식 중 하나가 크기를 변경 하는 경우 알림을 제공 하는 메서드를 재정의할 수 있습니다. 두 메서드 재정의 대 한 사용자 지정 레이아웃은 캐시의 선택을 취소 하 여 응답 해야 합니다. 자세한 내용은 참조 [계산 및 데이터 캐싱](#caching)합니다.

## <a name="creating-a-custom-layout"></a>사용자 지정 레이아웃 만들기

사용자 지정 레이아웃을 만드는 프로세스는 다음과 같습니다.

1. `Layout<View>` 클래스에서 파생되는 클래스를 만듭니다. 자세한 내용은 참조 [만들기는 WrapLayout](#creating)합니다.
1. [*선택적*] 레이아웃 클래스에 설정 해야 하는 모든 매개 변수에 대 한 바인딩 가능한 속성에서 지 원하는 속성을 추가 합니다. 자세한 내용은 참조 [추가 속성에 바인딩 가능한 속성을 받으며](#adding_properties)합니다.
1. 재정의 [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) 메서드를 호출 하는 [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) 레이아웃에 대 한 모든 레이아웃의 자식 및 반환 요청 된 크기에서 메서드가 있습니다. 자세한 내용은 참조 [OnMeasure 메서드 재정의](#onmeasure)합니다.
1. 재정의 [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) 메서드를 호출 하 여 [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) 레이아웃의 모든 자식에서 해당 메서드. 호출에 실패는 [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) 레이아웃의 각 자식에 대 한 메서드를 정확한 크기 또는 위치를 수신 하지 자식 고 따라서 자식 페이지에 표시 될 됩니다. 자세한 내용은 참조 [LayoutChildren 메서드 재정의](#layoutchildren)합니다.

  > [!NOTE]
>  자식의 열거는 [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) 및 [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) 재정의 건너뛰고 모든 자식 인 [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) 속성이`false`. 이렇게 하면가 사용자 지정 레이아웃 보이지 않는 자식에 대 한 공간에서 이동할 되지 않습니다.

1. [*선택적*] 재정의 [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) 메서드 자식이 추가 되거나 레이아웃에서 제거 하는 경우 알림을 받을 수 있습니다. 자세한 내용은 참조 [InvalidateLayout 메서드 재정의](#invalidatelayout)합니다.
1. [*선택적*] 재정의 [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) 메서드 크기 레이아웃의 자식 중 하나가 변경 될 때 알림을 받을 수 있습니다. 자세한 내용은 참조 [OnChildMeasureInvalidated 메서드 재정의](#onchildmeasureinvalidated)합니다.

> [!NOTE]
> [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) 자식 보다는 해당 부모에 의해 레이아웃의 크기 제한 하는 경우에 재정의 호출할 수 없습니다. 제약 조건 중 하나 이상이 무한, 또는 레이아웃 클래스에 기본이 아닌 경우 재정의 호출 하는 반면 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) 또는 [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) 속성 값입니다. 이러한 이유로 [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) 재정의 하는 동안 가져온 자식 크기를 사용할 수 없게 된 [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) 메서드를 호출 합니다. 대신, `LayoutChildren` 호출 해야는 [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) 호출 하기 전에 레이아웃의 자식에서 해당 메서드는 [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) 메서드. 자식의 크기에서 얻은 또는 `OnMeasure` 나중 방지 하려면 재정의 캐시할 수 있습니다 `Measure` 에서 호출의 `LayoutChildren` override, 하지만 레이아웃 클래스의 크기를 다시 가져올 해야 하는 경우를 알고 있어야 합니다. 자세한 내용은 참조 [계산 및 레이아웃 데이터 캐싱](#caching)합니다.

그러면 추가 하 여 레이아웃 클래스를 사용할 수는 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), 및 레이아웃에 자식을 추가 하 여 합니다. 자세한 내용은 참조 [는 WrapLayout 소비](#consuming)합니다.

<a name="creating" />

### <a name="creating-a-wraplayout"></a>WrapLayout 만들기

샘플 응용 프로그램에서는 방향 구분 `WrapLayout` 자식을 페이지에 걸쳐 가로로 정렬 하 고 그런 다음 후속 자식이 추가 행을 표시 하는 클래스입니다.

`WrapLayout` 클래스가 같은 양의 공간이 라고 각 자식에 대 한 할당 하는 *셀 크기*자식 항목의 최대 크기에 따라 합니다. 자식 셀 크기와 셀 안에 배치 될 수 있습니다는 것 보다 작은에 따라 자신의 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) 및 [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) 속성 값입니다.

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

`LayoutData` 구조 다양 한 속성에에서 자식 컬렉션에 대 한 데이터를 저장 합니다.

- `VisibleChildCount` -레이아웃에 표시 되는 자식의 수입니다.
- `CellSize` – 레이아웃의 크기를 조정 모든 자식의 최대 크기입니다.
- `Rows` -행의 수입니다.
- `Columns` -열 수 있습니다.

`layoutDataCache` 필드는 여러 저장에 사용 `LayoutData` 값입니다. 응용 프로그램이 시작 되 면 두 `LayoutData` 개체에 캐시 됩니다는 `layoutDataCache` 사전에 대 한 제약 조건 인수에 대 한 현재 방향 –에 대 한는 `OnMeasure` override, 되 고 다른 하나는 `width` 및 `height` 인수 에 `LayoutChildren` 재정의 합니다. 가로 방향으로 장치를 회전할 때는 `OnMeasure` 재정의 및 `LayoutChildren` 재정의 다시 호출 될, 다른 두 결과적 됩니다 `LayoutData` 사전에 캐시 되는 개체입니다. 그러나 세로 방향으로 장치를 반환할 때 더 이상 없는 계산 되므로 사항은 `layoutDataCache` 필요한 데이터에 이미 있습니다.

다음 코드 예제는 `GetLayoutData` 의 속성을 계산 하는 메서드는 `LayoutData` 구조적 특정 크기에 따라:

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

- 계산 여부를 결정 `LayoutData` 값은 캐시에 이미 및 사용 가능한 경우이 반환 합니다.
- 그렇지 않으면 호출 모든 자식을 통해 열거는 `Measure` 메서드 무한대의 너비 및 높이와 각 자식에 자식 최대 크기를 결정 합니다.
- 제공 되는 하나 이상의 자식 인 행과 열 필요한 개수를 계산 하의 크기에 따라 자식에 대 한 셀 크기를 계산 합니다는 `WrapLayout`합니다. 셀 크기는 최대 자식 크기 보다 약간 더 넓은 일반적으로 있지만 있는지 수 보다 작은 경우는 `WrapLayout` 있을 만큼의 너비만 가장 넓은 자식 또는 충분히 높이 대 한 가장 높은 자식에 대 한 되지 않습니다.
- 새 저장 `LayoutData` 캐시의 값입니다.

<a name="adding_properties" />

#### <a name="adding-properties-backed-by-bindable-properties"></a>바인딩 가능한 속성에 의해 지원 되는 속성 추가

`WrapLayout` 클래스 정의 `ColumnSpacing` 및 `RowSpacing` 속성을 해당 값 구분 행과 열 레이아웃에서 사용 됩니다 하 고 있는 바인딩 가능한 속성에 의해 지원 됩니다. 바인딩 가능한 속성은 다음 코드 예제에 나와 있습니다.

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

바인딩 가능한 각 속성의 속성 변경 처리기를 호출는 `InvalidateLayout` 새 레이아웃을 트리거할 메서드 재정의 전달 된 `WrapLayout`합니다. 자세한 내용은 참조 [InvalidateLayout 메서드 재정의](#invalidatelayout) 및 [OnChildMeasureInvalidated 메서드 재정의](#onchildmeasureinvalidated)합니다.

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

재정의 호출는 `GetLayoutData` 메서드 및 생성자는 `SizeRequest` 도 고려 하는 동안 반환된 된 데이터에서 개체는 `RowSpacing` 및 `ColumnSpacing` 속성 값입니다. 에 대 한 자세한 내용은 `GetLayoutData` 메서드를 참조 [계산 및 데이터 캐싱](#caching)합니다.

> [!IMPORTANT]
> [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) 및 [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) 메서드를 반환 하 여 무한 차원을 요청 하지 해야는 [ `SizeRequest` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SizeRequest/) 값으로 설정 된 속성을 `Double.PositiveInfinity`. 그러나 제약 조건 인수 중 하나 이상 `OnMeasure` 수 `Double.PositiveInfinity`합니다.

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

에 대 한 호출으로 시작 하는 재정의 `GetLayoutData` 메서드를 한 다음 모든 크기와 위치를 각 자식 셀 내에 자식 항목을 열거 합니다. 호출 하 여 이렇게는 [ `LayoutChildIntoBoundingRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/) 메서드를 기반으로 사각형 내에서 자식 위치를 지정 하는 데 사용 되는 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) 및 [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)속성 값입니다. 자식에 대 한 호출을 동일 [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) 메서드.

> [!NOTE]
> 사각형에 전달 하는 `LayoutChildIntoBoundingRegion` 메서드에 포함 전체 영역 자식 있을 수 있습니다.

에 대 한 자세한 내용은 `GetLayoutData` 메서드를 참조 [계산 및 데이터 캐싱](#caching)합니다.

<a name="invalidatelayout" />

#### <a name="overriding-the-invalidatelayout-method"></a>InvalidateLayout 메서드 재정의

[ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) 재정의 자식이 추가 되거나 하나 또는 레이아웃에서 제거 될 때 호출 되의 `WrapLayout` 속성 값을 변경, 다음 코드 예제에 나와 있는 것 처럼:

```csharp
protected override void InvalidateLayout()
{
  base.InvalidateLayout();
  layoutInfoCache.Clear();
}
```

재정의 레이아웃을 무효화 하 고 모든 캐시 된 레이아웃 정보를 무시 합니다.

> [!NOTE]
> 중지 하는 [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) 클래스를 호출 하는 [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) 자식에 추가 하거나는 레이아웃에서 제거 될 때마다 메서드 재정의 [ `ShouldInvalidateOnChildAdded` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.ShouldInvalidateOnChildAdded/p/Xamarin.Forms.View/) 및 [ `ShouldInvalidateOnChildRemoved` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.ShouldInvalidateOnChildRemoved/p/Xamarin.Forms.View/) 메서드와 반환 `false`합니다. 자식 항목을 추가 하거나 제거할 때 레이아웃 클래스 사용자 지정 프로세스를 구현할 수 있습니다.

<a name="onchildmeasureinvalidated" />

#### <a name="overriding-the-onchildmeasureinvalidated-method"></a>OnChildMeasureInvalidated 메서드 재정의

[ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) 재정의 크기를 변경 하 고 다음 코드 예제에 표시 되어 레이아웃의 자식 중 호출 됩니다.

```csharp
protected override void OnChildMeasureInvalidated()
{
  base.OnChildMeasureInvalidated();
  layoutInfoCache.Clear();
}
```

재정의 자식 레이아웃을 무효화 하 고 모든 캐시 된 레이아웃 정보를 무시 합니다.

<a name="consuming" />

### <a name="consuming-the-wraplayout"></a>WrapLayout 사용

`WrapLayout` 클래스에 배치 하 여 사용할 수는 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 다음 XAML 코드 예제에서와 같이 파생 된 유형인:

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

자식에 추가할 수 있습니다는 `WrapLayout` 필요에 따라 합니다. 다음 코드 예제와 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 요소에 추가 되는 `WrapLayout`:

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

때 포함 된 페이지는 `WrapLayout` 표시 되 면 비동기적으로 샘플 응용 프로그램 사진 목록이 포함 된 원격 JSON 파일에 액세스, 만듭니다는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 는 에추가하는요소각각에대한사진,`WrapLayout`. 다음 스크린샷에 표시 된 모양 결과이 됩니다.

![](custom-images/portait-screenshots.png "샘플 응용 프로그램 세로 스크린샷")

다음 스크린 샷 표시는 `WrapLayout` 를 가로 방향으로 회전 된 후:

![](custom-images/landscape-ios.png "IOS 응용 프로그램 가로 스크린 샷 샘플")
![](custom-images/landscape-android.png "샘플 Android 응용 프로그램 가로 스크린 샷") 
 ![ ] (custom-images/landscape-uwp.png " 샘플 UWP 응용 프로그램 가로 스크린 샷")

각 행의 열 수가 사진 크기, 화면 너비 및 장치 독립적 단위 당 픽셀 수에 따라 달라 집니다. [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 사진, 비동기적으로 로드 하는 요소 이므로 `WrapLayout` 클래스에 대 한 자주 호출을 받게 됩니다 해당 [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) 메서드 각각 `Image` 로드 된 사진에 따라 새 크기를 받습니다.

## <a name="summary"></a>요약

이 문서는 사용자 지정 레이아웃 클래스를 작성 하는 방법을 설명 하 고 방향 구분을 보여 주는 `WrapLayout` 자식을 페이지에 걸쳐 가로로 정렬 하 고 그런 다음 후속 자식이 추가 행을 표시 하는 클래스입니다.


## <a name="related-links"></a>관련 링크

- [WrapLayout (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/CustomLayout/WrapLayout/)
- [사용자 지정 레이아웃](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter26.md)
- [사용자 지정 레이아웃 xamarin.forms에서 만들기 (비디오)](https://evolve.xamarin.com/session/56e20f83bad314273ca4d81c)
- [레이아웃<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/)
- [레이아웃](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)
- [VisualElement](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)
