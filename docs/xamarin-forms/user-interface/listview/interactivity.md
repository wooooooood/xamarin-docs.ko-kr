---
title: ListView 대화형 작업
description: 이 문서에서는 선택, 상황에 맞는 작업 및 끌어오기-새로 고침을 구현 하 여 Xamarin.Forms ListView에 대화형 작업을 추가 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/25/2019
ms.openlocfilehash: aa717792bdaefe24d957c9781934933b67aaf92b
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306436"
---
# <a name="listview-interactivity"></a>ListView 대화형 작업

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)

Xamarin.ios [`ListView`](xref:Xamarin.Forms.ListView) 클래스는 표시 되는 데이터와의 사용자 상호 작용을 지원 합니다.

## <a name="selection-and-taps"></a>선택 및 탭

[`ListView`](xref:Xamarin.Forms.ListView) 선택 모드는 [`ListView.SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) 속성을 [`ListViewSelectionMode`](xref:Xamarin.Forms.ListViewSelectionMode) 열거형 값으로 설정 하 여 제어 됩니다.

- [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) 선택한 항목이 강조 표시 된 상태로 단일 항목을 선택할 수 있음을 나타냅니다. 이것은 기본값입니다.
- [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) 항목을 선택할 수 없음을 나타냅니다.

사용자가 항목을 누르면 두 이벤트가 발생 합니다.

- 새 항목을 선택 하면 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 발생 합니다.
- 항목을 탭 할 때 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 발생 합니다.

동일한 항목을 두 번 누르면 두 개의 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트가 발생 하지만 단일 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트만 발생 합니다.

> [!NOTE]
> [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트에 대 한 이벤트 인수를 포함 하는 [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs) 클래스에는 [`Group`](xref:Xamarin.Forms.ItemTappedEventArgs.Group) 및 [`Item`](xref:Xamarin.Forms.ItemTappedEventArgs.Item) 속성이 있으며 해당 값이 탭 항목의 [`ItemIndex`](xref:Xamarin.Forms.ListView) 에 있는 인덱스를 나타내는`ListView`속성이 있습니다. 마찬가지로, [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트에 대 한 이벤트 인수를 포함 하는 [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs) 클래스에는 [`SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) 속성이 있으며 해당 값이 선택 된 항목의 `ListView`에 있는 인덱스를 나타내는 `SelectedItemIndex` 속성이 있습니다.

[`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) 속성이 [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single)로 설정 된 경우 [`ListView`](xref:Xamarin.Forms.ListView) 항목을 선택할 수 있으며, [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 및 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트가 발생 하 고 [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) 속성이 선택 된 항목의 값으로 설정 됩니다.

[`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) 속성이 [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None)로 설정 된 경우 [`ListView`](xref:Xamarin.Forms.ListView) 의 항목을 선택할 수 없으며 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트가 발생 하지 않으며 [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) 속성이 `null`유지 됩니다. 그러나 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트는 계속 발생 하 고 탭 항목이 잠깐 동안 강조 표시 됩니다.

항목이 선택 되 고 [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) 속성이 [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) 에서 [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None)으로 변경 되 면 [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) 속성이 `null`로 설정 되 고`ItemSelected`이벤트가 `null` 항목으로 [발생 합니다.](xref:Xamarin.Forms.ListView.ItemSelected)

다음 스크린샷은 기본 선택 모드를 사용 하는 [`ListView`](xref:Xamarin.Forms.ListView) 를 보여 줍니다.

![](interactivity-images/selection-default.png "ListView with Selection Enabled")

### <a name="disable-selection"></a>선택 사용 안 함

[`ListView`](xref:Xamarin.Forms.ListView) 선택을 사용 하지 않도록 설정 하려면 [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) 속성을 [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None)로 설정 합니다.

```xaml
<ListView ... SelectionMode="None" />
```

```csharp
var listView = new ListView { ... SelectionMode = ListViewSelectionMode.None };
```

## <a name="context-actions"></a>컨텍스트 작업

사용자가 `ListView`의 항목에 대 한 조치를 취하는 경우가 종종 있습니다. 예를 들어 메일 앱에서 전자 메일 목록을 것이 좋습니다. IOS에서 살짝 밀어 메시지를 삭제할 수 있습니다.

![](interactivity-images/context-default.png "ListView with Context Actions")

C# 및 XAML의 상황에 맞는 작업을 구현할 수 있습니다. 아래 있습니다 특정 가이드 둘 다에 대 한 우선 보겠습니다 둘 다에 대 한 몇 가지 중요 한 구현 세부 정보를 살펴보세요.

`MenuItem` 요소를 사용 하 여 컨텍스트 작업을 만듭니다. `MenuItems` 개체에 대 한 탭 이벤트는 `ListView`아닌 `MenuItem` 자체에 의해 발생 합니다. 이는 셀에 대해 탭 이벤트를 처리 하는 방법과 다릅니다. 여기에서 `ListView`은 셀이 아닌 이벤트를 발생 시킵니다. `ListView`에서 이벤트를 발생 시키기 때문에 해당 이벤트 처리기에는 선택 하거나 탭 하는 항목과 같은 키 정보가 제공 됩니다.

기본적으로 `MenuItem`는 속한 셀을 알 수 없습니다. `CommandParameter` 속성은 `MenuItem`의 `ViewCell`뒤에 있는 개체와 같은 개체를 저장 하는 `MenuItem`에서 사용할 수 있습니다. `CommandParameter` 속성은 XAML 및 C#에서 설정할 수 있습니다.

### <a name="xaml"></a>XAML

`MenuItem` 요소는 XAML 컬렉션에서 만들 수 있습니다. 아래의 XAML 구현 하는 두 가지 상황에 맞는 작업을 사용 하 여 사용자 지정 셀을 보여 줍니다.

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

