---
title: 테이블 파트 및 Xamarin.iOS에서 기능
description: 이 문서는 iOS에서 UITableView 다양 한 정보를 설명합니다. 섹션 헤더, 셀, 섹션 바닥글, 인덱스 및 편집 모드에 설명 합니다.
ms.prod: xamarin
ms.assetid: B4139C8B-28F2-4C0F-297F-BF5432C5A915
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: c4d788cce12a9aabdd1170cd1a52915f3b30285f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113952"
---
# <a name="table-parts-and-functionality-in-xamarinios"></a>테이블 파트 및 Xamarin.iOS에서 기능

UITableView '그룹화' 또는 '일반' 스타일을 가질 수 있습니다 및 다음 부분으로 구성 됩니다.

-  [섹션 헤더](#Section_Header)
-  [셀](#Cells) (또는 행을 원하는 경우)
-  [바닥글 섹션](#Section_Footer)
-  [Index](#Index)
-  [편집 모드](#Edit_Features) ('삭제를 살짝 밉니다' 포함 핸들을 끌어 행 순서를 변경 하 고) 

이러한 스크린샷은 섹션 행, 머리글, 바닥글, 편집 컨트롤 및 인덱스 표시 되는 방식을 보여 줍니다.

 [![](table-parts-and-functionality-images/image1a.png "이러한 스크린샷은 섹션 행, 머리글, 바닥글, 편집 컨트롤 및 인덱스 표시 되는 방식을 보여 줍니다.")](table-parts-and-functionality-images/image1a.png#lightbox)

이러한 파트는 아래에서 자세히 설명 되어 있습니다.

<a name="Section_Header" />

## <a name="section-header"></a>섹션 헤더

셀 수 필요에 따라 수 섹션으로 그룹화, 사용자 지정 헤더를 사용 하 여 레이블이 지정 된 및/또는 바닥글을 사용 하 여 레이블이 지정 합니다. 문자열 값을 가진 헤더를 설정할 수 있습니다 또는 레이아웃이 나 스타일을 허용 하도록 사용자 지정 보기를 제공할 수 있습니다.

<a name="Cells" />

## <a name="cells"></a>셀

셀은 테이블에 대 한 기본 사용자 인터페이스 요소입니다. 올바르게 구현 되는 경우 셀은 메모리 효율성을 위해 다시 사용 됩니다. 네 가지 기본 셀 스타일 되며 스토리 보드를 사용 하는 경우 코드 또는 디자이너에서 사용자 고유의 사용자 지정 셀 – 만들 수 있습니다.

<a name="Section_Footer"/>

## <a name="section-footer"></a>바닥글 섹션

선택적인 섹션 바닥글 문자열 값으로 설정할 수 있습니다 또는 레이아웃이 나 스타일을 허용 하도록 사용자 지정 보기를 제공할 수 있습니다. 섹션 헤더 및 바닥글 수 독립적으로 설정 합니다.

<a name="Index" />

## <a name="index"></a>인덱스

인덱스는 테이블의 오른쪽 가장자리 아래로 문자의 스트립으로 나타납니다.
인덱스에 대해 끌거나 터치 스크롤 테이블의 해당 부분을 가속화 합니다. 인덱스는 선택 사항 이지만, 긴 목록을 이동 하는 데 좋습니다. 인덱스는 그룹화 스타일을 사용 하 여 일반적으로 사용 되지 않습니다.

<a name="Edit_Features" />

## <a name="editing-mode"></a>편집 모드

가지 몇 가지 다른 편집 기능이 제공 됩니다.

- 개별 셀 삭제을 살짝 밉니다.
- 각 행의 삭제 단추를 표시 하기 위해 편집 모드로 전환 
- 다시 정렬 핸들을 표시 하기 위해 편집 모드를 입력 합니다. 
- (사용 하 여 애니메이션) 새 셀을 삽입 합니다.

이 문서의 나머지 부분에 Xamarin.iOS 사용 하 여 이러한 모든 UITableView 기능을 구현 하는 방법을 보여 줍니다.


## <a name="classes-overview"></a>클래스 개요

테이블 보기를 표시 하는 데는 주 클래스는 다음과 같습니다.

[![](table-parts-and-functionality-images/classdiagram.png "테이블 보기를 표시 하는 데는 주 클래스는 같습니다.")](table-parts-and-functionality-images/classdiagram.png#lightbox)

각 클래스의 용도 대 한 설명은 다음과 같습니다.

- **UITableView** – 스크롤 컨테이너 내에서 셀의 컬렉션을 포함 하는 뷰입니다. IPhone 앱에서 테이블 보기 일반적으로 사용 하 여 전체 화면 하지만 더 크게 보려면 iPad의 일부로 존재 (수도 있습니다는 팝 오버에서 표시). 
- **UITableViewCell** – 표 보기에서 단일 셀 (또는 행)를 나타내는 보기. 네 가지 기본 제공 셀 유형 및에서 모두 사용자 지정 셀을 만드는 것이 불가능 C# 또는 iOS 디자이너를 사용 하 여 합니다. 
- **UITableViewSource** – Xamarin.iOS 배타적 테이블 행 개수를 포함 하 여, 각 행에 대해 셀 보기를 반환 하 고, 행 선택 및 기타 다양 한 선택적 기능 처리를 표시 하는 데 필요한 모든 메서드를 제공 하는 추상 클래스입니다. 있습니다 *해야* 서브 클래스 UITableView 작동 하기 위해 본입니다. 
- **NSIndexPath** – 테이블에서 셀의 위치를 고유 하 게 식별 하는 Contains 행 및 섹션 속성입니다. 
- **UITableViewController** – 해당 뷰로 및 TableView 속성을 통해 액세스할 수는 UITableView 하드 코드를 포함 하는 즉시 사용할 UIViewController 합니다. 
- **UIViewController** – 테이블에 해당 프레임을 사용 하 여 모든 UIViewController UITableView 적절 하 게 설정를 추가할 수 있습니다 하는 전체 화면을 차지 하지 않습니다. 

UITableViewSource는 Xamarin.iOS에서 사용할 수 있지만 일반적으로 필요 하지 않습니다 하는 다음 두 개의 클래스를 대체 합니다.

- **UITableViewDataSource** –는 Objective-c 프로토콜 클래스는 추상 클래스는 Xamarin.iOS에서 모델링 됩니다. 머리글과 바닥글 행 및 테이블의 섹션 수에 대 한 정보 뿐만 아니라 각 셀에 대 한 뷰를 사용 하 여 테이블을 제공 하 서브클래싱 해야 합니다. 
- **UITableViewDelegate** –는 Objective-c 프로토콜 클래스로 Xamarin.iOS에서 모델링 됩니다. 선택 항목을 편집 기능과 기타 선택적 테이블 기능을 처리 합니다. 

이 문서의 예제 UITableViewSource 모두 사용 하 고 이러한 두 클래스를 무시 합니다. Apple 설명서에에서 나와 있는 모든 Objective-c 예제 개체를 참조 하므로 수행할 작업 (및 Xamarin.iOS의 UITableViewSource을 대신 사용할 수 있는)를 이해 하는 데 유용 하기 때문에 여기 언급 됩니다.

## <a name="related-links"></a>관련 링크

- [WorkingWithTables (샘플)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
