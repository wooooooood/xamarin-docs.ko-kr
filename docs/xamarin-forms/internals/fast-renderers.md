---
title: Xamarin.Forms빠른 렌더러
description: 이 문서에서는 Xamarin.Forms 생성 된 네이티브 컨트롤 계층 구조를 평면화 하 여 Android에서 컨트롤의 인플레이션 및 렌더링 비용을 줄이는 빠른 렌더러를 소개 합니다.
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 29f79e4aed0314fe1590fa26c8e4b052e14a94d6
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84198059"
---
# <a name="xamarinforms-fast-renderers"></a>Xamarin.Forms빠른 렌더러

일반적으로 Android에서 대부분의 원래 컨트롤 렌더러는 다음과 같은 두 가지 보기로 구성 됩니다.

- 또는와 같은 네이티브 컨트롤 `Button` `TextView` 입니다.
- `ViewGroup`레이아웃 작업, 제스처 처리 및 기타 작업 중 일부를 처리 하는 컨테이너입니다.

그러나이 방법을 사용 하면 각 논리 컨트롤에 대해 두 개의 뷰가 만들어지므로 성능에 영향을 미칩니다 .이 경우 더 많은 메모리를 필요로 하는 더 복잡 한 시각적 트리와 화면에서 렌더링 하는 추가 처리가 발생 합니다.

빠른 렌더러를 통해 컨트롤의 인플레이션 및 렌더링 비용을 Xamarin.Forms 단일 보기로 줄일 수가 있습니다. 따라서 두 개의 뷰를 만들어 뷰 트리에 추가 하는 대신 하나만 생성 됩니다. 이렇게 하면 더 작은 개체를 만들어 성능을 향상 시킵니다 .이 경우 덜 복잡 한 보기 트리를 사용 하 고 더 작은 메모리 사용을 의미 합니다 .이 경우에도 가비지 수집 일시 중지가 줄어듭니다.

빠른 렌더러는 Android의에서 다음 컨트롤에 사용할 수 있습니다 Xamarin.Forms .

- [`Button`](xref:Xamarin.Forms.Button)
- [`Frame`](xref:Xamarin.Forms.Frame)
- [`Image`](xref:Xamarin.Forms.Image)
- [`Label`](xref:Xamarin.Forms.Label)
- [`MediaElement`](xref:Xamarin.Forms.MediaElement)

기능적으로 이러한 빠른 렌더러는 레거시 렌더러와 다르지 않습니다. Xamarin.Forms4.0부터 모든 응용 프로그램 `FormsAppCompatActivity` 은 기본적으로 이러한 빠른 렌더러를 사용 합니다. 및를 비롯 한 모든 새 컨트롤에 대 한 렌더러 [`ImageButton`](xref:Xamarin.Forms.ImageButton) [`CollectionView`](xref:Xamarin.Forms.CollectionView) 는 빠른 렌더러 방법을 사용 합니다.

빠른 렌더러를 사용 하는 경우의 성능 향상은 레이아웃의 복잡도에 따라 각 응용 프로그램에 따라 달라 집니다. 예를 들어, 수천 개의 데이터 행이 포함 된를 스크롤할 때 x2의 성능 향상이 가능 합니다 [`ListView`](xref:Xamarin.Forms.ListView) .이 경우 각 행의 셀은 빠른 렌더러를 사용 하는 컨트롤로 구성 되며이로 인해 스크롤이 원활 하 게 향상 됩니다.

> [!NOTE]
> 레거시 렌더러에 사용 되는 것과 동일한 방법을 사용 하 여 빠른 렌더러에 대해 사용자 지정 렌더러를 만들 수 있습니다. 자세한 내용은 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)를 참조하세요.

## <a name="backwards-compatibility"></a>이전 버전과의 호환성

빠른 렌더러는 다음과 같은 방법으로 재정의할 수 있습니다.

1. 를 `MainActivity` 호출 하기 전에 클래스에 다음 코드 줄을 추가 하 여 레거시 렌더러를 사용 하도록 설정 합니다 `Forms.Init` .

    ```csharp
    Forms.SetFlags("UseLegacyRenderers");
    ```

1. 레거시 렌더러를 대상으로 하는 사용자 지정 렌더러 사용. 기존의 모든 사용자 지정 렌더러는 레거시 렌더러를 사용 하 여 계속 작동 합니다.
1. `View.Visual`다른 렌더러를 사용 하는와 같은 다른를 지정 하 `Material` 는 경우 재질 시각적 개체에 대 한 자세한 내용은 [ Xamarin.Forms 재질 시각적 개체](~/xamarin-forms/user-interface/visual/material-visual.md)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
