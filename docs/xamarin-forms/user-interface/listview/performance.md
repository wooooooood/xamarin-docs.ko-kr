---
title: ListView 성능
description: ListView 데이터를 표시 하기 위한 강력한 뷰일 경우에 몇 가지 제한 사항이 있습니다. 이 문서에서는 응용 프로그램에서 Xamarin.Forms ListView를 사용 하 여 뛰어난 성능을 보장 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 1B085639-652C-4862-86EB-5D55D32B9395
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2017
ms.openlocfilehash: 4a0a7a4db4b0ca982a162ec3a0b67dc729af0ed2
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655935"
---
# <a name="listview-performance"></a>ListView 성능

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)

모바일 응용 프로그램을 작성할 때 성능에 중요 합니다. 사용자가 제대로 찾아 오셨습니다 부드러운 스크롤 및 빠른 로드 시간을 예상 합니다. 사용자의 기대를 충족 하기 위해 실패 한 응용 프로그램 저장소에 대 한 등급 비용이 되거나를 기간 업무 응용 프로그램의 경우 조직으로 시간과 비용을 비용 됩니다.

하지만 [ `ListView` ](xref:Xamarin.Forms.ListView) 강력한 뷰입니다 데이터를 표시 하는 것에 대 한 몇 가지 제한 사항이 있습니다. 깊이 중첩 된 뷰 계층 구조를 포함 하거나 측정을 많이 해야 하는 특정 레이아웃을 사용 하는 경우에 특히 사용자 지정 셀을 사용 하는 경우 스크롤 성능이 저하 될 수 있습니다. 다행 스럽게도 가지 방법이 성능 저하를 방지 하는 데 사용할 수 있습니다.

<a name="cachingstrategy" />

## <a name="caching-strategy"></a>캐싱 전략

Listview는 수 있는 것 보다 훨씬 더 많은 데이터를 표시 하는 데 종종 사용 화면 적합 합니다. 예를 들어 음악 앱을 사용 하는 것이 좋습니다. 수백 곡의 라이브러리에는 수천 개의 항목이 있을 수 있습니다. 모든 노래에 대 한 행을 만드는 것에 간단한 접근 방법은 성능 저하를 해야 합니다. 방법은 중요 한 메모리가 낭비 되 고 및 탐색을 찾아 스크롤하기가 저하 될 수 있습니다. 다른 방법은 만들고 뷰로 데이터가 스크롤됩니다으로 행을 제거 하는 것입니다. 이렇게 하려면 상수 인스턴스화 및 매우 느려질 수 있는 뷰 개체의 정리 합니다.

네이티브 메모리를 절약 하기 위해 [ `ListView` ](xref:Xamarin.Forms.ListView) 각 플랫폼에 해당 하는 기본 제공 기능이 다시 행을 사용 합니다. 화면에 보이는 셀만 메모리에 로드 되 고 **콘텐츠** 기존 셀에 로드 됩니다. 이렇게 하면 응용 프로그램을에서 수천 개의 개체, 시간과 메모리를 인스턴스화할 필요가지 않습니다.

Xamarin.Forms 허용 [ `ListView` ](xref:Xamarin.Forms.ListView) 셀을 통해 다시 사용 합니다 [ `ListViewCachingStrategy` ](xref:Xamarin.Forms.ListViewCachingStrategy) 열거형에는 다음 값입니다.

```csharp
public enum ListViewCachingStrategy
{
  RetainElement,   // the default value
  RecycleElement,
  RecycleElementAndDataTemplate
}
```

> [!NOTE]
> 유니버설 Windows 플랫폼 (UWP) 무시 합니다 [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) 캐싱 전략을 사용 하기 때문에 항상 캐싱 성능 향상을 위해. 기본적으로 동작 합니다 처럼 합니다 [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) 캐싱 전략 적용 됩니다.

### <a name="retainelement"></a>RetainElement

[ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) 지정 캐싱 전략을 [ `ListView` ](xref:Xamarin.Forms.ListView) 는 목록의 각 항목에 대 한 셀을 생성 하 고 기본값인 `ListView` 동작 합니다. 다음과 같은 경우에 일반적으로 사용 해야 합니다.

- 각 셀에 많은 바인딩의 경우 (20-30 개 이상).
- 경우 셀 템플릿은 자주 변경 됩니다.
- 경우 하면 테스트는 `RecycleElement` 감소 실행 속도가 전략 결과 캐시 합니다.

작업의 결과 인식 해야 합니다 [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) 사용자 지정 셀을 사용 하 여 작업 하는 경우에 캐싱 전략입니다. 셀 초기화 코드는 각 셀 생성, 실행 해야는 초당 여러 번 사용할 수 있습니다. 이 상황에서 레이아웃 페이지에서 문제가 있었던 같은 기술을 여러 중첩을 사용 하 여 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 인스턴스를 설정 하는 경우에 성능 병목 현상이 만들어지고 실시간에서으로 사용자 스크롤합니다.

<a name="recycleelement" />

### <a name="recycleelement"></a>RecycleElement

합니다 [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) 지정 캐싱 전략을 [ `ListView` ](xref:Xamarin.Forms.ListView) 목록 셀을 재활용 하 여 메모리 사용 공간 및 실행 속도 최소화 하려고 합니다. 이 모드는 성능 향상을 항상 제공 하지 않습니다 하 고 테스트 모든 향상 된 기능을 확인 하려면 수행 해야 합니다. 그러나 일반적으로 기본으로 선택 합니다 이며 다음과 같은 경우에 사용 해야 합니다.

- 각 셀에 바인딩 수가 보통을 작은 경우.
- 때 각 셀 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 모든 셀 데이터를 정의 합니다.
- 각 셀이 변경 되지 않는 셀 템플릿을 사용 하 여 아주 비슷한 있을 때.

가상화 중 셀을 업데이트 하는 바인딩 컨텍스트에 없는 고 따라서 응용 프로그램에서이 모드를 사용 하는 경우 바인딩 컨텍스트 업데이트 적절 하 게 처리 됩니다. 셀에 대 한 모든 데이터 바인딩 컨텍스트를 제공 해야 합니다 또는 일관성 오류가 발생할 수 있습니다. 셀 데이터를 표시할 데이터 바인딩을 사용 하 여이 수행할 수 있습니다. 셀 데이터 설정할 또는 `OnBindingContextChanged` 재정의 대신 다음 코드 예제에서 설명한 것 처럼 사용자 지정 셀의 생성자에서:

```csharp
public class CustomCell : ViewCell
{
  Image image = null;

  public CustomCell ()
  {
    image = new Image();
    View = image;
  }

  protected override void OnBindingContextChanged ()
  {
    base.OnBindingContextChanged ();

    var item = BindingContext as ImageItem;
    if (item != null) {
      image.Source = item.ImageUrl;
    }
  }
}
```

자세한 내용은 [바인딩 컨텍스트 변경](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#binding-context-changes)합니다.

IOS 및 Android에서 셀 사용자 지정 렌더러를 사용 하는 경우 해야 확인 속성 변경 알림 올바르게 구현 하는 합니다. 셀 재사용 되는 바인딩 컨텍스트를 사용할 수 있는 셀의 업데이트 된 경우 해당 속성 값 변경 됩니다 `PropertyChanged` 이벤트 발생 합니다. 자세한 내용은 [Viewcell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)합니다.

#### <a name="recycleelement-with-a-datatemplateselector"></a>DataTemplateSelector 사용 하 여 RecycleElement

경우는 [ `ListView` ](xref:Xamarin.Forms.ListView) 사용 하 여를 [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) 선택 하는 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) 캐싱 전략을 캐시 하지 않습니다 `DataTemplate`s입니다. 대신는 `DataTemplate` 목록에서 데이터의 각 항목에 대 한 선택입니다.

