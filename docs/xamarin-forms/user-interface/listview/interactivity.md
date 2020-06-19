---
title: ListView 대화형 작업
description: 이 문서에서는 Xamarin.Forms 선택 항목, 컨텍스트 작업 및 끌어오기-새로 고침을 구현 하 여 ListView에 대화형 작업을 추가 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/25/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5142965216b328172ae7fa04cdc0c13590f5ff38
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139890"
---
# <a name="listview-interactivity"></a>ListView 대화형 작업

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)

Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) 클래스는 표시 되는 데이터와의 사용자 상호 작용을 지원 합니다.

## <a name="selection-and-taps"></a>선택 및 탭

[`ListView`](xref:Xamarin.Forms.ListView)선택 모드는 [`ListView.SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) 속성을 열거형의 값으로 설정 하 여 제어 됩니다 [`ListViewSelectionMode`](xref:Xamarin.Forms.ListViewSelectionMode) .

- [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single)선택한 항목이 강조 표시 된 상태로 단일 항목을 선택할 수 있음을 나타냅니다. 이것은 기본값입니다.
- [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None)항목을 선택할 수 없음을 나타냅니다.

사용자가 항목을 탭 하면 다음과 같은 두 개의 이벤트가 발생 합니다.

- [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)새 항목이 선택 될 때 발생 합니다.
- [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped)항목을 탭 할 때 발생 합니다.

동일한 항목을 두 번 누르면 두 개의 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트가 발생 하지만 단일 이벤트만 발생 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 합니다.

> [!NOTE]
> [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs)이벤트에 대 한 이벤트 인수를 포함 하는 클래스에는 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) [`Group`](xref:Xamarin.Forms.ItemTappedEventArgs.Group) 및 [`Item`](xref:Xamarin.Forms.ItemTappedEventArgs.Item) 속성이 있고 `ItemIndex` [`ListView`](xref:Xamarin.Forms.ListView) 탭 항목의에서 인덱스를 나타내는 값을 갖는 속성이 있습니다. 마찬가지로, [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs) 이벤트에 대 한 이벤트 인수를 포함 하는 클래스에는 속성이 있으며, [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) [`SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) `SelectedItemIndex` 해당 값이 선택 된 항목의에 있는 인덱스를 나타내는 속성이 있습니다 `ListView` .