코드 숨김이 파일에서 `Clicked` 메서드가 구현 되었는지 확인 합니다.

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
> Android 용 `NavigationPageRenderer`에는 사용자 지정 `Drawable`에서 아이콘을 로드 하는 데 사용할 수 있는 재정의 가능한 `UpdateMenuItemIcon` 메서드가 있습니다. 이 재정의를 통해 Android의 `MenuItem` 인스턴스에서 SVG 이미지를 아이콘으로 사용할 수 있습니다.

### <a name="code"></a>코드

컨텍스트 작업은 `MenuItem` 인스턴스를 만들고 해당 셀에 대 한 `ContextActions` 컬렉션에 추가 하 여 `Cell` 하위 클래스 (그룹 헤더로 사용 되지 않는 한)에서 구현 될 수 있습니다. 다음과 같은 상황에 맞는 작업에 대 한 속성을 구성할 수 있습니다.

- 메뉴 항목에 표시 되는 문자열 &ndash; **텍스트** 입니다.
- 항목을 클릭할 때 이벤트를 &ndash; **클릭** 합니다.
- **Isdestructive** &ndash; (선택 사항) true 이면 항목이 iOS에서 다르게 렌더링 됩니다.

여러 컨텍스트 작업을 셀에 추가할 수 있지만 하나는 `true`로 `IsDestructive` 설정 해야 합니다. 다음 코드는 `ViewCell`에 컨텍스트 작업을 추가 하는 방법을 보여 줍니다.

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

## <a name="pull-to-refresh"></a>새로 고치려면 끌어오기

사용자는 데이터 목록을 아래로 끌어서 새로 고쳐집니다 목록 예상 제대로 찾아 오셨습니다. [`ListView`](xref:Xamarin.Forms.ListView) 컨트롤은이 기능을 지원 합니다. 끌어오기-새로 고침 기능을 사용 하도록 설정 하려면 [`IsPullToRefreshEnabled`](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) 를 `true`으로 설정 합니다.

```xaml
<ListView ...
          IsPullToRefreshEnabled="true" />
```

해당 하는 C# 코드가입니다.

```csharp
listView.IsPullToRefreshEnabled = true;
```

회전자는 새로 고침 중에 표시 되며 기본적으로 검은색입니다. 그러나 `RefreshControlColor` 속성을 [`Color`](xref:Xamarin.Forms.Color)설정 하 여 IOS 및 Android에서 회전자 색을 변경할 수 있습니다.

```xaml
<ListView ...
          IsPullToRefreshEnabled="true"
          RefreshControlColor="Red" />
```

해당 하는 C# 코드가입니다.

```csharp
listView.RefreshControlColor = Color.Red;
```

다음 스크린샷은 사용자가 끌어오기를 수행 하는 동안 끌어오기를 새로 고치는 방법을 보여 줍니다.

![](interactivity-images/refresh-start.png "ListView Pull to Refresh In-Progress")

다음 스크린샷은 사용자가 끌어오기를 릴리스한 후 [`ListView`](xref:Xamarin.Forms.ListView) 를 업데이트 하는 동안 회전자가 표시 되도록 끌어오기를 새로 고치는 방법을 보여 줍니다.

![](interactivity-images/refresh-in-progress.png "ListView Pull to Refresh Complete")

[`ListView`](xref:Xamarin.Forms.ListView) [`Refreshing`](xref:Xamarin.Forms.ListView.Refreshing) 이벤트를 발생 하 여 새로 고침을 시작 하 고 [`IsRefreshing`](xref:Xamarin.Forms.ListView.IsRefreshing) 속성이 `true`로 설정 됩니다. `ListView` 콘텐츠를 새로 고치는 데 필요한 모든 코드는 `Refreshing` 이벤트에 대 한 이벤트 처리기 또는 [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand)에서 실행 되는 메서드에 의해 실행 되어야 합니다. `ListView`를 새로 고치면 `IsRefreshing` 속성을 `false`로 설정 하거나, 새로 고침이 완료 되었음을 나타내기 위해 [`EndRefresh`](xref:Xamarin.Forms.ListView.EndRefresh) 메서드를 호출 해야 합니다.

> [!NOTE]
> [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand)를 정의할 때 명령의 `CanExecute` 메서드를 지정 하 여 명령을 사용 하거나 사용 하지 않도록 설정할 수 있습니다.

## <a name="detect-scrolling"></a>스크롤 검색

[`ListView`](xref:Xamarin.Forms.ListView) 은 스크롤이 발생 했음을 나타내기 위해 발생 하는 `Scrolled` 이벤트를 정의 합니다. 다음 XAML 예제에서는 `Scrolled` 이벤트에 대 한 이벤트 처리기를 설정 하는 `ListView` 보여 줍니다.

```xaml
<ListView Scrolled="OnListViewScrolled">
    ...
</ListView>
```

해당 하는 C# 코드가입니다.

```csharp
ListView listView = new ListView();
listView.Scrolled += OnListViewScrolled;
```

이 코드 예제에서는 `Scrolled` 이벤트가 발생 하면 `OnListViewScrolled` 이벤트 처리기가 실행 됩니다.

```csharp
void OnListViewScrolled(object sender, ScrolledEventArgs e)
{
    Debug.WriteLine("ScrollX: " + e.ScrollX);
    Debug.WriteLine("ScrollY: " + e.ScrollY);  
}
```

`OnListViewScrolled` 이벤트 처리기는 이벤트와 함께 제공 되는 `ScrolledEventArgs` 개체의 값을 출력 합니다.

## <a name="related-links"></a>관련 링크

- [ListView 대화형 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)
