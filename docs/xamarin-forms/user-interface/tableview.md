---
title: Xamarin.Forms TableView
description: 이 문서에서는 Xamarin.Forms TableView 클래스를 사용 하 여 응용 프로그램에서 스크롤 메뉴, 설정 및 입력된 폼을 표시 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2018
ms.openlocfilehash: fda8c08cdbdab01f5d68b7f349d5f28f31316290
ms.sourcegitcommit: 93c9fe61eb2cdfa530960b4253eb85161894c882
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55832004"
---
# <a name="xamarinforms-tableview"></a>Xamarin.Forms TableView

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)

[`TableView`](xref:Xamarin.Forms.TableView) 스크롤 가능한 목록이 데이터 나 선택 항목을 표시 하기 위한 뷰입니다 같은 서식 파일을 공유 하지 않는 행이 있습니다. 와 달리 [ListView](~/xamarin-forms/user-interface/listview/index.md)를 `TableView` 개념이 없는 `ItemsSource`이므로 자식으로 항목을 수동으로 추가 되어야 합니다.

![](tableview-images/tableview-all-sml.png "TableView 예제")

<a name="Use_Cases" />

## <a name="use-cases"></a>사용 사례

[`TableView`](xref:Xamarin.Forms.TableView) 경우에 유용 합니다.

- 설정의 목록 표시
- 폼에서 데이터 수집 또는
- 행의 행 (예: 숫자, 백분율 및 이미지)를 다르게 표시 되는 데이터를 표시 합니다.

[`TableView`](xref:Xamarin.Forms.TableView) 스크롤 및 매력적인 섹션에서는 위의 시나리오에 대 한 일반적인 필요한 행 레이아웃을 처리 합니다. `TableView` 컨트롤은 각 플랫폼의 기본 사용 가능한 경우 각 플랫폼에 대 한 기본 모양 만들기 해당 뷰를 사용 합니다.

<a name="TableView_Structure" />

## <a name="structure"></a>구조체

요소를 [ `TableView` ](xref:Xamarin.Forms.TableView) 섹션으로 구성 됩니다. 루트에는 `TableView` 은 합니다 [ `TableRoot` ](xref:Xamarin.Forms.TableRoot)는 하나 이상의 부모 [ `TableSection` ](xref:Xamarin.Forms.TableSection) 인스턴스. 각 [ `TableSection` ](xref:Xamarin.Forms.TableSection) 제목 및 하나 이상의 이루어져 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) 인스턴스:

```xaml
<TableView Intent="Settings">
    <TableRoot>
        <TableSection Title="Ring">
            <SwitchCell Text="New Voice Mail" />
            <SwitchCell Text="New Mail" On="true" />
        </TableSection>
    </TableRoot>
</TableView>
```

해당 하는 C# 코드가입니다.

```csharp
Content = new TableView
{
    Root = new TableRoot
    {
        new TableSection("Ring")
        {
          // TableSection constructor takes title as an optional parameter
          new SwitchCell { Text = "New Voice Mail" },
          new SwitchCell { Text = "New Mail", On = true }
        }
    },
    Intent = TableIntent.Settings
};
```

<a name="TableView_Appearance" />

## <a name="appearance"></a>모양

[`TableView`](xref:Xamarin.Forms.TableView) 노출 합니다 [ `Intent` ](xref:Xamarin.Forms.TableView.Intent) 중 하나로 설정할 수 있는 속성을 [ `TableIntent` ](xref:Xamarin.Forms.TableIntent) 열거형 멤버:

- `Data` – 데이터 항목을 표시할 때 사용 합니다. 사실은 [ListView](~/xamarin-forms/user-interface/listview/index.md) 데이터 목록을 스크롤 하는 것에 대 한 더 나은 옵션 일 수 있습니다.
- `Form` –는 TableView 형태로 역할 때 사용 합니다.
- `Menu` – 선택 항목의 메뉴를 표시할 때 사용 합니다.
- `Settings` – 구성 설정의 목록을 표시 하는 경우 사용 합니다.

합니다 [ `TableIntent` ](xref:Xamarin.Forms.TableIntent) 영향을 줄 수를 선택 하면 값 방법을 [ `TableView` ](xref:Xamarin.Forms.TableView) 각 플랫폼에 표시 됩니다. 가 차이 선택을 취소 하지 하는 경우에이 가장 좋은 방법은 선택 하는 `TableIntent` 가장 일치 하는 테이블을 사용 하는 방식입니다.

