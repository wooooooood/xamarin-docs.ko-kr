---
title: Xamarin.Mac의 개요 보기
description: 이 문서에서는 Xamarin.Mac 응용 프로그램의 개요 보기와 작업을 설명합니다. 및 작성 하 고, Xcode 및 인터페이스 작성기의 개요 보기를 유지 관리 하 고, 이러한 작업을 프로그래밍 방식으로 설명 합니다.
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: a12eee5f473ffdc6a235faeb55c0a3d6754f4629
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792829"
---
# <a name="outline-views-in-xamarinmac"></a>Xamarin.Mac의 개요 보기

_이 문서에서는 Xamarin.Mac 응용 프로그램의 개요 보기와 작업을 설명합니다. 및 작성 하 고, Xcode 및 인터페이스 작성기의 개요 보기를 유지 관리 하 고, 이러한 작업을 프로그래밍 방식으로 설명 합니다._

C# 및.NET Xamarin.Mac 응용 프로그램에서에서 작업할 때는 동일 하 게 액세스할 수 있습니다 개요 보기의 작업 개발자입니다 *Objective-c* 및 *Xcode* 않습니다. Xamarin.Mac Xcode에 직접 통합을 사용 하면 Xcode의 _인터페이스 작성기_ 만들기 및 개요 뷰를 유지 관리 (또는 C# 코드에서 직접 만들 필요에 따라).

개요 보기 수 있도록 사용자 테이블의 형식을 확장 하거나 계층적 데이터의 행을 축소 됩니다. 테이블 보기와 같은 개요 보기 개별 항목과 해당 항목의 특성을 나타내는 열을 나타내는 행과 관련 된 항목의 집합에 대 한 데이터를 표시 합니다. 표 보기와 달리 개요 보기의 항목에 단순 목록에 없는, 하드 드라이브에서 파일 및 폴더와 같은 계층에 구성 됩니다.

