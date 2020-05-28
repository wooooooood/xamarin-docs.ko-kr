---
title: Xamarin.FormsTableView
description: 이 문서에서는 TableView 클래스를 사용 하 여 Xamarin.Forms 응용 프로그램에서 스크롤 메뉴, 설정 및 입력 폼을 표시 하는 방법을 설명 합니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8f3fd8d84906844b578e71cb0774932561e0d507
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136229"
---
# <a name="xamarinforms-tableview"></a>Xamarin.FormsTableView

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview)

[`TableView`](xref:Xamarin.Forms.TableView)는 스크롤 가능한 데이터 목록이 나 동일한 템플릿을 공유 하지 않는 행이 있는 선택 항목을 표시 하기 위한 뷰입니다. [ListView](~/xamarin-forms/user-interface/listview/index.md)와 달리에 `TableView` 는의 개념이 없으므로 항목을 `ItemsSource` 수동으로 자식으로 추가 해야 합니다.

![TableView 예제](tableview-images/tableview-all-sml.png)

<a name="Use_Cases" />

## <a name="use-cases"></a>사용 사례

[`TableView`](xref:Xamarin.Forms.TableView)는 다음과 같은 경우에 유용 합니다.

- 설정 목록 표시
- 폼에서 데이터 수집 또는
- 행 마다 다르게 표시 되는 데이터 (예: 숫자, 비율 및 이미지)를 표시 합니다.

[`TableView`](xref:Xamarin.Forms.TableView)는 위의 시나리오에 대 한 일반적인 요구 사항이 매력적인 섹션에서 행 스크롤 및 레이아웃을 처리 합니다. `TableView`컨트롤은 사용 가능한 경우 각 플랫폼의 기본 동일 뷰를 사용 하 여 각 플랫폼에 대 한 네이티브 모양을 만듭니다.

<a name="TableView_Structure" />

## <a name="structure"></a>구조체

의 요소 [`TableView`](xref:Xamarin.Forms.TableView) 는 섹션으로 구성 됩니다. 의 루트에는 `TableView` [`TableRoot`](xref:Xamarin.Forms.TableRoot) 하나 이상의 인스턴스에 대 한 부모 인가 [`TableSection`](xref:Xamarin.Forms.TableSection) 있습니다. 각은 [`TableSection`](xref:Xamarin.Forms.TableSection) 제목과 하나 이상의 인스턴스로 구성 됩니다 [`ViewCell`](xref:Xamarin.Forms.ViewCell) .

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

해당하는 C# 코드는 다음과 같습니다.

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

[`TableView`](xref:Xamarin.Forms.TableView)[`Intent`](xref:Xamarin.Forms.TableView.Intent)열거형 멤버로 설정할 수 있는 속성을 노출 합니다 [`TableIntent`](xref:Xamarin.Forms.TableIntent) .

- `Data`– 데이터 항목을 표시할 때 사용 됩니다. [ListView](~/xamarin-forms/user-interface/listview/index.md) 는 데이터 목록을 스크롤 하는 데 더 나은 옵션 일 수 있습니다.
- `Form`– TableView가 폼 역할을 하는 경우 사용 합니다.
- `Menu`– 선택 항목 메뉴를 표시할 때 사용 합니다.
- `Settings`– 구성 설정 목록을 표시 하는 데 사용 됩니다.

[`TableIntent`](xref:Xamarin.Forms.TableIntent)선택한 값은가 [`TableView`](xref:Xamarin.Forms.TableView) 각 플랫폼에 표시 되는 방식에 영향을 줄 수 있습니다. 명확한 차이가 없는 경우에도 `TableIntent` 테이블 사용 방법과 가장 일치 하는를 선택 하는 것이 가장 좋습니다.

또한 속성을로 설정 하 여 각에 대해 표시 되는 텍스트의 색을 [`TableSection`](xref:Xamarin.Forms.TableSection) 변경할 수 있습니다 `TextColor` [`Color`](xref:Xamarin.Forms.Color) .

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>기본 제공 셀

Xamarin.Forms는 정보를 수집 하 고 표시 하기 위한 기본 제공 셀과 함께 제공 됩니다. [`ListView`](xref:Xamarin.Forms.ListView)및는 [`TableView`](xref:Xamarin.Forms.TableView) 동일한 셀을 모두 사용할 수 있지만는 [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) [`EntryCell`](xref:Xamarin.Forms.EntryCell) 시나리오와 가장 관련이 있습니다 `TableView` .

[Textcell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#textcell) 및 [ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#imagecell)에 대 한 자세한 설명은 [ListView 셀 모양](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) 을 참조 하세요.

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)on/off 또는 상태를 표시 하 고 캡처하는 데 사용 하는 컨트롤입니다 `true` / `false` . 이 파일은 다음 속성을 정의합니다.

- `Text`– 스위치 옆에 표시할 텍스트입니다.
- `On`– 스위치를 on 또는 off로 표시할지 여부를 지정 합니다.
- `OnColor`– [`Color`](xref:Xamarin.Forms.Color) on 위치에 있는 경우 스위치의입니다.

이러한 속성은 모두 바인딩할 수 있습니다.

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)는 또한 이벤트를 노출 하 여 `OnChanged` 셀의 상태 변경 내용에 응답할 수 있도록 합니다.

![SwitchCell 예제](tableview-images/switch-cell.png)

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell

[`EntryCell`](xref:Xamarin.Forms.EntryCell)는 사용자가 편집할 수 있는 텍스트 데이터를 표시 해야 하는 경우에 유용 합니다. 이 파일은 다음 속성을 정의합니다.

