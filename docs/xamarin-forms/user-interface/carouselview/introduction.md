---
title: Xamarin.FormsCarouselView 소개
description: CarouselView는 스크롤 가능한 레이아웃으로 데이터를 표시 하기 위한 뷰입니다. 사용자는 항목 컬렉션을 통해 이동할 수 있습니다.
ms.prod: xamarin
ms.assetid: 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/08/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2e67acd0188e1147481005502ad9ccdaada645d9
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84140272"
---
# <a name="xamarinforms-carouselview-introduction"></a>Xamarin.FormsCarouselView 소개

![](~/media/shared/preview.png "This API is currently pre-release")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)는 스크롤 가능한 레이아웃으로 데이터를 표시 하기 위한 뷰입니다. 사용자는 항목 컬렉션을 통해 이동할 수 있습니다. 기본적으로는 `CarouselView` 가로 방향으로 해당 항목을 표시 합니다. 단일 항목이 화면에 표시 되 고, 살짝 밀기 제스처를 사용 하 여 항목 컬렉션을 전달 하 고 뒤로 탐색 합니다. 또한의 각 항목을 나타내는 표시기를 표시할 수 있습니다 `CarouselView` .

[![IOS 및 Android에서 CarouselView 및 IndicatorView의 스크린샷](populate-data-images/indicators.png "IndicatorView 원")](populate-data-images/indicators-large.png#lightbox "IndicatorView 원")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)4.3에서 사용할 수 있습니다 Xamarin.Forms . 그러나 현재 실험적 이며 iOS의 클래스에 다음 코드 줄을 추가 `AppDelegate` 하거나를 `MainActivity` 호출 하기 전에 Android의 클래스에 다음 코드 줄을 추가 하는 방법 으로만 사용할 수 있습니다 `Forms.Init` .

```csharp
Forms.SetFlags("CarouselView_Experimental");
```

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)는 iOS 및 Android에서 사용할 수 있지만 일부 기능은 유니버설 Windows 플랫폼에서 부분적 으로만 사용할 수 있습니다.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)는를 사용 하 여 대부분의 구현을 공유 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 합니다. 그러나 두 컨트롤에는 서로 다른 사용 사례가 있습니다. `CollectionView`는 일반적으로 모든 길이의 데이터 목록을 표시 하는 데 사용 되는 반면 `CarouselView` 은 일반적으로 제한 된 길이 목록의 정보를 강조 표시 하는 데 사용 됩니다.