> [!NOTE]
> 합니다 [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) 필수에 캐싱 전략 Xamarin.Forms 2.4에 도입 된 때를 [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) 선택 하 라는 메시지가 표시 되는 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)각 `DataTemplate` 동일 하 게 반환 해야 합니다 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) 형식입니다. 예를 들어를 [ `ListView` ](xref:Xamarin.Forms.ListView) 사용 하 여를 `DataTemplateSelector` 중 하나를 반환할 수 있는 `MyDataTemplateA` (여기서 `MyDataTemplateA` 반환을 `ViewCell` 형식의 `MyViewCellA`), 또는 `MyDataTemplateB` (여기서 `MyDataTemplateB`반환을 `ViewCell` 형식의 `MyViewCellB`), `MyDataTemplateA` 반환 해야 반환 됩니다 `MyViewCellA` 그렇지 않으면 예외가 throw 됩니다.

### <a name="recycleelementanddatatemplate"></a>RecycleElementAndDataTemplate

[ `RecycleElementAndDataTemplate` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) 기반 캐싱 전략을 [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) 때 또한 함으로써 캐싱 전략을 [ `ListView` ](xref:Xamarin.Forms.ListView) 를사용하여[ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) 선택 하는 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), `DataTemplate`의목록에서 항목의 형식에 의해 캐시 됩니다. 따라서 `DataTemplate`의항목 인스턴스당 한 번이 아니라 항목 유형 마다 한 번 선택 합니다.

> [!NOTE]
> [ `RecycleElementAndDataTemplate` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) 필수에 캐싱 전략은를 `DataTemplate`반환한 s 합니다 [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) 사용 해야 합니다는 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type)) 사용 하는 생성자를 `Type`입니다.

### <a name="setting-the-caching-strategy"></a>캐싱 전략을 설정합니다.

합니다 [ `ListViewCachingStrategy` ](xref:Xamarin.Forms.ListViewCachingStrategy) 열거형 값이 지정 된 된 [ `ListView` ](xref:Xamarin.Forms.ListView) 생성자 오버 로드를 다음 코드 예제 에서처럼:

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

XAML에서 설정 된 `CachingStrategy` 아래 코드에 표시 된 대로 특성:

```xaml
<ListView CachingStrategy="RecycleElement">
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
              ...
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

생성자에서는 캐싱 전략 인수를 설정 하는 것과 동일한 효과가 C#; 없습니다 `CachingStrategy` 속성을 `ListView`입니다.

#### <a name="setting-the-caching-strategy-in-a-subclassed-listview"></a>서브클래싱된 ListView에서 캐싱 전략을 설정합니다.

설정 합니다 `CachingStrategy` 서브클래싱된를에서 XAML의 특성 [ `ListView` ](xref:Xamarin.Forms.ListView) 있기 때문에 원하는 동작을 생성 하지 것입니다 없습니다 `CachingStrategy` 속성을 `ListView`합니다. 또한 [XAMLC](~/xamarin-forms/xaml/xamlc.md) 를 사용 하는 경우 다음 오류 메시지가 생성 됩니다. **' CachingStrategy '에 대해 속성, 바인딩 가능한 속성 또는 이벤트를 찾을 수 없습니다.**

이 문제를 해결 하려면는 하위 클래스에서 생성자를 지정 하는 것 [ `ListView` ](xref:Xamarin.Forms.ListView) 받아들이는 [ `ListViewCachingStrategy` ](xref:Xamarin.Forms.ListViewCachingStrategy) 매개 변수를 기본 클래스로 전달:

```csharp
public class CustomListView : ListView
{
  public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
  {
  }
  ...
}
```

그런 다음 [ `ListViewCachingStrategy` ](xref:Xamarin.Forms.ListViewCachingStrategy) 사용 하 여 XAML에서 열거형 값을 지정할 수 있습니다는 `x:Arguments` 구문:

```xaml
<local:CustomListView>
  <x:Arguments>
    <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
  </x:Arguments>
