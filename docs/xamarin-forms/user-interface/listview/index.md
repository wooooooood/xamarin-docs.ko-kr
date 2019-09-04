---
title: Xamarin.Forms ListView
description: 이 가이드에서는 아름 답 고 대화형 목록의 데이터를 제공 하기 위해 사용할 수 있는 Xamarin.Forms ListView를 소개 합니다.
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2015
ms.openlocfilehash: f05703babd3f6e67713dfccdb1a1fc6a4ea6966e
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70228028"
---
# <a name="xamarinforms-listview"></a>Xamarin.Forms ListView

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview)

[`ListView`](xref:Xamarin.Forms.ListView)는 스크롤을 필요로 하는 데이터 목록, 특히 긴 목록을 제공 하는 뷰입니다.

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)는 다른 레이아웃 사양을 사용하여 데이터 목록을 표시하는 뷰입니다. 보다 유연 하 고 성능이 뛰어난 대안 [`ListView`](xref:Xamarin.Forms.ListView)을 제공 하는 것을 목표로 합니다. 자세한 내용은 [Xamarin.Forms CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)를 참조하세요.

## <a name="use-cases"></a>사용 사례

ListView에 맞게 오른쪽 컨트롤 인지 확인 합니다. ListView는 데이터의 스크롤 목록을 표시 하는 상황에서 사용할 수 있습니다. Listview는 상황에 맞는 작업 및 데이터 바인딩을 지원합니다.

ListView와 혼동 하지 마십시오 [TableView](~/xamarin-forms/user-interface/tableview.md)합니다. TableView 컨트롤이 바운드가 아닌 옵션 또는 데이터 목록이 있을 때마다 더 나은 옵션입니다. 예를 들어, 대부분 미리 정의 된 일련의 옵션에는 iOS 설정 앱은 TableView ListView 보다는 데 적합 합니다.

유형이 같은 데이터를 ListView는 가장 적합 한도 &ndash; 즉, 모든 데이터가 동일한 형식 이어야 합니다. 즉, 목록의 각 행에 하나의 유형의 셀을 사용할 수 있습니다. 뷰를 조합 해야 할 때 더 나은 옵션은 있도록 TableViews 여러 셀 유형의 지원할 수 있습니다.

## <a name="components"></a>구성 요소
ListView에는 여러 구성 요소가 각 플랫폼의 기본 기능을 실행 하도록 사용할 수 있습니다. 이러한 각 구성이 요소는 아래 설명 되어 있습니다.

- **[머리글 및 바닥글](customizing-list-appearance.md#Headers_and_Footers)**  &ndash; 목록의 시작과 끝에 표시할 텍스트 또는 뷰 목록의 데이터에서 분리 합니다. 머리글 및 바닥글 바인딩될 수 없습니다 데이터 소스에 독립적으로 ListView의 데이터 원본에서.
- **[그룹](customizing-list-appearance.md#Grouping)**  &ndash; 쉽게 탐색할는 ListView의 데이터를 그룹화 할 수 있습니다. 그룹은 일반적으로 바인딩된 데이터:

![](images/grouping-depth.png "그룹화 된 데이터를 사용 하 여 ListView")

- **[셀](customizing-cell-appearance.md)**  &ndash; 을 ListView에서 데이터 셀에 표시 됩니다. 각 셀은 데이터 행에 해당 합니다. 셀이 기본 제공 또는 사용자 고유의 사용자 지정 셀을 정의할 수 있습니다. 기본 제공 및 사용자 지정 셀 XAML 또는 코드에서 사용 되 는/정의 변수일 수 있습니다.
  - **[기본 제공](customizing-cell-appearance.md#Built_in_Cells)**  &ndash; 셀 TextCell과 ImageCell, 특히 기본 제공 성능에 대 한 훌륭한 있으므로 수는 각 플랫폼에서 네이티브 컨트롤에 해당 합니다.
    - **[TextCell](customizing-cell-appearance.md#TextCell)**  &ndash; 정보 텍스트를 사용 하 여 필요에 따라 텍스트 문자열을 표시 합니다. 세부 정보 텍스트를 강조 색을 사용 하 여 필요한 경우 작은 글꼴로 두 번째 줄으로 렌더링 됩니다.
    - **[ImageCell](customizing-cell-appearance.md#ImageCell)**  &ndash; 텍스트를 사용 하 여 이미지를 표시 합니다. 왼쪽에 있는 이미지를 사용 하 여 TextCell으로 표시 됩니다.
  - **[사용자 지정 셀](customizing-cell-appearance.md#customcells)**  &ndash; 복잡 한 데이터를 제공 해야 하는 경우 사용자 지정 셀 유용 합니다. 예를 들어, 노래, 앨범과 아티스트를 포함 하 여 목록을 표시 하는 사용자 지정 보기를 사용할 수 있습니다.

![](images/image-cell-default.png "ImageCells 사용 하 여 ListView")

ListView 셀 사용자 지정 하는 방법에 대 한 자세한 내용은 참조 하세요 [ListView 셀 모양 사용자 지정](customizing-cell-appearance.md)합니다.

## <a name="functionality"></a>기능
ListView는 다양을 한 상호 작용 스타일을 포함 하 여 지원 합니다.

- **[끌어오기-새로 고침](interactivity.md#Pull_to_Refresh)**  &ndash; ListView는 각 플랫폼에서 끌어오기-새로 고침을 지원 합니다.
- **[상황에 맞는 작업](interactivity.md#Context_Actions)**  &ndash; ListView 목록의 개별 항목에 관련 작업을 지원 합니다. 예를 들어, iOS에서 안쪽으로 살짝 밀어-작업을 구현 하거나 길게 누른 Android에 대 한 작업입니다.
- **[선택 영역](interactivity.md#selectiontaps)**  &ndash; 선택 및 행을 탭 할 경우 작업을 수행할 deselections 수신할 수 있습니다.

![](images/context-default.png "상황에 맞는 작업을 사용 하 여 ListView")

ListView의 대화형 기능에 대 한 자세한 내용은 참조 하세요 [ListView 사용 하 여 대화형 작업 및 작업](interactivity.md)합니다.

## <a name="related-links"></a>관련 링크

- [ListView와 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview)
- [양방향 바인딩을 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
- [셀의 기본 제공 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [사용자 지정 셀 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [그룹화 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)
- [사용자 지정 렌더러 보기 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative/)
- [ListView 대화형 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)
