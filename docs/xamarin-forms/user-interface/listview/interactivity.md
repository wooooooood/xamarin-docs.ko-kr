---
title: ListView 상호 작용
description: 이 문서에서는 선택 영역, 삭제 하려면 살짝 및 끌어오기-새로 고침을 구현 하 여 Xamarin.Forms ListView에 대화형 작업을 추가 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/01/2018
ms.openlocfilehash: 64ffda681c51c21b7485f0865af4b740316edaaa
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245109"
---
# <a name="listview-interactivity"></a>ListView 상호 작용

ListView에서는 다음 방법을 제공 하는 데이터와 상호 작용을 지원 합니다.

- [**선택 영역 및 탭** ](#selectiontaps) &ndash; 탭 및 항목의 선택 항목/deselections에 응답 합니다. 행 선택 (기본적으로 사용)를 사용 하지 않도록 설정 하거나 사용 합니다.
- [**상황에 맞는 작업** ](#Context_Actions) &ndash; 살짝-삭제 항목, 예를 들어 당 노출 기능입니다.
- [**끌어오기-새로 고침** ](#Pull_to_Refresh) &ndash; 하 사용자가 기대 하 네이티브 경험에서 끌어오기-새로 고침 관용구를 구현 합니다.

<a name="selectiontaps" />

## <a name="selection--taps"></a>선택 영역 및 탭
`ListView` 한 번에 하나의 항목을 선택할을 수 있습니다. 에 기본적으로 선택이 됩니다. 두 이벤트는 발생 하는 사용자가 항목을 누르면: `ItemTapped` 및 `ItemSelected`합니다. 동일한 항목을 두 번 눌러 다중 발생 하지 것입니다 참고 `ItemSelected` 이벤트를 다중 트리거는 발생 하지만 `ItemTapped` 이벤트입니다. 또한 `ItemSelected` 항목을 선택 해제 하는 경우 호출 됩니다.

주의 `ItemSelected` 항목의 선택이 취소 됩니다 때와 선택할 때 호출 됩니다. 즉, null 확인 해야 `SelectedItem` 에 프로그램 `ItemSelected` 이벤트 처리기를 사용할 수 있습니다.

```csharp
void OnSelection (object sender, SelectedItemChangedEventArgs e)
{
  if (e.SelectedItem == null) {
    return; //ItemSelected is called on deselection, which results in SelectedItem being set to null
  }
  DisplayAlert ("Item Selected", e.SelectedItem.ToString (), "Ok");
  //((ListView)sender).SelectedItem = null; //uncomment line if you want to disable the visual selection state.
}
```

### <a name="disabling-selection"></a>선택 영역을 사용 하지 않도록 설정

선택 영역을 사용 하지 않도록 설정 하려는 경우 처리는 `ItemSelected` 이벤트 집합과 `SelectedItem` 속성을 null로:

```csharp
SelectionDemoList.ItemSelected += (sender, e) => {
    ((ListView)sender).SelectedItem = null;
};
```

선택 영역을 사용 하도록 설정:

![](interactivity-images/selection-default.png "선택이 가능한 있는 ListView")

<a name="Context_Actions" />

## <a name="context-actions"></a>상황에 맞는 작업
대개 사용자가 사용할 항목에 대해 조치를 취할는 `ListView`합니다. 예를 들어 메일 응용 프로그램에서 전자 메일 주소 목록입니다. Ios에서 메시지를 삭제할 살짝::

![](interactivity-images/context-default.png "상황에 맞는 작업으로 ListView")

C# 및 XAML에서 상황에 맞는 작업을 구현할 수 있습니다. 아래 찾을 수 있습니다 특정 지침, 둘 다에 대해 먼저 하지만 보겠습니다 모두에 대 한 몇 가지 중요 한 구현 세부 정보를 살펴봅니다.

상황에 맞는 작업을 사용 하 여 만들어지며 `MenuItem`s입니다. ListView 하지 MenuItem 자체에서 Menuitem에 대 한 tap 이벤트가 발생 합니다. 이 ListView를 셀 보다는 이벤트를 발생 하는 여기서 셀에 대 한 tap 이벤트가 처리 되는 방식에서 다릅니다. ListView 이벤트를 발생을 하기 때문에 해당 이벤트 처리기는 같은 항목을 선택 하거나 누른 키 정보를 제공 됩니다.

기본적으로는 MenuItem에 속해 있는 셀을 알 수 없습니다에 있습니다. `CommandParameter` 사용할 수 `MenuItem` MenuItem의 ViewCell 뒤에 있는 개체와 같은 개체를 저장할 수 있습니다. `CommandParameter` XAML 및 C#에서 설정할 수 있습니다.

### <a name="c"></a>C#  

모든 상황에 맞는 작업을 구현할 수 있습니다 `Cell` (만큼 그룹 머리글로 사용 되 고 있지) 하위 클래스를 만들어 `MenuItem`s에 추가 하는 `ContextActions` 셀에 대 한 컬렉션입니다. 다음과 같은 상황에 맞는 작업에 대 한 속성을 구성할 수 있습니다.

* **텍스트** &ndash; 메뉴 항목에 표시 되는 문자열입니다.
* **클릭** &ndash; 항목을 클릭할 때 이벤트입니다.
* **IsDestructive** &ndash; true (선택 사항) 항목은 다른 방식으로 렌더링 iOS에서 합니다.

하지만 여러 상황에 맞는 작업 하나에만 사용 해야 한 셀에 추가할 수 있습니다 `IsDestructive` 로 설정 `true`합니다. 다음 코드는 상황에 맞는 작업을 추가 하는 하는 방법을 보여 줍니다.는 `ViewCell`:

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

`MenuItem`s는 XAML 컬렉션에서 선언적으로 만들 수도 있습니다. 다음 XAML 구현 하는 두 가지 상황에 맞는 작업으로 사용자 지정 셀을 보여 줍니다.

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

코드 숨김 파일에는 `Clicked` 메서드가 구현 됩니다.

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
> `NavigationPageRenderer` for Android에는 재정의 가능한 `UpdateMenuItemIcon` 사용자 지정에서 아이콘을 로드 하는 데 사용할 수 있는 메서드 `Drawable`합니다. 이 재정의 사용 하면에 아이콘으로 SVG 이미지를 사용 하 여 `MenuItem` Android에서 인스턴스.

<a name="Pull_to_Refresh" />

## <a name="pull-to-refresh"></a>새로 고침을 밀어넣을
데이터 목록에서 끌어와 해당 목록을 새로 고쳐집니다 기대 하는 사용자가 발견 했을 합니다. `ListView` 이-의-즉시 사용을 지원합니다. 끌어오기-새로 고침 기능을 사용 하려면 설정 `IsPullToRefreshEnabled` true로:

```csharp
listView.IsPullToRefreshEnabled = true;
```

끌어오기-새로 고침 사용자로 가져오는:

![](interactivity-images/refresh-start.png "ListView 진행 중인를 새로 고치려면 끌어오세요")

끌어오기-새로 고침 사용자로 끌어오기를 출시 했습니다. 이 사용자 목록을 업데이트 하는 동안 표시: ![ ] (interactivity-images/refresh-in-progress.png "ListView 끌어오세요 전체 새로 고침")

ListView는 끌어오기-새로 고침 이벤트에 응답할 수 있도록 하는 몇 가지 이벤트를 노출 합니다.

-  `RefreshCommand` 호출 될 및 `Refreshing` 호출 하는 이벤트입니다. `IsRefreshing` 로 설정 됩니다 `true`합니다.
-  모든 코드는 명령 또는 이벤트 목록 보기의 콘텐츠를 새로 고칠 때 필요를 수행 해야 합니다.
-  새로 고칠 때를 마치면 호출 `EndRefresh` 설정 또는 `IsRefreshing` 를 `false` 를 삭제 하 고 나면 목록 보기.

`CanExecute` 를 제공 하는 제어 하는 방법을 끌어오기-새로 고침 명령을 활성화할지 여부를 속성이 적용 됩니다.



## <a name="related-links"></a>관련 링크

- [ListView 상호 작용 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
- [1.4의 릴리스 정보](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1.3 릴리스 정보](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
