---
title: Xamarin.ios의 테이블 파트 및 기능
description: 이 문서에서는 iOS에서 UITableView의 다양 한 부분에 대해 설명 합니다. 이 단원에서는 머리글, 셀, 구역 바닥글, 인덱스 및 편집 모드에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: B4139C8B-28F2-4C0F-297F-BF5432C5A915
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: d6ad088f9223dccb1966148fe8f53d76e85040a6
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68645598"
---
# <a name="table-parts-and-functionality-in-xamarinios"></a>Xamarin.ios의 테이블 파트 및 기능

UITableView는 ' 그룹화 ' 또는 ' 일반 ' 스타일을 포함할 수 있으며 다음과 같은 부분으로 구성 됩니다.

-  [섹션 헤더](#Section_Header)
-  [셀](#Cells) (또는 원하는 행을 원하는 경우)
-  [섹션 바닥글](#Section_Footer)
-  [Index](#Index)
-  [편집 모드](#Edit_Features) (' 안쪽으로 살짝 밀기 ' 및 끌어서 핸들을 행 순서 변경) 

이러한 스크린샷에는 섹션 행, 머리글, 바닥글, 편집 컨트롤 및 인덱스가 표시 되는 방식이 나와 있습니다.

 [![](table-parts-and-functionality-images/image1a.png "이러한 스크린샷에는 섹션 행, 머리글, 바닥글, 편집 컨트롤 및 인덱스가 표시 되는 방식이 나와 있습니다.")](table-parts-and-functionality-images/image1a.png#lightbox)

이러한 부분에 대해서는 아래에서 자세히 설명 합니다.

<a name="Section_Header" />

## <a name="section-header"></a>섹션 헤더

필요에 따라 셀을 섹션으로 그룹화 하 고 사용자 지정 머리글을 사용 하 여 레이블을 지정 하거나 바닥글을 사용 하 여 레이블을 지정할 수 있습니다. 헤더는 문자열 값을 사용 하 여 설정 하거나 사용자 지정 보기를 제공 하 여 다른 레이아웃이 나 스타일을 허용할 수 있습니다.

<a name="Cells" />

## <a name="cells"></a>셀

셀은 테이블의 기본 사용자 인터페이스 요소입니다. 올바르게 구현 된 경우 셀은 메모리 효율성을 위해 다시 사용 됩니다. 네 가지 기본 제공 셀 스타일이 있습니다. 코드에서 또는 스토리 보드를 사용할 때 디자이너에서 사용자 지정 셀을 직접 만들 수 있습니다.

<a name="Section_Footer"/>

## <a name="section-footer"></a>섹션 바닥글

선택적 섹션 바닥글은 문자열 값을 사용 하 여 설정 하거나 사용자 지정 보기를 제공 하 여 다른 레이아웃이 나 스타일을 허용할 수 있습니다. 섹션 머리글 및 바닥글은 독립적으로 설정할 수 있습니다.

<a name="Index" />

## <a name="index"></a>인덱스

인덱스는 테이블의 오른쪽 가장자리 아래에 문자 구획으로 나타납니다.
인덱스에 대 한 터치 또는 끌기는 테이블의 해당 부분으로 스크롤을 가속화 합니다. 인덱스는 선택 사항 이지만 긴 목록을 탐색 하는 데 도움이 되는 것이 좋습니다. 인덱스는 일반적으로 그룹화 된 스타일과 함께 사용 되지 않습니다.

<a name="Edit_Features" />

## <a name="editing-mode"></a>편집 모드

다음과 같은 몇 가지 편집 기능을 사용할 수 있습니다.

- 개별 셀을 삭제 하려면 살짝 밉니다.
- 편집 모드를 입력 하 여 각 행에 대 한 삭제 단추 표시 
- 다시 정렬 하는 핸들을 표시 하는 편집 모드를 시작 합니다. 
- 애니메이션을 사용 하 여 새 셀 삽입

이 문서의 나머지 부분에서는 Xamarin.ios를 사용 하 여 이러한 모든 UITableView 기능을 구현 하는 방법을 보여 줍니다.


## <a name="classes-overview"></a>클래스 개요

테이블 뷰를 표시 하는 데 사용 되는 기본 클래스는 다음과 같습니다.

[![](table-parts-and-functionality-images/classdiagram.png "테이블 뷰를 표시 하는 데 사용 되는 기본 클래스는 다음과 같습니다.")](table-parts-and-functionality-images/classdiagram.png#lightbox)

각 클래스의 용도는 아래에 설명 되어 있습니다.

- **Uitableview** – 스크롤 컨테이너 내의 셀 컬렉션을 포함 하는 뷰입니다. 일반적으로 테이블 뷰는 iPhone 앱에서 전체 화면을 사용 하지만 iPad에서 더 큰 보기의 일부로 존재 하거나 팝 오버 표시 될 수 있습니다. 
- **Uitableviewcell** – 테이블 뷰의 단일 셀 또는 행을 나타내는 뷰입니다. 네 가지 기본 제공 셀 형식이 있으며, C# 또는 iOS 디자이너를 사용 하 여 사용자 지정 셀을 만들 수 있습니다. 
- **Uitableviewsource** – xamarin.ios-배타적 추상 클래스로, 행 개수, 각 행에 대 한 셀 뷰 반환, 행 선택 처리 및 기타 여러 가지 선택적 기능을 포함 하 여 테이블을 표시 하는 데 필요한 모든 메서드를 제공 합니다. UITableView를 가져오려면이를 서브 클래스 *해야* 합니다. 
- **Nsindexpath** – 테이블에서 셀의 위치를 고유 하 게 식별 하는 Row 및 Section 속성을 포함 합니다. 
- **Uitableviewcontroller** – TableView 속성을 통해 해당 뷰로 하드 코딩 되 고 액세스 가능한 UITableView를 포함 하는 바로 사용할 수 있는 uiviewcontroller입니다. 
- **Uiviewcontroller** – 테이블이 전체 화면을 사용 하지 않는 경우 해당 프레임을 적절 하 게 설정 하 여 Uiviewcontroller에 UITableView를 추가할 수 있습니다. 

UITableViewSource는 Xamarin.ios에서 여전히 사용할 수 있지만 일반적으로 필요 하지 않은 다음 두 클래스를 대체 합니다.

- **Uitableviewdatasource** – xamarin.ios에서 추상 클래스로 모델링 되는 목표-C 프로토콜입니다. 테이블에 각 셀에 대 한 뷰를 제공 하 고 테이블의 머리글, 바닥글 및 행과 섹션 수에 대 한 정보를 제공 하려면 서브클래싱된를 사용 해야 합니다. 
- **Uitableviewdelegate** – xamarin.ios에서 클래스로 모델링 된 목표-C 프로토콜입니다. 선택, 편집 기능 및 기타 선택적 테이블 기능을 처리 합니다. 

이 문서에서 예제는 모두 UITableViewSource를 사용 하 고이 두 클래스를 무시 합니다. Apple 설명서에 있는 모든 목표-C 예제가이를 참조 하기 때문에 여기에 설명 되어 있습니다. 따라서 사용 하는 작업을 이해 하 고, 대신 Xamarin.ios의 UITableViewSource를 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [WorkingWithTables (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
