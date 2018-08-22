---
title: Xamarin.Mac의 테이블 뷰
description: 이 문서에서는 Xamarin.Mac 응용 프로그램의 테이블 뷰를 사용 하 여 작업을 설명 합니다. Xcode 및 Interface Builder 및 코드에서 상호 작용 하에서 만드는 테이블 뷰를 설명 합니다.
ms.prod: xamarin
ms.assetid: 3B55B858-4769-4331-966A-7F53B3B7C720
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 68b52fb4b7a3a65b45fcbecdc865bc64d9865fd9
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "40251139"
---
# <a name="table-views-in-xamarinmac"></a>Xamarin.Mac의 테이블 뷰

_이 문서에서는 Xamarin.Mac 응용 프로그램의 테이블 뷰를 사용 하 여 작업을 설명 합니다. Xcode 및 Interface Builder 및 코드에서 상호 작용 하에서 만드는 테이블 뷰를 설명 합니다._

Xamarin.Mac 응용 프로그램에서 C# 및.NET을 사용 하 여 작업을 하는 경우 동일 하 게 액세스할 수 있습니다 하는 테이블에서 작업 하는 개발자를 제공 하는 뷰 *Objective-c* 하 고 *Xcode* 않습니다. Xcode의 Xamarin.Mac이 Xcode와 직접 통합 되므로 사용할 수 있습니다 _Interface Builder_ 만들기 및 테이블 뷰를 유지 관리 (또는 필요에 따라 C# 코드에서 바로 작성).

테이블 뷰를 하나 이상의 열을 여러 행의 정보를 포함 하 여 테이블 형식으로 데이터를 표시 합니다. 만들어지는 테이블 보기의 유형에 따라 사용자 수 열별로 정렬, 열을 다시 구성, 추가 열, 열을 제거 또는 테이블 내에 포함 된 데이터를 편집 합니다.

