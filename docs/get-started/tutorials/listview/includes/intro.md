---
ms.openlocfilehash: 164befeb08d7cb6691cb037b1f6b7f31c390afea
ms.sourcegitcommit: a153623a69b5cb125f672df8007838afa32e9edf
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67277346"
---
이 자습서를 시도하기 전에 다음 작업을 성공적으로 완료해야 합니다.

- [첫 번째 Xamarin.Forms 앱 빌드](~/get-started/first-app/index.md) 빠른 시작
- [StackLayout](~/get-started/tutorials/stacklayout/index.yml) 자습서.
- [그리드](~/get-started/tutorials/grid/index.yml) 자습서.
- [레이블](~/get-started/tutorials/label/index.yml) 자습서.
- [이미지](~/get-started/tutorials/image/index.yml) 자습서.

이 자습서에서는 다음과 같은 작업을 수행하는 방법을 살펴봅니다.

> [!div class="checklist"]
> - XAML에서 Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView)를 만듭니다.
> - `ListView`를 데이터로 채웁니다.
> - 선택된 `ListView` 항목에 응답합니다.
> - `ListView` 셀 모양을 사용자 지정합니다.

Visual Studio 2019 또는 Mac용 Visual Studio를 사용하여 [`ListView`](xref:Xamarin.Forms.ListView)의 모양을 사용자 지정하는 방법을 보여 주는 간단한 애플리케이션을 만들겠습니다. 다음 스크린샷은 최종 애플리케이션을 보여 줍니다.

[![데이터 템플릿으로 항목의 템플릿이 지정된 ListView의 스크린샷](../images/customize-cell-appearance-reduced.png "템플릿 지정된 데이터를 표시하는 ListView")](../images/customize-cell-appearance-large.png#lightbox "템플릿 지정된 데이터를 표시하는 ListView")
