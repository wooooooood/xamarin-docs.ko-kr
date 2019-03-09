---
title: Xamarin.iOS의 데이터로 테이블 채우기
description: 이 문서에서는 Xamarin.iOS 응용 프로그램에서 데이터를 사용 하 여 테이블을 채우는 방법을 설명 합니다. UITableViewSource, 셀 재사용 인덱스 및 머리글 및 바닥글 추가 설명 합니다.
ms.prod: xamarin
ms.assetid: 6FE64DDF-1029-EB9B-6EEC-1C7DFDFDF3AF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 5363e3a2210bdcf1efb870ac808ecb37584de6a7
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668922"
---
# <a name="populating-a-table-with-data-in-xamarinios"></a>Xamarin.iOS의 데이터로 테이블 채우기

행을 추가 하는 `UITableView` 구현 해야 할를 `UITableViewSource` 하위 클래스와 자체를 채우는 데 테이블 보기는 메서드를 호출 하는 재정의 합니다.

이 가이드에서 설명 합니다.

- UITableViewSource 서브클래싱
- 셀 재사용
- 인덱스 추가
- 머리글 및 바닥글 추가


<a name="Subclassing_UITableViewSource" />

## <a name="subclassing-uitableviewsource"></a>UITableViewSource 서브클래싱

A `UITableViewSource` 서브 클래스 할당을 모든 `UITableView`합니다. 테이블 보기 쿼리 원본 클래스 (예를 들어, 얼마나 많은 행이 필요 및 기본값과에서 다른 경우 각 행의 높이) 자체를 렌더링 하는 방법을 결정 합니다. 가장 중요 한 원본 데이터로 채워진 각 셀 보기를 제공 합니다.

가지 테이블 데이터를 표시 하는 데 필요한 두 개의 필수 방법이 있습니다.

-   **RowsInSection** 반환 –를 [ `nint` ](https://developer.xamarin.com/guides/cross-platform/macios/nativetypes/) 테이블 표시 해야 하는 데이터 행의 총 개수입니다.
-   **GetCell** – 반환을 `UITableCellView` 메서드에 전달 된 해당 행 인덱스에 대 한 데이터로 채워집니다.


BasicTable 샘플 파일 **TableSource.cs** 의 가장 간단한 가능한 구현이 `UITableViewSource`합니다. 테이블에 표시할 문자열 배열을 허용 하 고 각 문자열을 포함 하는 기본 셀 스타일을 반환 아래 코드 조각에서 확인할 수 있습니다.

```csharp
public class TableSource : UITableViewSource {

        string[] TableItems;
        string CellIdentifier = "TableCell";

        public TableSource (string[] items)
        {
            TableItems = items;
        }

        public override nint RowsInSection (UITableView tableview, nint section)
        {
            return TableItems.Length;
        }

        public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
        {
            UITableViewCell cell = tableView.DequeueReusableCell (CellIdentifier);
            string item = TableItems[indexPath.Row];

            //---- if there are no cells to reuse, create a new one
            if (cell == null)
            { cell = new UITableViewCell (UITableViewCellStyle.Default, CellIdentifier); }

            cell.TextLabel.Text = item;

            return cell;
        }
}
```

`UITableViewSource` 같이 사용할 수 있는 데이터 구조를 간단한 문자열 배열에서 (이 예제의) 목록 <> 또는 다른 컬렉션에 있습니다. 구현의 `UITableViewSource` 메서드 내부 데이터 구조에서 테이블을 격리 합니다.

이 서브 클래스를 사용 하려면 소스를 생성 하 고 인스턴스에 할당 문자열 배열을 만들려면 `UITableView`:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    table = new UITableView(View.Bounds); // defaults to Plain style
    string[] tableItems = new string[] {"Vegetables","Fruits","Flower Buds","Legumes","Bulbs","Tubers"};
    table.Source = new TableSource(tableItems);
    Add (table);
}
```

결과 테이블은 다음과 같습니다.

 [![](populating-a-table-with-data-images/image3.png "실행 하는 예제 테이블")](populating-a-table-with-data-images/image3.png#lightbox)

대부분의 테이블 선택 (예: song을 재생 하는 연락처, 호출 또는 다른 화면을 보여 주는) 일부 다른 작업을 수행 하는 행을 터치 할을 수 있습니다. 이 위해 수행 해야 하는 몇 가지 있습니다. 먼저 사용자를 추가 하 여 행을 클릭 하는 경우 메시지를 표시 하는 AlertController를 만들어 보겠습니다는 `RowSelected` 메서드:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("Row Selected", tableItems[indexPath.Row], UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    ...

    tableView.DeselectRow (indexPath, true);
}
```

