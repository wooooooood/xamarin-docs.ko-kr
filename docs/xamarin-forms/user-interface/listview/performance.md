---
title: ListView 성능
description: ListView에 데이터를 표시 하기 위한 강력한 보기 되어도 몇 가지 제한이 있습니다. 이 문서에서는 응용 프로그램에서 Xamarin.Forms ListView로 뛰어난 성능을 보장 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 1B085639-652C-4862-86EB-5D55D32B9395
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2017
ms.openlocfilehash: 4803a612e2b06e458f2859dbbbd30b970f0fc8ea
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244905"
---
# <a name="listview-performance"></a>ListView 성능

모바일 응용 프로그램을 작성할 때 성능에 중요 합니다. 부드러운 스크롤 및 빠른 로드 시간을 기대 하는 사용자가 발견 했을 합니다. 사용자의 요구에 맞게 실패 하면 응용 프로그램 저장소에 대 한 등급 비용 되거나는 기간 업무 응용 프로그램의 경우 조직 시간과 비용 비용 됩니다.

하지만 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 강력한 보기는 몇 가지 제한이 있기 때문에 데이터를 표시 합니다. 많이 중첩 된 보기 계층 구조를 포함 하거나 측정 많이 필요로 하는 특정 레이아웃을 사용 하는 경우에 특히 사용자 지정 셀을 사용 하는 경우 스크롤 성능 저하 될 수 있습니다. 다행히도 기술을 성능 저하를 방지 하는 데 사용할 수 있습니다.

<a name="cachingstrategy" />

## <a name="caching-strategy"></a>캐싱 전략

Listview는 주로 것 보다 훨씬 더 많은 데이터를 표시 하는 데 사용 화면 적합 합니다. 예를 들어 음악 앱을 고려 합니다. 노래 라이브러리에는 수천 개의 항목이 있을 수 있습니다. 간단한 방법에서는 모든 노래에 대 한 행을 만드는 것에 성능이 저하 될 것입니다. 이 방법 귀중 한 메모리가 낭비 및 탐색을 찾아 스크롤하기가 저하 될 수 있습니다. 다른 방법은 만들고 데이터 스크롤하여 볼 때 행을 삭제 하는 것입니다. 이렇게 하려면 상수 인스턴스화 및 매우 느려질 수 있는 뷰 개체의 정리 합니다.

네이티브 메모리를 절약 하기 위해 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 해당 하는 각 플랫폼에 대 한 기본 제공 기능이 다시 행을 사용 합니다. 화면에 표시 되는 셀은 메모리에 로드 되 고 **콘텐츠** 기존 셀에 로드 됩니다. 따라서 응용 프로그램에서 수천 개의 시간과 메모리 개체를 인스턴스화할 필요가 없습니다.

Xamarin.Forms 허용 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 셀을 통해 다시 사용는 [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) 열거형 값은 다음과 같습니다.

```csharp
public enum ListViewCachingStrategy
{
  RetainElement,   // the default value
  RecycleElement,
  RecycleElementAndDataTemplate
}
```

> [!NOTE]
> 유니버설 Windows 플랫폼 (UWP) 무시는 [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) 캐싱 전략을 사용 하기 때문에 항상 캐싱 성능 향상을 위해 합니다. 기본적으로 동작 하기 처럼는 [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) 캐싱 전략 적용 됩니다.

### <a name="retainelement"></a>RetainElement

[ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) 지정 캐싱 전략은 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 이며 기본적으로 목록에서 각 항목에 대 한 셀을 생성 합니다 `ListView` 동작 합니다. 다음과 같은 경우에 일반적으로 사용 해야 합니다.

- 각 셀에 많은 바인딩의 경우 (20-30 +).
- 때 셀 템플릿이 자주 변경 됩니다.
- 때 테스트 함을 의미할는 `RecycleElement` 감소 실행 속도가 전략 결과 캐시 합니다.

작업의 결과 인식 해야는 [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) 캐싱 전략 셀 사용자 지정 작업을 합니다. 모든 셀 초기화 코드가 각 셀 만들기 위해 실행 해야 1 초에 여러 번 사용할 수 있습니다. 이 경우에는 페이지에 제대로 된 레이아웃 기술을 다중 중첩을 사용 하 여 같은 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 인스턴스를 설정 하는 경우 성능 병목 현상이 되 고 사용자 스크롤으로 실시간으로 소멸 합니다.

<a name="recycleelement" />

### <a name="recycleelement"></a>RecycleElement

[ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) 지정 캐싱 전략은 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 목록 셀을 재활용 하 여 해당 메모리 사용 공간 및 실행 속도 최소화 하려고 합니다. 이 모드는 항상 향상 된 성능을 제공 하지와 테스트 확인 기능을 개선 하려면 수행 해야 합니다. 그러나는 일반적으로 가장 적합 하 고 다음과 같은 경우에 사용 해야 합니다.

- 각 셀에 바인딩의 적당 한 수에는 작은 경우.
- 때 각 셀의 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 모든 셀 데이터를 정의 합니다.
- 때 각 셀은 변하지 않는 셀 템플릿으로 비슷하긴 하지만입니다.

가상화 하는 동안 셀 업데이트 하 고 바인딩 컨텍스트에 하므로 응용 프로그램에서이 모드를 사용 하는 경우 동작이 보장 되어야 바인딩 컨텍스트 업데이트 적절 하 게 처리 됩니다. 셀에 대 한 모든 데이터 바인딩 컨텍스트를 제공 해야 하거나 일관성 오류가 발생할 수 있습니다. 셀 데이터를 표시 하려면 데이터 바인딩을 사용 하 여이 수행할 수 있습니다. 셀 데이터에 설정 해야 또는 `OnBindingContextChanged` 재정의 대신 다음 코드 예제에서와 같이 사용자 지정 셀의 생성자에서:

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

자세한 내용은 참조 [바인딩 컨텍스트 변경](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#binding-context-changes)합니다.

IOS 및 Android에서 셀 사용자 지정 렌더러를 사용 하는 경우 해야 확인 속성 변경 알림을 잘못 구현 합니다. 바인딩 컨텍스트를 사용할 수 있는 셀의 업데이트 된 경우 해당 속성 값이 변경 됩니다 셀이 다시 사용 하는 경우 `PropertyChanged` 발생 하는 이벤트입니다. 자세한 내용은 참조 [는 ViewCell 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)합니다.

#### <a name="recycleelement-with-a-datatemplateselector"></a>DataTemplateSelector와 RecycleElement

경우는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 사용 하 여 한 [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) 선택 하는 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) 캐싱 전략을 캐시 하지 않습니다 `DataTemplate`s입니다. 대신,는 `DataTemplate` 목록에는 데이터의 각 항목을 선택 합니다.

> [!NOTE]
> [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) 필수에 캐싱 전략 Xamarin.Forms 2.4에 도입 된 때를 [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) 선택 하는 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)각 `DataTemplate` 동일한 반환 해야 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) 유형입니다. 예를 들어 한 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 와 `DataTemplateSelector` 중 하나를 반환할 수 있는 `MyDataTemplateA` (여기서 `MyDataTemplateA` 반환는 `ViewCell` 형식의 `MyViewCellA`), 또는 `MyDataTemplateB` (여기서 `MyDataTemplateB`반환는 `ViewCell` 형식의 `MyViewCellB`), `MyDataTemplateA` 반환 해야이 반환 `MyViewCellA` 그렇지 않으면 예외가 throw 됩니다.

### <a name="recycleelementanddatatemplate"></a>RecycleElementAndDataTemplate

[ `RecycleElementAndDataTemplate` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate/) 기반 캐싱 전략의 [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) 는 또한 함으로써 캐싱 전략을 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 는 를사용하여[ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) 선택 하는 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), `DataTemplate`의형식 목록에서 항목에 따라 캐시 됩니다. 따라서 `DataTemplate`의항목 인스턴스당 한 번이 아닌 항목 형식 당 한 번만 선택 됩니다.

> [!NOTE]
> [ `RecycleElementAndDataTemplate` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate/) 필수에 캐싱 전략 하는 `DataTemplate`에서 반환 된 s는 [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) 사용 해야 합니다는 [ `DataTemplate` ](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/) 사용 하는 생성자는 `Type`합니다.

### <a name="setting-the-caching-strategy"></a>캐싱 전략을 설정합니다.

[ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) 열거형 값이 지정 된 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 생성자 오버 로드에서 다음 코드 예제에 나와 있는 것 처럼:

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

XAML에서 설정 된 `CachingStrategy` 아래 코드에 나와 있는 것 처럼 특성:

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

이 C#;의 생성자에는 캐싱 전략 인수를 설정 하는 것과 같습니다. 없는 `CachingStrategy` 속성 `ListView`합니다.

#### <a name="setting-the-caching-strategy-in-a-subclassed-listview"></a>서브클래싱된 ListView에서 캐싱 전략을 설정합니다.

