---
title: 테이블 파트 및 기능
ms.prod: xamarin
ms.assetid: B4139C8B-28F2-4C0F-297F-BF5432C5A915
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: be8ad21847aed3d8ad9bda7ac45b070e5ad868bc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="table-parts-and-functionality"></a>테이블 파트 및 기능

UITableView '그룹화' 또는 '일반' 스타일을 가질 수 있습니다 및 다음과 같은 부분으로 구성 됩니다.

-  [섹션 헤더](#Section_Header)
-  [셀](#Cells) (또는 행을 원하는 경우)
-  [바닥글 섹션](#Section_Footer)
-  [Index](#Index)
-  [편집 모드](#Edit_Features) ('삭제를 살짝 밉니다' 포함 핸들을 끌어 행 순서를 변경 하 고) 

이러한 스크린샷 섹션 행, 머리글, 바닥글, 편집 컨트롤 및 인덱스 표시 되는 방법을 보여 줍니다.

 [![](table-parts-and-functionality-images/image1a.png "이러한 스크린샷 섹션 행, 머리글, 바닥글, 편집 컨트롤 및 인덱스 표시 되는 방법을 보여 줍니다.")](table-parts-and-functionality-images/image1a.png#lightbox)

이러한 파트는 아래에 자세히 설명 되어 있습니다.

<a name="Section_Header" />

## <a name="section-header"></a>섹션 헤더

셀 수 필요에 따라 수 섹션으로 그룹화, 사용자 지정 헤더를 붙이지 및/또는 바닥글은 레이블로 지정 합니다. 다른 레이아웃 또는 스타일에 대 한 허용 하려면 사용자 지정 보기를 제공할 수 있습니다 또는 문자열 값을 가진 헤더를 설정할 수 있습니다.

<a name="Cells" />

## <a name="cells"></a>셀

셀은 테이블에 대 한 기본 사용자 인터페이스 요소입니다. 제대로 구현 되는 경우 셀은 메모리 효율성을 위해 다시 사용 됩니다. 네 가지 기본 제공 된 셀 스타일 있으며 스토리 보드를 사용 하는 경우, 코드 또는 디자이너에서 사용자 고유의 사용자 지정 셀 – 만들 수 있습니다.

<a name="Section_Footer"/>

## <a name="section-footer"></a>바닥글 섹션

선택적 섹션 바닥글 문자열 값으로 설정할 수 있습니다 또는 다른 레이아웃 또는 스타일에 대 한 허용 하려면 사용자 지정 보기를 제공할 수 있습니다. 섹션 머리글 및 바닥글 수 독립적으로 설정 합니다.

<a name="Index" />

## <a name="index"></a>인덱스

인덱스는 테이블의 오른쪽 가장자리 아래로 문자의 스트립으로 나타납니다.
셀에 접해 있거나 인덱스에 끌어 테이블의 해당 부분을 찾아 스크롤하기가 가속화 됩니다. 인덱스는 선택 사항 이지만 긴 목록을 탐색할 수 있도록 도와주 것이 좋습니다. 인덱스는 그룹화 스타일을 일반적으로 사용 되지 않습니다.

<a name="Edit_Features" />

## <a name="editing-mode"></a>편집 모드

사용할 수 있는 몇 가지 다른 편집 기능이 있습니다.

- 개별 셀 삭제을 살짝 밉니다.
- 각 행에서 삭제 단추를 표시 하기 위해 편집 모드로 전환 
- 다시 정렬 핸들을 표시 하기 위해 편집 모드를 입력 합니다. 
- 새 셀 (애니메이션)으로 삽입할 수 없습니다.

이 문서의 나머지 부분에서는 이러한 모든 UITableView 기능 Xamarin.iOS 구현 하는 방법을 보여 줍니다.


## <a name="classes-overview"></a>클래스 개요

테이블 보기를 표시 하는 데 사용 되는 기본 클래스는 다음과 같습니다.

[![](table-parts-and-functionality-images/classdiagram.png "테이블 보기를 표시 하는 데 사용 되는 기본 클래스는 같습니다.")](table-parts-and-functionality-images/classdiagram.png#lightbox)

다음은 각 클래스의 용도 대 한 설명입니다.

- **UITableView** – 스크롤 컨테이너 내부 셀의 컬렉션을 포함 하는 뷰입니다. IPhone 앱의 테이블 뷰 일반적으로 사용 하 여 전체 화면 있지만 iPad에서 더 크게 보려면의 일부로 (수도 있습니다는 popover에 표시). 
- **UITableViewCell** – 표 보기에서 단일 셀 (또는 행)을 나타내는 보기. 네 가지 기본 제공 된 셀 유형 및 ios 디자이너 또는 C#에서 사용자 지정 셀을 만들 수 있습니다. 
- **UITableViewSource** – Xamarin.iOS 배타적 행 개수를 포함 하 여, 각 행에 대 한 셀 뷰를 반환 하 고, 행 선택 및 기타 여러 가지 선택적 기능을 처리 하는 테이블을 표시 하는 데 필요한 모든 메서드를 제공 하는 추상 클래스입니다. 하면 *해야* 하위 클래스 UITableView 사용 하려면이 있습니다. 
- **NSIndexPath** – 테이블에서 셀의 위치를 고유 하 게 식별 하는 Contains 행 및 섹션 속성입니다. 
- **UITableViewController** –를 뷰로 및 TableView 속성을 통해 액세스할 수 있는 UITableView 하드 코드를 포함 하는 즉시 사용할 UIViewController 합니다. 
- **UIViewController** – 테이블의 프레임으로 모든 UIViewController UITableView 적절 하 게 설정 추가할 수는 전체 화면을 차지 하지 않습니다. 

UITableViewSource는 Xamarin.iOS에서 계속 제공 하지만 일반적으로 필요 하지 않은 다음과 같은 두 클래스를 대체 합니다.

- **UITableViewDataSource** – An Objective-c 프로토콜 Xamarin.iOS 클래스는 추상 클래스에서 모델링 된입니다. 머리글, 바닥글 및 행과 테이블의 섹션 수에 대 한 정보 뿐만 아니라 각 셀에 대 한 테이블 보기에 제공 하려면 서브클래싱 할 해야 합니다. 
- **UITableViewDelegate** – An Objective-c 프로토콜 클래스로 Xamarin.iOS에서 모델링 된입니다. 선택 항목을 편집 기능과 기타 선택적 테이블 기능을 처리 합니다. 

이 문서의 예제 UITableViewSource 모두 사용 하 고이 두 클래스를 무시 합니다. Objective C 예제를 Apple 설명서에에서 나와 있는 개체를 참조 하므로 수행할 작업 (및 Xamarin.iOS의 UITableViewSource을 대신 사용할 수 있는)를 이해 하는 데 유용 하기 때문에 여기 언급 됩니다.

## <a name="related-links"></a>관련 링크

- [WorkingWithTables (샘플)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