다음으로 보기 컨트롤러의 인스턴스를 만듭니다.

```csharp
HomeScreen owner;
```
매개 변수로 뷰 컨트롤러를 사용 하 고 필드에 저장 되는 UITableViewSource 클래스에 생성자를 추가 합니다.

```csharp
public TableSource (string[] items, HomeScreen owner)
{
    ...
    this.owner = owner;

}
```
전달할 UITableViewSource 클래스 만들어지는 ViewDidLoad 메서드를 수정 하 여 `this` 참조:

```csharp
table.Source = new TableSource(tableItems, this);
```
마지막으로 다시 하 `RowSelected` 메서드를 호출 `PresentViewController` 캐시 된 필드:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    ...
    owner.PresentViewController (okAlertController, true, null);

    ...
}
```


이제 사용자 수 행과 경고가 표시 됩니다.



 [![](populating-a-table-with-data-images/image4.png "행 선택한 경고")](populating-a-table-with-data-images/image4.png#lightbox)


## <a name="cell-reuse"></a>셀 재사용

이 예제는 6 개의 항목 이므로 필요 없는 셀 재사용 합니다. 그러나 수백 또는 수천 개의 행을 표시할 때는 것이 수백 또는 수천 개의 만들기 위해 메모리를 낭비 `UITableViewCell` 일부만 한 번에 화면에 맞게 개체입니다.

셀이 뷰는 다시 사용할 수 있도록 큐에 배치 됩니다 화면에서 사라지면이 이런 경우를 방지 하려면. 테이블을 호출 하는 사용자가 스크롤하면 `GetCell` (없습니다 현재 표시 되는) 기존 셀 재사용 – 표시할 새 보기를 요청 하려면 호출을 `DequeueReusableCell` 메서드. 셀이 반환 하는 다시 사용할 수 있도록 사용 가능한 경우 그렇지 않으면 null이 반환 하 고 코드 셀 새 인스턴스를 만들어야 합니다.

이 예제에서 코드이 조각에 패턴을 보여 줍니다.

```csharp
// request a recycled cell to save memory
UITableViewCell cell = tableView.DequeueReusableCell (cellIdentifier);
// if there are no cells to reuse, create a new one
if (cell == null)
    cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
```

`cellIdentifier` 효과적으로 다양 한 셀에 대 한 별도 큐를 만듭니다. 이 예제에서는 동일한 하나만 있으므로 하드 코드 된을 확인 하는 모든 셀 식별자 사용 됩니다. 다양 한 유형의 셀 있다면 각 들은 다른 식별자 문자열로 인스턴스화되는 경우와 다시 사용할 수 있도록 큐에서 요청 된 경우.

### <a name="cell-reuse-in-ios-6"></a>IOS 6 이상에 셀 재사용

iOS 6 컬렉션 뷰를 사용 하 여 하나의 소개와 비슷한 셀 재사용 패턴을 추가 합니다. 하지만 위에 표시 된 기존 재사용 패턴은 현재 지원 이전 버전과 호환성을이 새 패턴은 셀에 null 확인의 필요성 제거 되므로 것이 좋습니다.

새 패턴을 사용 하 여 응용 프로그램 등록 셀 클래스를 호출 하 여 사용할 xib `RegisterClassForCellReuse` 또는 `RegisterNibForCellReuse` 컨트롤러의 생성자에서. 다음 경우 큐에서 제거 된 셀에는 `GetCell` 메서드를 호출 하기만 하면 `DequeueReusableCell` 셀 클래스 또는 xib 인덱스 경로 대 한 등록 식별자를 전달 합니다.

예를 들어, 다음 코드는 UITableViewController에서 사용자 지정 셀 클래스를 등록합니다.

```csharp
public class MyTableViewController : UITableViewController
{
  static NSString MyCellId = new NSString ("MyCellId");