각각에 대해 텍스트의 색을 표시 하는 또한 [ `TableSection` ](xref:Xamarin.Forms.TableSection) 설정 하 여 변경할 수 있습니다 합니다 `TextColor` 속성을 [ `Color` ](xref:Xamarin.Forms.Color).

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>기본 제공 셀

Xamarin.Forms를 수집 하 고 정보를 표시 하는 기본 제공 셀 수반 됩니다. 하지만 [ `ListView` ](xref:Xamarin.Forms.ListView) 하 고 [ `TableView` ](xref:Xamarin.Forms.TableView) 같은 셀을 모두 사용 하 여 [ `SwitchCell` ](xref:Xamarin.Forms.SwitchCell) 하 고 [ `EntryCell` ](xref:Xamarin.Forms.EntryCell)에 가장 적합 한 `TableView` 시나리오입니다.

참조 [ListView 셀 모양](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) 에 대 한 자세한 설명은 [TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) 하 고 [ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell)합니다.

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) 컨트롤이 표시 하 고 캡처하는 데는 설정/해제 하거나 `true` / `false` 상태입니다. 다음 속성을 정의 합니다.

- `Text` – 스위치 옆에 표시할 텍스트입니다.
- `On` – 스위치를 on 또는 off 표시 되는지 여부입니다.
- `OnColor` – 합니다 [ `Color` ](xref:Xamarin.Forms.Color) on 위치에 있을 때 스위치입니다.

바인딩할 수 있는 이러한 모든 속성입니다.

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) 노출 된 `OnChanged` 이벤트, 셀의 상태 변경 내용에 응답할 수 있습니다.

![](tableview-images/switch-cell.png "SwitchCell 예제")

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell

[`EntryCell`](xref:Xamarin.Forms.EntryCell) 사용자를 편집할 수 있는 텍스트 데이터를 표시 해야 하는 경우 유용 합니다. 다음 속성을 정의 합니다.

- `Keyboard` – 키보드를 편집 하는 동안 표시 합니다. 숫자 값, 전자 메일, 전화 번호 등과 같은 항목에 대 한 옵션이 있습니다. [API 문서를 참조 하세요](xref:Xamarin.Forms.Keyboard)합니다.
- `Label` – 텍스트 입력 필드의 오른쪽에 표시할 레이블 텍스트입니다.
- `LabelColor` – 레이블 텍스트의 색입니다.
- `Placeholder` -Null 이거나 비워 둘 경우 입력 필드에 표시할 텍스트입니다. 이 텍스트는 텍스트 입력을 시작할 때 사라집니다.
- `Text` – 텍스트 입력 필드입니다.
- `HorizontalTextAlignment` 텍스트의 가로 맞춤 –입니다. 왼쪽 또는 오른쪽 가운데 정렬 수 있습니다. [API 문서를 참조 하세요](xref:Xamarin.Forms.TextAlignment)합니다.

[`EntryCell`](xref:Xamarin.Forms.EntryCell) 노출 된 `Completed` 사용자 텍스트를 편집 하는 동안 키보드에 '완료' 단추를 누를 때 발생 하는 경우.

![](tableview-images/entry-cell.png "EntryCell 예제")

<a name="Custom_Cells" />

## <a name="custom-cells"></a>사용자 지정 셀

기본 제공 셀에는 충분 하지 않으면 하는 경우 사용자 지정 셀 있고 앱에 대 한 적합 한 방식으로 데이터를에서 캡처를 사용할 수 있습니다. 예를 들어 다음 사용자가 이미지의 불투명도 선택할 수 있도록 하려면 슬라이더를 제공 하는 것이 좋습니다.

모든 사용자 지정 셀에서 파생 되어야 합니다 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell)를 사용 하 여 유형의 모든 기본 제공 셀 동일한 기본 클래스입니다.

사용자 지정 셀의 예로 다음과 같습니다.

![](tableview-images/custom-cell.png "사용자 지정 셀 예제")

다음 예제에서는 만드는 데 XAML 합니다 [ `TableView` ](xref:Xamarin.Forms.TableView) 위 스크린샷의:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DemoTableView.TablePage"
             Title="TableView">
      <TableView Intent="Settings">
          <TableRoot>
              <TableSection Title="Getting Started">
                  <ViewCell>
                      <StackLayout Orientation="Horizontal">
                          <Image Source="bulb.png" />
                          <Label Text="left"
                                 TextColor="#f35e20" />
                          <Label Text="right"
                                 HorizontalOptions="EndAndExpand"
                                 TextColor="#503026" />
                      </StackLayout>
                  </ViewCell>
              </TableSection>
          </TableRoot>
      </TableView>
