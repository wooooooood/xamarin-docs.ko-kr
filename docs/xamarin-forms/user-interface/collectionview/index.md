---
title: Xamarin.ios CollectionView
description: CollectionView는 다양 한 레이아웃 사양을 사용 하 여 데이터 목록을 표시 하기 위한 유연 하 고 성능이 뛰어난 뷰입니다.
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/24/2019
ms.openlocfilehash: b3841ed1287c980212ce37078f38f4984393c414
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69888602"
---
# <a name="xamarinforms-collectionview"></a>Xamarin.ios CollectionView

![](~/media/shared/preview.png "이 API는 현재 시험판임")

## <a name="introductionintroductionmd"></a>[소개](introduction.md)

는 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 다른 레이아웃 사양을 사용 하 여 데이터 목록을 표시 하기 위한 유연 하 고 성능이 뛰어난 뷰입니다.

## <a name="datapopulate-datamd"></a>[데이터](populate-data.md)

는 [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 속성을를 구현 `IEnumerable`하는 컬렉션으로 설정 하 여 데이터로 채워집니다. [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) 속성 [을`DataTemplate`](xref:Xamarin.Forms.DataTemplate)로 설정 하 여 목록에 있는 각 항목의 모양을 정의할 수 있습니다.

## <a name="layoutlayoutmd"></a>[레이아웃](layout.md)

기본적으로는 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 해당 항목을 세로 목록에 표시 합니다. 그러나 세로 및 가로 목록과 그리드를 지정할 수 있습니다.

## <a name="selectionselectionmd"></a>[선택 항목](selection.md)

기본적 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 으로 선택은 사용 하지 않도록 설정 되어 있습니다. 그러나 단일 및 다중 선택을 사용할 수 있습니다.

## <a name="empty-viewsemptyviewmd"></a>[빈 뷰](emptyview.md)

에서 [`CollectionView`](xref:Xamarin.Forms.CollectionView)데이터를 표시할 수 없는 경우 사용자에 게 피드백을 제공 하는 빈 뷰를 지정할 수 있습니다. 빈 뷰는 문자열, 뷰 또는 여러 뷰가 될 수 있습니다.

## <a name="scrollingscrollingmd"></a>[스크롤](scrolling.md)

사용자가 스크롤을 시작 swipes 면 항목이 완전히 표시 되도록 스크롤의 끝 위치를 제어할 수 있습니다. 또한 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 는 프로그래밍 방식으로 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 항목을 뷰로 스크롤 하는 두 가지 메서드를 정의 합니다. 오버 로드 중 하나는 지정 된 인덱스에 있는 항목을 뷰로 스크롤하고 다른 하나는 지정 된 항목을 뷰로 스크롤합니다.

## <a name="groupinggroupingmd"></a>[그룹화](grouping.md)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)`IsGrouped` 속성을로 설정 하 여 올바르게 그룹화 된 데이터 `true`를 표시할 수 있습니다.
