---
title: "편집"
ms.topic: article
ms.prod: xamarin
ms.assetid: EC197F25-E865-AFA3-E5CF-B33FAB7744A0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 1ea4489cd6f9839d5d32c97aa7ded41e4f15538a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="editing"></a>편집

메서드를 재정의 하 여 테이블 편집 기능을 사용할 수는 `UITableViewSource` 하위 클래스입니다. 가장 간단한 편집 동작에는 단일 메서드를 재정의할으로 구현할 수 있는 삭제 하려면 살짝 밀기입니다.
더 복잡 한 (포함 하 여 행 이동)을 편집 하는 테이블과 함께 편집 모드에 수행할 수 있습니다.

이 가이드는 다음에 표시 됩니다.

- [살짝 삭제 하려면](#Swipe_to_Delete)
- [편집 모드](#Edit_Mode)
- [행 삽입 편집 스타일](#row_insertion_editing_style)

<a name="Swipe_to_delete" />

## <a name="swipe-to-delete"></a>삭제를 살짝

살짝 기능을 삭제 하는 자연 스러운 제스처에 사용자가 예상 하는 iOS입니다. 

 [ ![](editing-images/image10.png "삭제를 살짝의 예")](editing-images/image10.png)

살짝 밀기 표시에 영향을 주는 세 개의 메서드 재정의는 **삭제** 셀의 단추:

-   **CommitEditingStyle** – 테이블 원본 감지 하 여이 메서드를 재정의할 자동으로 삭제 하려면 살짝 제스처를 사용 합니다. 메서드 구현을 호출 해야 `DeleteRows` 에 `UITableView` 와 같은 기본 데이터 (예를 들어 배열, 사전 또는 데이터베이스) 모델에서 제거를 계산할 셀을 발생할 수 있습니다. 
-   **CanEditRow** – 경우 CommitEditingStyle 재정의 되는 모든 행 편집 가능한 것으로 간주 됩니다. 이 메서드가 구현 되 고 (일부 특정 행에 대 한 또는 모든 행에 대해) false를 반환 하는 경우 다음의 통과 / 삭제 제스처가 됩니다 사용할 수 있는 해당 셀에. 
-   **TitleForDeleteConfirmation** – 필요한 경우에 대 한 텍스트를 지정 된 **삭제** 단추입니다. 이 메서드가 구현 되지 않은 경우 단추 텍스트를 "삭제" 됩니다. 


이러한 메서드는 구현에서 `TableSource` 다음과 클래스:

```csharp
public override void CommitEditingStyle (UITableView tableView, UITableViewCellEditingStyle editingStyle, Foundation.NSIndexPath indexPath)
{
    switch (editingStyle) {
        case UITableViewCellEditingStyle.Delete:
            // remove the item from the underlying data source
            tableItems.RemoveAt(indexPath.Row);
            // delete the row from the table
            tableView.DeleteRows (new NSIndexPath[] { indexPath }, UITableViewRowAnimation.Fade);
            break;
        case UITableViewCellEditingStyle.None:
            Console.WriteLine ("CommitEditingStyle:None called");
            break;
    }
}
public override bool CanEditRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you wish to disable editing for a specific indexPath or for all rows
}
public override string TitleForDeleteConfirmation (UITableView tableView, NSIndexPath indexPath)
{   // Optional - default text is 'Delete'
    return "Trash (" + tableItems[indexPath.Row].SubHeading + ")";
}
```

이 예는 `UITableViewSource` 사용 하도록 업데이트 되었습니다는 `List<TableItem>` (대신 문자열 배열) 문서로 지원 하기 때문에 데이터 원본을 추가 및 삭제 항목 컬렉션에서 합니다.

<a name="Edit_mode" />

## <a name="edit-mode"></a>편집 모드

테이블은 편집 모드에 액세스 하는 경우 삭제 단추를 보여 줍니다. 각 행에 빨간색 'stop' 위젯 사용자에 게 표시 합니다. 또한 테이블을 나타내는 행 순서를 변경 하려면 놓을 수 있습니다 '핸들' 아이콘을 표시 합니다.
**TableEditMode** 샘플 표시 된 것 처럼 이러한 기능을 구현 합니다.

 [ ![](editing-images/image11.png "표시 된 것 처럼 TableEditMode이 샘플에서는 이러한 기능을 구현")](editing-images/image11.png)

여러 가지 방법의 여러 가지 `UITableViewSource` 테이블의 편집 모드 동작에 영향을 주는:

-   **CanEditRow** 각 행을 편집할 수 있는지 여부를 – 합니다. 통과 / 삭제와 편집 모드에서 삭제를 방지 하려면 false를 반환 합니다. 
-   **CanMoveRow** true를 반환 하는 – 이동 '핸들' 또는 이동는 false를 사용 하도록 설정 합니다. 
-   **EditingStyleForRow** – 테이블은 편집 모드에이 메서드의 반환 값 셀 빨간색 삭제 아이콘을 표시 하는지를 녹색 추가 아이콘 결정 합니다. 반환 `UITableViewCellEditingStyle.None` 행 편집 가능 해야 합니다. 
-   **MoveRow** – 테이블에 표시 되는 데이터와 일치 하도록 기본 데이터 구조를 수정할 수 없도록 행 이동 하면 호출 됩니다. 


처음 세 가지 방법에 대 한 구현을 상대적으로 – 사용 하려는 경우가 아니면는 `indexPath` 특정 행의 동작을 변경 하려면 정당한 하드 코드의 반환 값을 전체 테이블에 대 한 합니다.

```csharp
public override bool CanEditRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you wish to disable editing for a specific indexPath or for all rows
}
public override bool CanMoveRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you don't allow re-ordering
}
public override UITableViewCellEditingStyle EditingStyleForRow (UITableView tableView, NSIndexPath indexPath)
{
    return UITableViewCellEditingStyle.Delete; // this example doesn't support Insert
}
```

`MoveRow` 구현은 새 순서와 일치 하도록 기본 데이터 구조를 변경 하기 때문에 좀 더 복잡 합니다. 데이터는으로 구현 되므로 `List` 아래 코드는 이전 위치에 있는 데이터 항목을 삭제 하 고 새 위치에 삽입 합니다. 데이터 (예) 'order' 열이 있는 SQLite 데이터베이스 테이블에 저장 된 경우이 메서드 대신 해당 열에에서 있는 숫자의 순서를 변경 하려면 몇 가지 SQL 작업을 수행 해야 합니다.

```csharp
public override void MoveRow (UITableView tableView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
    var item = tableItems[sourceIndexPath.Row];
    var deleteAt = sourceIndexPath.Row;
    var insertAt = destinationIndexPath.Row;
    
    // are we inserting 
    if (destinationIndexPath.Row < sourceIndexPath.Row) {
        // add one to where we delete, because we're increasing the index by inserting
        deleteAt += 1;
    } else {
        // add one to where we insert, because we haven't deleted the original yet
        insertAt += 1;
    }
    tableItems.Insert (insertAt, item);
    tableItems.RemoveAt (deleteAt);
}
```

마지막으로 가져올 테이블 편집 모드에서는 **편집** 단추를 호출 해야 `SetEditing` 다음과 같이

```csharp
table.SetEditing (true, true);
```

사용자가 완료 및 편집는 **수행** 단추를 해제 해야 편집 모드:

```csharp
table.SetEditing (false, true);
```

<a name="Edit_mode_–_row_insertion_editing_style" />

## <a name="row-insertion-editing-style"></a>행 삽입 편집 스타일

테이블 내에서 행을 삽입 한 경우를 흔히 볼 사용자 인터페이스는 – 표준 iOS 앱의 기본 예제는 **편집 연락처** 화면입니다. 이 스크린 샷 행 삽입 기능이 작동 하는 방법을 보여 줍니다.-편집에서 모드는 추가 행 (클릭) 하는 경우 데이터에 추가 행을 삽입 합니다. 편집이 완료 되었을 때, 임시 **(새로 추가)** 행이 제거 됩니다.

 [ ![](editing-images/image12.png "편집이 완료 되 면 새 임시 추가 행이 제거")](editing-images/image12.png)

여러 가지 방법의 여러 가지 `UITableViewSource` 테이블의 편집 모드 동작에 영향을 주는 합니다. 이러한 방법의 예제 코드에 다음과 같이 구현 되었습니다.

-   **EditingStyleForRow** – 반환 `UITableViewCellEditingStyle.Delete` 데이터 및 반환이 포함 된 행에 대 한 `UITableViewCellEditingStyle.Insert` (삽입 단추도 작동 하도록 특별히 추가 될 예정임)과 달리 마지막 행에 대 한 합니다. 
-   **CustomizeMoveTarget** – 사용자가 셀이 선택적 메서드의 반환 값의 다양 한 위치를 재정의할 수를 이동 합니다. 즉, '삭제'에서 금지 – 행 후 이동 되지 않도록 하는이 예제와 같은 특정 위치에 있는 셀의 **(새로 추가)** 행. 
-   **CanMoveRow** true를 반환 하는 – 이동 '핸들' 또는 이동는 false를 사용 하도록 설정 합니다. 예제에서는 마지막 행 핸들이 이동 '' 서버에만 삽입 단추 변수로 지정 된이 있으므로 합니다. 


또한 'insert' 행을 추가 하 고 다음 제거 다시 때 더 이상 필요 하지를 두 개의 사용자 지정 메서드를 추가 합니다. 호출 되는 **편집** 및 **수행** 단추:

-   **WillBeginTableEditing** – 때는 **편집** 단추는 호출 touched `SetEditing` 편집 모드에서 테이블을 배치 하 합니다. 이 경우 표시할 수 있는 WillBeginTableEditing 메서드는 **(새로 추가)** '삽입 단추' 역할을 테이블의 끝 행입니다. 
-   **DidFinishTableEditing** – 취소 단추에 접촉 될 경우 `SetEditing` 편집 모드를 해제 하기 위해 다시 호출 합니다. 예제 코드는 제거는 **(새로 추가)** 편집 하는 경우 테이블의에서 행을는 더 이상 필요 합니다. 


이러한 메서드 재정의 예제 파일에서 구현 됩니다 **TableEditModeAdd/Code/TableSource.cs**:

```csharp
public override UITableViewCellEditingStyle EditingStyleForRow (UITableView tableView, NSIndexPath indexPath)
{
    if (tableView.Editing) {
        if (indexPath.Row == tableView.NumberOfRowsInSection (0) - 1)
            return UITableViewCellEditingStyle.Insert;
        else
            return UITableViewCellEditingStyle.Delete;
    } else // not in editing mode, enable swipe-to-delete for all rows
        return UITableViewCellEditingStyle.Delete;
}
public override NSIndexPath CustomizeMoveTarget (UITableView tableView, NSIndexPath sourceIndexPath, NSIndexPath proposedIndexPath)
{
    var numRows = tableView.NumberOfRowsInSection (0) - 1; // less the (add new) one
    if (proposedIndexPath.Row >= numRows)
        return NSIndexPath.FromRowSection(numRows - 1, 0);
    else
        return proposedIndexPath;
} 
public override bool CanMoveRow (UITableView tableView, NSIndexPath indexPath)
{
    return indexPath.Row < tableView.NumberOfRowsInSection (0) - 1;
}
```

이러한 두 사용자 지정 메서드 추가 및 제거를 사용 하는 **(새로 추가)** 행 테이블의 모드를 편집 하는 경우 사용 하거나 사용 하지 않도록 설정:

```csharp
public void WillBeginTableEditing (UITableView tableView)
{
    tableView.BeginUpdates ();
    // insert the 'ADD NEW' row at the end of table display
    tableView.InsertRows (new NSIndexPath[] { 
            NSIndexPath.FromRowSection (tableView.NumberOfRowsInSection (0), 0) 
        }, UITableViewRowAnimation.Fade);
    // create a new item and add it to our underlying data (it is not intended to be permanent)
    tableItems.Add (new TableItem ("(add new)"));
    tableView.EndUpdates (); // applies the changes
}
public void DidFinishTableEditing (UITableView tableView)
{
    tableView.BeginUpdates ();
    // remove our 'ADD NEW' row from the underlying data
    tableItems.RemoveAt ((int)tableView.NumberOfRowsInSection (0) - 1); // zero based :)
    // remove the row from the table display
    tableView.DeleteRows (new NSIndexPath[] { NSIndexPath.FromRowSection (tableView.NumberOfRowsInSection (0) - 1, 0) }, UITableViewRowAnimation.Fade);
    tableView.EndUpdates (); // applies the changes
}
```

마지막으로,이 코드를 인스턴스화하는 **편집** 및 **수행** 그대로 유지 하는 경우 편집 모드를 사용할지 여부를 지정 하는 람다에서와 단추:

```csharp
done = new UIBarButtonItem(UIBarButtonSystemItem.Done, (s,e)=>{
    table.SetEditing (false, true);
    NavigationItem.RightBarButtonItem = edit;
    tableSource.DidFinishTableEditing(table);
});
edit = new UIBarButtonItem(UIBarButtonSystemItem.Edit, (s,e)=>{
    if (table.Editing)
        table.SetEditing (false, true); // if we've half-swiped a row
    tableSource.WillBeginTableEditing(table);
    table.SetEditing (true, true);
    NavigationItem.LeftBarButtonItem = null;
    NavigationItem.RightBarButtonItem = done;
});
```

사용할 수도 있지만이 행 삽입 UI 패턴, 자주 사용 되지 않습니다는 `UITableView.BeginUpdates` 및 `EndUpdates` 메서드를 삽입 하거나 테이블에 있는 셀의 제거 하는 애니메이션 효과 적용 합니다. 이러한 메서드를 사용 하기 위한 규칙은 값의 차이 반환한 `RowsInSection` 간에 `BeginUpdates` 및 `EndUpdates` 호출으로 추가/삭제 하는 셀의 net 수와 일치 해야 합니다는 `InsertRows` 및 `DeleteRows` 메서드. 기본 데이터 원본의 테이블 뷰의 삽입/삭제와 일치 하도록 변경 되지 않고 오류가 발생 합니다.


## <a name="related-links"></a>관련 링크

- [WorkingWithTables (샘플)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