설정의 `CachingStrategy` XAML에서 특성에는 서브클래싱된 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 있기 때문에 원하는 동작을 생성 하지 것입니다 없습니다 `CachingStrategy` 속성을 `ListView`합니다. 또한 경우 [XAMLC](~/xamarin-forms/xaml/xamlc.md) 가 사용 하도록 설정 하면 다음 오류 메시지가 생성 됩니다: **속성, 바인딩 가능 속성 또는 이벤트 'CachingStrategy'에 대 한 없습니다**

이 문제를 해결 하려면는 서브클래싱된에서 생성자를 지정 하는 것 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 수락 하는 [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) 매개 변수 하 고 기본 클래스를 전달 합니다.

```csharp
public class CustomListView : ListView
{
  public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
  {
  }
  ...
}
```

그런 다음 [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) 를 사용 하 여 XAML에서 열거형 값을 지정할 수 있습니다는 `x:Arguments` 구문:

```xaml
<local:CustomListView>
  <x:Arguments>
    <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
  </x:Arguments>
</local:CustomListView>
```

<a name="improving-performance" />

## <a name="improving-listview-performance"></a>ListView 성능 향상

성능 향상을 위한 여러 가지 방법이 `ListView`:

-  바인딩는 `ItemsSource` 속성을는 `IList<T>` 컬렉션 대신 한 `IEnumerable<T>` 컬렉션 때문에 `IEnumerable<T>` 컬렉션 임의 액세스를 지원 하지 않습니다.
-  기본 제공 된 셀을 사용 하 여 (같은 `TextCell`  /  `SwitchCell` ) 대신 `ViewCell` 때마다 수 있습니다.
-  더 적은 수의 요소를 사용 합니다. 예를 들어 단일을 사용 하는 것이 좋습니다 `FormattedString` 레이블 대신 레이블이 여러 개 있습니다.
-  대체는 `ListView` 와 `TableView` 형식이 다른 데이터-즉, 서로 다른 유형의 데이터를 표시 합니다.
-  사용을 제한 된 [ `Cell.ForceUpdateSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.ForceUpdateSize()/) 메서드. 게 사용 되어,이 성능이 저하 됩니다.
-  Android에서는 설정 하지는 `ListView`의 행 구분 기호 표시 유형 또는 큰 성능 저하 되므로 시작 된 후 색입니다.
-  기반으로 셀 레이아웃을 변경 하지 마십시오는 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)합니다. 이 경우 큰 레이아웃 및 초기화 비용이 있습니다.
-  많이 중첩 된 레이아웃 계층을 하지 마십시오. 사용 하 여 `AbsoluteLayout` 또는 `Grid` 중첩을 줄일 수 있습니다.
-  특정 방지 `LayoutOptions` 이외의 `Fill` (채우기는 싼 계산).
-  설치 하지 않습니다는 `ListView` 내는 `ScrollView` 이유는 다음과 같습니다.
  - `ListView` 자체 스크롤을 구현 합니다.
  - `ListView` 부모에서 처리 되는 대로 모든 제스처를 받지 못합니다 `ScrollView`합니다.
  - `ListView` 잠재적으로 제공 하는 기능 목록에서의 요소와 사용자 지정 된 헤더 및 스크롤 하는 바닥글을 제공할 수는 `ScrollView` 에 사용 되었습니다. 자세한 내용은 참조 [머리글 및 바닥글](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#Headers_and_Footers)합니다.
-  해당 셀에는 매우 구체적인, 복잡 한 디자인 해야 할 경우 사용자 지정 렌더러를 고려 합니다.

`AbsoluteLayout` 하나의 측정값을 호출 하지 않고 레이아웃 수행할 가능성이 있습니다. 이렇게 하면 성능에 대 한 매우 강력 합니다. 경우 `AbsoluteLayout` 안을 사용 하는 것이 좋습니다 [ `RelativeLayout` ](http://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/)합니다. 사용 하는 경우 `RelativeLayout`, 제약 조건에 직접 전달 API 식을 사용 하 여 보다 속도가 훨씬 빠를 수 있습니다. API JIT을 사용 하 고 iOS에서 트리 해석할 수 있는 느립니다 때문입니다. 여기서 하기만 하면 초기 레이아웃 및 회전에 식 API가 페이지 레이아웃에 적합 `ListView`, 스크롤 하는 동안 지속적으로 실행 위치, 성능 확인해 합니다.

빌드에 대 한 사용자 지정 렌더러는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 셀은 레이아웃 계산 스크롤 성능에 미치는 영향을 줄이기를 하나의 방법 또는 합니다. 자세한 내용은 참조 [ListView를 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) 및 [는 ViewCell 사용자 지정](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)합니다.


## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러 보기 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [사용자 지정 렌더러 ViewCell (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
- [ListViewCachingStrategy](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/)