  public MyTableViewController ()
  {
    TableView.RegisterClassForCellReuse (typeof(MyCell), MyCellId);
  }
  ...
}
```

등록 MyCell 클래스를 사용 하 여 셀에서 큐 수를 `GetCell` 메서드는 `UITableViewSource` 필요 없이 추가 null 검사 표시 된 것 처럼 아래:

```csharp
class MyTableSource : UITableViewSource
{
  public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
  {
    // if cell is not available in reuse pool, iOS will create one automatically
    // no need to do null check and create cell manually
    var cell = (MyCell) tableView.DequeueReusableCell (MyCellId, indexPath);

    // do whatever you need to with cell, such as assigning properties, etc.

    return cell;
  }
}
```

주의 사용 하는 생성자를 구현 해야 하는 사용자 지정 셀 클래스를 사용 하 여 새 다시 사용 패턴을 사용할 경우는 `IntPtr`, 아래 코드 조각에서와 같이 고, 그렇지 Objective-c 없게 셀 클래스의 인스턴스를 생성 합니다.

```csharp
public class MyCell : UITableViewCell
{
  public MyCell (IntPtr p):base(p)
  {
  }
  ...
}
```

위에 설명 된 항목의 예제를 확인할 수 있습니다 합니다 **BasicTable** 샘플이이 문서에 연결 합니다.

<a name="Adding_an_Index" />

## <a name="adding-an-index"></a>인덱스 추가

인덱스를 지 원하는 인덱싱할 수 있지만 일반적으로 사전순으로 정렬 된 긴 목록을 스크롤하여 원하는 어떤 기준으로 합니다. 합니다 **BasicTableIndex** 샘플 인덱스를 보여 주기 위해 파일에서 훨씬 더 긴 항목의 목록을 로드 합니다. 인덱스의 각 항목은 테이블의 'section'에 해당합니다.

 [![](populating-a-table-with-data-images/image5.png "인덱스 표시")](populating-a-table-with-data-images/image5.png#lightbox)

테이블 뒤의 데이터를 그룹화 할 해야 하므로 BasicTableIndex 샘플 만듭니다 ' sections'를 지원 하기는 `Dictionary<>` 사전 키로 각 항목의 첫 번째 문자를 사용 하 여 문자열의 배열에서:

```csharp
indexedTableItems = new Dictionary<string, List<string>>();
foreach (var t in items) {
    if (indexedTableItems.ContainsKey (t[0].ToString ())) {
        indexedTableItems[t[0].ToString ()].Add(t);
    } else {
        indexedTableItems.Add (t[0].ToString (), new List<string>() {t});
    }
}
keys = indexedTableItems.Keys.ToArray ();
```

합니다 `UITableViewSource` 서브 클래스에는 해당 다음 메서드를 사용 하도록 수정 또는 추가 해야 합니다 `Dictionary<>` :

-   **NumberOfSections** –이 메서드는 선택 사항, 기본적으로 테이블 한 섹션을 가정 합니다. 인덱스를 표시 하는 경우이 메서드 (예: 26 인덱스 영어 알파벳의 모든 문자를 포함 하는 경우) 인덱스의 항목 수를 반환 해야 합니다.
-   **RowsInSection** – 지정 된 섹션에 있는 행의 수를 반환 합니다.
-   **SectionIndexTitles** –는 인덱스를 표시 하는 데 사용할 문자열의 배열을 반환 합니다. 샘플 코드에는 문자 배열을 반환합니다.


샘플 파일에서 업데이트 된 메서드 **BasicTableIndex/TableSource.cs** 다음과 같습니다.

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return keys.Length;
}
public override nint RowsInSection (UITableView tableview, nint section)
{
    return indexedTableItems[keys[section]].Count;
}
public override string[] SectionIndexTitles (UITableView tableView)
{
    return keys;
}
```

일반적으로 인덱스를 일반 테이블 스타일만 사용 됩니다.


<a name="Adding_Headers_and_Footers" />

## <a name="adding-headers-and-footers"></a>머리글 및 바닥글 추가

머리글 및 바닥글 시각적으로 테이블의 행을 그룹화 할 수 있습니다. 인덱스를 추가 하는 데 필요한 데이터 구조 매우 비슷합니다. – `Dictionary<>` 매우 훌륭하게 작동 합니다. 이 예제는 알파벳을 사용 하 여 셀 그룹을 대신 botanical 유형별 야채 그룹화 됩니다.
출력은 다음과 같습니다.

 [![](populating-a-table-with-data-images/image6.png "샘플 머리글 및 바닥글")](populating-a-table-with-data-images/image6.png#lightbox)

머리글 및 바닥글을 표시 하는 `UITableViewSource` 서브 클래스에는 이러한 추가 메서드 필요:

-   **TitleForHeader** – 헤더로 사용할 텍스트를 반환 합니다.
-   **TitleForFooter** – 바닥글로 사용할 텍스트를 반환 합니다.


샘플 파일에서 업데이트 된 메서드 **BasicTableHeaderFooter/Code/TableSource.cs** 다음과 같습니다.

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    return keys[section];
}
public override string TitleForFooter (UITableView tableView, nint section)
{
    return indexedTableItems[keys[section]].Count + " items";
}
```

머리글과 바닥글을 보기의 모양을 정의할 수 있습니다 개체를 사용 하 여는 `GetViewForHeader` 하 고 `GetViewForFooter` 메서드 재정의에서 `UITableViewSource`합니다.


## <a name="related-links"></a>관련 링크

- [WorkingWithTables (샘플)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
