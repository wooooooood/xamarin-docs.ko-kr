---
title: TableView
description: 스크롤 메뉴, 설정 및 입력된 양식 처럼를 제공 합니다.
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: dc55f3fe70450c71b639cf33166720fc27d45a10
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="tableview"></a>TableView

[TableView](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) 스크롤할 수 있는 데이터 또는 선택 목록 표시에 대 한 뷰는 행이 같은 서식 파일을 공유 하지 않는 경우. 와 달리 [ListView](~/xamarin-forms/user-interface/listview/index.md), TableView의 개념이 없는 `ItemsSource`이므로 수동으로 항목 자식으로 추가 해야 합니다.

이 가이드는 다음 섹션으로 구성 됩니다.

- **[사용 사례](#Use_Cases)**  &ndash; TableView ListView 또는 사용자 지정 보기 대신 사용 하는 경우.
- **[TableView 구조](#TableView_Structure)**  &ndash; 는 TableView 내 필요 보기는 계층 구조입니다.
- **[TableView 모양](#TableView_Appearance)**  &ndash; TableView에 대 한 사용자 지정 옵션.
- **[기본 제공 셀](#Built-In_Cells)**  &ndash; 을 포함 한 기본 제공 된 셀 옵션 [EntryCell](#EntryCell) 및 [SwitchCell](#SwitchCell)합니다.
- **[사용자 지정 셀](#Custom_Cells)**  &ndash; 사용자 고유의 사용자 지정 셀을 확인 하는 방법입니다.

![](tableview-images/tableview-all-sml.png "TableView 예제")

<a name="Use_Cases" />

## <a name="use-cases"></a>사용 사례

TableView는 다음 경우에 유용 합니다.

- 설정의 목록 표시
- 폼에서 데이터를 수집 또는
- 행의 행 (예: 숫자, 백분율 및 이미지)를 다르게 표시 되는 데이터를 표시 합니다.

TableView 스크롤 및 레이아웃 위의 시나리오에 대 한 일반적인 요구가 매력적인 섹션에는 행을 처리 합니다. `TableView` 컨트롤 각 플랫폼의 사용 가능한 각 플랫폼에 대 한 기본 모양을 만드는 경우 해당 뷰를 원본으로 사용 합니다.

<a name="TableView_Structure" />

## <a name="tableview-structure"></a>TableView 구조

요소는 `TableView` 섹션으로 구성 됩니다. 루트에는 `TableView` 는 `TableRoot`, 하나 이상의 부모 변수인 `TableSections`:

```csharp
Content = new TableView {
    Root = new TableRoot {
        new TableSection...
    },
    Intent = TableIntent.Settings
};
```

각 `TableSection` 제목 및 하나 이상의 ViewCells로 구성 됩니다. 여기 보면는 `TableSection`의 `Title` 속성이로 설정 *"링"* 생성자에서:

```csharp
var section = new TableSection ("Ring") { //TableSection constructor takes title as an optional parameter
    new SwitchCell {Text = "New Voice Mail"},
    new SwitchCell {Text = "New Mail", On = true}
};
```

XAML에서 위와 같은 레이아웃을 수행 합니다.

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

<a name="TableView_Appearance" />

## <a name="tableview-appearance"></a>TableView 모양

TableView 노출는 `Intent` 속성은 다음 옵션 중에 열거형입니다.

- **데이터** &ndash; 데이터 항목을 표시할 때 사용 합니다. [ListView](~/xamarin-forms/user-interface/listview/index.md) 스크롤할 수 있도록 데이터 목록이 더 나은 옵션 일 수 있습니다.
- **양식** &ndash; 는 TableView 역할 폼을 때 사용 합니다.
- **메뉴** &ndash; 선택 항목의 메뉴를 표시할 때 사용 합니다.
- **설정** &ndash; 구성 설정의 목록을 표시할 때 사용 합니다.

`TableIntent` 영향을 줄 수를 선택 하는 방법을 `TableView` 각 플랫폼에 나타납니다. 선택 하는 가장 좋습니다 경우에 차이 지우지는 `TableIntent` 가장 근접 하 게 일치 하는 테이블을 사용 하려는 방법입니다.

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>기본 제공 셀

Xamarin.Forms를 수집 하 고 정보를 표시 하기 위한 기본 제공 된 셀과 함께 제공 됩니다. ListView 및 TableView를 사용할 수 있지만 동일한 셀을 모두 다음은 TableView 시나리오에 가장 적합 합니다.

- **SwitchCell** &ndash; 텍스트 레이블 함께 true/false 상태를 캡처 및 제시 합니다.
- **EntryCell** &ndash; 텍스트 캡처 및 제시 합니다.

참조 [ListView 셀 모양](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) 에 대 한 자세한 설명은 [TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) 및 [ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell)합니다.

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell
[`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/) 컨트롤이 표시 하 고 캡처하는 데는 설정/해제 하거나 `true` / `false` 상태입니다.

SwitchCells 한 줄의 텍스트를 편집 및 켜기/끄기 속성 있어야 합니다. 바인딩할 수 있는 이러한 두 속성이 있습니다.

- `Text` &ndash; 스위치 옆에 표시할 텍스트입니다.
- `On` &ndash; on 또는 off 스위치는 표시 여부입니다.

`SwitchCell` 노출는 `OnChanged` 이벤트를 셀의 상태 변경에 대응할 수 있습니다.

![](tableview-images/switch-cell.png "SwitchCell 예제")

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell
[`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) 사용자가 편집할 수 있는 텍스트 데이터를 표시 해야 하는 경우 유용 합니다. `EntryCell`s는 사용자 지정할 수 있는 다음과 같은 속성을 제공 합니다.

- `Keyboard` &ndash; 편집 하는 동안 표시할 키보드입니다. 숫자 값, 전자 메일, 전화 번호 등과 같은 항목에 대 한 옵션이 있습니다. [API 문서를 참조 하십시오.](http://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/)합니다.
- `Label` &ndash; 텍스트 입력 필드의 오른쪽에 표시할 레이블 텍스트입니다.
- `LabelColor` &ndash; 레이블 텍스트의 색입니다.
- `Placeholder` &ndash; 가 null 이거나 비어 있는 경우에 입력 필드에 표시할 텍스트입니다. 이 텍스트는 텍스트 입력을 시작할 때 사라집니다.
- `Text` &ndash; 입력 필드에 대 한 텍스트입니다.
- `HorizontalTextAlignment` &ndash; 텍스트의 가로 맞춤입니다. 왼쪽 또는 오른쪽 가운데 정렬 수 있습니다. [API 문서를 참조 하십시오.](http://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/)합니다.

`EntryCell` 노출 된 `Completed` 컨텍스트 '완료' 바로 가기 키 텍스트를 편집 하는 동안 발생 하는 이벤트입니다.

![](tableview-images/entry-cell.png "EntryCell 예제")

<a name="Custom_Cells" />

## <a name="custom-cells"></a>사용자 지정 셀
기본 제공 된 셀에는 충분 하지 않으면 때 사용자 지정 셀을 나타내고 캡처 데이터를 앱에 대 한 맞는 방식으로 사용할 수 있습니다. 예를 들어 다음 사용자는 이미지의 불투명도 선택할 수 있도록 하기 위한 슬라이더를 제공 하는 것이 좋습니다.

모든 사용자 지정 셀에서 파생 되어야 [ `ViewCell` ](http://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), 기본 제공 된 셀의 모든 사용을 입력 하는 동일한 기본 클래스입니다.

다음은 사용자 지정 셀의 예입니다.

![](tableview-images/custom-cell.png "사용자 지정 셀 예제")

### <a name="xaml"></a>XAML
XAML 위의 레이아웃을 만들 수는 다음과 같습니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="DemoTableView.TablePage" Title="TableView">
    <ContentPage.Content>
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
    </ContentPage.Content>
</ContentPage>

```

위의 XAML를 많이 수행 합니다. 분리 해 보겠습니다.

- 아래에 있는 루트 요소는 `TableView` 는 `TableRoot`합니다.
- 한 `TableSection` 바로 아래에서 `TableRoot`합니다.
- `ViewCell` 섹션 바로 아래에 정의 됩니다. 와 달리 `ListView`, `TableView` 해당 사용자 지정 (또는 모든) 필요 하지 않는 셀에 정의 된는 `ItemTemplate`합니다.
- StackLayout 사용자 지정 셀의 레이아웃을 관리 하는 데 사용 됩니다. 모든 레이아웃 여기에서 사용할 수 없습니다.

### <a name="cnum"></a>C&num;

때문에 `TableView` 작동 하는 정적 데이터 또는 수동으로 변경 되지 않는 데이터와 없는 항목 템플릿의 개념입니다. 대신, 사용자 지정 셀 수동으로 작성 및 테이블에 배치 합니다. 상속 하는 방법의 사용자 지정 만들기 하는 셀 참고 `ViewCell`, 다음에 추가 `TableView` 같은 기본 제공 된 셀은도 지원 합니다.
위의 레이아웃을 수행 하는 c# 코드는 다음과 같습니다.

```csharp
var table = new TableView();
table.Intent = TableIntent.Settings;
var layout = new StackLayout() { Orientation = StackOrientation.Horizontal };
layout.Children.Add (new Image() {Source = "bulb.png"});
layout.Children.Add (new Label() {
    Text = "left",
    TextColor = Color.FromHex("#f35e20"),
    VerticalOptions = LayoutOptions.Center
});
layout.Children.Add (new Label () {
    Text = "right",
    TextColor = Color.FromHex ("#503026"),
    VerticalOptions = LayoutOptions.Center,
    HorizontalOptions = LayoutOptions.EndAndExpand
});
table.Root = new TableRoot () {
    new TableSection("Getting Started") {
        new ViewCell() {View = layout}
    }
};

Content = table;
```

C# 위에서 수행 하는 작업을 너무 많이 있습니다. 분리 해 보겠습니다.

- 아래에 있는 루트 요소는 `TableView` 는 `TableRoot`합니다.
- 한 `TableSection` 바로 아래에서 `TableRoot`합니다.
- `ViewCell` 섹션 바로 아래에 정의 됩니다. 와 달리 `ListView`, `TableView` 해당 사용자 지정 (또는 모든) 필요 하지 않는 셀에 정의 된는 `ItemTemplate`합니다.
- StackLayout 사용자 지정 셀의 레이아웃을 관리 하는 데 사용 됩니다. 모든 레이아웃 여기에서 사용할 수 없습니다.

사용자 지정 셀에 대 한 클래스 정의 되지 않은 참고 합니다. 대신,는 `ViewCell`의 특정 인스턴스에 대 한 보기 속성은 `ViewCell`합니다.



## <a name="related-links"></a>관련 링크

- [TableView (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)
