---
title: NuGet을 사용하여 NUnit 2.6.4 설치하기
description: 이 가이드에서는 NuGet을 사용하여 NUnit 3.0 NUnit 2.6.4로 다운그레이드하는 방법을 다룹니다.
ms.prod: xamarin
ms.assetid: 7683F2B8-7FDF-48C4-8E7D-649D4D4E79F0
author: asb3993
ms.author: amburns
ms.date: 03/24/2017
ms.openlocfilehash: e74975864dc7f3f00c6b04fe48139589c1f52ad5
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="installing-nunit-264-using-nuget"></a>NuGet을 사용하여 NUnit 2.6.4 설치하기

_이 가이드에서는 NuGet을 사용하여 NUnit 3.0 NUnit 2.6.4로 다운그레이드하는 방법을 다룹니다._

Mac용 Visual Studio로 테스트를 작성하거나 또는 Xamarin.UITest를 사용하는 개발자는 NUnit 3.0 이상이 Mac용 Visual Studio 또는 Xamarin.UITest와 호환되지 않으므로 [NUnit 2.6.4](http://nunit.org/index.php?p=docHome&r=2.6.4)을 사용해야 합니다. Mac용 Visual Studio 또는 NUnit 3.0을 사용하는 Xamarin.UITests에서 단위 테스트 실행하려는 시도는 실패합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

이 가이드에서는 Mac용 Visual Studio에 대해 NuGet을 사용하여 NUnit 2.6.4를 설치하는 방법을 다룹니다. 또한 이러한 단계는 필요한 경우 NUnit 3.0를 제거합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

이 가이드에서는 Visual Studio 2015에서 NuGet을 사용하여 NUnit 3.0을 NUnit 2.6.4로 다운그레이드하는 방법을 다룹니다.

-----

## <a name="requirements"></a>요구 사항

이 가이드에서는 모바일 앱 프로젝트 및 테스트 프로젝트를 함께 사용하는 기존 솔루션이 있다고 가정합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="installing-nunit-264-in-visual-studio-for-mac"></a>NUnit 2.6.4를 Mac용 Visual Studio에 설치함

다음 단계는 NUnit 2.6.4를 설치하는 방법을 설명합니다.


1. **패키지 관리자 열기** - **패키지**를 마우스 오른쪽 단추로 클릭하고 팝업 메뉴에서 **패키지 추가**를 선택합니다.

    [![](installing-nunit-using-nuget-images/add-packages-xs.png "패키지를 마우스 오른쪽 단추로 클릭하고 팝업 메뉴에서 패키지 추가를 선택합니다.")](installing-nunit-using-nuget-images/add-packages-xs.png#lightbox)
    
1. **`NUnit version:2.6.4`를 검색함** - Mac용 Visual Studio는 NUnit 3.0을 제거(필요한 경우)하고 NUnit 2.6.4를 다운로드하여 설치합니다. **패키지 추가** 대화 상자에서 텍스트 `nunit version:2.6.4`를 오른쪽 위 모서리에 있는 **검색** 필드에 입력합니다. 검색 결과에서 **NUnit**을 선택하고 **패키지 추가** 단추를 클릭합니다.

    [![](installing-nunit-using-nuget-images/nunit-search-xs.png "검색 결과에서 NUnit를 선택하고 패키지 추가 단추를 클릭합니다.")](installing-nunit-using-nuget-images/nunit-search-xs.png#lightbox)


솔루션 패드에서 NUnit 패키지의 버전 번호를 검사하여 NUnit 2.6.4이 설치되었는지 확인할 수 있습니다.

[![](installing-nunit-using-nuget-images/nunit-2-6-4-installed.png "솔루션 패드에서 NUnit 패키지의 버전 번호 검사")](installing-nunit-using-nuget-images/nunit-2-6-4-installed.png#lightbox)

## <a name="summary"></a>요약

이 가이드는 패키지 관리자 콘솔을 사용하여 Mac용 Visual Studio에서 NUnit 3.0을 NUnit 2.6.4로 다운그레이드하는 방법을 설명합니다.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="installing-nunit-264-in-visual-studio"></a>NUnit 2.6.4를 Visual Studio에 설치함

이 섹션에서는 Visual Studio 2015에서 _NuGet 패키지 관리자 콘솔_을 사용하여 NUnit 3.0을 제거하고 NUnit 2.6.4를 설치하는 방법에 중점을 둡니다.


1. **NuGet 패키지 관리자 콘솔 시작하기** - **도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**을 선택합니다.

    [![](installing-nunit-using-nuget-images/package-manager-console.png "NuGet 패키지 관리자 콘솔 시작하기 - 도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔을 선택합니다.")](installing-nunit-using-nuget-images/package-manager-console.png#lightbox)
    
1. **NUnit 버전 확인** - 필요한 경우 명령 `Get-Package -Project <UITEST PROJECT>`을 실행하여 설치된 NUnit 버전을 확인할 수도 있습니다.

    ```bash
    [PM] Get-Package -Project [TEST PROJECT NAME]
    
        Id                                  Versions                                 ProjectName
        --                                  --------                                 -----------
    NUnit                               {3.0.1}                                  [TEST PROJECT NAME]
    Xamarin.UITest                      {1.2.0}                                  [TEST PROJECT NAME]
    ```

NUnit 3.0 이상을 보면 NUnit 2.6.4로 다운그레이드해야 합니다.

1. **NUnit 3.0 제거** - `Uninstall-Package` commandlet을 사용하여 NUnit 3.0을 제거합니다.

        <PM> Uninstall-Package NUnit -Project <TEST PROJECT NAME>
        Attempting to gather dependencies information for package 'NUnit.3.0.1' with respect to project '<TEST PROJECT NAME>', targeting '.NETFramework,Version=v4.5'
        Resolving actions to uninstall package 'NUnit.3.0.1'
        Resolved actions to uninstall package 'NUnit.3.0.1'
        Removed package 'NUnit.3.0.1' from 'packages.config'
        Successfully uninstalled 'NUnit.3.0.1' from <TEST PROJECT NAME>

1. **NUnit 2.6.4 설치** - 다음 코드 조각에서 설명한 것처럼 `Install-Package` commandlet을 사용하여 Nunit 2.6.4를 설치합니다.

        <PM> Install-Package NUnit -Version 2.6.4 -Project <TEST PROJECT NAME>
        Attempting to gather dependencies information for package 'NUnit.2.6.4' with respect to project '<TEST PROJECT NAME>', targeting '.NETFramework,Version=v4.5'
        Attempting to resolve dependencies for package 'NUnit.2.6.4' with DependencyBehavior 'Lowest'
        Resolving actions to install package 'NUnit.2.6.4'
        Resolved actions to install package 'NUnit.2.6.4'
        Adding package 'NUnit.2.6.4' to folder 'Z:\Desktop\DowngradeNunit\packages'
        Added package 'NUnit.2.6.4' to folder 'Z:\Desktop\DowngradeNunit\packages'
        Added package 'NUnit.2.6.4' to 'packages.config'
        Successfully installed 'NUnit 2.6.4' to <TEST PROJECT NAME>
    
## <a name="summary"></a>요약

이 가이드는 패키지 관리자 콘솔을 사용하여 Visual Studio 2015에서 NUnit 3.0을 NUnit 2.6.4로 다운그레이드하는 방법을 설명합니다.

-----

## <a name="related-links"></a>관련 링크

- [NUnit 2.6.4](http://nunit.org/index.php?p=docHome&r=2.6.4)
- [NUnit 2.6.4 NuGet 패키지](https://www.nuget.org/packages/NUnit/2.6.4)
