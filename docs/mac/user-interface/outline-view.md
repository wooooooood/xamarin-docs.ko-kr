---
title: Xamarin.ios의 개요 보기
description: 이 문서에서는 Xamarin.ios 응용 프로그램에서 개요 보기를 사용 하는 방법을 설명 합니다. Xcode에서 개요 보기를 만들고 유지 관리 하는 방법 및 Interface Builder 하 고 프로그래밍 방식으로 작업 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: e4f1f1333c35a72e7243e892e7aac8d98603c973
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70772597"
---
# <a name="outline-views-in-xamarinmac"></a>Xamarin.ios의 개요 보기

_이 문서에서는 Xamarin.ios 응용 프로그램에서 개요 보기를 사용 하는 방법을 설명 합니다. Xcode에서 개요 보기를 만들고 유지 관리 하는 방법 및 Interface Builder 하 고 프로그래밍 방식으로 작업 하는 방법을 설명 합니다._

Xamarin.ios 응용 프로그램 C# 에서 및 .net을 사용 하는 경우 *목표-C* 및 *Xcode* 에서 작업 하는 개발자가 같은 개요 보기에 액세스할 수 있습니다. Xamarin.ios는 Xcode와 직접 통합 되므로 Xcode의 _Interface Builder_ 를 사용 하 여 개요 보기를 만들고 유지 관리 하거나 (필요에 따라 코드에서 C# 직접 만들 수 있습니다.)

개요 뷰는 사용자가 계층적 데이터의 행을 확장 하거나 축소할 수 있도록 하는 테이블 유형입니다. 테이블 뷰와 마찬가지로 개요 보기에는 관련 항목 집합에 대 한 데이터와 해당 항목의 특성을 나타내는 개별 항목 및 열을 나타내는 행이 표시 됩니다. 테이블 뷰와 달리 개요 보기의 항목은 단순 목록에 있지 않으며 하드 드라이브의 파일 및 폴더와 같은 계층 구조로 구성 됩니다.

