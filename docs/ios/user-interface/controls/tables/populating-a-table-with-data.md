---
title: "데이터로 테이블 채우기"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6FE64DDF-1029-EB9B-6EEC-1C7DFDFDF3AF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: fb0e4341d8d8ad0719f35c691add9bad1d3f85a8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="populating-a-table-with-data"></a>데이터로 테이블 채우기

행을 추가 하는 `UITableView` 구현 해야는 `UITableViewSource` 하위 클래스와 자체를 채우는 데 테이블 보고 하는 메서드를 호출 하는 재정의 합니다.

이 가이드에서 설명 합니다.

- UITableViewSource 서브클래싱
- 셀 재사용
- 인덱스 추가
- 머리글 및 바닥글 추가


<a name="Subclassing_UITableViewSource" />

## <a name="subclassing-uitableviewsource"></a>UITableViewSource 서브클래싱

A `UITableViewSource` 하위 클래스는를 매 할당 `UITableView`합니다. 테이블 뷰 쿼리 원본 클래스 자체 (예를 들어, 얼마나 많은 행이 필요한 및 기본값과에서 다른 경우에 각 행의 높이) 렌더링 하는 방법을 결정 합니다. 가장 중요 한 점은 원본 데이터로 채워진 각 셀 보기를 제공 합니다.

데이터를 표시 하는 테이블을 만드는 데 필요한 두 개의 필수 가지가 있습니다.

