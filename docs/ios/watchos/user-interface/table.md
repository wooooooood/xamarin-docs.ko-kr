---
title: "Table 컨트롤"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C14126D-9591-4387-A588-3C4521F11C55
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: c55ba4fb90181aaa1aa8ec52e2fcb3e2b2cc76d0
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="table-control"></a>Table 컨트롤

watchOS `WKInterfaceTable` 제어가 해당 iOS 정수 보다 훨씬 간단 하지만 비슷한 역할을 수행 합니다. 터치 이벤트에 응답 하 고 사용자 지정 레이아웃을 사용할 수 있는 행에 나열 된 스크롤 목록을 만듭니다.

![](table-images/table-list-sml.png "조사식 테이블 목록") ![](table-images/table-detail-sml.png)
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="adding-a-table"></a>테이블 추가

끌어서는 **테이블** 를 장면에 대 한 제어 합니다. 기본적으로이 (표시 단일 지정 되지 않은 행 레이아웃) 처럼 보입니다.

[![](table-images/add-table-sml.png "테이블 추가")](table-images/add-table.png#lightbox)

테이블 이름을 부여는 **속성** 패드의 **이름** 상자를 코드에서를 참조할 수 있습니다.

## <a name="add-a-row-controller"></a>행 컨트롤러 추가

테이블은 자동으로 포함 하는 행 컨트롤러를 나타내는 단일 행을 포함 한 **그룹** 기본적으로 제어 합니다.

설정 하는 **클래스** 행 컨트롤러에 대 한에서 행을 선택는 **문서 개요** 에 클래스 이름을 입력 하 고는 **속성** 패드:

[![](table-images/add-row-controller-sml.png "속성 패드에서 클래스 이름 입력")](table-images/add-row-controller.png#lightbox)

행의 컨트롤러에 대 한 클래스 설정 되 고 나면 IDE는 프로젝트에 C# 파일을 해당 만들어집니다. 행에 (예: 레이블) 컨트롤을 끌어과 코드에서를 참조할 수 있도록 이름을 지정 합니다.




## <a name="create-and-populate-rows"></a>만들고 채울 행

`SetNumberOfRows` 각각에 대해 행 컨트롤러 클래스를 만듭니다 행을 사용 하 여는 `Identifier` 올바르다는 것을 선택할 수 있습니다. 사용자 지정 행 컨트롤러를 지정한 경우 `Identifier`, 변경 **기본** 사용한 식별자로 아래 코드 조각에서입니다. `RowController` *모든 행에 대해* 때 만들어집니다 `SetNumberOfRows` 라고 하 고 표시 되는 테이블입니다.

```csharp
myTable.SetNumberOfRows ((nint)rows.Count, "default");
        // loads row controller by identifier
```

> [!IMPORTANT]
> 테이블 행에는 ios에서 조건은 가상화 되지 않습니다. (Apple는 20 미만의 권장 하는 데 사용) 하는 행 수를 제한 하려고 합니다.

각 셀을 채우는 데 필요한 행을 만든 후 (같은 `GetCell` iOS에서 수행 하는). 이 코드 조각은 [WatchTables 예제](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/) 각 행에 레이블을 업데이트

```csharp
for (var i = 0; i < rows.Count; i++) {
    var elementRow = (RowController)myTable.GetRowController (i);
    elementRow.myRowLabel.SetText (rows [i]);
}
```

> [!IMPORTANT]
> 사용 하 여 `SetNumberOfRows` 를 사용 하 여 루핑 및 `GetRowController` 시계에 보내야 하는 전체 테이블입니다. 테이블의 이후 검토를 추가 하거나 제거 해야 할 경우 특정 행 사용 하 여 `InsertRowsAt` 및 `RemoveRowsAt` 성능 향상을 위해 합니다.


## <a name="respond-to-taps"></a>탭에 응답

두 가지 방법으로 행 선택 영역에 응답할 수 있습니다.

- 구현 된 `DidSelectRow` 인터페이스 컨트롤러, 메서드 또는
- 스토리 보드에서 한 segue 만들고 구현 `GetContextForSegue` 다른 장면 열려는 행 선택 하려는 경우.

### <a name="didselectrow"></a>DidSelectRow

행 선택을 프로그래밍 방식으로 처리 하려면 구현 된 `DidSelectRow` 메서드. 새 장면을 열려면 사용 `PushController` 장면의 식별자를 사용 하도록 데이터 컨텍스트를 전달 합니다.

```csharp
public override void DidSelectRow (WKInterfaceTable table, nint rowIndex)
{
    var rowData = rows [(int)rowIndex];
    Console.WriteLine ("Row selected:" + rowData);
    // if selection should open a new scene
    PushController ("secondInterface", rows[(int)rowIndex]);
}
```

### <a name="getcontextforsegue"></a>GetContextForSegue

다른 화면에 테이블 행에서 스토리 보드에서 한 segue 끌어 (키를 누른 채는 **제어** 끄는 동안 키)입니다.
segue를 선택 하 고 식별자에서 지정 해야는 **속성** 패드 (같은 `secondLevel` 아래 예에서).

인터페이스 컨트롤러에서 구현 된 `GetContextForSegue` 메서드와 segue에서 제공 되는 화면에 제공 해야 하는 데이터 컨텍스트를 반환 합니다.

```csharp
public override NSObject GetContextForSegue (string segueIdentifier, WKInterfaceTable table, nint rowIndex)
{
    if (segueIdentifier == "secondLevel") {
        return new NSString (rows[(int)rowIndex]);
    }
    return null;
}
```

이 데이터는에서 대상 스토리 보드 장면에 전달 되는 `Awake` 메서드.

## <a name="multiple-row-types"></a>여러 행 형식

기본적으로 테이블 컨트롤 디자인할 수 있는 단일 행 형식을 있습니다. 더 많은 행 '템플릿'를 사용 하 여 추가 하는 **행** 상자에 **속성** 패드 더 많은 행 컨트롤러를 만들려면:

![](table-images/prototype-rows1.png "프로토타입 행의 수를 설정합니다.")

설정의 **행** 속성을 **3** 에 컨트롤을 끌어다 놓을 수에 대 한 추가 행이 자리 표시자를 만듭니다. 각 행에 대해 설정 된 **클래스** 에 있는 이름이 **속성** 패드 행 컨트롤러 클래스 생성 되도록 합니다.

![](table-images/prototype-rows2.png "디자이너에서 프로토타입 행")

다른 행 형식 사용 하 여 테이블을 채우는 데는 `SetRowTypes` 메서드는 테이블의 각 행에 사용할 행 컨트롤러 종류를 지정할 수 있습니다. 행의 식별자를 사용 하 여 각 행에 대해 사용할 행 컨트롤러를 지정 합니다.

이 배열에 있는 요소의 수는 테이블의 예상 행 수가 일치 해야 합니다.

```csharp
myTable.SetRowTypes (new [] {"type1", "default", "default", "type2", "default"});
```

행 컨트롤러가 여러 개 있는 테이블을 채울 때 UI를 채울 때 원하는 종류를 추적 하기 위해 필요 합니다.

```csharp
for (var i = 0; i < rows.Count; i++) {
    if (i == 0) {
        var elementRow = (Type1RowController)myTable.GetRowController (i);
        // populate UI controls
    } else if (i == 3) {
        var elementRow = (Type2RowController)myTable.GetRowController (i);
        // populate UI controls
    } else {
        var elementRow = (DefaultRowController)myTable.GetRowController (i);
        // populate UI controls
    }
}
```


## <a name="vertical-detail-paging"></a>세로 세부 페이징

watchOS 3 테이블에 대 한 새로운 기능을 도입: 테이블로 이동 하 고 다른 행을 선택 하지 않고 각 행에 관련 된 세부 정보 페이지를 스크롤할 수 있습니다. 위쪽 / 아래쪽 넘기기가 하거나 디지털 왕관을 사용 하 여 세부 정보 화면을 스크롤할 수 있습니다.

![](table-images/table-scroll-sml.png "세로 세부 페이징 예제") ![](table-images/table-detail-sml.png)

> [!IMPORTANT]
> 이 기능은 현재만 사용할 수 있는 인터페이스 작성기 Xcode에서에서 스토리 보드를 편집 하 여 합니다.

이 기능을 사용 하려면 선택은 `WKInterfaceTable` 눈금의 디자인 화면에는 **세로 세부 페이징** 옵션:

![](table-images/vertical-detail-paging-sml.png "세로 세부 페이징 옵션을 선택 하면")

으로 [Apple에 의해 설명](https://developer.apple.com/reference/watchkit/wkinterfacetable#1682023) 테이블 탐색을 사용 해야 segues 페이징 기능이 작동 합니다. 다시 사용 하는 기존 코드를 작성 `PushController` 사용할 segues 대신 합니다.

<a name="add_row_controller" />

## <a name="appendix-row-controller-code-example"></a>부록: 행 컨트롤러 코드 예제

IDE 자동으로 만들어집니다 두 개의 코드 파일이 디자이너에서 행 컨트롤러를 만들 때. 생성 된 파일에서 코드 참조에 대 한 다음과 같습니다.

첫 번째 클래스의 경우에 같은 이름의 **RowController.cs**, 다음과 같이 합니다.

```csharp
using System;
using Foundation;

namespace WatchTablesExtension
{
    public partial class RowController : NSObject
    {
        public RowController ()
        {
        }
    }
}
```

다른 **. designer.cs** 파일은 콘센트 및 하나로이 예제와 같은 디자이너 화면에서 생성 된 작업을 포함 하는 partial 클래스 정의 `WKInterfaceLabel` 제어:

```csharp
using Foundation;
using System;
using System.CodeDom.Compiler;
using UIKit;

namespace WatchTables.OnWatchExtension
{
    [Register ("RowController")]
    partial class RowController
    {
        [Outlet]
        [GeneratedCode ("iOS Designer", "1.0")]
        public WatchKit.WKInterfaceLabel MyLabel { get; set; }

        void ReleaseDesignerOutlets ()
        {
            if (MyLabel != null) {
                MyLabel.Dispose ();
                MyLabel = null;
            }
        }
    }
}
```

여기에 선언 작업과 콘센트 참조할 수 있습니다-코드에서 있지만 **. designer.cs** 파일을 직접 편집 하지 말아야 합니다.



## <a name="related-links"></a>관련 링크

- [WatchTables (샘플)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)
- [WatchKitCatalog (샘플)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple의 테이블 문서](https://developer.apple.com/reference/watchkit/wkinterfacetable)
