---
title: ''
description: ListView는 데이터를 표시 하기 위한 강력한 뷰입니다. 하지만 몇 가지 제한 사항이 있습니다. 이 문서 Xamarin.Forms 에서는 응용 프로그램에서 ListView로 뛰어난 성능을 유지 하는 방법을 설명 합니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e2b8e057d9687cd0a472451fc73cc578f9358277
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139895"
---
# <a name="listview-performance"></a>ListView 성능

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)

모바일 응용 프로그램을 작성 하는 경우 성능이 중요 합니다. 사용자에 게는 부드러운 스크롤 및 빠른 로드 시간이 필요 합니다. 사용자의 기대를 충족 시 키 지 못하면 응용 프로그램 스토어에서 등급이 청구 되거나 기간 업무 응용 프로그램의 경우 조직의 시간과 비용을 절감할 수 있습니다.

는 Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) 데이터를 표시 하기 위한 강력한 뷰입니다. 하지만 몇 가지 제한 사항이 있습니다. 사용자 지정 셀을 사용 하는 경우, 특히 깊게 중첩 된 뷰 계층 구조를 포함 하거나 복잡 한 측정이 필요한 특정 레이아웃을 사용 하는 경우 스크롤 성능이 저하 될 수 있습니다. 다행히 성능 저하를 방지 하는 데 사용할 수 있는 기술이 있습니다.

## <a name="caching-strategy"></a>캐싱 전략

Listview는 화면에 적합 한 것 보다 훨씬 더 많은 데이터를 표시 하는 데 종종 사용 됩니다. 예를 들어 음악 앱에는 수천 개의 항목이 있는 노래 라이브러리가 있을 수 있습니다. 모든 항목에 대 한 항목을 만들면 귀중 한 메모리가 낭비 되 고 성능이 저하 됩니다. 계속 해 서 행을 만들고 소멸 시키려면 응용 프로그램에서 개체를 계속 인스턴스화하고 정리 해야 합니다 .이 경우에도 성능이 저하 됩니다.

메모리를 절약 하기 위해 각 플랫폼에 해당 하는 네이티브에는 [`ListView`](xref:Xamarin.Forms.ListView) 행 재사용을 위한 기본 제공 기능이 있습니다. 화면에 표시 되는 셀만 메모리에 로드 되 고 **내용이** 기존 셀에 로드 됩니다. 이 패턴은 응용 프로그램에서 수천 개의 개체를 인스턴스화하고 시간과 메모리를 절약 하는 것을 방지 합니다.

Xamarin.Forms[`ListView`](xref:Xamarin.Forms.ListView)다음 값을 포함 하는 열거형을 통해 셀 다시 사용을 허용 합니다 [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) .

```csharp
public enum ListViewCachingStrategy
{
    RetainElement,   // the default value
    RecycleElement,
    RecycleElementAndDataTemplate
}
```

> [!NOTE]
> UWP (유니버설 Windows 플랫폼)는 [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) 항상 캐싱을 사용 하 여 성능을 향상 시킬 수 있으므로 캐싱 전략을 무시 합니다. 따라서 기본적으로 [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) 캐싱 전략이 적용 되는 것 처럼 동작 합니다.

### <a name="retainelement"></a>RetainElement

[`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement)캐싱 전략은가 [`ListView`](xref:Xamarin.Forms.ListView) 목록의 각 항목에 대 한 셀을 생성 하 고 기본 동작을 지정 하도록 지정 합니다 `ListView` . 이는 다음과 같은 경우에 사용 해야 합니다.

- 각 셀에는 많은 수의 바인딩 (20-30 +)이 있습니다.
- 셀 템플릿이 자주 변경 됩니다.
- 테스트를 수행 하면 `RecycleElement` 캐싱 전략으로 인해 실행 속도가 감소 합니다.

[`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement)사용자 지정 셀을 사용할 때 캐싱 전략의 결과를 이해 하는 것이 중요 합니다. 셀을 만들 때마다 셀 초기화 코드를 실행 해야 합니다 .이 코드는 초당 여러 번 사용할 수 있습니다. 이 경우 여러 중첩 된 인스턴스를 사용 하는 것과 같이 페이지에서 잘 모르는 레이아웃 기술은 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 사용자가 스크롤할 때 실시간으로 설정 및 소멸 될 때 성능 병목 상태가 됩니다.

### <a name="recycleelement"></a>RecycleElement

