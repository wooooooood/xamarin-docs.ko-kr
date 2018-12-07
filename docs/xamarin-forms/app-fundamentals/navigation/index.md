---
title: Xamarin.Forms 탐색
description: 이 가이드에서는 Xamarin.Forms 앱에서 탐색을 수행하는 방법을 설명합니다. Xamarin.Forms는 사용되는 페이지 유형에 따라 다양한 페이지 탐색 환경을 제공합니다.
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 202f044ebd7dd5b110b94d2aa60eeb7151150607
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994730"
---
# <a name="xamarinforms-navigation"></a>Xamarin.Forms 탐색

_Xamarin.Forms는 사용되는 페이지 유형에 따라 다양한 페이지 탐색 환경을 제공합니다._

![](images/page-types.png "Xamarin.Forms 페이지 유형")

## <a name="hierarchical-navigationhierarchicalmd"></a>[계층적 탐색](hierarchical.md)

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 클래스는 사용자가 필요에 따라 페이지를 앞으로 뒤로 탐색할 수 있는 계층적 탐색 환경을 제공합니다. 이 모델은 탐색을 [`Page`](xref:Xamarin.Forms.Page) 개체의 LIFO(후입선출) 스택으로 구현합니다.

## <a name="tabbedpagetabbed-pagemd"></a>[TabbedPage](tabbed-page.md)

Xamarin.Forms [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)는 탭 목록과 더 큰 세부 정보 영역으로 구성되며 각 탭은 세부 정보 영역으로 콘텐츠를 로드합니다.

## <a name="carouselpagecarousel-pagemd"></a>[CarouselPage](carousel-page.md)

Xamarin.Forms [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)는 사용자가 옆으로 살짝 밀어서 갤러리와 같은 콘텐츠 페이지를 탐색할 수 있는 페이지입니다.

## <a name="masterdetailpagemaster-detail-pagemd"></a>[MasterDetailPage](master-detail-page.md)

Xamarin.Forms [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)는 두 페이지의 관련된 정보를 관리하는 페이지입니다. 두 페이지는 항목을 나타내는 마스터 페이지와 마스터 페이지에 있는 항목에 대한 세부 정보를 나타내는 세부 정보 페이지입니다.

## <a name="modal-pagesmodalmd"></a>[모달 페이지](modal.md)

Xamarin.Forms는 모달 페이지도 지원합니다. 모달 페이지는 사용자가 작업이 완료되거나 취소될 때까지 다른 부분으로 이동할 수 없는 자체 포함된 작업을 완료하도록 권장합니다.

## <a name="displaying-pop-upspop-upsmd"></a>[팝업 표시](pop-ups.md)

Xamarin.Forms는 팝업과 같은 두 가지 사용자 인터페이스 요소인 경고와 작업 시트를 제공합니다. 이러한 인터페이스 요소는 사용자에게 간단한 질문을 하고 작업을 안내하는 데 사용할 수 있습니다.
