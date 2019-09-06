---
title: Xamarin.ios의 테이블 뷰
description: 이 문서에서는 Xamarin.ios 응용 프로그램의 테이블 뷰 작업을 설명 합니다. Xcode 및 Interface Builder에서 테이블 뷰를 만들고 코드에서이를 조작 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 3B55B858-4769-4331-966A-7F53B3B7C720
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 758134b0c5171e46c47ff6fd8071b13a44d5789b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291617"
---
# <a name="table-views-in-xamarinmac"></a>Xamarin.ios의 테이블 뷰

_이 문서에서는 Xamarin.ios 응용 프로그램의 테이블 뷰 작업을 설명 합니다. Xcode 및 Interface Builder에서 테이블 뷰를 만들고 코드에서이를 조작 하는 방법을 설명 합니다._

Xamarin.ios 응용 프로그램 C# 에서 및 .net을 사용 하는 경우 *목표-C* 및 *Xcode* 에서 작업 하는 개발자가 동일한 테이블 뷰에 액세스할 수 있습니다. Xamarin.ios는 Xcode와 직접 통합 되므로 Xcode의 _Interface Builder_ 를 사용 하 여 테이블 뷰를 만들고 유지 관리 하거나 (필요에 따라 코드에서 C# 직접 만들 수 있습니다.)

테이블 뷰는 여러 행의 정보 열을 하나 이상 포함 하는 테이블 형식으로 데이터를 표시 합니다. 생성 되는 테이블 뷰의 유형에 따라 사용자는 열 기준으로 정렬, 열 다시 구성, 열 추가, 열 제거 또는 테이블 내에 포함 된 데이터 편집을 수행할 수 있습니다.

