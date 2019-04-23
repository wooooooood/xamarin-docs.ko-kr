---
ms.openlocfilehash: 5929648b802c27916c0a21142907644781d59d0d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61193258"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio를 실행하고 **LocalDatabaseTutorial**라는 새로운 빈 Xamarin.Forms 애플리케이션을 만듭니다. 앱이 공유 코드 메커니즘으로 .NET 표준을 사용하는지 확인합니다.

    > [!IMPORTANT]
    > 이 자습서의 C# 및 XAML 코드 조각은 솔루션의 이름이 **LocalDatabaseTutorial**이어야 합니다. 이 자습서에서 코드를 솔루션으로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

    생성된 .NET 표준 라이브러리에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](~/get-started/first-app/index.md)의 [Xamarin.Forms 애플리케이션 분석](~/get-started/first-app/index.md)을 참조하세요.

1. **솔루션 탐색기**에서 **LocalDatabaseTutorial** 프로젝트를 선택하고 마우스 오른쪽 단추로 클릭한 다음, **NuGet 패키지 관리...** 를 선택합니다.

    ![선택된 NuGet 패키지 관리 메뉴 항목의 스크린샷](../images/vs/add-nuget-packages.png "NuGet 패키지 추가 메뉴 항목")

1. **NuGet 패키지 관리자**에서 **찾아보기** 탭을 선택하고 **sqlite-net-pcl** NuGet 패키지를 검색하여 선택한 다음, **설치** 단추를 클릭하여 프로젝트에 추가합니다.

    ![NuGet 패키지 관리자의 SQLite.NET NuGet 패키지 스크린샷](../images/vs/add-package.png "SQLite.NET NuGet 패키지")

    > [!NOTE]
    > 이름이 유사한 NuGet 패키지가 여러 개 있습니다. 올바른 패키지에는 이러한 특성이 있습니다.
    > - **작성자:** Frank A. Krueger
    > - **ID:** sqlite-net-pcl
    > - **NuGet 링크:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > 패키지 이름에도 불구하고 이 NuGet 패키지는 .NET 표준 패키지에서 사용할 수 있습니다.

    이 패키지는 데이터베이스 작업을 애플리케이션에 통합하는 데 사용됩니다.

1. 오류가 없는지 확인하기 위해 솔루션을 빌드합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac용 Visual Studio를 실행하고 **LocalDatabaseTutorial**라는 새로운 빈 Xamarin.Forms 애플리케이션을 만듭니다. 앱이 공유 코드 메커니즘으로 .NET 표준을 사용하는지 확인합니다.

    > [!IMPORTANT]
    > 이 자습서의 C# 및 XAML 코드 조각은 솔루션의 이름이 **LocalDatabaseTutorial**이어야 합니다. 이 자습서에서 코드를 솔루션으로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.
    
    생성된 .NET 표준 라이브러리에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](~/get-started/first-app/index.md)의 [Xamarin.Forms 애플리케이션 분석](~/get-started/first-app/index.md)을 참조하세요.

1. **Solution Pad**에서 **LocalDatabaseTutorial** 프로젝트를 선택하고 마우스 오른쪽 단추로 클릭한 다음, **추가 > NuGet 패키지 추가...** 를 선택합니다.

    ![선택된 NuGet 패키지 추가 메뉴 항목의 스크린샷](../images/vsmac/add-nuget-packages.png "NuGet 패키지 추가 메뉴 항목")

1. **패키지 추가** 창에서 **sqlite-net-pcl** NuGet 패키지를 검색하여 선택한 다음, **패키지 추가** 단추를 클릭하여 프로젝트에 추가합니다.

    ![NuGet 패키지 관리자의 SQLite.NET NuGet 패키지 스크린샷](../images/vsmac/add-package.png "SQLite.NET NuGet 패키지")

    > [!NOTE]
    > 이름이 유사한 NuGet 패키지가 여러 개 있습니다. 올바른 패키지에는 이러한 특성이 있습니다.
    > - **작성자:** Frank A. Krueger
    > - **ID:** sqlite-net-pcl
    > - **NuGet 링크:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > 패키지 이름에도 불구하고 이 NuGet 패키지는 .NET 표준 패키지에서 사용할 수 있습니다.

    이 패키지는 데이터베이스 작업을 애플리케이션에 통합하는 데 사용됩니다.

1. 오류가 없는지 확인하기 위해 솔루션을 빌드합니다.
