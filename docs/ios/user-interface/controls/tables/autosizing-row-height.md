---
title: Xamarin.ios의 행 높이 자동 크기 조정
description: 이 문서에서는 콘텐츠에 따라 높이가 다른 행을 Xamarin.ios 앱 테이블 뷰 행에 추가 하는 방법을 설명 합니다. IOS 디자이너의 셀 레이아웃에 대해 설명 하 고 높이 자동 조정 기능을 사용 하도록 설정 합니다.
ms.prod: xamarin
ms.assetid: CE45A385-D40A-482A-90A0-E8382C2BFFB9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 4129370ecb465340a893e0a7f16703a08cc1db72
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021925"
---
# <a name="auto-sizing-row-height-in-xamarinios"></a>Xamarin.ios의 행 높이 자동 크기 조정

IOS 8부터 Apple은 자동 레이아웃, 크기 클래스 및 제약 조건을 사용 하 여 콘텐츠 크기에 따라 지정 된 행의 높이를 자동으로 증가 및 축소할 수 있는 테이블 뷰 (`UITableView`)를 만드는 기능을 추가 했습니다.

iOS 11에는 행이 자동으로 확장 될 수 있는 기능이 추가 되었습니다. 이제 머리글, 바닥글 및 셀의 크기를 내용에 따라 자동으로 조정할 수 있습니다. 그러나 테이블이 iOS 디자이너에서 만들어지거나 Interface Builder 또는 고정 행 높이가 있는 경우이 가이드에 설명 된 대로 자동 크기 조정 셀을 수동으로 사용 하도록 설정 해야 합니다.

## <a name="cell-layout-in-the-ios-designer"></a>IOS 디자이너의 셀 레이아웃

IOS 디자이너에서 행의 자동 크기 조정을 사용할 테이블 뷰에 대 한 스토리 보드를 열고 셀의 *프로토타입을* 선택한 다음 셀의 레이아웃을 디자인 합니다. 예를 들면,

[![](autosizing-row-height-images/table01.png "The Cell's Prototype design")](autosizing-row-height-images/table01.png#lightbox)

프로토타입의 각 요소에 대해 테이블 뷰의 크기가 회전 또는 다른 iOS 장치 화면 크기에 맞게 조정 될 때 올바른 위치에 요소를 유지 하는 제약 조건을 추가 합니다. 예를 들어 `Title`을 셀의 *콘텐츠 보기*의 위쪽, 왼쪽 및 오른쪽에 고정 합니다.

[![](autosizing-row-height-images/table02.png "Pinning the Title to the top, left and right of the Cells Content View")](autosizing-row-height-images/table02.png#lightbox)

예제 테이블의 경우 작은 `Label` (`Title`아래)는 축소 및 확장 하 여 행 높이를 늘리거나 줄일 수 있는 필드입니다. 이 효과를 얻으려면 레이블의 왼쪽, 오른쪽, 위쪽 및 아래쪽을 고정 하는 다음 제약 조건을 추가 합니다.

[![](autosizing-row-height-images/table03.png "These constraints to pin the left, right, top and bottom of the label")](autosizing-row-height-images/table03.png#lightbox)

이제 셀의 요소를 완전히 제한 했으므로 스트레치 되어야 하는 요소를 명확 하 게 지정 해야 합니다. 이렇게 하려면 Properties Pad의 **레이아웃** 섹션에서 필요에 따라 **content Hugging Priority** 및 **content 압축 저항 우선 순위** 를 설정 합니다.

[![](autosizing-row-height-images/table03a.png "The Layout section of the Properties Pad")](autosizing-row-height-images/table03a.png#lightbox)

확장 하려는 요소를 **낮은** Hugging 우선 순위 값으로 설정 하 고 **더 낮은** 압축 저항 우선 순위 값을 설정 합니다.

다음으로 셀 프로토타입을 선택 하 고 고유한 **식별자**를 지정 해야 합니다.

[![](autosizing-row-height-images/table04.png "Giving the Cell Prototype a unique Identifier")](autosizing-row-height-images/table04.png#lightbox)

이 예제의 경우 `GrowCell`합니다. 나중에 테이블을 채울 때이 값을 사용 합니다.

> [!IMPORTANT]
> 테이블에 두 개 이상의 셀 유형 (**프로토타입**)이 포함 된 경우 자동 행 크기 조정을 수행할 수 있도록 각 유형에 고유한 `Identifier` 있는지 확인 해야 합니다.

셀 프로토타입의 각 요소에 대해 코드에 C# 노출할 **이름을** 할당 합니다. 예를 들면,

[![](autosizing-row-height-images/table05.png "Assign a Name to expose it to C# code")](autosizing-row-height-images/table05.png#lightbox)

그런 다음 `UITableViewController`, `UITableView` 및 `UITableCell` (프로토타입)에 대 한 사용자 지정 클래스를 추가 합니다. 예를 들면, 

[![](autosizing-row-height-images/table06.png "Adding a custom class for the UITableViewController, the UITableView and the UITableCell")](autosizing-row-height-images/table06.png#lightbox)

마지막으로 모든 예상 내용이 레이블에 표시 되도록 하려면 **선** 속성을 `0`설정 합니다.

[![](autosizing-row-height-images/table06.png "The Lines property set to 0")](autosizing-row-height-images/table06a.png#lightbox)

UI를 정의한 상태에서 자동 행 높이 크기 조정을 사용 하도록 코드를 추가 해 보겠습니다.

## <a name="enabling-auto-resizing-height"></a>자동 크기 조정 높이 사용

테이블 뷰의 Datasource (`UITableViewDatasource`) 또는 원본 (`UITableViewSource`) 중 하나에서 셀을 큐에서 제거 하는 경우 디자이너에서 정의한 `Identifier`를 사용 해야 합니다. 예를 들면,

```csharp
public string CellID {
    get { return "GrowCell"; }
}
...

public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (CellID, indexPath) as GrowRowTableCell;
    var item = Items [indexPath.Row];

    // Setup
    cell.Image = UIImage.FromFile(item.ImageName);
    cell.Title = item.Title;
    cell.Description = item.Description;

    return cell;
}
```

기본적으로 테이블 뷰는 행 높이 자동 크기 조정에 대해 설정 됩니다. 이를 확인 하려면 `RowHeight` 속성을 `UITableView.AutomaticDimension`으로 설정 해야 합니다. 또한 `UITableViewController`에서 `EstimatedRowHeight` 속성을 설정 해야 합니다. 예를 들면,

```csharp
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Initialize table
    TableView.DataSource = new GrowRowTableDataSource(this);
    TableView.Delegate = new GrowRowTableDelegate (this);
    TableView.RowHeight = UITableView.AutomaticDimension;
    TableView.EstimatedRowHeight = 40f;
    TableView.ReloadData ();
}
```

이 예상 값은 정확 하지 않아도 되며, 테이블 뷰에서 각 행의 평균 높이를 대략적으로 예측 한 것입니다.

이 코드를 사용 하면 앱이 실행 될 때 각 행이 셀 프로토타입에서 마지막 레이블의 높이를 기준으로 축소 되 고 증가 합니다. 예를 들면,

[![](autosizing-row-height-images/table07.png "A sample table run")](autosizing-row-height-images/table07.png#lightbox)

## <a name="related-links"></a>관련 링크

- [GrowRowTable (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/growrowtable)
