---
title: Xamarin.ios Controls 클래스 계층 구조
description: 개발자는 Xamarin.ios 응용 프로그램의 사용자 인터페이스를 만드는 데 사용 되는 형식의 계층 구조에 대해 잘 알고 있어야 합니다.
ms.prod: xamarin
ms.assetid: C89E6B98-464D-4BBE-BF11-13A5FCBBF420
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2019
ms.openlocfilehash: f08146d4439ff1fc22edea71ab1cbb337f64c037
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "68984395"
---
# <a name="xamarinforms-controls-class-hierarchy"></a>Xamarin.ios Controls 클래스 계층 구조

Xamarin.ios는 여러 네임 스페이스에 대해 수백 개의 형식으로 구성 됩니다. 개발자는 `Xamarin.Forms` 네임 스페이스에 상주 하는 Xamarin.ios 응용 프로그램의 사용자 인터페이스를 만드는 데 사용 되는 형식의 계층 구조에 대해 가장 잘 알고 있어야 합니다.

이러한 형식은 페이지, 레이아웃, 보기 및 셀로 나눌 수 있습니다. Xamarin Forms 페이지는 일반적으로 전체 화면을 차지 하며 모든 페이지 형식은 [`Page`](xref:Xamarin.Forms.Page) 클래스에서 파생 됩니다. 페이지는 일반적으로 레이아웃을 포함 하 고 모든 레이아웃 형식은 [`Layout`](xref:Xamarin.Forms.Layout) 클래스에서 파생 됩니다. 레이아웃에는 일반적으로 뷰 및 다른 레이아웃이 포함 되어 있으며 모든 뷰 형식이 [`View`](xref:Xamarin.Forms.View) 클래스에서 파생 됩니다. 마지막으로 셀은 [`TableView`](xref:Xamarin.Forms.TableView) 및 [`ListView`](xref:Xamarin.Forms.ListView) 컨트롤의 표시 데이터에 사용 되는 특수 컨트롤입니다. 페이지, 레이아웃, 보기 및 셀은 모두 [`Element`](xref:Xamarin.Forms.Element) 클래스에서 궁극적으로 파생 됩니다.

다음 클래스 다이어그램은 일반적으로 Xamarin.ios에서 사용자 인터페이스를 작성 하는 데 사용 되는 형식의 계층 구조를 보여 줍니다.

[![Xamarin.ios Controls 클래스 다이어그램](class-hierarchy-images/class-diagram.png "Xamarin.ios controls 클래스 다이어그램")](class-hierarchy-images/class-diagram-large.png#lightbox "Xamarin.ios controls 클래스 다이어그램")

> [!NOTE]
> 클래스 다이어그램의 고해상도 버전은 [여기](class-hierarchy-images/class-diagram-high-resolution.png)에서 다운로드할 수 있습니다. 그러나 다이어그램에는 현재 `CarouselView` 및 `CollectionView` 형식이 표시 되지 않습니다. 이러한 설정은 컨트롤이 미리 보기 상태를 벗어난 후에 추가 됩니다. 또한 다이어그램은 단일 셸 유형만 표시 합니다.

## <a name="related-links"></a>관련 링크

- [Xamarin Forms 컨트롤 참조](~/xamarin-forms/user-interface/controls/index.md)