[![](outline-view-images/populate03.png "예제 앱 실행")](outline-view-images/populate03.png#lightbox)

이 문서에서는 Xamarin.ios 응용 프로그램에서 개요 보기를 사용 하는 기본 사항을 설명 합니다. [Hello, Mac](~/mac/get-started/hello-mac.md) 문서를 먼저 사용 하는 것이 가장 좋습니다. 특히 [Xcode 및 Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 및 [콘센트 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션을 소개 하 고,에서 사용할 주요 개념 및 기술을 설명 하 고 있습니다. 이 문서를 참조 하세요.

[Xamarin.ios 내부](~/mac/internals/how-it-works.md) 문서의 [클래스/메서드를 대상으로 노출 C# -C](~/mac/internals/how-it-works.md) 섹션을 살펴볼 수 있습니다. C# 클래스를 연결 하는 데 사용 되는 `Register` 및 `Export` 명령을 설명 합니다. 목표-C 개체 및 UI 요소입니다.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-outline-views"></a>개요 보기 소개

개요 뷰는 사용자가 계층적 데이터의 행을 확장 하거나 축소할 수 있도록 하는 테이블 유형입니다. 테이블 뷰와 마찬가지로 개요 보기에는 관련 항목 집합에 대 한 데이터와 해당 항목의 특성을 나타내는 개별 항목 및 열을 나타내는 행이 표시 됩니다. 테이블 뷰와 달리 개요 보기의 항목은 단순 목록에 있지 않으며 하드 드라이브의 파일 및 폴더와 같은 계층 구조로 구성 됩니다.

개요 보기의 항목에 다른 항목이 포함 되어 있으면 사용자가 확장 하거나 축소할 수 있습니다. 확장 가능한 항목은 항목이 축소 될 때 오른쪽을 가리키고 항목이 확장 될 때 아래로 이동 하는 공개 삼각형을 표시 합니다. 노출 삼각형을 클릭 하면 항목이 확장 또는 축소 됩니다.

개요 뷰 (`NSOutlineView`)는 테이블 뷰 (`NSTableView`)의 하위 클래스 이므로 부모 클래스에서 대부분의 동작을 상속 합니다. 따라서 행 또는 열 선택, 열 머리글을 끌어서 열 위치 조정 등 테이블 뷰에서 지원 되는 많은 작업은 개요 보기 에서도 지원 됩니다. Xamarin.ios 응용 프로그램은 이러한 기능을 제어할 수 있으며 코드 또는 Interface Builder에서 개요 뷰의 매개 변수를 구성 하 여 특정 작업을 허용 하거나 허용 하지 않을 수 있습니다.

개요 뷰에서는 자체 데이터를 저장 하지 않고 데이터 원본 (`NSOutlineViewDataSource`)을 사용 하 여 필요한 행과 열을 모두 필요에 따라 제공 합니다.

개요 열 관리를 지원 하 고, 기능을 선택 하 고, 열을 선택 하`NSOutlineViewDelegate`고, 편집 하 고, 사용자 지정 추적 하 고, 개별 사용자 지정 보기를 지원 하려면 개요 뷰 대리자 ()의 하위 클래스를 제공 하 여 개요 뷰의 동작을 사용자 지정할 수 있습니다. 열 및 행

개요 보기는 대부분의 동작 및 기능을 테이블 뷰와 공유 하므로이 문서를 계속 하기 전에 [표 뷰](~/mac/user-interface/table-view.md) 문서를 진행 하는 것이 좋습니다.

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode" />

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>Xcode에서 개요 보기 만들기 및 유지 관리

새 Xamarin.ios Cocoa 응용 프로그램을 만들면 기본적으로 표준 빈 창이 표시 됩니다. 이 창은 프로젝트에 자동으로 `.storyboard` 포함 되는 파일에 정의 됩니다. Windows 디자인을 편집 하려면 **솔루션 탐색기**에서 `Main.storyboard` 파일을 두 번 클릭 합니다.

[![](outline-view-images/edit01.png "주 스토리 보드 선택")](outline-view-images/edit01.png#lightbox)

이렇게 하면 Xcode의 Interface Builder에서 창 디자인이 열립니다.

[![](outline-view-images/edit02.png "Xcode에서 UI 편집")](outline-view-images/edit02.png#lightbox)

`outline` **라이브러리 검사기의** 검색 상자에를 입력 하 여 개요 보기 컨트롤을 보다 쉽게 찾을 수 있습니다.

[![](outline-view-images/edit03.png "라이브러리에서 개요 보기 선택")](outline-view-images/edit03.png#lightbox)

개요 뷰를 **인터페이스 편집기**에서 뷰 컨트롤러로 끌어 놓고 뷰 컨트롤러의 콘텐츠 영역을 채운 후 **제약 조건 편집기**에서 창으로 축소 하 고 증가 하는 위치로 설정 합니다.

[![](outline-view-images/edit04.png "제약 조건 편집")](outline-view-images/edit04.png#lightbox)

**인터페이스 계층 구조** 에서 개요 보기를 선택 하면 **특성 검사자**에서 다음 속성을 사용할 수 있습니다.

[![](outline-view-images/edit05.png "특성 검사자")](outline-view-images/edit05.png#lightbox)

- **윤곽 열** -계층적 데이터가 표시 되는 테이블 열입니다.
- **자동 저장 개요 열** - `true`이면 개요 열이 자동으로 저장 되 고 응용 프로그램 실행 사이에 복원 됩니다.
- **들여쓰기** -확장 된 항목에서 열을 들여쓸 크기입니다.
- **들여쓰기는 셀 뒤** 에 `true`표시 됩니다. 이면 들여쓰기 표시가 셀과 함께 들여쓰기 됩니다.
- **확장 된 항목 자동 저장** -인 경우 `true`항목의 확장/축소 상태가 자동으로 저장 되 고 응용 프로그램 실행 간에 복원 됩니다.
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
> 레거시 xamarin.ios 응용 프로그램을 유지 관리 하지 않는 경우 기반 `NSView` 개요 보기를 `NSCell` 기반으로 하는 테이블 뷰를 사용 해야 합니다. `NSCell`는 레거시로 간주 되며 향후 지원 되지 않을 수 있습니다.

**인터페이스 계층 구조** 에서 테이블 열을 선택 하면 **특성 검사자**에서 다음 속성을 사용할 수 있습니다.

[![](outline-view-images/edit06.png "특성 검사자")](outline-view-images/edit06.png#lightbox)

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

[![](outline-view-images/edit07.png "특성 검사자")](outline-view-images/edit07.png#lightbox)

이러한 속성은 모두 표준 보기의 속성입니다. 이 열에 대 한 행의 크기를 조정 하는 옵션도 있습니다.

`NSTextField` **인터페이스 계층 구조** 에서 테이블 뷰 셀 (기본적으로는)을 선택 하면 **특성 검사자**에서 다음 속성을 사용할 수 있습니다.

[![](outline-view-images/edit08.png "특성 검사자")](outline-view-images/edit08.png#lightbox)

여기에서 설정할 표준 텍스트 필드의 모든 속성을 갖게 됩니다. 기본적으로 표준 텍스트 필드는 열에 있는 셀에 대 한 데이터를 표시 하는 데 사용 됩니다.

**인터페이스 계층 구조** 에서 테이블 셀`NSTableFieldCell`뷰 ()를 선택 하면 **특성 검사자**에서 다음 속성을 사용할 수 있습니다.

[![](outline-view-images/edit09.png "특성 검사자")](outline-view-images/edit09.png#lightbox)

가장 중요 한 설정은 다음과 같습니다.

- **레이아웃** -이 열에 있는 셀의 레이아웃을 선택 하는 방법을 선택 합니다.
- **단일 줄 모드를 사용** 합니다 `true`. 인 경우 셀이 한 줄로 제한 됩니다.
- **첫 번째 런타임 레이아웃 너비** - `true`인 경우 셀은 응용 프로그램을 처음 실행할 때 표시 되는 너비 (수동 또는 자동)를 선호 합니다.
- **Action** -셀에 대 한 편집 **작업** 을 보내는 시기를 제어 합니다.
- **동작** -셀을 선택할 수 있는지 또는 편집할 수 있는지를 정의 합니다.
- **서식 있는 텍스트** - `true`인 경우 셀에 서식 지정 및 스타일 지정 된 텍스트가 표시 될 수 있습니다.
- **Undo** -이면 `true`셀에서 실행 취소 동작을 담당 하는 것으로 가정 합니다.

**인터페이스 계층 구조**에서 테이블 열의`NSTableFieldCell`맨 아래에 있는 테이블 셀 뷰 ()를 선택 합니다.

[![](outline-view-images/edit11.png "테이블 셀 보기 선택")](outline-view-images/edit10.png#lightbox)

이렇게 하면 지정 된 열에 대해 생성 된 모든 셀의 기본 _패턴_ 으로 사용 되는 테이블 셀 뷰를 편집할 수 있습니다.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>작업 및 콘센트 추가

다른 Cocoa UI 컨트롤과 마찬가지로 개요 뷰를 표시 하 고, 필요한 기능에 따라 **작업** 및 **콘센트** 를 사용 하 C# 여 코드에 열과 셀을 표시 해야 합니다.

이 프로세스는 표시 하려는 모든 개요 뷰 요소에 대해 동일 합니다.

1. **길잡이 편집기** 로 전환 하 여 `ViewController.h` 파일이 선택 되었는지 확인 합니다.

    [![](outline-view-images/edit11.png "올바른 .h 파일 선택")](outline-view-images/edit11.png#lightbox)
2. **인터페이스 계층 구조**에서 개요 뷰를 선택 하 고, 컨트롤을 클릭 한 다음 `ViewController.h` 파일을 끕니다.
3. 다음 이라는 `ProductOutline`개요 보기의 콘센트를 만듭니다.

    [![](outline-view-images/edit13.png "콘센트 구성")](outline-view-images/edit13.png#lightbox)
4. `ProductColumn` 및`DetailsColumn`라는 테이블 열에 대 한 콘센트를 만듭니다.

    [![](outline-view-images/edit14.png "콘센트 구성")](outline-view-images/edit14.png#lightbox)
5. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

다음으로, 응용 프로그램이 실행 될 때 개요에 대 한 일부 데이터를 표시 하는 코드를 작성 합니다.

<a name="Populating_the_Table_View" />

## <a name="populating-the-outline-view"></a>개요 보기 채우기

Interface Builder에서 설계 되 고 **콘센트**를 통해 노출 되는 개요 보기를 사용 하 여 다음 C# 코드를 작성 해야 합니다.

먼저 개별 행 및 하위 제품 그룹 `Product` 에 대 한 정보를 보유할 새 클래스를 만들어 보겠습니다. **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다. `Product` **일반**빈 클래스를 선택 하 고 이름으로를 입력 한 다음 새로 만들기 단추를 클릭 합니다. > 

[![](outline-view-images/populate01.png "빈 클래스 만들기")](outline-view-images/populate01.png#lightbox)

파일이 다음과 `Product.cs` 같이 표시 되도록 합니다.

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

다음으로 요청 될 때 개요에 대 한 `NSOutlineDataSource` 데이터를 제공 하는의 하위 클래스를 만들어야 합니다. **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다. `ProductOutlineDataSource` **일반**빈 클래스를 선택 하 고 이름으로를 입력 한 다음 새로 만들기 단추를 클릭 합니다. > 

`ProductTableDataSource.cs` 파일을 편집 하 여 다음과 같이 만듭니다.

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

이 클래스는 개요 보기의 항목에 대 한 저장소를 포함 `GetChildrenCount` 하 고를 재정의 하 여 테이블의 행 수를 반환 합니다. 는 `GetChild` 개요 뷰에서 요청한 대로 특정 부모 또는 자식 항목을 반환 하 고는 `ItemExpandable` 지정 된 항목을 부모 또는 자식으로 정의 합니다.

마지막으로의 `NSOutlineDelegate` 하위 클래스를 만들어 개요의 동작을 제공 해야 합니다. **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다. `ProductOutlineDelegate` **일반**빈 클래스를 선택 하 고 이름으로를 입력 한 다음 새로 만들기 단추를 클릭 합니다. > 

`ProductOutlineDelegate.cs` 파일을 편집 하 여 다음과 같이 만듭니다.

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

인스턴스 `ProductOutlineDelegate`를 만들 때 개요에 대 한 데이터를 제공 `ProductOutlineDataSource` 하는의 인스턴스도 전달 합니다. 메서드 `GetView` 는 열 및 행에 대 한 셀을 표시 하기 위해 뷰 (데이터)를 반환 합니다. 가능 하면 새 보기를 만들어야 하는 경우 기존 뷰를 다시 사용 하 여 셀을 표시 합니다.

개요를 채우기 위해 `MainWindow.cs` 파일을 편집 하 고 메서드를 `AwakeFromNib` 다음과 같이 만듭니다.

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

개요 보기에서 노드를 확장 하는 경우 다음과 같습니다.

[![](outline-view-images/populate03.png "확장 된 보기")](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>열별로 정렬

사용자가 열 머리글을 클릭 하 여 윤곽선의 데이터를 정렬할 수 있습니다. 먼저 `Main.storyboard` 파일을 두 번 클릭 하 여 Interface Builder에서 편집할 수 있도록 엽니다. `Title` `compare:` `Ascending`열을 선택 하 고, 정렬 키로를 입력 하 고, 선택기에 대해를 입력 하 고, 순서를 선택 합니다. `Product`

[![](outline-view-images/sort01.png "정렬 키 순서 설정")](outline-view-images/sort01.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

이제 파일을 `ProductOutlineDataSource.cs` 편집 하 고 다음 메서드를 추가 해 보겠습니다.

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

메서드 `Sort` 를 사용 하면 지정 `Product` 된 클래스 필드를 기반으로 하 여 데이터 소스의 데이터를 오름차순 또는 내림차순으로 정렬할 수 있습니다. 재정의 `SortDescriptorsChanged` 된 메서드는가 열 머리글을 클릭할 때마다 호출 됩니다. Interface Builder에 설정 된 **키** 값과 해당 열에 대 한 정렬 순서를 전달 합니다.

응용 프로그램을 실행 하 고 열 머리글을 클릭 하면 해당 열을 기준으로 행이 정렬 됩니다.

[![](outline-view-images/sort02.png "정렬 된 출력의 예")](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>행 선택

사용자가 단일 행을 선택할 수 있도록 하려면 `Main.storyboard` 파일을 두 번 클릭 하 여 Interface Builder에서 편집할 수 있도록 엽니다. **인터페이스 계층 구조** 에서 개요 보기를 선택 하 고 **특성 검사자**에서 **여러** 확인란의 선택을 취소 합니다.

[![](outline-view-images/select01.png "특성 검사자")](outline-view-images/select01.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

그런 다음 `ProductOutlineDelegate.cs` 파일을 편집 하 고 다음 메서드를 추가 합니다.

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

이렇게 하면 사용자가 개요 보기에서 단일 행을 선택할 수 있습니다. 사용자 `false` 가 항목 `ShouldSelectItem` 을 선택할 수 없도록 하려는 경우 사용자가 선택할 수 없도록 하거나 `false` 모든 항목에 대해를 반환 합니다.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>여러 행 선택

사용자가 여러 행을 선택할 수 있도록 하려면 `Main.storyboard` 파일을 두 번 클릭 하 여 Interface Builder에서 편집할 수 있도록 엽니다. **인터페이스 계층 구조** 에서 개요 보기를 선택 하 고 **특성 검사자**에서 **여러** 확인란을 선택 합니다.

[![](outline-view-images/select02.png "특성 검사자")](outline-view-images/select02.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

그런 다음 `ProductOutlineDelegate.cs` 파일을 편집 하 고 다음 메서드를 추가 합니다.

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

이렇게 하면 사용자가 개요 보기에서 단일 행을 선택할 수 있습니다. 사용자 `false` 가 항목 `ShouldSelectRow` 을 선택할 수 없도록 하려는 경우 사용자가 선택할 수 없도록 하거나 `false` 모든 항목에 대해를 반환 합니다.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>입력 하 여 행 선택

사용자가 개요 보기가 선택 된 문자를 입력 하 고 해당 문자가 포함 된 첫 번째 행을 선택 하려면 `Main.storyboard` 파일을 두 번 클릭 하 여 Interface Builder에서 편집할 수 있도록 엽니다. **인터페이스 계층 구조** 에서 개요 보기를 선택 하 고 **특성 검사자**에서 **유형 선택** 확인란을 선택 합니다.

[![](outline-view-images/type01.png "행 형식 편집")](outline-view-images/type01.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

이제 파일을 `ProductOutlineDelegate.cs` 편집 하 고 다음 메서드를 추가 해 보겠습니다.

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

메서드 `GetNextTypeSelectMatch` 는 지정 `searchString` 된를 가져와서 해당 문자열이 `Title`있는 첫 번째 `Product` 의 항목을 반환 합니다.

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>열 다시 정렬

사용자가 개요 보기에서 열 순서를 끌 수 있도록 하려면 `Main.storyboard` 파일을 두 번 클릭 하 여 Interface Builder에서 편집할 수 있도록 엽니다. **인터페이스 계층 구조** 에서 개요 뷰를 선택 하 고 **특성 검사자**에서 다시 **정렬** 확인란을 선택 합니다.

[![](outline-view-images/reorder01.png "특성 검사자")](outline-view-images/reorder01.png#lightbox)

**자동** 저장 속성의 값을 지정 하 고 **열 정보** 필드를 확인 하는 경우 테이블의 레이아웃에 대 한 모든 변경 내용은 자동으로 저장 되 고 다음에 응용 프로그램이 실행 될 때 복원 됩니다.

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

이제 파일을 `ProductOutlineDelegate.cs` 편집 하 고 다음 메서드를 추가 해 보겠습니다.

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

메서드 `ShouldReorder` 는로 다시 `true` 정렬 `newColumnIndex`되도록 허용할 모든 열에 대해를 반환 해야 하며, 그렇지 않으면를 반환 `false`합니다.

응용 프로그램을 실행 하는 경우 열 머리글을 끌어 열의 순서를 바꿀 수 있습니다.

[![](outline-view-images/reorder02.png "열 다시 정렬 예제")](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>셀 편집

사용자가 지정 된 셀에 대 한 값을 편집할 수 있게 하려면 `ProductOutlineDelegate.cs` 파일을 편집 하 고 메서드를 `GetViewForItem` 다음과 같이 변경 합니다.

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

이제 응용 프로그램을 실행 하는 경우 사용자는 테이블 뷰의 셀을 편집할 수 있습니다.

[![](outline-view-images/editing01.png "셀 편집 예제")](outline-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Outline_Views" />

## <a name="using-images-in-outline-views"></a>개요 뷰에서 이미지 사용

`NSOutlineView`에서 셀의 일부로 이미지를 포함 하려면 개요 `NSTableViewDelegate's` `GetView` 뷰의 메서드에서 데이터를 반환 하는 방법을 변경 하 여 일반적인 `NSTextField`대신를 `NSTableCellView` 사용 해야 합니다. 예를 들어:

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

자세한 내용은 [이미지 작업](~/mac/app-fundamentals/image.md) 설명서의 [개요 뷰에서 이미지 사용](~/mac/app-fundamentals/image.md) 섹션을 참조 하세요.

<a name="Data_Binding_Outline_Views" />

## <a name="data-binding-outline-views"></a>데이터 바인딩 개요 뷰

Xamarin.ios 응용 프로그램에서 키-값 코딩 및 데이터 바인딩 기술을 사용 하 여 UI 요소를 채우고 사용 하기 위해 작성 하 고 유지 관리 해야 하는 코드의 양을 크게 줄일 수 있습니다. 또한 프런트 엔드 사용자 인터페이스 (_모델-뷰-컨트롤러_)에서 지원 데이터 (_데이터 모델_)를 추가로 분리 하 여 더 쉽게 유지 관리 하 고 더욱 유연한 응용 프로그램을 디자인할 수 있는 이점을 누릴 수 있습니다.

KVC (키-값 코딩)는 키 (특수 형식의 문자열)를 사용 하 여 개체의 속성에 간접적으로 액세스 하 고 인스턴스 변수 또는 접근자 메서드 (`get/set`)를 통해 액세스 하는 대신 속성을 식별 하는 메커니즘입니다. Xamarin.ios 응용 프로그램에서 키-값 코딩 규격 접근자를 구현 하 여 키-값 관찰 (KVO), 데이터 바인딩, 코어 데이터, Cocoa 바인딩, scriptability 등의 다른 macOS 기능에 액세스할 수 있습니다.

자세한 내용은 [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md) 설명서의 [개요 뷰 데이터 바인딩](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding) 섹션을 참조 하세요.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 응용 프로그램에서 개요 보기를 사용 하는 방법을 자세히 살펴봅니다. Xcode의 Interface Builder에서 개요 보기를 만들고 유지 관리 하는 방법 및 코드에서 C# 개요 보기를 사용 하는 방법에 대 한 다양 한 유형 및 사용 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [MacOutlines (샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/macoutlines)
- [MacImages(샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/macimages)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [테이블 보기](~/mac/user-interface/table-view.md)
- [원본 목록](~/mac/user-interface/source-list.md)
- [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [개요 보기 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
