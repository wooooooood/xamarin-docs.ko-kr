---
title: ListView 대화형 작업
description: 이 문서에서는 선택, 상황에 맞는 작업 및 끌어오기-새로 고침을 구현 하 여 Xamarin.Forms ListView에 대화형 작업을 추가 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2018
ms.openlocfilehash: 939df6cfd17de82e28958363cfa51cd199f928cb
ms.sourcegitcommit: 93c9fe61eb2cdfa530960b4253eb85161894c882
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55831693"
---
# <a name="listview-interactivity"></a>ListView 대화형 작업

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)

[`ListView`](xref:Xamarin.Forms.ListView) 표시 데이터와 상호 작용을 지원 합니다.

<a name="selectiontaps" />

## <a name="selection--taps"></a>선택 및 탭

[ `ListView` ](xref:Xamarin.Forms.ListView) 선택 모드를 설정 하 여 제어 합니다 [ `ListView.SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) 속성 값을를 [ `ListViewSelectionMode` ](xref:Xamarin.Forms.ListViewSelectionMode) 열거형:

- [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) 단일 항목을 선택할 수 있는 있는, 선택한 항목이 강조 표시를 나타냅니다. 기본값입니다.
- [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) 항목을 선택할 수를 나타냅니다.

사용자가 항목을 누르면 두 이벤트가 발생 합니다.

- [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 새 항목이 선택 될 때 발생 합니다.
- [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 항목을 탭 할 때 발생 합니다.

> [!NOTE]
> 두 실행은 동일한 항목을 두 번 눌러 [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트 하지만 됩니다만 화재 단일 [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트입니다.

경우는 [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) 속성이로 설정 되어 [ `Single` ](xref:Xamarin.Forms.ListViewSelectionMode.Single), 항목를 [ `ListView` ](xref:Xamarin.Forms.ListView) 을 선택할 수는 [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) 하 고 [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트가 발생 하지 것입니다, 그리고 및 [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) 속성이 선택한 항목의 값으로 설정 됩니다.

경우는 [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) 속성이로 설정 되어 [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None), 항목을 [ `ListView` ](xref:Xamarin.Forms.ListView) 선택할 수 없습니다를 [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트를 발생 하지 것입니다, 그리고 및 [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) 속성 남아 `null`합니다. 그러나 [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트가 아직 발생 하지 것입니다 및 탭된 항목 탭 도중 간단 하 게 강조 표시 됩니다.

항목이 선택 된 경우와 [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) 에서 속성이 변경 될 [ `Single` ](xref:Xamarin.Forms.ListViewSelectionMode.Single) 하 [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None), [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) 속성으로 설정 됩니다 `null` 하며 [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) 사용 하 여 이벤트가 발생을 `null` 항목입니다.

다음 스크린샷에서 표시 된 [ `ListView` ](xref:Xamarin.Forms.ListView) 기본 선택 모드를 사용 하 여:

![](interactivity-images/selection-default.png "사용 하도록 설정 하는 선택 된 ListView")

### <a name="disabling-selection"></a>선택 영역을 사용 하지 않도록 설정

사용 하지 않으려면 [ `ListView` ](xref:Xamarin.Forms.ListView) 선택 집합을 [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) 속성을 [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None):

```xaml
<ListView ... SelectionMode="None" />
```

```csharp
var listView = new ListView { ... SelectionMode = ListViewSelectionMode.None };
```

<a name="Context_Actions" />

## <a name="context-actions"></a>상황에 맞는 작업

사용자는 항목에 대해 작업을 수행 하려면 종종는 `ListView`합니다. 예를 들어 메일 앱에서 전자 메일 목록을 것이 좋습니다. Ios의 경우 메시지를 삭제 하려면 살짝::

![](interactivity-images/context-default.png "상황에 맞는 작업을 사용 하 여 ListView")

C# 및 XAML의 상황에 맞는 작업을 구현할 수 있습니다. 아래 있습니다 특정 가이드 둘 다에 대 한 우선 보겠습니다 둘 다에 대 한 몇 가지 중요 한 구현 세부 정보를 살펴보세요.

상황에 맞는 작업을 사용 하 여 만들어집니다 `MenuItem`s입니다. ListView 하지 MenuItem 자체에서 MenuItems에 대 한 탭 이벤트가 발생 합니다. 셀에는 ListView 셀 대신 이벤트를 발생 하는 위치 탭 이벤트 처리 방법에서 다릅니다. ListView의 이벤트를 발생 시킨 때문에 해당 이벤트 처리기는 같은 항목을 선택 하거나 탭 한 키 정보가 제공 됩니다.

기본적으로 MenuItem에 속하는 셀을 알 수 없습니다. `CommandParameter` 사용할 수 있습니다 `MenuItem` 로 MenuItem의 ViewCell 뒤의 개체가 같은 개체를 저장 합니다. `CommandParameter` XAML과 C# 모두에서 설정할 수 있습니다.

### <a name="c"></a>C#  

있는 상황에 맞는 작업을 구현할 수 있습니다 `Cell` 서브 클래스 (한 그룹 헤더로 사용 되 고 있지)를 만들어 `MenuItem`s에 추가 하는 `ContextActions` 셀에 대 한 컬렉션입니다. 다음과 같은 상황에 맞는 작업에 대 한 속성을 구성할 수 있습니다.

* **텍스트** &ndash; 메뉴 항목에 표시 되는 문자열입니다.
* **클릭할** &ndash; 항목을 클릭할 때 이벤트입니다.
* **IsDestructive** &ndash; true (선택 사항) 항목에서 다르게 렌더링 됩니다 iOS입니다.

하지만 여러 상황에 맞는 작업 하나만 있어야 셀에 추가할 수 있습니다 `IsDestructive` 로 `true`합니다. 다음 코드는 상황에 맞는 작업에 추가 되는 방법을 보여 줍니다는 `ViewCell`:

```csharp
var moreAction = new MenuItem { Text = "More" };
moreAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
moreAction.Clicked += async (sender, e) => {
    var mi = ((MenuItem)sender);
    Debug.WriteLine("More Context Action clicked: " + mi.CommandParameter);
};

var deleteAction = new MenuItem { Text = "Delete", IsDestructive = true }; // red background
deleteAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
deleteAction.Clicked += async (sender, e) => {
    var mi = ((MenuItem)sender);
    Debug.WriteLine("Delete Context Action clicked: " + mi.CommandParameter);
};
// add to the ViewCell's ContextActions property
ContextActions.Add (moreAction);
ContextActions.Add (deleteAction);
```

### <a name="xaml"></a>XAML

`MenuItem`s는 XAML 컬렉션에서 선언적으로 만들 수도 있습니다. 아래의 XAML 구현 하는 두 가지 상황에 맞는 작업을 사용 하 여 사용자 지정 셀을 보여 줍니다.

```xaml
<ListView x:Name="ContextDemoList">
  <ListView.ItemTemplate>
    <DataTemplate>
      <ViewCell>
         <ViewCell.ContextActions>
            <MenuItem Clicked="OnMore" CommandParameter="{Binding .}"
               Text="More" />
            <MenuItem Clicked="OnDelete" CommandParameter="{Binding .}"
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

코드 숨김 파일에서 확인 된 `Clicked` 메서드가 구현 됩니다.

```csharp
public void OnMore (object sender, EventArgs e) {
    var mi = ((MenuItem)sender);
    DisplayAlert("More Context Action", mi.CommandParameter + " more context action", "OK");
}

public void OnDelete (object sender, EventArgs e) {
    var mi = ((MenuItem)sender);
    DisplayAlert("Delete Context Action", mi.CommandParameter + " delete context action", "OK");
}
```

> [!NOTE]
> 합니다 `NavigationPageRenderer` Android에는 재정의 가능한 `UpdateMenuItemIcon` 사용자 지정에서 아이콘을 로드 하는 메서드 `Drawable`합니다. 이 재정의 가능 하도록 SVG 이미지 아이콘으로 사용할 `MenuItem` Android에서 인스턴스.

<a name="Pull_to_Refresh" />

## <a name="pull-to-refresh"></a>고치려면 당김

사용자는 데이터 목록을 아래로 끌어서 새로 고쳐집니다 목록 예상 제대로 찾아 오셨습니다. [`ListView`](xref:Xamarin.Forms.ListView) 이-의-기본을 지원합니다. 끌어오기-새로 고침 기능을 사용 하려면 [ `IsPullToRefreshEnabled` ](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) 하려면 `true`:

```xaml
<ListView ...
          IsPullToRefreshEnabled="true" />
```

해당 하는 C# 코드가입니다.

```csharp
listView.IsPullToRefreshEnabled = true;
```

기본적으로 검정색 인 새로 고치는 동안 회전자 표시 됩니다. 그러나 설정 하 여 iOS 및 Android에서 회전자 색상에 변경할 수는 합니다 `RefreshControlColor` 속성을 [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<ListView ...
          IsPullToRefreshEnabled="true"
          RefreshControlColor="Red" />
```

해당 하는 C# 코드가입니다.

```csharp
listView.RefreshControlColor = Color.Red;
```

다음 스크린샷은 사용자 끌어오는으로 끌어오기-새로 고침을 보여 줍니다.

![](interactivity-images/refresh-start.png "ListView 당겨서 새로 고침 진행률의")

다음 스크린샷은 사용자가 표시 되는 회전자를 사용 하 여 끌어오기, 출시 된 후 끌어오기-새로 고침을 표시 하는 동안 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 업데이트:

![](interactivity-images/refresh-in-progress.png "ListView 당겨서 새로 고침 완료")

[`ListView`](xref:Xamarin.Forms.ListView) 발생 합니다 [ `Refreshing` ](xref:Xamarin.Forms.ListView.Refreshing) 새로 고침을 시작 하는 이벤트와 [ `IsRefreshing` ](xref:Xamarin.Forms.ListView.IsRefreshing) 속성에 설정할 `true`합니다. 내용 새로 고침 하는 데 필요한 코드를는 `ListView` 다음에 대 한 이벤트 처리기에서 실행할지를 `Refreshing` 이벤트 또는 실행 메서드에서 [ `RefreshCommand` ](xref:Xamarin.Forms.ListView.RefreshCommand)합니다. 한 번를 `ListView` 는 새로 고쳐지고를 `IsRefreshing` 속성 설정 해야 `false`, 또는 [ `EndRefresh` ](xref:Xamarin.Forms.ListView.EndRefresh) 메서드를 호출 해야 새로 고침 완료 되었음을 나타냅니다.

> [!NOTE]
> 정의 하는 경우는 [ `RefreshCommand` ](xref:Xamarin.Forms.ListView.RefreshCommand)는 `CanExecute` 명령의 메서드 명령을 사용할지 여부를 지정할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [ListView 대화형 작업 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
