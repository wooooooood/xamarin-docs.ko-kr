---
title: Xamarin.ios CarouselView
description: CarouselView는 회전식과 비슷한 레이아웃의 데이터 목록을 제공 하는 보기입니다.
ms.prod: xamarin
ms.assetid: 5b673347-cdba-4532-820f-fb5f070c86bc
ms.technology: xamarin-forms
author: pauldipietro
ms.author: padipi
ms.date: 09/09/2019
ms.openlocfilehash: 06d20341053c4f77cb72aac9e1abdc85d9f95fdb
ms.sourcegitcommit: e83035c746f165ee6d03f2e9fd0066ee4f20a9fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70908333"
---
# <a name="xamarinforms-carouselview"></a>Xamarin.ios CarouselView

![](~/media/shared/preview.png "이 API는 현재 시험판임")

> [!IMPORTANT]
> CarouselView 설명서는 개발 중 이며 일부 링크는 CollectionView 설명서를 참조할 수 있습니다. 대부분의 경우에는 CollectionView를 기반으로 하는 CarouselView의 특성으로 인해 정보를 적용할 수 있어야 합니다.

## <a name="introductionintroductionmd"></a>[소개](introduction.md)

는 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 회전식과 비슷한 레이아웃으로 데이터를 표시 하기 위한 뷰입니다. 해당 구현은의 [`CollectionView`](xref:Xamarin.Forms.CollectionView)해당 구현을 기반으로 합니다.

## <a name="datacollectionviewpopulate-datamd"></a>[데이터](../collectionview/populate-data.md)

는 [`CarouselView`](xref:Xamarin.Forms.CarouselView) [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 속성을를 구현 `IEnumerable`하는 컬렉션으로 설정 하 여 데이터로 채워집니다. [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) 속성 [을`DataTemplate`](xref:Xamarin.Forms.DataTemplate)로 설정 하 여 목록에 있는 각 항목의 모양을 정의할 수 있습니다.

## <a name="layoutlayoutmd"></a>[레이아웃](layout.md)

기본적으로에는 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 해당 항목이 가로 목록으로 표시 됩니다. 그러나 세로 방향을 포함 하 여 CollectionView와 동일한 레이아웃에도 액세스할 수 있습니다.

## <a name="empty-viewscollectionviewemptyviewmd"></a>[빈 뷰](../collectionview/emptyview.md)

에서 [`CarouselView`](xref:Xamarin.Forms.CarouselView)데이터를 표시할 수 없는 경우 사용자에 게 피드백을 제공 하는 빈 뷰를 지정할 수 있습니다. 빈 뷰는 문자열, 뷰 또는 여러 뷰가 될 수 있습니다.

## <a name="scrollingcollectionviewscrollingmd"></a>[스크롤](../collectionview/scrolling.md)

사용자가 스크롤을 시작 swipes 면 항목이 완전히 표시 되도록 스크롤의 끝 위치를 제어할 수 있습니다. 또한 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 는 프로그래밍 방식으로 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 항목을 뷰로 스크롤 하는 두 가지 메서드를 정의 합니다. 오버 로드 중 하나는 지정 된 인덱스에 있는 항목을 뷰로 스크롤하고 다른 하나는 지정 된 항목을 뷰로 스크롤합니다.