[![](table-view-images/intro01.png "예제 테이블")](table-view-images/intro01.png#lightbox)

이 문서에서는 Xamarin.Mac 응용 프로그램에서 테이블 뷰를 사용 하 여 작업의 기본 사항을 설명 합니다. 것이 가장 좋습니다를 통해 작업 하는 합니다 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서 합니다 [Xcode 및 Interface Builder 소개](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 하 고 [출 선 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션으로 주요 개념 및이 문서를 사용 하는 기술을 설명 합니다.

확인 하려는 경우는 [노출 C# 클래스 / objective-c 메서드](~/mac/internals/how-it-works.md) 섹션을 [Xamarin.Mac 내부 요소](~/mac/internals/how-it-works.md) 설명도 문서를 `Register` 및 `Export` 명령 하 여 통신-C# 클래스 Objective-c 개체 및 UI 요소에 사용 합니다.

<a name="Introduction_to_Table_Views" />

## <a name="introduction-to-table-views"></a>테이블 보기 소개

테이블 뷰를 하나 이상의 열을 여러 행의 정보를 포함 하 여 테이블 형식으로 데이터를 표시 합니다. 테이블 뷰 스크롤 뷰 내에서 표시 됩니다 (`NSScrollView`) 및 macOS 10.7부터 사용할 수 있습니다 `NSView` 셀 대신 (`NSCell`) 행과 열을 표시 합니다. 그러나 즉, 계속 사용할 수 있습니다 `NSCell` 하면 일반적으로 하위 클래스입니다 `NSTableCellView` 에서 사용자 지정 행 및 열을 만듭니다.

테이블 뷰 자체 데이터를 저장 하지 않으므로, 데이터 원본에서 대신 (`NSTableViewDataSource`) 행과 열에는 필요에 따라 필요한 수 있도록 합니다.

테이블 보기 대리자의 서브 클래스를 제공 하 여 테이블 보기의 동작을 사용자 지정할 수 있습니다 (`NSTableViewDelegate`) 기능, 행 선택 및 편집, 사용자 지정 추적 및 개별 열에 대 한 사용자 지정 보기를 선택 하려면 입력 테이블 열 관리를 지원 하 고 행 수입니다.

테이블 뷰를 만들 때 Apple는 다음을 제안 합니다.

* 사용자가 열 헤더를 클릭 하 여 테이블을 정렬할 수 있습니다.
* 명사 또는 해당 열에 표시 되는 데이터를 설명 하는 간단한 명사구 열 머리글을 만듭니다.

자세한 내용은 참조 하십시오 합니다 [콘텐츠 보기](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) Apple의 섹션 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)합니다.

<a name="Creating-and-Maintaining-Table-Views-in-Xcode" />

## <a name="creating-and-maintaining-table-views-in-xcode"></a>만들기 및 Xcode의 테이블 뷰를 유지 관리

새 Xamarin.Mac Cocoa 응용 프로그램을 만들면 기본적으로 표준 비어 창을 가져옵니다. 이 windows에 정의 된를 `.storyboard` 파일 프로젝트에 자동으로 포함 합니다. windows 디자인에 맞게 편집 하는 **솔루션 탐색기**를 두 번 클릭 합니다 `Main.storyboard` 파일:

[![](table-view-images/edit01.png "주 스토리 보드를 선택합니다.")](table-view-images/edit01.png#lightbox)

Xcode의 Interface Builder에서 창 디자인을 엽니다.

[![](table-view-images/edit02.png "Xcode에서 UI를 편집합니다.")](table-view-images/edit02.png#lightbox)

형식 `table` 에 **라이브러리 검사기** 테이블 뷰 컨트롤을 찾을 수 있도록 검색 상자:

[![](table-view-images/edit03.png "라이브러리에서 테이블 보기를 선택합니다.")](table-view-images/edit03.png#lightbox)

뷰 컨트롤러 테이블 뷰를 끌어 합니다 **인터페이스 편집기**, 뷰 컨트롤러의 콘텐츠 영역을 입력 하 고 축소 하 고에서 창을 사용 하 여 증가를 설정 합니다 **제약 조건 편집기**:

[![](table-view-images/edit04.png "제약 조건 편집")](table-view-images/edit04.png#lightbox)

테이블 보기를 선택 합니다 **인터페이스 계층 구조** 에서 사용할 수 있는 다음 속성을 합니다 **특성 검사기**:

[![](table-view-images/edit05.png "특성 검사기")](table-view-images/edit05.png#lightbox)

- **모드 콘텐츠** -두 뷰를 사용할 수 있습니다 (`NSView`) 또는 셀 (`NSCell`) 행과 열에는 데이터를 표시 합니다. MacOS 10.7 이상에서는 뷰 사용 해야 합니다.
- **그룹 행 부동** - `true`, 부동는 것 처럼 테이블 뷰 그룹화 된 셀을 그립니다.
- **열** -표시 된 열의 수를 정의 합니다.
- **헤더** - `true`, 열 헤더를 갖습니다.
- **다시 정렬** - `true`, 사용자 끌 수 없게 됩니다. 테이블의 열 순서를 변경 합니다.
- **크기 조정** - `true`, 사용자는 열 크기를 조정 하려면 열 머리글을 끌 수 없게 됩니다.
- **열 크기 조정** -테이블 열 크기 자동으로 됩니다 하는 방법을 제어 합니다.
- **강조 표시** -셀을 선택한 경우 테이블을 강조 표시의 형식을 사용 하 여 제어 합니다.
- **대체 행** - `true`적이 다른 행에 다른 배경색을 갖습니다.
- **가로 격자 눈금** -가로로 셀 사이 그려진 테두리 유형을 선택 합니다.
- **세로 격자 눈금** -세로로 셀 사이 그려진 테두리 유형을 선택 합니다.
- **눈금 색** -셀의 테두리 색을 설정 합니다.
- **백그라운드** -셀 배경색을 설정 합니다.
- **선택** -사용자가 테이블의 셀을 선택할 수는 방법을 제어할 수 있습니다.
    - **여러** - `true`, 행 및 열을 여러 개 선택할 수 있습니다.
    - **열** - `true`, 열을 선택할 수 있습니다.
    - **입력 선택** - `true`, 행을 선택 하는 문자를 입력할 수 있습니다.
    - **빈** - `true`, 사용자가 행 또는 열을 선택 하지 않아도, 테이블 없습니다을 선택할 수 있도록 전혀 합니다.
- **자동 저장** -테이블 형식으로 자동으로 된 이름으로 저장 합니다.
- **열 정보** - `true`, 순서와 열 너비를 자동으로 저장 됩니다.
- **줄 바꿈** -셀 줄 바꿈을 처리 하는 방법을 선택 합니다.
- **표시 되는 마지막 줄을 자릅니다** - `true`, 셀의 경계 내에 맞지 데이터 수에서 잘립니다.

> [!IMPORTANT]
> 레거시 Xamarin.Mac 응용 프로그램을 유지 하는 경우가 아니면 `NSView` 기반된 테이블 뷰를 통해 사용할 `NSCell` 테이블 뷰를 기반으로 합니다. `NSCell` 레거시 간주 되 고 앞으로 지원 되지 않습니다.

테이블 열을 선택 합니다 **인터페이스 계층 구조** 에서 사용할 수 있는 다음 속성을 합니다 **특성 검사기**:

[![](table-view-images/edit06.png "특성 검사기")](table-view-images/edit06.png#lightbox)

- **제목** -열 제목을 설정 합니다.
- **맞춤** -셀 내의 텍스트의 맞춤을 설정 합니다.
- **제목 글꼴** -셀의 머리글 텍스트의 글꼴을 선택 합니다.
- **정렬 키** -키 열의 데이터를 정렬 하는 데 사용 됩니다. 사용자가이 열을 정렬할 수 없습니다 경우 비워 둡니다.
- **선택기** -은 합니다 **동작** 정렬을 수행 하는 데 사용 합니다. 사용자가이 열을 정렬할 수 없습니다 경우 비워 둡니다.
- **순서** -열 데이터에 대 한 정렬 순서입니다.
- **크기 조정** -열에 대 한 크기 조정의 유형을 선택 합니다.
- **편집 가능한** - `true`, 셀 기반 테이블의 셀을 편집할 수 있습니다.
- **숨겨진** - `true`, 열이 숨겨집니다.

핸들 (열의 오른쪽에 세로로 가운데) 왼쪽 또는 오른쪽으로 끌어서 열 크기를 조정할 수도 있습니다.

테이블 보기의 각 열을 선택 하 고 첫 번째 열을 제공 하겠습니다을 **제목** 의 `Product` 하 고 두 번째 `Details`입니다.

테이블 셀 뷰를 선택 (`NSTableViewCell`)에서 **인터페이스 계층 구조** 에서 사용할 수 있는 다음 속성을 **특성 검사기**:

[![](table-view-images/edit07.png "특성 검사기")](table-view-images/edit07.png#lightbox)

이들은 모두 표준 보기의 속성입니다. 여기에이 열에 대 한 행의 크기를 조정 하는 옵션이 있습니다.

테이블 뷰 셀을 선택 (기본적으로이를 `NSTextField`)에 **인터페이스 계층 구조** 되며 다음과 같은 속성에서 사용할 수 있습니다 합니다 **특성 검사기**:

[![](table-view-images/edit08.png "특성 검사기")](table-view-images/edit08.png#lightbox)

여기에서 설정 하는 표준 텍스트 필드의 모든 속성을 해야 합니다. 기본적으로 표준 텍스트 필드가 열에 셀에 대 한 데이터를 표시할 사용 됩니다.

테이블 셀 뷰를 선택 (`NSTableFieldCell`)에서 **인터페이스 계층 구조** 에서 사용할 수 있는 다음 속성을 **특성 검사기**:

[![](table-view-images/edit09.png "특성 검사기")](table-view-images/edit09.png#lightbox)

여기서 가장 중요 한 설정은 다음과 같습니다.

- **레이아웃** -이 열의 셀에에서 배치 하는 방법을 선택 합니다.
- **단일 줄 모드를 사용 하 여** - `true`, 셀이 한 줄으로 제한 합니다.
- **첫 번째 런타임 레이아웃 너비** - `true`, 셀을 설정 (수동 또는 자동으로) 경우 너비를 선호 하는 처음에 응용 프로그램이 실행 될 때 표시 됩니다.
- **동작** -시기를 제어 편집 **동작** 셀에 대 한 전송 됩니다.
- **동작** -셀이 선택 또는 편집할 수 있는 경우를 정의 합니다.
- **서식 있는 텍스트** - `true`, 셀 서식 및 스타일이 적용 된 텍스트를 표시할 수 있습니다.
- **실행 취소** - `true`, 셀 것에 대 한 책임을 지지 동작 실행 취소 합니다.

테이블 셀 뷰 선택 (`NSTableFieldCell`)에서 테이블 열의 맨 아래에 **인터페이스 계층 구조**:

[![](table-view-images/edit10.png "테이블 셀 보기를 선택합니다.")](table-view-images/edit10.png#lightbox)

기본으로 사용 되는 테이블 셀 보기를 편집할 수 있습니다 _패턴_ 지정된 된 열에 대해 생성 하는 모든 셀에 대 한 합니다.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>작업 및 출 선을 추가합니다.

방금 다른 Cocoa UI 컨트롤과 마찬가지로 테이블 보기를 노출 해야 하 고 열은 셀로 C# 코드 **동작** 하 고 **출 선** (필요한 모든 기능에 따라).

프로세스는 테이블 뷰 요소를 공개할지 여부에 대해 동일 합니다.

1. 로 전환 합니다 **도우미 편집기** 있는지 확인 합니다 `ViewController.h` 파일이 선택 됩니다.: 

    [![](table-view-images/edit11.png "단말기 편집기")](table-view-images/edit11.png#lightbox)
2. 테이블 보기를 선택 합니다 **인터페이스 계층 구조**컨트롤을 클릭 하 고 끌어는 `ViewController.h` 파일입니다.
3. 만들기는 **출 선** 라는 테이블 보기에 대 한 `ProductTable`: 

    [![](table-view-images/edit13.png "출 선 구성")](table-view-images/edit13.png#lightbox)
4. 만들 **출 선** 도 테이블 열에 대 한 호출 `ProductColumn` 고 `DetailsColumn`: 

    [![](table-view-images/edit14.png "출 선 구성")](table-view-images/edit14.png#lightbox)
5. 변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

다음으로, 작성 코드 표시 테이블에 대 한 일부 데이터는 응용 프로그램이 실행 될 때.

<a name="Populating_the_Table_View" />

## <a name="populating-the-table-view"></a>테이블 뷰 채우기

테이블 보기를 사용 하 여 Interface Builder에서 디자인 하 고을 통해 노출 되는 **출 선**, 다음 채울 C# 코드를 생성 해야 합니다.

먼저 새를 만들어 보겠습니다 `Product` 개별 행에 대 한 정보를 저장 하는 클래스입니다. 에 **솔루션 탐색기**, 프로젝트를 마우스 오른쪽 단추로 **추가** > **새 파일...** 선택 **일반** > **빈 클래스**를 입력 `Product` 에 대 한를 **이름** 을 클릭 합니다 **새로 만들기** 단추:

[![](table-view-images/populate01.png "빈 클래스 만들기")](table-view-images/populate01.png#lightbox)

확인 된 `Product.cs` 다음과 같은 파일 확인:

```csharp
using System;

namespace MacTables
{
    public class Product
    {
        #region Computed Propoperties
        public string Title { get; set;} = "";
        public string Description { get; set;} = "";
        #endregion

        #region Constructors
        public Product ()
        {
        }

        public Product (string title, string description)
        {
            this.Title = title;
            this.Description = description;
        }
        #endregion
    }
}

```

그런 다음의 서브 클래스를 생성 해야 `NSTableDataSource` 요청 된 것 처럼 테이블에 대 한 데이터를 제공 합니다. 에 **솔루션 탐색기**, 프로젝트를 마우스 오른쪽 단추로 **추가** > **새 파일...** 선택 **일반** > **빈 클래스**, 입력 `ProductTableDataSource` 에 대 한를 **이름** 을 클릭 합니다 **새로 만들기** 단추.

편집 된 `ProductTableDataSource.cs` 파일을 다음과 같이 표시 되도록 합니다.

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacTables
{
    public class ProductTableDataSource : NSTableViewDataSource
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Constructors
        public ProductTableDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override nint GetRowCount (NSTableView tableView)
        {
            return Products.Count;
        }
        #endregion
    }
}

```

이 클래스는 테이블 보기의 항목에 대 한 저장소에는 및 재정의 `GetRowCount` 테이블의 행 수가 반환 합니다.

마지막으로,의 서브 클래스를 생성 해야 `NSTableDelegate` 테이블에 대 한 동작을 제공 하도록 합니다. 에 **솔루션 탐색기**, 프로젝트를 마우스 오른쪽 단추로 **추가** > **새 파일...** 선택 **일반** > **빈 클래스**, 입력 `ProductTableDelegate` 에 대 한를 **이름** 을 클릭 합니다 **새로 만들기** 단추.

편집 된 `ProductTableDelegate.cs` 파일을 다음과 같이 표시 되도록 합니다.

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacTables
{
    public class ProductTableDelegate: NSTableViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "ProdCell";
        #endregion

        #region Private Variables
        private ProductTableDataSource DataSource;
        #endregion

        #region Constructors
        public ProductTableDelegate (ProductTableDataSource datasource)
        {
            this.DataSource = datasource;
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)tableView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Product":
                view.StringValue = DataSource.Products [(int)row].Title;
                break;
            case "Details":
                view.StringValue = DataSource.Products [(int)row].Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

인스턴스를 만들 것을 `ProductTableDelegate`, 인스턴스의 전달는 `ProductTableDataSource` 테이블에 대 한 데이터를 제공 하는 합니다. `GetViewForItem` 메서드는 제공 열과 행에 대 한 셀을 표시 하려면 보기 (데이터)를 반환 하는 일을 담당 합니다. 가능한 경우 셀을 표시 하려면 기존 뷰를 다시 사용 됩니다 그렇지 않은 경우 새 뷰를 만들어야 합니다.

테이블을 채우는 데 편집할 합니다 `ViewController.cs` 파일을 확인 합니다 `AwakeFromNib` 다음과 같은 메서드 확인:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create the Product Table Data Source and populate it
    var DataSource = new ProductTableDataSource ();
    DataSource.Products.Add (new Product ("Xamarin.iOS", "Allows you to develop native iOS Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Android", "Allows you to develop native Android Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Mac", "Allows you to develop Mac native Applications in C#"));

    // Populate the Product Table
    ProductTable.DataSource = DataSource;
    ProductTable.Delegate = new ProductTableDelegate (DataSource);
}
```

응용 프로그램을 실행 하는 경우 다음이 표시 됩니다.

[![](table-view-images/populate02.png "샘플 앱 실행")](table-view-images/populate02.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>열 기준 정렬

열 머리글을 클릭 하 여 테이블의 데이터를 정렬 하려면 사용자를 허용 해 보겠습니다. 첫째, 두 번 클릭 합니다 `Main.storyboard` Interface Builder에서 편집용으로 열 파일입니다. 선택 합니다 `Product` 열 입력 `Title` 에 대 한 합니다 **정렬 키**, `compare:` 에 대 한를 **선택기** 선택한 `Ascending` 에 대 한를 **순서**:

[![](table-view-images/sort01.png "정렬 키를 설정합니다.")](table-view-images/sort01.png#lightbox)

선택 합니다 `Details` 열 입력 `Description` 에 대 한 합니다 **정렬 키**, `compare:` 에 대 한를 **선택기** 선택한 `Ascending` 에 대 한를 **순서**:

[![](table-view-images/sort02.png "정렬 키를 설정합니다.")](table-view-images/sort02.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

이제 편집는 `ProductTableDataSource.cs` 파일과 다음 메서드를 추가 합니다.

```csharp
public void Sort(string key, bool ascending) {

    // Take action based on key
    switch (key) {
    case "Title":
        if (ascending) {
            Products.Sort ((x, y) => x.Title.CompareTo (y.Title));
        } else {
            Products.Sort ((x, y) => -1 * x.Title.CompareTo (y.Title));
        }
        break;
    case "Description":
        if (ascending) {
            Products.Sort ((x, y) => x.Description.CompareTo (y.Description));
        } else {
            Products.Sort ((x, y) => -1 * x.Description.CompareTo (y.Description));
        }
        break;
    }

}

public override void SortDescriptorsChanged (NSTableView tableView, NSSortDescriptor[] oldDescriptors)
{
    // Sort the data
    if (oldDescriptors.Length > 0) {
        // Update sort
        Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
    } else {
        // Grab current descriptors and update sort
        NSSortDescriptor[] tbSort = tableView.SortDescriptors; 
        Sort (tbSort[0].Key, tbSort[0].Ascending); 
    }
            
    // Refresh table
    tableView.ReloadData ();
}
```

`Sort` 메서드를 허용 하는 데이터 원본에서 데이터를 정렬 하는 지정 된 `Product` 클래스 필드를 오름차순 또는 내림차순입니다. 재정의 된 `SortDescriptorsChanged` 메서드를 사용 하 여 열 머리글을 클릭할 때마다 호출 됩니다. 전달 되는 **키** Interface Builder 및 해당 열에 대 한 정렬 순서에서 설정 하는 값입니다.

응용 프로그램을 실행 하 고 열 머리글을 클릭 합니다 행 열에 따라 정렬 됩니다.

[![](table-view-images/sort03.png "앱의 예 실행")](table-view-images/sort03.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>행 선택

사용자가 두 번 클릭을 단일 행을 선택 하도록 허용 하려는 경우는 `Main.storyboard` Interface Builder에서 편집용으로 열 파일입니다. 테이블 보기를 선택 합니다 **인터페이스 계층 구조** 의 선택을 취소는 **여러** 에서 확인란을 선택 합니다 **특성 검사기**:

[![](table-view-images/select01.png "특성 검사기")](table-view-images/select01.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.


다음에 편집는 `ProductTableDelegate.cs` 파일과 다음 메서드를 추가 합니다.

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

이렇게 하면 사용자 테이블 뷰에서 임의의 단일 행을 선택할 수 있습니다. 반환 `false` 에 대 한 합니다 `ShouldSelectRow` 사용자가 선택할 수 있게 하는 모든 행에 대 한 않으려는 또는 `false` 모든 행을 선택 하려면 사용자를 표시 하지 않으려는 경우 모든 행에 대 한 합니다.

테이블 보기 (`NSTableView`) 행 선택 작업에 대 한 다음 메서드를 포함 합니다.

- `DeselectRow(nint)` -테이블의 지정된 된 행을 선택 취소합니다.
- `SelectRow(nint,bool)` -지정된 된 행을 선택합니다. 전달 `false` 두 번째 매개 변수를 한 번에 하나의 행을 선택 합니다.
- `SelectedRow` -테이블에서 선택한 현재 행을 반환 합니다.
- `IsRowSelected(nint)` -반환 `true` 지정된 된 행을 선택 합니다.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>여러 개의 행 선택

사용자가 두 번 클릭을 여러 행을 선택 하도록 허용 하려는 경우는 `Main.storyboard` Interface Builder에서 편집용으로 열 파일입니다. 테이블 보기를 선택는 **인터페이스 계층 구조** 확인 하 고를 **여러** 의 확인란을 선택 합니다 **특성 검사기**:

[![](table-view-images/select02.png "특성 검사기")](table-view-images/select02.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.


다음에 편집는 `ProductTableDelegate.cs` 파일과 다음 메서드를 추가 합니다.

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

이렇게 하면 사용자 테이블 뷰에서 임의의 단일 행을 선택할 수 있습니다. 반환 `false` 에 대 한 합니다 `ShouldSelectRow` 사용자가 선택할 수 있게 하는 모든 행에 대 한 않으려는 또는 `false` 모든 행을 선택 하려면 사용자를 표시 하지 않으려는 경우 모든 행에 대 한 합니다.

테이블 보기 (`NSTableView`) 행 선택 작업에 대 한 다음 메서드를 포함 합니다.

- `DeselectAll(NSObject)` -테이블의 모든 행을 선택 취소합니다. 사용 하 여 `this` 선택 하 여 수행 하는 개체에 전송할 첫 번째 매개 변수입니다. 
- `DeselectRow(nint)` -테이블의 지정된 된 행을 선택 취소합니다.
- `SelectAll(NSobject)` -테이블의 모든 행을 선택합니다. 사용 하 여 `this` 선택 하 여 수행 하는 개체에 전송할 첫 번째 매개 변수입니다.
- `SelectRow(nint,bool)` -지정된 된 행을 선택합니다. 전달 `false` 두 번째 매개 변수에 대 한 선택을 단일 행만 선택 하 고 전달 `true` 선택 영역을 확장 하 고이 행을 포함 합니다.
- `SelectRows(NSIndexSet,bool)` -지정된 된 행 집합을 선택합니다. 전달 `false` 두 번째 매개 변수에 대 한 선택을 취소 하 고 선택만 행을 전달 `true` 선택 영역을 확장 하 고 이러한 행을 포함 합니다.
- `SelectedRow` -테이블에서 선택한 현재 행을 반환 합니다.
- `SelectedRows` -반환을 `NSIndexSet` 선택한 행의 인덱스를 포함 합니다.
- `SelectedRowCount` -선택된 된 행의 수를 반환합니다.
- `IsRowSelected(nint)` -반환 `true` 지정된 된 행을 선택 합니다.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>행을 선택 하는 형식

문자가 있는 첫 번째 행을 선택한 사용자가 선택한 테이블 보기를 사용 하 여 문자를 입력 하도록 허용 하려는 경우, 두 번 클릭 합니다 `Main.storyboard` Interface Builder에서 편집용으로 열 파일입니다. 테이블 보기를 선택는 **인터페이스 계층 구조** 확인를 **형식 선택** 의 확인란을 선택 합니다 **특성 검사기**:

[![](table-view-images/type01.png "선택 유형 설정")](table-view-images/type01.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

이제 편집는 `ProductTableDelegate.cs` 파일과 다음 메서드를 추가 합니다.

```csharp
public override nint GetNextTypeSelectMatch (NSTableView tableView, nint startRow, nint endRow, string searchString)
{
    nint row = 0;
    foreach(Product product in DataSource.Products) {
        if (product.Title.Contains(searchString)) return row;

        // Increment row counter
        ++row;
    }

    // If not found select the first row
    return 0;
}
```

`GetNextTypeSelectMatch` 메서드는 지정 된 `searchString` 첫 번째 행을 반환 하 고 `Product` 는 포함 된 문자열의 `Title`합니다.

응용 프로그램을 실행 하 고 문자를 입력, 하나의 행을 선택 합니다.

[![](table-view-images/type02.png "샘플 앱 실행")](table-view-images/type02.png#lightbox)

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>열 다시 정렬

테이블 보기에서 열 순서 바꾸기를 끌어 놓을 수 있도록 하려는 경우, 두 번 클릭 합니다 `Main.storyboard` Interface Builder에서 편집용으로 열 파일입니다. 테이블 보기를 선택는 **인터페이스 계층 구조** 확인 하 고를 **순서 다시 매기기** 의 확인란을 선택 합니다 **특성 검사기**:

[![](table-view-images/reorder01.png "특성 검사기")](table-view-images/reorder01.png#lightbox)

에 대 한 값을 제공 하는 경우는 **자동 저장** 속성을 확인 합니다 **열 정보** 필드를 테이블의 레이아웃을 변경 했습니다 우리 회사에 저장 됩니다 하 고 다음에 응용 프로그램 복원 실행 됩니다.

변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

이제 편집는 `ProductTableDelegate.cs` 파일과 다음 메서드를 추가 합니다.

```csharp
public override bool ShouldReorder (NSTableView tableView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

합니다 `ShouldReorder` 메서드는 반환 해야 `true` 할 수 있도록 하려는 모든 열에 대 한으로 다시 정렬 끌어서 합니다 `newColumnIndex`, 그렇지 않으면 반환 `false`;

응용 프로그램을 실행 하는 경우 관련 열 머리글이 열 순서를 변경 하려면 끌 수 있습니다.

[![](table-view-images/reorder02.png "다시 정렬 된 열의 예")](table-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>셀 편집

사용자가 지정된 된 셀의 값을 편집, 편집 하도록 허용 하려는 경우는 `ProductTableDelegate.cs` 파일을 변경 합니다 `GetViewForItem` 같이 메서드:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{
    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTextField view = (NSTextField)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTextField ();
        view.Identifier = tableColumn.Title;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = true;

        view.EditingEnded += (sender, e) => {
                    
            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.Tag].Title = view.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.Tag].Description = view.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

이제 응용 프로그램을 실행 하는 경우 사용자 테이블 보기의 셀을 편집할 수 있습니다.

[![](table-view-images/editing01.png "셀 편집의 예제")](table-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Table_Views" />

## <a name="using-images-in-table-views"></a>표 보기에서 이미지를 사용 하 여

셀의 일부로 이미지를 포함 하는 `NSTableView`에서 테이블 보기의 데이터가 반환 되는 방식을 변경 해야 `NSTableViewDelegate's` `GetViewForItem` 메서드를 사용 하 여를 `NSTableCellView` 대신 일반적인 `NSTextField`합니다. 예를 들어:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = true;

        view.TextField.EditingEnded += (sender, e) => {

            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.TextField.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tags.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

자세한 내용은 참조 하십시오는 [테이블 뷰를 사용 하 여 이미지를 사용 하 여](~/mac/app-fundamentals/image.md) 부분 우리의 [이미지를 사용 하 여 작업](~/mac/app-fundamentals/image.md) 설명서.

<a name="Adding-a-Delete-Button-to-a-Row" />

## <a name="adding-a-delete-button-to-a-row"></a>행 삭제 단추 추가

앱의 요구 사항에 따라 있을 표의 각 행에 대 한 실행 단추를 제공 해야 합니다. 이 예를 들어, 확장을 포함 하도록 위에서 만든 테이블 보기 예는 **삭제** 각 행에는 단추입니다.

먼저 편집을 `Main.storyboard` Xcode의 Interface Builder에서 테이블 보기를 선택 하 고 열 수를 늘리려면 3 (3). 다음으로 변경 합니다 **제목** 새 열의 `Action`:

[![](table-view-images/delete01.png "열 이름 편집")](table-view-images/delete01.png#lightbox)

스토리 보드에 변경 내용을 저장 하 고 변경 내용을 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

다음으로 편집는 `ViewController.cs` 파일과 다음 공용 메서드를 추가 합니다.

```csharp
public void ReloadTable ()
{
    ProductTable.ReloadData ();
}
```

동일한 파일 내에 새 테이블 보기 대리자를 만들 수정 `ViewDidLoad` 같이 메서드:

```csharp
// Populate the Product Table
ProductTable.DataSource = DataSource;
ProductTable.Delegate = new ProductTableDelegate (this, DataSource);
```

이제 편집을 `ProductTableDelegate.cs` 보기 컨트롤러에 개인 연결을 포함 하 고 대리자의 새 인스턴스를 만들 때 컨트롤러를 매개 변수로를 파일:

```csharp
#region Private Variables
private ProductTableDataSource DataSource;
private ViewController Controller;
#endregion

#region Constructors
public ProductTableDelegate (ViewController controller, ProductTableDataSource datasource)
{
    this.Controller = controller;
    this.DataSource = datasource;
}
#endregion
```

클래스에 다음 새 private 메서드를 다음을 추가 합니다.

```csharp
private void ConfigureTextField (NSTableCellView view, nint row)
{
    // Add to view
    view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
    view.AddSubview (view.TextField);

    // Configure
    view.TextField.BackgroundColor = NSColor.Clear;
    view.TextField.Bordered = false;
    view.TextField.Selectable = false;
    view.TextField.Editable = true;

    // Wireup events
    view.TextField.EditingEnded += (sender, e) => {

        // Take action based on type
        switch (view.Identifier) {
        case "Product":
            DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
            break;
        case "Details":
            DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
            break;
        }
    };

    // Tag view
    view.TextField.Tag = row;
}
```

이 모든에서 이전에 수행 된 되는 텍스트 보기 구성을 `GetViewForItem` 메서드 (텍스트 볼 수 있지만 단추에는 테이블의 마지막 열 포함 되지 않습니다) 때문에 단일 호출 가능 위치에 배치 합니다.

마지막으로 편집 된 `GetViewForItem` 메서드 수 있으며 다음과 같은 모양:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();

        // Configure the view
        view.Identifier = tableColumn.Title;

        // Take action based on title
        switch (tableColumn.Title) {
        case "Product":
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
            ConfigureTextField (view, row);
            break;
        case "Details":
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
            ConfigureTextField (view, row);
            break;
        case "Action":
            // Create new button
            var button = new NSButton (new CGRect (0, 0, 81, 16));
            button.SetButtonType (NSButtonType.MomentaryPushIn);
            button.Title = "Delete";
            button.Tag = row;

            // Wireup events
            button.Activated += (sender, e) => {
                // Get button and product
                var btw = sender as NSButton;
                var product = DataSource.Products [(int)btn.Tag];

                // Configure alert
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Informational,
                    InformativeText = $"Are you sure you want to delete {product.Title}? This operation cannot be undone.",
                    MessageText = $"Delete {product.Title}?",
                };
                alert.AddButton ("Cancel");
                alert.AddButton ("Delete");
                alert.BeginSheetForResponse (Controller.View.Window, (result) => {
                    // Should we delete the requested row?
                    if (result == 1001) {
                        // Remove the given row from the dataset
                        DataSource.Products.RemoveAt((int)btn.Tag);
                        Controller.ReloadTable ();
                    }
                });
            };

            // Add to view
            view.AddSubview (button);
            break;
        }

    }

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tag.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        view.TextField.Tag = row;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        view.TextField.Tag = row;
        break;
    case "Action":
        foreach (NSView subview in view.Subviews) {
            var btw = subview as NSButton;
            if (btw != null) {
                btn.Tag = row;
            }
        }
        break;
    }

    return view;
}
```

이 코드를 자세히의 여러 섹션에서 살펴보겠습니다. 먼저 새 `NSTableViewCell` 는 작업 생성 되는 기반으로 수행는 열의 이름입니다. 처음 두 열에 대 한 (**제품** 하 고 **세부 정보**), 새 `ConfigureTextField` 메서드가 호출 됩니다.

에 대 한 합니다 **동작** 열에서 새 `NSButton` 만들어지고 하위 뷰로 셀에 추가:

```csharp
// Create new button
var button = new NSButton (new CGRect (0, 0, 81, 16));
button.SetButtonType (NSButtonType.MomentaryPushIn);
button.Title = "Delete";
button.Tag = row;
...

// Add to view
view.AddSubview (button);
```

단추의 `Tag` 속성은 현재 처리 중인 행 수를 보유 하는 데 사용 됩니다. 이 번호가 사용 됩니다 나중에 사용자 요청 단추에서 삭제할 행 `Activated` 이벤트:

```csharp
// Wireup events
button.Activated += (sender, e) => {
    // Get button and product
    var btw = sender as NSButton;
    var product = DataSource.Products [(int)btn.Tag];

    // Configure alert
    var alert = new NSAlert () {
        AlertStyle = NSAlertStyle.Informational,
        InformativeText = $"Are you sure you want to delete {product.Title}? This operation cannot be undone.",
        MessageText = $"Delete {product.Title}?",
    };
    alert.AddButton ("Cancel");
    alert.AddButton ("Delete");
    alert.BeginSheetForResponse (Controller.View.Window, (result) => {
        // Should we delete the requested row?
        if (result == 1001) {
            // Remove the given row from the dataset
            DataSource.Products.RemoveAt((int)btn.Tag);
            Controller.ReloadTable ();
        }
    });
};
```

이벤트 처리기를 시작할 때 단추 및 제품에 지정 된 테이블 행을 가져옵니다. 다음 경고는 행 삭제를 확인 하는 사용자에 게 표시 됩니다. 사용자가 행을 삭제 하기로 데이터 원본에서 지정된 된 행 제거 되 고 테이블 다시 로드 됩니다.

```csharp
// Remove the given row from the dataset
DataSource.Products.RemoveAt((int)btn.Tag);
Controller.ReloadTable ();
```

마지막으로, 테이블 뷰 셀 새로 생성 하는 대신를 재사용 하는 경우 다음 코드를 처리 중인 열에 따라 구성 합니다.

```csharp
// Setup view based on the column selected
switch (tableColumn.Title) {
case "Product":
    view.ImageView.Image = NSImage.ImageNamed ("tag.png");
    view.TextField.StringValue = DataSource.Products [(int)row].Title;
    view.TextField.Tag = row;
    break;
case "Details":
    view.TextField.StringValue = DataSource.Products [(int)row].Description;
    view.TextField.Tag = row;
    break;
case "Action":
    foreach (NSView subview in view.Subviews) {
        var btw = subview as NSButton;
        if (btw != null) {
            btn.Tag = row;
        }
    }
    break;
}

```

에 대 한 합니다 **작업** 열에서 모든 하위 보기까지 검색 되는 `NSButton` 발견 되는 `Tag` 속성이 현재 행을 가리키도록 업데이트 됩니다.

이러한 변경이 사용 하 여 앱을 실행할 각 행 하는 경우는 **삭제** 단추:

[![](table-view-images/delete02.png "삭제 단추를 사용 하 여 테이블 보기")](table-view-images/delete02.png#lightbox)

클릭할 때를 **삭제** 단추를 지정 된 행을 삭제 하 라는 경고가 표시 됩니다.

[![](table-view-images/delete03.png "Delete 행 경고")](table-view-images/delete03.png#lightbox)

사용자가 삭제를 위해서는 행을 제거 하 고 테이블 다시 그려집니다.

[![](table-view-images/delete04.png "행 삭제 된 후 테이블")](table-view-images/delete04.png#lightbox)

<a name="Data_Binding_Table_Views" />

## <a name="data-binding-table-views"></a>데이터 바인딩 테이블 뷰

Xamarin.Mac 응용 프로그램에서 데이터 바인딩 및 키-값 코딩 기술을 사용 하 여 작성 하 고 채우는 UI 요소를 사용 하 여 작업을 유지 해야 하는 코드의 양을 크게 줄일 수 있습니다. 추가 백업 데이터를 분리의 장점은 수도 있습니다 (_데이터 모델_)에 처음부터 사용자 인터페이스를 종료 합니다 (_모델-뷰-컨트롤러_)를 더 쉬워진 유지 관리를 보다 융통성 있는 응용 프로그램 디자인 합니다.

키-값 코딩 (KVC)는 개체의 속성을 직접 액세스, 인스턴스 변수를 통해 액세스 하는 대신 속성을 식별 (특별 한 형식의 문자열) 키 또는 접근자 메서드를 사용 하는 메커니즘 (`get/set`). Xamarin.Mac 응용 프로그램에서 키-값 코딩 규격 접근자를 구현, 키-값을 관찰 (KVO), 데이터 바인딩, 핵심 데이터, Cocoa 바인딩 및 스크립팅 등과 같은 다른 macOS 기능에 대 한 액세스를 얻을 수 있습니다.

자세한 내용은 참조 하십시오는 [테이블 뷰 데이터 바인딩](~/mac/app-fundamentals/databinding.md#Table_View_Data_Binding) 부분 우리의 [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md) 설명서.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Mac 응용 프로그램에서 테이블 뷰를 사용 하 여 작업을 자세히 살펴보려면을 걸렸습니다. 다양 한 종류를 확인 하 고 테이블 뷰를 만들고 Xcode의 Interface Builder에서 테이블 뷰를 유지 관리 하는 방법 및 C# 코드에서 테이블 뷰를 사용 하는 방법을 사용 합니다.

## <a name="related-links"></a>관련 링크

- [MacTables (샘플)](https://developer.xamarin.com/samples/mac/MacTables/)
- [MacImages(샘플)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [개요 보기](~/mac/user-interface/outline-view.md)
- [원본 목록](~/mac/user-interface/source-list.md)
- [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [NSTableView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSTableView_Class/index.html#//apple_ref/doc/uid/TP40004125)
- [NSTableViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSTableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008622)
- [NSTableViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSTableDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004178)
