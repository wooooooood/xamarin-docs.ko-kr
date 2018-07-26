---
title: 테이블 및 셀 Xamarin.iOS에서 작업
description: 이 문서는 Xamarin.iOS 앱에 UITableView 컨트롤을 사용 하 여 데이터를 표시 하는 방법을 설명 하는 다양 한 설명서를 링크 합니다.
ms.prod: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/06/2016
ms.openlocfilehash: ea5b6ba532d577bd503529065eef803acf3a7aa9
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241700"
---
# <a name="working-with-tables-and-cells-in-xamarinios"></a>테이블 및 셀 Xamarin.iOS에서 작업

이 섹션을 만들고 테이블을 표시 하는 데 클래스를 소개 다음 Xamarin.iOS에서 사용 하는 방법의 예제를 제공 합니다. 테이블을 편집 하 고 Xamarin iOS 디자이너를 사용 하 여 테이블을 시각적으로 디자인을 구현 하 여 레이아웃을 사용자 지정에 대 한 기본 모양을 사용 하 여 적용 됩니다. 경우에 따라 표시를 분명히 행 (예: 음악 앱) 및 (예: 연락처 앱에는 대화에서 메시지 앱 편집) 테이블 컨트롤을 인식 하는 것이 어렵습니다 경우도 목록입니다.

Xamarin.Android 사용 하 여 플랫폼 간 응용 프로그램에서 작업에 대해 UITableView 컨트롤은 Android에서 ListView 클래스 (및 UITableViewSource 클래스는 Android의 어댑터 클래스와 유사).

다음이 문서를 비롯 하 여 테이블 작업에 대해 포괄적으로 걸립니다.

-   **테이블 파트** – 소개 및 설명의 시각적 요소를 `UITableView` 제어 합니다. 
-   **테이블에서 데이터 표시** – 다양 한 테이블 및 셀 스타일을 사용 하 여 만들고 테이블을 채우는 방법 데모 및 cell 개체를 재활용 하 여 메모리 문제를 방지 합니다. 
-   **고급 사용법** – 사용자 지정 셀 빌드하고 UITableView 클래스의 편집 기능을 사용 합니다. 
-   **테이블을 시각적으로 만드는** – iOS에 대 한 Xamarin 디자이너를 사용 하 여 Storyboard를 사용 하 여 테이블 기반 인터페이스를 만듭니다. 

## <a name="contents"></a>목차

 [테이블 파트 &amp; 기능](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [데이터로 테이블 채우기](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [테이블 모양 사용자 지정](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [편집](~/ios/user-interface/controls/tables/editing.md)
 
 [행 작업](~/ios/user-interface/controls/tables/row-action.md)

 [스토리 보드에 테이블 만들기](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)
 
 [행 높이 자동 크기 조정](~/ios/user-interface/controls/tables/autosizing-row-height.md)

## <a name="related-links"></a>관련 링크

- [WorkingWithTables (샘플)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables/)
- [테이블에서 스토리 보드 (샘플)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Storyboards 소개](~/ios/user-interface/storyboards/index.md)
- [스토리 보드 TableView 레시피](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/storyboard_a_tableview)
- [MonoTouch.Dialog 소개](~/ios/user-interface/monotouch.dialog/index.md)
- [Github의 TableEditing 샘플](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [Github의 TableParts 샘플](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [Github의 TableAndCellStyles 샘플](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [UITableView 클래스 참조](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [UITableViewCell 클래스 참조](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [UITableViewDelegate](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [UITableViewDataSource](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)
