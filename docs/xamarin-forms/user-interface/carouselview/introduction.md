---
title: Xamarin Forms CarouselView 소개
description: CarouselView는 스크롤 가능한 레이아웃으로 데이터를 표시 하기 위한 뷰입니다. 사용자는 항목 컬렉션을 통해 이동할 수 있습니다.
ms.prod: xamarin
ms.assetid: 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/08/2019
ms.openlocfilehash: 2fe4d984f36880493a9a04d99b63876551366477
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696969"
---
# <a name="xamarinforms-carouselview-introduction"></a>Xamarin Forms CarouselView 소개

![](~/media/shared/preview.png "This API is currently pre-release")

[`CarouselView`](xref:Xamarin.Forms.CarouselView) 은 스크롤할 수 있는 레이아웃으로 데이터를 표시 하기 위한 뷰입니다. 사용자는 항목 컬렉션을 통해 이동할 수 있습니다. 기본적으로 `CarouselView`는 가로 방향으로 해당 항목을 표시 합니다. 단일 항목이 화면에 표시 되 고, 살짝 밀기 제스처를 사용 하 여 항목 컬렉션을 전달 하 고 뒤로 탐색 합니다.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) 는 Xamarin. Forms 4.3에서 사용할 수 있습니다. 그러나 현재 실험적 이며 `Forms.Init`를 호출 하기 전에 iOS의 `AppDelegate` 클래스 또는 Android의 `MainActivity` 클래스에 다음 코드 줄을 추가 하 여 사용할 수 있습니다.

```csharp
Forms.SetFlags("CarouselView_Experimental");
```

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView) 는 IOS 및 Android에서 사용할 수 있지만 일부 기능은 유니버설 Windows 플랫폼 일부만 사용할 수 있습니다.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) 은 [`CollectionView`](xref:Xamarin.Forms.CollectionView)를 사용 하 여 대부분의 구현을 공유 합니다. 그러나 두 컨트롤에는 서로 다른 사용 사례가 있습니다. `CollectionView`은 일반적으로 모든 길이의 데이터 목록을 표시 하는 데 사용 되는 반면 `CarouselView`는 일반적으로 제한 된 길이 목록의 정보를 강조 표시 하는 데 사용 됩니다.
