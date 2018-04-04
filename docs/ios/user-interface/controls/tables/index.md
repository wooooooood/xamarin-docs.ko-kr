---
title: 테이블 및 셀 작업
description: Xamarin.iOS UITableView 사용 하 여 데이터를 표시 합니다.
ms.prod: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/06/2016
ms.openlocfilehash: a1cda3632a75c7e462e763a34fdb5b586237b670
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-tables-and-cells"></a>테이블 및 셀 작업


이 섹션을 만들고 테이블을 표시 하는 데 사용 되는 클래스에서는 다음 Xamarin.iOS에서 사용 하는 방법의 예제를 제공 합니다. Xamarin iOS 디자이너를 사용 하 여 테이블을 시각적으로 디자인 하 고 편집을 구현 하 여 레이아웃을 사용자 지정 테이블에 대 한 기본 모양을 사용 하 여 적용 됩니다. 경우에 따라 디스플레이 분명히 행 (예: 음악 응용 프로그램) 및 다른 이유 (예: 연락처 앱 또는 대화에서 메시지 앱에서 편집) 테이블 컨트롤을 인식 하는 것이 어렵습니다의 목록입니다.

Xamarin.Android 플랫폼 간 응용 프로그램에서 작업을 위해 UITableView 제어는 Android에서 ListView 클래스 비슷합니다 (및 UITableViewSource 클래스는 Android의 어댑터 클래스와 유사).

이러한 문서에는 테이블을 포함 하 여 작업에 대 한 포괄적인 개요를 소요 됩니다.

-   **파트 테이블** – 소개 및 설명의 시각적 요소는 `UITableView` 제어 합니다. 
-   **테이블에서 데이터 표시** -서로 다른 테이블 및 셀 스타일을 사용 하 여 만들고 테이블을 입력 하는 방법을 보여 주는, 및 셀 개체를 재활용 하 여 메모리 문제를 방지 합니다. 
-   **고급 사용** – 사용자 지정 셀을 구축 하 고 UITableView 클래스의 편집 기능을 사용 합니다. 
-   **테이블을 시각적으로 만들** – iOS 용 Xamarin 디자이너를 사용 하 여 스토리 보드 테이블 기반 인터페이스를 만듭니다. 


## <a name="contents"></a>목차

 [파트 테이블 &amp; 기능](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [데이터로 테이블 채우기](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [테이블 모양 사용자 지정](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [편집](~/ios/user-interface/controls/tables/editing.md)
 
 [행 작업](~/ios/user-interface/controls/tables/row-action.md)

 [스토리 보드에서 표 만들기](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)
 
 [행 높이 자동 크기 조정](~/ios/user-interface/controls/tables/autosizing-row-height.md)


## <a name="related-links"></a>관련 링크

- [WorkingWithTables (샘플)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables/)
- [테이블에 스토리 보드 (샘플)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Storyboards 소개](~/ios/user-interface/storyboards/index.md)
- [스토리 보드 TableView 레시피](https://developer.xamarin.com/recipes/ios/general/storyboard/storyboard_a_tableview)
- [MonoTouch.Dialog 소개](~/ios/user-interface/monotouch.dialog/index.md)
- [Github에 TableEditing 샘플](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [Github에 TableParts 샘플](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [Github에 TableAndCellStyles 샘플](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [UITableView 클래스 참조](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [UITableViewCell 클래스 참조](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [UITableViewDelegate](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [UITableViewDataSource](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)