</ContentPage>
```

해당 하는 C# 코드가입니다.

```csharp
var table = new TableView();
table.Intent = TableIntent.Settings;
var layout = new StackLayout() { Orientation = StackOrientation.Horizontal };
layout.Children.Add (new Image() { Source = "bulb.png"});
layout.Children.Add (new Label()
{
    Text = "left",
    TextColor = Color.FromHex("#f35e20"),
    VerticalOptions = LayoutOptions.Center
});
layout.Children.Add (new Label ()
{
    Text = "right",
    TextColor = Color.FromHex ("#503026"),
    VerticalOptions = LayoutOptions.Center,
    HorizontalOptions = LayoutOptions.EndAndExpand
});
table.Root = new TableRoot ()
{
    new TableSection("Getting Started")
    {
        new ViewCell() {View = layout}
    }
};
Content = table;
```

아래에 루트 요소를 [ `TableView` ](xref:Xamarin.Forms.TableView) 은 [ `TableRoot` ](xref:Xamarin.Forms.TableRoot), 되며를 [ `TableSection` ](xref:Xamarin.Forms.TableSection) 바로 아래는 `TableRoot`합니다. 합니다 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) 바로 아래에 정의 된 합니다 `TableSection`, 및 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 모든 레이아웃 여기 사용 될 수 있지만 사용자 지정 셀의 레이아웃을 관리 하는 데 사용 됩니다.

> [!NOTE]
> 와 달리 [ `ListView` ](xref:Xamarin.Forms.ListView), [ `TableView` ](xref:Xamarin.Forms.TableView) 는 사용자 지정 (또는 모든) 하지 않아도 셀에 정의 된는 `ItemTemplate`합니다.

## <a name="row-height"></a>행 높이

합니다 [ `TableView` ](xref:Xamarin.Forms.TableView) 클래스에 셀의 행 높이 변경 하려면 사용할 수 있는 두 가지 속성이 있습니다.

- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight) – 각 행의 높이 `int`합니다.
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) – 행 경우 다양 한 높이 `true`합니다. 이 속성을 설정 하는 경우 사용자에 게 유의 `true`, 행 높이가 자동으로 계산 및 Xamarin.Forms에서 적용 됩니다.

경우에 셀 콘텐츠의 높이 [ `TableView` ](xref:Xamarin.Forms.TableView) 변경 되 면 행 높이가 Android 및 유니버설 Windows 플랫폼 (UWP)에서 암시적으로 업데이트 됩니다. 그러나 iOS에서이 해야 해야만 설정 하 여 업데이트 합니다 [ `HasUnevenRows` ](xref:Xamarin.Forms.TableView.HasUnevenRows) 속성을 `true` 호출 하 여 합니다 [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize) 메서드.

다음 XAML 예제를 [ `TableView` ](xref:Xamarin.Forms.TableView) 포함 하는 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell):

```xaml
<ContentPage ...>
    <TableView ...
               HasUnevenRows="true">
        <TableRoot>
            ...
            <TableSection ...>
                ...
                <ViewCell x:Name="_viewCell"
                          Tapped="OnViewCellTapped">
                    <Grid Margin="15,0">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="Auto" />
                        </Grid.RowDefinitions>
                        <Label Text="Tap this cell." />
                        <Label x:Name="_target"
                               Grid.Row="1"
                               Text="The cell has changed size."
                               IsVisible="false" />
                    </Grid>
                </ViewCell>
            </TableSection>
        </TableRoot>
    </TableView>
</ContentPage>
```

경우는 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) 를 탭 할는 `OnViewCellTapped` 이벤트 처리기가 실행:

```csharp
void OnViewCellTapped(object sender, EventArgs e)
{
    _target.IsVisible = !_target.IsVisible;
    _viewCell.ForceUpdateSize();
}
```

합니다 `OnViewCellTapped` 이벤트 처리기 표시 하거나 숨깁니다. 두 번째 [ `Label` ](xref:Xamarin.Forms.Label) 에 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell)를 명시적으로 호출 하 여 셀의 크기를 업데이트 하 고는 [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize) 메서드.

다음 스크린샷에서 시 탭 되 고 이전 셀을 표시 합니다.

![](tableview-images/cell-beforeresize.png "크기가 조정 되기 전의 ViewCell")

다음 스크린샷에서 시 탭 되 후 셀을 표시 합니다.

![](tableview-images/cell-afterresize.png "ViewCell 크기가 조정 된 후")

> [!IMPORTANT]
> 이 기능은 초과 사용 되 면 강력한 성능 저하가 발생할 수가 있습니다.

## <a name="related-links"></a>관련 링크

- [TableView (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)
