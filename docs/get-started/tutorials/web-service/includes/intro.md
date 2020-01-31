---
ms.openlocfilehash: 338b03ae5e52b06c6ddc225b418ee2bc7d5e5ffc
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2020
ms.locfileid: "71107304"
---
이 자습서를 시도하기 전에 다음 작업을 성공적으로 완료해야 합니다.

- [첫 번째 Xamarin.Forms 앱 빌드](~/get-started/first-app/index.md) 빠른 시작
- [그리드](~/get-started/tutorials/grid/index.yml) 자습서
- [레이블](~/get-started/tutorials/label/index.yml) 자습서
- [단추](~/get-started/tutorials/button/index.yml) 자습서
- [항목](~/get-started/tutorials/entry/index.yml) 자습서

이 자습서에서는 다음과 같은 작업을 수행하는 방법을 살펴봅니다.

> [!div class="checklist"]
>
> - NuGet 패키지 관리자를 사용하여 Newtonsoft.Json을 Xamarin.Forms 프로젝트에 추가합니다.
> - 웹 서비스 클래스를 만듭니다.
> - 웹 서비스 클래스를 사용합니다.

Visual Studio 2019 또는 Mac용 Visual Studio를 사용하여 [OpenWeatherMap](https://openweathermap.org/) 웹 서비스에서 데이터를 검색하는 방법을 보여 주는 간단한 애플리케이션을 만들겠습니다. 다음 스크린샷은 최종 애플리케이션을 보여 줍니다.

[![iOS 및 Android에서 시애틀 날씨 데이터 스크린샷](../images/consume-web-service.png "시애틀 날씨 데이터")](../images/consume-web-service-large.png#lightbox "시애틀 날씨 데이터")
