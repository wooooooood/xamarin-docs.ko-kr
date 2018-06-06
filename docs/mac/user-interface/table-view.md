---
title: 표 보기에서 Xamarin.Mac
description: 이 문서에서는 Xamarin.Mac 응용 프로그램에서 테이블 뷰를 사용 하는 작업을 설명합니다. Xcode 및 작성기 인터페이스 및 코드에서 상호 작용 하에서 만드는 테이블 뷰를 설명 합니다.
ms.prod: xamarin
ms.assetid: 3B55B858-4769-4331-966A-7F53B3B7C720
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: da26810869f23b8861ffb4409248c56bff12a521
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793232"
---
# <a name="table-views-in-xamarinmac"></a>표 보기에서 Xamarin.Mac

_이 문서에서는 Xamarin.Mac 응용 프로그램에서 테이블 뷰를 사용 하는 작업을 설명합니다. Xcode 및 작성기 인터페이스 및 코드에서 상호 작용 하에서 만드는 테이블 뷰를 설명 합니다._

C# 및.NET Xamarin.Mac 응용 프로그램에서에서 작업할 때는 동일 하 게 액세스할 수 있습니다 하는 테이블에서 작업을 수행할을 제공 하는 뷰 *Objective-c* 및 *Xcode* 않습니다. Xamarin.Mac Xcode에 직접 통합을 사용 하면 Xcode의 _인터페이스 작성기_ 만들기 및 테이블 뷰를 유지 관리 (또는 C# 코드에서 직접 만들 필요에 따라).

표 보기에는 하나 이상의 열이 여러 행에 대 한 정보를 테이블 형식으로 데이터가 표시 됩니다. 만들려는 테이블 뷰 형식에 따라 사용자 수 열별로 정렬, 열을 다시 구성, 추가, 열을 제거 하거나 테이블 내에 포함 된 데이터를 편집 합니다.

