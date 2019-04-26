---
title: Xamarin.iOS에서 자동 크기 조정 행 높이
description: 이 문서에서는 해당 높이에 따라 콘텐츠에 따라 변경 테이블 뷰 행 Xamarin.iOS 앱에 추가 하는 방법을 설명 합니다. IOS 디자이너의에서 셀 레이아웃 및 사용 하도록 설정 하면 자동 크기 조정 높이 설명 합니다.
ms.prod: xamarin
ms.assetid: CE45A385-D40A-482A-90A0-E8382C2BFFB9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: e4446abc73817eb0672cd10a69ff6f738de0c1e1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61029116"
---
# <a name="auto-sizing-row-height-in-xamarinios"></a>Xamarin.iOS에서 자동 크기 조정 행 높이

Apple iOS 8 부터는 테이블 뷰를 작성 하는 기능 추가 (`UITableView`) 자동으로 증가 하 고 자동 레이아웃 및 크기 클래스 제약 조건을 사용 하 여 해당 콘텐츠의 크기를 기준으로 지정된 된 행의 높이 축소 수는 있습니다.

iOS 11을 자동으로 확장 하는 행에 대 한 기능을 추가 했습니다. 머리글, 바닥글 및 셀 이제 자동으로 크기를 조정할 수 해당 콘텐츠를 기반으로 합니다. 그러나 iOS 디자이너 Interface Builder에서에서 테이블을 만들거나 하는 행 높이 수정 하는 경우 수동으로 해야 하는 경우 사용할 셀 크기 조정 자체이 가이드에 설명 된 대로.

## <a name="cell-layout-in-the-ios-designer"></a>IOS 디자이너의에서 셀 레이아웃

열린 행의 자동 크기 조정에 대 한 iOS 디자이너에 하려는 테이블 뷰에 대 한 스토리 보드의 셀을 선택 *프로토타입* 셀의 레이아웃을 디자인 하 고 있습니다. 예를 들어:

[![](autosizing-row-height-images/table01.png "셀의 프로토타입 디자인")](autosizing-row-height-images/table01.png#lightbox)

프로토타입에 각 요소에 대해 제약 조건을 테이블 뷰 회전 또는 다른 iOS 장치의 화면 크기에 대 한 크기를 조정할 때 올바른 위치에 요소를 계속 추가 합니다. 예를 들어, 고정 합니다 `Title` 셀의 위쪽, 왼쪽 및 오른쪽 *콘텐츠 뷰에*:

[![](autosizing-row-height-images/table02.png "위쪽, 왼쪽 및 오른쪽 셀 콘텐츠 뷰의 제목 고정")](autosizing-row-height-images/table02.png#lightbox)

예제 테이블, 작은 경우 `Label` (아래는 `Title`) 축소 하 고 행 높이 늘리거나 증가할 수 있는 필드입니다. 이 위해서는 왼쪽, 오른쪽, 위쪽 및 아래쪽 레이블의 고정 하려면 다음과 같은 제약 조건을 추가 합니다.

[![](autosizing-row-height-images/table03.png "이러한 제약 조건은 왼쪽, 오른쪽, 위쪽 및 아래쪽 레이블의 고정 하려면")](autosizing-row-height-images/table03.png#lightbox)

이제는 완전히 셀에 있는 요소에 제한에서는 요소를 늘이는 명확 하 게 해야 합니다. 이 위해 설정 합니다 **콘텐츠 Hugging 우선 순위** 하 고 **콘텐츠 압축 저항 우선 순위** 에서 필요에 따라는 **레이아웃** Properties Pad의 섹션:

[![](autosizing-row-height-images/table03a.png "Properties Pad의 레이아웃 섹션")](autosizing-row-height-images/table03a.png#lightbox)

할 확장 하려는 요소를 설정를 **낮은** Hugging 우선 순위 값 및 **낮은** 압축 저항 우선 순위 값입니다.

셀 프로토타입을 선택 하 고 고유한 제공 해야이 어 **식별자**:

[![](autosizing-row-height-images/table04.png "셀 프로토타입 고유 식별자 제공")](autosizing-row-height-images/table04.png#lightbox)

이 예의 경우 `GrowCell`합니다. 이 값에서는 테이블을 구성 하는 경우 나중에 사용 합니다.

> [!IMPORTANT]
> 테이블 셀 형식이 둘 이상 있는 경우 (**프로토타입**)를 각 형식에는 자체 고유 해야 `Identifier` 자동 행 크기 조정 작업에 대 한 합니다.

이 셀 프로토타입의 각 요소에 대해 할당 한 **이름** 를 노출 하 C# 코드. 예를 들어:

[![](autosizing-row-height-images/table05.png "이를 노출 하에 이름을 할당 C# 코드")](autosizing-row-height-images/table05.png#lightbox)

그런 다음에 대 한 사용자 지정 클래스를 추가 합니다 `UITableViewController`는 `UITableView` 및 `UITableCell` (프로토타입). 예를 들어: 

[![](autosizing-row-height-images/table06.png "사용자 지정 클래스는 UITableViewController는 UITableView 및는 UITableCell 추가")](autosizing-row-height-images/table06.png#lightbox)

마지막으로 필요한 모든 콘텐츠는 레이블에 표시 됩니다로 설정 합니다 **줄** 속성을 `0`:

[![](autosizing-row-height-images/table06.png "줄 속성이 0으로 설정")](autosizing-row-height-images/table06a.png#lightbox)

UI 정의 사용 하 여 자동 행 높이 크기 조정을 사용 하도록 설정 하는 코드를 추가 해 보겠습니다.

## <a name="enabling-auto-resizing-height"></a>높이 자동 크기 조정 사용

이 테이블 보기의 데이터 원본 (`UITableViewDatasource`) 또는 원본 (`UITableViewSource`)를 사용 해야 하는 셀을 큐에서 제거 했습니다 하는 경우는 `Identifier` 디자이너에서 정의한 합니다. 예를 들어:

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

기본적으로 테이블 뷰 행 높이 자동 크기 조정에 대 한 설정 됩니다. 이 위해 합니다 `RowHeight` 속성에 설정할 `UITableView.AutomaticDimension`합니다. 설정 해야 합니다 `EstimatedRowHeight` 속성에서 우리의 `UITableViewController`합니다. 예를 들어:

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

이 견적을 정확 하 게 없는 테이블 보기의 각 행의 평균 높이의 대략적인 예상 값만 합니다.

이 코드를 사용 하 여 앱을 실행 하는 경우 각 행 축소를 셀 프로토타입에서 마지막 레이블의 높이에 따라 증가 합니다. 예를 들어:

[![](autosizing-row-height-images/table07.png "실행 하는 예제 테이블")](autosizing-row-height-images/table07.png#lightbox)


## <a name="related-links"></a>관련 링크

- [GrowRowTable (샘플)](https://developer.xamarin.com/samples/monotouch/GrowRowTable/)
