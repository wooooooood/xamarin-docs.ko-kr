---
title: ListView 성능
description: ListView 데이터를 표시 하기 위한 강력한 뷰일 경우에 몇 가지 제한 사항이 있습니다. 이 문서에서는 응용 프로그램에서 Xamarin.Forms ListView를 사용 하 여 뛰어난 성능을 보장 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 1B085639-652C-4862-86EB-5D55D32B9395
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2017
ms.openlocfilehash: ed920db129d2af203046a648c0069580f99a295e
ms.sourcegitcommit: a5ef4497db04dfa016865bc7454b3de6ff088554
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/15/2019
ms.locfileid: "70998174"
---
# <a name="listview-performance"></a>ListView 성능

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)

모바일 응용 프로그램을 작성할 때 성능에 중요 합니다. 사용자가 제대로 찾아 오셨습니다 부드러운 스크롤 및 빠른 로드 시간을 예상 합니다. 사용자의 기대를 충족 하기 위해 실패 한 응용 프로그램 저장소에 대 한 등급 비용이 되거나를 기간 업무 응용 프로그램의 경우 조직으로 시간과 비용을 비용 됩니다.

Xamarin.ios [`ListView`](xref:Xamarin.Forms.ListView) 는 데이터를 표시 하기 위한 강력한 뷰입니다. 하지만 몇 가지 제한 사항이 있습니다. 사용자 지정 셀을 사용 하는 경우, 특히 깊게 중첩 된 뷰 계층 구조를 포함 하거나 복잡 한 측정이 필요한 특정 레이아웃을 사용 하는 경우 스크롤 성능이 저하 될 수 있습니다. 다행 스럽게도 가지 방법이 성능 저하를 방지 하는 데 사용할 수 있습니다.

## <a name="caching-strategy"></a>캐싱 전략

Listview는 화면에 적합 한 것 보다 훨씬 더 많은 데이터를 표시 하는 데 종종 사용 됩니다. 예를 들어 음악 앱에는 수천 개의 항목이 있는 노래 라이브러리가 있을 수 있습니다. 모든 항목에 대 한 항목을 만들면 귀중 한 메모리가 낭비 되 고 성능이 저하 됩니다. 계속 해 서 행을 만들고 소멸 시키려면 응용 프로그램에서 개체를 계속 인스턴스화하고 정리 해야 합니다 .이 경우에도 성능이 저하 됩니다.

메모리를 절약 하기 위해 각 [`ListView`](xref:Xamarin.Forms.ListView) 플랫폼에 해당 하는 네이티브에는 행 재사용을 위한 기본 제공 기능이 있습니다. 화면에 보이는 셀만 메모리에 로드 되 고 **콘텐츠** 기존 셀에 로드 됩니다. 이 패턴은 응용 프로그램에서 수천 개의 개체를 인스턴스화하고 시간과 메모리를 절약 하는 것을 방지 합니다.

Xamarin.ios는 다음 값 [`ListView`](xref:Xamarin.Forms.ListView) 을 가진 [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) 열거형을 통해 셀을 다시 사용할 수 있도록 합니다.

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

[ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) 지정 캐싱 전략을 [ `ListView` ](xref:Xamarin.Forms.ListView) 는 목록의 각 항목에 대 한 셀을 생성 하 고 기본값인 `ListView` 동작 합니다. 이는 다음과 같은 경우에 사용 해야 합니다.

- 각 셀에는 많은 수의 바인딩 (20-30 +)이 있습니다.
- 셀 템플릿이 자주 변경 됩니다.
- 테스트를 수행 하면 `RecycleElement` 캐싱 전략으로 인해 실행 속도가 감소 합니다.

작업의 결과 인식 해야 합니다 [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) 사용자 지정 셀을 사용 하 여 작업 하는 경우에 캐싱 전략입니다. 셀 초기화 코드는 각 셀 생성, 실행 해야는 초당 여러 번 사용할 수 있습니다. 이 경우 여러 중첩 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 된 인스턴스를 사용 하는 것과 같이 페이지에서 잘 모르는 레이아웃 기술은 사용자가 스크롤할 때 실시간으로 설정 및 소멸 될 때 성능 병목 상태가 됩니다.

### <a name="recycleelement"></a>RecycleElement

합니다 [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) 지정 캐싱 전략을 [ `ListView` ](xref:Xamarin.Forms.ListView) 목록 셀을 재활용 하 여 메모리 사용 공간 및 실행 속도 최소화 하려고 합니다. 이 모드는 항상 성능 개선을 제공 하지 않으며, 향상 된 기능을 확인 하기 위해 테스트를 수행 해야 합니다. 그러나 다음 상황에서 사용 해야 하는 것이 좋습니다.

- 각 셀에는 적은 수의 바인딩 수가 있습니다.
- 각 셀 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 은 셀 데이터를 모두 정의 합니다.
- 셀 템플릿이 변경 되지 않은 상태에서 각 셀은 거의 비슷합니다.

가상화 중 셀을 업데이트 하는 바인딩 컨텍스트에 없는 고 따라서 응용 프로그램에서이 모드를 사용 하는 경우 바인딩 컨텍스트 업데이트 적절 하 게 처리 됩니다. 셀에 대 한 모든 데이터 바인딩 컨텍스트를 제공 해야 합니다 또는 일관성 오류가 발생할 수 있습니다. 데이터 바인딩을 사용 하 여 셀 데이터를 표시 하면이 문제를 방지할 수 있습니다. 셀 데이터 설정할 또는 `OnBindingContextChanged` 재정의 대신 다음 코드 예제에서 설명한 것 처럼 사용자 지정 셀의 생성자에서:

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