[![](table-view-images/intro01.png "예제 테이블")](table-view-images/intro01.png#lightbox)

이 문서에서는의 기본적인 Xamarin.Mac 응용 프로그램에서 테이블 뷰를 사용 하겠습니다. 것이 가장 좋습니다를 통해 협력 하는 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서는 [Xcode 및 인터페이스 작성기 소개](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) 및 [콘센트 및 동작](~/mac/get-started/hello-mac.md#Outlets_and_Actions) 섹션으로이 문서에서 사용할 수 있는 주요 개념 및 기술을 설명 합니다.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명도 문서는 `Register` 및 `Export` 명령 와이어 접속 C# 클래스 Objective-c 개체 및 UI 요소에 하는 데 사용 합니다.

<a name="Introduction_to_Table_Views" />

## <a name="introduction-to-table-views"></a>테이블 보기 소개

표 보기에는 하나 이상의 열이 여러 행에 대 한 정보를 테이블 형식으로 데이터가 표시 됩니다. 테이블 뷰 스크롤 뷰 내에서 표시 됩니다 (`NSScrollView`) macOS 10.7 이상에서는 사용할 수 있습니다 및 `NSView` 셀 대신 (`NSCell`) 행과 열이 모두 표시 합니다. 그러나 즉, 사용할 수 있습니다 `NSCell` 살펴볼 일반적으로 하위 클래스 `NSTableCellView` 사용자 지정 행과 열을 만듭니다.

테이블 뷰 자체 데이터를 저장 하지 않으므로, 데이터 원본에서 대신 (`NSTableViewDataSource`) 행과 열에는 필요에 따라 필요한 수 있도록 합니다.

테이블 보기 대리자의 서브 클래스를 제공 하 여 테이블 보기의 동작을 사용자 지정할 수 있습니다 (`NSTableViewDelegate`)를 지원 하기 위해 테이블 열 관리 기능, 행 선택 및 편집, 사용자 지정 추적 및 개별 열에 대 한 사용자 지정 보기를 선택 하려면 입력 하 고 행 수입니다.

테이블 보기를 만들 때는 Apple 다음 제안 합니다.

* 열 머리글을 클릭 하 여 테이블을 정렬할 수 있습니다.
* 명사 또는 해당 열에 표시 되 고 데이터를 설명 하는 짧은 명사구에는 열 머리글을 만듭니다.

자세한 내용은 참조 하십시오는 [콘텐츠 보기](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) Apple의 섹션 [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)합니다.

<a name="Creating-and-Maintaining-Table-Views-in-Xcode" />

## <a name="creating-and-maintaining-table-views-in-xcode"></a>만들기 및 Xcode의 테이블 뷰를 유지 관리

새 Xamarin.Mac Cocoa 응용 프로그램을 만들 때 기본적으로 표준, 빈 창이 얻을 수 있습니다. 에 정의 되어 있는이 windows는 `.storyboard` 파일은 프로젝트에 자동으로 포함 합니다. windows 디자인에 맞게 편집 하는 **솔루션 탐색기**, 두 번 클릭 하 고 `Main.storyboard` 파일:

[![](table-view-images/edit01.png "주 스토리 보드를 선택합니다.")](table-view-images/edit01.png#lightbox)

Xcode의 인터페이스 작성기의 창 디자인을 열립니다.

[![](table-view-images/edit02.png "Xcode에서 UI를 편집합니다.")](table-view-images/edit02.png#lightbox)

형식 `table` 에 **라이브러리 검사기의** 쉽게 테이블 뷰 컨트롤을 찾을 수 있도록 하려면 검색 상자:

[![](table-view-images/edit03.png "라이브러리에서 테이블 뷰를 선택합니다.")](table-view-images/edit03.png#lightbox)

표 보기에서 보기 컨트롤러 끌어다는 **인터페이스 편집기**, 축소 하 고 있는 창으로 증가를 설정 하 고 보기 컨트롤러의 콘텐츠 영역을 채우도록는 **제약 조건 편집기**:

[![](table-view-images/edit04.png "제약 조건 편집")](table-view-images/edit04.png#lightbox)

테이블 뷰를 선택는 **인터페이스 계층 구조** 하며 다음과 같은 속성에서 사용할 수는 **특성 검사기**:

[![](table-view-images/edit05.png "특성 검사기")](table-view-images/edit05.png#lightbox)

- **모드 콘텐츠** -뷰 중 하나를 사용할 수 있습니다 (`NSView`) 또는 셀 (`NSCell`) 행과 열에는 데이터를 표시 합니다. MacOS 10.7 이상에서는 뷰 사용 해야 합니다.
- **그룹 행 부동** -경우 `true`, 부동은 마치 테이블 뷰 그룹화 된 셀을 그립니다.
- **열** -표시 된 열 수를 정의 합니다.
- **헤더** -경우 `true`, 열 머리글을 갖습니다.
- **다시 정렬** -경우 `true`, 사용자 끌 수는 테이블의 열 순서를 변경 합니다.
- **크기 조정** -경우 `true`, 사용자 열의 크기를 조정 하려면 열 머리글을 끌 수 없게 됩니다.
- **열 크기 조정** -테이블 열 크기를 자동 됩니다 방법을 제어 합니다.
- **강조 표시** -는 셀을 선택 하면 테이블을 강조 표시 유형을 사용 하 여 제어 합니다.
- **행을 교대로 할당** -경우 `true`적이 다른 행에 다른 배경색을 갖습니다.
- **가로 격자 눈금** -셀 사이 그려지는 가로 테두리의 유형을 선택 합니다.
- **세로 격자 눈금** -세로로 그릴 셀 사이의 테두리 유형을 선택 합니다.
- **눈금 색** -셀의 테두리 색을 설정 합니다.
- **배경** -배경 셀 색을 설정 합니다.
- **선택 영역** -사용자가 테이블의 셀을 선택할 수는 방법을 제어할 수 있습니다.
    - **여러** -경우 `true`, 사용자 여러 행과 열을 선택할 수 있습니다.
    - **열** -경우 `true`, 사용자는 열을 선택할 수 있습니다.
    - **입력 선택** -경우 `true`, 사용자는 행을 선택 하는 문자를 입력할 수 있습니다.
    - **빈** -경우 `true`, 사용자가 행 또는 열을 선택할 필요가 없습니다,는 테이블을 사용 하면 선택에 대 한 전혀 합니다.
- **자동 저장** -테이블 형식이 자동으로 이름으로 저장 합니다.
- **열 정보** -경우 `true`, 순서 및 열의 너비를 자동으로 저장 됩니다.
- **줄 바꿈을** -셀 줄 바꿈을 처리 하는 방법 선택 합니다.
- **마지막 줄 자릅니다** -경우 `true`, 셀의 범위 내에 포함 되지 데이터 수 잘립니다.

> [!IMPORTANT]
> 레거시 Xamarin.Mac 응용 프로그램을 유지 하는 경우가 아니면 `NSView` 기반된 테이블 뷰를 통해 사용 해야 `NSCell` 테이블 뷰를 기반으로 합니다. `NSCell` 레거시 간주 되 고 앞으로 지원 되지 않습니다.

테이블 열 선택의 **인터페이스 계층 구조** 다음 속성에서 사용할 수 있는 **특성 검사기**:

[![](table-view-images/edit06.png "특성 검사기")](table-view-images/edit06.png#lightbox)

- **제목** -열의 제목을 설정 합니다.
- **맞춤** -셀 내의 텍스트의 맞춤을 설정 합니다.
- **제목 글꼴** -셀의 머리글 텍스트 글꼴을 선택 합니다.
- **정렬 키** -열에 데이터를 정렬 하는 데 사용 되는 키입니다. 사용자는이 열을 정렬할 수 없습니다 비워 둡니다.
- **선택기** -가 **동작** 정렬을 수행 하는 데 사용 합니다. 사용자는이 열을 정렬할 수 없습니다 비워 둡니다.
- **주문** -는 열 데이터에 대 한 정렬 순서입니다.
- **크기 조정** -열에 대 한 크기 조정의 유형을 선택 합니다.
- **편집 가능한** -경우 `true`, 사용자 기반 셀 표의 셀을 편집할 수 있습니다.
- **숨겨진** -경우 `true`, 열이 숨겨집니다.

핸들 (열의 오른쪽에 세로로 가운데) 왼쪽 이나 오른쪽으로 끌어 열의 크기를 조정할 수도 있습니다.

우리의 테이블 보기의 각 열을 선택 하 고 첫 번째 열 지정 하겠습니다는 **제목** 의 `Product` 및 두 번째 `Details`합니다.

표 셀 보기 선택 (`NSTableViewCell`)에 **인터페이스 계층 구조** 하며 다음과 같은 속성에서 사용할 수는 **특성 검사기**:

[![](table-view-images/edit07.png "특성 검사기")](table-view-images/edit07.png#lightbox)

이들은 모두 표준 보기의 속성입니다. 여기에이 열에 대 한 행의 크기를 조정 하는 옵션이 수도 있습니다.

테이블 보기 셀을 선택 (이 기본적으로 `NSTextField`)에 **인터페이스 계층 구조** 다음 속성에서 사용할 수 있는 **특성 검사기**:

[![](table-view-images/edit08.png "특성 검사기")](table-view-images/edit08.png#lightbox)

여기에서 설정 하는 표준 텍스트 필드의 모든 속성이 있습니다. 기본적으로 표준 텍스트 필드는 데이터를 표시 하는 셀에 대 한 열에 사용 됩니다.

표 셀 보기 선택 (`NSTableFieldCell`)에 **인터페이스 계층 구조** 하며 다음과 같은 속성에서 사용할 수는 **특성 검사기**:

[![](table-view-images/edit09.png "특성 검사기")](table-view-images/edit09.png#lightbox)

여기에 가장 중요 한 설정은 다음과 같습니다.

- **레이아웃** -이 열의 셀에에서 배치 하는 방법을 선택 합니다.
- **한 줄 모드를 사용 하 여** -경우 `true`, 셀이 한 줄으로 제한 합니다.
- **첫 번째 런타임 레이아웃 너비** -경우 `true`, 셀 너비 설정에 대 한 (수동 또는 자동으로) 경우 것을 선호할 것 처음으로 응용 프로그램이 실행 될 때 표시 됩니다.
- **동작** -시점을 제어 편집 **동작** 셀에 대 한 전송 됩니다.
- **동작** -셀이 선택 또는 편집할 수 있는지를 정의 합니다.
- **서식 있는 텍스트** -경우 `true`, 셀으로 포맷 되 고 스타일이 적용 된 텍스트를 표시할 수 있습니다.
- **실행 취소** -경우 `true`, 셀은에 대 한 책임을 지지 동작 실행 취소 합니다.

표 셀 보기 선택 (`NSTableFieldCell`)에서 테이블 열의 맨 아래에 **인터페이스 계층 구조**:

[![](table-view-images/edit10.png "표 셀 보기를 선택합니다.")](table-view-images/edit10.png#lightbox)

기본으로 사용 되는 테이블 셀 보기를 편집할 수 있습니다 _패턴_ 지정된 된 열에 대해 생성 된 모든 셀에 대 한 합니다.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>작업 및 콘센트 추가

방금 다른 Cocoa UI 컨트롤 처럼 우리의 테이블 뷰를 노출 해야 하 고 열은 C# 코드를 사용 하 여 셀 **동작** 및 **콘센트** (필요한 기능에 따라).

프로세스를 노출 하는 모든 테이블 뷰 요소에 대해 같습니다.

1. 전환 하는 **도우미 편집기** 되어 있는지 확인 하 고는 `ViewController.h` 파일을 선택: 

    [![](table-view-images/edit11.png "도우미 편집기")](table-view-images/edit11.png#lightbox)
2. 테이블 뷰를 선택는 **인터페이스 계층 구조**control 클릭 하 고 끌어는 `ViewController.h` 파일입니다.
3. 만들기는 **콘센트** 라는 테이블 보기에 대 한 `ProductTable`: 

    [![](table-view-images/edit13.png "콘센트에 연결 구성")](table-view-images/edit13.png#lightbox)
4. 만들 **콘센트** 는 테이블 열에 대 한 호출 `ProductColumn` 및 `DetailsColumn`: 

    [![](table-view-images/edit14.png "콘센트에 연결 구성")](table-view-images/edit14.png#lightbox)
5. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

다음으로 작성 합니다 코드 표시 테이블에 대 한 일부 데이터는 응용 프로그램이 실행 될 때.

<a name="Populating_the_Table_View" />

## <a name="populating-the-table-view"></a>테이블 뷰를 채우는

우리의 표 보기와 인터페이스 작성기에서 디자인 하 고을 통해 노출 되는 **콘센트**, 다음은 C# 채우는 코드를 만들려는 필요 합니다.

첫째, 새를 만들어 보겠습니다 `Product` 개별 행에 대 한 정보를 보관 하는 클래스입니다. 에 **솔루션 탐색기**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...** 선택 **일반** > **빈 클래스**, 입력 `Product` 에 대 한는 **이름** 클릭는 **새로** 단추:

[![](table-view-images/populate01.png "빈 클래스 만들기")](table-view-images/populate01.png#lightbox)

확인 된 `Product.cs` 다음과 같은 파일 보기:

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

서브 클래스를 만들어야 한다는 다음으로, `NSTableDataSource` 을 요청 하며 테이블에 대 한 데이터 제공 합니다. 에 **솔루션 탐색기**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...** 선택 **일반** > **빈 클래스**, 입력 `ProductTableDataSource` 에 대 한는 **이름** 클릭는 **새로** 단추입니다.

편집 된 `ProductTableDataSource.cs` 파일을 다음과 같이 표시 하 게 합니다.

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

이 클래스에는 테이블 보기의 항목에 대 한 저장소가 및 재정의 `GetRowCount` 를 테이블의 행 수를 반환 합니다.

서브 클래스를 만들어야 한다는 마지막으로, `NSTableDelegate` 하며 테이블에 대 한 동작을 제공 하도록 합니다. 에 **솔루션 탐색기**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...** 선택 **일반** > **빈 클래스**, 입력 `ProductTableDelegate` 에 대 한는 **이름** 클릭는 **새로** 단추입니다.

편집 된 `ProductTableDelegate.cs` 파일을 다음과 같이 표시 하 게 합니다.

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

인스턴스를 만들 때는 `ProductTableDelegate`, 또한의 인스턴스에 전달는 `ProductTableDataSource` 테이블에 대 한 데이터를 제공 하는입니다. `GetViewForItem` 메서드는 제공 열과 행에 대 한 셀을 표시 하도록 보기 (데이터)를 반환 합니다. 가능 하면 셀을 표시 하려면 기존 뷰를 다시 사용 됩니다 하지 않은 경우 새 보기를 만들어야 합니다.

테이블을 구성 하려면 편집는 `ViewController.cs` 파일을 확인는 `AwakeFromNib` 다음과 같은 메서드 보기:

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

응용 프로그램을 실행 하는 경우 다음 내용이 표시 됩니다.

[![](table-view-images/populate02.png "샘플 응용 프로그램 실행")](table-view-images/populate02.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>열 기준 정렬

열 머리글을 클릭 하 여 테이블의 데이터를 정렬할 수 있는 사용자를 수 보겠습니다. 첫째, 두 번 클릭 하 고 `Main.storyboard` 인터페이스 작성기에서 편집을 위해 열 파일입니다. 선택 된 `Product` 열을 입력 `Title` 에 대 한는 **정렬 키**, `compare:` 에 대 한는 **선택기** 선택 `Ascending` 에 대 한는 **순서**:

[![](table-view-images/sort01.png "정렬 키를 설정합니다.")](table-view-images/sort01.png#lightbox)

선택 된 `Details` 열을 입력 `Description` 에 대 한는 **정렬 키**, `compare:` 에 대 한는 **선택기** 선택 `Ascending` 에 대 한는 **순서**:

[![](table-view-images/sort02.png "정렬 키를 설정합니다.")](table-view-images/sort02.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

이제 편집는 `ProductTableDataSource.cs` 파일을 다음 메서드를 추가 합니다.

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

`Sort` 메서드를 기반으로 데이터 원본에서 데이터를 정렬할 수 있도록 허용 된 주어진 `Product` 오름차순 또는 내림차순 클래스 필드입니다. 재정의 된 `SortDescriptorsChanged` 메서드를 사용 하는 열 머리글을 클릭할 때마다 호출 됩니다. 전달 되는 **키** 인터페이스 작성기와 해당 열에 대 한 정렬 순서에 설정 하는 값입니다.

응용 프로그램을 실행 하 고 열 머리글에서을 클릭 합니다 행이 해당 열으로 정렬 됩니다.

[![](table-view-images/sort03.png "실행 하는 예제 응용 프로그램")](table-view-images/sort03.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>행 선택

사용자가 두 번 클릭을 단일 행을 선택 하도록 허용 하려는 경우는 `Main.storyboard` 인터페이스 작성기에서 편집을 위해 열 파일입니다. 테이블 뷰를 선택는 **인터페이스 계층 구조** 의 선택을 취소 하 고는 **여러** 확인란을 선택은 **특성 검사기**:

[![](table-view-images/select01.png "특성 검사기")](table-view-images/select01.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.


다음에 편집는 `ProductTableDelegate.cs` 파일을 다음 메서드를 추가 합니다.

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

이렇게 하면 사용자가 테이블 보기에서 임의의 단일 행을 선택할 수 있습니다. 반환할 `false` 에 대 한는 `ShouldSelectRow` 사용자를 선택할 수 있는 모든 행에 대 한 않으려면 또는 `false` 사용자가 행을 선택 하려면 모든 행에 대 한 합니다.

테이블 뷰 (`NSTableView`) 행 선택 사용 하기 위한 다음 메서드를 포함 합니다.

- `DeselectRow(nint)` -테이블의 지정된 된 행을 선택 취소합니다.
- `SelectRow(nint,bool)` -지정된 된 행을 선택합니다. 전달 `false` 에 대 한 두 번째 매개 변수를 한 번에 하나의 행을 선택 합니다.
- `SelectedRow` -현재 테이블에서 선택한 행을 반환 합니다.
- `IsRowSelected(nint)` -반환 `true` 지정된 된 행을 선택 합니다.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>여러 개의 행 선택

사용자가 두 번 클릭을 여러 행을 선택 하도록 허용 하려는 경우는 `Main.storyboard` 인터페이스 작성기에서 편집을 위해 열 파일입니다. 테이블 뷰를 선택는 **인터페이스 계층 구조** 확인 하 고는 **여러** 확인란을 선택은 **특성 검사기**:

[![](table-view-images/select02.png "특성 검사기")](table-view-images/select02.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.


다음에 편집는 `ProductTableDelegate.cs` 파일을 다음 메서드를 추가 합니다.

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

이렇게 하면 사용자가 테이블 보기에서 임의의 단일 행을 선택할 수 있습니다. 반환할 `false` 에 대 한는 `ShouldSelectRow` 사용자를 선택할 수 있는 모든 행에 대 한 않으려면 또는 `false` 사용자가 행을 선택 하려면 모든 행에 대 한 합니다.

테이블 뷰 (`NSTableView`) 행 선택 사용 하기 위한 다음 메서드를 포함 합니다.

- `DeselectAll(NSObject)` -테이블의 모든 행을 선택 취소합니다. 사용 하 여 `this` 는 첫 번째 전송할 매개 변수를 선택 하 고 수행 하는 개체에 대 한 합니다. 
- `DeselectRow(nint)` -테이블의 지정된 된 행을 선택 취소합니다.
- `SelectAll(NSobject)` -테이블의 모든 행을 선택 합니다. 사용 하 여 `this` 는 첫 번째 전송할 매개 변수를 선택 하 고 수행 하는 개체에 대 한 합니다.
- `SelectRow(nint,bool)` -지정된 된 행을 선택합니다. 전달 `false` 두 번째 매개 변수는 선택 항목을 선택 취소 하 고 단일 행만 선택, 전달 `true` 하 선택 영역을 확장 하 고이 행을 포함 합니다.
- `SelectRows(NSIndexSet,bool)` -지정 된 행 집합을 선택합니다. 전달 `false` 두 번째 매개 변수에 대 한 선택을 취소 하 고 선택만 이러한 행을 전달 `true` 하 선택 영역을 확장 하 고 이러한 행을 포함 합니다.
- `SelectedRow` -현재 테이블에서 선택한 행을 반환 합니다.
- `SelectedRows` -반환는 `NSIndexSet` 선택 된 행의 인덱스를 포함 하 합니다.
- `SelectedRowCount` -선택 된 행의 수를 반환합니다.
- `IsRowSelected(nint)` -반환 `true` 지정된 된 행을 선택 합니다.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>행을 선택 하는 형식

사용자가 테이블 보기를 선택 하는 문자를 입력 하도록 허용 하 고 첫 번째 행을 선택 하려면 해당 문자를 두 번 클릭 하 고 `Main.storyboard` 인터페이스 작성기에서 편집을 위해 열 파일입니다. 테이블 뷰를 선택는 **인터페이스 계층 구조** 확인 하 고는 **유형 선택** 확인란을 선택은 **특성 검사기**:

[![](table-view-images/type01.png "선택 유형을 설정합니다.")](table-view-images/type01.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

이제 편집는 `ProductTableDelegate.cs` 파일을 다음 메서드를 추가 합니다.

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

`GetNextTypeSelectMatch` 메서드는 지정 된 `searchString` 첫 번째 행을 반환 하 고 `Product` 하에 해당 문자열의 `Title`합니다.

응용 프로그램을 실행 하 고 문자를 입력, 하나의 행을 선택 합니다.

[![](table-view-images/type02.png "샘플 응용 프로그램 실행")](table-view-images/type02.png#lightbox)

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>열 다시 정렬

표 보기에서 열 순서 바꾸기 끌어서 놓을 수 있도록 하려는 경우, 두 번 클릭 하 고 `Main.storyboard` 인터페이스 작성기에서 편집을 위해 열 파일입니다. 테이블 뷰를 선택는 **인터페이스 계층 구조** 확인 하 고는 **Reordering** 확인란을 선택은 **특성 검사기**:

[![](table-view-images/reorder01.png "특성 검사기")](table-view-images/reorder01.png#lightbox)

에 대 한 값을 제공 하는 경우는 **자동 저장** 속성 및 검사는 **열 정보** 필드 테이블의 레이아웃을 변경 했습니다 위해 자동으로 저장 되 고 다음에 응용 프로그램 복원 실행 됩니다.

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

이제 편집는 `ProductTableDelegate.cs` 파일을 다음 메서드를 추가 합니다.

```csharp
public override bool ShouldReorder (NSTableView tableView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

`ShouldReorder` 메서드를 반환 하도록 `true` 수 있도록 하려는 모든 열에 대 한으로 다시 정렬 끌어서는 `newColumnIndex`, 그렇지 않으면 return `false`;

응용 프로그램을 실행 하는 경우 열 순서를 변경 하려면 열 머리글 주위 끌어서:

[![](table-view-images/reorder02.png "다시 정렬 된 열의 예")](table-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>셀 편집

사용자 지정된 된 셀에 대 한 값을 편집, 편집 하도록 허용 하려는 경우는 `ProductTableDelegate.cs` 파일 및 변경 된 `GetViewForItem` 메서드를 다음과 같이:

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

이제 응용 프로그램을 실행 하는 경우 사용자 테이블 보기에 있는 셀을 편집할 수 있습니다.:

[![](table-view-images/editing01.png "셀 편집의 예")](table-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Table_Views" />

## <a name="using-images-in-table-views"></a>표 보기에서 이미지를 사용 하 여

에 있는 셀의 일부로 이미지를 포함 하는 `NSTableView`, 표 보기에서 데이터 반환 되는 방법을 변경 하려면 필요 합니다 `NSTableViewDelegate's` `GetViewForItem` 메서드를 사용 하 여는 `NSTableCellView` 일반적인 대신 `NSTextField`합니다. 예를 들어:

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

자세한 내용은 참조 하십시오는 [테이블 뷰를 사용 하 여 이미지를 사용 하 여](~/mac/app-fundamentals/image.md) 의 섹션 우리의 [이미지 작업](~/mac/app-fundamentals/image.md) 설명서입니다.

<a name="Adding-a-Delete-Button-to-a-Row" />

## <a name="adding-a-delete-button-to-a-row"></a>행에 삭제 단추 추가

응용 프로그램의 요구에 따라, 경우도 테이블의 각 행에 대 한 실행 단추를 제공 해야 합니다. 이 예를 들어 포함 하도록 앞에서 만든 테이블 뷰 예제를 확장 해 보겠습니다는 **삭제** 각 행에는 단추입니다.

먼저, 편집 하는 `Main.storyboard` Xcode의 인터페이스 작성기에서 테이블 뷰를 선택 하 고 작업을 열 수를 늘리려면 3 (3). 다음으로 변경 된 **제목** 에 새 열의 `Action`:

[![](table-view-images/delete01.png "열 이름 편집")](table-view-images/delete01.png#lightbox)

스토리 보드의 변경 내용을 저장 하 고 변경 내용을 동기화 하는 Mac에 대 한 Visual Studio로 돌아갑니다.

다음에 편집는 `ViewController.cs` 파일을 다음 공용 메서드를 추가 합니다.

```csharp
public void ReloadTable ()
{
    ProductTable.ReloadData ();
}
```

동일한 파일에서 내에 새 테이블 보기 대리자를 만들 수정 `ViewDidLoad` 메서드를 다음과 같이 합니다.

```csharp
// Populate the Product Table
ProductTable.DataSource = DataSource;
ProductTable.Delegate = new ProductTableDelegate (this, DataSource);
```

이제, 편집 하는 `ProductTableDelegate.cs` 보기 컨트롤러에 개인 연결을 포함 하 고 대리자의 새 인스턴스를 만들 때 컨트롤러를 매개 변수로 하도록 파일:

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

다음으로 클래스에 다음 새 전용 메서드를 추가 합니다.

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

이 작업에 걸리는 모든에서 이전에 수행 된 텍스트 보기 구성을 `GetViewForItem` 메서드 (없으므로 테이블의 마지막 열 텍스트 볼 수 있지만 단추)을 호출할 수를 단일 위치에 배치 합니다.

마지막으로 편집는 `GetViewForItem` 메서드 다음과 같이 만듭니다.

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

이 코드를 자세히의 여러 섹션에서 살펴보겠습니다. 첫 번째, 새 `NSTableViewCell` 은 만드는 작업은 기반으로 수행는 열의 이름입니다. 처음 두 열에 대 한 (**제품** 및 **세부 정보**), 새 `ConfigureTextField` 메서드를 호출 합니다.

에 대 한는 **작업** 열, 새 `NSButton` 만들어지고 하위 보기가으로 셀에 추가 됩니다.

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

단추의 `Tag` 속성은 현재 처리 중인 행의 수를 보유 하는 데 사용 합니다. 이 숫자는 나중에 사용자 단추의 삭제 될 행을 요청 하면 `Activated` 이벤트:

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

이벤트 처리기의 시작 부분에 단추와 지정 된 테이블 행에 있는 제품을 가져올 했습니다. 다음 행 삭제를 확인할 사용자에 게 경고가 표시 됩니다. 사용자가 행을 삭제 하도록 지정된 된 행 데이터 소스에서 제거 되 고 테이블을 다시 로드:

```csharp
// Remove the given row from the dataset
DataSource.Products.RemoveAt((int)btn.Tag);
Controller.ReloadTable ();
```

마지막으로, 표 보기 셀 새로 만드는 대신를 재사용 하는 경우 다음 코드 처리 되 고 열에 따라 구성 합니다.

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

에 대 한는 **동작** 열에서 모든 하위 보기까지 검색 되는 `NSButton` 발견 되는 `Tag` 속성이 현재 행을 가리키도록 업데이트 됩니다.

이러한 변경을 앱이 실행 되는 경우 각 행에는 필요가 **삭제** 단추:

[![](table-view-images/delete02.png "삭제 단추와 함께 테이블 뷰")](table-view-images/delete02.png#lightbox)

사용자가 클릭할 때 한 **삭제** 단추를 지정된 된 행을 삭제 하도록 요청 경고가 표시 됩니다.

[![](table-view-images/delete03.png "Delete 행 경고")](table-view-images/delete03.png#lightbox)

삭제를 선택 하는 경우 행 제거 되 고 테이블 다시 그려집니다.

[![](table-view-images/delete04.png "테이블 행이 삭제 한 후")](table-view-images/delete04.png#lightbox)

<a name="Data_Binding_Table_Views" />

## <a name="data-binding-table-views"></a>데이터 바인딩 테이블 뷰

키-값 코딩 및 데이터 바인딩 기술을 Xamarin.Mac 응용 프로그램 사용 하 여 작성 및 유지를 채우고 UI 요소를 사용 해야 하는 코드의 양을 크게 줄일 수 있습니다. 추가 백업 데이터를 분리 하는 이점이 있습니다 (_데이터 모델_) 사용자 인터페이스를 종료 하면 앞에서 (_모델-뷰-컨트롤러_)를 보다 융통성 있는 응용 프로그램 유지 관리 디자인 합니다.

키-값 코딩 (KVC)은 인스턴스 변수를 통해 액세스 하는 대신 속성을 식별 하 키 (특별 한 형식의 문자열) 또는 접근자 메서드를 사용 하 여, 개체의 속성을 직접 액세스 메커니즘 (`get/set`). Xamarin.Mac 응용 프로그램에서 키-값 코딩 규격 접근자를 구현 하 여 키 값을 관찰 (KVO), 데이터 바인딩, 핵심 데이터, Cocoa 바인딩 및 스크립팅 등의 기타 macOS 기능에 액세스할을 수 있습니다.

자세한 내용은 참조 하십시오는 [테이블 뷰 데이터 바인딩](~/mac/app-fundamentals/databinding.md#Table_View_Data_Binding) 섹션 우리의 [데이터 바인딩 및 코딩을 키-값](~/mac/app-fundamentals/databinding.md) 설명서입니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에 자세히 살펴보고 Xamarin.Mac 응용 프로그램에서 테이블 뷰 작업 수행 했습니다. 다양 한 유형을 보여 준다는 사실을 알았습니다 하 고 테이블 보기, 만들기 및 Xcode의 인터페이스 작성기에서 테이블 뷰를 유지 관리 하는 방법 및 C# 코드에서 테이블 뷰를 사용 하는 방법을 사용 합니다.

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
