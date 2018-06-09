---
title: Xamarin.Forms ListView
description: 이 가이드는 아름 다운, 대화형 목록에 데이터를 표시 하는 데 사용할 수 있는 Xamarin.Forms ListView를 소개 합니다.
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2015
ms.openlocfilehash: f73e7b70749a7a6913856d8c753db63a6d0a2bbe
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245002"
---
# <a name="xamarinforms-listview"></a>Xamarin.Forms ListView

ListView 스크롤이 필요한 긴 목록 특히, 데이터 목록을 표시 하기 위한 뷰입니다. 이 가이드에서는 ListView를 사용 하는 방법을 설명 합니다.

1. **[데이터 원본](data-and-databinding.md)**  &ndash; ListView 상관 없이 데이터 바인딩을 데이터로 채웁니다.
2. **[셀 모양](customizing-cell-appearance.md)**  &ndash; 기본 제공 된 셀의 모양을 사용자 지정 하거나 사용자 고유의 사용자 지정 셀을 만듭니다.
3. **[모양 목록](customizing-list-appearance.md)**  &ndash; ListView의 모양을 사용자 지정 합니다. 머리글 및 바닥글을 설정 하 고 그룹 설정을 다음 행의 높이 변경 합니다.
4. **[상호 작용](interactivity.md)**  &ndash; 탭 및 선택 항목을 처리 끌어오기-새로 구현 하 고 상황에 맞는 작업을 추가 합니다.
5. **[성능](performance.md)**  &ndash; 성능 문제를 방지 합니다.

## <a name="use-cases"></a>사용 사례
ListView 요구 사항에 대 한 올바른 제어 되는지 확인 합니다. ListView는 스크롤할 수 있는 데이터 목록을 표시 하는 경우에 사용할 수 있습니다. Listview 상황에 맞는 작업 및 데이터 바인딩을 지원합니다.

ListView와 혼동 해서는 안 [TableView](~/xamarin-forms/user-interface/tableview.md)합니다. TableView 제어는 비 바운드 옵션 또는 데이터 목록이 있는 경우 더 나은 옵션입니다. 예를 들어 대부분 미리 정의 된 옵션 집합이 있는 iOS 설정 앱은 ListView 보다 TableView 사용할 적합 합니다.

ListView 최상의 유형이 같은 데이터에도 알맞습니다 &ndash; 즉, 모든 데이터는 동일한 형식 이어야 합니다. 목록에서 각 행에 대 한 한 가지 유형의 셀을 사용할 수 있으므로입니다. TableViews 뷰를 혼합 해야 할 때 더 나은 옵션은 있도록 여러 셀 유형을 지원할 수 있습니다.


## <a name="components"></a>구성 요소
ListView에 다양 한 구성 요소는 각 플랫폼의 기본 기능을 실행할 수 있습니다. 이러한 각 구성이 요소는 다음과 같습니다.

- **[머리글 및 바닥글](customizing-list-appearance.md#Headers_and_Footers)**  &ndash; 목록의 데이터에서 목록의 시작과 끝에 표시할 텍스트 또는 보기를 분리 합니다. 머리글 및 바닥글 바인딩될 수 없습니다 데이터 소스에 독립적으로 ListView의 데이터 원본에서 합니다.
- **[그룹](customizing-list-appearance.md#Grouping)**  &ndash; 쉽게 탐색할 ListView에서 데이터를 그룹화 할 수 있습니다. 그룹은 일반적으로 바인딩된 데이터:

![](images/grouping-depth.png "그룹화 된 데이터와 ListView")

- **[셀](customizing-cell-appearance.md)**  &ndash; ListView에서 데이터 셀에 표시 됩니다. 각 셀의 데이터 행에 해당 합니다. 선택할 수 있는 기본 제공 된 셀 또는 사용자 고유의 사용자 지정 셀을 정의할 수 있습니다. 기본 및 사용자 지정 셀 XAML 또는 코드에 사용 되 는/정의 될 수 있습니다.
  - **[기본 제공](customizing-cell-appearance.md#Built_in_Cells)**  &ndash; 셀 TextCell과 ImageCell, 특히 기본 제공 될 수 있습니다, 성능에 좋지 있으므로 각 플랫폼에서 네이티브 컨트롤에 해당 합니다.
       - **[TextCell](customizing-cell-appearance.md#TextCell)**  &ndash; 정보 텍스트가 있는 필요에 따라 텍스트 문자열을 표시 합니다. 세부 정보 텍스트가 강조 색으로 더 작은 글꼴의 두 번째 줄으로 렌더링 됩니다.
       - **[ImageCell](customizing-cell-appearance.md#ImageCell)**  &ndash; 텍스트와 이미지를 표시 합니다. 왼쪽에 있는 이미지와 TextCell으로 나타납니다.
  - **[사용자 지정 셀](customizing-cell-appearance.md#customcells)**  &ndash; 사용자 지정 셀은 복잡 한 데이터를 제공 해야 하는 경우에 유용 합니다. 예를 들어 앨범 및 아티스트를 포함 하 여 음악의 목록을 표시할 사용자 지정 보기를 사용할 수 있습니다.

![](images/image-cell-default.png "ImageCells 있는 ListView")

ListView에서 셀 사용자 지정 하는 방법에 대 한 자세한 참조 [ListView 셀 모양 사용자 지정](customizing-cell-appearance.md)합니다.

## <a name="functionality"></a>기능
ListView에서는 다양 한 상호 작용 스타일을 포함 하 여 지원 합니다.

- **[끌어오기-새로 고침](interactivity.md#Pull_to_Refresh)**  &ndash; ListView 각 플랫폼에서 끌어오기-새로 고침을 지원 합니다.
- **[상황에 맞는 작업](interactivity.md#Context_Actions)**  &ndash; ListView 목록의 개별 항목에 기록 작업을 지원 합니다. 예를 들어 구현 살짝-액션 ios, 하거나 장기 눌러서 Android에 대 한 작업입니다.
- **[선택 영역](interactivity.md#selectiontaps)**  &ndash; 선택 내용 및 행의 탭이 수행 되는 경우 작업을 수행할 deselections 수신 대기할 수 있습니다.

![](images/context-default.png "상황에 맞는 작업으로 ListView")

ListView의 대화형 기능에 대 한 자세한 참조 [동작 및 상호 ListView](interactivity.md)합니다.


## <a name="related-links"></a>관련 링크

- [ListView와 작업 (샘플)](https://developer.xamarin.com/samples/WorkingWithListview)
- [양방향 바인딩을 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [셀의 기본 제공 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [사용자 지정 셀 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [그룹화 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [사용자 지정 렌더러 보기 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/WorkingWithListviewNative)
- [ListView 상호 작용 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
- [iOS Workbook](https://developer.xamarin.com/workbooks/xamarin-forms/user-interface/listview/ListView1-ios.workbook)
- [Android Workbook](https://developer.xamarin.com/workbooks/xamarin-forms/user-interface/listview/ListView1-android.workbook)
