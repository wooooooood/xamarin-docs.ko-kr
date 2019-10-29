---
title: 여러 플랫폼에서 코드 공유
description: 이 문서는 이식 가능한 클래스 라이브러리, 공유 프로젝트, .NET Standard 및 NuGet을 비롯 하 여 코드를 공유 하는 기술을 설명 하는 다양 한 가이드에 연결 됩니다.
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
author: davidortinau
ms.author: daortin
ms.date: 07/18/2018
ms.openlocfilehash: a91fba3cd1fba3bcf2317e8f9cb25631c62491cb
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016822"
---
# <a name="sharing-code-on-multiple-platforms"></a>여러 플랫폼에서 코드 공유

이러한 문서에서는 Windows, Android, iOS 등을 비롯 하 여 플랫폼 간에 코드를 공유 하는 데 사용할 수 있는 다양 한 옵션을 설명 합니다.

## <a name="code-sharing-overviewcode-sharingmd"></a>[코드 공유 개요](code-sharing.md)

.NET Standard 라이브러리 및 공유 프로젝트를 포함 하 여 Xamarin 프로젝트에 사용할 수 있는 다양 한 코드 공유 옵션에 대해 알아봅니다. 이식 가능한 클래스 라이브러리도 지원 되지만 .NET Standard를 위해 더 이상 사용 되지 않는 것으로 간주 됩니다.

## <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET 표준](~/cross-platform/app-fundamentals/net-standard.md)

.NET Standard는 플랫폼 간에 코드를 공유 하는 데 기본 설정 된 옵션입니다. 코드는 특정 버전에 대해 빌드됩니다 (2.0 기존 .NET Framework 코드와의 최상의 API 호환성을 제공). 그런 다음 해당 수준을 지 원하는 다른 프로젝트에서 사용 될 수 있습니다. .NET Standard 프로젝트는 Visual Studio 2019 및 Mac 용 Visual Studio 2019에서 모두 지원 됩니다.

## <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md)

공유 프로젝트를 사용 하면 다양 한 응용 프로그램 프로젝트에서 참조 하는 공통 코드를 작성할 수 있습니다. 이 코드는 각 참조 하는 프로젝트의 일부로 컴파일되며, 공유 코드 베이스에 플랫폼별 기능을 통합 하는 데 도움이 되는 컴파일러 지시문을 포함할 수 있습니다. 이 문서에서는 공유 프로젝트의 작동 방식 및 Xamarin 프로젝트를 사용 하 여 공유 프로젝트를 만들고 사용 하는 방법을 설명 합니다.

## <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md)

이식 가능한 클래스 라이브러리 프로젝트를 사용 하면 공유 코드를 포함 하는 어셈블리를 빌드하고 배포 하 여 여러 플랫폼에서 실행할 수 있습니다. 이식 가능한 클래스 라이브러리 (또는 "PCL")를 만들려면 먼저 대상으로 할 플랫폼을 선택 하 고 해당 플랫폼에 대해 정의 된 프로필에서 사용할 수 있는 .NET Framework의 하위 집합에 대 한 코드를 작성 합니다. PCLs는 최신 버전의 Visual Studio에서 더 이상 사용 되지 않는 것으로 간주 됩니다. 개발자는 .NET Standard 2.0를 대신 사용 하는 것이 좋습니다.

## <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[NuGet 프로젝트: 코드 공유용 다중 플랫폼 라이브러리](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

NuGet 패키지는 PCL 또는 .NET standard 프로젝트에서 자동으로 생성 될 수 있습니다. 및 공유 프로젝트를 별도의 NuGet 프로젝트 형식을 사용 하 여 "bait and switch" NuGet 패키지로 패키지할 수 있습니다. 이 섹션에서는 각 코드 공유 시나리오에 대 한 NuGet 패키지를 만드는 방법을 설명 합니다.

## <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Xamarin에 대 한 NuGet 패키지 수동 만들기](~/cross-platform/app-fundamentals/nuget-manual.md)

Xamarin 플랫폼에서 작동 하는 NuGet 패키지를 만들기 위한 팁입니다.

## <a name="use-cc-libraries-in-cross-platform-xamarin-projectscross-platformcppindexmd"></a>[플랫폼 간 XamarinC++ 프로젝트에서 C/라이브러리 사용](~/cross-platform/cpp/index.md)

이 기법을 사용 하면 C/C++ 라이브러리의 진화, NuGet의 C# 바인딩 및 Xamarin 응용 프로그램을 분리할 수 있습니다. 기능은 네이티브 플랫폼 C/C++ 라이브러리에서 제공 하지만 모든 플랫폼별 코드는 최종 Xamarin 응용 프로그램에서 격리 되므로 코드 중복 없이 가장 높은 성능을 사용할 수 있습니다. 
