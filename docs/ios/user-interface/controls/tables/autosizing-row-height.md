---
title: Xamarin.iOS에서 자동 크기 조정 행 높이
description: 이 문서에 따라 콘텐츠 인 높이 따라 변경 테이블 뷰 행 Xamarin.iOS 앱에 추가 하는 방법을 설명 합니다. IOS 디자이너에에서 셀 레이아웃 및 크기 자동 조정 설정 높이 설명합니다.
ms.prod: xamarin
ms.assetid: CE45A385-D40A-482A-90A0-E8382C2BFFB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 3c6beb112947f5423de200fd5c8957ef28dd48f9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789969"
---
# <a name="auto-sizing-row-height-in-xamarinios"></a>Xamarin.iOS에서 자동 크기 조정 행 높이

Apple iOS 8 이상에서는 테이블 뷰를 작성 하는 기능 추가 (`UITableView`) 자동으로 증가 하 고 자동 레이아웃 및 크기 클래스 제약 조건을 사용 하 여 해당 콘텐츠의 크기에 따라 지정된 된 행의 높이 축소할 수입니다.

iOS 11을 자동으로 확장 하는 행에 대 한 기능을 추가 했습니다. 머리글, 바닥글 및 셀 이제 자동으로 크기를 조정할 수 내용을 기준으로 합니다. 그러나 iOS 디자이너 인터페이스 작성기에서에서 테이블에 만들어지는 경우 또는 행의 높이 고정 하는 경우 수동으로 활성화 해야 셀 크기 조정 자체이 가이드에 설명 된 대로.

## <a name="cell-layout-in-the-ios-designer"></a>IOS 디자이너에에서 셀 레이아웃

열린 행의 자동 크기 조정에 대 한 iOS 디자이너에에서 포함 되도록 테이블 보기에 대 한 스토리 보드 선택 셀의 *프로토타입* 셀의 레이아웃 디자인입니다. 예를 들어:

[![](autosizing-row-height-images/table01.png "셀의 프로토타입 디자인")](autosizing-row-height-images/table01.png#lightbox)

프로토타입에 각 요소에 대 한 제약 조건을 테이블 뷰 회전 또는 다른 iOS 장치 화면 크기에 대 한 크기를 조정할 때 올바른 위치에 요소를 계속 추가 합니다. 예를 들어, 고정는 `Title` 셀의 위쪽, 왼쪽 및 오른쪽 *콘텐츠 보기*:

[![](autosizing-row-height-images/table02.png "고정 제목을 위쪽, 왼쪽 및 오른쪽의 셀 콘텐츠 뷰")](autosizing-row-height-images/table02.png#lightbox)

예제 테이블, 작은 경우 `Label` (아래에서 `Title`) 축소 하 고 행 높이 늘리거나 증가할 수 있는 필드입니다. 이를 위해서는, 왼쪽, 오른쪽, 위쪽 및 아래쪽 레이블의 고정 하려면 다음과 같은 제약 조건을 추가 합니다.

[![](autosizing-row-height-images/table03.png "이러한 제약 조건의 왼쪽, 오른쪽, 위쪽 및 아래쪽 레이블의 고정 하려면")](autosizing-row-height-images/table03.png#lightbox)

셀에서 요소 제약 조건이 완전히 म 했으므로 요소 늘이는 명확 하 게 해야 합니다. 이 위해 설정 된 **콘텐츠 Hugging 우선 순위** 및 **콘텐츠 압축 저항 우선 순위** 에서 필요에 따라는 **레이아웃** 속성 패드의 섹션:

[![](autosizing-row-height-images/table03a.png "속성 패드의 레이아웃 섹션")](autosizing-row-height-images/table03a.png#lightbox)

확장 해야 하는 요소를 설정는 **낮은** Hugging 우선 순위 값 및 **낮은** 압축 저항 우선 순위 값입니다.

다음으로 셀 프로토타입을 선택 하는 고유한 부여 해야 **식별자**:

[![](autosizing-row-height-images/table04.png "셀 프로토타입 고유 식별자 제공")](autosizing-row-height-images/table04.png#lightbox)

이 예제의 경우 `GrowCell`합니다. 에서는 테이블을 구성 하는 경우이 값이 나중에 사용 됩니다.

> [!IMPORTANT]
> 테이블에 둘 이상의 셀 유형 (**프로토타입**), 각 형식에는 자체 고유 하도록 해야 `Identifier` 자동 실행 되도록 행 크기 조정 합니다.

이 셀 프로토타입의 각 요소에 대 한 할당 한 **이름** C# 코드에 노출할 합니다. 예를 들어:

[![](autosizing-row-height-images/table05.png "C# 코드에 노출 하는 이름을 지정 합니다.")](autosizing-row-height-images/table05.png#lightbox)

다음으로 사용자 지정 클래스에 대 한 추가 `UITableViewController`, `UITableView` 및 `UITableCell` (프로토타입). 예를 들어: 

[![](autosizing-row-height-images/table06.png "UITableViewController는 UITableView 및는 UITableCell에 대 한 사용자 지정 클래스 추가")](autosizing-row-height-images/table06.png#lightbox)

마지막으로,을 예상 했던 모든 콘텐츠 우리의 레이블에 표시 되 고 확인 하기 위해 설정 된 **줄** 속성을 `0`:

[![](autosizing-row-height-images/table06.png "줄 속성이 0으로 설정")](autosizing-row-height-images/table06a.png#lightbox)

Ui가 정의 된, 자동 행 높이 크기 조정을 사용 하도록 코드를 추가 해 보겠습니다.

## <a name="enabling-auto-resizing-height"></a>크기 자동 조정 높이 사용 하도록 설정

우리의 테이블 보기의 데이터 원본 (`UITableViewDatasource`) 또는 소스 (`UITableViewSource`)를 사용 해야 하는 셀을에서는 큐에서 제거 하는 경우는 `Identifier` 디자이너에 정의 되어 있습니다. 예를 들어:

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

기본적으로 테이블 뷰 크기 자동 조정 행 높이 대 한 설정 됩니다. 이 확인 하 고 `RowHeight` 로 속성을 설정 해야 `UITableView.AutomaticDimension`합니다. 설정 해야는 `EstimatedRowHeight` 속성에 우리의 `UITableViewController`합니다. 예를 들어:

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

이 예상 정확 하지 않아도 테이블 뷰의 각 행의 평균 높이의 대략적인 예상 값만 합니다.

이 코드 위치에서 앱이 실행 되는 경우 각 행은 축소와 셀 프로토타입의 마지막 레이블의 높이에 따라 증가할 됩니다. 예를 들어:

[![](autosizing-row-height-images/table07.png "실행 하는 예제 테이블")](autosizing-row-height-images/table07.png#lightbox)


## <a name="related-links"></a>관련 링크

- [GrowRowTable (샘플)](https://developer.xamarin.com/samples/monotouch/GrowRowTable/)
