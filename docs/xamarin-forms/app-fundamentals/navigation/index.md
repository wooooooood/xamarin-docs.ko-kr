---
title: Xamarin.Forms 탐색
description: 이 가이드에서는 Xamarin.Forms 앱에서 탐색을 수행 하는 방법을 설명 합니다. Xamarin.Forms는 다양 한 사용 중인 페이지 형식에 따라 다른 페이지 탐색 환경 제공 합니다.
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 202f044ebd7dd5b110b94d2aa60eeb7151150607
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994730"
---
# <a name="xamarinforms-navigation"></a>Xamarin.Forms 탐색

_Xamarin.Forms는 다양 한 사용 중인 페이지 형식에 따라 다른 페이지 탐색 환경 제공 합니다._

![](images/page-types.png "Xamarin.Forms 페이지 유형")

## <a name="hierarchical-navigationhierarchicalmd"></a>[계층적 탐색](hierarchical.md)

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 클래스는 사용자가 필요에 따라 페이지를 앞으로 뒤로 탐색할 수 있는 계층적 탐색 환경을 제공합니다. 이 모델은 탐색을 [`Page`](xref:Xamarin.Forms.Page) 개체의 LIFO(후입선출) 스택으로 구현합니다.

## <a name="tabbedpagetabbed-pagemd"></a>[TabbedPage](tabbed-page.md)

Xamarin.Forms [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) 세부 정보 영역에 콘텐츠를 로드 하는 각 탭을 사용 하 여 탭 및 더 큰 세부 정보 영역을 목록으로 구성 합니다.

## <a name="carouselpagecarousel-pagemd"></a>[CarouselPage](carousel-page.md)

Xamarin.Forms [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) 갤러리와 같은 콘텐츠 페이지를 탐색 페이지 사용자 측면에서 살짝 밀 수입니다.

## <a name="masterdetailpagemaster-detail-pagemd"></a>[MasterDetailPage](master-detail-page.md)

Xamarin.Forms [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) 항목을 표시 하는 마스터 페이지 및 마스터 페이지의 항목에 대 한 세부 정보를 제공 하는 세부 정보 페이지 두 페이지의 관련된 정보를 관리 하는 페이지입니다.

## <a name="modal-pagesmodalmd"></a>[모달 페이지](modal.md)

Xamarin.Forms는 모달 페이지에 대 한 지원도 제공 합니다. 모달 페이지는 사용자가 작업이 완료되거나 취소될 때까지 다른 부분으로 이동할 수 없는 자체 포함된 작업을 완료하도록 권장합니다.

## <a name="displaying-pop-upspop-upsmd"></a>[팝업 표시](pop-ups.md)

두 팝업 등록와 같은 사용자 인터페이스 요소를 제공 하는 Xamarin.Forms: 경고 및 작업 시트를 합니다. 사용자에 게 간단한 질문을 요청 하는 데 사용자가 작업을 통해 이러한 인터페이스 요소를 사용할 수 있습니다.