</local:CustomListView>
```

<a name="improving-performance" />

## <a name="improving-listview-performance"></a>ListView 성능 향상

성능 향상을 위한 많은 기술이 있습니다를 `ListView`:

-  바인딩 합니다 `ItemsSource` 속성을는 `IList<T>` 대신 컬렉션을 `IEnumerable<T>` 컬렉션 때문에 `IEnumerable<T>` 컬렉션 임의 액세스를 지원 하지 않습니다.
-  기본 제공 셀을 사용 하 여 (같은 `TextCell`  /  `SwitchCell` ) 대신 `ViewCell` 수 있을 때마다 합니다.
-  더 적은 수의 요소를 사용 합니다. 예를 들어 단일을 사용 하는 것이 좋습니다 `FormattedString` 레이블 대신 여러 개의 레이블이 있습니다.
-  대체는 `ListView` 사용 하 여를 `TableView` 형식이 다른 데이터 – 즉, 서로 다른 유형의 데이터를 표시할 때.
-  사용을 제한 합니다 [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize) 메서드. 를 지나치게 사용이 성능이 저하 됩니다.
-  Android에서 설정 하지 마십시오는 `ListView`의 성능이 크게 저하 되므로 인스턴스화된 후 색 또는 행 구분 기호 표시 합니다.
-  기반으로 셀 레이아웃을 변경 하지 않도록 합니다 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)합니다. 이 비용이 큰 레이아웃 및 초기화 합니다.
-  많이 중첩 된 레이아웃 계층을 방지 합니다. 사용 하 여 `AbsoluteLayout` 또는 `Grid` 중첩을 줄일 수 있습니다.
-  특정 방지 `LayoutOptions` 이외의 `Fill` (채우기 계산 하는 중 가장 저렴 한입니다).
-  숨겨질 수를 `ListView` 안에 `ScrollView` 다음과 같은 이유로:
    - `ListView` 자체 스크롤을 구현 합니다.
    - 합니다 `ListView` 부모에 의해 처리 됩니다 하는 대로 모든 제스처를 받지 `ScrollView`합니다.
    - 합니다 `ListView` 잠재적으로 기능을 하는 제품 목록의 요소를 사용 하 여 사용자 지정된 헤더 및 스크롤 하는 바닥글을 제공할 수는 `ScrollView` 사용 되었습니다. 자세한 내용은 참조 [머리글 및 바닥글](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#Headers_and_Footers)합니다.
-  셀에 표시 되는 매우 특정 한 복잡 한 디자인 해야 하는 경우 사용자 지정 렌더러를 고려 합니다.

`AbsoluteLayout` 단일 측정값 호출 하지 않고 레이아웃 하는 데 있습니다. 이렇게 하면 성능에 대 한 매우 강력 합니다. 하는 경우 `AbsoluteLayout` 일 수 없습니다를 사용 하는 것이 좋습니다 [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout)합니다. 사용 하는 경우 `RelativeLayout`, 식 API를 사용 하 여 보다 크게 향상 되는 제약 조건에 직접 전달 합니다. 이것은 JIT에 사용 되는 식 API 하며 iOS에서 트리를 해석할 수는 느립니다. API 식에 적합 한 페이지 레이아웃 위치만 필요할 초기 레이아웃과 회전에서 `ListView`스크롤을 하는 동안 지속적으로 실행 되는 경우, 성능 문제가 발생 합니다.

빌드에 대 한 사용자 지정 렌더러를 [ `ListView` ](xref:Xamarin.Forms.ListView) 해당 셀의 스크롤 성능에 대 한 레이아웃 계산이 영향을 줄이는 한 가지 방법은 또는 합니다. 자세한 내용은 [는 ListView의 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) 하 고 [Viewcell 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)합니다.


## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러 보기 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)
- [사용자 지정 렌더러 ViewCell (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-viewcell)
- [ListViewCachingStrategy](xref:Xamarin.Forms.ListViewCachingStrategy)
