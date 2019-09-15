---
title: Xamarin Forms CarouselView 소개
description: CarouselView는 회전식과 비슷한 레이아웃으로 데이터를 표시 하는 보기입니다.
ms.prod: xamarin
ms.assetid: 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f
ms.technology: xamarin-forms
author: pauldipietro
ms.author: padipi
ms.date: 09/09/2019
ms.openlocfilehash: 7979f6ed362c580d9cf80f19b3bc0ea7550ca70c
ms.sourcegitcommit: cf56d2bae34dc0f8e94c2d3d28d5f460d59807bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985967"
---
# <a name="xamarinforms-carouselview-introduction"></a>Xamarin Forms CarouselView 소개

![](~/media/shared/preview.png "이 API는 현재 시험판임")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)는 회전식과 비슷한 방식으로 정보를 표시 하는 보기입니다.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)는 Xamarin. Forms 4.3에서 사용할 수 있습니다. 그러나 현재 실험적 이며 iOS의 `AppDelegate` 클래스에 다음 코드 줄을 추가 하거나 `MainActivity` 를 호출 `Forms.Init`하기 전에 Android의 클래스에 다음 코드 줄을 추가 하는 방법 으로만 사용할 수 있습니다.

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

_참고: 이 문서를 작성할 당시 CarouselView는 여전히 CollectionView 플래그를 사용 하 여 해당 기능을 사용 하도록 설정 합니다._

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)는 iOS 및 Android에서 사용할 수 있지만 일부 기능은 유니버설 Windows 플랫폼에서 부분적 으로만 사용할 수 있습니다.

## <a name="when-to-use-carouselview"></a>CarouselView를 사용 하는 경우

CarouselView는 CollectionView의 구현을 대부분 기반으로 하지만 그 용도는 더 집중 합니다. CollectionView 및 CarouselView를 사용 하는 것은 사용자의 재량 이지만, 앱의 슬라이드은 일반적으로 정보를 강조 표시 하는 데 사용 되 고 사용 되는 총 항목 수로 제한 됩니다.
