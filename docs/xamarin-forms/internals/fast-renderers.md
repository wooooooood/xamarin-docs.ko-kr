---
title: "빠른 렌더러"
description: "이 문서에서는 결과 네이티브 컨트롤 계층 구조를 평면화 하 여 확장 및 Android에서 Xamarin.Forms 컨트롤의 렌더링 비용을 줄일 빠른 렌더러를 소개 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 92a11ebe983840270d3679fd11f5faa0b8222cfe
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="fast-renderers"></a>빠른 렌더러

_이 문서에서는 결과 네이티브 컨트롤 계층 구조를 평면화 하 여 확장 및 Android에서 Xamarin.Forms 컨트롤의 렌더링 비용을 줄일 빠른 렌더러를 소개 합니다._

일반적으로 대부분의 Android에서 원래 컨트롤 렌더러는 두 보기 중 구성 되어 있습니다.

- 등의 네이티브 컨트롤는 `Button` 또는 `TextView`합니다.
- 컨테이너 `ViewGroup` 레이아웃 작업, 제스처 처리 및 기타 작업 중 일부를 처리 하는 합니다.

그러나이 방법은 성능 영향은 한다는 더 많은 메모리와 추가 처리가 화면에 렌더링 하는 보다 복잡 한 시각적 트리는 각 논리 컨트롤에 대해 두 개의 뷰가 만들어집니다.

빠른 렌더러를 하나의 보기에는 인플레이션이 및 Xamarin.Forms 컨트롤의 렌더링 비용을 절감 합니다. 따라서 두 가지 보기를 만들고 보기 트리에 추가 하는 대신 하나에만 생성 됩니다. 메모리 사용 (또한 적은 가비지 컬렉션 일시 중지가 발생)와 것을 의미는 덜 복잡 한 보기 트리, 적은 개체를 만들어 성능이 향상 됩니다.

빠른 렌더러 Android에서 Xamarin.Forms 2.4에서 다음 컨트롤에 대해 사용할 수 있습니다.

- [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)
- [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)
- [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)

기능적으로 이러한 빠른 렌더러는 원래 렌더러에 다르지 않습니다. 그러나 현재 실험적 되며 다음 줄의 코드를 추가 하 여 에서만 사용할 수 있습니다 프로그램 `MainActivity` 호출 하기 전에 클래스 `Forms.Init`:

```csharp
Forms.SetFlags("FastRenderers_Experimental");
```

> [!NOTE]
> **참고**: 빠른 렌더러는 에서만 앱 compat Android 백엔드에 적용할 수 있으므로 이전 응용 프로그램 호환성 활동에이 설정이 무시 됩니다.

레이아웃의 복잡성에 따라 각 응용 프로그램에 대 한 성능 향상 달라 집니다. 예를 들어, x2의 성능 향상은 스크롤할 때 가능한 한 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 수천 빠른 렌더러를 사용 하는 컨트롤의 각 행의 셀을 수행 되는, 데이터의 행을 포함 하는 결과에 시각적으로 부드러운 스크롤입니다.


## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
