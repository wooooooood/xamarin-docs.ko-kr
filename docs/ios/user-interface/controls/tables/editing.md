---
title: Xamarin.ios를 사용 하 여 테이블 편집
description: 이 문서에서는 Xamarin.ios에서 테이블을 편집 하는 방법을 설명 합니다. 삭제, 편집 모드 및 행 삽입에 대 한 살짝 밀기에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: EC197F25-E865-AFA3-E5CF-B33FAB7744A0
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/22/2017
ms.openlocfilehash: 9960167e2f71531e5ffeaecac94aede5d5ea3340
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768899"
---
# <a name="editing-tables-with-xamarinios"></a>Xamarin.ios를 사용 하 여 테이블 편집

`UITableViewSource` 서브 클래스의 메서드를 재정의 하 여 테이블 편집 기능을 사용할 수 있습니다. 가장 간단한 편집 동작은 단일 메서드 재정의를 사용 하 여 구현할 수 있는 살짝 밀기-삭제 제스처입니다.
편집 모드에서 테이블을 사용 하 여 보다 복잡 한 편집 (행 이동 포함)을 수행할 수 있습니다.

## <a name="swipe-to-delete"></a>삭제 하려면 살짝 밀기

삭제로 살짝 밀기 기능은 사용자가 필요로 하는 iOS에서 자연 스러운 제스처입니다. 

 [![](editing-images/image10.png "삭제할 살짝 밀기의 예")](editing-images/image10.png#lightbox)

셀에 **삭제** 단추를 표시 하는 살짝 밀기 제스처에 영향을 주는 세 가지 메서드 재정의가 있습니다.

- **CommitEditingStyle** –이 메서드가 재정의 되는 경우 테이블 소스에서 검색 하 고 자동으로 살짝 밀기-삭제 제스처를 사용 하도록 설정 합니다. 메서드의 구현은에서를 호출 `DeleteRows` `UITableView` 하 여 셀이 사라지게 하 고 모델에서 기본 데이터 (예: 배열, 사전 또는 데이터베이스)도 제거 해야 합니다. 
- **Caneditrow** – CommitEditingStyle를 재정의 하면 모든 행을 편집할 수 있는 것으로 간주 됩니다. 이 메서드를 구현 하 고 특정 행 또는 모든 행에 대해 false를 반환 하는 경우에는 해당 셀에서 삭제 후 제스처를 사용할 수 없습니다. 
- **TitleForDeleteConfirmation** – 필요에 따라 **삭제** 단추의 텍스트를 지정 합니다. 이 메서드가 구현 되지 않은 경우 단추 텍스트가 "삭제" 됩니다. 

이러한 메서드는 `TableSource` 클래스에서 구현 됩니다.

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

이 예제에서는 `UITableViewSource` 컬렉션에서 항목을 추가 및 `List<TableItem>` 삭제 하는 것을 지원 하기 때문에 문자열 배열 대신를 데이터 소스로 사용 하도록 업데이트 되었습니다.

## <a name="edit-mode"></a>편집 모드

테이블이 편집 모드에 있는 경우 사용자는 각 행에 빨간색 ' stop ' 위젯을 표시 하며,이 위젯은 작업 시 삭제 단추를 표시 합니다. 테이블에는 행을 끌어서 순서를 변경할 수 있음을 나타내는 ' 핸들 ' 아이콘도 표시 됩니다.
**Tableeditmode** 샘플은 다음과 같이 이러한 기능을 구현 합니다.

 [![](editing-images/image11.png "TableEditMode 샘플은 다음과 같이 이러한 기능을 구현 합니다.")](editing-images/image11.png#lightbox)

에 `UITableViewSource` 는 테이블의 편집 모드 동작에 영향을 주는 여러 가지 방법이 있습니다.

- **Caneditrow** – 각 행을 편집할 수 있는지 여부를 지정 합니다. 편집 모드에 있는 동안에는 살짝 delete와 삭제를 모두 방지 하려면 false를 반환 합니다. 
- **CanMoveRow** – 이동 하지 않도록 하려면 true를 반환 하 고, 이동 하지 않으려면 false를 반환 합니다. 
- **EditingStyleForRow** – 테이블이 편집 모드에 있는 경우이 메서드의 반환 값은 셀에 빨간색 삭제 아이콘이 표시 되는지 아니면 녹색 추가 아이콘이 표시 되는지를 결정 합니다. 행 `UITableViewCellEditingStyle.None` 을 편집할 수 없으면를 반환 합니다. 
- **MoveRow** – 테이블에 표시 되는 데이터와 일치 하도록 기본 데이터 구조를 수정할 수 있도록 행이 이동 될 때 호출 됩니다. 

를 사용 하 여 특정 행의 동작을 변경 하려는 경우를 제외 하 고는 `indexPath` 전체 테이블에 대 한 반환 값을 하드 코딩 하는 경우를 제외 하 고 처음 세 가지 메서드에 대 한 구현은 비교적 바로 전달 됩니다.

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

`MoveRow` 구현은 새 순서와 일치 하도록 기본 데이터 구조를 변경 해야 하기 때문에 좀 더 복잡 합니다. 데이터는로 `List` 구현 되기 때문에 아래 코드는 이전 위치에 있는 데이터 항목을 삭제 하 고 새 위치에 삽입 합니다. 데이터가 ' order ' 열 (예:)을 사용 하 여 SQLite 데이터베이스 테이블에 저장 된 경우이 메서드는 대신 해당 열의 숫자를 다시 정렬 하기 위해 일부 SQL 작업을 수행 해야 합니다.

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

마지막으로, 테이블을 편집 모드로 가져오려면 **편집** 단추가 다음과 같이 호출 `SetEditing` 해야 합니다.

```csharp
table.SetEditing (true, true);
```

사용자의 편집이 완료 되 면 **완료** 단추를 클릭 하 여 편집 모드를 꺼야 합니다.

```csharp
table.SetEditing (false, true);
```

## <a name="row-insertion-editing-style"></a>행 삽입 편집 스타일

테이블 내에서 행 삽입은 일반적이 지 않은 사용자 인터페이스입니다. 표준 iOS 앱의 주요 예는 **연락처 편집** 화면입니다. 이 스크린샷에서는 행 삽입 기능의 작동 방식을 보여 줍니다. 편집 모드에서 데이터에 추가 행을 삽입 하는 추가 행이 있습니다. 편집이 완료 되 면 임시 **(새 추가)** 행이 제거 됩니다.

 [![](editing-images/image12.png "편집이 완료 되 면 임시 새 행 추가가 제거 됩니다.")](editing-images/image12.png#lightbox)

에 `UITableViewSource` 는 테이블의 편집 모드 동작에 영향을 주는 여러 가지 방법이 있습니다. 이러한 메서드는 예제 코드에서 다음과 같이 구현 되었습니다.

- **EditingStyleForRow** – 데이터 `UITableViewCellEditingStyle.Delete` 를 포함 하는 행에 대해를 `UITableViewCellEditingStyle.Insert` 반환 하 고, 마지막 행 (삽입 단추로 동작 하도록 특별히 추가 됨)에 대해를 반환 합니다. 
- **CustomizeMoveTarget** – 사용자가 셀을 이동 하는 동안이 선택적 메서드의 반환 값은 해당 위치 선택을 재정의할 수 있습니다. 즉, 특정 위치에 있는 셀을 ' 삭제 ' 하는 것을 방지할 수 있습니다 .이 예는 **(새 추가)** 행 뒤에 행이 이동 되지 않도록 방지 하는 예입니다. 
- **CanMoveRow** – 이동 하지 않도록 하려면 true를 반환 하 고, 이동 하지 않으려면 false를 반환 합니다. 예제에서 마지막 행은 삽입 단추로만 서버를 사용 하기 때문에 ' 핸들 ' 이동이 숨겨집니다. 

또한 ' 삽입 ' 행을 추가 하는 두 개의 사용자 지정 메서드를 추가 하 고 더 이상 필요 하지 않은 경우 다시 제거 합니다. 이러한 작업은 **편집** 및 **완료** 단추에서 호출 됩니다.

- **WillBeginTableEditing** – **편집** 단추가 있는 경우를 호출 `SetEditing` 하 여 테이블을 편집 모드로 전환 합니다. 그러면 테이블의 끝에 있는 **(새 추가)** 행을 ' 삽입 단추 ' 역할을 수행 하는 WillBeginTableEditing 메서드가 트리거됩니다. 
- **DidFinishTableEditing** – 완료 단추 `SetEditing` 를 다시 호출 하 여 편집 모드를 해제 합니다. 예제 코드는 편집이 더 이상 필요 하지 않을 때 테이블에서 **(새 추가)** 행을 제거 합니다. 

이러한 메서드 재정의는 샘플 파일 **Tableeditmodeadd/Code/Tableource. cs**에서 구현 됩니다.

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

이러한 두 가지 사용자 지정 메서드를 사용 하 여 테이블의 편집 모드를 사용 하거나 사용 하지 않을 때 **(새 추가)** 행을 추가 하 고 제거 합니다.

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

마지막으로,이 **코드는 편집** 및 **완료** 단추를 인스턴스화하고 편집 모드를 사용 하거나 사용 하지 않도록 설정 하는 람다를 사용 합니다.

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

이 행 삽입 UI 패턴은 자주 사용 되지 않지만 `UITableView.BeginUpdates` 및 `EndUpdates` 메서드를 사용 하 여 테이블의 셀 삽입 또는 제거에 애니메이션 효과를 줄 수도 있습니다. 이러한 메서드를 사용 하는 `RowsInSection` 규칙은 `BeginUpdates` 및 `EndUpdates` 호출 사이에서 반환 되는 값의 차이가 `InsertRows` 및 `DeleteRows` 메서드와 함께 추가/삭제 된 셀의 순 수와 일치 해야 한다는 것입니다. 기본 데이터 원본이 테이블 뷰의 삽입/삭제와 일치 하도록 변경 되지 않은 경우 오류가 발생 합니다.

## <a name="related-links"></a>관련 링크

- [WorkingWithTables (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
