---
title: 여러 플랫폼에서 코드 공유
description: 이 문서는 이식 가능한 클래스 라이브러리, 공유 프로젝트,.NET Standard 및 NuGet을 포함 하 여 코드를 공유 하는 기술을 설명 하는 다양 한 가이드에 연결 합니다.
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 3a2c3f98e3ba83db0794a68ff1d62a9845a111c0
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270191"
---
# <a name="sharing-code-on-multiple-platforms"></a>여러 플랫폼에서 코드 공유

이 문서에서는 Windows, Android, iOS 등을 포함 하는 플랫폼 간 코드 공유에 사용할 수 있는 다양 한 옵션을 설명 합니다.

## <a name="code-sharing-overviewcode-sharingmd"></a>[코드 공유 개요](code-sharing.md)

Xamarin 프로젝트의 경우.NET 표준 라이브러리 및 공유 프로젝트를 포함 하 여 사용할 수 있는 옵션을 공유 다른 코드에 알아봅니다. 그러나 이식 가능한 클래스 라이브러리도 지원 됩니다, 그리고.NET Standard를 위해 사용 되지 않는 간주 됩니다.

## <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET standard는 플랫폼 간 코드 공유에 대 한 기본 옵션입니다. 코드가 작성 되는 특정 버전 (2.0 기존.NET Framework 코드를 사용 하 여 최상의 API 호환성을 제공 하는 데 사용) 이상 해당 수준이 지 원하는 다른 프로젝트에서 사용 될 수 있습니다. Mac 용 Visual Studio 2017 및 Visual Studio에서.NET standard 프로젝트는 지원

## <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md)

공유 프로젝트의 여러 다른 응용 프로그램 프로젝트에서 참조 되는 일반적인 코드를 작성할 수 있습니다. 코드를 참조 하는 각 프로젝트의 일부로 컴파일되고 공유 코드 베이스에 플랫폼 특정 기능을 통합 하는 데 컴파일러 지시문을 포함할 수 있습니다. 이 문서는 공유 프로젝트의 작동 방법 및 만들고 Xamarin 프로젝트와 함께 사용 하는 방법을 설명 합니다.

## <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md)

이식 가능한 클래스 라이브러리 프로젝트를 사용 하 여 작성 하 고 여러 플랫폼에서 실행 하는 공유 코드를 포함 하는 어셈블리를 배포할 수 있습니다. 이식 가능한 클래스 라이브러리 (또는 "PCL")을 만들려면 먼저 플랫폼을 대상으로 한 다음 해당 플랫폼에 대해 정의 된 프로필에서 사용할 수 있는.NET Framework의 하위 집합에 대 한 코드를 작성 합니다.를 선택 합니다. Pcl는 최신 버전의 Visual Studio에서 사용 되지 않을 것으로 간주 됩니다. 개발자는.NET Standard 2.0을 대신 사용 하는 것이 좋습니다.

## <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[NuGet 프로젝트: 코드가 공유에 대 한 다중 플랫폼 라이브러리](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

PCL 또는.NET standard 프로젝트에서 NuGet 패키지를 자동으로 생성할 수 있습니다. 및 공유 프로젝트를 별도 NuGet 프로젝트 형식을 사용 하 여 "bait 및 스위치" NuGet 패키지로 패키지할 수 있습니다. 이 섹션에서는 각 코드 공유 시나리오에 대 한 NuGet 패키지를 만드는 방법을 설명 합니다.

## <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Xamarin 용 NuGet 패키지를 수동으로 만들기](~/cross-platform/app-fundamentals/nuget-manual.md)

Xamarin 플랫폼을 사용 하는 NuGet 패키지를 만들기 위한 팁입니다.
