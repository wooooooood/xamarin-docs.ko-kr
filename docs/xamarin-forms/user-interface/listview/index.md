---
title: Xamarin.Forms ListView
description: 이 가이드에서는 대화형 목록에 데이터를 표시 하는 데 사용할 수 있는 Xamarin.ios ListView를 소개 합니다.
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/04/2019
ms.openlocfilehash: 5d09d76a44a6322285a143230173d244848ba4a6
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770208"
---
# <a name="xamarinforms-listview"></a>Xamarin.Forms ListView

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview)

[`ListView`](xref:Xamarin.Forms.ListView)는 스크롤을 필요로 하는 데이터 목록, 특히 긴 목록을 제공 하는 뷰입니다.

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)는 다른 레이아웃 사양을 사용하여 데이터 목록을 표시하는 뷰입니다. 보다 유연 하 고 성능이 뛰어난 대안 [`ListView`](xref:Xamarin.Forms.ListView)을 제공 하는 것을 목표로 합니다. 자세한 내용은 [Xamarin.Forms CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)를 참조하세요.

## <a name="use-cases"></a>사용 사례

스크롤 `ListView` 가능한 데이터 목록을 표시 하는 모든 상황에서 컨트롤을 사용할 수 있습니다. 클래스 `ListView` 는 컨텍스트 작업 및 데이터 바인딩을 지원 합니다.

컨트롤을 컨트롤과 혼동 [`TableView`](~/xamarin-forms/user-interface/tableview.md) 해서는 안 됩니다. `ListView` 옵션 또는 데이터의 바인딩되지 않은 목록이 XAML에서 미리 정의 된 옵션을 지정 하도록 허용 하기 때문에이 컨트롤은더나은옵션입니다.`TableView` 예를 들어 미리 정의 된 옵션 집합이 있는 iOS 설정 앱은 `TableView` `ListView`보다를 사용 하는 데 더 적합 합니다.

클래스 `ListView` 는 XAML에서 목록 항목을 정의 `ItemTemplate` 하는 기능을 지원 하지 `ItemsSource` 않습니다. 목록에서 항목을 정의 하려면에 속성 또는 데이터 바인딩을 사용 해야 합니다.

는 `ListView` 단일 데이터 형식으로 구성 된 컬렉션에 가장 적합 합니다. 이 요구 사항은 목록의 각 행에 대해 하나의 셀 유형만 사용할 수 있기 때문입니다. 컨트롤 `TableView` 은 여러 셀 형식을 지원할 수 있으므로 여러 데이터 형식을 표시 해야 하는 경우 더 나은 옵션입니다.

인스턴스에 데이터를 바인딩하는 `ListView` 방법에 대 한 자세한 내용은 [ListView 데이터 원본](~/xamarin-forms/user-interface/listview/data-and-databinding.md)을 참조 하세요.

## <a name="components"></a>구성 요소
`ListView` 컨트롤에는 각 플랫폼의 기본 기능을 실행 하는 데 사용할 수 있는 여러 가지 구성 요소가 있습니다. 이러한 구성 요소는 다음 섹션에 정의 되어 있습니다.

### <a name="headers-and-footerscustomizing-list-appearancemdheaders-and-footers"></a>[머리글 및 바닥글](customizing-list-appearance.md#headers-and-footers)

머리글 및 바닥글 구성 요소는 목록의 데이터와 구분 되는 목록의 시작과 끝에 표시 됩니다. 머리글 및 바닥글은 ListView의 데이터 원본에서 별도의 데이터 원본에 바인딩할 수 있습니다.

### <a name="groupscustomizing-list-appearancemdgrouping"></a>[그룹과](customizing-list-appearance.md#grouping)

쉽게 탐색할 수 `ListView` 있도록의 데이터를 그룹화 할 수 있습니다. 그룹은 일반적으로 데이터 바인딩됩니다. 다음 스크린샷은 그룹화 된 데이터가 `ListView` 있는을 보여 줍니다.

(images/grouping-depth-cropped.png)](images/grouping-depth.png#lightbox "Listview에서") ["listview의 그룹화 된 데이터" 그룹화 된 데이터 ![]

### <a name="cellscustomizing-cell-appearancemd"></a>[셀](customizing-cell-appearance.md)

의 `ListView` 데이터 항목을 셀 이라고 합니다. 각 셀은 데이터 행에 해당 합니다. 셀이 기본 제공 또는 사용자 고유의 사용자 지정 셀을 정의할 수 있습니다. 기본 제공 및 사용자 지정 셀 XAML 또는 코드에서 사용 되 는/정의 변수일 수 있습니다.

- [기본 제공 셀](customizing-cell-appearance.md#built-in-cells)(예: `TextCell` 및 `ImageCell`)은 네이티브 컨트롤에 해당 하며 특히 유용 합니다.
  - 는 [`TextCell`](customizing-cell-appearance.md#textcell) 텍스트 문자열을 표시 하 고 선택적으로 정보 텍스트를 표시 합니다. 세부 정보 텍스트를 강조 색을 사용 하 여 필요한 경우 작은 글꼴로 두 번째 줄으로 렌더링 됩니다.
  - 는 [`ImageCell`](customizing-cell-appearance.md#imagecell) 텍스트가 있는 이미지를 표시 합니다. 왼쪽에 이미지 `TextCell` 를 포함 하는로 표시 됩니다.
- [사용자 지정 셀](customizing-cell-appearance.md#customcells) 은 복잡 한 데이터를 표시 하는 데 사용 됩니다. 예를 들어 사용자 지정 셀을 사용 하 여 앨범 및 음악가를 포함 하는 노래 목록을 표시할 수 있습니다.

다음 스크린샷은 ImageCell 항목을 `ListView` 보여 줍니다.

(images/image-cell-default-cropped.png)](images/image-cell-default.png#lightbox "Listview의") ["listview 항목 ImageCell" ImageCell 항목 ![]

에서 `ListView`셀 사용자 지정에 대해 자세히 알아보려면 [ListView 셀 모양 사용자 지정](customizing-cell-appearance.md)을 참조 하세요.

## <a name="functionality"></a>기능
클래스 `ListView` 는 다양 한 상호 작용 스타일을 지원 합니다.

- [끌어오기-새로 고침](interactivity.md#pull-to-refresh) 을 사용 하 여 콘텐츠를 새로 `ListView` 고칠 수 있습니다.
- 개발자는 [컨텍스트 작업](interactivity.md#context-actions) 을 통해 개별 목록 항목에 대해 사용자 지정 작업을 지정할 수 있습니다. 예를 들어, iOS에서 안쪽으로 살짝 밀어-작업을 구현 하거나 길게 누른 Android에 대 한 작업입니다.
- [선택](interactivity.md#selectiontaps) 기능을 통해 개발자는 목록 항목의 선택 및 deselection 이벤트에 기능을 연결할 수 있습니다.

다음 스크린샷은 컨텍스트 작업을 `ListView` 포함 하는을 보여 줍니다.

(images/context-default-cropped.png)](images/context-default.png#lightbox "Listview의") ["listview의 컨텍스트 작업" 컨텍스트 작업 ![]

의 `ListView`대화형 기능에 대 한 자세한 내용은 ListView를 [사용 하 여 작업 & 대화형 작업](interactivity.md)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [ListView와 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview)
- [양방향 바인딩을 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
- [셀의 기본 제공 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [사용자 지정 셀 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [그룹화 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)
- [사용자 지정 렌더러 보기 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative/)
- [ListView 대화형 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)
