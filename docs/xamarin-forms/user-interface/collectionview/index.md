---
title: Xamarin.FormsCollectionView
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a2c9fd9e6e48192bc2237d6b451b533fcee6e6ed
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136450"
---
# <a name="xamarinforms-collectionview"></a>Xamarin.FormsCollectionView

## <a name="introduction"></a>[소개](introduction.md)

는 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 다른 레이아웃 사양을 사용 하 여 데이터 목록을 표시 하기 위한 유연 하 고 성능이 뛰어난 뷰입니다.

## <a name="data"></a>[Data](populate-data.md)

는 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 속성을를 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 구현 하는 컬렉션으로 설정 하 여 데이터로 채워집니다 `IEnumerable` . 속성을로 설정 하 여 목록에 있는 각 항목의 모양을 정의할 수 있습니다 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) .

## <a name="layout"></a>[레이아웃](layout.md)

기본적으로는 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 해당 항목을 세로 목록에 표시 합니다. 그러나 세로 및 가로 목록과 그리드를 지정할 수 있습니다.

## <a name="selection"></a>[선택 영역](selection.md)

기본적으로 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 선택은 사용 하지 않도록 설정 되어 있습니다. 그러나 단일 및 다중 선택을 사용할 수 있습니다.

## <a name="empty-views"></a>[빈 뷰](emptyview.md)

에서 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 데이터를 표시할 수 없는 경우 사용자에 게 피드백을 제공 하는 빈 뷰를 지정할 수 있습니다. 빈 뷰는 문자열, 뷰 또는 여러 뷰가 될 수 있습니다.

## <a name="scrolling"></a>[스크롤](scrolling.md)

사용자가 스크롤을 시작 swipes 면 항목이 완전히 표시 되도록 스크롤의 끝 위치를 제어할 수 있습니다. 또한 [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 는 프로그래밍 방식으로 항목을 뷰로 스크롤 하는 두 가지 메서드를 정의 합니다. 오버 로드 중 하나는 지정 된 인덱스에 있는 항목을 뷰로 스크롤하고 다른 하나는 지정 된 항목을 뷰로 스크롤합니다.

## <a name="grouping"></a>[묶어서](grouping.md)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)속성을로 설정 하 여 올바르게 그룹화 된 데이터를 표시할 수 있습니다 `IsGrouped` `true` .