[![](table-view-images/intro01.png "예제 테이블")](table-view-images/intro01.png#lightbox)

이 문서에서는 Xamarin.ios 응용 프로그램의 테이블 뷰 작업에 대 한 기본 사항을 다룹니다. [Hello, Mac](~/mac/get-started/hello-mac.md) 문서를 먼저 사용 하는 것이 가장 좋습니다. 특히 [Xcode 및 Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 및 [콘센트 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션을 소개 하 고,에서 사용할 주요 개념 및 기술을 설명 하 고 있습니다. 이 문서를 참조 하세요.

[Xamarin.ios 내부](~/mac/internals/how-it-works.md) 문서의 [클래스/메서드를 대상으로 노출 C# -C](~/mac/internals/how-it-works.md) 섹션을 살펴볼 수 있습니다. C# 클래스를 연결 하는 데 사용 되는 `Register` 및 `Export` 명령을 설명 합니다. 목표-C 개체 및 UI 요소입니다.

<a name="Introduction_to_Table_Views" />

## <a name="introduction-to-table-views"></a>테이블 뷰 소개

테이블 뷰는 여러 행의 정보 열을 하나 이상 포함 하는 테이블 형식으로 데이터를 표시 합니다. 테이블 뷰는 스크롤 뷰 (`NSScrollView`) 내에 표시 되 고 macos 10.7부터 시작 하 여 행과 열을 모두 표시 하는 대신 any `NSView` Cells (`NSCell`)을 사용할 수 있습니다. 그러나 계속 해 서 사용할 `NSCell` 수 있지만 일반적으로 사용자 지정 행과 열을 서브클래싱하 `NSTableCellView` 고 만듭니다.

테이블 뷰에서는 자체 데이터를 저장 하지 않고 데이터 원본 (`NSTableViewDataSource`)을 사용 하 여 필요한 경우 필요한 행과 열을 모두 제공 합니다.

테이블 뷰 대리자 (`NSTableViewDelegate`)의 하위 클래스를 제공 하 여 테이블 열 관리를 지원 하 고, 기능을 선택 하 고, 열을 선택 하 고 편집 하 고, 사용자 지정 추적을 제공 하 고, 개별 열에 대 한 사용자 지정 열.

테이블 뷰를 만들 때 Apple에서는 다음을 제안 합니다.

- 사용자가 열 머리글을 클릭 하 여 테이블을 정렬할 수 있습니다.
- 해당 열에 표시 되는 데이터를 설명 하는 명사 또는 짧은 명사 구에 열 머리글을 만듭니다.

자세한 내용은 Apple [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)의 [콘텐츠 보기](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) 섹션을 참조 하세요.

<a name="Creating-and-Maintaining-Table-Views-in-Xcode" />

## <a name="creating-and-maintaining-table-views-in-xcode"></a>Xcode에서 테이블 뷰 만들기 및 유지 관리

새 Xamarin.ios Cocoa 응용 프로그램을 만들면 기본적으로 표준 빈 창이 표시 됩니다. 이 창은 프로젝트에 자동으로 `.storyboard` 포함 되는 파일에 정의 됩니다. Windows 디자인을 편집 하려면 **솔루션 탐색기**에서 `Main.storyboard` 파일을 두 번 클릭 합니다.

[![](table-view-images/edit01.png "주 스토리 보드 선택")](table-view-images/edit01.png#lightbox)

이렇게 하면 Xcode의 Interface Builder에서 창 디자인이 열립니다.

[![](table-view-images/edit02.png "Xcode에서 UI 편집")](table-view-images/edit02.png#lightbox)

`table` **라이브러리 검사기의** 검색 상자에를 입력 하 여 테이블 뷰 컨트롤을 보다 쉽게 찾을 수 있습니다.

[![](table-view-images/edit03.png "라이브러리에서 테이블 뷰 선택")](table-view-images/edit03.png#lightbox)

**인터페이스 편집기**에서 뷰 컨트롤러로 테이블 뷰를 끌고, 뷰 컨트롤러의 콘텐츠 영역에 맞게 설정 하 고, **제약 조건 편집기**에서 창으로 축소 하 고 증가 하는 위치로 설정 합니다.

[![](table-view-images/edit04.png "제약 조건 편집")](table-view-images/edit04.png#lightbox)

**인터페이스 계층 구조** 에서 테이블 뷰를 선택 하면 **특성 검사자**에서 다음 속성을 사용할 수 있습니다.

[![](table-view-images/edit05.png "특성 검사자")](table-view-images/edit05.png#lightbox)

- **내용 모드** -뷰 (`NSView`) 또는 셀 (`NSCell`) 중 하나를 사용 하 여 행과 열에 데이터를 표시할 수 있습니다. MacOS 10.7부터 보기를 사용 해야 합니다.
- **Float 그룹 행** -인 `true`경우 테이블 뷰에서 그룹화 된 셀이 부동 된 것 처럼 그려집니다.
- **Columns** -표시 되는 열 수를 정의 합니다.
- **Headers** -이면 `true`열에 머리글이 포함 됩니다.
- 다시 **정렬** - `true`이면 사용자가 테이블의 열 순서를 끌 수 있습니다.
- **크기 조정** - `true`이면 사용자가 열 머리글을 끌어 열 크기를 조정할 수 있습니다.
- **열 크기 조정** -테이블에서 열 크기를 자동으로 조정 하는 방법을 제어 합니다.
- **강조 표시** -셀이 선택 될 때 테이블에서 사용 하는 강조 표시 유형을 제어 합니다.
- **교대로** 반복 되는 `true`행-다른 행의 배경색이 달라 집니다.
- **가로 그리드** -셀 사이에 그려진 테두리의 형식을 선택 합니다.
- **세로 그리드** -셀 사이에 그려진 테두리의 형식을 선택 합니다.
- **Grid color** -셀 테두리 색을 설정 합니다.
- **배경** -셀 배경색을 설정 합니다.
- **선택** -사용자가 테이블의 셀을 선택 하는 방법을 제어할 수 있습니다.
  - **Multiple** -이면 `true`사용자가 여러 행과 열을 선택할 수 있습니다.
  - **열** -이면 `true`사용자가 열을 선택할 수 있습니다.
  - **Select** -If `true`를 입력 하 고 사용자가 문자를 입력 하 여 행을 선택할 수 있습니다.
  - **Empty** -인 `true`경우 사용자가 행 또는 열을 선택할 필요가 없으면 테이블에서 선택할 수 없습니다.
- **자동 저장-테이블** 형식이 자동으로 저장 되는 이름입니다.
- **열 정보** -이면 `true`열의 순서와 너비가 자동으로 저장 됩니다.
- **줄 바꿈** -셀에서 줄 바꿈을 처리 하는 방법을 선택 합니다.
- **마지막으로 표시 되** 는 줄 `true`을 자릅니다.-인 경우 데이터에서 잘린 셀은 해당 범위 내에 맞지 않을 수 있습니다.

> [!IMPORTANT]
> 레거시 xamarin.ios 응용 프로그램을 유지 관리 하지 않는 경우 기반 `NSView` 테이블 뷰를 `NSCell` 기반으로 테이블 뷰를 사용 해야 합니다. `NSCell`는 레거시로 간주 되며 향후 지원 되지 않을 수 있습니다.

**인터페이스 계층 구조** 에서 테이블 열을 선택 하면 **특성 검사자**에서 다음 속성을 사용할 수 있습니다.

[![](table-view-images/edit06.png "특성 검사자")](table-view-images/edit06.png#lightbox)

- **제목** -열의 제목을 설정 합니다.
- **맞춤** -셀 내의 텍스트 맞춤을 설정 합니다.
- **제목 글꼴** -셀 머리글 텍스트의 글꼴을 선택 합니다.
- **정렬 키** -열의 데이터를 정렬 하는 데 사용 되는 키입니다. 사용자가이 열을 정렬할 수 없으면 비워 둡니다.
- **Selector** -정렬을 수행 하는 데 사용 되는 **동작** 입니다. 사용자가이 열을 정렬할 수 없으면 비워 둡니다.
- **Order** -열 데이터의 정렬 순서입니다.
- **크기 조정** -열의 크기 조정 유형을 선택 합니다.
- **편집 가능** - `true`이면 사용자가 셀 기반 테이블의 셀을 편집할 수 있습니다.
- **Hidden** -이면 `true`열이 숨겨집니다.

왼쪽 또는 오른쪽의 핸들 (열 오른쪽 가운데 세로 방향)을 끌어서 열 크기를 조정할 수도 있습니다.

테이블 뷰에서 각 열을 선택 하 고 첫 번째 열에의 `Product` **제목** `Details`및 두 번째 열을 지정 해 보겠습니다.

**인터페이스 계층 구조** 에서 테이블 셀`NSTableViewCell`뷰 ()를 선택 하면 **특성 검사자**에서 다음 속성을 사용할 수 있습니다.

[![](table-view-images/edit07.png "특성 검사자")](table-view-images/edit07.png#lightbox)

이러한 속성은 모두 표준 보기의 속성입니다. 이 열에 대 한 행의 크기를 조정 하는 옵션도 있습니다.

`NSTextField` **인터페이스 계층 구조** 에서 테이블 뷰 셀 (기본적으로는)을 선택 하면 **특성 검사자**에서 다음 속성을 사용할 수 있습니다.

[![](table-view-images/edit08.png "특성 검사자")](table-view-images/edit08.png#lightbox)

여기에서 설정할 표준 텍스트 필드의 모든 속성을 갖게 됩니다. 기본적으로 표준 텍스트 필드는 열에 있는 셀에 대 한 데이터를 표시 하는 데 사용 됩니다.

**인터페이스 계층 구조** 에서 테이블 셀`NSTableFieldCell`뷰 ()를 선택 하면 **특성 검사자**에서 다음 속성을 사용할 수 있습니다.

[![](table-view-images/edit09.png "특성 검사자")](table-view-images/edit09.png#lightbox)

가장 중요 한 설정은 다음과 같습니다.

- **레이아웃** -이 열에 있는 셀의 레이아웃을 선택 하는 방법을 선택 합니다.
- **단일 줄 모드를 사용** 합니다 `true`. 인 경우 셀이 한 줄로 제한 됩니다.
- **첫 번째 런타임 레이아웃 너비** - `true`인 경우 셀은 응용 프로그램을 처음 실행할 때 표시 되는 너비 (수동 또는 자동)를 선호 합니다.
- **Action** -셀에 대 한 편집 **작업** 을 보내는 시기를 제어 합니다.
- **동작** -셀을 선택할 수 있는지 또는 편집할 수 있는지를 정의 합니다.
- **서식 있는 텍스트** - `true`인 경우 셀에 서식 지정 및 스타일 지정 된 텍스트가 표시 될 수 있습니다.
- **Undo** -이면 `true`셀에서 실행 취소 동작을 담당 하는 것으로 가정 합니다.

**인터페이스 계층 구조**에서 테이블 열의`NSTableFieldCell`맨 아래에 있는 테이블 셀 뷰 ()를 선택 합니다.

[![](table-view-images/edit10.png "테이블 셀 보기 선택")](table-view-images/edit10.png#lightbox)

이렇게 하면 지정 된 열에 대해 생성 된 모든 셀의 기본 _패턴_ 으로 사용 되는 테이블 셀 뷰를 편집할 수 있습니다.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>작업 및 콘센트 추가

다른 Cocoa UI 컨트롤과 마찬가지로, **작업** 및 **콘센트** 를 사용 하 여 코드를 표시 하는 데 필요한 기능을 C# 기준으로 테이블 뷰 및 열과 셀을 표시 해야 합니다.

이 프로세스는 노출 하려는 모든 테이블 뷰 요소에 대해 동일 합니다.

1. **길잡이 편집기** 로 전환 하 여 `ViewController.h` 파일이 선택 되었는지 확인 합니다. 

    [![](table-view-images/edit11.png "길잡이 편집기")](table-view-images/edit11.png#lightbox)
2. **인터페이스 계층 구조**에서 테이블 뷰를 선택 하 고 컨트롤을 클릭 한 다음 `ViewController.h` 파일을 끕니다.
3. 이라는`ProductTable`테이블 뷰에 대 한 **콘센트** 를 만듭니다. 

    [![](table-view-images/edit13.png "콘센트 구성")](table-view-images/edit13.png#lightbox)
4. `ProductColumn` 및`DetailsColumn`라는 테이블 열에 대 한 콘센트를 만듭니다. 

    [![](table-view-images/edit14.png "콘센트 구성")](table-view-images/edit14.png#lightbox)
5. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

다음으로, 응용 프로그램을 실행할 때 테이블에 대 한 일부 데이터를 표시 하는 코드를 작성 합니다.

<a name="Populating_the_Table_View" />

## <a name="populating-the-table-view"></a>테이블 뷰 채우기

Interface Builder에서 디자인 되었으며 **콘센트**를 통해 노출 되는 테이블 뷰를 사용 하 여 다음 C# 코드를 작성 해야 합니다.

먼저 개별 행에 대 한 정보 `Product` 를 보유할 새 클래스를 만들어 보겠습니다. **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다. `Product` **일반**빈 클래스를 선택 하 고 이름으로를 입력 한 다음 새로 만들기 단추를 클릭 합니다. > 

[![](table-view-images/populate01.png "빈 클래스 만들기")](table-view-images/populate01.png#lightbox)

파일이 다음과 `Product.cs` 같이 표시 되도록 합니다.

```csharp
using System;

namespace MacTables
{
  public class Product
  {
    #region Computed Properties
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

다음으로, 요청 된 테이블에 대 한 `NSTableDataSource` 데이터를 제공 하는의 하위 클래스를 만들어야 합니다. **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다. `ProductTableDataSource` **일반**빈 클래스를 선택 하 고 이름으로를 입력 한 다음 새로 만들기 단추를 클릭 합니다. > 

`ProductTableDataSource.cs` 파일을 편집 하 여 다음과 같이 만듭니다.

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

이 클래스는 테이블 뷰의 항목에 대 한 저장소를 포함 하 `GetRowCount` 고를 재정의 하 여 테이블의 행 수를 반환 합니다.

마지막으로의 `NSTableDelegate` 서브 클래스를 만들어 테이블의 동작을 제공 해야 합니다. **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다. `ProductTableDelegate` **일반**빈 클래스를 선택 하 고 이름으로를 입력 한 다음 새로 만들기 단추를 클릭 합니다. > 

`ProductTableDelegate.cs` 파일을 편집 하 여 다음과 같이 만듭니다.

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

인스턴스 `ProductTableDelegate`를 만들 때 테이블에 대 한 데이터를 제공 `ProductTableDataSource` 하는의 인스턴스도 전달 합니다. 메서드 `GetViewForItem` 는 열 및 행에 대 한 셀을 표시 하기 위해 뷰 (데이터)를 반환 합니다. 가능 하면 새 보기를 만들어야 하는 경우 기존 뷰를 다시 사용 하 여 셀을 표시 합니다.

테이블을 채우기 위해 `ViewController.cs` 파일을 편집 하 고 메서드를 `AwakeFromNib` 다음과 같이 만듭니다.

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

## <a name="sorting-by-column"></a>열별로 정렬

사용자가 열 머리글을 클릭 하 여 테이블의 데이터를 정렬할 수 있습니다. 먼저 `Main.storyboard` 파일을 두 번 클릭 하 여 Interface Builder에서 편집할 수 있도록 엽니다. `Title` `compare:` `Ascending`열을 선택 하 고, 정렬 키로를 입력 하 고, 선택기에 대해를 입력 하 고, 순서를 선택 합니다. `Product`

[![](table-view-images/sort01.png "정렬 키 설정")](table-view-images/sort01.png#lightbox)

`Description` `compare:` `Ascending`열을 선택 하 고, 정렬 키로를 입력 하 고, 선택기에 대해를 입력 하 고, 순서를 선택 합니다. `Details`

[![](table-view-images/sort02.png "정렬 키 설정")](table-view-images/sort02.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

이제 파일을 `ProductTableDataSource.cs` 편집 하 고 다음 메서드를 추가 해 보겠습니다.

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

메서드 `Sort` 를 사용 하면 지정 `Product` 된 클래스 필드를 기반으로 하 여 데이터 소스의 데이터를 오름차순 또는 내림차순으로 정렬할 수 있습니다. 재정의 `SortDescriptorsChanged` 된 메서드는가 열 머리글을 클릭할 때마다 호출 됩니다. Interface Builder에 설정 된 **키** 값과 해당 열에 대 한 정렬 순서를 전달 합니다.

응용 프로그램을 실행 하 고 열 머리글을 클릭 하면 해당 열을 기준으로 행이 정렬 됩니다.

[![](table-view-images/sort03.png "예제 앱 실행")](table-view-images/sort03.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>행 선택

사용자가 단일 행을 선택할 수 있도록 하려면 `Main.storyboard` 파일을 두 번 클릭 하 여 Interface Builder에서 편집할 수 있도록 엽니다. **인터페이스 계층 구조** 에서 테이블 뷰를 선택 하 고 **특성 검사자**에서 **여러** 확인란의 선택을 취소 합니다.

[![](table-view-images/select01.png "특성 검사자")](table-view-images/select01.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.


그런 다음 `ProductTableDelegate.cs` 파일을 편집 하 고 다음 메서드를 추가 합니다.

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
  return true;
}
```

이렇게 하면 사용자가 테이블 뷰에서 단일 행을 선택할 수 있습니다. 사용자가 행을 선택할 수 없도록 하려는 경우 사용자가 선택할 수 없도록 하려는 행 이나 `false` `false` 모든 행에 대해를 반환 합니다. `ShouldSelectRow`

테이블 뷰 (`NSTableView`)에는 행 선택 작업을 위한 다음 메서드가 포함 되어 있습니다.

- `DeselectRow(nint)`-테이블에서 지정 된 행을 선택 취소 합니다.
- `SelectRow(nint,bool)`-지정 된 행을 선택 합니다. 두 `false` 번째 매개 변수에 대해를 전달 하 여 한 번에 하나의 행만 선택 합니다.
- `SelectedRow`-테이블에서 선택한 현재 행을 반환 합니다.
- `IsRowSelected(nint)`-지정 `true` 된 행이 선택 된 경우을 반환 합니다.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>여러 행 선택

사용자가 여러 행을 선택할 수 있도록 하려면 `Main.storyboard` 파일을 두 번 클릭 하 여 Interface Builder에서 편집할 수 있도록 엽니다. **인터페이스 계층 구조** 에서 테이블 뷰를 선택 하 고 **특성 검사자**에서 **여러** 확인란을 선택 합니다.

[![](table-view-images/select02.png "특성 검사자")](table-view-images/select02.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.


그런 다음 `ProductTableDelegate.cs` 파일을 편집 하 고 다음 메서드를 추가 합니다.

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
  return true;
}
```

이렇게 하면 사용자가 테이블 뷰에서 단일 행을 선택할 수 있습니다. 사용자가 행을 선택할 수 없도록 하려는 경우 사용자가 선택할 수 없도록 하려는 행 이나 `false` `false` 모든 행에 대해를 반환 합니다. `ShouldSelectRow`

테이블 뷰 (`NSTableView`)에는 행 선택 작업을 위한 다음 메서드가 포함 되어 있습니다.

- `DeselectAll(NSObject)`-테이블의 모든 행을 선택 취소 합니다. 첫 `this` 번째 매개 변수를 사용 하 여를 선택 하는 개체에서 보냅니다. 
- `DeselectRow(nint)`-테이블에서 지정 된 행을 선택 취소 합니다.
- `SelectAll(NSobject)`-테이블의 모든 행을 선택 합니다. 첫 `this` 번째 매개 변수를 사용 하 여를 선택 하는 개체에서 보냅니다.
- `SelectRow(nint,bool)`-지정 된 행을 선택 합니다. 두 `false` 번째 매개 변수를 전달 하 여 선택 영역을 지우고 단일 행 `true` 만 선택 하 여 선택 영역을 확장 하 고이 행을 포함 합니다.
- `SelectRows(NSIndexSet,bool)`-지정 된 행 집합을 선택 합니다. 두 `false` 번째 매개 변수를 전달 하 여 선택을 취소 하 고 이러한 행 `true` 만 선택 하 여 선택 영역을 확장 하 고 이러한 행을 포함 합니다.
- `SelectedRow`-테이블에서 선택한 현재 행을 반환 합니다.
- `SelectedRows`-선택한 행 `NSIndexSet` 의 인덱스를 포함 하는을 반환 합니다.
- `SelectedRowCount`-선택한 행의 수를 반환 합니다.
- `IsRowSelected(nint)`-지정 `true` 된 행이 선택 된 경우을 반환 합니다.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>입력 하 여 행 선택

사용자가 선택한 테이블 뷰로 문자를 입력 하 고 해당 문자가 포함 된 첫 번째 행을 선택할 수 있도록 하려면 `Main.storyboard` 파일을 두 번 클릭 하 여 Interface Builder에서 편집할 수 있도록 엽니다. **인터페이스 계층 구조** 에서 테이블 뷰를 선택 하 고 **특성 검사자**에서 **유형 선택** 확인란을 선택 합니다.

[![](table-view-images/type01.png "선택 유형 설정")](table-view-images/type01.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

이제 파일을 `ProductTableDelegate.cs` 편집 하 고 다음 메서드를 추가 해 보겠습니다.

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

메서드 `GetNextTypeSelectMatch` 는 지정 `searchString` 된를 가져와서 해당 문자열이 `Title`있는 첫 번째 `Product` 의 행을 반환 합니다.

응용 프로그램을 실행 하 고 문자를 입력 하면 행이 선택 됩니다.

[![](table-view-images/type02.png "샘플 앱 실행")](table-view-images/type02.png#lightbox)

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>열 다시 정렬

사용자가 테이블 뷰의 열 순서를 끌 수 있도록 하려면 `Main.storyboard` 파일을 두 번 클릭 하 여 Interface Builder에서 편집할 수 있도록 엽니다. **인터페이스 계층 구조** 에서 테이블 뷰를 선택 하 고 **특성 검사자**에서 다시 **정렬** 확인란을 선택 합니다.

[![](table-view-images/reorder01.png "특성 검사자")](table-view-images/reorder01.png#lightbox)

**자동** 저장 속성의 값을 지정 하 고 **열 정보** 필드를 확인 하는 경우 테이블의 레이아웃에 대 한 모든 변경 내용은 자동으로 저장 되 고 다음에 응용 프로그램이 실행 될 때 복원 됩니다.

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

이제 파일을 `ProductTableDelegate.cs` 편집 하 고 다음 메서드를 추가 해 보겠습니다.

```csharp
public override bool ShouldReorder (NSTableView tableView, nint columnIndex, nint newColumnIndex)
{
  return true;
}
```

메서드 `ShouldReorder` 는로 다시 `true` 정렬 `newColumnIndex`되도록 허용할 모든 열에 대해를 반환 해야 하며, 그렇지 않으면를 반환 `false`합니다.

응용 프로그램을 실행 하는 경우 열 머리글을 끌어 열의 순서를 바꿀 수 있습니다.

[![](table-view-images/reorder02.png "정렬 된 열의 예")](table-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>셀 편집

사용자가 지정 된 셀에 대 한 값을 편집할 수 있게 하려면 `ProductTableDelegate.cs` 파일을 편집 하 고 메서드를 `GetViewForItem` 다음과 같이 변경 합니다.

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

이제 응용 프로그램을 실행 하는 경우 사용자는 테이블 뷰의 셀을 편집할 수 있습니다.

[![](table-view-images/editing01.png "셀을 편집 하는 예")](table-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Table_Views" />

## <a name="using-images-in-table-views"></a>테이블 뷰에서 이미지 사용

에 `NSTableView`있는 셀의 일부로 이미지를 포함 하려면 테이블 `NSTableViewDelegate's` `GetViewForItem` 뷰의 메서드에서 데이터가 반환 되는 방법을 변경 하 여 일반적인 `NSTextField`대신를 `NSTableCellView` 사용 해야 합니다. 예:

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

자세한 내용은 이미지를 사용 하 여 [작업](~/mac/app-fundamentals/image.md) 설명서의 [테이블 뷰에서 이미지 사용](~/mac/app-fundamentals/image.md) 섹션을 참조 하세요.

<a name="Adding-a-Delete-Button-to-a-Row" />

## <a name="adding-a-delete-button-to-a-row"></a>행에 삭제 단추 추가

앱의 요구 사항에 따라 테이블의 각 행에 대 한 작업 단추를 제공 해야 하는 경우가 있을 수 있습니다. 이에 대 한 예로, 위에서 만든 테이블 뷰 예제를 확장 하 여 각 행에 **삭제** 단추를 포함 하겠습니다.

먼저 Xcode의 Interface Builder `Main.storyboard` 에서를 편집 하 고 테이블 뷰를 선택 하 여 열 수를 3 개로 늘립니다. 다음으로 새 열의 **제목을** 로 `Action`변경 합니다.

[![](table-view-images/delete01.png "열 이름 편집")](table-view-images/delete01.png#lightbox)

스토리 보드에 대 한 변경 내용을 저장 하 고 Mac용 Visual Studio로 돌아가서 변경 내용을 동기화 합니다.

그런 다음 `ViewController.cs` 파일을 편집 하 고 다음 공용 메서드를 추가 합니다.

```csharp
public void ReloadTable ()
{
  ProductTable.ReloadData ();
}
```

동일한 파일에서 `ViewDidLoad` 메서드 내에 있는 새 테이블 뷰 대리자의 생성을 다음과 같이 수정 합니다.

```csharp
// Populate the Product Table
ProductTable.DataSource = DataSource;
ProductTable.Delegate = new ProductTableDelegate (this, DataSource);
```

이제 `ProductTableDelegate.cs` 파일을 편집 하 여 뷰 컨트롤러에 대 한 개인 연결을 포함 하 고, 대리자의 새 인스턴스를 만들 때 컨트롤러를 매개 변수로 사용 합니다.

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

그런 다음 새 private 메서드를 클래스에 추가 합니다.

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

이렇게 하면 `GetViewForItem` 메서드에서 이전에 수행 된 모든 텍스트 뷰 구성이 사용 되며, 테이블의 마지막 열에는 텍스트 보기와 단추가 포함 되지 않기 때문에 호출 가능한 단일 위치에 배치 됩니다.

마지막으로 `GetViewForItem` 메서드를 편집 하 여 다음과 같이 만듭니다.

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
        var btn = sender as NSButton;
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
      var btn = subview as NSButton;
      if (btn != null) {
        btn.Tag = row;
      }
    }
    break;
  }

  return view;
}
```

이 코드의 몇 가지 섹션을 자세히 살펴보겠습니다. 첫째, 새 `NSTableViewCell` 을 만들 경우 열 이름을 기반으로 하는 작업이 수행 됩니다. 처음 두 열 (**제품** 및 **세부 정보**)에 대해 새 `ConfigureTextField` 메서드가 호출 됩니다.

**작업** 열에는 새 `NSButton` 가 만들어져 하위 뷰로 셀에 추가 됩니다.

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

단추의 `Tag` 속성은 현재 처리 중인 행의 번호를 유지 하는 데 사용 됩니다. 이 번호는 나중에 사용자가 단추의 `Activated` 이벤트에서 삭제할 행을 요청할 때 사용 됩니다.

```csharp
// Wireup events
button.Activated += (sender, e) => {
  // Get button and product
  var btn = sender as NSButton;
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

이벤트 처리기의 시작 부분에서 지정 된 테이블 행에 있는 단추와 제품을 가져옵니다. 그런 다음 사용자에 게 행 삭제를 확인 하는 경고가 표시 됩니다. 사용자가 행을 삭제 하도록 선택 하면 지정 된 행이 데이터 원본에서 제거 되 고 테이블이 다시 로드 됩니다.

```csharp
// Remove the given row from the dataset
DataSource.Products.RemoveAt((int)btn.Tag);
Controller.ReloadTable ();
```

마지막으로, 새 테이블 뷰 셀을 새로 만드는 대신 다시 사용 하는 경우 다음 코드는 처리 되는 열을 기준으로 구성 합니다.

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
    var btn = subview as NSButton;
    if (btn != null) {
      btn.Tag = row;
    }
  }
  break;
}

```

**작업** 열에 대 한 모든 하위 뷰는 `NSButton` 가 발견 될 때까지 검색 된 다음 `Tag` 현재 행을 가리키도록 속성을 업데이트 합니다.

이러한 변경 내용을 적용 하면 앱이 실행 될 때 각 행에 **삭제** 단추가 포함 됩니다.

[![](table-view-images/delete02.png "삭제 단추가 있는 테이블 뷰")](table-view-images/delete02.png#lightbox)

사용자가 **삭제** 단추를 클릭 하면 지정 된 행을 삭제 하 라는 경고가 표시 됩니다.

[![](table-view-images/delete03.png "행 삭제 경고")](table-view-images/delete03.png#lightbox)

사용자가 삭제를 선택 하면 해당 행이 제거 되 고 테이블이 다시 그려집니다.

[![](table-view-images/delete04.png "행이 삭제 된 후의 테이블")](table-view-images/delete04.png#lightbox)

<a name="Data_Binding_Table_Views" />

## <a name="data-binding-table-views"></a>데이터 바인딩 테이블 뷰

Xamarin.ios 응용 프로그램에서 키-값 코딩 및 데이터 바인딩 기술을 사용 하 여 UI 요소를 채우고 사용 하기 위해 작성 하 고 유지 관리 해야 하는 코드의 양을 크게 줄일 수 있습니다. 또한 프런트 엔드 사용자 인터페이스 (_모델-뷰-컨트롤러_)에서 지원 데이터 (_데이터 모델_)를 추가로 분리 하 여 더 쉽게 유지 관리 하 고 더욱 유연한 응용 프로그램을 디자인할 수 있는 이점을 누릴 수 있습니다.

KVC (키-값 코딩)는 키 (특수 형식의 문자열)를 사용 하 여 개체의 속성에 간접적으로 액세스 하 고 인스턴스 변수 또는 접근자 메서드 (`get/set`)를 통해 액세스 하는 대신 속성을 식별 하는 메커니즘입니다. Xamarin.ios 응용 프로그램에서 키-값 코딩 규격 접근자를 구현 하 여 키-값 관찰 (KVO), 데이터 바인딩, 코어 데이터, Cocoa 바인딩, scriptability 등의 다른 macOS 기능에 액세스할 수 있습니다.

자세한 내용은 [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md) 설명서의 [테이블 뷰 데이터 바인딩](~/mac/app-fundamentals/databinding.md#Table_View_Data_Binding) 섹션을 참조 하세요.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 응용 프로그램의 테이블 뷰로 작업 하는 방법에 대해 자세히 살펴봅니다. Xcode의 Interface Builder에서 테이블 뷰를 만들고 유지 관리 하는 방법 및 코드에서 C# 테이블 뷰를 사용 하는 방법에 대 한 다양 한 유형과 사용 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [MacTables (샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/mactables)
- [MacImages(샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/macimages)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [개요 보기](~/mac/user-interface/outline-view.md)
- [원본 목록](~/mac/user-interface/source-list.md)
- [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [NSTableView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSTableView_Class/index.html#//apple_ref/doc/uid/TP40004125)
- [NSTableViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSTableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008622)
- [NSTableViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSTableDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004178)
