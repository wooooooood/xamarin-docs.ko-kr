---
ms.openlocfilehash: 8820d52a2a41ce3c5de9cf1176d5a8a90a987900
ms.sourcegitcommit: 6b833f44d5fd8dc7ab7f8546e8b7d383e5a989db
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71107303"
---
이 자습서를 시도하기 전에 다음 작업을 성공적으로 완료해야 합니다.

- [첫 번째 Xamarin.Forms 앱 빌드](~/get-started/first-app/index.md) 빠른 시작

이 자습서에서는 다음과 같은 작업을 수행하는 방법을 살펴봅니다.

> [!div class="checklist"]
>
> - XAML에서 Xamarin.Forms [`StackLayout`](xref:Xamarin.Forms.StackLayout)을 만듭니다.
> - `StackLayout`의 방향을 지정합니다.
> - `StackLayout` 내에서 자식 뷰 맞춤 및 확장을 제어합니다.

Visual Studio 2019 또는 Mac용 Visual Studio를 사용하여 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 안에 컨트롤을 맞추는 방법을 보여 주는 간단한 애플리케이션을 만들겠습니다. 다음 스크린샷은 최종 애플리케이션을 보여 줍니다.

[![iOS 및 Android에서 맞춤 및 확장 옵션이 설정된 StackLayout의 자식 뷰 스크린샷](../images/alignment-expansion-reduced.png "맞춤 및 확장 집합이 있는 레이블 인스턴스가 포함된 StackLayout")](../images/alignment-expansion-large.png#lightbox "맞춤 및 확장 집합이 있는 레이블 인스턴스가 포함된 StackLayout")