[`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)캐싱 전략은에서 [`ListView`](xref:Xamarin.Forms.ListView) 목록 셀을 재생 하 여 메모리 사용 공간과 실행 속도를 최소화 하도록 지정 합니다. 이 모드는 항상 성능 개선을 제공 하지 않으며, 향상 된 기능을 확인 하기 위해 테스트를 수행 해야 합니다. 그러나 다음 상황에서 사용 해야 하는 것이 좋습니다.

- 각 셀에는 적은 수의 바인딩 수가 있습니다.
- 각 셀 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 은 셀 데이터를 모두 정의 합니다.
- 셀 템플릿이 변경 되지 않은 상태에서 각 셀은 거의 비슷합니다.

가상화를 수행 하는 동안 셀은 바인딩 컨텍스트를 업데이트 하므로 응용 프로그램에서이 모드를 사용 하는 경우 바인딩 컨텍스트 업데이트를 적절 하 게 처리 해야 합니다. 셀에 대 한 모든 데이터는 바인딩 컨텍스트에서 가져와야 합니다. 그렇지 않으면 일관성 오류가 발생할 수 있습니다. 데이터 바인딩을 사용 하 여 셀 데이터를 표시 하면이 문제를 방지할 수 있습니다. 또는 `OnBindingContextChanged` 다음 코드 예제와 같이 사용자 지정 셀의 생성자가 아니라 재정의에서 셀 데이터를 설정 해야 합니다.

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

자세한 내용은 [바인딩 컨텍스트 변경 내용](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#binding-context-changes)을 참조 하세요.

IOS 및 Android에서 셀에 사용자 지정 렌더러를 사용 하는 경우 속성 변경 알림이 올바르게 구현 되었는지 확인 해야 합니다. 셀을 다시 사용 하는 경우 바인딩 컨텍스트가 사용 가능한 셀의 값으로 업데이트 되 면 이벤트가 발생 하 여 해당 속성 값이 변경 됩니다 `PropertyChanged` . 자세한 내용은 [ViewCell 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)을 참조 하세요.

#### <a name="recycleelement-with-a-datatemplateselector"></a>RecycleElement DataTemplateSelector

에서를 [`ListView`](xref:Xamarin.Forms.ListView) 사용 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) 하 여를 선택 하는 경우 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) 캐싱 전략은 s를 캐시 하지 않습니다 `DataTemplate` . 대신 `DataTemplate` 목록의 각 데이터 항목에 대해가 선택 됩니다.

> [!NOTE]
> [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)캐싱 전략에는 2.4에 도입 된 필수 구성 요소가 있으며 Xamarin.Forms ,이를 통해 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 각각 `DataTemplate` 동일한 형식을 반환 해야 하는을 선택 하 라는 메시지가 표시 됩니다 [`ViewCell`](xref:Xamarin.Forms.ViewCell) . 예를 들어, [`ListView`](xref:Xamarin.Forms.ListView) `DataTemplateSelector` `MyDataTemplateA` ( `MyDataTemplateA` `ViewCell` 가 형식의을 반환 `MyViewCellA` 함) 또는 (가 형식의을 반환 함)을 반환할 수 있는가 포함 된 경우 `MyDataTemplateB` `MyDataTemplateB` 이 반환 되 면이 반환 되 고 `ViewCell` `MyViewCellB` , `MyDataTemplateA` 그렇지 않으면 `MyViewCellA` 예외가 throw 됩니다.

### <a name="recycleelementanddatatemplate"></a>RecycleElementAndDataTemplate

캐싱 전략은에서를 사용 하 여를 [`RecycleElementAndDataTemplate`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) [`ListView`](xref:Xamarin.Forms.ListView) [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) 선택할 때 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) `DataTemplate` 목록에 있는 항목의 형식에 따라가 캐시 되도록 하 여 캐싱 전략을 기반으로 합니다. 따라서 `DataTemplate` s는 항목 인스턴스당 한 번이 아니라 항목 유형별으로 한 번 선택 됩니다.

> [!NOTE]
> [`RecycleElementAndDataTemplate`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate)캐싱 전략에는에서 반환 하는가 `DataTemplate` [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type)) 를 사용 하는 생성자를 사용 해야 하는 필수 구성 요소가 있습니다 `Type` .

### <a name="set-the-caching-strategy"></a>캐싱 전략 설정

[`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy)열거형 값은 [`ListView`](xref:Xamarin.Forms.ListView) 다음 코드 예제와 같이 생성자 오버 로드를 사용 하 여 지정 됩니다.

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

XAML에서 `CachingStrategy` 아래 xaml에 표시 된 것 처럼 특성을 설정 합니다.

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

이 메서드는 c #의 생성자에서 캐싱 전략 인수를 설정 하는 것과 동일한 효과를 가집니다.

#### <a name="set-the-caching-strategy-in-a-subclassed-listview"></a>서브클래싱된 ListView에서 캐싱 전략 설정

