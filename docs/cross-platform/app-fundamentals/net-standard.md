---
title: .NET Standard 라이브러리를 사용 하 여 코드 공유
description: 이 문서에서는 .NET Standard 라이브러리를 사용 하 여 코드를 공유 하는 방법을 설명 합니다. .NET Standard 라이브러리를 만들고 해당 설정을 편집 하 고 응용 프로그램에서 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
author: davidortinau
ms.author: daortin
ms.custom: video
ms.date: 07/18/2018
ms.openlocfilehash: cae59053374f673a56d02e86cd59fb85f313c41b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016807"
---
# <a name="net-standard-library-code-sharing"></a>.NET Standard 라이브러리 코드 공유

.NET Standard 라이브러리에는 Xamarin 및 .NET Core를 비롯 한 모든 .NET 플랫폼에 대해 일관 된 API가 있습니다. 단일 .NET Standard 라이브러리를 만들어 .NET Standard 플랫폼을 지 원하는 모든 런타임에서 사용 합니다. 지원 되는 플랫폼에 대 한 자세한 내용은 [이 차트](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support) 를 참조 하세요.

1\.0 ~ 1.6 .NET Standard 버전은 .NET Framework의 증분 하위 집합을 제공 하지만, .NET Standard 2.0는 Xamarin 응용 프로그램에 대 한 최상의 지원과 기존 이식 가능한 클래스 라이브러리를 이식할 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

이 섹션에서는 Mac용 Visual Studio를 사용 하 여 .NET Standard 라이브러리를 만들고 사용 하는 방법을 안내 합니다.

### <a name="creating-a-net-standard-library"></a>.NET Standard 라이브러리 만들기

다음 단계를 사용 하 여 솔루션에 .NET Standard 라이브러리를 추가할 수 있습니다.

1. **새 프로젝트 추가** 대화 상자에서 **.net Core** 범주를 선택 하 고 **.NET Standard 라이브러리**를 선택 합니다.

    ![.NET Standard 라이브러리 만들기](net-standard-images/vsm01-m157.png "새 .NET Standard 라이브러리 만들기")

2. 다음 화면에서 대상 프레임 워크를 선택 합니다.- **.NET Standard 2.0** 를 권장 합니다.

    [![.NET Standard 2.0를 선택 합니다.](net-standard-images/vsm01a-m157-sml.png)](net-standard-images/vsm01a-m157.png#lightbox)

3. 최종 화면에서 프로젝트 이름을 입력 하 고 **만들기**를 클릭 합니다.

4. .NET Standard 라이브러리 프로젝트는 솔루션 탐색기 표시 된 것 처럼 표시 됩니다. 종속성 노드는 라이브러리에서 [Netstandard. library](https://www.nuget.org/packages/NETStandard.Library/)를 사용 하는 것을 표시 합니다.

    ![솔루션의 종속성 노드는 .NET Standard 나타냅니다.](net-standard-images/vsm02-m157.png)

#### <a name="editing-net-standard-library-settings"></a>.NET Standard 라이브러리 설정 편집

.NET Standard 라이브러리 설정은 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 다음 스크린샷에 표시 된 것 처럼 `Options`를 선택 하 여 보고 변경할 수 있습니다.

![프로젝트 옵션에서 .NET Standard 대상 프레임 워크 편집](net-standard-images/vsm03-m157.png "프로젝트 옵션에서 .NET Standard 대상 프레임 워크의 버전을 편집 합니다.")

내에서 `Target Framework` 드롭다운 값을 변경 하 여 `netstandard` 버전을 변경할 수 있습니다.

**또한 다음을 수행 합니다.** `.csproj`를 직접 편집 하 여이 값을 변경할 수 있습니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-2017-windows"></a>Visual Studio 2017 (Windows)

이 섹션에서는 Visual Studio를 사용 하 여 .NET Standard 라이브러리를 만들고 사용 하는 방법을 안내 합니다.

### <a name="creating-a-net-standard-library"></a>.NET Standard 라이브러리 만들기

솔루션에 .NET Standard 라이브러리를 추가 하는 것은 매우 간단 합니다.

1. **새 프로젝트** 대화 상자에서 **.NET Standard** 범주를 선택한 다음 **클래스 라이브러리 (.NET Standard)** 를 선택 합니다.

    ![새 .NET Standard 클래스 라이브러리 만들기](net-standard-images/vs01-w157.png "새 .NET Standard 클래스 라이브러리 만들기")

2. .NET Standard 라이브러리 프로젝트는 솔루션 탐색기 표시 된 것 처럼 표시 됩니다. 종속성 노드는 라이브러리에서 [Netstandard. library](https://www.nuget.org/packages/NETStandard.Library/)를 사용 하는 것을 표시 합니다.

    ![프로젝트 폴더의 NETStandard 라이브러리](net-standard-images/vs02-w157.png "솔루션의 .NET Standard 프로젝트")

### <a name="editing-net-standard-library-settings"></a>.NET Standard 라이브러리 설정 편집

다음 스크린샷에 표시 된 것 처럼 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성** 을 선택 하 여 .NET Standard 라이브러리 설정을 보고 변경할 수 있습니다.

![프로젝트 속성에서 .NET 표준 대상 프레임 워크 편집](net-standard-images/vs03-w157.png "다른 프로젝트와 동일한 방식으로 .NET Standard 라이브러리를 참조 합니다.")

**또한 다음을 수행 합니다.** `.csproj`를 직접 편집 하 여 `TargetFramework` 요소를 편집 하 고 대상 버전을 변경할 수 있습니다 (예: `<TargetFramework>netstandard2.0</TargetFramework>`).

### <a name="using-a-net-standard-library-project"></a>.NET Standard 라이브러리 프로젝트 사용

.NET Standard 라이브러리를 만든 후에는 일반적으로 참조를 추가 하는 것과 같은 방식으로 호환 되는 응용 프로그램 또는 라이브러리 프로젝트에서 해당 라이브러리에 대 한 참조를 추가할 수 있습니다. Visual Studio에서 참조 노드를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가** ...를 선택 하 고 다음과 같이 **프로젝트 > 솔루션** 탭으로 전환 합니다.

![.NET Standard 라이브러리 참조](net-standard-images/vs04.png "Visual Studio에서 참조 노드를 마우스 오른쪽 단추로 클릭 하 고 참조 추가 ...를 선택 합니다. 그런 다음 그림과 같이 솔루션 프로젝트 탭으로 전환 합니다.")

-----

## <a name="net-standard-and-xamarinforms-for-the-net-developer-video"></a>.NET 개발자를 위한 .NET Standard 및 Xamarin.ios (비디오)

> [!Video https://channel9.msdn.com/Shows/XamarinShow/NET-Standard-and-XamarinForms-for-the-NET-Developer/player]

## <a name="related-links"></a>관련 링크

* [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard) -자세한 정보 및 PCL 비교
