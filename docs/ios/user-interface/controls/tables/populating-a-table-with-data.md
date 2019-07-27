---
title: Xamarin.ios의 데이터로 테이블 채우기
description: 이 문서에서는 Xamarin.ios 응용 프로그램의 데이터로 테이블을 채우는 방법에 대해 설명 합니다. UITableViewSource, 셀 재사용, 인덱스 추가, 머리글 및 바닥글에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 6FE64DDF-1029-EB9B-6EEC-1C7DFDFDF3AF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: a7604eebed9c0effdaf7eff62d60666b8727304f
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511917"
---
# <a name="populating-a-table-with-data-in-xamarinios"></a>Xamarin.ios의 데이터로 테이블 채우기

에 행 `UITableView` 을 추가 하려면 하위 클래스를 `UITableViewSource` 구현 하 고 테이블 뷰에서 자신을 채우기 위해 호출 하는 메서드를 재정의 해야 합니다.

이 가이드에서는 다음 내용을 다룹니다.

- UITableViewSource 서브클래싱
- 셀 다시 사용
- 인덱스 추가
- 머리글 및 바닥글 추가


<a name="Subclassing_UITableViewSource" />

## <a name="subclassing-uitableviewsource"></a>UITableViewSource 서브클래싱

서브 클래스는 모든 `UITableView`에 할당 됩니다. `UITableViewSource` 원본 클래스를 쿼리하여 원본 클래스를 쿼리하여 자동으로 렌더링 하는 방법 (예: 필요한 행 수 및 기본값과 다른 경우 각 행의 높이)을 결정 합니다. 가장 중요 한 점은 원본에서 데이터로 채워진 각 셀 뷰를 제공 하는 것입니다.

테이블에 데이터를 표시 하는 데 필요한 두 가지 필수 메서드는 다음과 같습니다.

-   **Rowsinsection** – 테이블에 [`nint`](~/cross-platform/macios/nativetypes.md) 표시 해야 하는 총 데이터 행 수를 반환 합니다.
-   **Getcell** – 메서드에 전달 `UITableCellView` 된 해당 행 인덱스의 데이터로 채워진을 반환 합니다.


BasicTable 샘플 파일 **TableSource.cs** 에는의 `UITableViewSource`가장 간단한 구현이 있습니다. 아래 코드 조각에서 확인할 수 있습니다. 아래 코드 조각에서는 테이블에 표시 되는 문자열의 배열을 허용 하 고 각 문자열이 포함 된 기본 셀 스타일을 반환 합니다.

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

는 `UITableViewSource` 간단한 문자열 배열 (이 예제에서 볼 수 있음)에서 > 또는 기타 컬렉션 < 목록으로 모든 데이터 구조를 사용할 수 있습니다. `UITableViewSource` 메서드를 구현 하면 테이블이 기본 데이터 구조와 격리 됩니다.

