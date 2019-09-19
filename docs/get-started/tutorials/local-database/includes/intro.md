---
ms.openlocfilehash: 8f379e8639846d9a6424c4aaadf83ff89d7a8684
ms.sourcegitcommit: 6b833f44d5fd8dc7ab7f8546e8b7d383e5a989db
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71107300"
---
이 자습서를 시도하기 전에 다음 작업을 성공적으로 완료해야 합니다.

- [첫 번째 Xamarin.Forms 앱 빌드](~/get-started/first-app/index.md) 빠른 시작
- [StackLayout](~/get-started/tutorials/stacklayout/index.yml) 자습서
- [단추](~/get-started/tutorials/button/index.yml) 자습서
- [항목](~/get-started/tutorials/entry/index.yml) 자습서
- [ListView](~/get-started/tutorials/listview/index.yml) 자습서

이 자습서에서는 다음과 같은 작업을 수행하는 방법을 살펴봅니다.

> [!div class="checklist"]
>
> - NuGet Package Manager를 사용하여 SQLite.NET을 Xamarin.Forms 프로젝트에 추가합니다.
> - 데이터 액세스 클래스를 만듭니다.
> - 데이터 액세스 클래스를 사용합니다.

Visual Studio 2019 또는 Mac용 Visual Studio를 사용하여 로컬 SQLite.NET 데이터베이스에 데이터를 저장하는 방법을 보여 주는 간단한 애플리케이션을 만들겠습니다. 다음 스크린샷은 최종 애플리케이션을 보여 줍니다.

[![iOS 및 Android에서 로컬 SQLite.NET 데이터베이스 데이터 지속성의 스크린샷](../images/consume-data-access-classes-reduced.png "로컬 데이터베이스 데이터 지속성")](../images/consume-data-access-classes-large.png#lightbox "로컬 데이터베이스 데이터 지속성")
