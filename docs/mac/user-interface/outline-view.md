---
title: Xamarin.Mac의 개요 보기
description: 이 문서에서는 Xamarin.Mac 응용 프로그램에 대 한 개요 보기를 사용 하 여 작업을 설명 합니다. 만들고 유지 관리 개요 뷰가 Xcode 및 Interface Builder에서 프로그래밍 방식으로 작업을 설명 합니다.
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 7228a22eeae3dfdb354ba3693c277251fd2cd821
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "40251176"
---
# <a name="outline-views-in-xamarinmac"></a>Xamarin.Mac의 개요 보기

_이 문서에서는 Xamarin.Mac 응용 프로그램에 대 한 개요 보기를 사용 하 여 작업을 설명 합니다. 만들고 유지 관리 개요 뷰가 Xcode 및 Interface Builder에서 프로그래밍 방식으로 작업을 설명 합니다._

Xamarin.Mac 응용 프로그램에서 C# 및.NET을 사용 하 여 작업을 하는 경우 동일 하 게 액세스할 수 있습니다에서 작업 하는 개발자는 개요 뷰 *Objective-c* 하 고 *Xcode* 않습니다. Xcode의 Xamarin.Mac이 Xcode와 직접 통합 되므로 사용할 수 있습니다 _Interface Builder_ 만들기 및 개요 보기를 유지 관리 (또는 필요에 따라 C# 코드에서 바로 작성).

개요 보기를 사용자를 허용 하는 테이블 형식 확장 또는 계층적 데이터의 행을 축소 됩니다. 테이블 보기와 같은 개요 보기를 개별 항목과 해당 항목의 특성을 나타내는 열을 나타내는 행을 사용 하 여 관련된 항목 집합에 대 한 데이터를 표시 합니다. 표 보기와 달리 개요 보기에서 항목을 플랫 목록에 없는, 하드 드라이브의 파일 및 폴더와 같은 계층에 구성 된 합니다.