이 하위 클래스를 사용 하려면 소스를 생성 하는 문자열 배열을 만든 다음 인스턴스에 `UITableView`할당 합니다.

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

 [![](populating-a-table-with-data-images/image3.png "실행 중인 샘플 테이블")](populating-a-table-with-data-images/image3.png#lightbox)

대부분의 테이블을 사용 하면 사용자가 행을 터치 하 여 선택 하 고 다른 작업 (예: 노래 재생, 연락처 호출 또는 다른 화면 표시)을 수행할 수 있습니다. 이를 위해서는 몇 가지 작업을 수행 해야 합니다. 먼저 사용자가 `RowSelected` 메서드에 다음을 추가 하 여 행을 클릭할 때 메시지를 표시 하는 alertcontroller를 만들어 보겠습니다.

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
뷰 컨트롤러를 매개 변수로 사용 하 고 필드에 저장 하는 UITableViewSource 클래스에 생성자를 추가 합니다.

```csharp
public TableSource (string[] items, HomeScreen owner)
{
    ...
    this.owner = owner;

}
```
참조를 `this` 전달 하기 위해 uitableviewsource 클래스가 만들어지는 ViewDidLoad 메서드를 수정 합니다.

```csharp
table.Source = new TableSource(tableItems, this);
```
마지막으로 `RowSelected` 메서드로 돌아가서 캐시 된 필드에 `PresentViewController` 대해를 호출 합니다.

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    ...
    owner.PresentViewController (okAlertController, true, null);

    ...
}
```


이제 사용자가 행을 터치 할 수 있으며 경고가 표시 됩니다.



 [![](populating-a-table-with-data-images/image4.png "선택한 행 경고")](populating-a-table-with-data-images/image4.png#lightbox)


## <a name="cell-reuse"></a>셀 다시 사용

이 예제에서는 6 개의 항목만 있으므로 셀을 다시 사용할 필요가 없습니다. 그러나 수백 또는 수천 개의 행을 표시 하는 경우 한 번에 화면에 몇 가지만 표시 될 때 수백 또는 `UITableViewCell` 수천 개의 개체를 만들기 위해 메모리가 낭비 될 수 있습니다.

이러한 상황을 방지 하기 위해 셀이 화면에서 사라질 때 해당 뷰는 다시 사용할 수 있도록 큐에 배치 됩니다. 사용자가 스크롤하면 테이블은를 호출 `GetCell` 하 여 표시 되는 새 보기를 요청 합니다. 현재 표시 되지 않는 기존 셀을 다시 사용 하려면 메서드를 `DequeueReusableCell` 호출 하기만 하면 됩니다. 다시 사용할 수 있는 셀이 있으면 반환 됩니다. 그렇지 않으면 null이 반환 되 고 코드에서 새 셀 인스턴스를 만들어야 합니다.

이 예제의 코드 조각은 패턴을 보여 줍니다.

```csharp
// request a recycled cell to save memory
UITableViewCell cell = tableView.DequeueReusableCell (cellIdentifier);
// if there are no cells to reuse, create a new one
if (cell == null)
    cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
