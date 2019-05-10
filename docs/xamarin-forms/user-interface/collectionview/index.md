---
title: Xamarin.Forms CollectionView
description: CollectionView은 다른 레이아웃 사양을 사용 하 여 데이터의 목록을 제공 하기 위한 유연 하 고 성능이 뛰어난 뷰입니다.
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: a6cb6e695a4c19627b183060a7636320f8083ee2
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65048144"
---
# <a name="xamarinforms-collectionview"></a>Xamarin.Forms CollectionView

![](~/media/shared/preview.png "이 API는 현재 시험판")

## <a name="introductionintroductionmd"></a>[소개](introduction.md)

`CollectionView` 레이아웃이 사양을 사용 하 여 데이터의 목록을 제공 하기 위한 유연 하 고 성능이 뛰어난 뷰입니다.

## <a name="datapopulate-datamd"></a>[Data](populate-data.md)

A `CollectionView` 설정 하 여 데이터를 채운 해당 `ItemsSource` 속성을 구현 하는 컬렉션 `IEnumerable`합니다. 목록의 각 항목의 모양을 설정 하 여 정의할 수 있습니다 합니다 `ItemTemplate` 속성을 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)합니다.

## <a name="layoutlayoutmd"></a>[레이아웃](layout.md)

기본적으로 `CollectionView` 세로 목록에 항목을 표시 합니다. 그러나 가로 및 세로 목록 및 표를 지정할 수 있습니다.

## <a name="selectionselectionmd"></a>[선택 항목](selection.md)

기본적으로 `CollectionView` 선택할 수 없게 됩니다. 그러나 단일 및 다중 선택을 사용할 수 있습니다.

## <a name="empty-viewsemptyviewmd"></a>[빈 뷰](emptyview.md)

`CollectionView`를 표시할 수 있는 데이터가 없는 경우 사용자에 게 피드백을 제공 하는 빈 뷰를 지정할 수 있습니다. 빈 뷰는 문자열로, 뷰 또는 여러 뷰 수 있습니다.

## <a name="scrollingscrollingmd"></a>[스크롤](scrolling.md)

스크롤을 시작 하는 사용자 천공 기와 때 항목이 완전히 표시 되도록 스크롤의 끝 위치를 제어할 수 있습니다. 또한 `CollectionView` 두 개의 정의 `ScrollTo` 메서드를 프로그래밍 방식으로 스크롤하여 항목입니다. 오버 로드 중 하나는 동안 지정된 된 항목을 뷰로 스크롤합니다 다른 보기에 지정된 된 인덱스에 항목을 스크롤합니다.
