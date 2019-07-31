---
title: Xamarin.ios의 테이블 및 셀 작업
description: 이 문서는 Xamarin.ios 앱에서 UITableView 컨트롤을 사용 하 여 데이터를 표시 하는 방법을 설명 하는 다양 한 가이드에 연결 됩니다.
ms.prod: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 01/06/2016
ms.openlocfilehash: d01814bc241dcfb7b62f40bef226ee720a96ff23
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656999"
---
# <a name="working-with-tables-and-cells-in-xamarinios"></a>Xamarin.ios의 테이블 및 셀 작업

이 섹션에서는 테이블을 만들고 표시 하는 데 사용 되는 클래스를 소개 하 고 Xamarin.ios에서 사용 하는 방법에 대 한 예제를 제공 합니다. 테이블에 대 한 기본 모양을 사용 하 고, 레이아웃을 사용자 지정 하며, 편집을 구현 하 고 Xamarin iOS Designer를 사용 하 여 테이블을 시각적으로 디자인 하는 방법을 다룹니다. 경우에 따라 표시 되는 행의 목록 (예: 음악 앱) 및 테이블 컨트롤을 인식 하기 어려운 다른 시간 (예: 연락처 앱에서 편집 또는 메시지의 대화)이 표시 됩니다.

Xamarin.ios를 사용 하 여 플랫폼 간 응용 프로그램에서 작업 하는 경우 UITableView 컨트롤은 Android의 ListView 클래스와 유사 합니다. UITableViewSource 클래스는 Android의 어댑터 클래스와 유사 합니다.

다음 문서에서는 다음을 포함 하 여 테이블을 사용 하는 방법에 대해 자세히 살펴보겠습니다.

-   **테이블 파트** – `UITableView` 컨트롤의 시각적 요소를 소개 하 고 설명 합니다. 
-   **테이블에 데이터 표시** – 테이블을 만들고 채우고, 다른 테이블 및 셀 스타일을 사용 하 고, 셀 개체를 재생 하 여 메모리 문제를 방지 하는 방법을 보여 줍니다. 
-   **고급 사용** – 사용자 지정 셀을 빌드하고 UITableView 클래스의 편집 기능을 사용 합니다. 
-   **시각적으로 테이블 만들기** -Xamarin Designer for iOS를 사용 하 여 Storyboard를 사용 하 여 테이블 기반 인터페이스를 만듭니다. 

## <a name="contents"></a>목차

 [테이블 파트 &amp; 기능](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [데이터로 테이블 채우기](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [테이블 모양 사용자 지정](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [편집](~/ios/user-interface/controls/tables/editing.md)
 
 [행 작업](~/ios/user-interface/controls/tables/row-action.md)

 [스토리 보드에 테이블 만들기](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)
 
 [행 높이 자동 크기 조정](~/ios/user-interface/controls/tables/autosizing-row-height.md)

## <a name="related-links"></a>관련 링크

- [WorkingWithTables (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
- [스토리 보드의 테이블 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/storyboardtable)
- [Storyboards 소개](~/ios/user-interface/storyboards/index.md)
- [TableView 조리법 Storyboard](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/storyboard_a_tableview)
- [Monotouch.dialog 소개. 대화 상자](~/ios/user-interface/monotouch.dialog/index.md)
- [Github의 TableEditing 샘플](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [Github의 TableParts 샘플](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [Github의 TableAndCellStyles 샘플](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [UITableView 클래스 참조](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [UITableViewCell 클래스 참조](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [UITableViewDelegate](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [UITableViewDataSource](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)
