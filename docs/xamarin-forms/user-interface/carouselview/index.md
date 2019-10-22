---
title: Xamarin.ios CarouselView
description: CarouselView는 스크롤 가능한 레이아웃으로 데이터를 표시 하기 위한 뷰입니다. 사용자는 항목 컬렉션을 통해 이동할 수 있습니다.
ms.prod: xamarin
ms.assetid: 5b673347-cdba-4532-820f-fb5f070c86bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/08/2019
ms.openlocfilehash: 816c1b6e4ab497d0ada0f80fa3ad4800912587c3
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696983"
---
# <a name="xamarinforms-carouselview"></a>Xamarin.ios CarouselView

![](~/media/shared/preview.png "This API is currently pre-release")

## <a name="introductionintroductionmd"></a>[소개](introduction.md)

[@No__t_1](xref:Xamarin.Forms.CarouselView) 은 스크롤할 수 있는 레이아웃으로 데이터를 표시 하기 위한 뷰입니다. 사용자는 항목 컬렉션을 통해 이동할 수 있습니다.

## <a name="datapopulate-datamd"></a>[Data](populate-data.md)

[@No__t_1](xref:Xamarin.Forms.CarouselView) [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 속성을 `IEnumerable`를 구현 하는 컬렉션으로 설정 하 여 데이터로 채워집니다. [@No__t_1](xref:Xamarin.Forms.ItemsView.ItemTemplate) 속성을 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)설정 하 여 각 항목의 모양을 정의할 수 있습니다.

## <a name="layoutlayoutmd"></a>[레이아웃](layout.md)

기본적으로 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 는 해당 항목을 가로 목록으로 표시 합니다. 그러나 세로 방향을 포함 하 여 CollectionView와 동일한 레이아웃에도 액세스할 수 있습니다.

## <a name="interactioninteractionmd"></a>[작용과](interaction.md)

[@No__t_1](xref:Xamarin.Forms.CarouselView) 에서 현재 표시 되는 항목은 `CurrentItem` 및 `Position` 속성을 통해 액세스할 수 있습니다.

## <a name="empty-viewsemptyviewmd"></a>[빈 뷰](emptyview.md)

[@No__t_1](xref:Xamarin.Forms.CarouselView)에서 표시할 수 있는 데이터가 없는 경우 사용자에 게 피드백을 제공 하는 빈 뷰를 지정할 수 있습니다. 빈 뷰는 문자열, 뷰 또는 여러 뷰가 될 수 있습니다.

## <a name="scrollingscrollingmd"></a>[스크롤](scrolling.md)

사용자가 스크롤을 시작 swipes 면 항목이 완전히 표시 되도록 스크롤의 끝 위치를 제어할 수 있습니다. 또한 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 는 프로그래밍 방식으로 항목을 뷰로 스크롤 하는 두 개의 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 메서드를 정의 합니다. 오버 로드 중 하나는 지정 된 인덱스에 있는 항목을 뷰로 스크롤하고 다른 하나는 지정 된 항목을 뷰로 스크롤합니다.
