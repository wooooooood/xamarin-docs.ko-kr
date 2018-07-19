---
title: Xamarin.Forms TableView
description: 이 문서에서는 Xamarin.Forms TableView 클래스를 사용 하 여 응용 프로그램에서 스크롤 메뉴, 설정 및 입력된 폼을 표시 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 47cd79611cfeaf48c0422772d8f3e75eb57ba771
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996055"
---
# <a name="xamarinforms-tableview"></a>Xamarin.Forms TableView

[TableView](xref:Xamarin.Forms.TableView) 스크롤 가능한 목록이 데이터 나 선택 항목을 표시 하기 위한 뷰입니다 같은 서식 파일을 공유 하지 않는 행이 있습니다. 와 달리 [ListView](~/xamarin-forms/user-interface/listview/index.md), TableView 개념이 없는 `ItemsSource`이므로 수동으로 항목 자식으로 추가 해야 합니다.

이 가이드는 다음 섹션으로 구성 합니다.

- **[사용 사례](#Use_Cases)**  &ndash; TableView ListView 또는 사용자 지정 보기를 대신 사용 하는 경우.
- **[TableView 구조](#TableView_Structure)**  &ndash; 를 TableView 내에서 필요한 뷰 계층 구조입니다.
- **[TableView 모양을](#TableView_Appearance)**  &ndash; TableView에 대 한 사용자 지정 옵션입니다.
- **[기본 제공 셀](#Built-In_Cells)**  &ndash; 비롯 한 기본 제공 셀 옵션을 [EntryCell](#EntryCell) 하 고 [SwitchCell](#SwitchCell)합니다.
- **[사용자 지정 셀](#Custom_Cells)**  &ndash; 사용자 고유의 사용자 지정 셀을 확인 하는 방법입니다.

![](tableview-images/tableview-all-sml.png "TableView 예제")

<a name="Use_Cases" />

## <a name="use-cases"></a>사용 사례

TableView은 다음 경우에 유용 합니다.

- 설정의 목록 표시
- 폼에서 데이터 수집 또는
- 행의 행 (예: 숫자, 백분율 및 이미지)를 다르게 표시 되는 데이터를 표시 합니다.

TableView 스크롤 및 매력적인 섹션에서는 위의 시나리오에 대 한 일반적인 필요한 행 레이아웃을 처리 합니다. `TableView` 컨트롤은 각 플랫폼의 기본 사용 가능한 경우 각 플랫폼에 대 한 기본 모양 만들기 해당 뷰를 사용 합니다.

<a name="TableView_Structure" />

## <a name="tableview-structure"></a>TableView 구조

요소를 `TableView` 섹션으로 구성 됩니다. 루트에는 `TableView` 되는 `TableRoot`, 하나 이상의 부모는 `TableSections`:

```csharp
Content = new TableView {
    Root = new TableRoot {
        new TableSection...
    },
    Intent = TableIntent.Settings
};
```

각 `TableSection` 제목 및 하나 이상의 ViewCells 이루어져 있습니다. 여기에서는 합니다 `TableSection`의 `Title` 속성으로 설정 *"링"* 생성자에서:

```csharp
var section = new TableSection ("Ring") { //TableSection constructor takes title as an optional parameter
    new SwitchCell {Text = "New Voice Mail"},
    new SwitchCell {Text = "New Mail", On = true}
};
```

XAML에서 위와 같은 레이아웃 위해:

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

TableView 노출 된 `Intent` 다음 옵션 중 열거형 속성:

- **데이터** &ndash; 데이터 항목을 표시할 때 사용 합니다. 사실은 [ListView](~/xamarin-forms/user-interface/listview/index.md) 데이터 목록을 스크롤 하는 것에 대 한 더 나은 옵션 일 수 있습니다.
- **폼** &ndash; 는 TableView 형태로 역할 때 사용 합니다.
- **메뉴** &ndash; 선택 메뉴를 표시할 때 사용 합니다.
- **설정을** &ndash; 구성 설정의 목록을 표시 하는 경우 사용 합니다.

합니다 `TableIntent` 영향을 줄 수를 선택 하는 방법을 `TableView` 각 플랫폼에 표시 됩니다. 가 차이 선택을 취소 하지 하는 경우에이 가장 좋은 방법은 선택 하는 `TableIntent` 가장 일치 하는 테이블을 사용 하는 방식입니다.

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>기본 제공 셀

Xamarin.Forms를 수집 하 고 정보를 표시 하는 기본 제공 셀 수반 됩니다. ListView 및 TableView를 사용할 수 있지만 동일한 셀을 모두 다음은 가장 적합 한 TableView 시나리오입니다.

- **SwitchCell** &ndash; 제시 하 고 텍스트 레이블 함께 true/false 상태를 캡처하기 위한 합니다.
- **EntryCell** &ndash; 표시 및 텍스트 캡처에 대 한 합니다.

참조 [ListView 셀 모양](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) 에 대 한 자세한 설명은 [TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) 하 고 [ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell)합니다.

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell
[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) 컨트롤이 표시 하 고 캡처하는 데는 설정/해제 하거나 `true` / `false` 상태입니다.

SwitchCells 한 줄씩 텍스트를 편집 및 속성을 설정/해제 합니다. 이러한 속성을 모두 바인딩할 수 있는 합니다.

- `Text` &ndash; 스위치 옆에 표시할 텍스트입니다.
- `On` &ndash; on 또는 off 스위치는 표시 여부입니다.

`SwitchCell` 노출 된 `OnChanged` 이벤트, 셀의 상태 변경 내용에 응답할 수 있습니다.

![](tableview-images/switch-cell.png "SwitchCell 예제")

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell
[`EntryCell`](xref:Xamarin.Forms.EntryCell) 사용자를 편집할 수 있는 텍스트 데이터를 표시 해야 하는 경우 유용 합니다. `EntryCell`s는 사용자 지정할 수 있는 다음 속성을 제공 합니다.

- `Keyboard` &ndash; 키보드를 편집 하는 동안 표시를 합니다. 숫자 값, 전자 메일, 전화 번호 등과 같은 항목에 대 한 옵션이 있습니다. [API 문서를 참조 하세요](xref:Xamarin.Forms.Keyboard)합니다.
- `Label` &ndash; 텍스트 입력 필드의 오른쪽에 표시할 레이블 텍스트입니다.
- `LabelColor` &ndash; 레이블 텍스트의 색입니다.
- `Placeholder` &ndash; Null 이거나 비워 둘 경우 입력 필드에 표시할 텍스트입니다. 이 텍스트는 텍스트 입력을 시작할 때 사라집니다.
- `Text` &ndash; 텍스트 입력 필드입니다.
- `HorizontalTextAlignment` &ndash; 텍스트의 가로 맞춤입니다. 왼쪽 또는 오른쪽 가운데 정렬 수 있습니다. [API 문서를 참조 하세요](xref:Xamarin.Forms.TextAlignment)합니다.

사실은 `EntryCell` 노출는 `Completed` 사용자에 도달 하면 [완료] 바로 가기 키 텍스트를 편집 하는 동안 발생 하는 이벤트입니다.

![](tableview-images/entry-cell.png "EntryCell 예제")

<a name="Custom_Cells" />

## <a name="custom-cells"></a>사용자 지정 셀
기본 제공 셀에는 충분 하지 않으면 하는 경우 사용자 지정 셀 있고 앱에 대 한 적합 한 방식으로 데이터를에서 캡처를 사용할 수 있습니다. 예를 들어 다음 사용자가 이미지의 불투명도 선택할 수 있도록 하려면 슬라이더를 제공 하는 것이 좋습니다.

모든 사용자 지정 셀에서 파생 되어야 합니다 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell)를 사용 하 여 유형의 모든 기본 제공 셀 동일한 기본 클래스입니다.

사용자 지정 셀의 예로 다음과 같습니다.

![](tableview-images/custom-cell.png "사용자 지정 셀 예제")

### <a name="xaml"></a>XAML
위의 레이아웃을 만들려면 XAML은 다음과 같습니다.

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

위의 XAML 많이 수행 하는 합니다. 알아봅시다.

- 아래에 루트 요소를 `TableView` 되는 `TableRoot`.
- `TableSection` 의 바로 아래는 `TableRoot`합니다.
- `ViewCell` 섹션 바로 아래에 정의 됩니다. 와 달리 `ListView`, `TableView` 는 사용자 지정 (또는 모든) 하지 않아도 셀에 정의 된는 `ItemTemplate`합니다.
- 사용자 지정 셀의 레이아웃을 관리 하는 StackLayout 사용 됩니다. 모든 레이아웃 여기에서 사용할 수 없습니다.

### <a name="cnum"></a>C&num;

때문에 `TableView` 작동 정적인 데이터 또는 수동으로 변경 되지 않는 데이터를 사용 하 여 되지 않은 항목 템플릿의 개념입니다. 대신 사용자 지정 셀 수동으로 작성 및 테이블에 배치 합니다. 상속 되는 사용자 지정 만들기 기술을 셀는 참고 `ViewCell`를 추가 하는 `TableView` 것은 기본 제공 셀, 에서도 지원 됩니다.
위의 레이아웃을 위해 c# 코드는 다음과 같습니다.

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

C# 위에서 수행 하는 많은 합니다. 알아봅시다.

- 아래에 루트 요소를 `TableView` 되는 `TableRoot`.
- `TableSection` 의 바로 아래는 `TableRoot`합니다.
- `ViewCell` 섹션 바로 아래에 정의 됩니다. 와 달리 `ListView`, `TableView` 는 사용자 지정 (또는 모든) 하지 않아도 셀에 정의 된는 `ItemTemplate`합니다.
- 사용자 지정 셀의 레이아웃을 관리 하는 StackLayout 사용 됩니다. 모든 레이아웃 여기에서 사용할 수 없습니다.

사용자 지정 셀에 대 한 클래스 정의 되지 않은 참고 합니다. 대신 합니다 `ViewCell`의 뷰 속성의 특정 인스턴스에 대해 `ViewCell`합니다.



## <a name="related-links"></a>관련 링크

- [TableView (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)