[![](outline-view-images/populate03.png "앱의 예 실행")](outline-view-images/populate03.png#lightbox)

이 문서에서는 Xamarin.Mac 응용 프로그램의 개요 보기를 사용 하는 기본 사항을 설명 합니다. 것이 가장 좋습니다를 통해 작업 하는 합니다 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서 합니다 [Xcode 및 Interface Builder 소개](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 하 고 [출 선 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션으로 주요 개념 및이 문서를 사용 하는 기술을 설명 합니다.

확인 하려는 경우는 [노출 C# 클래스 / objective-c 메서드](~/mac/internals/how-it-works.md) 섹션을 [Xamarin.Mac 내부 요소](~/mac/internals/how-it-works.md) 설명도 문서를 `Register` 및 `Export` 명령 하 여 통신-C# 클래스 Objective-c 개체 및 UI 요소에 사용 합니다.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-outline-views"></a>출 선 보기 소개

개요 보기를 사용자를 허용 하는 테이블 형식 확장 또는 계층적 데이터의 행을 축소 됩니다. 테이블 보기와 같은 개요 보기를 개별 항목과 해당 항목의 특성을 나타내는 열을 나타내는 행을 사용 하 여 관련된 항목 집합에 대 한 데이터를 표시 합니다. 표 보기와 달리 개요 보기에서 항목을 플랫 목록에 없는, 하드 드라이브의 파일 및 폴더와 같은 계층에 구성 된 합니다.

개요 뷰에서 항목에 다른 항목이 포함 된 경우에 확장 또는 사용자가 축소 수 있습니다. 확장 가능한 항목 항목 축소 되 고 항목 확장 되 면 중단 지점 때 오른쪽을 가리키는 삼각형을 표시 합니다. 공개 삼각형 클릭 하면 항목을 확장 하거나 축소 합니다.

개요 보기 (`NSOutlineView`) 테이블 보기의 서브 클래스 (`NSTableView`) 따라서 동작의 대부분이 부모 클래스에서 상속 합니다. 결과적으로, 테이블 등 뷰 행 또는 열을 선택 하면 열 머리글 등 끌어서 열 위치를 지 원하는 많은 작업을 개요 보기에서 지원 됩니다. Xamarin.Mac 응용 프로그램이 이러한 기능을 제어 하 고 허용 하거나 특정 작업 (코드 또는 Interface Builder에서 중 하나) 개요 보기의 매개 변수를 구성할 수 있습니다.

개요 보기는 자체 데이터를 저장 하지 않으므로, 데이터 원본에서 대신 (`NSOutlineViewDataSource`) 행과 열에는 필요에 따라 필요한 수 있도록 합니다.

개요 보기 대리자의 서브 클래스를 제공 하 여 개요 보기의 동작을 사용자 지정할 수 있습니다 (`NSOutlineViewDelegate`)를 지원 하기 위해 개요 열 관리 기능, 행 선택 및 편집, 사용자 지정 추적 및 개인에 대 한 사용자 지정 보기를 선택 하려면 입력 행 및 열입니다.

통해 이동 하려는 테이블 뷰를 사용 하 여 대부분의 동작 및 기능을 공유 하는 개요 뷰를, 이후 우리의 [테이블 뷰](~/mac/user-interface/table-view.md) 이 문서를 진행 하기 전에 설명서.

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode" />

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>만들기 및 개요 뷰가 Xcode에서 유지 관리

새 Xamarin.Mac Cocoa 응용 프로그램을 만들면 기본적으로 표준 비어 창을 가져옵니다. 이 windows에 정의 된를 `.storyboard` 파일 프로젝트에 자동으로 포함 합니다. windows 디자인에 맞게 편집 하는 **솔루션 탐색기**를 두 번 클릭 합니다 `Main.storyboard` 파일:

[![](outline-view-images/edit01.png "주 스토리 보드를 선택합니다.")](outline-view-images/edit01.png#lightbox)

Xcode의 Interface Builder에서 창 디자인을 엽니다.

[![](outline-view-images/edit02.png "Xcode에서 UI를 편집합니다.")](outline-view-images/edit02.png#lightbox)

형식 `outline` 에 **라이브러리 검사기** 개요 보기 컨트롤을 찾을 수 있도록 검색 상자:

[![](outline-view-images/edit03.png "개요 보기를 라이브러리에서 선택")](outline-view-images/edit03.png#lightbox)

개요 보기에서 뷰 컨트롤러 끌어를 **인터페이스 편집기**, 뷰 컨트롤러의 콘텐츠 영역을 입력 하 고 축소 하 고에서 창을 사용 하 여 증가를 설정 합니다 **제약 조건 편집기**:

[![](outline-view-images/edit04.png "제약 조건 편집")](outline-view-images/edit04.png#lightbox)

개요 보기에서 선택 합니다 **인터페이스 계층 구조** 있고 다음 속성을 사용할 수 있는 합니다 **특성 검사기**:

[![](outline-view-images/edit05.png "특성 검사기")](outline-view-images/edit05.png#lightbox)

- **열을 간략하게 설명** -표시 되는 계층적 데이터의 테이블 열입니다.
- **자동 개요 열** - `true`, 개요 열을 자동으로 저장 하 고 응용 프로그램 실행 사이의 복원 됩니다.
- **들여쓰기** -확장 된 항목 아래에 있는 열을 들여쓰기 하는 정도입니다.
- **들여쓰기 따릅니다 셀** - `true`, 셀 함께 들여쓰기 표시를 들여씁니다.
- **항목을 확장 하는 자동 저장** - `true`, 항목의 확장/축소 상태를 자동으로 저장 하 고 응용 프로그램 실행 사이의 복원 됩니다.
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
> 레거시 Xamarin.Mac 응용 프로그램을 유지 하는 경우가 아니면 `NSView` 기반된 개요 보기를 통해 사용할 `NSCell` 테이블 뷰를 기반으로 합니다. `NSCell` 레거시 간주 되 고 앞으로 지원 되지 않습니다.

테이블 열을 선택 합니다 **인터페이스 계층 구조** 에서 사용할 수 있는 다음 속성을 합니다 **특성 검사기**:

[![](outline-view-images/edit06.png "특성 검사기")](outline-view-images/edit06.png#lightbox)

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

[![](outline-view-images/edit07.png "특성 검사기")](outline-view-images/edit07.png#lightbox)

이들은 모두 표준 보기의 속성입니다. 여기에이 열에 대 한 행의 크기를 조정 하는 옵션이 있습니다.

테이블 뷰 셀을 선택 (기본적으로이를 `NSTextField`)에 **인터페이스 계층 구조** 되며 다음과 같은 속성에서 사용할 수 있습니다 합니다 **특성 검사기**:

[![](outline-view-images/edit08.png "특성 검사기")](outline-view-images/edit08.png#lightbox)

여기에서 설정 하는 표준 텍스트 필드의 모든 속성을 해야 합니다. 기본적으로 표준 텍스트 필드가 열에 셀에 대 한 데이터를 표시할 사용 됩니다.

테이블 셀 뷰를 선택 (`NSTableFieldCell`)에서 **인터페이스 계층 구조** 에서 사용할 수 있는 다음 속성을 **특성 검사기**:

[![](outline-view-images/edit09.png "특성 검사기")](outline-view-images/edit09.png#lightbox)

여기서 가장 중요 한 설정은 다음과 같습니다.

- **레이아웃** -이 열의 셀에에서 배치 하는 방법을 선택 합니다.
- **단일 줄 모드를 사용 하 여** - `true`, 셀이 한 줄으로 제한 합니다.
- **첫 번째 런타임 레이아웃 너비** - `true`, 셀을 설정 (수동 또는 자동으로) 경우 너비를 선호 하는 처음에 응용 프로그램이 실행 될 때 표시 됩니다.
- **동작** -시기를 제어 편집 **동작** 셀에 대 한 전송 됩니다.
- **동작** -셀이 선택 또는 편집할 수 있는 경우를 정의 합니다.
- **서식 있는 텍스트** - `true`, 셀 서식 및 스타일이 적용 된 텍스트를 표시할 수 있습니다.
- **실행 취소** - `true`, 셀 것에 대 한 책임을 지지 동작 실행 취소 합니다.

테이블 셀 뷰 선택 (`NSTableFieldCell`)에서 테이블 열의 맨 아래에 **인터페이스 계층 구조**:

[![](outline-view-images/edit11.png "테이블 셀 보기를 선택합니다.")](outline-view-images/edit10.png#lightbox)

기본으로 사용 되는 테이블 셀 보기를 편집할 수 있습니다 _패턴_ 지정된 된 열에 대해 생성 하는 모든 셀에 대 한 합니다.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>작업 및 출 선을 추가합니다.

방금 다른 Cocoa UI 컨트롤과 마찬가지로 개요 보기를 노출 해야 하 고 열은 셀로 C# 코드 **동작** 하 고 **출 선** (필요한 모든 기능에 따라).

프로세스 개요 보기 요소를 공개할지 여부에 대해 동일 합니다.

1. 로 전환 합니다 **도우미 편집기** 있는지 확인 합니다 `ViewController.h` 파일이 선택 됩니다.: 

    [![](outline-view-images/edit11.png "올바른.h 파일 선택")](outline-view-images/edit11.png#lightbox)
2. 개요 보기에서 선택 합니다 **인터페이스 계층 구조**컨트롤을 클릭 하 고 끌어는 `ViewController.h` 파일입니다.
3. 만들기는 **출 선** 개요 보기 이라는 `ProductOutline`: 

    [![](outline-view-images/edit13.png "출 선 구성")](outline-view-images/edit13.png#lightbox)
4. 만들 **출 선** 도 테이블 열에 대 한 호출 `ProductColumn` 고 `DetailsColumn`: 

    [![](outline-view-images/edit14.png "출 선 구성")](outline-view-images/edit14.png#lightbox)
5. 변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

다음으로, 작성 코드 표시 개요에 대 한 일부 데이터는 응용 프로그램이 실행 될 때.

<a name="Populating_the_Table_View" />

## <a name="populating-the-outline-view"></a>개요 보기 채우기

개요 보기를 사용 하 여 Interface Builder에서 디자인 하 고을 통해 노출 되는 **출 선**, 다음 채울 C# 코드를 생성 해야 합니다.

먼저 새를 만들어 보겠습니다 `Product` 하위 제품 그룹을 확인 하 고 개별 행에 대 한 정보를 저장 하는 클래스입니다. 에 **솔루션 탐색기**, 프로젝트를 마우스 오른쪽 단추로 **추가** > **새 파일...** 선택 **일반** > **빈 클래스**를 입력 `Product` 에 대 한를 **이름** 을 클릭 합니다 **새로 만들기** 단추:

[![](outline-view-images/populate01.png "빈 클래스 만들기")](outline-view-images/populate01.png#lightbox)

확인 된 `Product.cs` 다음과 같은 파일 확인:

```csharp
using System;
using Foundation;
using System.Collections.Generic;

namespace MacOutlines
{
    public class Product : NSObject
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Computed Properties
        public string Title { get; set;} = "";
        public string Description { get; set;} = "";
        public bool IsProductGroup {
            get { return (Products.Count > 0); }
        }
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

그런 다음의 서브 클래스를 생성 해야 `NSOutlineDataSource` 요청 된 것 처럼이 윤곽선에 대 한 데이터를 제공 합니다. 에 **솔루션 탐색기**, 프로젝트를 마우스 오른쪽 단추로 **추가** > **새 파일...** 선택 **일반** > **빈 클래스**, 입력 `ProductOutlineDataSource` 에 대 한를 **이름** 을 클릭 합니다 **새로 만들기** 단추.

편집 된 `ProductTableDataSource.cs` 파일을 다음과 같이 표시 되도록 합니다.

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDataSource : NSOutlineViewDataSource
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Constructors
        public ProductOutlineDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override nint GetChildrenCount (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products.Count;
            } else {
                return ((Product)item).Products.Count;
            }

        }

        public override NSObject GetChild (NSOutlineView outlineView, nint childIndex, NSObject item)
        {
            if (item == null) {
                return Products [childIndex];
            } else {
                return ((Product)item).Products [childIndex];
            }
                
        }

        public override bool ItemExpandable (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products [0].IsProductGroup;
            } else {
                return ((Product)item).IsProductGroup;
            }
        
        }
        #endregion
    }
}
```

이 클래스는 개요 보기의 항목에 대 한 저장소에는 및 재정의 `GetChildrenCount` 테이블의 행 수가 반환 합니다. 합니다 `GetChild` (개요 보기 요청)에 따라 특정 부모 또는 자식 항목을 반환 하며 `ItemExpandable` 부모 또는 자식으로 지정된 된 항목을 정의 합니다.

마지막으로,의 서브 클래스를 생성 해야 `NSOutlineDelegate` 이 윤곽선에 대 한 동작을 제공 하도록 합니다. 에 **솔루션 탐색기**, 프로젝트를 마우스 오른쪽 단추로 **추가** > **새 파일...** 선택 **일반** > **빈 클래스**, 입력 `ProductOutlineDelegate` 에 대 한를 **이름** 을 클릭 합니다 **새로 만들기** 단추.

편집 된 `ProductOutlineDelegate.cs` 파일을 다음과 같이 표시 되도록 합니다.

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDelegate : NSOutlineViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "ProdCell";
        #endregion

        #region Private Variables
        private ProductOutlineDataSource DataSource;
        #endregion

        #region Constructors
        public ProductOutlineDelegate (ProductOutlineDataSource datasource)
        {
            this.DataSource = datasource;
        }
        #endregion

        #region Override Methods

        public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)outlineView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Cast item
            var product = item as Product;

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Product":
                view.StringValue = product.Title;
                break;
            case "Details":
                view.StringValue = product.Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

인스턴스를 만들 것을 `ProductOutlineDelegate`, 또한의 인스턴스에 전달를 `ProductOutlineDataSource` 윤곽선에 대 한 데이터를 제공 하는 합니다. `GetView` 메서드는 제공 열과 행에 대 한 셀을 표시 하려면 보기 (데이터)를 반환 하는 일을 담당 합니다. 가능한 경우 셀을 표시 하려면 기존 뷰를 다시 사용 됩니다 그렇지 않은 경우 새 뷰를 만들어야 합니다.

윤곽선을 채우려면 편집할 합니다 `MainWindow.cs` 파일을 확인 합니다 `AwakeFromNib` 다음과 같은 메서드 확인:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create data source and populate
    var DataSource = new ProductOutlineDataSource ();

    var Vegetables = new Product ("Vegetables", "Greens and Other Produce");
    Vegetables.Products.Add (new Product ("Cabbage", "Brassica oleracea - Leaves, axillary buds, stems, flowerheads"));
    Vegetables.Products.Add (new Product ("Turnip", "Brassica rapa - Tubers, leaves"));
    Vegetables.Products.Add (new Product ("Radish", "Raphanus sativus - Roots, leaves, seed pods, seed oil, sprouting"));
    Vegetables.Products.Add (new Product ("Carrot", "Daucus carota - Root tubers"));
    DataSource.Products.Add (Vegetables);

    var Fruits = new Product ("Fruits", "Fruit is a part of a flowering plant that derives from specific tissues of the flower");
    Fruits.Products.Add (new Product ("Grape", "True Berry"));
    Fruits.Products.Add (new Product ("Cucumber", "Pepo"));
    Fruits.Products.Add (new Product ("Orange", "Hesperidium"));
    Fruits.Products.Add (new Product ("Blackberry", "Aggregate fruit"));
    DataSource.Products.Add (Fruits);

    var Meats = new Product ("Meats", "Lean Cuts");
    Meats.Products.Add (new Product ("Beef", "Cow"));
    Meats.Products.Add (new Product ("Pork", "Pig"));
    Meats.Products.Add (new Product ("Veal", "Young Cow"));
    DataSource.Products.Add (Meats);

    // Populate the outline
    ProductOutline.DataSource = DataSource;
    ProductOutline.Delegate = new ProductOutlineDelegate (DataSource);

}
```

응용 프로그램을 실행 하는 경우 다음이 표시 됩니다.

[![](outline-view-images/populate02.png "축소 된 보기")](outline-view-images/populate02.png#lightbox)

개요 뷰에서 노드를 확장 하는 경우에 다음과 같이 표시 됩니다.

[![](outline-view-images/populate03.png "확장된 된 보기")](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>열 기준 정렬

열 머리글을 클릭 하 여 개요에서 데이터를 정렬 하는 사용자를 허용 해 보겠습니다. 첫째, 두 번 클릭 합니다 `Main.storyboard` Interface Builder에서 편집용으로 열 파일입니다. 선택 합니다 `Product` 열 입력 `Title` 에 대 한 합니다 **정렬 키**, `compare:` 에 대 한를 **선택기** 선택한 `Ascending` 에 대 한를 **순서**:

[![](outline-view-images/sort01.png "정렬 키를 설정합니다.")](outline-view-images/sort01.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

이제 편집는 `ProductOutlineDataSource.cs` 파일과 다음 메서드를 추가 합니다.

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
    }
}

public override void SortDescriptorsChanged (NSOutlineView outlineView, NSSortDescriptor[] oldDescriptors)
{
    // Sort the data
    Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
    outlineView.ReloadData ();
}
```

`Sort` 메서드를 허용 하는 데이터 원본에서 데이터를 정렬 하는 지정 된 `Product` 클래스 필드를 오름차순 또는 내림차순입니다. 재정의 된 `SortDescriptorsChanged` 메서드를 사용 하 여 열 머리글을 클릭할 때마다 호출 됩니다. 전달 되는 **키** Interface Builder 및 해당 열에 대 한 정렬 순서에서 설정 하는 값입니다.

응용 프로그램을 실행 하 고 열 머리글을 클릭 합니다 행 열에 따라 정렬 됩니다.

[![](outline-view-images/sort02.png "정렬 된 출력의 예")](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>행 선택

사용자가 두 번 클릭을 단일 행을 선택 하도록 허용 하려는 경우는 `Main.storyboard` Interface Builder에서 편집용으로 열 파일입니다. 개요 보기에서 선택는 **인터페이스 계층 구조** 의 선택을 취소를 **여러** 의 확인란을 선택 합니다 **특성 검사기**:

[![](outline-view-images/select01.png "특성 검사기")](outline-view-images/select01.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.


다음에 편집는 `ProductOutlineDelegate.cs` 파일과 다음 메서드를 추가 합니다.

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

이렇게 하면 사용자 개요 뷰에 있는 임의의 단일 행을 선택할 수 있습니다. 반환 `false` 에 대 한 합니다 `ShouldSelectItem` 사용자가 선택할 수 있게 하는 모든 항목에 대 한 않으려는 또는 `false` 모든 항목을 선택할 수 있으려면 사용자를 표시 하지 않으려는 경우 모든 항목에 대 한 합니다.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>여러 개의 행 선택

사용자가 두 번 클릭을 여러 행을 선택 하도록 허용 하려는 경우는 `Main.storyboard` Interface Builder에서 편집용으로 열 파일입니다. 개요 보기에서 선택는 **인터페이스 계층 구조** 확인를 **여러** 에서 확인란을 선택 합니다 **특성 검사기**:

[![](outline-view-images/select02.png "특성 검사기")](outline-view-images/select02.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.


다음에 편집는 `ProductOutlineDelegate.cs` 파일과 다음 메서드를 추가 합니다.

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

이렇게 하면 사용자 개요 뷰에 있는 임의의 단일 행을 선택할 수 있습니다. 반환 `false` 에 대 한 합니다 `ShouldSelectRow` 사용자가 선택할 수 있게 하는 모든 항목에 대 한 않으려는 또는 `false` 모든 항목을 선택할 수 있으려면 사용자를 표시 하지 않으려는 경우 모든 항목에 대 한 합니다.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>행을 선택 하는 형식

문자가 있는 첫 번째 행을 선택한 사용자가 선택한 개요 보기를 사용 하 여 문자를 입력 하도록 허용 하려는 경우, 두 번 클릭 합니다 `Main.storyboard` Interface Builder에서 편집용으로 열 파일입니다. 개요 보기에서 선택는 **인터페이스 계층 구조** 확인 및를 **형식 선택** 에서 확인란을 선택 합니다 **특성 검사기**:

[![](outline-view-images/type01.png "행 형식 편집")](outline-view-images/type01.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

이제 편집는 `ProductOutlineDelegate.cs` 파일과 다음 메서드를 추가 합니다.

```csharp
public override NSObject GetNextTypeSelectMatch (NSOutlineView outlineView, NSObject startItem, NSObject endItem, string searchString)
{
    foreach(Product product in DataSource.Products) {
        if (product.Title.Contains (searchString)) {
            return product;
        }
    }

    // Not found
    return null;
}
```

`GetNextTypeSelectMatch` 메서드는 지정 된 `searchString` 첫 번째 항목을 반환 하 고 `Product` 는 포함 된 문자열의 `Title`합니다.

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>열 다시 정렬

개요 보기에서 열 순서 바꾸기를 끌어 놓을 수 있도록 하려는 경우, 두 번 클릭 합니다 `Main.storyboard` Interface Builder에서 편집용으로 열 파일입니다. 개요 보기에서 선택는 **인터페이스 계층 구조** 확인를 **순서 다시 매기기** 에서 확인란을 선택 합니다 **특성 검사기**:

[![](outline-view-images/reorder01.png "특성 검사기")](outline-view-images/reorder01.png#lightbox)

에 대 한 값을 제공 하는 경우는 **자동 저장** 속성을 확인 합니다 **열 정보** 필드를 테이블의 레이아웃을 변경 했습니다 우리 회사에 저장 됩니다 하 고 다음에 응용 프로그램 복원 실행 됩니다.

변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

이제 편집는 `ProductOutlineDelegate.cs` 파일과 다음 메서드를 추가 합니다.

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

합니다 `ShouldReorder` 메서드는 반환 해야 `true` 할 수 있도록 하려는 모든 열에 대 한으로 다시 정렬 끌어서 합니다 `newColumnIndex`, 그렇지 않으면 반환 `false`;

응용 프로그램을 실행 하는 경우 관련 열 머리글이 열 순서를 변경 하려면 끌 수 있습니다.

[![](outline-view-images/reorder02.png "열 다시 정렬의 예")](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>셀 편집

사용자가 지정된 된 셀의 값을 편집, 편집 하도록 허용 하려는 경우는 `ProductOutlineDelegate.cs` 파일을 변경 합니다 `GetViewForItem` 같이 메서드:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTextField view = (NSTextField)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTextField ();
        view.Identifier = tableColumn.Title;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.StringValue;
            break;
        case "Details":
            prod.Description = view.StringValue;
            break; 
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.StringValue = product.Title;
        break;
    case "Details":
        view.StringValue = product.Description;
        break;
    }

    return view;
}
```

이제 응용 프로그램을 실행 하는 경우 사용자 테이블 보기의 셀을 편집할 수 있습니다.

[![](outline-view-images/editing01.png "셀 편집의 예제")](outline-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Outline_Views" />

## <a name="using-images-in-outline-views"></a>개요 보기에서 이미지 사용

셀의 일부로 이미지를 포함 하는 `NSOutlineView`, 개요 보기의 데이터가 반환 되는 방식을 변경 해야 `NSTableViewDelegate's` `GetView` 메서드를 사용 하 여를 `NSTableCellView` 일반적인 대신 `NSTextField`합니다. 예를 들어:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)outlineView.MakeView (tableColumn.Title, this);
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
        view.TextField.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.TextField.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.TextField.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.TextField.StringValue;
            break;
        case "Details":
            prod.Description = view.TextField.StringValue;
            break; 
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed (product.IsProductGroup ? "tags.png" : "tag.png");
        view.TextField.StringValue = product.Title;
        break;
    case "Details":
        view.TextField.StringValue = product.Description;
        break;
    }

    return view;
}
```

자세한 내용은 참조 하십시오는 [개요 보기를 사용 하 여 이미지를 사용 하 여](~/mac/app-fundamentals/image.md) 부분 우리의 [이미지를 사용 하 여 작업](~/mac/app-fundamentals/image.md) 설명서.

<a name="Data_Binding_Outline_Views" />

## <a name="data-binding-outline-views"></a>데이터 바인딩 개요 보기

Xamarin.Mac 응용 프로그램에서 데이터 바인딩 및 키-값 코딩 기술을 사용 하 여 작성 하 고 채우는 UI 요소를 사용 하 여 작업을 유지 해야 하는 코드의 양을 크게 줄일 수 있습니다. 추가 백업 데이터를 분리의 장점은 수도 있습니다 (_데이터 모델_)에 처음부터 사용자 인터페이스를 종료 합니다 (_모델-뷰-컨트롤러_)를 더 쉬워진 유지 관리를 보다 융통성 있는 응용 프로그램 디자인 합니다.

키-값 코딩 (KVC)는 개체의 속성을 직접 액세스, 인스턴스 변수를 통해 액세스 하는 대신 속성을 식별 (특별 한 형식의 문자열) 키 또는 접근자 메서드를 사용 하는 메커니즘 (`get/set`). Xamarin.Mac 응용 프로그램에서 키-값 코딩 규격 접근자를 구현, 키-값을 관찰 (KVO), 데이터 바인딩, 핵심 데이터, Cocoa 바인딩 및 스크립팅 등과 같은 다른 macOS 기능에 대 한 액세스를 얻을 수 있습니다.

자세한 내용은 참조 하십시오는 [개요 보기 데이터 바인딩](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding) 부분 우리의 [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md) 설명서.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Mac 응용 프로그램의 개요 보기를 사용 하 여 작업을 자세히 살펴보려면을 걸렸습니다. 다양 한 종류를 확인 하 고 개요 보기, 만들기 및 개요 뷰가 Xcode의 Interface Builder에서 유지 관리 하는 방법 및 개요 보기를 사용 하 여 C# 코드에서 작업 하는 방법을 사용 합니다.

## <a name="related-links"></a>관련 링크

- [MacOutlines (샘플)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [MacImages(샘플)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [테이블 보기](~/mac/user-interface/table-view.md)
- [원본 목록](~/mac/user-interface/source-list.md)
- [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [출 선 보기 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
