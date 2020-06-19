---
title: Xamarin.Forms뷰
description: 이 가이드에서는 Xamarin.Forms 대화형 목록에 데이터를 표시 하는 데 사용할 수 있는 ListView를 소개 합니다.
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/04/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a1ff8dd5c8a8a4051cea8ce4b288c42bdbaa8d31
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139908"
---
# <a name="xamarinforms-listview"></a>Xamarin.Forms뷰

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview)

[`ListView`](xref:Xamarin.Forms.ListView)는 스크롤을 필요로 하는 데이터 목록, 특히 긴 목록을 제공 하는 뷰입니다.

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)는 다른 레이아웃 사양을 사용하여 데이터 목록을 표시하는 뷰입니다. 보다 유연 하 고 성능이 뛰어난 대안을 제공 하는 것을 목표로 [`ListView`](xref:Xamarin.Forms.ListView) 합니다. 자세한 내용은 [Xamarin.Forms CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)를 참조하세요.

## <a name="use-cases"></a>사용 사례

`ListView`스크롤 가능한 데이터 목록을 표시 하는 모든 상황에서 컨트롤을 사용할 수 있습니다. `ListView`클래스는 컨텍스트 작업 및 데이터 바인딩을 지원 합니다.

컨트롤을 컨트롤과 `ListView` 혼동 해서는 안 됩니다 [`TableView`](~/xamarin-forms/user-interface/tableview.md) . `TableView`옵션 또는 데이터의 바인딩되지 않은 목록이 XAML에서 미리 정의 된 옵션을 지정 하도록 허용 하기 때문에이 컨트롤은 더 나은 옵션입니다. 예를 들어 미리 정의 된 옵션 집합이 있는 iOS 설정 앱은 보다를 사용 하는 데 더 적합 합니다 `TableView` `ListView` .

`ListView`클래스는 XAML에서 목록 항목을 정의 하는 기능을 지원 하지 않습니다 `ItemsSource` `ItemTemplate` . 목록에서 항목을 정의 하려면에 속성 또는 데이터 바인딩을 사용 해야 합니다.

는 `ListView` 단일 데이터 형식으로 구성 된 컬렉션에 가장 적합 합니다. 이 요구 사항은 목록의 각 행에 대해 하나의 셀 유형만 사용할 수 있기 때문입니다. `TableView`컨트롤은 여러 셀 형식을 지원할 수 있으므로 여러 데이터 형식을 표시 해야 하는 경우 더 나은 옵션입니다.

인스턴스에 데이터를 바인딩하는 방법에 대 한 자세한 내용은 `ListView` [ListView 데이터 원본](~/xamarin-forms/user-interface/listview/data-and-databinding.md)을 참조 하세요.

## <a name="components"></a>구성 요소

컨트롤에는 `ListView` 각 플랫폼의 기본 기능을 실행 하는 데 사용할 수 있는 여러 가지 구성 요소가 있습니다. 이러한 구성 요소는 다음 섹션에 정의 되어 있습니다.

### <a name="headers-and-footers"></a>[머리글 및 바닥글](customizing-list-appearance.md#headers-and-footers)

머리글 및 바닥글 구성 요소는 목록의 데이터와 구분 되는 목록의 시작과 끝에 표시 됩니다. 머리글 및 바닥글은 ListView의 데이터 원본에서 별도의 데이터 원본에 바인딩할 수 있습니다.

### <a name="groups"></a>[그룹](customizing-list-appearance.md#grouping)

쉽게 탐색할 수 있도록의 데이터를 `ListView` 그룹화 할 수 있습니다. 그룹은 일반적으로 데이터 바인딩됩니다. 다음 스크린샷은 그룹화 된 데이터가 있는을 보여 줍니다 `ListView` .

[!["ListView의 그룹화 된 데이터"](images/grouping-depth-cropped.png)](images/grouping-depth.png#lightbox "ListView의 그룹화 된 데이터")

### <a name="cells"></a>[셀](customizing-cell-appearance.md)

의 데이터 항목을 `ListView` 셀 이라고 합니다. 각 셀은 데이터 행에 해당 합니다. 선택할 수 있는 기본 제공 셀이 있거나 사용자 지정 셀을 정의할 수 있습니다. 기본 제공 및 사용자 지정 셀 모두 XAML 또는 코드에서 사용/정의할 수 있습니다.

- [기본 제공 셀](customizing-cell-appearance.md#built-in-cells)(예: 및) `TextCell` 은 `ImageCell` 네이티브 컨트롤에 해당 하며 특히 유용 합니다.
  - 는 [`TextCell`](customizing-cell-appearance.md#textcell) 텍스트 문자열을 표시 하 고 선택적으로 정보 텍스트를 표시 합니다. 세부 정보 텍스트는 강조 색을 사용 하 여 더 작은 글꼴의 둘째 선으로 렌더링 됩니다.
  - 는 [`ImageCell`](customizing-cell-appearance.md#imagecell) 텍스트가 있는 이미지를 표시 합니다. `TextCell`왼쪽에 이미지를 포함 하는로 표시 됩니다.
- [사용자 지정 셀](customizing-cell-appearance.md#custom-cells) 은 복잡 한 데이터를 표시 하는 데 사용 됩니다. 예를 들어 사용자 지정 셀을 사용 하 여 앨범 및 음악가를 포함 하는 노래 목록을 표시할 수 있습니다.

다음 스크린샷은 ImageCell 항목을 보여 줍니다 `ListView` .

[!["ListView에서 항목 ImageCell"](images/image-cell-default-cropped.png)](images/image-cell-default.png#lightbox "ListView에서 항목 ImageCell")

에서 셀 사용자 지정에 대해 자세히 알아보려면 `ListView` [ListView 셀 모양 사용자 지정](customizing-cell-appearance.md)을 참조 하세요.

## <a name="functionality"></a>기능

`ListView`클래스는 다양 한 상호 작용 스타일을 지원 합니다.

- [끌어오기-새로 고침](interactivity.md#pull-to-refresh) 을 사용 하 여 `ListView` 콘텐츠를 새로 고칠 수 있습니다.
- 개발자는 [컨텍스트 작업](interactivity.md#context-actions) 을 통해 개별 목록 항목에 대해 사용자 지정 작업을 지정할 수 있습니다. 예를 들어 iOS에서의 작업을 수행 하거나 Android에서 긴 탭 작업을 구현할 수 있습니다.
- [선택](interactivity.md#selection-and-taps) 기능을 통해 개발자는 목록 항목의 선택 및 deselection 이벤트에 기능을 연결할 수 있습니다.

다음 스크린샷은 컨텍스트 작업을 포함 하는을 보여 줍니다 `ListView` .

[!["ListView의 컨텍스트 작업"](images/context-default-cropped.png)](images/context-default.png#lightbox "ListView의 컨텍스트 작업")

의 대화형 기능에 대 한 자세한 `ListView` 내용은 ListView를 [사용 하 여 작업 & 대화형 작업](interactivity.md)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [ListView 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview)
- [양방향 바인딩 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
- [기본 제공 셀 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [사용자 지정 셀 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [그룹화 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)
- [사용자 지정 렌더러 뷰 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative/)
- [ListView 대화형 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)
