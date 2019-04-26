---
title: .NET 표준 라이브러리를 사용 하 여 코드 공유
description: 이 문서에서는.NET 표준 라이브러리를 사용 하 여 코드를 공유 하는 방법을 설명 합니다. .NET Standard 라이브러리를 만들고 해당 설정을 편집 응용 프로그램에서 사용에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
author: conceptdev
ms.author: crdun
ms.custom: video
ms.date: 07/18/2018
ms.openlocfilehash: d07b248b36feee909db9c863eb17f1a900f58e60
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61191362"
---
# <a name="net-standard-library-code-sharing"></a>.NET 표준 라이브러리 코드 공유

Xamarin 및.NET Core를 포함 하 여 모든.NET 플랫폼에 대 한 균일 한 API를가 하는.NET standard 라이브러리. 단일.NET Standard 라이브러리를 만들고 사용 하 여.NET Standard 플랫폼을 지 원하는 모든 런타임에서 합니다. 가리킵니다 [이 차트](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support) 지원 되는 플랫폼의 세부 정보에 대 한 합니다.

.NET 표준 버전 1.0부터 1.6까지.NET Framework의 증분 방식으로 큰 하위 집합을 제공 하는 동안.NET Standard 2.0 Xamarin 응용 프로그램 및 기존 이식 가능한 클래스 라이브러리를 이식 하는 것에 대 한 가장 높은 수준의 지원을 제공 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

이 섹션을 만들고 mac 용 Visual Studio를 사용 하 여.NET Standard 라이브러리를 사용 하는 방법을 안내합니다

### <a name="creating-a-net-standard-library"></a>.NET 표준 라이브러리 만들기

다음이 단계를 사용 하 여 솔루션에.NET Standard 라이브러리를 추가할 수 있습니다.

1. 에 **새 프로젝트 추가** 대화 상자에서 선택 합니다 **.NET Core** 범주 선택한 후 **.NET 표준 라이브러리**:

    ![.NET Standard 라이브러리를 만드는](net-standard-images/vsm01-m157.png "새.NET Standard 라이브러리 만들기")

2. 다음 화면에서 대상 프레임 워크-선택할 **.NET Standard 2.0** 것이 좋습니다.

    [![.NET Standard 2.0을 선택 합니다.](net-standard-images/vsm01a-m157-sml.png)](net-standard-images/vsm01a-m157.png#lightbox)

3. 마지막 화면에서 프로젝트 이름을 입력 하 고 클릭 **만들기**합니다.

4. .NET Standard 라이브러리 프로젝트는 솔루션 탐색기에 표시 된 대로 표시 됩니다. 라이브러리는 종속성 노드가 표시 됩니다는 [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/)합니다.

    ![솔루션의 종속성 노드의.NET Standard 나타냅니다.](net-standard-images/vsm02-m157.png)

#### <a name="editing-net-standard-library-settings"></a>.NET 표준 라이브러리 설정 편집

.NET 표준 라이브러리 설정 보기 및 변경 된 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 `Options` 이 스크린샷에 표시 된 대로:

![.NET 표준 대상 프레임 워크 프로젝트 옵션에서 편집](net-standard-images/vsm03-m157.png "프로젝트 옵션은.NET 표준 대상 프레임 워크의 버전 편집")

내부 버전을 변경할 수 있습니다 `netstandard` 바꿔서는 `Target Framework` 드롭다운 값입니다.

**또한:** 편집할 수는 `.csproj` 이 값을 변경 하려면 직접.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-2017-windows"></a>Visual Studio 2017 (Windows)

이 섹션을 만들고 Visual Studio를 사용 하 여.NET Standard 라이브러리를 사용 하는 방법을 안내 합니다.

### <a name="creating-a-net-standard-library"></a>.NET Standard 라이브러리 만들기

.NET Standard 라이브러리를 솔루션에 추가 매우 단순 합니다.

1. 에 **새 프로젝트** 대화 상자에서 선택 합니다 **.NET Standard** 범주 및 선택 **클래스 라이브러리 (.NET Standard)**.

    ![새.NET Standard 클래스 라이브러리를 만드는](net-standard-images/vs01-w157.png "만들기 새.NET Standard 클래스 라이브러리")

2. .NET Standard 라이브러리 프로젝트는 솔루션 탐색기에 표시 된 대로 표시 됩니다. 라이브러리는 종속성 노드가 표시 됩니다는 [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/)합니다.

    ![프로젝트 폴더에는 NETStandard.Library](net-standard-images/vs02-w157.png "솔루션의.NET Standard 프로젝트")

### <a name="editing-net-standard-library-settings"></a>.NET Standard 라이브러리 설정 편집

.NET 표준 라이브러리 설정을 확인 하 고 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 변경할 수 있습니다 **속성** 이 스크린샷에 표시 된 대로:

![.NET 표준 대상 프레임 워크 프로젝트 속성에서 편집](net-standard-images/vs03-w157.png "다른 프로젝트와 동일한 방식으로.NET 표준 라이브러리 참조")

**또한:** 편집할 수는 `.csproj` 직접 편집 하는 `TargetFramework` 요소 및 버전 변경 (예: 대상 `<TargetFramework>netstandard2.0</TargetFramework>`).

### <a name="using-a-net-standard-library-project"></a>.NET Standard 라이브러리 프로젝트를 사용 하 여

.NET Standard 라이브러리를 만든 후에 일반적으로 참조를 추가 하면 동일한 방식 호환 되는 모든 응용 프로그램 또는 라이브러리 프로젝트에 대 한 참조를 추가할 수 있습니다. Visual Studio에서 참조 노드를 마우스 오른쪽 단추로 클릭 하 고 선택 **참조 추가...**  다음으로 전환 합니다 **프로젝트 > 솔루션** 표시 된 것 처럼 탭:

![.NET 표준 라이브러리 참조](net-standard-images/vs04.png "Visual Studio에서 참조 노드를 마우스 오른쪽 단추로 클릭 하 고 참조 추가... 선택한 표시 된 것 처럼 솔루션 프로젝트 탭으로 전환")

-----

## <a name="net-standard-and-xamarinforms-for-the-net-developer-video"></a>.NET standard 및.NET 개발자 (비디오)에 대 한 Xamarin.Forms

> [!Video https://channel9.msdn.com/Shows/XamarinShow/NET-Standard-and-XamarinForms-for-the-NET-Developer/player]

## <a name="related-links"></a>관련 링크

* [.NET standard](https://docs.microsoft.com/dotnet/standard/net-standard) -자세한 내용 및 PCL 비교 합니다.