### <a name="set-the-caching-strategy"></a>캐싱 전략 설정

합니다 [ `ListViewCachingStrategy` ](xref:Xamarin.Forms.ListViewCachingStrategy) 열거형 값이 지정 된 된 [ `ListView` ](xref:Xamarin.Forms.ListView) 생성자 오버 로드를 다음 코드 예제 에서처럼:

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

Xaml에서 아래 xaml에 `CachingStrategy` 표시 된 것 처럼 특성을 설정 합니다.

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

이 메서드는의 C#생성자에서 캐싱 전략 인수를 설정 하는 것과 동일한 효과를 가집니다.

#### <a name="set-the-caching-strategy-in-a-subclassed-listview"></a>서브클래싱된 ListView에서 캐싱 전략 설정

`CachingStrategy` [`ListView`](xref:Xamarin.Forms.ListView) 에 속성이없기`CachingStrategy` 때문에 서브클래싱된의 XAML에서 특성을 설정 하면 원하는 동작이 생성 되지 않습니다. `ListView` 또한 [XAMLC](~/xamarin-forms/xaml/xamlc.md) 를 사용 하는 경우 다음 오류 메시지가 생성 됩니다. **' CachingStrategy '에 대해 속성, 바인딩 가능한 속성 또는 이벤트를 찾을 수 없습니다.**

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

## <a name="listview-performance-suggestions"></a>ListView 성능 제안

의 성능을 향상 시키기 위한 많은 기술이 있습니다 `ListView`. 다음 제안으로 ListView의 성능을 향상 시킬 수 있습니다.

- 바인딩 합니다 `ItemsSource` 속성을는 `IList<T>` 대신 컬렉션을 `IEnumerable<T>` 컬렉션 때문에 `IEnumerable<T>` 컬렉션 임의 액세스를 지원 하지 않습니다.
- 가능 하면 `TextCell` 가 아닌 `SwitchCell`  /  기본`ViewCell` 제공 셀 (예:)을 사용 합니다.
- 더 적은 수의 요소를 사용 합니다. 예를 들어 여러 레이블 대신 단일 `FormattedString` 레이블을 사용 하는 것이 좋습니다.
- 대체는 `ListView` 사용 하 여를 `TableView` 형식이 다른 데이터 – 즉, 서로 다른 유형의 데이터를 표시할 때.
- 사용을 제한 합니다 [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize) 메서드. 를 지나치게 사용이 성능이 저하 됩니다.
- Android에서 설정 하지 마십시오는 `ListView`의 성능이 크게 저하 되므로 인스턴스화된 후 색 또는 행 구분 기호 표시 합니다.
- 기반으로 셀 레이아웃을 변경 하지 않도록 합니다 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)합니다. 레이아웃을 변경 하면 많은 측정 및 초기화 비용이 발생 합니다.
- 많이 중첩 된 레이아웃 계층을 방지 합니다. 사용 하 여 `AbsoluteLayout` 또는 `Grid` 중첩을 줄일 수 있습니다.
- `Fill` 이외의 `LayoutOptions` 특정을사용하지마십시오.(는계산에가장저렴).`Fill`
- 숨겨질 수를 `ListView` 안에 `ScrollView` 다음과 같은 이유로:
  - `ListView` 자체 스크롤을 구현 합니다.
  - 합니다 `ListView` 부모에 의해 처리 됩니다 하는 대로 모든 제스처를 받지 `ScrollView`합니다.
  - 합니다 `ListView` 잠재적으로 기능을 하는 제품 목록의 요소를 사용 하 여 사용자 지정된 헤더 및 스크롤 하는 바닥글을 제공할 수는 `ScrollView` 사용 되었습니다. 자세한 내용은 [머리글 및 바닥글](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#headers-and-footers)을 참조 하세요.
- 셀에 특정 하 고 복잡 한 디자인이 표시 되어야 하는 경우 사용자 지정 렌더러를 고려 합니다.

`AbsoluteLayout`에서는 단일 측정 호출 없이 레이아웃을 수행 하 여 성능을 크게 높일 수 있습니다. 하는 경우 `AbsoluteLayout` 일 수 없습니다를 사용 하는 것이 좋습니다 [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout)합니다. 사용 하는 경우 `RelativeLayout`, 식 API를 사용 하 여 보다 크게 향상 되는 제약 조건에 직접 전달 합니다. 이 메서드는 식 API가 JIT를 사용 하 고 iOS에서는 더 느린 트리를 해석 해야 하기 때문에 더 빠릅니다. API 식에 적합 한 페이지 레이아웃 위치만 필요할 초기 레이아웃과 회전에서 `ListView`스크롤을 하는 동안 지속적으로 실행 되는 경우, 성능 문제가 발생 합니다.

빌드에 대 한 사용자 지정 렌더러를 [ `ListView` ](xref:Xamarin.Forms.ListView) 해당 셀의 스크롤 성능에 대 한 레이아웃 계산이 영향을 줄이는 한 가지 방법은 또는 합니다. 자세한 내용은 [는 ListView의 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) 하 고 [Viewcell 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)합니다.

## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러 보기 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)
- [사용자 지정 렌더러 ViewCell (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-viewcell)
- [ListViewCachingStrategy](xref:Xamarin.Forms.ListViewCachingStrategy)
