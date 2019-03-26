---
title: Xamarin.Forms 빠른 렌더러
description: 이 문서에서는 결과 네이티브 컨트롤 계층 구조를 평면화 하 여 인플레이션 및 Android에서 Xamarin.Forms 컨트롤의 렌더링 비용을 줄일 수 있는 빠른 렌더러를 소개 합니다.
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 9a40644df6abcbbcc327b1b0c2dcb26c2dbc4db5
ms.sourcegitcommit: 247a6d00a95fd7f4cf918d923e5f357c8db56761
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58420212"
---
# <a name="xamarinforms-fast-renderers"></a>Xamarin.Forms 빠른 렌더러

![미리 보기](~/media/shared/preview.png)

_이 문서에서는 결과 네이티브 컨트롤 계층 구조를 평면화 하 여 인플레이션 및 Android에서 Xamarin.Forms 컨트롤의 렌더링 비용을 줄일 수 있는 빠른 렌더러 (Xamarin.Forms 2.4에서 추가)를 소개 합니다._

일반적으로 대부분의 Android에서 원래 컨트롤 렌더러는 두 보기 중 구성 되어 있습니다.

- 등의 네이티브 컨트롤을 `Button` 또는 `TextView`합니다.
- 컨테이너 `ViewGroup` 레이아웃 작업, 제스처 처리 및 기타 작업 중 일부를 처리 하는 합니다.

그러나이 방법은 더 많은 메모리 및 추가 화면에서 렌더링할 처리가 필요한 복잡 한 시각적 트리는 각 논리적 컨트롤에 대해 두 개의 뷰가 만들어집니다에 성능 영향은 있습니다.

빠른 렌더러를 단일 보기로 인플레이션 및 Xamarin.Forms 컨트롤의 렌더링 비용을 줄입니다. 따라서 두 가지 보기를 만들고 보기 트리에 추가 하는 대신 하나만 생성 됩니다. 메모리 사용 (더 적은 가비지 컬렉션 일시 중지는 결과도) 것을 의미는 덜 복잡 한 뷰 트리를 더 적은 개체를 만들어 줄이고 성능을 향상 시킵니다.

빠른 렌더러 Android에서 Xamarin.Forms 2.4이 하에서는 다음 컨트롤에 대해 사용할 수 있습니다.

- [`Button`](xref:Xamarin.Forms.Button)
- [`Image`](xref:Xamarin.Forms.Image)
- [`Label`](xref:Xamarin.Forms.Label)
- [`Frame`](xref:Xamarin.Forms.Frame)

기능적으로 이러한 빠른 렌더러는 원래 렌더러에 차이가 없습니다. 그러나 현재 실험적 이며 코드의 다음 줄을 추가 하 여 에서만 사용할 수 있습니다 하 `MainActivity` 클래스를 호출 하기 전에 `Forms.Init`:

```csharp
Forms.SetFlags("FastRenderers_Experimental");
```

> [!NOTE]
> 빠른 렌더러 app compat Android 백 엔드에 적용할만 되므로 pre-app compat 활동에이 설정이 무시 됩니다.

성능 향상 각 응용 프로그램의 경우 레이아웃의 복잡성에 따라 달라 집니다. 예를 들어, x2의 성능 향상은 스크롤할 때 가능한 한 [ `ListView` ](xref:Xamarin.Forms.ListView) 수천 개의 행의 각 행의 셀이 이루어지는 빠른 렌더러를 사용 하는 컨트롤에 데이터를 포함 하는 결과에서 시각적으로 부드러운 스크롤입니다.


## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