```

는 `cellIdentifier` 여러 셀 형식에 대해 별도의 큐를 효율적으로 만듭니다. 이 예제에서는 모든 셀이 동일 하 게 표시 되므로 하드 코드 된 식별자 하나만 사용 됩니다. 서로 다른 형식의 셀이 있는 경우 인스턴스화될 때와 다시 사용 큐에서 요청 될 때 각각 다른 식별자 문자열이 있어야 합니다.

### <a name="cell-reuse-in-ios-6"></a>IOS 6 이상에서 셀 다시 사용

iOS 6 컬렉션 뷰를 사용 하는 것과 비슷한 셀 재사용 패턴을 추가 했습니다. 위에 나와 있는 기존 재사용 패턴은 이전 버전과의 호환성을 위해 계속 지원 되지만이 새 패턴은 셀에서 null 검사를 수행 하지 않아도 되기 때문에 더 좋습니다.

새 패턴을 사용 하면 응용 프로그램은 컨트롤러의 생성자에서 `RegisterClassForCellReuse` 또는 `RegisterNibForCellReuse` 를 호출 하 여 사용할 셀 클래스 또는 xib를 등록 합니다. 그런 다음 `GetCell` 메서드의 셀을 큐에서 제거 하는 경우 셀 클래스 `DequeueReusableCell` 또는 xib 및 인덱스 경로에 대해 등록 한 식별자를 전달 하기만 하면 됩니다.

예를 들어 다음 코드는 UITableViewController에 사용자 지정 셀 클래스를 등록 합니다.

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

MyCell 클래스가 등록 되 면 아래와 같이 추가 null 검사 `GetCell` `UITableViewSource` 없이의 메서드에서 셀의 큐를 제거할 수 있습니다.

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

사용자 지정 셀 클래스와 함께 새로운 재사용 패턴을 사용 하는 경우 아래 코드 조각과 같이 `IntPtr`를 사용 하는 생성자를 구현 해야 합니다. 그렇지 않은 경우에는 셀 클래스의 인스턴스를 생성할 수 없습니다.

```csharp
public class MyCell : UITableViewCell
{
  public MyCell (IntPtr p):base(p)
  {
  }
  ...
}
```

이 문서에 연결 된 **Basictable** 샘플에서 위에 설명 된 항목의 예를 볼 수 있습니다.

<a name="Adding_an_Index" />

## <a name="adding-an-index"></a>인덱스 추가

인덱스를 사용 하면 사용자가 원하는 조건에 따라 인덱싱할 수 있지만 일반적으로 사전순으로 정렬 된 긴 목록을 스크롤할 수 있습니다. **Basictableindex** 예제는 인덱스를 보여 주기 위해 파일에서 훨씬 긴 항목 목록을 로드 합니다. 인덱스의 각 항목은 테이블의 ' section '에 해당 합니다.

 [![](populating-a-table-with-data-images/image5.png "인덱스 표시")](populating-a-table-with-data-images/image5.png#lightbox)

' 섹션 '을 지원 하려면 테이블 뒤의 데이터를 그룹화 해야 하므로 basictableindex 샘플은 각 항목의 첫 `Dictionary<>` 문자를 사전 키로 사용 하 여 문자열 배열에서를 만듭니다.

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

그런 다음를 `Dictionary<>` 사용 하려면 서브클래스에다음메서드를추가하거나수정해야합니다.`UITableViewSource`

-   **Numberofsections** –이 메서드는 선택 사항이 며 기본적으로 테이블은 하나의 섹션을 가정 합니다. 인덱스를 표시 하는 경우이 메서드는 인덱스의 항목 수를 반환 해야 합니다. 예를 들어 인덱스에 영어 알파벳의 모든 문자가 포함 된 경우에는 26입니다.
-   **Rowsinsection** – 지정 된 섹션의 행 수를 반환 합니다.
-   **섹션 Indextitles** – 인덱스를 표시 하는 데 사용 되는 문자열의 배열을 반환 합니다. 샘플 코드는 문자 배열을 반환 합니다.


샘플 파일 **Basictableindex/Tableource. cs** 의 업데이트 된 메서드는 다음과 같습니다.

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

인덱스는 일반적으로 일반 테이블 스타일 에서만 사용 됩니다.


<a name="Adding_Headers_and_Footers" />

## <a name="adding-headers-and-footers"></a>머리글 및 바닥글 추가

머리글 및 바닥글을 사용 하 여 테이블의 행을 시각적으로 그룹화 할 수 있습니다. 필요한 데이터 구조는 인덱스를 추가 하는 것과 매우 유사 `Dictionary<>` 합니다. 즉, 매우 효과적입니다. 이 예에서는 알파벳을 사용 하 여 셀을 그룹화 하는 대신 야채 by 섬의 형식을 그룹화 합니다.
출력은 다음과 같습니다.

 [![](populating-a-table-with-data-images/image6.png "샘플 머리글 및 바닥글")](populating-a-table-with-data-images/image6.png#lightbox)

헤더 및 바닥글을 표시 하려면 `UITableViewSource` 하위 클래스에 다음과 같은 추가 방법이 필요 합니다.

-   **TitleForHeader** – 헤더로 사용할 텍스트를 반환 합니다.
-   **TitleForFooter** – 바닥글로 사용할 텍스트를 반환 합니다.


샘플 파일 **Basictableheaderfooter/Code/Tableource. cs** 의 업데이트 된 메서드는 다음과 같습니다.

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

에서 `GetViewForHeader` `GetViewForFooter` 및 메서드재정의를사용하여뷰개체를사용하여머리글및바닥글의모양을사용자지정할수있습니다.`UITableViewSource`


## <a name="related-links"></a>관련 링크

- [WorkingWithTables (샘플)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
