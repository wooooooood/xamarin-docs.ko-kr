---
title: Xamarin의 watchOS Table 컨트롤
description: 이 문서에서는 Xamarin에서 watchOS 테이블 컨트롤을 사용 하는 방법을 설명 합니다. 테이블 추가, 행 컨트롤러 추가, 행 생성 및 채우기, 탭에 응답 등에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 7C14126D-9591-4387-A588-3C4521F11C55
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: 2bed40c3ac2853a5f99c2b487e909164e12e676d
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "70766963"
---
# <a name="watchos-table-controls-in-xamarin"></a>Xamarin의 watchOS Table 컨트롤

WatchOS `WKInterfaceTable` 컨트롤은 해당 iOS에 해당 하는 것 보다 훨씬 더 간단 하지만 유사한 역할을 수행 합니다. 사용자 지정 레이아웃을 가질 수 있고 터치 이벤트에 응답 하는 행의 스크롤 목록을 만듭니다.

![](table-images/table-list-sml.png "조사식 테이블 목록") ![](table-images/table-detail-sml.png)
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="adding-a-table"></a>테이블 추가

**테이블** 컨트롤을 장면으로 끌어 옵니다. 기본적으로 다음과 같이 표시 됩니다 (지정 되지 않은 단일 행 레이아웃 표시).