속성이로 설정 된 경우의 항목을 선택할 수 있으며, [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) [`ListView`](xref:Xamarin.Forms.ListView) [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 및 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트가 발생 하 고 [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) 속성이 선택 된 항목의 값으로 설정 됩니다.

속성이로 설정 된 경우의 항목을 선택할 수 없으며,이 [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) 이벤트는 [`ListView`](xref:Xamarin.Forms.ListView) [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 발생 하지 않으며 [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) 속성은 그대로 유지 됩니다 `null` . 그러나 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트는 계속 발생 하 고 탭 하는 동안 탭 항목이 잠깐 강조 표시 됩니다.

항목을 선택 하 고 [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) 속성을에서로 변경 하면 [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) 속성이로 설정 되 `null` 고 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트가 항목을 사용 하 여 발생 합니다 `null` .

다음 스크린샷에는 [`ListView`](xref:Xamarin.Forms.ListView) 기본 선택 모드가 포함 된가 나와 있습니다.

![](interactivity-images/selection-default.png "ListView with Selection Enabled")

### <a name="disable-selection"></a>선택 사용 안 함

선택을 해제 하려면 [`ListView`](xref:Xamarin.Forms.ListView) 속성을 [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) 로 설정 합니다 [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) .

```xaml
<ListView ... SelectionMode="None" />
```

```csharp
var listView = new ListView { ... SelectionMode = ListViewSelectionMode.None };
```

## <a name="context-actions"></a>컨텍스트 작업

사용자가의 항목에 대 한 작업을 수행 하려고 하는 경우가 종종 `ListView` 있습니다. 예를 들어 메일 앱의 전자 메일 목록을 살펴보겠습니다. IOS에서 살짝 밀어 메시지를 삭제할 수 있습니다.

![](interactivity-images/context-default.png "ListView with Context Actions")

컨텍스트 작업은 c # 및 XAML로 구현할 수 있습니다. 아래에는 두 가지에 대 한 특정 가이드가 있지만 먼저 두 가지 주요 구현에 대 한 세부 정보를 살펴보겠습니다.

요소를 사용 하 여 컨텍스트 작업을 만듭니다 `MenuItem` . 개체에 대 한 탭 이벤트는가 `MenuItems` 아닌 자체에서 발생 합니다 `MenuItem` `ListView` . 이는 셀에 대해 탭 이벤트를 처리 하는 방법과 다릅니다. 여기서는 `ListView` 셀이 아닌 이벤트를 발생 시킵니다. 에서 `ListView` 이벤트를 발생 시키기 때문에 해당 이벤트 처리기에는 선택 하거나 탭 하는 항목과 같은 키 정보가 제공 됩니다.

기본적으로에는 `MenuItem` 자신이 속한 셀을 알 수 있는 방법이 없습니다. 속성은의 `CommandParameter` `MenuItem` 개체와 같은 개체를 저장 하기 위해에서 사용할 수 있습니다 `MenuItem` `ViewCell` . `CommandParameter`속성은 XAML과 c # 모두에서 설정할 수 있습니다.

### <a name="xaml"></a>XAML

`MenuItem`XAML 컬렉션에서 요소를 만들 수 있습니다. 다음 XAML에서는 두 개의 컨텍스트 작업이 구현 된 사용자 지정 셀을 보여 줍니다.

```xaml
<ListView x:Name="ContextDemoList">
  <ListView.ItemTemplate>
    <DataTemplate>
      <ViewCell>
         <ViewCell.ContextActions>
            <MenuItem Clicked="OnMore"
                      CommandParameter="{Binding .}"
                      Text="More" />
            <MenuItem Clicked="OnDelete"
                      CommandParameter="{Binding .}"
                      Text="Delete" IsDestructive="True" />
         </ViewCell.ContextActions>
         <StackLayout Padding="15,0">
              <Label Text="{Binding title}" />
         </StackLayout>
      </ViewCell>
    </DataTemplate>
  </ListView.ItemTemplate>
</ListView>
```

코드 파일에서 메서드가 구현 되었는지 확인 합니다 `Clicked` .

```csharp
public void OnMore (object sender, EventArgs e)
{
    var mi = ((MenuItem)sender);
    DisplayAlert("More Context Action", mi.CommandParameter + " more context action", "OK");
}

public void OnDelete (object sender, EventArgs e)
{
    var mi = ((MenuItem)sender);
    DisplayAlert("Delete Context Action", mi.CommandParameter + " delete context action", "OK");
}
```

> [!NOTE]
> `NavigationPageRenderer`Android 용에는 `UpdateMenuItemIcon` 사용자 지정에서 아이콘을 로드 하는 데 사용할 수 있는 재정의 가능한 메서드가 있습니다 `Drawable` . 이 재정의를 통해 Android의 인스턴스에서 SVG 이미지를 아이콘으로 사용할 수 있습니다 `MenuItem` .

### <a name="code"></a>코드

컨텍스트 작업은 인스턴스를 만들고 해당 `Cell` `MenuItem` 셀의 컬렉션에 추가 하 여 모든 하위 클래스 (그룹 헤더로 사용 되지 않는 한)에서 구현 될 수 있습니다 `ContextActions` . 컨텍스트 작업에 대해 다음 속성을 구성할 수 있습니다.

- **텍스트** &ndash; 메뉴 항목에 표시 되는 문자열입니다.
- **클릭** &ndash; 항목을 클릭할 때 발생 하는 이벤트입니다.
- **Isdestructive** &ndash; (선택 사항) true 이면 항목이 iOS에서 다르게 렌더링 됩니다.

여러 컨텍스트 작업을 셀에 추가할 수 있지만 하나는 `IsDestructive` 로 설정 해야 `true` 합니다. 다음 코드는 컨텍스트 작업을에 추가 하는 방법을 보여 줍니다 `ViewCell` .

```csharp
var moreAction = new MenuItem { Text = "More" };
moreAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
moreAction.Clicked += async (sender, e) =>
{
    var mi = ((MenuItem)sender);
    Debug.WriteLine("More Context Action clicked: " + mi.CommandParameter);
};

var deleteAction = new MenuItem { Text = "Delete", IsDestructive = true }; // red background
deleteAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
deleteAction.Clicked += async (sender, e) =>
{
    var mi = ((MenuItem)sender);
    Debug.WriteLine("Delete Context Action clicked: " + mi.CommandParameter);
};
// add to the ViewCell's ContextActions property
ContextActions.Add (moreAction);
ContextActions.Add (deleteAction);
```

## <a name="pull-to-refresh"></a>당겨서 새로 고침

사용자가 데이터 목록에서 아래로 끌면 해당 목록이 새로 고쳐집니다. 컨트롤은이 기본 기능을 [`ListView`](xref:Xamarin.Forms.ListView) 지원 합니다. 끌어오기-새로 고침 기능을 사용 하려면 [`IsPullToRefreshEnabled`](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) 를로 설정 합니다 `true` .

```xaml
<ListView ...
          IsPullToRefreshEnabled="true" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
listView.IsPullToRefreshEnabled = true;
```

회전자는 새로 고침 중에 표시 되며 기본적으로 검은색입니다. 그러나 속성을로 설정 하 여 iOS 및 Android에서 회전자 색을 변경할 수 있습니다 `RefreshControlColor` [`Color`](xref:Xamarin.Forms.Color) .

```xaml
<ListView ...
          IsPullToRefreshEnabled="true"
          RefreshControlColor="Red" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
listView.RefreshControlColor = Color.Red;
```

다음 스크린샷은 사용자가 끌어오기를 수행 하는 동안 끌어오기를 새로 고치는 방법을 보여 줍니다.

![](interactivity-images/refresh-start.png "ListView Pull to Refresh In-Progress")

다음 스크린샷은 사용자가 끌어오기를 릴리스한 후를 업데이트 하는 동안 회전자가 표시 되도록 끌어오기를 새로 고치는 방법을 보여 줍니다 [`ListView`](xref:Xamarin.Forms.ListView) .

![](interactivity-images/refresh-in-progress.png "ListView Pull to Refresh Complete")

[`ListView`](xref:Xamarin.Forms.ListView)이벤트를 발생 [`Refreshing`](xref:Xamarin.Forms.ListView.Refreshing) 하 여 새로 고침을 시작 하 고 [`IsRefreshing`](xref:Xamarin.Forms.ListView.IsRefreshing) 속성이로 설정 됩니다 `true` . 의 콘텐츠를 새로 고치는 데 필요한 모든 코드는 `ListView` 이벤트에 대 한 이벤트 처리기 또는에서 실행 되는 메서드에 의해 실행 되어야 합니다 `Refreshing` [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand) . 를 `ListView` 새로 고치면 `IsRefreshing` 속성이로 설정 `false` 되거나, [`EndRefresh`](xref:Xamarin.Forms.ListView.EndRefresh) 새로 고침이 완료 되었음을 나타내기 위해 메서드를 호출 해야 합니다.

> [!NOTE]
> 를 정의할 때 명령의 [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand) `CanExecute` 메서드를 지정 하 여 명령을 사용 하거나 사용 하지 않도록 설정할 수 있습니다.

## <a name="detect-scrolling"></a>스크롤 검색

[`ListView`](xref:Xamarin.Forms.ListView)`Scrolled`스크롤이 발생 했음을 나타내기 위해 발생 하는 이벤트를 정의 합니다. 다음 XAML 예제에서는 `ListView` 이벤트에 대 한 이벤트 처리기를 설정 하는을 보여 줍니다 `Scrolled` .

```xaml
<ListView Scrolled="OnListViewScrolled">
    ...
</ListView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
ListView listView = new ListView();
listView.Scrolled += OnListViewScrolled;
```

이 코드 예제에서 이벤트 `OnListViewScrolled` 처리기는 이벤트가 발생할 때 실행 됩니다 `Scrolled` .

```csharp
void OnListViewScrolled(object sender, ScrolledEventArgs e)
{
    Debug.WriteLine("ScrollX: " + e.ScrollX);
    Debug.WriteLine("ScrollY: " + e.ScrollY);  
}
```

`OnListViewScrolled`이벤트 처리기는 `ScrolledEventArgs` 이벤트와 함께 제공 되는 개체의 값을 출력 합니다.

## <a name="related-links"></a>관련 링크

- [ListView 대화형 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)