`CachingStrategy`에 속성이 없기 때문에 서브클래싱된의 XAML에서 특성을 설정 [`ListView`](xref:Xamarin.Forms.ListView) 하면 원하는 동작이 생성 되지 않습니다 `CachingStrategy` `ListView` . 또한 [XAMLC](~/xamarin-forms/xaml/xamlc.md) 를 사용 하는 경우 **' cachingstrategy '에 대해 속성, 바인딩 가능 속성 또는 이벤트를 찾을 수 없음** 오류 메시지가 생성 됩니다.

이 문제에 대 한 해결 방법은 [`ListView`](xref:Xamarin.Forms.ListView) 매개 변수를 허용 하 [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) 고 기본 클래스에 전달 하는 서브클래싱에서 생성자를 지정 하는 것입니다.

```csharp
public class CustomListView : ListView
{
    public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
    {
    }
    ...
}
```

그런 다음 [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) 구문을 사용 하 여 XAML에서 열거형 값을 지정할 수 있습니다 `x:Arguments` .

```xaml
<local:CustomListView>
    <x:Arguments>
        <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
    </x:Arguments>
</local:CustomListView>
```

## <a name="listview-performance-suggestions"></a>ListView 성능 제안

의 성능을 향상 시키기 위한 많은 기술이 있습니다 `ListView` . 다음 제안으로 ListView의 성능을 향상 시킬 수 있습니다.

- 컬렉션은 `ItemsSource` `IList<T>` `IEnumerable<T>` 임의 액세스를 지원 하지 않으므로 컬렉션 대신 컬렉션에 속성을 바인딩합니다 `IEnumerable<T>` .
- 가능 하면가 아닌 기본 제공 셀 (예:)을 사용 `TextCell`  /  `SwitchCell` `ViewCell` 합니다.
- 보다 작은 수의 요소를 사용 합니다. 예를 들어 여러 레이블 대신 단일 레이블을 사용 하는 것이 좋습니다 `FormattedString` .
- 동일 `ListView` `TableView` 하지 않은 데이터, 즉 다른 형식의 데이터를 표시할 때를로 바꿉니다.
- 메서드 사용을 제한 [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) 합니다. 남용 하면 성능이 저하 됩니다.
- Android에서는 `ListView` 성능 저하가 발생 하므로의 행 구분 기호 표시 또는 색을 인스턴스화한 후에 설정 하지 마십시오.
- 에 따라 셀 레이아웃을 변경 하지 마십시오 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) . 레이아웃을 변경 하면 많은 측정 및 초기화 비용이 발생 합니다.
- 깊게 중첩 된 레이아웃 계층을 사용 하지 않습니다. `AbsoluteLayout` `Grid` 중첩을 줄이기 위해 또는를 사용 합니다.
- 이외의 특정을 사용 하지 마십시오 `LayoutOptions` `Fill` `Fill` . (는 계산에 가장 저렴).
- 다음과 같은 이유로에를에 배치 하지 마십시오 `ListView` `ScrollView` .
  - 는 `ListView` 자체 스크롤을 구현 합니다.
  - 는 `ListView` 부모에 의해 처리 되므로 제스처를 수신 하지 않습니다 `ScrollView` .
  - 는 `ListView` 목록 요소로 스크롤되는 사용자 지정 된 머리글 및 바닥글을 제공할 수 있으며,가 사용 된 기능을 제공 합니다 `ScrollView` . 자세한 내용은 [머리글 및 바닥글](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#headers-and-footers)을 참조 하세요.
- 셀에 특정 하 고 복잡 한 디자인이 표시 되어야 하는 경우 사용자 지정 렌더러를 고려 합니다.

`AbsoluteLayout`에서는 단일 측정 호출 없이 레이아웃을 수행 하 여 성능을 크게 높일 수 있습니다. `AbsoluteLayout`을 사용할 수 없는 경우를 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) 고려 합니다. 를 사용 하는 경우 `RelativeLayout` 식 API를 사용 하는 것 보다 직접 제약 조건을 전달 하는 것이 훨씬 빠릅니다. 이 메서드는 식 API가 JIT를 사용 하 고 iOS에서는 더 느린 트리를 해석 해야 하기 때문에 더 빠릅니다. 식 API는 초기 레이아웃 및 회전 에서만 필요 하지만 스크롤 중에 지속적으로 실행 되는 페이지 레이아웃에 적합 `ListView` 합니다.

또는 해당 셀에 대 한 사용자 지정 렌더러를 작성 [`ListView`](xref:Xamarin.Forms.ListView) 하는 방법은 스크롤 성능에 대 한 레이아웃 계산의 효과를 줄이는 한 가지 방법입니다. 자세한 내용은 [ListView 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) 및 [viewcell 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러 뷰 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)
- [사용자 지정 렌더러 뷰 셀 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-viewcell)
- [ListViewCachingStrategy](xref:Xamarin.Forms.ListViewCachingStrategy)
