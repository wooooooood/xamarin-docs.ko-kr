---
title: 코드 공유
description: 응용 프로그램의 핵심 개념
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 01116a35dca80cd92ea16232a2abb127f60d9f0a
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
---
# <a name="sharing-code"></a>코드 공유

이 섹션에서 일반적인 작업 작업 또는 개발자가 모바일 응용 프로그램을 개발할 때 알고 있어야 하는 개념 중 일부는 지침을 제공 합니다.

## <a name="code-sharing-overviewcode-sharingmd"></a>[코드 공유 개요](code-sharing.md)

Xamarin 프로젝트의 경우 이식 가능한 클래스 라이브러리 (PCLs), 공유 프로젝트 및.NET 표준 라이브러리를 포함 하 여 사용할 수 있는 옵션을 공유 하는 다른 코드에 대 한 알아봅니다.


##  <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md)

이식 가능한 클래스 라이브러리 프로젝트를 빌드하고 여러 플랫폼에서 실행 하는 공유 코드를 포함 하는 어셈블리를 배포할 수 있습니다. 이식 가능한 클래스 라이브러리 (또는 "PCL")를 만들려면 먼저 플랫폼을 대상 다음 해당 플랫폼에 대해 정의 된 프로필에서 사용할 수 있는.NET Framework의 하위 집합에 대해 코드를 작성 하려면 선택 합니다. 이 문서에서는 만들고 PCLs Xamarin 함께 사용 하는 방법에 설명 합니다.

##  <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md)

공유 프로젝트의 여러 다른 응용 프로그램 프로젝트에서 참조 되는 일반적인 코드를 작성할 수 있습니다. 코드 참조 하는 각 프로젝트의 일부로 컴파일되고 공유 코드 베이스에서 플랫폼 관련 기능을 추가할 수 있도록 컴파일러 지시문을 포함할 수 있습니다. 이 문서에는 공유 프로젝트의 작동 방식 및 만들고 Xamarin 프로젝트와 함께 사용 하는 방법을 설명 합니다.

##  <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET 표준 플랫폼 간 코드 공유에 대 한 새로운 옵션입니다. 이식 가능한 클래스 라이브러리;에 유사한 방식으로 작동 코드는 특정 버전 (현재 1.0 1.6 ~)에 대해 만들어지고 해당 수준 지 원하는 다른 프로젝트에서 소비할 이상이 될 수 있습니다. 표준.NET 프로젝트는 Mac. Xamarin Studio 6.2, Visual Studio에 대 한 Windows 및 Visual Studio에서 지원

##  <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[코드 공유에 대 한 다중 플랫폼 라이브러리 NuGet 프로젝트의 경우:](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

표준 프로젝트; PCL 또는.NET에서에서 NuGet 패키지를 자동으로 생성 될 수 있습니다. 와 별도 NuGet 프로젝트 형식을 사용 하 여 "밥 and 스위치" NuGet 패키지로 공유 프로젝트를 패키지할 수 있습니다. 이 섹션에서는 각 코드 공유 시나리오에 대 한 NuGet 패키지를 만드는 방법에 설명 합니다.

##  <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Xamarin에 대 한 NuGet 패키지를 수동으로 만들기](~/cross-platform/app-fundamentals/nuget-manual.md)

Xamarin 플랫폼을 사용 하는 NuGet 패키지를 만들기 위한 팁입니다.
