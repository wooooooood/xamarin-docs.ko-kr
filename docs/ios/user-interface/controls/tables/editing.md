---
title: Xamarin.iOS 사용 하 여 테이블 편집
description: 이 문서에서는 Xamarin.iOS에서 테이블을 편집 하는 방법을 설명 합니다. 삭제, 편집 모드 및 행을 삽입 하는 안쪽으로 살짝 밀어를 설명 합니다.
ms.prod: xamarin
ms.assetid: EC197F25-E865-AFA3-E5CF-B33FAB7744A0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 1267de341a88130c18254f414d2fbb1c42595a0c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104822"
---
# <a name="editing-tables-with-xamarinios"></a>Xamarin.iOS 사용 하 여 테이블 편집

메서드를 재정의 하 여 테이블 편집 기능을 사용할 수는 `UITableViewSource` 하위 클래스입니다. 가장 간단한 편집 동작은 단일 메서드 재정의 사용 하 여 구현할 수 있는 삭제 하려면 살짝 밀기 제스처입니다.
더 복잡 한 (포함 하 여 행 이동)를 편집 하는 편집 모드에서 테이블을 사용 하 여 수행할 수 있습니다.

## <a name="swipe-to-delete"></a>삭제할 살짝 밀기

기능을 삭제 하려면 안쪽으로 살짝 밀어 ios 사용자가 필요로 하는 자연 스러운 제스처입니다. 

 [![](editing-images/image10.png "삭제할 안쪽으로 살짝 밀어의 예")](editing-images/image10.png#lightbox)

위한 살짝 밀기 제스처가 표시에 영향을 주는 세 가지 메서드 재정의 가지는 **삭제** 셀의 단추:

-   **CommitEditingStyle** –이 메서드가 재정의 되 고 자동으로 삭제 하려면 살짝 밀기 제스처를 사용 하는 경우 테이블 원본에서 검색 합니다. 메서드 구현을 호출 해야 `DeleteRows` 에 `UITableView` 사라지고도 (예를 들어, 배열, 사전 또는 데이터베이스) 모델에서 기본 데이터를 제거 하려면 셀을 합니다. 
-   **CanEditRow** – 경우 CommitEditingStyle 재정의 되는 모든 행 편집 가능한 것으로 간주 됩니다. 이 메서드는 구현 및 (일부 특정 행 또는 모든 행에 대해) false를 반환 하는 경우 다음 삭제 하려면 살짝 밀기 제스처를 사용할 수 없습니다는 셀의. 
-   **TitleForDeleteConfirmation** -필요에 따라 텍스트를 지정 합니다 **삭제** 단추입니다. 이 메서드가 구현 되지 않은 경우 단추 텍스트를 "삭제" 됩니다. 


이러한 메서드는 구현 된 `TableSource` 다음과 같이 클래스:

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

이 예제는 `UITableViewSource` 를 사용 하도록 업데이트 되었습니다를 `List<TableItem>` (대신 문자열 배열)를 지 원하는 데이터 원본 추가 및 삭제 항목 컬렉션에서.


## <a name="edit-mode"></a>편집 모드

테이블 편집 모드일 때 사용자에 게 작업 하는 경우 삭제 단추를 표시 하는 각 행에 빨간색 'stop' 위젯을 표시 합니다. 테이블에는 순서를 변경 하려면 행을 끌 수를 나타내는 '처리' 아이콘이 표시 됩니다.
합니다 **TableEditMode** 샘플에서는 표시 된 것 처럼 이러한 기능을 구현 합니다.

 [![](editing-images/image11.png "표시 된 것 처럼 TableEditMode이 샘플에서는 이러한 기능을 구현")](editing-images/image11.png#lightbox)

에 다른 방법의 여러 가지 `UITableViewSource` 테이블의 편집 모드 동작에 영향을 주는:

-   **CanEditRow** – 각 행을 편집할 수 있습니다. 삭제 하려면 살짝 밀기와 편집 모드에서 삭제를 방지 하려면 false를 반환 합니다. 
-   **CanMoveRow** true를 반환 하는 – 이동 하지 않으려면 false 또는 핸들' 이동'을 사용 하도록 설정 합니다. 
-   **EditingStyleForRow** – 테이블 편집 모드에 있으면이 메서드에서 반환 값은 빨간색 삭제 아이콘을 표시 하는 셀 또는 녹색 아이콘을 추가 하는지 여부를 결정 합니다. 반환 `UITableViewCellEditingStyle.None` 행 편집 가능 해야 합니다. 
-   **MoveRow** – 테이블에 표시 되는 데이터를 일치 시킬 기본 데이터 구조를 수정할 수 있도록 행을 이동할 때 호출 됩니다. 


처음 세 가지 방법에 대 한 구현은 비교적 간단 – 사용 하려는 경우가 아니면는 `indexPath` 특정 행의 동작을 변경 하려면만 하드 코드 된 반환 값 전체 테이블에 대 한 합니다.

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

`MoveRow` 새 순서에 맞게 기본 데이터 구조를 변경 해야 하므로 구현은 좀 더 복잡 합니다. 데이터는으로 구현 되므로 `List` 아래 코드는 이전 위치에 있는 데이터 항목을 삭제 하 고 새 위치에 삽입 합니다. 데이터 (예) '주문' 열을 사용 하 여 SQLite 데이터베이스 테이블에 저장 된 경우이 메서드 대신 해당 열에서 숫자의 순서를 변경 하려면 몇 가지 SQL 작업을 수행 해야 합니다.

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

마지막으로 가져오려면 테이블을 편집 모드의 **편집할** 단추를 호출 해야 합니다. `SetEditing` 같이

```csharp
table.SetEditing (true, true);
```

사용자가 완료 하는 경우 및 편집 합니다 **수행** 단추는 편집 모드를 해제:

```csharp
table.SetEditing (false, true);
```


## <a name="row-insertion-editing-style"></a>행 삽입 편집 스타일

테이블 내에서 행을 삽입은 일반적이 지 않은 사용자 인터페이스-표준 iOS 앱에서 주요 예제는 **연락처 편집** 화면. 행 삽입 기능이 작동 하는 방법을 보여 주는이 스크린샷-편집에서 모드는 추가 행입니다 (클릭) 하는 경우 데이터에 추가 행을 삽입 합니다. 편집이 완료 되었을 때, 임시 **(새로 추가)** 행이 제거 됩니다.

 [![](editing-images/image12.png "임시 새로 추가 편집이 완료 되 면 행이 제거")](editing-images/image12.png#lightbox)

에 다른 방법의 여러 가지 `UITableViewSource` 테이블의 편집 모드 동작에 영향을 주는 합니다. 이러한 방법의 예제 코드에 다음과 같이 구현 되었습니다.

-   **EditingStyleForRow** – 반환 `UITableViewCellEditingStyle.Delete` 데이터 및 반환에 포함 된 행에 대 한 `UITableViewCellEditingStyle.Insert` 마지막 행 (삽입 단추 처럼 동작에 맞게 추가 될 예정임)에 대 한 합니다. 
-   **CustomizeMoveTarget** – 움직이고 셀이 선택적 메서드의 반환 값의 다양 한 위치를 재정의할 수 있습니다. 즉, '삭제'를 방지할 수 있습니다 – 행 후 이동 되는 것을 방지 하는이 예제와 같이 특정 위치에 있는 셀의 **(새로 추가)** 행입니다. 
-   **CanMoveRow** true를 반환 하는 – 이동 하지 않으려면 false 또는 핸들' 이동'을 사용 하도록 설정 합니다. 예제에서는 마지막 행에는 핸들' 이동' 위해서 서버로 삽입 단추도 숨겨져 있습니다. 


'Insert' 행을 추가 하 고 제거 하는 것 다시 더 이상 필요한 경우에 두 개의 사용자 지정 메서드를 추가할 수도 없습니다. 호출 되는 **편집** 하 고 **수행** 단추:

-   **WillBeginTableEditing** – 합니다 **편집** 단추는 호출을 작업 `SetEditing` 테이블을 편집 모드에에서 둡니다. 이때 표시 되므로 여기서 WillBeginTableEditing 메서드는 **(새로 추가)** '삽입 단추' 역할을 테이블의 끝에 행. 
-   **DidFinishTableEditing** – 완료 단추를 연결 하는 경우 `SetEditing` 편집 모드를 해제 하기 위해 다시 호출 됩니다. 예제 코드를 제거 합니다 **(새로 추가)** 편집할 때 테이블의 행이 필요 하지 않습니다. 


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

이러한 두 사용자 지정 메서드 추가 및 제거를 사용 하는 **(새로 추가)** 테이블에는 편집 모드의 경우 행이 활성화 되거나 비활성화 합니다.

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

마지막으로,이 코드를 인스턴스화하는 **편집** 하 고 **수행** 단추를 사용 하도록 설정 하거나 연결 된 경우 편집 모드를 사용 하지 않도록 설정 하는 람다를 사용 하 여:

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

하지만이 행 삽입 UI 패턴을 사용할 수도 있습니다 매우 자주 사용 하지는 `UITableView.BeginUpdates` 고 `EndUpdates` 삽입 또는 모든 테이블의 셀 제거 애니메이션을 적용 하는 방법입니다. 해당 방법 사용에 대 한 규칙은 값의 차이 반환한 `RowsInSection` 간에 `BeginUpdates` 및 `EndUpdates` 호출에 사용 하 여 추가/삭제 하는 셀의 net 수와 일치 해야 합니다는 `InsertRows` 및 `DeleteRows` 메서드. 기본 데이터 원본의 테이블 보기에서 삽입/삭제와 일치 하도록 변경 되지 않고 오류가 발생 합니다.


## <a name="related-links"></a>관련 링크

- [WorkingWithTables (샘플)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
