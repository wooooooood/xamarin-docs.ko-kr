---
ms.openlocfilehash: cbf301b341f78ae8d8826580d30a13393716f56c
ms.sourcegitcommit: 61a35d0643eb3bf5adb8f8831da54771d8dde626
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71059736"
---
이 자습서를 시도하기 전에 다음 작업을 성공적으로 완료해야 합니다.

- [첫 번째 Xamarin.Forms 앱 빌드](~/get-started/first-app/index.md) 빠른 시작

이 자습서에서는 다음과 같은 작업을 수행하는 방법을 살펴봅니다.

> [!div class="checklist"]
>
> - XAML에서 Xamarin.Forms [`Grid`](xref:Xamarin.Forms.Grid)를 만듭니다.
> - `Grid`에 대한 열과 행을 지정합니다.
> - `Grid`의 여러 열 또는 여러 행에 콘텐츠를 스패닝합니다.

Visual Studio 2019 또는 Mac용 Visual Studio를 사용하여 [`Grid`](xref:Xamarin.Forms.Grid) 안에 컨트롤을 레이아웃하는 방법을 보여 주는 간단한 애플리케이션을 만들겠습니다. 다음 스크린샷은 최종 애플리케이션을 보여 줍니다.

[![iOS 및 Android에서 여러 열과 행에 분산된 콘텐츠가 포함된 그리드의 스크린샷](../images/span-columns-rows.png "열과 행이 분산된 콘텐츠가 있는 그리드")](../images/span-columns-rows-large.png#lightbox "열과 행이 분산된 콘텐츠가 있는 그리드")