- `Keyboard`– 편집 하는 동안 표시할 키보드입니다. 숫자 값, 전자 메일, 전화 번호 등과 같은 항목에 대 한 옵션이 있습니다. [API 문서를 참조](xref:Xamarin.Forms.Keyboard)하세요.
- `Label`– 텍스트 입력 필드의 왼쪽에 표시할 레이블 텍스트입니다.
- `LabelColor`– 레이블 텍스트의 색입니다.
- `Placeholder`– Null 이거나 비어 있는 경우 입력 필드에 표시할 텍스트입니다. 텍스트 항목이 시작 되 면이 텍스트가 사라집니다.
- `Text`– Entry 필드의 텍스트입니다.
- `HorizontalTextAlignment`– 텍스트의 가로 맞춤입니다. 값은 가운데, 왼쪽 또는 오른쪽 맞춤입니다. [API 문서를 참조](xref:Xamarin.Forms.TextAlignment)하세요.
- `VerticalTextAlignment`– 텍스트의 세로 맞춤입니다. 값은 `Start` , `Center` 또는 `End` 입니다.

[`EntryCell`](xref:Xamarin.Forms.EntryCell)또한는 `Completed` 텍스트를 편집 하는 동안 사용자가 키보드의 ' 완료 ' 단추를 누를 때 발생 하는 이벤트를 노출 합니다.

![EntryCell 예제](tableview-images/entry-cell.png)

<a name="Custom_Cells" />

## <a name="custom-cells"></a>사용자 지정 셀

기본 제공 셀이 충분 하지 않으면 사용자 지정 셀을 사용 하 여 응용 프로그램에 적합 한 방식으로 데이터를 표시 하 고 캡처할 수 있습니다. 예를 들어 사용자가 이미지의 불투명도를 선택할 수 있도록 슬라이더를 표시할 수 있습니다.

모든 사용자 지정 셀은 [`ViewCell`](xref:Xamarin.Forms.ViewCell) 기본 제공 셀 형식이 모두 사용 하는 것과 동일한 기본 클래스에서 파생 되어야 합니다.

다음은 사용자 지정 셀의 예입니다.

![사용자 지정 셀 예제](tableview-images/custom-cell.png)

다음 예제에서는 위의 스크린샷에를 만드는 데 사용 되는 XAML을 보여 줍니다 [`TableView`](xref:Xamarin.Forms.TableView) .

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

해당하는 C# 코드는 다음과 같습니다.

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

아래에 있는 루트 요소는 이며 [`TableView`](xref:Xamarin.Forms.TableView) [`TableRoot`](xref:Xamarin.Forms.TableRoot) 바로 아래에가 있습니다 [`TableSection`](xref:Xamarin.Forms.TableSection) `TableRoot` . 는에서 직접 정의 되며, 여기서는 레이아웃을 사용할 수 [`ViewCell`](xref:Xamarin.Forms.ViewCell) `TableSection` [`StackLayout`](xref:Xamarin.Forms.StackLayout) 있지만 사용자 지정 셀의 레이아웃을 관리 하는 데 사용 됩니다.

> [!NOTE]
> 와 달리에서는 [`ListView`](xref:Xamarin.Forms.ListView) [`TableView`](xref:Xamarin.Forms.TableView) 사용자 지정 (또는 임의) 셀이에 정의 되어 있지 않아도 됩니다 `ItemTemplate` .

## <a name="row-height"></a>행 높이

클래스에는 [`TableView`](xref:Xamarin.Forms.TableView) 셀의 행 높이를 변경 하는 데 사용할 수 있는 두 개의 속성이 있습니다.

- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight)– 각 행의 높이를로 설정 `int` 합니다.
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows)–로 설정 된 경우 행의 높이가 달라 집니다 `true` . 이 속성을로 설정 하면 `true` 에서 행 높이가 자동으로 계산 되어 적용 됩니다 Xamarin.Forms .

의 셀에 있는 콘텐츠의 높이가 변경 되 면 [`TableView`](xref:Xamarin.Forms.TableView) Android 및 유니버설 Windows 플랫폼 (UWP)에서 행 높이가 암시적으로 업데이트 됩니다. 그러나 iOS에서는 속성을로 설정 하 [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) `true` 고 메서드를 호출 하 여 강제로 업데이트 해야 합니다 [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) .

다음 XAML 예제에서는를 포함 하는을 보여 줍니다 [`TableView`](xref:Xamarin.Forms.TableView) [`ViewCell`](xref:Xamarin.Forms.ViewCell) .

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

를 [`ViewCell`](xref:Xamarin.Forms.ViewCell) 탭 하면 `OnViewCellTapped` 이벤트 처리기가 실행 됩니다.

```csharp
void OnViewCellTapped(object sender, EventArgs e)
{
    _target.IsVisible = !_target.IsVisible;
    _viewCell.ForceUpdateSize();
}
```

`OnViewCellTapped`이벤트 처리기는에서 두 번째를 표시 하거나 숨기고 [`Label`](xref:Xamarin.Forms.Label) [`ViewCell`](xref:Xamarin.Forms.ViewCell) 메서드를 호출 하 여 셀의 크기를 명시적으로 업데이트 합니다 [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) .

다음 스크린샷에는를 탭 하기 전의 셀이 표시 됩니다.

![크기를 조정 하기 전의 ViewCell](tableview-images/cell-beforeresize.png)

다음 스크린샷은 셀을 탭 하 여 표시 합니다.

![크기를 조정한 후의 ViewCell](tableview-images/cell-afterresize.png)

> [!IMPORTANT]
> 이 기능이 남용 경우 성능이 저하 될 가능성이 있습니다.

## <a name="related-links"></a>관련 링크

- [TableView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview)