-   **RowsInSection** 반환 –는 [ `nint` ](http://developer.xamarin.com/guides/cross-platform/macios/nativetypes/) 는 테이블에 표시 해야 하는 데이터 행의 총 수입니다.
-   **GetCell** 반환 –는 `UITableCellView` 메서드에 전달 된 해당 행 인덱스에 대 한 데이터로 채워집니다.


BasicTable 샘플 파일 **TableSource.cs** 의 단순한 구현에 `UITableViewSource`합니다. 테이블에 표시 하는 문자열의 배열을 수락 하 고 각 문자열을 포함 하는 기본 셀 스타일을 반환 아래 코드 조각에서 볼 수 있습니다.

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

A `UITableViewSource` 같이 사용할 수 모든 데이터 구조는 단순 문자열 배열에서 (이 예제의) 목록 <> 또는 다른 컬렉션에 있습니다. 구현 `UITableViewSource` 메서드는 기본 데이터 구조에서 테이블을 격리 합니다.

이 하위 클래스를 사용 하려면 소스 생성 다음 인스턴스에 할당 하는 문자열 배열 만들어 `UITableView`:

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

결과 테이블에는 다음과 같습니다.

 [ ![](populating-a-table-with-data-images/image3.png "실행 하는 예제 테이블")](populating-a-table-with-data-images/image3.png)

대부분의 테이블 터치 행을 선택 하 고 (예:는 곡을 재생 또는 호출 하는 연락처, 또는 다른 화면을 보여 주는) 다른 작업을 수행할 수 있습니다. 이를 위해 해야 할 몇 가지 있습니다. 첫째, 사용자는 다음을 추가 하 여 행을 클릭 하는 경우 메시지를 표시 하려면 AlertController 만들기는 `RowSelected` 메서드:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("Row Selected", tableItems[indexPath.Row], UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    ...

    tableView.DeselectRow (indexPath, true);
}
```

다음으로 우리의 뷰-컨트롤러의 인스턴스를 만듭니다.

```csharp
HomeScreen owner;
```
매개 변수로 뷰 컨트롤러를 사용 하는 필드에 저장 UITableViewSource 클래스에 생성자를 추가 합니다.

```csharp
public TableSource (string[] items, HomeScreen owner)
{
    ...
    this.owner = owner;

}
```
전달할 UITableViewSource 클래스를 만들 위치 ViewDidLoad 방법을 수정 된 `this` 참조:

```csharp
table.Source = new TableSource(tableItems, this);
```
마지막으로 다시 로그인 하면 `RowSelected` 메서드를 호출 `PresentViewController` 캐시 된 필드:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    ...
    owner.PresentViewController (okAlertController, true, null);

    ...
}
```


사용자가 행 인접할 수 이제 및 경고가 표시 됩니다.



 [ ![](populating-a-table-with-data-images/image4.png "행 선택한 경고")](populating-a-table-with-data-images/image4.png)


## <a name="cell-reuse"></a>셀 재사용

이 예제는 6 개의 항목 이므로 필요 없는 셀 다시 사용할 수 있도록 합니다. 그러나 수백 또는 수천 개의 행을 표시할 때 것 수백 또는 수천 개의 만들기 위해 메모리를 낭비 `UITableViewCell` 일부만 한 번에 화면에 맞게 개체입니다.

셀 해당 뷰가 다시 사용할 수 있도록 큐에 배치 되 면 화면에서 사라지면 이러한 상황을 피해야 합니다. 스크롤 하는 사용자 테이블 호출 `GetCell` 단순히 호출 새 보기를 표시 하려면-(하지 현재 표시 되는) 기존 셀을 다시 사용을 요청 하는 `DequeueReusableCell` 메서드. 셀이 반환할 다시 사용할 수 있으면 그렇지 않으면 null이 반환 하 고 코드 새 셀 인스턴스를 만들어야 합니다.

이 예제에서 코드이 조각에는 패턴을 보여 줍니다.

```csharp
// request a recycled cell to save memory
UITableViewCell cell = tableView.DequeueReusableCell (cellIdentifier);
// if there are no cells to reuse, create a new one
if (cell == null)
    cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
```

`cellIdentifier` 효과적으로 다양 한 유형의 셀에 대 한 별도 큐를 만듭니다. 이 예에서는 모든 셀 동일한 하나만 있으므로 하드 코드 된 조회 식별자 사용 됩니다. 다양 한 유형의 셀 되었으면 보유 해야 하 각각 서로 다른 식별자 문자열 인스턴스화되는 때와 다시 사용할 수 있도록 큐에서 요청 되는 경우.

### <a name="cell-reuse-in-ios-6"></a>IOS 6 이상에 셀 재사용

iOS 6 추가 셀 재사용 패턴 컬렉션 보기와 하나의 소개와 비슷합니다. 위에 표시 된 기존의 재사용 패턴은 이전 버전과 호환성을이 새 패턴은 셀에 null 검사의 필요성을 제거 하는 대로 것이 좋습니다 여전히 지원 됩니다.

새 패턴으로 응용 프로그램 등록 셀 클래스 또는 호출 하 여 사용할 xib `RegisterClassForCellReuse` 또는 `RegisterNibForCellReuse` 컨트롤러의 생성자에 있습니다. 다음 경우 이상의 셀에는 `GetCell` 메서드를 호출 하기만 하면 `DequeueReusableCell` 셀 클래스 또는 xib 인덱스 경로 대 한 등록 식별자를 전달 합니다.

예를 들어 다음 코드는 UITableViewController에서 사용자 지정 셀 클래스를 등록합니다.

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

에 큐에서 제거 될 수 셀 등록 MyCell 클래스와는 `GetCell` 의 메서드는 `UITableViewSource` 필요 없이 표시 된 것 처럼 추가 null 확인을 아래:

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

주의 사용 하는 생성자를 구현 해야 하는 사용자 지정 셀 클래스와 함께 새 재사용 패턴을 사용할 경우는 `IntPtr`, 아래 코드 조각에 나와 있는 것 처럼 그렇지 않으면 Objective-c 수 없습니다 셀 클래스의 인스턴스를 구성 하려면:

```csharp
public class MyCell : UITableViewCell
{
  public MyCell (IntPtr p):base(p)
  {
  }
  ...
}
```

위에서 설명한 항목의 예제를 확인할 수는 **BasicTable** 샘플이이 문서에 연결 합니다.

<a name="Adding_an_Index" />

## <a name="adding-an-index"></a>인덱스 추가

인덱스를 통해 사용자는 일반적으로 인덱싱할 수 있지만 사전순으로 정렬 하는 긴 목록을 스크롤하여 원하는 어떤 기준으로 합니다. **BasicTableIndex** 샘플 인덱스를 설명 하기 위해 파일에서 훨씬 더 긴 항목 목록이 로드 합니다. 각 항목의 인덱스는 테이블의 'section'에 해당합니다.

 [ ![](populating-a-table-with-data-images/image5.png "인덱스 표시")](populating-a-table-with-data-images/image5.png)

테이블 뒤에 있는 데이터를 그룹화 할 수 있어야 BasicTableIndex 샘플 만듭니다 하므로 ' sections'를 지원 하기 위해 한 `Dictionary<>` 사전 키로 각 항목의 첫 번째 문자를 사용 하 여 문자열 배열에서:

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

`UITableViewSource` 하위 클래스 추가 되거나 사용 하도록 수정 되는 다음 메서드를 다음 필요한는 `Dictionary<>` :

-   **NumberOfSections** –이 메서드는 선택 사항, 기본적으로 테이블에는 한 개의 절이 있다고 가정 합니다. 인덱스를 표시할 때이 메서드는 (예: 26 인덱스 영어 알파벳의 모든 문자를 포함 하는 경우) 인덱스에 항목의 수를 반환 합니다.
-   **RowsInSection** – 지정 된 섹션의 행 수를 반환 합니다.
-   **SectionIndexTitles** – 인덱스를 표시 하는 문자열의 배열을 반환 합니다. 샘플 코드에는 문자 배열을 반환합니다.


샘플 파일의 업데이트 된 메서드에 **BasicTableIndex/TableSource.cs** 다음과 같습니다.

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

머리글 및 바닥글 데 사용할 수는 테이블의 행을 시각적으로 그룹화 합니다. 필요한 데이터 구조는 매우 유사 인덱스를 추가 하는 – `Dictionary<>` 잘 작동 합니다. 셀을 그룹화 하는 알파벳을 사용 하는 대신이 예제에서는 야채 botanical 유형별로 그룹화 됩니다.
출력은 다음과 같습니다.

 [ ![](populating-a-table-with-data-images/image6.png "샘플 머리글 및 바닥글")](populating-a-table-with-data-images/image6.png)

머리글 및 바닥글을 표시 하는 `UITableViewSource` 하위 클래스에는 이러한 추가 메서드가 필요 합니다.

-   **TitleForHeader** – 헤더로 사용할 텍스트를 반환 합니다.
-   **TitleForFooter** – 바닥글으로 사용할 텍스트를 반환 합니다.


샘플 파일의 업데이트 된 메서드에 **BasicTableHeaderFooter/Code/TableSource.cs** 다음과 같습니다.

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

머리글 및 바닥글 보기의 모양을 지정할 수 있습니다 개체를 사용 하는 `GetViewForHeader` 및 `GetViewForFooter` 에 메서드를 재정의 `UITableViewSource`합니다.


## <a name="related-links"></a>관련 링크

- [WorkingWithTables (샘플)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