[![](outline-view-images/populate03.png "실행 하는 예제 응용 프로그램")](outline-view-images/populate03.png#lightbox)

이 문서에서는의 기본적인 개요 보기와 Xamarin.Mac 응용 프로그램에서 사용 하겠습니다. 것이 가장 좋습니다를 통해 협력 하는 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서는 [Xcode 및 인터페이스 작성기 소개](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) 및 [콘센트 및 동작](~/mac/get-started/hello-mac.md#Outlets_and_Actions) 섹션으로이 문서에서 사용할 수 있는 주요 개념 및 기술을 설명 합니다.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명도 문서는 `Register` 및 `Export` 명령 와이어 접속 C# 클래스 Objective-c 개체 및 UI 요소에 하는 데 사용 합니다.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-outline-views"></a>개요 보기 소개

개요 보기 수 있도록 사용자 테이블의 형식을 확장 하거나 계층적 데이터의 행을 축소 됩니다. 테이블 보기와 같은 개요 보기 개별 항목과 해당 항목의 특성을 나타내는 열을 나타내는 행과 관련 된 항목의 집합에 대 한 데이터를 표시 합니다. 표 보기와 달리 개요 보기의 항목에 단순 목록에 없는, 하드 드라이브에서 파일 및 폴더와 같은 계층에 구성 됩니다.

개요 보기의 항목에 다른 항목이 포함 된 경우에 확장 하거나 사용자가 축소할 수 있습니다. 확장 가능한 항목 가리키는 오른쪽 항목 축소 되 고 다운 가리키는 항목을 확장 하는 경우는 삼각형을 표시 합니다. 삼각형에 클릭 하면 항목을 확장 하거나 축소 합니다.

개요 보기 (`NSOutlineView`) 테이블 보기의 하위 클래스가 (`NSTableView`) 이므로 해당 부모 클래스에서 상속 하는 동작의 대부분이 및 합니다. 결과적으로, 표 보기 행 이나 열을 선택 하는 등 끌어 열 머리글 등의 열 위치를 변경 하 여 지원 되는 많은 작업 개요 보기 에서도 지원 됩니다. Xamarin.Mac 응용 프로그램을 이러한 기능을 제어 하는 및를 허용할지 또는 특정 작업을 허용 하지 않습니다 (코드 또는 인터페이스 작성기 중 하나) 개요 보기의 매개 변수를 구성할 수 있습니다.

개요 보기 자신의 데이터를 저장 하지 않으므로, 데이터 원본에서 대신 (`NSOutlineViewDataSource`) 행과 열에는 필요에 따라 필요한 수 있도록 합니다.

개요 보기 대리자의 서브 클래스를 제공 하 여 개요 보기의 동작을 사용자 지정할 수 있습니다 (`NSOutlineViewDelegate`) 기능, 행 선택 및 편집, 사용자 지정 추적 및 개인에 대 한 사용자 지정 보기를 선택 하려면 입력 열 관리 개요를 지원 하기 열과 행입니다.

통해 이동 하려는 표 보기와의 동작 및 기능 많이 공유 하는 개요 보기, 이후 우리의 [테이블 뷰](~/mac/user-interface/table-view.md) 이 문서를 계속 하기 전에 설명서입니다.

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode" />

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>만들기 및 Xcode에서 개요 뷰를 유지 관리

새 Xamarin.Mac Cocoa 응용 프로그램을 만들 때 기본적으로 표준, 빈 창이 얻을 수 있습니다. 에 정의 되어 있는이 windows는 `.storyboard` 파일은 프로젝트에 자동으로 포함 합니다. windows 디자인에 맞게 편집 하는 **솔루션 탐색기**, 두 번 클릭 하 고 `Main.storyboard` 파일:

[![](outline-view-images/edit01.png "주 스토리 보드를 선택합니다.")](outline-view-images/edit01.png#lightbox)

Xcode의 인터페이스 작성기의 창 디자인을 열립니다.

[![](outline-view-images/edit02.png "Xcode에서 UI를 편집합니다.")](outline-view-images/edit02.png#lightbox)

형식 `outline` 에 **라이브러리 검사기의** 쉽게 개요 보기 컨트롤을 찾을 수 있도록 하려면 검색 상자:

[![](outline-view-images/edit03.png "라이브러리에서 개요 보기를 선택 하면")](outline-view-images/edit03.png#lightbox)

개요 보기의 뷰 컨트롤러 끌어다는 **인터페이스 편집기**, 축소 하 고 있는 창으로 증가를 설정 하 고 뷰 컨트롤러의 콘텐츠 영역을 채우는 **제약 조건 편집기**:

[![](outline-view-images/edit04.png "제약 조건을 편집")](outline-view-images/edit04.png#lightbox)

개요 보기를 선택는 **인터페이스 계층 구조** 하며 다음과 같은 속성에서 사용할 수는 **특성 검사기**:

[![](outline-view-images/edit05.png "특성 검사기")](outline-view-images/edit05.png#lightbox)

- **열에 간략하게 설명** -해당 테이블 열 계층적 데이터가 표시 됩니다.
- **자동 저장 개요 열** -경우 `true`, 개요 열을 자동으로 저장 하 고 응용 프로그램 실행 간에 복원 됩니다.
- **들여쓰기** -확장 된 항목은 아래의 열 들여쓰기 하는 정도입니다.
- **들여쓰기 뒤에 오는 셀** -경우 `true`, 셀 함께 들여쓰기 표시를 들여씁니다.
- **항목을 확장 하는 자동 저장** -경우 `true`, 항목의 확장/축소 된 상태를 자동으로 저장 하 고 응용 프로그램 실행 간에 복원 됩니다.
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
> 레거시 Xamarin.Mac 응용 프로그램을 유지 하는 경우가 아니면 `NSView` 기반된 개요 보기를 통해 사용할지 `NSCell` 테이블 뷰를 기반으로 합니다. `NSCell` 레거시 간주 되 고 앞으로 지원 되지 않습니다.

테이블 열 선택의 **인터페이스 계층 구조** 다음 속성에서 사용할 수 있는 **특성 검사기**:

[![](outline-view-images/edit06.png "특성 검사기")](outline-view-images/edit06.png#lightbox)

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

[![](outline-view-images/edit07.png "특성 검사기")](outline-view-images/edit07.png#lightbox)

이들은 모두 표준 보기의 속성입니다. 여기에이 열에 대 한 행의 크기를 조정 하는 옵션이 수도 있습니다.

테이블 보기 셀을 선택 (이 기본적으로 `NSTextField`)에 **인터페이스 계층 구조** 다음 속성에서 사용할 수 있는 **특성 검사기**:

[![](outline-view-images/edit08.png "특성 검사기")](outline-view-images/edit08.png#lightbox)

여기에서 설정 하는 표준 텍스트 필드의 모든 속성이 있습니다. 기본적으로 표준 텍스트 필드는 데이터를 표시 하는 셀에 대 한 열에 사용 됩니다.

표 셀 보기 선택 (`NSTableFieldCell`)에 **인터페이스 계층 구조** 하며 다음과 같은 속성에서 사용할 수는 **특성 검사기**:

[![](outline-view-images/edit09.png "특성 검사기")](outline-view-images/edit09.png#lightbox)

여기에 가장 중요 한 설정은 다음과 같습니다.

- **레이아웃** -이 열의 셀에에서 배치 하는 방법을 선택 합니다.
- **한 줄 모드를 사용 하 여** -경우 `true`, 셀이 한 줄으로 제한 합니다.
- **첫 번째 런타임 레이아웃 너비** -경우 `true`, 셀 너비 설정에 대 한 (수동 또는 자동으로) 경우 것을 선호할 것 처음으로 응용 프로그램이 실행 될 때 표시 됩니다.
- **동작** -시점을 제어 편집 **동작** 셀에 대 한 전송 됩니다.
- **동작** -셀이 선택 또는 편집할 수 있는지를 정의 합니다.
- **서식 있는 텍스트** -경우 `true`, 셀으로 포맷 되 고 스타일이 적용 된 텍스트를 표시할 수 있습니다.
- **실행 취소** -경우 `true`, 셀은에 대 한 책임을 지지 동작 실행 취소 합니다.

표 셀 보기 선택 (`NSTableFieldCell`)에서 테이블 열의 맨 아래에 **인터페이스 계층 구조**:

[![](outline-view-images/edit11.png "표 셀 보기를 선택합니다.")](outline-view-images/edit10.png#lightbox)

기본으로 사용 되는 테이블 셀 보기를 편집할 수 있습니다 _패턴_ 지정된 된 열에 대해 생성 된 모든 셀에 대 한 합니다.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>작업 및 콘센트 추가

방금 다른 Cocoa UI 컨트롤 처럼 우리의 개요 보기를 노출 해야 하 고 열은 C# 코드를 사용 하 여 셀 **동작** 및 **콘센트** (필요한 기능에 따라).

프로세스 개요 보기 요소를 노출 하려는 같습니다.

1. 전환 하는 **도우미 편집기** 되어 있는지 확인 하 고는 `ViewController.h` 파일을 선택: 

    [![](outline-view-images/edit11.png "올바른.h 파일을 선택 하면")](outline-view-images/edit11.png#lightbox)
2. 개요 보기에서 선택 하는 **인터페이스 계층 구조**control 클릭 하 고 끌어는 `ViewController.h` 파일입니다.
3. 만들기는 **콘센트** 개요 보기 호출 된 `ProductOutline`: 

    [![](outline-view-images/edit13.png "콘센트에 연결 구성")](outline-view-images/edit13.png#lightbox)
4. 만들 **콘센트** 는 테이블 열에 대 한 호출 `ProductColumn` 및 `DetailsColumn`: 

    [![](outline-view-images/edit14.png "콘센트에 연결 구성")](outline-view-images/edit14.png#lightbox)
5. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

다음으로 작성 합니다 코드 디스플레이 윤곽선에 대 한 일부 데이터는 응용 프로그램이 실행 될 때.

<a name="Populating_the_Table_View" />

## <a name="populating-the-outline-view"></a>개요 보기 채우기

우리의 개요 보기와 인터페이스 작성기에서 디자인 하 고을 통해 노출 되는 **콘센트**, 다음은 C# 채우는 코드를 만들려는 필요 합니다.

첫째, 새를 만들어 보겠습니다 `Product` 개별 행과 하위 제품의 그룹에 대 한 정보를 보관 하는 클래스입니다. 에 **솔루션 탐색기**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...** 선택 **일반** > **빈 클래스**, 입력 `Product` 에 대 한는 **이름** 클릭는 **새로** 단추:

[![](outline-view-images/populate01.png "빈 클래스 만들기")](outline-view-images/populate01.png#lightbox)

확인 된 `Product.cs` 다음과 같은 파일 보기:

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

서브 클래스를 만들어야 한다는 다음으로, `NSOutlineDataSource` 요청 될 때이 개요에 대 한 데이터를 제공 하 합니다. 에 **솔루션 탐색기**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...** 선택 **일반** > **빈 클래스**, 입력 `ProductOutlineDataSource` 에 대 한는 **이름** 클릭는 **새로** 단추입니다.

편집 된 `ProductTableDataSource.cs` 파일을 다음과 같이 표시 하 게 합니다.

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

이 클래스는 우리의 개요 보기의 항목에 대 한 저장소가 및 재정의 `GetChildrenCount` 를 테이블의 행 수를 반환 합니다. `GetChild` (개요 보기에서 요청)에 따라 특정 부모 또는 자식 항목을 반환 하며 `ItemExpandable` 부모 또는 자식으로 지정된 된 항목을 정의 합니다.

서브 클래스를 만들어야 한다는 마지막으로, `NSOutlineDelegate` 우리의 윤곽선에 대 한 동작을 제공 하도록 합니다. 에 **솔루션 탐색기**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...** 선택 **일반** > **빈 클래스**, 입력 `ProductOutlineDelegate` 에 대 한는 **이름** 클릭는 **새로** 단추입니다.

편집 된 `ProductOutlineDelegate.cs` 파일을 다음과 같이 표시 하 게 합니다.

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

인스턴스를 만들 때는 `ProductOutlineDelegate`, 또한의 인스턴스에 전달는 `ProductOutlineDataSource` 윤곽선에 대 한 데이터를 제공 하는 합니다. `GetView` 메서드는 제공 열과 행에 대 한 셀을 표시 하도록 보기 (데이터)를 반환 합니다. 가능 하면 셀을 표시 하려면 기존 뷰를 다시 사용 됩니다 하지 않은 경우 새 보기를 만들어야 합니다.

윤곽선으로 채우려면 편집는 `MainWindow.cs` 파일을 확인는 `AwakeFromNib` 다음과 같은 메서드 보기:

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

응용 프로그램을 실행 하는 경우 다음 내용이 표시 됩니다.

[![](outline-view-images/populate02.png "축소 된 보기")](outline-view-images/populate02.png#lightbox)

개요 보기에서 노드를 확장 하는 경우 다음과 같이 표시 됩니다.

[![](outline-view-images/populate03.png "확장 된 보기")](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>열 기준 정렬

열 머리글을 클릭 하 여 개요 창에서 데이터를 정렬할 수 있는 사용자를 수 보겠습니다. 첫째, 두 번 클릭 하 고 `Main.storyboard` 인터페이스 작성기에서 편집을 위해 열 파일입니다. 선택 된 `Product` 열을 입력 `Title` 에 대 한는 **정렬 키**, `compare:` 에 대 한는 **선택기** 선택 `Ascending` 에 대 한는 **순서**:

[![](outline-view-images/sort01.png "정렬 키 순서 설정")](outline-view-images/sort01.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

이제 편집는 `ProductOutlineDataSource.cs` 파일을 다음 메서드를 추가 합니다.

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

`Sort` 메서드를 기반으로 데이터 원본에서 데이터를 정렬할 수 있도록 허용 된 주어진 `Product` 오름차순 또는 내림차순 클래스 필드입니다. 재정의 된 `SortDescriptorsChanged` 메서드를 사용 하는 열 머리글을 클릭할 때마다 호출 됩니다. 전달 되는 **키** 인터페이스 작성기와 해당 열에 대 한 정렬 순서에 설정 하는 값입니다.

응용 프로그램을 실행 하 고 열 머리글에서을 클릭 합니다 행이 해당 열으로 정렬 됩니다.

[![](outline-view-images/sort02.png "정렬 된 출력의 예")](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>행 선택

사용자가 두 번 클릭을 단일 행을 선택 하도록 허용 하려는 경우는 `Main.storyboard` 인터페이스 작성기에서 편집을 위해 열 파일입니다. 개요 보기를 선택는 **인터페이스 계층 구조** 의 선택을 취소 하 고는 **여러** 확인란을 선택은 **특성 검사기**:

[![](outline-view-images/select01.png "특성 검사기")](outline-view-images/select01.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.


다음에 편집는 `ProductOutlineDelegate.cs` 파일을 다음 메서드를 추가 합니다.

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

이렇게 하면 사용자가 개요 보기에서 임의의 단일 행을 선택할 수 있습니다. 반환할 `false` 에 대 한는 `ShouldSelectItem` 사용자를 선택할 수 있는 모든 항목에 대 한 않으려면 또는 `false` 사용자 모든 항목을 선택할 수 있게 하려는 경우 모든 항목에 대 한 합니다.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>여러 개의 행 선택

사용자가 두 번 클릭을 여러 행을 선택 하도록 허용 하려는 경우는 `Main.storyboard` 인터페이스 작성기에서 편집을 위해 열 파일입니다. 개요 보기를 선택는 **인터페이스 계층 구조** 확인 하 고는 **여러** 확인란을 선택은 **특성 검사기**:

[![](outline-view-images/select02.png "특성 검사기")](outline-view-images/select02.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.


다음에 편집는 `ProductOutlineDelegate.cs` 파일을 다음 메서드를 추가 합니다.

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

이렇게 하면 사용자가 개요 보기에서 임의의 단일 행을 선택할 수 있습니다. 반환할 `false` 에 대 한는 `ShouldSelectRow` 사용자를 선택할 수 있는 모든 항목에 대 한 않으려면 또는 `false` 사용자 모든 항목을 선택할 수 있게 하려는 경우 모든 항목에 대 한 합니다.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>행을 선택 하는 형식

사용자 개요 보기를 선택 하는 문자를 입력 하도록 허용 하 고 첫 번째 행을 선택 하려면 해당 문자를 두 번 클릭 하 고 `Main.storyboard` 인터페이스 작성기에서 편집을 위해 열 파일입니다. 개요 보기를 선택는 **인터페이스 계층 구조** 확인 하 고는 **유형 선택** 확인란을 선택은 **특성 검사기**:

[![](outline-view-images/type01.png "행 형식 편집")](outline-view-images/type01.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

이제 편집는 `ProductOutlineDelegate.cs` 파일을 다음 메서드를 추가 합니다.

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

`GetNextTypeSelectMatch` 메서드는 지정 된 `searchString` 첫 번째 항목을 반환 하 고 `Product` 하에 해당 문자열의 `Title`합니다.

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>열 다시 정렬

개요 보기에서 열 순서 바꾸기 끌어서 놓을 수 있도록 하려는 경우, 두 번 클릭 하 고 `Main.storyboard` 인터페이스 작성기에서 편집을 위해 열 파일입니다. 개요 보기를 선택는 **인터페이스 계층 구조** 확인 하 고는 **Reordering** 확인란을 선택은 **특성 검사기**:

[![](outline-view-images/reorder01.png "특성 검사기")](outline-view-images/reorder01.png#lightbox)

에 대 한 값을 제공 하는 경우는 **자동 저장** 속성 및 검사는 **열 정보** 필드 테이블의 레이아웃을 변경 했습니다 위해 자동으로 저장 되 고 다음에 응용 프로그램 복원 실행 됩니다.

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

이제 편집는 `ProductOutlineDelegate.cs` 파일을 다음 메서드를 추가 합니다.

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

`ShouldReorder` 메서드를 반환 하도록 `true` 수 있도록 하려는 모든 열에 대 한으로 다시 정렬 끌어서는 `newColumnIndex`, 그렇지 않으면 return `false`;

응용 프로그램을 실행 하는 경우 열 순서를 변경 하려면 열 머리글 주위 끌어서:

[![](outline-view-images/reorder02.png "열 다시 정렬의 예")](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>셀 편집

사용자 지정된 된 셀에 대 한 값을 편집, 편집 하도록 허용 하려는 경우는 `ProductOutlineDelegate.cs` 파일 및 변경 된 `GetViewForItem` 메서드를 다음과 같이:

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

이제 응용 프로그램을 실행 하는 경우 사용자 테이블 보기에 있는 셀을 편집할 수 있습니다.:

[![](outline-view-images/editing01.png "셀 편집의 예")](outline-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Outline_Views" />

## <a name="using-images-in-outline-views"></a>개요 보기에서 이미지를 사용 하 여

에 있는 셀의 일부로 이미지를 포함 하는 `NSOutlineView`, 개요 보기에서 데이터 반환 되는 방법을 변경 해야 `NSTableViewDelegate's` `GetView` 메서드를 사용 하 여는 `NSTableCellView` 일반적인 대신 `NSTextField`합니다. 예를 들어:

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

자세한 내용은 참조 하십시오는 [개요 뷰를 사용 하 여 이미지를 사용 하 여](~/mac/app-fundamentals/image.md) 의 섹션 우리의 [이미지 작업](~/mac/app-fundamentals/image.md) 설명서입니다.

<a name="Data_Binding_Outline_Views" />

## <a name="data-binding-outline-views"></a>데이터 바인딩 개요 뷰

키-값 코딩 및 데이터 바인딩 기술을 Xamarin.Mac 응용 프로그램 사용 하 여 작성 및 유지를 채우고 UI 요소를 사용 해야 하는 코드의 양을 크게 줄일 수 있습니다. 추가 백업 데이터를 분리 하는 이점이 있습니다 (_데이터 모델_) 사용자 인터페이스를 종료 하면 앞에서 (_모델-뷰-컨트롤러_)를 보다 융통성 있는 응용 프로그램 유지 관리 디자인 합니다.

키-값 코딩 (KVC)은 인스턴스 변수를 통해 액세스 하는 대신 속성을 식별 하 키 (특별 한 형식의 문자열) 또는 접근자 메서드를 사용 하 여, 개체의 속성을 직접 액세스 메커니즘 (`get/set`). Xamarin.Mac 응용 프로그램에서 키-값 코딩 규격 접근자를 구현 하 여 키 값을 관찰 (KVO), 데이터 바인딩, 핵심 데이터, Cocoa 바인딩 및 스크립팅 등의 기타 macOS 기능에 액세스할을 수 있습니다.

자세한 내용은 참조 하십시오는 [개요 보기 데이터 바인딩](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding) 섹션 우리의 [데이터 바인딩 및 코딩을 키-값](~/mac/app-fundamentals/databinding.md) 설명서입니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에 자세히 살펴보고 개요 뷰 Xamarin.Mac 응용 프로그램에서 작업 수행 했습니다. 다양 한 유형을 보여 준다는 사실을 알았습니다 하 고 개요 보기, 만들기 및 Xcode의 인터페이스 작성기의 개요 보기를 유지 관리 하는 방법 및 C# 코드에서 개요 뷰를 사용 하는 방법을 사용 합니다.

## <a name="related-links"></a>관련 링크

- [MacOutlines (샘플)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [MacImages(샘플)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [테이블 보기](~/mac/user-interface/table-view.md)
- [원본 목록](~/mac/user-interface/source-list.md)
- [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [개요 보기 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
