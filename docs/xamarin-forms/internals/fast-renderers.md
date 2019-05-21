---
title: Xamarin.Forms 빠른 렌더러
description: 이 문서에서는 결과 네이티브 컨트롤 계층 구조를 평면화 하 여 인플레이션 및 Android에서 Xamarin.Forms 컨트롤의 렌더링 비용을 줄일 수 있는 빠른 렌더러를 소개 합니다.
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
ms.openlocfilehash: 861d9e3f898dcd61015d9aca27ae66c3fe72d1a9
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970718"
---
# <a name="xamarinforms-fast-renderers"></a>Xamarin.Forms 빠른 렌더러

일반적으로 대부분의 Android에서 원래 컨트롤 렌더러는 두 보기 중 구성 되어 있습니다.

- 등의 네이티브 컨트롤을 `Button` 또는 `TextView`합니다.
- 컨테이너 `ViewGroup` 레이아웃 작업, 제스처 처리 및 기타 작업 중 일부를 처리 하는 합니다.

그러나이 방법은 더 많은 메모리 및 추가 화면에서 렌더링할 처리가 필요한 복잡 한 시각적 트리는 각 논리적 컨트롤에 대해 두 개의 뷰가 만들어집니다에 성능 영향은 있습니다.

빠른 렌더러를 단일 보기로 인플레이션 및 Xamarin.Forms 컨트롤의 렌더링 비용을 줄입니다. 따라서 두 가지 보기를 만들고 보기 트리에 추가 하는 대신 하나만 생성 됩니다. 메모리 사용 (더 적은 가비지 컬렉션 일시 중지는 결과도) 것을 의미는 덜 복잡 한 뷰 트리를 더 적은 개체를 만들어 줄이고 성능을 향상 시킵니다.

빠른 렌더러 Android에서 xamarin.forms에서 컨트롤에 사용할 수 있습니다.

- [`Button`](xref:Xamarin.Forms.Button)
- [`Image`](xref:Xamarin.Forms.Image)
- [`Label`](xref:Xamarin.Forms.Label)
- [`Frame`](xref:Xamarin.Forms.Frame)

기능적으로 이러한 빠른 렌더러는 레거시 렌더러에 차이가 없습니다. Xamarin.Forms 4.0에부터 사용 대상으로 하는 모든 응용 프로그램에서 `FormsAppCompatActivity` 기본적으로 이러한 빠른 렌더러를 사용 합니다. 비롯 하 여 모든 새 컨트롤에 대 한 렌더러 [ `ImageButton` ](xref:Xamarin.Forms.ImageButton) 하 고 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView), 빠른 렌더러 접근 방식을 사용 합니다.

빠른 렌더러를 사용 하는 경우 성능 향상 각 응용 프로그램의 경우 레이아웃의 복잡성에 따라 달라 집니다. 예를 들어, x2의 성능 향상은 스크롤할 때 가능한 한 [ `ListView` ](xref:Xamarin.Forms.ListView) 수천 개의 행의 각 행의 셀이 이루어지는 빠른 렌더러를 사용 하는 컨트롤에 데이터를 포함 하는 결과에서 시각적으로 부드러운 스크롤입니다.

> [!NOTE]
> 빠른 렌더러 사용 되는 동일한 접근 방식을 사용 하 여 레거시 렌더러에 대 한 사용자 지정 렌더러를 만들 수 있습니다. 자세한 내용은 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)를 참조하세요.

## <a name="backwards-compatibility"></a>이전 버전과의 호환성

빠른 렌더러는 다음 방법 중 하나를 사용 하 여 재정의할 수 있습니다.

1. 레거시 렌더러를 사용 하도록 설정 하는 코드의 다음 줄을 추가 하 여 사용자 `MainActivity` 클래스를 호출 하기 전에 `Forms.Init`:

    ```csharp
    Forms.SetFlags("UseLegacyRenderers");
    ```

1. 레거시 렌더러를 대상으로 하는 사용자 지정 렌더러를 사용 합니다. 모든 기존 사용자 지정 렌더러 계속 레거시 렌더러를 사용 하 여 작동 합니다.
1. 다른 지정 `View.Visual`와 같은 `Material`, 서로 다른 렌더러를 사용 하는 합니다. Visual 자료에 대 한 자세한 내용은 참조 하세요. [Xamarin.Forms 자료 Visual](~/xamarin-forms/user-interface/visual/material-visual.md)합니다.

## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