[![](table-images/add-table-sml.png "Adding a table")](table-images/add-table.png#lightbox)

코드에서 참조할 수 있도록 **속성** 패드의 **이름** 상자에 테이블 이름을 지정 합니다.

## <a name="add-a-row-controller"></a>행 컨트롤러 추가

테이블에는 기본적으로 **그룹** 컨트롤이 포함 된 행 컨트롤러로 표시 되는 단일 행이 자동으로 포함 됩니다.

행 컨트롤러에 대 한 **클래스** 를 설정 하려면 **문서 개요** 에서 행을 선택 하 고 **속성** 패드에 클래스 이름을 입력 합니다.

[![](table-images/add-row-controller-sml.png "Entering a class name in the Properties pad")](table-images/add-row-controller.png#lightbox)

행의 컨트롤러에 대 한 클래스를 설정 하면 IDE에서 프로젝트에 해당 C# 하는 파일을 만듭니다. 컨트롤 (예: 레이블)을 행에 끌어다 놓고 코드에서 참조할 수 있도록 이름을 지정 합니다.

## <a name="create-and-populate-rows"></a>행 만들기 및 채우기

`SetNumberOfRows`은 `Identifier`를 사용 하 여 각 행에 대 한 행 컨트롤러 클래스를 만들어 올바른 항목을 선택 합니다. 행 컨트롤러에 사용자 지정 `Identifier` 지정한 경우 아래 코드 조각의 **기본값** 을 사용한 식별자로 변경 합니다. *모든 행에 대 한* `RowController`는 `SetNumberOfRows`가 호출 되 고 테이블이 표시 될 때 생성 됩니다.

```csharp
myTable.SetNumberOfRows ((nint)rows.Count, "default");
    // loads row controller by identifier
```

> [!IMPORTANT]
> 테이블 행은 iOS에 있는 것 처럼 가상화 되지 않습니다. 행 수를 제한 하십시오 (Apple에서 20 개 미만 권장).

행을 만든 후에는 각 셀을 채워야 합니다 (예: iOS에서 수행 해야 하는 `GetCell`). [WatchTables 예제의](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchtables) 이 코드 조각은 각 행의 레이블을 업데이트 합니다.

```csharp
for (var i = 0; i < rows.Count; i++) {
    var elementRow = (RowController)myTable.GetRowController (i);
    elementRow.myRowLabel.SetText (rows [i]);
}
```

> [!IMPORTANT]
> @No__t_0를 사용한 다음 `GetRowController`를 사용 하 여 반복 하면 전체 테이블이 조사식으로 전송 됩니다. 테이블의 후속 뷰에서 특정 행을 추가 하거나 제거 해야 하는 경우 `InsertRowsAt` 및 `RemoveRowsAt`를 사용 하 여 성능을 향상 시킬 수 있습니다.

## <a name="respond-to-taps"></a>탭에 응답

다음 두 가지 방법으로 행 선택에 응답할 수 있습니다.

- 인터페이스 컨트롤러에서 `DidSelectRow` 메서드를 구현 하거나
- 스토리 보드에 대해 segue을 만들고 행을 선택 하 여 다른 장면을 열도록 하려면 `GetContextForSegue`을 구현 합니다.

### <a name="didselectrow"></a>DidSelectRow

프로그래밍 방식으로 행 선택을 처리 하려면 `DidSelectRow` 메서드를 구현 합니다. 새 장면을 열려면 `PushController`를 사용 하 고 장면 식별자와 사용할 데이터 컨텍스트를 전달 합니다.

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

스토리 보드에서 segue를 테이블 행에서 다른 장면으로 끌어 놓습니다 (끌기 중에는 **컨트롤** 키를 누른 채).
Segue를 선택 하 고 **속성** 패드에서 식별자를 제공 해야 합니다 (예: 아래 예제에서 `secondLevel`).

인터페이스 컨트롤러에서 `GetContextForSegue` 메서드를 구현 하 고 segue에서 제공 하는 장면에 제공 되어야 하는 데이터 컨텍스트를 반환 합니다.

```csharp
public override NSObject GetContextForSegue (string segueIdentifier, WKInterfaceTable table, nint rowIndex)
{
    if (segueIdentifier == "secondLevel") {
        return new NSString (rows[(int)rowIndex]);
    }
    return null;
}
```

이 데이터는 `Awake` 메서드에서 대상 스토리 보드 장면으로 전달 됩니다.

## <a name="multiple-row-types"></a>여러 행 형식

기본적으로 테이블 컨트롤은 디자인할 수 있는 단일 행 형식입니다. ' 템플릿 ' 행을 더 추가 하려면 **속성** 패드에서 **행** 상자를 사용 하 여 더 많은 행 컨트롤러를 만듭니다.

![](table-images/prototype-rows1.png "Setting the number of Prototype rows")

**Rows** 속성을 **3** 으로 설정 하면 컨트롤을 끌어 놓을 수 있는 추가 행 자리 표시 자가 만들어집니다. 각 행에 대해 **속성** 패드에서 **클래스** 이름을 설정 하 여 행 컨트롤러 클래스가 생성 되도록 합니다.

![](table-images/prototype-rows2.png "The prototype rows in the designer")

행 유형이 다른 테이블을 채우려면 `SetRowTypes` 메서드를 사용 하 여 테이블의 각 행에 사용할 행 컨트롤러 유형을 지정 합니다. 행의 식별자를 사용 하 여 각 행에 사용할 행 컨트롤러를 지정 합니다.

이 배열의 요소 수는 테이블에 있는 것으로 간주 되는 행 수와 일치 해야 합니다.

```csharp
myTable.SetRowTypes (new [] {"type1", "default", "default", "type2", "default"});
```

여러 행 컨트롤러를 사용 하 여 테이블을 채울 때 UI를 채울 때 필요한 형식을 추적 해야 합니다.

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

## <a name="vertical-detail-paging"></a>세로 세부 정보 페이징

watchOS 3에는 테이블에 대 한 새로운 기능이 도입 되었습니다. 테이블로 돌아가서 다른 행을 선택 하지 않고도 각 행과 관련 된 세부 정보 페이지를 스크롤할 수 있습니다. 위쪽 및 아래쪽으로 살짝 밀거나 Digital Crown를 사용 하 여 세부 정보 화면을 스크롤할 수 있습니다.

![](table-images/table-scroll-sml.png "세로 세부 정보 페이징 예제") ![](table-images/table-detail-sml.png)

> [!IMPORTANT]
> 이 기능은 현재 Xcode Interface Builder에서 storyboard를 편집 해야만 사용할 수 있습니다.

이 기능을 사용 하려면 디자인 화면에서 `WKInterfaceTable`를 선택 하 고 **세로 세부 정보 페이징** 옵션을 선택 합니다.

![](table-images/vertical-detail-paging-sml.png "Selecting the Vertical Detail Paging option")

[Apple에서 설명한](https://developer.apple.com/reference/watchkit/wkinterfacetable#1682023) 대로 테이블 탐색은 segue를 사용 하 여 페이징 기능이 작동 해야 합니다. @No__t_0를 사용 하 여 segue를 대신 사용 하는 기존 코드를 다시 작성 합니다.

<a name="add_row_controller" />

## <a name="appendix-row-controller-code-example"></a>부록: 행 컨트롤러 코드 예제

디자이너에서 행 컨트롤러를 만들 때 IDE에서 자동으로 두 개의 코드 파일을 만듭니다. 이러한 생성 된 파일의 코드는 아래에 참조를 위해 표시 됩니다.

첫 번째는 클래스의 이름으로 지정 됩니다 (예: **RowController.cs**).

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

**Designer.cs** 파일은 디자이너 `WKInterfaceLabel` 화면에 생성 된 작업을 포함 하는 부분 클래스 정의입니다. 예를 들어

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

그런 다음 여기에 선언 된 출 선 및 작업을 코드에서 참조할 수 있지만 **designer.cs** 파일은 직접 편집 하면 안 됩니다.

## <a name="related-links"></a>관련 링크

- [WatchTables (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchtables)
- [WatchKitCatalog (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Apple의 테이블 문서](https://developer.apple.com/reference/watchkit/wkinterfacetable)
